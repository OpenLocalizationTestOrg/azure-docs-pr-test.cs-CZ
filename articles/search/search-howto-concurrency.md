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
# <a name="how-toomanage-concurrency-in-azure-search"></a>Jak toomanage souběžnosti ve službě Azure Search

Při správě prostředků Azure Search, jako jsou indexy a zdroje dat, je důležité tooupdate prostředky bezpečně, zejména pokud prostředkům přistupuje souběžně různých komponent vaší aplikace. Když se dvě klientů současně aktualizace prostředku bez spolupráce, je možné konflikty časování. tooprevent se služba Azure Search nabízí *optimistickou metodu souběžného modelu*. Neexistují žádné zámky na prostředek. Místo toho není značku ETag pro každý prostředek, který identifikuje verze hello prostředků tak, aby může vytvořit požadavků, které vyhnout náhodných přepíše.

> [!Tip]
> Koncepční kód [ukázkové C# řešení](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) vysvětluje, jak funguje řízení souběžnosti ve službě Azure Search. Hello kód vytvoří podmínky, které vyvolají Kontrola souběžnosti. Čtení hello [fragment kódu níže](#samplecode) je pravděpodobně dostatečná pro většinu vývojáře, ale pokud budete chtít toorun ho, název služby hello tooadd appSettings.JSON určený upravit a rozhraní api – klíč správce. Danou adresu URL služby `http://myservice.search.windows.net`, je název služby hello `myservice`.

## <a name="how-it-works"></a>Jak to funguje

Optimistické, že je implementováno souběžnosti prostřednictvím přístupu podmínka kontroluje při voláních rozhraní API zápisu tooindexes, indexery, zdrojů dat a synonymMap prostředky. 

Všechny prostředky mít [ *entity tag (značka ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) poskytující informace o verzi objektu. Kontrolou nejprve hello značka ETag, se můžete vyhnout Souběžná aktualizace v obvyklý pracovní postup (získat, upravte místně, aktualizujte) zajištěním hello prostředků značka ETag odpovídá vaší místní kopie. 

+ Hello REST API používá [značka ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) v hlavičce požadavku hello.
+ Hello .NET SDK nastaví hello ETag prostřednictvím objekt accessCondition nastavení hello [If-Match | Záhlaví If-Match-žádný](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) na hello prostředku. Libovolného objektu, která dědí z [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) má accessCondition objekt.

Při každé aktualizaci prostředek automaticky změní jeho značka ETag. Při implementaci správy souběžnosti všechny vaše změny se uvedení předběžné podmínky na hello žádost o aktualizaci, která vyžaduje hello vzdálený prostředek toohave hello stejné ETag jako kopii hello hello prostředku, který změnil na klientovi hello. Pokud již souběžných procesů změnila hello vzdálený prostředek, hello ETag nebude odpovídat hello předběžnou a hello požadavek selže s HTTP 412. Pokud používáte hello .NET SDK, manifesty jako `CloudException` kde hello `IsAccessConditionFailed()` rozšíření metoda vrátí hodnotu true.

> [!Note]
> Existuje pouze jeden mechanismus pro souběžnosti. Používá se vždy, bez ohledu na to, který se používá rozhraní API pro aktualizace prostředků. 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Případy použití a ukázkový kód

Hello následující kód ukazuje, že accessCondition vyhledává operace klíče aktualizace:

+ Aktualizace nezdaří, pokud hello prostředků už existuje.
+ Pokud verze prostředků hello změní selhat aktualizace

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>Ukázkový kód z [DotNetETagsExplainer programu](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

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

## <a name="design-pattern"></a>Vzor návrhu

Vzor návrhu pro implementaci optimistickou metodu souběžného by měla obsahovat smyčku, která opakování hello podmínku kontroly přístupu testu pro podmínku hello přístup a volitelně načte aktualizovaný prostředek před pokusem o toore-změny hello. 

Tento fragment kódu ukazuje hello přidání index tooan synonymMap, který již existuje. Tento kód je z hello [synonymum (preview) C# – tutoriál pro službu Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk). 

fragment kódu Hello získá index hello "hotels", zkontroluje verzi objektu hello na operaci aktualizace, vyvolá výjimku, pokud selže hello podmínka, a pak hello opakování operace (až toothree dobu), od indexu z načtení hello server tooget hello nejnovější verze.

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


## <a name="next-steps"></a>Další kroky

Zkontrolujte hello [synonyma C# ukázka](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) pro další kontext na tom, jak toosafely aktualizovat existující index.

Zkuste upravit buď hello následující ukázky tooinclude značky etag binárním rozsáhlým nebo AccessCondition objektů.

+ [Rozhraní REST API ukázce na Githubu](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ [Ukázka sady .NET SDK na Githubu](https://github.com/Azure-Samples/search-dotnet-getting-started). Toto řešení zahrnuje projekt "DotNetEtagsExplainer" hello obsahující hello kód uvedený v tomto článku.

## <a name="see-also"></a>Viz také

  [Společné hlavičky požadavku a odpovědi HTTP](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)    
  [Stavové kódy HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [indexu operací (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)