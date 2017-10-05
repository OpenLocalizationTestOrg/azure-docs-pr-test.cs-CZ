---
title: "Dotazování indexu (REST API – Azure Search) | Dokumentace Microsoftu"
description: "Sestavení vyhledávacího dotazu ve službě Azure Search a použití parametrů hledání k filtrování a řazení výsledků vyhledávání."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/12/2017
ms.author: ashmaka
ms.openlocfilehash: 49062bec233ad35cd457f9665fa94c1855343582
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="query-your-azure-search-index-using-the-rest-api"></a><span data-ttu-id="af2bc-103">Dotazování indexu Azure Search pomocí REST API</span><span class="sxs-lookup"><span data-stu-id="af2bc-103">Query your Azure Search index using the REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="af2bc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="af2bc-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="af2bc-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="af2bc-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="af2bc-106">.NET</span><span class="sxs-lookup"><span data-stu-id="af2bc-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="af2bc-107">REST</span><span class="sxs-lookup"><span data-stu-id="af2bc-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="af2bc-108">Tento článek vám ukáže postup dotazování indexu pomocí [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="af2bc-108">This article shows you how to query an index using the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="af2bc-109">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="af2bc-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="af2bc-110">Rozšiřující informace najdete v tématu popisujícím [způsob fungování fulltextového vyhledávání ve službě Azure Search](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="af2bc-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="af2bc-111">Zjistěte klíč api-key správce služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="af2bc-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="af2bc-112">Klíčovou komponentou každé operace vyhledávání na REST API služby Azure Search je klíč *api-key*, který byl vygenerován pro službu, kterou jste zřídili.</span><span class="sxs-lookup"><span data-stu-id="af2bc-112">A key component of every search operation against the Azure Search REST API is the *api-key* that was generated for the service you provisioned.</span></span> <span data-ttu-id="af2bc-113">Platný klíč vytváří na základě žádosti vztah důvěryhodnosti mezi aplikací, která žádost odeslala, a službou, která ji zpracovává.</span><span class="sxs-lookup"><span data-stu-id="af2bc-113">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="af2bc-114">Pokud chcete najít klíče api-key svojí služby, přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="af2bc-114">To find your service's api-keys, you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="af2bc-115">Přejděte do okna služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="af2bc-115">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="af2bc-116">Klikněte na ikonu klíčů.</span><span class="sxs-lookup"><span data-stu-id="af2bc-116">Click the "Keys" icon</span></span>

<span data-ttu-id="af2bc-117">Vaše služba má *klíče správce* a *klíče dotazů*.</span><span class="sxs-lookup"><span data-stu-id="af2bc-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="af2bc-118">Primární a sekundární *klíče správce* udělují úplná práva ke všem operacím, včetně možnosti spravovat službu, vytvářet a odstraňovat indexy, indexery a zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="af2bc-118">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="af2bc-119">Existují dva klíče, takže pokud se rozhodnete znovu vygenerovat primární klíč, můžete dál používat sekundární klíč, a naopak.</span><span class="sxs-lookup"><span data-stu-id="af2bc-119">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="af2bc-120">Vaše *klíče dotazů* udělují přístup jen pro čtení k indexům a dokumentům a obvykle se distribuují klientským aplikacím, které vydávají požadavky hledání.</span><span class="sxs-lookup"><span data-stu-id="af2bc-120">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="af2bc-121">Pro účely dotazování indexu můžete použít jeden z klíčů dotazů.</span><span class="sxs-lookup"><span data-stu-id="af2bc-121">For the purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="af2bc-122">Pro dotazy lze použít i klíče správce, ale ve svých aplikacích byste měli používat klíče dotazů, což lépe odpovídá [Principu minimálního oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="af2bc-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="af2bc-123">Formulování dotazu</span><span class="sxs-lookup"><span data-stu-id="af2bc-123">Formulate your query</span></span>
<span data-ttu-id="af2bc-124">Existují dva způsoby [vyhledávání v indexu pomocí REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="af2bc-124">There are two ways to [search your index using the REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="af2bc-125">První způsob je vydání požadavku HTTP POST, kde parametry dotazu jsou určené v objektu JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="af2bc-125">One way is to issue an HTTP POST request where your query parameters are defined in a JSON object in the request body.</span></span> <span data-ttu-id="af2bc-126">Druhý způsob je vydání požadavku HTTP GET, kde parametry dotazu jsou určené v rámci URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="af2bc-126">The other way is to issue an HTTP GET request where your query parameters are defined within the request URL.</span></span> <span data-ttu-id="af2bc-127">Metoda POST má [mírnější omezení](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) velikosti parametrů dotazu než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="af2bc-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on the size of query parameters than GET.</span></span> <span data-ttu-id="af2bc-128">Z tohoto důvodu doporučujeme používat metodu POST, pokud pro vás neplatí zvláštní podmínky, kdy by bylo pohodlnější použití metody GET.</span><span class="sxs-lookup"><span data-stu-id="af2bc-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="af2bc-129">U metody POST i GET budete muset v URL požadavku poskytnout *název služby*, *název indexu* a správnou *verzi rozhraní API* (v době publikování tohoto dokumentu je aktuální verze rozhraní API `2016-09-01`).</span><span class="sxs-lookup"><span data-stu-id="af2bc-129">For both POST and GET, you need to provide your *service name*, *index name*, and the proper *API version* (the current API version is `2016-09-01` at the time of publishing this document) in the request URL.</span></span> <span data-ttu-id="af2bc-130">U metody GET zadáte parametry dotazu v rámci *řetězce dotazu* na konci adresy URL.</span><span class="sxs-lookup"><span data-stu-id="af2bc-130">For GET, the *query string* at the end of the URL is where you provide the query parameters.</span></span> <span data-ttu-id="af2bc-131">Formát URL vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="af2bc-131">See below for the URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="af2bc-132">Formát pro metodu POST je stejný, ale jako parametr řetězce dotazu obsahuje pouze api-version.</span><span class="sxs-lookup"><span data-stu-id="af2bc-132">The format for POST is the same, but with only api-version in the query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="af2bc-133">Ukázky dotazů</span><span class="sxs-lookup"><span data-stu-id="af2bc-133">Example Queries</span></span>
<span data-ttu-id="af2bc-134">Zde naleznete několik ukázky dotazů na index s názvem „hotels“.</span><span class="sxs-lookup"><span data-stu-id="af2bc-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="af2bc-135">Dotazy jsou ukázané ve formátech GET i POST.</span><span class="sxs-lookup"><span data-stu-id="af2bc-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="af2bc-136">Vyhledat výraz „budget“ v celém indexu a vrátit pouze pole `hotelName`:</span><span class="sxs-lookup"><span data-stu-id="af2bc-136">Search the entire index for the term 'budget' and return only the `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="af2bc-137">Použít na index filtr pro nalezení hotelů levnějších než 150 dolarů za noc a vrátit pole `hotelId` a `description`:</span><span class="sxs-lookup"><span data-stu-id="af2bc-137">Apply a filter to the index to find hotels cheaper than $150 per night, and return the `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="af2bc-138">Prohledat celý index, seřadit podle určitého pole (`lastRenovationDate`) v sestupném pořadí, vzít první dva výsledky a zobrazit pouze pole `hotelName` a `lastRenovationDate`:</span><span class="sxs-lookup"><span data-stu-id="af2bc-138">Search the entire index, order by a specific field (`lastRenovationDate`) in descending order, take the top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a><span data-ttu-id="af2bc-139">Odeslání požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="af2bc-139">Submit your HTTP request</span></span>
<span data-ttu-id="af2bc-140">Nyní, když jste formulovali dotaz jako součást URL požadavku HTTP (pro metodu GET) nebo textu požadavku HTTP (pro metodu POST), můžete definovat hlavičky požadavku a odeslat dotaz.</span><span class="sxs-lookup"><span data-stu-id="af2bc-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="af2bc-141">Požadavek a hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="af2bc-141">Request and Request Headers</span></span>
<span data-ttu-id="af2bc-142">Musíte definovat dvě hlavičky požadavku pro metodu GET, nebo tři hlavičky pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="af2bc-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="af2bc-143">Hlavičku `api-key` je nutné nastavit na klíč dotazu, který jste získali v kroku I.</span><span class="sxs-lookup"><span data-stu-id="af2bc-143">The `api-key` header must be set to the query key you found in step I above.</span></span> <span data-ttu-id="af2bc-144">Jako hlavičku `api-key` můžete použít i klíč správce, ale doporučujeme používat klíč dotazů, protože uděluje přístup k indexům a dokumentům výhradně pouze pro čtení.</span><span class="sxs-lookup"><span data-stu-id="af2bc-144">You can also use an admin key as the `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access to indexes and documents.</span></span>
2. <span data-ttu-id="af2bc-145">Hlavička `Accept` musí být nastavená na `application/json`.</span><span class="sxs-lookup"><span data-stu-id="af2bc-145">The `Accept` header must be set to `application/json`.</span></span>
3. <span data-ttu-id="af2bc-146">U metody POST by měla být hlavička `Content-Type` také nastavená na `application/json`.</span><span class="sxs-lookup"><span data-stu-id="af2bc-146">For POST only, the `Content-Type` header should also be set to `application/json`.</span></span>

<span data-ttu-id="af2bc-147">Níže vidíte požadavek HTTP GET s jednoduchým dotazem, který vyhledá výraz „motel“ v indexu „hotels“ pomocí REST API služby Azure Search:</span><span class="sxs-lookup"><span data-stu-id="af2bc-147">See below for an HTTP GET request to search the "hotels" index using the Azure Search REST API, using a simple query that searches for the term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="af2bc-148">Zde je stejný vzorový dotaz, tentokrát pomocí HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="af2bc-148">Here is the same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="af2bc-149">Po úspěšném požadavku dotazu nastane stavový kód `200 OK` a výsledky vyhledávání se vrátí jako JSON v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="af2bc-149">A successful query request will result in a Status Code of `200 OK` and the search results are returned as JSON in the response body.</span></span> <span data-ttu-id="af2bc-150">Zde vidíte, jak vypadají výsledky dotazů pro výše uvedený kód za předpokladu, že je index „hotels“ naplněný vzorovými daty v tématu [Import Dat do služby Azure Search pomocí REST API](search-import-data-rest-api.md) (všimněte si, že byl JSON pro přehlednost zformátován):</span><span class="sxs-lookup"><span data-stu-id="af2bc-150">Here is what the results for the above query look like, assuming the "hotels" index is populated with the sample data in [Data Import in Azure Search using the REST API](search-import-data-rest-api.md) (note that the JSON has been formatted for clarity).</span></span>

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

<span data-ttu-id="af2bc-151">Zjistěte více v sekci „Odpověď“ tématu [Vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="af2bc-151">To learn more, please visit the "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="af2bc-152">Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="af2bc-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
