---
title: "rozhraní příkazového řádku skriptu aaaAzure ukázkové – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
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
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="d3bf9-103">Vytvoření sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="d3bf9-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="d3bf9-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="d3bf9-105">Podsíť front-end toohello provozu je omezená tooHTTP a SSH, při provozu toohello back-end podsíť je omezená tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="d3bf9-106">Po spouštění skriptu hello budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="d3bf9-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d3bf9-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d3bf9-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d3bf9-108">Clean up deployment</span></span> 

<span data-ttu-id="d3bf9-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="d3bf9-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d3bf9-110">Script explanation</span></span>

<span data-ttu-id="d3bf9-111">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="d3bf9-112">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="d3bf9-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d3bf9-113">Command</span></span> | <span data-ttu-id="d3bf9-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d3bf9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d3bf9-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="d3bf9-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="d3bf9-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d3bf9-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="d3bf9-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="d3bf9-118">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="d3bf9-119">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="d3bf9-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="d3bf9-120">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-120">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="d3bf9-121">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="d3bf9-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="d3bf9-122">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="d3bf9-123">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="d3bf9-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="d3bf9-124">Vytvoří rozhraní virtuální sítě a připojí je toohello virtuální sítě front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-124">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="d3bf9-125">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="d3bf9-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="d3bf9-126">Vytvoří skupiny zabezpečení sítě (NSG), které jsou přidružené toohello front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-126">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="d3bf9-127">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="d3bf9-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="d3bf9-128">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty toospecific podsítě.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-128">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="d3bf9-129">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="d3bf9-129">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="d3bf9-130">Vytvoří virtuální počítače a připojí tooeach síťový adaptér virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-130">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="d3bf9-131">Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-131">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="d3bf9-132">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="d3bf9-132">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="d3bf9-133">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="d3bf9-133">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d3bf9-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3bf9-134">Next steps</span></span>

<span data-ttu-id="d3bf9-135">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d3bf9-135">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="d3bf9-136">Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="d3bf9-136">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
