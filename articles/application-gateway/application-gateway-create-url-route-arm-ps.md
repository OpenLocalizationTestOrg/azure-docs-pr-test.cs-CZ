---
title: "aaaCreate pravidla služby application gateway pomocí směrování adres URL | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pomocí pravidel směrování adres URL"
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
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a><span data-ttu-id="a15e8-103">Vytvoření služby application gateway pomocí směrování na základě cesty</span><span class="sxs-lookup"><span data-stu-id="a15e8-103">Create an application gateway using Path-based routing</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a15e8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a15e8-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="a15e8-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="a15e8-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="a15e8-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a15e8-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="a15e8-107">Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http.</span><span class="sxs-lookup"><span data-stu-id="a15e8-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="a15e8-108">Zkontroluje, jestli je fond back-end tooa trasy nakonfigurované pro adresu URL hello uvedené v hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a15e8-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway.</span></span> <span data-ttu-id="a15e8-109">Pak odešle hello síťový provoz toohello definovanými fond back-end.</span><span class="sxs-lookup"><span data-stu-id="a15e8-109">It then sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="a15e8-110">Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.</span><span class="sxs-lookup"><span data-stu-id="a15e8-110">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="a15e8-111">Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla.</span><span class="sxs-lookup"><span data-stu-id="a15e8-111">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="a15e8-112">Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="a15e8-112">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="a15e8-113">Typ základní pravidlo poskytuje kruhového dotazování služby pro hello back-end fondy při PathBasedRouting kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru hello back-endový fond.</span><span class="sxs-lookup"><span data-stu-id="a15e8-113">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="a15e8-114">Scénář</span><span class="sxs-lookup"><span data-stu-id="a15e8-114">Scenario</span></span>

<span data-ttu-id="a15e8-115">V následujícím příkladu hello, Application Gateway obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: fondu video serverů a fond serverů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a15e8-115">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: video server pool and image server pool.</span></span>

<span data-ttu-id="a15e8-116">Požadavky pro http://contoso.com/image * směrují fondu serverů tooimage (pool1) a http://contoso.com/video * směrují fondu serverů toovideo (pool2).</span><span class="sxs-lookup"><span data-stu-id="a15e8-116">Requests for http://contoso.com/image* are routed tooimage server pool (pool1), and http://contoso.com/video* are routed toovideo server pool (pool2).</span></span> <span data-ttu-id="a15e8-117">Pokud cesta vzory hello neodpovídají, bude vybrán výchozí fond serverů (pool1).</span><span class="sxs-lookup"><span data-stu-id="a15e8-117">if none of hello path patterns match, a default server pool (pool1) is selected.</span></span>

![Adresa URL trasy](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a><span data-ttu-id="a15e8-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a15e8-119">Before you begin</span></span>

1. <span data-ttu-id="a15e8-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="a15e8-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="a15e8-121">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a15e8-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="a15e8-122">Vytvoříte virtuální síť a podsíť pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-122">You create a virtual network and subnet for Application Gateway.</span></span> <span data-ttu-id="a15e8-123">Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="a15e8-123">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="a15e8-124">Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a15e8-124">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="a15e8-125">přidány servery Hello toohello fond back-end toouse hello Aplikační brána musí existovat nebo musí mít své koncové body vytvořené ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.</span><span class="sxs-lookup"><span data-stu-id="a15e8-125">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="a15e8-126">Co je požadovaná toocreate služby application gateway?</span><span class="sxs-lookup"><span data-stu-id="a15e8-126">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="a15e8-127">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="a15e8-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="a15e8-128">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="a15e8-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="a15e8-129">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="a15e8-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="a15e8-130">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="a15e8-131">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="a15e8-132">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="a15e8-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="a15e8-133">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="a15e8-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="a15e8-134">**Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-134">**Rule:** hello rule binds hello listener, hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="a15e8-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a15e8-135">Create an application gateway</span></span>

<span data-ttu-id="a15e8-136">Hello rozdíl mezi použitím Azure Classic a Azure Resource Manager je hello pořadí, ve kterém vytvoříte hello aplikační brány a hello položkách, které toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="a15e8-136">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="a15e8-137">S Resource Managerem se všechny položky, které služby application gateway se konfigurovat individuálně a potom se spojí dohromady prostředku aplikační brány toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-137">With Resource Manager, all items that make an application gateway are configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="a15e8-138">Zde jsou hello kroky, které jsou potřebné toocreate aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="a15e8-138">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="a15e8-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a15e8-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="a15e8-140">Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-140">Create a virtual network, subnet, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="a15e8-141">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a15e8-141">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="a15e8-142">Vytvoření prostředku služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a15e8-142">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="a15e8-143">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a15e8-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="a15e8-144">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-144">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="a15e8-145">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a15e8-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="a15e8-146">Krok 1</span><span class="sxs-lookup"><span data-stu-id="a15e8-146">Step 1</span></span>

<span data-ttu-id="a15e8-147">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="a15e8-147">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a15e8-148">Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a15e8-148">You are prompted tooauthenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="a15e8-149">Krok 2</span><span class="sxs-lookup"><span data-stu-id="a15e8-149">Step 2</span></span>

<span data-ttu-id="a15e8-150">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-150">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="a15e8-151">Krok 3</span><span class="sxs-lookup"><span data-stu-id="a15e8-151">Step 3</span></span>

<span data-ttu-id="a15e8-152">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="a15e8-152">Choose which of your Azure subscriptions toouse.</span></span> <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="a15e8-153">Krok 4</span><span class="sxs-lookup"><span data-stu-id="a15e8-153">Step 4</span></span>

<span data-ttu-id="a15e8-154">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="a15e8-154">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

<span data-ttu-id="a15e8-155">Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:</span><span class="sxs-lookup"><span data-stu-id="a15e8-155">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

<span data-ttu-id="a15e8-156">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="a15e8-156">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a15e8-157">Tato skupina prostředků se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a15e8-157">This resource group is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="a15e8-158">Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a15e8-158">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="a15e8-159">V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "appgw-RG" a umístěním "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="a15e8-159">In hello example above, we created a resource group called "appgw-RG" and location "West US".</span></span>

> [!NOTE]
> <span data-ttu-id="a15e8-160">Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a15e8-160">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="a15e8-161">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a15e8-161">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="a15e8-162">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="a15e8-162">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="a15e8-163">Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a15e8-163">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="a15e8-164">Tento příklad vytvoří virtuální síť pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a15e8-164">This example creates a VNET for hello Application Gateway.</span></span> <span data-ttu-id="a15e8-165">Aplikační brána vyžaduje vlastní podsíti, z tohoto důvodu je menší než hello adresní prostor sítě VNET podsíť hello vytvořená pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a15e8-165">Application Gateway requires its own subnet, for this reason hello subnet created for hello Application Gateway is smaller than hello VNET address space.</span></span> <span data-ttu-id="a15e8-166">To umožňuje, aby jiné prostředky, včetně, ale toobe mimo jiné tooweb servery konfigurované v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a15e8-166">This allows for other resources, including but not limited tooweb servers toobe configured in hello same VNET.</span></span>

### <a name="step-1"></a><span data-ttu-id="a15e8-167">Krok 1</span><span class="sxs-lookup"><span data-stu-id="a15e8-167">Step 1</span></span>

<span data-ttu-id="a15e8-168">Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe používá toocreate virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a15e8-168">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toocreate a virtual network.</span></span>  <span data-ttu-id="a15e8-169">Tím se vytvoří objekt konfigurace hello podsíť pro aplikační brány, který se používá v dalším příkladu hello hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-169">This creates hello subnet configuration object for hello Application Gateway, which is used in hello next example.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a><span data-ttu-id="a15e8-170">Krok 2</span><span class="sxs-lookup"><span data-stu-id="a15e8-170">Step 2</span></span>

<span data-ttu-id="a15e8-171">Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="a15e8-171">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span> <span data-ttu-id="a15e8-172">Tím dokončíte konfiguraci hello hello virtuální síť s jednu podsíť pro aplikační bránu tooreside hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-172">This completes hello configuration of hello VNET with a single subnet for hello Application Gateway tooreside.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a><span data-ttu-id="a15e8-173">Krok 3</span><span class="sxs-lookup"><span data-stu-id="a15e8-173">Step 3</span></span>

<span data-ttu-id="a15e8-174">Přiřaďte proměnnou podsítě hello pro hello další kroky, to je předán toohello `New-AzureRMApplicationGateway` rutiny v příštím kroku.</span><span class="sxs-lookup"><span data-stu-id="a15e8-174">Assign hello subnet variable for hello next steps, this is passed toohello `New-AzureRMApplicationGateway` cmdlet in a future step.</span></span>

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="a15e8-175">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="a15e8-175">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="a15e8-176">Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-176">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="a15e8-177">Aplikační bránu můžete použít veřejnou IP adresu, interní IP adresu nebo oba tooreceive požadavky pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a15e8-177">Application Gateway can use a public IP address, internal IP address or both tooreceive requests for load balancing.</span></span>  <span data-ttu-id="a15e8-178">V tomto příkladu se používá jen veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="a15e8-178">This example only uses a public IP address.</span></span> <span data-ttu-id="a15e8-179">V následujícím příkladu hello nakonfigurovaný žádný název DNS pro vytvoření hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-179">In hello following example, no DNS name is configured for creating hello Public IP address.</span></span>  <span data-ttu-id="a15e8-180">Služba Application Gateway nepodporuje u veřejných IP adres vlastní názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="a15e8-180">Application Gateway does not support custom DNS names on public IP addresses.</span></span>  <span data-ttu-id="a15e8-181">Pokud je požadován pro veřejný koncový bod hello vlastní název, by se vytvořit název DNS toopoint toohello automaticky generovány pro veřejnou IP adresu hello záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="a15e8-181">If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="a15e8-182">IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-182">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="a15e8-183">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="a15e8-183">Create application gateway configuration</span></span>

<span data-ttu-id="a15e8-184">Všechny položky konfigurace, musí ho nastavit před vytvořením hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-184">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="a15e8-185">Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-185">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="a15e8-186">Krok 1</span><span class="sxs-lookup"><span data-stu-id="a15e8-186">Step 1</span></span>

<span data-ttu-id="a15e8-187">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="a15e8-187">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="a15e8-188">Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="a15e8-188">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="a15e8-189">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-189">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a><span data-ttu-id="a15e8-190">Krok 2</span><span class="sxs-lookup"><span data-stu-id="a15e8-190">Step 2</span></span>

<span data-ttu-id="a15e8-191">Konfigurace služby fond hello back-end IP adresy s názvem **pool01** a **pool2** s IP adresami pro **pool1** a **pool2**.</span><span class="sxs-lookup"><span data-stu-id="a15e8-191">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses for **pool1** and **pool2**.</span></span> <span data-ttu-id="a15e8-192">Tyto IP adresy jsou IP adresy hello hello prostředků, které jsou hostiteli hello webové aplikace toobe chráněn hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-192">These IP addresses are hello IP addresses of hello resources that are hosting hello web application toobe protected by hello application gateway.</span></span> <span data-ttu-id="a15e8-193">Tito členové fondu back-end jsou všechny ověřené toobe podle sondy v pořádku, ať už sondy základní nebo vlastní testy paměti.</span><span class="sxs-lookup"><span data-stu-id="a15e8-193">These backend pool members are all validated toobe healthy by probes whether they are basic probes or custom probes.</span></span>  <span data-ttu-id="a15e8-194">Provoz se směruje pak toothem při žádosti o začalo hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-194">Traffic is then routed toothem when requests come into hello application gateway.</span></span> <span data-ttu-id="a15e8-195">Back-endové fondy mohou být využívána více pravidel v rámci hello aplikační bránu, což znamená, jeden fond back-end by mohly být použity několika webových aplikací, které jsou umístěné na hello stejné hostitele.</span><span class="sxs-lookup"><span data-stu-id="a15e8-195">Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

<span data-ttu-id="a15e8-196">V tomto příkladu jsou dva back endové fondy tooroute síťový provoz na základě cesty adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-196">In this example, there are two back-end pools tooroute network traffic based on hello URL path.</span></span> <span data-ttu-id="a15e8-197">Jeden fond přijímá provoz z cesty URL „/video“ a druhý fond přijímá provoz z cesty „/images“.</span><span class="sxs-lookup"><span data-stu-id="a15e8-197">One pool receives traffic from URL path "/video" and other pool receive traffic from path "/image".</span></span> <span data-ttu-id="a15e8-198">Nahraďte hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a15e8-198">Replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> 

### <a name="step-3"></a><span data-ttu-id="a15e8-199">Krok 3</span><span class="sxs-lookup"><span data-stu-id="a15e8-199">Step 3</span></span>

<span data-ttu-id="a15e8-200">Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-200">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="a15e8-201">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro hello back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="a15e8-201">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="a15e8-202">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a15e8-202">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="a15e8-203">Nastavení HTTP back-end jsou používány pravidla tooroute provoz toohello správné back-end členy fondu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-203">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="a15e8-204">Určuje hello protokol a port, který se používá při odesílání provozu toohello členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a15e8-204">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="a15e8-205">Na základě souborů cookie relací jsou určeny také nastavení HTTP back-end hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-205">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="a15e8-206">Pokud je povoleno, spřažení relace na základě souborů cookie odešle provoz toohello stejnou back-end jako předchozí požadavky jednotlivých paketů.</span><span class="sxs-lookup"><span data-stu-id="a15e8-206">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="a15e8-207">Krok 4</span><span class="sxs-lookup"><span data-stu-id="a15e8-207">Step 4</span></span>

<span data-ttu-id="a15e8-208">Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a15e8-208">Configure hello front-end IP with public IP endpoint.</span></span> <span data-ttu-id="a15e8-209">objekt konfigurace IP front-endu Hello je používán hello toorelate naslouchacího procesu vzdálené čelí IP adresu naslouchacího procesu hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-209">hello front-end IP configuration object is used by a listener toorelate hello outward facing IP address with hello listener.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="a15e8-210">Krok 5</span><span class="sxs-lookup"><span data-stu-id="a15e8-210">Step 5</span></span>

<span data-ttu-id="a15e8-211">Nakonfigurujte port front-end hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-211">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="a15e8-212">objekt konfigurace Hello front-end port je používán toodefine naslouchací proces port hello Application Gateway naslouchá provoz na hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="a15e8-212">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a><span data-ttu-id="a15e8-213">Krok 6</span><span class="sxs-lookup"><span data-stu-id="a15e8-213">Step 6</span></span>

<span data-ttu-id="a15e8-214">Nakonfigurujte hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="a15e8-214">Configure hello listener.</span></span> <span data-ttu-id="a15e8-215">Tento krok nakonfiguruje hello naslouchací proces pro hello veřejnou IP adresu a port používá tooreceive příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="a15e8-215">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="a15e8-216">Následující ukázka Hello trvá hello dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="a15e8-216">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="a15e8-217">V tomto příkladu naslouchá naslouchací proces hello tooHTTP přenosy na portu 80 na hello veřejnou IP adresu, která jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a15e8-217">In this example, hello listener listens tooHTTP traffic on port 80 on hello public IP address that was created earlier.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a><span data-ttu-id="a15e8-218">Krok 7</span><span class="sxs-lookup"><span data-stu-id="a15e8-218">Step 7</span></span>

<span data-ttu-id="a15e8-219">Konfigurovat pravidla cesty adresy URL pro hello back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="a15e8-219">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="a15e8-220">Tento krok nakonfiguruje hello relativní cesta používaná systémem aplikační brány a definuje hello mapování mezi hello cestu adresy URL a hello fond back-end, který je přiřazen toohandle hello příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="a15e8-220">This step configures hello relative path used by application gateway and defines hello mapping between hello URL path and hello back-end pool that is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a15e8-221">Každá cesta musí začínat znakem / a jediným místem hello "\*" je povolena, je na konci hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-221">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="a15e8-222">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="a15e8-222">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="a15e8-223">Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve "?" nebo "#" a tyto znaky nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="a15e8-223">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="a15e8-224">Hello následující příklad vytvoří dvě pravidla: jeden pro "/ image /" cesty směrování provozu tooback-end "pool1" a další pro "/ video či" cestu směrování provozu tooback-end "pool2."</span><span class="sxs-lookup"><span data-stu-id="a15e8-224">hello following example creates two rules: one for "/image/" path routing traffic tooback-end "pool1" and another one for "/video/" path routing traffic tooback-end "pool2."</span></span> <span data-ttu-id="a15e8-225">Tato pravidla zajistěte, aby byl provoz pro každou sadu adres URL směrované toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="a15e8-225">These rules ensure that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="a15e8-226">Například http://contoso.com/image/figure1.jpg přejde toopool1 a http://contoso.com/video/example.mp4 přejde toopool2.</span><span class="sxs-lookup"><span data-stu-id="a15e8-226">For example, http://contoso.com/image/figure1.jpg goes toopool1 and http://contoso.com/video/example.mp4 goes toopool2.</span></span>

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

<span data-ttu-id="a15e8-227">Pokud cesta hello neodpovídá žádné z pravidel hello předem definovaná cesta, hello pravidla cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="a15e8-227">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="a15e8-228">Například http://contoso.com/shoppingcart/test.html přejde toopool1, jako je definována jako hello výchozí fond pro neodpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="a15e8-228">For example, http://contoso.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a><span data-ttu-id="a15e8-229">Krok 8</span><span class="sxs-lookup"><span data-stu-id="a15e8-229">Step 8</span></span>

<span data-ttu-id="a15e8-230">Vytvořte nastavení pravidla.</span><span class="sxs-lookup"><span data-stu-id="a15e8-230">Create a rule setting.</span></span> <span data-ttu-id="a15e8-231">Tento krok nakonfiguruje hello aplikace brány toouse směrování adres URL na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="a15e8-231">This step configures hello application gateway toouse URL path-based routing.</span></span> <span data-ttu-id="a15e8-232">Hello `$urlPathMap` proměnná definovaná v hello z předchozích kroků je nyní použité toocreate hello na základě cesty pravidlo.</span><span class="sxs-lookup"><span data-stu-id="a15e8-232">hello `$urlPathMap` variable defined in hello earlier step is now used toocreate hello path-based rule.</span></span> <span data-ttu-id="a15e8-233">V tomto kroku jsme přidružit hello pravidlo naslouchací proces a mapování cesty adresy url hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a15e8-233">In this step, we associate hello rule with a listener and hello url path mapping created earlier.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a><span data-ttu-id="a15e8-234">Krok 9</span><span class="sxs-lookup"><span data-stu-id="a15e8-234">Step 9</span></span>

<span data-ttu-id="a15e8-235">Nakonfigurujte hello počet instancí a velikost hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-235">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="a15e8-236">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="a15e8-236">Create Application Gateway</span></span>

<span data-ttu-id="a15e8-237">Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="a15e8-237">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="a15e8-238">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a15e8-238">Get application gateway DNS name</span></span>

<span data-ttu-id="a15e8-239">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="a15e8-239">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="a15e8-240">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="a15e8-240">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="a15e8-241">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-241">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="a15e8-242">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a15e8-242">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="a15e8-243">tooconfigure hello front-endu záznam IP CNAME načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-243">tooconfigure hello frontend IP CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="a15e8-244">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="a15e8-244">hello application gateway's DNS name should be used toocreate a CNAME record.</span></span> <span data-ttu-id="a15e8-245">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a15e8-245">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a15e8-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a15e8-246">Next steps</span></span>

<span data-ttu-id="a15e8-247">Pokud chcete toolearn přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a15e8-247">If you want toolearn Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-arm.md).</span></span>

