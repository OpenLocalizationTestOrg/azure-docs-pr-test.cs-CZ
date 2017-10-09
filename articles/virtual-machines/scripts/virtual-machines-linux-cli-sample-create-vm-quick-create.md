---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - prostřednictvím funkce rychle vytvořit virtuální počítač s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: edecf274f4e401af3603e5be37a989e2e0eb22c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="01f1b-103">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="01f1b-103">Create a virtual machine</span></span>

<span data-ttu-id="01f1b-104">Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému a související síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="01f1b-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="01f1b-105">Po spuštění skriptu hello, můžete přistupovat přes protokol SSH hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="01f1b-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="01f1b-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="01f1b-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="01f1b-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="01f1b-107">Clean up deployment</span></span> 

<span data-ttu-id="01f1b-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="01f1b-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="01f1b-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="01f1b-109">Script explanation</span></span>

<span data-ttu-id="01f1b-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="01f1b-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="01f1b-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="01f1b-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="01f1b-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="01f1b-112">Command</span></span> | <span data-ttu-id="01f1b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01f1b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="01f1b-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="01f1b-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="01f1b-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="01f1b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="01f1b-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="01f1b-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="01f1b-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="01f1b-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="01f1b-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="01f1b-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="01f1b-119">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="01f1b-119">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="01f1b-120">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="01f1b-120">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="01f1b-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01f1b-121">Next steps</span></span>

<span data-ttu-id="01f1b-122">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="01f1b-122">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="01f1b-123">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01f1b-123">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
