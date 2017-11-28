---
title: "aaaConfigure SSL snižování zátěže prostředí PowerShell – Azure Application Gateway - | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate přesměrování zpracování úloh služby application gateway pomocí protokolu SSL pomocí Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a><span data-ttu-id="05867-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="05867-103">Configure an application gateway for SSL offload by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="05867-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="05867-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="05867-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="05867-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="05867-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="05867-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="05867-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="05867-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="05867-108">Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy.</span><span class="sxs-lookup"><span data-stu-id="05867-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="05867-109">Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="05867-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="05867-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="05867-110">Before you begin</span></span>

1. <span data-ttu-id="05867-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="05867-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="05867-112">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="05867-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="05867-113">Vytvoříte virtuální síť a podsíť pro aplikační bránu hello.</span><span class="sxs-lookup"><span data-stu-id="05867-113">You create a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="05867-114">Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="05867-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="05867-115">Aplikační brána musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="05867-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="05867-116">Hello servery nakonfigurujete toouse hello Aplikační brána musí existovat nebo mít své koncové body vytvořené ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.</span><span class="sxs-lookup"><span data-stu-id="05867-116">hello servers you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="05867-117">Co je požadovaná toocreate služby application gateway?</span><span class="sxs-lookup"><span data-stu-id="05867-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="05867-118">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="05867-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="05867-119">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="05867-119">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="05867-120">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="05867-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="05867-121">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="05867-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="05867-122">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="05867-123">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="05867-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="05867-124">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tato nastavení jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="05867-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="05867-125">**Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="05867-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="05867-126">V současné době pouze hello *základní* pravidel je podporována.</span><span class="sxs-lookup"><span data-stu-id="05867-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="05867-127">Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="05867-127">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="05867-128">**Další poznámky ke konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="05867-128">**Additional configuration notes**</span></span>

<span data-ttu-id="05867-129">Pro konfiguraci certifikátů SSL, hello protokol v **HttpListener** by se měl změnit příliš*Https* (malá a velká písmena).</span><span class="sxs-lookup"><span data-stu-id="05867-129">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="05867-130">Hello **SslCertificate** prvek přidán příliš**HttpListener** s hello hodnotou proměnné nakonfigurovanou pro certifikát SSL hello.</span><span class="sxs-lookup"><span data-stu-id="05867-130">hello **SslCertificate** element is added too**HttpListener** with hello variable value configured for hello SSL certificate.</span></span> <span data-ttu-id="05867-131">Hello front-end port musí být aktualizované too443.</span><span class="sxs-lookup"><span data-stu-id="05867-131">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="05867-132">**spřažení na základě souborů cookie tooenable**: služby application gateway může být nakonfigurované tooensure, zda je žádost o od klientské relace vždy směrovanou toohello stejný virtuální počítač v hello webové farmy.</span><span class="sxs-lookup"><span data-stu-id="05867-132">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="05867-133">Tento scénář se provádí injektáží souboru cookie relace, které umožňuje přenos toodirect hello brány správně.</span><span class="sxs-lookup"><span data-stu-id="05867-133">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="05867-134">Nastavení spřažení na základě souborů cookie tooenable **CookieBasedAffinity** příliš*povoleno* v hello **BackendHttpSettings** element.</span><span class="sxs-lookup"><span data-stu-id="05867-134">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="05867-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="05867-135">Create an application gateway</span></span>

<span data-ttu-id="05867-136">Hello rozdíl mezi použitím modelu nasazení Azure Classic hello a Azure Resource Manager je hello pořadí, ve kterém vytvoříte brány a hello položky aplikace vyžadující toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="05867-136">hello difference between using hello Azure Classic deployment model and Azure Resource Manager is hello order that you create an application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="05867-137">S Resource Managerem se všechny součásti služby application gateway se konfigurovat individuálně a potom se spojí dohromady toocreate prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-137">With Resource Manager, all components of an application gateway are configured individually and then put together toocreate an application gateway resource.</span></span>

<span data-ttu-id="05867-138">Zde jsou kroky potřebné toocreate hello aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="05867-138">Here are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="05867-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="05867-139">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="05867-140">Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro bránu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="05867-140">Create virtual network, subnet, and public IP for hello application gateway</span></span>
3. <span data-ttu-id="05867-141">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="05867-141">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="05867-142">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="05867-142">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="05867-143">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="05867-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="05867-144">Ujistěte se, že jste přepnuli rutiny Azure Resource Manager prostředí PowerShell režimu toouse hello.</span><span class="sxs-lookup"><span data-stu-id="05867-144">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="05867-145">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="05867-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="05867-146">Krok 1</span><span class="sxs-lookup"><span data-stu-id="05867-146">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="05867-147">Krok 2</span><span class="sxs-lookup"><span data-stu-id="05867-147">Step 2</span></span>

<span data-ttu-id="05867-148">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="05867-148">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="05867-149">Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="05867-149">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="05867-150">Krok 3</span><span class="sxs-lookup"><span data-stu-id="05867-150">Step 3</span></span>

<span data-ttu-id="05867-151">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="05867-151">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="05867-152">Krok 4</span><span class="sxs-lookup"><span data-stu-id="05867-152">Step 4</span></span>

<span data-ttu-id="05867-153">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="05867-153">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="05867-154">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="05867-154">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="05867-155">Toto nastavení se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="05867-155">This setting is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="05867-156">Ujistěte se, že všechny příkazy toocreate používá služby application gateway hello stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="05867-156">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="05867-157">V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístění **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="05867-157">In hello example above, we created a resource group called **appgw-RG** and location **West US**.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="05867-158">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="05867-158">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="05867-159">Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="05867-159">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="05867-160">Krok 1</span><span class="sxs-lookup"><span data-stu-id="05867-160">Step 1</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="05867-161">Tato ukázka přiřadí hello adresa rozsahu 10.0.0.0/24 tooa podsíť proměnné toobe používá toocreate virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="05867-161">This sample assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="05867-162">Krok 2</span><span class="sxs-lookup"><span data-stu-id="05867-162">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

<span data-ttu-id="05867-163">Tato ukázka vytvoří virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="05867-163">This sample creates a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="05867-164">Krok 3</span><span class="sxs-lookup"><span data-stu-id="05867-164">Step 3</span></span>

```powershell
$subnet = $vnet.Subnets[0]
```

<span data-ttu-id="05867-165">Tato ukázka přiřadí hello podsítě objektu toovariable $subnet pro další kroky hello.</span><span class="sxs-lookup"><span data-stu-id="05867-165">This sample assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="05867-166">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="05867-166">Create a public IP address for hello front-end configuration</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="05867-167">Tato ukázka vytvoří prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="05867-167">This sample creates a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="05867-168">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="05867-168">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="05867-169">Krok 1</span><span class="sxs-lookup"><span data-stu-id="05867-169">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="05867-170">Tato ukázka vytvoří konfigurace IP aplikační brány s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="05867-170">This sample creates an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="05867-171">Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="05867-171">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="05867-172">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="05867-172">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="05867-173">Krok 2</span><span class="sxs-lookup"><span data-stu-id="05867-173">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

<span data-ttu-id="05867-174">Tato ukázka konfiguruje hello back-end IP adres fond s názvem **pool01** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** .</span><span class="sxs-lookup"><span data-stu-id="05867-174">This sample configures hello back-end IP address pool named **pool01** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span></span> <span data-ttu-id="05867-175">Tyto hodnoty jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="05867-175">Those values are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="05867-176">Nahraďte hello IP adresy z hello předcházející příklad s hello IP adresy koncových bodů vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="05867-176">Replace hello IP addresses from hello preceding example with hello IP addresses of your web application endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="05867-177">Krok 3</span><span class="sxs-lookup"><span data-stu-id="05867-177">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

<span data-ttu-id="05867-178">Tato ukázka nakonfiguruje nastavení brány aplikace **poolsetting01** tooload vyrovnáváním síťového provozu ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="05867-178">This sample configures application gateway setting **poolsetting01** tooload-balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="05867-179">Krok 4</span><span class="sxs-lookup"><span data-stu-id="05867-179">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

<span data-ttu-id="05867-180">Tato ukázka nakonfiguruje hello port front-end IP s názvem **frontendport01** pro hello koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="05867-180">This sample configures hello front-end IP port named **frontendport01** for hello public IP endpoint.</span></span>

### <a name="step-5"></a><span data-ttu-id="05867-181">Krok 5</span><span class="sxs-lookup"><span data-stu-id="05867-181">Step 5</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

<span data-ttu-id="05867-182">Tato ukázka nakonfiguruje hello certifikátu používaného pro připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="05867-182">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="05867-183">certifikát Hello musí toobe ve formátu .pfx a hello heslo musí být mezi 4 znaky too12.</span><span class="sxs-lookup"><span data-stu-id="05867-183">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="05867-184">Krok 6</span><span class="sxs-lookup"><span data-stu-id="05867-184">Step 6</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

<span data-ttu-id="05867-185">Tato ukázka vytvoří hello konfiguraci front-end IP adresy s názvem **fipconfig01** a partnerů hello veřejnou IP adresu s konfigurací front-end IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="05867-185">This sample creates hello front-end IP configuration named **fipconfig01** and associates hello public IP address with hello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="05867-186">Krok 7</span><span class="sxs-lookup"><span data-stu-id="05867-186">Step 7</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

<span data-ttu-id="05867-187">Tato ukázka vytvoří název naslouchacího procesu hello **listener01** a partnerů hello front-end port toohello front-endové konfigurace protokolu IP a certifikát.</span><span class="sxs-lookup"><span data-stu-id="05867-187">This sample creates hello listener name **listener01** and associates hello front-end port toohello front-end IP configuration and certificate.</span></span>

### <a name="step-8"></a><span data-ttu-id="05867-188">Krok 8</span><span class="sxs-lookup"><span data-stu-id="05867-188">Step 8</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="05867-189">Tato ukázka vytvoří hello směrování se pravidlo Vyrovnávání zatížení s názvem **rule01** které konfiguruje chování nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="05867-189">This sample creates hello load balancer routing rule named **rule01** that configures hello load balancer behavior.</span></span>

### <a name="step-9"></a><span data-ttu-id="05867-190">Krok 9</span><span class="sxs-lookup"><span data-stu-id="05867-190">Step 9</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="05867-191">Tato ukázka se nakonfiguruje velikost instance hello hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-191">This sample configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="05867-192">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="05867-192">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="05867-193">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="05867-193">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="05867-194">Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).</span><span class="sxs-lookup"><span data-stu-id="05867-194">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

### <a name="step-10"></a><span data-ttu-id="05867-195">Krok 10</span><span class="sxs-lookup"><span data-stu-id="05867-195">Step 10</span></span>

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

<span data-ttu-id="05867-196">Tento krok definuje hello SSL zásad toouse ve hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="05867-196">This step defines hello SSL policy toouse on hello application gateway.</span></span> <span data-ttu-id="05867-197">Navštivte [verze zásad konfigurace protokolu SSL a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="05867-197">Visit [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn more.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="05867-198">Vytvořte aplikační bránu pomocí New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="05867-198">Create an application gateway by using New-AzureApplicationGateway</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

<span data-ttu-id="05867-199">Tato ukázka se vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="05867-199">This sample creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="05867-200">V příkladu hello hello Aplikační brána nazývá **appgwtest**.</span><span class="sxs-lookup"><span data-stu-id="05867-200">In hello example, hello application gateway is called **appgwtest**.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="05867-201">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="05867-201">Get application gateway DNS name</span></span>

<span data-ttu-id="05867-202">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="05867-202">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="05867-203">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="05867-203">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="05867-204">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-204">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="05867-205">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="05867-205">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="05867-206">toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-206">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="05867-207">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="05867-207">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="05867-208">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="05867-208">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="05867-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05867-209">Next steps</span></span>

<span data-ttu-id="05867-210">Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání interní zatížení (ILB), najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="05867-210">If you want tooconfigure an application gateway toouse with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="05867-211">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="05867-211">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="05867-212">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="05867-212">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="05867-213">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="05867-213">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

