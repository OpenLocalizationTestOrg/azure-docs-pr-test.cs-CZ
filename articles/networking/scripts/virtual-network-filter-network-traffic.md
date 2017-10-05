---
title: "Ukázka skriptu Azure CLI - provoz sítě virtuálních počítačů filtr | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – filtrovat příchozí a odchozí provoz sítě virtuálních počítačů."
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
ms.openlocfilehash: 68ee013cff4e0be15af30239e0314f779f50177a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="f4ec7-103">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f4ec7-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="f4ec7-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="f4ec7-105">Příchozí síťový provoz do front-endu podsíť je omezený na protokolu HTTP, HTTPS a SSH, zatímco odchozí provoz k Internetu z podsítě back-end není povolen.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-105">Inbound network traffic to the front-end subnet is limited to HTTP, HTTPS and SSH, while outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> <span data-ttu-id="f4ec7-106">Po spuštění skriptu, budete mít jeden virtuální počítač se dvěma síťovými adaptéry.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-106">After running the script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="f4ec7-107">Každý síťový adaptér je připojený k jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-107">Each NIC is connected to a different subnet.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f4ec7-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f4ec7-108">Sample script</span></span>


<span data-ttu-id="f4ec7-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "provoz sítě virtuálních počítačů filtru")]</span><span class="sxs-lookup"><span data-stu-id="f4ec7-109">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f4ec7-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f4ec7-110">Clean up deployment</span></span> 

<span data-ttu-id="f4ec7-111">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="f4ec7-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f4ec7-112">Script explanation</span></span>

<span data-ttu-id="f4ec7-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-113">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="f4ec7-114">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="f4ec7-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f4ec7-115">Command</span></span> | <span data-ttu-id="f4ec7-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f4ec7-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f4ec7-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f4ec7-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="f4ec7-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f4ec7-119">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="f4ec7-119">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="f4ec7-120">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-120">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="f4ec7-121">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="f4ec7-121">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="f4ec7-122">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-122">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="f4ec7-123">aktualizace az sítě vnet podsíť</span><span class="sxs-lookup"><span data-stu-id="f4ec7-123">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update) | <span data-ttu-id="f4ec7-124">Přidruží skupiny Nsg na podsítě.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-124">Associates NSGs to subnets.</span></span> |
| [<span data-ttu-id="f4ec7-125">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="f4ec7-125">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="f4ec7-126">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-126">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="f4ec7-127">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="f4ec7-127">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="f4ec7-128">Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-128">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="f4ec7-129">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="f4ec7-129">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="f4ec7-130">Vytvoří síť skupiny zabezpečení (NSG), které jsou přidruženy k podsítím front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-130">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="f4ec7-131">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="f4ec7-131">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="f4ec7-132">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-132">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="f4ec7-133">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="f4ec7-133">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="f4ec7-134">Vytvoří virtuální počítače a připojí síťovou kartu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-134">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="f4ec7-135">Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-135">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="f4ec7-136">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="f4ec7-136">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="f4ec7-137">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="f4ec7-137">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f4ec7-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4ec7-138">Next steps</span></span>

<span data-ttu-id="f4ec7-139">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f4ec7-139">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="f4ec7-140">Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="f4ec7-140">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>