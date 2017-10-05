---
title: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze snímku | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze snímku"
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
ms.openlocfilehash: 030fa06bc5819443dac5832172a93c2dca28d1ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="c305f-103">Vytvoření spravovaného disku ze snímku s rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c305f-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="c305f-104">Tento skript vytvoří se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="c305f-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="c305f-105">Ji použijte k obnovení virtuálního počítače z snímky operačního systému a dat disků.</span><span class="sxs-lookup"><span data-stu-id="c305f-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="c305f-106">Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky.</span><span class="sxs-lookup"><span data-stu-id="c305f-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="c305f-107">Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.</span><span class="sxs-lookup"><span data-stu-id="c305f-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c305f-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c305f-108">Sample script</span></span>

<span data-ttu-id="c305f-109">[!code-azurecli[hlavní](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "vytvořit spravovaných disků ze snímku")]</span><span class="sxs-lookup"><span data-stu-id="c305f-109">[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="c305f-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c305f-110">Script explanation</span></span>

<span data-ttu-id="c305f-111">Tento skript používá následující příkazy k vytvoření spravovaného disku ze snímku.</span><span class="sxs-lookup"><span data-stu-id="c305f-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="c305f-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c305f-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c305f-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c305f-113">Command</span></span> | <span data-ttu-id="c305f-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c305f-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c305f-115">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="c305f-115">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="c305f-116">Získá všechny vlastnosti snímku pomocí názvu a vlastnosti skupiny prostředků snímku.</span><span class="sxs-lookup"><span data-stu-id="c305f-116">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="c305f-117">Vlastnost ID se používá k vytvoření spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="c305f-117">Id property is used to create managed disk.</span></span>  |
| [<span data-ttu-id="c305f-118">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="c305f-118">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="c305f-119">Vytvoří spravované pomocí disku snímku Id spravované snímku</span><span class="sxs-lookup"><span data-stu-id="c305f-119">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c305f-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c305f-120">Next steps</span></span>

[<span data-ttu-id="c305f-121">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="c305f-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="c305f-122">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c305f-122">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c305f-123">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c305f-123">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
