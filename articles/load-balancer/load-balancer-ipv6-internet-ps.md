---
title: "Služba Vyrovnávání zatížení aaaCreate směřujících Internetu Azure s IPv6 – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate internetové nástroj pro vyrovnávání zatížení s IPv6 pomocí prostředí PowerShell pro Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 6ebb108399b070e06dddc33b7a774481eb44d717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a><span data-ttu-id="422e1-104">Začínáte s vytvářením internetovým Vyrovnávání zatížení s IPv6 pomocí prostředí PowerShell pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="422e1-104">Get started creating an Internet facing load balancer with IPv6 using PowerShell for Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="422e1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="422e1-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="422e1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="422e1-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="422e1-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="422e1-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="422e1-108">Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="422e1-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="422e1-109">Nástroj pro vyrovnávání zatížení Hello poskytuje vysokou dostupnost distribucí příchozí komunikaci mezi instance pořádku služby ve cloudových službách nebo virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="422e1-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="422e1-110">Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.</span><span class="sxs-lookup"><span data-stu-id="422e1-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="422e1-111">Příklad scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="422e1-111">Example deployment scenario</span></span>

<span data-ttu-id="422e1-112">Hello následující diagram znázorňuje řešení nasazení v tomto článku Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-112">hello following diagram illustrates hello load balancing solution being deployed in this article.</span></span>

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

<span data-ttu-id="422e1-114">V tomto scénáři vytvoříte následující prostředky Azure hello:</span><span class="sxs-lookup"><span data-stu-id="422e1-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="422e1-115">Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="422e1-115">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="422e1-116">dva načíst vyrovnávání pravidla toomap hello veřejné VIP toohello privátní koncové body</span><span class="sxs-lookup"><span data-stu-id="422e1-116">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>
* <span data-ttu-id="422e1-117">toothat sadu dostupnosti obsahuje hello dva virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="422e1-117">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="422e1-118">dva virtuální počítače (VM)</span><span class="sxs-lookup"><span data-stu-id="422e1-118">two virtual machines (VMs)</span></span>
* <span data-ttu-id="422e1-119">rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené</span><span class="sxs-lookup"><span data-stu-id="422e1-119">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>

## <a name="deploying-hello-solution-using-hello-azure-powershell"></a><span data-ttu-id="422e1-120">Nasazení řešení hello pomocí hello prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="422e1-120">Deploying hello solution using hello Azure PowerShell</span></span>

<span data-ttu-id="422e1-121">Hello následující kroky ukazují, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="422e1-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="422e1-122">S Azure Resource Manager, se vytvoří každý prostředek a konfigurovat individuálně, potom se spojí dohromady toocreate prostředku.</span><span class="sxs-lookup"><span data-stu-id="422e1-122">With Azure Resource Manager, each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="422e1-123">toodeploy nástroj pro vyrovnávání zatížení, můžete vytvořit a nakonfigurovat hello následující objekty:</span><span class="sxs-lookup"><span data-stu-id="422e1-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="422e1-124">Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="422e1-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="422e1-125">Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="422e1-126">Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="422e1-127">Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="422e1-128">Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="422e1-129">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="422e1-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="422e1-130">Nastavení prostředí PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="422e1-130">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="422e1-131">Ujistěte se, že máte nejnovější verzi produkční hello hello modulu Azure Resource Manager pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="422e1-131">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell.</span></span>

1. <span data-ttu-id="422e1-132">Přihlaste se k Azure</span><span class="sxs-lookup"><span data-stu-id="422e1-132">Sign into Azure</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="422e1-133">Po zobrazení výzvy zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="422e1-133">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="422e1-134">Zkontrolujte předplatná hello pro účet hello</span><span class="sxs-lookup"><span data-stu-id="422e1-134">Check hello subscriptions for hello account</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="422e1-135">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="422e1-135">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="422e1-136">Vytvořte skupinu prostředků (Přeskočit tento krok, pokud se používá existující skupina prostředků)</span><span class="sxs-lookup"><span data-stu-id="422e1-136">Create a resource group (skip this step if using an existing resource group)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="422e1-137">Vytvořit virtuální síť a veřejnou IP adresu pro fond hello front-end IP adres</span><span class="sxs-lookup"><span data-stu-id="422e1-137">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="422e1-138">Vytvořte virtuální síť s podsítí.</span><span class="sxs-lookup"><span data-stu-id="422e1-138">Create a virtual network with a subnet.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="422e1-139">Vytvořte Azure veřejná IP adresa (PIP) prostředky pro hello front-end fond IP adres.</span><span class="sxs-lookup"><span data-stu-id="422e1-139">Create Azure Public IP address (PIP) resources for hello front-end IP address pool.</span></span>

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="422e1-140">Vyrovnávání zatížení Hello používá hello domény popisek hello veřejnou IP adresu jako předpona pro jeho plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="422e1-140">hello load balancer uses hello domain label of hello public IP as prefix for its FQDN.</span></span> <span data-ttu-id="422e1-141">V tomto příkladu jsou plně kvalifikované názvy domény hello *lbnrpipv4.westus.cloudapp.azure.com* a *lbnrpipv6.westus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="422e1-141">In this example, hello FQDNs are *lbnrpipv4.westus.cloudapp.azure.com* and *lbnrpipv6.westus.cloudapp.azure.com*.</span></span>

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a><span data-ttu-id="422e1-142">Vytvoření konfigurace IP front-endový a fond Back-End adresy</span><span class="sxs-lookup"><span data-stu-id="422e1-142">Create a Front-End IP configurations and a Back-End Address Pool</span></span>

1. <span data-ttu-id="422e1-143">Vytvořte konfiguraci adresa front-endu, který používá hello veřejné IP adresy, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="422e1-143">Create front-end address configuration that uses hello Public IP addresses you created.</span></span>

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. <span data-ttu-id="422e1-144">Vytvořte fondy adres back-end.</span><span class="sxs-lookup"><span data-stu-id="422e1-144">Create back-end address pools.</span></span>

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a><span data-ttu-id="422e1-145">Vytvoření pravidla LB, pravidla NAT, sondu a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="422e1-145">Create LB rules, NAT rules, a probe, and a load balancer</span></span>

<span data-ttu-id="422e1-146">Tento příklad vytvoří hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="422e1-146">This example creates hello following items:</span></span>

* <span data-ttu-id="422e1-147">zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 443 tooport 4443</span><span class="sxs-lookup"><span data-stu-id="422e1-147">a NAT rule tootranslate all incoming traffic on port 443 tooport 4443</span></span>
* <span data-ttu-id="422e1-148">toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-148">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="422e1-149">zatížení vyrovnávání pravidlo tooallow RDP připojení toohello virtuálních počítačů na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="422e1-149">a load balancer rule tooallow RDP connection toohello VMs on port 3389.</span></span>
* <span data-ttu-id="422e1-150">Stav testu toocheck pravidlo hello na stránku s názvem *HealthProbe.aspx* nebo službu na portu 8080</span><span class="sxs-lookup"><span data-stu-id="422e1-150">a probe rule toocheck hello health status on a page named *HealthProbe.aspx* or a service on port 8080</span></span>
* <span data-ttu-id="422e1-151">Vyrovnávání zatížení, která používá tyto objekty</span><span class="sxs-lookup"><span data-stu-id="422e1-151">a load balancer that uses all these objects</span></span>

1. <span data-ttu-id="422e1-152">Vytvoření pravidla NAT hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-152">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. <span data-ttu-id="422e1-153">Vytvořte test stavu.</span><span class="sxs-lookup"><span data-stu-id="422e1-153">Create a health probe.</span></span> <span data-ttu-id="422e1-154">Existují dva způsoby tooconfigure sondu:</span><span class="sxs-lookup"><span data-stu-id="422e1-154">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="422e1-155">Test protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="422e1-155">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="422e1-156">nebo TCP testu</span><span class="sxs-lookup"><span data-stu-id="422e1-156">or TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="422e1-157">V tomto příkladu přidáme hello toouse sondy TCP.</span><span class="sxs-lookup"><span data-stu-id="422e1-157">For this example, we are going toouse hello TCP probes.</span></span>

3. <span data-ttu-id="422e1-158">Vytvořte pravidlo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="422e1-158">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. <span data-ttu-id="422e1-159">Vytvořte nástroj pro vyrovnávání zatížení hello pomocí hello dříve vytvořené objekty.</span><span class="sxs-lookup"><span data-stu-id="422e1-159">Create hello load balancer using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-hello-back-end-vms"></a><span data-ttu-id="422e1-160">Vytvoření síťové adaptéry pro hello back-end virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="422e1-160">Create NICs for hello back-end VMs</span></span>

1. <span data-ttu-id="422e1-161">Získat hello virtuální síť a podsíť virtuální sítě, kde hello síťové adaptéry musí toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="422e1-161">Get hello Virtual Network and Virtual Network Subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="422e1-162">Vytvoření konfigurace protokolu IP a síťové adaptéry pro virtuální počítače hello.</span><span class="sxs-lookup"><span data-stu-id="422e1-162">Create IP configurations and NICs for hello VMs.</span></span>

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-hello-newly-created-nics"></a><span data-ttu-id="422e1-163">Vytváření virtuálních počítačů a přiřaďte hello nově vytvořený síťové karty</span><span class="sxs-lookup"><span data-stu-id="422e1-163">Create virtual machines and assign hello newly created NICs</span></span>

<span data-ttu-id="422e1-164">Další informace o vytvoření virtuálního počítače najdete v tématu [vytvořte a předem nakonfigurujte virtuálního počítače s Windows pomocí Resource Manageru a prostředí Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="422e1-164">For more information about creating a VM, see [Create and preconfigure a Windows Virtual Machine with Resource Manager and Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="422e1-165">Vytvoření účtu skupiny dostupnosti a úložiště</span><span class="sxs-lookup"><span data-stu-id="422e1-165">Create an Availability Set and Storage account</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. <span data-ttu-id="422e1-166">Každý virtuální počítač vytvořit a přiřadit hello předchozího vytvořeny síťové karty</span><span class="sxs-lookup"><span data-stu-id="422e1-166">Create each VM and assign hello previous created NICs</span></span>

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type hello username and password of hello local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a><span data-ttu-id="422e1-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="422e1-167">Next steps</span></span>

[<span data-ttu-id="422e1-168">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="422e1-168">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="422e1-169">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="422e1-169">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="422e1-170">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="422e1-170">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
