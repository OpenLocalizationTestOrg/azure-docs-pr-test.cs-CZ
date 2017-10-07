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
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: metriky výkonu Linux ve službě Application Insights


metriky výkonu systému tooexplore Linux v [Application Insights](app-insights-overview.md), nainstalujte [collectd](http://collectd.org/), společně s jeho modulu plug-in Application Insights. Toto řešení open source shromáždí různé statistické údaje systému a sítě.

Obvykle použijete collectd, pokud již máte [instrumentovány webovou službu Java pomocí Application Insights][java]. Nabízí další data toohelp jste tooenhance výkon vaší aplikace nebo diagnostiky problémů. 

![Ukázkové grafy](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>Získejte klíč instrumentace
V hello [portálu Microsoft Azure](https://portal.azure.com), otevřete hello [Application Insights](app-insights-overview.md) prostředku, kde chcete hello data tooappear. (Nebo [vytvořte nový prostředek](app-insights-create-new-resource.md).)

Trvat kopii hello klíč instrumentace, který identifikuje prostředek hello.

![Procházet všechny, otevřete prostředek a potom v hello Essentials rozevíracího seznamu, vyberte a zkopírujte hello klíč instrumentace](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a>Nainstalujte collectd a hello modulu plug-in
V počítačích serverů Linux:

1. Nainstalujte [collectd](http://collectd.org/) verze 5.4.0 nebo novější.
2. Stáhnout hello [plug-in Application Insights collectd zapisovače](https://aka.ms/aijavasdk). Poznamenejte si číslo verze hello.
3. Zkopírujte hello modulu plug-in JAR do `/usr/share/collectd/java`.
4. Upravit `/etc/collectd/collectd.conf`:
   * Ujistěte se, že [hello modul Java plug-in](https://collectd.org/wiki/index.php/Plugin:Java) je povoleno.
   * Aktualizujte hello JVMArg pro hello java.class.path tooinclude hello následující JAR. Aktualizace hello verze číslo toomatch hello, které jste stáhli:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Přidejte tento fragment kódu, pomocí hello klíč instrumentace z prostředku:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Tady je součástí vzorový konfigurační soubor:

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

Konfigurace dalších [modulů plug-in collectd](https://collectd.org/wiki/index.php/Table_of_Plugins), který může shromažďovat různých dat z různých zdrojů.

Restartujte collectd podle tooits [ruční](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-hello-data-in-application-insights"></a>Zobrazení dat hello ve službě Application Insights
V prostředku Application Insights, otevřete [Průzkumníku metrik a grafy přidáte][metrics], výběr hello metriky chcete toosee z hello vlastní kategorie.

![](./media/app-insights-java-collectd/result.png)

Ve výchozím nastavení jsou hello metriky agregovat přes všechny hostitele počítače, ze kterých byly shromážděny hello metriky. tooview hello metriky na hostiteli, v okně podrobností hello graf, zapněte seskupování a poté zvolte toogroup CollectD hostitele.

## <a name="tooexclude-upload-of-specific-statistics"></a>nahrávání tooexclude konkrétní statistiky
Ve výchozím nastavení odešle plug-in Application Insights hello všechny hello data shromažďovaná společností všechny collectd hello povoleno "read" modulů plug-in. 

tooexclude data z konkrétní modulů plug-in nebo zdrojů dat:

* Upravte konfigurační soubor hello. 
* V `<Plugin ApplicationInsightsWriter>`, přidejte direktivy řádky takto:

| – Direktiva | Efekt |
| --- | --- |
| `Exclude disk` |Vyloučit všechny data shromažďovaná společností hello `disk` modulu plug-in |
| `Exclude disk:read,write` |Vyloučit hello zdrojů s názvem `read` a `write` z hello `disk` modulu plug-in. |

Samostatné direktivy s nový řádek.

## <a name="problems"></a>Problémy?
*Data v portálu hello se nezobrazí*

* Otevřete [vyhledávání] [ diagnostic] toosee, pokud jste dostali hello nezpracované události. Někdy se trvat déle tooappear v Průzkumníku metrik.
* Může být nutné příliš[nastavit výjimky brány firewall pro odchozí data](app-insights-ip-addresses.md)
* Povolte trasování v modulu plug-in Application Insights hello. Přidejte tento řádek v rámci `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Otevřete terminál a spusťte collectd v režimu s komentářem, toosee jakýchkoliv problémů, které je generování sestav:
  * `sudo collectd -f`

## <a name="known-issue"></a>Známý problém

modul plug-in Application Insights zápisu Hello není kompatibilní s určitým modulů plug-in pro čtení. Některé moduly plug-in někdy odeslat "NaN", kde plug-in Application Insights hello očekává číslo s plovoucí desetinnou čárkou.

Příznak: protokol collectd hello zobrazuje chyby, které zahrnují "AI:... Chyba syntaxe: Neočekávaný token N ".

Alternativní řešení: Vyloučit data shromažďovaná společností hello problém zápisu modulů plug-in. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


