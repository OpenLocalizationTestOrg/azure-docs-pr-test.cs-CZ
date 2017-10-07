---
title: "aaaManage záznamy řízení přístupu pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toomanage záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek na hello pole virtuální zařízení StorSimple."
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
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a><span data-ttu-id="06fae-103">Pomocí Správce zařízení StorSimple toomanage záznamy řízení přístupu pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="06fae-103">Use StorSimple Device Manager toomanage access control records for StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="06fae-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="06fae-104">Overview</span></span>

<span data-ttu-id="06fae-105">Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek na hello pole virtuální zařízení StorSimple (také označované jako hello místní virtuální zařízení StorSimple).</span><span class="sxs-lookup"><span data-stu-id="06fae-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device).</span></span> <span data-ttu-id="06fae-106">ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="06fae-107">Když se hostitel pokusí tooconnect tooa svazek, zařízení hello ověří hello ACR přidružený tento svazek pro název IQN hello, a pokud je nalezena shoda, pak hello připojení.</span><span class="sxs-lookup"><span data-stu-id="06fae-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name, and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="06fae-108">Hello **záznamy řízení přístupu** okno v rámci hello **konfigurace** části služby Správce zařízení zobrazí všechny záznamy řízení přístupu hello s hello odpovídající IQN hello hostitelů.</span><span class="sxs-lookup"><span data-stu-id="06fae-108">hello **Access control records** blade within hello **Configuration** section of your Device Manager service displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

![Spravovat přístup k záznamům v ovládací prvek](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

<span data-ttu-id="06fae-110">Tento kurz vysvětluje hello následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="06fae-110">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="06fae-111">Získání názvu IQN hello</span><span class="sxs-lookup"><span data-stu-id="06fae-111">Get hello IQN</span></span>
* <span data-ttu-id="06fae-112">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="06fae-112">Add an access control record</span></span>
* <span data-ttu-id="06fae-113">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="06fae-113">Edit an access control record</span></span>
* <span data-ttu-id="06fae-114">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="06fae-114">Delete an access control record</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="06fae-115">Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-115">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="06fae-116">Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="06fae-116">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


## <a name="get-hello-iqn"></a><span data-ttu-id="06fae-117">Získání názvu IQN hello</span><span class="sxs-lookup"><span data-stu-id="06fae-117">Get hello IQN</span></span>

<span data-ttu-id="06fae-118">Proveďte následující kroky tooget hello názvu IQN hostitele se systémem Windows Server 2012 Windows hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-118">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a><span data-ttu-id="06fae-119">Přidat ACR</span><span class="sxs-lookup"><span data-stu-id="06fae-119">Add an ACR</span></span>

<span data-ttu-id="06fae-120">Používáte **záznamy řízení přístupu** okno v rámci hello **konfigurace** část vaší tooadd služby StorSimple Manager zařízení ACRs.</span><span class="sxs-lookup"><span data-stu-id="06fae-120">You use **Access control records** blade within hello **Configuration** section of your StorSimple Device Manager service tooadd ACRs.</span></span> <span data-ttu-id="06fae-121">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="06fae-121">Typically, you associate one ACR with one volume.</span></span>

<span data-ttu-id="06fae-122">Informace o přiřazení ACR svazku, přejděte příliš[přidat svazek](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="06fae-122">For information about associating an ACR with a volume, go too[add a volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06fae-123">Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-123">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>


<span data-ttu-id="06fae-124">Proveďte následující kroky tooadd ACR hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-124">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="06fae-125">tooadd ACR</span><span class="sxs-lookup"><span data-stu-id="06fae-125">tooadd an ACR</span></span>

1. <span data-ttu-id="06fae-126">Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** klikněte na tlačítko **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="06fae-126">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="06fae-127">V hello **záznamy řízení přístupu** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="06fae-127">In hello **Access control records** blade, click **Add**.</span></span>
3. <span data-ttu-id="06fae-128">V hello **přidat ACR** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="06fae-128">In hello **Add ACR** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="06fae-129">Zadejte **Název** záznamu ACR.</span><span class="sxs-lookup"><span data-stu-id="06fae-129">Supply a **Name** for your ACR.</span></span>
    
    2. <span data-ttu-id="06fae-130">V části **název iniciátoru iSCSI**, zadejte název IQN hello hostitele s Windows.</span><span class="sxs-lookup"><span data-stu-id="06fae-130">Under **iSCSI Initiator Name**, provide hello IQN name of your Windows host.</span></span> <span data-ttu-id="06fae-131">hello tooget názvu IQN hostitele vašeho systému Windows Server hello následující:</span><span class="sxs-lookup"><span data-stu-id="06fae-131">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
    3. <span data-ttu-id="06fae-132">Na hostiteli s Windows spusťte iniciátor iSCSI společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-132">Start hello Microsoft iSCSI initiator on your Windows host.</span></span> <span data-ttu-id="06fae-133">V okně hello při vlastnosti iniciátoru iSCSI na hello **konfigurace** , vyberte a zkopírujte řetězec hello z hello **název iniciátoru** pole.</span><span class="sxs-lookup"><span data-stu-id="06fae-133">In hello iSCSI Initiator Properties window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
    <span data-ttu-id="06fae-134">Vložte tento řetězec hello **IQN** pole hello **přidat ACR** okno.</span><span class="sxs-lookup"><span data-stu-id="06fae-134">Paste this string in hello **IQN** field in hello **Add ACR** blade.</span></span>
   
    6. <span data-ttu-id="06fae-135">Klikněte na tlačítko **přidat** tooadd hello ACR.</span><span class="sxs-lookup"><span data-stu-id="06fae-135">Click **Add** tooadd hello ACR.</span></span>  
   
        ![Přidat záznamy řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. <span data-ttu-id="06fae-137">Hello tabulkové výpis je aktualizovaný tooreflect přidání.</span><span class="sxs-lookup"><span data-stu-id="06fae-137">hello tabular listing is updated tooreflect this addition.</span></span>

## <a name="edit-an-acr"></a><span data-ttu-id="06fae-138">Upravit ACR</span><span class="sxs-lookup"><span data-stu-id="06fae-138">Edit an ACR</span></span>

<span data-ttu-id="06fae-139">Použít hello **záznamy řízení přístupu** okno v rámci hello **konfigurace** části služby Správce zařízení v Azure portálu tooedit ACRs hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-139">You use hello **Access control records** blade within hello **Configuration** section of your Device Manager service in hello Azure portal tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="06fae-140">ACR, který je aktuálně používán, byste neměli upravovat.</span><span class="sxs-lookup"><span data-stu-id="06fae-140">You should not modify an ACR that is currently in use.</span></span> <span data-ttu-id="06fae-141">tooedit přidruženého ACR svazku, který je aktuálně používán, byste měli nejdřív vzít hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="06fae-141">tooedit an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>


<span data-ttu-id="06fae-142">Proveďte následující kroky tooedit ACR hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-142">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-acr"></a><span data-ttu-id="06fae-143">tooedit ACR</span><span class="sxs-lookup"><span data-stu-id="06fae-143">tooedit an ACR</span></span>

1. <span data-ttu-id="06fae-144">Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** části **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="06fae-144">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>
2. <span data-ttu-id="06fae-145">V hello **záznamy řízení přístupu** okno z hello tabulkové seznam hello záznamy řízení přístupu, klikněte dvakrát na hello ACR chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="06fae-145">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="06fae-146">V hello **upravit záznamy řízení přístupu** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="06fae-146">In hello **Edit access control records** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="06fae-147">Zadejte hello IQN pro hello ACR.</span><span class="sxs-lookup"><span data-stu-id="06fae-147">Supply hello IQN for hello ACR.</span></span>
   
    2. <span data-ttu-id="06fae-148">Klikněte na tlačítko **Uložit** hello horní části okna toosave hello hello upravit ACR.</span><span class="sxs-lookup"><span data-stu-id="06fae-148">Click **Save** at hello top of hello blade toosave hello modified ACR.</span></span> <span data-ttu-id="06fae-149">Zobrazí následující potvrzující zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="06fae-149">You see hello following confirmation message:</span></span>
   
        ![Úpravy záznamů o řízení přístupu](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a><span data-ttu-id="06fae-151">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="06fae-151">Delete an access control record</span></span>

<span data-ttu-id="06fae-152">Použít hello **konfigurace** stránku hello Azure portálu toodelete ACRs.</span><span class="sxs-lookup"><span data-stu-id="06fae-152">You use hello **Configuration** page in hello Azure portal toodelete ACRs.</span></span>

> [!NOTE]
> 
> * <span data-ttu-id="06fae-153">Nedoporučuje se mazat ACR, který je aktuálně používán.</span><span class="sxs-lookup"><span data-stu-id="06fae-153">You should not delete an ACR that is currently in use.</span></span> <span data-ttu-id="06fae-154">toodelete přidruženého ACR svazku, který je aktuálně používán, byste měli nejdřív vzít hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="06fae-154">toodelete an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>
> * <span data-ttu-id="06fae-155">Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="06fae-155">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


<span data-ttu-id="06fae-156">Proveďte následující kroky toodelete záznam řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="06fae-156">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="06fae-157">toodelete záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="06fae-157">toodelete an access control record</span></span>

1. <span data-ttu-id="06fae-158">Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **konfigurace** části **záznamy řízení přístupu**.</span><span class="sxs-lookup"><span data-stu-id="06fae-158">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>

2. <span data-ttu-id="06fae-159">V hello **záznamy řízení přístupu** okno z hello tabulkové seznam hello záznamy řízení přístupu, klikněte dvakrát na hello ACR chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="06fae-159">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toodelete.</span></span>

3. <span data-ttu-id="06fae-160">V okně hello úpravy přístupu řízení záznamy, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="06fae-160">In hello Edit access control records blade, click **Delete**.</span></span>
   
    ![Odstranit ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. <span data-ttu-id="06fae-162">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **odstranit** toocontinue s hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="06fae-162">When prompted for confirmation, click **Delete** toocontinue with hello deletion.</span></span> <span data-ttu-id="06fae-163">tabulkový výčet Hello je aktualizovaný tooreflect hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="06fae-163">hello tabular listing is updated tooreflect hello deletion.</span></span>
   
   ![Zpráva upozornění](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a><span data-ttu-id="06fae-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06fae-165">Next steps</span></span>

* <span data-ttu-id="06fae-166">Další informace o [přidání svazků a konfigurace ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="06fae-166">Learn more about [adding volumes and configuring ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

