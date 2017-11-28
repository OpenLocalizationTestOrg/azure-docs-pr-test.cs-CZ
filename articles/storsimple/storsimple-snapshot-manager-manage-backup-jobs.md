---
title: "úlohy zálohování aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje, jak toouse hello tooview modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a spravovat naplánované, aktuálně spuštěné a dokončené úlohy zálohování."
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
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a><span data-ttu-id="01a80-103">Použít tooview Snapshot Manager zařízení StorSimple a spravovat úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="01a80-103">Use StorSimple Snapshot Manager tooview and manage backup jobs</span></span>

## <a name="overview"></a><span data-ttu-id="01a80-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="01a80-104">Overview</span></span>
<span data-ttu-id="01a80-105">Hello **úlohy** uzel v hello **oboru** podokně se zobrazují hello **naplánovaná**, **posledních 24 hodin**, a **systémem**úlohy, které jste spustili interaktivně nebo nakonfigurované zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="01a80-105">hello **Jobs** node in hello **Scope** pane shows hello **Scheduled**, **Last 24 hours**, and **Running** backup tasks that you initiated interactively or by a configured policy.</span></span> 

<span data-ttu-id="01a80-106">Tento kurz vysvětluje, jak je možné používat hello **úlohy** uzlu toodisplay informace o naplánované, poslední a aktuálně spuštěné úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="01a80-106">This tutorial explains how you can use hello **Jobs** node toodisplay information about scheduled, recent, and currently running backup jobs.</span></span> <span data-ttu-id="01a80-107">(hello seznam úloh a odpovídající informace se zobrazí v hello **výsledky** podokně.) Kromě toho můžete klikněte pravým tlačítkem na úlohu uvedené a najdete v části z kontextové nabídky, které jsou uvedeny dostupné akce.</span><span class="sxs-lookup"><span data-stu-id="01a80-107">(hello list of jobs and corresponding information appears in hello **Results** pane.) Additionally, you can right-click a listed job and see a context menu that lists available actions.</span></span>

## <a name="view-scheduled-jobs"></a><span data-ttu-id="01a80-108">Zobrazit naplánované úlohy</span><span class="sxs-lookup"><span data-stu-id="01a80-108">View scheduled jobs</span></span>
<span data-ttu-id="01a80-109">Hello použijte následující postup tooview naplánovaných úlohách zálohování.</span><span class="sxs-lookup"><span data-stu-id="01a80-109">Use hello following procedure tooview scheduled backup jobs.</span></span>

#### <a name="tooview-scheduled-jobs"></a><span data-ttu-id="01a80-110">tooview naplánované úlohy</span><span class="sxs-lookup"><span data-stu-id="01a80-110">tooview scheduled jobs</span></span>
1. <span data-ttu-id="01a80-111">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="01a80-111">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="01a80-112">V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **naplánovaná**.</span><span class="sxs-lookup"><span data-stu-id="01a80-112">In hello **Scope** pane, expand hello **Jobs** node, and click **Scheduled**.</span></span> <span data-ttu-id="01a80-113">Hello následující informace se zobrazí v hello **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="01a80-113">hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="01a80-114">**Název** – hello název plánovaný snímek hello</span><span class="sxs-lookup"><span data-stu-id="01a80-114">**Name** – hello name of hello scheduled snapshot</span></span>
   * <span data-ttu-id="01a80-115">**Následně spusťte** – hello datum a čas hello další plánovaný snímek.</span><span class="sxs-lookup"><span data-stu-id="01a80-115">**Next Run** – hello date and time of hello next scheduled snapshot</span></span>
   * <span data-ttu-id="01a80-116">**Poslední spuštění** – hello datum a čas poslední plánovaný snímek hello</span><span class="sxs-lookup"><span data-stu-id="01a80-116">**Last Run** – hello date and time of hello most recent scheduled snapshot</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="01a80-117">Jednorázové pouze snímky hello **další spuštění** a **poslední spuštění** bude hello stejné.</span><span class="sxs-lookup"><span data-stu-id="01a80-117">For one-time only snapshots, hello **Next Run** and **Last Run** will be hello same.</span></span>
     
     ![Naplánované úlohy zálohování](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. <span data-ttu-id="01a80-119">Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="01a80-119">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="view-recent-jobs"></a><span data-ttu-id="01a80-120">Zobrazit nejnovější úlohy</span><span class="sxs-lookup"><span data-stu-id="01a80-120">View recent jobs</span></span>
<span data-ttu-id="01a80-121">Použijte hello následující postup tooview zálohování a obnovení úlohy, které byly dokončeny v hello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="01a80-121">Use hello following procedure tooview backup and restore jobs that were completed in hello last 24 hours.</span></span>

#### <a name="tooview-recent-jobs"></a><span data-ttu-id="01a80-122">tooview posledních úloh</span><span class="sxs-lookup"><span data-stu-id="01a80-122">tooview recent jobs</span></span>
1. <span data-ttu-id="01a80-123">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="01a80-123">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="01a80-124">V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **posledních 24 hodin**.</span><span class="sxs-lookup"><span data-stu-id="01a80-124">In hello **Scope** pane, expand hello **Jobs** node, and click **Last 24 hours**.</span></span> <span data-ttu-id="01a80-125">Hello **výsledky** podokně se zobrazí úlohy zálohování pro hello posledních 24 hodin (tooa maximálně 64 úlohy).</span><span class="sxs-lookup"><span data-stu-id="01a80-125">hello **Results** pane shows backup jobs for hello last 24 hours (tooa maximum of 64 jobs).</span></span> <span data-ttu-id="01a80-126">Hello následující informace se zobrazí v hello **výsledky** podokně, v závislosti na hello **zobrazení** možnosti můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="01a80-126">hello following information appears in hello **Results** pane, depending on hello **View** options you specify:</span></span>
   
   * <span data-ttu-id="01a80-127">**Název** – hello název hello plánovaný snímek.</span><span class="sxs-lookup"><span data-stu-id="01a80-127">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="01a80-128">**Spuštění** – hello datum a čas zahájení hello snímku.</span><span class="sxs-lookup"><span data-stu-id="01a80-128">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="01a80-129">**Zastavit** – hello datum a čas dokončení nebo byl ukončen hello snímku.</span><span class="sxs-lookup"><span data-stu-id="01a80-129">**Stopped** – hello date and time when hello snapshot finished or was terminated.</span></span>
   * <span data-ttu-id="01a80-130">**Uplynulý čas** – hello množství času mezi hello **Začínáme** a **Zastaveno** časy.</span><span class="sxs-lookup"><span data-stu-id="01a80-130">**Elapsed** – hello amount of time between hello **Started** and **Stopped** times.</span></span>
   * <span data-ttu-id="01a80-131">**Stav** – hello stav hello nedávném dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="01a80-131">**Status** – hello state of hello recently completed job.</span></span> <span data-ttu-id="01a80-132">**Úspěch** označuje zálohování hello byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="01a80-132">**Success** indicates that hello backup was created successfully.</span></span> <span data-ttu-id="01a80-133">**Se nezdařilo** označuje, že hello úlohy nebyl úspěšně spuštěn.</span><span class="sxs-lookup"><span data-stu-id="01a80-133">**Failed** indicates that hello job did not run successfully.</span></span>
   * <span data-ttu-id="01a80-134">**Informace o** – hello důvodem selhání hello.</span><span class="sxs-lookup"><span data-stu-id="01a80-134">**Information** – hello reason for hello failure.</span></span>
   * <span data-ttu-id="01a80-135">**Zpracování bajtů (MB)** – hello množství dat ze skupiny hello svazek, který byl zpracován (v MB).</span><span class="sxs-lookup"><span data-stu-id="01a80-135">**Bytes processed (MB)** – hello amount of data from hello volume group that was processed (in MBs).</span></span> 
     
     ![Úlohy, které byly spuštěny v hello posledních 24 hodin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. <span data-ttu-id="01a80-137">Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="01a80-137">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>
   
    ![Odstranit úlohu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a><span data-ttu-id="01a80-139">Zobrazit aktuálně spuštěné úlohy</span><span class="sxs-lookup"><span data-stu-id="01a80-139">View currently running jobs</span></span>
<span data-ttu-id="01a80-140">Použijte následující postup tooview úloh, které jsou aktuálně spuštěné hello.</span><span class="sxs-lookup"><span data-stu-id="01a80-140">Use hello following procedure tooview jobs that are currently running.</span></span>

#### <a name="tooview-currently-running-jobs"></a><span data-ttu-id="01a80-141">tooview aktuálně spuštěných úloh</span><span class="sxs-lookup"><span data-stu-id="01a80-141">tooview currently running jobs</span></span>
1. <span data-ttu-id="01a80-142">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="01a80-142">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="01a80-143">V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **systémem**.</span><span class="sxs-lookup"><span data-stu-id="01a80-143">In hello **Scope** pane, expand hello **Jobs** node, and click **Running**.</span></span> <span data-ttu-id="01a80-144">V závislosti na hello **zobrazení** možnosti určíte, hello následující informace se zobrazí v hello **výsledky** podokně:</span><span class="sxs-lookup"><span data-stu-id="01a80-144">Depending on hello **View** options you specify, hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="01a80-145">**Název** – hello název hello plánovaný snímek.</span><span class="sxs-lookup"><span data-stu-id="01a80-145">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="01a80-146">**Spuštění** – hello datum a čas zahájení hello snímku.</span><span class="sxs-lookup"><span data-stu-id="01a80-146">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="01a80-147">**Kontrolní bod** – hello aktuální akce hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="01a80-147">**Checkpoint** – hello current action of hello backup.</span></span>
   * <span data-ttu-id="01a80-148">**Stav** – hello procentuální hodnotě dokončení.</span><span class="sxs-lookup"><span data-stu-id="01a80-148">**Status** – hello percentage of completion.</span></span>
   * <span data-ttu-id="01a80-149">**Uplynulý čas** – hello množství času, který prošel od začátku hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="01a80-149">**Elapsed** – hello amount of time that has passed since hello backup began.</span></span> 
   * <span data-ttu-id="01a80-150">**Průměrná propustnost (MB)** – poměr celkový počet bajtů toothat zpracování dat o celkový čas potřebný pro zpracování (MB).</span><span class="sxs-lookup"><span data-stu-id="01a80-150">**Average throughput (MB)** – ratio of total bytes of data processed toothat of total time taken for processing (MBs).</span></span>
   * <span data-ttu-id="01a80-151">**Zpracování bajtů (MB)** – celkový počet bajtů dat, zpracování (v MB).</span><span class="sxs-lookup"><span data-stu-id="01a80-151">**Bytes processed (MB)** – total bytes of data processed (in MBs).</span></span>
   * <span data-ttu-id="01a80-152">**(MB) zapsaných bajtů** – celkový počet bajtů zapsaných (v MB).</span><span class="sxs-lookup"><span data-stu-id="01a80-152">**Bytes written (MB)** – total bytes of data written (in MBs).</span></span> <span data-ttu-id="01a80-153">Zahrnuje hello data a také hello metadata a proto je obvykle větší než hello zpracovat bajtů.</span><span class="sxs-lookup"><span data-stu-id="01a80-153">It includes hello data as well as hello metadata and hence is typically greater than hello Bytes Processed.</span></span>
     
     ![Aktuálně spuštěné úlohy](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. <span data-ttu-id="01a80-155">Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="01a80-155">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01a80-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01a80-156">Next steps</span></span>
* <span data-ttu-id="01a80-157">Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="01a80-157">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="01a80-158">Zjistěte, jak příliš[používat katalog zálohování hello StorSimple Snapshot Manager toomanage](storsimple-snapshot-manager-manage-backup-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="01a80-158">Learn how too[use StorSimple Snapshot Manager toomanage hello backup catalog](storsimple-snapshot-manager-manage-backup-catalog.md).</span></span>

