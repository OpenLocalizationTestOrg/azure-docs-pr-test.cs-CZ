---
title: "aaaHow toouse Fiddler tooevaluate a testování rozhraní REST API Azure Search | Microsoft Docs"
description: "Použijte aplikaci Fiddler s dostupností Azure Search bezkódovou tooverifying a vyzkoušení hello rozhraní REST API."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: 2912e1180717d7b40a1e4f7f7f00daf2cc254f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a><span data-ttu-id="df49f-103">Použijte aplikaci Fiddler tooevaluate a testování rozhraní REST API Azure Search</span><span class="sxs-lookup"><span data-stu-id="df49f-103">Use Fiddler tooevaluate and test Azure Search REST APIs</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="df49f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="df49f-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="df49f-105">Průzkumník služby Search</span><span class="sxs-lookup"><span data-stu-id="df49f-105">Search Explorer</span></span>](search-explorer.md)
> * [<span data-ttu-id="df49f-106">Fiddler</span><span class="sxs-lookup"><span data-stu-id="df49f-106">Fiddler</span></span>](search-fiddler.md)
> * [<span data-ttu-id="df49f-107">.NET</span><span class="sxs-lookup"><span data-stu-id="df49f-107">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="df49f-108">REST</span><span class="sxs-lookup"><span data-stu-id="df49f-108">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="df49f-109">Tento článek vysvětluje, jak toouse aplikaci Fiddler, je dostupná [zdarma ke stažení na webu Telerik](http://www.telerik.com/fiddler), tooissue HTTP požadavků tooand zobrazení odpovědí pomocí hello REST API služby Azure Search, bez nutnosti toowrite žádný kód.</span><span class="sxs-lookup"><span data-stu-id="df49f-109">This article explains how toouse Fiddler, available as a [free download from Telerik](http://www.telerik.com/fiddler), tooissue HTTP requests tooand view responses using hello Azure Search REST API, without having toowrite any code.</span></span> <span data-ttu-id="df49f-110">Azure Search je plně spravovaná, hostovaná cloudová vyhledávací služba v Microsoft Azure, která je snadno programovatelná prostřednictvím rozhraní .NET a REST API.</span><span class="sxs-lookup"><span data-stu-id="df49f-110">Azure Search is fully-managed, hosted cloud search service on Microsoft Azure, easily programmable through .NET and REST APIs.</span></span> <span data-ttu-id="df49f-111">Hello služby Azure Search jsou popsaná rozhraní REST API na [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span><span class="sxs-lookup"><span data-stu-id="df49f-111">hello Azure Search service REST APIs are documented on [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span></span>

<span data-ttu-id="df49f-112">V hello následující kroky budete vytvoření indexu, nahrávat dokumenty, dotazování hello indexu a systému hello dotazu informace o službě.</span><span class="sxs-lookup"><span data-stu-id="df49f-112">In hello following steps, you'll create an index, upload documents, query hello index, and then query hello system for service information.</span></span>

<span data-ttu-id="df49f-113">Tyto kroky toocomplete, budete potřebovat službu Azure Search a `api-key`.</span><span class="sxs-lookup"><span data-stu-id="df49f-113">toocomplete these steps, you will need an Azure Search service and `api-key`.</span></span> <span data-ttu-id="df49f-114">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) pokyny, jak tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="df49f-114">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for instructions on how tooget started.</span></span>

## <a name="create-an-index"></a><span data-ttu-id="df49f-115">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="df49f-115">Create an index</span></span>
1. <span data-ttu-id="df49f-116">Spusťte aplikaci Fiddler.</span><span class="sxs-lookup"><span data-stu-id="df49f-116">Start Fiddler.</span></span> <span data-ttu-id="df49f-117">Na hello **soubor** nabídce vypnout **zachycování provozu** toohide irelevantní aktivitu protokolu HTTP, je nesouvisejícími toohello aktuální úlohy.</span><span class="sxs-lookup"><span data-stu-id="df49f-117">On hello **File** menu, turn off **Capture Traffic** toohide extraneous HTTP activity that is unrelated toohello current task.</span></span>
2. <span data-ttu-id="df49f-118">Na hello **autora** kartě zformulujete požadavek, který vypadá jako hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="df49f-118">On hello **Composer** tab, you'll formulate a request that looks like hello following screen shot.</span></span>

      ![][1]
3. <span data-ttu-id="df49f-119">Vyberte **PUT**.</span><span class="sxs-lookup"><span data-stu-id="df49f-119">Select **PUT**.</span></span>
4. <span data-ttu-id="df49f-120">Zadejte adresu URL, která určuje adresu URL služby hello atributy požadavku a hello api-version.</span><span class="sxs-lookup"><span data-stu-id="df49f-120">Enter a URL that specifies hello service URL, request attributes, and hello api-version.</span></span> <span data-ttu-id="df49f-121">Několik tookeep ukazatele na paměti:</span><span class="sxs-lookup"><span data-stu-id="df49f-121">A few pointers tookeep in mind:</span></span>

   * <span data-ttu-id="df49f-122">Jako předpona hello použijte protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="df49f-122">Use HTTPS as hello prefix.</span></span>
   * <span data-ttu-id="df49f-123">Atribut žádosti je „/indexes/hotels“.</span><span class="sxs-lookup"><span data-stu-id="df49f-123">Request attribute is "/indexes/hotels".</span></span> <span data-ttu-id="df49f-124">Tato hodnota informuje vyhledávání toocreate index s názvem "hotely".</span><span class="sxs-lookup"><span data-stu-id="df49f-124">This tells Search toocreate an index named 'hotels'.</span></span>
   * <span data-ttu-id="df49f-125">Verze rozhraní API se zadává malými písmeny jako „?api-version=2016-09-01“.</span><span class="sxs-lookup"><span data-stu-id="df49f-125">Api-version is lowercase, specified as "?api-version=2016-09-01".</span></span> <span data-ttu-id="df49f-126">Verze rozhraní API jsou důležité, protože služba Azure Search pravidelně nasazuje aktualizace.</span><span class="sxs-lookup"><span data-stu-id="df49f-126">API versions are important because Azure Search deploys updates regularly.</span></span> <span data-ttu-id="df49f-127">Ve výjimečných případech mohou aktualizace služby zavést narušující změny toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df49f-127">On rare occasions, a service update may introduce a breaking change toohello API.</span></span> <span data-ttu-id="df49f-128">Z tohoto důvodu služba Azure Search v každé žádosti vyžaduje verzi rozhraní API, abyste měli plně pod kontrolou, která verze se použije.</span><span class="sxs-lookup"><span data-stu-id="df49f-128">For this reason, Azure Search requires an api-version on each request so that you are in full control over which one is used.</span></span>

     <span data-ttu-id="df49f-129">Hello úplnou adresu URL by měla vypadat podobně jako toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="df49f-129">hello full URL should look similar toohello following example.</span></span>

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. <span data-ttu-id="df49f-130">Zadejte hlavičku požadavku hello, nahraďte hello hostitele a klíč api-key hodnoty, které jsou platné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="df49f-130">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. <span data-ttu-id="df49f-131">V textu žádosti vložte pole hello, která tvoří definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="df49f-131">In Request Body, paste in hello fields that make up hello index definition.</span></span>

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. <span data-ttu-id="df49f-132">Klikněte na tlačítko **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="df49f-132">Click **Execute**.</span></span>

<span data-ttu-id="df49f-133">Během pár sekund měli byste vidět odpověď HTTP 201 v seznamu relace hello, naznačuje hello index úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="df49f-133">In a few seconds, you should see an HTTP 201 response in hello session list, indicating hello index was created successfully.</span></span>

<span data-ttu-id="df49f-134">Pokud se zobrazí kód HTTP 504, ověřte, zda že Hello URL určený protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="df49f-134">If you get HTTP 504, verify hello URL specifies HTTPS.</span></span> <span data-ttu-id="df49f-135">Pokud se zobrazí kód HTTP 400 nebo 404, zkontrolujte žádost hello textu tooverify došlo k chybám způsobené kopírováním a vkládáním.</span><span class="sxs-lookup"><span data-stu-id="df49f-135">If you see HTTP 400 or 404, check hello request body tooverify there were no copy-paste errors.</span></span> <span data-ttu-id="df49f-136">HTTP 403 obvykle znamená potíže s hello klíč api-key (neplatný klíč nebo syntaxe problém s jak je zadán hello klíč api-key).</span><span class="sxs-lookup"><span data-stu-id="df49f-136">An HTTP 403 typically indicates a problem with hello api-key (either an invalid key or a syntax problem with how hello api-key is specified).</span></span>

## <a name="load-documents"></a><span data-ttu-id="df49f-137">Nahrání dokumentů</span><span class="sxs-lookup"><span data-stu-id="df49f-137">Load documents</span></span>
<span data-ttu-id="df49f-138">Na hello **autora** kartě dokumentů toopost požadavek bude vypadat jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="df49f-138">On hello **Composer** tab, your request toopost documents will look like hello following.</span></span> <span data-ttu-id="df49f-139">text Hello hello žádosti obsahuje data hello hledání pro 4 hotely.</span><span class="sxs-lookup"><span data-stu-id="df49f-139">hello body of hello request contains hello search data for 4 hotels.</span></span>

   ![][2]

1. <span data-ttu-id="df49f-140">Vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="df49f-140">Select **POST**.</span></span>
2. <span data-ttu-id="df49f-141">Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby a „/indexes/<'indexname'>/docs/index?api-version=2016-09-01“.</span><span class="sxs-lookup"><span data-stu-id="df49f-141">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs/index?api-version=2016-09-01".</span></span> <span data-ttu-id="df49f-142">Hello úplnou adresu URL by měla vypadat podobně jako toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="df49f-142">hello full URL should look similar toohello following example.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. <span data-ttu-id="df49f-143">Hlavička žádosti by měla být hello stejné jako předtím.</span><span class="sxs-lookup"><span data-stu-id="df49f-143">Request Header should be hello same as before.</span></span> <span data-ttu-id="df49f-144">Nezapomeňte nahradit hello hostitele a klíč api-key s hodnotami, které jsou platné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="df49f-144">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="df49f-145">Hello text žádosti obsahuje čtyři dokumenty toobe přidané toohello hotels index.</span><span class="sxs-lookup"><span data-stu-id="df49f-145">hello Request Body contains four documents toobe added toohello hotels index.</span></span>

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be hello one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. <span data-ttu-id="df49f-146">Klikněte na tlačítko **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="df49f-146">Click **Execute**.</span></span>

<span data-ttu-id="df49f-147">Během pár sekund měli byste vidět odpověď HTTP 200 v seznamu relace hello.</span><span class="sxs-lookup"><span data-stu-id="df49f-147">In a few seconds, you should see an HTTP 200 response in hello session list.</span></span> <span data-ttu-id="df49f-148">To znamená hello dokumenty úspěšně vytvořily.</span><span class="sxs-lookup"><span data-stu-id="df49f-148">This indicates hello documents were created successfully.</span></span> <span data-ttu-id="df49f-149">Pokud se zobrazí kód 207, nejméně jeden dokument se nepovedlo tooupload.</span><span class="sxs-lookup"><span data-stu-id="df49f-149">If you get a 207, at least one document failed tooupload.</span></span> <span data-ttu-id="df49f-150">Pokud se zobrazí kód 404, máte chyby syntaxe v záhlaví hello nebo textu hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="df49f-150">If you get a 404, you have a syntax error in either hello header or body of hello request.</span></span>

## <a name="query-hello-index"></a><span data-ttu-id="df49f-151">Dotazování indexu hello</span><span class="sxs-lookup"><span data-stu-id="df49f-151">Query hello index</span></span>
<span data-ttu-id="df49f-152">Teď, když jsou index a dokumenty nahrané, můžete na ně vydávat dotazy.</span><span class="sxs-lookup"><span data-stu-id="df49f-152">Now that an index and documents are loaded, you can issue queries against them.</span></span>  <span data-ttu-id="df49f-153">Na hello **autora** kartě **získat** příkaz, který dotazuje službu bude vypadat podobně jako toohello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="df49f-153">On hello **Composer** tab, a **GET** command that queries your service will look similar toohello following screen shot.</span></span>

   ![][3]

1. <span data-ttu-id="df49f-154">Vyberte **GET**.</span><span class="sxs-lookup"><span data-stu-id="df49f-154">Select **GET**.</span></span>
2. <span data-ttu-id="df49f-155">Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby, „/indexes/<'indexname'>/docs?“ a nakonec parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="df49f-155">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs?", followed by query parameters.</span></span> <span data-ttu-id="df49f-156">Jako příklad použijte hello následující adresu URL, nahraďte název hostitele ukázka hello ten, který je platný pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="df49f-156">By way of example, use hello following URL, replacing hello sample host name with one that is valid for your service.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   <span data-ttu-id="df49f-157">Tento dotaz hledá hello výraz "motel" a načte kategorie faset pro hodnocení.</span><span class="sxs-lookup"><span data-stu-id="df49f-157">This query searches on hello term “motel” and retrieves facet categories for ratings.</span></span>
3. <span data-ttu-id="df49f-158">Hlavička žádosti by měla být hello stejné jako předtím.</span><span class="sxs-lookup"><span data-stu-id="df49f-158">Request Header should be hello same as before.</span></span> <span data-ttu-id="df49f-159">Nezapomeňte nahradit hello hostitele a klíč api-key s hodnotami, které jsou platné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="df49f-159">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

<span data-ttu-id="df49f-160">Kód odpovědi Hello by měla být 200 a hello odpověď by měla vypadat podobně jako toohello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="df49f-160">hello response code should be 200, and hello response output should look similar toohello following screen shot.</span></span>

   ![][4]

<span data-ttu-id="df49f-161">Hello následující příklad dotazu je převzatý z hello [operace prohledání indexu (rozhraní API Azure Search)](http://msdn.microsoft.com/library/dn798927.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="df49f-161">hello following example query is from hello [Search Index operation (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) on MSDN.</span></span> <span data-ttu-id="df49f-162">Řadu hello příklad dotazy v tomto tématu obsahuje mezery, které nejsou v aplikaci Fiddler povolené.</span><span class="sxs-lookup"><span data-stu-id="df49f-162">Many of hello example queries in this topic include spaces, which are not allowed in Fiddler.</span></span> <span data-ttu-id="df49f-163">Nahraďte každou mezeru znakem + před vkládání v hello řetězec dotazu před pokusem o hello dotazu v aplikaci Fiddler.</span><span class="sxs-lookup"><span data-stu-id="df49f-163">Replace each space with a + character before pasting in hello query string before attempting hello query in Fiddler.</span></span>

<span data-ttu-id="df49f-164">**Před nahrazením mezer:**</span><span class="sxs-lookup"><span data-stu-id="df49f-164">**Before spaces are replaced:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

<span data-ttu-id="df49f-165">**Po nahrazení mezer znakem +:**</span><span class="sxs-lookup"><span data-stu-id="df49f-165">**After spaces are replaced with +:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a><span data-ttu-id="df49f-166">Dotaz hello systému</span><span class="sxs-lookup"><span data-stu-id="df49f-166">Query hello system</span></span>
<span data-ttu-id="df49f-167">Můžete taky zadat dotaz hello systému tooget počty a úložiště využívání dokumentu.</span><span class="sxs-lookup"><span data-stu-id="df49f-167">You can also query hello system tooget document counts and storage consumption.</span></span> <span data-ttu-id="df49f-168">Na hello **autora** , požadavek bude vypadat podobně jako následující toohello a hello odpověď vrátí počet hello počet dokumentů a využité místo.</span><span class="sxs-lookup"><span data-stu-id="df49f-168">On hello **Composer** tab, your request will look similar toohello following, and hello response will return a count for hello number of documents and space used.</span></span>

 ![][5]

1. <span data-ttu-id="df49f-169">Vyberte **GET**.</span><span class="sxs-lookup"><span data-stu-id="df49f-169">Select **GET**.</span></span>
2. <span data-ttu-id="df49f-170">Zadejte adresu URL, která obsahuje adresu URL služby a potom „/indexes/hotels/stats?api-version=2016-09-01“:</span><span class="sxs-lookup"><span data-stu-id="df49f-170">Enter a URL that includes your service URL, followed by "/indexes/hotels/stats?api-version=2016-09-01":</span></span>

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. <span data-ttu-id="df49f-171">Zadejte hlavičku požadavku hello, nahraďte hello hostitele a klíč api-key hodnoty, které jsou platné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="df49f-171">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="df49f-172">Text žádosti hello ponechte prázdné.</span><span class="sxs-lookup"><span data-stu-id="df49f-172">Leave hello request body empty.</span></span>
5. <span data-ttu-id="df49f-173">Klikněte na tlačítko **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="df49f-173">Click **Execute**.</span></span> <span data-ttu-id="df49f-174">Měli byste vidět stavový kód HTTP 200 v seznamu relace hello.</span><span class="sxs-lookup"><span data-stu-id="df49f-174">You should see an HTTP 200 status code in hello session list.</span></span> <span data-ttu-id="df49f-175">Vyberte hello položku publikovanou k vašemu příkazu.</span><span class="sxs-lookup"><span data-stu-id="df49f-175">Select hello entry posted for your command.</span></span>
6. <span data-ttu-id="df49f-176">Klikněte na tlačítko hello **inspektoři** , klikněte na hello **hlavičky** kartu a pak vyberte hello formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="df49f-176">Click hello **Inspectors** tab, click hello **Headers** tab, and then select hello JSON format.</span></span> <span data-ttu-id="df49f-177">Měli byste vidět hello dokumentu počet a velikost úložiště (v KB).</span><span class="sxs-lookup"><span data-stu-id="df49f-177">You should see hello document count and storage size (in KB).</span></span>

## <a name="next-steps"></a><span data-ttu-id="df49f-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df49f-178">Next steps</span></span>
<span data-ttu-id="df49f-179">V tématu [Správa služby Search v Azure](search-manage.md) toomanaging žádný kód přístup a používání služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="df49f-179">See [Manage your Search service on Azure](search-manage.md) for a no-code approach toomanaging and using Azure Search.</span></span>

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
