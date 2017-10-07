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
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a>Více hlavní globálně replikované databáze architektury s Azure Cosmos DB
Podporuje Azure Cosmos DB připraveného [globální replikace](distribute-data-globally.md), což vám umožní toodistribute datové oblasti toomultiple přístup s nízkou latencí kdekoli v hello zatížení. Tento model se často používá pro vydavatele nebo příjemce zatížení tam, kde je zapisovač v jedné zeměpisné oblasti a globálně distribuované čtečky v jiných oblastech (čtení). 

Můžete také použít Azure Cosmos DB globální replikace podporuje toobuild aplikace, ve kterých jsou globálně distribuované zapisovače a čtečky. Tento dokument popisuje vzor, který umožňuje dosažení místní zápisu a čtení místní pro distribuované zapisovače pomocí Azure Cosmos DB.

## <a id="ExampleScenario"></a>Publikování obsahu – příklad scénáře
Podívejme se na toodescribe skutečných scénář použití globálně distribuované více region nebo více master čtení zápisu vzory s Azure Cosmos DB. Vezměte v úvahu vytvořené v Azure Cosmos DB platforma pro publikování obsahu. Tady jsou některé požadavky, které tuto platformu musí splňovat pro vysoký výkon uživatele pro vydavatele a spotřebitelé.

* Autoři a Odběratelé, kteří jsou rozloženy hello, world 
* Autoři musíte publikovat (zápisu) články tootheir místní (nejbližší) oblast
* Autoři mají čtečky nebo odběratele, jejich článků kteří jsou rozmístěny v celém světě hello. 
* Odběratelé, kteří měli obdržet oznámení, při publikování nových článků.
* Odběratelé, kteří musí být schopný tooread články ze své místní oblast. Měly by být také možné tooadd recenze toothese články. 
* Každý, kdo včetně hello Autor článků hello by měl být schopný zobrazení, že všechny hello recenze připojené tooarticles z místní oblast. 

Za předpokladu, že miliony s až miliardy článků, vydavatelům a spotřebitelům brzy máme problémy hello tooconfront měřítka společně s zaručující polohu přístupu. Stejně jako u většiny problémů škálovatelnost, hello řešení spočívá ve strategii je dobré rozdělení. Dále umožňuje vyhledat jak toomodel články, zkontrolujte a oznámení jako dokumenty, nakonfigurovat účty Azure Cosmos DB a implementovat vrstva přístupu k datům. 

Pokud vás zajímají další informace o vytváření oddílů a klíče oddílů toolearn, najdete v části [vytváření oddílů a škálování v Azure Cosmos DB](partition-data.md).

## <a id="ModelingNotifications"></a>Modelování oznámení
Oznámení jsou data informační kanály konkrétní tooa uživatele. Proto hello přístupové vzorce pro dokumenty oznámení se vždycky nacházejí v kontextu hello jednoho uživatele. Například by "post uživatele tooa oznámení" nebo "načíst všechna oznámení pro daného uživatele". Ano, hello optimální volbou oddíly klíč pro tento typ by být `UserId`.

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

## <a id="ModelingSubscriptions"></a>Odběry modelování
Odběry můžete vytvořit pro různé kritéria jako určitou kategorii články zájmu nebo konkrétní vydavatele. Proto hello `SubscriptionFilter` je vhodná pro klíč oddílu.

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

## <a id="ModelingArticles"></a>Články modelování
Jakmile článek identifikuje prostřednictvím oznámení, další dotazy jsou obvykle založené na hello `Article.Id`. Výběr `Article.Id` jako oddíl hello klíč tak poskytuje hello nejlepší distribuce pro ukládání články v kolekci Azure Cosmos DB. 

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

## <a id="ModelingReviews"></a>Zkontroluje modelování
Jako články jsou recenze většinou zapisovat a číst v kontextu hello článku. Výběr `ArticleId` jako oddíl klíč poskytuje nejlepší distribuce a efektivní přístup recenze přidružené článku. 

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

## <a id="DataAccessMethods"></a>Metody pro přístup k vrstvě
Nyní Podíváme se na hlavní data hello potřebujeme tooimplement metody přístupu. Tady je seznam hello metody, které hello `ContentPublishDatabase` musí:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Konfigurace účtu Azure Cosmos DB
tooguarantee místní čte a zapisuje, jsme musí rozdělení dat nejen na klíč oddílu, ale také na základě hello zeměpisné přístup vzoru do oblasti. Hello model spoléhá na nutnosti geograficky replikované Azure Cosmos DB databázového účtu pro každou oblast. Například se dvěma oblastmi, zde je instalace s pro zápisy více oblasti:

| Název účtu | Zápis oblast | Oblast pro čtení |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

Hello následující diagram ukazuje, jak provádět čtení a zápisu v typické aplikaci s tímto nastavením:

![Azure Cosmos DB více hlavních architektura](./media/multi-region-writers/multi-master.png)

Zde je fragment kódu ukazuje, jak tooinitialize hello klientům v DAL, spuštěné v hello `West US` oblast.
    
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

S hello před instalací může předat hello vrstva všech zápisů toohello místní účet na základě kde je nasazen. Čtení z obou účty tooget hello globální zobrazení dat provádí čtení. Tento přístup může být rozšířené tooas mnoha oblastech podle potřeby. Zde je ukázka, instalační program s tři zeměpisné oblasti:

| Název účtu | Zápis oblast | Oblast pro čtení 1 | Přečtěte si oblasti 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Data access layer implementace
Nyní Podíváme se na hello implementace hello vrstvou (DAL) pro aplikaci se dvěma oblastmi s možností zápisu. Hello DAL musí implementovat hello následující kroky:

* Vytvoření více instancí `DocumentClient` pro jednotlivé účty. Se dvěma oblastmi jeden má každá instance vrstvy DAL `writeClient` a jeden `readClient`. 
* Podle oblasti hello nasazené aplikace hello, nakonfigurovat hello koncové body pro `writeclient` a `readClient`. Například hello DAL nasazené v `West US` používá `contentpubdatabase-usa.documents.azure.com` pro provádění zápisy. Hello DAL nasazené v `NorthEurope` používá `contentpubdatabase-europ.documents.azure.com` pro zápis.

S hello předcházející instalace se dá implementovat metody pro přístup k hello. Zápis operací předávání hello zápisu toohello odpovídající `writeClient`.

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

Pro čtení, oznámení a recenze, musí přečíst z oblasti a výsledky union hello jak je znázorněno v následujícím fragmentu kódu hello:

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

Proto výběr dobrý klíč rozdělení a statické dělení založené na účet, můžete dosáhnout více oblast místní zápisů a čtení pomocí Azure Cosmos DB.

## <a id="NextSteps"></a>Další kroky
V tomto článku jsme popsané, jak je používat vzory globálně distribuované čtení zápisu více oblasti s Azure DB Cosmos jako vzorový scénář pomocí publikování obsahu.

* Další informace o tom, jak Azure Cosmos DB podporuje [globální distribuční](distribute-data-globally.md)
* Další informace o [automatickou a ruční převzetí služeb při selhání v Azure Cosmos DB](regional-failover.md)
* Další informace o [globální konzistence s Azure Cosmos DB](consistency-levels.md)
* Vývoj s více oblastí pomocí hello [DB Cosmos Azure - DocumentDB rozhraní API](tutorial-global-distribution-documentdb.md)
* Vývoj s více oblastí pomocí hello [Azure Cosmos DB - MongoDB rozhraní API](tutorial-global-distribution-MongoDB.md)
* Vývoj s více oblastí pomocí hello [Azure Cosmos DB - API tabulky](tutorial-global-distribution-table.md)
