---
title: "Globální distribuční kurz pro Azure Cosmos DB pro rozhraní Graph API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API."
services: cosmos-db
keywords: "globální distribuční, grafu, gremlin"
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 3c8794fe33c2ff5aa79559ea2c323cf8d92b426a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API

V tomto článku jsme ukazují, jak pomocí portálu Azure do instalačního programu globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní Graph API (preview).

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí [rozhraní Graph API](graph-introduction.md) (preview)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Připojování k upřednostňovaná oblast pomocí rozhraní Graph API pomocí sady .NET SDK

Rozhraní Graph API je k dispozici jako rozšíření knihovny nad DocumentDB SDK.

Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu. Tento krok můžete provést nastavením zásad pro připojení. Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod SDK k provedení operace zápisu a operace čtení.

Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK. Sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.

* **Zapíše**: Sada SDK bude automaticky posílat zápis všech zápisů na aktuální oblasti.
* **Přečte**: všechny operace čtení odešle první oblasti k dispozici v seznamu PreferredLocations. Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále. Sady SDK se pouze pokusí číst z oblastí, zadaný v PreferredLocations. Ano například pokud Cosmos DB účet je k dispozici v tři oblasti, ale klient pouze určuje pro PreferredLocations dvou oblastí bez zápisu, pak žádné čtení se zpracuje mimo oblast zápisu, i v případě převzetí služeb při selhání.

Aplikace můžete ověřit aktuální koncový bod zápisu a čtení koncový bod vybrali SDK kontrolou dvě vlastnosti WriteEndpoint a ReadEndpoint dostupné ve verzi sady SDK 1.8 a výše. Pokud není nastavena vlastnost PreferredLocations, bude z oblasti aktuální zápisu zpracovat všechny požadavky.

### <a name="using-the-sdk"></a>Pomocí sady SDK

Například v sadě SDK .NET `ConnectionPolicy` parametr pro `DocumentClient` konstruktor má vlastnost s názvem `PreferredLocations`. Tuto vlastnost lze nastavit na seznam názvů oblast. Zobrazení názvů pro [oblasti Azure] [ regions] lze zadat jako součást `PreferredLocations`.

> [!NOTE]
> Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty. Služba může aktualizovat tyto v libovolném bodě. Sada SDK zpracovává tuto změnu automaticky.
>
>

```cs
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

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

Je to, že dokončení tohoto kurzu. Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB

Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

