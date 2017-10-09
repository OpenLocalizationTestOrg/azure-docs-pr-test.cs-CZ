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
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="d7c49-103">Spravovat konec platnosti obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d7c49-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d7c49-104">Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS</span><span class="sxs-lookup"><span data-stu-id="d7c49-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="d7c49-105">Služba Azure blob Storage</span><span class="sxs-lookup"><span data-stu-id="d7c49-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="d7c49-106">Soubory z jakékoli veřejně přístupné původu webového serveru můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="d7c49-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="d7c49-107">Hello TTL je dáno hello [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v hello HTTP odpovědi ze serveru původu hello.</span><span class="sxs-lookup"><span data-stu-id="d7c49-107">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from hello origin server.</span></span>  <span data-ttu-id="d7c49-108">Tento článek popisuje, jak tooset `Cache-Control` hlavičky pro webové aplikace Azure, Azure Cloud Services, aplikace a Internetová informační služba lokalit, které jsou nakonfigurované podobně.</span><span class="sxs-lookup"><span data-stu-id="d7c49-108">This article describes how tooset `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="d7c49-109">Můžete se rozhodnout tooset žádné TTL na soubor.</span><span class="sxs-lookup"><span data-stu-id="d7c49-109">You may choose tooset no TTL on a file.</span></span>  <span data-ttu-id="d7c49-110">V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.</span><span class="sxs-lookup"><span data-stu-id="d7c49-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="d7c49-111">Další informace o tom, jak funguje Azure CDN toospeed toofiles přístup a jiným prostředkům, najdete v části hello [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d7c49-111">For more information about how Azure CDN works toospeed up access toofiles and other resources, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="d7c49-112">Nastavení hlavičky Cache-Control v konfiguraci</span><span class="sxs-lookup"><span data-stu-id="d7c49-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="d7c49-113">Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace hello změnou hello **applicationHost.config** nebo **web.config** soubory pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7c49-113">For static content, such as images and style sheets, you can control hello update frequency by modifying hello **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="d7c49-114">Hello **system.webServer\staticContent\clientCache** element v souboru konfigurace hello nastaví hello `Cache-Control` hlavičky pro obsah.</span><span class="sxs-lookup"><span data-stu-id="d7c49-114">hello **system.webServer\staticContent\clientCache** element in hello configuration file will set hello `Cache-Control` header for your content.</span></span> <span data-ttu-id="d7c49-115">Pro **web.config**, nastavení konfigurace hello ovlivní všechno, co v hello složce a všem podsložkám, není-li přepsat na úrovni podsložky hello.</span><span class="sxs-lookup"><span data-stu-id="d7c49-115">For **web.config**, hello configuration settings will affect everything in hello folder and all subfolders, unless overridden at hello subfolder level.</span></span>  <span data-ttu-id="d7c49-116">Například můžete nastavit výchozí čas to-live v hello kořenové toohave všechny statický obsah do mezipaměti pro 3 dny, ale mají do podsložky, která má další proměnná obsahu s nastavení mezipaměti na 6 hodin.</span><span class="sxs-lookup"><span data-stu-id="d7c49-116">For example, you can set a default time-to-live at hello root toohave all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="d7c49-117">Pro **applicationHost.config**, bude mít vliv na všechny aplikace na webovém serveru hello, ale mohou být přepsána nastaveními v **web.config** soubory v aplikacích hello.</span><span class="sxs-lookup"><span data-stu-id="d7c49-117">For **applicationHost.config**, all applications on hello site will be affected, but can be overridden in **web.config** files in hello applications.</span></span>

<span data-ttu-id="d7c49-118">Hello následující kód XML znázorňuje příklad nastavení **clientCache** toospecify a maximální stáří 3 dny:</span><span class="sxs-lookup"><span data-stu-id="d7c49-118">hello following XML shows an example of setting **clientCache** toospecify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="d7c49-119">Určení **UseMaxAge** přidá `Cache-Control: max-age=<nnn>` toohello odpověď záhlaví podle hello hodnota zadaná v hello **CacheControlMaxAge** atribut.</span><span class="sxs-lookup"><span data-stu-id="d7c49-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header toohello response based on hello value specified in hello **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="d7c49-120">není ve formátu Hello hello časový interval pro hello **cacheControlMaxAge** atribut je <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="d7c49-120">hello format of hello timespan is for hello **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="d7c49-121">Další informace o hello **clientCache** uzlu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="d7c49-121">For more information on hello **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="d7c49-122">Nastavení hlavičky Cache-Control v kódu</span><span class="sxs-lookup"><span data-stu-id="d7c49-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="d7c49-123">Pro aplikace ASP.NET, můžete nastavit chování ukládání do mezipaměti hello CDN prostřednictvím kódu programu pomocí nastavení hello **HttpResponse.Cache** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d7c49-123">For ASP.NET applications, you can set hello CDN caching behavior programmatically by setting hello **HttpResponse.Cache** property.</span></span> <span data-ttu-id="d7c49-124">Další informace o hello **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7c49-124">For more information on hello **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="d7c49-125">Pokud chcete obsah aplikace tooprogrammatically mezipaměti technologie ASP.NET, ujistěte se, že obsah hello je označen jako Uložitelný nastavením HttpCacheability příliš*veřejné*.</span><span class="sxs-lookup"><span data-stu-id="d7c49-125">If you want tooprogrammatically cache application content in ASP.NET, make sure that hello content is marked as cacheable by setting HttpCacheability too*Public*.</span></span> <span data-ttu-id="d7c49-126">Také ověřte, zda validátor mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d7c49-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="d7c49-127">validátor Hello mezipaměti může být poslední změny nastavit časové razítko nastavit, že zavoláte SetLastModified nebo hodnotu etag voláním SetETag.</span><span class="sxs-lookup"><span data-stu-id="d7c49-127">hello cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="d7c49-128">V případě potřeby můžete také nastavit dobu platnosti mezipaměti voláním SetExpires, nebo můžete spolehnout na hello výchozí mezipaměti heuristiky popsané dříve v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d7c49-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on hello default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="d7c49-129">Například toocache obsah na jednu hodinu, přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="d7c49-129">For example, toocache content for one hour, add hello following:</span></span>  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="d7c49-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7c49-130">Next Steps</span></span>
* [<span data-ttu-id="d7c49-131">Přečtěte si podrobnosti o hello **clientCache** – element</span><span class="sxs-lookup"><span data-stu-id="d7c49-131">Read details about hello **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="d7c49-132">Přečtěte si dokumentaci hello k hello **HttpResponse.Cache** vlastnost</span><span class="sxs-lookup"><span data-stu-id="d7c49-132">Read hello documentation for hello **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="d7c49-133">[Přečtěte si dokumentaci hello k hello **HttpCachePolicy třída**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7c49-133">[Read hello documentation for hello **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

