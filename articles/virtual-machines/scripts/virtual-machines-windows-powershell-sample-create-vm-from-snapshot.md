---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření virtuálního počítače ze snímku | Microsoft Docs"
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
ms.openlocfilehash: 89c65171b55bff0582c4a26df0b0f29f556845fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a><span data-ttu-id="4296f-103">Vytvoření virtuálního počítače ze snímku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4296f-103">Create a virtual machine from a snapshot with PowerShell</span></span>

<span data-ttu-id="4296f-104">Tento skript vytvoří virtuální počítač ze snímku disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="4296f-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4296f-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4296f-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4296f-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4296f-106">Clean up deployment</span></span> 

<span data-ttu-id="4296f-107">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="4296f-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4296f-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4296f-108">Script explanation</span></span>

<span data-ttu-id="4296f-109">Tento skript používá hello následující příkazy tooget vlastnosti snímku, vytvoření spravovaného disku ze snímku a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4296f-109">This script uses hello following commands tooget snapshot properties, create a managed disk from snapshot and create a VM.</span></span> <span data-ttu-id="4296f-110">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4296f-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4296f-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4296f-111">Command</span></span> | <span data-ttu-id="4296f-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4296f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4296f-113">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="4296f-113">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/get-azurermsnapshot) | <span data-ttu-id="4296f-114">Získá snímek pomocí názvu snímku.</span><span class="sxs-lookup"><span data-stu-id="4296f-114">Gets a snapshot using snapshot name.</span></span> |
| [<span data-ttu-id="4296f-115">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="4296f-115">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/new-azurermdiskconfig) | <span data-ttu-id="4296f-116">Vytvoří konfiguraci disku.</span><span class="sxs-lookup"><span data-stu-id="4296f-116">Creates a disk configuration.</span></span> <span data-ttu-id="4296f-117">Tato konfigurace se používá s proces vytvoření disku hello.</span><span class="sxs-lookup"><span data-stu-id="4296f-117">This configuration is used with hello disk creation process.</span></span> |
| [<span data-ttu-id="4296f-118">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="4296f-118">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/new-azurermdisk) | <span data-ttu-id="4296f-119">Vytvoří se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="4296f-119">Creates a managed disk.</span></span> |
| [<span data-ttu-id="4296f-120">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="4296f-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="4296f-121">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4296f-121">Creates a VM configuration.</span></span> <span data-ttu-id="4296f-122">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="4296f-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="4296f-123">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4296f-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="4296f-124">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="4296f-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="4296f-125">Připojí spravovaných disků na hello jako operačního systému disku toohello virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4296f-125">Attaches hello managed disk as OS disk toohello virtual machine</span></span> |
| [<span data-ttu-id="4296f-126">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="4296f-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="4296f-127">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4296f-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="4296f-128">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="4296f-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="4296f-129">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="4296f-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="4296f-130">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="4296f-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="4296f-131">Vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4296f-131">Creates a virtual machine.</span></span> |
|[<span data-ttu-id="4296f-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4296f-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="4296f-133">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="4296f-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4296f-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4296f-134">Next steps</span></span>

<span data-ttu-id="4296f-135">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4296f-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4296f-136">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4296f-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
