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
# <a name="analytics-in-application-insights"></a><span data-ttu-id="a333e-103">Analýza ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="a333e-103">Analytics in Application Insights</span></span>
<span data-ttu-id="a333e-104">[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a333e-104">[Analytics](app-insights-analytics.md) is the powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="a333e-105">Tyto stránek popisují dotazovací jazyk analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="a333e-105">These pages describe the Log Analytics query language.</span></span> 

* <span data-ttu-id="a333e-106">**[Podívejte se na úvodní video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="a333e-106">**[Watch the introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="a333e-107">**[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není odesílání dat do služby Application Insights ještě.</span><span class="sxs-lookup"><span data-stu-id="a333e-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data to Application Insights yet.</span></span>
* <span data-ttu-id="a333e-108">**[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms.</span><span class="sxs-lookup"><span data-stu-id="a333e-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates the most common idioms.</span></span>
* <span data-ttu-id="a333e-109">**[Referenční dokumentace jazyka](app-insights-analytics-reference.md)**  Naučte se používat výkonné funkce analýzy protokolů dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="a333e-109">**[Language Reference](app-insights-analytics-reference.md)** Learn how to use all the powerful features of the Log Analytics query language.</span></span>


## <a name="queries-in-analytics"></a><span data-ttu-id="a333e-110">Dotazy v Analytics</span><span class="sxs-lookup"><span data-stu-id="a333e-110">Queries in Analytics</span></span>
<span data-ttu-id="a333e-111">Typické dotaz je *zdroj* tabulky následuje řadu *operátory* oddělených `|`.</span><span class="sxs-lookup"><span data-stu-id="a333e-111">A typical query is a *source* table followed by a series of *operators* separated by `|`.</span></span> 

<span data-ttu-id="a333e-112">Například umožňuje zjistit, jaké čas občany Hyderabad zkuste naše webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a333e-112">For example, let's find out what time of day the citizens of Hyderabad try our web app.</span></span> <span data-ttu-id="a333e-113">A když jsme existuje, podíváme se, jaký kódy výsledků se vrátíte na jejich požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="a333e-113">And while we're there, let's see what result codes are returned to their HTTP requests.</span></span> 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

<span data-ttu-id="a333e-114">Jsme počet jedinečných klientských IP adres, je seskupit podle hodin dne za posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="a333e-114">We count distinct client IP addresses, grouping them by the hour of the day over the past 7 days.</span></span> 

> [!NOTE]
> <span data-ttu-id="a333e-115">Chcete-li získat výsledky mimo předchozích 24h, buď explicitně zahrnovat 'časové razítko' v dotazu nebo použijte rozevírací nabídky čas rozsah.</span><span class="sxs-lookup"><span data-stu-id="a333e-115">To get results outside the previous 24h, either include 'timestamp' explicitly in your query, or use the time range drop-down menu.</span></span>
>

<span data-ttu-id="a333e-116">Umožňuje zobrazit výsledky s prezentací pruhový graf, rozhodnete zásobníku výsledky z různých odpovědi kódy:</span><span class="sxs-lookup"><span data-stu-id="a333e-116">Let's display the results with the bar chart presentation, choosing to stack the results from different response codes:</span></span>

![Zvolte pruhový graf, x a y osy a pak segmentace](./media/app-insights-analytics/020.png)

<span data-ttu-id="a333e-118">Vypadá to naše aplikace je nejoblíbenější v lunchtime a dna čas v Hyderabad.</span><span class="sxs-lookup"><span data-stu-id="a333e-118">Looks like our app is most popular at lunchtime and bed-time in Hyderabad.</span></span> <span data-ttu-id="a333e-119">(A jsme byste měli prozkoumat tyto 500 kódů.)</span><span class="sxs-lookup"><span data-stu-id="a333e-119">(And we should investigate those 500 codes.)</span></span>

<span data-ttu-id="a333e-120">Existují také výkonné statistické operace:</span><span class="sxs-lookup"><span data-stu-id="a333e-120">There are also powerful statistical operations:</span></span>

![Výsledky dotazu statistické](./media/app-insights-analytics/025.png)

<span data-ttu-id="a333e-122">Jazyk obsahuje mnoho atraktivní funkcí:</span><span class="sxs-lookup"><span data-stu-id="a333e-122">The language has many attractive features:</span></span>


* <span data-ttu-id="a333e-123">[Filtr](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) telemetrie nezpracovaná aplikace podle všechna pole, včetně vlastní vlastnosti a metriky.</span><span class="sxs-lookup"><span data-stu-id="a333e-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) your raw app telemetry by any fields, including your custom properties and metrics.</span></span>
* <span data-ttu-id="a333e-124">[Připojení k](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) více tabulek – korelace požadavky s zobrazení stránky, volání závislostí, výjimek a protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="a333e-124">[Join](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) multiple tables – correlate requests with page views, dependency calls, exceptions and log traces.</span></span>
* <span data-ttu-id="a333e-125">Efektivní statistické [agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="a333e-125">Powerful statistical [aggregations](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>
* <span data-ttu-id="a333e-126">Stejně výkonné jako funkce SQL, ale mnohem snazší pro složité dotazy: místo vnoření příkazy přesměrováním data z jedné základní operace na další.</span><span class="sxs-lookup"><span data-stu-id="a333e-126">Just as powerful as SQL, but much easier for complex queries: instead of nesting statements, you pipe the data from one elementary operation to the next.</span></span>
* <span data-ttu-id="a333e-127">Okamžité a výkonné vizualizace.</span><span class="sxs-lookup"><span data-stu-id="a333e-127">Immediate and powerful visualizations.</span></span>
* <span data-ttu-id="a333e-128">[Připnout grafy na Azure řídicí panely](app-insights-analytics-using.md#pin-to-dashboard).</span><span class="sxs-lookup"><span data-stu-id="a333e-128">[Pin charts to Azure dashboards](app-insights-analytics-using.md#pin-to-dashboard).</span></span>
* <span data-ttu-id="a333e-129">[Exportovat dotazy do Power BI](app-insights-analytics-using.md#export-to-power-bi).</span><span class="sxs-lookup"><span data-stu-id="a333e-129">[Export queries to Power BI](app-insights-analytics-using.md#export-to-power-bi).</span></span>
* <span data-ttu-id="a333e-130">Je [REST API](https://dev.applicationinsights.io/) používané ke spouštění dotazů prostřednictvím kódu programu, například z prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="a333e-130">There's a [REST API](https://dev.applicationinsights.io/) that you can use to run queries programmatically, for example from Powershell.</span></span>


## <a name="connect-to-your-application-insights-data"></a><span data-ttu-id="a333e-131">Připojit se k datům Application Insights</span><span class="sxs-lookup"><span data-stu-id="a333e-131">Connect to your Application Insights data</span></span>
<span data-ttu-id="a333e-132">Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights:</span><span class="sxs-lookup"><span data-stu-id="a333e-132">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span> 

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a><span data-ttu-id="a333e-134">Video</span><span class="sxs-lookup"><span data-stu-id="a333e-134">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a><span data-ttu-id="a333e-135">Příklady dotazů</span><span class="sxs-lookup"><span data-stu-id="a333e-135">Query examples</span></span>

<span data-ttu-id="a333e-136">Vyzkoušejte tyto postupy pro ilustraci sílu pomocí Analytics:</span><span class="sxs-lookup"><span data-stu-id="a333e-136">Try these walkthroughs to illustrate the power of using Analytics:</span></span>

 *  [<span data-ttu-id="a333e-137">Automatické diagnostiky špičky a krok skáče v žádosti o doby trvání</span><span class="sxs-lookup"><span data-stu-id="a333e-137">Automatic diagnostics of spikes and step jumps in requests durations</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [<span data-ttu-id="a333e-138">Analýza výkonu degradations s analýzu časových řad</span><span class="sxs-lookup"><span data-stu-id="a333e-138">Analyzing performance degradations with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="a333e-139">Analýza selhání aplikace s autocluster a diffpatterns</span><span class="sxs-lookup"><span data-stu-id="a333e-139">Analyzing application failures with autocluster and diffpatterns</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [<span data-ttu-id="a333e-140">Detekce pokročilé obrazce s analýzu časových řad</span><span class="sxs-lookup"><span data-stu-id="a333e-140">Advanced shape detections with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="a333e-141">Použití posuvné okno operations k analýze využití aplikace (vrácení MAU nebo DAU atd.)</span><span class="sxs-lookup"><span data-stu-id="a333e-141">Using sliding window operations to analyze application usage (rolling MAU/DAU etc)</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  <span data-ttu-id="a333e-142">[Detekce přerušení služeb na základě analýzy protokolů ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) a odpovídající blogu [zde](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span><span class="sxs-lookup"><span data-stu-id="a333e-142">[Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) and a matching blog post [here](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span></span>
 *  <span data-ttu-id="a333e-143">[Profilace výkonu aplikací pomocí jednoduchého ladění protokoly](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span><span class="sxs-lookup"><span data-stu-id="a333e-143">[Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span></span>
 *  <span data-ttu-id="a333e-144">[Měření doby trvání při každém kroku ve vaší toku kódu pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="a333e-144">[Measuring the duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span></span>
 *  <span data-ttu-id="a333e-145">[Analýza souběžnosti pomocí protokolů jednoduché ladění](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) a odpovídající blogu [sem](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="a333e-145">[Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span></span>



## <a name="next-steps"></a><span data-ttu-id="a333e-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a333e-146">Next steps</span></span>
* <span data-ttu-id="a333e-147">Doporučujeme začínat [jazyk prohlídka](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="a333e-147">We recommend you start with the [language tour](app-insights-analytics-tour.md).</span></span> 
* <span data-ttu-id="a333e-148">Další informace o [pomocí Analytics](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="a333e-148">More about [using Analytics](app-insights-analytics-using.md).</span></span> 
* <span data-ttu-id="a333e-149">[Referenční dokumentace jazyka](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="a333e-149">[Language reference](app-insights-analytics-reference.md).</span></span> 
