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
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="d8e7c-103">Application Insights pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8e7c-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="d8e7c-104">[Application Insights](app-insights-overview.md) umožňuje monitorování vaší webové aplikace pro dostupnosti, výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="d8e7c-105">S hello zpětnou vazbu, které máte o hello výkon a efektivitu aplikace v rámci hello divoký můžete provést informované volby o hello směr hello návrhu v každé životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-105">With hello feedback you get about hello performance and effectiveness of your app in hello wild, you can make informed choices about hello direction of hello design in each development lifecycle.</span></span>

![Příklad](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="d8e7c-107">Budete potřebovat předplatné s [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="d8e7c-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="d8e7c-108">Přihlaste se pomocí účtu Microsoft, který můžete mít zřízen pro Windows, XBox Live nebo jiné cloudové služby Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="d8e7c-109">Tým může mít tooAzure organizace předplatné: Požádejte vlastníka tooadd hello tooit pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-109">Your team might have an organizational subscription tooAzure: ask hello owner tooadd you tooit using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="d8e7c-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="d8e7c-110">Getting started</span></span>

* <span data-ttu-id="d8e7c-111">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Přidat > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="d8e7c-112">[Další informace](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="d8e7c-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="d8e7c-113">Pokud nevidíte tyto příkazy nabídky, postupujte podle hello [ruční získávání Příručka Začínáme](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="d8e7c-113">If you don't see those menu commands, follow hello [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="d8e7c-114">Může být nutné toodo to pokud projekt byl vytvořen s verzí sady Visual Studio před 2017.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-114">You may need toodo this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="d8e7c-115">Použití Application Insights</span><span class="sxs-lookup"><span data-stu-id="d8e7c-115">Using Application Insights</span></span>
<span data-ttu-id="d8e7c-116">Přihlaste se k hello [portálu Microsoft Azure](https://portal.azure.com), vyberte **všechny prostředky** nebo **Application Insights**a potom vyberte hello prostředků, které jste vytvořili toomonitor vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-116">Sign into hello [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select hello resource you created toomonitor your app.</span></span>

<span data-ttu-id="d8e7c-117">V samostatném okně prohlížeče používají vaši aplikaci nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="d8e7c-118">Zobrazí se data zobrazí v grafech hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-118">You'll see data appearing in hello Application Insights charts.</span></span> <span data-ttu-id="d8e7c-119">(Můžete mít tooclick aktualizace.) Budou existovat jenom malé množství dat při vyvíjíte, ale tyto grafy skutečně pocházet zachování připojení při publikování aplikace a mít mnoho uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-119">(You might have tooclick Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="d8e7c-120">na stránce Přehled Hello se zobrazí grafy výkonu klíče: doba odezvy serveru, čas načítání stránky a počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-120">hello overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="d8e7c-121">Další grafy a data, klikněte na možnost žádné toosee grafu.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-121">Click any chart toosee more charts and data.</span></span>

<span data-ttu-id="d8e7c-122">Zobrazení portálu hello spadají do tří kategorií:</span><span class="sxs-lookup"><span data-stu-id="d8e7c-122">Views in hello portal fall into three main categories:</span></span>

* <span data-ttu-id="d8e7c-123">[Průzkumníku metrik](app-insights-metrics-explorer.md) zobrazuje grafů a tabulek metriky a počty, například dobu odezvy, selhání sazby nebo metriky vytvořit sami pomocí hello [rozhraní API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d8e7c-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="d8e7c-124">Filtr a segment hello dat tooget hodnoty vlastnosti lépe porozumět vaší aplikace a její uživatele.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-124">Filter and segment hello data by property values tooget a better understanding of your app and its users.</span></span>
* <span data-ttu-id="d8e7c-125">[Hledání Explorer](app-insights-diagnostic-search.md) uvádí jednotlivé události, jako jsou konkrétní požadavky, výjimky, trasování protokolu nebo události, které jste sami vytvořili pomocí hello [rozhraní API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d8e7c-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="d8e7c-126">Filtrovat a vyhledávat hello události a pohyb mezi tooinvestigate problémy související události.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-126">Filter and search in hello events, and navigate among related events tooinvestigate issues.</span></span>
* <span data-ttu-id="d8e7c-127">[Analýza](app-insights-analytics.md) umožňuje spouštět dotazy podobné jazyku SQL přes telemetrie a je výkonný nástroj pro analýzu a diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="d8e7c-128">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="d8e7c-128">Alerts</span></span>
* <span data-ttu-id="d8e7c-129">Automaticky získáte [proaktivní diagnostické výstrahy](app-insights-proactive-diagnostics.md) , vás informovat o neobvyklé změny v sazby selhání a další metriky.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="d8e7c-130">Nastavit [testy dostupnosti](app-insights-monitor-web-app-availability.md) tootest webu průběžně z umístění po celém světě a získání e-maily také všechny testovací selže.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) tootest your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="d8e7c-131">Nastavit [metriky výstrahy](app-insights-monitor-web-app-availability.md) tooknow když metriky, například dobu odezvy nebo výjimka sazby mimo přijatelný omezení.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) tooknow if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="d8e7c-132">Video</span><span class="sxs-lookup"><span data-stu-id="d8e7c-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="d8e7c-133">Open source</span><span class="sxs-lookup"><span data-stu-id="d8e7c-133">Open source</span></span>
[<span data-ttu-id="d8e7c-134">Přečtěte si a přispívat toohello kódu</span><span class="sxs-lookup"><span data-stu-id="d8e7c-134">Read and contribute toohello code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="d8e7c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8e7c-135">Next steps</span></span>
* <span data-ttu-id="d8e7c-136">[Přidat telemetrii tooyour webové stránky](app-insights-javascript.md) toomonitor stránky využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-136">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page usage and performance.</span></span>
* <span data-ttu-id="d8e7c-137">[Monitorování závislostí](app-insights-asp-net-dependencies.md) toosee Pokud REST, SQL nebo jiné externí prostředky zpomalují můžete.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) toosee if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="d8e7c-138">[Pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md) toosend vlastní události a metriky pro další podrobné zobrazení výkonu a využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-138">[Use hello API](app-insights-api-custom-events-metrics.md) toosend your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="d8e7c-139">[Testy dostupnosti](app-insights-monitor-web-app-availability.md) zkontrolujte aplikaci neustále z kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="d8e7c-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around hello world.</span></span> 

