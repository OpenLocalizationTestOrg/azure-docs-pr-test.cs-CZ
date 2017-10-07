---
title: "aaaView a spravovat úlohy pro řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje okno úlohy služby StorSimple Manager zařízení hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
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
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a>Použít tooview služby StorSimple Manager zařízení hello a spravovat úlohy (Update 3 a novější)

## <a name="overview"></a>Přehled
Hello **úlohy** okno poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených služby StorSimple Manager zařízení tooyour. Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení. Výsledky se zobrazí v tabulkovém formátu.

![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:

* **Stav** – úloh může být v průběhu, byly úspěšné, zrušené, se nezdařilo, ruší nebo úspěšně dokončeno s chybami.
* **Čas rozsah** – úloh může být filtrovaná podle hello rozsah data a času. Hello rozsahy jsou za hodiny za posledních 24 hodin, posledních 7 dnů, za 30 dní, minulém roce nebo vlastní hodnota data.
* **Typ** – typ hello úlohy můžou být plánované zálohování, ruční zálohy, obnovení zálohy, klonování svazku, převzít kontejnery svazků, vytváření místně vázaný svazek, úpravě svazku, instalaci aktualizací, shromažďovat protokoly podpory a vytvoření cloudu zařízení.
* **Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.
  
Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:
  
* **Název** – naplánované zálohování, ruční zálohování, obnovení zálohu, klonování svazku, převzetí služeb při selhání kontejnery svazků, vytvořte místně vázaný svazek, upravit svazek, instalaci aktualizací, shromažďovat protokoly podpory nebo vytvořte cloudu zařízení.
* **Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.
* **Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení. Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování. V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.
* **Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.
* **Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.
* **Doba trvání** – hello čase požadované toocomplete hello úloha.

Hello seznam úloh se aktualizují každých 30 sekund.

Můžete provádět následující akce související s úlohy na této stránce hello:

* Zobrazení podrobností o úloze
* Zrušení úlohy

## <a name="view-job-details"></a>Zobrazení podrobností o úloze
Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.

#### <a name="tooview-job-details"></a>Podrobnosti úlohy tooview
1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **úlohy**.

2. V hello **úlohy** okno, zobrazení úloh hello zajímá spuštěním dotazu s odpovídající filtry. Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Vyberte a klikněte na úlohu.

    ![Okno úlohy](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. V modulu snap-in okno Podrobnosti úlohy hello můžete zobrazit stav hello, podrobnosti, statistiku časových údajů a dat statistiky.
   
    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Zrušení úlohy
Proveďte následující kroky toocancel spuštěná úloha hello.

> [!NOTE]
> Některé úlohy, jako jsou úpravy typu svazku toochange hello svazku nebo rozšíření svazku, nelze zrušit.


### <a name="toocancel-a-job"></a>toocancel úlohy
1. Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry. Vyberte úlohu hello.

2. Klikněte pravým tlačítkem na hello vybrané úlohy tooinvoke hello kontextovou nabídku a klikněte na tlačítko **zrušit**.

    ![Podrobnosti úlohy](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Tato úloha je nyní zrušena.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-8000-manage-backup-policies-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

