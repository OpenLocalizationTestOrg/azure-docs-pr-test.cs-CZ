---
title: "souhrn zařízení řady StorSimple 8000 aaaUse | Microsoft Docs"
description: "Popisuje zařízení služby StorSimple Manager zařízení hello souhrnné a jak toouse ho tooview úložiště metriky a připojené iniciátory a najít hello sériové číslo a IQN."
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
ms.openlocfilehash: b45ffc6ec52ebb6549c25a00c68c62460b208b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="c7eeb-103">Použití zařízení hello souhrnné ve službě service Manager zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="c7eeb-103">Use hello device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="c7eeb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c7eeb-104">Overview</span></span>
<span data-ttu-id="c7eeb-105">Souhrn okno pro zařízení StorSimple Hello poskytuje přehled informací pro určité zařízení StorSimple, na rozdíl od toohello služby souhrnné okno, které nabízí informace o všech zařízeních hello součástí řešení Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-105">hello StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast toohello service summary blade, which gives you information about all hello devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="c7eeb-106">Souhrn okno Hello zařízení poskytuje přehled o zařízení řady StorSimple 8000, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-106">hello device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="c7eeb-107">Tento kurz představuje souhrn okno hello zařízení, popisuje hello obsah a funkce a popisuje hello úlohy, které můžete provádět v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-107">This tutorial introduces hello device summary blade, explains hello content and function, and describes hello tasks that you can perform from this blade.</span></span>

<span data-ttu-id="c7eeb-108">Souhrn okno Hello zařízení zobrazí hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="c7eeb-108">hello device summary blade displays hello following information:</span></span>

![Souhrn okno zařízení](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="c7eeb-110">Správa příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c7eeb-110">Management command bar</span></span>

<span data-ttu-id="c7eeb-111">V okně zařízení StorSimple hello zobrazí hello možnosti správy zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-111">In hello StorSimple device blade, you see hello options for managing your StorSimple device.</span></span> <span data-ttu-id="c7eeb-112">Zobrazí příkazy pro správu hello napříč hello horní části okna hello a na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-112">You see hello management commands across hello top of hello blade and on hello left side.</span></span> <span data-ttu-id="c7eeb-113">Tyto možnosti tooadd sdílené složky nebo svazky, nebo aktualizovat nebo selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-113">Use these options tooadd shares or volumes, or update or fail over your device.</span></span>

![Správa příkazového řádku](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="c7eeb-115">Základy</span><span class="sxs-lookup"><span data-stu-id="c7eeb-115">Essentials</span></span>

<span data-ttu-id="c7eeb-116">oblasti essentials Hello zaznamená některé hello důležité vlastnosti, jako, hello stav, modelu, cílový IQN a verze softwaru hello.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-116">hello essentials area captures some of hello important properties such as, hello status, model, target IQN, and hello software version.</span></span> 

![Zařízení essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="c7eeb-118">Monitorování</span><span class="sxs-lookup"><span data-stu-id="c7eeb-118">Monitoring</span></span>

* <span data-ttu-id="c7eeb-119">Hello **výstrahy** dlaždice poskytuje snímek hello všechny aktivní výstrahy pro vaše zařízení seskupené podle závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-119">hello **Alerts** tile provides a snapshot of all hello active alerts for your device, grouped by alert severity.</span></span>

    ![Dlaždice výstrah](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="c7eeb-121">Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy** okna a potom klikněte na konkrétního výstrahy tooview další podrobnosti o této výstraze, včetně všech doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-121">Click hello tile tooopen hello **Alerts** blade and then click an individual alert tooview additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="c7eeb-122">Pokud byl vyřešen problém hello, zrušte hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-122">You can also clear hello alert if hello issue has been resolved.</span></span>

    ![Klikněte na dlaždici výstrah](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="c7eeb-124">Hello **stav** dlaždice poskytuje přehledy o stav součásti hello hardwaru pro zařízení, včetně stavu zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-124">hello **Status and health** tile provides insights into hello hardware component health for a device including hello device status.</span></span> <span data-ttu-id="c7eeb-125">Stav zařízení Hello může být offline, online, Deaktivované nebo připravený tooset nahoru.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-125">hello device status may be offline, online, deactivated, or ready tooset up.</span></span>

    ![Dlaždice stavu a stavu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="c7eeb-127">Hello **svazky** dlaždice poskytuje souhrn hello počtu svazků v zařízení seskupené podle stavu.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-127">hello **Volumes** tile provides a summary of hello number of volumes in your device grouped by status.</span></span>

    ![Dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="c7eeb-129">Klikněte na tlačítko hello dlaždice tooopen hello **svazky** seznamu okno a pak klikněte na jednotlivých svazku tooview nebo upravit její vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-129">Click hello tile tooopen hello **Volumes** list blade, and then click on an individual volume tooview or modify its properties.</span></span>
    
    ![Klikněte na dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="c7eeb-131">Další informace najdete v tématu Jak příliš[správu svazků](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c7eeb-131">For more information, see how too[manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="c7eeb-132">V hello **využití** grafu, můžete zobrazit hello primárního úložiště používat v zařízení a úložiště v cloudu hello spotřebováno přes hello posledních 7 dnů, hello výchozí časové období.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-132">In hello **Usage** chart, you can view hello primary storage used across your device, and hello cloud storage consumed over hello past 7 days, hello default time period.</span></span>

     ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="c7eeb-134">toochoose různé časové rozmezí, použijte hello **upravit** možnost v pravém horním rohu hello hello grafu.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-134">toochoose a different time scale, use hello **Edit** option in hello top-right corner of hello chart.</span></span>

     ![Upravit graf využití](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="c7eeb-136">V tomto grafu, můžete zobrazit metriky pro hello celkový primárního úložiště (hello množství dat zapsaných zařízení tooyour hostitelů) a hello celkový cloudového úložiště spotřebovávají zařízení v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-136">In this chart, you can view metrics for hello total primary storage (hello amount of data written by hosts tooyour device) and hello total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="c7eeb-137">V tomto kontextu *primárního úložiště* odkazuje toohello celkovou velikost dat zapsaných hello hostitele, přičemž se dají rozdělit podle typu svazku: *primární vrstvené úložiště* zahrnuje obě místně uložená data a data vrstvený toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-137">In this context, *primary storage* refers toohello total amount of data written by hello host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered toohello cloud.</span></span> <span data-ttu-id="c7eeb-138">*Primární místně vázaný úložiště* zahrnuje jenom místně uložená data.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="c7eeb-139">*Cloudového úložiště*, na druhé straně text hello, je měření hello celkové množství dat uložených v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-139">*Cloud storage*, on hello other hand, is a measurement of hello total amount of data stored in hello cloud.</span></span> <span data-ttu-id="c7eeb-140">Toto úložiště zahrnuje vrstvený dat a zálohování.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="c7eeb-141">Hello data uložená v cloudu hello je s odstraněním duplicitních dat a komprimované, zatímco primární úložiště označuje hello množství úložiště využít, než je s odstraněním duplicitních dat a komprimované hello data.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-141">hello data stored in hello cloud is deduplicated and compressed, whereas primary storage indicates hello amount of storage used before hello data is deduplicated and compressed.</span></span> <span data-ttu-id="c7eeb-142">(Můžete porovnat těchto dvou čísel tooget představu o míra komprese hello.) Pro obě primární a cloudového úložiště, hello hodnoty uvedené jsou založené na hello sledování frekvence nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-142">(You can compare these two numbers tooget an idea of hello compression rate.) For both primary and cloud storage, hello amounts shown are based on hello tracking frequency you configure.</span></span> <span data-ttu-id="c7eeb-143">Například pokud se rozhodnete frekvencí jeden týden, pak hello graf znázorňuje dat pro každý den v hello předchozího týdne.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-143">For example, if you choose a one week frequency, then hello chart shows data for each day in hello previous week.</span></span>

     <span data-ttu-id="c7eeb-144">toosee hello množství cloudového úložiště spotřebováno přes doby, vyberte hello **CLOUDOVÉ úložiště používá** možnost.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-144">toosee hello amount of cloud storage consumed over time, select hello **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="c7eeb-145">hello toosee celkové se úložiště, která byla vytvořena hello hostitele, vyberte hello **primární VRSTVENÉ úložiště používá** a **primární MÍSTNĚ PŘIPNUTÝ úložiště používá** možnosti.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-145">toosee hello total storage that has been written by hello host, select hello **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="c7eeb-146">Další informace najdete v tématu [použití hello toomonitor service Manager zařízení StorSimple zařízení StorSimple](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="c7eeb-146">For more information, see [Use hello StorSimple Device Manager service toomonitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="c7eeb-147">Hello **kapacity** dlaždice zobrazí hello primární úložiště, které je zřízený a zbývající napříč hello zařízení relativní toohello celkové úložiště k dispozici pro hello stejné.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-147">hello **Capacity** tile displays hello primary storage that is provisioned and remaining across hello device relative toohello total storage available for hello same.</span></span> <span data-ttu-id="c7eeb-148">**Zřízení** odkazuje toohello množství úložiště, které je připravené a přidělení k použití, **zbývající** odkazuje toohello zbývající kapacitu, kterou může být zřízen přes toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-148">**Provisioned** refers toohello amount of storage that is prepared and allocated for use, **Remaining** refers toohello remaining capacity that can be provisioned across this device.</span></span> 

    ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="c7eeb-150">Kliknutím na tuto dlaždici tooview jak zřízení hello kapacity mezi vrstvami a místně vázaných svazků.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-150">Click this tile tooview how hello capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="c7eeb-151">Hello **zbývající vrstvené** kapacita je hello dostupné kapacity, který může být zřízen včetně cloudu při hello **zbývající místní** je kapacita hello zbývající na hello disky připojené toothis zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7eeb-151">hello **Remaining Tiered** capacity is hello available capacity that can be provisioned including cloud, while hello **Remaining Local** is hello capacity remaining on hello disks attached toothis device.</span></span>

    ![Klikněte na graf využití](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="c7eeb-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7eeb-153">Next steps</span></span>
* <span data-ttu-id="c7eeb-154">Další informace o hello [souhrnu okna služby StorSimple](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="c7eeb-154">Learn more about hello [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="c7eeb-155">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c7eeb-155">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

