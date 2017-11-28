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
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a><span data-ttu-id="7efc5-103">Jak toouse Azure Redis mezipaměti s Node.js</span><span class="sxs-lookup"><span data-stu-id="7efc5-103">How toouse Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7efc5-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7efc5-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="7efc5-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7efc5-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="7efc5-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="7efc5-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="7efc5-107">Java</span><span class="sxs-lookup"><span data-stu-id="7efc5-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="7efc5-108">Python</span><span class="sxs-lookup"><span data-stu-id="7efc5-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="7efc5-109">Azure Redis Cache vám přístup tooa zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem.</span><span class="sxs-lookup"><span data-stu-id="7efc5-109">Azure Redis Cache gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="7efc5-110">Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7efc5-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="7efc5-111">Toto téma ukazuje, jak tooget pracovat s Azure Redis Cache pomocí Node.js.</span><span class="sxs-lookup"><span data-stu-id="7efc5-111">This topic shows you how tooget started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="7efc5-112">Další příklad použití Azure Redis Cache s Node.js najdete v tématu [Sestavení aplikace Chat v Node.js se Socket.IO na webu Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="7efc5-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7efc5-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7efc5-113">Prerequisites</span></span>
<span data-ttu-id="7efc5-114">Nainstalujte [node_redis](https://github.com/mranney/node_redis):</span><span class="sxs-lookup"><span data-stu-id="7efc5-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="7efc5-115">Tento kurz používá [node_redis](https://github.com/mranney/node_redis).</span><span class="sxs-lookup"><span data-stu-id="7efc5-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="7efc5-116">Příklady použití jiných klientů Node.js, najdete v části hello jednotlivých dokumentace pro klienty Node.js hello uvedený na [klientů Node.js Redis](http://redis.io/clients#nodejs).</span><span class="sxs-lookup"><span data-stu-id="7efc5-116">For examples of using other Node.js clients, see hello individual documentation for hello Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="7efc5-117">Vytvoření mezipaměti Redis v Azure</span><span class="sxs-lookup"><span data-stu-id="7efc5-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="7efc5-118">Načtení hello hostitele název a přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="7efc5-118">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="7efc5-119">Připojit toohello mezipaměti zabezpečené pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7efc5-119">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="7efc5-120">Hello nejnovější sestavení [node_redis](https://github.com/mranney/node_redis) poskytovat podporu pro připojení tooAzure Redis Cache pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="7efc5-120">hello latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="7efc5-121">Hello následující příklad ukazuje, jak tooconnect tooAzure Redis Cache pomocí hello 6380 koncový bod protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="7efc5-121">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="7efc5-122">Nahraďte `<name>` s hello názvem vaší mezipaměti a `<key>` s buď primární nebo sekundární klíč jak je popsáno v hello předchozí [načíst název a přístupových klíčů hostitele hello](#retrieve-the-host-name-and-access-keys) části.</span><span class="sxs-lookup"><span data-stu-id="7efc5-122">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="7efc5-123">Hello port bez SSL pro nové instance služby Azure Redis Cache zakázán.</span><span class="sxs-lookup"><span data-stu-id="7efc5-123">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="7efc5-124">Pokud používáte jiný klient, který nepodporuje protokol SSL, najdete v části [jak tooenable hello port bez SSL](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="7efc5-124">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="7efc5-125">Přidání dat toohello mezipaměti a jejich načtení</span><span class="sxs-lookup"><span data-stu-id="7efc5-125">Add something toohello cache and retrieve it</span></span>
<span data-ttu-id="7efc5-126">Následující příklad ukazuje můžete jak tooconnect tooan Azure Redis instance, do mezipaměti a ukládání a načítání položky z mezipaměti hello Hello.</span><span class="sxs-lookup"><span data-stu-id="7efc5-126">hello following example shows you how tooconnect tooan Azure Redis Cache instance, and store and retrieve an item from hello cache.</span></span> <span data-ttu-id="7efc5-127">Další příklady použití Redis s hello [node_redis](https://github.com/mranney/node_redis) klienta, najdete v části [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="7efc5-127">For more examples of using Redis with hello [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="7efc5-128">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7efc5-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="7efc5-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7efc5-129">Next steps</span></span>
* <span data-ttu-id="7efc5-130">[Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , abyste mohli [monitorování](cache-how-to-monitor.md) hello stav svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7efc5-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span>
* <span data-ttu-id="7efc5-131">Čtení hello oficiální [dokumentaci k Redis](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="7efc5-131">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>

