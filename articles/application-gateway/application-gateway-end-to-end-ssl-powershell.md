---
title: "Konfigurace protokolu SSL koncová s Azure Application Gateway | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat Application Gateway pomocí Azure Resource Manager PowerShell koncové SSL"
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
ms.openlocfilehash: 6d969d6a0c649c263e1d5bb99bdbceec484cb9a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-end-to-end-ssl-with-application-gateway-using-powershell"></a><span data-ttu-id="357c9-103">Konfigurace protokolu SSL koncová s Application Gateway pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="357c9-103">Configure end to end SSL with Application Gateway using PowerShell</span></span>

## <a name="overview"></a><span data-ttu-id="357c9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="357c9-104">Overview</span></span>

<span data-ttu-id="357c9-105">Aplikační brána podporuje koncová šifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="357c9-105">Application Gateway supports end to end encryption of traffic.</span></span> <span data-ttu-id="357c9-106">Služba Application Gateway to provádí ukončením připojení protokolem SSL ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-106">Application Gateway does this by terminating the SSL connection at the application gateway.</span></span> <span data-ttu-id="357c9-107">Brána následně použije na provoz pravidla směrování, znovu zašifruje paket a předá tento paket do příslušného back-endu na základě nadefinovaných pravidel směrování.</span><span class="sxs-lookup"><span data-stu-id="357c9-107">The gateway then applies the routing rules to the traffic, re-encrypts the packet, and forwards the packet to the appropriate backend based on the routing rules defined.</span></span> <span data-ttu-id="357c9-108">Každá odpověď webového serveru prochází ke koncovému uživateli stejným procesem.</span><span class="sxs-lookup"><span data-stu-id="357c9-108">Any response from the web server goes through the same process back to the end user.</span></span>

<span data-ttu-id="357c9-109">Jiné funkce této aplikační brána podporuje, definování vlastních možností protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-109">Another feature that application gateway supports defining custom SSL options.</span></span> <span data-ttu-id="357c9-110">Aplikační brána podporuje, zakázání následující verze protokolu; **TLSv1.0**, **TLSv1.1**, a **TLSv1.2** jako dobře definující která sady mají být použity v pořadí podle preference šifer.</span><span class="sxs-lookup"><span data-stu-id="357c9-110">Application Gateway supports disabling the following protocol version; **TLSv1.0**, **TLSv1.1**, and **TLSv1.2** as well defining the which cipher suites to use and the order of preference.</span></span>  <span data-ttu-id="357c9-111">Další informace o konfigurovatelných možností protokolu SSL, najdete [zásady protokolu SSL přehled](application-gateway-SSL-policy-overview.md).</span><span class="sxs-lookup"><span data-stu-id="357c9-111">To learn more about configurable SSL options, visit [SSL Policy overview](application-gateway-SSL-policy-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="357c9-112">Protokol SSL 2.0 a SSL 3.0 jsou ve výchozím nastavení zakázané a nemůže být povolena.</span><span class="sxs-lookup"><span data-stu-id="357c9-112">SSL 2.0 and SSL 3.0 are disabled by default and cannot be enabled.</span></span> <span data-ttu-id="357c9-113">Tyto považovány za nezabezpečená a nemůžete použít s aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="357c9-113">They are considered unsecure and are not able to be used with Application Gateway.</span></span>

![scénář image][scenario]

## <a name="scenario"></a><span data-ttu-id="357c9-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="357c9-115">Scenario</span></span>

<span data-ttu-id="357c9-116">V tomto scénáři zjistíte postup vytvoření služby application gateway pomocí protokolu SSL koncová pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="357c9-116">In this scenario, you learn how to create an application gateway using end to end SSL using PowerShell.</span></span>

<span data-ttu-id="357c9-117">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="357c9-117">This scenario will:</span></span>

* <span data-ttu-id="357c9-118">Vytvořte skupinu prostředků s názvem **appgw-rg**</span><span class="sxs-lookup"><span data-stu-id="357c9-118">Create a resource group named **appgw-rg**</span></span>
* <span data-ttu-id="357c9-119">Vytvořit virtuální síť s názvem **appgwvnet** s adresním prostorem 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="357c9-119">Create a virtual network named **appgwvnet** with an address space of 10.0.0.0/16.</span></span>
* <span data-ttu-id="357c9-120">Vytvořte dvě podsítě názvem **appgwsubnet** a **appsubnet**.</span><span class="sxs-lookup"><span data-stu-id="357c9-120">Create two subnets called **appgwsubnet** and **appsubnet**.</span></span>
* <span data-ttu-id="357c9-121">Vytvoření brány malá aplikace podpora šifrování SSL koncová této verze protokoly SSL omezení a šifrovacích sad.</span><span class="sxs-lookup"><span data-stu-id="357c9-121">Create a small application gateway supporting end to end SSL encryption that limits SSL protocols versions and cipher suites.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="357c9-122">Než začnete</span><span class="sxs-lookup"><span data-stu-id="357c9-122">Before you begin</span></span>

<span data-ttu-id="357c9-123">Konfigurace protokolu SSL koncová s aplikační bránu, vyžaduje se certifikát pro bránu a certifikáty se vyžadují pro back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="357c9-123">To configure end to end SSL with an application gateway, a certificate is required for the gateway and certificates are required for the backend servers.</span></span> <span data-ttu-id="357c9-124">Certifikát brány se používá k šifrování a dešifrování data odesílaná do pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-124">The gateway certificate is used to encrypt and decrypt the traffic sent to it using SSL.</span></span> <span data-ttu-id="357c9-125">Certifikát brány musí být ve formátu Personal Information Exchange (pfx).</span><span class="sxs-lookup"><span data-stu-id="357c9-125">The gateway certificate needs to be in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="357c9-126">Tento formát souboru umožňuje pro export privátního klíče, který je vyžadován součástí služby application gateway k šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="357c9-126">This file format allows for the private key to be exported which is required by the application gateway to perform the encryption and decryption of traffic.</span></span>

<span data-ttu-id="357c9-127">Pro šifrování SSL koncová back-end musí být seznam povolených adres s aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="357c9-127">For end to end SSL encryption the backend must be whitelisted with application gateway.</span></span> <span data-ttu-id="357c9-128">K tomu je potřeba odesílání veřejný certifikát back-EndY aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="357c9-128">This is done by uploading the public certificate of the backends to the application gateway.</span></span> <span data-ttu-id="357c9-129">Tím se zajistí, že aplikační bránu pouze komunikuje s instancí známé back-end.</span><span class="sxs-lookup"><span data-stu-id="357c9-129">This ensures that the application gateway only communicates with known backend instances.</span></span> <span data-ttu-id="357c9-130">Tato další zabezpečuje komunikaci začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="357c9-130">This further secures the end to end communication.</span></span>

<span data-ttu-id="357c9-131">Tento proces je popsán v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="357c9-131">This process is described in the following steps:</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="357c9-132">Vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="357c9-132">Create the Resource Group</span></span>

<span data-ttu-id="357c9-133">Tato část vás provede procesem vytvoření skupiny prostředků, který obsahuje službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-133">This section walks you through creating a resource group, that contains the application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="357c9-134">Krok 1</span><span class="sxs-lookup"><span data-stu-id="357c9-134">Step 1</span></span>

<span data-ttu-id="357c9-135">Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="357c9-135">Log in to your Azure Account.</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="357c9-136">Krok 2</span><span class="sxs-lookup"><span data-stu-id="357c9-136">Step 2</span></span>

<span data-ttu-id="357c9-137">Vyberte předplatné, které chcete použít pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="357c9-137">Select the subscription to use for this scenario.</span></span>

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a><span data-ttu-id="357c9-138">Krok 3</span><span class="sxs-lookup"><span data-stu-id="357c9-138">Step 3</span></span>

<span data-ttu-id="357c9-139">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="357c9-139">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="357c9-140">Vytvoření virtuální sítě a podsítě pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="357c9-140">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="357c9-141">Následující příklad vytvoří virtuální síť a dvě podsítě.</span><span class="sxs-lookup"><span data-stu-id="357c9-141">The following example creates a virtual network and two subnets.</span></span> <span data-ttu-id="357c9-142">Jednu podsíť se používá k ukládání aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="357c9-142">One subnet is used to hold the application gateway.</span></span> <span data-ttu-id="357c9-143">Další podsítě se používá pro back-EndY hostování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="357c9-143">The other subnet is used for the backends hosting the web application.</span></span>

### <a name="step-1"></a><span data-ttu-id="357c9-144">Krok 1</span><span class="sxs-lookup"><span data-stu-id="357c9-144">Step 1</span></span>

<span data-ttu-id="357c9-145">Přiřaďte rozsah adres pro podsíť použít pro službu Application Gateway sám sebe.</span><span class="sxs-lookup"><span data-stu-id="357c9-145">Assign an address range for the subnet be used for the Application Gateway itself.</span></span>

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> <span data-ttu-id="357c9-146">Podsítě konfigurované pro službu application gateway by měl být správnou velikost.</span><span class="sxs-lookup"><span data-stu-id="357c9-146">Subnets configured for application gateway should be properly sized.</span></span> <span data-ttu-id="357c9-147">Aplikační brány mohou být konfigurovány pro až 10 instancí.</span><span class="sxs-lookup"><span data-stu-id="357c9-147">An application gateway can be configured for up to 10 instances.</span></span> <span data-ttu-id="357c9-148">Každá instance trvá jednu IP adresu z podsítě.</span><span class="sxs-lookup"><span data-stu-id="357c9-148">Each instance takes one IP address from the subnet.</span></span> <span data-ttu-id="357c9-149">Příliš malé podsítě může nepříznivě ovlivnit škálování služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-149">Too small of a subnet can adversely affect scaling out an application gateway.</span></span>
> 
> 

### <a name="step-2"></a><span data-ttu-id="357c9-150">Krok 2</span><span class="sxs-lookup"><span data-stu-id="357c9-150">Step 2</span></span>

<span data-ttu-id="357c9-151">Přiřaďte rozsah adres, který se má použít pro back-end fondu adres.</span><span class="sxs-lookup"><span data-stu-id="357c9-151">Assign an address range to be used for the Backend address pool.</span></span>

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a><span data-ttu-id="357c9-152">Krok 3</span><span class="sxs-lookup"><span data-stu-id="357c9-152">Step 3</span></span>

<span data-ttu-id="357c9-153">Vytvořte virtuální síť s podsítí definované v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="357c9-153">Create a virtual network with the subnets defined in the preceding steps.</span></span>

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a><span data-ttu-id="357c9-154">Krok 4</span><span class="sxs-lookup"><span data-stu-id="357c9-154">Step 4</span></span>

<span data-ttu-id="357c9-155">Načtení prostředků virtuální sítě a prostředky podsítě, který se má použít v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="357c9-155">Retrieve the virtual network resource and subnet resources to be used in the following steps:</span></span>

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="357c9-156">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="357c9-156">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="357c9-157">Vytvořte prostředek veřejné IP, který se má použít pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-157">Create a public IP resource to be used for the application gateway.</span></span> <span data-ttu-id="357c9-158">Tato veřejná IP adresa se používá následující krok.</span><span class="sxs-lookup"><span data-stu-id="357c9-158">This public IP address is used a following step.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> <span data-ttu-id="357c9-159">Aplikační brána nepodporuje použití veřejnou IP adresu vytvořit s definovaným popiskem domény.</span><span class="sxs-lookup"><span data-stu-id="357c9-159">Application Gateway does not support the use of a public IP address created with a domain label defined.</span></span> <span data-ttu-id="357c9-160">Je podporován pouze veřejné IP adresy s popiskem dynamicky vytvořené domény.</span><span class="sxs-lookup"><span data-stu-id="357c9-160">Only a public IP address with a dynamically created domain label is supported.</span></span> <span data-ttu-id="357c9-161">Pokud budete potřebovat dns popisný název pro službu application gateway, se doporučuje použít záznam CNAME jako alias.</span><span class="sxs-lookup"><span data-stu-id="357c9-161">If you require a friendly dns name for the application gateway, it is recommended to use a CNAME record as an alias.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="357c9-162">Vytvořte objekt konfigurace aplikační brány </span><span class="sxs-lookup"><span data-stu-id="357c9-162">Create an application gateway configuration object</span></span>

<span data-ttu-id="357c9-163">Před vytvořením služby application gateway se nastavit všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="357c9-163">All configuration items are set before creating the application gateway.</span></span> <span data-ttu-id="357c9-164">Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-164">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="357c9-165">Krok 1</span><span class="sxs-lookup"><span data-stu-id="357c9-165">Step 1</span></span>

<span data-ttu-id="357c9-166">Vytvoření konfigurace IP aplikační brány, toto nastavení konfiguruje jaké podsíť používá aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="357c9-166">Create an application gateway IP configuration, this setting configures what subnet the application gateway uses.</span></span> <span data-ttu-id="357c9-167">Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě a směruje síťový provoz na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="357c9-167">When application gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="357c9-168">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="357c9-168">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a><span data-ttu-id="357c9-169">Krok 2</span><span class="sxs-lookup"><span data-stu-id="357c9-169">Step 2</span></span>

<span data-ttu-id="357c9-170">Vytvořte konfiguraci front-end IP adresy, toto nastavení se mapuje na privátní nebo veřejné ip adresu front-endu služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-170">Create a front-end IP configuration, this setting maps a private or public ip address to the front end of the application gateway.</span></span> <span data-ttu-id="357c9-171">Následující krok přidruží veřejnou IP adresu v předchozím kroku s konfigurací front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="357c9-171">The following step associates the public IP address in the preceding step with the front-end IP configuration.</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a><span data-ttu-id="357c9-172">Krok 3</span><span class="sxs-lookup"><span data-stu-id="357c9-172">Step 3</span></span>

<span data-ttu-id="357c9-173">Nakonfigurujte fond back-end IP adres pomocí IP adresy webových serverů back-end.</span><span class="sxs-lookup"><span data-stu-id="357c9-173">Configure the back-end IP address pool with the IP addresses of the backend web servers.</span></span> <span data-ttu-id="357c9-174">Tyto IP adresy jsou IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy.</span><span class="sxs-lookup"><span data-stu-id="357c9-174">These IP addresses are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="357c9-175">Můžete nahradit následující adresy přidat koncové body vlastní aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="357c9-175">You replace the following IP addresses to add your own application IP address endpoints.</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> <span data-ttu-id="357c9-176">Platný plně kvalifikovaný název domény (FQDN) je také platnou hodnotu místo ip adresu pro back-end serverů s přepínačem - BackendFqdns.</span><span class="sxs-lookup"><span data-stu-id="357c9-176">A fully qualified domain name (FQDN) is also a valid value in place of an ip address for the backend servers by using the -BackendFqdns switch.</span></span> 

### <a name="step-4"></a><span data-ttu-id="357c9-177">Krok 4</span><span class="sxs-lookup"><span data-stu-id="357c9-177">Step 4</span></span>

<span data-ttu-id="357c9-178">Nakonfigurujte port front-end IP pro koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="357c9-178">Configure the front-end IP port for the public IP endpoint.</span></span> <span data-ttu-id="357c9-179">Tento port je port, který koncoví uživatelé připojit k.</span><span class="sxs-lookup"><span data-stu-id="357c9-179">This port is the port that end users connect to.</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a><span data-ttu-id="357c9-180">Krok 5</span><span class="sxs-lookup"><span data-stu-id="357c9-180">Step 5</span></span>

<span data-ttu-id="357c9-181">Nakonfigurujte certifikát pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-181">Configure the certificate for the application gateway.</span></span> <span data-ttu-id="357c9-182">Tento certifikát se používá k dešifrování a znovu zašifrovat přenosy ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-182">This certificate is used to decrypt and re-encrypt the traffic on the application gateway.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

> [!NOTE]
> <span data-ttu-id="357c9-183">Tímto vzorovým kódem se nakonfiguruje certifikát používaný pro připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-183">This sample configures the certificate used for SSL connection.</span></span> <span data-ttu-id="357c9-184">Certifikát musí být ve formátu .pfx, heslo musí mít 4 až 12 znaků.</span><span class="sxs-lookup"><span data-stu-id="357c9-184">The certificate needs to be in .pfx format, and the password must be between 4 to 12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="357c9-185">Krok 6</span><span class="sxs-lookup"><span data-stu-id="357c9-185">Step 6</span></span>

<span data-ttu-id="357c9-186">Vytvořte naslouchací proces protokolu HTTP pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-186">Create the HTTP listener for the application gateway.</span></span> <span data-ttu-id="357c9-187">Přiřadíte konfigurace front-end IP adresu, port a certifikátu SSL se má použít.</span><span class="sxs-lookup"><span data-stu-id="357c9-187">Assign the front-end ip configuration, port, and SSL certificate to use.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a><span data-ttu-id="357c9-188">Krok 7</span><span class="sxs-lookup"><span data-stu-id="357c9-188">Step 7</span></span>

<span data-ttu-id="357c9-189">Nahrajte certifikát, který se má použít na SSL povoleno back-end fondu zdrojů.</span><span class="sxs-lookup"><span data-stu-id="357c9-189">Upload the certificate to be used on the SSL enabled backend pool resources.</span></span>

> [!NOTE]
> <span data-ttu-id="357c9-190">Získá výchozí kontroly veřejného klíče z **výchozí** vazby SSL na IP adrese a porovná ji sem zadáte hodnotu veřejného klíče obdrží hodnotě veřejných klíčů můžete back-end.</span><span class="sxs-lookup"><span data-stu-id="357c9-190">The default probe gets the public key from the **default** SSL binding on the back-end's IP address and compares the public key value it receives to the public key value you provide here.</span></span> <span data-ttu-id="357c9-191">Načtený veřejný klíč nemusí být nutně zamýšlená lokalita, na které přenosové toky **Pokud** používáte hlavičky hostitele a SNI na back-end.</span><span class="sxs-lookup"><span data-stu-id="357c9-191">The retrieved public key may not necessarily be the intended site to which traffic flows **if** you are using host headers and SNI on the back-end.</span></span> <span data-ttu-id="357c9-192">Pokud máte pochybnosti, navštivte https://127.0.0.1/ na back EndY k potvrzení, který certifikát se používá pro **výchozí** vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-192">If in doubt, visit https://127.0.0.1/ on the back-ends to confirm which certificate is used for the **default** SSL binding.</span></span> <span data-ttu-id="357c9-193">V této části použijte veřejný klíč z tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="357c9-193">Use the public key from that request in this section.</span></span> <span data-ttu-id="357c9-194">Pokud používáte hlavičky hostitele a SNI na vazby HTTPS a neobdržíte odpověď a certifikát z žádost ruční prohlížeče na https://127.0.0.1/ na back EndY, musíte vytvořit vazbu výchozí SSL na back EndY.</span><span class="sxs-lookup"><span data-stu-id="357c9-194">If you are using host-headers and SNI on HTTPS bindings and you do not receive a response and certificate from a manual browser request to https://127.0.0.1/ on the back-ends, you must set up a default SSL binding on the back-ends.</span></span> <span data-ttu-id="357c9-195">Pokud to neuděláte, selhání sondy a back-end není povolený.</span><span class="sxs-lookup"><span data-stu-id="357c9-195">If you do not do so, probes fail and the back-end is not whitelisted.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> <span data-ttu-id="357c9-196">Certifikát zadaný v tomto kroku musí být veřejný klíč certifikátu pfx, nachází na back-end.</span><span class="sxs-lookup"><span data-stu-id="357c9-196">The certificate provided in this step should be the public key of the pfx cert present on the backend.</span></span> <span data-ttu-id="357c9-197">Exportujte certifikátu (ne kořenový certifikát), nainstalovaný na serveru back-end v. CER formátu a použít ho v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="357c9-197">Export the certificate (not the root certificate) installed on the backend server in .CER format and use it in this step.</span></span> <span data-ttu-id="357c9-198">Tento krok povolených programů back-end pomocí služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-198">This step whitelists the backend with the application gateway.</span></span>

### <a name="step-8"></a><span data-ttu-id="357c9-199">Krok 8</span><span class="sxs-lookup"><span data-stu-id="357c9-199">Step 8</span></span>

<span data-ttu-id="357c9-200">Nakonfigurujte nastavení http back-end služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-200">Configure the application gateway back-end http settings.</span></span> <span data-ttu-id="357c9-201">Přiřadíte certifikát odeslali v předchozím kroku nastavení protokolu http.</span><span class="sxs-lookup"><span data-stu-id="357c9-201">Assign the certificate uploaded in the preceding step to the http settings.</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a><span data-ttu-id="357c9-202">Krok 9</span><span class="sxs-lookup"><span data-stu-id="357c9-202">Step 9</span></span>

<span data-ttu-id="357c9-203">Vytvořte směrování pravidlo služby load balancer které konfiguruje chování nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="357c9-203">Create a load balancer routing rule that configures the load balancer behavior.</span></span> <span data-ttu-id="357c9-204">V tomto příkladu se vytvoří pravidlo základní kruhové dotazování.</span><span class="sxs-lookup"><span data-stu-id="357c9-204">In this example, a basic round robin rule is created.</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a><span data-ttu-id="357c9-205">Krok 10</span><span class="sxs-lookup"><span data-stu-id="357c9-205">Step 10</span></span>

<span data-ttu-id="357c9-206">Nakonfigurujte velikost instance služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-206">Configure the instance size of the application gateway.</span></span>  <span data-ttu-id="357c9-207">Dostupné velikosti jsou **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**.</span><span class="sxs-lookup"><span data-stu-id="357c9-207">The available sizes are **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large**.</span></span>  <span data-ttu-id="357c9-208">Pro kapacitu dostupné hodnoty jsou 1 až 10.</span><span class="sxs-lookup"><span data-stu-id="357c9-208">For capacity, the available values are 1 through 10.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> <span data-ttu-id="357c9-209">Počet instancí 1 lze zvolit pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="357c9-209">An instance count of 1 can be chosen for testing purposes.</span></span> <span data-ttu-id="357c9-210">Je důležité vědět, že libovolný počet instancí v rámci dvě instance není předmětem smlouvě SLA a proto nedoporučují.</span><span class="sxs-lookup"><span data-stu-id="357c9-210">It is important to know that any instance count under two instances is not covered by the SLA and are therefore not recommended.</span></span> <span data-ttu-id="357c9-211">Malé brány se mají použít pro vývoj testování a ne pro produkční účely.</span><span class="sxs-lookup"><span data-stu-id="357c9-211">Small gateways are to be used for dev test and not for production purposes.</span></span>

### <a name="step-11"></a><span data-ttu-id="357c9-212">Krok 11</span><span class="sxs-lookup"><span data-stu-id="357c9-212">Step 11</span></span>

<span data-ttu-id="357c9-213">Nakonfigurujte zásady protokolu SSL pro službu Application Gateway používat.</span><span class="sxs-lookup"><span data-stu-id="357c9-213">Configure the SSL policy to be used on the Application Gateway.</span></span> <span data-ttu-id="357c9-214">Aplikační brána podporuje možnost nastavit minimální verze pro verze protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-214">Application Gateway supports the ability to set a minimum version for SSL protocol versions.</span></span>

<span data-ttu-id="357c9-215">Seznam verzí protokolu, které lze definovat jsou následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="357c9-215">The following values are a list of protocol versions that can be defined.</span></span>

* <span data-ttu-id="357c9-216">**TLSv1_0**</span><span class="sxs-lookup"><span data-stu-id="357c9-216">**TLSv1_0**</span></span>
* <span data-ttu-id="357c9-217">**TLSv1_1**</span><span class="sxs-lookup"><span data-stu-id="357c9-217">**TLSv1_1**</span></span>
* <span data-ttu-id="357c9-218">**TLSv1_2**</span><span class="sxs-lookup"><span data-stu-id="357c9-218">**TLSv1_2**</span></span>

<span data-ttu-id="357c9-219">Nastaví verzi protokolu minimální **TLSv1_2** a umožňuje **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** pouze.</span><span class="sxs-lookup"><span data-stu-id="357c9-219">Sets the minimum protocol version to **TLSv1_2** and enables **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** only.</span></span>

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-the-application-gateway"></a><span data-ttu-id="357c9-220">Vytvoření aplikační brány</span><span class="sxs-lookup"><span data-stu-id="357c9-220">Create the Application Gateway</span></span>

<span data-ttu-id="357c9-221">Pomocí všech předchozích kroků vytvořte aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="357c9-221">Using all the preceding steps, create the Application Gateway.</span></span> <span data-ttu-id="357c9-222">Vytvoření brány je dlouhotrvající proces.</span><span class="sxs-lookup"><span data-stu-id="357c9-222">The creation of the gateway is a long running process.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a><span data-ttu-id="357c9-223">Omezit verzí protokolu SSL na existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="357c9-223">Limit SSL protocol versions on an existing Application Gateway</span></span>

<span data-ttu-id="357c9-224">Předchozí kroky trvat vás vytváření aplikací pomocí protokolu SSL a provést tak kompletní a zakázání určitých verzí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-224">The preceding steps take you through creating an application with end to end SSL and disabling certain SSL protocol versions.</span></span> <span data-ttu-id="357c9-225">Následující příklad zakazuje určité zásady protokolu SSL na existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="357c9-225">The following example disables certain SSL policies on an existing application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="357c9-226">Krok 1</span><span class="sxs-lookup"><span data-stu-id="357c9-226">Step 1</span></span>

<span data-ttu-id="357c9-227">Načtení služby application gateway k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="357c9-227">Retrieve the application gateway to update.</span></span>

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a><span data-ttu-id="357c9-228">Krok 2</span><span class="sxs-lookup"><span data-stu-id="357c9-228">Step 2</span></span>

<span data-ttu-id="357c9-229">Definujte zásady protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="357c9-229">Define an SSL policy.</span></span> <span data-ttu-id="357c9-230">V následujícím příkladu TLSv1.0 a TLSv1.1 jsou zakázány a šifrovací sada **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** jsou povoleny pouze ty.</span><span class="sxs-lookup"><span data-stu-id="357c9-230">In the following example, TLSv1.0 and TLSv1.1 are disabled and the cipher suites **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** are the only ones allowed.</span></span>

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a><span data-ttu-id="357c9-231">Krok 3</span><span class="sxs-lookup"><span data-stu-id="357c9-231">Step 3</span></span>

<span data-ttu-id="357c9-232">Nakonec aktualizujte bránu.</span><span class="sxs-lookup"><span data-stu-id="357c9-232">Finally, update the gateway.</span></span> <span data-ttu-id="357c9-233">Je důležité si uvědomit, že tento poslední krok je dlouho spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="357c9-233">It is important to note that this last step is a long running task.</span></span> <span data-ttu-id="357c9-234">Až skončíte, koncová SSL je nakonfigurován ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-234">When it is done, end to end SSL is configured on the application gateway.</span></span>

```powershell
$gw | Set-AzureRmApplicationGateway
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="357c9-235">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="357c9-235">Get application gateway DNS name</span></span>

<span data-ttu-id="357c9-236">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="357c9-236">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="357c9-237">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="357c9-237">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="357c9-238">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-238">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="357c9-239">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="357c9-239">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="357c9-240">Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-240">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="357c9-241">Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS.</span><span class="sxs-lookup"><span data-stu-id="357c9-241">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="357c9-242">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="357c9-242">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="357c9-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="357c9-243">Next steps</span></span>

<span data-ttu-id="357c9-244">Další informace o posílení zabezpečení webových aplikací pomocí brány Firewall webových aplikací prostřednictvím brány aplikace navštivte stránky [brány Firewall webových aplikací – přehled](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="357c9-244">Learn about hardening the security of your web applications with Web Application Firewall through Application Gateway by visiting [Web Application Firewall Overview](application-gateway-webapplicationfirewall-overview.md)</span></span>

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
