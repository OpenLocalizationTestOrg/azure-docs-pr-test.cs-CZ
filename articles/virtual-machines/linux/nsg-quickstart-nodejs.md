---
title: "Otevřete porty, které se virtuální počítač s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak otevřít port / create koncového bodu virtuálním počítačům s Linuxem pomocí modelu nasazení Azure resource manager a Azure CLI 1.0"
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
ms.openlocfilehash: 847bc76c37ed929851712ba1c12463a01032e267
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure-using-the-azure-cli-10"></a><span data-ttu-id="8afad-103">Otevření portů a koncové body pro virtuální počítač s Linuxem v Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8afad-103">Opening ports and endpoints to a Linux VM in Azure using the Azure CLI 1.0</span></span>
<span data-ttu-id="8afad-104">Otevření portu nebo vytvořte koncového bodu, virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8afad-104">You open a port, or create an endpoint, to a virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="8afad-105">Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě, který je připojen k prostředku, který přijímá provoz.</span><span class="sxs-lookup"><span data-stu-id="8afad-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached to the resource that receives the traffic.</span></span> <span data-ttu-id="8afad-106">Použijeme Běžným příkladem webové přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="8afad-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="8afad-107">Tento článek ukazuje, jak otevřít port k virtuálnímu počítači pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="8afad-107">This article shows you how to open a port to a VM using the Azure CLI 1.0.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="8afad-108">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="8afad-108">CLI versions to complete the task</span></span>
<span data-ttu-id="8afad-109">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8afad-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="8afad-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="8afad-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="8afad-111">[Azure CLI 2.0](nsg-quickstart.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="8afad-111">[Azure CLI 2.0](nsg-quickstart.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="8afad-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="8afad-112">Quick commands</span></span>
<span data-ttu-id="8afad-113">Chcete-li vytvořit skupinu zabezpečení sítě a pravidel, je nutné [Azure CLI 1.0](../../cli-install-nodejs.md) nainstalovaná a pomocí režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="8afad-113">To create a Network Security Group and rules you need [the Azure CLI 1.0](../../cli-install-nodejs.md) installed and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="8afad-114">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8afad-114">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="8afad-115">Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="8afad-115">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="8afad-116">Vytvořte vaši skupinu zabezpečení sítě, zadáte vlastní názvy a umístění správně.</span><span class="sxs-lookup"><span data-stu-id="8afad-116">Create your Network Security Group, entering your own names and location appropriately.</span></span> <span data-ttu-id="8afad-117">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="8afad-117">The following example creates a Network Security Group named *myNetworkSecurityGroup* in the *eastus* location:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="8afad-118">Přidáte pravidlo povolit provoz protokolu HTTP na svůj webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze).</span><span class="sxs-lookup"><span data-stu-id="8afad-118">Add a rule to allow HTTP traffic to your webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="8afad-119">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* umožňující komunikaci TCP na portu 80:</span><span class="sxs-lookup"><span data-stu-id="8afad-119">The following example creates a rule named *myNetworkSecurityGroupRule* to allow TCP traffic on port 80:</span></span>

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

<span data-ttu-id="8afad-120">Přidružte skupinu zabezpečení sítě Virtuálního počítače síťové rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="8afad-120">Associate the Network Security Group with your VM's network interface (NIC).</span></span> <span data-ttu-id="8afad-121">Následující příklad přiřadí stávající síťové karty s názvem *myNic* ke skupině zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="8afad-121">The following example associates an existing NIC named *myNic* with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

<span data-ttu-id="8afad-122">Alternativně můžete přidružit vaše skupina zabezpečení sítě s podsítí virtuální sítě a nikoli pouze k síťové rozhraní na jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8afad-122">Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just to the network interface on a single VM.</span></span> <span data-ttu-id="8afad-123">Následující příklad přiřadí existující podsíť s názvem *mySubnet* v *myVnet* virtuální síť s skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="8afad-123">The following example associates an existing subnet named *mySubnet* in the *myVnet* virtual network with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="8afad-124">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="8afad-124">More information on Network Security Groups</span></span>
<span data-ttu-id="8afad-125">Rychlé příkazy umožňují zprovoznění s provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8afad-125">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="8afad-126">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="8afad-126">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="8afad-127">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8afad-127">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span>

<span data-ttu-id="8afad-128">Skupiny zabezpečení sítě a pravidla seznamu ACL můžete definovat v rámci šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8afad-128">You can define Network Security Groups and ACL rules as part of Azure Resource Manager templates.</span></span> <span data-ttu-id="8afad-129">Další informace o [vytvoření skupin zabezpečení sítě pomocí šablony](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="8afad-129">Read more about [creating Network Security Groups with templates](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span></span>

<span data-ttu-id="8afad-130">Pokud budete muset použít předávání portů pro mapování jedinečný externí port pro interní port na vašem virtuálním počítači, použijte nástroj pro vyrovnávání zatížení a pravidla překladu adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="8afad-130">If you need to use port-forwarding to map a unique external port to an internal port on your VM, use a load balancer and Network Address Translation (NAT) rules.</span></span> <span data-ttu-id="8afad-131">Můžete například chtít vystavit externě TCP port 8080 a mít data na port TCP 80 na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8afad-131">For example, you may want to expose TCP port 8080 externally and have traffic directed to TCP port 80 on a VM.</span></span> <span data-ttu-id="8afad-132">Dozvíte se o [zařízení na Vyrovnávání zatížení internetového](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8afad-132">You can learn about [creating an Internet-facing load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8afad-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8afad-133">Next steps</span></span>
<span data-ttu-id="8afad-134">V tomto příkladu jste vytvořili jednoduché pravidlo umožňující přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="8afad-134">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="8afad-135">Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="8afad-135">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="8afad-136">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8afad-136">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="8afad-137">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="8afad-137">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="8afad-138">Přehled Azure Resource Manageru pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8afad-138">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

