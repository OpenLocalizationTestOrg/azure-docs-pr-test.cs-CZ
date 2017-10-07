---
title: "aaaAnalytics - hello vyhledávání výkonné nástroje Azure Application Insights | Microsoft Docs"
description: "Přehled analýzy, hello výkonné diagnostické vyhledávání nástroje Application Insights. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: d2b41e2fff7cc786e11fa3dfe94fc46f1b86e9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analytics-in-application-insights"></a>Analýza ve službě Application Insights
[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md). Tyto stránek popisují dotazovací jazyk analýzy protokolů. 

* **[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.
* **[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms hello.
* **[Referenční dokumentace jazyka](app-insights-analytics-reference.md)**  zjistěte, jak toouse všechny hello výkonné funkce hello dotazovacího jazyka pro analýzy protokolů.


## <a name="queries-in-analytics"></a>Dotazy v Analytics
Typické dotaz je *zdroj* tabulky následuje řadu *operátory* oddělených `|`. 

Například umožňuje zjistit, jaké čas den hello občanů Hyderabad zkuste naše webové aplikace. A když jsme existuje, podíváme se, co kódy výsledků jsou vráceny tootheir HTTP žádosti. 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

Jsme počet jedinečných klientských IP adres, je seskupit pomocí hello hodiny dne hello přes hello posledních 7 dnů. 

> [!NOTE]
> výsledky tooget mimo hello předchozích 24h, buď explicitně zahrnovat 'časové razítko' v dotazu, nebo použijte hello čas rozsah rozevírací nabídce.
>

Umožňuje zobrazit výsledky hello s hello panelu grafu prezentace, výběr toostack hello výsledky z různých odpovědi kódy:

![Zvolte pruhový graf, x a y osy a pak segmentace](./media/app-insights-analytics/020.png)

Vypadá to naše aplikace je nejoblíbenější v lunchtime a dna čas v Hyderabad. (A jsme byste měli prozkoumat tyto 500 kódů.)

Existují také výkonné statistické operace:

![Výsledky dotazu statistické](./media/app-insights-analytics/025.png)

jazyk Hello obsahuje mnoho atraktivní funkcí:


* [Filtr](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) telemetrie nezpracovaná aplikace podle všechna pole, včetně vlastní vlastnosti a metriky.
* [Připojení k](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) více tabulek – korelace požadavky s zobrazení stránky, volání závislostí, výjimek a protokolu trasování.
* Efektivní statistické [agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Stejně výkonné jako funkce SQL, ale mnohem snazší pro složité dotazy: místo vnoření příkazy, můžete další přesměrováním hello data z jednoho toohello základní operace.
* Okamžité a výkonné vizualizace.
* [PIN kód grafy, řídicí panely tooAzure](app-insights-analytics-using.md#pin-to-dashboard).
* [Export dotazy tooPower BI](app-insights-analytics-using.md#export-to-power-bi).
* Je [REST API](https://dev.applicationinsights.io/) používané toorun dotazy prostřednictvím kódu programu, například z prostředí Powershell.


## <a name="connect-tooyour-application-insights-data"></a>Připojení dat tooyour Application Insights
Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights: 

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a>Příklady dotazů

Vyzkoušejte tyto postupy tooillustrate hello výkon pomocí Analytics:

 *  [Automatické diagnostiky špičky a krok skáče v žádosti o doby trvání](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [Analýza výkonu degradations s analýzu časových řad](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [Analýza selhání aplikace s autocluster a diffpatterns](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [Detekce pokročilé obrazce s analýzu časových řad](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [Pomocí posuvné okno operations tooanalyze využití aplikace (vrácení MAU nebo DAU atd.)](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  [Detekce přerušení služeb na základě analýzy protokolů ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) a odpovídající blogu [zde](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).
 *  [Profilace výkonu aplikací pomocí jednoduchého ladění protokoly](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)
 *  [Měření doby trvání hello při každém kroku ve vaší toku kódu pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)
 *  [Analýza souběžnosti pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)



## <a name="next-steps"></a>Další kroky
* Doporučujeme začínat hello [jazyk prohlídka](app-insights-analytics-tour.md). 
* Další informace o [pomocí Analytics](app-insights-analytics-using.md). 
* [Referenční dokumentace jazyka](app-insights-analytics-reference.md). 
