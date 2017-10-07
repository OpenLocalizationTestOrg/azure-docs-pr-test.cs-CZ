---
title: aaaAzure Application Insights Telemetrie korelace | Microsoft Docs
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
ms.author: bwren
ms.openlocfilehash: 3ed8c589d237cac5daceac939ca893b7d81a2967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>Korelace telemetrii ve službě Application Insights

Hello world malých služeb každé logické operace vyžaduje práci v různých komponentách služby hello. Každou z těchto součástí samostatně agentem lze sledovat [Application Insights](app-insights-overview.md). součást aplikace webové Hello komunikuje s pověřeními uživatele toovalidate součást zprostředkovatele ověřování a s hello rozhraní API součásti tooget data pro vizualizaci. součást rozhraní API Hello následně můžete zadávat dotazy na data z jiných služeb a použití komponent, poskytovatel mezipaměti a oznámit fakturace součásti hello o toto volání. Application Insights podporuje distribuované telemetrie korelace. Umožňuje vám toodetect, která komponenta je zodpovědná za selhání nebo snížení výkonu.

Tento článek vysvětluje hello datový model používá telemetrie Application Insights toocorrelate poslal více součástí. Pokrývá hello kontextu šíření technik a protokoly. Také vysvětluje hello implementace hello korelace konceptů na různé jazyky a platformy.

## <a name="telemetry-correlation-data-model"></a>Telemetrie korelace datový model

Definuje Application Insights [datový model](application-insights-data-model.md) pro distribuované telemetrie korelace. tooassociate telemetrie s hello logický provoz, každá položka telemetrie má kontext pole s názvem `operation_Id`. Tento identifikátor je sdílen každá položka telemetrie v hello distribuované trasování. Proto i přes ztrátu telemetrie z jedné vrstvy stále můžete přidružit telemetrie hlášené ostatní součásti.

Distribuované logický provoz se obvykle skládá z sady menší operací - požadavků zpracovaných pomocí jedné z komponenty hello. Tyto operace jsou definovány [požadavku telemetrie](application-insights-data-model-request-telemetry.md). Každý požadavek telemetrie má svou vlastní `id` identifikující globálně jedinečné. A všechny telemetrie – trasování, výjimky, atd. s touto žádostí měli nastavit hello `operation_parentId` toohello hodnotu hello požadavku `id`.

Každé odchozí operace jako součást tooanother volání http reprezentována [telemetrických závislostí](application-insights-data-model-dependency-telemetry.md). Telemetrických závislostí také definuje vlastní `id` , je globálně jedinečný. Požadavek telemetrie, iniciovaná toto volání závislostí použije jako `operation_parentId`.

Můžete vytvořit zobrazení hello pomocí distribuovaných logický provoz `operation_Id`, `operation_parentId`, a `request.id` s `dependency.id`. Tato pole definovat také hello příčiny pořadí volání telemetrie.

V prostředí malých služeb může trasování ze součásti přejděte různých úložných toohello. Všechny komponenty může mít svůj vlastní klíč instrumentace ve službě Application Insights. tooget telemetrie hello logické operaci, musíte tooquery data z každé úložiště. Po velký počet úložných potřebujete toohave nápovědu, ve kterém toolook Další.

Application Insights datový model definuje dvě pole toosolve tohoto problému: `request.source` a `dependency.target`. první pole Hello identifikuje hello komponenty, která hello závislostí žádost iniciovala a hello druhý identifikuje která komponenta vrátila odpověď hello hello závislostí volání.


## <a name="example"></a>Příklad

Podívejme se na příklad ceny STOCK aplikace zobrazuje hello aktuální trhu ceny akcie pomocí hello externí rozhraní API volat rozhraní API akcií. Hello STOCK ceny aplikace má stránku `Stock page` otevřeli pomocí hello klienta webové prohlížeče `GET /Home/Stock`. dotazy aplikace Hello hello STOCK rozhraní API pomocí volání protokolu HTTP `GET /api/stock/value`.

Můžete analyzovat výsledné telemetrie spuštění dotazu:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

V hello výsledek zobrazení, Všimněte si, že všechny položky telemetrie sdílet kořenovou složku hello `operation_Id`. Při volání ajax provedené ze stránky hello – nové jedinečné id `qJSXU` telemetrických závislostí přiřazené toohello a stránkové zobrazení na id slouží jako `operation_ParentId`. Dále požadavek serveru používá rozhraní ajax na id jako `operation_ParentId`atd.

| Typ položky   | jméno                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Stránkové zobrazení   | Uložené stránky                |              | STYz               | STYz         |
| závislosti | / Home GET/Stock           | qJSXU        | STYz               | STYz         |
| Požadavek    | Domovské GET/Stock            | KqKwlrSt9PA = | qJSXU              | STYz         |
| závislosti | ZÍSKAT /api/stock/value      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Nyní když hello volání `GET /api/stock/value` provedené externí služba tooan chcete tooknow hello identitu tohoto serveru. Abyste mohli nastavit `dependency.target` pole správně. Když hello externí služba nepodporuje monitorování - `target` nastavena toohello název hostitele služby hello jako `stock-prices-api.com`. Ale pokud služby identifikuje vrácením i předdefinovanou hlavičky protokolu HTTP - `target` obsahuje hello identitu služby, který umožňuje trasování toobuild distributed Application Insights dotazováním telemetrie z dané služby. 

## <a name="correlation-headers"></a>Korelace hlavičky

Pracujeme na RFC návrh hello [korelace protokolu HTTP](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Tento návrh definuje dvě hlavičky:

- `Request-Id`provádění hello globálně jedinečné id hello volání
- `Correlation-Context`-provádění hello název hodnota páry kolekci vlastností hello distribuované trasování

Hello standardní také definuje dvě schémata `Request-Id` generování - centralizovaného a hierarchického. S hello ploché schématu, je dobře známé `Id` klíč pro hello `Correlation-Context` kolekce.

Application Insights definuje hello [rozšíření](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) pro korelaci hello protokolu HTTP. Používá `Request-Context` název toopropagate hello kolekci vlastností, které jsou používány bezprostředního volajícího hello nebo volaný páry hodnota. Application Insights SDK používá tato záhlaví tooset `dependency.target` a `request.source` pole.

## <a name="open-tracing-and-application-insights"></a>Otevřete trasování a Application Insights

[Otevřete trasování](http://opentracing.io/) a data modelů vypadá Application Insights 

- `request`, `pageView` mapuje příliš**rozpětí** s`span.kind = server`
- `dependency`mapuje příliš**rozpětí** s`span.kind = client`
- `id`nástroje `request` a `dependency` mapuje příliš**Span.Id**
- `operation_Id`mapuje příliš**TraceId**
- `operation_ParentId`mapuje příliš**odkaz** typu`ChileOf`

V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.

V tématu [specifikace](https://github.com/opentracing/specification/blob/master/specification.md) a [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) definice otevřít trasování konceptů.


## <a name="telemetry-correlation-in-net"></a>Korelace telemetrie v rozhraní .NET

V čase .NET definované počet způsoby toocorrelate telemetrie a diagnostické protokoly. Je `System.Diagnostics.CorrelationManager` povolení tootrack [LogicalOperationStack a aktivity](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`a definovat Windows trasování událostí pro Windows hello metoda [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`používá [protokolu obory](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF a Http přenosová nahoru "aktuální" šíření kontextu.

Ale tyto metody nepomohly podporu automatického distribuované trasování. `DiagnosticsSource`je způsob, jak toosupport automatické mezi počítači korelace. Knihovny .NET podporuje diagnostiky zdrojů a povolit automatické křížové počítač šíření hello korelace kontextu prostřednictvím přenosu hello jako http.

Hello [Průvodce tooActivities](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve zdroji diagnostiky vysvětluje základy hello sledování aktivit. 

Jádro ASP.NET 2.0 podporuje extrakce hlaviček protokolu Http a spouštění hello nová aktivita. 

`System.Net.HttpClient`Počáteční verze `<fill in>` podporuje automatické vkládání hello korelace hlavičky protokolu Http a sledování volání protokolu http hello jako aktivita.

Je-li nový modul Http [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) pro hello ASP.NET Classic. Tento modul implementuje pomocí DiagnosticsSource korelace telemetrie. Spustí aktivita podle příchozí hlavičky žádosti. Korelaci také telemetrie z hello různých fází zpracování žádosti. I pro hello případech, kdy jednotlivé fáze zpracování IIS běží na různých spravovat vláken.

Application Insights SDK počáteční verze `2.4.0-beta1` používá DiagnosticsSource a aktivity toocollect telemetrie a přidružte ji k aktuální aktivita hello. 

## <a name="next-steps"></a>Další kroky

- [Psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)
- Zařadit všechny součásti služby malých v Application Insights. Podívejte se na [podporované platformy](app-insights-platforms.md).
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Zjistěte, jak příliš[rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).
