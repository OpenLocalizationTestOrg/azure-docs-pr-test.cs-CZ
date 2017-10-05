---
title: "Jak spravovat souběžných zapíše k prostředkům ve službě Azure Search"
description: "Zabrání se tím kolizím střední letecké na aktualizace nebo odstranění indexů Azure Search, indexery, zdroje dat pomocí optimistickou metodu souběžného."
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
ms.openlocfilehash: aee1b7376d4829e3e2f5a232525e3c3cb4df9d8e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="how-to-manage-concurrency-in-azure-search"></a><span data-ttu-id="59198-103">Jak spravovat souběžnosti ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="59198-103">How to manage concurrency in Azure Search</span></span>

<span data-ttu-id="59198-104">Při správě prostředků Azure Search, jako jsou indexy a zdroje dat, je důležité aktualizace zdroje, bezpečně, zejména v případě, že prostředkům přistupuje souběžně různých komponent vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="59198-104">When managing Azure Search resources such as indexes and data sources, it's important to update resources safely, especially if resources are accessed concurrently by different components of your application.</span></span> <span data-ttu-id="59198-105">Když se dvě klientů současně aktualizace prostředku bez spolupráce, je možné konflikty časování.</span><span class="sxs-lookup"><span data-stu-id="59198-105">When two clients concurrently update a resource without coordination, race conditions are possible.</span></span> <span data-ttu-id="59198-106">Chcete-li tomu zabránit, Azure Search nabízí *optimistickou metodu souběžného modelu*.</span><span class="sxs-lookup"><span data-stu-id="59198-106">To prevent this, Azure Search offers an *optimistic concurrency model*.</span></span> <span data-ttu-id="59198-107">Neexistují žádné zámky na prostředek.</span><span class="sxs-lookup"><span data-stu-id="59198-107">There are no locks on a resource.</span></span> <span data-ttu-id="59198-108">Místo toho není značku ETag pro každý prostředek, který identifikuje prostředek verze tak, aby může vytvořit požadavků, které vyhnout náhodných přepíše.</span><span class="sxs-lookup"><span data-stu-id="59198-108">Instead, there is an ETag for every resource that identifies the resource version so that you can craft requests that avoid accidental overwrites.</span></span>

> [!Tip]
> <span data-ttu-id="59198-109">Koncepční kód [ukázkové C# řešení](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) vysvětluje, jak funguje řízení souběžnosti ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="59198-109">Conceptual code in a [sample C# solution](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) explains how concurrency control works in Azure Search.</span></span> <span data-ttu-id="59198-110">Kód vytvoří podmínky, které vyvolají Kontrola souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="59198-110">The code creates conditions that invoke concurrency control.</span></span> <span data-ttu-id="59198-111">Čtení [fragment kódu níže](#samplecode) je pravděpodobně dostatečná Většina vývojářů, ale pokud chcete spustit, upravit appSettings.JSON určený k přidání názvu služby a rozhraní api – klíč správce.</span><span class="sxs-lookup"><span data-stu-id="59198-111">Reading the [code fragment below](#samplecode) is probably sufficient for most developers, but if you want to run it, edit appsettings.json to add the service name and an admin api-key.</span></span> <span data-ttu-id="59198-112">Danou adresu URL služby `http://myservice.search.windows.net`, název služby je `myservice`.</span><span class="sxs-lookup"><span data-stu-id="59198-112">Given a service URL of `http://myservice.search.windows.net`, the service name is `myservice`.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="59198-113">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="59198-113">How it works</span></span>

<span data-ttu-id="59198-114">Optimistické, že je implementováno souběžnosti prostřednictvím přístupu podmínka kontroluje při voláních rozhraní API zápisu do indexy, indexery, zdrojů dat a synonymMap prostředky.</span><span class="sxs-lookup"><span data-stu-id="59198-114">Optimistic concurrency is implemented through access condition checks in API calls writing to indexes, indexers, datasources, and synonymMap resources.</span></span> 

<span data-ttu-id="59198-115">Všechny prostředky mít [ *entity tag (značka ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) poskytující informace o verzi objektu.</span><span class="sxs-lookup"><span data-stu-id="59198-115">All resources have an [*entity tag (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) that provides object version information.</span></span> <span data-ttu-id="59198-116">Kontrolou nejprve značku ETag, se můžete vyhnout Souběžná aktualizace v obvyklý pracovní postup (získat, upravte místně, aktualizujte) zajištěním prostředku značka ETag odpovídá vaší místní kopie.</span><span class="sxs-lookup"><span data-stu-id="59198-116">By checking the ETag first, you can avoid concurrent updates in a typical workflow (get, modify locally, update) by ensuring the resource's ETag matches your local copy.</span></span> 

+ <span data-ttu-id="59198-117">Používá rozhraní API REST [značka ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) v hlavičce žádosti.</span><span class="sxs-lookup"><span data-stu-id="59198-117">The REST API uses an [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on the request header.</span></span>
+ <span data-ttu-id="59198-118">.NET SDK nastaví značku ETag prostřednictvím objektu accessCondition, nastavení [If-Match | Záhlaví If-Match-žádný](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) u daného prostředku.</span><span class="sxs-lookup"><span data-stu-id="59198-118">The .NET SDK sets the ETag through an accessCondition object, setting the [If-Match | If-Match-None header](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on the resource.</span></span> <span data-ttu-id="59198-119">Libovolného objektu, která dědí z [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) má accessCondition objekt.</span><span class="sxs-lookup"><span data-stu-id="59198-119">Any object inheriting from [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) has an accessCondition object.</span></span>

<span data-ttu-id="59198-120">Při každé aktualizaci prostředek automaticky změní jeho značka ETag.</span><span class="sxs-lookup"><span data-stu-id="59198-120">Every time you update a resource, its ETag changes automatically.</span></span> <span data-ttu-id="59198-121">Při implementaci správy souběžnosti všechny, kdy provádíte je uvedení předběžné podmínky na žádost o aktualizaci, která vyžaduje vzdálený prostředek tak, aby měl stejné ETag jako kopii prostředku, který změnil na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="59198-121">When you implement concurrency management, all you're doing is putting a precondition on the update request that requires the remote resource to have the same ETag as the copy of the resource that you modified on the client.</span></span> <span data-ttu-id="59198-122">Pokud již souběžných procesů změnila vzdálený prostředek, značku ETag nebude odpovídat předběžnou podmínku a požadavek selže s HTTP 412.</span><span class="sxs-lookup"><span data-stu-id="59198-122">If a concurrent process has changed the remote resource already, the ETag will not match the precondition and the request will fail with HTTP 412.</span></span> <span data-ttu-id="59198-123">Pokud používáte sadu .NET SDK, manifesty jako `CloudException` kde `IsAccessConditionFailed()` rozšíření metoda vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="59198-123">If you're using the .NET SDK, this manifests as a `CloudException` where the `IsAccessConditionFailed()` extension method returns true.</span></span>

> [!Note]
> <span data-ttu-id="59198-124">Existuje pouze jeden mechanismus pro souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="59198-124">There is only one mechanism for concurrency.</span></span> <span data-ttu-id="59198-125">Používá se vždy, bez ohledu na to, který se používá rozhraní API pro aktualizace prostředků.</span><span class="sxs-lookup"><span data-stu-id="59198-125">It's always used regardless of which API is used for resource updates.</span></span> 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a><span data-ttu-id="59198-126">Případy použití a ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="59198-126">Use cases and sample code</span></span>

<span data-ttu-id="59198-127">Následující kód ukazuje, že accessCondition vyhledává operace klíče aktualizace:</span><span class="sxs-lookup"><span data-stu-id="59198-127">The following code demonstrates accessCondition checks for key update operations:</span></span>

+ <span data-ttu-id="59198-128">Aktualizace nezdaří, pokud prostředek už existuje.</span><span class="sxs-lookup"><span data-stu-id="59198-128">Fail an update if the resource no longer exists</span></span>
+ <span data-ttu-id="59198-129">Aktualizace nezdaří, pokud se změní verze</span><span class="sxs-lookup"><span data-stu-id="59198-129">Fail an update if the resource version changes</span></span>

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a><span data-ttu-id="59198-130">Ukázkový kód z [DotNetETagsExplainer programu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span><span class="sxs-lookup"><span data-stu-id="59198-130">Sample code from [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span></span>

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
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
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
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
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

## <a name="design-pattern"></a><span data-ttu-id="59198-131">Vzor návrhu</span><span class="sxs-lookup"><span data-stu-id="59198-131">Design pattern</span></span>

<span data-ttu-id="59198-132">Vzoru návrhu pro implementaci optimistickou metodu souběžného zpracování by měl obsahovat smyčku opakování podmínka přístupu zkontrolujte testu pro podmínku přístup a volitelně načte aktualizovaný prostředek dřív, než se znovu použít změny.</span><span class="sxs-lookup"><span data-stu-id="59198-132">A design pattern for implementing optimistic concurrency should include a loop that retries the access condition check, a test for the access condition, and optionally retrieves an updated resource before attempting to re-apply the changes.</span></span> 

<span data-ttu-id="59198-133">Tento fragment kódu ukazuje přidání synonymMap na index, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="59198-133">This code snippet illustrates the addition of a synonymMap to an index that already exists.</span></span> <span data-ttu-id="59198-134">Tento kód je z [synonymum (preview) C# – tutoriál pro službu Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span><span class="sxs-lookup"><span data-stu-id="59198-134">This code is from the [Synonym (preview) C# tutorial for Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span></span> 

<span data-ttu-id="59198-135">Fragment získá index "hotels", zkontroluje verzi objektu na operaci aktualizace, vyvolá výjimku, pokud se nezdaří podmínku a poté operaci zopakují (maximálně třikrát), od indexu načtení ze serveru získat nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="59198-135">The snippet gets the "hotels" index, checks the object version on an update operation, throws an exception if the condition fails, and then retries the operation (up to three times), starting with index retrieval from the server to get the latest version.</span></span>

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
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


## <a name="next-steps"></a><span data-ttu-id="59198-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59198-136">Next steps</span></span>

<span data-ttu-id="59198-137">Zkontrolujte [synonyma C# ukázka](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) pro další kontext na tom, jak bezpečně aktualizovat existující index.</span><span class="sxs-lookup"><span data-stu-id="59198-137">Review the [synonyms C# sample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) for more context on how to safely update an existing index.</span></span>

<span data-ttu-id="59198-138">Zkuste upravit některou z následujících ukázek o značky etag binárním rozsáhlým nebo AccessCondition objekty.</span><span class="sxs-lookup"><span data-stu-id="59198-138">Try modifying either of the following samples to include ETags or AccessCondition objects.</span></span>

+ [<span data-ttu-id="59198-139">Rozhraní REST API ukázce na Githubu</span><span class="sxs-lookup"><span data-stu-id="59198-139">REST API sample on Github</span></span>](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ <span data-ttu-id="59198-140">[Ukázka sady .NET SDK na Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="59198-140">[.NET SDK sample on Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span></span> <span data-ttu-id="59198-141">Toto řešení zahrnuje "DotNetEtagsExplainer" projekt obsahující kód uvedený v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="59198-141">This solution includes the "DotNetEtagsExplainer" project containing the code presented in this article.</span></span>

## <a name="see-also"></a><span data-ttu-id="59198-142">Viz také</span><span class="sxs-lookup"><span data-stu-id="59198-142">See also</span></span>

  <span data-ttu-id="59198-143">[Společné hlavičky požadavku a odpovědi HTTP](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span><span class="sxs-lookup"><span data-stu-id="59198-143">[Common HTTP request and response headers](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span></span>  
  <span data-ttu-id="59198-144">[Stavové kódy HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [indexu operací (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span><span class="sxs-lookup"><span data-stu-id="59198-144">[HTTP status codes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index operations (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span></span>