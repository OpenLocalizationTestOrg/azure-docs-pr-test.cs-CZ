---
title: "Dotazování indexu (.NET API – Azure Search) | Dokumentace Microsoftu"
description: "Sestavení vyhledávacího dotazu ve službě Azure Search a použití parametrů hledání k filtrování a řazení výsledků vyhledávání."
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
ms.openlocfilehash: 52bd0fd4cf70401dcf881c7f28d5cd91397bb059
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="query-your-azure-search-index-using-the-net-sdk"></a><span data-ttu-id="3d3f2-103">Dotazování indexu Azure Search pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="3d3f2-103">Query your Azure Search index using the .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d3f2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3d3f2-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="3d3f2-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3d3f2-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="3d3f2-106">.NET</span><span class="sxs-lookup"><span data-stu-id="3d3f2-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="3d3f2-107">REST</span><span class="sxs-lookup"><span data-stu-id="3d3f2-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="3d3f2-108">Tento článek vám ukáže postup dotazování indexu pomocí [.NET SDK služby Azure Search](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-108">This article will show you how to query an index using the [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span>

<span data-ttu-id="3d3f2-109">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3d3f2-110">Ukázkový kód v tomto článku je napsán v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="3d3f2-111">Úplný zdrojový kód najdete [na GitHubu](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-111">You can find the full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="3d3f2-112">Můžete si také přečíst článek o sadě [Azure Search .NET SDK](search-howto-dotnet-sdk.md), který vás podrobněji provede ukázkovým kódem.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-112">You can also read about the [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of the sample code.</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="3d3f2-113">Zjistěte klíč api-key správce služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="3d3f2-113">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="3d3f2-114">Po vytvoření indexu Azure Search jste již téměř připraveni vydávat dotazy pomocí .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-114">Now that you have created an Azure Search index, you are almost ready to issue queries using the .NET SDK.</span></span> <span data-ttu-id="3d3f2-115">Nejprve budete muset získat jeden z klíčů dotazů (api-key) vytvořených pro vyhledávací službu, kterou jste zřídili.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-115">First, you will need to obtain one of the query api-keys that was generated for the search service you provisioned.</span></span> <span data-ttu-id="3d3f2-116">.NET SDK bude tento klíč api-key odesílat v každém požadavku na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-116">The .NET SDK will send this api-key on every request to your service.</span></span> <span data-ttu-id="3d3f2-117">Platný klíč vytváří na základě žádosti vztah důvěryhodnosti mezi aplikací, která žádost odeslala, a službou, která ji zpracovává.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-117">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="3d3f2-118">Pokud chcete najít klíče api-key svojí služby, přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-118">To find your service's api-keys you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="3d3f2-119">Přejděte do okna služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-119">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="3d3f2-120">Klikněte na ikonu klíčů.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-120">Click on the "Keys" icon</span></span>

<span data-ttu-id="3d3f2-121">Vaše služba bude mít *klíče správce* a *klíče dotazů*.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-121">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="3d3f2-122">Primární a sekundární *klíče správce* udělují úplná práva ke všem operacím, včetně možnosti spravovat službu, vytvářet a odstraňovat indexy, indexery a zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-122">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="3d3f2-123">Existují dva klíče, takže pokud se rozhodnete znovu vygenerovat primární klíč, můžete dál používat sekundární klíč, a naopak.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-123">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="3d3f2-124">Vaše *klíče dotazů* udělují přístup jen pro čtení k indexům a dokumentům a obvykle se distribuují klientským aplikacím, které vydávají požadavky hledání.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-124">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="3d3f2-125">Pro účely dotazování indexu můžete použít jeden z klíčů dotazů.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-125">For the purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="3d3f2-126">Pro dotazy lze použít i klíče správce, ale ve svých aplikacích byste měli používat klíče dotazů, což lépe odpovídá [Principu minimálního oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-126">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="create-an-instance-of-the-searchindexclient-class"></a><span data-ttu-id="3d3f2-127">Vytvoření instance třídy SearchIndexClient</span><span class="sxs-lookup"><span data-stu-id="3d3f2-127">Create an instance of the SearchIndexClient class</span></span>
<span data-ttu-id="3d3f2-128">Chcete-li vydávat dotazy pomocí .NET SDK služby Azure Search, budete muset vytvořit instanci třídy `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-128">To issue queries with the Azure Search .NET SDK, you will need to create an instance of the `SearchIndexClient` class.</span></span> <span data-ttu-id="3d3f2-129">Tato třída obsahuje několik konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-129">This class has several constructors.</span></span> <span data-ttu-id="3d3f2-130">Ten, který chcete, přijímá jako parametry název vaší vyhledávací služby, název indexu a objekt `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-130">The one you want takes your search service name, index name, and a `SearchCredentials` object as parameters.</span></span> <span data-ttu-id="3d3f2-131">`SearchCredentials` zabalí váš klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-131">`SearchCredentials` wraps your api-key.</span></span>

<span data-ttu-id="3d3f2-132">Následující kód vytvoří novou třídu `SearchIndexClient` pro index „hotels“ (vytvořený v tématu [Vytvoření indexu Azure Search pomocí .NET SDK](search-create-index-dotnet.md)) pomocí hodnot pro název vyhledávací služby a klíče api-key, které jsou uložené v konfiguračním souboru aplikace (v případě [ukázkové aplikace](http://aka.ms/search-dotnet-howto) `appsettings.json`):</span><span class="sxs-lookup"><span data-stu-id="3d3f2-132">The code below creates a new `SearchIndexClient` for the "hotels" index (created in [Create an Azure Search index using the .NET SDK](search-create-index-dotnet.md)) using values for the search service name and api-key that are stored in the application's config file (`appsettings.json` in the case of the [sample application](http://aka.ms/search-dotnet-howto)):</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="3d3f2-133">`SearchIndexClient` obsahuje vlastnost `Documents`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-133">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="3d3f2-134">Tato vlastnost poskytuje všechny metody, které potřebujete k dotazování indexů Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-134">This property provides all the methods you need to query Azure Search indexes.</span></span>

## <a name="query-your-index"></a><span data-ttu-id="3d3f2-135">Dotázání indexu</span><span class="sxs-lookup"><span data-stu-id="3d3f2-135">Query your index</span></span>
<span data-ttu-id="3d3f2-136">Vyhledávání pomocí .NET SDK je jednoduché, stačí na `SearchIndexClient` zavolat metodu `Documents.Search`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-136">Searching with the .NET SDK is as simple as calling the `Documents.Search` method on your `SearchIndexClient`.</span></span> <span data-ttu-id="3d3f2-137">Tato metoda přijímá několik parametrů včetně textu vyhledávání, spolu s objektem `SearchParameters`, který lze použít pro další upřesnění dotazu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-137">This method takes a few parameters, including the search text, along with a `SearchParameters` object that can be used to further refine the query.</span></span>

#### <a name="types-of-queries"></a><span data-ttu-id="3d3f2-138">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="3d3f2-138">Types of Queries</span></span>
<span data-ttu-id="3d3f2-139">Dva hlavní [typy dotazů](search-query-overview.md#types-of-queries), které budete používat, jsou `search` a `filter`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-139">The two main [query types](search-query-overview.md#types-of-queries) you will use are `search` and `filter`.</span></span> <span data-ttu-id="3d3f2-140">Dotaz `search` vyhledává jeden nebo více výrazů ve všech *prohledávatelných* polích v indexu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-140">A `search` query searches for one or more terms in all *searchable* fields in your index.</span></span> <span data-ttu-id="3d3f2-141">Dotaz `filter` vyhodnocuje logický výraz na všech *filtrovatelných* polích v indexu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-141">A `filter` query evaluates a boolean expression over all *filterable* fields in an index.</span></span>

<span data-ttu-id="3d3f2-142">Vyhledávání i filtrování se provádí pomocí metody `Documents.Search`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-142">Both searches and filters are performed using the `Documents.Search` method.</span></span> <span data-ttu-id="3d3f2-143">Vyhledávací dotaz lze předat v parametru `searchText`, zatímco výraz filtru lze předat ve vlastnosti `Filter` třídy `SearchParameters`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-143">A search query can be passed in the `searchText` parameter, while a filter expression can be passed in the `Filter` property of the `SearchParameters` class.</span></span> <span data-ttu-id="3d3f2-144">Chcete-li filtrovat bez vyhledávání, stačí předat `"*"` jako hodnotu parametru `searchText`.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-144">To filter without searching, just pass `"*"` for the `searchText` parameter.</span></span> <span data-ttu-id="3d3f2-145">Chcete-li vyhledávat bez filtrování, ponechte vlastnost `Filter` nenastavenou nebo instanci `SearchParameters` vůbec nepředávejte.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-145">To search without filtering, just leave the `Filter` property unset, or do not pass in a `SearchParameters` instance at all.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="3d3f2-146">Ukázky dotazů</span><span class="sxs-lookup"><span data-stu-id="3d3f2-146">Example Queries</span></span>
<span data-ttu-id="3d3f2-147">Následující vzorový kód ukazuje několik různých způsobů dotazování indexu „hotels“, definovaného v tématu [Vytvoření indexu Azure Search pomocí .NET SDK](search-create-index-dotnet.md#DefineIndex).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-147">The following sample code shows a few different ways to query the "hotels" index defined in [Create an Azure Search index using the .NET SDK](search-create-index-dotnet.md#DefineIndex).</span></span> <span data-ttu-id="3d3f2-148">Všimněte si, že dokumenty vrácené ve výsledcích vyhledávání jsou instancemi třídy `Hotel`, která byla definovaná v tématu [Import dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md#HotelClass).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-148">Note that the documents returned with the search results are instances of the `Hotel` class, which was defined in [Data Import in Azure Search using the .NET SDK](search-import-data-dotnet.md#HotelClass).</span></span> <span data-ttu-id="3d3f2-149">Ukázkový kód využívá metody `WriteDocuments` k vypsání výsledků vyhledávání do konzoly.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-149">The sample code makes use of a `WriteDocuments` method to output the search results to the console.</span></span> <span data-ttu-id="3d3f2-150">Tato metoda je popsaná v následujícím oddílu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-150">This method is described in the next section.</span></span>

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
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

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a><span data-ttu-id="3d3f2-151">Zpracování výsledků vyhledávání</span><span class="sxs-lookup"><span data-stu-id="3d3f2-151">Handle search results</span></span>
<span data-ttu-id="3d3f2-152">Metoda `Documents.Search` vrací objekt `DocumentSearchResult`, který obsahuje výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-152">The `Documents.Search` method returns a `DocumentSearchResult` object that contains the results of the query.</span></span> <span data-ttu-id="3d3f2-153">Ukázka v předchozím oddílu používala pro vypsání výsledků vyhledávání do konzoly metodu s názvem `WriteDocuments`:</span><span class="sxs-lookup"><span data-stu-id="3d3f2-153">The example in the previous section used a method called `WriteDocuments` to output the search results to the console:</span></span>

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

<span data-ttu-id="3d3f2-154">Zde vidíte, jak vypadají výsledky dotazů v předchozím oddílu za předpokladu, že je index „hotels“ naplněný vzorovými daty v tématu [Import Dat do služby Azure Search pomocí .NET SDK](search-import-data-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="3d3f2-154">Here is what the results look like for the queries in the previous section, assuming the "hotels" index is populated with the sample data in [Data Import in Azure Search using the .NET SDK](search-import-data-dotnet.md):</span></span>

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

<span data-ttu-id="3d3f2-155">Výše uvedený ukázkový kód používá k vypsání výsledků vyhledávání konzolu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-155">The sample code above uses the console to output search results.</span></span> <span data-ttu-id="3d3f2-156">Stejně tak budete potřebovat zobrazit výsledky vyhledávání ve své aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-156">You will likewise need to display search results in your own application.</span></span> <span data-ttu-id="3d3f2-157">Ukázku vykreslování výsledků vyhledávání ve webové aplikaci založené na ASP.NET MVC naleznete v [této ukázce na GitHubu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample).</span><span class="sxs-lookup"><span data-stu-id="3d3f2-157">See [this sample on GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) for an example of how to render search results in an ASP.NET MVC-based web application.</span></span>

