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
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a>Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API MongoDB hello

V tomto článku ukážeme, jak toouse hello Azure portálu toosetup globální distribuční databázi Cosmos Azure a potom se připojte pomocí hello rozhraní API MongoDB.

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello [MongoDB rozhraní API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a>Ověření vaší místní instalaci pomocí hello MongoDB rozhraní API
Nejjednodušší způsob Hello dvojitou Kontrola globální konfigurace v rámci rozhraní API pro MongoDB je toorun hello *isMaster()* příkazu z prostředí Mongo hello.

Z vašeho prostředí Mongo:

   ```
      db.isMaster()
   ```
   
Příklad výsledků:

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

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a>Připojení tooa upřednostňovaná oblast pomocí hello MongoDB rozhraní API

Hello MongoDB API umožňuje vám toospecify vaší kolekce čtení předvoleb globálně distribuované databáze. Pro obě nízká latence čtení a globální vysokou dostupnost, doporučujeme, aby nastavení předvoleb čtení vaší kolekce příliš*nejbližší*. Přečtěte si předvolbu *nejbližší* je nakonfigurované tooread z hello nejbližší oblast.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Pro aplikace s primární pro čtení a zápis oblast a sekundární oblasti pro obnovení po havárii (DR) scénáře, doporučujeme, aby nastavení předvoleb čtení vaší kolekce příliš*sekundární upřednostňovaný*. Přečtěte si předvolbu *sekundární upřednostňovaný* je nakonfigurované tooread ze sekundární oblasti hello, když primární oblasti hello není k dispozici.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Nakonec pokud chcete jako toomanually zadejte vaše čtení oblasti. Oblast hello značky lze nastavit v rámci vaši volbu pro čtení.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Je to, že dokončení tohoto kurzu. Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API

Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru hello](local-emulator.md)
