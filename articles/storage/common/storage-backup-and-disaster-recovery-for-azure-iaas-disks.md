---
title: "aaaBackup a zotavení po havárii pro disky Azure IaaS | Microsoft Docs"
description: "V tomto článku vysvětlíme, jak tooplan pro zálohování a obnovení po havárii (DR) IaaS virtuálních počítačů (VM) a disky v Azure. Tento dokument popisuje spravované i nespravované disky"
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: luywang
ms.openlocfilehash: 49b0e7732d6df9407e1e44d9af2500c99a85b37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Zálohování a zotavení po havárii pro disky Azure IaaS

V tomto článku vysvětlíme, jak tooplan pro zálohování a obnovení po havárii (DR) IaaS virtuálních počítačů (VM) a disky v Azure. Tento dokument popisuje spravované i nespravované disky.

Nejprve budeme mluvit o hello předdefinované odolnost proti chybám možnosti v hello platformy Azure, které pomáhají chránit proti místní selhání. Potom probereme scénáře po havárii hello předmětem není plně hello integrované funkce, která je hlavní téma hello řešený v tomto dokumentu. Ukážeme vám také několik příkladů zatížení scénáře, kde může použít různé aspekty zálohování a zotavení po Havárii. Potom: přečtěte možná řešení pro zotavení po Havárii disků IaaS. 

## <a name="introduction"></a>Úvod

Hello platformy Azure používá různé metody pro redundanci a odolnost proti chybám toohelp chránit zákazníkům selhání lokalizované hardwaru, které se můžou vyskytnout. Místní selhání mohou obsahovat problémy s počítačem serveru úložiště Azure, která ukládá část dat hello pro virtuální disk nebo selhání SSD nebo pevný disk na tomto serveru. Takové selhání součásti izolované hardwaru může dojít během normálních operací a platforma hello je navrženou toobe odolné toothese selhání. Hlavní havárie může způsobit selhání nebo inaccessibility velkého počtu serverů úložiště nebo celého datového centra. Při z lokálních selhání jsou obvykle chráněné virtuální počítače a disky, další kroky jsou nezbytné tooprotect úlohu z celou oblast závažné chyby (například významnější havárie) ovlivňující virtuálních počítačů a disků.

Kromě toho toohello možnost selhání platformy, hello zákaznické aplikaci nebo dat může dojít k problémům. Nová verze aplikace může být například nechtěně narušující, toohello data změny. V takovém případě můžete aplikace hello toorevert a hello data tooa předchozí verze obsahující hello posledního známého stavu. To vyžaduje údržbu pravidelných záloh.

Místní zotavení po havárii musí zálohování virtuálních počítačů IaaS jiné oblasti disky tooa vaše. 

Předtím, než se podíváme na možnosti zálohování a zotavení po Havárii, můžeme recap několik metod k dispozici pro zpracování lokálních selhání.

### <a name="azure-iaas-resiliency"></a>Odolnost proti chybám Azure IaaS

*Odolnost proti chybám* odkazuje toohello tolerance pro běžné chyby, ke kterým došlo v hardwarové součásti. Odolnost proti chybám hello možnost toorecover selhání a pokračovat toofunction. Nejedná se o zamezení selhání, ale neodpovídá toofailures způsobem, který zabraňuje výpadku nebo ztráty dat. cílem Hello odolnosti je tooreturn hello aplikace tooa plně funkční stavu selhání. Azure virtuální počítače a disky jsou navržené toobe odolné toocommon hardwaru chyb. Dejte nám podívejte, jak Platforma Azure IaaS hello poskytuje tuto odolnost proti chybám.

Virtuální počítač se skládá ze dvou částí především: (1) A výpočetní server a (2) hello trvalé disky. Obě ovlivnit hello odolnost proti chybám virtuálního počítače.

Pokud hello výpočtů Azure hostitelský server, kde virtuální počítač dojde k selhání hardwaru, (což je taková situace vzácná), Azure je navrženou tooautomatically obnovení hello virtuálních počítačů na jiný server. V takovém případě si všimnete restart a hello virtuálních počítačů bude zálohovat po určité době. Azure automaticky zjišťuje takové selhání hardwaru a provede obnovení toohelp ujistěte hello zákazníka virtuálních počítačů bude k dispozici co nejdříve.

Týkající se disků IaaS odolnost dat je důležité pro platformu hello trvalého úložiště. Zákazníci, Azure mají důležitých podnikových aplikací běžících na IaaS a jsou závislé na hello trvalost dat hello. Ochrana Azure návrhy pro tyto disky IaaS s tři redundantní kopie dat uložených místně, poskytuje vysokou odolnost proti selhání místní. Pokud jeden z hello hardwarové součásti, které obsahuje váš disk selže, virtuální počítač není ovlivněná protože nejsou k dispozici dva další kopie toosupport disku požadavky. Funguje bez problémů, i v případě selžou dva různé hardwarové komponenty podpora disk v hello stejné (což by bylo velmi výjimečných). toohelp zkontrolujte uchováváme vždy tři repliky, služby Azure Storage hello automaticky vytvoří novou kopii dat na pozadí hello, pokud některý z tři kopie hello nedostupný. Proto by neměl být nezbytné toouse RAID s Azure disky pro odolnost proti chybám. Jednoduché RAID 0 konfigurace by mělo být dostatečné pro prokládání disky hello, pokud nezbytné toocreate větší svazky.

Z důvodu této architektuře **Azure má doručit konzistentně podnikové úrovni odolnost pro IaaS disky s špičkový NULOVÉ % [rozpočítat na roční období míra selhání](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Lokalizované hardwaru chyb na hello výpočetní hostitele nebo v úložišti hello platformy můžete někdy za následek dočasné nedostupnosti pro hello virtuálních počítačů, které se vztahuje hello [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) dostupnosti virtuálních počítačů. Azure poskytuje špičkový SLA pro jedné instance virtuálních počítačů, které použijte disky úložiště Premium.

toosafeguard úlohy aplikací z výpadek kvůli dočasné nedostupnosti toohello na disk nebo virtuální počítač, můžete využít zákazníci [skupiny dostupnosti](../../virtual-machines/windows/manage-availability.md). Dvě nebo více virtuálních počítačů v nastavení dostupnosti poskytuje redundanci pro aplikaci hello. Azure potom vytvoří tyto virtuální počítače a disky v doménách selhání samostatné s různé součásti napájení, sítě a serveru. Proto selhání lokalizované hardwaru obvykle neovlivní více virtuálních počítačů v hello nastavit na hello stejnou dobu, tím vysokou dostupnost pro vaši aplikaci. Předpokládá se, že s dostupností toouse vhodné nastavuje, když se vyžaduje vysoká dostupnost. Další informace najdete v tématu aspekty hello zotavení po havárii, jsou popsané dál.

### <a name="backup-and-disaster-recovery"></a>Zálohování a zotavení po havárii

Zotavení po havárii (DR) je možnost toorecover hello z výjimečných ale hlavní incidenty:-pouze dočasné, celou škálování selhání, jako je například přerušení služby, které ovlivňuje celou oblast. Zotavení po havárii zahrnuje zálohování dat a archivaci a může zahrnovat ruční zásah, například obnovit databázi ze zálohy.

Hello platformy Azure integrovanou ochranu proti selhání lokalizované nemusí chránit plně hello virtuálních počítačů/disků v případě hello hlavní katastrofy, které může způsobit výpadky ve velkém měřítku. To zahrnuje závažné události, jako je například datové centrum, se setkají hurikán, zemětřesení, ještě efektivněji nebo selhání jednotky hardwaru velkém měřítku. Kromě toho může dojít k selhání kvůli potížím s tooapplication nebo data.

toohelp chránit vaše úlohy IaaS z výpadky, měli byste naplánovat obnovu tooenable redundance a zálohování. Pro zotavení po havárii měli byste naplánovat redundance a zálohování v jiném geografické umístění mimo hello primární lokality. To pomáhá zajistit zálohování nemá vliv hello stejnou událost, která původně dopad hello virtuálního počítače nebo disků. Další informace najdete v tématu [zotavení po havárii pro aplikace vytvořené v Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

Důležité informace o vaší zotavení po Havárii může zahrnovat hello následující aspekty:

1. Vysoké dostupnosti (HA) je schopnost hello toocontinue hello aplikace spuštěna v dobrém stavu, bez významné výpadky. Podle "stavu v pořádku," jsme znamená hello aplikace reaguje, a uživatelé můžou připojit toohello aplikace a pracovat s ním. Některé důležité aplikace a databáze může být k dispozici požadované toobe vždy, i v případě, že tam jsou chyby v platformě hello. Pro tyto úlohy pravděpodobně bude třeba tooplan redundance pro hello aplikace a také hello data.

2. Odolnost data: V některých případech hlavní pozornost hello je zajistit, že uchování dat hello hello v případě havárie. Proto může být nutné zálohování dat v jiné lokalitě. Pro taková zatížení možná nebudete potřebovat úplné redundance pro hello aplikaci, ale pravidelné zálohy disků hello.

## <a name="backup-and-dr-scenarios"></a>Zálohování a zotavení po Havárii scénáře

Podívejme se na několik typické příklady hello aspekty pro plánování zotavení po havárii a scénáře zatížení aplikací.

### <a name="scenario-1-major-database-solutions"></a>Scénář 1: Hlavní databáze řešení

Vezměte v úvahu produkční databázový server jako SQL Server nebo Oracle, který podporuje vysokou dostupnost. Kritický produkční aplikace a uživatelé závisí na tuto databázi. Hello plánu zotavení po havárii pro tento systém může být nutné toosupport hello následující požadavky:

1. Data musí být chráněný a obnovitelný.
2.  Server musí být k dispozici pro použití.

Může to vyžadovat zachování repliky databáze hello v jiné oblasti jako záložní. Hello řešení může v závislosti na požadavcích hello pro dostupnost serveru a obnovení dat, v rozsahu od aktivní-aktivní nebo aktivní – pasivní repliky lokality tooperiodic offline zálohování dat hello. Relační databáze jako je například SQL Server a Oracle poskytují různé možnosti pro replikaci. Pro systém SQL Server [SQL serveru skupin dostupnosti Always On](https://msdn.microsoft.com/library/hh510230.aspx) lze použít pro vysokou dostupnost.

Databáze NoSQL, jako MongoDB také podporují [repliky](https://docs.mongodb.com/manual/replication/) redundanci. Hello replik pro zajištění vysoké dostupnosti lze použít.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scénář 2: Cluster redundantní virtuální počítače

Vezměte v úvahu zatížení zpracovává cluster virtuálních počítačů, které poskytují redundanci a vyrovnávání zatížení. Příkladem může být cluster Cassandra nasazené v oblasti. Tento typ architektury již poskytuje vysokou úroveň redundance v rámci této oblasti. Ale tooprotect hello zatížení v případě selhání místní úrovni, měli byste zvážit šíření hello clusteru rámci dvou oblastí nebo provádění pravidelné zálohy tooanother oblast.

### <a name="scenario-3-iaas-application-workload"></a>Scénář 3: IaaS aplikace zatížení

To může být typické provozní úlohu spuštěnou na virtuální počítač Azure. Například může být na webovém serveru nebo souborového serveru, která uchovává hello obsah a dalším prostředkům lokality. Mohou být také uživatelské obchodní aplikace běžící v virtuální počítač, který uložené prostředky, stav aplikace a jeho data na disky virtuálních počítačů hello. V takovém případě je důležité toomake se, že provedete zálohy v pravidelných intervalech. Četnost zálohování by měla být založena na hello povaha hello zatížení virtuálních počítačů. Například pokud aplikace hello spouští každý den a mění data, pak hello zálohování třeba přijmout každou hodinu.

Dalším příkladem je server pro sestavy této načítat data z jiných zdrojů a generuje souhrnné sestavy. Ztrátě tohoto virtuálního počítače nebo disky povede ke ztrátě toohello hello sestav. Však může být možné toorerun hello reporting proces a opětovné vytváření hello výstup. V takovém případě nemáte skutečně ztrátu dat i v případě, že server pro sestavy hello je dosáhl s po havárii, tak může mít vyšší úroveň tolerance pro ztráty součástí hello dat na server pro sestavy hello. V takovém případě méně časté zálohování je nákladů na možnost tooreduce hello.

### <a name="scenario-4-iaas-application-data-issues"></a>Scénář 4: Aplikace IaaS dat problémy

Máte aplikaci, která vypočítá, udržuje a slouží důležitá obchodní data, například informace o cenách. Nová verze aplikace měla chyb softwaru, který nesprávně počítaný hello ceny a poškozený stávající data a obchodu Spojených států hello obsloužených hello platformy. Zde hello nejlepším řešením může být toorevert toohello dřívější verzi hello aplikace a hello data první. tooenable se proveďte pravidelné zálohy systému.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Řešení zotavení po havárii: Služba Azure Backup

[Služba Azure Backup](https://azure.microsoft.com/services/backup/) je lze použít pro zálohování a zotavení po Havárii a funguje s [spravované disky](../../virtual-machines/windows/managed-disks-overview.md) a také [nespravované disky](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). Můžete vytvořit úlohu zálohování s zálohy založené na čase, snadno obnovení virtuálních počítačů a zásady uchovávání záloh. 

Pokud používáte [disky úložiště Premium](storage-premium-storage.md), [spravované disky](../../virtual-machines/windows/managed-disks-overview.md), nebo jiných typů disku s hello [místně redundantní úložiště (LRS)](storage-redundancy.md#locally-redundant-storage) možnost, je zvlášť důležité tooleverage pravidelné zálohy zotavení po Havárii. Zálohování Azure ukládá hello data ve vašem trezoru služeb zotavení pro dlouhodobé uchovávání. Zvolte hello [geograficky redundantní úložiště (GRS)](storage-redundancy.md#geo-redundant-storage) možnost pro trezor služeb zotavení hello zálohování. Se ujistěte se, že zálohy jsou replikované tooa jiné oblasti Azure pro zabezpečení z místní havárie.

Pro [nespravované disky](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), můžete použít typ úložiště LRS hello disků IaaS, ale ujistěte se, že Azure Backup je povolená s hello GRS možnost pro hello trezoru služeb zotavení.

**Pokud používáte hello [GRS](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) možnost pro nespravovaná disky, je stále nutné snímky konzistentní s aplikacemi pro zálohování a zotavení po Havárii.** Je nutné použít buď [služby zálohování Azure](https://azure.microsoft.com/services/backup/) nebo [snímky konzistentní](#alternative-solution-consistent-snapshots).

Hello Následuje souhrn řešení pro zotavení po Havárii.

| Scénář | Automatické replikace | Řešení zotavení po Havárii |
| --- | --- | --- |
| *Disky úložiště Premium* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nespravované LRS disky* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nespravované GRS disky* | Pro různé oblasti ([GRS](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Snímky konzistentní s aplikacemi](#alternative-solution-consistent-snapshots) |
| *Nespravované RA-GRS disky* | Pro různé oblasti ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Snímky konzistentní s aplikacemi](#alternative-solution-consistent-snapshots) |

Zajištění vysoké dostupnosti můžete nejlépe pomocí splní prostřednictvím použití hello spravované disků ve skupiny dostupnosti společně s Azure Backup. Pokud používáte nespravované disky, stále můžete zálohování Azure pro zotavení po Havárii. Pokud jste nelze toouse Azure Backup, pak trvá [snímky konzistentní](#alternative-solution-consistent-snapshots) jak je popsáno v další části je alternativní řešení pro zálohování a zotavení po Havárii.

Vaše volby pro vysokou dostupnost, zálohování a zotavení po Havárii aplikace nebo infrastruktury úrovni může být reprezentován jako níže:

| *Úroveň* | Vysoká dostupnost   | Zálohování nebo zotavení po Havárii |
| --- | --- | --- |
| *Aplikace* | Always On SQL | Azure Backup |
| *Infrastruktury*  | Skupina dostupnosti  | GRS s snímky konzistentní s aplikacemi |

### <a name="using-hello-azure-backup-service"></a>Pomocí hello služby zálohování Azure

[Zálohování Azure](../../backup/backup-azure-vms-introduction.md) může zálohovat virtuální počítače s Windows nebo Linux toohello trezoru služeb zotavení Azure. Zálohování a obnovení důležitých podnikových dat ztěžuje hello skutečnost, že důležitých podnikových dat je třeba zálohovat při hello aplikace, které vytvářejí hello dat běží. tooaddress, Azure Backup poskytuje zálohování konzistentní s aplikací pro Microsoft úlohy pomocí tooensure hello služby Stínová svazku (VSS), že data jsou zapsána správně toostorage. Pro virtuální počítače s Linuxem je možné, pouze konzistentními soubory zálohy, protože Linux nemá ekvivalentní tooVSS funkce.

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png)   

Když spustí úlohu zálohování Azure Backup v hello naplánované době aktivuje rozšíření zálohování hello nainstalované v hello tootake virtuálních počítačů v okamžiku snímku. Pořízení snímku v koordinaci s VSS tooget konzistentního snímku hello disků ve virtuálním počítači hello bez nutnosti tooshut ho dolů. Rozšíření zálohování Hello v hello virtuálních počítačů vyprázdnění všech zápisů před přepnutím snímky konzistentní s hello všech disků hello. Po pořízení snímku hello, hello data se přenáší přes Azure Backup toohello úložiště záloh. toomake hello proces zálohování efektivnější, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od poslední zálohy hello.


toorestore, můžete zobrazit hello k dispozici zálohování pomocí zálohování Azure a pak spustit obnovení. Můžete vytvořit a obnovit zálohování Azure prostřednictvím hello [portál Azure](https://portal.azure.com/), [pomocí prostředí PowerShell](../../backup/backup-azure-vms-automation.md ), nebo pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/xplat-cli-install). Další informace najdete v tématu Zálohování Azure.

### <a name="steps-tooenable-backup"></a>Kroky tooEnable zálohování

Hello následující kroky jsou obvykle používanými tooenable zálohování virtuálních počítačů pomocí hello [portálu Azure](https://portal.azure.com/). Bude existovat několik odchylek v závislosti na přesný scénář. Odkazovat toohello [Azure Backup](../../backup/backup-azure-vms-introduction.md) dokumentace pro úplné podrobnosti. Azure také zálohování [podporuje virtuální počítače s spravované disky](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Vytvoření trezoru služeb zotavení pro virtuální počítač pomocí hello následující kroky:

    a.  Pomocí hello [portál Azure](https://portal.azure.com/), procházet všechny prostředky a vyhledávat "Trezory služeb zotavení".

    b.  Na hello trezory služeb zotavení nabídky, klikněte na tlačítko Přidat a postupujte podle kroků toocreate hello nový trezor v hello stejné oblasti jako hello virtuálních počítačů. Například pokud vaše virtuální počítač v oblasti západní USA, vyberte pro trezor hello západní USA.

2.  Ověřte hello úložiště replikace pro nově vytvořený trezor hello. toodo se vault hello přístup z hello služeb zotavení trezory okno a přejděte toohello nastavení nebo zálohu konfigurace. Zkontrolujte, zda hello GRS možnost je standardně vybraná. Tím bude zajištěno, že je váš trezor automaticky replikované tooa sekundárního datového centra. Například trezoru v západní USA bude automaticky replikované tooEast USA.

3.  Nakonfigurujte zásady zálohování hello a vyberte hello virtuálních počítačů z hello stejné uživatelské rozhraní.

4.  Ujistěte se, zda text hello, instalace agenta zálohování na hello virtuálních počítačů. Pokud váš virtuální počítač je vytvořen pomocí image Galerie Azure, hello Backup agent již nainstalován. Jinak (tj. Pokud používáte vlastní image), použijte pokyny příliš[hello instalace agenta virtuálního počítače na virtuálním počítači hello](../../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Ujistěte se, že hello virtuálního počítače umožňuje připojení k síti pro služby zálohování toofunction hello. Postupujte podle těchto pokynů pro [připojení k síti](../../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  Po dokončení hello výše kroky jsou hello zálohování se spustí v pravidelných intervalech podle zásady zálohování hello. V případě potřeby můžete aktivovat hello první zálohy ručně z panelu trezoru hello na hello portálu Azure.

Pro automatizaci Azure Backup pomocí skriptů, naleznete v příliš[rutiny prostředí PowerShell pro zálohování virtuálních počítačů](../../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Kroky pro obnovení

Pokud potřebujete toorepair nebo znovu sestavte virtuální počítač, můžete obnovit hello virtuálních počítačů z libovolného hello body obnovení záloh v trezoru hello. Existuje několik různých možností pro obnovení hello:

1.  Můžete vytvořit nový virtuální počítač jako reprezentace v okamžiku zálohy virtuálního počítače.

2.  Můžete obnovit hello disky a potom hello šablonu použít pro virtuální počítač toocustomize hello a opětovné sestavení hello obnovit virtuální počítač. 

Zobrazí pokyny k příliš[virtuální počítače Azure pomocí portálu toorestore](../../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). Hello dokumentu se vysvětluje také hello konkrétní kroky pro obnovení zálohy virtuálních počítačů toohello spárované datového centra pomocí vaší geograficky redundantní úložiště záloh v případě hello nebo přírodní katastrofě v hello primární datové centrum. V takovém případě Azure Backup používá hello výpočetní služba z hello sekundární oblasti toocreate hello obnovení virtuálního počítače.

Můžete také pomocí prostředí PowerShell pro [obnovení virtuálního počítače](../../backup/backup-azure-vms-automation.md#restore-an-azure-vm) nebo [vytváření nového virtuálního počítače z obnovit disky](../../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternativní řešení: Snímky konzistentní

Pokud jste nelze toouse hello služby zálohování Azure, můžete implementovat vlastní zálohovací mechanismus, pomocí snímků. Je složitá toocreate snímky konzistentní s aplikacemi pro všechny disky používané Virtuálním počítačem a pak replikovat tyto snímky tooanother oblast. Z tohoto důvodu Azure zvažuje využívání hello služby zálohování vhodnější než vytváření vlastní řešení. Pokud používáte RA-GRS nebo GRS úložiště pro disky, snímky jsou automaticky replikované tooa sekundárního datového centra. Pokud používáte LRS úložiště pro disky, musíte se tooreplicate hello data sami. Další informace najdete v tématu [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md).

Snímek je reprezentace objektu v určitém bodě v čase. Snímek zpoplatněná fakturace pro hello přírůstkové velikost dat hello ho blokování. Další informace najdete v tématu [vytvoření snímku blob](../blobs/storage-blob-snapshots.md).

### <a name="creating-snapshots-while-hello-vm-is-running"></a>Vytváření snímků při hello je spuštěný virtuální počítač

Při snímek kdykoli, můžete provést v případě hello je spuštěný virtuální počítač, je stále data vysílá toohello disky a snímky hello může obsahovat částečné operace, které byly v cestě. Navíc pokud existují několik disků související se situací, hello snímky různých disků mohlo dojít v různou dobu, což znamená, že tyto snímky nemusí být koordinované. To je obzvláště problematické pro prokládané svazky, jejichž souborů může být poškozená, pokud se během zálohování provedeny změny.

tooavoid této situaci se proces zálohování hello musí implementovat hello následující kroky:

1.  Freeze – všechny disky hello

2.  Vyprázdnit všechny hello čekající na vyřízení zápisy

3.  Potom [vytvoření snímku blob](../blobs/storage-blob-snapshots.md) pro všechny disky hello

Některé aplikace systému Windows, jako je SQL Server poskytují mechanismus koordinované zálohování pomocí zálohování konzistentní s aplikací toocreate služby Stínová kopie svazku. V systému Linux můžete použít nástroje, jako je fsfreeze pro spolupráci hello disky, což poskytne konzistentními soubory zálohy, ale není snímky konzistentní s aplikacemi. Tento proces je složitá, takže a měli byste zvážit použití [Azure Backup](../../backup/backup-azure-vms-introduction.md) nebo řešení zálohování jiného výrobce, který už tuto funkci implementovat.

Hello výše proces bude v kolekci koordinované snímků pro všechny disky virtuálního počítače hello představující konkrétní zobrazení v okamžiku hello virtuálních počítačů. Toto je bod obnovení zálohy pro hello virtuálních počítačů. Proces hello v naplánovaných intervalech toocreate pravidelné zálohy, můžete opakovat. Níže najdete kroky hello příliš[kopie hello zálohy tooanother oblast](#copy-the-snapshots-to-another-region) pro zotavení po Havárii.

### <a name="creating-snapshots-while-hello-vm-is-offline"></a>Vytváření snímků při hello virtuální počítač je v režimu offline

Jiná možnost toocreate konzistentní zálohování probíhá ukončení hello virtuálních počítačů a objektů blob pořízení snímků každého disku. Toto je jednodušší než koordinující snímky spuštění virtuálního počítače, ale vyžaduje to pár minut nečinnosti. Pro tento proces postupujte takto:

1. Vypněte hello virtuálních počítačů.

2. Vytvoření snímku každý objekt blob virtuálního pevného disku, který přebírá jenom pár sekund.

    toocreate snímek, můžete použít [prostředí PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), hello [Rest API pro Azure Storage](https://msdn.microsoft.com/library/azure/ee691971.aspx), [rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/xplat-cli-install), nebo jeden z knihovny klienta Azure Storage hello jako [ Klientská knihovna pro Hello úložiště pro .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Spusťte virtuální počítač, který končí hello výpadek hello. Obvykle dokončí celý proces hello během několika minut.

Tento proces vypočítá kolekce snímky konzistentní s aplikacemi pro všechny disky hello poskytuje bod obnovení zálohy pro hello virtuálních počítačů. Níže najdete hello kroky toocopy hello snímky tooanother oblast pro zotavení po Havárii.

### <a name="copy-hello-snapshots-tooanother-region"></a>Zkopírujte hello snímky tooanother oblast

Vytváření snímků hello samostatně nemusí dostačovat pro zotavení po Havárii. Také je potřeba replikovat hello snímek zálohy tooanother oblast.

Pokud používáte GRS nebo RA-GRS pro disky, v hello snímky jsou replikované toohello sekundární oblasti automaticky. Může být několik minut prodlevy před hello replikace a pokud hello primární datové centrum ocitne mimo provoz, než hello snímky dokončit replikace, nebudete moct tooaccess hello snímky z hello sekundárního datového centra. Hello pravděpodobnost, že to je malá.

> [!Note] 
> Jenom s disky hello ve GRS nebo RA-GRS účet nechrání hello virtuálních počítačů z havárie. Také musíte vytvořit koordinované snímky nebo používat Azure Backup. Toto je požadované toorecover tooa konzistentní stav virtuálního počítače.

Pokud používáte LRS, musíte zkopírovat hello snímky tooa jiný účet úložiště hned po vytvoření snímku hello. Cíl kopírování Hello může být účet úložiště LRS v jiné oblasti, která výsledkem se v oblasti vzdálené kopie hello. Můžete také zkopírovat účet úložiště hello snímku tooan RA-GRS v hello stejné oblasti. V takovém případě bude snímek hello líné replikované toohello vzdálené sekundární oblasti. Zálohování je chráněn z havárie v primární lokalitě hello po dokončení kopírování hello a replikace.

toocopy vaše přírůstkové snímky pro zotavení po Havárii efektivně, zkontrolujte hello pokyny v [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md).

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png)   

### <a name="recovery-from-snapshots"></a>Obnovení ze snímků

tooretrieve snímek, zkopírujte jej toomake nového objektu blob. Pokud kopírujete hello snímek z hello primární účet, můžete zkopírovat hello snímku přes základní objekt blob toohello hello snímku, proto navrácení hello disku toohello snímku; To se označuje jako povýšení hello snímku. Pokud kopírujete zálohy snímku hello pomocí jiného účtu (v případě hello RA-GRS), musí být zkopírován tooa primární účet. Můžete zkopírovat snímek [pomocí prostředí PowerShell](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) nebo pomocí nástroje AzCopy hello. Další informace najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

V případě hello virtuálních počítačů s více disky, musíte zkopírovat všechny hello snímky, které jsou součástí stejné koordinované hello bod obnovení. Po zkopírujete BLOB VHD toowritable snímky hello, můžete použít objekty BLOB toorecreate hello virtuálního počítače pomocí šablony hello pro hello virtuálních počítačů.

## <a name="other-options"></a>Další možnosti

### <a name="sql-server"></a>SQL Server

SQL Server běžící ve virtuálním počítači má svou vlastní integrované funkce toobackup vaše tooAzure databáze systému SQL Server úložiště objektů Blob nebo sdílené složky. Pokud je účet úložiště hello GRS nebo RA-GRS, tyto zálohy v účtu úložiště hello sekundárního datového centra v hello události po havárii, můžete přistupovat pomocí hello stejná omezení, jak je popsáno dříve. Další informace najdete v tématu [zálohování a obnovení pro SQL Server v Azure Virtual Machines](../../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Přidání toobackup a obnovení [SQL serveru skupin dostupnosti Always On](../../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) můžete udržovat sekundární repliky databáze toogreatly zkrátit čas obnovení po havárii hello.

## <a name="other-considerations"></a>Další důležité informace

Tento článek má popsané jak toobackup nebo proveďte snímků virtuálních počítačů a jejich zotavení po havárii toosupport disky a jak toouse těchto toorecover. U modelu Azure Resource Manager hello Spousta lidí používá šablony toocreate jejich virtuální počítače a další infrastrukturou v Azure. Můžete použít šablonu toocreate virtuální počítač, který má hello stejnou konfiguraci pokaždé, když. Pokud používáte vlastní Image pro vytváření virtuálních počítačů, je rovněž třeba ověřit obrázků jsou chráněné pomocí účet toostore RA-GRS je.

V důsledku toho procesu zálohování může být kombinací dvě věci:

1. Zálohování hello dat (disky)
2. Konfigurace zálohování hello (šablony, vlastních bitových kopií)

V závislosti na hello zálohování možnost, kterou zvolíte může mít zálohu hello toohandle hello dat a konfigurace hello nebo služby zálohování hello může zpracovávat veškerý který pro vás.

## <a name="appendix---understanding-hello-impact-of-lrs-grs-and-ra-grs"></a>Příloha - porozumění hello vlivu LRS, GRS a RA-GRS

Pro účty úložiště v Azure existují tři typy redundanci dat, které byste měli uvažovat o zotavení po havárii – místně redundantní (LRS), geograficky redundantní (GRS), nebo geograficky redundantní s čtení (RA-GRS) přístup. 

Místně redundantní úložiště (LRS) uchovává tři kopie dat hello v hello stejném datovém centru. Při zápisu dat hello, jsou aktualizovány všechny tři kopie dříve, než úspěch vrácena toohello volajícího, abyste věděli, že jsou identické. Disk je chráněný před místní selhání, protože je to velmi nepravděpodobné, že všechny tři kopie bude mít dopad na hello stejnou dobu. V případě hello LRS neexistuje žádný geografická redundance, takže hello disku není chráněn před závažné chyby, které můžou mít vliv celého datového centra nebo úložiště jednotku.

S GRS a RA-GRS v primární oblasti hello (vámi zvolené) jsou uchovány tři kopie dat a další tři kopie dat jsou uchovány v odpovídající sekundární oblasti (nastavuje Azure). Například pokud data uchováváte v západní USA, hello data budou replikované tooEast USA. To se provádí asynchronně a existuje malé zpoždění mezi toohello aktualizace primární a sekundární. Repliky disků hello na sekundární lokalitě hello jsou konzistentní u jednotlivých disků (s odloženým hello), ale repliky více disků active pravděpodobně není synchronizována **mezi sebou**. toohave repliky konzistentní napříč více disky, snímky konzistentní s aplikacemi, je potřeba.

Hello hlavní rozdíl mezi GRS a RA-GRS je s RA-GRS, může číst sekundární kopie hello kdykoli. Pokud dojde k problému, který vykreslí hello data v primární oblasti hello nedostupné, hello tým Azure bude každý úsilí toorestore přístup. Zatímco primární hello je mimo provoz, pokud máte povolené RA-GRS, můžete přístup k datům hello v hello sekundárního datového centra. Proto pokud máte v plánu tooread z repliky hello při hello primární je nedostupná, pak RA-GRS měli zvážit.

Pokud se jím toobe významné výpadku, hello tým Azure může aktivovat selhání geograficky a změňte hello primárního DNS položky toopoint toosecondary úložiště. Pokud máte buď GRS nebo RA-GRS povolená, můžete v tomto okamžiku přistupovat k datům hello v oblasti hello, který používá sekundární toobe hello. Jinými slovy Pokud je váš účet úložiště GRS a došlo k potížím, můžete přistupovat hello sekundární úložiště jenom v případě, že je geo-převzetí služeb při selhání.

Další informace najdete v tématu [co toodo, když dojde k výpadku Azure Storage](storage-disaster-recovery-guidance.md). 

Všimněte si, že Microsoft řídí, zda dojde k selhání. Převzetí služeb při selhání není řízený na účet úložiště, takže ji není rozhodnuto pomocí jednotliví zákazníci. tooimplement zotavení po havárii pro účty konkrétní úložiště nebo disky virtuálního počítače, je nutné použít hello techniky popsané v tomto článku.
