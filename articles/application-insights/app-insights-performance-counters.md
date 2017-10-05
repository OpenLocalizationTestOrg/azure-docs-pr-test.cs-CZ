---
title: "Čítače výkonu ve službě Application Insights | Microsoft Docs"
description: "Systém monitorování a vlastní čítače výkonu .NET ve službě Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: bwren
ms.openlocfilehash: 038d6e051be8112b9264e7efa6485965d11e32c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="system-performance-counters-in-application-insights"></a><span data-ttu-id="7d05b-103">Čítače výkonu systému ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="7d05b-103">System performance counters in Application Insights</span></span>
<span data-ttu-id="7d05b-104">Systém Windows nabízí širokou škálu [čítače výkonu](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) například obsazení procesoru, paměti, disku a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="7d05b-104">Windows provides a wide variety of [performance counters](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) such as CPU occupancy, memory, disk, and network usage.</span></span> <span data-ttu-id="7d05b-105">Můžete také definovat vlastní.</span><span class="sxs-lookup"><span data-stu-id="7d05b-105">You can also define your own.</span></span> <span data-ttu-id="7d05b-106">[Application Insights](app-insights-overview.md) můžete zobrazit tyto čítače výkonu, pokud vaše aplikace běží v rámci služby IIS na místního hostitele nebo virtuální počítač, ke kterému mají přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-106">[Application Insights](app-insights-overview.md) can show these performance counters if your application is running under IIS on an on-premises host or virtual machine to which you have administrative access.</span></span> <span data-ttu-id="7d05b-107">Grafy znamenat prostředky k dispozici pro vaše živé aplikace a může pomoct identifikovat nevyváženou zatížení mezi instancemi serveru.</span><span class="sxs-lookup"><span data-stu-id="7d05b-107">The charts indicate the resources available to your live application, and can help to identify unbalanced load between server instances.</span></span>

<span data-ttu-id="7d05b-108">Čítače výkonu se zobrazí v okně servery, které obsahuje tabulku této segmenty instance serveru.</span><span class="sxs-lookup"><span data-stu-id="7d05b-108">Performance counters appear in the Servers blade, which includes a table that segments by server instance.</span></span>

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

<span data-ttu-id="7d05b-110">(Čítače výkonu nejsou k dispozici pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7d05b-110">(Performance counters aren't available for Azure Web Apps.</span></span> <span data-ttu-id="7d05b-111">Ale můžete [odesílání Azure Diagnostics Application Insights](app-insights-azure-diagnostics.md).)</span><span class="sxs-lookup"><span data-stu-id="7d05b-111">But you can [send Azure Diagnostics to Application Insights](app-insights-azure-diagnostics.md).)</span></span>

## <a name="view-counters"></a><span data-ttu-id="7d05b-112">Zobrazení čítačů</span><span class="sxs-lookup"><span data-stu-id="7d05b-112">View counters</span></span>
<span data-ttu-id="7d05b-113">V okně servery ukazuje výchozí sadu čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-113">The Servers blade shows a default set of performance counters.</span></span> 

<span data-ttu-id="7d05b-114">Informace o jiných čítačů, buď upravit grafy v okně servery nebo otevřete nové [Průzkumníku metrik](app-insights-metrics-explorer.md) okno a přidejte nové grafy.</span><span class="sxs-lookup"><span data-stu-id="7d05b-114">To see other counters, either edit the charts on the Servers blade, or open a new [Metrics Explorer](app-insights-metrics-explorer.md) blade and add new charts.</span></span> 

<span data-ttu-id="7d05b-115">K dispozici čítače jsou označeny jako metriky, když se upravit graf.</span><span class="sxs-lookup"><span data-stu-id="7d05b-115">The available counters are listed as metrics when you edit a chart.</span></span>

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

<span data-ttu-id="7d05b-117">Chcete-li zobrazit všechny nejužitečnější grafy na jednom místě, vytvořte [řídicí panel](app-insights-dashboards.md) a kód pin je k němu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-117">To see all your most useful charts in one place, create a [dashboard](app-insights-dashboards.md) and pin them to it.</span></span>

## <a name="add-counters"></a><span data-ttu-id="7d05b-118">Přidání čítačů</span><span class="sxs-lookup"><span data-stu-id="7d05b-118">Add counters</span></span>
<span data-ttu-id="7d05b-119">Není-li čítač výkonu, který má být zobrazena v seznamu metriky, je to způsobeno Application Insights SDK není shromažďování ve vašem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="7d05b-119">If the performance counter you want isn't shown in the list of metrics, that's because the Application Insights SDK isn't collecting it in your web server.</span></span> <span data-ttu-id="7d05b-120">Můžete nakonfigurovat ji tak.</span><span class="sxs-lookup"><span data-stu-id="7d05b-120">You can configure it to do so.</span></span>

1. <span data-ttu-id="7d05b-121">Zjistěte, jaké čítače jsou k dispozici na vašem serveru pomocí tohoto příkazu Powershellu na serveru:</span><span class="sxs-lookup"><span data-stu-id="7d05b-121">Find out what counters are available in your server by using this PowerShell command at the server:</span></span>
   
    `Get-Counter -ListSet *`
   
    <span data-ttu-id="7d05b-122">(Viz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)</span><span class="sxs-lookup"><span data-stu-id="7d05b-122">(See [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)</span></span>
2. <span data-ttu-id="7d05b-123">Otevřete soubor ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="7d05b-123">Open ApplicationInsights.config.</span></span>
   
   * <span data-ttu-id="7d05b-124">Pokud jste přidali Application Insights do vaší aplikace během vývoje, upravit soubor ApplicationInsights.config ve vašem projektu a pak znovu nasadit na vaše servery.</span><span class="sxs-lookup"><span data-stu-id="7d05b-124">If you added Application Insights to your app during development, edit ApplicationInsights.config in your project, and then re-deploy it to your servers.</span></span>
   * <span data-ttu-id="7d05b-125">Pokud jste použili monitorování stavu instrumentovat webové aplikace za běhu, najít soubor ApplicationInsights.config v kořenovém adresáři aplikace ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="7d05b-125">If you used Status Monitor to instrument a web app at runtime, find ApplicationInsights.config in the root directory of the app in IIS.</span></span> <span data-ttu-id="7d05b-126">Aktualizaci existuje v každé instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="7d05b-126">Update it there in each server instance.</span></span>
3. <span data-ttu-id="7d05b-127">Úpravy – direktiva kolekce výkonu:</span><span class="sxs-lookup"><span data-stu-id="7d05b-127">Edit the performance collector directive:</span></span>
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

<span data-ttu-id="7d05b-128">Můžete zaznamenat standardní čítače i těch, které že jste implementovali sami.</span><span class="sxs-lookup"><span data-stu-id="7d05b-128">You can capture both standard counters and those you have implemented yourself.</span></span> <span data-ttu-id="7d05b-129">`\Objects\Processes`Příkladem standardní čítač je k dispozici na všechny systémy Windows.</span><span class="sxs-lookup"><span data-stu-id="7d05b-129">`\Objects\Processes` is an example of a standard counter, available on all Windows systems.</span></span> <span data-ttu-id="7d05b-130">`\Sales(photo)\# Items Sold`je příklad vlastní čítače, který může být implementována ve webové službě.</span><span class="sxs-lookup"><span data-stu-id="7d05b-130">`\Sales(photo)\# Items Sold` is an example of a custom counter that might be implemented in a web service.</span></span> 

<span data-ttu-id="7d05b-131">Formát je `\Category(instance)\Counter"`, nebo kategorie, které nemají instancí, právě `\Category\Counter`.</span><span class="sxs-lookup"><span data-stu-id="7d05b-131">The format is `\Category(instance)\Counter"`, or for categories that don't have instances, just `\Category\Counter`.</span></span>

<span data-ttu-id="7d05b-132">`ReportAs`je vyžadována pro názvy čítačů, které se neshodují `[a-zA-Z()/-_ \.]+` – to znamená, že obsahovat znaky, které nejsou v následujících sadách: písmena, kulaté závorky, lomítkem, pomlčky, podtržítka, místo, tečka.</span><span class="sxs-lookup"><span data-stu-id="7d05b-132">`ReportAs` is required for counter names that do not match `[a-zA-Z()/-_ \.]+` - that is, they contain characters that are not in the following sets: letters, round brackets, forward slash, hyphen, underscore, space, dot.</span></span>

<span data-ttu-id="7d05b-133">Pokud zadáte instance, budou shromažďovány jako dimenze "CounterInstanceName" hlášené metriky.</span><span class="sxs-lookup"><span data-stu-id="7d05b-133">If you specify an instance, it will be collected as a dimension "CounterInstanceName" of the reported metric.</span></span>

### <a name="collecting-performance-counters-in-code"></a><span data-ttu-id="7d05b-134">Shromažďování čítače výkonu v kódu</span><span class="sxs-lookup"><span data-stu-id="7d05b-134">Collecting performance counters in code</span></span>
<span data-ttu-id="7d05b-135">Pokud chcete shromáždit čítače výkonu systému a jejich odeslání do služby Application Insights, můžete přizpůsobit fragmentu níže:</span><span class="sxs-lookup"><span data-stu-id="7d05b-135">To collect system performance counters and send them to Application Insights, you can adapt the snippet below:</span></span>


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

<span data-ttu-id="7d05b-136">Nebo můžete provést totéž s vlastní metriky, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="7d05b-136">Or you can do the same thing with custom metrics you created:</span></span>

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a><span data-ttu-id="7d05b-137">Čítače výkonu v Analytics</span><span class="sxs-lookup"><span data-stu-id="7d05b-137">Performance counters in Analytics</span></span>
<span data-ttu-id="7d05b-138">Můžete vyhledat a zobrazit sestavy čítače výkonu v [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="7d05b-138">You can search and display performance counter reports in [Analytics](app-insights-analytics.md).</span></span>

<span data-ttu-id="7d05b-139">**Čítače výkonu** schématu zpřístupní `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-139">The **performanceCounters** schema exposes the `category`, `counter` name, and `instance` name of each performance counter.</span></span>  <span data-ttu-id="7d05b-140">V telemetrii pro každou aplikaci zobrazí se pouze čítačů pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d05b-140">In the telemetry for each application, you’ll see only the counters for that application.</span></span> <span data-ttu-id="7d05b-141">Například pokud chcete zobrazit jsou k dispozici co čítače:</span><span class="sxs-lookup"><span data-stu-id="7d05b-141">For example, to see what counters are available:</span></span> 

![Čítače výkonu v analytics Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

<span data-ttu-id="7d05b-143">('Instance, zde označují instance čítače výkonu, není instance počítače role nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="7d05b-143">('Instance' here refers to the performance counter instance,  not the role or server machine instance.</span></span> <span data-ttu-id="7d05b-144">Název instance čítače výkonu obvykle segmenty čítače například využití procesoru podle názvu procesu nebo aplikace.)</span><span class="sxs-lookup"><span data-stu-id="7d05b-144">The performance counter instance name typically segments counters such as processor time by the name of the process or application.)</span></span>

<span data-ttu-id="7d05b-145">Pokud chcete získat graf dostupné paměti za poslední období:</span><span class="sxs-lookup"><span data-stu-id="7d05b-145">To get a chart of available memory over the recent period:</span></span> 

![Paměť timechart v analytics Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

<span data-ttu-id="7d05b-147">Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující identita instance hostitele serveru, na kterém aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="7d05b-147">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates the identity of the host server instance on which your app is running.</span></span> <span data-ttu-id="7d05b-148">Chcete-li například porovnat výkon vaší aplikace na různé počítače:</span><span class="sxs-lookup"><span data-stu-id="7d05b-148">For example, to compare the performance of your app on the different machines:</span></span> 

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a><span data-ttu-id="7d05b-150">Technologie ASP.NET a počty Application Insights</span><span class="sxs-lookup"><span data-stu-id="7d05b-150">ASP.NET and Application Insights counts</span></span>
<span data-ttu-id="7d05b-151">*Jaký je rozdíl mezi rychlost výjimky a výjimky metriky?*</span><span class="sxs-lookup"><span data-stu-id="7d05b-151">*What's the difference between the Exception rate and Exceptions metrics?*</span></span>

* <span data-ttu-id="7d05b-152">*Míra výjimka* je čítače výkonu systému.</span><span class="sxs-lookup"><span data-stu-id="7d05b-152">*Exception rate* is a system performance counter.</span></span> <span data-ttu-id="7d05b-153">Modul CLR spočítá všechny zpracovávaný a neošetřené výjimky, které jsou vyvolány a součet vydělí v intervalu vzorkování prahovou hodnotou délku intervalu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-153">The CLR counts all the handled and unhandled exceptions that are thrown, and divides the total in a sampling interval by the length of the interval.</span></span> <span data-ttu-id="7d05b-154">Application Insights SDK shromažďuje tento výsledek a odešle ji do portálu.</span><span class="sxs-lookup"><span data-stu-id="7d05b-154">The Application Insights SDK collects this result and sends it to the portal.</span></span>
* <span data-ttu-id="7d05b-155">*Výjimky* je počet přijatých portálu v intervalu vzorkování grafu TrackException sestavy.</span><span class="sxs-lookup"><span data-stu-id="7d05b-155">*Exceptions* is a count of the TrackException reports received by the portal in the sampling interval of the chart.</span></span> <span data-ttu-id="7d05b-156">Obsahuje pouze zpracovávaný výjimky, kde jste napsali TrackException zavolá do vašeho kódu a neobsahuje všechny [neošetřené výjimky](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="7d05b-156">It includes only the handled exceptions where you have written TrackException calls in your code, and doesn't include all [unhandled exceptions](app-insights-asp-net-exceptions.md).</span></span> 

## <a name="alerts"></a><span data-ttu-id="7d05b-157">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="7d05b-157">Alerts</span></span>
<span data-ttu-id="7d05b-158">Jako další metriky můžete [nastavit upozornění](app-insights-alerts.md) varovat, pokud čítače výkonu přejde mimo omezení zadáte.</span><span class="sxs-lookup"><span data-stu-id="7d05b-158">Like other metrics, you can [set an alert](app-insights-alerts.md) to warn you if a performance counter goes outside a limit you specify.</span></span> <span data-ttu-id="7d05b-159">Otevřete okno Výstrahy a klikněte na tlačítko Přidat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7d05b-159">Open the Alerts blade and click Add Alert.</span></span>

## <span data-ttu-id="7d05b-160"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d05b-160"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="7d05b-161">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="7d05b-161">Dependency tracking</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="7d05b-162">Sledování výjimek</span><span class="sxs-lookup"><span data-stu-id="7d05b-162">Exception tracking</span></span>](app-insights-asp-net-exceptions.md)

