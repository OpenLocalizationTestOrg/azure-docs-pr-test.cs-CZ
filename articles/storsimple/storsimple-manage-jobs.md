---
title: "aaaView a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b7341270e37a9f2530a8da1fb7c54f6b699c699c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs"></a><span data-ttu-id="04489-103">Použít tooview služby StorSimple Manager hello a spravovat úlohy StorSimple</span><span class="sxs-lookup"><span data-stu-id="04489-103">Use hello StorSimple Manager service tooview and manage StorSimple jobs</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="04489-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="04489-104">Overview</span></span>
<span data-ttu-id="04489-105">Hello **úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených tooyour služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="04489-105">hello **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Manager service.</span></span> <span data-ttu-id="04489-106">Můžete zobrazit naplánované, spuštěná, dokončené a k selhání úlohy pro více zařízení.</span><span class="sxs-lookup"><span data-stu-id="04489-106">You can view scheduled, running, completed, and failed jobs for multiple devices.</span></span> <span data-ttu-id="04489-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="04489-107">Results are presented in a tabular format.</span></span> 

![Stránka úlohy](./media/storsimple-manage-jobs/HCS_JobsPage.png)

<span data-ttu-id="04489-109">Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:</span><span class="sxs-lookup"><span data-stu-id="04489-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="04489-110">**Stav** – můžete spuštěny úlohy naplánované, se nezdařila, dokončené, ruší nebo zrušené.</span><span class="sxs-lookup"><span data-stu-id="04489-110">**Status** – Jobs can be running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="04489-111">**Typ** – úloh lze vytvořit v důsledku zálohu na vyžádání nebo naplánované (**provést zálohování**), klonování, obnovení zařízení nebo operaci aktualizace.</span><span class="sxs-lookup"><span data-stu-id="04489-111">**Type** – Jobs can be created as a result of a scheduled or an on-demand backup (**Take Backup**), cloning, a device restore, or an update operation.</span></span>
* <span data-ttu-id="04489-112">**Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.</span><span class="sxs-lookup"><span data-stu-id="04489-112">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
* <span data-ttu-id="04489-113">**Od a do** – úloh může být filtrovaná podle hello rozsah data a času.</span><span class="sxs-lookup"><span data-stu-id="04489-113">**From and To** – Jobs can be filtered based on hello date and time range.</span></span>

<span data-ttu-id="04489-114">Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="04489-114">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>

* <span data-ttu-id="04489-115">**Typ** – zálohování, obnovení, klonování převzetí služeb při selhání nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="04489-115">**Type** – Backup, clone, restore, failover, or update.</span></span>
* <span data-ttu-id="04489-116">**Stav** – systémem naplánované, se nezdařila, byla dokončena, zrušení nebo došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="04489-116">**Status** – Running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="04489-117">**Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="04489-117">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="04489-118">Úloha klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="04489-118">A clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="04489-119">V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.</span><span class="sxs-lookup"><span data-stu-id="04489-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="04489-120">**Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="04489-120">**Device** – hello name of hello device on which hello job was started.</span></span>
* <span data-ttu-id="04489-121">**Spuštění na** – hello čas, kdy byla spuštěna úloha hello.</span><span class="sxs-lookup"><span data-stu-id="04489-121">**Started On** – hello time when hello job was started.</span></span>
* <span data-ttu-id="04489-122">**Průběh** – hello procento dokončení běžící úlohy.</span><span class="sxs-lookup"><span data-stu-id="04489-122">**Progress** – hello percentage completion of a running job.</span></span> <span data-ttu-id="04489-123">Dokončené úlohy to by měl být vždy 100 %.</span><span class="sxs-lookup"><span data-stu-id="04489-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="04489-124">Hello seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="04489-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="04489-125">Můžete provádět následující akce související s úlohy na této stránce hello:</span><span class="sxs-lookup"><span data-stu-id="04489-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="04489-126">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="04489-126">View job details</span></span>
* <span data-ttu-id="04489-127">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="04489-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="04489-128">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="04489-128">View job details</span></span>
<span data-ttu-id="04489-129">Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="04489-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="04489-130">Podrobnosti úlohy tooview</span><span class="sxs-lookup"><span data-stu-id="04489-130">tooview job details</span></span>
1. <span data-ttu-id="04489-131">Na hello **úlohy** stránky, zobrazit hello úloh zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="04489-131">On hello **Jobs** page, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="04489-132">Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="04489-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="04489-133">Vyberte úlohu.</span><span class="sxs-lookup"><span data-stu-id="04489-133">Select a job.</span></span>
3. <span data-ttu-id="04489-134">V dolní části hello hello stránky, klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="04489-134">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="04489-135">V hello **podrobnosti úlohy zálohování** dialogové okno, můžete zobrazit stav hello, podrobnosti, statistiku časových údajů a dat statistiky.</span><span class="sxs-lookup"><span data-stu-id="04489-135">In hello **Backup Job Details** dialog box, you can view hello status, details, time statistics, and data statistics.</span></span>

## <a name="cancel-a-job"></a><span data-ttu-id="04489-136">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="04489-136">Cancel a job</span></span>
<span data-ttu-id="04489-137">Proveďte následující kroky toocancel spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="04489-137">Perform hello following steps toocancel a running job.</span></span>

### <a name="toocancel-a-job"></a><span data-ttu-id="04489-138">toocancel úlohy</span><span class="sxs-lookup"><span data-stu-id="04489-138">toocancel a job</span></span>
1. <span data-ttu-id="04489-139">Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="04489-139">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="04489-140">Vyberte úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="04489-140">Select hello job.</span></span>
3. <span data-ttu-id="04489-141">V dolní části hello hello stránky, klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="04489-141">At hello bottom of hello page, click **Cancel**.</span></span>
4. <span data-ttu-id="04489-142">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="04489-142">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="04489-143">Tato úloha je nyní zrušena.</span><span class="sxs-lookup"><span data-stu-id="04489-143">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04489-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04489-144">Next steps</span></span>
* <span data-ttu-id="04489-145">Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="04489-145">Learn how too[manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="04489-146">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="04489-146">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

