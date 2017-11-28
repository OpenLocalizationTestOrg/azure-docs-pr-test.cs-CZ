---
title: "aaaOpen porty tooa virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Linuxem pomocí hello Azure resource manager nasazení modelu a hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 337c37d151f527b43d4852291159b2f70a00accc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a><span data-ttu-id="00f6b-103">Hello otevírání portů a koncové body tooa virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="00f6b-103">Opening ports and endpoints tooa Linux VM in Azure using hello Azure CLI 1.0</span></span>
<span data-ttu-id="00f6b-104">Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="00f6b-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="00f6b-105">Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello.</span><span class="sxs-lookup"><span data-stu-id="00f6b-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="00f6b-106">Použijeme Běžným příkladem webové přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="00f6b-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="00f6b-107">Tento článek ukazuje, jak hello tooopen port tooa virtuální počítač pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="00f6b-107">This article shows you how tooopen a port tooa VM using hello Azure CLI 1.0.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="00f6b-108">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="00f6b-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="00f6b-109">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="00f6b-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="00f6b-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="00f6b-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="00f6b-111">[Azure CLI 2.0](nsg-quickstart.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="00f6b-111">[Azure CLI 2.0](nsg-quickstart.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="00f6b-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="00f6b-112">Quick commands</span></span>
<span data-ttu-id="00f6b-113">toocreate skupinu zabezpečení sítě a pravidel, je nutné [hello Azure CLI 1.0](../../cli-install-nodejs.md) nainstalovaná a pomocí režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="00f6b-113">toocreate a Network Security Group and rules you need [hello Azure CLI 1.0](../../cli-install-nodejs.md) installed and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="00f6b-114">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="00f6b-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="00f6b-115">Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="00f6b-115">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="00f6b-116">Vytvořte vaši skupinu zabezpečení sítě, zadáte vlastní názvy a umístění správně.</span><span class="sxs-lookup"><span data-stu-id="00f6b-116">Create your Network Security Group, entering your own names and location appropriately.</span></span> <span data-ttu-id="00f6b-117">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="00f6b-117">hello following example creates a Network Security Group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="00f6b-118">Přidat pravidlo tooallow HTTP provoz tooyour webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze).</span><span class="sxs-lookup"><span data-stu-id="00f6b-118">Add a rule tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="00f6b-119">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow TCP přenosy na portu 80:</span><span class="sxs-lookup"><span data-stu-id="00f6b-119">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

<span data-ttu-id="00f6b-120">Hello skupinu zabezpečení sítě přidružení síťového rozhraní Virtuálního počítače (NIC).</span><span class="sxs-lookup"><span data-stu-id="00f6b-120">Associate hello Network Security Group with your VM's network interface (NIC).</span></span> <span data-ttu-id="00f6b-121">Hello následující příklad přiřadí stávající síťové karty s názvem *myNic* s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="00f6b-121">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

<span data-ttu-id="00f6b-122">Alternativně můžete přidružit vaše skupina zabezpečení sítě podsíť virtuální sítě spíše než jenom toohello síťové rozhraní na jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="00f6b-122">Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="00f6b-123">Hello následující příklad přiřadí existující podsíť s názvem *mySubnet* v hello *myVnet* virtuální síť s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="00f6b-123">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="00f6b-124">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="00f6b-124">More information on Network Security Groups</span></span>
<span data-ttu-id="00f6b-125">Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="00f6b-125">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="00f6b-126">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="00f6b-126">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="00f6b-127">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="00f6b-127">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span>

<span data-ttu-id="00f6b-128">Skupiny zabezpečení sítě a pravidla seznamu ACL můžete definovat v rámci šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="00f6b-128">You can define Network Security Groups and ACL rules as part of Azure Resource Manager templates.</span></span> <span data-ttu-id="00f6b-129">Další informace o [vytvoření skupin zabezpečení sítě pomocí šablony](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="00f6b-129">Read more about [creating Network Security Groups with templates](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span></span>

<span data-ttu-id="00f6b-130">Pokud potřebujete toouse předávání portů toomap interní port tooan jedinečný externí port na vašem virtuálním počítači, použijte nástroj pro vyrovnávání zatížení a pravidla překladu adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="00f6b-130">If you need toouse port-forwarding toomap a unique external port tooan internal port on your VM, use a load balancer and Network Address Translation (NAT) rules.</span></span> <span data-ttu-id="00f6b-131">Můžete například chcete tooexpose port TCP 8080 externě a mít přenášená tooTCP port 80 na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="00f6b-131">For example, you may want tooexpose TCP port 8080 externally and have traffic directed tooTCP port 80 on a VM.</span></span> <span data-ttu-id="00f6b-132">Dozvíte se o [zařízení na Vyrovnávání zatížení internetového](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="00f6b-132">You can learn about [creating an Internet-facing load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="00f6b-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00f6b-133">Next steps</span></span>
<span data-ttu-id="00f6b-134">V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo.</span><span class="sxs-lookup"><span data-stu-id="00f6b-134">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="00f6b-135">Můžete najít informace o vytváření podrobnější prostředí v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="00f6b-135">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="00f6b-136">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="00f6b-136">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="00f6b-137">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="00f6b-137">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="00f6b-138">Přehled Azure Resource Manageru pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="00f6b-138">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

