---
title: "kurz globální distribuční databáze Cosmos aaaAzure pro rozhraní Graph API | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello rozhraní Graph API."
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
ms.openlocfilehash: 1629a31e12a18079f63e07c4909862b36b5f4c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a>Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní Graph API hello

V tomto článku ukážeme, jak toouse hello globální distribuční databázi Cosmos Azure Azure portálu toosetup a potom se připojte pomocí hello rozhraní Graph API (preview).

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello [rozhraní Graph API](graph-introduction.md) (preview)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a>Připojení tooa upřednostňovaná oblast pomocí hello rozhraní Graph API pomocí hello .NET SDK

Hello rozhraní Graph API je k dispozici jako rozšíření knihovny nad hello DocumentDB SDK.

V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace. Tento krok můžete provést nastavením zásad pro připojení hello. V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a seznamu předvoleb hello zadat, hello většina optimální koncový bod se zvolí hello SDK tooperform zápisu a operace čtení.

Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK hello. Hello sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.

* **Zapíše**: hello SDK automaticky odesílat všechny zapíše toohello aktuální zápisu oblasti.
* **Přečte**: všechny operace čtení zašle toohello první dostupné oblasti v seznamu PreferredLocations hello. Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále. Hello sady SDK se pouze pokusí tooread z oblastí hello zadaný v PreferredLocations. Ano například pokud hello Cosmos DB účtu je k dispozici v tři oblasti, ale hello klienta pouze určuje pro PreferredLocations dvou oblastí bez zápisu hello, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.

aplikace Hello můžete ověřte hello aktuální koncový bod zápisu a čtení koncový bod zvolí hello SDK pomocí kontrola dvě vlastnosti WriteEndpoint a ReadEndpoint, k dispozici ve verzi sady SDK 1.8 a vyšší. Pokud není nastavena vlastnost PreferredLocations hello, bude z aktuální oblasti zápisu hello zpracovat všechny požadavky.

### <a name="using-hello-sdk"></a>Pomocí hello SDK

Například v hello .NET SDK, hello `ConnectionPolicy` parametr pro hello `DocumentClient` konstruktor má vlastnost s názvem `PreferredLocations`. Tuto vlastnost lze nastavit tooa seznam názvů oblast. Hello zobrazované názvy pro [oblasti Azure] [ regions] lze zadat jako součást `PreferredLocations`.

> [!NOTE]
> Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty. Služba Hello může aktualizovat tyto v libovolném bodě. Tato změna zpracovává Hello SDK automaticky.
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

// connect tooAzure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

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

