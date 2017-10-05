---
title: "Zobrazit a spravovat úlohy pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager zařízení a způsobu jeho použití ke sledování poslední a aktuální úlohy pro pole virtuální zařízení StorSimple."
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
ms.openlocfilehash: 3fd1c262a8ce94d8e98f2b066a8028d974b15b1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a><span data-ttu-id="9adc3-103">Zobrazit úlohy pro pole virtuální zařízení StorSimple pomocí služby StorSimple Manager zařízení</span><span class="sxs-lookup"><span data-stu-id="9adc3-103">Use the StorSimple Device Manager service to view jobs for the StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="9adc3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9adc3-104">Overview</span></span>
<span data-ttu-id="9adc3-105">**Úlohy** okno poskytuje jednu centrální portál pro zobrazování a správu úloh, které jsou spuštěny na virtuální pole, které jsou připojené k vaší službě StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="9adc3-105">The **Jobs** blade provides a single central portal for viewing and managing jobs that are started on virtual arrays that are connected to your StorSimple Device Manager service.</span></span> <span data-ttu-id="9adc3-106">Můžete zobrazit spuštěné, dokončené a k selhání úlohy pro více virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="9adc3-106">You can view running, completed, and failed jobs for multiple virtual devices.</span></span> <span data-ttu-id="9adc3-107">Výsledky se zobrazí v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="9adc3-107">Results are presented in a tabular format.</span></span>

![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

<span data-ttu-id="9adc3-109">Můžete rychle najít úlohy, které zajímá pomocí filtrování na pole, jako například:</span><span class="sxs-lookup"><span data-stu-id="9adc3-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="9adc3-110">**Čas rozsah** – úlohy je možné filtrovat podle rozsahu datum a čas.</span><span class="sxs-lookup"><span data-stu-id="9adc3-110">**Time range** – Jobs can be filtered based on the date and time range.</span></span>
* <span data-ttu-id="9adc3-111">**Zařízení** – úloh se spouští na konkrétní zařízení připojena k službě.</span><span class="sxs-lookup"><span data-stu-id="9adc3-111">**Devices** – Jobs are initiated on a specific device connected to your service.</span></span> <span data-ttu-id="9adc3-112">Filtrované úlohy jsou pak poskytovalo na základě následujících atributů:</span><span class="sxs-lookup"><span data-stu-id="9adc3-112">The filtered jobs are then tabulated based on the following attributes:</span></span>
  
  * <span data-ttu-id="9adc3-113">**Název** – název úlohy může být **všechny**, **zálohování**, **klon**, **převzetí služeb při selhání**, **stahování aktualizací**, nebo **nainstalovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9adc3-113">**Name** – The job name can be **All**, **Backup**, **Clone**, **Fail over**, **Download updates**, or **Install updates**.</span></span>
  * <span data-ttu-id="9adc3-114">**Stav** – úloh může být **všechny**, **v průběhu**, **úspěšné**, nebo **se nezdařilo**, nebo **zrušeno**.</span><span class="sxs-lookup"><span data-stu-id="9adc3-114">**Status** – Jobs can be **All**, **In progress**, **Succeeded**, or **Failed**, or **Canceled**.</span></span>
  * <span data-ttu-id="9adc3-115">**Entity** – úloh může být přidružen svazek, sdílená složka nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="9adc3-115">**Entity** – The jobs can be associated with a volume, share, or device.</span></span>
  * <span data-ttu-id="9adc3-116">**Zařízení** – název zařízení, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9adc3-116">**Device** – The name of the device on which the job was started.</span></span>
  * <span data-ttu-id="9adc3-117">**Bylo zahájeno** – čas zahájení úlohy.</span><span class="sxs-lookup"><span data-stu-id="9adc3-117">**Started on** – The time when the job was started.</span></span>
  * <span data-ttu-id="9adc3-118">**Doba trvání** – dobu trvání pro na úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9adc3-118">**Duration** – The duration for on which the job was run.</span></span>
* <span data-ttu-id="9adc3-119">**Stav** – můžete vyhledat všechny, se nezdařila, dokončené nebo spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="9adc3-119">**Status** – You can search for all, running, completed, or failed jobs.</span></span>
* <span data-ttu-id="9adc3-120">**Typ úlohy** – typ úlohy může být vše, zálohování, obnovení, převzetí služeb při selhání, stáhnout aktualizace nebo instalace aktualizací.</span><span class="sxs-lookup"><span data-stu-id="9adc3-120">**Job type** – The job type can be all, backup, restore, failover, download updates, or install updates.</span></span>

<span data-ttu-id="9adc3-121">Seznam úloh se aktualizují každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9adc3-121">The list of jobs is refreshed every 30 seconds.</span></span>

## <a name="view-job-details"></a><span data-ttu-id="9adc3-122">Zobrazení podrobností o úloze</span><span class="sxs-lookup"><span data-stu-id="9adc3-122">View job details</span></span>
<span data-ttu-id="9adc3-123">Proveďte následující kroky, chcete-li zobrazit podrobnosti libovolné úlohy.</span><span class="sxs-lookup"><span data-stu-id="9adc3-123">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="9adc3-124">Chcete-li zobrazit podrobnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="9adc3-124">To view job details</span></span>
1. <span data-ttu-id="9adc3-125">Na **úlohy** okně zobrazení úloh se zajímá spuštěním dotazu s příslušné filtry.</span><span class="sxs-lookup"><span data-stu-id="9adc3-125">On the **Jobs** blade, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="9adc3-126">Můžete vyhledat dokončené nebo spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="9adc3-126">You can search for completed or running jobs.</span></span>
2. <span data-ttu-id="9adc3-127">Vyberte úlohu z tabulkového seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="9adc3-127">Select a job from the tabular list of jobs.</span></span>
   
    ![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. <span data-ttu-id="9adc3-129">V dolní části stránky klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="9adc3-129">At the bottom of the page, click **Details**.</span></span>
4. <span data-ttu-id="9adc3-130">V **podrobnosti** dialogové okno, můžete zobrazit stav, podrobnosti a statistiku časových údajů.</span><span class="sxs-lookup"><span data-stu-id="9adc3-130">In the **Details** dialog box, you can view status, details, and time statistics.</span></span> <span data-ttu-id="9adc3-131">Následující obrázek ukazuje příklad **podrobnosti úlohy zálohování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9adc3-131">The following illustration shows an example of the **Backup Job Details** dialog box.</span></span>
   
    ![Podrobnosti úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a><span data-ttu-id="9adc3-133">Selhání úlohy při virtuálního počítače je pozastaven v hypervisoru</span><span class="sxs-lookup"><span data-stu-id="9adc3-133">Job failures when the virtual machine is paused in the hypervisor</span></span>
<span data-ttu-id="9adc3-134">Když je úloha v průběhu na pole virtuální zařízení StorSimple a zařízení (virtuálního počítače v hypervisoru zřídit) je pozastavena pro více než 15 minut, úloha selže.</span><span class="sxs-lookup"><span data-stu-id="9adc3-134">When a job is in progress on your StorSimple Virtual Array and the device (virtual machine provisioned in hypervisor) is paused for greater than 15 minutes, the job fails.</span></span> <span data-ttu-id="9adc3-135">To je z důvodu doby pole virtuální zařízení StorSimple nejsou synchronizovány s časem Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9adc3-135">This is due to your StorSimple Virtual Array time being out of sync with the Microsoft Azure time.</span></span> 

<span data-ttu-id="9adc3-136">Zobrazí se následující chyba: "váš čas zařízení není synchronizován s časem Microsoft Azure ve víc než 15 minut.</span><span class="sxs-lookup"><span data-stu-id="9adc3-136">You will see the following error: "Your device time is out of sync with the Microsoft Azure time by more than 15 minutes.</span></span> <span data-ttu-id="9adc3-137">Zajistěte, aby hypervisoru a zařízení, které jsou synchronizovány časy se server NTP.</span><span class="sxs-lookup"><span data-stu-id="9adc3-137">Ensure that the hypervisor and the device times are synchronized with an NTP servier.</span></span> <span data-ttu-id="9adc3-138">Ověřte, že neexistují žádné problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="9adc3-138">Verify that there are no connectivity issues.</span></span> <span data-ttu-id="9adc3-139">Chcete-li vyřešit problémy s připojením, spustit diagnostické testy z místního webového uživatelského rozhraní vašeho virtuální zařízení."</span><span class="sxs-lookup"><span data-stu-id="9adc3-139">To troubleshoot connectivity issues, run diagnostic tests from the local web UI of your virtual device."</span></span>

<span data-ttu-id="9adc3-140">Tyto chyby se vztahují na úlohy zálohování, obnovení, aktualizace a převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9adc3-140">These failures apply to backup, restore, update, and failover jobs.</span></span> <span data-ttu-id="9adc3-141">Pokud zřízení virtuálního počítače technologie Hyper-V na počítač nakonec synchronizuje čas s vaší hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="9adc3-141">If your virtual machine is provisioned in Hyper-V, the machine eventually synchronizes time with your hypervisor.</span></span> <span data-ttu-id="9adc3-142">Jakmile k tomu dojde, můžete ji znovu úlohu.</span><span class="sxs-lookup"><span data-stu-id="9adc3-142">Once that happens, you can restart your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9adc3-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9adc3-143">Next steps</span></span>
<span data-ttu-id="9adc3-144">[Další informace o použití místního webového uživatelského rozhraní pro správu pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="9adc3-144">[Learn how to use the local web UI to administer your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

