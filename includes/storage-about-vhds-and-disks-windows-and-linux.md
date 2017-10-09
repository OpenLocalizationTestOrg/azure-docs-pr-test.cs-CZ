
## <a name="about-vhds"></a>Virtuální pevné disky

Hello virtuálních pevných disků použitých v Azure jsou soubory VHD uložené jako objekty BLOB stránky v účtu úložiště standard nebo premium v Azure. Podrobnosti o objektech blob stránky najdete v tématu [Vysvětlení objektů blob bloku a objektů blob stránky](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Podrobnosti o úložišti úrovně Premium najdete v článku [Vysoce výkonné úložiště úrovně Premium a virtuální počítače Azure](../articles/storage/common/storage-premium-storage.md).

Azure podporuje hello pevný disk Formát VHD. Hello pevné formát vytvoří hello logický disk se lineárně v rámci tohoto souboru hello, aby tento posun disku X je uložený v objektu blob Posun X. Malé zápatí na konci hello objektu hello blob popisuje vlastnosti hello hello virtuálního pevného disku. Pevný formát hello často zabírají místo, protože většina disky mají velké nepoužívané oblasti v nich. Však uloží Azure soubory VHD v zhuštěných formátu, aby zobrazí hello výhody obou hello fixed a dynamické disky na hello stejný čas. Další podrobnosti najdete v tématu [Začínáme s virtuálními pevnými disky](https://technet.microsoft.com/library/dd979539.aspx).

Všechny soubory VHD v Azure, které mají toouse jako zdrojové toocreate disky nebo bitové kopie jsou jen pro čtení. Při vytváření disku nebo bitové kopie, Azure vytváří kopie hello soubory VHD. Tyto kopie může být jen pro čtení nebo pro čtení a zápisu, v závislosti na tom, jak používáte hello virtuálního pevného disku.

Když vytvoříte virtuální počítač z bitové kopie, Azure vytvoří disk pro hello virtuální počítač, který je kopií souboru VHD zdrojového hello. tooprotect před náhodným odstraněním, Azure umístí zapůjčení na zdrojový soubor VHD, který byl použit toocreate bitovou kopii, disk operačního systému nebo datový disk.

Před odstraněním zdrojového souboru VHD, budete potřebovat tooremove hello zapůjčení odstraněním hello disku nebo bitové kopie. toodelete soubor VHD, který je používán jako disk operačního systému na virtuálním počítači, můžete odstranit hello virtuální počítač, disk operačního systému hello a soubor VHD zdroj hello všechny najednou tak, že odstraňuje hello virtuální počítač a odstraňování všechny přidružené disky. Nicméně odstranění souboru .vhd, který je zdrojem pro datový disk, vyžaduje několik kroků v určitém pořadí. Nejprve odpojte hello disku z hello virtuálního počítače a pak odstraňte hello disku a pak odstraňte soubor VHD hello.

> [!WARNING]
> Pokud odstraníte zdrojový soubor .vhd z úložiště nebo pokud odstraníte účet úložiště, Microsoft pro vás tato data nebude moct obnovit.
> 

## <a name="types-of-disks"></a>Typy disků 

Disky Azure jsou navržené pro 99,999% dostupnost. Disky systému Azure konzistentně mít doručit odolnost podnikové úrovni s špičkový NULOVÉ % Annualized míra selhání.

Při vytváření disků si můžete vybrat ze dvou úrovní výkonu úložiště – Storage úrovně Standard a Storage úrovně Premium. Existují také dva typy disků, spravované a nespravované, které můžou být umístěné v obou úrovních výkonu.


### <a name="standard-storage"></a>Storage úrovně Standard 

Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu. Službu Storage úrovně Standard je možné místně replikovat v jednom datacentru, nebo může být geograficky redundantní pomocí primárních a sekundárních datových center. Další informace o replikaci úložiště najdete v tématu [Replikace služby Azure Storage](../articles/storage/common/storage-redundancy.md). 

Další informace o používání služby Storage úrovně Standard s disky virtuálních počítačů najdete v tématu [Storage úrovně Standard a disky](../articles/storage/common/storage-standard-storage.md).

### <a name="premium-storage"></a>Storage úrovně Premium 

Služba Storage úrovně Premium je založená na jednotkách SSD a poskytuje podporu vysoce výkonných disků s nízkou latencí pro virtuální počítače, na kterých se spouští náročné vstupně-výstupní úlohy. Premium Storage můžete použít s DS, DSv2, GS, Ls nebo virtuálních počítačích Azure řady služby FS. Další informace najdete v článku [Storage úrovně Premium](../articles/storage/common/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Nespravované disky

Nespravované disky jsou hello tradiční typ disky, které byly používány virtuálními počítači. Pomocí těchto vytvoření účtu úložiště a zadejte tento účet úložiště při vytvoření disku hello. Máte toomake, zda není příliš mnoho disků chápat hello stejný účet úložiště, protože může překročit hello [cíle škálovatelnosti](../articles/storage/common/storage-scalability-targets.md) hello účtu úložiště (20 000 IOPS, např.), což vede k hello omezené virtuálních počítačů. Nespravované disků máte toofigure na tom, jak toomaximize hello použití jednoho nebo více účtů tooget hello nejlepší výkon úložiště mimo virtuální počítače.

### <a name="managed-disks"></a>Managed Disks 

Spravované obslužné rutiny disky úložiště hello vytvoření nebo Správa hello pozadí účtů pro vás a zajistí, že nemáte tooworry o limitech škálovatelnosti hello hello účtu úložiště. Jednoduše zadejte velikost disku hello a úroveň výkonu hello (Standard nebo Premium) a Azure vytváří a spravuje hello disku za vás. To i v případě přidat disky nebo škálovat nahoru a dolů hello virtuálních počítačů, nemáte tooworry o úložišti hello používá. 

Můžete také spravovat vlastních bitových kopií v jeden účet úložiště na oblast Azure a jejich použití toocreate stovky virtuálních počítačů v hello stejného předplatného. Další informace o spravovaných disky, najdete v tématu hello [přehled disky spravované](../articles/virtual-machines/windows/managed-disks-overview.md).

Doporučujeme použít Azure spravované disky pro nové virtuální počítače, a převést vaše předchozí nespravované disky toomanaged disky, tootake výhod hello mnoho funkcí dostupných ve spravované disky.

### <a name="disk-comparison"></a>Porovnání disků

Hello následující tabulka obsahuje porovnání Premium vs Standard pro obě nespravované a spravované toohelp disky můžete rozhodnout, jaké toouse.

|    | Disk Azure typu Premium | Disk Azure typu Standard |
|--- | ------------------ | ------------------- |
| Typ disku | SSD | HDD  |
| Přehled  | Založený na jednotkách SSD; poskytuje podporu vysoce výkonných disků s nízkou latencí pro virtuální počítače, na kterých se spouští náročné vstupně-výstupní úlohy nebo které hostují kriticky důležité produkční prostředí. | Založený na jednotkách HDD; poskytuje nákladově efektivní podporu pro scénáře vývoje nebo testování virtuálních počítačů. |
| Scénář  | Úlohy v produkčním prostředí a úlohy, u kterých záleží na výkonu | Vývoj a testování, nekritické úlohy, <br>úlohy s nepravidelným přístupem |
| Velikost disku | P4: 32 GB (pouze spravované disků)<br>P6: 64 GB (pouze spravované disků)<br>P10: 128 GB<br>P20: 512 GB<br>P30: 1 024 GB<br>P40: 2 048 GB<br>P50: 4095 GB | Nespravované disky: 1 GB – 4 TB (4095 GB) <br><br>Managed Disks:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1 024 GB <br>S40: 2 048 GB<br>S50: 4095 GB| 
| Maximální propustnost na disk | 250 MB/s | 60 MB/s | 
| Maximum vstupně-výstupních operací za sekundu (IOPS) na disk | 7500 IOPS | 500 IOPS | 

