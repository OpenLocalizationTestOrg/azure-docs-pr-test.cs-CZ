---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG"
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
ms.openlocfilehash: ba6a70200ca2923369e37b13531bd7ca65b05a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="6843d-103">Zabezpečení síťového provozu mezi virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="6843d-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="6843d-104">Tento skript vytvoří dva virtuální počítače a zabezpečuje tooboth příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="6843d-104">This script creates two virtual machines and secures incoming traffic tooboth.</span></span> <span data-ttu-id="6843d-105">Jednou z virtuálního počítače je dostupný na Internetu hello a skupinu zabezpečení sítě (NSG) nakonfiguroval tooallow provozu na port 22 a port 80.</span><span class="sxs-lookup"><span data-stu-id="6843d-105">One virtual machine is accessible on hello internet and has a network security group (NSG) configured tooallow traffic on port 22 and port 80.</span></span> <span data-ttu-id="6843d-106">Hello druhý virtuální počítač není přístupný na hello Internetu, a má tooonly NSG nakonfigurované povolit provoz z hello prvním virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6843d-106">hello second virtual machine is not accessible on hello internet, and has an NSG configured tooonly allow traffic from hello first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6843d-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6843d-107">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a><span data-ttu-id="6843d-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="6843d-108">Clean up deployment</span></span> 

<span data-ttu-id="6843d-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="6843d-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="6843d-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6843d-110">Script explanation</span></span>

<span data-ttu-id="6843d-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6843d-111">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="6843d-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6843d-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6843d-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6843d-113">Command</span></span> | <span data-ttu-id="6843d-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6843d-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6843d-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6843d-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6843d-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6843d-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6843d-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="6843d-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="6843d-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="6843d-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="6843d-119">Vytvoření az podsíti virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6843d-119">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="6843d-120">Vytvoří podsíť.</span><span class="sxs-lookup"><span data-stu-id="6843d-120">Creates a subnet.</span></span> |
| [<span data-ttu-id="6843d-121">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="6843d-121">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6843d-122">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="6843d-122">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="6843d-123">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="6843d-123">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="6843d-124">seznam pravidel nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="6843d-124">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="6843d-125">Vrací informace o pravidlo pro skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="6843d-125">Returns information about a network security group rule.</span></span> <span data-ttu-id="6843d-126">V této ukázce je název pravidla hello uložené v proměnné pro použití novější ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="6843d-126">In this sample, hello rule name is stored in a variable for use later in hello script.</span></span> |
| [<span data-ttu-id="6843d-127">aktualizace pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="6843d-127">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="6843d-128">Aktualizuje pravidlo NSG.</span><span class="sxs-lookup"><span data-stu-id="6843d-128">Updates an NSG rule.</span></span> <span data-ttu-id="6843d-129">V této ukázce je pravidlo back-end hello aktualizované toopass prostřednictvím jenom z podsítě front-endu hello.</span><span class="sxs-lookup"><span data-stu-id="6843d-129">In this sample, hello back-end rule is updated toopass through traffic only from hello front-end subnet.</span></span> |
| [<span data-ttu-id="6843d-130">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="6843d-130">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6843d-131">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="6843d-131">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6843d-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6843d-132">Next steps</span></span>

<span data-ttu-id="6843d-133">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6843d-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6843d-134">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6843d-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
