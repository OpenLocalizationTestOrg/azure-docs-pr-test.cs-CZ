---
title: "aaaRepairing úlohy importu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toorepair úlohu importu, který byl vytvořen a spustí pomocí hello Azure Import/Export služby."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: a9ed81f50cffd8ae6e0cb21b25a04815c2b51ee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-import-job"></a>Oprava úlohy importu
Hello služby Microsoft Azure Import/Export může selhat toocopy některé soubory nebo části souboru toohello služby Windows Azure Blob. Chyby z těchto důvodů:  
  
-   Poškozené soubory  
  
-   Poškozené jednotky  
  
-   klíč účtu úložiště Hello změnila. Přestože hello souboru během přenosu.  
  
Můžete spustit hello nástroj Microsoft Azure Import/Export hello importu souborů protokolu kopie úlohy a nástroj hello odešlete hello chybějící soubory (nebo její části souboru) tooyour systému Windows Azure storage účet toocomplete úlohy importu.  
  
## <a name="repairimport-parameters"></a>Parametry RepairImport

Hello mohou být zadány následující parametry s **RepairImport**: 
  
|||  
|-|-|  
|**/ r:**< RepairFile\>|**Vyžaduje se.** Cesta toohello oprava souboru, který sleduje hello průběh hello opravy a můžete tooresume opravu dojde k přerušení. Každé jednotky, musí mít pouze jeden soubor opravit. Když spustíte opravy pro danou jednotku, předáte bude v hello cesta tooa oprava souboru, který ještě neexistuje. tooresume opravu dojde k přerušení, by měla předávat název hello k existujícímu souboru opravit. Hello opravit soubor odpovídající toohello cílová jednotka je vždy třeba zadat.|  
|**/logdir:**< LogDirectory\>|**Volitelný parametr.** Adresář protokolu Hello. Podrobné soubory protokolu se zapíše toothis adresáře. Pokud není zadán žádný adresář protokolu, aktuální adresář hello se použije jako adresář protokolu hello.|  
|**/ d:**< TargetDirectories\>|**Vyžaduje se.** Jeden nebo více oddělených středníkem adresáře, které obsahují hello původní soubory, které byly naimportovány. Hello import disku mohou být využity také, ale pokud jsou k dispozici alternativní umístění původního souborů není potřeba.|  
|**/BK:**< BitLockerKey\>|**Volitelný parametr.** Pokud chcete, aby toounlock hello nástroj pro zašifrované jednotky, které jsou k dispozici původní soubory hello musíte zadat klíč nástroje BitLocker hello.|  
|**/sn:**< StorageAccountName\>|**Vyžaduje se.** Úloha importu Hello název účtu úložiště hello hello.|  
|**/Sk:**< StorageAccountKey\>|**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru. Úloha importu Hello klíč účtu pro účet úložiště hello hello.|  
|**/csas:**< ContainerSas\>|**Požadované** jenom v případě hello klíč účtu úložiště není zadán. kontejner Hello SAS pro přístup k BLOB hello spojené s úlohou import hello.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Vyžaduje se.** Cesta toohello jednotky kopie protokolový soubor (podrobného protokolování nebo v protokolu chyb). soubor Hello je generován hello služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob hello spojené s úlohou hello. Hello kopie protokolový soubor obsahuje informace o selhání objektů BLOB nebo soubory, které jsou toobe opravit.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**Volitelný parametr.** Cesta tooa textový soubor, který lze použít tooresolve mnohoznačnosti, pokud máte víc souborů s hello stejný název, aby byly importu v hello stejnou úlohu. Hello první čas hello nástroj spouští, se může vyplnit tento soubor se všemi nejednoznačné názvy hello. Při dalším spuštění nástroje hello použije tento soubor tooresolve hello nejednoznačnosti.|  
  
## <a name="using-hello-repairimport-command"></a>Pomocí příkazu RepairImport hello  
toorepair importovat data pomocí vysílání datového proudu hello data hello sítě, je nutné zadat hello hello adresáře, které obsahují hello původní soubory byly import pomocí `/d` parametr. Musíte také zadat hello kopie protokol, který jste stáhli z vašeho účtu úložiště. Typické příkazového řádku toorepair úlohy importu s chybami částečné vypadá takto:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Hello tady je příklad kopírování souboru protokolu. V tomto případě jednoho 64 tisíc část soubor byl poškozen na jednotce hello, která byla odeslaná úlohy importu hello. Protože se jedná pouze selhání hello uvedené, hello zbytek hello objektů BLOB v hello úlohy byly úspěšně importovány.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
Když tento protokol kopie předána toohello nástroj Azure Import/Export, hello nástroj pokusí zkopírováním hello chybějící obsah v síti hello toofinish hello import tohoto souboru. Následující příklad hello výše, bude nástroj hello hledat hello původní soubor `\animals\koala.jpg` v rámci hello dva adresáře `C:\Users\bob\Pictures` a `X:\BobBackup\photos`. Pokud hello souboru `C:\Users\bob\Pictures\animals\koala.jpg` existuje, hello nástroj Azure Import/Export zkopíruje hello chybějící rozsah datový objekt blob odpovídající toohello `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Řešení konfliktů při použití RepairImport  
V některých situacích hello nástroj nemusí být možné toofind nebo otevřete hello nezbytné soubor pro jednu z následujících důvodů hello: hello soubor nebyl nalezen, hello soubor není dostupný, název souboru hello je nejednoznačný nebo hello obsah souboru hello již není správný.  
  
Nejednoznačné chybě může dojít, pokud nástroj hello se pokouší toolocate `\animals\koala.jpg` a existuje soubor s tímto názvem v obou `C:\Users\bob\pictures` a `X:\BobBackup\photos`. To znamená i `C:\Users\bob\pictures\animals\koala.jpg` a `X:\BobBackup\photos\animals\koala.jpg` existovat na jednotkách úlohy importu hello.  
  
Hello `/PathMapFile` možnost povolit, tooresolve tyto chyby. Můžete zadat název hello souboru hello, obsahující hello seznam souborů, které hello nástroj nebylo možné určit toocorrectly. Hello následuje příklad příkazového řádku, na které by naplňuje `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Nástroj Hello pak zapíše hello problematické cesty příliš`9WM35C2V_pathmap.txt`, každou na jeden řádek. Například může obsahovat soubor hello hello po spuštění příkazu hello následující položky:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Pro každý soubor v seznamu hello by měl pokusit toolocate a otevřete tooensure souboru hello je k dispozici toohello nástroj. Pokud chcete nástroj hello tootell explicitně tam, kde můžete upravit toofind souboru, cesta hello mapování souboru a přidejte hello cesta tooeach soubor na hello stejném řádku, oddělených tabulátorem:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Po provedení hello potřebné soubory k dispozici toohello nástroje nebo aktualizace souboru s mapováním cesta hello můžete znovu spustit proces importu hello nástroj toocomplete hello.  
  
## <a name="next-steps"></a>Další kroky
 
* [Hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup-v1.md)   
* [Příprava pevných disků pro úlohu importu](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kontrola stavu úlohy s použitím kopií souborů protokolu](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Oprava úlohy exportu](storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Řešení potíží s hello nástroj Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
