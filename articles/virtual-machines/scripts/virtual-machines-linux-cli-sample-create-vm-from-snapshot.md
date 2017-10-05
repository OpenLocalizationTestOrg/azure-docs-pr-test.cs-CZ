---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače ze snímku | Microsoft Docs"
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
ms.openlocfilehash: 6e47c3baebd5b68ec29d55c43dc00ae7665c81f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a><span data-ttu-id="14357-103">Vytvoření virtuálního počítače ze snímku s rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="14357-103">Create a virtual machine from a snapshot with CLI</span></span>

<span data-ttu-id="14357-104">Tento skript vytvoří virtuální počítač ze snímku disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="14357-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="14357-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="14357-105">Sample script</span></span>

<span data-ttu-id="14357-106">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "vytvoření virtuálního počítače ze snímku")]</span><span class="sxs-lookup"><span data-stu-id="14357-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="14357-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="14357-107">Clean up deployment</span></span> 

<span data-ttu-id="14357-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="14357-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="14357-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="14357-109">Script explanation</span></span>

<span data-ttu-id="14357-110">Tento skript používá následující příkazy k vytvoření spravovaného disku, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="14357-110">This script uses the following commands to create a managed disk, virtual machine, and all related resources.</span></span> <span data-ttu-id="14357-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="14357-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="14357-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="14357-112">Command</span></span> | <span data-ttu-id="14357-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="14357-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="14357-114">Zobrazit az snímku</span><span class="sxs-lookup"><span data-stu-id="14357-114">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="14357-115">Získá snímek pomocí název snímku a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="14357-115">Gets snapshot using snapshot name and resource group name.</span></span> <span data-ttu-id="14357-116">Vlastnost ID vráceného objektu se používá k vytvoření spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="14357-116">Id property of the returned object is used to create a managed disk.</span></span>  |
| [<span data-ttu-id="14357-117">Vytvoření az disku</span><span class="sxs-lookup"><span data-stu-id="14357-117">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="14357-118">Vytvoří spravovaných disků ze snímku pomocí snímku Id, název disku, typ úložiště a velikosti</span><span class="sxs-lookup"><span data-stu-id="14357-118">Creates managed disks from a snapshot using snapshot Id, disk name, storage type, and size</span></span>  |
| [<span data-ttu-id="14357-119">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="14357-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="14357-120">Vytvoří virtuálního počítače pomocí spravovaného disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="14357-120">Creates a VM using a managed OS disk</span></span> |

## <a name="next-steps"></a><span data-ttu-id="14357-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14357-121">Next steps</span></span>

<span data-ttu-id="14357-122">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="14357-122">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="14357-123">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="14357-123">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
