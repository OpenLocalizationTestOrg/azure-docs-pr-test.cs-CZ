---
title: "aaaAzure Backup – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toocommon otázky týkající se: včetně služeb zotavení trezory, co ji můžete zálohovat, jak to funguje, šifrování a omezení funkce Azure Backup. "
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
ms.author: markgal;arunak;trinadhk;
ms.openlocfilehash: 3338f7620bcc6ebf53c9c161191f2d8bca1a29da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-service"></a>Dotazy týkající se hello služby zálohování Azure
Tento článek obsahuje odpovědi toocommon otázky toohelp rychle pochopit součásti hello Azure Backup. V některých hello odpovědi jsou články toohello odkazy, které mají komplexní informace. Kliknutím můžete klást otázky o Azure Backup **komentáře** (toohello vpravo). Komentáře se zobrazují dole hello v tomto článku. Účet Livefyre je požadovaná toocomment. Také můžete pokládat dotazy týkající se služby Azure Backup hello v hello [diskusní fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

tooquickly kontroly hello části v tomto článku použít hello odkazy toohello práva, v části **v tomto článku**.


## <a name="recovery-services-vault"></a>Trezor služby Recovery Services

### <a name="is-there-any-limit-on-hello-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Je k dispozici žádné omezení počtu hello trezory, které lze vytvořit v každé předplatné Azure? <br/>
Ano. Od září 2016 můžete vytvořit 25 trezorů služby Recovery Services nebo Backup na jedno předplatné. Můžete vytvořit až služeb zotavení too25 trezory za podporovanou oblast Azure Backup za předplatné. Pokud potřebujete další trezory, vytvořte další předplatné.

### <a name="are-there-limits-on-hello-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Existují omezení na hello počet serverů/počítačů, které lze registrovat k trezoru? <br/>
Ano, můžete zaregistrovat až too50 počítačů na trezor. Pro virtuální počítače Azure IaaS je hello limit 200 virtuálních počítačů na jeden trezor. Pokud potřebujete tooregister více počítačů, vytvořte jiný trezor.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Pokud má moje organizace jeden trezor, jak mohu během obnovování dat izolovat data jednoho serveru od jiného?<br/>
Všechny servery, které jsou registrované toohello stejnému trezoru mohou obnovit hello data zálohovaná jinými servery *využívající hello stejné heslo*. Pokud máte servery, jejichž zálohovaná data chcete tooisolate od ostatních serverů ve vaší organizaci, použijte pro tyto servery vyhrazené heslo. Například servery lidských zdrojů mohou používat jedno šifrovací heslo, účetní servery jiné a servery úložiště ještě jiné.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Mohu „migrovat“ data nebo trezor záloh mezi předplatnými? <br/>
Ne. Hello trezoru se vytvoří na úrovni předplatného a nemůže být přeřadit tooanother předplatné, jakmile je vytvořen.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Trezory služby Recovery Services jsou založené na Resource Manageru. Jsou trezory služby Backup (v klasickém režimu) stále podporovány? <br/>
Všechny existující trezory Backup v hello [portálu classic](https://manage.windowsazure.com) pokračovat toobe podporována. Ale můžete použít už hello portálu classic toodeploy nové trezory Backup. Společnost Microsoft doporučuje, protože budoucí vylepšení použít tooRecovery trezory služeb, pouze pomocí trezory služeb zotavení pro všechna nasazení. Když zkusíte toocreate portálu classic hello úložiště záloh, bude přesměrované toohello [portál Azure](https://portal.azure.com).

### <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Můžete migrovat úložiště záloh tooa trezoru služeb zotavení? <br/>
Bohužel Ne, nemůžete migrovat hello obsah tooa trezor zálohování, které trezor služeb zotavení. Na přidání této funkce pracujeme, zatím ale není dostupná.

### <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Svoje klasické virtuální počítače jsem zálohoval do trezoru služby Backup. Provést migraci virtuálních počítačů z klasického režimu tooResource Manager režimu a jejich ochraně v trezoru služeb zotavení?
Classic body obnovení virtuálního počítače v trezoru záloh není migrovat automaticky tooa trezor služeb zotavení, když přesouváte hello virtuálních počítačů z classic tooResource režimu správce. Postupujte podle těchto kroků tootransfer zálohování virtuálních počítačů:

1. V úložišti záloh hello, přejděte toohello **chráněné položky** a vyberte hello virtuálních počítačů. Klikněte na [Zastavit ochranu](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**.
2. Odstraňte rozšíření zálohování nebo snímek hello z hello virtuálních počítačů.
3. Migrujte hello virtuální počítač z režimu správce tooResource klasickém režimu. Ujistěte se, zda text hello úložiště a informace o síti, odpovídající toohello virtuální počítač je taky migrovat tooResource režimu správce.
4. Vytvoření trezoru služeb zotavení a konfigurace virtuálního počítače pomocí zálohování na hello migrovat **zálohování** akce nad panelu trezoru. Podrobné informace o zálohování virtuální počítač tooa obnovení služby trezoru, najdete v článku hello [chránit virtuální počítače Azure s trezoru služeb zotavení](backup-azure-vms-first-look-arm.md).

## <a name="azure-backup-agent"></a>Agent Azure Backup
Podrobný seznam dotazů je uveden v tématu [Nejčastější dotazy k zálohování souborů a složek v Azure](backup-azure-file-folder-backup-faq.md).

## <a name="azure-vm-backup"></a>Zálohování virtuálních počítačů Azure
Podrobný seznam dotazů je uveden v tématu [Nejčastější dotazy k zálohování virtuálních počítačů Azure](backup-azure-vm-backup-faq.md).

## <a name="back-up-vmware-servers"></a>Zálohování serverů VMware

### <a name="can-i-back-up-vmware-vcenter-servers-tooazure"></a>Můžete zálohovat tooAzure servery VMware vCenter?

Ano. Můžete použít Azure Backup Server tooback VMware vCenter a ESXi tooAzure. Informace o verzi VMware hello podporované, najdete v článku hello, [matice ochrany serveru Azure Backup](backup-mabs-protection-matrix.md). Podrobné pokyny najdete v tématu [tooback pomocí serveru Azure Backup server VMware](backup-azure-backup-server-vmware.md).


## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Server a System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-toocreate-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Mohu použít Azure Backup Server toocreate zálohy úplného obnovení (BMR) pro fyzický server? <br/>
Ano.

### <a name="can-i-register-my-dpm-server-toomultiple-vaults-br"></a>Můžete zaregistrovat Moje trezory toomultiple serveru aplikace DPM? <br/>
Ne. Aplikace DPM nebo MABS server může být registrovaný tooonly jeden trezor.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Jaká verze aplikace System Center Data Protection Manager je podporována? <br/>
Doporučujeme nainstalovat hello [nejnovější](http://aka.ms/azurebackup_agent) agenta Azure Backup na hello nejnovější kumulativní aktualizace (UR4) pro System Center Data Protection Manager (DPM). Od srpna 2016 11 kumulativní aktualizace je hello nejnovější aktualizace.

### <a name="i-have-installed-azure-backup-agent-tooprotect-my-files-and-folders-can-i-now-install-system-center-dpm-toowork-with-azure-backup-agent-tooprotect-on-premises-applicationvm-workloads-tooazure-br"></a>Azure Backup agent tooprotect nainstalována Moje soubory a složky. Lze nyní nainstalovat System Center DPM toowork s Azure Backup agent tooprotect místní aplikace a úloh virtuálního počítače tooAzure? <br/>
toouse Azure Backup se System Center Data Protection Manager (DPM), nejprve nainstalovat aplikaci DPM a pak nainstalujte agenta Azure Backup. Instalace komponent Azure Backup hello v tomto pořadí zajistí, že hello agenta Azure Backup pracuje s aplikací DPM. Instalaci agenta Azure Backup hello před instalací aplikace DPM není nedoporučuje nebo podporovány.


## <a name="how-azure-backup-works"></a>Jak funguje Azure Backup
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-hello-transferred-backup-data-deleted-br"></a>Pokud zruším úlohu zálohování, pokud již bylo spuštěno, je hello odstranění přenášených dat? <br/>
Ne. Všech dat přenesených do trezoru hello před hello úlohu zálohování byla zrušena, zůstane v trezoru hello. Azure Backup používá mechanismus toooccasionally kontrolní bod přidat kontrolní body toohello zálohovaná data během hello zálohování. Díky kontrolním bodům v zálohovaných dat hello, můžete hello dalším procesu zálohování ověřit integritu hello hello souborů. Další úlohy zálohování Hello bude přírůstkové toohello data dříve zálohovaná. Přírůstkové zálohování převod pouze nové nebo změněné data, která znamená zároveň toobetter využití šířky pásma.

Když ve virtuálním počítači Azure zrušíte úlohu zálohování, budou dosud přenesená data ignorována. Další úlohy zálohování Hello přenáší přírůstkových dat z hello poslední úspěšná úloha zálohování.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Existují omezení počtu a času naplánovaných úloh zálohování?<br/>
Ano. Úlohy zálohování můžete spustit na Windows serveru nebo pracovní stanice systému Windows až toothree časy za den. Úlohy zálohování na System Center DPM můžete spustit až tootwice denně. Úlohy zálohování pro virtuální počítače IaaS můžete spustit jednou za den. Můžete použít plánování zásad pro Windows Server nebo pracovní stanice toospecify Windows hello denní nebo Týdenní plány. Pomocí aplikace System Center DPM můžete zadat denní, týdenní, měsíční nebo roční plány.

### <a name="why-is-hello-size-of-hello-data-transferred-toohello-recovery-services-vault-smaller-than-hello-data-i-backed-upbr"></a>Proč je hello velikost dat přenesených toohello hello, které trezoru služeb zotavení s menší než hello data, která jsem zálohoval?<br/>
 Všechna data hello, které je zálohovat z Azure Backup Agent nebo SCDPM nebo serveru Azure Backup komprimovaná a šifrovaná před přenášením. Jakmile se použije hello komprese a šifrování, hello dat v úložišti záloh hello je 30-40 % menší.

## <a name="what-can-i-back-up"></a>Co můžu zálohovat
### <a name="which-operating-systems-do-azure-backup-support-br"></a>Které operační systémy podporuje služba Azure Backup? <br/>
Azure Backup podporuje následující seznam operačních systémů pro zálohování hello: soubory a složky a aplikace zatížení chráněné pomocí serveru Azure Backup a System Center Data Protection Manager (DPM).

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
| Windows Server 2008 SP2 |64bitová verze |Standard, Enterprise, Datacenter, Foundation |

**Pro zálohování virtuálního počítače Azure:**

* **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md), kromě základního OS Linux.  Další přineste-vaše – vlastní-Linuxových distribucích také může fungovat, dokud je k dispozici na virtuálním počítači hello hello agent virtuálního počítače a podpora pro Python existuje.
* **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.


### <a name="is-there-a-limit-on-hello-size-of-each-data-source-being-backed-up-br"></a>Existuje nějaké omezení velikosti hello každý zdroj dat zálohovaných? <br/>
Neexistuje žádné omezení na hello množství dat, která můžete zálohovat tooa trezoru. Zálohování Azure omezuje hello maximální velikost zdroje dat hello, ale tato omezení jsou velké. K srpnu 2015 je maximální velikost hello zdroje dat pro hello podporované operační systémy:

| Zdroj č. | Operační systém | Maximální velikost zdroje dat |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 nebo novější |54 400 GB |
| 2 |Windows 8 nebo novější |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

Hello následující tabulka vysvětluje, jakým způsobem je určován velikost jednotlivých zdrojů dat..

| Zdroj dat | Podrobnosti |
|:---:|:--- |
| Svazek |Hello množství dat zálohovaných z jednoho svazku serveru nebo klientského počítače |
| Virtuální počítač s technologií Hyper-V |Součet všechny hello virtuální pevné disky virtuálního počítače hello zálohovaných dat |
| Databáze Microsoft SQL Serveru |Velikost jedné zálohované databáze SQL. |
| Microsoft SharePoint |Součet databází obsahu a konfigurace hello v rámci zálohované farmy služby SharePoint |
| Microsoft Exchange |Součet všech databází systému Exchange na zálohovaném serveru Exchange. |
| BMR/Stav systému |Každá jednotlivá kopie BMR nebo stavu systém počítače hello zálohovaných |

Pro zálohování virtuálních počítačů Azure může mít každý virtuální počítač se too16 datových disků se každý datový disk se velikosti 1023GB nebo méně. 

## <a name="retention-policy-and-recovery-points"></a>Zásady uchovávání informací a body obnovení
### <a name="is-there-a-difference-between-hello-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Je nějaký rozdíl mezi hello zásady uchovávání informací pro DPM a Windows Server nebo klienta (který je na Windows Server bez DPM)?<br/>
Ne, DPM i Windows Server nebo klient mohou mít denní, týdenní, měsíční nebo roční zásady uchovávání.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Mohu konfigurovat zásady uchovávání informací selektivně – tj. nakonfigurovat týdenní a denní, ale ne roční a měsíční?<br/>
Ano, struktura uchovávání Azure Backup hello umožňuje toohave poskytuje úplnou flexibilitu při definování zásad uchovávání informací hello podle vašich požadavků.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Můžu naplánovat zálohování na 18:00 a pro zásady uchovávání informací zadat jiný čas?<br/>
Ne. Zásady uchovávání informací lze aplikovat pouze na body záloh. V hello následující obrázek zásady uchovávání informací hello je určený pro zálohy pořízené v 12: 00 a 18: 00. <br/>

![Plánování zálohování a uchovávání](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-toorecover-an-older-data-point-br"></a>Pokud se záloha uchovává po dlouhou dobu, trvá více času toorecover staršího datového bodu? <br/>
Ne – čas hello toorecover hello nejstarší nebo hello nejnovější bod je hello stejné. Každý bod obnovení se chová jako úplný bod.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-hello-total-billable-backup-storagebr"></a>Pokud každý bod obnovení chová jako úplný bod, ovlivní to hello celkové fakturovatelné úložiště zálohování?<br/>
Typické produkty s dlouhodobými body uchování ukládají zálohovaná data jako úplné body.  Hello úplné body jsou úložiště *neefektivní* , ale jsou snadnější a rychlejší toorestore. Přírůstkové kopie jsou úložiště *efektivní* ale vyžadují toorestore řetězu dat, což ovlivňuje dobu obnovení. Azure Backup úložiště architektura poskytuje hello nejlepší z obou světů optimální ukládání dat pro rychlé obnovení a nízké poplatky za úložiště. Tento přístup k ukládání dat zajišťuje efektivní využití příchozí i odchozí šířky pásma. Jak hello množství dat úložiště a hello času potřeba toorecover hello dat, je udržováno tooa minimální. Další informace o efektivitě [přírůstkového zálohování](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-hello-number-of-recovery-points-that-can-be-createdbr"></a>Existuje nějaké omezení na hello počet bodů obnovení, které lze vytvořit?<br/>
Můžete vytvořit až too9999 bodů obnovení za chráněné instance. Chráněné instance je počítač, server (fyzických nebo virtuálních) nebo úlohy, které jsou nakonfigurované tooback až data tooAzure. Další informace najdete v tématu Vysvětlení hello [zálohování a uchovávání](./backup-introduction-to-azure-backup.md#backup-and-retention), a [co je chráněný instance](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-recoveries-can-i-perform-on-hello-data-that-is-backed-up-tooazurebr"></a>Kolik obnovení můžete provést na hello dat, který je zálohovaný tooAzure?<br/>
Neexistuje žádné omezení počtu hello obnovení z Azure Backup.

### <a name="when-restoring-data-do-i-pay-for-hello-egress-traffic-from-azure-br"></a>Při obnovování dat, platím za hello odchozí provoz z Azure? <br/>
Ne. Vaše obnovení jsou zdarma a vám není účtován hello odchozí provoz.

## <a name="azure-backup-encryption"></a>Šifrování ve službě Azure Backup
### <a name="is-hello-data-sent-tooazure-encrypted-br"></a>Je hello data odesílají tooAzure zašifrovaná? <br/>
Ano. Data jsou zašifrována na hello místního serveru/klientu nebo počítači SCDPM pomocí AES256 a hello data se odesílají prostřednictvím zabezpečeného spojení HTTPS.

### <a name="is-hello-backup-data-on-azure-encrypted-as-wellbr"></a>Je v Azure šifrovaná i zálohovaná data hello?<br/>
Ano. Hello data odeslaná tooAzure zůstávají šifrovaná (v klidovém stavu). Microsoft nedešifruje zálohovaná data hello v libovolném bodě. Při zálohování virtuálního počítače Azure, Azure Backup se spoléhá na šifrování hello virtuálního počítače. Například pokud virtuální počítač se šifruje pomocí Azure Disk Encryption nebo nějaká jiná technologie šifrování, Azure Backup používá tento šifrování toosecure vaše data.

### <a name="what-is-hello-minimum-length-of-encryption-key-used-tooencrypt-backup-data-br"></a>Jaká je minimální délka šifrovacího klíče hello používá tooencrypt zálohovaných dat? <br/>
Hello šifrovací klíč musí být alespoň 16 znaků, pokud používáte Azure backup agent. Pro virtuální počítače Azure neexistuje žádné omezení toolength klíčů používaných Azure KeyVault. 

### <a name="what-happens-if-i-misplace-hello-encryption-key-can-i-recover-hello-data-or-can-microsoft-recover-hello-data-br"></a>Co se stane, když ztratím šifrovací klíč hello? Můžete obnovit hello data (nebo) může Microsoft obnovit hello data? <br/>
Hello klíče používané tooencrypt hello zálohování dat je přítomen pouze u zákazníka hello. Microsoft neudržuje jeho kopii v Azure a nemá žádné toohello přístupový klíč. Pokud zákazník hello ztratí klíč hello, Microsoft nemůže obnovit zálohovaná data hello.
