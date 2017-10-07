---
title: "Zálohování Azure: Tooa obnovení stavu systému Windows Server | Microsoft Docs"
description: "Krok podle kroku vysvětlení pro obnovení stavu systému Windows Server ze zálohy v Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a>Obnovit stav systému tooWindows serveru

Tento článek vysvětluje, jak toorestore zálohy stavu systému Windows Server ze služeb zotavení Azure trezoru. toorestore stav systému, musíte mít zálohu stavu systému (vytvořili pomocí pokynů hello v [zálohování stavu systému](backup-azure-system-state.md#back-up-windows-server-system-state-preview)) a ujistěte se, že máte nainstalovanou hello [nejnovější verzi Microsoft Azure Recovery hello Agent služeb (MARS)](http://aka.ms/azurebackup_agent). Obnovení dat stavu systému Windows Server z trezoru služby Azure Recovery Services je dvoustupňový proces:

1. Obnovení stavu systému souborů z Azure Backup. Při obnovení stavu systému jako soubory ze zálohy Azure, můžete se buď:
  * Obnovit stav systému toohello stejný server, kde byly provedeny hello zálohy, nebo
  * Obnovit stav systému souborů tooan alternativní server.

2. Použijte hello obnovit stav systému souborů tooa systému Windows Server.


## <a name="recover-system-state-files-toohello-same-server"></a>Obnovení stavu systému souborů toohello stejný server
Hello následující kroky popisují, jak tooroll zpět předchozí stav vašeho systému Windows Server konfigurace tooa. Vrácení zpět tooa konfigurace označuje, stabilního stavu váš server může být velmi důležité. Hello následujících kroků obnovení hello stavu systému serveru z trezoru služeb zotavení. 

1. Otevřete hello **Microsoft Azure Backup** modul snap-in. Pokud si nejste jisti, kde nainstalovaný modul snap-in hello, vyhledávat hello počítači nebo serveru pro **Microsoft Azure Backup**.

    aplikace na ploše Hello by se zobrazit ve výsledcích hledání hello.

2. Klikněte na tlačítko **obnovit Data** toostart hello průvodce.

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. Na hello **Začínáme** podokně, toorestore hello data toohello stejný server nebo počítač, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.

    ![Vyberte tento server možnost toorestore hello data toohello stejný počítač](./media/backup-azure-restore-system-state/samemachine.png)

4. Na hello **vyberte režimu obnovení** podokně vyberte **stav systému** a pak klikněte na **Další**.

    ![Procházet soubory](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. V kalendáři hello v **vyberte svazek a datum** podokně, vyberte obnovení bodu. 

    Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.

    ![Svazek a datum](./media/backup-azure-restore-system-state/select-date.png)

6. Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **Další**.

    Zálohování Azure připojí bodu místní obnovení hello a používá je jako svazek obnovení.

7. V podokně Další hello zadejte hello cíl pro hello obnovit stav systému souborů a klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete. Hello možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie hello hello celý archivu stavu systému.

    ![Možnosti obnovení](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Ověřte podrobnosti o hello obnovení na hello **potvrzení** panelu a klikněte na tlačítko **obnovit**.

   ![Klikněte na tlačítko Obnovit tooacknowledge hello obnovit akce](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopírování hello *WindowsImageBackup* adresář v hello nekritické svazek tooa obnovení cílového serveru hello. Hello svazku operačního systému Windows je obvykle hello nepostradatelný svazek.

10. Po úspěšné obnovení hello postupujte podle kroků hello v části hello [použít obnovit stav systému souborů toohello systému Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello proces obnovení stavu systému.

## <a name="recover-system-state-files-tooan-alternate-server"></a>Obnovení stavu systému souborů tooan alternativní server

Pokud váš Server systému Windows je poškozená nebo je nepřístupný, a chcete toorestore ho hello tooa stabilního stavu tím, že obnovení stavu systému Windows Server, můžete obnovit stav systému hello poškozená serveru z jiného serveru. Pomocí následujících kroků toohello obnovení stavu systému na samostatný server hello.  

zahrnuje Hello terminologie použitá v těchto kroků:

- *Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.
- *Cílový počítač* – hello počítač toowhich hello data obnovena.
- *Ukázka trezoru* – hello toowhich trezoru služeb zotavení hello *zdrojový počítač* a *cílový počítač* jsou registrované. <br/>

> [!NOTE]
> Zálohy vytvořené z jednoho počítače nelze počítač obnovený tooa starší verzí operačního systému hello. Zálohy vytvořené ze systému Windows Server 2016 počítač nemůže být třeba obnovit tooWindows Server 2012 R2. Inverzní hello je ale možné. Můžete použít zálohování z Windows serveru 2012 R2 toorestore systému Windows Server 2016.
>

1. Otevřete hello **Microsoft Azure Backup** modul snap-in na hello *cílový počítač*.
2. Ujistěte se, že hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello trezoru stejné služeb zotavení.
3. Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)

4. Vyberte **jiný server**

    ![Jiný Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*. Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost), stáhněte si nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portálu Azure. Jakmile je zadaný soubor s přihlašovacími údaji trezoru hello, zobrazí se trezor služeb zotavení hello přidružený soubor s přihlašovacími údaji trezoru hello.

6. V podokně vyberte Server Backup hello vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů.

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. V podokně hello vyberte režimu obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**. 

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. Na hello kalendáře v hello **vyberte svazek a datum** podokně, vyberte obnovení bodu. Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce. 

    ![Hledání položek](./media/backup-azure-restore-system-state/select-date.png)

9. Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **Další**.

10. Na hello **vyberte režim obnovení stavu systému** podokně zadejte cíl hello místo, kam chcete stav systému souborů toobe obnovit a pak klikněte na **Další**.

    ![Šifrování](./media/backup-azure-restore-system-state/recover-as-files.png)

    Hello možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie hello hello celý archivu stavu systému.

11. Ověřte podrobnosti o hello obnovení v podokně hello potvrzení a klikněte na tlačítko **obnovit**. 

    ![Klikněte na tlačítko Obnovit hello, tooconfirm proces obnovení hello](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopírování hello *WindowsImageBackup* directory tooa nekritické svazku hello serveru (například D:\). Obvykle je hello svazku operačního systému Windows hello nepostradatelný svazek.

13. proces obnovení hello toocomplete, použijte následující hello části příliš[použít hello obnovit stav systému souborů v systému Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Použít obnovený stav systému v systému Windows Server

Jakmile jste obnovili stavu systému jako soubory pomocí agenta služeb zotavení Azure, použijte hello zálohování serveru nástroj tooapply hello obnovit tooWindows stav systému serveru. Hello nástroj zálohování systému Windows Server je již k dispozici na serveru hello. Hello následující kroky popisují, jak tooapply hello obnovit stav systému.

1. Použití hello následující příkazy tooreboot serveru v *režimu obnovení adresářových služeb*. V řádku se zvýšenými oprávněními příkaz:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Po restartování hello otevřete modul snap-in Zálohování serveru Windows hello. Pokud si nejste jisti, kde nainstalovaný modul snap-in hello, vyhledávat hello počítači nebo serveru pro **zálohování serveru**.

    aplikace na ploše Hello se zobrazí ve výsledcích hledání hello.

3. V modulu snap-in hello, vyberte **místní záloha**.

    ![Vyberte místní záloha toorestore odtud](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. V konzole hello místní záloha v hello **podokna akce**, klikněte na tlačítko **obnovit** tooopen hello Průvodce obnovením.

5. Vyberte možnost hello **zálohy uložené v jiném umístění**a klikněte na tlačítko **Další**.

   ![Vyberte jiný server toorecover tooa](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Při zadávání typ umístění hello, vyberte **vzdálené sdílené složce** Pokud záloha stavu systému byla obnovená tooanother serveru. Pokud stav systému byla obnovena místně, pak vyberte **místní jednotky**. 

    ![Vyberte zda toorecovery z místního serveru nebo jiné](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Zadejte cestu toohello hello *WindowsImageBackup* adresář, nebo zvolte hello místní disk obsahující tento adresář (například D:\WindowsImageBackup), obnovit jako součást obnovení souborů hello stavu systému pomocí Azure Recovery Služba agenta a klikněte na tlačítko **Další**.

    ![Cesta toohello sdílený soubor](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Vyberte hello stav systému verze má toorestore a klikněte na **Další**.

9. V podokně hello vybrat typ obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**.

10. Pro umístění hello hello obnovení stavu systému, vyberte **původního umístění**a klikněte na tlačítko **Další**.

11. Zkontrolujte podrobnosti o potvrzení hello, ověřte nastavení restartování hello a klikněte na tlačítko **obnovit** tooapplly hello obnovit stav systému souborů.

    ![hello spuštění obnovení stavu systému souborů](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Zvláštní upozornění pro obnovení stavu systému na serveru služby Active Directory

Zálohování stavu systému zahrnuje data služby Active Directory. Pomocí následujících kroků toorestore služba Active Directory Domain Services (AD DS) z jeho aktuální stav předchozí tooa stavu hello.

1. Restartujte řadič domény hello v adresářových služeb obnovení režimu (DSRM).
2. Postupujte podle kroků hello [sem](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse zálohování serveru rutiny toorecover služby AD DS.


## <a name="troubleshoot-failed-system-state-restore"></a>Řešení potíží se nezdařilo obnovení stavu systému

Pokud předchozí proces hello použití stavu systému úspěšně nedokončí, můžete použijte modul snap-in prostředí Windows Recovery Environment (Win RE) toorecover hello systému Windows Server. Hello následující kroky popisují, jak pomocí prostředí Windows RE toorecover. Tuto možnost použijte pouze v případě systému Windows Server není normálnímu spuštění po obnovení stavu systému. Hello postupu vymaže data nesystémové, buďte opatrní. 

1. Spouštění systému Windows Server do hello prostředí Windows Recovery Environment (Win RE).

2. Vyberte Poradce při potížích ze tří dostupných možností hello.

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-1.png)

3. Z hello **pokročilé možnosti** obrazovku, vyberte **příkazového řádku** a zadejte uživatelské jméno správce serveru hello a heslo.

   ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-2.png)

4. Zadejte uživatelské jméno správce serveru hello a heslo.

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-3.png)

5. Když otevřete hello příkazový řádek v režimu správce, spusťte následující příkaz tooget hello stav systému zálohování verze.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-4.png)

6. Spusťte následující příkaz tooget hello všechny svazky, které jsou k dispozici v hello zálohování.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-5.png)

7. Hello následující příkaz obnoví všechny svazky, které jsou součástí hello zálohování stavu systému. Všimněte si, že tento krok obnoví pouze hello důležitých svazků, které jsou součástí stavu systému hello. Všechna nesystémové data budou vymazána.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Další kroky
* Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).
