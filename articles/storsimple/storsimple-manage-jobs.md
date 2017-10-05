---
title: "Zobrazit a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager a způsobu jeho použití ke sledování poslední, aktuální a naplánované úlohy zálohování."
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
ms.openlocfilehash: 7d9377bb8f3cb8c587823c2d71d61dfb9b50f52f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a><span data-ttu-id="dcd90-103">Pomocí služby StorSimple Manager můžete zobrazit a spravovat úlohy StorSimple</span><span class="sxs-lookup"><span data-stu-id="dcd90-103">Use the StorSimple Manager service to view and manage StorSimple jobs</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="dcd90-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="dcd90-104">Overview</span></span>
<span data-ttu-id="dcd90-105">**Úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojení ke službě StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="dcd90-105">The **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected to your StorSimple Manager service.</span></span> <span data-ttu-id="dcd90-106">Můžete zobrazit naplánované, spuštěná, dokončené a k selhání úlohy pro více zařízení.</span><span class="sxs-lookup"><span data-stu-id="dcd90-106">You can view scheduled, running, completed, and failed jobs for multiple devices.</span></span> <span data-ttu-id="dcd90-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="dcd90-107">Results are presented in a tabular format.</span></span> 

![Stránka úlohy](./media/storsimple-manage-jobs/HCS_JobsPage.png)

<span data-ttu-id="dcd90-109">Můžete rychle najít úlohy, které zajímá pomocí filtrování na pole, jako například:</span><span class="sxs-lookup"><span data-stu-id="dcd90-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="dcd90-110">**Stav** – můžete spuštěny úlohy naplánované, se nezdařila, dokončené, ruší nebo zrušené.</span><span class="sxs-lookup"><span data-stu-id="dcd90-110">**Status** – Jobs can be running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="dcd90-111">**Typ** – úloh lze vytvořit v důsledku zálohu na vyžádání nebo naplánované (**provést zálohování**), klonování, obnovení zařízení nebo operaci aktualizace.</span><span class="sxs-lookup"><span data-stu-id="dcd90-111">**Type** – Jobs can be created as a result of a scheduled or an on-demand backup (**Take Backup**), cloning, a device restore, or an update operation.</span></span>
* <span data-ttu-id="dcd90-112">**Zařízení** – úloh se spouští na určité zařízení připojena k službě.</span><span class="sxs-lookup"><span data-stu-id="dcd90-112">**Devices** – Jobs are initiated on a certain device connected to your service.</span></span>
* <span data-ttu-id="dcd90-113">**Od a do** – úlohy je možné filtrovat podle rozsahu datum a čas.</span><span class="sxs-lookup"><span data-stu-id="dcd90-113">**From and To** – Jobs can be filtered based on the date and time range.</span></span>

<span data-ttu-id="dcd90-114">Filtrované úlohy jsou pak v tabulce na základě následující atributy:</span><span class="sxs-lookup"><span data-stu-id="dcd90-114">The filtered jobs are then tabulated on the basis of the following attributes:</span></span>

* <span data-ttu-id="dcd90-115">**Typ** – zálohování, obnovení, klonování převzetí služeb při selhání nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="dcd90-115">**Type** – Backup, clone, restore, failover, or update.</span></span>
* <span data-ttu-id="dcd90-116">**Stav** – systémem naplánované, se nezdařila, byla dokončena, zrušení nebo došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="dcd90-116">**Status** – Running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="dcd90-117">**Entity** – úloh může být přidružen svazku, zásady zálohování nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="dcd90-117">**Entity** – The jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="dcd90-118">Úloha klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="dcd90-118">A clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="dcd90-119">V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.</span><span class="sxs-lookup"><span data-stu-id="dcd90-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="dcd90-120">**Zařízení** – název zařízení, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="dcd90-120">**Device** – The name of the device on which the job was started.</span></span>
* <span data-ttu-id="dcd90-121">**Spuštění na** – čas zahájení úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcd90-121">**Started On** – The time when the job was started.</span></span>
* <span data-ttu-id="dcd90-122">**Průběh** – procento dokončení spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcd90-122">**Progress** – The percentage completion of a running job.</span></span> <span data-ttu-id="dcd90-123">Dokončené úlohy to by měl být vždy 100 %.</span><span class="sxs-lookup"><span data-stu-id="dcd90-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="dcd90-124">Seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="dcd90-124">The list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="dcd90-125">Na této stránce můžete provádět následující úlohy související akce:</span><span class="sxs-lookup"><span data-stu-id="dcd90-125">You can perform the following job-related actions on this page:</span></span>

* <span data-ttu-id="dcd90-126">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="dcd90-126">View job details</span></span>
* <span data-ttu-id="dcd90-127">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="dcd90-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="dcd90-128">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="dcd90-128">View job details</span></span>
<span data-ttu-id="dcd90-129">Proveďte následující kroky, chcete-li zobrazit podrobnosti libovolné úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcd90-129">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="dcd90-130">Chcete-li zobrazit podrobnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="dcd90-130">To view job details</span></span>
1. <span data-ttu-id="dcd90-131">Na **úlohy** stránky, zobrazení úloh se zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="dcd90-131">On the **Jobs** page, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="dcd90-132">Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcd90-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="dcd90-133">Vyberte úlohu.</span><span class="sxs-lookup"><span data-stu-id="dcd90-133">Select a job.</span></span>
3. <span data-ttu-id="dcd90-134">V dolní části stránky klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="dcd90-134">At the bottom of the page, click **Details**.</span></span>
4. <span data-ttu-id="dcd90-135">V **podrobnosti úlohy zálohování** dialogové okno, můžete zobrazit stav, podrobnosti, čas statistiky a data statistiky.</span><span class="sxs-lookup"><span data-stu-id="dcd90-135">In the **Backup Job Details** dialog box, you can view the status, details, time statistics, and data statistics.</span></span>

## <a name="cancel-a-job"></a><span data-ttu-id="dcd90-136">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="dcd90-136">Cancel a job</span></span>
<span data-ttu-id="dcd90-137">Proveďte následující kroky, chcete-li zrušit probíhající úloze.</span><span class="sxs-lookup"><span data-stu-id="dcd90-137">Perform the following steps to cancel a running job.</span></span>

### <a name="to-cancel-a-job"></a><span data-ttu-id="dcd90-138">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="dcd90-138">To cancel a job</span></span>
1. <span data-ttu-id="dcd90-139">Na **úlohy** stránky, zobrazit spuštěné úlohy, kterou chcete zrušit spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="dcd90-139">On the **Jobs** page, display the running job(s) that you want to cancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="dcd90-140">Vyberte úlohu.</span><span class="sxs-lookup"><span data-stu-id="dcd90-140">Select the job.</span></span>
3. <span data-ttu-id="dcd90-141">V dolní části stránky klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="dcd90-141">At the bottom of the page, click **Cancel**.</span></span>
4. <span data-ttu-id="dcd90-142">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="dcd90-142">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="dcd90-143">Tato úloha je nyní zrušena.</span><span class="sxs-lookup"><span data-stu-id="dcd90-143">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dcd90-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcd90-144">Next steps</span></span>
* <span data-ttu-id="dcd90-145">Zjistěte, jak [spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="dcd90-145">Learn how to [manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="dcd90-146">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="dcd90-146">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

