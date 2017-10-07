---
title: "aaaBack až Windows serveru nebo pracovní stanice tooAzure (klasického modelu) | Microsoft Docs"
description: "Windows servery nebo klienty tooa zálohování úložiště záloh v Azure. Projít základy pro ochranu souborů a složek tooa trezoru služby Backup pomocí agenta Azure Backup hello."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Trezor záloh; zálohování na Windows server. zálohování systému windows."
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: c8f2a9bed1e5885f5c272c65b9282ededc05850a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-workstation-tooazure-using-hello-classic-portal"></a>Zálohování Windows serveru nebo pracovní stanice tooAzure pomocí portálu classic hello
> [!div class="op_single_selector"]
> * [Portál Classic](backup-configure-vault-classic.md)
> * [Azure Portal](backup-configure-vault.md)
>
>

Tento článek popisuje postupy hello potřebovat toofollow tooprepare prostředí a zálohování Windows serveru (nebo pracovní stanice) tooAzure. Také vysvětluje aspekty pro nasazení řešení pro zálohování. Pokud vás zajímá pokusu o zálohování Azure pro hello poprvé, tento článek vás rychle provede procesem hello.

Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Resource Manager a Klasický model. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="before-you-start"></a>Než začnete
tooback serveru nebo klienta tooAzure, potřebujete účet Azure. Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.

## <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
tooback soubory a složky ze serveru nebo klienta, musíte toocreate trezoru záloh v hello zeměpisnou oblast, kam chcete toostore hello data.

> [!IMPORTANT]
> Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
>
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> **15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell. <br/> **Od 1. listopadu 2017**:
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>


## <a name="download-hello-vault-credential-file"></a>Stáhněte si soubor s přihlašovacími údaji trezoru hello
na místním počítači Hello musí toobe předtím, než můžete zálohovat data tooAzure k ověření pomocí úložiště záloh. ověřování Hello je dosaženo pomocí *přihlašovací údaje trezoru*. soubor s přihlašovacími údaji trezoru Hello se stáhne prostřednictvím zabezpečeného kanálu z portálu classic hello. privátní klíč certifikátu Hello není zachována v portálu hello nebo hello služby.

### <a name="toodownload-hello-vault-credential-file-tooa-local-machine"></a>toodownload hello úložiště přihlašovacích údajů souboru tooa místního počítače
1. V levém navigačním podokně hello, klikněte na **služeb zotavení**a potom vyberte hello úložiště záloh, které jste vytvořili.

    ![Dokončení IR](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Na stránce hello rychlý Start, klikněte na **přihlašovací údaje trezoru Stáhnout**.

   portál classic Hello generuje přihlašovací údaje úložiště pomocí kombinace hello název trezoru a hello aktuální datum. soubor s přihlašovacími údaji trezoru Hello se používají pouze během pracovního postupu registrace hello a vyprší po 48 hodin.

   soubor s přihlašovacími údaji trezoru Hello si můžete stáhnout z portálu hello.
3. Klikněte na tlačítko **Uložit** toodownload hello úložiště přihlašovacích údajů souboru toohello složky se staženými soubory hello místního účtu. Můžete také vybrat **uložit jako** z hello **Uložit** nabídky toospecify umístění pro soubor s přihlašovacími údaji trezoru hello.

   > [!NOTE]
   > Ujistěte se, že soubor s přihlašovacími údaji trezoru hello je uložen v umístění, které jsou přístupné z počítače. Pokud je uložen v souboru bloku zpráv sdílenou složku nebo server, ověřte, zda máte oprávnění tooaccess hello ho.
   >
   >

## <a name="download-install-and-register-hello-backup-agent"></a>Stáhněte, nainstalujte a zaregistrujte agenta zálohování hello
Po vytvoření úložiště záloh hello a stáhnout soubor s přihlašovacími údaji trezoru hello agenta musí být nainstalován na všech počítačích s Windows.

### <a name="toodownload-install-and-register-hello-agent"></a>toodownload, nainstalujte a zaregistrujte agenta hello
1. Klikněte na tlačítko **služeb zotavení**a potom vyberte hello úložiště záloh, které chcete tooregister se serverem.
2. Na stránce hello rychlý Start, klikněte na agenta hello **agenta pro Windows Server nebo klienta System Center Data Protection Manager nebo Windows**. Potom klikněte na **Uložit**.

    ![Uložit agenta](./media/backup-configure-vault-classic/agent.png)
3. Po souboru MARSagentinstaller.exe hello stáhl, klikněte na tlačítko **spustit** (nebo dvakrát kliknout na **MARSAgentInstaller.exe** z umístění hello uložit).
4. Zvolte hello instalační složku a složku mezipaměti, které jsou požadovány pro hello agenta a pak klikněte na tlačítko **Další**. Hello umístění mezipaměti, které určíte musí mít minimálně 5 procentech zálohovaná data hello rovna tooat volného místa.
5. Můžete pokračovat v tooconnect toohello Internet prostřednictvím nastavení proxy serveru výchozí hello.             Pokud používáte proxy server tooconnect toohello Internetu, na stránce hello konfiguraci proxy serveru, vyberte hello **používat vlastní proxy server nastavení** zaškrtněte políčko a potom zadejte podrobnosti o serveru proxy hello. Pokud používáte ověřený server proxy, zadejte podrobnosti uživatelského jména a hesla hello a pak klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **nainstalovat** instalace agenta toobegin hello. agent zálohování Hello nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (pokud ještě nejsou nainstalované) toocomplete hello instalace.
7. Po instalaci agenta hello, klikněte na tlačítko **pokračovat tooRegistration** toocontinue s pracovním postupem hello.
8. Na stránce úložiště identifikační hello procházet tooand vyberte hello trezoru pověření souboru, který jste dříve stáhli.

    soubor s přihlašovacími údaji trezoru Hello je platný pouze 48 hodin po jeho stažení z portálu hello. Pokud dojde k chybě na této stránce (například "přihlašovací údaje trezoru, které souboru vypršela platnost."), přihlaste toohello portál a znovu stáhnout soubor s přihlašovacími údaji trezoru hello.

    Ujistěte se, že soubor přihlašovacích údajů trezoru hello je k dispozici v umístění, které budou mít přístup hello instalační program aplikace. Pokud narazíte na chyby týkající se přístupu, zkopírujte hello úložiště přihlašovacích údajů tooa dočasné umístění souboru na stejný počítač a opakujte operaci hello hello.

    Pokud dojde k chybě úložiště přihlašovacích údajů, jako je "v zadané údaje neplatný trezoru", soubor hello je poškozen nebo není mít hello nejnovější pověření spojená se službou obnovení hello. Opakujte operaci hello po stažení nový soubor s přihlašovacími údaji trezoru z portálu hello. Této chybě může také dojít, pokud uživatel klikne hello **přihlašovací údaje trezoru Stáhnout** možnost několikrát za sebou rychlé. V takovém případě je platný pouze hello poslední soubor pověření pro úložiště.
9. Na stránce nastavení šifrování hello můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (s minimálně 16 znaků). Mějte na paměti, toosave hello heslo v zabezpečeném umístění.
10. Klikněte na **Dokončit**. Hello Průvodce registrací serveru zaregistruje hello server pomocí zálohování.

    > [!WARNING]
    > Pokud ztratíte nebo zapomenete heslo hello, Microsoft vám nemůže pomoci obnovit zálohovaná data hello. Vlastníte hello šifrovací přístupové heslo a Microsoft nemá přehled hello heslo, které používáte. Uložte soubor hello v zabezpečeném umístění, protože se bude vyžadovat během operace obnovení.
    >
    >

11. Po nastavení hello šifrovací klíč, nechte hello **spusťte Microsoft agenta služeb zotavení Azure** zaškrtnuté políčko a pak klikněte na **Zavřít**.

## <a name="complete-hello-initial-backup"></a>Dokončení hello prvotní zálohování
Hello prvotní záloha zahrnuje dvě klíčové úlohy:

* Vytváření plánu zálohování hello
* Zálohování souborů a složek pro hello poprvé

Po dokončení prvotní zálohování hello zásady zálohování hello vytvoří body zálohy, které můžete použít, pokud potřebujete toorecover hello data. zásady zálohování Hello k tomu podle definovaného plánu hello.

### <a name="tooschedule-hello-backup"></a>tooschedule hello zálohování
1. Otevřete hello Microsoft Azure Backup agent. (Otevře se automaticky Pokud jste nechali hello **spusťte Microsoft agenta služeb zotavení Azure** zaškrtnutým políčkem při zavření hello Průvodce registrací serveru.) Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Spuštění agenta Azure Backup hello](./media/backup-configure-vault-classic/snap-in-search.png)
2. V agentovi hello zálohování, klikněte na tlačítko **plánem zálohování**.

    ![Naplánování zálohování Windows Serveru](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. Na hello Začínáme stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.
4. Na stránce tooBackup hello vyberte položky, klikněte na **přidat položky**.
5. Vyberte hello soubory a složky, které chcete tooback nahoru a pak klikněte na tlačítko **nevadí**.
6. Klikněte na **Další**.
7. Na hello **zadání plánu zálohování** zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.

    Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.

    ![Položky k zálohování z Windows Serveru](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Další informace o tom, jak toospecify hello plán zálohování, najdete v článku hello [pomocí Azure Backup tooreplace infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. Na hello **výběr zásady uchovávání informací** stránky, vyberte hello **zásady uchovávání informací** pro záložní kopii hello.

    Hello zásady uchovávání informací Určuje dobu hello, pro které se uloží hello zálohování. Místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací podle toho, kdy dochází k zálohování hello. Můžete upravit hello uchování denní, týdenní, měsíční a roční zásady toomeet vašim potřebám.
9. Na stránce Výběr typu prvotní zálohy hello vyberte typ prvotní zálohy hello. Ponechte možnost hello **automaticky přes síť hello** vybrané a potom klikněte na **Další**.

    Můžete zálohovat automaticky přes síť hello nebo můžete zálohovat do offline režimu. Hello zbývající část tohoto článku popisuje proces hello zálohování automaticky. Pokud dáváte přednost toodo zálohu do offline režimu, přečtěte si článek hello [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md) Další informace.
10. Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.
11. Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.

### <a name="enable-network-throttling-optional"></a>Zapnout omezování sítě (volitelné)
agent zálohování Hello poskytuje, omezení šířky pásma sítě. Omezení využití šířky pásma sítě při přenosu dat ovládací prvky. Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz. Omezení platí tooback zálohu a obnovte aktivity.

**omezení tooenable sítě**

1. V agentovi hello zálohování, klikněte na tlačítko **změnit vlastnosti**.

    ![Změnit vlastnosti](./media/backup-configure-vault-classic/change-properties.png)
2. Na hello **omezování** karty, vyberte hello **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.

    ![Omezení šířky pásma sítě](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Po povolení omezení šířky pásma, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    hodnoty šířky pásma Hello začít ve 512 kilobitů za sekundu (Kbps) a můžete přejít do too1 023 megabajtů za sekundu (MBps). Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány pracovní dny. Hodiny mimo určené práce, kterou jsou považovány za hodiny mimo pracovní hodiny.
4. Klikněte na **OK**.

### <a name="tooback-up-now"></a>tooback si teď
1. V agentovi hello zálohování, klikněte na tlačítko **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.

    ![Zálohovat nyní ve Windows Serveru](./media/backup-configure-vault-classic/backup-now.png)
2. Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač. Poté klikněte na **Zálohovat**.
3. Klikněte na tlačítko **Zavřít** tooclose hello průvodce. Pokud to uděláte před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.

Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.

![Dokončení IR](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Další kroky
* Zaregistrujte si [bezplatný účet Azure](https://azure.microsoft.com/free/).

Další informace o zálohování virtuálních počítačů nebo dalších úloh najdete v tématu:

* [Zálohování virtuálních počítačů IaaS](backup-azure-vms-prepare.md)
* [Zálohování úloh tooAzure s Microsoft Azure Backup Server](backup-azure-microsoft-azure-backup.md)
* [Zálohování úloh tooAzure s aplikací DPM](backup-azure-dpm-introduction.md)
