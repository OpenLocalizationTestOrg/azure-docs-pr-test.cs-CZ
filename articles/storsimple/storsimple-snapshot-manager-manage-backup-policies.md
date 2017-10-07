---
title: "zásady zálohování aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje, jak toouse hello toocreate modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a spravovat zásady zálohování hello, které řídí naplánovaných záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a><span data-ttu-id="c4404-103">Použít toocreate Snapshot Manager zařízení StorSimple a spravovat zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-103">Use StorSimple Snapshot Manager toocreate and manage backup policies</span></span>
## <a name="overview"></a><span data-ttu-id="c4404-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c4404-104">Overview</span></span>
<span data-ttu-id="c4404-105">Zásady zálohování vytvoří plán pro zálohování dat svazku místně nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c4404-105">A backup policy creates a schedule for backing up volume data locally or in hello cloud.</span></span> <span data-ttu-id="c4404-106">Když vytvoříte zásadu zálohování, můžete také zadat zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="c4404-106">When you create a backup policy, you can also specify a retention policy.</span></span> <span data-ttu-id="c4404-107">(Můžete uchovávat maximálně 64 snímky.) Další informace o zásady zálohování najdete v tématu [zálohování typy](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) v [řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4404-107">(You can retain a maximum of 64 snapshots.) For more information about backup policies, see [Backup types](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000 series: a hybrid cloud solution](storsimple-overview.md).</span></span>

<span data-ttu-id="c4404-108">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="c4404-108">This tutorial explains how to:</span></span>

* <span data-ttu-id="c4404-109">Vytvořit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-109">Create a backup policy</span></span>
* <span data-ttu-id="c4404-110">Upravit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-110">Edit a backup policy</span></span>
* <span data-ttu-id="c4404-111">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-111">Delete a backup policy</span></span>

## <a name="create-a-backup-policy"></a><span data-ttu-id="c4404-112">Vytvořit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-112">Create a backup policy</span></span>
<span data-ttu-id="c4404-113">Použijte následující postup toocreate nové zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="c4404-113">Use hello following procedure toocreate a new backup policy.</span></span>

#### <a name="toocreate-a-backup-policy"></a><span data-ttu-id="c4404-114">toocreate zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-114">toocreate a backup policy</span></span>
1. <span data-ttu-id="c4404-115">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c4404-115">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c4404-116">V hello **oboru** podokně klikněte pravým tlačítkem na **zásady zálohování**a klikněte na tlačítko **vytvořit zásadu zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c4404-116">In hello **Scope** pane, right-click **Backup Policies**, and click **Create Backup Policy**.</span></span>

    ![Vytvořit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    <span data-ttu-id="c4404-118">Hello **vytvořit zásadu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4404-118">hello **Create a Policy** dialog box appears.</span></span>

    ![Vytvoření zásady – karta Obecné](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. <span data-ttu-id="c4404-120">Na hello **Obecné** kartě, dokončení hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="c4404-120">On hello **General** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="c4404-121">V hello **název** textového pole zadejte název zásady hello.</span><span class="sxs-lookup"><span data-stu-id="c4404-121">In hello **Name** text box, type a name for hello policy.</span></span>
   2. <span data-ttu-id="c4404-122">V hello **svazku skupiny** textového pole, název typu hello hello svazku skupiny přidružené k hello zásad.</span><span class="sxs-lookup"><span data-stu-id="c4404-122">In hello **Volume group** text box, type hello name of hello volume group associated with hello policy.</span></span>
   3. <span data-ttu-id="c4404-123">Vyberte buď **místní snímek** nebo **cloudových snímků**.</span><span class="sxs-lookup"><span data-stu-id="c4404-123">Select either **Local Snapshot** or **Cloud Snapshot**.</span></span>
   4. <span data-ttu-id="c4404-124">Vyberte počet hello tooretain snímky.</span><span class="sxs-lookup"><span data-stu-id="c4404-124">Select hello number of snapshots tooretain.</span></span> <span data-ttu-id="c4404-125">Pokud vyberete **všechny**, budou se uchovávat 64 snímky (hello maximální).</span><span class="sxs-lookup"><span data-stu-id="c4404-125">If you select **All**, 64 snapshots will be retained (hello maximum).</span></span>
4. <span data-ttu-id="c4404-126">Klikněte na tlačítko hello **plán** kartě.</span><span class="sxs-lookup"><span data-stu-id="c4404-126">Click hello **Schedule** tab.</span></span>

    ![Vytvoření zásady – karta plán](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. <span data-ttu-id="c4404-128">Na hello **plán** kartě, dokončení hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="c4404-128">On hello **Schedule** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="c4404-129">Klikněte na tlačítko hello **povolit** políčko tooschedule hello další zálohování.</span><span class="sxs-lookup"><span data-stu-id="c4404-129">Click hello **Enable** check box tooschedule hello next backup.</span></span>
   2. <span data-ttu-id="c4404-130">V části **nastavení**, vyberte **jednou**, **denní**, **týdenní**, nebo **měsíční**.</span><span class="sxs-lookup"><span data-stu-id="c4404-130">Under **Settings**, select **One time**, **Daily**, **Weekly**, or **Monthly**.</span></span>
   3. <span data-ttu-id="c4404-131">V hello **spustit** textového pole, klikněte na ikonu hello kalendáře a vyberte počáteční datum.</span><span class="sxs-lookup"><span data-stu-id="c4404-131">In hello **Start** text box, click hello calendar icon and select a start date.</span></span>
   4. <span data-ttu-id="c4404-132">V části **Upřesnit nastavení**, můžete nastavit volitelné opakování plány a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="c4404-132">Under **Advanced Settings**, you can set optional repeat schedules and an end date.</span></span>
   5. <span data-ttu-id="c4404-133">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4404-133">Click **OK**.</span></span>

<span data-ttu-id="c4404-134">Po vytvoření zásady zálohování hello následující informace se zobrazí v hello **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="c4404-134">After you create a backup policy, hello following information appears in hello **Results** pane:</span></span>

* <span data-ttu-id="c4404-135">**Název** – hello název zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="c4404-135">**Name** – hello name of backup policy.</span></span>
* <span data-ttu-id="c4404-136">**Typ** – místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="c4404-136">**Type** – local snapshot or cloud snapshot.</span></span>
* <span data-ttu-id="c4404-137">**Svazek skupiny** – hello spojené s hello zásadami skupiny svazku.</span><span class="sxs-lookup"><span data-stu-id="c4404-137">**Volume Group** – hello volume group associated with hello policy.</span></span>
* <span data-ttu-id="c4404-138">**Uchování** – hello uchovávají počet snímků; hello maximální 64.</span><span class="sxs-lookup"><span data-stu-id="c4404-138">**Retention** – hello number of snapshots retained; hello maximum is 64.</span></span>
* <span data-ttu-id="c4404-139">**Vytvořit** – hello datum vytvoření tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="c4404-139">**Created** – hello date that this policy was created.</span></span>
* <span data-ttu-id="c4404-140">**Povolit** – jestli hello zásad je aktuálně v platnosti: **True** označuje, že je v platnosti; **False** signalizuje, že není platný.</span><span class="sxs-lookup"><span data-stu-id="c4404-140">**Enabled** – whether hello policy is currently in effect: **True** indicates that it is in effect; **False** indicates that it is not in effect.</span></span>

## <a name="edit-a-backup-policy"></a><span data-ttu-id="c4404-141">Upravit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-141">Edit a backup policy</span></span>
<span data-ttu-id="c4404-142">Použijte následující postup tooedit existující zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="c4404-142">Use hello following procedure tooedit an existing backup policy.</span></span>

#### <a name="tooedit-a-backup-policy"></a><span data-ttu-id="c4404-143">tooedit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-143">tooedit a backup policy</span></span>
1. <span data-ttu-id="c4404-144">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c4404-144">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c4404-145">V hello **oboru** podokně klikněte na tlačítko hello **zásady zálohování** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c4404-145">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="c4404-146">Zobrazí všechny zásady zálohování hello v hello **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="c4404-146">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="c4404-147">Klikněte pravým tlačítkem na zásadu hello má tooedit a pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="c4404-147">Right-click hello policy that you want tooedit, and then click **Edit**.</span></span>

    ![Upravit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. <span data-ttu-id="c4404-149">Když hello **vytvořit zásadu** okno se zobrazí, zadejte změny a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4404-149">When hello **Create a Policy** window appears, enter your changes, and then click **OK**.</span></span>

## <a name="delete-a-backup-policy"></a><span data-ttu-id="c4404-150">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-150">Delete a backup policy</span></span>
<span data-ttu-id="c4404-151">Použijte následující postup toodelete zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="c4404-151">Use hello following procedure toodelete a backup policy.</span></span>

#### <a name="toodelete-a-backup-policy"></a><span data-ttu-id="c4404-152">toodelete zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c4404-152">toodelete a backup policy</span></span>
1. <span data-ttu-id="c4404-153">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c4404-153">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c4404-154">V hello **oboru** podokně klikněte na tlačítko hello **zásady zálohování** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c4404-154">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="c4404-155">Zobrazí všechny zásady zálohování hello v hello **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="c4404-155">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="c4404-156">Klikněte pravým tlačítkem na zásady zálohování hello má toodelete a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c4404-156">Right-click hello backup policy that you want toodelete, and then click **Delete**.</span></span>
4. <span data-ttu-id="c4404-157">Jakmile se zobrazí potvrzovací zpráva hello, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c4404-157">When hello confirmation message appears, click **Yes**.</span></span>

    ![Odstranit zásady zálohování potvrzení](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a><span data-ttu-id="c4404-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4404-159">Next steps</span></span>
* <span data-ttu-id="c4404-160">Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="c4404-160">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="c4404-161">Zjistěte, jak příliš[použít tooview Snapshot Manager zařízení StorSimple a spravovat úlohy zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="c4404-161">Learn how too[use StorSimple Snapshot Manager tooview and manage backup jobs](storsimple-snapshot-manager-manage-backup-jobs.md).</span></span>
