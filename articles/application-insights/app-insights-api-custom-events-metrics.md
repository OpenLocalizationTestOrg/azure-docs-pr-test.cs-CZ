---
title: "aaaApplication rozhraní API pro přehledy pro vlastní události a metriky | Microsoft Docs"
description: "Vložte po zadání několika řádků kódu do vašeho zařízení nebo aplikace na ploše, webové stránky nebo služby, tootrack využití a diagnostikovat problémy."
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
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights API pro vlastní události a metriky

Vložte po zadání několika řádků kódu do vaší aplikace toofind se co uživatelé dělají s ním nebo toohelp diagnostikovat problémy. Odesílat telemetrická data z aplikace zařízení a vzdálené ploše, webovými klienty a webové servery. Použití hello [Azure Application Insights](app-insights-overview.md) základní rozhraní API telemetrie toosend vlastní události a metriky a vlastní verzích standardní telemetrie. Toto rozhraní API je hello stejné rozhraní API tohoto standardního hello Application Insights sběrače dat použít.

## <a name="api-summary"></a>Souhrn rozhraní API
Hello rozhraní API je uniform pro všechny platformy kromě několik malé rozdíly.

| Metoda | Použít pro |
| --- | --- |
| [`TrackPageView`](#page-views) |Stránky, obrazovky, okna nebo formuláře. |
| [`TrackEvent`](#trackevent) |Akce uživatelů a dalších událostí. Použít tootrack uživatele chování nebo toomonitor výkonu. |
| [`TrackMetric`](#trackmetric) |Měření výkonu, jako je například délky fronty nesouvisejí toospecific události. |
| [`TrackException`](#trackexception) |Protokolování výjimky pro diagnostiku. Sledování, kde ve vztahu tooother událostí a zkontrolujte trasování zásobníku. |
| [`TrackRequest`](#trackrequest) |Protokolování hello četnost a dobu trvání požadavky serveru pro analýzu výkonu. |
| [`TrackTrace`](#tracktrace) |Zprávy protokolů diagnostiky. Také můžete zaznamenat protokoly třetích stran. |
| [`TrackDependency`](#trackdependency) |Doba trvání hello protokolování a četnost volání tooexternal komponent, které závisí aplikace na. |

Můžete [připojení vlastnosti a metriky](#properties) toomost těchto volání telemetrie.

## <a name="prep"></a>Než začnete
Pokud nemáte k dispozici odkaz na Application Insights SDK ještě:

* Přidejte hello Application Insights SDK tooyour projektu:

  * [Projekt ASP.NET](app-insights-asp-net.md)
  * [Projektu Java](app-insights-java-get-started.md)
  * [JavaScript v každé webové stránky](app-insights-javascript.md) 
* V zařízení nebo webový server kódu patří:

    *C#:*`using Microsoft.ApplicationInsights;`

    *Visual Basic:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`

## <a name="constructing-a-telemetryclient-instance"></a>Vytváření TelemetryClient instance
Vytvořit instanci `TelemetryClient` (s výjimkou v jazyce JavaScript webové stránky):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();

TelemetryClient je bezpečné pro přístup z více vláken.

Doporučujeme použít instanci TelemetryClient pro každý modul vaší aplikace. Například můžete mít jednu instanci TelemetryClient ve vaší žádosti webové služby tooreport příchozí HTTP a druhý v události middleware třídy tooreport obchodní logiku. Můžete například nastavit vlastnosti `TelemetryClient.Context.User.Id` tootrack uživatelů a relací, nebo `TelemetryClient.Context.Device.Id` tooidentify hello počítače. Tyto informace jsou připojené tooall události, které hello zasílá instance.

## <a name="trackevent"></a>TrackEvent
Ve službě Application Insights *vlastní události* je datový bod, který můžete zobrazit v [Průzkumníku metrik](app-insights-metrics-explorer.md) jako agregovaného počtu a v [diagnostické vyhledávání](app-insights-diagnostic-search.md) jako jednotlivé události. (Není související tooMVC nebo jiných framework "události.")

Vložit `TrackEvent` volání do vašeho kódu toocount různé události. Jak často uživatelé vybrat konkrétní funkce, jak často budou dosáhnout určité cíle nebo možná četnosti provádění konkrétní typy chyb.

Herní aplikace, například odeslání události vždy, když uživatel wins herní hello:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*Visual Basic*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a>Zobrazit události v portálu Microsoft Azure hello
toosee počet událostí, otevřete [Průzkumníku metrik](app-insights-metrics-explorer.md) okně přidejte nový graf a vyberte **události**.  

![Zobrazí počet vlastní události](./media/app-insights-api-custom-events-metrics/01-custom.png)

počty hello toocompare různých událostí, nastavte typ grafu hello příliš**mřížky**a skupinu podle názvu událostí:

![Nastavte typ grafu hello a seskupení](./media/app-insights-api-custom-events-metrics/07-grid.png)

Na hello mřížky klikněte na tlačítko prostřednictvím události název toosee jednotlivé výskyty této události. toosee více podrobností – klikněte na kterýkoli z výskytů v seznamu hello.

![Procházení událostí hello](./media/app-insights-api-custom-events-metrics/03-instances.png)

toofocus na konkrétní události ve vyhledávání nebo Průzkumníku metrik okno hello sada filtru toohello událostí názvy, které vás zajímají:

![Otevřete filtry, rozbalte název události a vyberte jednu nebo více hodnot](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Vlastní události v Analytics

telemetrie Hello je k dispozici v hello `customEvents` tabulky v [Application Insights Analytics](app-insights-analytics.md). Každý řádek představuje volání příliš`trackEvent(..)` ve vaší aplikaci. 

Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1. Pro příklad itemCount == 10 znamená, že z 10 volání tootrackEvent() hello vzorkování proces přenášena pouze jeden z nich. tooget správný počet vlastních událostí, měli byste použít proto použít kód jako `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Application Insights můžete grafu metriky, které nejsou připojené tooparticular události. Délka fronty může například sledovat v pravidelných intervalech. O metriky jednotlivými měřeními hello jsou méně důležité než hello variace a trendy a proto statistické grafy jsou užitečné.

V pořadí toosend metriky tooApplication statistiky, můžete použít hello `TrackMetric(..)` rozhraní API. Existují dva způsoby toosend metriky: 

* Jednu hodnotu. Pokaždé, když provedete měření ve vaší aplikaci, můžete odeslat hello odpovídající hodnotu tooApplication statistiky. Předpokládejme například, že máte metriky popisující hello počet položek v kontejneru. V konkrétním časovém období můžete poprvé tři položky do kontejneru hello a potom odeberte dvě položky. Podle toho by volání `TrackMetric` dvakrát: první předání hello hodnotu `3` a pak hello hodnotu `-2`. Application Insights ukládá obě hodnoty vaším jménem. 

* Agregace. Při práci s metriky, je každé jedno měření zájmu zřídka. Místo toho je důležité souhrn co se stalo v konkrétním časovém období. Takové souhrn nazývá _agregace_. V hello výše příklad hello agregační metriky součet za toto období je `1` a hello počet hodnot metriky hello je `2`. Při použití hello agregace přístup, pouze vyvolání `TrackMetric` jednou za časové období a odesílání hello agregované hodnoty. Toto je hello doporučenému přístupu vzhledem k tomu může významně snížit náklady na hello a výkonu režie odesláním méně dat. bodů tooApplication přehledy, a stále shromažďovat všechny relevantní informace.

### <a name="examples"></a>Příklady:

#### <a name="single-values"></a>Jednotlivé hodnoty

toosend jednu hodnotu metriky:

*JavaScript*

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

*C#, Java*

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a>Agregování metriky

Před odesláním z vaší aplikace, tooreduce šířky pásma, náklady a tooimprove výkonu doporučujeme tooaggregate metriky.
Tady je příklad totožný kódu:

*C#*

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
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
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
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
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

### <a name="custom-metrics-in-metrics-explorer"></a>Vlastní metriky v Průzkumníku metrik

výsledky hello toosee, otevřete Průzkumníka metrik a přidejte nový graf. Upravte graf tooshow hello vaší metriku.

> [!NOTE]
> Vlastní metriku může trvat několik minut tooappear hello seznamu dostupné metriky.
>

![Přidejte nový graf nebo vyberte graf a v části vlastní, vyberte vaše metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Vlastní metriky v Analytics

telemetrie Hello je k dispozici v hello `customMetrics` tabulky v [Application Insights Analytics](app-insights-analytics.md). Každý řádek představuje volání příliš`trackMetric(..)` ve vaší aplikaci.
* `valueSum`-Toto je součet hello hello měření. tooget hello střední hodnoty, dělení podle `valueCount`.
* `valueCount`-hello počet měření, které byly agregovat do této `trackMetric(..)` volání.

## <a name="page-views"></a>Zobrazení stránky
V aplikaci pomocí zařízení nebo webové stránky je odeslána telemetrická zobrazení stránky ve výchozím nastavení při načtení každé obrazovky nebo stránky. Ale na další nebo jinou dobu můžete změnit této tootrack zobrazení stránky. Například v aplikaci, která zobrazí karty nebo okna, můžete tootrack stránky vždy, když uživatel hello otevře nové okno.

![Použití přehledu v okně Přehled](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Data uživatele a relace je odeslána jako vlastnosti společně s zobrazení stránky, takže hello uživatele a relace grafy pocházet zachování připojení při telemetrická zobrazení stránky.

### <a name="custom-page-views"></a>Zobrazení vlastních stránek
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Pokud máte několik karet v rámci jiné stránky HTML, můžete zadat adresu URL hello příliš:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Zobrazení stránky časování
Ve výchozím nastavení, časy hello hlášené jako **zobrazení času načítání stránky** se měří z při hello prohlížeč odešle požadavek hello, dokud se nazývá hello prohlížeče událostí načtení stránky.

Místo toho můžete buď:

* Nastavit explicitní doba trvání v hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) volání: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Použít hello stránky zobrazení časování volání `startTrackPage` a `stopTrackPage`.

*JavaScript*

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

Tlačítka ...

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

Hello název, který použijete jako první parametr hello přidruží hello spuštění a zastavení volání. Výchozí hodnota toohello název aktuální stránky.

Výsledný načtení stránky Hello doby trvání zobrazí v Průzkumníku metrik, které jsou odvozeny od hello interval mezi hello spuštění a zastavení volání. Je to tooyou jaké interval, ve skutečnosti čas.

### <a name="page-telemetry-in-analytics"></a>Stránka telemetrie Analytics

V [Analytics](app-insights-analytics.md) dvou tabulek zobrazit data z prohlížeče operace:

* Hello `pageViews` tabulka obsahuje data o název adresy URL a stránku hello
* Hello `browserTimings` tabulka obsahuje data o výkonu klienta, jako je doba tooprocess hello hello příchozích dat

toofind jak dlouho hello prohlížeče trvá tooprocess různé stránky:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

toodiscover hello popularities různých prohlížečů:

```
pageViews | summarize count() by client_Browser
```

tooassociate stránky zobrazení tooAJAX volání, připojení k závislosti:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
požadavky HTTP toolog TrackRequest používá Hello server SDK.

Můžete také volat ho sami Pokud chcete, aby toosimulate požadavky v kontextu, kde nemáte hello webové služby modul spuštěn.

Telemetrie požadavku toosend způsob, jak je, kde hello požadavek funguje jako však doporučeno hello <a href="#operation-context">operační kontext</a>.

## <a name="operation-context"></a>Operace kontextu
Telemetrie položky můžete přidružit společně připojením toothem běžné ID operace. Standardní modulu Sledování žádostí o Hello k tomu pro výjimky a dalších událostí, které se odesílají během zpracování požadavku HTTP. V [vyhledávání](app-insights-diagnostic-search.md) a [Analytics](app-insights-analytics.md), můžete použít hello ID tooeasily najít všechny události přidružené hello žádosti.

Hello nejjednodušší způsob, jak tooset hello ID je tooset kontextu operace pomocí tohoto vzoru:

*C#*

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Společně s nastavení kontextu operace `StartOperation` vytvoří položku telemetrie hello typu, který určíte. Odesláním telemetrie hello položky při vyřazení hello operaci, nebo pokud explicitně volání `StopOperation`. Pokud používáte `RequestTelemetry` jako typ hello telemetrická data, jeho trvání nastavena toohello časový interval mezi zahájení a ukončení.

Kontexty operaci nelze vnořit. Pokud je již kontextu operace, pak všechny položky hello obsažené, včetně hello položka vytvořená s přidružen jeho ID `StartOperation`.

Do pole hledání hello operaci kontext je použité toocreate hello **související položky** seznamu:

![Související položky](./media/app-insights-api-custom-events-metrics/21.png)

Další informace o vlastní operace sledování naleznete v tématu [aplikací – přehledy – vlastní operace tracking.md].

### <a name="requests-in-analytics"></a>Požadavky v Analytics 

V [Application Insights Analytics](app-insights-analytics.md), požadavky zobrazit nahoru v hello `requests` tabulky.

Pokud [vzorkování](app-insights-sampling.md) je v operaci vlastnost itemCount hello zobrazí hodnotu větší než 1. Pro příklad itemCount == 10 znamená, že z 10 volání tootrackRequest() hello vzorkování proces přenášena pouze jeden z nich. tooget správný počet požadavků a průměrné trvání segmentované podle požadavku názvy, například použít kód:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Odešlete výjimky tooApplication statistiky:

* příliš[jejich počet](app-insights-metrics-explorer.md), jako údaje o četnosti hello problému.
* příliš[Zkontrolujte jednotlivé výskyty](app-insights-diagnostic-search.md).

Hello sestavy obsahují hello trasování zásobníku.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

sady SDK Hello catch množství výjimek automaticky, takže není vždy nutné toocall TrackException explicitně.

* ASP.NET: [napsat kód výjimky toocatch](app-insights-asp-net-exceptions.md).
* J2EE: [výjimky jsou zachyceny automaticky](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Výjimky jsou zachyceny automaticky. Pokud chcete toodisable automatické shromažďování, Přidání fragmentu kódu toohello řádek, který vložte do své webové stránky:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Výjimky v Analytics

V [Application Insights Analytics](app-insights-analytics.md), výjimky objeví v hello `exceptions` tabulky.

Pokud [vzorkování](app-insights-sampling.md) je v provozu, hello `itemCount` vlastnost zobrazuje hodnotu větší než 1. Pro příklad itemCount == 10 znamená, že z 10 volání tootrackException() hello vzorkování proces přenášena pouze jeden z nich. tooget správný počet výjimek oddělených typ výjimky, jako například použít kód:

```
exceptions | summarize sum(itemCount) by type
```

Většina hello důležité informace zásobníku je již extrahován do samostatné proměnné, ale můžete vyžádat od sebe hello `details` struktura tooget Další. Vzhledem k tomu, že tato struktura je dynamický, by měl přetypování hello výsledek typu toohello očekáváte. Například:

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

výjimky tooassociate s jejich související požadavky, použijte spojení:

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a>TrackTrace
Použití TrackTrace toohelp diagnostikovat problémy odesláním "záznam s popisem cesty" tooApplication statistiky. Můžete odesílat bloky diagnostických dat a je v zkontrolovat [diagnostické vyhledávání](app-insights-diagnostic-search.md).

[Přihlaste se adaptéry](app-insights-asp-net-trace-logs.md) používat tento portál toohello jiných výrobců protokoly toosend rozhraní API.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


Můžete hledat na obsah zprávy, ale (na rozdíl od hodnoty vlastností) nelze filtrovat na něm.

limit velikosti Hello na `message` je mnohem vyšší než limit hello na vlastnosti.
Výhodou TrackTrace je, že můžete umístit relativně long – datový uvítací zprávu. Můžete například kódovat následných dat existuje.  

Kromě toho můžete přidat zprávu tooyour úrovně závažnosti. A, podobně jako ostatní telemetrických dat, můžete přidat toohelp hodnoty vlastností, které filtrovat nebo vyhledávání pro různé skupiny trasování. Například:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

V [vyhledávání](app-insights-diagnostic-search.md), můžete pak snadno odfiltrovat všechny zprávy hello úrovně konkrétní závažnosti, které se týkají tooa konkrétní databáze.


### <a name="traces-in-analytics"></a>Trasování v Analytics

V [Application Insights Analytics](app-insights-analytics.md), volání metod tooTrackTrace zobrazit hello `traces` tabulky.

Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1. Pro příklad itemCount == 10 znamená, že 10 volání příliš`trackTrace()`, proces vzorkování hello přenášena pouze jeden z nich. tooget správný počet volání trasování, měli byste použít proto kódu, jako `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
Použití hello TrackDependency volat tootrack hello odezvy a úspěšnosti volání tooan externí část kódu. Hello výsledky se zobrazí v grafech závislostí hello hello portálu.

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

Mějte na paměti, tento server hello zahrnují sady SDK [závislostí modulu](app-insights-asp-net-dependencies.md) , zjišťuje a sleduje určitých volání závislosti automaticky – například toodatabases a rozhraní REST API. Máte tooinstall agenta na server toomake hello modul fungovat. Pokud chcete není catch tootrack volání, které hello automatizované sledování, nebo pokud nechcete, aby tooinstall hello agent použijete toto volání.

Upravit tooturn vypnout hello standardní modul sledování závislostí, [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) a odstraňte odkaz hello příliš`DependencyCollector.DependencyTrackingTelemetryModule`.

### <a name="dependencies-in-analytics"></a>Závislosti v Analytics

V [Application Insights Analytics](app-insights-analytics.md), trackDependency volání zobrazí v hello `dependencies` tabulky.

Pokud [vzorkování](app-insights-sampling.md) je v provozu, vlastnost itemCount hello zobrazuje hodnotu větší než 1. Pro příklad itemCount == 10 znamená, že z 10 volání tootrackDependency() hello vzorkování proces přenášena pouze jeden z nich. tooget správný počet závislostí segmentované podle cílové součásti, jako například použít kód:

```
dependencies | summarize sum(itemCount) by target
```

závislosti tooassociate s jejich související požadavky, použijte spojení:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Probíhá vyprazdňování dat
Za normálních okolností hello SDK odešle data v některých případech vybrali toominimize hello dopad na uživatele hello. Ale v některých případech můžete tooflush hello vyrovnávací paměti – například pokud používáte hello SDK v aplikaci, která ukončí.

*C#*

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

Upozorňujeme, že je funkce hello asynchronní pro hello [kanálu telemetrii serveru](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

## <a name="authenticated-users"></a>Ověření uživatelé
Ve webové aplikaci jsou uživatelé (ve výchozím nastavení) identifikovat pomocí souborů cookie. Uživatel může počítají více než jednou, pokud budou přistupovat k vaší aplikace z v jiném počítači či prohlížeči, nebo pokud se soubory cookie odstranit.

Pokud se uživatel přihlašuje tooyour aplikace, můžete získat přesnější počet nastavením hello ověřit ID uživatele v hello prohlížeče kódu:

*JavaScript*

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

V technologie ASP.NET MVC aplikace, například:

*Syntaxe Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Není nutné toouse hello skutečné přihlašovací jméno uživatele. Má pouze toobe ID, které je jedinečné toothat uživatele. Nesmí obsahovat mezery ani znaky hello `,;=|`.

ID uživatele Hello je také nastavit v souboru cookie relace a odeslat toohello serveru. Pokud je nainstalován hello server SDK, hello ověřeného uživatele, které je odeslána jako součást vlastností kontextu hello telemetrie klient i server. Potom můžete filtrovat a hledání v něm.

Pokud vaše aplikace skupin uživatelů do účtů, můžete předat také identifikátor hello účtu (s hello znak stejné omezení).

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

V [Průzkumníku metrik](app-insights-metrics-explorer.md), můžete vytvořit graf, který udává **ověřeného uživatele,**, a **uživatelské účty**.

Můžete také [vyhledávání](app-insights-diagnostic-search.md) pro datové body klienta s účty a názvy konkrétního uživatele.

## <a name="properties"></a>Filtrování, vyhledávání a segmentace svá data pomocí vlastností
Můžete připojit vlastnosti měření tooyour události (a také toometrics, zobrazení stránek, výjimky a další data telemetrie).

*Vlastnosti* jsou řetězcové hodnoty, které můžete toofilter telemetrie v sestavách využití hello. Například pokud vaše aplikace obsahuje několik hry, můžete připojit hello název hello herní tooeach události tak, aby her, které jsou populárnější, si můžete zobrazit.

Existuje omezení 8192 na hello délka řetězce. (Pokud chcete toosend velké bloky dat, použijte parametr zprávy hello [TrackTrace](#track-trace).)

*Metriky* jsou číselné hodnoty, které lze zobrazit graficky. Můžete například toosee Pokud dojde k postupné zvýšení skóre hello, které vaše hráči dosáhnout. grafy Hello mohou být segmentovány podle hello vlastnosti, které se odesílají s hello událostí tak, aby vám oddělení nebo skládaný grafy pro různé hry.

Pro hodnoty metriky toobe správně zobrazen měly by být větší než nebo rovna too0.

Některé [omezení počtu hello vlastnosti, hodnoty vlastností a metriky](#limits) , kterou můžete použít.

*JavaScript*

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


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


*Visual Basic*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Vezměte v potaz není toolog identifikovatelné osobní údaje ve vlastnostech.
>
>

*Pokud jste použili metriky*, otevřete Průzkumníka metrik a vyberte metriku hello hello **vlastní** skupiny:

![Otevřete Průzkumníka metrik, vyberte hello grafu a vyberte metriku hello](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Pokud vaše metrika se nezobrazí, nebo pokud hello **vlastní** záhlaví není okno Výběr zavřít hello existuje a opakujte akci později. Metriky někdy může trvat hodinu toobe agregovat kanálem hello.

*Pokud jste použili vlastnosti a metriky*, segmentovat hello metrika vlastností hello:

![Nastavit seskupování a potom vyberte vlastnost hello pod Seskupit podle](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*Ve vyhledávání diagnostiky*, můžete zobrazit vlastnosti hello a metriky jednotlivé výskyty události.

![Vyberte instance a pak vyberte "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Použití hello **vyhledávání** pole toosee výskytů události, které mají konkrétní hodnotu vlastnosti.

![Zadejte termín do vyhledávání](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[Další informace o vyhledávacích výrazech](app-insights-diagnostic-search.md).

### <a name="alternative-way-tooset-properties-and-metrics"></a>Alternativní způsob tooset vlastnosti a metriky
Pokud je pohodlnější, můžete shromáždit hello parametry události v samostatném objektu:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Nemáte hello znovu použít stejnou instanci položky telemetrie (`event` v tomto příkladu) toocall Track*() vícekrát. To může způsobit telemetrie toobe odeslané s nesprávná konfigurace.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Vlastní měření a vlastnosti v Analytics

V [Analytics](app-insights-analytics.md), vlastní metriky a vlastnosti zobrazit v hello `customMeasurements` a `customDimensions` atributy každý záznam telemetrie.

Například pokud jste přidali vlastnost s názvem "hra" tooyour požadavku telemetrie, tento dotaz počty hello výskyty různé hodnoty "herní" a zobrazit hello průměr hello vlastní metriky "skóre":

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

Všimněte si, že:

* Při extrahování hodnotu z hello customDimensions nebo customMeasurements JSON, má dynamické typ, a proto musíte vysílat `tostring` nebo `todouble`.
* účet tootake Ahoj možnost vzniku [vzorkování](app-insights-sampling.md), měli byste použít `sum(itemCount)`, nikoli `count()`.



## <a name="timed"></a>Časování událostí
Někdy budete chtít toochart, jak dlouho trvá tooperform akce. Například můžete chtít tooknow jak dlouho uživatelé provádět tooconsider volby ve hře. To můžete pomocí parametru měření hello.

*C#*

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <a name="defaults"></a>Výchozí vlastnosti pro vlastní telemetrii
Pokud chcete tooset výchozí hodnoty vlastností pro některé hello vlastní události, které lze zadat, můžete je nastavit v instanci TelemetryClient. Jsou připojené tooevery telemetrie položek, který se odesílá z tohoto klienta.

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

*Visual Basic*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



Jednotlivé telemetrii volání můžete přepsat výchozí hodnoty hello ve slovnících jejich vlastnosti.

*Pro jazyk JavaScript webových klientů*, [pomocí jazyka JavaScript telemetrie inicializátory](#js-initializer).

*tooadd vlastnosti tooall telemetrie*, včetně dat hello z modulů standardního shromažďování [implementovat `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Vzorkování, filtrování a zpracování telemetrie
Můžete napsat kód tooprocess telemetrie před odesláním z hello SDK. zpracování Hello obsahuje data, která se odesílá z modulů standardní telemetrie hello, jako je shromažďování požadavků HTTP a kolekce závislost.

[Přidání vlastnosti](app-insights-api-filtering-sampling.md#add-properties) tootelemetry implementací `ITelemetryInitializer`. Můžete například přidat čísla verzí nebo vypočtené hodnoty od dalších vlastností.

[Filtrování](app-insights-api-filtering-sampling.md#filtering) můžete změnit nebo zrušit telemetrie před odesláním z hello SDK implementací `ITelemetryProcesor`. Můžete řídit, co se odesílá nebo se zahodí, ale máte tooaccount pro hello vliv na vaše metriky. V závislosti na tom, jak zrušit položek může dojít ke ztrátě hello možnost toonavigate mezi související položky.

[Vzorkování](app-insights-api-filtering-sampling.md) je svazek zabalené řešení tooreduce hello dat, který se odesílá z portálu toohello aplikace. Dělá to tak, aniž by to ovlivnilo hello zobrazí metriky. A dělá to tak, aniž by to ovlivnilo problémů toodiagnose možnost přechodem mezi související položky jako výjimky, požadavky a zobrazení stránek.

[Další informace](app-insights-api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Vypnutí telemetrie
příliš*dynamicky zastavit a spustit* hello shromažďování a předávání telemetrie:

*C#*

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

příliš*zakázat vybrané standardní Kolektory*– například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář hello relevantní řádků v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). To provedete, například pokud chcete toosend TrackRequest data.

## <a name="debug"></a>Režim vývojáře
Během ladění, je užitečné toohave telemetrie rychlé kanálem hello tak, aby se zobrazí výsledky okamžitě. Můžete také zprávy Další get, které vám pomůžou trasování všechny problémy s telemetrií hello. Vypněte ho v produkčním prostředí, protože mohou být zpomaleny vaší aplikace.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a>Nastavení hello klíč instrumentace pro vybranou vlastní telemetrii
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a>Klíč dynamické instrumentace
tooavoid kombinování až telemetrie z vývoj, testování a provozním prostředí, můžete [vytvořit samostatné prostředky Application Insights](app-insights-create-new-resource.md) a změnit jejich klíče, v závislosti na prostředí hello.

Místo získání klíč instrumentace hello z hello konfiguračního souboru, můžete ho nastavit v kódu. V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavit hello:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



Na webových stránkách, můžete chtít tooset ze stavu hello webového serveru, než kódování oznámena do skriptu hello. Například ve webové stránky vygenerované v aplikaci ASP.NET:

*JavaScript ve Razor*

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient má vlastnost kontext, který obsahuje hodnoty, které jsou odeslány společně s všechny telemetrická data. Za normálních okolností jsou nastavené moduly hello standardní telemetrie, ale můžete také nastavit jejich sami. Například:

    telemetry.Context.Operation.Name = "MyOperationName";

Pokud jste nastavili některou z těchto hodnot sami, zvažte odebrání hello příslušný řádek z [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)tak, aby hodnoty a standardní hodnoty hello získat nerozumíte nemáte.

* **Součást**: hello aplikace a její verzi.
* **Zařízení**: Data o hello zařízení, kde je spuštěna aplikace hello. (Ve službě web apps, to je hello serveru nebo klientského zařízení, které je odeslaný hello telemetrie.)
* **InstrumentationKey**: hello prostředek Application Insights v Azure, kde se zobrazí hello telemetrie. Ho je obvykle zachyceny ze souboru ApplicationInsights.config.
* **Umístění**: hello geografickém umístění zařízení hello.
* **Operace**: ve službě web apps hello aktuální žádost HTTP. V jiných typů aplikací můžete nastavit tato toogroup události společně.
  * **ID**: generované hodnoty, které koreluje různých událostí, takže když si prohlédnout všechny události ve vyhledávání diagnostiky, můžete najít související položky.
  * **Název**: identifikátor, obvykle hello URL hello HTTP žádosti.
  * **SyntheticSource**: Pokud není null nebo prázdný, řetězec, který určuje, které zdroj hello hello žádosti byl identifikován jako robot nebo webový test. Ve výchozím nastavení je vyloučen z výpočtů v Průzkumníku metrik.
* **Vlastnosti**: vlastnosti, které se odesílají s všechny telemetrická data. Může být přepsána v jednotlivých sledovat * volání.
* **Relace**: hello uživatelské relace. Hello ID je nastaveno tooa generuje hodnotu, která se změní, když uživatel hello nebyl active nějakou dobu.
* **Uživatel**: informace o uživateli.

## <a name="limits"></a>Omezení
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

tooavoid stiskne limit rychlosti hello data, použijte [vzorkování](app-insights-sampling.md).

toodetermine jak dlouho data se ukládají, najdete v části [uchovávání dat a ochrana osobních údajů](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Referenční dokumenty
* [Rozhraní ASP.NET – reference](https://msdn.microsoft.com/library/dn817570.aspx)
* [Referenční informace sady Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [Referenční dokumentace technologie JavaScript](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Sada SDK pro Android](https://github.com/Microsoft/ApplicationInsights-Android)
* [Sada SDK pro iOS](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>Kód SDK
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [Balíčky systému Windows Server](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Všechny platformy](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Otázky
* *Jaké výjimky může vyvolat volání Track_()?*

    Žádné. Nepotřebujete toowrap je v klauzulích try-catch. Pokud hello SDK zaznamená problémy, bude protokolování zpráv ve výstupu konzoly hello ladění a – pokud hello zprávy získat prostřednictvím – ve vyhledávání diagnostiky.
* *Je k dispozici rozhraní REST API tooget dat z portálu hello?*

    Ano, hello [rozhraní API pro přístup k datům](https://dev.applicationinsights.io/). Zahrnout další způsoby tooextract data [exportovat z Analytics tooPower BI](app-insights-export-power-bi.md) a [průběžné export](app-insights-export-telemetry.md).

## <a name="next"></a>Další kroky
* [Hledání událostí a protokolů](app-insights-diagnostic-search.md)

* [Řešení potíží](app-insights-troubleshoot-faq.md)


