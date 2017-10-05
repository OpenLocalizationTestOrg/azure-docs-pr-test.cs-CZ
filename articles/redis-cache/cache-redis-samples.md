---
title: "Ukázek Azure Redis Cache | Microsoft Docs"
description: "Další informace o použití Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: 7841fcf0b5f4dcb409abf8bfb804c2e03dad6d3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-redis-cache-samples"></a><span data-ttu-id="c3bc0-103">Ukázek Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c3bc0-103">Azure Redis Cache samples</span></span>
<span data-ttu-id="c3bc0-104">Toto téma obsahuje seznam ukázek Azure Redis Cache, pokrývajících scénáře, jako je připojení k mezipaměti, čtení a zápisu dat do a z mezipaměti a pomocí poskytovatelů ASP.NET Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-104">This topic provides a list of Azure Redis Cache samples, covering scenarios such as connecting to a cache, reading and writing data to and from a cache, and using the ASP.NET Redis Cache providers.</span></span> <span data-ttu-id="c3bc0-105">Některé z ukázky ke stažení projekty jsou a některé poskytují podrobné pokyny a součástí jsou fragmenty kódu ale nejsou připojeny k dispozici ke stažení projektu.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-105">Some of the samples are downloadable projects, and some provide step-by-step guidance and include code snippets but do not link to a downloadable project.</span></span>

## <a name="hello-world-samples"></a><span data-ttu-id="c3bc0-106">Hello world ukázky</span><span class="sxs-lookup"><span data-stu-id="c3bc0-106">Hello world samples</span></span>
<span data-ttu-id="c3bc0-107">V této části Ukázky klientů Redis a zobrazit základní informace o připojení k instanci služby Azure Redis Cache a čtení a zapisování dat do mezipaměti pomocí různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-107">The samples in this section show the basics of connecting to an Azure Redis Cache instance and reading and writing data to the cache using a variety of languages and Redis clients.</span></span>

<span data-ttu-id="c3bc0-108">[Hello, world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) příklad ukazuje, jak provádět různé operace mezipaměti pomocí [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) klient .NET.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-108">The [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample shows how to perform various cache operations using the [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET client.</span></span>

<span data-ttu-id="c3bc0-109">Tento příklad ukazuje postup:</span><span class="sxs-lookup"><span data-stu-id="c3bc0-109">This sample shows how to:</span></span>

* <span data-ttu-id="c3bc0-110">Používat různé možnosti připojení</span><span class="sxs-lookup"><span data-stu-id="c3bc0-110">Use various connection options</span></span>
* <span data-ttu-id="c3bc0-111">Číst a zapisovat objekty do a z mezipaměti pomocí synchronní a asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="c3bc0-111">Read and write objects to and from the cache using synchronous and asynchronous operations</span></span>
* <span data-ttu-id="c3bc0-112">Návratové hodnoty zadaného klíče pomocí příkazů Redis MGET/MSET</span><span class="sxs-lookup"><span data-stu-id="c3bc0-112">Use Redis MGET/MSET commands to return values of specified keys</span></span>
* <span data-ttu-id="c3bc0-113">Provádění operací transakční Redis</span><span class="sxs-lookup"><span data-stu-id="c3bc0-113">Perform Redis transactional operations</span></span>
* <span data-ttu-id="c3bc0-114">Práce s Redis seznamy a seřazené sady</span><span class="sxs-lookup"><span data-stu-id="c3bc0-114">Work with Redis lists and sorted sets</span></span>
* <span data-ttu-id="c3bc0-115">Ukládání objektů .NET pomocí JsonConvert serializátorů</span><span class="sxs-lookup"><span data-stu-id="c3bc0-115">Store .NET objects using JsonConvert serializers</span></span>
* <span data-ttu-id="c3bc0-116">Pomocí sad Redis k implementaci označování</span><span class="sxs-lookup"><span data-stu-id="c3bc0-116">Use Redis sets to implement tagging</span></span>
* <span data-ttu-id="c3bc0-117">Práce s clusteru Redis</span><span class="sxs-lookup"><span data-stu-id="c3bc0-117">Work with Redis Cluster</span></span>

<span data-ttu-id="c3bc0-118">Další informace najdete v tématu [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) najdete v dokumentaci na githubu a další scénáře použití [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) testování částí.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-118">For more information, see the [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) documentation on github, and for more usage scenarios see the [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) unit tests.</span></span>

<span data-ttu-id="c3bc0-119">[Postup použití Azure Redis Cache s Pythonem](cache-python-get-started.md) ukazuje, jak začít pracovat s Azure Redis Cache pomocí Pythonu a [redis-py](https://github.com/andymccurdy/redis-py) klienta.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-119">[How to use Azure Redis Cache with Python](cache-python-get-started.md) shows how to get started with Azure Redis Cache using Python and the [redis-py](https://github.com/andymccurdy/redis-py) client.</span></span>

<span data-ttu-id="c3bc0-120">[Práce s objekty .NET v mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) ukazuje jeden způsob, jak serializovat objekty .NET, abyste mohli zapíše je a přečtěte si je z instance Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-120">[Work with .NET objects in the cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) shows you one way to serialize .NET objects so you can write them to and read them from an Azure Redis Cache instance.</span></span> 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a><span data-ttu-id="c3bc0-121">Použití mezipaměti Redis jako propojovací rozhraní škálování pro ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c3bc0-121">Use Redis Cache as a Scale out Backplane for ASP.NET SignalR</span></span>
<span data-ttu-id="c3bc0-122">[Použijte Redis Cache jako horizontální navýšení kapacity propojovacího rozhraní pro ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) příklad ukazuje, jak můžete použít Azure Redis Cache jako propojovací rozhraní systému SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-122">The [Use Redis Cache as a Scale out Backplane for ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) sample demonstrates how you can use Azure Redis Cache as a SignalR backplane.</span></span> <span data-ttu-id="c3bc0-123">Další informace o propojovací rozhraní systému najdete v tématu [škálování SignalR s Redisem](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span><span class="sxs-lookup"><span data-stu-id="c3bc0-123">For more information about backplane, see [SignalR Scaleout with Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span></span>

## <a name="redis-cache-customer-query-sample"></a><span data-ttu-id="c3bc0-124">Ukázkový dotaz zákazníka mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="c3bc0-124">Redis Cache customer query sample</span></span>
<span data-ttu-id="c3bc0-125">Tento příklad znázorňuje výkon porovnává mezi přístup k datům z mezipaměti a přístup k datům z úložiště trvalosti.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-125">This sample demonstrates compares performance between accessing data from a cache and accessing data from persistence storage.</span></span> <span data-ttu-id="c3bc0-126">Tato ukázka má dva projekty.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-126">This sample has two projects.</span></span>

* [<span data-ttu-id="c3bc0-127">Ukázka, jak lze Redis Cache vylepšit výkon ukládaní dat do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="c3bc0-127">Demo how Redis Cache can improve performance by Caching data</span></span>](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [<span data-ttu-id="c3bc0-128">Počáteční hodnoty databáze a mezipaměti pro ukázku</span><span class="sxs-lookup"><span data-stu-id="c3bc0-128">Seed the Database and Cache for the demo</span></span>](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a><span data-ttu-id="c3bc0-129">Stavu relace ASP.NET a ukládání výstupu do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="c3bc0-129">ASP.NET Session State and Output Caching</span></span>
<span data-ttu-id="c3bc0-130">[Pomocí Azure Redis Cache k ukládání ASP.NET SessionState a OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) příklad ukazuje způsob použití Azure Redis Cache k ukládání relace ASP.NET a výstupní mezipaměti pomocí zprostředkovatele SessionState a OutputCache pro Redis.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-130">The [Use Azure Redis Cache to store ASP.NET SessionState and OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) sample demonstrates how you to use Azure Redis Cache to store ASP.NET Session and Output Cache using the SessionState and OutputCache providers for Redis.</span></span>

## <a name="manage-azure-redis-cache-with-maml"></a><span data-ttu-id="c3bc0-131">Spravovat MAML mezipaměť Redis systému Azure</span><span class="sxs-lookup"><span data-stu-id="c3bc0-131">Manage Azure Redis Cache with MAML</span></span>
<span data-ttu-id="c3bc0-132">[Spravovat Azure Redis Cache pomocí knihovny správy Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) ukázka znázorňuje, jak můžete pomocí správy knihovny Azure ke správě - (Vytvořit / aktualizovat / delete) vaší mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-132">The [Manage Azure Redis Cache using Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample demonstrates how can you use Azure Management Libraries to manage - (Create/ Update/ delete) your Cache.</span></span> 

## <a name="custom-monitoring-sample"></a><span data-ttu-id="c3bc0-133">Vlastní monitorování ukázka</span><span class="sxs-lookup"><span data-stu-id="c3bc0-133">Custom monitoring sample</span></span>
<span data-ttu-id="c3bc0-134">[Data přístup Redis Cache monitorování](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) příklad ukazuje, jak můžete přistupovat k datům monitorování pro mezipaměť Azure Redis mimo portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-134">The [Access Redis Cache Monitoring data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) sample demonstrates how you can access monitoring data for your Azure Redis Cache outside of the Azure Portal.</span></span>

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a><span data-ttu-id="c3bc0-135">Klon stylu služby Twitter vytvořené pomocí PHP a Redis</span><span class="sxs-lookup"><span data-stu-id="c3bc0-135">A Twitter-style clone written using PHP and Redis</span></span>
<span data-ttu-id="c3bc0-136">[Retwis](https://github.com/SyntaxC4-MSFT/retwis) ukázka je Redis Hello World.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-136">The [Retwis](https://github.com/SyntaxC4-MSFT/retwis) sample is the Redis Hello World.</span></span> <span data-ttu-id="c3bc0-137">Je minimální klon stylu služby Twitter sociální sítě vytvořené pomocí Redis a PHP, pomocí [Predis](https://github.com/nrk/predis) klienta.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-137">It is a minimal Twitter-style social network clone written using Redis and PHP using the [Predis](https://github.com/nrk/predis) client.</span></span> <span data-ttu-id="c3bc0-138">Zdrojový kód je určen velmi jednoduché a současně zobrazit různé Redis datové struktury.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-138">The source code is designed to be very simple and at the same time to show different Redis data structures.</span></span>

## <a name="bandwidth-monitor"></a><span data-ttu-id="c3bc0-139">Monitorování šířky pásma</span><span class="sxs-lookup"><span data-stu-id="c3bc0-139">Bandwidth monitor</span></span>
<span data-ttu-id="c3bc0-140">[Šířky pásma monitorování](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) ukázka umožňuje sledovat šířku pásma používá na klientovi.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-140">The [Bandwidth monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) sample allows you to monitor the bandwidth used on the client.</span></span> <span data-ttu-id="c3bc0-141">K měření šířky pásma, spuštění vzorového v klientském počítači mezipaměti, provádět volání do mezipaměti a sledovat šířku pásma hlášené ukázka monitorování šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="c3bc0-141">To measure the bandwidth, run the sample on the cache client machine, make calls to the cache, and observe the bandwidth reported by the bandwidth monitor sample.</span></span>

