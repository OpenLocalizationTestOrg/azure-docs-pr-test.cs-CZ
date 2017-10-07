---
title: "aaaIndexing úložiště objektů Blob Azure s Azure Search"
description: "Zjistěte, jak text úložiště a extrahování Azure Blob tooindex z dokumentů s Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/22/2017
ms.author: eugenesh
ms.openlocfilehash: 1bdd34e66a4a9192ed88cacbc7b8456d0dcdfeb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Indexování dokumentů v úložišti objektů Blob v Azure s Azure Search
Tento článek ukazuje, jak toouse Azure Search tooindex dokumenty (jako je PDF, dokumentů Microsoft Office a několik dalších běžných formátů) uložené v úložišti objektů Blob Azure. Nejprve vysvětluje hello základní informace o nastavení a konfiguraci indexer objektů blob. Potom nabízí podrobnější zkoumání chování a jsou pravděpodobně tooencounter scénáře.

## <a name="supported-document-formats"></a>Podporované formáty dokumentu
indexer Hello objektů blob můžete rozbalte text z hello následující formáty dokumentů:

* PDF
* Aplikace Microsoft Office formáty: DOCX/DOC, XLSX nebo XLS, PPTX/PPT, MSG (Outlook e-mailů)  
* HTML
* XML
* ZIP
* EML
* RTF
* Soubory ve formátu prostého textu (viz také [indexování prostý text](#IndexingPlainText))
* JSON (viz [objekty BLOB JSON indexování](search-howto-index-json-blobs.md))
* CSV (najdete v části [objekty BLOB indexování CSV](search-howto-index-csv-blobs.md) funkce ve verzi preview)

> [!IMPORTANT]
> Podpora pro sdílený svazek clusteru a JSON pole je aktuálně ve verzi preview. Tyto formáty jsou k dispozici pouze pomocí verze **2016-09-01-Preview** z hello REST API nebo verze 2.x preview hello .NET SDK. Prosím mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.
>
>

## <a name="setting-up-blob-indexing"></a>Nastavení indexování objektů blob
Můžete nastavit indexer Azure Blob Storage pomocí:

* [Azure Portal](https://ms.portal.azure.com)
* Služba Azure Search [rozhraní REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Služba Azure Search [sady .NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Některé funkce (například mapování polí) ještě nejsou k dispozici hello portálu a mít toobe používá prostřednictvím kódu programu.
>
>

Zde ukážeme hello toku pomocí hello REST API.

### <a name="step-1-create-a-data-source"></a>Krok 1: Vytvořte zdroj dat.
Zdroj dat určuje, které tooindex dat, přihlašovacích údajů nezbytných tooaccess hello data a zásady tooefficiently identifikovat změny v datech hello (nové, modified nebo deleted řádky). Zdroj dat můžete použít několik indexerů v hello stejné služby vyhledávání.

Pro indexování objektů blob, musí mít zdroj dat hello hello následující požadované vlastnosti:

* **název** je jedinečný název hello hello zdroje dat v rámci služby search.
* **typ** musí být `azureblob`.
* **přihlašovací údaje** poskytuje hello úložiště připojovací řetězce k účtu jako hello `credentials.connectionString` parametr. V tématu [jak přihlašovací údaje toospecify](#Credentials) níže podrobnosti.
* **kontejner** Určuje kontejner ve vašem účtu úložiště. Standardně jsou všechny objekty BLOB v kontejneru hello dá načíst. Pokud chcete pouze objekty BLOB tooindex do konkrétního virtuálního adresáře, můžete zadat Tento adresář pomocí hello volitelné **dotazu** parametr.

toocreate zdroj dat:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Další informace o hello vytvořit zdroj dat rozhraní API, najdete v části [vytvořit zdroj dat](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-toospecify-credentials"></a>Jak toospecify pověření ####

Můžete zadat hello přihlašovací údaje pro kontejner objektů blob hello v jednom z těchto způsobů:

- **Úplný přístup připojovací řetězce k účtu úložiště**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. Můžete získat připojovací řetězec hello hello portálu Azure tak, že přejdete v okně účtu úložiště toohello > Nastavení > klíče (pro účty úložiště Classic) nebo nastavení > přístupové klíče (pro účty úložiště Azure Resource Manager).
- **Podpis sdíleného přístupu účtu úložiště** (SAS) připojovací řetězec: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=b&sp=rl` hello SAS by měl mít hello seznamu a oprávnění ke čtení v kontejnery a objekty (v tomto případě objektů BLOB).
-  **Sdílený přístupový podpis kontejneru**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<hello signature>&se=<hello validity end time>&sp=rl` hello SAS by měl mít hello seznamu a oprávnění ke čtení v kontejneru hello.

Další informace o sdílené úložiště přístup podpisy, najdete v části [pomocí sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Pokud chcete použít přihlašovací údaje SAS, budete potřebovat tooupdate přihlašovacích údajů zdroje dat hello pravidelně s prodlouženou platností podpisy tooprevent vypršením jejich platnosti. Pokud přihlašovací údaje SAS vyprší, hello indexer selže s chybovou zprávou podobnou příliš`Credentials provided in hello connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Krok 2: Vytvoření indexu
Hello index určuje hello pole v dokumentu, atributy, a dalších vytvoří tento tvar hello vyhledáváním.

Tady je způsob toocreate index s vyhledávat `content` pole textu hello toostore extrahoval z objektů blob:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Další informace o vytváření indexů najdete v tématu [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>Krok 3: Vytvořením indexeru.
Indexer zdroje dat se připojí pomocí cílový index vyhledávání a poskytuje plánu aktualizace dat hello tooautomate.

Po vytvoření hello index a zdroj dat jste připravené toocreate hello indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Indexer se spustí každé dvě hodiny (interval plánování je nastavena příliš "PT2H"). toorun indexer každých 30 minut nastavit hello interval příliš "PT30M". Hello nejkratší podporovaný interval je 5 minut. Hello plán je volitelné - li tento parametr vynechán, indexer se spustí pouze po jeho vytvoření. Indexer na vyžádání je však můžete spustit kdykoli.   

Další informace o hello vytvoření rozhraní API Indexer, podívejte se na [vytvořit Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Jak Azure Search indexuje objektů BLOB

V závislosti na hello [indexer konfigurace](#PartsOfBlobToIndex), indexer hello objekt blob může indexovat pouze metadata úložiště (užitečné, pokud jste pouze péče o hello metadata a nepotřebujete tooindex hello obsah objektů BLOB), úložiště a obsah metadata nebo obojí metadata a textový obsah. Ve výchozím nastavení extrahuje hello indexer metadata a obsah.

> [!NOTE]
> Ve výchozím nastavení objektů BLOB s strukturovaných obsah, jako jsou JSON nebo CSV jsou uloženy jako jeden blok textu. Pokud chcete objekty BLOB JSON a CSV tooindex strukturované, najdete v části [objekty BLOB JSON indexování](search-howto-index-json-blobs.md) a [objekty BLOB indexování CSV](search-howto-index-csv-blobs.md) funkce verze preview.
>
> Složené nebo vloženého dokumentu (například archivu ZIP nebo dokument aplikace Word s embedded e-mailu aplikace Outlook obsahující přílohy) jsou také indexována jako jednotlivý dokument.

* Hello textový obsah dokumentu hello je extrahován do pole řetězce s názvem `content`.

> [!NOTE]
> Služba Azure Search omezuje množství textu extrahuje v závislosti na cenové úrovni hello: 32 000 znaků zdarma vrstvy, 64 000 pro základní a 4 milionů na úrovních Standard, Standard S2 a úrovně Standard S3. Upozornění je zahrnutý v odpovědi stav hello indexeru pro zkrácený dokumenty.  

* Metadata zadaného uživatelem v objektu blob hello, pokud existuje, extrahují se vlastnosti doslovné.
* Extrahují se vlastnosti metadat standardní objekt blob do hello následující pole:

  * **metadata\_úložiště\_název** (Edm.String) – název souboru hello objektu hello blob. Například pokud máte /my-container/my-folder/subfolder/resume.pdf objektů blob, hello hodnotu tohoto pole je `resume.pdf`.
  * **metadata\_úložiště\_cesta** (Edm.String) - hello úplný identifikátor URI objektu blob hello, včetně hello účet úložiště. Například `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`.
  * **metadata\_úložiště\_obsah\_typu** (Edm.String) – typ obsahu, jako určeného hello kód používá objekt blob tooupload hello. Například, `application/octet-stream`.
  * **metadata\_úložiště\_poslední\_upravil** (Edm.DateTimeOffset) - poslední změny časové razítko pro objekt blob hello. Služba Azure Search používá tento časové razítko tooidentify změnit objekty BLOB, tooavoid novou všechno indexaci po počáteční indexování hello.
  * **metadata\_úložiště\_velikost** (Edm.Int64) – objekt blob velikost v bajtech.
  * **metadata\_úložiště\_obsah\_md5** (Edm.String) - hodnota hash MD5 obsahu objektu blob hello, pokud je k dispozici.
* Metadata vlastnosti konkrétní tooeach dokument formátu extrahují do uvedených polí hello [zde](#ContentSpecificMetadata).

Nepotřebujete toodefine pole pro všechny hello výše vlastnosti v indexu vyhledávání – stačí zaznamenat hello vlastnosti, které potřebujete pro vaši aplikaci.

> [!NOTE]
> Názvy polí hello v existující index často, bude jiné než hello názvy polí vygenerovaných během extrakce dokumentu. Můžete použít **pole mapování** názvy vlastností hello toomap poskytované Azure Search toohello názvy polí v indexu vyhledávání. Zobrazí se pole, které mapování použít následující příklad.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Definování dokumentu klíče a mapování polí
Ve službě Azure Search je klíč dokumentu hello jednoznačně identifikuje dokumentu. Každý index hledání musí mít přesně jeden klíčové pole typu Edm.String. Hello klíčové pole je povinné pro každý dokument, který je přidáván toohello indexu (je ve skutečnosti hello pouze požadované pole).  

Měli pečlivě zvažte, které extrahované pole by měla být mapována toohello klíčové pole pro indexu. Hello kandidáti:

* **metadata\_úložiště\_název** – to může být vhodné kandidátem, ale názvy hello 1) nemusí být jedinečný, jako je možné, že objekty BLOB s hello stejný název v jiné složky, a 2) hello název může obsahovat znaky, jsou neplatné v dokumentu klíče, jako je například pomlčky. Neplatné znaky můžete řešit pomocí hello `base64Encode` [pole mapování funkce](search-indexer-field-mappings.md#base64EncodeFunction) – Pokud je to nezapomeňte tooencode dokumentu klíče, když předávání je v rozhraní API volá například vyhledávání. (Například v rozhraní .NET můžete hello [UrlTokenEncode metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) k tomuto účelu).
* **metadata\_úložiště\_cesta** – použití úplná cesta hello zajišťuje jedinečnost, ale cesta hello výborný obsahuje `/` znaky, které jsou [neplatný klíč dokumentu](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Jak je uvedeno výše, máte možnost hello kódování hello klíče pomocí hello `base64Encode` [funkce](search-indexer-field-mappings.md#base64EncodeFunction).
* Pokud žádná z výše uvedených možností hello fungovat pro vás, můžete přidat objekty BLOB vlastních metadat vlastnost toohello. Tuto možnost, však nevyžaduje vaše tooadd procesu nahrávání objektů blob této metadata vlastnosti tooall BLOB. Vzhledem k tomu, že hello klíč se vyžaduje vlastnost, všech objektů BLOB, které nemají tuto vlastnost toobe indexované selže.

> [!IMPORTANT]
> Pokud neexistuje žádné explicitní mapování pro hello klíčové pole v indexu hello, Azure Search automaticky použije `metadata_storage_path` jako hello klíče a kódování base-64 kóduje hodnoty klíče (hello druhá možnost výše).
>
>

V tomto příkladu vytvoříme hello `metadata_storage_name` pole jako klíč dokumentu hello. Také Předpokládejme indexu má klíčové pole s názvem `key` a pole `fileSize` pro ukládání velikost dokumentu hello. toowire věcí si podle potřeby, zadejte následující mapování polí při vytváření nebo aktualizaci indexer hello:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

toobring všechny společně, tady na tom, jak můžete přidat mapování polí a povolit kódování base-64 klíčů pro existujícího indexeru:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> toolearn Další informace o mapování polí, najdete v části [v tomto článku](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Řízení, které objekty BLOB jsou uloženy.
Můžete řídit, které objekty BLOB jsou indexované a který se přeskočí.

### <a name="index-only-hello-blobs-with-specific-file-extensions"></a>Index pouze hello objektů BLOB s konkrétní přípony souborů
Může indexovat pouze objekty BLOB hello s příponami názvů souborů hello zadáte pomocí hello `indexedFileNameExtensions` indexer konfigurační parametr. Hello hodnota je řetězec obsahující oddělený čárkami seznam přípon souborů (s úvodní tečky). Například tooindex pouze hello. PDF a. Objekty BLOB DOCX, postupujte takto:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Vyloučení objektů BLOB s konkrétní přípony souborů
Objekty BLOB s konkrétní přípony názvů souborů můžete vyloučit z indexování pomocí hello `excludedFileNameExtensions` konfigurační parametr. Hello hodnota je řetězec obsahující oddělený čárkami seznam přípon souborů (s úvodní tečky). Tooindex všechny objekty BLOB s výjimkou těch, které s hello. PNG a. Rozšíření JPEG, postupujte takto:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Pokud oba `indexedFileNameExtensions` a `excludedFileNameExtensions` dostupné parametry, Azure Search nejdřív zjistí `indexedFileNameExtensions`, pak na `excludedFileNameExtensions`. To znamená, že pokud hello stejnou příponu souboru nachází v obou seznamů, bude vyloučena z indexu.

### <a name="dealing-with-unsupported-content-types"></a>Práci s nepodporované typy obsahu

Ve výchozím nastavení zastaví hello blob indexer při výskytu objekt blob se nepodporovaný typ obsahu (například obrázek). Samozřejmě můžete hello `excludedFileNameExtensions` parametr tooskip určité typy obsahu. Ale musíte objekty BLOB tooindex bez znalosti všechny hello možné obsahu typy předem. toocontinue indexování při zjistil se nepodporovaný typ obsahu, nastavte hello `failOnUnsupportedContentType` konfigurační parametr příliš`false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

### <a name="ignoring-parsing-errors"></a>Ignorování chyb analýzy

Azure vyhledávání dokumentů extrakce logiku není úplně bez chyby a někdy selže tooparse dokumenty podporovaného typu obsahu, jako například. DOCX nebo. PDF. Pokud nechcete, aby toointerrupt hello indexování v takových případech, nastavte hello `maxFailedItems` a `maxFailedItemsPerBatch` přiměřené hodnoty toosome parametry konfigurace. Například:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-hello-blob-are-indexed"></a>Řízení, které části hello blob jsou uloženy.

Můžete řídit, které části hello objekty BLOB jsou indexované pomocí hello `dataToExtract` konfigurační parametr. Může trvat hello následující hodnoty:

* `storageMetadata`-Určuje, že pouze hello [vlastnosti standardní objektů blob a zadat uživatele metadata](../storage/blobs/storage-properties-metadata.md) jsou indexovány.
* `allMetadata`-Určuje, že metadata úložiště a hello [konkrétních metadat typu obsahu](#ContentSpecificMetadata) extrahovat z objektu blob hello jsou indexované obsah.
* `contentAndMetadata`-Určuje, že jsou indexován všechna metadata a textový obsah extrahovat z objektu blob hello. Toto je výchozí hodnota hello.

Například tooindex pouze metadata úložiště hello, použijte:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-toocontrol-how-blobs-are-indexed"></a>Pomocí toocontrol metadata objektu blob, jak jsou objekty BLOB indexované

výše uvedených parametrů konfigurace Hello použít tooall objekty BLOB. V některých případech může být vhodné toocontrol jak *jednotlivé objekty BLOB* jsou indexovány. To provedete tak, že přidáte následující hello blob metadata vlastnosti a hodnoty:

| Název vlastnosti | Hodnota vlastnosti | Vysvětlení |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Dá pokyn hello blob indexer toocompletely přeskočit hello blob. Extrakce metadat ani obsahu dojde k pokusu o. To je užitečné, když konkrétní objekt blob opakovaně a přerušení hello indexování procesu. |
| AzureSearch_SkipContent |"true" |Toto je ekvivalentní `"dataToExtract" : "allMetadata"` nastavení popsané [výše](#PartsOfBlobToIndex) oboru tooa konkrétní objekt blob. |

## <a name="incremental-indexing-and-deletion-detection"></a>Přírůstkové indexování a odstranění duplicit
Při nastavování toorun indexeru objektů blob podle plánu, ho znovu indexovat pouze hello změnit objekty BLOB, počítáno od hello blob `LastModified` časové razítko.

> [!NOTE]
> Nemáte toospecify zásady detekce změn – přírůstkové indexování se povolí automaticky.

odstraňování dokumentů toosupport, použít s "obnovitelného odstranění". Při odstranění objektů BLOB hello přímý odpovídající dokumenty nebudou odebrané z indexu vyhledávání hello. Místo toho použijte hello následující kroky:  

1. Přidání vlastních metadat vlastnost toohello objektu blob tooindicate tooAzure vyhledávání, které je logicky odstraněna
2. Nakonfigurovat zásadu obnovitelného odstranění duplicit na zdroj dat hello
3. Po zpracování hello blob (jak je znázorněno podle stavu indexer hello rozhraní API) hello indexer fyzicky odstraníte hello objektů blob

Například následující zásady hello zvažuje blob toobe, odstranit, pokud má vlastnost metadat `IsDeleted` s hodnotou hello `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Indexování rozsáhlých datových sad

Indexování objekty BLOB může být zdlouhavý proces. V případech, kdy máte miliony tooindex objekty BLOB můžete urychlit indexování dělení dat a použitím více dat hello tooprocess indexery paralelně. Zde je, jak můžete nastavit tuto možnost:

- Oddílu data do více kontejnery objektů blob nebo virtuální složek
- Nastavte několik zdrojů dat Azure Search, jeden pro každý kontejner nebo složky. toopoint tooa blob složky, použijte hello `query` parametr:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Vytvořte odpovídající indexer pro každý datový zdroj. Všechny hello indexery může bod toohello stejný cílový index vyhledávání.  

- Jedna jednotka vyhledávání ve službě můžete spustit jeden indexer v daném okamžiku. Vytvoření více indexery, jak je popsáno výše je pouze užitečné, pokud je ve skutečnosti spuštěna paralelně. toorun několik indexerů paralelně, škálování služby vyhledávání vytvořením odpovídající počet oddílů a repliky. Například pokud vaši službu vyhledávání má 6 jednotek služby vyhledávání (například 2 oddíly x 3 repliky), pak 6 indexery můžete spustit současně, což vede k six-fold zvýšení hello propustnost indexování. toolearn Další informace o škálování a plánování kapacity najdete v části [škálovat prostředek úrovně pro dotaz a indexování úlohy ve službě Azure Search](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Indexování dokumentů společně s související data

Může být vhodné příliš "sestavte" dokumenty z více zdrojů v indexu. Například můžete toomerge text z objektů BLOB s další metadata uloženy v databázi Cosmos. Můžete vytvořit i hello nabízené indexování API společně s různými indexery příliš vybudovat vyhledávání dokumentů z více částí. 

Pro tento toowork všechny indexery a další součásti nutné tooagree na klíč dokumentu hello. Podrobný návod, najdete v části v tomto článku externí: [dokumenty kombinovat s jinými daty ve službě Azure Search ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Indexování prostý text 

Pokud všech objektů BLOB obsahovat prostý text hello, stejné kódování, může výrazně zlepšit výkon indexování pomocí **text analýza režimu**. text toouse analýza režimu, sada hello `parsingMode` vlastnosti konfigurace příliš`text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Ve výchozím nastavení, hello `UTF-8` kódování se předpokládá. toospecify jiné kódování, použijte hello `encoding` vlastnosti konfigurace: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Vlastnosti metadata specifická pro typ obsahu
Hello následující tabulka shrnuje zpracování provedeno pro každý dokument formátu a popisuje vlastnosti metadat hello extrahovat ve službě Azure Search.

| Dokument formátu / typ obsahu | Vlastnosti metadat konkrétní typ obsahu | Podrobnosti o zpracování |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Odstranit kód HTML a rozbalte text |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Rozbalte text, včetně vložených dokumentů (s výjimkou bitové kopie) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Rozbalte text, včetně embedded dokumenty |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Rozbalte text, včetně embedded dokumenty |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Rozbalte text, včetně embedded dokumenty |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Rozbalte text, včetně embedded dokumenty |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Rozbalte text, včetně embedded dokumenty |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Rozbalte text, včetně embedded dokumenty |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Rozbalte text, včetně příloh |
| ZIP (aplikace nebo zip) |`metadata_content_type` |Rozbalte text z všechny dokumenty v archivu hello |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |Odstranit kód XML a rozbalte text |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Rozbalte text<br/>Poznámka: Pokud potřebujete tooextract více dokumentů pole z objektu blob JSON, přečtěte si [objekty BLOB JSON indexování](search-howto-index-json-blobs.md) podrobnosti |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Rozbalte text, včetně příloh |
| RTF (aplikace/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Rozbalte text|
| Prostý text (text/plain) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Rozbalte text|


## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, dejte nám vědět na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
