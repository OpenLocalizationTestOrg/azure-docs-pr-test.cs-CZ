---
title: "aaaDeploy virtuální počítače s Linuxem do stávající sítě pomocí portálu Azure | Microsoft Docs"
description: "Nasazení virtuálního počítače s Linuxem do existující virtuální síť Azure pomocí portálu hello."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a><span data-ttu-id="d8e93-103">Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d8e93-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure portal</span></span>

<span data-ttu-id="d8e93-104">Tento článek ukazuje, jak toodeploy virtuální počítač (VM) do existující virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="d8e93-104">This article shows you how toodeploy a virtual machine (VM) into an existing virtual network (VNet).</span></span> <span data-ttu-id="d8e93-105">Azure prostředky jako skupin zabezpečení virtuální sítě a sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="d8e93-105">Azure assets like VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="d8e93-106">Po nasazený virtuální síť, můžete použít znovu konstantní redeployments bez jakékoli infrastruktury toohello má negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="d8e93-106">Once a VNet has been deployed, it can be reused by constant redeployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="d8e93-107">Přemýšlíte o virtuální sítě, že je tradiční hardwaru síťový přepínač - nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="d8e93-107">Thinking about a VNet as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span>  

<span data-ttu-id="d8e93-108">Pomocí správně nakonfigurovaných virtuální síť můžete pokračovat toodeploy nových serverů do ní opakovaně v několika, pokud existuje, změny během hello doby trvání hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d8e93-108">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="d8e93-109">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="d8e93-109">Create hello resource group</span></span>

<span data-ttu-id="d8e93-110">Nejprve vytvořte tooorganize skupiny prostředků všechny objekty, které vytvoříte v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="d8e93-110">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="d8e93-111">Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d8e93-111">For more information about Azure resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a><span data-ttu-id="d8e93-113">Vytvoření hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d8e93-113">Create hello VNet</span></span>

<span data-ttu-id="d8e93-114">V dalším kroku sestavení hello toolaunch virtuální sítě virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="d8e93-114">Next, build a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="d8e93-115">Hello virtuální síť obsahuje jednu podsíť a je přidružená ke skupině zabezpečení sítě hello k této podsíti v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="d8e93-115">hello VNet contains one subnet and is associated with hello network security group with this subnet in a later step.</span></span>

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="d8e93-117">Přidat podsíť toohello VNic</span><span class="sxs-lookup"><span data-stu-id="d8e93-117">Add a VNic toohello subnet</span></span>

<span data-ttu-id="d8e93-118">Virtuální síťové karty (VNics) jsou důležité, jako je můžete připojit toodifferent virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d8e93-118">Virtual network cards (VNics) are important as you can connect them toodifferent VMs.</span></span> <span data-ttu-id="d8e93-119">Tento přístup zajišťuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné.</span><span class="sxs-lookup"><span data-stu-id="d8e93-119">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="d8e93-120">Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořená v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="d8e93-120">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a><span data-ttu-id="d8e93-122">Vytvořit skupinu zabezpečení sítě hello</span><span class="sxs-lookup"><span data-stu-id="d8e93-122">Create hello network security group</span></span>

<span data-ttu-id="d8e93-123">Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="d8e93-123">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="d8e93-124">Další informace o skupinách zabezpečení sítě Azure, najdete v části [co je skupina zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="d8e93-124">For more information on Azure network security groups, see [What is a Network Security Group](../../virtual-network/virtual-networks-nsg.md).</span></span>

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="d8e93-126">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="d8e93-126">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="d8e93-127">Hello virtuálního počítače potřebuje přístup z hello Internetu, aby pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d8e93-127">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is created.</span></span>

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a><span data-ttu-id="d8e93-129">Přidružit hello NSG k podsíti hello</span><span class="sxs-lookup"><span data-stu-id="d8e93-129">Associate hello NSG with hello subnet</span></span>

<span data-ttu-id="d8e93-130">Hello virtuální síť a podsíť hello vytvořená přidružte skupinu zabezpečení sítě hello hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="d8e93-130">With hello VNet and hello subnet created, associate hello network security group with hello subnet.</span></span> <span data-ttu-id="d8e93-131">Skupiny zabezpečení sítě lze přidružit celou podsíť nebo jednotlivé VNic.</span><span class="sxs-lookup"><span data-stu-id="d8e93-131">Network security groups can be associated with either an entire subnet or an individual VNic.</span></span> <span data-ttu-id="d8e93-132">Brána firewall hello filtrování přenosů dat na úrovni podsítě hello všechny VNics a hello virtuálních počítačů v rámci podsítě hello jsou chráněny hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d8e93-132">With hello firewall filtering traffic at hello subnet level, all VNics and hello VMs within hello subnet are protected by hello network security group.</span></span> <span data-ttu-id="d8e93-133">Hello jiných přístup je hello se skupina zabezpečení sítě spojené s pouze jednoho adaptéru VNic a chrání pouze jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d8e93-133">hello other approach is hello network security group being associated with just a single VNic and protecting just one VM.</span></span>

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="d8e93-135">Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="d8e93-135">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="d8e93-136">Hello virtuálního počítače s Linuxem pomocí hello portálu Azure, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.</span><span class="sxs-lookup"><span data-stu-id="d8e93-136">Using hello Azure portal, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

<span data-ttu-id="d8e93-138">Pomocí hello portálu toochoose stávajícími prostředky, dáte pokyn Azure toodeploy hello uvnitř hello stávající sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d8e93-138">By using hello portal toochoose existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="d8e93-139">Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="d8e93-139">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="d8e93-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8e93-140">Next steps</span></span>

* [<span data-ttu-id="d8e93-141">Použijte toocreate šablony Azure Resource Manager konkrétní nasazení</span><span class="sxs-lookup"><span data-stu-id="d8e93-141">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="d8e93-142">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="d8e93-142">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="d8e93-143">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="d8e93-143">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
