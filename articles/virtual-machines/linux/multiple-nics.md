---
title: "virtuální počítač s Linuxem v Azure s více síťovými kartami aaaCreate | Microsoft Docs"
description: "Zjistěte, jak připojit toocreate virtuálního počítače s Linuxem s více síťovými kartami tooit pomocí šablony Azure CLI 2.0 nebo správce prostředků hello."
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
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a><span data-ttu-id="dccd0-103">Jak toocreate virtuální počítač s Linuxem v Azure s více síťových rozhraní karty</span><span class="sxs-lookup"><span data-stu-id="dccd0-103">How toocreate a Linux virtual machine in Azure with multiple network interface cards</span></span>
<span data-ttu-id="dccd0-104">Virtuální počítač (VM) můžete vytvořit v Azure, který má více tooit rozhraním (síťovým kartám) připojené virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="dccd0-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="dccd0-105">Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování.</span><span class="sxs-lookup"><span data-stu-id="dccd0-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="dccd0-106">Tento článek popisuje, jak připojit tooit toocreate virtuálního počítače s více síťovými kartami a jak tooadd nebo odeberte síťové adaptéry ze stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dccd0-106">This article details how toocreate a VM with multiple NICs attached tooit and how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="dccd0-107">Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci vlastní Bash skripty, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="dccd0-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dccd0-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="dccd0-109">Tento článek podrobně popisuje, jak toocreate a virtuálních počítačů s více síťovými kartami s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dccd0-109">This article details how toocreate a VM with multiple NICs with hello Azure CLI 2.0.</span></span> <span data-ttu-id="dccd0-110">Můžete také provést tyto kroky hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-110">You can also perform these steps with hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="dccd0-111">Vytvoření doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="dccd0-111">Create supporting resources</span></span>
<span data-ttu-id="dccd0-112">Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="dccd0-112">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="dccd0-113">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="dccd0-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="dccd0-114">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="dccd0-114">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="dccd0-115">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-115">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="dccd0-116">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="dccd0-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="dccd0-117">Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-117">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="dccd0-118">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnetFrontEnd*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-118">hello following example creates a virtual network named *myVnet* and subnet named *mySubnetFrontEnd*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="dccd0-119">Vytvořte podsíť pro provoz back-end hello s [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-119">Create a subnet for hello back-end traffic with [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).</span></span> <span data-ttu-id="dccd0-120">Hello následující příklad vytvoří podsíť s názvem *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-120">hello following example creates a subnet named *mySubnetBackEnd*:</span></span>

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

<span data-ttu-id="dccd0-121">Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-121">Create a network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="dccd0-122">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-122">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="dccd0-123">Vytvořit a nakonfigurovat několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="dccd0-123">Create and configure multiple NICs</span></span>
<span data-ttu-id="dccd0-124">Vytvořte dva síťové adaptéry se [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-124">Create two NICs with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="dccd0-125">Hello následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, propojených hello skupinu zabezpečení sítě, s jednou síťovou KARTOU připojení tooeach podsítě:</span><span class="sxs-lookup"><span data-stu-id="dccd0-125">hello following example creates two NICs, named *myNic1* and *myNic2*, connected hello network security group, with one NIC connecting tooeach subnet:</span></span>

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

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="dccd0-126">Vytvoření virtuálního počítače a připojte síťové adaptéry hello</span><span class="sxs-lookup"><span data-stu-id="dccd0-126">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="dccd0-127">Při vytváření hello virtuální počítač, zadejte síťové adaptéry hello jste vytvořili pomocí `--nics`.</span><span class="sxs-lookup"><span data-stu-id="dccd0-127">When you create hello VM, specify hello NICs you created with `--nics`.</span></span> <span data-ttu-id="dccd0-128">Musíte taky tootake pozor, když vyberete hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dccd0-128">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="dccd0-129">Existují omezení hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dccd0-129">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="dccd0-130">Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-130">Read more about [Linux VM sizes](sizes.md).</span></span> 

<span data-ttu-id="dccd0-131">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-131">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="dccd0-132">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-132">hello following example creates a VM named *myVM*:</span></span>

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

## <a name="add-a-nic-tooa-vm"></a><span data-ttu-id="dccd0-133">Přidat tooa síťový adaptér virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dccd0-133">Add a NIC tooa VM</span></span>
<span data-ttu-id="dccd0-134">předchozí kroky Hello vytvořili virtuální počítač s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="dccd0-134">hello previous steps created a VM with multiple NICs.</span></span> <span data-ttu-id="dccd0-135">Můžete také přidat existující virtuální počítač s hello Azure CLI 2.0 tooan síťových karet.</span><span class="sxs-lookup"><span data-stu-id="dccd0-135">You can also add NICs tooan existing VM with hello Azure CLI 2.0.</span></span> 

<span data-ttu-id="dccd0-136">Vytvořte další síťová karta s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="dccd0-136">Create another NIC with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="dccd0-137">Hello následující příklad vytvoří síťový adaptér s názvem *myNic3* připojené podsítě toohello back-end a skupinu zabezpečení sítě vytvořené v předchozích krocích hello:</span><span class="sxs-lookup"><span data-stu-id="dccd0-137">hello following example creates a NIC named *myNic3* connected toohello back-end subnet and network security group created in hello previous steps:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="dccd0-138">tooadd tooan seskupování existující virtuální počítač, nejprve zrušit přidělení hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="dccd0-138">tooadd a NIC tooan existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="dccd0-139">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-139">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="dccd0-140">Přidat hello síťový adaptér s [přidat síťový adaptér virtuálního počítače az](/cli/azure/vm/nic#add).</span><span class="sxs-lookup"><span data-stu-id="dccd0-140">Add hello NIC with [az vm nic add](/cli/azure/vm/nic#add).</span></span> <span data-ttu-id="dccd0-141">Hello následující příklad přidá *myNic3* příliš*Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-141">hello following example adds *myNic3* too*myVM*:</span></span>

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

<span data-ttu-id="dccd0-142">Spustit virtuální počítač s hello [az spuštění virtuálního počítače](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="dccd0-142">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a><span data-ttu-id="dccd0-143">Odeberte síťový adaptér z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dccd0-143">Remove a NIC from a VM</span></span>
<span data-ttu-id="dccd0-144">tooremove síťový adaptér ze stávajícího virtuálního počítače, nejprve navrácení hello virtuálních počítačů s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="dccd0-144">tooremove a NIC from an existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="dccd0-145">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-145">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="dccd0-146">Odebrat hello síťový adaptér s [odebrat seskupování virtuálních počítačů az](/cli/azure/vm/nic#remove).</span><span class="sxs-lookup"><span data-stu-id="dccd0-146">Remove hello NIC with [az vm nic remove](/cli/azure/vm/nic#remove).</span></span> <span data-ttu-id="dccd0-147">Hello následující příklad odebere *myNic3* z *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dccd0-147">hello following example removes *myNic3* from *myVM*:</span></span>

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

<span data-ttu-id="dccd0-148">Spustit virtuální počítač s hello [az spuštění virtuálního počítače](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="dccd0-148">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="dccd0-149">Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="dccd0-149">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="dccd0-150">Šablony Azure Resource Manageru pomocí deklarativní toodefine soubory JSON prostředí.</span><span class="sxs-lookup"><span data-stu-id="dccd0-150">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="dccd0-151">Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-151">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="dccd0-152">Správce prostředků šablony poskytují způsob, jak toocreate více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="dccd0-152">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="dccd0-153">Používáte *kopie* toospecify hello počet instancí toocreate:</span><span class="sxs-lookup"><span data-stu-id="dccd0-153">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="dccd0-154">Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-154">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="dccd0-155">Můžete použít také `copyIndex()` toothen připojit číslo název prostředku tooa, což vám umožní toocreate `myNic1`, `myNic2`, atd. hello následující ukazuje příklad připojování hodnotu indexu hello:</span><span class="sxs-lookup"><span data-stu-id="dccd0-155">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="dccd0-156">Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="dccd0-156">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dccd0-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dccd0-157">Next steps</span></span>
<span data-ttu-id="dccd0-158">Zkontrolujte [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o toocreating virtuálního počítače s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="dccd0-158">Review [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="dccd0-159">Věnujte pozornost toohello maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="dccd0-159">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 
