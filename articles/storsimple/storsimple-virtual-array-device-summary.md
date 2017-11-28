---
title: "Souhrn okno zařízení aaaStorSimple virtuální pole | Microsoft Docs"
description: "Popisuje souhrnu okna hello zařízení ve Správci zařízení StorSimple a vysvětluje, jak toouse ho toomonitor hello stav pole virtuální zařízení StorSimple."
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
ms.openlocfilehash: 3649eaac8a924a772f310a809ddf9706e912157a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-blade-for-storsimple-device-manager-connected-toostorsimple-virtual-array"></a><span data-ttu-id="f75a8-103">Použití hello souhrnu okna zařízení ve Správci zařízení StorSimple připojené tooStorSimple virtuální pole</span><span class="sxs-lookup"><span data-stu-id="f75a8-103">Use hello device summary blade for StorSimple Device Manager connected tooStorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="f75a8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f75a8-104">Overview</span></span>

<span data-ttu-id="f75a8-105">okno zařízení StorSimple Manager zařízení Hello obsahuje souhrnné zobrazení pole virtuální zařízení StorSimple, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému.</span><span class="sxs-lookup"><span data-stu-id="f75a8-105">hello StorSimple Device Manager device blade provides a summary view of a StorSimple Virtual Array that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="f75a8-106">Tento kurz představuje souhrn okno hello zařízení, popisuje hello obsah a funkce a popisuje hello úlohy, které můžete provádět v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="f75a8-106">This tutorial introduces hello device summary blade, explains hello content and function, and describes hello tasks that you can perform from this blade.</span></span>

<span data-ttu-id="f75a8-107">Souhrn okno Hello zařízení zobrazí hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="f75a8-107">hello device summary blade displays hello following information:</span></span>

![Řídicí panel zařízení](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a><span data-ttu-id="f75a8-109">Správa</span><span class="sxs-lookup"><span data-stu-id="f75a8-109">Management</span></span>

<span data-ttu-id="f75a8-110">V okně zařízení StorSimple hello zobrazí hello možnosti správy zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="f75a8-110">In hello StorSimple device blade, you see hello options for managing your StorSimple device.</span></span> <span data-ttu-id="f75a8-111">Zobrazí příkazy pro správu hello napříč hello horní části okna hello a na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="f75a8-111">You see hello management commands across hello top of hello blade and on hello left side.</span></span> <span data-ttu-id="f75a8-112">Tyto možnosti tooadd sdílené složky nebo svazky, nebo aktualizovat nebo převzít virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="f75a8-112">Use these options tooadd shares or volumes, or update or fail over your virtual array.</span></span>

<span data-ttu-id="f75a8-113">Hello essentials oblasti zaznamená některé důležité vlastnosti hello jako, hello stav, modelu, verzi softwaru a také odkaz toohello **webového uživatelského rozhraní** hello pole.</span><span class="sxs-lookup"><span data-stu-id="f75a8-113">hello essentials area captures some of hello important properties such as, hello status, model, software version as well as a link toohello **Web UI** of hello array.</span></span> <span data-ttu-id="f75a8-114">Pokud jste v interní síti, můžete spustit přímo hello [místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md) tooadminister virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="f75a8-114">If you are on an internal network, you can directly launch hello [local web UI](storsimple-ova-web-ui-admin.md) tooadminister your virtual array.</span></span>

![Zařízení essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a><span data-ttu-id="f75a8-116">Souhrn zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="f75a8-116">StorSimple device summary</span></span>

* <span data-ttu-id="f75a8-117">Hello **výstrahy** dlaždice poskytuje snímek hello všechny aktivní výstrahy pro vaše virtuální pole seskupené podle závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="f75a8-117">hello **Alerts** tile provides a snapshot of all hello active alerts for your virtual array, grouped by alert severity.</span></span> <span data-ttu-id="f75a8-118">Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy** okna a potom klikněte na konkrétního výstrahy tooview další podrobnosti o této výstraze, včetně všech doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="f75a8-118">Click hello tile tooopen hello **Alerts** blade and then click an individual alert tooview additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="f75a8-119">Pokud byl vyřešen problém hello, zrušte hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f75a8-119">You can also clear hello alert if hello issue has been resolved.</span></span>

* <span data-ttu-id="f75a8-120">Hello **kapacity** dlaždice zobrazí hello primární úložiště, které je zřízený a zbývající napříč hello virtuální zařízení relativní toohello celkové úložiště k dispozici pro hello stejné.</span><span class="sxs-lookup"><span data-stu-id="f75a8-120">hello **Capacity** tile displays hello primary storage that is provisioned and remaining across hello virtual device relative toohello total storage available for hello same.</span></span> <span data-ttu-id="f75a8-121">**Zřízení** odkazuje toohello množství úložiště, které je připravené a přidělení k použití, **zbývající** odkazuje toohello zbývající kapacitu, kterou může být zřízen přes toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="f75a8-121">**Provisioned** refers toohello amount of storage that is prepared and allocated for use, **Remaining** refers toohello remaining capacity that can be provisioned across this device.</span></span> <span data-ttu-id="f75a8-122">Hello **zbývající vrstvené** kapacita je hello dostupné kapacity, který může být zřízen včetně cloudu při hello **zbývající místní** je kapacita hello zbývající na hello disky připojené toothis virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="f75a8-122">hello **Remaining Tiered** capacity is hello available capacity that can be provisioned including cloud, while hello **Remaining Local** is hello capacity remaining on hello disks attached toothis virtual array.</span></span>

* <span data-ttu-id="f75a8-123">V hello **využití** grafu, můžete zobrazit hello primárního úložiště používané v rámci celé vaší virtuální pole, jakož i úložiště v cloudu hello spotřebováno přes hello posledních 7 dnů, hello výchozí časové období.</span><span class="sxs-lookup"><span data-stu-id="f75a8-123">In hello **Usage** chart, you can view hello primary storage used across your virtual array, as well as hello cloud storage consumed  over hello past 7 days, hello default time period.</span></span> <span data-ttu-id="f75a8-124">Použití hello **upravit** možnost v pravém horním rohu hello hello grafu toochoose jinou dobu škálování.</span><span class="sxs-lookup"><span data-stu-id="f75a8-124">Use hello **Edit** option in hello top-right corner of hello chart toochoose a different time scale.</span></span>

* <span data-ttu-id="f75a8-125">Hello **sdílené složky** nebo **svazky** dlaždice poskytuje souhrn hello počet složek nebo svazků v zařízení seskupené podle stavu.</span><span class="sxs-lookup"><span data-stu-id="f75a8-125">hello **Shares** or **Volumes** tile provides a summary of hello number of shares or volumes in your device grouped by status.</span></span> <span data-ttu-id="f75a8-126">Klikněte na tlačítko hello dlaždice tooopen hello **sdílené složky** nebo **svazky** seznamu okno a pak klikněte na jednotlivé sdílené složky nebo svazku tooview nebo upravit její vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f75a8-126">Click hello tile tooopen hello **Shares**  or **Volumes** list blade, and then click on an individual share or volume tooview or modify its properties.</span></span> <span data-ttu-id="f75a8-127">Další informace najdete v tématu Jak příliš[spravovat sdílené složky](storsimple-virtual-array-manage-shares.md) nebo [správu svazků](storsimple-virtual-array-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="f75a8-127">For more information, see how too[manage shares](storsimple-virtual-array-manage-shares.md) or [manage volumes](storsimple-virtual-array-manage-volumes.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f75a8-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f75a8-128">Next steps</span></span>
<span data-ttu-id="f75a8-129">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="f75a8-129">Learn how to:</span></span>
- [<span data-ttu-id="f75a8-130">Spravovat sdílené složky v poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="f75a8-130">Manage shares on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-shares.md)
    
- [<span data-ttu-id="f75a8-131">Správa svazků v poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="f75a8-131">Manage volumes on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-volumes.md)

