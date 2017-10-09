---
title: "protokolů trasování .NET aaaExplore ve službě Application Insights"
description: "Vygenerovat pomocí trasování, NLog a Log4Net protokolů hledání."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 6bfcd9e5751c3656236d7eb2fc09321740171a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a>Prozkoumejte protokoly trasování .NET ve službě Application Insights
Pokud používáte NLog log4Net nebo System.Diagnostics.Trace pro diagnostické trasování v aplikaci ASP.NET, může mít vaše protokoly odeslané příliš[Azure Application Insights][start], kde můžete prozkoumat a vyhledávání je. Protokoly se sloučil s hello jiných telemetrie pocházející z vaší aplikace, takže můžete identifikovat hello trasování přidružené údržby každý požadavek uživatele a jejich korelující s jinými události a sestavy výjimek.

> [!NOTE]
> Je nutné modul zachycení hello protokolu? Je užitečné adaptér pro protokolovacích nástrojů 3. stran, ale pokud už nepoužíváte NLog, log4Net nebo System.Diagnostics.Trace, zvažte právě volání [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) přímo.
>
>

## <a name="install-logging-on-your-app"></a>Nainstalujte protokolování v aplikaci
Nainstalujte rozhraní framework vaši zvolenou protokolování ve vašem projektu. Výsledkem by mělo být položku v souboru app.config nebo web.config.

Pokud používáte System.Diagnostics.Trace, je třeba tooadd tooweb.config položku:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-toocollect-logs"></a>Konfigurace protokolů toocollect Application Insights
**[Přidejte Application Insights tooyour projekt](app-insights-asp-net.md)**  Pokud jste to neudělali, ještě. Uvidíte kolektor protokolů hello tooinclude možnost.

Nebo **konfigurovat Application Insights** kliknutím pravým tlačítkem na projekt v Průzkumníku řešení. Vyberte možnost hello příliš**konfigurace sběru trasovacích**.

*Žádná možnost Application Insights nabídky nebo protokolu kolekce?* Zkuste [řešení potíží s](#troubleshooting).

## <a name="manual-installation"></a>Ruční instalace
Tuto metodu použijte, pokud váš typ projektu není podporován hello Application Insights Instalační služby systému (třeba Windows desktop projektu).

1. Pokud máte v plánu toouse log4Net nebo NLog, nainstalujte ho do projektu.
2. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky NuGet**.
3. Vyhledání Application Insights
4. Vyberte odpovídající balíček hello – jeden z:

   * Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace volání)
   * Microsoft.ApplicationInsights.EventSourceListener (událostí EventSource toocapture)
   * Microsoft.ApplicationInsights.EtwListener (toocapture události trasování událostí pro Windows)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

balíček NuGet Hello nainstaluje potřebné sestavení hello a také upraví soubor web.config nebo app.config.

## <a name="insert-diagnostic-log-calls"></a>Vložit volání protokolů diagnostiky
Pokud používáte System.Diagnostics.Trace, typické volání by byl:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Pokud dáváte přednost log4net nebo NLog:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Pomocí událostí EventSource
Můžete nakonfigurovat [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) tooApplication toobe odeslané události Insights jako trasování. Nejdřív nainstalujte hello `Microsoft.ApplicationInsights.EventSourceListener` balíček NuGet. Upravte `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Pro každý zdroj můžete nastavit hello následující parametry:
 * `Name`Určuje název hello hello EventSource toocollect.
 * `Level`Určuje hello toocollect úrovně protokolování. Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Volitelné) Určuje celé číslo hello toouse kombinace klíčových slov.

## <a name="using-diagnosticsource-events"></a>Pomocí DiagnosticSource události
Můžete nakonfigurovat [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) tooApplication toobe odeslané události Insights jako trasování. Nejdřív nainstalujte hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) balíček NuGet. Upravte hello `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Pro každý DiagnosticSource tootrace, chcete přidat položku s hello `Name` toohello název vaší DiagnosticSource nastaven atribut.

## <a name="using-etw-events"></a>Pomocí události trasování událostí pro Windows
Můžete nakonfigurovat toobe události trasování událostí pro Windows odeslány tooApplication Insights jako trasování. Nejdřív nainstalujte hello `Microsoft.ApplicationInsights.EtwCollector` balíček NuGet. Upravte `TelemetryModules` části hello [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) souboru.

> [!NOTE] 
> Události trasování událostí pro Windows se můžou shromažďovat pouze pokud hello hostitelský proces hello SDK je spuštěný pod identitou, která je členem skupiny "Uživatelé protokolu výkonu" nebo správci.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Pro každý zdroj můžete nastavit hello následující parametry:
 * `ProviderName`je název hello toocollect zprostředkovatele trasování událostí pro Windows hello.
 * `ProviderGuid`Určuje hello GUID toocollect zprostředkovatele trasování událostí pro Windows hello, můžete použít místo `ProviderName`.
 * `Level`Nastaví hello toocollect úrovně protokolování. Může být jedna z `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Volitelné) sady hello celočíselnou hodnotu toouse kombinace – klíčové slovo.

## <a name="using-hello-trace-api-directly"></a>Přímo pomocí hello trasování API
Hello Application Insights trasování API můžete volat přímo. Toto rozhraní API použijte Hello protokolování adaptéry.

Například:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Výhodou TrackTrace je, že můžete umístit relativně long – datový uvítací zprávu. Například může zakódovat následných dat existuje.

Kromě toho můžete přidat zprávu tooyour úrovně závažnosti. A, podobně jako ostatní telemetrických dat, můžete přidat hodnoty vlastností, které můžete použít filtr toohelp nebo vyhledávání pro různé skupiny trasování. Například:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

To by vás, povolte v [vyhledávání][diagnostic], tooeasily filtru se všechny zprávy hello úrovně závažnosti konkrétní týkající se tooa konkrétní databáze.

## <a name="explore-your-logs"></a>Prozkoumejte protokoly
Spuštění aplikace, buď v režimu ladění, nebo ji nasadit za provozu.

V okně Přehled vaší aplikace v [portál Application Insights hello][portal], zvolte [vyhledávání][diagnostic].

![Ve službě Application Insights zvolte vyhledávání](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Search](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Je to možné, například:

* Filtrování v protokolu trasování nebo na položky s konkrétní vlastnosti
* Zkontrolujte konkrétní položku podrobně.
* Najít další telemetrii týkající se toohello stejné požadavku uživatele (to znamená, s hello stejné OperationId)
* Konfiguraci hello části této stránky uložit jako oblíbenou položku

> [!NOTE]
> **Vzorkování.** Pokud vaše aplikace odešle velké množství dat a používáte hello Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie. [Přečtěte si další informace o vzorkování.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Další kroky
[Diagnostikovat chyby a výjimky technologie ASP.NET][exceptions]

[Další informace o vyhledávání][diagnostic].

## <a name="troubleshooting"></a>Řešení potíží
### <a name="how-do-i-do-this-for-java"></a>Jak se to pro jazyk Java?
Použití hello [adaptéry protokolu Java](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a>Neexistuje žádná možnost Application Insights na místní nabídku hello projektu
* Kontrola instalace nástrojů Application Insights na tomto počítači pro vývoj. V nabídce Nástroje aplikace Visual Studio, rozšíření a aktualizace vyhledejte nástroje Application Insights. Pokud není na kartě hello nainstalovaná, otevřete kartu hello Online a nainstalujte ji.
* To může být typ projektu není podporováno nástrojů Application Insights. Použití [ruční instalace](#manual-installation).

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a>Žádná možnost pro adaptér protokolu v konfiguračním nástroji služby hello
* Nejdřív musíte rozhraní tooinstall hello protokolování.
* Pokud používáte System.Diagnostics.Trace, ověřte, že je [nakonfigurované v `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Mít jste získali nejnovější verzi hello Application Insights? V sadě Visual Studio **nástroje** nabídce zvolte **rozšíření a aktualizace**a otevřete hello **aktualizace** kartě. Pokud existuje Developer Analytics tools, klikněte na tlačítko tooupdate ho.

### <a name="emptykey"></a>Dojde k chybě "klíč instrumentace nemůže být prázdný"
Zdá se, zda jste nainstalovali hello protokolování balíček Nuget adaptér bez instalace služby Application Insights.

V Průzkumníku řešení klikněte pravým tlačítkem na `ApplicationInsights.config` a zvolte **aktualizace Application Insights**. Se vám zobrazí dialogové okno se žádostí můžete toosign v tooAzure a buď vytvořte prostředek Application Insights, nebo znovu použijte stávající. Který by měl opravte ji.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a>I najdete v trasování v diagnostické vyhledávání, ale není hello dalších událostí
Někdy může trvat nějakou dobu všechny hello události a žádosti o tooget kanálem hello.

### <a name="limits"></a>Množství dat, které se uchovávají?
Několik různých faktorů vliv hello množství dat, které jsou zachovány. V tématu hello [omezení](app-insights-api-custom-events-metrics.md#limits) oddílu hello zákazníka události metriky stránky pro další informace. 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a>Nejsou zobrazeny některé hello položky protokolu, které očekávat
Pokud vaše aplikace odešle velké množství dat a používáte hello Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie. [Přečtěte si další informace o vzorkování.](app-insights-sampling.md)

## <a name="add"></a>Další kroky
* [Nastavení dostupnosti a odezvy testů][availability]
* [Řešení potíží][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
