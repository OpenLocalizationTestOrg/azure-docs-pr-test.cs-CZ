---
title: "Spravovat záznamy řízení přístupu pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak spravovat záznamy řízení přístupu (ACRs) k určení, které hostitele může připojit k svazek v poli virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ce65aa4efba735305208f7a6d761bc2814d1b8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a><span data-ttu-id="74aee-103">Pomocí Správce zařízení StorSimple Spravovat záznamy řízení přístupu pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="74aee-103">Use StorSimple Device Manager to manage access control records for StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="74aee-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="74aee-104">Overview</span></span>

<span data-ttu-id="74aee-105">Záznamy řízení přístupu (ACRs) umožňují určit, které hostitele může připojit k svazek v poli virtuální zařízení StorSimple (také označované jako místní virtuální zařízení StorSimple).</span><span class="sxs-lookup"><span data-stu-id="74aee-105">Access control records (ACRs) allow you to specify which hosts can connect to a volume on the StorSimple Virtual Array (also known as the StorSimple on-premises virtual device).</span></span> <span data-ttu-id="74aee-106">ACRs jsou nastaveny na konkrétním svazku a obsahovat kvalifikované názvy iSCSI (IQN) hostitele.</span><span class="sxs-lookup"><span data-stu-id="74aee-106">ACRs are set to a specific volume and contain the iSCSI Qualified Names (IQNs) of the hosts.</span></span> <span data-ttu-id="74aee-107">Když hostitel pokusí o připojení ke svazku, zařízení ověří ACR přidružený tento svazek pro název IQN, a pokud je nalezena shoda, pak připojení.</span><span class="sxs-lookup"><span data-stu-id="74aee-107">When a host tries to connect to a volume, the device checks the ACR associated with that volume for the IQN name, and if there is a match, then the connection is established.</span></span> <span data-ttu-id="74aee-108">**Záznamy řízení přístupu** okno v rámci **konfigurace** části služby Správce zařízení zobrazí všechny záznamy řízení přístupu s odpovídající IQN hostitelů.</span><span class="sxs-lookup"><span data-stu-id="74aee-108">The **Access control records** blade within the **Configuration** section of your Device Manager service displays all the access control records with the corresponding IQNs of the hosts.</span></span>

![Spravovat přístup k záznamům v ovládací prvek](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

<span data-ttu-id="74aee-110">Tento kurz vysvětluje následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="74aee-110">This tutorial explains the following common ACR-related tasks:</span></span>

* <span data-ttu-id="74aee-111">Získání názvu IQN</span><span class="sxs-lookup"><span data-stu-id="74aee-111">Get the IQN</span></span>
* <span data-ttu-id="74aee-112">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="74aee-112">Add an access control record</span></span>
* <span data-ttu-id="74aee-113">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="74aee-113">Edit an access control record</span></span>
* <span data-ttu-id="74aee-114">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="74aee-114">Delete an access control record</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="74aee-115">Při přiřazování ACR svazku, vezměte v potaz, že svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku.</span><span class="sxs-lookup"><span data-stu-id="74aee-115">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>
> * <span data-ttu-id="74aee-116">Při odstraňování ACR ze svazku, ujistěte se, že odpovídající hostitele není přístup k svazku vzhledem k tomu, že odstranění může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="74aee-116">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>


## <a name="get-the-iqn"></a><span data-ttu-id="74aee-117">Získání názvu IQN</span><span class="sxs-lookup"><span data-stu-id="74aee-117">Get the IQN</span></span>

<span data-ttu-id="74aee-118">Proveďte následující kroky k získání názvu IQN hostitele Windows se systémem Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="74aee-118">Perform the following steps to get the IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a><span data-ttu-id="74aee-119">Přidat ACR</span><span class="sxs-lookup"><span data-stu-id="74aee-119">Add an ACR</span></span>

<span data-ttu-id="74aee-120">Používáte **záznamy řízení přístupu** okno v rámci **konfigurace** části služby StorSimple Manager zařízení přidat ACRs.</span><span class="sxs-lookup"><span data-stu-id="74aee-120">You use **Access control records** blade within the **Configuration** section of your StorSimple Device Manager service to add ACRs.</span></span> <span data-ttu-id="74aee-121">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="74aee-121">Typically, you associate one ACR with one volume.</span></span>

<span data-ttu-id="74aee-122">Další informace o přiřazení ACR svazku [přidat svazek](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="74aee-122">For information about associating an ACR with a volume, go to [add a volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74aee-123">Při přiřazování ACR svazku, vezměte v potaz, že svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku.</span><span class="sxs-lookup"><span data-stu-id="74aee-123">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>


<span data-ttu-id="74aee-124">Proveďte následující postup pro přidání ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-124">Perform the following steps to add an ACR.</span></span>

#### <a name="to-add-an-acr"></a><span data-ttu-id="74aee-125">Chcete-li přidat ACR</span><span class="sxs-lookup"><span data-stu-id="74aee-125">To add an ACR</span></span>

1. <span data-ttu-id="74aee-126">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a pak v rámci **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="74aee-126">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="74aee-127">V **záznamy řízení přístupu** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74aee-127">In the **Access control records** blade, click **Add**.</span></span>
3. <span data-ttu-id="74aee-128">V **přidat ACR** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="74aee-128">In the **Add ACR** blade, do the following:</span></span>
   
    1. <span data-ttu-id="74aee-129">Zadejte **Název** záznamu ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-129">Supply a **Name** for your ACR.</span></span>
    
    2. <span data-ttu-id="74aee-130">V části **název iniciátoru iSCSI**, zadejte název IQN hostitele s Windows.</span><span class="sxs-lookup"><span data-stu-id="74aee-130">Under **iSCSI Initiator Name**, provide the IQN name of your Windows host.</span></span> <span data-ttu-id="74aee-131">Získání názvu IQN hostitele vašeho systému Windows Server, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="74aee-131">To get the IQN of your Windows Server host, do the following:</span></span>
   
    3. <span data-ttu-id="74aee-132">Na hostiteli s Windows spusťte iniciátor iSCSI od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="74aee-132">Start the Microsoft iSCSI initiator on your Windows host.</span></span> <span data-ttu-id="74aee-133">V okně Vlastnosti iniciátoru iSCSI na **konfigurace** , vyberte a zkopírujte řetězec z **název iniciátoru** pole.</span><span class="sxs-lookup"><span data-stu-id="74aee-133">In the iSCSI Initiator Properties window, on the **Configuration** tab, select and copy the string from the **Initiator Name** field.</span></span>
    <span data-ttu-id="74aee-134">Vložte tento řetězec v **IQN** pole **přidat ACR** okno.</span><span class="sxs-lookup"><span data-stu-id="74aee-134">Paste this string in the **IQN** field in the **Add ACR** blade.</span></span>
   
    6. <span data-ttu-id="74aee-135">Klikněte na tlačítko **přidat** přidat ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-135">Click **Add** to add the ACR.</span></span>  
   
        ![Přidat záznamy řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. <span data-ttu-id="74aee-137">Tabulkový výčet je aktualizována tak, aby odrážela přidání.</span><span class="sxs-lookup"><span data-stu-id="74aee-137">The tabular listing is updated to reflect this addition.</span></span>

## <a name="edit-an-acr"></a><span data-ttu-id="74aee-138">Upravit ACR</span><span class="sxs-lookup"><span data-stu-id="74aee-138">Edit an ACR</span></span>

<span data-ttu-id="74aee-139">Můžete použít **záznamy řízení přístupu** okno v rámci **konfigurace** části služby Správce zařízení na portálu Azure, chcete-li upravit ACRs.</span><span class="sxs-lookup"><span data-stu-id="74aee-139">You use the **Access control records** blade within the **Configuration** section of your Device Manager service in the Azure portal to edit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="74aee-140">ACR, který je aktuálně používán, byste neměli upravovat.</span><span class="sxs-lookup"><span data-stu-id="74aee-140">You should not modify an ACR that is currently in use.</span></span> <span data-ttu-id="74aee-141">Upravit ACR přidružené k svazku, který je aktuálně používán, byste měli nejdřív vzít svazek offline.</span><span class="sxs-lookup"><span data-stu-id="74aee-141">To edit an ACR associated with a volume that is currently in use, you should first take the volume offline.</span></span>


<span data-ttu-id="74aee-142">Proveďte následující kroky, chcete-li upravit ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-142">Perform the following steps to edit an ACR.</span></span>

#### <a name="to-edit-an-acr"></a><span data-ttu-id="74aee-143">Chcete-li upravit ACR</span><span class="sxs-lookup"><span data-stu-id="74aee-143">To edit an ACR</span></span>

1. <span data-ttu-id="74aee-144">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a pak v rámci **konfigurace** části **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="74aee-144">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, **Access control records**.</span></span>
2. <span data-ttu-id="74aee-145">V **záznamy řízení přístupu** okno z tabulkové seznam záznamů, řízení přístupu, klikněte dvakrát na ACR, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="74aee-145">In the **Access control records** blade, from the tabular listing of the access control records, double-click the ACR that you wish to modify.</span></span>
3. <span data-ttu-id="74aee-146">V **upravit záznamy řízení přístupu** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="74aee-146">In the **Edit access control records** blade, do the following:</span></span>
   
    1. <span data-ttu-id="74aee-147">Zadáním názvu IQN pro ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-147">Supply the IQN for the ACR.</span></span>
   
    2. <span data-ttu-id="74aee-148">Klikněte na tlačítko **Uložit** v horní části okna uložit upravené ACR.</span><span class="sxs-lookup"><span data-stu-id="74aee-148">Click **Save** at the top of the blade to save the modified ACR.</span></span> <span data-ttu-id="74aee-149">Zobrazí následující potvrzující zpráva:</span><span class="sxs-lookup"><span data-stu-id="74aee-149">You see the following confirmation message:</span></span>
   
        ![Úpravy záznamů o řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a><span data-ttu-id="74aee-151">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="74aee-151">Delete an access control record</span></span>

<span data-ttu-id="74aee-152">Můžete použít **konfigurace** na portálu Azure odstranit ACRs.</span><span class="sxs-lookup"><span data-stu-id="74aee-152">You use the **Configuration** page in the Azure portal to delete ACRs.</span></span>

> [!NOTE]
> 
> * <span data-ttu-id="74aee-153">Nedoporučuje se mazat ACR, který je aktuálně používán.</span><span class="sxs-lookup"><span data-stu-id="74aee-153">You should not delete an ACR that is currently in use.</span></span> <span data-ttu-id="74aee-154">Pokud chcete odstranit ACR přidružené k svazku, který je aktuálně používán, byste měli nejdřív vzít svazek offline.</span><span class="sxs-lookup"><span data-stu-id="74aee-154">To delete an ACR associated with a volume that is currently in use, you should first take the volume offline.</span></span>
> * <span data-ttu-id="74aee-155">Při odstraňování ACR ze svazku, ujistěte se, že odpovídající hostitele není přístup k svazku vzhledem k tomu, že odstranění může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="74aee-155">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>


<span data-ttu-id="74aee-156">Proveďte následující kroky a odstraňte záznam řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="74aee-156">Perform the following steps to delete an access control record.</span></span>

#### <a name="to-delete-an-access-control-record"></a><span data-ttu-id="74aee-157">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="74aee-157">To delete an access control record</span></span>

1. <span data-ttu-id="74aee-158">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a pak v rámci **konfigurace** části **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="74aee-158">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, **Access control records**.</span></span>

2. <span data-ttu-id="74aee-159">V **záznamy řízení přístupu** okno z tabulkové seznam záznamů, řízení přístupu, klikněte dvakrát na ACR, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="74aee-159">In the **Access control records** blade, from the tabular listing of the access control records, double-click the ACR that you wish to delete.</span></span>

3. <span data-ttu-id="74aee-160">V okně Upravit přístup řízení záznamy klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="74aee-160">In the Edit access control records blade, click **Delete**.</span></span>
   
    ![Odstranit ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. <span data-ttu-id="74aee-162">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **odstranit** Chcete-li pokračovat v odstraňování.</span><span class="sxs-lookup"><span data-stu-id="74aee-162">When prompted for confirmation, click **Delete** to continue with the deletion.</span></span> <span data-ttu-id="74aee-163">Tabulkový výčet je aktualizována tak, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="74aee-163">The tabular listing is updated to reflect the deletion.</span></span>
   
   ![Zpráva upozornění](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a><span data-ttu-id="74aee-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74aee-165">Next steps</span></span>

* <span data-ttu-id="74aee-166">Další informace o [přidání svazků a konfigurace ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="74aee-166">Learn more about [adding volumes and configuring ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

