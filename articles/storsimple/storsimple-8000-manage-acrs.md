---
title: "Spravovat záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak používat záznamy řízení přístupu (ACRs) k určení, které hostitele může připojit k svazek v zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: 9173e34f889ce1c082b20bb382cb6ca9a03dd797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a><span data-ttu-id="660f4-103">Použít službu StorSimple Manager ke správě záznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-103">Use the StorSimple Manager service to manage access control records</span></span>

## <a name="overview"></a><span data-ttu-id="660f4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="660f4-104">Overview</span></span>
<span data-ttu-id="660f4-105">Záznamy řízení přístupu (ACRs) umožňují určit, které hostitele může připojit k svazek v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="660f4-105">Access control records (ACRs) allow you to specify which hosts can connect to a volume on the StorSimple device.</span></span> <span data-ttu-id="660f4-106">ACRs jsou nastaveny na konkrétním svazku a obsahovat kvalifikované názvy iSCSI (IQN) hostitele.</span><span class="sxs-lookup"><span data-stu-id="660f4-106">ACRs are set to a specific volume and contain the iSCSI Qualified Names (IQNs) of the hosts.</span></span> <span data-ttu-id="660f4-107">Když hostitel pokusí o připojení ke svazku, zařízení se kontroluje ACR přidružený tento svazek pro název IQN a pokud je nalezena shoda, pak připojení.</span><span class="sxs-lookup"><span data-stu-id="660f4-107">When a host tries to connect to a volume, the device checks the ACR associated with that volume for the IQN name and if there is a match, then the connection is established.</span></span> <span data-ttu-id="660f4-108">Zaznamenává řízení přístupu **konfigurace** části okně vaší služby StorSimple Manager zařízení zobrazí všechny záznamy řízení přístupu s odpovídající IQN hostitelů.</span><span class="sxs-lookup"><span data-stu-id="660f4-108">The access control records in the **Configuration** section of your StorSimple Device Manager service blade display all the access control records with the corresponding IQNs of the hosts.</span></span>

<span data-ttu-id="660f4-109">Tento kurz vysvětluje následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="660f4-109">This tutorial explains the following common ACR-related tasks:</span></span>

* <span data-ttu-id="660f4-110">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-110">Add an access control record</span></span>
* <span data-ttu-id="660f4-111">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-111">Edit an access control record</span></span>
* <span data-ttu-id="660f4-112">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-112">Delete an access control record</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="660f4-113">Při přiřazování ACR svazku, vezměte v potaz, že svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku.</span><span class="sxs-lookup"><span data-stu-id="660f4-113">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>
> * <span data-ttu-id="660f4-114">Při odstraňování ACR ze svazku, ujistěte se, že odpovídající hostitele není přístup k svazku vzhledem k tomu, že odstranění může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="660f4-114">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>

## <a name="get-the-iqn"></a><span data-ttu-id="660f4-115">Získání názvu IQN</span><span class="sxs-lookup"><span data-stu-id="660f4-115">Get the IQN</span></span>

<span data-ttu-id="660f4-116">Proveďte následující kroky k získání názvu IQN hostitele Windows se systémem Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="660f4-116">Perform the following steps to get the IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a><span data-ttu-id="660f4-117">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-117">Add an access control record</span></span>
<span data-ttu-id="660f4-118">Můžete použít **konfigurace** část v okně service Manager zařízení StorSimple přidat ACRs.</span><span class="sxs-lookup"><span data-stu-id="660f4-118">You use the **Configuration** section in the StorSimple Device Manager service blade to add ACRs.</span></span> <span data-ttu-id="660f4-119">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="660f4-119">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="660f4-120">Proveďte následující postup pro přidání ACR.</span><span class="sxs-lookup"><span data-stu-id="660f4-120">Perform the following steps to add an ACR.</span></span>

#### <a name="to-add-an-acr"></a><span data-ttu-id="660f4-121">Chcete-li přidat ACR</span><span class="sxs-lookup"><span data-stu-id="660f4-121">To add an ACR</span></span>

1. <span data-ttu-id="660f4-122">Přejděte do služby StorSimple Manager zařízení, klikněte dvakrát na název služby a pak v rámci **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="660f4-122">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="660f4-123">V **záznamy řízení přístupu** okně klikněte na tlačítko **+ přidat ACR**.</span><span class="sxs-lookup"><span data-stu-id="660f4-123">In the **Access control records** blade, click **+ Add ACR**.</span></span>

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. <span data-ttu-id="660f4-125">V **přidat ACR** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="660f4-125">In the **Add ACR** blade, do the following steps:</span></span>

    1. <span data-ttu-id="660f4-126">Zadáte název acr.</span><span class="sxs-lookup"><span data-stu-id="660f4-126">Supply a name for your ACR.</span></span>
    
    2. <span data-ttu-id="660f4-127">Zadejte název IQN hostitele vašeho systému Windows Server v části **iSCSI Initiator název IQN ()**.</span><span class="sxs-lookup"><span data-stu-id="660f4-127">Provide the IQN name of your Windows Server host under **iSCSI Initiator Name (IQN)**.</span></span>

    3. <span data-ttu-id="660f4-128">Klikněte na tlačítko **přidat** vytvořit ACR.</span><span class="sxs-lookup"><span data-stu-id="660f4-128">Click **Add** to create the ACR.</span></span>

        ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  <span data-ttu-id="660f4-130">Nově přidaný ACR se zobrazí v tabulkovém seznam ACRs.</span><span class="sxs-lookup"><span data-stu-id="660f4-130">The newly added ACR will display in the tabular listing of ACRs.</span></span>

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a><span data-ttu-id="660f4-132">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-132">Edit an access control record</span></span>
<span data-ttu-id="660f4-133">Můžete použít **konfigurace** část v okně service Manager zařízení StorSimple upravit ACRs.</span><span class="sxs-lookup"><span data-stu-id="660f4-133">You use the **Configuration** section in the StorSimple Device Manager service blade to edit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="660f4-134">Doporučujeme proto upravovat pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="660f4-134">It is recommended that you modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="660f4-135">Chcete-li upravit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.</span><span class="sxs-lookup"><span data-stu-id="660f4-135">To edit an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>

<span data-ttu-id="660f4-136">Proveďte následující kroky, chcete-li upravit ACR.</span><span class="sxs-lookup"><span data-stu-id="660f4-136">Perform the following steps to edit an ACR.</span></span>

#### <a name="to-edit-an-access-control-record"></a><span data-ttu-id="660f4-137">Chcete-li upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-137">To edit an access control record</span></span>
1.  <span data-ttu-id="660f4-138">Přejděte do služby StorSimple Manager zařízení, klikněte dvakrát na název služby a pak v rámci **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="660f4-138">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="660f4-140">V tabulkovém seznam záznamy řízení přístupu, klikněte na tlačítko a vyberte ACR, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="660f4-140">In the tabular listing of the access control records, click and select the ACR that you wish to modify.</span></span>

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr1.png)

3. <span data-ttu-id="660f4-142">V **záznam řízení přístupu upravit** okno, zadejte jiný IQN odpovídající do jiného hostitele.</span><span class="sxs-lookup"><span data-stu-id="660f4-142">In the **Edit access control record** blade, provide a different IQN corresponding to another host.</span></span>

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr2.png)

4. <span data-ttu-id="660f4-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="660f4-144">Click **Save**.</span></span> <span data-ttu-id="660f4-145">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="660f4-145">When prompted for confirmation, click **Yes**.</span></span> 

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr3.png)

5. <span data-ttu-id="660f4-147">Budete upozorněni, když se aktualizuje ACR.</span><span class="sxs-lookup"><span data-stu-id="660f4-147">You are notified when the ACR is updated.</span></span> <span data-ttu-id="660f4-148">Tabulkové se také seznam aktualizuje, aby odrážely změny.</span><span class="sxs-lookup"><span data-stu-id="660f4-148">The tabular listing also updates to reflect the change.</span></span>

   
## <a name="delete-an-access-control-record"></a><span data-ttu-id="660f4-149">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-149">Delete an access control record</span></span>
<span data-ttu-id="660f4-150">Můžete použít **konfigurace** část v okně služby StorSimple Manager zařízení odstranit ACRs.</span><span class="sxs-lookup"><span data-stu-id="660f4-150">You use the **Configuration** section in the StorSimple Device Manager service blade to delete ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="660f4-151">Lze odstranit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="660f4-151">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="660f4-152">Pokud chcete odstranit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.</span><span class="sxs-lookup"><span data-stu-id="660f4-152">To delete an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>

<span data-ttu-id="660f4-153">Proveďte následující kroky a odstraňte záznam řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="660f4-153">Perform the following steps to delete an access control record.</span></span>

#### <a name="to-delete-an-access-control-record"></a><span data-ttu-id="660f4-154">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="660f4-154">To delete an access control record</span></span>
1.  <span data-ttu-id="660f4-155">Přejděte do služby StorSimple Manager zařízení, klikněte dvakrát na název služby a pak v rámci **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="660f4-155">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="660f4-157">V tabulkovém seznam záznamy řízení přístupu, klikněte na tlačítko a vyberte ACR, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="660f4-157">In the tabular listing of the access control records, click and select the ACR that you wish to delete.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. <span data-ttu-id="660f4-159">Klikněte pravým tlačítkem a vyvolání v místní nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="660f4-159">Right-click to invoke the context menu and select **Delete**.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. <span data-ttu-id="660f4-161">Po zobrazení výzvy k potvrzení, zkontrolujte informace a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="660f4-161">When prompted for confirmation, review the information and then click **Delete**.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. <span data-ttu-id="660f4-163">Upozornění se zobrazí po dokončení odstranění.</span><span class="sxs-lookup"><span data-stu-id="660f4-163">You are notified when the deletion completes.</span></span> <span data-ttu-id="660f4-164">Tabulkový výčet je aktualizována tak, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="660f4-164">The tabular listing is updated to reflect the deletion.</span></span>

    ![Přejděte na záznamy řízení přístupu](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a><span data-ttu-id="660f4-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="660f4-166">Next steps</span></span>
* <span data-ttu-id="660f4-167">Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="660f4-167">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="660f4-168">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="660f4-168">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

