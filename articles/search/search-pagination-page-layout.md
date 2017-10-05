---
title: "Postup stránkování výsledků vyhledávání ve službě Azure Search | Microsoft Docs"
description: "Stránkování ve službě Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a><span data-ttu-id="55296-103">Jak v Azure Search stránkovat výsledky hledání</span><span class="sxs-lookup"><span data-stu-id="55296-103">How to page search results in Azure Search</span></span>
<span data-ttu-id="55296-104">Tento článek obsahuje pokyny týkající se používání rozhraní API REST služby vyhledávání Azure k implementaci standardní elementy stránka s výsledky hledání, například celkový počet, načtení dokumentu, pořadí řazení a navigace.</span><span class="sxs-lookup"><span data-stu-id="55296-104">This article provides guidance on how to use the Azure Search Service REST API to implement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="55296-105">V každém případě dál uvedených souvisejících se stránkami možnosti, které přispívají data nebo informace na stránce s výsledky hledání jsou zadaná pomocí [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798927.aspx) požadavky odeslané do služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="55296-105">In every case mentioned below, page-related options that contribute data or information to your search results page are specified through the [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent to your Azure Search Service.</span></span> <span data-ttu-id="55296-106">Žádosti o zahrnovat příkaz GET, cestu a parametry dotazu, které informovat o tom, co je požadované služby a postup formulovali odpovědi.</span><span class="sxs-lookup"><span data-stu-id="55296-106">Requests include a GET command, path, and query parameters that inform the service what is being requested, and how to formulate the response.</span></span>

> [!NOTE]
> <span data-ttu-id="55296-107">Počet elementů, například adresa URL služby a cesta, příkaz HTTP, zahrnuje žádost platná `api-version`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="55296-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="55296-108">Jako stručný výtah jsme oříznut příklady ke zvýraznění syntaxe, které se týkají stránkování.</span><span class="sxs-lookup"><span data-stu-id="55296-108">For brevity, we trimmed the examples to highlight just the syntax that is relevant to pagination.</span></span> <span data-ttu-id="55296-109">Najdete v tématu [rozhraní REST API služby Azure Search](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentace podrobnosti o žádosti o syntaxi.</span><span class="sxs-lookup"><span data-stu-id="55296-109">Please see the [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="55296-110">Celkový počet přístupů a počty stránky</span><span class="sxs-lookup"><span data-stu-id="55296-110">Total hits and Page Counts</span></span>
<span data-ttu-id="55296-111">Zobrazuje celkový počet výsledků vrácených z dotazu a pak vrácení výsledků v menší bloky dat, je nezbytné, aby prakticky všechny stránky vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="55296-111">Showing the total number of results returned from a query, and then returning those results in smaller chunks, is fundamental to virtually all search pages.</span></span>

![][1]

<span data-ttu-id="55296-112">Ve službě Azure Search, můžete použít `$count`, `$top`, a `$skip` parametry, které vrátí tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="55296-112">In Azure Search, you use the `$count`, `$top`, and `$skip` parameters to return these values.</span></span> <span data-ttu-id="55296-113">Následující příklad ukazuje ukázková žádost pro celkový počet přístupů, vrátí jako `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="55296-113">The following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="55296-114">Dokumenty ve skupinách 15 načíst a také se zobrazují celkový počet přístupů do začínající na první stránce:</span><span class="sxs-lookup"><span data-stu-id="55296-114">Retrieve documents in groups of 15, and also show the total hits, starting at the first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="55296-115">Přestránkování výsledky vyžaduje obě `$top` a `$skip`, kde `$top` Určuje, kolik položek k vrácení v dávce, a `$skip` Určuje, kolik položek Pokud chcete vynechat.</span><span class="sxs-lookup"><span data-stu-id="55296-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items to return in a batch, and `$skip` specifies how many items to skip.</span></span> <span data-ttu-id="55296-116">V následujícím příkladu každý stránka zobrazuje vedle 15 položky indikován přírůstkové přeskočí v `$skip` parametr.</span><span class="sxs-lookup"><span data-stu-id="55296-116">In the following example, each page shows the next 15 items, indicated by the incremental jumps in the `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="55296-117">Rozložení</span><span class="sxs-lookup"><span data-stu-id="55296-117">Layout</span></span>
<span data-ttu-id="55296-118">Na stránce výsledky hledání můžete zobrazit miniatury, podmnožinu pole a odkaz na stránku plnou verzi produktu.</span><span class="sxs-lookup"><span data-stu-id="55296-118">On a search results page, you might want to show a thumbnail image, a subset of fields, and a link to a full product page.</span></span>

 ![][2]

<span data-ttu-id="55296-119">Ve službě Azure Search, byste použili `$select` a příkaz vyhledávání implementovat toto prostředí.</span><span class="sxs-lookup"><span data-stu-id="55296-119">In Azure Search, you would use `$select` and a lookup command to implement this experience.</span></span>

<span data-ttu-id="55296-120">Chcete-li vrátit podmnožinu pole vedle sebe rozložení:</span><span class="sxs-lookup"><span data-stu-id="55296-120">To return a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="55296-121">Bitové kopie a mediálních souborů nejsou přímo s možností vyhledávání a by měly být uložené v jiné platformy úložiště, jako je například úložiště objektů Blob v Azure, jak snížit náklady.</span><span class="sxs-lookup"><span data-stu-id="55296-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, to reduce costs.</span></span> <span data-ttu-id="55296-122">V indexu a dokumenty zadejte pole, které obsahuje adresu URL externí obsah.</span><span class="sxs-lookup"><span data-stu-id="55296-122">In the index and documents, define a field that stores the URL address of the external content.</span></span> <span data-ttu-id="55296-123">Pak můžete toto pole jako odkaz na obrázek.</span><span class="sxs-lookup"><span data-stu-id="55296-123">You can then use the field as an image reference.</span></span> <span data-ttu-id="55296-124">Adresu URL do bitové kopie musí být v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="55296-124">The URL to the image should be in the document.</span></span>

<span data-ttu-id="55296-125">Načtení stránky produktu popis pro **onClick** událostí, použijte [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798929.aspx) předávání v klíči dokumentu pro načtení.</span><span class="sxs-lookup"><span data-stu-id="55296-125">To retrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) to pass in the key of the document to retrieve.</span></span> <span data-ttu-id="55296-126">Datový typ klíče je `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="55296-126">The data type of the key is `Edm.String`.</span></span> <span data-ttu-id="55296-127">V tomto příkladu je *246810*.</span><span class="sxs-lookup"><span data-stu-id="55296-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="55296-128">Řazení podle relevance, hodnocení a ceny</span><span class="sxs-lookup"><span data-stu-id="55296-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="55296-129">Pořadí řazení často výchozí nastavení relevance, ale je běžné, aby alternativní řazení objednávky snadno dostupné, aby zákazníci můžete rychle změnit pořadí existující výsledky do různých rank pořadí.</span><span class="sxs-lookup"><span data-stu-id="55296-129">Sort orders often default to relevance, but it's common to make alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="55296-130">Ve službě Azure Search, řazení podle `$orderby` výrazu pro všechna pole, které jsou uloženy jako`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="55296-130">In Azure Search, sorting is based on the `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="55296-131">Relevance důrazně souvisí s profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="55296-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="55296-132">Můžete vyhodnocování výchozí, což závisí na text analýzy a statistiky zařadit všechny výsledky, pořadí s vyšší skóre přejít do dokumentů s více nebo silnější odpovídá na hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="55296-132">You can use the default scoring, which relies on text analysis and statistics to rank order all results, with higher scores going to documents with more or stronger matches on a search term.</span></span>

<span data-ttu-id="55296-133">Alternativní pořadí řazení obvykle souvisejí s **onClick** události, které zpětné volání pro metodu, která vytvoří pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="55296-133">Alternative sort orders are typically associated with **onClick** events that call back to a method that builds the sort order.</span></span> <span data-ttu-id="55296-134">Například uděleno tento element stránky:</span><span class="sxs-lookup"><span data-stu-id="55296-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="55296-135">Vytvoříte metodu, která přijme jako vstup možnost vybrané řazení a vrátí seřazený seznam pro kritéria spojená s tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="55296-135">You would create a method that accepts the selected sort option as input, and returns an ordered list for the criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="55296-136">Při vyhodnocování výchozí stačí pro mnoho scénářů, doporučujeme místo vytvoření relevance na základě vlastní profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="55296-136">While the default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="55296-137">Vlastní profil vyhodnocování poskytuje způsob, jak nárůst položky, které jsou více užitečné k vaší firmě.</span><span class="sxs-lookup"><span data-stu-id="55296-137">A custom scoring profile gives you a way to boost items that are more beneficial to your business.</span></span> <span data-ttu-id="55296-138">V tématu [přidat profil vyhodnocování](http://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="55296-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="55296-139">Fasetová navigace</span><span class="sxs-lookup"><span data-stu-id="55296-139">Faceted navigation</span></span>
<span data-ttu-id="55296-140">Navigace vyhledávání je běžné na stránka s výsledky, často nacházející se v horní části stránky na straně.</span><span class="sxs-lookup"><span data-stu-id="55296-140">Search navigation is common on a results page, often located at the side or top of a page.</span></span> <span data-ttu-id="55296-141">Ve službě Azure Search poskytuje Fasetové navigace řízené samotným hledání podle předdefinovaných filtrů.</span><span class="sxs-lookup"><span data-stu-id="55296-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="55296-142">V tématu [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="55296-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-the-page-level"></a><span data-ttu-id="55296-143">Filtry na úrovni stránky</span><span class="sxs-lookup"><span data-stu-id="55296-143">Filters at the page level</span></span>
<span data-ttu-id="55296-144">Pokud návrh vašeho řešení zahrnuty vyhrazené vyhledávání stránky pro určité typy obsahu (například prodejní online aplikace, která má oddělení uvedených v horní části stránky), můžete vložit výraz filtru společně **onClick** událost a otevřete stránku v odkazující stavu.</span><span class="sxs-lookup"><span data-stu-id="55296-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at the top of the page), you can insert a filter expression alongside an **onClick** event to open a page in a prefiltered state.</span></span> 

<span data-ttu-id="55296-145">Můžete odeslat filtr s nebo bez výrazu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="55296-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="55296-146">Například následující požadavek bude filtrovat název značky, vrácení pouze dokumenty, které odpovídají ho.</span><span class="sxs-lookup"><span data-stu-id="55296-146">For example, the following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="55296-147">V tématu [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx) Další informace o `$filter` výrazy.</span><span class="sxs-lookup"><span data-stu-id="55296-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="55296-148">Viz také</span><span class="sxs-lookup"><span data-stu-id="55296-148">See Also</span></span>
* [<span data-ttu-id="55296-149">Rozhraní API REST služby vyhledávání systému Azure</span><span class="sxs-lookup"><span data-stu-id="55296-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="55296-150">Operace indexu</span><span class="sxs-lookup"><span data-stu-id="55296-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="55296-151">Operace dokumentu</span><span class="sxs-lookup"><span data-stu-id="55296-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="55296-152">Kurzy o službě Azure Search a video</span><span class="sxs-lookup"><span data-stu-id="55296-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="55296-153">Fasetové navigace ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="55296-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
