---
title: "Dotazování indexu (portál – Azure Search) | Dokumentace Microsoftu"
description: "Vydejte vyhledávací dotaz v Průzkumníku služby Search na webu Azure Portal."
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
ms.openlocfilehash: dd68d8ed073bf7b8666ddef35a2f1f84df690b4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a><span data-ttu-id="38a2e-103">Dotazování indexu služby Azure Search pomocí průzkumníka služby Search na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="38a2e-103">Query an Azure Search index using Search Explorer in the Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="38a2e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="38a2e-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="38a2e-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="38a2e-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="38a2e-106">.NET</span><span class="sxs-lookup"><span data-stu-id="38a2e-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="38a2e-107">REST</span><span class="sxs-lookup"><span data-stu-id="38a2e-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="38a2e-108">Tento článek vám ukáže postup dotazování indexu služby Azure Search pomocí **průzkumníka služby Search** na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="38a2e-108">This article shows you how to query an Azure Search index using **Search Explorer** in the Azure portal.</span></span> <span data-ttu-id="38a2e-109">Pomocí průzkumníka služby Search můžete odesílat jednoduché nebo úplné řetězce dotazů Lucene do jakéhokoli existujícího indexu v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="38a2e-109">You can use Search Explorer to submit simple or full Lucene query strings to any existing index in your service.</span></span>

## <a name="open-the-service-dashboard"></a><span data-ttu-id="38a2e-110">Otevření řídicího panelu služby</span><span class="sxs-lookup"><span data-stu-id="38a2e-110">Open the service dashboard</span></span>
1. <span data-ttu-id="38a2e-111">Klikněte na **Všechny prostředky** na panelu odkazů na levé straně webu [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span><span class="sxs-lookup"><span data-stu-id="38a2e-111">Click **All resources** in the jump bar on the left side of the [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="38a2e-112">Vyberte službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="38a2e-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="38a2e-113">Výběr indexu</span><span class="sxs-lookup"><span data-stu-id="38a2e-113">Select an index</span></span>

<span data-ttu-id="38a2e-114">Na dlaždici **Indexy** vyberte index, který chcete prohledat.</span><span class="sxs-lookup"><span data-stu-id="38a2e-114">Select the index you would like to search from the **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="38a2e-115">Otevření průzkumníka služby Search</span><span class="sxs-lookup"><span data-stu-id="38a2e-115">Open Search Explorer</span></span>

<span data-ttu-id="38a2e-116">Kliknutím na dlaždici Průzkumník služby Search otevřete vysouvací panel hledání a podokno výsledků.</span><span class="sxs-lookup"><span data-stu-id="38a2e-116">Click on the Search Explorer tile to slide open the search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="38a2e-117">Spusťte hledání</span><span class="sxs-lookup"><span data-stu-id="38a2e-117">Start searching</span></span>

<span data-ttu-id="38a2e-118">Pokud používáte průzkumníka služby Search, můžete dotaz formulovat zadáním [parametrů dotazu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="38a2e-118">When using the Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) to formulate the query.</span></span>

1. <span data-ttu-id="38a2e-119">V části **Řetězec dotazu** zadejte dotaz a potom stiskněte **Hledat**.</span><span class="sxs-lookup"><span data-stu-id="38a2e-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="38a2e-120">Řetězec dotazu se automaticky parsuje do správné adresy URL žádosti za účelem odeslání žádosti HTTP do rozhraní REST API služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="38a2e-120">The query string is automatically parsed into the proper request URL to submit a HTTP request against the Azure Search REST API.</span></span>   
   
   <span data-ttu-id="38a2e-121">K vytvoření žádosti můžete použít jakoukoli platnou syntaxi jednoduchého nebo úplného dotazu Lucene.</span><span class="sxs-lookup"><span data-stu-id="38a2e-121">You can use any valid simple or full Lucene query syntax to create the request.</span></span> <span data-ttu-id="38a2e-122">Znak `*` je ekvivalentní prázdnému nebo nespecifikovanému hledání, které vrátí všechny dokumenty bez zvláštního pořadí.</span><span class="sxs-lookup"><span data-stu-id="38a2e-122">The `*` character is equivalent to an empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="38a2e-123">V části **Výsledky** jsou výsledky dotazu uvedené ve stejném nezpracovaném formátu JSON jako datová část vrácená v textu odpovědi HTTP při vydávání žádostí prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="38a2e-123">In  **Results**, query results are presented in raw JSON, identical to the payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="38a2e-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38a2e-124">Next steps</span></span>

<span data-ttu-id="38a2e-125">V následujících zdrojích najdete další informace o syntaxi dotazů a příklady.</span><span class="sxs-lookup"><span data-stu-id="38a2e-125">The following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="38a2e-126">Jednoduchá syntaxe dotazů</span><span class="sxs-lookup"><span data-stu-id="38a2e-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="38a2e-127">Syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="38a2e-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="38a2e-128">Příklady syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="38a2e-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="38a2e-129">Syntaxe výrazů filtru OData</span><span class="sxs-lookup"><span data-stu-id="38a2e-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 