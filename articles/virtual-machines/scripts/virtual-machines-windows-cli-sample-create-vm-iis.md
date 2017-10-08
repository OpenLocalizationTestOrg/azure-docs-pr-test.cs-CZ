---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - instalace služby IIS | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure – instalace služby IIS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 2fabc9522f424cab4c672084ba8bedfd623c87cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="1d5bc-103">Rychlé vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1d5bc-103">Quick Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="1d5bc-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016 a používá hello rozšíření vlastních skriptů Azure virtuálního počítače tooinstall služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-104">This script creates an Azure Virtual Machine running Windows Server 2016, and uses hello Azure Virtual Machine Custom Script Extension tooinstall IIS.</span></span> <span data-ttu-id="1d5bc-105">Po spuštění skriptu hello, budete mít přístup hello výchozí web služby IIS na hello veřejnou IP adresu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-105">After running hello script, you can access hello default IIS website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1d5bc-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1d5bc-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1d5bc-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="1d5bc-107">Clean up deployment</span></span> 

<span data-ttu-id="1d5bc-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="1d5bc-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1d5bc-109">Script explanation</span></span>

<span data-ttu-id="1d5bc-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="1d5bc-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1d5bc-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1d5bc-112">Command</span></span> | <span data-ttu-id="1d5bc-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1d5bc-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1d5bc-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="1d5bc-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1d5bc-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1d5bc-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="1d5bc-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="1d5bc-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="1d5bc-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="1d5bc-119">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="1d5bc-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="1d5bc-120">Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="1d5bc-121">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="1d5bc-122">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="1d5bc-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="1d5bc-123">Přidá a běží virtuálním počítači rozšíření tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="1d5bc-124">V této ukázce je rozšíření vlastních skriptů hello použité tooinstall služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-124">In this sample, hello custom script extension is used tooinstall IIS.</span></span>|
| [<span data-ttu-id="1d5bc-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="1d5bc-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="1d5bc-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="1d5bc-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d5bc-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d5bc-127">Next steps</span></span>

<span data-ttu-id="1d5bc-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1d5bc-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1d5bc-129">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d5bc-129">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
