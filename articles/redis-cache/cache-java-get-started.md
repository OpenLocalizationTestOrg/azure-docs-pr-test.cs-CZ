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
# <a name="how-toouse-azure-redis-cache-with-java"></a><span data-ttu-id="70082-103">Jak toouse Azure Redis mezipaměti s Javou</span><span class="sxs-lookup"><span data-stu-id="70082-103">How toouse Azure Redis Cache with Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="70082-104">.NET</span><span class="sxs-lookup"><span data-stu-id="70082-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="70082-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="70082-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="70082-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="70082-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="70082-107">Java</span><span class="sxs-lookup"><span data-stu-id="70082-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="70082-108">Python</span><span class="sxs-lookup"><span data-stu-id="70082-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="70082-109">Azure Redis Cache vám přístup tooa vyhrazené mezipaměti Redis spravované microsoftem.</span><span class="sxs-lookup"><span data-stu-id="70082-109">Azure Redis Cache gives you access tooa dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="70082-110">Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="70082-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="70082-111">Toto téma ukazuje, jak tooget pracovat s Azure Redis Cache pomocí Javy.</span><span class="sxs-lookup"><span data-stu-id="70082-111">This topic shows you how tooget started with Azure Redis Cache using Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70082-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70082-112">Prerequisites</span></span>
<span data-ttu-id="70082-113">[Jedis](https://github.com/xetorthio/jedis) – Java klient pro Redis</span><span class="sxs-lookup"><span data-stu-id="70082-113">[Jedis](https://github.com/xetorthio/jedis) - Java client for Redis</span></span>

<span data-ttu-id="70082-114">V tomto kurzu používáme Jedis, ale můžete použít jakéhokoli Java klienta uvedeného na stránce [http://redis.io/clients](http://redis.io/clients).</span><span class="sxs-lookup"><span data-stu-id="70082-114">This tutorial uses Jedis, but you can use any Java client listed at [http://redis.io/clients](http://redis.io/clients).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="70082-115">Vytvoření mezipaměti Redis v Azure</span><span class="sxs-lookup"><span data-stu-id="70082-115">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="70082-116">Načtení hello hostitele název a přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="70082-116">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="70082-117">Připojit toohello mezipaměti zabezpečené pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="70082-117">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="70082-118">Hello nejnovější sestavení [jedis](https://github.com/xetorthio/jedis) poskytovat podporu pro připojení tooAzure Redis Cache pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="70082-118">hello latest builds of [jedis](https://github.com/xetorthio/jedis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="70082-119">Hello následující příklad ukazuje, jak tooconnect tooAzure Redis Cache pomocí hello 6380 koncový bod protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="70082-119">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="70082-120">Nahraďte `<name>` s hello názvem vaší mezipaměti a `<key>` s buď primární nebo sekundární klíč jak je popsáno v hello předchozí [načíst název a přístupových klíčů hostitele hello](#retrieve-the-host-name-and-access-keys) části.</span><span class="sxs-lookup"><span data-stu-id="70082-120">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> <span data-ttu-id="70082-121">Hello port bez SSL pro nové instance služby Azure Redis Cache zakázán.</span><span class="sxs-lookup"><span data-stu-id="70082-121">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="70082-122">Pokud používáte jiný klient, který nepodporuje protokol SSL, najdete v části [jak tooenable hello port bez SSL](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="70082-122">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="70082-123">Přidání dat toohello mezipaměti a jejich načtení</span><span class="sxs-lookup"><span data-stu-id="70082-123">Add something toohello cache and retrieve it</span></span>
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


## <a name="next-steps"></a><span data-ttu-id="70082-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70082-124">Next steps</span></span>
* <span data-ttu-id="70082-125">[Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , abyste mohli [monitorování](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello stav svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="70082-125">[Enable cache diagnostics](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) so you can [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello health of your cache.</span></span>
* <span data-ttu-id="70082-126">Čtení hello oficiální [dokumentaci k Redis](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="70082-126">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>
