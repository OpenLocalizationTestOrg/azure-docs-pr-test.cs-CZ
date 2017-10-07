---
title: "aaaCreate aplikační brány pro hostování více webů | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pro hostování několika webových aplikací na hello stejné bráně."
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
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="4b5c3-103">Vytvoření služby application gateway pro hostování několika webových aplikací</span><span class="sxs-lookup"><span data-stu-id="4b5c3-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b5c3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4b5c3-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="4b5c3-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b5c3-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="4b5c3-106">Hostování více lokality můžete toodeploy více než jednu webovou aplikaci na hello stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="4b5c3-107">Přitom spoléhá na přítomnost Hlavička hostitele v hello příchozí požadavek HTTP, který naslouchací proces by přijímat přenosy toodetermine.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="4b5c3-108">naslouchací proces Hello potom směrovat přenosy tooappropriate back-end fondu podle konfigurace v definici pravidla hello hello brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="4b5c3-109">V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na hello indikace názvu serveru (SNI) rozšíření toochoose hello správné naslouchací proces pro hello webový provoz.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="4b5c3-110">Běžně používá pro více hostování lokality je tooload vyrovnávat požadavky pro fondy jiných webových domén toodifferent back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="4b5c3-111">Podobně jako více subdomény hello stejné kořenové domény může být také hostovaná v hello stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="4b5c3-112">Scénář</span><span class="sxs-lookup"><span data-stu-id="4b5c3-112">Scenario</span></span>

<span data-ttu-id="4b5c3-113">V následujícím příkladu hello, aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="4b5c3-114">Podobně jako instalační program může být subdomény používané toohost jako app.contoso.com a blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="4b5c3-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4b5c3-116">Before you begin</span></span>

1. <span data-ttu-id="4b5c3-117">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-117">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="4b5c3-118">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-118">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4b5c3-119">přidány servery Hello toohello fond back-end toouse hello Aplikační brána musí existovat nebo musí mít své koncové body vytvořené, buď ve virtuální síti hello v jiné podsíti, nebo s veřejné nebo virtuálními IP Adresami přiřazené.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-119">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="4b5c3-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b5c3-120">Requirements</span></span>

* <span data-ttu-id="4b5c3-121">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-121">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="4b5c3-122">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-122">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="4b5c3-123">Můžete také použít plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-123">FQDN can also be used.</span></span>
* <span data-ttu-id="4b5c3-124">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="4b5c3-125">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-125">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="4b5c3-126">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-126">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="4b5c3-127">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-127">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="4b5c3-128">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-128">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="4b5c3-129">Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="4b5c3-130">**Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fondu back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-130">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="4b5c3-131">Pravidla se zpracovávají v pořadí hello, které jsou uvedeny a provoz se přesměruje prostřednictvím hello první pravidlo, které odpovídá bez ohledu na specifické podobě.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-131">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="4b5c3-132">Například pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces i na stejný port, hello pravidlo s hello naslouchací proces hello více lokalit musí být uvedený před hello pravidlo základní naslouchací proces hello-li pravidlo více lokalit toofunction hello jako byl očekáván.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="4b5c3-133">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4b5c3-133">Create an application gateway</span></span>

<span data-ttu-id="4b5c3-134">Následující Hello se, že kroky hello toocreate aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="4b5c3-134">hello following are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="4b5c3-135">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4b5c3-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="4b5c3-136">Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-136">Create a virtual network, subnets, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="4b5c3-137">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4b5c3-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="4b5c3-138">Vytvoření prostředku služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4b5c3-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="4b5c3-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4b5c3-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="4b5c3-140">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-140">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="4b5c3-141">Další informace najdete v [pomocí prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="4b5c3-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b5c3-142">Step 1</span></span>

<span data-ttu-id="4b5c3-143">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="4b5c3-143">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="4b5c3-144">Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-144">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="4b5c3-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b5c3-145">Step 2</span></span>

<span data-ttu-id="4b5c3-146">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-146">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="4b5c3-147">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b5c3-147">Step 3</span></span>

<span data-ttu-id="4b5c3-148">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-148">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="4b5c3-149">Krok 4</span><span class="sxs-lookup"><span data-stu-id="4b5c3-149">Step 4</span></span>

<span data-ttu-id="4b5c3-150">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="4b5c3-151">Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:</span><span class="sxs-lookup"><span data-stu-id="4b5c3-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="4b5c3-152">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="4b5c3-153">Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-153">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="4b5c3-154">Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-154">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="4b5c3-155">V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **appgw-RG** s umístění ve **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-155">In hello example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="4b5c3-156">Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-156">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="4b5c3-157">Navštivte [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="4b5c3-158">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="4b5c3-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="4b5c3-159">Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-159">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="4b5c3-160">Dvě podsítě jsou vytvořené v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-160">Two subnets are created in this step.</span></span> <span data-ttu-id="4b5c3-161">první podsíť Hello je pro službu application gateway hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-161">hello first subnet is for hello application gateway itself.</span></span> <span data-ttu-id="4b5c3-162">Aplikační brána vyžaduje vlastní podsíť toohold její instance.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-162">Application gateway requires its own subnet toohold its instances.</span></span> <span data-ttu-id="4b5c3-163">Pouze jiné službu application Gateway se dá nasadit v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="4b5c3-164">druhou podsíť Hello je použité toohold hello aplikace back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-164">hello second subnet is used toohold hello application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="4b5c3-165">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b5c3-165">Step 1</span></span>

<span data-ttu-id="4b5c3-166">Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe použité toohold hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-166">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toohold hello application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="4b5c3-167">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b5c3-167">Step 2</span></span>

<span data-ttu-id="4b5c3-168">Přiřaďte hello adresa rozsahu 10.0.1.0/24 toohello Podsíť2 proměnné toobe používá pro hello back-endové fondy.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-168">Assign hello address range 10.0.1.0/24 toohello subnet2 variable toobe used for hello backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="4b5c3-169">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b5c3-169">Step 3</span></span>

<span data-ttu-id="4b5c3-170">Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="4b5c3-171">Krok 4</span><span class="sxs-lookup"><span data-stu-id="4b5c3-171">Step 4</span></span>

<span data-ttu-id="4b5c3-172">Přiřaďte proměnnou podsítě pro hello další kroky, které se vytvoří aplikační brána.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-172">Assign a subnet variable for hello next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="4b5c3-173">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="4b5c3-173">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="4b5c3-174">Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="4b5c3-175">IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-175">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="4b5c3-176">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="4b5c3-176">Create application gateway configuration</span></span>

<span data-ttu-id="4b5c3-177">Před vytvořením brány aplikace hello máte tooset až všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-177">You have tooset up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="4b5c3-178">Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-178">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="4b5c3-179">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b5c3-179">Step 1</span></span>

<span data-ttu-id="4b5c3-180">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="4b5c3-181">Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-181">When application gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="4b5c3-182">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="4b5c3-183">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b5c3-183">Step 2</span></span>

<span data-ttu-id="4b5c3-184">Konfigurace služby fond hello back-end IP adresy s názvem **pool01** a **pool2** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** pro **pool1** a **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  pro **pool2**.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-184">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="4b5c3-185">V tomto příkladu jsou dva back endové fondy tooroute síťového provozu založené na požadovaný server hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-185">In this example, there are two back-end pools tooroute network traffic based on hello requested site.</span></span> <span data-ttu-id="4b5c3-186">Jeden fond přijímá provoz z lokality "contoso.com" a dalších fondu přijímá provoz z lokality "fabrikam.com".</span><span class="sxs-lookup"><span data-stu-id="4b5c3-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="4b5c3-187">Máte tooreplace hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-187">You have tooreplace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> <span data-ttu-id="4b5c3-188">Místo interní IP adresy můžete pro back-end instance také použít veřejné IP adresy, plně kvalifikovaný název domény nebo síťový adaptér Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="4b5c3-189">toospecify plně kvalifikované názvy domény místo IP adresy v prostředí PowerShell pomocí "-BackendFQDNs" parametr.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-189">toospecify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="4b5c3-190">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b5c3-190">Step 3</span></span>

<span data-ttu-id="4b5c3-191">Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="4b5c3-192">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro hello back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-192">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="4b5c3-193">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="4b5c3-194">Krok 4</span><span class="sxs-lookup"><span data-stu-id="4b5c3-194">Step 4</span></span>

<span data-ttu-id="4b5c3-195">Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-195">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="4b5c3-196">Krok 5</span><span class="sxs-lookup"><span data-stu-id="4b5c3-196">Step 5</span></span>

<span data-ttu-id="4b5c3-197">Nakonfigurujte port front-end hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-197">Configure hello front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="4b5c3-198">Krok 6</span><span class="sxs-lookup"><span data-stu-id="4b5c3-198">Step 6</span></span>

<span data-ttu-id="4b5c3-199">Konfigurovat dva certifikáty SSL pro hello dva weby přidáme toosupport v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-199">Configure two SSL certificates for hello two websites we are going toosupport in this example.</span></span> <span data-ttu-id="4b5c3-200">Jeden certifikát je pro provoz contoso.com a hello jiné pro provoz fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-200">One certificate is for contoso.com traffic and hello other is for fabrikam.com traffic.</span></span> <span data-ttu-id="4b5c3-201">Tyto certifikáty musí být z certifikační autority, která vydala certifikáty pro vaše weby.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="4b5c3-202">Certifikáty podepsané svým držitelem podporuje, ale nedoporučuje pro produkční provoz.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="4b5c3-203">Krok 7</span><span class="sxs-lookup"><span data-stu-id="4b5c3-203">Step 7</span></span>

<span data-ttu-id="4b5c3-204">V tomto příkladu nakonfigurujte dva naslouchací procesy pro hello dva webové servery.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-204">Configure two listeners for hello two web sites in this example.</span></span> <span data-ttu-id="4b5c3-205">Tento krok nakonfiguruje hello naslouchací procesy pro veřejné IP adresy, portu a hostitele použít tooreceive příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-205">This step configures hello listeners for public IP address, port, and host used tooreceive incoming traffic.</span></span> <span data-ttu-id="4b5c3-206">Parametr název hostitele je vyžadovaná pro podporu více lokality a musí být sada toohello odpovídající web, pro které hello příjem přenosů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-206">HostName parameter is required for multiple site support and should be set toohello appropriate website for which hello traffic is received.</span></span> <span data-ttu-id="4b5c3-207">Parametr RequireServerNameIndication je třeba nastavit tootrue pro weby, které potřebují podporu pro protokol SSL ve scénáři více hostitelů.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-207">RequireServerNameIndication parameter should be set tootrue for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="4b5c3-208">Pokud se vyžaduje podpora protokolu SSL, musíte taky toospecify hello SSL certifikát to znamená provoz toosecure použité pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-208">If SSL support is required, you also need toospecify hello SSL certificate that is used toosecure traffic for that web application.</span></span> <span data-ttu-id="4b5c3-209">kombinace Hello FrontendIPConfiguration, FrontendPort a název hostitele musí být jedinečný tooa naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-209">hello combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique tooa listener.</span></span> <span data-ttu-id="4b5c3-210">Každý naslouchací proces může podporovat jeden certifikát.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="4b5c3-211">Krok 8</span><span class="sxs-lookup"><span data-stu-id="4b5c3-211">Step 8</span></span>

<span data-ttu-id="4b5c3-212">Vytvořte dvě pravidla nastavení pro hello dva webové aplikace v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-212">Create two rule setting for hello two web applications in this example.</span></span> <span data-ttu-id="4b5c3-213">Pravidlo sváže společně naslouchací procesy, back-endové fondy a nastavení protokolu http.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="4b5c3-214">Tento krok nakonfiguruje hello aplikace brány toouse základní pravidlo směrování, jednu pro každý web.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-214">This step configures hello application gateway toouse Basic routing rule, one for each website.</span></span> <span data-ttu-id="4b5c3-215">Provoz webu tooeach přijme jeho nakonfigurované naslouchací proces a je pak přesměrované tooits nakonfigurovat fond back-end, pomocí zadané v hello BackendHttpSettings vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-215">Traffic tooeach website is received by its configured listener, and is then directed tooits configured backend pool, using hello properties specified in hello BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="4b5c3-216">Krok 9</span><span class="sxs-lookup"><span data-stu-id="4b5c3-216">Step 9</span></span>

<span data-ttu-id="4b5c3-217">Nakonfigurujte hello počet instancí a velikost hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-217">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="4b5c3-218">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="4b5c3-218">Create application gateway</span></span>

<span data-ttu-id="4b5c3-219">Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-219">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="4b5c3-220">Zřizování brány aplikace je dlouhotrvající operace a některé toocomplete dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-220">Application Gateway provisioning is a long running operation and may take some time toocomplete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="4b5c3-221">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4b5c3-221">Get application gateway DNS name</span></span>

<span data-ttu-id="4b5c3-222">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-222">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="4b5c3-223">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="4b5c3-224">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-224">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="4b5c3-225">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4b5c3-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="4b5c3-226">toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-226">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="4b5c3-227">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-227">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="4b5c3-228">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b5c3-228">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4b5c3-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b5c3-229">Next steps</span></span>

<span data-ttu-id="4b5c3-230">Zjistěte, jak tooprotect své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4b5c3-230">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

