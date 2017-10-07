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
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a>Použití zařízení hello souhrnné ve službě service Manager zařízení StorSimple

## <a name="overview"></a>Přehled
Souhrn okno pro zařízení StorSimple Hello poskytuje přehled informací pro určité zařízení StorSimple, na rozdíl od toohello služby souhrnné okno, které nabízí informace o všech zařízeních hello součástí řešení Microsoft Azure StorSimple.

Souhrn okno Hello zařízení poskytuje přehled o zařízení řady StorSimple 8000, které je registrované s danou StorSimple Správce zařízení, zvýraznění těchto problémů zařízení, které vyžadují pozornost. správce systému. Tento kurz představuje souhrn okno hello zařízení, popisuje hello obsah a funkce a popisuje hello úlohy, které můžete provádět v tomto okně.

Souhrn okno Hello zařízení zobrazí hello následující informace:

![Souhrn okno zařízení](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Správa příkazového řádku

V okně zařízení StorSimple hello zobrazí hello možnosti správy zařízení StorSimple. Zobrazí příkazy pro správu hello napříč hello horní části okna hello a na levé straně hello. Tyto možnosti tooadd sdílené složky nebo svazky, nebo aktualizovat nebo selhání zařízení.

![Správa příkazového řádku](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Základy

oblasti essentials Hello zaznamená některé hello důležité vlastnosti, jako, hello stav, modelu, cílový IQN a verze softwaru hello. 

![Zařízení essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>Monitorování

* Hello **výstrahy** dlaždice poskytuje snímek hello všechny aktivní výstrahy pro vaše zařízení seskupené podle závažnosti výstrah.

    ![Dlaždice výstrah](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy** okna a potom klikněte na konkrétního výstrahy tooview další podrobnosti o této výstraze, včetně všech doporučené akce. Pokud byl vyřešen problém hello, zrušte hello výstrahy.

    ![Klikněte na dlaždici výstrah](./media/storsimple-8000-device-dashboard/device-summary10.png)

* Hello **stav** dlaždice poskytuje přehledy o stav součásti hello hardwaru pro zařízení, včetně stavu zařízení hello. Stav zařízení Hello může být offline, online, Deaktivované nebo připravený tooset nahoru.

    ![Dlaždice stavu a stavu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* Hello **svazky** dlaždice poskytuje souhrn hello počtu svazků v zařízení seskupené podle stavu.

    ![Dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Klikněte na tlačítko hello dlaždice tooopen hello **svazky** seznamu okno a pak klikněte na jednotlivých svazku tooview nebo upravit její vlastnosti.
    
    ![Klikněte na dlaždice svazky](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Další informace najdete v tématu Jak příliš[správu svazků](storsimple-8000-manage-volumes-u2.md).

* V hello **využití** grafu, můžete zobrazit hello primárního úložiště používat v zařízení a úložiště v cloudu hello spotřebováno přes hello posledních 7 dnů, hello výchozí časové období.

     ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     toochoose různé časové rozmezí, použijte hello **upravit** možnost v pravém horním rohu hello hello grafu.

     ![Upravit graf využití](./media/storsimple-8000-device-dashboard/device-summary12.png)

     V tomto grafu, můžete zobrazit metriky pro hello celkový primárního úložiště (hello množství dat zapsaných zařízení tooyour hostitelů) a hello celkový cloudového úložiště spotřebovávají zařízení v časovém intervalu.
  
     V tomto kontextu *primárního úložiště* odkazuje toohello celkovou velikost dat zapsaných hello hostitele, přičemž se dají rozdělit podle typu svazku: *primární vrstvené úložiště* zahrnuje obě místně uložená data a data vrstvený toohello cloudu. *Primární místně vázaný úložiště* zahrnuje jenom místně uložená data. *Cloudového úložiště*, na druhé straně text hello, je měření hello celkové množství dat uložených v cloudu hello. Toto úložiště zahrnuje vrstvený dat a zálohování. Hello data uložená v cloudu hello je s odstraněním duplicitních dat a komprimované, zatímco primární úložiště označuje hello množství úložiště využít, než je s odstraněním duplicitních dat a komprimované hello data. (Můžete porovnat těchto dvou čísel tooget představu o míra komprese hello.) Pro obě primární a cloudového úložiště, hello hodnoty uvedené jsou založené na hello sledování frekvence nakonfigurujete. Například pokud se rozhodnete frekvencí jeden týden, pak hello graf znázorňuje dat pro každý den v hello předchozího týdne.

     toosee hello množství cloudového úložiště spotřebováno přes doby, vyberte hello **CLOUDOVÉ úložiště používá** možnost. hello toosee celkové se úložiště, která byla vytvořena hello hostitele, vyberte hello **primární VRSTVENÉ úložiště používá** a **primární MÍSTNĚ PŘIPNUTÝ úložiště používá** možnosti. 
     Další informace najdete v tématu [použití hello toomonitor service Manager zařízení StorSimple zařízení StorSimple](storsimple-monitor-device.md).


* Hello **kapacity** dlaždice zobrazí hello primární úložiště, které je zřízený a zbývající napříč hello zařízení relativní toohello celkové úložiště k dispozici pro hello stejné. **Zřízení** odkazuje toohello množství úložiště, které je připravené a přidělení k použití, **zbývající** odkazuje toohello zbývající kapacitu, kterou může být zřízen přes toto zařízení. 

    ![Dlaždice využití](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kliknutím na tuto dlaždici tooview jak zřízení hello kapacity mezi vrstvami a místně vázaných svazků. Hello **zbývající vrstvené** kapacita je hello dostupné kapacity, který může být zřízen včetně cloudu při hello **zbývající místní** je kapacita hello zbývající na hello disky připojené toothis zařízení.

    ![Klikněte na graf využití](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Další kroky
* Další informace o hello [souhrnu okna služby StorSimple](storsimple-8000-service-dashboard.md).
* Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

