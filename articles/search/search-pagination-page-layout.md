---
title: "výsledky hledání toopage aaaHow ve službě Azure Search | Microsoft Docs"
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
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a><span data-ttu-id="d6026-103">Jak výsledky hledání toopage ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="d6026-103">How toopage search results in Azure Search</span></span>
<span data-ttu-id="d6026-104">Tento článek obsahuje pokyny k jak stránky, například celkový počet, načtení dokumentu, pořadí řazení a navigační výsledků toouse hello rozhraní REST API služby Azure Search tooimplement standardních prvků hledání.</span><span class="sxs-lookup"><span data-stu-id="d6026-104">This article provides guidance on how toouse hello Azure Search Service REST API tooimplement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="d6026-105">V každém případě dál uvedených souvisejících se stránkami možnosti, které přispívají data nebo informace o tooyour stránky s výsledky hledání jsou určeny pomocí hello [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798927.aspx) požadavky odeslané tooyour služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="d6026-105">In every case mentioned below, page-related options that contribute data or information tooyour search results page are specified through hello [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent tooyour Azure Search Service.</span></span> <span data-ttu-id="d6026-106">Žádosti o zahrnovat příkaz GET, cestu a parametry dotazu, které informovat o tom, co je požadované hello služby a jak tooformulate hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d6026-106">Requests include a GET command, path, and query parameters that inform hello service what is being requested, and how tooformulate hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="d6026-107">Počet elementů, například adresa URL služby a cesta, příkaz HTTP, zahrnuje žádost platná `api-version`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d6026-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="d6026-108">Jako stručný výtah jsme oříznut hello příklady toohighlight jenom hello syntaxi, která je relevantní toopagination.</span><span class="sxs-lookup"><span data-stu-id="d6026-108">For brevity, we trimmed hello examples toohighlight just hello syntax that is relevant toopagination.</span></span> <span data-ttu-id="d6026-109">Najdete v tématu hello [rozhraní REST API služby Azure Search](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentace podrobnosti o žádosti o syntaxi.</span><span class="sxs-lookup"><span data-stu-id="d6026-109">Please see hello [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="d6026-110">Celkový počet přístupů a počty stránky</span><span class="sxs-lookup"><span data-stu-id="d6026-110">Total hits and Page Counts</span></span>
<span data-ttu-id="d6026-111">Zobrazuje hello celkový počet výsledků vrácených dotazem a ty pak vrácení výsledků v menší bloky dat, je základní toovirtually všechny stránky vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="d6026-111">Showing hello total number of results returned from a query, and then returning those results in smaller chunks, is fundamental toovirtually all search pages.</span></span>

![][1]

<span data-ttu-id="d6026-112">Ve službě Azure Search, můžete použít hello `$count`, `$top`, a `$skip` parametry tooreturn tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d6026-112">In Azure Search, you use hello `$count`, `$top`, and `$skip` parameters tooreturn these values.</span></span> <span data-ttu-id="d6026-113">Hello následující příklad ukazuje ukázková žádost pro celkový počet přístupů, vrátí jako `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="d6026-113">hello following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="d6026-114">Dokumenty ve skupinách 15 načíst a také se zobrazují celkový počet přístupů hello začínající na první stránku hello:</span><span class="sxs-lookup"><span data-stu-id="d6026-114">Retrieve documents in groups of 15, and also show hello total hits, starting at hello first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="d6026-115">Přestránkování výsledky vyžaduje obě `$top` a `$skip`, kde `$top` Určuje, kolik tooreturn položek v dávce, a `$skip` Určuje, kolik položek tooskip.</span><span class="sxs-lookup"><span data-stu-id="d6026-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items tooreturn in a batch, and `$skip` specifies how many items tooskip.</span></span> <span data-ttu-id="d6026-116">V hello následující ukázka, každé stránce zobrazuje hello vedle 15 položky indikován hello přírůstkové přeskočí v hello `$skip` parametr.</span><span class="sxs-lookup"><span data-stu-id="d6026-116">In hello following example, each page shows hello next 15 items, indicated by hello incremental jumps in hello `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="d6026-117">Rozložení</span><span class="sxs-lookup"><span data-stu-id="d6026-117">Layout</span></span>
<span data-ttu-id="d6026-118">Na stránce výsledky hledání můžete chtít tooshow miniaturu, podmnožinu pole a stránku odkaz tooa plnou verzi produktu.</span><span class="sxs-lookup"><span data-stu-id="d6026-118">On a search results page, you might want tooshow a thumbnail image, a subset of fields, and a link tooa full product page.</span></span>

 ![][2]

<span data-ttu-id="d6026-119">Ve službě Azure Search, byste použili `$select` a vyhledávací příkaz tooimplement toto prostředí.</span><span class="sxs-lookup"><span data-stu-id="d6026-119">In Azure Search, you would use `$select` and a lookup command tooimplement this experience.</span></span>

<span data-ttu-id="d6026-120">tooreturn podmnožinu pole vedle sebe rozložení:</span><span class="sxs-lookup"><span data-stu-id="d6026-120">tooreturn a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="d6026-121">Bitové kopie a mediálních souborů nejsou přímo s možností vyhledávání a by měly být uložené v jiné platformy úložiště, jako je například úložiště objektů Azure Blob, tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="d6026-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, tooreduce costs.</span></span> <span data-ttu-id="d6026-122">V indexu hello a dokumenty zadejte pole, které obsahuje adresu URL hello hello externí obsah.</span><span class="sxs-lookup"><span data-stu-id="d6026-122">In hello index and documents, define a field that stores hello URL address of hello external content.</span></span> <span data-ttu-id="d6026-123">Pole hello pak můžete použít jako odkaz na obrázek.</span><span class="sxs-lookup"><span data-stu-id="d6026-123">You can then use hello field as an image reference.</span></span> <span data-ttu-id="d6026-124">bitové kopie toohello Hello adresa URL musí být v dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="d6026-124">hello URL toohello image should be in hello document.</span></span>

<span data-ttu-id="d6026-125">tooretrieve produktu popis stránky pro **onClick** událostí, použijte [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass v klíči dokumentu tooretrieve hello hello.</span><span class="sxs-lookup"><span data-stu-id="d6026-125">tooretrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass in hello key of hello document tooretrieve.</span></span> <span data-ttu-id="d6026-126">datový typ Hello hello klíče je `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="d6026-126">hello data type of hello key is `Edm.String`.</span></span> <span data-ttu-id="d6026-127">V tomto příkladu je *246810*.</span><span class="sxs-lookup"><span data-stu-id="d6026-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="d6026-128">Řazení podle relevance, hodnocení a ceny</span><span class="sxs-lookup"><span data-stu-id="d6026-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="d6026-129">Výchozí toorelevance, ale je běžné toomake alternativní pořadí řazení snadno dostupné, aby zákazníci můžete rychle změnit pořadí existující výsledky do různých rank pořadí se často pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="d6026-129">Sort orders often default toorelevance, but it's common toomake alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="d6026-130">Ve službě Azure Search, řazení podle hello `$orderby` výrazu pro všechna pole, které jsou uloženy jako`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="d6026-130">In Azure Search, sorting is based on hello `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="d6026-131">Relevance důrazně souvisí s profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="d6026-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="d6026-132">Můžete použít hello vyhodnocování výchozí, což závisí na text analýzy a statistiky toorank pořadí všechny výsledky, s vyšší skóre toodocuments s více nebo silnější odpovídá děje hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="d6026-132">You can use hello default scoring, which relies on text analysis and statistics toorank order all results, with higher scores going toodocuments with more or stronger matches on a search term.</span></span>

<span data-ttu-id="d6026-133">Alternativní pořadí řazení obvykle souvisejí s **onClick** události, které zpětné volání metoda tooa které sestavení hello pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="d6026-133">Alternative sort orders are typically associated with **onClick** events that call back tooa method that builds hello sort order.</span></span> <span data-ttu-id="d6026-134">Například uděleno tento element stránky:</span><span class="sxs-lookup"><span data-stu-id="d6026-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="d6026-135">Metoda, která přijme hello vybrali možnost řazení jako vstup a vrátí seřazený seznam pro kritéria hello přidružené k této možnosti vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="d6026-135">You would create a method that accepts hello selected sort option as input, and returns an ordered list for hello criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="d6026-136">Při vyhodnocování výchozí hello stačí pro mnoho scénářů, doporučujeme místo vytvoření relevance na základě vlastní profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="d6026-136">While hello default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="d6026-137">Vlastní profil vyhodnocování vám dává položky tooboost způsobem, které jsou více výhodné tooyour firmy.</span><span class="sxs-lookup"><span data-stu-id="d6026-137">A custom scoring profile gives you a way tooboost items that are more beneficial tooyour business.</span></span> <span data-ttu-id="d6026-138">V tématu [přidat profil vyhodnocování](http://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d6026-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="d6026-139">Fasetová navigace</span><span class="sxs-lookup"><span data-stu-id="d6026-139">Faceted navigation</span></span>
<span data-ttu-id="d6026-140">Navigace vyhledávání je běžné na stránka s výsledky, často nacházející se v horní části stránky a hello straně.</span><span class="sxs-lookup"><span data-stu-id="d6026-140">Search navigation is common on a results page, often located at hello side or top of a page.</span></span> <span data-ttu-id="d6026-141">Ve službě Azure Search poskytuje Fasetové navigace řízené samotným hledání podle předdefinovaných filtrů.</span><span class="sxs-lookup"><span data-stu-id="d6026-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="d6026-142">V tématu [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d6026-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-hello-page-level"></a><span data-ttu-id="d6026-143">Filtry na úrovni stránky hello</span><span class="sxs-lookup"><span data-stu-id="d6026-143">Filters at hello page level</span></span>
<span data-ttu-id="d6026-144">Pokud návrh vašeho řešení zahrnuty vyhrazené vyhledávání stránky pro určité typy obsahu (například prodejní online aplikace, která má oddělení uvedené v hello horní části stránky hello), můžete vložit výraz filtru společně **onClick** tooopen událostí na stránce v odkazující stavu.</span><span class="sxs-lookup"><span data-stu-id="d6026-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at hello top of hello page), you can insert a filter expression alongside an **onClick** event tooopen a page in a prefiltered state.</span></span> 

<span data-ttu-id="d6026-145">Můžete odeslat filtr s nebo bez výrazu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="d6026-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="d6026-146">Například hello následující požadavek bude filtrovat název značky, vrácení pouze dokumenty, které odpovídají ho.</span><span class="sxs-lookup"><span data-stu-id="d6026-146">For example, hello following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="d6026-147">V tématu [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx) Další informace o `$filter` výrazy.</span><span class="sxs-lookup"><span data-stu-id="d6026-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="d6026-148">Viz také</span><span class="sxs-lookup"><span data-stu-id="d6026-148">See Also</span></span>
* [<span data-ttu-id="d6026-149">Rozhraní API REST služby vyhledávání systému Azure</span><span class="sxs-lookup"><span data-stu-id="d6026-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="d6026-150">Operace indexu</span><span class="sxs-lookup"><span data-stu-id="d6026-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="d6026-151">Operace dokumentu</span><span class="sxs-lookup"><span data-stu-id="d6026-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="d6026-152">Kurzy o službě Azure Search a video</span><span class="sxs-lookup"><span data-stu-id="d6026-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="d6026-153">Fasetové navigace ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="d6026-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
