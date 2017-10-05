---
title: "Kurz – sestavení vysokou dostupnost aplikací na virtuálních počítačích Azure | Microsoft Docs"
description: "Naučte se vytvořit vysoce dostupné a zabezpečení aplikací v rámci tři virtuální počítače Windows se nástroj pro vyrovnávání zatížení v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="3ce20-103">Sestavení zatížení vyrovnáváním, vysokou dostupnost aplikací na virtuálních počítačích s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="3ce20-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="3ce20-104">V tomto kurzu vytvoříte vysoce dostupné aplikace, která je odolné vůči události údržby.</span><span class="sxs-lookup"><span data-stu-id="3ce20-104">In this tutorial, you create a highly available application that is resilient to maintenance events.</span></span> <span data-ttu-id="3ce20-105">Aplikace používá nástroj pro vyrovnávání zatížení, skupinu dostupnosti a tři Windows virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="3ce20-105">The app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="3ce20-106">V tomto kurzu nainstaluje službu IIS, i když v tomto kurzu můžete použít k nasazení jinou aplikaci rozhraní pomocí stejné komponenty vysokou dostupnost a pokyny.</span><span class="sxs-lookup"><span data-stu-id="3ce20-106">This tutorial installs IIS, though you can use this tutorial to deploy a different application framework using the same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="3ce20-107">Krok 1 – požadavky Azure</span><span class="sxs-lookup"><span data-stu-id="3ce20-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="3ce20-108">K dokončení tohoto kurzu, ujistěte se, že jste nainstalovali nejnovější [prostředí Azure PowerShell](/powershell/azure/overview) modulu.</span><span class="sxs-lookup"><span data-stu-id="3ce20-108">To complete this tutorial, make sure that you have installed the latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="3ce20-109">První, přihlaste se k předplatnému Azure pomocí příkazu Login-AzureRmAccount a postupujte podle na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-109">First, log in to your Azure subscription with the Login-AzureRmAccount command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="3ce20-110">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="3ce20-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="3ce20-111">Před vytvořením veškeré prostředky Azure, je nutné vytvořit skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="3ce20-111">Before you can create any other Azure resources, you need to create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="3ce20-112">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westeurope` oblasti:</span><span class="sxs-lookup"><span data-stu-id="3ce20-112">The following example creates a resource group named `myResourceGroup` in the `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="3ce20-113">Krok 2 – Vytvoření sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="3ce20-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="3ce20-114">Virtuální počítače lze vytvořit v logické chyby a aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="3ce20-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="3ce20-115">Každé logické domény představuje část hardwaru v datovém centru základní Azure.</span><span class="sxs-lookup"><span data-stu-id="3ce20-115">Each logical domain represents a portion of hardware in the underlying Azure datacenter.</span></span> <span data-ttu-id="3ce20-116">Když vytvoříte dva nebo více virtuálních počítačů, výpočetní a úložnou kapacitu jsou rozmístěny v těchto doménách.</span><span class="sxs-lookup"><span data-stu-id="3ce20-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="3ce20-117">Tento distribuční udržuje dostupnost aplikace, pokud hardwarová komponenta potřebuje údržby.</span><span class="sxs-lookup"><span data-stu-id="3ce20-117">This distribution maintains the availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="3ce20-118">Skupiny dostupnosti umožňují definovat tyto logické domény selhání a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3ce20-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="3ce20-119">Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="3ce20-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="3ce20-120">Následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-120">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="3ce20-121">Krok 3 – vytvoření zatížení vyrovnávání</span><span class="sxs-lookup"><span data-stu-id="3ce20-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="3ce20-122">K nástroji pro vyrovnávání zatížení Azure rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3ce20-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="3ce20-123">Test stavu monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3ce20-123">A health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="3ce20-124">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="3ce20-124">Create public IP address</span></span>

<span data-ttu-id="3ce20-125">Pro přístup k vaší aplikace na Internetu, přiřaďte veřejnou IP adresu nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3ce20-125">To access your app on the Internet, assign a public IP address to the load balancer.</span></span> <span data-ttu-id="3ce20-126">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="3ce20-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="3ce20-127">Následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-127">The following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="3ce20-128">Vytvořit nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3ce20-128">Create load balancer</span></span>

<span data-ttu-id="3ce20-129">Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="3ce20-130">Následující příklad vytvoří na front-endovou IP adresu s názvem `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-130">The following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="3ce20-131">Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="3ce20-132">Následující příklad vytvoří fond back-end adresy s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-132">The following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="3ce20-133">Nyní, vytvoří se službou Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="3ce20-133">Now, create the load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="3ce20-134">Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer` pomocí `myPublicIP` adresa:</span><span class="sxs-lookup"><span data-stu-id="3ce20-134">The following example creates a load balancer named `myLoadBalancer` using the `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="3ce20-135">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="3ce20-135">Create health probe</span></span>

<span data-ttu-id="3ce20-136">Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="3ce20-136">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="3ce20-137">Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="3ce20-137">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="3ce20-138">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="3ce20-138">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="3ce20-139">Vytvoření test stavu s [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="3ce20-140">Následující příklad vytvoří kontrolu stavu s názvem `myHealthProbe` který monitoruje každý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="3ce20-140">The following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="3ce20-141">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3ce20-141">Create load balancer rule</span></span>

<span data-ttu-id="3ce20-142">Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="3ce20-142">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span>

<span data-ttu-id="3ce20-143">Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="3ce20-144">Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRule` a vyrovnává přenosy na portu `80`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-144">The following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="3ce20-145">Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="3ce20-145">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="3ce20-146">Krok 4 – konfigurace sítí</span><span class="sxs-lookup"><span data-stu-id="3ce20-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="3ce20-147">Každý virtuální počítač má jeden nebo více virtuálních síťových karet (NIC) připojených k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="3ce20-147">Each VM has one or more virtual network interface cards (NICs) that connect to a virtual network.</span></span> <span data-ttu-id="3ce20-148">Tato virtuální síť, je zabezpečena k filtrování provozu založené na definovaných pravidel přístupu.</span><span class="sxs-lookup"><span data-stu-id="3ce20-148">This virtual network is secured to filter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="3ce20-149">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="3ce20-149">Create virtual network</span></span>

<span data-ttu-id="3ce20-150">Nejprve nakonfigurujte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="3ce20-151">Následující příklad vytvoří podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-151">The following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="3ce20-152">Chcete-li poskytovat připojení k síti virtuálních počítačů, vytvořte virtuální síť s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="3ce20-152">To provide network connectivity to your VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="3ce20-153">Následující příklad vytvoří virtuální síť s názvem `myVnet` s `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-153">The following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="3ce20-154">Konfigurace zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="3ce20-154">Configure network security</span></span>

<span data-ttu-id="3ce20-155">Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="3ce20-156">Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="3ce20-157">Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="3ce20-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="3ce20-158">Povolit webový provoz k dosažení vaší aplikace, vytvořte pravidlo skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="3ce20-158">To allow web traffic to reach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="3ce20-159">Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-159">The following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

<span data-ttu-id="3ce20-160">Vytvořit skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="3ce20-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="3ce20-161">Následující příklad vytvoří skupinu NSG s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="3ce20-161">The following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="3ce20-162">Přidat skupinu zabezpečení sítě pro podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="3ce20-162">Add the network security group to the subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="3ce20-163">Aktualizace virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="3ce20-163">Update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="3ce20-164">Vytvořit virtuální síťové karty</span><span class="sxs-lookup"><span data-stu-id="3ce20-164">Create virtual network interface cards</span></span>

<span data-ttu-id="3ce20-165">Načíst funkce Vyrovnávání prostředků virtuální síťový adaptér, nikoli skutečné virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3ce20-165">Load balancers function with the virtual NIC resource rather than the actual VM.</span></span> <span data-ttu-id="3ce20-166">Virtuální síťový adaptér je připojený ke službě Vyrovnávání zatížení a potom připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="3ce20-166">The virtual NIC is connected to the load balancer, and then attached to a VM.</span></span>

<span data-ttu-id="3ce20-167">Vytvořit virtuální síťovou kartu s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="3ce20-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="3ce20-168">Následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="3ce20-168">The following example creates three virtual NICs.</span></span> <span data-ttu-id="3ce20-169">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích):</span><span class="sxs-lookup"><span data-stu-id="3ce20-169">(One virtual NIC for each VM you create for your app in the following steps):</span></span>


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="3ce20-170">Krok 5 – vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3ce20-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="3ce20-171">S všechny základní součásti v místě nyní můžete vytvořit vysoce dostupné virtuální počítače ke spouštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce20-171">With all the underlying components in place, you can now create highly available VMs to run your app.</span></span> 

<span data-ttu-id="3ce20-172">Uživatelské jméno a heslo, které potřebuje pro účet správce na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="3ce20-172">Get the username and password needed for the administrator account on the virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="3ce20-173">Vytvoření virtuálních počítačů s [nové AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [přidat AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3ce20-173">Create the VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="3ce20-174">Následující příklad vytvoří tři virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="3ce20-174">The following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

<span data-ttu-id="3ce20-175">Trvá několik minut vytvořit a nakonfigurovat všechny tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3ce20-175">It takes several minutes to create and configure all three VMs.</span></span> <span data-ttu-id="3ce20-176">Test stavu nástroje pro vyrovnávání zatížení automaticky rozpozná, když aplikace běží na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="3ce20-176">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="3ce20-177">Jakmile aplikace běží, spustí se pravidlo Vyrovnávání zatížení k distribuci přenosů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-177">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>

### <a name="install-the-app"></a><span data-ttu-id="3ce20-178">Nainstalujte aplikaci</span><span class="sxs-lookup"><span data-stu-id="3ce20-178">Install the app</span></span> 

<span data-ttu-id="3ce20-179">Rozšíření virtuálního počítače Azure se používají k automatizaci úloh konfigurace virtuálního počítače jako je instalace aplikací a konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3ce20-179">Azure virtual machine extensions are used to automate virtual machine configuration tasks such as installing applications and configuring the operating system.</span></span> <span data-ttu-id="3ce20-180">[Rozšíření vlastních skriptů pro systém Windows](./../virtual-machines-windows-extensions-customscript.md) se používá ke spuštění všech skriptů prostředí PowerShell na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="3ce20-180">The [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used to run any PowerShell script on the virtual machine.</span></span> <span data-ttu-id="3ce20-181">Skript může uložena v úložišti Azure, žádný dostupný koncový bod HTTP nebo vložené v konfiguraci rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-181">The script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in the custom script extension configuration.</span></span> <span data-ttu-id="3ce20-182">Pokud používáte rozšíření vlastních skriptů, agent virtuálního počítače Azure spravuje provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="3ce20-182">When using the custom script extension, the Azure VM agent manages the script execution.</span></span>

<span data-ttu-id="3ce20-183">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the custom script extension.</span></span> <span data-ttu-id="3ce20-184">Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS:</span><span class="sxs-lookup"><span data-stu-id="3ce20-184">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a><span data-ttu-id="3ce20-185">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="3ce20-185">Test your app</span></span>

<span data-ttu-id="3ce20-186">Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="3ce20-186">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="3ce20-187">Následující příklad, získá IP adresu pro `myPublicIP` vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="3ce20-187">The following example obtains the IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="3ce20-188">Zadejte veřejnou IP adresu ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3ce20-188">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="3ce20-189">Pomocí pravidel NSG na místě se zobrazí výchozí web služby IIS.</span><span class="sxs-lookup"><span data-stu-id="3ce20-189">With the NSG rule in place, the default IIS website is displayed.</span></span> 

![Výchozí web služby IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="3ce20-191">Krok 6 – úlohy správy</span><span class="sxs-lookup"><span data-stu-id="3ce20-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="3ce20-192">Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3ce20-192">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="3ce20-193">Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3ce20-193">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="3ce20-194">V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3ce20-194">This section shows you how to remove or add a VM from the load balancer.</span></span> 

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="3ce20-195">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3ce20-195">Remove a VM from the load balancer</span></span>

<span data-ttu-id="3ce20-196">Virtuální počítač odeberte z fondu adres back-end resetováním vlastnost pravidlo LoadBalancerBackendAddressPools karty síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3ce20-196">Remove a VM from the backend address pool by resetting the LoadBalancerBackendAddressPools property of the network interface card.</span></span>

<span data-ttu-id="3ce20-197">Získat karty síťového rozhraní s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="3ce20-197">Get the network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="3ce20-198">Nastaví vlastnost pravidlo LoadBalancerBackendAddressPools karty síťového rozhraní na $null:</span><span class="sxs-lookup"><span data-stu-id="3ce20-198">Set the LoadBalancerBackendAddressPools property of the network interface card to $null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="3ce20-199">Aktualizace síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3ce20-199">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="3ce20-200">Přidat virtuální počítač ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3ce20-200">Add a VM to the load balancer</span></span>

<span data-ttu-id="3ce20-201">Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, přidání síťový adaptér virtuálního počítače do fondu back-end adresy služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3ce20-201">After performing VM maintenance, or if you need to expand capacity, adding the NIC of a VM to the backend address pool of the load balancer.</span></span>

<span data-ttu-id="3ce20-202">Získáte nástroje pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="3ce20-202">Get the load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="3ce20-203">Back-endových adres nástroje pro vyrovnávání zatížení přidáte do karty síťového rozhraní:</span><span class="sxs-lookup"><span data-stu-id="3ce20-203">Add the backend address pool of the load balancer to the network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="3ce20-204">Aktualizace síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3ce20-204">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="3ce20-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3ce20-205">Next Steps</span></span>

<span data-ttu-id="3ce20-206">Ukázky – [ukázkové skripty prostředí Azure PowerShell virtuálního počítače](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="3ce20-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
