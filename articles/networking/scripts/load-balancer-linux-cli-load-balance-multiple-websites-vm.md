---
title: "Ukázka skriptu Azure CLI - vyrovnávat zatížení více webů pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vyrovnávat zatížení více webů na jednom virtuálním počítači"
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
ms.openlocfilehash: c5a584b33025122033b930822ae0a0864a7ec1cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="5e3b1-103">Vyrovnávat zatížení více webů</span><span class="sxs-lookup"><span data-stu-id="5e3b1-103">Load balance multiple websites</span></span>

<span data-ttu-id="5e3b1-104">Tento ukázkový skript vytvoří virtuální síť s dva virtuální počítače (VM), které jsou členy skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="5e3b1-105">Nástroj pro vyrovnávání zatížení bude směrovat provoz pro dva samostatné IP adresy na dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-105">A load balancer directs traffic for two separate IP addresses to the two VMs.</span></span> <span data-ttu-id="5e3b1-106">Po spuštění skriptu, můžete nasadit software webového serveru do virtuálních počítačů a hostitelů více webových serverů, každý s vlastní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-106">After running the script, you could deploy web server software to the VMs and host multiple web sites, each with its own IP address.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5e3b1-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5e3b1-107">Sample script</span></span>


<span data-ttu-id="5e3b1-108">[!code-azurecli-interactive[hlavní](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "vyrovnávat zatížení více webových serverů")]</span><span class="sxs-lookup"><span data-stu-id="5e3b1-108">[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5e3b1-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5e3b1-109">Clean up deployment</span></span> 

<span data-ttu-id="5e3b1-110">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5e3b1-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5e3b1-111">Script explanation</span></span>

<span data-ttu-id="5e3b1-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě, Vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-112">This script uses the following commands to create a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="5e3b1-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5e3b1-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5e3b1-114">Command</span></span> | <span data-ttu-id="5e3b1-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5e3b1-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5e3b1-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="5e3b1-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5e3b1-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5e3b1-118">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="5e3b1-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="5e3b1-119">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="5e3b1-120">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="5e3b1-120">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="5e3b1-121">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-121">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="5e3b1-122">Vytvoření sítě lb az</span><span class="sxs-lookup"><span data-stu-id="5e3b1-122">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="5e3b1-123">Vytvoří k nástroji pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-123">Creates an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="5e3b1-124">Vytvoření az sítě lb testu</span><span class="sxs-lookup"><span data-stu-id="5e3b1-124">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="5e3b1-125">Vytvoří sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-125">Creates a load balancer probe.</span></span> <span data-ttu-id="5e3b1-126">Sondu nástroje pro vyrovnávání zatížení se používá k monitorování jednotlivých virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-126">A load balancer probe is used to monitor each VM in the load balancer set.</span></span> <span data-ttu-id="5e3b1-127">Pokud žádné virtuální počítače bude nedostupné, není provoz směruje na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-127">If any VM becomes inaccessible, traffic is not routed to the VM.</span></span> |
| [<span data-ttu-id="5e3b1-128">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="5e3b1-128">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="5e3b1-129">Vytvoří pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-129">Creates a load balancer rule.</span></span> <span data-ttu-id="5e3b1-130">V této ukázce se vytvoří pravidlo pro port 80.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-130">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="5e3b1-131">Jako HTTP přenos dorazí na nástroje pro vyrovnávání zatížení, se směruje na portu 80 mezi virtuálními počítači v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-131">As HTTP traffic arrives at the load balancer, it is routed to port 80 one of the VMs in the load balancer set.</span></span> |
| [<span data-ttu-id="5e3b1-132">Vytvoření az sítě lb ip front-endu-</span><span class="sxs-lookup"><span data-stu-id="5e3b1-132">az network lb frontend-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | <span data-ttu-id="5e3b1-133">Vytvoření IP adresy front-endu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-133">Create a frontend IP address for the Load Balancer.</span></span> |
| [<span data-ttu-id="5e3b1-134">Vytvoření az sítě lb fond adres</span><span class="sxs-lookup"><span data-stu-id="5e3b1-134">az network lb address-pool create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | <span data-ttu-id="5e3b1-135">Vytvoří fond back-end adresy.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-135">Creates a backend address pool.</span></span> |
| [<span data-ttu-id="5e3b1-136">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="5e3b1-136">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="5e3b1-137">Vytvoří virtuální síťové karty a připojí jej virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-137">Creates a virtual network card and attaches it to the virtual network, and subnet.</span></span> |
| [<span data-ttu-id="5e3b1-138">Vytvoření virtuálního počítače az sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-138">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="5e3b1-139">Vytvoří skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-139">Creates an availability set.</span></span> <span data-ttu-id="5e3b1-140">Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena celou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-140">Availability sets ensure application uptime by spreading the virtual machines across physical resources such that if failure occurs, the entire set is not effected.</span></span> |
| [<span data-ttu-id="5e3b1-141">Seskupování síťových az ip-config vytvořit</span><span class="sxs-lookup"><span data-stu-id="5e3b1-141">az network nic ip-config create</span></span>](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | <span data-ttu-id="5e3b1-142">Vytvoří confiuration IP.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-142">Creates an IP confiuration.</span></span> <span data-ttu-id="5e3b1-143">Musí mít funkci Microsoft.Network/AllowMultipleIpConfigurationsPerNic pro vaše předplatné povolený.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-143">You must have the Microsoft.Network/AllowMultipleIpConfigurationsPerNic feature enabled for your subscription.</span></span> <span data-ttu-id="5e3b1-144">Konfiguraci pouze jednoho může být určen jako primární konfiguraci IP adresy pro síťový adaptér, pomocí--příznak změnit na primární.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-144">Only one configuration may be designated as the primary IP configuration per NIC, using the --make-primary flag.</span></span> |
| [<span data-ttu-id="5e3b1-145">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="5e3b1-145">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="5e3b1-146">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-146">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="5e3b1-147">Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-147">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="5e3b1-148">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="5e3b1-148">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5e3b1-149">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e3b1-149">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5e3b1-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e3b1-150">Next steps</span></span>

<span data-ttu-id="5e3b1-151">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5e3b1-151">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5e3b1-152">Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [přehled sítě Azure dokumentaci](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e3b1-152">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
