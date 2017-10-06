---
title: aaaHow tooUse Azure Redis Cache | Microsoft Docs
description: "Zjistěte, jak tooimprove hello výkon aplikací Azure pomocí Azure Redis Cache"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 763d70c10972eec9a1885969e8da5bf1c4084727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache"></a><span data-ttu-id="8b6bd-103">Jak tooUse Azure mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="8b6bd-103">How tooUse Azure Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8b6bd-104">.NET</span><span class="sxs-lookup"><span data-stu-id="8b6bd-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="8b6bd-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b6bd-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="8b6bd-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="8b6bd-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="8b6bd-107">Java</span><span class="sxs-lookup"><span data-stu-id="8b6bd-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="8b6bd-108">Python</span><span class="sxs-lookup"><span data-stu-id="8b6bd-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="8b6bd-109">Tento průvodce vám ukáže, jak tooget spuštění pomocí **Azure Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-109">This guide shows you how tooget started using **Azure Redis Cache**.</span></span> <span data-ttu-id="8b6bd-110">Microsoft Azure Redis Cache je založená na populární open source hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-110">Microsoft Azure Redis Cache is based on hello popular open source Redis Cache.</span></span> <span data-ttu-id="8b6bd-111">Nabízí přístup k tooa zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-111">It gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="8b6bd-112">Mezipaměť vytvořená pomocí Azure Redis Cache je přístupná ze všech aplikací v rámci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-112">A cache created using Azure Redis Cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="8b6bd-113">Microsoft Azure Redis Cache je dostupná v hello následující úrovně:</span><span class="sxs-lookup"><span data-stu-id="8b6bd-113">Microsoft Azure Redis Cache is available in hello following tiers:</span></span>

* <span data-ttu-id="8b6bd-114">**Basic** – jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-114">**Basic** – Single node.</span></span> <span data-ttu-id="8b6bd-115">Více velikostí až too53 GB.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-115">Multiple sizes up too53 GB.</span></span>
* <span data-ttu-id="8b6bd-116">**Standard** – dva uzly Primární/Replika.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-116">**Standard** – Two-node Primary/Replica.</span></span> <span data-ttu-id="8b6bd-117">Více velikostí až too53 GB.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-117">Multiple sizes up too53 GB.</span></span> <span data-ttu-id="8b6bd-118">99,9% SLA.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-118">99.9% SLA.</span></span>
* <span data-ttu-id="8b6bd-119">**Premium** – dva uzly primární/replika s po až too10 horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-119">**Premium** – Two-node Primary/Replica with up too10 shards.</span></span> <span data-ttu-id="8b6bd-120">Více velikosti od 6 GB too530 GB.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-120">Multiple sizes from 6 GB too530 GB.</span></span> <span data-ttu-id="8b6bd-121">Všechny funkce úrovně Standard a navíc podpora [clusteru Redis](cache-how-to-premium-clustering.md), [trvalosti Redis](cache-how-to-premium-persistence.md) a [služby Azure Virtual Network](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="8b6bd-121">All Standard tier features and more including support for [Redis cluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="8b6bd-122">99,9% SLA.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-122">99.9% SLA.</span></span>

<span data-ttu-id="8b6bd-123">Každá úroveň se liší z hlediska funkcí a cen.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-123">Each tier differs in terms of features and pricing.</span></span> <span data-ttu-id="8b6bd-124">Informace o cenách najdete na stránce [Podrobnosti o cenách Azure Redis Cache][Cache Pricing Details].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-124">For information on pricing, see [Cache Pricing Details][Cache Pricing Details].</span></span>

<span data-ttu-id="8b6bd-125">Tento průvodce vám ukáže, jak toouse hello [StackExchange.Redis] [ StackExchange.Redis] klienta pomocí C\# kódu.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-125">This guide shows you how toouse hello [StackExchange.Redis][StackExchange.Redis] client using C\# code.</span></span> <span data-ttu-id="8b6bd-126">Hello pokryté scénáře zahrnují **vytváření a konfiguraci mezipaměti**, **konfiguraci klientů mezipaměti**, a **přidávání a odebírání objektů z mezipaměti hello**.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-126">hello scenarios covered include **creating and configuring a cache**, **configuring cache clients**, and **adding and removing objects from hello cache**.</span></span> <span data-ttu-id="8b6bd-127">Další informace o používání Azure Redis Cache najdete v části [Další kroky][Next Steps].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-127">For more information on using Azure Redis Cache, see [Next Steps][Next Steps].</span></span> <span data-ttu-id="8b6bd-128">Podrobný kurz vytvoření webové aplikace s Redis Cache ASP.NET MVC, najdete v části [jak toocreate webové aplikace s Redis Cache](cache-web-app-howto.md).</span><span class="sxs-lookup"><span data-stu-id="8b6bd-128">For a step-by-step tutorial of building an ASP.NET MVC web app with Redis Cache, see [How toocreate a Web App with Redis Cache](cache-web-app-howto.md).</span></span>

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a><span data-ttu-id="8b6bd-129">Začínáme s Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8b6bd-129">Get Started with Azure Redis Cache</span></span>
<span data-ttu-id="8b6bd-130">Začít s Azure Redis Cache je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-130">Getting started with Azure Redis Cache is easy.</span></span> <span data-ttu-id="8b6bd-131">tooget spustili, můžete zřídit a nakonfigurujete mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-131">tooget started, you provision and configure a cache.</span></span> <span data-ttu-id="8b6bd-132">Dále nakonfigurujete klienty mezipaměti hello tak bude mít přístup k mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-132">Next, you configure hello cache clients so they can access hello cache.</span></span> <span data-ttu-id="8b6bd-133">Po nakonfigurování klientů mezipaměti hello můžete začít pracovat s nimi.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-133">Once hello cache clients are configured, you can begin working with them.</span></span>

* <span data-ttu-id="8b6bd-134">[Vytvoření mezipaměti hello][Create hello cache]</span><span class="sxs-lookup"><span data-stu-id="8b6bd-134">[Create hello cache][Create hello cache]</span></span>
* <span data-ttu-id="8b6bd-135">[Konfigurace klientů mezipaměti hello][Configure hello cache clients]</span><span class="sxs-lookup"><span data-stu-id="8b6bd-135">[Configure hello cache clients][Configure hello cache clients]</span></span>

<a name="create-cache"></a>

## <a name="create-a-cache"></a><span data-ttu-id="8b6bd-136">Vytvoření mezipaměti</span><span class="sxs-lookup"><span data-stu-id="8b6bd-136">Create a cache</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a><span data-ttu-id="8b6bd-137">tooaccess vaší mezipaměti po jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="8b6bd-137">tooaccess your cache after it's created</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="8b6bd-138">Další informace o konfiguraci mezipaměti najdete v tématu [jak tooconfigure Azure Redis Cache](cache-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8b6bd-138">For more information about configuring your cache, see [How tooconfigure Azure Redis Cache](cache-configure.md).</span></span>

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a><span data-ttu-id="8b6bd-139">Konfigurace klientů mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="8b6bd-139">Configure hello cache clients</span></span>
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

<span data-ttu-id="8b6bd-140">Po konfiguraci klientského projektu je nakonfigurován pro ukládání do mezipaměti, můžete hello technik popsaných v následující části pro práci s mezipamětí hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-140">Once your client project is configured for caching, you can use hello techniques described in hello following sections for working with your cache.</span></span>

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a><span data-ttu-id="8b6bd-141">Práce s mezipamětí</span><span class="sxs-lookup"><span data-stu-id="8b6bd-141">Working with Caches</span></span>
<span data-ttu-id="8b6bd-142">Hello kroky v této části popisují, jak tooperform běžné úkoly s mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-142">hello steps in this section describe how tooperform common tasks with Cache.</span></span>

* <span data-ttu-id="8b6bd-143">[Připojit toohello mezipaměti][Connect toohello cache]</span><span class="sxs-lookup"><span data-stu-id="8b6bd-143">[Connect toohello cache][Connect toohello cache]</span></span>
* <span data-ttu-id="8b6bd-144">[Přidejte a načtení objektů z mezipaměti hello][Add and retrieve objects from hello cache]</span><span class="sxs-lookup"><span data-stu-id="8b6bd-144">[Add and retrieve objects from hello cache][Add and retrieve objects from hello cache]</span></span>
* [<span data-ttu-id="8b6bd-145">Práce s objekty .NET v mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="8b6bd-145">Work with .NET objects in hello cache</span></span>](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a><span data-ttu-id="8b6bd-146">Připojit toohello mezipaměti</span><span class="sxs-lookup"><span data-stu-id="8b6bd-146">Connect toohello cache</span></span>
<span data-ttu-id="8b6bd-147">tooprogrammatically práci s mezipamětí, potřebujete odkaz toohello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-147">tooprogrammatically work with a cache, you need a reference toohello cache.</span></span> <span data-ttu-id="8b6bd-148">Přidejte následující toohello horní části souboru, ve kterém chcete toouse hello StackExchange.Redis klienta tooaccess Azure Redis Cache hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-148">Add hello following toohello top of any file from which you want toouse hello StackExchange.Redis client tooaccess an Azure Redis Cache.</span></span>

    using StackExchange.Redis;

> [!NOTE]
> <span data-ttu-id="8b6bd-149">Hello klient StackExchange.Redis vyžaduje rozhraní .NET Framework 4 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-149">hello StackExchange.Redis client requires .NET Framework 4 or higher.</span></span>
> 
> 

<span data-ttu-id="8b6bd-150">Hello toohello připojení Azure Redis Cache spravuje hello `ConnectionMultiplexer` třídy.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-150">hello connection toohello Azure Redis Cache is managed by hello `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="8b6bd-151">Tato třída by měla být sdílena a opětovné použití v rámci klientské aplikace a není nutné toobe vytvořena na každou operaci zvlášť.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-151">This class should be shared and reused throughout your client application, and does not need toobe created on a per operation basis.</span></span> 

<span data-ttu-id="8b6bd-152">tooconnect tooan Azure Redis Cache a vrátit instanci připojeného `ConnectionMultiplexer`, volání hello statické `Connect` metoda a předejte jí hello mezipaměti koncový bod a klíč.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-152">tooconnect tooan Azure Redis Cache and be returned an instance of a connected `ConnectionMultiplexer`, call hello static `Connect` method and pass in hello cache endpoint and key.</span></span> <span data-ttu-id="8b6bd-153">Použijte hello klíč vygenerovaný na portálu Azure jako parametr hesla hello hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-153">Use hello key generated from hello Azure portal as hello password parameter.</span></span>

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> <span data-ttu-id="8b6bd-154">Upozornění: Neuchovávejte přihlašovací údaje ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-154">Warning: Never store credentials in source code.</span></span> <span data-ttu-id="8b6bd-155">tookeep Tato ukázka jednoduché I mě zobrazující je v hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-155">tookeep this sample simple, I’m showing them in hello source code.</span></span> <span data-ttu-id="8b6bd-156">V tématu [fungování řetězců aplikace a připojovacích řetězců] [ How Application Strings and Connection Strings Work] informace o tom toostore přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-156">See [How Application Strings and Connection Strings Work][How Application Strings and Connection Strings Work] for information on how toostore credentials.</span></span>
> 
> 

<span data-ttu-id="8b6bd-157">Pokud nechcete, aby toouse SSL, nastavte `ssl=false` nebo vynechejte hello `ssl` parametr.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-157">If you don't want toouse SSL, either set `ssl=false` or omit hello `ssl` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="8b6bd-158">port bez SSL Hello je ve výchozím nastavení pro nové mezipaměti zakázán.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-158">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="8b6bd-159">Pokyny pro povolení portu bez SSL hello najdete v tématu [přístupové porty](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="8b6bd-159">For instructions on enabling hello non-SSL port, see [Access Ports](cache-configure.md#access-ports).</span></span>
> 
> 

<span data-ttu-id="8b6bd-160">Jeden ze způsobů toosharing `ConnectionMultiplexer` instance v aplikaci je toohave statické vlastnosti, která vrací připojenou instanci, podobně jako toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-160">One approach toosharing a `ConnectionMultiplexer` instance in your application is toohave a static property that returns a connected instance, similar toohello following example.</span></span> <span data-ttu-id="8b6bd-161">Tento přístup poskytuje způsob vláken tooinitialize pouze jedné připojené `ConnectionMultiplexer` instance.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-161">This approach provides a thread-safe way tooinitialize only a single connected `ConnectionMultiplexer` instance.</span></span> <span data-ttu-id="8b6bd-162">V těchto příkladech `abortConnect` je sada toofalse, což znamená, že i když nebude navázáno připojení toohello Azure Redis Cache je úspěšné volání hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-162">In these examples `abortConnect` is set toofalse, which means that hello call succeeds even if a connection toohello Azure Redis Cache is not established.</span></span> <span data-ttu-id="8b6bd-163">Klíčovou vlastností `ConnectionMultiplexer` je, že automaticky obnoví připojení k mezipaměti toohello po hello problém sítě nebo jiných příčin jsou vyřešeny.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-163">One key feature of `ConnectionMultiplexer` is that it automatically restores connectivity toohello cache once hello network issue or other causes are resolved.</span></span>

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

<span data-ttu-id="8b6bd-164">Další informace o rozšířených možnostech konfigurace připojení najdete v tématu [Konfigurační model StackExchange.Redis][StackExchange.Redis configuration model].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-164">For more information on advanced connection configuration options, see [StackExchange.Redis configuration model][StackExchange.Redis configuration model].</span></span>

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<span data-ttu-id="8b6bd-165">Po navázání připojení hello vrátit databázi mezipaměti redis toohello odkaz podle volání hello `ConnectionMultiplexer.GetDatabase` metoda.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-165">Once hello connection is established, return a reference toohello redis cache database by calling hello `ConnectionMultiplexer.GetDatabase` method.</span></span> <span data-ttu-id="8b6bd-166">Hello objekt byl vrácen ze hello `GetDatabase` metoda je prostý průchozí objekt a není nutné toobe uložené.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-166">hello object returned from hello `GetDatabase` method is a lightweight pass-through object and does not need toobe stored.</span></span>

    // Connection refers tooa property that returns a ConnectionMultiplexer
    // as shown in hello previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using hello cache object...
    // Simple put of integral data types into hello cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from hello cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

<span data-ttu-id="8b6bd-167">Azure Redis Cache úrovní mít konfigurovat počet databází (výchozí 16), které se dají použít toologically samostatné hello data v mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-167">Azure Redis caches have a configurable number of databases (default of 16) that can be used toologically separate hello data within a Redis cache.</span></span> <span data-ttu-id="8b6bd-168">Další informace najdete v tématu [Co jsou databáze Redis?](cache-faq.md#what-are-redis-databases) a [Výchozí konfigurace serveru Redis](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="8b6bd-168">For more information, see [What are Redis databases?](cache-faq.md#what-are-redis-databases) and [Default Redis server configuration](cache-configure.md#default-redis-server-configuration).</span></span>

<span data-ttu-id="8b6bd-169">Teď, když víte, jak instanci Azure Redis Cache tooan tooconnect a vrátit odkaz toohello mezipaměti databáze, podíváme se na práci s mezipamětí hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-169">Now that you know how tooconnect tooan Azure Redis Cache instance and return a reference toohello cache database, let's look at working with hello cache.</span></span>

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a><span data-ttu-id="8b6bd-170">Přidejte a načtení objektů z mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="8b6bd-170">Add and retrieve objects from hello cache</span></span>
<span data-ttu-id="8b6bd-171">Položky lze ukládat a načítat z mezipaměti pomocí hello `StringSet` a `StringGet` metody.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-171">Items can be stored in and retrieved from a cache by using hello `StringSet` and `StringGet` methods.</span></span>

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

<span data-ttu-id="8b6bd-172">Redis ukládá většinu dat jako řetězce Redis, ale tyto řetězce mohou obsahovat mnoho typů dat, včetně serializovaných binárních dat, který můžete použít při ukládání objektů .NET v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-172">Redis stores most data as Redis strings, but these strings can contain many types of data, including serialized binary data, which can be used when storing .NET objects in hello cache.</span></span>

<span data-ttu-id="8b6bd-173">Při volání metody `StringGet`, pokud existuje hello objektu, se vrátí, a pokud ne, `null` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-173">When calling `StringGet`, if hello object exists, it is returned, and if it does not, `null` is returned.</span></span> <span data-ttu-id="8b6bd-174">Pokud `null` se vrátí, můžete načíst hodnotu hello z hello požadovaného zdroje dat a uložte ho hello mezipaměti pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-174">If `null` is returned, you can retrieve hello value from hello desired data source and store it in hello cache for subsequent use.</span></span> <span data-ttu-id="8b6bd-175">Tento vzor používání se označuje jako hello doplňováním mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-175">This usage pattern is known as hello cache-aside pattern.</span></span>

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

<span data-ttu-id="8b6bd-176">Můžete také použít `RedisValue`, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-176">You can also use `RedisValue`, as shown in hello following example.</span></span> <span data-ttu-id="8b6bd-177">Typ `RedisValue` má implicitní operátory pro práci s celočíselnými datovými typy a může být užitečný v případě, že očekávanou hodnotou pro položku v mezipaměti je `null`.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-177">`RedisValue` has implicit operators for working with integral data types, and can be useful if `null` is an expected value for a cached item.</span></span>


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


<span data-ttu-id="8b6bd-178">toospecify hello vypršení platnosti položky v mezipaměti hello, použijte hello `TimeSpan` parametr `StringSet`.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-178">toospecify hello expiration of an item in hello cache, use hello `TimeSpan` parameter of `StringSet`.</span></span>

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a><span data-ttu-id="8b6bd-179">Práce s objekty .NET v mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="8b6bd-179">Work with .NET objects in hello cache</span></span>
<span data-ttu-id="8b6bd-180">Azure Redis Cache může do mezipaměti ukládat objekty .NET i primitivní datové typy. Objekty .NET je však nutné před uložením do mezipaměti serializovat.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-180">Azure Redis Cache can cache both .NET objects and primitive data types, but before a .NET object can be cached it must be serialized.</span></span> <span data-ttu-id="8b6bd-181">Tento objekt serializace rozhraní .NET je zodpovědností hello vývojář aplikace hello a poskytuje flexibilitu vývojáře hello v hello volbu hello serializátor.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-181">This .NET object serialization is hello responsibility of hello application developer, and gives hello developer flexibility in hello choice of hello serializer.</span></span>

<span data-ttu-id="8b6bd-182">Jeden jednoduchý způsob tooserialize objekty, je toouse hello `JsonConvert` metody serializace v [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) a serializovat tooand z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-182">One simple way tooserialize objects is toouse hello `JsonConvert` serialization methods in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) and serialize tooand from JSON.</span></span> <span data-ttu-id="8b6bd-183">Hello následující příklad ukazuje získání a nastavení pomocí `Employee` instance objektu.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-183">hello following example shows a get and set using an `Employee` object instance.</span></span>

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store toocache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a><span data-ttu-id="8b6bd-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b6bd-184">Next Steps</span></span>
<span data-ttu-id="8b6bd-185">Teď, když jste se naučili základy hello, postupujte podle těchto odkazů toolearn Další informace o Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-185">Now that you've learned hello basics, follow these links toolearn more about Azure Redis Cache.</span></span>

* <span data-ttu-id="8b6bd-186">Podívejte se na hello poskytovatele ASP.NET pro Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-186">Check out hello ASP.NET providers for Azure Redis Cache.</span></span>
  * [<span data-ttu-id="8b6bd-187">Zprostředkovatel stavu relace Azure Redis</span><span class="sxs-lookup"><span data-stu-id="8b6bd-187">Azure Redis Session State Provider</span></span>](cache-aspnet-session-state-provider.md)
  * [<span data-ttu-id="8b6bd-188">Poskytovatel výstupní mezipaměti ASP.NET služby Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8b6bd-188">Azure Redis Cache ASP.NET Output Cache Provider</span></span>](cache-aspnet-output-cache-provider.md)
* <span data-ttu-id="8b6bd-189">[Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , abyste mohli [monitorování](cache-how-to-monitor.md) hello stav svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-189">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span> <span data-ttu-id="8b6bd-190">Můžete zobrazit hello metriky ve hello portál Azure a může taky [stáhnout a revidovat](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) je pomocí hello nástrojů dle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-190">You can view hello metrics in hello Azure portal and you can also [download and review](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) them using hello tools of your choice.</span></span>
* <span data-ttu-id="8b6bd-191">Podívejte se na hello [dokumentaci ke klientu mezipaměti StackExchange.Redis][StackExchange.Redis cache client documentation].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-191">Check out hello [StackExchange.Redis cache client documentation][StackExchange.Redis cache client documentation].</span></span>
  * <span data-ttu-id="8b6bd-192">K Azure Redis Cache lze přistupovat z mnoha klientů Redis a programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-192">Azure Redis Cache can be accessed from many Redis clients and development languages.</span></span> <span data-ttu-id="8b6bd-193">Další informace najdete na stránce [http://redis.io/clients][http://redis.io/clients].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-193">For more information, see [http://redis.io/clients][http://redis.io/clients].</span></span>
* <span data-ttu-id="8b6bd-194">Azure Redis Cache lze rovněž použít se službami a nástroji třetích stran, jako jsou Redsmin a Redis Desktop Manager.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-194">Azure Redis Cache can also be used with third-party services and tools such as Redsmin and Redis Desktop Manager.</span></span>
  * <span data-ttu-id="8b6bd-195">Další informace o Redsmin najdete v tématu [jak tooretrieve připojení k Azure Redis řetězec a jeho použití s Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-195">For more information about Redsmin, see [How tooretrieve an Azure Redis connection string and use it with Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span></span>
  * <span data-ttu-id="8b6bd-196">[RedisDesktopManager](https://github.com/uglide/RedisDesktopManager) umožňuje získat přístup k datům v Azure Redis Cache a zkoumat je pomocí grafického uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8b6bd-196">Access and inspect your data in Azure Redis Cache with a GUI using [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).</span></span>
* <span data-ttu-id="8b6bd-197">V tématu hello [redis] [ redis] dokumentaci a přečtěte si o [datových typech redis] [ redis data types] a [15minutový Úvod datové typy tooRedis][a fifteen minute introduction tooRedis data types].</span><span class="sxs-lookup"><span data-stu-id="8b6bd-197">See hello [redis][redis] documentation and read about [redis data types][redis data types] and [a fifteen minute introduction tooRedis data types][a fifteen minute introduction tooRedis data types].</span></span>

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction tooAzure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project tooUse Azure Caching]: #prepare-vs
[Configure Your Application tooUse Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create hello cache]: #create-cache
[Configure hello cache]: #enable-caching
[Configure hello cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect toohello cache]: #connect-to-cache
[Add and retrieve objects from hello cache]: #add-object
[Specify hello expiration of an object in hello cache]: #specify-expiration
[Store ASP.NET session state in hello cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How tooretrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How tooConfigure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set hello Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in hello cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate tooAzure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups toomanage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction tooRedis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


