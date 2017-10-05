---
title: Azure Application Insights pro ASP.NET Core | Microsoft Docs
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
ms.openlocfilehash: d86495eea467977f6c079de72e2b49a2a1da2b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="265c4-103">Application Insights pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="265c4-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="265c4-104">[Application Insights](app-insights-overview.md) umožňuje monitorování vaší webové aplikace pro dostupnosti, výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="265c4-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="265c4-105">Na základě zpětné vazby ohledně výkonu a efektivity vaší aplikace při běžném používání můžete informovaně rozhodovat o směrování návrhu v každé fázi vývoje.</span><span class="sxs-lookup"><span data-stu-id="265c4-105">With the feedback you get about the performance and effectiveness of your app in the wild, you can make informed choices about the direction of the design in each development lifecycle.</span></span>

![Příklad](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="265c4-107">Budete potřebovat předplatné s [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="265c4-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="265c4-108">Přihlaste se pomocí účtu Microsoft, který můžete mít zřízen pro Windows, XBox Live nebo jiné cloudové služby Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="265c4-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="265c4-109">Tým může mít předplatné pro společnosti do Azure: Požádejte vlastníka, aby je přidejte do ní pomocí svého účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="265c4-109">Your team might have an organizational subscription to Azure: ask the owner to add you to it using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="265c4-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="265c4-110">Getting started</span></span>

* <span data-ttu-id="265c4-111">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Přidat > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="265c4-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="265c4-112">[Další informace](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="265c4-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="265c4-113">Pokud nevidíte tyto příkazy nabídky, postupujte podle kroků [ruční získávání Příručka Začínáme](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="265c4-113">If you don't see those menu commands, follow the [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="265c4-114">Můžete to udělat, pokud projekt byl vytvořen s verzí sady Visual Studio před 2017.</span><span class="sxs-lookup"><span data-stu-id="265c4-114">You may need to do this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="265c4-115">Použití Application Insights</span><span class="sxs-lookup"><span data-stu-id="265c4-115">Using Application Insights</span></span>
<span data-ttu-id="265c4-116">Přihlaste se k [portálu Microsoft Azure](https://portal.azure.com), vyberte **všechny prostředky** nebo **Application Insights**a pak vyberte prostředek, který jste vytvořili pro sledování aplikace.</span><span class="sxs-lookup"><span data-stu-id="265c4-116">Sign into the [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select the resource you created to monitor your app.</span></span>

<span data-ttu-id="265c4-117">V samostatném okně prohlížeče používají vaši aplikaci nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="265c4-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="265c4-118">Zobrazí se data zobrazí v grafech Application Insights.</span><span class="sxs-lookup"><span data-stu-id="265c4-118">You'll see data appearing in the Application Insights charts.</span></span> <span data-ttu-id="265c4-119">(Možná bude muset klikněte na tlačítko Aktualizovat.) Budou existovat jenom malé množství dat při vyvíjíte, ale tyto grafy skutečně pocházet zachování připojení při publikování aplikace a mít mnoho uživatelů.</span><span class="sxs-lookup"><span data-stu-id="265c4-119">(You might have to click Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="265c4-120">Na stránce Přehled se zobrazí grafy výkonu klíče: doba odezvy serveru, čas načítání stránky a počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="265c4-120">The overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="265c4-121">Klikněte na libovolný graf zobrazíte další grafy a data.</span><span class="sxs-lookup"><span data-stu-id="265c4-121">Click any chart to see more charts and data.</span></span>

<span data-ttu-id="265c4-122">Zobrazení portálu spadají do tří kategorií:</span><span class="sxs-lookup"><span data-stu-id="265c4-122">Views in the portal fall into three main categories:</span></span>

* <span data-ttu-id="265c4-123">[Průzkumníku metrik](app-insights-metrics-explorer.md) zobrazuje grafů a tabulek metriky a počty, například dobu odezvy, selhání sazby nebo metriky můžete vytvořit sami pomocí [rozhraní API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="265c4-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="265c4-124">Filtrovat a rozdělit data pomocí hodnoty vlastností pro dokázali lépe porozumět vaší aplikace a jeho uživatelů.</span><span class="sxs-lookup"><span data-stu-id="265c4-124">Filter and segment the data by property values to get a better understanding of your app and its users.</span></span>
* <span data-ttu-id="265c4-125">[Hledání Explorer](app-insights-diagnostic-search.md) uvádí jednotlivé události, jako jsou konkrétní požadavky, výjimky, trasování protokolu nebo události, které jste vytvořili sami pomocí [rozhraní API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="265c4-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="265c4-126">Filtrovat a hledání v událostech a pohyb mezi souvisejících událostí k prozkoumání problémů.</span><span class="sxs-lookup"><span data-stu-id="265c4-126">Filter and search in the events, and navigate among related events to investigate issues.</span></span>
* <span data-ttu-id="265c4-127">[Analýza](app-insights-analytics.md) umožňuje spouštět dotazy podobné jazyku SQL přes telemetrie a je výkonný nástroj pro analýzu a diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="265c4-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="265c4-128">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="265c4-128">Alerts</span></span>
* <span data-ttu-id="265c4-129">Automaticky získáte [proaktivní diagnostické výstrahy](app-insights-proactive-diagnostics.md) , vás informovat o neobvyklé změny v sazby selhání a další metriky.</span><span class="sxs-lookup"><span data-stu-id="265c4-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="265c4-130">Nastavit [testy dostupnosti](app-insights-monitor-web-app-availability.md) otestovat váš web průběžně z umístění po celém světě, a také všechny testovací nezdaří získat e-mailů.</span><span class="sxs-lookup"><span data-stu-id="265c4-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) to test your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="265c4-131">Nastavit [metriky výstrahy](app-insights-monitor-web-app-availability.md) vědět, pokud přejděte metriky, například dobu odezvy nebo výjimka sazby mimo přijatelný omezení.</span><span class="sxs-lookup"><span data-stu-id="265c4-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) to know if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="265c4-132">Video</span><span class="sxs-lookup"><span data-stu-id="265c4-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="265c4-133">Open source</span><span class="sxs-lookup"><span data-stu-id="265c4-133">Open source</span></span>
[<span data-ttu-id="265c4-134">Přečtěte si a přispívat ke kódu</span><span class="sxs-lookup"><span data-stu-id="265c4-134">Read and contribute to the code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="265c4-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="265c4-135">Next steps</span></span>
* <span data-ttu-id="265c4-136">[Přidání telemetrických údajů do webových stránek](app-insights-javascript.md) k monitorování stránky využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="265c4-136">[Add telemetry to your web pages](app-insights-javascript.md) to monitor page usage and performance.</span></span>
* <span data-ttu-id="265c4-137">[Monitorování závislostí](app-insights-asp-net-dependencies.md) chcete zobrazit, pokud REST, SQL nebo jiné externí prostředky zpomalují můžete.</span><span class="sxs-lookup"><span data-stu-id="265c4-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) to see if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="265c4-138">[Použít rozhraní API](app-insights-api-custom-events-metrics.md) k odeslání vlastní události a metriky pro další podrobné zobrazení výkonu a využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="265c4-138">[Use the API](app-insights-api-custom-events-metrics.md) to send your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="265c4-139">[Testy dostupnosti](app-insights-monitor-web-app-availability.md) zkontrolujte aplikaci neustále z po celém světě.</span><span class="sxs-lookup"><span data-stu-id="265c4-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around the world.</span></span> 

