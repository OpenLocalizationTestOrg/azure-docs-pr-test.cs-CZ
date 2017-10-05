---
title: "Azure Application Gateway pomocí vyrovnávání zatížení interní - prostředí PowerShell | Microsoft Docs"
description: "Tahle stránka poskytuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure application gateway s interním nástrojem pro vyrovnávání zatížení (ILB) pro nástroj Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: d218eab7e9f124e4825a8a781b4eeb0dcca58b4a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="9471a-103">Vytvořte aplikační bránu s interním nástrojem pro vyrovnávání zatížení (ILB) pomocí nástroje Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9471a-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9471a-104">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="9471a-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="9471a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="9471a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="9471a-106">Služba Azure Application Gateway se dá nakonfigurovat pomocí virtuální IP adresy s přístupem k Internetu, nebo pomocí interního koncového bodu, který není vystavený v Internetu, známého také jako koncový bod interního nástroje pro vyrovnávání zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="9471a-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed to the Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="9471a-107">Konfigurace brány pomocí ILB je užitečná pro interní-obchodní aplikace, které nejsou vystaveny v Internetu.</span><span class="sxs-lookup"><span data-stu-id="9471a-107">Configuring the gateway with an ILB is useful for internal line-of-business applications that are not exposed to the Internet.</span></span> <span data-ttu-id="9471a-108">To se hodí i pro služby a vrstvy v rámci vícevrstvé aplikace, které se nacházejí v rámci hranice zabezpečení nevystavené k Internetu, ale stále vyžadují distribuci zatížení modelu kruhového dotazování, dlouhodobost relace nebo ukončení SSL (Secure Sockets Layer).</span><span class="sxs-lookup"><span data-stu-id="9471a-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed to the Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="9471a-109">Tenhle článek vás provede kroky konfigurace aplikační brány s ILB.</span><span class="sxs-lookup"><span data-stu-id="9471a-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9471a-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9471a-110">Before you begin</span></span>

1. <span data-ttu-id="9471a-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="9471a-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="9471a-112">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="9471a-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="9471a-113">Vytvořte virtuální síť a podsíť služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="9471a-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="9471a-114">Ujistěte se, že tuto podsíť nepoužívají žádné virtuální počítače ani cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="9471a-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="9471a-115">Aplikační brána musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9471a-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="9471a-116">Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.</span><span class="sxs-lookup"><span data-stu-id="9471a-116">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="9471a-117">Co je potřeba k vytvoření služby Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="9471a-117">What is required to create an application gateway?</span></span>

* <span data-ttu-id="9471a-118">**Fond back-end serverů:** seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="9471a-118">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="9471a-119">Uvedené IP adresy by měly buď patřit do virtuální sítě, ale v jiné podsíti pro aplikační bránu, nebo by se mělo jednat o veřejné IP nebo virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9471a-119">The IP addresses listed should either belong to the virtual network but in a different subnet for the application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="9471a-120">**Nastavení fondu back-end serverů:** každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="9471a-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="9471a-121">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="9471a-121">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="9471a-122">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="9471a-122">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="9471a-123">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="9471a-123">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="9471a-124">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, s rozlišením malých a velkých písmen) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="9471a-124">**Listener:** The listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="9471a-125">**Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="9471a-125">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="9471a-126">V tuhle chvíli se podporuje jenom *základní* pravidlo.</span><span class="sxs-lookup"><span data-stu-id="9471a-126">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="9471a-127">*Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="9471a-127">The *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="9471a-128">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="9471a-128">Create an application gateway</span></span>

<span data-ttu-id="9471a-129">Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.</span><span class="sxs-lookup"><span data-stu-id="9471a-129">The difference between using Azure Classic and Azure Resource Manager is the order in which you create the application gateway and the items that need to be configured.</span></span>
<span data-ttu-id="9471a-130">S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="9471a-130">With Resource Manager, all items that make an application gateway is configured individually and then put together to create the application gateway resource.</span></span>

<span data-ttu-id="9471a-131">Toto jsou kroky, které se musí provést k vytvoření služby Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="9471a-131">Here are the steps that are needed to create an application gateway:</span></span>

1. <span data-ttu-id="9471a-132">Vytvořte skupinu prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9471a-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="9471a-133">Vytvoření virtuální sítě a podsítě pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="9471a-133">Create a virtual network and a subnet for the application gateway</span></span>
3. <span data-ttu-id="9471a-134">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="9471a-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="9471a-135">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="9471a-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="9471a-136">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9471a-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="9471a-137">Ujistěte se, že jste přepnuli režim prostředí PowerShell tak, aby se mohly použít rutiny Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="9471a-137">Make sure that you switch PowerShell mode to use the Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="9471a-138">Další informace jsou k dispozici v části [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9471a-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="9471a-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9471a-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="9471a-140">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9471a-140">Step 2</span></span>

<span data-ttu-id="9471a-141">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="9471a-141">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="9471a-142">Zobrazí se výzva k ověření pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="9471a-142">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="9471a-143">Krok 3</span><span class="sxs-lookup"><span data-stu-id="9471a-143">Step 3</span></span>

<span data-ttu-id="9471a-144">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="9471a-144">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="9471a-145">Krok 4</span><span class="sxs-lookup"><span data-stu-id="9471a-145">Step 4</span></span>

<span data-ttu-id="9471a-146">Vytvořte novou skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="9471a-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="9471a-147">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="9471a-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="9471a-148">To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9471a-148">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="9471a-149">Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9471a-149">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="9471a-150">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem "appgw-rg" a umístěním "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="9471a-150">In the preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="9471a-151">Vytvoření virtuální sítě a podsítě pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="9471a-151">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="9471a-152">Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:</span><span class="sxs-lookup"><span data-stu-id="9471a-152">The following example shows how to create a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="9471a-153">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9471a-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="9471a-154">Tento krok rozsah adres 10.0.0.0/24 přiřadí proměnné podsítě, který se má použít k vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9471a-154">This step assigns the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="9471a-155">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9471a-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="9471a-156">Tento krok vytvoří virtuální síť s názvem "appgwvnet" v prostředku skupiny "appgw-rg" pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="9471a-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="9471a-157">Krok 3</span><span class="sxs-lookup"><span data-stu-id="9471a-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="9471a-158">Tento krok se přiřadí objekt podsítě k proměnné $subnet pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="9471a-158">This step assigns the subnet object to variable $subnet for the next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="9471a-159">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="9471a-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="9471a-160">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9471a-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="9471a-161">Tento krok vytvoří konfigurace IP aplikační brány s názvem "gatewayIP01".</span><span class="sxs-lookup"><span data-stu-id="9471a-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="9471a-162">Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="9471a-162">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="9471a-163">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9471a-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="9471a-164">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9471a-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="9471a-165">Tento krok se konfiguruje fond back-end IP adresy s názvem "pool01" s IP adresami "10.1.1.8, 10.1.1.9, 10.1.1.10".</span><span class="sxs-lookup"><span data-stu-id="9471a-165">This step configures the back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="9471a-166">Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9471a-166">Those are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="9471a-167">Předchozí IP adresy nahradíte vlastními aplikačními koncovými body IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9471a-167">You replace the preceding IP addresses to add your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="9471a-168">Krok 3</span><span class="sxs-lookup"><span data-stu-id="9471a-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="9471a-169">Tento krok nakonfiguruje aplikaci brány nastavení "poolsetting01" pro zatížení vyrovnáváním síťového provozu ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="9471a-169">This step configures application gateway setting "poolsetting01" for the load balanced network traffic in the back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="9471a-170">Krok 4</span><span class="sxs-lookup"><span data-stu-id="9471a-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="9471a-171">Tento krok nakonfiguruje port front-end IP adresy s názvem "frontendport01" pro ILB.</span><span class="sxs-lookup"><span data-stu-id="9471a-171">This step configures the front-end IP port named "frontendport01" for the ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="9471a-172">Krok 5</span><span class="sxs-lookup"><span data-stu-id="9471a-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="9471a-173">Tento krok vytvoří konfiguraci front-end IP adresy, nazvanou "fipconfig01" a přidruží ji s privátní IP Adresou z aktuální podsítě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9471a-173">This step creates the front-end IP configuration called "fipconfig01" and associates it with a private IP from the current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="9471a-174">Krok 6</span><span class="sxs-lookup"><span data-stu-id="9471a-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="9471a-175">Tento krok vytvoří naslouchací proces nazvaný "listener01" a přiřadí front-end port ke konfiguraci front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9471a-175">This step creates the listener called "listener01" and associates the front-end port to the front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="9471a-176">Krok 7</span><span class="sxs-lookup"><span data-stu-id="9471a-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="9471a-177">Tento krok vytvoří pravidlo směrování pro vyrovnávání zatížení názvem "rule01", které konfiguruje chování nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9471a-177">This step creates the load balancer routing rule called "rule01" that configures the load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="9471a-178">Krok 8</span><span class="sxs-lookup"><span data-stu-id="9471a-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="9471a-179">Tento krok se nakonfiguruje velikost instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="9471a-179">This step configures the instance size of the application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="9471a-180">Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="9471a-180">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="9471a-181">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="9471a-181">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="9471a-182">Můžete vybrat mezi Standard_Small, Standard_Medium a Standard_Large.</span><span class="sxs-lookup"><span data-stu-id="9471a-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="9471a-183">Vytvořte aplikační bránu pomocí New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="9471a-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="9471a-184">Vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="9471a-184">Creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="9471a-185">V tomto příkladu má služba Application Gateway název „appgwtest“.</span><span class="sxs-lookup"><span data-stu-id="9471a-185">In this example, the application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="9471a-186">Tento krok vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="9471a-186">This step creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="9471a-187">V příkladu se aplikační brána nazývá „appgwtest“.</span><span class="sxs-lookup"><span data-stu-id="9471a-187">In the example, the application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="9471a-188">Odstranění služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="9471a-188">Delete an application gateway</span></span>

<span data-ttu-id="9471a-189">Pokud chcete odstranit aplikační bránu, musíte udělat následující kroky v pořadí:</span><span class="sxs-lookup"><span data-stu-id="9471a-189">To delete an application gateway, you need to do the following steps in order:</span></span>

1. <span data-ttu-id="9471a-190">Pomocí rutiny `Stop-AzureRmApplicationGateway` zastavte bránu.</span><span class="sxs-lookup"><span data-stu-id="9471a-190">Use the `Stop-AzureRmApplicationGateway` cmdlet to stop the gateway.</span></span>
2. <span data-ttu-id="9471a-191">Pomocí rutiny `Remove-AzureRmApplicationGateway` bránu odeberte.</span><span class="sxs-lookup"><span data-stu-id="9471a-191">Use the `Remove-AzureRmApplicationGateway` cmdlet to remove the gateway.</span></span>
3. <span data-ttu-id="9471a-192">Zkontrolujte odstranění brány pomocí rutiny `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="9471a-192">Verify that the gateway has been removed by using the `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="9471a-193">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9471a-193">Step 1</span></span>

<span data-ttu-id="9471a-194">Získejte objekt služby Application Gateway a přidružte ho k proměnné „$getgw“.</span><span class="sxs-lookup"><span data-stu-id="9471a-194">Get the application gateway object and associate it to a variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="9471a-195">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9471a-195">Step 2</span></span>

<span data-ttu-id="9471a-196">Pomocí rutiny `Stop-AzureRmApplicationGateway` zastavte službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="9471a-196">Use `Stop-AzureRmApplicationGateway` to stop the application gateway.</span></span> <span data-ttu-id="9471a-197">Tento příklad ukazuje `Stop-AzureRmApplicationGateway` rutiny na prvním řádku, následovanou výstupem.</span><span class="sxs-lookup"><span data-stu-id="9471a-197">This sample shows the `Stop-AzureRmApplicationGateway` cmdlet on the first line, followed by the output.</span></span>

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="9471a-198">Jakmile je služba Application Gateway v zastaveném stavu, pomocí rutiny `Remove-AzureRmApplicationGateway` službu odstraňte.</span><span class="sxs-lookup"><span data-stu-id="9471a-198">Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.</span></span>

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> <span data-ttu-id="9471a-199">K potlačení zprávy s potvrzením o odstranění se dá použít přepínač **-force**.</span><span class="sxs-lookup"><span data-stu-id="9471a-199">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="9471a-200">Pokud chcete zkontrolovat, že se služba odstranila, můžete použít rutinu `Get-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="9471a-200">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="9471a-201">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="9471a-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="9471a-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9471a-202">Next steps</span></span>

<span data-ttu-id="9471a-203">Když chcete konfigurovat přesměrování zpracování SSL, přejděte do části [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="9471a-203">If you want to configure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="9471a-204">Když chcete provést konfiguraci aplikační brány pro použití s ILB, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="9471a-204">If you want to configure an application gateway to use with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="9471a-205">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="9471a-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="9471a-206">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="9471a-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="9471a-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="9471a-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
