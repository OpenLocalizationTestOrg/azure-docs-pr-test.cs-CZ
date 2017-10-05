---
title: "Vytvoření Vyrovnávání zatížení Azure internetové s IPv6 – prostředí PowerShell | Microsoft Docs"
description: "Naučte se vytvářet internetovým Vyrovnávání zatížení s IPv6 pomocí prostředí PowerShell pro Resource Manager"
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
ms.openlocfilehash: 9d3cd37d3f2912301904b0a35f6fbc978d173079
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a><span data-ttu-id="4d937-104">Začínáte s vytvářením internetovým Vyrovnávání zatížení s IPv6 pomocí prostředí PowerShell pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4d937-104">Get started creating an Internet facing load balancer with IPv6 using PowerShell for Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d937-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d937-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="4d937-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4d937-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="4d937-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="4d937-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="4d937-108">Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="4d937-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="4d937-109">Nástroj pro vyrovnávání zatížení poskytuje vysokou dostupnost díky distribuci příchozích přenosů mezi instance služeb, které jsou v pořádku, v cloudových službách nebo virtuálních počítačích v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-109">The load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="4d937-110">Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.</span><span class="sxs-lookup"><span data-stu-id="4d937-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="4d937-111">Příklad scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="4d937-111">Example deployment scenario</span></span>

<span data-ttu-id="4d937-112">Následující diagram znázorňuje nasazení v tomto článku řešení vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-112">The following diagram illustrates the load balancing solution being deployed in this article.</span></span>

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

<span data-ttu-id="4d937-114">V tomto scénáři vytvoříte následující prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="4d937-114">In this scenario you will create the following Azure resources:</span></span>

* <span data-ttu-id="4d937-115">Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="4d937-115">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="4d937-116">dvě pravidla vyrovnávání mapovat veřejné virtuální privátní koncové body zatížení</span><span class="sxs-lookup"><span data-stu-id="4d937-116">two load balancing rules to map the public VIPs to the private endpoints</span></span>
* <span data-ttu-id="4d937-117">Skupiny dostupnosti pro, který obsahuje dva virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4d937-117">an Availability Set to that contains the two VMs</span></span>
* <span data-ttu-id="4d937-118">dva virtuální počítače (VM)</span><span class="sxs-lookup"><span data-stu-id="4d937-118">two virtual machines (VMs)</span></span>
* <span data-ttu-id="4d937-119">rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené</span><span class="sxs-lookup"><span data-stu-id="4d937-119">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>

## <a name="deploying-the-solution-using-the-azure-powershell"></a><span data-ttu-id="4d937-120">Nasazení řešení pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d937-120">Deploying the solution using the Azure PowerShell</span></span>

<span data-ttu-id="4d937-121">Následující kroky ukazují, jak vytvořit internetovým pomocí Azure Resource Manager pomocí prostředí PowerShell nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-121">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="4d937-122">S Azure Resource Manager, všechny prostředky se vytvoří a konfigurovat individuálně, potom put dohromady a vytvoří prostředek.</span><span class="sxs-lookup"><span data-stu-id="4d937-122">With Azure Resource Manager, each resource is created and configured individually, then put together to create a resource.</span></span>

<span data-ttu-id="4d937-123">Pokud chcete nasadit nástroj pro vyrovnávání zatížení, vytvořte a nakonfigurujte následující objekty:</span><span class="sxs-lookup"><span data-stu-id="4d937-123">To deploy a load balancer, you create and configure the following objects:</span></span>

* <span data-ttu-id="4d937-124">Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="4d937-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="4d937-125">Back-endový fond adres – obsahuje síťová rozhraní, pomocí kterých virtuální počítače přijímají síťový provoz z nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-125">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="4d937-126">Pravidla vyrovnávání zatížení – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="4d937-126">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="4d937-127">Pravidla příchozího překladu adres (NAT) – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port konkrétního virtuálního počítače v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="4d937-127">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="4d937-128">Testy – obsahuje testy stavu sloužící ke kontrole dostupnosti instancí virtuálních počítačů v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="4d937-128">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="4d937-129">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="4d937-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-to-use-resource-manager"></a><span data-ttu-id="4d937-130">Nastavení prostředí PowerShell pro použití Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4d937-130">Set up PowerShell to use Resource Manager</span></span>

<span data-ttu-id="4d937-131">Ujistěte se, že máte nejnovější produkční verzi modulu Azure Resource Manager pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4d937-131">Make sure you have the latest production version of the Azure Resource Manager module for PowerShell.</span></span>

1. <span data-ttu-id="4d937-132">Přihlaste se k Azure</span><span class="sxs-lookup"><span data-stu-id="4d937-132">Sign into Azure</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="4d937-133">Po zobrazení výzvy zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4d937-133">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="4d937-134">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="4d937-134">Check the subscriptions for the account</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="4d937-135">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4d937-135">Choose which of your Azure subscriptions to use.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="4d937-136">Vytvořte skupinu prostředků (Přeskočit tento krok, pokud se používá existující skupina prostředků)</span><span class="sxs-lookup"><span data-stu-id="4d937-136">Create a resource group (skip this step if using an existing resource group)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a><span data-ttu-id="4d937-137">Vytvoření virtuální sítě a veřejné IP adresy pro front-endový fond IP adres</span><span class="sxs-lookup"><span data-stu-id="4d937-137">Create a virtual network and a public IP address for the front-end IP pool</span></span>

1. <span data-ttu-id="4d937-138">Vytvořte virtuální síť s podsítí.</span><span class="sxs-lookup"><span data-stu-id="4d937-138">Create a virtual network with a subnet.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="4d937-139">Vytvořte Azure veřejná IP adresa (PIP) prostředky pro front-endu fond IP adres.</span><span class="sxs-lookup"><span data-stu-id="4d937-139">Create Azure Public IP address (PIP) resources for the front-end IP address pool.</span></span>

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="4d937-140">Nástroje pro vyrovnávání zatížení používá popisek domény veřejné IP adresy jako předpona pro jeho plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="4d937-140">The load balancer uses the domain label of the public IP as prefix for its FQDN.</span></span> <span data-ttu-id="4d937-141">V tomto příkladu jsou plně kvalifikované domény *lbnrpipv4.westus.cloudapp.azure.com* a *lbnrpipv6.westus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="4d937-141">In this example, the FQDNs are *lbnrpipv4.westus.cloudapp.azure.com* and *lbnrpipv6.westus.cloudapp.azure.com*.</span></span>

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a><span data-ttu-id="4d937-142">Vytvoření konfigurace IP front-endový a fond Back-End adresy</span><span class="sxs-lookup"><span data-stu-id="4d937-142">Create a Front-End IP configurations and a Back-End Address Pool</span></span>

1. <span data-ttu-id="4d937-143">Vytvořte konfiguraci adresa front-endu, který používá veřejné IP adresy, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4d937-143">Create front-end address configuration that uses the Public IP addresses you created.</span></span>

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. <span data-ttu-id="4d937-144">Vytvořte fondy adres back-end.</span><span class="sxs-lookup"><span data-stu-id="4d937-144">Create back-end address pools.</span></span>

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a><span data-ttu-id="4d937-145">Vytvoření pravidla LB, pravidla NAT, sondu a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="4d937-145">Create LB rules, NAT rules, a probe, and a load balancer</span></span>

<span data-ttu-id="4d937-146">Tento příklad vytvoří následující položky:</span><span class="sxs-lookup"><span data-stu-id="4d937-146">This example creates the following items:</span></span>

* <span data-ttu-id="4d937-147">pravidlo NAT přeložit všechny příchozí přenosy na portu 443 na port 4443</span><span class="sxs-lookup"><span data-stu-id="4d937-147">a NAT rule to translate all incoming traffic on port 443 to port 4443</span></span>
* <span data-ttu-id="4d937-148">Pravidlo nástroje pro vyrovnávání zatížení, které vyrovnává zatížení veškerého příchozího provozu na portu 80 na port 80 na adresách v back-endovém fondu.</span><span class="sxs-lookup"><span data-stu-id="4d937-148">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>
* <span data-ttu-id="4d937-149">pravidlo Vyrovnávání zatížení povolit připojení RDP k virtuálním počítačům na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="4d937-149">a load balancer rule to allow RDP connection to the VMs on port 3389.</span></span>
* <span data-ttu-id="4d937-150">pravidlo testu chcete zkontrolovat stav na stránku s názvem *HealthProbe.aspx* nebo službu na portu 8080</span><span class="sxs-lookup"><span data-stu-id="4d937-150">a probe rule to check the health status on a page named *HealthProbe.aspx* or a service on port 8080</span></span>
* <span data-ttu-id="4d937-151">Vyrovnávání zatížení, která používá tyto objekty</span><span class="sxs-lookup"><span data-stu-id="4d937-151">a load balancer that uses all these objects</span></span>

1. <span data-ttu-id="4d937-152">Vytvořte pravidla překladu adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="4d937-152">Create the NAT rules.</span></span>

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. <span data-ttu-id="4d937-153">Vytvořte test stavu.</span><span class="sxs-lookup"><span data-stu-id="4d937-153">Create a health probe.</span></span> <span data-ttu-id="4d937-154">Test můžete nakonfigurovat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="4d937-154">There are two ways to configure a probe:</span></span>

    <span data-ttu-id="4d937-155">Test protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="4d937-155">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="4d937-156">nebo TCP testu</span><span class="sxs-lookup"><span data-stu-id="4d937-156">or TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="4d937-157">V tomto příkladu budeme používat sondy TCP.</span><span class="sxs-lookup"><span data-stu-id="4d937-157">For this example, we are going to use the TCP probes.</span></span>

3. <span data-ttu-id="4d937-158">Vytvořte pravidlo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-158">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. <span data-ttu-id="4d937-159">Vytvořte objekty vytvořené pomocí nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4d937-159">Create the load balancer using the previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a><span data-ttu-id="4d937-160">Vytvoření síťové adaptéry pro virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="4d937-160">Create NICs for the back-end VMs</span></span>

1. <span data-ttu-id="4d937-161">Získáte virtuální síť a podsíť virtuální sítě, kdy je potřeba vytvořit síťovými adaptéry.</span><span class="sxs-lookup"><span data-stu-id="4d937-161">Get the Virtual Network and Virtual Network Subnet, where the NICs need to be created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="4d937-162">Vytvoření konfigurace protokolu IP a síťové adaptéry pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4d937-162">Create IP configurations and NICs for the VMs.</span></span>

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a><span data-ttu-id="4d937-163">Vytváření virtuálních počítačů a přiřaďte nově vytvořený síťové karty</span><span class="sxs-lookup"><span data-stu-id="4d937-163">Create virtual machines and assign the newly created NICs</span></span>

<span data-ttu-id="4d937-164">Další informace o vytvoření virtuálního počítače najdete v tématu [vytvořte a předem nakonfigurujte virtuálního počítače s Windows pomocí Resource Manageru a prostředí Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="4d937-164">For more information about creating a VM, see [Create and preconfigure a Windows Virtual Machine with Resource Manager and Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="4d937-165">Vytvoření účtu skupiny dostupnosti a úložiště</span><span class="sxs-lookup"><span data-stu-id="4d937-165">Create an Availability Set and Storage account</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. <span data-ttu-id="4d937-166">Každý virtuální počítač vytvořit a přiřadit předchozí vytvořili síťové karty</span><span class="sxs-lookup"><span data-stu-id="4d937-166">Create each VM and assign the previous created NICs</span></span>

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

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

## <a name="next-steps"></a><span data-ttu-id="4d937-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d937-167">Next steps</span></span>

[<span data-ttu-id="4d937-168">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="4d937-168">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="4d937-169">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="4d937-169">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="4d937-170">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="4d937-170">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
