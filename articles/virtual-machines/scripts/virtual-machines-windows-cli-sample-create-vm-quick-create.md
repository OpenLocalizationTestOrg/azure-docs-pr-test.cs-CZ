---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - prostřednictvím funkce rychle vytvořit virtuální počítač Windows serveru 2016 | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure – rychlé vytvoření virtuálního počítače Windows serveru 2016"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rickstercdn
ms.openlocfilehash: 4c736ce9e2ecc9ee75b34f903cad52c9c0ee0707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="65a2b-103">Rychlé vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="65a2b-103">Quick Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="65a2b-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="65a2b-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="65a2b-105">Po spuštění skriptu hello, dostanete hello virtuálního počítače přes připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="65a2b-105">After running hello script, you can access hello virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="65a2b-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="65a2b-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-windows-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="65a2b-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="65a2b-107">Clean up deployment</span></span> 

<span data-ttu-id="65a2b-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="65a2b-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="65a2b-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="65a2b-109">Script explanation</span></span>

<span data-ttu-id="65a2b-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="65a2b-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="65a2b-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="65a2b-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="65a2b-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="65a2b-112">Command</span></span> | <span data-ttu-id="65a2b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="65a2b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="65a2b-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="65a2b-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="65a2b-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="65a2b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="65a2b-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="65a2b-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="65a2b-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="65a2b-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="65a2b-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="65a2b-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="65a2b-119">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="65a2b-119">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="65a2b-120">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="65a2b-120">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="65a2b-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65a2b-121">Next steps</span></span>

<span data-ttu-id="65a2b-122">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="65a2b-122">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="65a2b-123">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65a2b-123">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
