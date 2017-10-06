---
title: "aaaHow toomanage souběžných zapíše tooresources ve službě Azure Search"
description: "Použijte optimistickou metodu souběžného tooavoid střední letecké kolize na aktualizace nebo odstranění tooAzure vyhledávací indexy, indexery, datových zdrojů."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: heidist
ms.openlocfilehash: c061d2b5c4d2dbd0fd5633405b01ab2912fbc754
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-concurrency-in-azure-search"></a><span data-ttu-id="51b81-103">Jak toomanage souběžnosti ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="51b81-103">How toomanage concurrency in Azure Search</span></span>

<span data-ttu-id="51b81-104">Při správě prostředků Azure Search, jako jsou indexy a zdroje dat, je důležité tooupdate prostředky bezpečně, zejména pokud prostředkům přistupuje souběžně různých komponent vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="51b81-104">When managing Azure Search resources such as indexes and data sources, it's important tooupdate resources safely, especially if resources are accessed concurrently by different components of your application.</span></span> <span data-ttu-id="51b81-105">Když se dvě klientů současně aktualizace prostředku bez spolupráce, je možné konflikty časování.</span><span class="sxs-lookup"><span data-stu-id="51b81-105">When two clients concurrently update a resource without coordination, race conditions are possible.</span></span> <span data-ttu-id="51b81-106">tooprevent se služba Azure Search nabízí *optimistickou metodu souběžného modelu*.</span><span class="sxs-lookup"><span data-stu-id="51b81-106">tooprevent this, Azure Search offers an *optimistic concurrency model*.</span></span> <span data-ttu-id="51b81-107">Neexistují žádné zámky na prostředek.</span><span class="sxs-lookup"><span data-stu-id="51b81-107">There are no locks on a resource.</span></span> <span data-ttu-id="51b81-108">Místo toho není značku ETag pro každý prostředek, který identifikuje verze hello prostředků tak, aby může vytvořit požadavků, které vyhnout náhodných přepíše.</span><span class="sxs-lookup"><span data-stu-id="51b81-108">Instead, there is an ETag for every resource that identifies hello resource version so that you can craft requests that avoid accidental overwrites.</span></span>

> [!Tip]
> <span data-ttu-id="51b81-109">Koncepční kód [ukázkové C# řešení](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) vysvětluje, jak funguje řízení souběžnosti ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="51b81-109">Conceptual code in a [sample C# solution](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) explains how concurrency control works in Azure Search.</span></span> <span data-ttu-id="51b81-110">Hello kód vytvoří podmínky, které vyvolají Kontrola souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="51b81-110">hello code creates conditions that invoke concurrency control.</span></span> <span data-ttu-id="51b81-111">Čtení hello [fragment kódu níže](#samplecode) je pravděpodobně dostatečná pro většinu vývojáře, ale pokud budete chtít toorun ho, název služby hello tooadd appSettings.JSON určený upravit a rozhraní api – klíč správce.</span><span class="sxs-lookup"><span data-stu-id="51b81-111">Reading hello [code fragment below](#samplecode) is probably sufficient for most developers, but if you want toorun it, edit appsettings.json tooadd hello service name and an admin api-key.</span></span> <span data-ttu-id="51b81-112">Danou adresu URL služby `http://myservice.search.windows.net`, je název služby hello `myservice`.</span><span class="sxs-lookup"><span data-stu-id="51b81-112">Given a service URL of `http://myservice.search.windows.net`, hello service name is `myservice`.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="51b81-113">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="51b81-113">How it works</span></span>

<span data-ttu-id="51b81-114">Optimistické, že je implementováno souběžnosti prostřednictvím přístupu podmínka kontroluje při voláních rozhraní API zápisu tooindexes, indexery, zdrojů dat a synonymMap prostředky.</span><span class="sxs-lookup"><span data-stu-id="51b81-114">Optimistic concurrency is implemented through access condition checks in API calls writing tooindexes, indexers, datasources, and synonymMap resources.</span></span> 

<span data-ttu-id="51b81-115">Všechny prostředky mít [ *entity tag (značka ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) poskytující informace o verzi objektu.</span><span class="sxs-lookup"><span data-stu-id="51b81-115">All resources have an [*entity tag (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) that provides object version information.</span></span> <span data-ttu-id="51b81-116">Kontrolou nejprve hello značka ETag, se můžete vyhnout Souběžná aktualizace v obvyklý pracovní postup (získat, upravte místně, aktualizujte) zajištěním hello prostředků značka ETag odpovídá vaší místní kopie.</span><span class="sxs-lookup"><span data-stu-id="51b81-116">By checking hello ETag first, you can avoid concurrent updates in a typical workflow (get, modify locally, update) by ensuring hello resource's ETag matches your local copy.</span></span> 

+ <span data-ttu-id="51b81-117">Hello REST API používá [značka ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) v hlavičce požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="51b81-117">hello REST API uses an [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello request header.</span></span>
+ <span data-ttu-id="51b81-118">Hello .NET SDK nastaví hello ETag prostřednictvím objekt accessCondition nastavení hello [If-Match | Záhlaví If-Match-žádný](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) na hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="51b81-118">hello .NET SDK sets hello ETag through an accessCondition object, setting hello [If-Match | If-Match-None header](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello resource.</span></span> <span data-ttu-id="51b81-119">Libovolného objektu, která dědí z [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) má accessCondition objekt.</span><span class="sxs-lookup"><span data-stu-id="51b81-119">Any object inheriting from [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) has an accessCondition object.</span></span>

<span data-ttu-id="51b81-120">Při každé aktualizaci prostředek automaticky změní jeho značka ETag.</span><span class="sxs-lookup"><span data-stu-id="51b81-120">Every time you update a resource, its ETag changes automatically.</span></span> <span data-ttu-id="51b81-121">Při implementaci správy souběžnosti všechny vaše změny se uvedení předběžné podmínky na hello žádost o aktualizaci, která vyžaduje hello vzdálený prostředek toohave hello stejné ETag jako kopii hello hello prostředku, který změnil na klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="51b81-121">When you implement concurrency management, all you're doing is putting a precondition on hello update request that requires hello remote resource toohave hello same ETag as hello copy of hello resource that you modified on hello client.</span></span> <span data-ttu-id="51b81-122">Pokud již souběžných procesů změnila hello vzdálený prostředek, hello ETag nebude odpovídat hello předběžnou a hello požadavek selže s HTTP 412.</span><span class="sxs-lookup"><span data-stu-id="51b81-122">If a concurrent process has changed hello remote resource already, hello ETag will not match hello precondition and hello request will fail with HTTP 412.</span></span> <span data-ttu-id="51b81-123">Pokud používáte hello .NET SDK, manifesty jako `CloudException` kde hello `IsAccessConditionFailed()` rozšíření metoda vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="51b81-123">If you're using hello .NET SDK, this manifests as a `CloudException` where hello `IsAccessConditionFailed()` extension method returns true.</span></span>

> [!Note]
> <span data-ttu-id="51b81-124">Existuje pouze jeden mechanismus pro souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="51b81-124">There is only one mechanism for concurrency.</span></span> <span data-ttu-id="51b81-125">Používá se vždy, bez ohledu na to, který se používá rozhraní API pro aktualizace prostředků.</span><span class="sxs-lookup"><span data-stu-id="51b81-125">It's always used regardless of which API is used for resource updates.</span></span> 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a><span data-ttu-id="51b81-126">Případy použití a ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="51b81-126">Use cases and sample code</span></span>

<span data-ttu-id="51b81-127">Hello následující kód ukazuje, že accessCondition vyhledává operace klíče aktualizace:</span><span class="sxs-lookup"><span data-stu-id="51b81-127">hello following code demonstrates accessCondition checks for key update operations:</span></span>

+ <span data-ttu-id="51b81-128">Aktualizace nezdaří, pokud hello prostředků už existuje.</span><span class="sxs-lookup"><span data-stu-id="51b81-128">Fail an update if hello resource no longer exists</span></span>
+ <span data-ttu-id="51b81-129">Pokud verze prostředků hello změní selhat aktualizace</span><span class="sxs-lookup"><span data-stu-id="51b81-129">Fail an update if hello resource version changes</span></span>

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a><span data-ttu-id="51b81-130">Ukázkový kód z [DotNetETagsExplainer programu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span><span class="sxs-lookup"><span data-stu-id="51b81-130">Sample code from [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span></span>

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Search has an associated ETag that keeps track of which version
            // of hello resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once hello resource exists in Azure Search, its ETag will be populated. Make sure toouse hello object
            // returned by hello SearchServiceClient! Otherwise, you will still have hello old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that hello update will only
            // succeed if hello index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates toohello same resource. If another
            // client tries tooupdate hello resource, it will fail as long as all clients are using hello right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. toostart, both clients see hello same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates hello index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries tooupdate hello index, but fails, thanks toohello ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2, 
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed tooupdate hello index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of hello DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using hello Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // hello resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key tooend application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a><span data-ttu-id="51b81-131">Vzor návrhu</span><span class="sxs-lookup"><span data-stu-id="51b81-131">Design pattern</span></span>

<span data-ttu-id="51b81-132">Vzor návrhu pro implementaci optimistickou metodu souběžného by měla obsahovat smyčku, která opakování hello podmínku kontroly přístupu testu pro podmínku hello přístup a volitelně načte aktualizovaný prostředek před pokusem o toore-změny hello.</span><span class="sxs-lookup"><span data-stu-id="51b81-132">A design pattern for implementing optimistic concurrency should include a loop that retries hello access condition check, a test for hello access condition, and optionally retrieves an updated resource before attempting toore-apply hello changes.</span></span> 

<span data-ttu-id="51b81-133">Tento fragment kódu ukazuje hello přidání index tooan synonymMap, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="51b81-133">This code snippet illustrates hello addition of a synonymMap tooan index that already exists.</span></span> <span data-ttu-id="51b81-134">Tento kód je z hello [synonymum (preview) C# – tutoriál pro službu Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span><span class="sxs-lookup"><span data-stu-id="51b81-134">This code is from hello [Synonym (preview) C# tutorial for Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span></span> 

<span data-ttu-id="51b81-135">fragment kódu Hello získá index hello "hotels", zkontroluje verzi objektu hello na operaci aktualizace, vyvolá výjimku, pokud selže hello podmínka, a pak hello opakování operace (až toothree dobu), od indexu z načtení hello server tooget hello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="51b81-135">hello snippet gets hello "hotels" index, checks hello object version on an update operation, throws an exception if hello condition fails, and then retries hello operation (up toothree times), starting with index retrieval from hello server tooget hello latest version.</span></span>

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // hello IfNotChanged condition ensures that hello index is updated only if hello ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated hello index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a><span data-ttu-id="51b81-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51b81-136">Next steps</span></span>

<span data-ttu-id="51b81-137">Zkontrolujte hello [synonyma C# ukázka](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) pro další kontext na tom, jak toosafely aktualizovat existující index.</span><span class="sxs-lookup"><span data-stu-id="51b81-137">Review hello [synonyms C# sample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) for more context on how toosafely update an existing index.</span></span>

<span data-ttu-id="51b81-138">Zkuste upravit buď hello následující ukázky tooinclude značky etag binárním rozsáhlým nebo AccessCondition objektů.</span><span class="sxs-lookup"><span data-stu-id="51b81-138">Try modifying either of hello following samples tooinclude ETags or AccessCondition objects.</span></span>

+ [<span data-ttu-id="51b81-139">Rozhraní REST API ukázce na Githubu</span><span class="sxs-lookup"><span data-stu-id="51b81-139">REST API sample on Github</span></span>](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ <span data-ttu-id="51b81-140">[Ukázka sady .NET SDK na Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="51b81-140">[.NET SDK sample on Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span></span> <span data-ttu-id="51b81-141">Toto řešení zahrnuje projekt "DotNetEtagsExplainer" hello obsahující hello kód uvedený v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="51b81-141">This solution includes hello "DotNetEtagsExplainer" project containing hello code presented in this article.</span></span>

## <a name="see-also"></a><span data-ttu-id="51b81-142">Viz také</span><span class="sxs-lookup"><span data-stu-id="51b81-142">See also</span></span>

  <span data-ttu-id="51b81-143">[Společné hlavičky požadavku a odpovědi HTTP](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span><span class="sxs-lookup"><span data-stu-id="51b81-143">[Common HTTP request and response headers](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span></span>  
  <span data-ttu-id="51b81-144">[Stavové kódy HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [indexu operací (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span><span class="sxs-lookup"><span data-stu-id="51b81-144">[HTTP status codes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index operations (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span></span>