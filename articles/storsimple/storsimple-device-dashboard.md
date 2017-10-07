---
title: "aaaUse řídicí panel zařízení StorSimple Manager hello | Microsoft Docs"
description: "Popisuje hello řídicí panel zařízení služby StorSimple Manager a jak toouse ho tooview úložiště metriky a připojené iniciátory a najít hello sériové číslo a IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e213fc0a081c21b9d6b408a3dd845cc93a31e250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-dashboard-in-storsimple-manager-service"></a>Použití hello zařízení řídicího panelu služby StorSimple Manager  

## <a name="overview"></a>Přehled
řídicí panel zařízení StorSimple Manager Hello poskytuje přehled informací pro určité zařízení StorSimple, na rozdíl od toohello služby řídicí panel, který poskytuje informace o všech zařízeních hello součástí řešení Microsoft Azure StorSimple.

![Stránka řídicího panelu zařízení](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

řídicí panel Hello obsahuje hello následující informace:

* **Oblast grafu** – můžete zobrazit hello relevantní úložiště metriky v oblasti grafu hello hello horní části řídicího panelu hello. V tomto grafu, můžete zobrazit metriky pro hello celkový primárního úložiště (hello množství dat zapsaných zařízení tooyour hostitelů) a hello celkový cloudového úložiště spotřebovávají zařízení v časovém intervalu.
  
     V tomto kontextu *primárního úložiště* odkazuje toohello celkovou velikost dat zapsaných hello hostitele, přičemž se dají rozdělit podle typu svazku: *primární vrstvené úložiště* zahrnuje obě místně uložená data a data vrstvený toohello cloudu; *primární místně vázaný úložiště* zahrnuje jenom místně uložená data. *Cloudového úložiště*, na druhé straně text hello, je měření hello celkové množství dat uložených v cloudu hello. To zahrnuje vrstvený dat a zálohování. Upozorňujeme, že se data uložená v cloudu hello s odstraněním duplicitních dat a komprimován, zatímco primárního úložiště označuje hello množství úložiště využít, než je s odstraněním duplicitních dat a komprimované hello data. (Můžete porovnat těchto dvou čísel tooget představu o míra komprese hello.) Pro obě primární a cloudového úložiště, hello hodnoty uvedené budou založeny na hello sledování frekvence nakonfigurujete. Například pokud se rozhodnete frekvencí jeden týden, pak hello grafu zobrazí data pro každý den v hello předchozího týdne.
  
     Graf hello můžete nakonfigurovat následujícím způsobem:
  
  * toosee hello množství cloudového úložiště spotřebováno přes doby, vyberte hello **CLOUDOVÉ úložiště používá** možnost. hello toosee celkové se úložiště, která byla vytvořena hello hostitele, vyberte hello **primární VRSTVENÉ úložiště používá** a **primární MÍSTNĚ PŘIPNUTÝ úložiště používá** možnosti. Hello obrázku jsou vybrány obě možnosti. proto hello graf znázorňuje objemy úložiště pro cloud a primárního úložiště. Jakékoli primární úložiště použity předchozí tooinstalling Update 2 je reprezentována hello **primární VRSTVENÉ úložiště používá** řádku.
  * Použití hello rozevírací nabídky v pravém horním rohu hello hello grafu toospecify 1 týden, měsíc 1, 3 měsíců nebo 1 rok časové období. Všimněte si, že hello nejvyšší úrovně grafu je aktualizovat jenom jednou za den a proto bude odrážet hello součty předchozí den.
    
    Další informace najdete v tématu [použití hello toomonitor služby StorSimple Manager zařízení StorSimple](storsimple-monitor-device.md).
* **Přehled využití** – v hello **přehled využití** oblasti, uvidíte hello množství primárního úložiště používá, množství hello zřízené úložiště a hello maximální kapacita úložiště pro vaše zařízení. Tak, že porovnáte tyto využití čísla toohello maximální velikost úložiště, které je k dispozici, zobrazí se na první pohled Pokud potřebujete další úložiště tooobtain. Všimněte si, že tento přehled je aktualizováno každých 15 minut a z důvodu hello rozdíl v četnosti aktualizace, může zobrazovat odlišné počty než ty zobrazené v hello oblasti grafu výše, která se aktualizuje denně. Další informace najdete v tématu [použití hello toomonitor služby StorSimple Manager zařízení StorSimple](storsimple-monitor-device.md).
* **Výstrahy** – hello **výstrahy** oblasti obsahuje přehled hello výstrahy pro vaše zařízení. Výstrahy jsou seskupené podle závažnosti a počet zajišťuje hello počtu výstrah na jednotlivé úrovně závažnosti. Kliknutím na tlačítko hello výstraha, že závažnost otevře vymezenou zobrazení hello výstrahy karta tooshow, že pouze hello výstrahy této úrovně závažnosti pro toto zařízení.
* **Úlohy** – hello **úlohy** oblasti ukazuje hello výsledek poslední aktivitu na úlohu. To můžete zajistit, hello systému funguje podle očekávání, nebo ho můžete nechat vás informuje, že je potřeba tootake opravné akce. Klikněte na další informace o nedávno dokončené úlohy toosee **úspěšné úlohy v hello posledních 24 hodin**.
* Hello **rychlého přehledu** oblast na hello napravo od hello řídicí panel poskytuje užitečné informace, jako je model zařízení, sériové číslo, stav, popisu a počtu svazků.

Můžete také konfigurovat převzetí služeb při selhání a zobrazit připojené iniciátorů z řídicího panelu hello zařízení.

Hello běžné úkoly, které lze provést na této stránce jsou:

* Zobrazit připojené iniciátory
* Najít sériové číslo zařízení hello
* Nalezen element target zařízení hello IQN

## <a name="view-connected-initiators"></a>Zobrazit připojené iniciátory
Můžete zobrazit hello iniciátory iSCSI, které jsou připojené tooyour zařízení kliknutím hello **zobrazit připojená iniciátory** odkaz uvedený v hello **rychlého přehledu** oblasti řídicího panelu zařízení. Tato stránka obsahuje tabulkové seznam hello iniciátory, které úspěšně jste se připojili tooyour zařízení. Pro každý iniciátor se zobrazí:

* Hello iSCSI kvalifikovaný název (IQN) hello připojené iniciátor.
* Název Hello hello záznam řízení přístupu (ACR), umožňuje tato připojené iniciátor.
* IP adresa Hello hello připojené iniciátor.
* Hello síťová rozhraní této iniciátor hello je tooon připojené zařízení úložiště. To může v rozsahu od DATA 0 tooDATA 5.
* Všechny svazky hello, které hello připojené iniciátor je povoleno tooaccess podle aktuální konfigurace ACR toohello.

Pokud najdete v části neočekávané iniciátory v tomto seznamu, nebo nejsou vidět, že hello očekává těch, které jsou, zkontrolujte konfiguraci ACR. Tooyour zařízení lze připojit maximálně 512 iniciátorů.

## <a name="find-hello-device-serial-number"></a>Najít sériové číslo zařízení hello
Sériové číslo zařízení hello musíte při konfiguraci Microsoft Multipath I/O (MPIO) na zařízení hello. Proveďte následující kroky toofind hello zařízení sériové číslo hello.

#### <a name="toofind-hello-device-serial-number"></a>sériové číslo zařízení hello toofind
1. Přejděte příliš**zařízení** > **řídicí panel**.
2. V pravém podokně hello hello řídicího panelu, vyhledejte hello **rychlého přehledu** oblasti.
3. Posuňte se dolů a najděte hello sériové číslo.

## <a name="find-hello-device-target-iqn"></a>Nalezen element target zařízení hello IQN
Cílové zařízení hello IQN musíte při konfiguraci hello Challenge Handshake Authentication Protocol (CHAP) v zařízení StorSimple. Proveďte následující kroky toofind hello zařízení cíl IQN hello.

### <a name="toofind-hello-device-target-iqn"></a>cílové zařízení hello toofind IQN
1. Přejděte příliš**zařízení** > **řídicí panel**.
2. V pravém podokně hello hello řídicího panelu, vyhledejte hello **rychlého přehledu** oblasti.
3. Posuňte se dolů a najděte hello cíl IQN.

## <a name="next-steps"></a>Další kroky
* Další informace o hello [řídicí panel služby StorSimple Manager](storsimple-service-dashboard.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

