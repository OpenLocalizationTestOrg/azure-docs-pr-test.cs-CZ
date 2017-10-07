---
title: "aaaFilter telemetrie Azure Application Insights ve webové aplikace Java | Microsoft Docs"
description: "Omezit přenos telemetrie filtrováním hello událostí toomonitor nepotřebujete."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 95713e11d5f86472777c67e4e7f3177fbf2cd0b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a><span data-ttu-id="dd5c1-103">Filtr telemetrie webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="dd5c1-103">Filter telemetry in your Java web app</span></span>

<span data-ttu-id="dd5c1-104">Filtry stanovit způsob tooselect hello telemetrie, že vaše [webovou aplikaci Java odešle tooApplication Insights](app-insights-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="dd5c1-104">Filters provide a way tooselect hello telemetry that your [Java web app sends tooApplication Insights](app-insights-java-get-started.md).</span></span> <span data-ttu-id="dd5c1-105">Existují některé filtry se na pole, které můžete použít, a můžete je zapsat také vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-105">There are some out-of-the-box filters that you can use, and you can also write your own custom filters.</span></span>

<span data-ttu-id="dd5c1-106">Hello se na pole filtry zahrnují:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-106">hello out-of-the-box filters include:</span></span>

* <span data-ttu-id="dd5c1-107">Úroveň závažnosti trasování</span><span class="sxs-lookup"><span data-stu-id="dd5c1-107">Trace severity level</span></span>
* <span data-ttu-id="dd5c1-108">Konkrétní adresy URL, klíčová slova nebo kódy odpovědí</span><span class="sxs-lookup"><span data-stu-id="dd5c1-108">Specific URLs, keywords or response codes</span></span>
* <span data-ttu-id="dd5c1-109">Rychlou odezvu – tj. požadavky toowhich aplikace odpověděl tooquickly</span><span class="sxs-lookup"><span data-stu-id="dd5c1-109">Fast responses - that is, requests toowhich your app responded tooquickly</span></span>
* <span data-ttu-id="dd5c1-110">Názvy konkrétním událostí</span><span class="sxs-lookup"><span data-stu-id="dd5c1-110">Specific event names</span></span>

> [!NOTE]
> <span data-ttu-id="dd5c1-111">Filtry zkreslit hello metrik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-111">Filters skew hello metrics of your app.</span></span> <span data-ttu-id="dd5c1-112">Například můžete rozhodnout, že v pořadí toodiagnose pomalé odezvy, nastavíte filtru toodiscard krátké doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-112">For example, you might decide that, in order toodiagnose slow responses, you will set a filter toodiscard fast response times.</span></span> <span data-ttu-id="dd5c1-113">Ale musíte být vědomi hello průměrná odezvy hlášené Application Insights bude nižší než rychlosti hello true, a bude menší než skutečná počet hello hello počet požadavků.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-113">But you must be aware that hello average response times reported by Application Insights will then be slower than hello true speed, and hello count of requests will be smaller than hello real count.</span></span>
> <span data-ttu-id="dd5c1-114">Pokud se jedná o problém, použijte [vzorkování](app-insights-sampling.md) místo.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-114">If this is a concern, use [Sampling](app-insights-sampling.md) instead.</span></span>

## <a name="setting-filters"></a><span data-ttu-id="dd5c1-115">Nastavení filtrů</span><span class="sxs-lookup"><span data-stu-id="dd5c1-115">Setting filters</span></span>

<span data-ttu-id="dd5c1-116">Přidejte soubor ApplicationInsights.xml, `TelemetryProcessors` části jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-116">In ApplicationInsights.xml, add a `TelemetryProcessors` section like this example:</span></span>


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want toosee -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




<span data-ttu-id="dd5c1-117">[Zkontrolujte hello úplnou sadu předdefinovaných procesorů](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span><span class="sxs-lookup"><span data-stu-id="dd5c1-117">[Inspect hello full set of built-in processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span></span>

## <a name="built-in-filters"></a><span data-ttu-id="dd5c1-118">Integrované filtry</span><span class="sxs-lookup"><span data-stu-id="dd5c1-118">Built-in filters</span></span>

### <a name="metric-telemetry-filter"></a><span data-ttu-id="dd5c1-119">Metriky Telemetrie filtru</span><span class="sxs-lookup"><span data-stu-id="dd5c1-119">Metric Telemetry filter</span></span>

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* <span data-ttu-id="dd5c1-120">`NotNeeded`-Čárkami oddělený seznam názvů vlastní metriky.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-120">`NotNeeded` - Comma-separated list of custom metric names.</span></span>


### <a name="page-view-telemetry-filter"></a><span data-ttu-id="dd5c1-121">Filtr Telemetrická zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="dd5c1-121">Page View Telemetry filter</span></span>

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* <span data-ttu-id="dd5c1-122">`DurationThresholdInMS`-Doba trvání odkazuje toohello doba tooload hello stránky.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-122">`DurationThresholdInMS` - Duration refers toohello time taken tooload hello page.</span></span> <span data-ttu-id="dd5c1-123">Pokud je toto nastaveno, nejsou hlášeny stránky, které načtené rychleji, než tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-123">If this is set, pages that loaded faster than this time are not reported.</span></span>
* <span data-ttu-id="dd5c1-124">`NotNeededNames`-Čárkami oddělený seznam názvů stránky.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-124">`NotNeededNames` - Comma-separated list of page names.</span></span>
* <span data-ttu-id="dd5c1-125">`NotNeededUrls`-Fragmenty čárkami oddělený seznam adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-125">`NotNeededUrls` - Comma-separated list of URL fragments.</span></span> <span data-ttu-id="dd5c1-126">Například `"home"` odfiltruje všechny stránky, které mají "Domů" v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-126">For example, `"home"` filters out all pages that have "home" in hello URL.</span></span>


### <a name="request-telemetry-filter"></a><span data-ttu-id="dd5c1-127">Filtr Telemetrie požadavku</span><span class="sxs-lookup"><span data-stu-id="dd5c1-127">Request Telemetry Filter</span></span>


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a><span data-ttu-id="dd5c1-128">Syntetické zdrojový filtr</span><span class="sxs-lookup"><span data-stu-id="dd5c1-128">Synthetic Source filter</span></span>

<span data-ttu-id="dd5c1-129">Odfiltruje všechny telemetrická data, která mají hodnoty v hello SyntheticSource vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-129">Filters out all telemetry that have values in hello SyntheticSource property.</span></span> <span data-ttu-id="dd5c1-130">Mezi ně patří požadavky od robotů, pavouci a testy dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-130">These include requests from bots, spiders and availability tests.</span></span>

<span data-ttu-id="dd5c1-131">Filtrovat telemetrická data pro všechny syntetické požadavky:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-131">Filter out telemetry for all synthetic requests:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" />
```

<span data-ttu-id="dd5c1-132">Filtrovat telemetrie pro konkrétní syntetické zdroje:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-132">Filter out telemetry for specific synthetic sources:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* <span data-ttu-id="dd5c1-133">`NotNeeded`-Čárkami oddělený seznam názvů syntetické zdroje.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-133">`NotNeeded` - Comma-separated list of synthetic source names.</span></span>

### <a name="telemetry-event-filter"></a><span data-ttu-id="dd5c1-134">Filtr událostí telemetrie</span><span class="sxs-lookup"><span data-stu-id="dd5c1-134">Telemetry Event filter</span></span>

<span data-ttu-id="dd5c1-135">Filtry vlastních událostí (přihlášení pomocí [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span><span class="sxs-lookup"><span data-stu-id="dd5c1-135">Filters custom events (logged using [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span></span>


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* <span data-ttu-id="dd5c1-136">`NotNeededNames`-Čárkami oddělený seznam názvů událostí.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-136">`NotNeededNames` - Comma-separated list of event names.</span></span>


### <a name="trace-telemetry-filter"></a><span data-ttu-id="dd5c1-137">Filtr Telemetrie trasování</span><span class="sxs-lookup"><span data-stu-id="dd5c1-137">Trace Telemetry filter</span></span>

<span data-ttu-id="dd5c1-138">Filtry protokolu trasování (přihlášení pomocí [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) nebo [protokolování framework kolekce](app-insights-java-trace-logs.md)).</span><span class="sxs-lookup"><span data-stu-id="dd5c1-138">Filters log traces (logged using [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) or a [logging framework collector](app-insights-java-trace-logs.md)).</span></span>

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* <span data-ttu-id="dd5c1-139">`FromSeverityLevel`Platné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-139">`FromSeverityLevel` valid values are:</span></span>
 *  <span data-ttu-id="dd5c1-140">VYPNOUT - vyfiltrovat všech trasování</span><span class="sxs-lookup"><span data-stu-id="dd5c1-140">OFF             - Filter out ALL traces</span></span>
 *  <span data-ttu-id="dd5c1-141">TRASOVÁNÍ – žádné filtrování.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-141">TRACE           - No filtering.</span></span> <span data-ttu-id="dd5c1-142">úroveň tooTrace je rovno</span><span class="sxs-lookup"><span data-stu-id="dd5c1-142">equals tooTrace level</span></span>
 *  <span data-ttu-id="dd5c1-143">Informace o - filtru na úroveň trasování</span><span class="sxs-lookup"><span data-stu-id="dd5c1-143">INFO            - Filter out TRACE level</span></span>
 *  <span data-ttu-id="dd5c1-144">VAROVÁNÍ - trasování a informace o filtru</span><span class="sxs-lookup"><span data-stu-id="dd5c1-144">WARN            - Filter out TRACE and INFO</span></span>
 *  <span data-ttu-id="dd5c1-145">Chyba: filtru se varování, informace trasování</span><span class="sxs-lookup"><span data-stu-id="dd5c1-145">ERROR           - Filter out WARN, INFO, TRACE</span></span>
 *  <span data-ttu-id="dd5c1-146">KRITICKÝ - filtru se jenom důležité</span><span class="sxs-lookup"><span data-stu-id="dd5c1-146">CRITICAL        - filter out all but CRITICAL</span></span>


## <a name="custom-filters"></a><span data-ttu-id="dd5c1-147">Vlastní filtry</span><span class="sxs-lookup"><span data-stu-id="dd5c1-147">Custom filters</span></span>

### <a name="1-code-your-filter"></a><span data-ttu-id="dd5c1-148">1. Kód filtru</span><span class="sxs-lookup"><span data-stu-id="dd5c1-148">1. Code your filter</span></span>

<span data-ttu-id="dd5c1-149">V kódu, vytvořte třídu, která implementuje `TelemetryProcessor`:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-149">In your code, create a class that implements `TelemetryProcessor`:</span></span>

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required toosupport hello filter.*/
       private final String successful;

       /* Initializers for hello parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry toobe sent.
          Return false toodiscard it.
          Return true tooallow other processors tooinspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-hello-configuration-file"></a><span data-ttu-id="dd5c1-150">2. Vyvolání filtru v konfiguračním souboru hello</span><span class="sxs-lookup"><span data-stu-id="dd5c1-150">2. Invoke your filter in hello configuration file</span></span>

<span data-ttu-id="dd5c1-151">V ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="dd5c1-151">In ApplicationInsights.xml:</span></span>

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

## <a name="troubleshooting"></a><span data-ttu-id="dd5c1-152">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dd5c1-152">Troubleshooting</span></span>

<span data-ttu-id="dd5c1-153">*Moje filtru není funkční.*</span><span class="sxs-lookup"><span data-stu-id="dd5c1-153">*My filter isn't working.*</span></span>

* <span data-ttu-id="dd5c1-154">Zkontrolujte, zda jste zadali platný parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-154">Check that you have provided valid parameter values.</span></span> <span data-ttu-id="dd5c1-155">Například doby trvání musí být celá čísla.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-155">For example, durations should be integers.</span></span> <span data-ttu-id="dd5c1-156">Neplatné hodnoty způsobí, že toobe filtru hello ignorovány.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-156">Invalid values will cause hello filter toobe ignored.</span></span> <span data-ttu-id="dd5c1-157">Pokud vaše vlastní filtr vyvolá výjimku z konstruktoru nebo metoda set, se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-157">If your custom filter throws an exception from a constructor or set method, it will be ignored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd5c1-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd5c1-158">Next steps</span></span>

* <span data-ttu-id="dd5c1-159">[Vzorkování](app-insights-sampling.md) – zvažte vzorkování jako alternativu, která není zkreslit vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="dd5c1-159">[Sampling](app-insights-sampling.md) - Consider sampling as an alternative that does not skew your metrics.</span></span>
