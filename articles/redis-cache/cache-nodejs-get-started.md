---
title: "Použití Azure Redis Cache s Node.js | Dokumentace Microsoftu"
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
ms.openlocfilehash: 530191637b1aa91ee1d7fe5b5bb032c60983f7dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a><span data-ttu-id="7f175-103">Použití Azure Redis Cache s Node.js</span><span class="sxs-lookup"><span data-stu-id="7f175-103">How to use Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f175-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7f175-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="7f175-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f175-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="7f175-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="7f175-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="7f175-107">Java</span><span class="sxs-lookup"><span data-stu-id="7f175-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="7f175-108">Python</span><span class="sxs-lookup"><span data-stu-id="7f175-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="7f175-109">Azure Redis Cache vám umožňuje přístup do zabezpečené, vyhrazené mezipaměti Redis spravované Microsoftem.</span><span class="sxs-lookup"><span data-stu-id="7f175-109">Azure Redis Cache gives you access to a secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="7f175-110">Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7f175-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="7f175-111">Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Node.js.</span><span class="sxs-lookup"><span data-stu-id="7f175-111">This topic shows you how to get started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="7f175-112">Další příklad použití Azure Redis Cache s Node.js najdete v tématu [Sestavení aplikace Chat v Node.js se Socket.IO na webu Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="7f175-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f175-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f175-113">Prerequisites</span></span>
<span data-ttu-id="7f175-114">Nainstalujte [node_redis](https://github.com/mranney/node_redis):</span><span class="sxs-lookup"><span data-stu-id="7f175-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="7f175-115">Tento kurz používá [node_redis](https://github.com/mranney/node_redis).</span><span class="sxs-lookup"><span data-stu-id="7f175-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="7f175-116">Příklady použití dalších klientů Node.js najdete v individuální dokumentaci pro klienty Node.js uvedené v [klientech Node.js Redis](http://redis.io/clients#nodejs).</span><span class="sxs-lookup"><span data-stu-id="7f175-116">For examples of using other Node.js clients, see the individual documentation for the Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="7f175-117">Vytvoření mezipaměti Redis v Azure</span><span class="sxs-lookup"><span data-stu-id="7f175-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a><span data-ttu-id="7f175-118">Načtení názvu hostitele a přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="7f175-118">Retrieve the host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a><span data-ttu-id="7f175-119">Bezpečné připojení k mezipaměti pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7f175-119">Connect to the cache securely using SSL</span></span>
<span data-ttu-id="7f175-120">Nejnovější sestavení [node_redis](https://github.com/mranney/node_redis) poskytuje podporu pro připojení k mezipaměti Azure Redis pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="7f175-120">The latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting to Azure Redis Cache using SSL.</span></span> <span data-ttu-id="7f175-121">Následující příklad ukazuje, jak se připojit k mezipaměti Redis Azure pomocí protokolu SSL koncového bodu 6380.</span><span class="sxs-lookup"><span data-stu-id="7f175-121">The following example shows how to connect to Azure Redis Cache using the SSL endpoint of 6380.</span></span> <span data-ttu-id="7f175-122">Nahraďte `<name>` názvem mezipaměti a `<key>` primárním nebo sekundárním klíčem popsaným v předchozí části [Načtení názvu hostitele a přístupových klíčů](#retrieve-the-host-name-and-access-keys).</span><span class="sxs-lookup"><span data-stu-id="7f175-122">Replace `<name>` with the name of your cache and `<key>` with either your primary or secondary key as described in the previous [Retrieve the host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="7f175-123">Port bez SSL je pro nové instance Azure Redis Cache zakázaný.</span><span class="sxs-lookup"><span data-stu-id="7f175-123">The non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="7f175-124">Pokud používáte jiného klienta, který nepodporuje SSL, přečtěte si téma věnované tomu, jak [povolit port bez SSL](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="7f175-124">If you are using a different client that doesn't support SSL, see [How to enable the non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a><span data-ttu-id="7f175-125">Přidání dat do mezipaměti a jejich načtení</span><span class="sxs-lookup"><span data-stu-id="7f175-125">Add something to the cache and retrieve it</span></span>
<span data-ttu-id="7f175-126">Následující příklad ukazuje, jak se připojit k instanci mezipaměti Redis Azure a uložit a načíst položku z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f175-126">The following example shows you how to connect to an Azure Redis Cache instance, and store and retrieve an item from the cache.</span></span> <span data-ttu-id="7f175-127">Pro další příklady použití Redis pomocí klienta [node_redis](https://github.com/mranney/node_redis) si zobrazte [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="7f175-127">For more examples of using Redis with the [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="7f175-128">Výstup:</span><span class="sxs-lookup"><span data-stu-id="7f175-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="7f175-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f175-129">Next steps</span></span>
* <span data-ttu-id="7f175-130">[Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics), abyste mohli [monitorovat](cache-how-to-monitor.md) stav svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7f175-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) the health of your cache.</span></span>
* <span data-ttu-id="7f175-131">Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="7f175-131">Read the official [Redis documentation](http://redis.io/documentation).</span></span>

