---
title: "AAA \"dotazování indexu (rozhraní REST - API Azure Search) | Microsoft Docs\""
description: "Sestavení vyhledávacího dotazu ve službě Azure search a pomocí vyhledávání parametrů toofilter a řazení výsledků vyhledávání."
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
ms.openlocfilehash: 2f12238b8f4b045f536489cfc8766fb68307bbe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-rest-api"></a><span data-ttu-id="b148a-103">Dotazování indexu Azure Search pomocí REST API hello</span><span class="sxs-lookup"><span data-stu-id="b148a-103">Query your Azure Search index using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="b148a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b148a-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="b148a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b148a-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="b148a-106">.NET</span><span class="sxs-lookup"><span data-stu-id="b148a-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="b148a-107">REST</span><span class="sxs-lookup"><span data-stu-id="b148a-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="b148a-108">Tento článek ukazuje, jak tooquery indexu pomocí hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="b148a-108">This article shows you how tooquery an index using hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="b148a-109">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="b148a-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="b148a-110">Rozšiřující informace najdete v tématu popisujícím [způsob fungování fulltextového vyhledávání ve službě Azure Search](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="b148a-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="b148a-111">Zjistěte klíč api-key správce služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="b148a-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="b148a-112">Klíčovou součástí každé operace vyhledávání na hello REST API služby Azure Search je hello *klíč api-key* který byl vygenerován pro službu hello jste zřídili.</span><span class="sxs-lookup"><span data-stu-id="b148a-112">A key component of every search operation against hello Azure Search REST API is hello *api-key* that was generated for hello service you provisioned.</span></span> <span data-ttu-id="b148a-113">Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="b148a-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="b148a-114">toofind klíče služby api Key, se můžete přihlásit toohello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="b148a-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="b148a-115">Okno služby Azure Search přejděte tooyour</span><span class="sxs-lookup"><span data-stu-id="b148a-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="b148a-116">Klikněte na ikonu hello "klíčů.</span><span class="sxs-lookup"><span data-stu-id="b148a-116">Click hello "Keys" icon</span></span>

<span data-ttu-id="b148a-117">Vaše služba má *klíče správce* a *klíče dotazů*.</span><span class="sxs-lookup"><span data-stu-id="b148a-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="b148a-118">Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat..</span><span class="sxs-lookup"><span data-stu-id="b148a-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="b148a-119">Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.</span><span class="sxs-lookup"><span data-stu-id="b148a-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="b148a-120">Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.</span><span class="sxs-lookup"><span data-stu-id="b148a-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="b148a-121">Pro účely hello dotazování indexu můžete použít jeden z klíčů dotazů.</span><span class="sxs-lookup"><span data-stu-id="b148a-121">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="b148a-122">Pro dotazy lze také použít klíče správce, ale byste měli používat klíče dotazů v kódu aplikace, což lépe odpovídá hello [Princip nejnižších nutných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="b148a-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="b148a-123">Formulování dotazu</span><span class="sxs-lookup"><span data-stu-id="b148a-123">Formulate your query</span></span>
<span data-ttu-id="b148a-124">Existují dva způsoby příliš[vyhledávání v indexu pomocí REST API hello](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="b148a-124">There are two ways too[search your index using hello REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="b148a-125">Jedním ze způsobů je tooissue požadavek HTTP POST, kde jsou definovány parametry dotazu v objektu JSON v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="b148a-125">One way is tooissue an HTTP POST request where your query parameters are defined in a JSON object in hello request body.</span></span> <span data-ttu-id="b148a-126">Hello jiný způsob je tooissue požadavek HTTP GET, kde jsou definovány parametry dotazu v adrese URL žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="b148a-126">hello other way is tooissue an HTTP GET request where your query parameters are defined within hello request URL.</span></span> <span data-ttu-id="b148a-127">Metoda POST má [mírnější omezení](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) na hello velikosti parametrů dotazu než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="b148a-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on hello size of query parameters than GET.</span></span> <span data-ttu-id="b148a-128">Z tohoto důvodu doporučujeme používat metodu POST, pokud pro vás neplatí zvláštní podmínky, kdy by bylo pohodlnější použití metody GET.</span><span class="sxs-lookup"><span data-stu-id="b148a-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="b148a-129">U metody POST i GET budete potřebovat tooprovide vaše *název služby*, *název indexu*a hello správné *verze rozhraní API* (aktuální verze rozhraní API hello je `2016-09-01` v době hello publikování tohoto dokumentu) v hello adresa URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="b148a-129">For both POST and GET, you need tooprovide your *service name*, *index name*, and hello proper *API version* (hello current API version is `2016-09-01` at hello time of publishing this document) in hello request URL.</span></span> <span data-ttu-id="b148a-130">U metody GET hello *řetězec dotazu* v hello je konec hello adresu URL, kde zadáte parametry dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="b148a-130">For GET, hello *query string* at hello end of hello URL is where you provide hello query parameters.</span></span> <span data-ttu-id="b148a-131">Formát adresy URL hello jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="b148a-131">See below for hello URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="b148a-132">Hello formátu pro POST je hello stejné, ale pouze api-version hello parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b148a-132">hello format for POST is hello same, but with only api-version in hello query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="b148a-133">Ukázky dotazů</span><span class="sxs-lookup"><span data-stu-id="b148a-133">Example Queries</span></span>
<span data-ttu-id="b148a-134">Zde naleznete několik ukázky dotazů na index s názvem „hotels“.</span><span class="sxs-lookup"><span data-stu-id="b148a-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="b148a-135">Dotazy jsou ukázané ve formátech GET i POST.</span><span class="sxs-lookup"><span data-stu-id="b148a-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="b148a-136">Vyhledejte hello výraz "budget" hello celý index a vrátí pouze hello `hotelName` pole:</span><span class="sxs-lookup"><span data-stu-id="b148a-136">Search hello entire index for hello term 'budget' and return only hello `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="b148a-137">Použít filtru toohello index toofind hotelů levnějších než 150 dolarů za noc a vrátit hello `hotelId` a `description`:</span><span class="sxs-lookup"><span data-stu-id="b148a-137">Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="b148a-138">Hledání hello celý index, řadit podle určitého pole (`lastRenovationDate`) v sestupném pořadí, vzít první dva výsledky hello a zobrazit pouze `hotelName` a `lastRenovationDate`:</span><span class="sxs-lookup"><span data-stu-id="b148a-138">Search hello entire index, order by a specific field (`lastRenovationDate`) in descending order, take hello top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

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

## <a name="submit-your-http-request"></a><span data-ttu-id="b148a-139">Odeslání požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="b148a-139">Submit your HTTP request</span></span>
<span data-ttu-id="b148a-140">Nyní, když jste formulovali dotaz jako součást URL požadavku HTTP (pro metodu GET) nebo textu požadavku HTTP (pro metodu POST), můžete definovat hlavičky požadavku a odeslat dotaz.</span><span class="sxs-lookup"><span data-stu-id="b148a-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="b148a-141">Požadavek a hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="b148a-141">Request and Request Headers</span></span>
<span data-ttu-id="b148a-142">Musíte definovat dvě hlavičky požadavku pro metodu GET, nebo tři hlavičky pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="b148a-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="b148a-143">Hello `api-key` záhlaví musí být nastaven klíč dotazu toohello jste získali v kroku I výše.</span><span class="sxs-lookup"><span data-stu-id="b148a-143">hello `api-key` header must be set toohello query key you found in step I above.</span></span> <span data-ttu-id="b148a-144">Můžete také použít klíč správce jako hello `api-key` záhlaví, ale doporučuje se používat klíče dotazů, protože výhradně uděluje oprávnění jen pro čtení tooindexes a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b148a-144">You can also use an admin key as hello `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access tooindexes and documents.</span></span>
2. <span data-ttu-id="b148a-145">Hello `Accept` záhlaví musí být nastaven příliš`application/json`.</span><span class="sxs-lookup"><span data-stu-id="b148a-145">hello `Accept` header must be set too`application/json`.</span></span>
3. <span data-ttu-id="b148a-146">U metody POST, hello `Content-Type` záhlaví měli nastavit také příliš`application/json`.</span><span class="sxs-lookup"><span data-stu-id="b148a-146">For POST only, hello `Content-Type` header should also be set too`application/json`.</span></span>

<span data-ttu-id="b148a-147">Níže najdete metody GET protokolu HTTP žádosti toosearch hello "hotels" indexu pomocí REST API služby Azure Search, s jednoduchým dotazem, který vyhledá hello výraz "motel" hello:</span><span class="sxs-lookup"><span data-stu-id="b148a-147">See below for an HTTP GET request toosearch hello "hotels" index using hello Azure Search REST API, using a simple query that searches for hello term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="b148a-148">Tady je hello stejný vzorový dotaz, tentokrát pomocí HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="b148a-148">Here is hello same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="b148a-149">Po úspěšném požadavku dotazu bude mít za následek stavový kód `200 OK` a hello výsledky vyhledávání se vrátí jako JSON v textu odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="b148a-149">A successful query request will result in a Status Code of `200 OK` and hello search results are returned as JSON in hello response body.</span></span> <span data-ttu-id="b148a-150">Tady je co hello výsledky pro hello výše dotazu vypadají, za předpokladu, že je index "hello"hotels"naplněný hello vzorovými daty v [hello Import dat do služby Azure Search pomocí REST API](search-import-data-rest-api.md) (Všimněte si, že hello JSON pro přehlednost zformátován).</span><span class="sxs-lookup"><span data-stu-id="b148a-150">Here is what hello results for hello above query look like, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello REST API](search-import-data-rest-api.md) (note that hello JSON has been formatted for clarity).</span></span>

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

<span data-ttu-id="b148a-151">toolearn víc, navštivte část "Odpověď" hello [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="b148a-151">toolearn more, please visit hello "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="b148a-152">Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="b148a-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
