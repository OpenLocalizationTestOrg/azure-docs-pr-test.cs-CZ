---
title: "aaaCreate Azure interní nástroj pro vyrovnávání - zatížení, prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní vyrovnávání zátěže pomocí prostředí PowerShell ve službě Správce prostředků"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a><span data-ttu-id="33a00-103">Vytvoření interního nástroje pro vyrovnávání zatížení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="33a00-103">Create an internal load balancer using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="33a00-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33a00-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="33a00-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="33a00-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="33a00-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="33a00-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="33a00-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="33a00-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="33a00-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="33a00-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="33a00-109">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="33a00-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

<span data-ttu-id="33a00-110">Hello následující kroky popisují, jak toocreate na interní vyrovnávání zátěže pomocí Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33a00-110">hello following steps explain how toocreate an internal load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="33a00-111">S Azure Resource Manager, hello položky toocreate Vyrovnávání zatížení pro vnitřní jsou konfigurovat individuálně a potom v kombinaci toocreate nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="33a00-111">With Azure Resource Manager, hello items toocreate a Internal load balancer are configured individually and then combined toocreate a load balancer.</span></span>

<span data-ttu-id="33a00-112">Je třeba toocreate a nakonfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="33a00-112">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="33a00-113">Koncová konfigurace IP front - nakonfiguruje hello privátní IP adresu pro příchozí síťový provoz</span><span class="sxs-lookup"><span data-stu-id="33a00-113">Front end IP configuration - will configure hello private IP address for incoming network traffic</span></span>
* <span data-ttu-id="33a00-114">Fond adres back-end - nakonfiguruje hello síťových rozhraní, které budou přijímat provoz hello skupinu s vyrovnáváním zatížení, který přichází z fondu IP front-endu</span><span class="sxs-lookup"><span data-stu-id="33a00-114">Backend address pool - will configure hello network interfaces which will receive hello load balanced traffic coming from front end IP pool</span></span>
* <span data-ttu-id="33a00-115">Pravidla Vyrovnávání zatížení – zdroj a konfigurace místního portu pro hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="33a00-115">Load balancing rules - source and local port configuration for hello load balancer.</span></span>
* <span data-ttu-id="33a00-116">Sondy – konfiguruje test stavu hello stavu pro instance virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-116">Probes - configures hello health status probe for hello Virtual Machine instances.</span></span>
* <span data-ttu-id="33a00-117">Příchozí pravidla NAT – konfiguruje hello portu pravidla toodirectly přístup, jednu z instancí virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-117">Inbound NAT rules - configures hello port rules toodirectly access one of hello Virtual Machine instances.</span></span>

<span data-ttu-id="33a00-118">Další informace o komponentách nástroje pro vyrovnávání zatížení s Azure Resource Managerem najdete v tématu [Podpora nástroje Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="33a00-118">You can get more information about load balancer components with Azure resource manager at [Azure Resource Manager support for load balancer](load-balancer-arm.md).</span></span>

<span data-ttu-id="33a00-119">Hello následující kroky popisují, jak tooconfigure Vyrovnávání zatížení mezi dvěma virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="33a00-119">hello following steps explain how tooconfigure a load balancer between two virtual machines.</span></span>

## <a name="setup-powershell-toouse-resource-manager"></a><span data-ttu-id="33a00-120">Instalace prostředí PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="33a00-120">Setup PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="33a00-121">Ujistěte se, že máte hello nejnovější produkční verzi hello Azure modul pro prostředí PowerShell a mít PowerShell nastavit správně tooaccess vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="33a00-121">Make sure you have hello latest production version of hello Azure module for PowerShell, and have PowerShell setup correctly tooaccess your Azure subscription.</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-122">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-122">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="33a00-123">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-123">Step 2</span></span>

<span data-ttu-id="33a00-124">Zkontrolujte předplatná hello pro účet hello</span><span class="sxs-lookup"><span data-stu-id="33a00-124">Check hello subscriptions for hello account</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="33a00-125">Výzvami tooAuthenticate bude pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="33a00-125">You will be prompted tooAuthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="33a00-126">Krok 3</span><span class="sxs-lookup"><span data-stu-id="33a00-126">Step 3</span></span>

<span data-ttu-id="33a00-127">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="33a00-127">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a><span data-ttu-id="33a00-128">Vytvoření skupiny prostředků pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-128">Create Resource Group for load balancer</span></span>

<span data-ttu-id="33a00-129">Vytvořte novou skupinu prostředků (tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků).</span><span class="sxs-lookup"><span data-stu-id="33a00-129">Create a new resource group (skip this step if using an existing resource group)</span></span>

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

<span data-ttu-id="33a00-130">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="33a00-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="33a00-131">To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="33a00-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="33a00-132">Zajistěte, aby všechny příkazy hello toocreate nástroj pro vyrovnávání zatížení bude používat stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="33a00-132">Make sure all commands toocreate a load balancer will use hello same resource group.</span></span>

<span data-ttu-id="33a00-133">V hello příkladu výše jsme vytvořili skupinu prostředků s názvem "NRP-RG" a umístěním "Západní USA".</span><span class="sxs-lookup"><span data-stu-id="33a00-133">In hello example above we created a resource group called "NRP-RG" and location "West US".</span></span>

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a><span data-ttu-id="33a00-134">Vytvoření virtuální sítě a privátní IP adresy pro front-endový fond IP adres</span><span class="sxs-lookup"><span data-stu-id="33a00-134">Create Virtual Network and a private IP address for front end IP pool</span></span>

<span data-ttu-id="33a00-135">Vytvoří podsíť virtuální sítě hello a přiřadí toovariable $backendSubnet</span><span class="sxs-lookup"><span data-stu-id="33a00-135">Creates a subnet for hello virtual network and assigns toovariable $backendSubnet</span></span>

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

<span data-ttu-id="33a00-136">Vytvořte virtuální síť:</span><span class="sxs-lookup"><span data-stu-id="33a00-136">Create a virtual network:</span></span>

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

<span data-ttu-id="33a00-137">Vytvoří virtuální síť hello a přidá hello podsíť virtuální sítě se podsíť lb toohello NRPVNet a přiřadí toovariable $vnet</span><span class="sxs-lookup"><span data-stu-id="33a00-137">Creates hello virtual network and adds hello subnet lb-subnet-be toohello virtual network NRPVNet and assigns toovariable $vnet</span></span>

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a><span data-ttu-id="33a00-138">Vytvoření front-endového fondu IP adres a back-endového fondu adres</span><span class="sxs-lookup"><span data-stu-id="33a00-138">Create Front end IP pool and backend address pool</span></span>

<span data-ttu-id="33a00-139">Nastavení fondu IP front-endu pro příchozí hello zatížení nástroje pro vyrovnávání zatížení sítě a back-end adresy fondu tooreceive hello přenosy přesměrované vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="33a00-139">Setting up a front end IP pool for hello incoming load balancer network traffic and backend address pool tooreceive hello load balanced traffic.</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-140">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-140">Step 1</span></span>

<span data-ttu-id="33a00-141">Vytvoření fondu IP front-endu pomocí hello privátní IP adresu 10.0.2.5 pro hello podsíť 10.0.2.0/24 kterým bude hello příchozí síťový provoz koncový bod.</span><span class="sxs-lookup"><span data-stu-id="33a00-141">Create a front end IP pool using hello private IP address 10.0.2.5 for hello subnet 10.0.2.0/24 which will be hello incoming network traffic endpoint.</span></span>

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a><span data-ttu-id="33a00-142">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-142">Step 2</span></span>

<span data-ttu-id="33a00-143">Nastavení fondu back-end adresy použít tooreceive příchozí provoz z fondu IP front-endu:</span><span class="sxs-lookup"><span data-stu-id="33a00-143">Set up a back end address pool used tooreceive incoming traffic from front end IP pool:</span></span>

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a><span data-ttu-id="33a00-144">Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT), testu a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-144">Create LB rules, NAT rules, probe and load balancer</span></span>

<span data-ttu-id="33a00-145">Po vytvoření hello fond IP front-endu a back-endových adres hello, budete potřebovat toocreate hello pravidla, která bude patřit toohello prostředek pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="33a00-145">After creating hello front end IP pool and hello backend address pool, you will need toocreate hello rules which will belong toohello load balancer resource:</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-146">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-146">Step 1</span></span>

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

<span data-ttu-id="33a00-147">výše uvedený příklad Hello vytváří hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="33a00-147">hello example above is creating hello following items:</span></span>

* <span data-ttu-id="33a00-148">Pravidlo NAT, které všechny příchozí přenosy tooport 3441 přejde tooport 3389.</span><span class="sxs-lookup"><span data-stu-id="33a00-148">NAT rule which all incoming traffic tooport 3441 will go tooport 3389.</span></span>
* <span data-ttu-id="33a00-149">druhé pravidlo NAT, která všechny příchozí přenosy tooport 3442 přejde tooport 3389.</span><span class="sxs-lookup"><span data-stu-id="33a00-149">a second NAT rule which all incoming traffic tooport 3442 will go tooport 3389.</span></span>
* <span data-ttu-id="33a00-150">pravidlo Vyrovnávání zatížení, která načte vyrovnávat veškerý příchozí provoz na veřejném portu 80 toolocal portu 80 ve fondu adres hello back-end.</span><span class="sxs-lookup"><span data-stu-id="33a00-150">a load balancer rule which will load balance all incoming traffic on public port 80 toolocal port 80 in hello back end address pool.</span></span>
* <span data-ttu-id="33a00-151">pravidlo testu, který zkontroluje hello stav pro cestu "HealthProbe.aspx"</span><span class="sxs-lookup"><span data-stu-id="33a00-151">a probe rule which will check hello health status for path "HealthProbe.aspx"</span></span>

### <a name="step-2"></a><span data-ttu-id="33a00-152">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-152">Step 2</span></span>

<span data-ttu-id="33a00-153">Vytvořte nástroj pro vyrovnávání zatížení hello společně přidání všechny objekty (pravidla NAT, pravidla nástroje pro vyrovnávání zatížení, test konfigurace):</span><span class="sxs-lookup"><span data-stu-id="33a00-153">Create hello load balancer adding all objects (NAT rules, Load balancer rules, probe configurations) together:</span></span>

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a><span data-ttu-id="33a00-154">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="33a00-154">Create network interfaces</span></span>

<span data-ttu-id="33a00-155">Po vytvoření hello interní vyrovnávání zátěže, třeba definovat rozhraní sítě, která bude moci přijmout hello příchozí skupinu s vyrovnáváním zatížení síťový provoz, pravidla NAT a kontroly.</span><span class="sxs-lookup"><span data-stu-id="33a00-155">After creating hello internal load balancer, you need define which network interfaces will be receiving hello incoming load balanced network traffic, NAT rules and probe.</span></span> <span data-ttu-id="33a00-156">v takovém případě Hello síťové rozhraní konfigurované jednotlivě a lze přiřadit tooa virtuální počítač později.</span><span class="sxs-lookup"><span data-stu-id="33a00-156">hello network interface in this case is configured individually and can be assigned tooa virtual machine later on.</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-157">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-157">Step 1</span></span>

<span data-ttu-id="33a00-158">Získáte prostředek hello virtuální síť a podsíť toocreate síťových rozhraní:</span><span class="sxs-lookup"><span data-stu-id="33a00-158">Get hello resource virtual network and subnet toocreate network interfaces:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

<span data-ttu-id="33a00-159">Tento krok vytvoří rozhraní sítě, která bude patřit toohello fond back-end, Vyrovnávání zatížení a přidružit hello první pravidlo NAT RDP pro toto síťové rozhraní:</span><span class="sxs-lookup"><span data-stu-id="33a00-159">This step creates a network interface which will belong toohello load balancer back end pool and associate hello first NAT rule for RDP for this network interface:</span></span>

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a><span data-ttu-id="33a00-160">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-160">Step 2</span></span>

<span data-ttu-id="33a00-161">Vytvořte druhé síťové rozhraní s názvem: LB-Nic2-BE:</span><span class="sxs-lookup"><span data-stu-id="33a00-161">Create a second network interface called LB-Nic2-BE:</span></span>

<span data-ttu-id="33a00-162">Tento krok vytvoří druhé síťové rozhraní, přiřazení toohello stejné Vyrovnávání zatížení zpět ukončení fondu a přidružení hello druhé pravidlo NAT pro RDP vytvořit:</span><span class="sxs-lookup"><span data-stu-id="33a00-162">This step creates a second network interface, assigning toohello same load balancer back end pool and associating hello second NAT rule created for RDP:</span></span>

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

<span data-ttu-id="33a00-163">Konečný výsledek Hello zobrazí hello následující:</span><span class="sxs-lookup"><span data-stu-id="33a00-163">hello end result will show hello following:</span></span>

    $backendnic1

<span data-ttu-id="33a00-164">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="33a00-164">Expected output:</span></span>

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a><span data-ttu-id="33a00-165">Krok 3</span><span class="sxs-lookup"><span data-stu-id="33a00-165">Step 3</span></span>

<span data-ttu-id="33a00-166">Používejte hello příkaz Add-AzureRmVMNetworkInterface tooassign hello seskupování tooa virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="33a00-166">Use hello command Add-AzureRmVMNetworkInterface tooassign hello NIC tooa virtual Machine.</span></span>

<span data-ttu-id="33a00-167">Můžete najít hello pokyny krok za krokem toocreate virtuálního počítače a přiřaďte tooa seskupování následující dokumentaci hello: [vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33a00-167">You can find hello step by step instructions toocreate a virtual machine and assign tooa NIC following hello documentation: [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface"></a><span data-ttu-id="33a00-168">Přidejte hello síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="33a00-168">Add hello network interface</span></span>

<span data-ttu-id="33a00-169">Pokud již máte virtuální počítač vytvořený, můžete přidat hello síťové rozhraní s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="33a00-169">If you already have a virtual machine created, you can add hello network interface with hello following steps:</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-170">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-170">Step 1</span></span>

<span data-ttu-id="33a00-171">Načte prostředek pro vyrovnávání zatížení hello do proměnné (Pokud jste to neudělali, ještě).</span><span class="sxs-lookup"><span data-stu-id="33a00-171">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="33a00-172">Proměnná Hello používá je názvem $lb a hello použijte stejné názvy z prostředek pro vyrovnávání zatížení hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="33a00-172">hello variable used is called $lb and use hello same names from hello load balancer resource created above.</span></span>

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="33a00-173">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-173">Step 2</span></span>

<span data-ttu-id="33a00-174">Načíst proměnnou tooa konfigurace back-end hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-174">Load hello backend configuration tooa variable.</span></span>

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a><span data-ttu-id="33a00-175">Krok 3</span><span class="sxs-lookup"><span data-stu-id="33a00-175">Step 3</span></span>

<span data-ttu-id="33a00-176">Zatížení hello už vytvořené síťové rozhraní do proměnné.</span><span class="sxs-lookup"><span data-stu-id="33a00-176">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="33a00-177">Název proměnné Hello používá je $ síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="33a00-177">hello variable name used is $nic.</span></span> <span data-ttu-id="33a00-178">název síťového rozhraní Hello používá stejný je hello z výše uvedeném příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-178">hello network interface name used is hello same from hello example above.</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a><span data-ttu-id="33a00-179">Krok 4</span><span class="sxs-lookup"><span data-stu-id="33a00-179">Step 4</span></span>

<span data-ttu-id="33a00-180">Změna konfigurace back-end hello hello síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="33a00-180">Change hello backend configuration on hello network interface.</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a><span data-ttu-id="33a00-181">Krok 5</span><span class="sxs-lookup"><span data-stu-id="33a00-181">Step 5</span></span>

<span data-ttu-id="33a00-182">Uložte objekt rozhraní sítě hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-182">Save hello network interface object.</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="33a00-183">Po přidání toohello fond back-end Vyrovnávání zatížení rozhraní sítě začne přijímat síťové přenosy podle pravidel pro tento prostředek pro vyrovnávání zatížení vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="33a00-183">After a network interface is added toohello load balancer backend pool, it starts receiving network traffic based on hello load balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="33a00-184">Aktualizace stávajícího nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-184">Update an existing load balancer</span></span>

### <a name="step-1"></a><span data-ttu-id="33a00-185">Krok 1</span><span class="sxs-lookup"><span data-stu-id="33a00-185">Step 1</span></span>
<span data-ttu-id="33a00-186">Použití služby Vyrovnávání zatížení hello z hello příkladu výše, přiřadíte toovariable objekt nástroje pro vyrovnávání zatížení $slb pomocí Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="33a00-186">Using hello load balancer from hello example above, assign load balancer object toovariable $slb using Get-AzureRmLoadBalancer</span></span>

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="33a00-187">Krok 2</span><span class="sxs-lookup"><span data-stu-id="33a00-187">Step 2</span></span>

<span data-ttu-id="33a00-188">V následujícím příkladu hello přidáte nové pravidlo příchozí NAT přes port 81 v hello front end a port 8181 pro hello back end fondu tooan existující nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-188">In hello following example, you will add a new Inbound NAT rule using port 81 in hello front end and port 8181 for hello back end pool tooan existing load balancer</span></span>

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a><span data-ttu-id="33a00-189">Krok 3</span><span class="sxs-lookup"><span data-stu-id="33a00-189">Step 3</span></span>

<span data-ttu-id="33a00-190">Uložte novou konfiguraci hello pomocí Set-AzureLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="33a00-190">Save hello new configuration using Set-AzureLoadBalancer</span></span>

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="33a00-191">Odebrání nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-191">Remove a load balancer</span></span>

<span data-ttu-id="33a00-192">Použít hello příkaz Remove-AzureRmLoadBalancer toodelete Vyrovnávání zatížení dříve vytvořenou s názvem "NRP-LB" ve skupině prostředků názvem "NRP-RG"</span><span class="sxs-lookup"><span data-stu-id="33a00-192">Use hello command Remove-AzureRmLoadBalancer toodelete a previously created load balancer named "NRP-LB"  in a resource group called "NRP-RG"</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="33a00-193">Můžete použít hello volitelný přepínač - Force tooavoid hello řádku pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="33a00-193">You can use hello optional switch -Force tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33a00-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33a00-194">Next steps</span></span>

[<span data-ttu-id="33a00-195">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-195">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="33a00-196">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="33a00-196">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
