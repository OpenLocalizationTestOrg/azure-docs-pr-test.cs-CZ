---
title: "Použít interní DNS pro překlad názvů virtuálních počítačů s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak vytvořit virtuální síťové adaptéry a používat interní DNS pro překlad názvů virtuálních počítačů v Azure pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: 992920adb1ae3736d43cc5f0bbb2081a20a1674d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="e8f71-103">Vytvořit virtuální síťové karty a používat interní DNS pro překlad názvů virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="e8f71-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="e8f71-104">Tento článek ukazuje, jak nastavit statické interní názvy DNS pro virtuální počítače s Linuxem pomocí Azure CLI 2.0 virtuální síťové karty (vNics) a štítek názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="e8f71-104">This article shows you how to set static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with the Azure CLI 2.0.</span></span> <span data-ttu-id="e8f71-105">K provedení těchto kroků můžete také využít [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8f71-105">You can also perform these steps with the [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e8f71-106">Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.</span><span class="sxs-lookup"><span data-stu-id="e8f71-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="e8f71-107">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="e8f71-107">The requirements are:</span></span>

* [<span data-ttu-id="e8f71-108">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="e8f71-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="e8f71-109">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="e8f71-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="e8f71-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="e8f71-110">Quick commands</span></span>
<span data-ttu-id="e8f71-111">Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="e8f71-111">If you need to quickly accomplish the task, the following section details the commands needed.</span></span> <span data-ttu-id="e8f71-112">Podrobnější informace a kontext pro každý krok naleznete ve zbývající části dokumentu [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="e8f71-112">More detailed information and context for each step can be found in the rest of the document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="e8f71-113">K provedení těchto kroků, budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e8f71-113">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="e8f71-114">Předběžných požadavků: Skupinu prostředků, virtuální síť a podsíť, skupinu zabezpečení sítě pomocí protokolu SSH příchozí.</span><span class="sxs-lookup"><span data-stu-id="e8f71-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="e8f71-115">Vytvořit virtuální síťový adaptér se statickou interní název DNS</span><span class="sxs-lookup"><span data-stu-id="e8f71-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="e8f71-116">Vytvoření vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-116">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="e8f71-117">`--internal-dns-name` Příznak rozhraní příkazového řádku je k nastavení DNS popisku, který poskytuje statické název DNS pro virtuální síťové karty (vNic).</span><span class="sxs-lookup"><span data-stu-id="e8f71-117">The `--internal-dns-name` CLI flag is for setting the DNS label, which provides the static DNS name for the virtual network interface card (vNic).</span></span> <span data-ttu-id="e8f71-118">Následující příklad vytvoří adaptéru vNic s názvem `myNic`, připojí se k `myVnet` virtuální síti a vytvoří interní záznam název DNS názvem `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-118">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a><span data-ttu-id="e8f71-119">Nasadit virtuální počítač a připojte se adaptéru vNic</span><span class="sxs-lookup"><span data-stu-id="e8f71-119">Deploy a VM and connect the vNic</span></span>
<span data-ttu-id="e8f71-120">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="e8f71-121">`--nics` Příznak během nasazení do Azure adaptér vNic připojuje k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e8f71-121">The `--nics` flag connects the vNic to the VM during the deployment to Azure.</span></span> <span data-ttu-id="e8f71-122">Následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí adaptér vNic s názvem `myNic` z předchozího kroku:</span><span class="sxs-lookup"><span data-stu-id="e8f71-122">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="e8f71-123">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="e8f71-123">Detailed walkthrough</span></span>

<span data-ttu-id="e8f71-124">Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určitých serverech být statické nebo dlohotrvající servery infrastruktury v Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f71-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers to be static or long-lived servers.</span></span> <span data-ttu-id="e8f71-125">Doporučujeme, aby Azure prostředky jako virtuální sítě a skupiny zabezpečení sítě jsou statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="e8f71-125">It is recommended that Azure assets like the virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="e8f71-126">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e8f71-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="e8f71-127">Později můžete přidat server úložiště Git nebo automatizační server volaných přináší CiCd do této virtuální sítě pro vývoj nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="e8f71-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd to this virtual network for your development or test environments.</span></span>  

<span data-ttu-id="e8f71-128">Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f71-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="e8f71-129">Protože se jedná o interní názvy DNS, nejsou přeložit na mimo Internetu, poskytuje dodatečné zabezpečení infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e8f71-129">Because the DNS names are internal, they are not resolvable to the outside internet, providing additional security to the infrastructure.</span></span>

<span data-ttu-id="e8f71-130">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e8f71-130">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e8f71-131">Zahrnout názvy parametrů příklad `myResourceGroup`, `myNic`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="e8f71-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="e8f71-132">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e8f71-132">Create the resource group</span></span>
<span data-ttu-id="e8f71-133">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-133">First, create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="e8f71-134">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="e8f71-134">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a><span data-ttu-id="e8f71-135">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e8f71-135">Create the virtual network</span></span>

<span data-ttu-id="e8f71-136">Dalším krokem je vytvoření virtuální sítě pro spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="e8f71-136">The next step is to build a virtual network to launch the VMs into.</span></span> <span data-ttu-id="e8f71-137">Virtuální síť obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="e8f71-137">The virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="e8f71-138">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8f71-138">For more information on Azure virtual networks, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="e8f71-139">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-139">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="e8f71-140">Následující příklad vytvoří virtuální síť s názvem `myVnet` a podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-140">The following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a><span data-ttu-id="e8f71-141">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="e8f71-141">Create the Network Security Group</span></span>
<span data-ttu-id="e8f71-142">Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="e8f71-142">Azure Network Security Groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="e8f71-143">Další informace o skupinách zabezpečení sítě najdete v tématu [vytvoření skupin Nsg v Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8f71-143">For more information about Network Security Groups, see [How to create NSGs in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="e8f71-144">Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-144">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="e8f71-145">Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-145">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a><span data-ttu-id="e8f71-146">Přidat příchozí pravidlo umožňující SSH</span><span class="sxs-lookup"><span data-stu-id="e8f71-146">Add an inbound rule to allow SSH</span></span>
<span data-ttu-id="e8f71-147">Přidat příchozí pravidlo pro skupinu zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-147">Add an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="e8f71-148">Následující příklad vytvoří pravidlo s názvem `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-148">The following example creates a rule named `myRuleAllowSSH`:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a><span data-ttu-id="e8f71-149">Podsíť přidružit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="e8f71-149">Associate the subnet with the Network Security Group</span></span>
<span data-ttu-id="e8f71-150">Chcete-li přidružit podsítě skupinu zabezpečení sítě, použijte [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="e8f71-150">To associate the subnet with the Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="e8f71-151">Následující příklad přiřadí název podsítě `mySubnet` ke skupině zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-151">The following example associates the subnet name `mySubnet` with the Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="e8f71-152">Vytvořit virtuální síťovou kartu a statické názvy DNS</span><span class="sxs-lookup"><span data-stu-id="e8f71-152">Create the virtual network interface card and static DNS names</span></span>
<span data-ttu-id="e8f71-153">Azure je velmi flexibilní, ale pokud chcete použít názvy DNS pro rozlišení názvu virtuálního počítače, budete muset vytvořit virtuální síťové adaptéry (vNics), které obsahují název DNS.</span><span class="sxs-lookup"><span data-stu-id="e8f71-153">Azure is very flexible, but to use DNS names for VM name resolution, you need to create virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="e8f71-154">vNics jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů přes životního cyklu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e8f71-154">vNics are important as you can reuse them by connecting them to different VMs over the infrastructure lifecycle.</span></span> <span data-ttu-id="e8f71-155">Tento přístup zajišťuje vNic jako statické prostředek při virtuálních počítačů může být dočasné.</span><span class="sxs-lookup"><span data-stu-id="e8f71-155">This approach keeps the vNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="e8f71-156">Pomocí DNS označování na kartě vNic jsou povolit jednoduchý překladu názvů z jiných virtuálních počítačů ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e8f71-156">By using DNS labeling on the vNic, we are able to enable simple name resolution from other VMs in the VNet.</span></span> <span data-ttu-id="e8f71-157">Použití přeložit názvy umožňuje ostatním virtuálním počítačům přístup k serveru automatizace podle názvu DNS `Jenkins` nebo server Git jako `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="e8f71-157">Using resolvable names enables other VMs to access the automation server by the DNS name `Jenkins` or the Git server as `gitrepo`.</span></span>  

<span data-ttu-id="e8f71-158">Vytvoření vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-158">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="e8f71-159">Následující příklad vytvoří adaptéru vNic s názvem `myNic`, připojí se k `myVnet` virtuální síť s názvem `myVnet`a vytvoří interní záznam název DNS názvem `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="e8f71-159">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="e8f71-160">Virtuální počítač nasaďte do infrastruktury virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e8f71-160">Deploy the VM into the virtual network infrastructure</span></span>
<span data-ttu-id="e8f71-161">Máme teď virtuální síť a podsíť, skupinu zabezpečení sítě funguje jako brána firewall blokuje veškerý příchozí provoz, s výjimkou port 22 pro SSH a adaptéru vNic chránit naše podsítě.</span><span class="sxs-lookup"><span data-stu-id="e8f71-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall to protect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="e8f71-162">Teď můžete nasadit virtuální počítač uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="e8f71-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="e8f71-163">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="e8f71-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="e8f71-164">Následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí adaptér vNic s názvem `myNic` z předchozího kroku:</span><span class="sxs-lookup"><span data-stu-id="e8f71-164">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="e8f71-165">Pomocí rozhraní příkazového řádku příznaky vyvolávající existující prostředky jsme vyzvat Azure k nasazení virtuálních počítačů uvnitř existující síť.</span><span class="sxs-lookup"><span data-stu-id="e8f71-165">By using the CLI flags to call out existing resources, we instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="e8f71-166">Chcete-li zopakovat, jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f71-166">To reiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="e8f71-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8f71-167">Next steps</span></span>
* <span data-ttu-id="e8f71-168">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8f71-168">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* [<span data-ttu-id="e8f71-169">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="e8f71-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
