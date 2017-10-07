---
title: "PowerShell – Azure Application Gateway - testu aaaCreate vlastní | Microsoft Docs"
description: "Zjistěte, jak toocreate vlastní sběru dat pro službu Application Gateway pomocí prostředí PowerShell ve službě Správce prostředků"
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
ms.openlocfilehash: 44c9ffa75401d6d0db023e66fa82c701fb0cf8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a><span data-ttu-id="b43bc-103">Vytvoření vlastní test paměti pro Azure Application Gateway pomocí prostředí PowerShell pro Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b43bc-103">Create a custom probe for Azure Application Gateway by using PowerShell for Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b43bc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b43bc-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="b43bc-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="b43bc-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="b43bc-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="b43bc-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="b43bc-107">V tomto článku můžete přidat vlastní test paměti tooan existující aplikace bránu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b43bc-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="b43bc-108">Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na hello výchozí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b43bc-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!NOTE]
> <span data-ttu-id="b43bc-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b43bc-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="b43bc-110">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b43bc-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](application-gateway-create-probe-classic-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a><span data-ttu-id="b43bc-111">Vytvoření služby application gateway s vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="b43bc-111">Create an application gateway with a custom probe</span></span>

### <a name="sign-in-and-create-resource-group"></a><span data-ttu-id="b43bc-112">Přihlaste se a vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="b43bc-112">Sign in and create resource group</span></span>

1. <span data-ttu-id="b43bc-113">Použití `Login-AzureRmAccount` tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="b43bc-113">Use `Login-AzureRmAccount` tooauthenticate.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

1. <span data-ttu-id="b43bc-114">Získáte hello předplatná pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="b43bc-114">Get hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

1. <span data-ttu-id="b43bc-115">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="b43bc-115">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. <span data-ttu-id="b43bc-116">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b43bc-116">Create a resource group.</span></span> <span data-ttu-id="b43bc-117">Pokud máte existující skupinu prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="b43bc-117">You can skip this step if you have an existing resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

<span data-ttu-id="b43bc-118">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="b43bc-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="b43bc-119">Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b43bc-119">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="b43bc-120">Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b43bc-120">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="b43bc-121">V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **appgw-RG** v umístění **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="b43bc-121">In hello preceding example, we created a resource group called **appgw-RG** in location **West US**.</span></span>

### <a name="create-a-virtual-network-and-a-subnet"></a><span data-ttu-id="b43bc-122">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="b43bc-122">Create a virtual network and a subnet</span></span>

<span data-ttu-id="b43bc-123">Hello následující příklad vytvoří virtuální síť a podsíť pro aplikační bránu hello.</span><span class="sxs-lookup"><span data-stu-id="b43bc-123">hello following example creates a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="b43bc-124">Aplikační brána vyžaduje vlastní podsíti pro použití.</span><span class="sxs-lookup"><span data-stu-id="b43bc-124">Application gateway requires its own subnet for use.</span></span> <span data-ttu-id="b43bc-125">Z tohoto důvodu by měla být menší než hello adresní prostor virtuální sítě tooallow hello pro další podsítě toobe vytvořit a použít hello podsíť vytvořená hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-125">For this reason, hello subnet created for hello application gateway should be smaller than hello address space of hello VNET tooallow for other subnets toobe created and used.</span></span>

```powershell
# Assign hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for hello next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="b43bc-126">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="b43bc-126">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="b43bc-127">Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="b43bc-127">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="b43bc-128">Tento příklad používá veřejnou IP adresu pro hello front-end IP adresu hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-128">This example uses a public IP address for hello front-end IP address of hello application gateway.</span></span>  <span data-ttu-id="b43bc-129">Aplikační brána vyžaduje hello veřejnou IP adresu toohave dynamicky vytvořený název DNS proto hello `-DomainNameLabel` nelze zadat během vytváření hello hello veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b43bc-129">Application gateway requires hello public IP address toohave a dynamically created DNS name therefore hello `-DomainNameLabel` cannot be specified during hello creation of hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a><span data-ttu-id="b43bc-130">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b43bc-130">Create an application gateway</span></span>

<span data-ttu-id="b43bc-131">Nastavit všechny položky konfigurace před vytvořením hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-131">You set up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="b43bc-132">Hello následující příklad vytvoří hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-132">hello following example creates hello configuration items that are needed for an application gateway resource.</span></span>

| <span data-ttu-id="b43bc-133">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="b43bc-133">**Component**</span></span> | <span data-ttu-id="b43bc-134">**Popis**</span><span class="sxs-lookup"><span data-stu-id="b43bc-134">**Description**</span></span> |
|---|---|
| <span data-ttu-id="b43bc-135">**Konfigurace IP brány**</span><span class="sxs-lookup"><span data-stu-id="b43bc-135">**Gateway IP configuration**</span></span> | <span data-ttu-id="b43bc-136">Konfigurace IP aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-136">An IP configuration for an application gateway.</span></span>|
| <span data-ttu-id="b43bc-137">**Fond back-end**</span><span class="sxs-lookup"><span data-stu-id="b43bc-137">**Backend pool**</span></span> | <span data-ttu-id="b43bc-138">Fond IP adres, plně kvalifikovaný název domény společnosti nebo síťové adaptéry, které jsou toohello aplikační servery, které jsou hostiteli hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="b43bc-138">A pool of IP addresses, FQDN's, or NICs that are toohello application servers that host hello web application</span></span>|
| <span data-ttu-id="b43bc-139">**Test stavu**</span><span class="sxs-lookup"><span data-stu-id="b43bc-139">**Health probe**</span></span> | <span data-ttu-id="b43bc-140">Stav hello toomonitor členy fondu back-end hello používá vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="b43bc-140">A custom probe used toomonitor hello health of hello backend pool members</span></span>|
| <span data-ttu-id="b43bc-141">**Nastavení protokolu HTTP**</span><span class="sxs-lookup"><span data-stu-id="b43bc-141">**HTTP settings**</span></span> | <span data-ttu-id="b43bc-142">Kolekce nastavení, včetně, port, protokol, spřažení na základě souborů cookie, test a vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="b43bc-142">A collection of settings including, port, protocol, cookie-based affinity, probe, and timeout.</span></span>  <span data-ttu-id="b43bc-143">Tato nastavení určují, jak provoz se směruje toohello členy fondu back-end</span><span class="sxs-lookup"><span data-stu-id="b43bc-143">These settings determine how traffic is routed toohello backend pool members</span></span>|
| <span data-ttu-id="b43bc-144">**Front-endový port**</span><span class="sxs-lookup"><span data-stu-id="b43bc-144">**Frontend port**</span></span> | <span data-ttu-id="b43bc-145">Hello port, který hello brány aplikace nenaslouchala komunikaci na</span><span class="sxs-lookup"><span data-stu-id="b43bc-145">hello port that hello application gateway listens for traffic on</span></span>|
| <span data-ttu-id="b43bc-146">**Naslouchací proces**</span><span class="sxs-lookup"><span data-stu-id="b43bc-146">**Listener**</span></span> | <span data-ttu-id="b43bc-147">Kombinace protokolu, front-endové konfigurace protokolu IP a port front-endu.</span><span class="sxs-lookup"><span data-stu-id="b43bc-147">A combination of a protocol, frontend IP configuration, and frontend port.</span></span> <span data-ttu-id="b43bc-148">Je to, co poslouchá příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="b43bc-148">This is what listens for incoming requests.</span></span>
|<span data-ttu-id="b43bc-149">**Pravidlo**</span><span class="sxs-lookup"><span data-stu-id="b43bc-149">**Rule**</span></span>| <span data-ttu-id="b43bc-150">Trasy hello provoz toohello odpovídající back-end na základě nastavení protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b43bc-150">Routes hello traffic toohello appropriate backend based on HTTP settings.</span></span>|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates hello backend http settings toobe used. This component references hello $probe created in hello previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for hello application gateway toolisten on port 80 that will be used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates hello $publicip variable defined previously with hello front-end IP that will be used by hello listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates hello listener. hello listener is a combination of protocol and hello frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates hello rule that routes traffic toohello backend pools.  In this example we create a basic rule that uses hello previous defined http settings and backend address pool.  It also associates hello listener toohello rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets hello SKU of hello application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# hello final step creates hello application gateway with all hello previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-tooan-existing-application-gateway"></a><span data-ttu-id="b43bc-151">Přidat existující aplikace bránu tooan testu</span><span class="sxs-lookup"><span data-stu-id="b43bc-151">Add a probe tooan existing application gateway</span></span>

<span data-ttu-id="b43bc-152">Hello následující fragment kódu přidá existující aplikace bránu tooan testu.</span><span class="sxs-lookup"><span data-stu-id="b43bc-152">hello following code snippet adds a probe tooan existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create hello probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set hello backend HTTP settings toouse hello new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a><span data-ttu-id="b43bc-153">Odstranit sondu z existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="b43bc-153">Remove a probe from an existing application gateway</span></span>

<span data-ttu-id="b43bc-154">Následující fragment kódu Hello sondu Odebere existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-154">hello following code snippet removes a probe from an existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove hello probe from hello application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set hello backend HTTP settings tooremove hello reference toohello probe. hello backend http settings now use hello default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="b43bc-155">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b43bc-155">Get application gateway DNS name</span></span>

<span data-ttu-id="b43bc-156">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="b43bc-156">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="b43bc-157">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="b43bc-157">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="b43bc-158">tooensure koncoví uživatelé mohou dosáhl hello aplikační brány lze použít záznam CNAME toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-158">tooensure end users can hit hello application gateway a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="b43bc-159">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b43bc-159">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="b43bc-160">toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="b43bc-161">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="b43bc-161">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="b43bc-162">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b43bc-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b43bc-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b43bc-163">Next steps</span></span>

<span data-ttu-id="b43bc-164">Další snižování zátěže protokolu SSL tooconfigure navštivte stránky: [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="b43bc-164">Learn tooconfigure SSL offloading by visiting: [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

