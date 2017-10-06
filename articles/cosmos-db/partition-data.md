---
title: "aaaPartitioning a horizontální škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak tooconfigure vytváření oddílů a klíče oddílů a jak toopick hello právo oddílu klíč pro vaši aplikaci."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87d56db8c4ccc6b94b1650baff0fcfb3db6d1777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopartition-and-scale-in-azure-cosmos-db"></a>Jak toopartition a škálování v Azure Cosmos DB

[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je toohelp služba navržená tak, globální distribuované, více modelu databáze můžete dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem. Tento článek obsahuje přehled o tom, jak dělení platí pro všechna data hello modelů v Azure Cosmos DB a popisuje, jak konfigurovat Azure Cosmos DB kontejnery tooeffectively škálování aplikace.

Dělení a klíče oddílů jsou také popsané v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Shireesh Thota pátek.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Vytváření oddílů v Azure Cosmos DB
V Azure DB Cosmos můžete ukládat a zadávat dotazy na data bez schémat s pořadí milisekundu odezvy v jakémkoli měřítku. Cosmos DB poskytuje kontejnery pro ukládání dat volat **kolekce (pro dokument), grafy nebo tabulky**. Kontejnery jsou logické prostředky a může mít rozsah jeden nebo více fyzických oddílů nebo serverů. Hello počet oddílů je dáno DB Cosmos na základě hello velikost úložiště a zřízené propustnosti hello hello kontejneru. Každý oddíl v Cosmos DB má pevně stanovený objem zálohovaná na SSD úložiště s ním spojená a se replikují pro vysokou dostupnost. Oddíl správy je plně spravovat Azure Cosmos DB a nemáte mít složitý kód toowrite nebo spravovat vaše oddíly. Kontejnery DB cosmos neomezená z hlediska úložiště a propustnosti. 

![vodorovné](./media/introduction/azure-cosmos-db-partitioning.png) 

Vytváření oddílů je transparentní tooyour aplikace. Cosmos DB podporuje rychlé čtení a zápisy, dotazy, transakční logiku, úrovně konzistence a řízení přístupu podrobných prostřednictvím metody nebo rozhraní API tooa jediný kontejner prostředků. Hello služba zpracovává rozděluje data mezi oddílů a směrování správné oddíl toohello dotazu žádosti. 

Jak funguje dělení Každá položka musí mít klíč oddílu a klíč řádku, které jeho jednoznačné identifikaci. Klíč oddílu funguje jako logický oddíl pro vaše data a poskytne Cosmos DB přirozené hranice pro distribuci dat mezi oddílů. Stručně řečeno zde je Princip vytváření oddílů v Azure Cosmos DB:

* Zřídit Cosmos DB kontejner s `T` propustnost požadavky/s
* Pozadí se děje hello Cosmos DB zřizuje oddíly potřeby tooserve `T` požadavky/s. Pokud `T` je vyšší než maximální propustnost hello na oddíl `t`, pak Cosmos DB zřizuje `N`  =  `T/t` oddíly
* Cosmos DB přiděluje hello klíče místo oddílu klíče hash rovnoměrně mezi hello `N` oddíly. Ano každý oddíl (fyzickém oddílu) hostitelem hodnoty klíče oddílu 1-N (logické oddíly)
* Při fyzickém oddílu `p` dosáhnou limitu úložiště Cosmos DB bezproblémově rozdělí `p` do dvou nových oddílů `p1` a `p2` a distribuuje hodnoty odpovídající tooroughly poloviční hello klíče tooeach Dobrý den oddíly. Toto rozdělení operaci je neviditelná tooyour aplikace.
* Podobně když zřídit propustnost vyšší než `t*N` propustnost, Cosmos DB rozdělí jeden nebo více vyšší propustnost vaší oddíly toosupport hello

Sémantika Hello pro klíče oddílu je mírně odlišný toomatch hello sémantiku každé rozhraní API, jak je znázorněno v následující tabulce hello:

| Rozhraní API | Klíč oddílu | Klíč řádku |
| --- | --- | --- |
| DocumentDB | Cesta klíče vlastní oddíl | Pevná`id` | 
| MongoDB | vlastní horizontálních klíč  | Pevná`_id` | 
| Graph | klíčovou vlastností vlastní oddíl | Pevná`id` | 
| Table | Pevná`PartitionKey` | Pevná`RowKey` | 

Cosmos DB používá algoritmus HMAC rozdělení do oddílů. Při zápisu položky Cosmos DB hashuje hodnotu klíče oddílu hello a použití hello rozdělí výsledek toodetermine která oddílu toostore hello položka v. Hello cosmos DB úložiště hello všechny položky se stejným klíčem oddílu v jednom fyzickém oddílu. Vybraná Hello hello klíč oddílu je rozhodnutí o důležité, abyste měli toomake v době návrhu. Je třeba vybrat název vlastnosti, která má široký rozsah hodnot a má i přístupové vzorce.

> [!NOTE]
> Je nejlepší postup toohave klíč oddílu s mnoha jedinečných hodnot (100s-1000s minimálně).
>

Kontejnery Azure Cosmos DB se dá vytvořit jako "Pevná" nebo "neomezená." Kontejnery pevné velikosti mají maximální limit 10 GB a propustnost 10 000 RU/s. Některé rozhraní API umožňují toobe klíče oddílu hello vynechání kontejnerů s pevnou velikostí. toocreate kontejneru jako neomezená, je nutné zadat minimální propustnost 2 500 RU/s.

## <a name="partitioning-and-provisioned-throughput"></a>Vytváření oddílů a zřízené propustnosti
Cosmos DB je určená pro předvídatelný výkon. Když vytvoříte kontejner, můžete vyhradit propustnost z hlediska  **[požadované jednotky](request-units.md) (RU) za sekundu s potenciální rozšíření pro RU za minutu**. Každý požadavek není přiřazen nákladů jednotek žádosti, která je přiměřené toohello množství systémové prostředky jako procesoru, paměti a spotřebovávají hello operaci vstupně-výstupní operace. Čtení 1 KB dokumentu s konzistence typu relace spotřebuje jednu jednotku požadavku. Pro čtení je 1 RU bez ohledu na počet hello položek, které uložené nebo hello počet souběžných požadavků, které jsou spuštěné v hello stejnou dobu. Položky větší vyžadují vyšší jednotek žádosti v závislosti na velikosti hello. Pokud znáte hello velikost vašeho entit a hello počet čtení toosupport pro vaši aplikaci, můžete zřídit hello přesné množství propustnost požadované pro vaše aplikace je vyžaduje čtení. 

> [!NOTE]
> hello úplnou propustnost tooachieve hello kontejneru, musíte zvolit, klíč oddílu, který vám umožní tooevenly distribuci požadavků mezi některé hodnoty klíče jedinečné oddílu.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-hello-azure-cosmos-db-apis"></a>Práce s hello rozhraní API Správce Azure Cosmos DB
Můžete použít hello portál Azure nebo Azure CLI toocreate kontejnerů a škálování je kdykoli. Tato část uvádí, jak toocreate kontejnery a zadejte hello propustnost a oddíl definici klíče v každé z hello podporované rozhraní API.

### <a name="documentdb-api"></a>Rozhraní DocumentDB API
Následující ukázka Hello ukazuje, jak hello toocreate kontejneru (kolekce) pomocí rozhraní API DocumentDB. Můžete najít další podrobnosti v [rozdělení do oddílů s rozhraním API DocumentDB](partition-data.md).

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Můžete si přečíst položku (dokument) pomocí hello `GET` metoda v hello REST API nebo pomocí `ReadDocumentAsync` v jednom z hello sady SDK.

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>Rozhraní MongoDB API
S hello MongoDB rozhraní API můžete vytvořit kolekci horizontálně dělené prostřednictvím vaše oblíbené nástroje, ovladače nebo sady SDK. V tomto příkladu používáme hello prostředí Mongo pro vytvoření kolekce hello.

V hello prostředí Mongo:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Výsledky:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Rozhraní Table API

Pomocí hello API tabulky je určit hello propustnost pro tabulky v hello appSettings konfiguraci pro aplikaci:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Potom můžete vytvořit tabulku pomocí Azure Table storage hello SDK. klíč oddílu Hello je vytvořena implicitně jako hello `PartitionKey` hodnotu. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

Můžete načíst jednu entitu pomocí hello následující fragment kódu:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
V tématu [vývoj s hello tabulky API](tutorial-develop-table-dotnet.md) další podrobnosti.

### <a name="graph-api"></a>Graph API

S hello rozhraní Graph API musíte použít hello portál Azure nebo rozhraní příkazového řádku toocreate kontejnerů. Alternativně vzhledem k tomu, že Azure Cosmos DB je více modelu, můžete použít jeden hello toocreate ostatní modely a škálovat vaše grafu kontejneru.

Může číst všechny vrchol nebo Microsoft edge v Gremlin hello klíč oddílu a id. Graf s oblasti ("USA") jako klíč oddílu hello a "Seattle" jako klíč řádku hello, například můžete najít pomocí následující syntaxe hello vrchol:

```
g.V(['USA', 'Seattle'])
```

Stejné jako u okraje, můžete odkazovat pomocí hello klíč oddílu a klíč řádku okraj.

```
g.E(['USA', 'I5'])
```

V tématu [Gremlin podpora systému Cosmos DB](gremlin-support.md) další podrobnosti.


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>Návrh a vytváření oddílů
tooscale efektivně s Azure Cosmos DB, musíte toopick vhodným klíčem oddílu při vytváření vašeho kontejneru. Existují dva klíčové faktory pro výběr klíč oddílu:

* **Hranice pro dotaz a transakce**: zvoleného klíč oddílu by měl vyrovnávat hello nutné tooenable hello použití transakce proti hello požadavek toodistribute vaší entity ve více tooensure klíče oddílu škálovatelné řešení. V jedné extreme, můžete nastavit hello stejným klíčem oddílu pro všechny položky, ale to může omezit škálovatelnost hello vašeho řešení. V hello jiných extreme můžete přiřadit oddílu jedinečný klíč pro každou položku, která by byla vysoce škálovatelné, ale by bránily použití transakcí křížové dokumentu prostřednictvím uložených procedur a aktivačních událostí. Klíč ideální oddílu je jedna, díky kterému toouse efektivní dotazy a který má dostatečná mohutnost tooensure řešení je škálovatelná. 
* **Žádné úložiště a výkon kritických bodů**: je důležité toopick vlastnost, která umožňuje zapíše toobe rozmístěny v různých jedinečných hodnot. Požadavky toohello stejným klíčem oddílu nesmí být delší než hello propustnost jednoho oddílu a jsou omezené. Proto je důležité toopick klíč oddílu, který nemá za následek "aktivní body" v rámci vaší aplikace. Od všech hello dat pro jeden oddíl klíč musí být uložen v rámci oddílu, je také vhodné tooavoid klíče oddílů, které mají velký objem dat pro hello stejnou hodnotu. 

Podívejme se na několik reálných scénářů a klíče dobrý oddílů pro každou:
* Pokud jste implementace back-end profilu uživatele, hello ID uživatele je vhodná pro klíč oddílu.
* Pokud ukládáte data IoT například stavu zařízení, je ID zařízení dobrou volbou pro klíč oddílu.
* Pokud používáte Azure Cosmos DB pro protokolování data časové řady, pak hello název hostitele nebo ID procesu je vhodná pro klíč oddílu.
* Pokud máte víceklientské architektuře, hello ID klienta je vhodná pro klíč oddílu.

V některých případech jako IoT použití a uživatelské profily, hello klíč oddílu může být hello stejné jako vaše id (klíč dokumentu). V jiných jako data hello časové řady může mít klíč oddílu, který se liší od hello id.

### <a name="partitioning-and-loggingtime-series-data"></a>Vytváření oddílů a protokolování nebo časových řad dat
Mezi běžné případy použití hello Cosmos databáze je pro protokolování a telemetrie. Vzhledem k tomu může být nutné tooread a zápis obrovské objemy dat je důležité toopick vhodným klíčem oddílu. Výběr Hello závisí na čtení a zápisu sazby a typy dotazů očekávat toorun. Tady jsou některé tipy, jak toochoose vhodným klíčem oddílu.

* Pokud váš případ použití zahrnuje malý počet zapíše pak pomocí souhrnu hello časové razítko, například datum jako klíč oddílu je dobrý způsob shromažďování po dlouhou dobu, a nutnost tooquery podle rozsahů časová razítka a ostatní filtry. To vám umožní tooquery přes všechny hello data pro datum z jednoho oddílu. 
* Pokud vaše úlohy je napsán náročné, což je více obvyklé, používejte klíč oddílu, který není založen na časové razítko tak, aby Cosmos DB můžete rovnoměrně distribuovat zápisy mezi různé oddíly. Název hostitele, ID procesu, ID aktivity nebo jinou vlastnost s vysokou kardinalitou tady je vhodné použít. 
* Třetí přístup je hybridní, jeden kde máte více kontejnerů, jednu pro každý den/měsíc a klíč oddílu hello je podrobné vlastnosti, jako je název hostitele. Tato akce nemá hello výhody, kterou můžete nastavit různé propustnost podle hello časový interval, například hello kontejner pro aktuální měsíc hello je opatřen vyšší propustnost vzhledem k tomu, že funguje čtení a zápisu, zatímco předchozích měsíců s nižší propustnost od slouží jenom čtení.

### <a name="partitioning-and-multi-tenancy"></a>Vytváření oddílů a víceklientské prostředí
Pokud implementujete víceklientské aplikace pomocí Cosmos DB, existují dva oblíbených vzory – jeden oddíl klíč každého klienta a jednoho kontejneru typu každého klienta. Zde jsou hello výhody a nevýhody pro každou:

* Jeden klíč oddílu každého klienta: V tomto modelu, klienti jsou společně umístěná v rámci jednoho kontejneru. Ale dotazy a vložení pro položky v rámci jednoho klienta je možné provádět proti jeden oddíl. Můžete také implementovat logiku transakcí mezi všechny položky v rámci klienta. Vzhledem k tomu, že více klientů sdílet kontejner, můžete uložit náklady na úložiště a propustnost sdružování prostředků pro klienty v rámci jednoho kontejneru spíše než zřizování navíc rezervou pro každého klienta. Nevýhodou Hello je nemají izolaci výkonu každého klienta. Zvýšení výkonu nebo propustnost použít toohello celého kontejneru vs cílové zvyšuje pro klienty.
* Jeden kontejner každého klienta: vlastní kontejner má každý klient. V tomto modelu je možné rezervovat výkonu každého klienta. Tento model je s Cosmos DB nový zajišťování cenový model pro víceklientské aplikace cenově výhodnější s několika klienty.

Také můžete použít kombinaci/vrstvené přístup, collocates malé klientů a migraci větší klienty tootheir vlastní kontejner.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme Přehled poskytuje přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB. 

* Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)
* Další informace o [globální distribuce v Azure Cosmos DB](distribute-data-globally.md)



