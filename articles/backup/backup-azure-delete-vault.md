---
title: " Odstranit trezor služeb zotavení v Azure | Microsoft Docs "
description: "Jak toodelete Azure Backup a služeb zotavení trezoru. Trezor záloh je možné volat trezoru cloudu Azure nebo Azure recovery vault. Řešení potíží, když nelze odstranit úložiště záloh v portálu classic hello nebo portálu Azure."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: 9047f50f4b2c991fbf2454ddcad08073ec7cd975
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-recovery-services-vault"></a>Odstranění trezoru služby Recovery Services
Hello služby zálohování Azure má dva typy trezorů - hello úložiště záloh a trezoru služeb zotavení hello. první byla přijata Hello úložiště záloh. Potom hello trezor služeb zotavení pocházejí podél nasazení Resource Manager toosupport hello rozšířit. Z důvodu hello rozšířené možnosti a závislosti hello informace, které musí být uložen v trezoru hello odstranění trezoru zálohování nebo obnovení služby může být matoucí. Tento článek vysvětluje, jak toodelete hello trezory v portálu classic hello a hello portálu Azure.  

| **Typ nasazení** | **Azure Portal** | **Název trezoru** |
| --- | --- | --- |
| Classic |Classic |Úložiště záloh |
| Resource Manager |Azure |Trezor služby Recovery Services |

> [!NOTE]
> Trezory Backup nemohou chránit řešení s modelem nasazení Resource Manager. Můžete však použít trezoru služeb zotavení tooprotect classically nasazení serverů a virtuálních počítačů.  
>

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> **15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell. <br/> **Od 1. listopadu 2017**:
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

V tomto článku používáme termín hello, trezoru, toorefer toohello obecný formulář úložiště záloh hello nebo trezor služeb zotavení. Pokud je nutné toodistinguish mezi hello trezory používáme hello formální název, trezor záloh nebo trezor služeb zotavení.

## <a name="deleting-a-recovery-services-vault"></a>Odstranění trezoru služby Recovery Services
Odstranění trezoru služeb zotavení je jednoduchý proces - *zadaný trezor hello neobsahuje žádné prostředky*. Než budete moct odstranit trezor služeb zotavení, musíte odebrat nebo odstranit všechny prostředky v trezoru hello. Když zkusíte toodelete trezoru, který obsahuje prostředky, dojde k chybě jako hello následující bitové kopie:

![Chyba při odstraňování trezoru](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Dokud jste vymazali hello prostředky z trezoru hello, kliknutím na tlačítko **opakujte** vytváří hello stejné chybě. Pokud jste se zablokuje na tato chybová zpráva, klikněte na tlačítko **zrušit** a použití hello následující kroky toodelete hello prostředky v trezoru hello.

### <a name="removing-hello-items-from-a-vault-protecting-a-vm"></a>Odebírání položek hello z trezoru ochranu virtuálního počítače
Pokud již máte otevřete trezoru služeb zotavení hello, toohello druhý krok přeskočte.

1. Otevřete hello portál Azure a z hello řídicí panel otevřete chcete toodelete trezoru hello.

   Pokud nemáte hello trezor služeb zotavení připnutý toohello řídicího panelu, v nabídce centra hello, klikněte na tlačítko **více služeb** a v hello seznamu prostředků zadejte **služeb zotavení**. Během zadávání hello seznamu filtrů podle vašeho zadání. Klikněte na tlačítko **trezory služeb zotavení**.

   ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Zobrazí se Hello seznamu trezorů služeb zotavení. Hello seznamu vyberte, chcete toodelete trezoru hello.

   ![Vyberte možnost trezoru ze seznamu](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. V hello trezoru zobrazení, vyhledejte v hello **Essentials** podokně. toodelete trezoru, nesmí být žádné chráněné položky. Pokud se zobrazí číslo větší než nula, v části buď **zálohování položek** nebo **zálohování serverů pro správu**, musíte odebrat tyto položky před odstraněním hello trezoru.

    ![Podívejte se na chráněných položkách v podokně Essentials](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    Virtuální počítače a soubory a složky jsou považovány za zálohování položek a jsou uvedeny v hello **zálohování položek** oblasti hello Essentials podokna. DPM server je uveden v hello **serveru pro správu zálohování** oblasti hello Essentials podokna. **Replikované položky** týkají toohello služba Azure Site Recovery.
3. odebrání hello toobegin chráněné položky z trezoru hello, najít hello položky v trezoru hello. Na řídicím panelu trezoru hello klikněte na tlačítko **nastavení**a potom klikněte na **zálohování položek** tooopen tohoto okna.

    ![Vyberte možnost trezoru ze seznamu](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    Hello **zálohování položek** okno obsahuje samostatné seznamy, založené na hello typ položky: virtuální počítače Azure nebo složek souborů (viz obrázek). v zobrazeném Hello výchozí typ položky seznamu je virtuálních počítačích Azure. tooview hello seznam položek souborů složek v trezoru hello, vyberte **složek souborů** z rozevírací nabídky hello.
4. Před odstraněním položky z trezoru hello ochranu virtuálního počítače, musíte zastavit úlohu zálohování hello položky a odstranit hello data bodu obnovení. Pro každou položku v trezoru hello postupujte takto:

    a. Na hello **položky zálohování** okně hello klikněte pravým tlačítkem na položku a hello místní nabídce, vyberte **zastavení zálohování**.

    ![Zastavit úlohu zálohování hello](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Otevře se okno zastavení zálohování Hello.

    b. Na hello **zastavení zálohování** okno z hello **zvolte možnost** nabídce vyberte možnost **odstranit Data zálohování** > název typu hello hello položky > a klikněte na tlačítko **zastavit Zálohování**.

    Název typu hello položky hello tooverify chcete toodelete ho. Hello **zastavení zálohování** tlačítko aktivuje, jakmile ověříte hello položky. Pokud nevidíte hello dialogové okno tootype hello název položky zálohování hello, jste zvolili hello **zachovat zálohovaná Data** možnost.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Volitelně můžete zadat důvod, proč jsou odstraňování hello dat a přidání komentářů. Po kliknutí na tlačítko **zastavení zálohování**, povolit hello odstranit úlohu toocomplete před pokusem o toodelete hello trezoru. tooverify, který hello úloha dokončí, zkontrolujte zprávy Azure hello ![odstranit záložní data](./media/backup-azure-delete-vault/messages.png). <br/>
    Po dokončení úlohy hello obdržíte zprávu o proces zálohování hello byla zastavena a hello zálohovaná data pro tuto položku, byla odstraněna.

    c. Po odstranění položky v seznamu hello na hello **zálohování položek** nabídky, klikněte na tlačítko **aktualizovat** toosee hello zbývající položky v trezoru hello.

      ![Odstranit záložní data](./media/backup-azure-delete-vault/empty-items-list.png)

      Jakmile se v seznamu hello neexistují žádné položky, posuňte se toohello **Essentials** podokně okno trezoru zálohování hello. Nesmí být žádné **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky** uvedené. Pokud stále se zobrazí položky v trezoru hello, vraťte toostep tři a vyberte typ seznamu různé položky.  
5. Když v panelu nástrojů hello trezoru nejsou žádné další položky, klikněte na tlačítko **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-vault.png)
6. Klikněte na tlačítko, které chcete toodelete hello trezoru, tooverify **Ano**.

    Hello úložiště je odstranit, a vrátí hello portál toohello **nový** nabídky služby.

## <a name="what-if-i-stopped-hello-backup-process-but-retained-hello-data"></a>Co když I zastavit proces zálohování hello ale hello data uchovávají?
Pokud je ale omylem zastavit proces zálohování hello *uchovávají* hello data, je nutné odstranit hello zálohovaná data před odstraněním hello trezoru. toodelete hello zálohovaných dat:

1. Na hello **položky zálohování** okně hello klikněte pravým tlačítkem na položku a v místní nabídce hello klikněte na tlačítko **odstranit záložní data**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Hello **odstranit Data zálohování** otevře se okno.
2. Na hello **odstranit Data zálohování** okno, název typu hello hello položku a klikněte na tlačítko **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Jakmile jste odstranili hello data, vraťte toostep 4c a pokračujte hello procesu.

## <a name="delete-a-vault-used-tooprotect-a-dpm-server"></a>Odstranit trezor tooprotect serveru aplikace DPM
Než budete moct odstranit trezor tooprotect serveru aplikace DPM, musíte vymazat všechny body obnovení, které byly vytvořeny a potom zrušit registraci serveru hello z trezoru hello.

data hello toodelete přidruženo ke skupině ochrany:

1. V konzole správce aplikace DPM hello, klikněte na **ochrany** > vyberte skupinu ochrany > vyberte hello člena skupiny ochrany > a v hello pásu karet nástroje klikněte na **odebrat**.

  Vyberte hello člena skupiny ochrany tooactivate hello **odebrat** tlačítko pásu karet nástroje hello. V příkladu hello hello člen je **dummyvm9**. tooselect více členy ve skupině ochrany hello, podržte stisknutou klávesu Ctrl hello při kliknutí na členy hello.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    Hello **zastavit ochranu** otevře se dialogové okno.
2. V hello **zastavit ochranu** dialogovém okně, vyberte **odstranit chráněná data**a klikněte na tlačítko **zastavit ochranu**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    toodelete trezoru, musíte vymazat, nebo odstranit, hello trezoru z chráněná data. V závislosti na hello počet bodů obnovení a data ve skupině ochrany hello ho může trvat pár sekund tooseveral minut toodelete hello data. Hello **zastavit ochranu** dialogové okno zobrazí stav hello po dokončení úlohy hello.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Dál tento proces pro všechny členy všech skupin ochrany.

    Odeberte všechny chráněné skupiny dat a ochrany.
4. Po odstranění všechny členy ze skupiny ochrany hello, přepínače toohello portálu Azure. Otevřete řídicí panel hello trezoru a ověřte, zda žádná **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky**. Na panelu nástrojů hello trezoru, klikněte na tlačítko **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-vault.png)

    Pokud je úložiště toohello zaregistrované servery správy záloh, nelze odstranit trezor hello i v případě, že neexistují žádná data v trezoru hello. Pokud jste odstranili přidružený k trezoru hello servery pro správu hello zálohování, ale jsou servery uvedené v hello **Essentials** podokně, najdete v části [najít hello zálohování správy serverů registrovaných toohello trezoru](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault) .
5. Klikněte na tlačítko, které chcete toodelete hello trezoru, tooverify **Ano**.

    Hello úložiště je odstranit, a vrátí hello portál toohello **nový** nabídky služby.

## <a name="delete-a-vault-used-tooprotect-a-production-server"></a>Odstranit trezor tooprotect provozním serveru
Před odstraněním trezor tooprotect provozním serveru, musíte odstranit nebo zrušit registraci serveru hello z trezoru hello.

Provozní server hello toodelete přidružený k trezoru hello:

1. V hello portálu Azure, otevřete řídicí panel hello trezor a klikněte na **nastavení** > **infrastruktura zálohování** > **produkční servery**.

    ![Otevřete okno provozních serverech](./media/backup-azure-delete-vault/delete-production-server.png)

    Hello **produkční servery** okno otevře a obsahuje seznam všech provozních serverů v trezoru hello.

    ![Seznam provozních serverech](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Na hello **produkční servery** , klikněte pravým tlačítkem na hello server a klikněte na tlačítko **odstranit**.

    ![Odstranit provozním serveru ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    Hello **odstranit** otevře se okno.

    ![Odstranit provozním serveru ](./media/backup-azure-delete-vault/delete-blade.png)
3. Na hello **odstranit** okně potvrďte hello název serveru a klikněte na tlačítko **odstranit**. Je třeba správně zadat název serveru hello tooactivate hello **odstranit** tlačítko.

    Po odstranění trezoru hello obdržíte zprávu s oznámením, že byl odstraněn hello trezoru. Po odstranění všech serverech v trezoru hello, posuňte se zpět toohello Essentials podokně na řídicím panelu trezoru hello.
4. Na řídicím panelu trezoru hello, zkontrolujte, že žádné **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky**. Na panelu nástrojů hello trezoru, klikněte na tlačítko **odstranit**.
5. Klikněte na tlačítko, které chcete toodelete hello trezoru, tooverify **Ano**.

    Hello úložiště je odstranit, a vrátí hello portál toohello **nový** nabídky služby.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Odstranit trezor záloh na portálu classic
Hello následující pokyny jsou pro odstranění úložiště záloh na portálu classic hello. Před odstraněním hello úložiště záloh, musíte odstranit hello body obnovení, nebo zálohovat položky a odeberte hello registrované servery. Hello registrované servery jsou hello systému Windows Server, pracovní stanice nebo virtuální počítače, které byly registrované toohello trezoru.

1. Otevřete hello [portálu Classic](https://manage.windowsazure.com).

2. Hello seznamu trezorů záloh vyberte trezor hello chcete toodelete.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Otevře se Hello panelu trezoru. Podívejte se na hello počet virtuálních počítačů Windows servery nebo Azure přidružený k trezoru hello. Podívejte se taky na celkové úložiště hello využívat v Azure. Zastavit všechny úlohy zálohování a odstranění všech dat před odstraněním hello trezoru.

3. Klikněte na tlačítko hello **chráněné položky** a pak klikněte **zastavit ochranu**

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    Hello **zastavit ochranu svůj trezor** otevře se dialogové okno.
4. V hello **zastavit ochranu svůj trezor** dialogové okno, zkontrolujte **odstranit související zálohovaná data** a klikněte na tlačítko ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png). <br/>
    Volitelně můžete vybrat důvod zastavení ochrany a zadejte komentář.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Po odstranění hello položky v trezoru hello, bude prázdný hello trezoru.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. V seznamu hello karty, klikněte na **registrované položky**. Hello **typ** rozevírací nabídky, umožňuje toochoose hello typ trezoru toohello server zaregistrován. Typ Hello může být Windows Server nebo virtuální počítač Azure. V následujícím příkladu hello, vyberte trezor registrované toohello hello virtuální počítač a klikněte na **Unregister**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Pokud chcete toodelete hello registrace pro Windows Server, z hello **typ** rozevírací nabídky vyberte **systému Windows Server**, klikněte na tlačítko ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png) toorefresh úvodní obrazovka, a pak klikněte na **odstranit**. <br/>

  ![Vyberte Server systému Windows](./media/backup-azure-delete-vault/select-windows-server.png)

6. V seznamu hello karty, klikněte na **řídicí panel** tooopen, který kartě. Ověřte, že neexistují žádné registrované servery nebo virtuálních počítačích Azure chráněna v cloudu hello. Také ověřte, že neexistují žádná data v úložišti. Klikněte na tlačítko **odstranit** toodelete hello trezoru.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Otevře se potvrzovací obrazovce a Hello odstranit zálohy trezoru. Vyberte možnost Proč při odstraňování hello trezor a klikněte na ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    Hello úložiště je odstranit, a vrátí toohello řídicí panel portálu classic.

### <a name="find-hello-backup-management-servers-registered-toohello-vault"></a>Najít hello zálohování správy serverů registrovaných toohello trezoru
Pokud máte více tooa trezoru zaregistrované servery, může být obtížné tooremember je. toosee hello servery zaregistrované toohello trezoru a odstraňte je:

1. Panelu trezoru otevřete hello.
2. V hello **Essentials** podokně klikněte na tlačítko **nastavení** tooopen tohoto okna.

    ![Otevřete okno nastavení](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Na hello **okno nastavení**, klikněte na tlačítko **infrastruktura zálohování**.
4. Na hello **infrastruktura zálohování** okně klikněte na tlačítko **servery pro správu zálohování**. Otevře se okno Hello servery pro správu zálohování.

    ![seznam serverů pro správu zálohování](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. toodelete server hello seznamu, klikněte pravým tlačítkem na název hello hello serveru a pak klikněte na **odstranit**.
    Hello **odstranit** otevře se okno.
6. Na hello **odstranit** okno, zadejte název hello hello serveru. Pokud je dlouhý název, můžete zkopírovat a vložit hello seznamu serverů pro správu zálohování. Pak klikněte na tlačítko **odstranit**.  
