---
title: "aaaAzure Cosmos DB .NET Core API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello .NET Core API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a>Azure Cosmos DB .NET Core SDK: Poznámky k verzi a prostředky
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [Informační kanál změnu rozhraní .NET](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [Poskytovatel prostředků REST](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**Stažení sady SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**Dokumentaci k rozhraní API**</td><td>[Referenční dokumentace rozhraní API .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Ukázky**</td><td>[Ukázky kódu rozhraní .NET](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Začínáme**</td><td>[Začínáme s Azure Cosmos DB .NET Core SDK hello](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td>**Kurz vývoje webové aplikace**</td><td>[Vývoj webových aplikací s Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuální podporovaných prostředí**</td><td>[Rozhraní .NET 1.6 standardní a .NET standardní 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

Hello Azure Cosmos DB .NET Core SDK má parity funkcí s nejnovější verzí hello hello [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).

> [!NOTE] 
> Hello Azure Cosmos DB .NET Core SDK ještě není kompatibilní s aplikací pro univerzální platformu Windows (UWP). Pokud vás zajímá hello .NET Core SDK, který podporuje aplikace UWP odeslání e-mailu příliš[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Přidaná podpora pro PartitionKeyRangeId jako FeedOption pro vymezení hodnotu klíče rozsahu konkrétního oddílu tooa výsledky dotazu. 
* Přidaná podpora pro StartTime jako ChangeFeedOption toostart, hledá změny hello po uplynutí této doby. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Byl opraven problém v hello JsonSerializable třídu, která může způsobit výjimce přetečení zásobníku.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Přidaná podpora pro zadání vlastní JsonSerializerSettings při vytváření instancí [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Podpora rozhraní .NET standardní 1.5 jako jeden z hello cílové rozhraní.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Byl opraven problém ovlivňující x64 počítače, které nemají podporu SSE4 instrukce a throw SEHException – při spuštění dotazů Azure Cosmos DB.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.
*   Přidaná podpora pro dotaz metriky pro jednotlivé oddíly.
*   Přidaná podpora pro omezení velikosti hello hello token pokračování pro dotazy.
*   Přidaná podpora pro podrobnější trasování pro chybné žádosti.
*   Provádí některé vylepšení výkonu v hello SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Opravit problém, který ignoruje hello PartitionKey hodnota zadaná v FeedOptions pro agregační dotazy.
* Byl opraven problém v transparentní zpracování oddílu správy během letu střední cross-partition Order By dotazu provádění.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Byl opraven problém, který chybu způsobil zablokování v některých hello asynchronní rozhraní API, pokud se používá v kontextu ASP.NET.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Opravy toomake SDK více odolné tooautomatic převzetí služeb při selhání za určitých podmínek.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Opravte pro problém způsobující příležitostně o výjimku WebException: hello vzdálený název nelze rozpoznat.
* Přidání hello podporu pro přímo čtení typu dokumentu přidáním nové rozhraní API tooReadDocumentAsync přetížení.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Byla přidána podpora LINQ pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).
* Opravte pro problém nevracení paměti pro objekt ConnectionPolicy hello způsobené hello použití obslužné rutiny události.
* Oprava problému, ve kterém nebyl UpsertAttachmentAsync pracovat, když byla použita značka ETag.
* Oprava problému, ve kterém nebyl křížové oddílu klauzule order by dotazu pokračování práce při řazení na pole řetězce.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr). V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).
* Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Hello Azure Cosmos DB .NET Core SDK vám umožní toobuild rychlá napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) toorun aplikace v systému Windows, Mac a Linux. Hello nejnovější verzi hello Azure Cosmos DB .NET Core SDK je plně [Xamarin](https://www.xamarin.com) kompatibilní a použít toobuild aplikacemi, které cílí na iOS, Android a Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

Hello Azure Cosmos DB .NET Core Preview SDK vám umožní toobuild rychlá napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) toorun aplikace v systému Windows, Mac a Linux.

Hello Azure Cosmos DB .NET Core Preview SDK má parity funkcí s nejnovější verzí hello hello [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) a podporuje následující hello:
* Všechny [připojení režimy](performance-tips.md#networking): režim brány, přímé TCP a přímé HTTPs. 
* Všechny [úrovně konzistence](consistency-levels.md): silným, relace, typu s ohraničenou Prošlostí a Eventual.
* [Oddíly kolekce](partition-data.md). 
* [Účty databáze více oblasti a geografická replikace](distribute-data-globally.md).

Pokud máte otázky související toothis SDK, post příliš[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), nebo soubor problém v hello [úložiště github](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Verze & vyřazení kalendářních dat

| Verze | Datum vydání | Datum vyřazení |
| --- | --- | --- |
| [1.5.0](#1.5.0) |10. srpnu 2017 |--- | 
| [1.4.1](#1.4.1) |07 srpen 2017 |--- |
| [1.4.0](#1.4.0) |02 srpen 2017 |--- |
| [1.3.2](#1.3.2) |12. června 2017 |--- |
| [1.3.1](#1.3.1) |23 může 2017 |--- |
| [1.3.0](#1.3.0) |10. května 2017 |--- |
| [1.2.2](#1.2.2) |19. dubna 2017 |--- |
| [1.2.1](#1.2.1) |29. března 2017 |--- |
| [1.2.0](#1.2.0) |25. března 2017 |--- |
| [1.1.2](#1.1.2) |20. března 2017 |--- |
| [1.1.1](#1.1.1) |14. března 2017 |--- |
| [1.1.0](#1.1.0) |16 února 2017 |--- |
| [1.0.0](#1.0.0) |21 prosince 2016 |--- |
| [0.1.0-Preview](#0.1.0-preview) |15. listopadu 2016 |31. prosinci 2016 |

## <a name="see-also"></a>Viz také
toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby. 

