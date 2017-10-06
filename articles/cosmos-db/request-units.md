---
title: aaaRequest jednotky & odhadnout propustnost - Azure Cosmos DB | Microsoft Docs
description: "Další informace o tom, jak toounderstand, zadejte a odhadnout požadavky na jednotky žádosti v Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a>Požadované jednotky v Azure Cosmos DB
Nyní k dispozici: Azure Cosmos DB [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner). Další informace v [odhadnout, musí vaše propustnost](request-units.md#estimating-throughput-needs).

![Propustnost kalkulačky][5]

## <a name="introduction"></a>Úvod
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je globálně distribuované databáze více modelu společnosti Microsoft. S Azure DB Cosmos nemáte toorent virtuálních počítačů, nasazování softwaru nebo monitorování databází. Azure Cosmos DB je provozována a průběžně monitorovat pomocí Microsoft nejvyšší technici toodeliver world třída data dostupnosti, výkonu a ochrany. Přistupujete k datům pomocí rozhraní API podle svého výběru jako [DocumentDB SQL](documentdb-sql-query.md) (dokumentu), MongoDB (dokumentu), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (klíč hodnota), a [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graf) jsou všechny nativně podporuje. Měna Hello Azure Cosmos databáze je hello jednotka žádosti (RU). S RUs není nutné tooreserve kapacity pro čtení a zápis a zřizovat procesoru, paměti a procesorů.

Azure Cosmos DB podporuje několik rozhraní API s různé operace, od jednoduchého čte a zapisuje toocomplex dotazy grafu. Vzhledem k tomu, že ne všechny požadavky jsou stejné, jsou přiřazeny normalizovaný objemu **požadované jednotky** podle hello objem výpočtů požadované tooserve hello požadavku. Hello počet jednotek žádosti operace je deterministická, a můžete sledovat hello počet jednotek žádosti spotřebovávají všechny operace v Azure Cosmos DB prostřednictvím hlavičky odpovědi. 

tooprovide předvídatelný výkon, je nutné tooreserve propustnost v jednotkách 100 RU za sekundu. 

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:  

* Jaké jsou požadované jednotky a požádat o poplatky?
* Jak určit kapacitu jednotky žádosti pro kolekci?
* Jak odhadnout, že je jednotka žádosti Moje aplikace?
* Co se stane, když I překročit kapacitu jednotky žádosti pro kolekci?

Jak Azure Cosmos DB je více modelu databáze, je důležité toonote, že bude označujeme tooa kolekce či dokumentu pro dokument rozhraní API, grafu nebo uzel pro graf rozhraní API a tabulka/entity pro tabulku rozhraní API. Propustnost tohoto dokumentu jsme se generalize toohello koncepty kontejneru nebo položky.

## <a name="request-units-and-request-charges"></a>Jednotek žádosti a poplatky požadavku
Azure Cosmos DB poskytuje rychlé, předvídatelný výkon pomocí *rezervování* toosatisfy prostředky musí propustnost vaší aplikace.  Vzhledem k aplikaci načíst a přístup k vzory změny v čase, Azure Cosmos DB vám umožní zvýšení tooeasily nebo snížit množství hello aplikace k dispozici tooyour vyhrazenou propustností.

S Azure Cosmos databáze je zadána vyhrazenou propustností z hlediska jednotek žádosti zpracování za sekundu. Si můžete představit jednotek žádosti jako měnu propustnost, které jste *rezervovat* množství zaručit jednotek žádosti k dispozici tooyour aplikace na základě za sekundu.  Každé operace v Azure DB Cosmos - zápis dokumentu, provádění dotazu, aktualizace dokumentu - spotřebuje procesoru, paměti a procesorů.  To znamená, každou operaci způsobuje *požadavku poplatků*, vyjádřeného v *požadované jednotky*.  Pochopení hello faktory, což ovlivňuje poplatky jednotek žádosti, společně s požadavky na propustnost vaší aplikace, umožňuje vám toorun aplikaci jako efektivně možné náklady. Průzkumník dotazů Hello je také jádro hello tootest skvělý nástroj dotazu.

Doporučujeme začít sledování hello následující video, kde vysvětluje Aravind Ramachandran jednotek žádosti a předvídatelného výkonu s Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>Určení požadavku jednotka kapacity v Azure Cosmos DB
Při spouštění novou kolekci, tabulka nebo grafu, můžete zadat číslo hello jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené. Na základě hello zřízené propustnosti, Azure Cosmos DB přiděluje fyzické oddíly toohost kolekce a rozdělení/rebalances dat napříč oddíly ho s růstem.

Azure Cosmos DB vyžaduje že toobe klíče oddílu zadat, když je kolekce s 2 500 jednotek žádosti přiděleným nebo vyšší. Klíč oddílu je taky požadované tooscale propustnost vaší kolekce nad rámec odpovídající 2500 jednotek žádosti v budoucnu hello. Proto důrazně doporučujeme tooconfigure [klíč oddílu](partition-data.md) při vytváření kontejneru bez ohledu na počáteční propustnosti. Vzhledem k tomu, že vaše data mít toobe rozdělit do několika oddílů, je nutné toopick klíč oddílu, který má vysokou kardinalitou (100 toomillions jedinečných hodnot), aby kolekce, tabulka nebo graf a žádostí je možné rozšířit jednotně pomocí Azure Cosmos DB. 

> [!NOTE]
> Klíč oddílu je logické hranice a není fyzický jeden. Proto není nutné toolimit hello počet hodnoty klíče jedinečné oddílu. Ve skutečnosti je lepší toohave více jedinečných oddílu hodnoty klíče menší, než databázi Cosmos Azure má další možnosti vyrovnávání zatížení.

Zde je fragment kódu pro vytvoření kolekce s 3 000 požadavek hello jednotek na druhý pomocí .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos DB funguje na rezervace modelu na propustnost. To znamená, že se vám účtuje hello množství propustnost *vyhrazené*, bez ohledu na to, kolik z této propustnost je aktivně *používá*. Jako aplikace zatížení, data a využití vzory změn je možné snadno škálovat nahoru a dolů hello množství vyhrazené RUs prostřednictvím sady SDK nebo pomocí hello [portálu Azure](https://portal.azure.com).

Každý kolekce a tabulka/grafika jsou namapované tooan `Offer` prostředků v Azure DB Cosmos, který má metadata o hello zřízené propustnosti. Vyhledávání hello odpovídající prostředek nabídka pro kontejner a poté aktualizace pomocí hello novou hodnotu propustnosti, můžete změnit hello přidělené propustnost. Zde je fragment kódu pro změnu hello propustnost kolekce too5, hello 000 jednotek žádosti za druhé pomocí .NET SDK:

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

Neexistuje žádný dopad toohello dostupnosti vaší kontejneru při změně hello propustnost. Nové vyhrazenou propustností hello je obvykle efektivní během několika sekund na aplikace hello nové propustnosti.

## <a name="request-unit-considerations"></a>Aspekty jednotek žádosti
Při odhadování hello počet tooreserve jednotek žádosti pro váš kontejner Azure Cosmos DB, je důležité tootake hello v úvahu následující proměnné:

* **Velikost položky**. Jak roste množství hello jednotek použití tooread nebo zapisovat data hello se taky zvýší.
* **Počet vlastností položky**. Za předpokladu, že výchozí indexování všech vlastností, toowrite hello jednotek použití, které dokumentu nebo uzel nebo ntity zvýší jako zvyšuje počet vlastnost hello.
* **Konzistenci dat**. Při použití úrovně konzistence dat silného nebo typu s ohraničenou Prošlostí, bude další jednotky spotřebované tooread položky.
* **Indexované vlastnosti**. Zásadu indexu na každý kontejner určuje vlastnosti, které jsou uloženy ve výchozím nastavení. Omezením hello počet indexované vlastnosti nebo povolením Opožděné indexování můžete snížit spotřebu jednotky vaší žádosti.
* **Indexování dokumentů**. Ve výchozím nastavení je každá položka automaticky indexovaný bude využívat méně jednotek žádosti, pokud si zvolíte není tooindex některé položky.
* **Dotaz vzory**. složitost Hello dotazu má dopad na tom, kolik jednotek žádosti se spotřebovávají pro operace. Hello počet predikáty, povaha hello predikáty, projekce, počet UDF a velikost hello hello zdroje dat sady všechny ovlivnit hello náklady na dotaz operace.
* **Použití skriptu**.  Stejně jako u dotazů, využívat jednotek žádosti podle složitosti hello operací prováděných na hello uložených procedur a aktivačních událostí. Když budete vyvíjet aplikace, zkontrolujte hello požadavek poplatků záhlaví toobetter pochopit, jak každou operaci spotřebovává požadavek jednotky kapacity.

## <a name="estimating-throughput-needs"></a>Odhad potřeb propustnost
Jednotka žádosti je normalizovaný míru náklady na zpracování požadavku. Jednotka jedné žádosti představuje hello zpracování požadovaná kapacita tooread (prostřednictvím id nebo vlastní odkaz) jeden 1KB položky skládající se z 10 jedinečnou vlastnost hodnot (s výjimkou vlastnosti systému). Žádost o toocreate (Vložit), nahraďte nebo odstranit hello stejné položky spotřebuje další zpracování ze služby hello a tak další požadované jednotky.   

> [!NOTE]
> Hello účaří požadavků 1 jednotka pro 1KB položky odpovídá tooa jednoduché získat vlastní odkaz nebo id položky hello.
> 
> 

Zde je tabulku, která ukazuje, kolik požadavků například jednotky tooprovision na tři různé položky velikosti (1KB, 4KB a 64KB) a na dvou různých výkonu úrovních (500 čtení za sekundu + 100 zápisů za sekundu a 500 čtení za sekundu + 500 zápisů za sekundu). Hello konzistenci dat byla nakonfigurována v relaci a hello zásady indexování byla nastavena tooNone.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Velikost položky</strong></p></td>
            <td valign="top"><p><strong>Čtení za sekundu</strong></p></td>
            <td valign="top"><p><strong>Zápisů za sekundu</strong></p></td>
            <td valign="top"><p><strong>Jednotky žádostí</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a>Použití kalkulačky jednotek žádosti hello
Zákazníci toohelp jemné vyladění jejich odhady propustnost, je web, na základě [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner) požadavky jednotky toohelp odhad hello požadavku pro typická operace, včetně:

* Vytvoří položku (zápisy)
* Čtení položky
* Odstranění položky
* Aktualizace položky

Nástroj Hello zahrnuje taky podporu odhadnout požadavky na úložiště dat podle hello ukázkové položky, které zadáte.

Pomocí nástroje hello je jednoduchý:

1. Nahrajte jednu nebo více reprezentativní položek.
   
    ![Nahrát položky toohello požadavek jednotky kalkulačky][2]
2. požadavky na úložiště dat tooestimate, zadejte hello celkový počet položek očekáváte, že toostore.
3. Zadejte hello počet položek, které vytvářet, číst, aktualizovat a odstranit operations vyžadují (na základě za sekundu). tooestimate hello požadavek jednotky poplatky položky aktualizace operací, nahrajte kopii hello ukázkové položky z kroku 1 výše, zahrnuje typické pole aktualizace.  Například pokud položku aktualizace obvykle upravit dvě vlastnosti s názvem lastLogin a userVisits a pak položku ukázka hello jednoduše zkopírovat, aktualizujte hello hodnoty pro tyto dvě vlastnosti a nahrajte hello zkopírovat položky.
   
    ![Zadejte požadavky na propustnost v kalkulačky jednotek žádosti hello][3]
4. Klikněte na tlačítko Vypočítat a prozkoumejte výsledky hello.
   
    ![Žádosti o výsledky kalkulačky jednotky][4]

> [!NOTE]
> Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, nahrajte vzorek každého *typ* z toohello typické položky nástroje a potom vypočítat hello výsledky.
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a>Použít hlavičku odpovědi hello Azure Cosmos DB požadavek zdarma
Každou odpověď z hello služby Azure Cosmos DB obsahuje vlastní hlavičky (`x-ms-request-charge`) obsahující jednotek žádosti hello využité pro požadavek hello. Tuto hlavičku je také přístupné prostřednictvím hello Azure Cosmos DB sady SDK. RequestCharge v hello .NET SDK, je vlastnost hello ResourceResponse objektu.  Pro dotazy hello Průzkumníka dotazů DB Cosmos Azure v hello portál Azure poskytuje informace poplatků požadavku pro spuštění dotazů.

![Zkoumání RU poplatky v hello Průzkumníka dotazů][1]

Myslete na to, jednu metodu k odhadování hello množství vyhrazenou propustností požadované aplikací je toorecord hello požadavek jednotky poplatků přidružené spuštěným typických operací pro položku reprezentativní používá vaše aplikace a potom odhad hello počet operací předpokládáte provádění každou sekundu.  Být jisti toomeasure a zahrnují typické dotazy a také při použití skriptu Azure Cosmos DB.

> [!NOTE]
> Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, potom si poznamenejte hello použít operaci požadavku jednotky poplatků spojených s jednotlivými *typ* typické položky.
> 
> 

Například:

1. Zaznamenejte poplatků jednotky hello žádost o vytvoření (vkládání) typické položky. 
2. Záznamů hello poplatků jednotky žádosti o čtení typické položky.
3. Záznamů hello požadavek jednotky poplatků aktualizace typické položky.
4. Záznamů hello požadavek jednotky poplatků typické, běžné položky dotazů.
5. Záznamů hello požadavek jednotky poplatků vlastních skriptů (uložené procedury, triggery, funkce definované uživatelem) využít aplikace hello
6. Vypočítejte hello požadované žádosti že jednotky dané hello odhadovaný počet operací předpokládáte toorun každou sekundu.

### <a id="GetLastRequestStatistics"></a>Použití rozhraní API pro příkaz GetLastRequestStatistics pro MongoDB
Rozhraní API pro MongoDB podporuje vlastního příkazu *getLastRequestStatistics*, pro načítání hello požadavek zdarma pro zadané operace.

Například v hello prostředí Mongo, hello operaci provést, které chcete tooverify hello požadavek zdarma pro.
```
> db.sample.find()
```

Potom spusťte příkaz hello *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Myslete na to, jednu metodu k odhadování hello množství vyhrazenou propustností požadované aplikací je toorecord hello požadavek jednotky poplatků přidružené spuštěným typických operací pro položku reprezentativní používá vaše aplikace a potom odhad hello počet operací předpokládáte provádění každou sekundu.

> [!NOTE]
> Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, potom si poznamenejte hello použít operaci požadavku jednotky poplatků spojených s jednotlivými *typ* typické položky.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>Použít rozhraní API pro portál metriky pro MongoDB
Nejjednodušší způsob, jak tooget dobrý odhad požadavek jednotky poplatky za vaše rozhraní API pro databázi MongoDB je toouse hello Hello [portál Azure](https://portal.azure.com) metriky. S hello *počet požadavků, které* a *požadavek poplatků* grafy, můžete získat odhad, kolik jednotek žádosti je každé operace využívání a kolik jednotek žádosti budou využívat relativní tooone jiné.

![Rozhraní API pro MongoDB portálu metriky][6]

## <a name="a-request-unit-estimation-example"></a>V příkladu odhad jednotek žádosti
Vezměte v úvahu následující ~ 1KB dokumentu hello:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Dokumenty jsou minifikovaný v Azure Cosmos DB, takže hello systému vypočítat velikost hello dokumentu výše je něco menší než 1 KB.
> 
> 

Hello následující tabulka uvádí přibližnou požadavek jednotky poplatky za typických operací u této položky (hello přibližnou požadavek jednotky poplatků předpokládá úroveň konzistence účtu hello nastavena příliš "Relace" a že jsou všechny položky automaticky indexovány):

| Operace | Žádost o jednotky zdarma |
| --- | --- |
| Vytvoření položky |~ 15 RU |
| Čtení položky |~ 1 RU |
| Dotaz položky podle id |~2.5 RU |

Kromě toho tato tabulka ukazuje přibližnou požadavek jednotky poplatky za typické dotazy používané v aplikaci hello:

| Dotaz | Žádost o jednotky zdarma | počet vrácených položek |
| --- | --- | --- |
| Vyberte jídlo podle id |~2.5 RU |1 |
| Vyberte potravin podle výrobce |~ 7 RU |7 |
| Vyberte skupiny jídlo a pořadí podle váhy |~ 70 RU |100 |
| Vyberte nejvyšší 10 potravin ve skupině jídlo |~ 10 RU |10 |

> [!NOTE]
> Poplatky za RU lišit v závislosti na hello počet vrácených položek.
> 
> 

Pomocí těchto informací jsme odhadnout hello RU požadavky pro toto číslo zadané aplikace hello operací a dotazy že Očekáváme, že za sekundu:

| Operace nebo dotazu | Očekávaný počet za sekundu | Požadované RUs |
| --- | --- | --- |
| Vytvoření položky |10 |150 |
| Čtení položky |100 |100 |
| Vyberte potravin podle výrobce |25 |175 |
| Vyberte jídlo skupinou |10 |700 |
| Vyberte nejvyšší 10 |15 |Celkem 150 |

V takovém případě Očekáváme, že požadavek průměrnou propustností 1,275 RU/s.  Zaokrouhlení nahoru toohello nejbližší 100, jsme by zřídit 1 300 RU/s pro kolekci této aplikace.

## <a id="RequestRateTooLarge"></a>Překročení omezení vyhrazenou propustností v Azure Cosmos DB
Odvolat, že spotřeba jednotek žádosti budou vyhodnocené jako za sekundu Pokud hello rozpočtu je prázdný. Pro aplikace, které překračují hello míra jednotky zřízené požadavku pro kontejner, požadavků, že kolekce toothat budou omezeny, dokud hello rychlost klesne pod úroveň hello vyhrazena. Když dojde omezení, hello server se ukončí ho preventivně hello žádost s RequestRateTooLargeException (kód stavu HTTP 429) a návratové hello hlavičky x-ms opakování za ms informující o hello množství času v milisekundách, která hello uživatele čekat, než neúspěšných hello požadavek.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Pokud používáte hello .NET klienta SDK a LINQ dotazů a pak většinu času hello toodeal s výjimku, máte nikdy jako hello aktuální verzi hello .NET klienta SDK implicitně zachytí této odpovědi, respektuje hello zadaný server opakovat po záhlaví, a žádost o hello opakování. Pokud váš účet je současně přistupuje více klientů, hello další pokus bude úspěšné.

Pokud máte více než jednoho klienta kumulativně operační vyšší rychlost požadavků hello, hello výchozí chování opakování nemusí stačit a hello klienta vyvolá výjimku DocumentClientException s aplikaci toohello 429 kódu stavu. V případech, jako je tato zvažte zpracování logiky aplikace chyba zpracování rutiny nebo zvýšení hello vyhrazenou propustností hello kontejneru a postup pro opakované.

## <a id="RequestRateTooLargeAPIforMongoDB"></a>Překročení omezení vyhrazenou propustností v rozhraní API pro MongoDB
Aplikace, které překračují jednotek žádosti hello zřízené pro kolekci budou omezeny, dokud hello rychlost klesne pod úroveň hello vyhrazena. Když dojde omezení, back-end hello se ukončí ho preventivně hello žádosti s *16500* kód chyby - *příliš mnoho požadavků*. Ve výchozím nastavení, bude rozhraní API pro MongoDB automaticky opakovat too10 časů před vrácením *příliš mnoho požadavků* kód chyby. Pokud se zobrazuje řada *příliš mnoho požadavků* kódy chyb, můžete zvážit buď přidání opakování chování vaší aplikace chyba zpracování rutiny nebo [zvýšení hello vyhrazenou propustností pro kolekci hello](set-throughput.md).

## <a name="next-steps"></a>Další kroky
toolearn Další informace o vyhrazenou propustností s databází Azure Cosmos DB těchto materiálech:

* [Azure Cosmos DB ceny](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Segmentace dat v Azure Cosmos DB](partition-data.md)

toolearn Další informace o databázi Cosmos Azure, najdete v části hello Azure Cosmos DB [dokumentaci](https://azure.microsoft.com/documentation/services/cosmos-db/). 

tooget začít s škálování a výkon testování pomocí Azure Cosmos DB, najdete v části [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md).

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
