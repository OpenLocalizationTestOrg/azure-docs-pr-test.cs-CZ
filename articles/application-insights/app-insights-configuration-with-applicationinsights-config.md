---
title: "referenční dokumentace aaaApplicationInsights.config - Azure | Microsoft Docs"
description: "Povolte nebo zakažte moduly shromažďování dat a přidejte čítače výkonu a dalších parametrů."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: alancameronwills
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 76cb11349d87dfc508ec8b1c454259a0b079c48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a><span data-ttu-id="1559c-103">Konfigurace hello Application Insights SDK souboru ApplicationInsights.config či .xml</span><span class="sxs-lookup"><span data-stu-id="1559c-103">Configuring hello Application Insights SDK with ApplicationInsights.config or .xml</span></span>
<span data-ttu-id="1559c-104">Hello Application Insights .NET SDK se skládá z řady balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-104">hello Application Insights .NET SDK consists of a number of NuGet packages.</span></span> <span data-ttu-id="1559c-105">[Základní balíček](http://www.nuget.org/packages/Microsoft.ApplicationInsights) poskytuje hello rozhraní API pro odesílání telemetrie do hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1559c-105">The [core package](http://www.nuget.org/packages/Microsoft.ApplicationInsights) provides hello API for sending telemetry to hello Application Insights.</span></span> <span data-ttu-id="1559c-106">[Další balíčky](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) poskytují telemetrie *moduly* a *inicializátory* pro automaticky sledování telemetrie z vaší aplikace a jeho kontextu.</span><span class="sxs-lookup"><span data-stu-id="1559c-106">[Additional packages](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) provide telemetry *modules* and *initializers* for automatically tracking telemetry from your application and its context.</span></span> <span data-ttu-id="1559c-107">Úpravou konfiguračního souboru hello můžete povolit nebo zakázat telemetrii moduly a inicializátory a nastavit parametry pro některé z nich.</span><span class="sxs-lookup"><span data-stu-id="1559c-107">By adjusting hello configuration file, you can enable or disable telemetry modules and initializers, and set parameters for some of them.</span></span>

<span data-ttu-id="1559c-108">konfigurační soubor Hello jmenuje `ApplicationInsights.config` nebo `ApplicationInsights.xml`, v závislosti na typu hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-108">hello configuration file is named `ApplicationInsights.config` or `ApplicationInsights.xml`, depending on hello type of your application.</span></span> <span data-ttu-id="1559c-109">Je automaticky přidán tooyour projektu, když jste [instalaci většiny verzích hello SDK][start].</span><span class="sxs-lookup"><span data-stu-id="1559c-109">It is automatically added tooyour project when you [install most versions of hello SDK][start].</span></span> <span data-ttu-id="1559c-110">Je také přidán tooa webové aplikace pomocí [monitorování stavu na serveru se službou IIS][redfield], nebo když vyberete hello Appplication Insights [rozšíření pro webové stránky Azure, nebo virtuální počítač](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1559c-110">It is also added tooa web app by [Status Monitor on an IIS server][redfield], or when you select hello Appplication Insights [extension for an Azure website or VM](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="1559c-111">Není k dispozici ekvivalentní soubor toocontrol hello [SDK na webové stránce][client].</span><span class="sxs-lookup"><span data-stu-id="1559c-111">There isn't an equivalent file toocontrol hello [SDK in a web page][client].</span></span>

<span data-ttu-id="1559c-112">Tento dokument popisuje hello oddíly, které vidíte v konfiguraci hello souboru, jak budou řídit hello součástí hello SDK, a které balíčky NuGet načtení těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="1559c-112">This document describes hello sections you see in hello configuration file, how they control hello components of hello SDK, and which NuGet packages load those components.</span></span>

## <a name="telemetry-modules-aspnet"></a><span data-ttu-id="1559c-113">Telemetrie moduly (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1559c-113">Telemetry Modules (ASP.NET)</span></span>
<span data-ttu-id="1559c-114">Každý modul telemetrie shromažďuje konkrétní typ dat a používá hello jádra rozhraní API toosend hello data.</span><span class="sxs-lookup"><span data-stu-id="1559c-114">Each telemetry module collects a specific type of data and uses hello core API toosend hello data.</span></span> <span data-ttu-id="1559c-115">Hello moduly jsou nainstalovány jiné balíčky NuGet, které také přidat soubor .config toohello hello požadovaných řádků.</span><span class="sxs-lookup"><span data-stu-id="1559c-115">hello modules are installed by different NuGet packages, which also add hello required lines toohello .config file.</span></span>

<span data-ttu-id="1559c-116">V hello konfiguračního souboru pro každý modul je uzel.</span><span class="sxs-lookup"><span data-stu-id="1559c-116">There's a node in hello configuration file for each module.</span></span> <span data-ttu-id="1559c-117">toodisable modul, odstraňte hello uzlu nebo ho nastavte jako komentář.</span><span class="sxs-lookup"><span data-stu-id="1559c-117">toodisable a module, delete hello node or comment it out.</span></span>

### <a name="dependency-tracking"></a><span data-ttu-id="1559c-118">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="1559c-118">Dependency Tracking</span></span>
<span data-ttu-id="1559c-119">[Závislost sledování](app-insights-asp-net-dependencies.md) shromažďuje telemetrická data o volání aplikace umožňuje toodatabases a externích služeb a databází.</span><span class="sxs-lookup"><span data-stu-id="1559c-119">[Dependency tracking](app-insights-asp-net-dependencies.md) collects telemetry about calls your app makes toodatabases and external services and databases.</span></span> <span data-ttu-id="1559c-120">tooallow tento modul toowork v serveru se službou IIS, je třeba příliš[nainstalujte monitorování stavu][redfield].</span><span class="sxs-lookup"><span data-stu-id="1559c-120">tooallow this module toowork in an IIS server, you need too[install Status Monitor][redfield].</span></span> <span data-ttu-id="1559c-121">toouse ji ve službě Azure web apps nebo virtuálních počítačů, [vyberte rozšíření Application Insights hello](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1559c-121">toouse it in Azure web apps or VMs, [select hello Application Insights extension](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="1559c-122">Můžete taky napsat vlastní závislost sledování kódu pomocí hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="1559c-122">You can also write your own dependency tracking code using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* <span data-ttu-id="1559c-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.</span></span>

### <a name="performance-collector"></a><span data-ttu-id="1559c-124">Kolekce výkonu</span><span class="sxs-lookup"><span data-stu-id="1559c-124">Performance collector</span></span>
<span data-ttu-id="1559c-125">[Shromažďuje čítače výkonu systému](app-insights-performance-counters.md) například CPU, paměť a síť načíst z instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1559c-125">[Collects system performance counters](app-insights-performance-counters.md) such as CPU, memory and network load from IIS installations.</span></span> <span data-ttu-id="1559c-126">Můžete určit, které toocollect čítače, včetně čítačů výkonu, které jste nastavili sami.</span><span class="sxs-lookup"><span data-stu-id="1559c-126">You can specify which counters toocollect, including performance counters you have set up yourself.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* <span data-ttu-id="1559c-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.</span></span>

### <a name="application-insights-diagnostics-telemetry"></a><span data-ttu-id="1559c-128">Telemetrii Diagnostics Application Insights</span><span class="sxs-lookup"><span data-stu-id="1559c-128">Application Insights Diagnostics Telemetry</span></span>
<span data-ttu-id="1559c-129">Hello `DiagnosticsTelemetryModule` sestavy chyb v hello samotný kód instrumentace Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1559c-129">hello `DiagnosticsTelemetryModule` reports errors in hello Application Insights instrumentation code itself.</span></span> <span data-ttu-id="1559c-130">Například pokud hello kód nemůže přistupovat k čítače výkonu nebo `ITelemetryInitializer` vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="1559c-130">For example, if hello code cannot access performance counters or if an `ITelemetryInitializer` throws an exception.</span></span> <span data-ttu-id="1559c-131">Trasování telemetrie sleduje tento modul se zobrazí v hello [diagnostické vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="1559c-131">Trace telemetry tracked by this module appears in hello [Diagnostic Search][diagnostic].</span></span> <span data-ttu-id="1559c-132">Odešle toodc.services.vsallin.net diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="1559c-132">Sends diagnostic data toodc.services.vsallin.net.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* <span data-ttu-id="1559c-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="1559c-134">Pokud máte pouze instalaci tohoto balíčku, není soubor ApplicationInsights.config hello vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="1559c-134">If you only install this package, hello ApplicationInsights.config file is not automatically created.</span></span>

### <a name="developer-mode"></a><span data-ttu-id="1559c-135">Režim vývojáře</span><span class="sxs-lookup"><span data-stu-id="1559c-135">Developer Mode</span></span>
<span data-ttu-id="1559c-136">`DeveloperModeWithDebuggerAttachedTelemetryModule`Vynutí hello Application Insights `TelemetryChannel` toosend data okamžitě, jeden telemetrie položky v době, kdy je ladicí program připojených toohello proces aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-136">`DeveloperModeWithDebuggerAttachedTelemetryModule` forces hello Application Insights `TelemetryChannel` toosend data immediately, one telemetry item at a time, when a debugger is attached toohello application process.</span></span> <span data-ttu-id="1559c-137">To snižuje hello množství času mezi hello chvíli, když vaše aplikace sleduje telemetrie a když se zobrazí na portálu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-137">This reduces hello amount of time between hello moment when your application tracks telemetry and when it appears on hello Application Insights portal.</span></span> <span data-ttu-id="1559c-138">Způsobuje významné režijní náklady v procesoru a šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="1559c-138">It causes significant overhead in CPU and network bandwidth.</span></span>

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* <span data-ttu-id="1559c-139">[Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="1559c-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package</span></span>

### <a name="web-request-tracking"></a><span data-ttu-id="1559c-140">Sledování webové žádosti</span><span class="sxs-lookup"><span data-stu-id="1559c-140">Web Request Tracking</span></span>
<span data-ttu-id="1559c-141">Sestavy hello [čas a výsledek kód odpovědi](app-insights-asp-net.md) požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="1559c-141">Reports hello [response time and result code](app-insights-asp-net.md) of HTTP requests.</span></span>

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* <span data-ttu-id="1559c-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="1559c-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>

### <a name="exception-tracking"></a><span data-ttu-id="1559c-143">Sledování výjimek</span><span class="sxs-lookup"><span data-stu-id="1559c-143">Exception tracking</span></span>
<span data-ttu-id="1559c-144">`ExceptionTrackingTelemetryModule`sleduje neošetřených výjimek ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1559c-144">`ExceptionTrackingTelemetryModule` tracks unhandled exceptions in your web app.</span></span> <span data-ttu-id="1559c-145">V tématu [chyby a výjimky][exceptions].</span><span class="sxs-lookup"><span data-stu-id="1559c-145">See [Failures and exceptions][exceptions].</span></span>

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* <span data-ttu-id="1559c-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="1559c-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>
* <span data-ttu-id="1559c-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-sleduje [nepozorovaná úloh výjimky](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span><span class="sxs-lookup"><span data-stu-id="1559c-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` - tracks [unobserved task exceptions](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span></span>
* <span data-ttu-id="1559c-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-sleduje neošetřené výjimky pro role pracovního procesu, služby systému windows a konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` - tracks unhandled exceptions for worker roles, windows services, and console applications.</span></span>
* <span data-ttu-id="1559c-149">[Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.</span></span>

### <a name="eventsource-tracking"></a><span data-ttu-id="1559c-150">EventSource sledování</span><span class="sxs-lookup"><span data-stu-id="1559c-150">EventSource Tracking</span></span>
<span data-ttu-id="1559c-151">`EventSourceTelemetryModule`Umožňuje vám tooconfigure toobe událostí EventSource odeslána tooApplication Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="1559c-151">`EventSourceTelemetryModule` allows you tooconfigure EventSource events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="1559c-152">Informace o sledování událostí EventSource najdete v tématu [pomocí událostí EventSource](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span><span class="sxs-lookup"><span data-stu-id="1559c-152">For information on tracking EventSource events, see [Using EventSource Events](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span></span>

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [<span data-ttu-id="1559c-153">Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="1559c-153">Microsoft.ApplicationInsights.EventSourceListener</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a><span data-ttu-id="1559c-154">Sledování událostí trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="1559c-154">ETW Event Tracking</span></span>
<span data-ttu-id="1559c-155">`EtwCollectorTelemetryModule`Umožňuje tooconfigure události z toobe zprostředkovatelé trasování událostí pro Windows odeslány tooApplication Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="1559c-155">`EtwCollectorTelemetryModule` allows you tooconfigure events from ETW providers toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="1559c-156">Informace o sledování události trasování událostí pro Windows najdete v tématu [události trasování událostí pro Windows pomocí](app-insights-asp-net-trace-logs.md#using-etw-events).</span><span class="sxs-lookup"><span data-stu-id="1559c-156">For information on tracking ETW events, see [Using ETW Events](app-insights-asp-net-trace-logs.md#using-etw-events).</span></span>

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [<span data-ttu-id="1559c-157">Microsoft.ApplicationInsights.EtwCollector</span><span class="sxs-lookup"><span data-stu-id="1559c-157">Microsoft.ApplicationInsights.EtwCollector</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a><span data-ttu-id="1559c-158">Microsoft.ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="1559c-158">Microsoft.ApplicationInsights</span></span>
<span data-ttu-id="1559c-159">balíček Microsoft.ApplicationInsights Hello poskytuje hello [základní rozhraní API](https://msdn.microsoft.com/library/mt420197.aspx) z hello SDK.</span><span class="sxs-lookup"><span data-stu-id="1559c-159">hello Microsoft.ApplicationInsights package provides hello [core API](https://msdn.microsoft.com/library/mt420197.aspx) of hello SDK.</span></span> <span data-ttu-id="1559c-160">Hello telemetrická data z ostatních modulů použít, a můžete také [použít toodefine vlastní telemetrii](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="1559c-160">hello other telemetry modules use this, and you can also [use it toodefine your own telemetry](app-insights-api-custom-events-metrics.md).</span></span>

* <span data-ttu-id="1559c-161">Žádný záznam v souboru ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="1559c-161">No entry in ApplicationInsights.config.</span></span>
* <span data-ttu-id="1559c-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="1559c-163">Pokud instalujete právě tento NuGet, vygeneruje se žádný soubor .config.</span><span class="sxs-lookup"><span data-stu-id="1559c-163">If you just install this NuGet, no .config file is generated.</span></span>

## <a name="telemetry-channel"></a><span data-ttu-id="1559c-164">Kanál telemetrie</span><span class="sxs-lookup"><span data-stu-id="1559c-164">Telemetry Channel</span></span>
<span data-ttu-id="1559c-165">kanál telemetrie Hello spravuje ukládání do vyrovnávací paměti a přenos telemetrie toohello služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1559c-165">hello telemetry channel manages buffering and transmission of telemetry toohello Application Insights service.</span></span>

* <span data-ttu-id="1559c-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`je výchozím hello kanálu pro služby.</span><span class="sxs-lookup"><span data-stu-id="1559c-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is hello default channel for services.</span></span> <span data-ttu-id="1559c-167">Data ukládány v paměti.</span><span class="sxs-lookup"><span data-stu-id="1559c-167">It buffers data in memory.</span></span>
* <span data-ttu-id="1559c-168">`Microsoft.ApplicationInsights.PersistenceChannel`představuje alternativu pro konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-168">`Microsoft.ApplicationInsights.PersistenceChannel` is an alternative for console applications.</span></span> <span data-ttu-id="1559c-169">Jakékoli úložiště toopersistent unflushed data ho můžete uložit, když vaše aplikace zavře a odešle ho při spuštění aplikace hello znovu.</span><span class="sxs-lookup"><span data-stu-id="1559c-169">It can save any unflushed data toopersistent storage when your app closes down, and will send it when hello app starts again.</span></span>

## <a name="telemetry-initializers-aspnet"></a><span data-ttu-id="1559c-170">Inicializátory telemetrie (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1559c-170">Telemetry Initializers (ASP.NET)</span></span>
<span data-ttu-id="1559c-171">Inicializátory telemetrie nastavit kontext vlastnosti, které se odesílají společně s každou položkou telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1559c-171">Telemetry initializers set context properties that are sent along with every item of telemetry.</span></span>

<span data-ttu-id="1559c-172">Můžete [zápisu vlastní inicializátory](app-insights-api-filtering-sampling.md#add-properties) tooset vlastností kontextu.</span><span class="sxs-lookup"><span data-stu-id="1559c-172">You can [write your own initializers](app-insights-api-filtering-sampling.md#add-properties) tooset context properties.</span></span>

<span data-ttu-id="1559c-173">Standardní inicializátory Hello jsou nastavené buď hello Web nebo Windows Server NuGet balíčky:</span><span class="sxs-lookup"><span data-stu-id="1559c-173">hello standard initializers are all set either by hello Web or WindowsServer NuGet packages:</span></span>

* <span data-ttu-id="1559c-174">`AccountIdTelemetryInitializer`Nastaví vlastnost AccountId hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-174">`AccountIdTelemetryInitializer` sets hello AccountId property.</span></span>
* <span data-ttu-id="1559c-175">`AuthenticatedUserIdTelemetryInitializer`Nastaví vlastnost AuthenticatedUserId hello jako sada podle hello JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="1559c-175">`AuthenticatedUserIdTelemetryInitializer` sets hello AuthenticatedUserId property as set by hello JavaScript SDK.</span></span>
* <span data-ttu-id="1559c-176">`AzureRoleEnvironmentTelemetryInitializer`aktualizace hello `RoleName` a `RoleInstance` vlastnosti hello `Device` kontext pro všechny položky telemetrie vyplní hello Azure běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="1559c-176">`AzureRoleEnvironmentTelemetryInitializer` updates hello `RoleName` and `RoleInstance` properties of hello `Device` context for all telemetry items with information extracted from hello Azure runtime environment.</span></span>
* <span data-ttu-id="1559c-177">`BuildInfoConfigComponentVersionTelemetryInitializer`aktualizace hello `Version` vlastnost hello `Component` kontext pro všechny položky telemetrie s hodnotou hello extrahoval z hello `BuildInfo.config` souboru vytvořeného pomocí MS Build.</span><span class="sxs-lookup"><span data-stu-id="1559c-177">`BuildInfoConfigComponentVersionTelemetryInitializer` updates hello `Version` property of hello `Component` context for all telemetry items with hello value extracted from hello `BuildInfo.config` file produced by MS Build.</span></span>
* <span data-ttu-id="1559c-178">`ClientIpHeaderTelemetryInitializer`aktualizace `Ip` vlastnost hello `Location` kontextu všechny položky telemetrie podle hello `X-Forwarded-For` hlavičky protokolu HTTP žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-178">`ClientIpHeaderTelemetryInitializer` updates `Ip` property of hello `Location` context of all telemetry items based on hello `X-Forwarded-For` HTTP header of hello request.</span></span>
* <span data-ttu-id="1559c-179">`DeviceTelemetryInitializer`aktualizace hello následující vlastnosti hello `Device` kontext pro všechny položky telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1559c-179">`DeviceTelemetryInitializer` updates hello following properties of hello `Device` context for all telemetry items.</span></span>
  * <span data-ttu-id="1559c-180">`Type`je nastaven příliš "Počítač"</span><span class="sxs-lookup"><span data-stu-id="1559c-180">`Type` is set too"PC"</span></span>
  * <span data-ttu-id="1559c-181">`Id`je nastavit název domény toohello hello počítače se spuštěným hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-181">`Id` is set toohello domain name of hello computer where hello web application is running.</span></span>
  * <span data-ttu-id="1559c-182">`OemName`je nastavena hodnota toohello extrahoval z hello `Win32_ComputerSystem.Manufacturer` pole pomocí služby WMI.</span><span class="sxs-lookup"><span data-stu-id="1559c-182">`OemName` is set toohello value extracted from hello `Win32_ComputerSystem.Manufacturer` field using WMI.</span></span>
  * <span data-ttu-id="1559c-183">`Model`je nastavena hodnota toohello extrahoval z hello `Win32_ComputerSystem.Model` pole pomocí služby WMI.</span><span class="sxs-lookup"><span data-stu-id="1559c-183">`Model` is set toohello value extracted from hello `Win32_ComputerSystem.Model` field using WMI.</span></span>
  * <span data-ttu-id="1559c-184">`NetworkType`je nastavena hodnota toohello extrahoval z hello `NetworkInterface`.</span><span class="sxs-lookup"><span data-stu-id="1559c-184">`NetworkType` is set toohello value extracted from hello `NetworkInterface`.</span></span>
  * <span data-ttu-id="1559c-185">`Language`je nastaven toohello název hello `CurrentCulture`.</span><span class="sxs-lookup"><span data-stu-id="1559c-185">`Language` is set toohello name of hello `CurrentCulture`.</span></span>
* <span data-ttu-id="1559c-186">`DomainNameRoleInstanceTelemetryInitializer`aktualizace hello `RoleInstance` vlastnost hello `Device` kontext pro všechny položky telemetrie s názvem domény hello hello počítače se spuštěným hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1559c-186">`DomainNameRoleInstanceTelemetryInitializer` updates hello `RoleInstance` property of hello `Device` context for all telemetry items with hello domain name of hello computer where hello web application is running.</span></span>
* <span data-ttu-id="1559c-187">`OperationNameTelemetryInitializer`aktualizace hello `Name` vlastnost hello `RequestTelemetry` a hello `Name` vlastnost hello `Operation` kontextu všechny položky telemetrie podle metoda hello HTTP, jakož i názvy ASP.NET MVC kontroleru a akce vyvolaná tooprocess hello požadavek.</span><span class="sxs-lookup"><span data-stu-id="1559c-187">`OperationNameTelemetryInitializer` updates hello `Name` property of hello `RequestTelemetry` and hello `Name` property of hello `Operation` context of all telemetry items based on hello HTTP method, as well as names of ASP.NET MVC controller and action invoked tooprocess hello request.</span></span>
* <span data-ttu-id="1559c-188">`OperationIdTelemetryInitializer`nebo `OperationCorrelationTelemetryInitializer` aktualizace hello `Operation.Id` vlastností kontextu všechny položky telemetrie sledovat při zpracování požadavku s hello automaticky generovány `RequestTelemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="1559c-188">`OperationIdTelemetryInitializer` or `OperationCorrelationTelemetryInitializer` updates hello `Operation.Id` context property of all telemetry items tracked while handling a request with hello automatically generated `RequestTelemetry.Id`.</span></span>
* <span data-ttu-id="1559c-189">`SessionTelemetryInitializer`aktualizace hello `Id` vlastnost hello `Session` kontext pro všechny položky telemetrie s hodnotou z hello extrahovat `ai_session` souboru cookie generované hello kód instrumentace ApplicationInsights JavaScript v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="1559c-189">`SessionTelemetryInitializer` updates hello `Id` property of hello `Session` context for all telemetry items with value extracted from hello `ai_session` cookie generated by hello ApplicationInsights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="1559c-190">`SyntheticTelemetryInitializer`nebo `SyntheticUserAgentTelemetryInitializer` aktualizace hello `User`, `Session` a `Operation` kontexty vlastnosti všech položek telemetrie sledovat při zpracování požadavku z syntetické zdroje, například dostupnosti testů nebo vyhledávání modul robota.</span><span class="sxs-lookup"><span data-stu-id="1559c-190">`SyntheticTelemetryInitializer` or `SyntheticUserAgentTelemetryInitializer` updates hello `User`, `Session` and `Operation` contexts properties of all telemetry items tracked when handling a request from a synthetic source, such as an availability test or search engine bot.</span></span> <span data-ttu-id="1559c-191">Ve výchozím nastavení [Průzkumníku metrik](app-insights-metrics-explorer.md) nezobrazí syntetické telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1559c-191">By default, [Metrics Explorer](app-insights-metrics-explorer.md) does not display synthetic telemetry.</span></span>

    <span data-ttu-id="1559c-192">Hello `<Filters>` nastavte výchozí určující vlastnosti hello požadavků.</span><span class="sxs-lookup"><span data-stu-id="1559c-192">hello `<Filters>` set identifying properties of hello requests.</span></span>
* <span data-ttu-id="1559c-193">`UserAgentTelemetryInitializer`aktualizace hello `UserAgent` vlastnost hello `User` kontextu všechny položky telemetrie podle hello `User-Agent` hlavičky protokolu HTTP žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-193">`UserAgentTelemetryInitializer` updates hello `UserAgent` property of hello `User` context of all telemetry items based on hello `User-Agent` HTTP header of hello request.</span></span>
* <span data-ttu-id="1559c-194">`UserTelemetryInitializer`aktualizace hello `Id` a `AcquisitionDate` vlastnosti `User` kontext pro všechny položky telemetrie s hodnotami z hello extrahovat `ai_user` souboru cookie generované kód instrumentace Application Insights JavaScript hello spuštěný v hello prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="1559c-194">`UserTelemetryInitializer` updates hello `Id` and `AcquisitionDate` properties of `User` context for all telemetry items with values extracted from hello `ai_user` cookie generated by hello Application Insights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="1559c-195">`WebTestTelemetryInitializer`Nastaví hello id uživatele, id relace a vlastnosti syntetické zdroje pro požadavky HTTP, která pocházejí z [testy dostupnosti](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="1559c-195">`WebTestTelemetryInitializer` sets hello user id, session id and synthetic source properties for HTTP requests that come from [availability tests](app-insights-monitor-web-app-availability.md).</span></span>
  <span data-ttu-id="1559c-196">Hello `<Filters>` nastavte výchozí určující vlastnosti hello požadavků.</span><span class="sxs-lookup"><span data-stu-id="1559c-196">hello `<Filters>` set identifying properties of hello requests.</span></span>

<span data-ttu-id="1559c-197">Pro aplikace .NET běžící v Service Fabric, můžete zahrnout hello `Microsoft.ApplicationInsights.ServiceFabric` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-197">For .NET applications running in Service Fabric, you can include hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet package.</span></span> <span data-ttu-id="1559c-198">Tento balíček obsahuje `FabricTelemetryInitializer`, který přidává Service Fabric vlastnosti tootelemetry položky.</span><span class="sxs-lookup"><span data-stu-id="1559c-198">This package includes a `FabricTelemetryInitializer`, which adds Service Fabric properties tootelemetry items.</span></span> <span data-ttu-id="1559c-199">Další informace najdete v tématu hello [GitHub stránce](https://go.microsoft.com/fwlink/?linkid=848457) o vlastnostech hello přidal tento balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1559c-199">For more information, see hello [GitHub page](https://go.microsoft.com/fwlink/?linkid=848457) about hello properties added by this NuGet package.</span></span>

## <a name="telemetry-processors-aspnet"></a><span data-ttu-id="1559c-200">Telemetrie procesorů (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1559c-200">Telemetry Processors (ASP.NET)</span></span>
<span data-ttu-id="1559c-201">Telemetrie procesory můžete filtrovat a upravit každou položku telemetrie těsně před odesláním z portálu toohello SDK hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-201">Telemetry processors can filter and modify each telemetry item just before it is sent from hello SDK toohello portal.</span></span>

<span data-ttu-id="1559c-202">Můžete [psát vlastní telemetrii procesory](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="1559c-202">You can [write your own telemetry processors](app-insights-api-filtering-sampling.md#filtering).</span></span>

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a><span data-ttu-id="1559c-203">Procesor telemetrie adaptivního vzorkování (od 2.0.0-beta3)</span><span class="sxs-lookup"><span data-stu-id="1559c-203">Adaptive sampling telemetry processor (from 2.0.0-beta3)</span></span>
<span data-ttu-id="1559c-204">Tato možnost je ve výchozím nastavení zapnutá.</span><span class="sxs-lookup"><span data-stu-id="1559c-204">This is enabled by default.</span></span> <span data-ttu-id="1559c-205">Pokud vaše aplikace odešle velké množství telemetrická data, některé jeho tohoto procesoru odebere.</span><span class="sxs-lookup"><span data-stu-id="1559c-205">If your app sends a lot of telemetry, this processor removes some of it.</span></span>

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="1559c-206">Parametr Hello poskytuje hello cíl, který hello algoritmus pokusí tooachieve.</span><span class="sxs-lookup"><span data-stu-id="1559c-206">hello parameter provides hello target that hello algorithm tries tooachieve.</span></span> <span data-ttu-id="1559c-207">Nezávisle, každou instanci hello SDK funguje, takže pokud je server clusteru s podporou několik počítačů, skutečný objem hello telemetrie se násobí odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1559c-207">Each instance of hello SDK works independently, so if your server is a cluster of several machines, hello actual volume of telemetry will be multiplied accordingly.</span></span>

<span data-ttu-id="1559c-208">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="1559c-208">[Learn more about sampling](app-insights-sampling.md).</span></span>

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a><span data-ttu-id="1559c-209">Procesor-míry vzorkování telemetrie (od 2.0.0-beta1)</span><span class="sxs-lookup"><span data-stu-id="1559c-209">Fixed-rate sampling telemetry processor (from 2.0.0-beta1)</span></span>
<span data-ttu-id="1559c-210">Je také standardní [vzorkování procesoru telemetrie](app-insights-api-filtering-sampling.md) (z 2.0.1):</span><span class="sxs-lookup"><span data-stu-id="1559c-210">There is also a standard [sampling telemetry processor](app-insights-api-filtering-sampling.md) (from 2.0.1):</span></span>

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a><span data-ttu-id="1559c-211">Parametry kanálu (Java)</span><span class="sxs-lookup"><span data-stu-id="1559c-211">Channel parameters (Java)</span></span>
<span data-ttu-id="1559c-212">Tyto parametry vliv na způsob hello sady Java SDK měli uložit a vyprázdnění hello telemetrická data, která shromažďuje.</span><span class="sxs-lookup"><span data-stu-id="1559c-212">These parameters affect how hello Java SDK should store and flush hello telemetry data that it collects.</span></span>

#### <a name="maxtelemetrybuffercapacity"></a><span data-ttu-id="1559c-213">MaxTelemetryBufferCapacity</span><span class="sxs-lookup"><span data-stu-id="1559c-213">MaxTelemetryBufferCapacity</span></span>
<span data-ttu-id="1559c-214">Hello počet položek telemetrie, které mohou být uloženy v úložišti hello SDK v paměti.</span><span class="sxs-lookup"><span data-stu-id="1559c-214">hello number of telemetry items that can be stored in hello SDK's in-memory storage.</span></span> <span data-ttu-id="1559c-215">Při dosažení tohoto počtu vyprázdní vyrovnávací paměť telemetrie hello – tedy hello telemetrie položky odesláním toohello Application Insights serveru.</span><span class="sxs-lookup"><span data-stu-id="1559c-215">When this number is reached, hello telemetry buffer is flushed - that is, hello telemetry items are sent toohello Application Insights server.</span></span>

* <span data-ttu-id="1559c-216">Min: 1</span><span class="sxs-lookup"><span data-stu-id="1559c-216">Min: 1</span></span>
* <span data-ttu-id="1559c-217">Maximální počet: 1 000</span><span class="sxs-lookup"><span data-stu-id="1559c-217">Max: 1000</span></span>
* <span data-ttu-id="1559c-218">Výchozí: 500</span><span class="sxs-lookup"><span data-stu-id="1559c-218">Default: 500</span></span>

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a><span data-ttu-id="1559c-219">FlushIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="1559c-219">FlushIntervalInSeconds</span></span>
<span data-ttu-id="1559c-220">Určuje, jak často hello data, která je uložená v úložišti hello v paměti by měla být vyprázdněn (odeslané tooApplication Insights).</span><span class="sxs-lookup"><span data-stu-id="1559c-220">Determines how often hello data that is stored in hello in-memory storage should be flushed (sent tooApplication Insights).</span></span>

* <span data-ttu-id="1559c-221">Min: 1</span><span class="sxs-lookup"><span data-stu-id="1559c-221">Min: 1</span></span>
* <span data-ttu-id="1559c-222">Maximální počet: 300</span><span class="sxs-lookup"><span data-stu-id="1559c-222">Max: 300</span></span>
* <span data-ttu-id="1559c-223">Výchozí: 5</span><span class="sxs-lookup"><span data-stu-id="1559c-223">Default: 5</span></span>

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a><span data-ttu-id="1559c-224">MaxTransmissionStorageCapacityInMB</span><span class="sxs-lookup"><span data-stu-id="1559c-224">MaxTransmissionStorageCapacityInMB</span></span>
<span data-ttu-id="1559c-225">Určuje hello maximální velikost v Megabajtech, která je vymezena toohello trvalé úložiště na místním disku hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-225">Determines hello maximum size in MB that is allotted toohello persistent storage on hello local disk.</span></span> <span data-ttu-id="1559c-226">Toto úložiště se používá pro zachování telemetrie položky, které toobe přenášených toohello Application Insights koncového bodu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="1559c-226">This storage is used for persisting telemetry items that failed toobe transmitted toohello Application Insights endpoint.</span></span> <span data-ttu-id="1559c-227">Pokud velikost úložiště hello byly splněny, nové položky telemetrie se zahodí.</span><span class="sxs-lookup"><span data-stu-id="1559c-227">When hello storage size has been met, new telemetry items will be discarded.</span></span>

* <span data-ttu-id="1559c-228">Min: 1</span><span class="sxs-lookup"><span data-stu-id="1559c-228">Min: 1</span></span>
* <span data-ttu-id="1559c-229">Maximální počet: 100</span><span class="sxs-lookup"><span data-stu-id="1559c-229">Max: 100</span></span>
* <span data-ttu-id="1559c-230">Výchozí: 10</span><span class="sxs-lookup"><span data-stu-id="1559c-230">Default: 10</span></span>

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a><span data-ttu-id="1559c-231">InstrumentationKey</span><span class="sxs-lookup"><span data-stu-id="1559c-231">InstrumentationKey</span></span>
<span data-ttu-id="1559c-232">Určuje, ve kterém se zobrazí data prostředek Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="1559c-232">This determines hello Application Insights resource in which your data appears.</span></span> <span data-ttu-id="1559c-233">Obvykle vytvoříte prostředek samostatné samostatné klíčem pro každý z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="1559c-233">Typically you create a separate resource, with a separate key, for each of your applications.</span></span>

<span data-ttu-id="1559c-234">Pokud chcete klíč hello tooset dynamicky – například, pokud chcete výsledky toosend z vašich prostředků toodifferent aplikace – můžete vynechat hello klíč z hello konfiguračního souboru a nastavení v kódu.</span><span class="sxs-lookup"><span data-stu-id="1559c-234">If you want tooset hello key dynamically - for example if you want toosend results from your application toodifferent resources - you can omit hello key from hello configuration file, and set it in code instead.</span></span>

<span data-ttu-id="1559c-235">klíč hello tooset pro všechny instance TelemetryClient, včetně standardní telemetrie moduly hello v nastavit TelemetryConfiguration.Active.</span><span class="sxs-lookup"><span data-stu-id="1559c-235">tooset hello key for all instances of TelemetryClient, including standard telemetry modules, set hello key in TelemetryConfiguration.Active.</span></span> <span data-ttu-id="1559c-236">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1559c-236">Do this in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

<span data-ttu-id="1559c-237">Pokud chcete toosend konkrétní sada události tooa různých prostředků, můžete nastavit hello klíč pro konkrétní TelemetryClient:</span><span class="sxs-lookup"><span data-stu-id="1559c-237">If you just want toosend a specific set of events tooa different resource, you can set hello key for a specific TelemetryClient:</span></span>

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

<span data-ttu-id="1559c-238">tooget nový klíč, [vytvořte nový prostředek Application Insights portálu hello][new].</span><span class="sxs-lookup"><span data-stu-id="1559c-238">tooget a new key, [create a new resource in hello Application Insights portal][new].</span></span>

## <a name="next-steps"></a><span data-ttu-id="1559c-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1559c-239">Next steps</span></span>
<span data-ttu-id="1559c-240">[Další informace o hello rozhraní API][api].</span><span class="sxs-lookup"><span data-stu-id="1559c-240">[Learn more about hello API][api].</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
