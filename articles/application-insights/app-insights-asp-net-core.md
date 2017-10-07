---
title: aaaAzure Application Insights pro ASP.NET Core | Microsoft Docs
description: "Sledování webových aplikací pro dostupnosti, výkonu a využití."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: a7a27f9eef1daec5b0deae9fd88906e646980659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights pro ASP.NET Core
[Application Insights](app-insights-overview.md) umožňuje monitorování vaší webové aplikace pro dostupnosti, výkonu a využití. S hello zpětnou vazbu, které máte o hello výkon a efektivitu aplikace v rámci hello divoký můžete provést informované volby o hello směr hello návrhu v každé životního cyklu.

![Příklad](./media/app-insights-asp-net-core/sample.png)

Budete potřebovat předplatné s [Microsoft Azure](http://azure.com). Přihlaste se pomocí účtu Microsoft, který můžete mít zřízen pro Windows, XBox Live nebo jiné cloudové služby Microsoftu. Tým může mít tooAzure organizace předplatné: Požádejte vlastníka tooadd hello tooit pomocí účtu Microsoft.

## <a name="getting-started"></a>Začínáme

* V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Přidat > Application Insights**. [Další informace](app-insights-asp-net.md).
* Pokud nevidíte tyto příkazy nabídky, postupujte podle hello [ruční získávání Příručka Začínáme](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Může být nutné toodo to pokud projekt byl vytvořen s verzí sady Visual Studio před 2017.

## <a name="using-application-insights"></a>Použití Application Insights
Přihlaste se k hello [portálu Microsoft Azure](https://portal.azure.com), vyberte **všechny prostředky** nebo **Application Insights**a potom vyberte hello prostředků, které jste vytvořili toomonitor vaší aplikaci.

V samostatném okně prohlížeče používají vaši aplikaci nějakou dobu. Zobrazí se data zobrazí v grafech hello Application Insights. (Můžete mít tooclick aktualizace.) Budou existovat jenom malé množství dat při vyvíjíte, ale tyto grafy skutečně pocházet zachování připojení při publikování aplikace a mít mnoho uživatelů. 

na stránce Přehled Hello se zobrazí grafy výkonu klíče: doba odezvy serveru, čas načítání stránky a počet neúspěšných požadavků. Další grafy a data, klikněte na možnost žádné toosee grafu.

Zobrazení portálu hello spadají do tří kategorií:

* [Průzkumníku metrik](app-insights-metrics-explorer.md) zobrazuje grafů a tabulek metriky a počty, například dobu odezvy, selhání sazby nebo metriky vytvořit sami pomocí hello [rozhraní API](app-insights-api-custom-events-metrics.md). Filtr a segment hello dat tooget hodnoty vlastnosti lépe porozumět vaší aplikace a její uživatele.
* [Hledání Explorer](app-insights-diagnostic-search.md) uvádí jednotlivé události, jako jsou konkrétní požadavky, výjimky, trasování protokolu nebo události, které jste sami vytvořili pomocí hello [rozhraní API](app-insights-api-custom-events-metrics.md). Filtrovat a vyhledávat hello události a pohyb mezi tooinvestigate problémy související události.
* [Analýza](app-insights-analytics.md) umožňuje spouštět dotazy podobné jazyku SQL přes telemetrie a je výkonný nástroj pro analýzu a diagnostiky.

## <a name="alerts"></a>Výstrahy
* Automaticky získáte [proaktivní diagnostické výstrahy](app-insights-proactive-diagnostics.md) , vás informovat o neobvyklé změny v sazby selhání a další metriky.
* Nastavit [testy dostupnosti](app-insights-monitor-web-app-availability.md) tootest webu průběžně z umístění po celém světě a získání e-maily také všechny testovací selže.
* Nastavit [metriky výstrahy](app-insights-monitor-web-app-availability.md) tooknow když metriky, například dobu odezvy nebo výjimka sazby mimo přijatelný omezení.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Open source
[Přečtěte si a přispívat toohello kódu](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Další kroky
* [Přidat telemetrii tooyour webové stránky](app-insights-javascript.md) toomonitor stránky využití a výkonu.
* [Monitorování závislostí](app-insights-asp-net-dependencies.md) toosee Pokud REST, SQL nebo jiné externí prostředky zpomalují můžete.
* [Pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md) toosend vlastní události a metriky pro další podrobné zobrazení výkonu a využití vaší aplikace.
* [Testy dostupnosti](app-insights-monitor-web-app-availability.md) zkontrolujte aplikaci neustále z kolem hello, world. 

