---
title: Odkaz na soubor ApplicationInsights.config - Azure | Microsoft Docs
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
ms.openlocfilehash: 7737f47d4181b5e920434f3a5372991efb58f63e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a><span data-ttu-id="0648d-103">Konfigurace sady Application Insights SDK pomocí souboru ApplicationInsights.config nebo .xml</span><span class="sxs-lookup"><span data-stu-id="0648d-103">Configuring the Application Insights SDK with ApplicationInsights.config or .xml</span></span>
<span data-ttu-id="0648d-104">Application Insights .NET SDK se skládá z počet balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-104">The Application Insights .NET SDK consists of a number of NuGet packages.</span></span> <span data-ttu-id="0648d-105">[Základní balíček](http://www.nuget.org/packages/Microsoft.ApplicationInsights) poskytuje rozhraní API pro odesílání telemetrie Application insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-105">The [core package](http://www.nuget.org/packages/Microsoft.ApplicationInsights) provides the API for sending telemetry to the Application Insights.</span></span> <span data-ttu-id="0648d-106">[Další balíčky](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) poskytují telemetrie *moduly* a *inicializátory* pro automaticky sledování telemetrie z vaší aplikace a jeho kontextu.</span><span class="sxs-lookup"><span data-stu-id="0648d-106">[Additional packages](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) provide telemetry *modules* and *initializers* for automatically tracking telemetry from your application and its context.</span></span> <span data-ttu-id="0648d-107">Úpravou konfiguračního souboru, můžete povolit nebo zakázat telemetrii moduly a inicializátory a nastavit parametry pro některé z nich.</span><span class="sxs-lookup"><span data-stu-id="0648d-107">By adjusting the configuration file, you can enable or disable telemetry modules and initializers, and set parameters for some of them.</span></span>

<span data-ttu-id="0648d-108">Konfigurační soubor je s názvem `ApplicationInsights.config` nebo `ApplicationInsights.xml`, v závislosti na typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-108">The configuration file is named `ApplicationInsights.config` or `ApplicationInsights.xml`, depending on the type of your application.</span></span> <span data-ttu-id="0648d-109">Je automaticky přidán do projektu když jste [nainstalujte většina verze sady SDK][start].</span><span class="sxs-lookup"><span data-stu-id="0648d-109">It is automatically added to your project when you [install most versions of the SDK][start].</span></span> <span data-ttu-id="0648d-110">Je také přidán do webové aplikace pomocí [monitorování stavu na serveru se službou IIS][redfield], nebo když vyberete Appplication Insights [rozšíření pro webové stránky Azure, nebo virtuální počítač](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="0648d-110">It is also added to a web app by [Status Monitor on an IIS server][redfield], or when you select the Appplication Insights [extension for an Azure website or VM](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="0648d-111">Není k dispozici soubor ekvivalentní k řízení [SDK na webové stránce][client].</span><span class="sxs-lookup"><span data-stu-id="0648d-111">There isn't an equivalent file to control the [SDK in a web page][client].</span></span>

<span data-ttu-id="0648d-112">Tento dokument popisuje oddílů, které se zobrazí v konfiguraci souboru, jak budou řídit komponenty sady SDK, a které balíčky NuGet načíst těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="0648d-112">This document describes the sections you see in the configuration file, how they control the components of the SDK, and which NuGet packages load those components.</span></span>

## <a name="telemetry-modules-aspnet"></a><span data-ttu-id="0648d-113">Telemetrie moduly (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0648d-113">Telemetry Modules (ASP.NET)</span></span>
<span data-ttu-id="0648d-114">Každý modul telemetrie shromažďuje konkrétní typ dat a používá základní rozhraní API odeslat data.</span><span class="sxs-lookup"><span data-stu-id="0648d-114">Each telemetry module collects a specific type of data and uses the core API to send the data.</span></span> <span data-ttu-id="0648d-115">Moduly jsou nainstalovány jiné balíčky NuGet, které také přidat do souboru .config požadovaných řádků.</span><span class="sxs-lookup"><span data-stu-id="0648d-115">The modules are installed by different NuGet packages, which also add the required lines to the .config file.</span></span>

<span data-ttu-id="0648d-116">V souboru konfigurace pro každý modul není uzlu.</span><span class="sxs-lookup"><span data-stu-id="0648d-116">There's a node in the configuration file for each module.</span></span> <span data-ttu-id="0648d-117">Zakázat modul, odstraňte uzlu nebo ho nastavte jako komentář.</span><span class="sxs-lookup"><span data-stu-id="0648d-117">To disable a module, delete the node or comment it out.</span></span>

### <a name="dependency-tracking"></a><span data-ttu-id="0648d-118">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="0648d-118">Dependency Tracking</span></span>
<span data-ttu-id="0648d-119">[Závislost sledování](app-insights-asp-net-dependencies.md) shromažďuje telemetrická data o volání aplikace provede databáze a externí služby a databáze.</span><span class="sxs-lookup"><span data-stu-id="0648d-119">[Dependency tracking](app-insights-asp-net-dependencies.md) collects telemetry about calls your app makes to databases and external services and databases.</span></span> <span data-ttu-id="0648d-120">Chcete-li povolit tento modul pro práci v serveru se službou IIS, je potřeba [nainstalujte monitorování stavu][redfield].</span><span class="sxs-lookup"><span data-stu-id="0648d-120">To allow this module to work in an IIS server, you need to [install Status Monitor][redfield].</span></span> <span data-ttu-id="0648d-121">Pro použití ve službě Azure web apps nebo virtuálních počítačů, [vyberte rozšíření Application Insights](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="0648d-121">To use it in Azure web apps or VMs, [select the Application Insights extension](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="0648d-122">Můžete taky napsat vlastní závislost sledování kódu pomocí [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="0648d-122">You can also write your own dependency tracking code using the [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* <span data-ttu-id="0648d-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.</span></span>

### <a name="performance-collector"></a><span data-ttu-id="0648d-124">Kolekce výkonu</span><span class="sxs-lookup"><span data-stu-id="0648d-124">Performance collector</span></span>
<span data-ttu-id="0648d-125">[Shromažďuje čítače výkonu systému](app-insights-performance-counters.md) například CPU, paměť a síť načíst z instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="0648d-125">[Collects system performance counters](app-insights-performance-counters.md) such as CPU, memory and network load from IIS installations.</span></span> <span data-ttu-id="0648d-126">Můžete zadat čítače, které mají shromažďovat, včetně čítačů výkonu, které jste nastavili sami.</span><span class="sxs-lookup"><span data-stu-id="0648d-126">You can specify which counters to collect, including performance counters you have set up yourself.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* <span data-ttu-id="0648d-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.</span></span>

### <a name="application-insights-diagnostics-telemetry"></a><span data-ttu-id="0648d-128">Telemetrii Diagnostics Application Insights</span><span class="sxs-lookup"><span data-stu-id="0648d-128">Application Insights Diagnostics Telemetry</span></span>
<span data-ttu-id="0648d-129">`DiagnosticsTelemetryModule` Sestavy chyb v samotný kód instrumentace Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-129">The `DiagnosticsTelemetryModule` reports errors in the Application Insights instrumentation code itself.</span></span> <span data-ttu-id="0648d-130">Například pokud kód nelze získat přístup k čítače výkonu nebo `ITelemetryInitializer` vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="0648d-130">For example, if the code cannot access performance counters or if an `ITelemetryInitializer` throws an exception.</span></span> <span data-ttu-id="0648d-131">Trasování telemetrie sleduje tento modul se zobrazí v [diagnostické vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="0648d-131">Trace telemetry tracked by this module appears in the [Diagnostic Search][diagnostic].</span></span> <span data-ttu-id="0648d-132">Odešle dc.services.vsallin.net diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="0648d-132">Sends diagnostic data to dc.services.vsallin.net.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* <span data-ttu-id="0648d-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="0648d-134">Pokud máte pouze instalaci tohoto balíčku, není soubor ApplicationInsights.config vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="0648d-134">If you only install this package, the ApplicationInsights.config file is not automatically created.</span></span>

### <a name="developer-mode"></a><span data-ttu-id="0648d-135">Režim vývojáře</span><span class="sxs-lookup"><span data-stu-id="0648d-135">Developer Mode</span></span>
<span data-ttu-id="0648d-136">`DeveloperModeWithDebuggerAttachedTelemetryModule`Vynutí Application Insights `TelemetryChannel` k odesílání dat okamžitě, jeden telemetrie položky v době, kdy je připojen ladicí program na proces aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-136">`DeveloperModeWithDebuggerAttachedTelemetryModule` forces the Application Insights `TelemetryChannel` to send data immediately, one telemetry item at a time, when a debugger is attached to the application process.</span></span> <span data-ttu-id="0648d-137">Tím se zkracuje dobu mezi odhalením Pokud vaše aplikace sleduje telemetrie a pokud se zobrazí na portálu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-137">This reduces the amount of time between the moment when your application tracks telemetry and when it appears on the Application Insights portal.</span></span> <span data-ttu-id="0648d-138">Způsobuje významné režijní náklady v procesoru a šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="0648d-138">It causes significant overhead in CPU and network bandwidth.</span></span>

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* <span data-ttu-id="0648d-139">[Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="0648d-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package</span></span>

### <a name="web-request-tracking"></a><span data-ttu-id="0648d-140">Sledování webové žádosti</span><span class="sxs-lookup"><span data-stu-id="0648d-140">Web Request Tracking</span></span>
<span data-ttu-id="0648d-141">Sestavy [čas a výsledek kód odpovědi](app-insights-asp-net.md) požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="0648d-141">Reports the [response time and result code](app-insights-asp-net.md) of HTTP requests.</span></span>

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* <span data-ttu-id="0648d-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="0648d-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>

### <a name="exception-tracking"></a><span data-ttu-id="0648d-143">Sledování výjimek</span><span class="sxs-lookup"><span data-stu-id="0648d-143">Exception tracking</span></span>
<span data-ttu-id="0648d-144">`ExceptionTrackingTelemetryModule`sleduje neošetřených výjimek ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0648d-144">`ExceptionTrackingTelemetryModule` tracks unhandled exceptions in your web app.</span></span> <span data-ttu-id="0648d-145">V tématu [chyby a výjimky][exceptions].</span><span class="sxs-lookup"><span data-stu-id="0648d-145">See [Failures and exceptions][exceptions].</span></span>

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* <span data-ttu-id="0648d-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="0648d-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>
* <span data-ttu-id="0648d-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-sleduje [nepozorovaná úloh výjimky](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span><span class="sxs-lookup"><span data-stu-id="0648d-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` - tracks [unobserved task exceptions](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span></span>
* <span data-ttu-id="0648d-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-sleduje neošetřené výjimky pro role pracovního procesu, služby systému windows a konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` - tracks unhandled exceptions for worker roles, windows services, and console applications.</span></span>
* <span data-ttu-id="0648d-149">[Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.</span></span>

### <a name="eventsource-tracking"></a><span data-ttu-id="0648d-150">EventSource sledování</span><span class="sxs-lookup"><span data-stu-id="0648d-150">EventSource Tracking</span></span>
<span data-ttu-id="0648d-151">`EventSourceTelemetryModule`Umožňuje nakonfigurovat událostí EventSource k odeslání do Application Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="0648d-151">`EventSourceTelemetryModule` allows you to configure EventSource events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="0648d-152">Informace o sledování událostí EventSource najdete v tématu [pomocí událostí EventSource](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span><span class="sxs-lookup"><span data-stu-id="0648d-152">For information on tracking EventSource events, see [Using EventSource Events](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span></span>

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [<span data-ttu-id="0648d-153">Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="0648d-153">Microsoft.ApplicationInsights.EventSourceListener</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a><span data-ttu-id="0648d-154">Sledování událostí trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="0648d-154">ETW Event Tracking</span></span>
<span data-ttu-id="0648d-155">`EtwCollectorTelemetryModule`Umožňuje nakonfigurovat události od zprostředkovatelů trasování událostí pro Windows k odeslání do Application Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="0648d-155">`EtwCollectorTelemetryModule` allows you to configure events from ETW providers to be sent to Application Insights as traces.</span></span> <span data-ttu-id="0648d-156">Informace o sledování události trasování událostí pro Windows najdete v tématu [události trasování událostí pro Windows pomocí](app-insights-asp-net-trace-logs.md#using-etw-events).</span><span class="sxs-lookup"><span data-stu-id="0648d-156">For information on tracking ETW events, see [Using ETW Events](app-insights-asp-net-trace-logs.md#using-etw-events).</span></span>

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [<span data-ttu-id="0648d-157">Microsoft.ApplicationInsights.EtwCollector</span><span class="sxs-lookup"><span data-stu-id="0648d-157">Microsoft.ApplicationInsights.EtwCollector</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a><span data-ttu-id="0648d-158">Microsoft.ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="0648d-158">Microsoft.ApplicationInsights</span></span>
<span data-ttu-id="0648d-159">Balíček Microsoft.ApplicationInsights poskytuje [základní rozhraní API](https://msdn.microsoft.com/library/mt420197.aspx) sady SDK.</span><span class="sxs-lookup"><span data-stu-id="0648d-159">The Microsoft.ApplicationInsights package provides the [core API](https://msdn.microsoft.com/library/mt420197.aspx) of the SDK.</span></span> <span data-ttu-id="0648d-160">Další moduly telemetrie použít, a můžete také [použijte jej k definování vlastní telemetrii](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0648d-160">The other telemetry modules use this, and you can also [use it to define your own telemetry](app-insights-api-custom-events-metrics.md).</span></span>

* <span data-ttu-id="0648d-161">Žádný záznam v souboru ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="0648d-161">No entry in ApplicationInsights.config.</span></span>
* <span data-ttu-id="0648d-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="0648d-163">Pokud instalujete právě tento NuGet, vygeneruje se žádný soubor .config.</span><span class="sxs-lookup"><span data-stu-id="0648d-163">If you just install this NuGet, no .config file is generated.</span></span>

## <a name="telemetry-channel"></a><span data-ttu-id="0648d-164">Kanál telemetrie</span><span class="sxs-lookup"><span data-stu-id="0648d-164">Telemetry Channel</span></span>
<span data-ttu-id="0648d-165">Kanál telemetrie spravuje ukládání do vyrovnávací paměti a přenos telemetrie do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-165">The telemetry channel manages buffering and transmission of telemetry to the Application Insights service.</span></span>

* <span data-ttu-id="0648d-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`je výchozím kanálu pro služby.</span><span class="sxs-lookup"><span data-stu-id="0648d-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is the default channel for services.</span></span> <span data-ttu-id="0648d-167">Data ukládány v paměti.</span><span class="sxs-lookup"><span data-stu-id="0648d-167">It buffers data in memory.</span></span>
* <span data-ttu-id="0648d-168">`Microsoft.ApplicationInsights.PersistenceChannel`představuje alternativu pro konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-168">`Microsoft.ApplicationInsights.PersistenceChannel` is an alternative for console applications.</span></span> <span data-ttu-id="0648d-169">Žádná unflushed data ho můžete uložit do trvalého úložiště, když vaše aplikace zavře a odešle ho při spuštění aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="0648d-169">It can save any unflushed data to persistent storage when your app closes down, and will send it when the app starts again.</span></span>

## <a name="telemetry-initializers-aspnet"></a><span data-ttu-id="0648d-170">Inicializátory telemetrie (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0648d-170">Telemetry Initializers (ASP.NET)</span></span>
<span data-ttu-id="0648d-171">Inicializátory telemetrie nastavit kontext vlastnosti, které se odesílají společně s každou položkou telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0648d-171">Telemetry initializers set context properties that are sent along with every item of telemetry.</span></span>

<span data-ttu-id="0648d-172">Můžete [zápisu vlastní inicializátory](app-insights-api-filtering-sampling.md#add-properties) pro nastavení vlastností kontextu.</span><span class="sxs-lookup"><span data-stu-id="0648d-172">You can [write your own initializers](app-insights-api-filtering-sampling.md#add-properties) to set context properties.</span></span>

<span data-ttu-id="0648d-173">Standardní inicializátory jsou nastavené buď webové nebo Windows Server NuGet balíčků:</span><span class="sxs-lookup"><span data-stu-id="0648d-173">The standard initializers are all set either by the Web or WindowsServer NuGet packages:</span></span>

* <span data-ttu-id="0648d-174">`AccountIdTelemetryInitializer`Nastaví vlastnost ID účtu.</span><span class="sxs-lookup"><span data-stu-id="0648d-174">`AccountIdTelemetryInitializer` sets the AccountId property.</span></span>
* <span data-ttu-id="0648d-175">`AuthenticatedUserIdTelemetryInitializer`Nastaví vlastnost AuthenticatedUserId jako sada SDK jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0648d-175">`AuthenticatedUserIdTelemetryInitializer` sets the AuthenticatedUserId property as set by the JavaScript SDK.</span></span>
* <span data-ttu-id="0648d-176">`AzureRoleEnvironmentTelemetryInitializer`aktualizace `RoleName` a `RoleInstance` vlastnosti `Device` kontext pro všechny položky telemetrie s informacemi, které jsou extrahovány z Azure běhové prostředí.</span><span class="sxs-lookup"><span data-stu-id="0648d-176">`AzureRoleEnvironmentTelemetryInitializer` updates the `RoleName` and `RoleInstance` properties of the `Device` context for all telemetry items with information extracted from the Azure runtime environment.</span></span>
* <span data-ttu-id="0648d-177">`BuildInfoConfigComponentVersionTelemetryInitializer`aktualizace `Version` vlastnost `Component` kontext pro všechny položky telemetrie s hodnotou z extrahovat `BuildInfo.config` souboru vytvořeného pomocí MS Build.</span><span class="sxs-lookup"><span data-stu-id="0648d-177">`BuildInfoConfigComponentVersionTelemetryInitializer` updates the `Version` property of the `Component` context for all telemetry items with the value extracted from the `BuildInfo.config` file produced by MS Build.</span></span>
* <span data-ttu-id="0648d-178">`ClientIpHeaderTelemetryInitializer`aktualizace `Ip` vlastnost `Location` na základě kontextu všechny položky telemetrii `X-Forwarded-For` hlavičky protokolu HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="0648d-178">`ClientIpHeaderTelemetryInitializer` updates `Ip` property of the `Location` context of all telemetry items based on the `X-Forwarded-For` HTTP header of the request.</span></span>
* <span data-ttu-id="0648d-179">`DeviceTelemetryInitializer`aktualizuje následující vlastnosti `Device` kontext pro všechny položky telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0648d-179">`DeviceTelemetryInitializer` updates the following properties of the `Device` context for all telemetry items.</span></span>
  * <span data-ttu-id="0648d-180">`Type`je nastavena na "Počítač"</span><span class="sxs-lookup"><span data-stu-id="0648d-180">`Type` is set to "PC"</span></span>
  * <span data-ttu-id="0648d-181">`Id`je nastavena na název domény počítače, kde je spuštěna webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-181">`Id` is set to the domain name of the computer where the web application is running.</span></span>
  * <span data-ttu-id="0648d-182">`OemName`je nastaven na hodnotu z extrahovat `Win32_ComputerSystem.Manufacturer` pole pomocí služby WMI.</span><span class="sxs-lookup"><span data-stu-id="0648d-182">`OemName` is set to the value extracted from the `Win32_ComputerSystem.Manufacturer` field using WMI.</span></span>
  * <span data-ttu-id="0648d-183">`Model`je nastaven na hodnotu z extrahovat `Win32_ComputerSystem.Model` pole pomocí služby WMI.</span><span class="sxs-lookup"><span data-stu-id="0648d-183">`Model` is set to the value extracted from the `Win32_ComputerSystem.Model` field using WMI.</span></span>
  * <span data-ttu-id="0648d-184">`NetworkType`je nastaven na hodnotu z extrahovat `NetworkInterface`.</span><span class="sxs-lookup"><span data-stu-id="0648d-184">`NetworkType` is set to the value extracted from the `NetworkInterface`.</span></span>
  * <span data-ttu-id="0648d-185">`Language`je nastavena na název `CurrentCulture`.</span><span class="sxs-lookup"><span data-stu-id="0648d-185">`Language` is set to the name of the `CurrentCulture`.</span></span>
* <span data-ttu-id="0648d-186">`DomainNameRoleInstanceTelemetryInitializer`aktualizace `RoleInstance` vlastnost `Device` kontext pro všechny položky telemetrie s názvem domény počítače, kde je spuštěna webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="0648d-186">`DomainNameRoleInstanceTelemetryInitializer` updates the `RoleInstance` property of the `Device` context for all telemetry items with the domain name of the computer where the web application is running.</span></span>
* <span data-ttu-id="0648d-187">`OperationNameTelemetryInitializer`aktualizace `Name` vlastnost `RequestTelemetry` a `Name` vlastnost `Operation` kontextu všechny položky telemetrie podle metoda HTTP, jakož i názvy ASP.NET MVC jsou řadič MVC a akce vyvolaná při zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="0648d-187">`OperationNameTelemetryInitializer` updates the `Name` property of the `RequestTelemetry` and the `Name` property of the `Operation` context of all telemetry items based on the HTTP method, as well as names of ASP.NET MVC controller and action invoked to process the request.</span></span>
* <span data-ttu-id="0648d-188">`OperationIdTelemetryInitializer`nebo `OperationCorrelationTelemetryInitializer` aktualizace `Operation.Id` vlastností kontextu všechny položky telemetrie sledovat při zpracování žádosti o se automaticky generované `RequestTelemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="0648d-188">`OperationIdTelemetryInitializer` or `OperationCorrelationTelemetryInitializer` updates the `Operation.Id` context property of all telemetry items tracked while handling a request with the automatically generated `RequestTelemetry.Id`.</span></span>
* <span data-ttu-id="0648d-189">`SessionTelemetryInitializer`aktualizace `Id` vlastnost `Session` kontext pro všechny položky telemetrie s hodnotou z extrahovat `ai_session` generované kód ApplicationInsights JavaScript instrumentace spouštění v prohlížeči uživatele soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="0648d-189">`SessionTelemetryInitializer` updates the `Id` property of the `Session` context for all telemetry items with value extracted from the `ai_session` cookie generated by the ApplicationInsights JavaScript instrumentation code running in the user's browser.</span></span>
* <span data-ttu-id="0648d-190">`SyntheticTelemetryInitializer`nebo `SyntheticUserAgentTelemetryInitializer` aktualizace `User`, `Session` a `Operation` kontexty vlastnosti všech položek telemetrie sledovat při zpracování požadavku z syntetické zdroje, například dostupnosti testů nebo vyhledávání modul robota.</span><span class="sxs-lookup"><span data-stu-id="0648d-190">`SyntheticTelemetryInitializer` or `SyntheticUserAgentTelemetryInitializer` updates the `User`, `Session` and `Operation` contexts properties of all telemetry items tracked when handling a request from a synthetic source, such as an availability test or search engine bot.</span></span> <span data-ttu-id="0648d-191">Ve výchozím nastavení [Průzkumníku metrik](app-insights-metrics-explorer.md) nezobrazí syntetické telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0648d-191">By default, [Metrics Explorer](app-insights-metrics-explorer.md) does not display synthetic telemetry.</span></span>

    <span data-ttu-id="0648d-192">`<Filters>` Nastavte výchozí určující vlastnosti žádosti.</span><span class="sxs-lookup"><span data-stu-id="0648d-192">The `<Filters>` set identifying properties of the requests.</span></span>
* <span data-ttu-id="0648d-193">`UserAgentTelemetryInitializer`aktualizace `UserAgent` vlastnost `User` na základě kontextu všechny položky telemetrii `User-Agent` hlavičky protokolu HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="0648d-193">`UserAgentTelemetryInitializer` updates the `UserAgent` property of the `User` context of all telemetry items based on the `User-Agent` HTTP header of the request.</span></span>
* <span data-ttu-id="0648d-194">`UserTelemetryInitializer`aktualizace `Id` a `AcquisitionDate` vlastnosti `User` kontext pro všechny položky telemetrie se extrahují z hodnoty `ai_user` souboru cookie generované kód instrumentace Application Insights JavaScript, který je spuštěný v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="0648d-194">`UserTelemetryInitializer` updates the `Id` and `AcquisitionDate` properties of `User` context for all telemetry items with values extracted from the `ai_user` cookie generated by the Application Insights JavaScript instrumentation code running in the user's browser.</span></span>
* <span data-ttu-id="0648d-195">`WebTestTelemetryInitializer`Nastaví id uživatele, id relace a vlastnosti syntetické zdroje pro požadavky HTTP, která pocházejí z [testy dostupnosti](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="0648d-195">`WebTestTelemetryInitializer` sets the user id, session id and synthetic source properties for HTTP requests that come from [availability tests](app-insights-monitor-web-app-availability.md).</span></span>
  <span data-ttu-id="0648d-196">`<Filters>` Nastavte výchozí určující vlastnosti žádosti.</span><span class="sxs-lookup"><span data-stu-id="0648d-196">The `<Filters>` set identifying properties of the requests.</span></span>

<span data-ttu-id="0648d-197">Pro aplikace .NET běžící v Service Fabric, můžete zahrnout `Microsoft.ApplicationInsights.ServiceFabric` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-197">For .NET applications running in Service Fabric, you can include the `Microsoft.ApplicationInsights.ServiceFabric` NuGet package.</span></span> <span data-ttu-id="0648d-198">Tento balíček obsahuje `FabricTelemetryInitializer`, přidává vlastnosti Service Fabric položkám telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0648d-198">This package includes a `FabricTelemetryInitializer`, which adds Service Fabric properties to telemetry items.</span></span> <span data-ttu-id="0648d-199">Další informace najdete v tématu [GitHub stránce](https://go.microsoft.com/fwlink/?linkid=848457) o vlastnostech přidal tento balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0648d-199">For more information, see the [GitHub page](https://go.microsoft.com/fwlink/?linkid=848457) about the properties added by this NuGet package.</span></span>

## <a name="telemetry-processors-aspnet"></a><span data-ttu-id="0648d-200">Telemetrie procesorů (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0648d-200">Telemetry Processors (ASP.NET)</span></span>
<span data-ttu-id="0648d-201">Telemetrie procesory můžete filtrovat a upravit každou položku telemetrie těsně před odesláním ze sady SDK k portálu.</span><span class="sxs-lookup"><span data-stu-id="0648d-201">Telemetry processors can filter and modify each telemetry item just before it is sent from the SDK to the portal.</span></span>

<span data-ttu-id="0648d-202">Můžete [psát vlastní telemetrii procesory](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="0648d-202">You can [write your own telemetry processors](app-insights-api-filtering-sampling.md#filtering).</span></span>

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a><span data-ttu-id="0648d-203">Procesor telemetrie adaptivního vzorkování (od 2.0.0-beta3)</span><span class="sxs-lookup"><span data-stu-id="0648d-203">Adaptive sampling telemetry processor (from 2.0.0-beta3)</span></span>
<span data-ttu-id="0648d-204">Tato možnost je ve výchozím nastavení zapnutá.</span><span class="sxs-lookup"><span data-stu-id="0648d-204">This is enabled by default.</span></span> <span data-ttu-id="0648d-205">Pokud vaše aplikace odešle velké množství telemetrická data, některé jeho tohoto procesoru odebere.</span><span class="sxs-lookup"><span data-stu-id="0648d-205">If your app sends a lot of telemetry, this processor removes some of it.</span></span>

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="0648d-206">Parametr poskytuje cíl, který algoritmus se pokusí dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="0648d-206">The parameter provides the target that the algorithm tries to achieve.</span></span> <span data-ttu-id="0648d-207">Každá instance sady SDK funguje nezávisle, takže pokud je server clusteru s podporou několik počítačů, skutečný objem telemetrie se násobí odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0648d-207">Each instance of the SDK works independently, so if your server is a cluster of several machines, the actual volume of telemetry will be multiplied accordingly.</span></span>

<span data-ttu-id="0648d-208">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="0648d-208">[Learn more about sampling](app-insights-sampling.md).</span></span>

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a><span data-ttu-id="0648d-209">Procesor-míry vzorkování telemetrie (od 2.0.0-beta1)</span><span class="sxs-lookup"><span data-stu-id="0648d-209">Fixed-rate sampling telemetry processor (from 2.0.0-beta1)</span></span>
<span data-ttu-id="0648d-210">Je také standardní [vzorkování procesoru telemetrie](app-insights-api-filtering-sampling.md) (z 2.0.1):</span><span class="sxs-lookup"><span data-stu-id="0648d-210">There is also a standard [sampling telemetry processor](app-insights-api-filtering-sampling.md) (from 2.0.1):</span></span>

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a><span data-ttu-id="0648d-211">Parametry kanálu (Java)</span><span class="sxs-lookup"><span data-stu-id="0648d-211">Channel parameters (Java)</span></span>
<span data-ttu-id="0648d-212">Tyto parametry vliv na způsob sady Java SDK měli uložit a vyprázdnit telemetrická data, která shromažďuje.</span><span class="sxs-lookup"><span data-stu-id="0648d-212">These parameters affect how the Java SDK should store and flush the telemetry data that it collects.</span></span>

#### <a name="maxtelemetrybuffercapacity"></a><span data-ttu-id="0648d-213">MaxTelemetryBufferCapacity</span><span class="sxs-lookup"><span data-stu-id="0648d-213">MaxTelemetryBufferCapacity</span></span>
<span data-ttu-id="0648d-214">Počet položek telemetrie, které mohou být uloženy v úložišti sady SDK v paměti.</span><span class="sxs-lookup"><span data-stu-id="0648d-214">The number of telemetry items that can be stored in the SDK's in-memory storage.</span></span> <span data-ttu-id="0648d-215">Při dosažení tohoto počtu vyprázdní vyrovnávací paměť telemetrie – to znamená, položky telemetrie posílají se na serveru Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-215">When this number is reached, the telemetry buffer is flushed - that is, the telemetry items are sent to the Application Insights server.</span></span>

* <span data-ttu-id="0648d-216">Min: 1</span><span class="sxs-lookup"><span data-stu-id="0648d-216">Min: 1</span></span>
* <span data-ttu-id="0648d-217">Maximální počet: 1 000</span><span class="sxs-lookup"><span data-stu-id="0648d-217">Max: 1000</span></span>
* <span data-ttu-id="0648d-218">Výchozí: 500</span><span class="sxs-lookup"><span data-stu-id="0648d-218">Default: 500</span></span>

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a><span data-ttu-id="0648d-219">FlushIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="0648d-219">FlushIntervalInSeconds</span></span>
<span data-ttu-id="0648d-220">Určuje, jak často mají být data, která je uložená v úložišti v paměti vyprázdněny (odeslané do služby Application Insights).</span><span class="sxs-lookup"><span data-stu-id="0648d-220">Determines how often the data that is stored in the in-memory storage should be flushed (sent to Application Insights).</span></span>

* <span data-ttu-id="0648d-221">Min: 1</span><span class="sxs-lookup"><span data-stu-id="0648d-221">Min: 1</span></span>
* <span data-ttu-id="0648d-222">Maximální počet: 300</span><span class="sxs-lookup"><span data-stu-id="0648d-222">Max: 300</span></span>
* <span data-ttu-id="0648d-223">Výchozí: 5</span><span class="sxs-lookup"><span data-stu-id="0648d-223">Default: 5</span></span>

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a><span data-ttu-id="0648d-224">MaxTransmissionStorageCapacityInMB</span><span class="sxs-lookup"><span data-stu-id="0648d-224">MaxTransmissionStorageCapacityInMB</span></span>
<span data-ttu-id="0648d-225">Určuje maximální velikost v MB, která je vymezena pro trvalé úložiště na místním disku.</span><span class="sxs-lookup"><span data-stu-id="0648d-225">Determines the maximum size in MB that is allotted to the persistent storage on the local disk.</span></span> <span data-ttu-id="0648d-226">Toto úložiště se používá pro zachování telemetrie položky, které se nepodařilo přenést do koncového bodu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0648d-226">This storage is used for persisting telemetry items that failed to be transmitted to the Application Insights endpoint.</span></span> <span data-ttu-id="0648d-227">Pokud velikost úložiště byly splněny, nové položky telemetrie se zahodí.</span><span class="sxs-lookup"><span data-stu-id="0648d-227">When the storage size has been met, new telemetry items will be discarded.</span></span>

* <span data-ttu-id="0648d-228">Min: 1</span><span class="sxs-lookup"><span data-stu-id="0648d-228">Min: 1</span></span>
* <span data-ttu-id="0648d-229">Maximální počet: 100</span><span class="sxs-lookup"><span data-stu-id="0648d-229">Max: 100</span></span>
* <span data-ttu-id="0648d-230">Výchozí: 10</span><span class="sxs-lookup"><span data-stu-id="0648d-230">Default: 10</span></span>

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a><span data-ttu-id="0648d-231">InstrumentationKey</span><span class="sxs-lookup"><span data-stu-id="0648d-231">InstrumentationKey</span></span>
<span data-ttu-id="0648d-232">Určuje prostředek Application Insights, ve kterém se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="0648d-232">This determines the Application Insights resource in which your data appears.</span></span> <span data-ttu-id="0648d-233">Obvykle vytvoříte prostředek samostatné samostatné klíčem pro každý z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="0648d-233">Typically you create a separate resource, with a separate key, for each of your applications.</span></span>

<span data-ttu-id="0648d-234">Pokud chcete nastavit klíč dynamicky – například pokud chcete odeslat výsledky z vaší aplikace do různých prostředků – můžete vynechat klíč z konfiguračního souboru a nastavení v kódu.</span><span class="sxs-lookup"><span data-stu-id="0648d-234">If you want to set the key dynamically - for example if you want to send results from your application to different resources - you can omit the key from the configuration file, and set it in code instead.</span></span>

<span data-ttu-id="0648d-235">Klíč pro všechny instance TelemetryClient, včetně standardní telemetrie moduly nastavit klíč v TelemetryConfiguration.Active.</span><span class="sxs-lookup"><span data-stu-id="0648d-235">To set the key for all instances of TelemetryClient, including standard telemetry modules, set the key in TelemetryConfiguration.Active.</span></span> <span data-ttu-id="0648d-236">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0648d-236">Do this in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

<span data-ttu-id="0648d-237">Pokud chcete odeslat sadu událostí na jiný prostředek, můžete nastavit klíč pro konkrétní TelemetryClient:</span><span class="sxs-lookup"><span data-stu-id="0648d-237">If you just want to send a specific set of events to a different resource, you can set the key for a specific TelemetryClient:</span></span>

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

<span data-ttu-id="0648d-238">Chcete-li získat nový klíč, [vytvoření nového prostředku na portálu služby Application Insights][new].</span><span class="sxs-lookup"><span data-stu-id="0648d-238">To get a new key, [create a new resource in the Application Insights portal][new].</span></span>

## <a name="next-steps"></a><span data-ttu-id="0648d-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0648d-239">Next steps</span></span>
<span data-ttu-id="0648d-240">[Další informace o rozhraní API][api].</span><span class="sxs-lookup"><span data-stu-id="0648d-240">[Learn more about the API][api].</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
