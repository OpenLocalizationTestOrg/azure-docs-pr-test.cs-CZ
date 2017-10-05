---
title: "Nasadit virtuální počítače s Linuxem do stávající sítě pomocí portálu Azure | Microsoft Docs"
description: "Nasazení virtuálního počítače s Linuxem do existující virtuální síť Azure pomocí portálu."
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
ms.openlocfilehash: 964c0fc41773b50a9fbe476df47460484c2ada66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-portal"></a><span data-ttu-id="72a66-103">Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="72a66-103">How to deploy a Linux virtual machine into an existing Azure Virtual Network with the Azure portal</span></span>

<span data-ttu-id="72a66-104">Tento článek ukazuje, jak nasadit virtuální počítač (VM) do existující virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="72a66-104">This article shows you how to deploy a virtual machine (VM) into an existing virtual network (VNet).</span></span> <span data-ttu-id="72a66-105">Azure prostředky jako skupin zabezpečení virtuální sítě a sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka.</span><span class="sxs-lookup"><span data-stu-id="72a66-105">Azure assets like VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="72a66-106">Po nasazený virtuální síť, můžete použít znovu konstantní redeployments bez žádné negativní ovlivňuje infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="72a66-106">Once a VNet has been deployed, it can be reused by constant redeployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="72a66-107">Přemýšlíte o virtuální sítě, že je tradiční hardwaru síťový přepínač - nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení.</span><span class="sxs-lookup"><span data-stu-id="72a66-107">Thinking about a VNet as being a traditional hardware network switch - you would not need to configure a brand new hardware switch with each deployment.</span></span>  

<span data-ttu-id="72a66-108">Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové servery do ní opakovaně s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="72a66-108">With a correctly configured VNet, you can continue to deploy new servers into that VNet over and over with few, if any, changes required over the life of the VNet.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="72a66-109">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="72a66-109">Create the resource group</span></span>

<span data-ttu-id="72a66-110">Nejprve vytvořte skupinu prostředků pro všechny objekty, které vytvoříte v tomto návodu uspořádání.</span><span class="sxs-lookup"><span data-stu-id="72a66-110">First, create a resource group to organize everything you create in this walkthrough.</span></span> <span data-ttu-id="72a66-111">Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="72a66-111">For more information about Azure resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a><span data-ttu-id="72a66-113">Vytvoření sítě VNet</span><span class="sxs-lookup"><span data-stu-id="72a66-113">Create the VNet</span></span>

<span data-ttu-id="72a66-114">V dalším kroku sestavení ke spuštění virtuálních počítačů do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="72a66-114">Next, build a VNet to launch the VMs into.</span></span> <span data-ttu-id="72a66-115">Virtuální sítě obsahuje jednu podsíť a je přidružen k této podsíti v pozdější fázi skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="72a66-115">The VNet contains one subnet and is associated with the network security group with this subnet in a later step.</span></span>

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a><span data-ttu-id="72a66-117">Přidání adaptéru VNic k podsíti</span><span class="sxs-lookup"><span data-stu-id="72a66-117">Add a VNic to the subnet</span></span>

<span data-ttu-id="72a66-118">Virtuální síťové karty (VNics) jsou důležité, protože je můžete připojit k jiné virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="72a66-118">Virtual network cards (VNics) are important as you can connect them to different VMs.</span></span> <span data-ttu-id="72a66-119">Tento přístup zajišťuje VNic jako statické prostředek při virtuálních počítačů může být dočasné.</span><span class="sxs-lookup"><span data-stu-id="72a66-119">This approach keeps the VNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="72a66-120">Vytvoření adaptéru VNic a přidružte ji k podsíť vytvořená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="72a66-120">Create a VNic and associate it with the subnet created in the previous step.</span></span>

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-network-security-group"></a><span data-ttu-id="72a66-122">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="72a66-122">Create the network security group</span></span>

<span data-ttu-id="72a66-123">Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="72a66-123">Azure network security groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="72a66-124">Další informace o skupinách zabezpečení sítě Azure, najdete v části [co je skupina zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="72a66-124">For more information on Azure network security groups, see [What is a Network Security Group](../../virtual-network/virtual-networks-nsg.md).</span></span>

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="72a66-126">Přidat pravidlo povolit příchozí SSH</span><span class="sxs-lookup"><span data-stu-id="72a66-126">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="72a66-127">Virtuální počítač potřebuje přístup z Internetu, takže se vytvoří pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="72a66-127">The VM needs access from the internet, so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the VM is created.</span></span>

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a><span data-ttu-id="72a66-129">Přidružení NSG k podsíti</span><span class="sxs-lookup"><span data-stu-id="72a66-129">Associate the NSG with the subnet</span></span>

<span data-ttu-id="72a66-130">Síť VNet a podsíť vytvořená přidružte skupinu zabezpečení sítě s podsítí.</span><span class="sxs-lookup"><span data-stu-id="72a66-130">With the VNet and the subnet created, associate the network security group with the subnet.</span></span> <span data-ttu-id="72a66-131">Skupiny zabezpečení sítě lze přidružit celou podsíť nebo jednotlivé VNic.</span><span class="sxs-lookup"><span data-stu-id="72a66-131">Network security groups can be associated with either an entire subnet or an individual VNic.</span></span> <span data-ttu-id="72a66-132">Brána firewall, filtrování přenosů dat na úrovni podsítě všechny VNics a virtuální počítače v rámci podsítě jsou chráněny skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="72a66-132">With the firewall filtering traffic at the subnet level, all VNics and the VMs within the subnet are protected by the network security group.</span></span> <span data-ttu-id="72a66-133">Ostatní přístup je skupina zabezpečení sítě bylo možné přidružit jen jedné adaptéru VNic a ochranu právě jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="72a66-133">The other approach is the network security group being associated with just a single VNic and protecting just one VM.</span></span>

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a><span data-ttu-id="72a66-135">Virtuální počítač nasaďte do virtuální sítě a NSG</span><span class="sxs-lookup"><span data-stu-id="72a66-135">Deploy the VM into the VNet and NSG</span></span>

<span data-ttu-id="72a66-136">Pomocí portálu Azure, virtuálního počítače s Linuxem se nasadí do existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.</span><span class="sxs-lookup"><span data-stu-id="72a66-136">Using the Azure portal, the Linux VM is deployed to the existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

<span data-ttu-id="72a66-138">Zvolit existující prostředky prostřednictvím portálu, požádejte Azure k nasazení virtuálních počítačů uvnitř existující síť.</span><span class="sxs-lookup"><span data-stu-id="72a66-138">By using the portal to choose existing resources, you instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="72a66-139">Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="72a66-139">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="72a66-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72a66-140">Next steps</span></span>

* [<span data-ttu-id="72a66-141">Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="72a66-141">Use an Azure Resource Manager template to create a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* <span data-ttu-id="72a66-142">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="72a66-142">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md)</span></span>
* [<span data-ttu-id="72a66-143">Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="72a66-143">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
