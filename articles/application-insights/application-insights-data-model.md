---
title: "aaaAzure Application Insights Telemetrie datový Model | Microsoft Docs"
description: "Přehled modelu data aplikací – přehled"
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
ms.openlocfilehash: 5610519c3db8ec68d6cf787639204fb79724f511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetrie datový model

[Azure Application Insights](app-insights-overview.md) odesílá telemetrii z vaší webové aplikace toohello portál Azure, takže je můžete analyzovat hello výkonu a využití vaší aplikace. model telemetrie Hello je standardizované, aby bylo možné toocreate platformy a monitorování nezávislé na jazyku. 

Data shromážděná pomocí Application Insights modelů tento vzor provádění Typická aplikace:

![Application Insights aplikačního modelu](./media/application-insights-data-model/application-insights-data-model.png)

Hello následující typy telemetrických dat jsou použité toomonitor hello provádění vaší aplikace. Hello následující tři typy jsou obvykle automaticky shromážděná sadou hello Application Insights SDK z hello Architektura webových aplikací:

* [**Žádost o** ](application-insights-data-model-request-telemetry.md) -generované toolog žádost přijatých vaší aplikace. Například hello Application Insights web SDK automaticky generuje položku telemetrická žádost pro každý požadavek HTTP, která přijímá vaší webové aplikace. 

    **Operace** je hello vláken provádění, který zpracuje žádost. Můžete také [napsat kód](app-insights-api-custom-events-metrics.md#trackrequest) toomonitor jiné typy operací, například "probuzení" v webové úlohy nebo funkce, pravidelně zpracovává data.  Každá operace má identifikátor. Toto ID, které můžou být použité too[group]((application-insights-correlation.md) všechny telemetrická vygenerované během zpracování požadavku hello vaší aplikace. Každou operaci buď úspěšné nebo selže a doba trvání intervalu.
* [**Výjimka** ](application-insights-data-model-exception-telemetry.md) – obvykle představuje výjimku, která určuje, že operaci toofail.
* [**Závislost** ](application-insights-data-model-dependency-telemetry.md) -představuje volání z aplikace tooan externí služba nebo úložiště, jako je rozhraní API REST nebo SQL. V technologii ASP.NET, jsou definovány závislostí volání tooSQL `System.Data`. Volání tooHTTP koncové body jsou definovány `System.Net`. 

Application Insights obsahuje tři typy další data pro vlastní telemetrii:

* [Trasování](application-insights-data-model-trace-telemetry.md) – použít buď přímo nebo prostřednictvím protokolování diagnostiky tooimplement adaptéru pomocí představuje instrumentace rozhraní, které je známé tooyou, jako například `Log4Net` nebo `System.Diagnostics`.
* [Událost](application-insights-data-model-event-telemetry.md) – obvykle používaných toocapture interakce uživatele s vaší služby, tooanalyze vzorce používání.
* [Metrika](application-insights-data-model-metric-telemetry.md) -použít tooreport pravidelná skalární měření.

Každá položka telemetrie můžete definovat hello [informace o kontextu](application-insights-data-model-context.md) jako id aplikace verze nebo uživatelské relace. Kontext není sadu silného typu pole, která odblokuje určité scénáře. Když verze aplikace správně inicializována, Application Insights zjistit nový vzory v korelaci s novým nasazením chování aplikace. Id relace může být použité toocalculate hello výpadku nebo problém dopad na uživatele. Pro některé výpočet jednoznačného počtu hodnot id relace došlo k chybě závislosti, Chyba trasování nebo kritické výjimky dává dobrou znalost jazyka vliv.

Application Insights modelu telemetrie definuje způsob, jak příliš[korelovat](application-insights-correlation.md) telemetrie toohello operaci, která je součástí. Například žádost o můžete provádět volání databáze SQL a zaznamenává informace o diagnostiky. Můžete nastavit kontext korelace hello telemetrie položek, které tie ji zpět toohello požadavku telemetrie.

## <a name="schema-improvements"></a>Vylepšení schématu

Application Insights datový model je způsob, jak toomodel jednoduché a základní, ale výkonné telemetrii aplikace. Jsme zajistit tookeep hello modelu jednoduché a tenký toosupport základní scénáře a povolit tooextend hello schématu pro pokročilé uživatele.

tooreport datový model nebo schématu problémy a návrhy použít Githubu [ApplicationInsights domovské](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) úložiště.

## <a name="next-steps"></a>Další kroky

- [Psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)
- Zjistěte, jak příliš[rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).
- Použití [vzorkování](app-insights-sampling.md) toominimize množství telemetrii na základě dat modelu.
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
