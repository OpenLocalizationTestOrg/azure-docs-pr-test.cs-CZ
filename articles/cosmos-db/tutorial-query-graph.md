---
title: "Postup dotazování dat grafu v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Další informace k dotazování na data grafu v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 81713c72da037f127e81239d214d7a877247dca1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-how-to-query-with-the-graph-api-preview"></a><span data-ttu-id="749c6-104">Azure Cosmos DB: Jak dotazovat pomocí rozhraní Graph API (preview)?</span><span class="sxs-lookup"><span data-stu-id="749c6-104">Azure Cosmos DB: How to query with the Graph API (preview)?</span></span>

<span data-ttu-id="749c6-105">Azure Cosmos DB [rozhraní Graph API](graph-introduction.md) (preview) podporuje [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) dotazy.</span><span class="sxs-lookup"><span data-stu-id="749c6-105">The Azure Cosmos DB [Graph API](graph-introduction.md) (preview) supports [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) queries.</span></span> <span data-ttu-id="749c6-106">Tento článek obsahuje ukázkové dokumentech a dotazech, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="749c6-106">This article provides sample documents and queries to get you started.</span></span> <span data-ttu-id="749c6-107">A podrobné Gremlin je součástí odkaz [Gremlin podporu](gremlin-support.md) článku.</span><span class="sxs-lookup"><span data-stu-id="749c6-107">A detailed Gremlin reference is provided in the [Gremlin support](gremlin-support.md) article.</span></span>

<span data-ttu-id="749c6-108">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="749c6-108">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="749c6-109">Dotazování na data s Gremlin</span><span class="sxs-lookup"><span data-stu-id="749c6-109">Querying data with Gremlin</span></span>

## <a name="prerequisites"></a><span data-ttu-id="749c6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="749c6-110">Prerequisites</span></span>

<span data-ttu-id="749c6-111">Pro tyto dotazy pro práci musí mít účet Azure Cosmos DB a mít dat grafu v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="749c6-111">For these queries to work, you must have an Azure Cosmos DB account and have graph data in the container.</span></span> <span data-ttu-id="749c6-112">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="749c6-112">Don't have any of those?</span></span> <span data-ttu-id="749c6-113">Dokončení [rychlý start 5 minut](create-graph-dotnet.md) nebo [vývojáře kurzu](tutorial-query-graph.md) k vytvoření účtu a naplnit databázi.</span><span class="sxs-lookup"><span data-stu-id="749c6-113">Complete the [5-minute quickstart](create-graph-dotnet.md) or the [developer tutorial](tutorial-query-graph.md) to create an account and populate your database.</span></span> <span data-ttu-id="749c6-114">Můžete spustit následující dotazy pomocí [knihovny Azure Cosmos DB .NET grafu](graph-sdk-dotnet.md), [Gremlin konzoly](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), nebo vaše oblíbené Gremlin ovladač.</span><span class="sxs-lookup"><span data-stu-id="749c6-114">You can run the following queries using the [Azure Cosmos DB .NET graph library](graph-sdk-dotnet.md), [Gremlin console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), or your favorite Gremlin driver.</span></span>

## <a name="count-vertices-in-the-graph"></a><span data-ttu-id="749c6-115">Počet vrcholy v grafu</span><span class="sxs-lookup"><span data-stu-id="749c6-115">Count vertices in the graph</span></span>

<span data-ttu-id="749c6-116">Následující fragment kódu ukazuje, jak můžete zjistit, kolik vrcholy v grafu:</span><span class="sxs-lookup"><span data-stu-id="749c6-116">The following snippet shows how to count the number of vertices in the graph:</span></span>

```
g.V().count()
```

## <a name="filters"></a><span data-ttu-id="749c6-117">Filtry</span><span class="sxs-lookup"><span data-stu-id="749c6-117">Filters</span></span>

<span data-ttu-id="749c6-118">Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` k vytvoření složitějších filtrů.</span><span class="sxs-lookup"><span data-stu-id="749c6-118">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` to build more complex filters.</span></span> <span data-ttu-id="749c6-119">Azure Cosmos DB poskytuje, bez ohledu na schéma indexu všech vlastností v rámci vrcholy a stupňů pro rychlé dotazy:</span><span class="sxs-lookup"><span data-stu-id="749c6-119">Azure Cosmos DB provides schema-agnostic indexing of all properties within your vertices and degrees for fast queries:</span></span>

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a><span data-ttu-id="749c6-120">Projekce</span><span class="sxs-lookup"><span data-stu-id="749c6-120">Projection</span></span>

<span data-ttu-id="749c6-121">Můžete promítnout některé vlastnosti ve výsledcích dotazu pomocí `values` kroku:</span><span class="sxs-lookup"><span data-stu-id="749c6-121">You can project certain properties in the query results using the `values` step:</span></span>

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a><span data-ttu-id="749c6-122">Vyhledání souvisejících okraje a vrcholy</span><span class="sxs-lookup"><span data-stu-id="749c6-122">Find related edges and vertices</span></span>

<span data-ttu-id="749c6-123">Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází.</span><span class="sxs-lookup"><span data-stu-id="749c6-123">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="749c6-124">Grafy jsou rychlé a efektivní pro operace traversal, když potřebujete přejít na související okraje a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="749c6-124">Graphs are fast and efficient for traversal operations when you need to navigate to related edges and vertices.</span></span> <span data-ttu-id="749c6-125">Umožňuje najít všechny přátelích Thomas.</span><span class="sxs-lookup"><span data-stu-id="749c6-125">Let's find all friends of Thomas.</span></span> <span data-ttu-id="749c6-126">Provedeme to pomocí na Gremlin `outE` krok najdete všechny odesílací okrajů z Thomas pak procházení k v vrcholy z těchto hran pomocí Gremlin na `inV` krok:</span><span class="sxs-lookup"><span data-stu-id="749c6-126">We do this by using Gremlin's `outE` step to find all the out-edges from Thomas, then traversing to the in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="749c6-127">Další dotaz provádí dvěma segmenty směrování k vyhledání všech Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát.</span><span class="sxs-lookup"><span data-stu-id="749c6-127">The next query performs two hops to find all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="749c6-128">Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně kombinování filtru výrazů, provádění opakování pomocí `loop` kroku a implementuje pomocí podmíněného navigace `choose` krok.</span><span class="sxs-lookup"><span data-stu-id="749c6-128">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using the `loop` step, and implementing conditional navigation using the `choose` step.</span></span> <span data-ttu-id="749c6-129">Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="749c6-129">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

## <a name="next-steps"></a><span data-ttu-id="749c6-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="749c6-130">Next steps</span></span>

<span data-ttu-id="749c6-131">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="749c6-131">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="749c6-132">Zjistili, jak k dotazování pomocí grafu</span><span class="sxs-lookup"><span data-stu-id="749c6-132">Learned how to query using Graph</span></span> 

<span data-ttu-id="749c6-133">Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.</span><span class="sxs-lookup"><span data-stu-id="749c6-133">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="749c6-134">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="749c6-134">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)