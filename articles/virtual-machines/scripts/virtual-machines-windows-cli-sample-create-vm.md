---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Windows serveru | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálních počítačů Windows serveru"
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
ms.author: rclaus
ms.openlocfilehash: f4cb431a68c911f877f5af87c393849bd8d2676a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="87a3e-103">Vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="87a3e-103">Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="87a3e-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="87a3e-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="87a3e-105">Po spuštění skriptu hello, dostanete hello virtuálního počítače přes připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="87a3e-105">After running hello script, you can access hello virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="87a3e-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="87a3e-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="87a3e-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="87a3e-107">Clean up deployment</span></span> 

<span data-ttu-id="87a3e-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="87a3e-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="87a3e-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="87a3e-109">Script explanation</span></span>

<span data-ttu-id="87a3e-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="87a3e-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="87a3e-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="87a3e-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="87a3e-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="87a3e-112">Command</span></span> | <span data-ttu-id="87a3e-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="87a3e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="87a3e-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="87a3e-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="87a3e-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="87a3e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="87a3e-116">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="87a3e-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="87a3e-117">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="87a3e-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="87a3e-118">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="87a3e-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="87a3e-119">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="87a3e-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="87a3e-120">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="87a3e-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="87a3e-121">Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi hello Internetu a hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="87a3e-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="87a3e-122">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="87a3e-122">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="87a3e-123">Vytvoří virtuální síťové karty a připojí jej toohello virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="87a3e-123">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="87a3e-124">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="87a3e-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="87a3e-125">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="87a3e-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="87a3e-126">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="87a3e-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="87a3e-127">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="87a3e-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="87a3e-128">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="87a3e-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="87a3e-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87a3e-129">Next steps</span></span>

<span data-ttu-id="87a3e-130">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="87a3e-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="87a3e-131">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="87a3e-131">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
