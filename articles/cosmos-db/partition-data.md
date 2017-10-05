---
title: "Vytváření oddílů a horizontální škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak nakonfigurovat, vytváření oddílů a oddílu klíče a jak vybrat klíč správné oddílu pro vaši aplikaci."
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
ms.openlocfilehash: e2d2847276e553d7511241ff323c3e00aad8e5c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-partition-and-scale-in-azure-cosmos-db"></a>Vytvoření oddílů a škálování v Azure Cosmos DB

[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je globální databáze distribuované, více modelu služba navržená tak, aby vám pomohou dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem. Tento článek obsahuje přehled jak rozdělení funguje u všech datových modelech v Azure Cosmos DB a popisuje konfiguraci kontejnery Azure Cosmos DB efektivní škálování vašich aplikací.

Dělení a klíče oddílů jsou také popsané v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Shireesh Thota pátek.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Vytváření oddílů v Azure Cosmos DB
V Azure DB Cosmos můžete ukládat a zadávat dotazy na data bez schémat s pořadí milisekundu odezvy v jakémkoli měřítku. Cosmos DB poskytuje kontejnery pro ukládání dat volat **kolekce (pro dokument), grafy nebo tabulky**. Kontejnery jsou logické prostředky a může mít rozsah jeden nebo více fyzických oddílů nebo serverů. Počet oddílů je dáno DB Cosmos na základě velikosti úložiště a zřízené propustnosti kontejneru. Každý oddíl v Cosmos DB má pevně stanovený objem zálohovaná na SSD úložiště s ním spojená a se replikují pro vysokou dostupnost. Oddíl správy je plně spravovat Azure Cosmos DB a není nutné zapsat složitý kód nebo spravovat vaše oddíly. Kontejnery DB cosmos neomezená z hlediska úložiště a propustnosti. 

![vodorovné](./media/introduction/azure-cosmos-db-partitioning.png) 

Vytváření oddílů je transparentní do vaší aplikace. Cosmos DB podporuje rychlé čtení a zápisy, dotazy, transakční logiku, úrovně konzistence a řízení přístupu podrobných prostřednictvím metody nebo rozhraní API pro jediný kontejner prostředků. Služba zpracovává distribuce data mezi oddílů a směrování požadavků na dotazy do správné oddílu. 

Jak funguje dělení Každá položka musí mít klíč oddílu a klíč řádku, které jeho jednoznačné identifikaci. Klíč oddílu funguje jako logický oddíl pro vaše data a poskytne Cosmos DB přirozené hranice pro distribuci dat mezi oddílů. Stručně řečeno zde je Princip vytváření oddílů v Azure Cosmos DB:

* Zřídit Cosmos DB kontejner s `T` propustnost požadavky/s
* Na pozadí Cosmos DB zřídí oddíly, které jsou potřebné k obsluze `T` požadavky/s. Pokud `T` je vyšší než maximální propustnost na oddíl `t`, pak Cosmos DB zřizuje `N`  =  `T/t` oddíly
* Cosmos DB přiděluje místo na klíče oddílu klíče hash rovnoměrně napříč `N` oddíly. Ano každý oddíl (fyzickém oddílu) hostitelem hodnoty klíče oddílu 1-N (logické oddíly)
* Při fyzickém oddílu `p` dosáhnou limitu úložiště Cosmos DB bezproblémově rozdělí `p` do dvou nových oddílů `p1` a `p2` a distribuuje hodnoty odpovídající přibližně poloviční klíče pro každou nadefinovaných oddílů. Toto rozdělení operace je pro vaše aplikace skrytá.
* Podobně když zřídit propustnost vyšší než `t*N` propustnost, Cosmos DB rozdělí jeden nebo více oddíly mohou podporovat vyšší propustnost

Sémantika pro klíče oddílů je mírně odlišný tak, aby odpovídaly sémantika každé rozhraní API, jak je znázorněno v následující tabulce:

| Rozhraní API | Klíč oddílu | Klíč řádku |
| --- | --- | --- |
| DocumentDB | Cesta klíče vlastní oddíl | Pevná`id` | 
| MongoDB | vlastní horizontálních klíč  | Pevná`_id` | 
| Graph | klíčovou vlastností vlastní oddíl | Pevná`id` | 
| Table | Pevná`PartitionKey` | Pevná`RowKey` | 

Cosmos DB používá algoritmus HMAC rozdělení do oddílů. Při zápisu položky databáze Cosmos hashuje hodnotu klíče oddílu a hash výsledek použít k určení oddíl, který k uložení položky v. Cosmos DB ukládá všechny položky se stejným klíčem oddílu v jednom fyzickém oddílu. Volba klíč oddílu je důležité rozhodnutí, která je nutné provést v době návrhu. Je třeba vybrat název vlastnosti, která má široký rozsah hodnot a má i přístupové vzorce.

> [!NOTE]
> Je vhodné mít klíč oddílu s mnoha jedinečných hodnot (100s-1000s minimálně).
>

Kontejnery Azure Cosmos DB se dá vytvořit jako "Pevná" nebo "neomezená." Kontejnery pevné velikosti mají maximální limit 10 GB a propustnost 10 000 RU/s. Některé rozhraní API umožňují klíč oddílu vynechává kontejnerů s pevnou velikostí. Chcete-li vytvořit kontejner jako neomezená, musíte zadat minimální propustnost 2 500 RU/s.

## <a name="partitioning-and-provisioned-throughput"></a>Vytváření oddílů a zřízené propustnosti
Cosmos DB je určená pro předvídatelný výkon. Když vytvoříte kontejner, můžete vyhradit propustnost z hlediska  **[požadované jednotky](request-units.md) (RU) za sekundu s potenciální rozšíření pro RU za minutu**. Každý požadavek je přiřazený nákladů jednotky žádosti, která je úměrné množství systémové prostředky jako procesoru, paměti a spotřebovávají operaci vstupně-výstupní operace. Čtení 1 KB dokumentu s konzistence typu relace spotřebuje jednu jednotku požadavku. Pro čtení je 1 RU bez ohledu na počet položek, které jsou uložené nebo počet souběžných požadavků, které se spouští ve stejnou dobu. Položky větší vyžadují vyšší jednotek žádosti v závislosti na velikosti. Pokud znáte velikost vaší entity a počet čtení, které budete potřebovat k podpoře pro aplikaci, můžete zřídit přesné množství propustnost požadované pro vaše aplikace je vyžaduje čtení. 

> [!NOTE]
> K dosažení úplné propustnosti kontejneru, je nutné zvolit klíč oddílu, který umožňuje rovnoměrně rozdělit požadavky mezi některé hodnoty klíče jedinečné oddílu.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-the-azure-cosmos-db-apis"></a>Práce s Azure Cosmos DB rozhraní API
Portál Azure nebo rozhraní příkazového řádku Azure slouží k vytvoření kontejnerů a škálování je kdykoli. Tato část ukazuje způsob vytvoření kontejnerů a zadejte definici klíče propustnost a oddílu v každé z podporovaných rozhraní API.

### <a name="documentdb-api"></a>Rozhraní DocumentDB API
Následující příklad ukazuje, jak vytvořit kontejner (kolekce) pomocí rozhraní API pro DocumentDB. Můžete najít další podrobnosti v [rozdělení do oddílů s rozhraním API DocumentDB](partition-data.md).

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

Můžete si přečíst k položky (dokument) pomocí `GET` metoda v rozhraní API REST nebo pomocí `ReadDocumentAsync` v jednom ze sady SDK.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>Rozhraní MongoDB API
S rozhraním API pro MongoDB můžete vytvořit kolekci horizontálně dělené prostřednictvím vaše oblíbené nástroje, ovladače nebo sady SDK. V tomto příkladu používáme prostředí Mongo pro vytvoření kolekce.

V prostředí Mongo:

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

Pomocí rozhraní API tabulky je určit propustnost pro tabulky v konfiguraci appSettings pro vaši aplikaci:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Potom můžete vytvořit tabulku pomocí Azure Table storage SDK. Klíč oddílu je vytvořena implicitně jako `PartitionKey` hodnotu. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

Můžete načíst jednu entitu pomocí následující fragment kódu:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
V tématu [vývoj s rozhraním API pro tabulku](tutorial-develop-table-dotnet.md) další podrobnosti.

### <a name="graph-api"></a>Graph API

Rozhraní Graph API musíte použít portál Azure nebo rozhraní příkazového řádku k vytvoření kontejnerů. Alternativně vzhledem k tomu, že Azure Cosmos DB je více modelu, můžete další modely vytvořit a škálovat vaše kontejneru grafu.

Může číst všechny vrchol nebo Microsoft edge v Gremlin klíč oddílu a id. Graf s oblasti ("USA") jako klíč oddílu a "Seattle" jako klíč řádku, například můžete najít vrchol pomocí následující syntaxe:

```
g.V(['USA', 'Seattle'])
```

Stejné jako u okraje, můžete odkazovat pomocí klíč oddílu a klíč řádku okraj.

```
g.E(['USA', 'I5'])
```

V tématu [Gremlin podpora systému Cosmos DB](gremlin-support.md) další podrobnosti.


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>Návrh a vytváření oddílů
Efektivní škálování s Azure Cosmos DB, musíte vybrat vhodným klíčem oddílu, při vytváření vašeho kontejneru. Existují dva klíčové faktory pro výběr klíč oddílu:

* **Hranice pro dotaz a transakce**: zvoleného klíč oddílu by měl vyvážit potřeba povolit používání transakce proti požadavku, distribuovat vaší entity napříč více klíčů oddílů zajistit škálovatelné řešení. V jedné extreme stejným klíčem oddílu můžete nastavit pro všechny položky, ale to může omezit škálovatelnost řešení. V jiných extreme může přiřadit oddílu jedinečný klíč pro každou položku, která by byla vysoce škálovatelné, ale by bránily použití transakcí křížové dokumentu prostřednictvím uložených procedur a aktivačních událostí. Klíč ideální oddílu je jeden, který vám umožňuje používat efektivní dotazy a má dostatek mohutnost zajistit, že vaše řešení je škálovatelná. 
* **Žádné úložiště a výkon kritických bodů**: je důležité vybrat vlastnost, která umožňuje zápis být distribuován do různých jedinečných hodnot. Požadavky na stejným klíčem oddílu nesmí být delší než propustnost jednoho oddílu a jsou omezené. Proto je důležité vybrat klíč oddílu, který nemá za následek "aktivní body" v rámci vaší aplikace. Vzhledem k tomu, že všechna data pro jeden oddíl klíč musí být uložen v rámci oddílu, doporučujeme také Vyhněte se klíče oddílů, které mají velký objem dat pro stejnou hodnotu. 

Podívejme se na několik reálných scénářů a klíče dobrý oddílů pro každou:
* Pokud jste implementace back-end profilu uživatele, ID uživatele je vhodná pro klíč oddílu.
* Pokud ukládáte data IoT například stavu zařízení, je ID zařízení dobrou volbou pro klíč oddílu.
* Pokud používáte Azure Cosmos DB pro protokolování data časové řady, název hostitele nebo proces ID je vhodná pro klíč oddílu.
* Pokud máte víceklientské architektuře, ID klienta je vhodná pro klíč oddílu.

V některých případech jako IoT a uživatelské profily použití, klíč oddílu může být stejný jako vaše id (klíč dokumentu). V jiných jako data časové řady může mít klíč oddílu, který se liší od id.

### <a name="partitioning-and-loggingtime-series-data"></a>Vytváření oddílů a protokolování nebo časových řad dat
Mezi běžné případy použití Cosmos databáze je pro protokolování a telemetrie. Je důležité vybrat vhodným klíčem oddílu, protože může být nutné obrovské objemy dat pro čtení a zápis. Výběr závisí na čtení a zápisu sazby a typy dotazů, které chcete spustit. Tady jsou některé tipy, jak zvolit vhodným klíčem oddílu.

* Pokud váš případ použití zahrnuje malý počet zapíše pak pomocí souhrnu časové razítko, například datum jako klíč oddílu je dobrý způsob shromažďování po dlouhou dobu a potřeba dotazu rozsahy časová razítka a ostatní filtry. To umožňuje dotazu přes všechna data pro datum z jednoho oddílu. 
* Pokud vaše úlohy je napsán náročné, což je více obvyklé, používejte klíč oddílu, který není založen na časové razítko tak, aby Cosmos DB můžete rovnoměrně distribuovat zápisy mezi různé oddíly. Název hostitele, ID procesu, ID aktivity nebo jinou vlastnost s vysokou kardinalitou tady je vhodné použít. 
* Třetí přístup je hybridní, jeden kde máte více kontejnerů, jednu pro každý den/měsíc a klíč oddílu je podrobné vlastnosti, jako je název hostitele. Výhodou je, kterou můžete nastavit různé propustnost podle časový interval, například kontejner pro aktuální měsíc je opatřen vyšší propustnost vzhledem k tomu, že funguje čtení a zápisu, zatímco předchozích měsíců s nižší propustnost od jejich pouze sloužit čtení.

### <a name="partitioning-and-multi-tenancy"></a>Vytváření oddílů a víceklientské prostředí
Pokud implementujete víceklientské aplikace pomocí Cosmos DB, existují dva oblíbených vzory – jeden oddíl klíč každého klienta a jednoho kontejneru typu každého klienta. Zde jsou výhody a nevýhody pro každou:

* Jeden klíč oddílu každého klienta: V tomto modelu, klienti jsou společně umístěná v rámci jednoho kontejneru. Ale dotazy a vložení pro položky v rámci jednoho klienta je možné provádět proti jeden oddíl. Můžete také implementovat logiku transakcí mezi všechny položky v rámci klienta. Vzhledem k tomu, že více klientů sdílet kontejner, můžete uložit náklady na úložiště a propustnost sdružování prostředků pro klienty v rámci jednoho kontejneru spíše než zřizování navíc rezervou pro každého klienta. Nevýhodou je, nemají izolaci výkonu každého klienta. Zvýšení výkonu nebo propustnost platí pro celou kontejneru vs cílové zvyšuje pro klienty.
* Jeden kontejner každého klienta: vlastní kontejner má každý klient. V tomto modelu je možné rezervovat výkonu každého klienta. Tento model je s Cosmos DB nový zajišťování cenový model pro víceklientské aplikace cenově výhodnější s několika klienty.

Můžete také vrstvené nebo kombinaci přístup, collocates malé klientů a migraci větší klienty do svých vlastních kontejneru.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme Přehled poskytuje přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB. 

* Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)
* Další informace o [globální distribuce v Azure Cosmos DB](distribute-data-globally.md)



