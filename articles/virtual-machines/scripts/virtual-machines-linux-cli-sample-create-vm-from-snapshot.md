---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače ze snímku | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače ze snímku"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ddc95289dcb8a0ca7c7854d969983f96b8f4613f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a><span data-ttu-id="a0584-103">Vytvoření virtuálního počítače ze snímku s rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a0584-103">Create a virtual machine from a snapshot with CLI</span></span>

<span data-ttu-id="a0584-104">Tento skript vytvoří virtuální počítač ze snímku disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="a0584-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a0584-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a0584-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a0584-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="a0584-106">Clean up deployment</span></span> 

<span data-ttu-id="a0584-107">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="a0584-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a0584-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a0584-108">Script explanation</span></span>

<span data-ttu-id="a0584-109">Tento skript používá hello následující příkazy toocreate spravovaných disků virtuálního počítače, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0584-109">This script uses hello following commands toocreate a managed disk, virtual machine, and all related resources.</span></span> <span data-ttu-id="a0584-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a0584-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a0584-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a0584-111">Command</span></span> | <span data-ttu-id="a0584-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a0584-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a0584-113">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="a0584-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="a0584-114">Získá snímek pomocí název snímku a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0584-114">Gets snapshot using snapshot name and resource group name.</span></span> <span data-ttu-id="a0584-115">Vlastnost ID hello vrátil objekt je použité toocreate se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="a0584-115">Id property of hello returned object is used toocreate a managed disk.</span></span>  |
| [<span data-ttu-id="a0584-116">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="a0584-116">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="a0584-117">Vytvoří spravovaných disků ze snímku pomocí snímku Id, název disku, typ úložiště a velikosti</span><span class="sxs-lookup"><span data-stu-id="a0584-117">Creates managed disks from a snapshot using snapshot Id, disk name, storage type, and size</span></span>  |
| [<span data-ttu-id="a0584-118">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="a0584-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="a0584-119">Vytvoří virtuálního počítače pomocí spravovaného disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="a0584-119">Creates a VM using a managed OS disk</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a0584-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0584-120">Next steps</span></span>

<span data-ttu-id="a0584-121">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a0584-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a0584-122">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a0584-122">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
