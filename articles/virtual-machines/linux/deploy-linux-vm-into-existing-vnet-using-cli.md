---
title: "Nasadit virtuální počítače s Linuxem do stávající sítě s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit virtuální počítač s Linuxem do existující virtuální síť pomocí Azure CLI 2.0"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 932fd74ec83f43b604382346ee2c273f5453fcd0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-cli"></a><span data-ttu-id="acc8a-103">Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="acc8a-103">How to deploy a Linux virtual machine into an existing Azure Virtual Network with the Azure CLI</span></span>

<span data-ttu-id="acc8a-104">Tento článek ukazuje, jak používat 2.0 rozhraní příkazového řádku Azure k nasazení virtuálního počítače (VM) do existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="acc8a-104">This article shows you how to use the Azure CLI 2.0 to deploy a virtual machine (VM) into an existing virtual network.</span></span> <span data-ttu-id="acc8a-105">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="acc8a-105">The requirements are:</span></span>

- [<span data-ttu-id="acc8a-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="acc8a-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="acc8a-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="acc8a-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

<span data-ttu-id="acc8a-108">K provedení těchto kroků můžete také využít [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-108">You can also perform these steps with the [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="acc8a-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="acc8a-109">Quick Commands</span></span>
<span data-ttu-id="acc8a-110">Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="acc8a-110">If you need to quickly accomplish the task, the following section details the  commands needed.</span></span> <span data-ttu-id="acc8a-111">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="acc8a-111">More detailed information and context for each step can be found the rest of the document, [starting here](#detailed-walkthrough).</span></span>

<span data-ttu-id="acc8a-112">K vytvoření tohoto vlastního prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="acc8a-112">To create this custom environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="acc8a-113">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="acc8a-113">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="acc8a-114">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="acc8a-114">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

<span data-ttu-id="acc8a-115">**Předběžných požadavků:** skupina prostředků Azure, virtuální síť a podsíť, příchozí skupinu zabezpečení sítě pomocí protokolu SSH a virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="acc8a-115">**Pre-requirements:** Azure resource group, virtual network and subnet, network security group with SSH inbound, and a virtual network interface card.</span></span>

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="acc8a-116">Virtuální počítač nasaďte do infrastruktury virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="acc8a-116">Deploy the VM into the virtual network infrastructure</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="acc8a-117">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="acc8a-117">Detailed walkthrough</span></span>

<span data-ttu-id="acc8a-118">Prostředky Azure jako virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="acc8a-118">Azure assets like virtual networks and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="acc8a-119">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="acc8a-119">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="acc8a-120">Vezměte v úvahu virtuální sítě, že síťový přepínač tradiční hardwaru – nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="acc8a-120">Think about a virtual network as being a traditional hardware network switch - you would not need to configure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="acc8a-121">Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové virtuální počítače do této virtuální sítě dokola s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="acc8a-121">With a correctly configured virtual network, you can continue to deploy new VMs into that virtual network over and over with few, if any, changes required over the life of the virtual network.</span></span>

<span data-ttu-id="acc8a-122">K vytvoření tohoto vlastního prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="acc8a-122">To create this custom environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="acc8a-123">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="acc8a-123">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="acc8a-124">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="acc8a-124">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="acc8a-125">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="acc8a-125">Create the resource group</span></span>

<span data-ttu-id="acc8a-126">Nejprve vytvořte skupinu prostředků Azure pro všechny objekty, které vytvoříte v tomto návodu uspořádání.</span><span class="sxs-lookup"><span data-stu-id="acc8a-126">First, create an Azure resource group to organize everything you create in this walkthrough.</span></span> <span data-ttu-id="acc8a-127">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-127">For more information on resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="acc8a-128">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-128">Create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="acc8a-129">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="acc8a-129">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-the-virtual-network"></a><span data-ttu-id="acc8a-130">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="acc8a-130">Create the virtual network</span></span>

<span data-ttu-id="acc8a-131">Umožňuje vytvářet virtuální sítě Azure ke spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="acc8a-131">Lets build an Azure virtual network to launch the VMs into.</span></span> <span data-ttu-id="acc8a-132">Další informace o virtuálních sítích najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-132">For more information on virtual networks, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span></span> <span data-ttu-id="acc8a-133">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-133">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="acc8a-134">Následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="acc8a-134">The following example creates a virtual network named *myVnet* and subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-the-network-security-group"></a><span data-ttu-id="acc8a-135">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="acc8a-135">Create the network security group</span></span>

<span data-ttu-id="acc8a-136">Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="acc8a-136">Azure network security groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="acc8a-137">Další informace o skupinách zabezpečení sítě najdete v tématu [postup vytvoření skupiny zabezpečení sítě v Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-137">For more information on network security groups, see [How to create network security groups in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span> <span data-ttu-id="acc8a-138">Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-138">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="acc8a-139">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="acc8a-139">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="acc8a-140">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="acc8a-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="acc8a-141">Virtuální počítač potřebuje přístup z Internetu, aby bylo pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="acc8a-141">The VM needs access from the internet, so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the VM is needed.</span></span> <span data-ttu-id="acc8a-142">Přidat příchozí pravidlo pro skupinu zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-142">Add an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="acc8a-143">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="acc8a-143">The following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-the-subnet-to-the-network-security-group"></a><span data-ttu-id="acc8a-144">Podsíť přiřadit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="acc8a-144">Attach the subnet to the network security group</span></span>

<span data-ttu-id="acc8a-145">Pravidla skupiny zabezpečení sítě je použít pro podsíť nebo konkrétní virtuální síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="acc8a-145">The network security group rules can be applied to a subnet or a specific virtual network interface.</span></span> <span data-ttu-id="acc8a-146">Umožňuje připojit skupinu zabezpečení sítě k naší podsítě.</span><span class="sxs-lookup"><span data-stu-id="acc8a-146">Lets attach the network security group to our subnet.</span></span> <span data-ttu-id="acc8a-147">Podsíť přiřadit skupinu zabezpečení sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update):</span><span class="sxs-lookup"><span data-stu-id="acc8a-147">Attach your subnet to the network security group with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update):</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-to-the-subnet"></a><span data-ttu-id="acc8a-148">Přidat virtuální síťový adaptér k podsíti</span><span class="sxs-lookup"><span data-stu-id="acc8a-148">Add a virtual network interface card to the subnet</span></span>

<span data-ttu-id="acc8a-149">Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="acc8a-149">Virtual network interface cards (VNics) are important as you can reuse them by connecting them to different VMs.</span></span> <span data-ttu-id="acc8a-150">Tato opakované použití umožňuje zachovat adaptéru VNic jako statické prostředků virtuálních počítačů mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="acc8a-150">This reuse allows you to keep the VNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="acc8a-151">Vytvoření adaptéru VNic a přidružte ji k podsíti s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-151">Create a VNic and associate it with the subnet with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="acc8a-152">Následující příklad vytvoří adaptéru VNic s názvem *myNic*:</span><span class="sxs-lookup"><span data-stu-id="acc8a-152">The following example creates a VNic named *myNic*:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="acc8a-153">Virtuální počítač nasaďte do infrastruktury virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="acc8a-153">Deploy the VM into the virtual network infrastructure</span></span>

<span data-ttu-id="acc8a-154">Nyní máte virtuální síť a podsíť a skupinu zabezpečení sítě chránit podsíť, blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH.</span><span class="sxs-lookup"><span data-stu-id="acc8a-154">You now have a virtual network and subnet, and a network security group to protect the subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="acc8a-155">Virtuální počítač teď můžou být nasazené uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="acc8a-155">The VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="acc8a-156">Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="acc8a-156">Create your VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="acc8a-157">Další informace o příznaky pomocí Azure CLI 2.0 používat a kompletní virtuální počítač nasadit, naleznete v části [vytvořit dokončení prostředí Linux pomocí příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-157">For more information on the flags to use with the Azure CLI 2.0 to deploy a complete VM, see [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md).</span></span>

<span data-ttu-id="acc8a-158">Následující příklad vytvoří virtuální počítač pomocí Azure spravované disků.</span><span class="sxs-lookup"><span data-stu-id="acc8a-158">The following example creates a VM using Azure Managed Disks.</span></span> <span data-ttu-id="acc8a-159">Tyto disky jsou zpracovávány platformy Azure a nevyžadují, aby všechny přípravné nebo umístění pro uložení.</span><span class="sxs-lookup"><span data-stu-id="acc8a-159">These disks are handled by the Azure platform and do not require any preparation or location to store them.</span></span> <span data-ttu-id="acc8a-160">Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../../storage/storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-160">For more information about managed disks, see [Azure Managed Disks overview](../../storage/storage-managed-disks-overview.md).</span></span> <span data-ttu-id="acc8a-161">Pokud chcete používat nespravované disky, najdete v části Další poznámku níže.</span><span class="sxs-lookup"><span data-stu-id="acc8a-161">If you wish to use unmanaged disks, see the additional note below.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

<span data-ttu-id="acc8a-162">Pokud používáte spravovaných disků, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="acc8a-162">If you use managed disks, skip this step.</span></span> <span data-ttu-id="acc8a-163">Pokud chcete používat nespravované disky, budete muset přidat další parametry do příkazu budete pokračovat, vytvořit nespravované disky v účtu úložiště s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="acc8a-163">If you wish to use unmanaged disks, you need to add the following additional parameters to the proceeding command to create unmanaged disks in the storage account named `mystorageaccount`:</span></span> 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

<span data-ttu-id="acc8a-164">Pomocí rozhraní příkazového řádku příznaky vyvolávající existujících prostředků dáte pokyn Azure k nasazení virtuálních počítačů uvnitř existující síť.</span><span class="sxs-lookup"><span data-stu-id="acc8a-164">By using the CLI flags to call out existing resources, you instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="acc8a-165">Jakmile jsou nasazené virtuální síť a podsíť, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="acc8a-165">Once a virtual network and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span> <span data-ttu-id="acc8a-166">V tomto příkladu není vytvořte a přiřaďte veřejnou IP adresu vnic, takže tento virtuální počítač není veřejně přístupné přes Internet.</span><span class="sxs-lookup"><span data-stu-id="acc8a-166">In this example, you did not create and assign a public IP address to the VNic, so this VM is not publicly accessible over the Internet.</span></span> <span data-ttu-id="acc8a-167">Další informace najdete v tématu [vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-167">For more information, see [Create a VM with a static public IP using the Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="acc8a-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acc8a-168">Next steps</span></span>
<span data-ttu-id="acc8a-169">Další informace o způsoby, jak vytvořit virtuální počítače v Azure najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="acc8a-169">For more information about ways to create virtual machines in Azure, see the following resources:</span></span>

* [<span data-ttu-id="acc8a-170">Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="acc8a-170">Use an Azure Resource Manager template to create a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="acc8a-171">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="acc8a-171">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="acc8a-172">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="acc8a-172">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
