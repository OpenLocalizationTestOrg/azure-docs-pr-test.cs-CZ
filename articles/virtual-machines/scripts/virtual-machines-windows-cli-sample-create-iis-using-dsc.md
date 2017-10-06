---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit virtuální počítač s Windows Server 2016 se službou IIS pomocí DSC | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 se službou IIS pomocí DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 9883b5925dddca3dd53d083679a0e76d0fb982e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a><span data-ttu-id="d12cd-103">Vytvoření virtuálního počítače se službou IIS pomocí DSC</span><span class="sxs-lookup"><span data-stu-id="d12cd-103">Create a VM with IIS using DSC</span></span>

<span data-ttu-id="d12cd-104">Tento skript vytvoří virtuální počítač a používá tooinstall rozšíření vlastních skriptů DSC virtuálního počítače Azure hello a nakonfigurovat službu IIS.</span><span class="sxs-lookup"><span data-stu-id="d12cd-104">This script creates a virtual machine, and uses hello Azure Virtual Machine DSC custom script extension tooinstall and configure IIS.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d12cd-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d12cd-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d12cd-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d12cd-106">Clean up deployment</span></span> 

<span data-ttu-id="d12cd-107">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="d12cd-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="d12cd-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d12cd-108">Script explanation</span></span>

<span data-ttu-id="d12cd-109">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="d12cd-109">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="d12cd-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d12cd-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d12cd-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d12cd-111">Command</span></span> | <span data-ttu-id="d12cd-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d12cd-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d12cd-113">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="d12cd-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d12cd-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d12cd-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d12cd-115">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="d12cd-115">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="d12cd-116">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="d12cd-116">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="d12cd-117">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="d12cd-117">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="d12cd-118">nastavení rozšíření virtuálního az</span><span class="sxs-lookup"><span data-stu-id="d12cd-118">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="d12cd-119">Přidáte hello rozšíření vlastních skriptů toohello virtuální počítač, který vyvolává skript tooinstall služby IIS.</span><span class="sxs-lookup"><span data-stu-id="d12cd-119">Add hello Custom Script Extension toohello virtual machine which invokes a script tooinstall IIS.</span></span> |
| [<span data-ttu-id="d12cd-120">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="d12cd-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="d12cd-121">Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="d12cd-121">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="d12cd-122">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d12cd-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="d12cd-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="d12cd-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="d12cd-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d12cd-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d12cd-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d12cd-125">Next steps</span></span>

<span data-ttu-id="d12cd-126">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d12cd-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d12cd-127">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12cd-127">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
