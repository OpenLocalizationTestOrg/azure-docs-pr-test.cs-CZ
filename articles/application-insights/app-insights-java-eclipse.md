---
title: "aaaGet začít s Azure Application Insights s Javou v prostředí Eclipse | Microsoft docs"
description: "Použít hello Eclipse modulu plug-in tooadd výkonu a využití sledování tooyour webu Java pomocí Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 3142a26a9e2d14c2c433882e3d337f2a8c8f2247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Začínáme s Application Insights s Javou v prostředí Eclipse
Hello Application Insights SDK odesílá telemetrii z webové aplikace Java, takže je můžete analyzovat využití a výkonu. Hello Eclipse modulu plug-in pro službu Application Insights automaticky nainstaluje hello SDK do projektu, abyste měli mimo hello pole telemetrie a rozhraní API, které můžete použít vlastní telemetrii toowrite.   

## <a name="prerequisites"></a>Požadavky
Modul plug-in hello aktuálně funguje pro projekty Maven a dynamické webové projekty v prostředí Eclipse.
([Přidat službu Application Insights tooother typy projektu Java][java].)

Budete potřebovat:

* Oracle JRE 1.6 nebo novější
* Předplatné příliš[Microsoft Azure](https://azure.microsoft.com/).
* [Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/), džínovinu nebo novější.
* Windows 7 nebo novější nebo Windows Server 2008 nebo novějším.

## <a name="install-hello-sdk-on-eclipse-one-time"></a>Nainstalujte hello SDK Eclipse (jednou)
Můžete mít pouze toodo tento jednou pro každý počítač. Tento krok nainstaluje sada nástrojů, který poté můžete přidat hello SDK tooeach Dynamic Web Project.

1. V prostředí Eclipse klikněte na tlačítko Nápověda, nainstalovat nový Software.

    ![Nápověda, instalace nového softwaru](./media/app-insights-java-eclipse/0-plugin.png)
2. Hello SDK se http://dl.microsoft.com/eclipse v rámci Azure Toolkit.
3. Zrušte zaškrtnutí políčka **obraťte se na všechny lokality aktualizace...**

    ![Application Insights SDK zrušte vám poskytne všechny aktualizace lokality](./media/app-insights-java-eclipse/1-plugin.png)

Postupujte podle zbývajících kroků pro každý projekt, Java hello.

## <a name="create-an-application-insights-resource-in-azure"></a>Vytvořte prostředek Application Insights v Azure
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Vytvořte nový prostředek Application Insights. Nastavit hello aplikace typu tooJava webové aplikace.  

    ![Klikněte na + a vyberte Application Insights](./media/app-insights-java-eclipse/01-create.png)  

4. Najít hello klíč instrumentace nového prostředku hello. Budete potřebovat toopaste tím do projektu kódu za chvíli.  

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a>Přidání Application Insights tooyour projektu
1. Přidejte Application Insights z kontextové nabídky hello webového projektu Java.

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/02-context-menu.png)
2. Vložení hello klíč instrumentace, který jste získali z portálu Azure hello.

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/03-ikey.png)

klíč Hello se odesílají společně s každou položkou telemetrie a říká toodisplay Application Insights je ve vašem prostředku.

## <a name="run-hello-application-and-see-metrics"></a>Spuštění aplikace hello a zobrazit metriky
Spusťte svoji aplikaci.

Vrátí prostředek Application Insights tooyour v Microsoft Azure.

Data požadavků HTTP se zobrazí v okně Přehled hello. (Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)

![Odpověď serveru, počty žádostí a selhání ](./media/app-insights-java-eclipse/5-results.png)

Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky.

![Počty žádostí podle názvu](./media/app-insights-java-eclipse/6-barchart.png)

[Další informace o metrikách.][metrics]

A při zobrazení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.

![Všechny trasování pro tento požadavek](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>Telemetrických dat na straně klienta
V okně rychlého startu hello klikněte na tlačítko získat toomonitor kódu webové stránky:

![V okně přehledu aplikace zvolte rychlý Start, získat kód toomonitor webové stránky. Zkopírujte skript hello.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Vložte fragment kódu hello hello head souborů HTML.

#### <a name="view-client-side-data"></a>Zobrazení dat na straně klienta
Otevřete aktualizované webových stránek a jejich použití. Počkejte minutu nebo dvě a poté vrátí tooApplication Insights a otevřete hello využití okno. (V okně Přehled hello, posuňte se dolů a klikněte na využití.)

Stránka zobrazení, uživatele a relace metriky se zobrazí v okně využití hello:

![Relace, uživatelů a zobrazení stránek](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Další informace o nastavení telemetrických dat na straně klienta.][usage]

## <a name="publish-your-application"></a>Publikování aplikace
Teď publikujte aplikačního serveru toohello, uživatelé mohou ho použít a sledujte telemetrii hello objeví na portálu hello.

* Ujistěte se, že brána firewall umožňuje vaší aplikace toosend telemetrie toothese porty:

  * dc.services.visualstudio.com:443
  * dc.services.visualstudio.com:80
  * f5.services.visualstudio.com:443
  * f5.services.visualstudio.com:80
* Na serverech Windows nainstalujte:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Umožňuje čítače výkonu.)

## <a name="exceptions-and-request-failures"></a>Výjimky a chyby požadavků
Nezpracované výjimky jsou shromažďovány automaticky:

![](./media/app-insights-java-eclipse/21-exceptions.png)

toocollect data o dalších výjimkách, máte dvě možnosti:

* [Vložení volání tooTrackException ve vašem kódu](app-insights-api-custom-events-metrics.md#trackexception).
* [Nainstalujte na server agenta Java hello](app-insights-java-agent.md). Zadáte hello metody, které chcete toowatch.

## <a name="monitor-method-calls-and-external-dependencies"></a>Volání metody monitorování a externí závislosti
[Nainstalujte agenta Java hello](app-insights-java-agent.md) toolog zadané vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.

## <a name="performance-counters"></a>Čítače výkonu
V okně vaší přehled přejděte dolů a klikněte na tlačítko hello **servery** dlaždici. Uvidíte rozsah čítačů výkonu.

![Posuňte se dolů tooclick hello servery dlaždice](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Vlastní nastavení kolekce čítačů výkonu
kolekce toodisable hello standardní sady čítačů výkonu, přidejte následující kód pod hello kořenového uzlu souboru ApplicationInsights.xml hello hello:

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Shromažďování dalších čítačů výkonu
Můžete zadat další výkonu čítače toobe shromážděných.

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a>Čítače JMX (vystavené ve virtuálním počítači Java hello)

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName`– hello název zobrazený na portálu služby Application Insights hello.
* `objectName`– Název objektu JMX hello.
* `attribute`– atribut hello toofetch název objektu JMX hello
* `type`(volitelné) – hello typ atributu JMX objektu:
  * Výchozí hodnota: jednoduchý typ, například int nebo long.
  * `composite`: data čítače výkonu hello je ve formátu "Attribute.Data" hello
  * `tabular`: data čítače výkonu hello je ve formátu hello řádku tabulky

#### <a name="windows-performance-counters"></a>Čítače výkonu Windows
Každý [čítačů výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (v hello stejným způsobem, že je pole členem třídy). Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – název hello zobrazený na portálu služby Application Insights hello.
* categoryName – hello kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazeno tento čítač výkonu.
* counterName – název čítače výkonu hello hello.
* instanceName – název hello hello instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud hello kategorie obsahuje jednu instanci. Pokud je hello categoryName proces a hello chcete toocollect je hodnota čítače výkonu z aktuálního procesu JVM hello na, které běží vaše aplikace, zadejte `"__SELF__"`.

Čítače výkonu jsou zobrazené jako vlastní metriky v [Průzkumníku metrik][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Čítače výkonu Unix
* [Nainstalujte collectd s plug-in Application Insights hello](app-insights-java-collectd.md) tooget celou řadu dat systému a sítě.

## <a name="availability-web-tests"></a>Testy dostupnosti webu
Application Insights může otestovat váš web v pravidelných intervalech toocheck, zda je funkční a dobře reaguje. [tooset až][availability], projděte dolů tooclick dostupnosti.

![Posuňte se dolů, klikněte na tlačítko Dostupnost a pak Přidat test webu](./media/app-insights-java-eclipse/31-config-web-test.png)

Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.

![Příklad webového testu](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Další informace o testech dostupnosti webu.][availability]

## <a name="diagnostic-logs"></a>Diagnostické protokoly
Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou tooApplication statistiky, které vám umožní zkoumat a hledat v nich.

[Další informace o diagnostických protokolů][javalogs]

## <a name="custom-telemetry"></a>Vlastní telemetrii
Vložte po zadání několika řádků kódu do vaší toofind Java webové aplikace se co uživatelé dělají s ním nebo toohelp diagnostikovat problémy.

Kód můžete vložit na webové stránce JavaScript i v jazyce Java serverové hello.

[Další informace o vlastní telemetrii][track]

## <a name="next-steps"></a>Další kroky
#### <a name="detect-and-diagnose-issues"></a>Najít a diagnostikovat problémy
* [Přidat telemetrie webového klienta] [ usage] tooget výkonu telemetrie z hello webového klienta.
* [Nastavit testy webu] [ availability] toomake, že vaše aplikace zůstává aktivní a reagující.
* [Hledání událostí a protokolů] [ diagnostic] toohelp diagnostikovat problémy.
* [Zaznamenat trasování Log4J nebo Logback][javalogs]

#### <a name="track-usage"></a>Sledovat využití
* [Přidat telemetrie webového klienta] [ usage] toomonitor stránky zobrazení a metriky základní uživatel.
* [Sledujte vlastní události a metriky](app-insights-web-track-usage.md) toolearn o tom, jak se používá aplikace, jak v hello klient a hello server.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
