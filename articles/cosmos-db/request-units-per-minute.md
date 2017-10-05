---
title: "Azure CosmosDB: Požadované jednotky za minutu (RU/m) | Microsoft Docs"
description: "Zjistěte, jak snížit náklady na s využitím jednotek žádosti za minutu."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Jednotek žádosti za minutu v Azure Cosmos DB

Azure Cosmos DB slouží k vám pomohou dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s růstem vaší aplikace. Můžete zřídit propustnosti na kontejner Cosmos DB v obou, za sekundu a členitostí za minutu (RU/m). Zřízená propustnost v přírůstcích po jedné minutě slouží k zvládání nečekaných nárůstů úloh, které nastávají v přírůstcích po jedné sekundě. 

Tento článek obsahuje přehled toho, jak funguje zřizování jednotek žádosti za minutu (RU/m). Cílem v paměti se zřizováním RU/m je zajistit předvídatelný výkon kolem nepředvídatelným potřebám (obzvláště pokud potřebujete spustit analytics nad provozních dat) a spiky úlohy. Chceme mít naše zákazníky využívat další, které zřizování, můžete rychle škálování se jistotu propustnost.

Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:

* Funkce jednotek žádosti za minutu
* Jaký je rozdíl mezi jednotek žádosti za minutu a jednotek žádosti za sekundu?
* Jak zřídit RU/m?
* V části scénář, který je nutné zvážit zřizování jednotek žádosti za minutu?
* Postup použití portálu metrik s cílem optimalizovat Moje nákladů a výkonu?
* Zadejte, jaký typ požadavku můžete využívat rozpočtem RU/m?

## <a name="provisioning-request-units-per-minute-rum"></a>Zřizování jednotek žádosti za minutu (RU/m)

Pokud zřizujete Azure Cosmos DB v druhé členitost (RU/s), získáte záruka, že vaši žádost o úspěšná na nízkou latencí, pokud vaše propustnost nebyla překročena kapacitu poskytnutém v rámci této sekundu. RU/m členitost je na chvíli se zárukou, který neproběhne v rámci této minutu. Porovnání s bursting systémy, zajišťujeme, že, výkon, které máte je předvídatelný a můžete naplánovat na něm.

Způsob, jakým za minutu zřizování funguje je jednoduchý:

* RU/m se fakturuje každou hodinu a kromě RU/s. Další podrobnosti naleznete na adrese Azure Cosmos DB [stránce s cenami](https://aka.ms/acdbpricing).
* RU/m se dá nastavit na úrovni kolekce. To můžete udělat pomocí sady SDK (Node.js, Java nebo .net) nebo prostřednictvím portálu (také obsahovat úlohy MongoDB rozhraní API)
* Pokud je povoleno RU/m, pro každých 100 RU/s zřízený, získáte 1000 RU/m zřízený (poměr je 10 x)
* V dané druhý, jednotka žádosti využívá vaší RU/m zřizování pouze v případě, že jste překročili vaší za druhé zřizování v rámci této sekundu
* Po skončení doby 60 sekund (UTC), za minutu zřizování je opakovaného plnění
* RU/m lze povolit pouze pro kolekce s maximální zřizování 5 000 RU/s na jeden oddíl. Pokud velikost potřeb propustnost a mají vysokou úroveň zřizování na oddíl, zobrazí se zpráva s upozorněním

Níže je konkrétní příklad, ve kterém můžete zřídit zákazník 10kRU/s s 100kRU/m, ukládání 73 % náklady proti zřizování ve špičce (na 50kRU za sekundu) prostřednictvím dobou 90 sekundu v kolekci, která má 10 000 RU/s a 100 000 RU/m zřízené:

* 1 sekundu: RU/m rozpočtu je nastavený na 100 000
* 3. sekundu: Při tomto sekundu spotřeby jednotek žádosti byl 11,010 RUs, 1,010 RUs výše zřizování RU/s. Proto jsou 1,010 RUs odečtena z rozpočtu RU/m. 98,990 RUs jsou k dispozici pro další 57 sekund v rozpočtu RU/m
* 29 sekundu: během druhé, došlo k velké Špička (> 4 x vyšší než zřizování za sekundu) a spotřeby jednotek žádosti byl 46,920 RUs. 36,920 RUs jsou odečtena z rozpočtu RU/m vyřadit z 92,323 RUs (28th sekundu) k 55,403 RUs (29 sekundu)
* 61st sekundu: RU/m nároky se nastaví zpátky na 100 000 RUs.
 
![Graf zobrazující spotřeby a zřizování Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Určení požadavku jednotka kapacity pomocí RU/m

Při vytváření kolekci Azure Cosmos DB, je třeba zadat počet jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené pro kolekci. Můžete také rozhodnout, pokud chcete přidat RU za minutu. To lze provést prostřednictvím portálu nebo sady SDK. 

### <a name="through-the-portal"></a>Prostřednictvím portálu

Povolení nebo zakázání RU za minutu jednoduše vyžaduje kliknutí při zřizování kolekce. 

 ![Snímek obrazovky ukazující, jak nastavit RU/m na portálu Azure](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a>Prostřednictvím sady SDK
První to je důležité si uvědomit, že RU/m je dostupná jenom pro následující sady SDK:

* Rozhraní .net 1.14.0
* Java 1.11.0
* Node.js 1.12.0
* Python 2.2.0

Zde je fragment kódu pro vytvoření kolekce s 3 000 jednotek žádosti za jednotek žádosti druhý a 30 000 za minutu pomocí sady .NET SDK:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Zde je fragment kódu pro změnu propustnost kolekce do 5 000 jednotek žádosti za sekundu bez zřizování RU za minutu pomocí sady .NET SDK:

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Dobré podle scénáře

V této části poskytujeme přehled scénářů, které jsou vhodné pro povolení jednotek žádosti za minutu.

**Prostředí pro vývoj/testování:** vhodné. Během fáze vývoje Pokud testujete aplikace s různé úlohy, RU/m nabízejí flexibilitu v této fázi. Když [emulátoru](local-emulator.md) je skvělým bezplatný nástroj k testování Azure Cosmos DB. Ale pokud chcete spustit v cloudovém prostředí, budete mít flexibilitu s RU/m pro vašim požadavkům na výkon ad hoc. Můžete se věnovat více času, méně plně soustředit na první požadavkům na výkon. Doporučujeme od minimální zřizování RU/s a povolit RU/m.

**Musí nepředvídatelným, spiky, minutu členitost:** dobré nevejde – úspory: 25 75 %. Jsme viděli velké zlepšování z RU/m a většině produkčních scénářích se do této skupiny. Pokud máte IoT pracovního vytížení, který má Špička několikrát za minutu, pokud máte dotazy spuštěn, když se váš systém díky hromadného vložení ve stejnou dobu, budete potřebovat víc kapacity pro potřeby spiky handeling. Doporučujeme, abyste optimalizace potřeb prostředků s použitím náš přístup krok za krokem níže.

 ![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Obrázek - RU spotřeba srovnávacího testu*

**Jistotu:** dobré nevejde – úspory: 10-20 %. V některých případech jenom chcete mít jistotu a nestarat se o potenciální vrcholů a omezení. Tato funkce je ten správný za vás. V takovém případě doporučujeme povolení RU/m a mírně nižší vaší za druhé zřizování. Tento případ se liší od výše jako nebude zkoušet za účelem optimalizace intenzivně zřizování. To je více přistupovat "Nula omezení", který se v.

Kritické operace s potřebami ad hoc: doporučujeme někdy umožníte pouze kritické operace přístupu RU/m rozpočtu, není-li získat rozpočtu využívat ad hoc nebo méně důležité operace. Který může být snadno definován v následující části.

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a>Pomocí portálu metriky za účelem optimalizace nákladů a výkonu

**V následujících týdnech jsme další vyvíjet obsah kolem monitorování RUs minutu spotřeby za účelem optimalizace potřeb propustnost.**

Prostřednictvím portálu metriky můžete zobrazit, kolik regulární RU sekund využívat versus RU minut. Monitorování tyto metriky by měly pomoci při optimalizaci vašeho zřizování. 

Doporučujeme, abyste krok za krokem přístup k použití RU/m využít. Při každém kroku byste měli mít přehled o využívání RU představující úplný cyklus úlohy (to může být hodiny, dny, nebo i týdny) a získat informace o využití těchto můžete zřídit.

Princip za tento přístup je vaše propustnost zřizování co nejblíže k zřizování bodu, který odpovídá kritériím výkonu níže. 

![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
Pokud chcete pochopit optimální zřizování bod pro úlohy, je potřeba pochopit:

* Spotřeba vzory: žádné, nepravidelným nebo dlouhodobě špičky? (2 x průměr) malá, střední a velké (> 10 x průměr) špičky?
* Procenta omezenému požadavků: udělat si projděte Pokud máte chvilku omezování? Pokud ano, jak moc? 

Po zjištění, jaké jsou vaše cíle, bude možné získat blíže optimální zřizování.

K usnadnění, chceme poskytovat celkové pokyny o tom, jak optimalizovat vaše zřizování na základě vaší spotřeby RU/m. V tomto návodu se nevztahuje na všechny druh úloh, ale je založena na znalosti privátní Preview verzi. Může se nám změnit tyto standardní hodnoty jako jsme Další informace:

|% Využití RU/m|Úroveň využití RU/m|Doporučené akce pro zřizování|
|---|---|---|
|0-1%|V části využití|Nižší využívat další RU/m. RU/s|
|1-10%|Použití v pořádku|Zachovat na stejné úrovni zřizování|
|Více než 10 %|Přes využití|Zvýšit spolehnout menší na RU/m. RU/s|

## <a name="select-which-operations-can-consume-the-rum-budget"></a>Vyberte operací, které můžou využívat nároky RU/m

Na žádost o úrovni můžete můžete také povolit nebo zakázat RU/m nároky na požadavek bez ohledu na typ operaci vyřídit. Pokud se využívá regulární zřízené nároky RUs za sekundu a požadavek nelze využívat nároky RU/m, budou omezeny tuto žádost. Ve výchozím nastavení každá žádost obsloužených RU/m rozpočtu, pokud je aktivovaná nároky propustnost RU/m. 

Zde je fragment kódu pro zakázání RU/m nároky pomocí DocumentDB rozhraní API pro operace CRUD a dotazu.

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Další kroky

V tomto článku jsme popsali, jak rozdělení funguje v Azure Cosmos DB, jak můžete vytvořit dělené kolekce a jak můžete vybrat vhodným klíčem oddílu pro vaši aplikaci.

* Proveďte škálování a výkon testování pomocí Azure Cosmos DB. V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.
* Začínáme s kódování [sady SDK](documentdb-sdk-dotnet.md) nebo [REST API](/rest/api/documentdb/).
* Další informace o [zřízené propustnosti](request-units.md) v Azure Cosmos DB 

