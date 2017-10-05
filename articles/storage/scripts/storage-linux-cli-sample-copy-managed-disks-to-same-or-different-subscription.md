---
title: "Ukázka skriptu Azure CLI - kopírování (přesunout) spravovaných disků na stejný nebo jiný předplatné | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) spravovaných disků na stejný nebo jiný odběr"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 784ad81db2c83da14665fa926425928f12a15c27
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a><span data-ttu-id="8e345-103">Zkopírujte spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8e345-103">Copy managed disks to same or different subscription with CLI</span></span>

<span data-ttu-id="8e345-104">Tento skript zkopíruje spravovaných disků na stejný nebo jiný odběr, ale ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8e345-104">This script copies a managed disk to same or different subscription but in the same region.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8e345-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8e345-105">Sample script</span></span>

<span data-ttu-id="8e345-106">[!code-azurecli[hlavní](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "kopie spravované disku")]</span><span class="sxs-lookup"><span data-stu-id="8e345-106">[!code-azurecli[main](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="8e345-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8e345-107">Script explanation</span></span>

<span data-ttu-id="8e345-108">Tento skript používá následující příkazy k vytvoření nového spravovaného disku v cílové předplatné pomocí Id zdrojového spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="8e345-108">This script uses following commands to create a new managed disk in the target subscription using the Id of the source managed disk.</span></span> <span data-ttu-id="8e345-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="8e345-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8e345-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8e345-110">Command</span></span> | <span data-ttu-id="8e345-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8e345-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8e345-112">Zobrazit az disku</span><span class="sxs-lookup"><span data-stu-id="8e345-112">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="8e345-113">Získá všechny vlastnosti spravovaného disku pomocí názvu a vlastnosti skupiny prostředků spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="8e345-113">Gets all the properties of a managed disk using the name and resource group properties of the managed disk.</span></span> <span data-ttu-id="8e345-114">Vlastnost ID se používá ke zkopírování spravovaných disků do jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="8e345-114">Id property is used to copy the managed disk to different subscription.</span></span>  |
| [<span data-ttu-id="8e345-115">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="8e345-115">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="8e345-116">Kopie spravovaného disku tak, že vytvoříte novou spravované disk v jiném předplatném. pomocí Id a název spravovaného nadřazený disk.</span><span class="sxs-lookup"><span data-stu-id="8e345-116">Copies a managed disk by creating a new managed disk in different subscription using Id and name the parent managed disk.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="8e345-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e345-117">Next steps</span></span>

[<span data-ttu-id="8e345-118">Vytvoření virtuálního počítače z spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="8e345-118">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="8e345-119">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8e345-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8e345-120">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8e345-120">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
