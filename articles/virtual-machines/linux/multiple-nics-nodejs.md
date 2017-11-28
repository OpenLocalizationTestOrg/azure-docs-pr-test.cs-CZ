---
title: "Vytvořte virtuální počítač s Linuxem v Azure s více síťovými kartami | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač s Linuxem s více síťovými kartami připojit se pomocí šablony Azure CLI nebo správce prostředků."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 814825cce61909167a1247a96c17a3ee9c5f2af4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-the-azure-cli-10"></a><span data-ttu-id="d3a44-103">Vytvořit virtuální počítač s Linuxem s více síťovými kartami pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d3a44-103">Create a Linux virtual machine with multiple NICs using the Azure CLI 1.0</span></span>
<span data-ttu-id="d3a44-104">Virtuální počítač (VM) můžete vytvořit v Azure, který má více rozhraní virtuální sítě (NIC) je připojený.</span><span class="sxs-lookup"><span data-stu-id="d3a44-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached to it.</span></span> <span data-ttu-id="d3a44-105">Obvyklým scénářem je mít různé podsítě pro připojení front-end a back-end nebo síť vyhrazený pro řešení monitorování nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="d3a44-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="d3a44-106">Tento článek obsahuje rychlý příkazů pro vytvoření virtuálního počítače s více síťovými kartami k němu připojen.</span><span class="sxs-lookup"><span data-stu-id="d3a44-106">This article provides quick commands to create a VM with multiple NICs attached to it.</span></span> <span data-ttu-id="d3a44-107">Podrobné informace, včetně toho, jak vytvořit několik síťových adaptérů v rámci své vlastní skripty Bash, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-107">For detailed information, including how to create multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="d3a44-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3a44-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

> [!WARNING]
> <span data-ttu-id="d3a44-109">Při vytvoření virtuálního počítače – síťovými kartami nelze přidat do existující virtuální počítač s 1.0 rozhraní příkazového řádku Azure, je nutné připojit víc síťových karet.</span><span class="sxs-lookup"><span data-stu-id="d3a44-109">You must attach multiple NICs when you create a VM - you cannot add NICs to an existing VM with the Azure CLI 1.0.</span></span> <span data-ttu-id="d3a44-110">Můžete [přidání síťových adaptérů do stávajícího virtuálního počítače pomocí Azure CLI 2.0](multiple-nics.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-110">You can [add NICs to an existing VM with the Azure CLI 2.0](multiple-nics.md).</span></span> <span data-ttu-id="d3a44-111">Můžete také [vytvoření virtuálního počítače založené na původní virtuální disky](copy-vm.md) a vytvořte několik síťových adaptérů, jak nasadit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d3a44-111">You can also [create a VM based on the original virtual disk(s)](copy-vm.md) and create multiple NICs as you deploy the VM.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="d3a44-112">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="d3a44-112">CLI versions to complete the task</span></span>
<span data-ttu-id="d3a44-113">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d3a44-113">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="d3a44-114">[Azure CLI 1.0](#create-supporting-resources) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="d3a44-114">[Azure CLI 1.0](#create-supporting-resources) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="d3a44-115">[Azure CLI 2.0](multiple-nics.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="d3a44-115">[Azure CLI 2.0](multiple-nics.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="d3a44-116">Vytvoření doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="d3a44-116">Create supporting resources</span></span>
<span data-ttu-id="d3a44-117">Ujistěte se, že máte [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="d3a44-117">Make sure that you have the [Azure CLI](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="d3a44-118">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d3a44-118">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="d3a44-119">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="d3a44-119">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="d3a44-120">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3a44-120">First, create a resource group.</span></span> <span data-ttu-id="d3a44-121">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="d3a44-121">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

<span data-ttu-id="d3a44-122">Vytvořte účet úložiště pro uložení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d3a44-122">Create a storage account to hold your VMs.</span></span> <span data-ttu-id="d3a44-123">Následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:</span><span class="sxs-lookup"><span data-stu-id="d3a44-123">The following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

<span data-ttu-id="d3a44-124">Vytvoření virtuální sítě pro virtuální počítače k připojení.</span><span class="sxs-lookup"><span data-stu-id="d3a44-124">Create a virtual network to connect your VMs to.</span></span> <span data-ttu-id="d3a44-125">Následující příklad vytvoří virtuální síť s názvem *myVnet* s předponu adresy z *192.168.0.0/16*:</span><span class="sxs-lookup"><span data-stu-id="d3a44-125">The following example creates a virtual network named *myVnet* with an address prefix of *192.168.0.0/16*:</span></span>

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="d3a44-126">Vytvořte dvě virtuální sítě podsítě – jednu pro provoz front-endu a jednu pro provoz back-end.</span><span class="sxs-lookup"><span data-stu-id="d3a44-126">Create two virtual network subnets - one for front-end traffic and one for back-end traffic.</span></span> <span data-ttu-id="d3a44-127">Následující příklad vytvoří dvě podsítě, s názvem *mySubnetFrontEnd* a *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="d3a44-127">The following example creates two subnets, named *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="d3a44-128">Vytvořit a nakonfigurovat několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="d3a44-128">Create and configure multiple NICs</span></span>
<span data-ttu-id="d3a44-129">Si můžete přečíst další informace o [nasazení pomocí rozhraní příkazového řádku Azure několik síťových adaptérů](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), včetně skriptování proces vytvoření všechny síťové adaptéry ve smyčce.</span><span class="sxs-lookup"><span data-stu-id="d3a44-129">You can read more details about [deploying multiple NICs using the Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), including scripting the process of looping through to create all the NICs.</span></span>

<span data-ttu-id="d3a44-130">Následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, s jednou síťovou KARTOU připojení ke každé podsítě:</span><span class="sxs-lookup"><span data-stu-id="d3a44-130">The following example creates two NICs, named *myNic1* and *myNic2*, with one NIC connecting to each subnet:</span></span>

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

<span data-ttu-id="d3a44-131">Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) ke správě a distribuci přenosů mezi virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d3a44-131">Typically you also create a [Network Security Group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) to help manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="d3a44-132">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="d3a44-132">The following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="d3a44-133">Vytvoření vazby vaše síťové adaptéry na skupinu zabezpečení sítě pomocí `azure network nic set`.</span><span class="sxs-lookup"><span data-stu-id="d3a44-133">Bind your NICs to the Network Security Group using `azure network nic set`.</span></span> <span data-ttu-id="d3a44-134">Následující příklad vytvoří vazbu *myNic1* a *myNic2* s *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="d3a44-134">The following example binds *myNic1* and *myNic2* with *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a><span data-ttu-id="d3a44-135">Vytvoření virtuálního počítače a připojte síťové karty</span><span class="sxs-lookup"><span data-stu-id="d3a44-135">Create a VM and attach the NICs</span></span>
<span data-ttu-id="d3a44-136">Při vytváření virtuálního počítače, můžete teď určit několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="d3a44-136">When creating the VM, you now specify multiple NICs.</span></span> <span data-ttu-id="d3a44-137">Místo použití `--nic-name` poskytovat jednu síťovou kartu, místo toho je použití `--nic-names` a zadejte čárkami oddělený seznam síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="d3a44-137">Rather using `--nic-name` to provide a single NIC, instead you use `--nic-names` and provide a comma-separated list of NICs.</span></span> <span data-ttu-id="d3a44-138">Musíte také ujistěte se, když vyberete velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3a44-138">You also need to take care when you select the VM size.</span></span> <span data-ttu-id="d3a44-139">Existují omezení pro celkový počet síťových adaptérů, které můžete přidat k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="d3a44-139">There are limits for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="d3a44-140">Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-140">Read more about [Linux VM sizes](sizes.md).</span></span> <span data-ttu-id="d3a44-141">Následující příklad ukazuje, jak určit víc síťových karet a pak velikost virtuálního počítače, který podporuje použití více síťových adaptérů (*Standard_DS2_v2*):</span><span class="sxs-lookup"><span data-stu-id="d3a44-141">The following example shows how to specify multiple NICs and then a VM size that supports using multiple NICs (*Standard_DS2_v2*):</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="d3a44-142">Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d3a44-142">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="d3a44-143">Šablony Azure Resource Manageru použijte k definování prostředí deklarativní soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="d3a44-143">Azure Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="d3a44-144">Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-144">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="d3a44-145">Správce prostředků šablony poskytují způsob, jak vytvořit více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="d3a44-145">Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="d3a44-146">Používáte *kopie* k určení počtu instancí vytvořit:</span><span class="sxs-lookup"><span data-stu-id="d3a44-146">You use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="d3a44-147">Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-147">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="d3a44-148">Můžete použít také `copyIndex()` pak připojit k názvu zdroje, které vám umožní vytvořit číslo `myNic1`, `myNic2`atd. Na obrázku je příkladem připojením index hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d3a44-148">You can also use a `copyIndex()` to then append a number to a resource name, which allows you to create `myNic1`, `myNic2`, etc. The following shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="d3a44-149">Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="d3a44-149">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3a44-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3a44-150">Next steps</span></span>
<span data-ttu-id="d3a44-151">Projděte si [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o vytvoření virtuálního počítače s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="d3a44-151">Make sure to review [Linux VM sizes](sizes.md) when trying to creating a VM with multiple NICs.</span></span> <span data-ttu-id="d3a44-152">Věnujte pozornost maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3a44-152">Pay attention to the maximum number of NICs each VM size supports.</span></span> 

<span data-ttu-id="d3a44-153">Mějte na paměti, že další síťové adaptéry nelze přidat do stávajícího virtuálního počítače, když nasazujete virtuální počítač je nutné vytvořit všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="d3a44-153">Remember that you cannot add additional NICs to an existing VM, you must create all the NICs when you deploy the VM.</span></span> <span data-ttu-id="d3a44-154">Vezměte v potaz při plánování nasazení a ujistěte se, že máte všechny požadované síťové připojení od samého počátku.</span><span class="sxs-lookup"><span data-stu-id="d3a44-154">Take care when planning your deployments to make sure that you have all the required network connectivity from the outset.</span></span>

