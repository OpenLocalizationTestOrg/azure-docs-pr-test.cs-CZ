---
title: "Filtrovat telemetrie Azure Application Insights ve webové aplikace Java | Microsoft Docs"
description: "Omezit přenos telemetrie filtrováním událostí, které nepotřebujete k monitorování."
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
ms.openlocfilehash: 5f6d6d4ad590b85810c42e9f9520850024c5446a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a><span data-ttu-id="085eb-103">Filtr telemetrie webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="085eb-103">Filter telemetry in your Java web app</span></span>

<span data-ttu-id="085eb-104">Filtry poskytnout způsob, jak vybrat telemetrii, vaše [webovou aplikaci Java odešle do služby Application Insights](app-insights-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="085eb-104">Filters provide a way to select the telemetry that your [Java web app sends to Application Insights](app-insights-java-get-started.md).</span></span> <span data-ttu-id="085eb-105">Existují některé filtry se na pole, které můžete použít, a můžete je zapsat také vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="085eb-105">There are some out-of-the-box filters that you can use, and you can also write your own custom filters.</span></span>

<span data-ttu-id="085eb-106">Filtry se na pole zahrnují:</span><span class="sxs-lookup"><span data-stu-id="085eb-106">The out-of-the-box filters include:</span></span>

* <span data-ttu-id="085eb-107">Úroveň závažnosti trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-107">Trace severity level</span></span>
* <span data-ttu-id="085eb-108">Konkrétní adresy URL, klíčová slova nebo kódy odpovědí</span><span class="sxs-lookup"><span data-stu-id="085eb-108">Specific URLs, keywords or response codes</span></span>
* <span data-ttu-id="085eb-109">Rychlou odezvu – to znamená, na které aplikace odpověděl rychle požadavky</span><span class="sxs-lookup"><span data-stu-id="085eb-109">Fast responses - that is, requests to which your app responded to quickly</span></span>
* <span data-ttu-id="085eb-110">Názvy konkrétním událostí</span><span class="sxs-lookup"><span data-stu-id="085eb-110">Specific event names</span></span>

> [!NOTE]
> <span data-ttu-id="085eb-111">Filtry zkreslit metrik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="085eb-111">Filters skew the metrics of your app.</span></span> <span data-ttu-id="085eb-112">Například můžete rozhodnout, že chcete-li diagnostikovat pomalé odezvy, můžete nastavit filtr vyřadí krátké doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="085eb-112">For example, you might decide that, in order to diagnose slow responses, you will set a filter to discard fast response times.</span></span> <span data-ttu-id="085eb-113">Ale je potřeba si uvědomit, že průměrná odezvy hlášené Application Insights bude nižší než skutečná rychlost a počet požadavků bude menší než skutečná počet.</span><span class="sxs-lookup"><span data-stu-id="085eb-113">But you must be aware that the average response times reported by Application Insights will then be slower than the true speed, and the count of requests will be smaller than the real count.</span></span>
> <span data-ttu-id="085eb-114">Pokud se jedná o problém, použijte [vzorkování](app-insights-sampling.md) místo.</span><span class="sxs-lookup"><span data-stu-id="085eb-114">If this is a concern, use [Sampling](app-insights-sampling.md) instead.</span></span>

## <a name="setting-filters"></a><span data-ttu-id="085eb-115">Nastavení filtrů</span><span class="sxs-lookup"><span data-stu-id="085eb-115">Setting filters</span></span>

<span data-ttu-id="085eb-116">Přidejte soubor ApplicationInsights.xml, `TelemetryProcessors` části jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="085eb-116">In ApplicationInsights.xml, add a `TelemetryProcessors` section like this example:</span></span>


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
                  <!-- Names of events we don't want to see -->
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




<span data-ttu-id="085eb-117">[Zkontrolujte úplnou sadu předdefinovaných procesorů](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span><span class="sxs-lookup"><span data-stu-id="085eb-117">[Inspect the full set of built-in processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span></span>

## <a name="built-in-filters"></a><span data-ttu-id="085eb-118">Integrované filtry</span><span class="sxs-lookup"><span data-stu-id="085eb-118">Built-in filters</span></span>

### <a name="metric-telemetry-filter"></a><span data-ttu-id="085eb-119">Metriky Telemetrie filtru</span><span class="sxs-lookup"><span data-stu-id="085eb-119">Metric Telemetry filter</span></span>

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* <span data-ttu-id="085eb-120">`NotNeeded`-Čárkami oddělený seznam názvů vlastní metriky.</span><span class="sxs-lookup"><span data-stu-id="085eb-120">`NotNeeded` - Comma-separated list of custom metric names.</span></span>


### <a name="page-view-telemetry-filter"></a><span data-ttu-id="085eb-121">Filtr Telemetrická zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="085eb-121">Page View Telemetry filter</span></span>

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* <span data-ttu-id="085eb-122">`DurationThresholdInMS`-Doba trvání odkazuje na čas potřebný k načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="085eb-122">`DurationThresholdInMS` - Duration refers to the time taken to load the page.</span></span> <span data-ttu-id="085eb-123">Pokud je toto nastaveno, nejsou hlášeny stránky, které načtené rychleji, než tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="085eb-123">If this is set, pages that loaded faster than this time are not reported.</span></span>
* <span data-ttu-id="085eb-124">`NotNeededNames`-Čárkami oddělený seznam názvů stránky.</span><span class="sxs-lookup"><span data-stu-id="085eb-124">`NotNeededNames` - Comma-separated list of page names.</span></span>
* <span data-ttu-id="085eb-125">`NotNeededUrls`-Fragmenty čárkami oddělený seznam adresy URL.</span><span class="sxs-lookup"><span data-stu-id="085eb-125">`NotNeededUrls` - Comma-separated list of URL fragments.</span></span> <span data-ttu-id="085eb-126">Například `"home"` odfiltruje všechny stránky, které mají "Domů" v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="085eb-126">For example, `"home"` filters out all pages that have "home" in the URL.</span></span>


### <a name="request-telemetry-filter"></a><span data-ttu-id="085eb-127">Filtr Telemetrie požadavku</span><span class="sxs-lookup"><span data-stu-id="085eb-127">Request Telemetry Filter</span></span>


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a><span data-ttu-id="085eb-128">Syntetické zdrojový filtr</span><span class="sxs-lookup"><span data-stu-id="085eb-128">Synthetic Source filter</span></span>

<span data-ttu-id="085eb-129">Odfiltruje všechny telemetrická data, která mají hodnoty ve vlastnosti SyntheticSource.</span><span class="sxs-lookup"><span data-stu-id="085eb-129">Filters out all telemetry that have values in the SyntheticSource property.</span></span> <span data-ttu-id="085eb-130">Mezi ně patří požadavky od robotů, pavouci a testy dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="085eb-130">These include requests from bots, spiders and availability tests.</span></span>

<span data-ttu-id="085eb-131">Filtrovat telemetrická data pro všechny syntetické požadavky:</span><span class="sxs-lookup"><span data-stu-id="085eb-131">Filter out telemetry for all synthetic requests:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" />
```

<span data-ttu-id="085eb-132">Filtrovat telemetrie pro konkrétní syntetické zdroje:</span><span class="sxs-lookup"><span data-stu-id="085eb-132">Filter out telemetry for specific synthetic sources:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* <span data-ttu-id="085eb-133">`NotNeeded`-Čárkami oddělený seznam názvů syntetické zdroje.</span><span class="sxs-lookup"><span data-stu-id="085eb-133">`NotNeeded` - Comma-separated list of synthetic source names.</span></span>

### <a name="telemetry-event-filter"></a><span data-ttu-id="085eb-134">Filtr událostí telemetrie</span><span class="sxs-lookup"><span data-stu-id="085eb-134">Telemetry Event filter</span></span>

<span data-ttu-id="085eb-135">Filtry vlastních událostí (přihlášení pomocí [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span><span class="sxs-lookup"><span data-stu-id="085eb-135">Filters custom events (logged using [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span></span>


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* <span data-ttu-id="085eb-136">`NotNeededNames`-Čárkami oddělený seznam názvů událostí.</span><span class="sxs-lookup"><span data-stu-id="085eb-136">`NotNeededNames` - Comma-separated list of event names.</span></span>


### <a name="trace-telemetry-filter"></a><span data-ttu-id="085eb-137">Filtr Telemetrie trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-137">Trace Telemetry filter</span></span>

<span data-ttu-id="085eb-138">Filtry protokolu trasování (přihlášení pomocí [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) nebo [protokolování framework kolekce](app-insights-java-trace-logs.md)).</span><span class="sxs-lookup"><span data-stu-id="085eb-138">Filters log traces (logged using [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) or a [logging framework collector](app-insights-java-trace-logs.md)).</span></span>

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* <span data-ttu-id="085eb-139">`FromSeverityLevel`Platné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="085eb-139">`FromSeverityLevel` valid values are:</span></span>
 *  <span data-ttu-id="085eb-140">VYPNOUT - vyfiltrovat všech trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-140">OFF             - Filter out ALL traces</span></span>
 *  <span data-ttu-id="085eb-141">TRASOVÁNÍ – žádné filtrování.</span><span class="sxs-lookup"><span data-stu-id="085eb-141">TRACE           - No filtering.</span></span> <span data-ttu-id="085eb-142">hodnotu úroveň trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-142">equals to Trace level</span></span>
 *  <span data-ttu-id="085eb-143">Informace o - filtru na úroveň trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-143">INFO            - Filter out TRACE level</span></span>
 *  <span data-ttu-id="085eb-144">VAROVÁNÍ - trasování a informace o filtru</span><span class="sxs-lookup"><span data-stu-id="085eb-144">WARN            - Filter out TRACE and INFO</span></span>
 *  <span data-ttu-id="085eb-145">Chyba: filtru se varování, informace trasování</span><span class="sxs-lookup"><span data-stu-id="085eb-145">ERROR           - Filter out WARN, INFO, TRACE</span></span>
 *  <span data-ttu-id="085eb-146">KRITICKÝ - filtru se jenom důležité</span><span class="sxs-lookup"><span data-stu-id="085eb-146">CRITICAL        - filter out all but CRITICAL</span></span>


## <a name="custom-filters"></a><span data-ttu-id="085eb-147">Vlastní filtry</span><span class="sxs-lookup"><span data-stu-id="085eb-147">Custom filters</span></span>

### <a name="1-code-your-filter"></a><span data-ttu-id="085eb-148">1. Kód filtru</span><span class="sxs-lookup"><span data-stu-id="085eb-148">1. Code your filter</span></span>

<span data-ttu-id="085eb-149">V kódu, vytvořte třídu, která implementuje `TelemetryProcessor`:</span><span class="sxs-lookup"><span data-stu-id="085eb-149">In your code, create a class that implements `TelemetryProcessor`:</span></span>

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
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


### <a name="2-invoke-your-filter-in-the-configuration-file"></a><span data-ttu-id="085eb-150">2. Vyvolání filtru v konfiguračním souboru</span><span class="sxs-lookup"><span data-stu-id="085eb-150">2. Invoke your filter in the configuration file</span></span>

<span data-ttu-id="085eb-151">V ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="085eb-151">In ApplicationInsights.xml:</span></span>

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

## <a name="troubleshooting"></a><span data-ttu-id="085eb-152">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="085eb-152">Troubleshooting</span></span>

<span data-ttu-id="085eb-153">*Moje filtru není funkční.*</span><span class="sxs-lookup"><span data-stu-id="085eb-153">*My filter isn't working.*</span></span>

* <span data-ttu-id="085eb-154">Zkontrolujte, zda jste zadali platný parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="085eb-154">Check that you have provided valid parameter values.</span></span> <span data-ttu-id="085eb-155">Například doby trvání musí být celá čísla.</span><span class="sxs-lookup"><span data-stu-id="085eb-155">For example, durations should be integers.</span></span> <span data-ttu-id="085eb-156">Neplatné hodnoty způsobí, že filtr budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="085eb-156">Invalid values will cause the filter to be ignored.</span></span> <span data-ttu-id="085eb-157">Pokud vaše vlastní filtr vyvolá výjimku z konstruktoru nebo metoda set, se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="085eb-157">If your custom filter throws an exception from a constructor or set method, it will be ignored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="085eb-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="085eb-158">Next steps</span></span>

* <span data-ttu-id="085eb-159">[Vzorkování](app-insights-sampling.md) – zvažte vzorkování jako alternativu, která není zkreslit vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="085eb-159">[Sampling](app-insights-sampling.md) - Consider sampling as an alternative that does not skew your metrics.</span></span>
