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
# <a name="analytics-in-application-insights"></a><span data-ttu-id="59bfa-103">Analýza ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="59bfa-103">Analytics in Application Insights</span></span>
<span data-ttu-id="59bfa-104">[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59bfa-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="59bfa-105">Tyto stránek popisují dotazovací jazyk analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="59bfa-105">These pages describe the Log Analytics query language.</span></span> 

* <span data-ttu-id="59bfa-106">**[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="59bfa-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="59bfa-107">**[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="59bfa-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="59bfa-108">**[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms hello.</span><span class="sxs-lookup"><span data-stu-id="59bfa-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>
* <span data-ttu-id="59bfa-109">**[Referenční dokumentace jazyka](app-insights-analytics-reference.md)**  zjistěte, jak toouse všechny hello výkonné funkce hello dotazovacího jazyka pro analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="59bfa-109">**[Language Reference](app-insights-analytics-reference.md)** Learn how toouse all hello powerful features of hello Log Analytics query language.</span></span>


## <a name="queries-in-analytics"></a><span data-ttu-id="59bfa-110">Dotazy v Analytics</span><span class="sxs-lookup"><span data-stu-id="59bfa-110">Queries in Analytics</span></span>
<span data-ttu-id="59bfa-111">Typické dotaz je *zdroj* tabulky následuje řadu *operátory* oddělených `|`.</span><span class="sxs-lookup"><span data-stu-id="59bfa-111">A typical query is a *source* table followed by a series of *operators* separated by `|`.</span></span> 

<span data-ttu-id="59bfa-112">Například umožňuje zjistit, jaké čas den hello občanů Hyderabad zkuste naše webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="59bfa-112">For example, let's find out what time of day hello citizens of Hyderabad try our web app.</span></span> <span data-ttu-id="59bfa-113">A když jsme existuje, podíváme se, co kódy výsledků jsou vráceny tootheir HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="59bfa-113">And while we're there, let's see what result codes are returned tootheir HTTP requests.</span></span> 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

<span data-ttu-id="59bfa-114">Jsme počet jedinečných klientských IP adres, je seskupit pomocí hello hodiny dne hello přes hello posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="59bfa-114">We count distinct client IP addresses, grouping them by hello hour of hello day over hello past 7 days.</span></span> 

> [!NOTE]
> <span data-ttu-id="59bfa-115">výsledky tooget mimo hello předchozích 24h, buď explicitně zahrnovat 'časové razítko' v dotazu, nebo použijte hello čas rozsah rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="59bfa-115">tooget results outside hello previous 24h, either include 'timestamp' explicitly in your query, or use hello time range drop-down menu.</span></span>
>

<span data-ttu-id="59bfa-116">Umožňuje zobrazit výsledky hello s hello panelu grafu prezentace, výběr toostack hello výsledky z různých odpovědi kódy:</span><span class="sxs-lookup"><span data-stu-id="59bfa-116">Let's display hello results with hello bar chart presentation, choosing toostack hello results from different response codes:</span></span>

![Zvolte pruhový graf, x a y osy a pak segmentace](./media/app-insights-analytics/020.png)

<span data-ttu-id="59bfa-118">Vypadá to naše aplikace je nejoblíbenější v lunchtime a dna čas v Hyderabad.</span><span class="sxs-lookup"><span data-stu-id="59bfa-118">Looks like our app is most popular at lunchtime and bed-time in Hyderabad.</span></span> <span data-ttu-id="59bfa-119">(A jsme byste měli prozkoumat tyto 500 kódů.)</span><span class="sxs-lookup"><span data-stu-id="59bfa-119">(And we should investigate those 500 codes.)</span></span>

<span data-ttu-id="59bfa-120">Existují také výkonné statistické operace:</span><span class="sxs-lookup"><span data-stu-id="59bfa-120">There are also powerful statistical operations:</span></span>

![Výsledky dotazu statistické](./media/app-insights-analytics/025.png)

<span data-ttu-id="59bfa-122">jazyk Hello obsahuje mnoho atraktivní funkcí:</span><span class="sxs-lookup"><span data-stu-id="59bfa-122">hello language has many attractive features:</span></span>


* <span data-ttu-id="59bfa-123">[Filtr](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) telemetrie nezpracovaná aplikace podle všechna pole, včetně vlastní vlastnosti a metriky.</span><span class="sxs-lookup"><span data-stu-id="59bfa-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) your raw app telemetry by any fields, including your custom properties and metrics.</span></span>
* <span data-ttu-id="59bfa-124">[Připojení k](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) více tabulek – korelace požadavky s zobrazení stránky, volání závislostí, výjimek a protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="59bfa-124">[Join](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) multiple tables – correlate requests with page views, dependency calls, exceptions and log traces.</span></span>
* <span data-ttu-id="59bfa-125">Efektivní statistické [agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="59bfa-125">Powerful statistical [aggregations](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>
* <span data-ttu-id="59bfa-126">Stejně výkonné jako funkce SQL, ale mnohem snazší pro složité dotazy: místo vnoření příkazy, můžete další přesměrováním hello data z jednoho toohello základní operace.</span><span class="sxs-lookup"><span data-stu-id="59bfa-126">Just as powerful as SQL, but much easier for complex queries: instead of nesting statements, you pipe hello data from one elementary operation toohello next.</span></span>
* <span data-ttu-id="59bfa-127">Okamžité a výkonné vizualizace.</span><span class="sxs-lookup"><span data-stu-id="59bfa-127">Immediate and powerful visualizations.</span></span>
* <span data-ttu-id="59bfa-128">[PIN kód grafy, řídicí panely tooAzure](app-insights-analytics-using.md#pin-to-dashboard).</span><span class="sxs-lookup"><span data-stu-id="59bfa-128">[Pin charts tooAzure dashboards](app-insights-analytics-using.md#pin-to-dashboard).</span></span>
* <span data-ttu-id="59bfa-129">[Export dotazy tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span><span class="sxs-lookup"><span data-stu-id="59bfa-129">[Export queries tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span></span>
* <span data-ttu-id="59bfa-130">Je [REST API](https://dev.applicationinsights.io/) používané toorun dotazy prostřednictvím kódu programu, například z prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="59bfa-130">There's a [REST API](https://dev.applicationinsights.io/) that you can use toorun queries programmatically, for example from Powershell.</span></span>


## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="59bfa-131">Připojení dat tooyour Application Insights</span><span class="sxs-lookup"><span data-stu-id="59bfa-131">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="59bfa-132">Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights:</span><span class="sxs-lookup"><span data-stu-id="59bfa-132">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span> 

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a><span data-ttu-id="59bfa-134">Video</span><span class="sxs-lookup"><span data-stu-id="59bfa-134">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a><span data-ttu-id="59bfa-135">Příklady dotazů</span><span class="sxs-lookup"><span data-stu-id="59bfa-135">Query examples</span></span>

<span data-ttu-id="59bfa-136">Vyzkoušejte tyto postupy tooillustrate hello výkon pomocí Analytics:</span><span class="sxs-lookup"><span data-stu-id="59bfa-136">Try these walkthroughs tooillustrate hello power of using Analytics:</span></span>

 *  [<span data-ttu-id="59bfa-137">Automatické diagnostiky špičky a krok skáče v žádosti o doby trvání</span><span class="sxs-lookup"><span data-stu-id="59bfa-137">Automatic diagnostics of spikes and step jumps in requests durations</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [<span data-ttu-id="59bfa-138">Analýza výkonu degradations s analýzu časových řad</span><span class="sxs-lookup"><span data-stu-id="59bfa-138">Analyzing performance degradations with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="59bfa-139">Analýza selhání aplikace s autocluster a diffpatterns</span><span class="sxs-lookup"><span data-stu-id="59bfa-139">Analyzing application failures with autocluster and diffpatterns</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [<span data-ttu-id="59bfa-140">Detekce pokročilé obrazce s analýzu časových řad</span><span class="sxs-lookup"><span data-stu-id="59bfa-140">Advanced shape detections with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="59bfa-141">Pomocí posuvné okno operations tooanalyze využití aplikace (vrácení MAU nebo DAU atd.)</span><span class="sxs-lookup"><span data-stu-id="59bfa-141">Using sliding window operations tooanalyze application usage (rolling MAU/DAU etc)</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  <span data-ttu-id="59bfa-142">[Detekce přerušení služeb na základě analýzy protokolů ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) a odpovídající blogu [zde](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span><span class="sxs-lookup"><span data-stu-id="59bfa-142">[Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) and a matching blog post [here](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span></span>
 *  <span data-ttu-id="59bfa-143">[Profilace výkonu aplikací pomocí jednoduchého ladění protokoly](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span><span class="sxs-lookup"><span data-stu-id="59bfa-143">[Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span></span>
 *  <span data-ttu-id="59bfa-144">[Měření doby trvání hello při každém kroku ve vaší toku kódu pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="59bfa-144">[Measuring hello duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span></span>
 *  <span data-ttu-id="59bfa-145">[Analýza souběžnosti pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="59bfa-145">[Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span></span>



## <a name="next-steps"></a><span data-ttu-id="59bfa-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59bfa-146">Next steps</span></span>
* <span data-ttu-id="59bfa-147">Doporučujeme začínat hello [jazyk prohlídka](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="59bfa-147">We recommend you start with hello [language tour](app-insights-analytics-tour.md).</span></span> 
* <span data-ttu-id="59bfa-148">Další informace o [pomocí Analytics](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="59bfa-148">More about [using Analytics](app-insights-analytics-using.md).</span></span> 
* <span data-ttu-id="59bfa-149">[Referenční dokumentace jazyka](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="59bfa-149">[Language reference](app-insights-analytics-reference.md).</span></span> 
