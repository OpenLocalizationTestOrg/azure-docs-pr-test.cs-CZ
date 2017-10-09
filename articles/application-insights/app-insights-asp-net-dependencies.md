---
title: "aaaDependency sledování ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: e72f5465462ae8e64363cbbaa62911aff636c504
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a><span data-ttu-id="4eba7-103">Nastavte Application Insights: sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="4eba7-103">Set up Application Insights: Dependency tracking</span></span>
<span data-ttu-id="4eba7-104">A *závislostí* je externí komponenta, která je volána aplikace.</span><span class="sxs-lookup"><span data-stu-id="4eba7-104">A *dependency* is an external component that is called by your app.</span></span> <span data-ttu-id="4eba7-105">Obvykle se jedná o službu volat pomocí protokolu HTTP, nebo databázi nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="4eba7-105">It's typically a service called using HTTP, or a database, or a file system.</span></span> <span data-ttu-id="4eba7-106">[Application Insights](app-insights-overview.md) měří, jak dlouho aplikace čeká závislosti a jak často závislostí volání selže.</span><span class="sxs-lookup"><span data-stu-id="4eba7-106">[Application Insights](app-insights-overview.md) measures how long your application waits for dependencies and how often a dependency call fails.</span></span> <span data-ttu-id="4eba7-107">Můžete prozkoumat konkrétní volání a propojovat je toorequests a výjimky.</span><span class="sxs-lookup"><span data-stu-id="4eba7-107">You can investigate specific calls, and relate them toorequests and exceptions.</span></span>

![ukázkové grafy](./media/app-insights-asp-net-dependencies/10-intro.png)

<span data-ttu-id="4eba7-109">monitorování závislostí na více systémů pole Hello aktuálně sestavy volání toothese typy závislosti:</span><span class="sxs-lookup"><span data-stu-id="4eba7-109">hello out-of-the-box dependency monitor currently reports calls toothese  types of dependencies:</span></span>

* <span data-ttu-id="4eba7-110">Server</span><span class="sxs-lookup"><span data-stu-id="4eba7-110">Server</span></span>
  * <span data-ttu-id="4eba7-111">Databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4eba7-111">SQL databases</span></span>
  * <span data-ttu-id="4eba7-112">Technologie ASP.NET a služby WCF, které používají vazby založené na protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="4eba7-112">ASP.NET web and WCF services that use HTTP-based bindings</span></span>
  * <span data-ttu-id="4eba7-113">Místní nebo vzdálené volání protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="4eba7-113">Local or remote HTTP calls</span></span>
  * <span data-ttu-id="4eba7-114">Azure Cosmos DB, table, úložiště objektů blob a fronty</span><span class="sxs-lookup"><span data-stu-id="4eba7-114">Azure Cosmos DB, table, blob storage, and queue</span></span>
* <span data-ttu-id="4eba7-115">Webové stránky</span><span class="sxs-lookup"><span data-stu-id="4eba7-115">Web pages</span></span>
  * <span data-ttu-id="4eba7-116">Volání AJAX</span><span class="sxs-lookup"><span data-stu-id="4eba7-116">AJAX calls</span></span>

<span data-ttu-id="4eba7-117">Monitorování funguje pomocí [bajtů kód instrumentace](https://msdn.microsoft.com/library/z9z62c29.aspx) kolem vybrané metody.</span><span class="sxs-lookup"><span data-stu-id="4eba7-117">Monitoring works by using [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) around selected methods.</span></span> <span data-ttu-id="4eba7-118">Nároky na výkon je minimální.</span><span class="sxs-lookup"><span data-stu-id="4eba7-118">Performance overhead is minimal.</span></span>

<span data-ttu-id="4eba7-119">Můžete taky napsat vlastní SDK volá toomonitor Další závislosti, v hello kód klienta a serveru, i pomocí hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="4eba7-119">You can also write your own SDK calls toomonitor other dependencies, both in hello client and server code, using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

## <a name="set-up-dependency-monitoring"></a><span data-ttu-id="4eba7-120">Nastavení monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="4eba7-120">Set up dependency monitoring</span></span>
<span data-ttu-id="4eba7-121">Automaticky shromažďuje informace o závislostech částečné hello [Application Insights SDK](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="4eba7-121">Partial dependency information is collected automatically by hello [Application Insights SDK](app-insights-asp-net.md).</span></span> <span data-ttu-id="4eba7-122">kompletní datový tooget, nainstalujte hello odpovídající agenta pro hostitelský server hello.</span><span class="sxs-lookup"><span data-stu-id="4eba7-122">tooget complete data, install hello appropriate agent for hello host server.</span></span>

| <span data-ttu-id="4eba7-123">Platforma</span><span class="sxs-lookup"><span data-stu-id="4eba7-123">Platform</span></span> | <span data-ttu-id="4eba7-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="4eba7-124">Install</span></span> |
| --- | --- |
| <span data-ttu-id="4eba7-125">Server služby IIS</span><span class="sxs-lookup"><span data-stu-id="4eba7-125">IIS Server</span></span> |<span data-ttu-id="4eba7-126">Buď [nainstalujte monitorování stavu na serveru](app-insights-monitor-performance-live-website-now.md) nebo [upgradu vaší aplikace too.NET framework 4.6 nebo novější](http://go.microsoft.com/fwlink/?LinkId=528259) a nainstalujte hello [Application Insights SDK](app-insights-asp-net.md) ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4eba7-126">Either [install Status Monitor on your server](app-insights-monitor-performance-live-website-now.md) or [Upgrade your application too.NET framework 4.6 or later](http://go.microsoft.com/fwlink/?LinkId=528259) and install hello [Application Insights SDK](app-insights-asp-net.md)  in your app.</span></span> |
| <span data-ttu-id="4eba7-127">Webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="4eba7-127">Azure Web App</span></span> |<span data-ttu-id="4eba7-128">Ve webové aplikaci ovládacího panelu [hello otevřete okno Application Insights ve vaší webové aplikace ovládacích panelů](app-insights-azure-web-apps.md) a instalace zvolte, pokud se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="4eba7-128">In your web app control panel, [open hello Application Insights blade in your web app control panel](app-insights-azure-web-apps.md) and choose Install if prompted.</span></span> |
| <span data-ttu-id="4eba7-129">Cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="4eba7-129">Azure Cloud Service</span></span> |<span data-ttu-id="4eba7-130">[Úloha spuštění použití](app-insights-cloudservices.md) nebo [nainstalovat rozhraní .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="4eba7-130">[Use startup task](app-insights-cloudservices.md) or [Install .NET framework 4.6+](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span></span> |

## <a name="where-toofind-dependency-data"></a><span data-ttu-id="4eba7-131">Kde toofind data závislostí</span><span class="sxs-lookup"><span data-stu-id="4eba7-131">Where toofind dependency data</span></span>
* <span data-ttu-id="4eba7-132">[Mapa aplikace](#application-map) vizualizuje závislosti mezi aplikací a sousedních součásti.</span><span class="sxs-lookup"><span data-stu-id="4eba7-132">[Application Map](#application-map) visualizes dependencies between your app and neighbouring components.</span></span>
* <span data-ttu-id="4eba7-133">[Okna výkonu, prohlížeče a selhání](#performance-and-blades) zobrazit závislosti dat serveru.</span><span class="sxs-lookup"><span data-stu-id="4eba7-133">[Performance, browser, and failure blades](#performance-and-blades) show server dependency data.</span></span>
* <span data-ttu-id="4eba7-134">[Okno prohlížečů](#ajax-calls) ukazuje volání AJAX z prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4eba7-134">[Browsers blade](#ajax-calls) shows AJAX calls from your users' browsers.</span></span>
* <span data-ttu-id="4eba7-135">[Klikněte na položku přes pomalé nebo chybných požadavků](#diagnose-slow-requests) volá toocheck jejich závislost.</span><span class="sxs-lookup"><span data-stu-id="4eba7-135">[Click through from slow or failed requests](#diagnose-slow-requests) toocheck their dependency calls.</span></span>
* <span data-ttu-id="4eba7-136">[Analýza](#analytics) lze použít tooquery data závislostí.</span><span class="sxs-lookup"><span data-stu-id="4eba7-136">[Analytics](#analytics) can be used tooquery dependency data.</span></span>

## <a name="application-map"></a><span data-ttu-id="4eba7-137">Mapa aplikace</span><span class="sxs-lookup"><span data-stu-id="4eba7-137">Application Map</span></span>
<span data-ttu-id="4eba7-138">Mapa aplikace funguje jako vizuální pomůcka toodiscovering závislosti mezi hello komponent vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4eba7-138">Application Map acts as a visual aid toodiscovering dependencies between hello components of your application.</span></span> <span data-ttu-id="4eba7-139">Automaticky se generují z hello telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4eba7-139">It is automatically generated from hello telemetry from your app.</span></span> <span data-ttu-id="4eba7-140">Tento příklad ukazuje volání AJAX z hello prohlížeč skriptů a volání REST ze serveru hello aplikace tootwo externích služeb.</span><span class="sxs-lookup"><span data-stu-id="4eba7-140">This example shows AJAX calls from hello browser scripts and REST calls from hello server app tootwo external services.</span></span>

![Mapa aplikace](./media/app-insights-asp-net-dependencies/08.png)

* <span data-ttu-id="4eba7-142">**Přejděte z oken hello** toorelevant závislostí a jinými grafy.</span><span class="sxs-lookup"><span data-stu-id="4eba7-142">**Navigate from hello boxes** toorelevant dependency and other charts.</span></span>
* <span data-ttu-id="4eba7-143">**PIN kód hello mapy** toohello [řídicí panel](app-insights-dashboards.md), kde budou plně funkční.</span><span class="sxs-lookup"><span data-stu-id="4eba7-143">**Pin hello map** toohello [dashboard](app-insights-dashboards.md), where it will be fully functional.</span></span>

<span data-ttu-id="4eba7-144">[Další informace](app-insights-app-map.md).</span><span class="sxs-lookup"><span data-stu-id="4eba7-144">[Learn more](app-insights-app-map.md).</span></span>

## <a name="performance-and-failure-blades"></a><span data-ttu-id="4eba7-145">Výkon a selhání oken</span><span class="sxs-lookup"><span data-stu-id="4eba7-145">Performance and failure blades</span></span>
<span data-ttu-id="4eba7-146">Hello výkonu ukazovat hello trvání závislosti volání aplikace server hello.</span><span class="sxs-lookup"><span data-stu-id="4eba7-146">hello performance blade shows hello duration of dependency calls made by hello server app.</span></span> <span data-ttu-id="4eba7-147">Není souhrnné graf a tabulku oddělených volání.</span><span class="sxs-lookup"><span data-stu-id="4eba7-147">There's a summary chart and a table segmented by call.</span></span>

![Grafy závislostí okno výkonu.](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

<span data-ttu-id="4eba7-149">Proklikejte se prostřednictvím souhrnné grafy hello nebo hello tabulky položky toosearch nezpracovaná výskyty těchto volání.</span><span class="sxs-lookup"><span data-stu-id="4eba7-149">Click through hello summary charts or hello table items toosearch raw occurrences of these calls.</span></span>

![Instance volání závislostí](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

<span data-ttu-id="4eba7-151">**Počet selhání** se zobrazují na hello **selhání** okno.</span><span class="sxs-lookup"><span data-stu-id="4eba7-151">**Failure counts** are shown on hello **Failures** blade.</span></span> <span data-ttu-id="4eba7-152">Selhání je jakýkoli návratový kód, který není v rozsahu 200 hello-399, nebo neznámý.</span><span class="sxs-lookup"><span data-stu-id="4eba7-152">A failure is any return code that is not in hello range 200-399, or unknown.</span></span>

> [!NOTE]
> <span data-ttu-id="4eba7-153">**100 % selhání?**</span><span class="sxs-lookup"><span data-stu-id="4eba7-153">**100% failures?**</span></span> <span data-ttu-id="4eba7-154">– Pravděpodobně to znamená, jsou jenom získávání dat částečné závislostí.</span><span class="sxs-lookup"><span data-stu-id="4eba7-154">- This probably indicates that you are only getting partial dependency data.</span></span> <span data-ttu-id="4eba7-155">Je třeba příliš[nastavit závislost monitorování platformy odpovídající tooyour](#set-up-dependency-monitoring).</span><span class="sxs-lookup"><span data-stu-id="4eba7-155">You need too[set up dependency monitoring appropriate tooyour platform](#set-up-dependency-monitoring).</span></span>
>
>

## <a name="ajax-calls"></a><span data-ttu-id="4eba7-156">Volání AJAX</span><span class="sxs-lookup"><span data-stu-id="4eba7-156">AJAX Calls</span></span>
<span data-ttu-id="4eba7-157">Doba trvání hello se zobrazí okno prohlížeče Hello a míra selhání AJAX volání ze [JavaScript na webových stránkách](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="4eba7-157">hello Browsers blade shows hello duration and failure rate of AJAX calls from [JavaScript in your web pages](app-insights-javascript.md).</span></span> <span data-ttu-id="4eba7-158">Zobrazí se jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="4eba7-158">They are shown as Dependencies.</span></span>

## <span data-ttu-id="4eba7-159"><a name="diagnosis"></a>Diagnostika pomalé požadavků</span><span class="sxs-lookup"><span data-stu-id="4eba7-159"><a name="diagnosis"></a> Diagnose slow requests</span></span>
<span data-ttu-id="4eba7-160">Každá žádost o událost souvisí s hello závislostí volání, výjimky a další události, které jsou sledovány při zpracování aplikace hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="4eba7-160">Each request event is associated with hello dependency calls, exceptions and other events that are tracked while your app is processing hello request.</span></span> <span data-ttu-id="4eba7-161">Takže pokud chybně provedli některé požadavky, můžete zjistit ať to je z důvodu tooslow odpovědí z závislost.</span><span class="sxs-lookup"><span data-stu-id="4eba7-161">So if some requests are performing badly, you can find out whether it's due tooslow responses from a dependency.</span></span>

<span data-ttu-id="4eba7-162">Projděme příklad této.</span><span class="sxs-lookup"><span data-stu-id="4eba7-162">Let's walk through an example of that.</span></span>

### <a name="tracing-from-requests-toodependencies"></a><span data-ttu-id="4eba7-163">Trasování z toodependencies požadavky</span><span class="sxs-lookup"><span data-stu-id="4eba7-163">Tracing from requests toodependencies</span></span>
<span data-ttu-id="4eba7-164">Otevřete okno výkon hello a podívejte se na hello mřížky požadavků:</span><span class="sxs-lookup"><span data-stu-id="4eba7-164">Open hello Performance blade, and look at hello grid of requests:</span></span>

![Seznam požadavků s průměry a počty](./media/app-insights-asp-net-dependencies/02-reqs.png)

<span data-ttu-id="4eba7-166">Hello nejvyšší, které jeden trvá velmi dlouho.</span><span class="sxs-lookup"><span data-stu-id="4eba7-166">hello top one is taking very long.</span></span> <span data-ttu-id="4eba7-167">Podívejme se, pokud jsme můžete zjistit, kde je hello čas strávený.</span><span class="sxs-lookup"><span data-stu-id="4eba7-167">Let's see if we can find out where hello time is spent.</span></span>

<span data-ttu-id="4eba7-168">Klikněte na události jednotlivých žádostí toosee tento řádek:</span><span class="sxs-lookup"><span data-stu-id="4eba7-168">Click that row toosee individual request events:</span></span>

![Seznam výskytů požadavku](./media/app-insights-asp-net-dependencies/03-instances.png)

<span data-ttu-id="4eba7-170">Klikněte na všechny instance tooinspect dlouho běžící dál a posuňte se dolů toohello závislostí vzdáleného volání související toothis žádost:</span><span class="sxs-lookup"><span data-stu-id="4eba7-170">Click any long-running instance tooinspect it further, and scroll down toohello remote dependency calls related toothis request:</span></span>

![Najít volání tooRemote závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-dependencies.png)

<span data-ttu-id="4eba7-172">To vypadá většinu hello doba údržby, které tento požadavek byl stráven v místní službě tooa volání.</span><span class="sxs-lookup"><span data-stu-id="4eba7-172">It looks like most of hello time servicing this request was spent in a call tooa local service.</span></span>

<span data-ttu-id="4eba7-173">Vyberte tento řádek tooget Další informace:</span><span class="sxs-lookup"><span data-stu-id="4eba7-173">Select that row tooget more information:</span></span>

![Klikněte na tlačítko prostřednictvím tohoto vzdáleného závislostí tooidentify hello který](./media/app-insights-asp-net-dependencies/05-detail.png)

<span data-ttu-id="4eba7-175">Vypadá to jde, kde je problém hello.</span><span class="sxs-lookup"><span data-stu-id="4eba7-175">Looks like this is where hello problem is.</span></span> <span data-ttu-id="4eba7-176">Jsme jste přesně vymezená problém hello, takže teď jsme právě měli toofind out proč tohoto volání trvá tak dlouho.</span><span class="sxs-lookup"><span data-stu-id="4eba7-176">We've pinpointed hello problem, so now we just need toofind out why that call is taking so long.</span></span>

### <a name="request-timeline"></a><span data-ttu-id="4eba7-177">Časová osa požadavku</span><span class="sxs-lookup"><span data-stu-id="4eba7-177">Request timeline</span></span>
<span data-ttu-id="4eba7-178">V případě různých je volání žádné závislosti, které je zvláště dlouhý.</span><span class="sxs-lookup"><span data-stu-id="4eba7-178">In a different case, there is no dependency call that is particularly long.</span></span> <span data-ttu-id="4eba7-179">Ale přepnutím zobrazení časové osy toohello uvidíme, kde došlo k hello zpoždění v našem interní zpracování:</span><span class="sxs-lookup"><span data-stu-id="4eba7-179">But by switching toohello timeline view, we can see where hello delay occurred in our internal processing:</span></span>

![Najít volání tooRemote závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-1.png)

<span data-ttu-id="4eba7-181">Vypadá to, toobe big mezera po prvním volání závislostí hello, takže jsme by měl vypadat v našem kódu toosee, proč to znamená.</span><span class="sxs-lookup"><span data-stu-id="4eba7-181">There seems toobe a big gap after hello first dependency call, so we should look at our code toosee why that is.</span></span>

### <a name="profile-your-live-site"></a><span data-ttu-id="4eba7-182">Profil živý web</span><span class="sxs-lookup"><span data-stu-id="4eba7-182">Profile your live site</span></span>

<span data-ttu-id="4eba7-183">Žádné představu, kde přejde hello čas? Hello [Application Insights profileru](app-insights-profiler.md) trasování HTTP volá tooyour živý web a ukazuje, které funkce ve vašem kódu trvalo hello nejdelší dobu.</span><span class="sxs-lookup"><span data-stu-id="4eba7-183">No idea where hello time goes? hello [Application Insights profiler](app-insights-profiler.md) traces HTTP calls tooyour live site and shows you which functions in your code took hello longest time.</span></span>

## <a name="failed-requests"></a><span data-ttu-id="4eba7-184">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="4eba7-184">Failed requests</span></span>
<span data-ttu-id="4eba7-185">Neúspěšné požadavky může být také přidružen toodependencies volání se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4eba7-185">Failed requests might also be associated with failed calls toodependencies.</span></span> <span data-ttu-id="4eba7-186">Jsme znovu, můžete kliknutím prostřednictvím tootrack dolů hello problém.</span><span class="sxs-lookup"><span data-stu-id="4eba7-186">Again, we can click through tootrack down hello problem.</span></span>

![Klikněte na graf neúspěšných požadavků hello](./media/app-insights-asp-net-dependencies/06-fail.png)

<span data-ttu-id="4eba7-188">Klikněte na tlačítko prostřednictvím tooan výskytem chybné žádosti a podívejte se na jeho přidružené události.</span><span class="sxs-lookup"><span data-stu-id="4eba7-188">Click through tooan occurrence of a failed request, and look at its associated events.</span></span>

![Klikněte na typ požadavku, klikněte na tlačítko hello tooget tooa různých zobrazení instance hello stejnou instanci, klikněte na podrobnosti o výjimce tooget.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a><span data-ttu-id="4eba7-190">Analýza</span><span class="sxs-lookup"><span data-stu-id="4eba7-190">Analytics</span></span>
<span data-ttu-id="4eba7-191">Můžete sledovat závislostí v hello [analýzy protokolů dotazu jazyka](https://docs.loganalytics.io/).</span><span class="sxs-lookup"><span data-stu-id="4eba7-191">You can track dependencies in hello [Log Analytics query language](https://docs.loganalytics.io/).</span></span> <span data-ttu-id="4eba7-192">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="4eba7-192">Here are some examples.</span></span>

* <span data-ttu-id="4eba7-193">Najděte žádné volání se nezdařilo závislost:</span><span class="sxs-lookup"><span data-stu-id="4eba7-193">Find any failed dependency calls:</span></span>

```

    dependencies | where success != "True" | take 10
```

* <span data-ttu-id="4eba7-194">Najděte volání AJAX:</span><span class="sxs-lookup"><span data-stu-id="4eba7-194">Find AJAX calls:</span></span>

```

    dependencies | where client_Type == "Browser" | take 10
```

* <span data-ttu-id="4eba7-195">Najděte závislostí volání přidružené k žádosti:</span><span class="sxs-lookup"><span data-stu-id="4eba7-195">Find dependency calls associated with requests:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* <span data-ttu-id="4eba7-196">Volání AJAX najít přidružené zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="4eba7-196">Find AJAX calls associated with page views:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a><span data-ttu-id="4eba7-197">Vlastní závislost sledování</span><span class="sxs-lookup"><span data-stu-id="4eba7-197">Custom dependency tracking</span></span>
<span data-ttu-id="4eba7-198">standardní modul sledování závislostí Hello automaticky zjišťuje externí závislosti, jako jsou databáze a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="4eba7-198">hello standard dependency-tracking module automatically discovers external dependencies such as databases and REST APIs.</span></span> <span data-ttu-id="4eba7-199">Ale můžete chtít některé další součásti toobe zpracovávají v hello stejný způsobem.</span><span class="sxs-lookup"><span data-stu-id="4eba7-199">But you might want some additional components toobe treated in hello same way.</span></span>

<span data-ttu-id="4eba7-200">Můžete napsat kód, který odesílá informace o závislostech, pomocí stejné hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) používané standardní moduly hello.</span><span class="sxs-lookup"><span data-stu-id="4eba7-200">You can write code that sends dependency information, using hello same [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) that is used by hello standard modules.</span></span>

<span data-ttu-id="4eba7-201">Například pokud vytvoříte kódu se sestavením, že napíšete nebyla sami, může čas všechny tooit volání hello, případech toofind out jaký příspěvek umožňuje tooyour odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4eba7-201">For example, if you build your code with an assembly that you didn't write yourself, you could time all hello calls tooit, toofind out what contribution it makes tooyour response times.</span></span> <span data-ttu-id="4eba7-202">toohave tato data zobrazí v grafech závislostí hello ve službě Application Insights odeslat pomocí `TrackDependency`.</span><span class="sxs-lookup"><span data-stu-id="4eba7-202">toohave this data displayed in hello dependency charts in Application Insights, send it using `TrackDependency`.</span></span>

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

<span data-ttu-id="4eba7-203">Pokud chcete tooswitch vypnout hello standardní závislost sledování modulu, odeberte hello tooDependencyTrackingTelemetryModule odkaz v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span><span class="sxs-lookup"><span data-stu-id="4eba7-203">If you want tooswitch off hello standard dependency tracking module, remove hello reference tooDependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4eba7-204">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4eba7-204">Troubleshooting</span></span>
<span data-ttu-id="4eba7-205">*Závislost úspěch příznak vždycky zobrazí hodnotu PRAVDA nebo NEPRAVDA.*</span><span class="sxs-lookup"><span data-stu-id="4eba7-205">*Dependency success flag always shows either true or false.*</span></span>

<span data-ttu-id="4eba7-206">*Úplné to není znázorněné dotazu SQL.*</span><span class="sxs-lookup"><span data-stu-id="4eba7-206">*SQL query not shown in full.*</span></span>

* <span data-ttu-id="4eba7-207">Upgrade toohello nejnovější verzi hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4eba7-207">Upgrade toohello latest version of hello SDK.</span></span> <span data-ttu-id="4eba7-208">Pokud vaše verze .NET je menší než 4.6:</span><span class="sxs-lookup"><span data-stu-id="4eba7-208">If your .NET version is less than 4.6:</span></span>
  * <span data-ttu-id="4eba7-209">Hostitele služby IIS: Nainstalujte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na hostitelských serverech hello.</span><span class="sxs-lookup"><span data-stu-id="4eba7-209">IIS host: Install [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on hello host servers.</span></span>
  * <span data-ttu-id="4eba7-210">Webové aplikace Azure: Otevřete Application Insights v hello webové aplikace ovládacích panelů a nainstalujte službu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4eba7-210">Azure web app: Open Application Insights tab in hello web app control panel, and install Application Insights.</span></span>

## <a name="video"></a><span data-ttu-id="4eba7-211">Video</span><span class="sxs-lookup"><span data-stu-id="4eba7-211">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="4eba7-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4eba7-212">Next steps</span></span>
* [<span data-ttu-id="4eba7-213">Výjimky</span><span class="sxs-lookup"><span data-stu-id="4eba7-213">Exceptions</span></span>](app-insights-asp-net-exceptions.md)
* [<span data-ttu-id="4eba7-214">Data uživatele a stránky</span><span class="sxs-lookup"><span data-stu-id="4eba7-214">User & page data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="4eba7-215">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="4eba7-215">Availability</span></span>](app-insights-monitor-web-app-availability.md)
