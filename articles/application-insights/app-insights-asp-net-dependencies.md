---
title: "Závislost sledování ve službě Azure Application Insights | Microsoft Docs"
description: "Analýza místního využití, dostupnosti a výkonu nebo webová aplikace Microsoft Azure s nástrojem Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: 6e0b67ba98af27017901608dde4401600eb9957f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a><span data-ttu-id="dfbbd-103">Nastavte Application Insights: sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="dfbbd-103">Set up Application Insights: Dependency tracking</span></span>
<span data-ttu-id="dfbbd-104">A *závislostí* je externí komponenta, která je volána aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-104">A *dependency* is an external component that is called by your app.</span></span> <span data-ttu-id="dfbbd-105">Obvykle se jedná o službu volat pomocí protokolu HTTP, nebo databázi nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-105">It's typically a service called using HTTP, or a database, or a file system.</span></span> <span data-ttu-id="dfbbd-106">[Application Insights](app-insights-overview.md) měří, jak dlouho aplikace čeká závislosti a jak často závislostí volání selže.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-106">[Application Insights](app-insights-overview.md) measures how long your application waits for dependencies and how often a dependency call fails.</span></span> <span data-ttu-id="dfbbd-107">Můžete prozkoumat konkrétní volání a propojovat je na požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-107">You can investigate specific calls, and relate them to requests and exceptions.</span></span>

![ukázkové grafy](./media/app-insights-asp-net-dependencies/10-intro.png)

<span data-ttu-id="dfbbd-109">Toto monitorování závislostí na více systémů pole sestavy aktuálně volání na tyto typy závislosti:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-109">The out-of-the-box dependency monitor currently reports calls to these  types of dependencies:</span></span>

* <span data-ttu-id="dfbbd-110">Server</span><span class="sxs-lookup"><span data-stu-id="dfbbd-110">Server</span></span>
  * <span data-ttu-id="dfbbd-111">Databáze SQL</span><span class="sxs-lookup"><span data-stu-id="dfbbd-111">SQL databases</span></span>
  * <span data-ttu-id="dfbbd-112">Technologie ASP.NET a služby WCF, které používají vazby založené na protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="dfbbd-112">ASP.NET web and WCF services that use HTTP-based bindings</span></span>
  * <span data-ttu-id="dfbbd-113">Místní nebo vzdálené volání protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="dfbbd-113">Local or remote HTTP calls</span></span>
  * <span data-ttu-id="dfbbd-114">Azure Cosmos DB, table, úložiště objektů blob a fronty</span><span class="sxs-lookup"><span data-stu-id="dfbbd-114">Azure Cosmos DB, table, blob storage, and queue</span></span>
* <span data-ttu-id="dfbbd-115">Webové stránky</span><span class="sxs-lookup"><span data-stu-id="dfbbd-115">Web pages</span></span>
  * <span data-ttu-id="dfbbd-116">Volání AJAX</span><span class="sxs-lookup"><span data-stu-id="dfbbd-116">AJAX calls</span></span>

<span data-ttu-id="dfbbd-117">Monitorování funguje pomocí [bajtů kód instrumentace](https://msdn.microsoft.com/library/z9z62c29.aspx) kolem vybrané metody.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-117">Monitoring works by using [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) around selected methods.</span></span> <span data-ttu-id="dfbbd-118">Nároky na výkon je minimální.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-118">Performance overhead is minimal.</span></span>

<span data-ttu-id="dfbbd-119">Můžete taky napsat vlastní volání sady SDK k monitorování Další závislosti, oba seznamy v kód klienta a serveru pomocí [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-119">You can also write your own SDK calls to monitor other dependencies, both in the client and server code, using the [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

## <a name="set-up-dependency-monitoring"></a><span data-ttu-id="dfbbd-120">Nastavení monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="dfbbd-120">Set up dependency monitoring</span></span>
<span data-ttu-id="dfbbd-121">Částečné závislostí informace jsou shromažďovány automaticky pomocí [Application Insights SDK](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-121">Partial dependency information is collected automatically by the [Application Insights SDK](app-insights-asp-net.md).</span></span> <span data-ttu-id="dfbbd-122">Chcete-li získat kompletní datový, nainstalujte příslušné agenta pro hostitelský server.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-122">To get complete data, install the appropriate agent for the host server.</span></span>

| <span data-ttu-id="dfbbd-123">Platforma</span><span class="sxs-lookup"><span data-stu-id="dfbbd-123">Platform</span></span> | <span data-ttu-id="dfbbd-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="dfbbd-124">Install</span></span> |
| --- | --- |
| <span data-ttu-id="dfbbd-125">Server služby IIS</span><span class="sxs-lookup"><span data-stu-id="dfbbd-125">IIS Server</span></span> |<span data-ttu-id="dfbbd-126">Buď [nainstalujte monitorování stavu na serveru](app-insights-monitor-performance-live-website-now.md) nebo [upgradu vaší aplikace rozhraní .NET Framework 4.6 nebo novější](http://go.microsoft.com/fwlink/?LinkId=528259) a nainstalujte [Application Insights SDK](app-insights-asp-net.md) ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-126">Either [install Status Monitor on your server](app-insights-monitor-performance-live-website-now.md) or [Upgrade your application to .NET framework 4.6 or later](http://go.microsoft.com/fwlink/?LinkId=528259) and install the [Application Insights SDK](app-insights-asp-net.md)  in your app.</span></span> |
| <span data-ttu-id="dfbbd-127">Webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="dfbbd-127">Azure Web App</span></span> |<span data-ttu-id="dfbbd-128">Ve webové aplikaci ovládacího panelu [otevřete okno Application Insights ve webové aplikaci ovládacího panelu](app-insights-azure-web-apps.md) a instalace zvolte, pokud se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-128">In your web app control panel, [open the Application Insights blade in your web app control panel](app-insights-azure-web-apps.md) and choose Install if prompted.</span></span> |
| <span data-ttu-id="dfbbd-129">Cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="dfbbd-129">Azure Cloud Service</span></span> |<span data-ttu-id="dfbbd-130">[Úloha spuštění použití](app-insights-cloudservices.md) nebo [nainstalovat rozhraní .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="dfbbd-130">[Use startup task](app-insights-cloudservices.md) or [Install .NET framework 4.6+](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span></span> |

## <a name="where-to-find-dependency-data"></a><span data-ttu-id="dfbbd-131">Kde najít data závislostí</span><span class="sxs-lookup"><span data-stu-id="dfbbd-131">Where to find dependency data</span></span>
* <span data-ttu-id="dfbbd-132">[Mapa aplikace](#application-map) vizualizuje závislosti mezi aplikací a sousedních součásti.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-132">[Application Map](#application-map) visualizes dependencies between your app and neighbouring components.</span></span>
* <span data-ttu-id="dfbbd-133">[Okna výkonu, prohlížeče a selhání](#performance-and-blades) zobrazit závislosti dat serveru.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-133">[Performance, browser, and failure blades](#performance-and-blades) show server dependency data.</span></span>
* <span data-ttu-id="dfbbd-134">[Okno prohlížečů](#ajax-calls) ukazuje volání AJAX z prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-134">[Browsers blade](#ajax-calls) shows AJAX calls from your users' browsers.</span></span>
* <span data-ttu-id="dfbbd-135">[Klikněte na položku přes pomalé nebo chybných požadavků](#diagnose-slow-requests) zkontrolujte jejich závislost volání.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-135">[Click through from slow or failed requests](#diagnose-slow-requests) to check their dependency calls.</span></span>
* <span data-ttu-id="dfbbd-136">[Analýza](#analytics) lze použít k dotazování na data závislostí.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-136">[Analytics](#analytics) can be used to query dependency data.</span></span>

## <a name="application-map"></a><span data-ttu-id="dfbbd-137">Mapa aplikace</span><span class="sxs-lookup"><span data-stu-id="dfbbd-137">Application Map</span></span>
<span data-ttu-id="dfbbd-138">Mapa aplikace funguje jako vizuální pomůcka zjišťování závislostí mezi součástmi aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-138">Application Map acts as a visual aid to discovering dependencies between the components of your application.</span></span> <span data-ttu-id="dfbbd-139">Automaticky se generují z telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-139">It is automatically generated from the telemetry from your app.</span></span> <span data-ttu-id="dfbbd-140">Tento příklad ukazuje volání AJAX z prohlížeče skriptů a volání REST z aplikace serveru dvě externích služeb.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-140">This example shows AJAX calls from the browser scripts and REST calls from the server app to two external services.</span></span>

![Mapa aplikace](./media/app-insights-asp-net-dependencies/08.png)

* <span data-ttu-id="dfbbd-142">**Přejděte z políčka** relevantní závislosti a jinými grafy.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-142">**Navigate from the boxes** to relevant dependency and other charts.</span></span>
* <span data-ttu-id="dfbbd-143">**Připnout mapy** k [řídicí panel](app-insights-dashboards.md), kde budou plně funkční.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-143">**Pin the map** to the [dashboard](app-insights-dashboards.md), where it will be fully functional.</span></span>

<span data-ttu-id="dfbbd-144">[Další informace](app-insights-app-map.md).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-144">[Learn more](app-insights-app-map.md).</span></span>

## <a name="performance-and-failure-blades"></a><span data-ttu-id="dfbbd-145">Výkon a selhání oken</span><span class="sxs-lookup"><span data-stu-id="dfbbd-145">Performance and failure blades</span></span>
<span data-ttu-id="dfbbd-146">Okno výkon zobrazuje dobu trvání závislosti volání aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-146">The performance blade shows the duration of dependency calls made by the server app.</span></span> <span data-ttu-id="dfbbd-147">Není souhrnné graf a tabulku oddělených volání.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-147">There's a summary chart and a table segmented by call.</span></span>

![Grafy závislostí okno výkonu.](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

<span data-ttu-id="dfbbd-149">Proklikejte se prostřednictvím souhrnné grafy nebo položky, které se mají hledat nezpracovaná výskyty těchto volání.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-149">Click through the summary charts or the table items to search raw occurrences of these calls.</span></span>

![Instance volání závislostí](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

<span data-ttu-id="dfbbd-151">**Počet selhání** se zobrazují na **selhání** okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-151">**Failure counts** are shown on the **Failures** blade.</span></span> <span data-ttu-id="dfbbd-152">Selhání je jakýkoli návratový kód, který není v rozsahu 200-399, nebo neznámý.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-152">A failure is any return code that is not in the range 200-399, or unknown.</span></span>

> [!NOTE]
> <span data-ttu-id="dfbbd-153">**100 % selhání?**</span><span class="sxs-lookup"><span data-stu-id="dfbbd-153">**100% failures?**</span></span> <span data-ttu-id="dfbbd-154">– Pravděpodobně to znamená, jsou jenom získávání dat částečné závislostí.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-154">- This probably indicates that you are only getting partial dependency data.</span></span> <span data-ttu-id="dfbbd-155">Budete muset [nastavení monitorování závislostí vhodné pro vaši platformu](#set-up-dependency-monitoring).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-155">You need to [set up dependency monitoring appropriate to your platform](#set-up-dependency-monitoring).</span></span>
>
>

## <a name="ajax-calls"></a><span data-ttu-id="dfbbd-156">Volání AJAX</span><span class="sxs-lookup"><span data-stu-id="dfbbd-156">AJAX Calls</span></span>
<span data-ttu-id="dfbbd-157">V okně prohlížeče zobrazuje míru doba trvání a selhání volání AJAX z [JavaScript na webových stránkách](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-157">The Browsers blade shows the duration and failure rate of AJAX calls from [JavaScript in your web pages](app-insights-javascript.md).</span></span> <span data-ttu-id="dfbbd-158">Zobrazí se jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-158">They are shown as Dependencies.</span></span>

## <span data-ttu-id="dfbbd-159"><a name="diagnosis"></a>Diagnostika pomalé požadavků</span><span class="sxs-lookup"><span data-stu-id="dfbbd-159"><a name="diagnosis"></a> Diagnose slow requests</span></span>
<span data-ttu-id="dfbbd-160">Každá událost požadavku je přidružen volání závislostí, výjimek a další události, které jsou sledovány, zatímco aplikace je zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-160">Each request event is associated with the dependency calls, exceptions and other events that are tracked while your app is processing the request.</span></span> <span data-ttu-id="dfbbd-161">Takže pokud chybně provedli některé požadavky, můžete zjistit ať to je z důvodu zpomalení odezvy ze závislost.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-161">So if some requests are performing badly, you can find out whether it's due to slow responses from a dependency.</span></span>

<span data-ttu-id="dfbbd-162">Projděme příklad této.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-162">Let's walk through an example of that.</span></span>

### <a name="tracing-from-requests-to-dependencies"></a><span data-ttu-id="dfbbd-163">Trasování z požadavky na závislosti</span><span class="sxs-lookup"><span data-stu-id="dfbbd-163">Tracing from requests to dependencies</span></span>
<span data-ttu-id="dfbbd-164">Otevřete okno výkon a podívejte se na mřížky požadavků:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-164">Open the Performance blade, and look at the grid of requests:</span></span>

![Seznam požadavků s průměry a počty](./media/app-insights-asp-net-dependencies/02-reqs.png)

<span data-ttu-id="dfbbd-166">Horní jeden trvá velmi dlouho.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-166">The top one is taking very long.</span></span> <span data-ttu-id="dfbbd-167">Podívejme se, pokud jsme můžete zjistit, kde čas strávený.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-167">Let's see if we can find out where the time is spent.</span></span>

<span data-ttu-id="dfbbd-168">Klikněte na daném řádku zobrazíte události jednotlivých žádostí:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-168">Click that row to see individual request events:</span></span>

![Seznam výskytů požadavku](./media/app-insights-asp-net-dependencies/03-instances.png)

<span data-ttu-id="dfbbd-170">Kliknutím na jakoukoli instanci dlouho běžící další zkontrolovat a posuňte se dolů a volání vzdálené závislosti související s tuto žádost:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-170">Click any long-running instance to inspect it further, and scroll down to the remote dependency calls related to this request:</span></span>

![Najít volání vzdálené závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-dependencies.png)

<span data-ttu-id="dfbbd-172">To vypadá většinu času údržby, které tento požadavek byl stráven v volání k místní službě.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-172">It looks like most of the time servicing this request was spent in a call to a local service.</span></span>

<span data-ttu-id="dfbbd-173">Vyberte tento řádek získat další informace:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-173">Select that row to get more information:</span></span>

![Klikněte na tlačítko prostřednictvím tohoto vzdáleného závislostí pro identifikaci, který](./media/app-insights-asp-net-dependencies/05-detail.png)

<span data-ttu-id="dfbbd-175">Vypadá to jde, kde je problém.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-175">Looks like this is where the problem is.</span></span> <span data-ttu-id="dfbbd-176">Jsme jste přesně vymezená problém, takže teď jsme právě měli zjistit, proč tento volání trvá tak dlouho.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-176">We've pinpointed the problem, so now we just need to find out why that call is taking so long.</span></span>

### <a name="request-timeline"></a><span data-ttu-id="dfbbd-177">Časová osa požadavku</span><span class="sxs-lookup"><span data-stu-id="dfbbd-177">Request timeline</span></span>
<span data-ttu-id="dfbbd-178">V případě různých je volání žádné závislosti, které je zvláště dlouhý.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-178">In a different case, there is no dependency call that is particularly long.</span></span> <span data-ttu-id="dfbbd-179">Ale přepnutím na zobrazení časové osy, uvidíme, kde došlo k zpoždění v našem interní zpracování:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-179">But by switching to the timeline view, we can see where the delay occurred in our internal processing:</span></span>

![Najít volání vzdálené závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-1.png)

<span data-ttu-id="dfbbd-181">Nejspíš big mezera po první závislost volat, proto jsme by měl vypadat v našem kódu, uvidíte, proč je.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-181">There seems to be a big gap after the first dependency call, so we should look at our code to see why that is.</span></span>

### <a name="profile-your-live-site"></a><span data-ttu-id="dfbbd-182">Profil živý web</span><span class="sxs-lookup"><span data-stu-id="dfbbd-182">Profile your live site</span></span>

<span data-ttu-id="dfbbd-183">Žádné představu, kde přejde čas?</span><span class="sxs-lookup"><span data-stu-id="dfbbd-183">No idea where the time goes?</span></span> <span data-ttu-id="dfbbd-184">[Application Insights profileru](app-insights-profiler.md) trasování HTTP volání živý web a ukazuje, které funkce ve vašem kódu trvalo nejdelší dobu.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-184">The [Application Insights profiler](app-insights-profiler.md) traces HTTP calls to your live site and shows you which functions in your code took the longest time.</span></span>

## <a name="failed-requests"></a><span data-ttu-id="dfbbd-185">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="dfbbd-185">Failed requests</span></span>
<span data-ttu-id="dfbbd-186">Neúspěšné požadavky může být také přidružen selhání volání závislosti.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-186">Failed requests might also be associated with failed calls to dependencies.</span></span> <span data-ttu-id="dfbbd-187">Jsme znovu, klikněte prostřednictvím sledovat problém.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-187">Again, we can click through to track down the problem.</span></span>

![Klikněte na graf neúspěšných požadavků](./media/app-insights-asp-net-dependencies/06-fail.png)

<span data-ttu-id="dfbbd-189">Proklikejte se k výskytu chybné žádosti a podívejte se na jeho přidružené události.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-189">Click through to an occurrence of a failed request, and look at its associated events.</span></span>

![Klikněte na typ požadavku, klikněte na instanci systému na získat do jiného zobrazení stejné instance, klikněte na něj získat podrobnosti o výjimce.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a><span data-ttu-id="dfbbd-191">Analýza</span><span class="sxs-lookup"><span data-stu-id="dfbbd-191">Analytics</span></span>
<span data-ttu-id="dfbbd-192">Můžete sledovat v závislosti [analýzy protokolů dotazu jazyka](https://docs.loganalytics.io/).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-192">You can track dependencies in the [Log Analytics query language](https://docs.loganalytics.io/).</span></span> <span data-ttu-id="dfbbd-193">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-193">Here are some examples.</span></span>

* <span data-ttu-id="dfbbd-194">Najděte žádné volání se nezdařilo závislost:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-194">Find any failed dependency calls:</span></span>

```

    dependencies | where success != "True" | take 10
```

* <span data-ttu-id="dfbbd-195">Najděte volání AJAX:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-195">Find AJAX calls:</span></span>

```

    dependencies | where client_Type == "Browser" | take 10
```

* <span data-ttu-id="dfbbd-196">Najděte závislostí volání přidružené k žádosti:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-196">Find dependency calls associated with requests:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* <span data-ttu-id="dfbbd-197">Volání AJAX najít přidružené zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-197">Find AJAX calls associated with page views:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a><span data-ttu-id="dfbbd-198">Vlastní závislost sledování</span><span class="sxs-lookup"><span data-stu-id="dfbbd-198">Custom dependency tracking</span></span>
<span data-ttu-id="dfbbd-199">Standardní modul sledování závislosti automaticky zjišťuje externí závislosti, jako jsou databáze a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-199">The standard dependency-tracking module automatically discovers external dependencies such as databases and REST APIs.</span></span> <span data-ttu-id="dfbbd-200">Ale můžete chtít některé další součásti považován stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-200">But you might want some additional components to be treated in the same way.</span></span>

<span data-ttu-id="dfbbd-201">Můžete napsat kód, který odesílá informace o závislostech, používající stejný [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) používané standardní moduly.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-201">You can write code that sends dependency information, using the same [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) that is used by the standard modules.</span></span>

<span data-ttu-id="dfbbd-202">Například pokud vytvoříte kódu se sestavením, které nebylo napsat sami, může čas všechna volání, a zjistěte, jaký příspěvek umožňuje na váš doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-202">For example, if you build your code with an assembly that you didn't write yourself, you could time all the calls to it, to find out what contribution it makes to your response times.</span></span> <span data-ttu-id="dfbbd-203">Pokud chcete, aby tato data zobrazí v grafech závislosti ve službě Application Insights, odeslat pomocí `TrackDependency`.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-203">To have this data displayed in the dependency charts in Application Insights, send it using `TrackDependency`.</span></span>

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

<span data-ttu-id="dfbbd-204">Pokud chcete vypnout modul sledování standardní závislostí, odeberte odkaz na DependencyTrackingTelemetryModule v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span><span class="sxs-lookup"><span data-stu-id="dfbbd-204">If you want to switch off the standard dependency tracking module, remove the reference to DependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="dfbbd-205">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dfbbd-205">Troubleshooting</span></span>
<span data-ttu-id="dfbbd-206">*Závislost úspěch příznak vždycky zobrazí hodnotu PRAVDA nebo NEPRAVDA.*</span><span class="sxs-lookup"><span data-stu-id="dfbbd-206">*Dependency success flag always shows either true or false.*</span></span>

<span data-ttu-id="dfbbd-207">*Úplné to není znázorněné dotazu SQL.*</span><span class="sxs-lookup"><span data-stu-id="dfbbd-207">*SQL query not shown in full.*</span></span>

* <span data-ttu-id="dfbbd-208">Upgrade na nejnovější verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-208">Upgrade to the latest version of the SDK.</span></span> <span data-ttu-id="dfbbd-209">Pokud vaše verze .NET je menší než 4.6:</span><span class="sxs-lookup"><span data-stu-id="dfbbd-209">If your .NET version is less than 4.6:</span></span>
  * <span data-ttu-id="dfbbd-210">Hostitele služby IIS: Nainstalujte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na hostitelských serverech.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-210">IIS host: Install [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on the host servers.</span></span>
  * <span data-ttu-id="dfbbd-211">Webové aplikace Azure: Otevřete Application Insights v Ovládacích panelech webové aplikace a nainstalujte službu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="dfbbd-211">Azure web app: Open Application Insights tab in the web app control panel, and install Application Insights.</span></span>

## <a name="video"></a><span data-ttu-id="dfbbd-212">Video</span><span class="sxs-lookup"><span data-stu-id="dfbbd-212">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="dfbbd-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfbbd-213">Next steps</span></span>
* [<span data-ttu-id="dfbbd-214">Výjimky</span><span class="sxs-lookup"><span data-stu-id="dfbbd-214">Exceptions</span></span>](app-insights-asp-net-exceptions.md)
* [<span data-ttu-id="dfbbd-215">Data uživatele a stránky</span><span class="sxs-lookup"><span data-stu-id="dfbbd-215">User & page data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="dfbbd-216">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="dfbbd-216">Availability</span></span>](app-insights-monitor-web-app-availability.md)
