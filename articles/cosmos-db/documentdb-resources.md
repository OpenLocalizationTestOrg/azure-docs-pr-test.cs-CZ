---
title: "aaaAzure Cosmos DB model prostředků a koncepty | Microsoft Docs"
description: "Další informace o databázi Cosmos Azure hierarchické model databáze, kolekce, uživatelem definované funkce (UDF), dokumentů, oprávnění toomanage prostředky a další."
keywords: "Model hierarchické, cosmosdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: ef9d5c0c-0867-4317-bb1b-98e219799fd5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: anhoh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc3642232b86cc27901ebd97456c386829324632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Hierarchický model prostředků a základní koncepty databáze Azure Cosmos
Hello entity databáze, které spravují databázi Cosmos Azure jsou odkazované tooas **prostředky**. Každý prostředek je jedinečně identifikovaný logického identifikátoru URI. Můžete pracovat s prostředky hello pomocí standardních operací protokolu HTTP, hlaviček požadavků a odpovědí a stavové kódy. 

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:

* Co je model prostředků Cosmos DB?
* Co jsou systému definované prostředky jako názvem na rozdíl od toouser definované prostředky?
* Jak řeší prostředku?
* Jak funguje s kolekcí?
* Jak funguje s uložené procedury, triggery a uživatelem definované funkce (UDF)?

## <a name="hierarchical-resource-model"></a>Model hierarchické prostředků
Jako hello následující diagram znázorňuje, hello Cosmos DB hierarchické **model prostředků** se skládá ze sady prostředků v rámci účtu databáze, každý adresovatelné prostřednictvím logické a stabilní identifikátoru URI. Sadu prostředků bude odkazované tooas **kanálu** v tomto článku. 

> [!NOTE]
> Azure Cosmos DB nabízí vysoce efektivní protokolu TCP, který je taky dosáhl standardu RESTful svůj model komunikace, k dispozici prostřednictvím hello [klient DocumentDB .NET rozhraní API](documentdb-sdk-dotnet.md).
> 
> 

![Model hierarchické prostředků Azure Cosmos DB][1]  
**Model hierarchické prostředků**   

toostart práci s prostředky, je nutné [vytvoření databázového účtu](create-documentdb-dotnet.md) pomocí svého předplatného Azure. Databázový účet se může skládat z sadu **databáze**, každá obsahuje několik **kolekce**, každý naopak obsahovat **uložené procedury, aktivuje, funkce UDF, dokumenty**a související **přílohy**. Databáze také přiřazeni **uživatelé**, každý s sadu **oprávnění** tooaccess kolekcí, uložené procedury, triggery, UDF, dokumenty nebo přílohy. Databáze, uživatelé, oprávnění a kolekce jsou systémem definované prostředky s dobře známými schématy, dokumenty a přílohy obsahují libovolný, uživatelem definovaný obsah JSON.  

| Prostředek | Popis |
| --- | --- |
| Účet databáze |Databázový účet je přidružen sadu databází a pevné velikosti úložiště objektů blob pro přílohy. Můžete vytvořit jeden nebo více účtů databáze pomocí svého předplatného Azure. Další informace najdete v článku naše [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Databáze |Databáze je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi. Je také kontejner uživatelé. |
| Uživatel |Hello logické obor názvů pro obor oprávnění. |
| Oprávnění |Autorizační token přidružit k uživateli pro konkrétní prostředek tooa přístup. |
| Kolekce |Kolekce je kontejner dokumentů JSON a přidružené logiky Javascriptové aplikace hello. Kolekce je fakturovatelná entita, kde hello [náklady](performance-levels.md) je určen podle úrovně výkonu hello přidružené hello kolekce. Kolekce může mít rozsah jeden nebo více oddílů nebo serverů a lze je škálovat toohandle prakticky neomezené objemy úložišť a propustnosti. |
| Uložená procedura |Logiku aplikace napsané v jazyce JavaScript, který je registrován s kolekcí a transakčně provést v rámci hello databázového stroje. |
| Aktivační události |Logiku aplikace napsané v jazyce JavaScript provést před nebo po buď vložit, nahradí nebo operace odstranění. |
| UDF |Logiku aplikace napsané v jazyce JavaScript. UDF povolte toomodel operátor vlastního dotazu a tím rozšiřovat hello jádra rozhraní API DocumentDB dotazovací jazyk. |
| Dokument |Uživatelem definovaný (libovolný) obsah JSON. Ve výchozím nastavení musí se žádné schéma definované toobe ani sekundární indexy, které potřebují toobe zadaný pro všechny hello dokumenty přidat tooa kolekce. |
| Přílohy |Přílohu je speciální dokument obsahující odkazy a související metadata pro externí objektů blob nebo médium. Hello vývojáře můžete vybrat blob hello toohave spravuje Cosmos DB nebo jej uložte u poskytovatele služeb externí objekt blob, jako je například OneDrive, Dropbox, atd. |

## <a name="system-vs-user-defined-resources"></a>Systém a prostředky definované uživatelem
Prostředkům, například účty databáze, databáze, kolekce, uživatelé, oprávnění, uložené procedury, aktivační události a UDF – všechny mají pevného schématu a se označují jako systémové prostředky. Naproti tomu prostředkům, například dokumentů a příloh mít žádná omezení na schéma hello a jsou příklady prostředků definované uživatelem. V Cosmos databáze, systém a uživatel definované prostředky reprezentované a spravovat jako kompatibilní se standardem standard JSON. Všechny prostředky, systém nebo uživatelsky definované, máte následující běžné vlastnosti hello.

> [!NOTE]
> Všimněte si, že všechny systému vygenerovaných vlastnosti prostředku začínají podtržítkem (_) v jejich reprezentace JSON.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Vlastnost</strong></p></td>
            <td valign="top"><p><strong>Nastavit uživatele nebo vygenerované systémem?</strong></p></td>
            <td valign="top"><p><strong>Účel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Vygenerované systémem</p></td>
            <td valign="top"><p>Generované systémem, hierarchické a jedinečný identifikátor prostředku hello</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Vygenerované systémem</p></td>
            <td valign="top"><p>Značka Etag hello prostředku požadované pro optimistické řízení souběžného</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Vygenerované systémem</p></td>
            <td valign="top"><p>Poslední aktualizované časové razítko prostředků hello</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Vygenerované systémem</p></td>
            <td valign="top"><p>Jedinečný identifikátor URI adresovatelné hello prostředku</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Vygenerované systémem</p></td>
            <td valign="top"><p>Uživatelem definované jedinečný název prostředku hello (s hello oddílu stejnou hodnotu klíče). Pokud uživatel hello neurčuje id, bude id vygenerované systémem</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Síťové vyjádření prostředků
Cosmos DB nenutí všechny vlastní rozšíření toohello JSON standardní nebo speciální kódování; funguje s standardní kompatibilní dokumentů JSON.  

### <a name="addressing-a-resource"></a>Adresování prostředku
Všechny prostředky jsou adresovatelné identifikátor URI. Hello hodnotu hello **_self** vlastnost prostředku hello představuje relativní identifikátor URI prostředku hello. Hello formát hello URI se skládá z hello /\<kanálu\>/ {_rid} segmenty cesty:  

| Hodnota hello _self | Popis |
| --- | --- |
| /DBS |Informační kanál databází pod účtem databáze |
| /DBS/ {dbName} |Databáze s id odpovídající hodnota hello {dbName} |
| /colls/ /DBS/ {dbName} |Informační kanál kolekcí v databázi |
| /colls/ /DBS/ {dbName} {collName} |Kolekce s id odpovídající hodnota hello {collName} |
| /colls/ /DBS/ {dbName} {collName} / docs |Informační kanál dokumentů v kolekci |
| /docs/ /colls/ {collName} /DBS/ {dbName} {docId} |Dokumentů s id odpovídající hodnota hello {doc} |
| /users/ /DBS/ {dbName} |Informační kanál uživatele v databázi |
| /users/ /DBS/ {dbName} {userId} |Uživatel s id odpovídající hodnota hello {uživatele} |
| /users/ /DBS/ {dbName} {userId} nebo oprávnění |Informační kanál oprávnění pod uživatelským |
| /permissions/ /users/ {userId} /DBS/ {dbName} {permissionId} |Oprávnění s id odpovídající hodnota hello {oprávnění} |

Každý prostředek, má název jedinečný uživatelsky definované zveřejňovány prostřednictvím vlastnost id hello. Poznámka: pro dokumenty, pokud uživatel hello neurčuje id, naše podporovaných sad SDK automaticky vygeneruje jedinečné id pro dokument hello. Hello id je řetězec definované uživatelem, až too256 znaky, které je jedinečné v rámci kontextu hello konkrétní nadřazený prostředek. 

Každý prostředek, také má identifikátor hierarchické prostředků vygenerované systémem (také odkazované tooas identifikátorů RID), která je dostupná přes vlastnost _rid hello. Hello identifikátorů RID kóduje hello celou hierarchii daného prostředku a je vhodné interního vyjádření použitou v distribuovanému tooenforce referenční integrity. Hello identifikátorů RID je jedinečná v rámci účtu databáze a Cosmos DB je používán interně pro efektivní směrování, bez nutnosti křížové oddílu vyhledávání. hodnoty Hello hello _self a hello _rid vlastnosti jsou alternativní i kanonický reprezentace prostředku. 

Podpora rozhraní REST API Hello adresování prostředků a směrování žádostí hello _rid vlastnosti i hello id.

## <a name="database-accounts"></a>Databáze účtů
Můžete zřídit jeden nebo více Cosmos DB databáze účtů pomocí svého předplatného Azure.

Můžete vytvořit a spravovat účty pro databázi Cosmos DB prostřednictvím hello portálu Azure v [http://portal.azure.com/](https://portal.azure.com/). Vytváření a správa databázový účet vyžaduje přístup pro správu a lze provést pouze v rámci vašeho předplatného Azure. 

### <a name="database-account-properties"></a>Vlastnosti účtu databáze
V rámci zřizování a správa databázového účtu můžete nakonfigurovat a čtení hello následující vlastnosti:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Název vlastnosti</strong></p></td>
            <td valign="top"><p><strong>Popis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Konzistence zásad</p></td>
            <td valign="top"><p>Nastavte tuto vlastnost tooconfigure hello výchozí úroveň konzistence pro všechny kolekce hello v rámci účtu databáze. Můžete přepsat úroveň konzistence hello na základě žádosti pomocí hlavička požadavku hello [x-ms--úroveň konzistence]. <p><p>Všimněte si, že tato vlastnost se týká pouze toohello <i>uživatelem definované prostředky</i>. Všechny prostředky definovaná systémem nejsou nakonfigurované toosupport čtení nebo dotazy s silnou konzistenci.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Autorizace klíče</p></td>
            <td valign="top"><p>Jedná se o hello primární a sekundární hlavní a jen pro čtení klíče, které poskytují pro správu přístupu tooall hello prostředků v rámci účtu databáze hello.</p></td>
        </tr>
    </tbody>
</table>

Všimněte si, že v přidání tooprovisioning, konfigurace a Správa účtu databáze z hello portálu Azure, můžete také prostřednictvím kódu programu vytvořit a spravovat účty Cosmos DB databáze pomocí hello [rozhraní API REST Azure Cosmos DB](/rest/api/documentdb/) jako a taky [klientskou sadu SDK](documentdb-sdk-dotnet.md).  

## <a name="databases"></a>Databáze
Cosmos DB databáze je logický kontejner jeden nebo více kolekcí a uživatelů, jak je znázorněno v následujícím diagramu hello. Můžete vytvořit libovolný počet databází v části Cosmos DB databáze účet subjektu toooffer omezení.  

![Účet a kolekce hierarchické model databáze][2]  
**Databáze je logický kontejner uživatelů a kolekce**

Databáze může obsahovat úložiště prakticky neomezené dokumentů rozděleného v rámci kolekce.

### <a name="elastic-scale-of-a-cosmos-db-database"></a>Elastické škálování databáze Cosmos DB
Je ve výchozím nastavení – od několika toopetabytes GB SSD zálohovaný dokumentu úložiště a zřízené propustnosti elastické databáze Cosmos DB. 

Na rozdíl od databáze v tradiční relační databáze v databázi Cosmos není vymezená tooa jeden počítač. Pomocí Cosmos DB jako škálování aplikace potřebuje toogrow, vytvoříte více kolekcí, databáze nebo obojí. Ve skutečnosti různé první strany aplikací v rámci Microsoft jste dosud používali Cosmos DB škálované příjemce vytvořením velmi velké databáze Cosmos DB každý obsahující tisíce kolekce s terabajtů úložiště dokumentů. Může zvětšovat a zmenšovat databáze přidáním nebo odebráním kolekce toomeet požadavky rozsahu vaší aplikace. 

Můžete vytvořit libovolný počet kolekcí v rámci na databázi subjektu toohello nabídku. Každá kolekce má založenou na SSD úložiště a zřízené pro vás v závislosti na úroveň výkonu vybrané hello propustnosti.

Cosmos DB databáze je také kontejner uživatelů. Uživatel, naopak, je logický obor názvů pro sadu oprávnění, která poskytuje jemně odstupňovaných toocollections autorizace a přístupu, dokumenty a přílohy.  

Jako s další prostředky ve model prostředků Cosmos DB hello databáze se dají vytvářet, nahradit, odstranit, přečíst nebo uvedené snadno pomocí buď hello [rozhraní REST API](/rest/api/documentdb/) ani v žádné z hello [klientskou sadu SDK](documentdb-sdk-dotnet.md). Cosmos DB zaručuje silnou konzistenci pro čtení nebo dotazování hello metadata databáze prostředků. Odstranění databáze automaticky zajistí, že nemůžete použít žádnou z kolekce hello nebo uživatelé jsou v něm obsažena.   

## <a name="collections"></a>Kolekce
Cosmos DB kolekce je kontejner dokumentů JSON. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastické SSD zálohovaný úložiště dokumentů
Kolekce je vnitřně elastické – automatické zvětšování a zmenší tak, jak přidat nebo odebrat dokumenty. Kolekce jsou logické prostředky a může mít rozsah jeden nebo více fyzických oddílů nebo serverů. Hello počet oddílů v rámci kolekce je dáno DB Cosmos na základě hello velikost úložiště a zřízené propustnosti hello vaší kolekce. Každý oddíl v Cosmos DB má pevně stanovený objem zálohovaná na SSD úložiště s ním spojená a se replikují pro vysokou dostupnost. Oddíl správy je plně spravovat Azure Cosmos DB a nemáte mít složitý kód toowrite nebo spravovat vaše oddíly. Cosmos DB kolekce jsou **prakticky neomezené** z hlediska úložiště a propustnosti. 

### <a name="automatic-indexing-of-collections"></a>Automatické indexování kolekcí
Cosmos DB je systém true databáze bez schémat. Se nepředpokládá ani nevyžaduje žádné schéma dokumentů JSON hello. Při přidávání tooa kolekce dokumentů, Cosmos DB automaticky indexuje a jsou k dispozici pro tooquery je. Automatické indexování dokumentů, aniž byste museli schématu nebo sekundární indexy je klíčové funkce Cosmos DB a je povolen jako techniky údržby optimalizovaný pro zápis, uvolněte zámku a protokolu strukturovaná index. Cosmos DB podporuje dlouhodobě svazku extrémně rychlou zápisů při poskytování stále konzistentní dotazů. Dokument a index úložiště se používá úložiště hello toocalculate spotřebovávají každou kolekci. Můžete ovládat hello úložiště a výkon kompromis přidružené k indexování nakonfigurováním hello zásady indexování pro kolekci. 

### <a name="configuring-hello-indexing-policy-of-a-collection"></a>Konfigurace zásady indexování hello kolekce
Hello indexování zásad každá kolekce vám umožní toomake výkonu a úložiště kompromis přidružené k indexování. Hello jsou následující možnosti k dispozici tooyou jako součást indexování konfigurace:  

* Zvolte, zda kolekce hello automaticky indexuje všechny dokumenty hello nebo ne. Ve výchozím nastavení jsou všechny dokumenty automaticky indexovány. Můžete zvolit tooturn vypnout automatické indexování a selektivně přidat pouze konkrétní dokumenty toohello index. Naopak selektivně můžete tooexclude pouze konkrétní dokumenty. Můžete dosáhnout nastavením hello automatické vlastnost toobe true nebo false na hello indexingPolicy kolekce a pomocí hlavička požadavku hello [x-ms-indexingdirective] při vkládání, nahraďte nebo odstranění dokumentu.  
* Zvolte, zda tooinclude nebo vyloučit konkrétní cesty nebo vzory v dokumentech z hello index. Můžete dosáhnout to tak, že nastavení includedPaths a excludedPaths na indexingPolicy hello kolekce v uvedeném pořadí. Můžete také konfigurovat hello úložiště a výkon kompromis pro rozsah a hash dotazy pro vzory konkrétní cesty. 
* Výběr mezi synchronní (konzistentní) a aktualizace asynchronní indexu (lazy). Ve výchozím nastavení hello aktualizace indexu synchronně na každý insert, replace nebo odstranění toohello kolekce dokumentů. To umožňuje hello dotazy toohonor hello stejnou úroveň konzistence jako čtení dokumentu hello. Při Cosmos databáze je optimalizovaná pro zápis a podporuje dlouhodobě svazky zápisů dokumentu společně s synchronní indexu údržby a obsluhující konzistentní dotazy, můžete nakonfigurovat určité kolekce tooupdate svého indexu líné. Opožděné indexování zvyšuje výkon hello zápisu další a je ideální pro hromadné přijímání scénáře pro především pro čtení náročné kolekce.

Hello indexování zásad lze změnit spuštěním PUT na kolekci hello. To může být buď prostřednictvím hello dosáhnout [klienta SDK](documentdb-sdk-dotnet.md), hello [portálu Azure](https://portal.azure.com) nebo hello [rozhraní REST API](/rest/api/documentdb/).

### <a name="querying-a-collection"></a>Dotazování na kolekci
Hello dokumenty v rámci kolekce může obsahovat libovolný schémata a můžete dát dotaz na dokumenty v rámci kolekce bez zadání žádné schéma nebo předem sekundárních indexů. Můžete dát dotaz na kolekce hello pomocí hello [rozhraní API služby Azure Cosmos databáze DocumentDB: reference syntaxe SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx), která nabízí bohaté hierarchické, relační a prostorových operátory a rozšiřitelnost prostřednictvím bázi jazyka JavaScript UDF. Gramatika JSON umožňuje modelování dokumentů JSON jako stromy s popisky jako uzly stromu hello. To je zneužití jak DocumentDB API automatických technikách indexování, jakož i dialekt DocumentDB API SQL. Hello DocumetDB API dotazovací jazyk zahrnuje tři hlavní aspekty:   

1. Malá sada operace dotazů, které mapují přirozeně toohello stromová struktura, včetně hierarchické dotazy a projekce. 
2. Podmnožinu relační operacím, včetně složení, filtr, projekce, agregace a vlastní spojení. 
3. Čistý JavaScript na základě UDF, které pracují s (1) a (2).  

model dotazování Cosmos DB Hello pokusí toostrike rovnováhu mezi funkce, efektivitu a jednoduchost. Hello Cosmos DB databázový stroj nativně kompilovaný a provede příkazy dotazu SQL hello. Můžete dát dotaz na kolekce pomocí hello [rozhraní REST API](/rest/api/documentdb/) ani v žádné z hello [klientskou sadu SDK](documentdb-sdk-dotnet.md). Hello .NET SDK se dodává s LINQ zprostředkovatele.

> [!TIP]
> Můžete vyzkoušet hello DocumentDB rozhraní API a spouštění dotazů SQL na našem datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Transakcí několika dokumentů
Databázové transakce zadejte bezpečné a předvídatelné programovací model pro práci s daty toohello souběžných změny. V RDBMS, hello tradičním způsobem, jakým toowrite obchodní logika je toowrite **uložené procedury** nebo **aktivační události** a dodávají spolu toohello databázový server pro spouštění transakcí. V RDBMS programátory hello aplikace je požadovaná toodeal se dvěma různorodých programovací jazyky: 

* programovací jazyk pro Hello (netransakční) aplikace (například JavaScript, Python, C#, Java, atd.)
* T-SQL, hello transakční programovací jazyk, který je nativně provedený hello databáze

Na základě jeho tooJavaScript hloubkové závazků a JSON přímo v databázovém stroji hello Cosmos DB poskytuje intuitivní programovací model pro provádění JavaScript na základě logiky aplikace přímo na hello kolekce z hlediska uložené procedury a aktivační události. To umožňuje, aby obě hello následující:

* Efektivní provádění souběžnosti řídit, obnovení, automatické indexování grafy objekt JSON hello přímo v databázovém stroji hello
* Přirozeně vyjadřující tok řízení, proměnné rozsahu, přiřazení a integrace primitiv s databázové transakce přímo z hlediska programovací jazyk JavaScript hello zpracování výjimek

logiky Javascriptové Hello registrované na úrovni kolekce potom můžete vydat databázových operací na dokumentech hello hello zadané kolekce. Cosmos DB implicitně hello zabalí JavaScript na základě uložených procedur a aktivačních událostí v rámci vedlejším transakce ACID s izolací snímku mezi dokumenty v rámci kolekce. Hello průběhu vykonávání Pokud hello JavaScript vyvolá výjimku, potom hello celá transakce zrušena. Hello výsledné programovací model je velmi jednoduchý ještě výkonné. Vývojáři JavaScript získat "trvanlivý" programovací model při stále pomocí svého známé jazykové konstrukty a knihovna primitiv.   

Hello možnost tooexecute JavaScript přímo v rámci databáze hello modul v hello stejné adresní prostor fondu vyrovnávací paměti hello umožňuje původce a transakční provádění databázových operací na dokumenty hello kolekce. Kromě toho Cosmos DB databázový stroj usnadňuje hloubkové závazků toohello JSON a JavaScript eliminuje jakákoli neshoda odpor mezi systémy hello typu aplikace a databáze hello.   

Po vytvoření kolekce, můžete zaregistrovat uložené procedury, triggery a UDF s kolekcí pomocí hello [rozhraní REST API](/rest/api/documentdb/) ani v žádné z hello [klientskou sadu SDK](documentdb-sdk-dotnet.md). Po registraci můžete odkazovat a jejich provedení. Zvažte následující hello uložené procedury vytvořené zcela v JavaScriptu, následující kód hello má dva argumenty (název adresáře a jméno autora) a vytvoří nový dokument, dotazuje pro dokument a pak ho – vše v rámci implicitní transakci ACID aktualizuje. Kdykoli během provádění hello Pokud je vyvolána výjimka JavaScript, celá transakce hello zruší.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Hello klienta se dají "dodávat" hello výše JavaScript logiku toohello databáze pro spouštění transakcí přes HTTP POST. Další informace o použití metod HTTP najdete v tématu [RESTful interakce s prostředky Azure Cosmos DB](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Všimněte si, že vzhledem k tomu, že databáze hello nativně funguje s technologií JSON a JavaScript, neexistuje žádný systém Neshoda typu, "OR mapování" nebo magic generování kódu vyžaduje.   

Uložené procedury a triggery komunikovat s kolekce a hello dokumenty v kolekci prostřednictvím dobře definovaný objekt modelu, který zveřejňuje hello aktuální kontext kolekce.  

Kolekce v hello DocumentDB API lze vytvořit, odstranit, číst, nebo ve výčtu snadno pomocí buď hello [rozhraní REST API](/rest/api/documentdb/) ani v žádné z hello [klientskou sadu SDK](documentdb-sdk-dotnet.md). Hello DocumentDB API vždy poskytuje silnou konzistenci pro čtení nebo dotazování hello metadata kolekce. Odstranění kolekce automaticky zajistí, že nemůžete použít žádnou hello dokumenty, přílohy, uložené procedury, triggery a jsou v něm obsažena UDF.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Uložené procedury, triggery a uživatel definované funkce (UDF)
Jak je popsáno v předchozí části hello, můžete napsat aplikaci logiky toorun přímo v rámci transakce uvnitř hello databázového stroje. Hello aplikace logiky můžete vytvořené zcela v JavaScriptu a můžete modelován jako uložené procedury, aktivační události nebo uživatelem definovanou FUNKCI. Hello kódu jazyka JavaScript v rámci uložené procedury nebo aktivační událost můžete vložit, nahradí, odstranit, číst nebo dotaz, dokumenty v rámci kolekce. Na dobrý den druhé straně, hello JavaScript v rámci uživatelem definovanou FUNKCI nelze vložit, nahradí nebo odstranit dokumenty. Funkce UDF výčet hello dokumenty sady výsledků dotazu a vytvořit jinou sadu výsledků. Víceklientský Cosmos DB vynucuje zásady správného řízení striktní rezervace na základě prostředků. Každý uložené procedury, aktivační události nebo uživatelem definovanou FUNKCI získá pevné množství toodo prostředky operačního systému svou práci. Kromě toho uložené procedury, aktivační události nebo UDF nemůže propojit s externí knihovny jazyka JavaScript a jsou zakázány, pokud se překročí rozpočty prostředků hello přidělené toothem. Můžete zaregistrovat, zrušit registraci uložené procedury, aktivační události nebo UDF s kolekcí pomocí hello rozhraní REST API.  Po registraci se uložené procedury, aktivační události nebo uživatelem definovanou FUNKCI předem zkompilovat a uložené jako bajtové kód, který se provede později. Následující části Hello znázorňují, jak můžete použít tooregister hello Cosmos DB JavaScript SDK, spouštění a zrušit registraci uložené procedury, aktivační události a UDF. Hello JavaScript SDK je jednoduché obálku nad hello [rozhraní REST API](/rest/api/documentdb/). 

### <a name="registering-a-stored-procedure"></a>Registrace uložené procedury
Registrace uložená procedura vytvoří nový prostředek uložené procedury na kolekci přes HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Provedení uložené procedury
Provádění uložené procedury se provádí po vydání požadavku HTTP POST na existující prostředek uložené procedury předáním parametry postupu toohello v textu žádosti hello.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Zrušení registrace uložené procedury
Zrušení registrace uložené procedury je jednoduše potřeba vydání HTTP DELETE se proti existující prostředek uložené procedury.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registrace předběžné aktivační událost
Registrace aktivační událost se provádí tak, že vytvoříte nový prostředek aktivační události u kolekce přes HTTP POST. Můžete zadat, pokud je aktivační událost hello po předem nebo post aktivační události a hello typ operace může být přidružen (například vytvořit, Replace, Delete nebo všechny).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Provádění aktivační události starší než
Provádění aktivační události je potřeba zadat název hello existující aktivační události v době hello vydáním hello požadavku POST, PUT nebo odstranění prostředku dokumentu prostřednictvím hlavička požadavku hello.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in hello Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Zrušení registrace předběžné aktivační událost
Zrušení registrace aktivační událost se jednoduše provádí prostřednictvím vydání HTTP DELETE se proti existující prostředek aktivační události.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registrace UDF
Registrace UDF se provádí tak, že vytvoříte nový prostředek UDF na kolekci přes HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-hello-query"></a>Provádění UDF jako součást dotazu hello
Uživatelem definovanou FUNKCI lze zadat jako součást dotazu SQL hello a slouží jako způsob tooextend hello jádro [dotazovací jazyk SQL pro hello DocumentDB API](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Zrušení registrace UDF
Zrušení registrace UDF jednoduše provádí vydáním HTTP DELETE se proti existující prostředek UDF.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

I když výše uvedené fragmenty kódu hello vám ukázal, registrace hello (POST), zrušení registrace (PUT), pro čtení nebo jejich výpisu (GET) a provádění (POST) prostřednictvím hello [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), můžete použít také hello [rozhraní REST API](/rest/api/documentdb/) nebo jiných [klientskou sadu SDK](documentdb-sdk-dotnet.md). 

## <a name="documents"></a>Dokumenty
Můžete vložit, nahradí, odstranit, číst, výčet a dotaz na libovolné dokumenty JSON v kolekci. Cosmos DB nenutí žádné schéma a nevyžaduje sekundární indexy v pořadí toosupport dotazování s dokumenty v kolekci. Hello maximální velikost pro dokument je 2 MB.   

Probíhá skutečně otevřenou databázi služby, Cosmos DB není skladová žádné speciální datové typy (např. datum čas) nebo konkrétní kódování pro dokumenty JSON. Všimněte si, že Cosmos DB nevyžaduje žádné speciální JSON konvence toocodify hello vztahy mezi různé dokumenty; Syntaxe Hello SQL Cosmos DB poskytuje velmi výkonný dotazu hierarchické a relační operátory tooquery a projekt dokumenty bez jakékoli speciální poznámky nebo nutnost toocodify vztahy mezi dokumenty pomocí vlastnosti rozlišujícího.  

Jak se všemi ostatními prostředky mohou být vytvořeny dokumenty, nahradit, odstranit, číst, výčet a dotazovat snadno pomocí rozhraní REST API nebo některou z hello [klientskou sadu SDK](documentdb-sdk-dotnet.md). Odstranění dokumentu okamžitě uvolní hello kvóty odpovídající tooall hello vnořené příloh. Hello čtení úroveň konzistence dokumentů odpovídá hello konzistence zásad na hello databázového účtu. Tuto zásadu lze přepsat na základě požadavků v závislosti na požadavcích konzistence dat vaší aplikace. Při dotazování dokumentů, přečtěte si hello hello způsobem konzistence indexování režim nastavený na kolekci hello. Pro "konzistentní" což odpovídá hello účet konzistence zásad. 

## <a name="attachments-and-media"></a>Přílohy a média
Cosmos DB vám umožní toostore binární objekty BLOB/média pomocí Cosmos DB (maximum 2 GB každý účet) nebo tooyour vlastní médium vzdáleného úložiště. Můžete taky toorepresent hello metadata médií z hlediska speciální dokument s názvem přílohy. Přílohy v databázi Cosmos je speciální dokument (JSON), který odkazuje na hello média nebo objekt blob uložená na jiném místě. Příloha je jednoduše speciální dokument, který zachycuje hello metadata (např. umístění, Autor atd.) médií uložených v médium vzdáleného úložiště. 

Zvažte sociálních čtení aplikace, která používá Cosmos DB toostore neobsahovaly a metadata včetně komentář, označuje záložky, hodnocení, líbí nebo nelíbí atd přidružené pro zde elektronickou knihu daného uživatele.   

* Hello hello knihy, samotné je obsah uložený v úložišti média hello buď jako součást Cosmos DB databázového účtu nebo médium vzdáleného úložiště k dispozici. 
* Aplikace může ukládat metadata každého uživatele jako odlišné dokument – například Michalův metadata pro Sešit1 se ukládají v dokumentu odkazuje /colls/joe/docs/book1. 
* Přílohy odkazující toohello stránkách obsahu daného adresáře uživatele, jsou uloženy v dokumentu odpovídající hello například /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 atd. 

Všimněte si, že uvedené příklady hello použijte popisný ID tooconvey hello prostředků hierarchie. Jsou přístup k prostředkům prostřednictvím hello rozhraní REST API prostřednictvím ID prostředků jedinečné. 

Pro hello média, který je spravovaný nástrojem Cosmos DB bude vlastnost _media hello hello přílohy odkazovat hello média pomocí jeho identifikátoru URI. Cosmos DB zajistí toogarbage shromažďování hello média, když všechny zbývající odkazy hello vyřadit. Cosmos DB automaticky generuje hello přílohy při nahrávání hello nové médium a naplní hello _media toopoint toohello nově přidané média. Pokud si zvolíte toostore hello média v úložišti objektů blob vzdálené spravované vámi (například OneDrive, Azure Storage, DropBox atd), můžete stále přílohy tooreference hello média. V tomto případě vytvoříte hello přílohy sami a naplnit její _media vlastnost.   

Stejně jako u všech ostatních prostředků přílohy lze vytvořit, nahradit, odstranit, čtení a vytvořit její výčet snadno pomocí rozhraní REST API nebo některou z hello klientskou sadu SDK. Jako s dokumenty, hello číst úroveň konzistence příloh odpovídá hello konzistence zásad na hello databázového účtu. Tuto zásadu lze přepsat na základě požadavků v závislosti na požadavcích konzistence dat vaší aplikace. Při dotazování pro přílohy, přečtěte si hello hello způsobem konzistence indexování režim nastavený na kolekci hello. Pro "konzistentní" což odpovídá hello účet konzistence zásad. 
 

## <a name="users"></a>Uživatelé
Uživatel Cosmos DB představuje logické obor názvů pro seskupování oprávnění. Cosmos DB uživatel může odpovídat tooa uživatele v systému správy protokolu identity nebo předdefinované aplikační role. Pro databáze Cosmos uživatel jednoduše představuje abstrakce toogroup sadou oprávnění v databázi.   

Pro implementaci víceklientský ve vaší aplikaci, můžete vytvořit uživatele v Cosmos DB, který odpovídá skutečným uživatele tooyour nebo hello klientům aplikace. Potom můžete vytvořit oprávnění pro daného uživatele, které odpovídají toohello řízení přístupu prostřednictvím různých kolekcí, dokumentů, přílohy, atd.   

Jak vaše aplikace potřebují tooscale s růstu vaší uživatele, může přijmout různé způsoby tooshard vaše data. Můžete model každému uživateli následujícím způsobem:   

* Každý uživatel mapuje tooa databáze.
* Každý uživatel mapuje tooa kolekce. 
* Dokumenty odpovídající uživatelé toomultiple přejděte tooa vyhrazené kolekce. 
* Dokumenty odpovídající uživatelé toomultiple přejděte tooa sady kolekcí.   

Bez ohledu na to strategie konkrétní horizontálního dělení hello, který zvolíte můžete model skutečné uživatelům jako uživatele v databázi Cosmos DB a přidružit uživatele tooeach jemně odstupňovaná oprávnění.  

![Kolekce uživatelů][3]  
**Strategie horizontálního dělení a uživatelé modelování**

Podobně jako všechny ostatní prostředky uživatelé v Cosmos DB lze vytvořit, nahradit, odstranit, číst nebo uvedené snadno pomocí rozhraní REST API nebo některou z hello klientskou sadu SDK. Cosmos DB vždy poskytuje silnou konzistenci pro čtení nebo dotazování hello metadata prostředek uživatele. Je vhodné ukazující, že odstranění uživatele automaticky zajistí, že nemůžete použít žádnou hello oprávnění jsou v něm obsažena. I když hello Cosmos DB získá hello kvóty hello oprávnění v rámci hello odstranit uživatele na pozadí hello, hello odstranit oprávnění jsou k dispozici okamžitě znovu pro toouse můžete.  

## <a name="permissions"></a>Oprávnění
Z hlediska řízení k přístupu, prostředkům, například účty databáze, databáze, uživatelů a oprávnění jsou považovány za *správu* prostředky, protože vyžadují oprávnění správce. Na hello druhé straně, včetně hello kolekcí, dokumentů, přílohy, uložené procedury, triggery, prostředky a funkce UDF jsou obor v části na danou databázi a považovány za *prostředky aplikace*. Odpovídající toohello dva typy prostředků a hello role, které přistupovat k nim (konkrétně hello správce a uživatele), modelu autorizace hello definuje dva typy *přístupové klíče*: *hlavní klíč* a *klíč prostředku*. Hello hlavní klíč je součástí hello databázového účtu a je k dispozici toohello developer (nebo správce) kdo je zřizování hello databázového účtu. Tento hlavní klíč musí správce sémantiku v, aby bylo možné ho použít tooauthorize přístup tooboth pro správu a prostředky aplikace. Klíč prostředku spočívá v tom, podrobné přístupový klíč, který umožňuje přístup tooa *konkrétní* prostředků aplikace. Proto zachycení hello vztah mezi hello uživatele databáze a hello oprávnění hello uživatelem pro konkrétní zdroje (například kolekce, dokument, přílohy, uložené procedury, aktivační události nebo UDF).   

Hello pouze způsob tooobtain je klíč prostředků tak, že vytvoříte prostředek oprávnění v rámci daného uživatele. Všimněte si, že se v pořadí toocreate nebo získat oprávnění, hlavní klíč musí být uvedené v hlavičce autorizace hello. Oprávnění prostředků ties hello prostředek, jeho přístup a hello uživatele. Po vytvoření oprávnění prostředku, musí uživatel hello pouze toopresent hello přidružených prostředků klíč v pořadí toogain přístup toohello relevantní prostředku. Klíč prostředku proto lze zobrazit jako znázornění logické a compact hello oprávnění prostředku.  

Stejně jako u všech ostatních prostředků oprávnění v databázi Cosmos lze vytvořit, nahradit, odstranit, čtení a vytvořit její výčet snadno pomocí rozhraní REST API nebo některou z hello klientskou sadu SDK. Cosmos DB vždy poskytuje silnou konzistenci pro čtení nebo dotazování hello metadata oprávnění. 

## <a name="next-steps"></a>Další kroky
Další informace o práci s prostředky pomocí příkazů HTTP v [RESTful interakce s prostředky Cosmos DB](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

