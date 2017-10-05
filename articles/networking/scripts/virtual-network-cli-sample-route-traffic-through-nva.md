---
title: "Ukázka skriptu Azure CLI - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
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
ms.openlocfilehash: 78091b515c00591a4af8d807945475b6be50188a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="7292b-103">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="7292b-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="7292b-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="7292b-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="7292b-105">Také vytvoří virtuální počítač se ke směrování provozu mezi dvěma podsítěmi povolené předávání IP.</span><span class="sxs-lookup"><span data-stu-id="7292b-105">It also creates a VM with IP forwarding enabled to route traffic between the two subnets.</span></span> <span data-ttu-id="7292b-106">Po spuštění skriptu můžete nasadit software pro sítě, jako je například Brána firewall aplikace, do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7292b-106">After running the script you can deploy network software, such as a firewall application, to the VM.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="7292b-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7292b-107">Sample script</span></span>


<span data-ttu-id="7292b-108">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "směrování provozu prostřednictvím sítě virtuálního zařízení")]</span><span class="sxs-lookup"><span data-stu-id="7292b-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="7292b-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="7292b-109">Clean up deployment</span></span> 

<span data-ttu-id="7292b-110">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="7292b-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="7292b-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7292b-111">Script explanation</span></span>

<span data-ttu-id="7292b-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="7292b-112">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="7292b-113">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="7292b-113">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="7292b-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7292b-114">Command</span></span> | <span data-ttu-id="7292b-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7292b-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7292b-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="7292b-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="7292b-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7292b-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7292b-118">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="7292b-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="7292b-119">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="7292b-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="7292b-120">Vytvoření podsítě az sítě</span><span class="sxs-lookup"><span data-stu-id="7292b-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="7292b-121">Vytvoří back-end a DMZ podsítě.</span><span class="sxs-lookup"><span data-stu-id="7292b-121">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="7292b-122">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="7292b-122">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="7292b-123">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7292b-123">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="7292b-124">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="7292b-124">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="7292b-125">Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni.</span><span class="sxs-lookup"><span data-stu-id="7292b-125">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="7292b-126">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="7292b-126">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="7292b-127">Vytvoří skupinu zabezpečení sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="7292b-127">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="7292b-128">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="7292b-128">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) | <span data-ttu-id="7292b-129">Vytvoří pravidla NSG, které umožní příchozí porty HTTP a HTTPS k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7292b-129">Creates NSG rules that allow HTTP and HTTPS ports inbound to the VM.</span></span> |
| [<span data-ttu-id="7292b-130">aktualizace az sítě vnet podsíť</span><span class="sxs-lookup"><span data-stu-id="7292b-130">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update)| <span data-ttu-id="7292b-131">Přidruží skupiny Nsg a směrovací tabulky k podsítím.</span><span class="sxs-lookup"><span data-stu-id="7292b-131">Associates the NSGs and route tables to subnets.</span></span> |
| [<span data-ttu-id="7292b-132">Vytvoření sítě az trasy – tabulka</span><span class="sxs-lookup"><span data-stu-id="7292b-132">az network route-table create</span></span>](/cli/azure/network/route-table#create)| <span data-ttu-id="7292b-133">Vytvoří směrovací tabulku pro všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="7292b-133">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="7292b-134">Vytvoření az síťovou směrovací tabulku trasu</span><span class="sxs-lookup"><span data-stu-id="7292b-134">az network route-table route create</span></span>](/cli/azure/network/route-table/route#create)| <span data-ttu-id="7292b-135">Vytvoří směrování směrovat provoz mezi podsítěmi a Internetu prostřednictvím virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7292b-135">Creates routes to route traffic between subnets and the Internet through the VM.</span></span> |
| [<span data-ttu-id="7292b-136">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="7292b-136">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="7292b-137">Vytvoří virtuální počítač a k němu připojí na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="7292b-137">Creates a virtual machine and attaches the NIC to it.</span></span> <span data-ttu-id="7292b-138">Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="7292b-138">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="7292b-139">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="7292b-139">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="7292b-140">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="7292b-140">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7292b-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7292b-141">Next steps</span></span>

<span data-ttu-id="7292b-142">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7292b-142">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="7292b-143">Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="7292b-143">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>