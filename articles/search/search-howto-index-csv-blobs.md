---
title: "aaaIndexing CSV objektů BLOB s indexer objektu blob Azure Search | Microsoft Docs"
description: "Zjistěte, jak tooindex CSV objektů BLOB s Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: f2b1ce824e62c5b3f6155c6e88887897cf1a8eae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexování CSV objekty BLOB se indexer objektu blob Azure Search
Ve výchozím nastavení [indexer objektu blob Azure Search](search-howto-indexing-azure-blob-storage.md) analyzuje oddělený text objekty BLOB jako jeden blok textu. Nicméně s objekty BLOB obsahující data ve formátu CSV, často chcete tootreat každý řádek v objektu blob hello jako samostatný dokument. Například daný hello oddělený následující text: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

můžete chtít tooparse ho do 2 dokumentů, každý obsahující pole "značky", "datePublished" a "id".

V tomto článku se dozvíte, jak tooparse CSV objektů BLOB s indexer Azure Search objektů blob. 

> [!IMPORTANT]
> Tato funkce je aktuálně ve verzi preview. Je k dispozici pouze v hello REST API pomocí verze **2015-02-28-Preview**. Prosím mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí. 
> 
> 

## <a name="setting-up-csv-indexing"></a>Nastavení indexování sdíleného svazku clusteru
objekty BLOB tooindex sdíleného svazku clusteru, umožňuje vytvořit nebo aktualizovat definici indexer s hello `delimitedText` analýza režimu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Další informace o hello vytvoření rozhraní API Indexer, podívejte se na [vytvořit Indexer](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Určuje, že hello prvního řádku (prázdný) daného každý objekt blob obsahuje záhlaví.
Pokud objekty BLOB neobsahují řádku s počáteční záhlaví, musí být zadán hello hlavičky v hello indexer konfigurace: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

V současné době je podporováno pouze kódování UTF-8 hello. Navíc jenom hello čárkami `','` znaku je podporováno, protože hello oddělovač. Pokud potřebujete podporu pro další kódování nebo oddělovače, dejte nám vědět, na [našeho webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Při použití hello oddělený text analýza režimu Azure Search předpokládá, že všechny objekty BLOB ve zdroji dat bude sdílený svazek clusteru. Pokud potřebujete toosupport kombinaci sdíleného svazku clusteru a jiné než CSV objektů BLOB v hello stejný zdroj dat, prosím dejte nám vědět, na [našeho webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Příklady požadavku
Vložení to všechny společně, zde jsou příklady hello úplnou datovou část. 

Zdroj dat: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, prosím oslovení toous na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).

