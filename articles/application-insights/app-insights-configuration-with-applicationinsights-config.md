---
title: "referenční dokumentace aaaApplicationInsights.config - Azure | Microsoft Docs"
description: "Povolte nebo zakažte moduly shromažďování dat a přidejte čítače výkonu a dalších parametrů."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: alancameronwills
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 76cb11349d87dfc508ec8b1c454259a0b079c48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurace hello Application Insights SDK souboru ApplicationInsights.config či .xml
Hello Application Insights .NET SDK se skládá z řady balíčky NuGet. [Základní balíček](http://www.nuget.org/packages/Microsoft.ApplicationInsights) poskytuje hello rozhraní API pro odesílání telemetrie do hello Application Insights. [Další balíčky](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) poskytují telemetrie *moduly* a *inicializátory* pro automaticky sledování telemetrie z vaší aplikace a jeho kontextu. Úpravou konfiguračního souboru hello můžete povolit nebo zakázat telemetrii moduly a inicializátory a nastavit parametry pro některé z nich.

konfigurační soubor Hello jmenuje `ApplicationInsights.config` nebo `ApplicationInsights.xml`, v závislosti na typu hello vaší aplikace. Je automaticky přidán tooyour projektu, když jste [instalaci většiny verzích hello SDK][start]. Je také přidán tooa webové aplikace pomocí [monitorování stavu na serveru se službou IIS][redfield], nebo když vyberete hello Appplication Insights [rozšíření pro webové stránky Azure, nebo virtuální počítač](app-insights-azure-web-apps.md).

Není k dispozici ekvivalentní soubor toocontrol hello [SDK na webové stránce][client].

Tento dokument popisuje hello oddíly, které vidíte v konfiguraci hello souboru, jak budou řídit hello součástí hello SDK, a které balíčky NuGet načtení těchto součástí.

## <a name="telemetry-modules-aspnet"></a>Telemetrie moduly (ASP.NET)
Každý modul telemetrie shromažďuje konkrétní typ dat a používá hello jádra rozhraní API toosend hello data. Hello moduly jsou nainstalovány jiné balíčky NuGet, které také přidat soubor .config toohello hello požadovaných řádků.

V hello konfiguračního souboru pro každý modul je uzel. toodisable modul, odstraňte hello uzlu nebo ho nastavte jako komentář.

### <a name="dependency-tracking"></a>Sledování závislostí
[Závislost sledování](app-insights-asp-net-dependencies.md) shromažďuje telemetrická data o volání aplikace umožňuje toodatabases a externích služeb a databází. tooallow tento modul toowork v serveru se službou IIS, je třeba příliš[nainstalujte monitorování stavu][redfield]. toouse ji ve službě Azure web apps nebo virtuálních počítačů, [vyberte rozšíření Application Insights hello](app-insights-azure-web-apps.md).

Můžete taky napsat vlastní závislost sledování kódu pomocí hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) balíček NuGet.

### <a name="performance-collector"></a>Kolekce výkonu
[Shromažďuje čítače výkonu systému](app-insights-performance-counters.md) například CPU, paměť a síť načíst z instalace služby IIS. Můžete určit, které toocollect čítače, včetně čítačů výkonu, které jste nastavili sami.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) balíček NuGet.

### <a name="application-insights-diagnostics-telemetry"></a>Telemetrii Diagnostics Application Insights
Hello `DiagnosticsTelemetryModule` sestavy chyb v hello samotný kód instrumentace Application Insights. Například pokud hello kód nemůže přistupovat k čítače výkonu nebo `ITelemetryInitializer` vyvolá výjimku. Trasování telemetrie sleduje tento modul se zobrazí v hello [diagnostické vyhledávání][diagnostic]. Odešle toodc.services.vsallin.net diagnostická data.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet. Pokud máte pouze instalaci tohoto balíčku, není soubor ApplicationInsights.config hello vytvoří automaticky.

### <a name="developer-mode"></a>Režim vývojáře
`DeveloperModeWithDebuggerAttachedTelemetryModule`Vynutí hello Application Insights `TelemetryChannel` toosend data okamžitě, jeden telemetrie položky v době, kdy je ladicí program připojených toohello proces aplikace. To snižuje hello množství času mezi hello chvíli, když vaše aplikace sleduje telemetrie a když se zobrazí na portálu služby Application Insights hello. Způsobuje významné režijní náklady v procesoru a šířku pásma sítě.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet

### <a name="web-request-tracking"></a>Sledování webové žádosti
Sestavy hello [čas a výsledek kód odpovědi](app-insights-asp-net.md) požadavků HTTP.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet

### <a name="exception-tracking"></a>Sledování výjimek
`ExceptionTrackingTelemetryModule`sleduje neošetřených výjimek ve vaší webové aplikaci. V tématu [chyby a výjimky][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) balíček NuGet
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-sleduje [nepozorovaná úloh výjimky](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-sleduje neošetřené výjimky pro role pracovního procesu, služby systému windows a konzolové aplikace.
* [Application Insights systému Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) balíček NuGet.

### <a name="eventsource-tracking"></a>EventSource sledování
`EventSourceTelemetryModule`Umožňuje vám tooconfigure toobe událostí EventSource odeslána tooApplication Insights jako trasování. Informace o sledování událostí EventSource najdete v tématu [pomocí událostí EventSource](app-insights-asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>Sledování událostí trasování událostí pro Windows
`EtwCollectorTelemetryModule`Umožňuje tooconfigure události z toobe zprostředkovatelé trasování událostí pro Windows odeslány tooApplication Insights jako trasování. Informace o sledování události trasování událostí pro Windows najdete v tématu [události trasování událostí pro Windows pomocí](app-insights-asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
balíček Microsoft.ApplicationInsights Hello poskytuje hello [základní rozhraní API](https://msdn.microsoft.com/library/mt420197.aspx) z hello SDK. Hello telemetrická data z ostatních modulů použít, a můžete také [použít toodefine vlastní telemetrii](app-insights-api-custom-events-metrics.md).

* Žádný záznam v souboru ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) balíček NuGet. Pokud instalujete právě tento NuGet, vygeneruje se žádný soubor .config.

## <a name="telemetry-channel"></a>Kanál telemetrie
kanál telemetrie Hello spravuje ukládání do vyrovnávací paměti a přenos telemetrie toohello služby Application Insights.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`je výchozím hello kanálu pro služby. Data ukládány v paměti.
* `Microsoft.ApplicationInsights.PersistenceChannel`představuje alternativu pro konzolové aplikace. Jakékoli úložiště toopersistent unflushed data ho můžete uložit, když vaše aplikace zavře a odešle ho při spuštění aplikace hello znovu.

## <a name="telemetry-initializers-aspnet"></a>Inicializátory telemetrie (ASP.NET)
Inicializátory telemetrie nastavit kontext vlastnosti, které se odesílají společně s každou položkou telemetrie.

Můžete [zápisu vlastní inicializátory](app-insights-api-filtering-sampling.md#add-properties) tooset vlastností kontextu.

Standardní inicializátory Hello jsou nastavené buď hello Web nebo Windows Server NuGet balíčky:

* `AccountIdTelemetryInitializer`Nastaví vlastnost AccountId hello.
* `AuthenticatedUserIdTelemetryInitializer`Nastaví vlastnost AuthenticatedUserId hello jako sada podle hello JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`aktualizace hello `RoleName` a `RoleInstance` vlastnosti hello `Device` kontext pro všechny položky telemetrie vyplní hello Azure běhového prostředí.
* `BuildInfoConfigComponentVersionTelemetryInitializer`aktualizace hello `Version` vlastnost hello `Component` kontext pro všechny položky telemetrie s hodnotou hello extrahoval z hello `BuildInfo.config` souboru vytvořeného pomocí MS Build.
* `ClientIpHeaderTelemetryInitializer`aktualizace `Ip` vlastnost hello `Location` kontextu všechny položky telemetrie podle hello `X-Forwarded-For` hlavičky protokolu HTTP žádosti hello.
* `DeviceTelemetryInitializer`aktualizace hello následující vlastnosti hello `Device` kontext pro všechny položky telemetrie.
  * `Type`je nastaven příliš "Počítač"
  * `Id`je nastavit název domény toohello hello počítače se spuštěným hello webové aplikace.
  * `OemName`je nastavena hodnota toohello extrahoval z hello `Win32_ComputerSystem.Manufacturer` pole pomocí služby WMI.
  * `Model`je nastavena hodnota toohello extrahoval z hello `Win32_ComputerSystem.Model` pole pomocí služby WMI.
  * `NetworkType`je nastavena hodnota toohello extrahoval z hello `NetworkInterface`.
  * `Language`je nastaven toohello název hello `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`aktualizace hello `RoleInstance` vlastnost hello `Device` kontext pro všechny položky telemetrie s názvem domény hello hello počítače se spuštěným hello webové aplikace.
* `OperationNameTelemetryInitializer`aktualizace hello `Name` vlastnost hello `RequestTelemetry` a hello `Name` vlastnost hello `Operation` kontextu všechny položky telemetrie podle metoda hello HTTP, jakož i názvy ASP.NET MVC kontroleru a akce vyvolaná tooprocess hello požadavek.
* `OperationIdTelemetryInitializer`nebo `OperationCorrelationTelemetryInitializer` aktualizace hello `Operation.Id` vlastností kontextu všechny položky telemetrie sledovat při zpracování požadavku s hello automaticky generovány `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`aktualizace hello `Id` vlastnost hello `Session` kontext pro všechny položky telemetrie s hodnotou z hello extrahovat `ai_session` souboru cookie generované hello kód instrumentace ApplicationInsights JavaScript v prohlížeči hello uživatele.
* `SyntheticTelemetryInitializer`nebo `SyntheticUserAgentTelemetryInitializer` aktualizace hello `User`, `Session` a `Operation` kontexty vlastnosti všech položek telemetrie sledovat při zpracování požadavku z syntetické zdroje, například dostupnosti testů nebo vyhledávání modul robota. Ve výchozím nastavení [Průzkumníku metrik](app-insights-metrics-explorer.md) nezobrazí syntetické telemetrie.

    Hello `<Filters>` nastavte výchozí určující vlastnosti hello požadavků.
* `UserAgentTelemetryInitializer`aktualizace hello `UserAgent` vlastnost hello `User` kontextu všechny položky telemetrie podle hello `User-Agent` hlavičky protokolu HTTP žádosti hello.
* `UserTelemetryInitializer`aktualizace hello `Id` a `AcquisitionDate` vlastnosti `User` kontext pro všechny položky telemetrie s hodnotami z hello extrahovat `ai_user` souboru cookie generované kód instrumentace Application Insights JavaScript hello spuštěný v hello prohlížeče uživatele.
* `WebTestTelemetryInitializer`Nastaví hello id uživatele, id relace a vlastnosti syntetické zdroje pro požadavky HTTP, která pocházejí z [testy dostupnosti](app-insights-monitor-web-app-availability.md).
  Hello `<Filters>` nastavte výchozí určující vlastnosti hello požadavků.

Pro aplikace .NET běžící v Service Fabric, můžete zahrnout hello `Microsoft.ApplicationInsights.ServiceFabric` balíček NuGet. Tento balíček obsahuje `FabricTelemetryInitializer`, který přidává Service Fabric vlastnosti tootelemetry položky. Další informace najdete v tématu hello [GitHub stránce](https://go.microsoft.com/fwlink/?linkid=848457) o vlastnostech hello přidal tento balíček NuGet.

## <a name="telemetry-processors-aspnet"></a>Telemetrie procesorů (ASP.NET)
Telemetrie procesory můžete filtrovat a upravit každou položku telemetrie těsně před odesláním z portálu toohello SDK hello.

Můžete [psát vlastní telemetrii procesory](app-insights-api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Procesor telemetrie adaptivního vzorkování (od 2.0.0-beta3)
Tato možnost je ve výchozím nastavení zapnutá. Pokud vaše aplikace odešle velké množství telemetrická data, některé jeho tohoto procesoru odebere.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametr Hello poskytuje hello cíl, který hello algoritmus pokusí tooachieve. Nezávisle, každou instanci hello SDK funguje, takže pokud je server clusteru s podporou několik počítačů, skutečný objem hello telemetrie se násobí odpovídajícím způsobem.

[Další informace o vzorkování](app-insights-sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Procesor-míry vzorkování telemetrie (od 2.0.0-beta1)
Je také standardní [vzorkování procesoru telemetrie](app-insights-api-filtering-sampling.md) (z 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Parametry kanálu (Java)
Tyto parametry vliv na způsob hello sady Java SDK měli uložit a vyprázdnění hello telemetrická data, která shromažďuje.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
Hello počet položek telemetrie, které mohou být uloženy v úložišti hello SDK v paměti. Při dosažení tohoto počtu vyprázdní vyrovnávací paměť telemetrie hello – tedy hello telemetrie položky odesláním toohello Application Insights serveru.

* Min: 1
* Maximální počet: 1 000
* Výchozí: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Určuje, jak často hello data, která je uložená v úložišti hello v paměti by měla být vyprázdněn (odeslané tooApplication Insights).

* Min: 1
* Maximální počet: 300
* Výchozí: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Určuje hello maximální velikost v Megabajtech, která je vymezena toohello trvalé úložiště na místním disku hello. Toto úložiště se používá pro zachování telemetrie položky, které toobe přenášených toohello Application Insights koncového bodu se nezdařilo. Pokud velikost úložiště hello byly splněny, nové položky telemetrie se zahodí.

* Min: 1
* Maximální počet: 100
* Výchozí: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey
Určuje, ve kterém se zobrazí data prostředek Application Insights hello. Obvykle vytvoříte prostředek samostatné samostatné klíčem pro každý z vašich aplikací.

Pokud chcete klíč hello tooset dynamicky – například, pokud chcete výsledky toosend z vašich prostředků toodifferent aplikace – můžete vynechat hello klíč z hello konfiguračního souboru a nastavení v kódu.

klíč hello tooset pro všechny instance TelemetryClient, včetně standardní telemetrie moduly hello v nastavit TelemetryConfiguration.Active. V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET, postupujte takto:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Pokud chcete toosend konkrétní sada události tooa různých prostředků, můžete nastavit hello klíč pro konkrétní TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

tooget nový klíč, [vytvořte nový prostředek Application Insights portálu hello][new].

## <a name="next-steps"></a>Další kroky
[Další informace o hello rozhraní API][api].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
