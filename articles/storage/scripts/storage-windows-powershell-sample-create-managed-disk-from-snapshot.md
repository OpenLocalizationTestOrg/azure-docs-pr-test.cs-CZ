---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze snímku | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze snímku"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 9105d9dc06eea33b3a4e1eeea7fd793919166c9b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="7b5f4-103">Vytvoření spravovaného disku ze snímku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b5f4-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="7b5f4-104">Tento skript vytvoří se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="7b5f4-105">Ji použijte k obnovení virtuálního počítače z snímky operačního systému a dat disků.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="7b5f4-106">Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="7b5f4-107">Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7b5f4-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7b5f4-108">Sample script</span></span>

<span data-ttu-id="7b5f4-109">[!code-powershell[hlavní](../../../powershell_scripts/storage/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "vytvořit spravovaných disků ze snímku")]</span><span class="sxs-lookup"><span data-stu-id="7b5f4-109">[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="7b5f4-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7b5f4-110">Script explanation</span></span>

<span data-ttu-id="7b5f4-111">Tento skript používá následující příkazy k vytvoření spravovaného disku ze snímku.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="7b5f4-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7b5f4-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7b5f4-113">Command</span></span> | <span data-ttu-id="7b5f4-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7b5f4-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7b5f4-115">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="7b5f4-115">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="7b5f4-116">Získá vlastnosti snímku.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-116">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="7b5f4-117">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="7b5f4-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="7b5f4-118">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="7b5f4-119">Obsahuje Id nadřazené snímku, umístění, které je stejné jako umístění nadřazeného snímku a typ úložiště prostředku.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-119">It includes the resource Id of the parent snapshot, location that is same as the location of parent snapshot and the storage type.</span></span>  |
| [<span data-ttu-id="7b5f4-120">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="7b5f4-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="7b5f4-121">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7b5f4-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="7b5f4-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b5f4-122">Next steps</span></span>

[<span data-ttu-id="7b5f4-123">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="7b5f4-123">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="7b5f4-124">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7b5f4-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7b5f4-125">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7b5f4-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>