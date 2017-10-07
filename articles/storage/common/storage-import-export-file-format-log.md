---
title: "Formát souboru protokolu importu a exportu aaaAzure | Microsoft Docs"
description: "Další informace o hello formát souborů protokolu hello vytvoří při provádění kroků pro úlohu importu/exportu služby."
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
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a>Formát souboru protokolu služby sady Azure Import/Export
Když hello služby Microsoft Azure Import/Export provede akci na disku jako součást úlohy importu nebo úlohy exportu, protokoly zapisují tooblock objekty BLOB v účtu úložiště hello přidruženého k této úlohy.  
  
Existují dva protokoly, které může být naprogramovaný hello službu Import/Export:  
  
-   v případě hello chyby vždy generování Hello protokolu chyb.  
  
-   Hello podrobného protokolování není ve výchozím nastavení povolené, ale lze ho zapnout pomocí nastavení hello `EnableVerboseLog` vlastnost [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operaci.  
  
## <a name="log-file-location"></a>Umístění souboru protokolu  
Hello se protokoly zapisují tooblock objekty BLOB v kontejneru hello nebo virtuální adresář zadaný hello `ImportExportStatesPath` nastavení, které můžete nastavit `Put Job` operaci. Hello umístění toowhich hello se protokoly zapisují závisí na tom, jak je zadán ověřování pro úlohu hello, společně s hello hodnota zadaná pro `ImportExportStatesPath`. Ověřování pro úlohu hello je možné zadat pomocí klíč účtu úložiště nebo kontejneru SAS (sdílený přístupový podpis).  
  
Hello název kontejneru hello nebo virtuální adresář může být hello výchozí název `waimportexport`, nebo jiný kontejner nebo název virtuálního adresáře, který určíte.  
  
Následující tabulka Hello ukazuje hello způsobů:  
  
|Metoda ověřování|Hodnota `ImportExportStatesPath`– Element|Umístění protokolu objektů BLOB|  
|---------------------------|----------------------------------------------|---------------------------|  
|Klíče účtu úložiště.|Výchozí hodnota|Kontejner s názvem `waimportexport`, což je výchozí kontejner hello. Například:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Klíče účtu úložiště.|Uživatelem zadanou hodnotu|Kontejner s názvem uživatelem hello. Například:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Sdíleného přístupového podpisu kontejneru|Výchozí hodnota|Virtuální adresář s názvem `waimportexport`, což je hello výchozí název, pod kontejnerem hello zadaný v hello SAS.<br /><br /> Například pokud hello SAS zadaná pro úlohu hello je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, pak by byl umístění protokolu hello`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Sdíleného přístupového podpisu kontejneru|Uživatelem zadanou hodnotu|Virtuální adresář s názvem uživatelem hello pod kontejnerem hello zadaný v hello SAS.<br /><br /> Například pokud hello SAS zadaná pro úlohu hello je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, a hello zadaný virtuální adresář název `mylogblobs`, pak by byl umístění protokolu hello `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
Adresy URL hello hello chyba a podrobné protokoly můžete načíst pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci. Hello protokoly jsou k dispozici po dokončení zpracování jednotky hello.  
  
## <a name="log-file-format"></a>Formát souboru protokolu  
Hello formát pro oba protokoly je hello stejné: Objekt blob, který obsahuje XML popis hello událostí, které došlo k chybě při kopírování objekty BLOB mezi hello pevný disk a účet hello zákazníka.  
  
Úplné protokolování Hello obsahuje kompletní informace o stavu hello hello kopírování pro každý objekt blob (pro úlohy importu) nebo soubor (pro úlohy exportu), zatímco hello chyba protokol obsahuje jenom hello informace pro objekty BLOB nebo soubory, které se vyskytly chyby během hello importovat nebo exportovat úlohy.  
  
Formát Hello podrobného protokolování jsou uvedeny níže. Protokol chyb Hello má hello stejné struktury, ale filtruje úspěšné operace.  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

Hello následující tabulka popisuje elementy hello hello souboru protokolu.  
  
|XML Element|Typ|Popis|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML Element|Představuje jednotku protokolu.|  
|`Version`|Atribut, řetězec|Hello verzi formátu protokolu hello.|  
|`DriveId`|Řetězec|Hello jednotky hardwaru sériové číslo.|  
|`Status`|Řetězec|Stav zpracování jednotky hello. V tématu hello `Drive Status Codes` tabulky níže Další informace.|  
|`Blob`|Vnořené – element XML|Představuje objekt blob.|  
|`Blob/BlobPath`|Řetězec|identifikátor URI objektu hello blob Hello.|  
|`Blob/FilePath`|Řetězec|Hello relativní cestu toohello soubor na disku hello.|  
|`Blob/Snapshot`|Data a času|Hello snímek aktuální verze objektu blob hello pro úlohu exportu.|  
|`Blob/Length`|Integer|Hello celková délka objektu blob hello v bajtech.|  
|`Blob/LastModified`|Data a času|Hello datum a čas poslední změny tohoto objektu blob hello pro úlohu exportu.|  
|`Blob/ImportDisposition`|Řetězec|Hello importovat dispozice hello objekt blob pro úlohu importu.|  
|`Blob/ImportDisposition/@Status`|Atribut, řetězec|Stav Hello hello importovat dispozice.|  
|`PageRangeList`|Vnořené – element XML|Představuje seznam rozsahů stránek pro objekt blob stránky.|  
|`PageRange`|XML element|Představuje rozsahu stránek.|  
|`PageRange/@Offset`|Atribut, celé číslo|Počáteční posun rozsahu stránku hello v objektu blob hello.|  
|`PageRange/@Length`|Atribut, celé číslo|Délka v bajtech hello rozsahu stránek.|  
|`PageRange/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 rozsahu stránku hello.|  
|`PageRange/@Status`|Atribut, řetězec|Stav zpracování hello rozsahu stránek.|  
|`BlockList`|Vnořené – element XML|Představuje seznam bloků pro objekt blob bloku.|  
|`Block`|XML element|Představuje blok.|  
|`Block/@Offset`|Atribut, celé číslo|Počáteční odsazení hello blok v objektu blob hello.|  
|`Block/@Length`|Atribut, celé číslo|Délka v bajtech hello bloku.|  
|`Block/@Id`|Atribut, řetězec|ID bloku Hello.|  
|`Block/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 hello bloku.|  
|`Block/@Status`|Atribut, řetězec|Stav zpracování hello bloku.|  
|`Metadata`|Vnořené – element XML|Představuje metadata objektu blob hello.|  
|`Metadata/@Status`|Atribut, řetězec|Stav zpracování hello metadata objektu blob.|  
|`Metadata/GlobalPath`|Řetězec|Relativní cesta toohello globální metadata souboru.|  
|`Metadata/GlobalPath/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 hello globální metadata souboru.|  
|`Metadata/Path`|Řetězec|Soubor metadat toohello relativní cestu.|  
|`Metadata/Path/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 hello metadata souboru.|  
|`Properties`|Vnořené – element XML|Představuje vlastnosti objektu blob hello.|  
|`Properties/@Status`|Atribut, řetězec|Stav zpracování hello vlastnosti objektů blob, například soubor nebyl nalezen, byla dokončena.|  
|`Properties/GlobalPath`|Řetězec|Relativní cesta toohello globální vlastnosti souboru.|  
|`Properties/GlobalPath/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 hello globální vlastnosti souboru.|  
|`Properties/Path`|Řetězec|Relativní cesta toohello vlastnosti souboru.|  
|`Properties/Path/@Hash`|Atribut, řetězec|Kódováním Base16 hodnota hash MD5 hello vlastnosti souboru.|  
|`Blob/Status`|Řetězec|Stav zpracování objektu blob hello.|  
  
# <a name="drive-status-codes"></a>Jednotka stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro zpracování na jednotku.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Completed`|jednotka Hello dokončil zpracování bez chyb.|  
|`CompletedWithWarnings`|jednotka Hello dokončení zpracování se upozornění v jedné nebo více objektů blob na potížemi import hello zadaný pro objekty BLOB hello.|  
|`CompletedWithErrors`|jednotka Hello bylo dokončeno s chybami v jedné nebo více objektů BLOB nebo bloky.|  
|`DiskNotFound`|Žádný disk nachází na jednotce hello.|  
|`VolumeNotNtfs`|Hello první datový svazek na disku hello není ve formátu systému souborů NTFS.|  
|`DiskOperationFailed`|Při provádění operací na jednotce hello došlo k neznámé chybě.|  
|`BitLockerVolumeNotFound`|Nebyla nalezena žádná encryptable svazku Bitlockeru.|  
|`BitLockerNotActivated`|BitLocker není na svazku hello povolené.|  
|`BitLockerProtectorNotFound`|ochranné zařízení klíče Hello číselné heslo na svazku hello neexistuje.|  
|`BitLockerKeyInvalid`|Hello číselné heslo zadané nemůžete odemknout hello svazku.|  
|`BitLockerUnlockVolumeFailed`|Došlo k neznámé chybě došlo při pokusu o toounlock hello svazku.|  
|`BitLockerFailed`|Při provádění operací se BitLocker došlo k neznámé chybě.|  
|`ManifestNameInvalid`|Název souboru manifestu Hello je neplatný.|  
|`ManifestNameTooLong`|Název souboru manifestu Hello je příliš dlouhý.|  
|`ManifestNotFound`|Hello soubor manifestu nebyl nalezen.|  
|`ManifestAccessDenied`|Soubor manifestu toohello přístup byl odepřen.|  
|`ManifestCorrupted`|Hello souboru manifestu je poškozený (hello obsah neodpovídá jeho hash).|  
|`ManifestFormatInvalid`|Obsah manifestu Hello nesplňuje požadovaný formát toohello.|  
|`ManifestDriveIdMismatch`|Hello jednotku, kterou neodpovídá ID v souboru manifestu hello hello jeden pro čtení z disku hello.|  
|`ReadManifestFailed`|Při čtení z manifestu hello došlo k vstupně-výstupních operací selhání disku.|  
|`BlobListFormatInvalid`|Hello export objektů blob seznamu objektů blob nesplňuje požadovaný formát toohello.|  
|`BlobRequestForbidden`|Přístup k objektům BLOB toohello v účtu úložiště hello je zakázáno. Může to být kvůli tooinvalid klíč účtu úložiště nebo sdíleného přístupového podpisu kontejneru.|  
|`InternalError`|A při zpracování hello jednotce došlo k vnitřní chybě.|  
  
## <a name="blob-status-codes"></a>Objekt BLOB stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro zpracování objektu blob.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Completed`|Objekt blob Hello dokončil zpracování bez chyb.|  
|`CompletedWithErrors`|Objekt blob Hello dokončil zpracování s chybami v jedné nebo více rozsahů stránek nebo bloky, metadata nebo vlastnosti.|  
|`FileNameInvalid`|Název souboru Hello je neplatný.|  
|`FileNameTooLong`|Název souboru Hello je příliš dlouhý.|  
|`FileNotFound`|Hello soubor nebyl nalezen.|  
|`FileAccessDenied`|Soubor toohello přístup byl odepřen.|  
|`BlobRequestFailed`|požadavek na službu Blob Hello, že tooaccess hello objekt blob se nezdařilo.|  
|`BlobRequestForbidden`|požadavek na službu Blob Hello, že tooaccess hello objektu blob je zakázán. Může to být kvůli tooinvalid klíč účtu úložiště nebo sdíleného přístupového podpisu kontejneru.|  
|`RenameFailed`|Objekt blob hello toorename (pro úlohy importu) nebo soubor hello (pro úlohy exportu) se nezdařilo.|  
|`BlobUnexpectedChange`|S objektem blob hello (pro úlohy exportu) došlo k neočekávané chování.|  
|`LeasePresent`|Není přítomen u objektu blob hello zapůjčení.|  
|`IOFailed`|Na disk nebo sítě vstupně-výstupních operací selhání došlo k chybě při zpracování objektu blob hello.|  
|`Failed`|Při zpracování objektu blob hello došlo k neznámé chybě.|  
  
## <a name="import-disposition-status-codes"></a>Import dispozice stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro řešení importu dispozice.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Created`|vytvořil se objekt blob Hello.|  
|`Renamed`|Hello blob byl přejmenován na přejmenování import dispozice. Hello `Blob/BlobPath` element obsahuje hello identifikátor URI pro objekt blob hello přejmenovat.|  
|`Skipped`|Objekt blob Hello byl vynechán za `no-overwrite` importovat dispozice.|  
|`Overwritten`|Objekt blob Hello má přepsat existující objekt blob za `overwrite` importovat dispozice.|  
|`Cancelled`|Předchozí selhání byla zastavena, další zpracování hello import dispozice.|  
  
## <a name="page-rangeblock-status-codes"></a>Stránka rozsah/blok stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro zpracování stránky rozsah nebo blok.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Completed`|Hello stránky rozsah nebo blok dokončení zpracování bez chyb.|  
|`Committed`|blok Hello potvrzeny, ale v hello úplné blokovaných protože jiné bloky se nezdařily, nebo put samotný seznam úplné bloku nedošlo k chybě.|  
|`Uncommitted`|Hello bloku je odeslat, ale nikoli potvrdit.|  
|`Corrupted`|Hello stránky rozsah nebo blok je poškozený (hello obsah neodpovídá jeho hash).|  
|`FileUnexpectedEnd`|Byl nalezen neočekávaný konec souboru.|  
|`BlobUnexpectedEnd`|Byl nalezen neočekávaný konec objektu blob.|  
|`BlobRequestFailed`|Hello žádost o služby objektů Blob, že tooaccess hello rozsahu stránek nebo bloku selhal.|  
|`IOFailed`|Na disk nebo síťový vstupně-výstupní chybě došlo k chybě při zpracování hello stránky rozsah nebo blok.|  
|`Failed`|Při zpracování hello stránky rozsah nebo blok došlo k neznámé chybě.|  
|`Cancelled`|Předchozí selhání byla zastavena, další zpracování rozsahu stránek hello nebo bloku.|  
  
## <a name="metadata-status-codes"></a>Metadata stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro zpracování metadata objektu blob.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Completed`|Hello metadata dokončil zpracování bez chyb.|  
|`FileNameInvalid`|Název souboru metadat Hello je neplatný.|  
|`FileNameTooLong`|Název souboru metadat Hello je příliš dlouhý.|  
|`FileNotFound`|Soubor metadat Hello nebyla nalezena.|  
|`FileAccessDenied`|Soubor metadat toohello přístup byl odepřen.|  
|`Corrupted`|je poškozený soubor metadat Hello (hello obsah neodpovídá jeho hash).|  
|`XmlReadFailed`|Hello metadata obsahu neodpovídá toohello požadovaný formát.|  
|`XmlWriteFailed`|Zápis metadat hello XML se nezdařila.|  
|`BlobRequestFailed`|Hello Blob žádosti o službu, že tooaccess hello metadat se nezdařila.|  
|`IOFailed`|Při zpracování hello metadat došlo k chybě vstupně-výstupních operací disku nebo v síti.|  
|`Failed`|Při zpracování metadat hello došlo k neznámé chybě.|  
|`Cancelled`|Předchozí selhání byla zastavena, další zpracování hello metadat.|  
  
## <a name="properties-status-codes"></a>Vlastnosti stavové kódy  
Hello následující tabulka uvádí hello stavové kódy pro zpracování vlastnosti objektů blob.  
  
|Stavový kód|Popis|  
|-----------------|-----------------|  
|`Completed`|Vlastnosti Hello dokončení zpracování bez chyb.|  
|`FileNameInvalid`|Název souboru Hello vlastnosti je neplatný.|  
|`FileNameTooLong`|Název souboru vlastnosti Hello je příliš dlouhý.|  
|`FileNotFound`|Hello vlastnosti soubor nebyl nalezen.|  
|`FileAccessDenied`|Soubor vlastnosti toohello přístup byl odepřen.|  
|`Corrupted`|Hello vlastnosti soubor je poškozený (hello obsah neodpovídá jeho hash).|  
|`XmlReadFailed`|Hello vlastnosti obsah není v souladu s toohello požadovaný formát.|  
|`XmlWriteFailed`|Zápis hello vlastnosti, které XML se nezdařila.|  
|`BlobRequestFailed`|požadavek na službu Blob Hello, že tooaccess hello vlastnosti se nezdařilo.|  
|`IOFailed`|Při zpracování hello vlastnosti došlo k chybě vstupně-výstupních operací disku nebo v síti.|  
|`Failed`|Při zpracování hello vlastnosti došlo k neznámé chybě.|  
|`Cancelled`|Předchozí selhání byla zastavena, další zpracování hello vlastností.|  
  
## <a name="sample-logs"></a>Ukázka protokoly  
Hello následuje příklad podrobného protokolování.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Protokol chyb odpovídající Hello je uveden níže.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 Hello postupujte podle kroků v protokolu chyb úlohy importu obsahuje chybu o soubor nebyl nalezen na disku import hello. Upozorňujeme, že se stav hello následující součásti `Cancelled`.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

Hello následující v protokolu chyb úlohy exportu označuje, že hello obsah objektu blob byla úspěšně zapsána toohello disku, ale, že došlo k chybě při exportu hello blob vlastnosti.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a>Další kroky
 
* [REST API pro Import/Export úložiště](/rest/api/storageimportexport/)
