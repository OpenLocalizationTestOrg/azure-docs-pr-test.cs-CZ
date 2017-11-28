---
title: "Application Insights API pro vlastní události a metriky | Microsoft Docs"
description: "Po zadání několika řádků kódu vložte do vaší aplikace nebo plochy zařízení, webové stránky nebo služby, sledovat využití a diagnostikovat problémy."
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
ms.openlocfilehash: e94c50de51612243386d89c5e0b3178a4f9cbd38
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a><span data-ttu-id="56d7d-103">Application Insights API pro vlastní události a metriky</span><span class="sxs-lookup"><span data-stu-id="56d7d-103">Application Insights API for custom events and metrics</span></span>

<span data-ttu-id="56d7d-104">Vložte po zadání několika řádků kódu v aplikaci a zjistěte, co uživatelé dělají s ním nebo pro usnadnění diagnostiky problémů.</span><span class="sxs-lookup"><span data-stu-id="56d7d-104">Insert a few lines of code in your application to find out what users are doing with it, or to help diagnose issues.</span></span> <span data-ttu-id="56d7d-105">Odesílat telemetrická data z aplikace zařízení a vzdálené ploše, webovými klienty a webové servery.</span><span class="sxs-lookup"><span data-stu-id="56d7d-105">You can send telemetry from device and desktop apps, web clients, and web servers.</span></span> <span data-ttu-id="56d7d-106">Použití [Azure Application Insights](app-insights-overview.md) základní rozhraní API telemetrie k odeslání vlastní události a metriky a vlastní verzích standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="56d7d-106">Use the [Azure Application Insights](app-insights-overview.md) core telemetry API to send custom events and metrics, and your own versions of standard telemetry.</span></span> <span data-ttu-id="56d7d-107">Toto rozhraní API je stejné rozhraní API, které použít standardní sběrače dat Application Insights.</span><span class="sxs-lookup"><span data-stu-id="56d7d-107">This API is the same API that the standard Application Insights data collectors use.</span></span>

## <a name="api-summary"></a><span data-ttu-id="56d7d-108">Souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="56d7d-108">API summary</span></span>
<span data-ttu-id="56d7d-109">Rozhraní API je uniform pro všechny platformy kromě několik malé rozdíly.</span><span class="sxs-lookup"><span data-stu-id="56d7d-109">The API is uniform across all platforms, apart from a few small variations.</span></span>

| <span data-ttu-id="56d7d-110">Metoda</span><span class="sxs-lookup"><span data-stu-id="56d7d-110">Method</span></span> | <span data-ttu-id="56d7d-111">Použít pro</span><span class="sxs-lookup"><span data-stu-id="56d7d-111">Used for</span></span> |
| --- | --- |
| [`TrackPageView`](#page-views) |<span data-ttu-id="56d7d-112">Stránky, obrazovky, okna nebo formuláře.</span><span class="sxs-lookup"><span data-stu-id="56d7d-112">Pages, screens, blades, or forms.</span></span> |
| [`TrackEvent`](#trackevent) |<span data-ttu-id="56d7d-113">Akce uživatelů a dalších událostí.</span><span class="sxs-lookup"><span data-stu-id="56d7d-113">User actions and other events.</span></span> <span data-ttu-id="56d7d-114">Používá ke sledování chování uživatele nebo k monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-114">Used to track user behavior or to monitor performance.</span></span> |
| [`TrackMetric`](#trackmetric) |<span data-ttu-id="56d7d-115">Měření výkonu, jako je například délky front, které nesouvisí s konkrétní události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-115">Performance measurements such as queue lengths not related to specific events.</span></span> |
| [`TrackException`](#trackexception) |<span data-ttu-id="56d7d-116">Protokolování výjimky pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="56d7d-116">Logging exceptions for diagnosis.</span></span> <span data-ttu-id="56d7d-117">Sledování, kde k nim dojde ve vztahu k jiné událostí a zkontrolujte trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="56d7d-117">Trace where they occur in relation to other events and examine stack traces.</span></span> |
| [`TrackRequest`](#trackrequest) |<span data-ttu-id="56d7d-118">Protokolování četnost a dobu trvání požadavky serveru pro analýzu výkonu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-118">Logging the frequency and duration of server requests for performance analysis.</span></span> |
| [`TrackTrace`](#tracktrace) |<span data-ttu-id="56d7d-119">Zprávy protokolů diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-119">Diagnostic log messages.</span></span> <span data-ttu-id="56d7d-120">Také můžete zaznamenat protokoly třetích stran.</span><span class="sxs-lookup"><span data-stu-id="56d7d-120">You can also capture third-party logs.</span></span> |
| [`TrackDependency`](#trackdependency) |<span data-ttu-id="56d7d-121">Protokolování doba trvání a četnost volání na externí komponenty, které závisí aplikace na.</span><span class="sxs-lookup"><span data-stu-id="56d7d-121">Logging the duration and frequency of calls to external components that your app depends on.</span></span> |

<span data-ttu-id="56d7d-122">Můžete [připojení vlastnosti a metriky](#properties) pro většinu těchto volání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="56d7d-122">You can [attach properties and metrics](#properties) to most of these telemetry calls.</span></span>

## <span data-ttu-id="56d7d-123"><a name="prep"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="56d7d-123"><a name="prep"></a>Before you start</span></span>
<span data-ttu-id="56d7d-124">Pokud nemáte k dispozici odkaz na Application Insights SDK ještě:</span><span class="sxs-lookup"><span data-stu-id="56d7d-124">If you don't have a reference on Application Insights SDK yet:</span></span>

* <span data-ttu-id="56d7d-125">Přidejte Application Insights SDK do projektu:</span><span class="sxs-lookup"><span data-stu-id="56d7d-125">Add the Application Insights SDK to your project:</span></span>

  * [<span data-ttu-id="56d7d-126">Projekt ASP.NET</span><span class="sxs-lookup"><span data-stu-id="56d7d-126">ASP.NET project</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="56d7d-127">Projektu Java</span><span class="sxs-lookup"><span data-stu-id="56d7d-127">Java project</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="56d7d-128">JavaScript v každé webové stránky</span><span class="sxs-lookup"><span data-stu-id="56d7d-128">JavaScript in each webpage</span></span>](app-insights-javascript.md) 
* <span data-ttu-id="56d7d-129">V zařízení nebo webový server kódu patří:</span><span class="sxs-lookup"><span data-stu-id="56d7d-129">In your device or web server code, include:</span></span>

    <span data-ttu-id="56d7d-130">*C#:*`using Microsoft.ApplicationInsights;`</span><span class="sxs-lookup"><span data-stu-id="56d7d-130">*C#:* `using Microsoft.ApplicationInsights;`</span></span>

    <span data-ttu-id="56d7d-131">*Visual Basic:*`Imports Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="56d7d-131">*Visual Basic:* `Imports Microsoft.ApplicationInsights`</span></span>

    <span data-ttu-id="56d7d-132">*Java:*`import com.microsoft.applicationinsights.TelemetryClient;`</span><span class="sxs-lookup"><span data-stu-id="56d7d-132">*Java:* `import com.microsoft.applicationinsights.TelemetryClient;`</span></span>

## <a name="constructing-a-telemetryclient-instance"></a><span data-ttu-id="56d7d-133">Vytváření TelemetryClient instance</span><span class="sxs-lookup"><span data-stu-id="56d7d-133">Constructing a TelemetryClient instance</span></span>
<span data-ttu-id="56d7d-134">Vytvořit instanci `TelemetryClient` (s výjimkou v jazyce JavaScript webové stránky):</span><span class="sxs-lookup"><span data-stu-id="56d7d-134">Construct an instance of `TelemetryClient` (except in JavaScript in webpages):</span></span>

<span data-ttu-id="56d7d-135">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-135">*C#*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="56d7d-136">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-136">*Visual Basic*</span></span>

    Private Dim telemetry As New TelemetryClient

<span data-ttu-id="56d7d-137">*Java*</span><span class="sxs-lookup"><span data-stu-id="56d7d-137">*Java*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="56d7d-138">TelemetryClient je bezpečné pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="56d7d-138">TelemetryClient is thread-safe.</span></span>

<span data-ttu-id="56d7d-139">Doporučujeme použít instanci TelemetryClient pro každý modul vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-139">We recommend that you use an instance of TelemetryClient for each module of your app.</span></span> <span data-ttu-id="56d7d-140">Například můžete mít jednu instanci TelemetryClient ve službě web tak, aby odesílaly příchozích požadavků HTTP a druhý v třídě middleware sestavy obchodní logiky události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-140">For instance, you may have one TelemetryClient instance in your web service to report incoming HTTP requests, and another in a middleware class to report business logic events.</span></span> <span data-ttu-id="56d7d-141">Můžete například nastavit vlastnosti `TelemetryClient.Context.User.Id` sledovat uživatele a relace, nebo `TelemetryClient.Context.Device.Id` identifikovat počítač.</span><span class="sxs-lookup"><span data-stu-id="56d7d-141">You can set properties such as `TelemetryClient.Context.User.Id` to track users and sessions, or `TelemetryClient.Context.Device.Id` to identify the machine.</span></span> <span data-ttu-id="56d7d-142">Tyto informace je připojený k všechny události, které odesílá instance.</span><span class="sxs-lookup"><span data-stu-id="56d7d-142">This information is attached to all events that the instance sends.</span></span>

## <a name="trackevent"></a><span data-ttu-id="56d7d-143">TrackEvent</span><span class="sxs-lookup"><span data-stu-id="56d7d-143">TrackEvent</span></span>
<span data-ttu-id="56d7d-144">Ve službě Application Insights *vlastní události* je datový bod, který můžete zobrazit v [Průzkumníku metrik](app-insights-metrics-explorer.md) jako agregovaného počtu a v [diagnostické vyhledávání](app-insights-diagnostic-search.md) jako jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-144">In Application Insights, a *custom event* is a data point that you can display in [Metrics Explorer](app-insights-metrics-explorer.md) as an aggregated count, and in [Diagnostic Search](app-insights-diagnostic-search.md) as individual occurrences.</span></span> <span data-ttu-id="56d7d-145">(Není souvisejících s MVC nebo jiných framework "události.")</span><span class="sxs-lookup"><span data-stu-id="56d7d-145">(It isn't related to MVC or other framework "events.")</span></span>

<span data-ttu-id="56d7d-146">Vložit `TrackEvent` volá ve vašem kódu počítat různé události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-146">Insert `TrackEvent` calls in your code to count various events.</span></span> <span data-ttu-id="56d7d-147">Jak často uživatelé vybrat konkrétní funkce, jak často budou dosáhnout určité cíle nebo možná četnosti provádění konkrétní typy chyb.</span><span class="sxs-lookup"><span data-stu-id="56d7d-147">How often users choose a particular feature, how often they achieve particular goals, or maybe how often they make particular types of mistakes.</span></span>

<span data-ttu-id="56d7d-148">Herní aplikace, například odeslání události vždy, když uživatel wins hra:</span><span class="sxs-lookup"><span data-stu-id="56d7d-148">For example, in a game app, send an event whenever a user wins the game:</span></span>

<span data-ttu-id="56d7d-149">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-149">*JavaScript*</span></span>

    appInsights.trackEvent("WinGame");

<span data-ttu-id="56d7d-150">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-150">*C#*</span></span>

    telemetry.TrackEvent("WinGame");

<span data-ttu-id="56d7d-151">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-151">*Visual Basic*</span></span>

    telemetry.TrackEvent("WinGame")

<span data-ttu-id="56d7d-152">*Java*</span><span class="sxs-lookup"><span data-stu-id="56d7d-152">*Java*</span></span>

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-the-microsoft-azure-portal"></a><span data-ttu-id="56d7d-153">Zobrazit události na portálu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="56d7d-153">View your events in the Microsoft Azure portal</span></span>
<span data-ttu-id="56d7d-154">Chcete-li zobrazit počet událostí, otevřete [Průzkumníku metrik](app-insights-metrics-explorer.md) okně přidejte nový graf a vyberte **události**.</span><span class="sxs-lookup"><span data-stu-id="56d7d-154">To see a count of your events, open a [Metrics Explorer](app-insights-metrics-explorer.md) blade, add a new chart, and select **Events**.</span></span>  

![Zobrazí počet vlastní události](./media/app-insights-api-custom-events-metrics/01-custom.png)

<span data-ttu-id="56d7d-156">Pro porovnání počty různé události, nastavte typ grafu **mřížky**a skupinu podle názvu událostí:</span><span class="sxs-lookup"><span data-stu-id="56d7d-156">To compare the counts of different events, set the chart type to **Grid**, and group by event name:</span></span>

![Nastavte typ grafu a seskupení](./media/app-insights-api-custom-events-metrics/07-grid.png)

<span data-ttu-id="56d7d-158">V mřížce klikněte na tlačítko prostřednictvím název události zobrazíte jednotlivé výskyty této události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-158">On the grid, click through an event name to see individual occurrences of that event.</span></span> <span data-ttu-id="56d7d-159">Chcete-li zobrazit více podrobností – klikněte na kterýkoli z výskytů v seznamu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-159">To see more detail - click any occurrence in the list.</span></span>

![Procházení událostí](./media/app-insights-api-custom-events-metrics/03-instances.png)

<span data-ttu-id="56d7d-161">Umožňuje zaměřit se na konkrétní události ve vyhledávání nebo Průzkumníku metrik, nastavte v okně filtru na názvy událostí, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="56d7d-161">To focus on specific events in either Search or Metrics Explorer, set the blade's filter to the event names that you're interested in:</span></span>

![Otevřete filtry, rozbalte název události a vyberte jednu nebo více hodnot](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a><span data-ttu-id="56d7d-163">Vlastní události v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-163">Custom events in Analytics</span></span>

<span data-ttu-id="56d7d-164">Je k dispozici v telemetrii `customEvents` tabulky v [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-164">The telemetry is available in the `customEvents` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="56d7d-165">Každý řádek představuje volání `trackEvent(..)` ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56d7d-165">Each row represents a call to `trackEvent(..)` in your app.</span></span> 

<span data-ttu-id="56d7d-166">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="56d7d-166">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="56d7d-167">Pro příklad itemCount == 10 znamená, že 10 volání trackEvent(), proces vzorkování přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="56d7d-167">For example itemCount==10 means that of 10 calls to trackEvent(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="56d7d-168">Chcete-li získat správný počet vlastních událostí, použijte proto použít kód jako `customEvent | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-168">To get a correct count of custom events, you should use therefore use code such as `customEvent | summarize sum(itemCount)`.</span></span>


## <a name="trackmetric"></a><span data-ttu-id="56d7d-169">TrackMetric</span><span class="sxs-lookup"><span data-stu-id="56d7d-169">TrackMetric</span></span>

<span data-ttu-id="56d7d-170">Application Insights můžete grafu metriky, které nejsou připojeny ke konkrétní události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-170">Application Insights can chart metrics that are not attached to particular events.</span></span> <span data-ttu-id="56d7d-171">Délka fronty může například sledovat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="56d7d-171">For example, you could monitor a queue length at regular intervals.</span></span> <span data-ttu-id="56d7d-172">O metriky jednotlivými měřeními jsou méně důležité než variace a trendy a proto statistické grafy jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="56d7d-172">With metrics, the individual measurements are of less interest than the variations and trends, and so statistical charts are useful.</span></span>

<span data-ttu-id="56d7d-173">Aby bylo možné odesílat metriky do služby Application Insights, můžete použít `TrackMetric(..)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="56d7d-173">In order to send metrics to Application Insights, you can use the `TrackMetric(..)` API.</span></span> <span data-ttu-id="56d7d-174">Existují dva způsoby, jak odeslat metriky:</span><span class="sxs-lookup"><span data-stu-id="56d7d-174">There are two ways to send a metric:</span></span> 

* <span data-ttu-id="56d7d-175">Jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-175">Single value.</span></span> <span data-ttu-id="56d7d-176">Pokaždé, když provedete měření ve vaší aplikaci, odešlete s odpovídající hodnotou Application Insights.</span><span class="sxs-lookup"><span data-stu-id="56d7d-176">Every time you perform a measurement in your application, you send the corresponding value to Application Insights.</span></span> <span data-ttu-id="56d7d-177">Předpokládejme například, že máte metriky popisující počet položek v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="56d7d-177">For example, assume that you have a metric describing the number of items in a container.</span></span> <span data-ttu-id="56d7d-178">V konkrétním časovém období můžete poprvé tři položky do kontejneru a potom odeberte dvě položky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-178">During a particular time period, you first put three items into the container and then you remove two items.</span></span> <span data-ttu-id="56d7d-179">Podle toho by volání `TrackMetric` dvakrát: první předání hodnota `3` a potom hodnotu `-2`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-179">Accordingly, you would call `TrackMetric` twice: first passing the value `3` and then the value `-2`.</span></span> <span data-ttu-id="56d7d-180">Application Insights ukládá obě hodnoty vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="56d7d-180">Application Insights stores both values on your behalf.</span></span> 

* <span data-ttu-id="56d7d-181">Agregace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-181">Aggregation.</span></span> <span data-ttu-id="56d7d-182">Při práci s metriky, je každé jedno měření zájmu zřídka.</span><span class="sxs-lookup"><span data-stu-id="56d7d-182">When working with metrics, every single measurement is rarely of interest.</span></span> <span data-ttu-id="56d7d-183">Místo toho je důležité souhrn co se stalo v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="56d7d-183">Instead a summary of what happened during a particular time period is important.</span></span> <span data-ttu-id="56d7d-184">Takové souhrn nazývá _agregace_.</span><span class="sxs-lookup"><span data-stu-id="56d7d-184">Such a summary is called _aggregation_.</span></span> <span data-ttu-id="56d7d-185">V předchozím příkladu agregační metriky součet za toto období je `1` a počet hodnot metriky je `2`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-185">In the above example, the aggregate metric sum for that time period is `1` and the count of the metric values is `2`.</span></span> <span data-ttu-id="56d7d-186">Pokud použijete způsob agregace, pouze vyvolání `TrackMetric` jednou za časové období a odesílat agregované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56d7d-186">When using the aggregation approach, you only invoke `TrackMetric` once per time period and send the aggregate values.</span></span> <span data-ttu-id="56d7d-187">Toto je doporučený postup, vzhledem k tomu může významně snížit náklady a výkon režie odesláním méně datových bodů do služby Application Insights a stále shromažďovat všechny relevantní informace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-187">This is the recommended approach since it can significantly reduce the cost and performance overhead by sending fewer data points to Application Insights, while still collecting all relevant information.</span></span>

### <a name="examples"></a><span data-ttu-id="56d7d-188">Příklady:</span><span class="sxs-lookup"><span data-stu-id="56d7d-188">Examples:</span></span>

#### <a name="single-values"></a><span data-ttu-id="56d7d-189">Jednotlivé hodnoty</span><span class="sxs-lookup"><span data-stu-id="56d7d-189">Single values</span></span>

<span data-ttu-id="56d7d-190">Odeslání jednoho metriky hodnoty:</span><span class="sxs-lookup"><span data-stu-id="56d7d-190">To send a single metric value:</span></span>

<span data-ttu-id="56d7d-191">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-191">*JavaScript*</span></span>

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

<span data-ttu-id="56d7d-192">*C#, Java*</span><span class="sxs-lookup"><span data-stu-id="56d7d-192">*C#, Java*</span></span>

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a><span data-ttu-id="56d7d-193">Agregování metriky</span><span class="sxs-lookup"><span data-stu-id="56d7d-193">Aggregating metrics</span></span>

<span data-ttu-id="56d7d-194">Doporučuje se agregovaná metrika před jejich odesláním z vaší aplikace, ke snížení šířky pásma, náklady a ke zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-194">It is recommended to aggregate metrics before sending them from your app, to reduce bandwidth, cost and to improve performance.</span></span>
<span data-ttu-id="56d7d-195">Tady je příklad totožný kódu:</span><span class="sxs-lookup"><span data-stu-id="56d7d-195">Here is an example of aggregating code:</span></span>

<span data-ttu-id="56d7d-196">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-196">*C#*</span></span>

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
    /// Accepts metric values and sends the aggregated values at 1-minute intervals.
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
                    // Wait for end end of the aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap the current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute the actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct the metric telemetry item and send:
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

### <a name="custom-metrics-in-metrics-explorer"></a><span data-ttu-id="56d7d-197">Vlastní metriky v Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="56d7d-197">Custom metrics in Metrics Explorer</span></span>

<span data-ttu-id="56d7d-198">Pokud chcete zobrazit výsledky, otevřete Průzkumníka metrik a přidejte nový graf.</span><span class="sxs-lookup"><span data-stu-id="56d7d-198">To see the results, open Metrics Explorer and add a new chart.</span></span> <span data-ttu-id="56d7d-199">Upravte v grafu zobrazí vaše metriku.</span><span class="sxs-lookup"><span data-stu-id="56d7d-199">Edit the chart to show your metric.</span></span>

> [!NOTE]
> <span data-ttu-id="56d7d-200">Vlastní metriku může trvat několik minut, než se objeví v seznamu dostupné metriky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-200">Your custom metric might take several minutes to appear in the list of available metrics.</span></span>
>

![Přidejte nový graf nebo vyberte graf a v části vlastní, vyberte vaše metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a><span data-ttu-id="56d7d-202">Vlastní metriky v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-202">Custom metrics in Analytics</span></span>

<span data-ttu-id="56d7d-203">Je k dispozici v telemetrii `customMetrics` tabulky v [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-203">The telemetry is available in the `customMetrics` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="56d7d-204">Každý řádek představuje volání `trackMetric(..)` ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56d7d-204">Each row represents a call to `trackMetric(..)` in your app.</span></span>
* <span data-ttu-id="56d7d-205">`valueSum`-Toto je součet měření.</span><span class="sxs-lookup"><span data-stu-id="56d7d-205">`valueSum` - This is the sum of the measurements.</span></span> <span data-ttu-id="56d7d-206">Chcete-li získat střední hodnoty, vydělte `valueCount`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-206">To get the mean value, divide by `valueCount`.</span></span>
* <span data-ttu-id="56d7d-207">`valueCount`-Počet měření, které byly agregovat do této `trackMetric(..)` volání.</span><span class="sxs-lookup"><span data-stu-id="56d7d-207">`valueCount` - The number of measurements that were aggregated into this `trackMetric(..)` call.</span></span>

## <a name="page-views"></a><span data-ttu-id="56d7d-208">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="56d7d-208">Page views</span></span>
<span data-ttu-id="56d7d-209">V aplikaci pomocí zařízení nebo webové stránky je odeslána telemetrická zobrazení stránky ve výchozím nastavení při načtení každé obrazovky nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-209">In a device or webpage app, page view telemetry is sent by default when each screen or page is loaded.</span></span> <span data-ttu-id="56d7d-210">Ale můžete změnit, sledovat zobrazení stránky v další nebo různých časech.</span><span class="sxs-lookup"><span data-stu-id="56d7d-210">But you can change that to track page views at additional or different times.</span></span> <span data-ttu-id="56d7d-211">Například v aplikaci, která zobrazí karty nebo okna, můžete sledovat na stránce vždy, když uživatel otevře nové okno.</span><span class="sxs-lookup"><span data-stu-id="56d7d-211">For example, in an app that displays tabs or blades, you might want to track a page whenever the user opens a new blade.</span></span>

![Použití přehledu v okně Přehled](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

<span data-ttu-id="56d7d-213">Data uživatele a relace se odešle jako vlastnosti společně s zobrazení stránky, uživatele a relace grafy pocházet zachování připojení při telemetrická zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-213">User and session data is sent as properties along with page views, so the user and session charts come alive when there is page view telemetry.</span></span>

### <a name="custom-page-views"></a><span data-ttu-id="56d7d-214">Zobrazení vlastních stránek</span><span class="sxs-lookup"><span data-stu-id="56d7d-214">Custom page views</span></span>
<span data-ttu-id="56d7d-215">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-215">*JavaScript*</span></span>

    appInsights.trackPageView("tab1");

<span data-ttu-id="56d7d-216">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-216">*C#*</span></span>

    telemetry.TrackPageView("GameReviewPage");

<span data-ttu-id="56d7d-217">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-217">*Visual Basic*</span></span>

    telemetry.TrackPageView("GameReviewPage")


<span data-ttu-id="56d7d-218">Pokud máte několik karet v rámci jiné stránky HTML, můžete zadat adresu URL příliš:</span><span class="sxs-lookup"><span data-stu-id="56d7d-218">If you have several tabs within different HTML pages, you can specify the URL too:</span></span>

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a><span data-ttu-id="56d7d-219">Zobrazení stránky časování</span><span class="sxs-lookup"><span data-stu-id="56d7d-219">Timing page views</span></span>
<span data-ttu-id="56d7d-220">Ve výchozím nastavení, časy hlášené jako **zobrazení času načítání stránky** se měří od, když prohlížeč odesílá požadavek, dokud se nazývá události načtení stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="56d7d-220">By default, the times reported as **Page view load time** are measured from when the browser sends the request, until the browser's page load event is called.</span></span>

<span data-ttu-id="56d7d-221">Místo toho můžete buď:</span><span class="sxs-lookup"><span data-stu-id="56d7d-221">Instead, you can either:</span></span>

* <span data-ttu-id="56d7d-222">Nastavit explicitní doba trvání [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) volání: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-222">Set an explicit duration in the [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) call: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span></span>
* <span data-ttu-id="56d7d-223">Použití zobrazení stránky časování volání `startTrackPage` a `stopTrackPage`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-223">Use the page view timing calls `startTrackPage` and `stopTrackPage`.</span></span>

<span data-ttu-id="56d7d-224">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-224">*JavaScript*</span></span>

    // To start timing a page:
    appInsights.startTrackPage("Page1");

<span data-ttu-id="56d7d-225">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="56d7d-225">...</span></span>

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

<span data-ttu-id="56d7d-226">Název, který použijete jako první parametr přidruží volání zahájení a ukončení.</span><span class="sxs-lookup"><span data-stu-id="56d7d-226">The name that you use as the first parameter associates the start and stop calls.</span></span> <span data-ttu-id="56d7d-227">Výchozí název aktuální stránky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-227">It defaults to the current page name.</span></span>

<span data-ttu-id="56d7d-228">Výsledný doby zatížení stránky zobrazí v Průzkumníku metrik jsou odvozeny od interval mezi volání zahájení a ukončení.</span><span class="sxs-lookup"><span data-stu-id="56d7d-228">The resulting page load durations displayed in Metrics Explorer are derived from the interval between the start and stop calls.</span></span> <span data-ttu-id="56d7d-229">Je to na můžete jaké interval, ve skutečnosti čas.</span><span class="sxs-lookup"><span data-stu-id="56d7d-229">It's up to you what interval you actually time.</span></span>

### <a name="page-telemetry-in-analytics"></a><span data-ttu-id="56d7d-230">Stránka telemetrie Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-230">Page telemetry in Analytics</span></span>

<span data-ttu-id="56d7d-231">V [Analytics](app-insights-analytics.md) dvou tabulek zobrazit data z prohlížeče operace:</span><span class="sxs-lookup"><span data-stu-id="56d7d-231">In [Analytics](app-insights-analytics.md) two tables show data from browser operations:</span></span>

* <span data-ttu-id="56d7d-232">`pageViews` Tabulka obsahuje data o název adresy URL a stránky</span><span class="sxs-lookup"><span data-stu-id="56d7d-232">The `pageViews` table contains data about the URL and page title</span></span>
* <span data-ttu-id="56d7d-233">`browserTimings` Tabulka obsahuje data o výkonu klienta, jako je například čas potřebný ke zpracování příchozích dat.</span><span class="sxs-lookup"><span data-stu-id="56d7d-233">The `browserTimings` table contains data about client performance, such as the time taken to process the incoming data</span></span>

<span data-ttu-id="56d7d-234">Vyhledání, jak dlouho trvá prohlížeče pro zpracování různých stránkách:</span><span class="sxs-lookup"><span data-stu-id="56d7d-234">To find how long the browser takes to process different pages:</span></span>

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

<span data-ttu-id="56d7d-235">Pro zjištění popularities různých prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="56d7d-235">To discover the popularities of different browsers:</span></span>

```
pageViews | summarize count() by client_Browser
```

<span data-ttu-id="56d7d-236">Chcete-li přidružit zobrazení stránky k volání AJAX, spojení s závislosti:</span><span class="sxs-lookup"><span data-stu-id="56d7d-236">To associate page views to AJAX calls, join with dependencies:</span></span>

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a><span data-ttu-id="56d7d-237">TrackRequest</span><span class="sxs-lookup"><span data-stu-id="56d7d-237">TrackRequest</span></span>
<span data-ttu-id="56d7d-238">Server SDK používá TrackRequest do protokolu HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="56d7d-238">The server SDK uses TrackRequest to log HTTP requests.</span></span>

<span data-ttu-id="56d7d-239">Můžete také volat ho sami Pokud chcete simulovat požadavků v kontextu, kde nemáte modulu služby web spuštěn.</span><span class="sxs-lookup"><span data-stu-id="56d7d-239">You can also call it yourself if you want to simulate requests in a context where you don't have the web service module running.</span></span>

<span data-ttu-id="56d7d-240">Doporučeným způsobem, jak odesílat telemetrická data požadavku je ale, kde žádost funguje jako <a href="#operation-context">operační kontext</a>.</span><span class="sxs-lookup"><span data-stu-id="56d7d-240">However, the recommended way to send request telemetry is where the request acts as an <a href="#operation-context">operation context</a>.</span></span>

## <a name="operation-context"></a><span data-ttu-id="56d7d-241">Operace kontextu</span><span class="sxs-lookup"><span data-stu-id="56d7d-241">Operation context</span></span>
<span data-ttu-id="56d7d-242">Telemetrie položky můžete přidružit společně připojením k nim běžné ID operace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-242">You can associate telemetry items together by attaching to them a common operation ID.</span></span> <span data-ttu-id="56d7d-243">Standardní modulu Sledování žádostí o tomu pro výjimky a dalších událostí, které se odesílají během zpracování požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="56d7d-243">The standard request-tracking module does this for exceptions and other events that are sent while an HTTP request is being processed.</span></span> <span data-ttu-id="56d7d-244">V [vyhledávání](app-insights-diagnostic-search.md) a [Analytics](app-insights-analytics.md), ID vám pomůže snadno najít všechny události přidružené k požadavku.</span><span class="sxs-lookup"><span data-stu-id="56d7d-244">In [Search](app-insights-diagnostic-search.md) and [Analytics](app-insights-analytics.md), you can use the ID to easily find any events associated with the request.</span></span>

<span data-ttu-id="56d7d-245">Nejjednodušší způsob, jak nastavit ID je nastavit kontextu operace pomocí tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="56d7d-245">The easiest way to set the ID is to set an operation context by using this pattern:</span></span>

<span data-ttu-id="56d7d-246">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-246">*C#*</span></span>

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use the same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

<span data-ttu-id="56d7d-247">Společně s nastavení kontextu operace `StartOperation` vytvoří položku telemetrie typu, který určíte.</span><span class="sxs-lookup"><span data-stu-id="56d7d-247">Along with setting an operation context, `StartOperation` creates a telemetry item of the type that you specify.</span></span> <span data-ttu-id="56d7d-248">Odešle položce telemetrie při vyřazení operaci, nebo pokud explicitně volání `StopOperation`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-248">It sends the telemetry item when you dispose the operation, or if you explicitly call `StopOperation`.</span></span> <span data-ttu-id="56d7d-249">Pokud používáte `RequestTelemetry` jako typ telemetrická data, jeho trvání nastavena na se časový interval mezi zahájení a ukončení.</span><span class="sxs-lookup"><span data-stu-id="56d7d-249">If you use `RequestTelemetry` as the telemetry type, its duration is set to the timed interval between start and stop.</span></span>

<span data-ttu-id="56d7d-250">Kontexty operaci nelze vnořit.</span><span class="sxs-lookup"><span data-stu-id="56d7d-250">Operation contexts can't be nested.</span></span> <span data-ttu-id="56d7d-251">Pokud je již kontextu operace, pak je přidružen všechny obsažené položky, včetně položky vytvořené pomocí jeho ID `StartOperation`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-251">If there is already an operation context, then its ID is associated with all the contained items, including the item created with `StartOperation`.</span></span>

<span data-ttu-id="56d7d-252">Do pole hledání kontext operace se používá k vytvoření **související položky** seznamu:</span><span class="sxs-lookup"><span data-stu-id="56d7d-252">In Search, the operation context is used to create the **Related Items** list:</span></span>

![Související položky](./media/app-insights-api-custom-events-metrics/21.png)

<span data-ttu-id="56d7d-254">Další informace o vlastní operace sledování naleznete v tématu [aplikací – přehledy – vlastní operace tracking.md].</span><span class="sxs-lookup"><span data-stu-id="56d7d-254">See [application-insights-custom-operations-tracking.md] for more information on custom operations tracking.</span></span>

### <a name="requests-in-analytics"></a><span data-ttu-id="56d7d-255">Požadavky v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-255">Requests in Analytics</span></span> 

<span data-ttu-id="56d7d-256">V [Application Insights Analytics](app-insights-analytics.md), požadavky zobrazit nahoru v `requests` tabulky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-256">In [Application Insights Analytics](app-insights-analytics.md), requests show up in the `requests` table.</span></span>

<span data-ttu-id="56d7d-257">Pokud [vzorkování](app-insights-sampling.md) je v operaci vlastnost itemCount zobrazí hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="56d7d-257">If [sampling](app-insights-sampling.md) is in operation, the itemCount property will show a value greater than 1.</span></span> <span data-ttu-id="56d7d-258">Pro příklad itemCount == 10 znamená, že 10 volání trackRequest(), proces vzorkování přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="56d7d-258">For example itemCount==10 means that of 10 calls to trackRequest(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="56d7d-259">Správný počet požadavků a průměrné trvání segmentované podle požadavku názvy získáte pomocí kódu, jako:</span><span class="sxs-lookup"><span data-stu-id="56d7d-259">To get a correct count of requests and average duration segmented by request names, use code such as:</span></span>

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a><span data-ttu-id="56d7d-260">TrackException</span><span class="sxs-lookup"><span data-stu-id="56d7d-260">TrackException</span></span>
<span data-ttu-id="56d7d-261">Odesílání výjimky Application Insights:</span><span class="sxs-lookup"><span data-stu-id="56d7d-261">Send exceptions to Application Insights:</span></span>

* <span data-ttu-id="56d7d-262">K [jejich počet](app-insights-metrics-explorer.md), jako údaje o četnosti problému.</span><span class="sxs-lookup"><span data-stu-id="56d7d-262">To [count them](app-insights-metrics-explorer.md), as an indication of the frequency of a problem.</span></span>
* <span data-ttu-id="56d7d-263">K [Zkontrolujte jednotlivé výskyty](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-263">To [examine individual occurrences](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="56d7d-264">Sestavy obsahují trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="56d7d-264">The reports include the stack traces.</span></span>

<span data-ttu-id="56d7d-265">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-265">*C#*</span></span>

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

<span data-ttu-id="56d7d-266">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-266">*JavaScript*</span></span>

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

<span data-ttu-id="56d7d-267">Sady SDK catch množství výjimek automaticky, takže nemáte vždy volat TrackException explicitně.</span><span class="sxs-lookup"><span data-stu-id="56d7d-267">The SDKs catch many exceptions automatically, so you don't always have to call TrackException explicitly.</span></span>

* <span data-ttu-id="56d7d-268">ASP.NET: [napsat kód pro zachycení výjimky](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-268">ASP.NET: [Write code to catch exceptions](app-insights-asp-net-exceptions.md).</span></span>
* <span data-ttu-id="56d7d-269">J2EE: [výjimky jsou zachyceny automaticky](app-insights-java-get-started.md#exceptions-and-request-failures).</span><span class="sxs-lookup"><span data-stu-id="56d7d-269">J2EE: [Exceptions are caught automatically](app-insights-java-get-started.md#exceptions-and-request-failures).</span></span>
* <span data-ttu-id="56d7d-270">JavaScript: Výjimky jsou zachyceny automaticky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-270">JavaScript: Exceptions are caught automatically.</span></span> <span data-ttu-id="56d7d-271">Pokud chcete zakázat automatické shromažďování, přidejte řádek fragment kódu, který vložte do své webové stránky:</span><span class="sxs-lookup"><span data-stu-id="56d7d-271">If you want to disable automatic collection, add a line to the code snippet that you insert in your webpages:</span></span>

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a><span data-ttu-id="56d7d-272">Výjimky v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-272">Exceptions in Analytics</span></span>

<span data-ttu-id="56d7d-273">V [Application Insights Analytics](app-insights-analytics.md), výjimky objeví v `exceptions` tabulky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-273">In [Application Insights Analytics](app-insights-analytics.md), exceptions show up in the `exceptions` table.</span></span>

<span data-ttu-id="56d7d-274">Pokud [vzorkování](app-insights-sampling.md) je v provozu, `itemCount` vlastnost zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="56d7d-274">If [sampling](app-insights-sampling.md) is in operation, the `itemCount` property shows a value greater than 1.</span></span> <span data-ttu-id="56d7d-275">Pro příklad itemCount == 10 znamená, že 10 volání pro trackException() proces vzorkování přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="56d7d-275">For example itemCount==10 means that of 10 calls to trackException(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="56d7d-276">Chcete-li získat správný počet výjimek segmentované podle typu výjimky, použijte kód, jako:</span><span class="sxs-lookup"><span data-stu-id="56d7d-276">To get a correct count of exceptions segmented by type of exception, use code such as:</span></span>

```
exceptions | summarize sum(itemCount) by type
```

<span data-ttu-id="56d7d-277">Většinu informací důležité zásobníku je již extrahován do samostatné proměnné, ale můžete vyžádat od sebe `details` struktura získat další.</span><span class="sxs-lookup"><span data-stu-id="56d7d-277">Most of the important stack information is already extracted into separate variables, but you can pull apart the `details` structure to get more.</span></span> <span data-ttu-id="56d7d-278">Vzhledem k tomu, že tato struktura je dynamický, by měl přetypovat na typ očekávaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="56d7d-278">Since this structure is dynamic, you should cast the result to the type you expect.</span></span> <span data-ttu-id="56d7d-279">Například:</span><span class="sxs-lookup"><span data-stu-id="56d7d-279">For example:</span></span>

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="56d7d-280">Chcete-li výjimky přidružit jejich související požadavky, použijte spojení:</span><span class="sxs-lookup"><span data-stu-id="56d7d-280">To associate exceptions with their related requests, use a join:</span></span>

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a><span data-ttu-id="56d7d-281">TrackTrace</span><span class="sxs-lookup"><span data-stu-id="56d7d-281">TrackTrace</span></span>
<span data-ttu-id="56d7d-282">Použijte TrackTrace k diagnostice potíží odesláním "záznam s popisem cesty" Application Insights.</span><span class="sxs-lookup"><span data-stu-id="56d7d-282">Use TrackTrace to help diagnose problems by sending a "breadcrumb trail" to Application Insights.</span></span> <span data-ttu-id="56d7d-283">Můžete odesílat bloky diagnostických dat a je v zkontrolovat [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-283">You can send chunks of diagnostic data and inspect them in [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="56d7d-284">[Přihlaste se adaptéry](app-insights-asp-net-trace-logs.md) používat toto rozhraní API k odesílání jiných výrobců protokoly na portál.</span><span class="sxs-lookup"><span data-stu-id="56d7d-284">[Log adapters](app-insights-asp-net-trace-logs.md) use this API to send third-party logs to the portal.</span></span>

<span data-ttu-id="56d7d-285">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-285">*C#*</span></span>

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


<span data-ttu-id="56d7d-286">Můžete hledat na obsah zprávy, ale (na rozdíl od hodnoty vlastností) nelze filtrovat na něm.</span><span class="sxs-lookup"><span data-stu-id="56d7d-286">You can search on message content, but (unlike property values) you can't filter on it.</span></span>

<span data-ttu-id="56d7d-287">Limit velikosti na `message` mnohem vyšší, než je limit na vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56d7d-287">The size limit on `message` is much higher than the limit on properties.</span></span>
<span data-ttu-id="56d7d-288">Výhodou TrackTrace je, že můžete ukládat poměrně dlouho data ve zprávě.</span><span class="sxs-lookup"><span data-stu-id="56d7d-288">An advantage of TrackTrace is that you can put relatively long data in the message.</span></span> <span data-ttu-id="56d7d-289">Můžete například kódovat následných dat existuje.</span><span class="sxs-lookup"><span data-stu-id="56d7d-289">For example, you can encode POST data there.</span></span>  

<span data-ttu-id="56d7d-290">Kromě toho můžete přidat úroveň závažnosti na zprávu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-290">In addition, you can add a severity level to your message.</span></span> <span data-ttu-id="56d7d-291">A, podobně jako ostatní telemetrických dat, můžete přidat hodnoty vlastností, které vám pomohou filtru nebo vyhledávání pro různé skupiny trasování.</span><span class="sxs-lookup"><span data-stu-id="56d7d-291">And, like other telemetry, you can add property values to help you filter or search for different sets of traces.</span></span> <span data-ttu-id="56d7d-292">Například:</span><span class="sxs-lookup"><span data-stu-id="56d7d-292">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="56d7d-293">V [vyhledávání](app-insights-diagnostic-search.md), můžete pak snadno odfiltrovat všechny zprávy konkrétní závažnosti úrovně, které se vztahují ke konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="56d7d-293">In [Search](app-insights-diagnostic-search.md), you can then easily filter out all the messages of a particular severity level that relate to a particular database.</span></span>


### <a name="traces-in-analytics"></a><span data-ttu-id="56d7d-294">Trasování v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-294">Traces in Analytics</span></span>

<span data-ttu-id="56d7d-295">V [Application Insights Analytics](app-insights-analytics.md), volání TrackTrace zobrazí v `traces` tabulky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-295">In [Application Insights Analytics](app-insights-analytics.md), calls to TrackTrace show up in the `traces` table.</span></span>

<span data-ttu-id="56d7d-296">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="56d7d-296">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="56d7d-297">Příklad itemCount == 10 znamená, že 10 volání `trackTrace()`, proces vzorkování přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="56d7d-297">For example itemCount==10 means that of 10 calls to `trackTrace()`, the sampling process only transmitted one of them.</span></span> <span data-ttu-id="56d7d-298">Pokud chcete získat správný počet volání trasování, měli byste použít proto kódu, jako `traces | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-298">To get a correct count of trace calls, you should use therefore code such as `traces | summarize sum(itemCount)`.</span></span>

## <a name="trackdependency"></a><span data-ttu-id="56d7d-299">TrackDependency</span><span class="sxs-lookup"><span data-stu-id="56d7d-299">TrackDependency</span></span>
<span data-ttu-id="56d7d-300">Volání TrackDependency použijte ke sledování doby odezvy a úspěšnost volání externí část kódu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-300">Use the TrackDependency call to track the response times and success rates of calls to an external piece of code.</span></span> <span data-ttu-id="56d7d-301">Výsledky se zobrazí v grafech závislost na portálu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-301">The results appear in the dependency charts in the portal.</span></span>

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

<span data-ttu-id="56d7d-302">Mějte na paměti, že zahrnují sady SDK serveru [závislostí modulu](app-insights-asp-net-dependencies.md) , zjišťuje a sleduje určitých volání závislosti automaticky – například k databázím a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="56d7d-302">Remember that the server SDKs include a [dependency module](app-insights-asp-net-dependencies.md) that discovers and tracks certain dependency calls automatically--for example, to databases and REST APIs.</span></span> <span data-ttu-id="56d7d-303">Je třeba nainstalovat agenta na vašem serveru, aby se modul fungovat.</span><span class="sxs-lookup"><span data-stu-id="56d7d-303">You have to install an agent on your server to make the module work.</span></span> <span data-ttu-id="56d7d-304">Toto volání použijte, pokud chcete sledovat volání, které není catch automatizované sledování, nebo pokud nechcete, aby pro instalaci agenta.</span><span class="sxs-lookup"><span data-stu-id="56d7d-304">You use this call if you want to track calls that the automated tracking doesn't catch, or if you don't want to install the agent.</span></span>

<span data-ttu-id="56d7d-305">Chcete-li vypnout standardní modul sledování závislostí, upravit [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) a odstraňte odkaz na `DependencyCollector.DependencyTrackingTelemetryModule`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-305">To turn off the standard dependency-tracking module, edit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) and delete the reference to `DependencyCollector.DependencyTrackingTelemetryModule`.</span></span>

### <a name="dependencies-in-analytics"></a><span data-ttu-id="56d7d-306">Závislosti v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-306">Dependencies in Analytics</span></span>

<span data-ttu-id="56d7d-307">V [Application Insights Analytics](app-insights-analytics.md), trackDependency volá zobrazit `dependencies` tabulky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-307">In [Application Insights Analytics](app-insights-analytics.md), trackDependency calls show up in the `dependencies` table.</span></span>

<span data-ttu-id="56d7d-308">Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount zobrazuje hodnotu větší než 1.</span><span class="sxs-lookup"><span data-stu-id="56d7d-308">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="56d7d-309">Pro příklad itemCount == 10 znamená, že 10 volání trackDependency(), proces vzorkování přenášena pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="56d7d-309">For example itemCount==10 means that of 10 calls to trackDependency(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="56d7d-310">Získat správný počet závislostí segmentované podle cílové součásti, například použijte kód:</span><span class="sxs-lookup"><span data-stu-id="56d7d-310">To get a correct count of dependencies segmented by target component, use code such as:</span></span>

```
dependencies | summarize sum(itemCount) by target
```

<span data-ttu-id="56d7d-311">Chcete-li přidružit své žádosti související závislosti, použijte spojení:</span><span class="sxs-lookup"><span data-stu-id="56d7d-311">To associate dependencies with their related requests, use a join:</span></span>

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a><span data-ttu-id="56d7d-312">Probíhá vyprazdňování dat</span><span class="sxs-lookup"><span data-stu-id="56d7d-312">Flushing data</span></span>
<span data-ttu-id="56d7d-313">Za normálních okolností sady SDK odešle data v některých případech se rozhodli minimalizovat dopad na uživatele.</span><span class="sxs-lookup"><span data-stu-id="56d7d-313">Normally, the SDK sends data at times chosen to minimize the impact on the user.</span></span> <span data-ttu-id="56d7d-314">Ale v některých případech můžete chtít vyprázdní vyrovnávací paměti – například pokud používáte sady SDK v aplikaci, která ukončí.</span><span class="sxs-lookup"><span data-stu-id="56d7d-314">However, in some cases, you might want to flush the buffer--for example, if you are using the SDK in an application that shuts down.</span></span>

<span data-ttu-id="56d7d-315">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-315">*C#*</span></span>

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

<span data-ttu-id="56d7d-316">Upozorňujeme, že je asynchronní pro funkce [kanálu telemetrii serveru](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span><span class="sxs-lookup"><span data-stu-id="56d7d-316">Note that the function is asynchronous for the [server telemetry channel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span></span>

## <a name="authenticated-users"></a><span data-ttu-id="56d7d-317">Ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="56d7d-317">Authenticated users</span></span>
<span data-ttu-id="56d7d-318">Ve webové aplikaci jsou uživatelé (ve výchozím nastavení) identifikovat pomocí souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="56d7d-318">In a web app, users are (by default) identified by cookies.</span></span> <span data-ttu-id="56d7d-319">Uživatel může počítají více než jednou, pokud budou přistupovat k vaší aplikace z v jiném počítači či prohlížeči, nebo pokud se soubory cookie odstranit.</span><span class="sxs-lookup"><span data-stu-id="56d7d-319">A user might be counted more than once if they access your app from a different machine or browser, or if they delete cookies.</span></span>

<span data-ttu-id="56d7d-320">Pokud se uživatel přihlásí k aplikaci, můžete získat přesnější počet nastavením ID ověřený uživatel v prohlížeči kódu:</span><span class="sxs-lookup"><span data-stu-id="56d7d-320">If users sign in to your app, you can get a more accurate count by setting the authenticated user ID in the browser code:</span></span>

<span data-ttu-id="56d7d-321">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-321">*JavaScript*</span></span>

```JS
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

<span data-ttu-id="56d7d-322">V technologie ASP.NET MVC aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="56d7d-322">In an ASP.NET web MVC application, for example:</span></span>

<span data-ttu-id="56d7d-323">*Syntaxe Razor*</span><span class="sxs-lookup"><span data-stu-id="56d7d-323">*Razor*</span></span>

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

<span data-ttu-id="56d7d-324">Není nutné používat skutečné přihlašovací jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="56d7d-324">It isn't necessary to use the user's actual sign-in name.</span></span> <span data-ttu-id="56d7d-325">Pouze musí být ID, které jsou jedinečné pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="56d7d-325">It only has to be an ID that is unique to that user.</span></span> <span data-ttu-id="56d7d-326">Nesmí obsahovat mezery ani znaky `,;=|`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-326">It must not include spaces or any of the characters `,;=|`.</span></span>

<span data-ttu-id="56d7d-327">ID uživatele je také nastavit v souboru cookie relace a odesílá na server.</span><span class="sxs-lookup"><span data-stu-id="56d7d-327">The user ID is also set in a session cookie and sent to the server.</span></span> <span data-ttu-id="56d7d-328">Pokud je nainstalován server SDK, ID ověřený uživatel odešle jako součást vlastností kontextu telemetrie klient i server.</span><span class="sxs-lookup"><span data-stu-id="56d7d-328">If the server SDK is installed, the authenticated user ID is sent as part of the context properties of both client and server telemetry.</span></span> <span data-ttu-id="56d7d-329">Potom můžete filtrovat a hledání v něm.</span><span class="sxs-lookup"><span data-stu-id="56d7d-329">You can then filter and search on it.</span></span>

<span data-ttu-id="56d7d-330">Pokud vaše aplikace skupin uživatelů do účtů, můžete předat také identifikátor k účtu (s stejné omezení znaků).</span><span class="sxs-lookup"><span data-stu-id="56d7d-330">If your app groups users into accounts, you can also pass an identifier for the account (with the same character restrictions).</span></span>

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

<span data-ttu-id="56d7d-331">V [Průzkumníku metrik](app-insights-metrics-explorer.md), můžete vytvořit graf, který udává **ověřeného uživatele,**, a **uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="56d7d-331">In [Metrics Explorer](app-insights-metrics-explorer.md), you can create a chart that counts **Users, Authenticated**, and **User accounts**.</span></span>

<span data-ttu-id="56d7d-332">Můžete také [vyhledávání](app-insights-diagnostic-search.md) pro datové body klienta s účty a názvy konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="56d7d-332">You can also [search](app-insights-diagnostic-search.md) for client data points with specific user names and accounts.</span></span>

## <span data-ttu-id="56d7d-333"><a name="properties"></a>Filtrování, vyhledávání a segmentace svá data pomocí vlastností</span><span class="sxs-lookup"><span data-stu-id="56d7d-333"><a name="properties"></a>Filtering, searching, and segmenting your data by using properties</span></span>
<span data-ttu-id="56d7d-334">Můžete přiřadit události vlastnosti a měření (a také k metrikám, stránka zobrazení, výjimky a další data telemetrie).</span><span class="sxs-lookup"><span data-stu-id="56d7d-334">You can attach properties and measurements to your events (and also to metrics, page views, exceptions, and other telemetry data).</span></span>

<span data-ttu-id="56d7d-335">*Vlastnosti* jsou řetězcové hodnoty, které můžete použít k filtrování telemetrie v sestavách využití.</span><span class="sxs-lookup"><span data-stu-id="56d7d-335">*Properties* are string values that you can use to filter your telemetry in the usage reports.</span></span> <span data-ttu-id="56d7d-336">Například pokud vaše aplikace obsahuje několik hry, můžete připojit název hry pro každou jednotlivou událost tak, aby her, které jsou populárnější, si můžete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="56d7d-336">For example, if your app provides several games, you can attach the name of the game to each event so that you can see which games are more popular.</span></span>

<span data-ttu-id="56d7d-337">Existuje omezení 8192 na délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="56d7d-337">There's a limit of 8192 on the string length.</span></span> <span data-ttu-id="56d7d-338">(Pokud chcete odeslat velké množství dat, použijte parametr zpráva [TrackTrace](#track-trace).)</span><span class="sxs-lookup"><span data-stu-id="56d7d-338">(If you want to send large chunks of data, use the message parameter of [TrackTrace](#track-trace).)</span></span>

<span data-ttu-id="56d7d-339">*Metriky* jsou číselné hodnoty, které lze zobrazit graficky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-339">*Metrics* are numeric values that can be presented graphically.</span></span> <span data-ttu-id="56d7d-340">Například můžete chtít zjistit, zda je k postupné zvýšení skóre, které vaše hráči dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="56d7d-340">For example, you might want to see if there's a gradual increase in the scores that your gamers achieve.</span></span> <span data-ttu-id="56d7d-341">V grafech mohou být segmentovány podle vlastnosti, které se odesílají s událostí, tak, aby vám samostatný nebo skládaný grafy pro různé hry.</span><span class="sxs-lookup"><span data-stu-id="56d7d-341">The graphs can be segmented by the properties that are sent with the event, so that you can get separate or stacked graphs for different games.</span></span>

<span data-ttu-id="56d7d-342">Metriky hodnot, který se má zobrazit správně musí být větší než nebo rovna 0.</span><span class="sxs-lookup"><span data-stu-id="56d7d-342">For metric values to be correctly displayed, they should be greater than or equal to 0.</span></span>

<span data-ttu-id="56d7d-343">Některé [omezení počtu vlastností, hodnoty vlastností a metriky](#limits) , kterou můžete použít.</span><span class="sxs-lookup"><span data-stu-id="56d7d-343">There are some [limits on the number of properties, property values, and metrics](#limits) that you can use.</span></span>

<span data-ttu-id="56d7d-344">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-344">*JavaScript*</span></span>

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


<span data-ttu-id="56d7d-345">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-345">*C#*</span></span>

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics);


<span data-ttu-id="56d7d-346">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-346">*Visual Basic*</span></span>

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics)


<span data-ttu-id="56d7d-347">*Java*</span><span class="sxs-lookup"><span data-stu-id="56d7d-347">*Java*</span></span>

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> <span data-ttu-id="56d7d-348">Je třeba dbát na ve vlastnostech protokolu identifikovatelné osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="56d7d-348">Take care not to log personally identifiable information in properties.</span></span>
>
>

<span data-ttu-id="56d7d-349">*Pokud jste použili metriky*, otevřete Průzkumníka metrik a vyberte metriku z **vlastní** skupiny:</span><span class="sxs-lookup"><span data-stu-id="56d7d-349">*If you used metrics*, open Metrics Explorer and select the metric from the **Custom** group:</span></span>

![Otevřete Průzkumníka metrik, vyberte graf a vyberte metriku](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> <span data-ttu-id="56d7d-351">Pokud vaše metrika se nezobrazí, nebo pokud **vlastní** záhlaví nejsou, zavřete okno Výběr a zkuste to znovu později.</span><span class="sxs-lookup"><span data-stu-id="56d7d-351">If your metric doesn't appear, or if the **Custom** heading isn't there, close the selection blade and try again later.</span></span> <span data-ttu-id="56d7d-352">Metriky někdy může trvat hodinu mají agregovat prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-352">Metrics can sometimes take an hour to be aggregated through the pipeline.</span></span>

<span data-ttu-id="56d7d-353">*Pokud jste použili vlastnosti a metriky*, segmentovat metriku vlastností:</span><span class="sxs-lookup"><span data-stu-id="56d7d-353">*If you used properties and metrics*, segment the metric by the property:</span></span>

![Nastavit seskupování a potom vyberte vlastnost pod Seskupit podle](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

<span data-ttu-id="56d7d-355">*Ve vyhledávání diagnostiky*, můžete zobrazit vlastnosti a metriky jednotlivé výskyty události.</span><span class="sxs-lookup"><span data-stu-id="56d7d-355">*In Diagnostic Search*, you can view the properties and metrics of individual occurrences of an event.</span></span>

![Vyberte instance a pak vyberte "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

<span data-ttu-id="56d7d-357">Použití **vyhledávání** pole zobrazíte výskytů události, které mají konkrétní hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56d7d-357">Use the **Search** field to see event occurrences that have a particular property value.</span></span>

![Zadejte termín do vyhledávání](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

<span data-ttu-id="56d7d-359">[Další informace o vyhledávacích výrazech](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-359">[Learn more about search expressions](app-insights-diagnostic-search.md).</span></span>

### <a name="alternative-way-to-set-properties-and-metrics"></a><span data-ttu-id="56d7d-360">Alternativní způsob, jak nastavit vlastnosti a metriky</span><span class="sxs-lookup"><span data-stu-id="56d7d-360">Alternative way to set properties and metrics</span></span>
<span data-ttu-id="56d7d-361">Pokud je pohodlnější, můžete shromáždit parametry události v samostatném objektu:</span><span class="sxs-lookup"><span data-stu-id="56d7d-361">If it's more convenient, you can collect the parameters of an event in a separate object:</span></span>

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> <span data-ttu-id="56d7d-362">Nemáte opakovaně použít stejnou instanci položky telemetrie (`event` v tomto příkladu) Track*() volat vícekrát.</span><span class="sxs-lookup"><span data-stu-id="56d7d-362">Don't reuse the same telemetry item instance (`event` in this example) to call Track*() multiple times.</span></span> <span data-ttu-id="56d7d-363">To může způsobit telemetrických dat k odeslání s nesprávná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-363">This may cause telemetry to be sent with incorrect configuration.</span></span>
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a><span data-ttu-id="56d7d-364">Vlastní měření a vlastnosti v Analytics</span><span class="sxs-lookup"><span data-stu-id="56d7d-364">Custom measurements and properties in Analytics</span></span>

<span data-ttu-id="56d7d-365">V [Analytics](app-insights-analytics.md), vlastní metriky a vlastnosti zobrazit v `customMeasurements` a `customDimensions` atributy každý záznam telemetrie.</span><span class="sxs-lookup"><span data-stu-id="56d7d-365">In [Analytics](app-insights-analytics.md), custom metrics and properties show in the `customMeasurements` and `customDimensions` attributes of each telemetry record.</span></span>

<span data-ttu-id="56d7d-366">Například pokud jste přidali vlastnost s názvem "herní" k žádosti o telemetrie, tento dotaz počet výskytů různé hodnoty "herní" a zobrazit průměr vlastní metriky "skóre":</span><span class="sxs-lookup"><span data-stu-id="56d7d-366">For example, if you have added a property named "game" to your request telemetry, this query counts the occurrences of different values of "game", and show the average of the custom metric "score":</span></span>

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

<span data-ttu-id="56d7d-367">Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="56d7d-367">Notice that:</span></span>

* <span data-ttu-id="56d7d-368">Při extrahování hodnotu z customDimensions nebo customMeasurements JSON, má dynamické typ, a proto musíte vysílat `tostring` nebo `todouble`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-368">When you extract a value from the customDimensions or customMeasurements JSON, it has dynamic type, and so you must cast it `tostring` or `todouble`.</span></span>
* <span data-ttu-id="56d7d-369">Vzít v úvahu možnost [vzorkování](app-insights-sampling.md), měli byste použít `sum(itemCount)`, nikoli `count()`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-369">To take account of the possibility of [sampling](app-insights-sampling.md), you should use `sum(itemCount)`, not `count()`.</span></span>



## <span data-ttu-id="56d7d-370"><a name="timed"></a>Časování událostí</span><span class="sxs-lookup"><span data-stu-id="56d7d-370"><a name="timed"></a> Timing events</span></span>
<span data-ttu-id="56d7d-371">Někdy budete chtít grafu doba potřebná k provedení akce.</span><span class="sxs-lookup"><span data-stu-id="56d7d-371">Sometimes you want to chart how long it takes to perform an action.</span></span> <span data-ttu-id="56d7d-372">Například můžete chtít vědět, jak dlouho uživatelé proveďte vzít v úvahu volbám v hru.</span><span class="sxs-lookup"><span data-stu-id="56d7d-372">For example, you might want to know how long users take to consider choices in a game.</span></span> <span data-ttu-id="56d7d-373">Můžete pro tento parametr měření.</span><span class="sxs-lookup"><span data-stu-id="56d7d-373">You can use the measurement parameter for this.</span></span>

<span data-ttu-id="56d7d-374">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-374">*C#*</span></span>

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform the timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send the event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <span data-ttu-id="56d7d-375"><a name="defaults"></a>Výchozí vlastnosti pro vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="56d7d-375"><a name="defaults"></a>Default properties for custom telemetry</span></span>
<span data-ttu-id="56d7d-376">Pokud chcete nastavit výchozí hodnoty vlastností pro některé vlastní události, které lze zadat, můžete je nastavit v instanci TelemetryClient.</span><span class="sxs-lookup"><span data-stu-id="56d7d-376">If you want to set default property values for some of the custom events that you write, you can set them in a TelemetryClient instance.</span></span> <span data-ttu-id="56d7d-377">Jsou připojené každé položce telemetrie, který se odesílá z tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="56d7d-377">They are attached to every telemetry item that's sent from that client.</span></span>

<span data-ttu-id="56d7d-378">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-378">*C#*</span></span>

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame");

<span data-ttu-id="56d7d-379">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-379">*Visual Basic*</span></span>

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame")

<span data-ttu-id="56d7d-380">*Java*</span><span class="sxs-lookup"><span data-stu-id="56d7d-380">*Java*</span></span>

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



<span data-ttu-id="56d7d-381">Jednotlivé telemetrii volání můžete přepsat výchozí hodnoty ve slovnících jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56d7d-381">Individual telemetry calls can override the default values in their property dictionaries.</span></span>

<span data-ttu-id="56d7d-382">*Pro jazyk JavaScript webových klientů*, [pomocí jazyka JavaScript telemetrie inicializátory](#js-initializer).</span><span class="sxs-lookup"><span data-stu-id="56d7d-382">*For JavaScript web clients*, [use JavaScript telemetry initializers](#js-initializer).</span></span>

<span data-ttu-id="56d7d-383">*Přidání vlastnosti do všech telemetrie*, včetně dat z modulů standardního shromažďování [implementovat `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).</span><span class="sxs-lookup"><span data-stu-id="56d7d-383">*To add properties to all telemetry*, including the data from standard collection modules, [implement `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).</span></span>

## <a name="sampling-filtering-and-processing-telemetry"></a><span data-ttu-id="56d7d-384">Vzorkování, filtrování a zpracování telemetrie</span><span class="sxs-lookup"><span data-stu-id="56d7d-384">Sampling, filtering, and processing telemetry</span></span>
<span data-ttu-id="56d7d-385">Můžete napsat kód pro zpracování telemetrie před odesláním ze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="56d7d-385">You can write code to process your telemetry before it's sent from the SDK.</span></span> <span data-ttu-id="56d7d-386">Zpracování obsahuje data, která se odesílá z modulů standardní telemetrie, jako je shromažďování požadavků HTTP a kolekce závislost.</span><span class="sxs-lookup"><span data-stu-id="56d7d-386">The processing includes data that's sent from the standard telemetry modules, such as HTTP request collection and dependency collection.</span></span>

<span data-ttu-id="56d7d-387">[Přidání vlastnosti](app-insights-api-filtering-sampling.md#add-properties) k telemetrie implementací `ITelemetryInitializer`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-387">[Add properties](app-insights-api-filtering-sampling.md#add-properties) to telemetry by implementing `ITelemetryInitializer`.</span></span> <span data-ttu-id="56d7d-388">Můžete například přidat čísla verzí nebo vypočtené hodnoty od dalších vlastností.</span><span class="sxs-lookup"><span data-stu-id="56d7d-388">For example, you can add version numbers or values that are calculated from other properties.</span></span>

<span data-ttu-id="56d7d-389">[Filtrování](app-insights-api-filtering-sampling.md#filtering) můžete změnit nebo zrušit telemetrie před odesláním ze sady SDK implementací `ITelemetryProcesor`.</span><span class="sxs-lookup"><span data-stu-id="56d7d-389">[Filtering](app-insights-api-filtering-sampling.md#filtering) can modify or discard telemetry before it's sent from the SDK by implementing `ITelemetryProcesor`.</span></span> <span data-ttu-id="56d7d-390">Můžete řídit, co se odesílá nebo se zahodí, ale budete muset účet pro vliv na vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-390">You control what is sent or discarded, but you have to account for the effect on your metrics.</span></span> <span data-ttu-id="56d7d-391">V závislosti na tom, jak zrušit položek může dojít ke ztrátě možnost přecházet mezi související položky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-391">Depending on how you discard items, you might lose the ability to navigate between related items.</span></span>

<span data-ttu-id="56d7d-392">[Vzorkování](app-insights-api-filtering-sampling.md) je zabalené řešení ke snížení objemu dat, který se odesílá z vaší aplikace na portál.</span><span class="sxs-lookup"><span data-stu-id="56d7d-392">[Sampling](app-insights-api-filtering-sampling.md) is a packaged solution to reduce the volume of data that's sent from your app to the portal.</span></span> <span data-ttu-id="56d7d-393">Dělá to tak, aniž by to ovlivnilo zobrazených metrik.</span><span class="sxs-lookup"><span data-stu-id="56d7d-393">It does so without affecting the displayed metrics.</span></span> <span data-ttu-id="56d7d-394">A dělá to tak, aniž by to ovlivnilo moct lépe diagnostikovat problémy tak, že přejdete mezi související položky jako výjimky, požadavky a zobrazení stránek.</span><span class="sxs-lookup"><span data-stu-id="56d7d-394">And it does so without affecting your ability to diagnose problems by navigating between related items such as exceptions, requests, and page views.</span></span>

<span data-ttu-id="56d7d-395">[Další informace](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-395">[Learn more](app-insights-api-filtering-sampling.md).</span></span>

## <a name="disabling-telemetry"></a><span data-ttu-id="56d7d-396">Vypnutí telemetrie</span><span class="sxs-lookup"><span data-stu-id="56d7d-396">Disabling telemetry</span></span>
<span data-ttu-id="56d7d-397">K *dynamicky zastavit a spustit* shromažďování a předávání telemetrie:</span><span class="sxs-lookup"><span data-stu-id="56d7d-397">To *dynamically stop and start* the collection and transmission of telemetry:</span></span>

<span data-ttu-id="56d7d-398">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-398">*C#*</span></span>

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

<span data-ttu-id="56d7d-399">K *zakázat vybrané standardní Kolektory*– například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář příslušné řádky v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). To provedete, například pokud chcete odeslat vlastní TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="56d7d-399">To *disable selected standard collectors*--for example, performance counters, HTTP requests, or dependencies--delete or comment out the relevant lines in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). You can do this, for example, if you want to send your own TrackRequest data.</span></span>

## <span data-ttu-id="56d7d-400"><a name="debug"></a>Režim vývojáře</span><span class="sxs-lookup"><span data-stu-id="56d7d-400"><a name="debug"></a>Developer mode</span></span>
<span data-ttu-id="56d7d-401">Během ladění, je užitečné používat telemetrie prostřednictvím kanálu je rychlé, aby mohli zobrazit výsledky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="56d7d-401">During debugging, it's useful to have your telemetry expedited through the pipeline so that you can see results immediately.</span></span> <span data-ttu-id="56d7d-402">Můžete také zprávy Další get, které vám pomůžou trasování všechny problémy s telemetrií.</span><span class="sxs-lookup"><span data-stu-id="56d7d-402">You also get additional messages that help you trace any problems with the telemetry.</span></span> <span data-ttu-id="56d7d-403">Vypněte ho v produkčním prostředí, protože mohou být zpomaleny vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-403">Switch it off in production, because it may slow down your app.</span></span>

<span data-ttu-id="56d7d-404">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-404">*C#*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

<span data-ttu-id="56d7d-405">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="56d7d-405">*Visual Basic*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <span data-ttu-id="56d7d-406"><a name="ikey"></a>Nastavení pro vybrané vlastní telemetrii klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="56d7d-406"><a name="ikey"></a> Setting the instrumentation key for selected custom telemetry</span></span>
<span data-ttu-id="56d7d-407">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-407">*C#*</span></span>

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <span data-ttu-id="56d7d-408"><a name="dynamic-ikey"></a>Klíč dynamické instrumentace</span><span class="sxs-lookup"><span data-stu-id="56d7d-408"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>
<span data-ttu-id="56d7d-409">Abyste se vyhnuli kombinování až telemetrie z vývoj, testování a provozním prostředí, můžete [vytvořit samostatné prostředky Application Insights](app-insights-create-new-resource.md) a změnit jejich klíče, v závislosti na prostředí.</span><span class="sxs-lookup"><span data-stu-id="56d7d-409">To avoid mixing up telemetry from development, test, and production environments, you can [create separate Application Insights resources](app-insights-create-new-resource.md) and change their keys, depending on the environment.</span></span>

<span data-ttu-id="56d7d-410">Místo získání klíč instrumentace z konfiguračního souboru, můžete ho nastavit v kódu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-410">Instead of getting the instrumentation key from the configuration file, you can set it in your code.</span></span> <span data-ttu-id="56d7d-411">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavte:</span><span class="sxs-lookup"><span data-stu-id="56d7d-411">Set the key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="56d7d-412">*C#*</span><span class="sxs-lookup"><span data-stu-id="56d7d-412">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

<span data-ttu-id="56d7d-413">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="56d7d-413">*JavaScript*</span></span>

    appInsights.config.instrumentationKey = myKey;



<span data-ttu-id="56d7d-414">Na webových stránkách můžete ho nastavit ze stavu webový server, než kódování oznámena do skriptu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-414">In webpages, you might want to set it from the web server's state, rather than coding it literally into the script.</span></span> <span data-ttu-id="56d7d-415">Například ve webové stránky vygenerované v aplikaci ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="56d7d-415">For example, in a webpage generated in an ASP.NET app:</span></span>

<span data-ttu-id="56d7d-416">*JavaScript ve Razor*</span><span class="sxs-lookup"><span data-stu-id="56d7d-416">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a><span data-ttu-id="56d7d-417">TelemetryContext</span><span class="sxs-lookup"><span data-stu-id="56d7d-417">TelemetryContext</span></span>
<span data-ttu-id="56d7d-418">TelemetryClient má vlastnost kontext, který obsahuje hodnoty, které jsou odeslány společně s všechny telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="56d7d-418">TelemetryClient has a Context property, which contains values that are sent along with all telemetry data.</span></span> <span data-ttu-id="56d7d-419">Za normálních okolností jsou nastavené moduly standardní telemetrie, ale můžete také nastavit jejich sami.</span><span class="sxs-lookup"><span data-stu-id="56d7d-419">They are normally set by the standard telemetry modules, but you can also set them yourself.</span></span> <span data-ttu-id="56d7d-420">Například:</span><span class="sxs-lookup"><span data-stu-id="56d7d-420">For example:</span></span>

    telemetry.Context.Operation.Name = "MyOperationName";

<span data-ttu-id="56d7d-421">Pokud jste nastavili některou z těchto hodnot sami, zvažte odebrání na příslušný řádek z [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)tak, aby vaše hodnoty a standardní hodnoty získat nerozumíte nemáte.</span><span class="sxs-lookup"><span data-stu-id="56d7d-421">If you set any of these values yourself, consider removing the relevant line from [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), so that your values and the standard values don't get confused.</span></span>

* <span data-ttu-id="56d7d-422">**Součást**: aplikace a její verzi.</span><span class="sxs-lookup"><span data-stu-id="56d7d-422">**Component**: The app and its version.</span></span>
* <span data-ttu-id="56d7d-423">**Zařízení**: Data o zařízení, na kterém je spuštěna aplikace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-423">**Device**: Data about the device where the app is running.</span></span> <span data-ttu-id="56d7d-424">(Ve službě web apps, to je serveru nebo klientském zařízení, který se odesílá telemetrii z.)</span><span class="sxs-lookup"><span data-stu-id="56d7d-424">(In web apps, this is the server or client device that the telemetry is sent from.)</span></span>
* <span data-ttu-id="56d7d-425">**InstrumentationKey**: The Application Insights prostředků v Azure, kde se zobrazí telemetrii.</span><span class="sxs-lookup"><span data-stu-id="56d7d-425">**InstrumentationKey**: The Application Insights resource in Azure where the telemetry appear.</span></span> <span data-ttu-id="56d7d-426">Ho je obvykle zachyceny ze souboru ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="56d7d-426">It's usually picked up from ApplicationInsights.config.</span></span>
* <span data-ttu-id="56d7d-427">**Umístění**: geografickém umístění zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d7d-427">**Location**: The geographic location of the device.</span></span>
* <span data-ttu-id="56d7d-428">**Operace**: ve službě web apps, aktuální žádost HTTP.</span><span class="sxs-lookup"><span data-stu-id="56d7d-428">**Operation**: In web apps, the current HTTP request.</span></span> <span data-ttu-id="56d7d-429">V jiných typů aplikací můžete tento parametr můžete nastavit pro skupiny událostí společně.</span><span class="sxs-lookup"><span data-stu-id="56d7d-429">In other app types, you can set this to group events together.</span></span>
  * <span data-ttu-id="56d7d-430">**ID**: generované hodnoty, které koreluje různých událostí, takže když si prohlédnout všechny události ve vyhledávání diagnostiky, můžete najít související položky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-430">**Id**: A generated value that correlates different events, so that when you inspect any event in Diagnostic Search, you can find related items.</span></span>
  * <span data-ttu-id="56d7d-431">**Název**: identifikátor, obvykle adresu URL požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="56d7d-431">**Name**: An identifier, usually the URL of the HTTP request.</span></span>
  * <span data-ttu-id="56d7d-432">**SyntheticSource**: není-li null nebo prázdný řetězec, který označuje, že zdroj žádosti bylo zjištěno, že robot nebo webový test.</span><span class="sxs-lookup"><span data-stu-id="56d7d-432">**SyntheticSource**: If not null or empty, a string that indicates that the source of the request has been identified as a robot or web test.</span></span> <span data-ttu-id="56d7d-433">Ve výchozím nastavení je vyloučen z výpočtů v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="56d7d-433">By default, it is excluded from calculations in Metrics Explorer.</span></span>
* <span data-ttu-id="56d7d-434">**Vlastnosti**: vlastnosti, které se odesílají s všechny telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="56d7d-434">**Properties**: Properties that are sent with all telemetry data.</span></span> <span data-ttu-id="56d7d-435">Může být přepsána v jednotlivých sledovat * volání.</span><span class="sxs-lookup"><span data-stu-id="56d7d-435">It can be overridden in individual Track* calls.</span></span>
* <span data-ttu-id="56d7d-436">**Relace**: uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="56d7d-436">**Session**: The user's session.</span></span> <span data-ttu-id="56d7d-437">ID nastavena pro generovanou hodnotu, která je změněn, pokud uživatel nebyl aktivní po dobu.</span><span class="sxs-lookup"><span data-stu-id="56d7d-437">The ID is set to a generated value, which is changed when the user has not been active for a while.</span></span>
* <span data-ttu-id="56d7d-438">**Uživatel**: informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="56d7d-438">**User**: User information.</span></span>

## <a name="limits"></a><span data-ttu-id="56d7d-439">Omezení</span><span class="sxs-lookup"><span data-stu-id="56d7d-439">Limits</span></span>
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

<span data-ttu-id="56d7d-440">Abyste se vyhnuli stiskne omezení četnosti data, použijte [vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-440">To avoid hitting the data rate limit, use [sampling](app-insights-sampling.md).</span></span>

<span data-ttu-id="56d7d-441">Chcete-li určit, jak dlouho se data ukládají najdete v tématu [uchovávání dat a ochrana osobních údajů](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-441">To determine how long data is kept, see [Data retention and privacy](app-insights-data-retention-privacy.md).</span></span>

## <a name="reference-docs"></a><span data-ttu-id="56d7d-442">Referenční dokumenty</span><span class="sxs-lookup"><span data-stu-id="56d7d-442">Reference docs</span></span>
* [<span data-ttu-id="56d7d-443">Rozhraní ASP.NET – reference</span><span class="sxs-lookup"><span data-stu-id="56d7d-443">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)
* [<span data-ttu-id="56d7d-444">Referenční informace sady Java</span><span class="sxs-lookup"><span data-stu-id="56d7d-444">Java reference</span></span>](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [<span data-ttu-id="56d7d-445">Referenční dokumentace technologie JavaScript</span><span class="sxs-lookup"><span data-stu-id="56d7d-445">JavaScript reference</span></span>](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [<span data-ttu-id="56d7d-446">Sada SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="56d7d-446">Android SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Android)
* [<span data-ttu-id="56d7d-447">Sada SDK pro iOS</span><span class="sxs-lookup"><span data-stu-id="56d7d-447">iOS SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a><span data-ttu-id="56d7d-448">Kód SDK</span><span class="sxs-lookup"><span data-stu-id="56d7d-448">SDK code</span></span>
* [<span data-ttu-id="56d7d-449">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="56d7d-449">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="56d7d-450">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="56d7d-450">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="56d7d-451">Balíčky systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="56d7d-451">Windows Server packages</span></span>](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [<span data-ttu-id="56d7d-452">Java SDK</span><span class="sxs-lookup"><span data-stu-id="56d7d-452">Java SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Java)
* [<span data-ttu-id="56d7d-453">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="56d7d-453">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)
* [<span data-ttu-id="56d7d-454">Všechny platformy</span><span class="sxs-lookup"><span data-stu-id="56d7d-454">All platforms</span></span>](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a><span data-ttu-id="56d7d-455">Otázky</span><span class="sxs-lookup"><span data-stu-id="56d7d-455">Questions</span></span>
* <span data-ttu-id="56d7d-456">*Jaké výjimky může vyvolat volání Track_()?*</span><span class="sxs-lookup"><span data-stu-id="56d7d-456">*What exceptions might Track_() calls throw?*</span></span>

    <span data-ttu-id="56d7d-457">Žádné.</span><span class="sxs-lookup"><span data-stu-id="56d7d-457">None.</span></span> <span data-ttu-id="56d7d-458">Nemusíte zabalení je v klauzulích try-catch.</span><span class="sxs-lookup"><span data-stu-id="56d7d-458">You don't need to wrap them in try-catch clauses.</span></span> <span data-ttu-id="56d7d-459">Pokud sada SDK zaznamená problémy, bude protokolování zpráv ve výstupu konzoly ladění a--zprávy získat prostřednictvím – ve vyhledávání diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="56d7d-459">If the SDK encounters problems, it will log messages in the debug console output and--if the messages get through--in Diagnostic Search.</span></span>
* <span data-ttu-id="56d7d-460">*Je k dispozici rozhraní REST API k získání dat z portálu?*</span><span class="sxs-lookup"><span data-stu-id="56d7d-460">*Is there a REST API to get data from the portal?*</span></span>

    <span data-ttu-id="56d7d-461">Ano, [rozhraní API pro přístup k datům](https://dev.applicationinsights.io/).</span><span class="sxs-lookup"><span data-stu-id="56d7d-461">Yes, the [data access API](https://dev.applicationinsights.io/).</span></span> <span data-ttu-id="56d7d-462">Zahrnout další způsoby, jak extrahovat data [exportovat z analýz do Power BI](app-insights-export-power-bi.md) a [průběžné export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="56d7d-462">Other ways to extract data include [export from Analytics to Power BI](app-insights-export-power-bi.md) and [continuous export](app-insights-export-telemetry.md).</span></span>

## <span data-ttu-id="56d7d-463"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="56d7d-463"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="56d7d-464">Hledání událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="56d7d-464">Search events and logs</span></span>](app-insights-diagnostic-search.md)

* [<span data-ttu-id="56d7d-465">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="56d7d-465">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)


