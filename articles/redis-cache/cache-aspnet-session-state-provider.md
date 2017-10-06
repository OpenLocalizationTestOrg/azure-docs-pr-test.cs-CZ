---
title: aaaCache poskytovatele stavu relace ASP.NET | Microsoft Docs
description: "Další informace o stavu relace ASP.NET toostore pomocí Azure Redis Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: sdanie
ms.openlocfilehash: 9ea84cf67b9314b15dce696f596d399921194510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Zprostředkovatel stavu relací ASP.NET pro Azure Redis Cache
Azure Redis Cache poskytuje poskytovatele stavu relace, můžete použít toostore vaše stav relace v mezipaměti spíše než v paměti, nebo v systému SQL Server databáze. toouse hello ukládání do mezipaměti poskytovatele stavu relace, nejdřív nakonfigurovat mezipaměť a potom konfiguraci aplikace ASP.NET pro mezipaměť pomocí balíčku Redis Cache relace stavu NuGet hello.

Je často není praktické v tooavoid aplikace cloudu reálného ukládání určitou formu stavu pro uživatelské relace, ale některé přístupy vliv na výkon a škálovatelnost více než jiné. Pokud máte toostore stavu, hello nejlepším řešením je tookeep hello množství malých stavu a uloží jej v souborech cookie. Pokud se nejedná o to vhodné, hello další nejlepším řešením je stavu relace ASP.NET toouse u poskytovatele pro distribuované, v paměti mezipaměti. nejhorší řešení Hello z hlediska výkonu a škálovatelnosti je toouse poskytovatele stavu relace zálohování databáze. Toto téma obsahuje informace o použití hello poskytovatele stavu relace ASP.NET pro Azure Redis Cache. Informace o dalších možnostech stavu relace najdete v tématu [možnosti stavu relace ASP.NET](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-hello-cache"></a>Ukládání stavu relace ASP.NET hello mezipaměti
tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku hello Redis Cache relace stavu NuGet, klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.

Spuštění hello následující příkaz z hello `Package Manager Console` okno.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Pokud používáte funkci clusteringu z úroveň premium hello hello, musíte použít [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 nebo vyšší nebo výjimku, je vyvolána. Přesunutí too2.0.1 nebo vyšší je narušující změně; Další informace najdete v tématu [v2.0.0 nejnovější podrobností o změně](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). V době hello této aktualizace článku hello aktuální verzi tohoto balíčku je 2.2.3.
> 
> 

Hello Redis NuGet poskytovatele stavu relace balíčku má závislost na hello StackExchange.Redis.StrongName balíčku. Pokud hello StackExchange.Redis.StrongName balíček není přítomen v projektu, je nainstalovaná.

>[!NOTE]
>Přidání toohello silným názvem StackExchange.Redis.StrongName balíčku je také hello StackExchange.Redis jiný silným názvem verze. Pokud váš projekt používá hello jiný silným názvem StackExchange.Redis verze, že je nutné odinstalovat, v opačném případě můžete získat konflikty pojmenování ve vašem projektu. Další informace o těchto balíčcích najdete v tématu [.NET konfigurace klientů mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Hello NuGet balíček stáhne a přidá hello vyžaduje sestavení odkazuje a přidá hello následující části do souboru web.config. Tato část obsahuje hello požadovanou konfiguraci pro vaše ASP.NET aplikace toouse hello poskytovatele stavu relace Redis Cache.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

Hello komentář, že část poskytuje příklad atributů hello a ukázkové nastavení pro každý atribut.

Nakonfigurovat atributy hello hello hodnotami z portálu Microsoft Azure hello okně vaší mezipaměti a nakonfigurovat hello ostatní hodnoty podle potřeby. Pokyny pro přístup k vlastnosti mezipaměti najdete v části [konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).

* **hostitele** – zadejte váš koncový bod mezipaměti.
* **port** – použít port bez SSL nebo vaše port SSL, v závislosti na nastavení ssl hello.
* **accessKey** – použít buď hello primární nebo sekundární klíč pro mezipaměť.
* **SSL** – hodnota true, pokud chcete, aby toosecure mezipaměti nebo klient komunikaci pomocí protokolu ssl; jinak hodnota false. Být jisti toospecify hello správný port.
  * port bez SSL Hello je ve výchozím nastavení pro nové mezipaměti zakázán. Zadejte hodnotu PRAVDA pro toto nastavení toouse hello SSL port. Další informace o povolení portu bez SSL hello najdete v tématu hello [přístupové porty](cache-configure.md#access-ports) část v hello [konfigurace mezipaměti](cache-configure.md) tématu.
* **throwOnError** – hodnota true, pokud chcete toobe výjimka vyvolána, pokud je selhání, nebo hodnotu NEPRAVDA Pokud chcete hello operaci toofail bezobslužně. Selhání můžete zkontrolovat zaškrtnutím statickou vlastnost Microsoft.Web.Redis.RedisSessionStateProvider.LastException hello. Hello výchozí hodnotu true.
* **retryTimeoutInMilliseconds** – operací, které selžou jsou opakovat během tohoto intervalu, zadaný v milisekundách. dojde k Hello první opakování, po 20 milisekundách a pak realizován v každou sekundu do vypršení platnosti hello retryTimeoutInMilliseconds interval opakování. Okamžitě po této době je hello operaci opakovat poslední jednou. Pokud se operace hello stále nezdaří, je vyvolána výjimka hello zpět toohello volajícího, v závislosti na nastavení throwOnError hello. Hello výchozí hodnota je 0 znamená bez opakování.
* **hodnotu databaseId** – Určuje, které toouse databáze pro mezipaměť výstupní data. Pokud není zadáno, je použita výchozí hodnota hello 0.
* **applicationName** – klíče jsou uložené v redis jako `{<Application Name>_<Session ID>}_Data`. Toto schéma pojmenování umožňuje více aplikacemi tooshare hello stejnou instanci Redis. Tento parametr je volitelný a pokud nezadáte, je použita výchozí hodnota.
* **connectionTimeoutInMilliseconds** – toto nastavení umožňuje connectTimeout hello toooverride nastavení v klienta StackExchange.Redis hello. Pokud není zadaný, se používá hello výchozí nastavení connectTimeout 5000. Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – toto nastavení umožňuje syncTimeout hello toooverride nastavení v klienta StackExchange.Redis hello. Pokud není zadaný, se používá hello výchozí nastavení syncTimeout 1000. Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Další informace o těchto vlastnostech najdete v tématu hello původní blogovém příspěvku na [uvedení ASP.NET poskytovatele stavu relace pro Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Nezapomeňte toocomment out hello standardní InProc relace stavu zprostředkovatele oddíl v souboru web.config.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

Jakmile jsou tyto kroky provést, je vaše aplikace nakonfigurovaná toouse hello poskytovatele stavu relace Redis Cache. Použijete-li stav relace v aplikaci, je uložený v instanci služby Azure Redis Cache.

> [!IMPORTANT]
> Data uložená v mezipaměti hello musí být serializovatelný, na rozdíl od hello data, která mohou být uloženy ve hello výchozí poskytovatele stavu relace ASP.NET v paměti. Při hello poskytovatele stavu relace pro Redis se používá, se ujistěte, že jsou serializovatelný hello typy dat, které jsou uložené ve stavu relace.
> 
> 

## <a name="aspnet-session-state-options"></a>Možnosti stavu relace ASP.NET
* V zprostředkovatel stavu relace paměti – tohoto zprostředkovatele ukládá stav relace hello v paměti. Hello výhodou používání tohoto zprostředkovatele je, že se že jedná o jednoduchým a rychlým. Pokud používáte ve zprostředkovateli paměti vzhledem k tomu, že není distribuovaná, ale nemůže škálování vašich webových aplikací.
* Zprostředkovatel stavu relace serveru SQL – tohoto zprostředkovatele ukládá stav relace hello v systému Sql Server. Tohoto zprostředkovatele použijte, pokud chcete stav relace hello toostore v trvalé úložiště. Je možné škálovat vaší webové aplikace, ale ve webové aplikaci pomocí Sql serveru pro relaci má dopad na výkon.
* Distribuované v paměti poskytovatele stavu relace například Redis Cache poskytovatele stavu relace – tohoto zprostředkovatele poskytuje hello nejlepší z obou světů. Webové aplikace může mít jednoduché, rychlé a škálovatelné poskytovatele stavu relace. Protože tato hello zprostředkovatele úložiště stavu relace v mezipaměti, aplikace má tootake v potaz všechny hello vlastnosti spojené při posuzování tooa distribuované v mezipaměti, jako je například selhání přechodný síťový. Osvědčené postupy týkající se použití mezipaměti najdete v části [ukládání do mezipaměti pokyny](../best-practices-caching.md) z Microsoft Patterns & postupy [Azure cloudové aplikace návrhu a implementace pokyny](https://github.com/mspnp/azure-guidance).

Další informace o stavu relace a ostatní osvědčené postupy najdete v tématu [webové vývoj osvědčených postupů (vytváření reálných cloudových aplikací s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Další kroky
Podívejte se na hello [poskytovatel výstupní mezipaměti technologie ASP.NET pro Azure Redis Cache](cache-aspnet-output-cache-provider.md).

