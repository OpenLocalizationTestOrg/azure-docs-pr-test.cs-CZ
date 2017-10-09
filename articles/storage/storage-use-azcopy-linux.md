---
title: "aaaCopy nebo přesunout data tooAzure úložiště s AzCopy v systému Linux | Microsoft Docs"
description: "Použijte hello AzCopy na Linux nástroj toomove nebo kopírování dat tooor z objektu blob a soubor obsahu. Kopírování dat tooAzure úložiště z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Vaše data tooAzure úložiště snadno migrujte."
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
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: dccb03c9e8cc3ea661494e7834f307b0e3e30cb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Přenos dat pomocí nástroje AzCopy v systému Linux
V systému Linux AzCopy je nástroj příkazového řádku pro kopírování tooand dat z úložiště objektů Blob v Microsoft Azure a soubor pomocí jednoduchých příkazů optimální výkon. Data můžete zkopírovat z jednoho objektu tooanother v rámci účtu úložiště nebo mezi účty úložiště.

Existují dvě verze nástroje AzCopy, které si můžete stáhnout. AzCopy v systému Linux je sestaven pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku. [AzCopy v systému Windows](storage-use-azcopy.md) je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows stylu. Tento článek se zabývá AzCopy v systému Linux.

## <a name="download-and-install-azcopy"></a>Stáhněte a nainstalujte AzCopy
### <a name="installation-on-linux"></a>Instalace v systému Linux

AzCopy v systému Linux vyžaduje základní rozhraní .NET framework na platformě hello. Najdete pokyny k instalaci hello na hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) stránky.

Jako příklad nainstalujme na Ubuntu 16.10 .NET Core. Hello nejnovější Průvodce instalací, najdete v článku [.NET Core v systému Linux](https://www.microsoft.com/net/core#linuxubuntu) instalační stránka.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

Po instalaci .NET Core, stáhněte a nainstalujte AzCopy.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Po instalaci nástroje AzCopy v systému Linux, můžete odebrat soubory extrahovány hello. Případně pokud nemáte oprávnění superuživatele, můžete také spustit AzCopy pomocí skriptu prostředí hello 'azcopy' v hello extrahovanou složku. 

### <a name="alternative-installation-on-ubuntu"></a>Alternativní instalace na Ubuntu

**Ubuntu 14.04**

Přidání výstižný zdroje pro platformu .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Přidání výstižný zdroje pro platformu .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.10**

Přidání výstižný zdroje pro platformu .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a>Zápis vaše první příkaz AzCopy
Hello základní syntaxe pro příkazy AzCopy je:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

Hello následující příklady ukazují různé scénáře pro kopírování data tooand z Microsoft Azure Blobs a soubory. Odkazovat toohello `azcopy --help` nabídky podrobné vysvětlení hello parametrů použitých v každém vzorku.

## <a name="blob-download"></a>Objekt BLOB: stažení
### <a name="download-single-blob"></a>Stáhnout jediného objektu blob

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Pokud složka hello `/mnt/myfiles` neexistuje, AzCopy ji vytvoří a stáhne `abc.txt ` do nové složky hello.

### <a name="download-single-blob-from-secondary-region"></a>Stáhnout jediného objektu blob ze sekundární oblasti

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="download-all-blobs"></a>Stažení všech objektů BLOB

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Po stažení operaci hello hello directory `/mnt/myfiles` zahrnuje hello následující soubory:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Pokud nezadáte možnost `--recursive`, budou staženy žádné objektů blob.

### <a name="download-blobs-with-specified-prefix"></a>Stáhnout objekty BLOB se zadanou předponou.

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello. Všechny objekty BLOB začínající předponou hello `a` staženy.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Po stažení operaci hello hello složky `/mnt/myfiles` zahrnuje hello následující soubory:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

Předpona Hello platí toohello virtuální adresář, který tvoří první část hello hello název objektu blob. Ve výše uvedeném příkladu hello hello virtuální adresář neodpovídá zadané předpony hello, takže se stáhne žádný objekt blob. Kromě toho pokud hello možnost `--recursive` není zadán, AzCopy nestáhne žádné objekty BLOB.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Nastavit čas poslední úpravy hello z exportované soubory toobe stejná hodnota jako hello zdroj objektů BLOB

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

Můžete také vyloučit objekty BLOB z operace nástroje download hello podle jejich čas poslední změny. Příklad: Pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejnou nebo novější než hello cílový soubor, přidejte hello `--exclude-newer` možnost:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Nebo pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejné nebo starší než hello cílový soubor, přidejte hello `--exclude-older` možnost:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>Objekt BLOB: nahrát
### <a name="upload-single-file"></a>Odeslat jedním souborem

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Pokud hello zadaný cílový kontejner neexistuje, AzCopy ji vytvoří, a nahrávání hello souboru do ní.

### <a name="upload-single-file-toovirtual-directory"></a>Nahrát toovirtual directory jedním souborem

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Pokud hello zadaný virtuální adresář neexistuje, AzCopy odešle hello souboru tooinclude hello virtuální adresář v název objektu blob hello (*například*, `vd/abc.txt` v předchozím příkladu hello).

### <a name="upload-all-files"></a>Odeslat všechny soubory.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Zadání `--recursive` nahrávání hello obsah hello zadaný adresář tooBlob úložiště rekurzivně, což znamená, že jsou také odeslány všechny podsložky a jejich soubory. Například předpokládejme hello následující soubory jsou umístěny ve složce `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Po operaci odeslání hello hello kontejner obsahuje hello následující soubory:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Když hello možnost `--recursive` není zadán, jen hello následující tři soubory jsou odeslány:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

Předpokládejme hello následující soubory jsou umístěny ve složce `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Po operaci odeslání hello hello kontejner obsahuje hello následující soubory:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Když hello možnost `--recursive` není zadán, AzCopy přeskočí soubory, které se nacházejí v podadresářích:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Zadejte hello obsahu typ MIME cílový objekt blob
Ve výchozím nastavení, AzCopy nastaví typ obsahu hello cílový objekt blob příliš`application/octet-stream`. Však můžete explicitně zadat typ obsahu hello prostřednictvím možnosti hello `--set-content-type [content-type]`. Tuto syntaxi nastaví hello typ obsahu pro všechny objekty BLOB v operaci odeslání.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Pokud hello možnost `--set-content-type` je pak AzCopy nastaví jednotlivých objektů blob nebo soubor zadaný bez hodnoty, je typ obsahu podle tooits příponu souboru.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a>Objekt BLOB: kopírování
### <a name="copy-single-blob-within-storage-account"></a>Zkopírujte jediného objektu blob v rámci účtu úložiště

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-single-blob-across-storage-accounts"></a>Zkopírujte jediného objektu blob mezi různými účty úložiště

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Zkopírujte jediného objektu blob ze sekundární oblasti tooprimary oblasti

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Po operaci kopírování hello hello cílový kontejner obsahuje hello blob a jeho snímků. Hello kontejneru zahrnuje následující hello objektů blob a jeho snímky:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchronně kopírovat mezi různými účty úložiště objektů BLOB
AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně. Proto se zkopíruje spustí operace kopírování hello hello pozadí využití kapacity přebytečné šířky pásma, která nemá žádné SLA z hlediska jak rychlé objekt blob. 

Hello `--sync-copy` možnost zajistí, že operace kopírování hello získá konzistentní rychlost. AzCopy provede synchronní kopie hello tak, že stáhnete objekty BLOB hello toocopy z hello zadaný zdroj toolocal paměti a odesílání je toohello cíl úložiště objektů Blob.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy`může generovat další odchozí náklady porovnání tooasynchronous kopie. Hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.

## <a name="file-download"></a>Soubor: stažení
### <a name="download-single-file"></a>Stáhnout jedním souborem

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (*například* `abc.txt`) toodownload jeden soubor, nebo zadejte možnost `--recursive` toodownload všechny soubory ve sdílené složce hello rekurzivní. Probíhá pokus toospecify vzor souborů i možnost `--recursive` společně dojde k chybě.

### <a name="download-all-files"></a>Stáhnout všechny soubory

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Všimněte si, že všechny prázdné složky nestahují.

## <a name="file-upload"></a>Soubor: nahrát
### <a name="upload-single-file"></a>Odeslat jedním souborem

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a>Odeslat všechny soubory.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Všimněte si, že nejsou uloženy žádné prázdné složky.

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Soubor: kopírování
### <a name="copy-across-file-shares"></a>Kopírování mezi sdílenými složkami

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-from-file-share-tooblob"></a>Zkopírujte z tooblob sdílené složky souborů

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Při kopírování souboru ze souboru tooblob sdílenou složku, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="copy-from-blob-toofile-share"></a>Zkopírujte ze složky toofile objektů blob

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Při kopírování souboru ze sdílené složky toofile objektů blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.

### <a name="synchronously-copy-files"></a>Synchronně kopírování souborů
Můžete zadat hello `--sync-copy` možnost toocopy dat z úložiště File tooFile úložiště, ze souboru úložiště tooBlob úložiště a z úložiště objektů Blob tooFile úložiště synchronně. AzCopy ve stahování hello zdroje dat toolocal paměti a odesílá je toodestination spustí tuto operaci. V takovém případě platí standardní výstupní náklady.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Při kopírování ze souboru úložiště tooBlob úložiště, typu blob výchozí hello je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` toochange hello cílový objekt blob typu.

Všimněte si, že `--sync-copy` může generovat další odchozí náklady porovnáním různých tooasynchronous kopie. Hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.

## <a name="other-azcopy-features"></a>Další funkce AzCopy
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Kopírovat pouze data, která neexistuje v cílovém umístění hello
Hello `--exclude-older` a `--exclude-newer` parametry umožňují tooexclude starší nebo novější zdroj prostředky z kopírování, v uvedeném pořadí. Pokud chcete pouze prostředky toocopy zdroje, které neexistují v cílovém umístění hello, můžete zadat oba parametry v hello AzCopy příkaz:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a>Používání konfiguraci souboru toospecify parametrů příkazového řádku

```azcopy
azcopy --config-file "azcopy-config.ini"
```

Parametry příkazového řádku AzCopy můžete zahrnout v konfiguračním souboru. AzCopy procesy hello parametry v souboru hello jako v případě, kdyby byly zadány na příkazovém řádku hello, provádění přímé nahrazení s hello obsah souboru hello.

Předpokládejme, konfigurační soubor s názvem `copyoperation`, který obsahuje následující řádky hello. Každý parametr AzCopy můžete zadat na jeden řádek.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

nebo na samostatné řádky:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy selže, pokud rozdělení hello parametr mezi dvěma čárami, jak je vidět tady pro hello `--source-key` parametr:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Zadejte sdílený přístupový podpis (SAS)

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

Můžete také zadat SAS na kontejneru hello URI:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

Všimněte si, že AzCopy aktuálně podporuje jenom hello [SAS účtu](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).

### <a name="journal-file-folder"></a>Složky pro soubor deníku
Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti. Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.

Pokud hello deníku soubor neexistuje, AzCopy kontroluje, zda text hello příkazový řádek, který můžete vložit odpovídá hello příkazového řádku v souboru deníku hello. Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace. Pokud se neshodují, AzCopy vyzve uživatele tooeither přepsat hello deníku souboru toostart novou operaci, nebo toocancel hello aktuální operace.

Pokud chcete, aby toouse hello výchozí umístění souboru deníku hello:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Pokud není možnost `--resume`, nebo zadejte možnost `--resume` bez hello cesta ke složce, jako v příkladu nahoře, AzCopy hello deníku soubor vytvoří v hello výchozího umístění, což je `~\Microsoft\Azure\AzCopy`. Pokud hello deníku soubor již existuje, AzCopy obnoví hello operaci na základě souboru deníku hello.

Pokud chcete, aby toospecify vlastní umístění souboru deníku hello:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

Tento příklad vytvoří soubor deníku hello, pokud ještě neexistuje. Pokud neexistuje, AzCopy obnoví hello operaci na základě souboru deníku hello.

Pokud chcete tooresume operace AzCopy, opakujte hello stejný příkaz. AzCopy v systému Linux potom zobrazí výzvu k potvrzení:

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Výstupní podrobné protokoly

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Zadejte hello počet souběžných operací toostart
Možnost `--parallel-level` určuje hello počet souběžných kopie operací. Ve výchozím nastavení spustí AzCopy počet propustnost přenosu dat hello tooincrease souběžných operací. Hello počet souběžných operací osm časy hello počet procesorů, které máte. Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro – paralelní úrovni tooavoid selhání kvůli konfliktům prostředků.

[!TIP]
>Úplný seznam hello tooview AzCopy parametrů, podívejte se na 'azcopy – Nápověda' nabídky.

## <a name="known-issues-and-best-practices"></a>Známé problémy a doporučené postupy
### <a name="error-net-core-is-not-found-in-hello-system"></a>Chyba: .NET Core nebyl nalezen v systému hello.
Pokud se vyskytne chyba s oznámením, že .NET Core není nainstalována v systému hello, hello cesta toohello .NET Core binární `dotnet` pravděpodobně chybí.

V pořadí tooaddress tento problém, najít binární .NET Core hello systému hello:
```bash
sudo find / -name dotnet
```

Tento příkaz vrátí hello cesta toohello dotnet binární. 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

Nyní přidejte proměnné PATH toohello této cesty. Sudo upravte secure_path toocontain hello cesta toohello dotnet binární:
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

V tomto příkladu secure_path proměnná přečte jako:

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

Pro aktuálního uživatele hello upravte.bash_profile/.profile tooinclude hello cesta toohello dotnet binární v proměnné PATH 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

Ověřte, zda .NET Core je teď v CESTĚ:
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a>Chyba při instalaci nástroje AzCopy
Pokud narazíte na potíže s instalací AzCopy, můžete se pokusit toorun AzCopy pomocí skriptu bash hello v hello extrahovat `azcopy` složky.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Limit souběžných zápisy při kopírování dat
Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být upravovat hello data během kopírování ho. Pokud je to možné Ujistěte se, že hello dat, který chcete kopírovat nemění během operace kopírování hello. Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis toohello virtuálního pevného disku. Dobře toodo, jedná se o leasing toobe prostředků hello zkopírovali. Alternativně můžete nejprve vytvořte snímek hello virtuální pevný disk a poté zkopírujte hello snímku.

Pokud nelze zabránit jiné aplikace z zápis tooblobs nebo soubory, když jsou kopírovány, pak mějte na paměti, která úlohou hello hello čas dokončení, hello kopírované prostředky buď již nemá úplné parita s prostředky zdroj hello.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Jedna instance nástroje AzCopy spusťte na jednom počítači.
AzCopy je navrženou toomaximize hello využití přenosu dat tooaccelerate hello váš počítač prostředků, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost hello `--parallel-level` Pokud potřebujete více souběžných operací. Další podrobnosti, zadejte `AzCopy --help parallel-level` v příkazovém řádku hello.

## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage a AzCopy najdete v tématu hello následující prostředky:

### <a name="azure-storage-documentation"></a>Dokumentace k Azure Storage:
* [Úvod tooAzure úložiště](storage-introduction.md)
* [Vytvoření účtu úložiště](storage-create-storage-account.md)
* [Spravovat objekty BLOB pomocí Průzkumníka úložiště](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [Použití hello 2.0 rozhraní příkazového řádku Azure s Azure Storage](storage-azure-cli.md)
* [Jak toouse úložiště objektů Blob z jazyka C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Jak toouse úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md)
* [Jak toouse úložiště objektů Blob z Node.js](storage-nodejs-how-to-use-blob-storage.md)
* [Jak toouse úložiště objektů Blob z Pythonu](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Příspěvky blogu Azure Storage:
* [Uvedení AzCopy na Linux Preview](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Představení náhled knihovny přesun dat úložiště Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Úvod synchronní kopie a vlastní typ obsahu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Přenos dat pomocí režimu s možností restartování a tokenu SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Objekt Blob kopírování mezi účet pomocí](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

