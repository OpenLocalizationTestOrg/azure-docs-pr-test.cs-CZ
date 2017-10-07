---
title: "aaaQuick reference pro příkazy úlohy Azure Import/Export nástroj import - v1 | Microsoft Docs"
description: "Azure Import/Export nástroj informace o příkazech pro import často používané příkazy úlohy. Vztahuje se toov1 hello nástroj pro Import nebo Export."
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
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 945eb4f9eff28c92ec963585d27cba73a7eb59bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Stručná referenční příručka pro často používané příkazy pro úlohy importu
Tato část poskytuje že rychlý přehled o některé často používané příkazy. Podrobné informace o použití, najdete v části [Příprava pevné disky pro úlohy importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-toohello-hard-drive"></a>Příprava na pevný disk, když data již byla zkopírována toohello pevný disk
 Hello následující příkaz připraví na pevný disk, když data již byla zkopírovaný tooit, ale nebyla ještě šifrované pomocí nástroje BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a>Zkopírujte na jednoho adresářového tooa pevný disk  
 Následující příkaz Hello zkopíruje jednoho zdroje directory tooa pevný disk, který nebyl ještě šifrované pomocí nástroje BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-tooa-hard-drive"></a>Kopírování dva adresáře tooa pevný disk  
 toocopy dva zdrojového adresáře tooa jednotka, hello použijte následující příkazy:  
  
 Určuje adresář protokolu hello, klíč účtu úložiště, cílový písmeno jednotky, zprostředkovatele Hello první příkaz `format/encrypt` požadavky a společné parametry:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 druhý příkaz Hello Určuje soubor hello deníku, nové ID relace a hello zdrojové a cílové umístění:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a>Kopírování velkého souboru tooa pevný disk v relaci druhé kopie  
 Následující příkaz Hello zkopíruje jeden velký soubor tooa pevný disk, který byl připraven v předchozí relaci kopie:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Další kroky

* [Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
