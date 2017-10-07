---
title: aaaConfigure end tooend SSL s Azure Application Gateway | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure ukončení tooend SSL s Application Gateway pomocí Azure Resource Manager PowerShell"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a><span data-ttu-id="1d46b-103">Nakonfigurovat koncové tooend SSL Application Gateway pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d46b-103">Configure end tooend SSL with Application Gateway using PowerShell</span></span>

## <a name="overview"></a><span data-ttu-id="1d46b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d46b-104">Overview</span></span>

<span data-ttu-id="1d46b-105">Aplikace podporuje brány ukončení tooend šifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="1d46b-105">Application Gateway supports end tooend encryption of traffic.</span></span> <span data-ttu-id="1d46b-106">Aplikační brána dosahuje tím, že se ukončuje připojení SSL hello na hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-106">Application Gateway does this by terminating hello SSL connection at hello application gateway.</span></span> <span data-ttu-id="1d46b-107">Brána Hello poté použije pravidla směrování hello toohello provoz, znovu je zašifruje hello paketů a předá hello paketu toohello odpovídající back-end na základě pravidel směrování hello definované.</span><span class="sxs-lookup"><span data-stu-id="1d46b-107">hello gateway then applies hello routing rules toohello traffic, re-encrypts hello packet, and forwards hello packet toohello appropriate backend based on hello routing rules defined.</span></span> <span data-ttu-id="1d46b-108">Odpověď od hello webový server, na které se prochází hello stejný proces back toohello koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1d46b-108">Any response from hello web server goes through hello same process back toohello end user.</span></span>

<span data-ttu-id="1d46b-109">Jiné funkce této aplikační brána podporuje, definování vlastních možností protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-109">Another feature that application gateway supports defining custom SSL options.</span></span> <span data-ttu-id="1d46b-110">Aplikační brána podporuje následující verze protokolu; zakázání hello **TLSv1.0**, **TLSv1.1**, a **TLSv1.2** také definování hello, který šifer sady toouse a hello pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="1d46b-110">Application Gateway supports disabling hello following protocol version; **TLSv1.0**, **TLSv1.1**, and **TLSv1.2** as well defining hello which cipher suites toouse and hello order of preference.</span></span>  <span data-ttu-id="1d46b-111">toolearn Další informace o konfigurovatelných možností protokolu SSL, navštivte [zásady protokolu SSL přehled](application-gateway-SSL-policy-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d46b-111">toolearn more about configurable SSL options, visit [SSL Policy overview](application-gateway-SSL-policy-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1d46b-112">Protokol SSL 2.0 a SSL 3.0 jsou ve výchozím nastavení zakázané a nemůže být povolena.</span><span class="sxs-lookup"><span data-stu-id="1d46b-112">SSL 2.0 and SSL 3.0 are disabled by default and cannot be enabled.</span></span> <span data-ttu-id="1d46b-113">Tyto považovány za nezabezpečená a nejsou možné toobe použít s aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-113">They are considered unsecure and are not able toobe used with Application Gateway.</span></span>

![scénář image][scenario]

## <a name="scenario"></a><span data-ttu-id="1d46b-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="1d46b-115">Scenario</span></span>

<span data-ttu-id="1d46b-116">V tomto scénáři zjistíte, jak toocreate brány aplikace pomocí ukončení tooend SSL pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1d46b-116">In this scenario, you learn how toocreate an application gateway using end tooend SSL using PowerShell.</span></span>

<span data-ttu-id="1d46b-117">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="1d46b-117">This scenario will:</span></span>

* <span data-ttu-id="1d46b-118">Vytvořte skupinu prostředků s názvem **appgw-rg**</span><span class="sxs-lookup"><span data-stu-id="1d46b-118">Create a resource group named **appgw-rg**</span></span>
* <span data-ttu-id="1d46b-119">Vytvořit virtuální síť s názvem **appgwvnet** s adresním prostorem 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="1d46b-119">Create a virtual network named **appgwvnet** with an address space of 10.0.0.0/16.</span></span>
* <span data-ttu-id="1d46b-120">Vytvořte dvě podsítě názvem **appgwsubnet** a **appsubnet**.</span><span class="sxs-lookup"><span data-stu-id="1d46b-120">Create two subnets called **appgwsubnet** and **appsubnet**.</span></span>
* <span data-ttu-id="1d46b-121">Vytvořte malá aplikace brány podpůrné tooend SSL šifrování této verze protokoly SSL omezení a šifrovacích sad.</span><span class="sxs-lookup"><span data-stu-id="1d46b-121">Create a small application gateway supporting end tooend SSL encryption that limits SSL protocols versions and cipher suites.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1d46b-122">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1d46b-122">Before you begin</span></span>

<span data-ttu-id="1d46b-123">tooconfigure end tooend SSL s aplikační bránu, vyžaduje se certifikát pro bránu hello a certifikáty se vyžadují pro hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="1d46b-123">tooconfigure end tooend SSL with an application gateway, a certificate is required for hello gateway and certificates are required for hello backend servers.</span></span> <span data-ttu-id="1d46b-124">certifikát brány Hello je použité tooencrypt a dešifrování hello provoz odeslaný tooit pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-124">hello gateway certificate is used tooencrypt and decrypt hello traffic sent tooit using SSL.</span></span> <span data-ttu-id="1d46b-125">certifikát brány Hello musí toobe formát Personal Information Exchange (pfx).</span><span class="sxs-lookup"><span data-stu-id="1d46b-125">hello gateway certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="1d46b-126">Tento formát souboru umožňuje pro hello privátní klíče toobe exportovali, které je požadované hello aplikace brány tooperform hello šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="1d46b-126">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

<span data-ttu-id="1d46b-127">Pro element end tooend SSL šifrování hello back-end musí být seznam povolených adres s aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-127">For end tooend SSL encryption hello backend must be whitelisted with application gateway.</span></span> <span data-ttu-id="1d46b-128">K tomu je potřeba odesílání hello veřejný certifikát hello back-EndY toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-128">This is done by uploading hello public certificate of hello backends toohello application gateway.</span></span> <span data-ttu-id="1d46b-129">Tím se zajistí, že tento aplikační brána hello pouze komunikuje s instancí známé back-end.</span><span class="sxs-lookup"><span data-stu-id="1d46b-129">This ensures that hello application gateway only communicates with known backend instances.</span></span> <span data-ttu-id="1d46b-130">Tato další zabezpečuje komunikaci tooend end hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-130">This further secures hello end tooend communication.</span></span>

<span data-ttu-id="1d46b-131">Tento proces je popsán v hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d46b-131">This process is described in hello following steps:</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="1d46b-132">Vytvoření hello skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1d46b-132">Create hello Resource Group</span></span>

<span data-ttu-id="1d46b-133">Tato část vás provede procesem vytvoření skupiny prostředků, který obsahuje hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-133">This section walks you through creating a resource group, that contains hello application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="1d46b-134">Krok 1</span><span class="sxs-lookup"><span data-stu-id="1d46b-134">Step 1</span></span>

<span data-ttu-id="1d46b-135">Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1d46b-135">Log in tooyour Azure Account.</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="1d46b-136">Krok 2</span><span class="sxs-lookup"><span data-stu-id="1d46b-136">Step 2</span></span>

<span data-ttu-id="1d46b-137">Vyberte hello toouse předplatné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="1d46b-137">Select hello subscription toouse for this scenario.</span></span>

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a><span data-ttu-id="1d46b-138">Krok 3</span><span class="sxs-lookup"><span data-stu-id="1d46b-138">Step 3</span></span>

<span data-ttu-id="1d46b-139">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="1d46b-139">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="1d46b-140">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="1d46b-140">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="1d46b-141">Hello následující příklad vytvoří virtuální síť a dvě podsítě.</span><span class="sxs-lookup"><span data-stu-id="1d46b-141">hello following example creates a virtual network and two subnets.</span></span> <span data-ttu-id="1d46b-142">Jednu podsíť je použité toohold hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-142">One subnet is used toohold hello application gateway.</span></span> <span data-ttu-id="1d46b-143">Hello jiné podsítě se používá pro hello back-EndY hostování hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d46b-143">hello other subnet is used for hello backends hosting hello web application.</span></span>

### <a name="step-1"></a><span data-ttu-id="1d46b-144">Krok 1</span><span class="sxs-lookup"><span data-stu-id="1d46b-144">Step 1</span></span>

<span data-ttu-id="1d46b-145">Přiřaďte rozsah adres pro podsíť hello použít pro hello Application Gateway, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1d46b-145">Assign an address range for hello subnet be used for hello Application Gateway itself.</span></span>

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> <span data-ttu-id="1d46b-146">Podsítě konfigurované pro službu application gateway by měl být správnou velikost.</span><span class="sxs-lookup"><span data-stu-id="1d46b-146">Subnets configured for application gateway should be properly sized.</span></span> <span data-ttu-id="1d46b-147">Aplikační bránu můžete nakonfigurovat pro až too10 instance.</span><span class="sxs-lookup"><span data-stu-id="1d46b-147">An application gateway can be configured for up too10 instances.</span></span> <span data-ttu-id="1d46b-148">Každá instance trvá jednu IP adresu z podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-148">Each instance takes one IP address from hello subnet.</span></span> <span data-ttu-id="1d46b-149">Příliš malé podsítě může nepříznivě ovlivnit škálování služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="1d46b-149">Too small of a subnet can adversely affect scaling out an application gateway.</span></span>
> 
> 

### <a name="step-2"></a><span data-ttu-id="1d46b-150">Krok 2</span><span class="sxs-lookup"><span data-stu-id="1d46b-150">Step 2</span></span>

<span data-ttu-id="1d46b-151">Přiřadíte toobe rozsah adres použitý pro hello back-endových adres.</span><span class="sxs-lookup"><span data-stu-id="1d46b-151">Assign an address range toobe used for hello Backend address pool.</span></span>

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a><span data-ttu-id="1d46b-152">Krok 3</span><span class="sxs-lookup"><span data-stu-id="1d46b-152">Step 3</span></span>

<span data-ttu-id="1d46b-153">Vytvořte virtuální síť s podsítí hello definované v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-153">Create a virtual network with hello subnets defined in hello preceding steps.</span></span>

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a><span data-ttu-id="1d46b-154">Krok 4</span><span class="sxs-lookup"><span data-stu-id="1d46b-154">Step 4</span></span>

<span data-ttu-id="1d46b-155">Načtení hello virtuální sítě prostředků a podsíť prostředky toobe použít v hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d46b-155">Retrieve hello virtual network resource and subnet resources toobe used in hello following steps:</span></span>

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="1d46b-156">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="1d46b-156">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="1d46b-157">Vytvoření veřejné toobe prostředků IP použít pro službu hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="1d46b-157">Create a public IP resource toobe used for hello application gateway.</span></span> <span data-ttu-id="1d46b-158">Tato veřejná IP adresa se používá následující krok.</span><span class="sxs-lookup"><span data-stu-id="1d46b-158">This public IP address is used a following step.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> <span data-ttu-id="1d46b-159">Aplikační brána nepodporuje použití hello veřejnou IP adresu vytvořit s definovaným popiskem domény.</span><span class="sxs-lookup"><span data-stu-id="1d46b-159">Application Gateway does not support hello use of a public IP address created with a domain label defined.</span></span> <span data-ttu-id="1d46b-160">Je podporován pouze veřejné IP adresy s popiskem dynamicky vytvořené domény.</span><span class="sxs-lookup"><span data-stu-id="1d46b-160">Only a public IP address with a dynamically created domain label is supported.</span></span> <span data-ttu-id="1d46b-161">Pokud budete potřebovat dns popisný název hello aplikační bránu, je vhodné toouse záznam CNAME záznam jako alias.</span><span class="sxs-lookup"><span data-stu-id="1d46b-161">If you require a friendly dns name for hello application gateway, it is recommended toouse a CNAME record as an alias.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="1d46b-162">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="1d46b-162">Create an application gateway configuration object</span></span>

<span data-ttu-id="1d46b-163">Všechny položky konfigurace jsou nastavené před vytvořením hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-163">All configuration items are set before creating hello application gateway.</span></span> <span data-ttu-id="1d46b-164">Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-164">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="1d46b-165">Krok 1</span><span class="sxs-lookup"><span data-stu-id="1d46b-165">Step 1</span></span>

<span data-ttu-id="1d46b-166">Vytvoření konfigurace IP aplikační brány, toto nastavení konfiguruje, jaké používá podsíť hello aplikace brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-166">Create an application gateway IP configuration, this setting configures what subnet hello application gateway uses.</span></span> <span data-ttu-id="1d46b-167">Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě hello a směruje síťový provoz toohello IP adresy ve fondu back-end IP hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-167">When application gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="1d46b-168">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1d46b-168">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a><span data-ttu-id="1d46b-169">Krok 2</span><span class="sxs-lookup"><span data-stu-id="1d46b-169">Step 2</span></span>

<span data-ttu-id="1d46b-170">Vytvořte konfiguraci front-end IP adresy, toto nastavení se mapuje privátní nebo veřejné ip adresy toohello front-endu hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-170">Create a front-end IP configuration, this setting maps a private or public ip address toohello front end of hello application gateway.</span></span> <span data-ttu-id="1d46b-171">Následující krok Hello přidruží hello veřejnou IP adresu v předchozím kroku s konfigurací front-end IP adresy hello hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-171">hello following step associates hello public IP address in hello preceding step with hello front-end IP configuration.</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a><span data-ttu-id="1d46b-172">Krok 3</span><span class="sxs-lookup"><span data-stu-id="1d46b-172">Step 3</span></span>

<span data-ttu-id="1d46b-173">Nakonfigurujte hello back-end IP adres fondu IP adres hello hello back-end webových serverů.</span><span class="sxs-lookup"><span data-stu-id="1d46b-173">Configure hello back-end IP address pool with hello IP addresses of hello backend web servers.</span></span> <span data-ttu-id="1d46b-174">Tyto IP adresy jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1d46b-174">These IP addresses are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="1d46b-175">Nahraďte hello následující IP adresy tooadd koncovými body IP adresy vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d46b-175">You replace hello following IP addresses tooadd your own application IP address endpoints.</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> <span data-ttu-id="1d46b-176">Platný plně kvalifikovaný název domény (FQDN) je také platnou hodnotu místo ip adresu pro hello back-end serverů pomocí přepínače - BackendFqdns hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-176">A fully qualified domain name (FQDN) is also a valid value in place of an ip address for hello backend servers by using hello -BackendFqdns switch.</span></span> 

### <a name="step-4"></a><span data-ttu-id="1d46b-177">Krok 4</span><span class="sxs-lookup"><span data-stu-id="1d46b-177">Step 4</span></span>

<span data-ttu-id="1d46b-178">Nakonfigurujte port front-end IP hello pro hello koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1d46b-178">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="1d46b-179">Toto je hello port, který koncoví uživatelé připojit k.</span><span class="sxs-lookup"><span data-stu-id="1d46b-179">This port is hello port that end users connect to.</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a><span data-ttu-id="1d46b-180">Krok 5</span><span class="sxs-lookup"><span data-stu-id="1d46b-180">Step 5</span></span>

<span data-ttu-id="1d46b-181">Nakonfigurujte certifikát hello hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-181">Configure hello certificate for hello application gateway.</span></span> <span data-ttu-id="1d46b-182">Tento certifikát je použité toodecrypt a znovu zašifrovat přenosy hello na hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-182">This certificate is used toodecrypt and re-encrypt hello traffic on hello application gateway.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> <span data-ttu-id="1d46b-183">Tato ukázka nakonfiguruje hello certifikátu používaného pro připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-183">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="1d46b-184">certifikát Hello musí toobe ve formátu .pfx a hello heslo musí být mezi 4 znaky too12.</span><span class="sxs-lookup"><span data-stu-id="1d46b-184">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="1d46b-185">Krok 6</span><span class="sxs-lookup"><span data-stu-id="1d46b-185">Step 6</span></span>

<span data-ttu-id="1d46b-186">Vytvořte naslouchací proces protokolu HTTP hello hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-186">Create hello HTTP listener for hello application gateway.</span></span> <span data-ttu-id="1d46b-187">Přiřaďte hello konfiguraci front-end IP adresy, portu a toouse certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-187">Assign hello front-end ip configuration, port, and SSL certificate toouse.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a><span data-ttu-id="1d46b-188">Krok 7</span><span class="sxs-lookup"><span data-stu-id="1d46b-188">Step 7</span></span>

<span data-ttu-id="1d46b-189">Nahrajte certifikát hello toobe použít na hello SSL s povoleným back-end fondu zdrojů.</span><span class="sxs-lookup"><span data-stu-id="1d46b-189">Upload hello certificate toobe used on hello SSL enabled backend pool resources.</span></span>

> [!NOTE]
> <span data-ttu-id="1d46b-190">získá výchozí kontroly Hello hello veřejný klíč z hello **výchozí** vazby SSL na hello back-end na IP adrese a porovnává hello hodnota veřejného klíče obdrží hodnota toohello veřejného klíče tady zadáte.</span><span class="sxs-lookup"><span data-stu-id="1d46b-190">hello default probe gets hello public key from hello **default** SSL binding on hello back-end's IP address and compares hello public key value it receives toohello public key value you provide here.</span></span> <span data-ttu-id="1d46b-191">Hello načtené veřejný klíč nemusí být nutně hello určené lokality toowhich přenosové toky **Pokud** používáte hlavičky hostitele a SNI na hello back-end.</span><span class="sxs-lookup"><span data-stu-id="1d46b-191">hello retrieved public key may not necessarily be hello intended site toowhich traffic flows **if** you are using host headers and SNI on hello back-end.</span></span> <span data-ttu-id="1d46b-192">Pokud máte pochybnosti, navštivte https://127.0.0.1/ na tooconfirm hello back EndY, který certifikát se používá pro hello **výchozí** vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-192">If in doubt, visit https://127.0.0.1/ on hello back-ends tooconfirm which certificate is used for hello **default** SSL binding.</span></span> <span data-ttu-id="1d46b-193">V této části použijte hello veřejný klíč z tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="1d46b-193">Use hello public key from that request in this section.</span></span> <span data-ttu-id="1d46b-194">Pokud používáte hlavičky hostitele a SNI na vazby HTTPS a neobdržíte odpověď a certifikát z toohttps://127.0.0.1/ požadavek ruční prohlížeče na hello back EndY, musíte vytvořit vazbu výchozí SSL na back EndY hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-194">If you are using host-headers and SNI on HTTPS bindings and you do not receive a response and certificate from a manual browser request toohttps://127.0.0.1/ on hello back-ends, you must set up a default SSL binding on hello back-ends.</span></span> <span data-ttu-id="1d46b-195">Pokud to neuděláte, selhání sondy a hello back-end není povolený.</span><span class="sxs-lookup"><span data-stu-id="1d46b-195">If you do not do so, probes fail and hello back-end is not whitelisted.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> <span data-ttu-id="1d46b-196">zadaný v tomto kroku Hello certifikát by měl být hello veřejný klíč certifikátu pfx hello v back-end hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-196">hello certificate provided in this step should be hello public key of hello pfx cert present on hello backend.</span></span> <span data-ttu-id="1d46b-197">Exportujte certifikátu hello (ne hello kořenový certifikát) nainstalován na back-end server hello. CER formátu a použít ho v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="1d46b-197">Export hello certificate (not hello root certificate) installed on hello backend server in .CER format and use it in this step.</span></span> <span data-ttu-id="1d46b-198">Tento krok povolených programů hello back-end s hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-198">This step whitelists hello backend with hello application gateway.</span></span>

### <a name="step-8"></a><span data-ttu-id="1d46b-199">Krok 8</span><span class="sxs-lookup"><span data-stu-id="1d46b-199">Step 8</span></span>

<span data-ttu-id="1d46b-200">Nakonfigurujte nastavení http back-end hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-200">Configure hello application gateway back-end http settings.</span></span> <span data-ttu-id="1d46b-201">Přiřadíte certifikát hello nahráli v předchozím kroku nastavení http toohello hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-201">Assign hello certificate uploaded in hello preceding step toohello http settings.</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a><span data-ttu-id="1d46b-202">Krok 9</span><span class="sxs-lookup"><span data-stu-id="1d46b-202">Step 9</span></span>

<span data-ttu-id="1d46b-203">Vytvořte směrování pravidlo služby load balancer které konfiguruje chování nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-203">Create a load balancer routing rule that configures hello load balancer behavior.</span></span> <span data-ttu-id="1d46b-204">V tomto příkladu se vytvoří pravidlo základní kruhové dotazování.</span><span class="sxs-lookup"><span data-stu-id="1d46b-204">In this example, a basic round robin rule is created.</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a><span data-ttu-id="1d46b-205">Krok 10</span><span class="sxs-lookup"><span data-stu-id="1d46b-205">Step 10</span></span>

<span data-ttu-id="1d46b-206">Nakonfigurujte velikost instance hello hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-206">Configure hello instance size of hello application gateway.</span></span>  <span data-ttu-id="1d46b-207">jsou k dispozici velikosti Hello **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**.</span><span class="sxs-lookup"><span data-stu-id="1d46b-207">hello available sizes are **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large**.</span></span>  <span data-ttu-id="1d46b-208">Pro kapacitu hello dostupné hodnoty jsou 1 až 10.</span><span class="sxs-lookup"><span data-stu-id="1d46b-208">For capacity, hello available values are 1 through 10.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> <span data-ttu-id="1d46b-209">Počet instancí 1 lze zvolit pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="1d46b-209">An instance count of 1 can be chosen for testing purposes.</span></span> <span data-ttu-id="1d46b-210">Je důležité tooknow, aby všechny instance počet v části dvě instance není předmětem hello SLA a proto nedoporučují.</span><span class="sxs-lookup"><span data-stu-id="1d46b-210">It is important tooknow that any instance count under two instances is not covered by hello SLA and are therefore not recommended.</span></span> <span data-ttu-id="1d46b-211">Malé brány jsou toobe použít pro vývoj testování a ne pro produkční účely.</span><span class="sxs-lookup"><span data-stu-id="1d46b-211">Small gateways are toobe used for dev test and not for production purposes.</span></span>

### <a name="step-11"></a><span data-ttu-id="1d46b-212">Krok 11</span><span class="sxs-lookup"><span data-stu-id="1d46b-212">Step 11</span></span>

<span data-ttu-id="1d46b-213">Nakonfigurujte toobe zásady protokolu SSL hello používá na hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="1d46b-213">Configure hello SSL policy toobe used on hello Application Gateway.</span></span> <span data-ttu-id="1d46b-214">Aplikační brána podporuje hello možnost tooset na minimální verzi pro verze protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-214">Application Gateway supports hello ability tooset a minimum version for SSL protocol versions.</span></span>

<span data-ttu-id="1d46b-215">Hello následující hodnoty jsou seznam verze protokolu, které lze definovat.</span><span class="sxs-lookup"><span data-stu-id="1d46b-215">hello following values are a list of protocol versions that can be defined.</span></span>

* <span data-ttu-id="1d46b-216">**TLSv1_0**</span><span class="sxs-lookup"><span data-stu-id="1d46b-216">**TLSv1_0**</span></span>
* <span data-ttu-id="1d46b-217">**TLSv1_1**</span><span class="sxs-lookup"><span data-stu-id="1d46b-217">**TLSv1_1**</span></span>
* <span data-ttu-id="1d46b-218">**TLSv1_2**</span><span class="sxs-lookup"><span data-stu-id="1d46b-218">**TLSv1_2**</span></span>

<span data-ttu-id="1d46b-219">Nastaví hello minimální protocol verze příliš**TLSv1_2** a umožňuje **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**a **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** pouze.</span><span class="sxs-lookup"><span data-stu-id="1d46b-219">Sets hello minimum protocol version too**TLSv1_2** and enables **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** only.</span></span>

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="1d46b-220">Vytvoření hello Application Gateway</span><span class="sxs-lookup"><span data-stu-id="1d46b-220">Create hello Application Gateway</span></span>

<span data-ttu-id="1d46b-221">Pomocí všechny hello předchozích kroků vytvořte hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="1d46b-221">Using all hello preceding steps, create hello Application Gateway.</span></span> <span data-ttu-id="1d46b-222">Vytvoření Hello hello brány je dlouhotrvající proces.</span><span class="sxs-lookup"><span data-stu-id="1d46b-222">hello creation of hello gateway is a long running process.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a><span data-ttu-id="1d46b-223">Omezit verzí protokolu SSL na existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="1d46b-223">Limit SSL protocol versions on an existing Application Gateway</span></span>

<span data-ttu-id="1d46b-224">Hello předchozí kroky vás vytváření aplikací s end tooend SSL a zakázání určitých verzí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-224">hello preceding steps take you through creating an application with end tooend SSL and disabling certain SSL protocol versions.</span></span> <span data-ttu-id="1d46b-225">Hello následující příklad zakazuje určité zásady protokolu SSL na existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-225">hello following example disables certain SSL policies on an existing application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="1d46b-226">Krok 1</span><span class="sxs-lookup"><span data-stu-id="1d46b-226">Step 1</span></span>

<span data-ttu-id="1d46b-227">Načtěte tooupdate brány aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-227">Retrieve hello application gateway tooupdate.</span></span>

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a><span data-ttu-id="1d46b-228">Krok 2</span><span class="sxs-lookup"><span data-stu-id="1d46b-228">Step 2</span></span>

<span data-ttu-id="1d46b-229">Definujte zásady protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1d46b-229">Define an SSL policy.</span></span> <span data-ttu-id="1d46b-230">V následujícím příkladu hello, TLSv1.0 a TLSv1.1 jsou zakázány a hello šifrovací sady **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** jsou povoleny pouze ty hello.</span><span class="sxs-lookup"><span data-stu-id="1d46b-230">In hello following example, TLSv1.0 and TLSv1.1 are disabled and hello cipher suites **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** are hello only ones allowed.</span></span>

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a><span data-ttu-id="1d46b-231">Krok 3</span><span class="sxs-lookup"><span data-stu-id="1d46b-231">Step 3</span></span>

<span data-ttu-id="1d46b-232">Nakonec aktualizujte hello brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-232">Finally, update hello gateway.</span></span> <span data-ttu-id="1d46b-233">Je důležité toonote, že tento poslední krok je dlouho spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="1d46b-233">It is important toonote that this last step is a long running task.</span></span> <span data-ttu-id="1d46b-234">Až skončíte, end tooend SSL je nakonfigurován na hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-234">When it is done, end tooend SSL is configured on hello application gateway.</span></span>

```powershell
$gw | Set-AzureRmApplicationGateway
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="1d46b-235">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="1d46b-235">Get application gateway DNS name</span></span>

<span data-ttu-id="1d46b-236">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="1d46b-236">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="1d46b-237">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="1d46b-237">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="1d46b-238">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-238">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="1d46b-239">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1d46b-239">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="1d46b-240">toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-240">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="1d46b-241">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="1d46b-241">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="1d46b-242">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="1d46b-242">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1d46b-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d46b-243">Next steps</span></span>

<span data-ttu-id="1d46b-244">Další informace o posílení zabezpečení hello webových aplikací pomocí brány Firewall webových aplikací prostřednictvím brány aplikace navštivte stránky [brány Firewall webových aplikací – přehled](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1d46b-244">Learn about hardening hello security of your web applications with Web Application Firewall through Application Gateway by visiting [Web Application Firewall Overview](application-gateway-webapplicationfirewall-overview.md)</span></span>

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
