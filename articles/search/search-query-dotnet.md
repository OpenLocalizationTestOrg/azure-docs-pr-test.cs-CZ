---
title: "AAA \"dotazování indexu (rozhraní .NET API - Azure Search) | Microsoft Docs\""
description: "Sestavení vyhledávacího dotazu ve službě Azure search a pomocí vyhledávání parametrů toofilter a řazení výsledků vyhledávání."
services: search
manager: jhubbard
documentationcenter: 
author: brjohnstmsft
ms.assetid: 12c3efba-ea99-4187-9d2d-f63b5ec7040d
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/19/2017
ms.author: brjohnst
ms.openlocfilehash: 8b3ba1cd1270aad038fb48d9053fcff35d243e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a><span data-ttu-id="86906-103">Dotazování indexu Azure Search pomocí .NET SDK hello</span><span class="sxs-lookup"><span data-stu-id="86906-103">Query your Azure Search index using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="86906-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="86906-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="86906-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="86906-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="86906-106">.NET</span><span class="sxs-lookup"><span data-stu-id="86906-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="86906-107">REST</span><span class="sxs-lookup"><span data-stu-id="86906-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="86906-108">Tento článek vám ukáže, jak tooquery indexu pomocí hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="86906-108">This article will show you how tooquery an index using hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span>

<span data-ttu-id="86906-109">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="86906-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span>

> [!NOTE]
> <span data-ttu-id="86906-110">Ukázkový kód v tomto článku je napsán v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="86906-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="86906-111">Hello úplný zdrojový kód najdete [na Githubu](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="86906-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="86906-112">Můžete si také přečíst o hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) pro podrobnější procházení prostřednictvím hello ukázek kódu.</span><span class="sxs-lookup"><span data-stu-id="86906-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="86906-113">Zjistěte klíč api-key správce služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="86906-113">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="86906-114">Teď, když jste vytvořili index Azure Search, jste skoro hotový tooissue dotazy pomocí .NET SDK hello.</span><span class="sxs-lookup"><span data-stu-id="86906-114">Now that you have created an Azure Search index, you are almost ready tooissue queries using hello .NET SDK.</span></span> <span data-ttu-id="86906-115">Nejprve budete potřebovat tooobtain mezi hello dotazu klíče api Key vytvořených pro hello vyhledávací službu, kterou jste zřídili.</span><span class="sxs-lookup"><span data-stu-id="86906-115">First, you will need tooobtain one of hello query api-keys that was generated for hello search service you provisioned.</span></span> <span data-ttu-id="86906-116">Hello .NET SDK odešle tento klíč rozhraní api služby tooyour každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="86906-116">hello .NET SDK will send this api-key on every request tooyour service.</span></span> <span data-ttu-id="86906-117">Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="86906-117">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="86906-118">toofind vaší služby klíče api Key přihlášením toohello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="86906-118">toofind your service's api-keys you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="86906-119">Okno služby Azure Search přejděte tooyour</span><span class="sxs-lookup"><span data-stu-id="86906-119">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="86906-120">Klikněte na hello ikonu klíčů."</span><span class="sxs-lookup"><span data-stu-id="86906-120">Click on hello "Keys" icon</span></span>

<span data-ttu-id="86906-121">Vaše služba bude mít *klíče správce* a *klíče dotazů*.</span><span class="sxs-lookup"><span data-stu-id="86906-121">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="86906-122">Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat..</span><span class="sxs-lookup"><span data-stu-id="86906-122">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="86906-123">Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.</span><span class="sxs-lookup"><span data-stu-id="86906-123">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="86906-124">Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.</span><span class="sxs-lookup"><span data-stu-id="86906-124">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="86906-125">Pro účely hello dotazování indexu můžete použít jeden z klíčů dotazů.</span><span class="sxs-lookup"><span data-stu-id="86906-125">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="86906-126">Pro dotazy lze také použít klíče správce, ale byste měli používat klíče dotazů v kódu aplikace, což lépe odpovídá hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="86906-126">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="86906-127">Vytvoření instance třídy SearchIndexClient hello</span><span class="sxs-lookup"><span data-stu-id="86906-127">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="86906-128">tooissue dotazy s hello .NET SDK služby Azure Search, budete potřebovat toocreate instanci hello `SearchIndexClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="86906-128">tooissue queries with hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="86906-129">Tato třída obsahuje několik konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="86906-129">This class has several constructors.</span></span> <span data-ttu-id="86906-130">Dobrý den, ten, který chcete přebírá název vaší vyhledávací služby, název indexu a `SearchCredentials` jako parametry.</span><span class="sxs-lookup"><span data-stu-id="86906-130">hello one you want takes your search service name, index name, and a `SearchCredentials` object as parameters.</span></span> <span data-ttu-id="86906-131">`SearchCredentials` zabalí váš klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="86906-131">`SearchCredentials` wraps your api-key.</span></span>

<span data-ttu-id="86906-132">Hello následující kód vytvoří novou `SearchIndexClient` pro index "hotels" hello (vytvořené v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md)) pomocí hodnot pro název vyhledávací služby hello a rozhraní api klíče, které jsou uložené v souboru config aplikace hello soubor (`appsettings.json` v případě hello hello [ukázkové aplikace](http://aka.ms/search-dotnet-howto)):</span><span class="sxs-lookup"><span data-stu-id="86906-132">hello code below creates a new `SearchIndexClient` for hello "hotels" index (created in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md)) using values for hello search service name and api-key that are stored in hello application's config file (`appsettings.json` in hello case of hello [sample application](http://aka.ms/search-dotnet-howto)):</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="86906-133">`SearchIndexClient` obsahuje vlastnost `Documents`.</span><span class="sxs-lookup"><span data-stu-id="86906-133">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="86906-134">Tato vlastnost poskytuje že všechny metody, které potřebujete indexů Azure Search tooquery hello.</span><span class="sxs-lookup"><span data-stu-id="86906-134">This property provides all hello methods you need tooquery Azure Search indexes.</span></span>

## <a name="query-your-index"></a><span data-ttu-id="86906-135">Dotázání indexu</span><span class="sxs-lookup"><span data-stu-id="86906-135">Query your index</span></span>
<span data-ttu-id="86906-136">Hledání s hello .NET SDK je jednoduché, volání hello `Documents.Search` metoda na vaše `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="86906-136">Searching with hello .NET SDK is as simple as calling hello `Documents.Search` method on your `SearchIndexClient`.</span></span> <span data-ttu-id="86906-137">Tato metoda přijímá několik parametrů včetně textu hello vyhledávání, spolu s `SearchParameters` objekt, který lze použít toofurther upravit hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="86906-137">This method takes a few parameters, including hello search text, along with a `SearchParameters` object that can be used toofurther refine hello query.</span></span>

#### <a name="types-of-queries"></a><span data-ttu-id="86906-138">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="86906-138">Types of Queries</span></span>
<span data-ttu-id="86906-139">Hello dva hlavní [typy dotazů](search-query-overview.md#types-of-queries) použijete jsou `search` a `filter`.</span><span class="sxs-lookup"><span data-stu-id="86906-139">hello two main [query types](search-query-overview.md#types-of-queries) you will use are `search` and `filter`.</span></span> <span data-ttu-id="86906-140">Dotaz `search` vyhledává jeden nebo více výrazů ve všech *prohledávatelných* polích v indexu.</span><span class="sxs-lookup"><span data-stu-id="86906-140">A `search` query searches for one or more terms in all *searchable* fields in your index.</span></span> <span data-ttu-id="86906-141">Dotaz `filter` vyhodnocuje logický výraz na všech *filtrovatelných* polích v indexu.</span><span class="sxs-lookup"><span data-stu-id="86906-141">A `filter` query evaluates a boolean expression over all *filterable* fields in an index.</span></span>

<span data-ttu-id="86906-142">Vyhledávání i filtrování se provádí pomocí hello `Documents.Search` metoda.</span><span class="sxs-lookup"><span data-stu-id="86906-142">Both searches and filters are performed using hello `Documents.Search` method.</span></span> <span data-ttu-id="86906-143">Vyhledávací dotaz lze předat ve hello `searchText` parametr, zatímco výraz filtru lze předat v hello `Filter` vlastnost hello `SearchParameters` třídy.</span><span class="sxs-lookup"><span data-stu-id="86906-143">A search query can be passed in hello `searchText` parameter, while a filter expression can be passed in hello `Filter` property of hello `SearchParameters` class.</span></span> <span data-ttu-id="86906-144">toofilter bez vyhledávání, stačí předat `"*"` pro hello `searchText` parametr.</span><span class="sxs-lookup"><span data-stu-id="86906-144">toofilter without searching, just pass `"*"` for hello `searchText` parameter.</span></span> <span data-ttu-id="86906-145">toosearch bez filtrování, ponechte hello `Filter` vlastnost nenastavenou nebo nepředávejte `SearchParameters` instance vůbec.</span><span class="sxs-lookup"><span data-stu-id="86906-145">toosearch without filtering, just leave hello `Filter` property unset, or do not pass in a `SearchParameters` instance at all.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="86906-146">Ukázky dotazů</span><span class="sxs-lookup"><span data-stu-id="86906-146">Example Queries</span></span>
<span data-ttu-id="86906-147">Hello následující vzorový kód ukazuje několik různých způsobů tooquery hello "hotels" indexu definované v [vytvoření indexu Azure Search pomocí .NET SDK hello](search-create-index-dotnet.md#DefineIndex).</span><span class="sxs-lookup"><span data-stu-id="86906-147">hello following sample code shows a few different ways tooquery hello "hotels" index defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex).</span></span> <span data-ttu-id="86906-148">Všimněte si, že hello dokumenty vrácené ve výsledcích hledání hello jsou instancemi třídy hello `Hotel` třída, která byla definovaná v [hello Import dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md#HotelClass).</span><span class="sxs-lookup"><span data-stu-id="86906-148">Note that hello documents returned with hello search results are instances of hello `Hotel` class, which was defined in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md#HotelClass).</span></span> <span data-ttu-id="86906-149">Hello ukázkový kód využívá `WriteDocuments` metoda toooutput hello vyhledávání výsledky toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="86906-149">hello sample code makes use of a `WriteDocuments` method toooutput hello search results toohello console.</span></span> <span data-ttu-id="86906-150">Tato metoda je popsaná v další části hello.</span><span class="sxs-lookup"><span data-stu-id="86906-150">This method is described in hello next section.</span></span>

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search hello entire index for hello term 'budget' and return only hello hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter toohello index toofind hotels cheaper than $150 per night, ");
Console.WriteLine("and return hello hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search hello entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take hello top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a><span data-ttu-id="86906-151">Zpracování výsledků vyhledávání</span><span class="sxs-lookup"><span data-stu-id="86906-151">Handle search results</span></span>
<span data-ttu-id="86906-152">Hello `Documents.Search` metoda vrátí `DocumentSearchResult` objekt, který obsahuje výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="86906-152">hello `Documents.Search` method returns a `DocumentSearchResult` object that contains hello results of hello query.</span></span> <span data-ttu-id="86906-153">Příklad Hello v předchozí části hello používá metodu s názvem `WriteDocuments` toooutput hello vyhledávání výsledky toohello konzoly:</span><span class="sxs-lookup"><span data-stu-id="86906-153">hello example in hello previous section used a method called `WriteDocuments` toooutput hello search results toohello console:</span></span>

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

<span data-ttu-id="86906-154">Tady je co najít hello výsledky jako pro dotazy hello v předchozí části hello, v případě index hello "hotels" naplněný hello vzorovými daty v [hello Import dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="86906-154">Here is what hello results look like for hello queries in hello previous section, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md):</span></span>

```
Search hello entire index for hello term 'budget' and return only hello hotelName field:

Name: Roach Motel

Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close tootown hall and hello river

Search hello entire index, order by a specific field (lastRenovationDate) in descending order, take hello top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search hello entire index for hello term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

<span data-ttu-id="86906-155">Hello výše uvedený ukázkový kód využívá výsledky hledání toooutput hello konzoly.</span><span class="sxs-lookup"><span data-stu-id="86906-155">hello sample code above uses hello console toooutput search results.</span></span> <span data-ttu-id="86906-156">Stejně tak budete potřebovat toodisplay výsledků vyhledávání ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86906-156">You will likewise need toodisplay search results in your own application.</span></span> <span data-ttu-id="86906-157">V tématu [této ukázce na Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) příklad jak výsledky hledání toorender založené na ASP.NET MVC webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="86906-157">See [this sample on GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) for an example of how toorender search results in an ASP.NET MVC-based web application.</span></span>

