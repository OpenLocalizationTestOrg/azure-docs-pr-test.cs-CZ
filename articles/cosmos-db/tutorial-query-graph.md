---
title: data grafu aaaHow tooquery v Azure Cosmos DB? | Dokumentace Microsoftu
description: "Další data grafu tooquery v Azure Cosmos DB"
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
ms.openlocfilehash: fdde881edd6c488e2fea51e5c9665e1d736009fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a><span data-ttu-id="79838-104">Azure Cosmos DB: Jak tooquery s hello rozhraní Graph API (preview)?</span><span class="sxs-lookup"><span data-stu-id="79838-104">Azure Cosmos DB: How tooquery with hello Graph API (preview)?</span></span>

<span data-ttu-id="79838-105">Hello Azure Cosmos DB [rozhraní Graph API](graph-introduction.md) (preview) podporuje [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) dotazy.</span><span class="sxs-lookup"><span data-stu-id="79838-105">hello Azure Cosmos DB [Graph API](graph-introduction.md) (preview) supports [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) queries.</span></span> <span data-ttu-id="79838-106">Tento článek obsahuje ukázkové dokumenty a dotazuje tooget, kterou jste zahájili.</span><span class="sxs-lookup"><span data-stu-id="79838-106">This article provides sample documents and queries tooget you started.</span></span> <span data-ttu-id="79838-107">Podrobné referenční Gremlin je součástí hello [Gremlin podporu](gremlin-support.md) článku.</span><span class="sxs-lookup"><span data-stu-id="79838-107">A detailed Gremlin reference is provided in hello [Gremlin support](gremlin-support.md) article.</span></span>

<span data-ttu-id="79838-108">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="79838-108">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="79838-109">Dotazování na data s Gremlin</span><span class="sxs-lookup"><span data-stu-id="79838-109">Querying data with Gremlin</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79838-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79838-110">Prerequisites</span></span>

<span data-ttu-id="79838-111">Pro tyto dotazy toowork musíte mít účet Azure Cosmos DB a mít dat grafu v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="79838-111">For these queries toowork, you must have an Azure Cosmos DB account and have graph data in hello container.</span></span> <span data-ttu-id="79838-112">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="79838-112">Don't have any of those?</span></span> <span data-ttu-id="79838-113">Dokončení hello [rychlý start 5 minut](create-graph-dotnet.md) nebo hello [vývojáře kurzu](tutorial-query-graph.md) toocreate účet a naplnit databázi.</span><span class="sxs-lookup"><span data-stu-id="79838-113">Complete hello [5-minute quickstart](create-graph-dotnet.md) or hello [developer tutorial](tutorial-query-graph.md) toocreate an account and populate your database.</span></span> <span data-ttu-id="79838-114">Můžete spustit následující dotazy pomocí hello hello [knihovny Azure Cosmos DB .NET grafu](graph-sdk-dotnet.md), [Gremlin konzoly](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), nebo vaše oblíbené Gremlin ovladač.</span><span class="sxs-lookup"><span data-stu-id="79838-114">You can run hello following queries using hello [Azure Cosmos DB .NET graph library](graph-sdk-dotnet.md), [Gremlin console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), or your favorite Gremlin driver.</span></span>

## <a name="count-vertices-in-hello-graph"></a><span data-ttu-id="79838-115">Počet vrcholy v grafu hello</span><span class="sxs-lookup"><span data-stu-id="79838-115">Count vertices in hello graph</span></span>

<span data-ttu-id="79838-116">Hello následující fragment kódu ukazuje, jak toocount hello počet vrcholy v grafu hello:</span><span class="sxs-lookup"><span data-stu-id="79838-116">hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```
g.V().count()
```

## <a name="filters"></a><span data-ttu-id="79838-117">Filtry</span><span class="sxs-lookup"><span data-stu-id="79838-117">Filters</span></span>

<span data-ttu-id="79838-118">Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` toobuild složitější filtry.</span><span class="sxs-lookup"><span data-stu-id="79838-118">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters.</span></span> <span data-ttu-id="79838-119">Azure Cosmos DB poskytuje, bez ohledu na schéma indexu všech vlastností v rámci vrcholy a stupňů pro rychlé dotazy:</span><span class="sxs-lookup"><span data-stu-id="79838-119">Azure Cosmos DB provides schema-agnostic indexing of all properties within your vertices and degrees for fast queries:</span></span>

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a><span data-ttu-id="79838-120">Projekce</span><span class="sxs-lookup"><span data-stu-id="79838-120">Projection</span></span>

<span data-ttu-id="79838-121">Můžete promítnout některé vlastnosti ve výsledcích dotazů hello pomocí hello `values` kroku:</span><span class="sxs-lookup"><span data-stu-id="79838-121">You can project certain properties in hello query results using hello `values` step:</span></span>

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a><span data-ttu-id="79838-122">Vyhledání souvisejících okraje a vrcholy</span><span class="sxs-lookup"><span data-stu-id="79838-122">Find related edges and vertices</span></span>

<span data-ttu-id="79838-123">Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází.</span><span class="sxs-lookup"><span data-stu-id="79838-123">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="79838-124">Grafy jsou rychlé a efektivní pro operace průchodu, pokud potřebujete toonavigate toorelated okraje a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="79838-124">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="79838-125">Umožňuje najít všechny přátelích Thomas.</span><span class="sxs-lookup"><span data-stu-id="79838-125">Let's find all friends of Thomas.</span></span> <span data-ttu-id="79838-126">Provedeme to pomocí na Gremlin `outE` krok toofind všechny hello odesílací okrajů z Thomas a pak z těchto hran pomocí Gremlin na procházení toohello v vrcholy `inV` krok:</span><span class="sxs-lookup"><span data-stu-id="79838-126">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="79838-127">Další dotaz Hello provádí dvěma segmenty směrování toofind všechny Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát.</span><span class="sxs-lookup"><span data-stu-id="79838-127">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="79838-128">Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně směšovací filtru výrazů, provádění opakování ve smyčce pomocí hello `loop` kroku a implementuje podmíněného navigační pomocí hello `choose` krok.</span><span class="sxs-lookup"><span data-stu-id="79838-128">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="79838-129">Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="79838-129">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

## <a name="next-steps"></a><span data-ttu-id="79838-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79838-130">Next steps</span></span>

<span data-ttu-id="79838-131">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="79838-131">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79838-132">Jak se naučili tooquery pomocí grafu</span><span class="sxs-lookup"><span data-stu-id="79838-132">Learned how tooquery using Graph</span></span> 

<span data-ttu-id="79838-133">Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.</span><span class="sxs-lookup"><span data-stu-id="79838-133">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="79838-134">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="79838-134">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)