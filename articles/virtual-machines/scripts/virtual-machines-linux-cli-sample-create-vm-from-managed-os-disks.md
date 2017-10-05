---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému | Microsoft Docs"
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
ms.openlocfilehash: 18eefee869b243b35d4426c226eff5f48cd490a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="5f920-103">Vytvoření virtuálního počítače existující spravované disk operačního systému pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5f920-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="5f920-104">Tento skript vytvoří virtuální počítač připojením existující spravované disk jako disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="5f920-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="5f920-105">Pomocí tohoto skriptu v předchozích scénáře:</span><span class="sxs-lookup"><span data-stu-id="5f920-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="5f920-106">Vytvoření virtuálního počítače z existujícího spravovaného disku operačního systému, který jste zkopírovali ze spravovaných disků v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="5f920-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="5f920-107">Vytvoření virtuálního počítače z existujícího spravovaného disku, který byl vytvořen z specializované soubor virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5f920-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="5f920-108">Vytvoření virtuálního počítače z existujícího spravované disku operačního systému, který byl vytvořen ze snímku</span><span class="sxs-lookup"><span data-stu-id="5f920-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5f920-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5f920-109">Sample script</span></span>

<span data-ttu-id="5f920-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "vytvoření virtuálního počítače ze spravovaných disků")]</span><span class="sxs-lookup"><span data-stu-id="5f920-110">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5f920-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5f920-111">Clean up deployment</span></span> 

<span data-ttu-id="5f920-112">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5f920-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5f920-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5f920-113">Script explanation</span></span>

<span data-ttu-id="5f920-114">Tento skript používá následující příkazy získat vlastnosti spravovaných disků, připojte spravovaných disků na nový virtuální počítač a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5f920-114">This script uses the following commands to get managed disk properties, attach a managed disk to a new VM and create a VM.</span></span> <span data-ttu-id="5f920-115">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="5f920-115">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5f920-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5f920-116">Command</span></span> | <span data-ttu-id="5f920-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5f920-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5f920-118">Zobrazit az disku</span><span class="sxs-lookup"><span data-stu-id="5f920-118">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="5f920-119">Vlastnosti spravovaných disků na získá pomocí název disku a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5f920-119">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="5f920-120">Vlastnost ID se používá k připojení se spravovaným diskem k vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5f920-120">Id property is used to attach a managed disk to a new VM</span></span> |
| [<span data-ttu-id="5f920-121">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="5f920-121">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5f920-122">Vytvoří virtuálního počítače pomocí spravovaného disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="5f920-122">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="5f920-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f920-123">Next steps</span></span>

<span data-ttu-id="5f920-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5f920-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5f920-125">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5f920-125">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
