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
# <a name="how-toouse-azure-redis-cache"></a>Jak tooUse Azure mezipaměti Redis
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Tento průvodce vám ukáže, jak tooget spuštění pomocí **Azure Redis Cache**. Microsoft Azure Redis Cache je založená na populární open source hello Redis Cache. Nabízí přístup k tooa zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem. Mezipaměť vytvořená pomocí Azure Redis Cache je přístupná ze všech aplikací v rámci Microsoft Azure.

Microsoft Azure Redis Cache je dostupná v hello následující úrovně:

* **Basic** – jeden uzel. Více velikostí až too53 GB.
* **Standard** – dva uzly Primární/Replika. Více velikostí až too53 GB. 99,9% SLA.
* **Premium** – dva uzly primární/replika s po až too10 horizontálních oddílů. Více velikosti od 6 GB too530 GB. Všechny funkce úrovně Standard a navíc podpora [clusteru Redis](cache-how-to-premium-clustering.md), [trvalosti Redis](cache-how-to-premium-persistence.md) a [služby Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Každá úroveň se liší z hlediska funkcí a cen. Informace o cenách najdete na stránce [Podrobnosti o cenách Azure Redis Cache][Cache Pricing Details].

Tento průvodce vám ukáže, jak toouse hello [StackExchange.Redis] [ StackExchange.Redis] klienta pomocí C\# kódu. Hello pokryté scénáře zahrnují **vytváření a konfiguraci mezipaměti**, **konfiguraci klientů mezipaměti**, a **přidávání a odebírání objektů z mezipaměti hello**. Další informace o používání Azure Redis Cache najdete v části [Další kroky][Next Steps]. Podrobný kurz vytvoření webové aplikace s Redis Cache ASP.NET MVC, najdete v části [jak toocreate webové aplikace s Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Začínáme s Azure Redis Cache
Začít s Azure Redis Cache je jednoduché. tooget spustili, můžete zřídit a nakonfigurujete mezipaměť. Dále nakonfigurujete klienty mezipaměti hello tak bude mít přístup k mezipaměti hello. Po nakonfigurování klientů mezipaměti hello můžete začít pracovat s nimi.

* [Vytvoření mezipaměti hello][Create hello cache]
* [Konfigurace klientů mezipaměti hello][Configure hello cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Vytvoření mezipaměti
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a>tooaccess vaší mezipaměti po jeho vytvoření
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Další informace o konfiguraci mezipaměti najdete v tématu [jak tooconfigure Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a>Konfigurace klientů mezipaměti hello
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Po konfiguraci klientského projektu je nakonfigurován pro ukládání do mezipaměti, můžete hello technik popsaných v následující části pro práci s mezipamětí hello.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Práce s mezipamětí
Hello kroky v této části popisují, jak tooperform běžné úkoly s mezipamětí.

* [Připojit toohello mezipaměti][Connect toohello cache]
* [Přidejte a načtení objektů z mezipaměti hello][Add and retrieve objects from hello cache]
* [Práce s objekty .NET v mezipaměti hello](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a>Připojit toohello mezipaměti
tooprogrammatically práci s mezipamětí, potřebujete odkaz toohello mezipaměti. Přidejte následující toohello horní části souboru, ve kterém chcete toouse hello StackExchange.Redis klienta tooaccess Azure Redis Cache hello.

    using StackExchange.Redis;

> [!NOTE]
> Hello klient StackExchange.Redis vyžaduje rozhraní .NET Framework 4 nebo vyšší.
> 
> 

Hello toohello připojení Azure Redis Cache spravuje hello `ConnectionMultiplexer` třídy. Tato třída by měla být sdílena a opětovné použití v rámci klientské aplikace a není nutné toobe vytvořena na každou operaci zvlášť. 

tooconnect tooan Azure Redis Cache a vrátit instanci připojeného `ConnectionMultiplexer`, volání hello statické `Connect` metoda a předejte jí hello mezipaměti koncový bod a klíč. Použijte hello klíč vygenerovaný na portálu Azure jako parametr hesla hello hello.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Upozornění: Neuchovávejte přihlašovací údaje ve zdrojovém kódu. tookeep Tato ukázka jednoduché I mě zobrazující je v hello zdrojového kódu. V tématu [fungování řetězců aplikace a připojovacích řetězců] [ How Application Strings and Connection Strings Work] informace o tom toostore přihlašovací údaje.
> 
> 

Pokud nechcete, aby toouse SSL, nastavte `ssl=false` nebo vynechejte hello `ssl` parametr.

> [!NOTE]
> port bez SSL Hello je ve výchozím nastavení pro nové mezipaměti zakázán. Pokyny pro povolení portu bez SSL hello najdete v tématu [přístupové porty](cache-configure.md#access-ports).
> 
> 

Jeden ze způsobů toosharing `ConnectionMultiplexer` instance v aplikaci je toohave statické vlastnosti, která vrací připojenou instanci, podobně jako toohello následující ukázka. Tento přístup poskytuje způsob vláken tooinitialize pouze jedné připojené `ConnectionMultiplexer` instance. V těchto příkladech `abortConnect` je sada toofalse, což znamená, že i když nebude navázáno připojení toohello Azure Redis Cache je úspěšné volání hello. Klíčovou vlastností `ConnectionMultiplexer` je, že automaticky obnoví připojení k mezipaměti toohello po hello problém sítě nebo jiných příčin jsou vyřešeny.

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

Další informace o rozšířených možnostech konfigurace připojení najdete v tématu [Konfigurační model StackExchange.Redis][StackExchange.Redis configuration model].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Po navázání připojení hello vrátit databázi mezipaměti redis toohello odkaz podle volání hello `ConnectionMultiplexer.GetDatabase` metoda. Hello objekt byl vrácen ze hello `GetDatabase` metoda je prostý průchozí objekt a není nutné toobe uložené.

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

Azure Redis Cache úrovní mít konfigurovat počet databází (výchozí 16), které se dají použít toologically samostatné hello data v mezipaměti Redis. Další informace najdete v tématu [Co jsou databáze Redis?](cache-faq.md#what-are-redis-databases) a [Výchozí konfigurace serveru Redis](cache-configure.md#default-redis-server-configuration).

Teď, když víte, jak instanci Azure Redis Cache tooan tooconnect a vrátit odkaz toohello mezipaměti databáze, podíváme se na práci s mezipamětí hello.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a>Přidejte a načtení objektů z mezipaměti hello
Položky lze ukládat a načítat z mezipaměti pomocí hello `StringSet` a `StringGet` metody.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis ukládá většinu dat jako řetězce Redis, ale tyto řetězce mohou obsahovat mnoho typů dat, včetně serializovaných binárních dat, který můžete použít při ukládání objektů .NET v mezipaměti hello.

Při volání metody `StringGet`, pokud existuje hello objektu, se vrátí, a pokud ne, `null` je vrácen. Pokud `null` se vrátí, můžete načíst hodnotu hello z hello požadovaného zdroje dat a uložte ho hello mezipaměti pro pozdější použití. Tento vzor používání se označuje jako hello doplňováním mezipaměti.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Můžete také použít `RedisValue`, jak ukazuje následující příklad hello. Typ `RedisValue` má implicitní operátory pro práci s celočíselnými datovými typy a může být užitečný v případě, že očekávanou hodnotou pro položku v mezipaměti je `null`.


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


toospecify hello vypršení platnosti položky v mezipaměti hello, použijte hello `TimeSpan` parametr `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a>Práce s objekty .NET v mezipaměti hello
Azure Redis Cache může do mezipaměti ukládat objekty .NET i primitivní datové typy. Objekty .NET je však nutné před uložením do mezipaměti serializovat. Tento objekt serializace rozhraní .NET je zodpovědností hello vývojář aplikace hello a poskytuje flexibilitu vývojáře hello v hello volbu hello serializátor.

Jeden jednoduchý způsob tooserialize objekty, je toouse hello `JsonConvert` metody serializace v [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) a serializovat tooand z formátu JSON. Hello následující příklad ukazuje získání a nastavení pomocí `Employee` instance objektu.

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello, postupujte podle těchto odkazů toolearn Další informace o Azure Redis Cache.

* Podívejte se na hello poskytovatele ASP.NET pro Azure Redis Cache.
  * [Zprostředkovatel stavu relace Azure Redis](cache-aspnet-session-state-provider.md)
  * [Poskytovatel výstupní mezipaměti ASP.NET služby Azure Redis Cache](cache-aspnet-output-cache-provider.md)
* [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , abyste mohli [monitorování](cache-how-to-monitor.md) hello stav svojí mezipaměti. Můžete zobrazit hello metriky ve hello portál Azure a může taky [stáhnout a revidovat](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) je pomocí hello nástrojů dle vašeho výběru.
* Podívejte se na hello [dokumentaci ke klientu mezipaměti StackExchange.Redis][StackExchange.Redis cache client documentation].
  * K Azure Redis Cache lze přistupovat z mnoha klientů Redis a programovacích jazyků. Další informace najdete na stránce [http://redis.io/clients][http://redis.io/clients].
* Azure Redis Cache lze rovněž použít se službami a nástroji třetích stran, jako jsou Redsmin a Redis Desktop Manager.
  * Další informace o Redsmin najdete v tématu [jak tooretrieve připojení k Azure Redis řetězec a jeho použití s Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].
  * [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager) umožňuje získat přístup k datům v Azure Redis Cache a zkoumat je pomocí grafického uživatelského rozhraní.
* V tématu hello [redis] [ redis] dokumentaci a přečtěte si o [datových typech redis] [ redis data types] a [15minutový Úvod datové typy tooRedis][a fifteen minute introduction tooRedis data types].

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


