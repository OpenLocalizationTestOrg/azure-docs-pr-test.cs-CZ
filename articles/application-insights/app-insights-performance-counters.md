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
# <a name="system-performance-counters-in-application-insights"></a>Čítače výkonu systému ve službě Application Insights
Systém Windows nabízí širokou škálu [čítače výkonu](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) například obsazení procesoru, paměti, disku a využití sítě. Můžete také definovat vlastní. [Application Insights](app-insights-overview.md) můžete zobrazit tyto čítače výkonu, pokud vaše aplikace běží v rámci služby IIS na toowhich místního hostitele nebo virtuálního počítače budete mít přístup pro správu. grafy Hello znamenat hello prostředky k dispozici tooyour živé aplikace a může pomoct tooidentify nevyváženou zatížení mezi instancemi serveru.

Čítače výkonu se zobrazí v okně hello servery, které obsahuje tabulku této segmenty instance serveru.

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Čítače výkonu nejsou k dispozici pro webové aplikace Azure. Ale můžete [odesílání Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Zobrazení čítačů
okno servery Hello ukazuje výchozí sadu čítačů výkonu. 

toosee jiných čítačů buď upravit hello grafy v okně servery hello nebo otevřete nové [Průzkumníku metrik](app-insights-metrics-explorer.md) okno a přidejte nové grafy. 

k dispozici čítače Hello jsou označeny jako metriky, když se upravit graf.

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Vytvořte všechny nejužitečnější grafy na jednom místě, toosee [řídicí panel](app-insights-dashboards.md) a připnete ji tooit.

## <a name="add-counters"></a>Přidání čítačů
Pokud není hello čítače výkonu, které chcete zobrazit v seznamu hello metrik, protože hello Application Insights SDK není shromažďování ve vašem webovém serveru. Můžete ho nakonfigurovat toodo tak.

1. Zjistěte, jaké čítače jsou k dispozici na vašem serveru pomocí tohoto příkazu Powershellu na serveru hello:
   
    `Get-Counter -ListSet *`
   
    (Viz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Otevřete soubor ApplicationInsights.config.
   
   * Pokud jste přidali aplikaci tooyour Application Insights během vývoje, upravit soubor ApplicationInsights.config ve vašem projektu a poté ji znovu nasadit tooyour servery.
   * Pokud jste použili tooinstrument monitorování stavu webové aplikace za běhu, najít soubor ApplicationInsights.config v kořenovém adresáři hello hello aplikace ve službě IIS. Aktualizaci existuje v každé instanci serveru.
3. Úpravy – direktiva kolekce výkonu hello:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Můžete zaznamenat standardní čítače i těch, které že jste implementovali sami. `\Objects\Processes`Příkladem standardní čítač je k dispozici na všechny systémy Windows. `\Sales(photo)\# Items Sold`je příklad vlastní čítače, který může být implementována ve webové službě. 

Formát Hello je `\Category(instance)\Counter"`, nebo kategorie, které nemají instancí, právě `\Category\Counter`.

`ReportAs`je vyžadována pro názvy čítačů, které se neshodují `[a-zA-Z()/-_ \.]+` – to znamená, obsahují znaky, které nejsou v hello následující sady: písmena, kulaté závorky, lomítkem, pomlčky, podtržítka, místo, tečka.

Pokud zadáte instance, budou shromážděna metrika udávaný dimenzi "CounterInstanceName" Dobrý den.

### <a name="collecting-performance-counters-in-code"></a>Shromažďování čítače výkonu v kódu
výkon systému toocollect čítače a jejich odeslání tooApplication statistiky, můžete přizpůsobit hello fragment kódu níže:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Nebo to můžete provést hello samé s vlastní metriky, které jste vytvořili:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Čítače výkonu v Analytics
Můžete vyhledat a zobrazit sestavy čítače výkonu v [Analytics](app-insights-analytics.md).

Hello **čítače výkonu** schématu zpřístupní hello `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.  V hello telemetrických dat pro každou aplikaci zobrazí se pouze hello čítače pro tuto aplikaci. Například toosee čítače, které jsou k dispozici: 

![Čítače výkonu v analytics Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

('Instance, zde označují toohello instance čítače výkonu, není hello role nebo serveru instance počítače. Název instance čítače výkonu Hello obvykle segmenty čítače například využití procesoru podle názvu hello hello procesu nebo aplikace.)

tooget graf dostupné paměti přes hello poslední období: 

![Paměť timechart v analytics Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující hello identitu hello hostitele serveru instance, na kterém aplikace běží. Například toocompare hello výkonu vaší aplikace na různé počítače hello: 

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>Technologie ASP.NET a počty Application Insights
*Co je hello rozdíl mezi rychlost hello výjimky a výjimky metriky?*

* *Míra výjimka* je čítače výkonu systému. Hello CLR spočítá všechny hello zpracovat a neošetřených výjimek, které jsou vyvolány a vydělí hello celkem v intervalu vzorkování prahovou hodnotou hello délka intervalu hello. Hello Application Insights SDK shromažďuje tento výsledek a odešle ji toohello portálu.
* *Výjimky* je počet hello TrackException sestavy přijatých hello portálu v intervalu vzorkování hello hello grafu. Obsahuje pouze hello zpracovává výjimky, kde jste napsali TrackException zavolá do vašeho kódu a neobsahuje všechny [neošetřené výjimky](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Výstrahy
Jako další metriky můžete [nastavit upozornění](app-insights-alerts.md) toowarn, pokud čítače výkonu ocitne mimo omezení zadáte. Otevřete okno hello výstrahy a klikněte na tlačítko Přidat výstrahy.

## <a name="next"></a>Další kroky
* [Sledování závislostí](app-insights-asp-net-dependencies.md)
* [Sledování výjimek](app-insights-asp-net-exceptions.md)

