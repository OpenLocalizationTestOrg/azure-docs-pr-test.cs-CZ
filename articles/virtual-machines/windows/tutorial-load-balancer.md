---
title: "Jak načíst vyvážit virtuální počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o použití nástroje pro vyrovnávání zatížení Azure k vytvoření vysoce dostupné a zabezpečené aplikace napříč tři virtuální počítače Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6738d88d5a0430abaf3855dbf97a618e4c83617f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a><span data-ttu-id="2e3cd-103">Jak načíst vyvážit virtuální počítače s Windows v Azure k vytvoření vysoce dostupné aplikace</span><span class="sxs-lookup"><span data-stu-id="2e3cd-103">How to load balance Windows virtual machines in Azure to create a highly available application</span></span>
<span data-ttu-id="2e3cd-104">Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="2e3cd-105">V tomto kurzu informace o různé součásti nástroje pro vyrovnávání zatížení Azure, které distribuci přenosů a zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-105">In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="2e3cd-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2e3cd-107">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2e3cd-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="2e3cd-108">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="2e3cd-109">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="2e3cd-110">Použití rozšíření vlastních skriptů pro vytvoření základní webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="2e3cd-110">Use the Custom Script Extension to create a basic IIS site</span></span>
> * <span data-ttu-id="2e3cd-111">Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-111">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="2e3cd-112">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="2e3cd-112">View a load balancer in action</span></span>
> * <span data-ttu-id="2e3cd-113">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-113">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="2e3cd-114">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-114">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="2e3cd-115">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-115">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="2e3cd-116">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-116">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="azure-load-balancer-overview"></a><span data-ttu-id="2e3cd-117">Přehled nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2e3cd-117">Azure load balancer overview</span></span>
<span data-ttu-id="2e3cd-118">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="2e3cd-119">Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-119">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

<span data-ttu-id="2e3cd-120">Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="2e3cd-121">Tuto konfiguraci front-end IP adresy umožňuje Vyrovnávání zatížení a aplikace přístupné přes Internet.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-121">This front-end IP configuration allows your load balancer and applications to be accessible over the Internet.</span></span> 

<span data-ttu-id="2e3cd-122">Virtuální počítače připojit k nástroji pro vyrovnávání zatížení pomocí jejich virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-122">Virtual machines connect to a load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="2e3cd-123">K distribuci provoz na virtuální počítače, fond back-end adres obsahuje IP adresy virtuální (NIC) připojené ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-123">To distribute traffic to the VMs, a back-end address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.</span></span>

<span data-ttu-id="2e3cd-124">Pokud chcete řídit tok přenosů dat, definujete pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které jsou mapovány na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-124">To control the flow of traffic, you define load balancer rules for specific ports and protocols that map to your VMs.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="2e3cd-125">Vytvořit nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2e3cd-125">Create Azure load balancer</span></span>
<span data-ttu-id="2e3cd-126">Tato část podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-126">This section details how you can create and configure each component of the load balancer.</span></span> <span data-ttu-id="2e3cd-127">Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-127">Before you can create your load balancer, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2e3cd-128">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-128">The following example creates a resource group named *myResourceGroupLoadBalancer* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="2e3cd-129">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="2e3cd-129">Create a public IP address</span></span>
<span data-ttu-id="2e3cd-130">Pro přístup k vaší aplikace v síti Internet, musíte nástroj pro vyrovnávání zatížení veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-130">To access your app on the Internet, you need a public IP address for the load balancer.</span></span> <span data-ttu-id="2e3cd-131">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-131">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="2e3cd-132">Následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v *myResourceGroupLoadBalancer* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-132">The following example creates a public IP address named *myPublicIP* in the *myResourceGroupLoadBalancer* resource group:</span></span>

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="2e3cd-133">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-133">Create a load balancer</span></span>
<span data-ttu-id="2e3cd-134">Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-134">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="2e3cd-135">Následující příklad vytvoří na front-endovou IP adresu s názvem *myFrontEndPool*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-135">The following example creates a frontend IP address named *myFrontEndPool*:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

<span data-ttu-id="2e3cd-136">Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-136">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="2e3cd-137">Následující příklad vytvoří fond back-end adresy s názvem *myBackEndPool*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-137">The following example creates a backend address pool named *myBackEndPool*:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="2e3cd-138">Nyní, vytvoří se službou Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-138">Now, create the load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="2e3cd-139">Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* pomocí *myPublicIP* adresa:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-139">The following example creates a load balancer named *myLoadBalancer* using the *myPublicIP* address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a><span data-ttu-id="2e3cd-140">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="2e3cd-140">Create a health probe</span></span>
<span data-ttu-id="2e3cd-141">Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-141">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="2e3cd-142">Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-142">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="2e3cd-143">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-143">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="2e3cd-144">Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-144">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="2e3cd-145">Následující příklad vytvoří sondou TCP.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-145">The following example creates a TCP probe.</span></span> <span data-ttu-id="2e3cd-146">Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-146">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="2e3cd-147">Pokud používáte vlastní sondu HTTP, musíte vytvořit stránka pro kontrolu stavu, jako například *healthcheck.aspx*.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-147">When using a custom HTTP probe, you must create the health check page, such as *healthcheck.aspx*.</span></span> <span data-ttu-id="2e3cd-148">Sondy musí vrátit **HTTP 200 OK** odpovědi pro vyrovnávání zatížení na hostiteli mějte otočení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-148">The probe must return an **HTTP 200 OK** response for the load balancer to keep the host in rotation.</span></span>

<span data-ttu-id="2e3cd-149">K vytvoření stavu sondou TCP, použijete [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-149">To create a TCP health probe, you use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="2e3cd-150">Následující příklad vytvoří kontrolu stavu s názvem *myHealthProbe* který monitoruje každý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-150">The following example creates a health probe named *myHealthProbe* that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

<span data-ttu-id="2e3cd-151">Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="2e3cd-151">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="2e3cd-152">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-152">Create a load balancer rule</span></span>
<span data-ttu-id="2e3cd-153">Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-153">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span> <span data-ttu-id="2e3cd-154">Můžete definovat front-endové konfiguraci protokolu IP pro příchozí provoz a fond back-end IP příjem provozu, společně s požadovaný zdrojový a cílový port.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-154">You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.</span></span> <span data-ttu-id="2e3cd-155">Pokud chcete mít jistotu, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat test stavu použít.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-155">To make sure only healthy VMs receive traffic, you also define the health probe to use.</span></span>

<span data-ttu-id="2e3cd-156">Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-156">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="2e3cd-157">Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem *myLoadBalancerRule* a vyrovnává přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-157">The following example creates a load balancer rule named *myLoadBalancerRule* and balances traffic on port *80*:</span></span>

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

<span data-ttu-id="2e3cd-158">Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="2e3cd-158">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a><span data-ttu-id="2e3cd-159">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2e3cd-159">Configure virtual network</span></span>
<span data-ttu-id="2e3cd-160">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-160">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="2e3cd-161">Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-161">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="2e3cd-162">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="2e3cd-162">Create network resources</span></span>
<span data-ttu-id="2e3cd-163">Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-163">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="2e3cd-164">Následující příklad vytvoří virtuální síť s názvem *myVnet* s *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-164">The following example creates a virtual network named *myVnet* with *mySubnet*:</span></span>

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

<span data-ttu-id="2e3cd-165">Vytvoření pravidla skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), pak vytvořte skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-165">Create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), then create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="2e3cd-166">Přidat skupinu zabezpečení sítě pro podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) a aktualizujte virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-166">Add the network security group to the subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) and then update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span></span> 

<span data-ttu-id="2e3cd-167">Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroup* a použije je k *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-167">The following example creates a network security group rule named *myNetworkSecurityGroup* and applies it to *mySubnet*:</span></span>

```powershell
# Create security rule config
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

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

<span data-ttu-id="2e3cd-168">Virtuální síťové adaptéry jsou vytvořeny pomocí [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-168">Virtual NICs are created with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="2e3cd-169">Následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-169">The following example creates three virtual NICs.</span></span> <span data-ttu-id="2e3cd-170">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-170">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="2e3cd-171">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-171">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a><span data-ttu-id="2e3cd-172">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2e3cd-172">Create virtual machines</span></span>
<span data-ttu-id="2e3cd-173">Pokud chcete zvýšit vysokou dostupnost vaší aplikace, umístíte virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-173">To improve the high availability of your app, place your VMs in an availability set.</span></span>

<span data-ttu-id="2e3cd-174">Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-174">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="2e3cd-175">Následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-175">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

<span data-ttu-id="2e3cd-176">Nastavte správce uživatelské jméno a heslo pro virtuální počítače s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="2e3cd-176">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="2e3cd-177">Nyní můžete vytvořit virtuálních počítačů s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-177">Now you can create the VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="2e3cd-178">Následující příklad vytvoří tři virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-178">The following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

<span data-ttu-id="2e3cd-179">Trvá několik minut vytvořit a nakonfigurovat všechny tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-179">It takes a few minutes to create and configure all three VMs.</span></span>

### <a name="install-iis-with-custom-script-extension"></a><span data-ttu-id="2e3cd-180">Nainstalovat službu ISS s rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="2e3cd-180">Install IIS with Custom Script Extension</span></span>
<span data-ttu-id="2e3cd-181">V předchozích kurz [postup přizpůsobení virtuálního počítače s Windows](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače pomocí ovládacího prvku vlastní skript rozšíření pro Windows.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-181">In a previous tutorial on [How to customize a Windows virtual machine](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with the Custom Script Extension for Windows.</span></span> <span data-ttu-id="2e3cd-182">Stejnou metodu můžete použít k instalaci a konfiguraci služby IIS na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-182">You can use the same approach to install and configure IIS on your VMs.</span></span>

<span data-ttu-id="2e3cd-183">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the Custom Script Extension.</span></span> <span data-ttu-id="2e3cd-184">Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS a aktualizací *Default.htm* stránku a zobrazit název hostitele virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-184">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the *Default.htm* page to show the hostname of the VM:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a><span data-ttu-id="2e3cd-185">Nástroj pro vyrovnávání zatížení testu</span><span class="sxs-lookup"><span data-stu-id="2e3cd-185">Test load balancer</span></span>
<span data-ttu-id="2e3cd-186">Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2e3cd-186">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="2e3cd-187">Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-187">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

<span data-ttu-id="2e3cd-188">Potom můžete zadat veřejnou IP adresu v do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-188">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="2e3cd-189">Zobrazí se na webu, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-189">The website is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Spuštění webu IIS](./media/tutorial-load-balancer/running-iis-website.png)

<span data-ttu-id="2e3cd-191">Nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny tři virtuální počítače spuštěné aplikace najdete můžete můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-191">To see the load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="2e3cd-192">Přidání a odebrání virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2e3cd-192">Add and remove VMs</span></span>
<span data-ttu-id="2e3cd-193">Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-193">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="2e3cd-194">Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-194">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="2e3cd-195">V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-195">This section shows you how to remove or add a VM from the load balancer.</span></span>

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="2e3cd-196">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-196">Remove a VM from the load balancer</span></span>
<span data-ttu-id="2e3cd-197">Získat karty síťového rozhraní s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), nastavte *pravidlo LoadBalancerBackendAddressPools* vlastnost virtuálního síťového adaptéru do *$null*.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-197">Get the network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), then set the *LoadBalancerBackendAddressPools* property of the virtual NIC to *$null*.</span></span> <span data-ttu-id="2e3cd-198">Nakonec aktualizujte virtuální síťový adaptér.:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-198">Finally, update the virtual NIC.:</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="2e3cd-199">Zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-199">To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="2e3cd-200">Nyní můžete provést údržbu na virtuální počítač, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-200">You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="2e3cd-201">Přidat virtuální počítač ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-201">Add a VM to the load balancer</span></span>
<span data-ttu-id="2e3cd-202">Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, nastavte *pravidlo LoadBalancerBackendAddressPools* vlastnost virtuálního síťového adaptéru do *BackendAddressPool* z [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="2e3cd-202">After performing VM maintenance, or if you need to expand capacity, set the *LoadBalancerBackendAddressPools* property of the virtual NIC to the *BackendAddressPool* from [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span></span>

<span data-ttu-id="2e3cd-203">Získáte nástroje pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-203">Get the load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="2e3cd-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e3cd-204">Next steps</span></span>

<span data-ttu-id="2e3cd-205">V tomto kurzu jste vytvořili pro vyrovnávání zatížení a je připojený virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-205">In this tutorial, you created a load balancer and attached VMs to it.</span></span> <span data-ttu-id="2e3cd-206">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="2e3cd-206">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2e3cd-207">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2e3cd-207">Create an Azure load balancer</span></span>
> * <span data-ttu-id="2e3cd-208">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-208">Create a load balancer health probe</span></span>
> * <span data-ttu-id="2e3cd-209">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-209">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="2e3cd-210">Použití rozšíření vlastních skriptů pro vytvoření základní webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="2e3cd-210">Use the Custom Script Extension to create a basic IIS site</span></span>
> * <span data-ttu-id="2e3cd-211">Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-211">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="2e3cd-212">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="2e3cd-212">View a load balancer in action</span></span>
> * <span data-ttu-id="2e3cd-213">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2e3cd-213">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="2e3cd-214">Přechodu na v dalším kurzu se dozvíte, jak ke správě sítí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e3cd-214">Advance to the next tutorial to learn how to manage VM networking.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2e3cd-215">Správa virtuálních počítačů a virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="2e3cd-215">Manage VMs and virtual networks</span></span>](./tutorial-virtual-network.md)
