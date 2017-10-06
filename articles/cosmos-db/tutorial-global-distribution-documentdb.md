---
title: "kurz globální distribuční aaaAzure Cosmos DB pro rozhraní API DocumentDB | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello DocumentDB rozhraní API."
services: cosmos-db
keywords: "globální distribuční, documentdb"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a>Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API DocumentDB hello

V tomto článku ukážeme, jak toouse hello Azure portálu toosetup globální distribuční databázi Cosmos Azure a potom se připojte pomocí hello DocumentDB rozhraní API.

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello [DocumentDB rozhraní API](documentdb-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a>Připojení tooa upřednostňovaná oblast pomocí hello DocumentDB rozhraní API

V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace. Tento krok můžete provést nastavením zásad pro připojení hello. V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a seznamu předvoleb hello zadat, hello většina optimální koncového bodu, bude použita volba hello DocumentDB SDK tooperform zápisu a operace čtení.

Tento seznam předvoleb je zadána při inicializaci připojení pomocí hello DocumentDB SDK. Hello sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.

Hello SDK automaticky odesílat všechny zápisy toohello aktuální zápisu oblasti.

Všechny operace čtení odešle první dostupné oblasti toohello v seznamu PreferredLocations hello. Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále.

Hello sady SDK se pouze pokusí tooread z oblastí hello zadaný v PreferredLocations. Ano například pokud hello databázového účtu je k dispozici v tři oblasti, ale hello klienta pouze určuje pro PreferredLocations dvou oblastí bez zápisu hello, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.

aplikace Hello můžete ověřte hello aktuální koncový bod zápisu a čtení koncový bod zvolí hello SDK pomocí kontrola dvě vlastnosti WriteEndpoint a ReadEndpoint, k dispozici ve verzi sady SDK 1.8 a vyšší.

Pokud není nastavena vlastnost PreferredLocations hello, bude z aktuální oblasti zápisu hello zpracovat všechny požadavky.

## <a name="net-sdk"></a>.NET SDK
Hello SDK můžete použít beze změn kódu. V takovém případě hello SDK automaticky přesměruje obě operace čtení a zapíše toohello aktuální zápisu oblasti.

Ve verzi 1,8 a novější. hello .NET SDK hello ConnectionPolicy parametr pro konstruktor DocumentClient hello má vlastnost s názvem Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Tato vlastnost je typu kolekce `<string>` a musí obsahovat seznam názvů oblast. Hello řetězcové hodnoty jsou formátovány každý sloupec název oblasti hello na hello [oblasti Azure] [ regions] stránky, bez mezer před nebo po hello první a poslední znak v uvedeném pořadí.

Hello aktuální zápisu a čtení koncové body k dispozici v DocumentClient.WriteEndpoint a DocumentClient.ReadEndpoint v uvedeném pořadí.

> [!NOTE]
> Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty. Služba Hello může aktualizovat tyto v libovolném bodě. Tato změna zpracovává Hello SDK automaticky.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript a Python SDK
Hello SDK můžete použít beze změn kódu. V takovém případě hello SDK bude automaticky nasměrovat jak čte a zapisuje toohello aktuální zápisu oblasti.

V verze 1,8 a později každý SDK hello ConnectionPolicy parametr pro konstruktor DocumentClient hello novou vlastnost s názvem DocumentClient.ConnectionPolicy.PreferredLocations. To je parametr pole řetězců, která přebírá seznam názvů oblast. názvy Hello jsou formátovány za hello oblast název sloupce v hello [oblasti Azure] [ regions] stránky. Můžete použít také hello předdefinované konstanty v objektu pohodlí hello AzureDocuments.Regions

Hello aktuální zápisu a čtení koncové body k dispozici v DocumentClient.getWriteEndpoint a DocumentClient.getReadEndpoint v uvedeném pořadí.

> [!NOTE]
> Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty. Služba Hello může aktualizovat tyto v libovolném bodě. Tato změna bude zpracována Hello SDK automaticky.
>
>

Níže je příklad kódu pro NodeJS/Javascript. Python a Java bude postupovat podle hello stejného vzoru.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Jakmile se databázový účet má k dispozici v několika oblastech, klienti mohou odesílat dotazy jeho dostupnost provedením požadavek GET na hello následující identifikátor URI.

    https://{databaseaccount}.documents.azure.com/

Hello služby, vrátí se seznam oblastí a jejich odpovídající endpoint Azure Cosmos DB identifikátory URI pro repliky hello. aktuální oblast zápisu Hello budou uvedené v odpovědi hello. Hello klienta můžete pak vybrat hello příslušný koncový bod pro všechny další požadavky REST API následujícím způsobem.

Příklad odpovědi

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Požadavky PUT, POST a DELETE musí přejít toohello uvedené zápisu identifikátoru URI
* Získá všechny a ostatních jen pro čtení požadavků (například dotazy) může se stát, koncový bod tooany výběru hello klienta

Zapisovat pouze tooread oblasti požadavky selže s kódem chyby protokolu HTTP 403 "(zakázáno).

Pokud oblast zápisu hello změní po zapíše hello klienta počáteční zjišťování fázi následné toohello předchozí zápisu oblast selže s kódem chyby protokolu HTTP 403 "(zakázáno). Hello Klient by pak získat hello seznam oblastí znovu tooget hello aktualizované zápisu oblast.

Je to, že dokončení tohoto kurzu. Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API

Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru hello](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

