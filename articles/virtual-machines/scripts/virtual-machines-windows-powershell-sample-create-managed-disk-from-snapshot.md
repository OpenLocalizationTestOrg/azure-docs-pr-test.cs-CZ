---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření spravovaného disku ze snímku | Microsoft Docs"
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
ms.openlocfilehash: b99413b4c3e9a662a5fef74b7e4aeb5ecbb26af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="60a74-103">Vytvoření spravovaného disku ze snímku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="60a74-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="60a74-104">Tento skript vytvoří se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="60a74-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="60a74-105">Použijte toorestore pro virtuální počítač z snímky operačního systému a dat disků.</span><span class="sxs-lookup"><span data-stu-id="60a74-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="60a74-106">Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky.</span><span class="sxs-lookup"><span data-stu-id="60a74-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="60a74-107">Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.</span><span class="sxs-lookup"><span data-stu-id="60a74-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="60a74-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="60a74-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="60a74-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="60a74-109">Script explanation</span></span>

<span data-ttu-id="60a74-110">Tento skript používá tyto příkazy toocreate se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="60a74-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="60a74-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="60a74-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="60a74-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="60a74-112">Command</span></span> | <span data-ttu-id="60a74-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="60a74-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="60a74-114">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="60a74-114">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="60a74-115">Získá vlastnosti snímku.</span><span class="sxs-lookup"><span data-stu-id="60a74-115">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="60a74-116">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="60a74-116">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="60a74-117">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="60a74-117">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="60a74-118">Obsahuje Id hello nadřazené snímku, umístění, které je stejné jako umístění hello nadřazené snímku a hello úložiště typu prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="60a74-118">It includes hello resource Id of hello parent snapshot, location that is same as hello location of parent snapshot and hello storage type.</span></span>  |
| [<span data-ttu-id="60a74-119">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="60a74-119">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="60a74-120">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="60a74-120">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="60a74-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60a74-121">Next steps</span></span>

[<span data-ttu-id="60a74-122">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="60a74-122">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="60a74-123">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60a74-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="60a74-124">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60a74-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
