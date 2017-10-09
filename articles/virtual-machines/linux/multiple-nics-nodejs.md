---
title: "virtuální počítač s Linuxem v Azure s více síťovými kartami aaaCreate | Microsoft Docs"
description: "Zjistěte, jak připojit toocreate virtuálního počítače s Linuxem s více síťovými kartami tooit pomocí rozhraní příkazového řádku Azure nebo správce prostředků šablony hello."
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
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="72402-103">Vytvořit virtuální počítač s Linuxem s více síťovými kartami pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="72402-103">Create a Linux virtual machine with multiple NICs using hello Azure CLI 1.0</span></span>
<span data-ttu-id="72402-104">Virtuální počítač (VM) můžete vytvořit v Azure, který má více tooit rozhraním (síťovým kartám) připojené virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="72402-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="72402-105">Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování.</span><span class="sxs-lookup"><span data-stu-id="72402-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="72402-106">Tento článek obsahuje rychlý příkazy toocreate virtuálního počítače s více tooit síťové adaptéry připojené.</span><span class="sxs-lookup"><span data-stu-id="72402-106">This article provides quick commands toocreate a VM with multiple NICs attached tooit.</span></span> <span data-ttu-id="72402-107">Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci vlastní Bash skripty, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="72402-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="72402-108">Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="72402-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

> [!WARNING]
> <span data-ttu-id="72402-109">Při vytvoření virtuálního počítače – nelze přidat stávající virtuální počítač s hello Azure CLI 1.0 tooan síťové adaptéry, je nutné připojit víc síťových karet.</span><span class="sxs-lookup"><span data-stu-id="72402-109">You must attach multiple NICs when you create a VM - you cannot add NICs tooan existing VM with hello Azure CLI 1.0.</span></span> <span data-ttu-id="72402-110">Můžete [přidat síťové adaptéry tooan existující virtuální počítač s hello Azure CLI 2.0](multiple-nics.md).</span><span class="sxs-lookup"><span data-stu-id="72402-110">You can [add NICs tooan existing VM with hello Azure CLI 2.0](multiple-nics.md).</span></span> <span data-ttu-id="72402-111">Můžete také [vytvoření virtuálního počítače založené na původní virtuální disky hello](copy-vm.md) a vytvořte několik síťových adaptérů, jak nasadit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="72402-111">You can also [create a VM based on hello original virtual disk(s)](copy-vm.md) and create multiple NICs as you deploy hello VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="72402-112">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="72402-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="72402-113">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="72402-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="72402-114">[Azure CLI 1.0](#create-supporting-resources) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="72402-114">[Azure CLI 1.0](#create-supporting-resources) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="72402-115">[Azure CLI 2.0](multiple-nics.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="72402-115">[Azure CLI 2.0](multiple-nics.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="72402-116">Vytvoření doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="72402-116">Create supporting resources</span></span>
<span data-ttu-id="72402-117">Ujistěte se, že máte hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="72402-117">Make sure that you have hello [Azure CLI](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="72402-118">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="72402-118">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="72402-119">Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="72402-119">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="72402-120">Nejprve vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="72402-120">First, create a resource group.</span></span> <span data-ttu-id="72402-121">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="72402-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

<span data-ttu-id="72402-122">Vytvořte virtuální počítače toohold účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="72402-122">Create a storage account toohold your VMs.</span></span> <span data-ttu-id="72402-123">Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:</span><span class="sxs-lookup"><span data-stu-id="72402-123">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

<span data-ttu-id="72402-124">Vytvořte virtuální počítače na tooconnect virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="72402-124">Create a virtual network tooconnect your VMs to.</span></span> <span data-ttu-id="72402-125">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s předponu adresy z *192.168.0.0/16*:</span><span class="sxs-lookup"><span data-stu-id="72402-125">hello following example creates a virtual network named *myVnet* with an address prefix of *192.168.0.0/16*:</span></span>

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="72402-126">Vytvořte dvě virtuální sítě podsítě – jednu pro provoz front-endu a jednu pro provoz back-end.</span><span class="sxs-lookup"><span data-stu-id="72402-126">Create two virtual network subnets - one for front-end traffic and one for back-end traffic.</span></span> <span data-ttu-id="72402-127">Hello následující příklad vytvoří dvě podsítě, s názvem *mySubnetFrontEnd* a *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="72402-127">hello following example creates two subnets, named *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

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

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="72402-128">Vytvořit a nakonfigurovat několik síťových adaptérů</span><span class="sxs-lookup"><span data-stu-id="72402-128">Create and configure multiple NICs</span></span>
<span data-ttu-id="72402-129">Si můžete přečíst další informace o [nasazení pomocí hello rozhraní příkazového řádku Azure několik síťových adaptérů](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), včetně skriptování hello proces ve smyčce přes toocreate všechny síťové adaptéry hello.</span><span class="sxs-lookup"><span data-stu-id="72402-129">You can read more details about [deploying multiple NICs using hello Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), including scripting hello process of looping through toocreate all hello NICs.</span></span>

<span data-ttu-id="72402-130">Hello následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, s jednou síťovou KARTOU připojení tooeach podsítě:</span><span class="sxs-lookup"><span data-stu-id="72402-130">hello following example creates two NICs, named *myNic1* and *myNic2*, with one NIC connecting tooeach subnet:</span></span>

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

<span data-ttu-id="72402-131">Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) toohelp spravovat a distribuovat provoz do virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="72402-131">Typically you also create a [Network Security Group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="72402-132">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="72402-132">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="72402-133">Vytvořit vazbu vaše síťové adaptéry toohello skupinu zabezpečení sítě pomocí `azure network nic set`.</span><span class="sxs-lookup"><span data-stu-id="72402-133">Bind your NICs toohello Network Security Group using `azure network nic set`.</span></span> <span data-ttu-id="72402-134">Hello následující příklad vytvoří vazbu *myNic1* a *myNic2* s *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="72402-134">hello following example binds *myNic1* and *myNic2* with *myNetworkSecurityGroup*:</span></span>

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

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="72402-135">Vytvoření virtuálního počítače a připojte síťové adaptéry hello</span><span class="sxs-lookup"><span data-stu-id="72402-135">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="72402-136">Při vytváření hello virtuálních počítačů, je nyní zadat několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="72402-136">When creating hello VM, you now specify multiple NICs.</span></span> <span data-ttu-id="72402-137">Místo použití `--nic-name` tooprovide jednu síťovou kartu, místo toho použít `--nic-names` a zadejte čárkami oddělený seznam síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="72402-137">Rather using `--nic-name` tooprovide a single NIC, instead you use `--nic-names` and provide a comma-separated list of NICs.</span></span> <span data-ttu-id="72402-138">Musíte taky tootake pozor, když vyberete hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="72402-138">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="72402-139">Existují omezení hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="72402-139">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="72402-140">Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="72402-140">Read more about [Linux VM sizes](sizes.md).</span></span> <span data-ttu-id="72402-141">Hello následující příklad ukazuje, jak toospecify několik síťových adaptérů a potom virtuální počítač velikost této podporuje použití více síťových adaptérů (*Standard_DS2_v2*):</span><span class="sxs-lookup"><span data-stu-id="72402-141">hello following example shows how toospecify multiple NICs and then a VM size that supports using multiple NICs (*Standard_DS2_v2*):</span></span>

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

## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="72402-142">Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="72402-142">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="72402-143">Šablony Azure Resource Manageru pomocí deklarativní toodefine soubory JSON prostředí.</span><span class="sxs-lookup"><span data-stu-id="72402-143">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="72402-144">Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="72402-144">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="72402-145">Správce prostředků šablony poskytují způsob, jak toocreate více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="72402-145">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="72402-146">Používáte *kopie* toospecify hello počet instancí toocreate:</span><span class="sxs-lookup"><span data-stu-id="72402-146">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="72402-147">Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="72402-147">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="72402-148">Můžete použít také `copyIndex()` toothen připojit číslo název prostředku tooa, což vám umožní toocreate `myNic1`, `myNic2`, atd. hello následující ukazuje příklad připojování hodnotu indexu hello:</span><span class="sxs-lookup"><span data-stu-id="72402-148">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="72402-149">Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="72402-149">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="72402-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72402-150">Next steps</span></span>
<span data-ttu-id="72402-151">Ujistěte se, že tooreview [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o toocreating virtuálního počítače s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="72402-151">Make sure tooreview [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="72402-152">Věnujte pozornost toohello maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="72402-152">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 

<span data-ttu-id="72402-153">Mějte na paměti, že nemůžete přidat další síťové adaptéry tooan existující virtuální počítač, je nutné vytvořit všechny síťové adaptéry hello při nasazování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="72402-153">Remember that you cannot add additional NICs tooan existing VM, you must create all hello NICs when you deploy hello VM.</span></span> <span data-ttu-id="72402-154">Vezměte v potaz při plánování vašeho nasazení toomake, že máte všechny potřebné hello síťové připojení z hello outset.</span><span class="sxs-lookup"><span data-stu-id="72402-154">Take care when planning your deployments toomake sure that you have all hello required network connectivity from hello outset.</span></span>

