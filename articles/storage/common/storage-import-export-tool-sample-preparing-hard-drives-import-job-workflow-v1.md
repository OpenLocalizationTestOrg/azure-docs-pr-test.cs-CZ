---
title: "aaaSample pracovního postupu tooprep pevné disky pro Azure importu a exportu importovat úlohy - v1 | Microsoft Docs"
description: "Návod pro dokončení procesu hello přípravy jednotky pro úlohy importu v hello služba Azure Import/Export najdete."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a>Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu
Toto téma vás provede procesem hello dokončení procesu přípravy jednotky pro úlohy importu.  
  
Tento příklad importuje následující data do účtu úložiště Azure okno s názvem hello `mystorageaccount`:  
  
|Umístění|Popis|  
|--------------|-----------------|  
|H:\Video|Kolekce videa, 5 TB celkem.|  
|H:\Photo|Kolekce fotografií, celkem 30 GB.|  
|K:\Temp\FavoriteMovie.ISO|Image disku A Blu-ray™, 25 GB.|  
|\\\bigshare\john\music|Kolekce hudebních souborů ve sdílené síťové složce, celkem 10 GB.|  
  
Úloha importu Hello importuje tato data do hello následující cíle v účtu úložiště hello:  
  
|Zdroj|Cílový virtuální adresář nebo objekt blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
Pomocí této mapování hello souboru `H:\Video\Drama\GreatMovie.mov` je objekt blob importované toohello `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Dále toodetermine kolik pevné disky jsou potřeba, výpočetní hello velikost dat hello:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
V tomto příkladu by mělo být dostatečné dva 3 TB pevné disky. Ale protože hello zdrojový adresář `H:\Video` má 5 TB dat a jeden pevný disk je kapacita je jenom 3 TB, je nutné toobreak `H:\Video` do dvou menší adresářů: `H:\Video1` a `H:\Video2`, než spuštěna hello Microsoft Nástroj Azure importu a exportu. Tento krok vypočítá hello následující zdrojového adresáře:  
  
|Umístění|Velikost|Cílový virtuální adresář nebo objekt blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|30 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
 I když hello `H:\Video`directory rozdělení tootwo adresáře, budou bodu toohello stejný cílový virtuální adresář v účtu úložiště hello. Tímto způsobem, všechny soubory videa se udržují v rámci jedné `video` kontejneru v účtu úložiště hello.  
  
 V dalším kroku hello předchozí zdrojového adresáře jsou rovnoměrně distribuované toohello dva pevné disky:  
  
||||  
|-|-|-|  
|Pevný disk|Zdrojové adresáře|Celková velikost|  
|První jednotky|H:\Video1|2,5 TB + 30 GB|  
||H:\Photo||  
|Sekundy jednotky|H:\Video2|2,5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Kromě toho můžete nastavit hello následující metadata pro všechny soubory:  
  
-   **UploadMethod:** služby Windows Azure Import/Export  
  
-   **DataSetName:** SampleData  
  
-   **Datum vytvoření:** 10/1/2013  
  
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
  
-   **Content-Type:** application/octet-stream  
  
-   **Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
-   **Cache-Control:** no cache  
  
tooset tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Teď je připraven toorun hello nástroj Azure Import/Export tooprepare hello dva pevné disky. Poznámky:  
  
-   první disk Hello je připojit jako jednotka X.  
  
-   druhý disk Hello je připojit jako disk Y.  
  
-   Hello klíč pro účet úložiště hello `mystorageaccount` je `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Příprava disku pro import, když je předem načtená data
 
 Pokud toobe hello data importovat je již na disku hello, použijte příznak /skipwrite hello. Hodnota Hello /t a /srcdir má oba bodu toohello disku připraveném pro import. Pokud všechny toobe hello data importovat nepůjde toohello stejné cílový virtuální adresář nebo kořenové hello účtu úložiště, spusťte hello stejný příkaz pro každý cílový adresář samostatně, udržování hello hodnota /id hello stejné napříč všechny spustí.

>[!NOTE] 
>Nezadávejte/Format, jak se bude vymazání hello dat na disku hello. Můžete zadat / šifrování nebo /bk v závislosti na tom, jestli hello disk už je šifrovaný nebo ne. 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Zkopírujte relací - nejprve jednotky

Hello první disk spusťte hello nástroj Azure Import/Export zdroje dvakrát hello toocopy dva adresáře:  

**Nejdříve zkopírovat relace**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**Druhá kopie relace**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Zkopírujte relací - druhé jednotky
 
Pro hello druhý disku, spusťte hello nástroj Azure Import/Export třikrát po jednotlivých hello zdrojového adresáře a jednou pro samostatnou hello Blu-Ray™ soubor obrázku):  
  
**Nejdříve zkopírovat relace** 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
**Druhá kopie relace**

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
**Třetí kopii relace**  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a>Zkopírujte dokončení relace

Jakmile jste dokončili hello kopírování relací, můžete odpojit hello dvě jednotky z počítače kopie hello a dodávat je toohello příslušné služby Windows Azure datového centra. Nahrát hello dva soubory deníku, `FirstDrive.jrn` a `SecondDrive.jrn`, když vytvoříte hello úlohy importu v hello [portálu Windows Azure](https://manage.windowsazure.com/).  
  
## <a name="next-steps"></a>Další kroky

* [Příprava pevných disků pro úlohu importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Stručná referenční příručka pro často používané příkazy](../storage-import-export-tool-quick-reference-v1.md) 
