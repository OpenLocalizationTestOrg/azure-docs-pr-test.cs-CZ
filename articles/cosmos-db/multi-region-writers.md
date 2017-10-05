---
title: "Více hlavní databázi architektury s Azure Cosmos DB | Microsoft Docs"
description: "Další informace o návrhu architektury aplikací s místní čtení a zápisu v různých geografických oblastech s Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 706ced74-ea67-45dd-a7de-666c3c893687
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf1482ae7b1070023703f5dbe861d151f5d64fd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a><span data-ttu-id="5360a-103">Více hlavní globálně replikované databáze architektury s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5360a-103">Multi-master globally replicated database architectures with Azure Cosmos DB</span></span>
<span data-ttu-id="5360a-104">Podporuje Azure Cosmos DB připraveného [globální replikace](distribute-data-globally.md), která umožňuje distribuci dat do několika oblastí přístup s nízkou latencí kdekoli v zatížení.</span><span class="sxs-lookup"><span data-stu-id="5360a-104">Azure Cosmos DB supports turnkey [global replication](distribute-data-globally.md), which allows you to distribute data to multiple regions with low latency access anywhere in the workload.</span></span> <span data-ttu-id="5360a-105">Tento model se často používá pro vydavatele nebo příjemce zatížení tam, kde je zapisovač v jedné zeměpisné oblasti a globálně distribuované čtečky v jiných oblastech (čtení).</span><span class="sxs-lookup"><span data-stu-id="5360a-105">This model is commonly used for publisher/consumer workloads where there is a writer in a single geographic region and globally distributed readers in other (read) regions.</span></span> 

<span data-ttu-id="5360a-106">Podpora globální replikace databáze Cosmos Azure můžete taky vytvářet aplikace, ve kterých jsou globálně distribuované zapisovače a čtečky.</span><span class="sxs-lookup"><span data-stu-id="5360a-106">You can also use Azure Cosmos DB's global replication support to build applications in which writers and readers are globally distributed.</span></span> <span data-ttu-id="5360a-107">Tento dokument popisuje vzor, který umožňuje dosažení místní zápisu a čtení místní pro distribuované zapisovače pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5360a-107">This document outlines a pattern that enables achieving local write and local read access for distributed writers using Azure Cosmos DB.</span></span>

## <span data-ttu-id="5360a-108"><a id="ExampleScenario"></a>Publikování obsahu – příklad scénáře</span><span class="sxs-lookup"><span data-stu-id="5360a-108"><a id="ExampleScenario"></a>Content Publishing - an example scenario</span></span>
<span data-ttu-id="5360a-109">Podívejme se na scénář skutečných popisují, jak můžete používat vzory globálně distribuované více region nebo více master čtení zápisu s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5360a-109">Let's look at a real world scenario to describe how you can use globally distributed multi-region/multi-master read write patterns with Azure Cosmos DB.</span></span> <span data-ttu-id="5360a-110">Vezměte v úvahu vytvořené v Azure Cosmos DB platforma pro publikování obsahu.</span><span class="sxs-lookup"><span data-stu-id="5360a-110">Consider a content publishing platform built on Azure Cosmos DB.</span></span> <span data-ttu-id="5360a-111">Tady jsou některé požadavky, které tuto platformu musí splňovat pro vysoký výkon uživatele pro vydavatele a spotřebitelé.</span><span class="sxs-lookup"><span data-stu-id="5360a-111">Here are some requirements that this platform must meet for a great user experience for both publishers and consumers.</span></span>

* <span data-ttu-id="5360a-112">Autoři a Odběratelé, kteří jsou rozloženy na světě.</span><span class="sxs-lookup"><span data-stu-id="5360a-112">Both authors and subscribers are spread over the world</span></span> 
* <span data-ttu-id="5360a-113">Autoři články (zápisu) musíte publikovat své místní oblast (nejbližší)</span><span class="sxs-lookup"><span data-stu-id="5360a-113">Authors must publish (write) articles to their local (closest) region</span></span>
* <span data-ttu-id="5360a-114">Autoři mají čtečky nebo odběratele, jejich článků kteří distribuují po celém světě.</span><span class="sxs-lookup"><span data-stu-id="5360a-114">Authors have readers/subscribers of their articles who are distributed across the globe.</span></span> 
* <span data-ttu-id="5360a-115">Odběratelé, kteří měli obdržet oznámení, při publikování nových článků.</span><span class="sxs-lookup"><span data-stu-id="5360a-115">Subscribers should get a notification when new articles are published.</span></span>
* <span data-ttu-id="5360a-116">Odběratelé, kteří musí být možné číst články ze své místní oblast.</span><span class="sxs-lookup"><span data-stu-id="5360a-116">Subscribers must be able to read articles from their local region.</span></span> <span data-ttu-id="5360a-117">Musí být také možné přidat recenze na tyto články.</span><span class="sxs-lookup"><span data-stu-id="5360a-117">They should also be able to add reviews to these articles.</span></span> 
* <span data-ttu-id="5360a-118">Každý, kdo včetně autora články musí být možné zobrazit všechny recenze připojená k články z místní oblast.</span><span class="sxs-lookup"><span data-stu-id="5360a-118">Anyone including the author of the articles should be able view all the reviews attached to articles from a local region.</span></span> 

<span data-ttu-id="5360a-119">Za předpokladu, že miliony s až miliardy článků, vydavatelům a spotřebitelům brzy máme boji s problémy měřítka společně s zaručující polohu přístupu.</span><span class="sxs-lookup"><span data-stu-id="5360a-119">Assuming millions of consumers and publishers with billions of articles, soon we have to confront the problems of scale along with guaranteeing locality of access.</span></span> <span data-ttu-id="5360a-120">Stejně jako u většiny problémů škálovatelnost řešení spočívá ve strategii je dobré rozdělení.</span><span class="sxs-lookup"><span data-stu-id="5360a-120">As with most scalability problems, the solution lies in a good partitioning strategy.</span></span> <span data-ttu-id="5360a-121">V dalším kroku podíváme, jak model články, zkontrolujte a oznámení jako dokumenty, nakonfigurovat účty Azure Cosmos DB a implementovat vrstva přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="5360a-121">Next, let's look at how to model articles, review, and notifications as documents, configure Azure Cosmos DB accounts, and implement a data access layer.</span></span> 

<span data-ttu-id="5360a-122">Pokud vás zajímají další informace o vytváření oddílů a klíče oddílů, najdete v části [vytváření oddílů a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="5360a-122">If you would like to learn more about partitioning and partition keys, see [Partitioning and Scaling in Azure Cosmos DB](partition-data.md).</span></span>

## <span data-ttu-id="5360a-123"><a id="ModelingNotifications"></a>Modelování oznámení</span><span class="sxs-lookup"><span data-stu-id="5360a-123"><a id="ModelingNotifications"></a>Modeling notifications</span></span>
<span data-ttu-id="5360a-124">Oznámení jsou datových kanálů specifické pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="5360a-124">Notifications are data feeds specific to a user.</span></span> <span data-ttu-id="5360a-125">Proto přístupové vzorce pro dokumenty oznámení jsou vždy v rámci jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="5360a-125">Therefore, the access patterns for notifications documents are always in the context of single user.</span></span> <span data-ttu-id="5360a-126">Například by "post oznámení pro uživatele" nebo "načíst všechna oznámení pro daného uživatele".</span><span class="sxs-lookup"><span data-stu-id="5360a-126">For example, you would "post a notification to a user" or "fetch all notifications for a given user".</span></span> <span data-ttu-id="5360a-127">Ano, bude optimální volbou oddíly klíč pro tento typ `UserId`.</span><span class="sxs-lookup"><span data-stu-id="5360a-127">So, the optimal choice of partitioning key for this type would be `UserId`.</span></span>

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

## <span data-ttu-id="5360a-128"><a id="ModelingSubscriptions"></a>Odběry modelování</span><span class="sxs-lookup"><span data-stu-id="5360a-128"><a id="ModelingSubscriptions"></a>Modeling subscriptions</span></span>
<span data-ttu-id="5360a-129">Odběry můžete vytvořit pro různé kritéria jako určitou kategorii články zájmu nebo konkrétní vydavatele.</span><span class="sxs-lookup"><span data-stu-id="5360a-129">Subscriptions can be created for various criteria like a specific category of articles of interest, or a specific publisher.</span></span> <span data-ttu-id="5360a-130">Proto `SubscriptionFilter` je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="5360a-130">Hence the `SubscriptionFilter` is a good choice for partition key.</span></span>

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <span data-ttu-id="5360a-131"><a id="ModelingArticles"></a>Články modelování</span><span class="sxs-lookup"><span data-stu-id="5360a-131"><a id="ModelingArticles"></a>Modeling articles</span></span>
<span data-ttu-id="5360a-132">Jakmile článek identifikuje prostřednictvím oznámení, další dotazy jsou obvykle založené na `Article.Id`.</span><span class="sxs-lookup"><span data-stu-id="5360a-132">Once an article is identified through notifications, subsequent queries are typically based on the `Article.Id`.</span></span> <span data-ttu-id="5360a-133">Výběr `Article.Id` jako oddíl klíč tak poskytuje nejlepší distribuce pro ukládání články v kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5360a-133">Choosing `Article.Id` as partition the key thus provides the best distribution for storing articles inside an Azure Cosmos DB collection.</span></span> 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }
        
        //... 
    }

## <span data-ttu-id="5360a-134"><a id="ModelingReviews"></a>Zkontroluje modelování</span><span class="sxs-lookup"><span data-stu-id="5360a-134"><a id="ModelingReviews"></a>Modeling reviews</span></span>
<span data-ttu-id="5360a-135">Jako články jsou recenze většinou zapisovat a číst v kontextu článku.</span><span class="sxs-lookup"><span data-stu-id="5360a-135">Like articles, reviews are mostly written and read in the context of article.</span></span> <span data-ttu-id="5360a-136">Výběr `ArticleId` jako oddíl klíč poskytuje nejlepší distribuce a efektivní přístup recenze přidružené článku.</span><span class="sxs-lookup"><span data-stu-id="5360a-136">Choosing `ArticleId` as a partition key provides best distribution and efficient access of reviews associated with article.</span></span> 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <span data-ttu-id="5360a-137"><a id="DataAccessMethods"></a>Metody pro přístup k vrstvě</span><span class="sxs-lookup"><span data-stu-id="5360a-137"><a id="DataAccessMethods"></a>Data access layer methods</span></span>
<span data-ttu-id="5360a-138">Nyní Podíváme se na hlavní data musíme implementovat metody přístupu.</span><span class="sxs-lookup"><span data-stu-id="5360a-138">Now let's look at the main data access methods we need to implement.</span></span> <span data-ttu-id="5360a-139">Tady je seznam metod, `ContentPublishDatabase` musí:</span><span class="sxs-lookup"><span data-stu-id="5360a-139">Here's the list of methods that the `ContentPublishDatabase` needs:</span></span>

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <span data-ttu-id="5360a-140"><a id="Architecture"></a>Konfigurace účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5360a-140"><a id="Architecture"></a>Azure Cosmos DB account configuration</span></span>
<span data-ttu-id="5360a-141">Zaručit místní čte a zapisuje, jsme musí oddílu data nejen v oddílu klíče, ale také podle vzoru zeměpisné přístup do oblasti.</span><span class="sxs-lookup"><span data-stu-id="5360a-141">To guarantee local reads and writes, we must partition data not just on partition key, but also based on the geographical access pattern into regions.</span></span> <span data-ttu-id="5360a-142">Model spoléhá na nutnosti geograficky replikované Azure Cosmos DB databázového účtu pro každou oblast.</span><span class="sxs-lookup"><span data-stu-id="5360a-142">The model relies on having a geo-replicated Azure Cosmos DB database account for each region.</span></span> <span data-ttu-id="5360a-143">Například se dvěma oblastmi, zde je instalace s pro zápisy více oblasti:</span><span class="sxs-lookup"><span data-stu-id="5360a-143">For example, with two regions, here's a setup for multi-region writes:</span></span>

| <span data-ttu-id="5360a-144">Název účtu</span><span class="sxs-lookup"><span data-stu-id="5360a-144">Account Name</span></span> | <span data-ttu-id="5360a-145">Zápis oblast</span><span class="sxs-lookup"><span data-stu-id="5360a-145">Write Region</span></span> | <span data-ttu-id="5360a-146">Oblast pro čtení</span><span class="sxs-lookup"><span data-stu-id="5360a-146">Read Region</span></span> |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

<span data-ttu-id="5360a-147">Následující diagram znázorňuje, jak provádět čtení a zápisu v typické aplikaci s tímto nastavením:</span><span class="sxs-lookup"><span data-stu-id="5360a-147">The following diagram shows how reads and writes are performed in a typical application with this setup:</span></span>

![Azure Cosmos DB více hlavních architektura](./media/multi-region-writers/multi-master.png)

<span data-ttu-id="5360a-149">Zde je fragment kódu znázorňující k chybě při inicializaci klienty v DAL, spuštěné v `West US` oblast.</span><span class="sxs-lookup"><span data-stu-id="5360a-149">Here is a code snippet showing how to initialize the clients in a DAL running in the `West US` region.</span></span>
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

<span data-ttu-id="5360a-150">V předchozí instalaci může předat vrstva přístupu k datům všech zápisů místní účet, podle které se nasadí.</span><span class="sxs-lookup"><span data-stu-id="5360a-150">With the preceding setup, the data access layer can forward all writes to the local account based on where it is deployed.</span></span> <span data-ttu-id="5360a-151">Čtení ze oba účty, a získat globální zobrazení dat provádí čtení.</span><span class="sxs-lookup"><span data-stu-id="5360a-151">Reads are performed by reading from both accounts to get the global view of data.</span></span> <span data-ttu-id="5360a-152">Tuto metodu lze rozšířit na jako v mnoha oblastech podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5360a-152">This approach can be extended to as many regions as required.</span></span> <span data-ttu-id="5360a-153">Zde je ukázka, instalační program s tři zeměpisné oblasti:</span><span class="sxs-lookup"><span data-stu-id="5360a-153">For example, here's a setup with three geographic regions:</span></span>

| <span data-ttu-id="5360a-154">Název účtu</span><span class="sxs-lookup"><span data-stu-id="5360a-154">Account Name</span></span> | <span data-ttu-id="5360a-155">Zápis oblast</span><span class="sxs-lookup"><span data-stu-id="5360a-155">Write Region</span></span> | <span data-ttu-id="5360a-156">Oblast pro čtení 1</span><span class="sxs-lookup"><span data-stu-id="5360a-156">Read Region 1</span></span> | <span data-ttu-id="5360a-157">Přečtěte si oblasti 2</span><span class="sxs-lookup"><span data-stu-id="5360a-157">Read Region 2</span></span> |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <span data-ttu-id="5360a-158"><a id="DataAccessImplementation"></a>Data access layer implementace</span><span class="sxs-lookup"><span data-stu-id="5360a-158"><a id="DataAccessImplementation"></a>Data access layer implementation</span></span>
<span data-ttu-id="5360a-159">Nyní Podíváme se na provádění vrstva přístupu k datům (DAL) pro aplikaci se dvěma oblastmi s možností zápisu.</span><span class="sxs-lookup"><span data-stu-id="5360a-159">Now let's look at the implementation of the data access layer (DAL) for an application with two writable regions.</span></span> <span data-ttu-id="5360a-160">DAL musí implementovat následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5360a-160">The DAL must implement the following steps:</span></span>

* <span data-ttu-id="5360a-161">Vytvoření více instancí `DocumentClient` pro jednotlivé účty.</span><span class="sxs-lookup"><span data-stu-id="5360a-161">Create multiple instances of `DocumentClient` for each account.</span></span> <span data-ttu-id="5360a-162">Se dvěma oblastmi jeden má každá instance vrstvy DAL `writeClient` a jeden `readClient`.</span><span class="sxs-lookup"><span data-stu-id="5360a-162">With two regions, each DAL instance has one `writeClient` and one `readClient`.</span></span> 
* <span data-ttu-id="5360a-163">Podle oblasti nasazené aplikace, konfigurace koncových bodů pro `writeclient` a `readClient`.</span><span class="sxs-lookup"><span data-stu-id="5360a-163">Based on the deployed region of the application, configure the endpoints for `writeclient` and `readClient`.</span></span> <span data-ttu-id="5360a-164">Například DAL nasazené v `West US` používá `contentpubdatabase-usa.documents.azure.com` pro provádění zápisy.</span><span class="sxs-lookup"><span data-stu-id="5360a-164">For example, the DAL deployed in `West US` uses `contentpubdatabase-usa.documents.azure.com` for performing writes.</span></span> <span data-ttu-id="5360a-165">DAL nasazené v `NorthEurope` používá `contentpubdatabase-europ.documents.azure.com` pro zápis.</span><span class="sxs-lookup"><span data-stu-id="5360a-165">The DAL deployed in `NorthEurope` uses `contentpubdatabase-europ.documents.azure.com` for writes.</span></span>

<span data-ttu-id="5360a-166">V předchozí instalaci se dá implementovat datové metody přístupu.</span><span class="sxs-lookup"><span data-stu-id="5360a-166">With the preceding setup, the data access methods can be implemented.</span></span> <span data-ttu-id="5360a-167">Zápis předávat operace zápisu do odpovídajících `writeClient`.</span><span class="sxs-lookup"><span data-stu-id="5360a-167">Write operations forward the write to the corresponding `writeClient`.</span></span>

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

<span data-ttu-id="5360a-168">Pro čtení, oznámení a recenze, můžete musí číst od oblasti a sjednocení výsledky jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="5360a-168">For reading notifications and reviews, you must read from both regions and union the results as shown in the following snippet:</span></span>

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

<span data-ttu-id="5360a-169">Proto výběr dobrý klíč rozdělení a statické dělení založené na účet, můžete dosáhnout více oblast místní zápisů a čtení pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5360a-169">Thus, by choosing a good partitioning key and static account-based partitioning, you can achieve multi-region local writes and reads using Azure Cosmos DB.</span></span>

## <span data-ttu-id="5360a-170"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="5360a-170"><a id="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="5360a-171">V tomto článku jsme popsané, jak je používat vzory globálně distribuované čtení zápisu více oblasti s Azure DB Cosmos jako vzorový scénář pomocí publikování obsahu.</span><span class="sxs-lookup"><span data-stu-id="5360a-171">In this article, we described how you can use globally distributed multi-region read write patterns with Azure Cosmos DB using content publishing as a sample scenario.</span></span>

* <span data-ttu-id="5360a-172">Další informace o tom, jak Azure Cosmos DB podporuje [globální distribuční](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-172">Learn about how Azure Cosmos DB supports [global distribution](distribute-data-globally.md)</span></span>
* <span data-ttu-id="5360a-173">Další informace o [automatickou a ruční převzetí služeb při selhání v Azure Cosmos DB](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-173">Learn about [automatic and manual failovers in Azure Cosmos DB](regional-failover.md)</span></span>
* <span data-ttu-id="5360a-174">Další informace o [globální konzistence s Azure Cosmos DB](consistency-levels.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-174">Learn about [global consistency with Azure Cosmos DB](consistency-levels.md)</span></span>
* <span data-ttu-id="5360a-175">Vývoj s více oblastí pomocí [DB Cosmos Azure - DocumentDB rozhraní API](tutorial-global-distribution-documentdb.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-175">Develop with multiple regions using the [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span></span>
* <span data-ttu-id="5360a-176">Vývoj s více oblastí pomocí [Azure Cosmos DB - MongoDB rozhraní API](tutorial-global-distribution-MongoDB.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-176">Develop with multiple regions using the [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span></span>
* <span data-ttu-id="5360a-177">Vývoj s více oblastí pomocí [Azure Cosmos DB - API tabulky](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="5360a-177">Develop with multiple regions using the [Azure Cosmos DB - Table API](tutorial-global-distribution-table.md)</span></span>
