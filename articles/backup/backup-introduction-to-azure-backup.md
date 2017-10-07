---
title: aaaWhat je Azure Backup? | Dokumentace Microsoftu
description: "Pomocí Azure Backup tooback a obnovit data a úlohy ze serverů se systémem Windows, pracovních stanic, serverů System Center DPM a virtuálních počítačích Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování a obnovení; recovery services; řešení zálohování"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/11/2017
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 953a19600f67a6b7451f71b1e3234d913816d18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-features-in-azure-backup"></a>Přehled funkcí hello ve službě Azure Backup
Azure Backup je hello služby Azure můžete použít tooback (nebo chránit) a obnovování vašich dat v cloudu Microsoft hello. Azure Backup nahrazuje současná řešení místního nebo odlehlého zálohování spolehlivým, bezpečným a cenově konkurenceschopným cloudovým řešením. Zálohování Azure nabízí několik komponent, které můžete stáhnout a nasadit na hello příslušný počítač, server, nebo v cloudu hello. součást Hello nebo agenta, který nasazujete, závisí na co chcete tooprotect. Všechny součásti zálohování Azure (bez ohledu na to, jestli chcete chránit data místně nebo v cloudu hello) lze použít tooback do trezoru služeb zotavení tooa dat v Azure. V tématu hello [tabulky Azure Backup součásti](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (dále v tomto článku) informace o které komponenta toouse tooprotect konkrétní data, aplikace nebo zatížení.

[Podívejte se na video s přehledem Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Proč používat Azure Backup?
Tradiční řešení zálohování vyvinuly tootreat hello cloud jako koncový bod, nebo cíl statické úložiště, podobně jako toodisks nebo na pásku. Tento přístup je jednoduchý, je omezený a není plně využít výhod základní cloudovou platformu, která přeloží tooan nákladné a neefektivní řešení. Jiné řešení je nákladná, protože skončili platícího pro hello nesprávný typ úložiště nebo úložiště, které nepotřebujete. Jiná řešení jsou neefektivní často, protože nemáte vám nabízí hello typ nebo velikost úložiště, které potřebujete, nebo úlohy správy vyžadují příliš mnoho času. Azure Backup naopak nabízí tyto klíčové výhody:

**Automatická správa úložiště** – hybridní prostředí často vyžadují heterogenní úložiště – některé na místě a některé v hello cloudu. Azure Backup neznamená žádné náklady na používání místních zařízení úložiště. Azure Backup automaticky přiděluje a spravuje úložiště záloh a používá model založený na průběžných platbách. Platím jako--používání znamená, že platíte jenom hello úložiště, které spotřebujete. Další informace najdete v tématu hello [Azure ceny článku](https://azure.microsoft.com/pricing/details/backup).

**Neomezené škálování** – Azure Backup používá základní power hello a neomezené škálování hello Azure cloud toodeliver vysoká dostupnost – žádná údržba nebo monitorování režijní náklady. Můžete nastavit výstrahy tooprovide informace o událostech, ale nepotřebujete tooworry o vysoké dostupnosti pro vaše data v cloudu hello.

**Více možností úložiště** – Jedním z aspektů vysoké dostupnosti je replikace úložiště. Azure Backup nabízí dva typy replikace: [místně redundantní úložiště](../storage/common/storage-redundancy.md#locally-redundant-storage) a [geograficky redundantní úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage). Vyberte možnost hello úložiště záloh podle potřeb:

* Místně redundantní úložiště (LRS) replikuje data třikrát (vytvoří tři kopie dat) ve spárovaném datovém centru v hello stejné oblasti. Místně redundantní úložiště nabízí cenově úsporný způsob ochrany dat před selháním místního hardwaru.

* Geograficky redundantní úložiště (GRS) replikuje sekundární oblasti vašeho dat tooa (stovky miles od hello primární umístění hello zdroje dat). Geograficky redundantní úložiště je nákladnější než místně redundantní úložiště, ale nabízí vyšší úroveň odolnosti dat i v případě regionálního výpadku.

**Neomezený přenos dat** – Azure Backup neomezuje hello množství příchozí nebo odchozí datové přenosy. Zálohování Azure také nebude uplatňovat pro přenos dat hello. Pokud používáte hello Azure Import/Export služby tooimport velké objemy dat, je však s náklady spojené s příchozí data. Další informace o těchto nákladech najdete v části [Pracovní postup zálohování offline v Azure Backup](backup-azure-backup-import-export.md). Odchozí data představují toodata přenést z trezoru služeb zotavení během operace obnovení.

**Šifrování dat** -šifrování dat umožňuje bezpečný přenos a ukládání vašich dat ve veřejném cloudu hello. Ukládání hello šifrovací přístupové heslo místně a se nikdy nepřenáší ani neukládá v Azure. Pokud je nutné toorestore všechny hello dat pouze máte šifrovací přístupové heslo, nebo klíč.

**Zálohování konzistentní s aplikací** -zda zálohování souborového serveru, virtuální počítač nebo databázi SQL, je nutné tooknow bod obnovení obsahuje všechny požadované data toorestore hello záložní kopii. Azure Backup poskytuje zálohování konzistentní s aplikací, které je zajištěno, že další opravy nejsou potřebné toorestore hello data. Obnovení dat aplikací konzistentní snižuje dobu obnovení hello, umožní vám tooquickly návratový tooa běžícím stavu.

**Dlouhodobé uchovávání** -místo přepínání záložní kopie z disku tootape a přesunutí hello pásku tooan odlehlého umístění, můžete použít Azure pro krátkodobé a dlouhodobé uchovávání. Azure nepodporuje omezit hello časový úsek data zůstávají v trezoru zálohování nebo obnovení služby. Data můžete v trezoru ponechat, jak dlouho chcete. Služba Azure Backup má omezení 9999 bodů obnovení na chráněnou instanci. V tématu hello [zálohování a uchovávání](backup-introduction-to-azure-backup.md#backup-and-retention) v tomto článku Další informace o tom, jak tento limit může mít vliv na svoje potřeby zálohování.  

## <a name="which-azure-backup-components-should-i-use"></a>Které komponenty Azure Backup mám použít?
Pokud si nejste jisti, která komponenta Azure Backup pracuje vašim potřebám, najdete v následující tabulce získáte informace o můžete chránit pomocí jednotlivé komponenty hello. Hello portál Azure poskytuje průvodce, která je integrována v hello portál, tooguide vás výběr hello součást toodownload a nasazení. Hello průvodce, který je součástí hello vytvoření trezoru služeb zotavení, vás provede kroky hello pro výběr cíle zálohování a výběr tooprotect hello dat nebo aplikace.

| Komponenta | Výhody | Omezení | Co se chrání? | Kde jsou zálohy uložené? |
| --- | --- | --- | --- | --- |
| Agent Azure Backup (MARS) |<li>Zálohování souborů a složek ve fyzickém nebo virtuálním operačním systému Windows (virtuální počítač může být místní nebo v Azure)<li>Není vyžadován samostatný záložní server. |<li>Zálohování 3x denně <li>Nerozpoznávají se aplikace; obnovování pouze na úrovni souboru, složky nebo svazku. <li>  Bez podpory Linux |<li>Soubory <li>Složky |Trezor služby Recovery Services |
| System Center DPM |<li>Snímky schopné rozeznávat aplikace (VSS)<li>Úplná flexibilita pro případ tootake zálohování<li>Členitost obnovení (všechny)<li>Může použít trezor služby Recovery Services<li>Podpora Linuxu ve virtuálních počítačích Hyper-V a VMware <li>Zálohování a obnovení virtuálních počítačů VMware pomocí DPM 2012 R2 |Nejde zálohovat úlohu Oracle.|<li>Soubory <li>Složky<li> Svazky <li>Virtuální počítače<li> Aplikace<li> Úlohy |<li>Trezor služby Recovery Services,<li> Místně připojený disk,<li>  Páska (pouze místní) |
| Server Azure Backup |<li>Snímky schopné rozeznávat aplikace (VSS)<li>Úplná flexibilita pro případ tootake zálohování<li>Členitost obnovení (všechny)<li>Může použít trezor služby Recovery Services<li>Podpora Linuxu ve virtuálních počítačích Hyper-V a VMware<li>Zálohování a obnovení virtuálních počítačů VMware <li>Nevyžaduje licenci produktu System Center |<li>Nejde zálohovat úlohu Oracle.<li>Vždy vyžaduje živé předplatné Azure<li>Nepodporuje zálohování na pásku |<li>Soubory <li>Složky<li> Svazky <li>Virtuální počítače<li> Aplikace<li> Úlohy |<li>Trezor služby Recovery Services,<li> Místně připojený disk |
| Zálohování virtuálních počítačů Azure IaaS |<li>Nativní zálohy pro Windows a Linux<li>Bez nutnosti instalace konkrétního agenta<li>Zálohování na úrovni prostředků infrastruktury bez potřeby infrastruktury zálohování |<li>Zálohování virtuálních počítačů jednou denně <li>Obnovení virtuálních počítačů pouze na úrovni disku<li>Nemožnost místního zálohování |<li>Virtuální počítače <li>Všechny disky (pomocí PowerShellu) |<p>Trezor služby Recovery Services</p> |

## <a name="what-are-hello-deployment-scenarios-for-each-component"></a>Jaké jsou hello scénáře nasazení pro každou součást?
| Komponenta | Lze nasadit v Azure? | Lze nasadit místně? | Podpora cílového úložiště |
| --- | --- | --- | --- |
| Agent Azure Backup (MARS) |<p>**Ano**</p> <p>Hello agenta Azure Backup lze nasadit na všechny virtuální počítače Windows serveru, který běží v Azure.</p> |<p>**Ano**</p> <p>agent zálohování Hello se dá nasadit na všechny virtuální počítače Windows serveru nebo fyzického počítače.</p> |<p>Trezor služby Recovery Services</p> |
| System Center DPM |<p>**Ano**</p><p>Další informace o [jak tooprotect úlohy v Azure pomocí System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ano**</p> <p>Další informace o [jak tooprotect zatížení a virtuální počítače ve vašem datovém centru](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Místně připojený disk,</p> <p>Trezor služby Recovery Services,</p> <p>páska (pouze místní)</p> |
| Server Azure Backup |<p>**Ano**</p><p>Další informace o [jak tooprotect úlohy v Azure pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ano**</p> <p>Další informace o [jak tooprotect úlohy v Azure pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</p> |<p>Místně připojený disk,</p> <p>Trezor služby Recovery Services</p> |
| Zálohování virtuálních počítačů Azure IaaS |<p>**Ano**</p><p>Součást prostředků infrastruktury Azure</p><p>Specializované pro [zálohování virtuálních počítačů Azure IaaS (infrastruktura jako služba)](backup-azure-vms-introduction.md).</p> |<p>**Ne**</p> <p>Pomocí System Center DPM tooback až virtuální počítače ve vašem datovém centru.</p> |<p>Trezor služby Recovery Services</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Které aplikace a úlohy lze zálohovat?
Hello následující tabulka poskytuje přehled hello dat a úlohy, které se dají chránit pomocí Azure Backup. sloupec řešení Azure Backup Hello má dokumentaci k nasazení toohello odkazy pro toto řešení. Každou komponentu Azure Backup je možné nasadit v prostředí Classic (nasazení v Service Manageru) nebo v prostředí modelu nasazení Resource Manageru.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Data nebo úloha | Zdrojové prostředí | Řešení Azure Backup |
| --- | --- | --- |
| Soubory a složky |Windows Server |<p>[Agent Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Soubory a složky |Počítač s Windows |<p>[Agent Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Virtuální počítač s technologií Hyper-V (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Virtuální počítač s technologií Hyper-V (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup hello),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup hello)</p> |
| Virtuální počítače Azure IaaS (Windows) |spuštěno v Azure |[Azure Backup (rozšíření virtuálního počítače)](backup-azure-vms-introduction.md) |
| Virtuální počítače Azure IaaS (Linux) |spuštěno v Azure |[Azure Backup (rozšíření virtuálního počítače)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Podpora Linuxu
Hello následující tabulka uvádí hello Azure Backup součásti, které mají podporu pro Linux.  

| Komponenta | Podpora Linuxu (schváleného Azure) |
| --- | --- |
| Agent Azure Backup (MARS) |Ne (pouze agent založený na Windows) |
| System Center DPM |<li> Záloha s konzistentními soubory virtuálních počítačů hosta s Linuxem v Hyper-V a VMWaru<br/> <li> Obnovení virtuálního počítače pro virtuální počítače hosta s Linuxem v Hyper-V a VMwaru </br> </br>  *Zálohování s konzistentními soubory není dostupné pro virtuální počítače Azure* <br/> |
| Server Azure Backup |<li>Záloha s konzistentními soubory virtuálních počítačů hosta s Linuxem v Hyper-V a VMWaru<br/> <li> Obnovení virtuálního počítače pro virtuální počítače hosta s Linuxem v Hyper-V a VMwaru </br></br> *Zálohování s konzistentními soubory není dostupné pro virtuální počítače Azure*  |
| Zálohování virtuálních počítačů Azure IaaS |Zálohování konzistentní s aplikací pomocí [rozhraní s předzálohovacími a pozálohovacími skripty](backup-azure-linux-app-consistent.md)<br/> [Detailní obnovení souborů](backup-azure-restore-files-from-vm.md)<br/> [Obnovení všech disků virtuálního počítače](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [Obnovení virtuálního počítače](backup-azure-arm-restore-vms.md#create-a-new-vm-from-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Použití virtuálních počítačů služby Storage úrovně Premium s Azure Backup
Azure Backup chrání virtuální počítače služby Storage úrovně Premium. Azure Premium Storage je jednotka SSD (SSD) – na základě úložiště určené toosupport I náročnými úlohy. Služba Storage úrovně Premium je zajímavá pro úlohy virtuálních počítačů. Další informace o Premium Storage najdete v článku hello, [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage/common/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Zálohování virtuálních počítačů služby Storage úrovně Premium
Při zálohování virtuálních počítačů služby Storage úrovně Premium, služba Backup hello vytvoří dočasné pracovní umístění s názvem "AzureBackup-", v účtu Storage úrovně Premium hello. velikost Hello hello pracovního umístění je rovna toohello velikost snímku bodu obnovení hello. Ujistěte se, že má účet služby Premium Storage hello dostatkem volného místa tooaccommodate hello dočasné pracovní umístění. Další informace najdete v článku hello, [omezení úložiště premium](../storage/common/storage-premium-storage.md#scalability-and-performance-targets). Po dokončení úlohy zálohování hello hello pracovního umístění je odstraněn. Hello cena úložiště použitého pro hello pracovního umístění je konzistentní s [Premium storage – ceny](../storage/common/storage-premium-storage.md#pricing-and-billing).

> [!NOTE]
> Změnit nebo upravit hello pracovního umístění.
>
>

### <a name="restore-premium-storage-vms"></a>Obnovení virtuálních počítačů služby Storage úrovně Premium
Virtuálních počítačů služby Storage úrovně Premium se dá obnovené tooeither Storage úrovně Premium nebo toonormal úložiště. Obnovení virtuálního počítače služby Storage úrovně Premium obnovení bodu back tooPremium úložiště je hello typickým procesem obnovení. Však může být toorestore nákladově efektivní úložiště toostandard bodu obnovení virtuálního počítače služby Storage úrovně Premium. Tento typ obnovení lze použít, pokud potřebujete podmnožinu souborů z hello virtuálních počítačů.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Používání virtuálních počítačů se spravovanými disky se službou Azure Backup
Azure Backup chrání virtuální počítače se spravovanými disky. Díky zpravovaným diskům už nemusíte spravovat účty úložiště virtuálních počítačů a zřizování virtuálních počítačů je výrazně zjednodušené.

### <a name="back-up-managed-disk-vms"></a>Zálohování virtuálních počítačů se spravovanými disky
Zálohování virtuálních počítačů na spravovaných discích se nijak neliší od zálohování virtuálních počítačů vytvořených pomocí Resource Manageru. V hello portálu Azure můžete nakonfigurovat úlohu zálohování hello přímo z hello zobrazení virtuálního počítače nebo z služeb zotavení hello trezoru zobrazení. Virtuální počítače na spravovaných discích můžete zálohovat prostřednictvím kolekcí RestorePoint postavených na spravovaných discích. Azure Backup podporuje také zálohování virtuálních počítačů se spravovanými disky, které jsou šifrované pomocí služby Azure Disk Encryption (ADE).

### <a name="restore-managed-disk-vms"></a>Obnovení virtuálních počítačů se spravovanými disky
Zálohování Azure vám umožní toorestore kompletní virtuální počítač s spravované disky nebo obnovení spravovaný účet úložiště tooa disky. Během procesu obnovení hello spravuje Azure hello spravované disky. (Hello zákazníka) spravujete hello účtu úložiště vytvořeném v rámci procesu obnovení hello. Při obnovování spravovaných šifrované virtuálních počítačů, hello klíče a tajné klíče Virtuálního počítače by měla existovat v operaci obnovení hello trezoru klíčů předchozí toostarting hello.

## <a name="what-are-hello-features-of-each-backup-component"></a>Jaké jsou funkce hello jednotlivých součástí zálohování?
Hello následující oddíly poskytují tabulky, které shrnují hello dostupnosti nebo podporu různých funkcí v jednotlivých součástí Azure Backup. Zobrazit informace hello následující každá tabulka pro další podporu nebo podrobnosti.

### <a name="storage"></a>Úložiště
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Trezor služby Recovery Services |![Ano][green] |![Ano][green] |![Ano][green] |![Ano][green] |
| Diskové úložiště | |![Ano][green] |![Ano][green] | |
| Páskové úložiště | |![Ano][green] | | |
| Komprese <br/>(v trezoru služby Recovery Services) |![Ano][green] |![Ano][green] |![Ano][green] | |
| Přírůstkové zálohování |![Ano][green] |![Ano][green] |![Ano][green] |![Ano][green] |
| Odstranění duplicit disku | |![Částečně][yellow] |![Částečně][yellow] | | |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key.png)

Trezor služeb zotavení Hello je hello upřednostňovaným cílem úložiště napříč všemi komponentami. System Center DPM a Azure Backup Server také poskytují toohave hello možnost kopie místního disku. Pouze System Center DPM však nabízí hello možnost toowrite data tooa páskové úložiště zařízení.

#### <a name="compression"></a>Komprese
Díky komprimování záloh dochází tooreduce hello požadovaný prostor úložiště. Hello jedinou komponentou, která nepoužívá komprimaci je hello rozšíření virtuálního počítače. hello Hello virtuálního počítače rozšíření kopií, které veškerá zálohovaná data z vaší toohello účet úložiště služby zotavení trezoru ve stejné oblasti. Komprese při přenosu dat hello. Přenášení dat hello bez komprese mírně zvýšení kapacity úložiště hello používá. Ukládání dat hello bez komprese umožňuje rychlejší obnovení, by však tento bod obnovení.


#### <a name="disk-deduplication"></a>Odstranění duplicit disku
Výhody odstranění duplicit můžete využívat při nasazení aplikace System Center DPM nebo Azure Backup Serveru [na virtuálním počítači Hyper-V](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server provede odstranění duplicitních dat (na úrovni hostitele hello) na virtuální pevné disky (VHD), které jsou připojené toohello virtuálního počítače jako úložiště pro zálohy.

> [!NOTE]
> Odstranění duplicit není v Azure k dispozici pro žádné komponenty služby Backup. Pokud System Center DPM a Backup Server nasazené v Azure, hello úložiště disků připojených toohello, které virtuálních počítačů, nelze provést odstranění duplicit.
>
>

### <a name="incremental-backup-explained"></a>Vysvětlení přírůstkového zálohování
Každý Azure Backup komponenty podporují přírůstkové zálohování bez ohledu na to hello cílového úložiště (disk, páska, trezor služeb zotavení). Přírůstkové zálohování zajišťuje, aby zálohování úložiště a času efektivní, podle přenášení pouze změn od poslední zálohy hello.

#### <a name="comparing-full-differential-and-incremental-backup"></a>Porovnání úplného, rozdílového a přírůstkového zálohování

Využití úložiště, plánovaná doba obnovení (RTO) a využití sítě se liší u každého způsobu zálohování. tookeep hello zálohování celkové náklady na vlastnictví (TCO) dolů, budete potřebovat toounderstand jak toochoose hello nejlepší řešení zálohování. Následující obrázek Hello porovná úplné zálohy, rozdílové zálohy a přírůstkové zálohování. Zdroj dat, který se skládá A 10 úložiště bloků hello obrázku A1-A10, jsou zálohovány měsíční. Bloky A2, A3, A4 a A9 změnit v hello první měsíc a blokovat A5 změny v hello příští měsíc.

![obrázek ukazující porovnání způsobů zálohování](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

S **úplné zálohování**, každý záložní kopie obsahuje hello celého datového zdroje. Úplné zálohování při každém přenosu záložní kopie spotřebovává velkou část úložiště a šířky pásma sítě.

**Rozdílovou zálohu** ukládá pouze hello blokuje, který změnil od hello počáteční úplné zálohování, což vede k menší množství využívání sítě a úložiště. Rozdílové zálohování neuchovává redundantní kopie nezměněných dat. Ale protože hello datových bloků, které zůstanou nezměněny mezi následné zálohy budou přeneseny a uložené, rozdílové zálohy jsou neefektivní. V hello druhého měsíce změněné bloky A2, A3, A4 a A9 jsou zálohovány. V hello třetí měsíc, tyto stejné bloky jsou zálohovány znovu, společně s změněné bloku A5. Hello změněné bloky pokračovat toobe zálohovat, dokud se stane hello další úplné zálohování.

**Přírůstkové zálohování** dosahuje vysoké účinnost sítě a úložiště ukládání pouze hello bloků dat, který změnil od hello předchozí zálohy. S přírůstkové zálohování není nutné tootake pravidelné úplné zálohování. V příkladu hello po hello úplné zálohování je prováděné na hello první měsíc změněné bloky A2, A3, A4 a A9 jsou označeny jako změnit a přenosu pro hello druhého měsíce. V hello třetí měsíc změnit jenom bloku A5 je označena a přenáší. Přenos menšího objemu dat šetří prostředky úložiště a sítě a snižuje tak celkové náklady na vlastnictví.   

### <a name="security"></a>Zabezpečení
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Zabezpečení sítě<br/> (tooAzure) |![Ano][green] |![Ano][green] |![Ano][green] |![Částečně][yellow] |
| Zabezpečení dat<br/> (v Azure) |![Ano][green] |![Ano][green] |![Ano][green] |![Částečně][yellow] |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Zabezpečení sítě
Veškerý provoz zálohování z vašeho toohello servery, které trezor služeb zotavení se šifruje pomocí Advanced Encryption Standard 256. Hello zálohovaná data se odesílají přes zabezpečené spojení HTTPS. zálohovaná data Hello je také uloženo v trezoru služeb zotavení hello v šifrovaném tvaru. Pouze, hello Azure zákazníků, máte hello přístupové heslo toounlock tato data. Microsoft nemůže dešifrovat hello zálohování dat v libovolném bodě.

> [!WARNING]
> Po vytvoření trezoru služeb zotavení hello pouze máte přístup toohello šifrovací klíč. Společnost Microsoft nikdy udržuje kopii šifrovacího klíče a nemá přístup k toohello klíč. Pokud dojde ke ztrátě klíče hello, Microsoft nemůže obnovit zálohovaná data hello.
>
>

#### <a name="data-security"></a>Zabezpečení dat
Zálohování virtuálních počítačů Azure vyžaduje nastavení šifrování *v rámci* hello virtuálního počítače. Na virtuálních počítačích se systémem Windows použijte BitLocker a na virtuálních počítačích s Linuxem použijte **dm-crypt**. Azure Backup neprovádí automatické šifrování zálohovaných dat, která přicházejí touto cestou.

### <a name="network"></a>Síť
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Komprese sítě <br/>(příliš**zálohování serveru**) | |![Ano][green] |![Ano][green] | |
| Komprese sítě <br/>(příliš**trezor služeb zotavení**) |![Ano][green] |![Ano][green] |![Ano][green] | |
| Síťový protokol <br/>(příliš**zálohování serveru**) | |TCP |TCP | |
| Síťový protokol <br/>(příliš**trezor služeb zotavení**) |HTTPS |HTTPS |HTTPS |HTTPS |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key-2.png)

Hello rozšíření virtuálního počítače (v hello virtuálních počítačů IaaS) čte hello data přímo z hello účtu úložiště Azure přes síť úložiště hello, takže není nutné toocompress tento provoz.

Pokud používáte System Center DPM serveru nebo Azure Backup Server jako sekundární server zálohování, komprese dat hello přecházející z hello primární server toohello zálohování serveru. Komprese dat před zahájením zálohování tooDPM nebo serveru Azure Backup, šetří šířku pásma.

#### <a name="network-throttling"></a>Omezování šířky pásma sítě
agent Azure Backup Hello nabízí omezení šířky pásma sítě, což umožňuje použití šířky pásma sítě při přenosu dat toocontrol. Omezování může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz. Omezení pro data přenos platí tooback zálohu a obnovte aktivity.

## <a name="backup-and-retention"></a>Zálohování a uchovávání

Azure Backup má limit 9999 bodů obnovení (označovaných také jako záložní kopie nebo snímky) na jednu *chráněnou instanci*. Chráněné instance je počítač, server (fyzických nebo virtuálních) nebo úlohy, které jsou nakonfigurované tooback až data tooAzure. Další informace najdete v tématu část hello [co je chráněný instance](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Instance je chráněná, jakmile se uloží záložní kopie dat. Hello záložní kopii dat je ochrana hello. Pokud hello zdroje dat došlo ke ztrátě nebo jsou poškozená, může obnovit záložní kopie hello hello zdrojová data. Hello následující tabulka uvádí hello maximální četnost záloh pro každou součást. Konfiguraci zásady zálohování Určuje, jak rychle využívat hello body obnovení. Pokud například vytváříte bod obnovení každý den, můžete zachovat body obnovení 27 let, teprve potom vám dojdou. Pokud pořídíte měsíčního bodu obnovení, je 833 let před spuštěním zachovat body obnovení. Hello služby Backup není nastaven vypršení časového limitu pro bod obnovení.

|  | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Frekvence zálohování<br/> (trezor služeb tooRecovery) |Tři zálohy za den |Dvě zálohy za den |Dvě zálohy za den |Jedna záloha za den |
| Frekvence zálohování<br/> (toodisk) |Neuvedeno |<li>Každých 15 minut pro SQL Server <li>Každou hodinu pro ostatní úlohy |<li>Každých 15 minut pro SQL Server <li>Každou hodinu pro ostatní úlohy</p> |Neuvedeno |
| Možnosti uchovávání |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |
| Maximální počet bodů obnovení na chráněnou instanci |9999|9999|9999|9999|
| Maximální doba uchovávání |Závisí na četnosti zálohování |Závisí na četnosti zálohování |Závisí na četnosti zálohování |Závisí na četnosti zálohování |
| Body obnovení na místním disku |Neuvedeno |<li>64 pro souborové servery,<li>448 pro aplikační servery |<li>64 pro souborové servery,<li>448 pro aplikační servery |Neuvedeno |
| Body obnovení na pásku |Neuvedeno |Unlimited |Neuvedeno |Neuvedeno |

## <a name="what-is-a-protected-instance"></a>Co je chráněná instance
Chráněné instance je Windows tooa Obecné referenční počítač, server (fyzických nebo virtuálních) nebo databázi SQL, který byl nakonfigurovaný tooback až tooAzure. Instance je chráněn po konfiguraci zásady zálohování pro hello počítače, server nebo databáze a vytvořit si záložní kopii dat hello. Další kopie hello záložní data pro dané chráněné instance (která se volá bodů obnovení) zvýšit množství hello úložiště využívat. Můžete vytvořit až too9999 body obnovení pro chráněné instance. Pokud odstraníte bod obnovení z úložiště, nepočítá proti celkový počet bodů obnovení 9999 hello.
Několik běžných příkladů chráněné instance jsou virtuální počítače, aplikační servery, databáze a osobní počítače s operačním systémem Windows hello. Například:

* Virtuální počítač spuštěný hello Hyper-V nebo Azure IaaS hypervisoru prostředků infrastruktury. Hello hostované operační systémy pro hello virtuální počítač může být Windows Server nebo Linux.
* Aplikační server: hello aplikační server může být fyzický nebo virtuální počítač se systémem Windows Server a úlohy s daty, která potřebuje toobe zálohovat. Běžné úlohy jsou Microsoft SQL Server, Microsoft Exchange server, Microsoft SharePoint server a hello role souborového serveru v systému Windows Server. tooback až tyto úlohy je třeba System Center Data Protection Manager (DPM) nebo serveru Azure Backup.
* Osobní počítače, pracovní stanice nebo přenosný počítač s operačním systémem Windows hello.


## <a name="what-is-a-recovery-services-vault"></a>Co je trezor služby Recovery Services?
Trezor služeb zotavení je že entita online úložiště v Azure používá toohold data, jako jsou záložní kopie, body obnovení a zásady zálohování. Zálohování dat pro toohold trezory služeb zotavení můžete použít pro Azure služeb a místními servery a pracovní stanice. Trezory služeb zotavení bylo snadné tooorganize zálohovaných dat, a současně minimalizujete její režie na správu. V rámci předplatného můžete podle potřeby vytvořit libovolný počet trezorů služby Recovery Services.

Záloh, které jsou založeny na portálu Azure Service Manager, byly hello první verzi hello trezoru. Trezory služeb zotavení, která přidávají funkce modelu hello Azure Resource Manager, jsou hello druhá verze hello trezoru. V tématu hello [článek s přehledem trezoru služeb zotavení](backup-azure-recovery-services-vault-overview.md) úplný popis rozdíly ve funkcích hello. Jste již nelze vytvářet použití hello portálu toocreate trezory Backup, ale trezory Backup jsou stále podporovány.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> **15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell. <br/> **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Čím se liší Azure Backup od Azure Site Recovery?
Služby Azure Backup a Azure Site Recovery spolu souvisí v tom smyslu, že obě služby zálohují data a můžou tato data obnovit. Tyto služby však v podniku slouží k jiným účelům, co se týče zajištění kontinuity podnikových procesů a zotavení po havárii. Pomocí Azure Backup tooprotect a obnovení dat na podrobnější úrovni. Například prezentace na přenosný počítač poškozením, byste použili prezentace hello toorestore Azure Backup. Pokud byste chtěli tooreplicate hello konfigurace a data na virtuálním počítači v jiném datovém centru, pomocí Azure Site Recovery.

Zálohování Azure chrání data místně a v cloudu hello. Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení mezi virtuálním počítačem a fyzickým serverem. Obě služby jsou důležité, protože vaše řešení zotavení po havárii vyžaduje tookeep vaše data zabezpečená a obnovitelná (služba Backup) *a* zachovat vaše úlohy byly dostupné (služba Site Recovery), když dojde k výpadku.

Hello následující koncepty vám může pomoct rozhodování ohledně zálohování a zotavení po havárii.

| Koncept | Podrobnosti | Zálohování | Zotavení po havárii (DR) |
| --- | --- | --- | --- |
| Cíl bodu obnovení (RPO) |Hello množství ztráty přijatelné dat v případě potřeby provedení obnovení toobe Hotovo. |Řešení pro zálohování jsou velmi variabilní ohledně přijatelného RPO. Zálohy virtuálních počítačů obvykle mají RPO jeden den, zatímco zálohy databází mají RPO nižší, až 15 minut. |Řešení zotavení po havárii mají nízké RPO. Hello kopie DR může být za pár sekund nebo několik minut. |
| Plánovaná doba obnovení (RTO) |Hello množství času, který přebírá toocomplete, obnovení nebo obnovení. |Z důvodu hello vyššího RPO hello množství dat, musí řešení zálohování tooprocess je typicky mnohem vyšší, což vede toolonger RTO. Například může trvat i dny toorestore data z pásky, v závislosti na hello čas potřebný tootransport hello pásku z odlehlého umístění. |Řešení zotavení po havárii mají nižší RTO, protože jsou více synchronizované se zdrojem hello. Méně změn potřebovat toobe zpracovat. |
| Uchovávání |Jak dlouho je toobe uložená data |Pro scénáře, které vyžadují provozní obnovení (poškození dat, neúmyslné odstranění souborů, selhání operačního systému), se zálohovaná data obvykle uchovávají po dobu 30 dnů nebo méně.<br>Z hlediska dodržování předpisů dat může být nutné toobe uložené pro měsíců nebo i let. V takovém případě jsou pro archivaci ideální zálohovaná data. |Zotavení po havárii vyžaduje pouze provozní data obnovení, která obvykle zabírají několik hodin nebo až den tooa. Z důvodu hello odstupňovaných dat zaznamenávání v řešeních zotavení po Havárii se nedoporučuje používat dat zotavení po Havárii pro dlouhodobé uchovávání. |

## <a name="next-steps"></a>Další kroky
Použijte jednu z hello následující kurzy pro podrobné a podrobné, pokyny pro ochranu dat v systému Windows Server nebo ochrany virtuálního počítače (VM) v Azure:

* [Zálohování souborů a složek](backup-try-azure-backup-in-10-mins.md)
* [Zálohování virtuálních počítačů Azure](backup-azure-vms-first-look-arm.md)

Podrobnosti o ochraně jiných úloh můžete zkusit najít v některém z těchto článků:

* [Zálohování Windows Serveru](backup-configure-vault.md)
* [Zálohování úloh aplikací](backup-azure-microsoft-azure-backup.md)
* [Zálohování virtuálních počítačů Azure IaaS](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
