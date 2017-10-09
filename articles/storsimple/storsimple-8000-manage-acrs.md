---
title: "aaaManage záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toouse záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek v zařízení StorSimple hello."
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
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="11195-103">Použití služby StorSimple Manager hello, toomanage záznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-103">Use hello StorSimple Manager service toomanage access control records</span></span>

## <a name="overview"></a><span data-ttu-id="11195-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="11195-104">Overview</span></span>
<span data-ttu-id="11195-105">Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="11195-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="11195-106">ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="11195-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="11195-107">Když se hostitel pokusí tooconnect tooa svazku, zkontroluje zařízení hello hello ACR přidruženého tento svazek pro název IQN hello a pokud je nalezen, pak hello připojení.</span><span class="sxs-lookup"><span data-stu-id="11195-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="11195-108">Hello záznamy řízení přístupu v hello **konfigurace** části okně vaší služby StorSimple Manager zařízení zobrazí všechny záznamy řízení přístupu hello s hello odpovídající IQN hello hostitelů.</span><span class="sxs-lookup"><span data-stu-id="11195-108">hello access control records in hello **Configuration** section of your StorSimple Device Manager service blade display all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="11195-109">Tento kurz vysvětluje hello následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="11195-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="11195-110">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-110">Add an access control record</span></span>
* <span data-ttu-id="11195-111">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-111">Edit an access control record</span></span>
* <span data-ttu-id="11195-112">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-112">Delete an access control record</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="11195-113">Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.</span><span class="sxs-lookup"><span data-stu-id="11195-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="11195-114">Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="11195-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>

## <a name="get-hello-iqn"></a><span data-ttu-id="11195-115">Získání názvu IQN hello</span><span class="sxs-lookup"><span data-stu-id="11195-115">Get hello IQN</span></span>

<span data-ttu-id="11195-116">Proveďte následující kroky tooget hello názvu IQN hostitele se systémem Windows Server 2012 Windows hello.</span><span class="sxs-lookup"><span data-stu-id="11195-116">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a><span data-ttu-id="11195-117">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-117">Add an access control record</span></span>
<span data-ttu-id="11195-118">Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno tooadd ACRs části.</span><span class="sxs-lookup"><span data-stu-id="11195-118">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooadd ACRs.</span></span> <span data-ttu-id="11195-119">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="11195-119">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="11195-120">Proveďte následující kroky tooadd ACR hello.</span><span class="sxs-lookup"><span data-stu-id="11195-120">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="11195-121">tooadd ACR</span><span class="sxs-lookup"><span data-stu-id="11195-121">tooadd an ACR</span></span>

1. <span data-ttu-id="11195-122">Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="11195-122">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="11195-123">V hello **záznamy řízení přístupu** okně klikněte na tlačítko **+ přidat ACR**.</span><span class="sxs-lookup"><span data-stu-id="11195-123">In hello **Access control records** blade, click **+ Add ACR**.</span></span>

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. <span data-ttu-id="11195-125">V hello **přidat ACR** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="11195-125">In hello **Add ACR** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="11195-126">Zadáte název acr.</span><span class="sxs-lookup"><span data-stu-id="11195-126">Supply a name for your ACR.</span></span>
    
    2. <span data-ttu-id="11195-127">Zadejte název IQN hello hostiteli systému Windows Server v části **iSCSI Initiator název IQN ()**.</span><span class="sxs-lookup"><span data-stu-id="11195-127">Provide hello IQN name of your Windows Server host under **iSCSI Initiator Name (IQN)**.</span></span>

    3. <span data-ttu-id="11195-128">Klikněte na tlačítko **přidat** toocreate hello ACR.</span><span class="sxs-lookup"><span data-stu-id="11195-128">Click **Add** toocreate hello ACR.</span></span>

        ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  <span data-ttu-id="11195-130">Hello nově přidaní že ACR se zobrazí v tabulkovém seznam ACRs hello.</span><span class="sxs-lookup"><span data-stu-id="11195-130">hello newly added ACR will display in hello tabular listing of ACRs.</span></span>

    ![Klikněte na tlačítko Přidat ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a><span data-ttu-id="11195-132">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-132">Edit an access control record</span></span>
<span data-ttu-id="11195-133">Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno tooedit ACRs části.</span><span class="sxs-lookup"><span data-stu-id="11195-133">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="11195-134">Doporučujeme proto upravovat pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="11195-134">It is recommended that you modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="11195-135">tooedit přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="11195-135">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="11195-136">Proveďte následující kroky tooedit ACR hello.</span><span class="sxs-lookup"><span data-stu-id="11195-136">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="11195-137">tooedit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-137">tooedit an access control record</span></span>
1.  <span data-ttu-id="11195-138">Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="11195-138">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="11195-140">V hello tabulkové výpis hello záznamy řízení přístupu, klikněte na tlačítko a vyberte hello ACR chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="11195-140">In hello tabular listing of hello access control records, click and select hello ACR that you wish toomodify.</span></span>

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr1.png)

3. <span data-ttu-id="11195-142">V hello **záznam řízení přístupu upravit** okno, zadejte jiný hostitelský odpovídající tooanother IQN.</span><span class="sxs-lookup"><span data-stu-id="11195-142">In hello **Edit access control record** blade, provide a different IQN corresponding tooanother host.</span></span>

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr2.png)

4. <span data-ttu-id="11195-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11195-144">Click **Save**.</span></span> <span data-ttu-id="11195-145">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="11195-145">When prompted for confirmation, click **Yes**.</span></span> 

    ![Úpravy záznamů o řízení přístupu](./media/storsimple-8000-manage-acrs/editacr3.png)

5. <span data-ttu-id="11195-147">Budete upozorněni, když je aktualizována hello ACR.</span><span class="sxs-lookup"><span data-stu-id="11195-147">You are notified when hello ACR is updated.</span></span> <span data-ttu-id="11195-148">tabulkový výčet Hello rovněž aktualizuje tooreflect hello změnu.</span><span class="sxs-lookup"><span data-stu-id="11195-148">hello tabular listing also updates tooreflect hello change.</span></span>

   
## <a name="delete-an-access-control-record"></a><span data-ttu-id="11195-149">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-149">Delete an access control record</span></span>
<span data-ttu-id="11195-150">Použít hello **konfigurace** v hello Správce zařízení StorSimple služby okno toodelete ACRs části.</span><span class="sxs-lookup"><span data-stu-id="11195-150">You use hello **Configuration** section in hello StorSimple Device Manager service blade toodelete ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="11195-151">Lze odstranit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="11195-151">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="11195-152">toodelete přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="11195-152">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="11195-153">Proveďte následující kroky toodelete záznam řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="11195-153">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="11195-154">toodelete záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="11195-154">toodelete an access control record</span></span>
1.  <span data-ttu-id="11195-155">Přejděte tooyour service Manager zařízení StorSimple, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="11195-155">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="11195-157">V hello tabulkové výpis hello záznamy řízení přístupu, klikněte na tlačítko a vyberte hello ACR chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="11195-157">In hello tabular listing of hello access control records, click and select hello ACR that you wish toodelete.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. <span data-ttu-id="11195-159">Klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="11195-159">Right-click tooinvoke hello context menu and select **Delete**.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. <span data-ttu-id="11195-161">Po zobrazení výzvy k potvrzení, zkontrolujte hello informace a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="11195-161">When prompted for confirmation, review hello information and then click **Delete**.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. <span data-ttu-id="11195-163">Upozornění se zobrazí po dokončení odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="11195-163">You are notified when hello deletion completes.</span></span> <span data-ttu-id="11195-164">tabulkový výčet Hello je aktualizovaný tooreflect hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="11195-164">hello tabular listing is updated tooreflect hello deletion.</span></span>

    ![Přejděte tooaccess řízení záznamů](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a><span data-ttu-id="11195-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11195-166">Next steps</span></span>
* <span data-ttu-id="11195-167">Další informace o [správu svazků zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="11195-167">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="11195-168">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="11195-168">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

