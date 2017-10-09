---
title: "aaaPerformance osvědčené postupy pro SQL Server v Azure | Microsoft Docs"
description: "Obsahuje osvědčené postupy pro optimalizaci výkonu serveru SQL Server ve virtuálních počítačích Microsoft Azure."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: jroth
ms.openlocfilehash: 42ec9fbeb2dec3a654b93bbd08d666369835ee73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Osvědčené postupy z hlediska výkonu pro SQL Server na Azure Virtual Machines

## <a name="overview"></a>Přehled

Toto téma obsahuje osvědčené postupy pro optimalizaci výkonu systému SQL Server na virtuálním počítači Microsoft Azure. Při spuštění systému SQL Server v Azure Virtual Machines, doporučujeme dál používat hello stejného výkonu databáze ladění možnosti, které jsou použitelné tooSQL Server v prostředí místní server. Hello výkonu relační databáze ve veřejném cloudu však závisí na mnoha faktorech, jako je například hello velikost virtuálního počítače a konfigurace hello hello datových disků.

Při vytváření bitové kopie systému SQL Server, [zvažte zřizování virtuálních počítačů v hello portál Azure](virtual-machines-windows-portal-sql-server-provision.md). Zřízené v hello portálu s Resource Managerem virtuálním počítačům systému SQL Server implementovat všechny tyto osvědčené postupy, včetně konfigurace úložiště hello.

Tento článek se zaměřuje na získávání hello *nejlepší* výkonu pro SQL Server na virtuálních počítačích Azure. Pokud vaše úlohy je méně náročné, nemusejí být nutné každých optimalizace uvedené níže. Zvažte požadavkům na výkon a vzory úlohy, jak vyhodnotit tato doporučení.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Rychlá kontrola seznamu

Následující Hello je rychlá kontrola seznam pro optimální výkon systému SQL Server na virtuálních počítačích Azure:

| Oblast | Optimalizace |
| --- | --- |
| [Velikost virtuálního počítače](#vm-size-guidance) |[DS3](../../virtual-machines-windows-sizes-memory.md) nebo vyšší pro SQL Enterprise edition.<br/><br/>[DS2](../../virtual-machines-windows-sizes-memory.md) nebo vyšší verze SQL Standard a Web. |
| [Úložiště](#storage-guidance) |Použití [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md). Standardní úložiště se doporučuje jenom pro vývojové a testovací.<br/><br/>Zachovat hello [účet úložiště](../../../storage/common/storage-create-storage-account.md) a virtuální počítač SQL Server v hello stejné oblasti.<br/><br/>Zakázat Azure [geograficky redundantní úložiště](../../../storage/common/storage-redundancy.md) (geografická replikace) na účet úložiště hello. |
| [Disky](#disks-guidance) |Použití minimálně 2 [P30 disky](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) (1 pro soubory protokolů, 1 pro datové soubory a databáze TempDB).<br/><br/>Vyhněte se použití operačního systému nebo dočasné disků pro úložiště databáze nebo protokolování.<br/><br/>Povolte čtení ukládání do mezipaměti na disky hello hostování hello datové soubory a databáze TempDB.<br/><br/>Nepovolujte ukládání do mezipaměti na disky hostování hello souboru protokolu.<br/><br/>Důležité: Zastavení služby SQL Server hello při změně nastavení hello mezipaměti pro disk pro virtuální počítač Azure.<br/><br/>Prokládané více dat Azure disky tooget vyšší vstupně-výstupní operace propustnost.<br/><br/>Formát s velikostí zdokumentovaných přidělení. |
| [VSTUPNĚ-VÝSTUPNÍCH OPERACÍ](#io-guidance) |Povolte kompresi stránky databáze.<br/><br/>Povolte rychlé soubor inicializace pro datové soubory.<br/><br/>Omezit nebo zakázat automatické zvětšování databáze hello.<br/><br/>Zakažte autoshrink v databázi hello.<br/><br/>Přesuňte všechny disky toodata databáze, včetně systémové databáze.<br/><br/>SQL Server chyba protokolu a trasování soubor adresáře toodata disky přesuňte.<br/><br/>Nastavte výchozí zálohování a databáze umístění souborů.<br/><br/>Povolte uzamčených stránek.<br/><br/>Použijte opravy výkonu systému SQL Server. |
| [Konkrétní funkce](#feature-specific-guidance) |Zálohujte přímo tooblob úložiště. |

Další informace o *jak* a *proč* toomake tyto optimalizace, přečtěte si podrobnosti hello a pokyny uvedené v následujících částech.

## <a name="vm-size-guidance"></a>Pokyny velikost virtuálního počítače

Výkon citlivých aplikací, je doporučeno používat následující hello [velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json):

* **SQL Server Enterprise Edition**: DS3 nebo vyšší
* **SQL Server Standard a Web edice**: DS2 nebo vyšší

## <a name="storage-guidance"></a>Pokynů pro virtualizované úložiště

Podpora virtuálních počítačů služby DS-series (spolu s DSv2-series a GS-series) [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md). Storage úrovně Premium se doporučuje pro všechny úlohy v produkčním prostředí.

> [!WARNING]
> Standardní úložiště má různou latenci a šířky pásma a doporučuje se pouze pro procesy vývoje/testování. Úlohy v produkčním prostředí by měl používat úložiště úrovně Premium.

Kromě toho doporučujeme vytvořit účet úložiště Azure v hello stejném datovém centru jako zpoždění přenosu tooreduce virtuální počítače vašeho systému SQL Server. Při vytváření účtu úložiště, zakážete geografická replikace není zaručena pořadí konzistentní zápisu na více disků. Místo toho zvažte nakonfigurování technologie obnovení po havárii systému SQL Server mezi dvěma datových center Azure. Další informace najdete v tématu [vysokou dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Disky pokyny

Existují tři typy hlavní disku na virtuální počítač Azure:

* **Disk s operačním systémem**: při vytváření virtuálního počítače Azure připojí hello platformy alespoň jeden disk (označený jako hello **C** jednotku) toohello virtuálních počítačů pro disk operačního systému. Tento disk je virtuální pevný disk uložený jako objekt blob stránky v úložišti.
* **Dočasným diskovým**: virtuální počítače Azure obsahovat jiné disku s názvem dočasným diskovým hello (označené jako hello **D**: jednotku). Toto je disku hello uzlu, který lze použít pro pomocné místo.
* **Datové disky**: další disky tooyour virtuálního počítače můžete také připojit jako datové disky, a ty budou uloženy v úložišti jako objekty BLOB stránky.

Hello následující části popisují doporučení pro používání těchto různých disků.

### <a name="operating-system-disk"></a>Disk operačním systému

Disk operačního systému je virtuální pevný disk, který můžete spustit a připojit jako spuštěné verze operačního systému a je označený jako **C** jednotky.

Ukládání do mezipaměti zásady na disk operačního systému hello výchozí hodnota je **pro čtení a zápis**. Pro citlivé aplikace výkonu doporučujeme používat datových disků místo hello disk operačního systému. O datových disků níže najdete v části hello.

### <a name="temporary-disk"></a>Dočasné disku

Hello dočasné úložiště disku, označený jako hello **D**: disk, není tooAzure trvalého úložiště objektů blob. Neukládejte databázové soubory uživatelů nebo soubory protokolu transakcí uživatele na hello **D**: jednotky.

D-series, Dv2-series a virtuální počítače G-series hello jednotku na těchto virtuálních počítačích je založená na SSD. Pokud vaše úlohy provede výrazně využívá databáze TempDB (např. pro dočasné objekty nebo komplexní spojení), ukládání databáze TempDB na hello **D** jednotky může mít za následek vyšší propustnost databáze TempDB a snížení latence databázi TempDB.

Pro virtuální počítače, které podporují službu Premium Storage (DS-series, DSv2-series a GS-series) doporučujeme uložit databázi TempDB na disk, který podporuje službu Premium Storage s čtení povoleno ukládání do mezipaměti. Existuje jedna výjimka toothis doporučení; Pokud vaše databáze TempDB využití je náročné na zápis, můžete dosáhnout vyšší výkon uložením databáze TempDB na místní hello **D** disku, který je taky založená na SSD na velikosti těchto počítačů.

### <a name="data-disks"></a>Datové disky

* **Použití datových disků pro soubory protokolu a data**: minimálně používat 2 úložiště úrovně Premium [P30 disky](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) kde jeden disk hello soubory protokolu a hello jiné obsahuje hello data a soubory databáze TempDB. Každý disk úložiště Premium poskytuje řadu IOPs a šířky pásma (MB/s) v závislosti na jeho velikosti, jak je popsáno v následující článek hello: [pomocí úložiště Premium pro disky](../../../storage/common/storage-premium-storage.md).

* **Disk proložení**: pro další propustnosti, můžete přidat další datové disky a používat prokládání disků. toodetermine hello počet datových disků, je nutné tooanalyze hello počtu IOPS a šířky pásma požadované pro soubory protokolu a pro data a soubory databáze TempDB. Všimněte si, že různé velikosti virtuálních počítačů mají různé limity hello počtu IOPs a šířky pásma, které jsou podporovány, naleznete v tématu hello tabulky na IOPS na [velikost virtuálního počítače](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Hello použijte následující pokyny:

  * Pro Windows 8 nebo Windows Server 2012 nebo novější, použijte [prostory úložiště](https://technet.microsoft.com/library/hh831739.aspx) s hello následující pokyny:

      1. Sada hello interleave (stripe velikost) too64 KB (65536 bajtů) pro zatížení OLTP a 256 KB (262 144 bajtů) pro datových skladů úlohy z důvodu chybné zarovnání toopartition tooavoid vlivu na výkon. Toto musí být nastavena v prostředí PowerShell.
      1. Nastavit počet sloupců = počet fyzických disků. Při konfiguraci maximálně 8 disky (ne uživatelského rozhraní správce serveru), pomocí prostředí PowerShell. 

    Například hello následující prostředí PowerShell vytvoří nový fond úložiště s hello interleave too64 velikost KB a hello číslo sloupce too2:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 nebo starší můžete použít dynamické disky (OS prokládané svazky) a velikost stripe hello je vždy 64 KB. Všimněte si, že tato možnost je zastaralé od verze Windows 8 nebo Windows Server 2012. Informace najdete v prohlášení o odborné pomoci hello v [virtuální disková služba je přechod tooWindows rozhraní API správy úložišť](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Pokud vaše úlohy není náročné protokol a nemusí vyhrazené IOPs, můžete nakonfigurovat pouze jeden fond úložiště. Vytvořte, jinak hodnota dvěma fondy úložiště, jeden pro soubory protokolu hello a jiným fondem úložiště pro soubory dat hello a databáze TempDB. Určete hello počet disků přidružených každý fond úložiště založený na vaše očekávání zatížení. Uvědomte si, že různé velikosti virtuálních počítačů povolit různý počet připojených datových disků. Další informace najdete v tématu [velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Pokud nepoužíváte Storage úrovně Premium (scénáře vývoje/testování), hello doporučení je tooadd hello maximální počet datových disků, které podporuje vaše [velikost virtuálního počítače](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a používat prokládání disků.

* **Ukládání do mezipaměti zásad**: datových disků pro Storage úrovně Premium, povolit čtení ukládání do mezipaměti na hostování datové soubory a databáze TempDB pouze hello datových disků. Pokud nepoužíváte Storage úrovně Premium, nepovolujte žádné ukládání do mezipaměti na všech datových disků. Pokyny ke konfiguraci disku ukládání do mezipaměti, naleznete v následujících tématech hello: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) a [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

  > [!WARNING]
  > Zastavení služby SQL Server hello při změně nastavení mezipaměti hello virtuálního počítače Azure disky tooavoid Ahoj možnost vzniku nedošlo k poškození databáze.

* **Velikost alokační jednotky systému souborů NTFS**: při formátování hello datový disk, je doporučeno používat velikost 64 KB alokační jednotky pro data a soubory protokolu, jakož i databáze TempDB.

* **Osvědčené postupy správy na disku**: Při odebrání datový disk nebo změna jeho mezipaměti typ, zastavit službu systému SQL Server hello během změny hello. Po nastavení ukládání do mezipaměti hello změny na disku hello operačního systému, Azure zastaví hello virtuálních počítačů, změní typ mezipaměti hello a restartuje hello virtuálních počítačů. Při změně nastavení mezipaměti hello datový disk, hello virtuálního počítače nebyla zastavena, ale datový disk hello odpojený od hello virtuálních počítačů během hello změnit a poté znovu připojit.

  > [!WARNING]
  > Selhání toostop hello služby SQL Server během těchto operací může způsobit poškození databáze.

## <a name="io-guidance"></a>Pokyny vstupně-výstupních operací

* Nejlepších výsledků dosáhnete Hello Storage úrovně Premium jsou dosáhnete, když paralelní aplikace a požadavky. Storage úrovně Premium je určen pro scénáře, kde je hloubka fronty vstupně-výstupní operace hello větší než 1, takže uvidíte žádné nebo téměř žádné zvýšení výkonu pro jednovláknové sériové požadavky (i když jsou náročné na úložiště). Například to by mohlo mít vliv výsledky testů jednovláknové hello nástrojů pro analýzu výkonu, jako je například SQLIO.

* Zvažte použití [databáze stránky komprese](https://msdn.microsoft.com/library/cc280449.aspx) jako může zvýšit výkon zatížení s intenzivním vstupně-výstupní operace. Komprese dat hello však může zvýšit hello spotřeby procesoru na serveru databáze hello.

* Zvažte povolení rychlých soubor inicializace tooreduce hello dobu nutnou pro přidělení počáteční souboru. výhody tootake rychlých soubor inicializace udělení účtu služby SQL Server (MSSQLSERVER) hello s SE_MANAGE_VOLUME_NAME a přidejte ho toohello **provádět úlohy údržby svazku** zásady zabezpečení. Pokud používáte image platformy SQL Server pro Azure, účet služby výchozí hello (NT Service\MSSQLSERVER) nebyla přidána toohello **provádět úlohy údržby svazku** zásady zabezpečení. Jinými slovy inicializace rychlých souboru není povoleno v imagi platformy SQL Server Azure. Po přidání toohello účtu služby SQL Server hello **provádět úlohy údržby svazku** zásady zabezpečení, restart služby systému SQL Server hello. Je možné, úvahy o zabezpečení pro použití této funkce. Další informace najdete v tématu [inicializace souboru databáze](https://msdn.microsoft.com/library/ms175935.aspx).

* **operace autogrow** se považuje za toobe jenom možnost pro neočekávané růstu. Řízení nebyla růstu protokolu a data na základě každodenní s automaticky zvětšovat. Pokud se používá k automatické zvětšování, předem růst souboru hello pomocí přepínače velikost hello.

* Zajistěte, aby **autoshrink** je zakázáno tooavoid nárokům, který může negativně ovlivnit výkon.

* Přesuňte všechny disky toodata databáze, včetně systémové databáze. Další informace najdete v tématu [přesunout systémové databáze](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server chyba protokolu a trasování soubor adresáře toodata disky přesuňte. To můžete provést v nástroji SQL Server Configuration Manager tak, že kliknete pravým tlačítkem na instanci serveru SQL a výběr vlastností. Hello nastavení souboru protokolu a trasování chyb lze změnit v hello **parametry spuštění** kartě hello výpis je uveden v hello **Upřesnit** kartě hello následující snímek obrazovky ukazuje, kde toolook pro parametr pro spuštění protokolu chyby Hello.

    ![Snímek obrazovky protokolu chyb SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Nastavte výchozí zálohování a databáze umístění souborů. Použít hello doporučení v tomto tématu a proveďte změny hello v okně vlastnosti Server hello. Pokyny najdete v tématu [hello zobrazení, nebo změňte výchozí umístění pro Data a soubory protokolů (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Hello následující snímek obrazovky ukazuje, kde toomake tyto změny.

    ![Soubory protokolu SQL dat a zálohování](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Povolit uzamčen stránky tooreduce vstupně-výstupní operace a všech aktivit stránkování. Další informace najdete v tématu [povolit hello Zamknout stránky možnosti paměti (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* Pokud používáte systém SQL Server 2012, nainstalujte Service Pack 1 kumulativní aktualizací 10. Tato aktualizace obsahuje hello oprava nízký výkon na vstupně-výstupní operace při spuštění vyberte Select dočasné tabulky v systému SQL Server 2012. Informace najdete v tématu to [článku znalostní báze](http://support.microsoft.com/kb/2958012).

* Vezměte v úvahu všechny datové soubory byla zahájena komprese při přenosu a konec Azure.

## <a name="feature-specific-guidance"></a>Konkrétní pokyny funkce

Některá nasazení může dosáhnout výhody další výkonu pomocí složitější postupy konfigurace. Hello následujícím seznamu jsou uvedeny některé funkce serveru SQL Server, které vám mohou pomoci tooachieve lepší výkon:

* **Zálohování úložiště tooAzure**: při provádění zálohování pro SQL Server běžící na virtuálních počítačích Azure, můžete použít [zálohování systému SQL Server tooURL](https://msdn.microsoft.com/library/dn435916.aspx). Tato funkce je k dispozici od verze SQL Server 2012 SP1 CU2 a doporučené pro zálohování datových disků toohello připojen. Pokud je zálohování a obnovení do nebo ze služby Azure storage, postupujte podle hello doporučení uvedených v [zálohování serveru SQL tooURL osvědčené postupy a řešení potíží a obnovení ze zálohy uložené ve službě Azure Storage](https://msdn.microsoft.com/library/jj919149.aspx). Můžete také automatizovat tyto zálohy pomocí [automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

    Předchozí tooSQL Server 2012, můžete použít [zálohování systému SQL Server tooAzure nástroj](https://www.microsoft.com/download/details.aspx?id=40740). Tento nástroj může pomoct zálohování propustnost tooincrease pomocí více cílů zálohování stripe.

* **Datové soubory SQL serveru v Azure**: Tato nová funkce [datových souborů SQL serveru v Azure](https://msdn.microsoft.com/library/dn385720.aspx), je k dispozici od verze SQL Server 2014. Ukazuje vlastnosti porovnatelný z hlediska výkonu jako pomocí Azure datových disků, systémem SQL Server s datovými soubory v Azure.

## <a name="next-steps"></a>Další kroky

Osvědčené postupy zabezpečení, najdete v části [důležité informace o zabezpečení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Přečtěte si další témata virtuálního počítače systému SQL Server na [SQL Server na Přehled virtuálních počítačů Azure](virtual-machines-windows-sql-server-iaas-overview.md).
