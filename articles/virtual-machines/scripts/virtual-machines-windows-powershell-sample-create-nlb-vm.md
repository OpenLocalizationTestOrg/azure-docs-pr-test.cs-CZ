---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření Vyrovnávání zatížení sítě virtuálních počítačů Windows | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření Vyrovnávání zatížení sítě virtuálních počítačů Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 25bcbbcd1615e01a384825d7bd1582a528e91f71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a><span data-ttu-id="783fe-103">Vyrovnávání zatížení přenosů mezi vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="783fe-103">Load balance traffic between highly available virtual machines</span></span>

<span data-ttu-id="783fe-104">Tento ukázkový skript vytvoří vše potřebné ke spuštění několika virtuálních počítačů systému Windows Server 2016 nakonfigurovaných v s vysokou dostupností a načíst vyrovnáváním konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="783fe-104">This script sample creates everything needed to run several Windows Server 2016 virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="783fe-105">Po spuštění skriptu, budete mít tři virtuální počítače připojené k Azure skupiny dostupnosti a přístupné prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="783fe-105">After running the script, you will have three virtual machines, joined to an Azure Availability Set, and accessible through an Azure Load Balancer.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="783fe-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="783fe-106">Sample script</span></span>

<span data-ttu-id="783fe-107">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "vytvořit Vyrovnávání zatížení sítě virtuálních počítačů")]</span><span class="sxs-lookup"><span data-stu-id="783fe-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="783fe-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="783fe-108">Clean up deployment</span></span> 

<span data-ttu-id="783fe-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="783fe-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="783fe-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="783fe-110">Script explanation</span></span>

<span data-ttu-id="783fe-111">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="783fe-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="783fe-112">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="783fe-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="783fe-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="783fe-113">Command</span></span> | <span data-ttu-id="783fe-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="783fe-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="783fe-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="783fe-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="783fe-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="783fe-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="783fe-117">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="783fe-118">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="783fe-118">Creates a subnet configuration.</span></span> <span data-ttu-id="783fe-119">Tato konfigurace se používá s procesem vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="783fe-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="783fe-120">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="783fe-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="783fe-121">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="783fe-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="783fe-122">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="783fe-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="783fe-123">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="783fe-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="783fe-124">Nové AzureRmLoadBalancerFrontendIpConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-124">New-AzureRmLoadBalancerFrontendIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | <span data-ttu-id="783fe-125">Vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-125">Creates a front-end IP configuration for a load balancer.</span></span> |
| [<span data-ttu-id="783fe-126">Nové AzureRmLoadBalancerBackendAddressPoolConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-126">New-AzureRmLoadBalancerBackendAddressPoolConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | <span data-ttu-id="783fe-127">Vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-127">Creates a backend address pool configuration for a load balancer.</span></span> |
| [<span data-ttu-id="783fe-128">Nové AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-128">New-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | <span data-ttu-id="783fe-129">Vytvoří test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-129">Creates a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="783fe-130">Nové AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-130">New-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | <span data-ttu-id="783fe-131">Vytvoří pravidlo konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-131">Creates a rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="783fe-132">Nové AzureRmLoadBalancerInboundNatRuleConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-132">New-AzureRmLoadBalancerInboundNatRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | <span data-ttu-id="783fe-133">Vytvoří příchozí konfigurace pravidla NAT nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-133">Creates an inbound NAT rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="783fe-134">Nové AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="783fe-134">New-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancer) | <span data-ttu-id="783fe-135">Vytvoří nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="783fe-135">Creates a load balancer.</span></span> |
| [<span data-ttu-id="783fe-136">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-136">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="783fe-137">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="783fe-137">Creates a network security group rule configuration.</span></span> <span data-ttu-id="783fe-138">Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG.</span><span class="sxs-lookup"><span data-stu-id="783fe-138">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="783fe-139">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="783fe-139">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="783fe-140">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="783fe-140">Creates a network security group.</span></span> |
| [<span data-ttu-id="783fe-141">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-141">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="783fe-142">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="783fe-142">Gets subnet information.</span></span> <span data-ttu-id="783fe-143">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="783fe-143">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="783fe-144">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="783fe-144">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="783fe-145">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="783fe-145">Creates a network interface.</span></span> |
| [<span data-ttu-id="783fe-146">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="783fe-146">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="783fe-147">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="783fe-147">Creates a VM configuration.</span></span> <span data-ttu-id="783fe-148">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="783fe-148">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="783fe-149">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="783fe-149">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="783fe-150">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="783fe-150">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="783fe-151">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="783fe-151">Create a virtual machine.</span></span> |
|[<span data-ttu-id="783fe-152">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="783fe-152">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="783fe-153">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="783fe-153">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="783fe-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="783fe-154">Next steps</span></span>

<span data-ttu-id="783fe-155">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="783fe-155">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="783fe-156">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="783fe-156">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
