---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tahle stránka obsahuje informace k vytvoření aplikační brány s přesměrováním zpracování SSL pomocí Azure Resource Manageru"
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
ms.openlocfilehash: ededabc7c665d6bb05b91e4d21d01fb1379add32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a><span data-ttu-id="ee831-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ee831-103">Configure an application gateway for SSL offload by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ee831-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ee831-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="ee831-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee831-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="ee831-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee831-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="ee831-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ee831-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="ee831-108">Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="ee831-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="ee831-109">Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ee831-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ee831-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ee831-110">Before you begin</span></span>

1. <span data-ttu-id="ee831-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="ee831-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="ee831-112">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ee831-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="ee831-113">Vytvoříte virtuální síť a podsíť pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-113">You create a virtual network and a subnet for the application gateway.</span></span> <span data-ttu-id="ee831-114">Ujistěte se, že tuto podsíť nepoužívají žádné virtuální počítače ani cloudová nasazení.</span><span class="sxs-lookup"><span data-stu-id="ee831-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="ee831-115">Aplikační brána musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ee831-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="ee831-116">Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.</span><span class="sxs-lookup"><span data-stu-id="ee831-116">The servers you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="ee831-117">Co je potřeba k vytvoření služby Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="ee831-117">What is required to create an application gateway?</span></span>

* <span data-ttu-id="ee831-118">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="ee831-118">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="ee831-119">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="ee831-119">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="ee831-120">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="ee831-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="ee831-121">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="ee831-121">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="ee831-122">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-122">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="ee831-123">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="ee831-123">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="ee831-124">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, tato nastavení rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="ee831-124">**Listener:** The listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="ee831-125">**Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="ee831-125">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="ee831-126">V tuhle chvíli se podporuje jenom *základní* pravidlo.</span><span class="sxs-lookup"><span data-stu-id="ee831-126">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="ee831-127">*Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="ee831-127">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="ee831-128">**Další poznámky ke konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="ee831-128">**Additional configuration notes**</span></span>

<span data-ttu-id="ee831-129">Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen).</span><span class="sxs-lookup"><span data-stu-id="ee831-129">For SSL certificates configuration, the protocol in **HttpListener** should change to *Https* (case sensitive).</span></span> <span data-ttu-id="ee831-130">Element **SslCertificate** se přidá do **HttpListener** s hodnotou proměnné nakonfigurovanou pro certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="ee831-130">The **SslCertificate** element is added to **HttpListener** with the variable value configured for the SSL certificate.</span></span> <span data-ttu-id="ee831-131">Front-end port se musí aktualizovat na hodnotu 443.</span><span class="sxs-lookup"><span data-stu-id="ee831-131">The front-end port should be updated to 443.</span></span>

<span data-ttu-id="ee831-132">**Když chcete povolit spřažení na základě souborů cookie**: aplikační brána se může nakonfigurovat tak, aby se žádost od klientské relace vždy směrovala na stejný virtuální počítač v prostředí webové serverové farmy.</span><span class="sxs-lookup"><span data-stu-id="ee831-132">**To enable cookie-based affinity**: An application gateway can be configured to ensure that a request from a client session is always directed to the same VM in the web farm.</span></span> <span data-ttu-id="ee831-133">Takový scénář se provádí injektáží souboru cookie relace, který bráně umožňuje řídit provoz odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ee831-133">This scenario is done by injection of a session cookie that allows the gateway to direct traffic appropriately.</span></span> <span data-ttu-id="ee831-134">Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.</span><span class="sxs-lookup"><span data-stu-id="ee831-134">To enable cookie-based affinity, set **CookieBasedAffinity** to *Enabled* in the **BackendHttpSettings** element.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="ee831-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ee831-135">Create an application gateway</span></span>

<span data-ttu-id="ee831-136">Rozdíl mezi použitím modelu nasazení Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte aplikační bránu, a v položkách, které konfigurujete.</span><span class="sxs-lookup"><span data-stu-id="ee831-136">The difference between using the Azure Classic deployment model and Azure Resource Manager is the order that you create an application gateway and the items that need to be configured.</span></span>

<span data-ttu-id="ee831-137">S Resource Managerem se všechny komponenty služby Application Gateway konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-137">With Resource Manager, all components of an application gateway are configured individually and then put together to create an application gateway resource.</span></span>

<span data-ttu-id="ee831-138">Tady jsou kroky, které se musí udělat k vytvoření aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="ee831-138">Here are the steps needed to create an application gateway:</span></span>

1. <span data-ttu-id="ee831-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ee831-139">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="ee831-140">Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="ee831-140">Create virtual network, subnet, and public IP for the application gateway</span></span>
3. <span data-ttu-id="ee831-141">Vytvoření objektu konfigurace služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ee831-141">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="ee831-142">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="ee831-142">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="ee831-143">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ee831-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="ee831-144">Ujistěte se, že jste přepnuli režim prostředí PowerShell tak, aby se mohly použít rutiny Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ee831-144">Make sure that you switch PowerShell mode to use the Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="ee831-145">Další informace jsou k dispozici v části [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ee831-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="ee831-146">Krok 1</span><span class="sxs-lookup"><span data-stu-id="ee831-146">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="ee831-147">Krok 2</span><span class="sxs-lookup"><span data-stu-id="ee831-147">Step 2</span></span>

<span data-ttu-id="ee831-148">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="ee831-148">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="ee831-149">Zobrazí se výzva k ověření pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ee831-149">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="ee831-150">Krok 3</span><span class="sxs-lookup"><span data-stu-id="ee831-150">Step 3</span></span>

<span data-ttu-id="ee831-151">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ee831-151">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="ee831-152">Krok 4</span><span class="sxs-lookup"><span data-stu-id="ee831-152">Step 4</span></span>

<span data-ttu-id="ee831-153">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="ee831-153">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="ee831-154">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="ee831-154">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="ee831-155">Toto nastavení slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="ee831-155">This setting is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="ee831-156">Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ee831-156">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="ee831-157">V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístěním **Západní USA**.</span><span class="sxs-lookup"><span data-stu-id="ee831-157">In the example above, we created a resource group called **appgw-RG** and location **West US**.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="ee831-158">Vytvoření virtuální sítě a podsítě pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ee831-158">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="ee831-159">Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:</span><span class="sxs-lookup"><span data-stu-id="ee831-159">The following example shows how to create a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="ee831-160">Krok 1</span><span class="sxs-lookup"><span data-stu-id="ee831-160">Step 1</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="ee831-161">Tento vzorový kód přiřadí proměnné podsítě rozsah adres 10.0.0.0/24, který se použije k vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ee831-161">This sample assigns the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="ee831-162">Krok 2</span><span class="sxs-lookup"><span data-stu-id="ee831-162">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

<span data-ttu-id="ee831-163">Tato ukázka vytvoří virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="ee831-163">This sample creates a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="ee831-164">Krok 3</span><span class="sxs-lookup"><span data-stu-id="ee831-164">Step 3</span></span>

```powershell
$subnet = $vnet.Subnets[0]
```

<span data-ttu-id="ee831-165">Tímto vzorovým kódem se přiřadí objekt podsítě k proměnné $subnet pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="ee831-165">This sample assigns the subnet object to variable $subnet for the next steps.</span></span>

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="ee831-166">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="ee831-166">Create a public IP address for the front-end configuration</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="ee831-167">Tato ukázka vytvoří prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA.</span><span class="sxs-lookup"><span data-stu-id="ee831-167">This sample creates a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="ee831-168">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="ee831-168">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="ee831-169">Krok 1</span><span class="sxs-lookup"><span data-stu-id="ee831-169">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="ee831-170">Tato ukázka vytvoří konfigurace IP aplikační brány s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="ee831-170">This sample creates an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="ee831-171">Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="ee831-171">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="ee831-172">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ee831-172">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="ee831-173">Krok 2</span><span class="sxs-lookup"><span data-stu-id="ee831-173">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

<span data-ttu-id="ee831-174">Tato ukázka konfiguruje fond back-end IP adres s názvem **pool01** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span><span class="sxs-lookup"><span data-stu-id="ee831-174">This sample configures the back-end IP address pool named **pool01** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span></span> <span data-ttu-id="ee831-175">Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ee831-175">Those values are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="ee831-176">Nahraďte IP adresy z předchozího příkladu IP adresami koncových bodů vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee831-176">Replace the IP addresses from the preceding example with the IP addresses of your web application endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="ee831-177">Krok 3</span><span class="sxs-lookup"><span data-stu-id="ee831-177">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

<span data-ttu-id="ee831-178">Tato ukázka nakonfiguruje nastavení brány aplikace **poolsetting01** síťový provoz s vyrovnáváním zatížení ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="ee831-178">This sample configures application gateway setting **poolsetting01** to load-balanced network traffic in the back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="ee831-179">Krok 4</span><span class="sxs-lookup"><span data-stu-id="ee831-179">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

<span data-ttu-id="ee831-180">Tato ukázka se nakonfiguruje port front-end IP s názvem **frontendport01** pro koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ee831-180">This sample configures the front-end IP port named **frontendport01** for the public IP endpoint.</span></span>

### <a name="step-5"></a><span data-ttu-id="ee831-181">Krok 5</span><span class="sxs-lookup"><span data-stu-id="ee831-181">Step 5</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

<span data-ttu-id="ee831-182">Tímto vzorovým kódem se nakonfiguruje certifikát používaný pro připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="ee831-182">This sample configures the certificate used for SSL connection.</span></span> <span data-ttu-id="ee831-183">Certifikát musí být ve formátu .pfx, heslo musí mít 4 až 12 znaků.</span><span class="sxs-lookup"><span data-stu-id="ee831-183">The certificate needs to be in .pfx format, and the password must be between 4 to 12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="ee831-184">Krok 6</span><span class="sxs-lookup"><span data-stu-id="ee831-184">Step 6</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

<span data-ttu-id="ee831-185">Tato ukázka vytvoří konfiguraci front-end IP adresy s názvem **fipconfig01** a přidruží veřejnou IP adresu s konfigurací front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ee831-185">This sample creates the front-end IP configuration named **fipconfig01** and associates the public IP address with the front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="ee831-186">Krok 7</span><span class="sxs-lookup"><span data-stu-id="ee831-186">Step 7</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

<span data-ttu-id="ee831-187">Tato ukázka vytvoří název naslouchacího procesu **listener01** a přiřadí front-end port ke konfiguraci front-end IP adresy a certifikát.</span><span class="sxs-lookup"><span data-stu-id="ee831-187">This sample creates the listener name **listener01** and associates the front-end port to the front-end IP configuration and certificate.</span></span>

### <a name="step-8"></a><span data-ttu-id="ee831-188">Krok 8</span><span class="sxs-lookup"><span data-stu-id="ee831-188">Step 8</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="ee831-189">Tento příklad vytvoří pravidlo směrování pro vyrovnávání zatížení s názvem **rule01** které konfiguruje chování nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ee831-189">This sample creates the load balancer routing rule named **rule01** that configures the load balancer behavior.</span></span>

### <a name="step-9"></a><span data-ttu-id="ee831-190">Krok 9</span><span class="sxs-lookup"><span data-stu-id="ee831-190">Step 9</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="ee831-191">Tímto vzorovým kódem se nakonfiguruje velikost instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="ee831-191">This sample configures the instance size of the application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="ee831-192">Výchozí hodnota pro *InstanceCount* je 2 s maximální hodnotou 10.</span><span class="sxs-lookup"><span data-stu-id="ee831-192">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="ee831-193">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="ee831-193">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="ee831-194">Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).</span><span class="sxs-lookup"><span data-stu-id="ee831-194">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

### <a name="step-10"></a><span data-ttu-id="ee831-195">Krok 10</span><span class="sxs-lookup"><span data-stu-id="ee831-195">Step 10</span></span>

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

<span data-ttu-id="ee831-196">Tento krok definuje zásady protokolu SSL pro použití ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-196">This step defines the SSL policy to use on the application gateway.</span></span> <span data-ttu-id="ee831-197">Navštivte [verze zásad konfigurace protokolu SSL a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ee831-197">Visit [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) to learn more.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="ee831-198">Vytvořte aplikační bránu pomocí New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="ee831-198">Create an application gateway by using New-AzureApplicationGateway</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

<span data-ttu-id="ee831-199">Tenhle vzorový kód vytvoří službu Application Gateway se všemi položkami konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="ee831-199">This sample creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="ee831-200">V příkladu se Aplikační brána nazývá **appgwtest**.</span><span class="sxs-lookup"><span data-stu-id="ee831-200">In the example, the application gateway is called **appgwtest**.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="ee831-201">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ee831-201">Get application gateway DNS name</span></span>

<span data-ttu-id="ee831-202">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="ee831-202">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="ee831-203">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="ee831-203">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="ee831-204">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-204">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="ee831-205">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ee831-205">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="ee831-206">Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-206">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="ee831-207">Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS.</span><span class="sxs-lookup"><span data-stu-id="ee831-207">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="ee831-208">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="ee831-208">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="ee831-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee831-209">Next steps</span></span>

<span data-ttu-id="ee831-210">Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení (ILB), přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="ee831-210">If you want to configure an application gateway to use with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="ee831-211">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="ee831-211">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="ee831-212">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="ee831-212">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="ee831-213">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="ee831-213">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

