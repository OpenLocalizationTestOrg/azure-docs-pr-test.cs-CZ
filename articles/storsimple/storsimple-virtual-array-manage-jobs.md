---
title: "aaaView a spravovat úlohy pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje stránku hello Správce zařízení StorSimple služby úlohy a jak toouse ho tootrack poslední a aktuální úlohy pro hello pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a>Pomocí hello Správce zařízení StorSimple služby tooview úloh pro hello pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Hello **úlohy** okno poskytuje jednu centrální portál pro zobrazování a správu úloh, které jsou spuštěny na virtuální pole, které jsou připojené tooyour služby StorSimple Manager zařízení. Můžete zobrazit spuštěné, dokončené a k selhání úlohy pro více virtuální zařízení. Výsledky se zobrazí v tabulkovém formátu.

![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Můžete rychle najít hello úlohy, které zajímá pomocí filtrování pole, jako:

* **Čas rozsah** – úloh může být filtrovaná podle hello rozsah data a času.
* **Zařízení** – úloh se spouští na konkrétní zařízení, které jsou připojené tooyour službě. Hello filtrované úlohy jsou pak v tabulce podle hello následující atributy:
  
  * **Název** – může být název úlohy hello **všechny**, **zálohování**, **klon**, **převzetí služeb při selhání**, **stahování aktualizací** , nebo **nainstalovat aktualizace**.
  * **Stav** – úloh může být **všechny**, **v průběhu**, **úspěšné**, nebo **se nezdařilo**, nebo **zrušeno**.
  * **Entity** – hello úloh může být přidružen svazek, sdílená složka nebo zařízení.
  * **Zařízení** – hello název hello zařízení, na které hello úloha byla spuštěna.
  * **Bylo zahájeno** – hello čas, kdy byla spuštěna úloha hello.
  * **Doba trvání** – hello doba na která hello úloha byla spuštěna.
* **Stav** – můžete vyhledat všechny, se nezdařila, dokončené nebo spuštěné úlohy.
* **Typ úlohy** – typ úlohy hello být vše, zálohování, obnovení, převzetí služeb při selhání, stažení aktualizací nebo instalace aktualizací.

Hello seznam úloh se aktualizují každých 30 sekund.

## <a name="view-job-details"></a>Zobrazení podrobností o úloze
Proveďte následující kroky tooview hello podrobnosti libovolné úlohy hello.

#### <a name="tooview-job-details"></a>Podrobnosti úlohy tooview
1. Na hello **úlohy** okno, zobrazení úloh hello zajímá spuštěním dotazu s odpovídající filtry. Můžete vyhledat dokončené nebo spuštěné úlohy.
2. Vyberte úlohu z hello tabulkového seznamu úloh.
   
    ![Okno úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. V dolní části hello hello stránky, klikněte na tlačítko **podrobnosti**.
4. V hello **podrobnosti** dialogové okno, můžete zobrazit stav, podrobnosti a statistiku časových údajů. Hello následující obrázek ukazuje příklad hello **podrobnosti úlohy zálohování** dialogové okno.
   
    ![Podrobnosti úlohy](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a>Selhání úlohy při hello virtuálního počítače je pozastaven v hypervisoru hello
Když je úloha v průběhu na pole virtuální zařízení StorSimple a hello zařízení (virtuálního počítače v hypervisoru zřídit) je pozastavena pro více než 15 minut, úloha hello selže. To je z důvodu tooyour pole virtuální zařízení StorSimple čas nejsou synchronizovány s časem hello Microsoft Azure. 

Zobrazí se hello následující chybě: "váš čas zařízení není synchronizován s časem hello Microsoft Azure ve víc než 15 minut. Ujistěte se, že hello hypervisoru a hello zařízení časy jsou synchronizovány se server NTP. Ověřte, že neexistují žádné problémy s připojením. problémy s připojením tootroubleshoot, dat spuštění diagnostických testů z hello místního webového uživatelského rozhraní vašeho virtuální zařízení."

Selhání použít toobackup, obnovení, aktualizace až po převzetí služeb při selhání úlohy. Pokud virtuální počítač je zajištěna v Hyper-V, počítač hello nakonec synchronizuje čas s vaší hypervisoru. Jakmile k tomu dojde, můžete ji znovu úlohu.

## <a name="next-steps"></a>Další kroky
[Zjistěte, jak toouse hello místního webového uživatelského rozhraní tooadminister pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

