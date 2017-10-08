---
title: "aaaAzure ukázkový skript prostředí PowerShell - NGINX | Microsoft Docs"
description: "Ukázka Azure skriptu prostředí PowerShell - NGINX"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 017a4853e400f8f544a3f92771adaf468b5dee76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-nginx-vm-with-powershell"></a><span data-ttu-id="f20fa-103">Vytvoření virtuálního počítače NGINX pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f20fa-103">Create an NGINX VM with PowerShell</span></span>

<span data-ttu-id="f20fa-104">Tento skript vytvoří virtuální počítač Azure a potom pomocí hello rozšíření vlastních skriptů Azure virtuálního počítače tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="f20fa-104">This script creates an Azure Virtual Machine and then uses hello Azure Virtual Machine Custom Script Extension tooinstall NGINX.</span></span> <span data-ttu-id="f20fa-105">Po spuštění skriptu hello, můžete přejít na web ukázku hello veřejnou IP adresu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f20fa-105">After running hello script, you can access a demo website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f20fa-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f20fa-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.ps1 "Create VM NGINX")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f20fa-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f20fa-107">Clean up deployment</span></span> 

<span data-ttu-id="f20fa-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="f20fa-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f20fa-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f20fa-109">Script explanation</span></span>

<span data-ttu-id="f20fa-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="f20fa-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="f20fa-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f20fa-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f20fa-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f20fa-112">Command</span></span> | <span data-ttu-id="f20fa-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f20fa-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f20fa-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f20fa-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="f20fa-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f20fa-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f20fa-116">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="f20fa-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="f20fa-117">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="f20fa-117">Creates a subnet configuration.</span></span> <span data-ttu-id="f20fa-118">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="f20fa-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="f20fa-119">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="f20fa-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="f20fa-120">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="f20fa-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="f20fa-121">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="f20fa-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="f20fa-122">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f20fa-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="f20fa-123">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="f20fa-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="f20fa-124">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="f20fa-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="f20fa-125">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="f20fa-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="f20fa-126">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="f20fa-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="f20fa-127">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="f20fa-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="f20fa-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="f20fa-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="f20fa-129">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="f20fa-129">Gets subnet information.</span></span> <span data-ttu-id="f20fa-130">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f20fa-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="f20fa-131">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="f20fa-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="f20fa-132">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="f20fa-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="f20fa-133">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="f20fa-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="f20fa-134">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f20fa-134">Creates a VM configuration.</span></span> <span data-ttu-id="f20fa-135">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="f20fa-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="f20fa-136">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f20fa-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="f20fa-137">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="f20fa-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="f20fa-138">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f20fa-138">Create a virtual machine.</span></span> |
| [<span data-ttu-id="f20fa-139">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="f20fa-139">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="f20fa-140">Přidáte virtuální počítač toohello rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f20fa-140">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="f20fa-141">V této ukázce je rozšíření vlastních skriptů hello použité tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="f20fa-141">In this sample, hello custom script extension is used tooinstall NGINX.</span></span> |
|[<span data-ttu-id="f20fa-142">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f20fa-142">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="f20fa-143">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="f20fa-143">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f20fa-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f20fa-144">Next steps</span></span>

<span data-ttu-id="f20fa-145">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f20fa-145">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f20fa-146">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f20fa-146">Additional virtual machine PowerShell script samples can be found in hello [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
