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
# <a name="use-hello-device-summary-blade-for-storsimple-device-manager-connected-toostorsimple-virtual-array"></a>Použití hello souhrnu okna zařízení ve Správci zařízení StorSimple připojené tooStorSimple virtuální pole

## <a name="overview"></a>Přehled

okno zařízení StorSimple Manager zařízení Hello obsahuje souhrnné zobrazení pole virtuální zařízení StorSimple, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému. Tento kurz představuje souhrn okno hello zařízení, popisuje hello obsah a funkce a popisuje hello úlohy, které můžete provádět v tomto okně.

Souhrn okno Hello zařízení zobrazí hello následující informace:

![Řídicí panel zařízení](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Správa

V okně zařízení StorSimple hello zobrazí hello možnosti správy zařízení StorSimple. Zobrazí příkazy pro správu hello napříč hello horní části okna hello a na levé straně hello. Tyto možnosti tooadd sdílené složky nebo svazky, nebo aktualizovat nebo převzít virtuální pole.

Hello essentials oblasti zaznamená některé důležité vlastnosti hello jako, hello stav, modelu, verzi softwaru a také odkaz toohello **webového uživatelského rozhraní** hello pole. Pokud jste v interní síti, můžete spustit přímo hello [místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md) tooadminister virtuální pole.

![Zařízení essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>Souhrn zařízení StorSimple

* Hello **výstrahy** dlaždice poskytuje snímek hello všechny aktivní výstrahy pro vaše virtuální pole seskupené podle závažnosti výstrah. Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy** okna a potom klikněte na konkrétního výstrahy tooview další podrobnosti o této výstraze, včetně všech doporučené akce. Pokud byl vyřešen problém hello, zrušte hello výstrahy.

* Hello **kapacity** dlaždice zobrazí hello primární úložiště, které je zřízený a zbývající napříč hello virtuální zařízení relativní toohello celkové úložiště k dispozici pro hello stejné. **Zřízení** odkazuje toohello množství úložiště, které je připravené a přidělení k použití, **zbývající** odkazuje toohello zbývající kapacitu, kterou může být zřízen přes toto zařízení. Hello **zbývající vrstvené** kapacita je hello dostupné kapacity, který může být zřízen včetně cloudu při hello **zbývající místní** je kapacita hello zbývající na hello disky připojené toothis virtuální pole.

* V hello **využití** grafu, můžete zobrazit hello primárního úložiště používané v rámci celé vaší virtuální pole, jakož i úložiště v cloudu hello spotřebováno přes hello posledních 7 dnů, hello výchozí časové období. Použití hello **upravit** možnost v pravém horním rohu hello hello grafu toochoose jinou dobu škálování.

* Hello **sdílené složky** nebo **svazky** dlaždice poskytuje souhrn hello počet složek nebo svazků v zařízení seskupené podle stavu. Klikněte na tlačítko hello dlaždice tooopen hello **sdílené složky** nebo **svazky** seznamu okno a pak klikněte na jednotlivé sdílené složky nebo svazku tooview nebo upravit její vlastnosti. Další informace najdete v tématu Jak příliš[spravovat sdílené složky](storsimple-virtual-array-manage-shares.md) nebo [správu svazků](storsimple-virtual-array-manage-volumes.md).

## <a name="next-steps"></a>Další kroky
Naučte se:
- [Spravovat sdílené složky v poli virtuální zařízení StorSimple](storsimple-virtual-array-manage-shares.md)
    
- [Správa svazků v poli virtuální zařízení StorSimple](storsimple-virtual-array-manage-volumes.md)

