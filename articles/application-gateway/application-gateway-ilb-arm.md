---
title: "aaaUsing Azure Application Gateway s interní nástroj pro vyrovnávání zatížení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfiguraci, spuštění a odstranění služby Azure application gateway s vyrovnávání interní zatížení (ILB) pro Azure Resource Manager"
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
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="48f2a-103">Vytvořte aplikační bránu s interním nástrojem pro vyrovnávání zatížení (ILB) pomocí nástroje Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="48f2a-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="48f2a-104">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="48f2a-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="48f2a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="48f2a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="48f2a-106">Služba Azure Application Gateway lze nastavit straně Internetu VIP nebo s vnitřní koncový bod, který není zveřejněné toohello Internetu, také známé jako koncový bod vyrovnávání (ILB) interní zatížení.</span><span class="sxs-lookup"><span data-stu-id="48f2a-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed toohello Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="48f2a-107">Konfigurace hello brány s ILB je užitečná pro interní-obchodní aplikace, které nejsou zveřejněné toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="48f2a-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications that are not exposed toohello Internet.</span></span> <span data-ttu-id="48f2a-108">Je také užitečné pro služby a vrstvy v rámci vícevrstvé aplikace, které se nacházejí v rámci hranice zabezpečení, který není zveřejněné toohello Internet, ale stále vyžadují kruhového dotazování Načíst distribuční, dlouhodobost relace nebo ukončení Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="48f2a-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed toohello Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="48f2a-109">Tento článek vás provede kroky tooconfigure hello aplikační brány s ILB.</span><span class="sxs-lookup"><span data-stu-id="48f2a-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="48f2a-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="48f2a-110">Before you begin</span></span>

1. <span data-ttu-id="48f2a-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="48f2a-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="48f2a-112">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="48f2a-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="48f2a-113">Vytvořte virtuální síť a podsíť služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="48f2a-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="48f2a-114">Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="48f2a-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="48f2a-115">Aplikační brána musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="48f2a-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="48f2a-116">Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.</span><span class="sxs-lookup"><span data-stu-id="48f2a-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="48f2a-117">Co je požadovaná toocreate služby application gateway?</span><span class="sxs-lookup"><span data-stu-id="48f2a-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="48f2a-118">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="48f2a-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="48f2a-119">Hello uvedené IP adresy by měly buď patřit toohello virtuální sítě, ale v jiné podsíti pro aplikační bránu hello nebo by měly být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="48f2a-119">hello IP addresses listed should either belong toohello virtual network but in a different subnet for hello application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="48f2a-120">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="48f2a-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="48f2a-121">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="48f2a-122">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="48f2a-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="48f2a-123">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="48f2a-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="48f2a-124">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="48f2a-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="48f2a-125">**Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="48f2a-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="48f2a-126">V současné době pouze hello *základní* pravidel je podporována.</span><span class="sxs-lookup"><span data-stu-id="48f2a-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="48f2a-127">Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="48f2a-127">hello *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="48f2a-128">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="48f2a-128">Create an application gateway</span></span>

<span data-ttu-id="48f2a-129">Hello rozdíl mezi použitím Azure Classic a Azure Resource Manager je hello pořadí, ve kterém vytvoříte hello aplikační brány a hello položkách, které toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="48f2a-129">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>
<span data-ttu-id="48f2a-130">S Resource Managerem se všechny položky, které služby application gateway je konfigurovat individuálně a potom se spojí dohromady prostředku aplikační brány toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-130">With Resource Manager, all items that make an application gateway is configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="48f2a-131">Zde jsou hello kroky, které jsou potřebné toocreate aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="48f2a-131">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="48f2a-132">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="48f2a-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="48f2a-133">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="48f2a-133">Create a virtual network and a subnet for hello application gateway</span></span>
3. <span data-ttu-id="48f2a-134">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="48f2a-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="48f2a-135">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="48f2a-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="48f2a-136">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="48f2a-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="48f2a-137">Ujistěte se, že jste přepnuli rutiny Azure Resource Manager prostředí PowerShell režimu toouse hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-137">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="48f2a-138">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="48f2a-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="48f2a-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="48f2a-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="48f2a-140">Krok 2</span><span class="sxs-lookup"><span data-stu-id="48f2a-140">Step 2</span></span>

<span data-ttu-id="48f2a-141">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-141">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="48f2a-142">Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="48f2a-142">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="48f2a-143">Krok 3</span><span class="sxs-lookup"><span data-stu-id="48f2a-143">Step 3</span></span>

<span data-ttu-id="48f2a-144">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="48f2a-144">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="48f2a-145">Krok 4</span><span class="sxs-lookup"><span data-stu-id="48f2a-145">Step 4</span></span>

<span data-ttu-id="48f2a-146">Vytvořte novou skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="48f2a-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="48f2a-147">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="48f2a-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="48f2a-148">To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="48f2a-148">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="48f2a-149">Ujistěte se, že všechny příkazy toocreate používá služby application gateway hello stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="48f2a-149">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="48f2a-150">V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "Appgw-rg" a umístěním "západní USA".</span><span class="sxs-lookup"><span data-stu-id="48f2a-150">In hello preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="48f2a-151">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="48f2a-151">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="48f2a-152">Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="48f2a-152">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="48f2a-153">Krok 1</span><span class="sxs-lookup"><span data-stu-id="48f2a-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="48f2a-154">Tento krok přiřadí hello adresa rozsahu 10.0.0.0/24 tooa podsíť proměnné toobe používá toocreate virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="48f2a-154">This step assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="48f2a-155">Krok 2</span><span class="sxs-lookup"><span data-stu-id="48f2a-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="48f2a-156">Tento krok vytvoří virtuální síť s názvem "appgwvnet" v prostředku skupiny "appgw-rg" pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="48f2a-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="48f2a-157">Krok 3</span><span class="sxs-lookup"><span data-stu-id="48f2a-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="48f2a-158">Tento krok přiřadí hello podsítě objektu toovariable $subnet pro další kroky hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-158">This step assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="48f2a-159">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="48f2a-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="48f2a-160">Krok 1</span><span class="sxs-lookup"><span data-stu-id="48f2a-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="48f2a-161">Tento krok vytvoří konfigurace IP aplikační brány s názvem "gatewayIP01".</span><span class="sxs-lookup"><span data-stu-id="48f2a-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="48f2a-162">Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="48f2a-162">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="48f2a-163">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="48f2a-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="48f2a-164">Krok 2</span><span class="sxs-lookup"><span data-stu-id="48f2a-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="48f2a-165">Tento krok konfiguruje hello back-end IP adres fond s názvem "pool01" s IP adresami "10.1.1.8, 10.1.1.9, 10.1.1.10".</span><span class="sxs-lookup"><span data-stu-id="48f2a-165">This step configures hello back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="48f2a-166">Ty jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP.</span><span class="sxs-lookup"><span data-stu-id="48f2a-166">Those are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="48f2a-167">Nahraďte hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f2a-167">You replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="48f2a-168">Krok 3</span><span class="sxs-lookup"><span data-stu-id="48f2a-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="48f2a-169">Tento krok nakonfiguruje aplikaci brány nastavení "poolsetting01" pro zatížení hello vyrovnáváním síťového provozu ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-169">This step configures application gateway setting "poolsetting01" for hello load balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="48f2a-170">Krok 4</span><span class="sxs-lookup"><span data-stu-id="48f2a-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="48f2a-171">Tento krok nakonfiguruje hello port front-end IP adresy s názvem "frontendport01" pro hello ILB.</span><span class="sxs-lookup"><span data-stu-id="48f2a-171">This step configures hello front-end IP port named "frontendport01" for hello ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="48f2a-172">Krok 5</span><span class="sxs-lookup"><span data-stu-id="48f2a-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="48f2a-173">Tento krok vytvoří hello konfiguraci front-end IP adresy, nazvanou "fipconfig01" a přidruží ji s privátní IP Adresou z aktuální podsítě virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-173">This step creates hello front-end IP configuration called "fipconfig01" and associates it with a private IP from hello current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="48f2a-174">Krok 6</span><span class="sxs-lookup"><span data-stu-id="48f2a-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="48f2a-175">Tento krok vytvoří hello naslouchací proces nazvaný "listener01" a přiřadí hello front-end port toohello konfiguraci front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="48f2a-175">This step creates hello listener called "listener01" and associates hello front-end port toohello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="48f2a-176">Krok 7</span><span class="sxs-lookup"><span data-stu-id="48f2a-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="48f2a-177">Tento krok vytvoří hello pravidlo Vyrovnávání zatížení směrování názvem "rule01", které konfiguruje chování nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-177">This step creates hello load balancer routing rule called "rule01" that configures hello load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="48f2a-178">Krok 8</span><span class="sxs-lookup"><span data-stu-id="48f2a-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="48f2a-179">Tento krok nakonfiguruje velikost instance hello hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="48f2a-179">This step configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="48f2a-180">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="48f2a-180">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="48f2a-181">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="48f2a-181">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="48f2a-182">Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).</span><span class="sxs-lookup"><span data-stu-id="48f2a-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="48f2a-183">Vytvořte aplikační bránu pomocí New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="48f2a-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="48f2a-184">Vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-184">Creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="48f2a-185">V tomto příkladu hello Aplikační brána nazývá "appgwtest".</span><span class="sxs-lookup"><span data-stu-id="48f2a-185">In this example, hello application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="48f2a-186">Tento krok vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-186">This step creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="48f2a-187">V příkladu hello hello Aplikační brána nazývá "appgwtest".</span><span class="sxs-lookup"><span data-stu-id="48f2a-187">In hello example, hello application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="48f2a-188">Odstranění služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="48f2a-188">Delete an application gateway</span></span>

<span data-ttu-id="48f2a-189">toodelete služby application gateway, je třeba toodo hello následující kroky v pořadí:</span><span class="sxs-lookup"><span data-stu-id="48f2a-189">toodelete an application gateway, you need toodo hello following steps in order:</span></span>

1. <span data-ttu-id="48f2a-190">Použití hello `Stop-AzureRmApplicationGateway` rutiny toostop hello brány.</span><span class="sxs-lookup"><span data-stu-id="48f2a-190">Use hello `Stop-AzureRmApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="48f2a-191">Použití hello `Remove-AzureRmApplicationGateway` rutiny tooremove hello brány.</span><span class="sxs-lookup"><span data-stu-id="48f2a-191">Use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="48f2a-192">Ověřte, že hello brány byla odebrána pomocí hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="48f2a-192">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="48f2a-193">Krok 1</span><span class="sxs-lookup"><span data-stu-id="48f2a-193">Step 1</span></span>

<span data-ttu-id="48f2a-194">Získejte hello objektu application gateway a přidružte ji tooa proměnné "$getgw".</span><span class="sxs-lookup"><span data-stu-id="48f2a-194">Get hello application gateway object and associate it tooa variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="48f2a-195">Krok 2</span><span class="sxs-lookup"><span data-stu-id="48f2a-195">Step 2</span></span>

<span data-ttu-id="48f2a-196">Použití `Stop-AzureRmApplicationGateway` toostop hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="48f2a-196">Use `Stop-AzureRmApplicationGateway` toostop hello application gateway.</span></span> <span data-ttu-id="48f2a-197">Tento příklad ukazuje hello `Stop-AzureRmApplicationGateway` na hello prvním řádku, následovanou výstup hello.</span><span class="sxs-lookup"><span data-stu-id="48f2a-197">This sample shows hello `Stop-AzureRmApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

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

<span data-ttu-id="48f2a-198">Jakmile hello Aplikační brána v zastaveném stavu, použijte hello `Remove-AzureRmApplicationGateway` rutiny tooremove hello služby.</span><span class="sxs-lookup"><span data-stu-id="48f2a-198">Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.</span></span>

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
> <span data-ttu-id="48f2a-199">Hello **-force** přepínač může být použité toosuppress hello odebrat potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="48f2a-199">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="48f2a-200">Odebrali jsme tooverify, který hello služby, můžete použít hello `Get-AzureRmApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="48f2a-200">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="48f2a-201">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="48f2a-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="48f2a-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48f2a-202">Next steps</span></span>

<span data-ttu-id="48f2a-203">Pokud chcete tooconfigure přesměrování zpracování SSL, najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="48f2a-203">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="48f2a-204">Pokud chcete, aby tooconfigure toouse aplikace brány s ILB, přečtěte si téma [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="48f2a-204">If you want tooconfigure an application gateway toouse with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="48f2a-205">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="48f2a-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="48f2a-206">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="48f2a-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="48f2a-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="48f2a-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

