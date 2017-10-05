---
title: "Mezipaměť poskytovatel výstupní mezipaměti ASP.NET"
description: "Naučte se mezipaměť výstupu stránky ASP.NET pomocí Azure Redis Cache"
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
ms.openlocfilehash: 845f25637a0e48460fc76c1ee36060274b3cec38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="85d49-103">Poskytovatel výstupní mezipaměti ASP.NET pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="85d49-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="85d49-104">Poskytovatel výstupní mezipaměti Redis je mechanismus mimo proces úložiště pro data do výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="85d49-104">The Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="85d49-105">Tato data jsou speciálně pro úplné odpovědi protokolu HTTP (stránka ukládání výstupu do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="85d49-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="85d49-106">Zprostředkovatel připojuje do nový výstupní mezipaměti poskytovatele rozšíření bod byla zavedena v technologii ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="85d49-106">The provider plugs into the new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="85d49-107">Pokud chcete používat poskytovatel výstupní mezipaměti Redis, nejdřív nakonfigurovat mezipaměť a pak nakonfigurujte vaši aplikaci ASP.NET pomocí balíčku Redis NuGet poskytovatel výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="85d49-107">To use the Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using the Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="85d49-108">Toto téma obsahuje pokyny týkající se konfigurace aplikace k používání poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-108">This topic provides guidance on configuring your application to use the Redis Output Cache Provider.</span></span> <span data-ttu-id="85d49-109">Další informace o vytváření a konfiguraci instanci služby Azure Redis Cache najdete v tématu [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="85d49-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-the-cache"></a><span data-ttu-id="85d49-110">Ukládání výstupu stránek ASP.NET do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="85d49-110">Store ASP.NET page output in the cache</span></span>
<span data-ttu-id="85d49-111">Chcete-li konfigurovat klientskou aplikaci v sadě Visual Studio pomocí balíčku Redis Cache relace stavu NuGet, klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z **nástroje** nabídky.</span><span class="sxs-lookup"><span data-stu-id="85d49-111">To configure a client application in Visual Studio using the Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu.</span></span>

<span data-ttu-id="85d49-112">V okně `Package Manager Console` spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="85d49-112">Run the following command from the `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="85d49-113">Balíček Redis NuGet poskytovatel výstupní mezipaměti má závislost na StackExchange.Redis.StrongName balíčku.</span><span class="sxs-lookup"><span data-stu-id="85d49-113">The Redis Output Cache Provider NuGet package has a dependency on the StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="85d49-114">Pokud balíček StackExchange.Redis.StrongName se nenachází ve vašem projektu, je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="85d49-114">If the StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="85d49-115">Další informace o balíčku pro Redis NuGet poskytovatel výstupní mezipaměti najdete v tématu [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet stránky.</span><span class="sxs-lookup"><span data-stu-id="85d49-115">For more information about the Redis Output Cache Provider NuGet package, see the [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="85d49-116">Kromě StackExchange.Redis.StrongName balíčku silným názvem vzrůstá také jiný silným názvem verze StackExchange.Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-116">In addition to the strong-named StackExchange.Redis.StrongName package, there is also the StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="85d49-117">Pokud váš projekt používá StackExchange.Redis verze jiný silným názvem, že je nutné odinstalovat, v opačném případě můžete získat konflikty pojmenování ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="85d49-117">If your project is using the non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="85d49-118">Další informace o těchto balíčcích najdete v tématu [.NET konfigurace klientů mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="85d49-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="85d49-119">Balíček NuGet se stáhne a přidá požadované odkazy na sestavení a přidá do souboru web.config v následující části.</span><span class="sxs-lookup"><span data-stu-id="85d49-119">The NuGet package downloads and adds the required assembly references and adds the following section into your web.config file.</span></span> <span data-ttu-id="85d49-120">Tato část obsahuje požadovanou konfiguraci pro aplikace ASP.NET pomocí poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-120">This section contains the required configuration for your ASP.NET application to use the Redis Output Cache Provider.</span></span>

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

<span data-ttu-id="85d49-121">V komentáři části poskytuje příklad atributů a ukázkové nastavení pro každý atribut.</span><span class="sxs-lookup"><span data-stu-id="85d49-121">The commented section provides an example of the attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="85d49-122">Nakonfigurovat atributy s hodnotami v okně vaší mezipaměti na portálu Microsoft Azure a podle potřeby nakonfigurujte další hodnoty.</span><span class="sxs-lookup"><span data-stu-id="85d49-122">Configure the attributes with the values from your cache blade in the Microsoft Azure portal, and configure the other values as desired.</span></span> <span data-ttu-id="85d49-123">Pokyny pro přístup k vlastnosti mezipaměti najdete v části [konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="85d49-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="85d49-124">**hostitele** – zadejte váš koncový bod mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="85d49-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="85d49-125">**port** – použít port bez SSL nebo vaše port SSL, v závislosti na nastavení ssl.</span><span class="sxs-lookup"><span data-stu-id="85d49-125">**port** – use either your non-SSL port or your SSL port, depending on the ssl settings.</span></span>
* <span data-ttu-id="85d49-126">**accessKey** – použít primární nebo sekundární klíč pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="85d49-126">**accessKey** – use either the primary or secondary key for your cache.</span></span>
* <span data-ttu-id="85d49-127">**SSL** – hodnota true, pokud chcete zabezpečit komunikaci mezipaměti nebo klienta pomocí protokolu ssl; jinak hodnota false.</span><span class="sxs-lookup"><span data-stu-id="85d49-127">**ssl** – true if you want to secure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="85d49-128">Nezapomeňte zadat správný port.</span><span class="sxs-lookup"><span data-stu-id="85d49-128">Be sure to specify the correct port.</span></span>
  * <span data-ttu-id="85d49-129">Port bez SSL je ve výchozím nastavení pro nové mezipaměti zakázán.</span><span class="sxs-lookup"><span data-stu-id="85d49-129">The non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="85d49-130">Zadejte hodnotu PRAVDA pro toto nastavení pro použití portu SSL.</span><span class="sxs-lookup"><span data-stu-id="85d49-130">Specify true for this setting to use the SSL port.</span></span> <span data-ttu-id="85d49-131">Další informace o povolení portu bez SSL najdete v tématu [přístupové porty](cache-configure.md#access-ports) tématu [konfigurace mezipaměti](cache-configure.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="85d49-131">For more information about enabling the non-SSL port, see the [Access Ports](cache-configure.md#access-ports) section in the [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="85d49-132">**hodnotu databaseId** – zadanou databázi, kterou chcete použít pro mezipaměť výstupní data.</span><span class="sxs-lookup"><span data-stu-id="85d49-132">**databaseId** – Specified which database to use for cache output data.</span></span> <span data-ttu-id="85d49-133">Pokud není zadáno, je použita výchozí hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="85d49-133">If not specified, the default value of 0 is used.</span></span>
* <span data-ttu-id="85d49-134">**applicationName** – klíče jsou uložené v redis jako `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="85d49-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="85d49-135">Toto schéma pojmenování umožňuje více aplikacím sdílet stejný klíč.</span><span class="sxs-lookup"><span data-stu-id="85d49-135">This naming scheme enables multiple applications to share the same key.</span></span> <span data-ttu-id="85d49-136">Tento parametr je volitelný a pokud nezadáte, je použita výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="85d49-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="85d49-137">**connectionTimeoutInMilliseconds** – toto nastavení umožňuje potlačit connectTimeout nastavení klienta StackExchange.Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-137">**connectionTimeoutInMilliseconds** – This setting allows you to override the connectTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="85d49-138">Pokud není zadaný, použije se výchozí nastavení connectTimeout 5000.</span><span class="sxs-lookup"><span data-stu-id="85d49-138">If not specified, the default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="85d49-139">Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="85d49-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="85d49-140">**operationTimeoutInMilliseconds** – toto nastavení umožňuje potlačit syncTimeout nastavení klienta StackExchange.Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-140">**operationTimeoutInMilliseconds** – This setting allows you to override the syncTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="85d49-141">Pokud není zadaný, použije se výchozí nastavení syncTimeout 1000.</span><span class="sxs-lookup"><span data-stu-id="85d49-141">If not specified, the default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="85d49-142">Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="85d49-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="85d49-143">Přidáte direktivu OutputCache na každé stránce, pro kterou chcete výstupu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="85d49-143">Add an OutputCache directive to each page for which you wish to cache the output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="85d49-144">V předchozím příkladu data uložená v mezipaměti stránky zůstává v mezipaměti po dobu 60 sekund, a jinou verzi stránky se uloží do mezipaměti pro každou kombinaci parametrů.</span><span class="sxs-lookup"><span data-stu-id="85d49-144">In the previous example, the cached page data remains in the cache for 60 seconds, and a different version of the page is cached for each parameter combination.</span></span> <span data-ttu-id="85d49-145">Další informace o direktivu OutputCache najdete v tématu [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="85d49-145">For more information about the OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="85d49-146">Jakmile jsou tyto kroky provést, vaše aplikace nakonfigurována pro použití poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="85d49-146">Once these steps are performed, your application is configured to use the Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85d49-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85d49-147">Next steps</span></span>
<span data-ttu-id="85d49-148">Podívejte se [poskytovatele stavu relace ASP.NET pro Azure Redis Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="85d49-148">Check out the [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

