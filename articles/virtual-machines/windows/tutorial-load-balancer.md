---
title: "aaaHow tooload vyvážit virtuálních počítačích s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure zatížení vyrovnávání toocreate vysoká dostupnost a zabezpečení aplikací v rámci tři virtuální počítače Windows"
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
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="9f3a4-103">Jak tooload vyvážit virtuální počítače s Windows v Azure toocreate vysoce dostupné aplikace</span><span class="sxs-lookup"><span data-stu-id="9f3a4-103">How tooload balance Windows virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="9f3a4-104">Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="9f3a4-105">V tomto kurzu informace o hello různé součásti nástroje pro vyrovnávání zatížení Azure hello které distribuci přenosů a zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="9f3a4-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f3a4-107">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="9f3a4-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="9f3a4-108">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="9f3a4-109">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="9f3a4-110">Použít hello rozšíření vlastních skriptů toocreate základní webu IIS</span><span class="sxs-lookup"><span data-stu-id="9f3a4-110">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="9f3a4-111">Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="9f3a4-112">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="9f3a4-112">View a load balancer in action</span></span>
> * <span data-ttu-id="9f3a4-113">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-113">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="9f3a4-114">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-114">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="9f3a4-115">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-115">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="9f3a4-116">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-116">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="azure-load-balancer-overview"></a><span data-ttu-id="9f3a4-117">Přehled nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="9f3a4-117">Azure load balancer overview</span></span>
<span data-ttu-id="9f3a4-118">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="9f3a4-119">Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="9f3a4-120">Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="9f3a4-121">Tuto konfiguraci front-end IP adresy umožňuje vaší zatížení vyrovnávání a aplikace toobe přístupné přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="9f3a4-122">Virtuální počítače připojit nástroj pro vyrovnávání zatížení tooa pomocí jejich virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="9f3a4-123">toodistribute toohello přenosy virtuálních počítačů, fond back-end adres obsahuje hello IP adresy služby Vyrovnávání zatížení připojená toohello virtuální (NIC) hello.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="9f3a4-124">toocontrol hello tok přenosů, můžete definovat pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které mapují tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="9f3a4-125">Vytvořit nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="9f3a4-125">Create Azure load balancer</span></span>
<span data-ttu-id="9f3a4-126">V této části podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-126">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="9f3a4-127">Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-127">Before you can create your load balancer, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="9f3a4-128">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v hello *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-128">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="9f3a4-129">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="9f3a4-129">Create a public IP address</span></span>
<span data-ttu-id="9f3a4-130">tooaccess hello vaší aplikace na Internetu, musíte na veřejnou IP adresu pro nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-130">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="9f3a4-131">Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-131">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="9f3a4-132">Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v hello *myResourceGroupLoadBalancer* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-132">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="9f3a4-133">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-133">Create a load balancer</span></span>
<span data-ttu-id="9f3a4-134">Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-134">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="9f3a4-135">Hello následující příklad vytvoří na front-endovou IP adresu s názvem *myFrontEndPool*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-135">hello following example creates a frontend IP address named *myFrontEndPool*:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

<span data-ttu-id="9f3a4-136">Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-136">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="9f3a4-137">Hello následující příklad vytvoří fond back-end adresy s názvem *myBackEndPool*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-137">hello following example creates a backend address pool named *myBackEndPool*:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="9f3a4-138">Teď vytvořte hello Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-138">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="9f3a4-139">Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* pomocí hello *myPublicIP* adresa:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-139">hello following example creates a load balancer named *myLoadBalancer* using hello *myPublicIP* address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a><span data-ttu-id="9f3a4-140">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="9f3a4-140">Create a health probe</span></span>
<span data-ttu-id="9f3a4-141">tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-141">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="9f3a4-142">Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-142">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="9f3a4-143">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-143">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="9f3a4-144">Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-144">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="9f3a4-145">Hello následující ukázka vytvoří sondou TCP.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-145">hello following example creates a TCP probe.</span></span> <span data-ttu-id="9f3a4-146">Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-146">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="9f3a4-147">Pokud používáte vlastní sondu HTTP, musíte vytvořit hello stránka pro kontrolu stavu, jako například *healthcheck.aspx*.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-147">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.aspx*.</span></span> <span data-ttu-id="9f3a4-148">Test Hello musí vrátit **HTTP 200 OK** odpovědi pro hello zatížení vyrovnávání tookeep hello hostitele v otočení.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-148">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="9f3a4-149">toocreate sondou TCP stavu, můžete použít [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-149">toocreate a TCP health probe, you use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="9f3a4-150">Hello následující příklad vytvoří sondu stavu s názvem *myHealthProbe* který monitoruje každý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-150">hello following example creates a health probe named *myHealthProbe* that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

<span data-ttu-id="9f3a4-151">Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="9f3a4-151">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="9f3a4-152">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-152">Create a load balancer rule</span></span>
<span data-ttu-id="9f3a4-153">Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-153">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="9f3a4-154">Můžete definovat hello front-endové konfiguraci protokolu IP pro příchozí provoz hello a hello back-end fondu tooreceive hello provozu IP, spolu s hello požadované zdrojový a cílový port.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-154">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="9f3a4-155">toomake se, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat toouse test stavu hello.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-155">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="9f3a4-156">Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-156">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="9f3a4-157">Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem *myLoadBalancerRule* a vyrovnává přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-157">hello following example creates a load balancer rule named *myLoadBalancerRule* and balances traffic on port *80*:</span></span>

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

<span data-ttu-id="9f3a4-158">Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="9f3a4-158">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a><span data-ttu-id="9f3a4-159">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="9f3a4-159">Configure virtual network</span></span>
<span data-ttu-id="9f3a4-160">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-160">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="9f3a4-161">Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-161">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="9f3a4-162">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="9f3a4-162">Create network resources</span></span>
<span data-ttu-id="9f3a4-163">Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-163">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="9f3a4-164">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-164">hello following example creates a virtual network named *myVnet* with *mySubnet*:</span></span>

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

<span data-ttu-id="9f3a4-165">Vytvoření pravidla skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), pak vytvořte skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-165">Create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), then create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="9f3a4-166">Přidat hello toohello podsíť skupiny zabezpečení sítě s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) a aktualizujte hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-166">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) and then update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span></span> 

<span data-ttu-id="9f3a4-167">Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroup* a použije je příliš*mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-167">hello following example creates a network security group rule named *myNetworkSecurityGroup* and applies it too*mySubnet*:</span></span>

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

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

<span data-ttu-id="9f3a4-168">Virtuální síťové adaptéry jsou vytvořeny pomocí [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-168">Virtual NICs are created with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="9f3a4-169">Hello následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-169">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="9f3a4-170">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-170">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="9f3a4-171">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-171">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-virtual-machines"></a><span data-ttu-id="9f3a4-172">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9f3a4-172">Create virtual machines</span></span>
<span data-ttu-id="9f3a4-173">tooimprove hello vysokou dostupnost vaší aplikace, umístit virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-173">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span>

<span data-ttu-id="9f3a4-174">Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-174">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="9f3a4-175">Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-175">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

<span data-ttu-id="9f3a4-176">Nastavte uživatelské jméno a heslo správce pro virtuální počítače hello s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="9f3a4-176">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="9f3a4-177">Nyní můžete vytvořit hello virtuálních počítačů s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-177">Now you can create hello VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="9f3a4-178">Hello následující ukázka vytvoří tři virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-178">hello following example creates three VMs:</span></span>

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

<span data-ttu-id="9f3a4-179">Trvá několik minut toocreate a nakonfigurovat všechny tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-179">It takes a few minutes toocreate and configure all three VMs.</span></span>

### <a name="install-iis-with-custom-script-extension"></a><span data-ttu-id="9f3a4-180">Nainstalovat službu ISS s rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="9f3a4-180">Install IIS with Custom Script Extension</span></span>
<span data-ttu-id="9f3a4-181">V předchozích kurz [jak toocustomize virtuálního počítače s Windows](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak tooautomate přizpůsobení virtuálního počítače s hello vlastní skript rozšíření pro Windows.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-181">In a previous tutorial on [How toocustomize a Windows virtual machine](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with hello Custom Script Extension for Windows.</span></span> <span data-ttu-id="9f3a4-182">Můžete použít hello stejné přístupu tooinstall a konfigurovat služby IIS na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-182">You can use hello same approach tooinstall and configure IIS on your VMs.</span></span>

<span data-ttu-id="9f3a4-183">Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="9f3a4-184">Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` tooinstall hello webový server služby IIS a poté aktualizace hello *Default.htm* stránky tooshow hello název hostitele hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

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

## <a name="test-load-balancer"></a><span data-ttu-id="9f3a4-185">Nástroj pro vyrovnávání zatížení testu</span><span class="sxs-lookup"><span data-stu-id="9f3a4-185">Test load balancer</span></span>
<span data-ttu-id="9f3a4-186">Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="9f3a4-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="9f3a4-187">Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-187">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

<span data-ttu-id="9f3a4-188">Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-188">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="9f3a4-189">Hello web se zobrazuje, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-189">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Spuštění webu IIS](./media/tutorial-load-balancer/running-iis-website.png)

<span data-ttu-id="9f3a4-191">Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes všechny tři virtuální počítače spuštěné aplikace, můžete můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-191">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="9f3a4-192">Přidání a odebrání virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9f3a4-192">Add and remove VMs</span></span>
<span data-ttu-id="9f3a4-193">Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-193">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="9f3a4-194">toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-194">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="9f3a4-195">V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-195">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="9f3a4-196">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="9f3a4-196">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="9f3a4-197">Získat hello síťová karta s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), pak sada hello *pravidlo LoadBalancerBackendAddressPools* vlastnost hello virtuální síťovou kartu příliš*$null*.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), then set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC too*$null*.</span></span> <span data-ttu-id="9f3a4-198">Nakonec aktualizujte virtuální síťový adaptér. hello:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-198">Finally, update hello virtual NIC.:</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="9f3a4-199">Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes hello zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-199">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="9f3a4-200">Nyní můžete provést údržbu na hello virtuálních počítačů, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-200">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="9f3a4-201">Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-201">Add a VM toohello load balancer</span></span>
<span data-ttu-id="9f3a4-202">Po provádění údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, nastavte hello *pravidlo LoadBalancerBackendAddressPools* vlastnost hello virtuální síťový adaptér toohello *BackendAddressPool* z [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="9f3a4-202">After performing VM maintenance, or if you need tooexpand capacity, set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC toohello *BackendAddressPool* from [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span></span>

<span data-ttu-id="9f3a4-203">Získejte nástroj pro vyrovnávání zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-203">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="9f3a4-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f3a4-204">Next steps</span></span>

<span data-ttu-id="9f3a4-205">V tomto kurzu jste vytvořili pro vyrovnávání zatížení a připojené tooit virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-205">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="9f3a4-206">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="9f3a4-206">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f3a4-207">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="9f3a4-207">Create an Azure load balancer</span></span>
> * <span data-ttu-id="9f3a4-208">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-208">Create a load balancer health probe</span></span>
> * <span data-ttu-id="9f3a4-209">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-209">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="9f3a4-210">Použít hello rozšíření vlastních skriptů toocreate základní webu IIS</span><span class="sxs-lookup"><span data-stu-id="9f3a4-210">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="9f3a4-211">Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-211">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="9f3a4-212">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="9f3a4-212">View a load balancer in action</span></span>
> * <span data-ttu-id="9f3a4-213">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9f3a4-213">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="9f3a4-214">Jak zálohy další kurz toolearn toohello toomanage sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f3a4-214">Advance toohello next tutorial toolearn how toomanage VM networking.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9f3a4-215">Správa virtuálních počítačů a virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="9f3a4-215">Manage VMs and virtual networks</span></span>](./tutorial-virtual-network.md)
