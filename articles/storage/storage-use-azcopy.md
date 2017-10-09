---
title: "aaaCopy nebo přesunout data tooAzure úložiště s AzCopy v systému Windows | Microsoft Docs"
description: "Použijte hello AzCopy v systému Windows nástroj toomove nebo kopírování dat tooor z objektu blob, table a obsah souboru. Kopírování dat tooAzure úložiště z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Vaše data tooAzure úložiště snadno migrujte."
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
ms.openlocfilehash: a77db84c3a3e06f0ad4e87d02b14a5c62ed8d9ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a>Přenos dat pomocí hello AzCopy v systému Windows
AzCopy je nástroj příkazového řádku pro kopírování tooand dat z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon. Data můžete zkopírovat z jednoho objektu tooanother v rámci účtu úložiště nebo mezi účty úložiště.

Existují dvě verze nástroje AzCopy, které si můžete stáhnout. AzCopy v systému Windows je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows styl. [AzCopy v systému Linux](storage-use-azcopy-linux.md) sestavena pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku. Tento článek se zabývá AzCopy v systému Windows.

## <a name="download-and-install-azcopy"></a>Stáhněte a nainstalujte AzCopy
### <a name="azcopy-on-windows"></a>AzCopy ve Windows
Stáhnout hello [nejnovější verzi AzCopy v systému Windows](http://aka.ms/downloadazcopy).

#### <a name="installation-on-windows"></a>Instalace v systému Windows
Po instalaci nástroje AzCopy v systému Windows pomocí instalačního programu hello, otevřete okno příkazového řádku a přejděte toohello instalační adresář AzCopy ve vašem počítači – kde hello `AzCopy.exe` spustitelný soubor se nachází. V případě potřeby můžete přidat hello AzCopy tooyour systému cesta k umístění instalace. Ve výchozím nastavení, AzCopy je nainstalován příliš`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` nebo `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Zápis vaše první příkaz AzCopy
Hello základní syntaxe pro příkazy AzCopy je:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

Hello následující příklady ukazují celou řadu scénářů kopírování data tooand z tabulek, souborů a objektů BLOB služby Microsoft Azure. Odkazovat toohello [AzCopy parametry](#azcopy-parameters) části Podrobné vysvětlení hello parametrů použitých v každém vzorku.

## <a name="blob-download"></a>Objekt BLOB: stažení
### <a name="download-single-blob"></a>Stáhnout jediného objektu blob

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Všimněte si, že pokud hello složky `C:\myfolder` neexistuje, bude AzCopy jej vytvořte a stáhnout `abc.txt ` do nové složky hello.

### <a name="download-single-blob-from-secondary-region"></a>Stáhnout jediného objektu blob ze sekundární oblasti

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="download-all-blobs"></a>Stažení všech objektů BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operaci hello hello directory `C:\myfolder` bude obsahovat hello následující soubory:

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

Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello. Všechny objekty BLOB začínající předponou hello `a` budou staženy:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operaci hello hello složky `C:\myfolder` bude obsahovat hello následující soubory:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Předpona Hello platí toohello virtuální adresář, který tvoří první část hello hello název objektu blob. Ve výše uvedeném příkladu hello virtuální adresář hello neodpovídá zadané předpony hello, takže nebude stažen. Kromě toho pokud hello možnost `\S` není zadán, AzCopy nebude stáhnout všechny objekty BLOB.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Nastavit čas poslední úpravy hello z exportované soubory toobe stejná hodnota jako hello zdroj objektů BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Můžete také vyloučit objekty BLOB z operace nástroje download hello podle jejich čas poslední změny. Příklad: Pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejnou nebo novější než hello cílový soubor, přidejte hello `/XN` možnost:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Nebo pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejné nebo starší než hello cílový soubor, přidejte hello `/XO` možnost:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a>Objekt BLOB: nahrát
### <a name="upload-single-file"></a>Odeslat jedním souborem

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Pokud hello zadaný cílový kontejner neexistuje, bude AzCopy jej vytvořte a nahrajte soubor hello do ní.

### <a name="upload-single-file-toovirtual-directory"></a>Nahrát toovirtual directory jedním souborem

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Pokud hello zadaný virtuální adresář neexistuje, bude AzCopy nahrávat hello tooinclude hello virtuální adresář souboru ve svém názvu (*například*, `vd/abc.txt` v předchozím příkladu hello).

### <a name="upload-all-files"></a>Odeslat všechny soubory.

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Zadání `/S` nahrávání hello obsah hello zadaný adresář tooBlob úložiště rekurzivně, což znamená, že všechny podsložky a jejich soubory, nebude možné odesílat i. Například předpokládejme hello následující soubory jsou umístěny ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy nebude odesílat rekurzivně. Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Předpokládejme hello následující soubory jsou umístěny ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy odešlete pouze objekty BLOB, které nejsou jsou umístěny ve virtuálním adresáři:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Zadejte hello obsahu typ MIME cílový objekt blob
Ve výchozím nastavení, AzCopy nastaví typ obsahu hello cílový objekt blob příliš`application/octet-stream`. Počínaje verzí 3.1.0, lze explicitně zadat typ obsahu hello prostřednictvím možnosti hello `/SetContentType:[content-type]`. Tuto syntaxi nastaví hello typ obsahu pro všechny objekty BLOB v operaci odeslání.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Pokud zadáte `/SetContentType` bez hodnoty, pak AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle tooits příponu souboru.

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

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Zkopírujte jediného objektu blob ze sekundární oblasti tooprimary oblasti

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Po operaci kopírování hello bude obsahovat hello cílový kontejner objektů blob hello a jeho snímků. Za předpokladu, že objektu blob hello v předchozím příkladu hello má dva snímky, hello kontejner bude zahrnovat následující hello objektů blob a snímky:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchronně kopírovat mezi různými účty úložiště objektů BLOB
AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně. Proto se operace kopírování hello spustí hello pozadí využití kapacity přebytečné šířky pásma, který má žádné SLA z hlediska jak rychlé se zkopírují do objektu blob a AzCopy bude průběžně kontrolovat stav kopie hello, dokud kopírování hello je dokončena nebo se nezdařilo.

Hello `/SyncCopy` možnost zajistí, že operace kopírování hello získá konzistentní rychlost. AzCopy provede synchronní kopie hello tak, že stáhnete objekty BLOB hello toocopy z hello zadaný zdroj toolocal paměti a odesílání je toohello cíl úložiště objektů Blob.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`může generovat další odchozí náklady porovnání tooasynchronous kopie hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.

## <a name="file-download"></a>Soubor: stažení
### <a name="download-single-file"></a>Stáhnout jedním souborem

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (*například* `abc.txt`) toodownload jeden soubor, nebo zadejte možnost `/S` toodownload všechny soubory ve sdílené složce hello rekurzivní. Probíhá pokus toospecify vzor souborů i možnost `/S` společně dojde chybě.

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

### <a name="copy-from-file-share-tooblob"></a>Zkopírujte z tooblob sdílené složky souborů

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Při kopírování souboru ze souboru tooblob sdílenou složku, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.


### <a name="copy-from-blob-toofile-share"></a>Zkopírujte ze složky toofile objektů blob

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Při kopírování souboru ze sdílené složky toofile objektů blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="synchronously-copy-files"></a>Synchronně kopírování souborů
Můžete zadat hello `/SyncCopy` možnost toocopy data z úložiště File tooFile úložiště, ze souboru úložiště tooBlob úložiště a z úložiště objektů Blob tooFile úložiště synchronně, AzCopy k tomu stahování hello zdroje dat toolocal paměti a nahrajte ho znovu toodestination. Standardní výstupní náklady budou platit.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Při kopírování ze souboru úložiště tooBlob úložiště, typu blob výchozí hello je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` toochange hello cílový objekt blob typu.

Všimněte si, že `/SyncCopy` může generovat další odchozí náklady porovnáním různých tooasynchronous kopírování, hello doporučený přístup je tato možnost v hello virtuálního počítače Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.

## <a name="table-export"></a>Tabulka: Export
### <a name="export-table"></a>Export tabulky

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy zapíše soubor manifestu toohello zadanou cílovou složku. Soubor manifestu Hello se používá v hello import proces toolocate hello potřebná data souborů a provádět ověřování dat. Soubor manifestu Hello používá hello následující zásady vytváření názvů ve výchozím nastavení:

    <account name>_<table name>_<timestamp>.manifest

Uživatele můžete také zadat možnost hello `/Manifest:<manifest file name>` název souboru manifestu tooset hello.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a>Export rozdělené do několika souborů

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

Používá AzCopy *svazku index* v hello rozdělení dat názvy souborů toodistinguish více souborů. index svazku Hello se skládá ze dvou částí *index klíče rozsahu oddílu* a *rozdělení souboru index*. Obě indexy jsou od nuly.

index klíče rozsahu oddílu Hello bude 0, pokud uživatel není nastavena možnost `/PKRS`.

Předpokládejme například, AzCopy generuje dva soubory dat po hello uživatel zadá možnost `/SplitSize`. Hello Výsledná data názvy souborů, může být:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Všimněte si, že hello minimální možné hodnoty pro možnost `/SplitSize` je 32 MB. Pokud hello zadaný cíl je úložiště objektů Blob, budou AzCopy rozdělení hello datový soubor po jeho velikosti dosáhnou hello blob velikost omezenou (200GB), bez ohledu na tom, jestli možnost `/SplitSize` byla zadána uživatelem hello.

### <a name="export-table-toojson-or-csv-data-file-format"></a>Export tabulky tooJSON nebo datový formát souboru CSV
AzCopy ve výchozím nastavení exportuje tabulky tooJSON datových souborů. Můžete zadat možnost hello `/PayloadFormat:JSON|CSV` tooexport hello tabulky jako JSON nebo CSV.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Při zadávání Formát datové části hello sdíleného svazku clusteru, AzCopy bude také generovat soubor schématu s příponou `.schema.csv` pro každý datový soubor.

### <a name="export-table-entities-concurrently"></a>Export tabulky souběžně entity

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy se spustí souběžných operací tooexport entity, když uživatel hello určuje možnost `/PKRS`. Každé operace exportuje jeden rozsah klíče oddílu.

Všimněte si, že hello počet souběžných operací se řídí možnost `/NC`. AzCopy používá jako výchozí hodnota hello hello počet jader procesorů `/NC` při kopírování entity tabulky, i když `/NC` nebyl zadán. Když uživatel hello určuje možnost `/PKRS`, AzCopy používá menší hello dvě hodnoty - oddílu rozsahy klíčů versus implicitně nebo explicitně zadané souběžných operací - toodetermine hello počtu souběžných operací toostart hello. Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.

### <a name="export-table-tooblob"></a>Export tabulky tooblob

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy vygeneruje soubor dat JSON do kontejneru objektů blob hello následující zásady vytváření názvů:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Soubor dat JSON Hello generované následuje hello Formát datové části pro – minimální metadata. Podrobnosti o této Formát datové části v tématu [Formát datové části pro operace služby s tabulkou](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Upozorňujeme, že při exportu tabulky tooblobs, AzCopy bude stažení hello tabulka entity toolocal dočasná data souborů a potom nahrát objekt blob se tyto entity toohello. Tyto soubory dočasná data, která jsou umístěny do hello deníku soubor, složku s cestou výchozí hello "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", můžete zadat možnost/toochange [deníku – soubor a složka] Z: hello umístění složky se souborem deníku a proto změnit umístění souborů hello dočasná data. Hello dočasná data, velikost souborů je určeno podle velikosti entity tabulky a hello velikost, který jste zadali pomocí /SplitSize možnost hello, i když hello dočasných dat souboru v místním disku budou odstraněny okamžitě poté, co byl nahrán objekt blob toohello, Zkontrolujte prosím, že jste máte dostatek místa toostore místního disku tyto soubory dočasná data před jejich odstraněním.

## <a name="table-import"></a>Tabulka: Import
### <a name="import-table"></a>Tabulky import

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Hello možnost `/EntityOperation` Určuje, jak hello tooinsert entity do tabulky. Možné hodnoty:

* `InsertOrSkip`: Přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.
* `InsertOrMerge`: Sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.
* `InsertOrReplace`: Nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.

Všimněte si, že nelze zadat možnost `/PKRS` ve scénáři import hello. Na rozdíl od hello export scénář, ve kterém musíte zadat možnost `/PKRS` toostart souběžných operací, AzCopy ve výchozím nastavení spustí souběžných operací při importu tabulky. Hello výchozí počet souběžných operací spuštění je rovna toohello počet procesorů jádra; Můžete však zadat jiný počet souběžných s možností `/NC`. Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.

Všimněte si, že AzCopy podporuje pouze import pro formát JSON, není sdílený svazek clusteru. AzCopy nepodporuje import tabulky z vytvořené uživatelem JSON a manifest soubory. Oba tyto soubory musí pocházet z tabulky exportního AzCopy. chyby tooavoid, neprovádějte žádné změny hello exportovat JSON nebo soubor manifestu.

### <a name="import-entities-tootable-using-blobs"></a>Import entity tootable použití objektů BLOB
Předpokládejme, kontejner objektů Blob obsahuje následující hello: souboru A JSON představující Azure Table a jeho doprovodné soubor manifestu.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Můžete spustit následující příkaz tooimport entity do tabulky pomocí souboru manifestu hello v tomto kontejneru objektů blob hello:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Další funkce AzCopy
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Kopírovat pouze data, která neexistuje v cílovém umístění hello
Hello `/XO` a `/XN` parametry umožňují tooexclude starší nebo novější zdroj prostředky z kopírování, v uvedeném pořadí. Pokud chcete pouze prostředky toocopy zdroje, které neexistují v cílovém umístění hello, můžete zadat oba parametry v hello AzCopy příkaz:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Všimněte si, že to není podporováno hello zdrojové nebo cílové tabulky.

### <a name="use-a-response-file-toospecify-command-line-parameters"></a>Použít parametrů souboru odpovědí toospecify příkazového řádku

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

Parametry příkazového řádku AzCopy můžete zahrnout do souboru odpovědí. AzCopy procesy hello parametry v souboru hello jako v případě, kdyby byly zadány na příkazovém řádku hello, provádění přímé nahrazení s hello obsah souboru hello.

Předpokládejme, soubor odpovědí s názvem `copyoperation.txt`, který obsahuje následující řádky hello. Každý parametr AzCopy můžete zadat na jeden řádek

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

nebo na samostatné řádky:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy se nezdaří, pokud rozdělení hello parametr mezi dvěma čárami, jak je vidět tady pro hello `/sourcekey` parametr:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a>Používání více odpovědi soubory toospecify parametrů příkazového řádku
Předpokládejme, soubor odpovědí s názvem `source.txt` kontejner zdroj, který určuje:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

A soubor odpovědí s názvem `dest.txt` specifikuje cílovou složku v systému souborů hello:

    /Dest:C:\myfolder

A soubor odpovědí s názvem `options.txt` určující možnosti AzCopy:

    /S /Y

toocall AzCopy s těmito soubory odpovědí, což jsou umístěny v adresáři `C:\responsefiles`, použijte tento příkaz:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy zpracovává tento příkaz stejně, jako kdyby jste zahrnuli všechny hello jednotlivé parametry na příkazovém řádku hello:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Zadejte sdílený přístupový podpis (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Můžete také zadat SAS na kontejneru hello URI:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Složky pro soubor deníku
Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti. Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.

Pokud soubor deníku hello neexistuje, AzCopy zkontroluje, zda hello příkazového řádku, které můžete vložit odpovídá hello příkazového řádku v souboru deníku hello. Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace. Pokud se neshodují, bude výzvami tooeither přepsat hello deníku souboru toostart operaci nového nebo toocancel hello aktuální operace.

Pokud chcete, aby toouse hello výchozí umístění souboru deníku hello:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Pokud není možnost `/Z`, nebo zadejte možnost `/Z` bez hello cesta ke složce, jako v příkladu nahoře, AzCopy hello deníku soubor vytvoří v hello výchozího umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Pokud hello deníku soubor již existuje, AzCopy obnoví hello operaci na základě souboru deníku hello.

Pokud chcete, aby toospecify vlastní umístění souboru deníku hello:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Tento příklad vytvoří soubor deníku hello, pokud ještě neexistuje. Pokud neexistuje, AzCopy obnoví hello operaci na základě souboru deníku hello.

Pokud chcete, aby tooresume AzCopy operace:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Tento příklad obnoví hello poslední operace, které se pravděpodobně nezdařila toocomplete.

### <a name="generate-a-log-file"></a>Generování souboru protokolu

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Pokud zadáte možnost `/V` bez zadání protokolu podrobné toohello cesta k souboru, potom AzCopy vytvoří soubor protokolu hello v hello výchozího umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Jinak můžete vytvořit soubor protokolu v do vlastního umístění:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Všimněte si, že pokud zadáte relativní cestu následující možnost `/V`, jako například `/V:test/azcopy1.log`, pak hello podrobného protokolování se vytvoří v hello aktuální pracovní adresář v podsložce s názvem `test`.

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Zadejte hello počet souběžných operací toostart
Možnost `/NC` určuje hello počet souběžných kopie operací. Ve výchozím nastavení spustí AzCopy počet propustnost přenosu dat hello tooincrease souběžných operací. Pro operace s tabulkou hello počet souběžných operací je rovna toohello počet procesorů, které máte. Pro objekt Blob a soubor operace, hello počet souběžných operací je rovnat 8 časy hello počet procesorů, které máte. Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro /NC tooavoid selhání kvůli konfliktům prostředků.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Spusťte nástroj AzCopy emulátoru úložiště Azure
AzCopy můžete spustit proti hello [emulátoru úložiště Azure](storage-use-emulator.md) pro objekty BLOB:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

a tabulky:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>Parametry AzCopy
Parametry pro AzCopy jsou popsané níže. Můžete také zadat jednu z následujících příkazů z příkazového řádku hello nápovědu pomocí nástroje AzCopy hello:

* Podrobnou nápovědu příkazového řádku pro AzCopy:`AzCopy /?`
* Další informace o všech AzCopy parametr:`AzCopy /?:SourceKey`
* Příklady příkazového řádku:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Zdroj: "zdroj"
Určuje hello zdrojová data z které toocopy. Hello zdroj může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="destdestination"></a>/ Cíle: "cílové"
Určuje cílový toocopy hello k. Hello cílem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="patternfile-pattern"></a>/ Vzor: "vzor souborů"
Určuje vzor souborů, která určuje, které toocopy soubory. chování Hello hello /Pattern parametru je určen podle umístění hello hello zdroje dat a hello přítomnost hello rekurzivní v uživatelském režimu. Prostřednictvím možnosti parametrem/s. je zadán rekurzivní režim

Pokud hello zadaný zdrojový adresář v systému souborů hello, pak platí standardní zástupné znaky, a zadat šablonu souboru hello je shoda na základě souborů v adresáři hello. Pokud možnost, který je zadán parametr, pak AzCopy taky odpovídá zadanému vzoru hello proti všechny soubory ve všech podsložek pod adresářem hello.

Pokud je zadaný zdroj hello kontejner objektů blob nebo virtuální adresář, nejsou použity zástupné znaky. Pokud je možnost, který je zadán parametr, pak AzCopy hello zadaný soubor vzor interpretuje jako předpona objektu blob. Pokud možnosti, kterou není zadán parametr, pak AzCopy odpovídá hello vzor souborů s názvy objektů blob přesný.

Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (například abc.txt) toocopy jeden soubor, nebo zadejte možnost /S toocopy všechny soubory v rekurzivně hello sdílené složky. Pokus toospecify vzor souborů i možnost /S společně bude mít za následek chybu.

AzCopy používá, když hello/Source je kontejner objektů blob nebo virtuální adresář objektů blob a hello používá velká a malá písmena odpovídající ve všech ostatních případech porovnávání malá a velká písmena.

Hello výchozí soubor vzor použít, pokud je zadána žádná vzor souborů je *.* pro umístění systému souborů nebo prázdnou předponu pro umístění služby Azure Storage. Zadání více vzorů souborů není podporováno.

**Platí pro:** objekty BLOB, soubory

### <a name="destkeystorage-key"></a>/ DestKey: "klíč úložiště"
Určuje klíč účtu úložiště hello hello cílovému prostředku.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="destsassas-token"></a>/ DestSAS: "tokenu sas"
Určuje sdíleného přístupového podpisu (SAS) s oprávněními ke čtení a zápisu pro cíl hello (pokud existuje). Obklopit hello SAS s dvojitými uvozovkami, protože pravděpodobně obsahuje speciální znaky příkazového řádku.

Pokud hello cílovému prostředku je kontejner objektů blob, sdílené složky nebo tabulky, můžete buď zadat tuto možnost, za nímž následuje tokenu SAS hello nebo jako součást hello cílový objekt blob kontejner, sdílené složky nebo identifikátor URI tabulky bez této možnosti můžete zadat hello SAS.

Pokud hello zdrojové a cílové jsou oba objekty BLOB, pak hello cílový objekt blob se musí nacházet v rámci hello stejný jako zdrojový objekt blob hello účet úložiště.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcekeystorage-key"></a>/ SourceKey: "klíč úložiště"
Určuje klíč účtu úložiště hello hello zdrojovému prostředku.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcesassas-token"></a>/ SourceSAS: "tokenu sas"
Určuje sdíleného přístupového podpisu oprávnění ke čtení a seznamu zdroje hello (pokud existuje). Obklopit hello SAS s dvojitými uvozovkami, protože pravděpodobně obsahuje speciální znaky příkazového řádku.

Pokud hello zdroje prostředků je kontejner objektů blob a je k dispozici klíč ani SAS, budou kontejner objektů blob hello číst přes anonymní přístup.

Pokud je zdroj hello sdílené složky nebo tabulka, je třeba zadat klíč nebo SAS.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="s"></a>/S
Určuje režim rekurzivní operace kopírování. V režimu rekurzivní AzCopy zkopíruje všechny objekty BLOB nebo soubory, které odpovídají hello zadaný soubor vzor, včetně programů v podsložkách.

**Platí pro:** objekty BLOB, soubory

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blokem" | "stránka" | "připojit"
Určuje, zda text hello cílový objekt blob je objekt blob bloku, objektů blob stránky nebo doplňovací objekt blob. Tuto možnost lze použít pouze v případě, že nahráváte do objektu blob. Jinak je generována chyba. Pokud hello cílový objekt blob a není tato možnost zadána, ve výchozím nastavení, AzCopy vytvoří objekt blob bloku.

**Platí pro:** objektů BLOB

### <a name="checkmd5"></a>/ CheckMD5
Vypočítá hodnotu hash MD5 pro stažená data a ověří, zda hodnota hash MD5 hello uložené v objektu blob hello nebo vlastnost MD5 obsah souboru odpovídá hello vypočítat hodnotu hash. Kontrola Hello MD5 je vypnuta ve výchozím nastavení, takže při stahování dat je třeba zadat tuto možnost tooperform hello MD5 kontrolu.

Všimněte si, že Azure Storage není zárukou toho, že hello hodnota hash MD5 uložené pro objekt blob hello nebo souboru je aktuální. Klienta odpovědnost tooupdate hello MD5 je vždy, když se mění hello blob nebo souboru.

AzCopy vždy nastaví hello obsah MD5 vlastnost pro objektů blob v Azure nebo soubor po jeho nahrání toohello služby.  

**Platí pro:** objekty BLOB, soubory

### <a name="snapshot"></a>/ Snímku
Určuje, zda tootransfer snímky. Tato možnost je platná, pouze pokud je zdroj hello objekt blob.

Hello přenášená blob snímky jsou přejmenovány v tomto formátu: .extension název objektu blob (snímku čas)

Ve výchozím nastavení nebudou zkopírovány snímky.

**Platí pro:** objektů BLOB

### <a name="vverbose-log-file"></a>/ V: [podrobné souboru protokolu]
Výstupy podrobné stavové zprávy do souboru protokolu.

Ve výchozím nastavení, je hello podrobný soubor protokolu s názvem AzCopyVerbose.log v `%LocalAppData%\Microsoft\Azure\AzCopy`. Pokud zadáte existující umístění souborů pro tuto možnost, bude podrobného protokolování hello připojením toothat souboru.  

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="zjournal-file-folder"></a>/ Z: [deníku – soubor a složka]
Určuje složku souboru deníku pro operace obnovení.

AzCopy vždy podporuje obnovení, pokud operace byla přerušena.

Pokud není tato možnost zadána, nebo je zadán bez cestu ke složce, pak AzCopy vytvoří v umístění hello výchozí, což je % LocalAppData%\Microsoft\Azure\AzCopy hello deníku souboru.

Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti. Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.

Pokud soubor deníku hello neexistuje, AzCopy zkontroluje, zda hello příkazového řádku, které můžete vložit odpovídá hello příkazového řádku v souboru deníku hello. Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace. Pokud se neshodují, bude výzvami tooeither přepsat hello deníku souboru toostart operaci nového nebo toocancel hello aktuální operace.

soubor deníku Hello se odstraní po úspěšném dokončení operace hello.

Všimněte si, že obnovení ze souboru deníku vytvořeného v předchozí verzi AzCopy operace není podporována.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="parameter-file"></a>/@:"Parameter-File"
Určuje soubor, který obsahuje parametry. AzCopy procesy hello parametry v souboru hello stejně, jako kdyby kdyby byly zadány na příkazovém řádku hello.

V souboru odpovědí můžete zadat několik parametrů na jeden řádek nebo zadejte každý parametr na samostatném řádku. Všimněte si, že jednotlivé parametr nemůže zahrnovat více řádků.

Soubory odezvy může zahrnovat komentáře řádky, které začínají symbolem # hello.

Můžete zadat několik souborů odpovědi. Všimněte si však, že AzCopy nepodporuje soubory vnořené odezvy.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="y"></a>/Y
Potlačí všechny výzvy potvrzení AzCopy.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="l"></a>/ L
Určuje operaci výpis pouze; žádná data budou zkopírována.

AzCopy bude interpretovat hello pomocí této možnosti simulace pro spuštěné hello příkazového řádku bez možnosti/l a počet, kolik objekty se zkopírují, můžete zadat možnost /V v hello stejný čas toocheck objektů, které se mají zkopírovat hello podrobného protokolování.

chování Hello tato možnost je určena také hello umístění hello zdroje dat a hello přítomnost hello rekurzivní možnost /S a soubor vzor možnost režimu /Pattern.

AzCopy vyžaduje oprávnění seznamu a přečtěte si toto umístění zdroje, při použití této možnosti.

**Platí pro:** objekty BLOB, soubory

### <a name="mt"></a>/ MT
Nastaví čas hello stažený soubor poslední změny toobe hello stejný jako zdrojový objekt blob hello nebo souboru.

**Platí pro:** objekty BLOB, soubory

### <a name="xn"></a>/XN
Vyloučí novější zdrojovému prostředku. Pokud hello čas poslední změny zdroje hello hello stejnou nebo novější, než cílový se nezkopírují Hello prostředků.

**Platí pro:** objekty BLOB, soubory

### <a name="xo"></a>/XO
Vyloučí starší zdrojovému prostředku. Pokud hello čas poslední změny zdroje hello hello stejné nebo starší než cílový se nezkopírují Hello prostředků.

**Platí pro:** objekty BLOB, soubory

### <a name="a"></a>/A
Ukládání pouze soubory, které mají nastaven atribut archivu hello.

**Platí pro:** objekty BLOB, soubory

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]
Nahrávání pouze soubory, které žádné hello zadané sady atributů.

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
Vyloučí soubory, které žádné hello zadané sady atributů.

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
Označuje, že hello oddělovací znak použít toodelimit virtuálních adresářů v názvu objektu blob.

Ve výchozím nastavení používá AzCopy / jako hello oddělovací znak. Ale AzCopy podporuje používání libovolného znaku běžné (například @, #, nebo %) jako oddělovač. Pokud potřebujete tooinclude jednu z těchto speciálních znaků v hello příkazového řádku, uzavřete název souboru hello s dvojité uvozovky.

Tato možnost platí pouze pro stahování objekty BLOB.

**Platí pro:** objektů BLOB

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "číslo z souběžných operace"
Určuje hello počet souběžných operací.

AzCopy ve výchozím nastavení spustí počet propustnost přenosu dat hello tooincrease souběžných operací. Všimněte si, že velký počet souběžných operací v prostředí s malou šířkou pásma může zahlcovat hello síťové připojení a zabránit hello operací z plně dokončení. Omezení souběžných operací podle skutečné dostupnou šířku pásma sítě.

Hello horní limit pro souběžných operací je 512.

**Platí pro:** objekty BLOB, soubory, tabulky

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tabulka"
Určuje, že hello `source` prostředek je k dispozici v hello místní vývojové prostředí, spouštění v emulátoru úložiště hello objektu blob.

**Platí pro:** objekty BLOB, tabulek

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tabulka"
Určuje, že hello `destination` prostředek je k dispozici v hello místní vývojové prostředí, spouštění v emulátoru úložiště hello objektu blob.

**Platí pro:** objekty BLOB, tabulek

### <a name="pkrskey1key2key3"></a>/ PKRS: "key&#1;key&#2; klíč&#3;..."
Rozdělení hello tooenable klíče rozsahu oddílu Export dat v tabulce současně, což zvyšuje rychlost hello operace exportu hello.

Pokud není tato možnost zadána, použije AzCopy jedním vláknem tooexport tabulka entity. Například pokud hello uživatel zadává /PKRS: "aa #bb", pak AzCopy spustí tři souběžných operací.

Každé operace exportuje mezi tři rozsahy klíčů oddílu, jak je uvedeno níže:

  [klíč prvního oddílu, aa)

  [aa, bb)

  [bb, klíč oddílu poslední]

**Platí pro:** tabulky

### <a name="splitsizefile-size"></a>/ SplitSize: "velikost souboru"
Určuje text hello exportovaný soubor rozdělení velikost v MB, hello minimální povolená hodnota je 32.

Pokud není tato možnost zadána, bude AzCopy exportovat soubor toosingle dat tabulky.

Pokud data tabulky hello je tooa exportovaný blob a hello exportovaný soubor dosáhne velikosti hello 200 GB limit pro velikost objektu blob, pak AzCopy rozdělí hello exportovaný soubor, i když není tato možnost zadána.

**Platí pro:** tabulky

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"
Určuje chování importu dat tabulky hello.

* InsertOrSkip - přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.
* InsertOrMerge - sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.
* InsertOrReplace - nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.

**Platí pro:** tabulky

### <a name="manifestmanifest-file"></a>/ Manifest: "manifest – soubor"
Určuje soubor manifestu hello pro tabulky hello exportovat a importovat operaci.

Tato možnost je volitelné během operace exportu hello, AzCopy vygeneruje soubor manifestu se předdefinované názvem, pokud není tato možnost zadána.

Tato možnost je povinná během operace importu hello týkající se umísťování hello datových souborů.

**Platí pro:** tabulky

### <a name="synccopy"></a>/ SyncCopy
Určuje, zda toosynchronously kopírování objektů BLOB nebo soubory mezi dva koncové body Azure Storage.

AzCopy ve výchozím nastavení používá asynchronní kopie straně serveru. Zadejte tuto možnost tooperform synchronního kopírovat, které soubory ke stažení objektů BLOB nebo soubory toolocal paměti a odesílá je tooAzure úložiště.

Tuto možnost můžete použít při kopírování souborů v rámci úložiště objektů Blob, úložiště File, nebo z objektu Blob úložiště tooFile úložiště nebo naopak naopak.

**Platí pro:** objekty BLOB, soubory

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "content-type"
Určuje typ obsahu hello MIME pro cílové objekty BLOB nebo soubory.

Nastaví AzCopy hello typ obsahu pro objekt blob nebo soubor tooapplication/octet-stream ve výchozím nastavení. Můžete nastavit hello typ obsahu pro všechny objekty BLOB nebo soubory a explicitně zadat hodnotu pro tuto možnost.

Pokud zadáte tuto možnost bez hodnoty, bude nastavte AzCopy, každý objekt blob nebo typ obsahu souboru podle tooits příponu souboru.

**Platí pro:** objekty BLOB, soubory

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"
Určuje formát hello hello tabulky exportovaná data souboru.

Pokud není tato možnost zadána, exportuje AzCopy ve výchozím nastavení tabulky datový soubor ve formátu JSON.

**Platí pro:** tabulky

## <a name="known-issues-and-best-practices"></a>Známé problémy a doporučené postupy
### <a name="limit-concurrent-writes-while-copying-data"></a>Limit souběžných zápisy při kopírování dat
Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být upravovat hello data během kopírování ho. Pokud je to možné Ujistěte se, že hello dat, který chcete kopírovat nemění během operace kopírování hello. Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis toohello virtuálního pevného disku. Dobře toodo, jedná se o leasing toobe prostředků hello zkopírovali. Alternativně můžete nejprve vytvořte snímek hello virtuální pevný disk a poté zkopírujte hello snímku.

Pokud nelze zabránit jiné aplikace z zápis tooblobs nebo soubory, když jsou kopírovány, pak mějte na paměti, která úlohou hello hello čas dokončení, hello kopírované prostředky buď již nemá úplné parita s prostředky zdroj hello.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Jedna instance nástroje AzCopy spusťte na jednom počítači.
AzCopy je navrženou toomaximize hello využití přenosu dat tooaccelerate hello váš počítač prostředků, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost hello `/NC` Pokud potřebujete více souběžných operací. Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Povolit algoritmus MD5 kompatibilní s FIPS pro AzCopy když jste "použití standardu FIPS kompatibilní s algoritmy pro šifrování, hašování a podpisování".
AzCopy ve výchozím nastavení používá rozhraní .NET MD5 implementace toocalculate hello MD5 při kopírování objektů, ale existují některé požadavky na zabezpečení, které je třeba AzCopy tooenable kompatibilní MD5 nastavení FIPS.

Můžete vytvořit soubor app.config `AzCopy.exe.config` s vlastností `AzureStorageUseV1MD5` a umístí jej vyhraďte s AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Pro vlastnost "AzureStorageUseV1MD5" • True – hello výchozí hodnota AzCopy použije implementace rozhraní .NET MD5.
• False – AzCopy použije algoritmus MD5 kompatibilní se standardem FIPS.

Všimněte si, že na počítač se systémem Windows ve výchozím nastavení je zakázáno algoritmy splňující standard FIPS, můžete v okně Spustit zadejte secpol.msc a zkontrolujte tento přepínač v nastavení zabezpečení -> Místní zásady -> zabezpečení -> – Možnosti kryptografie systému: použití vyhovující standardu FIPS pro šifrování, hašování a podpisování.

## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage a AzCopy najdete v části toohello následující prostředky.

### <a name="azure-storage-documentation"></a>Dokumentace k Azure Storage:
* [Úvod tooAzure úložiště](storage-introduction.md)
* [Jak toouse úložiště Blob z rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Jak toouse úložiště File z rozhraní .NET](storage-dotnet-how-to-use-files.md)
* [Jak toouse úložiště Table z rozhraní .NET](storage-dotnet-how-to-use-tables.md)
* [Jak toocreate, spravovat nebo odstranit účet úložiště](storage-create-storage-account.md)
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

