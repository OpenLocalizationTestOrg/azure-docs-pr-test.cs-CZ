---
title: "Ukázky aaaAzure Redis Cache | Microsoft Docs"
description: "Zjistěte, jak toouse Azure mezipaměti Redis"
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
ms.openlocfilehash: 5cf9287b577758b5d880d1ca3928c1bee643a8ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-redis-cache-samples"></a>Ukázek Azure Redis Cache
Toto téma obsahuje seznam ukázek Azure Redis Cache, který po sobě zakrývá scénáře, jako je připojení tooa mezipaměti, čtení a zápis dat tooand z mezipaměti a použitím zprostředkovatelů ASP.NET Redis Cache hello. Některé hello vzorků, které jsou ke stažení projekty a některé poskytují podrobné pokyny a součástí jsou fragmenty kódu ale nepropojovat tooa ke stažení projektu.

## <a name="hello-world-samples"></a>Hello world ukázky
Ukázky Hello v této části ukazují hello základní informace o připojení tooan instanci Azure Redis Cache a čtení a zápis dat toohello mezipaměti pomocí různých jazycích a Redis klientů.

Hello [Hello, world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázkové ukazuje, jak tooperform různé operace pomocí hello mezipaměti [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) klient .NET.

Tento příklad ukazuje postup:

* Používat různé možnosti připojení
* Čtení a zápis tooand objektů z mezipaměti hello pomocí synchronní a asynchronní operace
* Použijte Redis MGET/MSET příkazy tooreturn hodnoty zadané klíčů
* Provádění operací transakční Redis
* Práce s Redis seznamy a seřazené sady
* Ukládání objektů .NET pomocí JsonConvert serializátorů
* Použijte Redis nastaví tooimplement označování
* Práce s clusteru Redis

Další informace najdete v tématu hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentaci na githubu a další scénáře využití najdete v části hello [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) testování částí.

[Jak toouse Azure Redis mezipaměti s Pythonem](cache-python-get-started.md) ukazuje, jak tooget pracovat s Azure Redis Cache pomocí Pythonu a hello [redis-py](https://github.com/andymccurdy/redis-py) klienta.

[Práce s objekty .NET v mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) ukazuje je jedním ze způsobů tooserialize .NET objekty, zapíše je tooand číst z instance Azure Redis Cache. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Použití mezipaměti Redis jako propojovací rozhraní škálování pro ASP.NET SignalR
Hello [použijte Redis Cache jako horizontální navýšení kapacity propojovacího rozhraní pro ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) příklad ukazuje, jak můžete použít Azure Redis Cache jako propojovací rozhraní systému SignalR. Další informace o propojovací rozhraní systému najdete v tématu [škálování SignalR s Redisem](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Ukázkový dotaz zákazníka mezipaměti redis
Tento příklad znázorňuje výkon porovnává mezi přístup k datům z mezipaměti a přístup k datům z úložiště trvalosti. Tato ukázka má dva projekty.

* [Ukázka, jak lze Redis Cache vylepšit výkon ukládaní dat do mezipaměti](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Počáteční hodnoty hello databáze a mezipaměti pro ukázku hello](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Stavu relace ASP.NET a ukládání výstupu do mezipaměti
Hello [toostore použití Azure Redis Cache ASP.NET SessionState a OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) příklad ukazuje, jak můžete toouse Azure Redis Cache toostore relace ASP.NET a výstupní mezipaměti pomocí hello SessionState a OutputCache zprostředkovatele pro Redis.

## <a name="manage-azure-redis-cache-with-maml"></a>Spravovat MAML mezipaměť Redis systému Azure
Hello [spravovat Azure Redis Cache pomocí knihovny správy Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) příklad ukazuje, jak můžete využívat správu knihovny Azure toomanage - (Vytvořit / aktualizovat / delete) svojí mezipaměti. 

## <a name="custom-monitoring-sample"></a>Vlastní monitorování ukázka
Hello [data přístup Redis Cache monitorování](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) příklad ukazuje, jak můžete přistupovat k datům monitorování pro mezipaměť Azure Redis mimo hello portálu Azure.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Klon stylu služby Twitter vytvořené pomocí PHP a Redis
Hello [Retwis](https://github.com/SyntaxC4-MSFT/retwis) ukázka je hello Redis Hello World. Je minimální klon stylu služby Twitter sociální sítě vytvořené pomocí Redis a PHP pomocí hello [Predis](https://github.com/nrk/predis) klienta. Hello zdrojový kód je navrženou toobe velmi jednoduché a v hello stejný čas tooshow různých Redis datové struktury.

## <a name="bandwidth-monitor"></a>Monitorování šířky pásma
Hello [šířky pásma monitorování](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) ukázka vám umožní šířky pásma toomonitor hello používá na klientovi hello. toomeasure hello šířky pásma, hello ukázku spustit na hello mezipaměti klientského počítače, zkontrolujte volání toohello mezipaměti a sledovat šířku pásma hello hlášené ukázka monitorování hello šířky pásma.

