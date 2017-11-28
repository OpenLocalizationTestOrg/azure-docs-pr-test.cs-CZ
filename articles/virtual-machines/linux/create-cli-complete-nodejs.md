---
title: "aaaCreate dokončení prostředí Linux s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě z hello pozadí pomocí hello Azure CLI 1.0."
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
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a><span data-ttu-id="86135-103">Vytvořte prostředí dokončení Linux s hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="86135-103">Create a complete Linux environment with hello Azure CLI 1.0</span></span>
<span data-ttu-id="86135-104">V tomto článku jsme sestavení jednoduchá síť se nástroj pro vyrovnávání zatížení a dvojice virtuálních počítačů, které jsou užitečné pro vývoj a jednoduchý computing.</span><span class="sxs-lookup"><span data-stu-id="86135-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="86135-105">Jsme provede postupem hello příkazu command, dokud nebudete mít dvě pracovní, zabezpečené toowhich virtuální počítače s Linuxem, se můžete připojit z kdekoliv na Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="86135-105">We walk through hello process command by command, until you have two working, secure Linux VMs toowhich you can connect from anywhere on hello Internet.</span></span> <span data-ttu-id="86135-106">Potom můžete přesunout na toomore složité sítě a prostředí.</span><span class="sxs-lookup"><span data-stu-id="86135-106">Then you can move on toomore complex networks and environments.</span></span>

<span data-ttu-id="86135-107">Společně hello způsob můžete další informace o hierarchii závislostí hello hello modelu nasazení Resource Manager nabízí, a o tom, kolik power poskytuje.</span><span class="sxs-lookup"><span data-stu-id="86135-107">Along hello way, you learn about hello dependency hierarchy that hello Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="86135-108">Jakmile uvidíte, jak se sestaví systém hello, můžete znovu sestavit je mnohem rychlejší pomocí [šablon Azure Resource Manageru](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-108">After you see how hello system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="86135-109">Kromě toho po zjistíte, jak jsou navzájem propojené hello části vašeho prostředí, vytváření šablony tooautomate je jednodušší.</span><span class="sxs-lookup"><span data-stu-id="86135-109">Also, after you learn how hello parts of your environment fit together, creating templates tooautomate them becomes easier.</span></span>

<span data-ttu-id="86135-110">Hello prostředí obsahuje:</span><span class="sxs-lookup"><span data-stu-id="86135-110">hello environment contains:</span></span>

* <span data-ttu-id="86135-111">Dva virtuální počítače uvnitř skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="86135-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="86135-112">Vyrovnávání zatížení s pravidlem Vyrovnávání zatížení na portu 80.</span><span class="sxs-lookup"><span data-stu-id="86135-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="86135-113">Skupina zabezpečení sítě (NSG) pravidla tooprotect virtuálního počítače z nevyžádaný provoz.</span><span class="sxs-lookup"><span data-stu-id="86135-113">Network security group (NSG) rules tooprotect your VM from unwanted traffic.</span></span>

<span data-ttu-id="86135-114">toocreate tento vlastní prostředí, je nutné hello nejnovější [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) v režimu Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="86135-114">toocreate this custom environment, you need hello latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="86135-115">Musíte taky analýza nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="86135-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="86135-116">Tento příklad používá [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="86135-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="86135-117">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="86135-117">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="86135-118">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="86135-118">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="86135-119">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="86135-119">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="86135-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="86135-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="86135-121">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="86135-121">Quick commands</span></span>
<span data-ttu-id="86135-122">Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-122">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="86135-123">Podrobnější informace a kontext pro každý krok lze nalézt v hello zbytek dokumentu hello od [zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="86135-123">More detailed information and context for each step can be found in hello rest of hello document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="86135-124">Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="86135-124">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="86135-125">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="86135-125">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="86135-126">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="86135-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="86135-127">Vytvořte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="86135-127">Create hello resource group.</span></span> <span data-ttu-id="86135-128">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění:</span><span class="sxs-lookup"><span data-stu-id="86135-128">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="86135-129">Skupina prostředků hello ověřte pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-129">Verify hello resource group by using hello JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="86135-130">Vytvořte účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="86135-130">Create hello storage account.</span></span> <span data-ttu-id="86135-131">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="86135-131">hello following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="86135-132">(hello název účtu úložiště musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="86135-132">(hello storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="86135-133">Účet úložiště hello ověřte pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-133">Verify hello storage account by using hello JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="86135-134">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="86135-134">Create hello virtual network.</span></span> <span data-ttu-id="86135-135">Hello následující příklad vytvoří virtuální síť s názvem `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="86135-135">hello following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="86135-136">Vytvoření podsítě.</span><span class="sxs-lookup"><span data-stu-id="86135-136">Create a subnet.</span></span> <span data-ttu-id="86135-137">Hello následující příklad vytvoří podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="86135-137">hello following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="86135-138">Ověřte hello virtuální síť a podsíť pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-138">Verify hello virtual network and subnet by using hello JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="86135-139">Vytvořte veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="86135-139">Create a public IP.</span></span> <span data-ttu-id="86135-140">Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS hello `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="86135-140">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="86135-141">(název DNS hello musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)</span><span class="sxs-lookup"><span data-stu-id="86135-141">(hello DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="86135-142">Vytvořte nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="86135-142">Create hello load balancer.</span></span> <span data-ttu-id="86135-143">Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="86135-143">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="86135-144">Vytvořte fond IP front-endu nástroje pro vyrovnávání zatížení hello a přidružte veřejnou IP adresu hello.</span><span class="sxs-lookup"><span data-stu-id="86135-144">Create a front-end IP pool for hello load balancer, and associate hello public IP.</span></span> <span data-ttu-id="86135-145">Hello následující příklad vytvoří front-end IP fond s názvem `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="86135-145">hello following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="86135-146">Vytvořte fond IP hello back-end pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="86135-146">Create hello back-end IP pool for hello load balancer.</span></span> <span data-ttu-id="86135-147">Hello následující příklad vytvoří fond back-end IP s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="86135-147">hello following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="86135-148">Vytvoření pravidla překladu adres nástroje pro vyrovnávání zatížení hello příchozích síťových SSH.</span><span class="sxs-lookup"><span data-stu-id="86135-148">Create SSH inbound network address translation (NAT) rules for hello load balancer.</span></span> <span data-ttu-id="86135-149">Hello následující příklad vytvoří dvě pravidla nástroje pro vyrovnávání zatížení, `myLoadBalancerRuleSSH1` a `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="86135-149">hello following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="86135-150">Vytvoření webové hello příchozích pravidel NAT pro hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="86135-150">Create hello web inbound NAT rules for hello load balancer.</span></span> <span data-ttu-id="86135-151">Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="86135-151">hello following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="86135-152">Vytvořte test stavu nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="86135-152">Create hello load balancer health probe.</span></span> <span data-ttu-id="86135-153">Hello následující příklad vytvoří sondou TCP s názvem `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="86135-153">hello following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="86135-154">Ověřte hello nástroj pro vyrovnávání zatížení, fondy IP adres a pravidla NAT pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-154">Verify hello load balancer, IP pools, and NAT rules by using hello JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="86135-155">Vytvořte hello první síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="86135-155">Create hello first network interface card (NIC).</span></span> <span data-ttu-id="86135-156">Nahraďte hello `#####-###-###` oddíly s vlastními ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="86135-156">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="86135-157">Vaše předplatné ID je uvedeno v výstup hello **jq** při kontrole hello prostředků, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="86135-157">Your subscription ID is noted in hello output of **jq** when you examine hello resources you are creating.</span></span> <span data-ttu-id="86135-158">Můžete také zobrazit svoje ID předplatného s `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="86135-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="86135-159">Hello následující příklad vytvoří síťový adaptér s názvem `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="86135-159">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="86135-160">Vytvoření hello druhý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="86135-160">Create hello second NIC.</span></span> <span data-ttu-id="86135-161">Hello následující příklad vytvoří síťový adaptér s názvem `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="86135-161">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="86135-162">Ověřte hello dva síťové adaptéry pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-162">Verify hello two NICs by using hello JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="86135-163">Vytvořte skupinu zabezpečení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="86135-163">Create hello network security group.</span></span> <span data-ttu-id="86135-164">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="86135-164">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="86135-165">Přidejte dva příchozích pravidel pro skupinu zabezpečení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="86135-165">Add two inbound rules for hello network security group.</span></span> <span data-ttu-id="86135-166">Hello následující příklad vytvoří dvě pravidla `myNetworkSecurityGroupRuleSSH` a `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="86135-166">hello following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="86135-167">Ověřte hello skupinu zabezpečení sítě a příchozích pravidel pomocí analyzátoru hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-167">Verify hello network security group and inbound rules by using hello JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="86135-168">Vazby zabezpečení sítě hello skupiny toohello dva síťové adaptéry:</span><span class="sxs-lookup"><span data-stu-id="86135-168">Bind hello network security group toohello two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="86135-169">Vytvořte skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="86135-169">Create hello availability set.</span></span> <span data-ttu-id="86135-170">Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="86135-170">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="86135-171">Vytvoření hello první virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="86135-171">Create hello first Linux VM.</span></span> <span data-ttu-id="86135-172">Hello následující příklad vytvoří virtuální počítač s názvem `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="86135-172">hello following example creates a VM named `myVM1`:</span></span>

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

<span data-ttu-id="86135-173">Vytvoření hello druhé virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="86135-173">Create hello second Linux VM.</span></span> <span data-ttu-id="86135-174">Hello následující příklad vytvoří virtuální počítač s názvem `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="86135-174">hello following example creates a VM named `myVM2`:</span></span>

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

<span data-ttu-id="86135-175">Použijte hello JSON analyzátor tooverify, že vše, co byl vytvořený:</span><span class="sxs-lookup"><span data-stu-id="86135-175">Use hello JSON parser tooverify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="86135-176">Exportujte vaší nové prostředí tooa šablony tooquickly znovu vytvořit nové instance služby:</span><span class="sxs-lookup"><span data-stu-id="86135-176">Export your new environment tooa template tooquickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="86135-177">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="86135-177">Detailed walkthrough</span></span>
<span data-ttu-id="86135-178">Hello podrobné kroky, které následují popisují, co každý příkaz probíhá při sestavování na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="86135-178">hello detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="86135-179">Tyto koncepty jsou užitečné, když vytváříte vlastní vlastní prostředí pro vývoj nebo produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="86135-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="86135-180">Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="86135-180">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="86135-181">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="86135-181">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="86135-182">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="86135-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="86135-183">Vytvoření skupiny prostředků a vyberte umístění nasazení</span><span class="sxs-lookup"><span data-stu-id="86135-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="86135-184">Skupiny prostředků Azure jsou logické nasazení entity, které obsahují konfigurační informace metadat tooenable hello logické správu a nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="86135-184">Azure resource groups are logical deployment entities that contain configuration information and metadata tooenable hello logical management of resource deployments.</span></span> <span data-ttu-id="86135-185">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění:</span><span class="sxs-lookup"><span data-stu-id="86135-185">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="86135-186">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-186">Output:</span></span>

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

## <a name="create-a-storage-account"></a><span data-ttu-id="86135-187">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="86135-187">Create a storage account</span></span>
<span data-ttu-id="86135-188">Je třeba účty úložiště pro disky virtuálních počítačů a jakýchkoli dalších datových disků, které chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="86135-188">You need storage accounts for your VM disks and for any additional data disks that you want tooadd.</span></span> <span data-ttu-id="86135-189">Můžete vytvořit účty úložiště téměř okamžitě po vytvoření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="86135-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="86135-190">Tady používáme hello `azure storage account create` příkazu předávání hello umístění skupiny prostředků hello hello účtu, který řídí a typ úložiště s podporou chcete hello.</span><span class="sxs-lookup"><span data-stu-id="86135-190">Here we use hello `azure storage account create` command, passing hello location of hello account, hello resource group that controls it, and hello type of storage support you want.</span></span> <span data-ttu-id="86135-191">Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="86135-191">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="86135-192">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="86135-193">Skupina naše prostředků pomocí hello tooexamine `azure group show` příkaz, použijeme hello [jq](https://stedolan.github.io/jq/) nástroj společně s hello `--json` možnost příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="86135-193">tooexamine our resource group by using hello `azure group show` command, let's use hello [jq](https://stedolan.github.io/jq/) tool along with hello `--json` Azure CLI option.</span></span> <span data-ttu-id="86135-194">(Můžete použít **jsawk** nebo žádnou knihovnu jazyk dáváte přednost tooparse hello JSON.)</span><span class="sxs-lookup"><span data-stu-id="86135-194">(You can use **jsawk** or any language library you prefer tooparse hello JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="86135-195">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-195">Output:</span></span>

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

<span data-ttu-id="86135-196">účet úložiště tooinvestigate hello pomocí hello rozhraní příkazového řádku, musíte nejprve názvy účtů hello tooset a klíče.</span><span class="sxs-lookup"><span data-stu-id="86135-196">tooinvestigate hello storage account by using hello CLI, you first need tooset hello account names and keys.</span></span> <span data-ttu-id="86135-197">Nahraďte hello název účtu úložiště hello v hello následující ukázka s názvem, který zvolíte:</span><span class="sxs-lookup"><span data-stu-id="86135-197">Replace hello name of hello storage account in hello following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="86135-198">Potom můžete zobrazit informace z vašeho úložiště snadno:</span><span class="sxs-lookup"><span data-stu-id="86135-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="86135-199">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="86135-200">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="86135-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="86135-201">Dále budete tooneed toocreate virtuální síti se spuštěnou ve službě Azure a podsíť, ve kterém můžete vytvořit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="86135-201">Next you're going tooneed toocreate a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="86135-202">Hello následující příklad vytvoří virtuální síť s názvem `myVnet` s hello `192.168.0.0/16` předpona adresy:</span><span class="sxs-lookup"><span data-stu-id="86135-202">hello following example creates a virtual network named `myVnet` with hello `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="86135-203">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-203">Output:</span></span>

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

<span data-ttu-id="86135-204">Znovu, můžeme použít možnost--json hello `azure group show` a `jq` toosee jak vytváříme naše prostředky.</span><span class="sxs-lookup"><span data-stu-id="86135-204">Again, let's use hello --json option of `azure group show` and `jq` toosee how we're building our resources.</span></span> <span data-ttu-id="86135-205">Nyní je k dispozici `storageAccounts` prostředků a `virtualNetworks` prostředků.</span><span class="sxs-lookup"><span data-stu-id="86135-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="86135-206">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-206">Output:</span></span>

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

<span data-ttu-id="86135-207">Nyní Pojďme vytvořit podsíť v hello `myVnet` virtuální sítě, do které hello nasazených virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-207">Now let's create a subnet in hello `myVnet` virtual network into which hello VMs are deployed.</span></span> <span data-ttu-id="86135-208">Používáme hello `azure network vnet subnet create` příkazu společně s hello prostředky, které jste už vytvořili: hello `myResourceGroup` skupinu prostředků a hello `myVnet` virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="86135-208">We use hello `azure network vnet subnet create` command, along with hello resources we've already created: hello `myResourceGroup` resource group and hello `myVnet` virtual network.</span></span> <span data-ttu-id="86135-209">V následujícím příkladu hello, přidáme hello podsíť s názvem `mySubnet` s předponu adresy podsítě hello `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="86135-209">In hello following example, we add hello subnet named `mySubnet` with hello subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="86135-210">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="86135-211">Protože je ve virtuální síti hello logicky hello podsíť, podíváme pro informace o podsíti hello mírně odlišné příkazem.</span><span class="sxs-lookup"><span data-stu-id="86135-211">Because hello subnet is logically inside hello virtual network, we look for hello subnet information with a slightly different command.</span></span> <span data-ttu-id="86135-212">příkaz Hello používáme je `azure network vnet show`, abychom mohli pokračovat výstup JSON hello tooexamine pomocí, ale `jq`.</span><span class="sxs-lookup"><span data-stu-id="86135-212">hello command we use is `azure network vnet show`, but we continue tooexamine hello JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="86135-213">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-213">Output:</span></span>

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

## <a name="create-a-public-ip-address"></a><span data-ttu-id="86135-214">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="86135-214">Create a public IP address</span></span>
<span data-ttu-id="86135-215">Nyní vytvoříme hello veřejná IP adresa (PIP), jsme přiřadíte tooyour nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="86135-215">Now let's create hello public IP address (PIP) that we assign tooyour load balancer.</span></span> <span data-ttu-id="86135-216">Umožňuje tooconnect tooyour virtuální počítače z hello Internetu pomocí hello `azure network public-ip create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="86135-216">It enables you tooconnect tooyour VMs from hello Internet by using hello `azure network public-ip create` command.</span></span> <span data-ttu-id="86135-217">Protože hello výchozí adresa je dynamická, se nám vytvořit položku s názvem DNS v hello **cloudapp.azure.com** domény pomocí hello `--domain-name-label` možnost.</span><span class="sxs-lookup"><span data-stu-id="86135-217">Because hello default address is dynamic, we create a named DNS entry in hello **cloudapp.azure.com** domain by using hello `--domain-name-label` option.</span></span> <span data-ttu-id="86135-218">Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS hello `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="86135-218">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="86135-219">Vzhledem k tomu, že název DNS hello musí být jedinečný, zadáte svůj vlastní jedinečný název DNS:</span><span class="sxs-lookup"><span data-stu-id="86135-219">Because hello DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="86135-220">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
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

<span data-ttu-id="86135-221">Hello veřejná IP adresa je také prostředek nejvyšší úrovně, zobrazí se s `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="86135-221">hello public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="86135-222">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-222">Output:</span></span>

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

<span data-ttu-id="86135-223">Můžete prozkoumat další podrobnosti prostředků, včetně hello plně kvalifikovaný název domény (FQDN) hello poddomény pomocí hello dokončení `azure network public-ip show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="86135-223">You can investigate more resource details, including hello fully qualified domain name (FQDN) of hello subdomain, by using hello complete `azure network public-ip show` command.</span></span> <span data-ttu-id="86135-224">logicky byl přidělen prostředek Hello veřejné IP adresy, ale ještě nebyly přiřazeny konkrétní adresu.</span><span class="sxs-lookup"><span data-stu-id="86135-224">hello public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="86135-225">tooobtain IP adresu, budete tooneed Vyrovnávání zatížení, které jste dosud nevytvořili.</span><span class="sxs-lookup"><span data-stu-id="86135-225">tooobtain an IP address, you're going tooneed a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="86135-226">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-226">Output:</span></span>

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

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="86135-227">Vytvořit nástroj pro vyrovnávání zatížení a fondy IP adres</span><span class="sxs-lookup"><span data-stu-id="86135-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="86135-228">Když vytvoříte Vyrovnávání zatížení, umožní vám toodistribute provoz napříč více virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="86135-228">When you create a load balancer, it enables you toodistribute traffic across multiple VMs.</span></span> <span data-ttu-id="86135-229">Také poskytuje redundanci tooyour aplikace s spuštění několika virtuálních počítačů, které reagují toouser požadavky v případě hello údržby nebo velkým zatížením.</span><span class="sxs-lookup"><span data-stu-id="86135-229">It also provides redundancy tooyour application by running multiple VMs that respond toouser requests in hello event of maintenance or heavy loads.</span></span> <span data-ttu-id="86135-230">Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="86135-230">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="86135-231">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="86135-232">Naše nástroj pro vyrovnávání zatížení je docela prázdná, takže vytvoříme některé fondy IP adres.</span><span class="sxs-lookup"><span data-stu-id="86135-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="86135-233">Chceme toocreate dvěma fondy IP adres pro naše nástroj pro vyrovnávání zatížení, jednu pro hello front-endu a jeden pro hello back-end.</span><span class="sxs-lookup"><span data-stu-id="86135-233">We want toocreate two IP pools for our load balancer, one for hello front end and one for hello back end.</span></span> <span data-ttu-id="86135-234">fond IP front-endu Hello je veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="86135-234">hello front-end IP pool is publicly visible.</span></span> <span data-ttu-id="86135-235">Je také toowhich hello umístění, které jsme přiřadit hello PIP, kterou jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="86135-235">It's also hello location toowhich we assign hello PIP that we created earlier.</span></span> <span data-ttu-id="86135-236">Potom používáme fond back-end hello jako umístění pro naše tooconnect virtuálních počítačů k.</span><span class="sxs-lookup"><span data-stu-id="86135-236">Then we use hello back-end pool as a location for our VMs tooconnect to.</span></span> <span data-ttu-id="86135-237">Tímto způsobem můžete hello provoz procházet skrz toohello nástroje pro vyrovnávání zatížení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-237">That way, hello traffic can flow through hello load balancer toohello VMs.</span></span>

<span data-ttu-id="86135-238">Nejdříve vytvoříme naše front-end fond IP adres.</span><span class="sxs-lookup"><span data-stu-id="86135-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="86135-239">Hello následující příklad vytvoří front-end fond s názvem `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="86135-239">hello following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="86135-240">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="86135-241">Všimněte si, jak jsme použili hello `--public-ip-name` přepínač toopass ve hello `myPublicIP` kterou jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="86135-241">Note how we used hello `--public-ip-name` switch toopass in hello `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="86135-242">Přiřazení hello veřejnou IP adresu pro vyrovnávání zatížení toohello vám umožní tooreach virtuální počítače napříč hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="86135-242">Assigning hello public IP address toohello load balancer allows you tooreach your VMs across hello Internet.</span></span>

<span data-ttu-id="86135-243">V dalším kroku vytvoříme naše druhý fond IP této doby provozu našich back-end.</span><span class="sxs-lookup"><span data-stu-id="86135-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="86135-244">Hello následující příklad vytvoří fond back-end, s názvem `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="86135-244">hello following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="86135-245">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="86135-246">Vidíme úspěšnost naše nástroj pro vyrovnávání zatížení tak, že vyhledá s `azure network lb show` a prozkoumání výstup hello JSON:</span><span class="sxs-lookup"><span data-stu-id="86135-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining hello JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="86135-247">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-247">Output:</span></span>

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

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="86135-248">Vytvoření pravidla NAT nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="86135-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="86135-249">tooget provoz v našich Vyrovnávání zatížení, potřebujeme toocreate síťových adres překlad (NAT) pravidla určující příchozí nebo odchozí akce.</span><span class="sxs-lookup"><span data-stu-id="86135-249">tooget traffic flowing through our load balancer, we need toocreate network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="86135-250">Můžete zadat toouse hello protokol a pak mapování portů toointernal externí porty podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="86135-250">You can specify hello protocol toouse, then map external ports toointernal ports as desired.</span></span> <span data-ttu-id="86135-251">Pro naše prostředí vytvoříme některá pravidla, které umožňují SSH pomocí našich tooour nástroje pro vyrovnávání zatížení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-251">For our environment, let's create some rules that allow SSH through our load balancer tooour VMs.</span></span> <span data-ttu-id="86135-252">Nastaví port TCP porty 4222 a 4223 toodirect tooTCP 22 na našem virtuálních počítačích (které vytvoříme později).</span><span class="sxs-lookup"><span data-stu-id="86135-252">We set up TCP ports 4222 and 4223 toodirect tooTCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="86135-253">Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="86135-253">hello following example creates a rule named `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="86135-254">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
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

<span data-ttu-id="86135-255">Opakujte postup hello druhého pravidla NAT u SSH.</span><span class="sxs-lookup"><span data-stu-id="86135-255">Repeat hello procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="86135-256">Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="86135-256">hello following example creates a rule named `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="86135-257">Umožňuje také pokračujte a vytvořte tak pravidlo NAT pro port TCP 80 webových přenosů, pravidlo hello zapojování tooour fondy IP adres.</span><span class="sxs-lookup"><span data-stu-id="86135-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking hello rule up tooour IP pools.</span></span> <span data-ttu-id="86135-258">Pokud jsme propojte hello pravidlo tooan fondu IP, namísto zapojování hello pravidlo tooour virtuálních počítačů jednotlivě, jsme můžete přidat nebo odebrat z fondu IP adres hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-258">If we hook up hello rule tooan IP pool, instead of hooking up hello rule tooour VMs individually, we can add or remove VMs from hello IP pool.</span></span> <span data-ttu-id="86135-259">Nástroj pro vyrovnávání zatížení Hello automaticky upraví hello tok provozu.</span><span class="sxs-lookup"><span data-stu-id="86135-259">hello load balancer automatically adjusts hello flow of traffic.</span></span> <span data-ttu-id="86135-260">Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleWeb` toomap TCP port 80 tooport 80:</span><span class="sxs-lookup"><span data-stu-id="86135-260">hello following example creates a rule named `myLoadBalancerRuleWeb` toomap TCP port 80 tooport 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="86135-261">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
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

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="86135-262">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="86135-262">Create a load balancer health probe</span></span>
<span data-ttu-id="86135-263">Stavu testů pravidelně kontroly na hello virtuálních počítačů, které jsou za naše toomake nástroje pro vyrovnávání zatížení se svém operační a odpovídá toorequests, jak jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="86135-263">A health probe periodically checks on hello VMs that are behind our load balancer toomake sure they're operating and responding toorequests as defined.</span></span> <span data-ttu-id="86135-264">Pokud ne, jste odebrali z tooensure operaci, která nejsou uživatelé se přesměruje toothem.</span><span class="sxs-lookup"><span data-stu-id="86135-264">If not, they're removed from operation tooensure that users aren't being directed toothem.</span></span> <span data-ttu-id="86135-265">Můžete definovat vlastní kontroly pro test stavu hello, spolu s intervalech a hodnoty časového limitu.</span><span class="sxs-lookup"><span data-stu-id="86135-265">You can define custom checks for hello health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="86135-266">Další informace o sondy stavu najdete v tématu [nástroj pro vyrovnávání zatížení sondy](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="86135-267">Hello následující příklad vytvoří TCP stavu zjištěný pojmenované `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="86135-267">hello following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="86135-268">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="86135-269">Zde jsme zadali interval 15 sekund pro naše kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="86135-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="86135-270">Jsme můžete neproběhly maximálně čtyři sondy (jedné minuty) předtím, než nástroj pro vyrovnávání zatížení hello domnívá, že tohoto hostitele hello již není funkční.</span><span class="sxs-lookup"><span data-stu-id="86135-270">We can miss a maximum of four probes (one minute) before hello load balancer considers that hello host is no longer functioning.</span></span>

## <a name="verify-hello-load-balancer"></a><span data-ttu-id="86135-271">Ověřte hello nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="86135-271">Verify hello load balancer</span></span>
<span data-ttu-id="86135-272">Nyní se provádí konfigurace služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="86135-272">Now hello load balancer configuration is done.</span></span> <span data-ttu-id="86135-273">Zde jsou hello kroky, které jste pořídili:</span><span class="sxs-lookup"><span data-stu-id="86135-273">Here are hello steps you took:</span></span>

1. <span data-ttu-id="86135-274">Vytvořili jste nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="86135-274">You created a load balancer.</span></span>
2. <span data-ttu-id="86135-275">Vytvořili front-end IP fond a přiřazení veřejné tooit IP.</span><span class="sxs-lookup"><span data-stu-id="86135-275">You created a front-end IP pool and assigned a public IP tooit.</span></span>
3. <span data-ttu-id="86135-276">Můžete vytvořit fond back-end IP, které se mohou připojit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="86135-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="86135-277">Můžete vytvořit pravidla NAT, které umožňují SSH toohello virtuálních počítačů pro správu, společně s pravidlo, které umožní port TCP 80 pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86135-277">You created NAT rules that allow SSH toohello VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="86135-278">Jste přidali stav testu tooperiodically kontrola hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-278">You added a health probe tooperiodically check hello VMs.</span></span> <span data-ttu-id="86135-279">Tento test stavu zajistí, že si uživatelé pokusí tooaccess virtuální počítač, který je už funguje nebo poskytování obsahu.</span><span class="sxs-lookup"><span data-stu-id="86135-279">This health probe ensures that users don't try tooaccess a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="86135-280">Pojďme si shrnout, co nástroj pro vyrovnávání zatížení vypadá jako nyní:</span><span class="sxs-lookup"><span data-stu-id="86135-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="86135-281">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-281">Output:</span></span>

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

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a><span data-ttu-id="86135-282">Vytvoření toouse síťový adaptér s hello virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="86135-282">Create an NIC toouse with hello Linux VM</span></span>
<span data-ttu-id="86135-283">Síťové adaptéry jsou prostřednictvím kódu programu k dispozici, protože můžete použít pravidla tootheir použití.</span><span class="sxs-lookup"><span data-stu-id="86135-283">NICs are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="86135-284">Také můžete mít více než jednu.</span><span class="sxs-lookup"><span data-stu-id="86135-284">You can also have more than one.</span></span> <span data-ttu-id="86135-285">V následující hello `azure network nic create` příkaz spojit fond hello zatížení toohello síťový adaptér back-end IP adres a přidružte ji k hello NAT pravidlo toopermit SSH provoz.</span><span class="sxs-lookup"><span data-stu-id="86135-285">In hello following `azure network nic create` command, you hook up hello NIC toohello load back-end IP pool and associate it with hello NAT rule toopermit SSH traffic.</span></span>

<span data-ttu-id="86135-286">Nahraďte hello `#####-###-###` oddíly s vlastními ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="86135-286">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="86135-287">Vaše předplatné ID je uvedeno v výstup hello `jq` při kontrole hello prostředků, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="86135-287">Your subscription ID is noted in hello output of `jq` when you examine hello resources you are creating.</span></span> <span data-ttu-id="86135-288">Můžete také zobrazit svoje ID předplatného s `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="86135-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="86135-289">Hello následující příklad vytvoří síťový adaptér s názvem `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="86135-289">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="86135-290">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
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

<span data-ttu-id="86135-291">Zobrazí se podrobnosti hello prověřením přímo hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="86135-291">You can see hello details by examining hello resource directly.</span></span> <span data-ttu-id="86135-292">Zkontrolujte hello prostředků pomocí hello `azure network nic show` příkaz:</span><span class="sxs-lookup"><span data-stu-id="86135-292">You examine hello resource by using hello `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="86135-293">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-293">Output:</span></span>

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

<span data-ttu-id="86135-294">Teď vytvoříme hello druhý síťový adaptér, zapojování ve fondu back-end IP tooour znovu.</span><span class="sxs-lookup"><span data-stu-id="86135-294">Now we create hello second NIC, hooking in tooour back-end IP pool again.</span></span> <span data-ttu-id="86135-295">Tento čas hello druhé pravidlo NAT povolí provoz SSH.</span><span class="sxs-lookup"><span data-stu-id="86135-295">This time hello second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="86135-296">Hello následující příklad vytvoří síťový adaptér s názvem `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="86135-296">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="86135-297">Vytvořte skupinu zabezpečení sítě a pravidla</span><span class="sxs-lookup"><span data-stu-id="86135-297">Create a network security group and rules</span></span>
<span data-ttu-id="86135-298">Vytvořit skupinu zabezpečení sítě a hello příchozí teď pravidla, která řídí přístup k toohello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="86135-298">Now we create a network security group and hello inbound rules that govern access toohello NIC.</span></span> <span data-ttu-id="86135-299">Skupina zabezpečení sítě může být použité tooa síťového adaptéru nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="86135-299">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="86135-300">Můžete definovat pravidla toocontrol hello tok přenosů dat do aplikace a z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-300">You define rules toocontrol hello flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="86135-301">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="86135-301">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="86135-302">Přidejme hello příchozí pravidlo pro hello NSG tooallow příchozí připojení na portu 22 (toosupport SSH).</span><span class="sxs-lookup"><span data-stu-id="86135-302">Let's add hello inbound rule for hello NSG tooallow inbound connections on port 22 (toosupport SSH).</span></span> <span data-ttu-id="86135-303">Hello následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleSSH` tooallow TCP na portu 22:</span><span class="sxs-lookup"><span data-stu-id="86135-303">hello following example creates a rule named `myNetworkSecurityGroupRuleSSH` tooallow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="86135-304">Nyní Pojďme přidat příchozí pravidlo hello hello NSG tooallow příchozí připojení na portu 80 (toosupport webový provoz).</span><span class="sxs-lookup"><span data-stu-id="86135-304">Now let's add hello inbound rule for hello NSG tooallow inbound connections on port 80 (toosupport web traffic).</span></span> <span data-ttu-id="86135-305">Hello následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleHTTP` tooallow TCP na portu 80:</span><span class="sxs-lookup"><span data-stu-id="86135-305">hello following example creates a rule named `myNetworkSecurityGroupRuleHTTP` tooallow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="86135-306">Příchozí pravidlo Hello je filtr pro příchozí síťová připojení.</span><span class="sxs-lookup"><span data-stu-id="86135-306">hello inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="86135-307">V tomto příkladu jsme vazbu hello NSG toohello virtuální počítače virtuální síťovou kartu, což znamená, že všechny žádosti o tooport 22 předána toohello síťový adaptér na našem virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-307">In this example, we bind hello NSG toohello VMs virtual NIC, which means that any request tooport 22 is passed through toohello NIC on our VM.</span></span> <span data-ttu-id="86135-308">Toto pravidlo příchozí je o připojení k síti a není o koncový bod, který je co by bylo o v nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="86135-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="86135-309">tooopen port, musí zůstat hello `--source-port-range` nastavit příliš '\*' tooaccept (hello výchozí hodnota) příchozí požadavky od **žádné** požaduje portu.</span><span class="sxs-lookup"><span data-stu-id="86135-309">tooopen a port, you must leave hello `--source-port-range` set too'\*' (hello default value) tooaccept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="86135-310">Porty jsou obvykle dynamická.</span><span class="sxs-lookup"><span data-stu-id="86135-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-toohello-nic"></a><span data-ttu-id="86135-311">Vytvoření vazby toohello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="86135-311">Bind toohello NIC</span></span>
<span data-ttu-id="86135-312">Vytvoření vazby hello NSG toohello síťových karet.</span><span class="sxs-lookup"><span data-stu-id="86135-312">Bind hello NSG toohello NICs.</span></span> <span data-ttu-id="86135-313">Je nutné tooconnect naše síťové adaptéry s naše skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="86135-313">We need tooconnect our NICs with our network security group.</span></span> <span data-ttu-id="86135-314">Spuštění obou příkazů, toohook nahoru i naše síťových karet:</span><span class="sxs-lookup"><span data-stu-id="86135-314">Run both commands, toohook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="86135-315">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="86135-315">Create an availability set</span></span>
<span data-ttu-id="86135-316">Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="86135-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="86135-317">Umožňuje vytvořit sadu dostupnosti pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="86135-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="86135-318">Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="86135-318">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="86135-319">Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="86135-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="86135-320">Ve výchozím nastavení hello virtuálních počítačů, které jsou nakonfigurované v rámci vaší sady dostupnosti jsou oddělené v rámci až toothree domén selhání.</span><span class="sxs-lookup"><span data-stu-id="86135-320">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="86135-321">Rada Hello je, že problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="86135-321">hello idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="86135-322">Virtuální počítače Azure automaticky distribuuje mezi doménami selhání hello, když umístění do nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="86135-322">Azure automatically distributes VMs across hello fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="86135-323">Domén upgradu označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="86135-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="86135-324">Hello pořadí, ve kterém se restartují upgradu domény nemusí být po sobě jdoucích během plánované údržby, ale jenom jeden upgradu po restartu najednou.</span><span class="sxs-lookup"><span data-stu-id="86135-324">hello order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="86135-325">Znovu Azure automaticky distribuuje virtuální počítače napříč doménami upgradu, když umístění do stránku dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="86135-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="86135-326">Další informace o [Správa hello dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-326">Read more about [managing hello availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-hello-linux-vms"></a><span data-ttu-id="86135-327">Vytvořit virtuální počítače s Linuxem hello</span><span class="sxs-lookup"><span data-stu-id="86135-327">Create hello Linux VMs</span></span>
<span data-ttu-id="86135-328">Úložiště a síťové prostředky hello jste vytvořili virtuální počítače toosupport přístupné z Internetu.</span><span class="sxs-lookup"><span data-stu-id="86135-328">You've created hello storage and network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="86135-329">Nyní Pojďme vytvořit ty virtuální počítače a zabezpečit protokolem klíč SSH, který nemá heslo.</span><span class="sxs-lookup"><span data-stu-id="86135-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="86135-330">V tomto případě vytvoříme toocreate virtuálního počítače s Ubuntu podle hello nejnovější LTS.</span><span class="sxs-lookup"><span data-stu-id="86135-330">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="86135-331">Nemůžeme najít informace o tomto obrázku pomocí `azure vm image list`, jak je popsáno v [hledání Image virtuálního počítače Azure](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="86135-332">Jsme vybrali bitovou kopii pomocí příkazu hello `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="86135-332">We selected an image by using hello command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="86135-333">V tomto případě používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="86135-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="86135-334">V poli poslední hello jsme předat `latest` tak, aby v budoucnu hello nám vždycky získat nejnovější sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="86135-334">For hello last field, we pass `latest` so that in hello future we always get hello most recent build.</span></span> <span data-ttu-id="86135-335">(hello řetězec používáme je `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="86135-335">(hello string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="86135-336">Tento další krok je známé tooanyone, který již vytvořen ssh veřejného a privátního klíče rsa spárujte na systému Linux nebo Mac. pomocí **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="86135-336">This next step is familiar tooanyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="86135-337">Pokud nemáte žádné páry klíčů certifikátů vaší `~/.ssh` adresáře, můžete je vytvořit:</span><span class="sxs-lookup"><span data-stu-id="86135-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="86135-338">Automaticky pomocí hello `azure vm create --generate-ssh-keys` možnost.</span><span class="sxs-lookup"><span data-stu-id="86135-338">Automatically, by using hello `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="86135-339">Ručně pomocí [toocreate pokyny hello je sami](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-339">Manually, by using [hello instructions toocreate them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="86135-340">Alternativně můžete použít hello `--admin-password` tooauthenticate metoda vytvoření připojení SSH po hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-340">Alternatively, you can use hello `--admin-password` method tooauthenticate your SSH connections after hello VM is created.</span></span> <span data-ttu-id="86135-341">Tato metoda je obvykle méně bezpečné.</span><span class="sxs-lookup"><span data-stu-id="86135-341">This method is typically less secure.</span></span>

<span data-ttu-id="86135-342">Vytvoříme hello virtuálních počítačů tak, že převedou všechny naše prostředky a informace o společně s hello `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="86135-342">We create hello VM by bringing all our resources and information together with hello `azure vm create` command:</span></span>

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

<span data-ttu-id="86135-343">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="86135-344">Tooyour virtuálních počítačů můžete okamžitě připojit pomocí klíče SSH výchozí.</span><span class="sxs-lookup"><span data-stu-id="86135-344">You can connect tooyour VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="86135-345">Ujistěte se, zadejte odpovídající port hello, protože jsme se prošla hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="86135-345">Make sure that you specify hello appropriate port since we're passing through hello load balancer.</span></span> <span data-ttu-id="86135-346">(Pro naše první virtuální počítač, nastavíme hello NAT pravidlo tooforward port 4222 tooour virtuálního počítače.)</span><span class="sxs-lookup"><span data-stu-id="86135-346">(For our first VM, we set up hello NAT rule tooforward port 4222 tooour VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="86135-347">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-347">Output:</span></span>

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="86135-348">Pokračujte a vytvořit druhý virtuální počítač v hello stejným způsobem:</span><span class="sxs-lookup"><span data-stu-id="86135-348">Go ahead and create your second VM in hello same manner:</span></span>

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

<span data-ttu-id="86135-349">A teď můžete použít hello `azure vm show myResourceGroup myVM1` příkaz tooexamine, co jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="86135-349">And you can now use hello `azure vm show myResourceGroup myVM1` command tooexamine what you've created.</span></span> <span data-ttu-id="86135-350">V tomto okamžiku používáte Ubuntu virtuálních počítačů za službou Vyrovnávání zatížení v Azure, který můžete se přihlaste pouze s dvojici klíčů SSH (protože hesla jsou zakázané).</span><span class="sxs-lookup"><span data-stu-id="86135-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="86135-351">Můžete nainstalovat nginx nebo httpd, nasazení webové aplikace a zobrazit provoz hello toku prostřednictvím tooboth nástroje pro vyrovnávání zatížení hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-351">You can install nginx or httpd, deploy a web app, and see hello traffic flow through hello load balancer tooboth of hello VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="86135-352">Výstup:</span><span class="sxs-lookup"><span data-stu-id="86135-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
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


## <a name="export-hello-environment-as-a-template"></a><span data-ttu-id="86135-353">Export hello prostředí jako šablonu</span><span class="sxs-lookup"><span data-stu-id="86135-353">Export hello environment as a template</span></span>
<span data-ttu-id="86135-354">Teď, který jste vytvořili na tomto prostředí, jak postupovat, pokud chcete, aby toocreate další vývoj prostředí s hello stejnými parametry nebo produkčním prostředí, který odpovídá jeho?</span><span class="sxs-lookup"><span data-stu-id="86135-354">Now that you have built out this environment, what if you want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="86135-355">Správce prostředků používá šablony JSON, které definují všech hello parametrů pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="86135-355">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="86135-356">Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="86135-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="86135-357">Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávající šablonu JSON toocreate hello prostředí pro vás:</span><span class="sxs-lookup"><span data-stu-id="86135-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="86135-358">Tento příkaz vytvoří hello `myResourceGroup.json` souboru v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="86135-358">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="86135-359">Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků hello, včetně hello názvy pro nástroj pro vyrovnávání zatížení hello síťových rozhraní a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="86135-359">When you create an environment from this template, you are prompted for all hello resource names, including hello names for hello load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="86135-360">Tyto názvy v souboru šablony může vyplnit přidáním hello `-p` nebo `--includeParameterDefaultValue` parametr toohello `azure group export` příkaz, který byl dříve vidět.</span><span class="sxs-lookup"><span data-stu-id="86135-360">You can populate these names in your template file by adding hello `-p` or `--includeParameterDefaultValue` parameter toohello `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="86135-361">Upravit vaše JSON šablony toospecify hello názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="86135-361">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="86135-362">toocreate prostředí z šablony:</span><span class="sxs-lookup"><span data-stu-id="86135-362">toocreate an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="86135-363">Můžete chtít tooread [více o toodeploy ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="86135-363">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="86135-364">Další informace o tom, jak použít soubor parametrů hello tooincrementally aktualizace prostředí a přístup k šablony jedno umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="86135-364">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86135-365">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86135-365">Next steps</span></span>
<span data-ttu-id="86135-366">Nyní jste připravené toobegin práce s více síťovými součástmi a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="86135-366">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="86135-367">Tato ukázka toobuild prostředí můžete použít se vaše aplikace pomocí sem zavedl hello základní součásti služby.</span><span class="sxs-lookup"><span data-stu-id="86135-367">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
