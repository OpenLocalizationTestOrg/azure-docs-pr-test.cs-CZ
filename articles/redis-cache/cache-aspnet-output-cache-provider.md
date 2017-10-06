---
title: "aaaCache poskytovatel výstupní mezipaměti technologie ASP.NET"
description: "Zjistěte, jak pomocí Azure Redis Cache výstupní toocache stránky ASP.NET"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: sdanie
ms.openlocfilehash: fc38cc657604b351f55ad8febac383783ac29700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Poskytovatel výstupní mezipaměti ASP.NET pro Azure Redis Cache
Poskytovatel výstupní mezipaměti Redis Hello je mechanismus mimo proces úložiště pro data do výstupní mezipaměti. Tato data jsou speciálně pro úplné odpovědi protokolu HTTP (stránka ukládání výstupu do mezipaměti). Zprostředkovatel Hello zapojení do hello nový výstupní mezipaměti poskytovatele rozšíření bod byla zavedena v technologii ASP.NET 4.

hello toouse Redis poskytovatel výstupní mezipaměti napřed nakonfigurovat mezipaměť a pak konfiguraci aplikace ASP.NET pomocí balíčku Redis výstupní mezipaměti zprostředkovatele NuGet hello. Toto téma obsahuje pokyny ke konfiguraci vaší aplikace toouse hello poskytovatel výstupní mezipaměti Redis. Další informace o vytváření a konfiguraci instanci služby Azure Redis Cache najdete v tématu [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-hello-cache"></a>Ukládání výstupu stránek ASP.NET hello mezipaměti
tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku hello Redis Cache relace stavu NuGet, klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.

Spuštění hello následující příkaz z hello `Package Manager Console` okno.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

balíček Redis výstupní mezipaměti zprostředkovatele NuGet Hello má závislost na hello StackExchange.Redis.StrongName balíčku. Pokud hello StackExchange.Redis.StrongName balíček není přítomen v projektu, je nainstalovaná. Další informace o balíčku hello Redis NuGet poskytovatel výstupní mezipaměti najdete v tématu hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet stránky.

>[!NOTE]
>Přidání toohello silným názvem StackExchange.Redis.StrongName balíčku je také hello StackExchange.Redis jiný silným názvem verze. Pokud váš projekt používá hello jiný silným názvem StackExchange.Redis verze, že je nutné odinstalovat, v opačném případě můžete získat konflikty pojmenování ve vašem projektu. Další informace o těchto balíčcích najdete v tématu [.NET konfigurace klientů mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Hello NuGet balíček stáhne a přidá hello vyžaduje sestavení odkazuje a přidá hello následující části do souboru web.config. Tato část obsahuje hello požadovanou konfiguraci pro vaše ASP.NET aplikace toouse hello poskytovatel výstupní mezipaměti Redis.

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

Hello komentář, že část poskytuje příklad atributů hello a ukázkové nastavení pro každý atribut.

Nakonfigurovat atributy hello hello hodnotami z portálu Microsoft Azure hello okně vaší mezipaměti a nakonfigurovat hello ostatní hodnoty podle potřeby. Pokyny pro přístup k vlastnosti mezipaměti najdete v části [konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).

* **hostitele** – zadejte váš koncový bod mezipaměti.
* **port** – použít port bez SSL nebo vaše port SSL, v závislosti na nastavení ssl hello.
* **accessKey** – použít buď hello primární nebo sekundární klíč pro mezipaměť.
* **SSL** – hodnota true, pokud chcete, aby toosecure mezipaměti nebo klient komunikaci pomocí protokolu ssl; jinak hodnota false. Být jisti toospecify hello správný port.
  * port bez SSL Hello je ve výchozím nastavení pro nové mezipaměti zakázán. Zadejte hodnotu PRAVDA pro toto nastavení toouse hello SSL port. Další informace o povolení portu bez SSL hello najdete v tématu hello [přístupové porty](cache-configure.md#access-ports) část v hello [konfigurace mezipaměti](cache-configure.md) tématu.
* **hodnotu databaseId** – zadaná, které toouse databáze pro mezipaměť výstupní data. Pokud není zadáno, je použita výchozí hodnota hello 0.
* **applicationName** – klíče jsou uložené v redis jako `<AppName>_<SessionId>_Data`. Toto schéma pojmenování umožňuje více hello tooshare aplikace stejný klíč. Tento parametr je volitelný a pokud nezadáte, je použita výchozí hodnota.
* **connectionTimeoutInMilliseconds** – toto nastavení umožňuje connectTimeout hello toooverride nastavení v klienta StackExchange.Redis hello. Pokud není zadaný, se používá hello výchozí nastavení connectTimeout 5000. Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – toto nastavení umožňuje syncTimeout hello toooverride nastavení v klienta StackExchange.Redis hello. Pokud není zadaný, se používá hello výchozí nastavení syncTimeout 1000. Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Přidáte stránku OutputCache direktivy tooeach, pro kterou chcete toocache hello výstup.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

V předchozím příkladu hello hello do stránky data zůstanou v mezipaměti hello mezipaměti po dobu 60 sekund, a jinou verzi stránku hello se uloží do mezipaměti pro každou kombinaci parametrů. Další informace o direktivy OutputCache hello najdete v tématu [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

Jakmile jsou tyto kroky provést, je vaše aplikace nakonfigurovaná toouse hello poskytovatel výstupní mezipaměti Redis.

## <a name="next-steps"></a>Další kroky
Podívejte se na hello [poskytovatele stavu relace ASP.NET pro Azure Redis Cache](cache-aspnet-session-state-provider.md).

