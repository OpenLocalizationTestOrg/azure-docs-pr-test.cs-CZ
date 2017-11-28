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
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="ac53d-103">Poskytovatel výstupní mezipaměti ASP.NET pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="ac53d-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="ac53d-104">Poskytovatel výstupní mezipaměti Redis Hello je mechanismus mimo proces úložiště pro data do výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ac53d-104">hello Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="ac53d-105">Tato data jsou speciálně pro úplné odpovědi protokolu HTTP (stránka ukládání výstupu do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="ac53d-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="ac53d-106">Zprostředkovatel Hello zapojení do hello nový výstupní mezipaměti poskytovatele rozšíření bod byla zavedena v technologii ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="ac53d-106">hello provider plugs into hello new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="ac53d-107">hello toouse Redis poskytovatel výstupní mezipaměti napřed nakonfigurovat mezipaměť a pak konfiguraci aplikace ASP.NET pomocí balíčku Redis výstupní mezipaměti zprostředkovatele NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="ac53d-107">toouse hello Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using hello Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="ac53d-108">Toto téma obsahuje pokyny ke konfiguraci vaší aplikace toouse hello poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="ac53d-108">This topic provides guidance on configuring your application toouse hello Redis Output Cache Provider.</span></span> <span data-ttu-id="ac53d-109">Další informace o vytváření a konfiguraci instanci služby Azure Redis Cache najdete v tématu [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="ac53d-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-hello-cache"></a><span data-ttu-id="ac53d-110">Ukládání výstupu stránek ASP.NET hello mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ac53d-110">Store ASP.NET page output in hello cache</span></span>
<span data-ttu-id="ac53d-111">tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku hello Redis Cache relace stavu NuGet, klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.</span><span class="sxs-lookup"><span data-stu-id="ac53d-111">tooconfigure a client application in Visual Studio using hello Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>

<span data-ttu-id="ac53d-112">Spuštění hello následující příkaz z hello `Package Manager Console` okno.</span><span class="sxs-lookup"><span data-stu-id="ac53d-112">Run hello following command from hello `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="ac53d-113">balíček Redis výstupní mezipaměti zprostředkovatele NuGet Hello má závislost na hello StackExchange.Redis.StrongName balíčku.</span><span class="sxs-lookup"><span data-stu-id="ac53d-113">hello Redis Output Cache Provider NuGet package has a dependency on hello StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="ac53d-114">Pokud hello StackExchange.Redis.StrongName balíček není přítomen v projektu, je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="ac53d-114">If hello StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="ac53d-115">Další informace o balíčku hello Redis NuGet poskytovatel výstupní mezipaměti najdete v tématu hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet stránky.</span><span class="sxs-lookup"><span data-stu-id="ac53d-115">For more information about hello Redis Output Cache Provider NuGet package, see hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="ac53d-116">Přidání toohello silným názvem StackExchange.Redis.StrongName balíčku je také hello StackExchange.Redis jiný silným názvem verze.</span><span class="sxs-lookup"><span data-stu-id="ac53d-116">In addition toohello strong-named StackExchange.Redis.StrongName package, there is also hello StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="ac53d-117">Pokud váš projekt používá hello jiný silným názvem StackExchange.Redis verze, že je nutné odinstalovat, v opačném případě můžete získat konflikty pojmenování ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="ac53d-117">If your project is using hello non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="ac53d-118">Další informace o těchto balíčcích najdete v tématu [.NET konfigurace klientů mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="ac53d-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="ac53d-119">Hello NuGet balíček stáhne a přidá hello vyžaduje sestavení odkazuje a přidá hello následující části do souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="ac53d-119">hello NuGet package downloads and adds hello required assembly references and adds hello following section into your web.config file.</span></span> <span data-ttu-id="ac53d-120">Tato část obsahuje hello požadovanou konfiguraci pro vaše ASP.NET aplikace toouse hello poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="ac53d-120">This section contains hello required configuration for your ASP.NET application toouse hello Redis Output Cache Provider.</span></span>

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

<span data-ttu-id="ac53d-121">Hello komentář, že část poskytuje příklad atributů hello a ukázkové nastavení pro každý atribut.</span><span class="sxs-lookup"><span data-stu-id="ac53d-121">hello commented section provides an example of hello attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="ac53d-122">Nakonfigurovat atributy hello hello hodnotami z portálu Microsoft Azure hello okně vaší mezipaměti a nakonfigurovat hello ostatní hodnoty podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ac53d-122">Configure hello attributes with hello values from your cache blade in hello Microsoft Azure portal, and configure hello other values as desired.</span></span> <span data-ttu-id="ac53d-123">Pokyny pro přístup k vlastnosti mezipaměti najdete v části [konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="ac53d-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="ac53d-124">**hostitele** – zadejte váš koncový bod mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ac53d-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="ac53d-125">**port** – použít port bez SSL nebo vaše port SSL, v závislosti na nastavení ssl hello.</span><span class="sxs-lookup"><span data-stu-id="ac53d-125">**port** – use either your non-SSL port or your SSL port, depending on hello ssl settings.</span></span>
* <span data-ttu-id="ac53d-126">**accessKey** – použít buď hello primární nebo sekundární klíč pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="ac53d-126">**accessKey** – use either hello primary or secondary key for your cache.</span></span>
* <span data-ttu-id="ac53d-127">**SSL** – hodnota true, pokud chcete, aby toosecure mezipaměti nebo klient komunikaci pomocí protokolu ssl; jinak hodnota false.</span><span class="sxs-lookup"><span data-stu-id="ac53d-127">**ssl** – true if you want toosecure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="ac53d-128">Být jisti toospecify hello správný port.</span><span class="sxs-lookup"><span data-stu-id="ac53d-128">Be sure toospecify hello correct port.</span></span>
  * <span data-ttu-id="ac53d-129">port bez SSL Hello je ve výchozím nastavení pro nové mezipaměti zakázán.</span><span class="sxs-lookup"><span data-stu-id="ac53d-129">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="ac53d-130">Zadejte hodnotu PRAVDA pro toto nastavení toouse hello SSL port.</span><span class="sxs-lookup"><span data-stu-id="ac53d-130">Specify true for this setting toouse hello SSL port.</span></span> <span data-ttu-id="ac53d-131">Další informace o povolení portu bez SSL hello najdete v tématu hello [přístupové porty](cache-configure.md#access-ports) část v hello [konfigurace mezipaměti](cache-configure.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ac53d-131">For more information about enabling hello non-SSL port, see hello [Access Ports](cache-configure.md#access-ports) section in hello [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="ac53d-132">**hodnotu databaseId** – zadaná, které toouse databáze pro mezipaměť výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac53d-132">**databaseId** – Specified which database toouse for cache output data.</span></span> <span data-ttu-id="ac53d-133">Pokud není zadáno, je použita výchozí hodnota hello 0.</span><span class="sxs-lookup"><span data-stu-id="ac53d-133">If not specified, hello default value of 0 is used.</span></span>
* <span data-ttu-id="ac53d-134">**applicationName** – klíče jsou uložené v redis jako `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="ac53d-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="ac53d-135">Toto schéma pojmenování umožňuje více hello tooshare aplikace stejný klíč.</span><span class="sxs-lookup"><span data-stu-id="ac53d-135">This naming scheme enables multiple applications tooshare hello same key.</span></span> <span data-ttu-id="ac53d-136">Tento parametr je volitelný a pokud nezadáte, je použita výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="ac53d-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="ac53d-137">**connectionTimeoutInMilliseconds** – toto nastavení umožňuje connectTimeout hello toooverride nastavení v klienta StackExchange.Redis hello.</span><span class="sxs-lookup"><span data-stu-id="ac53d-137">**connectionTimeoutInMilliseconds** – This setting allows you toooverride hello connectTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="ac53d-138">Pokud není zadaný, se používá hello výchozí nastavení connectTimeout 5000.</span><span class="sxs-lookup"><span data-stu-id="ac53d-138">If not specified, hello default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="ac53d-139">Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="ac53d-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="ac53d-140">**operationTimeoutInMilliseconds** – toto nastavení umožňuje syncTimeout hello toooverride nastavení v klienta StackExchange.Redis hello.</span><span class="sxs-lookup"><span data-stu-id="ac53d-140">**operationTimeoutInMilliseconds** – This setting allows you toooverride hello syncTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="ac53d-141">Pokud není zadaný, se používá hello výchozí nastavení syncTimeout 1000.</span><span class="sxs-lookup"><span data-stu-id="ac53d-141">If not specified, hello default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="ac53d-142">Další informace najdete v tématu [konfigurační model StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="ac53d-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="ac53d-143">Přidáte stránku OutputCache direktivy tooeach, pro kterou chcete toocache hello výstup.</span><span class="sxs-lookup"><span data-stu-id="ac53d-143">Add an OutputCache directive tooeach page for which you wish toocache hello output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="ac53d-144">V předchozím příkladu hello hello do stránky data zůstanou v mezipaměti hello mezipaměti po dobu 60 sekund, a jinou verzi stránku hello se uloží do mezipaměti pro každou kombinaci parametrů.</span><span class="sxs-lookup"><span data-stu-id="ac53d-144">In hello previous example, hello cached page data remains in hello cache for 60 seconds, and a different version of hello page is cached for each parameter combination.</span></span> <span data-ttu-id="ac53d-145">Další informace o direktivy OutputCache hello najdete v tématu [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="ac53d-145">For more information about hello OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="ac53d-146">Jakmile jsou tyto kroky provést, je vaše aplikace nakonfigurovaná toouse hello poskytovatel výstupní mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="ac53d-146">Once these steps are performed, your application is configured toouse hello Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac53d-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac53d-147">Next steps</span></span>
<span data-ttu-id="ac53d-148">Podívejte se na hello [poskytovatele stavu relace ASP.NET pro Azure Redis Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="ac53d-148">Check out hello [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

