---
title: "Konfigurace protokolu SSL snižování zátěže - Azure Application Gateway - PowerShell classic | Microsoft Docs"
description: "Tento článek poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí modelu nasazení Azure classic."
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
ms.openlocfilehash: 2eba6fb24c11add12ac16d04d3445e19a3486216
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a><span data-ttu-id="85a37-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="85a37-103">Configure an application gateway for SSL offload by using the classic deployment model</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="85a37-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85a37-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="85a37-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="85a37-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="85a37-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="85a37-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="85a37-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="85a37-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="85a37-108">Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="85a37-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="85a37-109">Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="85a37-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="85a37-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="85a37-110">Before you begin</span></span>

1. <span data-ttu-id="85a37-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="85a37-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="85a37-112">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="85a37-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="85a37-113">Ověřte, že máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="85a37-113">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="85a37-114">Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť.</span><span class="sxs-lookup"><span data-stu-id="85a37-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="85a37-115">Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="85a37-115">The application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="85a37-116">Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.</span><span class="sxs-lookup"><span data-stu-id="85a37-116">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="85a37-117">Pokud chcete konfigurovat přesměrování zpracování SSL na aplikační brány, proveďte následující kroky v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="85a37-117">To configure SSL offload on an application gateway, do the following steps in the order listed:</span></span>

1. [<span data-ttu-id="85a37-118">Vytvoření služby application gateway</span><span class="sxs-lookup"><span data-stu-id="85a37-118">Create an application gateway</span></span>](#create-an-application-gateway)
2. [<span data-ttu-id="85a37-119">Nahrát certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="85a37-119">Upload SSL certificates</span></span>](#upload-ssl-certificates)
3. [<span data-ttu-id="85a37-120">Konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="85a37-120">Configure the gateway</span></span>](#configure-the-gateway)
4. [<span data-ttu-id="85a37-121">Nastavení konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="85a37-121">Set the gateway configuration</span></span>](#set-the-gateway-configuration)
5. [<span data-ttu-id="85a37-122">Spusťte bránu</span><span class="sxs-lookup"><span data-stu-id="85a37-122">Start the gateway</span></span>](#start-the-gateway)
6. [<span data-ttu-id="85a37-123">Ověřte stav brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-123">Verify the gateway status</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="85a37-124">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="85a37-124">Create an application gateway</span></span>

<span data-ttu-id="85a37-125">Pokud chcete vytvořit bránu, použijte rutinu `New-AzureApplicationGateway` a zadejte vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="85a37-125">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="85a37-126">Fakturace brány se nespustí v tomhle okamžiku.</span><span class="sxs-lookup"><span data-stu-id="85a37-126">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="85a37-127">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-127">Billing begins in a later step, when the gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="85a37-128">Pokud chcete ověřit vytvoření brány, můžete použít rutinu `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="85a37-128">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="85a37-129">V ukázce *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="85a37-129">In the sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="85a37-130">Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="85a37-130">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="85a37-131">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="85a37-131">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="85a37-132">Malých a velkých jsou ostatní dostupné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="85a37-132">Small and Large are other available values.</span></span> <span data-ttu-id="85a37-133">Hodnoty *VirtualIPs* a *DnsName* se zobrazují jako prázdné, protože se brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="85a37-133">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="85a37-134">Tyto hodnoty se vytvoří, jakmile je brána v běžícím stavu.</span><span class="sxs-lookup"><span data-stu-id="85a37-134">These values are created once the gateway is in the running state.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a><span data-ttu-id="85a37-135">Nahrát certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="85a37-135">Upload SSL certificates</span></span>

<span data-ttu-id="85a37-136">Použití `Add-AzureApplicationGatewaySslCertificate` nahrát certifikát serveru ve *pfx* formátu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-136">Use `Add-AzureApplicationGatewaySslCertificate` to upload the server certificate in *pfx* format to the application gateway.</span></span> <span data-ttu-id="85a37-137">Název certifikátu je název vybrané uživatelem a musí být jedinečný v rámci služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="85a37-137">The certificate name is a user-chosen name and must be unique within the application gateway.</span></span> <span data-ttu-id="85a37-138">Tento certifikát se označuje s tímto názvem v všechny operace správy certifikátů ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="85a37-138">This certificate is referred to by this name in all certificate management operations on the application gateway.</span></span>

<span data-ttu-id="85a37-139">Tento následující příklad ukazuje rutinu, nahraďte hodnotami v ukázce vlastní.</span><span class="sxs-lookup"><span data-stu-id="85a37-139">This following sample shows the cmdlet, replace the values in the sample with your own.</span></span>

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

<span data-ttu-id="85a37-140">Dále ověřte nahrávání certifikátu.</span><span class="sxs-lookup"><span data-stu-id="85a37-140">Next, validate the certificate upload.</span></span> <span data-ttu-id="85a37-141">Použití `Get-AzureApplicationGatewayCertificate` rutiny.</span><span class="sxs-lookup"><span data-stu-id="85a37-141">Use the `Get-AzureApplicationGatewayCertificate` cmdlet.</span></span>

<span data-ttu-id="85a37-142">Tento příklad ukazuje rutinu na prvním řádku, následovanou výstupem.</span><span class="sxs-lookup"><span data-stu-id="85a37-142">This sample shows the cmdlet on the first line, followed by the output.</span></span>

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
> <span data-ttu-id="85a37-143">Heslo certifikátu musí být mezi 4 až 12 znaků, písmen nebo číslic.</span><span class="sxs-lookup"><span data-stu-id="85a37-143">The certificate password has to be between 4 to 12 characters, letters, or numbers.</span></span> <span data-ttu-id="85a37-144">Speciální znaky nejsou přijata.</span><span class="sxs-lookup"><span data-stu-id="85a37-144">Special characters are not accepted.</span></span>

## <a name="configure-the-gateway"></a><span data-ttu-id="85a37-145">Konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="85a37-145">Configure the gateway</span></span>

<span data-ttu-id="85a37-146">Konfigurace brány aplikace se skládá z více hodnot.</span><span class="sxs-lookup"><span data-stu-id="85a37-146">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="85a37-147">Může být vázáno hodnoty společně k vytvoření konfigurace.</span><span class="sxs-lookup"><span data-stu-id="85a37-147">The values can be tied together to construct the configuration.</span></span>

<span data-ttu-id="85a37-148">Hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="85a37-148">The values are:</span></span>

* <span data-ttu-id="85a37-149">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="85a37-149">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="85a37-150">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="85a37-150">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="85a37-151">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="85a37-151">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="85a37-152">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="85a37-152">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="85a37-153">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="85a37-153">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="85a37-154">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="85a37-154">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="85a37-155">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="85a37-155">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="85a37-156">**Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="85a37-156">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="85a37-157">V tuhle chvíli se podporuje jenom *základní* pravidlo.</span><span class="sxs-lookup"><span data-stu-id="85a37-157">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="85a37-158">*Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="85a37-158">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="85a37-159">**Další poznámky ke konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="85a37-159">**Additional configuration notes**</span></span>

<span data-ttu-id="85a37-160">Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen).</span><span class="sxs-lookup"><span data-stu-id="85a37-160">For SSL certificates configuration, the protocol in **HttpListener** should change to *Https* (case sensitive).</span></span> <span data-ttu-id="85a37-161">**SslCert** prvek přidán na **HttpListener** hodnota nastavena na stejný název jako použít v odesílání předcházející SSL certifikáty části.</span><span class="sxs-lookup"><span data-stu-id="85a37-161">The **SslCert** element is added to **HttpListener** with the value set to the same name as used in the upload of preceding SSL certificates section.</span></span> <span data-ttu-id="85a37-162">Front-end port se musí aktualizovat na hodnotu 443.</span><span class="sxs-lookup"><span data-stu-id="85a37-162">The front-end port should be updated to 443.</span></span>

<span data-ttu-id="85a37-163">**Když chcete povolit spřažení na základě souborů cookie**: aplikační brána se může nakonfigurovat tak, aby se žádost od klientské relace vždy směrovala na stejný virtuální počítač v prostředí webové serverové farmy.</span><span class="sxs-lookup"><span data-stu-id="85a37-163">**To enable cookie-based affinity**: An application gateway can be configured to ensure that a request from a client session is always directed to the same VM in the web farm.</span></span> <span data-ttu-id="85a37-164">Takový scénář se provádí injektáží souboru cookie relace, který bráně umožňuje řídit provoz odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="85a37-164">This scenario is done by injection of a session cookie that allows the gateway to direct traffic appropriately.</span></span> <span data-ttu-id="85a37-165">Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.</span><span class="sxs-lookup"><span data-stu-id="85a37-165">To enable cookie-based affinity, set **CookieBasedAffinity** to *Enabled* in the **BackendHttpSettings** element.</span></span>

<span data-ttu-id="85a37-166">Konfiguraci můžete vytvořit buď pomocí vytvoření objektu konfigurace, nebo pomocí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="85a37-166">You can construct your configuration either by creating a configuration object or by using a configuration XML file.</span></span>
<span data-ttu-id="85a37-167">Chcete-li vytvořit konfiguraci pomocí konfiguračního souboru XML, použijte následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="85a37-167">To construct your configuration by using a configuration XML file, use the following sample:</span></span>

<span data-ttu-id="85a37-168">**Ukázkový kód XML konfigurace**</span><span class="sxs-lookup"><span data-stu-id="85a37-168">**Configuration XML sample**</span></span>

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

## <a name="set-the-gateway-configuration"></a><span data-ttu-id="85a37-169">Nastavení konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="85a37-169">Set the gateway configuration</span></span>

<span data-ttu-id="85a37-170">Dále nastavte aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="85a37-170">Next, you set the application gateway.</span></span> <span data-ttu-id="85a37-171">Můžete použít `Set-AzureApplicationGatewayConfig` rutiny buď objekt konfigurace nebo s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="85a37-171">You can use the `Set-AzureApplicationGatewayConfig` cmdlet with either a configuration object or with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a><span data-ttu-id="85a37-172">Spusťte bránu</span><span class="sxs-lookup"><span data-stu-id="85a37-172">Start the gateway</span></span>

<span data-ttu-id="85a37-173">Jakmile se brána nakonfiguruje, pomocí rutiny `Start-AzureApplicationGateway` ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="85a37-173">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="85a37-174">Fakturace aplikační brány se spustí až po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-174">Billing for an application gateway begins after the gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="85a37-175">Dokončení rutiny `Start-AzureApplicationGateway` může trvat 15 až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="85a37-175">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to finish.</span></span>
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a><span data-ttu-id="85a37-176">Ověřte stav brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-176">Verify the gateway status</span></span>

<span data-ttu-id="85a37-177">Pomocí rutiny `Get-AzureApplicationGateway` zkontrolujte stav brány.</span><span class="sxs-lookup"><span data-stu-id="85a37-177">Use the `Get-AzureApplicationGateway` cmdlet to check the status of the gateway.</span></span> <span data-ttu-id="85a37-178">Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku, *stavu* by měl být spuštěn, a *rezervovaná* a *DnsName* musí obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="85a37-178">If `Start-AzureApplicationGateway` succeeded in the previous step, *State* should be Running, and *VirtualIPs* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="85a37-179">Tento příklad ukazuje aplikační bránu, která je v provozu, spuštěna a je připraven přijmout provoz.</span><span class="sxs-lookup"><span data-stu-id="85a37-179">This sample shows an application gateway that is up, running, and is ready to take traffic.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="85a37-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85a37-180">Next steps</span></span>

<span data-ttu-id="85a37-181">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="85a37-181">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="85a37-182">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="85a37-182">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="85a37-183">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="85a37-183">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

