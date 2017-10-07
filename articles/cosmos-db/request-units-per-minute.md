---
title: "Azure CosmosDB: Požadované jednotky za minutu (RU/m) | Microsoft Docs"
description: "Zjistěte, jak tooreduce náklady s využitím požadované jednotky za minutu."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Jednotek žádosti za minutu v Azure Cosmos DB

Azure Cosmos DB je navrženou toohelp dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s růstem vaší aplikace. Můžete zřídit propustnosti na kontejner Cosmos DB v obou, za sekundu a členitostí za minutu (RU/m). Hello zřízené propustnosti na za minutu členitost je použité toomanage neočekávané špičky v hello úlohy, ke kterým dochází v rozlišením za sekundu. 

Tento článek obsahuje přehled toho, jak funguje hello zřizování jednotek žádosti za minutu (RU/m). cílem Hello nezapomeňte se zřizováním RU/m je tooprovide předvídatelný výkon kolem nepředvídatelným potřebám (obzvláště pokud potřebujete toorun analytics nad provozních dat) a spiky úlohy. Chceme toohave další hello propustnost, které zřizování, můžete rychle škálování se jistotu využívat naše zákazníky.

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:

* Funkce jednotek žádosti za minutu
* Jaký je rozdíl hello jednotek žádosti za minutu a jednotek žádosti za sekundu?
* Jak tooprovision RU/m?
* V části scénář, který je nutné zvážit zřizování jednotek žádosti za minutu?
* Jak toouse hello portálu metriky toooptimize Moje nákladů a výkonu?
* Zadejte, jaký typ požadavku můžete využívat rozpočtem RU/m?

## <a name="provisioning-request-units-per-minute-rum"></a>Zřizování jednotek žádosti za minutu (RU/m)

Pokud zřizujete Azure Cosmos DB v druhé členitosti hello (RU/s), získáte hello záruka, že vaši žádost o úspěšná na nízkou latencí, pokud vaše propustnost nebyla překročena hello kapacity poskytnutém v rámci této sekundu. S RU/m členitosti hello je na minutu hello s hello záruka, že neproběhne v rámci této minutu. Porovnání toobursting systémy jsme Ujistěte se, že je předvídatelný výkon hello získáte a můžete naplánovat na něm.

způsob Hello za minutu zřizování funguje je jednoduchý:

* RU/m se fakturuje každou hodinu a přidání tooRU/s. Další podrobnosti naleznete na adrese Azure Cosmos DB [stránce s cenami](https://aka.ms/acdbpricing).
* RU/m se dá nastavit na úrovni kolekce. To můžete udělat pomocí hello sady SDK (Node.js, Java nebo .net) nebo prostřednictvím portálu hello (také obsahovat úlohy MongoDB rozhraní API)
* Pokud je povoleno RU/m, pro každých 100 RU/s zřízený, získáte 1000 RU/m zřízený (hello poměr je 10 x)
* V dané druhý, jednotka žádosti využívá vaší RU/m zřizování pouze v případě, že jste překročili vaší za druhé zřizování v rámci této sekundu
* Jednou hello končí dobu 60 sekund (UTC), je opakovaného plnění hello za minutu zřizování
* RU/m lze povolit pouze pro kolekce s maximální zřizování 5 000 RU/s na jeden oddíl. Pokud velikost potřeb propustnost a mají vysokou úroveň zřizování na oddíl, zobrazí se zpráva s upozorněním

Níže je konkrétní příklad, ve kterém můžete zřídit zákazník 10kRU/s s 100kRU/m, ukládání 73 % náklady proti zřizování ve špičce (na 50kRU za sekundu) prostřednictvím dobou 90 sekundu v kolekci, která má 10 000 RU/s a 100 000 RU/m zřízené:

* 1 sekundu: hello RU/m rozpočtu je nastavený na 100 000
* 3. sekundu: Při této druhé hello využívání jednotek žádosti byl 11,010 RUs, 1,010 RUs výše zřizování hello RU/s. Proto jsou 1,010 RUs odečtena z rozpočtu RU/m hello. 98,990 RUs jsou k dispozici pro hello další 57 sekund v rozpočtu RU/m hello
* 29 sekundu: během druhé, došlo k velké Špička (> 4 x vyšší než zřizování za sekundu) a hello spotřeby jednotek žádosti byl 46,920 RUs. Snižuje 36,920 RUs hello RU/m rozpočtu z 92,323 too55 RUs (28th sekundu), 403 RUs (29 sekundu)
* 61st sekundu: RU/m nároky se nastaví zpět too100 000 RUs.
 
![Graf zobrazující hello spotřeby a zřizování Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Určení požadavku jednotka kapacity pomocí RU/m

Když vytváříte kolekci Azure Cosmos DB, můžete zadat číslo hello jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené pro kolekci hello. Můžete také rozhodnout, pokud chcete, aby tooadd RU za minutu. To lze provést prostřednictvím hello portál nebo hello SDK. 

### <a name="through-hello-portal"></a>Prostřednictvím hello portálu

Povolení nebo zakázání RU za minutu jednoduše vyžaduje kliknutí při zřizování kolekce. 

 ![Snímek obrazovky zobrazující jak tooset RU/m v hello portálu Azure](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a>Prostřednictvím hello SDK
První to je důležité toonote, RU/m je dostupná jenom pro následující sady SDK hello:

* Rozhraní .net 1.14.0
* Java 1.11.0
* Node.js 1.12.0
* Python 2.2.0

Zde je fragment kódu pro vytvoření kolekce s 3 000 jednotek žádosti za jednotek žádosti druhý a 30 000 za minutu pomocí hello .NET SDK:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Zde je fragment kódu pro změnu hello propustnost kolekce too5, hello 000 jednotek žádosti za sekundu bez zřizování RU za minutu pomocí .NET SDK:

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Dobré podle scénáře

V této části poskytujeme přehled scénářů, které jsou vhodné pro povolení jednotek žádosti za minutu.

**Prostředí pro vývoj/testování:** vhodné. Během fáze vývoj hello Pokud testujete aplikace s různé úlohy, RU/m nabízejí flexibilitu hello v této fázi. Při hello [emulátoru](local-emulator.md) je skvělý nástroj volné tootest Azure Cosmos DB. Ale pokud chcete toostart v cloudovém prostředí, budete mít flexibilitu s RU/m pro vašim požadavkům na výkon ad hoc. Můžete se věnovat více času, méně plně soustředit na první požadavkům na výkon. Doporučujeme počínaje hello minimální zřizování RU/s a povolit RU/m.

**Musí nepředvídatelným, spiky, minutu členitost:** dobré nevejde – úspory: 25 75 %. Jsme viděli velké zlepšování z RU/m a většině produkčních scénářích se do této skupiny. Pokud máte IoT pracovního vytížení, který má Špička několikrát za minutu, pokud máte dotazy spuštěnými při systému provede hromadného vložení na hello stejný čas, budete potřebovat víc kapacity pro handeling spiky potřebuje. Doporučujeme, abyste optimalizace potřeb prostředků s použitím náš přístup krok za krokem níže.

 ![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Obrázek - RU spotřeba srovnávacího testu*

**Jistotu:** dobré nevejde – úspory: 10-20 %. V některých případech stačí chcete jistotu toohave a nestarat se o potenciální vrcholů a omezení. Tato funkce je pro vás ideální hello jeden. V takovém případě doporučujeme povolení RU/m a mírně nižší vaší za druhé zřizování. Tento případ se liší od hello výše jako nebude zkoušet toooptimize intenzivně zřizování. To je více přistupovat "Nula omezení", který se v.

Kritické operace s potřebami ad hoc: doporučujeme někdy tooonly umožní kritické operace přístupu RU/m rozpočtu, proto nepodporuje hello nároky využívat ad hoc nebo méně důležité operace. Který může být snadno definováno v následující části hello.

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a>Pomocí hello portálu metriky toooptimize nákladů a výkonu

**V následujících týdnech hello budeme vyvíjet další obsah hello kolem monitorování toooptimize RUs minutu spotřebu, musí vaše propustnost.**

Prostřednictvím portálu metriky hello můžete zobrazit, kolik regulární RU sekund využívat versus RU minut. Monitorování tyto metriky by měly pomoci při optimalizaci vašeho zřizování. 

Krok za krokem přístup návod, jak se doporučuje toouse využít tooyour RU/m. Při každém kroku byste měli mít přehled o využívání hello RU představující úplný cyklus úlohy (to může být hodiny, dny, nebo i týdny) a přehledné na využití hello můžete zřídit.

Princip Hello za tento přístup je toomake vaší propustnost zřizování jako zavřete jako možné tooa zřizování bod, který odpovídá kritériím výkonu níže. 

![Graf zobrazující žádost o spotřebě v členitosti 5 minut](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
toounderstand hello optimální zřizování bodu pro úlohy, je třeba toounderstand:

* Spotřeba vzory: žádné, nepravidelným nebo dlouhodobě špičky? (2 x průměr) malá, střední a velké (> 10 x průměr) špičky?
* Procenta omezenému požadavků: udělat si projděte Pokud máte chvilku omezování? Pokud ano, jak moc? 

Po zjištění, jaké jsou vaše cíle, bude možné tooget blíže toohello optimální zřizování.

tooassist, chceme tooprovide celkové pokyny na tom, jak vaše zřizování toooptimize podle vaší spotřeby RU/m. Tyto pokyny netýká tooall druh úloh, ale je založena na hello privátní Preview verzi znalostní báze. Může se nám změnit tyto standardní hodnoty jako jsme Další informace:

|% Využití RU/m|Úroveň využití RU/m|Doporučené akce pro zřizování|
|---|---|---|
|0-1%|V části využití|Nižší další RU/m tooconsume RU/s|
|1-10%|Použití v pořádku|Zachovat hello stejné zřizování úroveň|
|Více než 10 %|Přes využití|Zvýšit toorely RU/s menší na RU/m|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a>Vyberte operací, které můžou využívat nároky RU/m hello

Na úrovni žádost vám může také povolit nebo zakázat RU/m nároky tooserve hello požadavek bez ohledu na typ operace. Pokud obsazením regulární zřízené nároky RUs za sekundu a hello žádost nemůže využívat hello RU/m rozpočtu, budou omezeny tuto žádost. Ve výchozím nastavení každá žádost obsloužených RU/m rozpočtu, pokud je aktivovaná nároky propustnost RU/m. 

Zde je fragment kódu pro zakázání RU/m nároky pomocí hello DocumentDB rozhraní API pro operace CRUD a dotazu.

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Další kroky

V tomto článku jsme popsali, jak rozdělení funguje v Azure Cosmos DB, jak můžete vytvořit dělené kolekce a jak můžete vybrat vhodným klíčem oddílu pro vaši aplikaci.

* Proveďte škálování a výkon testování pomocí Azure Cosmos DB. V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.
* Začínáme s hello kódování [sady SDK](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/).
* Další informace o [zřízené propustnosti](request-units.md) v Azure Cosmos DB 

