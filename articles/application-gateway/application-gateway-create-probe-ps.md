---
title: "Vytvoření vlastní test paměti - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Naučte se vytvářet vlastní test paměti pro službu Application Gateway pomocí prostředí PowerShell ve službě Správce prostředků"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: b54fe5267d87a41eb9e81d5d1dc9b1b16c5c5e88
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a><span data-ttu-id="c9030-103">Vytvoření vlastní test paměti pro Azure Application Gateway pomocí prostředí PowerShell pro Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c9030-103">Create a custom probe for Azure Application Gateway by using PowerShell for Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9030-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c9030-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="c9030-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9030-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="c9030-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9030-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="c9030-107">V tomto článku přidejte vlastní test paměti existující application gateway pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c9030-107">In this article, you add a custom probe to an existing application gateway with PowerShell.</span></span> <span data-ttu-id="c9030-108">Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na výchozí webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9030-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on the default web application.</span></span>

> [!NOTE]
> <span data-ttu-id="c9030-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c9030-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c9030-110">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c9030-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](application-gateway-create-probe-classic-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a><span data-ttu-id="c9030-111">Vytvoření služby application gateway s vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="c9030-111">Create an application gateway with a custom probe</span></span>

### <a name="sign-in-and-create-resource-group"></a><span data-ttu-id="c9030-112">Přihlaste se a vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="c9030-112">Sign in and create resource group</span></span>

1. <span data-ttu-id="c9030-113">Použití `Login-AzureRmAccount` k ověření.</span><span class="sxs-lookup"><span data-stu-id="c9030-113">Use `Login-AzureRmAccount` to authenticate.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

1. <span data-ttu-id="c9030-114">Získáte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="c9030-114">Get the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

1. <span data-ttu-id="c9030-115">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c9030-115">Choose which of your Azure subscriptions to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. <span data-ttu-id="c9030-116">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9030-116">Create a resource group.</span></span> <span data-ttu-id="c9030-117">Pokud máte existující skupinu prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="c9030-117">You can skip this step if you have an existing resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

<span data-ttu-id="c9030-118">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="c9030-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c9030-119">Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9030-119">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="c9030-120">Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9030-120">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="c9030-121">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** v umístění **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="c9030-121">In the preceding example, we created a resource group called **appgw-RG** in location **West US**.</span></span>

### <a name="create-a-virtual-network-and-a-subnet"></a><span data-ttu-id="c9030-122">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="c9030-122">Create a virtual network and a subnet</span></span>

<span data-ttu-id="c9030-123">Následující příklad vytvoří virtuální síť a podsíť pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="c9030-123">The following example creates a virtual network and a subnet for the application gateway.</span></span> <span data-ttu-id="c9030-124">Aplikační brána vyžaduje vlastní podsíti pro použití.</span><span class="sxs-lookup"><span data-stu-id="c9030-124">Application gateway requires its own subnet for use.</span></span> <span data-ttu-id="c9030-125">Z tohoto důvodu by měla být menší než adresní prostor sítě vnet umožňující jiných podsítí k vytvoření a použití podsíť vytvořená pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="c9030-125">For this reason, the subnet created for the application gateway should be smaller than the address space of the VNET to allow for other subnets to be created and used.</span></span>

```powershell
# Assign the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for the next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="c9030-126">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="c9030-126">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="c9030-127">Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA.</span><span class="sxs-lookup"><span data-stu-id="c9030-127">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span> <span data-ttu-id="c9030-128">Tento příklad používá veřejnou IP adresu pro front-end IP adresu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9030-128">This example uses a public IP address for the front-end IP address of the application gateway.</span></span>  <span data-ttu-id="c9030-129">Aplikační brána vyžaduje veřejnou IP adresu, kterou proto mít dynamicky vytvořený název DNS `-DomainNameLabel` nelze zadat při vytváření veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c9030-129">Application gateway requires the public IP address to have a dynamically created DNS name therefore the `-DomainNameLabel` cannot be specified during the creation of the public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a><span data-ttu-id="c9030-130">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="c9030-130">Create an application gateway</span></span>

<span data-ttu-id="c9030-131">Nastavit všechny položky konfigurace před vytvořením služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="c9030-131">You set up all configuration items before creating the application gateway.</span></span> <span data-ttu-id="c9030-132">Následující příklad vytvoří položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9030-132">The following example creates the configuration items that are needed for an application gateway resource.</span></span>

| <span data-ttu-id="c9030-133">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c9030-133">**Component**</span></span> | <span data-ttu-id="c9030-134">**Popis**</span><span class="sxs-lookup"><span data-stu-id="c9030-134">**Description**</span></span> |
|---|---|
| <span data-ttu-id="c9030-135">**Konfigurace IP brány**</span><span class="sxs-lookup"><span data-stu-id="c9030-135">**Gateway IP configuration**</span></span> | <span data-ttu-id="c9030-136">Konfigurace IP aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9030-136">An IP configuration for an application gateway.</span></span>|
| <span data-ttu-id="c9030-137">**Fond back-end**</span><span class="sxs-lookup"><span data-stu-id="c9030-137">**Backend pool**</span></span> | <span data-ttu-id="c9030-138">Fond IP adres, plně kvalifikovaný název domény společnosti nebo síťové adaptéry, které jsou na aplikační servery, které jsou hostiteli webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c9030-138">A pool of IP addresses, FQDN's, or NICs that are to the application servers that host the web application</span></span>|
| <span data-ttu-id="c9030-139">**Test stavu**</span><span class="sxs-lookup"><span data-stu-id="c9030-139">**Health probe**</span></span> | <span data-ttu-id="c9030-140">Vlastní test paměti používá k monitorování stavu členy fondu back-end</span><span class="sxs-lookup"><span data-stu-id="c9030-140">A custom probe used to monitor the health of the backend pool members</span></span>|
| <span data-ttu-id="c9030-141">**Nastavení protokolu HTTP**</span><span class="sxs-lookup"><span data-stu-id="c9030-141">**HTTP settings**</span></span> | <span data-ttu-id="c9030-142">Kolekce nastavení, včetně, port, protokol, spřažení na základě souborů cookie, test a vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="c9030-142">A collection of settings including, port, protocol, cookie-based affinity, probe, and timeout.</span></span>  <span data-ttu-id="c9030-143">Tato nastavení určují, jak se provoz směruje na členy fondu back-end</span><span class="sxs-lookup"><span data-stu-id="c9030-143">These settings determine how traffic is routed to the backend pool members</span></span>|
| <span data-ttu-id="c9030-144">**Front-endový port**</span><span class="sxs-lookup"><span data-stu-id="c9030-144">**Frontend port**</span></span> | <span data-ttu-id="c9030-145">Port, který službu application gateway naslouchá pro přenosy na</span><span class="sxs-lookup"><span data-stu-id="c9030-145">The port that the application gateway listens for traffic on</span></span>|
| <span data-ttu-id="c9030-146">**Naslouchací proces**</span><span class="sxs-lookup"><span data-stu-id="c9030-146">**Listener**</span></span> | <span data-ttu-id="c9030-147">Kombinace protokolu, front-endové konfigurace protokolu IP a port front-endu.</span><span class="sxs-lookup"><span data-stu-id="c9030-147">A combination of a protocol, frontend IP configuration, and frontend port.</span></span> <span data-ttu-id="c9030-148">Je to, co poslouchá příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="c9030-148">This is what listens for incoming requests.</span></span>
|<span data-ttu-id="c9030-149">**Pravidlo**</span><span class="sxs-lookup"><span data-stu-id="c9030-149">**Rule**</span></span>| <span data-ttu-id="c9030-150">Směrování provozu na příslušné back-end podle nastavení protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c9030-150">Routes the traffic to the appropriate backend based on HTTP settings.</span></span>|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates the backend http settings to be used. This component references the $probe created in the previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for the application gateway to listen on port 80 that will be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates the $publicip variable defined previously with the front-end IP that will be used by the listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates the listener. The listener is a combination of protocol and the frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates the rule that routes traffic to the backend pools.  In this example we create a basic rule that uses the previous defined http settings and backend address pool.  It also associates the listener to the rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets the SKU of the application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# The final step creates the application gateway with all the previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-to-an-existing-application-gateway"></a><span data-ttu-id="c9030-151">Přidat sondu existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="c9030-151">Add a probe to an existing application gateway</span></span>

<span data-ttu-id="c9030-152">Následující fragment kódu přidá sondu existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9030-152">The following code snippet adds a probe to an existing application gateway.</span></span>

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create the probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set the backend HTTP settings to use the new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save the application gateway with the configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a><span data-ttu-id="c9030-153">Odstranit sondu z existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="c9030-153">Remove a probe from an existing application gateway</span></span>

<span data-ttu-id="c9030-154">Následující fragment kódu sondu Odebere existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9030-154">The following code snippet removes a probe from an existing application gateway.</span></span>

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove the probe from the application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set the backend HTTP settings to remove the reference to the probe. The backend http settings now use the default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save the application gateway with the configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="c9030-155">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="c9030-155">Get application gateway DNS name</span></span>

<span data-ttu-id="c9030-156">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="c9030-156">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="c9030-157">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="c9030-157">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="c9030-158">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME sloužící k odkazovaní na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9030-158">To ensure end users can hit the application gateway a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="c9030-159">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c9030-159">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="c9030-160">Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9030-160">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="c9030-161">Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS.</span><span class="sxs-lookup"><span data-stu-id="c9030-161">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="c9030-162">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9030-162">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a><span data-ttu-id="c9030-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9030-163">Next steps</span></span>

<span data-ttu-id="c9030-164">Informace o konfiguraci, navštivte stránky snižování zátěže protokolu SSL: [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="c9030-164">Learn to configure SSL offloading by visiting: [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

