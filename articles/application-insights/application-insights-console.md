---
title: "Azure Application Insights pro konzolové aplikace | Microsoft Docs"
description: "Sledování webových aplikací pro dostupnosti, výkonu a využití."
services: application-insights
documentationcenter: .net
author: lmolkova
manager: bfung
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 57f5670eec36ff2c4332da592dd2a3eef73fdefc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights pro rozhraní .NET Konzolová aplikace
[Application Insights](app-insights-overview.md) umožňuje monitorování vaší webové aplikace pro dostupnosti, výkonu a využití.

Potřebujete předplatné s [Microsoft Azure](http://azure.com). Přihlaste se pomocí účtu Microsoft, který může mít pro Windows, Xbox Live nebo jiných cloudových služeb Microsoftu. Tým může mít předplatné pro společnosti do Azure: Požádejte vlastníka, aby je přidejte do ní pomocí svého účtu Microsoft.

## <a name="getting-started"></a>Začínáme

* Na webu [Azure Portal](https://portal.azure.com) [vytvořte prostředek Application Insights](app-insights-create-new-resource.md). Jako typ aplikace vyberte aplikaci ASP.NET.
* Zkopírujte klíč instrumentace. V rozevíracím Essentials nového prostředku, kterou jste vytvořili najít klíč. 
* Nainstalujte nejnovější [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) balíčku.
* Nastavte klíč instrumentace před sledování všech telemetrie v kódu (nebo nastavená proměnná prostředí APPINSIGHTS_INSTRUMENTATIONKEY). Potom byste měli ručně sledovat telemetrie a zobrazit ji na portálu Azure

```csharp
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* Nainstalujte nejnovější verzi [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) balíček - automaticky sleduje HTTP, SQL nebo jiná volání vnější závislosti.

Může inicializace a konfigurace Application Insights z kódu nebo použití `ApplicationInsights.config` souboru. Zajistěte, aby co nejdříve se stane inicializace.

### <a name="using-config-file"></a>Použití konfiguračního souboru

Ve výchozím nastavení, Application Insights SDK hledá `ApplicationInsights.config` souboru v pracovním adresáři při `TelemetryConfiguration` se vytváří

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Read ApplicationInsights.config file if present
```

Můžete také určit cestu k souboru config.

```csharp
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration("ApplicationInsights.config");
```

Další informace najdete v tématu [referenci na konfigurační soubor](app-insights-configuration-with-applicationinsights-config.md).

Úplný příklad konfiguračního souboru může dojít po instalaci nejnovější verze [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) balíčku. Tady je **minimální** konfigurace pro kolekce závislost, která je ekvivalentní příkladu kódu.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Konfigurace sběru telemetrie z kódu

* Při spuštění aplikace vytvořit a nakonfigurovat `DependencyTrackingTelemetryModule` instance - musí být singleton a musí být zachováno pro životního cyklu aplikace.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Přidat běžné inicializátory telemetrie

```csharp
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* Pro aplikace pro Windows rozhraní .NET Framework, můžete také nainstalovat a inicializaci modulu kolekce čítače výkonu, jak je popsáno [sem](http://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Úplný příklad

```csharp
static void Main(string[] args)
{
    TelemetryConfiguration configuration = TelemetryConfiguration.Active;

    configuration.InstrumentationKey = "removed";
    configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
    configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

    var telemetryClient = new TelemetryClient();
    using (IntitializeDependencyTracking(configuration))
    {
        telemetryClient.TrackTrace("Hello World!");

        using (var httpClient = new HttpClient())
        {
            // Http dependency is automatically tracked!
            httpClient.GetAsync("https://microsoft.com").Wait();
        }
    }

    // run app...

    // when application stops or you are done with dependency tracking, do not forget to dispose the module
    dependencyTrackingModule.Dispose();

    telemetryClient.Flush();
}

static DependencyTrackingTelemetryModule IntitializeDependencyTracking(TelemetryConfiguration configuration)
{
    // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");    
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

    // enable known dependency tracking, note that in future versions, we will extend this list. 
    // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

    // initialize the module
    module.Initialize(configuration);

    return module;
}
```

## <a name="next-steps"></a>Další postup
* [Monitorování závislostí](app-insights-asp-net-dependencies.md) chcete zobrazit, pokud REST, SQL nebo jiné externí prostředky zpomalují můžete.
* [Použít rozhraní API](app-insights-api-custom-events-metrics.md) k odeslání vlastní události a metriky pro další podrobné zobrazení výkonu a využití vaší aplikace.
