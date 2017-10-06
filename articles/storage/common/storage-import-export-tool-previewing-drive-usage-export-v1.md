---
title: "využívání jednotek aaaPreviewing úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toopreview hello seznam objektů BLOB jste vybrali pro úlohy exportu ve službě Azure Import/Export hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7378c159f6d11702cda9ae7654e84d85f9b671b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Náhled využití disku pro úlohu exportu
Než vytvoříte úlohy exportu, je třeba toochoose sadu objektů BLOB toobe exportovali. Hello služby Microsoft Azure Import/Export můžete toouse seznam objektů blob cesty nebo objekt blob předpony toorepresent hello BLOB, které jste vybrali.  
  
Dále musíte toodetermine, kolik jednotek potřebujete toosend. Hello nástroj pro Import nebo Export poskytuje hello `PreviewExport` příkaz toopreview využívání jednotek pro objekty BLOB hello jste vybrali, na základě velikosti hello jednotek hello budete toouse.

## <a name="command-line-parameters"></a>Parametry příkazového řádku

Můžete použít následující parametry při použití hello hello `PreviewExport` příkazu z hello nástroj pro Import nebo Export.

|Parametr příkazového řádku|Popis|  
|--------------------------|-----------------|  
|**/logdir:**< LogDirectory\>|Volitelné. Adresář protokolu Hello. Podrobné soubory protokolu se zapíše toothis adresáře. Pokud není zadán žádný adresář protokolu, aktuální adresář hello se použije jako adresář protokolu hello.|  
|**/sn:**< StorageAccountName\>|Povinná hodnota. Úloha exportu Hello název účtu úložiště hello hello.|  
|**/Sk:**< StorageAccountKey\>|Vyžaduje, pokud není zadán sdíleného přístupového podpisu kontejneru. Úloha exportu Hello klíč účtu pro účet úložiště hello hello.|  
|**/csas:**< ContainerSas\>|Vyžaduje, pokud není zadaný klíč účtu úložiště. Hello kontejneru SAS pro výpis hello objekty BLOB toobe exportují do úlohy exportu hello.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Povinná hodnota. Cesta toohello XML soubor obsahující seznam objektů blob cesty nebo objektu blob předpony cestu pro toobe objekty BLOB hello exportovali. Formát souboru Hello používá v hello `BlobListBlobPath` element v hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci hello REST API služby importu a exportu.|  
|**/ DriveSize:**< DriveSize\>|Povinná hodnota. Hello velikost jednotky toouse pro úlohy exportu, *například*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Příklad příkazového řádku

Hello následující příklad ukazuje hello `PreviewExport` příkaz:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Hello export souboru seznamu objektů blob může obsahovat názvy objektů blob a objektů blob předpony, jak je vidět tady:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Hello Azure Import/Export nástroj obsahuje seznam všech objektů BLOB toobe exportovali a vypočítá jak toopack do jednotky hello určená velikost, vezme v úvahu žádné režijní náklady na potřebné, pak odhadne hello počet jednotek potřebné objekty BLOB hello toohold a využití disku informace.  
  
Tady je příklad výstupu hello informační protokoly vynechání:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Další kroky

* [Referenční dokumentace Azure nástroj pro Import/Export](../storage-import-export-tool-how-to-v1.md)
