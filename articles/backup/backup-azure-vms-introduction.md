---
title: "aaaPlanning infrastruktury zálohování virtuálních počítačů v Azure | Microsoft Docs"
description: "Je důležité zvážit při plánování tooback až virtuálních počítačů v Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů, zálohování virtuálních počítačů"
ms.assetid: 19d2cf82-1f60-43e1-b089-9238042887a9
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: d11982431610000293038ee6aa7df8e7bc2d8b70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Plánování infrastruktury zálohování virtuálních počítačů v Azure
Tento článek obsahuje výkonu a prostředků návrhy toohelp plánování vaší infrastruktury zálohování virtuálních počítačů. Definuje také klíčových aspektů služby zálohování hello; Tyto aspekty může být velmi důležité při určování vaší architektury plánování kapacity a plánování. Pokud jste [připravit vaše prostředí](backup-azure-vms-prepare.md), je dalším krokem hello plánování před zahájením [tooback až virtuálních počítačů](backup-azure-vms.md). Pokud potřebujete další informace o virtuálních počítačích Azure, najdete v části hello [virtuální počítače dokumentaci](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Jak funguje Azure zálohuje virtuální počítače?
V případě hello služba Azure Backup zahájí během hello naplánované úlohy zálohování, aktivuje hello rozšíření zálohování tootake snímku v daném okamžiku. Hello služby Azure Backup používá hello _VMSnapshot_ rozšíření v systému Windows a hello _VMSnapshotLinux_ rozšíření v systému Linux. rozšíření Hello je nainstalován během hello první zálohování virtuálních počítačů. rozšíření hello tooinstall, musí být spuštěna hello virtuálních počítačů. Pokud hello virtuální počítač není spuštěný, hello služby zálohování pořídí snímek hello základní úložiště (protože žádné zápisy aplikace dochází při hello zastavení virtuálního počítače).

Při pořizování snímku virtuálních počítačů Windows, služba Backup hello koordinuje pomocí služby Stínová kopie svazku (VSS) tooget hello konzistentního snímku disky hello virtuálního počítače. Pokud zálohujete virtuální počítače s Linuxem, můžete napsat vlastní vlastní skripty tooensure konzistence při pořizování snímku virtuálního počítače. Podrobnosti o vyvolání tyto skripty jsou uvedeny dále v tomto článku.

Jakmile hello služby Azure Backup používá hello snímku, je hello data přenášená toohello trezoru. toomaximize efektivitu, služba hello identifikuje a přenáší pouze hello bloky dat, které se změnily od hello předchozí zálohy.

![Architektura zálohování virtuálního počítače Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Po dokončení přenosu dat hello hello snímek odebrán a vytvoří bod obnovení.

> [!NOTE]
> 1. Během procesu zálohování hello Azure Backup neobsahuje hello dočasné disk připojený toohello virtuálního počítače. Další informace najdete v tématu hello blog na [dočasné úložiště](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Vzhledem k tomu, že Azure Backup používá úroveň úložiště snímků a přenosy, které snímku toovault, neměňte hello klíče účtu úložiště, až do dokončení úlohy zálohování hello.
> 3. U virtuálních počítačů premium jsme zkopírujte hello snímku toostorage účtu. Toto je toomake jistotu, že služba Azure Backup získá dostatečná IOPS pro přenos dat toovault. Tato další kopie úložiště je účtovat podle hello přidělené velikost virtuálního počítače. 
>

### <a name="data-consistency"></a>Konzistence dat
Zálohování a obnovení důležitá obchodní data ztěžuje hello skutečnost, že při hello aplikace, které vytvářejí hello dat běží, je třeba zálohovat důležitá obchodní data. tooaddress se Azure Backup podporuje zálohování konzistentní s aplikací pro Windows a virtuální počítače s Linuxem
#### <a name="windows-vm"></a>Virtuální počítač s Windows
Zálohování Azure trvá úplné zálohování VSS na virtuálních počítačích systému Windows (Další informace o [úplné zálohování VSS](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). tooenable kopie zálohy VSS, hello následující sadě toobe potřebám klíče registru na hello virtuálních počítačů.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Virtuální počítače s Linuxem
Azure Backup poskytuje skriptovací rozhraní. konzistence aplikací tooensure při zálohování virtuálních počítačů Linux, vytvořte vlastní skripty před a po skripty, které řídí hello zálohování workflow a prostředí. Zálohování Azure vyvolá hello předběžné skript před přepnutím hello snímku virtuálního počítače a vyvolá hello po skript po dokončení hello úlohu snímku virtuálního počítače. Další podrobnosti najdete v tématu [konzistentní zálohování virtuálních počítačů aplikace pomocí skriptů před a po skript](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Zálohování Azure pouze vyvolá hello zapsána zákazníka před a po skripty. Pokud skript hello před a po skripty spustí úspěšně, Azure Backup označí bod obnovení hello jako aplikace konzistentní. Hello zákazníka je však nakonec odpovědné za konzistence aplikace hello při použití vlastních skriptů.
>


Tato tabulka vysvětluje hello typy konzistence a hello podmínky, že k nim dojde v části během zálohování virtuálních počítačů Azure a obnovit postupy.

| Konzistence | Na základě služby Stínová kopie svazku | Vysvětlení a podrobnosti |
| --- | --- | --- |
| Konzistentnost aplikací |Ano pro Windows|Aplikace konzistence je ideální pro úlohy, protože zajišťuje, že:<ol><li> Hello virtuálních počítačů *spustí*. <li>Je *žádné poškození*. <li>Je *nedošlo ke ztrátě dat*.<li> Hello data je konzistentní toohello aplikace, která používá hello data podle zahrnující aplikace hello během hello zálohy – pomocí služby Stínová kopie svazku nebo předzálohovacího nebo pozálohovacího skriptu.</ol> <li>*Virtuální počítače Windows*-úlohy nejvíce Microsoft mají zapisovače služby Stínová kopie svazku, které provádějí akce určeného pro konkrétní úlohy související toodata konzistence. Například Microsoft SQL Server obsahuje zapisovač VSS, které zajišťuje, aby správně provést hello zápisy toohello transakční protokol a databáze hello. Pro zálohování virtuálního počítače Windows Azure, toocreate bod obnovení konzistentních s aplikací musí rozšíření zálohování hello vyvolání hello VSS pracovního postupu a dokončete ji před přepnutím hello snímku virtuálního počítače. Pro toobe snímku hello v virtuálního počítače Azure přesné musíte dokončit také zapisovače VSS hello všech aplikací virtuálního počítače Azure. (Další hello [Základy služby Stínová kopie svazku](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) a podrobné informace o hello podrobnosti o [jak to funguje](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Virtuální počítače s Linuxem*– zákazníci mohou spouštět [konzistence aplikací vlastní skript před a po skript tooensure](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Konzistence systému souborů |Ano – pro počítače se systémem Windows |Existují dva scénáře, kde může být bod obnovení hello *systému souborů konzistentní*:<ul><li>Zálohování virtuálních počítačů Linux v Azure, bez vedlejší script nebo po script nebo pokud vedlejší script nebo po script se nezdařilo. <li>Chyba služby Stínová kopie svazku během zálohování pro virtuální počítače Windows v Azure.</li></ul> V obou těchto případech je hello nejlepší, kterou lze provést tooensure který: <ol><li> Hello virtuálních počítačů *spustí*. <li>Je *žádné poškození*.<li>Je *nedošlo ke ztrátě dat*.</ol> Aplikace potřebují mechanismus tooimplement vlastní "oprava up" hello obnovit data. |
| Konzistence havárií |Ne |Tato situace je ekvivalentní tooa virtuální počítač má "selhání" (prostřednictvím buď logicky nebo pevné resetování). Konzistence havárií obvykle dojde, když v době zálohování hello je vypnutý hello virtuální počítač Azure. Bod obnovení konzistentní při selhání obsahuje žádné záruky ohledně hello konzistence dat hello střední hello úložiště – buď z hlediska hello hello operačního systému nebo aplikace hello. Jediná hello data, která již existuje na disku hello v době zálohování hello je zaznamenat a zálohovat. <br/> <br/> Když dochází obvykle žádné záruky, hello jednotlivými spuštěními operačního systému, za nímž následuje kontrolu disku postupu, jako je nástroj chkdsk, toofix všechny chyby poškození. Data v paměti ani zápisů, které nebyly přenášená toohello disku se ztratí. aplikace Hello se obvykle následuje s vlastní mechanismus ověřování v případě, že data vrácení musí toobe Hotovo. <br><br>Například pokud hello transakční protokol obsahuje položky, které se nenacházejí v databázi hello, pak hello databázový software nepodporuje vrácení zpět. dokud hello dat je konzistentní. Když dat je rozdělena mezi více virtuálních disků (např. rozložené svazky), poskytuje bod konzistentní při selhání obnovení žádné záruky hello správnost údajů hello. |

## <a name="performance-and-resource-utilization"></a>Výkon a využití prostředků
Jako zálohovací software, který je nasazený na místě měli byste naplánovat kapacitu a využití prostředků musí při zálohování virtuálních počítačů v Azure. Hello [Azure Storage omezuje](../azure-subscription-service-limits.md#storage-limits) definovat, jak toostructure virtuálních počítačů nasazení tooget maximální výkon s minimální vliv toorunning úlohy.

Věnovat pozornost toohello, který omezuje následující Azure Storage při plánování výkonu zálohování:

* Maximální počet odchozí na účet úložiště
* Rychlost celkový počet požadavků na účet úložiště

### <a name="storage-account-limits"></a>Limity účtu úložiště
Zálohovaná data zkopírovat z účtu úložiště, přidá toohello vstupně výstupních operací za sekundu (IOPS) a odchozí (nebo propustnost) metriky hello účtu úložiště. Na hello stejný čas, virtuální počítače spotřebovávají také IOPS a propustnosti. cílem Hello je tooensure zálohování a přenosy dat virtuálních počítačů nepřekračují omezení účtu úložiště.

### <a name="number-of-disks"></a>Počet disků
proces zálohování Hello pokusí provést co nejrychleji toocomplete úlohu zálohování. Při tom spotřebuje množství prostředků, jak je to možné. Však mají omezenou všechny vstupně-výstupních operací hello *propustnost cíl pro jeden objekt Blob*, což může mít 60 MB za sekundu. V toomaximize pokusu o jeho rychlost hello proces zálohování se pokusí tooback až všechny disky Virtuálního počítače hello *paralelně*. Pokud virtuální počítač má čtyři disky, služba hello pokusí o tooback všechny čtyři disky paralelně. Hello **počet disků** zálohovaných, je hello nejdůležitějším faktorem při určování provoz zálohování účet úložiště.

### <a name="backup-schedule"></a>Plán zálohování
Dodatečný faktor, který má dopad na výkon je hello **plán zálohování**. Pokud nakonfigurujete hello zásady tak, že všechny virtuální počítače jsou zálohovány v hello stejný čas, jste naplánovali papír provoz. proces zálohování Hello pokusí tooback všechny disky paralelně. provoz zálohování tooreduce hello z účtu úložiště, zálohování různých virtuálních počítačů v jiné době hello den s bez překrytí.

## <a name="capacity-planning"></a>Plánování kapacity
Předchozí faktory hello uvedení společně, potřebujete tooplan pro potřeby využití účtu úložiště hello. Stáhnout hello [Excelová tabulka pro plánování kapacity služby zálohování virtuálních počítačů](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) toosee hello dopad disku a plán zálohování volby.

### <a name="backup-throughput"></a>Zálohování propustnost
Pro každý disk zálohovaných Azure Backup přečte hello bloky na disku hello a ukládá jenom hello změněná data (přírůstkové zálohování). Hello následující tabulka uvádí hello průměrné zálohování služby propustnost hodnoty. Pomocí hello následující data, můžete odhadnout, že hello množství času potřebné tooback až disk na danou velikost.

| Operace zálohování | Nejlepší možný propustnost |
| --- | --- |
| Prvotní zálohování |160 MB/s |
| Přírůstkové zálohování (DR) |640 MB/s <br><br> Propustnost vyřazuje výrazně změnu hello data (vyžadující toobe zálohovat) jsou rozptýlená v rámci hello disku.|

## <a name="total-vm-backup-time"></a>Celkový čas zálohování virtuálních počítačů
Při čtení a kopírování dat, je většina čas zálohování hello stráví, jiné operace přispívat toohello celkový čas potřebný tooback si virtuální počítač:

* Doba potřebná příliš[instalovat nebo aktualizovat rozšíření zálohování hello](backup-azure-vms.md).
* Čas snímek, což je hello doba tootrigger snímku. Snímky jsou spouštěná zavřít toohello naplánovaný čas zálohování.
* Doba čekání ve frontě. Vzhledem k tomu, že služba zálohování hello zpracovává záloh z více zákazníků, kopírování zálohovaná data ze zálohy snímku toohello nebo služeb zotavení trezoru nemusí spustit okamžitě. Časů zátěž ve špičce lze hello čekání roztáhnout tooeight hodin z důvodu toohello počet záloh zpracovává. Hello celkový počet virtuálních počítačů čas zálohování je však méně než 24 hodin pro denní zásady zálohování.
* Doba přenosu dat, čas potřebný pro zálohování služby toocompute hello přírůstkové změny z předchozí zálohy a přenos tyto změny toovault úložiště.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Proč se sledování longer(>12 hours) zálohování čas?
Zálohování se skládá ze dvou fází: pořizování snímků a přenosu hello snímky toohello trezoru. Hello služby zálohování optimalizuje úložiště. Při přenosu hello snímek dat tooa trezoru, převede služba hello z předchozího snímku hello pouze přírůstkové změny.  přírůstkové změny hello toodetermine hello služby vypočítá kontrolního součtu hello hello bloků. Pokud dojde ke změně blok, hello bloku je označený bloku toobe, odeslané toohello trezoru. Cvičení služby hello další do každé hello identifikaci bloky, hledá tootransfer data hello toominimize příležitosti. Po vyhodnocení všechny změněné bloky, služba hello spojí hello změny a odešle je toohello trezoru. V některých starších verzí aplikací nejsou malé, fragmentovaných zápisy optimální pro úložiště. Pokud snímek hello obsahuje mnoho malých, fragmentovaných zápisy, služba hello stráví další čas zpracování dat hello zapsaných aplikace hello. Hello doporučená bloku zápisu aplikace ze služby Azure, pro aplikace spuštěné v hello virtuálních počítačů, je minimálně 8 KB. Pokud vaše aplikace používá blok menší než 8 KB, se provádí výkon zálohování. Pomoc s optimalizací výkonu zálohování tooimprove aplikací najdete v tématu [ladění aplikací pro optimální výkon s Azure storage](../storage/common/storage-premium-storage-performance.md). I když hello článek na výkon zálohování používá příklady úložiště Premium, je použít u standardního úložiště disky hello pokyny.

## <a name="total-restore-time"></a>Celkový počet obnovení čas
Operace obnovení se skládá z dvě hlavní dílčí úlohy: kopírování dat zpět z účtu úložiště hello trezoru toohello vybrali zákazníka a vytváření hello virtuálního počítače. Kopírování dat zpět z trezoru hello závisí na zálohování hello uložení interně v Azure a kde je uložen účet úložiště zákazníka hello. Doba trvání toocopy dat závisí na:
* Fronty čekací dobu – od zpracuje služba hello obnovení úlohy z víc zákazníků na hello současně, žádosti o obnovení jsou umístit do fronty.
* Kopírování dat time – Data budou zkopírována z účtu úložiště hello trezoru toohello zákazníka. Obnovení doba závisí na IOPS a propustnost služby Azure Backup získá hello vybraný účet úložiště zákazníka. tooreduce hello kopírování času během procesu obnovení hello, vyberte účet úložiště s jinými aplikace zápisů a čtení není načtená.

## <a name="best-practices"></a>Osvědčené postupy
Doporučujeme následující tyto postupy při konfiguraci zálohování pro virtuální počítače:

* Nemáte naplánovat víc než 10 klasické virtuální počítače z hello stejné cloudové služby tooback v hello stejnou dobu. Pokud chcete tooback se víc virtuálních počítačů z stejné cloudové služby, odstupňování časů zahájení zálohování hello o hodinu.
* Neplánujte víc než 40 tooback virtuální počítače až na hello stejnou dobu.
* Plánování zálohování virtuálních počítačů v době mimo špičku. Tento způsob hello služby Backup používá IOPS pro přenos dat z trezoru toohello hello zákazníka úložiště účtu.
* Ujistěte se, že je zásada na virtuálních počítačích rozloženy jiným účtům úložiště. Doporučujeme více než 20 celkový disky z jednoho úložiště účtu chráněn hello stejný plán zálohování. Pokud máte větší než 20 disky v účtu úložiště, šíří tyto virtuální počítače napříč více tooget zásady hello požadované IOPS během fáze přenos hello hello proces zálohování.
* Neobnovujte systémem prémiový účet úložiště toosame úložiště virtuálního počítače. Pokud proces operaci obnovení hello se shoduje s hello operaci zálohování, snižuje hello k dispozici IOPS pro zálohování.
* Pro zálohování virtuálních počítačů Premium Ujistěte se, tento účet úložiště, hostitelé prémiové disky má alespoň 50 % volného místa pro přípravu snímek pro úspěšnou zálohu. 
* Ujistěte se, že danou verzi jazyka python na virtuální počítače s Linuxem povolené pro zálohování je 2.7

## <a name="data-encryption"></a>Šifrování dat
Zálohování Azure data jako součást procesu zálohování hello nešifruje. Ale můžete šifrovat data v rámci hello virtuálních počítačů a zálohování hello chráněných dat bezproblémově (Další informace o [zálohování šifrovaná data](backup-azure-vms-encryption.md)).

## <a name="calculating-hello-cost-of-protected-instances"></a>Výpočet nákladů hello chráněné instance
Azure virtuální počítače, které jsou zálohovány pomocí Azure Backup jsou příliš předmět[cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/). Hello chráněné instance výpočtu je založena na hello *skutečné* velikost hello virtuální počítač, který je součtem hello všechna data hello ve virtuálním počítači hello – s výjimkou hello "prostředků disku."

Ceny pro zálohování virtuálních počítačů je *není* na základě velikosti hello maximální podporované pro každý virtuální počítač datového disku připojené toohello. Ceny vychází hello dat uložených v hello datový disk. Podobně faktury hello úložiště záloh podle hello množství dat, který je uložený ve službě Azure Backup, tedy součet hello hello skutečných dat v každém bodu obnovení.

Například využít A2 standardní velikosti virtuálního počítače, který má dva dalších datových disků s maximální velikost 1 TB. Hello následující tabulka poskytuje hello dat uložených na každý z těchto disků:

| Typ disku | Maximální velikost | Skutečná data obsažená |
| --- | --- | --- |
| Disk operačním systému |1023 GB |17 GB |
| Místní disk nebo prostředek disku |135 GB |5 GB (pro zálohování není součástí) |
| Datový disk 1 |1023 GB |30 GB |
| Datový disk 2 |1023 GB |0 GB |

Hello *skutečné* velikost hello virtuálního počítače v tomto případě je 17 GB + 30 GB + 0 GB = 47 GB. Tato velikost chráněné Instance (47 GB) se změní na hello základ pro hello měsíčních nákladů. Jako hello roste množství dat v hello virtuálního počítače, velikost chráněné Instance hello použitý pro změny fakturace odpovídajícím způsobem.

Fakturace se nespustí, dokud se nedokončí hello první úspěšné zálohy. V tomto okamžiku začne hello fakturace pro úložiště a chráněné instance. Fakturace pokračuje, dokud je *žádné zálohování dat uložených v trezoru* hello virtuálního počítače. Pokud zastavíte ochranu na virtuálním počítači hello, ale v trezoru zálohování dat virtuálního počítače existuje, bude pokračovat fakturace.

Fakturace zastaví zadaný virtuální počítač jenom v případě, že hello ochrana je zastavena *a* veškerá zálohovaná data se odstraní. Při zastavení ochrany a neexistují žádné aktivní úlohy zálohování, hello velikost zálohy virtuálního počítače poslední úspěšné hello změní velikost chráněné Instance hello používá pro hello měsíčních nákladů.

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
* [Zálohování virtuálních počítačů](backup-azure-vms.md)
* [Správa zálohování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-restore-vms.md)
* [Řešení problémů zálohování virtuálních počítačů](backup-azure-vms-troubleshoot.md)
