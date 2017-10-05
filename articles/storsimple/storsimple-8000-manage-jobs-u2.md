---
title: "Zobrazit a spravovat úlohy pro řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, v okně úlohy služby StorSimple Manager zařízení a způsobu jeho použití ke sledování poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: bf8038b7171053b75eeb9aed88bff4246e65a8a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a><span data-ttu-id="9a6be-103">Pomocí služby StorSimple Manager zařízení můžete zobrazit a spravovat úlohy (Update 3 a novější)</span><span class="sxs-lookup"><span data-stu-id="9a6be-103">Use the StorSimple Device Manager service to view and manage jobs (Update 3 and later)</span></span>

## <a name="overview"></a><span data-ttu-id="9a6be-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9a6be-104">Overview</span></span>
<span data-ttu-id="9a6be-105">**Úlohy** okno poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojení ke službě StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-105">The **Jobs** blade provides a single central portal for viewing and managing jobs that were started on devices connected to your StorSimple Device Manager service.</span></span> <span data-ttu-id="9a6be-106">Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="9a6be-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="9a6be-107">Results are presented in a tabular format.</span></span>

![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

<span data-ttu-id="9a6be-109">Můžete rychle najít úlohy, které zajímá pomocí filtrování na pole, jako například:</span><span class="sxs-lookup"><span data-stu-id="9a6be-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="9a6be-110">**Stav** – úloh může být v průběhu, byly úspěšné, zrušené, se nezdařilo, ruší nebo úspěšně dokončeno s chybami.</span><span class="sxs-lookup"><span data-stu-id="9a6be-110">**Status** – Jobs can be in progress, succeeded, canceled, failed, canceling, or succeeded with errors.</span></span>
* <span data-ttu-id="9a6be-111">**Čas rozsah** – úlohy je možné filtrovat podle rozsahu datum a čas.</span><span class="sxs-lookup"><span data-stu-id="9a6be-111">**Time range** – Jobs can be filtered based on the date and time range.</span></span> <span data-ttu-id="9a6be-112">Tyto rozsahy následují za hodiny za posledních 24 hodin, posledních 7 dnů, za 30 dní, minulém roce nebo vlastní hodnota data.</span><span class="sxs-lookup"><span data-stu-id="9a6be-112">The ranges are past 1 hour, past 24 hours, past 7 days, past 30 days, past year, or custom date.</span></span>
* <span data-ttu-id="9a6be-113">**Typ** – typ úlohy můžou být plánované zálohování, ruční zálohy, obnovení zálohu, klonování svazku, převzít kontejnery svazků, vytváření místně vázaný svazek, úpravě svazku, instalaci aktualizací, shromažďovat protokoly podpory a vytvoření cloudu zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-113">**Type** – The job type can be scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally-pinned volume, modify volume, install updates, collect support logs and create cloud appliance.</span></span>
* <span data-ttu-id="9a6be-114">**Zařízení** – úloh se spouští na určité zařízení připojena k službě.</span><span class="sxs-lookup"><span data-stu-id="9a6be-114">**Devices** – Jobs are initiated on a certain device connected to your service.</span></span>
  
<span data-ttu-id="9a6be-115">Filtrované úlohy jsou pak v tabulce na základě následující atributy:</span><span class="sxs-lookup"><span data-stu-id="9a6be-115">The filtered jobs are then tabulated on the basis of the following attributes:</span></span>
  
* <span data-ttu-id="9a6be-116">**Název** – naplánované zálohování, ruční zálohování, obnovení zálohu, klonování svazku, převzetí služeb při selhání kontejnery svazků, vytvořte místně vázaný svazek, upravit svazek, instalaci aktualizací, shromažďovat protokoly podpory nebo vytvořte cloudu zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-116">**Name** – scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally pinned volume, modify volume, install updates, collect support logs, or create cloud appliance.</span></span>
* <span data-ttu-id="9a6be-117">**Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.</span><span class="sxs-lookup"><span data-stu-id="9a6be-117">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="9a6be-118">**Entity** – úloh může být přidružen svazku, zásady zálohování nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-118">**Entity** – The jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="9a6be-119">Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="9a6be-119">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="9a6be-120">V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a6be-120">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="9a6be-121">**Zařízení** – název zařízení, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9a6be-121">**Device** – The name of the device on which the job was started.</span></span>
* <span data-ttu-id="9a6be-122">**Bylo zahájeno** – čas zahájení úlohy.</span><span class="sxs-lookup"><span data-stu-id="9a6be-122">**Started on** – The time when the job was started.</span></span>
* <span data-ttu-id="9a6be-123">**Doba trvání** – čas potřebný k dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="9a6be-123">**Duration** – The time required to complete the job.</span></span>

<span data-ttu-id="9a6be-124">Seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9a6be-124">The list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="9a6be-125">Na této stránce můžete provádět následující úlohy související akce:</span><span class="sxs-lookup"><span data-stu-id="9a6be-125">You can perform the following job-related actions on this page:</span></span>

* <span data-ttu-id="9a6be-126">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="9a6be-126">View job details</span></span>
* <span data-ttu-id="9a6be-127">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="9a6be-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="9a6be-128">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="9a6be-128">View job details</span></span>
<span data-ttu-id="9a6be-129">Proveďte následující kroky, chcete-li zobrazit podrobnosti libovolné úlohy.</span><span class="sxs-lookup"><span data-stu-id="9a6be-129">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="9a6be-130">Chcete-li zobrazit podrobnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="9a6be-130">To view job details</span></span>
1. <span data-ttu-id="9a6be-131">Přejděte do služby StorSimple Manager zařízení a pak klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="9a6be-131">Go to your StorSimple Device Manager service and then click **Jobs**.</span></span>

2. <span data-ttu-id="9a6be-132">V **úlohy** okně zobrazení úloh se zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="9a6be-132">In the **Jobs** blade, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="9a6be-133">Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="9a6be-133">You can search for completed, running, or canceled jobs.</span></span>

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. <span data-ttu-id="9a6be-135">Vyberte a klikněte na úlohu.</span><span class="sxs-lookup"><span data-stu-id="9a6be-135">Select and click a job.</span></span>

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. <span data-ttu-id="9a6be-137">V okně podrobností úlohy můžete zobrazit stav, podrobnosti, čas statistiky a data statistiky.</span><span class="sxs-lookup"><span data-stu-id="9a6be-137">In the job details blade, you can view the status, details, time statistics, and data statistics.</span></span>
   
    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a><span data-ttu-id="9a6be-139">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="9a6be-139">Cancel a job</span></span>
<span data-ttu-id="9a6be-140">Proveďte následující kroky, chcete-li zrušit probíhající úloze.</span><span class="sxs-lookup"><span data-stu-id="9a6be-140">Perform the following steps to cancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="9a6be-141">Některé úlohy, jako jsou úpravy svazek, chcete-li změnit typ svazku nebo rozšíření svazku, nelze zrušit.</span><span class="sxs-lookup"><span data-stu-id="9a6be-141">Some jobs, such as modifying a volume to change the volume type or expanding a volume, cannot be canceled.</span></span>


### <a name="to-cancel-a-job"></a><span data-ttu-id="9a6be-142">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="9a6be-142">To cancel a job</span></span>
1. <span data-ttu-id="9a6be-143">Na **úlohy** stránky, zobrazit spuštěné úlohy, kterou chcete zrušit spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="9a6be-143">On the **Jobs** page, display the running job(s) that you want to cancel by running a query with appropriate filters.</span></span> <span data-ttu-id="9a6be-144">Vyberte úlohu.</span><span class="sxs-lookup"><span data-stu-id="9a6be-144">Select the job.</span></span>

2. <span data-ttu-id="9a6be-145">Klikněte pravým tlačítkem na vybrané úlohy vyvolání místní nabídky a klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="9a6be-145">Right-click on the selected job to invoke the context menu and click **Cancel**.</span></span>

    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. <span data-ttu-id="9a6be-147">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9a6be-147">When prompted for confirmation, click **Yes**.</span></span> <span data-ttu-id="9a6be-148">Tato úloha je nyní zrušena.</span><span class="sxs-lookup"><span data-stu-id="9a6be-148">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a6be-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a6be-149">Next steps</span></span>
* <span data-ttu-id="9a6be-150">Zjistěte, jak [spravovat vaše zásady zálohování StorSimple](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9a6be-150">Learn how to [manage your StorSimple backup policies](storsimple-8000-manage-backup-policies-u2.md).</span></span>
* <span data-ttu-id="9a6be-151">Zjistěte, jak [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9a6be-151">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

