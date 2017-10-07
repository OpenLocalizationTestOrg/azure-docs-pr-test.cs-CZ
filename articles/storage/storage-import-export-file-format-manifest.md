---
title: "Formát souboru manifestu importu a exportu aaaAzure | Microsoft Docs"
description: "Informace o hello formát souboru manifestu hello disku, který popisuje hello mapování mezi objekty BLOB v Azure Blob storage a souborů na disku v úlohu import nebo export ve službě Import/Export hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f3119e1c-2c25-48ad-8752-a6ed4adadbb0
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d7e5e1990482916f7ff5f891c97343b52e82b2f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure formát souboru manifestu služby importu a exportu
Soubor manifestu jednotky Hello popisuje hello mapování mezi objekty BLOB v Azure Blob storage a souborů na jednotce, která obsahuje úlohu import nebo export. Operace importu souboru manifestu hello je vytvořen jako součást procesu přípravy hello jednotky a je uložená na disku hello před odesláním disku hello toohello datového centra Azure. Během operace exportu se hello manifest vytvoří a uloží na jednotce hello podle hello služba Azure Import/Export.  
  
Jak importovat a úlohy exportu souboru manifestu hello disk je uložený na hello importovat nebo exportovat jednotky; není přenášených toohello service pomocí všechny operace rozhraní API.  
  
Hello následující text popisuje hello obecný formát souboru manifestu jednotky:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveManifest Version="2014-11-01">  
  <Drive>  
    <DriveId>drive-id</DriveId>  
    import-export-credential  
  
    <!-- First Blob List -->  
    <BlobList>  
      <!-- Global properties and metadata that applies tooall blobs -->  
      [<MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>]  
      [<PropertiesPath   
        Hash="md5-hash">global-properties-file-path</PropertiesPath>]  
  
      <!-- First Blob -->  
      <Blob>  
        <BlobPath>blob-path-relative-to-account</BlobPath>  
        <FilePath>file-path-relative-to-transfer-disk</FilePath>  
        [<ClientData>client-data</ClientData>]  
        [<Snapshot>snapshot</Snapshot>]  
        <Length>content-length</Length>  
        [<ImportDisposition>import-disposition</ImportDisposition>]  
        page-range-list-or-block-list          
        [<MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>]  
        [<PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>]  
      </Blob>  
  
      <!-- Second Blob -->  
      <Blob>  
      . . .  
      </Blob>  
    </BlobList>  
  
    <!-- Second Blob List -->  
    <BlobList>  
    . . .  
    </BlobList>  
  </Drive>  
</DriveManifest>  
  
import-export-credential ::=   
  <StorageAccountKey>storage-account-key</StorageAccountKey> | <ContainerSas>container-sas</ContainerSas>  
  
page-range-list-or-block-list ::=   
  page-range-list | block-list  
  
page-range-list ::=   
    <PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
    </PageRangeList>  
  
block-list ::=  
    <BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       Hash="md5-hash"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       Hash="md5-hash"/>]  
    </BlockList>  

```

## <a name="manifest-xml-elements-and-attributes"></a>Manifestu XML elementů a atributů

Hello data elementů a atributů hello jednotky manifestu XML formátu jsou určené v hello následující tabulka.  
  
|XML Element|Typ|Popis|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Kořenový element|kořenový element Hello souboru manifestu hello. Všechny elementy v souboru hello jsou pod tohoto elementu.|  
|`Version`|Atribut, řetězec|Hello verze souboru manifestu hello.|  
|`Drive`|Vnořené – element XML|Obsahuje hello manifest pro každou jednotku.|  
|`DriveId`|Řetězec|Hello jednotky jedinečný identifikátor pro hello jednotky. identifikátor disku Hello nachází dotazováním hello jednotky pro jeho sériového čísla. Hello jednotky sériové číslo je obvykle vytištěno na hello mimo také hello jednotky. Hello `DriveID` element musí být před spuštěním `BlobList` element v souboru manifestu hello.|  
|`StorageAccountKey`|Řetězec|Požadovaný pro import úlohy Pokud a pouze v případě `ContainerSas` není zadán. Hello klíč účtu pro hello účtu úložiště Azure spojené s úlohou hello.<br /><br /> Tento element je vynechaný hello manifestu pro operace exportu.|  
|`ContainerSas`|Řetězec|Požadovaný pro import úlohy Pokud a pouze v případě `StorageAccountKey` není zadán. kontejner Hello SAS pro přístup k objektům BLOB hello spojené s úlohou hello. V tématu [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) pro formát. Tento element je vynechaný hello manifestu pro operace exportu.|  
|`ClientCreator`|Řetězec|Určuje hello klienta, který vytvoří soubor XML hello. Tato hodnota není interpretovat hello služby importu a exportu.|  
|`BlobList`|Vnořené – element XML|Obsahuje seznam objektů BLOB, které jsou součástí hello import nebo export úlohy. Každý objekt blob v seznamu objektů blob sdílené složky hello stejnou metadata a vlastnosti.|  
|`BlobList/MetadataPath`|Řetězec|Volitelné. Určuje hello relativní cestu k souboru na hello disk, který obsahuje hello výchozí metadata, která bude nastavena na objekty BLOB v seznamu objektů blob hello operace importu. Tato metadata lze volitelně přepsat na základě objektů blob blob.<br /><br /> Tento element je vynechaný hello manifestu pro operace exportu.|  
|`BlobList/MetadataPath/@Hash`|Atribut, řetězec|Určuje hodnotu hash MD5 kódováním Base16 hello soubor metadat hello.|  
|`BlobList/PropertiesPath`|Řetězec|Volitelné. Určuje hello relativní cestu k souboru na hello disku, který obsahuje hello výchozí vlastnosti, které budou nastaveny na objekty BLOB v seznamu objektů blob hello operace importu. Tyto vlastnosti lze volitelně přepsat na základě objektů blob blob.<br /><br /> Tento element je vynechaný hello manifestu pro operace exportu.|  
|`BlobList/PropertiesPath/@Hash`|Atribut, řetězec|Určuje hodnotu hash MD5 kódováním Base16 hello hello vlastnosti souboru.|  
|`Blob`|Vnořené – element XML|Obsahuje informace o jednotlivých objektů blob v každém seznamu objektů blob.|  
|`Blob/BlobPath`|Řetězec|Hello relativní identifikátor URI toohello objektů blob, počínaje hello název kontejneru. Pokud hello objektů blob v kontejneru kořenové, musí začínat `$root`.|  
|`Blob/FilePath`|Řetězec|Určuje soubor toohello hello relativní cesty na jednotce hello. Pro úlohy exportu cesta blobu hello se použije pro cestu souboru hello Pokud je to možné; *například*, `pictures/bob/wild/desert.jpg` vyexportují se příliš`\pictures\bob\wild\desert.jpg`. Z důvodu omezení toohello názvů systému souborů NTFS, ale objekt blob může být tooa exportovaný soubor s cestu, která není vypadat cesta blobu hello.|  
|`Blob/ClientData`|Řetězec|Volitelné. Obsahuje komentáře od hello zákazníka. Tato hodnota není interpretovat hello služby importu a exportu.|  
|`Blob/Snapshot`|Data a času|Volitelné pro úlohy exportu. Určuje identifikátor snímku hello exportovaný blob snímku.|  
|`Blob/Length`|Integer|Určuje hello celková délka objektu hello blob v bajtech. Hello hodnota může být too200 GB pro objekt blob bloku a až too1 TB pro objekt blob stránky. Pro objekt blob stránky tato hodnota musí být násobkem 512.|  
|`Blob/ImportDisposition`|Řetězec|Volitelné pro úlohy import, export úloh tento parametr vynechán. To určí, jak hello službu Import/Export hello případ úlohy importu kde objekt blob se hello stejný název již existuje. Pokud tato hodnota je vynechán z manifestu import hello, hello výchozí hodnota je `rename`.<br /><br /> Hello hodnoty pro tento element:<br /><br /> -   `no-overwrite`: Pokud je cílový objekt blob se již nachází s hello stejný název, operace importu hello přeskočí import tohoto souboru.<br />-   `overwrite`: Žádné existující cílový objekt blob je zcela přepsány hello nově importovaného souboru.<br />-   `rename`: hello nový objekt blob se nahrál upravený název.<br /><br /> Přejmenování pravidlo Hello vypadá takto:<br /><br /> -Li název objektu blob hello neobsahuje tečku, generuje se nový název připojením `(2)` toohello původní název objektu blob; Pokud je tento nový název taky v konfliktu s existujícím názvem objektu blob, pak `(3)` se připojí místě `(2)`; a tak dále.<br />– Pokud je název objektu blob hello obsahuje tečku, považuje za část hello následující poslední tečkou hello hello název rozšíření. Podobné toohello výše postupu `(2)` je vložen před hello poslední tečkou toogenerate nový název; Pokud je nový název hello stále konfliktu s existujícím názvem objektu blob, potom hello služba se pokusí `(3)`, `(4)`a tak dále, až jiný konfliktní název je nalezena.<br /><br /> Příklady:<br /><br /> Objekt blob Hello `BlobNameWithoutDot` bude přejmenován na:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> Objekt blob Hello `Seattle.jpg` bude přejmenován na:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|Vnořené – element XML|Vyžaduje se pro objekt blob stránky.<br /><br /> Operace, určuje pro importu seznam rozsahů bajtů toobe soubor importu. Každý rozsahu stránek je popsán posun a délka v hello zdrojový soubor, který popisuje hello rozsahu stránek, společně s hodnotu hash MD5 hello oblasti. Hello `Hash` atribut rozsahu stránek je povinný. Služba Hello ověří, že hodnota hash hello hello dat v objektu blob hello odpovídající hodnotě hash MD5 hello počítaný z rozsahu stránek hello. Libovolný počet rozsahů stránek může být použité toodescribe soubor pro import, s hello celková velikost až too1 TB. Jsou povoleny žádné překrytí a všechny rozsahy stránky musí být seřazené podle posun.<br /><br /> Určuje, že sada rozsahů bajtů objektu blob, které byly exportovány toohello jednotky pro operace exportu.<br /><br /> rozsahů stránek Hello společně se může vztahovat pouze dílčí rozsahy objektů blob nebo souboru.  je očekávána Hello zbývající část hello souboru není uvedena v jakémkoli rozsahu stránky a její obsah může být definovaný.|  
|`PageRange`|XML element|Představuje rozsahu stránek.|  
|`PageRange/@Offset`|Atribut, celé číslo|Určuje, že začne hello posun v souboru přenos hello a objektů blob hello tam, kde hello zadán rozsahu stránek. Tato hodnota musí být násobkem 512.|  
|`PageRange/@Length`|Atribut, celé číslo|Určuje délku hello rozsahu stránku hello. Tato hodnota musí být násobkem 512 a více než 4 MB.|  
|`PageRange/@Hash`|Atribut, řetězec|Určuje hodnotu hash MD5 kódováním Base16 hello hello rozsahu stránek.|  
|`BlockList`|Vnořené – element XML|Vyžaduje se pro objekt blob bloku pomocí pojmenovaných bloků.<br /><br /> Pro operace importu hello blokovaných určuje sadu bloků, které bude naimportován do úložiště Azure. Pro operace exportu hello blokovaných Určuje, kde každý blok byla uložena v souboru hello na disku export hello. Každý blok je popsán posun v souboru hello a délka bloku; Každý blok je kromě s názvem atributem ID bloku a obsahuje hodnotu hash MD5 pro hello blok. Až too50 může být 000 bloků použité toodescribe objekt blob.  Všechny bloky musejí být seřazeny podle posun a společně by mělo zahrnovat hello dokončení rozsah souboru hello *tj*, nesmí být mezera mezi bloky. Pokud hello blob je delší než 64 MB, hello ID bloku pro každého bloku musí být buď všechny chybí, nebo všechny k dispozici. ID bloku jsou řetězce, požadované toobe kódováním Base64. V tématu [Put bloku](/rest/api/storageservices/put-block) další požadavky pro ID bloku.|  
|`Block`|XML element|Představuje blok.|  
|`Block/@Offset`|Atribut, celé číslo|Určuje posun hello kde začíná hello zadaný blok.|  
|`Block/@Length`|Atribut, celé číslo|Určuje hello počet bajtů v bloku hello; Tato hodnota musí být delší než 4 MB volného místa.|  
|`Block/@Id`|Atribut, řetězec|Určuje řetězec představující hello ID bloku pro hello blok.|  
|`Block/@Hash`|Atribut, řetězec|Určuje algoritmus hash MD5 kódováním Base16 hello hello bloku.|  
|`Blob/MetadataPath`|Řetězec|Volitelné. Určuje hello relativní cestu k souboru metadat. Během importu hello metadata nastavena na hello cílový objekt blob. Během operace exportu se metadata objektu blob hello uložené v souboru metadat hello na jednotce hello.|  
|`Blob/MetadataPath/@Hash`|Atribut, řetězec|Určuje algoritmus hash MD5 kódováním Base16 hello hello blob metadata souboru.|  
|`Blob/PropertiesPath`|Řetězec|Volitelné. Určuje hello relativní cestu k souboru vlastnosti. Během importu jsou vlastnosti hello nastavit hello cílový objekt blob. Během operace exportu jsou vlastnosti objektů blob hello uložené v souboru vlastnosti hello na jednotce hello.|  
|`Blob/PropertiesPath/@Hash`|Atribut, řetězec|Určuje algoritmus hash MD5 kódováním Base16 hello hello blob vlastnosti souboru.|  
  
## <a name="next-steps"></a>Další kroky
 
* [REST API pro Import/Export úložiště](/rest/api/storageimportexport/)
