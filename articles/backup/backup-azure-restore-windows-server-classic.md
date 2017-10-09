---
title: "hello aaaRestore data tooa systému Windows Server nebo klienta Windows Azure pomocí modelu nasazení classic | Microsoft Docs"
description: "Zjistěte, jak toorestore z Windows serveru nebo klienta Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a>Obnovit soubory tooa Windows server nebo klientský počítač systému Windows pomocí modelu nasazení classic hello
> [!div class="op_single_selector"]
> * [Portál Classic](backup-azure-restore-windows-server-classic.md)
> * [Azure Portal](backup-azure-restore-windows-server.md)
>
>

Tento článek vysvětluje, jak toorecover data ze zálohy trezoru a obnovte ji tooa serveru nebo počítači. Počínaje března 2017, můžete již nemohou vytvářet záloh na portálu classic hello.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> **15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell. <br/> **Od 1. listopadu 2017**:
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

toorestore data, která používáte Průvodce obnovení dat hello v agentovi Microsoft Azure Recovery Services (MARS) hello. Při obnovování dat, je možné:

* Obnovení dat toohello stejný počítač, ze které hello zálohy byly provedeny.
* Obnovení dat tooan alternativní počítače.

V lednu 2017 společnost Microsoft vydala agenta MARS toohello aktualizace verzi Preview. Společně s oprav chyb, tato aktualizace umožňuje rychlé obnovení, což vám umožní toomount obnovení zapisovatelného snímku bodu jako svazek obnovení. Pak můžete zkoumat hello obnovení svazku a zkopírujte soubory tooa místního počítače a následné obnovení souborů.

> [!NOTE]
> Hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete, aby toouse rychlé obnovení dat toorestore. Hello zálohovaná data musí být chráněna v trezorů v uvedené v článku podpory hello národní prostředí. Poraďte se hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) hello nejnovější seznam národních prostředí, které podporují rychlé obnovení. Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.
>

Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v hello portál Azure a trezory Backup portálu classic hello. Pokud chcete toouse rychlé obnovení, stažení aktualizace MARS hello a postupujte podle hello postupy, které zmínili, rychlé obnovení.


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
    > Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené šest hodin od času hello, pokud byla připojena. Žádná operace zálohování se spustí hello svazek je připojen. Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.
    >


## <a name="recover-data-toohello-same-machine"></a>Obnovení dat toohello stejný počítač
Pokud jste omylem odstranili toorestore souboru a chcete ho toohello stejného počítače (z které hello je odebrán zálohování), hello následující kroky vám pomůže obnovit hello data.

1. Otevřete hello **Microsoft Azure Backup** přichycení v.
2. Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Vyberte hello  **tento server (*yourmachinename*) ** hello toorestore možnost zálohovat soubor na hello stejný počítač.

    ![Stejný počítač](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Zvolte příliš**Procházet soubory** nebo **vyhledávání souborů**.

    Ponechte výchozí možnost hello Pokud máte v plánu toorestore jeden nebo více souborů, jejichž cesta je známá. Pokud nejste jisti o hello struktura složek, ale chcete toosearch pro soubor, vyberte hello **vyhledávání souborů** možnost. Hello za účelem v této části budeme pokračovat s hello výchozí možnost.

    ![Procházet soubory](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Vyberte svazek hello, ze kterého chcete soubor toorestore hello.

    Můžete obnovit z libovolného bodu v čase. Data, které jsou v **tučné** v ovládacím prvku Kalendář hello znamenat hello dostupnost bodu obnovení. Jakmile je vybrat datum, na základě vašeho plánu zálohování (a hello úspěch operace zálohování), můžete vybrat bod v čase, ze hello **čas** rozevírací nabídku.

    ![Svazek a datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Vyberte položky toorecover hello. Můžete vybrat víc chcete toorestore složek nebo souborů.

    ![Výběr souborů](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Zadejte parametry obnovení hello.

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Máte možnost obnovení původního umístění toohello (v které hello soubor nebo složku, budou přepsána) nebo tooanother umístění v hello stejný počítač.
   * Pokud hello soubor nebo složku, toorestore existuje v cílovém umístění hello, můžete vytvořit kopie (hello dvě verze stejného souboru), přepsat hello soubory v cílovém umístění hello nebo přeskočit obnovení hello hello souborů, které existují v cílovém hello.
   * Důrazně doporučujeme ponechat hello výchozí možnost obnovení hello seznamy ACL v hello soubory, které se obnovuje.
8. Jakmile jsou k dispozici tyto vstupy, klikněte na možnost **Další**. Postup obnovení Hello, která obnoví hello soubory toothis počítač, začne.

## <a name="recover-tooan-alternate-machine"></a>Obnovit tooan alternativní počítač
Pokud dojde ke ztrátě celý server, stále můžete obnovit data z Azure Backup tooa jiný počítač. Následující kroky Hello znázorňují hello pracovního postupu.  

zahrnuje Hello terminologie použitá v těchto kroků:

* *Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.
* *Cílový počítač* – hello počítač toowhich hello data obnovena.
* *Ukázka trezoru* – hello zálohy trezoru toowhich hello *zdrojový počítač* a *cílový počítač* jsou registrované. <br/>

> [!NOTE]
> Zálohy vytvořené z počítače nelze obnovit v počítači, který běží starší verze operačního systému hello. Například pokud zálohy jsou převzaty z počítače s Windows 7, může být obnovena do systému Windows 8 nebo novější verze počítače. Naopak hello jsou však nemá hodnotu true.
>
>

1. Otevřete hello **Microsoft Azure Backup** přichycení v na hello *cílový počítač*.
2. Ujistěte se, že hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello stejné úložiště záloh.
3. Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Vyberte **jiný server**

    ![Jiný Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*. Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost) stáhnout nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portál Azure classic. Jakmile je zadaný soubor s přihlašovacími údaji trezoru hello, se zobrazí úložiště záloh hello proti soubor s přihlašovacími údaji trezoru hello.
6. Vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů.

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Vyberte buď hello **vyhledávání souborů** nebo **Procházet soubory** možnost. Hello za účelem v této části, použijeme hello **vyhledávání souborů** možnost.

    ![Search](./media/backup-azure-restore-windows-server-classic/search.png)
8. Vyberte svazek hello a datum na další obrazovce hello. Vyhledávání pro název složky nebo souboru hello chcete toorestore.

    ![Hledání položek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Vyberte hello umístění, kde hello soubory musí toobe obnovit.

    ![Obnovení umístění](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Zadejte šifrovací přístupové heslo hello, který jste zadali během *zdrojový počítač* registrace příliš*ukázka trezoru*.

    ![Šifrování](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Jakmile je k dispozici vstup hello, klikněte na možnost **obnovit**, které aktivační události hello obnovení hello zálohovat soubory toohello cíle zadaného.

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
    > Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené šest hodin od času hello, pokud byla připojena. Žádná operace zálohování se spustí hello svazek je připojen. Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.
    >


## <a name="next-steps"></a>Další kroky
* [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
* Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Další informace
* [Přehled služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Zálohování virtuálních počítačů Azure](backup-azure-vms-introduction.md)
* [Zálohování se úlohy Microsoft](backup-azure-dpm-introduction.md)
