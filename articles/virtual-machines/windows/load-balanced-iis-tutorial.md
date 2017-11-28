---
title: "aaaTutorial - sestavení vysokou dostupnost aplikací na virtuálních počítačích Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate vysoká dostupnost a zabezpečení aplikací mezi tři virtuální počítače Windows se nástroj pro vyrovnávání zatížení v Azure"
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
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="b60cd-103">Sestavení zatížení vyrovnáváním, vysokou dostupnost aplikací na virtuálních počítačích s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="b60cd-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="b60cd-104">V tomto kurzu vytvoříte vysoce dostupné aplikace, která je odolný toomaintenance události.</span><span class="sxs-lookup"><span data-stu-id="b60cd-104">In this tutorial, you create a highly available application that is resilient toomaintenance events.</span></span> <span data-ttu-id="b60cd-105">aplikace Hello používá nástroj pro vyrovnávání zatížení, skupinu dostupnosti a tři Windows virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="b60cd-105">hello app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="b60cd-106">V tomto kurzu nainstaluje službu IIS, i když můžete použít tento kurz toodeploy framework jinou aplikaci pomocí hello stejné komponenty vysokou dostupnost a pokyny.</span><span class="sxs-lookup"><span data-stu-id="b60cd-106">This tutorial installs IIS, though you can use this tutorial toodeploy a different application framework using hello same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="b60cd-107">Krok 1 – požadavky Azure</span><span class="sxs-lookup"><span data-stu-id="b60cd-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="b60cd-108">toocomplete tohoto kurzu, ujistěte se, že jste nainstalovali hello nejnovější [prostředí Azure PowerShell](/powershell/azure/overview) modulu.</span><span class="sxs-lookup"><span data-stu-id="b60cd-108">toocomplete this tutorial, make sure that you have installed hello latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="b60cd-109">Nejdřív přihlásit tooyour předplatné s hello příkaz Login-AzureRmAccount a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-109">First, log in tooyour Azure subscription with hello Login-AzureRmAccount command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="b60cd-110">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="b60cd-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="b60cd-111">Před vytvořením veškeré prostředky Azure, je nutné toocreate skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="b60cd-111">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="b60cd-112">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` oblasti:</span><span class="sxs-lookup"><span data-stu-id="b60cd-112">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="b60cd-113">Krok 2 – Vytvoření sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b60cd-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="b60cd-114">Virtuální počítače lze vytvořit v logické chyby a aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="b60cd-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="b60cd-115">Každé logické domény představuje část hardwaru v datovém základní Azure hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-115">Each logical domain represents a portion of hardware in hello underlying Azure datacenter.</span></span> <span data-ttu-id="b60cd-116">Když vytvoříte dva nebo více virtuálních počítačů, výpočetní a úložnou kapacitu jsou rozmístěny v těchto doménách.</span><span class="sxs-lookup"><span data-stu-id="b60cd-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="b60cd-117">Tento distribuční udržuje hello dostupnosti vaší aplikace, pokud hardwarová komponenta potřebuje údržby.</span><span class="sxs-lookup"><span data-stu-id="b60cd-117">This distribution maintains hello availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="b60cd-118">Skupiny dostupnosti umožňují definovat tyto logické domény selhání a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b60cd-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="b60cd-119">Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="b60cd-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="b60cd-120">Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-120">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="b60cd-121">Krok 3 – vytvoření zatížení vyrovnávání</span><span class="sxs-lookup"><span data-stu-id="b60cd-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="b60cd-122">K nástroji pro vyrovnávání zatížení Azure rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b60cd-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="b60cd-123">Test stavu monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom provoz tooan provozní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-123">A health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="b60cd-124">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="b60cd-124">Create public IP address</span></span>

<span data-ttu-id="b60cd-125">tooaccess vaši aplikaci ve hello Internetu, přiřaďte veřejnou IP adresu toohello služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b60cd-125">tooaccess your app on hello Internet, assign a public IP address toohello load balancer.</span></span> <span data-ttu-id="b60cd-126">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="b60cd-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="b60cd-127">Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-127">hello following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="b60cd-128">Vytvořit nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b60cd-128">Create load balancer</span></span>

<span data-ttu-id="b60cd-129">Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="b60cd-130">Hello následující příklad vytvoří na front-endovou IP adresu s názvem `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-130">hello following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="b60cd-131">Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="b60cd-132">Hello následující příklad vytvoří fond back-end adresy s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-132">hello following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="b60cd-133">Teď vytvořte hello Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="b60cd-133">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="b60cd-134">Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer` pomocí hello `myPublicIP` adresa:</span><span class="sxs-lookup"><span data-stu-id="b60cd-134">hello following example creates a load balancer named `myLoadBalancer` using hello `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="b60cd-135">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="b60cd-135">Create health probe</span></span>

<span data-ttu-id="b60cd-136">tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="b60cd-136">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="b60cd-137">Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b60cd-137">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="b60cd-138">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="b60cd-138">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="b60cd-139">Vytvoření test stavu s [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="b60cd-140">Hello následující příklad vytvoří sondu stavu s názvem `myHealthProbe` který monitoruje každý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="b60cd-140">hello following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="b60cd-141">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b60cd-141">Create load balancer rule</span></span>

<span data-ttu-id="b60cd-142">Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-142">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span>

<span data-ttu-id="b60cd-143">Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="b60cd-144">Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRule` a vyrovnává přenosy na portu `80`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-144">hello following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="b60cd-145">Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="b60cd-145">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="b60cd-146">Krok 4 – konfigurace sítí</span><span class="sxs-lookup"><span data-stu-id="b60cd-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="b60cd-147">Každý virtuální počítač má jeden nebo více virtuálních síťových karet (NIC) připojujících tooa virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b60cd-147">Each VM has one or more virtual network interface cards (NICs) that connect tooa virtual network.</span></span> <span data-ttu-id="b60cd-148">Tato virtuální síť, je zabezpečena toofilter provozu založené na definovaných pravidel přístupu.</span><span class="sxs-lookup"><span data-stu-id="b60cd-148">This virtual network is secured toofilter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="b60cd-149">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b60cd-149">Create virtual network</span></span>

<span data-ttu-id="b60cd-150">Nejprve nakonfigurujte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="b60cd-151">Hello následující příklad vytvoří podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-151">hello following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="b60cd-152">tooprovide síťové připojení tooyour virtuální počítače, vytvořte virtuální síť s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="b60cd-152">tooprovide network connectivity tooyour VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="b60cd-153">Hello následující příklad vytvoří virtuální síť s názvem `myVnet` s `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-153">hello following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="b60cd-154">Konfigurace zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="b60cd-154">Configure network security</span></span>

<span data-ttu-id="b60cd-155">Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="b60cd-156">Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="b60cd-157">Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="b60cd-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="b60cd-158">tooallow tooreach provoz webové aplikace, vytvořte pravidlo skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="b60cd-158">tooallow web traffic tooreach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="b60cd-159">Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-159">hello following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

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

<span data-ttu-id="b60cd-160">Vytvořit skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="b60cd-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="b60cd-161">Hello následující příklad vytvoří skupinu NSG s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="b60cd-161">hello following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="b60cd-162">Přidat hello toohello podsíť skupiny zabezpečení sítě s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="b60cd-162">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="b60cd-163">Aktualizace hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="b60cd-163">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="b60cd-164">Vytvořit virtuální síťové karty</span><span class="sxs-lookup"><span data-stu-id="b60cd-164">Create virtual network interface cards</span></span>

<span data-ttu-id="b60cd-165">Funkce hello virtuální síťový adaptér prostředků nástroje pro vyrovnávání zatížení a nikoli hello skutečné virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-165">Load balancers function with hello virtual NIC resource rather than hello actual VM.</span></span> <span data-ttu-id="b60cd-166">Hello virtuální síťovou kartu připojené toohello služba Vyrovnávání zatížení a pak připojit tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-166">hello virtual NIC is connected toohello load balancer, and then attached tooa VM.</span></span>

<span data-ttu-id="b60cd-167">Vytvořit virtuální síťovou kartu s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="b60cd-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="b60cd-168">Hello následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="b60cd-168">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="b60cd-169">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky):</span><span class="sxs-lookup"><span data-stu-id="b60cd-169">(One virtual NIC for each VM you create for your app in hello following steps):</span></span>


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

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="b60cd-170">Krok 5 – vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b60cd-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="b60cd-171">S všechny hello základní součásti v místě nyní můžete vytvořit vysoce dostupné virtuální počítače toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b60cd-171">With all hello underlying components in place, you can now create highly available VMs toorun your app.</span></span> 

<span data-ttu-id="b60cd-172">Získat hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuální počítač s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="b60cd-172">Get hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="b60cd-173">Vytvoření hello virtuálních počítačů s [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Přidat AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="b60cd-173">Create hello VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="b60cd-174">Hello následující ukázka vytvoří tři virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="b60cd-174">hello following example creates three VMs:</span></span>

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

<span data-ttu-id="b60cd-175">Trvá několik minut toocreate a nakonfigurovat všechny tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b60cd-175">It takes several minutes toocreate and configure all three VMs.</span></span> <span data-ttu-id="b60cd-176">Hello test stavu nástroje pro vyrovnávání zatížení automaticky zjišťuje, když na každém virtuálním počítači běží aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-176">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="b60cd-177">Jakmile hello aplikace běží, se spustí pravidlo Vyrovnávání zatížení hello toodistribute provoz.</span><span class="sxs-lookup"><span data-stu-id="b60cd-177">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>

### <a name="install-hello-app"></a><span data-ttu-id="b60cd-178">Instalace aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b60cd-178">Install hello app</span></span> 

<span data-ttu-id="b60cd-179">Rozšíření virtuálního počítače Azure jsou úlohy konfigurace virtuálního počítače používané tooautomate jako je instalace aplikací a konfigurace hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b60cd-179">Azure virtual machine extensions are used tooautomate virtual machine configuration tasks such as installing applications and configuring hello operating system.</span></span> <span data-ttu-id="b60cd-180">Hello [rozšíření vlastních skriptů pro systém Windows](./../virtual-machines-windows-extensions-customscript.md) je použité toorun všech skriptů prostředí PowerShell na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-180">hello [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used toorun any PowerShell script on hello virtual machine.</span></span> <span data-ttu-id="b60cd-181">Hello skriptu můžete uložené v úložišti Azure, žádný dostupný koncový bod HTTP nebo vložené v konfiguraci rozšíření vlastních skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-181">hello script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in hello custom script extension configuration.</span></span> <span data-ttu-id="b60cd-182">Pokud používáte rozšíření vlastních skriptů hello, spravuje agenta virtuálního počítače Azure hello hello provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="b60cd-182">When using hello custom script extension, hello Azure VM agent manages hello script execution.</span></span>

<span data-ttu-id="b60cd-183">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) rozšíření vlastních skriptů tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello custom script extension.</span></span> <span data-ttu-id="b60cd-184">Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` webový server IIS tooinstall hello:</span><span class="sxs-lookup"><span data-stu-id="b60cd-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver:</span></span>

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

### <a name="test-your-app"></a><span data-ttu-id="b60cd-185">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="b60cd-185">Test your app</span></span>

<span data-ttu-id="b60cd-186">Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="b60cd-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="b60cd-187">Hello následující příklad získá hello IP adresu pro `myPublicIP` vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="b60cd-187">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="b60cd-188">Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b60cd-188">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="b60cd-189">S hello pravidla NSG v místě se zobrazí výchozí web služby IIS hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-189">With hello NSG rule in place, hello default IIS website is displayed.</span></span> 

![Výchozí web služby IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="b60cd-191">Krok 6 – úlohy správy</span><span class="sxs-lookup"><span data-stu-id="b60cd-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="b60cd-192">Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b60cd-192">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="b60cd-193">toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b60cd-193">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="b60cd-194">V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-194">This section shows you how tooremove or add a VM from hello load balancer.</span></span> 

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="b60cd-195">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="b60cd-195">Remove a VM from hello load balancer</span></span>

<span data-ttu-id="b60cd-196">Virtuální počítač odeberte z fondu adres back-end hello resetováním hello pravidlo LoadBalancerBackendAddressPools vlastnost hello karty síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b60cd-196">Remove a VM from hello backend address pool by resetting hello LoadBalancerBackendAddressPools property of hello network interface card.</span></span>

<span data-ttu-id="b60cd-197">Získat hello síťová karta s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="b60cd-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="b60cd-198">Nastaví vlastnost pravidlo LoadBalancerBackendAddressPools hello hello síťová karta příliš$ null:</span><span class="sxs-lookup"><span data-stu-id="b60cd-198">Set hello LoadBalancerBackendAddressPools property of hello network interface card too$null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="b60cd-199">Aktualizace hello síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="b60cd-199">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="b60cd-200">Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b60cd-200">Add a VM toohello load balancer</span></span>

<span data-ttu-id="b60cd-201">Po provedení údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, přidání hello síťový adaptér virtuálních počítačů toohello back-end fondu adres nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="b60cd-201">After performing VM maintenance, or if you need tooexpand capacity, adding hello NIC of a VM toohello backend address pool of hello load balancer.</span></span>

<span data-ttu-id="b60cd-202">Získejte nástroj pro vyrovnávání zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="b60cd-202">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="b60cd-203">Přidejte fond adres back-end hello hello zatížení vyrovnávání toohello síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="b60cd-203">Add hello backend address pool of hello load balancer toohello network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="b60cd-204">Aktualizace hello síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="b60cd-204">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="b60cd-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b60cd-205">Next Steps</span></span>

<span data-ttu-id="b60cd-206">Ukázky – [ukázkové skripty prostředí Azure PowerShell virtuálního počítače](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="b60cd-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
