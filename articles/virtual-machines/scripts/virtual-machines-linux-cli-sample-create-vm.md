---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem"
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
ms.openlocfilehash: c9855204a85cc0f6cd758a1d20121fbeea070943
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="8b3b0-103">Vytvoření kompletně nakonfigurovaný virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8b3b0-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="8b3b0-104">Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="8b3b0-105">Po spuštění skriptu hello, můžete přistupovat přes protokol SSH hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8b3b0-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8b3b0-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8b3b0-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8b3b0-107">Clean up deployment</span></span> 

<span data-ttu-id="8b3b0-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8b3b0-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8b3b0-109">Script explanation</span></span>

<span data-ttu-id="8b3b0-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="8b3b0-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8b3b0-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8b3b0-112">Command</span></span> | <span data-ttu-id="8b3b0-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8b3b0-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8b3b0-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="8b3b0-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8b3b0-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8b3b0-116">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="8b3b0-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="8b3b0-117">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="8b3b0-118">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="8b3b0-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="8b3b0-119">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="8b3b0-120">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="8b3b0-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="8b3b0-121">Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi hello Internetu a hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="8b3b0-122">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="8b3b0-122">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="8b3b0-123">Vytvoří tooallow pravidla NSG příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-123">Creates an NSG rule tooallow inbound traffic.</span></span> <span data-ttu-id="8b3b0-124">V této ukázce je otevřen port 22 pro SSH provoz.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-124">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="8b3b0-125">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="8b3b0-125">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="8b3b0-126">Vytvoří virtuální síťové karty a připojí jej toohello virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-126">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="8b3b0-127">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="8b3b0-127">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8b3b0-128">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-128">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="8b3b0-129">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-129">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="8b3b0-130">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="8b3b0-130">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8b3b0-131">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="8b3b0-131">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8b3b0-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b3b0-132">Next steps</span></span>

<span data-ttu-id="8b3b0-133">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8b3b0-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8b3b0-134">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8b3b0-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
