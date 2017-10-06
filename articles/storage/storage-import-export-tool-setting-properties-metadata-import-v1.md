---
title: "aaaSetting vlastnosti a metadat pomocí Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak nastavit toospecify vlastnosti a metadata toobe na objekty BLOB cílové hello při spuštění nástroje Azure Import/Export tooprepare hello jednotky. Vztahuje se toov1 hello nástroj pro Import nebo Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 66e55c2076fbcda9b78302f17b5ff2cf96bb24e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a>Vlastnosti nastavení a metadata během hello importu
Když spustíte nástroj Microsoft Azure Import/Export tooprepare hello jednotky, můžete zadat vlastnosti a metadata toobe nastavit na hello cílové objekty BLOB. Postupujte následovně:  
  
1.  vlastnosti objektu blob tooset, vytvořte textový soubor do místního počítače, který určuje názvy a hodnoty vlastností.  
  
2.  tooset metadata objektu blob, vytvořte textový soubor do místního počítače, který určuje metadata názvy a hodnoty.  
  
3.  Předat tooone hello úplnou cestu nebo oba tyto soubory toohello nástroj Azure Import/Export jako součást hello `PrepImport` operaci.  
  
> [!NOTE]
>  Když zadáte vlastnosti nebo metadata souboru v rámci relace kopírování, jsou pro každý objekt blob, který je naimportováno v rámci této relace kopie nastavit tyto vlastnosti nebo metadata. Pokud chcete pro některé objekty BLOB hello importovaných toospecify jinou sadu vlastnosti nebo metadata, budete potřebovat toocreate samostatné zkopírujte relace s jinou vlastnosti nebo soubory metadat.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Zadejte vlastnosti objektů Blob v textovém souboru  
vlastnosti objektu blob toospecify, vytvořte místní textový soubor a zahrnují kód XML, který určuje názvy vlastností jako elementy a hodnoty vlastností jako hodnoty. Tady je příklad, který určuje některé hodnoty vlastností:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Zadejte Metadata objektu Blob do textového souboru  
Podobně toospecify metadata objektu blob, vytvořte místní textový soubor, který určuje názvy metadat jako elementy a metadata hodnoty jako hodnoty. Tady je příklad, který určuje některé hodnoty metadat:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a>Vytvořit kopie relace včetně hello vlastnosti nebo soubory metadat  
Při spuštění úlohy importu hello nástroj Azure Import/Export tooprepare hello zadejte hello vlastnosti soubor na příkazovém řádku hello pomocí hello `PropertyFile` parametr. Zadejte soubor metadat hello na hello příkazového řádku pomocí hello `/MetadataFile` parametr. Tady je příklad, který určuje oba soubory:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Další kroky

* [Formát souborů metadat a vlastností služby Import/export](storage-import-export-file-format-metadata-and-properties.md)
