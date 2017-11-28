---
title: "Prozkoumejte protokoly trasování .NET ve službě Application Insights"
description: "Vygenerovat pomocí trasování, NLog a Log4Net protokolů hledání."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 68e03bf10167ecde675d62782de7063aea9e81d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a><span data-ttu-id="ddf30-103">Prozkoumejte protokoly trasování .NET ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddf30-103">Explore .NET trace logs in Application Insights</span></span>
<span data-ttu-id="ddf30-104">Pokud používáte NLog log4Net nebo System.Diagnostics.Trace pro diagnostické trasování v aplikaci ASP.NET, může mít vaše protokoly posílá [Azure Application Insights][start], kde můžete prozkoumat a vyhledávání je.</span><span class="sxs-lookup"><span data-stu-id="ddf30-104">If you use NLog, log4Net or System.Diagnostics.Trace for diagnostic tracing in your ASP.NET application, you can have your logs sent to [Azure Application Insights][start], where you can explore and search them.</span></span> <span data-ttu-id="ddf30-105">Protokoly bude sloučen s další telemetrií pocházející z vaší aplikace, tak, aby mohli identifikovat trasování přidružené údržby každý požadavek uživatele a jejich korelující s jinými události a sestavy výjimek.</span><span class="sxs-lookup"><span data-stu-id="ddf30-105">Your logs will be merged with the other telemetry coming from your application, so that you can identify the traces associated with servicing each user request, and correlate them with other events and exception reports.</span></span>

> [!NOTE]
> <span data-ttu-id="ddf30-106">Je nutné modul zachycení protokolu?</span><span class="sxs-lookup"><span data-stu-id="ddf30-106">Do you need the log capture module?</span></span> <span data-ttu-id="ddf30-107">Je užitečné adaptér pro protokolovacích nástrojů 3. stran, ale pokud už nepoužíváte NLog, log4Net nebo System.Diagnostics.Trace, zvažte právě volání [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) přímo.</span><span class="sxs-lookup"><span data-stu-id="ddf30-107">It's a useful adapter for 3rd-party loggers, but if you aren't already using NLog, log4Net or System.Diagnostics.Trace, consider just calling [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) directly.</span></span>
>
>

## <a name="install-logging-on-your-app"></a><span data-ttu-id="ddf30-108">Nainstalujte protokolování v aplikaci</span><span class="sxs-lookup"><span data-stu-id="ddf30-108">Install logging on your app</span></span>
<span data-ttu-id="ddf30-109">Nainstalujte rozhraní framework vaši zvolenou protokolování ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="ddf30-109">Install your chosen logging framework in your project.</span></span> <span data-ttu-id="ddf30-110">Výsledkem by mělo být položku v souboru app.config nebo web.config.</span><span class="sxs-lookup"><span data-stu-id="ddf30-110">This should result in an entry in app.config or web.config.</span></span>

<span data-ttu-id="ddf30-111">Pokud používáte System.Diagnostics.Trace, budete muset přidat záznam do souboru web.config:</span><span class="sxs-lookup"><span data-stu-id="ddf30-111">If you're using System.Diagnostics.Trace, you need to add an entry to web.config:</span></span>

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-to-collect-logs"></a><span data-ttu-id="ddf30-112">Konfigurovat Application Insights se mají shromažďovat protokoly</span><span class="sxs-lookup"><span data-stu-id="ddf30-112">Configure Application Insights to collect logs</span></span>
<span data-ttu-id="ddf30-113">**[Do projektu přidejte Application Insights](app-insights-asp-net.md)**  Pokud jste to neudělali, ještě.</span><span class="sxs-lookup"><span data-stu-id="ddf30-113">**[Add Application Insights to your project](app-insights-asp-net.md)** if you haven't done that yet.</span></span> <span data-ttu-id="ddf30-114">Zobrazí se možnost zahrnout kolektoru protokolů.</span><span class="sxs-lookup"><span data-stu-id="ddf30-114">You'll see an option to include the log collector.</span></span>

<span data-ttu-id="ddf30-115">Nebo **konfigurovat Application Insights** kliknutím pravým tlačítkem na projekt v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ddf30-115">Or **Configure Application Insights** by right-clicking your project in Solution Explorer.</span></span> <span data-ttu-id="ddf30-116">Vyberte možnost **konfigurace sběru trasovacích**.</span><span class="sxs-lookup"><span data-stu-id="ddf30-116">Select the option to **Configure trace collection**.</span></span>

<span data-ttu-id="ddf30-117">*Žádná možnost Application Insights nabídky nebo protokolu kolekce?*</span><span class="sxs-lookup"><span data-stu-id="ddf30-117">*No Application Insights menu or log collector option?*</span></span> <span data-ttu-id="ddf30-118">Zkuste [řešení potíží s](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="ddf30-118">Try [Troubleshooting](#troubleshooting).</span></span>

## <a name="manual-installation"></a><span data-ttu-id="ddf30-119">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="ddf30-119">Manual installation</span></span>
<span data-ttu-id="ddf30-120">Tuto metodu použijte, pokud váš typ projektu není podporován Instalační služby Application Insights (například Windows desktop projektu).</span><span class="sxs-lookup"><span data-stu-id="ddf30-120">Use this method if your project type isn't supported by the Application Insights installer (for example a Windows desktop project).</span></span>

1. <span data-ttu-id="ddf30-121">Pokud máte v plánu používat log4Net nebo NLog, nainstalujte ho do projektu.</span><span class="sxs-lookup"><span data-stu-id="ddf30-121">If you plan to use log4Net or NLog, install it in your project.</span></span>
2. <span data-ttu-id="ddf30-122">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ddf30-122">In Solution Explorer, right-click your project and choose **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="ddf30-123">Vyhledání Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddf30-123">Search for "Application Insights"</span></span>
4. <span data-ttu-id="ddf30-124">Vyberte odpovídající balíček – jeden z:</span><span class="sxs-lookup"><span data-stu-id="ddf30-124">Select the appropriate package - one of:</span></span>

   * <span data-ttu-id="ddf30-125">Microsoft.ApplicationInsights.TraceListener (pro zachycení System.Diagnostics.Trace volání)</span><span class="sxs-lookup"><span data-stu-id="ddf30-125">Microsoft.ApplicationInsights.TraceListener (to capture System.Diagnostics.Trace calls)</span></span>
   * <span data-ttu-id="ddf30-126">Microsoft.ApplicationInsights.EventSourceListener (k zachycení událostí EventSource)</span><span class="sxs-lookup"><span data-stu-id="ddf30-126">Microsoft.ApplicationInsights.EventSourceListener (to capture EventSource events)</span></span>
   * <span data-ttu-id="ddf30-127">Microsoft.ApplicationInsights.EtwListener (k zachycení událostí trasování událostí pro Windows)</span><span class="sxs-lookup"><span data-stu-id="ddf30-127">Microsoft.ApplicationInsights.EtwListener (to capture ETW events)</span></span>
   * <span data-ttu-id="ddf30-128">Microsoft.ApplicationInsights.NLogTarget</span><span class="sxs-lookup"><span data-stu-id="ddf30-128">Microsoft.ApplicationInsights.NLogTarget</span></span>
   * <span data-ttu-id="ddf30-129">Microsoft.ApplicationInsights.Log4NetAppender</span><span class="sxs-lookup"><span data-stu-id="ddf30-129">Microsoft.ApplicationInsights.Log4NetAppender</span></span>

<span data-ttu-id="ddf30-130">Balíček NuGet nainstaluje potřebné sestavení a také upraví soubor web.config nebo app.config.</span><span class="sxs-lookup"><span data-stu-id="ddf30-130">The NuGet package installs the necessary assemblies, and also modifies web.config or app.config.</span></span>

## <a name="insert-diagnostic-log-calls"></a><span data-ttu-id="ddf30-131">Vložit volání protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="ddf30-131">Insert diagnostic log calls</span></span>
<span data-ttu-id="ddf30-132">Pokud používáte System.Diagnostics.Trace, typické volání by byl:</span><span class="sxs-lookup"><span data-stu-id="ddf30-132">If you use System.Diagnostics.Trace, a typical call would be:</span></span>

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

<span data-ttu-id="ddf30-133">Pokud dáváte přednost log4net nebo NLog:</span><span class="sxs-lookup"><span data-stu-id="ddf30-133">If you prefer log4net or NLog:</span></span>

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a><span data-ttu-id="ddf30-134">Pomocí událostí EventSource</span><span class="sxs-lookup"><span data-stu-id="ddf30-134">Using EventSource events</span></span>
<span data-ttu-id="ddf30-135">Můžete nakonfigurovat [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) odeslání do služby Application Insights jako trasování událostí.</span><span class="sxs-lookup"><span data-stu-id="ddf30-135">You can configure [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="ddf30-136">Nejdřív nainstalujte `Microsoft.ApplicationInsights.EventSourceListener` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ddf30-136">First, install the `Microsoft.ApplicationInsights.EventSourceListener` NuGet package.</span></span> <span data-ttu-id="ddf30-137">Upravte `TelemetryModules` části [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="ddf30-137">Then edit `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="ddf30-138">Pro každý zdroj můžete nastavit následující parametry:</span><span class="sxs-lookup"><span data-stu-id="ddf30-138">For each source, you can set the following parameters:</span></span>
 * <span data-ttu-id="ddf30-139">`Name`Určuje název EventSource ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-139">`Name` specifies the name of the EventSource to collect.</span></span>
 * <span data-ttu-id="ddf30-140">`Level`Určuje úroveň protokolování ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-140">`Level` specifies the logging level to collect.</span></span> <span data-ttu-id="ddf30-141">Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="ddf30-141">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="ddf30-142">`Keywords`(Volitelné) Určuje celočíselnou hodnotu kombinace klíčových slov používat.</span><span class="sxs-lookup"><span data-stu-id="ddf30-142">`Keywords` (Optional) specifies the integer value of keywords combinations to use.</span></span>

## <a name="using-diagnosticsource-events"></a><span data-ttu-id="ddf30-143">Pomocí DiagnosticSource události</span><span class="sxs-lookup"><span data-stu-id="ddf30-143">Using DiagnosticSource events</span></span>
<span data-ttu-id="ddf30-144">Můžete nakonfigurovat [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) odeslání do služby Application Insights jako trasování událostí.</span><span class="sxs-lookup"><span data-stu-id="ddf30-144">You can configure [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="ddf30-145">Nejdřív nainstalujte [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ddf30-145">First, install the [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet package.</span></span> <span data-ttu-id="ddf30-146">Upravte `TelemetryModules` části [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="ddf30-146">Then edit the `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

<span data-ttu-id="ddf30-147">Pro každý DiagnosticSource chcete trasovat, přidat položku s `Name` atributu nastavena na název vaší DiagnosticSource.</span><span class="sxs-lookup"><span data-stu-id="ddf30-147">For each DiagnosticSource you want to trace, add an entry with the `Name` attribute  set to the name of your DiagnosticSource.</span></span>

## <a name="using-etw-events"></a><span data-ttu-id="ddf30-148">Pomocí události trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="ddf30-148">Using ETW events</span></span>
<span data-ttu-id="ddf30-149">Můžete nakonfigurovat události trasování událostí pro odeslání do Application Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-149">You can configure ETW events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="ddf30-150">Nejdřív nainstalujte `Microsoft.ApplicationInsights.EtwCollector` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ddf30-150">First, install the `Microsoft.ApplicationInsights.EtwCollector` NuGet package.</span></span> <span data-ttu-id="ddf30-151">Upravte `TelemetryModules` části [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="ddf30-151">Then edit `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

> [!NOTE] 
> <span data-ttu-id="ddf30-152">Události trasování událostí pro Windows se můžou shromažďovat pouze pokud proces hostování sady SDK je spuštěný pod identitou, která je členem skupiny "Uživatelé protokolu výkonu" nebo správci.</span><span class="sxs-lookup"><span data-stu-id="ddf30-152">ETW events can only be collected if the process hosting the SDK is running under an identity that is a member of "Performance Log Users" or Administrators.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="ddf30-153">Pro každý zdroj můžete nastavit následující parametry:</span><span class="sxs-lookup"><span data-stu-id="ddf30-153">For each source, you can set the following parameters:</span></span>
 * <span data-ttu-id="ddf30-154">`ProviderName`je název zprostředkovatele trasování událostí pro Windows, který má shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="ddf30-154">`ProviderName` is the name of the ETW provider to collect.</span></span>
 * <span data-ttu-id="ddf30-155">`ProviderGuid`Určuje identifikátor GUID zprostředkovatele trasování událostí pro Windows ke shromažďování, můžete použít místo `ProviderName`.</span><span class="sxs-lookup"><span data-stu-id="ddf30-155">`ProviderGuid` specifies the GUID of the ETW provider to collect, can be used instead of `ProviderName`.</span></span>
 * <span data-ttu-id="ddf30-156">`Level`Nastaví úroveň protokolování ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-156">`Level` sets the logging level to collect.</span></span> <span data-ttu-id="ddf30-157">Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="ddf30-157">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="ddf30-158">`Keywords`(Volitelné) nastaví celé číslo – klíčové slovo kombinace používat.</span><span class="sxs-lookup"><span data-stu-id="ddf30-158">`Keywords` (Optional) sets the integer value of keyword combinations to use.</span></span>

## <a name="using-the-trace-api-directly"></a><span data-ttu-id="ddf30-159">Přímo pomocí trasování rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ddf30-159">Using the Trace API directly</span></span>
<span data-ttu-id="ddf30-160">Trasování Application Insights API můžete volat přímo.</span><span class="sxs-lookup"><span data-stu-id="ddf30-160">You can call the Application Insights trace API directly.</span></span> <span data-ttu-id="ddf30-161">Toto rozhraní API použijte adaptéry protokolování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-161">The logging adapters use this API.</span></span>

<span data-ttu-id="ddf30-162">Například:</span><span class="sxs-lookup"><span data-stu-id="ddf30-162">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

<span data-ttu-id="ddf30-163">Výhodou TrackTrace je, že můžete ukládat poměrně dlouho data ve zprávě.</span><span class="sxs-lookup"><span data-stu-id="ddf30-163">An advantage of TrackTrace is that you can put relatively long data in the message.</span></span> <span data-ttu-id="ddf30-164">Například může zakódovat následných dat existuje.</span><span class="sxs-lookup"><span data-stu-id="ddf30-164">For example, you could encode POST data there.</span></span>

<span data-ttu-id="ddf30-165">Kromě toho můžete přidat úroveň závažnosti na zprávu.</span><span class="sxs-lookup"><span data-stu-id="ddf30-165">In addition, you can add a severity level to your message.</span></span> <span data-ttu-id="ddf30-166">A, podobně jako ostatní telemetrických dat, můžete přidat hodnoty vlastností, které můžete použít k lepšímu filtru nebo vyhledávání pro různé skupiny trasování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-166">And, like other telemetry, you can add property values that you can use to help filter or search for different sets of traces.</span></span> <span data-ttu-id="ddf30-167">Například:</span><span class="sxs-lookup"><span data-stu-id="ddf30-167">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="ddf30-168">To by vás, povolte v [vyhledávání][diagnostic], snadno filtrovat všechny zprávy konkrétní závažnosti úrovně týkající se konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="ddf30-168">This would enable you, in [Search][diagnostic], to easily filter out all the messages of a particular severity level relating to a particular database.</span></span>

## <a name="explore-your-logs"></a><span data-ttu-id="ddf30-169">Prozkoumejte protokoly</span><span class="sxs-lookup"><span data-stu-id="ddf30-169">Explore your logs</span></span>
<span data-ttu-id="ddf30-170">Spuštění aplikace, buď v režimu ladění, nebo ji nasadit za provozu.</span><span class="sxs-lookup"><span data-stu-id="ddf30-170">Run your app, either in debug mode or deploy it live.</span></span>

<span data-ttu-id="ddf30-171">V okně Přehled vaší aplikace v [portálu služby Application Insights][portal], zvolte [vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="ddf30-171">In your app's overview blade in [the Application Insights portal][portal], choose [Search][diagnostic].</span></span>

![Ve službě Application Insights zvolte vyhledávání](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Search](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

<span data-ttu-id="ddf30-174">Je to možné, například:</span><span class="sxs-lookup"><span data-stu-id="ddf30-174">You can, for example:</span></span>

* <span data-ttu-id="ddf30-175">Filtrování v protokolu trasování nebo na položky s konkrétní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="ddf30-175">Filter on log traces, or on items with specific properties</span></span>
* <span data-ttu-id="ddf30-176">Zkontrolujte konkrétní položku podrobně.</span><span class="sxs-lookup"><span data-stu-id="ddf30-176">Inspect a specific item in detail.</span></span>
* <span data-ttu-id="ddf30-177">Najít další telemetrií vztahující se ke stejnému požadavku uživatele (to znamená, se stejnou OperationId)</span><span class="sxs-lookup"><span data-stu-id="ddf30-177">Find other telemetry relating to the same user request (that is, with the same OperationId)</span></span>
* <span data-ttu-id="ddf30-178">Konfiguraci této stránky uložit jako oblíbenou položku</span><span class="sxs-lookup"><span data-stu-id="ddf30-178">Save the configuration of this page as a Favorite</span></span>

> [!NOTE]
> <span data-ttu-id="ddf30-179">**Vzorkování.**</span><span class="sxs-lookup"><span data-stu-id="ddf30-179">**Sampling.**</span></span> <span data-ttu-id="ddf30-180">Pokud vaše aplikace odešle velké množství dat a používáte Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddf30-180">If your application sends a lot of data and you are using the Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="ddf30-181">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-181">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <a name="next-steps"></a><span data-ttu-id="ddf30-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddf30-182">Next steps</span></span>
<span data-ttu-id="ddf30-183">[Diagnostikovat chyby a výjimky technologie ASP.NET][exceptions]</span><span class="sxs-lookup"><span data-stu-id="ddf30-183">[Diagnose failures and exceptions in ASP.NET][exceptions]</span></span>

<span data-ttu-id="ddf30-184">[Další informace o vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="ddf30-184">[Learn more about Search][diagnostic].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ddf30-185">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ddf30-185">Troubleshooting</span></span>
### <a name="how-do-i-do-this-for-java"></a><span data-ttu-id="ddf30-186">Jak se to pro jazyk Java?</span><span class="sxs-lookup"><span data-stu-id="ddf30-186">How do I do this for Java?</span></span>
<span data-ttu-id="ddf30-187">Použití [adaptéry protokolu Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="ddf30-187">Use the [Java log adapters](app-insights-java-trace-logs.md).</span></span>

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a><span data-ttu-id="ddf30-188">Neexistuje žádná možnost Application Insights na místní nabídky projektu</span><span class="sxs-lookup"><span data-stu-id="ddf30-188">There's no Application Insights option on the project context menu</span></span>
* <span data-ttu-id="ddf30-189">Kontrola instalace nástrojů Application Insights na tomto počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="ddf30-189">Check Application Insights tools is installed on this development machine.</span></span> <span data-ttu-id="ddf30-190">V nabídce Nástroje aplikace Visual Studio, rozšíření a aktualizace vyhledejte nástroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddf30-190">In Visual Studio menu Tools, Extensions and Updates, look for Application Insights Tools.</span></span> <span data-ttu-id="ddf30-191">Pokud není na kartě nainstalovaná, otevřete kartu Online a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="ddf30-191">If it isn't in the Installed tab, open the Online tab and install it.</span></span>
* <span data-ttu-id="ddf30-192">To může být typ projektu není podporováno nástrojů Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddf30-192">This might be a type of project not supported by Application Insights tools.</span></span> <span data-ttu-id="ddf30-193">Použití [ruční instalace](#manual-installation).</span><span class="sxs-lookup"><span data-stu-id="ddf30-193">Use [manual installation](#manual-installation).</span></span>

### <a name="no-log-adapter-option-in-the-configuration-tool"></a><span data-ttu-id="ddf30-194">Žádná možnost pro adaptér protokolu v konfiguračním nástroji služby</span><span class="sxs-lookup"><span data-stu-id="ddf30-194">No log adapter option in the configuration tool</span></span>
* <span data-ttu-id="ddf30-195">Musíte nejprve nainstalovat rozhraní protokolování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-195">You need to install the logging framework first.</span></span>
* <span data-ttu-id="ddf30-196">Pokud používáte System.Diagnostics.Trace, ověřte, že je [nakonfigurované v `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="ddf30-196">If you're using System.Diagnostics.Trace, make sure you [configured it in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span></span>
* <span data-ttu-id="ddf30-197">Mít jste získali nejnovější verzi služby Application Insights?</span><span class="sxs-lookup"><span data-stu-id="ddf30-197">Have you got the latest version of Application Insights?</span></span> <span data-ttu-id="ddf30-198">V sadě Visual Studio **nástroje** nabídce zvolte **rozšíření a aktualizace**a otevřete **aktualizace** kartě. Pokud Developer Analytics tools existuje, klikněte na tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ddf30-198">In Visual Studio **Tools** menu, choose **Extensions and Updates**, and open the **Updates** tab. If Developer Analytics tools is there, click to update it.</span></span>

### <span data-ttu-id="ddf30-199"><a name="emptykey"></a>Dojde k chybě "klíč instrumentace nemůže být prázdný"</span><span class="sxs-lookup"><span data-stu-id="ddf30-199"><a name="emptykey"></a>I get an error "Instrumentation key cannot be empty"</span></span>
<span data-ttu-id="ddf30-200">Zdá se, zda jste nainstalovali balíček Nuget adaptér protokolování bez instalace služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddf30-200">Looks like you installed the logging adapter Nuget package without installing Application Insights.</span></span>

<span data-ttu-id="ddf30-201">V Průzkumníku řešení klikněte pravým tlačítkem na `ApplicationInsights.config` a zvolte **aktualizace Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="ddf30-201">In Solution Explorer, right-click `ApplicationInsights.config` and choose **Update Application Insights**.</span></span> <span data-ttu-id="ddf30-202">Zobrazí dialogové okno se žádostí, můžete se přihlásit k Azure a buď vytvořte prostředek Application Insights, nebo znovu použijte stávající.</span><span class="sxs-lookup"><span data-stu-id="ddf30-202">You'll get a dialog that invites you to sign in to Azure and either create an Application Insights resource, or re-use an existing one.</span></span> <span data-ttu-id="ddf30-203">Který by měl opravte ji.</span><span class="sxs-lookup"><span data-stu-id="ddf30-203">That should fix it.</span></span>

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a><span data-ttu-id="ddf30-204">Zobrazují se trasování v diagnostické vyhledávání, ale není ostatní události</span><span class="sxs-lookup"><span data-stu-id="ddf30-204">I can see traces in diagnostic search, but not the other events</span></span>
<span data-ttu-id="ddf30-205">Někdy může trvat nějakou dobu všechny události a žádosti o získání prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ddf30-205">It can sometimes take a while for all the events and requests to get through the pipeline.</span></span>

### <span data-ttu-id="ddf30-206"><a name="limits"></a>Množství dat, které se uchovávají?</span><span class="sxs-lookup"><span data-stu-id="ddf30-206"><a name="limits"></a>How much data is retained?</span></span>
<span data-ttu-id="ddf30-207">Několik různých faktorů vliv na množství dat, které jsou zachovány.</span><span class="sxs-lookup"><span data-stu-id="ddf30-207">Several factors impact the amount of data retained.</span></span> <span data-ttu-id="ddf30-208">Najdete v článku [omezení](app-insights-api-custom-events-metrics.md#limits) části stránky zákazníka události metriky pro další informace.</span><span class="sxs-lookup"><span data-stu-id="ddf30-208">See the [limits](app-insights-api-custom-events-metrics.md#limits) section of the customer event metrics page for more information.</span></span> 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a><span data-ttu-id="ddf30-209">Některé položky protokolu, které očekávat nejsou zobrazeny</span><span class="sxs-lookup"><span data-stu-id="ddf30-209">I'm not seeing some of the log entries that I expect</span></span>
<span data-ttu-id="ddf30-210">Pokud vaše aplikace odešle velké množství dat a používáte Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddf30-210">If your application sends a lot of data and you are using the Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="ddf30-211">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddf30-211">Learn more about sampling.</span></span>](app-insights-sampling.md)

## <span data-ttu-id="ddf30-212"><a name="add"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddf30-212"><a name="add"></a>Next steps</span></span>
* <span data-ttu-id="ddf30-213">[Nastavení dostupnosti a odezvy testů][availability]</span><span class="sxs-lookup"><span data-stu-id="ddf30-213">[Set up availability and responsiveness tests][availability]</span></span>
* <span data-ttu-id="ddf30-214">[Řešení potíží][qna]</span><span class="sxs-lookup"><span data-stu-id="ddf30-214">[Troubleshooting][qna]</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
