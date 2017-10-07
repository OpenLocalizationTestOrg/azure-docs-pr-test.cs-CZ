---
title: "aaaAzure Cosmos DB Node.js API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello rozhraní API Node.js a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB Node.js SDK."
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d450b9a9ea7b0f4717ddae8940121fc458ea3744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a>Azure SDK Cosmos DB Node.js: Poznámky k verzi a prostředky
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

<tr><td>**Stáhněte si sadu SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td>**Dokumentaci k rozhraní API**</td><td>[Referenční dokumentace rozhraní API Node.js](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td>**Pokyny k instalaci sady SDK**</td><td>[Pokyny k instalaci](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td>**Přispívat tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td>**Ukázky**</td><td>[Ukázky kódu Node.js](documentdb-nodejs-samples.md)</td></tr>

<tr><td>**Kurz Začínáme**</td><td>[Začínáme s hello Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>

<tr><td>**Kurz vývoje webové aplikace**</td><td>[Vytvoření webové aplikace Node.js pomocí Azure Cosmos DB](documentdb-nodejs-application.md)</td></tr>

<tr><td>**Aktuální podporované platformy**</td><td> 
[Verze 6.x Node.js](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

### <a name="1.12.2"/>1.12.2</a>
*   dokumentace npm pevné.

### <a name="1.12.1"/>1.12.1</a>
* Pevná chyby ve executeStoredProcedure kde dokumentů, které měl speciální znaky znakové sady Unicode (LS, PS).
* Opravit chyby při zpracování dokumentů s znaky kódování Unicode v hello klíč oddílu.
* Opravené podpora pro vytváření kolekcí s hello název média. Problém Githubu #114.
* Opravené podpora oprávnění autorizační token. Problém Githubu #178.

### <a name="1.12.0"/>1.12.0</a>
* Přidaná podpora pro novou [úroveň konzistence](consistency-levels.md) názvem ConsistentPrefix.
* Přidaná podpora pro UriFactory.
* Opravit chyby Podpora kódování Unicode. #171 potíže Githubu.

### <a name="1.11.0"/>1.11.0</a>
* Podpora přidání hello dotazy agregace (COUNT, MIN, MAX, součet a průměr).
* Přidání hello možnost řízení stupně paralelního zpracování pro různé dotazy oddílu.
* Přidání hello možnost pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos Azure.
* Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.
* Chyby tokenu pevné hello pokračování pro kolekce tvořené jedním oddílem. Problém Githubu #107.
* Chyby opravené hello executeStoredProcedure při zpracování 0 jako jeden param. Problém Githubu #155.

### <a name="1.10.2"/>1.10.2</a>
* Opravené identifikační hlavička tooinclude hello SDK verze.
* Méně závažné kód čištění.

### <a name="1.10.1"/>1.10.1</a>
* Při použití hello SDK tootarget hello emulator(hostname=localhost) se zakazuje ověření protokol SSL.
* Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.

### <a name="1.10.0"/>1.10.0</a>
* Přidaná podpora pro křížové paralelní dotazy oddílu.
* Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.

### <a name="1.9.0"/>1.9.0</a>
* Podpora zásad přidané opakování omezenému požadavky. (Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena hello retryAfter čas v hlavičku odpovědi hello. Časový interval opakování pevné lze nyní nastavit jako součást hello RetryOptions vlastnost u objektu ConnectionPolicy hello Pokud chcete, aby tooignore hello retryAfter čas vrácená serverem mezi opakovanými pokusy hello. Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď hello s kódem chyby 429. Tentokrát lze také ve hello RetryOptions vlastnosti v objektu ConnectionPolicy přepsat.
* Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako hello hlavičky odpovědi v každé žádosti toodenote hello omezení počtu a hello kumulativní čas požadavku hello čekali mezi opakovanými pokusy hello zkuste provést znovu.
* byla přidána Hello RetryOptions třída vystavení hello RetryOptions vlastnost u hello ConnectionPolicy třídy, které můžou být použité toooverride některé hello výchozí možnosti opakování.

### <a name="1.8.0"/>1.8.0</a>
* Podpora přidání hello účty databáze více oblast.

### <a name="1.7.0"/>1.7.0</a>
* Přidání hello podporu pro funkci tooLive(TTL) čas pro dokumenty.

### <a name="1.6.0"/>1.6.0</a>
* Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Opravené chyby RangePartitionResolver.resolveForRead, kde ji nebyl vrácení odkazy z důvodu chybné concat tooa výsledků.

### <a name="1.5.5"/>1.5.5</a>
* Pevné hashParitionResolver resolveForRead(): žádné předaný klíč oddílu byla při vyvolání výjimky, místo vrací seznam všech registrovaných odkazů.

### <a name="1.5.4"/>1.5.4</a>
* Řeší problém [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -vyhrazené agenta HTTPS: neměli upravovat hello globální agenta pro účely Azure Cosmos DB. Použijte vyhrazenou agenta pro všechny požadavky hello lib.

### <a name="1.5.3"/>1.5.3</a>
* Řeší problém [#81](https://github.com/Azure/azure-documentdb-node/issues/81) – správně zpracovat pomlčky v ID média.

### <a name="1.5.2"/>1.5.2</a>
* Řeší problém [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -naslouchací proces EventEmitter úniku upozornění.

### <a name="1.5.1"/>1.5.1</a>
* Řeší problém [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -přejmenovat složku Hash toohash systémů malá a velká písmena.

### <a name="1.5.0"/>1.5.0</a>
* Podpora horizontálního dělení implementace přidáním hash p & ro překladače oddílu.

### <a name="1.4.0"/>1.4.0</a>
* Implementujte Upsert. Nové metody upsertXXX na documentClient.

### <a name="1.3.0"/>1.3.0</a>
* Přeskočené toobring čísla verze v zarovnání s dalších sadách SDK.

### <a name="1.2.2"/>1.2.2</a>
* Rozdělení Q nabízí obálku toonew úložiště.
* Aktualizujte soubor toopackage pro npm registru.

### <a name="1.2.1"/>1.2.1</a>
* Implementuje ID založené na směrování.
* Řeší problém [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuální vlastnost je v konfliktu s má objekt current() metoda.

### <a name="1.2.0"/>1.2.0</a>
* Byla přidána podpora pro geoprostorové index.
* Ověří vlastnost id pro všechny prostředky. Identifikátory prostředků nesmí obsahovat?, /, # &#47; &#47; znaky nebo končit mezerou.
* Přidá nový tooResourceResponse "průběh transformace index" záhlaví.

### <a name="1.1.0"/>1.1.0</a>
* Implementuje zásady indexování V2.

### <a name="1.0.3"/>1.0.3</a>
* Problém [#40](https://github.com/Azure/azure-documentdb-node/issues/40) – implementována eslint grunt konfigurace v hello jádra a promise SDK.

### <a name="1.0.2"/>1.0.2</a>
* Problém [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -lišící obálku nezahrnuje hlavičky s chybou.

### <a name="1.0.1"/>1.0.1</a>
* Přidání readConflicts, readConflictAsync a queryConflicts implementují možnost tooquery konflikty.
* Aktualizované dokumentace rozhraní API.
* Problém [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync chyby.

### <a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Verze & vyřazení kalendářních dat
Společnost Microsoft poskytuje oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.

Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK, jako například se doporučuje tento můžete vždy upgradu toohello nejnovější verze sady SDK co nejdříve.

Každá žádost tooCosmos DB pomocí vyřazeno sady SDK je odmítnuta službou hello.

<br/>

| Verze | Datum vydání | Datum vyřazení |
| --- | --- | --- |
| [1.12.2](#1.12.2) |10. srpnu 2017 |--- |
| [1.12.1](#1.12.1) |10. srpnu 2017 |--- |
| [1.12.0](#1.12.0) |10. května 2017 |--- |
| [1.11.0](#1.11.0) |16 března 2017 |--- |
| [1.10.2](#1.10.2) |27. ledna 2017 |--- |
| [1.10.1](#1.10.1) |22. prosinci 2016 |--- |
| [1.10.0](#1.10.0) |03. října 2016 |--- |
| [1.9.0](#1.9.0) |07 července 2016 |--- |
| [1.8.0](#1.8.0) |14. června 2016 |--- |
| [1.7.0](#1.7.0) |26. dubna 2016 |--- |
| [1.6.0](#1.6.0) |29. března 2016 |--- |
| [1.5.6](#1.5.6) |08 března 2016 |--- |
| [1.5.5](#1.5.5) |02. února 2016 |--- |
| [1.5.4](#1.5.4) |01. února 2016 |--- |
| [1.5.2](#1.5.2) |26 leden 2016 |--- |
| [1.5.2](#1.5.2) |22. ledna 2016 |--- |
| [1.5.1](#1.5.1) |4 leden 2016 |--- |
| [1.5.0](#1.5.0) |31. prosince 2015 |--- |
| [1.4.0](#1.4.0) |06 říjen 2015 |--- |
| [1.3.0](#1.3.0) |06 říjen 2015 |--- |
| [1.2.2](#1.2.2) |10 září 2015 |--- |
| [1.2.1](#1.2.1) |15 srpen 2015 |--- |
| [1.2.0](#1.2.0) |05 srpen 2015 |--- |
| [1.1.0](#1.1.0) |09 července 2015 |--- |
| [1.0.3](#1.0.3) |04 červen 2015 |--- |
| [1.0.2](#1.0.2) |23 květen 2015 |--- |
| [1.0.1](#1.0.1) |15. května 2015 |--- |
| [1.0.0](#1.0.0) |08 duben 2015 |--- |

## <a name="faq"></a>Nejčastější dotazy
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Viz také
toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.

