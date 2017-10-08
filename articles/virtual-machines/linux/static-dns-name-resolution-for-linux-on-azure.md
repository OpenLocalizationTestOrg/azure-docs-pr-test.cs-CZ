---
title: "aaaUse interní DNS pro virtuální počítač překlad s hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak virtuální sítě toocreate kartami a používat interní DNS pro překlad názvů virtuálních počítačů v Azure s hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="27e7c-103">Vytvořit virtuální síťové karty a používat interní DNS pro překlad názvů virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="27e7c-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="27e7c-104">Tento článek ukazuje, jak tooset statické interní názvy DNS pro virtuální počítače s Linuxem pomocí virtuální síťové adaptéry (vNics) a názvy DNS popisek s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="27e7c-104">This article shows you how tooset static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with hello Azure CLI 2.0.</span></span> <span data-ttu-id="27e7c-105">Můžete také provést tyto kroky hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27e7c-105">You can also perform these steps with hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="27e7c-106">Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.</span><span class="sxs-lookup"><span data-stu-id="27e7c-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="27e7c-107">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="27e7c-107">hello requirements are:</span></span>

* [<span data-ttu-id="27e7c-108">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="27e7c-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="27e7c-109">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="27e7c-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="27e7c-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="27e7c-110">Quick commands</span></span>
<span data-ttu-id="27e7c-111">Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="27e7c-111">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="27e7c-112">Podrobnější informace a kontext pro každý krok lze nalézt v hello zbytek dokumentu hello [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="27e7c-112">More detailed information and context for each step can be found in hello rest of hello document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="27e7c-113">Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="27e7c-113">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="27e7c-114">Předběžných požadavků: Skupinu prostředků, virtuální síť a podsíť, skupinu zabezpečení sítě pomocí protokolu SSH příchozí.</span><span class="sxs-lookup"><span data-stu-id="27e7c-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="27e7c-115">Vytvořit virtuální síťový adaptér se statickou interní název DNS</span><span class="sxs-lookup"><span data-stu-id="27e7c-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="27e7c-116">Vytvoření hello vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-116">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="27e7c-117">Hello `--internal-dns-name` rozhraní příkazového řádku příznak je pro popisek DNS hello nastavení, která poskytuje hello statické název DNS pro virtuální síťová karta hello (vNic).</span><span class="sxs-lookup"><span data-stu-id="27e7c-117">hello `--internal-dns-name` CLI flag is for setting hello DNS label, which provides hello static DNS name for hello virtual network interface card (vNic).</span></span> <span data-ttu-id="27e7c-118">Hello následující příklad vytvoří adaptéru vNic s názvem `myNic`, ho připojí toohello `myVnet` virtuální síti a vytvoří interní záznam název DNS názvem `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-118">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a><span data-ttu-id="27e7c-119">Nasadit virtuální počítač a připojte hello vNic</span><span class="sxs-lookup"><span data-stu-id="27e7c-119">Deploy a VM and connect hello vNic</span></span>
<span data-ttu-id="27e7c-120">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="27e7c-121">Hello `--nics` příznak během nasazení tooAzure hello připojuje hello vNic toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="27e7c-121">hello `--nics` flag connects hello vNic toohello VM during hello deployment tooAzure.</span></span> <span data-ttu-id="27e7c-122">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí hello vNic s názvem `myNic` z hello předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="27e7c-122">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="27e7c-123">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="27e7c-123">Detailed walkthrough</span></span>

<span data-ttu-id="27e7c-124">Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určité toobe statickou nebo dlohotrvající servery infrastruktury v Azure.</span><span class="sxs-lookup"><span data-stu-id="27e7c-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span> <span data-ttu-id="27e7c-125">Doporučujeme, aby Azure prostředky jako hello virtuální sítě a skupiny zabezpečení sítě jsou statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="27e7c-125">It is recommended that Azure assets like hello virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="27e7c-126">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="27e7c-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="27e7c-127">Později můžete přidat server úložiště Git nebo automatizační server volaných přináší CiCd toothis virtuální sítě pro vaše prostředí pro vývoj nebo testování.</span><span class="sxs-lookup"><span data-stu-id="27e7c-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd toothis virtual network for your development or test environments.</span></span>  

<span data-ttu-id="27e7c-128">Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="27e7c-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="27e7c-129">Protože se jedná o interní hello názvy DNS, nejsou přeložit toohello mimo Internetu, poskytuje dodatečné zabezpečení toohello infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="27e7c-129">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="27e7c-130">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="27e7c-130">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="27e7c-131">Zahrnout názvy parametrů příklad `myResourceGroup`, `myNic`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="27e7c-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="27e7c-132">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="27e7c-132">Create hello resource group</span></span>
<span data-ttu-id="27e7c-133">Nejprve vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-133">First, create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="27e7c-134">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:</span><span class="sxs-lookup"><span data-stu-id="27e7c-134">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="27e7c-135">Vytvoření virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="27e7c-135">Create hello virtual network</span></span>

<span data-ttu-id="27e7c-136">dalším krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="27e7c-136">hello next step is toobuild a virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="27e7c-137">virtuální síť Hello obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="27e7c-137">hello virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="27e7c-138">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27e7c-138">For more information on Azure virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="27e7c-139">Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-139">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="27e7c-140">Hello následující příklad vytvoří virtuální síť s názvem `myVnet` a podsíť s názvem `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-140">hello following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="27e7c-141">Vytvoření hello skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="27e7c-141">Create hello Network Security Group</span></span>
<span data-ttu-id="27e7c-142">Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="27e7c-142">Azure Network Security Groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="27e7c-143">Další informace o skupinách zabezpečení sítě najdete v tématu [jak toocreate skupin Nsg v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27e7c-143">For more information about Network Security Groups, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="27e7c-144">Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-144">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="27e7c-145">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-145">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a><span data-ttu-id="27e7c-146">Přidat příchozí pravidlo tooallow SSH</span><span class="sxs-lookup"><span data-stu-id="27e7c-146">Add an inbound rule tooallow SSH</span></span>
<span data-ttu-id="27e7c-147">Přidat příchozí pravidlo pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-147">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="27e7c-148">Hello následující příklad vytvoří pravidlo s názvem `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-148">hello following example creates a rule named `myRuleAllowSSH`:</span></span>

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

## <a name="associate-hello-subnet-with-hello-network-security-group"></a><span data-ttu-id="27e7c-149">Hello podsíť přidružit hello skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="27e7c-149">Associate hello subnet with hello Network Security Group</span></span>
<span data-ttu-id="27e7c-150">tooassociate hello podsíť s hello skupinu zabezpečení sítě používat [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="27e7c-150">tooassociate hello subnet with hello Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="27e7c-151">Hello následující příklad přiřadí název podsítě hello `mySubnet` s hello skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-151">hello following example associates hello subnet name `mySubnet` with hello Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="27e7c-152">Vytvořte virtuální síťová karta hello a statické názvy DNS</span><span class="sxs-lookup"><span data-stu-id="27e7c-152">Create hello virtual network interface card and static DNS names</span></span>
<span data-ttu-id="27e7c-153">Azure je velmi flexibilní, ale toouse názvy DNS pro rozlišení názvu virtuálního počítače, je nutné toocreate virtuální síťové karty (vNics) zahrnující název DNS.</span><span class="sxs-lookup"><span data-stu-id="27e7c-153">Azure is very flexible, but toouse DNS names for VM name resolution, you need toocreate virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="27e7c-154">vNics jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů přes hello infrastruktury životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="27e7c-154">vNics are important as you can reuse them by connecting them toodifferent VMs over hello infrastructure lifecycle.</span></span> <span data-ttu-id="27e7c-155">Tento přístup zajišťuje hello vNic jako statické prostředek hello virtuální počítače mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="27e7c-155">This approach keeps hello vNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="27e7c-156">Pomocí DNS označování na kartě vNic hello jsme možné tooenable jednoduchý název řešení z jiných virtuálních počítačů v hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="27e7c-156">By using DNS labeling on hello vNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span> <span data-ttu-id="27e7c-157">Automatizační server hello tooaccess jiné virtuální počítače pomocí přeložit názvy umožňuje podle názvu DNS hello `Jenkins` nebo server hello Git jako `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="27e7c-157">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  

<span data-ttu-id="27e7c-158">Vytvoření hello vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-158">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="27e7c-159">Hello následující příklad vytvoří adaptéru vNic s názvem `myNic`, ho připojí toohello `myVnet` virtuální síť s názvem `myVnet`a vytvoří interní záznam název DNS názvem `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="27e7c-159">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="27e7c-160">Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="27e7c-160">Deploy hello VM into hello virtual network infrastructure</span></span>
<span data-ttu-id="27e7c-161">Máme teď virtuální síť a podsíť, funguje jako brána firewall tooprotect naše podsíť blokováním veškerý příchozí provoz, s výjimkou port 22 pro SSH a adaptéru vNic skupina zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="27e7c-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="27e7c-162">Teď můžete nasadit virtuální počítač uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="27e7c-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="27e7c-163">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="27e7c-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="27e7c-164">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí hello vNic s názvem `myNic` z hello předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="27e7c-164">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="27e7c-165">Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, jsme pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě.</span><span class="sxs-lookup"><span data-stu-id="27e7c-165">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="27e7c-166">tooreiterate, jakmile nasazených virtuálních sítí a podsítí, mohou být levá jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="27e7c-166">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="27e7c-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27e7c-167">Next steps</span></span>
* <span data-ttu-id="27e7c-168">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27e7c-168">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* [<span data-ttu-id="27e7c-169">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="27e7c-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
