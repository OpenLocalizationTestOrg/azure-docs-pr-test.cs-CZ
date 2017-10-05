---
title: "Zálohování a zotavení po havárii pro disky Azure IaaS | Microsoft Docs"
description: "V tomto článku vysvětlíme, jak naplánovat zálohování a obnovení po havárii (DR) IaaS virtuálních počítačů (VM) a disky v Azure. Tento dokument popisuje spravované i nespravované disky"
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
ms.openlocfilehash: 03d38bc3383b5fd39eca5ca67c315b34b98f0c39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Zálohování a zotavení po havárii pro disky Azure IaaS

V tomto článku vysvětlíme, jak naplánovat zálohování a obnovení po havárii (DR) IaaS virtuálních počítačů (VM) a disky v Azure. Tento dokument popisuje spravované i nespravované disky.

Nejprve budeme mluvit na předdefinované odolnost proti chybám možnosti platformy Azure které pomáhají chránit proti místní selhání. Potom probereme scénáře po havárii, není plně předmětem integrované funkce, která je hlavní téma řešený v tomto dokumentu. Ukážeme vám také několik příkladů zatížení scénáře, kde může použít různé aspekty zálohování a zotavení po Havárii. Potom: přečtěte možná řešení pro zotavení po Havárii disků IaaS. 

## <a name="introduction"></a>Úvod

Platformy Azure využívá k ochraně zákazníků selhání lokalizované hardwaru, které můžou nastat různé metody pro redundanci a odolnost proti chybám. Místní selhání může zahrnovat problémy s počítačem serveru úložiště Azure, která ukládá část dat pro virtuální disk nebo selhání SSD nebo pevný disk na tomto serveru. Tyto součásti selhání izolované hardwaru může dojít během normálních operací a platformy je navržený jako odolné vůči selhání. Hlavní havárie může způsobit selhání nebo inaccessibility velkého počtu serverů úložiště nebo celého datového centra. Při z lokálních selhání jsou obvykle chráněné virtuální počítače a disky, jsou nutné k ochraně vaše úlohy celou oblast závažné selhání (například významnější havárie) ovlivňující virtuálních počítačů a disků další kroky.

Kromě možnosti selhání platformy může dojít k problémům s aplikací zákazníka nebo data. Nová verze aplikace může být například nechtěně narušující, změňte na data. V takovém případě můžete vrátit aplikace a data, která mají na předchozí verzi obsahující poslední známého funkčního stavu. To vyžaduje údržbu pravidelných záloh.

Místní zotavení po havárii musíte zálohovat vaše disky virtuálních počítačů IaaS v jiné oblasti. 

Předtím, než se podíváme na možnosti zálohování a zotavení po Havárii, můžeme recap několik metod k dispozici pro zpracování lokálních selhání.

### <a name="azure-iaas-resiliency"></a>Odolnost proti chybám Azure IaaS

*Odolnost proti chybám* odkazuje na tolerance pro běžné chyby, ke kterým došlo v hardwarové součásti. Odolnost proti chybám je možnost obnovení v případě selhání a i nadále fungovat. Nejedná se o zamezení selhání, ale odpověď na selhání způsobem, který zabraňuje výpadku nebo ztráty dat. Cílem odolnosti je pro návrat aplikace plně funkční stavu selhání. Azure virtuální počítače a disky jsou navržené jako odolné vůči běžné chyby hardwaru. Dejte nám podívejte, jak na platformu Azure IaaS poskytuje tuto odolnost proti chybám.

Virtuální počítač se skládá ze dvou částí především: (1) A výpočetní server a (2) je trvalé disky. Obě ovlivnit odolnost proti chybám virtuálního počítače.

Pokud na serveru hostitele výpočtů Azure, zaštiťující virtuálního počítače dojde k selhání hardwaru (což je taková situace vzácná), Azure je určena pro automatické obnovení virtuálního počítače na jiný server. V takovém případě si všimnete restart a virtuální počítač bude zálohovat po určité době. Azure automaticky zjišťuje takové selhání hardwaru a provede obnovení k zajištění, že zákazník virtuálních počítačů bude k dispozici co nejdříve.

O discích IaaS odolnost dat je důležité pro platformu trvalého úložiště. Zákazníci, Azure mají důležitých podnikových aplikací běžících na IaaS a jsou závislé na trvalost dat. Ochrana Azure návrhy pro tyto disky IaaS s tři redundantní kopie dat uložených místně, poskytuje vysokou odolnost proti selhání místní. Pokud jeden z hardwarové součásti, které obsahuje váš disk selže, virtuální počítač není dopad, protože nejsou k dispozici dvě další kopie pro podporu požadavků na disku. Funguje bez problémů i v případě selžou dva různé hardwarové komponenty podpora disk ve stejnou dobu (které by bylo velmi výjimečných). K zajištění, že vždy uchováváme tři repliky, nastavení služby Azure Storage automaticky vytvoří novou kopii dat na pozadí, pokud některý z tři kopie nedostupný. Proto by nemělo být potřeba pomocí Azure disky RAID pro odolnost proti chybám. Jednoduché RAID 0 konfigurace by mělo být dostatečné pro prokládání disky v případě potřeby vytvořte větší svazky.

Z důvodu této architektuře **Azure má doručit konzistentně podnikové úrovni odolnost pro IaaS disky s špičkový NULOVÉ % [rozpočítat na roční období míra selhání](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Lokalizované hardwaru chyb ve výpočetní hostitele nebo v platformě úložiště můžete někdy výsledek v dočasné nedostupnosti virtuálního počítače, které se vztahuje [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) dostupnosti virtuálních počítačů. Azure poskytuje špičkový SLA pro jedné instance virtuálních počítačů, které použijte disky úložiště Premium.

Aby se předešlo úlohy aplikací z výpadek kvůli dočasné nedostupnosti disk nebo virtuální počítač, můžete využít zákazníci [skupiny dostupnosti](../../virtual-machines/windows/manage-availability.md). Dvě nebo více virtuálních počítačů v nastavení dostupnosti poskytuje redundanci pro aplikaci. Azure potom vytvoří tyto virtuální počítače a disky v doménách selhání samostatné s různé součásti napájení, sítě a serveru. Proto selhání lokalizované hardwaru obvykle nemají vliv na víc virtuálních počítačů v sadě ve stejnou dobu, tím vysokou dostupnost pro vaši aplikaci. Má se za vhodné použít skupiny dostupnosti, pokud se vyžaduje vysoká dostupnost. Další informace najdete v tématu na zotavení po havárii aspekty, které jsou popsané dál.

### <a name="backup-and-disaster-recovery"></a>Zálohování a zotavení po havárii

Zotavení po havárii (DR), je schopnost zotavit výjimečných ale hlavní incidenty:-pouze dočasné, celou škálování selhání, jako je například přerušení služby, které ovlivňuje celou oblast. Zotavení po havárii zahrnuje zálohování dat a archivaci a může zahrnovat ruční zásah, například obnovit databázi ze zálohy.

Platformy Azure integrovanou ochranu proti selhání lokalizované nemusí plně chránit virtuální počítače nebo disky v případě hlavní havárie, které může způsobit výpadky ve velkém měřítku. To zahrnuje závažné události, jako je například datové centrum, se setkají hurikán, zemětřesení, ještě efektivněji nebo selhání jednotky hardwaru velkém měřítku. Kromě toho může dojít k chybám způsobeným problémy dat nebo aplikace.

K ochraně vašich úloh IaaS z výpadky, měli byste naplánovat redundanci a zálohování za účelem umožnění obnovy. Pro zotavení po havárii měli byste naplánovat redundance a zálohování v jiném geografické umístění směrem od primární lokality. To pomáhá zajistit, že záloha není ovlivněn stejnou událost, která původně dopad na počítač nebo disků. Další informace najdete v tématu [zotavení po havárii pro aplikace vytvořené v Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

Důležité informace o vaší zotavení po Havárii může zahrnovat následující aspekty:

1. Vysoké dostupnosti (HA) je schopnost aplikace a pokračovat v činnosti ve stavu v pořádku, bez významné výpadky. Podle "stavu v pořádku," jsme znamená reaguje aplikace a uživatelé můžou připojit k aplikaci a pracovat s ním. Některé důležité aplikace a databáze může být požadované k dispozici vždy, i v případě, že tam jsou chyby samotné platformy. Pro tyto úlohy musíte naplánovat redundance pro aplikace a také data.

2. Odolnost data: V některých případech hlavní faktor je zajistit, že data se zachová, i v případě havárie. Proto může být nutné zálohování dat v jiné lokalitě. Pro taková zatížení možná nebudete potřebovat úplné redundance pro aplikace, ale pravidelné zálohy disků.

## <a name="backup-and-dr-scenarios"></a>Zálohování a zotavení po Havárii scénáře

Podívejme se na několik příkladů typické scénáře zatížení aplikací a jaké jsou požadavky týkající se plánování zotavení po havárii.

### <a name="scenario-1-major-database-solutions"></a>Scénář 1: Hlavní databáze řešení

Vezměte v úvahu produkční databázový server jako SQL Server nebo Oracle, který podporuje vysokou dostupnost. Kritický produkční aplikace a uživatelé závisí na tuto databázi. Plán obnovení po havárii pro tento systém může třeba podporovat následující požadavky:

1. Data musí být chráněný a obnovitelný.
2.  Server musí být k dispozici pro použití.

Může to vyžadovat zachování repliky databáze v jiné oblasti jako záložní. V závislosti na požadavcích pro dostupnost serveru a obnovení dat může řešení rozsahu od aktivní-aktivní nebo aktivní – pasivní repliky lokality do pravidelné zálohy dat offline. Relační databáze jako je například SQL Server a Oracle poskytují různé možnosti pro replikaci. Pro systém SQL Server [SQL serveru skupin dostupnosti Always On](https://msdn.microsoft.com/library/hh510230.aspx) lze použít pro vysokou dostupnost.

Databáze NoSQL, jako MongoDB také podporují [repliky](https://docs.mongodb.com/manual/replication/) redundanci. Můžete použít repliky pro vysokou dostupnost.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scénář 2: Cluster redundantní virtuální počítače

Vezměte v úvahu zatížení zpracovává cluster virtuálních počítačů, které poskytují redundanci a vyrovnávání zatížení. Příkladem může být cluster Cassandra nasazené v oblasti. Tento typ architektury již poskytuje vysokou úroveň redundance v rámci této oblasti. Chránit úlohy v případě selhání místní úrovni, byste však měli zvážit šíření clusteru v rámci dvou oblastí nebo prováděním pravidelné zálohy jiné oblasti.

### <a name="scenario-3-iaas-application-workload"></a>Scénář 3: IaaS aplikace zatížení

To může být typické provozní úlohu spuštěnou na virtuální počítač Azure. Například může být na webovém serveru nebo souborového serveru, která uchovává obsah a dalším prostředkům lokality. Mohou být také uživatelské obchodní aplikace běžící v virtuální počítač, který uložené prostředky, stav aplikace a jeho data na disky virtuálních počítačů. V takovém případě je důležité se ujistit, že provedete zálohy v pravidelných intervalech. Četnost zálohování by měla být založena na povaze zatížení virtuálních počítačů. Například pokud aplikace spouští každý den a mění data, zálohování by měl být bere každou hodinu.

Dalším příkladem je server pro sestavy této načítat data z jiných zdrojů a generuje souhrnné sestavy. Ztrátě tohoto virtuálního počítače nebo disky povede ke ztrátě sestavy. Nicméně je možné spusťte proces vytváření sestav a znovu vygenerovat výstup. V takovém případě nemáte skutečně ztrátu dat i v případě, že server pro sestavy je dosáhl s po havárii, tak může mít vyšší úroveň tolerance pro ztráty část dat na serveru sestav. V takovém případě méně časté zálohování je možnost snížit náklady.

### <a name="scenario-4-iaas-application-data-issues"></a>Scénář 4: Aplikace IaaS dat problémy

Máte aplikaci, která vypočítá, udržuje a slouží důležitá obchodní data, například informace o cenách. Nová verze aplikace měla chyb softwaru, který nesprávně počítaný cenách a poškozený stávající data a obchodu Spojených států obsloužených platformu. Nejlepším řešením zde, je nejprve vrátit na předchozí verzi aplikace a data. Chcete-li povolit, trvat pravidelné zálohy systému.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Řešení zotavení po havárii: Služba Azure Backup

[Služba Azure Backup](https://azure.microsoft.com/services/backup/) je lze použít pro zálohování a zotavení po Havárii a funguje s [spravované disky](../../virtual-machines/windows/managed-disks-overview.md) a také [nespravované disky](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). Můžete vytvořit úlohu zálohování s zálohy založené na čase, snadno obnovení virtuálních počítačů a zásady uchovávání záloh. 

Pokud používáte [disky úložiště Premium](storage-premium-storage.md), [spravované disky](../../virtual-machines/windows/managed-disks-overview.md), nebo jiných typů disku s [místně redundantní úložiště (LRS)](storage-redundancy.md#locally-redundant-storage) možnost, je velmi důležité využít pravidelné zálohy zotavení po Havárii. Zálohování Azure ukládá data do trezoru služeb zotavení pro dlouhodobé uchovávání. Vyberte [geograficky redundantní úložiště (GRS)](storage-redundancy.md#geo-redundant-storage) možnost pro trezor služeb zotavení zálohování. Který bude zajištěno, aby zálohování se replikují do jiné oblasti Azure pro zabezpečení z místní havárie.

Pro [nespravované disky](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), můžete použít typ úložiště LRS disků IaaS, ale ujistěte se, že Azure Backup je povolena možnost GRS pro trezor služeb zotavení.

**Pokud použijete [GRS](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) možnost pro nespravovaná disky, je stále nutné snímky konzistentní s aplikacemi pro zálohování a zotavení po Havárii.** Je nutné použít buď [služby zálohování Azure](https://azure.microsoft.com/services/backup/) nebo [snímky konzistentní](#alternative-solution-consistent-snapshots).

Zde je souhrn řešení pro zotavení po Havárii.

| Scénář | Automatické replikace | Řešení zotavení po Havárii |
| --- | --- | --- |
| *Disky úložiště Premium* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nespravované LRS disky* | Místní ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Nespravované GRS disky* | Pro různé oblasti ([GRS](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Snímky konzistentní s aplikacemi](#alternative-solution-consistent-snapshots) |
| *Nespravované RA-GRS disky* | Pro různé oblasti ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Snímky konzistentní s aplikacemi](#alternative-solution-consistent-snapshots) |

Zajištění vysoké dostupnosti můžete nejlépe pomocí splní prostřednictvím používat spravované disky v skupiny dostupnosti společně s Azure Backup. Pokud používáte nespravované disky, stále můžete zálohování Azure pro zotavení po Havárii. Pokud není možné používat Azure Backup, pak trvá [snímky konzistentní](#alternative-solution-consistent-snapshots) jak je popsáno v další části je alternativní řešení pro zálohování a zotavení po Havárii.

Vaše volby pro vysokou dostupnost, zálohování a zotavení po Havárii aplikace nebo infrastruktury úrovni může být reprezentován jako níže:

| *Úroveň* | Vysoká dostupnost   | Zálohování nebo zotavení po Havárii |
| --- | --- | --- |
| *Aplikace* | Always On SQL | Azure Backup |
| *Infrastruktury*  | Skupina dostupnosti  | GRS s snímky konzistentní s aplikacemi |

### <a name="using-the-azure-backup-service"></a>Pomocí služby zálohování Azure

[Zálohování Azure](../../backup/backup-azure-vms-introduction.md) může zálohovat virtuální počítače s Windows nebo Linuxem do trezoru služeb zotavení Azure. Zálohování a obnovení důležitých podnikových dat ztěžuje skutečnost, že důležitých podnikových dat je třeba zálohovat v době spuštění aplikace, které produkují data. Chcete-li vyřešit tím, Azure Backup poskytuje zálohování konzistentní s aplikací pro úlohy Microsoftu pomocí služby Stínová svazku (VSS) k zajištění, že je správně zapisovat data do úložiště. Pro virtuální počítače s Linuxem je možné, pouze konzistentními soubory zálohy, protože Linux nemá funkce odpovídající VSS.

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png)   

Spustí úlohu zálohování Azure Backup v naplánovaném čase spustí rozšíření zálohování nainstalovaná ve virtuálním počítači k pořízení snímku v daném okamžiku. Snímek se provede v koordinaci s VSS získat konzistentního snímku disků ve virtuálním počítači bez nutnosti ho vypnout. Rozšíření zálohování ve virtuálním počítači vyprázdnění všech zápisů před přepnutím konzistentní snímek všechny disky. Po pořízení snímku, data se přenáší přes Azure Backup do trezoru záloh. Chcete-li proces zálohování efektivnější, služba identifikuje a přenáší pouze bloky dat, které se změnily od poslední zálohy.


Pokud chcete obnovit, můžete zobrazit dostupné zálohy pomocí zálohování Azure a poté zahájit obnovení. Můžete vytvořit a obnovit zálohování Azure prostřednictvím [portál Azure](https://portal.azure.com/), [pomocí prostředí PowerShell](../../backup/backup-azure-vms-automation.md ), nebo pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/xplat-cli-install). Další informace najdete v tématu Zálohování Azure.

### <a name="steps-to-enable-backup"></a>Postup povolení zálohování

Následující kroky jsou obvykle používány k povolení zálohování virtuálních počítačů pomocí [portálu Azure](https://portal.azure.com/). Bude existovat několik odchylek v závislosti na přesný scénář. Odkazovat [Azure Backup](../../backup/backup-azure-vms-introduction.md) dokumentace pro úplné podrobnosti. Azure také zálohování [podporuje virtuální počítače s spravované disky](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Vytvoření trezoru služeb zotavení pro virtuální počítač pomocí následujících kroků:

    a.  Pomocí [portál Azure](https://portal.azure.com/), procházet všechny prostředky a vyhledávat "Trezory služeb zotavení".

    b.  V nabídce trezory služeb zotavení klikněte na tlačítko Přidat a postupujte podle kroků pro vytvoření nového trezoru ve stejné oblasti jako virtuální počítač. Například pokud vaše virtuální počítač v oblasti západní USA, vyberte západní USA trezoru.

2.  Ověření úložiště replikace pro nově vytvořený trezor. K tomu, přístup k úložišti z okna trezory služeb zotavení a přejděte do nastavení nebo zálohu konfigurace. Zkontrolujte, zda že je vybrána možnost GRS ve výchozím nastavení. Tím bude zajištěno, že váš trezor je automaticky replikovat do sekundárního datového centra. Například trezoru v západní USA bude automaticky replikován východní USA.

3.  Nakonfigurujte zásady zálohování a vyberte virtuální počítač ze stejné uživatelské rozhraní.

4.  Zkontrolujte, zda že je nainstalovaný na Virtuálním počítači agenta zálohování. Pokud váš virtuální počítač je vytvořen pomocí image Galerie Azure, agenta zálohování již nainstalována. Jinak (tj. Pokud používáte vlastní image), použijte pokyny k [nainstalujte agenta virtuálního počítače na virtuálním počítači](../../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Ujistěte se, že virtuální počítač umožňuje připojení k síti pro službu zálohování funkce. Postupujte podle těchto pokynů pro [připojení k síti](../../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  Po dokončení výše uvedené kroky, zálohování se spustí v pravidelných intervalech podle zásady zálohování. V případě potřeby můžete aktivovat první zálohy ručně na řídicím panelu trezoru na portálu Azure.

Automatizace Azure Backup pomocí skriptů, naleznete v [rutiny prostředí PowerShell pro zálohování virtuálních počítačů](../../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Kroky pro obnovení

Pokud potřebujete opravit nebo znovu vytvořit virtuální počítač, můžete obnovit virtuální počítač z jakýchkoli bodů obnovení záloh v trezoru. Existuje několik různých možností při provedení obnovení:

1.  Můžete vytvořit nový virtuální počítač jako reprezentace v okamžiku zálohy virtuálního počítače.

2.  Můžete obnovit disky a potom pomocí této šablony virtuálního počítače upravit a znovu vytvořit obnovený virtuální počítač. 

Zobrazí pokyny k [portálu Azure použijte k obnovení virtuálních počítačů](../../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). Dokument taky vysvětluje konkrétní kroky pro obnovení zálohy virtuálních počítačů do spárované datového centra pomocí geograficky redundantní úložiště záloh, v případě havárie v primárním datovém centru. V takovém případě Azure Backup používá výpočetní služba ze sekundární oblasti k vytvoření obnovený virtuální počítač.

Můžete také pomocí prostředí PowerShell pro [obnovení virtuálního počítače](../../backup/backup-azure-vms-automation.md#restore-an-azure-vm) nebo [vytváření nového virtuálního počítače z obnovit disky](../../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternativní řešení: Snímky konzistentní

Pokud není možné pomocí služby zálohování Azure, můžete implementovat vlastní zálohovací mechanismus, pomocí snímků. Je složité vytvořit snímky konzistentní s aplikacemi pro všechny disky používané Virtuálním počítačem a tyto snímky se pak replikují do jiné oblasti. Z tohoto důvodu Azure zvažuje využívání služby zálohování vhodnější než vytváření vlastní řešení. Pokud používáte RA-GRS nebo GRS úložiště pro disky, snímky jsou automaticky replikovat do sekundárního datového centra. Pokud používáte LRS úložiště pro disky, musíte replikovat data. Další informace najdete v tématu [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md).

Snímek je reprezentace objektu v určitém bodě v čase. Snímek zpoplatněná fakturace pro přírůstkové velikost dat je blokování. Další informace najdete v tématu [vytvoření snímku blob](../blobs/storage-blob-snapshots.md).

### <a name="creating-snapshots-while-the-vm-is-running"></a>Vytváření snímků spuštěného virtuálního počítače

Když v každém okamžiku může pořízení snímku, pokud je virtuální počítač spuštěný, je stále data vysílá na discích a snímky může obsahovat částečné operace, které byly v cestě. Navíc pokud existují několik disků související se situací, snímky různých disků mohlo dojít v různou dobu, což znamená, že tyto snímky nemusí být koordinované. To je obzvláště problematické pro prokládané svazky, jejichž souborů může být poškozená, pokud se během zálohování provedeny změny.

Abyste předešli této situaci, proces zálohování musí implementovat následující kroky:

1.  Freeze – všechny disky

2.  Vyprázdnění všech zápisů čekající na vyřízení

3.  Potom [vytvoření snímku blob](../blobs/storage-blob-snapshots.md) pro všechny disky

Některé aplikace systému Windows, jako je SQL Server poskytují mechanismus koordinované zálohování pomocí služby Stínová kopie svazku pro vytvoření zálohování konzistentní s aplikací. V systému Linux můžete použít nástroje, jako je fsfreeze pro spolupráci disky, které zajistí konzistentními soubory zálohy, ale není snímky konzistentní s aplikacemi. Tento proces je složitá, takže a měli byste zvážit použití [Azure Backup](../../backup/backup-azure-vms-introduction.md) nebo řešení zálohování jiného výrobce, který už tuto funkci implementovat.

Proces výše způsobí kolekce koordinované snímků pro všechny disky virtuálních počítačů, představující konkrétní zobrazení v okamžiku virtuálního počítače. Toto je bod obnovení zálohy pro virtuální počítač. Proces lze opakovat v naplánovaných intervalech k vytvoření pravidelné zálohy. Níže najdete postup [zkopírujte zálohy do jiné oblasti](#copy-the-snapshots-to-another-region) pro zotavení po Havárii.

### <a name="creating-snapshots-while-the-vm-is-offline"></a>Vytváření snímků při offline virtuálního počítače

Další možnost vytvářet konzistentní zálohy je vypnutí virtuálního počítače a přepnutím blob snímky každého disku. Toto je jednodušší než koordinující snímky spuštění virtuálního počítače, ale vyžaduje to pár minut nečinnosti. Pro tento proces postupujte takto:

1. Vypněte virtuální počítač.

2. Vytvoření snímku každý objekt blob virtuálního pevného disku, který přebírá jenom pár sekund.

    Pro vytvoření snímku, můžete použít [prostředí PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), [Rest API pro Azure Storage](https://msdn.microsoft.com/library/azure/ee691971.aspx), [rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/xplat-cli-install), nebo jeden z knihovny klienta úložiště Azure, jako [Klientská knihovna pro úložiště pro .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Spusťte virtuální počítač, který končí výpadek. Obvykle se celý proces dokončení během několika minut.

Tento proces vypočítá kolekce snímky konzistentní s aplikacemi pro všechny disky, poskytuje bod obnovení zálohy pro virtuální počítač. Níže najdete pokyny pro zkopírování snímky jiné oblasti pro zotavení po Havárii.

### <a name="copy-the-snapshots-to-another-region"></a>Zkopírujte snímky do jiné oblasti

Vytváření snímků samostatně nemusí dostačovat pro zotavení po Havárii. Zálohy snímků musí také replikovat do jiné oblasti.

Pokud používáte GRS nebo RA-GRS pro disky, snímky jsou replikovat do sekundární oblasti automaticky. Může být několik minut prodlevy před replikace a pokud primární datové centrum ocitne mimo provoz, než snímky dokončit replikace, nebudete mít přístup k snímky ze sekundárního datového centra. Pravděpodobnost, že to je malá.

> [!Note] 
> Jenom s disky ve GRS nebo RA-GRS účet nechrání virtuálního počítače z havárie. Také musíte vytvořit koordinované snímky nebo používat Azure Backup. To je potřebné k obnovení virtuálního počítače do konzistentního stavu.

Pokud používáte LRS, musíte zkopírovat snímky na jiný účet úložiště ihned po vytvoření snímku. Cíl kopírování může být účet úložiště LRS v jiné oblasti, která výsledkem kopie se ve vzdálené oblasti. Můžete také zkopírovat snímku na účet úložiště RA-GRS ve stejné oblasti. V takovém případě snímku budou líné replikovány vzdálené sekundární oblast. Zálohování je chráněn z havárie v primární lokalitě jednou kopírování a se replikace nedokončí.

Pokud chcete zkopírovat vaší přírůstkové snímky pro zotavení po Havárii efektivně, zkontrolujte podle pokynů v [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md).

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png)   

### <a name="recovery-from-snapshots"></a>Obnovení ze snímků

Načíst snímek, zkopírujte jej, aby nový objekt blob. Pokud kopírujete snímek z primární účet, můžete zkopírovat snímku přes na základní objekt blob snímku, proto dynamického disku na snímku; To se označuje jako povýšení snímku. Pokud kopírujete zálohy snímku pomocí jiného účtu (v případě RA-GRS), se musí zkopírovat primární účet. Můžete zkopírovat snímek [pomocí prostředí PowerShell](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) nebo pomocí nástroje azcopy. Další informace najdete v tématu [přenos dat pomocí nástroje příkazového řádku Azcopy](storage-use-azcopy.md).

V případě virtuálních počítačů s více disky musíte zkopírovat všechny snímky, které jsou součástí stejného bodu koordinované obnovení. Po snímky zkopírujete do zapisovatelné BLOB VHD, můžete k opětovnému vytvoření virtuálního počítače pro virtuální počítač pomocí šablony objektů BLOB.

## <a name="other-options"></a>Další možnosti

### <a name="sql-server"></a>SQL Server

SQL Server běžící ve virtuálním počítači má svou vlastní integrované funkce zálohování databáze systému SQL Server do úložiště objektů Blob v Azure nebo sdílené složky. Pokud je účet úložiště GRS nebo RA-GRS, dostanete tyto zálohy v účtu úložiště sekundárního datového centra v případě havárie, s stejná omezení, jak je popsáno dříve. Další informace najdete v tématu [zálohování a obnovení pro SQL Server v Azure Virtual Machines](../../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Kromě zálohování a obnovení [SQL serveru skupin dostupnosti Always On](../../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) můžete udržovat sekundární repliky databází výrazně zkrátit čas obnovení po havárii.

## <a name="other-considerations"></a>Další důležité informace

Tento článek má popsané, jak zálohovat nebo pořizovat snímky virtuálních počítačů a jejich disky k podpoře zotavení po havárii a postup použít k obnovení. Spousta lidí s modelem Azure Resource Manager používá šablony v Azure vytvořit jejich virtuální počítače a další infrastrukturou. Chcete-li vytvořit virtuální počítač, který má stejnou konfiguraci pokaždé, když můžete použít šablonu. Pokud používáte vlastní Image pro vytváření virtuálních počítačů, můžete musí také ujistěte se, že vaše image jsou chráněné pomocí účtu RA-GRS k jejich uložení.

V důsledku toho procesu zálohování může být kombinací dvě věci:

1. Zálohování dat (disky)
2. Zálohování konfigurace (šablony, vlastních bitových kopií)

V závislosti na možnost zálohování, který zvolíte bude pravděpodobně nutné zpracovat zálohování dat a konfigurace nebo služby zálohování může zpracovávat veškerý který pro vás.

## <a name="appendix---understanding-the-impact-of-lrs-grs-and-ra-grs"></a>Příloha - porozumění vlivu LRS, GRS a RA-GRS

Pro účty úložiště v Azure existují tři typy redundanci dat, které byste měli uvažovat o zotavení po havárii – místně redundantní (LRS), geograficky redundantní (GRS), nebo geograficky redundantní s čtení (RA-GRS) přístup. 

Místně redundantní úložiště (LRS) uchovává tři kopie dat ve stejném datovém centru. Při zápisu dat, jsou aktualizovány všechny tři kopie, před úspěchu je vrácen volajícímu, abyste věděli, že jsou identické. Disk je chráněný před místní selhání, protože je to velmi nepravděpodobné, že všechny tři kopie bude mít dopad ve stejnou dobu. V případě LRS neexistuje žádný geografická redundance, takže disk není chráněn před závažné chyby, které můžou mít vliv celého datového centra nebo úložiště jednotku.

S GRS a RA-GRS v primární oblasti (vámi zvolené) jsou uchovány tři kopie dat a další tři kopie dat jsou uchovány v odpovídající sekundární oblasti (nastavuje Azure). Například pokud data uchováváte v západní USA, data se replikují do východní USA. To se provádí asynchronně a existuje malé zpoždění mezi aktualizace na primární a sekundární. Repliky disků v sekundární lokalitě nejsou konzistentní u jednotlivých disků (s odloženým), ale nemusí být synchronizované repliky více disků active **mezi sebou**. Pokud chcete, aby konzistentní repliky na více disků, je potřeba snímky konzistentní.

Hlavní rozdíl mezi GRS a RA-GRS je s RA-GRS, může číst sekundární kopie kdykoli. Pokud dojde k problému, který vykreslí data v primární oblasti nedostupné, tým Azure bude snažit obnovit přístup. Zatímco primární je mimo provoz, pokud máte povolené RA-GRS, může přístupu k datům v sekundárního datového centra. Proto pokud máte v plánu číst z repliky, zatímco primární je nedostupná, pak RA-GRS měli zvážit.

Pokud se jím být významné výpadku, tým Azure může aktivovat selhání geograficky a změňte primární záznamy DNS tak, aby odkazovaly do sekundárního úložiště. Pokud máte buď GRS nebo RA-GRS povolená, můžete v tomto okamžiku přístupu k datům v oblasti, která používá jako sekundární. Jinými slovy Pokud je váš účet úložiště GRS a došlo k potížím, můžete přistupovat sekundární úložiště jenom v případě, že je geo-převzetí služeb při selhání.

Další informace najdete v tématu [co dělat, když dojde k výpadku Azure Storage](storage-disaster-recovery-guidance.md). 

Všimněte si, že Microsoft řídí, zda dojde k selhání. Převzetí služeb při selhání není řízený na účet úložiště, takže ji není rozhodnuto pomocí jednotliví zákazníci. Implementace zotavení po havárii pro účty konkrétní úložiště nebo disky virtuálního počítače, je nutné použít techniky popsané v tomto článku.
