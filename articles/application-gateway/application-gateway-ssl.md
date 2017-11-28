---
title: "aaaConfigure SSL snižování zátěže prostředí PowerShell – Azure Application Gateway - classic | Microsoft Docs"
description: "Tento článek obsahuje pokyny hello toocreate přesměrování zpracování úloh služby application gateway pomocí protokolu SSL pomocí modelu nasazení Azure classic."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 5cb128015747ed4b71802cf751c80b60634601a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a><span data-ttu-id="78974-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="78974-103">Configure an application gateway for SSL offload by using hello classic deployment model</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="78974-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78974-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="78974-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="78974-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="78974-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="78974-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="78974-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="78974-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="78974-108">Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy.</span><span class="sxs-lookup"><span data-stu-id="78974-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="78974-109">Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="78974-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="78974-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="78974-110">Before you begin</span></span>

1. <span data-ttu-id="78974-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="78974-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="78974-112">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="78974-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="78974-113">Ověřte, že máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="78974-113">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="78974-114">Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="78974-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="78974-115">Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="78974-115">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="78974-116">Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.</span><span class="sxs-lookup"><span data-stu-id="78974-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="78974-117">tooconfigure SSL přesměrování zpracování úloh na aplikační brány, text hello, proveďte kroky v uvedeném pořadí hello:</span><span class="sxs-lookup"><span data-stu-id="78974-117">tooconfigure SSL offload on an application gateway, do hello following steps in hello order listed:</span></span>

1. [<span data-ttu-id="78974-118">Vytvoření služby application gateway</span><span class="sxs-lookup"><span data-stu-id="78974-118">Create an application gateway</span></span>](#create-an-application-gateway)
2. [<span data-ttu-id="78974-119">Nahrát certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="78974-119">Upload SSL certificates</span></span>](#upload-ssl-certificates)
3. [<span data-ttu-id="78974-120">Konfigurace brány hello</span><span class="sxs-lookup"><span data-stu-id="78974-120">Configure hello gateway</span></span>](#configure-the-gateway)
4. [<span data-ttu-id="78974-121">Konfigurace brány hello sady</span><span class="sxs-lookup"><span data-stu-id="78974-121">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
5. [<span data-ttu-id="78974-122">Spusťte bránu hello</span><span class="sxs-lookup"><span data-stu-id="78974-122">Start hello gateway</span></span>](#start-the-gateway)
6. [<span data-ttu-id="78974-123">Ověření stavu brány hello</span><span class="sxs-lookup"><span data-stu-id="78974-123">Verify hello gateway status</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="78974-124">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="78974-124">Create an application gateway</span></span>

<span data-ttu-id="78974-125">toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="78974-125">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="78974-126">Fakturace hello brány se nespustí v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="78974-126">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="78974-127">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="78974-127">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="78974-128">byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="78974-128">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="78974-129">V ukázce hello *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="78974-129">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="78974-130">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="78974-130">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="78974-131">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="78974-131">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="78974-132">Malých a velkých jsou ostatní dostupné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="78974-132">Small and Large are other available values.</span></span> <span data-ttu-id="78974-133">*Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="78974-133">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="78974-134">Tyto hodnoty se vytvoří, jakmile hello brána v běžícím stavu hello.</span><span class="sxs-lookup"><span data-stu-id="78974-134">These values are created once hello gateway is in hello running state.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a><span data-ttu-id="78974-135">Nahrát certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="78974-135">Upload SSL certificates</span></span>

<span data-ttu-id="78974-136">Použití `Add-AzureApplicationGatewaySslCertificate` tooupload hello certifikátu serveru v *pfx* formát toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="78974-136">Use `Add-AzureApplicationGatewaySslCertificate` tooupload hello server certificate in *pfx* format toohello application gateway.</span></span> <span data-ttu-id="78974-137">název certifikátu Hello je název vybrané uživatelem a musí být jedinečný v rámci hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="78974-137">hello certificate name is a user-chosen name and must be unique within hello application gateway.</span></span> <span data-ttu-id="78974-138">Tento certifikát je tooby označují název v všechny operace správy certifikátů ve hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="78974-138">This certificate is referred tooby this name in all certificate management operations on hello application gateway.</span></span>

<span data-ttu-id="78974-139">Tento následující příklad ukazuje rutinu hello nahraďte hello hodnoty v ukázce hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="78974-139">This following sample shows hello cmdlet, replace hello values in hello sample with your own.</span></span>

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

<span data-ttu-id="78974-140">Dále ověřte nahrávání certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="78974-140">Next, validate hello certificate upload.</span></span> <span data-ttu-id="78974-141">Použití hello `Get-AzureApplicationGatewayCertificate` rutiny.</span><span class="sxs-lookup"><span data-stu-id="78974-141">Use hello `Get-AzureApplicationGatewayCertificate` cmdlet.</span></span>

<span data-ttu-id="78974-142">Tento příklad ukazuje rutinu hello na prvním řádku hello, následuje výstup hello.</span><span class="sxs-lookup"><span data-stu-id="78974-142">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> <span data-ttu-id="78974-143">heslo certifikátu Hello má toobe mezi 4 too12 znaky, písmena nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="78974-143">hello certificate password has toobe between 4 too12 characters, letters, or numbers.</span></span> <span data-ttu-id="78974-144">Speciální znaky nejsou přijata.</span><span class="sxs-lookup"><span data-stu-id="78974-144">Special characters are not accepted.</span></span>

## <a name="configure-hello-gateway"></a><span data-ttu-id="78974-145">Konfigurace brány hello</span><span class="sxs-lookup"><span data-stu-id="78974-145">Configure hello gateway</span></span>

<span data-ttu-id="78974-146">Konfigurace brány aplikace se skládá z více hodnot.</span><span class="sxs-lookup"><span data-stu-id="78974-146">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="78974-147">může být vázáno Hello hodnoty společně tooconstruct hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="78974-147">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="78974-148">Hello hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="78974-148">hello values are:</span></span>

* <span data-ttu-id="78974-149">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="78974-149">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="78974-150">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="78974-150">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="78974-151">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="78974-151">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="78974-152">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="78974-152">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="78974-153">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="78974-153">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="78974-154">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="78974-154">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="78974-155">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="78974-155">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="78974-156">**Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="78974-156">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="78974-157">V současné době pouze hello *základní* pravidel je podporována.</span><span class="sxs-lookup"><span data-stu-id="78974-157">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="78974-158">Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="78974-158">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="78974-159">**Další poznámky ke konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="78974-159">**Additional configuration notes**</span></span>

<span data-ttu-id="78974-160">Pro konfiguraci certifikátů SSL, hello protokol v **HttpListener** by se měl změnit příliš*Https* (malá a velká písmena).</span><span class="sxs-lookup"><span data-stu-id="78974-160">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="78974-161">Hello **SslCert** prvek přidán příliš**HttpListener** s hello nastavena hodnota toohello stejný název jako použít v odesílání hello předcházející části certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="78974-161">hello **SslCert** element is added too**HttpListener** with hello value set toohello same name as used in hello upload of preceding SSL certificates section.</span></span> <span data-ttu-id="78974-162">Hello front-end port musí být aktualizované too443.</span><span class="sxs-lookup"><span data-stu-id="78974-162">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="78974-163">**spřažení na základě souborů cookie tooenable**: služby application gateway může být nakonfigurované tooensure, zda je žádost o od klientské relace vždy směrovanou toohello stejný virtuální počítač v hello webové farmy.</span><span class="sxs-lookup"><span data-stu-id="78974-163">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="78974-164">Tento scénář se provádí injektáží souboru cookie relace, které umožňuje přenos toodirect hello brány správně.</span><span class="sxs-lookup"><span data-stu-id="78974-164">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="78974-165">Nastavení spřažení na základě souborů cookie tooenable **CookieBasedAffinity** příliš*povoleno* v hello **BackendHttpSettings** element.</span><span class="sxs-lookup"><span data-stu-id="78974-165">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

<span data-ttu-id="78974-166">Konfiguraci můžete vytvořit buď pomocí vytvoření objektu konfigurace, nebo pomocí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="78974-166">You can construct your configuration either by creating a configuration object or by using a configuration XML file.</span></span>
<span data-ttu-id="78974-167">použít konfiguraci pomocí konfigurační soubor XML, tooconstruct hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="78974-167">tooconstruct your configuration by using a configuration XML file, use hello following sample:</span></span>

<span data-ttu-id="78974-168">**Ukázkový kód XML konfigurace**</span><span class="sxs-lookup"><span data-stu-id="78974-168">**Configuration XML sample**</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="78974-169">Konfigurace brány hello sady</span><span class="sxs-lookup"><span data-stu-id="78974-169">Set hello gateway configuration</span></span>

<span data-ttu-id="78974-170">Dále nastavte aplikační bránu hello.</span><span class="sxs-lookup"><span data-stu-id="78974-170">Next, you set hello application gateway.</span></span> <span data-ttu-id="78974-171">Můžete použít hello `Set-AzureApplicationGatewayConfig` rutiny buď objekt konfigurace nebo s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="78974-171">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with either a configuration object or with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a><span data-ttu-id="78974-172">Spusťte bránu hello</span><span class="sxs-lookup"><span data-stu-id="78974-172">Start hello gateway</span></span>

<span data-ttu-id="78974-173">Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány.</span><span class="sxs-lookup"><span data-stu-id="78974-173">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="78974-174">Fakturace aplikační brány se spustí po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="78974-174">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="78974-175">Hello `Start-AzureApplicationGateway` rutiny může trvat až toofinish too15-20 minut.</span><span class="sxs-lookup"><span data-stu-id="78974-175">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="78974-176">Ověření stavu brány hello</span><span class="sxs-lookup"><span data-stu-id="78974-176">Verify hello gateway status</span></span>

<span data-ttu-id="78974-177">Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav hello brány.</span><span class="sxs-lookup"><span data-stu-id="78974-177">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="78974-178">Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku hello *stavu* by měl být spuštěn, a *rezervovaná* a *DnsName* musí obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="78974-178">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *VirtualIPs* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="78974-179">Tento příklad ukazuje aplikační bránu, která je, spuštěna a je připravený tootake provoz.</span><span class="sxs-lookup"><span data-stu-id="78974-179">This sample shows an application gateway that is up, running, and is ready tootake traffic.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="78974-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78974-180">Next steps</span></span>

<span data-ttu-id="78974-181">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="78974-181">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="78974-182">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="78974-182">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="78974-183">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="78974-183">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

