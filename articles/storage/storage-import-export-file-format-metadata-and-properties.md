---
title: "aaaAzure importu a exportu metadat a vlastnosti souboru formátu | Microsoft Docs"
description: "Zjistěte, jak toospecify metadata a vlastnosti pro jeden nebo více objektů BLOB, jsou součástí importu nebo exportu úlohy."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: bb13c1f1a27baea77298cb224970cd521d02d8c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure Import/Export metadat a vlastnosti souboru formát služby
Metadata a vlastnosti pro jeden nebo více objektů BLOB můžete zadat jako součást úlohy importu nebo úlohy exportu. vlastnosti pro objekty BLOB vytváří jako součást úlohy importu nebo tooset metadata, je třeba zadat soubor metadat nebo vlastnosti na hello pevný disk obsahující toobe hello data importovat. Pro úlohy exportu metadat a vlastnosti jsou zapsány do tooa metadata nebo vlastnosti souboru, který je zahrnut na pevném disku hello vrátil tooyou.  
  
## <a name="metadata-file-format"></a>Formát souboru metadat  
Hello formát souboru metadat je následující:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML Element|Typ|Popis|  
|-----------------|----------|-----------------|  
|`Metadata`|Kořenový element|kořenový element Hello hello metadata souboru.|  
|`metadata-name`|Řetězec|Volitelné. Hello – element XML určuje název hello hello metadata pro objekt blob hello a jeho hodnota určuje hello hodnotu nastavení metadata hello.|  
  
## <a name="properties-file-format"></a>Formát souboru vlastnosti  
Hello formát souboru vlastnosti vypadá takto:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|XML Element|Typ|Popis|  
|-----------------|----------|-----------------|  
|`Properties`|Kořenový element|kořenový element Hello hello vlastnosti souboru.|  
|`Last-Modified`|Řetězec|Volitelné. Hello čas poslední změny pro objekt blob hello. Pro export pouze úlohy.|  
|`Etag`|Řetězec|Volitelné. Hello hodnota ETag objektu blob. Pro export pouze úlohy.|  
|`Content-Length`|Řetězec|Volitelné. Hello velikost objektu blob hello v bajtech. Pro export pouze úlohy.|  
|`Content-Type`|Řetězec|Volitelné. Typ obsahu Hello objektu hello blob.|  
|`Content-MD5`|Řetězec|Volitelné. Hello hodnota hash MD5 objektu blob.|  
|`Content-Encoding`|Řetězec|Volitelné. Hello obsahu objektu blob kódování.|  
|`Content-Language`|Řetězec|Volitelné. Hello jazyk obsahu objektu blob.|  
|`Cache-Control`|Řetězec|Volitelné. Hello řetězec ovládací prvek mezipaměti pro objekt blob hello.|  

## <a name="next-steps"></a>Další kroky

V tématu [umožňuje nastavit vlastnosti objektu blob](/rest/api/storageservices/set-blob-properties), [nastavit Metadata objektu Blob](/rest/api/storageservices/set-blob-metadata), a [vlastnosti a metadata pro nastavení a načítání objektů blob prostředky](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) pro podrobná pravidla o nastavení vlastnosti a metadata objektu blob.
