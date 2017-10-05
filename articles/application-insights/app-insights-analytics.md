---
title: "Analýza – nástroj výkonné vyhledávání systému Azure Application Insights | Microsoft Docs"
description: "Přehled analýzy, nástroj výkonné diagnostické vyhledávání služby Application Insights. "
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
ms.openlocfilehash: 8174745a00a107eea648b223a00466b6a7f37331
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="analytics-in-application-insights"></a>Analýza ve službě Application Insights
[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí [Application Insights](app-insights-overview.md). Tyto stránek popisují dotazovací jazyk analýzy protokolů. 

* **[Podívejte se na úvodní video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není odesílání dat do služby Application Insights ještě.
* **[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms.
* **[Referenční dokumentace jazyka](app-insights-analytics-reference.md)**  Naučte se používat výkonné funkce analýzy protokolů dotazovací jazyk.


## <a name="queries-in-analytics"></a>Dotazy v Analytics
Typické dotaz je *zdroj* tabulky následuje řadu *operátory* oddělených `|`. 

Například umožňuje zjistit, jaké čas občany Hyderabad zkuste naše webové aplikace. A když jsme existuje, podíváme se, jaký kódy výsledků se vrátíte na jejich požadavky HTTP. 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

Jsme počet jedinečných klientských IP adres, je seskupit podle hodin dne za posledních 7 dnů. 

> [!NOTE]
> Chcete-li získat výsledky mimo předchozích 24h, buď explicitně zahrnovat 'časové razítko' v dotazu nebo použijte rozevírací nabídky čas rozsah.
>

Umožňuje zobrazit výsledky s prezentací pruhový graf, rozhodnete zásobníku výsledky z různých odpovědi kódy:

![Zvolte pruhový graf, x a y osy a pak segmentace](./media/app-insights-analytics/020.png)

Vypadá to naše aplikace je nejoblíbenější v lunchtime a dna čas v Hyderabad. (A jsme byste měli prozkoumat tyto 500 kódů.)

Existují také výkonné statistické operace:

![Výsledky dotazu statistické](./media/app-insights-analytics/025.png)

Jazyk obsahuje mnoho atraktivní funkcí:


* [Filtr](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) telemetrie nezpracovaná aplikace podle všechna pole, včetně vlastní vlastnosti a metriky.
* [Připojení k](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) více tabulek – korelace požadavky s zobrazení stránky, volání závislostí, výjimek a protokolu trasování.
* Efektivní statistické [agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Stejně výkonné jako funkce SQL, ale mnohem snazší pro složité dotazy: místo vnoření příkazy přesměrováním data z jedné základní operace na další.
* Okamžité a výkonné vizualizace.
* [Připnout grafy na Azure řídicí panely](app-insights-analytics-using.md#pin-to-dashboard).
* [Exportovat dotazy do Power BI](app-insights-analytics-using.md#export-to-power-bi).
* Je [REST API](https://dev.applicationinsights.io/) používané ke spouštění dotazů prostřednictvím kódu programu, například z prostředí Powershell.


## <a name="connect-to-your-application-insights-data"></a>Připojit se k datům Application Insights
Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights: 

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a>Příklady dotazů

Vyzkoušejte tyto postupy pro ilustraci sílu pomocí Analytics:

 *  [Automatické diagnostiky špičky a krok skáče v žádosti o doby trvání](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [Analýza výkonu degradations s analýzu časových řad](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [Analýza selhání aplikace s autocluster a diffpatterns](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [Detekce pokročilé obrazce s analýzu časových řad](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [Použití posuvné okno operations k analýze využití aplikace (vrácení MAU nebo DAU atd.)](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  [Detekce přerušení služeb na základě analýzy protokolů ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) a odpovídající blogu [zde](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).
 *  [Profilace výkonu aplikací pomocí jednoduchého ladění protokoly](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)
 *  [Měření doby trvání při každém kroku ve vaší toku kódu pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)
 *  [Analýza souběžnosti pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)



## <a name="next-steps"></a>Další kroky
* Doporučujeme začínat [jazyk prohlídka](app-insights-analytics-tour.md). 
* Další informace o [pomocí Analytics](app-insights-analytics-using.md). 
* [Referenční dokumentace jazyka](app-insights-analytics-reference.md). 
