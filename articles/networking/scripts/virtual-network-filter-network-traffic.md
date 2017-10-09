---
title: "Ukázka skriptu aaaAzure rozhraní příkazového řádku - provoz sítě virtuálních počítačů filtr | Microsoft Docs"
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
ms.openlocfilehash: c2f14e54bc96c99420b4300d1c24a457ac8c948c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="9e7d3-103">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9e7d3-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="9e7d3-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="9e7d3-105">Příchozí síťový provoz toohello front-end podsíť je omezená tooHTTP, HTTPS a SSH, zatímco odchozí provoz toohello Internetu z podsítě hello back-end není povoleno.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="9e7d3-106">Po spuštění skriptu hello, budete mít jeden virtuální počítač se dvěma síťovými adaptéry.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="9e7d3-107">Každý síťový adaptér je připojený tooa jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-107">Each NIC is connected tooa different subnet.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9e7d3-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9e7d3-108">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9e7d3-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9e7d3-109">Clean up deployment</span></span> 

<span data-ttu-id="9e7d3-110">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="9e7d3-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9e7d3-111">Script explanation</span></span>

<span data-ttu-id="9e7d3-112">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="9e7d3-113">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="9e7d3-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9e7d3-114">Command</span></span> | <span data-ttu-id="9e7d3-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9e7d3-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9e7d3-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="9e7d3-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="9e7d3-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9e7d3-118">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="9e7d3-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="9e7d3-119">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="9e7d3-120">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="9e7d3-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="9e7d3-121">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="9e7d3-122">aktualizace az sítě vnet podsíť</span><span class="sxs-lookup"><span data-stu-id="9e7d3-122">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update) | <span data-ttu-id="9e7d3-123">Přidruží toosubnets skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-123">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="9e7d3-124">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="9e7d3-124">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="9e7d3-125">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-125">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="9e7d3-126">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="9e7d3-126">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="9e7d3-127">Vytvoří rozhraní virtuální sítě a připojí je toohello virtuální sítě front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-127">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="9e7d3-128">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="9e7d3-128">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="9e7d3-129">Vytvoří skupiny zabezpečení sítě (NSG), které jsou přidružené toohello front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-129">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="9e7d3-130">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="9e7d3-130">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="9e7d3-131">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty toospecific podsítě.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-131">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="9e7d3-132">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="9e7d3-132">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="9e7d3-133">Vytvoří virtuální počítače a připojí tooeach síťový adaptér virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-133">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="9e7d3-134">Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-134">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="9e7d3-135">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="9e7d3-135">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="9e7d3-136">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="9e7d3-136">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9e7d3-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e7d3-137">Next steps</span></span>

<span data-ttu-id="9e7d3-138">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9e7d3-138">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="9e7d3-139">Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="9e7d3-139">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
