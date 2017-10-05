---
title: "Vytvoření a správa brány Azure Application Gateway – PowerShell | Dokumentace Microsoftu"
description: "Tato stránka obsahuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure Application Gateway pomocí Azure Resource Manageru."
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
ms.openlocfilehash: 5f1713365406764998de505ff62309bab9fa2567
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="127cd-103">Vytvoření, spuštění nebo odstranění služby Application Gateway pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="127cd-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="127cd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="127cd-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="127cd-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="127cd-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="127cd-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="127cd-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="127cd-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="127cd-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="127cd-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="127cd-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="127cd-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="127cd-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="127cd-110">Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="127cd-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="127cd-111">Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="127cd-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="127cd-112">Úplný seznam podporovaných funkcí najdete v tématu [Přehled služby Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-112">To find a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="127cd-113">Tenhle článek vás provede kroky k vytvoření, konfiguraci, spuštění a odstranění aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="127cd-113">This article walks you through the steps to create, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="127cd-114">Než začnete pracovat s prostředky Azure, je důležité uvědomit si, že Azure aktuálně používá dva modely nasazení: Resource Manager a klasický model.</span><span class="sxs-lookup"><span data-stu-id="127cd-114">Before you work with Azure resources, it is important to understand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="127cd-115">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md) rozumíte.</span><span class="sxs-lookup"><span data-stu-id="127cd-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="127cd-116">Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="127cd-116">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="127cd-117">Tento dokument se týká vytvoření služby Application Gateway pomocí Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="127cd-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="127cd-118">Pokud chcete použít klasickou verzi, přejděte na téma [Vytvoření klasického nasazení služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-118">To use the classic version, go to [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="127cd-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="127cd-119">Before you begin</span></span>

1. <span data-ttu-id="127cd-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="127cd-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="127cd-121">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="127cd-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="127cd-122">Pokud máte existující virtuální sítě, vyberte buď existující prázdnou podsíť, nebo vytvořte podsíť v existující virtuální síti výhradně pro účely služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by the application gateway.</span></span> <span data-ttu-id="127cd-123">Nejde nasadit službu Application Gateway do jiné virtuální sítě, než jsou prostředky, které máte v úmyslu nasadit za službou Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-123">You cannot deploy the application gateway to a different virtual network than the resources you intend to deploy behind the application gateway.</span></span>
1. <span data-ttu-id="127cd-124">Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.</span><span class="sxs-lookup"><span data-stu-id="127cd-124">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="127cd-125">Co je potřeba k vytvoření služby Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="127cd-125">What is required to create an application gateway?</span></span>

* <span data-ttu-id="127cd-126">**Fond serverů back-end:** Seznam IP adres, plně kvalifikovaných názvů domén nebo síťových karet serverů back-end.</span><span class="sxs-lookup"><span data-stu-id="127cd-126">**Backend server pool:** The list of IP addresses, FQDNs, or NICs of the backend servers.</span></span> <span data-ttu-id="127cd-127">Pokud jsou používány IP adresy, měly by buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="127cd-127">If IP addresses are used, they should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="127cd-128">**Nastavení fondu serverů back-end:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="127cd-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="127cd-129">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="127cd-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="127cd-130">**Port front-end:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-130">**frontend port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="127cd-131">Když provoz dorazí na tento port, přesměruje se na některý ze serverů back-end.</span><span class="sxs-lookup"><span data-stu-id="127cd-131">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="127cd-132">**Naslouchací proces:** Naslouchací proces má port front-end, protokol (HTTP nebo HTTPS, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="127cd-132">**Listener:** The listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="127cd-133">**Pravidlo:** Pravidlo váže naslouchací proces a fond serverů back-end a definuje, ke kterému fondu serverů back-end se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="127cd-133">**Rule:** The rule binds the listener, the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="127cd-134">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="127cd-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="127cd-135">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="127cd-135">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="127cd-136">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="127cd-137">Přihlaste se k Azure a zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="127cd-137">Log in to Azure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="127cd-138">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="127cd-138">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="127cd-139">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="127cd-139">Choose which of your Azure subscriptions to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="127cd-140">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="127cd-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="127cd-141">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="127cd-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="127cd-142">Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="127cd-142">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="127cd-143">Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="127cd-143">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="127cd-144">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **ContosoRG** a umístěním **Východní USA**.</span><span class="sxs-lookup"><span data-stu-id="127cd-144">In the example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="127cd-145">Když potřebujete nakonfigurovat vlastní test paměti služby Application Gateway, přečtěte si článek [Vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-145">If you need to configure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="127cd-146">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-the-application-gateway-configuration-objects"></a><span data-ttu-id="127cd-147">Vytvoření objektů konfigurace aplikační brány</span><span class="sxs-lookup"><span data-stu-id="127cd-147">Create the application gateway configuration objects</span></span>

<span data-ttu-id="127cd-148">Před vytvořením služby Application Gateway musí být nastaveny všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="127cd-148">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="127cd-149">Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-149">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign the address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with the address space of 10.0.0.0/16 and add the subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used to connect to the application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. The gateway picks up an IP addressfrom the configured subnet and routes network traffic to the IP addresses in the backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with the addresses of your web servers. These backend pool members are all validated to be healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed to them when requests come into the application gateway. Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings to determine the protocol and port that is used when sending traffic to the backend servers. Cookie-based sessions are also determined by the backend HTTP settings.  If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used to connect to the application gateway through the public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure the frontend IP configuration with the public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure the listener.  The listener is a combination of the front end IP configuration, protocol, and port and is used to receive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used to route traffic to the backend servers. The backend pool settings, listener, and backend pool created in the previous steps make up the rule. Based on the criteria defined traffic is routed to the appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU for the application gateway, this determines the size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="127cd-150">Po dokončení načtěte podrobnosti o DNS a virtuální IP adrese služby Application Gateway z prostředku veřejné IP adresy připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-150">When complete, retrieve DNS and VIP details of the application gateway from the public IP resource attached to the application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-the-application-gateway"></a><span data-ttu-id="127cd-151">Odstranění služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="127cd-151">Delete the application gateway</span></span>

<span data-ttu-id="127cd-152">Následující příklad odstraní službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-152">The following example deletes the application gateway.</span></span>

```powershell
# Retrieve the application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops the application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="127cd-153">K potlačení zprávy s potvrzením o odstranění se dá použít přepínač **-force**.</span><span class="sxs-lookup"><span data-stu-id="127cd-153">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="127cd-154">Pokud chcete zkontrolovat, že se služba odstranila, můžete použít rutinu `Get-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="127cd-154">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="127cd-155">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="127cd-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="127cd-156">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="127cd-156">Get application gateway DNS name</span></span>

<span data-ttu-id="127cd-157">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="127cd-157">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="127cd-158">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="127cd-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="127cd-159">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-159">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="127cd-160">Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-160">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="127cd-161">Můžete to provést s využitím DNS Azure nebo dalších poskytovatelů DNS a vytvořením záznamu CNAME, který odkazuje na [veřejnou IP adresu](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="127cd-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points to the [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="127cd-162">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="127cd-162">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="127cd-163">IP adresa je ke službě Application Gateway přiřazena při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="127cd-163">An IP address is assigned to the application gateway when the service starts.</span></span>

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

## <a name="delete-all-resources"></a><span data-ttu-id="127cd-164">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="127cd-164">Delete all resources</span></span>

<span data-ttu-id="127cd-165">Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="127cd-165">To delete all resources created in this article, complete the following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="127cd-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="127cd-166">Next steps</span></span>

<span data-ttu-id="127cd-167">Pokud chcete konfigurovat přesměrování zpracování SSL, přečtěte si článek [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-167">If you want to configure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="127cd-168">Pokud chcete službu Application Gateway nakonfigurovat pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si článek [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="127cd-168">If you want to configure an application gateway to use with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="127cd-169">Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:</span><span class="sxs-lookup"><span data-stu-id="127cd-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="127cd-170">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="127cd-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="127cd-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="127cd-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
