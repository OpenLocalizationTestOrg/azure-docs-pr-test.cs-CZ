---
title: "aaaApplication rozhraní API pro přehledy pro vlastní události a metriky | Microsoft Docs"
description: "Vložte po zadání několika řádků kódu do vašeho zařízení nebo aplikace na ploše, webové stránky nebo služby, tootrack využití a diagnostikovat problémy."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a><span data-ttu-id="b045e-103">Application Insights API pro vlastní události a metriky</span><span class="sxs-lookup"><span data-stu-id="b045e-103">Application Insights API for custom events and metrics</span></span>

<span data-ttu-id="b045e-104">Vložte po zadání několika řádků kódu do vaší aplikace toofind se co uživatelé dělají s ním nebo toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="b045e-104">Insert a few lines of code in your application toofind out what users are doing with it, or toohelp diagnose issues.</span></span> <span data-ttu-id="b045e-105">Odesílat telemetrická data z aplikace zařízení a vzdálené ploše, webovými klienty a webové servery.</span><span class="sxs-lookup"><span data-stu-id="b045e-105">You can send telemetry from device and desktop apps, web clients, and web servers.</span></span> <span data-ttu-id="b045e-106">Použití hello [Azure Application Insights](app-insights-overview.md) základní rozhraní API telemetrie toosend vlastní události a metriky a vlastní verzích standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="b045e-106">Use hello [Azure Application Insights](app-insights-overview.md) core telemetry API toosend custom events and metrics, and your own versions of standard telemetry.</span></span> <span data-ttu-id="b045e-107">Toto rozhraní API je hello stejné rozhraní API tohoto standardního hello Application Insights sběrače dat použít.</span><span class="sxs-lookup"><span data-stu-id="b045e-107">This API is hello same API that hello standard Application Insights data collectors use.</span></span>

## <a name="api-summary"></a><span data-ttu-id="b045e-108">Souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b045e-108">API summary</span></span>
<span data-ttu-id="b045e-109">Hello rozhraní API je uniform pro všechny platformy kromě několik malé rozdíly.</span><span class="sxs-lookup"><span data-stu-id="b045e-109">hello API is uniform across all platforms, apart from a few small variations.</span></span>

| <span data-ttu-id="b045e-110">Metoda</span><span class="sxs-lookup"><span data-stu-id="b045e-110">Method</span></span> | <span data-ttu-id="b045e-111">Použít pro</span><span class="sxs-lookup"><span data-stu-id="b045e-111">Used for</span></span> |
| --- | --- |
| [`TrackPageView`](#page-views) |<span data-ttu-id="b045e-112">Stránky, obrazovky, okna nebo formuláře.</span><span class="sxs-lookup"><span data-stu-id="b045e-112">Pages, screens, blades, or forms.</span></span> |
| [`TrackEvent`](#trackevent) |<span data-ttu-id="b045e-113">Akce uživatelů a dalších událostí.</span><span class="sxs-lookup"><span data-stu-id="b045e-113">User actions and other events.</span></span> <span data-ttu-id="b045e-114">Použít tootrack uživatele chování nebo toomonitor výkonu.</span><span class="sxs-lookup"><span data-stu-id="b045e-114">Used tootrack user behavior or toomonitor performance.</span></span> |
| [`TrackMetric`](#trackmetric) |<span data-ttu-id="b045e-115">Měření výkonu, jako je například délky fronty nesouvisejí toospecific události.</span><span class="sxs-lookup"><span data-stu-id="b045e-115">Performance measurements such as queue lengths not related toospecific events.</span></span> |
| [`TrackException`](#trackexception) |<span data-ttu-id="b045e-116">Protokolování výjimky pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="b045e-116">Logging exceptions for diagnosis.</span></span> <span data-ttu-id="b045e-117">Sledování, kde ve vztahu tooother událostí a zkontrolujte trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="b045e-117">Trace where they occur in relation tooother events and examine stack traces.</span></span> |
| [`TrackRequest`](#trackrequest) |<span data-ttu-id="b045e-118">Protokolování hello četnost a dobu trvání požadavky serveru pro analýzu výkonu.</span><span class="sxs-lookup"><span data-stu-id="b045e-118">Logging hello frequency and duration of server requests for performance analysis.</span></span> |
| [`TrackTrace`](#tracktrace) |<span data-ttu-id="b045e-119">Zprávy protokolů diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b045e-119">Diagnostic log messages.</span></span> <span data-ttu-id="b045e-120">Také můžete zaznamenat protokoly třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b045e-120">You can also capture third-party logs.</span></span> |
| [`TrackDependency`](#trackdependency) |<span data-ttu-id="b045e-121">Doba trvání hello protokolování a četnost volání tooexternal komponent, které závisí aplikace na.</span><span class="sxs-lookup"><span data-stu-id="b045e-121">Logging hello duration and frequency of calls tooexternal components that your app depends on.</span></span> |

<span data-ttu-id="b045e-122">Můžete [připojení vlastnosti a metriky](#properties) toomost těchto volání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="b045e-122">You can [attach properties and metrics](#properties) toomost of these telemetry calls.</span></span>

## <span data-ttu-id="b045e-123"><a name="prep"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="b045e-123"><a name="prep"></a>Before you start</span></span>
<span data-ttu-id="b045e-124">Pokud nemáte k dispozici odkaz na Application Insights SDK ještě:</span><span class="sxs-lookup"><span data-stu-id="b045e-124">If you don't have a reference on Application Insights SDK yet:</span></span>

* <span data-ttu-id="b045e-125">Přidejte hello Application Insights SDK tooyour projektu:</span><span class="sxs-lookup"><span data-stu-id="b045e-125">Add hello Application Insights SDK tooyour project:</span></span>

  * [<span data-ttu-id="b045e-126">Projekt ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b045e-126">ASP.NET project</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="b045e-127">Projektu Java</span><span class="sxs-lookup"><span data-stu-id="b045e-127">Java project</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="b045e-128">JavaScript v každé webové stránky</span><span class="sxs-lookup"><span data-stu-id="b045e-128">JavaScript in each webpage</span></span>](app-insights-javascript.md) 
* <span data-ttu-id="b045e-129">V zařízení nebo webový server kódu patří:</span><span class="sxs-lookup"><span data-stu-id="b045e-129">In your device or web server code, include:</span></span>

    <span data-ttu-id="b045e-130">*C#:*`using Microsoft.ApplicationInsights;`</span><span class="sxs-lookup"><span data-stu-id="b045e-130">*C#:* `using Microsoft.ApplicationInsights;`</span></span>

    <span data-ttu-id="b045e-131">*Visual Basic:*`Imports Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="b045e-131">*Visual Basic:* `Imports Microsoft.ApplicationInsights`</span></span>

    <span data-ttu-id="b045e-132">*Java:*`import com.microsoft.applicationinsights.TelemetryClient;`</span><span class="sxs-lookup"><span data-stu-id="b045e-132">*Java:* `import com.microsoft.applicationinsights.TelemetryClient;`</span></span>

## <a name="constructing-a-telemetryclient-instance"></a><span data-ttu-id="b045e-133">Vytváření TelemetryClient instance</span><span class="sxs-lookup"><span data-stu-id="b045e-133">Constructing a TelemetryClient instance</span></span>
<span data-ttu-id="b045e-134">Vytvořit instanci `TelemetryClient` (s výjimkou v jazyce JavaScript webové stránky):</span><span class="sxs-lookup"><span data-stu-id="b045e-134">Construct an instance of `TelemetryClient` (except in JavaScript in webpages):</span></span>

<span data-ttu-id="b045e-135">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-135">*C#*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="b045e-136">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-136">*Visual Basic*</span></span>

    Private Dim telemetry As New TelemetryClient

<span data-ttu-id="b045e-137">*Java*</span><span class="sxs-lookup"><span data-stu-id="b045e-137">*Java*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="b045e-138">TelemetryClient je bezpečné pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="b045e-138">TelemetryClient is thread-safe.</span></span>

<span data-ttu-id="b045e-139">Doporučujeme použít instanci TelemetryClient pro každý modul vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b045e-139">We recommend that you use an instance of TelemetryClient for each module of your app.</span></span> <span data-ttu-id="b045e-140">Například můžete mít jednu instanci TelemetryClient ve vaší žádosti webové služby tooreport příchozí HTTP a druhý v události middleware třídy tooreport obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="b045e-140">For instance, you may have one TelemetryClient instance in your web service tooreport incoming HTTP requests, and another in a middleware class tooreport business logic events.</span></span> <span data-ttu-id="b045e-141">Můžete například nastavit vlastnosti `TelemetryClient.Context.User.Id` tootrack uživatelů a relací, nebo `TelemetryClient.Context.Device.Id` tooidentify hello počítače.</span><span class="sxs-lookup"><span data-stu-id="b045e-141">You can set properties such as `TelemetryClient.Context.User.Id` tootrack users and sessions, or `TelemetryClient.Context.Device.Id` tooidentify hello machine.</span></span> <span data-ttu-id="b045e-142">Tyto informace jsou připojené tooall události, které hello zasílá instance.</span><span class="sxs-lookup"><span data-stu-id="b045e-142">This information is attached tooall events that hello instance sends.</span></span>

## <a name="trackevent"></a><span data-ttu-id="b045e-143">TrackEvent</span><span class="sxs-lookup"><span data-stu-id="b045e-143">TrackEvent</span></span>
<span data-ttu-id="b045e-144">Ve službě Application Insights *vlastní události* je datový bod, který můžete zobrazit v [Průzkumníku metrik](app-insights-metrics-explorer.md) jako agregovaného počtu a v [diagnostické vyhledávání](app-insights-diagnostic-search.md) jako jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="b045e-144">In Application Insights, a *custom event* is a data point that you can display in [Metrics Explorer](app-insights-metrics-explorer.md) as an aggregated count, and in [Diagnostic Search](app-insights-diagnostic-search.md) as individual occurrences.</span></span> <span data-ttu-id="b045e-145">(Není související tooMVC nebo jiných framework "události.")</span><span class="sxs-lookup"><span data-stu-id="b045e-145">(It isn't related tooMVC or other framework "events.")</span></span>

<span data-ttu-id="b045e-146">Vložit `TrackEvent` volání do vašeho kódu toocount různé události.</span><span class="sxs-lookup"><span data-stu-id="b045e-146">Insert `TrackEvent` calls in your code toocount various events.</span></span> <span data-ttu-id="b045e-147">Jak často uživatelé vybrat konkrétní funkce, jak často budou dosáhnout určité cíle nebo možná četnosti provádění konkrétní typy chyb.</span><span class="sxs-lookup"><span data-stu-id="b045e-147">How often users choose a particular feature, how often they achieve particular goals, or maybe how often they make particular types of mistakes.</span></span>

<span data-ttu-id="b045e-148">Herní aplikace, například odeslání události vždy, když uživatel wins herní hello:</span><span class="sxs-lookup"><span data-stu-id="b045e-148">For example, in a game app, send an event whenever a user wins hello game:</span></span>

<span data-ttu-id="b045e-149">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-149">*JavaScript*</span></span>

    appInsights.trackEvent("WinGame");

<span data-ttu-id="b045e-150">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-150">*C#*</span></span>

    telemetry.TrackEvent("WinGame");

<span data-ttu-id="b045e-151">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-151">*Visual Basic*</span></span>

    telemetry.TrackEvent("WinGame")

<span data-ttu-id="b045e-152">*Java*</span><span class="sxs-lookup"><span data-stu-id="b045e-152">*Java*</span></span>

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a><span data-ttu-id="b045e-153">Zobrazit události v portálu Microsoft Azure hello</span><span class="sxs-lookup"><span data-stu-id="b045e-153">View your events in hello Microsoft Azure portal</span></span>
<span data-ttu-id="b045e-154">toosee počet událostí, otevřete [Průzkumníku metrik](app-insights-metrics-explorer.md) okně přidejte nový graf a vyberte **události**.</span><span class="sxs-lookup"><span data-stu-id="b045e-154">toosee a count of your events, open a [Metrics Explorer](app-insights-metrics-explorer.md) blade, add a new chart, and select **Events**.</span></span>  

![Zobrazí počet vlastní události](./media/app-insights-api-custom-events-metrics/01-custom.png)

<span data-ttu-id="b045e-156">počty hello toocompare různých událostí, nastavte typ grafu hello příliš**mřížky**a skupinu podle názvu událostí:</span><span class="sxs-lookup"><span data-stu-id="b045e-156">toocompare hello counts of different events, set hello chart type too**Grid**, and group by event name:</span></span>

![Nastavte typ grafu hello a seskupení](./media/app-insights-api-custom-events-metrics/07-grid.png)

<span data-ttu-id="b045e-158">Na hello mřížky klikněte na tlačítko prostřednictvím události název toosee jednotlivé výskyty této události.</span><span class="sxs-lookup"><span data-stu-id="b045e-158">On hello grid, click through an event name toosee individual occurrences of that event.</span></span> <span data-ttu-id="b045e-159">toosee více podrobností – klikněte na kterýkoli z výskytů v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-159">toosee more detail - click any occurrence in hello list.</span></span>

![Procházení událostí hello](./media/app-insights-api-custom-events-metrics/03-instances.png)

<span data-ttu-id="b045e-161">toofocus na konkrétní události ve vyhledávání nebo Průzkumníku metrik okno hello sada filtru toohello událostí názvy, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="b045e-161">toofocus on specific events in either Search or Metrics Explorer, set hello blade's filter toohello event names that you're interested in:</span></span>

![Otevřete filtry, rozbalte název události a vyberte jednu nebo více hodnot](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a><span data-ttu-id="b045e-163">Vlastní události v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-163">Custom events in Analytics</span></span>

<span data-ttu-id="b045e-164">telemetrie Hello je k dispozici v hello `customEvents` tabulky v [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-164">hello telemetry is available in hello `customEvents` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="b045e-165">Každý řádek představuje volání příliš`trackEvent(..)` ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b045e-165">Each row represents a call too`trackEvent(..)` in your app.</span></span> 

<span data-ttu-id="b045e-166">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="b045e-166">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="b045e-167">Pro příklad itemCount == 10 znamená, že z 10 volání tootrackEvent() hello vzorkování proces přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="b045e-167">For example itemCount==10 means that of 10 calls tootrackEvent(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="b045e-168">tooget správný počet vlastních událostí, měli byste použít proto použít kód jako `customEvent | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="b045e-168">tooget a correct count of custom events, you should use therefore use code such as `customEvent | summarize sum(itemCount)`.</span></span>


## <a name="trackmetric"></a><span data-ttu-id="b045e-169">TrackMetric</span><span class="sxs-lookup"><span data-stu-id="b045e-169">TrackMetric</span></span>

<span data-ttu-id="b045e-170">Application Insights můžete grafu metriky, které nejsou připojené tooparticular události.</span><span class="sxs-lookup"><span data-stu-id="b045e-170">Application Insights can chart metrics that are not attached tooparticular events.</span></span> <span data-ttu-id="b045e-171">Délka fronty může například sledovat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="b045e-171">For example, you could monitor a queue length at regular intervals.</span></span> <span data-ttu-id="b045e-172">O metriky jednotlivými měřeními hello jsou méně důležité než hello variace a trendy a proto statistické grafy jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="b045e-172">With metrics, hello individual measurements are of less interest than hello variations and trends, and so statistical charts are useful.</span></span>

<span data-ttu-id="b045e-173">V pořadí toosend metriky tooApplication statistiky, můžete použít hello `TrackMetric(..)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b045e-173">In order toosend metrics tooApplication Insights, you can use hello `TrackMetric(..)` API.</span></span> <span data-ttu-id="b045e-174">Existují dva způsoby toosend metriky:</span><span class="sxs-lookup"><span data-stu-id="b045e-174">There are two ways toosend a metric:</span></span> 

* <span data-ttu-id="b045e-175">Jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b045e-175">Single value.</span></span> <span data-ttu-id="b045e-176">Pokaždé, když provedete měření ve vaší aplikaci, můžete odeslat hello odpovídající hodnotu tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="b045e-176">Every time you perform a measurement in your application, you send hello corresponding value tooApplication Insights.</span></span> <span data-ttu-id="b045e-177">Předpokládejme například, že máte metriky popisující hello počet položek v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b045e-177">For example, assume that you have a metric describing hello number of items in a container.</span></span> <span data-ttu-id="b045e-178">V konkrétním časovém období můžete poprvé tři položky do kontejneru hello a potom odeberte dvě položky.</span><span class="sxs-lookup"><span data-stu-id="b045e-178">During a particular time period, you first put three items into hello container and then you remove two items.</span></span> <span data-ttu-id="b045e-179">Podle toho by volání `TrackMetric` dvakrát: první předání hello hodnotu `3` a pak hello hodnotu `-2`.</span><span class="sxs-lookup"><span data-stu-id="b045e-179">Accordingly, you would call `TrackMetric` twice: first passing hello value `3` and then hello value `-2`.</span></span> <span data-ttu-id="b045e-180">Application Insights ukládá obě hodnoty vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="b045e-180">Application Insights stores both values on your behalf.</span></span> 

* <span data-ttu-id="b045e-181">Agregace.</span><span class="sxs-lookup"><span data-stu-id="b045e-181">Aggregation.</span></span> <span data-ttu-id="b045e-182">Při práci s metriky, je každé jedno měření zájmu zřídka.</span><span class="sxs-lookup"><span data-stu-id="b045e-182">When working with metrics, every single measurement is rarely of interest.</span></span> <span data-ttu-id="b045e-183">Místo toho je důležité souhrn co se stalo v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="b045e-183">Instead a summary of what happened during a particular time period is important.</span></span> <span data-ttu-id="b045e-184">Takové souhrn nazývá _agregace_.</span><span class="sxs-lookup"><span data-stu-id="b045e-184">Such a summary is called _aggregation_.</span></span> <span data-ttu-id="b045e-185">V hello výše příklad hello agregační metriky součet za toto období je `1` a hello počet hodnot metriky hello je `2`.</span><span class="sxs-lookup"><span data-stu-id="b045e-185">In hello above example, hello aggregate metric sum for that time period is `1` and hello count of hello metric values is `2`.</span></span> <span data-ttu-id="b045e-186">Při použití hello agregace přístup, pouze vyvolání `TrackMetric` jednou za časové období a odesílání hello agregované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b045e-186">When using hello aggregation approach, you only invoke `TrackMetric` once per time period and send hello aggregate values.</span></span> <span data-ttu-id="b045e-187">Toto je hello doporučenému přístupu vzhledem k tomu může významně snížit náklady na hello a výkonu režie odesláním méně dat. bodů tooApplication přehledy, a stále shromažďovat všechny relevantní informace.</span><span class="sxs-lookup"><span data-stu-id="b045e-187">This is hello recommended approach since it can significantly reduce hello cost and performance overhead by sending fewer data points tooApplication Insights, while still collecting all relevant information.</span></span>

### <a name="examples"></a><span data-ttu-id="b045e-188">Příklady:</span><span class="sxs-lookup"><span data-stu-id="b045e-188">Examples:</span></span>

#### <a name="single-values"></a><span data-ttu-id="b045e-189">Jednotlivé hodnoty</span><span class="sxs-lookup"><span data-stu-id="b045e-189">Single values</span></span>

<span data-ttu-id="b045e-190">toosend jednu hodnotu metriky:</span><span class="sxs-lookup"><span data-stu-id="b045e-190">toosend a single metric value:</span></span>

<span data-ttu-id="b045e-191">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-191">*JavaScript*</span></span>

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

<span data-ttu-id="b045e-192">*C#, Java*</span><span class="sxs-lookup"><span data-stu-id="b045e-192">*C#, Java*</span></span>

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a><span data-ttu-id="b045e-193">Agregování metriky</span><span class="sxs-lookup"><span data-stu-id="b045e-193">Aggregating metrics</span></span>

<span data-ttu-id="b045e-194">Před odesláním z vaší aplikace, tooreduce šířky pásma, náklady a tooimprove výkonu doporučujeme tooaggregate metriky.</span><span class="sxs-lookup"><span data-stu-id="b045e-194">It is recommended tooaggregate metrics before sending them from your app, tooreduce bandwidth, cost and tooimprove performance.</span></span>
<span data-ttu-id="b045e-195">Tady je příklad totožný kódu:</span><span class="sxs-lookup"><span data-stu-id="b045e-195">Here is an example of aggregating code:</span></span>

<span data-ttu-id="b045e-196">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-196">*C#*</span></span>

```C#
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a><span data-ttu-id="b045e-197">Vlastní metriky v Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="b045e-197">Custom metrics in Metrics Explorer</span></span>

<span data-ttu-id="b045e-198">výsledky hello toosee, otevřete Průzkumníka metrik a přidejte nový graf.</span><span class="sxs-lookup"><span data-stu-id="b045e-198">toosee hello results, open Metrics Explorer and add a new chart.</span></span> <span data-ttu-id="b045e-199">Upravte graf tooshow hello vaší metriku.</span><span class="sxs-lookup"><span data-stu-id="b045e-199">Edit hello chart tooshow your metric.</span></span>

> [!NOTE]
> <span data-ttu-id="b045e-200">Vlastní metriku může trvat několik minut tooappear hello seznamu dostupné metriky.</span><span class="sxs-lookup"><span data-stu-id="b045e-200">Your custom metric might take several minutes tooappear in hello list of available metrics.</span></span>
>

![Přidejte nový graf nebo vyberte graf a v části vlastní, vyberte vaše metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a><span data-ttu-id="b045e-202">Vlastní metriky v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-202">Custom metrics in Analytics</span></span>

<span data-ttu-id="b045e-203">telemetrie Hello je k dispozici v hello `customMetrics` tabulky v [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-203">hello telemetry is available in hello `customMetrics` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="b045e-204">Každý řádek představuje volání příliš`trackMetric(..)` ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b045e-204">Each row represents a call too`trackMetric(..)` in your app.</span></span>
* <span data-ttu-id="b045e-205">`valueSum`-Toto je součet hello hello měření.</span><span class="sxs-lookup"><span data-stu-id="b045e-205">`valueSum` - This is hello sum of hello measurements.</span></span> <span data-ttu-id="b045e-206">tooget hello střední hodnoty, dělení podle `valueCount`.</span><span class="sxs-lookup"><span data-stu-id="b045e-206">tooget hello mean value, divide by `valueCount`.</span></span>
* <span data-ttu-id="b045e-207">`valueCount`-hello počet měření, které byly agregovat do této `trackMetric(..)` volání.</span><span class="sxs-lookup"><span data-stu-id="b045e-207">`valueCount` - hello number of measurements that were aggregated into this `trackMetric(..)` call.</span></span>

## <a name="page-views"></a><span data-ttu-id="b045e-208">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="b045e-208">Page views</span></span>
<span data-ttu-id="b045e-209">V aplikaci pomocí zařízení nebo webové stránky je odeslána telemetrická zobrazení stránky ve výchozím nastavení při načtení každé obrazovky nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="b045e-209">In a device or webpage app, page view telemetry is sent by default when each screen or page is loaded.</span></span> <span data-ttu-id="b045e-210">Ale na další nebo jinou dobu můžete změnit této tootrack zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="b045e-210">But you can change that tootrack page views at additional or different times.</span></span> <span data-ttu-id="b045e-211">Například v aplikaci, která zobrazí karty nebo okna, můžete tootrack stránky vždy, když uživatel hello otevře nové okno.</span><span class="sxs-lookup"><span data-stu-id="b045e-211">For example, in an app that displays tabs or blades, you might want tootrack a page whenever hello user opens a new blade.</span></span>

![Použití přehledu v okně Přehled](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

<span data-ttu-id="b045e-213">Data uživatele a relace je odeslána jako vlastnosti společně s zobrazení stránky, takže hello uživatele a relace grafy pocházet zachování připojení při telemetrická zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="b045e-213">User and session data is sent as properties along with page views, so hello user and session charts come alive when there is page view telemetry.</span></span>

### <a name="custom-page-views"></a><span data-ttu-id="b045e-214">Zobrazení vlastních stránek</span><span class="sxs-lookup"><span data-stu-id="b045e-214">Custom page views</span></span>
<span data-ttu-id="b045e-215">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-215">*JavaScript*</span></span>

    appInsights.trackPageView("tab1");

<span data-ttu-id="b045e-216">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-216">*C#*</span></span>

    telemetry.TrackPageView("GameReviewPage");

<span data-ttu-id="b045e-217">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-217">*Visual Basic*</span></span>

    telemetry.TrackPageView("GameReviewPage")


<span data-ttu-id="b045e-218">Pokud máte několik karet v rámci jiné stránky HTML, můžete zadat adresu URL hello příliš:</span><span class="sxs-lookup"><span data-stu-id="b045e-218">If you have several tabs within different HTML pages, you can specify hello URL too:</span></span>

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a><span data-ttu-id="b045e-219">Zobrazení stránky časování</span><span class="sxs-lookup"><span data-stu-id="b045e-219">Timing page views</span></span>
<span data-ttu-id="b045e-220">Ve výchozím nastavení, časy hello hlášené jako **zobrazení času načítání stránky** se měří z při hello prohlížeč odešle požadavek hello, dokud se nazývá hello prohlížeče událostí načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="b045e-220">By default, hello times reported as **Page view load time** are measured from when hello browser sends hello request, until hello browser's page load event is called.</span></span>

<span data-ttu-id="b045e-221">Místo toho můžete buď:</span><span class="sxs-lookup"><span data-stu-id="b045e-221">Instead, you can either:</span></span>

* <span data-ttu-id="b045e-222">Nastavit explicitní doba trvání v hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) volání: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span><span class="sxs-lookup"><span data-stu-id="b045e-222">Set an explicit duration in hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) call: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span></span>
* <span data-ttu-id="b045e-223">Použít hello stránky zobrazení časování volání `startTrackPage` a `stopTrackPage`.</span><span class="sxs-lookup"><span data-stu-id="b045e-223">Use hello page view timing calls `startTrackPage` and `stopTrackPage`.</span></span>

<span data-ttu-id="b045e-224">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-224">*JavaScript*</span></span>

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

<span data-ttu-id="b045e-225">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="b045e-225">...</span></span>

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

<span data-ttu-id="b045e-226">Hello název, který použijete jako první parametr hello přidruží hello spuštění a zastavení volání.</span><span class="sxs-lookup"><span data-stu-id="b045e-226">hello name that you use as hello first parameter associates hello start and stop calls.</span></span> <span data-ttu-id="b045e-227">Výchozí hodnota toohello název aktuální stránky.</span><span class="sxs-lookup"><span data-stu-id="b045e-227">It defaults toohello current page name.</span></span>

<span data-ttu-id="b045e-228">Výsledný načtení stránky Hello doby trvání zobrazí v Průzkumníku metrik, které jsou odvozeny od hello interval mezi hello spuštění a zastavení volání.</span><span class="sxs-lookup"><span data-stu-id="b045e-228">hello resulting page load durations displayed in Metrics Explorer are derived from hello interval between hello start and stop calls.</span></span> <span data-ttu-id="b045e-229">Je to tooyou jaké interval, ve skutečnosti čas.</span><span class="sxs-lookup"><span data-stu-id="b045e-229">It's up tooyou what interval you actually time.</span></span>

### <a name="page-telemetry-in-analytics"></a><span data-ttu-id="b045e-230">Stránka telemetrie Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-230">Page telemetry in Analytics</span></span>

<span data-ttu-id="b045e-231">V [Analytics](app-insights-analytics.md) dvou tabulek zobrazit data z prohlížeče operace:</span><span class="sxs-lookup"><span data-stu-id="b045e-231">In [Analytics](app-insights-analytics.md) two tables show data from browser operations:</span></span>

* <span data-ttu-id="b045e-232">Hello `pageViews` tabulka obsahuje data o název adresy URL a stránku hello</span><span class="sxs-lookup"><span data-stu-id="b045e-232">hello `pageViews` table contains data about hello URL and page title</span></span>
* <span data-ttu-id="b045e-233">Hello `browserTimings` tabulka obsahuje data o výkonu klienta, jako je doba tooprocess hello hello příchozích dat</span><span class="sxs-lookup"><span data-stu-id="b045e-233">hello `browserTimings` table contains data about client performance, such as hello time taken tooprocess hello incoming data</span></span>

<span data-ttu-id="b045e-234">toofind jak dlouho hello prohlížeče trvá tooprocess různé stránky:</span><span class="sxs-lookup"><span data-stu-id="b045e-234">toofind how long hello browser takes tooprocess different pages:</span></span>

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

<span data-ttu-id="b045e-235">toodiscover hello popularities různých prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="b045e-235">toodiscover hello popularities of different browsers:</span></span>

```
pageViews | summarize count() by client_Browser
```

<span data-ttu-id="b045e-236">tooassociate stránky zobrazení tooAJAX volání, připojení k závislosti:</span><span class="sxs-lookup"><span data-stu-id="b045e-236">tooassociate page views tooAJAX calls, join with dependencies:</span></span>

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a><span data-ttu-id="b045e-237">TrackRequest</span><span class="sxs-lookup"><span data-stu-id="b045e-237">TrackRequest</span></span>
<span data-ttu-id="b045e-238">požadavky HTTP toolog TrackRequest používá Hello server SDK.</span><span class="sxs-lookup"><span data-stu-id="b045e-238">hello server SDK uses TrackRequest toolog HTTP requests.</span></span>

<span data-ttu-id="b045e-239">Můžete také volat ho sami Pokud chcete, aby toosimulate požadavky v kontextu, kde nemáte hello webové služby modul spuštěn.</span><span class="sxs-lookup"><span data-stu-id="b045e-239">You can also call it yourself if you want toosimulate requests in a context where you don't have hello web service module running.</span></span>

<span data-ttu-id="b045e-240">Telemetrie požadavku toosend způsob, jak je, kde hello požadavek funguje jako však doporučeno hello <a href="#operation-context">operační kontext</a>.</span><span class="sxs-lookup"><span data-stu-id="b045e-240">However, hello recommended way toosend request telemetry is where hello request acts as an <a href="#operation-context">operation context</a>.</span></span>

## <a name="operation-context"></a><span data-ttu-id="b045e-241">Operace kontextu</span><span class="sxs-lookup"><span data-stu-id="b045e-241">Operation context</span></span>
<span data-ttu-id="b045e-242">Telemetrie položky můžete přidružit společně připojením toothem běžné ID operace.</span><span class="sxs-lookup"><span data-stu-id="b045e-242">You can associate telemetry items together by attaching toothem a common operation ID.</span></span> <span data-ttu-id="b045e-243">Standardní modulu Sledování žádostí o Hello k tomu pro výjimky a dalších událostí, které se odesílají během zpracování požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="b045e-243">hello standard request-tracking module does this for exceptions and other events that are sent while an HTTP request is being processed.</span></span> <span data-ttu-id="b045e-244">V [vyhledávání](app-insights-diagnostic-search.md) a [Analytics](app-insights-analytics.md), můžete použít hello ID tooeasily najít všechny události přidružené hello žádosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-244">In [Search](app-insights-diagnostic-search.md) and [Analytics](app-insights-analytics.md), you can use hello ID tooeasily find any events associated with hello request.</span></span>

<span data-ttu-id="b045e-245">Hello nejjednodušší způsob, jak tooset hello ID je tooset kontextu operace pomocí tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="b045e-245">hello easiest way tooset hello ID is tooset an operation context by using this pattern:</span></span>

<span data-ttu-id="b045e-246">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-246">*C#*</span></span>

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

<span data-ttu-id="b045e-247">Společně s nastavení kontextu operace `StartOperation` vytvoří položku telemetrie hello typu, který určíte.</span><span class="sxs-lookup"><span data-stu-id="b045e-247">Along with setting an operation context, `StartOperation` creates a telemetry item of hello type that you specify.</span></span> <span data-ttu-id="b045e-248">Odesláním telemetrie hello položky při vyřazení hello operaci, nebo pokud explicitně volání `StopOperation`.</span><span class="sxs-lookup"><span data-stu-id="b045e-248">It sends hello telemetry item when you dispose hello operation, or if you explicitly call `StopOperation`.</span></span> <span data-ttu-id="b045e-249">Pokud používáte `RequestTelemetry` jako typ hello telemetrická data, jeho trvání nastavena toohello časový interval mezi zahájení a ukončení.</span><span class="sxs-lookup"><span data-stu-id="b045e-249">If you use `RequestTelemetry` as hello telemetry type, its duration is set toohello timed interval between start and stop.</span></span>

<span data-ttu-id="b045e-250">Kontexty operaci nelze vnořit.</span><span class="sxs-lookup"><span data-stu-id="b045e-250">Operation contexts can't be nested.</span></span> <span data-ttu-id="b045e-251">Pokud je již kontextu operace, pak všechny položky hello obsažené, včetně hello položka vytvořená s přidružen jeho ID `StartOperation`.</span><span class="sxs-lookup"><span data-stu-id="b045e-251">If there is already an operation context, then its ID is associated with all hello contained items, including hello item created with `StartOperation`.</span></span>

<span data-ttu-id="b045e-252">Do pole hledání hello operaci kontext je použité toocreate hello **související položky** seznamu:</span><span class="sxs-lookup"><span data-stu-id="b045e-252">In Search, hello operation context is used toocreate hello **Related Items** list:</span></span>

![Související položky](./media/app-insights-api-custom-events-metrics/21.png)

<span data-ttu-id="b045e-254">Další informace o vlastní operace sledování naleznete v tématu [aplikací – přehledy – vlastní operace tracking.md].</span><span class="sxs-lookup"><span data-stu-id="b045e-254">See [application-insights-custom-operations-tracking.md] for more information on custom operations tracking.</span></span>

### <a name="requests-in-analytics"></a><span data-ttu-id="b045e-255">Požadavky v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-255">Requests in Analytics</span></span> 

<span data-ttu-id="b045e-256">V [Application Insights Analytics](app-insights-analytics.md), požadavky zobrazit nahoru v hello `requests` tabulky.</span><span class="sxs-lookup"><span data-stu-id="b045e-256">In [Application Insights Analytics](app-insights-analytics.md), requests show up in hello `requests` table.</span></span>

<span data-ttu-id="b045e-257">Pokud [vzorkování](app-insights-sampling.md) je v operaci vlastnost itemCount hello zobrazí hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="b045e-257">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property will show a value greater than 1.</span></span> <span data-ttu-id="b045e-258">Pro příklad itemCount == 10 znamená, že z 10 volání tootrackRequest() hello vzorkování proces přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="b045e-258">For example itemCount==10 means that of 10 calls tootrackRequest(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="b045e-259">tooget správný počet požadavků a průměrné trvání segmentované podle požadavku názvy, například použít kód:</span><span class="sxs-lookup"><span data-stu-id="b045e-259">tooget a correct count of requests and average duration segmented by request names, use code such as:</span></span>

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a><span data-ttu-id="b045e-260">TrackException</span><span class="sxs-lookup"><span data-stu-id="b045e-260">TrackException</span></span>
<span data-ttu-id="b045e-261">Odešlete výjimky tooApplication statistiky:</span><span class="sxs-lookup"><span data-stu-id="b045e-261">Send exceptions tooApplication Insights:</span></span>

* <span data-ttu-id="b045e-262">příliš[jejich počet](app-insights-metrics-explorer.md), jako údaje o četnosti hello problému.</span><span class="sxs-lookup"><span data-stu-id="b045e-262">too[count them](app-insights-metrics-explorer.md), as an indication of hello frequency of a problem.</span></span>
* <span data-ttu-id="b045e-263">příliš[Zkontrolujte jednotlivé výskyty](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-263">too[examine individual occurrences](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="b045e-264">Hello sestavy obsahují hello trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="b045e-264">hello reports include hello stack traces.</span></span>

<span data-ttu-id="b045e-265">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-265">*C#*</span></span>

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

<span data-ttu-id="b045e-266">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-266">*JavaScript*</span></span>

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

<span data-ttu-id="b045e-267">sady SDK Hello catch množství výjimek automaticky, takže není vždy nutné toocall TrackException explicitně.</span><span class="sxs-lookup"><span data-stu-id="b045e-267">hello SDKs catch many exceptions automatically, so you don't always have toocall TrackException explicitly.</span></span>

* <span data-ttu-id="b045e-268">ASP.NET: [napsat kód výjimky toocatch](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-268">ASP.NET: [Write code toocatch exceptions](app-insights-asp-net-exceptions.md).</span></span>
* <span data-ttu-id="b045e-269">J2EE: [výjimky jsou zachyceny automaticky](app-insights-java-get-started.md#exceptions-and-request-failures).</span><span class="sxs-lookup"><span data-stu-id="b045e-269">J2EE: [Exceptions are caught automatically](app-insights-java-get-started.md#exceptions-and-request-failures).</span></span>
* <span data-ttu-id="b045e-270">JavaScript: Výjimky jsou zachyceny automaticky.</span><span class="sxs-lookup"><span data-stu-id="b045e-270">JavaScript: Exceptions are caught automatically.</span></span> <span data-ttu-id="b045e-271">Pokud chcete toodisable automatické shromažďování, Přidání fragmentu kódu toohello řádek, který vložte do své webové stránky:</span><span class="sxs-lookup"><span data-stu-id="b045e-271">If you want toodisable automatic collection, add a line toohello code snippet that you insert in your webpages:</span></span>

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a><span data-ttu-id="b045e-272">Výjimky v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-272">Exceptions in Analytics</span></span>

<span data-ttu-id="b045e-273">V [Application Insights Analytics](app-insights-analytics.md), výjimky objeví v hello `exceptions` tabulky.</span><span class="sxs-lookup"><span data-stu-id="b045e-273">In [Application Insights Analytics](app-insights-analytics.md), exceptions show up in hello `exceptions` table.</span></span>

<span data-ttu-id="b045e-274">Pokud [vzorkování](app-insights-sampling.md) je v provozu, hello `itemCount` vlastnost zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="b045e-274">If [sampling](app-insights-sampling.md) is in operation, hello `itemCount` property shows a value greater than 1.</span></span> <span data-ttu-id="b045e-275">Pro příklad itemCount == 10 znamená, že z 10 volání tootrackException() hello vzorkování proces přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="b045e-275">For example itemCount==10 means that of 10 calls tootrackException(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="b045e-276">tooget správný počet výjimek oddělených typ výjimky, jako například použít kód:</span><span class="sxs-lookup"><span data-stu-id="b045e-276">tooget a correct count of exceptions segmented by type of exception, use code such as:</span></span>

```
exceptions | summarize sum(itemCount) by type
```

<span data-ttu-id="b045e-277">Většina hello důležité informace zásobníku je již extrahován do samostatné proměnné, ale můžete vyžádat od sebe hello `details` struktura tooget Další.</span><span class="sxs-lookup"><span data-stu-id="b045e-277">Most of hello important stack information is already extracted into separate variables, but you can pull apart hello `details` structure tooget more.</span></span> <span data-ttu-id="b045e-278">Vzhledem k tomu, že tato struktura je dynamický, by měl přetypování hello výsledek typu toohello očekáváte.</span><span class="sxs-lookup"><span data-stu-id="b045e-278">Since this structure is dynamic, you should cast hello result toohello type you expect.</span></span> <span data-ttu-id="b045e-279">Například:</span><span class="sxs-lookup"><span data-stu-id="b045e-279">For example:</span></span>

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="b045e-280">výjimky tooassociate s jejich související požadavky, použijte spojení:</span><span class="sxs-lookup"><span data-stu-id="b045e-280">tooassociate exceptions with their related requests, use a join:</span></span>

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a><span data-ttu-id="b045e-281">TrackTrace</span><span class="sxs-lookup"><span data-stu-id="b045e-281">TrackTrace</span></span>
<span data-ttu-id="b045e-282">Použití TrackTrace toohelp diagnostikovat problémy odesláním "záznam s popisem cesty" tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="b045e-282">Use TrackTrace toohelp diagnose problems by sending a "breadcrumb trail" tooApplication Insights.</span></span> <span data-ttu-id="b045e-283">Můžete odesílat bloky diagnostických dat a je v zkontrolovat [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-283">You can send chunks of diagnostic data and inspect them in [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="b045e-284">[Přihlaste se adaptéry](app-insights-asp-net-trace-logs.md) používat tento portál toohello jiných výrobců protokoly toosend rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b045e-284">[Log adapters](app-insights-asp-net-trace-logs.md) use this API toosend third-party logs toohello portal.</span></span>

<span data-ttu-id="b045e-285">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-285">*C#*</span></span>

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


<span data-ttu-id="b045e-286">Můžete hledat na obsah zprávy, ale (na rozdíl od hodnoty vlastností) nelze filtrovat na něm.</span><span class="sxs-lookup"><span data-stu-id="b045e-286">You can search on message content, but (unlike property values) you can't filter on it.</span></span>

<span data-ttu-id="b045e-287">limit velikosti Hello na `message` je mnohem vyšší než limit hello na vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-287">hello size limit on `message` is much higher than hello limit on properties.</span></span>
<span data-ttu-id="b045e-288">Výhodou TrackTrace je, že můžete umístit relativně long – datový uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="b045e-288">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="b045e-289">Můžete například kódovat následných dat existuje.</span><span class="sxs-lookup"><span data-stu-id="b045e-289">For example, you can encode POST data there.</span></span>  

<span data-ttu-id="b045e-290">Kromě toho můžete přidat zprávu tooyour úrovně závažnosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-290">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="b045e-291">A, podobně jako ostatní telemetrických dat, můžete přidat toohelp hodnoty vlastností, které filtrovat nebo vyhledávání pro různé skupiny trasování.</span><span class="sxs-lookup"><span data-stu-id="b045e-291">And, like other telemetry, you can add property values toohelp you filter or search for different sets of traces.</span></span> <span data-ttu-id="b045e-292">Například:</span><span class="sxs-lookup"><span data-stu-id="b045e-292">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="b045e-293">V [vyhledávání](app-insights-diagnostic-search.md), můžete pak snadno odfiltrovat všechny zprávy hello úrovně konkrétní závažnosti, které se týkají tooa konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="b045e-293">In [Search](app-insights-diagnostic-search.md), you can then easily filter out all hello messages of a particular severity level that relate tooa particular database.</span></span>


### <a name="traces-in-analytics"></a><span data-ttu-id="b045e-294">Trasování v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-294">Traces in Analytics</span></span>

<span data-ttu-id="b045e-295">V [Application Insights Analytics](app-insights-analytics.md), volání metod tooTrackTrace zobrazit hello `traces` tabulky.</span><span class="sxs-lookup"><span data-stu-id="b045e-295">In [Application Insights Analytics](app-insights-analytics.md), calls tooTrackTrace show up in hello `traces` table.</span></span>

<span data-ttu-id="b045e-296">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="b045e-296">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="b045e-297">Pro příklad itemCount == 10 znamená, že 10 volání příliš`trackTrace()`, proces vzorkování hello přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="b045e-297">For example itemCount==10 means that of 10 calls too`trackTrace()`, hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="b045e-298">tooget správný počet volání trasování, měli byste použít proto kódu, jako `traces | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="b045e-298">tooget a correct count of trace calls, you should use therefore code such as `traces | summarize sum(itemCount)`.</span></span>

## <a name="trackdependency"></a><span data-ttu-id="b045e-299">TrackDependency</span><span class="sxs-lookup"><span data-stu-id="b045e-299">TrackDependency</span></span>
<span data-ttu-id="b045e-300">Použití hello TrackDependency volat tootrack hello odezvy a úspěšnosti volání tooan externí část kódu.</span><span class="sxs-lookup"><span data-stu-id="b045e-300">Use hello TrackDependency call tootrack hello response times and success rates of calls tooan external piece of code.</span></span> <span data-ttu-id="b045e-301">Hello výsledky se zobrazí v grafech závislostí hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b045e-301">hello results appear in hello dependency charts in hello portal.</span></span>

```C#
var success = false;
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

<span data-ttu-id="b045e-302">Mějte na paměti, tento server hello zahrnují sady SDK [závislostí modulu](app-insights-asp-net-dependencies.md) , zjišťuje a sleduje určitých volání závislosti automaticky – například toodatabases a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="b045e-302">Remember that hello server SDKs include a [dependency module](app-insights-asp-net-dependencies.md) that discovers and tracks certain dependency calls automatically--for example, toodatabases and REST APIs.</span></span> <span data-ttu-id="b045e-303">Máte tooinstall agenta na server toomake hello modul fungovat.</span><span class="sxs-lookup"><span data-stu-id="b045e-303">You have tooinstall an agent on your server toomake hello module work.</span></span> <span data-ttu-id="b045e-304">Pokud chcete není catch tootrack volání, které hello automatizované sledování, nebo pokud nechcete, aby tooinstall hello agent použijete toto volání.</span><span class="sxs-lookup"><span data-stu-id="b045e-304">You use this call if you want tootrack calls that hello automated tracking doesn't catch, or if you don't want tooinstall hello agent.</span></span>

<span data-ttu-id="b045e-305">Upravit tooturn vypnout hello standardní modul sledování závislostí, [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) a odstraňte odkaz hello příliš`DependencyCollector.DependencyTrackingTelemetryModule`.</span><span class="sxs-lookup"><span data-stu-id="b045e-305">tooturn off hello standard dependency-tracking module, edit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) and delete hello reference too`DependencyCollector.DependencyTrackingTelemetryModule`.</span></span>

### <a name="dependencies-in-analytics"></a><span data-ttu-id="b045e-306">Závislosti v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-306">Dependencies in Analytics</span></span>

<span data-ttu-id="b045e-307">V [Application Insights Analytics](app-insights-analytics.md), trackDependency volání zobrazí v hello `dependencies` tabulky.</span><span class="sxs-lookup"><span data-stu-id="b045e-307">In [Application Insights Analytics](app-insights-analytics.md), trackDependency calls show up in hello `dependencies` table.</span></span>

<span data-ttu-id="b045e-308">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="b045e-308">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="b045e-309">Pro příklad itemCount == 10 znamená, že z 10 volání tootrackDependency() hello vzorkování proces přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="b045e-309">For example itemCount==10 means that of 10 calls tootrackDependency(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="b045e-310">tooget správný počet závislostí segmentované podle cílové součásti, jako například použít kód:</span><span class="sxs-lookup"><span data-stu-id="b045e-310">tooget a correct count of dependencies segmented by target component, use code such as:</span></span>

```
dependencies | summarize sum(itemCount) by target
```

<span data-ttu-id="b045e-311">závislosti tooassociate s jejich související požadavky, použijte spojení:</span><span class="sxs-lookup"><span data-stu-id="b045e-311">tooassociate dependencies with their related requests, use a join:</span></span>

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a><span data-ttu-id="b045e-312">Probíhá vyprazdňování dat</span><span class="sxs-lookup"><span data-stu-id="b045e-312">Flushing data</span></span>
<span data-ttu-id="b045e-313">Za normálních okolností hello SDK odešle data v některých případech vybrali toominimize hello dopad na uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-313">Normally, hello SDK sends data at times chosen toominimize hello impact on hello user.</span></span> <span data-ttu-id="b045e-314">Ale v některých případech můžete tooflush hello vyrovnávací paměti – například pokud používáte hello SDK v aplikaci, která ukončí.</span><span class="sxs-lookup"><span data-stu-id="b045e-314">However, in some cases, you might want tooflush hello buffer--for example, if you are using hello SDK in an application that shuts down.</span></span>

<span data-ttu-id="b045e-315">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-315">*C#*</span></span>

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

<span data-ttu-id="b045e-316">Upozorňujeme, že je funkce hello asynchronní pro hello [kanálu telemetrii serveru](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span><span class="sxs-lookup"><span data-stu-id="b045e-316">Note that hello function is asynchronous for hello [server telemetry channel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span></span>

## <a name="authenticated-users"></a><span data-ttu-id="b045e-317">Ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="b045e-317">Authenticated users</span></span>
<span data-ttu-id="b045e-318">Ve webové aplikaci jsou uživatelé (ve výchozím nastavení) identifikovat pomocí souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="b045e-318">In a web app, users are (by default) identified by cookies.</span></span> <span data-ttu-id="b045e-319">Uživatel může počítají více než jednou, pokud budou přistupovat k vaší aplikace z v jiném počítači či prohlížeči, nebo pokud se soubory cookie odstranit.</span><span class="sxs-lookup"><span data-stu-id="b045e-319">A user might be counted more than once if they access your app from a different machine or browser, or if they delete cookies.</span></span>

<span data-ttu-id="b045e-320">Pokud se uživatel přihlašuje tooyour aplikace, můžete získat přesnější počet nastavením hello ověřit ID uživatele v hello prohlížeče kódu:</span><span class="sxs-lookup"><span data-stu-id="b045e-320">If users sign in tooyour app, you can get a more accurate count by setting hello authenticated user ID in hello browser code:</span></span>

<span data-ttu-id="b045e-321">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-321">*JavaScript*</span></span>

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

<span data-ttu-id="b045e-322">V technologie ASP.NET MVC aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="b045e-322">In an ASP.NET web MVC application, for example:</span></span>

<span data-ttu-id="b045e-323">*Syntaxe Razor*</span><span class="sxs-lookup"><span data-stu-id="b045e-323">*Razor*</span></span>

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

<span data-ttu-id="b045e-324">Není nutné toouse hello skutečné přihlašovací jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="b045e-324">It isn't necessary toouse hello user's actual sign-in name.</span></span> <span data-ttu-id="b045e-325">Má pouze toobe ID, které je jedinečné toothat uživatele.</span><span class="sxs-lookup"><span data-stu-id="b045e-325">It only has toobe an ID that is unique toothat user.</span></span> <span data-ttu-id="b045e-326">Nesmí obsahovat mezery ani znaky hello `,;=|`.</span><span class="sxs-lookup"><span data-stu-id="b045e-326">It must not include spaces or any of hello characters `,;=|`.</span></span>

<span data-ttu-id="b045e-327">ID uživatele Hello je také nastavit v souboru cookie relace a odeslat toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="b045e-327">hello user ID is also set in a session cookie and sent toohello server.</span></span> <span data-ttu-id="b045e-328">Pokud je nainstalován hello server SDK, hello ověřeného uživatele, které je odeslána jako součást vlastností kontextu hello telemetrie klient i server.</span><span class="sxs-lookup"><span data-stu-id="b045e-328">If hello server SDK is installed, hello authenticated user ID is sent as part of hello context properties of both client and server telemetry.</span></span> <span data-ttu-id="b045e-329">Potom můžete filtrovat a hledání v něm.</span><span class="sxs-lookup"><span data-stu-id="b045e-329">You can then filter and search on it.</span></span>

<span data-ttu-id="b045e-330">Pokud vaše aplikace skupin uživatelů do účtů, můžete předat také identifikátor hello účtu (s hello znak stejné omezení).</span><span class="sxs-lookup"><span data-stu-id="b045e-330">If your app groups users into accounts, you can also pass an identifier for hello account (with hello same character restrictions).</span></span>

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

<span data-ttu-id="b045e-331">V [Průzkumníku metrik](app-insights-metrics-explorer.md), můžete vytvořit graf, který udává **ověřeného uživatele,**, a **uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="b045e-331">In [Metrics Explorer](app-insights-metrics-explorer.md), you can create a chart that counts **Users, Authenticated**, and **User accounts**.</span></span>

<span data-ttu-id="b045e-332">Můžete také [vyhledávání](app-insights-diagnostic-search.md) pro datové body klienta s účty a názvy konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="b045e-332">You can also [search](app-insights-diagnostic-search.md) for client data points with specific user names and accounts.</span></span>

## <span data-ttu-id="b045e-333"><a name="properties"></a>Filtrování, vyhledávání a segmentace svá data pomocí vlastností</span><span class="sxs-lookup"><span data-stu-id="b045e-333"><a name="properties"></a>Filtering, searching, and segmenting your data by using properties</span></span>
<span data-ttu-id="b045e-334">Můžete připojit vlastnosti měření tooyour události (a také toometrics, zobrazení stránek, výjimky a další data telemetrie).</span><span class="sxs-lookup"><span data-stu-id="b045e-334">You can attach properties and measurements tooyour events (and also toometrics, page views, exceptions, and other telemetry data).</span></span>

<span data-ttu-id="b045e-335">*Vlastnosti* jsou řetězcové hodnoty, které můžete toofilter telemetrie v sestavách využití hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-335">*Properties* are string values that you can use toofilter your telemetry in hello usage reports.</span></span> <span data-ttu-id="b045e-336">Například pokud vaše aplikace obsahuje několik hry, můžete připojit hello název hello herní tooeach události tak, aby her, které jsou populárnější, si můžete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="b045e-336">For example, if your app provides several games, you can attach hello name of hello game tooeach event so that you can see which games are more popular.</span></span>

<span data-ttu-id="b045e-337">Existuje omezení 8192 na hello délka řetězce.</span><span class="sxs-lookup"><span data-stu-id="b045e-337">There's a limit of 8192 on hello string length.</span></span> <span data-ttu-id="b045e-338">(Pokud chcete toosend velké bloky dat, použijte parametr zprávy hello [TrackTrace](#track-trace).)</span><span class="sxs-lookup"><span data-stu-id="b045e-338">(If you want toosend large chunks of data, use hello message parameter of [TrackTrace](#track-trace).)</span></span>

<span data-ttu-id="b045e-339">*Metriky* jsou číselné hodnoty, které lze zobrazit graficky.</span><span class="sxs-lookup"><span data-stu-id="b045e-339">*Metrics* are numeric values that can be presented graphically.</span></span> <span data-ttu-id="b045e-340">Můžete například toosee Pokud dojde k postupné zvýšení skóre hello, které vaše hráči dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="b045e-340">For example, you might want toosee if there's a gradual increase in hello scores that your gamers achieve.</span></span> <span data-ttu-id="b045e-341">grafy Hello mohou být segmentovány podle hello vlastnosti, které se odesílají s hello událostí tak, aby vám oddělení nebo skládaný grafy pro různé hry.</span><span class="sxs-lookup"><span data-stu-id="b045e-341">hello graphs can be segmented by hello properties that are sent with hello event, so that you can get separate or stacked graphs for different games.</span></span>

<span data-ttu-id="b045e-342">Pro hodnoty metriky toobe správně zobrazen měly by být větší než nebo rovna too0.</span><span class="sxs-lookup"><span data-stu-id="b045e-342">For metric values toobe correctly displayed, they should be greater than or equal too0.</span></span>

<span data-ttu-id="b045e-343">Některé [omezení počtu hello vlastnosti, hodnoty vlastností a metriky](#limits) , kterou můžete použít.</span><span class="sxs-lookup"><span data-stu-id="b045e-343">There are some [limits on hello number of properties, property values, and metrics](#limits) that you can use.</span></span>

<span data-ttu-id="b045e-344">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-344">*JavaScript*</span></span>

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


<span data-ttu-id="b045e-345">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-345">*C#*</span></span>

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


<span data-ttu-id="b045e-346">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-346">*Visual Basic*</span></span>

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


<span data-ttu-id="b045e-347">*Java*</span><span class="sxs-lookup"><span data-stu-id="b045e-347">*Java*</span></span>

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> <span data-ttu-id="b045e-348">Vezměte v potaz není toolog identifikovatelné osobní údaje ve vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="b045e-348">Take care not toolog personally identifiable information in properties.</span></span>
>
>

<span data-ttu-id="b045e-349">*Pokud jste použili metriky*, otevřete Průzkumníka metrik a vyberte metriku hello hello **vlastní** skupiny:</span><span class="sxs-lookup"><span data-stu-id="b045e-349">*If you used metrics*, open Metrics Explorer and select hello metric from hello **Custom** group:</span></span>

![Otevřete Průzkumníka metrik, vyberte hello grafu a vyberte metriku hello](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> <span data-ttu-id="b045e-351">Pokud vaše metrika se nezobrazí, nebo pokud hello **vlastní** záhlaví není okno Výběr zavřít hello existuje a opakujte akci později.</span><span class="sxs-lookup"><span data-stu-id="b045e-351">If your metric doesn't appear, or if hello **Custom** heading isn't there, close hello selection blade and try again later.</span></span> <span data-ttu-id="b045e-352">Metriky někdy může trvat hodinu toobe agregovat kanálem hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-352">Metrics can sometimes take an hour toobe aggregated through hello pipeline.</span></span>

<span data-ttu-id="b045e-353">*Pokud jste použili vlastnosti a metriky*, segmentovat hello metrika vlastností hello:</span><span class="sxs-lookup"><span data-stu-id="b045e-353">*If you used properties and metrics*, segment hello metric by hello property:</span></span>

![Nastavit seskupování a potom vyberte vlastnost hello pod Seskupit podle](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

<span data-ttu-id="b045e-355">*Ve vyhledávání diagnostiky*, můžete zobrazit vlastnosti hello a metriky jednotlivé výskyty události.</span><span class="sxs-lookup"><span data-stu-id="b045e-355">*In Diagnostic Search*, you can view hello properties and metrics of individual occurrences of an event.</span></span>

![Vyberte instance a pak vyberte "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

<span data-ttu-id="b045e-357">Použití hello **vyhledávání** pole toosee výskytů události, které mají konkrétní hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-357">Use hello **Search** field toosee event occurrences that have a particular property value.</span></span>

![Zadejte termín do vyhledávání](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

<span data-ttu-id="b045e-359">[Další informace o vyhledávacích výrazech](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-359">[Learn more about search expressions](app-insights-diagnostic-search.md).</span></span>

### <a name="alternative-way-tooset-properties-and-metrics"></a><span data-ttu-id="b045e-360">Alternativní způsob tooset vlastnosti a metriky</span><span class="sxs-lookup"><span data-stu-id="b045e-360">Alternative way tooset properties and metrics</span></span>
<span data-ttu-id="b045e-361">Pokud je pohodlnější, můžete shromáždit hello parametry události v samostatném objektu:</span><span class="sxs-lookup"><span data-stu-id="b045e-361">If it's more convenient, you can collect hello parameters of an event in a separate object:</span></span>

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> <span data-ttu-id="b045e-362">Nemáte hello znovu použít stejnou instanci položky telemetrie (`event` v tomto příkladu) toocall Track*() vícekrát.</span><span class="sxs-lookup"><span data-stu-id="b045e-362">Don't reuse hello same telemetry item instance (`event` in this example) toocall Track*() multiple times.</span></span> <span data-ttu-id="b045e-363">To může způsobit telemetrie toobe odeslané s nesprávná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b045e-363">This may cause telemetry toobe sent with incorrect configuration.</span></span>
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a><span data-ttu-id="b045e-364">Vlastní měření a vlastnosti v Analytics</span><span class="sxs-lookup"><span data-stu-id="b045e-364">Custom measurements and properties in Analytics</span></span>

<span data-ttu-id="b045e-365">V [Analytics](app-insights-analytics.md), vlastní metriky a vlastnosti zobrazit v hello `customMeasurements` a `customDimensions` atributy každý záznam telemetrie.</span><span class="sxs-lookup"><span data-stu-id="b045e-365">In [Analytics](app-insights-analytics.md), custom metrics and properties show in hello `customMeasurements` and `customDimensions` attributes of each telemetry record.</span></span>

<span data-ttu-id="b045e-366">Například pokud jste přidali vlastnost s názvem "hra" tooyour požadavku telemetrie, tento dotaz počty hello výskyty různé hodnoty "herní" a zobrazit hello průměr hello vlastní metriky "skóre":</span><span class="sxs-lookup"><span data-stu-id="b045e-366">For example, if you have added a property named "game" tooyour request telemetry, this query counts hello occurrences of different values of "game", and show hello average of hello custom metric "score":</span></span>

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

<span data-ttu-id="b045e-367">Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="b045e-367">Notice that:</span></span>

* <span data-ttu-id="b045e-368">Při extrahování hodnotu z hello customDimensions nebo customMeasurements JSON, má dynamické typ, a proto musíte vysílat `tostring` nebo `todouble`.</span><span class="sxs-lookup"><span data-stu-id="b045e-368">When you extract a value from hello customDimensions or customMeasurements JSON, it has dynamic type, and so you must cast it `tostring` or `todouble`.</span></span>
* <span data-ttu-id="b045e-369">účet tootake Ahoj možnost vzniku [vzorkování](app-insights-sampling.md), měli byste použít `sum(itemCount)`, nikoli `count()`.</span><span class="sxs-lookup"><span data-stu-id="b045e-369">tootake account of hello possibility of [sampling](app-insights-sampling.md), you should use `sum(itemCount)`, not `count()`.</span></span>



## <span data-ttu-id="b045e-370"><a name="timed"></a>Časování událostí</span><span class="sxs-lookup"><span data-stu-id="b045e-370"><a name="timed"></a> Timing events</span></span>
<span data-ttu-id="b045e-371">Někdy budete chtít toochart, jak dlouho trvá tooperform akce.</span><span class="sxs-lookup"><span data-stu-id="b045e-371">Sometimes you want toochart how long it takes tooperform an action.</span></span> <span data-ttu-id="b045e-372">Například můžete chtít tooknow jak dlouho uživatelé provádět tooconsider volby ve hře.</span><span class="sxs-lookup"><span data-stu-id="b045e-372">For example, you might want tooknow how long users take tooconsider choices in a game.</span></span> <span data-ttu-id="b045e-373">To můžete pomocí parametru měření hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-373">You can use hello measurement parameter for this.</span></span>

<span data-ttu-id="b045e-374">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-374">*C#*</span></span>

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <span data-ttu-id="b045e-375"><a name="defaults"></a>Výchozí vlastnosti pro vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="b045e-375"><a name="defaults"></a>Default properties for custom telemetry</span></span>
<span data-ttu-id="b045e-376">Pokud chcete tooset výchozí hodnoty vlastností pro některé hello vlastní události, které lze zadat, můžete je nastavit v instanci TelemetryClient.</span><span class="sxs-lookup"><span data-stu-id="b045e-376">If you want tooset default property values for some of hello custom events that you write, you can set them in a TelemetryClient instance.</span></span> <span data-ttu-id="b045e-377">Jsou připojené tooevery telemetrie položek, který se odesílá z tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="b045e-377">They are attached tooevery telemetry item that's sent from that client.</span></span>

<span data-ttu-id="b045e-378">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-378">*C#*</span></span>

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

<span data-ttu-id="b045e-379">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-379">*Visual Basic*</span></span>

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

<span data-ttu-id="b045e-380">*Java*</span><span class="sxs-lookup"><span data-stu-id="b045e-380">*Java*</span></span>

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



<span data-ttu-id="b045e-381">Jednotlivé telemetrii volání můžete přepsat výchozí hodnoty hello ve slovnících jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-381">Individual telemetry calls can override hello default values in their property dictionaries.</span></span>

<span data-ttu-id="b045e-382">*Pro jazyk JavaScript webových klientů*, [pomocí jazyka JavaScript telemetrie inicializátory](#js-initializer).</span><span class="sxs-lookup"><span data-stu-id="b045e-382">*For JavaScript web clients*, [use JavaScript telemetry initializers](#js-initializer).</span></span>

<span data-ttu-id="b045e-383">*tooadd vlastnosti tooall telemetrie*, včetně dat hello z modulů standardního shromažďování [implementovat `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).</span><span class="sxs-lookup"><span data-stu-id="b045e-383">*tooadd properties tooall telemetry*, including hello data from standard collection modules, [implement `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).</span></span>

## <a name="sampling-filtering-and-processing-telemetry"></a><span data-ttu-id="b045e-384">Vzorkování, filtrování a zpracování telemetrie</span><span class="sxs-lookup"><span data-stu-id="b045e-384">Sampling, filtering, and processing telemetry</span></span>
<span data-ttu-id="b045e-385">Můžete napsat kód tooprocess telemetrie před odesláním z hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b045e-385">You can write code tooprocess your telemetry before it's sent from hello SDK.</span></span> <span data-ttu-id="b045e-386">zpracování Hello obsahuje data, která se odesílá z modulů standardní telemetrie hello, jako je shromažďování požadavků HTTP a kolekce závislost.</span><span class="sxs-lookup"><span data-stu-id="b045e-386">hello processing includes data that's sent from hello standard telemetry modules, such as HTTP request collection and dependency collection.</span></span>

<span data-ttu-id="b045e-387">[Přidání vlastnosti](app-insights-api-filtering-sampling.md#add-properties) tootelemetry implementací `ITelemetryInitializer`.</span><span class="sxs-lookup"><span data-stu-id="b045e-387">[Add properties](app-insights-api-filtering-sampling.md#add-properties) tootelemetry by implementing `ITelemetryInitializer`.</span></span> <span data-ttu-id="b045e-388">Můžete například přidat čísla verzí nebo vypočtené hodnoty od dalších vlastností.</span><span class="sxs-lookup"><span data-stu-id="b045e-388">For example, you can add version numbers or values that are calculated from other properties.</span></span>

<span data-ttu-id="b045e-389">[Filtrování](app-insights-api-filtering-sampling.md#filtering) můžete změnit nebo zrušit telemetrie před odesláním z hello SDK implementací `ITelemetryProcesor`.</span><span class="sxs-lookup"><span data-stu-id="b045e-389">[Filtering](app-insights-api-filtering-sampling.md#filtering) can modify or discard telemetry before it's sent from hello SDK by implementing `ITelemetryProcesor`.</span></span> <span data-ttu-id="b045e-390">Můžete řídit, co se odesílá nebo se zahodí, ale máte tooaccount pro hello vliv na vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="b045e-390">You control what is sent or discarded, but you have tooaccount for hello effect on your metrics.</span></span> <span data-ttu-id="b045e-391">V závislosti na tom, jak zrušit položek může dojít ke ztrátě hello možnost toonavigate mezi související položky.</span><span class="sxs-lookup"><span data-stu-id="b045e-391">Depending on how you discard items, you might lose hello ability toonavigate between related items.</span></span>

<span data-ttu-id="b045e-392">[Vzorkování](app-insights-api-filtering-sampling.md) je svazek zabalené řešení tooreduce hello dat, který se odesílá z portálu toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b045e-392">[Sampling](app-insights-api-filtering-sampling.md) is a packaged solution tooreduce hello volume of data that's sent from your app toohello portal.</span></span> <span data-ttu-id="b045e-393">Dělá to tak, aniž by to ovlivnilo hello zobrazí metriky.</span><span class="sxs-lookup"><span data-stu-id="b045e-393">It does so without affecting hello displayed metrics.</span></span> <span data-ttu-id="b045e-394">A dělá to tak, aniž by to ovlivnilo problémů toodiagnose možnost přechodem mezi související položky jako výjimky, požadavky a zobrazení stránek.</span><span class="sxs-lookup"><span data-stu-id="b045e-394">And it does so without affecting your ability toodiagnose problems by navigating between related items such as exceptions, requests, and page views.</span></span>

<span data-ttu-id="b045e-395">[Další informace](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-395">[Learn more](app-insights-api-filtering-sampling.md).</span></span>

## <a name="disabling-telemetry"></a><span data-ttu-id="b045e-396">Vypnutí telemetrie</span><span class="sxs-lookup"><span data-stu-id="b045e-396">Disabling telemetry</span></span>
<span data-ttu-id="b045e-397">příliš*dynamicky zastavit a spustit* hello shromažďování a předávání telemetrie:</span><span class="sxs-lookup"><span data-stu-id="b045e-397">too*dynamically stop and start* hello collection and transmission of telemetry:</span></span>

<span data-ttu-id="b045e-398">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-398">*C#*</span></span>

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

<span data-ttu-id="b045e-399">příliš*zakázat vybrané standardní Kolektory*– například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář hello relevantní řádků v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). To provedete, například pokud chcete toosend TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="b045e-399">too*disable selected standard collectors*--for example, performance counters, HTTP requests, or dependencies--delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). You can do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <span data-ttu-id="b045e-400"><a name="debug"></a>Režim vývojáře</span><span class="sxs-lookup"><span data-stu-id="b045e-400"><a name="debug"></a>Developer mode</span></span>
<span data-ttu-id="b045e-401">Během ladění, je užitečné toohave telemetrie rychlé kanálem hello tak, aby se zobrazí výsledky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="b045e-401">During debugging, it's useful toohave your telemetry expedited through hello pipeline so that you can see results immediately.</span></span> <span data-ttu-id="b045e-402">Můžete také zprávy Další get, které vám pomůžou trasování všechny problémy s telemetrií hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-402">You also get additional messages that help you trace any problems with hello telemetry.</span></span> <span data-ttu-id="b045e-403">Vypněte ho v produkčním prostředí, protože mohou být zpomaleny vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b045e-403">Switch it off in production, because it may slow down your app.</span></span>

<span data-ttu-id="b045e-404">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-404">*C#*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

<span data-ttu-id="b045e-405">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="b045e-405">*Visual Basic*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <span data-ttu-id="b045e-406"><a name="ikey"></a>Nastavení hello klíč instrumentace pro vybranou vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="b045e-406"><a name="ikey"></a> Setting hello instrumentation key for selected custom telemetry</span></span>
<span data-ttu-id="b045e-407">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-407">*C#*</span></span>

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <span data-ttu-id="b045e-408"><a name="dynamic-ikey"></a>Klíč dynamické instrumentace</span><span class="sxs-lookup"><span data-stu-id="b045e-408"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>
<span data-ttu-id="b045e-409">tooavoid kombinování až telemetrie z vývoj, testování a provozním prostředí, můžete [vytvořit samostatné prostředky Application Insights](app-insights-create-new-resource.md) a změnit jejich klíče, v závislosti na prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-409">tooavoid mixing up telemetry from development, test, and production environments, you can [create separate Application Insights resources](app-insights-create-new-resource.md) and change their keys, depending on hello environment.</span></span>

<span data-ttu-id="b045e-410">Místo získání klíč instrumentace hello z hello konfiguračního souboru, můžete ho nastavit v kódu.</span><span class="sxs-lookup"><span data-stu-id="b045e-410">Instead of getting hello instrumentation key from hello configuration file, you can set it in your code.</span></span> <span data-ttu-id="b045e-411">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavit hello:</span><span class="sxs-lookup"><span data-stu-id="b045e-411">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="b045e-412">*C#*</span><span class="sxs-lookup"><span data-stu-id="b045e-412">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

<span data-ttu-id="b045e-413">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b045e-413">*JavaScript*</span></span>

    appInsights.config.instrumentationKey = myKey;



<span data-ttu-id="b045e-414">Na webových stránkách, můžete chtít tooset ze stavu hello webového serveru, než kódování oznámena do skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-414">In webpages, you might want tooset it from hello web server's state, rather than coding it literally into hello script.</span></span> <span data-ttu-id="b045e-415">Například ve webové stránky vygenerované v aplikaci ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b045e-415">For example, in a webpage generated in an ASP.NET app:</span></span>

<span data-ttu-id="b045e-416">*JavaScript ve Razor*</span><span class="sxs-lookup"><span data-stu-id="b045e-416">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a><span data-ttu-id="b045e-417">TelemetryContext</span><span class="sxs-lookup"><span data-stu-id="b045e-417">TelemetryContext</span></span>
<span data-ttu-id="b045e-418">TelemetryClient má vlastnost kontext, který obsahuje hodnoty, které jsou odeslány společně s všechny telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="b045e-418">TelemetryClient has a Context property, which contains values that are sent along with all telemetry data.</span></span> <span data-ttu-id="b045e-419">Za normálních okolností jsou nastavené moduly hello standardní telemetrie, ale můžete také nastavit jejich sami.</span><span class="sxs-lookup"><span data-stu-id="b045e-419">They are normally set by hello standard telemetry modules, but you can also set them yourself.</span></span> <span data-ttu-id="b045e-420">Například:</span><span class="sxs-lookup"><span data-stu-id="b045e-420">For example:</span></span>

    telemetry.Context.Operation.Name = "MyOperationName";

<span data-ttu-id="b045e-421">Pokud jste nastavili některou z těchto hodnot sami, zvažte odebrání hello příslušný řádek z [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)tak, aby hodnoty a standardní hodnoty hello získat nerozumíte nemáte.</span><span class="sxs-lookup"><span data-stu-id="b045e-421">If you set any of these values yourself, consider removing hello relevant line from [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), so that your values and hello standard values don't get confused.</span></span>

* <span data-ttu-id="b045e-422">**Součást**: hello aplikace a její verzi.</span><span class="sxs-lookup"><span data-stu-id="b045e-422">**Component**: hello app and its version.</span></span>
* <span data-ttu-id="b045e-423">**Zařízení**: Data o hello zařízení, kde je spuštěna aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-423">**Device**: Data about hello device where hello app is running.</span></span> <span data-ttu-id="b045e-424">(Ve službě web apps, to je hello serveru nebo klientského zařízení, které je odeslaný hello telemetrie.)</span><span class="sxs-lookup"><span data-stu-id="b045e-424">(In web apps, this is hello server or client device that hello telemetry is sent from.)</span></span>
* <span data-ttu-id="b045e-425">**InstrumentationKey**: hello prostředek Application Insights v Azure, kde se zobrazí hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="b045e-425">**InstrumentationKey**: hello Application Insights resource in Azure where hello telemetry appear.</span></span> <span data-ttu-id="b045e-426">Ho je obvykle zachyceny ze souboru ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="b045e-426">It's usually picked up from ApplicationInsights.config.</span></span>
* <span data-ttu-id="b045e-427">**Umístění**: hello geografickém umístění zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="b045e-427">**Location**: hello geographic location of hello device.</span></span>
* <span data-ttu-id="b045e-428">**Operace**: ve službě web apps hello aktuální žádost HTTP.</span><span class="sxs-lookup"><span data-stu-id="b045e-428">**Operation**: In web apps, hello current HTTP request.</span></span> <span data-ttu-id="b045e-429">V jiných typů aplikací můžete nastavit tato toogroup události společně.</span><span class="sxs-lookup"><span data-stu-id="b045e-429">In other app types, you can set this toogroup events together.</span></span>
  * <span data-ttu-id="b045e-430">**ID**: generované hodnoty, které koreluje různých událostí, takže když si prohlédnout všechny události ve vyhledávání diagnostiky, můžete najít související položky.</span><span class="sxs-lookup"><span data-stu-id="b045e-430">**Id**: A generated value that correlates different events, so that when you inspect any event in Diagnostic Search, you can find related items.</span></span>
  * <span data-ttu-id="b045e-431">**Název**: identifikátor, obvykle hello URL hello HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="b045e-431">**Name**: An identifier, usually hello URL of hello HTTP request.</span></span>
  * <span data-ttu-id="b045e-432">**SyntheticSource**: Pokud není null nebo prázdný, řetězec, který určuje, které zdroj hello hello žádosti byl identifikován jako robot nebo webový test.</span><span class="sxs-lookup"><span data-stu-id="b045e-432">**SyntheticSource**: If not null or empty, a string that indicates that hello source of hello request has been identified as a robot or web test.</span></span> <span data-ttu-id="b045e-433">Ve výchozím nastavení je vyloučen z výpočtů v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="b045e-433">By default, it is excluded from calculations in Metrics Explorer.</span></span>
* <span data-ttu-id="b045e-434">**Vlastnosti**: vlastnosti, které se odesílají s všechny telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="b045e-434">**Properties**: Properties that are sent with all telemetry data.</span></span> <span data-ttu-id="b045e-435">Může být přepsána v jednotlivých sledovat * volání.</span><span class="sxs-lookup"><span data-stu-id="b045e-435">It can be overridden in individual Track* calls.</span></span>
* <span data-ttu-id="b045e-436">**Relace**: hello uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="b045e-436">**Session**: hello user's session.</span></span> <span data-ttu-id="b045e-437">Hello ID je nastaveno tooa generuje hodnotu, která se změní, když uživatel hello nebyl active nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="b045e-437">hello ID is set tooa generated value, which is changed when hello user has not been active for a while.</span></span>
* <span data-ttu-id="b045e-438">**Uživatel**: informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="b045e-438">**User**: User information.</span></span>

## <a name="limits"></a><span data-ttu-id="b045e-439">Omezení</span><span class="sxs-lookup"><span data-stu-id="b045e-439">Limits</span></span>
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

<span data-ttu-id="b045e-440">tooavoid stiskne limit rychlosti hello data, použijte [vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-440">tooavoid hitting hello data rate limit, use [sampling](app-insights-sampling.md).</span></span>

<span data-ttu-id="b045e-441">toodetermine jak dlouho data se ukládají, najdete v části [uchovávání dat a ochrana osobních údajů](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-441">toodetermine how long data is kept, see [Data retention and privacy](app-insights-data-retention-privacy.md).</span></span>

## <a name="reference-docs"></a><span data-ttu-id="b045e-442">Referenční dokumenty</span><span class="sxs-lookup"><span data-stu-id="b045e-442">Reference docs</span></span>
* [<span data-ttu-id="b045e-443">Rozhraní ASP.NET – reference</span><span class="sxs-lookup"><span data-stu-id="b045e-443">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)
* [<span data-ttu-id="b045e-444">Referenční informace sady Java</span><span class="sxs-lookup"><span data-stu-id="b045e-444">Java reference</span></span>](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [<span data-ttu-id="b045e-445">Referenční dokumentace technologie JavaScript</span><span class="sxs-lookup"><span data-stu-id="b045e-445">JavaScript reference</span></span>](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [<span data-ttu-id="b045e-446">Sada SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="b045e-446">Android SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Android)
* [<span data-ttu-id="b045e-447">Sada SDK pro iOS</span><span class="sxs-lookup"><span data-stu-id="b045e-447">iOS SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a><span data-ttu-id="b045e-448">Kód SDK</span><span class="sxs-lookup"><span data-stu-id="b045e-448">SDK code</span></span>
* [<span data-ttu-id="b045e-449">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="b045e-449">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="b045e-450">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="b045e-450">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="b045e-451">Balíčky systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="b045e-451">Windows Server packages</span></span>](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [<span data-ttu-id="b045e-452">Java SDK</span><span class="sxs-lookup"><span data-stu-id="b045e-452">Java SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Java)
* [<span data-ttu-id="b045e-453">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="b045e-453">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)
* [<span data-ttu-id="b045e-454">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="b045e-454">All platforms</span></span>](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a><span data-ttu-id="b045e-455">Otázky</span><span class="sxs-lookup"><span data-stu-id="b045e-455">Questions</span></span>
* <span data-ttu-id="b045e-456">*Jaké výjimky může vyvolat volání Track_()?*</span><span class="sxs-lookup"><span data-stu-id="b045e-456">*What exceptions might Track_() calls throw?*</span></span>

    <span data-ttu-id="b045e-457">Žádné.</span><span class="sxs-lookup"><span data-stu-id="b045e-457">None.</span></span> <span data-ttu-id="b045e-458">Nepotřebujete toowrap je v klauzulích try-catch.</span><span class="sxs-lookup"><span data-stu-id="b045e-458">You don't need toowrap them in try-catch clauses.</span></span> <span data-ttu-id="b045e-459">Pokud hello SDK zaznamená problémy, bude protokolování zpráv ve výstupu konzoly hello ladění a – pokud hello zprávy získat prostřednictvím – ve vyhledávání diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b045e-459">If hello SDK encounters problems, it will log messages in hello debug console output and--if hello messages get through--in Diagnostic Search.</span></span>
* <span data-ttu-id="b045e-460">*Je k dispozici rozhraní REST API tooget dat z portálu hello?*</span><span class="sxs-lookup"><span data-stu-id="b045e-460">*Is there a REST API tooget data from hello portal?*</span></span>

    <span data-ttu-id="b045e-461">Ano, hello [rozhraní API pro přístup k datům](https://dev.applicationinsights.io/).</span><span class="sxs-lookup"><span data-stu-id="b045e-461">Yes, hello [data access API](https://dev.applicationinsights.io/).</span></span> <span data-ttu-id="b045e-462">Zahrnout další způsoby tooextract data [exportovat z Analytics tooPower BI](app-insights-export-power-bi.md) a [průběžné export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="b045e-462">Other ways tooextract data include [export from Analytics tooPower BI](app-insights-export-power-bi.md) and [continuous export](app-insights-export-telemetry.md).</span></span>

## <span data-ttu-id="b045e-463"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="b045e-463"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="b045e-464">Hledání událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="b045e-464">Search events and logs</span></span>](app-insights-diagnostic-search.md)

* [<span data-ttu-id="b045e-465">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b045e-465">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)


