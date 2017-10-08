---
title: "skupiny zabezpečení sítě aaaCreate - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí hello portálu Azure."
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
ms.openlocfilehash: f74ecc7db06bb69f2041aa64d7b38b63eb379a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-portal"></a><span data-ttu-id="b5724-103">Vytvoření skupiny zabezpečení pomocí portálu Azure hello sítě</span><span class="sxs-lookup"><span data-stu-id="b5724-103">Create network security groups using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="b5724-104">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="b5724-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="b5724-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b5724-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="b5724-106">Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše.</span><span class="sxs-lookup"><span data-stu-id="b5724-106">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="b5724-107">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b5724-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span> <span data-ttu-id="b5724-108">Hello kroky níže použijte **RG NSG** jako hello název šablony hello skupiny prostředků hello byl nasazen na.</span><span class="sxs-lookup"><span data-stu-id="b5724-108">hello steps below use **RG-NSG** as hello name of hello resource group hello template was deployed to.</span></span>

## <a name="create-hello-nsg-frontend-nsg"></a><span data-ttu-id="b5724-109">Vytvoření hello NSG front-endu NSG</span><span class="sxs-lookup"><span data-stu-id="b5724-109">Create hello NSG-FrontEnd NSG</span></span>
<span data-ttu-id="b5724-110">toocreate hello **NSG front-endu** NSG jak je znázorněno v hello scénáři výše, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="b5724-110">toocreate hello **NSG-FrontEnd** NSG as shown in hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="b5724-111">V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5724-111">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="b5724-112">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="b5724-112">Click **Browse >** > **Network Security Groups**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. <span data-ttu-id="b5724-114">V hello **skupin zabezpečení sítě** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b5724-114">In hello **Network security groups** blade, click **Add**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. <span data-ttu-id="b5724-116">V hello **vytvořit skupinu zabezpečení sítě** okně Vytvořit skupinu NSG s názvem *NSG front-endu* v hello *RG NSG* skupinu prostředků a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b5724-116">In hello **Create network security group** blade, create an NSG named *NSG-FrontEnd* in hello *RG-NSG* resource group, and then click **Create**.</span></span>
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a><span data-ttu-id="b5724-118">Vytvoření pravidel v existující skupině NSG</span><span class="sxs-lookup"><span data-stu-id="b5724-118">Create rules in an existing NSG</span></span>
<span data-ttu-id="b5724-119">toocreate pravidla v existující skupina NSG z hello portál Azure, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="b5724-119">toocreate rules in an existing NSG from hello Azure portal, follow hello steps below.</span></span>

1. <span data-ttu-id="b5724-120">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="b5724-120">Click **Browse >** > **Network security groups**.</span></span>
2. <span data-ttu-id="b5724-121">Hello seznamu skupin Nsg, klikněte na **NSG front-endu** > **příchozí pravidla zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="b5724-121">In hello list of NSGs, click **NSG-FrontEnd** > **Inbound security rules**</span></span>
   
    ![Portál Azure – skupina NSG front-endu](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. <span data-ttu-id="b5724-123">V seznamu hello **příchozí pravidla zabezpečení**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b5724-123">In hello list of **Inbound security rules**, click **Add**.</span></span>
   
    ![Portál Azure – přidání pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. <span data-ttu-id="b5724-125">V hello **přidat příchozí pravidlo zabezpečení** okně Vytvořit pravidlo s názvem *pravidlo webové* s prioritu *200* povolením přístupu prostřednictvím *TCP* tooport *80* tooany virtuálních počítačů z libovolného zdroje a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5724-125">In hello **Add inbound security rule** blade, create a rule named *web-rule* with priority of *200* allowing access via *TCP* tooport *80* tooany VM from any source, and then click **OK**.</span></span> <span data-ttu-id="b5724-126">Všimněte si, že většinu těchto nastavení jsou již výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b5724-126">Notice that most of these settings are default values already.</span></span>
   
    ![Portál Azure – nastavení pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. <span data-ttu-id="b5724-128">Za několik sekund zobrazí se nové pravidlo hello v hello NSG.</span><span class="sxs-lookup"><span data-stu-id="b5724-128">After a few seconds you will see hello new rule in hello NSG.</span></span>
   
    ![Portál Azure – nové pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. <span data-ttu-id="b5724-130">Opakujte kroky too6 toocreate příchozí pravidlo s názvem *rdp pravidlo* s prioritou *250* povolením přístupu prostřednictvím *TCP* tooport *3389* tooany virtuálních počítačů z jakéhokoli zdroje.</span><span class="sxs-lookup"><span data-stu-id="b5724-130">Repeat steps  too6 toocreate an inbound rule named *rdp-rule* with a priority of *250* allowing access via *TCP* tooport *3389* tooany VM from any source.</span></span>

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a><span data-ttu-id="b5724-131">Přidružení podsítě front-endu toohello NSG hello</span><span class="sxs-lookup"><span data-stu-id="b5724-131">Associate hello NSG toohello FrontEnd subnet</span></span>
1. <span data-ttu-id="b5724-132">Klikněte na tlačítko **procházet >** > **skupiny prostředků** > **RG NSG**.</span><span class="sxs-lookup"><span data-stu-id="b5724-132">Click **Browse >** > **Resource groups** > **RG-NSG**.</span></span>
2. <span data-ttu-id="b5724-133">V hello **RG NSG** okně klikněte na tlačítko **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="b5724-133">In hello **RG-NSG** blade, click **...** > **TestVNet**.</span></span>
   
    ![Portál Azure – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. <span data-ttu-id="b5724-135">V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="b5724-135">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. <span data-ttu-id="b5724-137">V hello **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b5724-137">In hello **FrontEnd** blade, click **Save**.</span></span>
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a><span data-ttu-id="b5724-139">Vytvoření hello NSG NSG back-end</span><span class="sxs-lookup"><span data-stu-id="b5724-139">Create hello NSG-BackEnd NSG</span></span>
<span data-ttu-id="b5724-140">toocreate hello **NSG back-end** NSG a přidružte ji toohello **back-end** podsíť, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="b5724-140">toocreate hello **NSG-BackEnd** NSG and associate it toohello **BackEnd** subnet, follow hello steps below.</span></span>

1. <span data-ttu-id="b5724-141">Opakování hello kroky [vytvořit hello NSG front-endu NSG](#Create-the-NSG-FrontEnd-NSG) toocreate skupinu NSG s názvem *NSG back-end*</span><span class="sxs-lookup"><span data-stu-id="b5724-141">Repeat hello steps in [Create hello NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) toocreate an NSG named *NSG-BackEnd*</span></span>
2. <span data-ttu-id="b5724-142">Opakování hello kroky [vytvořit pravidla v existující skupině](#Create-rules-in-an-existing-NSG) toocreate hello **příchozí** pravidla v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="b5724-142">Repeat hello steps in [Create rules in an existing NSG](#Create-rules-in-an-existing-NSG) toocreate hello **inbound** rules in hello table below.</span></span>
   
   | <span data-ttu-id="b5724-143">Příchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="b5724-143">Inbound rule</span></span> | <span data-ttu-id="b5724-144">Odchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="b5724-144">Outbound rule</span></span> |
   | --- | --- |
   | ![Portál Azure – příchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Portál Azure – odchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. <span data-ttu-id="b5724-147">Opakování hello kroky [přidružit podsíť FrontEnd toohello NSG hello](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG back-end** NSG toohello **back-end** podsítě.</span><span class="sxs-lookup"><span data-stu-id="b5724-147">Repeat hello steps in [Associate hello NSG toohello FrontEnd subnet](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG-Backend** NSG toohello **BackEnd** subnet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5724-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5724-148">Next Steps</span></span>
* <span data-ttu-id="b5724-149">Zjistěte, jak příliš[spravovat existující skupiny Nsg](virtual-network-manage-nsg-arm-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b5724-149">Learn how too[manage existing NSGs](virtual-network-manage-nsg-arm-portal.md)</span></span>
* <span data-ttu-id="b5724-150">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="b5724-150">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

