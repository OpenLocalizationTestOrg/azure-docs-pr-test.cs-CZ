---
title: aaaHow tooquery s SQL v Azure Cosmos DB? | Dokumentace Microsoftu
description: "Další informace tooquery pomocí dat DocumentDB pomocí SQL v Azure Cosmos DB"
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
ms.openlocfilehash: d3dc51acf92cb78d4f4d9dbac7ec54b1382431cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a><span data-ttu-id="f9212-104">Azure Cosmos DB: Jak tooquery pomocí SQL?</span><span class="sxs-lookup"><span data-stu-id="f9212-104">Azure Cosmos DB: How tooquery using SQL?</span></span>

<span data-ttu-id="f9212-105">Hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) podporuje dotazování dokumentů pomocí SQL.</span><span class="sxs-lookup"><span data-stu-id="f9212-105">hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="f9212-106">Tento článek obsahuje ukázkové dokumentu a dva ukázkové dotazy SQL a výsledky.</span><span class="sxs-lookup"><span data-stu-id="f9212-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="f9212-107">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f9212-107">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="f9212-108">Dotazování na data pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="f9212-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="f9212-109">Ukázka dokumentu</span><span class="sxs-lookup"><span data-stu-id="f9212-109">Sample document</span></span>

<span data-ttu-id="f9212-110">dotazy SQL Hello v tomto článku použít hello následující ukázka dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f9212-110">hello SQL queries in this article use hello following sample document.</span></span>

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
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="f9212-111">Kde je můžete spouštět dotazy SQL?</span><span class="sxs-lookup"><span data-stu-id="f9212-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="f9212-112">Můžete spouštět dotazy pomocí hello Průzkumníku dat v hello portál Azure, prostřednictvím hello [REST API a sadám SDK](documentdb-sdk-dotnet.md)a to i v hello [Query playground](https://www.documentdb.com/sql/demo), která se spouští dotazy na existující sady ukázková data.</span><span class="sxs-lookup"><span data-stu-id="f9212-112">You can run queries using hello Data Explorer in hello Azure portal, via hello [REST API and SDKs](documentdb-sdk-dotnet.md), and even hello [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="f9212-113">Další informace o dotazech SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f9212-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="f9212-114">Dotaz SQL a syntaxe SQL</span><span class="sxs-lookup"><span data-stu-id="f9212-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="f9212-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f9212-115">Prerequisites</span></span>

<span data-ttu-id="f9212-116">Tento kurz předpokládá, že máte účet Azure Cosmos databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="f9212-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="f9212-117">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="f9212-117">Don't have any of those?</span></span> <span data-ttu-id="f9212-118">Dokončení hello [rychlý start 5 minut](create-mongodb-nodejs.md) nebo hello [vývojáře kurzu](tutorial-develop-mongodb.md) toocreate účet a kolekce.</span><span class="sxs-lookup"><span data-stu-id="f9212-118">Complete hello [5-minute quickstart](create-mongodb-nodejs.md) or hello [developer tutorial](tutorial-develop-mongodb.md) toocreate an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="f9212-119">Příklad dotazu 1</span><span class="sxs-lookup"><span data-stu-id="f9212-119">Example query 1</span></span>

<span data-ttu-id="f9212-120">Zadané hello ukázka rodiny dokumentu výše, následující dotaz SQL vrátí hello dokumenty kde pole id hello odpovídá `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="f9212-120">Given hello sample family document above, following SQL query returns hello documents where hello id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="f9212-121">Vzhledem k tomu, že je `SELECT *` příkaz hello výstup hello dotazu je hello dokončení dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="f9212-121">Since it's a `SELECT *` statement, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="f9212-122">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f9212-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="f9212-123">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f9212-123">**Results**</span></span>

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

## <a name="example-query-2"></a><span data-ttu-id="f9212-124">Příklad dotazu 2</span><span class="sxs-lookup"><span data-stu-id="f9212-124">Example query 2</span></span>

<span data-ttu-id="f9212-125">Hello další dotaz vrátí všechny názvy daným hello podřízených prvků řady hello shoduje s id `WakefieldFamily` seřazené podle jejich úrovni.</span><span class="sxs-lookup"><span data-stu-id="f9212-125">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="f9212-126">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="f9212-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="f9212-127">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="f9212-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="f9212-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9212-128">Next steps</span></span>

<span data-ttu-id="f9212-129">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="f9212-129">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f9212-130">Jak se naučili tooquery pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="f9212-130">Learned how tooquery using SQL</span></span>  

<span data-ttu-id="f9212-131">Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.</span><span class="sxs-lookup"><span data-stu-id="f9212-131">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f9212-132">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="f9212-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

