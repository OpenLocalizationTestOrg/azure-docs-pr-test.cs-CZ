---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému"
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
ms.openlocfilehash: 71fc5c6a577c64b913cfa35e99b2b9b75dea0c31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="cef85-103">Vytvoření virtuálního počítače existující spravované disk operačního systému pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="cef85-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="cef85-104">Tento skript vytvoří virtuální počítač připojením existující spravované disk jako disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="cef85-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="cef85-105">Pomocí tohoto skriptu v předchozích scénáře:</span><span class="sxs-lookup"><span data-stu-id="cef85-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="cef85-106">Vytvoření virtuálního počítače z existujícího spravovaného disku operačního systému, který jste zkopírovali ze spravovaných disků v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="cef85-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="cef85-107">Vytvoření virtuálního počítače z existujícího spravovaného disku, který byl vytvořen z specializované soubor virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="cef85-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="cef85-108">Vytvoření virtuálního počítače z existujícího spravované disku operačního systému, který byl vytvořen ze snímku</span><span class="sxs-lookup"><span data-stu-id="cef85-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cef85-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="cef85-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cef85-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="cef85-110">Clean up deployment</span></span> 

<span data-ttu-id="cef85-111">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="cef85-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="cef85-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="cef85-112">Script explanation</span></span>

<span data-ttu-id="cef85-113">Tento skript používá hello následující vlastnosti disku tooget spravovat příkazů, připojte tooa spravovaných disků na nový virtuální počítač a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cef85-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="cef85-114">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cef85-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cef85-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="cef85-115">Command</span></span> | <span data-ttu-id="cef85-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cef85-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cef85-117">Zobrazit az disku</span><span class="sxs-lookup"><span data-stu-id="cef85-117">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="cef85-118">Vlastnosti spravovaných disků na získá pomocí název disku a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="cef85-118">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="cef85-119">Vlastnost ID je použité tooattach tooa spravovaných disků na nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="cef85-119">Id property is used tooattach a managed disk tooa new VM</span></span> |
| [<span data-ttu-id="cef85-120">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="cef85-120">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cef85-121">Vytvoří virtuálního počítače pomocí spravovaného disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="cef85-121">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="cef85-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cef85-122">Next steps</span></span>

<span data-ttu-id="cef85-123">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cef85-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cef85-124">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cef85-124">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
