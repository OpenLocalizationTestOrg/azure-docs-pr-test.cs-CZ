---
title: "Otevřete porty, které se virtuální počítač s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak otevřít port / create koncového bodu virtuálním počítačům s Linuxem pomocí modelu nasazení Azure resource manager a Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: d176187fe465264b5f433260de5178b48ca9dd4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="7a686-103">Otevřete porty a koncové body pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7a686-103">Open ports and endpoints to a Linux VM with the Azure CLI</span></span>
<span data-ttu-id="7a686-104">Otevření portu nebo vytvořte koncového bodu, virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7a686-104">You open a port, or create an endpoint, to a virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="7a686-105">Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě, který je připojen k prostředku, který přijímá provoz.</span><span class="sxs-lookup"><span data-stu-id="7a686-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached to the resource that receives the traffic.</span></span> <span data-ttu-id="7a686-106">Použijeme Běžným příkladem webové přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="7a686-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="7a686-107">Tento článek ukazuje, jak otevřít port pro virtuální počítač s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="7a686-107">This article shows you how to open a port to a VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="7a686-108">K provedení těchto kroků můžete také využít [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7a686-108">You can also perform these steps with the [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="7a686-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="7a686-109">Quick commands</span></span>
<span data-ttu-id="7a686-110">Chcete-li vytvořit skupinu zabezpečení sítě a pravidel, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="7a686-110">To create a Network Security Group and rules you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="7a686-111">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7a686-111">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="7a686-112">Zahrnout názvy parametrů příklad *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="7a686-112">Example parameter names include *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="7a686-113">Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="7a686-113">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="7a686-114">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="7a686-114">The following example creates a network security group named *myNetworkSecurityGroup* in the *eastus* location:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="7a686-115">Přidat pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) povolit provoz protokolu HTTP na svůj webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze).</span><span class="sxs-lookup"><span data-stu-id="7a686-115">Add a rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) to allow HTTP traffic to your webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="7a686-116">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* umožňující komunikaci TCP na portu 80:</span><span class="sxs-lookup"><span data-stu-id="7a686-116">The following example creates a rule named *myNetworkSecurityGroupRule* to allow TCP traffic on port 80:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

<span data-ttu-id="7a686-117">Přidružení skupiny zabezpečení sítě Virtuálního počítače síťovým rozhraním (NIC) [aktualizace seskupování sítě az](/cli/azure/network/nic#update).</span><span class="sxs-lookup"><span data-stu-id="7a686-117">Associate the Network Security Group with your VM's network interface (NIC) with [az network nic update](/cli/azure/network/nic#update).</span></span> <span data-ttu-id="7a686-118">Následující příklad přiřadí stávající síťové karty s názvem *myNic* ke skupině zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="7a686-118">The following example associates an existing NIC named *myNic* with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="7a686-119">Alternativně můžete přiřadit skupině zabezpečení sítě podsíť virtuální sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) spíše než jenom k síťové rozhraní na jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a686-119">Alternatively, you can associate your Network Security Group with a virtual network subnet with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) rather than just to the network interface on a single VM.</span></span> <span data-ttu-id="7a686-120">Následující příklad přiřadí existující podsíť s názvem *mySubnet* v *myVnet* virtuální síť s skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="7a686-120">The following example associates an existing subnet named *mySubnet* in the *myVnet* virtual network with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="7a686-121">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="7a686-121">More information on Network Security Groups</span></span>
<span data-ttu-id="7a686-122">Rychlé příkazy umožňují zprovoznění s provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a686-122">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="7a686-123">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="7a686-123">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="7a686-124">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#secure-network-traffic).</span><span class="sxs-lookup"><span data-stu-id="7a686-124">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#secure-network-traffic).</span></span>

<span data-ttu-id="7a686-125">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="7a686-125">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="7a686-126">Nástroje pro vyrovnávání zatížení distribuuje provoz do virtuálních počítačů s skupinu zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="7a686-126">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="7a686-127">Další informace najdete v tématu [jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="7a686-127">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a686-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a686-128">Next steps</span></span>
<span data-ttu-id="7a686-129">V tomto příkladu jste vytvořili jednoduché pravidlo umožňující přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a686-129">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="7a686-130">Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="7a686-130">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="7a686-131">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="7a686-131">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="7a686-132">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="7a686-132">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
