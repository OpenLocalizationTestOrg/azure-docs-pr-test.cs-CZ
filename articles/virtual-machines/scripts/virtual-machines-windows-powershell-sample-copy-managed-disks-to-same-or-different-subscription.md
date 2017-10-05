---
title: "Ukázkový skript prostředí PowerShell Azure - kopírování (přesunout) spravovaných disků na stejný nebo jiný předplatné | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure - kopírování (přesunout) spravovaných disků na stejný nebo jiný odběr"
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
ms.openlocfilehash: 75beb35dc19fa530d9b2c19aed6040f74afafbc0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="1ebae-103">Zkopírujte spravované disky ve stejném předplatném nebo jiného předplatného pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1ebae-103">Copy managed disks in the same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="1ebae-104">Tento skript vytvoří kopii existující spravovaných disků ve stejném předplatném nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="1ebae-104">This script creates a copy of an existing managed disk in the same subscription or different subscription.</span></span> <span data-ttu-id="1ebae-105">Nový disk se vytvoří ve stejné oblasti jako nadřazený disk spravované.</span><span class="sxs-lookup"><span data-stu-id="1ebae-105">The new disk is created in the same region as the parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1ebae-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1ebae-106">Sample script</span></span>

<span data-ttu-id="1ebae-107">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "kopie spravované disku")]</span><span class="sxs-lookup"><span data-stu-id="1ebae-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="1ebae-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1ebae-108">Script explanation</span></span>

<span data-ttu-id="1ebae-109">Tento skript používá následující příkazy k vytvoření nového spravovaného disku v cílové předplatné pomocí Id zdrojového spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="1ebae-109">This script uses following commands to create a new managed disk in the target subscription using the Id of the source managed disk.</span></span> <span data-ttu-id="1ebae-110">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="1ebae-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1ebae-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1ebae-111">Command</span></span> | <span data-ttu-id="1ebae-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1ebae-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1ebae-113">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="1ebae-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="1ebae-114">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="1ebae-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="1ebae-115">Obsahuje Id nadřazený disk a umístění, které je stejné jako umístění nadřazený disk prostředku.</span><span class="sxs-lookup"><span data-stu-id="1ebae-115">It includes the resource Id of the parent disk and location that is same as the location of parent disk.</span></span>  |
| [<span data-ttu-id="1ebae-116">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="1ebae-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="1ebae-117">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1ebae-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="1ebae-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ebae-118">Next steps</span></span>

[<span data-ttu-id="1ebae-119">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="1ebae-119">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="1ebae-120">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1ebae-120">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1ebae-121">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1ebae-121">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>