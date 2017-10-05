---
title: "Azure ukázkový skript prostředí PowerShell - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný předplatné | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr"
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
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: f7b4869669a2c5e840f9bd384dcd6d6096ba58e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="19646-103">Kopírování snímku spravovaného disku v rámci stejného předplatného nebo jiného předplatného pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="19646-103">Copy snapshot of a managed disk in same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="19646-104">Tento skript vytvoří kopii snímku ve stejné stejného předplatného nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="19646-104">This script creates a copy of a snapshot in the same same subscription or different subscription.</span></span> <span data-ttu-id="19646-105">Pomocí tohoto skriptu přesunout do jiného předplatného pro uchovávání dat snímku.</span><span class="sxs-lookup"><span data-stu-id="19646-105">Use this script to move a snapshot to different subscription for data retention.</span></span> <span data-ttu-id="19646-106">Ukládání snímků v jiném předplatném. můžete chránit před nechtěným odstraněním snímků ve vašem předplatném hlavní.</span><span class="sxs-lookup"><span data-stu-id="19646-106">Storing snapshots in different subscription protect you from accidental deletion of snapshots in your main subscription.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="19646-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="19646-107">Sample script</span></span>

<span data-ttu-id="19646-108">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "kopírování snímku")]</span><span class="sxs-lookup"><span data-stu-id="19646-108">[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="19646-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="19646-109">Script explanation</span></span>

<span data-ttu-id="19646-110">Tento skript používá následující příkazy pro vytvoření snímku v cílové předplatné pomocí Id zdrojové snímku.</span><span class="sxs-lookup"><span data-stu-id="19646-110">This script uses following commands to create a snapshot in the target subscription using the Id of the source snapshot.</span></span> <span data-ttu-id="19646-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="19646-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="19646-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="19646-112">Command</span></span> | <span data-ttu-id="19646-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="19646-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="19646-114">Nové AzureRmSnapshotConfig</span><span class="sxs-lookup"><span data-stu-id="19646-114">New-AzureRmSnapshotConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | <span data-ttu-id="19646-115">Vytvoří snímek konfigurace, který se používá pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="19646-115">Creates snapshot configuration that is used for snapshot creation.</span></span> <span data-ttu-id="19646-116">Obsahuje Id nadřazené snímku a umístění, která je stejná jako nadřazená snímku prostředku.</span><span class="sxs-lookup"><span data-stu-id="19646-116">It includes the resource Id of the parent snapshot and location that is same as the parent snapshot.</span></span>  |
| [<span data-ttu-id="19646-117">Nové AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="19646-117">New-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="19646-118">Vytvoří snímek pomocí snímek konfigurace, název snímku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="19646-118">Creates a snapshot using snapshot configuration, snapshot name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="19646-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19646-119">Next steps</span></span>

[<span data-ttu-id="19646-120">Vytvoření virtuálního počítače ze snímku</span><span class="sxs-lookup"><span data-stu-id="19646-120">Create a virtual machine from a snapshot</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="19646-121">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="19646-121">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="19646-122">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19646-122">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>