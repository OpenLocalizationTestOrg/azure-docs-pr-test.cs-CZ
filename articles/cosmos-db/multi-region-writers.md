---
title: "architektury aaaMulti hlavní databáze s Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak toodesign architektury aplikací s místní čte a zapisuje v různých geografických oblastech s Azure Cosmos DB."
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
ms.openlocfilehash: 3269c8405afe16f75db69b42e576fe76e00a8e16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a><span data-ttu-id="4f066-103">Více hlavní globálně replikované databáze architektury s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4f066-103">Multi-master globally replicated database architectures with Azure Cosmos DB</span></span>
<span data-ttu-id="4f066-104">Podporuje Azure Cosmos DB připraveného [globální replikace](distribute-data-globally.md), což vám umožní toodistribute datové oblasti toomultiple přístup s nízkou latencí kdekoli v hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="4f066-104">Azure Cosmos DB supports turnkey [global replication](distribute-data-globally.md), which allows you toodistribute data toomultiple regions with low latency access anywhere in hello workload.</span></span> <span data-ttu-id="4f066-105">Tento model se často používá pro vydavatele nebo příjemce zatížení tam, kde je zapisovač v jedné zeměpisné oblasti a globálně distribuované čtečky v jiných oblastech (čtení).</span><span class="sxs-lookup"><span data-stu-id="4f066-105">This model is commonly used for publisher/consumer workloads where there is a writer in a single geographic region and globally distributed readers in other (read) regions.</span></span> 

<span data-ttu-id="4f066-106">Můžete také použít Azure Cosmos DB globální replikace podporuje toobuild aplikace, ve kterých jsou globálně distribuované zapisovače a čtečky.</span><span class="sxs-lookup"><span data-stu-id="4f066-106">You can also use Azure Cosmos DB's global replication support toobuild applications in which writers and readers are globally distributed.</span></span> <span data-ttu-id="4f066-107">Tento dokument popisuje vzor, který umožňuje dosažení místní zápisu a čtení místní pro distribuované zapisovače pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4f066-107">This document outlines a pattern that enables achieving local write and local read access for distributed writers using Azure Cosmos DB.</span></span>

## <span data-ttu-id="4f066-108"><a id="ExampleScenario"></a>Publikování obsahu – příklad scénáře</span><span class="sxs-lookup"><span data-stu-id="4f066-108"><a id="ExampleScenario"></a>Content Publishing - an example scenario</span></span>
<span data-ttu-id="4f066-109">Podívejme se na toodescribe skutečných scénář použití globálně distribuované více region nebo více master čtení zápisu vzory s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4f066-109">Let's look at a real world scenario toodescribe how you can use globally distributed multi-region/multi-master read write patterns with Azure Cosmos DB.</span></span> <span data-ttu-id="4f066-110">Vezměte v úvahu vytvořené v Azure Cosmos DB platforma pro publikování obsahu.</span><span class="sxs-lookup"><span data-stu-id="4f066-110">Consider a content publishing platform built on Azure Cosmos DB.</span></span> <span data-ttu-id="4f066-111">Tady jsou některé požadavky, které tuto platformu musí splňovat pro vysoký výkon uživatele pro vydavatele a spotřebitelé.</span><span class="sxs-lookup"><span data-stu-id="4f066-111">Here are some requirements that this platform must meet for a great user experience for both publishers and consumers.</span></span>

* <span data-ttu-id="4f066-112">Autoři a Odběratelé, kteří jsou rozloženy hello, world</span><span class="sxs-lookup"><span data-stu-id="4f066-112">Both authors and subscribers are spread over hello world</span></span> 
* <span data-ttu-id="4f066-113">Autoři musíte publikovat (zápisu) články tootheir místní (nejbližší) oblast</span><span class="sxs-lookup"><span data-stu-id="4f066-113">Authors must publish (write) articles tootheir local (closest) region</span></span>
* <span data-ttu-id="4f066-114">Autoři mají čtečky nebo odběratele, jejich článků kteří jsou rozmístěny v celém světě hello.</span><span class="sxs-lookup"><span data-stu-id="4f066-114">Authors have readers/subscribers of their articles who are distributed across hello globe.</span></span> 
* <span data-ttu-id="4f066-115">Odběratelé, kteří měli obdržet oznámení, při publikování nových článků.</span><span class="sxs-lookup"><span data-stu-id="4f066-115">Subscribers should get a notification when new articles are published.</span></span>
* <span data-ttu-id="4f066-116">Odběratelé, kteří musí být schopný tooread články ze své místní oblast.</span><span class="sxs-lookup"><span data-stu-id="4f066-116">Subscribers must be able tooread articles from their local region.</span></span> <span data-ttu-id="4f066-117">Měly by být také možné tooadd recenze toothese články.</span><span class="sxs-lookup"><span data-stu-id="4f066-117">They should also be able tooadd reviews toothese articles.</span></span> 
* <span data-ttu-id="4f066-118">Každý, kdo včetně hello Autor článků hello by měl být schopný zobrazení, že všechny hello recenze připojené tooarticles z místní oblast.</span><span class="sxs-lookup"><span data-stu-id="4f066-118">Anyone including hello author of hello articles should be able view all hello reviews attached tooarticles from a local region.</span></span> 

<span data-ttu-id="4f066-119">Za předpokladu, že miliony s až miliardy článků, vydavatelům a spotřebitelům brzy máme problémy hello tooconfront měřítka společně s zaručující polohu přístupu.</span><span class="sxs-lookup"><span data-stu-id="4f066-119">Assuming millions of consumers and publishers with billions of articles, soon we have tooconfront hello problems of scale along with guaranteeing locality of access.</span></span> <span data-ttu-id="4f066-120">Stejně jako u většiny problémů škálovatelnost, hello řešení spočívá ve strategii je dobré rozdělení.</span><span class="sxs-lookup"><span data-stu-id="4f066-120">As with most scalability problems, hello solution lies in a good partitioning strategy.</span></span> <span data-ttu-id="4f066-121">Dále umožňuje vyhledat jak toomodel články, zkontrolujte a oznámení jako dokumenty, nakonfigurovat účty Azure Cosmos DB a implementovat vrstva přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="4f066-121">Next, let's look at how toomodel articles, review, and notifications as documents, configure Azure Cosmos DB accounts, and implement a data access layer.</span></span> 

<span data-ttu-id="4f066-122">Pokud vás zajímají další informace o vytváření oddílů a klíče oddílů toolearn, najdete v části [vytváření oddílů a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="4f066-122">If you would like toolearn more about partitioning and partition keys, see [Partitioning and Scaling in Azure Cosmos DB](partition-data.md).</span></span>

## <span data-ttu-id="4f066-123"><a id="ModelingNotifications"></a>Modelování oznámení</span><span class="sxs-lookup"><span data-stu-id="4f066-123"><a id="ModelingNotifications"></a>Modeling notifications</span></span>
<span data-ttu-id="4f066-124">Oznámení jsou data informační kanály konkrétní tooa uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f066-124">Notifications are data feeds specific tooa user.</span></span> <span data-ttu-id="4f066-125">Proto hello přístupové vzorce pro dokumenty oznámení se vždycky nacházejí v kontextu hello jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f066-125">Therefore, hello access patterns for notifications documents are always in hello context of single user.</span></span> <span data-ttu-id="4f066-126">Například by "post uživatele tooa oznámení" nebo "načíst všechna oznámení pro daného uživatele".</span><span class="sxs-lookup"><span data-stu-id="4f066-126">For example, you would "post a notification tooa user" or "fetch all notifications for a given user".</span></span> <span data-ttu-id="4f066-127">Ano, hello optimální volbou oddíly klíč pro tento typ by být `UserId`.</span><span class="sxs-lookup"><span data-stu-id="4f066-127">So, hello optimal choice of partitioning key for this type would be `UserId`.</span></span>

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // hello user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // hello partition Key for hello resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of hello notification. 
        public string ArticleId { get; set; } 
    }

## <span data-ttu-id="4f066-128"><a id="ModelingSubscriptions"></a>Odběry modelování</span><span class="sxs-lookup"><span data-stu-id="4f066-128"><a id="ModelingSubscriptions"></a>Modeling subscriptions</span></span>
<span data-ttu-id="4f066-129">Odběry můžete vytvořit pro různé kritéria jako určitou kategorii články zájmu nebo konkrétní vydavatele.</span><span class="sxs-lookup"><span data-stu-id="4f066-129">Subscriptions can be created for various criteria like a specific category of articles of interest, or a specific publisher.</span></span> <span data-ttu-id="4f066-130">Proto hello `SubscriptionFilter` je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="4f066-130">Hence hello `SubscriptionFilter` is a good choice for partition key.</span></span>

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

## <span data-ttu-id="4f066-131"><a id="ModelingArticles"></a>Články modelování</span><span class="sxs-lookup"><span data-stu-id="4f066-131"><a id="ModelingArticles"></a>Modeling articles</span></span>
<span data-ttu-id="4f066-132">Jakmile článek identifikuje prostřednictvím oznámení, další dotazy jsou obvykle založené na hello `Article.Id`.</span><span class="sxs-lookup"><span data-stu-id="4f066-132">Once an article is identified through notifications, subsequent queries are typically based on hello `Article.Id`.</span></span> <span data-ttu-id="4f066-133">Výběr `Article.Id` jako oddíl hello klíč tak poskytuje hello nejlepší distribuce pro ukládání články v kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4f066-133">Choosing `Article.Id` as partition hello key thus provides hello best distribution for storing articles inside an Azure Cosmos DB collection.</span></span> 

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
        
        // Author of hello article
        public string Author { get; set; }

        // Category/genre of hello article
        public string Category { get; set; }

        // Tags associated with hello article
        public string[] Tags { get; set; }

        // Title of hello article
        public string Title { get; set; }
        
        //... 
    }

## <span data-ttu-id="4f066-134"><a id="ModelingReviews"></a>Zkontroluje modelování</span><span class="sxs-lookup"><span data-stu-id="4f066-134"><a id="ModelingReviews"></a>Modeling reviews</span></span>
<span data-ttu-id="4f066-135">Jako články jsou recenze většinou zapisovat a číst v kontextu hello článku.</span><span class="sxs-lookup"><span data-stu-id="4f066-135">Like articles, reviews are mostly written and read in hello context of article.</span></span> <span data-ttu-id="4f066-136">Výběr `ArticleId` jako oddíl klíč poskytuje nejlepší distribuce a efektivní přístup recenze přidružené článku.</span><span class="sxs-lookup"><span data-stu-id="4f066-136">Choosing `ArticleId` as a partition key provides best distribution and efficient access of reviews associated with article.</span></span> 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of hello review 
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

## <span data-ttu-id="4f066-137"><a id="DataAccessMethods"></a>Metody pro přístup k vrstvě</span><span class="sxs-lookup"><span data-stu-id="4f066-137"><a id="DataAccessMethods"></a>Data access layer methods</span></span>
<span data-ttu-id="4f066-138">Nyní Podíváme se na hlavní data hello potřebujeme tooimplement metody přístupu.</span><span class="sxs-lookup"><span data-stu-id="4f066-138">Now let's look at hello main data access methods we need tooimplement.</span></span> <span data-ttu-id="4f066-139">Tady je seznam hello metody, které hello `ContentPublishDatabase` musí:</span><span class="sxs-lookup"><span data-stu-id="4f066-139">Here's hello list of methods that hello `ContentPublishDatabase` needs:</span></span>

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <span data-ttu-id="4f066-140"><a id="Architecture"></a>Konfigurace účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4f066-140"><a id="Architecture"></a>Azure Cosmos DB account configuration</span></span>
<span data-ttu-id="4f066-141">tooguarantee místní čte a zapisuje, jsme musí rozdělení dat nejen na klíč oddílu, ale také na základě hello zeměpisné přístup vzoru do oblasti.</span><span class="sxs-lookup"><span data-stu-id="4f066-141">tooguarantee local reads and writes, we must partition data not just on partition key, but also based on hello geographical access pattern into regions.</span></span> <span data-ttu-id="4f066-142">Hello model spoléhá na nutnosti geograficky replikované Azure Cosmos DB databázového účtu pro každou oblast.</span><span class="sxs-lookup"><span data-stu-id="4f066-142">hello model relies on having a geo-replicated Azure Cosmos DB database account for each region.</span></span> <span data-ttu-id="4f066-143">Například se dvěma oblastmi, zde je instalace s pro zápisy více oblasti:</span><span class="sxs-lookup"><span data-stu-id="4f066-143">For example, with two regions, here's a setup for multi-region writes:</span></span>

| <span data-ttu-id="4f066-144">Název účtu</span><span class="sxs-lookup"><span data-stu-id="4f066-144">Account Name</span></span> | <span data-ttu-id="4f066-145">Zápis oblast</span><span class="sxs-lookup"><span data-stu-id="4f066-145">Write Region</span></span> | <span data-ttu-id="4f066-146">Oblast pro čtení</span><span class="sxs-lookup"><span data-stu-id="4f066-146">Read Region</span></span> |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

<span data-ttu-id="4f066-147">Hello následující diagram ukazuje, jak provádět čtení a zápisu v typické aplikaci s tímto nastavením:</span><span class="sxs-lookup"><span data-stu-id="4f066-147">hello following diagram shows how reads and writes are performed in a typical application with this setup:</span></span>

![Azure Cosmos DB více hlavních architektura](./media/multi-region-writers/multi-master.png)

<span data-ttu-id="4f066-149">Zde je fragment kódu ukazuje, jak tooinitialize hello klientům v DAL, spuštěné v hello `West US` oblast.</span><span class="sxs-lookup"><span data-stu-id="4f066-149">Here is a code snippet showing how tooinitialize hello clients in a DAL running in hello `West US` region.</span></span>
    
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

<span data-ttu-id="4f066-150">S hello před instalací může předat hello vrstva všech zápisů toohello místní účet na základě kde je nasazen.</span><span class="sxs-lookup"><span data-stu-id="4f066-150">With hello preceding setup, hello data access layer can forward all writes toohello local account based on where it is deployed.</span></span> <span data-ttu-id="4f066-151">Čtení z obou účty tooget hello globální zobrazení dat provádí čtení.</span><span class="sxs-lookup"><span data-stu-id="4f066-151">Reads are performed by reading from both accounts tooget hello global view of data.</span></span> <span data-ttu-id="4f066-152">Tento přístup může být rozšířené tooas mnoha oblastech podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4f066-152">This approach can be extended tooas many regions as required.</span></span> <span data-ttu-id="4f066-153">Zde je ukázka, instalační program s tři zeměpisné oblasti:</span><span class="sxs-lookup"><span data-stu-id="4f066-153">For example, here's a setup with three geographic regions:</span></span>

| <span data-ttu-id="4f066-154">Název účtu</span><span class="sxs-lookup"><span data-stu-id="4f066-154">Account Name</span></span> | <span data-ttu-id="4f066-155">Zápis oblast</span><span class="sxs-lookup"><span data-stu-id="4f066-155">Write Region</span></span> | <span data-ttu-id="4f066-156">Oblast pro čtení 1</span><span class="sxs-lookup"><span data-stu-id="4f066-156">Read Region 1</span></span> | <span data-ttu-id="4f066-157">Přečtěte si oblasti 2</span><span class="sxs-lookup"><span data-stu-id="4f066-157">Read Region 2</span></span> |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <span data-ttu-id="4f066-158"><a id="DataAccessImplementation"></a>Data access layer implementace</span><span class="sxs-lookup"><span data-stu-id="4f066-158"><a id="DataAccessImplementation"></a>Data access layer implementation</span></span>
<span data-ttu-id="4f066-159">Nyní Podíváme se na hello implementace hello vrstvou (DAL) pro aplikaci se dvěma oblastmi s možností zápisu.</span><span class="sxs-lookup"><span data-stu-id="4f066-159">Now let's look at hello implementation of hello data access layer (DAL) for an application with two writable regions.</span></span> <span data-ttu-id="4f066-160">Hello DAL musí implementovat hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4f066-160">hello DAL must implement hello following steps:</span></span>

* <span data-ttu-id="4f066-161">Vytvoření více instancí `DocumentClient` pro jednotlivé účty.</span><span class="sxs-lookup"><span data-stu-id="4f066-161">Create multiple instances of `DocumentClient` for each account.</span></span> <span data-ttu-id="4f066-162">Se dvěma oblastmi jeden má každá instance vrstvy DAL `writeClient` a jeden `readClient`.</span><span class="sxs-lookup"><span data-stu-id="4f066-162">With two regions, each DAL instance has one `writeClient` and one `readClient`.</span></span> 
* <span data-ttu-id="4f066-163">Podle oblasti hello nasazené aplikace hello, nakonfigurovat hello koncové body pro `writeclient` a `readClient`.</span><span class="sxs-lookup"><span data-stu-id="4f066-163">Based on hello deployed region of hello application, configure hello endpoints for `writeclient` and `readClient`.</span></span> <span data-ttu-id="4f066-164">Například hello DAL nasazené v `West US` používá `contentpubdatabase-usa.documents.azure.com` pro provádění zápisy.</span><span class="sxs-lookup"><span data-stu-id="4f066-164">For example, hello DAL deployed in `West US` uses `contentpubdatabase-usa.documents.azure.com` for performing writes.</span></span> <span data-ttu-id="4f066-165">Hello DAL nasazené v `NorthEurope` používá `contentpubdatabase-europ.documents.azure.com` pro zápis.</span><span class="sxs-lookup"><span data-stu-id="4f066-165">hello DAL deployed in `NorthEurope` uses `contentpubdatabase-europ.documents.azure.com` for writes.</span></span>

<span data-ttu-id="4f066-166">S hello předcházející instalace se dá implementovat metody pro přístup k hello.</span><span class="sxs-lookup"><span data-stu-id="4f066-166">With hello preceding setup, hello data access methods can be implemented.</span></span> <span data-ttu-id="4f066-167">Zápis operací předávání hello zápisu toohello odpovídající `writeClient`.</span><span class="sxs-lookup"><span data-stu-id="4f066-167">Write operations forward hello write toohello corresponding `writeClient`.</span></span>

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

<span data-ttu-id="4f066-168">Pro čtení, oznámení a recenze, musí přečíst z oblasti a výsledky union hello jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="4f066-168">For reading notifications and reviews, you must read from both regions and union hello results as shown in hello following snippet:</span></span>

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

<span data-ttu-id="4f066-169">Proto výběr dobrý klíč rozdělení a statické dělení založené na účet, můžete dosáhnout více oblast místní zápisů a čtení pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4f066-169">Thus, by choosing a good partitioning key and static account-based partitioning, you can achieve multi-region local writes and reads using Azure Cosmos DB.</span></span>

## <span data-ttu-id="4f066-170"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f066-170"><a id="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="4f066-171">V tomto článku jsme popsané, jak je používat vzory globálně distribuované čtení zápisu více oblasti s Azure DB Cosmos jako vzorový scénář pomocí publikování obsahu.</span><span class="sxs-lookup"><span data-stu-id="4f066-171">In this article, we described how you can use globally distributed multi-region read write patterns with Azure Cosmos DB using content publishing as a sample scenario.</span></span>

* <span data-ttu-id="4f066-172">Další informace o tom, jak Azure Cosmos DB podporuje [globální distribuční](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-172">Learn about how Azure Cosmos DB supports [global distribution](distribute-data-globally.md)</span></span>
* <span data-ttu-id="4f066-173">Další informace o [automatickou a ruční převzetí služeb při selhání v Azure Cosmos DB](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-173">Learn about [automatic and manual failovers in Azure Cosmos DB](regional-failover.md)</span></span>
* <span data-ttu-id="4f066-174">Další informace o [globální konzistence s Azure Cosmos DB](consistency-levels.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-174">Learn about [global consistency with Azure Cosmos DB](consistency-levels.md)</span></span>
* <span data-ttu-id="4f066-175">Vývoj s více oblastí pomocí hello [DB Cosmos Azure - DocumentDB rozhraní API](tutorial-global-distribution-documentdb.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-175">Develop with multiple regions using hello [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span></span>
* <span data-ttu-id="4f066-176">Vývoj s více oblastí pomocí hello [Azure Cosmos DB - MongoDB rozhraní API](tutorial-global-distribution-MongoDB.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-176">Develop with multiple regions using hello [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span></span>
* <span data-ttu-id="4f066-177">Vývoj s více oblastí pomocí hello [Azure Cosmos DB - API tabulky](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="4f066-177">Develop with multiple regions using hello [Azure Cosmos DB - Table API](tutorial-global-distribution-table.md)</span></span>
