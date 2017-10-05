---
title: "Souhrn zařízení řady StorSimple 8000 použití | Microsoft Docs"
description: "Popisuje souhrnu okna služby StorSimple a vysvětluje, jak použít jej k monitorování stavu vašeho řešení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: d987a4ae170f21532a886552cbe1eb5a0d25fc3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a><span data-ttu-id="bb3b7-103">Pro zařízení řady StorSimple 8000 použít okno Souhrn služby</span><span class="sxs-lookup"><span data-stu-id="bb3b7-103">Use the service summary blade for StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="bb3b7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bb3b7-104">Overview</span></span>

<span data-ttu-id="bb3b7-105">V okně Souhrn služby StorSimple Manager zařízení poskytuje přehled o všech zařízení, které jsou připojené ke službě StorSimple Manager zařízení zvýraznění těchto zařízení, které vyžadují pozornost. správce systému.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-105">The StorSimple Device Manager service summary blade provides a summary view of all the devices that are connected to the StorSimple Device Manager service, highlighting those devices that need a system administrator's attention.</span></span> <span data-ttu-id="bb3b7-106">Tento kurz představuje souhrn okno služby, popisuje obsah řídicího panelu a funkce a popisuje úlohy, které můžete provést z této stránky.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-106">This tutorial introduces the service summary blade, explains the dashboard content and function, and describes the tasks that you can perform from this page.</span></span>

![Souhrn služby](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a><span data-ttu-id="bb3b7-108">Příkazy pro správu</span><span class="sxs-lookup"><span data-stu-id="bb3b7-108">Management commands</span></span>

<span data-ttu-id="bb3b7-109">V souhrnu okně služby StorSimple zobrazí možnosti pro správu služby StorSimple Manager zařízení a řadu zařízení StorSimple 8000 registrované k této službě.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-109">In the StorSimple service summary blade, you see the options for managing your StorSimple Device Manager service and the StorSimple 8000 series devices registered to this service.</span></span> <span data-ttu-id="bb3b7-110">Příkazy pro správu zobrazí v horní části okna a na levé straně.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-110">You see the management commands across the top of the blade and on the left side.</span></span>

![Panel příkazů](./media/storsimple-8000-service-dashboard/service-summary2.png)

<span data-ttu-id="bb3b7-112">Pomocí těchto možností můžete provádět různé operace, například jako přidat sdílené složky nebo svazky nebo monitorování různé úlohy běžící na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-112">Use these options to perform various operations such as add shares or volumes, or monitor the various jobs running on the StorSimple devices.</span></span>


## <a name="essentials"></a><span data-ttu-id="bb3b7-113">Základy</span><span class="sxs-lookup"><span data-stu-id="bb3b7-113">Essentials</span></span>

<span data-ttu-id="bb3b7-114">Oblasti essentials zaznamená některé důležité vlastnosti například skupinu prostředků, umístění a předplatné, ve kterém byla vytvořena vaše zařízení StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-114">The essentials area captures some of the important properties such as, the resource group, location, and subscription in which your StorSimple Device Manager was created.</span></span>

![Základy](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a><span data-ttu-id="bb3b7-116">Souhrn služby StorSimple Manager zařízení</span><span class="sxs-lookup"><span data-stu-id="bb3b7-116">StorSimple Device Manager service summary</span></span>

* <span data-ttu-id="bb3b7-117">**Výstrahy** dlaždice obsahuje snímek všech aktivních výstrah ve všech zařízeních, seskupené podle závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-117">The **Alerts** tile provides a snapshot of all the active alerts across all devices, grouped by alert severity.</span></span>

    ![Dlaždice výstrah](./media/storsimple-8000-service-dashboard/service-summary4.png)

    <span data-ttu-id="bb3b7-119">Kliknutím na dlaždici otevře **výstrahy** okno, kde můžete kliknout na jednotlivé výstrahy zobrazíte další podrobnosti o výstraze, včetně všech doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-119">Clicking the tile opens the **Alerts** blade, where you can click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="bb3b7-120">Pokud byl problém vyřešen, zrušte výstrahy.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-120">You can also clear the alert if the issue has been resolved.</span></span>

    ![Klikněte na dlaždici výstrahy](./media/storsimple-8000-service-dashboard/service-summary8.png)

* <span data-ttu-id="bb3b7-122">**Kapacity** zobrazí dlaždice zobrazuje primární úložiště, které je zřízený a zbývající ve všech zařízeních relativně k celkové úložiště k dispozici ve všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-122">The **Capacity** tile displays shows the primary storage that is provisioned and remaining across all devices relative to the total storage available across all devices.</span></span> <span data-ttu-id="bb3b7-123">**Zřízení** odkazuje na velikost úložiště, která je připravená a přidělení k použití, **zbývající** odkazuje na zbývající kapacitu, kterou může být zřízen ve všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-123">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across all devices.</span></span>

    ![Kapacity dlaždice](./media/storsimple-8000-service-dashboard/service-summary6.png)

    <span data-ttu-id="bb3b7-125">**Zbývající vrstvené** kapacita je dostupné kapacity, který může být zřízen včetně cloudu, zatímco **zbývající místní** je kapacita zbývající u disků připojených k řadu zařízení StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-125">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to the StorSimple 8000 series devices.</span></span>


* <span data-ttu-id="bb3b7-126">V **využití** grafu, můžete zobrazit relevantní metriky pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-126">In the **Usage** chart, you can see the relevant metrics for your devices.</span></span> <span data-ttu-id="bb3b7-127">Můžete zobrazit primárního úložiště používá na všech zařízeních a cloudového úložiště využívat zařízení za posledních 7 dnů, výchozí časové období.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-127">You can view the primary storage used across all devices, and the cloud storage consumed by devices over the past 7 days, the default time period.</span></span> 

    ![Dlaždice využití](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    <span data-ttu-id="bb3b7-129">Chcete-li vybrat jinou dobu měřítko, použijte **upravit** možnost v pravém horním rohu grafu.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-129">To choose a different time scale, use the **Edit** option in the top-right corner of the chart.</span></span>

     ![Klikněte na dlaždici využití](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Export dat grafu](./media/storsimple-8000-service-dashboard/service-summary11.png)

* <span data-ttu-id="bb3b7-132">**Zařízení** dlaždice obsahuje souhrn počtu řadu zařízení StorSimple 8000 ve vaší StorSimple Správci zařízení seskupené podle stavu zařízení.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-132">The **Devices** tile provides a summary of the number of StorSimple 8000 series devices in your StorSimple Device Manager grouped by device status.</span></span> 

    ![Dlaždice zařízení](./media/storsimple-8000-service-dashboard/service-summary5.png)

    <span data-ttu-id="bb3b7-134">Klikněte na tuto dlaždici otevřete **zařízení** okno a potom klikněte na k jednotlivým zařízením přejdete do souhrn mobilního zařízení, která je specifická pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-134">Click this tile to open the **Devices** list blade and then click an individual device to drill into the device summary specific to the device.</span></span> <span data-ttu-id="bb3b7-135">Můžete také provést akce konkrétní zařízení ze souhrnné okno daného zařízení.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-135">You can also perform device-specific actions from a given device summary blade.</span></span> <span data-ttu-id="bb3b7-136">Další informace o souhrnu okna zařízení, přejděte na [souhrnu okna zařízení](storsimple-8000-device-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="bb3b7-136">For more information about the device summary blade, go to [Device summary blade](storsimple-8000-device-dashboard.md).</span></span>

    ![Klikněte na dlaždici zařízení](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a><span data-ttu-id="bb3b7-138">Zobrazit protokoly aktivity</span><span class="sxs-lookup"><span data-stu-id="bb3b7-138">View the activity logs</span></span>

<span data-ttu-id="bb3b7-139">Chcete-li zobrazit různé operace, které provádějí v rámci vašeho správce zařízení StorSimple, klikněte na tlačítko **protokoly aktivity** odkaz na levé straně vaší okna Souhrn služby StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-139">To view the various operations carried out within your StorSimple Device Manager, click the **Activity logs** link on the left side of your StorSimple service summary blade.</span></span> <span data-ttu-id="bb3b7-140">Tím přejdete **protokoly aktivity** okno, kde se zobrazí souhrn posledních prováděných operací.</span><span class="sxs-lookup"><span data-stu-id="bb3b7-140">This takes you to the **Activity logs** blade, where you can see a summary of the recent operations carried out.</span></span>

![Protokoly aktivit](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a><span data-ttu-id="bb3b7-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb3b7-142">Next steps</span></span>

* <span data-ttu-id="bb3b7-143">Další informace o tom, jak [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bb3b7-143">Learn more about how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

