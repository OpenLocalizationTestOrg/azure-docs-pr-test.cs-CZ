---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pomocí pravidel směrování adres URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: ba756d3262b9780c5701e69faad860ba32bba08b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a><span data-ttu-id="b028d-103">Vytvoření služby application gateway pomocí směrování na základě cesty</span><span class="sxs-lookup"><span data-stu-id="b028d-103">Create an application gateway using Path-based routing</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b028d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b028d-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="b028d-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="b028d-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="b028d-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b028d-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="b028d-107">Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL požadavku Http.</span><span class="sxs-lookup"><span data-stu-id="b028d-107">URL Path-based routing enables you to associate routes based on the URL path of an Http request.</span></span> <span data-ttu-id="b028d-108">Zkontroluje, pokud je trasu k back-end fondu pro adresu URL uvedené v bráně aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="b028d-108">It checks if there is a route to a back-end pool configured for the URL presented in the Application Gateway.</span></span> <span data-ttu-id="b028d-109">Pak odešle síťový provoz na definované fond back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-109">It then sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="b028d-110">Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="b028d-110">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="b028d-111">Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b028d-111">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="b028d-112">Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="b028d-112">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="b028d-113">Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při PathBasedRouting kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-113">Basic rule type provides round-robin service for the back-end pools while PathBasedRouting in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="b028d-114">Scénář</span><span class="sxs-lookup"><span data-stu-id="b028d-114">Scenario</span></span>

<span data-ttu-id="b028d-115">V následujícím příkladu se Aplikační brána obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: fondu video serverů a fond serverů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b028d-115">In the following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: video server pool and image server pool.</span></span>

<span data-ttu-id="b028d-116">Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (pool1) a http://contoso.com/video * jsou směrovány do fondu serverů videa (pool2).</span><span class="sxs-lookup"><span data-stu-id="b028d-116">Requests for http://contoso.com/image* are routed to image server pool (pool1), and http://contoso.com/video* are routed to video server pool (pool2).</span></span> <span data-ttu-id="b028d-117">Pokud cesta vzory neodpovídají, je vybrán výchozí fond serverů (pool1).</span><span class="sxs-lookup"><span data-stu-id="b028d-117">if none of the path patterns match, a default server pool (pool1) is selected.</span></span>

![Adresa URL trasy](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a><span data-ttu-id="b028d-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b028d-119">Before you begin</span></span>

1. <span data-ttu-id="b028d-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="b028d-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="b028d-121">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b028d-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="b028d-122">Vytvoříte virtuální síť a podsíť pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="b028d-122">You create a virtual network and subnet for Application Gateway.</span></span> <span data-ttu-id="b028d-123">Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť.</span><span class="sxs-lookup"><span data-stu-id="b028d-123">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="b028d-124">Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b028d-124">The application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="b028d-125">Servery přidané do fondu back-end pro použití služby application gateway, musí existovat nebo mít své koncové body vytvořené ve virtuální síti nebo s veřejné nebo virtuálními IP Adresami přiřazen.</span><span class="sxs-lookup"><span data-stu-id="b028d-125">The servers added to the back-end pool to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="b028d-126">Co je potřeba k vytvoření služby Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="b028d-126">What is required to create an application gateway?</span></span>

* <span data-ttu-id="b028d-127">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="b028d-127">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="b028d-128">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="b028d-128">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="b028d-129">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="b028d-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="b028d-130">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="b028d-130">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="b028d-131">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-131">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="b028d-132">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="b028d-132">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="b028d-133">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="b028d-133">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="b028d-134">**Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="b028d-134">**Rule:** The rule binds the listener, the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="b028d-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-135">Create an application gateway</span></span>

<span data-ttu-id="b028d-136">Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.</span><span class="sxs-lookup"><span data-stu-id="b028d-136">The difference between using Azure Classic and Azure Resource Manager is the order in which you create the application gateway and the items that need to be configured.</span></span>

<span data-ttu-id="b028d-137">S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-137">With Resource Manager, all items that make an application gateway are configured individually and then put together to create the application gateway resource.</span></span>

<span data-ttu-id="b028d-138">Toto jsou kroky, které se musí provést k vytvoření služby Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="b028d-138">Here are the steps that are needed to create an application gateway:</span></span>

1. <span data-ttu-id="b028d-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b028d-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="b028d-140">Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-140">Create a virtual network, subnet, and public IP for the application gateway.</span></span>
3. <span data-ttu-id="b028d-141">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-141">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="b028d-142">Vytvoření prostředku služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-142">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="b028d-143">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b028d-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="b028d-144">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b028d-144">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="b028d-145">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b028d-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="b028d-146">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b028d-146">Step 1</span></span>

<span data-ttu-id="b028d-147">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="b028d-147">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="b028d-148">Zobrazí se výzva k ověření pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b028d-148">You are prompted to authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="b028d-149">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b028d-149">Step 2</span></span>

<span data-ttu-id="b028d-150">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="b028d-150">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="b028d-151">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b028d-151">Step 3</span></span>

<span data-ttu-id="b028d-152">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="b028d-152">Choose which of your Azure subscriptions to use.</span></span> <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="b028d-153">Krok 4</span><span class="sxs-lookup"><span data-stu-id="b028d-153">Step 4</span></span>

<span data-ttu-id="b028d-154">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="b028d-154">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

<span data-ttu-id="b028d-155">Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:</span><span class="sxs-lookup"><span data-stu-id="b028d-155">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

<span data-ttu-id="b028d-156">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="b028d-156">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="b028d-157">Tato skupina prostředků se používá jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b028d-157">This resource group is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="b028d-158">Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b028d-158">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="b028d-159">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem „appgw-RG“ a umístěním „Západní USA“.</span><span class="sxs-lookup"><span data-stu-id="b028d-159">In the example above, we created a resource group called "appgw-RG" and location "West US".</span></span>

> [!NOTE]
> <span data-ttu-id="b028d-160">Pokud pro svoji službu Application Gateway potřebujete nakonfigurovat vlastní test paměti, přečtěte si článek [Vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b028d-160">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="b028d-161">Další informace najdete v článku [Vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b028d-161">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="b028d-162">Vytvoření virtuální sítě a podsítě pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-162">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="b028d-163">Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="b028d-163">The following example shows how to create a virtual network by using Resource Manager.</span></span> <span data-ttu-id="b028d-164">Tento příklad vytvoří virtuální síť pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-164">This example creates a VNET for the Application Gateway.</span></span> <span data-ttu-id="b028d-165">Aplikační brána vyžaduje vlastní podsíti, z tohoto důvodu je menší než adresní prostor sítě VNET podsíť vytvořená pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-165">Application Gateway requires its own subnet, for this reason the subnet created for the Application Gateway is smaller than the VNET address space.</span></span> <span data-ttu-id="b028d-166">To umožňuje jiné prostředky, včetně mimo jiné webové servery do nakonfigurovat ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b028d-166">This allows for other resources, including but not limited to web servers to be configured in the same VNET.</span></span>

### <a name="step-1"></a><span data-ttu-id="b028d-167">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b028d-167">Step 1</span></span>

<span data-ttu-id="b028d-168">Proměnné podsítě, která se má použít k vytvoření virtuální podsítě, přiřaďte rozsah adres 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="b028d-168">Assign the address range 10.0.0.0/24 to the subnet variable to be used to create a virtual network.</span></span>  <span data-ttu-id="b028d-169">Tím se vytvoří objekt konfigurace podsítě pro službu Application Gateway, který se používá v dalším příkladu.</span><span class="sxs-lookup"><span data-stu-id="b028d-169">This creates the subnet configuration object for the Application Gateway, which is used in the next example.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a><span data-ttu-id="b028d-170">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b028d-170">Step 2</span></span>

<span data-ttu-id="b028d-171">Vytvořte virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast Západní USA s použitím předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="b028d-171">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span> <span data-ttu-id="b028d-172">Tím dokončíte konfiguraci virtuální sítě s jednu podsíť pro aplikační brány, aby se nacházejí.</span><span class="sxs-lookup"><span data-stu-id="b028d-172">This completes the configuration of the VNET with a single subnet for the Application Gateway to reside.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a><span data-ttu-id="b028d-173">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b028d-173">Step 3</span></span>

<span data-ttu-id="b028d-174">Přiřaďte proměnnou podsítě pro další kroky, to je předán `New-AzureRMApplicationGateway` rutiny v příštím kroku.</span><span class="sxs-lookup"><span data-stu-id="b028d-174">Assign the subnet variable for the next steps, this is passed to the `New-AzureRMApplicationGateway` cmdlet in a future step.</span></span>

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="b028d-175">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="b028d-175">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="b028d-176">Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA.</span><span class="sxs-lookup"><span data-stu-id="b028d-176">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span> <span data-ttu-id="b028d-177">Služba Application Gateway může pro příjem požadavků na vyrovnávání zatížení používat veřejnou IP adresu, interní IP adresu nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="b028d-177">Application Gateway can use a public IP address, internal IP address or both to receive requests for load balancing.</span></span>  <span data-ttu-id="b028d-178">V tomto příkladu se používá jen veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="b028d-178">This example only uses a public IP address.</span></span> <span data-ttu-id="b028d-179">V následujícím příkladu není pro vytvoření veřejné IP adresy nakonfigurován žádný název DNS.</span><span class="sxs-lookup"><span data-stu-id="b028d-179">In the following example, no DNS name is configured for creating the Public IP address.</span></span>  <span data-ttu-id="b028d-180">Služba Application Gateway nepodporuje u veřejných IP adres vlastní názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="b028d-180">Application Gateway does not support custom DNS names on public IP addresses.</span></span>  <span data-ttu-id="b028d-181">Pokud je pro veřejný koncový bod vyžadován vlastní název, měl by být vytvořen záznam CNAME odkazující na automaticky vygenerovaný název DNS pro příslušnou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b028d-181">If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="b028d-182">IP adresa je ke službě Application Gateway přiřazena při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="b028d-182">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="b028d-183">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="b028d-183">Create application gateway configuration</span></span>

<span data-ttu-id="b028d-184">Před vytvořením služby Application Gateway musí být nastaveny všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b028d-184">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="b028d-185">Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-185">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="b028d-186">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b028d-186">Step 1</span></span>

<span data-ttu-id="b028d-187">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="b028d-187">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="b028d-188">Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="b028d-188">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="b028d-189">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b028d-189">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a><span data-ttu-id="b028d-190">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b028d-190">Step 2</span></span>

<span data-ttu-id="b028d-191">Nakonfigurujte fond back-end IP adres s názvem **pool01** a **pool2** s IP adresami pro **pool1** a **pool2**.</span><span class="sxs-lookup"><span data-stu-id="b028d-191">Configure the back-end IP address pool named **pool01** and **pool2** with IP addresses for **pool1** and **pool2**.</span></span> <span data-ttu-id="b028d-192">Tyto IP adresy jsou IP adresy prostředků, které jsou hostiteli webové aplikace, která má být chráněna službou Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-192">These IP addresses are the IP addresses of the resources that are hosting the web application to be protected by the application gateway.</span></span> <span data-ttu-id="b028d-193">Funkčnost všech těchto členů fondu back-end se ověřuje základními nebo vlastními testy.</span><span class="sxs-lookup"><span data-stu-id="b028d-193">These backend pool members are all validated to be healthy by probes whether they are basic probes or custom probes.</span></span>  <span data-ttu-id="b028d-194">Provoz je pak směrován do nich, když služba Application Gateway obdrží požadavky.</span><span class="sxs-lookup"><span data-stu-id="b028d-194">Traffic is then routed to them when requests come into the application gateway.</span></span> <span data-ttu-id="b028d-195">Fondy back-end můžou být využívány více pravidly v rámci služby Application Gateway. To znamená, že jeden fond back-end může být využíván pro více webových aplikacích umístěných ve stejném hostiteli.</span><span class="sxs-lookup"><span data-stu-id="b028d-195">Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

<span data-ttu-id="b028d-196">V tomto příkladu jsou pro směrování síťového provozu na základě cesty URL použity dva fondy back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-196">In this example, there are two back-end pools to route network traffic based on the URL path.</span></span> <span data-ttu-id="b028d-197">Jeden fond přijímá provoz z cesty URL „/video“ a druhý fond přijímá provoz z cesty „/images“.</span><span class="sxs-lookup"><span data-stu-id="b028d-197">One pool receives traffic from URL path "/video" and other pool receive traffic from path "/image".</span></span> <span data-ttu-id="b028d-198">Nahrazením předchozích IP adres přidejte vlastní koncové body IP adres aplikace.</span><span class="sxs-lookup"><span data-stu-id="b028d-198">Replace the preceding IP addresses to add your own application IP address endpoints.</span></span> 

### <a name="step-3"></a><span data-ttu-id="b028d-199">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b028d-199">Step 3</span></span>

<span data-ttu-id="b028d-200">Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-200">Configure application gateway setting **poolsetting01** and **poolsetting02** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="b028d-201">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="b028d-201">In this example, you configure different back-end pool settings for the back-end pools.</span></span> <span data-ttu-id="b028d-202">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-202">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="b028d-203">Nastavení back-endu HTTP jsou využívána pravidly pro směrování provozu do správných členů fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-203">Backend HTTP settings are used by rules to route traffic to the correct backend pool members.</span></span> <span data-ttu-id="b028d-204">Určuje protokol a port, který se používá při odesílání provozu do členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-204">This determines the protocol and port that is used when sending traffic to the backend pool members.</span></span> <span data-ttu-id="b028d-205">Podle nastavení HTTP back-endu se určují i relace založené na souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="b028d-205">Cookie-based sessions are also determined by the backend HTTP settings.</span></span>  <span data-ttu-id="b028d-206">Pokud je tato funkce povolena, spřažení relace založené na souborech cookie odesílá provoz do stejného back-endu jako předchozí požadavky pro jednotlivé pakety.</span><span class="sxs-lookup"><span data-stu-id="b028d-206">If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="b028d-207">Krok 4</span><span class="sxs-lookup"><span data-stu-id="b028d-207">Step 4</span></span>

<span data-ttu-id="b028d-208">Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou.</span><span class="sxs-lookup"><span data-stu-id="b028d-208">Configure the front-end IP with public IP endpoint.</span></span> <span data-ttu-id="b028d-209">Objekt konfigurace IP adresy front-endu je používán naslouchacím procesem k předání veřejně viditelné IP adresy pro naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b028d-209">The front-end IP configuration object is used by a listener to relate the outward facing IP address with the listener.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="b028d-210">Krok 5</span><span class="sxs-lookup"><span data-stu-id="b028d-210">Step 5</span></span>

<span data-ttu-id="b028d-211">Nakonfigurujte front-end port pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-211">Configure the front-end port for an application gateway.</span></span> <span data-ttu-id="b028d-212">Objekt konfigurace portu front-end je používán naslouchacím procesem k definování portu, na kterém služba Application Gateway v naslouchacím procesu naslouchá provozu.</span><span class="sxs-lookup"><span data-stu-id="b028d-212">The front-end port configuration object is used by a listener to define what port the Application Gateway listens for traffic on the listener.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a><span data-ttu-id="b028d-213">Krok 6</span><span class="sxs-lookup"><span data-stu-id="b028d-213">Step 6</span></span>

<span data-ttu-id="b028d-214">Nakonfigurujte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b028d-214">Configure the listener.</span></span> <span data-ttu-id="b028d-215">V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="b028d-215">This step configures the listener for the public IP address and port used to receive incoming network traffic.</span></span> <span data-ttu-id="b028d-216">Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b028d-216">The following example takes the previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures the listener.</span></span> <span data-ttu-id="b028d-217">V tomto příkladu naslouchací proces naslouchá provozu HTTP na portu 80 pro veřejnou IP adresu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b028d-217">In this example, the listener listens to HTTP traffic on port 80 on the public IP address that was created earlier.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a><span data-ttu-id="b028d-218">Krok 7</span><span class="sxs-lookup"><span data-stu-id="b028d-218">Step 7</span></span>

<span data-ttu-id="b028d-219">Konfigurovat pravidla cesty adresy URL pro back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="b028d-219">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="b028d-220">Tento krok nakonfiguruje relativní cesta používaná systémem aplikační brány a definuje mapování mezi cestu adresy URL a fond back-end, která je přiřazena k zpracovávat příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="b028d-220">This step configures the relative path used by application gateway and defines the mapping between the URL path and the back-end pool that is assigned to handle the incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b028d-221">Každá cesta musí začínat znakem / a bude jediným místem "\*" je povolena, je na konci.</span><span class="sxs-lookup"><span data-stu-id="b028d-221">Each path must start with / and the only place a "\*" is allowed, is at the end.</span></span> <span data-ttu-id="b028d-222">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="b028d-222">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="b028d-223">Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="b028d-223">The string fed to the path matcher does not include any text after the first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="b028d-224">Následující příklad vytvoří dvě pravidla: jeden pro "/ image /" cesty směrování provozu na back-end "pool1" a další pro "/ video /" cesty směrování provozu na back-end "pool2."</span><span class="sxs-lookup"><span data-stu-id="b028d-224">The following example creates two rules: one for "/image/" path routing traffic to back-end "pool1" and another one for "/video/" path routing traffic to back-end "pool2."</span></span> <span data-ttu-id="b028d-225">Tato pravidla se ujistěte, že přenosy dat pro každou sadu adresy URL se směruje na back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-225">These rules ensure that traffic for each set of urls is routed to the backend.</span></span> <span data-ttu-id="b028d-226">Například http://contoso.com/image/figure1.jpg přejde na pool1 a http://contoso.com/video/example.mp4 přejde na pool2.</span><span class="sxs-lookup"><span data-stu-id="b028d-226">For example, http://contoso.com/image/figure1.jpg goes to pool1 and http://contoso.com/video/example.mp4 goes to pool2.</span></span>

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

<span data-ttu-id="b028d-227">Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="b028d-227">If the path doesn't match any of the pre-defined path rules, the rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="b028d-228">Například http://contoso.com/shoppingcart/test.html přejde na pool1, jako je definován jako výchozí fond pro neodpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="b028d-228">For example, http://contoso.com/shoppingcart/test.html goes to pool1 as it is defined as the default pool for unmatched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a><span data-ttu-id="b028d-229">Krok 8</span><span class="sxs-lookup"><span data-stu-id="b028d-229">Step 8</span></span>

<span data-ttu-id="b028d-230">Vytvořte nastavení pravidla.</span><span class="sxs-lookup"><span data-stu-id="b028d-230">Create a rule setting.</span></span> <span data-ttu-id="b028d-231">Tento krok konfiguruje službu application gateway používat na základě cestu směrování adres URL.</span><span class="sxs-lookup"><span data-stu-id="b028d-231">This step configures the application gateway to use URL path-based routing.</span></span> <span data-ttu-id="b028d-232">`$urlPathMap` Proměnná definovaná v předchozím kroku se teď používá k vytvoření pravidla na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="b028d-232">The `$urlPathMap` variable defined in the earlier step is now used to create the path-based rule.</span></span> <span data-ttu-id="b028d-233">V tomto kroku jsme pravidlo přidružit naslouchací proces a mapování cesty adresy url vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b028d-233">In this step, we associate the rule with a listener and the url path mapping created earlier.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a><span data-ttu-id="b028d-234">Krok 9</span><span class="sxs-lookup"><span data-stu-id="b028d-234">Step 9</span></span>

<span data-ttu-id="b028d-235">Nakonfigurujte počet instancí a velikost pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-235">Configure the number of instances and size for the application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="b028d-236">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="b028d-236">Create Application Gateway</span></span>

<span data-ttu-id="b028d-237">Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="b028d-237">Create an application gateway with all configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="b028d-238">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b028d-238">Get application gateway DNS name</span></span>

<span data-ttu-id="b028d-239">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="b028d-239">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="b028d-240">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="b028d-240">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="b028d-241">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-241">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="b028d-242">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b028d-242">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="b028d-243">Pokud chcete nakonfigurovat záznam IP CNAME front-endu, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-243">To configure the frontend IP CNAME record, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="b028d-244">Název DNS služby application gateway by měl použít k vytvoření záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="b028d-244">The application gateway's DNS name should be used to create a CNAME record.</span></span> <span data-ttu-id="b028d-245">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b028d-245">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b028d-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b028d-246">Next steps</span></span>

<span data-ttu-id="b028d-247">Pokud chcete získat další přesměrování zpracování Secure Sockets Layer (SSL), přečtěte si téma [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b028d-247">If you want to learn Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-arm.md).</span></span>

