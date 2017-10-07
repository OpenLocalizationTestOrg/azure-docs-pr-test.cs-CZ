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
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Stav relace s mezipamětí Azure Redis ve službě Azure App Service
Toto téma vysvětluje, jak toouse hello Azure Redis Cache Service pro stav relace.

Pokud webová aplikace ASP.NET používá stav relace, budete potřebovat tooconfigure externího poskytovatele stavu relace (hello Redis Cache Service nebo poskytovatele stavu relace serveru SQL Server). Pokud je používání stavu relace a nepoužíváte externího poskytovatele, bude omezený tooone instanci vaší webové aplikace. Hello Redis Cache Service je tooenable nejrychleji a nejsnáze hello.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="createcache"></a>Vytvoření hello mezipaměti
Postupujte podle [těchto pokynů](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello mezipaměti.

## <a id="configureproject"></a>Přidat hello RedisSessionStateProvider NuGet balíček tooyour webové aplikace
Nainstalujte hello NuGet `RedisSessionStateProvider` balíčku.  Použití hello následující příkaz tooinstall z konzoly Správce balíčků hello (**nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

tooinstall z **nástroje** > **Správce balíčků NuGet** > **Správa balíčků Nuget pro řešení**, vyhledejte `RedisSessionStateProvider`.

Další informace najdete v části hello [stránka NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) a [konfigurace klienta mezipaměti hello](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

## <a id="configurewebconfig"></a>Upravit soubor Web.Config hello
Kromě toho toomaking sestavení odkazuje na pro mezipaměť, přidává hello balíček NuGet položky zástupných procedur v hello *web.config* souboru. 

1. Otevřete hello *web.config* a najde hello hello **sessionState** elementu.
2. Zadejte hodnoty hello `host`, `accessKey`, `port` (hello SSL port by měl být 6380) a nastavte `SSL` příliš`true`. Tyto hodnoty lze získat z hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715) okně instance mezipaměti. Další informace najdete v tématu [připojit toohello mezipaměti](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Všimněte si, že je ve výchozím nastavení pro nové mezipaměti zakázán port bez SSL hello. Další informace o povolení portu bez SSL hello najdete v tématu hello [přístupové porty](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) část v hello [konfigurace mezipaměti ve službě Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) tématu. Hello následující kód ukazuje změny toohello hello *web.config* souboru, konkrétně změny hello příliš*port*, *hostitele*, accessKey *, a *ssl*.
   
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

## <a id="usesessionobject"></a>Hello použití objektu Session v kódu
posledním krokem Hello je toobegin pomocí hello objektu Session v kódu ASP.NET. Přidat objekty toosession stavu pomocí hello **Session.Add** metoda. Tato metoda používá položky toostore páry klíč hodnota v mezipaměti stavu relace hello.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Hello následující kód tuto hodnotu načítá ze stavu relace.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

Můžete taky hello Redis Cache toocache objekty ve vaší webové aplikaci. Další informace naleznete v tématu [Filmová aplikace MVC se službou Azure Redis Cache během 15 minut](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Další informace o stavu relace ASP.NET toouse, najdete v části [přehled stavu relace ASP.NET][ASP.NET Session State Overview].

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
  
  Autor: *Rick Anderson[](https://twitter.com/RickAndMSFT)*

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

