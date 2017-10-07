---
title: "tooback agenta Azure Backup aaaUse soubory a složky | Microsoft Docs"
description: "Použijte hello Microsoft Azure Backup agent tooback až tooAzure soubory a složky systému Windows. Vytvoření trezoru služeb zotavení, nainstalujte agenta zálohování hello, definovat zásady zálohování hello a spusťte prvotní zálohování hello hello souborů a složek."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Trezor záloh; zálohování na Windows server. zálohování systému windows."
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: be203c24841971872b5c6e7cf260a2fa5c58ac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-client-tooazure-using-hello-resource-manager-deployment-model"></a>Zálohování Windows serveru nebo klienta tooAzure pomocí modelu nasazení Resource Manager hello
> [!div class="op_single_selector"]
> * [Azure Portal](backup-configure-vault.md)
> * [Portál Classic](backup-configure-vault-classic.md)
>
>

Tento článek vysvětluje, jak hello soubory a složky tooAzure tooback Windows serveru (nebo klienta Windows) s Azure Backup pomocí modelu nasazení Resource Manager.

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Kroky procesu zálohování](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Než začnete
tooback serveru nebo klienta tooAzure, potřebujete účet Azure. Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
Trezor služeb zotavení je entita, která ukládá všechny hello zálohy a body obnovení, které vytvoříte v čase. Hello trezor služeb zotavení obsahuje také hello zásady zálohování použije toohello chráněné soubory a složky. Při vytváření trezoru služeb zotavení, je také možnost hello příslušné úložiště redundance.

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

    * Vyberte **vytvořit nový** Pokud chcete, aby toocreate novou skupinu prostředků.
    Nebo
    * Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.

  Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello. Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.

8. Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.

  Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu. Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení. Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.

  ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.


### <a name="set-storage-redundancy"></a>Sada redundance úložiště
Při prvním vytvoření trezoru Služeb zotavení určíte, jak má být úložiště replikované.

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

Teď, když jste vytvořili trezor, Příprava stahování a instalace agenta služeb zotavení Microsoft Azure hello, stáhnete pověření k úložišti a potom pomocí těchto pověření tooregister hello agenta s vaší infrastruktury tooback soubory a složky Trezor Hello.

## <a name="configure-hello-vault"></a>Konfigurace hello trezoru

1. Na hello okno trezoru služeb zotavení (pro hello trezor jste právě vytvořili), v části Začínáme hello, klikněte na tlačítko **zálohování**, pak na hello **Začínáme se zálohováním** vyberte  **Cíle zálohování**.

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  Hello **cíl zálohování** otevře se okno. Pokud byl dříve nakonfigurován hello trezor služeb zotavení, pak hello **cíl zálohování** okna otevře po kliknutí na tlačítko **zálohování** na služeb zotavení hello trezoru okno.

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Z hello **kde běží vaše úloha?** rozevírací nabídky vyberte **místní**.

  Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.

3. Z hello **co chcete toobackup?** nabídce vyberte možnost **soubory a složky**a klikněte na tlačítko **OK**.

  ![Konfigurace souborů a složek](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

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
> Povolení zálohování prostřednictvím portálu Azure hello ještě není k dispozici. Použijte agenta služeb zotavení Microsoft Azure tooback hello soubory a složky.
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

## <a name="network-and-connectivity-requirements"></a>Požadavky na síť a připojení

Pokud váš počítač nebo na proxy server má omezený přístup k Internetu, zajistěte, aby nastavení brány firewall na počítač nebo proxy server hello jsou nakonfigurované tooallow hello následující adresy URL: <br>
    1. www.msftncsi.com
    2. *.Microsoft.com
    3. *.WindowsAzure.com
    4. *.microsoftonline.com
    5. *.windows.ne


## <a name="create-hello-backup-policy"></a>Vytvořit zásady zálohování hello
zásady zálohování Hello je plán hello, pokud body obnovení jsou pořízeny a hello dobu, kterou se uchovají body obnovení hello. Použijte hello Microsoft Azure Backup agent toocreate hello zásady zálohování pro soubory a složky.

### <a name="toocreate-a-backup-schedule"></a>toocreate plán zálohování
1. Otevřete hello Microsoft Azure Backup agent. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Spuštění agenta Azure Backup hello](./media/backup-configure-vault/snap-in-search.png)
2. V agentu Backup hello **akce** podokně klikněte na tlačítko **plánem zálohování** toolaunch hello Průvodce plánem zálohování.

    ![Naplánování zálohování Windows Serveru](./media/backup-configure-vault/schedule-first-backup.png)

3. Na hello **Začínáme** stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.
4. Na hello **vyberte položky tooBackup** klikněte na tlačítko **přidat položky**.

  Otevře se dialogové okno Vybrat položky Hello.

5. Vyberte hello soubory a složky mají tooprotect a pak klikněte na tlačítko **OK**.
6. V hello **vyberte položky tooBackup** klikněte na tlačítko **Další**.
7. Na hello **zadání plánu zálohování** zadejte hello plán zálohování a klikněte na tlačítko **Další**.

    Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.

    ![Položky k zálohování z Windows Serveru](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > Další informace o tom, jak toospecify hello plán zálohování, najdete v článku hello [pomocí Azure Backup tooreplace infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. Na hello **výběr zásady uchovávání informací** , zvolte hello zásady hello konkrétní uchování pro záložní kopii hello a klikněte na tlačítko **Další**.

    Hello zásady uchovávání informací Určuje dobu hello které hello záloha uložená. Místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací podle toho, kdy dochází k zálohování hello. Můžete upravit hello uchování denní, týdenní, měsíční a roční zásady toomeet vašim potřebám.
9. Na stránce Výběr typu prvotní zálohy hello vyberte typ prvotní zálohy hello. Ponechte možnost hello **automaticky přes síť hello** vybrané a potom klikněte na **Další**.

    Můžete zálohovat automaticky přes síť hello nebo můžete zálohovat do offline režimu. Hello zbývající část tohoto článku popisuje proces hello zálohování automaticky. Pokud dáváte přednost toodo zálohu do offline režimu, přečtěte si článek hello [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md) Další informace.
10. Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.
11. Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.

### <a name="enable-network-throttling"></a>Povolit omezení šířky pásma sítě
Microsoft Azure Backup agent Hello poskytuje, omezení šířky pásma sítě. Omezení využití šířky pásma sítě při přenosu dat ovládací prvky. Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz. Omezení platí tooback zálohu a obnovte aktivity.

> [!NOTE]
> Omezení šířky pásma sítě není k dispozici v systému Windows Server 2008 R2 SP1, Windows Server 2008 SP2 nebo Windows 7 (s aktualizací service Pack). omezení šířky funkce sítě Azure Backup Hello zapojí Quality of Service (QoS) hello místního operačního systému. I když Azure Backup dokáže ochránit těchto operačních systémů, hello verze technologie QoS, které jsou k dispozici na těchto platformách nefunguje s omezení sítě Azure Backup. Omezení šířky pásma sítě můžete použít na jiných [podporované operační systémy](backup-azure-backup-faq.md).
>
>

**omezení tooenable sítě**

1. V rámci Microsoft Azure Backup agent hello, klikněte na tlačítko **změnit vlastnosti**.

    ![Změnit vlastnosti](./media/backup-configure-vault/change-properties.png)
2. Na hello **omezování** karty, vyberte hello **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.

    ![Omezení šířky pásma sítě](./media/backup-configure-vault/throttling-dialog.png)
3. Po povolení omezení šířky pásma, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    hodnoty šířky pásma Hello začít ve 512 kilobitů za sekundu (Kbps) a můžete přejít do too1 023 megabajtů za sekundu (MBps). Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány pracovní dny. Hodiny mimo určené práce, kterou jsou považovány za hodiny mimo pracovní hodiny.
4. Klikněte na **OK**.

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a>tooback soubory a složky pro hello poprvé
1. V hello agenta zálohování, klikněte na **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.

    ![Zálohovat nyní ve Windows Serveru](./media/backup-configure-vault/backup-now.png)
2. Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač. Poté klikněte na **Zálohovat**.
3. Klikněte na tlačítko **Zavřít** tooclose hello průvodce. Pokud to uděláte před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.

Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.

![Dokončení IR](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
Další informace o zálohování virtuálních počítačů nebo dalších úloh najdete v tématu:

* Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).
* Pokud potřebujete toorestore zálohu, použijte tento článek příliš[obnovit soubory tooa Windows počítač](backup-azure-restore-windows-server.md).
