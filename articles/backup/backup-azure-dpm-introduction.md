---
title: "aaaUse tooback DPM si portál tooAzure úlohy | Microsoft Docs"
description: "Toobacking Úvod do serverů DPM pomocí služby zálohování Azure hello"
services: backup
documentationcenter: 
author: adigan
manager: nkolli
editor: 
keywords: "System Center Data Protection Manager, nástroje data protection manager, zálohy aplikace dpm"
ms.assetid: c8c322cf-f5eb-422c-a34c-04a4801bfec7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: 1dd988ae55012ac7dc485d2416458542c60b6ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Příprava tooback až tooAzure úlohy s aplikací DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek obsahuje úvod toousing Microsoft Azure Backup tooprotect vaše servery System Center Data Protection Manager (DPM) a úlohy. Načtením ji budete porozumíte:

* Jak funguje Azure zálohování serveru
* Hello požadavky tooachieve smooth zálohování prostředí
* Hello typické došlo k chybám a jak toodeal s nimi
* Podporované scénáře

> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek obsahuje hello informace a postupy pro obnovení virtuální počítače nasazené pomocí modelu Resource Manager hello.
>
>

System Center DPM zálohuje data souborů a aplikací. Data zálohovaná tooDPM můžete uložit na pásce, na disku, nebo zálohovat tooAzure s Microsoft Azure Backup. Aplikace DPM komunikuje s Azure Backup následujícím způsobem:

* **Aplikace DPM nasazená jako fyzický server nebo místní virtuální počítač** – Pokud je aplikace DPM nasazená jako fyzický server nebo jako místní virtuální počítač technologie Hyper-V můžete zálohovat data tooa služeb zotavení trezoru kromě toodisk a zálohování na pásku.
* **Aplikace DPM nasazená jako virtuální počítač Azure** – ze System Center 2012 R2 s aktualizací 3, DPM dá nasadit jako virtuální počítač Azure. Pokud je aplikace DPM nasazená jako virtuální počítač Azure můžete zálohovat data tooAzure připojené disky virtuálnímu počítači DPM Azure toohello nebo může přenést úložiště dat hello zálohováním tooa, které trezor služeb zotavení.

## <a name="why-backup-from-dpm-tooazure"></a>Proč zálohovat z aplikace DPM tooAzure?
Příklady výhod Hello firmy pomocí služby Azure Backup k zálohování serverů aplikace DPM:

* Pro místní nasazení aplikace DPM můžete použít Azure jako tootape alternativní toolong termín nasazení.
* Pro nasazení aplikace DPM v Azure Azure Backup toooffload úložiště z disku Azure a hello vám umožní tooscale nahoru a to uložením starší data do trezoru služeb zotavení a nových dat na disku.

## <a name="prerequisites"></a>Požadavky
Příprava zálohování Azure tooback zálohovat data aplikace DPM následujícím způsobem:

1. **Vytvoření trezoru služeb zotavení** – vytvoření trezoru na portálu Azure.
2. **Stažení přihlašovacích údajů trezoru** – stáhnout hello přihlašovací údaje, které používají tooregister hello DPM serveru tooRecovery trezor služeb.
3. **Nainstalujte hello Azure Backup Agent** – z Azure Backup, instalaci agenta hello na každém serveru DPM.
4. **Zaregistrujte hello server** – registrace hello DPM serveru tooRecovery trezor služeb.

### <a name="1-create-a-recovery-services-vault"></a>1. Vytvoření trezoru služby Recovery Services
toocreate obnovení služby úložiště:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **Procházet** a v hello seznamu prostředků zadejte **služeb zotavení**. Během zadávání hello seznam bude filtrovat podle vašeho zadání. Klikněte na **Trezor Recovery Services**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Zobrazí se Hello seznamu trezorů služeb zotavení.
3. Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. Pro **název**, zadejte popisný název tooidentify hello trezoru. Název Hello musí toobe jedinečný pro hello předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Klikněte na tlačítko **předplatné** toosee hello seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné. Více možností bude dostupných pouze pokud je váš účet organizace přidružený k více předplatným Azure.
6. Klikněte na tlačítko **skupiny prostředků** toosee hello seznam dostupných skupin prostředků, nebo klikněte na **nový** toocreate novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello.
8. Klikněte na možnost **Vytvořit**. Může trvat nějakou dobu hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu.
   Po vytvoření svůj trezor otevře na portálu hello.

### <a name="set-storage-replication"></a>Nastavení replikace úložiště
možnost replikace úložiště Hello umožňuje toochoose mezi geograficky redundantním úložištěm a místně redundantního úložiště. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Ponechte hello možnost set toogeo redundantní úložiště, pokud se jedná o vaši primární zálohu. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá. Další informace o [geograficky redundantní](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantní](../storage/common/storage-redundancy.md#locally-redundant-storage) možnosti úložiště v hello [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).

nastavení replikace úložiště tooedit hello:

1. Vyberte řídicí panel trezoru tooopen hello trezoru a okna nastavení hello. Pokud hello **nastavení** neotevře, klikněte na tlačítko **všechna nastavení** na řídicím panelu trezoru hello.
2. Na hello **nastavení** okně klikněte na tlačítko **infrastruktura zálohování** > **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno. Na hello **konfigurace zálohování** okno, zvolte možnost replikace úložiště hello pro svůj trezor.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Po výběru možnosti hello úložiště pro svůj trezor, jsou připravené tooassociate hello virtuálních počítačů s úložištěm hello. přidružení hello toobegin, by měl zjistit a zaregistrujte hello virtuální počítače Azure.

### <a name="2-download-vault-credentials"></a>2. Stažení přihlašovacích údajů trezoru
Hello soubor s přihlašovacími údaji trezoru je certifikát vytvořený portál hello pro každý trezor záloh. Hello portál poté odešle veřejný klíč toohello hello služby Řízení přístupu (ACS). privátní klíč certifikátu hello Hello přišla uživatele k dispozici toohello jako součást pracovního postupu hello, která je zadána jako vstup v pracovním postupu registrace počítače hello. To se ověřuje trezoru tooan identifikovat hello počítač toosend zálohování dat v hello služba Azure Backup.

přihlašovací údaje úložiště Hello se používají pouze během pracovního postupu registrace hello. Je tooensure odpovědnost hello uživatele, který hello pověření pro úložiště, které nejsou ohrožená souboru. Jestliže spadá do nesprávných rukou hello žádné neautorizovaný uživatel, soubor s přihlašovacími údaji trezoru hello lze použít tooregister dalších počítačů hello stejnému trezoru. Jako hello zálohovaná data šifrovaná pomocí hesla, která patří toohello zákazníka, však nemůže být ohrožena stávajících zálohovaných dat.. toomitigate tuto situaci přihlašovací údaje úložiště jsou nastaveny tooexpire v 48hrs. Si můžete stáhnout hello přihlašovací údaje trezoru služeb zotavení libovolný počet časy – ale během hello pracovního postupu registrace lze použít pouze hello nejnovější soubor pověření pro úložiště.

soubor s přihlašovacími údaji trezoru Hello se stáhne prostřednictvím zabezpečeného kanálu z hello portálu Azure. Hello služby Azure Backup nezaznamená hello privátní klíč certifikátu hello, a v portálu hello nebo služby hello neuchovává jako trvalý hello privátní klíč. Pomocí následujících kroků toodownload hello úložiště přihlašovacích údajů souboru tooa místního počítače hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Otevřete služeb zotavení trezoru toowhich toowhich chcete tooregister počítač aplikace DPM.
3. Otevře se okno nastavení ve výchozím nastavení. Pokud je zavřený, klikněte na **nastavení** v okně Nastavení hello tooopen řídicího panelu trezoru. V okně nastavení klikněte na **vlastnosti**.

    ![Otevřené okno trezoru](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. Na stránce vlastností hello, klikněte na tlačítko **Stáhnout** pod **zálohování pověření**. portál Hello generuje soubor s přihlašovacími údaji trezoru hello, který je k dispozici ke stažení.

    ![Ke stažení](./media/backup-azure-dpm-introduction/vault-credentials.png)

Hello portálu vygeneruje přihlašovací údaje úložiště pomocí kombinace hello název trezoru a hello aktuální datum. Klikněte na tlačítko **Uložit** toodownload hello trezoru pověření toohello místního účtu stáhne složku nebo vyberte možnost Uložit jako z hello umístění pro přihlašovací údaje trezoru hello uložení toospecify nabídky. Bude to trvat až tooa minutu po dobu toobe souboru hello vygenerovat.

### <a name="note"></a>Poznámka
* Zkontrolujte, zda že tento soubor s přihlašovacími údaji hello je uložen v umístění, které jsou přístupné z počítače. Pokud je uložený v souboru sdílené složky nebo SMB, zkontrolujte hello přístupová oprávnění.
* soubor s přihlašovacími údaji trezoru Hello se používají pouze během pracovního postupu registrace hello.
* Hello soubor s přihlašovacími údaji trezoru vyprší po 48hrs a lze stáhnout z portálu hello.

### <a name="3-install-backup-agent"></a>3. Instalace agenta zálohování
Po vytvoření trezoru zálohování Azure hello, měla nainstalovat agenta na všechny vaše Windows počítače (Windows Server, klient systému Windows, server System Center Data Protection Manager nebo počítače serveru Azure Backup), které povoluje zálohování dat a aplikací tooAzure.

1. Otevřete služeb zotavení trezoru toowhich toowhich chcete tooregister počítač aplikace DPM.
2. Otevře se okno nastavení ve výchozím nastavení. Pokud je zavřený, klikněte na **nastavení** okno nastavení tooopen hello. V okně nastavení klikněte na **vlastnosti**.

    ![Otevřené okno trezoru](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Na stránce nastavení hello, klikněte na tlačítko **Stáhnout** pod **Azure Backup Agent**.

    ![Ke stažení](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Jakmile se stáhne hello agenta, poklikejte na MARSAgentInstaller.exe toolaunch hello instalace agenta Azure Backup hello. Vyberte instalační složku hello a pomocné složky, které jsou potřebné pro agenta hello. Zadané umístění mezipaměti Hello musí mít volné místo, který je nejméně 5 % hello zálohovaná data.
4. Pokud používáte proxy serveru tooconnect toohello Internetu, v hello **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o serveru proxy hello. Pokud používáte ověřený server proxy, zadejte podrobnosti uživatelského jména a hesla hello na této obrazovce.
5. agent Azure Backup Hello nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (Pokud není k dispozici již) toocomplete hello instalaci.
6. Po instalaci agenta hello **Zavřít** hello okno.

   ![Zavřít](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. příliš**hello registrace serveru DPM** toohello trezoru v hello **správy** kartě, klikněte na **Online**. Pak vyberte **zaregistrovat**. Otevře se hello zaregistrovat Průvodce instalací.
8. Pokud používáte proxy serveru tooconnect toohello Internetu, v hello **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o serveru proxy hello. Pokud používáte ověřený server proxy, zadejte podrobnosti uživatelského jména a hesla hello na této obrazovce.

    ![Konfigurace proxy serveru](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. V úvodní obrazovka přihlašovací údaje trezoru vyhledejte tooand vyberte hello trezoru pověření souboru, který byl dříve staženy.

    ![Přihlašovací údaje trezoru](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    soubor s přihlašovacími údaji trezoru Hello je platná pouze pro 48 hodin (po jeho stažení z portálu hello). Pokud narazíte na chyby na této obrazovce (například "Trezoru souboru vypršela platnost přihlašovací údaje"), přihlášení toohello Azure portal a stahování hello soubor s přihlašovacími údaji znovu.

    Ujistěte se, že soubor přihlašovacích údajů trezoru hello je k dispozici v umístění, která je přístupná hello instalační program aplikace. Pokud narazíte na přístup k související chyby, přihlašovací údaje trezoru hello kopírování souboru tooa dočasného umístění v tomto počítači a opakujte operaci hello.

    Pokud dojde k chybě neplatný trezoru přihlašovací údaje (například "Neplatný trezoru zadané údaje") hello soubor je buď poškozený nebo není mít hello nejnovější pověření spojená se službou obnovení hello. Opakujte operaci hello po stažení nový soubor s přihlašovacími údaji trezoru z portálu hello. Tato chyba je zpravidla se zobrazí, pokud hello uživatel klikne na hello **přihlašovací údaje trezoru Stáhnout** možnost v hello portál Azure, rychle po sobě. V takovém případě je platný pouze hello druhý soubor pověření pro úložiště.
10. toocontrol hello využití šířky pásma sítě během pracovní a nepracovní dobu v hello **nastavení omezení šířky pásma** obrazovky, můžete nastavit omezení využití šířky pásma hello a definovat hello pracovní a nepracovní dobu.

    ![Nastavení omezení](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. V hello **nastavení složky obnovení** obrazovky, procházet pro hello složku, kam hello soubory stáhli z Azure se dočasně připravený.

    ![Obnovení nastavení složky](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. V hello **nastavení šifrování** obrazovky, můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (minimálně 16 znaků). Mějte na paměti, toosave hello heslo v zabezpečeném umístění.

    ![Šifrování](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Pokud hello přístupové heslo ztratíte nebo zapomenete; Microsoft vám nemůže pomoci obnovit zálohovaná data hello. koncový uživatel Hello vlastní hello šifrovací přístupové heslo a Microsoft nemá přehled hello přístupové heslo používané hello koncového uživatele. Uložte soubor hello v zabezpečeném umístění, jako je vyžadována během operace obnovení.
    >
    >
13. Po kliknutí na tlačítko hello **zaregistrovat** tlačítko, hello počítač je registrovaný úspěšně toohello trezoru a můžete je teď připravený toostart zálohování tooMicrosoft Azure.
14. Pokud používáte Data Protection Manager, můžete upravit hello nastavení určené během pracovního postupu registrace hello kliknutím hello **konfigurace** možnost výběrem **Online** pod hello  **Správa** kartě.

## <a name="requirements-and-limitations"></a>Požadavky (a omezení)
* Aplikace DPM může být spuštěná jako fyzický server nebo virtuální počítač technologie Hyper-V nainstalovaná v System Center 2012 SP1 nebo System Center 2012 R2. Můžete také používat jako virtuální počítač Azure používá aspoň na System Center 2012 R2 s kumulativní aktualizace 3 pro DPM 2012 R2 nebo virtuálního počítače s Windows v prostředí VMWare alespoň systémem System Center 2012 R2 s kumulativní aktualizací 5.
* Pokud spouštíte aplikaci DPM s nástrojem System Center 2012 SP1 musíte nainstalovat aktualizaci vrátit up 2 pro System Center Data Protection Manager SP1. To je potřeba, před instalací hello Azure Backup Agent.
* Hello DPM server by měl mít prostředí Windows PowerShell a rozhraní .net Framework 4.5 nainstalované.
* Aplikace DPM můžete zálohovat tooAzure většinu úloh zálohování. Úplný seznam, co je podporováno najdete text hello Azure Backup podporují následující položky.
* Data uložená ve službě Azure Backup nelze obnovit pomocí možnosti "zkopírovat tootape" hello.
* Budete potřebovat účet Azure s povolenou funkcí zálohování Azure hello. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Přečtěte si informace o [cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/).
* Použití zálohování Azure vyžaduje toobe hello Azure Backup Agent nainstalovaný na hello servery, které chcete tooback nahoru. Každý server musí mít nejméně 5 % hello velikost hello data, která se zálohuje, k dispozici jako místní volný úložný prostor. Například zálohování 100 GB dat vyžaduje minimálně 5 GB volného místa v umístění pomocné hello.
* Data budou uložena v hello trezor služby Azure storage. Neexistuje žádné omezení toohello množství dat, které můžete zálohovat trezoru zálohování Azure tooan ale hello velikost zdroje dat (třeba virtuální počítač nebo databáze) by neměl být delší než 54400 GB.

Pro zálohování tooAzure se podporují tyto typy souborů:

* Šifrované (pouze úplné zálohy)
* Komprimované (je podporováno přírůstkové zálohování)
* Zhuštěné (je podporováno přírůstkové zálohování)
* Komprimované a zhuštěné (zpracovány jako zhuštěné)

A tyto nejsou podporovány:

* Servery v systémech souborů s rozlišením velkých nejsou podporovány.
* Pevné odkazy (vynecháno)
* Body rozboru (vynecháno)
* Zašifrované a komprimované (vynecháno)
* Šifrované a zhuštěné (vynecháno)
* Komprimovaný datový proud
* Zhuštěný datový proud

> [!NOTE]
> Z v System Center DPM 2012 s aktualizací SP1 a vyšší můžete zálohovat do zatížení chráněné aplikací DPM tooAzure pomocí služby Microsoft Azure Backup.
>
>
