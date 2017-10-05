---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času"
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
ms.openlocfilehash: 8588a33a1f0b4cce0059a40239301587cdc28920
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="7ae26-103">Vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ae26-103">Create a snapshot from a VHD to create multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="7ae26-104">Tento skript vytvoří snímek ze souboru virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="7ae26-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="7ae26-105">Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na snímku a pak použít snímku vytvořit více identické discích spravovaných malé množství času.</span><span class="sxs-lookup"><span data-stu-id="7ae26-105">Use this script to import a specialized (not generalized/sysprepped) VHD to a snapshot and then use the snapshot to create multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="7ae26-106">Navíc můžete importovat data virtuálního pevného disku na snímek a potom pomocí snímku můžete vytvořit více spravovaných disků malé množství času.</span><span class="sxs-lookup"><span data-stu-id="7ae26-106">Also, use it to import a data VHD to a snapshot and then use the snapshot to create multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7ae26-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7ae26-107">Sample script</span></span>

<span data-ttu-id="7ae26-108">[!code-powershell[hlavní](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "vytvořit snímek z virtuálního pevného disku")]</span><span class="sxs-lookup"><span data-stu-id="7ae26-108">[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="7ae26-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7ae26-109">Script explanation</span></span>

<span data-ttu-id="7ae26-110">Tento skript používá následující příkazy k vytvoření spravovaného disku z disku VHD v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="7ae26-110">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="7ae26-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="7ae26-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7ae26-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7ae26-112">Command</span></span> | <span data-ttu-id="7ae26-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7ae26-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7ae26-114">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="7ae26-114">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="7ae26-115">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="7ae26-115">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="7ae26-116">Obsahuje typ úložiště, umístění, prostředek Id účtu úložiště, kde je uložený nadřazený virtuální pevný disk a URI virtuálního pevného disku nadřazeného virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="7ae26-116">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, and VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="7ae26-117">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="7ae26-117">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="7ae26-118">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ae26-118">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7ae26-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ae26-119">Next steps</span></span>

[<span data-ttu-id="7ae26-120">Vytvoření spravovaného disku ze snímku</span><span class="sxs-lookup"><span data-stu-id="7ae26-120">Create a managed disk from snapshot</span></span>](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="7ae26-121">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="7ae26-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="7ae26-122">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7ae26-122">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7ae26-123">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7ae26-123">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>