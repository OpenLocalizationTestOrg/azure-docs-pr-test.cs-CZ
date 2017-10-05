---
title: "Monitorování výkonu aplikací Java webů v systému Linux - Azure | Microsoft Docs"
description: "Rozšířené monitorování výkonu aplikací z vašeho webu Java pomocí modulu plug-in CollectD pro službu Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: 4ea917b068e0242bfb88d7357eca032607a43a3f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="15619-103">collectd: metriky výkonu Linux ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="15619-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="15619-104">Prozkoumat metriky výkonu systému Linux v [Application Insights](app-insights-overview.md), nainstalujte [collectd](http://collectd.org/), společně s jeho modulu plug-in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="15619-104">To explore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="15619-105">Toto řešení open source shromáždí různé statistické údaje systému a sítě.</span><span class="sxs-lookup"><span data-stu-id="15619-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="15619-106">Obvykle použijete collectd, pokud již máte [instrumentovány webovou službu Java pomocí Application Insights][java].</span><span class="sxs-lookup"><span data-stu-id="15619-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="15619-107">Nabízí více dat můžete vylepšit výkon vaší aplikace nebo diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="15619-107">It gives you more data to help you to enhance your app's performance or diagnose problems.</span></span> 

![Ukázkové grafy](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="15619-109">Získejte klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="15619-109">Get your instrumentation key</span></span>
<span data-ttu-id="15619-110">V [portálu Microsoft Azure](https://portal.azure.com), otevřete [Application Insights](app-insights-overview.md) prostředku, kde chcete data zobrazí.</span><span class="sxs-lookup"><span data-stu-id="15619-110">In the [Microsoft Azure portal](https://portal.azure.com), open the [Application Insights](app-insights-overview.md) resource where you want the data to appear.</span></span> <span data-ttu-id="15619-111">(Nebo [vytvořte nový prostředek](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="15619-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="15619-112">Trvat kopii klíč instrumentace, který identifikuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="15619-112">Take a copy of the instrumentation key, which identifies the resource.</span></span>

![Procházet všechny, otevřete prostředek a pak v Essentials rozevíracího seznamu, vyberte a zkopírujte klíč instrumentace](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a><span data-ttu-id="15619-114">Nainstalujte collectd a modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="15619-114">Install collectd and the plug-in</span></span>
<span data-ttu-id="15619-115">V počítačích serverů Linux:</span><span class="sxs-lookup"><span data-stu-id="15619-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="15619-116">Nainstalujte [collectd](http://collectd.org/) verze 5.4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="15619-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="15619-117">Stažení [plug-in Application Insights collectd zapisovače](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="15619-117">Download the [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="15619-118">Poznamenejte si číslo verze.</span><span class="sxs-lookup"><span data-stu-id="15619-118">Note the version number.</span></span>
3. <span data-ttu-id="15619-119">Tento modul plug-in JAR do kopie `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="15619-119">Copy the plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="15619-120">Upravit `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="15619-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="15619-121">Ujistěte se, že [modul Java plug-in](https://collectd.org/wiki/index.php/Plugin:Java) je povoleno.</span><span class="sxs-lookup"><span data-stu-id="15619-121">Ensure that [the Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="15619-122">Aktualizujte JVMArg pro java.class.path zahrnout následující JAR.</span><span class="sxs-lookup"><span data-stu-id="15619-122">Update the JVMArg for the java.class.path to include the following JAR.</span></span> <span data-ttu-id="15619-123">Aktualizujte číslo verze tak, aby odpovídaly ten, který jste stáhli:</span><span class="sxs-lookup"><span data-stu-id="15619-123">Update the version number to match the one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="15619-124">Přidejte tento fragment kódu, pomocí klíč instrumentace z prostředku:</span><span class="sxs-lookup"><span data-stu-id="15619-124">Add this snippet, using the Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="15619-125">Tady je součástí vzorový konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="15619-125">Here's part of a sample configuration file:</span></span>

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

<span data-ttu-id="15619-126">Konfigurace dalších [modulů plug-in collectd](https://collectd.org/wiki/index.php/Table_of_Plugins), který může shromažďovat různých dat z různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="15619-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="15619-127">Restartujte collectd podle jeho [ruční](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="15619-127">Restart collectd according to its [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-the-data-in-application-insights"></a><span data-ttu-id="15619-128">Zobrazení dat ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="15619-128">View the data in Application Insights</span></span>
<span data-ttu-id="15619-129">V prostředku Application Insights, otevřete [Průzkumníku metrik a grafy přidáte][metrics], výběr metriky, které chcete z vlastní kategorie.</span><span class="sxs-lookup"><span data-stu-id="15619-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting the metrics you want to see from the Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="15619-130">Ve výchozím nastavení jsou metriky agregovat přes všechny hostitele počítače, ze kterých byly shromážděny metriky.</span><span class="sxs-lookup"><span data-stu-id="15619-130">By default, the metrics are aggregated across all host machines from which the metrics were collected.</span></span> <span data-ttu-id="15619-131">K zobrazení metriky na hostiteli, v okně podrobností graf, zapněte seskupování a poté zvolte podle CollectD hostitele.</span><span class="sxs-lookup"><span data-stu-id="15619-131">To view the metrics per host, in the Chart details blade, turn on Grouping and then choose to group by CollectD-Host.</span></span>

## <a name="to-exclude-upload-of-specific-statistics"></a><span data-ttu-id="15619-132">Vyloučit odesílání konkrétní Statistika</span><span class="sxs-lookup"><span data-stu-id="15619-132">To exclude upload of specific statistics</span></span>
<span data-ttu-id="15619-133">Ve výchozím nastavení odešle modulem plug-in Application Insights všechna data shromažďovaná společností povoleno collectd číst modulů plug-in.</span><span class="sxs-lookup"><span data-stu-id="15619-133">By default, the Application Insights plugin sends all the data collected by all the enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="15619-134">Vyloučení dat z konkrétní modulů plug-in nebo zdrojů dat:</span><span class="sxs-lookup"><span data-stu-id="15619-134">To exclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="15619-135">Upravte konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="15619-135">Edit the configuration file.</span></span> 
* <span data-ttu-id="15619-136">V `<Plugin ApplicationInsightsWriter>`, přidejte direktivy řádky takto:</span><span class="sxs-lookup"><span data-stu-id="15619-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="15619-137">– Direktiva</span><span class="sxs-lookup"><span data-stu-id="15619-137">Directive</span></span> | <span data-ttu-id="15619-138">Efekt</span><span class="sxs-lookup"><span data-stu-id="15619-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="15619-139">Vyloučit všechny data shromažďovaná společností `disk` modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="15619-139">Exclude all data collected by the `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="15619-140">Vyloučení zdrojů s názvem `read` a `write` z `disk` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="15619-140">Exclude the sources named `read` and `write` from the `disk` plugin.</span></span> |

<span data-ttu-id="15619-141">Samostatné direktivy s nový řádek.</span><span class="sxs-lookup"><span data-stu-id="15619-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="15619-142">Problémy?</span><span class="sxs-lookup"><span data-stu-id="15619-142">Problems?</span></span>
<span data-ttu-id="15619-143">*Data na portálu se nezobrazí*</span><span class="sxs-lookup"><span data-stu-id="15619-143">*I don't see data in the portal*</span></span>

* <span data-ttu-id="15619-144">Otevřete [vyhledávání] [ diagnostic] chcete zobrazit, pokud byly přijaty nezpracované události.</span><span class="sxs-lookup"><span data-stu-id="15619-144">Open [Search][diagnostic] to see if the raw events have arrived.</span></span> <span data-ttu-id="15619-145">Někdy se trvat déle, než se objeví v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="15619-145">Sometimes they take longer to appear in metrics explorer.</span></span>
* <span data-ttu-id="15619-146">Možná budete muset [nastavit výjimky brány firewall pro odchozí data](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="15619-146">You might need to [set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="15619-147">Povolte trasování v modulem plug-in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="15619-147">Enable tracing in the Application Insights plugin.</span></span> <span data-ttu-id="15619-148">Přidejte tento řádek v rámci `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="15619-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="15619-149">Otevřete terminál a spusťte collectd v režimu s komentářem, zobrazíte jakýchkoliv problémů, které je generování sestav:</span><span class="sxs-lookup"><span data-stu-id="15619-149">Open a terminal and start collectd in verbose mode, to see any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="15619-150">Známý problém</span><span class="sxs-lookup"><span data-stu-id="15619-150">Known issue</span></span>

<span data-ttu-id="15619-151">Modul plug-in Application Insights zápisu není kompatibilní s určitým modulů plug-in pro čtení.</span><span class="sxs-lookup"><span data-stu-id="15619-151">The Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="15619-152">Některé moduly plug-in někdy odeslat "NaN", kde modulem plug-in Application Insights očekává číslo s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="15619-152">Some plugins sometimes send "NaN" where the Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="15619-153">Příznak: Protokol collectd zobrazuje chyby, které zahrnují "AI:... Chyba syntaxe: Neočekávaný token N ".</span><span class="sxs-lookup"><span data-stu-id="15619-153">Symptom: The collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="15619-154">Alternativní řešení: Vyloučit data shromažďovaná společností modulů plug-in zápisu problém.</span><span class="sxs-lookup"><span data-stu-id="15619-154">Workaround: Exclude data collected by the problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


