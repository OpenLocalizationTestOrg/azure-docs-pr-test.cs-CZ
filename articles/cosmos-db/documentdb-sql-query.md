---
title: "Dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
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
ms.openlocfilehash: 9b2b5668ef0552485a86f63a120b57c4623bfe35
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="4e022-105">Dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="4e022-105">SQL queries for Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="4e022-106">Microsoft Azure Cosmos DB podporuje dotazování dokumentů pomocí jazyka SQL (Structured Query Language) jako dotazovací jazyk JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-106">Microsoft Azure Cosmos DB supports querying documents using SQL (Structured Query Language) as a JSON query language.</span></span> <span data-ttu-id="4e022-107">Cosmos DB je skutečně bez schémat.</span><span class="sxs-lookup"><span data-stu-id="4e022-107">Cosmos DB is truly schema-free.</span></span> <span data-ttu-id="4e022-108">Na základě jeho závazků do datového modelu JSON přímo v rámci databázový stroj poskytuje automatické indexování dokumentů JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="4e022-108">By virtue of its commitment to the JSON data model directly within the database engine, it provides automatic indexing of JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> 

<span data-ttu-id="4e022-109">Při navrhování dotazovacího jazyka pro Cosmos DB, jsme měli dva cíle v paměti:</span><span class="sxs-lookup"><span data-stu-id="4e022-109">While designing the query language for Cosmos DB, we had two goals in mind:</span></span>

* <span data-ttu-id="4e022-110">Místo inventing o nový jazyk dotazů JSON, jsme chtěli podporu SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-110">Instead of inventing a new JSON query language, we wanted to support SQL.</span></span> <span data-ttu-id="4e022-111">SQL je jedním z nejvíce známé a oblíbených jazyků dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-111">SQL is one of the most familiar and popular query languages.</span></span> <span data-ttu-id="4e022-112">SQL databáze cosmos umožňuje formální programovací model o bohaté dotazy prostřednictvím dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-112">Cosmos DB SQL provides a formal programming model for rich queries over JSON documents.</span></span>
* <span data-ttu-id="4e022-113">Jako dokument databáze JSON může provést JavaScript přímo v databázovém stroji jsme chtěli použít model programování v jazyce JavaScript jako základ pro naše dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="4e022-113">As a JSON document database capable of executing JavaScript directly in the database engine, we wanted to use JavaScript's programming model as the foundation for our query language.</span></span> <span data-ttu-id="4e022-114">DocumentDB SQL rozhraní API je integrován do systému typů JavaScript na vyhodnocení výrazu a volání funkce.</span><span class="sxs-lookup"><span data-stu-id="4e022-114">The DocumentDB API SQL is rooted in JavaScript's type system, expression evaluation, and function invocation.</span></span> <span data-ttu-id="4e022-115">Tato naopak poskytuje přirozené programovací model pro projekce relačních, hierarchických navigace mezi dokumenty JSON, vlastní spojení, prostorových dotazů a vyvolání uživatelem definovaných funkcí (UDF) vytvořené zcela v JavaScriptu mezi dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="4e022-115">This in-turn provides a natural programming model for relational projections, hierarchical navigation across JSON documents, self joins, spatial queries, and invocation of user-defined functions (UDFs) written entirely in JavaScript, among other features.</span></span> 

<span data-ttu-id="4e022-116">Věříme, že tyto funkce jsou klíčem k omezení tření mezi aplikací a databáze a jsou zásadní pro produktivita vývojářů.</span><span class="sxs-lookup"><span data-stu-id="4e022-116">We believe that these capabilities are key to reducing the friction between the application and the database and are crucial for developer productivity.</span></span>

<span data-ttu-id="4e022-117">Doporučujeme začít následujícím videem, kde Aravind Ramachandran zobrazí Cosmos DB je dotazování možnosti, a navštívíte naše [Query Playground](http://www.documentdb.com/sql/demo), kde můžete vyzkoušet Cosmos DB a spouštět dotazy SQL pro naše datové sady.</span><span class="sxs-lookup"><span data-stu-id="4e022-117">We recommend getting started by watching the following video, where Aravind Ramachandran shows Cosmos DB's querying capabilities, and by visiting our [Query Playground](http://www.documentdb.com/sql/demo), where you can try out Cosmos DB and run SQL queries against our dataset.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

<span data-ttu-id="4e022-118">Pak se vraťte k tomuto článku, kde Začneme s kurz dotaz SQL, který vás provede některé jednoduché dokumentů JSON a příkazy SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-118">Then, return to this article, where we start with a SQL query tutorial that walks you through some simple JSON documents and SQL commands.</span></span>

## <span data-ttu-id="4e022-119"><a id="GettingStarted"></a>Začínáme s příkazy SQL v databázi systému Cosmos</span><span class="sxs-lookup"><span data-stu-id="4e022-119"><a id="GettingStarted"></a>Getting started with SQL commands in Cosmos DB</span></span>
<span data-ttu-id="4e022-120">SQL databáze Cosmos v práci najdete umožňuje začínat několik jednoduchých dokumentů JSON a provede několik jednoduchých dotazů u ní.</span><span class="sxs-lookup"><span data-stu-id="4e022-120">To see Cosmos DB SQL at work, let's begin with a few simple JSON documents and walk through some simple queries against it.</span></span> <span data-ttu-id="4e022-121">Vezměte v úvahu tyto dva dokumenty JSON o dvou řad.</span><span class="sxs-lookup"><span data-stu-id="4e022-121">Consider these two JSON documents about two families.</span></span> <span data-ttu-id="4e022-122">S Cosmos DB jsme není potřeba explicitně vytvořit žádné schémata nebo sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="4e022-122">With Cosmos DB, we do not need to create any schemas or secondary indices explicitly.</span></span> <span data-ttu-id="4e022-123">Jednoduše musíme vložit dokumenty JSON do kolekce Cosmos DB a následně dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-123">We simply need to insert the JSON documents to a Cosmos DB collection and subsequently query.</span></span> <span data-ttu-id="4e022-124">Tady bychom měli jednoduché JSON dokumentů pro rodinu, rodiče, děti (a jejich mazlíčků), adresu a informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="4e022-124">Here we have a simple JSON document for the Andersen family, the parents, children (and their pets), address, and registration information.</span></span> <span data-ttu-id="4e022-125">Má dokument řetězců, čísel, logické hodnoty, pole a vnořené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e022-125">The document has strings, numbers, Booleans, arrays, and nested properties.</span></span> 

<span data-ttu-id="4e022-126">**Dokument**</span><span class="sxs-lookup"><span data-stu-id="4e022-126">**Document**</span></span>  

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

<span data-ttu-id="4e022-127">Tady je druhý dokument s jedním jemně rozdílem – `givenName` a `familyName` se používají místo `firstName` a `lastName`.</span><span class="sxs-lookup"><span data-stu-id="4e022-127">Here's a second document with one subtle difference – `givenName` and `familyName` are used instead of `firstName` and `lastName`.</span></span>

<span data-ttu-id="4e022-128">**Dokument**</span><span class="sxs-lookup"><span data-stu-id="4e022-128">**Document**</span></span>  

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

<span data-ttu-id="4e022-129">Nyní nyní si vyzkoušíte několik dotazů pro tato data pochopit některé z klíčových aspektů DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e022-129">Now let's try a few queries against this data to understand some of the key aspects of DocumentDB API SQL.</span></span> <span data-ttu-id="4e022-130">Například následující dotaz vrátí dokumenty, kde v poli id odpovídá `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="4e022-130">For example, the following query returns the documents where the id field matches `AndersenFamily`.</span></span> <span data-ttu-id="4e022-131">Vzhledem k tomu, že je `SELECT *`, výstup tohoto dotazu je kompletní dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="4e022-131">Since it's a `SELECT *`, the output of the query is the complete JSON document:</span></span>

<span data-ttu-id="4e022-132">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-132">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-133">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-133">**Results**</span></span>

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


<span data-ttu-id="4e022-134">Teď se podíváme případu, které je třeba přeformátujte výstup JSON v různých obrazce.</span><span class="sxs-lookup"><span data-stu-id="4e022-134">Now consider the case where we need to reformat the JSON output in a different shape.</span></span> <span data-ttu-id="4e022-135">Tento dotaz projekty nový objekt JSON s dvě vybrané pole název a města, když na adresu města má stejný název jako stavu.</span><span class="sxs-lookup"><span data-stu-id="4e022-135">This query projects a new JSON object with two selected fields, Name and City, when the address' city has the same name as the state.</span></span> <span data-ttu-id="4e022-136">V tomto případě "NY, NY" odpovídá.</span><span class="sxs-lookup"><span data-stu-id="4e022-136">In this case, "NY, NY" matches.</span></span>

<span data-ttu-id="4e022-137">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-137">**Query**</span></span>    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

<span data-ttu-id="4e022-138">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-138">**Results**</span></span>

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


<span data-ttu-id="4e022-139">Další dotaz vrátí všechny názvy daným podřízených prvků v dané rodině, jehož id odpovídá `WakefieldFamily` seřazené podle města pobytu.</span><span class="sxs-lookup"><span data-stu-id="4e022-139">The next query returns all the given names of children in the family whose id matches `WakefieldFamily` ordered by the city of residence.</span></span>

<span data-ttu-id="4e022-140">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-140">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

<span data-ttu-id="4e022-141">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-141">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


<span data-ttu-id="4e022-142">Rádi bychom se upozornit na několik pozoruhodné aspektů dotazovací jazyk Cosmos DB provede příklady, které jste viděli, pokud:</span><span class="sxs-lookup"><span data-stu-id="4e022-142">We would like to draw attention to a few noteworthy aspects of the Cosmos DB query language through the examples we've seen so far:</span></span>  

* <span data-ttu-id="4e022-143">Vzhledem k tomu, že DocumentDB SQL rozhraní API funguje na hodnoty JSON, zabývá stromu ve tvaru entity místo řádků a sloupců.</span><span class="sxs-lookup"><span data-stu-id="4e022-143">Since DocumentDB API SQL works on JSON values, it deals with tree shaped entities instead of rows and columns.</span></span> <span data-ttu-id="4e022-144">Proto jazyk umožňuje vztahují na všechny uzly stromu v jakékoli libovolný hloubku jako `Node1.Node2.Node3…..Nodem`, podobně jako relační SQL odkazující na odkaz na dvě části `<table>.<column>`.</span><span class="sxs-lookup"><span data-stu-id="4e022-144">Therefore, the language lets you refer to nodes of the tree at any arbitrary depth, like `Node1.Node2.Node3…..Nodem`, similar to relational SQL referring to the two part reference of `<table>.<column>`.</span></span>   
* <span data-ttu-id="4e022-145">Jazyk SQL pracuje s daty bez schématu.</span><span class="sxs-lookup"><span data-stu-id="4e022-145">The structured query language works with schema-less data.</span></span> <span data-ttu-id="4e022-146">Systém typů proto musí být vázána dynamicky.</span><span class="sxs-lookup"><span data-stu-id="4e022-146">Therefore, the type system needs to be bound dynamically.</span></span> <span data-ttu-id="4e022-147">Stejný výraz může přinést různých typů na různé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="4e022-147">The same expression could yield different types on different documents.</span></span> <span data-ttu-id="4e022-148">Výsledek dotazu není platná hodnota JSON, ale není zaručena bezpečnost pro přístup z pevného schématu.</span><span class="sxs-lookup"><span data-stu-id="4e022-148">The result of a query is a valid JSON value, but is not guaranteed to be of a fixed schema.</span></span>  
* <span data-ttu-id="4e022-149">Cosmos databáze podporuje pouze striktní dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-149">Cosmos DB only supports strict JSON documents.</span></span> <span data-ttu-id="4e022-150">To znamená, že systém typů a výrazy jsou omezeny na pracují jenom s typy JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-150">This means the type system and expressions are restricted to deal only with JSON types.</span></span> <span data-ttu-id="4e022-151">Odkazovat [JSON specifikace](http://www.json.org/) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4e022-151">Refer to the [JSON specification](http://www.json.org/) for more details.</span></span>  
* <span data-ttu-id="4e022-152">Cosmos DB kolekce je kontejner dokumentů JSON bez schémat.</span><span class="sxs-lookup"><span data-stu-id="4e022-152">A Cosmos DB collection is a schema-free container of JSON documents.</span></span> <span data-ttu-id="4e022-153">Vztahy v datových entit v rámci a na dokumentech v kolekci jsou implicitně zaznamenat členství ve skupině a ne primárního a cizího klíče relace.</span><span class="sxs-lookup"><span data-stu-id="4e022-153">The relations in data entities within and across documents in a collection are implicitly captured by containment and not by primary key and foreign key relations.</span></span> <span data-ttu-id="4e022-154">Toto je důležitým aspektem vhodné odkazující na základě spojení intra-document probírat později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4e022-154">This is an important aspect worth pointing out in light of the intra-document joins discussed later in this article.</span></span>

## <span data-ttu-id="4e022-155"><a id="Indexing"></a>Indexování cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4e022-155"><a id="Indexing"></a> Cosmos DB indexing</span></span>
<span data-ttu-id="4e022-156">Než se nám získat do syntaxi DocumentDB SQL rozhraní API, je vhodné využít indexování návrhu v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-156">Before we get into the DocumentDB API SQL syntax, it is worth exploring the indexing design in Cosmos DB.</span></span> 

<span data-ttu-id="4e022-157">Účelem indexy databáze je poskytovat dotazy v různých formách a tvarů s spotřeby minimální prostředků (např. využití procesoru a vstup/výstup) současně poskytují dobrý prostupnosti a nízké latence.</span><span class="sxs-lookup"><span data-stu-id="4e022-157">The purpose of database indexes is to serve queries in their various forms and shapes with minimum resource consumption (like CPU and input/output) while providing good throughput and low latency.</span></span> <span data-ttu-id="4e022-158">Volba správného indexu pro dotazování databáze často vyžaduje mnohem plánování a experimentování.</span><span class="sxs-lookup"><span data-stu-id="4e022-158">Often, the choice of the right index for querying a database requires much planning and experimentation.</span></span> <span data-ttu-id="4e022-159">Tento přístup představuje výzvu pro bez schématu databáze, kde data neodpovídají striktní schéma a zpracovaní rychle.</span><span class="sxs-lookup"><span data-stu-id="4e022-159">This approach poses a challenge for schema-less databases where the data doesn’t conform to a strict schema and evolves rapidly.</span></span> 

<span data-ttu-id="4e022-160">Proto když jsme navržený subsystém indexování Cosmos DB, nastaví sledovat tyto cíle:</span><span class="sxs-lookup"><span data-stu-id="4e022-160">Therefore, when we designed the Cosmos DB indexing subsystem, we set the following goals:</span></span>

* <span data-ttu-id="4e022-161">Indexování dokumentů bez nutnosti schématu: subsystém indexování nevyžaduje žádné informace o schématu ani vytvořit žádný odhad o schéma dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4e022-161">Index documents without requiring schema: The indexing subsystem does not require any schema information or make any assumptions about schema of the documents.</span></span> 
* <span data-ttu-id="4e022-162">Podpora pro efektivní, bohaté hierarchické a relační dotazy: index podporuje dotazovací jazyk Cosmos DB efektivně, včetně podpory pro hierarchické a relační projekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-162">Support for efficient, rich hierarchical, and relational queries: The index supports the Cosmos DB query language efficiently, including support for hierarchical and relational projections.</span></span>
* <span data-ttu-id="4e022-163">Podpora pro konzistentní dotazy in face of svazek dlouhodobě zápisů: pro zápisu vysokou propustnost úlohy s konzistentní dotazy, aktualizace indexu postupně, efektivně a online při krátkodobém dlouhodobě svazku zápisů.</span><span class="sxs-lookup"><span data-stu-id="4e022-163">Support for consistent queries in face of a sustained volume of writes: For high write throughput workloads with consistent queries, the index is updated incrementally, efficiently, and online in the face of a sustained volume of writes.</span></span> <span data-ttu-id="4e022-164">Aktualizace konzistentní index je zásadní význam pro poskytovat dotazy na úrovni konzistence, ve kterém uživatel nakonfigurovali službu dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-164">The consistent index update is crucial to serve the queries at the consistency level in which the user configured the document service.</span></span>
* <span data-ttu-id="4e022-165">Podpora pro více klientů: zadána modelu založené na vyhrazené pro řízení prostředků mezi klienty v rámci rozpočtu systémových prostředků (procesoru, paměti a vstupně-výstupních operací za sekundu) přidělený na repliky jsou provedeny aktualizace indexu.</span><span class="sxs-lookup"><span data-stu-id="4e022-165">Support for multi-tenancy: Given the reservation-based model for resource governance across tenants, index updates are performed within the budget of system resources (CPU, memory, and input/output operations per second) allocated per replica.</span></span> 
* <span data-ttu-id="4e022-166">Efektivitu úložiště: pro finanční efektivita režijní náklady na úložiště na disku indexu je ohraničené a předvídatelné.</span><span class="sxs-lookup"><span data-stu-id="4e022-166">Storage efficiency: For cost effectiveness, the on-disk storage overhead of the index is bounded and predictable.</span></span> <span data-ttu-id="4e022-167">To je velmi důležitý, protože Cosmos DB umožňuje vývojáři aby náklady na základě kompromisy mezi režijní náklady na indexů ve vztahu k dotazu výkon.</span><span class="sxs-lookup"><span data-stu-id="4e022-167">This is crucial because Cosmos DB allows the developer to make cost-based tradeoffs between index overhead in relation to the query performance.</span></span>  

<span data-ttu-id="4e022-168">Odkazovat [Azure Cosmos DB – ukázky](https://github.com/Azure/azure-documentdb-net) na webu MSDN ukázek znázorňující postup konfigurace zásady indexování pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="4e022-168">Refer to the [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) on MSDN for samples showing how to configure the indexing policy for a collection.</span></span> <span data-ttu-id="4e022-169">Nyní Pojďme na podrobné informace o syntaxi Azure Cosmos DB SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-169">Let’s now get into the details of the Azure Cosmos DB SQL syntax.</span></span>

## <span data-ttu-id="4e022-170"><a id="Basics"></a>Základní informace o příkazu jazyka Azure Cosmos DB SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-170"><a id="Basics"></a>Basics of an Azure Cosmos DB SQL query</span></span>
<span data-ttu-id="4e022-171">Každý dotaz sestává z klauzule SELECT a volitelné FROM a klauzule WHERE za standardy ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-171">Every query consists of a SELECT clause and optional FROM and WHERE clauses per ANSI-SQL standards.</span></span> <span data-ttu-id="4e022-172">Pro každý dotaz, obvykle je výčet zdroji v klauzuli FROM.</span><span class="sxs-lookup"><span data-stu-id="4e022-172">Typically, for each query, the source in the FROM clause is enumerated.</span></span> <span data-ttu-id="4e022-173">Filtru v klauzuli WHERE se pak použije ve zdroji k načtení podmnožinu dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-173">Then the filter in the WHERE clause is applied on the source to retrieve a subset of JSON documents.</span></span> <span data-ttu-id="4e022-174">Klauzule SELECT se nakonec slouží k plánování požadovaný JSON hodnot v seznamu select.</span><span class="sxs-lookup"><span data-stu-id="4e022-174">Finally, the SELECT clause is used to project the requested JSON values in the select list.</span></span>

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <span data-ttu-id="4e022-175"><a id="FromClause"></a>FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="4e022-175"><a id="FromClause"></a>FROM clause</span></span>
<span data-ttu-id="4e022-176">`FROM <from_specification>` Klauzule je nepovinný, pokud je zdroj filtrovat nebo projekci později v dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-176">The `FROM <from_specification>` clause is optional unless the source is filtered or projected later in the query.</span></span> <span data-ttu-id="4e022-177">Účelem této klauzule je zadat zdroj dat, na kterém musí fungovat dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-177">The purpose of this clause is to specify the data source upon which the query must operate.</span></span> <span data-ttu-id="4e022-178">Běžně celé kolekce je zdrojem, ale jeden místo toho zadat podmnožinu kolekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-178">Commonly the whole collection is the source, but one can specify a subset of the collection instead.</span></span> 

<span data-ttu-id="4e022-179">Dotaz jako `SELECT * FROM Families` označuje, že je celou kolekci rodiny zdroji, za které se vytvořit výčet.</span><span class="sxs-lookup"><span data-stu-id="4e022-179">A query like `SELECT * FROM Families` indicates that the entire Families collection is the source over which to enumerate.</span></span> <span data-ttu-id="4e022-180">Identifikátor speciální KOŘENOVÉ slouží k představují kolekci nepoužívejte název kolekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-180">A special identifier ROOT can be used to represent the collection instead of using the collection name.</span></span> <span data-ttu-id="4e022-181">Následující seznam obsahuje pravidla, které vynucuje na jeden dotaz:</span><span class="sxs-lookup"><span data-stu-id="4e022-181">The following list contains the rules that are enforced per query:</span></span>

* <span data-ttu-id="4e022-182">Kolekce je to možné, jako například `SELECT f.id FROM Families AS f` nebo jednoduše `SELECT f.id FROM Families f`.</span><span class="sxs-lookup"><span data-stu-id="4e022-182">The collection can be aliased, such as `SELECT f.id FROM Families AS f` or simply `SELECT f.id FROM Families f`.</span></span> <span data-ttu-id="4e022-183">Zde `f` je ekvivalentem `Families`.</span><span class="sxs-lookup"><span data-stu-id="4e022-183">Here `f` is the equivalent of `Families`.</span></span> <span data-ttu-id="4e022-184">`AS`optional – klíčové slovo alias je identifikátor.</span><span class="sxs-lookup"><span data-stu-id="4e022-184">`AS` is an optional keyword to alias the identifier.</span></span>
* <span data-ttu-id="4e022-185">Jednou alias, nemůže být vázán původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="4e022-185">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="4e022-186">Například `SELECT Families.id FROM Families f` je syntakticky neplatný, protože už nelze přeložit identifikátor "Rodiny".</span><span class="sxs-lookup"><span data-stu-id="4e022-186">For example, `SELECT Families.id FROM Families f` is syntactically invalid since the identifier "Families" cannot be resolved anymore.</span></span>
* <span data-ttu-id="4e022-187">Všechny vlastnosti, které je potřeba na něj odkazovat musí být plně kvalifikovaný.</span><span class="sxs-lookup"><span data-stu-id="4e022-187">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="4e022-188">Chybí dodržování striktní schématu tato velikost je vyžadována předejdete žádné nejednoznačný vazby.</span><span class="sxs-lookup"><span data-stu-id="4e022-188">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="4e022-189">Proto `SELECT id FROM Families f` je syntakticky neplatný, protože vlastnost `id` není vázán.</span><span class="sxs-lookup"><span data-stu-id="4e022-189">Therefore, `SELECT id FROM Families f` is syntactically invalid since the property `id` is not bound.</span></span>

### <a name="subdocuments"></a><span data-ttu-id="4e022-190">Vnořené dokumenty</span><span class="sxs-lookup"><span data-stu-id="4e022-190">Subdocuments</span></span>
<span data-ttu-id="4e022-191">Zdroj může být také omezené menší podmnožinu.</span><span class="sxs-lookup"><span data-stu-id="4e022-191">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="4e022-192">Například k vytváření výčtu pouze podstrom v každém dokumentu, subroot může pak mohou stát zdroje, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-192">For instance, to enumerating only a subtree in each document, the subroot could then become the source, as shown in the following example:</span></span>

<span data-ttu-id="4e022-193">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-193">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="4e022-194">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-194">**Results**</span></span>  

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

<span data-ttu-id="4e022-195">Při výše uvedeném příkladu pole jako zdroj, objekt může také použít jako zdroj, který je co je znázorněno v následujícím příkladu: žádné platná hodnota JSON (nedefinovaná), můžete najít ve zdroji je považován za pro zahrnutí do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-195">While the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example: Any valid JSON value (not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="4e022-196">Pokud nemáte některé rodiny `address.state` hodnotu, jsou vyloučeny ve výsledku dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-196">If some families don’t have an `address.state` value, they are excluded in the query result.</span></span>

<span data-ttu-id="4e022-197">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-197">**Query**</span></span>

    SELECT * 
    FROM Families.address.state

<span data-ttu-id="4e022-198">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-198">**Results**</span></span>

    [
      "WA", 
      "NY"
    ]


## <span data-ttu-id="4e022-199"><a id="WhereClause"></a>Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="4e022-199"><a id="WhereClause"></a>WHERE clause</span></span>
<span data-ttu-id="4e022-200">Klauzule WHERE (**`WHERE <filter_condition>`**) je volitelný.</span><span class="sxs-lookup"><span data-stu-id="4e022-200">The WHERE clause (**`WHERE <filter_condition>`**) is optional.</span></span> <span data-ttu-id="4e022-201">Určuje, že podmínku (podmínky), které dokumenty JSON poskytuje zdroj musí splňovat, aby byla součástí výsledek.</span><span class="sxs-lookup"><span data-stu-id="4e022-201">It specifies the condition(s) that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="4e022-202">Dokumentu JSON se musí vyhodnotit na hodnotu true, aby byla považována za pro výsledek k zadaným podmínkám.</span><span class="sxs-lookup"><span data-stu-id="4e022-202">Any JSON document must evaluate the specified conditions to "true" to be considered for the result.</span></span> <span data-ttu-id="4e022-203">Klauzule WHERE se používá vrstvou index aby bylo možné zjistit absolutní nejmenší podmnožinu dokumentů zdroje, které můžou být součástí výsledek.</span><span class="sxs-lookup"><span data-stu-id="4e022-203">The WHERE clause is used by the index layer in order to determine the absolute smallest subset of source documents that can be part of the result.</span></span> 

<span data-ttu-id="4e022-204">Následující dotaz požadavků dokumentů, které obsahují název vlastnosti, jehož hodnota je `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="4e022-204">The following query requests documents that contain a name property whose value is `AndersenFamily`.</span></span> <span data-ttu-id="4e022-205">Jiného dokumentu, který nemá název vlastnosti, nebo kde hodnota neodpovídá `AndersenFamily` je vyloučen.</span><span class="sxs-lookup"><span data-stu-id="4e022-205">Any other document that does not have a name property, or where the value does not match `AndersenFamily` is excluded.</span></span> 

<span data-ttu-id="4e022-206">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-206">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-207">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-207">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


<span data-ttu-id="4e022-208">Předchozí příklad ukázal dotazu jednoduché rovnosti.</span><span class="sxs-lookup"><span data-stu-id="4e022-208">The previous example showed a simple equality query.</span></span> <span data-ttu-id="4e022-209">DocumentDB SQL rozhraní API také podporuje celou řadu skalární výrazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-209">DocumentDB API SQL also supports a variety of scalar expressions.</span></span> <span data-ttu-id="4e022-210">Nejčastěji používané jsou výrazy binární a unární.</span><span class="sxs-lookup"><span data-stu-id="4e022-210">The most commonly used are binary and unary expressions.</span></span> <span data-ttu-id="4e022-211">Vlastnost odkazy z objektu JSON zdroje jsou také platné výrazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-211">Property references from the source JSON object are also valid expressions.</span></span> 

<span data-ttu-id="4e022-212">Následující binární operátory jsou aktuálně podporovány a lze použít v dotazech, jak je znázorněno v následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="4e022-212">The following binary operators are currently supported and can be used in queries as shown in the following examples:</span></span>  

<table>
<tr>
<td><span data-ttu-id="4e022-213">Aritmetické operace</span><span class="sxs-lookup"><span data-stu-id="4e022-213">Arithmetic</span></span></td>    
<td><span data-ttu-id="4e022-214">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="4e022-214">+,-,*,/,%</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4e022-215">Bitový</span><span class="sxs-lookup"><span data-stu-id="4e022-215">Bitwise</span></span></td>    
<td><span data-ttu-id="4e022-216">|, &, ^, <<>>,, >>> (nula výplně posunutí doprava)</span><span class="sxs-lookup"><span data-stu-id="4e022-216">|, &, ^, <<, >>, >>> (zero-fill right shift)</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4e022-217">Logické</span><span class="sxs-lookup"><span data-stu-id="4e022-217">Logical</span></span></td>
<td><span data-ttu-id="4e022-218">A, NEBO NE</span><span class="sxs-lookup"><span data-stu-id="4e022-218">AND, OR, NOT</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4e022-219">Porovnání</span><span class="sxs-lookup"><span data-stu-id="4e022-219">Comparison</span></span></td>    
<td><span data-ttu-id="4e022-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span><span class="sxs-lookup"><span data-stu-id="4e022-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4e022-221">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e022-221">String</span></span></td>    
<td><span data-ttu-id="4e022-222">|| (zřetězení)</span><span class="sxs-lookup"><span data-stu-id="4e022-222">|| (concatenate)</span></span></td>
</tr>
</table>  


<span data-ttu-id="4e022-223">Podívejme se na některé dotazy pomocí binární operátory.</span><span class="sxs-lookup"><span data-stu-id="4e022-223">Let’s take a look at some queries using binary operators.</span></span>

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


<span data-ttu-id="4e022-224">Unární operátory +,-, ~ není jsou podporovány také a dá se použít uvnitř dotazy, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-224">The unary operators +,-, ~ and NOT are also supported, and can be used inside queries as shown in the following example:</span></span>

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



<span data-ttu-id="4e022-225">Kromě binární a unární operátory mohou také vlastnost odkazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-225">In addition to binary and unary operators, property references are also allowed.</span></span> <span data-ttu-id="4e022-226">Například `SELECT * FROM Families f WHERE f.isRegistered` vrátí dokumentu JSON obsahující vlastnost `isRegistered` kde hodnotu vlastnosti rovná JSON `true` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-226">For example, `SELECT * FROM Families f WHERE f.isRegistered` returns the JSON document containing the property `isRegistered` where the property's value is equal to the JSON `true` value.</span></span> <span data-ttu-id="4e022-227">Všechny ostatní hodnoty (false, hodnotu null a nedefinovaná, `<number>`, `<string>`, `<object>`, `<array>`atd) vede k zdrojový dokument k vyloučení z výsledku.</span><span class="sxs-lookup"><span data-stu-id="4e022-227">Any other values (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) leads to the source document being excluded from the result.</span></span> 

### <a name="equality-and-comparison-operators"></a><span data-ttu-id="4e022-228">Operátory rovnosti a porovnání</span><span class="sxs-lookup"><span data-stu-id="4e022-228">Equality and comparison operators</span></span>
<span data-ttu-id="4e022-229">Následující tabulka uvádí výsledek porovnání rovnosti v DocumentDB API SQL mezi všechny dva typy JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-229">The following table shows the result of equality comparisons in DocumentDB API SQL between any two JSON types.</span></span>

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-230">
            <strong>OP</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-230">
            <strong>Op</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-231">
            <strong>Nedefinovaná</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-231">
            <strong>Undefined</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-232">
            <strong>Hodnotu Null</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-232">
            <strong>Null</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-233">
            <strong>Logická hodnota</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-233">
            <strong>Boolean</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-234">
            <strong>Číslo</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-234">
            <strong>Number</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-235">
            <strong>Řetězec</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-235">
            <strong>String</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-236">
            <strong>Objekt</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-236">
            <strong>Object</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="4e022-237">
            <strong>Pole</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-237">
            <strong>Array</strong>
         </span></span></td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-238">
            <strong>Nedefinovaná<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-238">
            <strong>Undefined<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-239">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-239">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-240">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-240">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-241">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-241">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-242">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-242">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-243">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-243">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-244">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-244">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-245">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-245">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-246">
            <strong>Hodnotu Null<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-246">
            <strong>Null<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-247">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-247">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-248">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-248">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-249">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-249">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-250">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-250">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-251">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-251">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-252">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-252">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-253">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-253">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-254">
            <strong>Logická hodnota<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-254">
            <strong>Boolean<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-255">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-255">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-256">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-256">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-257">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-257">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-258">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-258">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-259">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-259">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-260">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-260">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-261">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-261">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-262">
            <strong>Číslo<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-262">
            <strong>Number<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-263">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-263">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-264">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-264">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-265">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-265">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-266">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-266">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-267">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-267">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-268">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-268">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-269">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-269">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-270">
            <strong>Řetězec<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-270">
            <strong>String<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-271">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-271">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-272">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-272">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-273">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-273">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-274">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-274">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-275">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-275">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-276">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-276">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-277">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-277">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-278">
            <strong>Objekt<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-278">
            <strong>Object<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-279">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-279">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-280">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-280">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-281">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-281">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-282">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-282">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-283">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-283">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-284">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-284">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-285">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-285">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="4e022-286">
            <strong>Pole<strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-286">
            <strong>Array<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="4e022-287">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-287">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-288">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-288">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-289">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-289">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-290">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-290">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-291">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-291">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="4e022-292">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-292">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="4e022-293">
            <strong>OK</strong>
         </span><span class="sxs-lookup"><span data-stu-id="4e022-293">
            <strong>OK</strong>
         </span></span></td>
      </tr>
   </tbody>
</table>

<span data-ttu-id="4e022-294">Pro jiné operátory porovnání jako >, > =,! =, < a < =, následující pravidla platí:</span><span class="sxs-lookup"><span data-stu-id="4e022-294">For other comparison operators such as >, >=, !=, < and <=, the following rules apply:</span></span>   

* <span data-ttu-id="4e022-295">Výsledkem porovnání mezi typy Undefined.</span><span class="sxs-lookup"><span data-stu-id="4e022-295">Comparison across types results in Undefined.</span></span>
* <span data-ttu-id="4e022-296">Porovnání mezi dvěma objekty nebo dvě maticových má za následek Undefined.</span><span class="sxs-lookup"><span data-stu-id="4e022-296">Comparison between two objects or two arrays results in Undefined.</span></span>   

<span data-ttu-id="4e022-297">Pokud je výsledek skalární výraz, který ve filtru není definována, odpovídající dokument není zahrnuta do výsledek, protože Undefined není logicky rovnat "true".</span><span class="sxs-lookup"><span data-stu-id="4e022-297">If the result of the scalar expression in the filter is Undefined, the corresponding document would not be included in the result, since Undefined doesn't logically equate to "true".</span></span>

### <a name="between-keyword"></a><span data-ttu-id="4e022-298">MEZI klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="4e022-298">BETWEEN keyword</span></span>
<span data-ttu-id="4e022-299">Můžete taky – klíčové slovo BETWEEN pro dotazy na rozsah hodnot jako v ANSI SQL express.</span><span class="sxs-lookup"><span data-stu-id="4e022-299">You can also use the BETWEEN keyword to express queries against ranges of values like in ANSI SQL.</span></span> <span data-ttu-id="4e022-300">MEZI můžete použít u řetězců nebo čísla.</span><span class="sxs-lookup"><span data-stu-id="4e022-300">BETWEEN can be used against strings or numbers.</span></span>

<span data-ttu-id="4e022-301">Například tento dotaz vrací všechny rodiny dokumenty, ve kterých je prvním podřízeným objektem úrovni mezi 1-5 (obě včetně).</span><span class="sxs-lookup"><span data-stu-id="4e022-301">For example, this query returns all family documents in which the first child's grade is between 1-5 (both inclusive).</span></span> 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

<span data-ttu-id="4e022-302">Na rozdíl od v ANSI SQL, můžete taky klauzuli BETWEEN v klauzuli FROM jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="4e022-302">Unlike in ANSI-SQL, you can also use the BETWEEN clause in the FROM clause like in the following example.</span></span>

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

<span data-ttu-id="4e022-303">Pro kratší časy spuštění dotazu mějte na paměti k vytvoření zásady indexování, která používá typ indexu rozsah proti jakékoli číselné vlastnosti nebo cesty, které jsou filtrovány v klauzuli BETWEEN.</span><span class="sxs-lookup"><span data-stu-id="4e022-303">For faster query execution times, remember to create an indexing policy that uses a range index type against any numeric properties/paths that are filtered in the BETWEEN clause.</span></span> 

<span data-ttu-id="4e022-304">Hlavní rozdíl mezi použitím BETWEEN v DocumentDB rozhraní API a ANSI SQL je, že můžete express rozsah dotazy na vlastnosti smíšený typů – například můžete mít "základní" být číslo (5) v některých dokumentů a řetězce v jiné ("grade4").</span><span class="sxs-lookup"><span data-stu-id="4e022-304">The main difference between using BETWEEN in DocumentDB API and ANSI SQL is that you can express range queries against properties of mixed types – for example, you might have "grade" be a number (5) in some documents and strings in others ("grade4").</span></span> <span data-ttu-id="4e022-305">V těchto případech jako je v jazyce JavaScript, porovnání mezi dva různé typy výsledků v "undefined" a dokument bude přeskočen.</span><span class="sxs-lookup"><span data-stu-id="4e022-305">In these cases, like in JavaScript, a comparison between two different types results in "undefined", and the document will be skipped.</span></span>

### <a name="logical-and-or-and-not-operators"></a><span data-ttu-id="4e022-306">Logický (AND, OR a NOT) operátory</span><span class="sxs-lookup"><span data-stu-id="4e022-306">Logical (AND, OR and NOT) operators</span></span>
<span data-ttu-id="4e022-307">Logické operátory pracovat logické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e022-307">Logical operators operate on Boolean values.</span></span> <span data-ttu-id="4e022-308">Logické tabulky pravdivosti pro tyto operátory jsou uvedené v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="4e022-308">The logical truth tables for these operators are shown in the following tables.</span></span>

| <span data-ttu-id="4e022-309">NEBO</span><span class="sxs-lookup"><span data-stu-id="4e022-309">OR</span></span> | <span data-ttu-id="4e022-310">True</span><span class="sxs-lookup"><span data-stu-id="4e022-310">True</span></span> | <span data-ttu-id="4e022-311">False</span><span class="sxs-lookup"><span data-stu-id="4e022-311">False</span></span> | <span data-ttu-id="4e022-312">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-312">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4e022-313">True</span><span class="sxs-lookup"><span data-stu-id="4e022-313">True</span></span> |<span data-ttu-id="4e022-314">True</span><span class="sxs-lookup"><span data-stu-id="4e022-314">True</span></span> |<span data-ttu-id="4e022-315">True</span><span class="sxs-lookup"><span data-stu-id="4e022-315">True</span></span> |<span data-ttu-id="4e022-316">True</span><span class="sxs-lookup"><span data-stu-id="4e022-316">True</span></span> |
| <span data-ttu-id="4e022-317">False</span><span class="sxs-lookup"><span data-stu-id="4e022-317">False</span></span> |<span data-ttu-id="4e022-318">True</span><span class="sxs-lookup"><span data-stu-id="4e022-318">True</span></span> |<span data-ttu-id="4e022-319">False</span><span class="sxs-lookup"><span data-stu-id="4e022-319">False</span></span> |<span data-ttu-id="4e022-320">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-320">Undefined</span></span> |
| <span data-ttu-id="4e022-321">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-321">Undefined</span></span> |<span data-ttu-id="4e022-322">True</span><span class="sxs-lookup"><span data-stu-id="4e022-322">True</span></span> |<span data-ttu-id="4e022-323">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-323">Undefined</span></span> |<span data-ttu-id="4e022-324">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-324">Undefined</span></span> |

| <span data-ttu-id="4e022-325">A</span><span class="sxs-lookup"><span data-stu-id="4e022-325">AND</span></span> | <span data-ttu-id="4e022-326">True</span><span class="sxs-lookup"><span data-stu-id="4e022-326">True</span></span> | <span data-ttu-id="4e022-327">False</span><span class="sxs-lookup"><span data-stu-id="4e022-327">False</span></span> | <span data-ttu-id="4e022-328">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-328">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4e022-329">True</span><span class="sxs-lookup"><span data-stu-id="4e022-329">True</span></span> |<span data-ttu-id="4e022-330">True</span><span class="sxs-lookup"><span data-stu-id="4e022-330">True</span></span> |<span data-ttu-id="4e022-331">False</span><span class="sxs-lookup"><span data-stu-id="4e022-331">False</span></span> |<span data-ttu-id="4e022-332">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-332">Undefined</span></span> |
| <span data-ttu-id="4e022-333">False</span><span class="sxs-lookup"><span data-stu-id="4e022-333">False</span></span> |<span data-ttu-id="4e022-334">False</span><span class="sxs-lookup"><span data-stu-id="4e022-334">False</span></span> |<span data-ttu-id="4e022-335">False</span><span class="sxs-lookup"><span data-stu-id="4e022-335">False</span></span> |<span data-ttu-id="4e022-336">False</span><span class="sxs-lookup"><span data-stu-id="4e022-336">False</span></span> |
| <span data-ttu-id="4e022-337">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-337">Undefined</span></span> |<span data-ttu-id="4e022-338">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-338">Undefined</span></span> |<span data-ttu-id="4e022-339">False</span><span class="sxs-lookup"><span data-stu-id="4e022-339">False</span></span> |<span data-ttu-id="4e022-340">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-340">Undefined</span></span> |

| <span data-ttu-id="4e022-341">NENÍ</span><span class="sxs-lookup"><span data-stu-id="4e022-341">NOT</span></span> |  |
| --- | --- |
| <span data-ttu-id="4e022-342">True</span><span class="sxs-lookup"><span data-stu-id="4e022-342">True</span></span> |<span data-ttu-id="4e022-343">False</span><span class="sxs-lookup"><span data-stu-id="4e022-343">False</span></span> |
| <span data-ttu-id="4e022-344">False</span><span class="sxs-lookup"><span data-stu-id="4e022-344">False</span></span> |<span data-ttu-id="4e022-345">True</span><span class="sxs-lookup"><span data-stu-id="4e022-345">True</span></span> |
| <span data-ttu-id="4e022-346">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-346">Undefined</span></span> |<span data-ttu-id="4e022-347">Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="4e022-347">Undefined</span></span> |

### <a name="in-keyword"></a><span data-ttu-id="4e022-348">IN – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="4e022-348">IN keyword</span></span>
<span data-ttu-id="4e022-349">Klíčové slovo IN slouží ke kontrole, zda zadaná hodnota odpovídá žádnou hodnotu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="4e022-349">The IN keyword can be used to check whether a specified value matches any value in a list.</span></span> <span data-ttu-id="4e022-350">Například tento dotaz vrací všechny rodiny dokumenty, kde id je jedním z "WakefieldFamily" nebo "AndersenFamily".</span><span class="sxs-lookup"><span data-stu-id="4e022-350">For example, this query returns all family documents where the id is one of "WakefieldFamily" or "AndersenFamily".</span></span> 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

<span data-ttu-id="4e022-351">Tento příklad vrátí všechny dokumenty, kde je stav žádný ze zadaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e022-351">This example returns all documents where the state is any of the specified values.</span></span>

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a><span data-ttu-id="4e022-352">Unární (?) a operátory Coalesce (?)</span><span class="sxs-lookup"><span data-stu-id="4e022-352">Ternary (?) and Coalesce (??) operators</span></span>
<span data-ttu-id="4e022-353">Operátory Unární a Coalesce lze použít k vytvoření podmíněné výrazy, podobně jako oblíbené programovacích jazyků, jako je C# a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-353">The Ternary and Coalesce operators can be used to build conditional expressions, similar to popular programming languages like C# and JavaScript.</span></span> 

<span data-ttu-id="4e022-354">Operátor unární (?) může být velmi užitečné při vytváření nové vlastnosti JSON za chodu.</span><span class="sxs-lookup"><span data-stu-id="4e022-354">The Ternary (?) operator can be very handy when constructing new JSON properties on the fly.</span></span> <span data-ttu-id="4e022-355">Například teď můžete napsat dotazy ke klasifikaci třída úrovně do lidského čitelné podoby jako Začátečník nebo zprostředkující nebo Upřesnit, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-355">For example, now you can write queries to classify the class levels into a human readable form like Beginner/Intermediate/Advanced as shown below.</span></span>

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

<span data-ttu-id="4e022-356">Můžete také vnořit volání operátor jako v dotazu níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-356">You can also nest the calls to the operator like in the query below.</span></span>

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

<span data-ttu-id="4e022-357">Jako s dalšími operátory dotazu, pokud v dokumentu chybí odkazovaný vlastností v podmíněným výrazem, nebo pokud typy porovnávané se liší, pak tyto dokumenty nevylučují se ve výsledcích dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-357">As with other query operators, if the referenced properties in the conditional expression are missing in any document, or if the types being compared are different, then those documents are excluded in the query results.</span></span>

<span data-ttu-id="4e022-358">Operátor Coalesce (?) slouží k efektivní (také známa jako kontrolovat přítomnost vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e022-358">The Coalesce (??) operator can be used to efficiently check for the presence of a property (a.k.a.</span></span> <span data-ttu-id="4e022-359">je definován) v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-359">is defined) in a document.</span></span> <span data-ttu-id="4e022-360">To je užitečné při dotazování na částečně strukturovaných nebo data smíšený typů.</span><span class="sxs-lookup"><span data-stu-id="4e022-360">This is useful when querying against semi-structured or data of mixed types.</span></span> <span data-ttu-id="4e022-361">Tento dotaz vrací například "lastName", pokud existuje, nebo "Přezdívka" Pokud není přítomen.</span><span class="sxs-lookup"><span data-stu-id="4e022-361">For example, this query returns the "lastName" if present, or the "surname" if it isn't present.</span></span>

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <span data-ttu-id="4e022-362"><a id="EscapingReservedKeywords"></a>Vlastnost uvozovkách přístupového objektu</span><span class="sxs-lookup"><span data-stu-id="4e022-362"><a id="EscapingReservedKeywords"></a>Quoted property accessor</span></span>
<span data-ttu-id="4e022-363">Můžete také přístup k vlastnostem pomocí operátoru vlastnost uvozovkách `[]`.</span><span class="sxs-lookup"><span data-stu-id="4e022-363">You can also access properties using the quoted property operator `[]`.</span></span> <span data-ttu-id="4e022-364">Například `SELECT c.grade` a `SELECT c["grade"]` odpovídají.</span><span class="sxs-lookup"><span data-stu-id="4e022-364">For example, `SELECT c.grade` and `SELECT c["grade"]` are equivalent.</span></span> <span data-ttu-id="4e022-365">Tato syntaxe je užitečné, když potřebujete, abyste se vyhnuli vlastnost, která obsahuje mezery, speciální znaky, nebo se stane sdílet stejný název jako SQL – klíčové slovo nebo vyhrazené slovo.</span><span class="sxs-lookup"><span data-stu-id="4e022-365">This syntax is useful when you need to escape a property that contains spaces, special characters, or happens to share the same name as a SQL keyword or reserved word.</span></span>

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <span data-ttu-id="4e022-366"><a id="SelectClause"></a>Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="4e022-366"><a id="SelectClause"></a>SELECT clause</span></span>
<span data-ttu-id="4e022-367">Klauzule SELECT (**`SELECT <select_list>`**) je povinná a určuje, jaké hodnoty jsou načteny z dotazu, podobně jako v ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-367">The SELECT clause (**`SELECT <select_list>`**) is mandatory and specifies what values are retrieved from the query, just like in ANSI-SQL.</span></span> <span data-ttu-id="4e022-368">Podmnožina je filtrované nad dokumenty zdroje jsou předávány do fáze projekce, kde jsou načteny zadaných hodnot JSON a je vytvořený nový objekt JSON, pro každý vstupní předán na něj.</span><span class="sxs-lookup"><span data-stu-id="4e022-368">The subset that's been filtered on top of the source documents are passed onto the projection phase, where the specified JSON values are retrieved and a new JSON object is constructed, for each input passed onto it.</span></span> 

<span data-ttu-id="4e022-369">Následující příklad ukazuje typické dotaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="4e022-369">The following example shows a typical SELECT query.</span></span> 

<span data-ttu-id="4e022-370">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-370">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-371">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-371">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a><span data-ttu-id="4e022-372">Vnořené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="4e022-372">Nested properties</span></span>
<span data-ttu-id="4e022-373">V následujícím příkladu jsme jsou projekce dvě vnořené vlastnosti `f.address.state` a `f.address.city`.</span><span class="sxs-lookup"><span data-stu-id="4e022-373">In the following example, we are projecting two nested properties `f.address.state` and `f.address.city`.</span></span>

<span data-ttu-id="4e022-374">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-374">**Query**</span></span>

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-375">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-375">**Results**</span></span>

    [{
      "state": "WA", 
      "city": "seattle"
    }]


<span data-ttu-id="4e022-376">Projekce také podporuje JSON výrazy, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-376">Projection also supports JSON expressions as shown in the following example:</span></span>

<span data-ttu-id="4e022-377">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-377">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-378">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-378">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


<span data-ttu-id="4e022-379">Podívejme se na roli `$1` sem.</span><span class="sxs-lookup"><span data-stu-id="4e022-379">Let's look at the role of `$1` here.</span></span> <span data-ttu-id="4e022-380">`SELECT` Klauzule musí vytvořit objekt JSON a vzhledem k tomu, že žádný klíč je k dispozici, používáme názvy proměnných implicitní argument počínaje `$1`.</span><span class="sxs-lookup"><span data-stu-id="4e022-380">The `SELECT` clause needs to create a JSON object and since no key is provided, we use implicit argument variable names starting with `$1`.</span></span> <span data-ttu-id="4e022-381">Například tento dotaz vrací dvě implicitní argument proměnné s názvem bez přípony `$1` a `$2`.</span><span class="sxs-lookup"><span data-stu-id="4e022-381">For example, this query returns two implicit argument variables, labeled `$1` and `$2`.</span></span>

<span data-ttu-id="4e022-382">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-382">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-383">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-383">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a><span data-ttu-id="4e022-384">Aliasy</span><span class="sxs-lookup"><span data-stu-id="4e022-384">Aliasing</span></span>
<span data-ttu-id="4e022-385">Teď umožňuje rozšířit výše uvedeného příkladu s explicitní aliasy hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e022-385">Now let's extend the example above with explicit aliasing of values.</span></span> <span data-ttu-id="4e022-386">Tak, jak jsou klíčové slovo používané pro aliasy.</span><span class="sxs-lookup"><span data-stu-id="4e022-386">AS is the keyword used for aliasing.</span></span> <span data-ttu-id="4e022-387">Zadání je volitelné, jak je znázorněno při promítnutí druhá hodnota jako `NameInfo`.</span><span class="sxs-lookup"><span data-stu-id="4e022-387">It's optional as shown while projecting the second value as `NameInfo`.</span></span> 

<span data-ttu-id="4e022-388">V případě, že dotaz má dvě vlastnosti se stejným názvem, musí být aliasy používá k přejmenování jedno nebo obě vlastnosti tak, aby se jsou od sebe jednoznačně rozlišeny ve předpokládané výsledku.</span><span class="sxs-lookup"><span data-stu-id="4e022-388">In case a query has two properties with the same name, aliasing must be used to rename one or both of the properties so that they are disambiguated in the projected result.</span></span>

<span data-ttu-id="4e022-389">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-389">**Query**</span></span>

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-390">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-390">**Results**</span></span>

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a><span data-ttu-id="4e022-391">Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="4e022-391">Scalar expressions</span></span>
<span data-ttu-id="4e022-392">Kromě odkazů na vlastnost klauzule SELECT také podporuje skalární výrazy konstanty, aritmetických výrazech, logických výrazů, atd. Tady je příklad jednoduchého dotazu "Hello World".</span><span class="sxs-lookup"><span data-stu-id="4e022-392">In addition to property references, the SELECT clause also supports scalar expressions like constants, arithmetic expressions, logical expressions, etc. For example, here's a simple "Hello World" query.</span></span>

<span data-ttu-id="4e022-393">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-393">**Query**</span></span>

    SELECT "Hello World"

<span data-ttu-id="4e022-394">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-394">**Results**</span></span>

    [{
      "$1": "Hello World"
    }]


<span data-ttu-id="4e022-395">Zde je ukázka používající skalární výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-395">Here's a more complex example that uses a scalar expression.</span></span>

<span data-ttu-id="4e022-396">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-396">**Query**</span></span>

    SELECT ((2 + 11 % 7)-2)/3    

<span data-ttu-id="4e022-397">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-397">**Results**</span></span>

    [{
      "$1": 1.33333
    }]


<span data-ttu-id="4e022-398">V následujícím příkladu je výsledek skalární výraz logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="4e022-398">In the following example, the result of the scalar expression is a Boolean.</span></span>

<span data-ttu-id="4e022-399">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-399">**Query**</span></span>

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

<span data-ttu-id="4e022-400">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-400">**Results**</span></span>

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a><span data-ttu-id="4e022-401">Vytvoření objektu a pole</span><span class="sxs-lookup"><span data-stu-id="4e022-401">Object and array creation</span></span>
<span data-ttu-id="4e022-402">Další klíčovou funkcí DocumentDB SQL rozhraní API je vytvoření pole nebo objektu.</span><span class="sxs-lookup"><span data-stu-id="4e022-402">Another key feature of DocumentDB API SQL is array/object creation.</span></span> <span data-ttu-id="4e022-403">V předchozím příkladu Všimněte si, že jsme vytvořili nový objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-403">In the previous example, note that we created a new JSON object.</span></span> <span data-ttu-id="4e022-404">Podobně jeden můžete také vytvořit pole podle následujících příkladů:</span><span class="sxs-lookup"><span data-stu-id="4e022-404">Similarly, one can also construct arrays as shown in the following examples:</span></span>

<span data-ttu-id="4e022-405">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-405">**Query**</span></span>

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

<span data-ttu-id="4e022-406">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-406">**Results**</span></span>  

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

### <span data-ttu-id="4e022-407"><a id="ValueKeyword"></a>VALUE – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="4e022-407"><a id="ValueKeyword"></a>VALUE keyword</span></span>
<span data-ttu-id="4e022-408">**Hodnotu** – klíčové slovo poskytuje způsob, jak vrátit hodnotu JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-408">The **VALUE** keyword provides a way to return JSON value.</span></span> <span data-ttu-id="4e022-409">Například následující dotaz vrátí skalárních `"Hello World"` místo `{$1: "Hello World"}`.</span><span class="sxs-lookup"><span data-stu-id="4e022-409">For example, the query shown below returns the scalar `"Hello World"` instead of `{$1: "Hello World"}`.</span></span>

<span data-ttu-id="4e022-410">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-410">**Query**</span></span>

    SELECT VALUE "Hello World"

<span data-ttu-id="4e022-411">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-411">**Results**</span></span>

    [
      "Hello World"
    ]


<span data-ttu-id="4e022-412">Následující dotaz vrátí hodnotu JSON bez `"address"` popisek ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="4e022-412">The following query returns the JSON value without the `"address"` label in the results.</span></span>

<span data-ttu-id="4e022-413">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-413">**Query**</span></span>

    SELECT VALUE f.address
    FROM Families f    

<span data-ttu-id="4e022-414">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-414">**Results**</span></span>  

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

<span data-ttu-id="4e022-415">Následující příklad rozšiřuje na ukazují, jak vrátit JSON primitivní hodnoty (úroveň listu stromu JSON).</span><span class="sxs-lookup"><span data-stu-id="4e022-415">The following example extends this to show how to return JSON primitive values (the leaf level of the JSON tree).</span></span> 

<span data-ttu-id="4e022-416">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-416">**Query**</span></span>

    SELECT VALUE f.address.state
    FROM Families f    

<span data-ttu-id="4e022-417">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-417">**Results**</span></span>

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a><span data-ttu-id="4e022-418">* – Operátor</span><span class="sxs-lookup"><span data-stu-id="4e022-418">* Operator</span></span>
<span data-ttu-id="4e022-419">Podporován je speciální operátor (*) do projektu dokumentu jako-je.</span><span class="sxs-lookup"><span data-stu-id="4e022-419">The special operator (*) is supported to project the document as-is.</span></span> <span data-ttu-id="4e022-420">Pokud se používá, musí být pouze předpokládané pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-420">When used, it must be the only projected field.</span></span> <span data-ttu-id="4e022-421">Při dotazu jako `SELECT * FROM Families f` je platný, `SELECT VALUE * FROM Families f ` a `SELECT *, f.id FROM Families f ` nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="4e022-421">While a query like `SELECT * FROM Families f` is valid, `SELECT VALUE * FROM Families f ` and  `SELECT *, f.id FROM Families f ` are not valid.</span></span>

<span data-ttu-id="4e022-422">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-422">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="4e022-423">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-423">**Results**</span></span>

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

### <span data-ttu-id="4e022-424"><a id="TopKeyword"></a>Operátor TOP</span><span class="sxs-lookup"><span data-stu-id="4e022-424"><a id="TopKeyword"></a>TOP Operator</span></span>
<span data-ttu-id="4e022-425">TOP – klíčové slovo lze omezit počet hodnot z dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-425">The TOP keyword can be used to limit the number of values from a query.</span></span> <span data-ttu-id="4e022-426">Když horní se používá ve spojení s klauzulí ORDER BY, sadu výsledků dotazu je omezený na první číslo N seřazené hodnot. jinak vrátí první N počet výsledků v nedefinované pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e022-426">When TOP is used in conjunction with the ORDER BY clause, the result set is limited to the first N number of ordered values; otherwise, it returns the first N number of results in an undefined order.</span></span> <span data-ttu-id="4e022-427">Jako osvědčený postup v příkazu SELECT, s vždy používejte klauzuli ORDER BY v klauzuli nejvyšší.</span><span class="sxs-lookup"><span data-stu-id="4e022-427">As a best practice, in a SELECT statement, always use an ORDER BY clause with the TOP clause.</span></span> <span data-ttu-id="4e022-428">Toto je jediný způsob, jak předvídatelné označují řádky, které jsou ovlivněné TOP.</span><span class="sxs-lookup"><span data-stu-id="4e022-428">This is the only way to predictably indicate which rows are affected by TOP.</span></span> 

<span data-ttu-id="4e022-429">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-429">**Query**</span></span>

    SELECT TOP 1 * 
    FROM Families f 

<span data-ttu-id="4e022-430">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-430">**Results**</span></span>

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

<span data-ttu-id="4e022-431">HORNÍ lze použít s konstantní hodnotou (jak jsme ukázali výše) nebo s hodnotou proměnné použití parametrických dotazů.</span><span class="sxs-lookup"><span data-stu-id="4e022-431">TOP can be used with a constant value (as shown above) or with a variable value using parameterized queries.</span></span> <span data-ttu-id="4e022-432">Další podrobnosti najdete v tématu parametrizované dotazy níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-432">For more details, please see parameterized queries below.</span></span>

### <span data-ttu-id="4e022-433"><a id="Aggregates"></a>Agregační funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-433"><a id="Aggregates"></a>Aggregate Functions</span></span>
<span data-ttu-id="4e022-434">Můžete také provést agregace v `SELECT` klauzule.</span><span class="sxs-lookup"><span data-stu-id="4e022-434">You can also perform aggregations in the `SELECT` clause.</span></span> <span data-ttu-id="4e022-435">Agregační funkce provádět výpočet sadu hodnot a vrátí jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-435">Aggregate functions perform a calculation on a set of values and return a single value.</span></span> <span data-ttu-id="4e022-436">Například následující dotaz vrátí počet rodiny dokumentů v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-436">For example, the following query returns the count of family documents within the collection.</span></span>

<span data-ttu-id="4e022-437">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-437">**Query**</span></span>

    SELECT COUNT(1) 
    FROM Families f 

<span data-ttu-id="4e022-438">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-438">**Results**</span></span>

    [{
        "$1": 2
    }]

<span data-ttu-id="4e022-439">Můžete se taky vrátit skalární hodnota agregace pomocí `VALUE` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="4e022-439">You can also return the scalar value of the aggregate by using the `VALUE` keyword.</span></span> <span data-ttu-id="4e022-440">Například následující dotaz vrátí počet hodnot jako jediné číslo:</span><span class="sxs-lookup"><span data-stu-id="4e022-440">For example, the following query returns the count of values as a single number:</span></span>

<span data-ttu-id="4e022-441">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-441">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f 

<span data-ttu-id="4e022-442">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-442">**Results**</span></span>

    [ 2 ]

<span data-ttu-id="4e022-443">Můžete také provést agregace v kombinaci s filtry.</span><span class="sxs-lookup"><span data-stu-id="4e022-443">You can also perform aggregates in combination with filters.</span></span> <span data-ttu-id="4e022-444">Například následující dotaz vrátí počet dokumentů s adresou v státu Washington.</span><span class="sxs-lookup"><span data-stu-id="4e022-444">For example, the following query returns the count of documents with the address in the state of Washington.</span></span>

<span data-ttu-id="4e022-445">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-445">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

<span data-ttu-id="4e022-446">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-446">**Results**</span></span>

    [ 1 ]

<span data-ttu-id="4e022-447">Následující tabulka uvádí seznam podporovaných agregační funkce v DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e022-447">The following table shows the list of supported aggregate functions in DocumentDB API.</span></span> <span data-ttu-id="4e022-448">`SUM`a `AVG` se provádí přes číselných hodnot, zatímco `COUNT`, `MIN`, a `MAX` lze provést přes čísla, řetězce, logické hodnoty a hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="4e022-448">`SUM` and `AVG` are performed over numeric values, whereas `COUNT`, `MIN`, and `MAX` can be performed over numbers, strings, Booleans, and nulls.</span></span> 

| <span data-ttu-id="4e022-449">Využití</span><span class="sxs-lookup"><span data-stu-id="4e022-449">Usage</span></span> | <span data-ttu-id="4e022-450">Popis</span><span class="sxs-lookup"><span data-stu-id="4e022-450">Description</span></span> |
|-------|-------------|
| <span data-ttu-id="4e022-451">POČET</span><span class="sxs-lookup"><span data-stu-id="4e022-451">COUNT</span></span> | <span data-ttu-id="4e022-452">Vrátí počet položek ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-452">Returns the number of items in the expression.</span></span> |
| <span data-ttu-id="4e022-453">SOUČET</span><span class="sxs-lookup"><span data-stu-id="4e022-453">SUM</span></span>   | <span data-ttu-id="4e022-454">Vrátí součet všech hodnot ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-454">Returns the sum of all the values in the expression.</span></span> |
| <span data-ttu-id="4e022-455">MIN.</span><span class="sxs-lookup"><span data-stu-id="4e022-455">MIN</span></span>   | <span data-ttu-id="4e022-456">Vrátí minimální hodnotu ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-456">Returns the minimum value in the expression.</span></span> |
| <span data-ttu-id="4e022-457">MAXIMÁLNÍ POČET</span><span class="sxs-lookup"><span data-stu-id="4e022-457">MAX</span></span>   | <span data-ttu-id="4e022-458">Vrací maximální hodnotu ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-458">Returns the maximum value in the expression.</span></span> |
| <span data-ttu-id="4e022-459">PRŮMĚR</span><span class="sxs-lookup"><span data-stu-id="4e022-459">AVG</span></span>   | <span data-ttu-id="4e022-460">Vrátí průměr hodnot ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-460">Returns the average of the values in the expression.</span></span> |

<span data-ttu-id="4e022-461">Agreguje lze také provést přes výsledky iterace pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-461">Aggregates can also be performed over the results of an array iteration.</span></span> <span data-ttu-id="4e022-462">Další informace najdete v tématu [pole iterace v dotazech](#Iteration).</span><span class="sxs-lookup"><span data-stu-id="4e022-462">For more information, see [Array Iteration in Queries](#Iteration).</span></span>

> [!NOTE]
> <span data-ttu-id="4e022-463">Při použití Průzkumníka dotazů portálu Azure, Všimněte si, že agregace dotazy může vracet částečně agregované výsledky dotazu stránky.</span><span class="sxs-lookup"><span data-stu-id="4e022-463">When using the Azure portal's Query Explorer, note that aggregation queries may return the partially aggregated results over a query page.</span></span> <span data-ttu-id="4e022-464">Sady SDK vytvoří jednu kumulativní hodnotu na všech stránkách.</span><span class="sxs-lookup"><span data-stu-id="4e022-464">The SDKs produces a single cumulative value across all pages.</span></span> 
> 
> <span data-ttu-id="4e022-465">Aby bylo možné provádět dotazy agregace pomocí kódu, je nutné .NET SDK 1.12.0, .NET Core SDK 1.1.0 nebo Java SDK 1.9.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4e022-465">In order to perform aggregation queries using code, you need .NET SDK 1.12.0, .NET Core SDK 1.1.0, or Java SDK 1.9.5 or above.</span></span>    
>

## <span data-ttu-id="4e022-466"><a id="OrderByClause"></a>Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="4e022-466"><a id="OrderByClause"></a>ORDER BY clause</span></span>
<span data-ttu-id="4e022-467">Podobně jako v ANSI SQL, můžete zahrnout volitelné klauzule Order By při dotazování.</span><span class="sxs-lookup"><span data-stu-id="4e022-467">Like in ANSI-SQL, you can include an optional Order By clause while querying.</span></span> <span data-ttu-id="4e022-468">V klauzuli může zahrnovat nepovinný argument ASC nebo DESC zadat pořadí, ve kterém musí načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="4e022-468">The clause can include an optional ASC/DESC argument to specify the order in which results must be retrieved.</span></span>

<span data-ttu-id="4e022-469">Tady je příklad dotaz, který načte rodiny v pořadí podle název trvalé města.</span><span class="sxs-lookup"><span data-stu-id="4e022-469">For example, here's a query that retrieves families in order of the resident city's name.</span></span>

<span data-ttu-id="4e022-470">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-470">**Query**</span></span>

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

<span data-ttu-id="4e022-471">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-471">**Results**</span></span>

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

<span data-ttu-id="4e022-472">A zde uvádíme dotaz, který načte rodiny v pořadí podle data vytvoření, který je uložený jako číslo představující epoch čas, tj, uplynulý čas od 1 ledna, pod hodnotou 1970 v sekundách.</span><span class="sxs-lookup"><span data-stu-id="4e022-472">And here's a query that retrieves families in order of creation date, which is stored as a number representing the epoch time, i.e, elapsed time since Jan 1, 1970 in seconds.</span></span>

<span data-ttu-id="4e022-473">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-473">**Query**</span></span>

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

<span data-ttu-id="4e022-474">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-474">**Results**</span></span>

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

## <span data-ttu-id="4e022-475"><a id="Advanced"></a>Pokročilé databázových koncepcí a dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-475"><a id="Advanced"></a>Advanced database concepts and SQL queries</span></span>

### <span data-ttu-id="4e022-476"><a id="Iteration"></a>Iterace</span><span class="sxs-lookup"><span data-stu-id="4e022-476"><a id="Iteration"></a>Iteration</span></span>
<span data-ttu-id="4e022-477">Byl přidán nový konstrukce prostřednictvím **IN** – klíčové slovo v DocumentDB SQL rozhraní API poskytuje podporu pro iterování přes pole JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-477">A new construct was added via the **IN** keyword in DocumentDB API SQL to provide support for iterating over JSON arrays.</span></span> <span data-ttu-id="4e022-478">Zdroj FROM poskytuje podporu pro iterací.</span><span class="sxs-lookup"><span data-stu-id="4e022-478">The FROM source provides support for iteration.</span></span> <span data-ttu-id="4e022-479">Začneme v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-479">Let's start with the following example:</span></span>

<span data-ttu-id="4e022-480">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-480">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="4e022-481">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-481">**Results**</span></span>  

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

<span data-ttu-id="4e022-482">Nyní Podíváme se na další dotaz, který provádí iteraci přes podřízené položky v kolekci.</span><span class="sxs-lookup"><span data-stu-id="4e022-482">Now let's look at another query that performs iteration over children in the collection.</span></span> <span data-ttu-id="4e022-483">Poznámka: rozdíl v poli výstup.</span><span class="sxs-lookup"><span data-stu-id="4e022-483">Note the difference in the output array.</span></span> <span data-ttu-id="4e022-484">Tento příklad rozdělí `children` a vyrovná výsledky do jednoho pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-484">This example splits `children` and flattens the results into a single array.</span></span>  

<span data-ttu-id="4e022-485">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-485">**Query**</span></span>

    SELECT * 
    FROM c IN Families.children

<span data-ttu-id="4e022-486">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-486">**Results**</span></span>  

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

<span data-ttu-id="4e022-487">To dále lze filtrovat na každou položku pole, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-487">This can be further used to filter on each individual entry of the array as shown in the following example:</span></span>

<span data-ttu-id="4e022-488">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-488">**Query**</span></span>

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

<span data-ttu-id="4e022-489">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-489">**Results**</span></span>  

    [{
      "givenName": "Lisa"
    }]

<span data-ttu-id="4e022-490">Můžete také provést agregace přes výsledek iterace pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-490">You can also perform aggregation over the result of array iteration.</span></span> <span data-ttu-id="4e022-491">Například následující dotaz vrátí počet podřízených prvků mezi všechny řady.</span><span class="sxs-lookup"><span data-stu-id="4e022-491">For example, the following query counts the number of children among all families.</span></span>

<span data-ttu-id="4e022-492">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-492">**Query**</span></span>

    SELECT COUNT(child) 
    FROM child IN Families.children

<span data-ttu-id="4e022-493">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-493">**Results**</span></span>  

    [
      { 
        "$1": 3
      }
    ]

### <span data-ttu-id="4e022-494"><a id="Joins"></a>Spojení</span><span class="sxs-lookup"><span data-stu-id="4e022-494"><a id="Joins"></a>Joins</span></span>
<span data-ttu-id="4e022-495">V relační databázi je důležité potřeba připojení u tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e022-495">In a relational database, the need to join across tables is important.</span></span> <span data-ttu-id="4e022-496">Je logické důsledkem k navrhování normalizovaný schémat.</span><span class="sxs-lookup"><span data-stu-id="4e022-496">It's the logical corollary to designing normalized schemas.</span></span> <span data-ttu-id="4e022-497">Na rozdíl od toho se zabývá DocumentDB API nenormalizované datový model bez schémat dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4e022-497">Contrary to this, DocumentDB API deals with the denormalized data model of schema-free documents.</span></span> <span data-ttu-id="4e022-498">Toto je logický ekvivalent a "spojení sama na sebe".</span><span class="sxs-lookup"><span data-stu-id="4e022-498">This is the logical equivalent of a "self-join".</span></span>

<span data-ttu-id="4e022-499">Syntaxe, které jazyk podporuje je < from_source1 > připojit < from_source2 > připojit... Připojte < from_sourceN >.</span><span class="sxs-lookup"><span data-stu-id="4e022-499">The syntax that the language supports is <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>.</span></span> <span data-ttu-id="4e022-500">Celkově platí, tento příkaz vrátí sadu **N**- n-tice (řazené kolekce členů s **N** hodnoty).</span><span class="sxs-lookup"><span data-stu-id="4e022-500">Overall, this returns a set of **N**-tuples (tuple with **N** values).</span></span> <span data-ttu-id="4e022-501">Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e022-501">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span> <span data-ttu-id="4e022-502">Jinými slovy Toto je úplná smíšený produkt sad účastní spojení.</span><span class="sxs-lookup"><span data-stu-id="4e022-502">In other words, this is a full cross product of the sets participating in the join.</span></span>

<span data-ttu-id="4e022-503">Následující příklady ukazují, jak funguje klauzuli JOIN.</span><span class="sxs-lookup"><span data-stu-id="4e022-503">The following examples show how the JOIN clause works.</span></span> <span data-ttu-id="4e022-504">V následujícím příkladu výsledkem je prázdný, od smíšený produkt každého dokumentu ze zdroje a prázdnou sadou je prázdný.</span><span class="sxs-lookup"><span data-stu-id="4e022-504">In the following example, the result is empty since the cross product of each document from source and an empty set is empty.</span></span>

<span data-ttu-id="4e022-505">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-505">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

<span data-ttu-id="4e022-506">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-506">**Results**</span></span>  

    [{
    }]


<span data-ttu-id="4e022-507">V následujícím příkladu je spojení mezi kořen dokumentu a `children` subroot.</span><span class="sxs-lookup"><span data-stu-id="4e022-507">In the following example, the join is between the document root and the `children` subroot.</span></span> <span data-ttu-id="4e022-508">Je smíšený produkt mezi dvěma objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-508">It's a cross product between two JSON objects.</span></span> <span data-ttu-id="4e022-509">Skutečnost, že je podřízených prvků pole není platná ve spojení vzhledem k tomu, že jsme se zabývají na jednom kořenovou, která je pole podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="4e022-509">The fact that children is an array is not effective in the JOIN since we are dealing with a single root that is the children array.</span></span> <span data-ttu-id="4e022-510">Proto výsledek obsahuje pouze dva výsledky, protože smíšený produkt každý dokument s poli vypočítá přesně pouze jeden dokument.</span><span class="sxs-lookup"><span data-stu-id="4e022-510">Hence the result contains only two results, since the cross product of each document with the array yields exactly only one document.</span></span>

<span data-ttu-id="4e022-511">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-511">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.children

<span data-ttu-id="4e022-512">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-512">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


<span data-ttu-id="4e022-513">Následující příklad ukazuje konvenční připojení:</span><span class="sxs-lookup"><span data-stu-id="4e022-513">The following example shows a more conventional join:</span></span>

<span data-ttu-id="4e022-514">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-514">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

<span data-ttu-id="4e022-515">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-515">**Results**</span></span>

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



<span data-ttu-id="4e022-516">Nejprve si všimněte si je, že `from_source` z **připojení** klauzule je iterátor.</span><span class="sxs-lookup"><span data-stu-id="4e022-516">The first thing to note is that the `from_source` of the **JOIN** clause is an iterator.</span></span> <span data-ttu-id="4e022-517">Ano tok v takovém případě je následující:</span><span class="sxs-lookup"><span data-stu-id="4e022-517">So, the flow in this case is as follows:</span></span>  

* <span data-ttu-id="4e022-518">Rozbalte každý podřízený element **c** v poli.</span><span class="sxs-lookup"><span data-stu-id="4e022-518">Expand each child element **c** in the array.</span></span>
* <span data-ttu-id="4e022-519">Použít smíšený produkt s kořene dokumentu **f** s každou podřízený element **c** , byl průmětu v prvním kroku.</span><span class="sxs-lookup"><span data-stu-id="4e022-519">Apply a cross product with the root of the document **f** with each child element **c** that was flattened in the first step.</span></span>
* <span data-ttu-id="4e022-520">Nakonec projektu kořenový objekt **f** name – vlastnost samostatně.</span><span class="sxs-lookup"><span data-stu-id="4e022-520">Finally, project the root object **f** name property alone.</span></span> 

<span data-ttu-id="4e022-521">První dokument (`AndersenFamily`) obsahuje pouze jeden podřízený element, takže sadu výsledků dotazu obsahuje pouze jeden objekt odpovídající do tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-521">The first document (`AndersenFamily`) contains only one child element, so the result set contains only a single object corresponding to this document.</span></span> <span data-ttu-id="4e022-522">Druhý dokumentu (`WakefieldFamily`) obsahuje dva podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="4e022-522">The second document (`WakefieldFamily`) contains two children.</span></span> <span data-ttu-id="4e022-523">Ano smíšený produkt vytváří samostatný objekt pro všechny podřízené, což by vedlo k dva objekty, jeden pro každou podřízenou odpovídající do tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-523">So, the cross product produces a separate object for each child, thereby resulting in two objects, one for each child corresponding to this document.</span></span> <span data-ttu-id="4e022-524">Kořenové pole v obou tyto dokumenty jsou stejné, stejně, jako byste očekávali v smíšený produkt.</span><span class="sxs-lookup"><span data-stu-id="4e022-524">The root fields in both these documents are the same, just as you would expect in a cross product.</span></span>

<span data-ttu-id="4e022-525">Skutečné nástroje připojení k je formulář řazených kolekcí členů z smíšený produkt tvar, který je jinak obtížné projektu.</span><span class="sxs-lookup"><span data-stu-id="4e022-525">The real utility of the JOIN is to form tuples from the cross-product in a shape that's otherwise difficult to project.</span></span> <span data-ttu-id="4e022-526">Kromě toho, jak vidíte v následujícím příkladu můžete vyfiltrovat na kombinaci řazené kolekce členů, aby se umožňuje se uživatel rozhodl podmínku splňují celkové řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="4e022-526">Furthermore, as we see in the example below, you could filter on the combination of a tuple that lets' the user chose a condition satisfied by the tuples overall.</span></span>

<span data-ttu-id="4e022-527">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-527">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

<span data-ttu-id="4e022-528">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-528">**Results**</span></span>

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



<span data-ttu-id="4e022-529">Tento příklad představuje přirozené rozšíření v předchozím příkladu a spojí double.</span><span class="sxs-lookup"><span data-stu-id="4e022-529">This example is a natural extension of the preceding example, and performs a double join.</span></span> <span data-ttu-id="4e022-530">Ano smíšený produkt lze zobrazit jako pseudo následující kód:</span><span class="sxs-lookup"><span data-stu-id="4e022-530">So, the cross product can be viewed as the following pseudo-code:</span></span>

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

<span data-ttu-id="4e022-531">`AndersenFamily`má jednu podřízenou, který má jednoho nebo více mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="4e022-531">`AndersenFamily` has one child who has one pet.</span></span> <span data-ttu-id="4e022-532">Ano, smíšený produkt vypočítá jeden řádek (1\*1\*1) z této rodiny.</span><span class="sxs-lookup"><span data-stu-id="4e022-532">So, the cross product yields one row (1\*1\*1) from this family.</span></span> <span data-ttu-id="4e022-533">WakefieldFamily ale má dva podřízené, ale pouze jednu podřízenou "Jesse" má mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="4e022-533">WakefieldFamily however has two children, but only one child "Jesse" has pets.</span></span> <span data-ttu-id="4e022-534">Jesse, když má dva mazlíčků.</span><span class="sxs-lookup"><span data-stu-id="4e022-534">Jesse has two pets though.</span></span> <span data-ttu-id="4e022-535">Proto smíšený produkt vypočítá 1\*1\*řádků z této rodině, 2 = 2.</span><span class="sxs-lookup"><span data-stu-id="4e022-535">Hence the cross product yields 1\*1\*2 = 2 rows from this family.</span></span>

<span data-ttu-id="4e022-536">V následujícím příkladu je další filtr na `pet`.</span><span class="sxs-lookup"><span data-stu-id="4e022-536">In the next example, there is an additional filter on `pet`.</span></span> <span data-ttu-id="4e022-537">Nevztahuje se na všech záznamů, kde název pet není "Stínové".</span><span class="sxs-lookup"><span data-stu-id="4e022-537">This excludes all the tuples where the pet name is not "Shadow".</span></span> <span data-ttu-id="4e022-538">Všimněte si, že jsou jsme sestavení řazenými kolekcemi členů z pole filtru na všech elementů řazené kolekce členů a projektu libovolnou kombinaci prvků.</span><span class="sxs-lookup"><span data-stu-id="4e022-538">Notice that we are able to build tuples from arrays, filter on any of the elements of the tuple, and project any combination of the elements.</span></span> 

<span data-ttu-id="4e022-539">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-539">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

<span data-ttu-id="4e022-540">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-540">**Results**</span></span>

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <span data-ttu-id="4e022-541"><a id="JavaScriptIntegration"></a>Integrace jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e022-541"><a id="JavaScriptIntegration"></a>JavaScript integration</span></span>
<span data-ttu-id="4e022-542">Azure Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na kolekcích z hlediska uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="4e022-542">Azure Cosmos DB provides a programming model for executing JavaScript based application logic directly on the collections in terms of stored procedures and triggers.</span></span> <span data-ttu-id="4e022-543">To umožňuje, aby obě:</span><span class="sxs-lookup"><span data-stu-id="4e022-543">This allows for both:</span></span>

* <span data-ttu-id="4e022-544">Možnost udělat transakční operace CRUD vysoce výkonné a dotazy na dokumenty v kolekci na základě těsná integrace běhu programu JavaScript přímo v rámci databázového stroje.</span><span class="sxs-lookup"><span data-stu-id="4e022-544">Ability to do high-performance transactional CRUD operations and queries against documents in a collection by virtue of the deep integration of JavaScript runtime directly within the database engine.</span></span> 
* <span data-ttu-id="4e022-545">Fyzická modelování tok řízení, proměnné rozsahu a přiřazení a integrace výjimky zpracování primitiv s databázové transakce.</span><span class="sxs-lookup"><span data-stu-id="4e022-545">A natural modeling of control flow, variable scoping, and assignment and integration of exception handling primitives with database transactions.</span></span> <span data-ttu-id="4e022-546">Další podrobnosti o podpoře Azure Cosmos DB integrace jazyka JavaScript naleznete v dokumentaci serverové programovatelnosti JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-546">For more details about Azure Cosmos DB support for JavaScript integration, please refer to the JavaScript server-side programmability documentation.</span></span>

### <span data-ttu-id="4e022-547"><a id="UserDefinedFunctions"></a>Uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="4e022-547"><a id="UserDefinedFunctions"></a>User-Defined Functions (UDFs)</span></span>
<span data-ttu-id="4e022-548">Společně s typy již definována v tomto článku DocumentDB SQL rozhraní API poskytuje podporu pro uživatele definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="4e022-548">Along with the types already defined in this article, DocumentDB API SQL provides support for User Defined Functions (UDF).</span></span> <span data-ttu-id="4e022-549">Skalární funkce UDF zejména, jsou podporovány, kde mohou vývojáři předejte v počtu nula či více argumentů a vrácení zpět výsledku jeden argument.</span><span class="sxs-lookup"><span data-stu-id="4e022-549">In particular, scalar UDFs are supported where the developers can pass in zero or many arguments and return a single argument result back.</span></span> <span data-ttu-id="4e022-550">Každý z těchto argumentů, se kontroluje na právě platné hodnoty na JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-550">Each of these arguments is checked for being legal JSON values.</span></span>  

<span data-ttu-id="4e022-551">Syntaxi DocumentDB SQL rozhraní API není rozšířené k podpoře vlastní logiky aplikace pomocí tyto funkce definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="4e022-551">The DocumentDB API SQL syntax is extended to support custom application logic using these User-Defined Functions.</span></span> <span data-ttu-id="4e022-552">Funkce UDF lze registrovat pomocí rozhraní API DocumentDB a pak odkazuje v rámci dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-552">UDFs can be registered with DocumentDB API and then be referenced as part of a SQL query.</span></span> <span data-ttu-id="4e022-553">Ve skutečnosti UDF jsou exquisitely navrženy pro vyvolat dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-553">In fact, the UDFs are exquisitely designed to be invoked by queries.</span></span> <span data-ttu-id="4e022-554">Jako nezbytným důsledkem této volby UDF nemají přístup k objektu kontextu, které mají jiné JavaScript typy (uložených procedur a aktivačních událostí).</span><span class="sxs-lookup"><span data-stu-id="4e022-554">As a corollary to this choice, UDFs do not have access to the context object which the other JavaScript types (stored procedures and triggers) have.</span></span> <span data-ttu-id="4e022-555">Vzhledem k tomu, že dotazy se spustí jen pro čtení, mohou spouštět na primární nebo na sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="4e022-555">Since queries execute as read-only, they can run either on primary or on secondary replicas.</span></span> <span data-ttu-id="4e022-556">Proto UDF jsou určená ke spuštění na sekundárních replikách na rozdíl od jiných typů jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-556">Therefore, UDFs are designed to run on secondary replicas unlike other JavaScript types.</span></span>

<span data-ttu-id="4e022-557">Níže je příklad, jak se dají registrovat UDF v databázi Cosmos DB, konkrétně v rámci kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4e022-557">Below is an example of how a UDF can be registered at the Cosmos DB database, specifically under a document collection.</span></span>

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

<span data-ttu-id="4e022-558">V předchozím příkladu se vytváří UDF, jehož název je `REGEX_MATCH`.</span><span class="sxs-lookup"><span data-stu-id="4e022-558">The preceding example creates a UDF whose name is `REGEX_MATCH`.</span></span> <span data-ttu-id="4e022-559">Přijímá dvou řetězcových hodnot JSON `input` a `pattern` a ověří, zda je první odpovídá vzoru zadaný ve druhém pomocí funkce string.match() jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-559">It accepts two JSON string values `input` and `pattern` and checks if the first matches the pattern specified in the second using JavaScript's string.match() function.</span></span>

<span data-ttu-id="4e022-560">Tato UDF jsme teď můžete použít v dotazu v projekci.</span><span class="sxs-lookup"><span data-stu-id="4e022-560">We can now use this UDF in a query in a projection.</span></span> <span data-ttu-id="4e022-561">Funkce UDF musí být kvalifikovaný pomocí malá a velká písmena předponu "udf."</span><span class="sxs-lookup"><span data-stu-id="4e022-561">UDFs must be qualified with the case-sensitive prefix "udf."</span></span> <span data-ttu-id="4e022-562">Když je volána v rámci dotazů.</span><span class="sxs-lookup"><span data-stu-id="4e022-562">when called from within queries.</span></span> 

> [!NOTE]
> <span data-ttu-id="4e022-563">Před 3/17/2015 Cosmos DB podporované UDF volání bez "udf."</span><span class="sxs-lookup"><span data-stu-id="4e022-563">Prior to 3/17/2015, Cosmos DB supported UDF calls without the "udf."</span></span> <span data-ttu-id="4e022-564">Předpona jako vyberte REGEX_MATCH().</span><span class="sxs-lookup"><span data-stu-id="4e022-564">prefix like SELECT REGEX_MATCH().</span></span> <span data-ttu-id="4e022-565">Tento vzor volání je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="4e022-565">This calling pattern has been deprecated.</span></span>  
> 
> 

<span data-ttu-id="4e022-566">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-566">**Query**</span></span>

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

<span data-ttu-id="4e022-567">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-567">**Results**</span></span>

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

<span data-ttu-id="4e022-568">UDF můžete použít také uvnitř filtr, jak je znázorněno v následujícím příkladu také kvalifikovaný pomocí "udf."</span><span class="sxs-lookup"><span data-stu-id="4e022-568">The UDF can also be used inside a filter as shown in the example below, also qualified with the "udf."</span></span> <span data-ttu-id="4e022-569">Předpona:</span><span class="sxs-lookup"><span data-stu-id="4e022-569">prefix:</span></span>

<span data-ttu-id="4e022-570">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-570">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

<span data-ttu-id="4e022-571">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-571">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


<span data-ttu-id="4e022-572">V podstatě UDF jsou platné skalární výrazy a mohou být používány projekce a filtry.</span><span class="sxs-lookup"><span data-stu-id="4e022-572">In essence, UDFs are valid scalar expressions and can be used in both projections and filters.</span></span> 

<span data-ttu-id="4e022-573">Chcete-li rozšířit na výkon UDF, podíváme se na další příklad s podmíněnou logiku:</span><span class="sxs-lookup"><span data-stu-id="4e022-573">To expand on the power of UDFs, let's look at another example with conditional logic:</span></span>

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


<span data-ttu-id="4e022-574">Níže je příklad, který vykonává UDF.</span><span class="sxs-lookup"><span data-stu-id="4e022-574">Below is an example that exercises the UDF.</span></span>

<span data-ttu-id="4e022-575">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-575">**Query**</span></span>

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

<span data-ttu-id="4e022-576">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-576">**Results**</span></span>

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


<span data-ttu-id="4e022-577">Jako v předchozích příkladech prezentují, funkce UDF integrovat s DocumentDB SQL rozhraní API k poskytnutí bohaté programovatelný rozhraní udělat komplexní logiku procedurální, podmíněného pomocí integrované funkce JavaScript runtime sílu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-577">As the preceding examples showcase, UDFs integrate the power of JavaScript language with the DocumentDB API SQL to provide a rich programmable interface to do complex procedural, conditional logic with the help of inbuilt JavaScript runtime capabilities.</span></span>

<span data-ttu-id="4e022-578">DocumentDB SQL rozhraní API poskytuje argumenty k UDF pro každý dokument ve zdroji na aktuální fázi (klauzuli WHERE nebo klauzuli SELECT) zpracování UDF.</span><span class="sxs-lookup"><span data-stu-id="4e022-578">DocumentDB API SQL provides the arguments to the UDFs for each document in the source at the current stage (WHERE clause or SELECT clause) of processing the UDF.</span></span> <span data-ttu-id="4e022-579">Výsledkem je obsažena v celkové spouštěcí kanál bezproblémově.</span><span class="sxs-lookup"><span data-stu-id="4e022-579">The result is incorporated in the overall execution pipeline seamlessly.</span></span> <span data-ttu-id="4e022-580">Jestliže podle vlastnosti ve UDF parametry nejsou k dispozici v hodnotě JSON, parametr se považuje za není definována a proto je volání UDF zcela přeskočeno.</span><span class="sxs-lookup"><span data-stu-id="4e022-580">If the properties referred to by the UDF parameters are not available in the JSON value, the parameter is considered as undefined and hence the UDF invocation is entirely skipped.</span></span> <span data-ttu-id="4e022-581">Podobně pokud výsledek UDF, není součástí výsledek.</span><span class="sxs-lookup"><span data-stu-id="4e022-581">Similarly if the result of the UDF is undefined, it's not included in the result.</span></span> 

<span data-ttu-id="4e022-582">V souhrnu funkce UDF jsou vynikající aplikace udělat komplexní obchodní logiky v rámci dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-582">In summary, UDFs are great tools to do complex business logic as part of the query.</span></span>

### <a name="operator-evaluation"></a><span data-ttu-id="4e022-583">Vyhodnocení – operátor</span><span class="sxs-lookup"><span data-stu-id="4e022-583">Operator evaluation</span></span>
<span data-ttu-id="4e022-584">Cosmos databáze, důsledku způsobená databáze JSON nevykresluje parallels s operátory jazyka JavaScript a jeho sémantiku vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="4e022-584">Cosmos DB, by the virtue of being a JSON database, draws parallels with JavaScript operators and its evaluation semantics.</span></span> <span data-ttu-id="4e022-585">Při Cosmos DB pokusí zachovat sémantiku JavaScript z hlediska podporu JSON, v některých případech odchylují vyhodnocení operaci.</span><span class="sxs-lookup"><span data-stu-id="4e022-585">While Cosmos DB tries to preserve JavaScript semantics in terms of JSON support, the operation evaluation deviates in some instances.</span></span>

<span data-ttu-id="4e022-586">V DocumentDB SQL rozhraní API, na rozdíl od v tradiční SQL, typy hodnot, jsou často není známý teprve po načtení hodnoty z databáze.</span><span class="sxs-lookup"><span data-stu-id="4e022-586">In DocumentDB API SQL, unlike in traditional SQL, the types of values are often not known until the values are retrieved from database.</span></span> <span data-ttu-id="4e022-587">Efektivní provádění dotazů, většina operátory má požadavky na typ strict.</span><span class="sxs-lookup"><span data-stu-id="4e022-587">In order to efficiently execute queries, most of the operators have strict type requirements.</span></span> 

<span data-ttu-id="4e022-588">DocumentDB API SQL neprovede implicitní převody, na rozdíl od jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4e022-588">DocumentDB API SQL doesn't perform implicit conversions, unlike JavaScript.</span></span> <span data-ttu-id="4e022-589">Například dotazu jako `SELECT * FROM Person p WHERE p.Age = 21` odpovídá dokumentů, které obsahují ve vlastnosti stáří, jehož hodnota je 21.</span><span class="sxs-lookup"><span data-stu-id="4e022-589">For instance, a query like `SELECT * FROM Person p WHERE p.Age = 21` matches documents that contain an Age property whose value is 21.</span></span> <span data-ttu-id="4e022-590">Jiného dokumentu, jejichž stáří vlastnost odpovídá řetězec "21" nebo jiných může být nekonečné variace jako "021", "21.0", "0021", "00021", nebude odpovídat atd.</span><span class="sxs-lookup"><span data-stu-id="4e022-590">Any other document whose Age property matches string "21", or other possibly infinite variations like "021", "21.0", "0021", "00021", etc. will not be matched.</span></span> <span data-ttu-id="4e022-591">Jde na rozdíl od jazyka JavaScript, kde jsou implicitně převedena na čísla řetězcové hodnoty (podle operátoru, například: ==).</span><span class="sxs-lookup"><span data-stu-id="4e022-591">This is in contrast to the JavaScript where the string values are implicitly casted to numbers (based on operator, ex: ==).</span></span> <span data-ttu-id="4e022-592">Tato volba je zásadní pro efektivní indexu odpovídající v DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e022-592">This choice is crucial for efficient index matching in DocumentDB API SQL.</span></span> 

## <a name="parameterized-sql-queries"></a><span data-ttu-id="4e022-593">Parametrizované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-593">Parameterized SQL queries</span></span>
<span data-ttu-id="4e022-594">Cosmos DB podporuje dotazy s parametry vyjádřené se známými @ zápis.</span><span class="sxs-lookup"><span data-stu-id="4e022-594">Cosmos DB supports queries with parameters expressed with the familiar @ notation.</span></span> <span data-ttu-id="4e022-595">Parametrizované SQL poskytuje robustní zpracování a uvozovací znaky vstup uživatele brání náhodnou expozici dat prostřednictvím Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-595">Parameterized SQL provides robust handling and escaping of user input, preventing accidental exposure of data through SQL injection.</span></span> 

<span data-ttu-id="4e022-596">Můžete například napsat dotaz, který přebírá příjmení a stav adresy jako parametry a potom spusťte pro různé hodnoty příjmení a stav adresy založené na vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e022-596">For example, you can write a query that takes last name and address state as parameters, and then execute it for various values of last name and address state based on user input.</span></span>

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

<span data-ttu-id="4e022-597">Tento požadavek potom můžete odeslat do databáze Cosmos jako parametrizovaného dotazu JSON, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-597">This request can then be sent to Cosmos DB as a parameterized JSON query like shown below.</span></span>

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

<span data-ttu-id="4e022-598">Argument TOP se dá nastavit pomocí parametrizované dotazy, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-598">The argument to TOP can be set using parameterized queries like shown below.</span></span>

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

<span data-ttu-id="4e022-599">Hodnoty parametru může být libovolný platný kód JSON (řetězce, čísla, logické hodnoty null, dokonce i pole nebo vnořený JSON).</span><span class="sxs-lookup"><span data-stu-id="4e022-599">Parameter values can be any valid JSON (strings, numbers, Booleans, null, even arrays or nested JSON).</span></span> <span data-ttu-id="4e022-600">Také bez schématu totiž Cosmos DB parametry nejsou ověřovat na libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="4e022-600">Also since Cosmos DB is schema-less, parameters are not validated against any type.</span></span>

## <span data-ttu-id="4e022-601"><a id="BuiltinFunctions"></a>Integrované funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-601"><a id="BuiltinFunctions"></a>Built-in functions</span></span>
<span data-ttu-id="4e022-602">Cosmos DB podporuje také řadu integrovaných funkcí pro běžné operace, které lze použít uvnitř dotazy jako uživatelsky definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="4e022-602">Cosmos DB also supports a number of built-in functions for common operations, that can be used inside queries like user-defined functions (UDFs).</span></span>

| <span data-ttu-id="4e022-603">Funkce skupiny</span><span class="sxs-lookup"><span data-stu-id="4e022-603">Function group</span></span>          | <span data-ttu-id="4e022-604">Operace</span><span class="sxs-lookup"><span data-stu-id="4e022-604">Operations</span></span>                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4e022-605">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-605">Mathematical functions</span></span>  | <span data-ttu-id="4e022-606">ABS mezní hodnoty, EXP, FLOOR, protokolu, LOG10, POWER, KRUHOVÉ, přihlášení, SQRT, HRANATÉ, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COP, STUPŇŮ, PI, RADIÁNECH, SIN a TAN</span><span class="sxs-lookup"><span data-stu-id="4e022-606">ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, and TAN</span></span> |
| <span data-ttu-id="4e022-607">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-607">Type checking functions</span></span> | <span data-ttu-id="4e022-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED a IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="4e022-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, and IS_PRIMITIVE</span></span>                                                           |
| <span data-ttu-id="4e022-609">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-609">String functions</span></span>        | <span data-ttu-id="4e022-610">CONCAT, obsahuje, ENDSWITH, INDEX_OF, vlevo, délka, nižší, LTRIM, NAHRAĎTE, REPLIKOVAT, zpětného, vpravo, RTRIM, STARTSWITH, SUBSTRING a horní</span><span class="sxs-lookup"><span data-stu-id="4e022-610">CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, and UPPER</span></span>       |
| <span data-ttu-id="4e022-611">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="4e022-611">Array functions</span></span>         | <span data-ttu-id="4e022-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH a ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="4e022-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH, and ARRAY_SLICE</span></span>                                                                                         |
| <span data-ttu-id="4e022-613">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-613">Spatial functions</span></span>       | <span data-ttu-id="4e022-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID a ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="4e022-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, and ST_ISVALIDDETAILED</span></span>                                                                           | 

<span data-ttu-id="4e022-615">Pokud aktuálně používáte uživatelem definované funkce (UDF) pro kterou integrovaná funkce je nyní k dispozici, byste měli používat odpovídající integrované funkce, má být ke spuštění rychlejší a efektivnější.</span><span class="sxs-lookup"><span data-stu-id="4e022-615">If you’re currently using a user-defined function (UDF) for which a built-in function is now available, you should use the corresponding built-in function as it is going to be quicker to run and more efficiently.</span></span> 

### <a name="mathematical-functions"></a><span data-ttu-id="4e022-616">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-616">Mathematical functions</span></span>
<span data-ttu-id="4e022-617">Matematické funkce provedení výpočtu, podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-617">The mathematical functions each perform a calculation, based on input values that are provided as arguments, and return a numeric value.</span></span> <span data-ttu-id="4e022-618">Zde je tabulku podporovaných předdefinovaných matematické funkce.</span><span class="sxs-lookup"><span data-stu-id="4e022-618">Here’s a table of supported built-in mathematical functions.</span></span>


| <span data-ttu-id="4e022-619">Využití</span><span class="sxs-lookup"><span data-stu-id="4e022-619">Usage</span></span> | <span data-ttu-id="4e022-620">Popis</span><span class="sxs-lookup"><span data-stu-id="4e022-620">Description</span></span> |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4e022-621">[[ABS (num_expr)](#bk_abs)</span><span class="sxs-lookup"><span data-stu-id="4e022-621">[[ABS (num_expr)](#bk_abs)</span></span> | <span data-ttu-id="4e022-622">Vrátí absolutní hodnotu (kladné) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-622">Returns the absolute (positive) value of the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-623">Horní MEZ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-623">CEILING (num_expr)</span></span>](#bk_ceiling) | <span data-ttu-id="4e022-624">Vrátí nejmenší hodnotu, celé číslo větší než nebo rovna hodnotě zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-624">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-625">FLOOR (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-625">FLOOR (num_expr)</span></span>](#bk_floor) | <span data-ttu-id="4e022-626">Vrátí největší celé číslo menší než nebo rovna zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-626">Returns the largest integer less than or equal to the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-627">EXP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-627">EXP (num_expr)</span></span>](#bk_exp) | <span data-ttu-id="4e022-628">Vrátí exponent zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-628">Returns the exponent of the specified numeric expression.</span></span> |
| <span data-ttu-id="4e022-629">[PROTOKOL (num_expr [, základní])](#bk_log)</span><span class="sxs-lookup"><span data-stu-id="4e022-629">[LOG (num_expr [,base])](#bk_log)</span></span> | <span data-ttu-id="4e022-630">Vrátí přirozený logaritmus zadaný číselný výraz nebo pomocí o zadaném základu logaritmus</span><span class="sxs-lookup"><span data-stu-id="4e022-630">Returns the natural logarithm of the specified numeric expression, or the logarithm using the specified base</span></span> |
| [<span data-ttu-id="4e022-631">LOG10 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-631">LOG10 (num_expr)</span></span>](#bk_log10) | <span data-ttu-id="4e022-632">Vrátí hodnotu logaritmické základu 10 zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-632">Returns the base-10 logarithmic value of the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-633">ZAOKROUHLÍ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-633">ROUND (num_expr)</span></span>](#bk_round) | <span data-ttu-id="4e022-634">Vrátí číselnou hodnotu, zaokrouhlí na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="4e022-634">Returns a numeric value, rounded to the closest integer value.</span></span> |
| [<span data-ttu-id="4e022-635">TRUNC (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-635">TRUNC (num_expr)</span></span>](#bk_trunc) | <span data-ttu-id="4e022-636">Vrátí číselnou hodnotu, zkrácen na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="4e022-636">Returns a numeric value, truncated to the closest integer value.</span></span> |
| [<span data-ttu-id="4e022-637">SQRT (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-637">SQRT (num_expr)</span></span>](#bk_sqrt) | <span data-ttu-id="4e022-638">Vrátí druhou odmocninu čísla zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-638">Returns the square root of the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-639">HRANATÉ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-639">SQUARE (num_expr)</span></span>](#bk_square) | <span data-ttu-id="4e022-640">Vrátí druhou mocninu zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-640">Returns the square of the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-641">NAPÁJENÍ (num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-641">POWER (num_expr, num_expr)</span></span>](#bk_power) | <span data-ttu-id="4e022-642">Hodnota zadaná vrátí sílu zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-642">Returns the power of the specified numeric expression to the value specified.</span></span> |
| [<span data-ttu-id="4e022-643">PŘIHLÁŠENÍ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-643">SIGN (num_expr)</span></span>](#bk_sign) | <span data-ttu-id="4e022-644">Vrátí hodnotu přihlašovací (-1, 0, 1) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-644">Returns the sign value (-1, 0, 1) of the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-645">ACOS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-645">ACOS (num_expr)</span></span>](#bk_acos) | <span data-ttu-id="4e022-646">Vrací úhel, v radiánech, jehož kosinus je zadaný číselný výraz. Zkratka Arkus.</span><span class="sxs-lookup"><span data-stu-id="4e022-646">Returns the angle, in radians, whose cosine is the specified numeric expression; also called arccosine.</span></span> |
| [<span data-ttu-id="4e022-647">ASIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-647">ASIN (num_expr)</span></span>](#bk_asin) | <span data-ttu-id="4e022-648">Vrací úhel, v radiánech, jehož sinus je zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-648">Returns the angle, in radians, whose sine is the specified numeric expression.</span></span> <span data-ttu-id="4e022-649">To je také označován Arkus sinus.</span><span class="sxs-lookup"><span data-stu-id="4e022-649">This is also called arcsine.</span></span> |
| [<span data-ttu-id="4e022-650">ATAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-650">ATAN (num_expr)</span></span>](#bk_atan) | <span data-ttu-id="4e022-651">Vrací úhel, v radiánech, jehož tangens je zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-651">Returns the angle, in radians, whose tangent is the specified numeric expression.</span></span> <span data-ttu-id="4e022-652">To je také označován Arkus.</span><span class="sxs-lookup"><span data-stu-id="4e022-652">This is also called arctangent.</span></span> |
| [<span data-ttu-id="4e022-653">ATN2 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-653">ATN2 (num_expr)</span></span>](#bk_atn2) | <span data-ttu-id="4e022-654">Vrací úhel, v radiánech, mezi kladné osy x a paprsek z tohoto počátku do bodu (y, x), kde x a y jsou hodnoty dvou výrazů zadaný float.</span><span class="sxs-lookup"><span data-stu-id="4e022-654">Returns the angle, in radians, between the positive x-axis and the ray from the origin to the point (y, x), where x and y are the values of the two specified float expressions.</span></span> |
| [<span data-ttu-id="4e022-655">COS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-655">COS (num_expr)</span></span>](#bk_cos) | <span data-ttu-id="4e022-656">Vrací trigonometrické kosinus určeného úhlu v radiánech v zadaným výrazem.</span><span class="sxs-lookup"><span data-stu-id="4e022-656">Returns the trigonometric cosine of the specified angle, in radians, in the specified expression.</span></span> |
| [<span data-ttu-id="4e022-657">COP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-657">COT (num_expr)</span></span>](#bk_cot) | <span data-ttu-id="4e022-658">Vrací trigonometrické kotangens zadaný úhel v radiánech v zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-658">Returns the trigonometric cotangent of the specified angle, in radians, in the specified numeric expression.</span></span> |
| [<span data-ttu-id="4e022-659">STUPŇŮ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-659">DEGREES (num_expr)</span></span>](#bk_degrees) | <span data-ttu-id="4e022-660">Vrací odpovídající úhel ve stupních pro úhlu uvedeného v radiánech.</span><span class="sxs-lookup"><span data-stu-id="4e022-660">Returns the corresponding angle in degrees for an angle specified in radians.</span></span> |
| [<span data-ttu-id="4e022-661">PI)</span><span class="sxs-lookup"><span data-stu-id="4e022-661">PI ()</span></span>](#bk_pi) | <span data-ttu-id="4e022-662">Vrátí konstantní hodnotu čísla PÍ.</span><span class="sxs-lookup"><span data-stu-id="4e022-662">Returns the constant value of PI.</span></span> |
| [<span data-ttu-id="4e022-663">RADIÁNECH (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-663">RADIANS (num_expr)</span></span>](#bk_radians) | <span data-ttu-id="4e022-664">Vrátí radiánech při zadání číselného výrazu, ve stupních, se.</span><span class="sxs-lookup"><span data-stu-id="4e022-664">Returns radians when a numeric expression, in degrees, is entered.</span></span> |
| [<span data-ttu-id="4e022-665">SIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-665">SIN (num_expr)</span></span>](#bk_sin) | <span data-ttu-id="4e022-666">Vrací trigonometrické sinus určeného úhlu v radiánech v zadaným výrazem.</span><span class="sxs-lookup"><span data-stu-id="4e022-666">Returns the trigonometric sine of the specified angle, in radians, in the specified expression.</span></span> |
| [<span data-ttu-id="4e022-667">TAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-667">TAN (num_expr)</span></span>](#bk_tan) | <span data-ttu-id="4e022-668">Vrátí tangens vstupní výraz zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-668">Returns the tangent of the input expression, in the specified expression.</span></span> |

<span data-ttu-id="4e022-669">Například můžete spustit nyní dotazy takto:</span><span class="sxs-lookup"><span data-stu-id="4e022-669">For example, you can now run queries like the following:</span></span>

<span data-ttu-id="4e022-670">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-670">**Query**</span></span>

    SELECT VALUE ABS(-4)

<span data-ttu-id="4e022-671">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-671">**Results**</span></span>

    [4]

<span data-ttu-id="4e022-672">Hlavní rozdíl mezi Cosmos DB funkce ve srovnání s ANSI SQL je, že jsou navrženy fungují dobře u dat bez schématu a smíšený schématu.</span><span class="sxs-lookup"><span data-stu-id="4e022-672">The main difference between Cosmos DB’s functions compared to ANSI SQL is that they are designed to work well with schema-less and mixed schema data.</span></span> <span data-ttu-id="4e022-673">Například pokud máte dokument, kdy je velikost vlastnost chybí, nebo má jiné než číselné hodnoty jako "Neznámý" a potom dokument se přeskočil, místo vrátila chybu.</span><span class="sxs-lookup"><span data-stu-id="4e022-673">For example, if you have a document where the Size property is missing, or has a non-numeric value like “unknown”, then the document is skipped over, instead of returning an error.</span></span>

### <a name="type-checking-functions"></a><span data-ttu-id="4e022-674">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-674">Type checking functions</span></span>
<span data-ttu-id="4e022-675">Kontrola, zda funkce typů umožňují zkontrolujte typ výrazu v rámci dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-675">The type checking functions allow you to check the type of an expression within SQL queries.</span></span> <span data-ttu-id="4e022-676">Kontrola, zda funkce typu slouží k určení typu vlastnosti v rámci dokumenty za chodu, když je neznámý nebo proměnné.</span><span class="sxs-lookup"><span data-stu-id="4e022-676">Type checking functions can be used to determine the type of properties within documents on the fly when it is variable or unknown.</span></span> <span data-ttu-id="4e022-677">Zde je tabulku kontroluje funkce podporované předdefinovaný typ.</span><span class="sxs-lookup"><span data-stu-id="4e022-677">Here’s a table of supported built-in type checking functions.</span></span>

<table>
<tr>
  <td><span data-ttu-id="4e022-678"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="4e022-678"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="4e022-679"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="4e022-679"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-681">Vrátí logickou hodnotu, která určuje, jestli je typ hodnoty pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-681">Returns a Boolean indicating if the type of the value is an array.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-683">Vrátí logickou hodnotu udávající, pokud typ hodnoty je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="4e022-683">Returns a Boolean indicating if the type of the value is a Boolean.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-685">Vrátí logickou hodnotu, která určuje, jestli je typ hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="4e022-685">Returns a Boolean indicating if the type of the value is null.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-687">Vrátí logickou hodnotu udávající, zda je typ hodnoty číslo.</span><span class="sxs-lookup"><span data-stu-id="4e022-687">Returns a Boolean indicating if the type of the value is a number.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-689">Vrátí logickou hodnotu udávající, pokud typ hodnoty je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-689">Returns a Boolean indicating if the type of the value is a JSON object.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-691">Vrátí logickou hodnotu udávající, pokud je typ hodnoty string.</span><span class="sxs-lookup"><span data-stu-id="4e022-691">Returns a Boolean indicating if the type of the value is a string.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-693">Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-693">Returns a Boolean indicating if the property has been assigned a value.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (výraz)</a></span><span class="sxs-lookup"><span data-stu-id="4e022-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></span></span></td>
  <td><span data-ttu-id="4e022-695">Vrátí logickou hodnotu, která udává, pokud je typ hodnoty řetězce, číslo, logickou hodnotu nebo hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4e022-695">Returns a Boolean indicating if the type of the value is a string, number, Boolean or null.</span></span></td>
</tr>

</table>

<span data-ttu-id="4e022-696">Pomocí těchto funkcí, teď můžete spouštět dotazy takto:</span><span class="sxs-lookup"><span data-stu-id="4e022-696">Using these functions, you can now run queries like the following:</span></span>

<span data-ttu-id="4e022-697">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-697">**Query**</span></span>

    SELECT VALUE IS_NUMBER(-4)

<span data-ttu-id="4e022-698">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-698">**Results**</span></span>

    [true]

### <a name="string-functions"></a><span data-ttu-id="4e022-699">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-699">String functions</span></span>
<span data-ttu-id="4e022-700">Následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="4e022-700">The following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span> <span data-ttu-id="4e022-701">Tady je tabulku funkce integrované řetězce:</span><span class="sxs-lookup"><span data-stu-id="4e022-701">Here's a table of built-in string functions:</span></span>

| <span data-ttu-id="4e022-702">Využití</span><span class="sxs-lookup"><span data-stu-id="4e022-702">Usage</span></span> | <span data-ttu-id="4e022-703">Popis</span><span class="sxs-lookup"><span data-stu-id="4e022-703">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="4e022-704">Délka (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-704">LENGTH (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |<span data-ttu-id="4e022-705">Vrátí počet znaků ze zadaného řetězcového výrazu</span><span class="sxs-lookup"><span data-stu-id="4e022-705">Returns the number of characters of the specified string expression</span></span> |
| <span data-ttu-id="4e022-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span><span class="sxs-lookup"><span data-stu-id="4e022-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span></span> |<span data-ttu-id="4e022-707">Vrátí řetězec, který je výsledkem zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e022-707">Returns a string that is the result of concatenating two or more string values.</span></span> |
| [<span data-ttu-id="4e022-708">SUBSTRING (str_expr, num_expr num_expr.)</span><span class="sxs-lookup"><span data-stu-id="4e022-708">SUBSTRING (str_expr, num_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |<span data-ttu-id="4e022-709">Vrátí část řetězcového výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-709">Returns part of a string expression.</span></span> |
| [<span data-ttu-id="4e022-710">STARTSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-710">STARTSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |<span data-ttu-id="4e022-711">Vrátí logická hodnota, která určuje zda první řetězec výraz končí druhý</span><span class="sxs-lookup"><span data-stu-id="4e022-711">Returns a Boolean indicating whether the first string expression ends with the second</span></span> |
| [<span data-ttu-id="4e022-712">ENDSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-712">ENDSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |<span data-ttu-id="4e022-713">Vrátí logická hodnota, která určuje zda první řetězec výraz končí druhý</span><span class="sxs-lookup"><span data-stu-id="4e022-713">Returns a Boolean indicating whether the first string expression ends with the second</span></span> |
| [<span data-ttu-id="4e022-714">OBSAHUJE (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-714">CONTAINS (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |<span data-ttu-id="4e022-715">Vrátí logická hodnota, která určuje zda první řetězec výraz obsahuje druhý.</span><span class="sxs-lookup"><span data-stu-id="4e022-715">Returns a Boolean indicating whether the first string expression contains the second.</span></span> |
| [<span data-ttu-id="4e022-716">INDEX_OF (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-716">INDEX_OF (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |<span data-ttu-id="4e022-717">Vrátí počáteční pozici prvního výskytu druhý řetězec výrazu v rámci první zadaného řetězcového výrazu nebo -1, pokud není nalezen řetězec.</span><span class="sxs-lookup"><span data-stu-id="4e022-717">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span> |
| [<span data-ttu-id="4e022-718">LEFT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-718">LEFT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |<span data-ttu-id="4e022-719">Vrátí levé části řetězec s zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="4e022-719">Returns the left part of a string with the specified number of characters.</span></span> |
| [<span data-ttu-id="4e022-720">RIGHT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-720">RIGHT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |<span data-ttu-id="4e022-721">Vrátí pravou část řetězec s zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="4e022-721">Returns the right part of a string with the specified number of characters.</span></span> |
| [<span data-ttu-id="4e022-722">LTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-722">LTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |<span data-ttu-id="4e022-723">Vrací výraz řetězce po ho odebere úvodní mezery.</span><span class="sxs-lookup"><span data-stu-id="4e022-723">Returns a string expression after it removes leading blanks.</span></span> |
| [<span data-ttu-id="4e022-724">RTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-724">RTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |<span data-ttu-id="4e022-725">Vrací výraz řetězce po zkracování všechny koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="4e022-725">Returns a string expression after truncating all trailing blanks.</span></span> |
| [<span data-ttu-id="4e022-726">NIŽŠÍ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-726">LOWER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |<span data-ttu-id="4e022-727">Vrací výraz řetězce po převodu dat velké písmeno na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4e022-727">Returns a string expression after converting uppercase character data to lowercase.</span></span> |
| [<span data-ttu-id="4e022-728">HORNÍ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-728">UPPER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |<span data-ttu-id="4e022-729">Vrací výraz řetězce po převodu dat malé písmeno na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="4e022-729">Returns a string expression after converting lowercase character data to uppercase.</span></span> |
| [<span data-ttu-id="4e022-730">NAHRAĎTE (str_expr, str_expr str_expr.)</span><span class="sxs-lookup"><span data-stu-id="4e022-730">REPLACE (str_expr, str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |<span data-ttu-id="4e022-731">Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="4e022-731">Replaces all occurrences of a specified string value with another string value.</span></span> |
| [<span data-ttu-id="4e022-732">REPLIKOVAT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-732">REPLICATE (str_expr, num_expr)</span></span>](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |<span data-ttu-id="4e022-733">Opakuje hodnotu řetězce zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="4e022-733">Repeats a string value a specified number of times.</span></span> |
| [<span data-ttu-id="4e022-734">ZPĚTNÉHO (str_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-734">REVERSE (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |<span data-ttu-id="4e022-735">Vrátí obráceném pořadí řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-735">Returns the reverse order of a string value.</span></span> |

<span data-ttu-id="4e022-736">Pomocí těchto funkcí, můžete nyní spustit dotazy podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="4e022-736">Using these functions, you can now run queries like the following.</span></span> <span data-ttu-id="4e022-737">Například se můžete vrátit název rodiny na velká písmena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e022-737">For example, you can return the family name in uppercase as follows:</span></span>

<span data-ttu-id="4e022-738">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-738">**Query**</span></span>

    SELECT VALUE UPPER(Families.id)
    FROM Families

<span data-ttu-id="4e022-739">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-739">**Results**</span></span>

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

<span data-ttu-id="4e022-740">Nebo zřetězení řetězců jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-740">Or concatenate strings like in this example:</span></span>

<span data-ttu-id="4e022-741">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-741">**Query**</span></span>

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

<span data-ttu-id="4e022-742">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-742">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


<span data-ttu-id="4e022-743">Funkce řetězce mohou sloužit také v klauzuli WHERE chcete filtrovat výsledky, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e022-743">String functions can also be used in the WHERE clause to filter results, like in the following example:</span></span>

<span data-ttu-id="4e022-744">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-744">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

<span data-ttu-id="4e022-745">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-745">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a><span data-ttu-id="4e022-746">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="4e022-746">Array functions</span></span>
<span data-ttu-id="4e022-747">Následující skalární funkce provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="4e022-747">The following scalar functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span> <span data-ttu-id="4e022-748">Zde je také tabulka předdefinovaných pole funkcí:</span><span class="sxs-lookup"><span data-stu-id="4e022-748">Here's a table of built-in array functions:</span></span>

| <span data-ttu-id="4e022-749">Využití</span><span class="sxs-lookup"><span data-stu-id="4e022-749">Usage</span></span> | <span data-ttu-id="4e022-750">Popis</span><span class="sxs-lookup"><span data-stu-id="4e022-750">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="4e022-751">ARRAY_LENGTH (arr_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-751">ARRAY_LENGTH (arr_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |<span data-ttu-id="4e022-752">Vrátí počet prvků výrazu zadané pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-752">Returns the number of elements of the specified array expression.</span></span> |
| <span data-ttu-id="4e022-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span><span class="sxs-lookup"><span data-stu-id="4e022-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span></span> |<span data-ttu-id="4e022-754">Vrátí pole, které je výsledkem zřetězení dvě nebo více hodnot pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-754">Returns an array that is the result of concatenating two or more array values.</span></span> |
| <span data-ttu-id="4e022-755">[ARRAY_CONTAINS (arr_expr, výraz [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span><span class="sxs-lookup"><span data-stu-id="4e022-755">[ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span></span> |<span data-ttu-id="4e022-756">Vrátí logickou hodnotu udávající, zda pole obsahuje zadanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-756">Returns a Boolean indicating whether the array contains the specified value.</span></span> <span data-ttu-id="4e022-757">Můžete zadat, pokud je shoda celé nebo jeho část.</span><span class="sxs-lookup"><span data-stu-id="4e022-757">Can specify if the match is full or partial.</span></span> |
| <span data-ttu-id="4e022-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span><span class="sxs-lookup"><span data-stu-id="4e022-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span></span> |<span data-ttu-id="4e022-759">Vrátí část výraz pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-759">Returns part of an array expression.</span></span> |

<span data-ttu-id="4e022-760">Funkce pole můžete použít k manipulaci s pole v rámci JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-760">Array functions can be used to manipulate arrays within JSON.</span></span> <span data-ttu-id="4e022-761">Tady je příklad dotaz, který vrátí všechny dokumenty, kde jeden z rodičů je "Každý s každým Wakefieldů".</span><span class="sxs-lookup"><span data-stu-id="4e022-761">For example, here's a query that returns all documents where one of the parents is "Robin Wakefield".</span></span> 

<span data-ttu-id="4e022-762">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-762">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

<span data-ttu-id="4e022-763">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-763">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="4e022-764">Můžete zadat částečné fragment pro párování elementů v rámci pole.</span><span class="sxs-lookup"><span data-stu-id="4e022-764">You can specify a partial fragment for matching elements within the array.</span></span> <span data-ttu-id="4e022-765">Následující dotaz hledá všechny nadřazené objekty s `givenName` z `Robin`.</span><span class="sxs-lookup"><span data-stu-id="4e022-765">The following query finds all parents with the `givenName` of `Robin`.</span></span>

<span data-ttu-id="4e022-766">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-766">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

<span data-ttu-id="4e022-767">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-767">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]


<span data-ttu-id="4e022-768">Zde je další příklad používající ARRAY_LENGTH získat počet podřízených za rodiny.</span><span class="sxs-lookup"><span data-stu-id="4e022-768">Here's another example that uses ARRAY_LENGTH to get the number of children per family.</span></span>

<span data-ttu-id="4e022-769">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-769">**Query**</span></span>

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

<span data-ttu-id="4e022-770">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-770">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a><span data-ttu-id="4e022-771">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="4e022-771">Spatial functions</span></span>
<span data-ttu-id="4e022-772">Cosmos DB podporuje následující předdefinované funkce otevřete geoprostorové Consortium (OGC) pro geoprostorové dotazování.</span><span class="sxs-lookup"><span data-stu-id="4e022-772">Cosmos DB supports the following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> 

<table>
<tr>
  <td><span data-ttu-id="4e022-773"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="4e022-773"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="4e022-774"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="4e022-774"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-775">ST_DISTANCE (point_expr, point_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-775">ST_DISTANCE (point_expr, point_expr)</span></span></td>
  <td><span data-ttu-id="4e022-776">Vrací vzdálenost mezi dvěma GeoJSON bodu, mnohoúhelníku nebo LineString výrazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-776">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-777">ST_WITHIN (point_expr, polygon_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-777">ST_WITHIN (point_expr, polygon_expr)</span></span></td>
  <td><span data-ttu-id="4e022-778">Vrací výraz logická hodnota určující, zda je první objekt GeoJSON (bod, mnohoúhelníku nebo LineString) je v rámci druhý objekt GeoJSON (bod, mnohoúhelníku nebo LineString).</span><span class="sxs-lookup"><span data-stu-id="4e022-778">Returns a Boolean expression indicating whether the first GeoJSON object (Point, Polygon, or LineString) is within the second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="4e022-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="4e022-780">Vrátí hodnotu označující, zda dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-780">Returns a Boolean expression indicating whether the two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-781">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="4e022-781">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="4e022-782">Vrátí logickou hodnotu udávající, zda je zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString platný.</span><span class="sxs-lookup"><span data-stu-id="4e022-782">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="4e022-783">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="4e022-783">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="4e022-784">Vrátí hodnotu hodnotu JSON obsahující logickou hodnotu, pokud zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString je platný a pokud neplatný, dále z důvodu jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="4e022-784">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="4e022-785">Prostorové funkcí lze provádět dotazy blízkosti proti prostorová data.</span><span class="sxs-lookup"><span data-stu-id="4e022-785">Spatial functions can be used to perform proximity queries against spatial data.</span></span> <span data-ttu-id="4e022-786">Tady je příklad dotaz, který vrátí všechny rodiny dokumenty, které jsou v rámci 30 km v zadaném umístění pomocí předdefinované funkci ST_DISTANCE.</span><span class="sxs-lookup"><span data-stu-id="4e022-786">For example, here's a query that returns all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="4e022-787">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="4e022-787">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="4e022-788">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-788">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="4e022-789">Další informace o podporovaných geoprostorové v Cosmos DB, najdete v tématu [práci s daty geoprostorové v Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="4e022-789">For more details on geospatial support in Cosmos DB, please see [Working with geospatial data in Azure Cosmos DB](geospatial.md).</span></span> <span data-ttu-id="4e022-790">Který zabalí prostorových funkce a syntaxe SQL pro Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-790">That wraps up spatial functions, and the SQL syntax for Cosmos DB.</span></span> <span data-ttu-id="4e022-791">Nyní Podívejme se na tom, jak LINQ dotazování funguje a jak komunikuje se syntaxí jsme viděli dosavadní.</span><span class="sxs-lookup"><span data-stu-id="4e022-791">Now let's take a look at how LINQ querying works and how it interacts with the syntax we've seen so far.</span></span>

## <span data-ttu-id="4e022-792"><a id="Linq"></a>Technologie LINQ to SQL DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e022-792"><a id="Linq"></a>LINQ to DocumentDB API SQL</span></span>
<span data-ttu-id="4e022-793">LINQ je programovací model rozhraní .NET, která vyjadřuje výpočetní jako dotazy na datové proudy objektů.</span><span class="sxs-lookup"><span data-stu-id="4e022-793">LINQ is a .NET programming model that expresses computation as queries on streams of objects.</span></span> <span data-ttu-id="4e022-794">Cosmos DB poskytuje knihovnu klienta pro rozhraní s dotazy LINQ usnadněním převod mezi objekty JSON a rozhraní .NET a mapování z určité podmnožiny dotazů LINQ dotazy Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-794">Cosmos DB provides a client-side library to interface with LINQ by facilitating a conversion between JSON and .NET objects and a mapping from a subset of LINQ queries to Cosmos DB queries.</span></span> 

<span data-ttu-id="4e022-795">Následující obrázek ukazuje architekturu podporu dotazů LINQ pomocí Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-795">The picture below shows the architecture of supporting LINQ queries using Cosmos DB.</span></span>  <span data-ttu-id="4e022-796">Pomocí klienta aplikace Cosmos DB vývojáři můžou vytvářet **IQueryable** objekt, který dotazuje přímo poskytovatele dotazu Cosmos DB, který pak překládá dotaz LINQ do dotazu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-796">Using the Cosmos DB client, developers can create an **IQueryable** object that directly queries the Cosmos DB query provider, which then translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="4e022-797">Dotaz je předána na server Cosmos databáze k načtení sady výsledků ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-797">The query is then passed to the Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="4e022-798">Do vrácených výsledků se deserializovat do datového proudu objektů .NET na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4e022-798">The returned results are deserialized into a stream of .NET objects on the client side.</span></span>

![Architektura podporu dotazů LINQ pomocí rozhraní API DocumentDB - syntaxe SQL, JSON dotazovací jazyk, databázových koncepcí a dotazy SQL][1]

### <a name="net-and-json-mapping"></a><span data-ttu-id="4e022-800">Rozhraní .NET a mapování JSON</span><span class="sxs-lookup"><span data-stu-id="4e022-800">.NET and JSON mapping</span></span>
<span data-ttu-id="4e022-801">Mapování mezi objekty .NET a dokumenty JSON přirozené – každé datové pole, člen je namapovaný na objekt JSON, kde název pole je namapovaná na část "klíč" objektu a části "value" je rekurzivní namapované na část hodnoty objektu.</span><span class="sxs-lookup"><span data-stu-id="4e022-801">The mapping between .NET objects and JSON documents is natural - each data member field is mapped to a JSON object, where the field name is mapped to the "key" part of the object and the "value" part is recursively mapped to the value part of the object.</span></span> <span data-ttu-id="4e022-802">Podívejte se na následující příklad: rodiny objekt vytvořený je namapována na dokumentu JSON, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4e022-802">Consider the following example: The Family object created is mapped to the JSON document as shown below.</span></span> <span data-ttu-id="4e022-803">A naopak a dokumentu JSON je mapována na objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="4e022-803">And vice versa, the JSON document is mapped back to a .NET object.</span></span>

<span data-ttu-id="4e022-804">**C# – třída**</span><span class="sxs-lookup"><span data-stu-id="4e022-804">**C# Class**</span></span>

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


<span data-ttu-id="4e022-805">**JSON**</span><span class="sxs-lookup"><span data-stu-id="4e022-805">**JSON**</span></span>  

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



### <a name="linq-to-sql-translation"></a><span data-ttu-id="4e022-806">Technologie LINQ to SQL překlad</span><span class="sxs-lookup"><span data-stu-id="4e022-806">LINQ to SQL translation</span></span>
<span data-ttu-id="4e022-807">Zprostředkovatel dotazu Cosmos DB provede nejlepší úsilí mapování z dotazu LINQ do dotazu Cosmos DB SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-807">The Cosmos DB query provider performs a best effort mapping from a LINQ query into a Cosmos DB SQL query.</span></span> <span data-ttu-id="4e022-808">V následující popis předpokládáme, že program pro čtení má základní znalosti o LINQ.</span><span class="sxs-lookup"><span data-stu-id="4e022-808">In the following description, we assume the reader has a basic familiarity of LINQ.</span></span>

<span data-ttu-id="4e022-809">Nejprve pro typ systému, budeme podporovat všechny JSON primitivní typy – číselnými typy, logická hodnota, string a hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4e022-809">First, for the type system, we support all JSON primitive types – numeric types, boolean, string, and null.</span></span> <span data-ttu-id="4e022-810">Jsou podporovány pouze tyto typy JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-810">Only these JSON types are supported.</span></span> <span data-ttu-id="4e022-811">Jsou podporovány následující skalární výrazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-811">The following scalar expressions are supported.</span></span>

* <span data-ttu-id="4e022-812">Konstantní hodnoty – patří konstantní hodnoty primitivní datové typy v době, kdy je vyhodnocován dotaz.</span><span class="sxs-lookup"><span data-stu-id="4e022-812">Constant values – these include constant values of the primitive data types at the time the query is evaluated.</span></span>
* <span data-ttu-id="4e022-813">Vlastnost nebo pole indexu výrazy – tyto výrazy odkazovat na vlastnost objekt nebo pole elementu.</span><span class="sxs-lookup"><span data-stu-id="4e022-813">Property/array index expressions – these expressions refer to the property of an object or an array element.</span></span>
  
     <span data-ttu-id="4e022-814">rodiny. ID;    Family.Children[0].familyName;    Family.Children[0].Grade;    Family.Children[n].Grade; n je proměnná typu int.</span><span class="sxs-lookup"><span data-stu-id="4e022-814">family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is an int variable</span></span>
* <span data-ttu-id="4e022-815">Aritmetických výrazech – jedná se o běžné aritmetických výrazech na číselné a logické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e022-815">Arithmetic expressions - These include common arithmetic expressions on numerical and boolean values.</span></span> <span data-ttu-id="4e022-816">Úplný seznam najdete v části specifikace SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-816">For the complete list, refer to the SQL specification.</span></span>
  
     <span data-ttu-id="4e022-817">2 * family.children[0].grade;    x a y;</span><span class="sxs-lookup"><span data-stu-id="4e022-817">2 * family.children[0].grade;    x + y;</span></span>
* <span data-ttu-id="4e022-818">Výraz pro porovnání řetězce - patří porovnávání řetězcovou hodnotu na hodnotu konstantní řetězec.</span><span class="sxs-lookup"><span data-stu-id="4e022-818">String comparison expression - these include comparing a string value to some constant string value.</span></span>  
  
     <span data-ttu-id="4e022-819">mother.familyName == "Smith";    child.givenName == s; s je proměnná řetězce</span><span class="sxs-lookup"><span data-stu-id="4e022-819">mother.familyName == "Smith";    child.givenName == s; //s is a string variable</span></span>
* <span data-ttu-id="4e022-820">Objekt nebo pole vytvoření výrazu - tyto výrazy návratový typ složené hodnoty nebo anonymní typ objekt nebo pole tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="4e022-820">Object/array creation expression - these expressions return an object of compound value type or anonymous type or an array of such objects.</span></span> <span data-ttu-id="4e022-821">Tyto hodnoty mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="4e022-821">These values can be nested.</span></span>
  
     <span data-ttu-id="4e022-822">novou nadřazenou položku {familyName = "Smith", givenName = "Jan"}; nové {nejprve = 1, druhý = 2}; anonymní typ s dvě pole</span><span class="sxs-lookup"><span data-stu-id="4e022-822">new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //an anonymous type with two fields</span></span>              
     <span data-ttu-id="4e022-823">New [] int {3, child.grade, 5};</span><span class="sxs-lookup"><span data-stu-id="4e022-823">new int[] { 3, child.grade, 5 };</span></span>

### <span data-ttu-id="4e022-824"><a id="SupportedLinqOperators"></a>Seznam podporovaných operátory LINQ</span><span class="sxs-lookup"><span data-stu-id="4e022-824"><a id="SupportedLinqOperators"></a>List of supported LINQ operators</span></span>
<span data-ttu-id="4e022-825">Tady je seznam podporovaných LINQ operátory ve zprostředkovateli LINQ součástí sadu DocumentDB .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="4e022-825">Here is a list of supported LINQ operators in the LINQ provider included with the DocumentDB .NET SDK.</span></span>

* <span data-ttu-id="4e022-826">**Vyberte**: projekce převede vyberte SQL, včetně vytváření objektů</span><span class="sxs-lookup"><span data-stu-id="4e022-826">**Select**: Projections translate to the SQL SELECT including object construction</span></span>
* <span data-ttu-id="4e022-827">**Kde**: filtry nepřeloží na SQL kde a podporovat překlad mezi & &, || a!</span><span class="sxs-lookup"><span data-stu-id="4e022-827">**Where**: Filters translate to the SQL WHERE, and support translation between && , || and !</span></span> <span data-ttu-id="4e022-828">SQL operátorů</span><span class="sxs-lookup"><span data-stu-id="4e022-828">to the SQL operators</span></span>
* <span data-ttu-id="4e022-829">**Označit více**: umožňuje unwinding polí pro klauzuli SQL JOIN.</span><span class="sxs-lookup"><span data-stu-id="4e022-829">**SelectMany**: Allows unwinding of arrays to the SQL JOIN clause.</span></span> <span data-ttu-id="4e022-830">Je možné řetězec nebo vnoření výrazy k filtrování v rámci prvků pole</span><span class="sxs-lookup"><span data-stu-id="4e022-830">Can be used to chain/nest expressions to filter on array elements</span></span>
* <span data-ttu-id="4e022-831">**OrderBy a OrderByDescending**: přeloží na order pořadí</span><span class="sxs-lookup"><span data-stu-id="4e022-831">**OrderBy and OrderByDescending**: Translates to ORDER BY ascending/descending</span></span>
* <span data-ttu-id="4e022-832">**Počet**, **součet**, **Min**, **maximální**, a **průměrná** operátory pro agregaci a jejich ekvivalenty asynchronní  **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, a **AverageAsync**.</span><span class="sxs-lookup"><span data-stu-id="4e022-832">**Count**, **Sum**, **Min**, **Max**, and **Average** operators for aggregation, and their async equivalents **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, and **AverageAsync**.</span></span>
* <span data-ttu-id="4e022-833">**CompareTo**: překládá do rozsahu porovnání.</span><span class="sxs-lookup"><span data-stu-id="4e022-833">**CompareTo**: Translates to range comparisons.</span></span> <span data-ttu-id="4e022-834">Běžně používané pro řetězce vzhledem k tomu, že nejste porovnatelný v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="4e022-834">Commonly used for strings since they’re not comparable in .NET</span></span>
* <span data-ttu-id="4e022-835">**Trvat**: překládá do horní části SQL pro omezení výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="4e022-835">**Take**: Translates to the SQL TOP for limiting results from a query</span></span>
* <span data-ttu-id="4e022-836">**Matematické funkce**: podporuje překlad z. Asin Abs Acos, je NET, Atan, Ceiling Cos, Exp, Floor, protokolu, Log10, Pow, kruhové, přihlášení, Sin, Sqrt, Tan, Truncate na ekvivalentní integrované funkce SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-836">**Math Functions**: Supports translation from .NET’s Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="4e022-837">**Funkce pro řetězce**: podporuje překlad z. NET na Concat, obsahuje, EndsWith, IndexOf, počet, ToLower, TrimStart –, nahraďte, zpětného, TrimEnd, StartsWith, SubString, ToUpper na ekvivalentní integrované funkce SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-837">**String Functions**: Supports translation from .NET’s Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="4e022-838">**Pole funkce**: podporuje překlad z. NET na Concat, obsahuje a počet, který má ekvivalentní integrované funkce SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-838">**Array Functions**: Supports translation from .NET’s Concat, Contains, and Count to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="4e022-839">**Funkce rozšíření geoprostorové**: podporuje překlad z metod se zakázaným inzerováním vzdálenost v rámci, IsValid a IsValidDetailed na ekvivalentní integrované funkce SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-839">**Geospatial Extension Functions**: Supports translation from stub methods Distance, Within, IsValid, and IsValidDetailed to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="4e022-840">**Uživatelem definované funkce rozšíření funkce**: podporuje překlad z metody se zakázaným inzerováním UserDefinedFunctionProvider.Invoke odpovídající uživatelem definované funkce.</span><span class="sxs-lookup"><span data-stu-id="4e022-840">**User-Defined Function Extension Function**: Supports translation from the stub method UserDefinedFunctionProvider.Invoke to the corresponding user-defined function.</span></span>
* <span data-ttu-id="4e022-841">**Různé**: podporuje překlad coalesce a podmíněné operátory.</span><span class="sxs-lookup"><span data-stu-id="4e022-841">**Miscellaneous**: Supports translation of the coalesce and conditional operators.</span></span> <span data-ttu-id="4e022-842">Může překládat obsahuje řetězec obsahuje, ARRAY_CONTAINS nebo v SQL v závislosti na kontextu.</span><span class="sxs-lookup"><span data-stu-id="4e022-842">Can translate Contains to String CONTAINS, ARRAY_CONTAINS, or the SQL IN depending on context.</span></span>

### <a name="sql-query-operators"></a><span data-ttu-id="4e022-843">Operátory dotazu SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-843">SQL query operators</span></span>
<span data-ttu-id="4e022-844">Zde jsou některé příklady, které ilustrují, jak některé standardní operátory dotazu LINQ přeložit dolů Cosmos DB dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-844">Here are some examples that illustrate how some of the standard LINQ query operators are translated down to Cosmos DB queries.</span></span>

#### <a name="select-operator"></a><span data-ttu-id="4e022-845">Select – operátor</span><span class="sxs-lookup"><span data-stu-id="4e022-845">Select Operator</span></span>
<span data-ttu-id="4e022-846">Syntaxe je `input.Select(x => f(x))`, kde `f` je skalární výraz.</span><span class="sxs-lookup"><span data-stu-id="4e022-846">The syntax is `input.Select(x => f(x))`, where `f` is a scalar expression.</span></span>

<span data-ttu-id="4e022-847">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-847">**LINQ lambda expression**</span></span>

    input.Select(family => family.parents[0].familyName);

<span data-ttu-id="4e022-848">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-848">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



<span data-ttu-id="4e022-849">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-849">**LINQ lambda expression**</span></span>

    input.Select(family => family.children[0].grade + c); // c is an int variable


<span data-ttu-id="4e022-850">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-850">**SQL**</span></span> 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



<span data-ttu-id="4e022-851">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-851">**LINQ lambda expression**</span></span>

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


<span data-ttu-id="4e022-852">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-852">**SQL**</span></span> 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a><span data-ttu-id="4e022-853">Označit více – operátor</span><span class="sxs-lookup"><span data-stu-id="4e022-853">SelectMany operator</span></span>
<span data-ttu-id="4e022-854">Syntaxe je `input.SelectMany(x => f(x))`, kde `f` se skalární výraz, který vrátí typ kolekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-854">The syntax is `input.SelectMany(x => f(x))`, where `f` is a scalar expression that returns a collection type.</span></span>

<span data-ttu-id="4e022-855">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-855">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children);

<span data-ttu-id="4e022-856">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-856">**SQL**</span></span> 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a><span data-ttu-id="4e022-857">Kde – operátor</span><span class="sxs-lookup"><span data-stu-id="4e022-857">Where operator</span></span>
<span data-ttu-id="4e022-858">Syntaxe je `input.Where(x => f(x))`, kde `f` je skalární výraz, který vrací logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e022-858">The syntax is `input.Where(x => f(x))`, where `f` is a scalar expression, which returns a Boolean value.</span></span>

<span data-ttu-id="4e022-859">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-859">**LINQ lambda expression**</span></span>

    input.Where(family=> family.parents[0].familyName == "Smith");

<span data-ttu-id="4e022-860">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-860">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



<span data-ttu-id="4e022-861">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-861">**LINQ lambda expression**</span></span>

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

<span data-ttu-id="4e022-862">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-862">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a><span data-ttu-id="4e022-863">Složené příkazy jazyka SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-863">Composite SQL queries</span></span>
<span data-ttu-id="4e022-864">Výše uvedené operátory musí být komponovaná k vytvoření více výkonných dotazů.</span><span class="sxs-lookup"><span data-stu-id="4e022-864">The above operators can be composed to form more powerful queries.</span></span> <span data-ttu-id="4e022-865">Vzhledem k tomu, že Cosmos DB podporuje vnořené kolekcí, složení můžete být zřetězen nebo vnořený.</span><span class="sxs-lookup"><span data-stu-id="4e022-865">Since Cosmos DB supports nested collections, the composition can either be concatenated or nested.</span></span>

#### <a name="concatenation"></a><span data-ttu-id="4e022-866">Zřetězení</span><span class="sxs-lookup"><span data-stu-id="4e022-866">Concatenation</span></span>
<span data-ttu-id="4e022-867">Syntaxe je `input(.|.SelectMany())(.Select()|.Where())*`.</span><span class="sxs-lookup"><span data-stu-id="4e022-867">The syntax is `input(.|.SelectMany())(.Select()|.Where())*`.</span></span> <span data-ttu-id="4e022-868">Zřetězené dotaz můžete spustit v volitelný `SelectMany` dotazu následuje více `Select` nebo `Where` operátory.</span><span class="sxs-lookup"><span data-stu-id="4e022-868">A concatenated query can start with an optional `SelectMany` query followed by multiple `Select` or `Where` operators.</span></span>

<span data-ttu-id="4e022-869">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-869">**LINQ lambda expression**</span></span>

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

<span data-ttu-id="4e022-870">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-870">**SQL**</span></span>

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



<span data-ttu-id="4e022-871">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-871">**LINQ lambda expression**</span></span>

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

<span data-ttu-id="4e022-872">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-872">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



<span data-ttu-id="4e022-873">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-873">**LINQ lambda expression**</span></span>

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

<span data-ttu-id="4e022-874">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-874">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



<span data-ttu-id="4e022-875">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-875">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

<span data-ttu-id="4e022-876">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-876">**SQL**</span></span> 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a><span data-ttu-id="4e022-877">Vnoření</span><span class="sxs-lookup"><span data-stu-id="4e022-877">Nesting</span></span>
<span data-ttu-id="4e022-878">Syntaxe je `input.SelectMany(x=>x.Q())` kde Q je `Select`, `SelectMany`, nebo `Where` operátor.</span><span class="sxs-lookup"><span data-stu-id="4e022-878">The syntax is `input.SelectMany(x=>x.Q())` where Q is a `Select`, `SelectMany`, or `Where` operator.</span></span>

<span data-ttu-id="4e022-879">Pro každý prvek vnější kolekce se v vnořený dotaz, použít vnitřní dotaz.</span><span class="sxs-lookup"><span data-stu-id="4e022-879">In a nested query, the inner query is applied to each element of the outer collection.</span></span> <span data-ttu-id="4e022-880">Jednou z důležitou součást je, že vnitřní dotaz mohou odkazovat na pole elementů v kolekci vnější jako spojení sama na sebe.</span><span class="sxs-lookup"><span data-stu-id="4e022-880">One important feature is that the inner query can refer to the fields of the elements in the outer collection like self-joins.</span></span>

<span data-ttu-id="4e022-881">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-881">**LINQ lambda expression**</span></span>

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

<span data-ttu-id="4e022-882">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-882">**SQL**</span></span> 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


<span data-ttu-id="4e022-883">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-883">**LINQ lambda expression**</span></span>

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

<span data-ttu-id="4e022-884">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-884">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



<span data-ttu-id="4e022-885">**Výraz lambda LINQ**</span><span class="sxs-lookup"><span data-stu-id="4e022-885">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

<span data-ttu-id="4e022-886">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4e022-886">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <span data-ttu-id="4e022-887"><a id="ExecutingSqlQueries"></a>Provádění dotazů SQL</span><span class="sxs-lookup"><span data-stu-id="4e022-887"><a id="ExecutingSqlQueries"></a>Executing SQL queries</span></span>
<span data-ttu-id="4e022-888">Cosmos DB zveřejňuje prostředky přes rozhraní REST API, kterou lze volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e022-888">Cosmos DB exposes resources through a REST API that can be called by any language capable of making HTTP/HTTPS requests.</span></span> <span data-ttu-id="4e022-889">Cosmos DB dále nabízí programovací knihovny pro několik oblíbených jazyků, jako je rozhraní .NET, Node.js, JavaScript a Python.</span><span class="sxs-lookup"><span data-stu-id="4e022-889">Additionally, Cosmos DB offers programming libraries for several popular languages like .NET, Node.js, JavaScript, and Python.</span></span> <span data-ttu-id="4e022-890">Rozhraní REST API různých knihoven podporují a dotazování pomocí SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-890">The REST API and the various libraries all support querying through SQL.</span></span> <span data-ttu-id="4e022-891">.NET SDK podporuje LINQ dotazování kromě SQL.</span><span class="sxs-lookup"><span data-stu-id="4e022-891">The .NET SDK supports LINQ querying in addition to SQL.</span></span>

<span data-ttu-id="4e022-892">Následující příklady ukazují, jak vytvořit dotaz a odešlete ji proti Cosmos DB databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="4e022-892">The following examples show how to create a query and submit it against a Cosmos DB database account.</span></span>

### <span data-ttu-id="4e022-893"><a id="RestAPI"></a>ROZHRANÍ REST API</span><span class="sxs-lookup"><span data-stu-id="4e022-893"><a id="RestAPI"></a>REST API</span></span>
<span data-ttu-id="4e022-894">Cosmos DB nabízí otevřete RESTful programovací model přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e022-894">Cosmos DB offers an open RESTful programming model over HTTP.</span></span> <span data-ttu-id="4e022-895">Databáze účtů se dá zřídit pomocí předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="4e022-895">Database accounts can be provisioned using an Azure subscription.</span></span> <span data-ttu-id="4e022-896">Model prostředků Cosmos DB obsahuje sadu prostředků v rámci účtu databáze, z nichž každý je adresovatelné logické a stabilní identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="4e022-896">The Cosmos DB resource model consists of a set of resources under a database account, each  of which is addressable using a logical and stable URI.</span></span> <span data-ttu-id="4e022-897">Sadu prostředků se označuje jako informačního kanálu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-897">A set of resources is referred to as a feed in this document.</span></span> <span data-ttu-id="4e022-898">Databázový účet se skládá ze sady databází, každá obsahuje několik kolekcí, každý z které naopak obsahovat dokumenty, funkce UDF a další typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="4e022-898">A database account consists of a set of databases, each containing multiple collections, each of which in-turn contain documents, UDFs, and other resource types.</span></span>

<span data-ttu-id="4e022-899">Základní interakce model pomocí těchto prostředků je pomocí příkazů HTTP GET, PUT, POST a odstranit pomocí jejich standardní překladu.</span><span class="sxs-lookup"><span data-stu-id="4e022-899">The basic interaction model with these resources is through the HTTP verbs GET, PUT, POST, and DELETE with their standard interpretation.</span></span> <span data-ttu-id="4e022-900">Příkaz POST se používá pro vytvoření nového prostředku, pro spuštění uložené procedury nebo pro zadání dotazu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e022-900">The POST verb is used for creation of a new resource, for executing a stored procedure or for issuing a Cosmos DB query.</span></span> <span data-ttu-id="4e022-901">Dotazy jsou vždy jen pro čtení operací s žádné vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="4e022-901">Queries are always read-only operations with no side-effects.</span></span>

<span data-ttu-id="4e022-902">Následující příklady ukazují POST pro rozhraní API DocumentDB dotaz směřovaný na kolekce obsahující dva ukázkové dokumenty, že jsme si přečetli dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="4e022-902">The following examples show a POST for a DocumentDB API query made against a collection containing the two sample documents we've reviewed so far.</span></span> <span data-ttu-id="4e022-903">Dotaz je jednoduchý filtr na název vlastnosti JSON.</span><span class="sxs-lookup"><span data-stu-id="4e022-903">The query has a simple filter on the JSON name property.</span></span> <span data-ttu-id="4e022-904">Všimněte si použití `x-ms-documentdb-isquery` a Content-Type: `application/query+json` hlavičky k označení, že operace je dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-904">Note the use of the `x-ms-documentdb-isquery` and Content-Type: `application/query+json` headers to denote that the operation is a query.</span></span>

<span data-ttu-id="4e022-905">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4e022-905">**Request**</span></span>

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


<span data-ttu-id="4e022-906">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-906">**Results**</span></span>

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


<span data-ttu-id="4e022-907">Druhý příklad ukazuje komplexnější dotaz, který vrátí více výsledků z spojení.</span><span class="sxs-lookup"><span data-stu-id="4e022-907">The second example shows a more complex query that returns multiple results from the join.</span></span>

<span data-ttu-id="4e022-908">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4e022-908">**Request**</span></span>

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


<span data-ttu-id="4e022-909">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="4e022-909">**Results**</span></span>

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


<span data-ttu-id="4e022-910">Pokud výsledku dotazu se nemůže vejít do jedné stránky s výsledky, pak rozhraní REST API vrátí token pokračování prostřednictvím `x-ms-continuation-token` hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4e022-910">If a query's results cannot fit within a single page of results, then the REST API returns a continuation token through the `x-ms-continuation-token` response header.</span></span> <span data-ttu-id="4e022-911">Klienty můžete stránkování výsledků zahrnutím záhlaví v následných výsledky.</span><span class="sxs-lookup"><span data-stu-id="4e022-911">Clients can paginate results by including the header in subsequent results.</span></span> <span data-ttu-id="4e022-912">Počet výsledků na stránce lze také řídit prostřednictvím `x-ms-max-item-count` číslo záhlaví.</span><span class="sxs-lookup"><span data-stu-id="4e022-912">The number of results per page can also be controlled through the `x-ms-max-item-count` number header.</span></span> <span data-ttu-id="4e022-913">Pokud zadaný dotaz obsahuje agregační funkci jako `COUNT`, pak stránce dotaz může vrátit hodnotu částečně agregované přes stránky s výsledky.</span><span class="sxs-lookup"><span data-stu-id="4e022-913">If the specified query has an aggregation function like `COUNT`, then the query page may return a partially aggregated value over the page of results.</span></span> <span data-ttu-id="4e022-914">Klienti musí provést druhé úrovně agregace nad těmito výsledky. Chcete-li vytvořit konečných výsledků, například, součet přes počty vrátil na jednotlivých stránkách vrátit celkového počtu.</span><span class="sxs-lookup"><span data-stu-id="4e022-914">The clients must perform a second-level aggregation over these results to produce the final results, for example, sum over the counts returned in the individual pages to return the total count.</span></span>

<span data-ttu-id="4e022-915">Ke správě zásad konzistence dat pro dotazy, použijte `x-ms-consistency-level` záhlaví jako všechny požadavky REST API.</span><span class="sxs-lookup"><span data-stu-id="4e022-915">To manage the data consistency policy for queries, use the `x-ms-consistency-level` header like all REST API requests.</span></span> <span data-ttu-id="4e022-916">Konzistence typu relace, je potřeba také odezvu na nejnovější `x-ms-session-token` hlavička Cookie v dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="4e022-916">For session consistency, it is required to also echo the latest `x-ms-session-token` Cookie header in the query request.</span></span> <span data-ttu-id="4e022-917">Zásady indexování dotazované kolekce můžete ovlivnit taky konzistence výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-917">The queried collection's indexing policy can also influence the consistency of query results.</span></span> <span data-ttu-id="4e022-918">S výchozí nastavení zásady indexování, pro kolekce index je vždy aktuální pomocí obsahu dokumentu a výsledky dotazu odpovídat konzistence zvolené pro data.</span><span class="sxs-lookup"><span data-stu-id="4e022-918">With the default indexing policy settings, for collections the index is always current with the document contents and query results match the consistency chosen for data.</span></span> <span data-ttu-id="4e022-919">Pokud k Lazy je zmírnit zásady indexování, dotazy mohou vracet zastaralé výsledky.</span><span class="sxs-lookup"><span data-stu-id="4e022-919">If the indexing policy is relaxed to Lazy, then queries can return stale results.</span></span> <span data-ttu-id="4e022-920">Další informace najdete v tématu [úrovně konzistence databáze Azure Cosmos][consistency-levels].</span><span class="sxs-lookup"><span data-stu-id="4e022-920">For more information, see [Azure Cosmos DB Consistency Levels][consistency-levels].</span></span>

<span data-ttu-id="4e022-921">Pokud nakonfigurované zásady indexování na kolekce nepodporuje zadaný dotaz, vrátí server Azure Cosmos DB 400 "Chybný požadavek".</span><span class="sxs-lookup"><span data-stu-id="4e022-921">If the configured indexing policy on the collection cannot support the specified query, the Azure Cosmos DB server returns 400 "Bad Request".</span></span> <span data-ttu-id="4e022-922">Se vrátí pro dotazy na rozsah pro cesty, které jsou nakonfigurované pro vyhledávání hodnoty hash (rovnosti) a cesty explicitně vyloučená z indexování.</span><span class="sxs-lookup"><span data-stu-id="4e022-922">This is returned for range queries against paths configured for hash (equality) lookups, and for paths explicitly excluded from indexing.</span></span> <span data-ttu-id="4e022-923">`x-ms-documentdb-query-enable-scan` Záhlaví lze povolit dotazu, který chcete provést kontrolu, když indexu není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4e022-923">The `x-ms-documentdb-query-enable-scan` header can be specified to allow the query to perform a scan when an index is not available.</span></span>

<span data-ttu-id="4e022-924">Podrobné metriky můžete získat na spuštění dotazu nastavením `x-ms-documentdb-populatequerymetrics` hlavičky k `True`.</span><span class="sxs-lookup"><span data-stu-id="4e022-924">You can get detailed metrics on query execution by setting `x-ms-documentdb-populatequerymetrics` header to `True`.</span></span> <span data-ttu-id="4e022-925">Další informace najdete v tématu [metriky dotazů SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="4e022-925">For more information, see [SQL query metrics for Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span></span>

### <span data-ttu-id="4e022-926"><a id="DotNetSdk"></a>SADA SDK JAZYKA C# (.NET)</span><span class="sxs-lookup"><span data-stu-id="4e022-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span></span>
<span data-ttu-id="4e022-927">.NET SDK podporuje LINQ a SQL dotazování.</span><span class="sxs-lookup"><span data-stu-id="4e022-927">The .NET SDK supports both LINQ and SQL querying.</span></span> <span data-ttu-id="4e022-928">Následující příklad ukazuje, jak k provedení dotazu jednoduchý filtr zavedená dříve v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4e022-928">The following example shows how to perform the simple filter query introduced earlier in this document.</span></span>

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


<span data-ttu-id="4e022-929">Tato ukázka porovná dvě vlastnosti rovnosti v rámci každého dokumentu a používá anonymní projekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-929">This sample compares two properties for equality within each document and uses anonymous projections.</span></span> 

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


<span data-ttu-id="4e022-930">Další příklad ukazuje spojení, vyjádřit pomocí LINQ označit více.</span><span class="sxs-lookup"><span data-stu-id="4e022-930">The next sample shows joins, expressed through LINQ SelectMany.</span></span>

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



<span data-ttu-id="4e022-931">Klient .NET automaticky iteruje všechny stránky výsledků dotazu v blocích foreach, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="4e022-931">The .NET client automatically iterates through all the pages of query results in the foreach blocks as shown above.</span></span> <span data-ttu-id="4e022-932">Možnosti dotazu byla zavedená v části REST API jsou také k dispozici v pomocí .NET SDK `FeedOptions` a `FeedResponse` třídy v metodě CreateDocumentQuery.</span><span class="sxs-lookup"><span data-stu-id="4e022-932">The query options introduced in the REST API section are also available in the .NET SDK using the `FeedOptions` and `FeedResponse` classes in the CreateDocumentQuery method.</span></span> <span data-ttu-id="4e022-933">Počet stránek se dá řídit pomocí `MaxItemCount` nastavení.</span><span class="sxs-lookup"><span data-stu-id="4e022-933">The number of pages can be controlled using the `MaxItemCount` setting.</span></span> 

<span data-ttu-id="4e022-934">Můžete také explicitně řídit stránkování tak, že vytvoříte `IDocumentQueryable` pomocí `IQueryable` objekt, pak načtením` ResponseContinuationToken` hodnot a jejich předání zpět jako `RequestContinuationToken` v `FeedOptions`.</span><span class="sxs-lookup"><span data-stu-id="4e022-934">You can also explicitly control paging by creating `IDocumentQueryable` using the `IQueryable` object, then by reading the` ResponseContinuationToken` values and passing them back as `RequestContinuationToken` in `FeedOptions`.</span></span> <span data-ttu-id="4e022-935">`EnableScanInQuery`lze nastavit pro povolení kontroly, pokud dotaz nemůže být podporována nakonfigurované zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="4e022-935">`EnableScanInQuery` can be set to enable scans when the query cannot be supported by the configured indexing policy.</span></span> <span data-ttu-id="4e022-936">Pro dělené kolekce, můžete použít `PartitionKey` ke spouštění dotazu na jednoho oddílu (i když Cosmos DB může automaticky extrahovat to z text dotazu), a `EnableCrossPartitionQuery` ke spouštění dotazů, které může být nutné ke spuštění s více oddílů.</span><span class="sxs-lookup"><span data-stu-id="4e022-936">For partitioned collections, you can use `PartitionKey` to run the query against a single partition (though Cosmos DB can automatically extract this from the query text), and `EnableCrossPartitionQuery` to run queries that may need to be run against multiple partitions.</span></span> 

<span data-ttu-id="4e022-937">Odkazovat na [Azure Cosmos DB .NET ukázky](https://github.com/Azure/azure-documentdb-net) pro další ukázky obsahující dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e022-937">Refer to [Azure Cosmos DB .NET samples](https://github.com/Azure/azure-documentdb-net) for more samples containing queries.</span></span> 

### <span data-ttu-id="4e022-938"><a id="JavaScriptServerSideApi"></a>Rozhraní API jazyka JavaScript na straně serveru</span><span class="sxs-lookup"><span data-stu-id="4e022-938"><a id="JavaScriptServerSideApi"></a>JavaScript server-side API</span></span>
<span data-ttu-id="4e022-939">Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na kolekce pomocí uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="4e022-939">Cosmos DB provides a programming model for executing JavaScript based application logic directly on the collections using stored procedures and triggers.</span></span> <span data-ttu-id="4e022-940">JavaScript logiku registrované na úrovni kolekce potom můžou provádět databázové operace na operace s dokumenty dané kolekce.</span><span class="sxs-lookup"><span data-stu-id="4e022-940">The JavaScript logic registered at a collection level can then issue database operations on the operations on the documents of the given collection.</span></span> <span data-ttu-id="4e022-941">Tyto operace jsou zabalená vedlejším ACID transakcí.</span><span class="sxs-lookup"><span data-stu-id="4e022-941">These operations are wrapped in ambient ACID transactions.</span></span>

<span data-ttu-id="4e022-942">Následující příklad ukazuje, jak lze pomocí dokumenty dotazu v rozhraní API serveru JavaScript na dotazy z uvnitř uložené procedury a triggery.</span><span class="sxs-lookup"><span data-stu-id="4e022-942">The following example shows how to use the queryDocuments in the JavaScript server API to make queries from inside stored procedures and triggers.</span></span>

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

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <span data-ttu-id="4e022-943"><a id="References"></a>Odkazy</span><span class="sxs-lookup"><span data-stu-id="4e022-943"><a id="References"></a>References</span></span>
1. <span data-ttu-id="4e022-944">[Úvod do Azure Cosmos DB][introduction]</span><span class="sxs-lookup"><span data-stu-id="4e022-944">[Introduction to Azure Cosmos DB][introduction]</span></span>
2. [<span data-ttu-id="4e022-945">Azure Cosmos DB SQL specifikace</span><span class="sxs-lookup"><span data-stu-id="4e022-945">Azure Cosmos DB SQL specification</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [<span data-ttu-id="4e022-946">Ukázek Azure DB Cosmos rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="4e022-946">Azure Cosmos DB .NET samples</span></span>](https://github.com/Azure/azure-documentdb-net)
4. <span data-ttu-id="4e022-947">[Úrovně konzistence databáze Azure Cosmos][consistency-levels]</span><span class="sxs-lookup"><span data-stu-id="4e022-947">[Azure Cosmos DB Consistency Levels][consistency-levels]</span></span>
5. <span data-ttu-id="4e022-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span><span class="sxs-lookup"><span data-stu-id="4e022-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span></span>
6. <span data-ttu-id="4e022-949">JSON [http://json.org/](http://json.org/)</span><span class="sxs-lookup"><span data-stu-id="4e022-949">JSON [http://json.org/](http://json.org/)</span></span>
7. <span data-ttu-id="4e022-950">Specifikace jazyka JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span><span class="sxs-lookup"><span data-stu-id="4e022-950">Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span></span> 
8. <span data-ttu-id="4e022-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span><span class="sxs-lookup"><span data-stu-id="4e022-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span></span> 
9. <span data-ttu-id="4e022-952">Dotaz na vyhodnocení techniky pro velké databáze [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span><span class="sxs-lookup"><span data-stu-id="4e022-952">Query evaluation techniques for large databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span></span>
10. <span data-ttu-id="4e022-953">Zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů</span><span class="sxs-lookup"><span data-stu-id="4e022-953">Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994</span></span>
11. <span data-ttu-id="4e022-954">Logická jednotka, Ooi, Tan, zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů.</span><span class="sxs-lookup"><span data-stu-id="4e022-954">Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.</span></span>
12. <span data-ttu-id="4e022-955">Olston Kryštof, Robert Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: není tak cizí jazyk pro zpracování dat, SIGMOD 2008.</span><span class="sxs-lookup"><span data-stu-id="4e022-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.</span></span>
13. <span data-ttu-id="4e022-956">G.</span><span class="sxs-lookup"><span data-stu-id="4e022-956">G.</span></span> <span data-ttu-id="4e022-957">Graefe.</span><span class="sxs-lookup"><span data-stu-id="4e022-957">Graefe.</span></span> <span data-ttu-id="4e022-958">Cascades architektura pro optimalizaci dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e022-958">The Cascades framework for query optimization.</span></span> <span data-ttu-id="4e022-959">Eng. IEEE dat</span><span class="sxs-lookup"><span data-stu-id="4e022-959">IEEE Data Eng.</span></span> <span data-ttu-id="4e022-960">Bull., 18(3): 1995.</span><span class="sxs-lookup"><span data-stu-id="4e022-960">Bull., 18(3): 1995.</span></span>

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md