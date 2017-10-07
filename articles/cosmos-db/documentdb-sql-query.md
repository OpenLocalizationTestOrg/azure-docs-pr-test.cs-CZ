---
title: "aaaSQL dotazy pro rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
description: "Další informace o syntaxi jazyka SQL, databáze koncepty a dotazy SQL pro Azure Cosmos DB. SQL lze použít jako dotazovací jazyk JSON v Azure Cosmos DB."
keywords: "syntaxe SQL, dotaz sql, sql dotazy, json dotazovací jazyk, databázových koncepcí a sql, agregační funkce"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: f4db95b87f5796c4e4299aaf016435cb6301bbfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="f6dd2-105">Dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="f6dd2-105">SQL queries for Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="f6dd2-106">Microsoft Azure Cosmos DB podporuje dotazování dokumentů pomocí jazyka SQL (Structured Query Language) jako dotazovací jazyk JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-106">Microsoft Azure Cosmos DB supports querying documents using SQL (Structured Query Language) as a JSON query language.</span></span> <span data-ttu-id="f6dd2-107">Cosmos DB je skutečně bez schémat.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-107">Cosmos DB is truly schema-free.</span></span> <span data-ttu-id="f6dd2-108">Na základě jeho závazků toohello datového modelu JSON přímo v rámci hello databázový stroj poskytuje automatické indexování dokumentů JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-108">By virtue of its commitment toohello JSON data model directly within hello database engine, it provides automatic indexing of JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> 

<span data-ttu-id="f6dd2-109">Při navrhování hello dotazovacího jazyka pro Cosmos DB, jsme měli dva cíle v paměti:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-109">While designing hello query language for Cosmos DB, we had two goals in mind:</span></span>

* <span data-ttu-id="f6dd2-110">Místo inventing o nový jazyk dotazů JSON, jsme chtěli toosupport SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-110">Instead of inventing a new JSON query language, we wanted toosupport SQL.</span></span> <span data-ttu-id="f6dd2-111">SQL je jedním z jazyků hello nejvíce známé a oblíbených dotazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-111">SQL is one of hello most familiar and popular query languages.</span></span> <span data-ttu-id="f6dd2-112">SQL databáze cosmos umožňuje formální programovací model o bohaté dotazy prostřednictvím dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-112">Cosmos DB SQL provides a formal programming model for rich queries over JSON documents.</span></span>
* <span data-ttu-id="f6dd2-113">Jako dokument databáze JSON může provést JavaScript přímo v databázovém stroji hello jsme chtěli toouse JavaScript programovací model jako hello foundation pro naše dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-113">As a JSON document database capable of executing JavaScript directly in hello database engine, we wanted toouse JavaScript's programming model as hello foundation for our query language.</span></span> <span data-ttu-id="f6dd2-114">Hello DocumentDB SQL rozhraní API je integrován do systému typů JavaScript na vyhodnocení výrazu a volání funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-114">hello DocumentDB API SQL is rooted in JavaScript's type system, expression evaluation, and function invocation.</span></span> <span data-ttu-id="f6dd2-115">Tato naopak poskytuje přirozené programovací model pro projekce relačních, hierarchických navigace mezi dokumenty JSON, vlastní spojení, prostorových dotazů a vyvolání uživatelem definovaných funkcí (UDF) vytvořené zcela v JavaScriptu mezi dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-115">This in-turn provides a natural programming model for relational projections, hierarchical navigation across JSON documents, self joins, spatial queries, and invocation of user-defined functions (UDFs) written entirely in JavaScript, among other features.</span></span> 

<span data-ttu-id="f6dd2-116">Věříme, že tyto funkce jsou klíče tooreducing hello tření mezi hello databázové a aplikační hello a jsou zásadní pro produktivita vývojářů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-116">We believe that these capabilities are key tooreducing hello friction between hello application and hello database and are crucial for developer productivity.</span></span>

<span data-ttu-id="f6dd2-117">Doporučujeme začít hello následující video, kde Aravind Ramachandran zobrazí možnosti dotazování Cosmos DB, sledování a navštívíte naše [Query Playground](http://www.documentdb.com/sql/demo), kde můžete vyzkoušet Cosmos DB a spouštět dotazy SQL pro naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-117">We recommend getting started by watching hello following video, where Aravind Ramachandran shows Cosmos DB's querying capabilities, and by visiting our [Query Playground](http://www.documentdb.com/sql/demo), where you can try out Cosmos DB and run SQL queries against our dataset.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

<span data-ttu-id="f6dd2-118">Pak se vraťte toothis článku, kde Začneme s kurz dotaz SQL, který vás provede některé jednoduché dokumentů JSON a příkazy SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-118">Then, return toothis article, where we start with a SQL query tutorial that walks you through some simple JSON documents and SQL commands.</span></span>

## <span data-ttu-id="f6dd2-119"><a id="GettingStarted"></a>Začínáme s příkazy SQL v databázi systému Cosmos</span><span class="sxs-lookup"><span data-stu-id="f6dd2-119"><a id="GettingStarted"></a>Getting started with SQL commands in Cosmos DB</span></span>
<span data-ttu-id="f6dd2-120">toosee Cosmos SQL databáze v práci, umožňuje začínat několik jednoduchých dokumentů JSON a provede několik jednoduchých dotazů u ní.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-120">toosee Cosmos DB SQL at work, let's begin with a few simple JSON documents and walk through some simple queries against it.</span></span> <span data-ttu-id="f6dd2-121">Vezměte v úvahu tyto dva dokumenty JSON o dvou řad.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-121">Consider these two JSON documents about two families.</span></span> <span data-ttu-id="f6dd2-122">S Cosmos DB jsme nemusí toocreate všechny schémata nebo sekundárních indexů explicitně.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-122">With Cosmos DB, we do not need toocreate any schemas or secondary indices explicitly.</span></span> <span data-ttu-id="f6dd2-123">Můžeme jednoduše potřebovat tooinsert hello JSON dokumentů tooa Cosmos DB kolekce a následně dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-123">We simply need tooinsert hello JSON documents tooa Cosmos DB collection and subsequently query.</span></span> <span data-ttu-id="f6dd2-124">Tady bychom měli jednoduché JSON dokumentů pro hello rodinu, hello nadřazených položek, děti (a jejich mazlíčků), adresu a informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-124">Here we have a simple JSON document for hello Andersen family, hello parents, children (and their pets), address, and registration information.</span></span> <span data-ttu-id="f6dd2-125">Hello dokumentu je řetězců, čísel, logické hodnoty, pole a vnořené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-125">hello document has strings, numbers, Booleans, arrays, and nested properties.</span></span> 

<span data-ttu-id="f6dd2-126">**Dokument**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-126">**Document**</span></span>  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

<span data-ttu-id="f6dd2-127">Tady je druhý dokument s jedním jemně rozdílem – `givenName` a `familyName` se používají místo `firstName` a `lastName`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-127">Here's a second document with one subtle difference – `givenName` and `familyName` are used instead of `firstName` and `lastName`.</span></span>

<span data-ttu-id="f6dd2-128">**Dokument**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-128">**Document**</span></span>  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

<span data-ttu-id="f6dd2-129">Nyní nyní si vyzkoušíte několik dotazů vůči tato data toounderstand některé hello klíče aspektů DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-129">Now let's try a few queries against this data toounderstand some of hello key aspects of DocumentDB API SQL.</span></span> <span data-ttu-id="f6dd2-130">Například hello následující dotaz vrátí hello dokumenty, kde pole id hello odpovídá `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-130">For example, hello following query returns hello documents where hello id field matches `AndersenFamily`.</span></span> <span data-ttu-id="f6dd2-131">Vzhledem k tomu, že je `SELECT *`, výstup hello hello dotazu je hello dokončení dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-131">Since it's a `SELECT *`, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="f6dd2-132">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-132">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-133">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-133">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


<span data-ttu-id="f6dd2-134">Teď se podíváme hello případ potřebujeme tooreformat hello výstup JSON v různých obrazce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-134">Now consider hello case where we need tooreformat hello JSON output in a different shape.</span></span> <span data-ttu-id="f6dd2-135">Tento dotaz, zda projekty nové JSON objektu s dvě vybrané pole název a města, když hello adresa město má hello stejný název jako hello stavu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-135">This query projects a new JSON object with two selected fields, Name and City, when hello address' city has hello same name as hello state.</span></span> <span data-ttu-id="f6dd2-136">V tomto případě "NY, NY" odpovídá.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-136">In this case, "NY, NY" matches.</span></span>

<span data-ttu-id="f6dd2-137">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-137">**Query**</span></span>    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

<span data-ttu-id="f6dd2-138">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-138">**Results**</span></span>

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


<span data-ttu-id="f6dd2-139">Hello další dotaz vrátí všechny názvy daným hello podřízených prvků řady hello shoduje s id `WakefieldFamily` seřazené podle města hello pobytu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-139">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by hello city of residence.</span></span>

<span data-ttu-id="f6dd2-140">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-140">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

<span data-ttu-id="f6dd2-141">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-141">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


<span data-ttu-id="f6dd2-142">Rádi bychom znali tooa pozornost toodraw několik pozoruhodné aspektů hello Cosmos DB dotazu jazyka prostřednictvím hello příklady, které jste viděli, pokud:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-142">We would like toodraw attention tooa few noteworthy aspects of hello Cosmos DB query language through hello examples we've seen so far:</span></span>  

* <span data-ttu-id="f6dd2-143">Vzhledem k tomu, že DocumentDB SQL rozhraní API funguje na hodnoty JSON, zabývá stromu ve tvaru entity místo řádků a sloupců.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-143">Since DocumentDB API SQL works on JSON values, it deals with tree shaped entities instead of rows and columns.</span></span> <span data-ttu-id="f6dd2-144">Proto hello jazyk umožňuje naleznete toonodes hello stromu při jakékoli libovolný hloubce, jako je třeba `Node1.Node2.Node3…..Nodem`, podobně jako toorelational SQL odkazující toohello dvě části odkaz `<table>.<column>`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-144">Therefore, hello language lets you refer toonodes of hello tree at any arbitrary depth, like `Node1.Node2.Node3…..Nodem`, similar toorelational SQL referring toohello two part reference of `<table>.<column>`.</span></span>   
* <span data-ttu-id="f6dd2-145">Hello strukturovaná dotazu jazyka funguje s daty bez schématu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-145">hello structured query language works with schema-less data.</span></span> <span data-ttu-id="f6dd2-146">Proto dynamicky hello typ systému potřebám toobe hranice.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-146">Therefore, hello type system needs toobe bound dynamically.</span></span> <span data-ttu-id="f6dd2-147">Hello stejný výraz může přinést různých typů na různé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-147">hello same expression could yield different types on different documents.</span></span> <span data-ttu-id="f6dd2-148">Hello výsledek dotazu není platná hodnota JSON, ale není zaručena toobe pevného schématu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-148">hello result of a query is a valid JSON value, but is not guaranteed toobe of a fixed schema.</span></span>  
* <span data-ttu-id="f6dd2-149">Cosmos databáze podporuje pouze striktní dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-149">Cosmos DB only supports strict JSON documents.</span></span> <span data-ttu-id="f6dd2-150">To znamená, že systém typů hello a výrazy s omezeným přístupem toodeal jenom s typy JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-150">This means hello type system and expressions are restricted toodeal only with JSON types.</span></span> <span data-ttu-id="f6dd2-151">Odkazovat toohello [JSON specifikace](http://www.json.org/) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-151">Refer toohello [JSON specification](http://www.json.org/) for more details.</span></span>  
* <span data-ttu-id="f6dd2-152">Cosmos DB kolekce je kontejner dokumentů JSON bez schémat.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-152">A Cosmos DB collection is a schema-free container of JSON documents.</span></span> <span data-ttu-id="f6dd2-153">Hello vztahy v datových entit v rámci a na dokumentech v kolekci jsou implicitně zaznamenat členství ve skupině a ne primárního a cizího klíče relace.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-153">hello relations in data entities within and across documents in a collection are implicitly captured by containment and not by primary key and foreign key relations.</span></span> <span data-ttu-id="f6dd2-154">Toto je důležitým aspektem vhodné odkazující na základě spojení intra-document hello probírat později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-154">This is an important aspect worth pointing out in light of hello intra-document joins discussed later in this article.</span></span>

## <span data-ttu-id="f6dd2-155"><a id="Indexing"></a>Indexování cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f6dd2-155"><a id="Indexing"></a> Cosmos DB indexing</span></span>
<span data-ttu-id="f6dd2-156">Než se nám získat do hello syntaxi DocumentDB SQL rozhraní API, je vhodné využít hello indexování návrhu v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-156">Before we get into hello DocumentDB API SQL syntax, it is worth exploring hello indexing design in Cosmos DB.</span></span> 

<span data-ttu-id="f6dd2-157">účelem Hello indexy databáze je tooserve dotazy v různých formách a obrazců pomocí spotřeby minimální prostředků (např. využití procesoru a vstup/výstup) současně poskytují dobrý prostupnosti a nízké latence.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-157">hello purpose of database indexes is tooserve queries in their various forms and shapes with minimum resource consumption (like CPU and input/output) while providing good throughput and low latency.</span></span> <span data-ttu-id="f6dd2-158">Často hello Volba správného indexu hello k dotazování databáze vyžaduje mnohem plánování a experimenty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-158">Often, hello choice of hello right index for querying a database requires much planning and experimentation.</span></span> <span data-ttu-id="f6dd2-159">Tento přístup představuje výzvu pro bez schématu databáze, kde hello data neodpovídají schématu striktní tooa a zpracovaní rychle.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-159">This approach poses a challenge for schema-less databases where hello data doesn’t conform tooa strict schema and evolves rapidly.</span></span> 

<span data-ttu-id="f6dd2-160">Proto když jsme navržený hello Cosmos DB indexování subsystému, nastaví hello následující cíle:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-160">Therefore, when we designed hello Cosmos DB indexing subsystem, we set hello following goals:</span></span>

* <span data-ttu-id="f6dd2-161">Indexování dokumentů bez nutnosti schématu: hello indexování subsystému nevyžaduje žádné informace o schématu ani vytvořit žádný odhad o schématu hello dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-161">Index documents without requiring schema: hello indexing subsystem does not require any schema information or make any assumptions about schema of hello documents.</span></span> 
* <span data-ttu-id="f6dd2-162">Podpora pro efektivní, bohaté hierarchické a relační dotazy: hello index podporuje hello Cosmos databáze dotazovací jazyk efektivně, včetně podpory pro hierarchické a relační projekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-162">Support for efficient, rich hierarchical, and relational queries: hello index supports hello Cosmos DB query language efficiently, including support for hierarchical and relational projections.</span></span>
* <span data-ttu-id="f6dd2-163">Podpora pro konzistentní dotazy in face of svazek dlouhodobě zápisů: pro zápisu vysokou propustnost úlohy s konzistentní dotazy, hello aktualizace indexu postupně, efektivně a online hello stěně dlouhodobě svazku zápisů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-163">Support for consistent queries in face of a sustained volume of writes: For high write throughput workloads with consistent queries, hello index is updated incrementally, efficiently, and online in hello face of a sustained volume of writes.</span></span> <span data-ttu-id="f6dd2-164">aktualizace konzistentní index Hello je zásadní tooserve hello dotazy na úrovni konzistence hello v které hello uživateli nakonfigurovanému hello dokumentu služby.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-164">hello consistent index update is crucial tooserve hello queries at hello consistency level in which hello user configured hello document service.</span></span>
* <span data-ttu-id="f6dd2-165">Podpora pro více klientů: zadána hello založené na vyhrazené modelu pro řízení prostředků mezi klienty v rámci rozpočtu hello systémových prostředků (procesoru, paměti a vstupně-výstupních operací za sekundu) přidělený na repliky jsou provedeny aktualizace indexu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-165">Support for multi-tenancy: Given hello reservation-based model for resource governance across tenants, index updates are performed within hello budget of system resources (CPU, memory, and input/output operations per second) allocated per replica.</span></span> 
* <span data-ttu-id="f6dd2-166">Efektivitu úložiště: pro finanční efektivita je hello na disk úložiště režie hello indexu ohraničené a předvídatelné.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-166">Storage efficiency: For cost effectiveness, hello on-disk storage overhead of hello index is bounded and predictable.</span></span> <span data-ttu-id="f6dd2-167">Toto je velmi důležitý, protože Cosmos DB vývojáře toomake náklady na základě kompromisy mezi režijní náklady na index výkonnosti dotazu toohello vztah umožňuje hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-167">This is crucial because Cosmos DB allows hello developer toomake cost-based tradeoffs between index overhead in relation toohello query performance.</span></span>  

<span data-ttu-id="f6dd2-168">Odkazovat toohello [Azure Cosmos DB – ukázky](https://github.com/Azure/azure-documentdb-net) na webu MSDN ukázek znázorňující, jak tooconfigure hello zásady indexování pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-168">Refer toohello [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) on MSDN for samples showing how tooconfigure hello indexing policy for a collection.</span></span> <span data-ttu-id="f6dd2-169">Nyní Pojďme do hello podrobnosti o hello syntaxe Azure Cosmos DB SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-169">Let’s now get into hello details of hello Azure Cosmos DB SQL syntax.</span></span>

## <span data-ttu-id="f6dd2-170"><a id="Basics"></a>Základní informace o příkazu jazyka Azure Cosmos DB SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-170"><a id="Basics"></a>Basics of an Azure Cosmos DB SQL query</span></span>
<span data-ttu-id="f6dd2-171">Každý dotaz sestává z klauzule SELECT a volitelné FROM a klauzule WHERE za standardy ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-171">Every query consists of a SELECT clause and optional FROM and WHERE clauses per ANSI-SQL standards.</span></span> <span data-ttu-id="f6dd2-172">Pro každý dotaz, obvykle je výčet hello zdroj v klauzuli FROM hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-172">Typically, for each query, hello source in hello FROM clause is enumerated.</span></span> <span data-ttu-id="f6dd2-173">Potom hello filtrovat v hello klauzule WHERE se použije na hello zdroj tooretrieve podmnožinu dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-173">Then hello filter in hello WHERE clause is applied on hello source tooretrieve a subset of JSON documents.</span></span> <span data-ttu-id="f6dd2-174">Nakonec se používá klauzuli SELECT hello tooproject hello požadované hodnoty JSON v hello vybrat seznamu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-174">Finally, hello SELECT clause is used tooproject hello requested JSON values in hello select list.</span></span>

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <span data-ttu-id="f6dd2-175"><a id="FromClause"></a>FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="f6dd2-175"><a id="FromClause"></a>FROM clause</span></span>
<span data-ttu-id="f6dd2-176">Hello `FROM <from_specification>` klauzule je nepovinný, pokud je zdroj hello filtrovat nebo projekci později v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-176">hello `FROM <from_specification>` clause is optional unless hello source is filtered or projected later in hello query.</span></span> <span data-ttu-id="f6dd2-177">účelem Hello tuto klauzuli je zdroj dat hello toospecify, při které hello musí fungovat dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-177">hello purpose of this clause is toospecify hello data source upon which hello query must operate.</span></span> <span data-ttu-id="f6dd2-178">Běžně hello celé kolekce je zdrojem hello, ale jeden místo toho zadat podmnožinu kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-178">Commonly hello whole collection is hello source, but one can specify a subset of hello collection instead.</span></span> 

<span data-ttu-id="f6dd2-179">Dotaz jako `SELECT * FROM Families` označuje, že celou kolekci rodiny hello je přes které tooenumerate hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-179">A query like `SELECT * FROM Families` indicates that hello entire Families collection is hello source over which tooenumerate.</span></span> <span data-ttu-id="f6dd2-180">Zvláštní identifikátor KOŘENOVÉ lze použít toorepresent hello kolekce místo použití hello název kolekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-180">A special identifier ROOT can be used toorepresent hello collection instead of using hello collection name.</span></span> <span data-ttu-id="f6dd2-181">Hello následující seznam obsahuje hello pravidla, které vynucuje na jeden dotaz:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-181">hello following list contains hello rules that are enforced per query:</span></span>

* <span data-ttu-id="f6dd2-182">kolekce Hello je to možné, jako například `SELECT f.id FROM Families AS f` nebo jednoduše `SELECT f.id FROM Families f`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-182">hello collection can be aliased, such as `SELECT f.id FROM Families AS f` or simply `SELECT f.id FROM Families f`.</span></span> <span data-ttu-id="f6dd2-183">Zde `f` je ekvivalentem hello `Families`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-183">Here `f` is hello equivalent of `Families`.</span></span> <span data-ttu-id="f6dd2-184">`AS`je identifikátor hello tooalias optional – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-184">`AS` is an optional keyword tooalias hello identifier.</span></span>
* <span data-ttu-id="f6dd2-185">Jednou alias, nemůže být vázán hello původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-185">Once aliased, hello original source cannot be bound.</span></span> <span data-ttu-id="f6dd2-186">Například `SELECT Families.id FROM Families f` je syntakticky neplatný, protože už nelze přeložit identifikátor hello "Rodiny".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-186">For example, `SELECT Families.id FROM Families f` is syntactically invalid since hello identifier "Families" cannot be resolved anymore.</span></span>
* <span data-ttu-id="f6dd2-187">Všechny vlastnosti, které je třeba toobe odkazuje musí být plně kvalifikovaný.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-187">All properties that need toobe referenced must be fully qualified.</span></span> <span data-ttu-id="f6dd2-188">V hello chybí dodržování striktní schématu, je to vynucené tooavoid žádné nejednoznačný vazby.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-188">In hello absence of strict schema adherence, this is enforced tooavoid any ambiguous bindings.</span></span> <span data-ttu-id="f6dd2-189">Proto `SELECT id FROM Families f` je syntakticky neplatný od hello vlastnost `id` není vázán.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-189">Therefore, `SELECT id FROM Families f` is syntactically invalid since hello property `id` is not bound.</span></span>

### <a name="subdocuments"></a><span data-ttu-id="f6dd2-190">Vnořené dokumenty</span><span class="sxs-lookup"><span data-stu-id="f6dd2-190">Subdocuments</span></span>
<span data-ttu-id="f6dd2-191">Hello zdroj může být také snížené tooa menší podmnožinu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-191">hello source can also be reduced tooa smaller subset.</span></span> <span data-ttu-id="f6dd2-192">Například tooenumerating pouze podstrom v každém dokumentu hello subroot může pak mohou stát hello zdroje, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-192">For instance, tooenumerating only a subtree in each document, hello subroot could then become hello source, as shown in hello following example:</span></span>

<span data-ttu-id="f6dd2-193">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-193">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="f6dd2-194">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-194">**Results**</span></span>  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

<span data-ttu-id="f6dd2-195">Při hello výše příklad pole jako zdroj hello, objekt mohou být využity také jako hello zdroj, který je informace zobrazené v hello následující ukázka: žádné platná hodnota JSON (nedefinovaná), můžete najít ve zdroji hello je považován za pro zařazení výsledek hello dotaz Hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-195">While hello above example used an array as hello source, an object could also be used as hello source, which is what's shown in hello following example: Any valid JSON value (not undefined) that can be found in hello source is considered for inclusion in hello result of hello query.</span></span> <span data-ttu-id="f6dd2-196">Pokud nemáte některé rodiny `address.state` hodnotu, jsou vyloučeny ve výsledku dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-196">If some families don’t have an `address.state` value, they are excluded in hello query result.</span></span>

<span data-ttu-id="f6dd2-197">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-197">**Query**</span></span>

    SELECT * 
    FROM Families.address.state

<span data-ttu-id="f6dd2-198">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-198">**Results**</span></span>

    [
      "WA", 
      "NY"
    ]


## <span data-ttu-id="f6dd2-199"><a id="WhereClause"></a>Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="f6dd2-199"><a id="WhereClause"></a>WHERE clause</span></span>
<span data-ttu-id="f6dd2-200">klauzule WHERE Hello (**`WHERE <filter_condition>`**) je volitelný.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-200">hello WHERE clause (**`WHERE <filter_condition>`**) is optional.</span></span> <span data-ttu-id="f6dd2-201">Určuje, zda text hello, podmínku (podmínky), musí splňovat dokumentů JSON hello poskytované hello zdroje v pořadí toobe jako součást výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-201">It specifies hello condition(s) that hello JSON documents provided by hello source must satisfy in order toobe included as part of hello result.</span></span> <span data-ttu-id="f6dd2-202">Dokumentu JSON se musí vyhodnotit hello zadané podmínky příliš "true" toobe považována za výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-202">Any JSON document must evaluate hello specified conditions too"true" toobe considered for hello result.</span></span> <span data-ttu-id="f6dd2-203">použití klauzule vrstvou hello indexu v pořadí toodetermine hello absolutní nejmenší podmnožinu dokumentů zdroje, které můžou být součástí hello výsledek Hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-203">hello WHERE clause is used by hello index layer in order toodetermine hello absolute smallest subset of source documents that can be part of hello result.</span></span> 

<span data-ttu-id="f6dd2-204">Hello následující dotaz požadavků dokumentů, které obsahují název vlastnosti, jehož hodnota je `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-204">hello following query requests documents that contain a name property whose value is `AndersenFamily`.</span></span> <span data-ttu-id="f6dd2-205">Jiného dokumentu, který nemá název vlastnosti, nebo kde hello hodnota neodpovídá `AndersenFamily` je vyloučen.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-205">Any other document that does not have a name property, or where hello value does not match `AndersenFamily` is excluded.</span></span> 

<span data-ttu-id="f6dd2-206">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-206">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-207">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-207">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


<span data-ttu-id="f6dd2-208">Hello předchozí příklad ukázal dotazu jednoduché rovnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-208">hello previous example showed a simple equality query.</span></span> <span data-ttu-id="f6dd2-209">DocumentDB SQL rozhraní API také podporuje celou řadu skalární výrazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-209">DocumentDB API SQL also supports a variety of scalar expressions.</span></span> <span data-ttu-id="f6dd2-210">Hello nejčastěji používaná jsou binární a unární výrazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-210">hello most commonly used are binary and unary expressions.</span></span> <span data-ttu-id="f6dd2-211">Odkazy na vlastnost z objektu JSON, hello zdroje jsou také platné výrazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-211">Property references from hello source JSON object are also valid expressions.</span></span> 

<span data-ttu-id="f6dd2-212">Hello následující binární operátory jsou aktuálně podporovány a lze je použít v dotazech, jak je znázorněno v hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-212">hello following binary operators are currently supported and can be used in queries as shown in hello following examples:</span></span>  

<table>
<tr>
<td><span data-ttu-id="f6dd2-213">Aritmetické operace</span><span class="sxs-lookup"><span data-stu-id="f6dd2-213">Arithmetic</span></span></td>    
<td><span data-ttu-id="f6dd2-214">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="f6dd2-214">+,-,*,/,%</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f6dd2-215">Bitový</span><span class="sxs-lookup"><span data-stu-id="f6dd2-215">Bitwise</span></span></td>    
<td><span data-ttu-id="f6dd2-216">|, &, ^, <<>>,, >>> (nula výplně posunutí doprava)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-216">|, &, ^, <<, >>, >>> (zero-fill right shift)</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f6dd2-217">Logické</span><span class="sxs-lookup"><span data-stu-id="f6dd2-217">Logical</span></span></td>
<td><span data-ttu-id="f6dd2-218">A, NEBO NE</span><span class="sxs-lookup"><span data-stu-id="f6dd2-218">AND, OR, NOT</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f6dd2-219">Porovnání</span><span class="sxs-lookup"><span data-stu-id="f6dd2-219">Comparison</span></span></td>    
<td><span data-ttu-id="f6dd2-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span><span class="sxs-lookup"><span data-stu-id="f6dd2-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f6dd2-221">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f6dd2-221">String</span></span></td>    
<td><span data-ttu-id="f6dd2-222">|| (zřetězení)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-222">|| (concatenate)</span></span></td>
</tr>
</table>  


<span data-ttu-id="f6dd2-223">Podívejme se na některé dotazy pomocí binární operátory.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-223">Let’s take a look at some queries using binary operators.</span></span>

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


<span data-ttu-id="f6dd2-224">Unární operátory Hello +,-, ~ není jsou podporovány také a dá se použít uvnitř dotazy, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-224">hello unary operators +,-, ~ and NOT are also supported, and can be used inside queries as shown in hello following example:</span></span>

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



<span data-ttu-id="f6dd2-225">Kromě toho toobinary a unární operátory, vlastnost odkazy jsou také povoleny.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-225">In addition toobinary and unary operators, property references are also allowed.</span></span> <span data-ttu-id="f6dd2-226">Například `SELECT * FROM Families f WHERE f.isRegistered` vrátí hello dokumentu JSON obsahující hello vlastnost `isRegistered` kde hodnota vlastnosti hello je rovna toohello JSON `true` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-226">For example, `SELECT * FROM Families f WHERE f.isRegistered` returns hello JSON document containing hello property `isRegistered` where hello property's value is equal toohello JSON `true` value.</span></span> <span data-ttu-id="f6dd2-227">Všechny ostatní hodnoty (false, hodnotu null a nedefinovaná, `<number>`, `<string>`, `<object>`, `<array>`atd) vede k vyloučení z výsledku hello toohello zdrojový dokument.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-227">Any other values (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) leads toohello source document being excluded from hello result.</span></span> 

### <a name="equality-and-comparison-operators"></a><span data-ttu-id="f6dd2-228">Operátory rovnosti a porovnání</span><span class="sxs-lookup"><span data-stu-id="f6dd2-228">Equality and comparison operators</span></span>
<span data-ttu-id="f6dd2-229">Hello následující tabulka uvádí hello výsledek porovnání rovnosti v DocumentDB API SQL mezi všechny dva typy JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-229">hello following table shows hello result of equality comparisons in DocumentDB API SQL between any two JSON types.</span></span>

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-230">
            <strong>OP</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-230">
            <strong>Op</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-231">
            <strong>Nedefinovaná</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-231">
            <strong>Undefined</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-232">
            <strong>Hodnotu Null</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-232">
            <strong>Null</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-233">
            <strong>Logická hodnota</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-233">
            <strong>Boolean</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-234">
            <strong>Číslo</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-234">
            <strong>Number</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-235">
            <strong>Řetězec</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-235">
            <strong>String</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-236">
            <strong>Objekt</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-236">
            <strong>Object</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="f6dd2-237">
            <strong>Pole</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-237">
            <strong>Array</strong>
         </span></span></td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-238">
            <strong>Nedefinovaná<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-238">
            <strong>Undefined<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-239">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-239">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-240">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-240">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-241">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-241">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-242">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-242">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-243">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-243">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-244">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-244">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-245">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-245">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-246">
            <strong>Hodnotu Null<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-246">
            <strong>Null<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-247">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-247">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-248">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-248">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-249">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-249">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-250">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-250">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-251">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-251">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-252">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-252">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-253">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-253">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-254">
            <strong>Logická hodnota<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-254">
            <strong>Boolean<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-255">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-255">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-256">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-256">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-257">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-257">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-258">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-258">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-259">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-259">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-260">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-260">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-261">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-261">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-262">
            <strong>Číslo<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-262">
            <strong>Number<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-263">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-263">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-264">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-264">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-265">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-265">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-266">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-266">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-267">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-267">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-268">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-268">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-269">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-269">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-270">
            <strong>Řetězec<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-270">
            <strong>String<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-271">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-271">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-272">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-272">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-273">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-273">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-274">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-274">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-275">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-275">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-276">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-276">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-277">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-277">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-278">
            <strong>Objekt<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-278">
            <strong>Object<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-279">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-279">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-280">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-280">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-281">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-281">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-282">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-282">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-283">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-283">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-284">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-284">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-285">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-285">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="f6dd2-286">
            <strong>Pole<strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-286">
            <strong>Array<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="f6dd2-287">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-287">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-288">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-288">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-289">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-289">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-290">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-290">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-291">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-291">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="f6dd2-292">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-292">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="f6dd2-293">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="f6dd2-293">
            <strong>OK</strong>
         </span></span></td>
      </tr>
   </tbody>
</table>

<span data-ttu-id="f6dd2-294">Pro jiné operátory porovnání jako >, > =,! =, < a < =, hello platí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-294">For other comparison operators such as >, >=, !=, < and <=, hello following rules apply:</span></span>   

* <span data-ttu-id="f6dd2-295">Výsledkem porovnání mezi typy Undefined.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-295">Comparison across types results in Undefined.</span></span>
* <span data-ttu-id="f6dd2-296">Porovnání mezi dvěma objekty nebo dvě maticových má za následek Undefined.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-296">Comparison between two objects or two arrays results in Undefined.</span></span>   

<span data-ttu-id="f6dd2-297">Pokud je výsledek hello hello skalární výraz, který ve filtru hello Undefined, hello odpovídající dokument není zahrnuta do hello výsledek, protože Undefined není označení rovnosti logicky příliš "true".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-297">If hello result of hello scalar expression in hello filter is Undefined, hello corresponding document would not be included in hello result, since Undefined doesn't logically equate too"true".</span></span>

### <a name="between-keyword"></a><span data-ttu-id="f6dd2-298">MEZI klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="f6dd2-298">BETWEEN keyword</span></span>
<span data-ttu-id="f6dd2-299">Můžete taky hello BETWEEN – klíčové slovo tooexpress dotazy na rozsah hodnot jako v ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-299">You can also use hello BETWEEN keyword tooexpress queries against ranges of values like in ANSI SQL.</span></span> <span data-ttu-id="f6dd2-300">MEZI můžete použít u řetězců nebo čísla.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-300">BETWEEN can be used against strings or numbers.</span></span>

<span data-ttu-id="f6dd2-301">Například tento dotaz vrací všechny rodiny dokumenty, ve které hello úrovni prvním podřízeným objektem je mezi 1-5 (obě včetně).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-301">For example, this query returns all family documents in which hello first child's grade is between 1-5 (both inclusive).</span></span> 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

<span data-ttu-id="f6dd2-302">Na rozdíl od v ANSI SQL, můžete taky klauzule BETWEEN hello v klauzuli FROM hello jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-302">Unlike in ANSI-SQL, you can also use hello BETWEEN clause in hello FROM clause like in hello following example.</span></span>

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

<span data-ttu-id="f6dd2-303">Kratší časy spuštění dotazu mějte na paměti toocreate indexování zásad, která používá typ indexu rozsah proti jakékoli číselné vlastnosti nebo cesty, které jsou filtrovány v klauzuli BETWEEN hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-303">For faster query execution times, remember toocreate an indexing policy that uses a range index type against any numeric properties/paths that are filtered in hello BETWEEN clause.</span></span> 

<span data-ttu-id="f6dd2-304">Hello hlavní rozdíl mezi použitím BETWEEN v DocumentDB rozhraní API a ANSI SQL je, že můžete express rozsah dotazy na vlastnosti smíšený typů – například můžete mít "základní" být číslo (5) v některých dokumentů a řetězce v jiné ("grade4").</span><span class="sxs-lookup"><span data-stu-id="f6dd2-304">hello main difference between using BETWEEN in DocumentDB API and ANSI SQL is that you can express range queries against properties of mixed types – for example, you might have "grade" be a number (5) in some documents and strings in others ("grade4").</span></span> <span data-ttu-id="f6dd2-305">V těchto případech jako je v jazyce JavaScript, porovnání mezi dva různé typy výsledků v "undefined" a hello dokumentu bude přeskočen.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-305">In these cases, like in JavaScript, a comparison between two different types results in "undefined", and hello document will be skipped.</span></span>

### <a name="logical-and-or-and-not-operators"></a><span data-ttu-id="f6dd2-306">Logický (AND, OR a NOT) operátory</span><span class="sxs-lookup"><span data-stu-id="f6dd2-306">Logical (AND, OR and NOT) operators</span></span>
<span data-ttu-id="f6dd2-307">Logické operátory pracovat logické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-307">Logical operators operate on Boolean values.</span></span> <span data-ttu-id="f6dd2-308">Hello logické pravdivosti tabulky pro tyto operátory jsou uvedené v následující tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-308">hello logical truth tables for these operators are shown in hello following tables.</span></span>

| <span data-ttu-id="f6dd2-309">NEBO</span><span class="sxs-lookup"><span data-stu-id="f6dd2-309">OR</span></span> | <span data-ttu-id="f6dd2-310">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-310">True</span></span> | <span data-ttu-id="f6dd2-311">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-311">False</span></span> | <span data-ttu-id="f6dd2-312">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-312">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f6dd2-313">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-313">True</span></span> |<span data-ttu-id="f6dd2-314">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-314">True</span></span> |<span data-ttu-id="f6dd2-315">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-315">True</span></span> |<span data-ttu-id="f6dd2-316">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-316">True</span></span> |
| <span data-ttu-id="f6dd2-317">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-317">False</span></span> |<span data-ttu-id="f6dd2-318">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-318">True</span></span> |<span data-ttu-id="f6dd2-319">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-319">False</span></span> |<span data-ttu-id="f6dd2-320">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-320">Undefined</span></span> |
| <span data-ttu-id="f6dd2-321">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-321">Undefined</span></span> |<span data-ttu-id="f6dd2-322">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-322">True</span></span> |<span data-ttu-id="f6dd2-323">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-323">Undefined</span></span> |<span data-ttu-id="f6dd2-324">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-324">Undefined</span></span> |

| <span data-ttu-id="f6dd2-325">A</span><span class="sxs-lookup"><span data-stu-id="f6dd2-325">AND</span></span> | <span data-ttu-id="f6dd2-326">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-326">True</span></span> | <span data-ttu-id="f6dd2-327">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-327">False</span></span> | <span data-ttu-id="f6dd2-328">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-328">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f6dd2-329">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-329">True</span></span> |<span data-ttu-id="f6dd2-330">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-330">True</span></span> |<span data-ttu-id="f6dd2-331">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-331">False</span></span> |<span data-ttu-id="f6dd2-332">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-332">Undefined</span></span> |
| <span data-ttu-id="f6dd2-333">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-333">False</span></span> |<span data-ttu-id="f6dd2-334">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-334">False</span></span> |<span data-ttu-id="f6dd2-335">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-335">False</span></span> |<span data-ttu-id="f6dd2-336">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-336">False</span></span> |
| <span data-ttu-id="f6dd2-337">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-337">Undefined</span></span> |<span data-ttu-id="f6dd2-338">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-338">Undefined</span></span> |<span data-ttu-id="f6dd2-339">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-339">False</span></span> |<span data-ttu-id="f6dd2-340">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-340">Undefined</span></span> |

| <span data-ttu-id="f6dd2-341">NENÍ</span><span class="sxs-lookup"><span data-stu-id="f6dd2-341">NOT</span></span> |  |
| --- | --- |
| <span data-ttu-id="f6dd2-342">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-342">True</span></span> |<span data-ttu-id="f6dd2-343">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-343">False</span></span> |
| <span data-ttu-id="f6dd2-344">False</span><span class="sxs-lookup"><span data-stu-id="f6dd2-344">False</span></span> |<span data-ttu-id="f6dd2-345">True</span><span class="sxs-lookup"><span data-stu-id="f6dd2-345">True</span></span> |
| <span data-ttu-id="f6dd2-346">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-346">Undefined</span></span> |<span data-ttu-id="f6dd2-347">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="f6dd2-347">Undefined</span></span> |

### <a name="in-keyword"></a><span data-ttu-id="f6dd2-348">IN – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="f6dd2-348">IN keyword</span></span>
<span data-ttu-id="f6dd2-349">Hello IN – klíčové slovo lze použít toocheck, zda zadaná hodnota odpovídá žádnou hodnotu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-349">hello IN keyword can be used toocheck whether a specified value matches any value in a list.</span></span> <span data-ttu-id="f6dd2-350">Například tento dotaz vrací všechny rodiny dokumenty, kde je hello id "WakefieldFamily" nebo "AndersenFamily".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-350">For example, this query returns all family documents where hello id is one of "WakefieldFamily" or "AndersenFamily".</span></span> 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

<span data-ttu-id="f6dd2-351">Tento příklad vrátí všechny dokumenty, je-li hello stát žádné hello zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-351">This example returns all documents where hello state is any of hello specified values.</span></span>

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a><span data-ttu-id="f6dd2-352">Unární (?) a operátory Coalesce (?)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-352">Ternary (?) and Coalesce (??) operators</span></span>
<span data-ttu-id="f6dd2-353">operátory Unární a Coalesce Hello se dá použít toobuild podmíněné výrazy, podobně jako toopopular programovacích jazyků, jako je C# a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-353">hello Ternary and Coalesce operators can be used toobuild conditional expressions, similar toopopular programming languages like C# and JavaScript.</span></span> 

<span data-ttu-id="f6dd2-354">operátor unární (?) Hello může být velmi užitečný, pokud fyzicky dostavili vytváření nových vlastností JSON na hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-354">hello Ternary (?) operator can be very handy when constructing new JSON properties on hello fly.</span></span> <span data-ttu-id="f6dd2-355">Například teď můžete napsat dotazy tooclassify hello třída úrovně do lidského čitelné podoby jako Začátečník nebo zprostředkující nebo Upřesnit, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-355">For example, now you can write queries tooclassify hello class levels into a human readable form like Beginner/Intermediate/Advanced as shown below.</span></span>

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

<span data-ttu-id="f6dd2-356">Můžete také vnořovat hello operátor volání toohello jako v dotazu hello níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-356">You can also nest hello calls toohello operator like in hello query below.</span></span>

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

<span data-ttu-id="f6dd2-357">Jako s dalšími operátory dotazu, pokud hello odkazované vlastnosti podmíněného výrazu hello chybí v dokumentu, nebo pokud hello typy porovnávané se liší, pak tyto dokumenty jsou vyloučeny ve výsledcích dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-357">As with other query operators, if hello referenced properties in hello conditional expression are missing in any document, or if hello types being compared are different, then those documents are excluded in hello query results.</span></span>

<span data-ttu-id="f6dd2-358">Hello Coalesce (?) operátor může být použit tooefficiently zkontrolujte přítomnost hello vlastnost (také známa jako</span><span class="sxs-lookup"><span data-stu-id="f6dd2-358">hello Coalesce (??) operator can be used tooefficiently check for hello presence of a property (a.k.a.</span></span> <span data-ttu-id="f6dd2-359">je definován) v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-359">is defined) in a document.</span></span> <span data-ttu-id="f6dd2-360">To je užitečné při dotazování na částečně strukturovaných nebo data smíšený typů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-360">This is useful when querying against semi-structured or data of mixed types.</span></span> <span data-ttu-id="f6dd2-361">Tento dotaz vrací například hello "lastName" Pokud existuje, nebo "Přezdívka" hello, pokud není přítomen.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-361">For example, this query returns hello "lastName" if present, or hello "surname" if it isn't present.</span></span>

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <span data-ttu-id="f6dd2-362"><a id="EscapingReservedKeywords"></a>Vlastnost uvozovkách přístupového objektu</span><span class="sxs-lookup"><span data-stu-id="f6dd2-362"><a id="EscapingReservedKeywords"></a>Quoted property accessor</span></span>
<span data-ttu-id="f6dd2-363">Můžete také přístup k vlastnostem pomocí operátoru vlastnost uvozovkách hello `[]`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-363">You can also access properties using hello quoted property operator `[]`.</span></span> <span data-ttu-id="f6dd2-364">Například `SELECT c.grade` a `SELECT c["grade"]` odpovídají.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-364">For example, `SELECT c.grade` and `SELECT c["grade"]` are equivalent.</span></span> <span data-ttu-id="f6dd2-365">Tato syntaxe je užitečné, když potřebujete tooescape vlastnost, která obsahuje mezery, speciální znaky, nebo se stane tooshare hello stejný název jako SQL – klíčové slovo nebo vyhrazené slovo.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-365">This syntax is useful when you need tooescape a property that contains spaces, special characters, or happens tooshare hello same name as a SQL keyword or reserved word.</span></span>

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <span data-ttu-id="f6dd2-366"><a id="SelectClause"></a>Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="f6dd2-366"><a id="SelectClause"></a>SELECT clause</span></span>
<span data-ttu-id="f6dd2-367">Klauzule SELECT Hello (**`SELECT <select_list>`**) je povinná a určuje, jaké hodnoty jsou načteny z hello dotaz, podobně jako v ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-367">hello SELECT clause (**`SELECT <select_list>`**) is mandatory and specifies what values are retrieved from hello query, just like in ANSI-SQL.</span></span> <span data-ttu-id="f6dd2-368">podmnožina Hello je filtrované nad hello zdroj dokumenty jsou předávány do fáze projekce hello, kde hello zadané hodnoty JSON se načítají a je vytvořený nový objekt JSON, pro každý vstupní předán na něj.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-368">hello subset that's been filtered on top of hello source documents are passed onto hello projection phase, where hello specified JSON values are retrieved and a new JSON object is constructed, for each input passed onto it.</span></span> 

<span data-ttu-id="f6dd2-369">Hello následující příklad ukazuje typické dotaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-369">hello following example shows a typical SELECT query.</span></span> 

<span data-ttu-id="f6dd2-370">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-370">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-371">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-371">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a><span data-ttu-id="f6dd2-372">Vnořené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f6dd2-372">Nested properties</span></span>
<span data-ttu-id="f6dd2-373">V následujícím příkladu hello, jsme jsou projekce dvě vnořené vlastnosti `f.address.state` a `f.address.city`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-373">In hello following example, we are projecting two nested properties `f.address.state` and `f.address.city`.</span></span>

<span data-ttu-id="f6dd2-374">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-374">**Query**</span></span>

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-375">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-375">**Results**</span></span>

    [{
      "state": "WA", 
      "city": "seattle"
    }]


<span data-ttu-id="f6dd2-376">Projekce také podporuje JSON výrazy, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-376">Projection also supports JSON expressions as shown in hello following example:</span></span>

<span data-ttu-id="f6dd2-377">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-377">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-378">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-378">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


<span data-ttu-id="f6dd2-379">Podívejme se na roli hello `$1` sem.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-379">Let's look at hello role of `$1` here.</span></span> <span data-ttu-id="f6dd2-380">Hello `SELECT` klauzule potřebuje toocreate objekt JSON a vzhledem k tomu, že žádný klíč je k dispozici, používáme názvy proměnných implicitní argument počínaje `$1`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-380">hello `SELECT` clause needs toocreate a JSON object and since no key is provided, we use implicit argument variable names starting with `$1`.</span></span> <span data-ttu-id="f6dd2-381">Například tento dotaz vrací dvě implicitní argument proměnné s názvem bez přípony `$1` a `$2`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-381">For example, this query returns two implicit argument variables, labeled `$1` and `$2`.</span></span>

<span data-ttu-id="f6dd2-382">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-382">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-383">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-383">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a><span data-ttu-id="f6dd2-384">Aliasy</span><span class="sxs-lookup"><span data-stu-id="f6dd2-384">Aliasing</span></span>
<span data-ttu-id="f6dd2-385">Nyní Pojďme hello příklad rozšířit nad s explicitní aliasy hodnot.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-385">Now let's extend hello example above with explicit aliasing of values.</span></span> <span data-ttu-id="f6dd2-386">Stejně jako se používá pro aliasy – klíčové slovo hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-386">AS is hello keyword used for aliasing.</span></span> <span data-ttu-id="f6dd2-387">Zadání je volitelné, jak je znázorněno při plánování hello druhá hodnota, která jako `NameInfo`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-387">It's optional as shown while projecting hello second value as `NameInfo`.</span></span> 

<span data-ttu-id="f6dd2-388">V případě, že dotaz má dvě vlastnosti se hello stejný název, aliasy musí být použité toorename jedno nebo obě hello vlastnosti tak, aby se jsou od sebe jednoznačně rozlišeny v projekci hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-388">In case a query has two properties with hello same name, aliasing must be used toorename one or both of hello properties so that they are disambiguated in hello projected result.</span></span>

<span data-ttu-id="f6dd2-389">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-389">**Query**</span></span>

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-390">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-390">**Results**</span></span>

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a><span data-ttu-id="f6dd2-391">Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="f6dd2-391">Scalar expressions</span></span>
<span data-ttu-id="f6dd2-392">Kromě toho tooproperty odkazuje, klauzule SELECT hello také podporuje skalární výrazy konstanty, aritmetických výrazech, logických výrazů, atd. Tady je příklad jednoduchého dotazu "Hello World".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-392">In addition tooproperty references, hello SELECT clause also supports scalar expressions like constants, arithmetic expressions, logical expressions, etc. For example, here's a simple "Hello World" query.</span></span>

<span data-ttu-id="f6dd2-393">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-393">**Query**</span></span>

    SELECT "Hello World"

<span data-ttu-id="f6dd2-394">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-394">**Results**</span></span>

    [{
      "$1": "Hello World"
    }]


<span data-ttu-id="f6dd2-395">Zde je ukázka používající skalární výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-395">Here's a more complex example that uses a scalar expression.</span></span>

<span data-ttu-id="f6dd2-396">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-396">**Query**</span></span>

    SELECT ((2 + 11 % 7)-2)/3    

<span data-ttu-id="f6dd2-397">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-397">**Results**</span></span>

    [{
      "$1": 1.33333
    }]


<span data-ttu-id="f6dd2-398">V následujícím příkladu hello hello výsledek hello skalární výraz, který je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-398">In hello following example, hello result of hello scalar expression is a Boolean.</span></span>

<span data-ttu-id="f6dd2-399">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-399">**Query**</span></span>

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

<span data-ttu-id="f6dd2-400">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-400">**Results**</span></span>

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a><span data-ttu-id="f6dd2-401">Vytvoření objektu a pole</span><span class="sxs-lookup"><span data-stu-id="f6dd2-401">Object and array creation</span></span>
<span data-ttu-id="f6dd2-402">Další klíčovou funkcí DocumentDB SQL rozhraní API je vytvoření pole nebo objektu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-402">Another key feature of DocumentDB API SQL is array/object creation.</span></span> <span data-ttu-id="f6dd2-403">V předchozím příkladu hello Všimněte si, že jsme vytvořili nový objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-403">In hello previous example, note that we created a new JSON object.</span></span> <span data-ttu-id="f6dd2-404">Podobně jeden můžete také vytvořit pole jak je znázorněno v hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-404">Similarly, one can also construct arrays as shown in hello following examples:</span></span>

<span data-ttu-id="f6dd2-405">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-405">**Query**</span></span>

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

<span data-ttu-id="f6dd2-406">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-406">**Results**</span></span>  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <span data-ttu-id="f6dd2-407"><a id="ValueKeyword"></a>VALUE – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="f6dd2-407"><a id="ValueKeyword"></a>VALUE keyword</span></span>
<span data-ttu-id="f6dd2-408">Hello **hodnotu** – klíčové slovo poskytuje hodnotu způsob tooreturn formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-408">hello **VALUE** keyword provides a way tooreturn JSON value.</span></span> <span data-ttu-id="f6dd2-409">Například hello dotazu vidíte níže vrátí hello skalární `"Hello World"` místo `{$1: "Hello World"}`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-409">For example, hello query shown below returns hello scalar `"Hello World"` instead of `{$1: "Hello World"}`.</span></span>

<span data-ttu-id="f6dd2-410">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-410">**Query**</span></span>

    SELECT VALUE "Hello World"

<span data-ttu-id="f6dd2-411">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-411">**Results**</span></span>

    [
      "Hello World"
    ]


<span data-ttu-id="f6dd2-412">Hello následující dotaz vrátí hodnotu JSON hello bez hello `"address"` popisek ve výsledcích hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-412">hello following query returns hello JSON value without hello `"address"` label in hello results.</span></span>

<span data-ttu-id="f6dd2-413">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-413">**Query**</span></span>

    SELECT VALUE f.address
    FROM Families f    

<span data-ttu-id="f6dd2-414">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-414">**Results**</span></span>  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

<span data-ttu-id="f6dd2-415">Hello následující příklad rozšiřuje tento tooshow jak tooreturn JSON primitivní hodnoty (hello listové úrovni stromu hello JSON).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-415">hello following example extends this tooshow how tooreturn JSON primitive values (hello leaf level of hello JSON tree).</span></span> 

<span data-ttu-id="f6dd2-416">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-416">**Query**</span></span>

    SELECT VALUE f.address.state
    FROM Families f    

<span data-ttu-id="f6dd2-417">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-417">**Results**</span></span>

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a><span data-ttu-id="f6dd2-418">* – Operátor</span><span class="sxs-lookup"><span data-stu-id="f6dd2-418">* Operator</span></span>
<span data-ttu-id="f6dd2-419">Hello speciální – operátor (*) je podporované tooproject hello dokumentu jako-je.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-419">hello special operator (*) is supported tooproject hello document as-is.</span></span> <span data-ttu-id="f6dd2-420">Pokud se používá, musí být, že hello k projekci pouze pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-420">When used, it must be hello only projected field.</span></span> <span data-ttu-id="f6dd2-421">Při dotazu jako `SELECT * FROM Families f` je platný, `SELECT VALUE * FROM Families f ` a `SELECT *, f.id FROM Families f ` nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-421">While a query like `SELECT * FROM Families f` is valid, `SELECT VALUE * FROM Families f ` and  `SELECT *, f.id FROM Families f ` are not valid.</span></span>

<span data-ttu-id="f6dd2-422">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-422">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="f6dd2-423">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-423">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <span data-ttu-id="f6dd2-424"><a id="TopKeyword"></a>Operátor TOP</span><span class="sxs-lookup"><span data-stu-id="f6dd2-424"><a id="TopKeyword"></a>TOP Operator</span></span>
<span data-ttu-id="f6dd2-425">TOP – klíčové slovo Hello lze použít toolimit hello počet hodnot z dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-425">hello TOP keyword can be used toolimit hello number of values from a query.</span></span> <span data-ttu-id="f6dd2-426">Při horní se používá ve spojení s hello klauzule ORDER by, sadu výsledků hello je omezená toohello první N počet seřazené hodnoty; Funkce hello první N počet výsledků v nedefinované pořadí.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-426">When TOP is used in conjunction with hello ORDER BY clause, hello result set is limited toohello first N number of ordered values; otherwise, it returns hello first N number of results in an undefined order.</span></span> <span data-ttu-id="f6dd2-427">Jako osvědčený postup v příkazu SELECT, vždy používejte klauzuli ORDER BY pomocí klauzule TOP hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-427">As a best practice, in a SELECT statement, always use an ORDER BY clause with hello TOP clause.</span></span> <span data-ttu-id="f6dd2-428">To je jedinou možností hello toopredictably označují řádky, které jsou ovlivněné TOP.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-428">This is hello only way toopredictably indicate which rows are affected by TOP.</span></span> 

<span data-ttu-id="f6dd2-429">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-429">**Query**</span></span>

    SELECT TOP 1 * 
    FROM Families f 

<span data-ttu-id="f6dd2-430">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-430">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

<span data-ttu-id="f6dd2-431">HORNÍ lze použít s konstantní hodnotou (jak jsme ukázali výše) nebo s hodnotou proměnné použití parametrických dotazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-431">TOP can be used with a constant value (as shown above) or with a variable value using parameterized queries.</span></span> <span data-ttu-id="f6dd2-432">Další podrobnosti najdete v tématu parametrizované dotazy níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-432">For more details, please see parameterized queries below.</span></span>

### <span data-ttu-id="f6dd2-433"><a id="Aggregates"></a>Agregační funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-433"><a id="Aggregates"></a>Aggregate Functions</span></span>
<span data-ttu-id="f6dd2-434">Můžete také provádět agregace v hello `SELECT` klauzule.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-434">You can also perform aggregations in hello `SELECT` clause.</span></span> <span data-ttu-id="f6dd2-435">Agregační funkce provádět výpočet sadu hodnot a vrátí jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-435">Aggregate functions perform a calculation on a set of values and return a single value.</span></span> <span data-ttu-id="f6dd2-436">Například hello následující dotaz vrátí počet hello rodiny dokumentů v rámci kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-436">For example, hello following query returns hello count of family documents within hello collection.</span></span>

<span data-ttu-id="f6dd2-437">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-437">**Query**</span></span>

    SELECT COUNT(1) 
    FROM Families f 

<span data-ttu-id="f6dd2-438">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-438">**Results**</span></span>

    [{
        "$1": 2
    }]

<span data-ttu-id="f6dd2-439">Můžete se taky vrátit hello skalární hodnota hello agregační pomocí hello `VALUE` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-439">You can also return hello scalar value of hello aggregate by using hello `VALUE` keyword.</span></span> <span data-ttu-id="f6dd2-440">Například hello následující dotaz vrátí hello počet hodnot jako jediné číslo:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-440">For example, hello following query returns hello count of values as a single number:</span></span>

<span data-ttu-id="f6dd2-441">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-441">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f 

<span data-ttu-id="f6dd2-442">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-442">**Results**</span></span>

    [ 2 ]

<span data-ttu-id="f6dd2-443">Můžete také provést agregace v kombinaci s filtry.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-443">You can also perform aggregates in combination with filters.</span></span> <span data-ttu-id="f6dd2-444">Například hello následující dotaz vrátí hello počet dokumentů s adresou hello v hello státu Washington.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-444">For example, hello following query returns hello count of documents with hello address in hello state of Washington.</span></span>

<span data-ttu-id="f6dd2-445">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-445">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

<span data-ttu-id="f6dd2-446">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-446">**Results**</span></span>

    [ 1 ]

<span data-ttu-id="f6dd2-447">Hello následující tabulka uvádí hello seznam podporovaných agregační funkce v DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-447">hello following table shows hello list of supported aggregate functions in DocumentDB API.</span></span> <span data-ttu-id="f6dd2-448">`SUM`a `AVG` se provádí přes číselných hodnot, zatímco `COUNT`, `MIN`, a `MAX` lze provést přes čísla, řetězce, logické hodnoty a hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-448">`SUM` and `AVG` are performed over numeric values, whereas `COUNT`, `MIN`, and `MAX` can be performed over numbers, strings, Booleans, and nulls.</span></span> 

| <span data-ttu-id="f6dd2-449">Využití</span><span class="sxs-lookup"><span data-stu-id="f6dd2-449">Usage</span></span> | <span data-ttu-id="f6dd2-450">Popis</span><span class="sxs-lookup"><span data-stu-id="f6dd2-450">Description</span></span> |
|-------|-------------|
| <span data-ttu-id="f6dd2-451">POČET</span><span class="sxs-lookup"><span data-stu-id="f6dd2-451">COUNT</span></span> | <span data-ttu-id="f6dd2-452">Vrátí hello počet položek ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-452">Returns hello number of items in hello expression.</span></span> |
| <span data-ttu-id="f6dd2-453">SOUČET</span><span class="sxs-lookup"><span data-stu-id="f6dd2-453">SUM</span></span>   | <span data-ttu-id="f6dd2-454">Vrátí hello součet všech hodnot hello ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-454">Returns hello sum of all hello values in hello expression.</span></span> |
| <span data-ttu-id="f6dd2-455">MIN.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-455">MIN</span></span>   | <span data-ttu-id="f6dd2-456">Vrátí hello minimální hodnotu ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-456">Returns hello minimum value in hello expression.</span></span> |
| <span data-ttu-id="f6dd2-457">MAXIMÁLNÍ POČET</span><span class="sxs-lookup"><span data-stu-id="f6dd2-457">MAX</span></span>   | <span data-ttu-id="f6dd2-458">Vrátí hello maximální hodnotu ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-458">Returns hello maximum value in hello expression.</span></span> |
| <span data-ttu-id="f6dd2-459">PRŮMĚR</span><span class="sxs-lookup"><span data-stu-id="f6dd2-459">AVG</span></span>   | <span data-ttu-id="f6dd2-460">Vrátí hello průměr hodnot hello ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-460">Returns hello average of hello values in hello expression.</span></span> |

<span data-ttu-id="f6dd2-461">Agreguje lze také provést přes hello výsledky iterace pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-461">Aggregates can also be performed over hello results of an array iteration.</span></span> <span data-ttu-id="f6dd2-462">Další informace najdete v tématu [pole iterace v dotazech](#Iteration).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-462">For more information, see [Array Iteration in Queries](#Iteration).</span></span>

> [!NOTE]
> <span data-ttu-id="f6dd2-463">Při použití hello Průzkumníka dotazů portálu Azure, Všimněte si, že agregace dotazy může vracet hello částečně agregované výsledky dotazu stránky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-463">When using hello Azure portal's Query Explorer, note that aggregation queries may return hello partially aggregated results over a query page.</span></span> <span data-ttu-id="f6dd2-464">Hello sady SDK vytvoří jednu kumulativní hodnotu na všech stránkách.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-464">hello SDKs produces a single cumulative value across all pages.</span></span> 
> 
> <span data-ttu-id="f6dd2-465">V pořadí tooperform agregace dotazy pomocí kódu, potřebujete .NET SDK 1.12.0, .NET Core SDK 1.1.0 nebo Java SDK 1.9.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-465">In order tooperform aggregation queries using code, you need .NET SDK 1.12.0, .NET Core SDK 1.1.0, or Java SDK 1.9.5 or above.</span></span>    
>

## <span data-ttu-id="f6dd2-466"><a id="OrderByClause"></a>Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="f6dd2-466"><a id="OrderByClause"></a>ORDER BY clause</span></span>
<span data-ttu-id="f6dd2-467">Podobně jako v ANSI SQL, můžete zahrnout volitelné klauzule Order By při dotazování.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-467">Like in ANSI-SQL, you can include an optional Order By clause while querying.</span></span> <span data-ttu-id="f6dd2-468">klauzule Hello může obsahovat volitelné ASC nebo DESC argument toospecify hello pořadí ve kterém musí načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-468">hello clause can include an optional ASC/DESC argument toospecify hello order in which results must be retrieved.</span></span>

<span data-ttu-id="f6dd2-469">Zde je například dotaz, který načte rodiny v pořadí podle název hello města trvalé.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-469">For example, here's a query that retrieves families in order of hello resident city's name.</span></span>

<span data-ttu-id="f6dd2-470">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-470">**Query**</span></span>

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

<span data-ttu-id="f6dd2-471">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-471">**Results**</span></span>

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

<span data-ttu-id="f6dd2-472">A tady je dotaz, který načte rodiny v pořadí podle data vytvoření, které je uloženo jako číslo představující hello epoch čas, tj, uplynulý čas od 1. ledna 1970 v sekundách.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-472">And here's a query that retrieves families in order of creation date, which is stored as a number representing hello epoch time, i.e, elapsed time since Jan 1, 1970 in seconds.</span></span>

<span data-ttu-id="f6dd2-473">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-473">**Query**</span></span>

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

<span data-ttu-id="f6dd2-474">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-474">**Results**</span></span>

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <span data-ttu-id="f6dd2-475"><a id="Advanced"></a>Pokročilé databázových koncepcí a dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-475"><a id="Advanced"></a>Advanced database concepts and SQL queries</span></span>

### <span data-ttu-id="f6dd2-476"><a id="Iteration"></a>Iterace</span><span class="sxs-lookup"><span data-stu-id="f6dd2-476"><a id="Iteration"></a>Iteration</span></span>
<span data-ttu-id="f6dd2-477">Byl přidán nový konstrukce prostřednictvím hello **IN** – klíčové slovo v DocumentDB API SQL tooprovide podpora iterování přes pole JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-477">A new construct was added via hello **IN** keyword in DocumentDB API SQL tooprovide support for iterating over JSON arrays.</span></span> <span data-ttu-id="f6dd2-478">Zdroj FROM Hello poskytuje podporu pro iterací.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-478">hello FROM source provides support for iteration.</span></span> <span data-ttu-id="f6dd2-479">Začneme hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-479">Let's start with hello following example:</span></span>

<span data-ttu-id="f6dd2-480">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-480">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="f6dd2-481">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-481">**Results**</span></span>  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

<span data-ttu-id="f6dd2-482">Nyní Podíváme se na další dotaz, který provádí iteraci přes podřízené položky v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-482">Now let's look at another query that performs iteration over children in hello collection.</span></span> <span data-ttu-id="f6dd2-483">Všimněte si hello rozdíl v poli výstup hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-483">Note hello difference in hello output array.</span></span> <span data-ttu-id="f6dd2-484">Tento příklad rozdělí `children` a vyrovná hello výsledky do jednoho pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-484">This example splits `children` and flattens hello results into a single array.</span></span>  

<span data-ttu-id="f6dd2-485">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-485">**Query**</span></span>

    SELECT * 
    FROM c IN Families.children

<span data-ttu-id="f6dd2-486">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-486">**Results**</span></span>  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

<span data-ttu-id="f6dd2-487">To může být další používané toofilter na každou položku hello pole, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-487">This can be further used toofilter on each individual entry of hello array as shown in hello following example:</span></span>

<span data-ttu-id="f6dd2-488">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-488">**Query**</span></span>

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

<span data-ttu-id="f6dd2-489">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-489">**Results**</span></span>  

    [{
      "givenName": "Lisa"
    }]

<span data-ttu-id="f6dd2-490">Můžete také provést agregace přes hello výsledek iterace pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-490">You can also perform aggregation over hello result of array iteration.</span></span> <span data-ttu-id="f6dd2-491">Například hello následující dotaz vrátí hello počet podřízených prvků mezi všechny řady.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-491">For example, hello following query counts hello number of children among all families.</span></span>

<span data-ttu-id="f6dd2-492">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-492">**Query**</span></span>

    SELECT COUNT(child) 
    FROM child IN Families.children

<span data-ttu-id="f6dd2-493">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-493">**Results**</span></span>  

    [
      { 
        "$1": 3
      }
    ]

### <span data-ttu-id="f6dd2-494"><a id="Joins"></a>Spojení</span><span class="sxs-lookup"><span data-stu-id="f6dd2-494"><a id="Joins"></a>Joins</span></span>
<span data-ttu-id="f6dd2-495">V relační databázi je důležité hello nutné toojoin mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-495">In a relational database, hello need toojoin across tables is important.</span></span> <span data-ttu-id="f6dd2-496">Jeho hello logické corollary toodesigning normalized schémat.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-496">It's hello logical corollary toodesigning normalized schemas.</span></span> <span data-ttu-id="f6dd2-497">Jinak zvláštní toothis, DocumentDB rozhraní API se zabývá hello nenormalizované datový model bez schémat dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-497">Contrary toothis, DocumentDB API deals with hello denormalized data model of schema-free documents.</span></span> <span data-ttu-id="f6dd2-498">Toto je logický ekvivalent hello a "spojení sama na sebe".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-498">This is hello logical equivalent of a "self-join".</span></span>

<span data-ttu-id="f6dd2-499">Hello syntaxe, který podporuje jazyk hello je < from_source1 > připojit < from_source2 > připojit... Připojte < from_sourceN >.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-499">hello syntax that hello language supports is <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>.</span></span> <span data-ttu-id="f6dd2-500">Celkově platí, tento příkaz vrátí sadu **N**- n-tice (řazené kolekce členů s **N** hodnoty).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-500">Overall, this returns a set of **N**-tuples (tuple with **N** values).</span></span> <span data-ttu-id="f6dd2-501">Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-501">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span> <span data-ttu-id="f6dd2-502">Jinými slovy Toto je úplná smíšený produkt sad hello účastní spojení hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-502">In other words, this is a full cross product of hello sets participating in hello join.</span></span>

<span data-ttu-id="f6dd2-503">Hello následující příklady ukazují, jak funguje klauzuli JOIN hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-503">hello following examples show how hello JOIN clause works.</span></span> <span data-ttu-id="f6dd2-504">V následujícím příkladu hello hello výsledek je prázdná, protože hello smíšený produkt každého dokumentu ze zdroje a prázdnou sadou je prázdný.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-504">In hello following example, hello result is empty since hello cross product of each document from source and an empty set is empty.</span></span>

<span data-ttu-id="f6dd2-505">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-505">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

<span data-ttu-id="f6dd2-506">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-506">**Results**</span></span>  

    [{
    }]


<span data-ttu-id="f6dd2-507">V následujícím příkladu hello, je hello spojení mezi kořen dokumentu hello a hello `children` subroot.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-507">In hello following example, hello join is between hello document root and hello `children` subroot.</span></span> <span data-ttu-id="f6dd2-508">Je smíšený produkt mezi dvěma objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-508">It's a cross product between two JSON objects.</span></span> <span data-ttu-id="f6dd2-509">Hello skutečnost, že podřízené objekty je pole není platná v hello spojení, protože jsme se zabývají na jednom kořenovou, která je hello podřízených prvků pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-509">hello fact that children is an array is not effective in hello JOIN since we are dealing with a single root that is hello children array.</span></span> <span data-ttu-id="f6dd2-510">Proto hello výsledek obsahuje pouze dva výsledky, protože hello smíšený produkt každý dokument s polem hello vypočítá přesně pouze jeden dokument.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-510">Hence hello result contains only two results, since hello cross product of each document with hello array yields exactly only one document.</span></span>

<span data-ttu-id="f6dd2-511">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-511">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.children

<span data-ttu-id="f6dd2-512">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-512">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


<span data-ttu-id="f6dd2-513">Hello následující příklad ukazuje konvenční připojení:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-513">hello following example shows a more conventional join:</span></span>

<span data-ttu-id="f6dd2-514">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-514">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

<span data-ttu-id="f6dd2-515">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-515">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



<span data-ttu-id="f6dd2-516">Hello nejprve thing toonote je tento hello `from_source` z hello **připojení** klauzule je iterátor.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-516">hello first thing toonote is that hello `from_source` of hello **JOIN** clause is an iterator.</span></span> <span data-ttu-id="f6dd2-517">Ano hello tok v takovém případě je následující:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-517">So, hello flow in this case is as follows:</span></span>  

* <span data-ttu-id="f6dd2-518">Rozbalte každý podřízený element **c** v poli hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-518">Expand each child element **c** in hello array.</span></span>
* <span data-ttu-id="f6dd2-519">Použít smíšený produkt s hello kořen dokumentu hello **f** s každou podřízený element **c** , byl průmětu v prvním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-519">Apply a cross product with hello root of hello document **f** with each child element **c** that was flattened in hello first step.</span></span>
* <span data-ttu-id="f6dd2-520">Nakonec projektu hello kořenový objekt **f** name – vlastnost samostatně.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-520">Finally, project hello root object **f** name property alone.</span></span> 

<span data-ttu-id="f6dd2-521">první dokument Hello (`AndersenFamily`) obsahuje pouze jeden podřízený element, takže hello sadu výsledků dotazu obsahuje pouze jeden objekt odpovídající toothis dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-521">hello first document (`AndersenFamily`) contains only one child element, so hello result set contains only a single object corresponding toothis document.</span></span> <span data-ttu-id="f6dd2-522">druhý dokumentu Hello (`WakefieldFamily`) obsahuje dva podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-522">hello second document (`WakefieldFamily`) contains two children.</span></span> <span data-ttu-id="f6dd2-523">Ano hello smíšený produkt vytváří samostatný objekt pro všechny podřízené, což by vedlo k dva objekty, jednu pro každý dokument podřízené odpovídající toothis.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-523">So, hello cross product produces a separate object for each child, thereby resulting in two objects, one for each child corresponding toothis document.</span></span> <span data-ttu-id="f6dd2-524">Kořenová Hello pole v obou tyto dokumenty jsou stejné, hello stejně, jako byste očekávali v smíšený produkt.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-524">hello root fields in both these documents are hello same, just as you would expect in a cross product.</span></span>

<span data-ttu-id="f6dd2-525">Hello skutečných nástroj z hello spojení je tooform řazenými kolekcemi členů z hello smíšený produkt ve tvaru, který je jinak tooproject obtížná.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-525">hello real utility of hello JOIN is tooform tuples from hello cross-product in a shape that's otherwise difficult tooproject.</span></span> <span data-ttu-id="f6dd2-526">Kromě toho, jak vidíte v následujícím příkladu hello, byste mohli vyfiltrovat na kombinaci hello řazené kolekce členů umožňuje hello Uživatel reagoval podmínku celkové uspokojit hello řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-526">Furthermore, as we see in hello example below, you could filter on hello combination of a tuple that lets' hello user chose a condition satisfied by hello tuples overall.</span></span>

<span data-ttu-id="f6dd2-527">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-527">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

<span data-ttu-id="f6dd2-528">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-528">**Results**</span></span>

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



<span data-ttu-id="f6dd2-529">Tento příklad představuje přirozené rozšíření Dobrý den předcházející příklad a spojí double.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-529">This example is a natural extension of hello preceding example, and performs a double join.</span></span> <span data-ttu-id="f6dd2-530">Ano hello smíšený produkt je možné zobrazit jako hello následující pseudo kódu:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-530">So, hello cross product can be viewed as hello following pseudo-code:</span></span>

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

<span data-ttu-id="f6dd2-531">`AndersenFamily`má jednu podřízenou, který má jednoho nebo více mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-531">`AndersenFamily` has one child who has one pet.</span></span> <span data-ttu-id="f6dd2-532">Ano, hello smíšený produkt vypočítá jeden řádek (1\*1\*1) z této rodiny.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-532">So, hello cross product yields one row (1\*1\*1) from this family.</span></span> <span data-ttu-id="f6dd2-533">WakefieldFamily ale má dva podřízené, ale pouze jednu podřízenou "Jesse" má mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-533">WakefieldFamily however has two children, but only one child "Jesse" has pets.</span></span> <span data-ttu-id="f6dd2-534">Jesse, když má dva mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-534">Jesse has two pets though.</span></span> <span data-ttu-id="f6dd2-535">Proto hello smíšený produkt vypočítá 1\*1\*řádků z této rodině, 2 = 2.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-535">Hence hello cross product yields 1\*1\*2 = 2 rows from this family.</span></span>

<span data-ttu-id="f6dd2-536">V dalším příkladu hello, je další filtr na `pet`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-536">In hello next example, there is an additional filter on `pet`.</span></span> <span data-ttu-id="f6dd2-537">Nevztahuje se na všechny řazených kolekcí členů hello kde hello pet název není "Stínové".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-537">This excludes all hello tuples where hello pet name is not "Shadow".</span></span> <span data-ttu-id="f6dd2-538">Všimněte si, že jsme jsou možné toobuild řazenými kolekcemi členů z pole filtru na všech elementů hello hello řazené kolekce členů a projektu libovolnou kombinaci elementů hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-538">Notice that we are able toobuild tuples from arrays, filter on any of hello elements of hello tuple, and project any combination of hello elements.</span></span> 

<span data-ttu-id="f6dd2-539">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-539">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

<span data-ttu-id="f6dd2-540">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-540">**Results**</span></span>

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <span data-ttu-id="f6dd2-541"><a id="JavaScriptIntegration"></a>Integrace jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="f6dd2-541"><a id="JavaScriptIntegration"></a>JavaScript integration</span></span>
<span data-ttu-id="f6dd2-542">Azure Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na hello kolekce z hlediska uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-542">Azure Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections in terms of stored procedures and triggers.</span></span> <span data-ttu-id="f6dd2-543">To umožňuje, aby obě:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-543">This allows for both:</span></span>

* <span data-ttu-id="f6dd2-544">Možnost toodo vysoce výkonné transakční operace CRUD a dotazy na dokumenty v kolekci na základě hello těsná integrace běhu programu JavaScript přímo v rámci hello databázového stroje.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-544">Ability toodo high-performance transactional CRUD operations and queries against documents in a collection by virtue of hello deep integration of JavaScript runtime directly within hello database engine.</span></span> 
* <span data-ttu-id="f6dd2-545">Fyzická modelování tok řízení, proměnné rozsahu a přiřazení a integrace výjimky zpracování primitiv s databázové transakce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-545">A natural modeling of control flow, variable scoping, and assignment and integration of exception handling primitives with database transactions.</span></span> <span data-ttu-id="f6dd2-546">Další informace o podpoře Azure Cosmos DB integrace jazyka JavaScript naleznete v toohello JavaScript na straně serveru programovatelnosti dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-546">For more details about Azure Cosmos DB support for JavaScript integration, please refer toohello JavaScript server-side programmability documentation.</span></span>

### <span data-ttu-id="f6dd2-547"><a id="UserDefinedFunctions"></a>Uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-547"><a id="UserDefinedFunctions"></a>User-Defined Functions (UDFs)</span></span>
<span data-ttu-id="f6dd2-548">Společně s typy hello už definované v tomto článku DocumentDB SQL rozhraní API poskytuje podporu pro uživatele definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-548">Along with hello types already defined in this article, DocumentDB API SQL provides support for User Defined Functions (UDF).</span></span> <span data-ttu-id="f6dd2-549">Skalární funkce UDF zejména, jsou podporovány, kde hello vývojáři můžete předat v počtu nula či více argumentů a vrácení zpět výsledku jeden argument.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-549">In particular, scalar UDFs are supported where hello developers can pass in zero or many arguments and return a single argument result back.</span></span> <span data-ttu-id="f6dd2-550">Každý z těchto argumentů, se kontroluje na právě platné hodnoty na JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-550">Each of these arguments is checked for being legal JSON values.</span></span>  

<span data-ttu-id="f6dd2-551">Hello syntaxi DocumentDB SQL rozhraní API je rozšířeno toosupport vlastní logiky aplikace pomocí tyto funkce definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-551">hello DocumentDB API SQL syntax is extended toosupport custom application logic using these User-Defined Functions.</span></span> <span data-ttu-id="f6dd2-552">Funkce UDF lze registrovat pomocí rozhraní API DocumentDB a pak odkazuje v rámci dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-552">UDFs can be registered with DocumentDB API and then be referenced as part of a SQL query.</span></span> <span data-ttu-id="f6dd2-553">Ve skutečnosti hello UDF jsou exquisitely určená toobe vyvolané dotazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-553">In fact, hello UDFs are exquisitely designed toobe invoked by queries.</span></span> <span data-ttu-id="f6dd2-554">Jako volba corollary toothis, funkce UDF nemají objekt kontextu toohello přístup který hello jiných JavaScript mají typy (uložených procedur a aktivačních událostí).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-554">As a corollary toothis choice, UDFs do not have access toohello context object which hello other JavaScript types (stored procedures and triggers) have.</span></span> <span data-ttu-id="f6dd2-555">Vzhledem k tomu, že dotazy se spustí jen pro čtení, mohou spouštět na primární nebo na sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-555">Since queries execute as read-only, they can run either on primary or on secondary replicas.</span></span> <span data-ttu-id="f6dd2-556">Proto UDF jsou navrženou toorun na sekundárních replikách na rozdíl od jiných typů jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-556">Therefore, UDFs are designed toorun on secondary replicas unlike other JavaScript types.</span></span>

<span data-ttu-id="f6dd2-557">Níže je příklad, jak můžete registrovat UDF u databáze hello Cosmos DB, konkrétně v rámci kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-557">Below is an example of how a UDF can be registered at hello Cosmos DB database, specifically under a document collection.</span></span>

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

<span data-ttu-id="f6dd2-558">Hello předchozí příklad vytvoří UDF, jehož název je `REGEX_MATCH`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-558">hello preceding example creates a UDF whose name is `REGEX_MATCH`.</span></span> <span data-ttu-id="f6dd2-559">Přijímá dvou řetězcových hodnot JSON `input` a `pattern` a ověří, zda je první odpovídá hello zadat vzor hello v hello druhý pomocí funkce string.match() jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-559">It accepts two JSON string values `input` and `pattern` and checks if hello first matches hello pattern specified in hello second using JavaScript's string.match() function.</span></span>

<span data-ttu-id="f6dd2-560">Tato UDF jsme teď můžete použít v dotazu v projekci.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-560">We can now use this UDF in a query in a projection.</span></span> <span data-ttu-id="f6dd2-561">Funkce UDF musí být kvalifikovaný pomocí hello malá a velká písmena předponu "udf."</span><span class="sxs-lookup"><span data-stu-id="f6dd2-561">UDFs must be qualified with hello case-sensitive prefix "udf."</span></span> <span data-ttu-id="f6dd2-562">Když je volána v rámci dotazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-562">when called from within queries.</span></span> 

> [!NOTE]
> <span data-ttu-id="f6dd2-563">Předchozí too3/17/2015 Cosmos DB podporované UDF volání bez hello "udf."</span><span class="sxs-lookup"><span data-stu-id="f6dd2-563">Prior too3/17/2015, Cosmos DB supported UDF calls without hello "udf."</span></span> <span data-ttu-id="f6dd2-564">Předpona jako vyberte REGEX_MATCH().</span><span class="sxs-lookup"><span data-stu-id="f6dd2-564">prefix like SELECT REGEX_MATCH().</span></span> <span data-ttu-id="f6dd2-565">Tento vzor volání je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-565">This calling pattern has been deprecated.</span></span>  
> 
> 

<span data-ttu-id="f6dd2-566">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-566">**Query**</span></span>

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

<span data-ttu-id="f6dd2-567">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-567">**Results**</span></span>

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

<span data-ttu-id="f6dd2-568">Hello UDF můžete použít také uvnitř filtr, jak je znázorněno v následujícím příkladu hello, také kvalifikovaný pomocí hello "udf."</span><span class="sxs-lookup"><span data-stu-id="f6dd2-568">hello UDF can also be used inside a filter as shown in hello example below, also qualified with hello "udf."</span></span> <span data-ttu-id="f6dd2-569">Předpona:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-569">prefix:</span></span>

<span data-ttu-id="f6dd2-570">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-570">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

<span data-ttu-id="f6dd2-571">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-571">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


<span data-ttu-id="f6dd2-572">V podstatě UDF jsou platné skalární výrazy a mohou být používány projekce a filtry.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-572">In essence, UDFs are valid scalar expressions and can be used in both projections and filters.</span></span> 

<span data-ttu-id="f6dd2-573">tooexpand na výkon hello UDF, podíváme se na další příklad s podmíněnou logiku:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-573">tooexpand on hello power of UDFs, let's look at another example with conditional logic:</span></span>

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


<span data-ttu-id="f6dd2-574">Dole je příklad, cvičení hello UDF.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-574">Below is an example that exercises hello UDF.</span></span>

<span data-ttu-id="f6dd2-575">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-575">**Query**</span></span>

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

<span data-ttu-id="f6dd2-576">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-576">**Results**</span></span>

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


<span data-ttu-id="f6dd2-577">Jako hello předchozí příklady představením, funkce UDF integrovat hello sílu jazyka JavaScript tooprovide hello DocumentDB SQL rozhraní API bohaté programovatelný rozhraní toodo komplexní procedurální, podmíněného logiku hello pomoci integrované prostředí JavaScript runtime Možnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-577">As hello preceding examples showcase, UDFs integrate hello power of JavaScript language with hello DocumentDB API SQL tooprovide a rich programmable interface toodo complex procedural, conditional logic with hello help of inbuilt JavaScript runtime capabilities.</span></span>

<span data-ttu-id="f6dd2-578">DocumentDB SQL rozhraní API poskytuje hello argumenty toohello UDF pro každý dokument ve zdroji hello, v hello aktuální fázi (klauzuli WHERE nebo klauzuli SELECT) zpracování hello UDF.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-578">DocumentDB API SQL provides hello arguments toohello UDFs for each document in hello source at hello current stage (WHERE clause or SELECT clause) of processing hello UDF.</span></span> <span data-ttu-id="f6dd2-579">Hello výsledek je obsažena v bezproblémově hello celkové spouštěcí kanál.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-579">hello result is incorporated in hello overall execution pipeline seamlessly.</span></span> <span data-ttu-id="f6dd2-580">Pokud hello vlastnosti odkazované tooby hello UDF parametry nejsou k dispozici v hello hodnota JSON hello, že parametr se považuje za není definována a proto hello UDF volání zcela přeskočen.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-580">If hello properties referred tooby hello UDF parameters are not available in hello JSON value, hello parameter is considered as undefined and hence hello UDF invocation is entirely skipped.</span></span> <span data-ttu-id="f6dd2-581">Podobně pokud hello výsledek hello UDF, není součástí hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-581">Similarly if hello result of hello UDF is undefined, it's not included in hello result.</span></span> 

<span data-ttu-id="f6dd2-582">V souhrnu funkce UDF jsou skvělý nástroje toodo komplexní obchodní logiky v rámci dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-582">In summary, UDFs are great tools toodo complex business logic as part of hello query.</span></span>

### <a name="operator-evaluation"></a><span data-ttu-id="f6dd2-583">Vyhodnocení – operátor</span><span class="sxs-lookup"><span data-stu-id="f6dd2-583">Operator evaluation</span></span>
<span data-ttu-id="f6dd2-584">Cosmos databáze, hello důsledku způsobená databáze JSON nevykresluje parallels s operátory jazyka JavaScript a jeho sémantiku vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-584">Cosmos DB, by hello virtue of being a JSON database, draws parallels with JavaScript operators and its evaluation semantics.</span></span> <span data-ttu-id="f6dd2-585">Při Cosmos DB pokusí toopreserve sémantiku JavaScript z hlediska podporu JSON, v některých případech odchylují hello operaci vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-585">While Cosmos DB tries toopreserve JavaScript semantics in terms of JSON support, hello operation evaluation deviates in some instances.</span></span>

<span data-ttu-id="f6dd2-586">V DocumentDB SQL rozhraní API, na rozdíl od v tradiční SQL hello typů hodnot se často není známý dokud hello hodnoty jsou načteny z databáze.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-586">In DocumentDB API SQL, unlike in traditional SQL, hello types of values are often not known until hello values are retrieved from database.</span></span> <span data-ttu-id="f6dd2-587">V pořadí tooefficiently spouštět dotazy, většina hello operátory má požadavky na typ strict.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-587">In order tooefficiently execute queries, most of hello operators have strict type requirements.</span></span> 

<span data-ttu-id="f6dd2-588">DocumentDB API SQL neprovede implicitní převody, na rozdíl od jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-588">DocumentDB API SQL doesn't perform implicit conversions, unlike JavaScript.</span></span> <span data-ttu-id="f6dd2-589">Například dotazu jako `SELECT * FROM Person p WHERE p.Age = 21` odpovídá dokumentů, které obsahují ve vlastnosti stáří, jehož hodnota je 21.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-589">For instance, a query like `SELECT * FROM Person p WHERE p.Age = 21` matches documents that contain an Age property whose value is 21.</span></span> <span data-ttu-id="f6dd2-590">Jiného dokumentu, jejichž stáří vlastnost odpovídá řetězec "21" nebo jiných může být nekonečné variace jako "021", "21.0", "0021", "00021", nebude odpovídat atd.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-590">Any other document whose Age property matches string "21", or other possibly infinite variations like "021", "21.0", "0021", "00021", etc. will not be matched.</span></span> <span data-ttu-id="f6dd2-591">Toto je na rozdíl od toohello JavaScript, kde jsou hodnoty řetězce hello implicitně převedena toonumbers (podle operátoru, například: ==).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-591">This is in contrast toohello JavaScript where hello string values are implicitly casted toonumbers (based on operator, ex: ==).</span></span> <span data-ttu-id="f6dd2-592">Tato volba je zásadní pro efektivní indexu odpovídající v DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-592">This choice is crucial for efficient index matching in DocumentDB API SQL.</span></span> 

## <a name="parameterized-sql-queries"></a><span data-ttu-id="f6dd2-593">Parametrizované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-593">Parameterized SQL queries</span></span>
<span data-ttu-id="f6dd2-594">Cosmos DB podporuje dotazy s parametry vyjádřit s hello známé @ zápis.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-594">Cosmos DB supports queries with parameters expressed with hello familiar @ notation.</span></span> <span data-ttu-id="f6dd2-595">Parametrizované SQL poskytuje robustní zpracování a uvozovací znaky vstup uživatele brání náhodnou expozici dat prostřednictvím Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-595">Parameterized SQL provides robust handling and escaping of user input, preventing accidental exposure of data through SQL injection.</span></span> 

<span data-ttu-id="f6dd2-596">Můžete například napsat dotaz, který přebírá příjmení a stav adresy jako parametry a potom spusťte pro různé hodnoty příjmení a stav adresy založené na vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-596">For example, you can write a query that takes last name and address state as parameters, and then execute it for various values of last name and address state based on user input.</span></span>

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

<span data-ttu-id="f6dd2-597">Tento požadavek potom můžete odeslat tooCosmos DB jako parametrizovaného dotazu JSON, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-597">This request can then be sent tooCosmos DB as a parameterized JSON query like shown below.</span></span>

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

<span data-ttu-id="f6dd2-598">Hello argument tooTOP se dá nastavit pomocí parametrizované dotazy, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-598">hello argument tooTOP can be set using parameterized queries like shown below.</span></span>

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

<span data-ttu-id="f6dd2-599">Hodnoty parametru může být libovolný platný kód JSON (řetězce, čísla, logické hodnoty null, dokonce i pole nebo vnořený JSON).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-599">Parameter values can be any valid JSON (strings, numbers, Booleans, null, even arrays or nested JSON).</span></span> <span data-ttu-id="f6dd2-600">Také bez schématu totiž Cosmos DB parametry nejsou ověřovat na libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-600">Also since Cosmos DB is schema-less, parameters are not validated against any type.</span></span>

## <span data-ttu-id="f6dd2-601"><a id="BuiltinFunctions"></a>Integrované funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-601"><a id="BuiltinFunctions"></a>Built-in functions</span></span>
<span data-ttu-id="f6dd2-602">Cosmos DB podporuje také řadu integrovaných funkcí pro běžné operace, které lze použít uvnitř dotazy jako uživatelsky definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-602">Cosmos DB also supports a number of built-in functions for common operations, that can be used inside queries like user-defined functions (UDFs).</span></span>

| <span data-ttu-id="f6dd2-603">Funkce skupiny</span><span class="sxs-lookup"><span data-stu-id="f6dd2-603">Function group</span></span>          | <span data-ttu-id="f6dd2-604">Operace</span><span class="sxs-lookup"><span data-stu-id="f6dd2-604">Operations</span></span>                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6dd2-605">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-605">Mathematical functions</span></span>  | <span data-ttu-id="f6dd2-606">ABS mezní hodnoty, EXP, FLOOR, protokolu, LOG10, POWER, KRUHOVÉ, přihlášení, SQRT, HRANATÉ, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COP, STUPŇŮ, PI, RADIÁNECH, SIN a TAN</span><span class="sxs-lookup"><span data-stu-id="f6dd2-606">ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, and TAN</span></span> |
| <span data-ttu-id="f6dd2-607">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-607">Type checking functions</span></span> | <span data-ttu-id="f6dd2-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED a IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="f6dd2-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, and IS_PRIMITIVE</span></span>                                                           |
| <span data-ttu-id="f6dd2-609">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-609">String functions</span></span>        | <span data-ttu-id="f6dd2-610">CONCAT, obsahuje, ENDSWITH, INDEX_OF, vlevo, délka, nižší, LTRIM, NAHRAĎTE, REPLIKOVAT, zpětného, vpravo, RTRIM, STARTSWITH, SUBSTRING a horní</span><span class="sxs-lookup"><span data-stu-id="f6dd2-610">CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, and UPPER</span></span>       |
| <span data-ttu-id="f6dd2-611">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="f6dd2-611">Array functions</span></span>         | <span data-ttu-id="f6dd2-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH a ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="f6dd2-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH, and ARRAY_SLICE</span></span>                                                                                         |
| <span data-ttu-id="f6dd2-613">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-613">Spatial functions</span></span>       | <span data-ttu-id="f6dd2-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID a ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="f6dd2-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, and ST_ISVALIDDETAILED</span></span>                                                                           | 

<span data-ttu-id="f6dd2-615">Pokud aktuálně používáte uživatelem definované funkce (UDF) pro kterou integrovaná funkce je nyní k dispozici, byste měli používat hello odpovídající integrovaná funkce, bude rychlejší toorun toobe a další efektivně.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-615">If you’re currently using a user-defined function (UDF) for which a built-in function is now available, you should use hello corresponding built-in function as it is going toobe quicker toorun and more efficiently.</span></span> 

### <a name="mathematical-functions"></a><span data-ttu-id="f6dd2-616">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-616">Mathematical functions</span></span>
<span data-ttu-id="f6dd2-617">Hello matematické funkce každé provedení výpočtu, podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-617">hello mathematical functions each perform a calculation, based on input values that are provided as arguments, and return a numeric value.</span></span> <span data-ttu-id="f6dd2-618">Zde je tabulku podporovaných předdefinovaných matematické funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-618">Here’s a table of supported built-in mathematical functions.</span></span>


| <span data-ttu-id="f6dd2-619">Využití</span><span class="sxs-lookup"><span data-stu-id="f6dd2-619">Usage</span></span> | <span data-ttu-id="f6dd2-620">Popis</span><span class="sxs-lookup"><span data-stu-id="f6dd2-620">Description</span></span> |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6dd2-621">[[ABS (num_expr)](#bk_abs)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-621">[[ABS (num_expr)](#bk_abs)</span></span> | <span data-ttu-id="f6dd2-622">Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-622">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-623">Horní MEZ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-623">CEILING (num_expr)</span></span>](#bk_ceiling) | <span data-ttu-id="f6dd2-624">Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-624">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-625">FLOOR (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-625">FLOOR (num_expr)</span></span>](#bk_floor) | <span data-ttu-id="f6dd2-626">Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-626">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-627">EXP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-627">EXP (num_expr)</span></span>](#bk_exp) | <span data-ttu-id="f6dd2-628">Vrátí exponent hello hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-628">Returns hello exponent of hello specified numeric expression.</span></span> |
| <span data-ttu-id="f6dd2-629">[PROTOKOL (num_expr [, základní])](#bk_log)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-629">[LOG (num_expr [,base])](#bk_log)</span></span> | <span data-ttu-id="f6dd2-630">Vrátí hello přirozený logaritmus hello číselného výrazu, nebo pomocí hello logaritmus hello zadán základní</span><span class="sxs-lookup"><span data-stu-id="f6dd2-630">Returns hello natural logarithm of hello specified numeric expression, or hello logarithm using hello specified base</span></span> |
| [<span data-ttu-id="f6dd2-631">LOG10 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-631">LOG10 (num_expr)</span></span>](#bk_log10) | <span data-ttu-id="f6dd2-632">Vrátí hello základu 10 logaritmické hello zadána hodnota číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-632">Returns hello base-10 logarithmic value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-633">ZAOKROUHLÍ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-633">ROUND (num_expr)</span></span>](#bk_round) | <span data-ttu-id="f6dd2-634">Vrátí číselnou hodnotu, zaokrouhlené toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-634">Returns a numeric value, rounded toohello closest integer value.</span></span> |
| [<span data-ttu-id="f6dd2-635">TRUNC (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-635">TRUNC (num_expr)</span></span>](#bk_trunc) | <span data-ttu-id="f6dd2-636">Vrátí číselnou hodnotu, zkrácený toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-636">Returns a numeric value, truncated toohello closest integer value.</span></span> |
| [<span data-ttu-id="f6dd2-637">SQRT (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-637">SQRT (num_expr)</span></span>](#bk_sqrt) | <span data-ttu-id="f6dd2-638">Vrátí hello druhou odmocninu čísla hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-638">Returns hello square root of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-639">HRANATÉ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-639">SQUARE (num_expr)</span></span>](#bk_square) | <span data-ttu-id="f6dd2-640">Vrátí hello odmocnina z hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-640">Returns hello square of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-641">NAPÁJENÍ (num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-641">POWER (num_expr, num_expr)</span></span>](#bk_power) | <span data-ttu-id="f6dd2-642">Vrátí hello výkon hello zadán toohello hodnotu číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-642">Returns hello power of hello specified numeric expression toohello value specified.</span></span> |
| [<span data-ttu-id="f6dd2-643">PŘIHLÁŠENÍ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-643">SIGN (num_expr)</span></span>](#bk_sign) | <span data-ttu-id="f6dd2-644">Vrátí hello přihlašovací (-1, 0, 1) hello zadána hodnota číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-644">Returns hello sign value (-1, 0, 1) of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-645">ACOS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-645">ACOS (num_expr)</span></span>](#bk_acos) | <span data-ttu-id="f6dd2-646">Vrátí hello úhel v radiánech, jehož kosinus je hello zadaný číselného výrazu; Zkratka Arkus.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-646">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span> |
| [<span data-ttu-id="f6dd2-647">ASIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-647">ASIN (num_expr)</span></span>](#bk_asin) | <span data-ttu-id="f6dd2-648">Vrátí hello úhlu, v radiánech, jehož sinus je hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-648">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="f6dd2-649">To je také označován Arkus sinus.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-649">This is also called arcsine.</span></span> |
| [<span data-ttu-id="f6dd2-650">ATAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-650">ATAN (num_expr)</span></span>](#bk_atan) | <span data-ttu-id="f6dd2-651">Vrátí hello úhlu, v radiánech, jehož tangens je hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-651">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="f6dd2-652">To je také označován Arkus.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-652">This is also called arctangent.</span></span> |
| [<span data-ttu-id="f6dd2-653">ATN2 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-653">ATN2 (num_expr)</span></span>](#bk_atn2) | <span data-ttu-id="f6dd2-654">Vrátí text hello úhel v radiánech, mezi hello kladné osy x a hello ray z hello počátek toohello bodu (y, x), kde x a y jsou hodnoty hello hello dvou výrazů zadaný float.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-654">Returns hello angle, in radians, between hello positive x-axis and hello ray from hello origin toohello point (y, x), where x and y are hello values of hello two specified float expressions.</span></span> |
| [<span data-ttu-id="f6dd2-655">COS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-655">COS (num_expr)</span></span>](#bk_cos) | <span data-ttu-id="f6dd2-656">Vrátí hello trigonometrické kosinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-656">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="f6dd2-657">COP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-657">COT (num_expr)</span></span>](#bk_cot) | <span data-ttu-id="f6dd2-658">Vrátí hello trigonometrické kotangens hello úhlu, uvedeného v radiánech, v hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-658">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span> |
| [<span data-ttu-id="f6dd2-659">STUPŇŮ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-659">DEGREES (num_expr)</span></span>](#bk_degrees) | <span data-ttu-id="f6dd2-660">Vrátí hello odpovídající úhel ve stupních pro úhlu uvedeného v radiánech.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-660">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span> |
| [<span data-ttu-id="f6dd2-661">PI)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-661">PI ()</span></span>](#bk_pi) | <span data-ttu-id="f6dd2-662">Vrátí hello konstantní hodnotu čísla PÍ.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-662">Returns hello constant value of PI.</span></span> |
| [<span data-ttu-id="f6dd2-663">RADIÁNECH (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-663">RADIANS (num_expr)</span></span>](#bk_radians) | <span data-ttu-id="f6dd2-664">Vrátí radiánech při zadání číselného výrazu, ve stupních, se.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-664">Returns radians when a numeric expression, in degrees, is entered.</span></span> |
| [<span data-ttu-id="f6dd2-665">SIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-665">SIN (num_expr)</span></span>](#bk_sin) | <span data-ttu-id="f6dd2-666">Vrátí hello trigonometrické sinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-666">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="f6dd2-667">TAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-667">TAN (num_expr)</span></span>](#bk_tan) | <span data-ttu-id="f6dd2-668">Vrátí tangens hello vstupní výraz hello v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-668">Returns hello tangent of hello input expression, in hello specified expression.</span></span> |

<span data-ttu-id="f6dd2-669">Například můžete spustit nyní dotazy jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-669">For example, you can now run queries like hello following:</span></span>

<span data-ttu-id="f6dd2-670">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-670">**Query**</span></span>

    SELECT VALUE ABS(-4)

<span data-ttu-id="f6dd2-671">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-671">**Results**</span></span>

    [4]

<span data-ttu-id="f6dd2-672">Hello hlavní rozdíl mezi tooANSI funkce porovnání Cosmos databáze SQL je, že jsou navrženou toowork i s daty bez schématu a smíšený schématu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-672">hello main difference between Cosmos DB’s functions compared tooANSI SQL is that they are designed toowork well with schema-less and mixed schema data.</span></span> <span data-ttu-id="f6dd2-673">Například pokud máte dokument, kdy hello velikost vlastnost chybí, nebo má jiné než číselné hodnoty jako "Neznámý" a potom dokument hello je přeskočil, místo vrátila chybu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-673">For example, if you have a document where hello Size property is missing, or has a non-numeric value like “unknown”, then hello document is skipped over, instead of returning an error.</span></span>

### <a name="type-checking-functions"></a><span data-ttu-id="f6dd2-674">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-674">Type checking functions</span></span>
<span data-ttu-id="f6dd2-675">Kontrola, zda Funkce Hello typu Povolit toocheck hello typ výrazu v rámci dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-675">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span> <span data-ttu-id="f6dd2-676">Typ funkce Kontrola, zda lze použít toodetermine hello typ vlastnosti v rámci dokumenty v chodu hello, když je neznámý nebo proměnné.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-676">Type checking functions can be used toodetermine hello type of properties within documents on hello fly when it is variable or unknown.</span></span> <span data-ttu-id="f6dd2-677">Zde je tabulku kontroluje funkce podporované předdefinovaný typ.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-677">Here’s a table of supported built-in type checking functions.</span></span>

<table>
<tr>
  <td><span data-ttu-id="f6dd2-678"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="f6dd2-678"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="f6dd2-679"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="f6dd2-679"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-681">Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-681">Returns a Boolean indicating if hello type of hello value is an array.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-683">Vrátí logickou hodnotu udávající, pokud hello typ hodnoty hello je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-683">Returns a Boolean indicating if hello type of hello value is a Boolean.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-685">Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-685">Returns a Boolean indicating if hello type of hello value is null.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-687">Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty number.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-687">Returns a Boolean indicating if hello type of hello value is a number.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-689">Vrátí logickou hodnotu udávající, pokud hello typ hodnoty hello je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-689">Returns a Boolean indicating if hello type of hello value is a JSON object.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-691">Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty string.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-691">Returns a Boolean indicating if hello type of hello value is a string.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-693">Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-693">Returns a Boolean indicating if hello property has been assigned a value.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="f6dd2-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></span></span></td>
  <td><span data-ttu-id="f6dd2-695">Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty řetězce, číslo, logickou hodnotu nebo hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-695">Returns a Boolean indicating if hello type of hello value is a string, number, Boolean or null.</span></span></td>
</tr>

</table>

<span data-ttu-id="f6dd2-696">Pomocí těchto funkcí, teď můžete spouštět dotazy jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-696">Using these functions, you can now run queries like hello following:</span></span>

<span data-ttu-id="f6dd2-697">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-697">**Query**</span></span>

    SELECT VALUE IS_NUMBER(-4)

<span data-ttu-id="f6dd2-698">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-698">**Results**</span></span>

    [true]

### <a name="string-functions"></a><span data-ttu-id="f6dd2-699">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-699">String functions</span></span>
<span data-ttu-id="f6dd2-700">Hello následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-700">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span> <span data-ttu-id="f6dd2-701">Tady je tabulku funkce integrované řetězce:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-701">Here's a table of built-in string functions:</span></span>

| <span data-ttu-id="f6dd2-702">Využití</span><span class="sxs-lookup"><span data-stu-id="f6dd2-702">Usage</span></span> | <span data-ttu-id="f6dd2-703">Popis</span><span class="sxs-lookup"><span data-stu-id="f6dd2-703">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="f6dd2-704">Délka (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-704">LENGTH (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |<span data-ttu-id="f6dd2-705">Vrátí hello počet znaků, který hello zadán řetězcovým výrazem</span><span class="sxs-lookup"><span data-stu-id="f6dd2-705">Returns hello number of characters of hello specified string expression</span></span> |
| <span data-ttu-id="f6dd2-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span></span> |<span data-ttu-id="f6dd2-707">Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-707">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| [<span data-ttu-id="f6dd2-708">SUBSTRING (str_expr, num_expr num_expr.)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-708">SUBSTRING (str_expr, num_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |<span data-ttu-id="f6dd2-709">Vrátí část řetězcového výrazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-709">Returns part of a string expression.</span></span> |
| [<span data-ttu-id="f6dd2-710">STARTSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-710">STARTSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |<span data-ttu-id="f6dd2-711">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý</span><span class="sxs-lookup"><span data-stu-id="f6dd2-711">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="f6dd2-712">ENDSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-712">ENDSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |<span data-ttu-id="f6dd2-713">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý</span><span class="sxs-lookup"><span data-stu-id="f6dd2-713">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="f6dd2-714">OBSAHUJE (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-714">CONTAINS (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |<span data-ttu-id="f6dd2-715">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-715">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |
| [<span data-ttu-id="f6dd2-716">INDEX_OF (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-716">INDEX_OF (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |<span data-ttu-id="f6dd2-717">Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-717">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span> |
| [<span data-ttu-id="f6dd2-718">LEFT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-718">LEFT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |<span data-ttu-id="f6dd2-719">Vrátí hello levé části řetězce s hello zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-719">Returns hello left part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="f6dd2-720">RIGHT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-720">RIGHT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |<span data-ttu-id="f6dd2-721">Vrátí hello právo součástí řetězec s hello zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-721">Returns hello right part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="f6dd2-722">LTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-722">LTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |<span data-ttu-id="f6dd2-723">Vrací výraz řetězce po ho odebere úvodní mezery.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-723">Returns a string expression after it removes leading blanks.</span></span> |
| [<span data-ttu-id="f6dd2-724">RTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-724">RTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |<span data-ttu-id="f6dd2-725">Vrací výraz řetězce po zkracování všechny koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-725">Returns a string expression after truncating all trailing blanks.</span></span> |
| [<span data-ttu-id="f6dd2-726">NIŽŠÍ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-726">LOWER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |<span data-ttu-id="f6dd2-727">Vrací výraz řetězce po převodu dat toolowercase velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-727">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| [<span data-ttu-id="f6dd2-728">HORNÍ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-728">UPPER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |<span data-ttu-id="f6dd2-729">Vrací výraz řetězce po převodu dat toouppercase malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-729">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| [<span data-ttu-id="f6dd2-730">NAHRAĎTE (str_expr, str_expr str_expr.)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-730">REPLACE (str_expr, str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |<span data-ttu-id="f6dd2-731">Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-731">Replaces all occurrences of a specified string value with another string value.</span></span> |
| [<span data-ttu-id="f6dd2-732">REPLIKOVAT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-732">REPLICATE (str_expr, num_expr)</span></span>](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |<span data-ttu-id="f6dd2-733">Opakuje hodnotu řetězce zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-733">Repeats a string value a specified number of times.</span></span> |
| [<span data-ttu-id="f6dd2-734">ZPĚTNÉHO (str_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-734">REVERSE (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |<span data-ttu-id="f6dd2-735">Vrátí hello zpětné pořadí řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-735">Returns hello reverse order of a string value.</span></span> |

<span data-ttu-id="f6dd2-736">Pomocí těchto funkcí, můžete nyní spustit dotazy jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-736">Using these functions, you can now run queries like hello following.</span></span> <span data-ttu-id="f6dd2-737">Například se můžete vrátit název rodiny hello na velká písmena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-737">For example, you can return hello family name in uppercase as follows:</span></span>

<span data-ttu-id="f6dd2-738">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-738">**Query**</span></span>

    SELECT VALUE UPPER(Families.id)
    FROM Families

<span data-ttu-id="f6dd2-739">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-739">**Results**</span></span>

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

<span data-ttu-id="f6dd2-740">Nebo zřetězení řetězců jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-740">Or concatenate strings like in this example:</span></span>

<span data-ttu-id="f6dd2-741">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-741">**Query**</span></span>

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

<span data-ttu-id="f6dd2-742">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-742">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


<span data-ttu-id="f6dd2-743">Funkce řetězce mohou sloužit také v hello kde klauzule toofilter výsledky, stejně jako v nástroji hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-743">String functions can also be used in hello WHERE clause toofilter results, like in hello following example:</span></span>

<span data-ttu-id="f6dd2-744">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-744">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

<span data-ttu-id="f6dd2-745">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-745">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a><span data-ttu-id="f6dd2-746">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="f6dd2-746">Array functions</span></span>
<span data-ttu-id="f6dd2-747">Hello následující skalární funkce provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-747">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span> <span data-ttu-id="f6dd2-748">Zde je také tabulka předdefinovaných pole funkcí:</span><span class="sxs-lookup"><span data-stu-id="f6dd2-748">Here's a table of built-in array functions:</span></span>

| <span data-ttu-id="f6dd2-749">Využití</span><span class="sxs-lookup"><span data-stu-id="f6dd2-749">Usage</span></span> | <span data-ttu-id="f6dd2-750">Popis</span><span class="sxs-lookup"><span data-stu-id="f6dd2-750">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="f6dd2-751">ARRAY_LENGTH (arr_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-751">ARRAY_LENGTH (arr_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |<span data-ttu-id="f6dd2-752">Vrátí číslo hello elementů hello zadaný výraz pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-752">Returns hello number of elements of hello specified array expression.</span></span> |
| <span data-ttu-id="f6dd2-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span></span> |<span data-ttu-id="f6dd2-754">Vrátí pole, které je výsledkem hello zřetězení dvě nebo více hodnot pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-754">Returns an array that is hello result of concatenating two or more array values.</span></span> |
| <span data-ttu-id="f6dd2-755">[ARRAY_CONTAINS (arr_expr, výraz [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-755">[ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span></span> |<span data-ttu-id="f6dd2-756">Vrátí logickou hodnotu udávající, zda text hello pole obsahuje hello zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-756">Returns a Boolean indicating whether hello array contains hello specified value.</span></span> <span data-ttu-id="f6dd2-757">Můžete zadat, pokud je shoda hello celé nebo jeho část.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-757">Can specify if hello match is full or partial.</span></span> |
| <span data-ttu-id="f6dd2-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span></span> |<span data-ttu-id="f6dd2-759">Vrátí část výraz pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-759">Returns part of an array expression.</span></span> |

<span data-ttu-id="f6dd2-760">Pole funkce se dá použít toomanipulate pole v rámci JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-760">Array functions can be used toomanipulate arrays within JSON.</span></span> <span data-ttu-id="f6dd2-761">Tady je příklad dotaz, který vrátí všechny dokumenty, kde jedním z nadřazené položky hello je "Každý s každým Wakefieldů".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-761">For example, here's a query that returns all documents where one of hello parents is "Robin Wakefield".</span></span> 

<span data-ttu-id="f6dd2-762">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-762">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

<span data-ttu-id="f6dd2-763">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-763">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="f6dd2-764">Můžete zadat částečné fragment pro odpovídající elementů v rámci pole hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-764">You can specify a partial fragment for matching elements within hello array.</span></span> <span data-ttu-id="f6dd2-765">Hello následující dotaz hledá všechny nadřazené objekty s hello `givenName` z `Robin`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-765">hello following query finds all parents with hello `givenName` of `Robin`.</span></span>

<span data-ttu-id="f6dd2-766">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-766">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

<span data-ttu-id="f6dd2-767">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-767">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]


<span data-ttu-id="f6dd2-768">Zde je další příklad používající ARRAY_LENGTH tooget hello počet podřízených prvků na rodiny.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-768">Here's another example that uses ARRAY_LENGTH tooget hello number of children per family.</span></span>

<span data-ttu-id="f6dd2-769">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-769">**Query**</span></span>

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

<span data-ttu-id="f6dd2-770">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-770">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a><span data-ttu-id="f6dd2-771">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-771">Spatial functions</span></span>
<span data-ttu-id="f6dd2-772">Cosmos DB podporuje následující otevřete geoprostorové Consortium (OGC) integrované funkce pro dotazování geoprostorové hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-772">Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> 

<table>
<tr>
  <td><span data-ttu-id="f6dd2-773"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="f6dd2-773"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="f6dd2-774"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="f6dd2-774"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-775">ST_DISTANCE (point_expr, point_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-775">ST_DISTANCE (point_expr, point_expr)</span></span></td>
  <td><span data-ttu-id="f6dd2-776">Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-776">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-777">ST_WITHIN (point_expr, polygon_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-777">ST_WITHIN (point_expr, polygon_expr)</span></span></td>
  <td><span data-ttu-id="f6dd2-778">Vrací výraz logická hodnota určující, zda text hello první GeoJSON objekt (bod, mnohoúhelníku nebo LineString) je v rámci hello druhý GeoJSON objekt (bod, mnohoúhelníku nebo LineString).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-778">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="f6dd2-780">Vrátí hodnotu označující, zda text hello dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-780">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-781">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="f6dd2-781">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="f6dd2-782">Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-782">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="f6dd2-783">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="f6dd2-783">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="f6dd2-784">Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-784">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="f6dd2-785">Prostorové funkce můžou být použité tooperform blízkosti dotazy pro prostorová data.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-785">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="f6dd2-786">Tady je příklad dotaz, který vrátí že všechny rodiny dokumenty, v rámci 30 km hello zadané umístění používáte hello ST_DISTANCE integrovaná funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-786">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="f6dd2-787">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-787">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="f6dd2-788">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-788">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="f6dd2-789">Další informace o podporovaných geoprostorové v Cosmos DB, najdete v tématu [práci s daty geoprostorové v Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-789">For more details on geospatial support in Cosmos DB, please see [Working with geospatial data in Azure Cosmos DB](geospatial.md).</span></span> <span data-ttu-id="f6dd2-790">Který zabalí prostorových funkce a hello syntaxe SQL pro Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-790">That wraps up spatial functions, and hello SQL syntax for Cosmos DB.</span></span> <span data-ttu-id="f6dd2-791">Nyní Podívejme se na tom, jak LINQ dotazování funguje a jak komunikuje se syntaxí hello jsme viděli dosavadní.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-791">Now let's take a look at how LINQ querying works and how it interacts with hello syntax we've seen so far.</span></span>

## <span data-ttu-id="f6dd2-792"><a id="Linq"></a>LINQ tooDocumentDB SQL rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f6dd2-792"><a id="Linq"></a>LINQ tooDocumentDB API SQL</span></span>
<span data-ttu-id="f6dd2-793">LINQ je programovací model rozhraní .NET, která vyjadřuje výpočetní jako dotazy na datové proudy objektů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-793">LINQ is a .NET programming model that expresses computation as queries on streams of objects.</span></span> <span data-ttu-id="f6dd2-794">Cosmos DB poskytuje knihovna klienta toointerface s dotazy LINQ usnadněním převod mezi objekty JSON a rozhraní .NET a mapování z určité podmnožiny LINQ dotazy tooCosmos DB dotazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-794">Cosmos DB provides a client-side library toointerface with LINQ by facilitating a conversion between JSON and .NET objects and a mapping from a subset of LINQ queries tooCosmos DB queries.</span></span> 

<span data-ttu-id="f6dd2-795">Hello obrázek níže ukazuje architekturu hello podporu dotazů LINQ pomocí Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-795">hello picture below shows hello architecture of supporting LINQ queries using Cosmos DB.</span></span>  <span data-ttu-id="f6dd2-796">Pomocí klienta aplikace hello Cosmos DB vývojáři můžou vytvářet **IQueryable** objektu, že přímo dotazy hello Cosmos DB dotaz na zprostředkovatele, který pak překládá dotaz LINQ hello do dotazu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-796">Using hello Cosmos DB client, developers can create an **IQueryable** object that directly queries hello Cosmos DB query provider, which then translates hello LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="f6dd2-797">Hello dotazu je předána toohello tooretrieve Cosmos DB serveru a sadu výsledků ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-797">hello query is then passed toohello Cosmos DB server tooretrieve a set of results in JSON format.</span></span> <span data-ttu-id="f6dd2-798">výsledky jsou deserializovat do datového proudu objektů .NET na straně klienta hello vrátit Hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-798">hello returned results are deserialized into a stream of .NET objects on hello client side.</span></span>

![Architektura podporu dotazů LINQ pomocí rozhraní API DocumentDB - syntaxe SQL, JSON dotazovací jazyk, databázových koncepcí a dotazy SQL][1]

### <a name="net-and-json-mapping"></a><span data-ttu-id="f6dd2-800">Rozhraní .NET a mapování JSON</span><span class="sxs-lookup"><span data-stu-id="f6dd2-800">.NET and JSON mapping</span></span>
<span data-ttu-id="f6dd2-801">Hello mapování mezi objekty .NET a dokumenty JSON je přirozené – každé datové pole, člen je mapován objekt JSON tooa, kde je název pole hello namapované část "klíč" toohello objektu hello a část "value" hello je součástí hodnota toohello rekurzivně namapované hello objektu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-801">hello mapping between .NET objects and JSON documents is natural - each data member field is mapped tooa JSON object, where hello field name is mapped toohello "key" part of hello object and hello "value" part is recursively mapped toohello value part of hello object.</span></span> <span data-ttu-id="f6dd2-802">Vezměte v úvahu hello následující ukázka: objekt rodiny hello vytvořený je namapované toohello dokumentu JSON, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-802">Consider hello following example: hello Family object created is mapped toohello JSON document as shown below.</span></span> <span data-ttu-id="f6dd2-803">A naopak a hello dokumentu JSON je namapované back tooa objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-803">And vice versa, hello JSON document is mapped back tooa .NET object.</span></span>

<span data-ttu-id="f6dd2-804">**C# – třída**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-804">**C# Class**</span></span>

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


<span data-ttu-id="f6dd2-805">**JSON**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-805">**JSON**</span></span>  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-toosql-translation"></a><span data-ttu-id="f6dd2-806">Překlad tooSQL LINQ</span><span class="sxs-lookup"><span data-stu-id="f6dd2-806">LINQ tooSQL translation</span></span>
<span data-ttu-id="f6dd2-807">Hello Cosmos DB dotaz na zprostředkovatele provede nejlepší úsilí mapování z dotazu LINQ do dotazu Cosmos DB SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-807">hello Cosmos DB query provider performs a best effort mapping from a LINQ query into a Cosmos DB SQL query.</span></span> <span data-ttu-id="f6dd2-808">V následující popis hello předpokládáme, že hello čtečky má základní znalosti o LINQ.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-808">In hello following description, we assume hello reader has a basic familiarity of LINQ.</span></span>

<span data-ttu-id="f6dd2-809">Nejprve hello typ systému, budeme podporovat všechny JSON primitivní typy – číselnými typy, logická hodnota, string a hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-809">First, for hello type system, we support all JSON primitive types – numeric types, boolean, string, and null.</span></span> <span data-ttu-id="f6dd2-810">Jsou podporovány pouze tyto typy JSON.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-810">Only these JSON types are supported.</span></span> <span data-ttu-id="f6dd2-811">jsou podporovány následující skalární výrazy Hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-811">hello following scalar expressions are supported.</span></span>

* <span data-ttu-id="f6dd2-812">Konstantní hodnoty – patří konstantní hodnoty hello primitivní datové typy ve hello dobu, kdy je vyhodnocován dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-812">Constant values – these include constant values of hello primitive data types at hello time hello query is evaluated.</span></span>
* <span data-ttu-id="f6dd2-813">Vlastnost nebo pole indexu výrazy – tyto výrazy odkazovat toohello vlastnosti objektu nebo k elementu pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-813">Property/array index expressions – these expressions refer toohello property of an object or an array element.</span></span>
  
     <span data-ttu-id="f6dd2-814">rodiny. ID;    Family.Children[0].familyName;    Family.Children[0].Grade;    Family.Children[n].Grade; n je proměnná typu int.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-814">family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is an int variable</span></span>
* <span data-ttu-id="f6dd2-815">Aritmetických výrazech – jedná se o běžné aritmetických výrazech na číselné a logické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-815">Arithmetic expressions - These include common arithmetic expressions on numerical and boolean values.</span></span> <span data-ttu-id="f6dd2-816">Úplný seznam hello najdete v části specifikace toohello SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-816">For hello complete list, refer toohello SQL specification.</span></span>
  
     <span data-ttu-id="f6dd2-817">2 * family.children[0].grade;    x a y;</span><span class="sxs-lookup"><span data-stu-id="f6dd2-817">2 * family.children[0].grade;    x + y;</span></span>
* <span data-ttu-id="f6dd2-818">Výraz pro porovnání řetězce - patří porovnávání řetězcovou hodnotu toosome konstantní řetězec hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-818">String comparison expression - these include comparing a string value toosome constant string value.</span></span>  
  
     <span data-ttu-id="f6dd2-819">mother.familyName == "Smith";    child.givenName == s; s je proměnná řetězce</span><span class="sxs-lookup"><span data-stu-id="f6dd2-819">mother.familyName == "Smith";    child.givenName == s; //s is a string variable</span></span>
* <span data-ttu-id="f6dd2-820">Objekt nebo pole vytvoření výrazu - tyto výrazy návratový typ složené hodnoty nebo anonymní typ objekt nebo pole tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-820">Object/array creation expression - these expressions return an object of compound value type or anonymous type or an array of such objects.</span></span> <span data-ttu-id="f6dd2-821">Tyto hodnoty mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-821">These values can be nested.</span></span>
  
     <span data-ttu-id="f6dd2-822">novou nadřazenou položku {familyName = "Smith", givenName = "Jan"}; nové {nejprve = 1, druhý = 2}; anonymní typ s dvě pole</span><span class="sxs-lookup"><span data-stu-id="f6dd2-822">new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //an anonymous type with two fields</span></span>              
     <span data-ttu-id="f6dd2-823">New [] int {3, child.grade, 5};</span><span class="sxs-lookup"><span data-stu-id="f6dd2-823">new int[] { 3, child.grade, 5 };</span></span>

### <span data-ttu-id="f6dd2-824"><a id="SupportedLinqOperators"></a>Seznam podporovaných operátory LINQ</span><span class="sxs-lookup"><span data-stu-id="f6dd2-824"><a id="SupportedLinqOperators"></a>List of supported LINQ operators</span></span>
<span data-ttu-id="f6dd2-825">Tady je seznam podporovaných LINQ operátory ve zprostředkovateli LINQ hello součástí hello DocumentDB .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-825">Here is a list of supported LINQ operators in hello LINQ provider included with hello DocumentDB .NET SDK.</span></span>

* <span data-ttu-id="f6dd2-826">**Vyberte**: projekce převede toohello SQL SELECT, včetně vytváření objektů</span><span class="sxs-lookup"><span data-stu-id="f6dd2-826">**Select**: Projections translate toohello SQL SELECT including object construction</span></span>
* <span data-ttu-id="f6dd2-827">**Kde**: filtry převede toohello kde SQL a podporovat překlad mezi & &, || a!</span><span class="sxs-lookup"><span data-stu-id="f6dd2-827">**Where**: Filters translate toohello SQL WHERE, and support translation between && , || and !</span></span> <span data-ttu-id="f6dd2-828">operátory toohello SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-828">toohello SQL operators</span></span>
* <span data-ttu-id="f6dd2-829">**Označit více**: umožňuje unwinding klauzule SQL JOIN toohello pole.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-829">**SelectMany**: Allows unwinding of arrays toohello SQL JOIN clause.</span></span> <span data-ttu-id="f6dd2-830">Může být toofilter výrazy použité toochain/vnoření na elementy pole</span><span class="sxs-lookup"><span data-stu-id="f6dd2-830">Can be used toochain/nest expressions toofilter on array elements</span></span>
* <span data-ttu-id="f6dd2-831">**OrderBy a OrderByDescending**: překládá tooORDER BY pořadí</span><span class="sxs-lookup"><span data-stu-id="f6dd2-831">**OrderBy and OrderByDescending**: Translates tooORDER BY ascending/descending</span></span>
* <span data-ttu-id="f6dd2-832">**Počet**, **součet**, **Min**, **maximální**, a **průměrná** operátory pro agregaci a jejich ekvivalenty asynchronní  **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, a **AverageAsync**.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-832">**Count**, **Sum**, **Min**, **Max**, and **Average** operators for aggregation, and their async equivalents **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, and **AverageAsync**.</span></span>
* <span data-ttu-id="f6dd2-833">**CompareTo**: překládá toorange porovnání.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-833">**CompareTo**: Translates toorange comparisons.</span></span> <span data-ttu-id="f6dd2-834">Běžně používané pro řetězce vzhledem k tomu, že nejste porovnatelný v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f6dd2-834">Commonly used for strings since they’re not comparable in .NET</span></span>
* <span data-ttu-id="f6dd2-835">**Trvat**: překládá toohello horní SQL pro omezení výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="f6dd2-835">**Take**: Translates toohello SQL TOP for limiting results from a query</span></span>
* <span data-ttu-id="f6dd2-836">**Matematické funkce**: podporuje překlad z. Asin Abs Acos, je NET, Atan, Ceiling Cos, Exp, Floor, protokolu, Log10, Pow, kruhové, přihlášení, Sin, Sqrt, Tan, zkrátit toohello ekvivalentní SQL integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-836">**Math Functions**: Supports translation from .NET’s Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="f6dd2-837">**Funkce pro řetězce**: podporuje překlad z. NET na Concat, obsahuje, EndsWith, IndexOf, počet, ToLower, TrimStart, nahraďte, zpětného, TrimEnd, StartsWith, dílčí řetězec, ToUpper toohello ekvivalentní SQL integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-837">**String Functions**: Supports translation from .NET’s Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="f6dd2-838">**Pole funkce**: podporuje překlad z. NET na Concat, obsahuje a počet toohello ekvivalentní SQL integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-838">**Array Functions**: Supports translation from .NET’s Concat, Contains, and Count toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="f6dd2-839">**Funkce rozšíření geoprostorové**: podporuje překlad z metod se zakázaným inzerováním vzdálenost v rámci IsValid a IsValidDetailed toohello ekvivalentní SQL integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-839">**Geospatial Extension Functions**: Supports translation from stub methods Distance, Within, IsValid, and IsValidDetailed toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="f6dd2-840">**Uživatelem definované funkce rozšíření funkce**: podporuje překlad z hello zóny se zakázaným inzerováním metoda UserDefinedFunctionProvider.Invoke toohello odpovídající uživatelem definované funkce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-840">**User-Defined Function Extension Function**: Supports translation from hello stub method UserDefinedFunctionProvider.Invoke toohello corresponding user-defined function.</span></span>
* <span data-ttu-id="f6dd2-841">**Různé**: podporuje překlad hello sloučení a podmíněné operátory.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-841">**Miscellaneous**: Supports translation of hello coalesce and conditional operators.</span></span> <span data-ttu-id="f6dd2-842">Může překládat obsahuje tooString obsahuje, ARRAY_CONTAINS nebo hello v SQL v závislosti na kontextu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-842">Can translate Contains tooString CONTAINS, ARRAY_CONTAINS, or hello SQL IN depending on context.</span></span>

### <a name="sql-query-operators"></a><span data-ttu-id="f6dd2-843">Operátory dotazu SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-843">SQL query operators</span></span>
<span data-ttu-id="f6dd2-844">Zde jsou některé příklady, které ilustrují, jak některé hello standardní operátory dotazu LINQ přeložit dolů tooCosmos DB dotazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-844">Here are some examples that illustrate how some of hello standard LINQ query operators are translated down tooCosmos DB queries.</span></span>

#### <a name="select-operator"></a><span data-ttu-id="f6dd2-845">Select – operátor</span><span class="sxs-lookup"><span data-stu-id="f6dd2-845">Select Operator</span></span>
<span data-ttu-id="f6dd2-846">Syntaxe Hello je `input.Select(x => f(x))`, kde `f` je skalární výraz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-846">hello syntax is `input.Select(x => f(x))`, where `f` is a scalar expression.</span></span>

<span data-ttu-id="f6dd2-847">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-847">**LINQ lambda expression**</span></span>

    input.Select(family => family.parents[0].familyName);

<span data-ttu-id="f6dd2-848">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-848">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



<span data-ttu-id="f6dd2-849">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-849">**LINQ lambda expression**</span></span>

    input.Select(family => family.children[0].grade + c); // c is an int variable


<span data-ttu-id="f6dd2-850">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-850">**SQL**</span></span> 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



<span data-ttu-id="f6dd2-851">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-851">**LINQ lambda expression**</span></span>

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


<span data-ttu-id="f6dd2-852">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-852">**SQL**</span></span> 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a><span data-ttu-id="f6dd2-853">Označit více – operátor</span><span class="sxs-lookup"><span data-stu-id="f6dd2-853">SelectMany operator</span></span>
<span data-ttu-id="f6dd2-854">Syntaxe Hello je `input.SelectMany(x => f(x))`, kde `f` se skalární výraz, který vrátí typ kolekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-854">hello syntax is `input.SelectMany(x => f(x))`, where `f` is a scalar expression that returns a collection type.</span></span>

<span data-ttu-id="f6dd2-855">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-855">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children);

<span data-ttu-id="f6dd2-856">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-856">**SQL**</span></span> 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a><span data-ttu-id="f6dd2-857">Kde – operátor</span><span class="sxs-lookup"><span data-stu-id="f6dd2-857">Where operator</span></span>
<span data-ttu-id="f6dd2-858">Syntaxe Hello je `input.Where(x => f(x))`, kde `f` je skalární výraz, který vrací logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-858">hello syntax is `input.Where(x => f(x))`, where `f` is a scalar expression, which returns a Boolean value.</span></span>

<span data-ttu-id="f6dd2-859">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-859">**LINQ lambda expression**</span></span>

    input.Where(family=> family.parents[0].familyName == "Smith");

<span data-ttu-id="f6dd2-860">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-860">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



<span data-ttu-id="f6dd2-861">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-861">**LINQ lambda expression**</span></span>

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

<span data-ttu-id="f6dd2-862">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-862">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a><span data-ttu-id="f6dd2-863">Složené příkazy jazyka SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-863">Composite SQL queries</span></span>
<span data-ttu-id="f6dd2-864">Hello výše operátory může být složený tooform více výkonných dotazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-864">hello above operators can be composed tooform more powerful queries.</span></span> <span data-ttu-id="f6dd2-865">Vzhledem k tomu, že Cosmos DB podporuje vnořené kolekcí, hello složení můžete být zřetězen nebo vnořený.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-865">Since Cosmos DB supports nested collections, hello composition can either be concatenated or nested.</span></span>

#### <a name="concatenation"></a><span data-ttu-id="f6dd2-866">Zřetězení</span><span class="sxs-lookup"><span data-stu-id="f6dd2-866">Concatenation</span></span>
<span data-ttu-id="f6dd2-867">Syntaxe Hello je `input(.|.SelectMany())(.Select()|.Where())*`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-867">hello syntax is `input(.|.SelectMany())(.Select()|.Where())*`.</span></span> <span data-ttu-id="f6dd2-868">Zřetězené dotaz můžete spustit v volitelný `SelectMany` dotazu následuje více `Select` nebo `Where` operátory.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-868">A concatenated query can start with an optional `SelectMany` query followed by multiple `Select` or `Where` operators.</span></span>

<span data-ttu-id="f6dd2-869">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-869">**LINQ lambda expression**</span></span>

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

<span data-ttu-id="f6dd2-870">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-870">**SQL**</span></span>

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



<span data-ttu-id="f6dd2-871">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-871">**LINQ lambda expression**</span></span>

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

<span data-ttu-id="f6dd2-872">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-872">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



<span data-ttu-id="f6dd2-873">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-873">**LINQ lambda expression**</span></span>

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

<span data-ttu-id="f6dd2-874">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-874">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



<span data-ttu-id="f6dd2-875">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-875">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

<span data-ttu-id="f6dd2-876">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-876">**SQL**</span></span> 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a><span data-ttu-id="f6dd2-877">Vnoření</span><span class="sxs-lookup"><span data-stu-id="f6dd2-877">Nesting</span></span>
<span data-ttu-id="f6dd2-878">Syntaxe Hello je `input.SelectMany(x=>x.Q())` kde Q je `Select`, `SelectMany`, nebo `Where` operátor.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-878">hello syntax is `input.SelectMany(x=>x.Q())` where Q is a `Select`, `SelectMany`, or `Where` operator.</span></span>

<span data-ttu-id="f6dd2-879">Vnitřní dotaz hello v vnořený dotaz, je použité tooeach elementu hello vnější kolekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-879">In a nested query, hello inner query is applied tooeach element of hello outer collection.</span></span> <span data-ttu-id="f6dd2-880">Jednou z důležitou součást je, že takový dotaz vnitřní hello najdete toohello pole hello elementů v kolekci vnější hello jako spojení sama na sebe.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-880">One important feature is that hello inner query can refer toohello fields of hello elements in hello outer collection like self-joins.</span></span>

<span data-ttu-id="f6dd2-881">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-881">**LINQ lambda expression**</span></span>

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

<span data-ttu-id="f6dd2-882">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-882">**SQL**</span></span> 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


<span data-ttu-id="f6dd2-883">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-883">**LINQ lambda expression**</span></span>

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

<span data-ttu-id="f6dd2-884">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-884">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



<span data-ttu-id="f6dd2-885">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-885">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

<span data-ttu-id="f6dd2-886">**SQL**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-886">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <span data-ttu-id="f6dd2-887"><a id="ExecutingSqlQueries"></a>Provádění dotazů SQL</span><span class="sxs-lookup"><span data-stu-id="f6dd2-887"><a id="ExecutingSqlQueries"></a>Executing SQL queries</span></span>
<span data-ttu-id="f6dd2-888">Cosmos DB zveřejňuje prostředky přes rozhraní REST API, kterou lze volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-888">Cosmos DB exposes resources through a REST API that can be called by any language capable of making HTTP/HTTPS requests.</span></span> <span data-ttu-id="f6dd2-889">Cosmos DB dále nabízí programovací knihovny pro několik oblíbených jazyků, jako je rozhraní .NET, Node.js, JavaScript a Python.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-889">Additionally, Cosmos DB offers programming libraries for several popular languages like .NET, Node.js, JavaScript, and Python.</span></span> <span data-ttu-id="f6dd2-890">Hello REST API a hello různé knihovny všechny podporovat, dotazování pomocí SQL.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-890">hello REST API and hello various libraries all support querying through SQL.</span></span> <span data-ttu-id="f6dd2-891">Hello .NET SDK podporuje dotazování kromě tooSQL LINQ.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-891">hello .NET SDK supports LINQ querying in addition tooSQL.</span></span>

<span data-ttu-id="f6dd2-892">Dobrý den, jak následující příklady zobrazují toocreate dotazu a odešlete ji proti Cosmos DB databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-892">hello following examples show how toocreate a query and submit it against a Cosmos DB database account.</span></span>

### <span data-ttu-id="f6dd2-893"><a id="RestAPI"></a>ROZHRANÍ REST API</span><span class="sxs-lookup"><span data-stu-id="f6dd2-893"><a id="RestAPI"></a>REST API</span></span>
<span data-ttu-id="f6dd2-894">Cosmos DB nabízí otevřete RESTful programovací model přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-894">Cosmos DB offers an open RESTful programming model over HTTP.</span></span> <span data-ttu-id="f6dd2-895">Databáze účtů se dá zřídit pomocí předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-895">Database accounts can be provisioned using an Azure subscription.</span></span> <span data-ttu-id="f6dd2-896">model prostředků Cosmos DB Hello obsahuje sadu prostředků v rámci účtu databáze, z nichž každý je adresovatelné logické a stabilní identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-896">hello Cosmos DB resource model consists of a set of resources under a database account, each  of which is addressable using a logical and stable URI.</span></span> <span data-ttu-id="f6dd2-897">Sadu prostředků je odkazované tooas informačního kanálu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-897">A set of resources is referred tooas a feed in this document.</span></span> <span data-ttu-id="f6dd2-898">Databázový účet se skládá ze sady databází, každá obsahuje několik kolekcí, každý z které naopak obsahovat dokumenty, funkce UDF a další typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-898">A database account consists of a set of databases, each containing multiple collections, each of which in-turn contain documents, UDFs, and other resource types.</span></span>

<span data-ttu-id="f6dd2-899">model základní interakce Hello pomocí těchto prostředků je prostřednictvím příkazy hello HTTP GET, PUT, POST a odstranit pomocí jejich standardní překladu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-899">hello basic interaction model with these resources is through hello HTTP verbs GET, PUT, POST, and DELETE with their standard interpretation.</span></span> <span data-ttu-id="f6dd2-900">příkaz POST Hello se používá pro vytvoření nového prostředku, pro spuštění uložené procedury nebo pro zadání dotazu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-900">hello POST verb is used for creation of a new resource, for executing a stored procedure or for issuing a Cosmos DB query.</span></span> <span data-ttu-id="f6dd2-901">Dotazy jsou vždy jen pro čtení operací s žádné vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-901">Queries are always read-only operations with no side-effects.</span></span>

<span data-ttu-id="f6dd2-902">Hello následující příklady ukazují POST pro rozhraní API DocumentDB dotaz směřovaný na kolekce obsahující hello dva ukázkové dokumenty, že jsme si přečetli dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-902">hello following examples show a POST for a DocumentDB API query made against a collection containing hello two sample documents we've reviewed so far.</span></span> <span data-ttu-id="f6dd2-903">Hello dotazu je jednoduchý filtr u hello JSON název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-903">hello query has a simple filter on hello JSON name property.</span></span> <span data-ttu-id="f6dd2-904">Všimněte si použití hello hello `x-ms-documentdb-isquery` a Content-Type: `application/query+json` toodenote hlavičky, která hello operaci je dotaz.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-904">Note hello use of hello `x-ms-documentdb-isquery` and Content-Type: `application/query+json` headers toodenote that hello operation is a query.</span></span>

<span data-ttu-id="f6dd2-905">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-905">**Request**</span></span>

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


<span data-ttu-id="f6dd2-906">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-906">**Results**</span></span>

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


<span data-ttu-id="f6dd2-907">Hello druhý příklad ukazuje komplexnější dotaz, který vrátí více výsledků z hello spojení.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-907">hello second example shows a more complex query that returns multiple results from hello join.</span></span>

<span data-ttu-id="f6dd2-908">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-908">**Request**</span></span>

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


<span data-ttu-id="f6dd2-909">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f6dd2-909">**Results**</span></span>

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


<span data-ttu-id="f6dd2-910">Pokud výsledku dotazu se nemůže vejít do jedné stránky s výsledky, pak hello REST API vrátí token pokračování prostřednictvím hello `x-ms-continuation-token` hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-910">If a query's results cannot fit within a single page of results, then hello REST API returns a continuation token through hello `x-ms-continuation-token` response header.</span></span> <span data-ttu-id="f6dd2-911">Klienti mohou stránkování výsledků zahrnutím hello hlavičky v následných výsledky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-911">Clients can paginate results by including hello header in subsequent results.</span></span> <span data-ttu-id="f6dd2-912">Hello počet výsledků na stránce lze také řídit prostřednictvím hello `x-ms-max-item-count` číslo záhlaví.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-912">hello number of results per page can also be controlled through hello `x-ms-max-item-count` number header.</span></span> <span data-ttu-id="f6dd2-913">Pokud zadaný dotaz hello má agregační funkci jako `COUNT`, pak stránku hello dotaz může vrátit hodnotu částečně agregované přes hello stránky s výsledky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-913">If hello specified query has an aggregation function like `COUNT`, then hello query page may return a partially aggregated value over hello page of results.</span></span> <span data-ttu-id="f6dd2-914">Hello klientům musí provést přes tyto výsledky tooproduce hello konečných výsledků, například agregace druhé úrovně, součet přes hello počty vrátil hello jednotlivé stránky tooreturn hello celkový počet.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-914">hello clients must perform a second-level aggregation over these results tooproduce hello final results, for example, sum over hello counts returned in hello individual pages tooreturn hello total count.</span></span>

<span data-ttu-id="f6dd2-915">zásady konzistence dat hello toomanage pro dotazy, použijte hello `x-ms-consistency-level` záhlaví jako všechny požadavky REST API.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-915">toomanage hello data consistency policy for queries, use hello `x-ms-consistency-level` header like all REST API requests.</span></span> <span data-ttu-id="f6dd2-916">Konzistence typu relace, je požadovaná tooalso echo hello nejnovější `x-ms-session-token` hello dotazu požadavek obsahoval hlavičku souboru Cookie.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-916">For session consistency, it is required tooalso echo hello latest `x-ms-session-token` Cookie header in hello query request.</span></span> <span data-ttu-id="f6dd2-917">Hello zásady indexování dotazované kolekce můžete také ovlivnit hello konzistence výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-917">hello queried collection's indexing policy can also influence hello consistency of query results.</span></span> <span data-ttu-id="f6dd2-918">Hello výchozí nastavení zásady indexování, pro kolekce hello index je vždy aktuální pomocí obsahu dokumentu hello a dotazů, výsledky odpovídaly hello konzistence zvolené pro data.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-918">With hello default indexing policy settings, for collections hello index is always current with hello document contents and query results match hello consistency chosen for data.</span></span> <span data-ttu-id="f6dd2-919">Pokud hello indexování zásad volný tooLazy, dotazy mohou vracet zastaralé výsledky.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-919">If hello indexing policy is relaxed tooLazy, then queries can return stale results.</span></span> <span data-ttu-id="f6dd2-920">Další informace najdete v tématu [úrovně konzistence databáze Azure Cosmos][consistency-levels].</span><span class="sxs-lookup"><span data-stu-id="f6dd2-920">For more information, see [Azure Cosmos DB Consistency Levels][consistency-levels].</span></span>

<span data-ttu-id="f6dd2-921">Pokud zásady indexování hello nakonfigurované na kolekci hello nepodporuje zadaný dotaz hello, vrátí server Azure Cosmos DB hello 400 "Chybný požadavek".</span><span class="sxs-lookup"><span data-stu-id="f6dd2-921">If hello configured indexing policy on hello collection cannot support hello specified query, hello Azure Cosmos DB server returns 400 "Bad Request".</span></span> <span data-ttu-id="f6dd2-922">Se vrátí pro dotazy na rozsah pro cesty, které jsou nakonfigurované pro vyhledávání hodnoty hash (rovnosti) a cesty explicitně vyloučená z indexování.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-922">This is returned for range queries against paths configured for hash (equality) lookups, and for paths explicitly excluded from indexing.</span></span> <span data-ttu-id="f6dd2-923">Hello `x-ms-documentdb-query-enable-scan` záhlaví může být zadaný tooallow hello dotazu tooperform kontrolu, když indexu není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-923">hello `x-ms-documentdb-query-enable-scan` header can be specified tooallow hello query tooperform a scan when an index is not available.</span></span>

<span data-ttu-id="f6dd2-924">Podrobné metriky můžete získat na spuštění dotazu nastavením `x-ms-documentdb-populatequerymetrics` záhlaví příliš`True`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-924">You can get detailed metrics on query execution by setting `x-ms-documentdb-populatequerymetrics` header too`True`.</span></span> <span data-ttu-id="f6dd2-925">Další informace najdete v tématu [metriky dotazů SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="f6dd2-925">For more information, see [SQL query metrics for Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span></span>

### <span data-ttu-id="f6dd2-926"><a id="DotNetSdk"></a>SADA SDK JAZYKA C# (.NET)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span></span>
<span data-ttu-id="f6dd2-927">Hello .NET SDK podporuje LINQ i SQL dotazování.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-927">hello .NET SDK supports both LINQ and SQL querying.</span></span> <span data-ttu-id="f6dd2-928">Hello následující příklad ukazuje, jak tooperform hello jednoduchého filtru dotazu zavedená dříve v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-928">hello following example shows how tooperform hello simple filter query introduced earlier in this document.</span></span>

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


<span data-ttu-id="f6dd2-929">Tato ukázka porovná dvě vlastnosti rovnosti v rámci každého dokumentu a používá anonymní projekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-929">This sample compares two properties for equality within each document and uses anonymous projections.</span></span> 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


<span data-ttu-id="f6dd2-930">Hello další příklad ukazuje spojení, vyjádřit pomocí LINQ označit více.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-930">hello next sample shows joins, expressed through LINQ SelectMany.</span></span>

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



<span data-ttu-id="f6dd2-931">klient .NET Hello automaticky iteruje všechny stránky hello výsledků dotazu v blocích foreach hello jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-931">hello .NET client automatically iterates through all hello pages of query results in hello foreach blocks as shown above.</span></span> <span data-ttu-id="f6dd2-932">Možnosti zavedené v hello REST API části jsou také k dispozici v dotazu Hello hello .NET SDK pomocí hello `FeedOptions` a `FeedResponse` třídy v hello CreateDocumentQuery metoda.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-932">hello query options introduced in hello REST API section are also available in hello .NET SDK using hello `FeedOptions` and `FeedResponse` classes in hello CreateDocumentQuery method.</span></span> <span data-ttu-id="f6dd2-933">Dobrý den, kolik stránek se dá řídit pomocí hello `MaxItemCount` nastavení.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-933">hello number of pages can be controlled using hello `MaxItemCount` setting.</span></span> 

<span data-ttu-id="f6dd2-934">Můžete také explicitně řídit stránkování tak, že vytvoříte `IDocumentQueryable` pomocí hello `IQueryable` objekt, pak načtením` ResponseContinuationToken` hodnot a jejich předání zpět jako `RequestContinuationToken` v `FeedOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-934">You can also explicitly control paging by creating `IDocumentQueryable` using hello `IQueryable` object, then by reading the` ResponseContinuationToken` values and passing them back as `RequestContinuationToken` in `FeedOptions`.</span></span> <span data-ttu-id="f6dd2-935">`EnableScanInQuery`může být sada tooenable kontrol, když hello dotazu nemůže být nepodporuje hello nakonfigurované zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-935">`EnableScanInQuery` can be set tooenable scans when hello query cannot be supported by hello configured indexing policy.</span></span> <span data-ttu-id="f6dd2-936">Pro dělené kolekce, můžete použít `PartitionKey` toorun hello dotaz jednoho oddílu (i když Cosmos DB může automaticky extrahovat to z textu hello dotazu), a `EnableCrossPartitionQuery` toorun dotazy, které může být nutné toobe spouštění více oddílů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-936">For partitioned collections, you can use `PartitionKey` toorun hello query against a single partition (though Cosmos DB can automatically extract this from hello query text), and `EnableCrossPartitionQuery` toorun queries that may need toobe run against multiple partitions.</span></span> 

<span data-ttu-id="f6dd2-937">Odkazovat příliš[Azure Cosmos DB .NET ukázky](https://github.com/Azure/azure-documentdb-net) pro další ukázky obsahující dotazy.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-937">Refer too[Azure Cosmos DB .NET samples](https://github.com/Azure/azure-documentdb-net) for more samples containing queries.</span></span> 

### <span data-ttu-id="f6dd2-938"><a id="JavaScriptServerSideApi"></a>Rozhraní API jazyka JavaScript na straně serveru</span><span class="sxs-lookup"><span data-stu-id="f6dd2-938"><a id="JavaScriptServerSideApi"></a>JavaScript server-side API</span></span>
<span data-ttu-id="f6dd2-939">Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na kolekce hello pomocí uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-939">Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections using stored procedures and triggers.</span></span> <span data-ttu-id="f6dd2-940">logiky Javascriptové Hello registrované na úrovni kolekce potom můžou provádět databázové operace na operace hello na dokumentech hello hello zadané kolekce.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-940">hello JavaScript logic registered at a collection level can then issue database operations on hello operations on hello documents of hello given collection.</span></span> <span data-ttu-id="f6dd2-941">Tyto operace jsou zabalená vedlejším ACID transakcí.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-941">These operations are wrapped in ambient ACID transactions.</span></span>

<span data-ttu-id="f6dd2-942">Hello následující příklad ukazuje, jak se dotazuje dokumenty toouse hello dotazu v toomake hello rozhraní API serveru JavaScript z uvnitř uložené procedury a triggery.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-942">hello following example shows how toouse hello queryDocuments in hello JavaScript server API toomake queries from inside stored procedures and triggers.</span></span>

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <span data-ttu-id="f6dd2-943"><a id="References"></a>Odkazy</span><span class="sxs-lookup"><span data-stu-id="f6dd2-943"><a id="References"></a>References</span></span>
1. <span data-ttu-id="f6dd2-944">[Úvod tooAzure Cosmos DB][introduction]</span><span class="sxs-lookup"><span data-stu-id="f6dd2-944">[Introduction tooAzure Cosmos DB][introduction]</span></span>
2. [<span data-ttu-id="f6dd2-945">Azure Cosmos DB SQL specifikace</span><span class="sxs-lookup"><span data-stu-id="f6dd2-945">Azure Cosmos DB SQL specification</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [<span data-ttu-id="f6dd2-946">Ukázek Azure DB Cosmos rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f6dd2-946">Azure Cosmos DB .NET samples</span></span>](https://github.com/Azure/azure-documentdb-net)
4. <span data-ttu-id="f6dd2-947">[Úrovně konzistence databáze Azure Cosmos][consistency-levels]</span><span class="sxs-lookup"><span data-stu-id="f6dd2-947">[Azure Cosmos DB Consistency Levels][consistency-levels]</span></span>
5. <span data-ttu-id="f6dd2-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span></span>
6. <span data-ttu-id="f6dd2-949">JSON [http://json.org/](http://json.org/)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-949">JSON [http://json.org/](http://json.org/)</span></span>
7. <span data-ttu-id="f6dd2-950">Specifikace jazyka JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-950">Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span></span> 
8. <span data-ttu-id="f6dd2-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span></span> 
9. <span data-ttu-id="f6dd2-952">Dotaz na vyhodnocení techniky pro velké databáze [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span><span class="sxs-lookup"><span data-stu-id="f6dd2-952">Query evaluation techniques for large databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span></span>
10. <span data-ttu-id="f6dd2-953">Zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů</span><span class="sxs-lookup"><span data-stu-id="f6dd2-953">Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994</span></span>
11. <span data-ttu-id="f6dd2-954">Logická jednotka, Ooi, Tan, zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-954">Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.</span></span>
12. <span data-ttu-id="f6dd2-955">Olston Kryštof, Robert Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: není tak cizí jazyk pro zpracování dat, SIGMOD 2008.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.</span></span>
13. <span data-ttu-id="f6dd2-956">G.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-956">G.</span></span> <span data-ttu-id="f6dd2-957">Graefe.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-957">Graefe.</span></span> <span data-ttu-id="f6dd2-958">Hello Cascades architektura pro optimalizaci dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-958">hello Cascades framework for query optimization.</span></span> <span data-ttu-id="f6dd2-959">Eng. IEEE dat</span><span class="sxs-lookup"><span data-stu-id="f6dd2-959">IEEE Data Eng.</span></span> <span data-ttu-id="f6dd2-960">Bull., 18(3): 1995.</span><span class="sxs-lookup"><span data-stu-id="f6dd2-960">Bull., 18(3): 1995.</span></span>

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md