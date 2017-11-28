---
title: "aaaAzure ukázkový skript prostředí PowerShell - kopírování (přesunout) spravované toosame disky nebo jiné předplatné | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - toosame kopírování (přesunout) spravované disky nebo jiné předplatné"
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
ms.openlocfilehash: 5a92118e10a14615e5b1713f1b90188b37b05305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-in-hello-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="85a89-103">Spravované kopírování disků ve hello stejné předplatné nebo jiné předplatné pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="85a89-103">Copy managed disks in hello same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="85a89-104">Tento skript vytvoří kopii existující spravované disk v hello stejného předplatného nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="85a89-104">This script creates a copy of an existing managed disk in hello same subscription or different subscription.</span></span> <span data-ttu-id="85a89-105">Hello se vytvoří nový disk v hello stejné oblasti jako nadřazené hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="85a89-105">hello new disk is created in hello same region as hello parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="85a89-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="85a89-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="85a89-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="85a89-107">Script explanation</span></span>

<span data-ttu-id="85a89-108">Tento skript používá následující příkazy toocreate nový disk spravované hello cílové předplatné pomocí hello Id zdroje hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="85a89-108">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="85a89-109">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="85a89-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="85a89-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="85a89-110">Command</span></span> | <span data-ttu-id="85a89-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="85a89-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="85a89-112">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="85a89-112">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="85a89-113">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="85a89-113">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="85a89-114">Obsahuje Id hello nadřazený disk a umístění, které je stejné jako umístění hello nadřazený disk hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="85a89-114">It includes hello resource Id of hello parent disk and location that is same as hello location of parent disk.</span></span>  |
| [<span data-ttu-id="85a89-115">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="85a89-115">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="85a89-116">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="85a89-116">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="85a89-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85a89-117">Next steps</span></span>

[<span data-ttu-id="85a89-118">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="85a89-118">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="85a89-119">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85a89-119">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="85a89-120">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="85a89-120">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
