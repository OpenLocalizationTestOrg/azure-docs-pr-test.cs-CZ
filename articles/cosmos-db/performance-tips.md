---
title: "Tipy aaaPerformance - Azure Cosmos databáze NoSQL | Microsoft Docs"
description: "Další možnosti konfigurace klienta, tooimprove výkonu databáze Azure Cosmos DB"
keywords: "jak tooimprove databázi s vyšším výkonem"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 94ff155e-f9bc-488f-8c7a-5e7037091bb9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: mimig
ms.openlocfilehash: 4f3e82ae5048e3dbc20b0fd891a2d3aa22adf3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tips-for-azure-cosmos-db"></a>Tipy pro zvýšení výkonu pro Azure Cosmos DB
Azure Cosmos DB je rychlé a flexibilní distribuovanou databázi, která bezproblémově škáluje s zaručenou latence a propustnosti. Nemáte provést změny hlavní architektura toomake nebo zapsat složitý kód tooscale vaší databáze pomocí Cosmos DB. Škálování nahoru a dolů je stejně snadná jako provedení jednoho volání rozhraní API nebo [volání metody SDK](set-throughput.md#set-throughput-sdk). Ale protože Cosmos DB přistupuje prostřednictvím sítě volání existuje jsou optimalizace na straně klienta provedete tooachieve nejvyššího výkonu.

Takže pokud vás nemůže ověřit "jak vylepšit výkon Moje databáze?" Vezměte v úvahu hello následující možnosti:

## <a name="networking"></a>Sítě
<a id="direct-connection"></a>

1. **Zásady připojení: použití režimu přímé připojení**

    Jak se klient připojuje tooCosmos DB má důležité dopady na výkon, hlavně z hlediska zjištěnou latence klienta. Nejsou k dispozici pro konfiguraci klienta zásady připojení – hello připojení dvě nastavení konfigurace klíče *režimu* a hello [připojení *protokol*](#connection-protocol).  jsou dva režimy dostupné Hello:

   1. Režim brány (výchozí)
   2. Přímý režim

      Režim brány je podporovaná na všech platformách SDK a je hello nakonfigurované výchozí.  Pokud vaše aplikace běží v rámci podnikové sítě s omezeními striktní brány firewall, režim brány je nejlepší volbou hello vzhledem k tomu, že používá hello standardní port HTTPS a jeden koncový bod. Dobrý den kompromis výkonu, je však, že brány režimu zahrnuje směrování další síti pokaždé, když je číst nebo zapisovat tooCosmos DB data. Z toho důvodu přímý režim nabízí lepší výkon z důvodu toofewer síťové směrování.
<a id="use-tcp"></a>
2. **Zásady připojení: použití protokolu TCP hello**

    Při použití režimu přímého, existují dvě možnosti protokolu k dispozici:

   * TCP
   * HTTPS

     Cosmos DB nabízí jednoduchý a otevřete programovací model RESTful přes protokol HTTPS. Kromě toho nabízí efektivní protokolu TCP, který je také dosáhl standardu RESTful v jeho komunikace modelu a je k dispozici prostřednictvím hello klient .NET SDK. Přímé TCP a HTTPS používat protokol SSL pro počáteční ověřování a šifrování přenosů. Pro nejlepší výkon použijte hello protokolu TCP, pokud je to možné.

     Při použití protokolu TCP v režimu brány, TCP Port 443 je hello Cosmos DB portu a 10255 hello MongoDB API portu. Při použití protokolu TCP v režimu přímého v přidání toohello brány porty, třeba tooensure hello port rozsahu 10000 až 20000 je otevřený, protože Cosmos DB používá dynamické porty TCP. Pokud nejsou tyto porty otevřít a pokusíte se toouse TCP, obdržíte chybu 503 Služba není k dispozici.

     během vytváření hello instance DocumentClient hello s parametrem ConnectionPolicy hello je nakonfigurován Hello režim připojení. Pokud se používá přímý režim hello protokolu můžete také nastavit v rámci hello ConnectionPolicy parametr.

    ```C#
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from hello Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    Protože TCP je podporována pouze v režimu přímého, pokud se používá v režimu brány, pak hello protokol HTTPS, je vždy použít toocommunicate s hello brány a hello hodnotu protokolu v hello ConnectionPolicy je ignorován.

    ![Obrázek hello zásady Azure Cosmos DB připojení](./media/performance-tips/connection-policy.png)

3. **Volání OpenAsync tooavoid spuštění latence na první požadavek**

    Ve výchozím nastavení první požadavek hello má vyšší latence, protože má toofetch hello adresu směrovací tabulky. tooavoid tato čekací doba spuštění na hello nejdřív požádat, OpenAsync() by měly volat jednou během inicializace následujícím způsobem.

        await client.OpenAsync();
   <a id="same-region"></a>
4. **Společně umístit klienty ve stejné oblasti Azure výkonu**

    Pokud to možné, všechny aplikace, volání Cosmos DB v hello stejné oblasti jako místní hello Cosmos DB databáze. Při porovnávání přibližnou volá tooCosmos DB v rámci stejné oblasti dokončení v ms 1 – 2, ale hello latenci mezi hello – západ a východním pobřeží USA je hello hello > 50 ms. Tato čekací doba se můžou pravděpodobně lišit od toorequest žádosti v závislosti na hello trasy provedenou hello žádost jako předává z hello klienta toohello datové centrum Azure hranic. Hello nejnižší možnou latenci je dosaženo tak hello volání aplikace se nachází v rámci hello stejné oblasti Azure jako hello zřízený Cosmos DB koncový bod. Seznam dostupných oblastí najdete v tématu [oblasti Azure](https://azure.microsoft.com/regions/#services).

    ![Obrázek hello zásady Azure Cosmos DB připojení](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
5. **Zvýšit počet vláken/úlohy**

    Vzhledem k tomu, že volání tooAzure Cosmos DB probíhají hello síti, může být nutné toovary hello stupně paralelního zpracování žádostí o tak, aby hello klientská aplikace stráví velmi malé dobu čekání mezi požadavky. Pokud například používáte. NET na [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx), vytvořte v pořadí hello 100s úloh čtení nebo zápis tooCosmos DB.

## <a name="sdk-usage"></a>Použití sady SDK
1. **Nainstalujte hello nejnovější SDK**

    Hello Cosmos DB SDK neustále vylepšené tooprovide hello nejlepší výkon. V tématu hello [Cosmos DB SDK](documentdb-sdk-dotnet.md) stránky toodetermine hello nejnovější SDK a zkontrolujte vylepšení.
2. **Použít klienta Cosmos DB jednotlivý prvek pro hello životního cyklu aplikace**

    Všimněte si, že každá instance DocumentClient je bezpečné pro přístup z více vláken a provádí správu efektivní připojení a ukládání adres do mezipaměti v režimu přímého. Správa tooallow efektivní připojení a lepší výkon pomocí DocumentClient, je doporučeno toouse jedna instance DocumentClient jednotlivé domény aplikace pro hello životního cyklu aplikace hello.

   <a id="max-connection"></a>
3. **Při použití režimu brány zvýšit System.Net MaxConnections na hostitele**

    Žádosti o cosmos DB jsou proveden prostřednictvím protokolu HTTPS nebo REST, při použití režimu brány a jsou vystaveno toohello výchozí limit připojení pro hostitele nebo IP adresa. Tooset hello MaxConnections tooa vyšší hodnota (100-1000) může být nutné, aby hello klientské knihovny může využívat víc souběžných připojení tooCosmos DB. V hello .NET SDK 1.8.0 a výše, hello výchozí hodnota pro [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) je 50 a toochange hello hodnotu, můžete nastavit hello [Documents.Client.ConnectionPolicy.MaxConnectionLimit ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) tooa vyšší hodnotu.   
4. **Ladění paralelní dotazy pro dělené kolekce**

     DocumentDB .NET SDK verze 1.9.0 a vyšší paralelní dotazy podpory, které umožňují tooquery dělenou kolekci paralelně (najdete v části [práce s hello sady SDK](documentdb-partition-data.md#working-with-the-azure-cosmos-db-sdks) a hello související [ukázky kódu](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) pro Další informace o). Paralelní dotazy jsou přes jejich sériové protějšku navrženou tooimprove dotazu latence a propustnosti. Paralelní dotazy poskytují dva parametry, uživatelé mohli vyladit toocustom-fit jejich požadavků (a) MaxDegreeOfParallelism: toocontrol hello maximální počet oddílů pak může být dotazována v paralelní a (b) MaxBufferedItemCount: počet toocontrol hello předem načtených výsledky.

    (a) ***ladění MaxDegreeOfParallelism\: *** paralelní dotaz funguje dotazováním více oddílů souběžně. Data z shromažďování jednotlivých oddílů je však načtených sériově s ohledem toohello dotazu. Ano nastavení hello MaxDegreeOfParallelism toohello počet oddílů má hello maximální riziko dosažení hello většina původce dotazu, zadaný všechny další podmínky systému zůstanou hello stejné. Pokud si nejste jisti hello počet oddílů, můžete nastavit hello MaxDegreeOfParallelism tooa vysoké číslo, a systém hello zvolí hello minimální (počet oddílů, uživatel zadal vstup) jako hello MaxDegreeOfParallelism.

    Je důležité toonote, který paralelní dotazy produktu hello nejlepší výhody, pokud hello dat je rovnoměrně rozdělené mezi všechny oddíly s ohledem toohello dotazu. Pokud hello kolekce rozdělena na oddíly je rozdělena na oddíly tak, že se soustřeďuje všechny nebo většinu hello dat vrácených dotazem do několika oddílů (jeden oddíl v nejhorším případě), a potom hello výkonu hello dotazu by omezené podle těchto oddílů.

    (b) ***ladění MaxBufferedItemCount\: *** paralelního dotazu je navrženou toopre načítání výsledků, během zpracování dávky aktuální hello výsledků klientem hello. předběžné načítání pomáhá v celkového zlepšení latence dotazu Hello. MaxBufferedItemCount je hello parametr toolimit hello počet předem načtených výsledky. Nastavení MaxBufferedItemCount toohello očekává, že počet výsledků vrácených (nebo vyšším číslem) umožňuje maximální benefit tooreceive hello dotazu z předem načítání.

    Upozorňujeme, že předběžné načítání funguje hello stejný způsobem bez ohledu hello MaxDegreeOfParallelism a je do jedné vyrovnávací paměti pro hello data ze všech oddílů.  
5. **Zapnout na straně serveru globálního katalogu**

    V některých případech mohou pomoci snížit frekvenci hello uvolňování paměti. V rozhraní .NET, nastavte [gcserver –](https://msdn.microsoft.com/library/ms229357.aspx) tootrue.
6. **Implementace omezení rychlosti v intervalech RetryAfter**

    Během testování výkonu měli zvýšit zatížení, dokud malý počet žádostí o získání omezeny. Pokud omezené, měli klientské aplikace hello omezení rychlosti na omezení pro interval opakování zadaný server hello. Bere ohledy na omezení rychlosti hello zajišťuje strávený minimální množství času čekat mezi opakovanými pokusy. Podpora zásad opakování je součástí verze 1.8.0 a výše z hello DocumentDB [.NET](documentdb-sdk-dotnet.md) a [Java](documentdb-sdk-java.md), verze 1.9.0 a vyšší z hello [Node.js](documentdb-sdk-node.md) a [ Python](documentdb-sdk-python.md), a všechny podporované verze systému hello [.NET Core](documentdb-sdk-dotnet-core.md) sady SDK. Další informace najdete v tématu [Exceeding vyhrazené omezení propustnosti](request-units.md#RequestRateTooLarge) a [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **Horizontální navýšení kapacity vaše úlohy klienta**

    Pokud testujete na úrovních vysoké propustnosti (> 50 000 RU/s), klientská aplikace hello se může stát úzkým místem hello kvůli toohello počítače malá a velká se na využití procesoru nebo sítě. Pokud dostanete tento bod, můžete pokračovat toopush hello Cosmos DB účtu další tím škálování klientských aplikací na více serverech.
8. **Dokument mezipaměti identifikátorů URI pro nižší latenci pro čtení**

    Dokument mezipaměti identifikátory URI, kdykoli je to možné pro nejlepší výkon čtení hello.
   <a id="tune-page-size"></a>
9. **Vyladění hello velikost stránky pro dotazy/číst informační kanály pro lepší výkon**

    Při provádění hromadně číst dokumenty pomocí funkce (například ReadDocumentFeedAsync) informační kanál čtení, nebo při vystavování dotazu DocumentDB SQL, hello výsledky se vrátí segmentovaným způsobem, pokud hello sadu výsledků dotazu je příliš velký. Ve výchozím nastavení budou vráceny výsledky v bloky 100 položky nebo 1 MB, z těchto omezení dosáhl první.

    počet hello tooreduce síť odezev požadované tooretrieve všechny příslušné výsledky, můžete zvýšit velikost stránky hello pomocí too1000 tooup hlavičky x-ms-max--počet položek požadavku. V případech, kde je nutné toodisplay jenom pár výsledky například pokud vaše uživatelské rozhraní nebo aplikací rozhraní API vrátí jenom 10 výsledků na dobu, může také snížit hello stránky velikost too10 tooreduce hello propustnost využité pro čtení a dotazy.

    Můžete také nastavit velikost stránky hello pomocí hello k dispozici sady SDK DB Cosmos.  Například:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Zvýšit počet vláken/úlohy**

    V tématu [zvýšit počet vláken/úlohy](#increase-threads) v hello části sítě.

11. **Použijte hostitele 64-bit zpracování**

    Hello DocumentDB SDK pracuje v 32bitové hostitelský proces, při použití DocumentDB .NET SDK verze 1.11.4 a vyšší. Ale pokud používáte křížové oddílu dotazy, se doporučuje zpracování hostitelem 64-bit pro zlepšení výkonu. Hello následující typy aplikací mít 32bitové hostitele zpracovat jako výchozí hello, takže v pořadí toochange to too64 bitů, podle těchto kroků v závislosti na typu hello vaší aplikace:

    - Pro spustitelný soubor aplikace, to lze provést pomocí zaškrtnutí políčka hello **raději 32bitové** možnost v hello **vlastnosti projektu** okně na hello **sestavení** kartě.

    - Pro testovací projekty založené na VSTest, to lze provést výběrem **testování**->**Test nastavení**->**výchozí architekturu procesoru jako X64**, z hello **testu sady Visual Studio** možnost nabídky.

    - Pro místně nasazené webové aplikace ASP.NET, stačí kontrolou hello **použití hello 64bitová verze služby IIS Express pro weby a projekty**v části **nástroje** ->  **Možnosti**->**projekty a řešení**->**webových projektů**.

    - Pro ASP.NET – webové aplikace nasazené na platformě Azure, to lze provést výběrem hello **platforma jako 64-bit** v hello **nastavení aplikace** na hello portálu Azure.

## <a name="indexing-policy"></a>Zásady indexování
1. **Použít Opožděné indexování pro rychlejší sazby přijímání dobu ve špičce**

    Cosmos DB vám umožní toospecify – na úrovni kolekce hello – indexování zásady, které umožní vám toochoose, pokud chcete, aby hello dokumenty v kolekci toobe, automaticky indexované nebo ne.  Kromě toho můžete taky rozhodnout mezi synchronní (konzistentní) a asynchronní aktualizace indexu (Lazy). Ve výchozím nastavení hello aktualizace indexu synchronně na každý insert, replace nebo odstranění toohello kolekce dokumentů. Synchronně režim umožňuje hello dotazy toohonor hello stejné [úroveň konzistence](consistency-levels.md) jako hello dokumentu načte bez jakéhokoli zpoždění pro hello index příliš "nezaznamenaly".

    Opožděné indexování lze považovat za pro scénáře, ve kterých je zapisovat data v shluky a chcete tooamortize hello práce potřebné tooindex obsah po delší dobu. Opožděné indexování také umožňuje toouse vaší efektivně zřízené propustnosti a používat požadavků na zápis ve špičce s minimální latenci. Je důležité toonote, ale, že pokud je povoleno indexování opožděné, výsledky dotazu jsou nakonec byl konzistentní bez ohledu na nakonfigurované pro účet Cosmos DB hello úroveň konzistence hello.

    Proto konzistentní indexování režimu (IndexingPolicy.IndexingMode je nastavený tooConsistent) způsobuje hello nejvyšší požadavek jednotky poplatků za zápis při Lazy indexování režim (IndexingPolicy.IndexingMode je nastavený tooLazy) a žádný indexování (IndexingPolicy.Automatic nastavena tooFalse) mají nulové indexování náklady v době zápisu hello.
2. **Vyloučit z indexování pro rychlejší zápisy nepoužívané cesty**

    Zásady indexování cosmos DB společnosti vám umožní toospecify, která dokumentu tooinclude cesty nebo vyloučit z indexování s využitím indexování cesty (IndexingPolicy.IncludedPaths a IndexingPolicy.ExcludedPaths). použití Hello indexování cesty můžete nabízí vylepšené zápisu výkonu a nižší index úložiště pro scénáře, ve které hello vzorům dotazů znám předem, jsou-li indexování náklady přímo korelační toohello počet jedinečných cesty indexované.  Například hello následující kód ukazuje, jak tooexclude celý oddíl hello dokumentů (také známa jako podstrom) z indexování pomocí hello "*" zástupný znak.

    ```C#
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Další informace najdete v tématu [Azure DB Cosmos indexování zásady](indexing-policies.md).

## <a name="throughput"></a>Propustnost
<a id="measure-rus"></a>

1. **Měření a optimalizovat pro nižší požadavek jednotek za sekundu využití**

    Cosmos DB nabízí širokou škálu databázové operace, včetně dotazů na relační a hierarchických UDF, uložené procedury a triggery – všechny provozní na dokumentech hello v rámci kolekce databáze. Hello náklady spojené s každou z těchto operací se liší podle hello procesoru, vstupně-výstupní operace a požadované paměti toocomplete hello operaci. Namísto přemýšlení o a správu hardwarové prostředky si můžete představit jednotka žádosti (RU) jako jednu míru pro tooperform požadované prostředky hello různé operace databáze a služba žádostí na aplikace.

    [Požadované jednotky](request-units.md) zřízeny pro každý účet databáze na základě hello počtu jednotek kapacity, které jste si koupili. Spotřeba jednotek žádosti budou vyhodnocené jako za sekundu. Aplikace, které překročit hello zřízené požadavek jednotku hodnocení pro svůj účet je omezená, dokud hello rychlost klesne pod hello úroveň vyhrazené pro účet hello. Pokud vaše aplikace vyžaduje vyšší úroveň propustnosti, si můžete zakoupit jednotky dodatečnou kapacitu.

    složitost Hello dotazu má dopad na tom, kolik jednotek žádosti se spotřebovávají pro operace. Hello počet predikáty, povaha hello predikáty, počtu UDF a velikosti hello hello zdroje dat sady všechny ovlivnit hello náklady na dotaz operace.

    toomeasure hello režii všechny operace (vytvoření, aktualizace nebo odstranění), zkontrolujte hlavičky x-ms požadavku poplatků hello (nebo ekvivalentní vlastnost RequestCharge v ResourceResponse hello<T> nebo FeedResponse<T> v hello .NET SDK) toomeasure Hello počet jednotek žádosti uplatníte tyto operace.

    ```C#
    // Measure hello performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure hello performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    Hello požadavek poplatků, vrátí se v tuto hlavičku je zlomek zřízené propustnosti (tj, RUs 2000 / druhý). Například pokud hello předchozí dotaz vrátí 1000 1KB – dokumenty, náklady na hello operace hello je 1 000. Jako takový jedné sekundy ctí hello server pouze dva takových požadavků před omezení následných žádostí. Další informace najdete v tématu [požadované jednotky](request-units.md) a hello [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Rychlost omezení nebo požadavků popisovač míra příliš velký**

    Když se klient pokusí tooexceed hello vyhrazenou propustností pro účet, není bez snížení výkonu na serveru hello a bez využití kapacity propustnosti mimo úroveň hello vyhrazena. Hello server se ukončí ho preventivně hello žádost s RequestRateTooLarge (kód stavu HTTP 429) a návratové hello hlavičky x-ms opakování za ms informující o hello množství času v milisekundách, která hello uživatel musí počkat před provedením nového pokusu hello požadavku.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    sady SDK Hello všechny implicitně catch této odpovědi, respektují hello zadaný server opakovat po záhlaví a opakujte žádost hello. Pokud váš účet je současně přistupuje více klientů, hello další pokus bude úspěšné.

    Pokud máte více než jednoho klienta kumulativně operační konzistentně vyšší rychlost požadavků hello, hello výchozí počet opakování aktuálně sadu too9 interně klientem hello nemusí stačit; v takovém případě hello klienta vyvolá DocumentClientException s aplikaci toohello 429 kódu stavu. Počet opakování výchozí Hello může změnit nastavení hello RetryOptions na instanci ConnectionPolicy hello. Ve výchozím nastavení je hello DocumentClientException se stavovým kódem 429 vrátila po uplynutí určité doby kumulativní Počkejte 30 sekund, pokud požadavek hello pokračuje toooperate vyšší rychlost požadavků hello. K tomu dojde i v případě hello aktuální počet opakování je menší než počet hello maximálního počtu opakování, je jej hello výchozí hodnotu 9 nebo hodnotu definovanou uživatelem.

    Při automatizované hello opakování chování pomáhá tooimprove odolnost a použitelnost pro hello většinu aplikací, by mohl pocházet ve odds, když srovnávacího testu výkonu, zejména při měření latence.  latence klienta zjištěnými Hello bude špiček Pokud hello experimentu dotkne hello serveru omezení a způsobí, že hello klienta SDK toosilently opakování. latence tooavoid špičky během výkonu experimenty měr hello poplatků vrácený jednotlivých operací a ujistěte se, že jsou pod rychlost vyhrazené požadavků hello operační požadavky. Další informace najdete v tématu [požadované jednotky](request-units.md).
3. **Návrh pro menší dokumenty pro vyšší propustnost**

    Hello poplatků požadavku (tj. zpracování žádosti náklady) dané operace je přímo korelační toohello velikost hello dokumentu. Operace u velkých dokumentů nákladů více než operací pro malé dokumenty.

## <a name="next-steps"></a>Další kroky
Ukázková aplikace používá tooevaluate Cosmos DB pro vysoce výkonné scénáře na několik klientských počítačů, najdete v části [výkonu a možností škálování testování pomocí Cosmos DB](performance-testing.md).

Viz také toolearn Další informace o návrhu aplikace pro škálování a vysoký výkon, [dělení a škálování v Azure Cosmos DB](partition-data.md).
