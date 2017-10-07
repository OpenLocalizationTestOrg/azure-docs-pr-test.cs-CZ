---
title: "brány firewall v protokolu aaaConfigure webových aplikací – Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny k jak toostart pomocí webové aplikace brány firewall na existující nebo nové aplikační brány."
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
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a><span data-ttu-id="44b3c-103">Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway</span><span class="sxs-lookup"><span data-stu-id="44b3c-103">Configure web application firewall on a new or existing Application Gateway</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="44b3c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="44b3c-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="44b3c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44b3c-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="44b3c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="44b3c-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="44b3c-107">Zjistěte, jak toocreate brány firewall webových aplikací povoleno aplikační bránu, nebo přidejte webové aplikace brány firewall tooan existující Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-107">Learn how toocreate a web application firewall enabled Application Gateway or add web application firewall tooan existing Application Gateway.</span></span>

<span data-ttu-id="44b3c-108">Hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.</span><span class="sxs-lookup"><span data-stu-id="44b3c-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="44b3c-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="44b3c-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="44b3c-110">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="44b3c-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="44b3c-111">Application Gateway poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další.</span><span class="sxs-lookup"><span data-stu-id="44b3c-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="44b3c-112">toofind úplný seznam podporovaných funkcích, navštivte: [Přehled služby Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="44b3c-113">Hello následující článek ukazuje, jak příliš[webové aplikace brány firewall tooan existující aplikační brány přidat](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby Application Gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="44b3c-113">hello following article shows how too[add web application firewall tooan existing Application Gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an Application Gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![scénář image][scenario]

## <a name="waf-configuration-differences"></a><span data-ttu-id="44b3c-115">Rozdíly konfigurace firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="44b3c-115">WAF configuration differences</span></span>

<span data-ttu-id="44b3c-116">Pokud jste si přečetli [vytvoření služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md), rozumíte hello SKU nastavení tooconfigure při vytváření služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-116">If you have read [Create an Application Gateway with PowerShell](application-gateway-create-gateway-arm.md), you understand hello SKU settings tooconfigure when creating an Application Gateway.</span></span> <span data-ttu-id="44b3c-117">Firewall webových aplikací poskytuje další nastavení toodefine při konfiguraci hello SKU na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="44b3c-117">WAF provides additional settings toodefine when configuring hello SKU on an Application Gateway.</span></span> <span data-ttu-id="44b3c-118">Neexistují žádné další změny, které provedete na hello samotné bráně aplikace.</span><span class="sxs-lookup"><span data-stu-id="44b3c-118">There are no additional changes that you make on hello Application Gateway itself.</span></span>

| <span data-ttu-id="44b3c-119">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="44b3c-119">**Setting**</span></span> | <span data-ttu-id="44b3c-120">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="44b3c-120">**Details**</span></span>
|---|---|
|<span data-ttu-id="44b3c-121">**SKU**</span><span class="sxs-lookup"><span data-stu-id="44b3c-121">**SKU**</span></span> |<span data-ttu-id="44b3c-122">Normální Application Gateway bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti.</span><span class="sxs-lookup"><span data-stu-id="44b3c-122">A normal Application Gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="44b3c-123">Se zavedením hello firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**.</span><span class="sxs-lookup"><span data-stu-id="44b3c-123">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="44b3c-124">Malé aplikačních bran firewall webových aplikací nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="44b3c-124">WAF is not supported on small Application Gateways.</span></span>|
|<span data-ttu-id="44b3c-125">**Vrstvy**</span><span class="sxs-lookup"><span data-stu-id="44b3c-125">**Tier**</span></span> | <span data-ttu-id="44b3c-126">Hello dostupné jsou hodnoty **standardní** nebo **firewall webových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="44b3c-126">hello available values are **Standard** or **WAF**.</span></span> <span data-ttu-id="44b3c-127">Při použití brány firewall webových aplikací, **firewall webových aplikací** musí být vybrána.</span><span class="sxs-lookup"><span data-stu-id="44b3c-127">When using web application firewall, **WAF** must be chosen.</span></span>|
|<span data-ttu-id="44b3c-128">**Režim**</span><span class="sxs-lookup"><span data-stu-id="44b3c-128">**Mode**</span></span> | <span data-ttu-id="44b3c-129">Toto nastavení je režim hello firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="44b3c-129">This setting is hello mode of WAF.</span></span> <span data-ttu-id="44b3c-130">Povolené hodnoty jsou **detekce** a **prevence**.</span><span class="sxs-lookup"><span data-stu-id="44b3c-130">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="44b3c-131">Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="44b3c-131">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="44b3c-132">V režimu prevence události jsou protokolovány ale hello útočník obdrží 403 neoprávněným odpověď z hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-132">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello Application Gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="44b3c-133">Přidání webové aplikace brány firewall tooan existující Application Gateway</span><span class="sxs-lookup"><span data-stu-id="44b3c-133">Add web application firewall tooan existing Application Gateway</span></span>

<span data-ttu-id="44b3c-134">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="44b3c-134">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="44b3c-135">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-135">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="44b3c-136">Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="44b3c-136">Log in tooyour Azure Account.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

2. <span data-ttu-id="44b3c-137">Vyberte hello toouse předplatné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="44b3c-137">Select hello subscription toouse for this scenario.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. <span data-ttu-id="44b3c-138">Načtěte hello brány, který chcete přidat brány firewall webových aplikací na.</span><span class="sxs-lookup"><span data-stu-id="44b3c-138">Retrieve hello gateway that you are adding web application firewall to.</span></span>

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. <span data-ttu-id="44b3c-139">Nakonfigurujte skladová položka brány firewall hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="44b3c-139">Configure hello web application firewall sku.</span></span> <span data-ttu-id="44b3c-140">jsou k dispozici velikosti Hello **firewall webových aplikací\_velké** a **firewall webových aplikací\_střední**.</span><span class="sxs-lookup"><span data-stu-id="44b3c-140">hello available sizes are **WAF\_Large** and **WAF\_Medium**.</span></span> <span data-ttu-id="44b3c-141">Při použití brány firewall webových aplikací hello vrstva musí být **firewall webových aplikací**, hello kapacita musí být potvrzen, při nastavování hello sku.</span><span class="sxs-lookup"><span data-stu-id="44b3c-141">When web application firewall is used hello tier must be **WAF**, hello capacity must be confirmed when setting hello sku.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. <span data-ttu-id="44b3c-142">Nakonfigurujte nastavení hello firewall webových aplikací, jak jsou definovány v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="44b3c-142">Configure hello WAF settings as defined in hello following example:</span></span>

   <span data-ttu-id="44b3c-143">Pro **FirewallMode**, jsou k dispozici hodnoty hello prevence a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="44b3c-143">For **FirewallMode**, hello available values are Prevention and Detection.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. <span data-ttu-id="44b3c-144">Aktualizujte hello Application Gateway s nastavením hello definované v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="44b3c-144">Update hello Application Gateway with hello settings defined in hello preceding step.</span></span>

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

<span data-ttu-id="44b3c-145">Tento příkaz aktualizuje hello Application Gateway pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="44b3c-145">This command updates hello Application Gateway with web application firewall.</span></span> <span data-ttu-id="44b3c-146">Navštivte [diagnostics Application Gateway](application-gateway-diagnostics.md) toounderstand jak tooview protokoly pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-146">Visit [Application Gateway diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your Application Gateway.</span></span> <span data-ttu-id="44b3c-147">Z důvodu zabezpečení povaze toohello firewall webových aplikací zkontrolovat toobe třeba protokoly pravidelně toounderstand hello postavení zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="44b3c-147">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="44b3c-148">Vytvoření služby Application Gateway pomocí brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="44b3c-148">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="44b3c-149">Hello následující kroky vás prostřednictvím hello celý proces od začátku tooend pro vytvoření služby Application Gateway pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="44b3c-149">hello following steps take you through hello entire process from beginning tooend for creating an Application Gateway with web application firewall.</span></span>

<span data-ttu-id="44b3c-150">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="44b3c-150">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="44b3c-151">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-151">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="44b3c-152">Přihlaste se spuštěním tooAzure `Login-AzureRmAccount`, jsou výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="44b3c-152">Log in tooAzure by running `Login-AzureRmAccount`, you are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="44b3c-153">Zkontrolujte předplatná hello pro účet hello spuštěním`Get-AzureRmSubscription`</span><span class="sxs-lookup"><span data-stu-id="44b3c-153">Check hello subscriptions for hello account by running `Get-AzureRmSubscription`</span></span>

1. <span data-ttu-id="44b3c-154">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="44b3c-154">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a><span data-ttu-id="44b3c-155">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="44b3c-155">Create a resource group</span></span>

<span data-ttu-id="44b3c-156">Vytvořte skupinu prostředků pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-156">Create a resource group for hello Application Gateway.</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="44b3c-157">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="44b3c-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="44b3c-158">Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="44b3c-158">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="44b3c-159">Ujistěte se, že všechny příkazy, které používá toocreate na aplikační brána hello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="44b3c-159">Make sure that all commands toocreate an Application Gateway uses hello same resource group.</span></span>

<span data-ttu-id="44b3c-160">V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "Appgw-RG" a umístěním "západní USA."</span><span class="sxs-lookup"><span data-stu-id="44b3c-160">In hello preceding example, we created a resource group called "appgw-RG" and location "West US."</span></span>

> [!NOTE]
> <span data-ttu-id="44b3c-161">Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-161">If you need tooconfigure a custom probe for your Application Gateway, see [Create an Application Gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="44b3c-162">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-162">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

### <a name="configure-virtual-network"></a><span data-ttu-id="44b3c-163">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="44b3c-163">Configure virtual network</span></span>

<span data-ttu-id="44b3c-164">Aplikační brána vyžaduje vlastní podsíť.</span><span class="sxs-lookup"><span data-stu-id="44b3c-164">Application Gateway requires a subnet of its own.</span></span> <span data-ttu-id="44b3c-165">V tomto kroku vytvoříte virtuální síť s adresní prostor 10.0.0.0/16 a dvě podsítě – jednu pro hello aplikační brány a jeden pro členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="44b3c-165">In this step, you create a virtual network with an address space of 10.0.0.0/16 and two subnets, one for hello Application Gateway and one for backend pool members.</span></span>

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a><span data-ttu-id="44b3c-166">Konfigurace veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="44b3c-166">Configure public IP address</span></span>

<span data-ttu-id="44b3c-167">Aplikační brána v pořadí toohandle externí požadavků, vyžaduje veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="44b3c-167">In order toohandle external requests, Application Gateway requires a public IP address.</span></span> <span data-ttu-id="44b3c-168">Tato veřejná IP adresa nesmí mít `DomainNameLabel` definované toobe používané hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-168">This public IP address must not have a `DomainNameLabel` defined toobe used by hello Application Gateway.</span></span>

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a><span data-ttu-id="44b3c-169">Konfigurace hello Application Gateway</span><span class="sxs-lookup"><span data-stu-id="44b3c-169">Configure hello Application Gateway</span></span>

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> <span data-ttu-id="44b3c-170">Application Gateway, které jsou vytvořené pomocí konfigurace brány firewall hello základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="44b3c-170">Application Gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="44b3c-171">Získat název DNS brány aplikace</span><span class="sxs-lookup"><span data-stu-id="44b3c-171">Get Application Gateway DNS name</span></span>

<span data-ttu-id="44b3c-172">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="44b3c-172">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="44b3c-173">Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který není popisný.</span><span class="sxs-lookup"><span data-stu-id="44b3c-173">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="44b3c-174">tooensure koncoví uživatelé mohou dosáhl hello Application Gateway, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-174">tooensure end users can hit hello Application Gateway, a CNAME record can be used toopoint toohello public endpoint of hello Application Gateway.</span></span> <span data-ttu-id="44b3c-175">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="44b3c-175">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="44b3c-176">tooconfigure alias, načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-176">tooconfigure an alias, retrieve details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello Application Gateway.</span></span> <span data-ttu-id="44b3c-177">název DNS Hello Application Gateway by měl být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="44b3c-177">hello Application Gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="44b3c-178">použití Hello záznamů A nedoporučuje, protože hello VIP může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="44b3c-178">hello use of A-records is not recommended since hello VIP may change on restart of Application Gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="44b3c-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44b3c-179">Next steps</span></span>

<span data-ttu-id="44b3c-180">Zjistěte, jak protokolování diagnostiky tooconfigure, toolog hello události, které jsou zjištěna nebo předcházet pomocí brány firewall webových aplikací, navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="44b3c-180">Learn how tooconfigure diagnostic logging, toolog hello events that are detected or prevented with web application firewall by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
