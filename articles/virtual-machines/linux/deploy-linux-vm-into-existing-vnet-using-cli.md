---
title: "aaaDeploy virtuální počítače s Linuxem do stávající sítě s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toodeploy virtuální počítač s Linuxem do existující virtuální sítě pomocí Azure CLI 2.0"
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
ms.openlocfilehash: 0df44b3437002df050db56f3b3899083fb49d803
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a><span data-ttu-id="2677f-103">Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2677f-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI</span></span>

<span data-ttu-id="2677f-104">Tento článek ukazuje, jak toouse hello Azure CLI 2.0 toodeploy virtuální počítač (VM) do existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="2677f-104">This article shows you how toouse hello Azure CLI 2.0 toodeploy a virtual machine (VM) into an existing virtual network.</span></span> <span data-ttu-id="2677f-105">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="2677f-105">hello requirements are:</span></span>

- [<span data-ttu-id="2677f-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="2677f-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="2677f-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="2677f-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

<span data-ttu-id="2677f-108">Můžete také provést tyto kroky hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-108">You can also perform these steps with hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="2677f-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="2677f-109">Quick Commands</span></span>
<span data-ttu-id="2677f-110">Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné.</span><span class="sxs-lookup"><span data-stu-id="2677f-110">If you need tooquickly accomplish hello task, hello following section details hello  commands needed.</span></span> <span data-ttu-id="2677f-111">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="2677f-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>

<span data-ttu-id="2677f-112">toocreate tento vlastní prostředí budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="2677f-112">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="2677f-113">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2677f-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="2677f-114">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="2677f-114">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

<span data-ttu-id="2677f-115">**Předběžných požadavků:** skupina prostředků Azure, virtuální síť a podsíť, příchozí skupinu zabezpečení sítě pomocí protokolu SSH a virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="2677f-115">**Pre-requirements:** Azure resource group, virtual network and subnet, network security group with SSH inbound, and a virtual network interface card.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="2677f-116">Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="2677f-116">Deploy hello VM into hello virtual network infrastructure</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="2677f-117">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="2677f-117">Detailed walkthrough</span></span>

<span data-ttu-id="2677f-118">Prostředky Azure jako virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="2677f-118">Azure assets like virtual networks and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="2677f-119">Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="2677f-119">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="2677f-120">Vezměte v úvahu virtuální sítě, že síťový přepínač tradiční hardwaru – nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="2677f-120">Think about a virtual network as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="2677f-121">Správně nakonfigurována virtuální síť, můžete pokračovat toodeploy nové virtuální počítače do této virtuální sítě s několika opakovaně, pokud existuje, nevyžadují změny během hello doby trvání hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2677f-121">With a correctly configured virtual network, you can continue toodeploy new VMs into that virtual network over and over with few, if any, changes required over hello life of hello virtual network.</span></span>

<span data-ttu-id="2677f-122">toocreate tento vlastní prostředí budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="2677f-122">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="2677f-123">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2677f-123">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="2677f-124">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="2677f-124">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="2677f-125">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="2677f-125">Create hello resource group</span></span>

<span data-ttu-id="2677f-126">Nejprve vytvořte tooorganize skupiny prostředků Azure. všechny objekty, které vytvoříte v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="2677f-126">First, create an Azure resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="2677f-127">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-127">For more information on resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="2677f-128">Vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-128">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="2677f-129">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="2677f-129">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="2677f-130">Vytvoření virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="2677f-130">Create hello virtual network</span></span>

<span data-ttu-id="2677f-131">Umožňuje vytvářet virtuální počítače do toolaunch hello virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="2677f-131">Lets build an Azure virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="2677f-132">Další informace o virtuálních sítích najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-132">For more information on virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span></span> <span data-ttu-id="2677f-133">Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-133">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="2677f-134">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="2677f-134">hello following example creates a virtual network named *myVnet* and subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="2677f-135">Vytvořit skupinu zabezpečení sítě hello</span><span class="sxs-lookup"><span data-stu-id="2677f-135">Create hello network security group</span></span>

<span data-ttu-id="2677f-136">Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="2677f-136">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="2677f-137">Další informace o skupinách zabezpečení sítě najdete v tématu [jak skupiny zabezpečení sítě toocreate ve hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-137">For more information on network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span> <span data-ttu-id="2677f-138">Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-138">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="2677f-139">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="2677f-139">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="2677f-140">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="2677f-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="2677f-141">Hello virtuálního počítače potřebuje přístup z hello je nutná Internetu, aby pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2677f-141">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span> <span data-ttu-id="2677f-142">Přidat příchozí pravidlo pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-142">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="2677f-143">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="2677f-143">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a><span data-ttu-id="2677f-144">Připojit skupinu zabezpečení sítě toohello podsítí hello</span><span class="sxs-lookup"><span data-stu-id="2677f-144">Attach hello subnet toohello network security group</span></span>

<span data-ttu-id="2677f-145">Hello pravidel skupiny zabezpečení sítě můžou být použité tooa podsíť nebo konkrétní virtuální síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2677f-145">hello network security group rules can be applied tooa subnet or a specific virtual network interface.</span></span> <span data-ttu-id="2677f-146">Umožňuje připojit hello sítě zabezpečení skupiny tooour podsítě.</span><span class="sxs-lookup"><span data-stu-id="2677f-146">Lets attach hello network security group tooour subnet.</span></span> <span data-ttu-id="2677f-147">Připojit vaše podsíť toohello skupina zabezpečení sítě se [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update):</span><span class="sxs-lookup"><span data-stu-id="2677f-147">Attach your subnet toohello network security group with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update):</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a><span data-ttu-id="2677f-148">Přidat podsíť virtuální sítě rozhraní karty toohello</span><span class="sxs-lookup"><span data-stu-id="2677f-148">Add a virtual network interface card toohello subnet</span></span>

<span data-ttu-id="2677f-149">Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2677f-149">Virtual network interface cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="2677f-150">Tato opakované použití vám umožní tookeep hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="2677f-150">This reuse allows you tookeep hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="2677f-151">Vytvoření adaptéru VNic a přidružte ji k podsíti hello s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-151">Create a VNic and associate it with hello subnet with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="2677f-152">Hello následující příklad vytvoří adaptéru VNic s názvem *myNic*:</span><span class="sxs-lookup"><span data-stu-id="2677f-152">hello following example creates a VNic named *myNic*:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="2677f-153">Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="2677f-153">Deploy hello VM into hello virtual network infrastructure</span></span>

<span data-ttu-id="2677f-154">Nyní máte virtuální síť a podsíť a podsíť hello tooprotect skupiny zabezpečení sítě pomocí blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH.</span><span class="sxs-lookup"><span data-stu-id="2677f-154">You now have a virtual network and subnet, and a network security group tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="2677f-155">Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="2677f-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="2677f-156">Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="2677f-156">Create your VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="2677f-157">Další informace o hello flags toouse s hello Azure CLI 2.0 toodeploy dokončení virtuálních počítačů, najdete v části [vytvořit úplný prostředí Linux pomocí příkazového řádku Azure CLI hello](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-157">For more information on hello flags toouse with hello Azure CLI 2.0 toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md).</span></span>

<span data-ttu-id="2677f-158">Hello následující ukázka vytvoří virtuální počítač pomocí Azure spravované disků.</span><span class="sxs-lookup"><span data-stu-id="2677f-158">hello following example creates a VM using Azure Managed Disks.</span></span> <span data-ttu-id="2677f-159">Tyto disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je.</span><span class="sxs-lookup"><span data-stu-id="2677f-159">These disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="2677f-160">Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../../storage/storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-160">For more information about managed disks, see [Azure Managed Disks overview](../../storage/storage-managed-disks-overview.md).</span></span> <span data-ttu-id="2677f-161">Pokud chcete toouse nespravované disky, najdete v části hello Další poznámka níže.</span><span class="sxs-lookup"><span data-stu-id="2677f-161">If you wish toouse unmanaged disks, see hello additional note below.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

<span data-ttu-id="2677f-162">Pokud používáte spravovaných disků, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="2677f-162">If you use managed disks, skip this step.</span></span> <span data-ttu-id="2677f-163">Pokud chcete disky toouse nespravované, potřebujete následující další parametry toohello pokračováním příkaz toocreate nespravované disky v hello účet úložiště s názvem hello tooadd `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="2677f-163">If you wish toouse unmanaged disks, you need tooadd hello following additional parameters toohello proceeding command toocreate unmanaged disks in hello storage account named `mystorageaccount`:</span></span> 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

<span data-ttu-id="2677f-164">Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, dáte pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě.</span><span class="sxs-lookup"><span data-stu-id="2677f-164">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="2677f-165">Jakmile jsou nasazené virtuální síť a podsíť, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="2677f-165">Once a virtual network and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span> <span data-ttu-id="2677f-166">V tomto příkladu není vytvořte a přiřaďte toohello veřejnou adresu IP VNic, takže tento virtuální počítač není veřejně přístupné přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2677f-166">In this example, you did not create and assign a public IP address toohello VNic, so this VM is not publicly accessible over hello Internet.</span></span> <span data-ttu-id="2677f-167">Další informace najdete v tématu [vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-167">For more information, see [Create a VM with a static public IP using hello Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2677f-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2677f-168">Next steps</span></span>
<span data-ttu-id="2677f-169">Další informace o způsobech toocreate virtuálních počítačů v Azure najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="2677f-169">For more information about ways toocreate virtual machines in Azure, see hello following resources:</span></span>

* [<span data-ttu-id="2677f-170">Použijte toocreate šablony Azure Resource Manager konkrétní nasazení</span><span class="sxs-lookup"><span data-stu-id="2677f-170">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="2677f-171">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="2677f-171">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="2677f-172">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="2677f-172">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
