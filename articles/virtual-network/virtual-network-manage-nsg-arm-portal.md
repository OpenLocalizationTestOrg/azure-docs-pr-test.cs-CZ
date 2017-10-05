---
title: "Správa skupin Nsg pomocí portálu Azure | Microsoft Docs"
description: "Naučte se spravovat existující skupiny Nsg pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-nsgs-using-the-portal"></a><span data-ttu-id="3572e-103">Správa skupin Nsg pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="3572e-103">Manage NSGs using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3572e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3572e-104">Portal</span></span>](virtual-network-manage-nsg-arm-portal.md)
> * [<span data-ttu-id="3572e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3572e-105">PowerShell</span></span>](virtual-network-manage-nsg-arm-ps.md)
> * [<span data-ttu-id="3572e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3572e-106">Azure CLI</span></span>](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="3572e-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3572e-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3572e-108">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="3572e-108">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="3572e-109">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="3572e-109">Retrieve Information</span></span>
<span data-ttu-id="3572e-110">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="3572e-110">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="3572e-111">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="3572e-111">View existing NSGs</span></span>

<span data-ttu-id="3572e-112">Chcete-li zobrazit všechny existující skupiny Nsg v předplatném, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-112">To view all existing NSGs in a subscription, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-113">V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="3572e-113">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>

2. <span data-ttu-id="3572e-114">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="3572e-114">Click **Browse >** > **Network security groups**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. <span data-ttu-id="3572e-116">Zkontrolujte seznam skupin Nsg v **skupin zabezpečení sítě** okno.</span><span class="sxs-lookup"><span data-stu-id="3572e-116">Check the list of NSGs in the **Network security groups** blade.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a><span data-ttu-id="3572e-118">Zobrazení skupiny Nsg ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="3572e-118">View NSGs in a resource group</span></span>

<span data-ttu-id="3572e-119">Chcete-li zobrazit seznam skupin Nsg v **RG NSG** prostředků skupiny, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-119">To view the list of NSGs in the **RG-NSG** resource group, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-120">Klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...** .</span><span class="sxs-lookup"><span data-stu-id="3572e-120">Click **Resource groups >** > **RG-NSG** > **...**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. <span data-ttu-id="3572e-122">V seznamu prostředků, vyhledejte položky zobrazení na ikonu NSG, jak je znázorněno v **prostředky** okno níže.</span><span class="sxs-lookup"><span data-stu-id="3572e-122">In the list of resources, look for items displaying the NSG icon, as shown in the **Resources** blade below.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="3572e-124">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="3572e-124">List all rules for an NSG</span></span>

<span data-ttu-id="3572e-125">Chcete-li zobrazit pravidla s názvem skupiny NSG **NSG front-endu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-125">To view the rules of an NSG named **NSG-FrontEnd**, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-126">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-126">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="3572e-127">V **nastavení** , klikněte na **příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3572e-127">In the **Settings** tab, click **Inbound security rules**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. <span data-ttu-id="3572e-129">**Příchozí pravidla zabezpečení** jak je uvedeno níže, zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="3572e-129">The **Inbound security rules** blade is displayed as shown below.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. <span data-ttu-id="3572e-131">V **nastavení** , klikněte na **odchozí pravidla zabezpečení** zobrazíte odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="3572e-131">In the **Settings** tab, click **Outbound security rules** to see the outbound rules.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3572e-132">Chcete-li zobrazit výchozí pravidla, klikněte na tlačítko **výchozí pravidla** ikonu v horní části okna, která zobrazuje pravidla.</span><span class="sxs-lookup"><span data-stu-id="3572e-132">To view default rules, click the **Default rules** icon at the top of the blade that displays the rules.</span></span>
    >

### <a name="view-nsgs-associations"></a><span data-ttu-id="3572e-133">Zobrazte přidružení skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="3572e-133">View NSGs associations</span></span>

<span data-ttu-id="3572e-134">Chcete-li zobrazit prostředky **NSG front-endu** NSG je spojený s, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-134">To view what resources the **NSG-FrontEnd** NSG is associate with, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-135">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-135">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="3572e-136">V **nastavení** , klikněte na **podsítě** Chcete-li zobrazit, jaké podsítě jsou přidružené k této skupině.</span><span class="sxs-lookup"><span data-stu-id="3572e-136">In the **Settings** tab, click **Subnets** to view what subnets are associated to the NSG.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. <span data-ttu-id="3572e-138">V **nastavení** , klikněte na **síťových rozhraní** Chcete-li zobrazit, co jsou přidružené k této skupině síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="3572e-138">In the **Settings** tab, click **Network interfaces** to view what NICs are associated to the NSG.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="3572e-139">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="3572e-139">Manage rules</span></span>
<span data-ttu-id="3572e-140">Můžete přidat pravidla do existující skupiny NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="3572e-140">You can add rules to an existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="3572e-141">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="3572e-141">Add a rule</span></span>
<span data-ttu-id="3572e-142">Chcete-li přidat pravidlo, které povoluje **příchozí** přenosy na portu **443** z libovolného počítače k **NSG front-endu** NSG, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-142">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-143">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-143">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="3572e-144">V **nastavení** , klikněte na **příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3572e-144">In the **Settings** tab, click **Inbound security rules**.</span></span>
3. <span data-ttu-id="3572e-145">V **příchozí pravidla zabezpečení** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3572e-145">In the **Inbound security rules** blade, click **Add**.</span></span> <span data-ttu-id="3572e-146">Pak na **přidat příchozí pravidlo zabezpečení** okno, zadejte hodnoty, jak je uvedeno níže a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3572e-146">Then, in the **Add inbound security rule** blade, fill the values as shown below, and then click **OK**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    <span data-ttu-id="3572e-148">Za několik sekund, Všimněte si nové pravidlo v **příchozí pravidla zabezpečení** okno.</span><span class="sxs-lookup"><span data-stu-id="3572e-148">After a few seconds, notice the new rule in the **Inbound security rules** blade.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a><span data-ttu-id="3572e-150">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="3572e-150">Change a rule</span></span>
<span data-ttu-id="3572e-151">Chcete-li změnit pravidlo vytvořili výše, které pokud chcete povolit příchozí přenosy z **Internet** pouze, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-151">To change the rule created above to allow inbound traffic from the **Internet** only, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-152">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-152">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="3572e-153">V **nastavení** , klikněte na pravidlo vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3572e-153">In the **Settings** tab, click the rule created above.</span></span>
3. <span data-ttu-id="3572e-154">V **povolit https** okně změnu **zdroj** vlastnost, jak je uvedeno níže a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3572e-154">In the **allow-https** blade, change the **Source** property as shown below, and then click **Save**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a><span data-ttu-id="3572e-156">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="3572e-156">Delete a rule</span></span>

<span data-ttu-id="3572e-157">Pokud chcete odstranit pravidlo vytvořili výše, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-157">To delete the rule created above, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-158">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-158">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="3572e-159">V **nastavení** , klikněte na pravidlo vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3572e-159">In the **Settings** tab, click the rule created above.</span></span>
3. <span data-ttu-id="3572e-160">V **povolit https** okně klikněte na tlačítko **odstranit**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3572e-160">In the **allow-https** blade, click **Delete**, and then click **Yes**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a><span data-ttu-id="3572e-162">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="3572e-162">Manage associations</span></span>
<span data-ttu-id="3572e-163">Můžete přidružit skupiny NSG k podsítí a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="3572e-163">You can associate an NSG to subnets and NICs.</span></span> <span data-ttu-id="3572e-164">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="3572e-164">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="3572e-165">Přidružení skupiny NSG k síťové karty</span><span class="sxs-lookup"><span data-stu-id="3572e-165">Associate an NSG to a NIC</span></span>
<span data-ttu-id="3572e-166">Pro přidružení **NSG front-endu** NSG k **TestNICWeb1** síťovou kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-166">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-167">Z **skupin zabezpečení sítě** okno, nebo **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-167">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="3572e-168">V **nastavení** , klikněte na **síťových rozhraní** > **přidružit** > **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="3572e-168">In the **Settings** tab, click **Network interfaces** > **Associate** > **TestNICWeb1**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="3572e-170">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="3572e-170">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="3572e-171">Zrušení přidružení **NSG front-endu** NSG z **TestNICWeb1** síťovou kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-171">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-172">Z portálu Azure klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="3572e-172">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestNICWeb1**.</span></span>

2. <span data-ttu-id="3572e-173">V **TestNICWeb1** okně klikněte na tlačítko **změnit zabezpečení...**   >  **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="3572e-173">In the **TestNICWeb1** blade, click **Change security...** > **None**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> <span data-ttu-id="3572e-175">Toto okno můžete taky přidružit síťovou kartu k žádné existující skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="3572e-175">You can also use this blade to associate the NIC to any existing NSG.</span></span>
>

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="3572e-176">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="3572e-176">Dissociate an NSG from a subnet</span></span>

<span data-ttu-id="3572e-177">Zrušení přidružení **NSG front-endu** NSG z **front-endu** podsíť, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-177">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-178">Z portálu Azure klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="3572e-178">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>

2. <span data-ttu-id="3572e-179">V **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="3572e-179">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **None**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. <span data-ttu-id="3572e-181">V **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3572e-181">In the **FrontEnd** blade, click **Save**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="3572e-183">Přidružení skupiny NSG k podsíti</span><span class="sxs-lookup"><span data-stu-id="3572e-183">Associate an NSG to a subnet</span></span>

<span data-ttu-id="3572e-184">Pro přidružení **NSG front-endu** NSG k **FronEnd** podsíť znovu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3572e-184">To associate the **NSG-FrontEnd** NSG to the **FronEnd** subnet again, complete the following steps:</span></span>

1. <span data-ttu-id="3572e-185">Z portálu Azure klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="3572e-185">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>
2. <span data-ttu-id="3572e-186">V **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě** > **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-186">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
3. <span data-ttu-id="3572e-187">V **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3572e-187">In the **FrontEnd** blade, click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="3572e-188">Můžete také přidružíte skupinu NSG k podsíti z thh NSG na **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="3572e-188">You can also associate an NSG to a subnet from thh NSG's **Settings** blade.</span></span>
>

## <a name="delete-an-nsg"></a><span data-ttu-id="3572e-189">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="3572e-189">Delete an NSG</span></span>
<span data-ttu-id="3572e-190">Skupinu NSG můžete odstranit, pouze pokud má není přidružen k žádnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="3572e-190">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="3572e-191">Chcete-li odstranit skupinu NSG, proveďte následující kroky:.</span><span class="sxs-lookup"><span data-stu-id="3572e-191">To delete an NSG, complete the following steps:.</span></span>

1. <span data-ttu-id="3572e-192">Z portálu Azure klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="3572e-192">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="3572e-193">V **nastavení** okně klikněte na tlačítko **síťových rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="3572e-193">In the **Settings** blade, click **Network interfaces**.</span></span>
3. <span data-ttu-id="3572e-194">Pokud nejsou žádné síťové adaptéry uvedené, klikněte na síťový adaptér a postupujte podle kroku 2 v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC).</span><span class="sxs-lookup"><span data-stu-id="3572e-194">If there are any NICs listed, click the NIC, and follow step 2 in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC).</span></span>
4. <span data-ttu-id="3572e-195">Opakujte krok 3 pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3572e-195">Repeat step 3 for each NIC.</span></span>
5. <span data-ttu-id="3572e-196">V **nastavení** okně klikněte na tlačítko **podsítě**.</span><span class="sxs-lookup"><span data-stu-id="3572e-196">In the **Settings** blade, click **Subnets**.</span></span>
6. <span data-ttu-id="3572e-197">Pokud neexistují žádné podsítě uvedena, klikněte na podsíť a postupujte podle kroků 2 a 3 v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet).</span><span class="sxs-lookup"><span data-stu-id="3572e-197">If there are any subnets listed, click the subnet and follow steps 2 and 3 in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet).</span></span>
7. <span data-ttu-id="3572e-198">Posune zleva **NSG front-endu** okno, pak klikněte na tlačítko **odstranit** > **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3572e-198">Scrolls left to the **NSG-FrontEnd** blade, then click **Delete** > **Yes**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a><span data-ttu-id="3572e-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3572e-200">Next steps</span></span>
* <span data-ttu-id="3572e-201">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="3572e-201">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>
