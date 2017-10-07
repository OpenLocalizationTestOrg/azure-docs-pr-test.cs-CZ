---
title: "aaaBack až tooAzure stavu systému Windows | Microsoft Docs"
description: "Přečtěte si tooback hello stav systému Windows Server nebo tooAzure počítače Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "jak toobackup; jak tooback; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Zálohování stavu systému Windows v nasazení Resource Manager
Tento článek vysvětluje, jak tooback systému Windows Server stavu tooAzure. Je kurz určený toowalk vás provede základy hello.

Pokud chcete, aby tooknow Další informace o zálohování Azure, přečtěte si to [přehled](backup-introduction-to-azure-backup.md).

Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
tooback soubory a složky, musíte toocreate trezoru služeb zotavení v hello oblasti, kde chcete toostore hello data. Budete také potřebovat toodetermine způsob replikace úložiště.

### <a name="toocreate-a-recovery-services-vault"></a>toocreate trezoru služeb zotavení
1. Pokud jste tak již neučinili, přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.
2. V nabídce centra hello, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **služeb zotavení** a klikněte na tlačítko **trezory služeb zotavení**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Pokud jsou v rámci předplatného hello trezory služeb zotavení, jsou uvedeny hello trezorů.
3. Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Pro **název**, zadejte popisný název tooidentify hello trezoru. Název Hello musí toobe jedinečný pro hello předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.

5. V hello **předplatné** pomocí hello rozevírací nabídky toochoose hello předplatného Azure. Pokud chcete použít jenom jedno předplatné, zobrazí se toto předplatné a toohello další krok můžete vynechat. Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné. Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.

6. V hello **skupiny prostředků** části:

    * Vyberte **vytvořit nový** Pokud chcete, aby toocreate skupinu prostředků.
    Nebo
    * Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.

  Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello. Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.

8. Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.

    Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu. Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení. Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.

### <a name="set-storage-redundancy-for-hello-vault"></a>Nastavit redundance úložiště pro trezor hello
Při vytváření trezoru služeb zotavení, ujistěte se, že redundance úložiště nakonfigurovaných hello požadovaným způsobem.

1. Z hello **trezory služeb zotavení** okně klikněte na tlačítko Nový trezor hello.

    ![Vyberte nový trezor hello ze seznamu hello trezor služeb zotavení](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Když vyberete hello trezoru, hello **trezor služeb zotavení** narrows okno a okno nastavení hello (*jehož hello název trezoru hello v horní části hello*) a otevřete okno Podrobnosti trezoru hello.

    ![Zobrazení hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. V okně Nastavení hello nový trezor, použijte hello svislé snímku tooscroll dolů toohello části Správa a klikněte na tlačítko **infrastruktura zálohování**.
    Otevře se okno infrastruktura zálohování Hello.
3. V okně infrastruktura zálohování hello, klikněte na tlačítko **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno.

    ![Nastavit hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Zvolte hello vhodnou možnost replikace pro svůj trezor.

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud používáte Azure jako koncový bod primární úložiště záloh, pokračujte toouse **geograficky redundantní**. Pokud nepoužíváte Azure jako koncový bod primární úložiště záloh, zvolte **místně redundantní**, což snižuje náklady na úložiště Azure hello. Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).

Teď, když jste vytvořili trezor, můžete ho nakonfigurujte pro zálohování stavu systému Windows.

## <a name="configure-hello-vault"></a>Konfigurace hello trezoru
1. Na hello okno trezoru služeb zotavení (pro hello trezor jste právě vytvořili), v části Začínáme hello, klikněte na tlačítko **zálohování**, pak na hello **Začínáme se zálohováním** vyberte  **Cíle zálohování**.

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Hello **cíl zálohování** otevře se okno.

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Z hello **kde běží vaše úloha?** rozevírací nabídky vyberte **místní**.

    Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.

3. Z hello **co chcete toobackup?** nabídce vyberte možnost **stav systému**a klikněte na tlačítko **OK**.

    ![Konfigurace souborů a složek](./media/backup-azure-system-state/backup-goal-system-state.png)

    Po kliknutí na tlačítko OK, objeví se zaškrtnutí další příliš**cíl zálohování**a hello **připravit infrastrukturu** otevře se okno.

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. Na hello **připravit infrastrukturu** okně klikněte na tlačítko **stáhnout agenta pro Windows Server nebo klienta Windows**.

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Pokud používáte Windows Server nezbytné, zvolte toodownload hello agenta pro Windows Server nezbytné. Místní nabídky vás vyzve k toorun nebo uložit MARSAgentInstaller.exe.

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. V místní nabídce hello stahování, klikněte na tlačítko **Uložit**.

    Ve výchozím nastavení, hello **MARSagentinstaller.exe** soubor je uložen tooyour složky se staženými soubory. Po dokončení instalačního programu hello, zobrazí se automaticky otevírané okno s dotazem, pokud má instalační program hello toorun nebo otevřete složku hello.

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Tooinstall hello agent ještě nepotřebujete. Po stažení hello přihlašovací údaje trezoru můžete nainstalovat agenta hello.

6. Na hello **připravit infrastrukturu** okně klikněte na tlačítko **Stáhnout**.

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    přihlašovací údaje trezoru Hello tooyour stahování složka pro stahování. Po dokončení stahování přihlašovací údaje trezoru hello zobrazí automaticky otevírané okno s dotazem, pokud mají být tooopen nebo uložit přihlašovací údaje hello. Klikněte na **Uložit**. Pokud omylem kliknete **otevřete**, umožní hello dialog, který se pokusí přihlašovací údaje trezoru hello tooopen, nezdaří. Nelze otevřít přihlašovací údaje trezoru hello. Pokračujte dalším krokem toohello. přihlašovací údaje trezoru Hello jsou ve složce pro stahování hello.   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a>Instalace a registrace agenta hello

> [!NOTE]
> Povolení zálohování prostřednictvím portálu Azure hello ještě není k dispozici. Použijte agenta služeb zotavení Microsoft Azure tooback hello zálohu stavu systému Windows Server.
>

1. Vyhledejte a dvakrát klikněte na hello **MARSagentinstaller.exe** z hello stahování složky (nebo jiného uloženého umístění).

    Instalační program Hello poskytuje řadu zprávy jako extrahuje, nainstaluje a zaregistruje hello agenta služeb zotavení.

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Hello dokončení Průvodce instalací agenta služeb zotavení Microsoft Azure. toocomplete hello průvodce, budete muset:

   * Vyberte umístění složky pro instalaci a mezipaměti hello.
   * Pokud používáte proxy serveru tooconnect toohello zadejte informace o serveru vašeho proxy Internetu.
   * Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.
   * Zadejte přihlašovací údaje trezoru stáhnout hello
   * Šifrovací přístupové heslo hello uložte na bezpečném místě.

     > [!NOTE]
     > Pokud ztratíte nebo zapomenete heslo hello, Microsoft vám nemůže pomoci obnovit zálohovaná data hello. Hello soubor uložte na bezpečném místě. Je požadovaná toorestore zálohu.
     >
     >

Hello agent je nyní nainstalovaný a váš počítač je registrovaný toohello trezoru. Máte připravené tooconfigure a naplánovat zálohování.

## <a name="back-up-windows-server-system-state-preview"></a>Zálohování stavu systému Windows Server (Preview)
Hello prvotní záloha zahrnuje tři úkoly:

* Povolit zálohování stavu systému pomocí agenta Azure Backup hello
* Plán zálohování hello
* Zálohování souborů a složek pro hello poprvé

toocomplete hello prvotní zálohování, agenta služeb zotavení Microsoft Azure použijte hello.

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a>zálohování stavu systému tooenable pomocí agenta Azure Backup hello

1. V relaci prostředí PowerShell spusťte následující příkaz toostop hello Azure Backup modul hello.

  ```
  PS C:\> Net stop obengine
  ```

2. Otevřete hello registru systému Windows.

  ```
  PS C:\> regedit.exe
  ```

3. Přidejte hodnotu DWORD s hodnotou zadanou hello následující klíč registru s hello.

  | Cesta k registru | Klíč registru | Přidejte hodnotu DWord |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Restartujte hello modul Backup spuštěním hello následující příkaz v příkazovém řádku se zvýšenými oprávněními.

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a>Úloha zálohování tooschedule hello

1. Otevřete agenta služeb zotavení Microsoft Azure hello. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Spuštění agenta služeb zotavení Azure hello](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. V hello agenta služeb zotavení, klikněte na **plánem zálohování**.

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Na hello Začínáme stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.

4. Na stránce tooBackup hello vyberte položky, klikněte na **přidat položky**.

5. Vyberte **stav systému** a pak klikněte na **OK**.

6. Klikněte na **Další**.

7. Hello plán zálohování stavu systému a jejich uchovávání bude automaticky nastavena tooback až každých neděle ve 21:00:00 místního času, a je nastavit dobu uchování hello too60 dnů.

   > [!NOTE]
   > Zálohování a uchovávání zásad stavu systému je automaticky nakonfigurovaná. Pokud zálohujete soubory a složky kromě stav systému serveru systému Windows toohello, zadejte pouze hello zálohování a uchovávání zásady pro soubor zálohy z Průvodce hello. 
   >

8. Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.

9. Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a>tooback zálohu stavu systému Windows Server pro hello poprvé

1. Ujistěte se, že neexistují žádné čekající aktualizace pro systém Windows Server, které vyžadují restart počítače.

2. V hello agenta služeb zotavení, klikněte na **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač. Poté klikněte na **Zálohovat**.

4. Klikněte na tlačítko **Zavřít** tooclose hello průvodce. Pokud hello průvodce zavřete před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.

5. Pokud zálohujete soubory a složky na serveru, kromě stav systému serveru systému Windows toohello, Průvodce zálohování nyní hello pouze zálohovat soubory. tooperform ad hoc stav systému zálohovat, použijte následující příkaz prostředí PowerShell hello:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.

  ![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

Hello následující otázky a odpovědi poskytovat doplňující informace.

### <a name="what-is-hello-staging-volume"></a>Co je hello pracovní svazku?

Hello pracovní svazku představuje hello zprostředkující umístění, kde hello nativně dostupné, Windows Server Backup zpracuje hello zálohování stavu systému. Azure Backup agent pak komprimuje a šifruje zprostředkující záloha a odešle, že prostřednictvím zabezpečeného toohello protokol HTTPS nakonfigurovaná trezoru služeb zotavení. **Důrazně doporučujeme že vytvořit hello pracovní svazek na svazku bez operačního systému Windows. Pokud zjistíte problémy s zálohování stavu systému, kontrola hello umístění svazku pracovní je prvním krokem řešení potíží hello.** 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a>Jak můžete změnit hello cesty ke svazku pracovní zadaný v hello agenta Azure Backup?

Hello pracovní svazku se nachází ve složce mezipaměti hello ve výchozím nastavení. 

1. toochange toto umístění hello použijte následující příkaz (v příkazovém řádku se zvýšenými):
  ```
  PS C:\> Net stop obengine
  ```

2. Aktualizujte hello následující položky registru s hello cesta toohello nový svazek pracovní složky.

  |Cesta k registru|Klíč registru|Hodnota|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | nový pracovní umístění svazku |

Hello cesta pracovní velká a malá písmena a musí být hello přesně stejnou velká a malá písmena jako co existuje v serveru pro hello. 

3. Jakmile změníte cestu svazku pracovní hello, restartujte modul Backup hello:
  ```
  PS C:\> Net start obengine
  ```
4. toopick hello změnit cestu, otevřete hello agenta služeb zotavení Microsoft Azure a aktivační události ad hoc zálohu stavu systému.

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a>Proč je stav systému hello výchozí uchování nastavit dny too60?

Hello životnosti zálohu stavu systému je hello stejné jako nastavení hello "životnosti objektů označených jako neplatné" pro roli systému Windows Server Active Directory hello. Hello výchozí hodnota pro položku životnosti objektů označených jako neplatné hello je 60 dnů. Tuto hodnotu lze nastavit u objektu konfigurace služby Directory (NTDS) hello.

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a>Jak změnit výchozí hello zálohování a zásadami uchovávání informací pro stav systému?

toochange hello výchozí zálohování a zásadami uchovávání informací pro stav systému:
1. Zastavte modul zálohování hello. Spusťte následující příkaz z příkazového řádku se zvýšenými oprávněními hello.

  ```
  PS C:\> Net stop obengine
  ```

2. Přidat nebo aktualizovat hello následující položky klíče registru v HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.

  |Název registru|Popis|Hodnota|
  |-------------|-----------|-----|
  |SSBScheduleTime|Použít tooconfigure hello čas hello zálohy. Výchozí hodnota je 21: 00 místního času.|DWord: Formátu hh: mm (decimální): pro příklad. 2130 21:30:00 místního času|
  |SSBScheduleDays|Použít tooconfigure hello dní při zálohování stavu systému, musí se provést na hello zadat čas. Jednotlivé číslic zadejte počet dnů v týdnu hello. představuje neděli 0, 1 je pondělí, a tak dále. Výchozí den pro zálohování je neděli.|DWord: počet dnů hello týden toorun zálohování (decimální): Příklad 1 230 plány zálohování v pondělí, úterý, středu a neděli.|
  |SSBRetentionDays|Použít tooconfigure hello dnů tooretain zálohování. Výchozí hodnota je 60. Maximální povolená hodnota je 180.|DWord: Zálohování tooretain v dní (decimal).|

3. Použijte následující příkaz toorestart hello modulu zálohování hello.
    ```
    PS C:\> Net start obengine
    ```

4. Otevřete agenta služeb zotavení Microsoft hello.

5. Klikněte na tlačítko **plánem zálohování** a pak klikněte na **Další** dokud neuvidíte hello změny projeví.

6. Klikněte na tlačítko **Dokončit** tooapply hello změny.


## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
* Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).
* Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).
* Pokud potřebujete toorestore zálohu, použijte tento článek příliš[obnovit soubory tooa Windows počítač](backup-azure-restore-windows-server.md).
