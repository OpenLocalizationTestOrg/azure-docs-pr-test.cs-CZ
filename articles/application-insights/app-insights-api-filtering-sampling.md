---
title: "aaaFiltering a předzpracování v hello Azure Application Insights SDK | Microsoft Docs"
description: "Zápis inicializátory Telemetrie a Telemetrie procesorů pro hello SDK toofilter nebo přidání vlastnosti toohello dat před odesláním telemetrie hello toohello portál Application Insights."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a>Filtrování a předběžného zpracování telemetrie hello Application Insights SDK


Můžete napsat a konfigurovat moduly plug-in pro hello Application Insights SDK toocustomize jak telemetrie bude zachycen a před odesláním toohello služby Application Insights.

* [Vzorkování](app-insights-sampling.md) snižuje objem hello telemetrie bez ovlivnění statistice. Udržuje společně související datových bodů tak, aby můžete procházet mezi nimi při diagnostikování problému. Hello portálu celkový počet hello je vynásobená toocompensate pro hello vzorkování.
* Filtrování s procesory Telemetrie [pro technologii ASP.NET](#filtering) nebo [Java](app-insights-java-filter-telemetry.md) umožňuje vybrat nebo upravit telemetrie hello SDK před odesláním toohello serveru. Například může snížit hello svazku telemetrie tak, že vyloučíte z robotů požadavky. Ale filtrování je více základní provoz tooreducing přístup než vzorkování. Umožňuje větší kontrolu nad co se přenášejí, ale máte toobe vědět, že ovlivňuje statistice – například pokud můžete filtrovat všechny úspěšné požadavky.
* [Inicializátory telemetrie přidávat vlastnosti](#add-properties) tooany telemetrická data odesílaná z vaší aplikace, včetně telemetrie z hello standardní moduly. Například můžete přidat vypočítané hodnoty; nebo čísla verzí pomocí datových hello toofilter hello portálu.
* [Hello rozhraní API sady SDK](app-insights-api-custom-events-metrics.md) je použité toosend vlastní události a metriky.

Než začnete, potřebujete:

* Nainstalujte službu hello Application Insights [SDK pro technologii ASP.NET](app-insights-asp-net.md) nebo [SDK pro jazyk Java](app-insights-java-get-started.md) ve vaší aplikaci.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filtrování: ITelemetryProcessor
Tento postup vám dává větší kontrolu nad co je zahrnout nebo vyloučit z datový proud telemetrie hello. Můžete jej použít ve spojení s vzorkování, nebo samostatně.

telemetrie toofilter zápisu procesor telemetrie a zaregistrovat ji pomocí hello SDK. Všechny telemetrická prochází procesor, a můžete toodrop z hello Streamovat nebo přidání vlastností. To zahrnuje telemetrie z hello standardní moduly, například kolekce požadavku HTTP hello a hello závislosti kolekcí a také telemetrických dat, který jste napsali sami. Můžete například odfiltrovat telemetrická data týkající se požadavků z robotů nebo volání úspěšné závislostí.

> [!WARNING]
> Filtrování hello telemetrická data odesílaná z hello SDK pomocí procesorů zkreslit hello statistiky hello portálu a nastavit jej jako obtížné toofollow související položky.
>
> Místo toho zvažte použití [vzorkování](app-insights-sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Vytvoření telemetrie procesoru (C#)
1. Ověřte, že hello Application Insights SDK do projektu je verze 2.0.0 nebo novější. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení Visual Studio a zvolte spravovat balíčky NuGet. V Správce balíčků NuGet zkontrolujte Microsoft.ApplicationInsights.Web.
2. toocreate filtr, implementovat ITelemetryProcessor. Toto je jiný bod rozšiřitelnost jako modul telemetrie, inicializátoru telemetrie a telemetrie kanál.

    Všimněte si, že Telemetrie procesory vytvořit řetěz zpracování. Můžete vytvořit instanci telemetrie procesor, předáte další procesor toohello odkaz v řetězu hello. Když bod telemetrická data předána metoda proces toohello, dělá svou práci a pak volání hello další procesor Telemetrie v řetězu hello.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. Vložte tuto položku v souboru ApplicationInsights.config:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Toto je hello stejné části, kde inicializovat vzorkování filtr.)

Můžete předat hodnoty řetězce ze souboru .config hello tím, že poskytuje veřejné s názvem vlastnosti v třídě.

> [!WARNING]
> Vezměte v potaz, název typu hello toomatch a všechny názvy vlastností v třídě toohello soubor .config hello a názvy vlastností v kódu hello. Pokud soubor .config hello odkazuje na neexistující typ nebo vlastnost, hello SDK bezobslužně podařit toosend žádné telemetrie.
>
>

**Alternativně** můžete inicializovat hello filtru v kódu. Ve třídě vhodný inicializace – například AppStart v Global.asax.cs - vložte do řetězu hello procesor:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients vytvořené po tomto bodu použije vaše procesory.

### <a name="example-filters"></a>Příklad filtrů
#### <a name="synthetic-requests"></a>Syntetické požadavků
Filtrovat robotů a webové testy. I když Průzkumníku metrik poskytuje hello možnost toofilter out syntetické zdroje, tato možnost je filtrování na hello SDK snižuje zatížení.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Ověřování se nezdařilo
Filtrování požadavků s odpovědi "401".

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Odfiltrovat rychlé vzdálené závislostí volání
Pokud chcete pouze toodiagnose volání, které jsou pomalé, filtrujte ty fast hello.

> [!NOTE]
> To bude zkreslit hello statistiky, které vidíte na portálu hello. Graf závislostí Hello bude vypadat, jako by volání závislostí hello všechny chyby.
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Diagnóza problémů se závislostí
[Tomto blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) popisuje problémy závislost projektu toodiagnose automaticky odesláním toodependencies regulární příkazy ping.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Přidat vlastnosti: ITelemetryInitializer
Pomocí telemetrie inicializátory toodefine globální vlastnosti, které jsou odesílány všechny telemetrická; a toooverride vybraná chování hello standardní telemetrie modulů.

Například hello Application Insights pro webové balíček shromažďuje telemetrická data o požadavcích HTTP. Ve výchozím nastavení, označuje jako neúspěšný každá žádost s kódem odpovědi > = 400. Ale pokud chcete, aby tootreat 400 jako úspěšné, můžete zadat inicializátoru telemetrická data, která nastaví vlastnost úspěch hello.

Pokud zadáte inicializátoru telemetrie, nazývá se vždy, když žádné z hello Track*() metody je volána. To zahrnuje metody, které jsou volány hello standardní telemetrie moduly. Podle konvence nenastavujte tyto moduly jakákoli vlastnost, která již byla nastavena pomocí inicializátoru.

**Zadejte vaše inicializátoru**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

**Načíst vaše inicializátoru**

V souboru ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Alternativně* můžete vytvořit instanci hello inicializátoru v kódu, například v souboru Global.aspx.cs:

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Další informace naleznete v této ukázky.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a>Inicializátory telemetrie JavaScript
*JavaScript*

Vložte inicializátoru telemetrie ihned po hello inicializace kód, který jste získali z portálu hello:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Souhrn hello bez vlastní vlastnosti v hello telemetryItem k dispozici, najdete v části [Application Insights Exportovat datový Model](app-insights-export-data-model.md).

Můžete přidat libovolný počet inicializátory, jak se vám líbí.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor a ITelemetryInitializer
Co je hello rozdíl mezi procesory telemetrie a inicializátory telemetrie?

* Existují některé překrytí v co můžete dělat s nimi: lze použít tooadd vlastnosti tootelemetry i.
* Vždy spustit před TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors umožňují toocompletely nahradit nebo zrušení položku telemetrie.
* TelemetryProcessors nemáte zpracovat telemetrická data čítače výkonu.


## <a name="reference-docs"></a>Referenční dokumenty
* [Přehled rozhraní API](app-insights-api-custom-events-metrics.md)
* [Rozhraní ASP.NET – reference](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>Kód SDK
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Další kroky
* [Hledání událostí a protokolů](app-insights-diagnostic-search.md)
* [Vzorkování](app-insights-sampling.md)
* [Řešení potíží](app-insights-troubleshoot-faq.md)
