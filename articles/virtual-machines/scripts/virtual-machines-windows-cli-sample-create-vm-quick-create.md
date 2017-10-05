---
title: "Ukázka skriptu rozhraní příkazového řádku Azure – rychlé vytvoření virtuálního počítače Windows serveru 2016 | Microsoft Docs"
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
ms.openlocfilehash: 084518bf7bc1d01c4a146efe3e0b7fe08149ce35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="97aa0-103">Rychlé vytvoření virtuálního počítače pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="97aa0-103">Quick Create a virtual machine with the Azure CLI</span></span>

<span data-ttu-id="97aa0-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="97aa0-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="97aa0-105">Po spuštění skriptu, můžete k virtuálnímu počítači prostřednictvím připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="97aa0-105">After running the script, you can access the virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="97aa0-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="97aa0-106">Sample script</span></span>

<span data-ttu-id="97aa0-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-quick/create-windows-vm-quick.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="97aa0-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-windows-vm-quick.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="97aa0-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="97aa0-108">Clean up deployment</span></span> 

<span data-ttu-id="97aa0-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="97aa0-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="97aa0-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="97aa0-110">Script explanation</span></span>

<span data-ttu-id="97aa0-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="97aa0-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="97aa0-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="97aa0-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="97aa0-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="97aa0-113">Command</span></span> | <span data-ttu-id="97aa0-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="97aa0-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="97aa0-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="97aa0-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="97aa0-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="97aa0-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="97aa0-117">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="97aa0-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="97aa0-118">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="97aa0-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="97aa0-119">Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.</span><span class="sxs-lookup"><span data-stu-id="97aa0-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="97aa0-120">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="97aa0-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="97aa0-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="97aa0-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="97aa0-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97aa0-122">Next steps</span></span>

<span data-ttu-id="97aa0-123">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="97aa0-123">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="97aa0-124">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97aa0-124">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
