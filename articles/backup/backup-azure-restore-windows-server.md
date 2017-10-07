---
title: "aaaRestore data v Azure tooa systému Windows Server nebo počítač se systémem Windows | Microsoft Docs"
description: "Zjistěte, jak toorestore data uložená v Azure tooa systému Windows Server nebo počítač se systémem Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Obnovit soubory tooa Windows server nebo klientský počítač systému Windows pomocí modelu nasazení Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](backup-azure-restore-windows-server.md)
> * [Portál Classic](backup-azure-restore-windows-server-classic.md)
>
>

Tento článek vysvětluje, jak toorestore data z trezoru záloh. toorestore data, která používáte Průvodce obnovení dat hello v agentovi Microsoft Azure Recovery Services (MARS) hello. Při obnovování dat, je možné:

* Obnovení dat toohello stejný počítač, ze které hello zálohy byly provedeny.
* Obnovení dat tooan alternativní počítače.

V lednu 2017 společnost Microsoft vydala agenta MARS toohello aktualizace verzi Preview. Společně s oprav chyb, tato aktualizace umožňuje rychlé obnovení, což vám umožní toomount obnovení zapisovatelného snímku bodu jako svazek obnovení. Pak můžete zkoumat hello obnovení svazku a zkopírujte soubory tooa místního počítače a následné obnovení souborů.

> [!NOTE]
> Hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete, aby toouse rychlé obnovení dat toorestore. Hello zálohovaná data musí být chráněna v trezorů v uvedené v článku podpory hello národní prostředí. Poraďte se hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) hello nejnovější seznam národních prostředí, které podporují rychlé obnovení. Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.
>

Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v hello portál Azure a trezory Backup portálu classic hello. Pokud chcete toouse rychlé obnovení, stažení aktualizace MARS hello a postupujte podle hello postupy, které zmínili, rychlé obnovení.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a>Rychlé obnovení dat toohello toorecover použít stejný počítač

Pokud jste omylem odstranili toorestore souboru a chcete ho toohello stejného počítače (z které hello je odebrán zálohování), hello následující kroky vám pomůže obnovit hello data.

1. Otevřete hello **Microsoft Azure Backup** přichycení v. Pokud si nejste jisti, kam se nainstaloval hello modul snap-in, vyhledávat hello počítači nebo serveru pro **Microsoft Azure Backup**.

    aplikace na ploše Hello by se zobrazit ve výsledcích hledání hello.

2. Klikněte na tlačítko **obnovit Data** toostart hello průvodce.

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. Na hello **Začínáme** podokně, toorestore hello data toohello stejný server nebo počítač, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.

    ![Vyberte tento server možnost toorestore hello data toohello stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Na hello **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.

    Na hello kalendáři vyberte bod obnovení. Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **připojit**.

    Zálohování Azure připojí bodu místní obnovení hello a používá je jako svazek obnovení.

7. Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. V Průzkumníku Windows, kopie hello soubory nebo složky toorestore a vložit je tooany umístění místní toohello serveru nebo počítači. Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.

    ![Kopírovat a vkládat soubory a složky z připojeného svazku toolocal umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**. Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.

    ![Odpojení hello svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené 6 hodin od času hello, pokud byla připojena. Čas připojení hello je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů. Žádná operace zálohování se spustí hello svazek je připojen. Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.
    >


## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a>Použít rychlé obnovení toorestore data tooan alternativní počítač
Pokud dojde ke ztrátě celý server, stále můžete obnovit data z Azure Backup tooa jiný počítač. Následující kroky Hello znázorňují hello pracovního postupu.


zahrnuje Hello terminologie použitá v těchto kroků:

* *Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.
* *Cílový počítač* – hello počítač toowhich hello data obnovena.
* *Ukázka trezoru* – hello toowhich trezoru služeb zotavení hello *zdrojový počítač* a *cílový počítač* jsou registrované. <br/>

> [!NOTE]
> Zálohování nemůže být starší verzí operačního systému hello obnovené tooa cílový počítač. Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače. Zálohy z počítače se systémem Windows 8 nemůže být počítač obnovený tooa Windows 7.
>
>

1. Otevřete hello **Microsoft Azure Backup** přichycení v na hello *cílový počítač*.

2. Ujistěte se, hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello trezoru stejné služeb zotavení.

3. Klikněte na tlačítko **obnovit Data** tooopen hello **Průvodce obnovení dat**.

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. Na hello **Začínáme** podokně, vyberte **jiný server**

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*a klikněte na tlačítko **Další**.

    Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost), stáhněte si nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portálu Azure. Po zadání přihlašovacích údajů platné úložiště, zobrazí se název hello hello odpovídající úložiště záloh.


6. Na hello **vyberte zálohování serveru** podokně, vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů a zadejte přístupové heslo hello. Pak klikněte na tlačítko **Další**.

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Na hello **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.

    Na hello kalendáři vyberte bod obnovení. Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klikněte na tlačítko **připojit** toolocally přípojného hello obnovení bodu jako svazek obnovení na vaše *cílový počítač*.

10. Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. V Průzkumníku Windows, zkopírujte hello soubory nebo složky ze svazku obnovení hello a vložte je tooyour *cílový počítač* umístění. Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**. Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené 6 hodin od času hello, pokud byla připojena. Čas připojení hello je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů. Žádná operace zálohování se spustí hello svazek je připojen. Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.
    >

## <a name="troubleshooting"></a>Řešení potíží
Pokud nelze Azure Backup úspěšně připojit svazek hello obnovení ani po několika minutách kliknutí na možnost **připojit** nebo selže toomount hello obnovení svazku s jedné nebo více chybám, postupujte podle hello kroků toobegin obnovení normálně.

1.  Proces probíhající připojení hello zrušte, v případě, že po spuštění pro několik minut.

2.  Ujistěte se, že jste na nejnovější verzi agenta Azure Backup hello hello. toofind informace hello verze agenta Azure Backup, klikněte na **o Microsoft agenta služeb zotavení Azure** na hello **akce** podokně služby Microsoft Azure Backup konzoly a ujistěte se, že hello  **Verze** číslo je rovna tooor vyšší než verze hello uvedený v [v tomto článku](https://go.microsoft.com/fwlink/?linkid=229525). Můžete stáhnout nejnovější verzi hello [sem](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Přejděte příliš**Správce zařízení** -> **řadiče úložiště** a ujistěte se, že je možné najít **iniciátor iSCSI společnosti Microsoft**. Pokud abyste ji mohli najít, přejděte přímo toostep 7 níže. 

4.  Pokud služba iniciátoru iSCSI společnosti Microsoft nelze najít, jak je uvedeno v kroku 3, zkontrolujte toosee, zda můžete najít položku v části **Správce zařízení** -> **řadiče úložiště** názvem  **Neznámé zařízení** s ID hardwaru **ROOT\ISCSIPRT**.

5.  Klikněte pravým tlačítkem na **neznámé zařízení** a vyberte **aktualizace ovladače**.

6.  Aktualizovat ovladač hello výběrem možnosti hello příliš **vyhledání automaticky aktualizovaný ovladač softwaru**. Dokončení hello aktualizace by se měl změnit **neznámé zařízení** příliš**iniciátor iSCSI společnosti Microsoft** jak je uvedeno níže. 

    ![Šifrování](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Přejděte příliš**Správce úloh** -> **služby (místní počítač)** -> **služba iniciátoru iSCSI společnosti Microsoft**. 

    ![Šifrování](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Restartujte službu iniciátoru iSCSI společnosti Microsoft hello kliknutím pravým tlačítkem na službu hello, kliknutím na **Zastavit** a další kliknutím pravým tlačítkem na znovu a kliknete na **spustit**.

9.  Opakujte obnovení pomocí rychlé obnovení. 

Pokud se obnovení hello stále nezdaří, restartujte server nebo klienta. Pokud restartování není žádoucí nebo hello obnovení stále nedaří i po restartování serveru hello, pokuste obnovení provést z alternativní počítače a obraťte se na podporu Azure tak, že přejdete příliš[portálu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a odeslání žádosti o podporu.

## <a name="next-steps"></a>Další kroky
* Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).
