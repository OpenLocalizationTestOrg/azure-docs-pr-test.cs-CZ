---
title: "aaaOpen porty tooa virtuálního počítače s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Linuxem pomocí hello Azure resource manager nasazení modelu a hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: c79b31206e97558171609cf033bb3cb3370777c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="1f174-103">Otevřete porty a koncové body tooa virtuálního počítače s Linuxem se hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1f174-103">Open ports and endpoints tooa Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="1f174-104">Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1f174-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="1f174-105">Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello.</span><span class="sxs-lookup"><span data-stu-id="1f174-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="1f174-106">Použijeme Běžným příkladem webové přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="1f174-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="1f174-107">Tento článek ukazuje, jak tooopen port tooa virtuálních počítačů s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1f174-107">This article shows you how tooopen a port tooa VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="1f174-108">Můžete také provést tyto kroky hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="1f174-108">You can also perform these steps with hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="1f174-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="1f174-109">Quick commands</span></span>
<span data-ttu-id="1f174-110">Skupina zabezpečení sítě toocreate a pravidel, je nutné hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1f174-110">toocreate a Network Security Group and rules you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="1f174-111">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="1f174-111">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1f174-112">Zahrnout názvy parametrů příklad *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="1f174-112">Example parameter names include *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="1f174-113">Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="1f174-113">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="1f174-114">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="1f174-114">hello following example creates a network security group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="1f174-115">Přidat pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) tooallow HTTP provozu tooyour webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze).</span><span class="sxs-lookup"><span data-stu-id="1f174-115">Add a rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="1f174-116">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow TCP přenosy na portu 80:</span><span class="sxs-lookup"><span data-stu-id="1f174-116">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

<span data-ttu-id="1f174-117">Přidružení hello skupinu zabezpečení sítě Virtuálního počítače síťovým rozhraním (NIC) [aktualizace seskupování sítě az](/cli/azure/network/nic#update).</span><span class="sxs-lookup"><span data-stu-id="1f174-117">Associate hello Network Security Group with your VM's network interface (NIC) with [az network nic update](/cli/azure/network/nic#update).</span></span> <span data-ttu-id="1f174-118">Hello následující příklad přiřadí stávající síťové karty s názvem *myNic* s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="1f174-118">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="1f174-119">Alternativně můžete přiřadit skupině zabezpečení sítě podsíť virtuální sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) místo právě toohello síťové rozhraní na jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1f174-119">Alternatively, you can associate your Network Security Group with a virtual network subnet with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="1f174-120">Hello následující příklad přiřadí existující podsíť s názvem *mySubnet* v hello *myVnet* virtuální síť s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="1f174-120">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="1f174-121">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="1f174-121">More information on Network Security Groups</span></span>
<span data-ttu-id="1f174-122">Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1f174-122">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="1f174-123">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="1f174-123">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="1f174-124">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#secure-network-traffic).</span><span class="sxs-lookup"><span data-stu-id="1f174-124">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#secure-network-traffic).</span></span>

<span data-ttu-id="1f174-125">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="1f174-125">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="1f174-126">Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="1f174-126">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="1f174-127">Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="1f174-127">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f174-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f174-128">Next steps</span></span>
<span data-ttu-id="1f174-129">V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1f174-129">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="1f174-130">Můžete najít informace o vytváření podrobnější prostředí v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="1f174-130">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="1f174-131">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1f174-131">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="1f174-132">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="1f174-132">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
