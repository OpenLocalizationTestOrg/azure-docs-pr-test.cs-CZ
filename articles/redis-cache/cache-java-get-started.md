---
title: aaaHow toouse Azure Redis Cache s Javou | Microsoft Docs
description: "Začínáme s Azure Redis Cache pomocí Javy"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 04/13/2017
ms.author: sdanie
ms.openlocfilehash: 7768e879d71f61585b59cf4bd6634ba3f12e001d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-java"></a>Jak toouse Azure Redis mezipaměti s Javou
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache vám přístup tooa vyhrazené mezipaměti Redis spravované microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak tooget pracovat s Azure Redis Cache pomocí Javy.

## <a name="prerequisites"></a>Požadavky
[Jedis](https://github.com/xetorthio/jedis) – Java klient pro Redis

V tomto kurzu používáme Jedis, ale můžete použít jakéhokoli Java klienta uvedeného na stránce [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Načtení hello hostitele název a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Připojit toohello mezipaměti zabezpečené pomocí protokolu SSL
Hello nejnovější sestavení [jedis](https://github.com/xetorthio/jedis) poskytovat podporu pro připojení tooAzure Redis Cache pomocí protokolu SSL. Hello následující příklad ukazuje, jak tooconnect tooAzure Redis Cache pomocí hello 6380 koncový bod protokolu SSL. Nahraďte `<name>` s hello názvem vaší mezipaměti a `<key>` s buď primární nebo sekundární klíč jak je popsáno v hello předchozí [načíst název a přístupových klíčů hostitele hello](#retrieve-the-host-name-and-access-keys) části.

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> Hello port bez SSL pro nové instance služby Azure Redis Cache zakázán. Pokud používáte jiný klient, který nepodporuje protokol SSL, najdete v části [jak tooenable hello port bez SSL](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Přidání dat toohello mezipaměti a jejich načtení
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , abyste mohli [monitorování](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello stav svojí mezipaměti.
* Čtení hello oficiální [dokumentaci k Redis](http://redis.io/documentation).
