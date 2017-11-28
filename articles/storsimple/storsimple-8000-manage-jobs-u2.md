---
title: "aaaView a spravovat úlohy pro řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje okno úlohy služby StorSimple Manager zařízení hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
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
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a><span data-ttu-id="a1fbe-103">Použít tooview služby StorSimple Manager zařízení hello a spravovat úlohy (Update 3 a novější)</span><span class="sxs-lookup"><span data-stu-id="a1fbe-103">Use hello StorSimple Device Manager service tooview and manage jobs (Update 3 and later)</span></span>

## <a name="overview"></a><span data-ttu-id="a1fbe-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a1fbe-104">Overview</span></span>
<span data-ttu-id="a1fbe-105">Hello **úlohy** okno poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="a1fbe-106">Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="a1fbe-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-107">Results are presented in a tabular format.</span></span>

![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

<span data-ttu-id="a1fbe-109">Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:</span><span class="sxs-lookup"><span data-stu-id="a1fbe-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="a1fbe-110">**Stav** – úloh může být v průběhu, byly úspěšné, zrušené, se nezdařilo, ruší nebo úspěšně dokončeno s chybami.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-110">**Status** – Jobs can be in progress, succeeded, canceled, failed, canceling, or succeeded with errors.</span></span>
* <span data-ttu-id="a1fbe-111">**Čas rozsah** – úloh může být filtrovaná podle hello rozsah data a času.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-111">**Time range** – Jobs can be filtered based on hello date and time range.</span></span> <span data-ttu-id="a1fbe-112">Hello rozsahy jsou za hodiny za posledních 24 hodin, posledních 7 dnů, za 30 dní, minulém roce nebo vlastní hodnota data.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-112">hello ranges are past 1 hour, past 24 hours, past 7 days, past 30 days, past year, or custom date.</span></span>
* <span data-ttu-id="a1fbe-113">**Typ** – typ hello úlohy můžou být plánované zálohování, ruční zálohy, obnovení zálohy, klonování svazku, převzít kontejnery svazků, vytváření místně vázaný svazek, úpravě svazku, instalaci aktualizací, shromažďovat protokoly podpory a vytvoření cloudu zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-113">**Type** – hello job type can be scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally-pinned volume, modify volume, install updates, collect support logs and create cloud appliance.</span></span>
* <span data-ttu-id="a1fbe-114">**Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-114">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  
<span data-ttu-id="a1fbe-115">Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="a1fbe-115">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
* <span data-ttu-id="a1fbe-116">**Název** – naplánované zálohování, ruční zálohování, obnovení zálohu, klonování svazku, převzetí služeb při selhání kontejnery svazků, vytvořte místně vázaný svazek, upravit svazek, instalaci aktualizací, shromažďovat protokoly podpory nebo vytvořte cloudu zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-116">**Name** – scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally pinned volume, modify volume, install updates, collect support logs, or create cloud appliance.</span></span>
* <span data-ttu-id="a1fbe-117">**Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-117">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="a1fbe-118">**Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-118">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="a1fbe-119">Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-119">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="a1fbe-120">V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-120">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="a1fbe-121">**Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-121">**Device** – hello name of hello device on which hello job was started.</span></span>
* <span data-ttu-id="a1fbe-122">**Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-122">**Started on** – hello time when hello job was started.</span></span>
* <span data-ttu-id="a1fbe-123">**Doba trvání** – hello čase požadované toocomplete hello úloha.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-123">**Duration** – hello time required toocomplete hello job.</span></span>

<span data-ttu-id="a1fbe-124">Hello seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="a1fbe-125">Můžete provádět následující akce související s úlohy na této stránce hello:</span><span class="sxs-lookup"><span data-stu-id="a1fbe-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="a1fbe-126">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="a1fbe-126">View job details</span></span>
* <span data-ttu-id="a1fbe-127">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="a1fbe-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="a1fbe-128">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="a1fbe-128">View job details</span></span>
<span data-ttu-id="a1fbe-129">Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="a1fbe-130">Podrobnosti úlohy tooview</span><span class="sxs-lookup"><span data-stu-id="a1fbe-130">tooview job details</span></span>
1. <span data-ttu-id="a1fbe-131">Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-131">Go tooyour StorSimple Device Manager service and then click **Jobs**.</span></span>

2. <span data-ttu-id="a1fbe-132">V hello **úlohy** okno, zobrazení úloh hello zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-132">In hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="a1fbe-133">Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-133">You can search for completed, running, or canceled jobs.</span></span>

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. <span data-ttu-id="a1fbe-135">Vyberte a klikněte na úlohu.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-135">Select and click a job.</span></span>

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. <span data-ttu-id="a1fbe-137">V modulu snap-in okno Podrobnosti úlohy hello můžete zobrazit stav hello, podrobnosti, statistiku časových údajů a dat statistiky.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-137">In hello job details blade, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a><span data-ttu-id="a1fbe-139">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="a1fbe-139">Cancel a job</span></span>
<span data-ttu-id="a1fbe-140">Proveďte následující kroky toocancel spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-140">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="a1fbe-141">Některé úlohy, jako jsou úpravy typu svazku toochange hello svazku nebo rozšíření svazku, nelze zrušit.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-141">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>


### <a name="toocancel-a-job"></a><span data-ttu-id="a1fbe-142">toocancel úlohy</span><span class="sxs-lookup"><span data-stu-id="a1fbe-142">toocancel a job</span></span>
1. <span data-ttu-id="a1fbe-143">Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-143">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span> <span data-ttu-id="a1fbe-144">Vyberte úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-144">Select hello job.</span></span>

2. <span data-ttu-id="a1fbe-145">Klikněte pravým tlačítkem na hello vybrané úlohy tooinvoke hello kontextovou nabídku a klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-145">Right-click on hello selected job tooinvoke hello context menu and click **Cancel**.</span></span>

    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. <span data-ttu-id="a1fbe-147">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-147">When prompted for confirmation, click **Yes**.</span></span> <span data-ttu-id="a1fbe-148">Tato úloha je nyní zrušena.</span><span class="sxs-lookup"><span data-stu-id="a1fbe-148">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1fbe-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1fbe-149">Next steps</span></span>
* <span data-ttu-id="a1fbe-150">Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="a1fbe-150">Learn how too[manage your StorSimple backup policies](storsimple-8000-manage-backup-policies-u2.md).</span></span>
* <span data-ttu-id="a1fbe-151">Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="a1fbe-151">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

