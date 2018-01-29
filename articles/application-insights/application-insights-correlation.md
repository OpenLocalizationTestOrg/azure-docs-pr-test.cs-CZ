---
title: Azure Application Insights Telemetrie korelace | Microsoft Docs
description: Application Insights telemetrie korelace
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: e821a640d3d75e712c022bd681eb07b83da91911
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>Korelace telemetrii ve službě Application Insights

Na světě služby malých každé logické operace vyžaduje práci v různých součástí služby. Každou z těchto součástí samostatně agentem lze sledovat [Application Insights](app-insights-overview.md). Součást webové aplikace komunikuje s součást zprostředkovatele ověřování k ověření pověření uživatele a s komponentou rozhraní API se získat data pro vizualizaci. Rozhraní API součásti následně můžete zadávat dotazy na data z jiných služeb a použití komponent, poskytovatel mezipaměti a oznámit komponentu fakturace o toto volání. Application Insights podporuje distribuované telemetrie korelace. Umožňuje zjistit, které komponenta je zodpovědná za selhání nebo snížení výkonu.

Tento článek vysvětluje datový model použít ke korelaci telemetrické zprávy odesílané součástmi více pomocí Application Insights. Pokrývá kontextu šíření technik a protokoly. Také vysvětluje implementace koncepty korelace na různé jazyky a platformy.

## <a name="telemetry-correlation-data-model"></a>Telemetrie korelace datový model

Definuje Application Insights [datový model](application-insights-data-model.md) pro distribuované telemetrie korelace. Pokud chcete přiřadit telemetrie logický provoz, každá položka telemetrie má kontext pole s názvem `operation_Id`. Tento identifikátor je sdílen každá položka telemetrie v distribuované trasování. Proto i přes ztrátu telemetrie z jedné vrstvy stále můžete přidružit telemetrie hlášené ostatní součásti.

Distribuované logický provoz se obvykle skládá z sady menší operací - žádostí zpracovaných rozhraním jedna z komponent. Tyto operace jsou definovány [požadavku telemetrie](application-insights-data-model-request-telemetry.md). Každý požadavek telemetrie má svou vlastní `id` identifikující globálně jedinečné. A všechny telemetrie – trasování, výjimky, atd. s touto žádostí měli nastavit `operation_parentId` na hodnotu požadavku `id`.

Každé odchozí operace jako volání protokolu http na jinou součást reprezentována [telemetrických závislostí](application-insights-data-model-dependency-telemetry.md). Telemetrických závislostí také definuje vlastní `id` , je globálně jedinečný. Požadavek telemetrie, iniciovaná toto volání závislostí použije jako `operation_parentId`.

Můžete vytvořit v zobrazení distribuované logické operace pomocí `operation_Id`, `operation_parentId`, a `request.id` s `dependency.id`. Tato pole také definovat pořadí příčiny telemetrii volání.

V prostředí malých služeb může trasování ze součásti přejděte do různých úložných. Všechny komponenty může mít svůj vlastní klíč instrumentace ve službě Application Insights. Pro získání telemetrických dat pro logický provoz, musíte se k dotazování na data z každé úložiště. Po velký počet úložných je potřeba mít na kde hledat další nápovědu.

Application Insights datový model definuje dvě pole chcete tento problém vyřešit: `request.source` a `dependency.target`. První pole identifikuje komponentu závislostí žádost iniciovala a druhý identifikuje, která komponenta vrátila odpověď volání závislostí.


## <a name="example"></a>Příklad

Podívejme se na příklad ceny STOCK aplikace zobrazuje aktuální cena trhu populace pomocí externí rozhraní API volat rozhraní API akcií. CENY STOCK aplikace má na stránce `Stock page` otevřít s použitím klienta webové prohlížeče `GET /Home/Stock`. Aplikace dotazuje rozhraní API STOCK pomocí volání protokolu HTTP `GET /api/stock/value`.

Můžete analyzovat výsledné telemetrie spuštění dotazu:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

V poznámce zobrazení výsledek, že všechny položky telemetrie sdílet kořenu `operation_Id`. Při volání ajax provedené na stránce – nové jedinečné id `qJSXU` je přiřazena k telemetrických závislostí a stránkové zobrazení na id slouží jako `operation_ParentId`. Dále požadavek serveru používá rozhraní ajax na id jako `operation_ParentId`atd.

| Typ položky   | jméno                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Stránkové zobrazení   | Uložené stránky                |              | STYz               | STYz         |
| závislosti | / Home GET/Stock           | qJSXU        | STYz               | STYz         |
| Požadavek    | Domovské GET/Stock            | KqKwlrSt9PA = | qJSXU              | STYz         |
| závislosti | ZÍSKAT /api/stock/value      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Nyní když volání `GET /api/stock/value` provedené na externí službu chcete znát identitu tohoto serveru. Abyste mohli nastavit `dependency.target` pole správně. Když externí služba nepodporuje monitorování - `target` nastavena na název hostitele služby jako `stock-prices-api.com`. Ale pokud služby identifikuje vrácením i předdefinovanou hlavičky protokolu HTTP - `target` obsahuje identitu služby, která umožňuje vytvářet distribuované trasování dotazováním telemetrie z dané služby Application Insights. 

## <a name="correlation-headers"></a>Korelace hlavičky

Pracujeme na návrh RFC [korelace protokolu HTTP](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Tento návrh definuje dvě hlavičky:

- `Request-Id`provádění globálně jedinečné id volání
- `Correlation-Context`-provádění kolekce párů hodnot názvu vlastnosti distribuované trasování

Standardní také definuje dvě schémata `Request-Id` generování - centralizovaného a hierarchického. S plochým schématu, je dobře známé `Id` definovaný pro klíč `Correlation-Context` kolekce.

Definuje Application Insights [rozšíření](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) pro korelačního protokolu HTTP. Používá `Request-Context` název páry hodnota potřebný k šíření kolekci vlastností, které jsou používány bezprostředního volajícího nebo volaný. Application Insights SDK používá tuto hlavičku nastavit `dependency.target` a `request.source` pole.

## <a name="open-tracing-and-application-insights"></a>Otevřete trasování a Application Insights

[Otevřete trasování](http://opentracing.io/) a data modelů vypadá Application Insights 

- `request`, `pageView` mapuje **rozpětí** s`span.kind = server`
- `dependency`se mapuje na **rozpětí** s`span.kind = client`
- `id`nástroje `request` a `dependency` mapuje **Span.Id**
- `operation_Id`se mapuje na **TraceId**
- `operation_ParentId`se mapuje na **odkaz** typu`ChildOf`

V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.

V tématu [specifikace](https://github.com/opentracing/specification/blob/master/specification.md) a [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) definice otevřít trasování konceptů.


## <a name="telemetry-correlation-in-net"></a>Korelace telemetrie v rozhraní .NET

V čase rozhraní .NET definovány různými způsoby korelovat telemetrii a diagnostické protokoly. Je `System.Diagnostics.CorrelationManager` umožní sledovat [LogicalOperationStack a aktivity](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`a Windows ETW definovat metodu [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`používá [protokolu obory](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF a Http přenosová nahoru "aktuální" šíření kontextu.

Ale tyto metody nepomohly podporu automatického distribuované trasování. `DiagnosticsSource`je způsob, jak podporují automatické mezi počítači korelace. Knihovny .NET podporuje diagnostiky zdrojů a povolit automatické mezi počítači šíření kontextu korelace prostřednictvím přenosu jako http.

[Průvodce aktivity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve zdroji diagnostiky vysvětluje základy sledování aktivit. 

Jádro ASP.NET 2.0 podporuje extrakce hlavičky protokolu Http a spuštění nové aktivity. 

`System.Net.HttpClient`Počáteční verze `<fill in>` podporuje automatické vkládání korelace hlavičky protokolu Http a sledování volání protokolu http jako aktivita.

Je-li nový modul Http [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) pro ASP.NET Classic. Tento modul implementuje pomocí DiagnosticsSource korelace telemetrie. Spustí aktivita podle příchozí hlavičky žádosti. Korelaci také telemetrie z různých fází zpracování žádosti. I pro případy, spuštění každé fáze zpracování služby IIS na jiný spravovat vláken.

Application Insights SDK počáteční verze `2.4.0-beta1` používá k shromažďování telemetrických dat a přidružte ji k aktuální aktivita DiagnosticsSource a aktivity. 

## <a name="next-steps"></a>Další kroky

- [Psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)
- Zařadit všechny součásti služby malých v Application Insights. Podívejte se na [podporované platformy](app-insights-platforms.md).
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Zjistěte, jak [rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).
