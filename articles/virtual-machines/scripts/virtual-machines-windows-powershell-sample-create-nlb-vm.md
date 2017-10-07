---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvořit Vyrovnávání zatížení sítě virtuálních počítačů Windows | Microsoft Docs"
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
ms.openlocfilehash: 60a48c5f243c8ff3ab886437ae45b21ce069ea4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a><span data-ttu-id="3478b-103">Vyrovnávání zatížení přenosů mezi vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3478b-103">Load balance traffic between highly available virtual machines</span></span>

<span data-ttu-id="3478b-104">Tento ukázkový skript vytvoří vše potřebné toorun několik virtuálních počítačů Windows serveru 2016 nakonfigurovaný v s vysokou dostupností a načíst vyrovnáváním konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3478b-104">This script sample creates everything needed toorun several Windows Server 2016 virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="3478b-105">Po spouštění skriptu hello, budete mít tři virtuální počítače připojené k tooan Azure skupiny dostupnosti a je přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="3478b-105">After running hello script, you will have three virtual machines, joined tooan Azure Availability Set, and accessible through an Azure Load Balancer.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3478b-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3478b-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="3478b-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="3478b-107">Clean up deployment</span></span> 

<span data-ttu-id="3478b-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="3478b-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="3478b-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3478b-109">Script explanation</span></span>

<span data-ttu-id="3478b-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="3478b-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="3478b-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3478b-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3478b-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3478b-112">Command</span></span> | <span data-ttu-id="3478b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3478b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3478b-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3478b-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="3478b-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3478b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3478b-116">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="3478b-117">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="3478b-117">Creates a subnet configuration.</span></span> <span data-ttu-id="3478b-118">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="3478b-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="3478b-119">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="3478b-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="3478b-120">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="3478b-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="3478b-121">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="3478b-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="3478b-122">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3478b-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="3478b-123">Nové AzureRmLoadBalancerFrontendIpConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-123">New-AzureRmLoadBalancerFrontendIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | <span data-ttu-id="3478b-124">Vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-124">Creates a front-end IP configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3478b-125">Nové AzureRmLoadBalancerBackendAddressPoolConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-125">New-AzureRmLoadBalancerBackendAddressPoolConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | <span data-ttu-id="3478b-126">Vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-126">Creates a backend address pool configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3478b-127">Nové AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-127">New-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | <span data-ttu-id="3478b-128">Vytvoří test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-128">Creates a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3478b-129">Nové AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-129">New-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | <span data-ttu-id="3478b-130">Vytvoří pravidlo konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-130">Creates a rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3478b-131">Nové AzureRmLoadBalancerInboundNatRuleConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-131">New-AzureRmLoadBalancerInboundNatRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | <span data-ttu-id="3478b-132">Vytvoří příchozí konfigurace pravidla NAT nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-132">Creates an inbound NAT rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3478b-133">Nové AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="3478b-133">New-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancer) | <span data-ttu-id="3478b-134">Vytvoří nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3478b-134">Creates a load balancer.</span></span> |
| [<span data-ttu-id="3478b-135">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-135">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="3478b-136">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="3478b-136">Creates a network security group rule configuration.</span></span> <span data-ttu-id="3478b-137">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="3478b-137">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="3478b-138">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="3478b-138">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="3478b-139">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="3478b-139">Creates a network security group.</span></span> |
| [<span data-ttu-id="3478b-140">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-140">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="3478b-141">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="3478b-141">Gets subnet information.</span></span> <span data-ttu-id="3478b-142">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3478b-142">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="3478b-143">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="3478b-143">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="3478b-144">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="3478b-144">Creates a network interface.</span></span> |
| [<span data-ttu-id="3478b-145">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="3478b-145">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="3478b-146">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3478b-146">Creates a VM configuration.</span></span> <span data-ttu-id="3478b-147">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="3478b-147">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="3478b-148">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3478b-148">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="3478b-149">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="3478b-149">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="3478b-150">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3478b-150">Create a virtual machine.</span></span> |
|[<span data-ttu-id="3478b-151">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3478b-151">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="3478b-152">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="3478b-152">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3478b-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3478b-153">Next steps</span></span>

<span data-ttu-id="3478b-154">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3478b-154">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3478b-155">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3478b-155">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
