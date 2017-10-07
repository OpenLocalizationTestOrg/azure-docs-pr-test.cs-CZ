---
title: "aaaMigrate Služba mezipaměti spravovaných aplikací tooRedis - Azure | Microsoft Docs"
description: "Zjistěte, jak toomigrate spravované služby mezipaměti a mezipaměti v roli aplikace tooAzure Redis Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: sdanie
ms.openlocfilehash: bd81722820acf0d2637828fbb6100c723aafeba5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-managed-cache-service-tooazure-redis-cache"></a>Migrace z tooAzure spravované služby mezipaměti Redis Cache
Migrace vaší aplikace, které používají tooAzure služba spravovaných mezipaměti Azure Redis Cache lze provést s minimálními změnami tooyour aplikací, v závislosti na hello spravované služby mezipaměti funkce, které používá aplikaci ukládání do mezipaměti. Když hello rozhraní API jsou přesně hello stejné jsou podobné a většinu váš stávající kód, který používá služba spravovaných mezipaměti tooaccess mezipaměti lze opětovně použít s minimálními změnami. Toto téma ukazuje, jak toomake hello nezbytné konfigurace a aplikace se změní toomigrate toouse aplikace vaší spravovanou službu mezipaměti Azure Redis Cache a ukazuje, jak některých funkcí hello Azure Redis Cache lze použít tooimplement hello funkce Spravovat službu mezipaměti mezipaměti.

>[!NOTE]
>Spravované služby mezipaměti a mezipaměť hostovaná v instanci Role byly [vyřazeno](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30. listopadu 2016. Pokud máte všechna nasazení mezipaměť hostovaná v instanci Role, které chcete toomigrate tooAzure Redis Cache, můžete provést hello kroky v tomto článku.

## <a name="migration-steps"></a>Kroky migrace
Hello následující kroky jsou požadované toomigrate toouse aplikace spravovanou službu mezipaměti Azure Redis Cache.

* Mapování funkce tooAzure spravované služby mezipaměti Redis Cache
* Vyberte nabídku mezipaměti
* Vytvoření mezipaměti
* Konfigurace klientů mezipaměti hello
  * Odebrat hello konfigurace služby spravované mezipaměti
  * Konfigurace klienta mezipaměti pomocí hello balíček StackExchange.Redis NuGet
* Migrace kódu spravované služby mezipaměti
  * Připojit toohello mezipaměti pomocí hello ConnectionMultiplexer – třída
  * Primitivní datové typy přístupu v mezipaměti hello
  * Práce s objekty .NET v mezipaměti hello
* Migrace stavu relace ASP.NET a ukládání výstupu do mezipaměti tooAzure Redis Cache 

## <a name="map-managed-cache-service-features-tooazure-redis-cache"></a>Mapování funkce tooAzure spravované služby mezipaměti Redis Cache
Azure spravované Cache Service a Azure Redis Cache jsou podobné, ale některé z jejich funkcí implementovat různými způsoby. Tato část popisuje některé rozdíly hello a poskytuje pokyny k implementaci hello funkcí služby mezipaměti spravované ve službě Azure Redis Cache.

| Spravované funkce Cache Service | Podpora spravovaného Cache Service | Podpora Azure Redis Cache |
| --- | --- | --- |
| Pojmenované mezipaměti |Výchozí mezipaměti je nakonfigurován, a v hello lze nakonfigurovat Standard a Premium mezipaměti nabídek, až toonine další s názvem mezipaměti v případě potřeby. |Azure Redis Cache úrovní mít konfigurovat počet databází (výchozí 16), které se dají použít tooimplement, který ukládá do mezipaměti podobné funkce toonamed. Další informace najdete v tématu [Co jsou databáze Redis?](cache-faq.md#what-are-redis-databases) a [Výchozí konfigurace serveru Redis](cache-configure.md#default-redis-server-configuration). |
| Vysoká dostupnost |Poskytuje vysokou dostupnost pro položky v mezipaměti hello v mezipaměti nabídky hello Standard a Premium. Pokud jsou ztraceny z důvodu selhání tooa položky, jsou stále k dispozici záložní kopie hello položky v mezipaměti hello. Zapíše sekundární mezipaměti toohello probíhají synchronně. |Zajištění vysoké dostupnosti je k dispozici v hello Standard a Premium mezipaměti nabídky, které mají konfiguraci dva uzly primární/replika (pár primární/replika má každý horizontálního oddílu v mezipaměti Premium). Zápisy toohello repliky jsou vytvářeny asynchronně. Další informace najdete v tématu [cenách Azure Redis Cache](https://azure.microsoft.com/pricing/details/cache/). |
| Oznámení |Umožňuje asynchronní oznámení, klienti tooreceive když celou řadu operace mezipaměti, ke kterým došlo u pojmenované mezipaměti. |Klientské aplikace můžete použít protokol pub nebo sub Redis nebo [oznámení Keyspace](cache-configure.md#keyspace-notifications-advanced-settings) tooachieve podobné toonotifications funkce. |
| Místní mezipaměť |Ukládá kopie v mezipaměti objektů místně na klientovi hello velmi rychlý přístup. |Klientské aplikace potřebovat tooimplement tuto funkci pomocí slovníku nebo podobné datové struktury. |
| Zásady vyřazení |Žádná nebo hodnoty nejdelšího Nepoužití. Výchozí zásada Hello je hodnoty nejdelšího Nepoužití. |Azure Redis Cache podporuje následující zásady vyřazení hello: volatile lru, allkeys lru, náhodné volatile, allkeys náhodné, volatile ttl, noeviction. Výchozí zásada Hello je volatile hodnoty nejdelšího nepoužití. Další informace najdete v tématu [konfigurace serveru výchozí Redis](cache-configure.md#default-redis-server-configuration). |
| Zásady vypršení platnosti |interval vypršení platnosti výchozí hello je deset minut zásad vypršení platnosti výchozí Hello je absolutní. Klouzavé a nikdy zásady jsou také k dispozici. |Ve výchozím nastavení se položky v mezipaměti hello nevyprší, ale dá se vypršení platnosti na základě zápisu za použití mezipaměti sady přetížení. Další informace najdete v tématu [přidat a načtení objektů z mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache). |
| Oblasti a označování |Oblasti jsou podskupiny pro položky v mezipaměti. Oblasti také podporují hello poznámky položek v mezipaměti s další popisné řetězce názvem značky. Oblasti podpory hello možnost tooperform operace hledání na všech položek s příznakem v této oblasti. Všechny položky v rámci oblasti se nacházejí v rámci jednoho uzlu clusteru mezipaměti hello. |Mezipaměť Redis se skládá z jednoho uzlu (Pokud je povolená Redis clusteru) tak hello konceptu oblastech spravovaných Cache Service se nevztahuje. Při načítání klíče tak, aby popisné značky mohou být vloženy do hello názvy klíčů a později používá tooretrieve hello položky redis podporuje vyhledávání a operace zástupný znak. Příklad implementace označování řešení pomocí Redis, naleznete v části [implementace mezipaměť s Redisem označování](http://stackify.com/implementing-cache-tagging-redis/). |
| Serializace |Managed Cache podporuje NetDataContractSerializer, BinaryFormatter a hello použití vlastní serializátorů. Výchozí hodnota Hello je NetDataContractSerializer. |Je zodpovědností hello objekty .NET hello klienta aplikace tooserialize před uvedením do mezipaměti hello hello vybíráte hello serializátor si vývojář aplikace klienta toohello. Další informace a ukázkový kód, najdete v části [práce s objekty .NET v mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Emulátor mezipaměti |Managed Cache poskytuje emulátoru místní mezipaměti. |Azure Redis Cache nemá emulátoru, ale můžete [místní spuštění sestavení MSOpenTech hello redis server.exe](cache-faq.md#cache-emulator) tooprovide emulátoru prostředí. |

## <a name="choose-a-cache-offering"></a>Vyberte nabídku mezipaměti
Microsoft Azure Redis Cache je dostupná v hello následující úrovně:

* **Basic** – jeden uzel. Více velikostí až too53 GB.
* **Standard** – dva uzly Primární/Replika. Více velikostí až too53 GB. 99,9% SLA.
* **Premium** – dva uzly primární/replika s po až too10 horizontálních oddílů. Více velikosti od 6 GB too530 GB. Všechny funkce úrovně Standard a navíc podpora [clusteru Redis](cache-how-to-premium-clustering.md), [trvalosti Redis](cache-how-to-premium-persistence.md) a [služby Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Každá úroveň se liší z hlediska funkcí a cen. Hello funkce jsou popsané dál v této příručce a další informace o cenách najdete v tématu [podrobnosti o cenách mezipaměti](https://azure.microsoft.com/pricing/details/cache/).

Počáteční bod pro migraci je toopick hello velikost, která odpovídá hello velikost předchozí mezipaměti spravované služby mezipaměti a pak škálovat nahoru nebo dolů v závislosti na požadavcích hello vaší aplikace. Další pokyny k výběru správné nabídky Azure Redis Cache hello, najdete v části [jaké mezipaměť Redis nabídky a velikosti použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Vytvoření mezipaměti
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-hello-cache-clients"></a>Konfigurace klientů mezipaměti hello
Jakmile hello mezipaměti je vytvořen a nakonfigurován, hello dalším krokem je konfigurace spravované služby mezipaměti hello tooremove a přidejte hello přidat hello konfigurace Azure Redis Cache a reference, aby klienti mezipaměti mají přístup k mezipaměti hello.

* Odebrat hello konfigurace služby spravované mezipaměti
* Konfigurace klienta mezipaměti pomocí hello balíček StackExchange.Redis NuGet

### <a name="remove-hello-managed-cache-service-configuration"></a>Odebrat hello konfigurace služby spravované mezipaměti
Před hello je možné nakonfigurovat klientské aplikace pro Azure Redis Cache, existující konfigurace spravované služby mezipaměti hello a odkazy na sestavení musí být odstraněn odinstalací balíčku NuGet pro spravované mezipaměti Service hello.

toouninstall hello balíčku NuGet pro spravované mezipaměti Service, klikněte pravým tlačítkem na projekt klienta hello v **Průzkumníku řešení** a zvolte **spravovat balíčky NuGet**. Vyberte hello **nainstalované balíky** uzel a typ W**indowsAzure.Caching** do hello nainstalováno vyhledávání balíčky pole. Vyberte **Windows** **Azure Cache** (nebo **Windows** **ukládání do mezipaměti Azure** v závislosti na verzi hello balíčku NuGet hello), klikněte na tlačítko  **Odinstalace**a potom klikněte na **Zavřít**.

![Odinstalace balíčku NuGet Azure Managed Cache Service](./media/cache-migrate-to-redis/IC757666.jpg)

Odinstalaci balíčku NuGet pro spravované mezipaměti Service hello odebere hello Služba mezipaměti spravovaných sestavení a hello spravované služby mezipaměti položky v hello app.config nebo web.config klientské aplikace hello. Protože některé vlastní nastavení nemusí být odebrány při odinstalaci hello balíček NuGet, otevřete soubor web.config nebo app.config a ujistěte se, že hello následující prvky jsou zcela odebrána.

Ujistěte se, že hello `dataCacheClients` položka je odebrána z hello `configSections` elementu. Neodebírejte hello celý `configSections` element; právě odebrat hello `dataCacheClients` položku, pokud je k dispozici.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Ujistěte se, že hello `dataCacheClients` oddíl je odebrat. Hello `dataCacheClients` skupinový rámeček je podobné toohello následující ukázka.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--toouse hello in-role flavor of Azure Cache, set identifier toobe hello cache cluster role name -->
    <!--toouse hello Azure Managed Cache Service, set identifier toobe hello endpoint of hello cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section toospecify security settings for connecting tooyour cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Odebraný konfigurace hello spravované služby mezipaměti můžete nakonfigurovat hello mezipaměti klienta, jak je popsáno v následující části hello.

### <a name="configure-a-cache-client-using-hello-stackexchangeredis-nuget-package"></a>Konfigurace klienta mezipaměti pomocí hello balíček StackExchange.Redis NuGet
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrace kódu spravované služby mezipaměti
Hello rozhraní API pro klienta mezipaměti StackExchange.Redis hello je podobné toohello spravované mezipaměti služby. Tato část obsahuje přehled hello rozdíly.

### <a name="connect-toohello-cache-using-hello-connectionmultiplexer-class"></a>Připojit toohello mezipaměti pomocí hello ConnectionMultiplexer – třída
Ve spravované službě mezipaměti byly zpracovány mezipaměti toohello připojení pomocí hello `DataCacheFactory` a `DataCache` třídy. Tato připojení jsou ve službě Azure Redis Cache spravuje hello `ConnectionMultiplexer` třídy.

Přidejte následující hello pomocí příkazu toohello začátek všechny soubory, ze kterého mají být tooaccess hello mezipaměti.

```c#
using StackExchange.Redis
```

Pokud se tento obor názvů se nevyřeší, ujistěte se, že jste přidali hello balíčku StackExchange.Redis NuGet jak je popsáno v [konfigurace klientů mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

> [!NOTE]
> Všimněte si, že hello klient StackExchange.Redis vyžaduje rozhraní .NET Framework 4 nebo vyšší.
> 
> 

instanci Azure Redis Cache tooconnect tooan, statické hello volání `ConnectionMultiplexer.Connect` metoda a předejte jí koncový bod hello a klíč. Jeden ze způsobů toosharing `ConnectionMultiplexer` instance v aplikaci je toohave statické vlastnosti, která vrací připojenou instanci, podobně jako toohello následující ukázka. To poskytuje způsob vláken tooinitialize pouze jedné připojené `ConnectionMultiplexer` instance. V tomto příkladu `abortConnect` je sada toofalse, což znamená, že hello volání bude úspěšné i v případě, že nebude navázáno připojení toohello mezipaměti. Klíčovou vlastností `ConnectionMultiplexer` je, že automatické obnovení připojení toohello mezipaměti po vyřešení hello problém sítě nebo jiných příčin.

```c#
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
```

Hello koncový bod mezipaměti, klíče a portů můžete získat z hello **Redis Cache** okně instance mezipaměti. Další informace najdete v tématu [Redis Cache vlastnosti](cache-configure.md#properties).

Po navázání připojení hello vrátit databázi mezipaměti Redis toohello odkaz podle volání hello `ConnectionMultiplexer.GetDatabase` metoda. Hello objekt byl vrácen ze hello `GetDatabase` metoda je prostý průchozí objekt a není nutné toobe uložené.

```c#
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using hello cache object...
// Simple put of integral data types into hello cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from hello cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

Klient StackExchange.Redis Hello používá hello `RedisKey` a `RedisValue` typy pro přístup k informacím a ukládání položek v mezipaměti hello. Tyto typy namapovat na nejvíce primitivní typy jazyka, včetně řetězec a často nepoužívají se přímo. Redis řetězce jsou hello nejzákladnější druh hodnotu Redis a může obsahovat mnoho typů dat, včetně serializovaných binární datové proudy, a při hello typ nemusí používat přímo, budete používat metody, které obsahují `String` v názvu hello. Pro většinu primitivní datové typy slouží k ukládání a načítání položky z mezipaměti hello pomocí hello `StringSet` a `StringGet` metody, pokud jsou kolekce nebo jiné datové typy Redis ukládání do mezipaměti hello. 

`StringSet`a `StringGet` jsou velmi podobné toohello spravované mezipaměti služby `Put` a `Get` metody s jedním hlavní rozdíl, že před nastavování a získávání objekt .NET do mezipaměti hello musí serializovat nejdřív. 

Při volání metody `StringGet`, pokud objekt hello existuje, bude vrácen, a pokud ne, vrátí se hodnota null. V takovém případě můžete načíst hodnotu hello z hello požadovaného zdroje dat a uložte ho hello mezipaměti pro pozdější použití. To se označuje jako hello doplňováním mezipaměti.

toospecify hello vypršení platnosti položky v mezipaměti hello, použijte hello `TimeSpan` parametr `StringSet`.

```c#
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis Cache může pracovat s objekty .NET, jakož i primitivní datové typy, ale před objekt .NET můžete uložit do mezipaměti, musí být serializovány. Toto je zodpovědností hello vývojář aplikace hello. To dává flexibilní vývojáře hello v hello volbu hello serializátor. Další informace a ukázkový kód, najdete v části [práce s objekty .NET v mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-tooazure-redis-cache"></a>Migrace stavu relace ASP.NET a ukládání výstupu do mezipaměti tooAzure Redis Cache
Azure Redis Cache má zprostředkovatele pro stavu relace ASP.NET a stránky ukládání výstupu do mezipaměti. toomigrate aplikace, který používá hello spravované služby mezipaměti verze těchto poskytovatelů, nejprve odeberte hello stávající části ze souboru web.config a pak nakonfigurujte hello Azure Redis Cache verze zprostředkovatelů hello. Pokyny týkající se použití hello poskytovatelů Azure Redis Cache ASP.NET, najdete v části [poskytovatele stavu relace ASP.NET pro Azure Redis Cache](cache-aspnet-session-state-provider.md) a [poskytovatel výstupní mezipaměti technologie ASP.NET pro Azure Redis Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Další kroky
Prozkoumejte hello [dokumentace k Azure Redis Cache](https://azure.microsoft.com/documentation/services/cache/) pro kurzy, ukázky, videa a další.

