---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vyrovnávat zatížení více webů s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vyrovnávat zatížení více webů toohello stejného virtuálního počítače"
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
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="a9279-103">Vyrovnávat zatížení více webů</span><span class="sxs-lookup"><span data-stu-id="a9279-103">Load balance multiple websites</span></span>

<span data-ttu-id="a9279-104">Tento ukázkový skript vytvoří virtuální síť s dva virtuální počítače (VM), které jsou členy skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a9279-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="a9279-105">Nástroj pro vyrovnávání zatížení přesměruje přenosy pro dva samostatné IP adres toohello dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a9279-105">A load balancer directs traffic for two separate IP addresses toohello two VMs.</span></span> <span data-ttu-id="a9279-106">Po spouštění skriptu hello můžete nasadit webový server softwaru toohello virtuální počítače a hostování více webů, každou s vlastní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a9279-106">After running hello script, you could deploy web server software toohello VMs and host multiple web sites, each with its own IP address.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a9279-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a9279-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a9279-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="a9279-108">Clean up deployment</span></span> 

<span data-ttu-id="a9279-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="a9279-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="a9279-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a9279-110">Script explanation</span></span>

<span data-ttu-id="a9279-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků virtuální sítě, zatížení vyrovnávání a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="a9279-111">This script uses hello following commands toocreate a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="a9279-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a9279-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a9279-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a9279-113">Command</span></span> | <span data-ttu-id="a9279-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a9279-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a9279-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="a9279-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a9279-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="a9279-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a9279-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="a9279-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="a9279-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="a9279-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="a9279-119">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="a9279-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="a9279-120">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="a9279-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="a9279-121">Vytvoření sítě lb az</span><span class="sxs-lookup"><span data-stu-id="a9279-121">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="a9279-122">Vytvoří k nástroji pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="a9279-122">Creates an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="a9279-123">Vytvoření az sítě lb testu</span><span class="sxs-lookup"><span data-stu-id="a9279-123">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="a9279-124">Vytvoří sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a9279-124">Creates a load balancer probe.</span></span> <span data-ttu-id="a9279-125">Sondu nástroje pro vyrovnávání zatížení je použité toomonitor každý virtuální počítač v sadě nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="a9279-125">A load balancer probe is used toomonitor each VM in hello load balancer set.</span></span> <span data-ttu-id="a9279-126">Pokud žádné virtuální počítače bude nedostupné, není provoz směruje toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a9279-126">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="a9279-127">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="a9279-127">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="a9279-128">Vytvoří pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a9279-128">Creates a load balancer rule.</span></span> <span data-ttu-id="a9279-129">V této ukázce se vytvoří pravidlo pro port 80.</span><span class="sxs-lookup"><span data-stu-id="a9279-129">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="a9279-130">HTTP přenos dorazí na hello nástroj pro vyrovnávání zatížení, je směrované tooport 80 jeden hello virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="a9279-130">As HTTP traffic arrives at hello load balancer, it is routed tooport 80 one of hello VMs in hello load balancer set.</span></span> |
| [<span data-ttu-id="a9279-131">Vytvoření az sítě lb ip front-endu-</span><span class="sxs-lookup"><span data-stu-id="a9279-131">az network lb frontend-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | <span data-ttu-id="a9279-132">Vytvoření front-endovou IP adresy pro hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a9279-132">Create a frontend IP address for hello Load Balancer.</span></span> |
| [<span data-ttu-id="a9279-133">Vytvoření az sítě lb fond adres</span><span class="sxs-lookup"><span data-stu-id="a9279-133">az network lb address-pool create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | <span data-ttu-id="a9279-134">Vytvoří fond back-end adresy.</span><span class="sxs-lookup"><span data-stu-id="a9279-134">Creates a backend address pool.</span></span> |
| [<span data-ttu-id="a9279-135">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="a9279-135">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="a9279-136">Vytvoří virtuální síťové karty a připojí jej toohello virtuální síť a podsíť.</span><span class="sxs-lookup"><span data-stu-id="a9279-136">Creates a virtual network card and attaches it toohello virtual network, and subnet.</span></span> |
| [<span data-ttu-id="a9279-137">Vytvoření virtuálního počítače az sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a9279-137">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="a9279-138">Vytvoří skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a9279-138">Creates an availability set.</span></span> <span data-ttu-id="a9279-139">Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se hello virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena hello celou sadu.</span><span class="sxs-lookup"><span data-stu-id="a9279-139">Availability sets ensure application uptime by spreading hello virtual machines across physical resources such that if failure occurs, hello entire set is not effected.</span></span> |
| [<span data-ttu-id="a9279-140">Seskupování síťových az ip-config vytvořit</span><span class="sxs-lookup"><span data-stu-id="a9279-140">az network nic ip-config create</span></span>](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | <span data-ttu-id="a9279-141">Vytvoří confiuration IP.</span><span class="sxs-lookup"><span data-stu-id="a9279-141">Creates an IP confiuration.</span></span> <span data-ttu-id="a9279-142">Musíte mít hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic funkce pro vaše předplatné povolený.</span><span class="sxs-lookup"><span data-stu-id="a9279-142">You must have hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic feature enabled for your subscription.</span></span> <span data-ttu-id="a9279-143">Konfiguraci pouze jednoho může být určen jako hello primární konfiguraci IP adresy pro síťový adaptér, pomocí hello – příznak změnit na primární.</span><span class="sxs-lookup"><span data-stu-id="a9279-143">Only one configuration may be designated as hello primary IP configuration per NIC, using hello --make-primary flag.</span></span> |
| [<span data-ttu-id="a9279-144">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="a9279-144">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="a9279-145">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="a9279-145">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="a9279-146">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="a9279-146">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="a9279-147">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="a9279-147">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="a9279-148">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="a9279-148">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a9279-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9279-149">Next steps</span></span>

<span data-ttu-id="a9279-150">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a9279-150">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a9279-151">Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [přehled sítě Azure dokumentaci](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a9279-151">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
