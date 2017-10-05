---
title: "Zásady zálohování Snapshot Manager zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak pomocí modulu snap-in konzoly MMC StorSimple Snapshot Manager vytvořit a spravovat zásady zálohování, které řídí naplánovaných záloh."
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
ms.openlocfilehash: 218c89e403673c16c72da95aa2c1d685bbed5a86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a><span data-ttu-id="5dd22-103">Pomocí StorSimple Snapshot Manager vytvořit a spravovat zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-103">Use StorSimple Snapshot Manager to create and manage backup policies</span></span>
## <a name="overview"></a><span data-ttu-id="5dd22-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5dd22-104">Overview</span></span>
<span data-ttu-id="5dd22-105">Zásady zálohování vytvoří plán pro zálohování dat svazku místně nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="5dd22-105">A backup policy creates a schedule for backing up volume data locally or in the cloud.</span></span> <span data-ttu-id="5dd22-106">Když vytvoříte zásadu zálohování, můžete také zadat zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="5dd22-106">When you create a backup policy, you can also specify a retention policy.</span></span> <span data-ttu-id="5dd22-107">(Můžete uchovávat maximálně 64 snímky.) Další informace o zásady zálohování najdete v tématu [zálohování typy](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) v [řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5dd22-107">(You can retain a maximum of 64 snapshots.) For more information about backup policies, see [Backup types](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000 series: a hybrid cloud solution](storsimple-overview.md).</span></span>

<span data-ttu-id="5dd22-108">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="5dd22-108">This tutorial explains how to:</span></span>

* <span data-ttu-id="5dd22-109">Vytvořit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-109">Create a backup policy</span></span>
* <span data-ttu-id="5dd22-110">Upravit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-110">Edit a backup policy</span></span>
* <span data-ttu-id="5dd22-111">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-111">Delete a backup policy</span></span>

## <a name="create-a-backup-policy"></a><span data-ttu-id="5dd22-112">Vytvořit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-112">Create a backup policy</span></span>
<span data-ttu-id="5dd22-113">Použijte následující postup k vytvoření nové zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5dd22-113">Use the following procedure to create a new backup policy.</span></span>

#### <a name="to-create-a-backup-policy"></a><span data-ttu-id="5dd22-114">Chcete-li vytvořit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-114">To create a backup policy</span></span>
1. <span data-ttu-id="5dd22-115">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="5dd22-115">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="5dd22-116">V **oboru** podokně klikněte pravým tlačítkem na **zásady zálohování**a klikněte na tlačítko **vytvořit zásadu zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-116">In the **Scope** pane, right-click **Backup Policies**, and click **Create Backup Policy**.</span></span>

    ![Vytvořit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    <span data-ttu-id="5dd22-118">**Vytvořit zásadu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dd22-118">The **Create a Policy** dialog box appears.</span></span>

    ![Vytvoření zásady – karta Obecné](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. <span data-ttu-id="5dd22-120">Na **Obecné** proveďte následující informace:</span><span class="sxs-lookup"><span data-stu-id="5dd22-120">On the **General** tab, complete the following information:</span></span>

   1. <span data-ttu-id="5dd22-121">V **název** textového pole zadejte název pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="5dd22-121">In the **Name** text box, type a name for the policy.</span></span>
   2. <span data-ttu-id="5dd22-122">V **svazku skupiny** textového pole, zadejte název skupiny svazku přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="5dd22-122">In the **Volume group** text box, type the name of the volume group associated with the policy.</span></span>
   3. <span data-ttu-id="5dd22-123">Vyberte buď **místní snímek** nebo **cloudových snímků**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-123">Select either **Local Snapshot** or **Cloud Snapshot**.</span></span>
   4. <span data-ttu-id="5dd22-124">Vyberte počet snímků, které chcete zachovat.</span><span class="sxs-lookup"><span data-stu-id="5dd22-124">Select the number of snapshots to retain.</span></span> <span data-ttu-id="5dd22-125">Pokud vyberete **všechny**, 64 snímky bude zachována (maximum).</span><span class="sxs-lookup"><span data-stu-id="5dd22-125">If you select **All**, 64 snapshots will be retained (the maximum).</span></span>
4. <span data-ttu-id="5dd22-126">Klikněte **plán** kartě.</span><span class="sxs-lookup"><span data-stu-id="5dd22-126">Click the **Schedule** tab.</span></span>

    ![Vytvoření zásady – karta plán](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. <span data-ttu-id="5dd22-128">Na **plán** proveďte následující informace:</span><span class="sxs-lookup"><span data-stu-id="5dd22-128">On the **Schedule** tab, complete the following information:</span></span>

   1. <span data-ttu-id="5dd22-129">Klikněte **povolit** políčko naplánovat další zálohování.</span><span class="sxs-lookup"><span data-stu-id="5dd22-129">Click the **Enable** check box to schedule the next backup.</span></span>
   2. <span data-ttu-id="5dd22-130">V části **nastavení**, vyberte **jednou**, **denní**, **týdenní**, nebo **měsíční**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-130">Under **Settings**, select **One time**, **Daily**, **Weekly**, or **Monthly**.</span></span>
   3. <span data-ttu-id="5dd22-131">V **spustit** textového pole, klikněte na ikonu kalendáři a vyberte počáteční datum.</span><span class="sxs-lookup"><span data-stu-id="5dd22-131">In the **Start** text box, click the calendar icon and select a start date.</span></span>
   4. <span data-ttu-id="5dd22-132">V části **Upřesnit nastavení**, můžete nastavit volitelné opakování plány a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="5dd22-132">Under **Advanced Settings**, you can set optional repeat schedules and an end date.</span></span>
   5. <span data-ttu-id="5dd22-133">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-133">Click **OK**.</span></span>

<span data-ttu-id="5dd22-134">Po vytvoření zásady zálohování, tyto informace se zobrazí v **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="5dd22-134">After you create a backup policy, the following information appears in the **Results** pane:</span></span>

* <span data-ttu-id="5dd22-135">**Název** – název zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5dd22-135">**Name** – the name of backup policy.</span></span>
* <span data-ttu-id="5dd22-136">**Typ** – místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="5dd22-136">**Type** – local snapshot or cloud snapshot.</span></span>
* <span data-ttu-id="5dd22-137">**Svazek skupiny** – skupině svazku přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="5dd22-137">**Volume Group** – the volume group associated with the policy.</span></span>
* <span data-ttu-id="5dd22-138">**Uchování** – počet snímků uchovávají; maximální počet je 64.</span><span class="sxs-lookup"><span data-stu-id="5dd22-138">**Retention** – the number of snapshots retained; the maximum is 64.</span></span>
* <span data-ttu-id="5dd22-139">**Vytvořit** – datum, tato zásada byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="5dd22-139">**Created** – the date that this policy was created.</span></span>
* <span data-ttu-id="5dd22-140">**Povolit** – jestli zásady je aktuálně v platnosti: **True** označuje, že je v platnosti; **False** signalizuje, že není platný.</span><span class="sxs-lookup"><span data-stu-id="5dd22-140">**Enabled** – whether the policy is currently in effect: **True** indicates that it is in effect; **False** indicates that it is not in effect.</span></span>

## <a name="edit-a-backup-policy"></a><span data-ttu-id="5dd22-141">Upravit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-141">Edit a backup policy</span></span>
<span data-ttu-id="5dd22-142">Chcete-li upravit existující zásady zálohování použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="5dd22-142">Use the following procedure to edit an existing backup policy.</span></span>

#### <a name="to-edit-a-backup-policy"></a><span data-ttu-id="5dd22-143">Chcete-li upravit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-143">To edit a backup policy</span></span>
1. <span data-ttu-id="5dd22-144">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="5dd22-144">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="5dd22-145">V **oboru** podokně klikněte **zásady zálohování** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5dd22-145">In the **Scope** pane, click the **Backup Policies** node.</span></span> <span data-ttu-id="5dd22-146">Zobrazí všechny zásady zálohování v **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="5dd22-146">All the backup policies appear in the **Results** pane.</span></span>
3. <span data-ttu-id="5dd22-147">Klikněte pravým tlačítkem na zásadu, kterou chcete upravit a pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-147">Right-click the policy that you want to edit, and then click **Edit**.</span></span>

    ![Upravit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. <span data-ttu-id="5dd22-149">Když **vytvořit zásadu** okno se zobrazí, zadejte změny a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-149">When the **Create a Policy** window appears, enter your changes, and then click **OK**.</span></span>

## <a name="delete-a-backup-policy"></a><span data-ttu-id="5dd22-150">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-150">Delete a backup policy</span></span>
<span data-ttu-id="5dd22-151">Pomocí následujícího postupu můžete odstranit zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5dd22-151">Use the following procedure to delete a backup policy.</span></span>

#### <a name="to-delete-a-backup-policy"></a><span data-ttu-id="5dd22-152">Chcete-li odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5dd22-152">To delete a backup policy</span></span>
1. <span data-ttu-id="5dd22-153">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="5dd22-153">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="5dd22-154">V **oboru** podokně klikněte **zásady zálohování** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5dd22-154">In the **Scope** pane, click the **Backup Policies** node.</span></span> <span data-ttu-id="5dd22-155">Zobrazí všechny zásady zálohování v **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="5dd22-155">All the backup policies appear in the **Results** pane.</span></span>
3. <span data-ttu-id="5dd22-156">Klikněte pravým tlačítkem na zásadu zálohování, který chcete odstranit a potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-156">Right-click the backup policy that you want to delete, and then click **Delete**.</span></span>
4. <span data-ttu-id="5dd22-157">Jakmile se zobrazí zpráva o potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="5dd22-157">When the confirmation message appears, click **Yes**.</span></span>

    ![Odstranit zásady zálohování potvrzení](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a><span data-ttu-id="5dd22-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dd22-159">Next steps</span></span>
* <span data-ttu-id="5dd22-160">Zjistěte, jak [použít ke správě vašeho řešení StorSimple Snapshot Manager zařízení StorSimple](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="5dd22-160">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="5dd22-161">Zjistěte, jak [pomocí StorSimple Snapshot Manager můžete zobrazit a spravovat úlohy zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="5dd22-161">Learn how to [use StorSimple Snapshot Manager to view and manage backup jobs](storsimple-snapshot-manager-manage-backup-jobs.md).</span></span>
