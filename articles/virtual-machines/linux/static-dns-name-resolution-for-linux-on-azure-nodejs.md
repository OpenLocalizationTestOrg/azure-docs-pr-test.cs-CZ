---
title: "aaaUsing interní DNS pro virtuální počítač překlad v Azure | Microsoft Docs"
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
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="f6916-103">Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="f6916-103">Using internal DNS for VM name resolution on Azure</span></span>

<span data-ttu-id="f6916-104">Tento článek ukazuje, jak tooset statické interní DNS názvů pro virtuální počítače s Linuxem pomocí karty virtuální síťovou kartu (VNic) a štítek názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="f6916-104">This article shows how tooset static internal DNS names for Linux VMs using Virtual NIC cards (VNic) and DNS label names.</span></span> <span data-ttu-id="f6916-105">Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.</span><span class="sxs-lookup"><span data-stu-id="f6916-105">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="f6916-106">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="f6916-106">hello requirements are:</span></span>

* [<span data-ttu-id="f6916-107">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="f6916-107">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="f6916-108">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="f6916-108">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="f6916-109">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f6916-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="f6916-110">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f6916-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="f6916-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="f6916-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="f6916-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="f6916-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="f6916-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="f6916-113">Quick commands</span></span>

<span data-ttu-id="f6916-114">Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="f6916-114">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="f6916-115">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="f6916-115">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>  

<span data-ttu-id="f6916-116">Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť.</span><span class="sxs-lookup"><span data-stu-id="f6916-116">Pre-Requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span>

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a><span data-ttu-id="f6916-117">Vytvoření adaptéru VNic pomocí statické interní název DNS</span><span class="sxs-lookup"><span data-stu-id="f6916-117">Create a VNic with a static internal DNS name</span></span>

<span data-ttu-id="f6916-118">Hello `-r` rozhraní příkazového řádku příznak je pro popisek DNS hello nastavení, která poskytuje hello statické název DNS pro hello VNic.</span><span class="sxs-lookup"><span data-stu-id="f6916-118">hello `-r` cli flag is for setting hello DNS label, which provides hello static DNS name for hello VNic.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a><span data-ttu-id="f6916-119">Nasazení hello virtuálních počítačů do hello sítě VNet, NSG a připojit hello VNic</span><span class="sxs-lookup"><span data-stu-id="f6916-119">Deploy hello VM into hello VNet, NSG and, connect hello VNic</span></span>

<span data-ttu-id="f6916-120">Hello `-N` hello VNic toohello připojí nový virtuální počítač během nasazení tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="f6916-120">hello `-N` connects hello VNic toohello new VM during hello deployment tooAzure.</span></span>

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

## <a name="detailed-walkthrough"></a><span data-ttu-id="f6916-121">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="f6916-121">Detailed walkthrough</span></span>

<span data-ttu-id="f6916-122">Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určité toobe statickou nebo dlohotrvající servery infrastruktury v Azure.</span><span class="sxs-lookup"><span data-stu-id="f6916-122">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span>  <span data-ttu-id="f6916-123">Doporučujeme, aby Azure prostředky jako hello virtuální sítě (virtuální sítě) a skupiny zabezpečení sítě (Nsg), by měla být statická a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="f6916-123">It is recommended that Azure assets like hello Virtual Networks (VNets) and Network Security Groups (NSGs), should be static and long lived resources that are rarely deployed.</span></span>  <span data-ttu-id="f6916-124">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="f6916-124">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span>  <span data-ttu-id="f6916-125">Přidání toothis statické sítě Git úložiště server a server automatizace volaných přináší CiCd tooyour vývoj nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="f6916-125">Adding toothis static network a Git repository server and a Jenkins automation server delivers CiCd tooyour development or test environments.</span></span>  

<span data-ttu-id="f6916-126">Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="f6916-126">Internal DNS names are only resolvable inside an Azure virtual network.</span></span>  <span data-ttu-id="f6916-127">Protože se jedná o interní hello názvy DNS, nejsou přeložit toohello mimo Internetu, poskytuje dodatečné zabezpečení toohello infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="f6916-127">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="f6916-128">_Nahradí všechny příklady vlastní názvy._</span><span class="sxs-lookup"><span data-stu-id="f6916-128">_Replace any examples with your own naming._</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="f6916-129">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="f6916-129">Create hello Resource group</span></span>

<span data-ttu-id="f6916-130">Skupina prostředků je potřebné tooorganize všechno se nám vytvořit v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="f6916-130">A Resource Group is needed tooorganize everything we create in this walkthrough.</span></span>  <span data-ttu-id="f6916-131">Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f6916-131">For more information on Azure Resource Groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="f6916-132">Vytvoření hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f6916-132">Create hello VNet</span></span>

<span data-ttu-id="f6916-133">prvním krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="f6916-133">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span>  <span data-ttu-id="f6916-134">Hello virtuální síť obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="f6916-134">hello VNet contains one subnet for this walkthrough.</span></span>  <span data-ttu-id="f6916-135">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f6916-135">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a><span data-ttu-id="f6916-136">Vytvoření hello NSG</span><span class="sxs-lookup"><span data-stu-id="f6916-136">Create hello NSG</span></span>

<span data-ttu-id="f6916-137">Hello podsíť je postavený za existující skupinu zabezpečení sítě, takže jsme sestavení hello NSG před hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="f6916-137">hello Subnet is built behind an existing Network Security Group so we build hello NSG before hello Subnet.</span></span>  <span data-ttu-id="f6916-138">Skupiny Nsg Azure jsou brány firewall ekvivalentní tooa hello síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="f6916-138">Azure NSGs are equivalent tooa firewall at hello network layer.</span></span>  <span data-ttu-id="f6916-139">Další informace o Azure Nsg najdete v tématu [jak toocreate skupin Nsg v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f6916-139">For more information on Azure NSGs, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="f6916-140">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="f6916-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="f6916-141">virtuální počítač s Linuxem Hello potřebuje přístup z hello je nutná internet, pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f6916-141">hello Linux VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello Linux VM is needed.</span></span>

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

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="f6916-142">Přidat toohello podsíť virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f6916-142">Add a subnet toohello VNet</span></span>

<span data-ttu-id="f6916-143">Virtuální počítače v rámci hello virtuální síť musí být umístěny v podsíti.</span><span class="sxs-lookup"><span data-stu-id="f6916-143">VMs within hello VNet must be located in a subnet.</span></span>  <span data-ttu-id="f6916-144">Každý virtuální sítě může mít několik podsítí.</span><span class="sxs-lookup"><span data-stu-id="f6916-144">Each VNet can have multiple subnets.</span></span>  <span data-ttu-id="f6916-145">Vytvořit hello podsíť a podsíť hello přidružit hello NSG tooadd toohello podsíť brány firewall.</span><span class="sxs-lookup"><span data-stu-id="f6916-145">Create hello subnet and associate hello subnet with hello NSG tooadd a firewall toohello subnet.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

<span data-ttu-id="f6916-146">Hello podsíť je nyní přidána uvnitř hello virtuální sítě a přidružené hello NSG a pravidla NSG hello.</span><span class="sxs-lookup"><span data-stu-id="f6916-146">hello Subnet is now added inside hello VNet and associated with hello NSG and hello NSG rule.</span></span>

## <a name="creating-static-dns-names"></a><span data-ttu-id="f6916-147">Vytvoření statické názvy DNS</span><span class="sxs-lookup"><span data-stu-id="f6916-147">Creating static DNS names</span></span>

<span data-ttu-id="f6916-148">Azure je velmi flexibilní, ale toouse názvy DNS pro překlad názvů virtuálních počítačů, je nutné je jako virtuální síťové karty (VNics) pomocí DNS označování toocreate.</span><span class="sxs-lookup"><span data-stu-id="f6916-148">Azure is very flexible, but toouse DNS names for VMs name resolution, you need toocreate them as Virtual network cards (VNics) using DNS labeling.</span></span>  <span data-ttu-id="f6916-149">VNics jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů, které udržuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="f6916-149">VNics are important as you can reuse them by connecting them toodifferent VMs, which keeps hello VNic as a static resource while hello VMs can be temporary.</span></span>  <span data-ttu-id="f6916-150">Pomocí DNS označování na hello VNic jsme možné tooenable jednoduchý název řešení z jiných virtuálních počítačů v hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f6916-150">By using DNS labeling on hello VNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span>  <span data-ttu-id="f6916-151">Automatizační server hello tooaccess jiné virtuální počítače pomocí přeložit názvy umožňuje podle názvu DNS hello `Jenkins` nebo server hello Git jako `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="f6916-151">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  <span data-ttu-id="f6916-152">Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="f6916-152">Create a VNic and associate it with hello Subnet created in hello previous step.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="f6916-153">Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="f6916-153">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="f6916-154">Nyní je k dispozici virtuální síť, podsíť uvnitř této virtuální síti a funguje jako brána firewall tooprotect naše podsíť blokováním veškerý příchozí provoz, s výjimkou port 22 pro SSH skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="f6916-154">We now have a VNet, a subnet inside that VNet, and an NSG acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH.</span></span>  <span data-ttu-id="f6916-155">Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="f6916-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="f6916-156">Pomocí rozhraní příkazového řádku Azure hello a hello `azure vm create` příkaz, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f6916-156">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>  <span data-ttu-id="f6916-157">Další informace o používání rozhraní příkazového řádku toodeploy hello dokončení virtuálních počítačů najdete v tématu [vytvořit úplný prostředí Linux pomocí hello rozhraní příkazového řádku Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f6916-157">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

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

<span data-ttu-id="f6916-158">Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, jsme pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě.</span><span class="sxs-lookup"><span data-stu-id="f6916-158">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span>  <span data-ttu-id="f6916-159">tooreiterate, jakmile nasazených virtuálních sítí a podsítí, mohou být levá jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="f6916-159">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f6916-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6916-160">Next steps</span></span>
* <span data-ttu-id="f6916-161">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f6916-161">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* [<span data-ttu-id="f6916-162">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="f6916-162">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
