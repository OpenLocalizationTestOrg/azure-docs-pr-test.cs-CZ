---
title: "Otevřete porty, které se virtuální počítač pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak otevřít port / create koncového bodu váš virtuální počítač s Windows pomocí modelu nasazení resource manager na portálu Azure"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 33bc0be0aeae6d0276fd8999b9ac0a010e3067ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="788bc-103">Jak otevřít porty, které se virtuální počítač pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="788bc-103">How to open ports to a virtual machine with the Azure portal</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="788bc-104">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="788bc-104">Quick commands</span></span>
<span data-ttu-id="788bc-105">Můžete také [proveďte tyto kroky, pomocí Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="788bc-105">You can also [perform these steps using Azure PowerShell](nsg-quickstart-powershell.md).</span></span>

<span data-ttu-id="788bc-106">Nejprve vytvořte skupinu zabezpečení vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="788bc-106">First, create your Network Security Group.</span></span> <span data-ttu-id="788bc-107">Vyberte skupinu prostředků na portálu, zvolte **přidat**, vyhledejte a vyberte **skupinu zabezpečení sítě**:</span><span class="sxs-lookup"><span data-stu-id="788bc-107">Select a resource group in the portal, choose **Add**, then search for and select **Network security group**:</span></span>

![Přidat skupinu zabezpečení sítě](./media/nsg-quickstart-portal/add-nsg.png)

<span data-ttu-id="788bc-109">Zadejte název pro vaší skupinu zabezpečení sítě, vyberte nebo vytvořte skupinu prostředků a vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="788bc-109">Enter a name for your Network Security Group, select or create a resource group, and select a location.</span></span> <span data-ttu-id="788bc-110">Vyberte **vytvořit** po dokončení:</span><span class="sxs-lookup"><span data-stu-id="788bc-110">Select **Create** when finished:</span></span>

![Vytvořit skupinu zabezpečení sítě](./media/nsg-quickstart-portal/create-nsg.png)

<span data-ttu-id="788bc-112">Vyberte novou skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="788bc-112">Select your new Network Security Group.</span></span> <span data-ttu-id="788bc-113">Vyberte 'příchozí pravidla zabezpečení, a pak vyberte **přidat** tlačítko k vytvoření pravidla:</span><span class="sxs-lookup"><span data-stu-id="788bc-113">Select 'Inbound security rules', then select the **Add** button to create a rule:</span></span>

![Přidat příchozí pravidlo](./media/nsg-quickstart-portal/add-inbound-rule.png)

<span data-ttu-id="788bc-115">Zvolte společného **služby** z rozevírací nabídky, jako například *HTTP*.</span><span class="sxs-lookup"><span data-stu-id="788bc-115">Choose a common **Service** from the drop-down menu, such as *HTTP*.</span></span> <span data-ttu-id="788bc-116">Můžete také vybrat *vlastní* zajistit konkrétní port, které se má použít.</span><span class="sxs-lookup"><span data-stu-id="788bc-116">You can also select *Custom* to provide a specific port to use.</span></span> <span data-ttu-id="788bc-117">V případě potřeby změňte prioritu nebo název.</span><span class="sxs-lookup"><span data-stu-id="788bc-117">If desired, change the priority or name.</span></span> <span data-ttu-id="788bc-118">Prioritu má vliv pořadí, ve kterém jsou použity pravidla - dolní číselnou hodnotu, dříve pravidlo se použije.</span><span class="sxs-lookup"><span data-stu-id="788bc-118">The priority affects the order in which rules are applied - the lower the numerical value, the earlier the rule is applied.</span></span> <span data-ttu-id="788bc-119">Můžete také vybrat **Upřesnit** v horní části této obrazovky, zadejte konkrétní rozsah zdrojových IP bloku nebo port, např.</span><span class="sxs-lookup"><span data-stu-id="788bc-119">You can also select **Advanced** at the top of this screen to enter a specific source IP block or port range, for example.</span></span> <span data-ttu-id="788bc-120">Až budete připravení, vyberte **OK** vytvoření pravidla:</span><span class="sxs-lookup"><span data-stu-id="788bc-120">When you are ready, select **OK** to create the rule:</span></span>

![Vytvoření příchozího pravidla](./media/nsg-quickstart-portal/create-inbound-rule.png)

<span data-ttu-id="788bc-122">Posledním krokem je vaše skupina zabezpečení sítě přidružit podsítě nebo konkrétní síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="788bc-122">Your final step is to associate your Network Security Group with a subnet or a specific network interface.</span></span> <span data-ttu-id="788bc-123">Skupina zabezpečení sítě umožňuje přidružit podsítě.</span><span class="sxs-lookup"><span data-stu-id="788bc-123">Let's associate the Network Security Group with a subnet.</span></span> <span data-ttu-id="788bc-124">Vyberte **podsítě**, zvolte **přidružit**:</span><span class="sxs-lookup"><span data-stu-id="788bc-124">Select **Subnets**, then choose **Associate**:</span></span>

![Přidružte skupinu zabezpečení sítě s podsítí](./media/nsg-quickstart-portal/associate-subnet.png)

<span data-ttu-id="788bc-126">Vyberte virtuální síť a pak vyberte příslušnou podsítí:</span><span class="sxs-lookup"><span data-stu-id="788bc-126">Select your virtual network, and then select the appropriate subnet:</span></span>

![Přidružení skupiny zabezpečení sítě pomocí virtuální sítě](./media/nsg-quickstart-portal/select-vnet-subnet.png)

<span data-ttu-id="788bc-128">Nyní jste vytvořili skupinu zabezpečení sítě, vytvořit vstupní pravidlo umožňující přenosy na portu 80 a spojené s podsítí.</span><span class="sxs-lookup"><span data-stu-id="788bc-128">You have now created a Network Security Group, created an inbound rule that allows traffic on port 80, and associated it with a subnet.</span></span> <span data-ttu-id="788bc-129">Všechny virtuální počítače, ke kterým se připojujete k této podsíti jsou dostupné na portu 80.</span><span class="sxs-lookup"><span data-stu-id="788bc-129">Any VMs you connect to that subnet are reachable on port 80.</span></span>

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="788bc-130">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="788bc-130">More information on Network Security Groups</span></span>
<span data-ttu-id="788bc-131">Rychlé příkazy umožňují zprovoznění s provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="788bc-131">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="788bc-132">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="788bc-132">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="788bc-133">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="788bc-133">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span></span>

<span data-ttu-id="788bc-134">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="788bc-134">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="788bc-135">Nástroje pro vyrovnávání zatížení distribuuje provoz do virtuálních počítačů s skupinu zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="788bc-135">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="788bc-136">Další informace najdete v tématu [jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="788bc-136">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="788bc-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="788bc-137">Next steps</span></span>
<span data-ttu-id="788bc-138">V tomto příkladu jste vytvořili jednoduché pravidlo umožňující přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="788bc-138">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="788bc-139">Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="788bc-139">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="788bc-140">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="788bc-140">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="788bc-141">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="788bc-141">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)