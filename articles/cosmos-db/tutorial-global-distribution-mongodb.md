---
title: "kurz globální distribuční Cosmos DB aaaAzure pro MongoDB API | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello rozhraní API MongoDB."
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
ms.openlocfilehash: 0fc2d670bb4e21ac5f813f9586b407ba06ccf354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a><span data-ttu-id="47339-104">Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API MongoDB hello</span><span class="sxs-lookup"><span data-stu-id="47339-104">How toosetup Azure Cosmos DB global distribution using hello MongoDB API</span></span>

<span data-ttu-id="47339-105">V tomto článku ukážeme, jak toouse hello Azure portálu toosetup globální distribuční databázi Cosmos Azure a potom se připojte pomocí hello rozhraní API MongoDB.</span><span class="sxs-lookup"><span data-stu-id="47339-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello MongoDB API.</span></span>

<span data-ttu-id="47339-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="47339-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="47339-107">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="47339-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="47339-108">Nakonfigurujte globální distribuci pomocí hello [MongoDB rozhraní API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="47339-108">Configure global distribution using hello [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a><span data-ttu-id="47339-109">Ověření vaší místní instalaci pomocí hello MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="47339-109">Verifying your regional setup using hello MongoDB API</span></span>
<span data-ttu-id="47339-110">Nejjednodušší způsob Hello dvojitou Kontrola globální konfigurace v rámci rozhraní API pro MongoDB je toorun hello *isMaster()* příkazu z prostředí Mongo hello.</span><span class="sxs-lookup"><span data-stu-id="47339-110">hello simplest way of double checking your global configuration within API for MongoDB is toorun hello *isMaster()* command from hello Mongo Shell.</span></span>

<span data-ttu-id="47339-111">Z vašeho prostředí Mongo:</span><span class="sxs-lookup"><span data-stu-id="47339-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="47339-112">Příklad výsledků:</span><span class="sxs-lookup"><span data-stu-id="47339-112">Example results:</span></span>

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

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a><span data-ttu-id="47339-113">Připojení tooa upřednostňovaná oblast pomocí hello MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="47339-113">Connecting tooa preferred region using hello MongoDB API</span></span>

<span data-ttu-id="47339-114">Hello MongoDB API umožňuje vám toospecify vaší kolekce čtení předvoleb globálně distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="47339-114">hello MongoDB API enables you toospecify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="47339-115">Pro obě nízká latence čtení a globální vysokou dostupnost, doporučujeme, aby nastavení předvoleb čtení vaší kolekce příliš*nejbližší*.</span><span class="sxs-lookup"><span data-stu-id="47339-115">For both low latency reads and global high availability, we recommend setting your collection's read preference too*nearest*.</span></span> <span data-ttu-id="47339-116">Přečtěte si předvolbu *nejbližší* je nakonfigurované tooread z hello nejbližší oblast.</span><span class="sxs-lookup"><span data-stu-id="47339-116">A read preference of *nearest* is configured tooread from hello closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="47339-117">Pro aplikace s primární pro čtení a zápis oblast a sekundární oblasti pro obnovení po havárii (DR) scénáře, doporučujeme, aby nastavení předvoleb čtení vaší kolekce příliš*sekundární upřednostňovaný*.</span><span class="sxs-lookup"><span data-stu-id="47339-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference too*secondary preferred*.</span></span> <span data-ttu-id="47339-118">Přečtěte si předvolbu *sekundární upřednostňovaný* je nakonfigurované tooread ze sekundární oblasti hello, když primární oblasti hello není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="47339-118">A read preference of *secondary preferred* is configured tooread from hello secondary region when hello primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="47339-119">Nakonec pokud chcete jako toomanually zadejte vaše čtení oblasti.</span><span class="sxs-lookup"><span data-stu-id="47339-119">Lastly, if you would like toomanually specify your read regions.</span></span> <span data-ttu-id="47339-120">Oblast hello značky lze nastavit v rámci vaši volbu pro čtení.</span><span class="sxs-lookup"><span data-stu-id="47339-120">You can set hello region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="47339-121">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="47339-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="47339-122">Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="47339-122">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="47339-123">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="47339-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47339-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47339-124">Next steps</span></span>

<span data-ttu-id="47339-125">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="47339-125">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="47339-126">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="47339-126">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="47339-127">Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="47339-127">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="47339-128">Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47339-128">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="47339-129">Vývoj místně pomocí emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="47339-129">Develop locally with hello emulator</span></span>](local-emulator.md)
