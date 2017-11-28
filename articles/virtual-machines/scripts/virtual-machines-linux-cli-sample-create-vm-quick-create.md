---
title: "Ukázka skriptu rozhraní příkazového řádku Azure – rychlé vytvoření virtuálního počítače s Linuxem | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure – rychlé vytvoření virtuálního počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3df3affcedf9edbe6e548d881a877f14204ec280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="9800b-103">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9800b-103">Create a virtual machine</span></span>

<span data-ttu-id="9800b-104">Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému a související síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="9800b-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="9800b-105">Po spuštění skriptu, můžete k virtuálnímu počítači přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="9800b-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9800b-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9800b-106">Sample script</span></span>

<span data-ttu-id="9800b-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="9800b-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="9800b-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9800b-108">Clean up deployment</span></span> 

<span data-ttu-id="9800b-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="9800b-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9800b-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9800b-110">Script explanation</span></span>

<span data-ttu-id="9800b-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="9800b-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="9800b-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9800b-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9800b-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9800b-113">Command</span></span> | <span data-ttu-id="9800b-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9800b-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9800b-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="9800b-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9800b-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9800b-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9800b-117">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="9800b-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="9800b-118">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="9800b-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="9800b-119">Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.</span><span class="sxs-lookup"><span data-stu-id="9800b-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="9800b-120">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="9800b-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="9800b-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="9800b-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9800b-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9800b-122">Next steps</span></span>

<span data-ttu-id="9800b-123">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9800b-123">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9800b-124">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9800b-124">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
