---
title: "zásady indexování Cosmos DB aaaAzure | Microsoft Docs"
description: "Vysvětluje, jak indexování v Azure Cosmos DB. Zjistěte, jak tooconfigure a změňte hello zásady indexování pro automatické indexování a lepší výkon."
keywords: "jak indexování funguje, automatické indexování, indexování databáze"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 4f77b352b89382aa3352136038cb0e95c7588aac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Jak funguje Azure Cosmos DB data indexu?

Ve výchozím nastavení je indexovaný všechna data v Azure Cosmos DB. A mnoho zákazníků jsou radostí toolet Azure Cosmos DB automaticky zpracovávat všechny aspekty indexování, Azure Cosmos DB podporuje také zadat vlastní **indexování zásad** pro kolekce během vytváření. Indexování zásady v Azure Cosmos DB jsou pružnější a výkonnější než nabízené na jiných platformách, databáze, sekundární indexy, protože umožňují návrhu a přizpůsobit hello tvar hello index bez zachovávají flexibilitu schémat. toolearn indexování fungování v Azure Cosmos DB, je potřeba pochopit, můžete provést pomocí správy zásady indexování, jemně odstupňovaných kompromisy mezi režijní náklady na úložiště indexů, zápisu a propustnost dotazu a konzistence dotazu.  

V tomto článku jsme trvat zblízka na Azure Cosmos DB indexování zásady, jak můžete upravit zásady indexování a hello přidružené kompromis. 

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:

* Jak lze přepsat hello vlastnosti tooinclude nebo vyloučit z indexování?
* Konfigurování hello index pro případné aktualizace
* Konfigurování indexování dotazy Order By, nebo rozsah tooperform
* Jak vytvořit zásady indexování kolekce tooa změny?
* Jak porovnat úložiště a výkon různé zásady indexování?

## <a id="CustomizingIndexingPolicy"></a>Přizpůsobení zásady indexování hello kolekce
Vývojářům můžete přizpůsobit hello kompromis mezi úložiště, výkon zápisu nebo dotazů a konzistence dotazu přepsání hello výchozí zásady indexování na kolekci Azure Cosmos DB a konfigurací hello následující aspekty.

* **Včetně/vyloučení dokumenty a cesty do nebo z indexu**. Vývojářům můžete zvolit určitých dokumentů toobe vyloučit nebo součástí hello indexu v době hello vložení nebo nahrazení je toohello kolekce. Vývojářům můžete také zvolit tooinclude nebo také znám jako vyloučit některé vlastnosti JSON indexované na dokumentech, které jsou součástí indexu toobe cesty (včetně zástupné znaky).
* **Konfigurace různé typy indexu**. Pro každou z cest hello zahrnuté vývojáři můžete taky zadat typ hello indexu, které vyžadují v kolekci na základě jejich dat a očekávané dotazu úlohy a hello číselný či řetězce "přesnost" pro každou z cest.
* **Konfigurace režimů aktualizace indexu**. Azure Cosmos DB podporuje tři indexování režimy, které je možné nakonfigurovat přes hello indexování zásady na kolekci Azure Cosmos DB: konzistentní, Lazy a None. 

Dobrý den, jak následující fragment kódu ukazuje kód .NET tooset vlastní zásady indexování během vytváření hello kolekce. Zde jsme nastavit zásady hello s indexem rozsahu pro řetězce a čísla na maximální přesnost hello. Tato zásada umožňuje nám spouštět dotazy Order By na řetězce.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> Hello schéma JSON pro zásady indexování bylo změněno hello verze rozhraní REST API verze 2015-06-03 toosupport rozsah indexů pro řetězce. .NET SDK 1.2.0 a Java, Python a Node.js SDK 1.1.0 podporovat hello nové zásady schéma. Starší sady SDK použijte hello REST API verze 2015-04-08 a podporují starší schéma hello zásady indexování.
> 
> Ve výchozím nastavení Azure Cosmos DB indexuje všechny vlastnosti řetězce v rámci dokumenty konzistentně s indexem Hash a číselné vlastnosti s indexem rozsahu.  
> 
> 

### <a name="customizing-hello-indexing-policy-using-hello-portal"></a>Přizpůsobení zásady indexování hello pomocí portálu hello

Hello indexování zásad kolekce pomocí hello portálu Azure, můžete změnit. Otevřít vaše kolekce účtu Azure Cosmos DB v hello portál Azure, vyberte v hello levé navigační nabídce klikněte na položku **nastavení**a potom klikněte na **zásady indexování**. V hello **zásady indexování** okně změnit zásady indexování a pak klikněte na **OK** toosave změny. 

### <a id="indexing-modes"></a>Režimy indexování databáze
Azure Cosmos DB podporuje tři indexování režimů, které je možné nakonfigurovat přes hello indexování zásady na kolekci Azure Cosmos DB – konzistentní, Lazy a None.

**Konzistentní**: Pokud zásady Azure Cosmos DB kolekce je určený jako "konzistentní", hello dotazy v dané kolekci postupujte Azure Cosmos DB hello stejnou úroveň konzistence jsou zadány pro hello bodu čtení (tj. silné, ohraničenou odolností, založenou relace nebo případné). Hello index se aktualizuje synchronně jako součást aktualizace dokumentu hello (tj. vložení, nahraďte, aktualizace a odstranění dokumentu v kolekci Azure Cosmos DB).  Konzistentní indexování podporuje konzistentní dotazy na možné snížení nákladů na hello zápisu propustnost. Toto snížení je funkce hello jedinečné cesty, které je třeba toobe indexované a hello "úroveň konzistence". Konzistentní indexování režimu je určená pro "zápisu rychle dotaz okamžitě" úlohy.

**Opožděné**: tooallow maximální dokumentu přijímání propustnost, kolekci Azure Cosmos DB se dá nakonfigurovat s opožděné konzistence; význam dotazy jsou nakonec byl konzistentní. Hello index je aktualizována asynchronně při kolekci Azure Cosmos DB je tichém tj. když kapacity propustnosti hello kolekce není plně využívané tooserve požadavků uživatele. Pro "ingestování teď dotaz později" procesy vyžadující přijímání nerušený dokumentu může být vhodné "opožděné" indexování režimu.

**Žádný**: kolekce označené jako index režim "Žádný" neobsahuje index s ním spojená. To se často používá, pokud se využívá Azure Cosmos DB jako klíč hodnota úložiště a dokumenty jsou dostupné jenom přes jejich vlastnost ID. 

> [!NOTE]
> Konfigurace hello indexování zásadou "Žádný" má hello vedlejším účinkem vyřadit všechny existující index. Použijte, pokud jsou přístupové vzorce vyžadují jenom "id" nebo "vlastního odkazu".
> 
> 

Následující ukázka zobrazit jak Hello vytvořte kolekci Azure Cosmos DB hello .NET SDK pomocí konzistentní automatické indexování na všechny vložení dokumentu.

Hello následující tabulka ukazuje hello konzistence pro dotazy na základě hello indexování režimu (konzistentní a Lazy) nakonfigurovaný pro hello kolekce a hello úroveň konzistence zadané pro hello dotazu požadavku. To platí tooqueries provedené pomocí žádné rozhraní - REST API sady SDK nebo v rámci uložené procedury a triggery. 

|Konzistence|Indexování režim: konzistentní|Indexování režim: opožděné|
|---|---|---|
|Silné|Silné|Nahodilé|
|Omezená neaktuálnost|Omezená neaktuálnost|Nahodilé|
|Relace|Relace|Nahodilé|
|Nahodilé|Nahodilé|Nahodilé|

Azure Cosmos DB vrátí chybu pro dotazy provedená na kolekce se žádné indexování režimu. Dotazy mohou být provedeny stále jako kontroly prostřednictvím hello explicitní `x-ms-documentdb-enable-scan` záhlaví v hello REST API nebo hello `EnableScanInQuery` požadavek pomocí možnost hello .NET SDK. Některé funkce dotazu jako ORDER nejsou podporovány jako kontroly s `EnableScanInQuery`.

Hello následující tabulka uvádí hello konzistence pro dotazy na základě hello indexování režimu (konzistentní, Lazy a None) Pokud je zadán parametr EnableScanInQuery.

|Konzistence|Indexování režim: konzistentní|Indexování režim: opožděné|Indexování režim: žádné|
|---|---|---|---|
|Silné|Silné|Nahodilé|Silné|
|Omezená neaktuálnost|Omezená neaktuálnost|Nahodilé|Omezená neaktuálnost|
|Relace|Relace|Nahodilé|Relace|
|Nahodilé|Nahodilé|Nahodilé|Nahodilé|

Hello následující zobrazit ukázkový kód jak vytvořte kolekci Azure Cosmos DB hello .NET SDK pomocí konzistentní indexování na všechny vložení dokumentu.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index cesty
Azure Cosmos DB modelů dokumentů JSON a hello indexu jako stromy a můžete tootune toopolicies pro cesty v rámci stromu hello. V rámci dokumenty můžete, které cesty musí být zahrnout nebo vyloučit z indexování. To můžete nabízet zápisu lepší výkon a dolní index úložiště pro scénáře při vzorům dotazů hello se ví, že předem.

Index cesty začínat s kořenem hello (/) a obvykle končit hello? operátor zástupných znaků, které označuje, že existuje více možných hodnot pro předponu hello. Například tooserve vybrat * z řady F kde F.familyName = "Rodinu", musí obsahovat cestu index pro /familyName/? v zásadách index hello kolekce.

Index cesty můžete také použít hello * zástupný znak operátor toospecify hello chování pro rekurzivní cesty s předponou hello. Například/datové části / * lze použít tooexclude vše pod vlastnost hello datovou část z indexování.

Zde jsou hello obecné vzory pro zadání cesty index:

| Cesta                | Popis nebo používají                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Výchozí cesta pro kolekci. Rekurzivní a použije toowhole stromu dokumentu.                                                                                                                                                                                                                                   |
| / prop /?             | Cesta index vyžaduje tooserve dotazy jako následující hello (s Hash nebo rozsah typy v uvedeném pořadí.):<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop > 5<br><br>Vyberte z kolekce c Order c.prop                                                                       |
| / prop / *             | Index cesta u všech cest v rámci zadaného popisku hello. Funguje s hello následující dotazy<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop.subprop > 5<br><br>Vyberte z kolekce c WHERE c.prop.subprop.nextprop = "hodnota"<br><br>Vyberte z kolekce c Order c.prop         |
| / props / [] /?         | Index cesta požadované tooserve iterace a dotazy na spojení pro pole skalárních hodnot jako ["a", "b", "c"]:<br><br>Vyberte označit z značky ve collection.props kde značky = "hodnota"<br><br>Vyberte značku z kolekce c spojení značky v c.props kde značky > 5                                                                         |
| [] /subprop/ /props/? | Cesta index vyžaduje tooserve iterace a dotazy na spojení pro pole objektů, jako jsou [{subprop: "a"}, {subprop: "b"}]:<br><br>Vyberte označit z značky ve collection.props kde tag.subprop = "hodnota"<br><br>Vyberte označit z kolekce c spojení značky v c.props kde tag.subprop = "hodnota"                                  |
| / prop/subprop /?     | Cesta index vyžaduje tooserve dotazy (s Hash nebo rozsah typy v uvedeném pořadí.):<br><br>Vyberte z kolekce c WHERE c.prop.subprop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Při nastavování vlastním indexu cesty, jsou požadované toospecify hello výchozí indexování pravidlo pro strom celý dokument hello odlišené speciální cesta hello "/ *". 
> 
> 

Hello následující příklad konfiguruje konkrétní cestu rozsah indexování a hodnotu vlastní přesnost 20 bajtů:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Index datové typy, typy a přesnosti
Teď, podívejte se na to, jak bylo toospecify cesty, podíváme se na možnosti hello můžeme použít tooconfigure hello zásady indexování pro cestu. Můžete určit jeden nebo více indexování definice pro každý cestu:

* Datový typ: **řetězec**, **číslo**, **bodu**, **mnohoúhelníku**, nebo **LineString** (může obsahovat pouze jednu položka pro každý datový typ za cesta)
* Index typu: **Hash** (dotazy na rovnost), **rozsah** (rovnosti, rozsah nebo Order By dotazy), nebo **Spatial** (prostorových dotazů) 
* Přesnost: Pro hash index to se liší od 1 too8 pro řetězce a čísla s výchozí jako 3. Tato hodnota pro rozsah index můžete mít hodnotu -1 (Maximální přesnost) a liší mezi 1-100 (Maximální přesnost) pro řetězec nebo číselné hodnoty.

#### <a name="index-kind"></a>Typ indexu
Azure Cosmos DB podporuje hodnoty Hash a rozsah typy index pro každou cestu (která můžete nakonfigurovat řetězce, čísla nebo obojí).

* **Hodnota hash** podporuje efektivní rovnosti a dotazy spojení. Pro většinu případy použití indexy hash není nutné vyšší přesností než hello výchozí hodnota 3 bajtů. Datový typ může být řetězec nebo číslo.
* **Rozsah** podporuje dotazy na rovnost efektivní, dotazy na rozsah (pomocí >, <>, =, < =,! =) a dotazy Order By. Dotazy Order By ve výchozím nastavení také vyžadují maximální index přesnost (-1). Datový typ může být řetězec nebo číslo.

Typ prostorového indexu hello Azure Cosmos DB také podporuje pro každou cestu, kterou lze zadat pro hello bodu, mnohoúhelníku nebo LineString datové typy. Hodnota Hello v zadané cestě hello musí být platný fragment GeoJSON jako `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Prostorové** podporuje efektivní prostorových (v rámci a vzdálenost) dotazy. Datový typ může být bodu, mnohoúhelníku nebo LineString.

> [!NOTE]
> Azure Cosmos DB podporuje automatické indexování bodů, mnohoúhelníky a LineStrings.
> 
> 

Zde jsou příklady dotazů, aby mohly být použité tooserve a hello podporované typy index:

| Typ indexu | Popis nebo používají                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hodnota hash       | Hodnoty hash přes/prop /? (nebo nebo) může být použité tooserve hello následující dotazy efektivně:<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Hodnota hash přes/props / [] /? (nebo / nebo/props /) můžou být použité tooserve hello následující dotazy efektivně:<br><br>Vyberte označit z kolekce c spojení značky v c.props kde značky = 5                                                                                                                       |
| rozsah      | Rozsah přes/prop /? (nebo nebo) může být použité tooserve hello následující dotazy efektivně:<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop > 5<br><br>Vyberte z kolekce c Order c.prop                                                                                                                                                                                                              |
| Spatial     | Rozsah přes/prop /? (nebo nebo) může být použité tooserve hello následující dotazy efektivně:<br><br>Vyberte z kolekce c<br><br>KDE ST_DISTANCE (c.prop, {"typ": "Bod", "coordinates": [0.0, 10.0]}) < 40<br><br>Vyberte z kolekce c kde ST_WITHIN(c.prop, {"type": "Polygon",...}) – s v bodech povoleno indexování<br><br>Vyberte z kolekce c kde ST_WITHIN({"type": "Point",...}, c.prop) – s indexování na mnohoúhelníky povoleno              |

Ve výchozím nastavení, je vrácena chyba pro dotazy s rozsahu operátory, jako > =, pokud neexistuje žádný index rozsah (z jakékoli přesnost) v pořadí toosignal, kontrolu může být nutné tooserve hello dotazu. Dotazy na rozsah můžete provést bez indexem rozsahu pomocí hlavičky x-ms-documentdb-enable kontroly hello v hello REST API nebo hello EnableScanInQuery žádost o možnost pomocí hello .NET SDK. Pokud jsou v hello dotaz, který Azure Cosmos DB můžete použít hello toofilter index pro všechny ostatní filtry, bude vrácena žádná chyba.

Hello stejná pravidla pro platí prostorových dotazů. Ve výchozím nastavení je vrácena chyba prostorových dotazů, pokud neexistuje žádné prostorový index, a neexistují žádné filtry, které se dají obsluhovat z indexu hello. Mohou být prováděny tak kontroly s využitím x-ms-documentdb-enable kontroly/EnableScanInQuery.

#### <a name="index-precision"></a>Index přesnost
Index přesnost umožňuje kompromis mezi režie index úložiště a výkon dotazů. Pro čísla doporučujeme používat hello výchozí přesnost konfigurace-1 ("maximální"). Vzhledem k tomu, že jsou čísla 8 bajtů ve formátu JSON, to je ekvivalentní tooa konfigurace 8 bajtů. Výběr na nižší hodnotu pro přesnost, jako je například 1-7, znamená, že hodnoty v některé oblasti mapy toohello stejné indexu položky. Proto se zmenší prostor úložiště index, ale může mít tooprocess další dokumenty a tedy z tohoto důvodu využívat další propustnost při provádění dotazu požadované jednotky.

Přesnost konfigurace indexu má více praktické aplikace s rozsahy řetězec. Vzhledem k tomu, že řetězce může být jakékoli libovolné délky, hello volbu hello index přesnost může ovlivnit výkon hello řetězec dotazy na rozsah a dopad hello množství prostoru úložiště index vyžaduje. Řetězec indexů rozsah můžete nakonfigurovat 1-100 nebo -1 ("maximální"). Pokud chcete tooperform Order dotazy na vlastnosti řetězce, je nutné zadat přesností-1 pro odpovídající cesty hello.

Prostorové indexy vždy použijte hello výchozí index přesnost pro všechny typy (body, LineStrings a mnohoúhelníky) a nelze přepsat. 

Hello následující příklad ukazuje, jak tooincrease hello přesnost pro rozsah indexy v kolekci pomocí hello .NET SDK. 

**Vytvořte kolekci s přesností vlastním indexu**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override hello default policy for Strings toorange indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB vrátí chybu, pokud dotaz používá Order By, ale nemá indexem rozsahu proti hello dotazované cestu s maximální přesnost hello. 
> 
> 

Podobně cesty můžete úplně vyloučit z indexování. Hello další příklad ukazuje, jak tooexclude celý oddíl hello dokumentů (také známa jako dílčí stromu) z indexování pomocí hello "*" zástupný znak.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Vyjádření výslovného souhlasu a zrušení indexování
Můžete zvolit, jestli chcete index tooautomatically hello kolekce všechny dokumenty. Ve výchozím nastavení, jsou automaticky indexovány všechny dokumenty, ale můžete tooturn ho vypnout. Když je vypnutý indexování, dokumenty jsou přístupné pouze prostřednictvím jejich odkazů na sebe sama nebo dotazy pomocí ID.

Pomocí automatické indexování, vypnutý, můžete stále selektivně přidat pouze konkrétní dokumenty toohello index. Naopak můžete nechat automatické indexování na a selektivně zvolte tooexclude pouze konkrétní dokumenty. Indexování zapnout nebo vypnout konfigurace jsou užitečné v případě, že máte jenom podmnožinu dokumentů, které je třeba toobe dotaz.

Například následující hello ukázkové ukazuje jak tooinclude a dokumentu explicitně pomocí hello [DocumentDB rozhraní API .NET SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) a hello [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) vlastnost.

    // If you want toooverride hello default collection behavior tooeither
    // exclude (or include) a Document from indexing,
    // use hello RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-hello-indexing-policy-of-a-collection"></a>Úprava zásady indexování hello kolekce
Azure Cosmos DB umožňuje toomake změny toohello zásady indexování kolekce v chodu hello. Změnu indexování zásady na kolekci Azure Cosmos DB může způsobit změnu tooa ve tvaru hello hello indexu včetně hello, které lze indexovat cesty, jejich přesnost, jakož i hello konzistence modelu hello indexu sám sebe. Změnu zásady indexování, proto vyžaduje efektivně transformaci hello staré indexu na novou. 

**Transformace indexu online**

![Indexování fungování – Azure Cosmos DB indexu online transformace](./media/indexing-policies/index-transformations.png)

Transformace indexu jsou vytvářeny online, což znamená, že dokumenty hello indexované podle zásad staré hello jsou transformovány efektivně na nové zásady hello **bez ovlivnění dostupnosti zápisu hello nebo zřízené propustnosti hello** z kolekce Hello. Hello konzistenci pro čtení a zápisu operace provedené pomocí hello REST API sady SDK nebo z v rámci uložené procedury a triggery není ovlivněná během index transformace. To znamená, že je žádná výkonu snížení nebo výpadek tooyour aplikací při provádění zásady indexování změnit.

Během doby hello, který index transformaci je průběh, jsou však nakonec byl konzistentní bez ohledu na to hello indexování konfiguraci režimu (konzistentní nebo Lazy) dotazy. To také platí tooqueries od všech rozhraní – rozhraní REST API sady SDK a v rámci uložené procedury a triggery. Stejně jako s Lazy indexování, index je provedena transformace asynchronně hello pozadí na replikách hello pomocí hello k výměně za chodu prostředky k dispozici pro danou repliku. 

Transformace indexu jsou také vytvářeny **na místě** (na místě), tj. Azure Cosmos DB nespravuje dvě kopie hello a swap hello staré indexem se s novým hello. To znamená, že žádné další místo na disku je požadováno, nebo využívat v kolekcích při provádění transformací index.

Když změníte zásady indexování, jak hello změny jsou použité toomove z hello původní index toohello nové jeden závisí primárně na hello indexování režimu konfigurace více, než hello jako ostatní hodnoty zahrnout nebo vyloučit cesty, typy index a přesnosti. Pokud vaše staré a nové zásady použít konzistentní indexování, Azure Cosmos DB provede transformaci indexu online. Jiné indexování změny zásad v režimu konzistentní indexování nelze použít, když probíhá hello transformace.

Můžete ale přesunout tooLazy nebo hodnotu None probíhá indexování režimu při transformaci. 

* Při přesunutí tooLazy hello index zásad změny efektivní okamžitě a Azure Cosmos DB spustí znovu vytvořit hello index asynchronně. 
* Když přesouváte tooNone, pak hello index vyřazen efektivní okamžitě. Přesunutí tooNone je užitečné, když chcete toocancel v průběhu transformaci a spusťte novou s jiný zásady indexování. 

Pokud používáte hello .NET SDK, můžete ji indexování změn zásad pomocí nového hello **ReplaceDocumentCollectionAsync** metoda a sledovat průběh procento hello transformace hello index pomocí hello  **IndexTransformationProgress** vlastnost odpověď z **ReadDocumentCollectionAsync** volání. Jiné sady SDK a hello REST API podporují ekvivalentní vlastnosti a metody pro provádění změn v zásadách indexování.

Zde je fragment kódu, který ukazuje, jak je toomodify kolekce indexování zásady z konzistentní indexování tooLazy režimu.

**Upravit zásady indexování z konzistentní tooLazy**

    // Switch toolazy indexing.
    Console.WriteLine("Changing from Default tooLazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Průběh hello transformaci index pomocí volání ReadDocumentCollectionAsync, například můžete zkontrolovat, jak je uvedeno níže.

**Sledovat průběh Index transformace**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Hello index pro kolekci můžete vyřadit přesunutím toohello žádné indexování režimu. To může být užitečné provozní nástroj toocancel transformaci v průběhu a okamžitě začít novou.

**Vyřadit hello index pro kolekci**

    // Switch toolazy indexing.
    Console.WriteLine("Dropping index by changing tootoohello None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Pokud by provedete indexování změny zásad tooyour Azure Cosmos DB kolekce? Hello následují hello nejběžnější případy použití:

* Poskytovat konzistentní výsledky při běžném provozu, ale vrátit toolazy indexování během hromadné importy dat
* Začít používat nové funkce indexování na aktuální databázi Cosmos Azure kolekce, například jako geoprostorové dotazování vyžadují typ prostorového indexu hello nebo Order By / řetězec rozsah dotazy, které vyžadují hello řetězec index typu rozsah
* Ruční toobe vlastnosti vyberte hello indexované a časem změnit
* Optimalizovat výkon dotazu tooimprove indexování přesnost nebo snižte spotřebovaného úložiště

> [!NOTE]
> toomodify indexování zásad pomocí ReplaceDocumentCollectionAsync, je třeba verze > = 1.3.0 hello .NET SDK
> 
> Pro index transformace toocomplete úspěšně, můžete musí zajistěte, aby dostatek volného místa k dispozici na kolekci hello. Pokud kolekce hello dosáhne kvóta úložiště, bude pozastavena hello index transformace. Index transformace se automaticky obnoví, jakmile prostor úložiště je k dispozici, například pokud odstraníte některé dokumenty.
> 
> 

## <a name="performance-tuning"></a>Ladění výkonu
Hello rozhraní API DocumentDB poskytují informace o metrik výkonu, jako je úložiště index hello používá a propustnost hello náklady (jednotek žádosti) pro všechny operace. Tyto informace lze použít toocompare různé zásady indexování a optimalizace výkonu.

toocheck hello úložiště kvóta a využití kolekce, žádost HEAD nebo GET spouštění hello kolekci prostředků a zkontrolovat hello x-ms požadavku kvóty a hlavičky x-ms požadavku využití hello. V hello .NET SDK, hello [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) a [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) vlastnosti v [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) obsahovat tyto odpovídající hodnoty .

     // Measure hello document size usage (which includes hello index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


toomeasure hello režii indexování na každou operaci zápisu (vytvoření, aktualizace nebo odstranění), zkontrolujte hlavičky x-ms požadavku poplatků hello (nebo ekvivalentní hello [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) vlastnost [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) v hello .NET SDK) toomeasure hello počet jednotek žádosti uplatníte tyto operace.

     // Measure hello performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure hello performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-toohello-indexing-policy-specification"></a>Změny toohello indexování specifikace zásad
Změnu hello schéma pro zásady indexování byla zavedena v 7. července 2015 se rozhraní REST API verze 2015-06-03. Hello odpovídající třídy v verze sady SDK hello mají nové implementace toomatch hello schéma. 

Hello následující změny byly implementovány v hello specifikace formátu JSON:

* Indexování zásad podporuje rozsah indexů pro řetězce
* Každá cesta může mít několik definic index, jednu pro každý typ dat
* Indexování přesnost podporuje 1 – 8 pro čísla 1 až 100 pro řetězce a -1 (Maximální přesnost)
* Segmenty cesty nevyžadují tooescape dvojité uvozovky, každá cesta zahrnovat. Například můžete přidat cestu pro/název /? místo / "title" /?
* Cesta ke kořenovému Hello představující "všechny cesty" může být reprezentován jako / * (kromě příliš nebo)

Pokud máte kód, který se zřídí kolekce s vlastní zásady indexování napsané pomocí verze 1.1.0 hello .NET SDK nebo starší, budete potřebovat toochange aplikace code toohandle tyto změny v pořadí toomove tooSDK verzi 1.2.0. Pokud není máte kód, který nakonfiguruje zásady indexování nebo plánování toocontinue pomocí starší verze sady SDK, je potřeba žádné změny.

Praktické porovnání tady je jeden příklad vlastní zásady indexování, které jsou napsané v hello REST API verze 2015-06-03 a také hello předchozí verze 2015-04-08.

**Předchozí zásady indexování JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Aktuální zásady indexování JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Další kroky
Postupujte podle hello najdete pod odkazy níže pro index zásady správy ukázky a toolearn více informací o databázi Cosmos Azure dotazovací jazyk.

1. [Ukázky kódu Správa indexu .NET DocumentDB rozhraní API](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [Operace kolekci DocumentDB rozhraní API REST](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [Dotaz s SQL](documentdb-sql-query.md)

