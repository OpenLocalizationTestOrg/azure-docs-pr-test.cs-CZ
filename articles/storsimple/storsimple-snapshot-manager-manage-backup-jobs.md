---
title: "Úlohy zálohování Snapshot Manager zařízení StorSimple | Microsoft Docs"
description: "Popisuje způsob použití modulu snap-in konzoly MMC StorSimple Snapshot Manager můžete zobrazit a spravovat naplánované, aktuálně spuštěné a dokončené úlohy zálohování."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 03e306b62250f2bb033cc14e856a59760b5406c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a><span data-ttu-id="79f8b-103">Pomocí StorSimple Snapshot Manager můžete zobrazit a spravovat úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="79f8b-103">Use StorSimple Snapshot Manager to view and manage backup jobs</span></span>

## <a name="overview"></a><span data-ttu-id="79f8b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="79f8b-104">Overview</span></span>
<span data-ttu-id="79f8b-105">**Úlohy** uzel v **oboru** podokně zobrazí **naplánovaná**, **posledních 24 hodin**, a **systémem** úlohy zálohování, které jste spustili interaktivně nebo nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="79f8b-105">The **Jobs** node in the **Scope** pane shows the **Scheduled**, **Last 24 hours**, and **Running** backup tasks that you initiated interactively or by a configured policy.</span></span> 

<span data-ttu-id="79f8b-106">Tento kurz popisuje, jak můžete použít **úlohy** uzel k zobrazení informací o naplánované, poslední a aktuálně spuštěné úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="79f8b-106">This tutorial explains how you can use the **Jobs** node to display information about scheduled, recent, and currently running backup jobs.</span></span> <span data-ttu-id="79f8b-107">(Se zobrazí v seznamu úloh a odpovídající informace **výsledky** podokně.) Kromě toho můžete klikněte pravým tlačítkem na úlohu uvedené a najdete v části z kontextové nabídky, které jsou uvedeny dostupné akce.</span><span class="sxs-lookup"><span data-stu-id="79f8b-107">(The list of jobs and corresponding information appears in the **Results** pane.) Additionally, you can right-click a listed job and see a context menu that lists available actions.</span></span>

## <a name="view-scheduled-jobs"></a><span data-ttu-id="79f8b-108">Zobrazit naplánované úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-108">View scheduled jobs</span></span>
<span data-ttu-id="79f8b-109">Následujícím postupem zobrazíte naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="79f8b-109">Use the following procedure to view scheduled backup jobs.</span></span>

#### <a name="to-view-scheduled-jobs"></a><span data-ttu-id="79f8b-110">Chcete-li zobrazit naplánované úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-110">To view scheduled jobs</span></span>
1. <span data-ttu-id="79f8b-111">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="79f8b-111">Click the desktop icon to start StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="79f8b-112">V **oboru** podokně rozbalte **úlohy** uzel a klikněte na tlačítko **naplánovaná**.</span><span class="sxs-lookup"><span data-stu-id="79f8b-112">In the **Scope** pane, expand the **Jobs** node, and click **Scheduled**.</span></span> <span data-ttu-id="79f8b-113">Tyto informace se zobrazí v **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="79f8b-113">The following information appears in the **Results** pane:</span></span>
   
   * <span data-ttu-id="79f8b-114">**Název** – název plánovaný snímek</span><span class="sxs-lookup"><span data-stu-id="79f8b-114">**Name** – the name of the scheduled snapshot</span></span>
   * <span data-ttu-id="79f8b-115">**Následně spusťte** – datum a čas na další plánovaný snímek</span><span class="sxs-lookup"><span data-stu-id="79f8b-115">**Next Run** – the date and time of the next scheduled snapshot</span></span>
   * <span data-ttu-id="79f8b-116">**Poslední spuštění** – datum a čas poslední plánovaný snímek</span><span class="sxs-lookup"><span data-stu-id="79f8b-116">**Last Run** – the date and time of the most recent scheduled snapshot</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="79f8b-117">Pro jednorázové pouze snímky **další spuštění** a **poslední spuštění** budou stejné.</span><span class="sxs-lookup"><span data-stu-id="79f8b-117">For one-time only snapshots, the **Next Run** and **Last Run** will be the same.</span></span>
     
     ![Naplánované úlohy zálohování](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. <span data-ttu-id="79f8b-119">Chcete-li provádět další akce pro konkrétní úlohu, klikněte pravým tlačítkem na název úlohy v **výsledky** panelu a vyberte jednu z možností v nabídce.</span><span class="sxs-lookup"><span data-stu-id="79f8b-119">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>

## <a name="view-recent-jobs"></a><span data-ttu-id="79f8b-120">Zobrazit nejnovější úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-120">View recent jobs</span></span>
<span data-ttu-id="79f8b-121">Následující postup použijte k zobrazení zálohování a obnovení úlohy, které byly dokončeny za posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="79f8b-121">Use the following procedure to view backup and restore jobs that were completed in the last 24 hours.</span></span>

#### <a name="to-view-recent-jobs"></a><span data-ttu-id="79f8b-122">Chcete-li zobrazit nejnovější úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-122">To view recent jobs</span></span>
1. <span data-ttu-id="79f8b-123">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="79f8b-123">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="79f8b-124">V **oboru** podokně rozbalte **úlohy** uzel a klikněte na tlačítko **posledních 24 hodin**.</span><span class="sxs-lookup"><span data-stu-id="79f8b-124">In the **Scope** pane, expand the **Jobs** node, and click **Last 24 hours**.</span></span> <span data-ttu-id="79f8b-125">**Výsledky** podokně se zobrazují úloh zálohování za posledních 24 hodin (na maximálně 64 úlohy).</span><span class="sxs-lookup"><span data-stu-id="79f8b-125">The **Results** pane shows backup jobs for the last 24 hours (to a maximum of 64 jobs).</span></span> <span data-ttu-id="79f8b-126">Tyto informace se zobrazí v **výsledky** podokně, v závislosti na **zobrazení** možnosti můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="79f8b-126">The following information appears in the **Results** pane, depending on the **View** options you specify:</span></span>
   
   * <span data-ttu-id="79f8b-127">**Název** – název plánovaný snímek.</span><span class="sxs-lookup"><span data-stu-id="79f8b-127">**Name** – the name of the scheduled snapshot.</span></span>
   * <span data-ttu-id="79f8b-128">**Spuštění** – datum a čas zahájení snímku.</span><span class="sxs-lookup"><span data-stu-id="79f8b-128">**Started** – the date and time when the snapshot began.</span></span>
   * <span data-ttu-id="79f8b-129">**Zastavit** – datum a čas dokončení nebo byl ukončen snímku.</span><span class="sxs-lookup"><span data-stu-id="79f8b-129">**Stopped** – the date and time when the snapshot finished or was terminated.</span></span>
   * <span data-ttu-id="79f8b-130">**Uplynulý čas** – časového intervalu mezi **Začínáme** a **Zastaveno** časy.</span><span class="sxs-lookup"><span data-stu-id="79f8b-130">**Elapsed** – the amount of time between the **Started** and **Stopped** times.</span></span>
   * <span data-ttu-id="79f8b-131">**Stav** – stav nedávno dokončené úlohy.</span><span class="sxs-lookup"><span data-stu-id="79f8b-131">**Status** – the state of the recently completed job.</span></span> <span data-ttu-id="79f8b-132">**Úspěch** označuje, že záloha byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="79f8b-132">**Success** indicates that the backup was created successfully.</span></span> <span data-ttu-id="79f8b-133">**Se nezdařilo** označuje, že úloha nebyla úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="79f8b-133">**Failed** indicates that the job did not run successfully.</span></span>
   * <span data-ttu-id="79f8b-134">**Informace o** – příčinu selhání.</span><span class="sxs-lookup"><span data-stu-id="79f8b-134">**Information** – the reason for the failure.</span></span>
   * <span data-ttu-id="79f8b-135">**Zpracování bajtů (MB)** – množství dat ze skupiny svazek, který byl zpracován (v MB).</span><span class="sxs-lookup"><span data-stu-id="79f8b-135">**Bytes processed (MB)** – the amount of data from the volume group that was processed (in MBs).</span></span> 
     
     ![Úlohy, které byly spuštěny za posledních 24 hodin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. <span data-ttu-id="79f8b-137">Chcete-li provádět další akce pro konkrétní úlohu, klikněte pravým tlačítkem na název úlohy v **výsledky** panelu a vyberte jednu z možností v nabídce.</span><span class="sxs-lookup"><span data-stu-id="79f8b-137">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>
   
    ![Odstranit úlohu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a><span data-ttu-id="79f8b-139">Zobrazit aktuálně spuštěné úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-139">View currently running jobs</span></span>
<span data-ttu-id="79f8b-140">Použijte následující postup zobrazení úloh, které jsou aktuálně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="79f8b-140">Use the following procedure to view jobs that are currently running.</span></span>

#### <a name="to-view-currently-running-jobs"></a><span data-ttu-id="79f8b-141">Chcete-li zobrazit aktuálně spuštěné úlohy</span><span class="sxs-lookup"><span data-stu-id="79f8b-141">To view currently running jobs</span></span>
1. <span data-ttu-id="79f8b-142">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="79f8b-142">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="79f8b-143">V **oboru** podokně rozbalte **úlohy** uzel a klikněte na tlačítko **systémem**.</span><span class="sxs-lookup"><span data-stu-id="79f8b-143">In the **Scope** pane, expand the **Jobs** node, and click **Running**.</span></span> <span data-ttu-id="79f8b-144">V závislosti na tom **zobrazení** určíte, následující informace se zobrazí v možnosti **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="79f8b-144">Depending on the **View** options you specify, the following information appears in the **Results** pane:</span></span>
   
   * <span data-ttu-id="79f8b-145">**Název** – název plánovaný snímek.</span><span class="sxs-lookup"><span data-stu-id="79f8b-145">**Name** – the name of the scheduled snapshot.</span></span>
   * <span data-ttu-id="79f8b-146">**Spuštění** – datum a čas zahájení snímku.</span><span class="sxs-lookup"><span data-stu-id="79f8b-146">**Started** – the date and time when the snapshot began.</span></span>
   * <span data-ttu-id="79f8b-147">**Kontrolní bod** – zálohy aktuální akce.</span><span class="sxs-lookup"><span data-stu-id="79f8b-147">**Checkpoint** – the current action of the backup.</span></span>
   * <span data-ttu-id="79f8b-148">**Stav** – o procentuální hodnotě dokončení.</span><span class="sxs-lookup"><span data-stu-id="79f8b-148">**Status** – the percentage of completion.</span></span>
   * <span data-ttu-id="79f8b-149">**Uplynulý čas** – množství času, který prošel od začátku zálohování.</span><span class="sxs-lookup"><span data-stu-id="79f8b-149">**Elapsed** – the amount of time that has passed since the backup began.</span></span> 
   * <span data-ttu-id="79f8b-150">**Průměrná propustnost (MB)** – poměr celkový počet bajtů dat, které zpracoval, celkový čas potřebný pro zpracování (MB).</span><span class="sxs-lookup"><span data-stu-id="79f8b-150">**Average throughput (MB)** – ratio of total bytes of data processed to that of total time taken for processing (MBs).</span></span>
   * <span data-ttu-id="79f8b-151">**Zpracování bajtů (MB)** – celkový počet bajtů dat, zpracování (v MB).</span><span class="sxs-lookup"><span data-stu-id="79f8b-151">**Bytes processed (MB)** – total bytes of data processed (in MBs).</span></span>
   * <span data-ttu-id="79f8b-152">**(MB) zapsaných bajtů** – celkový počet bajtů zapsaných (v MB).</span><span class="sxs-lookup"><span data-stu-id="79f8b-152">**Bytes written (MB)** – total bytes of data written (in MBs).</span></span> <span data-ttu-id="79f8b-153">Zahrnuje data, jakož i metadata a proto je obvykle větší než zpracovat bajtů.</span><span class="sxs-lookup"><span data-stu-id="79f8b-153">It includes the data as well as the metadata and hence is typically greater than the Bytes Processed.</span></span>
     
     ![Aktuálně spuštěné úlohy](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. <span data-ttu-id="79f8b-155">Chcete-li provádět další akce pro konkrétní úlohu, klikněte pravým tlačítkem na název úlohy v **výsledky** panelu a vyberte jednu z možností v nabídce.</span><span class="sxs-lookup"><span data-stu-id="79f8b-155">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79f8b-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79f8b-156">Next steps</span></span>
* <span data-ttu-id="79f8b-157">Zjistěte, jak [použít ke správě vašeho řešení StorSimple Snapshot Manager zařízení StorSimple](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="79f8b-157">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="79f8b-158">Zjistěte, jak [StorSimple Snapshot Manager použít ke správě katalogu zálohování](storsimple-snapshot-manager-manage-backup-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="79f8b-158">Learn how to [use StorSimple Snapshot Manager to manage the backup catalog](storsimple-snapshot-manager-manage-backup-catalog.md).</span></span>

