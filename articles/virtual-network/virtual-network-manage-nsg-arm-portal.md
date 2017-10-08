---
title: "skupiny Nsg aaaManage pomocí hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage existující skupiny Nsg pomocí hello portálu Azure."
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
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a><span data-ttu-id="ec40b-103">Správa skupin Nsg pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="ec40b-103">Manage NSGs using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec40b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ec40b-104">Portal</span></span>](virtual-network-manage-nsg-arm-portal.md)
> * [<span data-ttu-id="ec40b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec40b-105">PowerShell</span></span>](virtual-network-manage-nsg-arm-ps.md)
> * [<span data-ttu-id="ec40b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ec40b-106">Azure CLI</span></span>](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="ec40b-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ec40b-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ec40b-108">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="ec40b-108">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="ec40b-109">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="ec40b-109">Retrieve Information</span></span>
<span data-ttu-id="ec40b-110">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="ec40b-110">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="ec40b-111">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="ec40b-111">View existing NSGs</span></span>

<span data-ttu-id="ec40b-112">tooview všechny existující skupiny Nsg v předplatném, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-112">tooview all existing NSGs in a subscription, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-113">V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ec40b-113">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>

2. <span data-ttu-id="ec40b-114">Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-114">Click **Browse >** > **Network security groups**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. <span data-ttu-id="ec40b-116">Zkontrolujte hello seznam skupin Nsg v hello **skupin zabezpečení sítě** okno.</span><span class="sxs-lookup"><span data-stu-id="ec40b-116">Check hello list of NSGs in hello **Network security groups** blade.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a><span data-ttu-id="ec40b-118">Zobrazení skupiny Nsg ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="ec40b-118">View NSGs in a resource group</span></span>

<span data-ttu-id="ec40b-119">tooview hello seznam skupin Nsg v hello **RG NSG** skupinu prostředků, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-119">tooview hello list of NSGs in hello **RG-NSG** resource group, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-120">Klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...** .</span><span class="sxs-lookup"><span data-stu-id="ec40b-120">Click **Resource groups >** > **RG-NSG** > **...**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. <span data-ttu-id="ec40b-122">V seznamu hello prostředků, vyhledejte položky zobrazení hello NSG ikonu, jak je znázorněno v hello **prostředky** okno níže.</span><span class="sxs-lookup"><span data-stu-id="ec40b-122">In hello list of resources, look for items displaying hello NSG icon, as shown in hello **Resources** blade below.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="ec40b-124">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="ec40b-124">List all rules for an NSG</span></span>

<span data-ttu-id="ec40b-125">pravidla hello tooview skupinu NSG s názvem **NSG front-endu**, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-125">tooview hello rules of an NSG named **NSG-FrontEnd**, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-126">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-126">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="ec40b-127">V hello **nastavení** , klikněte na **příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-127">In hello **Settings** tab, click **Inbound security rules**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. <span data-ttu-id="ec40b-129">Hello **příchozí pravidla zabezpečení** jak je uvedeno níže, zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="ec40b-129">hello **Inbound security rules** blade is displayed as shown below.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. <span data-ttu-id="ec40b-131">V hello **nastavení** , klikněte na **odchozí pravidla zabezpečení** toosee hello odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="ec40b-131">In hello **Settings** tab, click **Outbound security rules** toosee hello outbound rules.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec40b-132">tooview výchozích pravidel, klikněte na tlačítko hello **výchozí pravidla** ikonu hello horní části okna hello, který zobrazuje hello pravidla.</span><span class="sxs-lookup"><span data-stu-id="ec40b-132">tooview default rules, click hello **Default rules** icon at hello top of hello blade that displays hello rules.</span></span>
    >

### <a name="view-nsgs-associations"></a><span data-ttu-id="ec40b-133">Zobrazte přidružení skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="ec40b-133">View NSGs associations</span></span>

<span data-ttu-id="ec40b-134">tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-134">tooview what resources hello **NSG-FrontEnd** NSG is associate with, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-135">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-135">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="ec40b-136">V hello **nastavení** , klikněte na **podsítě** tooview podsítě, které jsou přidružené toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="ec40b-136">In hello **Settings** tab, click **Subnets** tooview what subnets are associated toohello NSG.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. <span data-ttu-id="ec40b-138">V hello **nastavení** , klikněte na **síťových rozhraní** tooview jaké jsou síťové adaptéry přidružené toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="ec40b-138">In hello **Settings** tab, click **Network interfaces** tooview what NICs are associated toohello NSG.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="ec40b-139">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="ec40b-139">Manage rules</span></span>
<span data-ttu-id="ec40b-140">Můžete přidat pravidla tooan existující skupina NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="ec40b-140">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="ec40b-141">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="ec40b-141">Add a rule</span></span>
<span data-ttu-id="ec40b-142">pravidlo, které povoluje tooadd **příchozí** provoz tooport **443** z toohello všechny počítače **NSG front-endu** NSG, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-142">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-143">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-143">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="ec40b-144">V hello **nastavení** , klikněte na **příchozí pravidla zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-144">In hello **Settings** tab, click **Inbound security rules**.</span></span>
3. <span data-ttu-id="ec40b-145">V hello **příchozí pravidla zabezpečení** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-145">In hello **Inbound security rules** blade, click **Add**.</span></span> <span data-ttu-id="ec40b-146">Potom v hello **přidat příchozí pravidlo zabezpečení** okno, zadejte hodnoty hello, jak je uvedeno níže a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-146">Then, in hello **Add inbound security rule** blade, fill hello values as shown below, and then click **OK**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    <span data-ttu-id="ec40b-148">Za několik sekund, Všimněte si hello nové pravidlo v hello **příchozí pravidla zabezpečení** okno.</span><span class="sxs-lookup"><span data-stu-id="ec40b-148">After a few seconds, notice hello new rule in hello **Inbound security rules** blade.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a><span data-ttu-id="ec40b-150">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="ec40b-150">Change a rule</span></span>
<span data-ttu-id="ec40b-151">pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-151">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-152">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-152">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="ec40b-153">V hello **nastavení** , klikněte na pravidlo hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="ec40b-153">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="ec40b-154">V hello **povolit https** okno, změna hello **zdroj** vlastnost, jak je uvedeno níže a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-154">In hello **allow-https** blade, change hello **Source** property as shown below, and then click **Save**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a><span data-ttu-id="ec40b-156">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="ec40b-156">Delete a rule</span></span>

<span data-ttu-id="ec40b-157">toodelete hello pravidlo vytvořené výše, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-157">toodelete hello rule created above, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-158">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-158">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="ec40b-159">V hello **nastavení** , klikněte na pravidlo hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="ec40b-159">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="ec40b-160">V hello **povolit https** okně klikněte na tlačítko **odstranit**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-160">In hello **allow-https** blade, click **Delete**, and then click **Yes**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a><span data-ttu-id="ec40b-162">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="ec40b-162">Manage associations</span></span>
<span data-ttu-id="ec40b-163">Můžete přidružit toosubnets NSG a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="ec40b-163">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="ec40b-164">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="ec40b-164">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="ec40b-165">Přidružit NSG tooa síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="ec40b-165">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="ec40b-166">tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-166">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-167">Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-167">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="ec40b-168">V hello **nastavení** , klikněte na **síťových rozhraní** > **přidružit** > **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-168">In hello **Settings** tab, click **Network interfaces** > **Associate** > **TestNICWeb1**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="ec40b-170">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="ec40b-170">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="ec40b-171">toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-171">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-172">Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-172">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestNICWeb1**.</span></span>

2. <span data-ttu-id="ec40b-173">V hello **TestNICWeb1** okně klikněte na tlačítko **změnit zabezpečení...**   >  **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-173">In hello **TestNICWeb1** blade, click **Change security...** > **None**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> <span data-ttu-id="ec40b-175">Můžete také použít toto okno tooassociate hello seskupování tooany existující skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="ec40b-175">You can also use this blade tooassociate hello NIC tooany existing NSG.</span></span>
>

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="ec40b-176">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="ec40b-176">Dissociate an NSG from a subnet</span></span>

<span data-ttu-id="ec40b-177">toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-177">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-178">Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-178">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>

2. <span data-ttu-id="ec40b-179">V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-179">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **None**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. <span data-ttu-id="ec40b-181">V hello **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-181">In hello **FrontEnd** blade, click **Save**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="ec40b-183">Přidružení podsíť tooa NSG</span><span class="sxs-lookup"><span data-stu-id="ec40b-183">Associate an NSG tooa subnet</span></span>

<span data-ttu-id="ec40b-184">tooassociate hello **NSG front-endu** NSG toohello **FronEnd** znovu podsíť, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec40b-184">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="ec40b-185">Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-185">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>
2. <span data-ttu-id="ec40b-186">V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-186">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
3. <span data-ttu-id="ec40b-187">V hello **front-endu** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-187">In hello **FrontEnd** blade, click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="ec40b-188">Můžete také přidružit podsíť NSG tooa z thh NSG na **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="ec40b-188">You can also associate an NSG tooa subnet from thh NSG's **Settings** blade.</span></span>
>

## <a name="delete-an-nsg"></a><span data-ttu-id="ec40b-189">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="ec40b-189">Delete an NSG</span></span>
<span data-ttu-id="ec40b-190">Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen.</span><span class="sxs-lookup"><span data-stu-id="ec40b-190">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="ec40b-191">toodelete skupinu NSG dokončení hello následující kroky:.</span><span class="sxs-lookup"><span data-stu-id="ec40b-191">toodelete an NSG, complete hello following steps:.</span></span>

1. <span data-ttu-id="ec40b-192">Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-192">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="ec40b-193">V hello **nastavení** okně klikněte na tlačítko **síťových rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-193">In hello **Settings** blade, click **Network interfaces**.</span></span>
3. <span data-ttu-id="ec40b-194">Pokud nejsou žádné síťové adaptéry uvedené, klikněte na tlačítko hello síťový adaptér a postupujte podle kroku 2 v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC).</span><span class="sxs-lookup"><span data-stu-id="ec40b-194">If there are any NICs listed, click hello NIC, and follow step 2 in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC).</span></span>
4. <span data-ttu-id="ec40b-195">Opakujte krok 3 pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ec40b-195">Repeat step 3 for each NIC.</span></span>
5. <span data-ttu-id="ec40b-196">V hello **nastavení** okně klikněte na tlačítko **podsítě**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-196">In hello **Settings** blade, click **Subnets**.</span></span>
6. <span data-ttu-id="ec40b-197">Pokud neexistují žádné podsítě uvedena, klikněte na hello podsítě a postupujte podle kroků 2 a 3 v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet).</span><span class="sxs-lookup"><span data-stu-id="ec40b-197">If there are any subnets listed, click hello subnet and follow steps 2 and 3 in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet).</span></span>
7. <span data-ttu-id="ec40b-198">Posune levém toohello **NSG front-endu** okno, pak klikněte na tlačítko **odstranit** > **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ec40b-198">Scrolls left toohello **NSG-FrontEnd** blade, then click **Delete** > **Yes**.</span></span>

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a><span data-ttu-id="ec40b-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec40b-200">Next steps</span></span>
* <span data-ttu-id="ec40b-201">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="ec40b-201">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>
