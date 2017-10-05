---
title: "Souhrn okno zařízení pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje souhrnu okna zařízení ve Správci zařízení StorSimple a vysvětluje, jak použít jej k monitorování stavu pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 35413d597c3b6b1c7600241a78572b63f982d175
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a><span data-ttu-id="b707c-103">Použití okna souhrnu zařízení ve Správci zařízení StorSimple připojené k poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b707c-103">Use the device summary blade for StorSimple Device Manager connected to StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="b707c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b707c-104">Overview</span></span>

<span data-ttu-id="b707c-105">V okně zařízení StorSimple Manager zařízení obsahuje souhrnné zobrazení pole virtuální zařízení StorSimple, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému.</span><span class="sxs-lookup"><span data-stu-id="b707c-105">The StorSimple Device Manager device blade provides a summary view of a StorSimple Virtual Array that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="b707c-106">Tento kurz představuje souhrn okno zařízení, popisuje obsah a funkce a popisuje úlohy, které můžete provádět v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="b707c-106">This tutorial introduces the device summary blade, explains the content and function, and describes the tasks that you can perform from this blade.</span></span>

<span data-ttu-id="b707c-107">V okně Souhrn zařízení zobrazí následující informace:</span><span class="sxs-lookup"><span data-stu-id="b707c-107">The device summary blade displays the following information:</span></span>

![Řídicí panel zařízení](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a><span data-ttu-id="b707c-109">Správa</span><span class="sxs-lookup"><span data-stu-id="b707c-109">Management</span></span>

<span data-ttu-id="b707c-110">V okně zařízení StorSimple zobrazí se možnosti pro správu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b707c-110">In the StorSimple device blade, you see the options for managing your StorSimple device.</span></span> <span data-ttu-id="b707c-111">Příkazy pro správu zobrazí v horní části okna a na levé straně.</span><span class="sxs-lookup"><span data-stu-id="b707c-111">You see the management commands across the top of the blade and on the left side.</span></span> <span data-ttu-id="b707c-112">Pomocí těchto možností můžete přidat sdílené složky nebo svazky, nebo aktualizovat nebo převzít virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="b707c-112">Use these options to add shares or volumes, or update or fail over your virtual array.</span></span>

<span data-ttu-id="b707c-113">Oblasti essentials zaznamená některé důležité vlastnosti například stav, modelu, verzi softwaru, jakož i odkaz **webového uživatelského rozhraní** pole.</span><span class="sxs-lookup"><span data-stu-id="b707c-113">The essentials area captures some of the important properties such as, the status, model, software version as well as a link to the **Web UI** of the array.</span></span> <span data-ttu-id="b707c-114">Pokud jste v interní síti, můžete přímo spustit [místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md) ke správě virtuálních pole.</span><span class="sxs-lookup"><span data-stu-id="b707c-114">If you are on an internal network, you can directly launch the [local web UI](storsimple-ova-web-ui-admin.md) to administer your virtual array.</span></span>

![Zařízení essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a><span data-ttu-id="b707c-116">Souhrn zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b707c-116">StorSimple device summary</span></span>

* <span data-ttu-id="b707c-117">**Výstrahy** dlaždice poskytuje snímek všechny aktivní výstrahy pro vaše virtuální pole seskupené podle závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="b707c-117">The **Alerts** tile provides a snapshot of all the active alerts for your virtual array, grouped by alert severity.</span></span> <span data-ttu-id="b707c-118">Kliknutím na dlaždici otevřít **výstrahy** okna a potom klikněte na jednotlivé výstrahy zobrazíte další podrobnosti o této výstraze, včetně všech doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="b707c-118">Click the tile to open the **Alerts** blade and then click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="b707c-119">Pokud byl problém vyřešen, zrušte výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b707c-119">You can also clear the alert if the issue has been resolved.</span></span>

* <span data-ttu-id="b707c-120">**Kapacity** dlaždice zobrazí primární úložiště, které je zřízený a zbývající přes virtuální zařízení relativně k celkové úložiště k dispozici pro stejné.</span><span class="sxs-lookup"><span data-stu-id="b707c-120">The **Capacity** tile displays the primary storage that is provisioned and remaining across the virtual device relative to the total storage available for the same.</span></span> <span data-ttu-id="b707c-121">**Zřízení** odkazuje na velikost úložiště, která je připravená a přidělení k použití, **zbývající** odkazuje na zbývající kapacitu, kterou může být zřízen přes toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="b707c-121">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across this device.</span></span> <span data-ttu-id="b707c-122">**Zbývající vrstvené** kapacita dostupnou kapacitu, může být zřízen včetně cloudu, je při **zbývající místní** je kapacita zbývající na disky připojené k této virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="b707c-122">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to this virtual array.</span></span>

* <span data-ttu-id="b707c-123">V **využití** grafu, můžete zobrazit primárního úložiště používané v rámci celé virtuální pole, jakož i cloudového úložiště spotřebované za posledních 7 dnů, výchozí časové období.</span><span class="sxs-lookup"><span data-stu-id="b707c-123">In the **Usage** chart, you can view the primary storage used across your virtual array, as well as the cloud storage consumed  over the past 7 days, the default time period.</span></span> <span data-ttu-id="b707c-124">Použití **upravit** možnost v pravém horním rohu grafu vyberte jiné časové rozmezí.</span><span class="sxs-lookup"><span data-stu-id="b707c-124">Use the **Edit** option in the top-right corner of the chart to choose a different time scale.</span></span>

* <span data-ttu-id="b707c-125">**Sdílené složky** nebo **svazky** dlaždice obsahuje souhrn počtu sdílených složek nebo svazků v zařízení seskupené podle stavu.</span><span class="sxs-lookup"><span data-stu-id="b707c-125">The **Shares** or **Volumes** tile provides a summary of the number of shares or volumes in your device grouped by status.</span></span> <span data-ttu-id="b707c-126">Kliknutím na dlaždici otevřít **sdílené složky** nebo **svazky** seznamu okno a klikněte na jednotlivé sdílené složky nebo svazku lze zobrazit nebo upravit jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b707c-126">Click the tile to open the **Shares**  or **Volumes** list blade, and then click on an individual share or volume to view or modify its properties.</span></span> <span data-ttu-id="b707c-127">Další informace najdete v tématu Jak [spravovat sdílené složky](storsimple-virtual-array-manage-shares.md) nebo [správu svazků](storsimple-virtual-array-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="b707c-127">For more information, see how to [manage shares](storsimple-virtual-array-manage-shares.md) or [manage volumes](storsimple-virtual-array-manage-volumes.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b707c-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b707c-128">Next steps</span></span>
<span data-ttu-id="b707c-129">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="b707c-129">Learn how to:</span></span>
- [<span data-ttu-id="b707c-130">Spravovat sdílené složky v poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b707c-130">Manage shares on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-shares.md)
    
- [<span data-ttu-id="b707c-131">Správa svazků v poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b707c-131">Manage volumes on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-volumes.md)

