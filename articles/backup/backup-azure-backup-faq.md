---
title: "Nejčastější dotazy k Azure Backup | Dokumentace Microsoftu"
description: "Odpovědi na běžné dotazy týkající se funkcí služby Azure Backup, včetně trezorů služby Recovery Services, co může zálohovat, jak to funguje, šifrování a omezení. "
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování a zotavení po havárii; služba zálohování"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/21/2017
ms.author: markgal;arunak;trinadhk;sogup;
ms.openlocfilehash: 66c2f1c5e8ba26d5c50cf60b7f448406814408b0
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="questions-about-the-azure-backup-service"></a>Dotazy týkající se služby Azure Backup
Tento článek obsahuje odpovědi na časté otázky týkající se součástí Azure Backup. Některé odpovědi zahrnují odkazy na články obsahující komplexní informace. Můžete klást otázky týkající se Azure Backup kliknutím na **Komentáře** (napravo). Komentáře se zobrazují v dolní části tohoto článku. Pro komentáře je potřeba účet Livefyre. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

K rychlému procházení částmi tohoto článku použijte odkazy vpravo v části **V tomto článku**.


## <a name="recovery-services-vault"></a>Trezor služby Recovery Services

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Je v rámci předplatného Azure nějak omezený počet trezorů, které lze vytvořit? <br/>
Ano. Od září 2016 můžete vytvořit 25 trezorů služeb zotavení jedno předplatné. Můžete vytvořit až 25 trezorů služby Recovery Services pro každou podporovanou oblast služby Azure Backup na jedno předplatné. Pokud potřebujete další trezory, vytvořte další předplatné.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Je nějak omezený počet serverů nebo počítačů, které lze zaregistrovat k trezoru? <br/>
Ano, k jednomu trezoru můžete zaregistrovat až 50 počítačů. Pro virtuální počítače Azure IaaS je limit 200 virtuálních počítačů na jeden trezor. Pokud potřebujete zaregistrovat víc počítačů, vytvořte nový trezor.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Pokud má moje organizace jeden trezor, jak mohu během obnovování dat izolovat data jednoho serveru od jiného?<br/>
Všechny servery zaregistrované ke stejnému trezoru mohou obnovit data zálohovaná ostatními servery, *které používají stejné heslo* Máte-li servery, jejichž zálohovaná data chcete izolovat od ostatních serverů ve vaší organizaci, použijte pro tyto servery vyhrazené heslo. Například servery lidských zdrojů mohou používat jedno šifrovací heslo, účetní servery jiné a servery úložiště ještě jiné.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Mohu „migrovat“ data nebo trezor záloh mezi předplatnými? <br/>
Ne. Trezor je vytvořený na úrovni předplatného a po vytvoření ho nelze přiřadit k jinému předplatnému.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-still-supported-br"></a>Trezory služby Recovery Services jsou založené na Resource Manageru. Jsou trezory Backup stále podporovány? <br/>
Záloh byla převedena na trezory služeb zotavení. Pokud nebyly převedeny trezoru služby Backup do trezoru služeb zotavení, pak trezoru služby Backup byl převeden do trezoru služeb zotavení pro vás. 

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Můžu migrovat trezor Backup do trezoru Služeb zotavení? <br/>
Všechny trezory Backup byly převedeny na trezory služeb zotavení. Pokud nebyly převedeny trezoru služby Backup do trezoru služeb zotavení, pak trezoru služby Backup byl převeden do trezoru služeb zotavení pro vás.

## <a name="azure-backup-agent"></a>Agent Azure Backup
Podrobný seznam dotazů je uveden v tématu [Nejčastější dotazy k zálohování souborů a složek v Azure](backup-azure-file-folder-backup-faq.md).

## <a name="azure-vm-backup"></a>Zálohování virtuálních počítačů Azure
Podrobný seznam dotazů je uveden v tématu [Nejčastější dotazy k zálohování virtuálních počítačů Azure](backup-azure-vm-backup-faq.md).

## <a name="back-up-vmware-servers"></a>Zálohování serverů VMware

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Je možné zálohovat servery VMware vCenter do Azure?

Ano. Pomocí Azure Backup Serveru můžete do Azure zálohovat servery VMware vCenter a ESXi. Informace o podporovaných verzích VMware najdete v článku [Systém ochrany Azure Backup Serveru](backup-mabs-protection-matrix.md). Podrobné pokyny najdete v tématu [Použití Azure Backup Serveru k zálohování serveru VMware](backup-azure-backup-server-vmware.md).


## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Server a System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Mohu použít server Azure Backup k vytvoření zálohy úplného obnovení (BMR) pro fyzický server? <br/>
Ano.

### <a name="can-i-register-my-dpm-server-to-multiple-vaults-br"></a>Mohu zaregistrovat svůj server DPM pro více trezorů? <br/>
Ne. Server DPM nebo MABS lze zaregistrovat pouze pro jeden trezor.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Jaká verze aplikace System Center Data Protection Manager je podporována? <br/>
Doporučujeme vám nainstalovat [nejnovějšího](http://aka.ms/azurebackup_agent) agenta Azure Backup na nejnovější kumulativní aktualizaci aplikace System Center Data Protection Manager (DPM). Od srpna 2016 je nejnovější aktualizací Kumulativní aktualizace 11.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-system-center-dpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Nainstaloval jsem agenta Azure Backup, aby chránil moje soubory a složky. Můžu nyní do Azure nainstalovat aplikaci System Center DPM, aby spolupracovala s agentem Azure Backup při ochraně místních aplikací a úloh virtuálního počítače? <br/>
Když chcete používat Azure Backup se sadou System Center Data Protection Manager (DPM), nainstalujte nejprve DPM a až potom agenta Azure Backup. Instalace komponent služby Azure Backup v tomto pořadí zajistí, že agent Azure Backup bude fungovat s DPM. Instalovat agenta Azure Backup dříve než DPM se nedoporučuje ani nepodporuje.


## <a name="how-azure-backup-works"></a>Jak funguje Azure Backup
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Pokud zruším úlohu zálohování poté, co již byla spuštěná, dojde k odstranění přenášených dat? <br/>
Ne. Všechna data přenášená do trezoru před zrušením úlohy zálohování zůstanou v trezoru. Azure Backup používá mechanismus kontrolních bodů k příležitostnému přidávání kontrolních bodů do zálohovaných dat během zálohování. Díky kontrolním bodům v zálohovaných datech je možné při dalším procesu zálohování ověřit integritu souborů. Následující zálohování proběhne jako přírůstkové vzhledem k naposledy zálohovaným datům. Přírůstkové zálohování přenáší jen nová nebo změněná data, což znamená lepší využití přenosové kapacity.

Když ve virtuálním počítači Azure zrušíte úlohu zálohování, budou dosud přenesená data ignorována. Při následujícím přírůstkovém zálohování se přenesou data, která se změnila od poslední úspěšně dokončené úlohy zálohování.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Existují omezení počtu a času naplánovaných úloh zálohování?<br/>
Ano. Úlohy zálohování na pracovních stanicích s Windows Serverem nebo s Windows můžete spustit až třikrát za den. Úlohy zálohování na System Center DPM můžete spustit až dvakrát za den. Úlohy zálohování pro virtuální počítače IaaS můžete spustit jednou za den. Pomocí zásady plánování můžete pro pracovní stanice s Windows Serverem nebo s Windows zadat denní nebo týdenní plány. Pomocí aplikace System Center DPM můžete zadat denní, týdenní, měsíční nebo roční plány.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Proč je velikost dat přenášených do trezoru služby Recovery Services menší než velikost dat, která jsem zálohoval?<br/>
 Všechna data zálohovaná z agenta Azure Backup, SCDPM nebo serveru Azure Backup, projdou před odesláním kompresí a šifrováním. Po kompresi a šifrování se použije, data v trezoru služeb zotavení je 30-40 % menší.

## <a name="what-can-i-back-up"></a>Co můžu zálohovat
### <a name="which-operating-systems-do-azure-backup-support-br"></a>Které operační systémy podporuje služba Azure Backup? <br/>
Azure Backup podporuje pro zálohování souborů a složek a pro zálohování úloh aplikací chráněných pomocí Azure Backup Serveru a aplikace System Center Data Protection Manager (SCDPM) následující operační systémy.

| Operační systém | Platforma | Skladová jednotka (SKU) |
|:--- | --- |:--- |
| Windows 8 a nejnovější aktualizace Service Packu |64bitová verze |Enterprise, Pro |
| Windows 7 a nejnovější aktualizace Service Packu |64bitová verze |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 a nejnovější aktualizace Service Packu |64bitová verze |Enterprise, Pro |
| Windows 10 |64bitová verze |Enterprise, Pro, Home |
| Windows Server 2016 |64bitová verze |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Datacenter, Foundation |
| Windows Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup | 
| Windows Storage Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |
| Windows Storage Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |
| Windows Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Essential |
| Windows Server 2008 R2 SP1 |64bitová verze |Standard, Enterprise, Datacenter, Foundation |

**Pro zálohování virtuálního počítače Azure:**

* **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md), kromě základního OS Linux.  Další vlastní distribuce Linuxu mohou také fungovat, pokud je ve virtuálním počítači dostupný agent virtuálního počítače a pokud je podporovaný Python.
* **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Existuje nějaké omezení velikosti jednotlivých zdrojů zálohovaných dat? <br/>
Množství dat, která můžete do trezoru zálohovat, není nijak omezené. Azure Backup omezuje maximální velikost pro zdroj dat, ale tato omezení jsou velká. K srpnu 2015 je maximální velikost zdroje dat pro podporované operační systémy:

| Zdroj č. | Operační systém | Maximální velikost zdroje dat |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 nebo novější |54 400 GB |
| 2 |Windows 8 nebo novější |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

Následující tabulka vysvětluje, jak je určená velikost jednotlivých zdrojů dat.

| Zdroj dat | Podrobnosti |
|:---:|:--- |
| Svazek |Množství dat zálohovaných z jednoho svazku serveru nebo klientského počítače. |
| Virtuální počítač s technologií Hyper-V |Součet dat všech virtuálních pevných disků zálohovaného virtuálního počítače. |
| Databáze Microsoft SQL Serveru |Velikost jedné zálohované databáze SQL. |
| Microsoft SharePoint |Součet databází obsahu a konfigurace v rámci zálohované farmy služby SharePoint. |
| Microsoft Exchange |Součet všech databází systému Exchange na zálohovaném serveru Exchange. |
| BMR/Stav systému |Každá jednotlivá kopie BMR nebo stavu systému zálohovaného počítače. |

Pro zálohování virtuálních počítačů Azure může mít každý virtuální počítač až 16 datových disků, každý z nich do velikosti 1 023 GB. 

## <a name="retention-policy-and-recovery-points"></a>Zásady uchovávání informací a body obnovení
### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Je nějaký rozdíl mezi zásadami uchovávání informací pro DPM a Windows Server nebo klienta (tj. pro Windows Server bez DPM)?<br/>
Ne, DPM i Windows Server nebo klient mohou mít denní, týdenní, měsíční nebo roční zásady uchovávání.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Mohu konfigurovat zásady uchovávání informací selektivně – tj. nakonfigurovat týdenní a denní, ale ne roční a měsíční?<br/>
Ano, struktura uchovávání Azure Backup vám poskytuje úplnou flexibilitu při definování zásad uchovávání informací podle vašich požadavků.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Můžu naplánovat zálohování na 18:00 a pro zásady uchovávání informací zadat jiný čas?<br/>
Ne. Zásady uchovávání informací lze aplikovat pouze na body záloh. Následující obrázek ukazuje zadanou zásadu uchovávání informací pro zálohy pořízené v 00:00 a v 18:00. <br/>

![Plánování zálohování a uchovávání](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Pokud se záloha uchovává po dlouhou dobu, trvá pak obnovení staršího datového bodu déle? <br/>
Ne – obnovení nejstaršího i nejnovějšího bodu trvá stejně dlouho. Každý bod obnovení se chová jako úplný bod.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Jestliže se každý bod obnovení chová jako úplný bod, ovlivní to celkové fakturovatelné úložiště zálohování?<br/>
Typické produkty s dlouhodobými body uchování ukládají zálohovaná data jako úplné body.  Úplné body jsou *neefektivní* z hlediska úložiště, ale jejich obnovení je snadnější a rychlejší. Přírůstkové kopie jsou *efektivní* z hlediska úložiště, ale vyžadují obnovení řetězu dat, což ovlivňuje dobu obnovení. Architektura úložiště Azure Backup nabízí to nejlepší z obou světů – optimální ukládání dat pro rychlé obnovení a nízké poplatky za úložiště. Tento přístup k ukládání dat zajišťuje efektivní využití příchozí i odchozí šířky pásma. Velikost úložiště dat i čas potřebný k obnovení dat jsou omezeny na minimum. Další informace o efektivitě [přírůstkového zálohování](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Je nějak omezený počet bodů obnovení, které lze vytvořit?<br/>
Na jednu chráněnou instanci je možné vytvořit až 9 999 bodů obnovení. Chráněná instance je počítač, server (fyzický nebo virtuální) nebo úloha nakonfigurovaná pro zálohování dat do Azure. Další informace najdete vysvětlené v částech [Zálohování a uchovávání](./backup-introduction-to-azure-backup.md#backup-and-retention) a [Co je chráněná instance?](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance).

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Kolikrát mohu provést obnovení dat zálohovaných v Azure?<br/>
Počet obnovení z Azure Backup není omezený.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Platí se za výchozí přenos z Azure při obnovování dat? <br/>
Ne. Vaše obnovení jsou zdarma a výchozí přenos vám není účtován.

## <a name="azure-backup-encryption"></a>Šifrování ve službě Azure Backup
### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Jsou data odesílaná do Azure šifrovaná? <br/>
Ano. Data jsou zašifrována na místním serveru, klientu nebo počítači SCDPM pomocí AES256 a odesílaná jsou přes zabezpečené spojení HTTPS.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Jsou šifrovaná i zálohovaná data v Azure?<br/>
Ano. Data odeslaná do Azure zůstávají šifrovaná (neaktivní uložená data). Microsoft nikdy nedešifruje zálohovaná data. Při zálohování virtuálního počítače Azure spoléhá služba Azure Backup na šifrování virtuálního počítače. Například pokud je váš virtuální počítač šifrovaný pomocí služby Azure Disk Encryption nebo jiné technologie šifrování, Azure Backup k zabezpečení dat použije toto šifrování.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Jaká je minimální délka šifrovacího klíče, který se používá k šifrování zálohovaných dat? <br/>
Pokud používáte agenta Azure Backup, šifrovací klíč by měl obsahovat alespoň 16 znaků. Pro virtuální počítače Azure neplatí žádné omezení délky klíčů, které používá služba Azure Key Vault. 

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Co se stane, když ztratím šifrovací klíč? Mohu obnovit data (nebo) může Microsoft obnovit data? <br/>
Klíč, pomocí kterého se šifrují zálohovaná data, je přítomen pouze u zákazníka. Microsoft neudržuje jeho kopii v Azure a nemá ke klíči žádný přístup. Pokud zákazník klíč ztratí, Microsoft nemůže zálohovaná data obnovit.
