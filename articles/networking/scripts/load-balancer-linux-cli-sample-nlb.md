---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - tooVMs provoz Vyrovnávání zatížení pro zajištění vysoké dostupnosti | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - tooVMs provoz Vyrovnávání zatížení pro zajištění vysoké dostupnosti"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 0954b5c261512724dfb9c6e7be123c9d45624f4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a><span data-ttu-id="37492-103">Načíst vyrovnávat přenosy tooVMs pro zajištění vysoké dostupnosti</span><span class="sxs-lookup"><span data-stu-id="37492-103">Load balance traffic tooVMs for high availability</span></span>

<span data-ttu-id="37492-104">Tento ukázkový skript vytvoří vše potřebné vyrovnáváním toorun několik Ubuntu virtuální počítače nakonfigurované v s vysokou dostupností a načtení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="37492-104">This script sample creates everything needed toorun several Ubuntu virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="37492-105">Po spouštění skriptu hello, budete mít tři virtuální počítače připojené k tooan Azure skupiny dostupnosti a je přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="37492-105">After running hello script, you will have three virtual machines, joined tooan Azure Availability Set, and accessible through an Azure Load Balancer.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="37492-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="37492-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="37492-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="37492-107">Clean up deployment</span></span> 

<span data-ttu-id="37492-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="37492-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="37492-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="37492-109">Script explanation</span></span>

<span data-ttu-id="37492-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="37492-110">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="37492-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="37492-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="37492-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="37492-112">Command</span></span> | <span data-ttu-id="37492-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37492-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="37492-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="37492-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="37492-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="37492-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="37492-116">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="37492-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="37492-117">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="37492-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="37492-118">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="37492-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="37492-119">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="37492-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="37492-120">Vytvoření sítě lb az</span><span class="sxs-lookup"><span data-stu-id="37492-120">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="37492-121">Vytvoří k nástroji pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="37492-121">Creates an Azure load balancer.</span></span> |
| [<span data-ttu-id="37492-122">Vytvoření az sítě lb testu</span><span class="sxs-lookup"><span data-stu-id="37492-122">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="37492-123">Vytvoří sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="37492-123">Creates a load balancer probe.</span></span> <span data-ttu-id="37492-124">Sondu nástroje pro vyrovnávání zatížení je použité toomonitor každý virtuální počítač v sadě nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="37492-124">A load balancer probe is used toomonitor each VM in hello load balancer set.</span></span> <span data-ttu-id="37492-125">Pokud žádné virtuální počítače bude nedostupné, není provoz směruje toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37492-125">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="37492-126">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="37492-126">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="37492-127">Vytvoří pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="37492-127">Creates a load balancer rule.</span></span> <span data-ttu-id="37492-128">V této ukázce se vytvoří pravidlo pro port 80.</span><span class="sxs-lookup"><span data-stu-id="37492-128">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="37492-129">HTTP přenos dorazí na hello nástroj pro vyrovnávání zatížení, je směrované tooport 80 jeden hello virtuálních počítačů v sadě hello LB.</span><span class="sxs-lookup"><span data-stu-id="37492-129">As HTTP traffic arrives at hello load balancer, it is routed tooport 80 one of hello VMs in hello LB set.</span></span> |
| [<span data-ttu-id="37492-130">Vytvoření az sítě lb příchozí--pravidlo nat</span><span class="sxs-lookup"><span data-stu-id="37492-130">az network lb inbound-nat-rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | <span data-ttu-id="37492-131">Vytvoří pravidlo překladu adres (NAT) pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="37492-131">Creates load balancer Network Address Translation (NAT) rule.</span></span>  <span data-ttu-id="37492-132">Pravidla NAT namapovat port hello zatížení vyrovnávání tooa portu na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="37492-132">NAT rules map a port of hello load balancer tooa port on a VM.</span></span> <span data-ttu-id="37492-133">V této ukázce se vytvoří pravidlo NAT pro SSH provoz tooeach virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="37492-133">In this sample, a NAT rule is created for SSH traffic tooeach VM in hello load balancer set.</span></span>  |
| [<span data-ttu-id="37492-134">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="37492-134">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="37492-135">Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi hello Internetu a hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="37492-135">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="37492-136">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="37492-136">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="37492-137">Vytvoří tooallow pravidla NSG příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="37492-137">Creates an NSG rule tooallow inbound traffic.</span></span> <span data-ttu-id="37492-138">V této ukázce je otevřen port 22 pro SSH provoz.</span><span class="sxs-lookup"><span data-stu-id="37492-138">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="37492-139">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="37492-139">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="37492-140">Vytvoří virtuální síťové karty a připojí jej toohello virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="37492-140">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="37492-141">Vytvoření virtuálního počítače az sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="37492-141">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="37492-142">Vytvoří skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="37492-142">Creates an availability set.</span></span> <span data-ttu-id="37492-143">Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se hello virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena hello celou sadu.</span><span class="sxs-lookup"><span data-stu-id="37492-143">Availability sets ensure application uptime by spreading hello virtual machines across physical resources such that if failure occurs, hello entire set is not effected.</span></span> |
| [<span data-ttu-id="37492-144">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="37492-144">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="37492-145">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="37492-145">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="37492-146">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="37492-146">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="37492-147">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="37492-147">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="37492-148">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="37492-148">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="37492-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37492-149">Next steps</span></span>

<span data-ttu-id="37492-150">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="37492-150">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="37492-151">Další ukázky skriptu síťových rozhraní příkazového řádku Azure lze nalézt v hello [sítí Azure dokumentaci](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="37492-151">Additional Azure Networking CLI script samples can be found in hello [Azure Networking documentation](../cli-samples.md).</span></span>
