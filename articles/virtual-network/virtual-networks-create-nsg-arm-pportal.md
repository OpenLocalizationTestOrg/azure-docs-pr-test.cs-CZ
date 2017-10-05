---
title: "Vytvoření sítě skupiny zabezpečení - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit skupin zabezpečení sítě pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 865032f350735d35668bb199ccf1ef3f0fae81de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-portal"></a><span data-ttu-id="746f8-103">Vytvoření sítě skupiny zabezpečení pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="746f8-103">Create network security groups using the Azure portal</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="746f8-104">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="746f8-104">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="746f8-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="746f8-105">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="746f8-106">Ukázka jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="746f8-106">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="746f8-107">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.</span><span class="sxs-lookup"><span data-stu-id="746f8-107">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span> <span data-ttu-id="746f8-108">Následující použijte postup **RG NSG** jako název šablony byla nasazena do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="746f8-108">The steps below use **RG-NSG** as the name of the resource group the template was deployed to.</span></span>

## <a name="create-the-nsg-frontend-nsg"></a><span data-ttu-id="746f8-109">Vytvořit skupinu NSG NSG front-endu</span><span class="sxs-lookup"><span data-stu-id="746f8-109">Create the NSG-FrontEnd NSG</span></span>
<span data-ttu-id="746f8-110">Chcete-li vytvořit **NSG front-endu** NSG, jak je znázorněno v tomto scénáři výše, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="746f8-110">To create the **NSG-FrontEnd** NSG as shown in the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="746f8-111">V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="746f8-111">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="746f8-112">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="746f8-112">Click **Browse >** > **Network Security Groups**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. <span data-ttu-id="746f8-114">V **skupin zabezpečení sítě** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="746f8-114">In the **Network security groups** blade, click **Add**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. <span data-ttu-id="746f8-116">V **vytvořit skupinu zabezpečení sítě** okně Vytvořit skupinu NSG s názvem *NSG front-endu* v *RG NSG* skupinu prostředků a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="746f8-116">In the **Create network security group** blade, create an NSG named *NSG-FrontEnd* in the *RG-NSG* resource group, and then click **Create**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a><span data-ttu-id="746f8-118">Vytvoření pravidel v existující skupině NSG</span><span class="sxs-lookup"><span data-stu-id="746f8-118">Create rules in an existing NSG</span></span>
<span data-ttu-id="746f8-119">Při vytváření pravidla v existující skupina NSG z portálu Azure, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="746f8-119">To create rules in an existing NSG from the Azure portal, follow the steps below.</span></span>

1. <span data-ttu-id="746f8-120">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="746f8-120">Click **Browse >** > **Network security groups**.</span></span>
2. <span data-ttu-id="746f8-121">V seznamu skupin Nsg, klikněte na **NSG front-endu** > **příchozí pravidla zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="746f8-121">In the list of NSGs, click **NSG-FrontEnd** > **Inbound security rules**</span></span>
   
    ![Portál Azure – skupina NSG front-endu](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. <span data-ttu-id="746f8-123">V seznamu **příchozí pravidla zabezpečení**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="746f8-123">In the list of **Inbound security rules**, click **Add**.</span></span>
   
    ![Portál Azure – přidání pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. <span data-ttu-id="746f8-125">V **přidat příchozí pravidlo zabezpečení** okně Vytvořit pravidlo s názvem *pravidlo webové* s prioritu *200* povolením přístupu prostřednictvím *TCP* na port *80* k žádné virtuální počítače z jakéhokoli zdroje a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="746f8-125">In the **Add inbound security rule** blade, create a rule named *web-rule* with priority of *200* allowing access via *TCP* to port *80* to any VM from any source, and then click **OK**.</span></span> <span data-ttu-id="746f8-126">Všimněte si, že většinu těchto nastavení jsou již výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="746f8-126">Notice that most of these settings are default values already.</span></span>
   
    ![Portál Azure – nastavení pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. <span data-ttu-id="746f8-128">Za několik sekund zobrazí se nové pravidlo v této skupině.</span><span class="sxs-lookup"><span data-stu-id="746f8-128">After a few seconds you will see the new rule in the NSG.</span></span>
   
    ![Portál Azure – nové pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. <span data-ttu-id="746f8-130">Opakujte kroky 6 k vytvoření příchozího pravidla s názvem *rdp pravidlo* s prioritou *250* povolením přístupu prostřednictvím *TCP* na port *3389* k žádné virtuální počítače z jakéhokoli zdroje.</span><span class="sxs-lookup"><span data-stu-id="746f8-130">Repeat steps  to 6 to create an inbound rule named *rdp-rule* with a priority of *250* allowing access via *TCP* to port *3389* to any VM from any source.</span></span>

## <a name="associate-the-nsg-to-the-frontend-subnet"></a><span data-ttu-id="746f8-131">Přidružení NSG k podsíti FrontEnd</span><span class="sxs-lookup"><span data-stu-id="746f8-131">Associate the NSG to the FrontEnd subnet</span></span>
1. <span data-ttu-id="746f8-132">Klikněte na tlačítko **procházet >** > **skupiny prostředků** > **RG NSG**.</span><span class="sxs-lookup"><span data-stu-id="746f8-132">Click **Browse >** > **Resource groups** > **RG-NSG**.</span></span>
2. <span data-ttu-id="746f8-133">V **RG NSG** okně klikněte na tlačítko **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="746f8-133">In the **RG-NSG** blade, click **...** > **TestVNet**.</span></span>
   
    ![Portál Azure – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. <span data-ttu-id="746f8-135">V **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě** > **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="746f8-135">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. <span data-ttu-id="746f8-137">V **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="746f8-137">In the **FrontEnd** blade, click **Save**.</span></span>
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a><span data-ttu-id="746f8-139">Vytvořit skupinu NSG NSG back-end</span><span class="sxs-lookup"><span data-stu-id="746f8-139">Create the NSG-BackEnd NSG</span></span>
<span data-ttu-id="746f8-140">K vytvoření **NSG back-end** NSG a přidružte ji k **back-end** podsíť, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="746f8-140">To create the **NSG-BackEnd** NSG and associate it to the **BackEnd** subnet, follow the steps below.</span></span>

1. <span data-ttu-id="746f8-141">Opakujte kroky v [vytvořit NSG NSG front-endu](#Create-the-NSG-FrontEnd-NSG) vytvořit skupinu NSG s názvem *NSG back-end*</span><span class="sxs-lookup"><span data-stu-id="746f8-141">Repeat the steps in [Create the NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) to create an NSG named *NSG-BackEnd*</span></span>
2. <span data-ttu-id="746f8-142">Opakujte kroky v [vytvořit pravidla v existující skupině](#Create-rules-in-an-existing-NSG) vytvořit **příchozí** pravidla v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="746f8-142">Repeat the steps in [Create rules in an existing NSG](#Create-rules-in-an-existing-NSG) to create the **inbound** rules in the table below.</span></span>
   
   | <span data-ttu-id="746f8-143">Příchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="746f8-143">Inbound rule</span></span> | <span data-ttu-id="746f8-144">Odchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="746f8-144">Outbound rule</span></span> |
   | --- | --- |
   | ![Portál Azure – příchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Portál Azure – odchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. <span data-ttu-id="746f8-147">Opakujte kroky v [přidružení skupiny NSG k podsíti front-endu](#Associate-the-NSG-to-the-FrontEnd-subnet) přidružit **NSG back-end** NSG k **back-end** podsítě.</span><span class="sxs-lookup"><span data-stu-id="746f8-147">Repeat the steps in [Associate the NSG to the FrontEnd subnet](#Associate-the-NSG-to-the-FrontEnd-subnet) to associate the **NSG-Backend** NSG to the **BackEnd** subnet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="746f8-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="746f8-148">Next Steps</span></span>
* <span data-ttu-id="746f8-149">Zjistěte, jak [spravovat existující skupiny Nsg](virtual-network-manage-nsg-arm-portal.md)</span><span class="sxs-lookup"><span data-stu-id="746f8-149">Learn how to [manage existing NSGs](virtual-network-manage-nsg-arm-portal.md)</span></span>
* <span data-ttu-id="746f8-150">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="746f8-150">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

