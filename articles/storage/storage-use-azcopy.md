---
title: "Zkopírovat nebo přesunout data do služby Azure Storage s AzCopy v systému Windows | Microsoft Docs"
description: "Přesunutí nebo zkopírování dat z objektu blob, table a obsah souboru nebo pomocí AzCopy na nástroj systému Windows. Kopírování dat do úložiště Azure z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Snadno migrujte data do úložiště Azure."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 045778822022752295bb634bdf734daaf36ab938
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a>Přenos dat pomocí AzCopy v systému Windows
AzCopy je nástroj příkazového řádku pro kopírování dat do a z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon. Data můžete zkopírovat z jednoho objektu do druhého v rámci účtu úložiště nebo mezi účty úložiště.

Existují dvě verze nástroje AzCopy, které si můžete stáhnout. AzCopy v systému Windows je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows styl. [AzCopy v systému Linux](storage-use-azcopy-linux.md) sestavena pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku. Tento článek se zabývá AzCopy v systému Windows.

## <a name="download-and-install-azcopy"></a>Stáhněte a nainstalujte AzCopy
### <a name="azcopy-on-windows"></a>AzCopy ve Windows
Stažení [nejnovější verzi AzCopy v systému Windows](http://aka.ms/downloadazcopy).

#### <a name="installation-on-windows"></a>Instalace v systému Windows
Po instalaci nástroje AzCopy pomocí Instalační služby systému Windows, otevřete okno příkazového řádku a přejděte do instalačního adresáře nástroje AzCopy ve vašem počítači - kde `AzCopy.exe` spustitelný soubor se nachází. V případě potřeby můžete přidat umístění instalace AzCopy cestu v systému. Ve výchozím nastavení, je nainstalován nástroj AzCopy k `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` nebo `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Zápis vaše první příkaz AzCopy
Základní syntaxe pro příkazy AzCopy je:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

Následující příklady ukazují celou řadu scénářů pro kopírování dat do a z tabulek, souborů a objektů BLOB služby Microsoft Azure. Odkazovat [AzCopy parametry](#azcopy-parameters) části Podrobné vysvětlení parametrů použitých v každém vzorku.

## <a name="blob-download"></a>Objekt BLOB: stažení
### <a name="download-single-blob"></a>Stáhnout jediného objektu blob

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Všimněte si, že pokud složce `C:\myfolder` neexistuje, bude AzCopy jej vytvořte a stáhnout `abc.txt ` do nové složky.

### <a name="download-single-blob-from-secondary-region"></a>Stáhnout jediného objektu blob ze sekundární oblasti

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="download-all-blobs"></a>Stažení všech objektů BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operaci adresáři `C:\myfolder` bude obsahovat následující soubory:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Pokud nezadáte možnost `/S`, budou staženy žádné objekty BLOB.

### <a name="download-blobs-with-specified-prefix"></a>Stáhnout objekty BLOB se zadanou předponou.

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru. Všechny objekty BLOB začínající předponou `a` budou staženy:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operaci složce `C:\myfolder` bude obsahovat následující soubory:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Předpona, která se vztahuje na virtuální adresář, který tvoří první část názvu objektu blob. Ve výše uvedeném příkladu virtuální adresář neodpovídá zadané předpony, takže nebude stažen. Kromě toho pokud možnost `\S` není zadán, AzCopy nebude stáhnout všechny objekty BLOB.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Nastavte čas poslední úpravy exportované soubory na stejnou hodnotu jako zdroj objektů BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Můžete také vyloučit objekty BLOB z operace stažení podle jejich čas poslední změny. Pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je například stejnou nebo novější, než cílový soubor, přidejte `/XN` možnost:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Nebo pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je na stejné nebo starší než cílový soubor, přidejte `/XO` možnost:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a>Objekt BLOB: nahrát
### <a name="upload-single-file"></a>Odeslat jedním souborem

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Pokud zadaný cílový kontejner neexistuje, bude AzCopy jej vytvořte a nahrajte soubor do ní.

### <a name="upload-single-file-to-virtual-directory"></a>Jediný soubor nahrát do virtuálního adresáře

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Pokud zadaný virtuální adresář neexistuje, bude AzCopy nahrání souboru ve svém názvu virtuálního adresáře (*například*, `vd/abc.txt` v předchozím příkladu).

### <a name="upload-all-files"></a>Odeslat všechny soubory.

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Zadání `/S` odešle obsah zadaného adresáře k rekurzivnímu úložiště objektů Blob, což znamená, že všechny podsložky a jejich soubory, nebude možné odesílat i. Například předpokládejme následující soubory jsou umístěny ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po operaci odeslání kontejneru bude obsahovat následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy nebude odesílat rekurzivně. Po operaci odeslání kontejneru bude obsahovat následující soubory:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Předpokládejme následující soubory jsou umístěny ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po operaci odeslání kontejneru bude obsahovat následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy odešlete pouze objekty BLOB, které nejsou jsou umístěny ve virtuálním adresáři:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Zadejte typ MIME obsahu cílový objekt blob
Ve výchozím nastavení, AzCopy nastaví typ obsahu cílový objekt blob do `application/octet-stream`. Počínaje verzí 3.1.0, lze explicitně zadat typ obsahu prostřednictvím možnosti `/SetContentType:[content-type]`. Tuto syntaxi nastaví typ obsahu pro všechny objekty BLOB v operaci odeslání.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Pokud zadáte `/SetContentType` bez hodnoty, pak AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle jeho přípony.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a>Objekt BLOB: kopírování
### <a name="copy-single-blob-within-storage-account"></a>Zkopírujte jediného objektu blob v rámci účtu úložiště

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Při kopírování objektu blob v účtu úložiště, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-single-blob-across-storage-accounts"></a>Zkopírujte jediného objektu blob mezi různými účty úložiště

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Při kopírování objektu blob mezi různými účty úložiště [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Zkopírujte jediného objektu blob ze sekundární oblasti primární oblasti

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Po operaci kopírování bude obsahovat cílový kontejner objektu blob a jeho snímků. Za předpokladu, že objektu blob ve výše uvedeném příkladu má dva snímky, bude obsahovat kontejneru následující objektů blob a snímky:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchronně kopírovat mezi různými účty úložiště objektů BLOB
AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně. Proto kopírování se spustí v pozadí využití kapacity přebytečné šířky pásma, který má žádné SLA z hlediska jak rychle bude zkopírován do objektu blob a AzCopy bude průběžně kontrolovat stav kopírování, dokud nebude kopírování je dokončena nebo se nezdařilo.

`/SyncCopy` Možnost zajistí, že operace kopírování získá konzistentní rychlost. AzCopy provede synchronní kopie stažením objektů blob pro kopírování ze zadaného zdroje do místní paměti a uložte je do cílového umístění úložiště objektů Blob.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`může vygenerovat další odchozí nákladů ve srovnání s asynchronní kopírování, doporučuje se tuto možnost použít ve virtuálním počítači Azure, který je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.

## <a name="file-download"></a>Soubor: stažení
### <a name="download-single-file"></a>Stáhnout jedním souborem

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Pokud je zadaný zdroj sdílenou složku Azure, pak buď musíte zadat přesný název souboru, (*například* `abc.txt`) ke stažení jeden soubor nebo zadejte možnost `/S` ke stažení všechny soubory ve sdílené složce rekurzivně. Probíhá pokus o zadat šablonu souboru a možnost `/S` společně dojde chybě.

### <a name="download-all-files"></a>Stáhnout všechny soubory

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Všimněte si, že všechny prázdné složky nebudou staženy.

## <a name="file-upload"></a>Soubor: nahrát
### <a name="upload-single-file"></a>Odeslat jedním souborem

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a>Odeslat všechny soubory.

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Všimněte si, že všechny prázdné složky nebude odeslána.

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a>Soubor: kopírování
### <a name="copy-across-file-shares"></a>Kopírování mezi sdílenými složkami

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-from-file-share-to-blob"></a>Kopírování ze sdílené složky do objektu blob

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Při kopírování souboru ze sdílené složky do objektu blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.


### <a name="copy-from-blob-to-file-share"></a>Zkopírujte z objektu blob do sdílené složky

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Při kopírování souboru z objektu blob do sdílené složky [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="synchronously-copy-files"></a>Synchronně kopírování souborů
Můžete zadat `/SyncCopy` možnost ke zkopírování dat z úložiště souborů do úložiště File, ze souboru úložiště do úložiště objektů Blob a z úložiště objektů Blob k úložišti souborů synchronně, AzCopy k tomu stahování zdrojová data do místní paměti a nahrajte ho znovu do cílového umístění. Standardní výstupní náklady budou platit.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Při kopírování ze souboru úložiště do úložiště objektů Blob, výchozí typ objektu blob je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` Chcete-li změnit typ cílového objektu blob.

Všimněte si, že `/SyncCopy` může generovat další odchozí náklady srovnáním asynchronní kopírování, doporučuje se tuto možnost použít ve virtuálním počítači Azure, která je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.

## <a name="table-export"></a>Tabulka: Export
### <a name="export-table"></a>Export tabulky

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy zapíše soubor manifestu do určené cílové složky. Soubor manifestu se používá v procesu importu vyhledat potřebná data souborů a provést ověření dat. Soubor manifestu používá ve výchozím nastavení této zásady vytváření názvů:

    <account name>_<table name>_<timestamp>.manifest

Uživatele můžete také zadat možnost `/Manifest:<manifest file name>` nastavit název souboru manifestu.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a>Export rozdělené do několika souborů

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

Používá AzCopy *svazku index* v názvech souborů dat rozdělení k rozlišení více souborů. Index založený na svazku se skládá ze dvou částí *index klíče rozsahu oddílu* a *rozdělení souboru index*. Obě indexy jsou od nuly.

Index klíče rozsahu oddílu bude 0, pokud uživatel není nastavena možnost `/PKRS`.

Předpokládejme například, AzCopy generuje dva soubory dat, až uživatel vybere možnost `/SplitSize`. Výsledná data názvy souborů může být:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Všimněte si, že minimální možné hodnoty pro možnost `/SplitSize` je 32 MB. Pokud je Zadaná cílová úložiště objektů Blob, budou AzCopy rozdělení datový soubor po jeho velikosti dosáhne omezení velikosti objektu blob (200GB), bez ohledu na tom, jestli možnost `/SplitSize` byla zadána uživatelem.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Export tabulky do formátu dat JSON nebo sdílený svazek clusteru
AzCopy ve výchozím nastavení exportuje tabulky soubory dat JSON. Můžete zadat možnost `/PayloadFormat:JSON|CSV` Export tabulky jako JSON nebo CSV.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Při zadávání Formát datové části sdíleného svazku clusteru, AzCopy bude také generovat soubor schématu s příponou `.schema.csv` pro každý datový soubor.

### <a name="export-table-entities-concurrently"></a>Export tabulky souběžně entity

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy spustí souběžných operací pro export entity, když uživatel vybere možnost `/PKRS`. Každé operace exportuje jeden rozsah klíče oddílu.

Všimněte si, že počet souběžných operací se řídí možnost `/NC`. AzCopy používá jako výchozí hodnota počet jader procesorů `/NC` při kopírování entity tabulky, i když `/NC` nebyl zadán. Když uživatel vybere možnost `/PKRS`, AzCopy používá menší ze dvou hodnot – rozsahy klíčů oddílu versus implicitně nebo explicitně zadaná souběžných operací – k určení počtu souběžných operací spustit. Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.

### <a name="export-table-to-blob"></a>Export tabulky do objektu blob

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy vygeneruje soubor dat JSON do kontejneru objektů blob s následující zásady vytváření názvů:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Vygenerovaný soubor dat JSON následuje Formát datové části pro – minimální metadata. Podrobnosti o této Formát datové části v tématu [Formát datové části pro operace služby s tabulkou](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Všimněte si, že při exportu tabulky do objektů BLOB, AzCopy se stáhne entity tabulky do místní dočasné datové soubory a pak nahrajte tyto entity do objektu blob. Tyto soubory dočasná data budou umístěny do složky souboru deníku s výchozí cestou "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", můžete zadat možnost/Z: [deníku – soubor a složka] deník změnit umístění složky souborů a proto změňte umístění souborů dočasná data. Velikost dočasné datové soubory, které je určeno podle velikosti entity tabulky a velikost, který jste zadali pomocí /SplitSize možnost, i když soubor dočasná data v místním disku budou odstraněny okamžitě, až se nahrají na objekt blob, ujistěte se, že máte dostatek místa na místním disku k ukládání těchto souborů dočasná data před jejich odstraněním.

## <a name="table-import"></a>Tabulka: Import
### <a name="import-table"></a>Tabulky import

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Možnost `/EntityOperation` určuje způsob vložení entity do tabulky. Možné hodnoty:

* `InsertOrSkip`: Přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.
* `InsertOrMerge`: Sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce.
* `InsertOrReplace`: Nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.

Všimněte si, že nelze zadat možnost `/PKRS` ve scénáři importu. Na rozdíl od export scénář, ve kterém musíte zadat možnost `/PKRS` zahájíte souběžných operací AzCopy ve výchozím nastavení spustí souběžných operací při importu tabulky. Výchozí počet souběžných operací spuštění se rovná počet jader procesorů; Můžete však zadat jiný počet souběžných s možností `/NC`. Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.

Všimněte si, že AzCopy podporuje pouze import pro formát JSON, není sdílený svazek clusteru. AzCopy nepodporuje import tabulky z vytvořené uživatelem JSON a manifest soubory. Oba tyto soubory musí pocházet z tabulky exportního AzCopy. Abyste se vyhnuli chybám, prosím neupravujte exportovaný JSON a soubor manifestu.

### <a name="import-entities-to-table-using-blobs"></a>Importovat entity do tabulky pomocí objektů BLOB
Předpokládejme, kontejner objektů Blob obsahuje následující: souboru A JSON představující Azure Table a jeho doprovodné soubor manifestu.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Spuštěním následujícího příkazu pro import entity do tabulky s použitím souboru manifestu v tomto kontejneru objektů blob:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Další funkce AzCopy
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Kopírovat pouze data, která neexistuje v cílovém
`/XO` a `/XN` parametry umožňují vyloučit prostředky starší nebo novější zdroj z kopírování, v uvedeném pořadí. Pokud chcete kopírovat zdroje prostředky, které neexistují v cílovém, můžete zadat oba parametry v příkazu AzCopy:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Všimněte si, že to není podporováno zdrojové nebo cílové tabulky.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Zadejte parametry příkazového řádku pomocí souboru odpovědí

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

Parametry příkazového řádku AzCopy můžete zahrnout do souboru odpovědí. AzCopy zpracovává parametry v souboru jako v případě, kdyby byly zadány na příkazovém řádku, provádění přímé nahrazení s obsahem souboru.

Předpokládejme, soubor odpovědí s názvem `copyoperation.txt`, který obsahuje následující řádky. Každý parametr AzCopy můžete zadat na jeden řádek

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

nebo na samostatné řádky:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy se nezdaří, pokud rozložení parametru mezi dvěma čárami, jak je vidět tady pro `/sourcekey` parametr:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Zadejte parametry příkazového řádku pomocí několika soubory odezvy
Předpokládejme, soubor odpovědí s názvem `source.txt` kontejner zdroj, který určuje:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

A soubor odpovědí s názvem `dest.txt` určující cílovou složku v systému souborů:

    /Dest:C:\myfolder

A soubor odpovědí s názvem `options.txt` určující možnosti AzCopy:

    /S /Y

Volat AzCopy s těmito soubory odpovědí, které jsou umístěny v adresáři `C:\responsefiles`, použijte tento příkaz:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy zpracovává tento příkaz stejně, jako kdyby jste zahrnuli všechny jednotlivé parametry na příkazovém řádku:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Zadejte sdílený přístupový podpis (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Můžete také určit SAS URI kontejneru:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Složky pro soubor deníku
Pokaždé, když příkaz azcopy, zkontroluje existenci souboru deníku ve výchozí složce, nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti. Pokud soubor deníku neexistuje ani na jednom místě, AzCopy zpracovává operaci jako nové a generuje nový soubor deníku.

Pokud soubor deníku neexistuje, AzCopy zkontroluje, zda příkazového řádku, který můžete vložit odpovídá příkazového řádku v souboru deníku. Pokud se dva příkazové řádky shodují, obnoví AzCopy neúplné operaci. Pokud se neshodují, zobrazí se výzva k buď přepsat soubor deníku spustit novou operaci, nebo zrušit aktuální operace.

Pokud chcete použít výchozí umístění pro soubor deníku:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Pokud není možnost `/Z`, nebo zadejte možnost `/Z` bez cesta ke složce, jako v příkladu nahoře, AzCopy vytvoří soubor deníku ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Pokud soubor deníku již existuje, AzCopy obnoví operaci na základě souboru deníku.

Pokud chcete určit vlastní umístění souboru deníku:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Tento příklad vytvoří soubor deníku, pokud ještě neexistuje. Pokud neexistuje, AzCopy obnoví operaci na základě souboru deníku.

Pokud chcete pokračovat v činnosti AzCopy:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Tento příklad obnoví poslední operace, které může se nepodařilo dokončit.

### <a name="generate-a-log-file"></a>Generování souboru protokolu

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Pokud zadáte možnost `/V` bez zadání cestu k souboru na úplné protokolování, pak AzCopy vytváří protokolový soubor ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Jinak můžete vytvořit soubor protokolu v do vlastního umístění:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Všimněte si, že pokud zadáte relativní cestu následující možnost `/V`, jako například `/V:test/azcopy1.log`, pak podrobného protokolování se vytvoří v aktuálním pracovním adresáři v podsložce s názvem `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Zadejte počet souběžných operací spuštění
Možnost `/NC` určuje počet souběžných kopie operací. Ve výchozím nastavení spustí AzCopy počet souběžných operací, pokud chcete zvýšit propustnost dat přenosu. Pro operace s tabulkou počet souběžných operací se rovná počet procesorů, které máte. Pro operace objektů Blob a souboru, počet souběžných operací je rovnat 8 časy počet procesorů, které máte. Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro /NC, aby se zabránilo selhání kvůli konfliktům prostředků.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Spusťte nástroj AzCopy emulátoru úložiště Azure
Můžete spustit AzCopy proti [emulátoru úložiště Azure](storage-use-emulator.md) pro objekty BLOB:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

a tabulky:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>Parametry AzCopy
Parametry pro AzCopy jsou popsané níže. Můžete také zadat jednu z následujících příkazů z příkazového řádku nápovědu pomocí nástroje AzCopy:

* Podrobnou nápovědu příkazového řádku pro AzCopy:`AzCopy /?`
* Další informace o všech AzCopy parametr:`AzCopy /?:SourceKey`
* Příklady příkazového řádku:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Zdroj: "zdroj"
Určuje zdroj dat ze kterého chcete zkopírovat. Zdrojem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="destdestination"></a>/ Cíle: "cílové"
Určuje cíl pro kopírování. Cílem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="patternfile-pattern"></a>/ Vzor: "vzor souborů"
Určuje vzor souborů, který označuje soubory, které chcete kopírovat. Chování parametr /Pattern je určen podle umístění zdroje dat a přítomnost možnost rekurzivní režim. Prostřednictvím možnosti parametrem/s. je zadán rekurzivní režim

Pokud zadaný zdrojový adresář v systému souborů, pak jsou platné standardní zástupné znaky a vzor souborů poskytuje je shoda na základě souborů v adresáři. Pokud možnost, který je zadán parametr, pak AzCopy taky odpovídá zadanému vzoru proti všechny soubory ve všech podsložek pod adresářem.

Pokud zadaný zdroj je kontejner objektů blob nebo virtuální adresář, nejsou použity zástupné znaky. Pokud je možnost, který je zadán parametr, pak AzCopy interpretuje jako předpona objektu blob vzoru zadaný soubor. Pokud možnosti, kterou není zadán parametr, pak AzCopy odpovídá vzoru souboru s názvy objektů blob přesný.

Pokud se zadaný zdroj sdílenou složku Azure, pak musí buď zadejte přesný název souboru (například abc.txt) zkopírovat jeden soubor nebo zadejte možnost /S zkopíruje všechny soubory ve sdílené složce rekurzivně. Probíhá pokus o zadat šablonu souboru a možnost /S společně bude výsledkem chyba.

AzCopy používá, když / Source je kontejner objektů blob nebo virtuální adresář objektů blob a používá porovnávání ve všech ostatních případech odpovídající malá a velká písmena.

Soubor výchozím způsobem používaným při žádné vzor souborů je *.* pro umístění systému souborů nebo prázdnou předponu pro umístění služby Azure Storage. Zadání více vzorů souborů není podporováno.

**Platí pro:** objekty BLOB, soubory

### <a name="destkeystorage-key"></a>/ DestKey: "klíč úložiště"
Určuje klíč účtu úložiště pro cílový prostředek.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="destsassas-token"></a>/ DestSAS: "tokenu sas"
Určuje sdíleného přístupového podpisu (SAS) s oprávněními ke čtení a zápisu pro cíl (pokud existuje). Obklopit SAS s dvojité uvozovky, protože pravděpodobně obsahuje speciální znaky příkazového řádku.

Pokud cílový prostředek je kontejner objektů blob, sdílené složky nebo tabulky, můžete buď zadat tuto možnost, za nímž následuje tokenu SAS nebo jako součást cílový kontejner objektů blob, sdílené složky nebo identifikátor URI tabulky bez této možnosti můžete zadat SAS.

Pokud zdrojové a cílové jsou oba objekty BLOB, pak cílový objekt blob se musí nacházet ve stejném účtu úložiště jako zdrojový objekt blob.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcekeystorage-key"></a>/ SourceKey: "klíč úložiště"
Určuje klíč účtu úložiště pro zdrojovému prostředku.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcesassas-token"></a>/ SourceSAS: "tokenu sas"
Určuje sdíleného přístupového podpisu oprávnění ke čtení a seznamu pro zdroj (pokud existuje). Obklopit SAS s dvojité uvozovky, protože pravděpodobně obsahuje speciální znaky příkazového řádku.

Pokud zdroj prostředků je kontejner objektů blob a je k dispozici klíč ani SAS, budou kontejneru objektů blob číst přes anonymní přístup.

Pokud je zdroj sdílené složky nebo tabulka, je třeba zadat klíč nebo SAS.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="s"></a>/S
Určuje režim rekurzivní operace kopírování. V režimu rekurzivní AzCopy zkopíruje všechny objekty BLOB nebo soubory, které se shodují se vzorem zadaný soubor, včetně programů v podsložkách.

**Platí pro:** objekty BLOB, soubory

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blokem" | "stránka" | "připojit"
Určuje, zda cílový objekt blob je objekt blob bloku, objektů blob stránky nebo doplňovací objekt blob. Tuto možnost lze použít pouze v případě, že nahráváte do objektu blob. Jinak je generována chyba. Pokud cílový objekt blob a není tato možnost zadána, ve výchozím nastavení, AzCopy vytvoří objekt blob bloku.

**Platí pro:** objektů BLOB

### <a name="checkmd5"></a>/ CheckMD5
Vypočítá hodnotu hash MD5 pro stažená data a ověří, zda hodnota hash MD5 uložené v objektu blob nebo vlastnost MD5 obsah souboru odpovídá vypočtený hash. Kontrola MD5 je vypnutý ve výchozím nastavení, proto musíte určit tuto možnost, při stahování dat provést kontrolu MD5.

Všimněte si, že Azure Storage nezaručuje, že je aktuální hodnota hash MD5 uložené pro objekt blob, nebo soubor. Je zodpovědností klienta aktualizovat MD5 změně objektů blob nebo souboru je.

AzCopy vždy nastaví vlastnost obsah MD5 pro objektů blob v Azure nebo soubor nahráním do služby.  

**Platí pro:** objekty BLOB, soubory

### <a name="snapshot"></a>/ Snímku
Označuje, zda přenos snímky. Tato možnost je platná, pouze pokud je zdroj objektu blob.

Přejmenování snímky přenášená objektů blob v tomto formátu: .extension název objektu blob (snímku čas)

Ve výchozím nastavení nebudou zkopírovány snímky.

**Platí pro:** objektů BLOB

### <a name="vverbose-log-file"></a>/ V: [podrobné souboru protokolu]
Výstupy podrobné stavové zprávy do souboru protokolu.

Ve výchozím nastavení, je soubor podrobného protokolování s názvem AzCopyVerbose.log v `%LocalAppData%\Microsoft\Azure\AzCopy`. Pokud zadáte existující umístění souborů pro tuto možnost, připojí se k tomuto souboru podrobného protokolování.  

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="zjournal-file-folder"></a>/ Z: [deníku – soubor a složka]
Určuje složku souboru deníku pro operace obnovení.

AzCopy vždy podporuje obnovení, pokud operace byla přerušena.

Pokud není tato možnost zadána, nebo je zadán bez cestu ke složce, pak AzCopy vytvoří ve výchozím umístění, což je % LocalAppData%\Microsoft\Azure\AzCopy soubor deníku.

Pokaždé, když příkaz azcopy, zkontroluje existenci souboru deníku ve výchozí složce, nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti. Pokud soubor deníku neexistuje ani na jednom místě, AzCopy zpracovává operaci jako nové a generuje nový soubor deníku.

Pokud soubor deníku neexistuje, AzCopy zkontroluje, zda příkazového řádku, který můžete vložit odpovídá příkazového řádku v souboru deníku. Pokud se dva příkazové řádky shodují, obnoví AzCopy neúplné operaci. Pokud se neshodují, zobrazí se výzva k buď přepsat soubor deníku spustit novou operaci, nebo zrušit aktuální operace.

Soubor deníku se odstraní po úspěšném dokončení operace.

Všimněte si, že obnovení ze souboru deníku vytvořeného v předchozí verzi AzCopy operace není podporována.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="parameter-file"></a>/@:"Parameter-File"
Určuje soubor, který obsahuje parametry. AzCopy zpracovává parametry v souboru stejně, jako kdyby kdyby byly zadány na příkazovém řádku.

V souboru odpovědí můžete zadat několik parametrů na jeden řádek nebo zadejte každý parametr na samostatném řádku. Všimněte si, že jednotlivé parametr nemůže zahrnovat více řádků.

Soubory odezvy může zahrnovat komentáře řádky, které začínají symbolem #.

Můžete zadat několik souborů odpovědi. Všimněte si však, že AzCopy nepodporuje soubory vnořené odezvy.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="y"></a>/Y
Potlačí všechny výzvy potvrzení AzCopy.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="l"></a>/ L
Určuje operaci výpis pouze; žádná data budou zkopírována.

AzCopy bude interpretovat použití této možnosti simulace pro spuštění příkazového řádku bez možnosti/l a počet, kolik objekty se zkopírují, můžete zadat možnost /V ve stejnou dobu a zkontrolujte, které objekty se zkopírují v podrobného protokolování.

Chování tato možnost je určena také umístění zdroje dat a přítomnost rekurzivní možnost /S a soubor vzor možnost režim /Pattern.

AzCopy vyžaduje oprávnění seznamu a přečtěte si toto umístění zdroje, při použití této možnosti.

**Platí pro:** objekty BLOB, soubory

### <a name="mt"></a>/ MT
Nastaví čas poslední změny stažený soubor být stejný jako zdrojový objekt blob nebo souboru.

**Platí pro:** objekty BLOB, soubory

### <a name="xn"></a>/XN
Vyloučí novější zdrojovému prostředku. Prostředek se nezkopírují, pokud čas poslední změny zdroje je stejná nebo novější než cílový.

**Platí pro:** objekty BLOB, soubory

### <a name="xo"></a>/XO
Vyloučí starší zdrojovému prostředku. Prostředek se nezkopírují, pokud čas poslední změny zdroje je stejná nebo starší než cílový.

**Platí pro:** objekty BLOB, soubory

### <a name="a"></a>/A
Ukládání pouze soubory, které mají atribut Archivovat nastaven.

**Platí pro:** objekty BLOB, soubory

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]
Ukládání pouze soubory, které mají všechny zadané atributy sady.

Dostupné atributy patří:

* R = soubory jen pro čtení
* A = soubory připravené k archivaci
* S = systémové soubory
* H = skryté soubory
* C = komprimované soubory
* N = normální soubory
* E = šifrovaných souborů
* T = dočasné soubory
* O = Offline soubory
* I = není indexované soubory

**Platí pro:** objekty BLOB, soubory

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]
Vyloučí soubory, které mají všechny zadané atributy sady.

Dostupné atributy patří:

* R = soubory jen pro čtení
* A = soubory připravené k archivaci
* S = systémové soubory
* H = skryté soubory
* C = komprimované soubory
* N = normální soubory
* E = šifrovaných souborů
* T = dočasné soubory
* O = Offline soubory
* I = není indexované soubory

**Platí pro:** objekty BLOB, soubory

### <a name="delimiterdelimiter"></a>/ Oddělovač: "oddělovač.
Označuje znak oddělovač použitý pro vymezení virtuálních adresářů v název objektu blob.

Ve výchozím nastavení používá AzCopy / jako oddělovací znak. Ale AzCopy podporuje používání libovolného znaku běžné (například @, #, nebo %) jako oddělovač. Pokud potřebujete použít jeden z těchto speciálních znaků v příkazovém řádku, uzavřete název souboru s dvojité uvozovky.

Tato možnost platí pouze pro stahování objekty BLOB.

**Platí pro:** objektů BLOB

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "číslo z souběžných operace"
Určuje počet souběžných operací.

AzCopy ve výchozím nastavení spustí počet souběžných operací, pokud chcete zvýšit propustnost dat přenosu. Všimněte si, že velký počet souběžných operací v prostředí s malou šířkou pásma může zahlcovat síťové připojení a zabránit v plně dokončení operace. Omezení souběžných operací podle skutečné dostupnou šířku pásma sítě.

Horní limit pro souběžných operací je 512.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tabulka"
Určuje, že `source` prostředek je k dispozici v místním vývojovém prostředí, spouštění v emulátoru úložiště objektu blob.

**Platí pro:** objekty BLOB, tabulek

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tabulka"
Určuje, že `destination` prostředek je k dispozici v místním vývojovém prostředí, spouštění v emulátoru úložiště objektu blob.

**Platí pro:** objekty BLOB, tabulek

### <a name="pkrskey1key2key3"></a>/ PKRS: "key&#1;key&#2; klíč&#3;..."
Rozdělí rozsah klíče oddílu Povolit export dat v tabulce současně, což zvyšuje rychlost operace exportu.

Pokud není tato možnost zadána, AzCopy používá jedno vlákno pro export entity tabulky. Například pokud uživatel zadá /PKRS: "aa #bb", pak AzCopy spustí tři souběžných operací.

Každé operace exportuje mezi tři rozsahy klíčů oddílu, jak je uvedeno níže:

  [klíč prvního oddílu, aa)

  [aa, bb)

  [bb, klíč oddílu poslední]

**Platí pro:** tabulky

### <a name="splitsizefile-size"></a>/ SplitSize: "velikost souboru"
Určuje, v exportovaném souboru rozdělení velikost v MB, je minimální povolená hodnota je 32.

Pokud není tato možnost zadána, bude AzCopy exportovat data tabulky do jednoho souboru.

Pokud data tabulky se exportují do objektu blob a velikost exportovaný soubor dosáhne 200 GB limit pro velikost objektu blob, pak AzCopy rozdělí exportovaný soubor i v případě, že není tato možnost zadána.

**Platí pro:** tabulky

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"
Určuje chování importu dat tabulky.

* InsertOrSkip - přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.
* InsertOrMerge - sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce.
* InsertOrReplace - nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.

**Platí pro:** tabulky

### <a name="manifestmanifest-file"></a>/ Manifest: "manifest – soubor"
Určuje soubor manifestu pro tabulky exportovat a importovat operaci.

Tato možnost je volitelné během exportu, AzCopy vygeneruje soubor manifestu se předdefinované názvem, pokud není tato možnost zadána.

Tato možnost je povinná během operace importu pro vyhledání datových souborů.

**Platí pro:** tabulky

### <a name="synccopy"></a>/ SyncCopy
Označuje, zda synchronně kopírování objektů BLOB nebo soubory mezi dva koncové body Azure Storage.

AzCopy ve výchozím nastavení používá asynchronní kopie straně serveru. Zadejte tuto možnost, chcete-li provést synchronní kopie, která stáhne objektů BLOB nebo soubory k místní paměti a odesílá je do úložiště Azure.

Tuto možnost můžete použít při kopírování souborů v rámci úložiště objektů Blob, úložiště File, nebo z úložiště objektů Blob k úložišti souborů nebo naopak.

**Platí pro:** objekty BLOB, soubory

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "content-type"
Určuje typ MIME pro cílové objekty BLOB nebo soubory obsahu.

AzCopy nastaví typ obsahu pro objekt blob nebo soubor na application/octet-stream ve výchozím nastavení. Typ obsahu pro všechny objekty BLOB nebo soubory můžete nastavit a explicitně zadat hodnotu pro tuto možnost.

Pokud zadáte tuto možnost bez hodnoty, bude nastavte AzCopy, každý objekt blob nebo typ obsahu souboru podle jeho přípony.

**Platí pro:** objekty BLOB, soubory

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"
Určuje formát souboru exportovaná data tabulky.

Pokud není tato možnost zadána, exportuje AzCopy ve výchozím nastavení tabulky datový soubor ve formátu JSON.

**Platí pro:** tabulky

## <a name="known-issues-and-best-practices"></a>Známé problémy a doporučené postupy
### <a name="limit-concurrent-writes-while-copying-data"></a>Limit souběžných zápisy při kopírování dat
Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být úprava dat během kopírování ho. Pokud je to možné Ujistěte se, které chcete kopírovat data nemění při kopírování. Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis na virtuální pevný disk. Dobrým způsobem, jak to udělat, je leasing prostředků, které se mají zkopírovat. Alternativně můžete nejprve vytvořte snímek virtuálního pevného disku a poté zkopírujte snímku.

Pokud nelze zabránit jiné aplikace z zápis do objektů BLOB nebo soubory, když se kopírují, pak mějte na paměti, že v době dokončení úlohy, kopírované prostředky buď již nemá úplné parita s prostředky zdroje.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Jedna instance nástroje AzCopy spusťte na jednom počítači.
AzCopy je navržen chcete maximalizovat využití prostředků vašeho počítače urychlit přenos dat, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost `/NC` Pokud potřebujete více souběžných operací. Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Povolit algoritmus MD5 kompatibilní s FIPS pro AzCopy když jste "použití standardu FIPS kompatibilní s algoritmy pro šifrování, hašování a podpisování".
AzCopy ve výchozím nastavení používá k výpočtu MD5 při kopírování objektů implementace rozhraní .NET MD5, ale existují některé požadavky na zabezpečení, které je třeba povolit nastavení MD5 kompatibilní s FIPS AzCopy.

Můžete vytvořit soubor app.config `AzCopy.exe.config` s vlastností `AzureStorageUseV1MD5` a umístí jej vyhraďte s AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Pro vlastnost "AzureStorageUseV1MD5" • True - použije výchozí hodnotu, AzCopy implementace rozhraní .NET MD5.
• False – AzCopy použije algoritmus MD5 kompatibilní se standardem FIPS.

Všimněte si, že na počítač se systémem Windows ve výchozím nastavení je zakázáno algoritmy splňující standard FIPS, můžete v okně Spustit zadejte secpol.msc a zkontrolujte tento přepínač v nastavení zabezpečení -> Místní zásady -> zabezpečení -> – Možnosti kryptografie systému: použití vyhovující standardu FIPS pro šifrování, hašování a podpisování.

## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage a AzCopy naleznete v následujících zdrojích informací.

### <a name="azure-storage-documentation"></a>Dokumentace k Azure Storage:
* [Úvod do Azure Storage](storage-introduction.md)
* [Používání úložiště Blob z rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Jak používat úložiště File z rozhraní .NET](storage-dotnet-how-to-use-files.md)
* [Používání úložiště Table z rozhraní .NET](storage-dotnet-how-to-use-tables.md)
* [Jak vytvořit, spravovat nebo odstranit účet úložiště](storage-create-storage-account.md)
* [Přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Příspěvky blogu Azure Storage:
* [Představení náhled knihovny přesun dat úložiště Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Úvod synchronní kopie a vlastní typ obsahu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Přenos dat pomocí SAS token a znovu spustit režim](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Objekt Blob kopírování mezi účet pomocí](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

