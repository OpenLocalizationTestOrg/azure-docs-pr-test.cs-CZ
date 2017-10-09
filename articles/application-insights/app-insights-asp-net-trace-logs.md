---
title: "protokolů trasování .NET aaaExplore ve službě Application Insights"
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
ms.openlocfilehash: 6bfcd9e5751c3656236d7eb2fc09321740171a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a><span data-ttu-id="7bd59-103">Prozkoumejte protokoly trasování .NET ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="7bd59-103">Explore .NET trace logs in Application Insights</span></span>
<span data-ttu-id="7bd59-104">Pokud používáte NLog log4Net nebo System.Diagnostics.Trace pro diagnostické trasování v aplikaci ASP.NET, může mít vaše protokoly odeslané příliš[Azure Application Insights][start], kde můžete prozkoumat a vyhledávání je.</span><span class="sxs-lookup"><span data-stu-id="7bd59-104">If you use NLog, log4Net or System.Diagnostics.Trace for diagnostic tracing in your ASP.NET application, you can have your logs sent too[Azure Application Insights][start], where you can explore and search them.</span></span> <span data-ttu-id="7bd59-105">Protokoly se sloučil s hello jiných telemetrie pocházející z vaší aplikace, takže můžete identifikovat hello trasování přidružené údržby každý požadavek uživatele a jejich korelující s jinými události a sestavy výjimek.</span><span class="sxs-lookup"><span data-stu-id="7bd59-105">Your logs will be merged with hello other telemetry coming from your application, so that you can identify hello traces associated with servicing each user request, and correlate them with other events and exception reports.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd59-106">Je nutné modul zachycení hello protokolu?</span><span class="sxs-lookup"><span data-stu-id="7bd59-106">Do you need hello log capture module?</span></span> <span data-ttu-id="7bd59-107">Je užitečné adaptér pro protokolovacích nástrojů 3. stran, ale pokud už nepoužíváte NLog, log4Net nebo System.Diagnostics.Trace, zvažte právě volání [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) přímo.</span><span class="sxs-lookup"><span data-stu-id="7bd59-107">It's a useful adapter for 3rd-party loggers, but if you aren't already using NLog, log4Net or System.Diagnostics.Trace, consider just calling [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) directly.</span></span>
>
>

## <a name="install-logging-on-your-app"></a><span data-ttu-id="7bd59-108">Nainstalujte protokolování v aplikaci</span><span class="sxs-lookup"><span data-stu-id="7bd59-108">Install logging on your app</span></span>
<span data-ttu-id="7bd59-109">Nainstalujte rozhraní framework vaši zvolenou protokolování ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="7bd59-109">Install your chosen logging framework in your project.</span></span> <span data-ttu-id="7bd59-110">Výsledkem by mělo být položku v souboru app.config nebo web.config.</span><span class="sxs-lookup"><span data-stu-id="7bd59-110">This should result in an entry in app.config or web.config.</span></span>

<span data-ttu-id="7bd59-111">Pokud používáte System.Diagnostics.Trace, je třeba tooadd tooweb.config položku:</span><span class="sxs-lookup"><span data-stu-id="7bd59-111">If you're using System.Diagnostics.Trace, you need tooadd an entry tooweb.config:</span></span>

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
## <a name="configure-application-insights-toocollect-logs"></a><span data-ttu-id="7bd59-112">Konfigurace protokolů toocollect Application Insights</span><span class="sxs-lookup"><span data-stu-id="7bd59-112">Configure Application Insights toocollect logs</span></span>
<span data-ttu-id="7bd59-113">**[Přidejte Application Insights tooyour projekt](app-insights-asp-net.md)**  Pokud jste to neudělali, ještě.</span><span class="sxs-lookup"><span data-stu-id="7bd59-113">**[Add Application Insights tooyour project](app-insights-asp-net.md)** if you haven't done that yet.</span></span> <span data-ttu-id="7bd59-114">Uvidíte kolektor protokolů hello tooinclude možnost.</span><span class="sxs-lookup"><span data-stu-id="7bd59-114">You'll see an option tooinclude hello log collector.</span></span>

<span data-ttu-id="7bd59-115">Nebo **konfigurovat Application Insights** kliknutím pravým tlačítkem na projekt v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="7bd59-115">Or **Configure Application Insights** by right-clicking your project in Solution Explorer.</span></span> <span data-ttu-id="7bd59-116">Vyberte možnost hello příliš**konfigurace sběru trasovacích**.</span><span class="sxs-lookup"><span data-stu-id="7bd59-116">Select hello option too**Configure trace collection**.</span></span>

<span data-ttu-id="7bd59-117">*Žádná možnost Application Insights nabídky nebo protokolu kolekce?*</span><span class="sxs-lookup"><span data-stu-id="7bd59-117">*No Application Insights menu or log collector option?*</span></span> <span data-ttu-id="7bd59-118">Zkuste [řešení potíží s](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="7bd59-118">Try [Troubleshooting](#troubleshooting).</span></span>

## <a name="manual-installation"></a><span data-ttu-id="7bd59-119">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="7bd59-119">Manual installation</span></span>
<span data-ttu-id="7bd59-120">Tuto metodu použijte, pokud váš typ projektu není podporován hello Application Insights Instalační služby systému (třeba Windows desktop projektu).</span><span class="sxs-lookup"><span data-stu-id="7bd59-120">Use this method if your project type isn't supported by hello Application Insights installer (for example a Windows desktop project).</span></span>

1. <span data-ttu-id="7bd59-121">Pokud máte v plánu toouse log4Net nebo NLog, nainstalujte ho do projektu.</span><span class="sxs-lookup"><span data-stu-id="7bd59-121">If you plan toouse log4Net or NLog, install it in your project.</span></span>
2. <span data-ttu-id="7bd59-122">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7bd59-122">In Solution Explorer, right-click your project and choose **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="7bd59-123">Vyhledání Application Insights</span><span class="sxs-lookup"><span data-stu-id="7bd59-123">Search for "Application Insights"</span></span>
4. <span data-ttu-id="7bd59-124">Vyberte odpovídající balíček hello – jeden z:</span><span class="sxs-lookup"><span data-stu-id="7bd59-124">Select hello appropriate package - one of:</span></span>

   * <span data-ttu-id="7bd59-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace volání)</span><span class="sxs-lookup"><span data-stu-id="7bd59-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace calls)</span></span>
   * <span data-ttu-id="7bd59-126">Microsoft.ApplicationInsights.EventSourceListener (událostí EventSource toocapture)</span><span class="sxs-lookup"><span data-stu-id="7bd59-126">Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource events)</span></span>
   * <span data-ttu-id="7bd59-127">Microsoft.ApplicationInsights.EtwListener (toocapture události trasování událostí pro Windows)</span><span class="sxs-lookup"><span data-stu-id="7bd59-127">Microsoft.ApplicationInsights.EtwListener (toocapture ETW events)</span></span>
   * <span data-ttu-id="7bd59-128">Microsoft.ApplicationInsights.NLogTarget</span><span class="sxs-lookup"><span data-stu-id="7bd59-128">Microsoft.ApplicationInsights.NLogTarget</span></span>
   * <span data-ttu-id="7bd59-129">Microsoft.ApplicationInsights.Log4NetAppender</span><span class="sxs-lookup"><span data-stu-id="7bd59-129">Microsoft.ApplicationInsights.Log4NetAppender</span></span>

<span data-ttu-id="7bd59-130">balíček NuGet Hello nainstaluje potřebné sestavení hello a také upraví soubor web.config nebo app.config.</span><span class="sxs-lookup"><span data-stu-id="7bd59-130">hello NuGet package installs hello necessary assemblies, and also modifies web.config or app.config.</span></span>

## <a name="insert-diagnostic-log-calls"></a><span data-ttu-id="7bd59-131">Vložit volání protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="7bd59-131">Insert diagnostic log calls</span></span>
<span data-ttu-id="7bd59-132">Pokud používáte System.Diagnostics.Trace, typické volání by byl:</span><span class="sxs-lookup"><span data-stu-id="7bd59-132">If you use System.Diagnostics.Trace, a typical call would be:</span></span>

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

<span data-ttu-id="7bd59-133">Pokud dáváte přednost log4net nebo NLog:</span><span class="sxs-lookup"><span data-stu-id="7bd59-133">If you prefer log4net or NLog:</span></span>

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a><span data-ttu-id="7bd59-134">Pomocí událostí EventSource</span><span class="sxs-lookup"><span data-stu-id="7bd59-134">Using EventSource events</span></span>
<span data-ttu-id="7bd59-135">Můžete nakonfigurovat [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) tooApplication toobe odeslané události Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-135">You can configure [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="7bd59-136">Nejdřív nainstalujte hello `Microsoft.ApplicationInsights.EventSourceListener` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="7bd59-136">First, install hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet package.</span></span> <span data-ttu-id="7bd59-137">Upravte `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="7bd59-137">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="7bd59-138">Pro každý zdroj můžete nastavit hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="7bd59-138">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="7bd59-139">`Name`Určuje název hello hello EventSource toocollect.</span><span class="sxs-lookup"><span data-stu-id="7bd59-139">`Name` specifies hello name of hello EventSource toocollect.</span></span>
 * <span data-ttu-id="7bd59-140">`Level`Určuje hello toocollect úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-140">`Level` specifies hello logging level toocollect.</span></span> <span data-ttu-id="7bd59-141">Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="7bd59-141">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="7bd59-142">`Keywords`(Volitelné) Určuje celé číslo hello toouse kombinace klíčových slov.</span><span class="sxs-lookup"><span data-stu-id="7bd59-142">`Keywords` (Optional) specifies hello integer value of keywords combinations toouse.</span></span>

## <a name="using-diagnosticsource-events"></a><span data-ttu-id="7bd59-143">Pomocí DiagnosticSource události</span><span class="sxs-lookup"><span data-stu-id="7bd59-143">Using DiagnosticSource events</span></span>
<span data-ttu-id="7bd59-144">Můžete nakonfigurovat [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) tooApplication toobe odeslané události Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-144">You can configure [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="7bd59-145">Nejdřív nainstalujte hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="7bd59-145">First, install hello [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet package.</span></span> <span data-ttu-id="7bd59-146">Upravte hello `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="7bd59-146">Then edit hello `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

<span data-ttu-id="7bd59-147">Pro každý DiagnosticSource tootrace, chcete přidat položku s hello `Name` toohello název vaší DiagnosticSource nastaven atribut.</span><span class="sxs-lookup"><span data-stu-id="7bd59-147">For each DiagnosticSource you want tootrace, add an entry with hello `Name` attribute  set toohello name of your DiagnosticSource.</span></span>

## <a name="using-etw-events"></a><span data-ttu-id="7bd59-148">Pomocí události trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="7bd59-148">Using ETW events</span></span>
<span data-ttu-id="7bd59-149">Můžete nakonfigurovat toobe události trasování událostí pro Windows odeslány tooApplication Insights jako trasování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-149">You can configure ETW events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="7bd59-150">Nejdřív nainstalujte hello `Microsoft.ApplicationInsights.EtwCollector` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="7bd59-150">First, install hello `Microsoft.ApplicationInsights.EtwCollector` NuGet package.</span></span> <span data-ttu-id="7bd59-151">Upravte `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.</span><span class="sxs-lookup"><span data-stu-id="7bd59-151">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

> [!NOTE] 
> <span data-ttu-id="7bd59-152">Události trasování událostí pro Windows se můžou shromažďovat pouze pokud hello hostitelský proces hello SDK je spuštěný pod identitou, která je členem skupiny "Uživatelé protokolu výkonu" nebo správci.</span><span class="sxs-lookup"><span data-stu-id="7bd59-152">ETW events can only be collected if hello process hosting hello SDK is running under an identity that is a member of "Performance Log Users" or Administrators.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="7bd59-153">Pro každý zdroj můžete nastavit hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="7bd59-153">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="7bd59-154">`ProviderName`je název hello toocollect zprostředkovatele trasování událostí pro Windows hello.</span><span class="sxs-lookup"><span data-stu-id="7bd59-154">`ProviderName` is hello name of hello ETW provider toocollect.</span></span>
 * <span data-ttu-id="7bd59-155">`ProviderGuid`Určuje hello GUID toocollect zprostředkovatele trasování událostí pro Windows hello, můžete použít místo `ProviderName`.</span><span class="sxs-lookup"><span data-stu-id="7bd59-155">`ProviderGuid` specifies hello GUID of hello ETW provider toocollect, can be used instead of `ProviderName`.</span></span>
 * <span data-ttu-id="7bd59-156">`Level`Nastaví hello toocollect úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-156">`Level` sets hello logging level toocollect.</span></span> <span data-ttu-id="7bd59-157">Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="7bd59-157">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="7bd59-158">`Keywords`(Volitelné) sady hello celočíselnou hodnotu toouse kombinace – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="7bd59-158">`Keywords` (Optional) sets hello integer value of keyword combinations toouse.</span></span>

## <a name="using-hello-trace-api-directly"></a><span data-ttu-id="7bd59-159">Přímo pomocí hello trasování API</span><span class="sxs-lookup"><span data-stu-id="7bd59-159">Using hello Trace API directly</span></span>
<span data-ttu-id="7bd59-160">Hello Application Insights trasování API můžete volat přímo.</span><span class="sxs-lookup"><span data-stu-id="7bd59-160">You can call hello Application Insights trace API directly.</span></span> <span data-ttu-id="7bd59-161">Toto rozhraní API použijte Hello protokolování adaptéry.</span><span class="sxs-lookup"><span data-stu-id="7bd59-161">hello logging adapters use this API.</span></span>

<span data-ttu-id="7bd59-162">Například:</span><span class="sxs-lookup"><span data-stu-id="7bd59-162">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

<span data-ttu-id="7bd59-163">Výhodou TrackTrace je, že můžete umístit relativně long – datový uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="7bd59-163">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="7bd59-164">Například může zakódovat následných dat existuje.</span><span class="sxs-lookup"><span data-stu-id="7bd59-164">For example, you could encode POST data there.</span></span>

<span data-ttu-id="7bd59-165">Kromě toho můžete přidat zprávu tooyour úrovně závažnosti.</span><span class="sxs-lookup"><span data-stu-id="7bd59-165">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="7bd59-166">A, podobně jako ostatní telemetrických dat, můžete přidat hodnoty vlastností, které můžete použít filtr toohelp nebo vyhledávání pro různé skupiny trasování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-166">And, like other telemetry, you can add property values that you can use toohelp filter or search for different sets of traces.</span></span> <span data-ttu-id="7bd59-167">Například:</span><span class="sxs-lookup"><span data-stu-id="7bd59-167">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="7bd59-168">To by vás, povolte v [vyhledávání][diagnostic], tooeasily filtru se všechny zprávy hello úrovně závažnosti konkrétní týkající se tooa konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="7bd59-168">This would enable you, in [Search][diagnostic], tooeasily filter out all hello messages of a particular severity level relating tooa particular database.</span></span>

## <a name="explore-your-logs"></a><span data-ttu-id="7bd59-169">Prozkoumejte protokoly</span><span class="sxs-lookup"><span data-stu-id="7bd59-169">Explore your logs</span></span>
<span data-ttu-id="7bd59-170">Spuštění aplikace, buď v režimu ladění, nebo ji nasadit za provozu.</span><span class="sxs-lookup"><span data-stu-id="7bd59-170">Run your app, either in debug mode or deploy it live.</span></span>

<span data-ttu-id="7bd59-171">V okně Přehled vaší aplikace v [portál Application Insights hello][portal], zvolte [vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="7bd59-171">In your app's overview blade in [hello Application Insights portal][portal], choose [Search][diagnostic].</span></span>

![Ve službě Application Insights zvolte vyhledávání](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Search](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

<span data-ttu-id="7bd59-174">Je to možné, například:</span><span class="sxs-lookup"><span data-stu-id="7bd59-174">You can, for example:</span></span>

* <span data-ttu-id="7bd59-175">Filtrování v protokolu trasování nebo na položky s konkrétní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7bd59-175">Filter on log traces, or on items with specific properties</span></span>
* <span data-ttu-id="7bd59-176">Zkontrolujte konkrétní položku podrobně.</span><span class="sxs-lookup"><span data-stu-id="7bd59-176">Inspect a specific item in detail.</span></span>
* <span data-ttu-id="7bd59-177">Najít další telemetrii týkající se toohello stejné požadavku uživatele (to znamená, s hello stejné OperationId)</span><span class="sxs-lookup"><span data-stu-id="7bd59-177">Find other telemetry relating toohello same user request (that is, with hello same OperationId)</span></span>
* <span data-ttu-id="7bd59-178">Konfiguraci hello části této stránky uložit jako oblíbenou položku</span><span class="sxs-lookup"><span data-stu-id="7bd59-178">Save hello configuration of this page as a Favorite</span></span>

> [!NOTE]
> <span data-ttu-id="7bd59-179">**Vzorkování.**</span><span class="sxs-lookup"><span data-stu-id="7bd59-179">**Sampling.**</span></span> <span data-ttu-id="7bd59-180">Pokud vaše aplikace odešle velké množství dat a používáte hello Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="7bd59-180">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="7bd59-181">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-181">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <a name="next-steps"></a><span data-ttu-id="7bd59-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7bd59-182">Next steps</span></span>
<span data-ttu-id="7bd59-183">[Diagnostikovat chyby a výjimky technologie ASP.NET][exceptions]</span><span class="sxs-lookup"><span data-stu-id="7bd59-183">[Diagnose failures and exceptions in ASP.NET][exceptions]</span></span>

<span data-ttu-id="7bd59-184">[Další informace o vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="7bd59-184">[Learn more about Search][diagnostic].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7bd59-185">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7bd59-185">Troubleshooting</span></span>
### <a name="how-do-i-do-this-for-java"></a><span data-ttu-id="7bd59-186">Jak se to pro jazyk Java?</span><span class="sxs-lookup"><span data-stu-id="7bd59-186">How do I do this for Java?</span></span>
<span data-ttu-id="7bd59-187">Použití hello [adaptéry protokolu Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="7bd59-187">Use hello [Java log adapters](app-insights-java-trace-logs.md).</span></span>

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a><span data-ttu-id="7bd59-188">Neexistuje žádná možnost Application Insights na místní nabídku hello projektu</span><span class="sxs-lookup"><span data-stu-id="7bd59-188">There's no Application Insights option on hello project context menu</span></span>
* <span data-ttu-id="7bd59-189">Kontrola instalace nástrojů Application Insights na tomto počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="7bd59-189">Check Application Insights tools is installed on this development machine.</span></span> <span data-ttu-id="7bd59-190">V nabídce Nástroje aplikace Visual Studio, rozšíření a aktualizace vyhledejte nástroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="7bd59-190">In Visual Studio menu Tools, Extensions and Updates, look for Application Insights Tools.</span></span> <span data-ttu-id="7bd59-191">Pokud není na kartě hello nainstalovaná, otevřete kartu hello Online a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="7bd59-191">If it isn't in hello Installed tab, open hello Online tab and install it.</span></span>
* <span data-ttu-id="7bd59-192">To může být typ projektu není podporováno nástrojů Application Insights.</span><span class="sxs-lookup"><span data-stu-id="7bd59-192">This might be a type of project not supported by Application Insights tools.</span></span> <span data-ttu-id="7bd59-193">Použití [ruční instalace](#manual-installation).</span><span class="sxs-lookup"><span data-stu-id="7bd59-193">Use [manual installation](#manual-installation).</span></span>

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a><span data-ttu-id="7bd59-194">Žádná možnost pro adaptér protokolu v konfiguračním nástroji služby hello</span><span class="sxs-lookup"><span data-stu-id="7bd59-194">No log adapter option in hello configuration tool</span></span>
* <span data-ttu-id="7bd59-195">Nejdřív musíte rozhraní tooinstall hello protokolování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-195">You need tooinstall hello logging framework first.</span></span>
* <span data-ttu-id="7bd59-196">Pokud používáte System.Diagnostics.Trace, ověřte, že je [nakonfigurované v `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd59-196">If you're using System.Diagnostics.Trace, make sure you [configured it in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span></span>
* <span data-ttu-id="7bd59-197">Mít jste získali nejnovější verzi hello Application Insights?</span><span class="sxs-lookup"><span data-stu-id="7bd59-197">Have you got hello latest version of Application Insights?</span></span> <span data-ttu-id="7bd59-198">V sadě Visual Studio **nástroje** nabídce zvolte **rozšíření a aktualizace**a otevřete hello **aktualizace** kartě. Pokud existuje Developer Analytics tools, klikněte na tlačítko tooupdate ho.</span><span class="sxs-lookup"><span data-stu-id="7bd59-198">In Visual Studio **Tools** menu, choose **Extensions and Updates**, and open hello **Updates** tab. If Developer Analytics tools is there, click tooupdate it.</span></span>

### <span data-ttu-id="7bd59-199"><a name="emptykey"></a>Dojde k chybě "klíč instrumentace nemůže být prázdný"</span><span class="sxs-lookup"><span data-stu-id="7bd59-199"><a name="emptykey"></a>I get an error "Instrumentation key cannot be empty"</span></span>
<span data-ttu-id="7bd59-200">Zdá se, zda jste nainstalovali hello protokolování balíček Nuget adaptér bez instalace služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="7bd59-200">Looks like you installed hello logging adapter Nuget package without installing Application Insights.</span></span>

<span data-ttu-id="7bd59-201">V Průzkumníku řešení klikněte pravým tlačítkem na `ApplicationInsights.config` a zvolte **aktualizace Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="7bd59-201">In Solution Explorer, right-click `ApplicationInsights.config` and choose **Update Application Insights**.</span></span> <span data-ttu-id="7bd59-202">Se vám zobrazí dialogové okno se žádostí můžete toosign v tooAzure a buď vytvořte prostředek Application Insights, nebo znovu použijte stávající.</span><span class="sxs-lookup"><span data-stu-id="7bd59-202">You'll get a dialog that invites you toosign in tooAzure and either create an Application Insights resource, or re-use an existing one.</span></span> <span data-ttu-id="7bd59-203">Který by měl opravte ji.</span><span class="sxs-lookup"><span data-stu-id="7bd59-203">That should fix it.</span></span>

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a><span data-ttu-id="7bd59-204">I najdete v trasování v diagnostické vyhledávání, ale není hello dalších událostí</span><span class="sxs-lookup"><span data-stu-id="7bd59-204">I can see traces in diagnostic search, but not hello other events</span></span>
<span data-ttu-id="7bd59-205">Někdy může trvat nějakou dobu všechny hello události a žádosti o tooget kanálem hello.</span><span class="sxs-lookup"><span data-stu-id="7bd59-205">It can sometimes take a while for all hello events and requests tooget through hello pipeline.</span></span>

### <span data-ttu-id="7bd59-206"><a name="limits"></a>Množství dat, které se uchovávají?</span><span class="sxs-lookup"><span data-stu-id="7bd59-206"><a name="limits"></a>How much data is retained?</span></span>
<span data-ttu-id="7bd59-207">Několik různých faktorů vliv hello množství dat, které jsou zachovány.</span><span class="sxs-lookup"><span data-stu-id="7bd59-207">Several factors impact hello amount of data retained.</span></span> <span data-ttu-id="7bd59-208">V tématu hello [omezení](app-insights-api-custom-events-metrics.md#limits) oddílu hello zákazníka události metriky stránky pro další informace.</span><span class="sxs-lookup"><span data-stu-id="7bd59-208">See hello [limits](app-insights-api-custom-events-metrics.md#limits) section of hello customer event metrics page for more information.</span></span> 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a><span data-ttu-id="7bd59-209">Nejsou zobrazeny některé hello položky protokolu, které očekávat</span><span class="sxs-lookup"><span data-stu-id="7bd59-209">I'm not seeing some of hello log entries that I expect</span></span>
<span data-ttu-id="7bd59-210">Pokud vaše aplikace odešle velké množství dat a používáte hello Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="7bd59-210">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="7bd59-211">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="7bd59-211">Learn more about sampling.</span></span>](app-insights-sampling.md)

## <span data-ttu-id="7bd59-212"><a name="add"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="7bd59-212"><a name="add"></a>Next steps</span></span>
* <span data-ttu-id="7bd59-213">[Nastavení dostupnosti a odezvy testů][availability]</span><span class="sxs-lookup"><span data-stu-id="7bd59-213">[Set up availability and responsiveness tests][availability]</span></span>
* <span data-ttu-id="7bd59-214">[Řešení potíží][qna]</span><span class="sxs-lookup"><span data-stu-id="7bd59-214">[Troubleshooting][qna]</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
