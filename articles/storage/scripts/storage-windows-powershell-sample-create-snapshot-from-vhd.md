---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvořit snímek z virtuálního pevného disku toocreate více stejné spravované disky malé množství času | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření snímku z virtuálního pevného disku toocreate více stejné spravované disky malé množství času"
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
ms.openlocfilehash: 0a13e399b692f32b3772add39fe5b5c023808c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-snapshot-from-a-vhd-toocreate-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="b2702-103">Vytvoření snímku z virtuálního pevného disku toocreate více stejné spravované disky malé množství času v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b2702-103">Create a snapshot from a VHD toocreate multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="b2702-104">Tento skript vytvoří snímek ze souboru virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="b2702-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="b2702-105">Použijte tento skript tooimport specializované tooa snímek virtuálního pevného disku (není zobecněný/Sysprep) a potom pomocí hello snímku toocreate více stejné spravované disky malé množství času.</span><span class="sxs-lookup"><span data-stu-id="b2702-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD tooa snapshot and then use hello snapshot toocreate multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="b2702-106">Taky ho používat tooimport snímek tooa data virtuálního pevného disku a potom pomocí hello snímku toocreate více spravovaných disků malé množství času.</span><span class="sxs-lookup"><span data-stu-id="b2702-106">Also, use it tooimport a data VHD tooa snapshot and then use hello snapshot toocreate multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b2702-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b2702-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="b2702-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b2702-108">Script explanation</span></span>

<span data-ttu-id="b2702-109">Tento skript používá tyto příkazy toocreate spravovaného disku z disku VHD v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="b2702-109">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="b2702-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="b2702-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b2702-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b2702-111">Command</span></span> | <span data-ttu-id="b2702-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b2702-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b2702-113">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="b2702-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="b2702-114">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="b2702-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="b2702-115">Obsahuje typ úložiště, umístění, prostředek Id účtu úložiště hello se uloží hello nadřazený virtuální pevný disk a identifikátor URI virtuálního pevného disku hello nadřazený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b2702-115">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, and VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="b2702-116">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="b2702-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="b2702-117">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b2702-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b2702-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2702-118">Next steps</span></span>

[<span data-ttu-id="b2702-119">Vytvoření spravovaného disku ze snímku</span><span class="sxs-lookup"><span data-stu-id="b2702-119">Create a managed disk from snapshot</span></span>](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="b2702-120">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="b2702-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="b2702-121">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b2702-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b2702-122">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2702-122">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
