---
title: "aaaAzure ukázka skriptu prostředí PowerShell - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného"
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
ms.openlocfilehash: d7b8a71cc09d1950271f472e89b95bb551323be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="5e2b0-103">Kopírování snímku spravovaného disku v rámci stejného předplatného nebo jiného předplatného pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e2b0-103">Copy snapshot of a managed disk in same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="5e2b0-104">Tento skript vytvoří kopii snímku v hello stejné předplatné stejné nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-104">This script creates a copy of a snapshot in hello same same subscription or different subscription.</span></span> <span data-ttu-id="5e2b0-105">Pro uchovávání dat použijte tento skript toomove předplatné toodifferent snímku.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-105">Use this script toomove a snapshot toodifferent subscription for data retention.</span></span> <span data-ttu-id="5e2b0-106">Ukládání snímků v jiném předplatném. můžete chránit před nechtěným odstraněním snímků ve vašem předplatném hlavní.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-106">Storing snapshots in different subscription protect you from accidental deletion of snapshots in your main subscription.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5e2b0-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5e2b0-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="5e2b0-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5e2b0-108">Script explanation</span></span>

<span data-ttu-id="5e2b0-109">Tento skript používá následující příkazy toocreate snímku hello cílové předplatné pomocí hello Id hello zdroj snímku.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-109">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="5e2b0-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5e2b0-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5e2b0-111">Command</span></span> | <span data-ttu-id="5e2b0-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5e2b0-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5e2b0-113">Nové AzureRmSnapshotConfig</span><span class="sxs-lookup"><span data-stu-id="5e2b0-113">New-AzureRmSnapshotConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | <span data-ttu-id="5e2b0-114">Vytvoří snímek konfigurace, který se používá pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-114">Creates snapshot configuration that is used for snapshot creation.</span></span> <span data-ttu-id="5e2b0-115">Obsahuje Id hello nadřazené snímku a umístění, které je stejné jako snímek nadřazené hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-115">It includes hello resource Id of hello parent snapshot and location that is same as hello parent snapshot.</span></span>  |
| [<span data-ttu-id="5e2b0-116">Nové AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="5e2b0-116">New-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="5e2b0-117">Vytvoří snímek pomocí snímek konfigurace, název snímku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e2b0-117">Creates a snapshot using snapshot configuration, snapshot name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="5e2b0-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e2b0-118">Next steps</span></span>

[<span data-ttu-id="5e2b0-119">Vytvoření virtuálního počítače ze snímku</span><span class="sxs-lookup"><span data-stu-id="5e2b0-119">Create a virtual machine from a snapshot</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="5e2b0-120">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5e2b0-120">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="5e2b0-121">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e2b0-121">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
