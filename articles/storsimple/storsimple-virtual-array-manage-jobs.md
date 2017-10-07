---
title: "aaaView a spravovat úlohy pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje stránku hello Správce zařízení StorSimple služby úlohy a jak toouse ho tootrack poslední a aktuální úlohy pro hello pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a><span data-ttu-id="6d559-103">Pomocí hello Správce zařízení StorSimple služby tooview úloh pro hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="6d559-103">Use hello StorSimple Device Manager service tooview jobs for hello StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="6d559-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6d559-104">Overview</span></span>
<span data-ttu-id="6d559-105">Hello **úlohy** okno poskytuje jednu centrální portál pro zobrazování a správu úloh, které jsou spuštěny na virtuální pole, které jsou připojené tooyour služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d559-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that are started on virtual arrays that are connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="6d559-106">Můžete zobrazit spuštěné, dokončené a k selhání úlohy pro více virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d559-106">You can view running, completed, and failed jobs for multiple virtual devices.</span></span> <span data-ttu-id="6d559-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="6d559-107">Results are presented in a tabular format.</span></span>

![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

<span data-ttu-id="6d559-109">Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:</span><span class="sxs-lookup"><span data-stu-id="6d559-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="6d559-110">**Čas rozsah** – úloh může být filtrovaná podle hello rozsah data a času.</span><span class="sxs-lookup"><span data-stu-id="6d559-110">**Time range** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="6d559-111">**Zařízení** – úloh se spouští na konkrétní zařízení, které jsou připojené tooyour službě.</span><span class="sxs-lookup"><span data-stu-id="6d559-111">**Devices** – Jobs are initiated on a specific device connected tooyour service.</span></span> <span data-ttu-id="6d559-112">Hello filtrované úlohy jsou pak v tabulce podle hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="6d559-112">hello filtered jobs are then tabulated based on hello following attributes:</span></span>
  
  * <span data-ttu-id="6d559-113">**Název** – může být název úlohy hello **všechny**, **zálohování**, **klon**, **převzetí služeb při selhání**, **stahování aktualizací** , nebo **nainstalovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6d559-113">**Name** – hello job name can be **All**, **Backup**, **Clone**, **Fail over**, **Download updates**, or **Install updates**.</span></span>
  * <span data-ttu-id="6d559-114">**Stav** – úloh může být **všechny**, **v průběhu**, **úspěšné**, nebo **se nezdařilo**, nebo **zrušeno**.</span><span class="sxs-lookup"><span data-stu-id="6d559-114">**Status** – Jobs can be **All**, **In progress**, **Succeeded**, or **Failed**, or **Canceled**.</span></span>
  * <span data-ttu-id="6d559-115">**Entity** – hello úloh může být přidružen svazek, sdílená složka nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d559-115">**Entity** – hello jobs can be associated with a volume, share, or device.</span></span>
  * <span data-ttu-id="6d559-116">**Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6d559-116">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="6d559-117">**Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.</span><span class="sxs-lookup"><span data-stu-id="6d559-117">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="6d559-118">**Doba trvání** – hello doba na která hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6d559-118">**Duration** – hello duration for on which hello job was run.</span></span>
* <span data-ttu-id="6d559-119">**Stav** – můžete vyhledat všechny, se nezdařila, dokončené nebo spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="6d559-119">**Status** – You can search for all, running, completed, or failed jobs.</span></span>
* <span data-ttu-id="6d559-120">**Typ úlohy** – typ úlohy hello být vše, zálohování, obnovení, převzetí služeb při selhání, stažení aktualizací nebo instalace aktualizací.</span><span class="sxs-lookup"><span data-stu-id="6d559-120">**Job type** – hello job type can be all, backup, restore, failover, download updates, or install updates.</span></span>

<span data-ttu-id="6d559-121">Hello seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6d559-121">hello list of jobs is refreshed every 30 seconds.</span></span>

## <a name="view-job-details"></a><span data-ttu-id="6d559-122">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="6d559-122">View job details</span></span>
<span data-ttu-id="6d559-123">Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="6d559-123">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="6d559-124">Podrobnosti úlohy tooview</span><span class="sxs-lookup"><span data-stu-id="6d559-124">tooview job details</span></span>
1. <span data-ttu-id="6d559-125">Na hello **úlohy** okno, zobrazení úloh hello zajímá spuštěním dotazu s odpovídající filtry.</span><span class="sxs-lookup"><span data-stu-id="6d559-125">On hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="6d559-126">Můžete vyhledat dokončené nebo spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="6d559-126">You can search for completed or running jobs.</span></span>
2. <span data-ttu-id="6d559-127">Vyberte úlohu z hello tabulkového seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="6d559-127">Select a job from hello tabular list of jobs.</span></span>
   
    ![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. <span data-ttu-id="6d559-129">V dolní části hello hello stránky, klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="6d559-129">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="6d559-130">V hello **podrobnosti** dialogové okno, můžete zobrazit stav, podrobnosti a statistiku časových údajů.</span><span class="sxs-lookup"><span data-stu-id="6d559-130">In hello **Details** dialog box, you can view status, details, and time statistics.</span></span> <span data-ttu-id="6d559-131">Hello následující obrázek ukazuje příklad hello **podrobnosti úlohy zálohování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d559-131">hello following illustration shows an example of hello **Backup Job Details** dialog box.</span></span>
   
    ![Podrobnosti úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a><span data-ttu-id="6d559-133">Selhání úlohy při hello virtuálního počítače je pozastaven v hypervisoru hello</span><span class="sxs-lookup"><span data-stu-id="6d559-133">Job failures when hello virtual machine is paused in hello hypervisor</span></span>
<span data-ttu-id="6d559-134">Když je úloha v průběhu na pole virtuální zařízení StorSimple a hello zařízení (virtuálního počítače v hypervisoru zřídit) je pozastavena pro více než 15 minut, úloha hello selže.</span><span class="sxs-lookup"><span data-stu-id="6d559-134">When a job is in progress on your StorSimple Virtual Array and hello device (virtual machine provisioned in hypervisor) is paused for greater than 15 minutes, hello job fails.</span></span> <span data-ttu-id="6d559-135">To je z důvodu tooyour pole virtuální zařízení StorSimple čas nejsou synchronizovány s časem hello Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6d559-135">This is due tooyour StorSimple Virtual Array time being out of sync with hello Microsoft Azure time.</span></span> 

<span data-ttu-id="6d559-136">Zobrazí se hello následující chybě: "váš čas zařízení není synchronizován s časem hello Microsoft Azure ve víc než 15 minut.</span><span class="sxs-lookup"><span data-stu-id="6d559-136">You will see hello following error: "Your device time is out of sync with hello Microsoft Azure time by more than 15 minutes.</span></span> <span data-ttu-id="6d559-137">Ujistěte se, že hello hypervisoru a hello zařízení časy jsou synchronizovány se server NTP.</span><span class="sxs-lookup"><span data-stu-id="6d559-137">Ensure that hello hypervisor and hello device times are synchronized with an NTP servier.</span></span> <span data-ttu-id="6d559-138">Ověřte, že neexistují žádné problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="6d559-138">Verify that there are no connectivity issues.</span></span> <span data-ttu-id="6d559-139">problémy s připojením tootroubleshoot, dat spuštění diagnostických testů z hello místního webového uživatelského rozhraní vašeho virtuální zařízení."</span><span class="sxs-lookup"><span data-stu-id="6d559-139">tootroubleshoot connectivity issues, run diagnostic tests from hello local web UI of your virtual device."</span></span>

<span data-ttu-id="6d559-140">Selhání použít toobackup, obnovení, aktualizace až po převzetí služeb při selhání úlohy.</span><span class="sxs-lookup"><span data-stu-id="6d559-140">These failures apply toobackup, restore, update, and failover jobs.</span></span> <span data-ttu-id="6d559-141">Pokud virtuální počítač je zajištěna v Hyper-V, počítač hello nakonec synchronizuje čas s vaší hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="6d559-141">If your virtual machine is provisioned in Hyper-V, hello machine eventually synchronizes time with your hypervisor.</span></span> <span data-ttu-id="6d559-142">Jakmile k tomu dojde, můžete ji znovu úlohu.</span><span class="sxs-lookup"><span data-stu-id="6d559-142">Once that happens, you can restart your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d559-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d559-143">Next steps</span></span>
<span data-ttu-id="6d559-144">[Zjistěte, jak toouse hello místního webového uživatelského rozhraní tooadminister pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="6d559-144">[Learn how toouse hello local web UI tooadminister your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

