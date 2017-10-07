---
title: "aaaView a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager hello a jak toouse ho tootrack poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a>Použít tooview služby StorSimple Manager hello a spravovat úlohy StorSimple (Update 2)
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Přehled
Hello **úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojených tooyour služby StorSimple Manager. Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení. Výsledky se zobrazí v tabulkovém formátu. 

![Stránka úlohy](./media/storsimple-manage-jobs-u2/jobs.png)

Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:

* **Stav** – úloh může být spuštěn, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.
* **Od a do** – úloh může být filtrovaná podle hello rozsah data a času.
* **Typ** – hello typ úlohy může být zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.
* **Zařízení** – úloh se spouští na určité službě tooyour zařízení připojené.
  Hello filtrované úlohy jsou pak v tabulce na základě hello hello následující atributy:
  
  * **Typ** – zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.
  * **Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.
  * **Entity** – hello úloh může být přidružen svazku, zásady zálohování nebo zařízení. Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování. V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.
  * **Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.
  * **Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.
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
   
    ![Stránka Podrobnosti úlohy](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Zrušení úlohy
Proveďte následující kroky toocancel spuštěná úloha hello.

> [!NOTE]
> Některé úlohy, jako jsou úpravy typu svazku toochange hello svazku nebo rozšíření svazku, nelze zrušit.
> 
> 

### <a name="toocancel-a-job"></a>toocancel úlohy
1. Na hello **úlohy** stránky, zobrazit hello spuštěných úloh má toocancel spuštěním dotazu s odpovídající filtry.
2. Vyberte úlohu hello.
3. V dolní části hello hello stránky, klikněte na tlačítko **zrušit**.
4. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

Tato úloha je nyní zrušena.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

