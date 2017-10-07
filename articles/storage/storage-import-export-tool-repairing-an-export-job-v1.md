---
title: "aaaRepairing úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toorepair úlohu export, který byl vytvořen a spustí pomocí hello Azure Import/Export služby."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 96c674fc7c697c37882fb2980c340303896ac6c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-export-job"></a>Oprava úlohy exportu
Po dokončení úlohy exportu, můžete spustit hello Microsoft Azure Import/Export nástroj místně pro:  
  
1.  Stáhněte všechny soubory, aby byla služba Azure Import/Export hello nelze tooexport.  
  
2.  Ověření, aby byly správně exportovány hello souborů na disku hello.  
  
Tato funkce musí mít připojení k tooAzure úložiště toouse.  
  
příkaz Hello pro opravu úlohy importu je **RepairExport**.

## <a name="repairexport-parameters"></a>Parametry RepairExport

Hello mohou být zadány následující parametry s **RepairExport**:  
  
|Parametr|Popis|  
|---------------|-----------------|  
|**/ r: < RepairFile\>**|Povinná hodnota. Cesta toohello oprava souboru, který sleduje hello průběh hello opravy a můžete tooresume opravu dojde k přerušení. Každé jednotky, musí mít pouze jeden soubor opravit. Když spustíte opravy pro danou jednotku, předáte bude v hello cesta tooa oprava souboru, který ještě neexistuje. tooresume opravu dojde k přerušení, by měla předávat název hello k existujícímu souboru opravit. Hello opravit soubor odpovídající toohello cílová jednotka je vždy třeba zadat.|  
|**/logdir: < LogDirectory\>**|Volitelné. Adresář protokolu Hello. Podrobné soubory protokolu se zapíše toothis adresáře. Pokud není zadán žádný adresář protokolu, aktuální adresář hello se použije jako adresář protokolu hello.|  
|**/ d: < TargetDirectory\>**|Povinná hodnota. Hello directory toovalidate a opravy. To je obvykle hello kořenový adresář jednotky export hello, ale může také být do síťové sdílené složky obsahující kopii hello exportované soubory.|  
|**/BK: < BitLockerKey\>**|Volitelné. Pokud chcete, aby toounlock nástroj hello šifrované kdy hello exportovaných souborů jsou uložené musíte zadat klíč nástroje BitLocker hello.|  
|**/sn: < StorageAccountName\>**|Povinná hodnota. Úloha exportu Hello název účtu úložiště hello hello.|  
|**/Sk: < StorageAccountKey\>**|**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru. Úloha exportu Hello klíč účtu pro účet úložiště hello hello.|  
|**/csas: < ContainerSas\>**|**Požadované** jenom v případě hello klíč účtu úložiště není zadán. kontejner Hello SAS pro přístup k objektům BLOB hello spojené s úlohou export hello.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Povinná hodnota. Hello cesta toohello jednotky kopírování souboru protokolu. soubor Hello je generován hello služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob hello spojené s úlohou hello. Hello kopie protokolový soubor obsahuje informace o selhání objektů BLOB nebo soubory, které jsou toobe opravit.|  
|**/ ManifestFile: < DriveManifestFile\>**|Volitelné. Soubor manifestu Hello cesta toohello export jednotky. Tento soubor je vygenerované službou Windows Azure Import/Export hello a uložené na jednotce export hello a volitelně do objektu BLOB v účtu úložiště hello spojené s úlohou hello.<br /><br /> obsah hello souborů na jednotce export hello Hello se ověří pomocí hodnot hash MD5 hello obsažené v tomto souboru. Všechny soubory, které jsou určené toobe poškozený budou staženy a přepsaná toohello cíl adresáře.|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a>Pomocí RepairExport toocorrect režimu se nezdařilo exporty  
Můžete použít hello nástroj Azure Import/Export toodownload soubory, které tooexport se nezdařilo. soubor protokolu Hello kopie bude obsahovat seznam souborů, které se nezdařily tooexport.  
  
Hello selhání exportu příčiny hello následující možnosti:  
  
-   Poškozené jednotky  
  
-   klíč účtu úložiště Hello změnit během přenosu hello  
  
Nástroj hello toorun v **RepairExport** režim, musíte nejprve tooconnect hello jednotce, která obsahuje počítače tooyour hello exportované soubory. Potom spustíte hello nástroj Azure Import/Export, zadáním hello cesta toothat jednotky s hello `/d` parametr. Musíte taky toospecify hello cesta toohello jednotku na kopírování protokolu souboru, který jste stáhli. Hello následující příkazový řádek příklad spustí nástroj toorepair hello všechny soubory, které se nezdařilo tooexport:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Hello následuje příklad kopírování protokolového souboru, který zobrazuje tento jeden blok v tooexport hello objekt blob se nezdařilo:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
soubor protokolu kopie Hello označuje, že došlo k selhání při hello služby Windows Azure Import/Export byl stahování mezi hello blob bloků toohello soubor na disku export hello. Hello ostatní součásti hello soubor stažený úspěšně, a délka souboru hello byla správně nastavená. Nástroj hello budou v takovém případě otevřete hello soubor na disku hello, stáhněte hello bloku z účtu úložiště hello a zapisuje toohello souboru rozsah od posun 65536 s délkou 65536.  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a>Pomocí RepairExport toovalidate jednotky obsah  
Můžete také použít Azure Import/Export s hello **RepairExport** možnost toovalidate hello obsah na jednotce hello jsou správné. Soubor manifestu Hello na každém disku exportu obsahuje MD5s pro obsah hello hello jednotky.  
  
Hello služba Azure Import/Export můžete také ukládat soubory manifestu hello účet úložiště tooa během procesu exportu hello. Hello umístění souborů manifestu hello je k dispozici prostřednictvím hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci po dokončení úlohy hello. V tématu [službu Import/Export formát souboru manifestu](storage-import-export-file-format-metadata-and-properties.md) Další informace o hello formát souboru manifestu jednotky.  
  
Hello následující příklad ukazuje, jak toorun hello Azure Import/Export nástroj s hello **/ManifestFile** a **/CopyLogFile** parametry:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Hello tady je příklad souboru manifestu:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Po dokončení procesu opravy hello bude nástroj hello si přečíst každý soubor v souboru manifestu hello odkazuje a ověří integritu souboru hello s hello MD5 hodnoty hash. Pro manifest hello výše protože budou přenášeny prostřednictvím hello následující součásti.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Všechny součásti, které selhání ověření hello bude stažena nástrojem hello a přepsaná toohello stejný soubor na disku hello.  
  
## <a name="next-steps"></a>Další kroky
 
* [Hello nastavení se nástroj Azure Import/Export](storage-import-export-tool-setup-v1.md)   
* [Příprava pevných disků pro úlohu importu](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kontrola stavu úlohy s použitím kopií souborů protokolu](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Oprava úlohy importu](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Řešení potíží s hello nástroj Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
