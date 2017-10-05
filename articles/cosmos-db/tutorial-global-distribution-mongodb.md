---
title: "Globální distribuční kurz pro Azure Cosmos DB pro MongoDB API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro MongoDB."
services: cosmos-db
keywords: "globální distribuční, MongoDB"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a2747102f4d8cac412b67abc3fd07cfa3661bcee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a><span data-ttu-id="8bb78-104">Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="8bb78-104">How to setup Azure Cosmos DB global distribution using the MongoDB API</span></span>

<span data-ttu-id="8bb78-105">V tomto článku jsme ukazují, jak pomocí portálu Azure nastavit globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8bb78-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the MongoDB API.</span></span>

<span data-ttu-id="8bb78-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="8bb78-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8bb78-107">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8bb78-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="8bb78-108">Nakonfigurujte globální distribuční pomocí [MongoDB rozhraní API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="8bb78-108">Configure global distribution using the [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a><span data-ttu-id="8bb78-109">Ověření vašeho regionální nastavení pomocí rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="8bb78-109">Verifying your regional setup using the MongoDB API</span></span>
<span data-ttu-id="8bb78-110">Nejjednodušší způsob dvojitou Kontrola globální konfigurace v rámci rozhraní API pro MongoDB je ke spuštění *isMaster()* příkazu z prostředí Mongo.</span><span class="sxs-lookup"><span data-stu-id="8bb78-110">The simplest way of double checking your global configuration within API for MongoDB is to run the *isMaster()* command from the Mongo Shell.</span></span>

<span data-ttu-id="8bb78-111">Z vašeho prostředí Mongo:</span><span class="sxs-lookup"><span data-stu-id="8bb78-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="8bb78-112">Příklad výsledků:</span><span class="sxs-lookup"><span data-stu-id="8bb78-112">Example results:</span></span>

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a><span data-ttu-id="8bb78-113">Připojování k upřednostňovaná oblast pomocí rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="8bb78-113">Connecting to a preferred region using the MongoDB API</span></span>

<span data-ttu-id="8bb78-114">Rozhraní API MongoDB umožňuje určit vaší kolekce čtení předvoleb globálně distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="8bb78-114">The MongoDB API enables you to specify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="8bb78-115">Pro obě nízká latence čtení a globální vysokou dostupnost, doporučujeme, aby nastavení předvoleb čtení vaší kolekce *nejbližší*.</span><span class="sxs-lookup"><span data-stu-id="8bb78-115">For both low latency reads and global high availability, we recommend setting your collection's read preference to *nearest*.</span></span> <span data-ttu-id="8bb78-116">Přečtěte si předvolbu *nejbližší* nakonfigurovaný tak, aby číst z nejbližší oblast.</span><span class="sxs-lookup"><span data-stu-id="8bb78-116">A read preference of *nearest* is configured to read from the closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="8bb78-117">Pro aplikace s primární pro čtení a zápis oblast a sekundární oblasti pro obnovení po havárii (DR) scénáře, doporučujeme, aby nastavení předvoleb čtení vaší kolekce *sekundární upřednostňovaný*.</span><span class="sxs-lookup"><span data-stu-id="8bb78-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference to *secondary preferred*.</span></span> <span data-ttu-id="8bb78-118">Přečtěte si předvolbu *sekundární upřednostňovaný* nakonfigurovaný tak, aby ke čtení z oblasti sekundární, pokud primární oblasti není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8bb78-118">A read preference of *secondary preferred* is configured to read from the secondary region when the primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="8bb78-119">Nakonec pokud chcete ručně zadejte vaše čtení oblasti.</span><span class="sxs-lookup"><span data-stu-id="8bb78-119">Lastly, if you would like to manually specify your read regions.</span></span> <span data-ttu-id="8bb78-120">Oblast značky lze nastavit v rámci vaši volbu pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8bb78-120">You can set the region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="8bb78-121">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8bb78-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="8bb78-122">Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="8bb78-122">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="8bb78-123">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="8bb78-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bb78-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bb78-124">Next steps</span></span>

<span data-ttu-id="8bb78-125">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="8bb78-125">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bb78-126">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8bb78-126">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="8bb78-127">Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="8bb78-127">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="8bb78-128">Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8bb78-128">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8bb78-129">Vývoj místně pomocí emulátoru</span><span class="sxs-lookup"><span data-stu-id="8bb78-129">Develop locally with the emulator</span></span>](local-emulator.md)