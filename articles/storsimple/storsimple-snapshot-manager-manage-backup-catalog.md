---
title: "Katalog zálohování aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje, jak toouse hello tooview modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a správě hello zálohování katalogu."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 173410095bcec7948d780d7fc258d2e3670bde8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toomanage-hello-backup-catalog"></a>Použití StorSimple Snapshot Manager toomanage hello zálohování katalogu

## <a name="overview"></a>Přehled
primární Funkce Hello služby StorSimple Snapshot Manager je tooallow jste toocreate zálohování konzistentní s aplikací zkopíruje svazky zařízení StorSimple v podobě hello snímků. Snímky jsou pak uvedeny v souboru XML s názvem *zálohování katalogu*. Katalog zálohování Hello Uspořádá snímky skupinou svazku a poté podle místní snímek nebo cloudový snímek.

Tento kurz popisuje, jak můžete použít hello **katalog zálohování** hello toocomplete uzlu následující úlohy:

* Obnovení svazku
* Klonování svazku nebo svazku skupiny
* Odstranit zálohy
* Obnovit soubor
* Obnovení databáze hello Snapshot Manager zařízení Storsimple

Můžete zobrazit katalog zálohování hello rozšířením hello **zálohování katalogu** uzel v hello **oboru** podokně a pak rozšiřování skupiny hello svazku.

* Pokud kliknete na název skupiny hello svazku, hello **výsledky** podokně se zobrazuje počet hello místní snímky a cloudových snímků, které jsou k dispozici pro skupinu hello svazku. 
* Pokud kliknete na tlačítko **místní snímek** nebo **Cloudový snímek**, hello **výsledky** podokně se zobrazí následující informace o jednotlivých snímek zálohy hello (v závislosti na vaší **Zobrazení** nastavení):
  
  * **Název** – pořízení snímku hello čas hello.
  * **Typ** – jestli se jedná o místní snímek nebo cloudový snímek.
  * **Vlastník** – hello vlastníka obsahu. 
  * **K dispozici** – jestli hello snímku je nyní k dispozici. **Hodnota TRUE,** označuje tento snímek hello je k dispozici a může být obnovena; **False** označuje tento snímek hello již není k dispozici. 
  * **Importovat** – jestli naimportované hello zálohování. **Hodnota TRUE,** označuje zálohování hello byla naimportována ze Správce zařízení StorSimple nakonfigurována služba v hello čas hello zařízení ve Snapshot Manageru zařízení StorSimple; hello **False** znamená, že nebyl importován, ale byla vytvořena ve Snapshot Manageru zařízení StorSimple. (Umožňuje snadno identifikovat skupinu importované svazku protože příponu je přidána, která určuje hello zařízení, ze kterého byl importován hello svazku skupiny.)
    
    ![Zálohování katalogu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Pokud rozbalíte **místní snímek** nebo **Cloudový snímek**a potom klikněte na název jednotlivých snímku hello **výsledky** podokně se zobrazí následující informace o hello hello snímek, který jste vybrali:
  
  * **Název** – hello svazku identifikovaný písmeno jednotky. 
  * **Místní název** – hello místní název jednotky hello (Pokud je k dispozici). 
  * **Zařízení** – hello název hello zařízení, na které hello svazku nachází. 
  * **K dispozici** – jestli hello snímku je nyní k dispozici. **Hodnota TRUE,** označuje tento snímek hello je k dispozici a může být obnovena; **False** označuje tento snímek hello již není k dispozici. 

## <a name="restore-a-volume"></a>Obnovení svazku
Použijte následující postup toorestore svazek ze zálohy hello.

#### <a name="prerequisites"></a>Požadavky
Pokud jste tak již neučinili, vytvořte svazek a svazek skupiny a pak odstraňte hello svazku. Ve výchozím nastavení StorSimple Snapshot Manager zálohuje svazek před umožňující ho toobe odstranit. Toto opatření můžete zabránit ztrátě dat, pokud hello svazek je neúmyslně odstraněn nebo pokud je toobe obnovit z jakéhokoli důvodu hello data. 

StorSimple Snapshot Manager zobrazí hello následující zprávou v průběhu vytváření hello preventivní zálohování.

![Zpráva automatické snímku](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Nelze odstranit svazek, který je součástí skupiny svazku. možnost odstranění Hello je k dispozici. <br>
> 
> 

#### <a name="toorestore-a-volume"></a>toorestore svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně rozbalte hello **katalog zálohování** uzlu, rozbalte skupinu svazku a pak klikněte na tlačítko **místní snímky** nebo **cloudových snímků**. Zobrazí se seznam snímků zálohy v hello **výsledky** podokně.
3. Najít hello zálohování, že chcete toorestore, klikněte pravým tlačítkem a pak klikněte na tlačítko **obnovení**.
   
    ![Obnovit zálohování katalogu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Na stránce pro potvrzení hello, projděte podrobnosti hello, zadejte **potvrdit**a potom klikněte na **OK**. StorSimple Snapshot Manager používá hello zálohování toorestore hello svazku.
   
    ![Obnovit zprávy s potvrzením](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Akce obnovení hello můžete monitorovat při jeho spuštění. V hello **oboru** podokně rozbalte hello **úlohy** uzel a pak klikněte na tlačítko **systémem**. Podrobnosti úlohy Hello se zobrazí v hello **výsledky** podokně. Po dokončení úlohy obnovení hello hello podrobnosti úlohy jsou přenášená toohello **posledních 24 hodin** seznamu.

## <a name="clone-a-volume-or-volume-group"></a>Klonování svazku nebo svazku skupiny
Použijte následující postup toocreate duplicitní (klonování) svazek nebo svazek skupiny hello.

#### <a name="tooclone-a-volume-or-volume-group"></a>tooclone svazek nebo svazek skupiny
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **katalog zálohování** uzlu, rozbalte skupinu svazku a pak klikněte na tlačítko **cloudových snímků**. Zobrazí se seznam záloh v hello **výsledky** podokně.
3. Najít hello svazek nebo svazek skupiny chcete tooclone, klikněte pravým tlačítkem na hello svazku nebo název skupiny svazku a klikněte na **klon**. Hello **klon Cloudový snímek** zobrazí se dialogové okno.
   
    ![Klonování cloudový snímek](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Dokončení hello **klon Cloudový snímek** dialogové okno následujícím způsobem: 
   
   1. V hello **název** textového pole zadejte název hello klonovat svazku. Tento název se zobrazí v hello **svazky** uzlu. 
   2. (Volitelné) vyberte **jednotky**a potom vyberte z rozevíracího seznamu hello písmeno jednotky.
   3. (Volitelné) vyberte **složky (NTFS)**a zadejte cestu ke složce nebo klikněte na tlačítko Procházet a vyberte umístění složky pro hello. 
   4. Klikněte na možnost **Vytvořit**.
5. Po dokončení hello proces klonování je třeba inicializovat hello klonovat svazku. Spusťte správce serveru a pak spusťte nástroj Správa disků. Podrobné pokyny najdete v tématu [připojit svazky](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po inicializaci, zobrazí se hello svazku v části hello **svazky** uzel v hello **oboru** podokně. Pokud nevidíte hello svazku uvedené, aktualizujte hello seznam svazků (klikněte pravým tlačítkem na hello **svazky** uzel a pak klikněte na tlačítko **aktualizovat**).

## <a name="delete-a-backup"></a>Odstranit zálohy
Použijte následující postup toodelete snímek z katalogu zálohování hello hello. 

> [!NOTE]
> Odstranění snímku odstraní hello zálohovat data přidružená k hello snímku. Hello proces vyčištění dat z cloudu hello však může chvíli trvat.<br>


#### <a name="toodelete-a-backup"></a>toodelete zálohu
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **katalog zálohování** uzlu, rozbalte skupinu svazku a pak klikněte na tlačítko **místní snímky** nebo **cloudových snímků**. Zobrazí se seznam snímky v hello **výsledky** podokně.
3. Klikněte pravým tlačítkem na hello snímku toodelete a pak klikněte na **odstranit**.
4. Jakmile se zobrazí potvrzovací zpráva hello, klikněte na tlačítko **OK**.

## <a name="recover-a-file"></a>Obnovit soubor
Pokud soubor je omylem odstraněn ze svazku, můžete obnovit soubor hello načtením snímek, který předem data hello odstranění, pomocí hello snímku toocreate klon hello svazek a potom kopírování souboru hello z hello klonovaný svazku toohello původního svazku.

#### <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte aktuální záloha hello svazku skupiny. Potom odstraňte soubor uložené v jednom ze svazků hello v dané skupině svazku. Nakonec použijte následující postup toorestore hello odstranit soubor ze zálohy hello. 

#### <a name="toorecover-a-deleted-file"></a>toorecover odstraněného souboru
1. Klikněte na ikonu StorSimple Snapshot Manager hello na ploše. Zobrazí okno konzoly Hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně rozbalte hello **katalog zálohování** uzel a procházet tooa snímku, která obsahuje soubor hello odstranit. Obvykle byste měli vybrat snímek, který byl vytvořen těsně před hello odstranění.
3. Najít hello svazku, že chcete tooclone, klikněte pravým tlačítkem a klikněte na **klon**. Hello **klon Cloudový snímek** zobrazí se dialogové okno.
   
    ![Klonování cloudový snímek](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Dokončení hello **klon Cloudový snímek** dialogové okno následujícím způsobem: 
   
   1. V hello **název** textového pole zadejte název hello klonovat svazku. Tento název se zobrazí v hello **svazky** uzlu. 
   2. (Volitelné) Vyberte **jednotky**a potom vyberte z rozevíracího seznamu hello písmeno jednotky. 
   3. (Volitelné) Vyberte **složky (NTFS)**a zadejte cestu ke složce, nebo klikněte na tlačítko **Procházet** a vyberte umístění složky pro hello. 
   4. Klikněte na možnost **Vytvořit**. 
5. Po dokončení hello proces klonování je třeba inicializovat hello klonovat svazku. Spusťte správce serveru a pak spusťte nástroj Správa disků. Podrobné pokyny najdete v tématu [připojit svazky](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po inicializaci, zobrazí se hello svazku v části hello **svazky** uzel v hello **oboru** podokně. 
   
    Pokud nevidíte hello svazku uvedené, aktualizujte hello seznam svazků (klikněte pravým tlačítkem na hello **svazky** uzel a pak klikněte na tlačítko **aktualizovat**).
6. Otevřete hello složky systému souborů NTFS, která obsahuje hello klonovat svazku, rozbalte položku hello **svazky** uzel a pak otevřete hello klonovat svazku. Najít soubor hello má toorecover a zkopírujte jej toohello primární svazku.
7. Po obnovení souboru hello můžete odstranit hello složky systému souborů NTFS, která obsahuje svazek hello klonovat.

## <a name="restore-hello-storsimple-snapshot-manager-database"></a>Obnovení databáze hello Snapshot Manager zařízení StorSimple
Měli pravidelně zálohovat databázi hello Snapshot Manager zařízení StorSimple v hostitelském počítači hello. Pokud dojde k havárii nebo hello hostitelský počítač z nějakého důvodu selže, ho můžete obnovit ze zálohy hello. Vytvoření zálohy databáze hello je ruční proces.

#### <a name="tooback-up-and-restore-hello-database"></a>tooback zálohu a obnovte databázi hello
1. Zastavte službu StorSimple Management společnosti Microsoft hello:
   
   1. Spusťte správce serveru.
   2. Na řídicím panelu Správce serveru hello, na hello **nástroje** nabídce vyberte možnost **služby**.
   3. Na hello **služby** okno, vyberte hello **služba správy zařízení StorSimple Microsoft**.
   4. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **zastavit službu hello**.
2. Na hostitelském počítači hello procházet tooC:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData je skrytá složka.
   > 
   > 
3. Najít soubor XML hello katalogu, kopírovat hello soubor a zkopírujte hello úložiště do bezpečného umístění nebo v cloudu hello. Pokud hello z hostitelů selže, můžete použít tento záložní soubor toohelp obnovit hello zásady zálohování, které jste vytvořili v zařízení StorSimple Snapshot Manager.
   
    ![Soubor zálohy katalogu Azure StorSimple](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Restartujte službu StorSimple Management společnosti Microsoft hello: 
   
   1. Na řídicím panelu Správce serveru hello, na hello **nástroje** nabídce vyberte možnost **služby**.
   2. Na hello **služby** okno, vyberte hello **služba správy zařízení StorSimple Microsoft**.
   3. V hello pravým podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **restartujte službu hello**.
5. Na hostitelském počítači hello procházet tooC:\ProgramData\Microsoft\StorSimple\BACatalog. 
6. Odstraňte soubor XML hello katalogu a nahraďte ji metodou hello verze zálohy, který jste vytvořili. 
7. Klikněte na tlačítko hello plochy StorSimple Snapshot Manager ikonu toostart StorSimple Snapshot Manager. 

## <a name="next-steps"></a>Další kroky
* Další informace o [pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Další informace o [úlohy StorSimple Snapshot Manager a pracovních postupů](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

