---
title: "Postup dotazování pomocí SQL v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Postup dotazování pomocí dat DocumentDB pomocí SQL v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a2a562c06c6302b9548e758b4c6754ec13b6001d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a><span data-ttu-id="b5b54-104">Azure Cosmos DB: Jak dotazovat pomocí SQL?</span><span class="sxs-lookup"><span data-stu-id="b5b54-104">Azure Cosmos DB: How to query using SQL?</span></span>

<span data-ttu-id="b5b54-105">Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) podporuje dotazování dokumentů pomocí SQL.</span><span class="sxs-lookup"><span data-stu-id="b5b54-105">The Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="b5b54-106">Tento článek obsahuje ukázkové dokumentu a dva ukázkové dotazy SQL a výsledky.</span><span class="sxs-lookup"><span data-stu-id="b5b54-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="b5b54-107">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="b5b54-107">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="b5b54-108">Dotazování na data pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="b5b54-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="b5b54-109">Ukázka dokumentu</span><span class="sxs-lookup"><span data-stu-id="b5b54-109">Sample document</span></span>

<span data-ttu-id="b5b54-110">Dotazy SQL v tomto článku použít následující ukázka dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b5b54-110">The SQL queries in this article use the following sample document.</span></span>

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
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="b5b54-111">Kde je můžete spouštět dotazy SQL?</span><span class="sxs-lookup"><span data-stu-id="b5b54-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="b5b54-112">Můžete spouštět dotazy pomocí Průzkumníku dat na portálu Azure pomocí [REST API a sadám SDK,](documentdb-sdk-dotnet.md)a to i v [Query playground](https://www.documentdb.com/sql/demo), která se spouští dotazy na existující sady ukázková data.</span><span class="sxs-lookup"><span data-stu-id="b5b54-112">You can run queries using the Data Explorer in the Azure portal, via the [REST API and SDKs](documentdb-sdk-dotnet.md), and even the [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="b5b54-113">Další informace o dotazech SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="b5b54-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="b5b54-114">Dotaz SQL a syntaxe SQL</span><span class="sxs-lookup"><span data-stu-id="b5b54-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="b5b54-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5b54-115">Prerequisites</span></span>

<span data-ttu-id="b5b54-116">Tento kurz předpokládá, že máte účet Azure Cosmos databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="b5b54-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="b5b54-117">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="b5b54-117">Don't have any of those?</span></span> <span data-ttu-id="b5b54-118">Dokončení [rychlý start 5 minut](create-mongodb-nodejs.md) nebo [vývojáře kurzu](tutorial-develop-mongodb.md) vytvoření účtu a kolekce.</span><span class="sxs-lookup"><span data-stu-id="b5b54-118">Complete the [5-minute quickstart](create-mongodb-nodejs.md) or the [developer tutorial](tutorial-develop-mongodb.md) to create an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="b5b54-119">Příklad dotazu 1</span><span class="sxs-lookup"><span data-stu-id="b5b54-119">Example query 1</span></span>

<span data-ttu-id="b5b54-120">Zadaný vzorek rodiny dokumentu výše, následující dotaz SQL vrátí dokumenty kde pole id odpovídá `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="b5b54-120">Given the sample family document above, following SQL query returns the documents where the id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="b5b54-121">Vzhledem k tomu, že je `SELECT *` příkaz výstup tohoto dotazu je kompletní dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="b5b54-121">Since it's a `SELECT *` statement, the output of the query is the complete JSON document:</span></span>

<span data-ttu-id="b5b54-122">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="b5b54-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="b5b54-123">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="b5b54-123">**Results**</span></span>

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

## <a name="example-query-2"></a><span data-ttu-id="b5b54-124">Příklad dotazu 2</span><span class="sxs-lookup"><span data-stu-id="b5b54-124">Example query 2</span></span>

<span data-ttu-id="b5b54-125">Další dotaz vrátí všechny názvy daným podřízených prvků v dané rodině, jehož id odpovídá `WakefieldFamily` seřazené podle jejich úrovni.</span><span class="sxs-lookup"><span data-stu-id="b5b54-125">The next query returns all the given names of children in the family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="b5b54-126">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="b5b54-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="b5b54-127">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="b5b54-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="b5b54-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5b54-128">Next steps</span></span>

<span data-ttu-id="b5b54-129">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="b5b54-129">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5b54-130">Zjistili, jak k dotazování pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="b5b54-130">Learned how to query using SQL</span></span>  

<span data-ttu-id="b5b54-131">Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.</span><span class="sxs-lookup"><span data-stu-id="b5b54-131">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b5b54-132">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="b5b54-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

