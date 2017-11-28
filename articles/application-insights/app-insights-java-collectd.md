---
title: "aaaMonitor výkonu aplikací Java webů v systému Linux - Azure | Microsoft Docs"
description: "Rozšířené monitorování výkonu aplikací z vašeho webu Java pomocí hello CollectD modulu plug-in pro službu Application Insights."
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
ms.openlocfilehash: f783e8607a83b2b43f67d3a2fc20f100aa2f75ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="d9bba-103">collectd: metriky výkonu Linux ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="d9bba-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="d9bba-104">metriky výkonu systému tooexplore Linux v [Application Insights](app-insights-overview.md), nainstalujte [collectd](http://collectd.org/), společně s jeho modulu plug-in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d9bba-104">tooexplore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="d9bba-105">Toto řešení open source shromáždí různé statistické údaje systému a sítě.</span><span class="sxs-lookup"><span data-stu-id="d9bba-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="d9bba-106">Obvykle použijete collectd, pokud již máte [instrumentovány webovou službu Java pomocí Application Insights][java].</span><span class="sxs-lookup"><span data-stu-id="d9bba-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="d9bba-107">Nabízí další data toohelp jste tooenhance výkon vaší aplikace nebo diagnostiky problémů.</span><span class="sxs-lookup"><span data-stu-id="d9bba-107">It gives you more data toohelp you tooenhance your app's performance or diagnose problems.</span></span> 

![Ukázkové grafy](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="d9bba-109">Získejte klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="d9bba-109">Get your instrumentation key</span></span>
<span data-ttu-id="d9bba-110">V hello [portálu Microsoft Azure](https://portal.azure.com), otevřete hello [Application Insights](app-insights-overview.md) prostředku, kde chcete hello data tooappear.</span><span class="sxs-lookup"><span data-stu-id="d9bba-110">In hello [Microsoft Azure portal](https://portal.azure.com), open hello [Application Insights](app-insights-overview.md) resource where you want hello data tooappear.</span></span> <span data-ttu-id="d9bba-111">(Nebo [vytvořte nový prostředek](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="d9bba-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="d9bba-112">Trvat kopii hello klíč instrumentace, který identifikuje prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="d9bba-112">Take a copy of hello instrumentation key, which identifies hello resource.</span></span>

![Procházet všechny, otevřete prostředek a potom v hello Essentials rozevíracího seznamu, vyberte a zkopírujte hello klíč instrumentace](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a><span data-ttu-id="d9bba-114">Nainstalujte collectd a hello modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="d9bba-114">Install collectd and hello plug-in</span></span>
<span data-ttu-id="d9bba-115">V počítačích serverů Linux:</span><span class="sxs-lookup"><span data-stu-id="d9bba-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="d9bba-116">Nainstalujte [collectd](http://collectd.org/) verze 5.4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d9bba-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="d9bba-117">Stáhnout hello [plug-in Application Insights collectd zapisovače](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="d9bba-117">Download hello [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="d9bba-118">Poznamenejte si číslo verze hello.</span><span class="sxs-lookup"><span data-stu-id="d9bba-118">Note hello version number.</span></span>
3. <span data-ttu-id="d9bba-119">Zkopírujte hello modulu plug-in JAR do `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="d9bba-119">Copy hello plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="d9bba-120">Upravit `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="d9bba-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="d9bba-121">Ujistěte se, že [hello modul Java plug-in](https://collectd.org/wiki/index.php/Plugin:Java) je povoleno.</span><span class="sxs-lookup"><span data-stu-id="d9bba-121">Ensure that [hello Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="d9bba-122">Aktualizujte hello JVMArg pro hello java.class.path tooinclude hello následující JAR.</span><span class="sxs-lookup"><span data-stu-id="d9bba-122">Update hello JVMArg for hello java.class.path tooinclude hello following JAR.</span></span> <span data-ttu-id="d9bba-123">Aktualizace hello verze číslo toomatch hello, které jste stáhli:</span><span class="sxs-lookup"><span data-stu-id="d9bba-123">Update hello version number toomatch hello one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="d9bba-124">Přidejte tento fragment kódu, pomocí hello klíč instrumentace z prostředku:</span><span class="sxs-lookup"><span data-stu-id="d9bba-124">Add this snippet, using hello Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="d9bba-125">Tady je součástí vzorový konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="d9bba-125">Here's part of a sample configuration file:</span></span>

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

<span data-ttu-id="d9bba-126">Konfigurace dalších [modulů plug-in collectd](https://collectd.org/wiki/index.php/Table_of_Plugins), který může shromažďovat různých dat z různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="d9bba-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="d9bba-127">Restartujte collectd podle tooits [ruční](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="d9bba-127">Restart collectd according tooits [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-hello-data-in-application-insights"></a><span data-ttu-id="d9bba-128">Zobrazení dat hello ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="d9bba-128">View hello data in Application Insights</span></span>
<span data-ttu-id="d9bba-129">V prostředku Application Insights, otevřete [Průzkumníku metrik a grafy přidáte][metrics], výběr hello metriky chcete toosee z hello vlastní kategorie.</span><span class="sxs-lookup"><span data-stu-id="d9bba-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting hello metrics you want toosee from hello Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="d9bba-130">Ve výchozím nastavení jsou hello metriky agregovat přes všechny hostitele počítače, ze kterých byly shromážděny hello metriky.</span><span class="sxs-lookup"><span data-stu-id="d9bba-130">By default, hello metrics are aggregated across all host machines from which hello metrics were collected.</span></span> <span data-ttu-id="d9bba-131">tooview hello metriky na hostiteli, v okně podrobností hello graf, zapněte seskupování a poté zvolte toogroup CollectD hostitele.</span><span class="sxs-lookup"><span data-stu-id="d9bba-131">tooview hello metrics per host, in hello Chart details blade, turn on Grouping and then choose toogroup by CollectD-Host.</span></span>

## <a name="tooexclude-upload-of-specific-statistics"></a><span data-ttu-id="d9bba-132">nahrávání tooexclude konkrétní statistiky</span><span class="sxs-lookup"><span data-stu-id="d9bba-132">tooexclude upload of specific statistics</span></span>
<span data-ttu-id="d9bba-133">Ve výchozím nastavení odešle plug-in Application Insights hello všechny hello data shromažďovaná společností všechny collectd hello povoleno "read" modulů plug-in.</span><span class="sxs-lookup"><span data-stu-id="d9bba-133">By default, hello Application Insights plugin sends all hello data collected by all hello enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="d9bba-134">tooexclude data z konkrétní modulů plug-in nebo zdrojů dat:</span><span class="sxs-lookup"><span data-stu-id="d9bba-134">tooexclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="d9bba-135">Upravte konfigurační soubor hello.</span><span class="sxs-lookup"><span data-stu-id="d9bba-135">Edit hello configuration file.</span></span> 
* <span data-ttu-id="d9bba-136">V `<Plugin ApplicationInsightsWriter>`, přidejte direktivy řádky takto:</span><span class="sxs-lookup"><span data-stu-id="d9bba-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="d9bba-137">– Direktiva</span><span class="sxs-lookup"><span data-stu-id="d9bba-137">Directive</span></span> | <span data-ttu-id="d9bba-138">Efekt</span><span class="sxs-lookup"><span data-stu-id="d9bba-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="d9bba-139">Vyloučit všechny data shromažďovaná společností hello `disk` modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="d9bba-139">Exclude all data collected by hello `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="d9bba-140">Vyloučit hello zdrojů s názvem `read` a `write` z hello `disk` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="d9bba-140">Exclude hello sources named `read` and `write` from hello `disk` plugin.</span></span> |

<span data-ttu-id="d9bba-141">Samostatné direktivy s nový řádek.</span><span class="sxs-lookup"><span data-stu-id="d9bba-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="d9bba-142">Problémy?</span><span class="sxs-lookup"><span data-stu-id="d9bba-142">Problems?</span></span>
<span data-ttu-id="d9bba-143">*Data v portálu hello se nezobrazí*</span><span class="sxs-lookup"><span data-stu-id="d9bba-143">*I don't see data in hello portal*</span></span>

* <span data-ttu-id="d9bba-144">Otevřete [vyhledávání] [ diagnostic] toosee, pokud jste dostali hello nezpracované události.</span><span class="sxs-lookup"><span data-stu-id="d9bba-144">Open [Search][diagnostic] toosee if hello raw events have arrived.</span></span> <span data-ttu-id="d9bba-145">Někdy se trvat déle tooappear v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="d9bba-145">Sometimes they take longer tooappear in metrics explorer.</span></span>
* <span data-ttu-id="d9bba-146">Může být nutné příliš[nastavit výjimky brány firewall pro odchozí data](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="d9bba-146">You might need too[set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="d9bba-147">Povolte trasování v modulu plug-in Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="d9bba-147">Enable tracing in hello Application Insights plugin.</span></span> <span data-ttu-id="d9bba-148">Přidejte tento řádek v rámci `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="d9bba-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="d9bba-149">Otevřete terminál a spusťte collectd v režimu s komentářem, toosee jakýchkoliv problémů, které je generování sestav:</span><span class="sxs-lookup"><span data-stu-id="d9bba-149">Open a terminal and start collectd in verbose mode, toosee any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="d9bba-150">Známý problém</span><span class="sxs-lookup"><span data-stu-id="d9bba-150">Known issue</span></span>

<span data-ttu-id="d9bba-151">modul plug-in Application Insights zápisu Hello není kompatibilní s určitým modulů plug-in pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d9bba-151">hello Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="d9bba-152">Některé moduly plug-in někdy odeslat "NaN", kde plug-in Application Insights hello očekává číslo s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="d9bba-152">Some plugins sometimes send "NaN" where hello Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="d9bba-153">Příznak: protokol collectd hello zobrazuje chyby, které zahrnují "AI:... Chyba syntaxe: Neočekávaný token N ".</span><span class="sxs-lookup"><span data-stu-id="d9bba-153">Symptom: hello collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="d9bba-154">Alternativní řešení: Vyloučit data shromažďovaná společností hello problém zápisu modulů plug-in.</span><span class="sxs-lookup"><span data-stu-id="d9bba-154">Workaround: Exclude data collected by hello problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


