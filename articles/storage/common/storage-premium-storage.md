---
title: "Storage úrovně Premium aaaHigh výkonu Azure ke správě a disky pro virtuální počítače | Microsoft Docs"
description: "Další informace o vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače Azure. Azure DS-series, DSv2-series, GS-series a virtuálních počítačů služby Fs-series, které podporují službu Premium Storage."
services: storage
documentationcenter: 
author: ramankumarlive
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: ramankum
ms.openlocfilehash: 5f08e615869ed59a9d80d798ec191dd4d3abc3e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače
Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače (VM) s vstupní a výstupní (I/O)-zatížení s intenzivním. Disky virtuálních počítačů, které používají úložiště Premium ukládat data do disků SSD (Solid-State Drive). výhody tootake hello rychlosti a výkonu prémiové disky úložiště, můžete migrovat existující virtuální počítač tooPremium disky úložiště.

V Azure můžete připojit několik premium storage disky tooa virtuálních počítačů. Použití více disků poskytuje aplikace až too256 TB úložiště na virtuální počítač. Storage úrovně Premium můžete dosáhnout aplikace 80 000 vstupně-výstupních operací za sekundu (IOPS) na virtuální počítač a disku propustností až too2 000 MB za sekundu (MB/s) na virtuální počítač. Operace čtení poskytují velmi nízkou latenci.

Storage úrovně Premium Azure nabízí možnost hello tootruly navýšení a shift náročné podnikové aplikace jako Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite a SharePoint farmy toohello cloudu. Úlohy náročné na výkon databáze můžete spustit v aplikacích, jako je SQL Server, Oracle, MongoDB, MySQL a Redis, které vyžadují konzistentní vysoký výkon a nízkou latencí.

> [!NOTE]
> Hello nejlepšího výkonu pro vaši aplikaci doporučujeme migraci všech disků virtuálního počítače, které vyžaduje vysokou IOPS tooPremium úložiště. Pokud disk nevyžadují vysokou IOPS, můžete pomoct limit náklady udržováním v standardní úložiště Azure. V standardního úložiště data disku virtuálního počítače ukládána na pevných disků (HDD) místo na jednotkách SSD.
> 

Azure nabízí dva způsoby, jak disky úložiště premium toocreate pro virtuální počítače:

* **Nespravované disky**

    původní metodou Hello je toouse nespravované disky. Nespravované disku spravovat účty úložiště hello použít toostore hello virtuální pevný disk (VHD) soubory, které odpovídají tooyour disky virtuálních počítačů. Soubory VHD jsou uloženy jako objekty BLOB stránky v účtech úložiště Azure. 

* **Spravované disky**

    Pokud vyberete [Azure spravované disky](../../virtual-machines/windows/managed-disks-overview.md), spravuje hello účty úložiště, které používáte pro disky virtuálních počítačů Azure. Určete typ disku hello (Standard nebo Premium) a velikost hello hello disku, které potřebujete. Azure vytváří a spravuje hello disku za vás. Nemáte tooworry o uvedení hello disky více tooensure účty úložiště, který zůstat v limitech škálovatelnosti pro účty úložiště. Který zpracovává Azure.

Doporučujeme, abyste zvolili spravované disky, tootake výhod jejich mnoha funkcí.

začít s Storage úrovně Premium, tooget [vytvořit účet Azure zdarma](https://azure.microsoft.com/pricing/free-trial/). 

Informace o migraci stávající virtuální počítače tooPremium úložiště najdete v tématu [převést virtuální počítač s Windows z disků toomanaged nespravované disky](../../virtual-machines/windows/convert-unmanaged-to-managed-disks.md) nebo [převést virtuální počítač s Linuxem z disků toomanaged nespravované disky](../../virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Storage úrovně Premium je k dispozici ve většině oblastí. Pro hello seznam dostupných oblastí v [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services), podívejte se na hello oblasti, ve kterém podporované virtuální počítače velikost řady podporu Premium (DS-series, DSV2-series, GS-series a virtuálních počítačů služby Fs-series) jsou podporovány.
> 

## <a name="features"></a>Funkce

Zde jsou některé z funkcí hello úložiště Premium Storage:

* **Disky úložiště Premium**

    Premium Storage podporuje VM disky, které může být připojen toospecific velikost series virtuálních počítačů. Premium Storage podporuje DS-series, DSv2-series, GS-series, Ls-series a virtuálních počítačů služby Fs-series. Máte možnost volby velikostí disku sedm: P4 (32GB), P6 (64GB), P10 (128GB), P20 (512GB), P30 (1024GB), P40 (2 048 GB), P50 (4095GB). P4 a velikosti disků P6 ještě podporují jenom pro spravované disky. Velikost každého disku má svou vlastní specifikace výkonu. V závislosti na požadavcích vaší aplikace můžete připojit jednoho nebo více disků tooyour virtuálních počítačů. Jsme hello specifikace v podrobněji popisují [Storage úrovně Premium škálovatelnosti a cílech výkonnosti](#scalability-and-performance-targets).

* **Objekty BLOB stránky Premium**

    Premium Storage podporuje objekty BLOB stránky. Pomocí stránky objekty BLOB toostore trvalé, nespravované disků pro virtuální počítače v Storage úrovně Premium. Na rozdíl od standardní Azure Storage Storage úrovně Premium není podporují objekty BLOB bloku, doplňovací objekty BLOB, soubory, tabulky a fronty. Objekty BLOB stránky Premium podporuje šesti velikost se pohybuje od P10 tooP50 a P60 (8191GiB). Objekt blob stránky P60 Premium není podporované toobe připojené jako disky virtuálních počítačů. 

    Jakýkoli objekt umístit do prémiový účet úložiště bude objekt blob stránky. Objekt blob stránky Hello Připnutí podporovaném tooone hello zřízené velikosti. Z tohoto důvodu prémiový účet úložiště není určen toobe používá toostore jen nepatrnou objekty BLOB.

* **Prémiový účet úložiště**

    toostart použití služby Premium Storage, vytvořit účet úložiště premium pro nespravovaná disky. V hello [portál Azure](https://portal.azure.com), toocreate prémiový účet úložiště, zvolte hello **Premium** úroveň výkonu. Vyberte hello **místně redundantní úložiště (LRS)** možnost replikace. Také vytvoříte účet úložiště premium nastavením typu hello příliš**Premium_LRS** v jednom z následujících umístění hello:
    * [Rozhraní API REST úložiště](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (verze 2014-02-14 nebo novější)
    * [REST API pro správu služby](http://msdn.microsoft.com/library/azure/ee460799.aspx) (verze 2014-10-01 nebo novější; pro nasazení Azure classic)
    * [Azure Storage prostředků zprostředkovatele REST API](https://docs.microsoft.com/rest/api/storagerp) (pro nasazení Azure Resource Manager)
    * [Prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs.md) (verze 0.8.10 nebo novější)

    toolearn o omezeních účtu úložiště premium, najdete v části [Storage úrovně Premium škálovatelnosti a cílech výkonnosti](#premium-storage-scalability-and-performance-targets).

* **Místně redundantní úložiště úrovně Premium**

    Prémiový účet úložiště podporuje jenom místně redundantního úložiště jako možnost replikace hello. Místně redundantní úložiště udržuje tři kopie dat hello v jedné oblasti. Místní zotavení po havárii, je třeba zálohovat vaše disky virtuálních počítačů v jiné oblasti s použitím [Azure Backup](../../backup/backup-introduction-to-azure-backup.md). Také musíte použít účet geograficky redundantní úložiště (GRS) jako úložiště záloh hello. 

    Azure pomocí svého účtu úložiště jako kontejner pro nespravovaná disky. Když vytvoříte Azure DS-series, DSv2-series, GS-series, nebo služby Fs-series virtuálních počítačů s nespravované disky a vyberte účet úložiště premium operačního systému a datové disky jsou uložené v daném účtu úložiště.

## <a name="supported-vms"></a>Podporované virtuální počítače
Premium Storage podporuje DS-series, DSv2-series, GS-series, Ls-series a virtuálních počítačů služby Fs-series. Můžete použít úložiště standard a premium disků s těmito typy virtuálních počítačů. Disky úložiště premium nelze použít s řadou virtuálních počítačů, které nejsou Premium úložiště kompatibilní.

Informace o typech virtuálních počítačů a velikosti v Azure pro Windows najdete v tématu [velikosti virtuálních počítačů Windows](../../virtual-machines/virtual-machines-windows-sizes-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informace o typech virtuálních počítačů a velikosti v Azure pro Linux najdete v tématu [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Toto jsou některé funkce hello hello DS-series, DSv2-series, GS-series, Ls-series a Fs-series virtuální počítače:

* **Cloudové služby**

    Můžete přidat virtuální počítače DS-series tooa Cloudová služba, která má jenom virtuální počítače DS-series. Nepřidávejte virtuálních počítačů služby DS-series tooan stávající cloudovou službu, která má jiného typu než DS-series virtuálních počítačů. Můžete migrovat vaše stávající virtuální pevné disky tooa novou cloudovou službu spuštěnou jenom virtuální počítače DS-series. Pokud chcete toouse hello stejné virtuální IP adresy pro hello novou cloudovou službu, která hostuje virtuální počítače DS-series, použijte [rezervovaných IP adres](../../virtual-network/virtual-networks-instance-level-public-ip.md). Virtuální počítače GS-series lze přidat tooan stávající cloudovou službu, která má jenom virtuální počítače GS-series.

* **Disk operačního systému**

    Vaše toouse virtuálního počítače služby Premium Storage můžete nastavit premium nebo disk standardní operačního systému. Pro hello co nejlepších výsledků doporučujeme používat disk operačního systému úložiště Premium.

* **Datové disky**

    Můžete použít premium a standardní disky v hello stejné virtuálního počítače služby Storage úrovně Premium. Storage úrovně Premium můžete zřídit virtuální počítač a připojit několik trvalých datových disků toohello virtuálních počítačů. V případě potřeby tooincrease hello kapacitu a výkon hello svazku, můžete rozkládají napříč vaše disky.

    > [!NOTE]
    > Pokud rozkládají premium storage datových disků pomocí [prostory úložiště](http://technet.microsoft.com/library/hh831739.aspx), nastavit prostory úložiště s 1 sloupec pro každý disk, který používáte. Jinak celkový výkon hello rozdělená svazku může být nižší, než se očekávalo kvůli nevyrovnaná distribuce přenosů mezi disky hello. Ve výchozím nastavení, ve Správci serveru můžete nastavit sloupce pro too8 disky. Pokud máte více než 8 disky pomocí prostředí PowerShell toocreate hello svazku. Ručně zadejte hello počet sloupců. Hello uživatelského rozhraní správce serveru, jinak hodnota pokračuje sloupce toouse 8 i v případě, že máte více disků. Pokud máte v sadě jeden stripe 32 disků, můžete například zadejte 32 sloupců. počet sloupců se hello toospecify hello virtuální disk používá, v hello [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) rutiny prostředí PowerShell, použijte hello *NumberOfColumns* parametr. Další informace najdete v tématu [prostory úložiště – přehled](http://technet.microsoft.com/library/hh831739.aspx) a [nejčastější dotazy prostory úložiště](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Mezipaměti**

    Virtuální počítače v hello velikost řady, která podporují službu Premium Storage mít jedinečné funkce ukládání do mezipaměti pro vysoké úrovně propustnosti a latence. ukládání do mezipaměti schopností Hello překračuje základní výkon disku úložiště premium. Můžete nastavit hello disku příliš ukládání do mezipaměti zásad na discích úložiště premium**jen pro čtení**, **ReadWrite**, nebo **žádné**. disk Hello výchozí zásady ukládání do mezipaměti je **jen pro čtení** pro všechny datové disky premium a **ReadWrite** pro disky operačního systému. Pro optimální výkon pro vaši aplikaci opravte hello použití nastavení mezipaměti. Například pro náročné pro čtení nebo jen pro čtení datových disků, jako je například datové soubory SQL serveru, nastavte hello disku příliš zásady ukládání do mezipaměti**jen pro čtení**. Pro náročné zápisu nebo pouze pro zápis datových disků, jako jsou soubory protokolu serveru SQL Server, nastavte hello disku příliš zásady ukládání do mezipaměti**žádné**. toolearn Další informace o optimalizaci návrhu Storage úrovně Premium, najdete v části [návrh z hlediska výkonu Storage úrovně Premium](storage-premium-storage-performance.md).

* **Analýzy**

    výkon virtuálního počítače tooanalyze použitím disků ve Storage úrovně Premium, zapnout Diagnostika virtuálních počítačů v hello [portál Azure](https://portal.azure.com). Další informace najdete v tématu [monitorování virtuálního počítače Azure pomocí rozšíření diagnostiky Azure](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    výkon disku toosee, pomocí nástrojů na základě operačního systému jako [sledování výkonu systému Windows](https://technet.microsoft.com/library/cc749249.aspx) pro virtuální počítače Windows a hello [iostat](http://linux.die.net/man/1/iostat) příkazu pro virtuální počítače s Linuxem.

* **Limity škálování virtuálních počítačů a výkon**

    Velikost pro všechny podporované Storage úrovně Premium virtuálních počítačů má limity škálování a výkon specifikace IOPS, šířky pásma a hello počet disků, které je možné připojit na virtuální počítač. Pokud používáte prémiové disky úložiště s virtuálními počítači, ujistěte se, zda je dostatek IOPS a šířky pásma v provozu toodrive disku virtuálního počítače.

    Virtuální počítač STANDARD_DS1 má například vyhrazené šířky pásma souboru 32 MB/s pro provoz disk úložiště premium. Disk úložiště premium P10 můžete zadat šířku pásma 100 MB/s. Pokud disk úložiště premium P10 připojené toothis virtuálních počítačů, můžete přejít jenom až too32 MB/s. Dobrý den, můžete zadat maximální 100 MB/s tento disk P10 hello nemůže používat.

    V současné době hello největší virtuálního počítače v hello DS-series je hello Standard_DS15_v2. Hello Standard_DS15_v2 poskytuje až too960 MB/s na všechny disky. Hello největší virtuálního počítače v hello GS-series je hello Standard_GS5. Hello Standard_GS5 poskytuje až too2 000 MB/s na všechny disky.

    Všimněte si, že těchto mezních hodnot disku pouze pro provoz. Tyto limity neobsahují přístupů k mezipaměti a síťový provoz. Samostatné šířky pásma je k dispozici pro provoz sítě virtuálních počítačů. Šířka pásma pro síťový provoz se liší od hello vyhrazené šířky pásma používané disky úložiště premium.

    Hello nejaktuálnější informace o maximální IOPS a propustnost (šířka pásma) pro virtuální počítače podporované Premium Storage najdete v tématu [velikosti virtuálních počítačů Windows](/azure/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    Další informace o discích úložiště premium a jejich omezení IOPS a propustnost najdete v části hello tabulky v další části hello.

## <a name="scalability-and-performance-targets"></a>Škálovatelnost a cíle výkonnosti
V této části jsme popisují tooconsider cíle hello škálovatelnost a výkon při použití úložiště Premium.

Prémiové účty úložiště mají následující cíle škálovatelnosti hello:

| Celkový počet účet kapacity | Celková šířka pásma pro účet místně redundantní úložiště |
| --- | --- | 
| Kapacita disku: 35 TB <br>Pořízení snímku kapacity: 10 TB | Až too50 gigabity za sekundu pro příchozí<sup>1</sup> + odchozí<sup>2</sup> |

<sup>1</sup> všechna data (počet požadavků), které se odesílají tooa účet úložiště

<sup>2</sup> všechna data (odpovědí), které byly přijaty z účtu úložiště

Další informace najdete v tématu [Azure Storage škálovatelnosti a cílech výkonnosti](storage-scalability-targets.md).

Pokud používáte prémiové účty úložiště pro nespravovaná disky a aplikace překračuje hello cíle škálovatelnosti účtu jedno úložiště, můžete chtít toomigrate toomanaged disky. Pokud nechcete, aby toomigrate toomanaged disky, vytvoříte více účtů úložiště toouse vaší aplikace. Potom oddílu data mezi různými tyto účty úložiště. Například pokud chcete tooattach 51 TB disky napříč více virtuálními počítači, rozloženy je dva účty úložiště. 35 TB je hello omezení pro jednu prémiový účet úložiště. Zajistěte, aby jeden prémiový účet úložiště nikdy má více než 35 TB zřizované disky.

### <a name="premium-storage-disk-limits"></a>Limity disk úložiště Premium
Při zřizování disk úložiště premium, velikost hello hello disku určuje hello maximální IOPS a propustnost (šířka pásma). Azure nabízí sedm typy disků úložiště premium: P4 (spravované jen disky), P6 (spravované jen disky), P10, P20, P30, P40 a P50. Každý typ disku úložiště premium má zvláštní omezení pro IOPS a propustnosti. Limity pro typy disků hello jsou popsané v následující tabulce hello:

| Disky typu Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Velikost disku           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Vstupně-výstupní operace za sekundu / disk       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Propustnost / disk | 25 MB za sekundu  | 50 MB za sekundu  | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu | 250 MB za sekundu | 250 MB za sekundu | 

> [!NOTE]
> Zkontrolujte, zda je k dispozici v provozu toodrive disku virtuálního počítače, dostatečnou šířku pásma, jak je popsáno v [Storage úrovně Premium podporované virtuální počítače](#premium-storage-supported-vms). Propustnost disku a IOPS, jinak je omezené toolower hodnoty. Maximální propustnost a IOPS jsou založené na omezení hello virtuálních počítačů, nikoli na omezení disku hello popsané v předcházející tabulce hello.  
> 
> 

Zde jsou některé důležité věci tooknow o cíle škálovatelnost a výkon úložiště Premium:

* **Zřízené kapacitu a výkon**

    Při zřizování disk úložiště premium, na rozdíl od standardní úložiště je zaručena hello kapacitu, IOPS a propustnost tohoto disku. Například pokud vytvoříte P50 disk, Azure zřídí kapacitu 4095 GB úložiště, 7500 IOPS a propustnost 250 MB/s pro tento disk. Aplikace můžete použít, nebo jeho část hello kapacitu a výkon.

* **Velikost disku**

    Azure mapy hello toohello (zaokrouhlený nahoru) velikost disku nejbližší možnost disk úložiště premium, jak je uvedeno v tabulce hello v předcházející části hello. Například velikost 100 GB na disku je klasifikován tak možnost P10. Too500 IOPS, provést s až propustnost too100 MB/s. Podobně disk 400 GB je klasifikován tak P20 velikost. Může provádět až too2, 300 IOPS, s propustností 150 MB/s.
    
    > [!NOTE]
    > Snadno můžete zvýšit velikost hello stávající disky. Například můžete tooincrease hello velikost 30 GB disk too128 GB nebo TB i too1. Nebo můžete chtít tooconvert tooa P30 P20 disku vzhledem k tomu, že budete potřebovat větší kapacitu nebo další IOPS a propustnosti. 
    > 
 
* **Velikost vstupně-výstupních operací**

    velikost Hello vstupně-výstupních operací je 256 KB. Pokud hello přenášených dat je menší než 256 KB, považuje 1 jednotka vstupně-výstupní operace. Větší velikosti vstupně-výstupních operací, se počítají jako více vstupně-výstupních operací velikost 256 KB. Například 1100 KB vstupně-výstupních operací se počítá jako jednotky 5 vstupně-výstupní operace.

* **Propustnost**

    omezení propustnosti Hello zahrnuje zápisy toohello disku a obsahuje operací čtení disku, které nejsou obsluhovat z mezipaměti hello. Například P10 disk má propustnost 100 MB/s na disk. Některé příklady platný propustnost pro P10 disk jsou zobrazeny v hello následující tabulka:

    | Maximální propustnost za P10 disku | Bez mezipaměti čtení z disku | Bez mezipaměti zapíše toodisk |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Přístupů k mezipaměti**

    Počet přístupů do mezipaměti nejsou omezeny hello přidělené IOPS nebo propustnost disku hello. Například při použití datový disk se **jen pro čtení** nastavení mezipaměti na virtuální počítač, který podporuje úložiště úrovně Premium, čtení, které jsou obsluhovány z mezipaměti hello nejsou subjektu toohello IOPS a propustnost CAP k vzdálené ploše hello disku. Pokud je zatížení hello disku převážně operace čtení a může získat velmi vysoké propustnosti. mezipaměť Hello je subjektu tooseparate IOPS a omezení propustnosti na hello úroveň virtuálního počítače podle hello velikost virtuálního počítače. Virtuální počítače DS-series mají přibližně 4 000 IOPS a 33 MB/s propustnost za jádra pro mezipaměť a místní SSD vstupně-výstupních operací. Virtuální počítače GS-series mít maximálně 5 000 IOPS a 50 MB/s propustnost za jádra pro mezipaměť a místní SSD vstupně-výstupních operací. 

## <a name="throttling"></a>Omezování
Omezování může dojít, pokud vaše aplikace IOPS nebo propustnost překračuje omezení hello přidělené pro disk úložiště premium. Omezení také může dojít k provozu celkový disku na všech discích na hello virtuálního počítače překračuje limit šířky pásma hello disku k dispozici pro hello virtuálních počítačů. tooavoid omezení, doporučujeme vám, že omezíte hello počet nevyřízených žádostí v/v disku hello. Použijte omezení na základě škálovatelnosti a cílech výkonnosti pro hello disk, který máte zřízen a na hello disku šířky pásma k dispozici toohello virtuálních počítačů.  

Aplikace můžete dosáhnout hello nejnižší latenci, když je navržen tak tooavoid omezení. Ale pokud hello počet nevyřízených žádostí v/v disku hello je příliš malá, vaše aplikace nemohou využívat hello maximální IOPS a propustnost úrovně, které jsou k dispozici toohello disku.

Hello následující příklady ukazují, jak toocalculate omezení úrovně. Všechny výpočty vycházejí z jednotky velikost 256 KB vstupně-výstupních operací.

### <a name="example-1"></a>Příklad 1
Vaše aplikace zpracovala za jednu sekundu na P10 disk 495 vstupně-výstupních operací jednotky o velikosti 16 KB. jednotky Hello vstupně-výstupních operací se počítají jako 495 IOPS. Pokud se pokusíte vstupně-výstupní 2 MB, ve stejné druhý Dobrý den, hello celkem vstupně-výstupních operací jednotky je rovna too495 + 8 IOPS. Důvodem je, že vstupně-výstupních operací 2 MB = 2 048 KB / 256 KB = 8 vstupně-výstupních operací jednotek, když hello velikost jednotky vstupně-výstupních operací je 256 KB. Protože hello součet 495 + 8 přesahuje hello 500 limit IOPS pro hello disk, omezování nastane.

### <a name="example-2"></a>Příklad 2
Vaše aplikace zpracovala 400 vstupně-výstupních operací jednotky velikost 256 KB na P10 disku. je Hello celkovou šířku pásma využívat (400 &#215; 256) / 1 024 KB = 100 MB/s. P10 disk může mít propustnost 100 MB/s. Pokud se aplikace pokusí tooperform více vstupně-výstupních operací v této druhé, je omezena, protože překračuje limit přidělené hello.

### <a name="example-3"></a>Příklad 3
Máte virtuální počítač, DS4 s dvěma P30 disky připojené. Každý disk P30 je propustnost 200 MB/s. Virtuální počítač DS4 má však kapacitou disku celkové šířky pásma 256 MB/s. Maximální propustnost i připojené disky toohello nelze jednotky na tomto virtuálním počítači DS4 v hello stejnou dobu. tooresolve, dokáže odolat provoz 200 MB/s na jednom disku a 56 MB/s na hello jiný disk. Pokud hello součet disku provozu, prochází přes 256 MB/s, je omezen provoz disku.

> [!NOTE]
> Pokud disk provozu většinou obsahuje malé vstupně-výstupních operací velikosti, se setkají aplikace pravděpodobně limit IOPS hello před omezení propustnosti hello. Ale pokud přenosů disku hello většinou obsahuje velké vstupně-výstupních operací velikosti, pravděpodobně aplikace se setkají omezení propustnosti hello nejprve místo hello IOPS limit. Pomocí optimální velikost vstupně-výstupních operací můžete zvýšit IOPS vaší aplikace a kapacity propustnosti. Můžete také omezit hello počet čekajících požadavků vstupně-výstupních operací pro disk.
> 

toolearn Další informace o návrhu pro vysoký výkon pomocí Storage úrovně Premium, najdete v části [návrh z hlediska výkonu Storage úrovně Premium](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Snímků a kopírování objektů Blob

toohello služba úložiště, soubor virtuálního pevného disku hello je objekt blob stránky. Můžete pořizovat snímky objekty BLOB stránky a zkopírujte je tooanother umístění, jako například tooa jiný účet úložiště.

### <a name="unmanaged-disks"></a>Nespravované disky

Vytvoření [přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md) pro nespravovaná prémiové disky hello stejný způsob, jak použít snímky s standardní úložiště. Storage úrovně Premium podporuje pouze místně redundantního úložiště jako možnost replikace hello. Doporučujeme vytvořit snímky a poté zkopírujte hello snímky tooa geograficky redundantní standardní účet úložiště. Další informace najdete v tématu [možnosti redundance úložiště Azure](storage-redundancy.md).

Pokud disk připojené tooa virtuálních počítačů, nejsou některé operace rozhraní API na disku hello povolené. Například nelze provést [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob) operace u objektu blob Pokud hello disk připojen tooa virtuálních počítačů. Místo toho nejprve vytvořte snímek tomuto objektu blob pomocí hello [snímku Blob](/rest/api/storageservices/Snapshot-Blob) REST API. Potom proveďte hello [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob) disk připojil hello snímku toocopy hello. Alternativně můžete odpojit hello disk a potom proveďte všechny potřebné operace.

Hello následující omezení platí toopremium úložiště objektů blob snímky:

| Limit úložiště Premium | Hodnota |
| --- | --- |
| Maximální počet snímků za objektů blob | 100 |
| Kapacitě účtu úložiště pro snímky<br>(Obsahuje data pouze snímky. Nezahrnuje data v základní objekt blob.) | 10 TB |
| Minimální doba mezi po sobě jdoucí snímky | 10 minut |

geograficky redundantní kopie toomaintain vaše snímků, snímky můžete zkopírovat z prémiový účet tooa geograficky redundantní úložiště standardní účet úložiště pomocí nástroje AzCopy nebo kopírovat objekt Blob. Další informace najdete v tématu [přenosu dat pomocí příkazového řádku azcopy hello](storage-use-azcopy.md) a [kopírovat objekt Blob](/rest/api/storageservices/Copy-Blob).

Podrobné informace o provádění operací REST pro objekty BLOB stránky v prémiový účet úložiště najdete v tématu [Blob operace služby s Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Managed Disks

Snímek se spravovaným diskem je kopii spravovaných disků na hello jen pro čtení. Hello snímek je uložený jako standardní se spravovaným diskem. V současné době [přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md) nejsou podporovány pro spravované disky. toolearn jak zjistit, tootake snímek spravovaným diskem, [vytvoření kopie virtuální pevný disk uložený jako spravovaný Azure disku pomocí spravovaného snímky ve Windows](../../virtual-machines/windows/snapshot-copy-managed-disk.md) nebo [vytvoření kopie virtuální pevný disk uložený jako spravovaný Azure disku pomocí spravované snímky v systému Linux](../../virtual-machines/linux/snapshot-copy-managed-disk.md).

Pokud se spravovaným diskem připojené tooa virtuálních počítačů, nejsou povolené některé operace rozhraní API na disku hello. Například nelze generovat sdílený přístupový podpis (SAS) tooperform operace kopírování při hello disku připojené tooa virtuálních počítačů. Místo toho nejprve vytvořte snímek hello disku a pak proveďte hello kopii hello snímku. Alternativně můžete odpojit hello disk a pak generovat operace kopírování hello tooperform SAS.


## <a name="premium-storage-for-linux-vms"></a>Storage úrovně Premium pro virtuální počítače s Linuxem
Můžete použít následující toohelp informace, které vaše virtuální počítače s Linuxem v Storage úrovně Premium hello:

škálovatelnost tooachieve cílem v Storage úrovně Premium, pro všechny disky úložiště premium se do mezipaměti sady příliš**jen pro čtení** nebo **žádné**, je nutné zakázat "překážek", když připojíte hello systému souborů. Překážek v tomto scénáři není nutné, protože hello zapíše disky úložiště toopremium jsou trvalé pro tato nastavení mezipaměti. Až se žádost o zápis hello úspěšně dokončí, byla data zapsána toohello trvalého úložiště. toodisable "překážek," použijte jednu z následujících metod hello. Zvolte hello, jednu pro systém souborů:
  
* Pro **reiserFS**, toodisable překážek, použijte hello `barrier=none` možnost připojit. (tooenable překážek, použijte `barrier=flush`.)
* Pro **ext3/ext4**, toodisable překážek, použijte hello `barrier=0` možnost připojit. (tooenable překážek, použijte `barrier=1`.)
* Pro **XFS**, toodisable překážek, použijte hello `nobarrier` možnost připojit. (tooenable překážek, použijte `barrier`.)
* Pro prémiové disky úložiště s mezipamětí nastavit příliš**ReadWrite**, povolit překážek pro odolnost zápisu.
* Pro svazek popisky toopersist po restartování hello virtuálních počítačů, je nutné aktualizovat /etc/fstab s hello identifikátor UUID (UUID) odkazy toohello disky. Další informace najdete v tématu [přidat tooa spravovaných disků na virtuální počítač s Linuxem](../../virtual-machines/linux/add-disk.md).

Hello následující Linuxových distribucích ověření pro Storage úrovně Premium. Pro lepší výkon a stabilitu Storage úrovně Premium, doporučujeme upgradu tooone vaše virtuální počítače těchto verzí minimálně (nebo tooa novější verzi). Některé verze vyžadují hello hello nejnovější Linux integrační služby LIS (), v4.0, pro Azure. toodownload a nainstalovat distribuční, postupujte podle uvedených v následující tabulce hello odkaz hello. Bitové kopie toohello seznamu přidáme jsme dokončení ověření. Všimněte si, že naše ověření ukazují, že výkon se liší u každé bitové kopie. Výkon závisí na charakteristiky zatížení a nastavení bitové kopie. Jiné bitové kopie jsou optimalizovaných pro různé druhy úloh.

| Distribuce | Verze | Podporované jádra | Podrobnosti |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-Server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-Server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SuSE-sles-12-priority-v20150213 <br> SuSE sles 12 v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 vyžaduje](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Další informace v poznámce v další části hello* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Doporučená LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Další informace v poznámce v další části hello* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 nebo RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 nebo RHCK s[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 nebo RHCK s[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>Ovladače LIS pro OpenLogic CentOS

Pokud používáte OpenLogic CentOS virtuální počítače, spusťte následující příkaz tooinstall hello nejnovější ovladače hello:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

tooactivate hello nové ovladače, restartujte počítač hello.

## <a name="pricing-and-billing"></a>Ceny a fakturace

Pokud používáte Storage úrovně Premium, platí hello následující fakturace aspekty:

* **Velikost disku a objektů blob úložiště Premium**

    Fakturace disku úložiště premium nebo objekt blob závisí na velikosti hello zřízený hello disku nebo objektů blob. Azure mapy hello zřízené velikost (zaokrouhlený nahoru) toohello nejbližší možnost disk úložiště premium. Podrobnosti najdete v tématu hello tabulky v [Storage úrovně Premium škálovatelnosti a cílech výkonnosti](#premium-storage-scalability-and-performance-targets). Každý disk mapy tooa podporované zřízený velikost disku a se fakturuje odpovídajícím způsobem. Fakturace pro všechny zajišťovaný disk je pomocí hello měsíční poplatek pro Storage úrovně Premium nabídka hello průběžně každou hodinu. Například pokud zřízený P10 disku a odstranit po 20 hodin, se vám účtuje hello P10 nabídky poměrné too20 hodin. To je bez ohledu na množství hello skutečná data zapsána toohello disku nebo hello IOPS a propustnost použít.

* **Snímky Premium nespravované disky**

    Snímky na nespravované prémiové disky se účtují hello používá hello snímky dodatečnou kapacitu. Další informace o snímcích najdete v tématu [vytvoření snímku objektu blob](/rest/api/storageservices/Snapshot-Blob).

* **Premium spravované snímky disky**

    Snímek se spravovaným diskem je kopii hello disku jen pro čtení. Hello disk je uložený jako standardní se spravovaným diskem. Snímek náklady hello stejné jako standardní spravovaná disku. Například pokud pořídíte snímek 128 GB premium se spravovaným diskem, náklady na hello hello snímku je ekvivalentní tooa 128 GB spravovaných disků na úrovni standard.  

* **Odchozí přenosy dat**

    [Odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/) (data přejdete mimo datových centrech Azure) jsou zpoplatněné podle využití šířky pásma.

Podrobné informace o cenách pro Storage úrovně Premium Storage úrovně Premium podporované virtuální počítače a spravované disků, naleznete v článcích:

* [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Spravované disky ceny](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Podporu zálohování Azure 

Místní zotavení po havárii, je třeba zálohovat vaše disky virtuálních počítačů v jiné oblasti s použitím [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) a účet úložiště GRS jako trezor záloh.

toocreate úlohu zálohování s zálohy založené na čase, snadno obnovení virtuálních počítačů a zásady uchovávání záloh, pomocí Azure Backup. Můžete použít zálohování i s disky a nespravované. Další informace najdete v tématu [zálohování Azure pro virtuální počítače s nespravované disky](../../backup/backup-azure-vms-first-look-arm.md) a [zálohování Azure pro virtuální počítače s spravované disky](../../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Další kroky
Další informace o Premium Storage najdete v tématu hello následující články.

### <a name="design-and-implement-with-premium-storage"></a>Návrh a implementaci Storage úrovně Premium
* [Návrh pro výkon Storage úrovně Premium](storage-premium-storage-performance.md)
* [Operace úložiště objektů BLOB Storage úrovně Premium](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Provozní pokyny
* [Migrace tooAzure Storage úrovně Premium](../common/storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Azure Premium Storage všeobecně dostupná](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Uvedení hello GS-series: Přidání úložiště Premium podporu toohello největší virtuálních počítačů v hello veřejného cloudu](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)
