---
title: "Vytvoření kompletní prostředí Linux pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě, všechny od základů pomocí Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 201ccd523e49d638ace50fbc0ffdceb705b35473
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-10"></a><span data-ttu-id="45513-103">Vytvoření kompletní prostředí Linux pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45513-103">Create a complete Linux environment with the Azure CLI 1.0</span></span>
<span data-ttu-id="45513-104">V tomto článku jsme sestavení jednoduchá síť se nástroj pro vyrovnávání zatížení a dvojice virtuálních počítačů, které jsou užitečné pro vývoj a jednoduchý computing.</span><span class="sxs-lookup"><span data-stu-id="45513-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="45513-105">Provede jsme proces příkazu command, dokud nebudete mít dvě funkční a zabezpečené virtuální počítače s Linuxem do kterých můžete připojit z kdekoli v síti Internet.</span><span class="sxs-lookup"><span data-stu-id="45513-105">We walk through the process command by command, until you have two working, secure Linux VMs to which you can connect from anywhere on the Internet.</span></span> <span data-ttu-id="45513-106">Potom můžete přesunout složitější sítě a prostředí.</span><span class="sxs-lookup"><span data-stu-id="45513-106">Then you can move on to more complex networks and environments.</span></span>

<span data-ttu-id="45513-107">Na cestě můžete další informace o hierarchii závislostí modelu nasazení Resource Manager nabízí, a o tom, kolik ho zapněte poskytuje.</span><span class="sxs-lookup"><span data-stu-id="45513-107">Along the way, you learn about the dependency hierarchy that the Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="45513-108">Jakmile uvidíte, jak se sestaví systém, můžete znovu sestavit je mnohem rychlejší pomocí [šablon Azure Resource Manageru](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-108">After you see how the system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="45513-109">Kromě toho po zjistíte, jak jsou navzájem propojené části vašeho prostředí, vytváření šablon pro automatizaci je jednodušší.</span><span class="sxs-lookup"><span data-stu-id="45513-109">Also, after you learn how the parts of your environment fit together, creating templates to automate them becomes easier.</span></span>

<span data-ttu-id="45513-110">Prostředí obsahuje:</span><span class="sxs-lookup"><span data-stu-id="45513-110">The environment contains:</span></span>

* <span data-ttu-id="45513-111">Dva virtuální počítače uvnitř skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="45513-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="45513-112">Vyrovnávání zatížení s pravidlem Vyrovnávání zatížení na portu 80.</span><span class="sxs-lookup"><span data-stu-id="45513-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="45513-113">Pravidla skupiny (NSG) zabezpečení sítě k ochraně virtuálního počítače z nevyžádaný provoz.</span><span class="sxs-lookup"><span data-stu-id="45513-113">Network security group (NSG) rules to protect your VM from unwanted traffic.</span></span>

<span data-ttu-id="45513-114">K vytvoření tohoto vlastního prostředí, je třeba nejnovější [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) v režimu Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="45513-114">To create this custom environment, you need the latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="45513-115">Musíte taky analýza nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="45513-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="45513-116">Tento příklad používá [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="45513-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="45513-117">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="45513-117">CLI versions to complete the task</span></span>
<span data-ttu-id="45513-118">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="45513-118">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="45513-119">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="45513-119">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="45513-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="45513-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="45513-121">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="45513-121">Quick commands</span></span>
<span data-ttu-id="45513-122">Pokud potřebujete rychle provedení úlohy, následující část podrobně popisuje základní příkazy k nahrání virtuálního počítače do Azure.</span><span class="sxs-lookup"><span data-stu-id="45513-122">If you need to quickly accomplish the task, the following section details the base commands to upload a VM to Azure.</span></span> <span data-ttu-id="45513-123">Podrobnější informace a kontext pro každý krok naleznete ve zbývající části dokumentu, od [zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="45513-123">More detailed information and context for each step can be found in the rest of the document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="45513-124">Ujistěte se, že máte [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="45513-124">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="45513-125">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="45513-125">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="45513-126">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="45513-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="45513-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="45513-127">Create the resource group.</span></span> <span data-ttu-id="45513-128">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westeurope` umístění:</span><span class="sxs-lookup"><span data-stu-id="45513-128">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="45513-129">Skupina prostředků ověřte pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-129">Verify the resource group by using the JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="45513-130">Vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="45513-130">Create the storage account.</span></span> <span data-ttu-id="45513-131">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="45513-131">The following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="45513-132">(Název účtu úložiště musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="45513-132">(The storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="45513-133">Ověřte účet úložiště pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-133">Verify the storage account by using the JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="45513-134">Vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="45513-134">Create the virtual network.</span></span> <span data-ttu-id="45513-135">Následující příklad vytvoří virtuální síť s názvem `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="45513-135">The following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="45513-136">Vytvoření podsítě.</span><span class="sxs-lookup"><span data-stu-id="45513-136">Create a subnet.</span></span> <span data-ttu-id="45513-137">Následující příklad vytvoří podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="45513-137">The following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="45513-138">Ověřte virtuální síť a podsíť pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-138">Verify the virtual network and subnet by using the JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="45513-139">Vytvořte veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="45513-139">Create a public IP.</span></span> <span data-ttu-id="45513-140">Následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="45513-140">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="45513-141">(Název DNS musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="45513-141">(The DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="45513-142">Vytvořte nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-142">Create the load balancer.</span></span> <span data-ttu-id="45513-143">Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="45513-143">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="45513-144">Vytvořte fond IP front-endu nástroje pro vyrovnávání zatížení a přidružte veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="45513-144">Create a front-end IP pool for the load balancer, and associate the public IP.</span></span> <span data-ttu-id="45513-145">Následující příklad vytvoří front-end IP fond s názvem `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="45513-145">The following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="45513-146">Vytvořte fond back-end IP nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-146">Create the back-end IP pool for the load balancer.</span></span> <span data-ttu-id="45513-147">Následující příklad vytvoří fond back-end IP s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="45513-147">The following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="45513-148">Vytvoření příchozích síťových SSH pravidla překladu adres nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-148">Create SSH inbound network address translation (NAT) rules for the load balancer.</span></span> <span data-ttu-id="45513-149">Následující příklad vytvoří dvě pravidla nástroje pro vyrovnávání zatížení, `myLoadBalancerRuleSSH1` a `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="45513-149">The following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="45513-150">Vytvoření webu příchozí pravidla NAT nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-150">Create the web inbound NAT rules for the load balancer.</span></span> <span data-ttu-id="45513-151">Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="45513-151">The following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="45513-152">Vytvořte test stavu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-152">Create the load balancer health probe.</span></span> <span data-ttu-id="45513-153">Následující příklad vytvoří sondou TCP s názvem `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="45513-153">The following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="45513-154">Ověřte nástroj pro vyrovnávání zatížení, fondy IP adres a pravidla NAT pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-154">Verify the load balancer, IP pools, and NAT rules by using the JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="45513-155">Vytvoření první síťové karty (NIC).</span><span class="sxs-lookup"><span data-stu-id="45513-155">Create the first network interface card (NIC).</span></span> <span data-ttu-id="45513-156">Nahraďte `#####-###-###` oddíly s vlastními ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="45513-156">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="45513-157">Vaše předplatné ID je uvedeno ve výstupu **jq** při kontrole prostředků, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="45513-157">Your subscription ID is noted in the output of **jq** when you examine the resources you are creating.</span></span> <span data-ttu-id="45513-158">Můžete také zobrazit svoje ID předplatného s `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="45513-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="45513-159">Následující příklad vytvoří síťový adaptér s názvem `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="45513-159">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="45513-160">Vytvořte druhý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="45513-160">Create the second NIC.</span></span> <span data-ttu-id="45513-161">Následující příklad vytvoří síťový adaptér s názvem `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="45513-161">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="45513-162">Dva síťové adaptéry ověřte pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-162">Verify the two NICs by using the JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="45513-163">Vytvořte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="45513-163">Create the network security group.</span></span> <span data-ttu-id="45513-164">Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="45513-164">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="45513-165">Přidejte dva příchozích pravidel pro skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="45513-165">Add two inbound rules for the network security group.</span></span> <span data-ttu-id="45513-166">Následující příklad vytvoří dvě pravidla `myNetworkSecurityGroupRuleSSH` a `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="45513-166">The following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="45513-167">Skupina zabezpečení sítě a ověřte příchozí pravidla pomocí analyzátoru JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-167">Verify the network security group and inbound rules by using the JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="45513-168">Vytvořit skupinu zabezpečení sítě vazbu na dva síťové adaptéry:</span><span class="sxs-lookup"><span data-stu-id="45513-168">Bind the network security group to the two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="45513-169">Vytvořte sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="45513-169">Create the availability set.</span></span> <span data-ttu-id="45513-170">Následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="45513-170">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="45513-171">Vytvoření prvního virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="45513-171">Create the first Linux VM.</span></span> <span data-ttu-id="45513-172">Následující příklad vytvoří virtuální počítač s názvem `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="45513-172">The following example creates a VM named `myVM1`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="45513-173">Vytvoření druhého virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="45513-173">Create the second Linux VM.</span></span> <span data-ttu-id="45513-174">Následující příklad vytvoří virtuální počítač s názvem `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="45513-174">The following example creates a VM named `myVM2`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="45513-175">Pomocí analyzátoru JSON ověřte, zda vše, co byla vytvořena:</span><span class="sxs-lookup"><span data-stu-id="45513-175">Use the JSON parser to verify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="45513-176">Exportujte do šablony rychle znovu vytvořit nové instance nové prostředí:</span><span class="sxs-lookup"><span data-stu-id="45513-176">Export your new environment to a template to quickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="45513-177">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="45513-177">Detailed walkthrough</span></span>
<span data-ttu-id="45513-178">Podrobné kroky, které následují popisují, co každý příkaz probíhá při sestavování na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="45513-178">The detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="45513-179">Tyto koncepty jsou užitečné, když vytváříte vlastní vlastní prostředí pro vývoj nebo produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="45513-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="45513-180">Ujistěte se, že máte [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="45513-180">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="45513-181">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="45513-181">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="45513-182">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="45513-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="45513-183">Vytvoření skupiny prostředků a vyberte umístění nasazení</span><span class="sxs-lookup"><span data-stu-id="45513-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="45513-184">Skupiny prostředků Azure jsou logické nasazení entit, které obsahují informace o konfiguraci a metadata povolení logické správy nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="45513-184">Azure resource groups are logical deployment entities that contain configuration information and metadata to enable the logical management of resource deployments.</span></span> <span data-ttu-id="45513-185">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westeurope` umístění:</span><span class="sxs-lookup"><span data-stu-id="45513-185">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="45513-186">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-186">Output:</span></span>

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a><span data-ttu-id="45513-187">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="45513-187">Create a storage account</span></span>
<span data-ttu-id="45513-188">Budete potřebovat účty úložiště pro disky virtuálního počítače a pro jakýchkoli dalších datových disků, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="45513-188">You need storage accounts for your VM disks and for any additional data disks that you want to add.</span></span> <span data-ttu-id="45513-189">Můžete vytvořit účty úložiště téměř okamžitě po vytvoření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="45513-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="45513-190">Tady používáme `azure storage account create` příkazu předávání umístění účtu, skupinu prostředků, které řídí a typ úložiště s podporou chcete.</span><span class="sxs-lookup"><span data-stu-id="45513-190">Here we use the `azure storage account create` command, passing the location of the account, the resource group that controls it, and the type of storage support you want.</span></span> <span data-ttu-id="45513-191">Následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="45513-191">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="45513-192">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="45513-193">Prozkoumat naše skupinu prostředků s použitím `azure group show` příkaz, použijeme [jq](https://stedolan.github.io/jq/) nástroj spolu s `--json` možnost příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="45513-193">To examine our resource group by using the `azure group show` command, let's use the [jq](https://stedolan.github.io/jq/) tool along with the `--json` Azure CLI option.</span></span> <span data-ttu-id="45513-194">(Můžete použít **jsawk** nebo všechny knihovny jazyka dáváte přednost analyzovat ve formátu JSON.)</span><span class="sxs-lookup"><span data-stu-id="45513-194">(You can use **jsawk** or any language library you prefer to parse the JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="45513-195">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-195">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="45513-196">K prozkoumání účet úložiště pomocí rozhraní příkazového řádku, musíte nejdřív nastavit názvy účtů a klíčů.</span><span class="sxs-lookup"><span data-stu-id="45513-196">To investigate the storage account by using the CLI, you first need to set the account names and keys.</span></span> <span data-ttu-id="45513-197">Nahraďte název, který zvolíte, název účtu úložiště v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="45513-197">Replace the name of the storage account in the following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="45513-198">Potom můžete zobrazit informace z vašeho úložiště snadno:</span><span class="sxs-lookup"><span data-stu-id="45513-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="45513-199">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="45513-200">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="45513-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="45513-201">Potom budete muset vytvořit virtuální síť spuštěná ve službě Azure a podsíť, ve kterém můžete vytvořit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-201">Next you're going to need to create a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="45513-202">Následující příklad vytvoří virtuální síť s názvem `myVnet` s `192.168.0.0/16` předpona adresy:</span><span class="sxs-lookup"><span data-stu-id="45513-202">The following example creates a virtual network named `myVnet` with the `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="45513-203">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-203">Output:</span></span>

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

<span data-ttu-id="45513-204">Znovu, budeme používat možnost--json `azure group show` a `jq` zobrazíte jak vytváříme naše prostředky.</span><span class="sxs-lookup"><span data-stu-id="45513-204">Again, let's use the --json option of `azure group show` and `jq` to see how we're building our resources.</span></span> <span data-ttu-id="45513-205">Nyní je k dispozici `storageAccounts` prostředků a `virtualNetworks` prostředků.</span><span class="sxs-lookup"><span data-stu-id="45513-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="45513-206">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-206">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="45513-207">Nyní vytvoříme podsíť v `myVnet` virtuální sítě, do které jsou nasazené virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-207">Now let's create a subnet in the `myVnet` virtual network into which the VMs are deployed.</span></span> <span data-ttu-id="45513-208">Používáme `azure network vnet subnet create` příkazu, spolu s prostředky, které jste už vytvořili: `myResourceGroup` skupinu prostředků a `myVnet` virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="45513-208">We use the `azure network vnet subnet create` command, along with the resources we've already created: the `myResourceGroup` resource group and the `myVnet` virtual network.</span></span> <span data-ttu-id="45513-209">V následujícím příkladu přidáme podsíť s názvem `mySubnet` s předponou adresy podsítě z `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="45513-209">In the following example, we add the subnet named `mySubnet` with the subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="45513-210">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="45513-211">Protože podsíť je logicky ve virtuální síti, podíváme pro informace o podsíti pomocí příkazu mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="45513-211">Because the subnet is logically inside the virtual network, we look for the subnet information with a slightly different command.</span></span> <span data-ttu-id="45513-212">Příkaz používáme je `azure network vnet show`, ale budeme pokračovat, vyhledejte ve výstupu JSON pomocí `jq`.</span><span class="sxs-lookup"><span data-stu-id="45513-212">The command we use is `azure network vnet show`, but we continue to examine the JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="45513-213">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-213">Output:</span></span>

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a><span data-ttu-id="45513-214">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="45513-214">Create a public IP address</span></span>
<span data-ttu-id="45513-215">Nyní vytvoříme veřejnou IP adresu (PIP), který jsme přiřadit nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-215">Now let's create the public IP address (PIP) that we assign to your load balancer.</span></span> <span data-ttu-id="45513-216">Umožňuje připojení k virtuální počítače z Internetu pomocí `azure network public-ip create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="45513-216">It enables you to connect to your VMs from the Internet by using the `azure network public-ip create` command.</span></span> <span data-ttu-id="45513-217">Protože výchozí adresa je dynamická, můžeme vytvořit záznam DNS s názvem v **cloudapp.azure.com** domény pomocí `--domain-name-label` možnost.</span><span class="sxs-lookup"><span data-stu-id="45513-217">Because the default address is dynamic, we create a named DNS entry in the **cloudapp.azure.com** domain by using the `--domain-name-label` option.</span></span> <span data-ttu-id="45513-218">Následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="45513-218">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="45513-219">Vzhledem k tomu, že název DNS musí být jedinečný, zadáte svůj vlastní jedinečný název DNS:</span><span class="sxs-lookup"><span data-stu-id="45513-219">Because the DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="45513-220">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

<span data-ttu-id="45513-221">Veřejná IP adresa je také prostředek nejvyšší úrovně, zobrazí se s `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="45513-221">The public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="45513-222">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-222">Output:</span></span>

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

<span data-ttu-id="45513-223">Můžete prozkoumat další podrobnosti prostředků, včetně plně kvalifikovaný název domény (FQDN) subdomény, pomocí kompletní `azure network public-ip show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="45513-223">You can investigate more resource details, including the fully qualified domain name (FQDN) of the subdomain, by using the complete `azure network public-ip show` command.</span></span> <span data-ttu-id="45513-224">Logicky byl přidělen prostředek veřejné IP adresy, ale ještě nebyly přiřazeny konkrétní adresu.</span><span class="sxs-lookup"><span data-stu-id="45513-224">The public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="45513-225">Pokud chcete získat IP adresu, budete potřebovat pro vyrovnávání zatížení, které jste dosud nevytvořili.</span><span class="sxs-lookup"><span data-stu-id="45513-225">To obtain an IP address, you're going to need a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="45513-226">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-226">Output:</span></span>

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="45513-227">Vytvořit nástroj pro vyrovnávání zatížení a fondy IP adres</span><span class="sxs-lookup"><span data-stu-id="45513-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="45513-228">Když vytvoříte Vyrovnávání zatížení, umožňuje distribuci přenosů mezi několika virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="45513-228">When you create a load balancer, it enables you to distribute traffic across multiple VMs.</span></span> <span data-ttu-id="45513-229">Také poskytuje redundance, aby vaše aplikace s spuštění několika virtuálních počítačů, které reagují na požadavky uživatele v případě údržby nebo velkým zatížením.</span><span class="sxs-lookup"><span data-stu-id="45513-229">It also provides redundancy to your application by running multiple VMs that respond to user requests in the event of maintenance or heavy loads.</span></span> <span data-ttu-id="45513-230">Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="45513-230">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="45513-231">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="45513-232">Naše nástroj pro vyrovnávání zatížení je docela prázdná, takže vytvoříme některé fondy IP adres.</span><span class="sxs-lookup"><span data-stu-id="45513-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="45513-233">Chceme vytvořit dva fondy IP adres pro naše nástroj pro vyrovnávání zatížení, jeden pro front-endu a jeden pro back-end.</span><span class="sxs-lookup"><span data-stu-id="45513-233">We want to create two IP pools for our load balancer, one for the front end and one for the back end.</span></span> <span data-ttu-id="45513-234">Front-endu fond IP adres je veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="45513-234">The front-end IP pool is publicly visible.</span></span> <span data-ttu-id="45513-235">Je také umístění, do které budeme přiřadit PIP, kterou jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="45513-235">It's also the location to which we assign the PIP that we created earlier.</span></span> <span data-ttu-id="45513-236">Potom používáme fond back-end jako umístění pro naše virtuální počítače pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="45513-236">Then we use the back-end pool as a location for our VMs to connect to.</span></span> <span data-ttu-id="45513-237">Tímto způsobem můžete provoz procházet skrz nástroje pro vyrovnávání zatížení pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-237">That way, the traffic can flow through the load balancer to the VMs.</span></span>

<span data-ttu-id="45513-238">Nejdříve vytvoříme naše front-end fond IP adres.</span><span class="sxs-lookup"><span data-stu-id="45513-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="45513-239">Následující příklad vytvoří front-end fond s názvem `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="45513-239">The following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="45513-240">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="45513-241">Všimněte si, jak jsme použili `--public-ip-name` přepínač předávat `myPublicIP` kterou jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="45513-241">Note how we used the `--public-ip-name` switch to pass in the `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="45513-242">Přiřazení veřejnou IP adresu nástroji pro vyrovnávání zatížení, můžete dosáhnout na virtuální počítače na Internetu.</span><span class="sxs-lookup"><span data-stu-id="45513-242">Assigning the public IP address to the load balancer allows you to reach your VMs across the Internet.</span></span>

<span data-ttu-id="45513-243">V dalším kroku vytvoříme naše druhý fond IP této doby provozu našich back-end.</span><span class="sxs-lookup"><span data-stu-id="45513-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="45513-244">Následující příklad vytvoří fond back-end s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="45513-244">The following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="45513-245">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="45513-246">Vidíme úspěšnost naše nástroj pro vyrovnávání zatížení tak, že vyhledá s `azure network lb show` a prozkoumání výstup JSON:</span><span class="sxs-lookup"><span data-stu-id="45513-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining the JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="45513-247">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-247">Output:</span></span>

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="45513-248">Vytvoření pravidla NAT nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="45513-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="45513-249">Získat provoz v našem nástroj pro vyrovnávání zatížení, je potřeba vytvořit síťová adresa pravidla překlad (NAT), která zadejte příchozí nebo odchozí akce.</span><span class="sxs-lookup"><span data-stu-id="45513-249">To get traffic flowing through our load balancer, we need to create network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="45513-250">Můžete zadat protokol používat a pak mapování externí porty na interní porty podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="45513-250">You can specify the protocol to use, then map external ports to internal ports as desired.</span></span> <span data-ttu-id="45513-251">Pro naše prostředí vytvoříme některá pravidla, které umožňují SSH pomocí našich nástroj pro vyrovnávání zatížení pro naše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-251">For our environment, let's create some rules that allow SSH through our load balancer to our VMs.</span></span> <span data-ttu-id="45513-252">Porty TCP 4222 a 4223 jsme nastavení pro přesměrování na TCP port 22 na našem virtuálních počítačích (které vytvoříme později).</span><span class="sxs-lookup"><span data-stu-id="45513-252">We set up TCP ports 4222 and 4223 to direct to TCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="45513-253">Následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH1` mapovat TCP port 4222 port 22:</span><span class="sxs-lookup"><span data-stu-id="45513-253">The following example creates a rule named `myLoadBalancerRuleSSH1` to map TCP port 4222 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="45513-254">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

<span data-ttu-id="45513-255">Opakujte postup pro druhý pravidla NAT pro SSH.</span><span class="sxs-lookup"><span data-stu-id="45513-255">Repeat the procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="45513-256">Následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH2` mapovat TCP port 4223 port 22:</span><span class="sxs-lookup"><span data-stu-id="45513-256">The following example creates a rule named `myLoadBalancerRuleSSH2` to map TCP port 4223 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="45513-257">Můžeme také pokračujte a vytvořte tak pravidlo NAT pro port TCP 80 webových přenosů, zapojování pravidlo až naše fondy IP adres.</span><span class="sxs-lookup"><span data-stu-id="45513-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking the rule up to our IP pools.</span></span> <span data-ttu-id="45513-258">Pokud jsme spojit pravidlo k fondu IP adres, místo zapojování pravidlo naše virtuálních počítačů jednotlivě, jsme můžete přidat nebo odebrat z fondu IP adres virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-258">If we hook up the rule to an IP pool, instead of hooking up the rule to our VMs individually, we can add or remove VMs from the IP pool.</span></span> <span data-ttu-id="45513-259">Nástroje pro vyrovnávání zatížení automaticky upraví tok provozu.</span><span class="sxs-lookup"><span data-stu-id="45513-259">The load balancer automatically adjusts the flow of traffic.</span></span> <span data-ttu-id="45513-260">Následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleWeb` k mapování TCP port 80 na portu 80:</span><span class="sxs-lookup"><span data-stu-id="45513-260">The following example creates a rule named `myLoadBalancerRuleWeb` to map TCP port 80 to port 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="45513-261">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="45513-262">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-262">Create a load balancer health probe</span></span>
<span data-ttu-id="45513-263">Stavu testů pravidelně kontroluje virtuálních počítačů, které jsou za naše pro vyrovnávání zatížení a ujistěte se, že jsou operační a reagovat na požadavky, jak jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="45513-263">A health probe periodically checks on the VMs that are behind our load balancer to make sure they're operating and responding to requests as defined.</span></span> <span data-ttu-id="45513-264">V opačném případě se odebere z operace zajistit, že uživatelé nejsou směrovat na ně.</span><span class="sxs-lookup"><span data-stu-id="45513-264">If not, they're removed from operation to ensure that users aren't being directed to them.</span></span> <span data-ttu-id="45513-265">Můžete definovat vlastní kontroly pro kontrolu stavu, spolu s intervalech a hodnoty časového limitu.</span><span class="sxs-lookup"><span data-stu-id="45513-265">You can define custom checks for the health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="45513-266">Další informace o sondy stavu najdete v tématu [nástroj pro vyrovnávání zatížení sondy](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="45513-267">Následující příklad vytvoří TCP stavu zjištěný pojmenované `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="45513-267">The following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="45513-268">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="45513-269">Zde jsme zadali interval 15 sekund pro naše kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="45513-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="45513-270">Jsme můžete neproběhly maximálně čtyři sondy (jedné minuty) před nástroje pro vyrovnávání zatížení domnívá, že hostitel již není funkční.</span><span class="sxs-lookup"><span data-stu-id="45513-270">We can miss a maximum of four probes (one minute) before the load balancer considers that the host is no longer functioning.</span></span>

## <a name="verify-the-load-balancer"></a><span data-ttu-id="45513-271">Ověřte, nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="45513-271">Verify the load balancer</span></span>
<span data-ttu-id="45513-272">Nyní se provádí konfigurace služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-272">Now the load balancer configuration is done.</span></span> <span data-ttu-id="45513-273">Tady jsou kroky, které jste pořídili:</span><span class="sxs-lookup"><span data-stu-id="45513-273">Here are the steps you took:</span></span>

1. <span data-ttu-id="45513-274">Vytvořili jste nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-274">You created a load balancer.</span></span>
2. <span data-ttu-id="45513-275">Vytvořit fond IP front-endu a přiřazenou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="45513-275">You created a front-end IP pool and assigned a public IP to it.</span></span>
3. <span data-ttu-id="45513-276">Můžete vytvořit fond back-end IP, které se mohou připojit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="45513-277">Můžete vytvořit pravidla NAT, které umožňují SSH pro virtuální počítače pro správu, společně s pravidlo, které umožní port TCP 80 pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45513-277">You created NAT rules that allow SSH to the VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="45513-278">Jste přidali test stavu na pravidelně kontrolovat, virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-278">You added a health probe to periodically check the VMs.</span></span> <span data-ttu-id="45513-279">Tento test stavu zajistí, že si uživatelé pokusí o přístup virtuální počítač, který je už funguje nebo poskytování obsahu.</span><span class="sxs-lookup"><span data-stu-id="45513-279">This health probe ensures that users don't try to access a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="45513-280">Pojďme si shrnout, co nástroj pro vyrovnávání zatížení vypadá jako nyní:</span><span class="sxs-lookup"><span data-stu-id="45513-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="45513-281">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-281">Output:</span></span>

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a><span data-ttu-id="45513-282">Vytvořte síťovou kartu k použití s virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="45513-282">Create an NIC to use with the Linux VM</span></span>
<span data-ttu-id="45513-283">Síťové adaptéry jsou prostřednictvím kódu programu k dispozici, protože pravidla můžete použít pro jejich použití.</span><span class="sxs-lookup"><span data-stu-id="45513-283">NICs are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="45513-284">Také můžete mít více než jednu.</span><span class="sxs-lookup"><span data-stu-id="45513-284">You can also have more than one.</span></span> <span data-ttu-id="45513-285">V následujícím `azure network nic create` příkaz spojit síťový adaptér a fond zatížení back-end IP a přidružte ji k pravidlo NAT pro povolení komunikace SSH.</span><span class="sxs-lookup"><span data-stu-id="45513-285">In the following `azure network nic create` command, you hook up the NIC to the load back-end IP pool and associate it with the NAT rule to permit SSH traffic.</span></span>

<span data-ttu-id="45513-286">Nahraďte `#####-###-###` oddíly s vlastními ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="45513-286">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="45513-287">Vaše předplatné ID je uvedeno ve výstupu `jq` při kontrole prostředků, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="45513-287">Your subscription ID is noted in the output of `jq` when you examine the resources you are creating.</span></span> <span data-ttu-id="45513-288">Můžete také zobrazit svoje ID předplatného s `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="45513-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="45513-289">Následující příklad vytvoří síťový adaptér s názvem `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="45513-289">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="45513-290">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

<span data-ttu-id="45513-291">Prověřením přímo prostředek můžete zobrazit podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="45513-291">You can see the details by examining the resource directly.</span></span> <span data-ttu-id="45513-292">Zkontrolujte prostředek pomocí `azure network nic show` příkaz:</span><span class="sxs-lookup"><span data-stu-id="45513-292">You examine the resource by using the `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="45513-293">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-293">Output:</span></span>

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

<span data-ttu-id="45513-294">Nyní vytvoříme druhý síťový adaptér, zapojování v do našich fondu back-end IP znovu.</span><span class="sxs-lookup"><span data-stu-id="45513-294">Now we create the second NIC, hooking in to our back-end IP pool again.</span></span> <span data-ttu-id="45513-295">Toto pravidlo NAT čas druhý povolí provoz SSH.</span><span class="sxs-lookup"><span data-stu-id="45513-295">This time the second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="45513-296">Následující příklad vytvoří síťový adaptér s názvem `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="45513-296">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="45513-297">Vytvořte skupinu zabezpečení sítě a pravidla</span><span class="sxs-lookup"><span data-stu-id="45513-297">Create a network security group and rules</span></span>
<span data-ttu-id="45513-298">Teď vytvoříme skupinu zabezpečení sítě a příchozích pravidel, které řídí přístup na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="45513-298">Now we create a network security group and the inbound rules that govern access to the NIC.</span></span> <span data-ttu-id="45513-299">Skupina zabezpečení sítě je použít pro síťový adaptér nebo podsíť.</span><span class="sxs-lookup"><span data-stu-id="45513-299">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="45513-300">Můžete definovat pravidla pro řízení toku provozu do a z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-300">You define rules to control the flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="45513-301">Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="45513-301">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="45513-302">Umožňuje přidat příchozí pravidlo skupiny nsg umožňuje příchozí připojení na portu 22 (pro podporu SSH).</span><span class="sxs-lookup"><span data-stu-id="45513-302">Let's add the inbound rule for the NSG to allow inbound connections on port 22 (to support SSH).</span></span> <span data-ttu-id="45513-303">Následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleSSH` umožňující TCP na portu 22:</span><span class="sxs-lookup"><span data-stu-id="45513-303">The following example creates a rule named `myNetworkSecurityGroupRuleSSH` to allow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="45513-304">Nyní Pojďme přidat příchozí pravidlo skupiny nsg umožňuje příchozí připojení na portu 80 (pro podporu webový provoz).</span><span class="sxs-lookup"><span data-stu-id="45513-304">Now let's add the inbound rule for the NSG to allow inbound connections on port 80 (to support web traffic).</span></span> <span data-ttu-id="45513-305">Následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleHTTP` umožňující TCP na portu 80:</span><span class="sxs-lookup"><span data-stu-id="45513-305">The following example creates a rule named `myNetworkSecurityGroupRuleHTTP` to allow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="45513-306">Příchozí pravidlo je filtr pro příchozí síťová připojení.</span><span class="sxs-lookup"><span data-stu-id="45513-306">The inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="45513-307">V tomto příkladu jsme vazbu NSG na virtuální počítače virtuální síťový adaptér, což znamená, že všechny žádosti na port 22 předána do síťového adaptéru na našem virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-307">In this example, we bind the NSG to the VMs virtual NIC, which means that any request to port 22 is passed through to the NIC on our VM.</span></span> <span data-ttu-id="45513-308">Toto pravidlo příchozí je o připojení k síti a není o koncový bod, který je co by bylo o v nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="45513-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="45513-309">Otevření portu, musí zůstat `--source-port-range` nastavena na "\*" (výchozí hodnota) pro příjem příchozích požadavků z **všechny** požaduje port.</span><span class="sxs-lookup"><span data-stu-id="45513-309">To open a port, you must leave the `--source-port-range` set to '\*' (the default value) to accept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="45513-310">Porty jsou obvykle dynamická.</span><span class="sxs-lookup"><span data-stu-id="45513-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-to-the-nic"></a><span data-ttu-id="45513-311">Vytvořit vazbu na síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="45513-311">Bind to the NIC</span></span>
<span data-ttu-id="45513-312">Vazbu NSG k síťové karty.</span><span class="sxs-lookup"><span data-stu-id="45513-312">Bind the NSG to the NICs.</span></span> <span data-ttu-id="45513-313">Je potřeba připojit naše síťové adaptéry s naše skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="45513-313">We need to connect our NICs with our network security group.</span></span> <span data-ttu-id="45513-314">Spuštění obou příkazů, spojit i naše síťových karet:</span><span class="sxs-lookup"><span data-stu-id="45513-314">Run both commands, to hook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="45513-315">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="45513-315">Create an availability set</span></span>
<span data-ttu-id="45513-316">Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="45513-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="45513-317">Umožňuje vytvořit sadu dostupnosti pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="45513-318">Následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="45513-318">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="45513-319">Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="45513-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="45513-320">Ve výchozím nastavení jsou oddělené virtuální počítače, které jsou nakonfigurované v rámci vaší skupiny dostupnosti v rámci až tři domény selhání.</span><span class="sxs-lookup"><span data-stu-id="45513-320">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="45513-321">Cílem je, že problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="45513-321">The idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="45513-322">Virtuální počítače Azure automaticky distribuuje mezi doménami selhání, když umístění do nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="45513-322">Azure automatically distributes VMs across the fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="45513-323">Domén upgradu označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="45513-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="45513-324">Pořadí, ve kterém se restartují upgradu domény nemusí být po sobě jdoucích během plánované údržby, ale jenom jeden upgradu po restartu najednou.</span><span class="sxs-lookup"><span data-stu-id="45513-324">The order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="45513-325">Znovu Azure automaticky distribuuje virtuální počítače napříč doménami upgradu, když umístění do stránku dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="45513-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="45513-326">Další informace o [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-326">Read more about [managing the availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-the-linux-vms"></a><span data-ttu-id="45513-327">Vytvořit virtuální počítače Linux</span><span class="sxs-lookup"><span data-stu-id="45513-327">Create the Linux VMs</span></span>
<span data-ttu-id="45513-328">Vytvoření úložiště a síťové prostředky pro podporu přístupné z Internetu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-328">You've created the storage and network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="45513-329">Nyní Pojďme vytvořit ty virtuální počítače a zabezpečit protokolem klíč SSH, který nemá heslo.</span><span class="sxs-lookup"><span data-stu-id="45513-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="45513-330">V tomto případě vytvoříme vytvořit Ubuntu podle nejnovější LTS virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-330">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="45513-331">Nemůžeme najít informace o tomto obrázku pomocí `azure vm image list`, jak je popsáno v [hledání Image virtuálního počítače Azure](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="45513-332">Jsme vybrali bitovou kopii pomocí příkazu `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="45513-332">We selected an image by using the command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="45513-333">V tomto případě používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="45513-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="45513-334">V poli poslední jsme předat `latest` tak, aby v budoucnu nám vždycky získat nejnovější sestavení.</span><span class="sxs-lookup"><span data-stu-id="45513-334">For the last field, we pass `latest` so that in the future we always get the most recent build.</span></span> <span data-ttu-id="45513-335">(Je řetězec používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="45513-335">(The string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="45513-336">Tento další krok je pro každý, kdo již vytvořen ssh veřejného a privátního klíče rsa spárujte na systému Linux nebo Mac. pomocí **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="45513-336">This next step is familiar to anyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="45513-337">Pokud nemáte žádné páry klíčů certifikátů vaší `~/.ssh` adresáře, můžete je vytvořit:</span><span class="sxs-lookup"><span data-stu-id="45513-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="45513-338">Automaticky pomocí `azure vm create --generate-ssh-keys` možnost.</span><span class="sxs-lookup"><span data-stu-id="45513-338">Automatically, by using the `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="45513-339">Ručně pomocí [pokyny pro vytvoření sami](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-339">Manually, by using [the instructions to create them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="45513-340">Alternativně můžete použít `--admin-password` metodu pro ověřování připojení SSH po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-340">Alternatively, you can use the `--admin-password` method to authenticate your SSH connections after the VM is created.</span></span> <span data-ttu-id="45513-341">Tato metoda je obvykle méně bezpečné.</span><span class="sxs-lookup"><span data-stu-id="45513-341">This method is typically less secure.</span></span>

<span data-ttu-id="45513-342">Nemůžeme vytvořit virtuální počítač tak, že převedou všechny naše prostředky a informace o společně s `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="45513-342">We create the VM by bringing all our resources and information together with the `azure vm create` command:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="45513-343">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="45513-344">Připojením k virtuálnímu počítači okamžitě pomocí klíče SSH výchozí.</span><span class="sxs-lookup"><span data-stu-id="45513-344">You can connect to your VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="45513-345">Ujistěte se, zadejte odpovídající port, protože jsme se prošla pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="45513-345">Make sure that you specify the appropriate port since we're passing through the load balancer.</span></span> <span data-ttu-id="45513-346">(Pro naše první virtuální počítač, nastavíme pravidlo NAT předávání port 4222 do našich virtuálního počítače.)</span><span class="sxs-lookup"><span data-stu-id="45513-346">(For our first VM, we set up the NAT rule to forward port 4222 to our VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="45513-347">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-347">Output:</span></span>

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="45513-348">Pokračujte a vytvořte druhý virtuální počítač stejným způsobem:</span><span class="sxs-lookup"><span data-stu-id="45513-348">Go ahead and create your second VM in the same manner:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="45513-349">A teď můžete použít `azure vm show myResourceGroup myVM1` příkaz zjistit, co jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="45513-349">And you can now use the `azure vm show myResourceGroup myVM1` command to examine what you've created.</span></span> <span data-ttu-id="45513-350">V tomto okamžiku používáte Ubuntu virtuálních počítačů za službou Vyrovnávání zatížení v Azure, který můžete se přihlaste pouze s dvojici klíčů SSH (protože hesla jsou zakázané).</span><span class="sxs-lookup"><span data-stu-id="45513-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="45513-351">Můžete nainstalovat nginx nebo httpd, nasazení webové aplikace a sledovat provoz toku prostřednictvím nástroje pro vyrovnávání zatížení do obou virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45513-351">You can install nginx or httpd, deploy a web app, and see the traffic flow through the load balancer to both of the VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="45513-352">Výstup:</span><span class="sxs-lookup"><span data-stu-id="45513-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a><span data-ttu-id="45513-353">Export prostředí jako šablonu</span><span class="sxs-lookup"><span data-stu-id="45513-353">Export the environment as a template</span></span>
<span data-ttu-id="45513-354">Teď, který jste vytvořili na tomto prostředí, jak postupovat, pokud chcete vytvořit další vývoj prostředí se stejnými parametry nebo produkčním prostředí, který odpovídá jeho?</span><span class="sxs-lookup"><span data-stu-id="45513-354">Now that you have built out this environment, what if you want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="45513-355">Správce prostředků používá šablony JSON, které definují všech parametrů pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="45513-355">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="45513-356">Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="45513-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="45513-357">Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávajícího prostředí pro vytvoření šablony JSON pro vás:</span><span class="sxs-lookup"><span data-stu-id="45513-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="45513-358">Tento příkaz vytvoří `myResourceGroup.json` souboru v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="45513-358">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="45513-359">Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků, včetně názvů pro nástroj pro vyrovnávání zatížení, síťová rozhraní nebo virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45513-359">When you create an environment from this template, you are prompted for all the resource names, including the names for the load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="45513-360">Tyto názvy v souboru šablony může vyplnit přidáním `-p` nebo `--includeParameterDefaultValue` parametru `azure group export` příkaz, který byl dříve vidět.</span><span class="sxs-lookup"><span data-stu-id="45513-360">You can populate these names in your template file by adding the `-p` or `--includeParameterDefaultValue` parameter to the `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="45513-361">Upravte svou šablonu JSON zadat názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků.</span><span class="sxs-lookup"><span data-stu-id="45513-361">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="45513-362">K vytvoření prostředí z vaší šablony:</span><span class="sxs-lookup"><span data-stu-id="45513-362">To create an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="45513-363">Můžete chtít číst [Další informace o tom, jak nasadit ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45513-363">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="45513-364">Další informace o tom, jak přírůstkově aktualizovat prostředí, použijte soubor parametrů a přístup k šablony z jedno umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="45513-364">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45513-365">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45513-365">Next steps</span></span>
<span data-ttu-id="45513-366">Nyní jste připraveni začít pracovat s více síťovými součástmi a virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="45513-366">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="45513-367">Tato ukázka prostředí můžete vytváří vaše aplikace pomocí základních komponent zavedená sem.</span><span class="sxs-lookup"><span data-stu-id="45513-367">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
