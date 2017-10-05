---
title: "Azure CLI skriptu ukázkové – vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: cc80e3fc5aaa12200e9a441db9d4a9c613c0548a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="5432b-103">Zabezpečení síťového provozu mezi virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="5432b-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="5432b-104">Tento skript vytvoří dva virtuální počítače a zabezpečuje příchozí provoz do obou.</span><span class="sxs-lookup"><span data-stu-id="5432b-104">This script creates two virtual machines and secures incoming traffic to both.</span></span> <span data-ttu-id="5432b-105">Jeden virtuální počítač je přístupný v síti internet a pokud chcete povolit přenosy na portu 3389 a port 80 nakonfiguroval skupinu zabezpečení sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="5432b-105">One virtual machine is accessible on the internet and has a network security group (NSG) configured to allow traffic on port 3389 and port 80.</span></span> <span data-ttu-id="5432b-106">Druhý virtuální počítač není přístupné z Internetu a možnost Povolit jenom přenos z první virtuální počítač má nakonfigurované skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="5432b-106">The second virtual machine is not accessible on the internet, and has an NSG configured to only allow traffic from the first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5432b-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5432b-107">Sample script</span></span>

<span data-ttu-id="5432b-108">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "vytvoření virtuálního počítače s NSG")]</span><span class="sxs-lookup"><span data-stu-id="5432b-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5432b-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5432b-109">Clean up deployment</span></span> 

<span data-ttu-id="5432b-110">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5432b-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5432b-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5432b-111">Script explanation</span></span>

<span data-ttu-id="5432b-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5432b-112">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5432b-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="5432b-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5432b-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5432b-114">Command</span></span> | <span data-ttu-id="5432b-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5432b-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5432b-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="5432b-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5432b-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5432b-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5432b-118">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="5432b-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="5432b-119">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="5432b-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="5432b-120">Vytvoření az podsíti virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="5432b-120">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="5432b-121">Vytvoří podsíť.</span><span class="sxs-lookup"><span data-stu-id="5432b-121">Creates a subnet.</span></span> |
| [<span data-ttu-id="5432b-122">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="5432b-122">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5432b-123">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="5432b-123">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="5432b-124">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="5432b-124">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="5432b-125">aktualizace pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="5432b-125">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="5432b-126">Aktualizuje pravidlo NSG.</span><span class="sxs-lookup"><span data-stu-id="5432b-126">Updates an NSG rule.</span></span> <span data-ttu-id="5432b-127">V této ukázce se aktualizuje pravidlo back-end předávat provoz jenom z podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="5432b-127">In this sample, the back-end rule is updated to pass through traffic only from the front-end subnet.</span></span> |
| [<span data-ttu-id="5432b-128">seznam pravidel nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="5432b-128">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="5432b-129">Vrací informace o pravidlo pro skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="5432b-129">Returns information about a network security group rule.</span></span> <span data-ttu-id="5432b-130">V této ukázce je název pravidla uložené v proměnné pro použití novější ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="5432b-130">In this sample, the rule name is stored in a variable for use later in the script.</span></span> |
| [<span data-ttu-id="5432b-131">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="5432b-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5432b-132">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5432b-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5432b-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5432b-133">Next steps</span></span>

<span data-ttu-id="5432b-134">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5432b-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5432b-135">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5432b-135">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
