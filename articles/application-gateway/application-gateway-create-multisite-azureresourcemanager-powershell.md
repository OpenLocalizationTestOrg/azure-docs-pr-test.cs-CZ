---
title: "Vytvoření služby application gateway pro hostování více webů | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pro hostování několika webových aplikací na stejnou bránu."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="57f2a-103">Vytvoření služby application gateway pro hostování několika webových aplikací</span><span class="sxs-lookup"><span data-stu-id="57f2a-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="57f2a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="57f2a-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="57f2a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="57f2a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="57f2a-106">Hostování více lokalitu umožňuje nasadit více než jednu webovou aplikaci ve stejném application gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="57f2a-107">Přitom spoléhá na přítomnost Hlavička hostitele v příchozím požadavku HTTP, chcete-li zjistit, který naslouchací proces by přijímat přenosy.</span><span class="sxs-lookup"><span data-stu-id="57f2a-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="57f2a-108">Naslouchací proces pak přesměruje přenosy na příslušné back-end fondu podle konfigurace v definici pravidla brány.</span><span class="sxs-lookup"><span data-stu-id="57f2a-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="57f2a-109">V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na toto rozšíření indikace názvu serveru (SNI) vyberte správné naslouchací proces pro webový provoz.</span><span class="sxs-lookup"><span data-stu-id="57f2a-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="57f2a-110">Běžně používá pro více hostování lokality je načíst vyrovnávat požadavky na jiných webových domén na jiný server back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="57f2a-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="57f2a-111">Více subdomény stejné kořenové domény podobně také může být hostovaný na stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="57f2a-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="57f2a-112">Scénář</span><span class="sxs-lookup"><span data-stu-id="57f2a-112">Scenario</span></span>

<span data-ttu-id="57f2a-113">V následujícím příkladu se Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam.</span><span class="sxs-lookup"><span data-stu-id="57f2a-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="57f2a-114">Podobně jako instalační program může subdomény hostitele jako app.contoso.com a blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="57f2a-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="57f2a-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="57f2a-116">Before you begin</span></span>

1. <span data-ttu-id="57f2a-117">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="57f2a-117">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="57f2a-118">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="57f2a-118">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="57f2a-119">Servery přidané do fondu back-end pro použití služby application gateway, musí existovat nebo mít své koncové body vytvořené, buď ve virtuální síti v jiné podsíti, nebo s veřejné nebo virtuálními IP Adresami přiřazené.</span><span class="sxs-lookup"><span data-stu-id="57f2a-119">The servers added to the back-end pool to use the application gateway must exist or have their endpoints created either in the virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="57f2a-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="57f2a-120">Requirements</span></span>

* <span data-ttu-id="57f2a-121">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="57f2a-121">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="57f2a-122">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="57f2a-122">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="57f2a-123">Můžete také použít plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="57f2a-123">FQDN can also be used.</span></span>
* <span data-ttu-id="57f2a-124">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="57f2a-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="57f2a-125">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-125">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="57f2a-126">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-126">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="57f2a-127">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="57f2a-127">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="57f2a-128">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="57f2a-128">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="57f2a-129">Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.</span><span class="sxs-lookup"><span data-stu-id="57f2a-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="57f2a-130">**Pravidlo:** pravidlo váže naslouchací proces fondu back-end serverů a definuje, kterému fondu back-end serverů provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-130">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="57f2a-131">Pravidla se zpracovávají v pořadí, ve kterém jsou uvedeny a provoz se přesměruje přes první pravidlo, které odpovídá bez ohledu na specifické podobě.</span><span class="sxs-lookup"><span data-stu-id="57f2a-131">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="57f2a-132">Například, pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces obou na stejném portu, musí být uvedené pravidlo s několika lokalitami naslouchací proces před pravidlo základní naslouchací proces, aby pravidlo více lokalit a fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="57f2a-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="57f2a-133">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="57f2a-133">Create an application gateway</span></span>

<span data-ttu-id="57f2a-134">Tady jsou kroky potřebné k vytvoření aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="57f2a-134">The following are the steps needed to create an application gateway:</span></span>

1. <span data-ttu-id="57f2a-135">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="57f2a-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="57f2a-136">Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-136">Create a virtual network, subnets, and public IP for the application gateway.</span></span>
3. <span data-ttu-id="57f2a-137">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="57f2a-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="57f2a-138">Vytvoření prostředku služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="57f2a-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="57f2a-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="57f2a-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="57f2a-140">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="57f2a-140">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="57f2a-141">Další informace najdete v [pomocí prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="57f2a-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="57f2a-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="57f2a-142">Step 1</span></span>

<span data-ttu-id="57f2a-143">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="57f2a-143">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="57f2a-144">Zobrazí se výzva k ověření pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="57f2a-144">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="57f2a-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="57f2a-145">Step 2</span></span>

<span data-ttu-id="57f2a-146">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="57f2a-146">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="57f2a-147">Krok 3</span><span class="sxs-lookup"><span data-stu-id="57f2a-147">Step 3</span></span>

<span data-ttu-id="57f2a-148">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="57f2a-148">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="57f2a-149">Krok 4</span><span class="sxs-lookup"><span data-stu-id="57f2a-149">Step 4</span></span>

<span data-ttu-id="57f2a-150">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="57f2a-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="57f2a-151">Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:</span><span class="sxs-lookup"><span data-stu-id="57f2a-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="57f2a-152">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="57f2a-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="57f2a-153">Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="57f2a-153">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="57f2a-154">Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="57f2a-154">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="57f2a-155">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** s umístění ve **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="57f2a-155">In the example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="57f2a-156">Když potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si část [Vytvořit bránu s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="57f2a-156">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="57f2a-157">Navštivte [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="57f2a-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="57f2a-158">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="57f2a-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="57f2a-159">Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="57f2a-159">The following example shows how to create a virtual network by using Resource Manager.</span></span> <span data-ttu-id="57f2a-160">Dvě podsítě jsou vytvořené v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="57f2a-160">Two subnets are created in this step.</span></span> <span data-ttu-id="57f2a-161">První podsíť je pro službu application gateway sám sebe.</span><span class="sxs-lookup"><span data-stu-id="57f2a-161">The first subnet is for the application gateway itself.</span></span> <span data-ttu-id="57f2a-162">Aplikační brána vyžaduje vlastní podsíti pro uložení její instance.</span><span class="sxs-lookup"><span data-stu-id="57f2a-162">Application gateway requires its own subnet to hold its instances.</span></span> <span data-ttu-id="57f2a-163">Pouze jiné službu application Gateway se dá nasadit v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="57f2a-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="57f2a-164">Druhou podsíť se používá pro uložení aplikace back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="57f2a-164">The second subnet is used to hold the application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="57f2a-165">Krok 1</span><span class="sxs-lookup"><span data-stu-id="57f2a-165">Step 1</span></span>

<span data-ttu-id="57f2a-166">Přiřaďte rozsah adres 10.0.0.0/24 proměnné podsítě, který se má použít pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-166">Assign the address range 10.0.0.0/24 to the subnet variable to be used to hold the application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="57f2a-167">Krok 2</span><span class="sxs-lookup"><span data-stu-id="57f2a-167">Step 2</span></span>

<span data-ttu-id="57f2a-168">Přiřaďte proměnnou Podsíť2 má být použit pro back-endové fondy 10.0.1.0/24 rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="57f2a-168">Assign the address range 10.0.1.0/24 to the subnet2 variable to be used for the backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="57f2a-169">Krok 3</span><span class="sxs-lookup"><span data-stu-id="57f2a-169">Step 3</span></span>

<span data-ttu-id="57f2a-170">Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="57f2a-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="57f2a-171">Krok 4</span><span class="sxs-lookup"><span data-stu-id="57f2a-171">Step 4</span></span>

<span data-ttu-id="57f2a-172">Přiřaďte proměnnou podsítě pro další kroky, které se vytvoří aplikační brána.</span><span class="sxs-lookup"><span data-stu-id="57f2a-172">Assign a subnet variable for the next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="57f2a-173">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="57f2a-173">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="57f2a-174">Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA.</span><span class="sxs-lookup"><span data-stu-id="57f2a-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="57f2a-175">IP adresa je ke službě Application Gateway přiřazena při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="57f2a-175">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="57f2a-176">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="57f2a-176">Create application gateway configuration</span></span>

<span data-ttu-id="57f2a-177">Před vytvořením aplikační brány musíte nastavit všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="57f2a-177">You have to set up all configuration items before creating the application gateway.</span></span> <span data-ttu-id="57f2a-178">Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-178">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="57f2a-179">Krok 1</span><span class="sxs-lookup"><span data-stu-id="57f2a-179">Step 1</span></span>

<span data-ttu-id="57f2a-180">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="57f2a-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="57f2a-181">Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě a směrovat síťový provoz na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="57f2a-181">When application gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="57f2a-182">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="57f2a-183">Krok 2</span><span class="sxs-lookup"><span data-stu-id="57f2a-183">Step 2</span></span>

<span data-ttu-id="57f2a-184">Nakonfigurujte fond back-end IP adres s názvem **pool01** a **pool2** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** pro **pool1** a **134.170.186.46**, **134.170.189.221**, **134.170.186.50** pro **pool2**.</span><span class="sxs-lookup"><span data-stu-id="57f2a-184">Configure the back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="57f2a-185">V tomto příkladu jsou dva back endové fondy ke směrování síťového provozu založené na požadovaný server.</span><span class="sxs-lookup"><span data-stu-id="57f2a-185">In this example, there are two back-end pools to route network traffic based on the requested site.</span></span> <span data-ttu-id="57f2a-186">Jeden fond přijímá provoz z lokality "contoso.com" a dalších fondu přijímá provoz z lokality "fabrikam.com".</span><span class="sxs-lookup"><span data-stu-id="57f2a-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="57f2a-187">Je třeba nahradit předchozí IP adresy, chcete-li přidat vlastní koncové body aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-187">You have to replace the preceding IP addresses to add your own application IP address endpoints.</span></span> <span data-ttu-id="57f2a-188">Místo interní IP adresy můžete pro back-end instance také použít veřejné IP adresy, plně kvalifikovaný název domény nebo síťový adaptér Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="57f2a-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="57f2a-189">Určete plně kvalifikované názvy domény místo IP adresy v prostředí PowerShell pomocí "-BackendFQDNs" parametr.</span><span class="sxs-lookup"><span data-stu-id="57f2a-189">To specify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="57f2a-190">Krok 3</span><span class="sxs-lookup"><span data-stu-id="57f2a-190">Step 3</span></span>

<span data-ttu-id="57f2a-191">Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="57f2a-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="57f2a-192">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="57f2a-192">In this example, you configure different back-end pool settings for the back-end pools.</span></span> <span data-ttu-id="57f2a-193">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="57f2a-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="57f2a-194">Krok 4</span><span class="sxs-lookup"><span data-stu-id="57f2a-194">Step 4</span></span>

<span data-ttu-id="57f2a-195">Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou.</span><span class="sxs-lookup"><span data-stu-id="57f2a-195">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="57f2a-196">Krok 5</span><span class="sxs-lookup"><span data-stu-id="57f2a-196">Step 5</span></span>

<span data-ttu-id="57f2a-197">Nakonfigurujte front-end port pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-197">Configure the front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="57f2a-198">Krok 6</span><span class="sxs-lookup"><span data-stu-id="57f2a-198">Step 6</span></span>

<span data-ttu-id="57f2a-199">Nakonfigurujte dva certifikáty SSL pro dva weby, že budeme podporovat v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-199">Configure two SSL certificates for the two websites we are going to support in this example.</span></span> <span data-ttu-id="57f2a-200">Jeden certifikát je pro provoz contoso.com a druhá je pro provoz fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="57f2a-200">One certificate is for contoso.com traffic and the other is for fabrikam.com traffic.</span></span> <span data-ttu-id="57f2a-201">Tyto certifikáty musí být z certifikační autority, která vydala certifikáty pro vaše weby.</span><span class="sxs-lookup"><span data-stu-id="57f2a-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="57f2a-202">Certifikáty podepsané svým držitelem podporuje, ale nedoporučuje pro produkční provoz.</span><span class="sxs-lookup"><span data-stu-id="57f2a-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="57f2a-203">Krok 7</span><span class="sxs-lookup"><span data-stu-id="57f2a-203">Step 7</span></span>

<span data-ttu-id="57f2a-204">Nakonfigurujte dva naslouchací procesy pro dva weby v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-204">Configure two listeners for the two web sites in this example.</span></span> <span data-ttu-id="57f2a-205">Tento krok nakonfiguruje naslouchací procesy pro veřejné IP adresy, portu a hostitele, na které se používá k přijetí příchozích přenosů.</span><span class="sxs-lookup"><span data-stu-id="57f2a-205">This step configures the listeners for public IP address, port, and host used to receive incoming traffic.</span></span> <span data-ttu-id="57f2a-206">Parametr název hostitele je vyžadovaná pro podporu více lokality a musí být nastavena na příslušné web, pro kterou je provoz přijata.</span><span class="sxs-lookup"><span data-stu-id="57f2a-206">HostName parameter is required for multiple site support and should be set to the appropriate website for which the traffic is received.</span></span> <span data-ttu-id="57f2a-207">RequireServerNameIndication parametr musí být nastavena na hodnotu true pro weby, které potřebují podporu pro protokol SSL ve scénáři více hostitelů.</span><span class="sxs-lookup"><span data-stu-id="57f2a-207">RequireServerNameIndication parameter should be set to true for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="57f2a-208">Pokud se vyžaduje podpora protokolu SSL, musíte taky zadat certifikát SSL, který se používá k zabezpečení přenosů pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57f2a-208">If SSL support is required, you also need to specify the SSL certificate that is used to secure traffic for that web application.</span></span> <span data-ttu-id="57f2a-209">Kombinace FrontendIPConfiguration, FrontendPort a název hostitele musí být jedinečné pro naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="57f2a-209">The combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique to a listener.</span></span> <span data-ttu-id="57f2a-210">Každý naslouchací proces může podporovat jeden certifikát.</span><span class="sxs-lookup"><span data-stu-id="57f2a-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="57f2a-211">Krok 8</span><span class="sxs-lookup"><span data-stu-id="57f2a-211">Step 8</span></span>

<span data-ttu-id="57f2a-212">Vytvořte dvě nastavení pravidla pro dva webové aplikace v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="57f2a-212">Create two rule setting for the two web applications in this example.</span></span> <span data-ttu-id="57f2a-213">Pravidlo sváže společně naslouchací procesy, back-endové fondy a nastavení protokolu http.</span><span class="sxs-lookup"><span data-stu-id="57f2a-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="57f2a-214">Tento krok konfiguruje službu application gateway používat základní pravidlo směrování, jednu pro každý web.</span><span class="sxs-lookup"><span data-stu-id="57f2a-214">This step configures the application gateway to use Basic routing rule, one for each website.</span></span> <span data-ttu-id="57f2a-215">Data na každý web přijme jeho nakonfigurované naslouchací proces a je pak přesměrované na jeho nakonfigurované back-end fondu pomocí vlastnosti uvedené na BackendHttpSettings.</span><span class="sxs-lookup"><span data-stu-id="57f2a-215">Traffic to each website is received by its configured listener, and is then directed to its configured backend pool, using the properties specified in the BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="57f2a-216">Krok 9</span><span class="sxs-lookup"><span data-stu-id="57f2a-216">Step 9</span></span>

<span data-ttu-id="57f2a-217">Nakonfigurujte počet instancí a velikost pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-217">Configure the number of instances and size for the application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="57f2a-218">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="57f2a-218">Create application gateway</span></span>

<span data-ttu-id="57f2a-219">Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="57f2a-219">Create an application gateway with all configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="57f2a-220">Zřizování aplikace brány je dlouhotrvající operace a může trvat delší dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="57f2a-220">Application Gateway provisioning is a long running operation and may take some time to complete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="57f2a-221">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="57f2a-221">Get application gateway DNS name</span></span>

<span data-ttu-id="57f2a-222">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="57f2a-222">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="57f2a-223">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="57f2a-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="57f2a-224">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-224">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="57f2a-225">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="57f2a-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="57f2a-226">Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-226">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="57f2a-227">Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS.</span><span class="sxs-lookup"><span data-stu-id="57f2a-227">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="57f2a-228">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="57f2a-228">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="57f2a-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57f2a-229">Next steps</span></span>

<span data-ttu-id="57f2a-230">Zjistěte, jak chránit své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="57f2a-230">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

