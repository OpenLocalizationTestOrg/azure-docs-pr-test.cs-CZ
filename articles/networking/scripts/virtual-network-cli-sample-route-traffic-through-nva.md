---
title: "Ukázka skriptu aaaAzure rozhraní příkazového řádku - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure CLI ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
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
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="ec8a2-103">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="ec8a2-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="ec8a2-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="ec8a2-105">Vytvoří také virtuální počítač s IP Adresou předávání povoleno tooroute provozu mezi dvěma podsítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-105">It also creates a VM with IP forwarding enabled tooroute traffic between hello two subnets.</span></span> <span data-ttu-id="ec8a2-106">Po spuštění skriptu hello můžete nasadit software pro sítě, jako například aplikací brány firewall, toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-106">After running hello script you can deploy network software, such as a firewall application, toohello VM.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="ec8a2-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ec8a2-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ec8a2-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="ec8a2-108">Clean up deployment</span></span> 

<span data-ttu-id="ec8a2-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="ec8a2-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ec8a2-110">Script explanation</span></span>

<span data-ttu-id="ec8a2-111">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="ec8a2-112">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="ec8a2-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ec8a2-113">Command</span></span> | <span data-ttu-id="ec8a2-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ec8a2-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ec8a2-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ec8a2-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="ec8a2-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ec8a2-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="ec8a2-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="ec8a2-118">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="ec8a2-119">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="ec8a2-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="ec8a2-120">Vytvoří back-end a DMZ podsítě.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-120">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="ec8a2-121">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="ec8a2-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="ec8a2-122">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="ec8a2-123">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="ec8a2-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="ec8a2-124">Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-124">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="ec8a2-125">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="ec8a2-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="ec8a2-126">Vytvoří skupinu zabezpečení sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="ec8a2-126">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="ec8a2-127">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="ec8a2-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) | <span data-ttu-id="ec8a2-128">Vytvoří pravidla NSG, které umožňují, že porty HTTP a HTTPS příchozí toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-128">Creates NSG rules that allow HTTP and HTTPS ports inbound toohello VM.</span></span> |
| [<span data-ttu-id="ec8a2-129">aktualizace az sítě vnet podsíť</span><span class="sxs-lookup"><span data-stu-id="ec8a2-129">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update)| <span data-ttu-id="ec8a2-130">Partnerů hello skupiny Nsg a toosubnets tabulky trasy.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-130">Associates hello NSGs and route tables toosubnets.</span></span> |
| [<span data-ttu-id="ec8a2-131">Vytvoření sítě az trasy – tabulka</span><span class="sxs-lookup"><span data-stu-id="ec8a2-131">az network route-table create</span></span>](/cli/azure/network/route-table#create)| <span data-ttu-id="ec8a2-132">Vytvoří směrovací tabulku pro všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-132">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="ec8a2-133">Vytvoření az síťovou směrovací tabulku trasu</span><span class="sxs-lookup"><span data-stu-id="ec8a2-133">az network route-table route create</span></span>](/cli/azure/network/route-table/route#create)| <span data-ttu-id="ec8a2-134">Vytvoří trasy tooroute provoz mezi podsítěmi a hello Internet prostřednictvím hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-134">Creates routes tooroute traffic between subnets and hello Internet through hello VM.</span></span> |
| [<span data-ttu-id="ec8a2-135">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="ec8a2-135">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="ec8a2-136">Vytvoří virtuální počítač a připojí tooit hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-136">Creates a virtual machine and attaches hello NIC tooit.</span></span> <span data-ttu-id="ec8a2-137">Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-137">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="ec8a2-138">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="ec8a2-138">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="ec8a2-139">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="ec8a2-139">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ec8a2-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec8a2-140">Next steps</span></span>

<span data-ttu-id="ec8a2-141">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ec8a2-141">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="ec8a2-142">Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="ec8a2-142">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
