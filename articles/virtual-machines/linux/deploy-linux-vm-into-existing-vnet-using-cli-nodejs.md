---
title: "aaaDeploy virtuální počítače s Linuxem do existující síť s Azure CLI 1.0 | Microsoft Docs"
description: "Jak hello toodeploy virtuálního počítače s Linuxem do existující virtuální sítě pomocí Azure CLI 1.0"
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
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a><span data-ttu-id="6cf3d-103">Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6cf3d-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI 1.0</span></span>

<span data-ttu-id="6cf3d-104">Tento článek ukazuje, jak toouse Azure CLI 1.0 toodeploy virtuální počítač (VM) do existující virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="6cf3d-104">This article shows you how toouse Azure CLI 1.0 toodeploy a virtual machine (VM) into an existing Virtual Network (VNet).</span></span> <span data-ttu-id="6cf3d-105">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="6cf3d-105">hello requirements are:</span></span>

- [<span data-ttu-id="6cf3d-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="6cf3d-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="6cf3d-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="6cf3d-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="6cf3d-108">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="6cf3d-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="6cf3d-109">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6cf3d-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="6cf3d-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="6cf3d-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6cf3d-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="6cf3d-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="6cf3d-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="6cf3d-112">Quick Commands</span></span>

<span data-ttu-id="6cf3d-113">Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-113">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="6cf3d-114">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="6cf3d-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span></span>

<span data-ttu-id="6cf3d-115">Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-115">Pre-requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span> <span data-ttu-id="6cf3d-116">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-116">Replace any examples with your own settings.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="6cf3d-117">Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="6cf3d-117">Deploy hello VM into hello virtual network infrastructure</span></span>

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

## <a name="detailed-walkthrough"></a><span data-ttu-id="6cf3d-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="6cf3d-118">Detailed walkthrough</span></span>

<span data-ttu-id="6cf3d-119">Prostředky Azure, například hello virtuálních sítí a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-119">Azure assets like hello VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="6cf3d-120">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-120">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="6cf3d-121">Vezměte v úvahu, že je tradiční hardwaru síťový přepínač virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-121">Think about a VNet as being a traditional hardware network switch.</span></span> <span data-ttu-id="6cf3d-122">Nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-122">You would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="6cf3d-123">Pomocí správně nakonfigurovaných virtuální síť můžete pokračovat toodeploy nových serverů do ní opakovaně v několika, pokud existuje, změny během hello doby trvání hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-123">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="6cf3d-124">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="6cf3d-124">Create hello resource group</span></span>

<span data-ttu-id="6cf3d-125">Nejprve vytvořte tooorganize skupiny prostředků všechny objekty, které vytvoříte v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-125">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="6cf3d-126">Další informace o skupinách prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="6cf3d-126">For more information about resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="6cf3d-127">Vytvoření hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf3d-127">Create hello VNet</span></span>

<span data-ttu-id="6cf3d-128">prvním krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-128">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="6cf3d-129">Hello virtuální síť obsahuje jednu podsíť pro účely tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-129">hello VNet contains one subnet for this walkthrough.</span></span> <span data-ttu-id="6cf3d-130">Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6cf3d-130">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span></span>

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="6cf3d-131">Vytvořit skupinu zabezpečení sítě hello</span><span class="sxs-lookup"><span data-stu-id="6cf3d-131">Create hello network security group</span></span>

<span data-ttu-id="6cf3d-132">podsíť Hello vychází za existující skupinu zabezpečení sítě, vytvořit skupinu zabezpečení sítě hello před hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-132">hello subnet is built behind an existing network security group so build hello network security group before hello subnet.</span></span> <span data-ttu-id="6cf3d-133">Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-133">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="6cf3d-134">Další informace o skupinách zabezpečení sítě Azure, najdete v části [jak skupiny zabezpečení sítě toocreate ve hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6cf3d-134">For more information on Azure network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span></span>

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="6cf3d-135">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="6cf3d-135">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="6cf3d-136">Hello virtuálního počítače potřebuje přístup z hello je nutná internet, pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-136">hello VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span>

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

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="6cf3d-137">Přidat toohello podsíť virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf3d-137">Add a subnet toohello VNet</span></span>

<span data-ttu-id="6cf3d-138">Virtuální počítače v rámci hello virtuální síť musí být umístěny v podsíti.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-138">VMs within hello VNet must be located in a subnet.</span></span> <span data-ttu-id="6cf3d-139">Každý virtuální sítě může mít několik podsítí.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-139">Each VNet can have multiple subnets.</span></span> <span data-ttu-id="6cf3d-140">Vytvořte podsíť hello a přidružte skupinu zabezpečení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-140">Create hello subnet and associate with hello network security group.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

<span data-ttu-id="6cf3d-141">Hello podsíť je nyní přidána uvnitř hello virtuální sítě a přidružené skupiny zabezpečení sítě hello a pravidla.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-141">hello Subnet is now added inside hello VNet and associated with hello network security group and rule.</span></span>


## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="6cf3d-142">Přidat podsíť toohello VNic</span><span class="sxs-lookup"><span data-stu-id="6cf3d-142">Add a VNic toohello subnet</span></span>

<span data-ttu-id="6cf3d-143">Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-143">Virtual network cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="6cf3d-144">Tento přístup zajišťuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-144">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="6cf3d-145">Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořená v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-145">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="6cf3d-146">Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="6cf3d-146">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="6cf3d-147">Nyní máte virtuální síť a podsíť uvnitř této virtuální sítě a skupinu zabezpečení sítě může tooprotect hello podsítě ve blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-147">You now have a VNet and subnet inside that VNet, and a network security group acting tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="6cf3d-148">Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-148">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="6cf3d-149">Pomocí rozhraní příkazového řádku Azure hello a hello `azure vm create` příkaz, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-149">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span> <span data-ttu-id="6cf3d-150">Další informace o používání rozhraní příkazového řádku toodeploy hello dokončení virtuálních počítačů najdete v tématu [vytvořit úplný prostředí Linux pomocí hello rozhraní příkazového řádku Azure](create-cli-complete.md)</span><span class="sxs-lookup"><span data-stu-id="6cf3d-150">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md)</span></span>

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

<span data-ttu-id="6cf3d-151">Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, dáte pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-151">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="6cf3d-152">Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6cf3d-152">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="6cf3d-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cf3d-153">Next steps</span></span>

* [<span data-ttu-id="6cf3d-154">Použijte toocreate šablony Azure Resource Manager konkrétní nasazení</span><span class="sxs-lookup"><span data-stu-id="6cf3d-154">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="6cf3d-155">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="6cf3d-155">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="6cf3d-156">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="6cf3d-156">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
