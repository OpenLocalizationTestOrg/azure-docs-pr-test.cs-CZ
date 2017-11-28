---
title: "Spravovat vypršení platnosti webového obsahu v Azure CDN | Microsoft Docs"
description: "Zjistěte, jak spravovat platnost obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN."
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
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="afb9a-103">Spravovat konec platnosti obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="afb9a-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="afb9a-104">Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS</span><span class="sxs-lookup"><span data-stu-id="afb9a-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="afb9a-105">Služba Azure blob Storage</span><span class="sxs-lookup"><span data-stu-id="afb9a-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="afb9a-106">Soubory z jakékoli veřejně přístupné původu webového serveru můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="afb9a-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="afb9a-107">Hodnota TTL je dáno [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP ze zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="afb9a-107">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from the origin server.</span></span>  <span data-ttu-id="afb9a-108">Tento článek popisuje, jak nastavit `Cache-Control` hlavičky pro webové aplikace Azure, Azure Cloud Services, aplikace a Internetová informační služba lokalit, které jsou nakonfigurované podobně.</span><span class="sxs-lookup"><span data-stu-id="afb9a-108">This article describes how to set `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="afb9a-109">Můžete nastavit žádné TTL na soubor.</span><span class="sxs-lookup"><span data-stu-id="afb9a-109">You may choose to set no TTL on a file.</span></span>  <span data-ttu-id="afb9a-110">V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.</span><span class="sxs-lookup"><span data-stu-id="afb9a-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="afb9a-111">Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k souborům a dalším prostředkům, najdete v článku [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="afb9a-111">For more information about how Azure CDN works to speed up access to files and other resources, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="afb9a-112">Nastavení hlavičky Cache-Control v konfiguraci</span><span class="sxs-lookup"><span data-stu-id="afb9a-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="afb9a-113">Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace úpravou **applicationHost.config** nebo **web.config** soubory pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="afb9a-113">For static content, such as images and style sheets, you can control the update frequency by modifying the **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="afb9a-114">**System.webServer\staticContent\clientCache** nastaví element v konfiguračním souboru `Cache-Control` hlavičky pro obsah.</span><span class="sxs-lookup"><span data-stu-id="afb9a-114">The **system.webServer\staticContent\clientCache** element in the configuration file will set the `Cache-Control` header for your content.</span></span> <span data-ttu-id="afb9a-115">Pro **web.config**, nastavení konfigurace ovlivní všechno, co ve složce a všechny podsložky, není-li přepsat na úrovni podsložky.</span><span class="sxs-lookup"><span data-stu-id="afb9a-115">For **web.config**, the configuration settings will affect everything in the folder and all subfolders, unless overridden at the subfolder level.</span></span>  <span data-ttu-id="afb9a-116">Můžete například nastavit výchozí čas to-live v kořenovém adresáři mít všechny statický obsah do mezipaměti pro 3 dny, ale mají do podsložky, která má více proměnné obsah s nastavení mezipaměti na 6 hodin.</span><span class="sxs-lookup"><span data-stu-id="afb9a-116">For example, you can set a default time-to-live at the root to have all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="afb9a-117">Pro **applicationHost.config**, bude mít vliv na všechny aplikace na webu, ale mohou být přepsána nastaveními v **web.config** soubory v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="afb9a-117">For **applicationHost.config**, all applications on the site will be affected, but can be overridden in **web.config** files in the applications.</span></span>

<span data-ttu-id="afb9a-118">Následující kód XML znázorňuje příklad nastavení **clientCache** zadat maximální stáří 3 dny:</span><span class="sxs-lookup"><span data-stu-id="afb9a-118">The following XML shows an example of setting **clientCache** to specify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="afb9a-119">Určení **UseMaxAge** přidá `Cache-Control: max-age=<nnn>` hlavičky odpovědi na základě hodnoty zadané v **CacheControlMaxAge** atribut.</span><span class="sxs-lookup"><span data-stu-id="afb9a-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header to the response based on the value specified in the **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="afb9a-120">Není ve formátu časový interval pro **cacheControlMaxAge** atribut je <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="afb9a-120">The format of the timespan is for the **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="afb9a-121">Další informace o **clientCache** uzlu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="afb9a-121">For more information on the **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="afb9a-122">Nastavení hlavičky Cache-Control v kódu</span><span class="sxs-lookup"><span data-stu-id="afb9a-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="afb9a-123">Pro aplikace ASP.NET, můžete nastavit CDN chování ukládání do mezipaměti prostřednictvím kódu programu nastavením **HttpResponse.Cache** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="afb9a-123">For ASP.NET applications, you can set the CDN caching behavior programmatically by setting the **HttpResponse.Cache** property.</span></span> <span data-ttu-id="afb9a-124">Další informace o **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="afb9a-124">For more information on the **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="afb9a-125">Pokud chcete prostřednictvím kódu programu obsah mezipaměti aplikace technologie ASP.NET, ujistěte se, že obsah je označen jako Uložitelný nastavením HttpCacheability na *veřejné*.</span><span class="sxs-lookup"><span data-stu-id="afb9a-125">If you want to programmatically cache application content in ASP.NET, make sure that the content is marked as cacheable by setting HttpCacheability to *Public*.</span></span> <span data-ttu-id="afb9a-126">Také ověřte, zda validátor mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="afb9a-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="afb9a-127">Validátor mezipaměti může být poslední změny nastavit časové razítko nastavit, že zavoláte SetLastModified nebo hodnotu etag voláním SetETag.</span><span class="sxs-lookup"><span data-stu-id="afb9a-127">The cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="afb9a-128">V případě potřeby můžete také nastavit dobu platnosti mezipaměti voláním SetExpires, nebo můžete spoléhat na výchozí mezipaměti heuristické metody popsané výše v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="afb9a-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on the default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="afb9a-129">Například do mezipaměti obsah pro jednu hodinu, přidejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="afb9a-129">For example, to cache content for one hour, add the following:</span></span>  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="afb9a-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="afb9a-130">Next Steps</span></span>
* [<span data-ttu-id="afb9a-131">Přečtěte si podrobnosti o **clientCache** – element</span><span class="sxs-lookup"><span data-stu-id="afb9a-131">Read details about the **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="afb9a-132">Přečtěte si dokumentaci k **HttpResponse.Cache** vlastnost</span><span class="sxs-lookup"><span data-stu-id="afb9a-132">Read the documentation for the **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="afb9a-133">[Přečtěte si dokumentaci k **HttpCachePolicy třída**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="afb9a-133">[Read the documentation for the **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

