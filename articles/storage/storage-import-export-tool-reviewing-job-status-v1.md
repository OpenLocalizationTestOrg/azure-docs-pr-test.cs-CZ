---
title: "aaaReviewing stav úlohy Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toouse hello protokolu soubory vytvořené při hello importovat nebo exportovat úloha byla spuštěna toosee hello stav úlohy importu a exportu hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: 378bc9a7ef6bfe65209413c8c4134f313a2c0d79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Kontrola stavu úlohy Azure Import/Export s kopírovat soubory protokolu
Když hello služby Microsoft Azure Import/Export zpracovává jednotky, které jsou přidružené úloze importu nebo exportu, zapíše kopie protokolu soubory toohello úložiště účet tooor ze kterého jsou import nebo export objektů BLOB. Hello protokolový soubor obsahuje podrobné informace o stavu o každý soubor, který byl importovat nebo exportovat. soubor protokolu kopie tooeach URL Hello je vrácena, pokud dotaz hello stav dokončené úlohy; v tématu [Get Job](/rest/api/storageservices/Get-Job3) Další informace.  

## <a name="example-urls"></a>Příklad adresy URL

Příklad adresy URL pro kopírování souborů protokolu pro úlohy importu se dvěma disky se Hello následující:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 V tématu [službu Import/Export formát souboru protokolu](storage-import-export-file-format-log.md) pro formát hello kopírování protokolů a hello úplný seznam stavových kódů.  
  
## <a name="next-steps"></a>Další kroky
 
 * [Hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup-v1.md)   
 * [Příprava pevných disků pro úlohu importu](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Oprava úlohy importu](storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Oprava úlohy exportu](storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Řešení potíží s hello nástroj Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
