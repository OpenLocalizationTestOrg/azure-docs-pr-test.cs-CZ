---
title: "Stav aaaSession s mezipamětí Azure Redis ve službě Azure App Service"
description: "Zjistěte, jak toouse hello Azure Cache Service toosupport ASP.NET relace stavu ukládání do mezipaměti."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
ms.openlocfilehash: f689b6754ea072aa195f822ab6482f4bf2748375
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a><span data-ttu-id="f2eaf-103">Stav relace s mezipamětí Azure Redis ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f2eaf-103">Session state with Azure Redis cache in Azure App Service</span></span>
<span data-ttu-id="f2eaf-104">Toto téma vysvětluje, jak toouse hello Azure Redis Cache Service pro stav relace.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-104">This topic explains how toouse hello Azure Redis Cache Service for session state.</span></span>

<span data-ttu-id="f2eaf-105">Pokud webová aplikace ASP.NET používá stav relace, budete potřebovat tooconfigure externího poskytovatele stavu relace (hello Redis Cache Service nebo poskytovatele stavu relace serveru SQL Server).</span><span class="sxs-lookup"><span data-stu-id="f2eaf-105">If your ASP.NET web app uses session state, you will need tooconfigure an external session state provider (either hello Redis Cache Service or a SQL Server session state provider).</span></span> <span data-ttu-id="f2eaf-106">Pokud je používání stavu relace a nepoužíváte externího poskytovatele, bude omezený tooone instanci vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-106">If you use session state, and don't use an external provider, you will be limited tooone instance of your web app.</span></span> <span data-ttu-id="f2eaf-107">Hello Redis Cache Service je tooenable nejrychleji a nejsnáze hello.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-107">hello Redis Cache Service is hello fastest and simplest tooenable.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="f2eaf-108"><a id="createcache"></a>Vytvoření hello mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f2eaf-108"><a id="createcache"></a>Create hello Cache</span></span>
<span data-ttu-id="f2eaf-109">Postupujte podle [těchto pokynů](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-109">Follow [these directions](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello cache.</span></span>

## <span data-ttu-id="f2eaf-110"><a id="configureproject"></a>Přidat hello RedisSessionStateProvider NuGet balíček tooyour webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f2eaf-110"><a id="configureproject"></a>Add hello RedisSessionStateProvider NuGet package tooyour web app</span></span>
<span data-ttu-id="f2eaf-111">Nainstalujte hello NuGet `RedisSessionStateProvider` balíčku.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-111">Install hello NuGet `RedisSessionStateProvider` package.</span></span>  <span data-ttu-id="f2eaf-112">Použití hello následující příkaz tooinstall z konzoly Správce balíčků hello (**nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**):</span><span class="sxs-lookup"><span data-stu-id="f2eaf-112">Use hello following command tooinstall from hello package manager console (**Tools** > **NuGet Package Manager** > **Package Manager Console**):</span></span>

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

<span data-ttu-id="f2eaf-113">tooinstall z **nástroje** > **Správce balíčků NuGet** > **Správa balíčků Nuget pro řešení**, vyhledejte `RedisSessionStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-113">tooinstall from **Tools** > **NuGet Package Manager** > **Manage NugGet Packages for Solution**, search for `RedisSessionStateProvider`.</span></span>

<span data-ttu-id="f2eaf-114">Další informace najdete v části hello [stránka NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) a [konfigurace klienta mezipaměti hello](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span><span class="sxs-lookup"><span data-stu-id="f2eaf-114">For more information see hello [NuGet RedisSessionStateProvider page](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) and [Configure hello cache client](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span></span>

## <span data-ttu-id="f2eaf-115"><a id="configurewebconfig"></a>Upravit soubor Web.Config hello</span><span class="sxs-lookup"><span data-stu-id="f2eaf-115"><a id="configurewebconfig"></a>Modify hello Web.Config File</span></span>
<span data-ttu-id="f2eaf-116">Kromě toho toomaking sestavení odkazuje na pro mezipaměť, přidává hello balíček NuGet položky zástupných procedur v hello *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-116">In addition toomaking assembly references for Cache, hello NuGet package adds stub entries in hello *web.config* file.</span></span> 

1. <span data-ttu-id="f2eaf-117">Otevřete hello *web.config* a najde hello hello **sessionState** elementu.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-117">Open hello *web.config* and find hello hello **sessionState** element.</span></span>
2. <span data-ttu-id="f2eaf-118">Zadejte hodnoty hello `host`, `accessKey`, `port` (hello SSL port by měl být 6380) a nastavte `SSL` příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-118">Enter hello values for `host`, `accessKey`, `port` (hello SSL port should be 6380), and set `SSL` too`true`.</span></span> <span data-ttu-id="f2eaf-119">Tyto hodnoty lze získat z hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715) okně instance mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-119">These values can be obtained from hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) blade for your cache instance.</span></span> <span data-ttu-id="f2eaf-120">Další informace najdete v tématu [připojit toohello mezipaměti](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span><span class="sxs-lookup"><span data-stu-id="f2eaf-120">For more information, see [Connect toohello cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span></span> <span data-ttu-id="f2eaf-121">Všimněte si, že je ve výchozím nastavení pro nové mezipaměti zakázán port bez SSL hello.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-121">Note that hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="f2eaf-122">Další informace o povolení portu bez SSL hello najdete v tématu hello [přístupové porty](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) část v hello [konfigurace mezipaměti ve službě Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-122">For more information about enabling hello non-SSL port, see hello [Access Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) section in hello [Configure a cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) topic.</span></span> <span data-ttu-id="f2eaf-123">Hello následující kód ukazuje změny toohello hello *web.config* souboru, konkrétně změny hello příliš*port*, *hostitele*, accessKey *, a *ssl*.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-123">hello following markup shows hello changes toohello *web.config* file, specifically hello changes too*port*, *host*, accessKey*, and *ssl*.</span></span>
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <span data-ttu-id="f2eaf-124"><a id="usesessionobject"></a>Hello použití objektu Session v kódu</span><span class="sxs-lookup"><span data-stu-id="f2eaf-124"><a id="usesessionobject"></a> Use hello Session Object in Code</span></span>
<span data-ttu-id="f2eaf-125">posledním krokem Hello je toobegin pomocí hello objektu Session v kódu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-125">hello final step is toobegin using hello Session object in your ASP.NET code.</span></span> <span data-ttu-id="f2eaf-126">Přidat objekty toosession stavu pomocí hello **Session.Add** metoda.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-126">You add objects toosession state by using hello **Session.Add** method.</span></span> <span data-ttu-id="f2eaf-127">Tato metoda používá položky toostore páry klíč hodnota v mezipaměti stavu relace hello.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-127">This method uses key-value pairs toostore items in hello session state cache.</span></span>

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

<span data-ttu-id="f2eaf-128">Hello následující kód tuto hodnotu načítá ze stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-128">hello following code retrieves this value from session state.</span></span>

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

<span data-ttu-id="f2eaf-129">Můžete taky hello Redis Cache toocache objekty ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-129">You can also use hello Redis Cache toocache objects in your web app.</span></span> <span data-ttu-id="f2eaf-130">Další informace naleznete v tématu [Filmová aplikace MVC se službou Azure Redis Cache během 15 minut](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span><span class="sxs-lookup"><span data-stu-id="f2eaf-130">For more info, see [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span></span>
<span data-ttu-id="f2eaf-131">Další informace o stavu relace ASP.NET toouse, najdete v části [přehled stavu relace ASP.NET][ASP.NET Session State Overview].</span><span class="sxs-lookup"><span data-stu-id="f2eaf-131">For more details about how toouse ASP.NET session state, see [ASP.NET Session State Overview][ASP.NET Session State Overview].</span></span>

> [!NOTE]
> <span data-ttu-id="f2eaf-132">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-132">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="f2eaf-133">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="f2eaf-133">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="f2eaf-134">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="f2eaf-134">What's changed</span></span>
* <span data-ttu-id="f2eaf-135">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="f2eaf-135">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
  
  <span data-ttu-id="f2eaf-136">Autor: *Rick Anderson[](https://twitter.com/RickAndMSFT)*</span><span class="sxs-lookup"><span data-stu-id="f2eaf-136">*By [Rick Anderson](https://twitter.com/RickAndMSFT)*</span></span>

[installed hello latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png

