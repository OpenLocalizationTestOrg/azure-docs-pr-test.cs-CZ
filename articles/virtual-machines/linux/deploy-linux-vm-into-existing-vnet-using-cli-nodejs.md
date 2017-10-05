---
title: "Nasadit virtuální počítače s Linuxem do existující síť s Azure CLI 1.0 | Microsoft Docs"
description: "Postup nasazení virtuálního počítače s Linuxem do existující virtuální síť pomocí Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 767a3f7cadba6b1e71e5a8f5995a9db090e419dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-cli-10"></a><span data-ttu-id="83c9a-103">Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure s Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="83c9a-103">How to deploy a Linux virtual machine into an existing Azure Virtual Network with the Azure CLI 1.0</span></span>

<span data-ttu-id="83c9a-104">Tento článek ukazuje, jak pomocí Azure CLI 1.0 nasazení virtuálního počítače (VM) do existující virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="83c9a-104">This article shows you how to use Azure CLI 1.0 to deploy a virtual machine (VM) into an existing Virtual Network (VNet).</span></span> <span data-ttu-id="83c9a-105">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="83c9a-105">The requirements are:</span></span>

- [<span data-ttu-id="83c9a-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="83c9a-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="83c9a-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="83c9a-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="83c9a-108">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="83c9a-108">CLI versions to complete the task</span></span>
<span data-ttu-id="83c9a-109">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="83c9a-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="83c9a-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="83c9a-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="83c9a-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="83c9a-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="83c9a-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="83c9a-112">Quick Commands</span></span>

<span data-ttu-id="83c9a-113">Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="83c9a-113">If you need to quickly accomplish the task, the following section details the commands needed.</span></span> <span data-ttu-id="83c9a-114">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="83c9a-114">More detailed information and context for each step can be found the rest of the document, [starting here](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span></span>

<span data-ttu-id="83c9a-115">Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť.</span><span class="sxs-lookup"><span data-stu-id="83c9a-115">Pre-requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span> <span data-ttu-id="83c9a-116">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="83c9a-116">Replace any examples with your own settings.</span></span>

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="83c9a-117">Virtuální počítač nasaďte do infrastruktury virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="83c9a-117">Deploy the VM into the virtual network infrastructure</span></span>

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="83c9a-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="83c9a-118">Detailed walkthrough</span></span>

<span data-ttu-id="83c9a-119">Prostředky Azure, například virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="83c9a-119">Azure assets like the VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="83c9a-120">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="83c9a-120">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="83c9a-121">Vezměte v úvahu, že je tradiční hardwaru síťový přepínač virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="83c9a-121">Think about a VNet as being a traditional hardware network switch.</span></span> <span data-ttu-id="83c9a-122">Nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="83c9a-122">You would not need to configure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="83c9a-123">Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové servery do ní opakovaně s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="83c9a-123">With a correctly configured VNet, you can continue to deploy new servers into that VNet over and over with few, if any, changes required over the life of the VNet.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="83c9a-124">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="83c9a-124">Create the resource group</span></span>

<span data-ttu-id="83c9a-125">Nejprve vytvořte skupinu prostředků pro všechny objekty, které vytvoříte v tomto návodu uspořádání.</span><span class="sxs-lookup"><span data-stu-id="83c9a-125">First, create a resource group to organize everything you create in this walkthrough.</span></span> <span data-ttu-id="83c9a-126">Další informace o skupinách prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="83c9a-126">For more information about resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-the-vnet"></a><span data-ttu-id="83c9a-127">Vytvoření sítě VNet</span><span class="sxs-lookup"><span data-stu-id="83c9a-127">Create the VNet</span></span>

<span data-ttu-id="83c9a-128">Prvním krokem je vytvoření virtuální sítě ke spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="83c9a-128">The first step is to build a VNet to launch the VMs into.</span></span> <span data-ttu-id="83c9a-129">Virtuální sítě obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="83c9a-129">The VNet contains one subnet for this walkthrough.</span></span> <span data-ttu-id="83c9a-130">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="83c9a-130">For more information on Azure VNets, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span></span>

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-the-network-security-group"></a><span data-ttu-id="83c9a-131">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="83c9a-131">Create the network security group</span></span>

<span data-ttu-id="83c9a-132">Podsíť je vytvořené za existující síti skupinu zabezpečení, vytvořit skupinu zabezpečení sítě před podsíť.</span><span class="sxs-lookup"><span data-stu-id="83c9a-132">The subnet is built behind an existing network security group so build the network security group before the subnet.</span></span> <span data-ttu-id="83c9a-133">Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="83c9a-133">Azure network security groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="83c9a-134">Další informace o skupinách zabezpečení sítě Azure, najdete v části [postup vytvoření skupiny zabezpečení sítě v rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="83c9a-134">For more information on Azure network security groups, see [How to create network security groups in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span></span>

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="83c9a-135">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="83c9a-135">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="83c9a-136">Virtuální počítač potřebuje přístup z Internetu, aby bylo pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="83c9a-136">The VM needs access from the internet so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a><span data-ttu-id="83c9a-137">Přidat podsíť k virtuální síti</span><span class="sxs-lookup"><span data-stu-id="83c9a-137">Add a subnet to the VNet</span></span>

<span data-ttu-id="83c9a-138">Virtuální počítače v rámci virtuální sítě musí být umístěny v podsíti.</span><span class="sxs-lookup"><span data-stu-id="83c9a-138">VMs within the VNet must be located in a subnet.</span></span> <span data-ttu-id="83c9a-139">Každý virtuální sítě může mít několik podsítí.</span><span class="sxs-lookup"><span data-stu-id="83c9a-139">Each VNet can have multiple subnets.</span></span> <span data-ttu-id="83c9a-140">Vytvořte podsíť a přidružte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="83c9a-140">Create the subnet and associate with the network security group.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

<span data-ttu-id="83c9a-141">Podsíť je nyní přidána uvnitř virtuální sítě a přidružené skupiny zabezpečení sítě a pravidla.</span><span class="sxs-lookup"><span data-stu-id="83c9a-141">The Subnet is now added inside the VNet and associated with the network security group and rule.</span></span>


## <a name="add-a-vnic-to-the-subnet"></a><span data-ttu-id="83c9a-142">Přidání adaptéru VNic k podsíti</span><span class="sxs-lookup"><span data-stu-id="83c9a-142">Add a VNic to the subnet</span></span>

<span data-ttu-id="83c9a-143">Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="83c9a-143">Virtual network cards (VNics) are important as you can reuse them by connecting them to different VMs.</span></span> <span data-ttu-id="83c9a-144">Tento přístup zajišťuje VNic jako statické prostředek při virtuálních počítačů může být dočasné.</span><span class="sxs-lookup"><span data-stu-id="83c9a-144">This approach keeps the VNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="83c9a-145">Vytvoření adaptéru VNic a přidružte ji k podsíť vytvořená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="83c9a-145">Create a VNic and associate it with the subnet created in the previous step.</span></span>

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a><span data-ttu-id="83c9a-146">Virtuální počítač nasaďte do virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="83c9a-146">Deploy the VM into the VNet and NSG</span></span>

<span data-ttu-id="83c9a-147">Nyní máte virtuální síť a podsíť uvnitř této virtuální sítě a skupinu zabezpečení sítě funguje chránit podsíť, blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH.</span><span class="sxs-lookup"><span data-stu-id="83c9a-147">You now have a VNet and subnet inside that VNet, and a network security group acting to protect the subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="83c9a-148">Virtuální počítač teď můžou být nasazené uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="83c9a-148">The VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="83c9a-149">Použití Azure CLI a `azure vm create` příkaz virtuálního počítače s Linuxem se nasadí do existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.</span><span class="sxs-lookup"><span data-stu-id="83c9a-149">Using the Azure CLI, and the `azure vm create` command, the Linux VM is deployed to the existing Azure Resource Group, VNet, Subnet, and VNic.</span></span> <span data-ttu-id="83c9a-150">Další informace o nasazení dokončení virtuálního počítače pomocí rozhraní příkazového řádku najdete v tématu [vytvořit úplný prostředí Linux pomocí rozhraní příkazového řádku Azure](create-cli-complete.md)</span><span class="sxs-lookup"><span data-stu-id="83c9a-150">For more information on using the CLI to deploy a complete VM, see [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md)</span></span>

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

<span data-ttu-id="83c9a-151">Pomocí rozhraní příkazového řádku příznaky vyvolávající existujících prostředků dáte pokyn Azure k nasazení virtuálních počítačů uvnitř existující síť.</span><span class="sxs-lookup"><span data-stu-id="83c9a-151">By using the CLI flags to call out existing resources, you instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="83c9a-152">Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="83c9a-152">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="83c9a-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83c9a-153">Next steps</span></span>

* [<span data-ttu-id="83c9a-154">Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="83c9a-154">Use an Azure Resource Manager template to create a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="83c9a-155">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="83c9a-155">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="83c9a-156">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="83c9a-156">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
