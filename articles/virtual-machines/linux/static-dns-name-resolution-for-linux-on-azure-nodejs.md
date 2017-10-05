---
title: "Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure | Microsoft Docs"
description: "Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure."
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
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: bfba2cf38a0624e8480a32bf153f391d820da5a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="6e0fd-103">Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="6e0fd-103">Using internal DNS for VM name resolution on Azure</span></span>

<span data-ttu-id="6e0fd-104">Tento článek ukazuje, jak nastavit statické interní názvy DNS pro virtuální počítače s Linuxem pomocí karty virtuální síťovou kartu (VNic) a štítek názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-104">This article shows how to set static internal DNS names for Linux VMs using Virtual NIC cards (VNic) and DNS label names.</span></span> <span data-ttu-id="6e0fd-105">Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-105">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="6e0fd-106">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="6e0fd-106">The requirements are:</span></span>

* [<span data-ttu-id="6e0fd-107">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="6e0fd-107">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="6e0fd-108">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="6e0fd-108">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="6e0fd-109">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="6e0fd-109">CLI versions to complete the task</span></span>
<span data-ttu-id="6e0fd-110">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6e0fd-110">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="6e0fd-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="6e0fd-111">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6e0fd-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="6e0fd-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="6e0fd-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="6e0fd-113">Quick commands</span></span>

<span data-ttu-id="6e0fd-114">Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-114">If you need to quickly accomplish the task, the following section details the commands needed.</span></span> <span data-ttu-id="6e0fd-115">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="6e0fd-115">More detailed information and context for each step can be found the rest of the document, [starting here](#detailed-walkthrough).</span></span>  

<span data-ttu-id="6e0fd-116">Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-116">Pre-Requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span>

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a><span data-ttu-id="6e0fd-117">Vytvoření adaptéru VNic pomocí statické interní název DNS</span><span class="sxs-lookup"><span data-stu-id="6e0fd-117">Create a VNic with a static internal DNS name</span></span>

<span data-ttu-id="6e0fd-118">`-r` Příznak rozhraní příkazového řádku je k nastavení DNS popisku, který poskytuje statické název DNS adaptér VNic.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-118">The `-r` cli flag is for setting the DNS label, which provides the static DNS name for the VNic.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-the-vm-into-the-vnet-nsg-and-connect-the-vnic"></a><span data-ttu-id="6e0fd-119">Virtuální počítač nasaďte do sítě VNet, NSG a připojte adaptér VNic</span><span class="sxs-lookup"><span data-stu-id="6e0fd-119">Deploy the VM into the VNet, NSG and, connect the VNic</span></span>

<span data-ttu-id="6e0fd-120">`-N` Připojí VNic do nového virtuálního počítače během nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-120">The `-N` connects the VNic to the new VM during the deployment to Azure.</span></span>

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="6e0fd-121">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="6e0fd-121">Detailed walkthrough</span></span>

<span data-ttu-id="6e0fd-122">Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určitých serverech být statické nebo dlohotrvající servery infrastruktury v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-122">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers to be static or long-lived servers.</span></span>  <span data-ttu-id="6e0fd-123">Doporučujeme, aby Azure prostředky jako virtuální sítě (virtuální sítě) a skupiny zabezpečení sítě (Nsg), by měla být statická a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-123">It is recommended that Azure assets like the Virtual Networks (VNets) and Network Security Groups (NSGs), should be static and long lived resources that are rarely deployed.</span></span>  <span data-ttu-id="6e0fd-124">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-124">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span>  <span data-ttu-id="6e0fd-125">Přidání k této síti statické server úložiště Git a automatizační server volaných přináší CiCd do vašeho prostředí pro vývoj nebo testování.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-125">Adding to this static network a Git repository server and a Jenkins automation server delivers CiCd to your development or test environments.</span></span>  

<span data-ttu-id="6e0fd-126">Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-126">Internal DNS names are only resolvable inside an Azure virtual network.</span></span>  <span data-ttu-id="6e0fd-127">Protože se jedná o interní názvy DNS, nejsou přeložit na mimo Internetu, poskytuje dodatečné zabezpečení infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-127">Because the DNS names are internal, they are not resolvable to the outside internet, providing additional security to the infrastructure.</span></span>

<span data-ttu-id="6e0fd-128">_Nahradí všechny příklady vlastní názvy._</span><span class="sxs-lookup"><span data-stu-id="6e0fd-128">_Replace any examples with your own naming._</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="6e0fd-129">Vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="6e0fd-129">Create the Resource group</span></span>

<span data-ttu-id="6e0fd-130">K uspořádání vše, co se nám vytvořit v tomto návodu je nutné skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-130">A Resource Group is needed to organize everything we create in this walkthrough.</span></span>  <span data-ttu-id="6e0fd-131">Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6e0fd-131">For more information on Azure Resource Groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-the-vnet"></a><span data-ttu-id="6e0fd-132">Vytvoření sítě VNet</span><span class="sxs-lookup"><span data-stu-id="6e0fd-132">Create the VNet</span></span>

<span data-ttu-id="6e0fd-133">Prvním krokem je vytvoření virtuální sítě ke spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-133">The first step is to build a VNet to launch the VMs into.</span></span>  <span data-ttu-id="6e0fd-134">Virtuální sítě obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-134">The VNet contains one subnet for this walkthrough.</span></span>  <span data-ttu-id="6e0fd-135">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6e0fd-135">For more information on Azure VNets, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-the-nsg"></a><span data-ttu-id="6e0fd-136">Vytvoření NSG</span><span class="sxs-lookup"><span data-stu-id="6e0fd-136">Create the NSG</span></span>

<span data-ttu-id="6e0fd-137">Podsíť je postavený za existující skupinu zabezpečení sítě, takže jsme sestavení NSG před podsíť.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-137">The Subnet is built behind an existing Network Security Group so we build the NSG before the Subnet.</span></span>  <span data-ttu-id="6e0fd-138">Brána firewall síťové vrstvy odpovídají Azure skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-138">Azure NSGs are equivalent to a firewall at the network layer.</span></span>  <span data-ttu-id="6e0fd-139">Další informace o Azure Nsg najdete v tématu [vytvoření skupin Nsg v rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6e0fd-139">For more information on Azure NSGs, see [How to create NSGs in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="6e0fd-140">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="6e0fd-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="6e0fd-141">Virtuální počítač s Linuxem potřebuje přístup z Internetu, aby bylo pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-141">The Linux VM needs access from the internet so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the Linux VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a><span data-ttu-id="6e0fd-142">Přidat podsíť k virtuální síti</span><span class="sxs-lookup"><span data-stu-id="6e0fd-142">Add a subnet to the VNet</span></span>

<span data-ttu-id="6e0fd-143">Virtuální počítače v rámci virtuální sítě musí být umístěny v podsíti.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-143">VMs within the VNet must be located in a subnet.</span></span>  <span data-ttu-id="6e0fd-144">Každý virtuální sítě může mít několik podsítí.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-144">Each VNet can have multiple subnets.</span></span>  <span data-ttu-id="6e0fd-145">Vytvořte podsíť a podsíť přidružit NSG, chcete-li přidat bránu firewall k podsíti.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-145">Create the subnet and associate the subnet with the NSG to add a firewall to the subnet.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

<span data-ttu-id="6e0fd-146">Podsíť je nyní přidána uvnitř virtuální sítě a přidružené k této skupině a pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-146">The Subnet is now added inside the VNet and associated with the NSG and the NSG rule.</span></span>

## <a name="creating-static-dns-names"></a><span data-ttu-id="6e0fd-147">Vytvoření statické názvy DNS</span><span class="sxs-lookup"><span data-stu-id="6e0fd-147">Creating static DNS names</span></span>

<span data-ttu-id="6e0fd-148">Azure je velmi flexibilní, ale pokud chcete použít názvy DNS pro překlad názvů virtuálních počítačů, musíte je vytvořit jako virtuální síťové karty (VNics) pomocí označování DNS.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-148">Azure is very flexible, but to use DNS names for VMs name resolution, you need to create them as Virtual network cards (VNics) using DNS labeling.</span></span>  <span data-ttu-id="6e0fd-149">VNics jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů, zůstanou adaptéru VNic jako statické prostředek při virtuálních počítačů může být dočasné.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-149">VNics are important as you can reuse them by connecting them to different VMs, which keeps the VNic as a static resource while the VMs can be temporary.</span></span>  <span data-ttu-id="6e0fd-150">Pomocí DNS označování na kartě VNic jsou povolit jednoduchý překladu názvů z jiných virtuálních počítačů ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-150">By using DNS labeling on the VNic, we are able to enable simple name resolution from other VMs in the VNet.</span></span>  <span data-ttu-id="6e0fd-151">Použití přeložit názvy umožňuje ostatním virtuálním počítačům přístup k serveru automatizace podle názvu DNS `Jenkins` nebo server Git jako `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-151">Using resolvable names enables other VMs to access the automation server by the DNS name `Jenkins` or the Git server as `gitrepo`.</span></span>  <span data-ttu-id="6e0fd-152">Vytvoření adaptéru VNic a přidružte ji k podsíť vytvořená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-152">Create a VNic and associate it with the Subnet created in the previous step.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a><span data-ttu-id="6e0fd-153">Virtuální počítač nasaďte do virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="6e0fd-153">Deploy the VM into the VNet and NSG</span></span>

<span data-ttu-id="6e0fd-154">Nyní je k dispozici virtuální síť, podsíť uvnitř této virtuální sítě a skupinu NSG, který funguje jako brána firewall blokuje veškerý příchozí provoz, s výjimkou port 22 pro SSH chránit naše podsítě.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-154">We now have a VNet, a subnet inside that VNet, and an NSG acting as a firewall to protect our subnet by blocking all inbound traffic except port 22 for SSH.</span></span>  <span data-ttu-id="6e0fd-155">Virtuální počítač teď můžou být nasazené uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-155">The VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="6e0fd-156">Použití Azure CLI a `azure vm create` příkaz virtuálního počítače s Linuxem se nasadí do existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-156">Using the Azure CLI, and the `azure vm create` command, the Linux VM is deployed to the existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>  <span data-ttu-id="6e0fd-157">Další informace o nasazení dokončení virtuálního počítače pomocí rozhraní příkazového řádku najdete v tématu [vytvořit úplný prostředí Linux pomocí rozhraní příkazového řádku Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6e0fd-157">For more information on using the CLI to deploy a complete VM, see [Create a complete Linux environment by using the Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

<span data-ttu-id="6e0fd-158">Pomocí rozhraní příkazového řádku příznaky vyvolávající existující prostředky jsme vyzvat Azure k nasazení virtuálních počítačů uvnitř existující síť.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-158">By using the CLI flags to call out existing resources, we instruct Azure to deploy the VM inside the existing network.</span></span>  <span data-ttu-id="6e0fd-159">Chcete-li zopakovat, jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0fd-159">To reiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="6e0fd-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e0fd-160">Next steps</span></span>
* <span data-ttu-id="6e0fd-161">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6e0fd-161">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* [<span data-ttu-id="6e0fd-162">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="6e0fd-162">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
