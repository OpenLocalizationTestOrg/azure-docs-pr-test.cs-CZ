---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuálního počítače ze snímku | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuálního počítače ze snímku"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 63d108bbfd0f58f8a40bf1c7c8649e3a1f7ed288
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a><span data-ttu-id="1969a-103">Vytvoření virtuálního počítače ze snímku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1969a-103">Create a virtual machine from a snapshot with PowerShell</span></span>

<span data-ttu-id="1969a-104">Tento skript vytvoří virtuální počítač ze snímku disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="1969a-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1969a-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1969a-105">Sample script</span></span>

<span data-ttu-id="1969a-106">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "vytvoření virtuálního počítače z disku spravované operačního systému")]</span><span class="sxs-lookup"><span data-stu-id="1969a-106">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1969a-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="1969a-107">Clean up deployment</span></span> 

<span data-ttu-id="1969a-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1969a-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="1969a-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1969a-109">Script explanation</span></span>

<span data-ttu-id="1969a-110">Tento skript používá následující příkazy a získat vlastnosti snímku, vytvoření spravovaného disku ze snímku vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1969a-110">This script uses the following commands to get snapshot properties, create a managed disk from snapshot and create a VM.</span></span> <span data-ttu-id="1969a-111">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="1969a-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1969a-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1969a-112">Command</span></span> | <span data-ttu-id="1969a-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1969a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1969a-114">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="1969a-114">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/get-azurermsnapshot) | <span data-ttu-id="1969a-115">Získá snímek pomocí názvu snímku.</span><span class="sxs-lookup"><span data-stu-id="1969a-115">Gets a snapshot using snapshot name.</span></span> |
| [<span data-ttu-id="1969a-116">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="1969a-116">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/new-azurermdiskconfig) | <span data-ttu-id="1969a-117">Vytvoří konfiguraci disku.</span><span class="sxs-lookup"><span data-stu-id="1969a-117">Creates a disk configuration.</span></span> <span data-ttu-id="1969a-118">Tato konfigurace se používá s procesem vytvoření disku.</span><span class="sxs-lookup"><span data-stu-id="1969a-118">This configuration is used with the disk creation process.</span></span> |
| [<span data-ttu-id="1969a-119">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="1969a-119">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/new-azurermdisk) | <span data-ttu-id="1969a-120">Vytvoří se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="1969a-120">Creates a managed disk.</span></span> |
| [<span data-ttu-id="1969a-121">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="1969a-121">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="1969a-122">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1969a-122">Creates a VM configuration.</span></span> <span data-ttu-id="1969a-123">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="1969a-123">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="1969a-124">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1969a-124">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="1969a-125">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="1969a-125">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="1969a-126">Připojí spravované disk jako disk operačního systému k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="1969a-126">Attaches the managed disk as OS disk to the virtual machine</span></span> |
| [<span data-ttu-id="1969a-127">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="1969a-127">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="1969a-128">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1969a-128">Creates a public IP address.</span></span> |
| [<span data-ttu-id="1969a-129">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="1969a-129">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="1969a-130">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="1969a-130">Creates a network interface.</span></span> |
| [<span data-ttu-id="1969a-131">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="1969a-131">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="1969a-132">Vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1969a-132">Creates a virtual machine.</span></span> |
|[<span data-ttu-id="1969a-133">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1969a-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="1969a-134">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="1969a-134">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1969a-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1969a-135">Next steps</span></span>

<span data-ttu-id="1969a-136">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1969a-136">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1969a-137">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1969a-137">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>