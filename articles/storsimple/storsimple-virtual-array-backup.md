---
title: "kurz zálohování Azure StorSimple virtuální pole aaaMicrosoft | Microsoft Docs"
description: "Popisuje, jak sdílené složky tooback až pole virtuální zařízení StorSimple a svazky."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Zálohování sdílené složky nebo svazky na pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Hello pole virtuální zařízení StorSimple je hybridní cloudové úložiště místní virtuální zařízení, které se dají konfigurovat jako souborový server nebo server se službou iSCSI. pole virtuálním Hello umožňuje hello uživatele toocreate plánované a ruční zálohování všech hello složek nebo svazků v zařízení hello. Když nakonfigurovaný jako souborový server, také umožňuje obnovení na úrovni položek. Tento kurz popisuje, jak naplánovat toocreate a ručního zálohování a proveďte obnovení na úrovni položek toorestore odstraněnému souboru na vaše virtuální pole.

V tomto kurzu platí toohello StorSimple virtuální pole pouze. Informace o řady 8000 přejděte příliš[vytvoření zálohy pro řady 8000 zařízení](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Zálohování sdílených složek a svazků

Zálohování poskytuje ochranu v okamžiku, zvyšují možnost a minimalizovat dobu obnovení sdílené složky a svazky. Můžete zálohovat na sdílené složky nebo svazku zařízení StorSimple dvěma způsoby: **naplánovaná** nebo **ruční**. Každá z metod hello je popsané v následující části hello.

## <a name="change-hello-backup-start-time"></a>Změňte čas spuštění zálohování hello

> [!NOTE]
> V této verzi jsou naplánované zálohy vytvořené pomocí výchozí zásadu, která spouští každý den v určitou dobu a zálohuje všechny hello sdílené složky nebo svazky na hello zařízení. Není možné toocreate vlastní zásady pro naplánovaných záloh v tuto chvíli.


Pole virtuální zařízení StorSimple má výchozí zásady zálohování, který začíná v zadané době den (22:30) a zálohuje všechny hello sdílené složky nebo svazky na zařízení hello jednou denně. Hello dobu, na které hello zálohování se spustí, ale hello četnost a hello uchování (který určuje hello počet záloh tooretain) nelze změnit, můžete změnit. Během tyto zálohy je zálohován hello celý virtuální zařízení. To může potenciálně ovlivnit výkon hello hello zařízení a ovlivnit hello úlohy nasazené na zařízení hello. Proto doporučujeme, abyste naplánovali tyto zálohy pro hodiny mimo špičku.

 Počáteční čas toochange hello výchozí zálohování, proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a>Počáteční čas pro zásady zálohování výchozí hello toochange hello

1. Přejděte příliš**zařízení**. Zobrazí se seznam Hello zařízení zaregistrovaná s služby StorSimple Manager zařízení. 
   
    ![přejděte toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Vyberte a klikněte na zařízení. Hello **nastavení** zobrazí se okno. Přejděte příliš**spravovat > zásady zálohování**.
   
    ![Vyberte příslušné zařízení.](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. V hello **zásady zálohování** okně čas spuštění výchozí hello je 22:30. Hello nový počáteční čas pro denní plán hello můžete zadat v zařízení časovém pásmu.
   
    ![přejděte toobackup zásady](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Klikněte na **Uložit**.

### <a name="take-a-manual-backup"></a>Proveďte ruční zálohy

Kromě toho tooscheduled zálohování, můžete využít ručního zálohování (na vyžádání) dat zařízení kdykoli.

#### <a name="toocreate-a-manual-backup"></a>toocreate ruční zálohy

1. Přejděte příliš**zařízení**. Vyberte zařízení a klikněte pravým tlačítkem na **...**  v hello úplně vpravo v hello vybraný řádek. Hello místní nabídce vyberte **provést zálohování**.
   
    ![přejděte tootake zálohování](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. V hello **provést zálohování** okně klikněte na tlačítko **provést zálohování**. To bude zálohovat všechny hello sdílené složky na souborovém serveru hello nebo všechny svazky hello na vašem serveru iSCSI. 
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Spustí zálohu na vyžádání a uvidíte, že byla spuštěna úloha zálohování.
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    Po hello úloha byla úspěšně dokončena, zobrazí se upozornění znovu. pak spustí proces zálohování Hello.
   
    ![Vytvořit úlohu zálohování](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. průběh hello tootrack hello zálohy a podívejte se na podrobnosti úlohy hello, klikněte na tlačítko hello oznámení. Tím přejdete příliš **podrobnosti úlohy**.
   
     ![Podrobnosti úlohy zálohování.](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Po dokončení zálohování hello přejděte příliš**správy > Katalog zálohování**. Zobrazí se cloudový snímek všech sdílených složek hello (nebo svazky) ve vašem zařízení.
   
    ![Dokončené zálohování](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Zobrazit existující zálohy
tooview hello existující zálohy, proveďte hello proveďte kroky v hello portálu Azure.

#### <a name="tooview-existing-backups"></a>tooview existující zálohy

1. Přejděte příliš**zařízení** okno. Vyberte a klikněte na zařízení. V hello **nastavení** okně přejděte příliš**správy > Katalog zálohování**.
   
    ![Přejděte toobackup katalogu](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Zadejte hello následující kritéria toobe použitých pro filtrování:
   
    - **Čas rozsah** – může být **poslední 1 hodinu**, **za posledních 24 hodin**, **za posledních 7 dní**, **za posledních 30 dnů**, **za poslední rok**, a **vlastní datum**.
    
    - **Zařízení** – vyberte ze seznamu hello souborových serverů nebo serverů iSCSI zaregistrována služby StorSimple Manager zařízení.
   
    - **Iniciované** – lze automaticky **naplánovaná** (podle zásady zálohování) nebo **ručně** iniciovaná (můžete).
   
    ![Filtr zálohy](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Klikněte na tlačítko **Použít**. Hello filtrovaný seznam zálohy se zobrazí v hello **katalog zálohování** okno. Poznámka: pouze 100 zálohování elementy lze zobrazit v daném okamžiku.
   
    ![Aktualizované zálohování katalogu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Další kroky

Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

