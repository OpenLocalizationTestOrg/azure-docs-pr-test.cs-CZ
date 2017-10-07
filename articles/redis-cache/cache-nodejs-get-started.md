---
title: aaaHow toouse Azure Redis Cache s Node.js | Microsoft Docs
description: "Začínáme s Azure Redis Cache pomocí Node.js a node_redis."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: dc8732041d2c4e5793e684e0c80b87a1c9d17f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a>Jak toouse Azure Redis mezipaměti s Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache vám přístup tooa zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak tooget pracovat s Azure Redis Cache pomocí Node.js. Další příklad použití Azure Redis Cache s Node.js najdete v tématu [Sestavení aplikace Chat v Node.js se Socket.IO na webu Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).

## <a name="prerequisites"></a>Požadavky
Nainstalujte [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Tento kurz používá [node_redis](https://github.com/mranney/node_redis). Příklady použití jiných klientů Node.js, najdete v části hello jednotlivých dokumentace pro klienty Node.js hello uvedený na [klientů Node.js Redis](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Načtení hello hostitele název a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Připojit toohello mezipaměti zabezpečené pomocí protokolu SSL
Hello nejnovější sestavení [node_redis](https://github.com/mranney/node_redis) poskytovat podporu pro připojení tooAzure Redis Cache pomocí protokolu SSL. Hello následující příklad ukazuje, jak tooconnect tooAzure Redis Cache pomocí hello 6380 koncový bod protokolu SSL. Nahraďte `<name>` s hello názvem vaší mezipaměti a `<key>` s buď primární nebo sekundární klíč jak je popsáno v hello předchozí [načíst název a přístupových klíčů hostitele hello](#retrieve-the-host-name-and-access-keys) části.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> Hello port bez SSL pro nové instance služby Azure Redis Cache zakázán. Pokud používáte jiný klient, který nepodporuje protokol SSL, najdete v části [jak tooenable hello port bez SSL](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Přidání dat toohello mezipaměti a jejich načtení
Následující příklad ukazuje můžete jak tooconnect tooan Azure Redis instance, do mezipaměti a ukládání a načítání položky z mezipaměti hello Hello. Další příklady použití Redis s hello [node_redis](https://github.com/mranney/node_redis) klienta, najdete v části [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Výstup:

    OK
    value


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , abyste mohli [monitorování](cache-how-to-monitor.md) hello stav svojí mezipaměti.
* Čtení hello oficiální [dokumentaci k Redis](http://redis.io/documentation).

