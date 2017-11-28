---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření virtuálního počítače s Windows | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuálního počítače systému Windows"
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
ms.date: 03/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 81f04ed88a968cb7ac540b0b4b874dcf230a8e1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-powershell"></a><span data-ttu-id="a970b-103">Vytvoření virtuálního počítače v kompletně nakonfigurovaný pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a970b-103">Create a fully configured virtual machine with PowerShell</span></span>

<span data-ttu-id="a970b-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="a970b-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="a970b-105">Po spuštění skriptu hello, získat přístup hello virtuální počítač prostřednictvím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="a970b-105">After running hello script, you can access hello virtual machine over RDP.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a970b-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a970b-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.ps1 "Create VM detailed")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a970b-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="a970b-107">Clean up deployment</span></span> 

<span data-ttu-id="a970b-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="a970b-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a970b-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a970b-109">Script explanation</span></span>

<span data-ttu-id="a970b-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="a970b-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="a970b-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a970b-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a970b-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a970b-112">Command</span></span> | <span data-ttu-id="a970b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a970b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a970b-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a970b-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="a970b-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="a970b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a970b-116">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="a970b-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="a970b-117">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="a970b-117">Creates a subnet configuration.</span></span> <span data-ttu-id="a970b-118">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="a970b-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="a970b-119">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="a970b-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="a970b-120">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="a970b-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="a970b-121">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="a970b-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="a970b-122">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a970b-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="a970b-123">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="a970b-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="a970b-124">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a970b-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="a970b-125">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="a970b-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="a970b-126">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="a970b-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="a970b-127">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a970b-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="a970b-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="a970b-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="a970b-129">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="a970b-129">Gets subnet information.</span></span> <span data-ttu-id="a970b-130">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a970b-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="a970b-131">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="a970b-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="a970b-132">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="a970b-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="a970b-133">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="a970b-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="a970b-134">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a970b-134">Creates a VM configuration.</span></span> <span data-ttu-id="a970b-135">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="a970b-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="a970b-136">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a970b-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="a970b-137">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="a970b-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="a970b-138">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a970b-138">Create a virtual machine.</span></span> |
|[<span data-ttu-id="a970b-139">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a970b-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="a970b-140">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="a970b-140">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a970b-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a970b-141">Next steps</span></span>

<span data-ttu-id="a970b-142">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a970b-142">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a970b-143">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a970b-143">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
