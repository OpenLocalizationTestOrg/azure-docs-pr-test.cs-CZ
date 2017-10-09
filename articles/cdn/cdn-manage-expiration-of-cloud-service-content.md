---
title: "aaaManage vypršení platnosti webového obsahu v Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toomanage vypršení platnosti obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a0154236c3893b627f4480e0688f555964862556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Spravovat konec platnosti obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN
> [!div class="op_single_selector"]
> * [Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Služba Azure blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

Soubory z jakékoli veřejně přístupné původu webového serveru můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).  Hello TTL je dáno hello [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v hello HTTP odpovědi ze serveru původu hello.  Tento článek popisuje, jak tooset `Cache-Control` hlavičky pro webové aplikace Azure, Azure Cloud Services, aplikace a Internetová informační služba lokalit, které jsou nakonfigurované podobně.

> [!TIP]
> Můžete se rozhodnout tooset žádné TTL na soubor.  V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.
> 
> Další informace o tom, jak funguje Azure CDN toospeed toofiles přístup a jiným prostředkům, najdete v části hello [přehled CDN Azure](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Nastavení hlavičky Cache-Control v konfiguraci
Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace hello změnou hello **applicationHost.config** nebo **web.config** soubory pro vaši webovou aplikaci.  Hello **system.webServer\staticContent\clientCache** element v souboru konfigurace hello nastaví hello `Cache-Control` hlavičky pro obsah. Pro **web.config**, nastavení konfigurace hello ovlivní všechno, co v hello složce a všem podsložkám, není-li přepsat na úrovni podsložky hello.  Například můžete nastavit výchozí čas to-live v hello kořenové toohave všechny statický obsah do mezipaměti pro 3 dny, ale mají do podsložky, která má další proměnná obsahu s nastavení mezipaměti na 6 hodin.  Pro **applicationHost.config**, bude mít vliv na všechny aplikace na webovém serveru hello, ale mohou být přepsána nastaveními v **web.config** soubory v aplikacích hello.

Hello následující kód XML znázorňuje příklad nastavení **clientCache** toospecify a maximální stáří 3 dny:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Určení **UseMaxAge** přidá `Cache-Control: max-age=<nnn>` toohello odpověď záhlaví podle hello hodnota zadaná v hello **CacheControlMaxAge** atribut. není ve formátu Hello hello časový interval pro hello **cacheControlMaxAge** atribut je <days>.<hours>:<min>:<sec>. Další informace o hello **clientCache** uzlu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Nastavení hlavičky Cache-Control v kódu
Pro aplikace ASP.NET, můžete nastavit chování ukládání do mezipaměti hello CDN prostřednictvím kódu programu pomocí nastavení hello **HttpResponse.Cache** vlastnost. Další informace o hello **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Pokud chcete obsah aplikace tooprogrammatically mezipaměti technologie ASP.NET, ujistěte se, že obsah hello je označen jako Uložitelný nastavením HttpCacheability příliš*veřejné*. Také ověřte, zda validátor mezipaměti. validátor Hello mezipaměti může být poslední změny nastavit časové razítko nastavit, že zavoláte SetLastModified nebo hodnotu etag voláním SetETag. V případě potřeby můžete také nastavit dobu platnosti mezipaměti voláním SetExpires, nebo můžete spolehnout na hello výchozí mezipaměti heuristiky popsané dříve v tomto dokumentu.  

Například toocache obsah na jednu hodinu, přidejte následující hello:  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Další kroky
* [Přečtěte si podrobnosti o hello **clientCache** – element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Přečtěte si dokumentaci hello k hello **HttpResponse.Cache** vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Přečtěte si dokumentaci hello k hello **HttpCachePolicy třída**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

