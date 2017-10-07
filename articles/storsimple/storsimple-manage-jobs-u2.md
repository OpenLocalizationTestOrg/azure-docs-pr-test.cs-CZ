---
title: "aaaView a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a><span data-ttu-id="7f0cf-103">Použít tooview služby StorSimple Manager hello a spravovat úlohy StorSimple (Update 2)</span><span class="sxs-lookup"><span data-stu-id="7f0cf-103">Use hello StorSimple Manager service tooview and manage StorSimple jobs (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="7f0cf-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7f0cf-104">Overview</span></span>
<span data-ttu-id="7f0cf-105">Hello **úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených tooyour služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-105">hello **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Manager service.</span></span> <span data-ttu-id="7f0cf-106">Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="7f0cf-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-107">Results are presented in a tabular format.</span></span> 

![Stránka úlohy](./media/storsimple-manage-jobs-u2/jobs.png)

<span data-ttu-id="7f0cf-109">Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:</span><span class="sxs-lookup"><span data-stu-id="7f0cf-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="7f0cf-110">**Stav** – úloh může být spuštěn, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-110">**Status** – Jobs can be running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="7f0cf-111">**Od a do** – úloh může být filtrovaná podle hello rozsah data a času.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-111">**From and To** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="7f0cf-112">**Typ** – hello typ úlohy může být zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-112">**Type** – hello job type can be backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
* <span data-ttu-id="7f0cf-113">**Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-113">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  <span data-ttu-id="7f0cf-114">Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="7f0cf-114">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
  * <span data-ttu-id="7f0cf-115">**Typ** – zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-115">**Type** – backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
  * <span data-ttu-id="7f0cf-116">**Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-116">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
  * <span data-ttu-id="7f0cf-117">**Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-117">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="7f0cf-118">Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-118">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="7f0cf-119">V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
  * <span data-ttu-id="7f0cf-120">**Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-120">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="7f0cf-121">**Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-121">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="7f0cf-122">**Průběh** – hello procento dokončení běžící úlohy.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-122">**Progress** – hello percentage completion of a running job.</span></span> <span data-ttu-id="7f0cf-123">Dokončené úlohy to by měl být vždy 100 %.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="7f0cf-124">Hello seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="7f0cf-125">Můžete provádět následující akce související s úlohy na této stránce hello:</span><span class="sxs-lookup"><span data-stu-id="7f0cf-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="7f0cf-126">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="7f0cf-126">View job details</span></span>
* <span data-ttu-id="7f0cf-127">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="7f0cf-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="7f0cf-128">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="7f0cf-128">View job details</span></span>
<span data-ttu-id="7f0cf-129">Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="7f0cf-130">Podrobnosti úlohy tooview</span><span class="sxs-lookup"><span data-stu-id="7f0cf-130">tooview job details</span></span>
1. <span data-ttu-id="7f0cf-131">Na hello **úlohy** stránky, zobrazit hello úloh zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-131">On hello **Jobs** page, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="7f0cf-132">Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="7f0cf-133">Vyberte úlohu.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-133">Select a job.</span></span>
3. <span data-ttu-id="7f0cf-134">V dolní části hello hello stránky, klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-134">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="7f0cf-135">V hello **podrobnosti úlohy zálohování** dialogové okno, můžete zobrazit stav hello, podrobnosti, statistiku časových údajů a dat statistiky.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-135">In hello **Backup Job Details** dialog box, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Stránka Podrobnosti úlohy](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a><span data-ttu-id="7f0cf-137">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="7f0cf-137">Cancel a job</span></span>
<span data-ttu-id="7f0cf-138">Proveďte následující kroky toocancel spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-138">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="7f0cf-139">Některé úlohy, jako jsou úpravy typu svazku toochange hello svazku nebo rozšíření svazku, nelze zrušit.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-139">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>
> 
> 

### <a name="toocancel-a-job"></a><span data-ttu-id="7f0cf-140">toocancel úlohy</span><span class="sxs-lookup"><span data-stu-id="7f0cf-140">toocancel a job</span></span>
1. <span data-ttu-id="7f0cf-141">Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-141">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="7f0cf-142">Vyberte úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-142">Select hello job.</span></span>
3. <span data-ttu-id="7f0cf-143">V dolní části hello hello stránky, klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-143">At hello bottom of hello page, click **Cancel**.</span></span>
4. <span data-ttu-id="7f0cf-144">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-144">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="7f0cf-145">Tato úloha je nyní zrušena.</span><span class="sxs-lookup"><span data-stu-id="7f0cf-145">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f0cf-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f0cf-146">Next steps</span></span>
* <span data-ttu-id="7f0cf-147">Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7f0cf-147">Learn how too[manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="7f0cf-148">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7f0cf-148">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

