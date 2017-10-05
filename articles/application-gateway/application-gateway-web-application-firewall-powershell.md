---
title: "Konfigurace brány firewall webových aplikací – Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny o tom, jak začít používat brány firewall webových aplikací na existující nebo nové aplikační brány."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: dab489a1c9fb7d4a51b9ccbce543b209bec23575
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a><span data-ttu-id="a502b-103">Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a502b-103">Configure web application firewall on a new or existing Application Gateway</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a502b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a502b-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="a502b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a502b-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="a502b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a502b-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="a502b-107">Zjistěte, jak vytvořit webovou aplikaci povoleno Application Gateway s branou firewall nebo přidání brány firewall webových aplikací do existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a502b-107">Learn how to create a web application firewall enabled Application Gateway or add web application firewall to an existing Application Gateway.</span></span>

<span data-ttu-id="a502b-108">Brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.</span><span class="sxs-lookup"><span data-stu-id="a502b-108">The web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="a502b-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="a502b-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="a502b-110">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="a502b-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="a502b-111">Application Gateway poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další.</span><span class="sxs-lookup"><span data-stu-id="a502b-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="a502b-112">Úplný seznam podporovaných funkcích naleznete: [Přehled služby Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-112">To find a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="a502b-113">Tento článek ukazuje postup [přidání brány firewall webových aplikací do existující aplikace bránu](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby Application Gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="a502b-113">The following article shows how to [add web application firewall to an existing Application Gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an Application Gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![scénář image][scenario]

## <a name="waf-configuration-differences"></a><span data-ttu-id="a502b-115">Rozdíly konfigurace firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="a502b-115">WAF configuration differences</span></span>

<span data-ttu-id="a502b-116">Pokud jste si přečetli [vytvoření služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md), rozumíte SKU nastavení ke konfiguraci při vytváření služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-116">If you have read [Create an Application Gateway with PowerShell](application-gateway-create-gateway-arm.md), you understand the SKU settings to configure when creating an Application Gateway.</span></span> <span data-ttu-id="a502b-117">Firewall webových aplikací poskytuje další nastavení můžete definovat při konfiguraci verze SKU na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a502b-117">WAF provides additional settings to define when configuring the SKU on an Application Gateway.</span></span> <span data-ttu-id="a502b-118">Neexistují žádné další změny, které provedete ve službě Application Gateway sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a502b-118">There are no additional changes that you make on the Application Gateway itself.</span></span>

| <span data-ttu-id="a502b-119">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="a502b-119">**Setting**</span></span> | <span data-ttu-id="a502b-120">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="a502b-120">**Details**</span></span>
|---|---|
|<span data-ttu-id="a502b-121">**SKU**</span><span class="sxs-lookup"><span data-stu-id="a502b-121">**SKU**</span></span> |<span data-ttu-id="a502b-122">Normální Application Gateway bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti.</span><span class="sxs-lookup"><span data-stu-id="a502b-122">A normal Application Gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="a502b-123">Se zavedením firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**.</span><span class="sxs-lookup"><span data-stu-id="a502b-123">With the introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="a502b-124">Malé aplikačních bran firewall webových aplikací nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="a502b-124">WAF is not supported on small Application Gateways.</span></span>|
|<span data-ttu-id="a502b-125">**Vrstvy**</span><span class="sxs-lookup"><span data-stu-id="a502b-125">**Tier**</span></span> | <span data-ttu-id="a502b-126">Dostupné hodnoty jsou **standardní** nebo **firewall webových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="a502b-126">The available values are **Standard** or **WAF**.</span></span> <span data-ttu-id="a502b-127">Při použití brány firewall webových aplikací, **firewall webových aplikací** musí být vybrána.</span><span class="sxs-lookup"><span data-stu-id="a502b-127">When using web application firewall, **WAF** must be chosen.</span></span>|
|<span data-ttu-id="a502b-128">**Režim**</span><span class="sxs-lookup"><span data-stu-id="a502b-128">**Mode**</span></span> | <span data-ttu-id="a502b-129">Toto nastavení je režim firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a502b-129">This setting is the mode of WAF.</span></span> <span data-ttu-id="a502b-130">Povolené hodnoty jsou **detekce** a **prevence**.</span><span class="sxs-lookup"><span data-stu-id="a502b-130">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="a502b-131">Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="a502b-131">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="a502b-132">V režimu prevence události jsou protokolovány ale útočník přijme 403 neoprávněným odpověď ze služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-132">In prevention mode, events are still logged but the attacker receives a 403 unauthorized response from the Application Gateway.</span></span>|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a><span data-ttu-id="a502b-133">Přidání brány firewall webových aplikací do existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="a502b-133">Add web application firewall to an existing Application Gateway</span></span>

<span data-ttu-id="a502b-134">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a502b-134">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="a502b-135">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-135">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="a502b-136">Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a502b-136">Log in to your Azure Account.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

2. <span data-ttu-id="a502b-137">Vyberte předplatné, které chcete použít pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="a502b-137">Select the subscription to use for this scenario.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. <span data-ttu-id="a502b-138">Načtěte brány, kterou přidáváte brány firewall webových aplikací na.</span><span class="sxs-lookup"><span data-stu-id="a502b-138">Retrieve the gateway that you are adding web application firewall to.</span></span>

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. <span data-ttu-id="a502b-139">Nakonfigurujte sku webové aplikace brány firewall.</span><span class="sxs-lookup"><span data-stu-id="a502b-139">Configure the web application firewall sku.</span></span> <span data-ttu-id="a502b-140">Dostupné velikosti jsou **firewall webových aplikací\_velké** a **firewall webových aplikací\_střední**.</span><span class="sxs-lookup"><span data-stu-id="a502b-140">The available sizes are **WAF\_Large** and **WAF\_Medium**.</span></span> <span data-ttu-id="a502b-141">Při použití brány firewall webových aplikací vrstva musí být **firewall webových aplikací**, kapacita musí být potvrzen, při nastavování verze sku.</span><span class="sxs-lookup"><span data-stu-id="a502b-141">When web application firewall is used the tier must be **WAF**, the capacity must be confirmed when setting the sku.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. <span data-ttu-id="a502b-142">Nakonfigurujte nastavení firewall webových aplikací, jak jsou definovány v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a502b-142">Configure the WAF settings as defined in the following example:</span></span>

   <span data-ttu-id="a502b-143">Pro **FirewallMode**, dostupné hodnoty jsou prevence a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="a502b-143">For **FirewallMode**, the available values are Prevention and Detection.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. <span data-ttu-id="a502b-144">Aktualizujte aplikační bránu s nastavením, které jsou definované v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="a502b-144">Update the Application Gateway with the settings defined in the preceding step.</span></span>

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

<span data-ttu-id="a502b-145">Tento příkaz aktualizuje aplikační bránu pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a502b-145">This command updates the Application Gateway with web application firewall.</span></span> <span data-ttu-id="a502b-146">Navštivte [diagnostics Application Gateway](application-gateway-diagnostics.md) pochopit, jak zobrazit protokoly pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-146">Visit [Application Gateway diagnostics](application-gateway-diagnostics.md) to understand how to view logs for your Application Gateway.</span></span> <span data-ttu-id="a502b-147">Vzhledem k zabezpečení povaze firewall webových aplikací protokoly třeba, aby pravidelně pochopit postavení zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a502b-147">Due to the security nature of WAF, logs need to be reviewed regularly to understand the security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="a502b-148">Vytvoření služby Application Gateway pomocí brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="a502b-148">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="a502b-149">Následující postup vás provede celý proces od začátku do konce pro vytvoření služby Application Gateway pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a502b-149">The following steps take you through the entire process from beginning to end for creating an Application Gateway with web application firewall.</span></span>

<span data-ttu-id="a502b-150">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a502b-150">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="a502b-151">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-151">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="a502b-152">Přihlaste se k Azure spuštěním `Login-AzureRmAccount`, zobrazí se výzva k ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a502b-152">Log in to Azure by running `Login-AzureRmAccount`, you are prompted to authenticate with your credentials.</span></span>

1. <span data-ttu-id="a502b-153">Zkontrolujte předplatná pro příslušný účet spuštěním`Get-AzureRmSubscription`</span><span class="sxs-lookup"><span data-stu-id="a502b-153">Check the subscriptions for the account by running `Get-AzureRmSubscription`</span></span>

1. <span data-ttu-id="a502b-154">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a502b-154">Choose which of your Azure subscriptions to use.</span></span>

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a><span data-ttu-id="a502b-155">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a502b-155">Create a resource group</span></span>

<span data-ttu-id="a502b-156">Vytvořte skupinu prostředků pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-156">Create a resource group for the Application Gateway.</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="a502b-157">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="a502b-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a502b-158">Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a502b-158">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="a502b-159">Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používá stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a502b-159">Make sure that all commands to create an Application Gateway uses the same resource group.</span></span>

<span data-ttu-id="a502b-160">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem "appgw-RG" a umístěním "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="a502b-160">In the preceding example, we created a resource group called "appgw-RG" and location "West US."</span></span>

> [!NOTE]
> <span data-ttu-id="a502b-161">Pokud potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si téma [vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-161">If you need to configure a custom probe for your Application Gateway, see [Create an Application Gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="a502b-162">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-162">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

### <a name="configure-virtual-network"></a><span data-ttu-id="a502b-163">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a502b-163">Configure virtual network</span></span>

<span data-ttu-id="a502b-164">Aplikační brána vyžaduje vlastní podsíť.</span><span class="sxs-lookup"><span data-stu-id="a502b-164">Application Gateway requires a subnet of its own.</span></span> <span data-ttu-id="a502b-165">V tomto kroku vytvoříte virtuální síť s adresní prostor 10.0.0.0/16 a dvě podsítě, jeden pro službu Application Gateway a jeden pro členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a502b-165">In this step, you create a virtual network with an address space of 10.0.0.0/16 and two subnets, one for the Application Gateway and one for backend pool members.</span></span>

```powershell
# Create a subnet configuration object for the Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your Application Gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a><span data-ttu-id="a502b-166">Konfigurace veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="a502b-166">Configure public IP address</span></span>

<span data-ttu-id="a502b-167">Aby bylo možné zpracovávat požadavky externí, aplikační brána vyžaduje veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a502b-167">In order to handle external requests, Application Gateway requires a public IP address.</span></span> <span data-ttu-id="a502b-168">Tato veřejná IP adresa nesmí mít `DomainNameLabel` definovaná používat službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-168">This public IP address must not have a `DomainNameLabel` defined to be used by the Application Gateway.</span></span>

```powershell
# Create a public IP address for use with the Application Gateway. Defining the domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a><span data-ttu-id="a502b-169">Nakonfigurujte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="a502b-169">Configure the Application Gateway</span></span>

```powershell
# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool to hold the addresses or NICs for the application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authenication certificate that will be used to communicate with the backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration to associate the public IP address with the Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the Application Gateway. This certificate is used to decrypt and re-encrypt the traffic on the Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the Application Gateway. Assign the front-end ip configuration, port, and ssl certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure the waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the Application Gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> <span data-ttu-id="a502b-170">Application Gateway vytvoří s konfigurací brány firewall základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="a502b-170">Application Gateways created with the basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="a502b-171">Získat název DNS brány aplikace</span><span class="sxs-lookup"><span data-stu-id="a502b-171">Get Application Gateway DNS name</span></span>

<span data-ttu-id="a502b-172">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="a502b-172">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="a502b-173">Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který není popisný.</span><span class="sxs-lookup"><span data-stu-id="a502b-173">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="a502b-174">Zajistěte, aby koncoví uživatelé mohou dosáhl aplikační bránu, záznamu CNAME použít tak, aby odkazoval koncový bod veřejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a502b-174">To ensure end users can hit the Application Gateway, a CNAME record can be used to point to the public endpoint of the Application Gateway.</span></span> <span data-ttu-id="a502b-175">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a502b-175">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="a502b-176">Pokud chcete konfigurovat alias, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-176">To configure an alias, retrieve details of the Application Gateway and its associated IP/DNS name using the PublicIPAddress element attached to the Application Gateway.</span></span> <span data-ttu-id="a502b-177">Název DNS služby Application Gateway by měl použít k vytvoření záznamu CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS.</span><span class="sxs-lookup"><span data-stu-id="a502b-177">The Application Gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="a502b-178">Použití záznamů A se nedoporučuje, protože VIP může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a502b-178">The use of A-records is not recommended since the VIP may change on restart of Application Gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a502b-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a502b-179">Next steps</span></span>

<span data-ttu-id="a502b-180">Naučte se konfigurovat protokolování diagnostiky, do protokolu událostí, které jsou zjištěna nebo nebylo možné provést pomocí brány firewall webových aplikací navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="a502b-180">Learn how to configure diagnostic logging, to log the events that are detected or prevented with web application firewall by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
