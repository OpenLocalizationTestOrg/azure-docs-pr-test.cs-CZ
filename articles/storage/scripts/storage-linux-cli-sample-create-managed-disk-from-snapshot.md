---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření spravovaného disku ze snímku | Microsoft Docs"
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
ms.openlocfilehash: f92059353bfb739cff05213a9d206bfde2829a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="0e155-103">Vytvoření spravovaného disku ze snímku s rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0e155-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="0e155-104">Tento skript vytvoří se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="0e155-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="0e155-105">Použijte toorestore pro virtuální počítač z snímky operačního systému a dat disků.</span><span class="sxs-lookup"><span data-stu-id="0e155-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="0e155-106">Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky.</span><span class="sxs-lookup"><span data-stu-id="0e155-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="0e155-107">Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.</span><span class="sxs-lookup"><span data-stu-id="0e155-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="0e155-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0e155-108">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="0e155-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0e155-109">Script explanation</span></span>

<span data-ttu-id="0e155-110">Tento skript používá tyto příkazy toocreate se spravovaným diskem ze snímku.</span><span class="sxs-lookup"><span data-stu-id="0e155-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="0e155-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0e155-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0e155-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="0e155-112">Command</span></span> | <span data-ttu-id="0e155-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0e155-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0e155-114">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="0e155-114">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="0e155-115">Získá všechny vlastnosti hello snímku pomocí názvu hello a vlastnosti skupiny prostředků hello snímku.</span><span class="sxs-lookup"><span data-stu-id="0e155-115">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="0e155-116">Vlastnost ID je použité toocreate spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="0e155-116">Id property is used toocreate managed disk.</span></span>  |
| [<span data-ttu-id="0e155-117">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="0e155-117">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="0e155-118">Vytvoří spravované pomocí disku snímku Id spravované snímku</span><span class="sxs-lookup"><span data-stu-id="0e155-118">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0e155-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e155-119">Next steps</span></span>

[<span data-ttu-id="0e155-120">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="0e155-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="0e155-121">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0e155-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0e155-122">Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e155-122">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
