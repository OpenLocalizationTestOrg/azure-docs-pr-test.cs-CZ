---
title: "aaaOpen porty tooa virtuální počítač pomocí hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Windows v modelu nasazení resource manager hello hello portálu Azure"
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
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="cc3d5-103">Jak tooopen porty tooa virtuální počítač s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cc3d5-103">How tooopen ports tooa virtual machine with hello Azure portal</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="cc3d5-104">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="cc3d5-104">Quick commands</span></span>
<span data-ttu-id="cc3d5-105">Můžete také [proveďte tyto kroky, pomocí Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="cc3d5-105">You can also [perform these steps using Azure PowerShell](nsg-quickstart-powershell.md).</span></span>

<span data-ttu-id="cc3d5-106">Nejprve vytvořte skupinu zabezpečení vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-106">First, create your Network Security Group.</span></span> <span data-ttu-id="cc3d5-107">Vyberte skupinu prostředků hello portálu, zvolte **přidat**, vyhledejte a vyberte **skupinu zabezpečení sítě**:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-107">Select a resource group in hello portal, choose **Add**, then search for and select **Network security group**:</span></span>

![Přidat skupinu zabezpečení sítě](./media/nsg-quickstart-portal/add-nsg.png)

<span data-ttu-id="cc3d5-109">Zadejte název pro vaší skupinu zabezpečení sítě, vyberte nebo vytvořte skupinu prostředků a vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-109">Enter a name for your Network Security Group, select or create a resource group, and select a location.</span></span> <span data-ttu-id="cc3d5-110">Vyberte **vytvořit** po dokončení:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-110">Select **Create** when finished:</span></span>

![Vytvořit skupinu zabezpečení sítě](./media/nsg-quickstart-portal/create-nsg.png)

<span data-ttu-id="cc3d5-112">Vyberte novou skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-112">Select your new Network Security Group.</span></span> <span data-ttu-id="cc3d5-113">Vyberte 'příchozí pravidla zabezpečení, a pak vyberte hello **přidat** tlačítko toocreate pravidlo:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-113">Select 'Inbound security rules', then select hello **Add** button toocreate a rule:</span></span>

![Přidat příchozí pravidlo](./media/nsg-quickstart-portal/add-inbound-rule.png)

<span data-ttu-id="cc3d5-115">Zvolte společného **služby** z rozevírací nabídky hello, jako například *HTTP*.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-115">Choose a common **Service** from hello drop-down menu, such as *HTTP*.</span></span> <span data-ttu-id="cc3d5-116">Můžete také vybrat *vlastní* tooprovide toouse specifického portu.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-116">You can also select *Custom* tooprovide a specific port toouse.</span></span> <span data-ttu-id="cc3d5-117">V případě potřeby změňte prioritu hello nebo název.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-117">If desired, change hello priority or name.</span></span> <span data-ttu-id="cc3d5-118">Hello prioritu má vliv pořadí hello, ve kterém jsou použity pravidla - hello nižší hello číselnou hodnotu, hello starší hello pravidlo se použije.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-118">hello priority affects hello order in which rules are applied - hello lower hello numerical value, hello earlier hello rule is applied.</span></span> <span data-ttu-id="cc3d5-119">Můžete také vybrat **Upřesnit** hello horní části této obrazovky tooenter konkrétní zdroje rozsah bloku nebo portu IP, třeba.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-119">You can also select **Advanced** at hello top of this screen tooenter a specific source IP block or port range, for example.</span></span> <span data-ttu-id="cc3d5-120">Až budete připravení, vyberte **OK** toocreate hello pravidlo:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-120">When you are ready, select **OK** toocreate hello rule:</span></span>

![Vytvoření příchozího pravidla](./media/nsg-quickstart-portal/create-inbound-rule.png)

<span data-ttu-id="cc3d5-122">Posledním krokem je tooassociate skupiny zabezpečení sítě se podsíť nebo konkrétní síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-122">Your final step is tooassociate your Network Security Group with a subnet or a specific network interface.</span></span> <span data-ttu-id="cc3d5-123">Umožňuje přidružit hello skupinu zabezpečení sítě s podsítí.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-123">Let's associate hello Network Security Group with a subnet.</span></span> <span data-ttu-id="cc3d5-124">Vyberte **podsítě**, zvolte **přidružit**:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-124">Select **Subnets**, then choose **Associate**:</span></span>

![Přidružte skupinu zabezpečení sítě s podsítí](./media/nsg-quickstart-portal/associate-subnet.png)

<span data-ttu-id="cc3d5-126">Vyberte virtuální síť a pak vyberte příslušnou podsítí hello:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-126">Select your virtual network, and then select hello appropriate subnet:</span></span>

![Přidružení skupiny zabezpečení sítě pomocí virtuální sítě](./media/nsg-quickstart-portal/select-vnet-subnet.png)

<span data-ttu-id="cc3d5-128">Nyní jste vytvořili skupinu zabezpečení sítě, vytvořit vstupní pravidlo umožňující přenosy na portu 80 a spojené s podsítí.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-128">You have now created a Network Security Group, created an inbound rule that allows traffic on port 80, and associated it with a subnet.</span></span> <span data-ttu-id="cc3d5-129">Všechny virtuální počítače připojit toothat podsítě jsou dostupné na portu 80.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-129">Any VMs you connect toothat subnet are reachable on port 80.</span></span>

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="cc3d5-130">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="cc3d5-130">More information on Network Security Groups</span></span>
<span data-ttu-id="cc3d5-131">Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-131">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="cc3d5-132">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-132">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="cc3d5-133">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="cc3d5-133">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span></span>

<span data-ttu-id="cc3d5-134">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-134">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="cc3d5-135">Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-135">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="cc3d5-136">Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="cc3d5-136">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc3d5-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc3d5-137">Next steps</span></span>
<span data-ttu-id="cc3d5-138">V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo.</span><span class="sxs-lookup"><span data-stu-id="cc3d5-138">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="cc3d5-139">Můžete najít informace o vytváření podrobnější prostředí v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="cc3d5-139">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="cc3d5-140">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="cc3d5-140">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="cc3d5-141">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="cc3d5-141">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)