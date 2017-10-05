---
title: "Vytvořte virtuální počítač s Linuxem v Azure s více síťovými kartami | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač s Linuxem s více síťovými kartami připojit se pomocí šablony Azure CLI 2.0 nebo správce prostředků."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 8a2931e462079c101c91497d459d7d3126234244
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a><span data-ttu-id="1de54-103">Vytvoření virtuální počítač s Linuxem v Azure s více síťových karet</span><span class="sxs-lookup"><span data-stu-id="1de54-103">How to create a Linux virtual machine in Azure with multiple network interface cards</span></span>
<span data-ttu-id="1de54-104">Virtuální počítač (VM) můžete vytvořit v Azure, který má více rozhraní virtuální sítě (NIC) je připojený.</span><span class="sxs-lookup"><span data-stu-id="1de54-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached to it.</span></span> <span data-ttu-id="1de54-105">Obvyklým scénářem je mít různé podsítě pro připojení front-end a back-end nebo síť vyhrazený pro řešení monitorování nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="1de54-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="1de54-106">Tento článek podrobně popisuje postup vytvoření virtuálního počítače s více síťovými kartami k němu připojen a postup přidání nebo odebrání síťové adaptéry ze stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1de54-106">This article details how to create a VM with multiple NICs attached to it and how to add or remove NICs from an existing VM.</span></span> <span data-ttu-id="1de54-107">Podrobné informace, včetně toho, jak vytvořit několik síťových adaptérů v rámci své vlastní skripty Bash, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-107">For detailed information, including how to create multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="1de54-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1de54-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="1de54-109">Tento článek podrobně popisuje postup vytvoření virtuálního počítače s více síťovými kartami s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="1de54-109">This article details how to create a VM with multiple NICs with the Azure CLI 2.0.</span></span> <span data-ttu-id="1de54-110">K provedení těchto kroků můžete také využít [Azure CLI 1.0](multiple-nics-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-110">You can also perform these steps with the [Azure CLI 1.0](multiple-nics-nodejs.md).</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="1de54-111">Vytvoření doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="1de54-111">Create supporting resources</span></span>
<span data-ttu-id="1de54-112">Nainstalujte si nejnovější verzi [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1de54-112">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="1de54-113">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1de54-113">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1de54-114">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="1de54-114">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="1de54-115">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-115">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1de54-116">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="1de54-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="1de54-117">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-117">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="1de54-118">Následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnetFrontEnd*:</span><span class="sxs-lookup"><span data-stu-id="1de54-118">The following example creates a virtual network named *myVnet* and subnet named *mySubnetFrontEnd*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="1de54-119">Vytvořte podsíť pro provoz back-end s [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-119">Create a subnet for the back-end traffic with [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).</span></span> <span data-ttu-id="1de54-120">Následující příklad vytvoří podsíť s názvem *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="1de54-120">The following example creates a subnet named *mySubnetBackEnd*:</span></span>

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

<span data-ttu-id="1de54-121">Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-121">Create a network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="1de54-122">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="1de54-122">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="1de54-123">Vytvořit a nakonfigurovat několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="1de54-123">Create and configure multiple NICs</span></span>
<span data-ttu-id="1de54-124">Vytvořte dva síťové adaptéry se [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-124">Create two NICs with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="1de54-125">Následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, připojené skupinu zabezpečení sítě s jednou síťovou KARTOU připojení ke každé podsítě:</span><span class="sxs-lookup"><span data-stu-id="1de54-125">The following example creates two NICs, named *myNic1* and *myNic2*, connected the network security group, with one NIC connecting to each subnet:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a><span data-ttu-id="1de54-126">Vytvoření virtuálního počítače a připojte síťové karty</span><span class="sxs-lookup"><span data-stu-id="1de54-126">Create a VM and attach the NICs</span></span>
<span data-ttu-id="1de54-127">Při vytváření virtuálního počítače, zadejte tyto adaptéry jste vytvořili pomocí `--nics`.</span><span class="sxs-lookup"><span data-stu-id="1de54-127">When you create the VM, specify the NICs you created with `--nics`.</span></span> <span data-ttu-id="1de54-128">Musíte také ujistěte se, když vyberete velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1de54-128">You also need to take care when you select the VM size.</span></span> <span data-ttu-id="1de54-129">Existují omezení pro celkový počet síťových adaptérů, které můžete přidat k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="1de54-129">There are limits for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="1de54-130">Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-130">Read more about [Linux VM sizes](sizes.md).</span></span> 

<span data-ttu-id="1de54-131">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-131">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="1de54-132">Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="1de54-132">The following example creates a VM named *myVM*:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-to-a-vm"></a><span data-ttu-id="1de54-133">Přidat síťovou kartu k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="1de54-133">Add a NIC to a VM</span></span>
<span data-ttu-id="1de54-134">V předchozím postupu vytvořili virtuální počítač s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="1de54-134">The previous steps created a VM with multiple NICs.</span></span> <span data-ttu-id="1de54-135">Síťové adaptéry můžete také přidat na existující virtuální počítač s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="1de54-135">You can also add NICs to an existing VM with the Azure CLI 2.0.</span></span> 

<span data-ttu-id="1de54-136">Vytvořte další síťová karta s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="1de54-136">Create another NIC with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="1de54-137">Následující příklad vytvoří síťový adaptér s názvem *myNic3* připojené k back-end podsíť a síť skupiny zabezpečení vytvořené v předchozích krocích:</span><span class="sxs-lookup"><span data-stu-id="1de54-137">The following example creates a NIC named *myNic3* connected to the back-end subnet and network security group created in the previous steps:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="1de54-138">Chcete-li přidat síťový adaptér na existující virtuální počítač, nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="1de54-138">To add a NIC to an existing VM, first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="1de54-139">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="1de54-139">The following example deallocates the VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1de54-140">Přidat síťovou kartu s [přidat síťový adaptér virtuálního počítače az](/cli/azure/vm/nic#add).</span><span class="sxs-lookup"><span data-stu-id="1de54-140">Add the NIC with [az vm nic add](/cli/azure/vm/nic#add).</span></span> <span data-ttu-id="1de54-141">Následující příklad přidá *myNic3* k *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="1de54-141">The following example adds *myNic3* to *myVM*:</span></span>

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

<span data-ttu-id="1de54-142">Spusťte virtuální počítač s [spuštění virtuálního počítače az](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="1de54-142">Start the VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a><span data-ttu-id="1de54-143">Odeberte síťový adaptér z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1de54-143">Remove a NIC from a VM</span></span>
<span data-ttu-id="1de54-144">Odebrat síťový adaptér ze stávajícího virtuálního počítače, nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="1de54-144">To remove a NIC from an existing VM, first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="1de54-145">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="1de54-145">The following example deallocates the VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1de54-146">Odeberte na síťový adaptér s [odebrat seskupování virtuálních počítačů az](/cli/azure/vm/nic#remove).</span><span class="sxs-lookup"><span data-stu-id="1de54-146">Remove the NIC with [az vm nic remove](/cli/azure/vm/nic#remove).</span></span> <span data-ttu-id="1de54-147">Následující příklad odebere *myNic3* z *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="1de54-147">The following example removes *myNic3* from *myVM*:</span></span>

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

<span data-ttu-id="1de54-148">Spusťte virtuální počítač s [spuštění virtuálního počítače az](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="1de54-148">Start the VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="1de54-149">Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1de54-149">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="1de54-150">Šablony Azure Resource Manageru použijte k definování prostředí deklarativní soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="1de54-150">Azure Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="1de54-151">Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-151">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="1de54-152">Správce prostředků šablony poskytují způsob, jak vytvořit více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="1de54-152">Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="1de54-153">Používáte *kopie* k určení počtu instancí vytvořit:</span><span class="sxs-lookup"><span data-stu-id="1de54-153">You use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="1de54-154">Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-154">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="1de54-155">Můžete použít také `copyIndex()` pak připojit k názvu zdroje, které vám umožní vytvořit číslo `myNic1`, `myNic2`atd. Na obrázku je příkladem připojením index hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1de54-155">You can also use a `copyIndex()` to then append a number to a resource name, which allows you to create `myNic1`, `myNic2`, etc. The following shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="1de54-156">Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="1de54-156">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1de54-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1de54-157">Next steps</span></span>
<span data-ttu-id="1de54-158">Zkontrolujte [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o vytvoření virtuálního počítače s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="1de54-158">Review [Linux VM sizes](sizes.md) when trying to creating a VM with multiple NICs.</span></span> <span data-ttu-id="1de54-159">Věnujte pozornost maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1de54-159">Pay attention to the maximum number of NICs each VM size supports.</span></span> 