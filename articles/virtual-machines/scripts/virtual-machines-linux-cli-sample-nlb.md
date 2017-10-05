---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí vyrovnávání zatížení sítě | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí vyrovnávání zatížení sítě"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a0052605da9f0023d11cc9253d8aecfb1d452e1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-highly-available-vm"></a><span data-ttu-id="69dce-103">Vytvoření virtuálního počítače s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="69dce-103">Create a highly available VM</span></span>

<span data-ttu-id="69dce-104">Tento ukázkový skript vytvoří vše potřebné ke spuštění několika Ubuntu virtuální počítače nakonfigurované v s vysokou dostupností a načíst vyrovnáváním konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="69dce-104">This script sample creates everything needed to run several Ubuntu virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="69dce-105">Po spuštění skriptu, budete mít tři virtuální počítače připojené k Azure skupiny dostupnosti a přístupné prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="69dce-105">After running the script, you will have three virtual machines, joined to an Azure Availability Set, and accessible through an Azure Load Balancer.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="69dce-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="69dce-106">Sample script</span></span>

<span data-ttu-id="69dce-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="69dce-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="69dce-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="69dce-108">Clean up deployment</span></span> 

<span data-ttu-id="69dce-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="69dce-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="69dce-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="69dce-110">Script explanation</span></span>

<span data-ttu-id="69dce-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="69dce-111">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="69dce-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="69dce-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="69dce-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="69dce-113">Command</span></span> | <span data-ttu-id="69dce-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="69dce-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="69dce-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="69dce-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="69dce-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="69dce-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="69dce-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="69dce-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="69dce-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="69dce-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="69dce-119">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="69dce-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="69dce-120">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="69dce-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="69dce-121">Vytvoření sítě lb az</span><span class="sxs-lookup"><span data-stu-id="69dce-121">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="69dce-122">Vytvoří k síť Azure pro vyrovnávání zatížení (sítě NLB).</span><span class="sxs-lookup"><span data-stu-id="69dce-122">Creates an Azure Network Load Balancer (NLB).</span></span> |
| [<span data-ttu-id="69dce-123">Vytvoření az sítě lb testu</span><span class="sxs-lookup"><span data-stu-id="69dce-123">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="69dce-124">Vytvoří kontrolu Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="69dce-124">Creates an NLB probe.</span></span> <span data-ttu-id="69dce-125">Vyrovnávání zatížení sítě testu se používá k monitorování jednotlivých virtuálních počítačů v sadě Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="69dce-125">An NLB probe is used to monitor each VM in the NLB set.</span></span> <span data-ttu-id="69dce-126">Pokud žádné virtuální počítače bude nedostupné, není provoz směruje na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="69dce-126">If any VM becomes inaccessible, traffic is not routed to the VM.</span></span> |
| [<span data-ttu-id="69dce-127">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="69dce-127">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="69dce-128">Vytvoří pravidlo Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="69dce-128">Creates an NLB rule.</span></span> <span data-ttu-id="69dce-129">V této ukázce se vytvoří pravidlo pro port 80.</span><span class="sxs-lookup"><span data-stu-id="69dce-129">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="69dce-130">Jako HTTP přenos dorazí na Vyrovnávání zatížení sítě, se směruje na portu 80 mezi virtuálními počítači v sadě Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="69dce-130">As HTTP traffic arrives at the NLB, it is routed to port 80 one of the VMs in the NLB set.</span></span> |
| [<span data-ttu-id="69dce-131">Vytvoření az sítě lb příchozí--pravidlo nat</span><span class="sxs-lookup"><span data-stu-id="69dce-131">az network lb inbound-nat-rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | <span data-ttu-id="69dce-132">Vytvoří pravidlo Vyrovnávání zatížení sítě překlad adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="69dce-132">Creates an NLB Network Address Translation (NAT) rule.</span></span>  <span data-ttu-id="69dce-133">Pravidla NAT namapovat port služby NLB port na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="69dce-133">NAT rules map a port of the NLB to a port on a VM.</span></span> <span data-ttu-id="69dce-134">V této ukázce se vytvoří pravidlo NAT pro provoz SSH pro každý virtuální počítač v sadě Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="69dce-134">In this sample, a NAT rule is created for SSH traffic to each VM in the NLB set.</span></span>  |
| [<span data-ttu-id="69dce-135">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="69dce-135">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="69dce-136">Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi Internetu a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="69dce-136">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="69dce-137">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="69dce-137">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="69dce-138">Vytvoří pravidlo NSG chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="69dce-138">Creates an NSG rule to allow inbound traffic.</span></span> <span data-ttu-id="69dce-139">V této ukázce je otevřen port 22 pro SSH provoz.</span><span class="sxs-lookup"><span data-stu-id="69dce-139">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="69dce-140">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="69dce-140">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="69dce-141">Vytvoří virtuální síťové karty a připojí jej k virtuální síti, podsíti a NSG.</span><span class="sxs-lookup"><span data-stu-id="69dce-141">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="69dce-142">Vytvoření virtuálního počítače az sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="69dce-142">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="69dce-143">Vytvoří skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="69dce-143">Creates an availability set.</span></span> <span data-ttu-id="69dce-144">Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena celou sadu.</span><span class="sxs-lookup"><span data-stu-id="69dce-144">Availability sets ensure application uptime by spreading the virtual machines across physical resources such that if failure occurs, the entire set is not effected.</span></span> |
| [<span data-ttu-id="69dce-145">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="69dce-145">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="69dce-146">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="69dce-146">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="69dce-147">Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.</span><span class="sxs-lookup"><span data-stu-id="69dce-147">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="69dce-148">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="69dce-148">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="69dce-149">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="69dce-149">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="69dce-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69dce-150">Next steps</span></span>

<span data-ttu-id="69dce-151">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="69dce-151">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="69dce-152">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="69dce-152">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
