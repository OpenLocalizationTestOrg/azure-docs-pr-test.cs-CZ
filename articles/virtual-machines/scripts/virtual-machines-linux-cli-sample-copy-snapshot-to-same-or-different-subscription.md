---
title: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku"
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
ms.openlocfilehash: 6cc0125c08ccb77d014b4642d702c556fffdc8bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a><span data-ttu-id="9ea6d-103">Kopírování snímku spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9ea6d-103">Copy snapshot of a managed disk to same or different subscription with CLI</span></span>

<span data-ttu-id="9ea6d-104">Tento skript zkopíruje snímek spravovaných disků na stejný nebo jiný odběr.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-104">This script copies a snapshot of a managed disk to same or different subscription.</span></span> <span data-ttu-id="9ea6d-105">Pomocí tohoto skriptu snímek přesunout do jiného předplatného ve stejné oblasti jako nadřazené snímku.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-105">Use this script to move a snapshot to different subscription in the same region as the parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9ea6d-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9ea6d-106">Sample script</span></span>

<span data-ttu-id="9ea6d-107">[!code-azurecli[hlavní](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "kopírování snímku")]</span><span class="sxs-lookup"><span data-stu-id="9ea6d-107">[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="9ea6d-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9ea6d-108">Script explanation</span></span>

<span data-ttu-id="9ea6d-109">Tento skript používá následující příkazy pro vytvoření snímku v cílové předplatné pomocí Id zdrojové snímku.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-109">This script uses following commands to create a snapshot in the target subscription using the Id of the source snapshot.</span></span> <span data-ttu-id="9ea6d-110">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9ea6d-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9ea6d-111">Command</span></span> | <span data-ttu-id="9ea6d-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9ea6d-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9ea6d-113">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="9ea6d-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="9ea6d-114">Získá všechny vlastnosti snímku pomocí názvu a vlastnosti skupiny prostředků snímku.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-114">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="9ea6d-115">Vlastnost ID se používá ke kopírování snímku do jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-115">Id property is used to copy the snapshot to different subscription.</span></span>  |
| [<span data-ttu-id="9ea6d-116">Vytvoření snímku az</span><span class="sxs-lookup"><span data-stu-id="9ea6d-116">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="9ea6d-117">Zkopíruje snímek vytvořením snímku v jiné předplatné pomocí Id a název nadřazené snímku.</span><span class="sxs-lookup"><span data-stu-id="9ea6d-117">Copies a snapshot by creating a snapshot in different subscription using the Id and name of the parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="9ea6d-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ea6d-118">Next steps</span></span>

[<span data-ttu-id="9ea6d-119">Vytvoření virtuálního počítače ze snímku</span><span class="sxs-lookup"><span data-stu-id="9ea6d-119">Create a virtual machine from a snapshot</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="9ea6d-120">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9ea6d-120">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9ea6d-121">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ea6d-121">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
