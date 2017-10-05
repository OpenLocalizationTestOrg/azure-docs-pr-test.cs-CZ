---
title: "Azure CLI skriptu ukázkové – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuální sítě pro vícevrstvé aplikace."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="bb088-103">Vytvoření sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="bb088-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="bb088-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="bb088-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="bb088-105">Provoz do front-endu podsítě je omezen na protokolu HTTP a SSH, zatímco provozu do podsítě back-end je omezený na MySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="bb088-105">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> <span data-ttu-id="bb088-106">Po spuštění skriptu, budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.</span><span class="sxs-lookup"><span data-stu-id="bb088-106">After running the script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="bb088-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bb088-107">Sample script</span></span>


<span data-ttu-id="bb088-108">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "virtuální sítě pro vícevrstvé aplikace")]</span><span class="sxs-lookup"><span data-stu-id="bb088-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bb088-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bb088-109">Clean up deployment</span></span> 

<span data-ttu-id="bb088-110">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="bb088-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="bb088-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bb088-111">Script explanation</span></span>

<span data-ttu-id="bb088-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="bb088-112">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="bb088-113">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="bb088-113">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="bb088-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bb088-114">Command</span></span> | <span data-ttu-id="bb088-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb088-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bb088-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bb088-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="bb088-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bb088-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bb088-118">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="bb088-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="bb088-119">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="bb088-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="bb088-120">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="bb088-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="bb088-121">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="bb088-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="bb088-122">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="bb088-122">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="bb088-123">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="bb088-123">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="bb088-124">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="bb088-124">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="bb088-125">Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="bb088-125">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="bb088-126">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="bb088-126">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="bb088-127">Vytvoří síť skupiny zabezpečení (NSG), které jsou přidruženy k podsítím front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="bb088-127">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="bb088-128">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="bb088-128">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="bb088-129">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím.</span><span class="sxs-lookup"><span data-stu-id="bb088-129">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="bb088-130">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="bb088-130">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="bb088-131">Vytvoří virtuální počítače a připojí síťovou kartu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bb088-131">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="bb088-132">Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="bb088-132">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="bb088-133">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="bb088-133">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="bb088-134">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="bb088-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bb088-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb088-135">Next steps</span></span>

<span data-ttu-id="bb088-136">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bb088-136">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="bb088-137">Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="bb088-137">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>