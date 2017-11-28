---
title: objekty BLOB JSON aaaIndexing s indexer objektu blob Azure Search
description: "Indexování objekty BLOB JSON s indexer objektu blob Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: 269968714358cd40ea66863b4dbb97766e1d77e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexování objekty BLOB JSON s indexer objektu blob Azure Search
Tento článek ukazuje, jak tooextract indexer objektu blob Azure Search tooconfigure strukturovaná obsah z objektů BLOB, které obsahují JSON.

## <a name="scenarios"></a>Scénáře
Ve výchozím nastavení [indexer objektu blob Azure Search](search-howto-indexing-azure-blob-storage.md) analyzuje objekty BLOB JSON jako jeden blok textu. Často budete chtít toopreserve hello struktura dokumentů JSON. Například zadaný dokument JSON hello

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

můžete chtít tooparse do Azure Search dokumentů s "text", "datePublished" a "značky" pole.

Případně, pokud obsahovat objektů blob **pole objektů JSON**, můžete každý prvek hello pole toobecome samostatný dokument Azure Search. Například zadaný objekt blob se tento text JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

jej můžete naplnit index Azure Search s tři samostatné dokumenty, každý s pole "id" a "text".

> [!IMPORTANT]
> pole JSON Hello analýza funkce je aktuálně ve verzi preview. Je k dispozici pouze v hello REST API pomocí verze **2015-02-28-Preview**. Mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.
>
>

## <a name="setting-up-json-indexing"></a>Nastavení indexování JSON
Indexování objekty BLOB JSON je podobné extrakce toohello běžný dokument. Nejprve vytvořte zdroj dat hello přesně stejně jako za normálních okolností: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Poté vytvořte index vyhledávání cíl hello, pokud jste ještě nemáte. 

Nakonec vytvořte indexer a nastavte hello `parsingMode` parametr příliš`json` (tooindex každý objekt blob jako jedním dokumentem) nebo `jsonArray` (Pokud objektů BLOB obsahovat pole JSON a je třeba každý prvek pole toobe považovat za samostatný dokument):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

V případě potřeby použijte **pole mapování** toopick hello vlastnosti hello zdroj JSON dokumentu používá toopopulate cíl indexu vyhledávání, jak je znázorněno v další části hello.

> [!IMPORTANT]
> Při použití `json` nebo `jsonArray` analýzy režimu Azure Search předpokládá, že všechny objekty BLOB ve zdroji dat obsahovat JSON. Pokud potřebujete toosupport směs JSON a jiný JSON objektů BLOB v hello stejný zdroj dat, dejte nám vědět, na [našeho webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-toobuild-search-documents"></a>Pomocí pole mapování toobuild vyhledávání dokumentů
V současné době Azure Search nemůže indexovat libovolné dokumenty JSON přímo, protože podporuje jenom primitivní datové typy, řetězce pole a GeoJSON body. Můžete však použít **pole mapování** toopick částí vaše struktury JSON dokumentu a "navýšení" je do nejvyšší úrovně pole hello vyhledávání dokumentů. toolearn o základech mapování pole, najdete v části [mapování polí indexer Azure Search přemostění hello rozdíly mezi zdroje dat a indexy vyhledávání](search-indexer-field-mappings.md).

Vracející se zpět dokumentu JSON tooour příklad:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Řekněme, že máte index vyhledávání s hello následující pole: `text` typu `Edm.String`, `date` typu `Edm.DateTimeOffset`, a `tags` typu `Collection(Edm.String)`. toomap vaše struktury JSON do hello požadovaného tvaru, použijte následující mapování polí hello:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Hello zdroj názvy polí v mapování hello jsou určeny pomocí hello [JSON ukazatel](http://tools.ietf.org/html/rfc6901) zápis. Začínat lomítkem toorefer toohello kořenové dokumentu JSON a potom vyberte vlastnost hello požadovaného (v libovolné úroveň vnoření) pomocí dopředného cesty oddělené lomítko.

Můžete se také podívat elementy pole tooindividual pomocí index počítaný od nuly. Například toopick hello první prvek pole "značky" hello z hello výše například použít mapování polí takto:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Pokud název pole zdroje v cestě mapování pole odkazuje vlastnost tooa, který neexistuje ve formátu JSON, že mapování přeskočen bez chyby. To se provádí tak, aby podporujeme dokumentů s zvolte jiné schéma (což je běžný případ použití). Protože není k dispozici žádné ověření, musíte tootake pozor tooavoid překlepům specifikací mapování pole.
>
>

Pokud vaše dokumenty JSON obsahovat pouze jednoduché vlastnosti nejvyšší úrovně, možná nebudete potřebovat mapování polí vůbec. Například pokud vaše struktury JSON vypadá se hello nejvyšší úrovně vlastnosti "text", "datePublished" a "značky" přímo mapuje toohello odpovídající pole v indexu vyhledávání hello:

    {
       "text" : "A hopefully useful article explaining how tooparse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Tady je kompletní indexer datové části s mapování polí:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Indexování vnořená pole JSON
Co dělat, když chcete tooindex pole objektů JSON, ale tohoto pole je vnořený někde v rámci hello dokumentu? Můžete si vybrat vlastností, které obsahuje pole hello pomocí hello `documentRoot` vlastnosti konfigurace. Pokud například objektů BLOB vypadat takto:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use hello documentRoot property" },
                { "id" : "2", "text" : "toopluck hello array you want tooindex" },
                { "id" : "3", "text" : "even if it's nested inside hello document" }  
            ]
        }
    }

Pomocí této konfigurace tooindex hello pole obsažené ve hello `level2` vlastnost:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, oslovení toous na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
