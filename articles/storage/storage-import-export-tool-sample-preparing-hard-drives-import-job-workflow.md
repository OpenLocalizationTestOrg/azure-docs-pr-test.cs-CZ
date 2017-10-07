---
title: "Úloha importu aaaSample pracovního postupu tooprep pevné disky pro Azure importu a exportu | Microsoft Docs"
description: "Návod pro dokončení procesu hello přípravy jednotky pro úlohy importu v hello služba Azure Import/Export najdete."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 560220b7dc9f87416f1fec1ff30fa5cd65812ce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu

Tento článek vás provede procesem dokončení hello přípravy jednotky pro úlohy importu.

## <a name="sample-data"></a>Ukázková data

Tento příklad importuje následující data do účtu úložiště Azure s názvem hello `mystorageaccount`:

|Umístění|Popis|Velikost dat|
|--------------|-----------------|-----|
|H:\Video\ |Kolekce videa|12 TB|
|H:\Photo\ |Kolekce fotografií|30 GB|
|K:\Temp\FavoriteMovie.ISO|Bitová kopie disku A Blu-ray™|25 GB|
|\\\bigshare\john\music\|Kolekce hudebních souborů ve sdílené síťové složce|10 GB|

## <a name="storage-account-destinations"></a>Cíle účtu úložiště

Úloha importu Hello se importu dat hello do hello následující cíle v účtu úložiště hello:

|Zdroj|Cílový virtuální adresář nebo objekt blob|
|------------|-------------------------------------------|
|H:\Video\ |video nebo|
|H:\Photo\ |fotografie nebo|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Hudba|

Pomocí této mapování hello souboru `H:\Video\Drama\GreatMovie.mov` bude objekt blob importované toohello `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Stanovení požadavků na pevném disku

Dále toodetermine kolik pevné disky jsou potřeba, výpočetní hello velikost dat hello:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

V tomto příkladu by mělo být dostatečné dva 8TB pevné disky. Ale protože hello zdrojový adresář `H:\Video` má 12TB dat a jeden pevný disk je kapacita je pouze 8TB, bude možné toospecify v hello následující způsobem hello **driveset.csv** souboru:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
Nástroj Hello provede distribuci dat mezi dva pevné disky optimálního.

## <a name="attach-drives-and-configure-hello-job"></a>Připojte jednotky a konfiguraci hello úlohy
Bude Připojte oba počítače toohello disky a vytvářet svazky. Pak vytvořte **dataset.csv** souboru:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

Kromě toho můžete nastavit hello následující metadata pro všechny soubory:

* **UploadMethod:** služby Windows Azure Import/Export
* **DataSetName:** SampleData
* **Datum vytvoření:** 10/1/2013

metadata tooset hello importu souborů, vytvořte textový soubor, `c:\WAImportExport\SampleMetadata.txt`, s hello následující obsah:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Můžete vytvořit také některé vlastnosti pro hello `FavoriteMovie.ISO` objektů blob:

* **Content-Type:** application/octet-stream
* **Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
* **Cache-Control:** no cache

tooset tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a>Spuštění hello nástroj Azure Import/Export (WAImportExport.exe)

Teď je připraven toorun hello nástroj Azure Import/Export tooprepare hello dva pevné disky.

**Pro první relaci hello:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Pokud žádná další data potřebuje toobe přidat, vytvořte jiný soubor datovou sadu (stejný formát jako Initialdataset).

**Pro hello druhý relace:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Jakmile jste dokončili hello kopírování relací, můžete odpojit hello dvě jednotky z počítače kopie hello a dodávat je toohello příslušné datové centrum Azure. Budete nahrát hello dva soubory deníku, `<FirstDriveSerialNumber>.xml` a `<SecondDriveSerialNumber>.xml`, když vytvoříte hello úlohy importu v hello portálu Azure.

## <a name="next-steps"></a>Další kroky

* [Příprava pevných disků pro úlohu importu](storage-import-export-tool-preparing-hard-drives-import.md)
* [Stručná referenční příručka pro často používané příkazy](storage-import-export-tool-quick-reference.md)
