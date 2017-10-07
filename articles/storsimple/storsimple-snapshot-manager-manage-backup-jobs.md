---
title: "úlohy zálohování aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje, jak toouse hello tooview modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a spravovat naplánované, aktuálně spuštěné a dokončené úlohy zálohování."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a>Použít tooview Snapshot Manager zařízení StorSimple a spravovat úlohy zálohování

## <a name="overview"></a>Přehled
Hello **úlohy** uzel v hello **oboru** podokně se zobrazují hello **naplánovaná**, **posledních 24 hodin**, a **systémem**úlohy, které jste spustili interaktivně nebo nakonfigurované zásady zálohování. 

Tento kurz vysvětluje, jak je možné používat hello **úlohy** uzlu toodisplay informace o naplánované, poslední a aktuálně spuštěné úlohy zálohování. (hello seznam úloh a odpovídající informace se zobrazí v hello **výsledky** podokně.) Kromě toho můžete klikněte pravým tlačítkem na úlohu uvedené a najdete v části z kontextové nabídky, které jsou uvedeny dostupné akce.

## <a name="view-scheduled-jobs"></a>Zobrazit naplánované úlohy
Hello použijte následující postup tooview naplánovaných úlohách zálohování.

#### <a name="tooview-scheduled-jobs"></a>tooview naplánované úlohy
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **naplánovaná**. Hello následující informace se zobrazí v hello **výsledky** podokně:
   
   * **Název** – hello název plánovaný snímek hello
   * **Následně spusťte** – hello datum a čas hello další plánovaný snímek.
   * **Poslední spuštění** – hello datum a čas poslední plánovaný snímek hello
     
     > [!NOTE]
     > Jednorázové pouze snímky hello **další spuštění** a **poslední spuštění** bude hello stejné.
     
     ![Naplánované úlohy zálohování](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.

## <a name="view-recent-jobs"></a>Zobrazit nejnovější úlohy
Použijte hello následující postup tooview zálohování a obnovení úlohy, které byly dokončeny v hello posledních 24 hodin.

#### <a name="tooview-recent-jobs"></a>tooview posledních úloh
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **posledních 24 hodin**. Hello **výsledky** podokně se zobrazí úlohy zálohování pro hello posledních 24 hodin (tooa maximálně 64 úlohy). Hello následující informace se zobrazí v hello **výsledky** podokně, v závislosti na hello **zobrazení** možnosti můžete zadat:
   
   * **Název** – hello název hello plánovaný snímek.
   * **Spuštění** – hello datum a čas zahájení hello snímku.
   * **Zastavit** – hello datum a čas dokončení nebo byl ukončen hello snímku.
   * **Uplynulý čas** – hello množství času mezi hello **Začínáme** a **Zastaveno** časy.
   * **Stav** – hello stav hello nedávném dokončení úlohy. **Úspěch** označuje zálohování hello byla úspěšně vytvořena. **Se nezdařilo** označuje, že hello úlohy nebyl úspěšně spuštěn.
   * **Informace o** – hello důvodem selhání hello.
   * **Zpracování bajtů (MB)** – hello množství dat ze skupiny hello svazek, který byl zpracován (v MB). 
     
     ![Úlohy, které byly spuštěny v hello posledních 24 hodin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.
   
    ![Odstranit úlohu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Zobrazit aktuálně spuštěné úlohy
Použijte následující postup tooview úloh, které jsou aktuálně spuštěné hello.

#### <a name="tooview-currently-running-jobs"></a>tooview aktuálně spuštěných úloh
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **systémem**. V závislosti na hello **zobrazení** možnosti určíte, hello následující informace se zobrazí v hello **výsledky** podokně:
   
   * **Název** – hello název hello plánovaný snímek.
   * **Spuštění** – hello datum a čas zahájení hello snímku.
   * **Kontrolní bod** – hello aktuální akce hello zálohy.
   * **Stav** – hello procentuální hodnotě dokončení.
   * **Uplynulý čas** – hello množství času, který prošel od začátku hello zálohování. 
   * **Průměrná propustnost (MB)** – poměr celkový počet bajtů toothat zpracování dat o celkový čas potřebný pro zpracování (MB).
   * **Zpracování bajtů (MB)** – celkový počet bajtů dat, zpracování (v MB).
   * **(MB) zapsaných bajtů** – celkový počet bajtů zapsaných (v MB). Zahrnuje hello data a také hello metadata a proto je obvykle větší než hello zpracovat bajtů.
     
     ![Aktuálně spuštěné úlohy](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Další akce tooperform na konkrétní úlohy, klikněte pravým tlačítkem na název úlohy hello v hello **výsledky** panelu a vyberte z možností nabídky hello.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[používat katalog zálohování hello StorSimple Snapshot Manager toomanage](storsimple-snapshot-manager-manage-backup-catalog.md).

