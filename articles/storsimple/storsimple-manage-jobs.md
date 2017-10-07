---
title: "aaaView a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b7341270e37a9f2530a8da1fb7c54f6b699c699c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs"></a>Použít tooview služby StorSimple Manager hello a spravovat úlohy StorSimple
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Přehled
Hello **úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených tooyour služby StorSimple Manager. Můžete zobrazit naplánované, spuštěná, dokončené a k selhání úlohy pro více zařízení. Výsledky se zobrazí v tabulkovém formátu. 

![Stránka úlohy](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:

* **Stav** – můžete spuštěny úlohy naplánované, se nezdařila, dokončené, ruší nebo zrušené.
* **Typ** – úloh lze vytvořit v důsledku zálohu na vyžádání nebo naplánované (**provést zálohování**), klonování, obnovení zařízení nebo operaci aktualizace.
* **Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.
* **Od a do** – úloh může být filtrovaná podle hello rozsah data a času.

Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:

* **Typ** – zálohování, obnovení, klonování převzetí služeb při selhání nebo aktualizace.
* **Stav** – systémem naplánované, se nezdařila, byla dokončena, zrušení nebo došlo ke zrušení.
* **Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení. Úloha klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování. V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.
* **Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.
* **Spuštění na** – hello čas, kdy byla spuštěna úloha hello.
* **Průběh** – hello procento dokončení běžící úlohy. Dokončené úlohy to by měl být vždy 100 %.

Hello seznam úloh se aktualizují každých 30 sekund.

Můžete provádět následující akce související s úlohy na této stránce hello:

* Zobrazení podrobností o úloze
* Zrušení úlohy

## <a name="view-job-details"></a>Zobrazení podrobností o úloze
Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.

#### <a name="tooview-job-details"></a>Podrobnosti úlohy tooview
1. Na hello **úlohy** stránky, zobrazit hello úloh zajímá spuštěním dotazu s odpovídající filtry. Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.
2. Vyberte úlohu.
3. V dolní části hello hello stránky, klikněte na tlačítko **podrobnosti**.
4. V hello **podrobnosti úlohy zálohování** dialogové okno, můžete zobrazit stav hello, podrobnosti, statistiku časových údajů a dat statistiky.

## <a name="cancel-a-job"></a>Zrušení úlohy
Proveďte následující kroky toocancel spuštěná úloha hello.

### <a name="toocancel-a-job"></a>toocancel úlohy
1. Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry.
2. Vyberte úlohu hello.
3. V dolní části hello hello stránky, klikněte na tlačítko **zrušit**.
4. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

Tato úloha je nyní zrušena.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

