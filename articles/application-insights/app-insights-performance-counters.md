---
title: "čítače aaaPerformance ve službě Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 0a51c225f1d1124c9e7fe89f34e747cb26a3589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="system-performance-counters-in-application-insights"></a><span data-ttu-id="3bdae-103">Čítače výkonu systému ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="3bdae-103">System performance counters in Application Insights</span></span>
<span data-ttu-id="3bdae-104">Systém Windows nabízí širokou škálu [čítače výkonu](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) například obsazení procesoru, paměti, disku a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="3bdae-104">Windows provides a wide variety of [performance counters](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) such as CPU occupancy, memory, disk, and network usage.</span></span> <span data-ttu-id="3bdae-105">Můžete také definovat vlastní.</span><span class="sxs-lookup"><span data-stu-id="3bdae-105">You can also define your own.</span></span> <span data-ttu-id="3bdae-106">[Application Insights](app-insights-overview.md) můžete zobrazit tyto čítače výkonu, pokud vaše aplikace běží v rámci služby IIS na toowhich místního hostitele nebo virtuálního počítače budete mít přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="3bdae-106">[Application Insights](app-insights-overview.md) can show these performance counters if your application is running under IIS on an on-premises host or virtual machine toowhich you have administrative access.</span></span> <span data-ttu-id="3bdae-107">grafy Hello znamenat hello prostředky k dispozici tooyour živé aplikace a může pomoct tooidentify nevyváženou zatížení mezi instancemi serveru.</span><span class="sxs-lookup"><span data-stu-id="3bdae-107">hello charts indicate hello resources available tooyour live application, and can help tooidentify unbalanced load between server instances.</span></span>

<span data-ttu-id="3bdae-108">Čítače výkonu se zobrazí v okně hello servery, které obsahuje tabulku této segmenty instance serveru.</span><span class="sxs-lookup"><span data-stu-id="3bdae-108">Performance counters appear in hello Servers blade, which includes a table that segments by server instance.</span></span>

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

<span data-ttu-id="3bdae-110">(Čítače výkonu nejsou k dispozici pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3bdae-110">(Performance counters aren't available for Azure Web Apps.</span></span> <span data-ttu-id="3bdae-111">Ale můžete [odesílání Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)</span><span class="sxs-lookup"><span data-stu-id="3bdae-111">But you can [send Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)</span></span>

## <a name="view-counters"></a><span data-ttu-id="3bdae-112">Zobrazení čítačů</span><span class="sxs-lookup"><span data-stu-id="3bdae-112">View counters</span></span>
<span data-ttu-id="3bdae-113">okno servery Hello ukazuje výchozí sadu čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="3bdae-113">hello Servers blade shows a default set of performance counters.</span></span> 

<span data-ttu-id="3bdae-114">toosee jiných čítačů buď upravit hello grafy v okně servery hello nebo otevřete nové [Průzkumníku metrik](app-insights-metrics-explorer.md) okno a přidejte nové grafy.</span><span class="sxs-lookup"><span data-stu-id="3bdae-114">toosee other counters, either edit hello charts on hello Servers blade, or open a new [Metrics Explorer](app-insights-metrics-explorer.md) blade and add new charts.</span></span> 

<span data-ttu-id="3bdae-115">k dispozici čítače Hello jsou označeny jako metriky, když se upravit graf.</span><span class="sxs-lookup"><span data-stu-id="3bdae-115">hello available counters are listed as metrics when you edit a chart.</span></span>

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

<span data-ttu-id="3bdae-117">Vytvořte všechny nejužitečnější grafy na jednom místě, toosee [řídicí panel](app-insights-dashboards.md) a připnete ji tooit.</span><span class="sxs-lookup"><span data-stu-id="3bdae-117">toosee all your most useful charts in one place, create a [dashboard](app-insights-dashboards.md) and pin them tooit.</span></span>

## <a name="add-counters"></a><span data-ttu-id="3bdae-118">Přidání čítačů</span><span class="sxs-lookup"><span data-stu-id="3bdae-118">Add counters</span></span>
<span data-ttu-id="3bdae-119">Pokud není hello čítače výkonu, které chcete zobrazit v seznamu hello metrik, protože hello Application Insights SDK není shromažďování ve vašem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="3bdae-119">If hello performance counter you want isn't shown in hello list of metrics, that's because hello Application Insights SDK isn't collecting it in your web server.</span></span> <span data-ttu-id="3bdae-120">Můžete ho nakonfigurovat toodo tak.</span><span class="sxs-lookup"><span data-stu-id="3bdae-120">You can configure it toodo so.</span></span>

1. <span data-ttu-id="3bdae-121">Zjistěte, jaké čítače jsou k dispozici na vašem serveru pomocí tohoto příkazu Powershellu na serveru hello:</span><span class="sxs-lookup"><span data-stu-id="3bdae-121">Find out what counters are available in your server by using this PowerShell command at hello server:</span></span>
   
    `Get-Counter -ListSet *`
   
    <span data-ttu-id="3bdae-122">(Viz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)</span><span class="sxs-lookup"><span data-stu-id="3bdae-122">(See [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)</span></span>
2. <span data-ttu-id="3bdae-123">Otevřete soubor ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="3bdae-123">Open ApplicationInsights.config.</span></span>
   
   * <span data-ttu-id="3bdae-124">Pokud jste přidali aplikaci tooyour Application Insights během vývoje, upravit soubor ApplicationInsights.config ve vašem projektu a poté ji znovu nasadit tooyour servery.</span><span class="sxs-lookup"><span data-stu-id="3bdae-124">If you added Application Insights tooyour app during development, edit ApplicationInsights.config in your project, and then re-deploy it tooyour servers.</span></span>
   * <span data-ttu-id="3bdae-125">Pokud jste použili tooinstrument monitorování stavu webové aplikace za běhu, najít soubor ApplicationInsights.config v kořenovém adresáři hello hello aplikace ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="3bdae-125">If you used Status Monitor tooinstrument a web app at runtime, find ApplicationInsights.config in hello root directory of hello app in IIS.</span></span> <span data-ttu-id="3bdae-126">Aktualizaci existuje v každé instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="3bdae-126">Update it there in each server instance.</span></span>
3. <span data-ttu-id="3bdae-127">Úpravy – direktiva kolekce výkonu hello:</span><span class="sxs-lookup"><span data-stu-id="3bdae-127">Edit hello performance collector directive:</span></span>
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

<span data-ttu-id="3bdae-128">Můžete zaznamenat standardní čítače i těch, které že jste implementovali sami.</span><span class="sxs-lookup"><span data-stu-id="3bdae-128">You can capture both standard counters and those you have implemented yourself.</span></span> <span data-ttu-id="3bdae-129">`\Objects\Processes`Příkladem standardní čítač je k dispozici na všechny systémy Windows.</span><span class="sxs-lookup"><span data-stu-id="3bdae-129">`\Objects\Processes` is an example of a standard counter, available on all Windows systems.</span></span> <span data-ttu-id="3bdae-130">`\Sales(photo)\# Items Sold`je příklad vlastní čítače, který může být implementována ve webové službě.</span><span class="sxs-lookup"><span data-stu-id="3bdae-130">`\Sales(photo)\# Items Sold` is an example of a custom counter that might be implemented in a web service.</span></span> 

<span data-ttu-id="3bdae-131">Formát Hello je `\Category(instance)\Counter"`, nebo kategorie, které nemají instancí, právě `\Category\Counter`.</span><span class="sxs-lookup"><span data-stu-id="3bdae-131">hello format is `\Category(instance)\Counter"`, or for categories that don't have instances, just `\Category\Counter`.</span></span>

<span data-ttu-id="3bdae-132">`ReportAs`je vyžadována pro názvy čítačů, které se neshodují `[a-zA-Z()/-_ \.]+` – to znamená, obsahují znaky, které nejsou v hello následující sady: písmena, kulaté závorky, lomítkem, pomlčky, podtržítka, místo, tečka.</span><span class="sxs-lookup"><span data-stu-id="3bdae-132">`ReportAs` is required for counter names that do not match `[a-zA-Z()/-_ \.]+` - that is, they contain characters that are not in hello following sets: letters, round brackets, forward slash, hyphen, underscore, space, dot.</span></span>

<span data-ttu-id="3bdae-133">Pokud zadáte instance, budou shromážděna metrika udávaný dimenzi "CounterInstanceName" Dobrý den.</span><span class="sxs-lookup"><span data-stu-id="3bdae-133">If you specify an instance, it will be collected as a dimension "CounterInstanceName" of hello reported metric.</span></span>

### <a name="collecting-performance-counters-in-code"></a><span data-ttu-id="3bdae-134">Shromažďování čítače výkonu v kódu</span><span class="sxs-lookup"><span data-stu-id="3bdae-134">Collecting performance counters in code</span></span>
<span data-ttu-id="3bdae-135">výkon systému toocollect čítače a jejich odeslání tooApplication statistiky, můžete přizpůsobit hello fragment kódu níže:</span><span class="sxs-lookup"><span data-stu-id="3bdae-135">toocollect system performance counters and send them tooApplication Insights, you can adapt hello snippet below:</span></span>


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

<span data-ttu-id="3bdae-136">Nebo to můžete provést hello samé s vlastní metriky, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="3bdae-136">Or you can do hello same thing with custom metrics you created:</span></span>

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a><span data-ttu-id="3bdae-137">Čítače výkonu v Analytics</span><span class="sxs-lookup"><span data-stu-id="3bdae-137">Performance counters in Analytics</span></span>
<span data-ttu-id="3bdae-138">Můžete vyhledat a zobrazit sestavy čítače výkonu v [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="3bdae-138">You can search and display performance counter reports in [Analytics](app-insights-analytics.md).</span></span>

<span data-ttu-id="3bdae-139">Hello **čítače výkonu** schématu zpřístupní hello `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="3bdae-139">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span>  <span data-ttu-id="3bdae-140">V hello telemetrických dat pro každou aplikaci zobrazí se pouze hello čítače pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bdae-140">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="3bdae-141">Například toosee čítače, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="3bdae-141">For example, toosee what counters are available:</span></span> 

![Čítače výkonu v analytics Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

<span data-ttu-id="3bdae-143">('Instance, zde označují toohello instance čítače výkonu, není hello role nebo serveru instance počítače.</span><span class="sxs-lookup"><span data-stu-id="3bdae-143">('Instance' here refers toohello performance counter instance,  not hello role or server machine instance.</span></span> <span data-ttu-id="3bdae-144">Název instance čítače výkonu Hello obvykle segmenty čítače například využití procesoru podle názvu hello hello procesu nebo aplikace.)</span><span class="sxs-lookup"><span data-stu-id="3bdae-144">hello performance counter instance name typically segments counters such as processor time by hello name of hello process or application.)</span></span>

<span data-ttu-id="3bdae-145">tooget graf dostupné paměti přes hello poslední období:</span><span class="sxs-lookup"><span data-stu-id="3bdae-145">tooget a chart of available memory over hello recent period:</span></span> 

![Paměť timechart v analytics Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

<span data-ttu-id="3bdae-147">Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující hello identitu hello hostitele serveru instance, na kterém aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="3bdae-147">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host server instance on which your app is running.</span></span> <span data-ttu-id="3bdae-148">Například toocompare hello výkonu vaší aplikace na různé počítače hello:</span><span class="sxs-lookup"><span data-stu-id="3bdae-148">For example, toocompare hello performance of your app on hello different machines:</span></span> 

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a><span data-ttu-id="3bdae-150">Technologie ASP.NET a počty Application Insights</span><span class="sxs-lookup"><span data-stu-id="3bdae-150">ASP.NET and Application Insights counts</span></span>
<span data-ttu-id="3bdae-151">*Co je hello rozdíl mezi rychlost hello výjimky a výjimky metriky?*</span><span class="sxs-lookup"><span data-stu-id="3bdae-151">*What's hello difference between hello Exception rate and Exceptions metrics?*</span></span>

* <span data-ttu-id="3bdae-152">*Míra výjimka* je čítače výkonu systému.</span><span class="sxs-lookup"><span data-stu-id="3bdae-152">*Exception rate* is a system performance counter.</span></span> <span data-ttu-id="3bdae-153">Hello CLR spočítá všechny hello zpracovat a neošetřených výjimek, které jsou vyvolány a vydělí hello celkem v intervalu vzorkování prahovou hodnotou hello délka intervalu hello.</span><span class="sxs-lookup"><span data-stu-id="3bdae-153">hello CLR counts all hello handled and unhandled exceptions that are thrown, and divides hello total in a sampling interval by hello length of hello interval.</span></span> <span data-ttu-id="3bdae-154">Hello Application Insights SDK shromažďuje tento výsledek a odešle ji toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="3bdae-154">hello Application Insights SDK collects this result and sends it toohello portal.</span></span>
* <span data-ttu-id="3bdae-155">*Výjimky* je počet hello TrackException sestavy přijatých hello portálu v intervalu vzorkování hello hello grafu.</span><span class="sxs-lookup"><span data-stu-id="3bdae-155">*Exceptions* is a count of hello TrackException reports received by hello portal in hello sampling interval of hello chart.</span></span> <span data-ttu-id="3bdae-156">Obsahuje pouze hello zpracovává výjimky, kde jste napsali TrackException zavolá do vašeho kódu a neobsahuje všechny [neošetřené výjimky](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="3bdae-156">It includes only hello handled exceptions where you have written TrackException calls in your code, and doesn't include all [unhandled exceptions](app-insights-asp-net-exceptions.md).</span></span> 

## <a name="alerts"></a><span data-ttu-id="3bdae-157">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="3bdae-157">Alerts</span></span>
<span data-ttu-id="3bdae-158">Jako další metriky můžete [nastavit upozornění](app-insights-alerts.md) toowarn, pokud čítače výkonu ocitne mimo omezení zadáte.</span><span class="sxs-lookup"><span data-stu-id="3bdae-158">Like other metrics, you can [set an alert](app-insights-alerts.md) toowarn you if a performance counter goes outside a limit you specify.</span></span> <span data-ttu-id="3bdae-159">Otevřete okno hello výstrahy a klikněte na tlačítko Přidat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="3bdae-159">Open hello Alerts blade and click Add Alert.</span></span>

## <span data-ttu-id="3bdae-160"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bdae-160"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="3bdae-161">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="3bdae-161">Dependency tracking</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="3bdae-162">Sledování výjimek</span><span class="sxs-lookup"><span data-stu-id="3bdae-162">Exception tracking</span></span>](app-insights-asp-net-exceptions.md)

