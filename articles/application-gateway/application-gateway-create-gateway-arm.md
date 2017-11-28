---
title: "aaaCreate a spravovat služby Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfiguraci, spuštění a odstranění služby Azure application gateway pomocí Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: ab98d5f9aa0dc309f8353b7f72591359e1121849
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="f0b3c-103">Vytvoření, spuštění nebo odstranění služby Application Gateway pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f0b3c-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0b3c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0b3c-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="f0b3c-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0b3c-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="f0b3c-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0b3c-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="f0b3c-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f0b3c-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="f0b3c-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f0b3c-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="f0b3c-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="f0b3c-110">Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="f0b3c-111">Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="f0b3c-112">Navštivte toofind úplný seznam podporovaných funkcích [Application Gateway přehled](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="f0b3c-113">Tento článek vás provede kroky toocreate hello, konfiguraci, spuštění a odstranění služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0b3c-114">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-114">Before you work with Azure resources, it is important toounderstand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="f0b3c-115">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md) rozumíte.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="f0b3c-116">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-116">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="f0b3c-117">Tento dokument se týká vytvoření služby Application Gateway pomocí Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="f0b3c-118">toouse hello klasickou verzi, přejděte příliš[vytvoření klasického nasazení brány aplikace pomocí prostředí PowerShell](application-gateway-create-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-118">toouse hello classic version, go too[Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f0b3c-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f0b3c-119">Before you begin</span></span>

1. <span data-ttu-id="f0b3c-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="f0b3c-121">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="f0b3c-122">Pokud máte existující virtuální síť, vyberte existující prázdný podsítí nebo vytvořit podsíť ve stávající virtuální síti výhradně pro účely hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="f0b3c-123">Nelze nasadit hello aplikace brány tooa v jinou virtuální síť než hello prostředků, který chcete toodeploy za hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-123">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway.</span></span>
1. <span data-ttu-id="f0b3c-124">Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-124">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="f0b3c-125">Co je požadovaná toocreate služby application gateway?</span><span class="sxs-lookup"><span data-stu-id="f0b3c-125">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="f0b3c-126">**Fond back-end serverů:** hello seznam IP adres, plně kvalifikovaných názvů domény nebo síťové adaptéry hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-126">**Backend server pool:** hello list of IP addresses, FQDNs, or NICs of hello backend servers.</span></span> <span data-ttu-id="f0b3c-127">Pokud se používají IP adresy, by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-127">If IP addresses are used, they should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="f0b3c-128">**Nastavení fondu serverů back-end:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="f0b3c-129">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="f0b3c-130">**front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-130">**frontend port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="f0b3c-131">Provoz volá Tenhle port a pak získá přesměrovaného tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-131">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="f0b3c-132">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-132">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="f0b3c-133">**Pravidlo:** hello pravidlo váže naslouchací proces hello, fondu hello back-end serverů a definuje, jaký back-end serveru fondu hello provoz by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-133">**Rule:** hello rule binds hello listener, hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="f0b3c-134">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f0b3c-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="f0b3c-135">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-135">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="f0b3c-136">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="f0b3c-137">Přihlaste se tooAzure a zadejte svá pověření.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-137">Log in tooAzure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="f0b3c-138">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-138">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="f0b3c-139">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-139">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="f0b3c-140">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="f0b3c-141">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="f0b3c-142">Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-142">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="f0b3c-143">Ujistěte se, že všechny příkazy toocreate používá služby application gateway hello stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-143">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="f0b3c-144">V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **ContosoRG** a umístění **východní USA**.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-144">In hello example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="f0b3c-145">Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, navštivte: [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-145">If you need tooconfigure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="f0b3c-146">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-hello-application-gateway-configuration-objects"></a><span data-ttu-id="f0b3c-147">Vytvoření objektů konfigurace hello application gateway</span><span class="sxs-lookup"><span data-stu-id="f0b3c-147">Create hello application gateway configuration objects</span></span>

<span data-ttu-id="f0b3c-148">Všechny položky konfigurace, musí ho nastavit před vytvořením hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-148">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="f0b3c-149">Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-149">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign hello address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with hello address space of 10.0.0.0/16 and add hello subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve hello newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used tooconnect toohello application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. hello gateway picks up an IP addressfrom hello configured subnet and routes network traffic toohello IP addresses in hello backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with hello addresses of your web servers. These backend pool members are all validated toobe healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed toothem when requests come into hello application gateway. Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings toodetermine hello protocol and port that is used when sending traffic toohello backend servers. Cookie-based sessions are also determined by hello backend HTTP settings.  If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used tooconnect toohello application gateway through hello public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure hello frontend IP configuration with hello public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure hello listener.  hello listener is a combination of hello front end IP configuration, protocol, and port and is used tooreceive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used tooroute traffic toohello backend servers. hello backend pool settings, listener, and backend pool created in hello previous steps make up hello rule. Based on hello criteria defined traffic is routed toohello appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU for hello application gateway, this determines hello size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create hello application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="f0b3c-150">Po dokončení načíst DNS a VIP podrobnosti o hello aplikační brány z hello veřejné IP prostředků připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-150">When complete, retrieve DNS and VIP details of hello application gateway from hello public IP resource attached toohello application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="f0b3c-151">Odstranění hello application gateway</span><span class="sxs-lookup"><span data-stu-id="f0b3c-151">Delete hello application gateway</span></span>

<span data-ttu-id="f0b3c-152">Hello následující příklad odstraní hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-152">hello following example deletes hello application gateway.</span></span>

```powershell
# Retrieve hello application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops hello application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="f0b3c-153">Hello **-force** přepínač může být použité toosuppress hello odebrat potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-153">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="f0b3c-154">Odebrali jsme tooverify, který hello služby, můžete použít hello `Get-AzureRmApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-154">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="f0b3c-155">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="f0b3c-156">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="f0b3c-156">Get application gateway DNS name</span></span>

<span data-ttu-id="f0b3c-157">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-157">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="f0b3c-158">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="f0b3c-159">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-159">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="f0b3c-160">toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="f0b3c-161">To lze provést pomocí Azure DNS nebo jiní poskytovatelé DNS pomocí vytvoření záznamu CNAME, který ukazuje toohello [veřejnou IP adresu](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points toohello [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="f0b3c-162">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="f0b3c-163">IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.</span><span class="sxs-lookup"><span data-stu-id="f0b3c-163">An IP address is assigned toohello application gateway when hello service starts.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="delete-all-resources"></a><span data-ttu-id="f0b3c-164">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="f0b3c-164">Delete all resources</span></span>

<span data-ttu-id="f0b3c-165">toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující krok:</span><span class="sxs-lookup"><span data-stu-id="f0b3c-165">toodelete all resources created in this article, complete hello following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="f0b3c-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0b3c-166">Next steps</span></span>

<span data-ttu-id="f0b3c-167">Pokud chcete, aby tooconfigure přesměrování zpracování SSL, navštivte: [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-167">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="f0b3c-168">Pokud chcete, aby tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, navštivte: [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="f0b3c-168">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="f0b3c-169">Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:</span><span class="sxs-lookup"><span data-stu-id="f0b3c-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="f0b3c-170">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="f0b3c-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="f0b3c-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="f0b3c-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
