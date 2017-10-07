---
title: "AAA \"dotazování indexu (portál – Azure Search) | Microsoft Docs\""
description: "Vydejte vyhledávací dotaz v portálu Azure hello Průzkumník služby Search."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: 56bab3ef8a66eeb053fbbeb6d322acb6824fb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a><span data-ttu-id="e9fab-103">Dotazování indexu Azure Search pomocí Průzkumník služby Search v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9fab-103">Query an Azure Search index using Search Explorer in hello Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9fab-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e9fab-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="e9fab-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e9fab-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="e9fab-106">.NET</span><span class="sxs-lookup"><span data-stu-id="e9fab-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="e9fab-107">REST</span><span class="sxs-lookup"><span data-stu-id="e9fab-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="e9fab-108">Tento článek ukazuje, jak index tooquery Azure Search pomocí **Průzkumník služby Search** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9fab-108">This article shows you how tooquery an Azure Search index using **Search Explorer** in hello Azure portal.</span></span> <span data-ttu-id="e9fab-109">Průzkumník služby Search toosubmit jednoduchý nebo úplné Lucene dotazu řetězce tooany existujícího indexu můžete použít ve službě.</span><span class="sxs-lookup"><span data-stu-id="e9fab-109">You can use Search Explorer toosubmit simple or full Lucene query strings tooany existing index in your service.</span></span>

## <a name="open-hello-service-dashboard"></a><span data-ttu-id="e9fab-110">Řídicí panel služby otevřete hello</span><span class="sxs-lookup"><span data-stu-id="e9fab-110">Open hello service dashboard</span></span>
1. <span data-ttu-id="e9fab-111">Klikněte na tlačítko **všechny prostředky** panelu hello přechod na levé straně hello hello [portál Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span><span class="sxs-lookup"><span data-stu-id="e9fab-111">Click **All resources** in hello jump bar on hello left side of hello [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="e9fab-112">Vyberte službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="e9fab-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="e9fab-113">Výběr indexu</span><span class="sxs-lookup"><span data-stu-id="e9fab-113">Select an index</span></span>

<span data-ttu-id="e9fab-114">Vyberte hello index chcete toosearch z hello **indexy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="e9fab-114">Select hello index you would like toosearch from hello **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="e9fab-115">Otevření průzkumníka služby Search</span><span class="sxs-lookup"><span data-stu-id="e9fab-115">Open Search Explorer</span></span>

<span data-ttu-id="e9fab-116">Klikněte na hello panelu vyhledávání otevřete hello tooslide dlaždici Průzkumník služby Search a podokně výsledků.</span><span class="sxs-lookup"><span data-stu-id="e9fab-116">Click on hello Search Explorer tile tooslide open hello search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="e9fab-117">Spusťte hledání</span><span class="sxs-lookup"><span data-stu-id="e9fab-117">Start searching</span></span>

<span data-ttu-id="e9fab-118">Pokud používáte hello Průzkumník služby Search, můžete zadat [parametrů dotazu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="e9fab-118">When using hello Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello query.</span></span>

1. <span data-ttu-id="e9fab-119">V části **Řetězec dotazu** zadejte dotaz a potom stiskněte **Hledat**.</span><span class="sxs-lookup"><span data-stu-id="e9fab-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="e9fab-120">řetězec dotazu Hello se automaticky Parsuje do správné požadavek hello toosubmit adresy URL žádosti HTTP hello REST API služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="e9fab-120">hello query string is automatically parsed into hello proper request URL toosubmit a HTTP request against hello Azure Search REST API.</span></span>   
   
   <span data-ttu-id="e9fab-121">Můžete vytvořit žádné platné jednoduchý nebo úplné Lucene syntaxe toocreate hello požadavku dotazu.</span><span class="sxs-lookup"><span data-stu-id="e9fab-121">You can use any valid simple or full Lucene query syntax toocreate hello request.</span></span> <span data-ttu-id="e9fab-122">Hello `*` znak je ekvivalentní tooan prázdný nebo nezadanou vyhledávání, který vrátí všechny dokumenty v žádné konkrétní pořadí.</span><span class="sxs-lookup"><span data-stu-id="e9fab-122">hello `*` character is equivalent tooan empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="e9fab-123">V **výsledky**, výsledky dotazu jsou uvedeny v nezpracovaném formátu JSON, identické toohello datové části, vrátí se v textu odpovědi HTTP při vydávání žádostí prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="e9fab-123">In  **Results**, query results are presented in raw JSON, identical toohello payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="e9fab-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9fab-124">Next steps</span></span>

<span data-ttu-id="e9fab-125">Hello následující prostředky poskytují další dotaz informace ze syntaxe a příklady.</span><span class="sxs-lookup"><span data-stu-id="e9fab-125">hello following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="e9fab-126">Jednoduchá syntaxe dotazů</span><span class="sxs-lookup"><span data-stu-id="e9fab-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="e9fab-127">Syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="e9fab-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="e9fab-128">Příklady syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="e9fab-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="e9fab-129">Syntaxe výrazů filtru OData</span><span class="sxs-lookup"><span data-stu-id="e9fab-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 