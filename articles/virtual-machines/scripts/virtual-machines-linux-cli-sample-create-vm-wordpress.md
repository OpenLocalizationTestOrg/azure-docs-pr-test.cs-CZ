---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem pomocí WordPress | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí WordPress"
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
ms.openlocfilehash: 2c5c03d08b6d5d27eb8c505b1dbd817eda5f2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="978ee-103">Vytvoření virtuálního počítače s WordPress</span><span class="sxs-lookup"><span data-stu-id="978ee-103">Create a VM with WordPress</span></span>

<span data-ttu-id="978ee-104">Tento skript vytvoří virtuální počítač a potom pomocí hello virtuálního počítače Azure vlastní skript rozšíření tooinstall WordPress.</span><span class="sxs-lookup"><span data-stu-id="978ee-104">This script creates a virtual machine, and then uses hello Azure Virtual Machine custom script extension tooinstall WordPress.</span></span> <span data-ttu-id="978ee-105">Po spouštění skriptu hello, budete mít přístup hello WordPress konfigurace lokality v `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="978ee-105">After running hello script, you can access hello WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="978ee-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="978ee-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="978ee-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="978ee-107">Clean up deployment</span></span> 

<span data-ttu-id="978ee-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="978ee-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="978ee-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="978ee-109">Script explanation</span></span>

<span data-ttu-id="978ee-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="978ee-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="978ee-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="978ee-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="978ee-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="978ee-112">Command</span></span> | <span data-ttu-id="978ee-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="978ee-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="978ee-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="978ee-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="978ee-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="978ee-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="978ee-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="978ee-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="978ee-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="978ee-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="978ee-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="978ee-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="978ee-119">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="978ee-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="978ee-120">Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="978ee-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="978ee-121">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="978ee-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="978ee-122">nastavení rozšíření virtuálního az</span><span class="sxs-lookup"><span data-stu-id="978ee-122">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="978ee-123">Přidáte hello rozšíření vlastních skriptů toohello virtuální počítač, který vyvolává skript tooinstall WordPress.</span><span class="sxs-lookup"><span data-stu-id="978ee-123">Add hello Custom Script Extension toohello virtual machine, which invokes a script tooinstall WordPress.</span></span> |
| [<span data-ttu-id="978ee-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="978ee-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="978ee-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="978ee-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="978ee-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="978ee-126">Next steps</span></span>

<span data-ttu-id="978ee-127">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="978ee-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="978ee-128">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="978ee-128">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
