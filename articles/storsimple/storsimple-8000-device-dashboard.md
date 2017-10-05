---
title: "Souhrn zařízení řady StorSimple 8000 použití | Microsoft Docs"
description: "Popisuje zařízení služby StorSimple Manager zařízení, souhrn a způsobu jeho použití k zobrazení metriky úložiště a připojené iniciátory a najít sériové číslo a IQN."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 784d3ce9d8f926b00ac1c6fbf48a05c0b04f900a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="53643-103">Použití zařízení souhrnné ve službě service Manager zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="53643-103">Use the device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="53643-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="53643-104">Overview</span></span>
<span data-ttu-id="53643-105">V okně Souhrn zařízení StorSimple bude stručně charakterizovat informace pro určité zařízení StorSimple, na rozdíl od souhrnu okna služby, které nabízí informace o všech zařízeních, které jsou součástí vašeho řešení Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="53643-105">The StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast to the service summary blade, which gives you information about all the devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="53643-106">V okně Souhrn zařízení poskytuje přehled o zařízení řady StorSimple 8000, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému.</span><span class="sxs-lookup"><span data-stu-id="53643-106">The device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="53643-107">Tento kurz představuje souhrn okno zařízení, popisuje obsah a funkce a popisuje úlohy, které můžete provádět v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="53643-107">This tutorial introduces the device summary blade, explains the content and function, and describes the tasks that you can perform from this blade.</span></span>

<span data-ttu-id="53643-108">V okně Souhrn zařízení zobrazí následující informace:</span><span class="sxs-lookup"><span data-stu-id="53643-108">The device summary blade displays the following information:</span></span>

![Souhrn okno zařízení](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="53643-110">Správa příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="53643-110">Management command bar</span></span>

<span data-ttu-id="53643-111">V okně zařízení StorSimple zobrazí se možnosti pro správu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="53643-111">In the StorSimple device blade, you see the options for managing your StorSimple device.</span></span> <span data-ttu-id="53643-112">Příkazy pro správu zobrazí v horní části okna a na levé straně.</span><span class="sxs-lookup"><span data-stu-id="53643-112">You see the management commands across the top of the blade and on the left side.</span></span> <span data-ttu-id="53643-113">Pomocí těchto možností můžete přidat sdílené složky nebo svazky, nebo aktualizovat nebo selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="53643-113">Use these options to add shares or volumes, or update or fail over your device.</span></span>

![Správa příkazového řádku](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="53643-115">Základy</span><span class="sxs-lookup"><span data-stu-id="53643-115">Essentials</span></span>

<span data-ttu-id="53643-116">Oblasti essentials zaznamená některé důležité vlastnosti, jako, stav, modelu, cílový IQN a verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="53643-116">The essentials area captures some of the important properties such as, the status, model, target IQN, and the software version.</span></span> 

![Zařízení essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="53643-118">Monitorování</span><span class="sxs-lookup"><span data-stu-id="53643-118">Monitoring</span></span>

* <span data-ttu-id="53643-119">**Výstrahy** dlaždice poskytuje snímek všechny aktivní výstrahy pro vaše zařízení seskupené podle závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="53643-119">The **Alerts** tile provides a snapshot of all the active alerts for your device, grouped by alert severity.</span></span>

    ![Dlaždice výstrah](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="53643-121">Kliknutím na dlaždici otevřít **výstrahy** okna a potom klikněte na jednotlivé výstrahy zobrazíte další podrobnosti o této výstraze, včetně všech doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="53643-121">Click the tile to open the **Alerts** blade and then click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="53643-122">Pokud byl problém vyřešen, zrušte výstrahy.</span><span class="sxs-lookup"><span data-stu-id="53643-122">You can also clear the alert if the issue has been resolved.</span></span>

    ![Klikněte na dlaždici výstrah](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="53643-124">**Stav** dlaždice poskytuje přehled o stavu součásti hardwaru pro zařízení, včetně stavu zařízení.</span><span class="sxs-lookup"><span data-stu-id="53643-124">The **Status and health** tile provides insights into the hardware component health for a device including the device status.</span></span> <span data-ttu-id="53643-125">Stav zařízení může být offline, online, Deaktivované nebo připravení nastavit.</span><span class="sxs-lookup"><span data-stu-id="53643-125">The device status may be offline, online, deactivated, or ready to set up.</span></span>

    ![Dlaždice stavu a stavu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="53643-127">**Svazky** dlaždice poskytuje přehled o počtu svazků v zařízení seskupené podle stavu.</span><span class="sxs-lookup"><span data-stu-id="53643-127">The **Volumes** tile provides a summary of the number of volumes in your device grouped by status.</span></span>

    ![Dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="53643-129">Kliknutím na dlaždici otevřít **svazky** seznamu okna a potom klikněte na svazku lze zobrazit nebo upravit jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="53643-129">Click the tile to open the **Volumes** list blade, and then click on an individual volume to view or modify its properties.</span></span>
    
    ![Klikněte na dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="53643-131">Další informace najdete v tématu Jak [správu svazků](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="53643-131">For more information, see how to [manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="53643-132">V **využití** grafu, můžete zobrazit primárního úložiště používat v zařízení a cloudového úložiště spotřebované za posledních 7 dnů, výchozí časové období.</span><span class="sxs-lookup"><span data-stu-id="53643-132">In the **Usage** chart, you can view the primary storage used across your device, and the cloud storage consumed over the past 7 days, the default time period.</span></span>

     ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="53643-134">Chcete-li vybrat jinou dobu měřítko, použijte **upravit** možnost v pravém horním rohu grafu.</span><span class="sxs-lookup"><span data-stu-id="53643-134">To choose a different time scale, use the **Edit** option in the top-right corner of the chart.</span></span>

     ![Upravit graf využití](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="53643-136">V tomto grafu můžete zobrazit metrik pro celkový počet primárního úložiště (velikost dat zapsaných hostiteli k zařízení) a celkový počet cloudového úložiště spotřebovávají zařízení v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="53643-136">In this chart, you can view metrics for the total primary storage (the amount of data written by hosts to your device) and the total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="53643-137">V tomto kontextu *primárního úložiště* odkazuje na celkovém množství dat zapsaných hostitele a můžete rozdělit podle typu svazku: *primární vrstvené úložiště* zahrnuje obě místně uložená data a data vrstvené na v cloudu.</span><span class="sxs-lookup"><span data-stu-id="53643-137">In this context, *primary storage* refers to the total amount of data written by the host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered to the cloud.</span></span> <span data-ttu-id="53643-138">*Primární místně vázaný úložiště* zahrnuje jenom místně uložená data.</span><span class="sxs-lookup"><span data-stu-id="53643-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="53643-139">*Cloudového úložiště*, na druhé straně je měření celkové množství dat uložených v cloudu.</span><span class="sxs-lookup"><span data-stu-id="53643-139">*Cloud storage*, on the other hand, is a measurement of the total amount of data stored in the cloud.</span></span> <span data-ttu-id="53643-140">Toto úložiště zahrnuje vrstvený dat a zálohování.</span><span class="sxs-lookup"><span data-stu-id="53643-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="53643-141">Data uložená v cloudu je s odstraněním duplicitních dat a komprimované, zatímco primární úložiště určuje velikost úložiště využít, než je s odstraněním duplicitních dat a komprimované data.</span><span class="sxs-lookup"><span data-stu-id="53643-141">The data stored in the cloud is deduplicated and compressed, whereas primary storage indicates the amount of storage used before the data is deduplicated and compressed.</span></span> <span data-ttu-id="53643-142">(Těchto dvou čísel, kde získáte představu míry komprese můžete porovnat.) Pro obě primární a cloudového úložiště, hodnoty uvedené jsou na základě sledování četnosti nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="53643-142">(You can compare these two numbers to get an idea of the compression rate.) For both primary and cloud storage, the amounts shown are based on the tracking frequency you configure.</span></span> <span data-ttu-id="53643-143">Například pokud se rozhodnete frekvencí jeden týden, pak graf zobrazuje data pro každý den předchozího týdne.</span><span class="sxs-lookup"><span data-stu-id="53643-143">For example, if you choose a one week frequency, then the chart shows data for each day in the previous week.</span></span>

     <span data-ttu-id="53643-144">Pokud chcete zobrazit velikost úložiště v cloudu využívat v čase, vyberte **CLOUDOVÉ úložiště používá** možnost.</span><span class="sxs-lookup"><span data-stu-id="53643-144">To see the amount of cloud storage consumed over time, select the **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="53643-145">Pokud chcete zobrazit celkový úložiště, která byla vytvořena pro hostitele, vyberte **primární VRSTVENÉ úložiště používá** a **primární MÍSTNĚ PŘIPNUTÝ úložiště používá** možnosti.</span><span class="sxs-lookup"><span data-stu-id="53643-145">To see the total storage that has been written by the host, select the **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="53643-146">Další informace najdete v tématu [použít službu StorSimple Manager zařízení k monitorování zařízení StorSimple](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="53643-146">For more information, see [Use the StorSimple Device Manager service to monitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="53643-147">**Kapacity** dlaždice zobrazí primárního úložiště, která je zřízený a zbývající u všech zařízení relativně k celkové úložiště k dispozici pro stejné.</span><span class="sxs-lookup"><span data-stu-id="53643-147">The **Capacity** tile displays the primary storage that is provisioned and remaining across the device relative to the total storage available for the same.</span></span> <span data-ttu-id="53643-148">**Zřízení** odkazuje na velikost úložiště, která je připravená a přidělení k použití, **zbývající** odkazuje na zbývající kapacitu, kterou může být zřízen přes toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="53643-148">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across this device.</span></span> 

    ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="53643-150">Klikněte na tuto dlaždici zobrazit, jak je zřízený kapacitu mezi vrstvami a místně vázaných svazků.</span><span class="sxs-lookup"><span data-stu-id="53643-150">Click this tile to view how the capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="53643-151">**Zbývající vrstvené** kapacita je dostupné kapacity, který může být zřízen včetně cloudu, zatímco **zbývající místní** je kapacita zbývající u disků připojených k tomuto zařízení.</span><span class="sxs-lookup"><span data-stu-id="53643-151">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to this device.</span></span>

    ![Klikněte na graf využití](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="53643-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53643-153">Next steps</span></span>
* <span data-ttu-id="53643-154">Další informace o [souhrnu okna služby StorSimple](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="53643-154">Learn more about the [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="53643-155">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="53643-155">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

