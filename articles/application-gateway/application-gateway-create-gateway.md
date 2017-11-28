---
title: "aaaCreate, spuštění nebo odstranění aplikační brány | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfiguraci, spuštění a odstranění služby Azure application gateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a><span data-ttu-id="4d8d6-103">Vytvoření, spuštění nebo odstranění aplikační brány pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="4d8d6-103">Create, start, or delete an application gateway with PowerShell</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d8d6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d8d6-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="4d8d6-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d8d6-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="4d8d6-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d8d6-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="4d8d6-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4d8d6-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="4d8d6-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4d8d6-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="4d8d6-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="4d8d6-110">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="4d8d6-111">Application Gateway poskytuje mnoho funkcí Application Delivery Controlleru (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních testů stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-111">Application Gateway provides many Application Delivery Controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="4d8d6-112">Navštivte toofind úplný seznam podporovaných funkcích [brány aplikací – přehled](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="4d8d6-112">toofind a complete list of supported features, visit [Application Gateway Overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="4d8d6-113">Tento článek vás provede kroky toocreate hello, konfiguraci, spuštění a odstranění služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4d8d6-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4d8d6-114">Before you begin</span></span>

1. <span data-ttu-id="4d8d6-115">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-115">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="4d8d6-116">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-116">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4d8d6-117">Pokud máte existující virtuální síť, vyberte existující prázdný podsítí nebo vytvořit novou podsíť ve stávající virtuální síti výhradně pro účely hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-117">If you have an existing virtual network, either select an existing empty subnet or create a new subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="4d8d6-118">Nelze nasadit hello aplikace brány tooa v jinou virtuální síť než hello prostředků, který chcete toodeploy za hello Aplikační brána Pokud partnerský vztah virtuální síť se používá.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-118">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway unless vnet peering is used.</span></span> <span data-ttu-id="4d8d6-119">toolearn více navštivte [partnerský vztah virtuální sítě](../virtual-network/virtual-network-peering-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4d8d6-119">toolearn more visit [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)</span></span>
3. <span data-ttu-id="4d8d6-120">Ověřte, že máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-120">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="4d8d6-121">Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-121">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="4d8d6-122">Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-122">hello application gateway must be by itself in a virtual network subnet.</span></span>
4. <span data-ttu-id="4d8d6-123">Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-123">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="4d8d6-124">Co je požadovaná toocreate služby application gateway?</span><span class="sxs-lookup"><span data-stu-id="4d8d6-124">What is required toocreate an application gateway?</span></span>

<span data-ttu-id="4d8d6-125">Při použití hello `New-AzureApplicationGateway` příkaz toocreate hello Aplikační brána v tomto bodě se nenastaví žádná konfigurace a jsou nakonfigurovány hello nově vytvořený prostředek buď pomocí XML nebo objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-125">When you use hello `New-AzureApplicationGateway` command toocreate hello application gateway, no configuration is set at this point and hello newly created resource are configured either by using XML or a configuration object.</span></span>

<span data-ttu-id="4d8d6-126">Hello hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="4d8d6-126">hello values are:</span></span>

* <span data-ttu-id="4d8d6-127">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="4d8d6-128">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="4d8d6-129">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="4d8d6-130">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="4d8d6-131">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="4d8d6-132">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="4d8d6-133">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="4d8d6-134">**Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-134">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="4d8d6-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4d8d6-135">Create an application gateway</span></span>

<span data-ttu-id="4d8d6-136">toocreate aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="4d8d6-136">toocreate an application gateway:</span></span>

1. <span data-ttu-id="4d8d6-137">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-137">Create an application gateway resource.</span></span>
2. <span data-ttu-id="4d8d6-138">Vytvořte konfigurační soubor XML nebo objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-138">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="4d8d6-139">Potvrďte toohello konfigurace hello nově vytvořeného prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-139">Commit hello configuration toohello newly created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="4d8d6-140">Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-140">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md).</span></span> <span data-ttu-id="4d8d6-141">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-141">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

![Příklad scénáře][scenario]

### <a name="create-an-application-gateway-resource"></a><span data-ttu-id="4d8d6-143">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="4d8d6-143">Create an application gateway resource</span></span>

<span data-ttu-id="4d8d6-144">toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-144">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="4d8d6-145">Fakturace hello brány se nespustí v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-145">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="4d8d6-146">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-146">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="4d8d6-147">Hello následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "subnet-1":</span><span class="sxs-lookup"><span data-stu-id="4d8d6-147">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1":</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="4d8d6-148">*Popis*, *InstanceCount* a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-148">*Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span>

<span data-ttu-id="4d8d6-149">byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-149">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> <span data-ttu-id="4d8d6-150">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-150">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="4d8d6-151">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-151">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="4d8d6-152">Můžete vybrat mezi Malá, Střední a Velká.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-152">You can choose between Small, Medium and Large.</span></span>

<span data-ttu-id="4d8d6-153">*Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-153">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="4d8d6-154">Tyto soubory jsou vytvořeny po hello brána je v běžícím stavu hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-154">These are created once hello gateway is in hello running state.</span></span>

## <a name="configure-hello-application-gateway"></a><span data-ttu-id="4d8d6-155">Konfigurace brány aplikace hello</span><span class="sxs-lookup"><span data-stu-id="4d8d6-155">Configure hello application gateway</span></span>

<span data-ttu-id="4d8d6-156">Hello aplikační bránu můžete nakonfigurovat pomocí XML nebo objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-156">You can configure hello application gateway by using XML or a configuration object.</span></span>

### <a name="configure-hello-application-gateway-by-using-xml"></a><span data-ttu-id="4d8d6-157">Konfigurace hello aplikační bránu pomocí XML</span><span class="sxs-lookup"><span data-stu-id="4d8d6-157">Configure hello application gateway by using XML</span></span>

<span data-ttu-id="4d8d6-158">V následujícím příkladu hello použijte tooconfigure souboru XML všech nastavení aplikační brány a potvrdíte je toohello prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-158">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

#### <a name="step-1"></a><span data-ttu-id="4d8d6-159">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4d8d6-159">Step 1</span></span>

<span data-ttu-id="4d8d6-160">Zkopírujte následující text tooNotepad hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-160">Copy hello following text tooNotepad.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="4d8d6-161">Upravte hodnoty hello mezi hello závorkách pro položky konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-161">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="4d8d6-162">Uložte hello soubor s příponou .xml.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-162">Save hello file with extension .xml.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4d8d6-163">Položka Hello protokolu Http nebo Https rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-163">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="4d8d6-164">Hello následující příklad ukazuje, jak toouse konfigurační soubor tooset bránu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-164">hello following example shows how toouse a configuration file tooset up hello application gateway.</span></span> <span data-ttu-id="4d8d6-165">Příklad zatížení Hello vyrovnává provozu HTTP na veřejném portu 80 a odešle síťový provoz tooback-end port 80 mezi dvěma IP adresami.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-165">hello example load balances HTTP traffic on public port 80 and sends network traffic tooback-end port 80 between two IP addresses.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
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
            <Protocol>Http</Protocol>
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

#### <a name="step-2"></a><span data-ttu-id="4d8d6-166">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4d8d6-166">Step 2</span></span>

<span data-ttu-id="4d8d6-167">Dále nastavte aplikační bránu hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-167">Next, set hello application gateway.</span></span> <span data-ttu-id="4d8d6-168">Použití hello `Set-AzureApplicationGatewayConfig` rutiny s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-168">Use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a><span data-ttu-id="4d8d6-169">Konfigurace hello aplikační bránu pomocí objektu konfigurace</span><span class="sxs-lookup"><span data-stu-id="4d8d6-169">Configure hello application gateway by using a configuration object</span></span>

<span data-ttu-id="4d8d6-170">Hello následující příklad ukazuje, jak tooconfigure hello aplikační bránu pomocí objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-170">hello following example shows how tooconfigure hello application gateway by using configuration objects.</span></span> <span data-ttu-id="4d8d6-171">Všechny položky konfigurace musíte musí nakonfigurovat individuálně a potom přidat objektu konfigurace tooan application gateway.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-171">All configuration items must be configured individually and then added tooan application gateway configuration object.</span></span> <span data-ttu-id="4d8d6-172">Po vytvoření objektu konfigurace hello, použijete hello `Set-AzureApplicationGateway` příkaz toocommit hello konfigurace toohello vytvořili prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-172">After creating hello configuration object, you use hello `Set-AzureApplicationGateway` command toocommit hello configuration toohello previously created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="4d8d6-173">Před přiřazením objekt hodnoty tooeach konfigurace, je nutné toodeclare jaký druh objektu používá prostředí PowerShell pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-173">Before assigning a value tooeach configuration object, you need toodeclare what kind of object PowerShell uses for storage.</span></span> <span data-ttu-id="4d8d6-174">Hello první řádek toocreate hello jednotlivých položek definuje, co `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` se používají.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-174">hello first line toocreate hello individual items defines what `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` are used.</span></span>

#### <a name="step-1"></a><span data-ttu-id="4d8d6-175">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4d8d6-175">Step 1</span></span>

<span data-ttu-id="4d8d6-176">Vytvořte všechny položky individuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-176">Create all individual configuration items.</span></span>

<span data-ttu-id="4d8d6-177">Vytvořte front-end IP adresu hello, jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-177">Create hello front-end IP as shown in hello following example.</span></span>

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

<span data-ttu-id="4d8d6-178">Vytvořte hello front-end port, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-178">Create hello front-end port as shown in hello following example.</span></span>

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

<span data-ttu-id="4d8d6-179">Vytvořte fond back-end serverů hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-179">Create hello back-end server pool.</span></span>

<span data-ttu-id="4d8d6-180">Definujte hello IP adresy, které jsou přidány toohello fondu back-end serverů, jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-180">Define hello IP addresses that are added toohello back-end server pool as shown in hello next example.</span></span>

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

<span data-ttu-id="4d8d6-181">Pomocí hello $server objekt tooadd hello hodnoty toohello fond back-end objektu ($pool).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-181">Use hello $server object tooadd hello values toohello back-end pool object ($pool).</span></span>

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

<span data-ttu-id="4d8d6-182">Vytvořte nastavení fondu back-end serverů hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-182">Create hello back-end server pool setting.</span></span>

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

<span data-ttu-id="4d8d6-183">Vytvořte naslouchací proces hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-183">Create hello listener.</span></span>

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

<span data-ttu-id="4d8d6-184">Vytvořte pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-184">Create hello rule.</span></span>

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a><span data-ttu-id="4d8d6-185">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4d8d6-185">Step 2</span></span>

<span data-ttu-id="4d8d6-186">Přiřaďte všechny individuální konfigurace položky tooan objektu konfigurace application gateway ($appgwconfig).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-186">Assign all individual configuration items tooan application gateway configuration object ($appgwconfig).</span></span>

<span data-ttu-id="4d8d6-187">Přidejte toohello konfiguraci front-end IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-187">Add hello front-end IP toohello configuration.</span></span>

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

<span data-ttu-id="4d8d6-188">Přidáte konfiguraci toohello front-end port hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-188">Add hello front-end port toohello configuration.</span></span>

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
<span data-ttu-id="4d8d6-189">Přidáte konfiguraci toohello fondu back-end serverů hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-189">Add hello back-end server pool toohello configuration.</span></span>

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

<span data-ttu-id="4d8d6-190">Přidáte konfiguraci toohello nastavení fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-190">Add hello back-end pool setting toohello configuration.</span></span>

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

<span data-ttu-id="4d8d6-191">Přidáte konfiguraci toohello hello naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-191">Add hello listener toohello configuration.</span></span>

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

<span data-ttu-id="4d8d6-192">Přidáte konfiguraci toohello pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-192">Add hello rule toohello configuration.</span></span>

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a><span data-ttu-id="4d8d6-193">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4d8d6-193">Step 3</span></span>
<span data-ttu-id="4d8d6-194">Potvrdit hello konfigurace objektu toohello prostředku aplikační brány pomocí `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-194">Commit hello configuration object toohello application gateway resource by using `Set-AzureApplicationGatewayConfig`.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a><span data-ttu-id="4d8d6-195">Spusťte bránu hello</span><span class="sxs-lookup"><span data-stu-id="4d8d6-195">Start hello gateway</span></span>

<span data-ttu-id="4d8d6-196">Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-196">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="4d8d6-197">Fakturace aplikační brány se spustí po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-197">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="4d8d6-198">Hello `Start-AzureApplicationGateway` rutiny může trvat až toofinish too15-20 minut.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-198">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="4d8d6-199">Ověření stavu brány hello</span><span class="sxs-lookup"><span data-stu-id="4d8d6-199">Verify hello gateway status</span></span>

<span data-ttu-id="4d8d6-200">Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav hello brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-200">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="4d8d6-201">Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku hello *stavu* by měl být spuštěn, a *Vip* a *DnsName* musí obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-201">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *Vip* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="4d8d6-202">Hello následující příklad ukazuje aplikační bránu, která je aktivní, spuštěná, a je připravený tootake provoz určený pro `http://<generated-dns-name>.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-202">hello following example shows an application gateway that is up, running, and ready tootake traffic destined for `http://<generated-dns-name>.cloudapp.net`.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="4d8d6-203">Odstranění hello application gateway</span><span class="sxs-lookup"><span data-stu-id="4d8d6-203">Delete hello application gateway</span></span>

<span data-ttu-id="4d8d6-204">toodelete hello aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="4d8d6-204">toodelete hello application gateway:</span></span>

1. <span data-ttu-id="4d8d6-205">Použití hello `Stop-AzureApplicationGateway` rutiny toostop hello brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-205">Use hello `Stop-AzureApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="4d8d6-206">Použití hello `Remove-AzureApplicationGateway` rutiny tooremove hello brány.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-206">Use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="4d8d6-207">Ověřte, že hello brány byla odebrána pomocí hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-207">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="4d8d6-208">Hello následující příklad ukazuje hello `Stop-AzureApplicationGateway` na hello prvním řádku, následovanou výstup hello.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-208">hello following example shows hello `Stop-AzureApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="4d8d6-209">Jakmile hello Aplikační brána v zastaveném stavu, použijte hello `Remove-AzureApplicationGateway` rutiny tooremove hello služby.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-209">Once hello application gateway is in a stopped state, use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello service.</span></span>

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

<span data-ttu-id="4d8d6-210">Odebrali jsme tooverify, který hello služby, můžete použít hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-210">tooverify that hello service has been removed, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> <span data-ttu-id="4d8d6-211">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="4d8d6-211">This step is not required.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a><span data-ttu-id="4d8d6-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d8d6-212">Next steps</span></span>

<span data-ttu-id="4d8d6-213">Pokud chcete tooconfigure přesměrování zpracování SSL, najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-213">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="4d8d6-214">Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="4d8d6-214">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="4d8d6-215">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="4d8d6-215">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="4d8d6-216">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="4d8d6-216">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="4d8d6-217">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="4d8d6-217">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
