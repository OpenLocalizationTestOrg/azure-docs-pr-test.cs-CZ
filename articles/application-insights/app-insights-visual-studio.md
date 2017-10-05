---
title: "Ladění aplikací pomocí služby Azure Application Insights v sadě Visual Studio | Microsoft Docs"
description: "Analýza výkonu a diagnostika webové aplikace během ladění a v produkčním prostředí."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: e0ac2bf01992520cdbea22a232dc42d678d77c7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a><span data-ttu-id="eee92-103">Ladění aplikací pomocí služby Azure Application Insights v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eee92-103">Debug your applications with Azure Application Insights in Visual Studio</span></span>
<span data-ttu-id="eee92-104">V sadě Visual Studio (2015 a novější) můžete analyzovat výkon a diagnostikovat problémy ve vaší webové aplikaci v ASP.NET během ladění i v produkčním prostředí pomocí telemetrie z [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eee92-104">In Visual Studio (2015 and later), you can analyze performance and diagnose issues in your ASP.NET web app both in debugging and in production, using telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="eee92-105">Pokud jste webovou aplikaci v ASP.NET vytvořili pomocí sady Visual Studio 2017 nebo novější, sada Application Insights SDK už v ní je.</span><span class="sxs-lookup"><span data-stu-id="eee92-105">If you created your ASP.NET web app using Visual Studio 2017 or later, it already has the Application Insights SDK.</span></span> <span data-ttu-id="eee92-106">V opačném případě, pokud jste to ještě neudělali, [přidejte Application Insights do své aplikace](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="eee92-106">Otherwise, if you haven't done so already, [add Application Insights to your app](app-insights-asp-net.md).</span></span>

<span data-ttu-id="eee92-107">Pokud chcete monitorovat aplikaci za provozu v produkčním prostředí, telemetrii Application Insights normálně zobrazíte na webu [Azure Portal](https://portal.azure.com), kde můžete nastavit upozornění a použít výkonné monitorovací nástroje.</span><span class="sxs-lookup"><span data-stu-id="eee92-107">To monitor your app when it's in live production, you normally view the Application Insights telemetry in the [Azure portal](https://portal.azure.com), where you can set alerts and apply powerful monitoring tools.</span></span> <span data-ttu-id="eee92-108">Pro účely ladění ale můžete vyhledávat a analyzovat telemetrii také v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eee92-108">But for debugging, you can also search and analyze the telemetry in Visual Studio.</span></span> <span data-ttu-id="eee92-109">Visual Studio můžete použít k analýze telemetrie z vaší pracoviště a z ladění na vývojovém počítači běží.</span><span class="sxs-lookup"><span data-stu-id="eee92-109">You can use Visual Studio to analyze telemetry both from your production site and from debugging runs on your development machine.</span></span> <span data-ttu-id="eee92-110">V druhém případě můžete spuštěné ladění analyzovat, i když jste ještě nenakonfigurovali sadu SDK k odesílání telemetrie na web Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="eee92-110">In the latter case, you can analyze debugging runs even if you haven't yet configured the SDK to send telemetry to the Azure portal.</span></span> 

## <span data-ttu-id="eee92-111"><a name="run"></a> Ladění projektu</span><span class="sxs-lookup"><span data-stu-id="eee92-111"><a name="run"></a> Debug your project</span></span>
<span data-ttu-id="eee92-112">Spusťte webovou aplikaci v režimu místního ladění pomocí klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="eee92-112">Run your web app in local debug mode by using F5.</span></span> <span data-ttu-id="eee92-113">Otevřete různé stránky k vygenerování nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eee92-113">Open different pages to generate some telemetry.</span></span>

<span data-ttu-id="eee92-114">V sadě Visual Studio zobrazí počet událostí, které byly zaprotokolovány modulem Application Insights ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="eee92-114">In Visual Studio, you see a count of the events that have been logged by the Application Insights module in your project.</span></span>

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

<span data-ttu-id="eee92-116">Kliknutím na toto tlačítko můžete vyhledávat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="eee92-116">Click this button to search your telemetry.</span></span> 

## <a name="application-insights-search"></a><span data-ttu-id="eee92-117">Hledání Application Insights</span><span class="sxs-lookup"><span data-stu-id="eee92-117">Application Insights search</span></span>
<span data-ttu-id="eee92-118">V okně Hledání Application Insights se zobrazí události, které byly zaprotokolovány.</span><span class="sxs-lookup"><span data-stu-id="eee92-118">The Application Insights Search window shows events that have been logged.</span></span> <span data-ttu-id="eee92-119">(Pokud jste přihlášeni do Azure při nastavení Application Insights, můžete vyhledávat stejné události na portálu Azure.)</span><span class="sxs-lookup"><span data-stu-id="eee92-119">(If you signed in to Azure when you set up Application Insights, you can search the same events in the Azure portal.)</span></span>

![Klikněte pravým tlačítkem myši na projekt a vyberte Application Insights, Vyhledávání](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> <span data-ttu-id="eee92-121">Jakmile vyberete nebo zrušíte výběr filtrů, klikněte na tlačítko Vyhledat na konci textového vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="eee92-121">After you select or deselect filters, click the Search button at the end of the text search field.</span></span>
>

<span data-ttu-id="eee92-122">Textové vyhledávání funguje na všechna pole v událostech.</span><span class="sxs-lookup"><span data-stu-id="eee92-122">The free text search works on any fields in the events.</span></span> <span data-ttu-id="eee92-123">Například vyhledejte část adresy URL stránky nebo hodnotu vlastnosti, například města klienta; nebo určitá slova v protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="eee92-123">For example, search for part of the URL of a page; or the value of a property such as client city; or specific words in a trace log.</span></span>

<span data-ttu-id="eee92-124">Kliknutím na libovolnou událost zobrazíte podrobné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="eee92-124">Click any event to see its detailed properties.</span></span>

<span data-ttu-id="eee92-125">Pokud chcete zobrazit podrobné vlastnosti požadavků na webovou aplikaci, můžete se proklikat ke kódu.</span><span class="sxs-lookup"><span data-stu-id="eee92-125">For requests to your web app, you can click through to the code.</span></span>

![V části Podrobnosti o požadavku se proklikejte ke kódu](./media/app-insights-visual-studio/31.png)

<span data-ttu-id="eee92-127">Můžete také otevřít související položky a pomocí nich diagnostikovat neúspěšné požadavky nebo výjimky.</span><span class="sxs-lookup"><span data-stu-id="eee92-127">You can also open related items to help diagnose failed requests or exceptions.</span></span>

![V části Podrobnosti o požadavku přejděte dolů k souvisejícím položkám](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a><span data-ttu-id="eee92-129">Zobrazit výjimky a nezdařených požadavků.</span><span class="sxs-lookup"><span data-stu-id="eee92-129">View exceptions and failed requests</span></span>
<span data-ttu-id="eee92-130">Sestavy výjimek se zobrazí v okně Hledání.</span><span class="sxs-lookup"><span data-stu-id="eee92-130">Exception reports show in the Search window.</span></span> <span data-ttu-id="eee92-131">(V některých starších typech aplikací v ASP.NET je potřeba [nastavit monitorování výjimek](app-insights-asp-net-exceptions.md), abyste viděli výjimky, které rozhraní zpracovává.)</span><span class="sxs-lookup"><span data-stu-id="eee92-131">(In some older types of ASP.NET application, you have to [set up exception monitoring](app-insights-asp-net-exceptions.md) to see exceptions that are handled by the framework.)</span></span>

<span data-ttu-id="eee92-132">Klikněte na výjimku a získejte trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="eee92-132">Click an exception to get a stack trace.</span></span> <span data-ttu-id="eee92-133">Pokud je kód aplikace otevřen v sadě Visual Studio, můžete kliknutím z trasování zásobníku přejít na příslušný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="eee92-133">If the code of the app is open in Visual Studio, you can click through from the stack trace to the relevant line of the code.</span></span>

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-the-code"></a><span data-ttu-id="eee92-135">Zobrazit souhrny požadavku a výjimek v kódu</span><span class="sxs-lookup"><span data-stu-id="eee92-135">View request and exception summaries in the code</span></span>
<span data-ttu-id="eee92-136">V řádku kódu přehledu výše každá metoda obslužné rutiny uvidíte počet požadavky a výjimkami, přihlášení pomocí Application Insights v posledních 24 h.</span><span class="sxs-lookup"><span data-stu-id="eee92-136">In the Code Lens line above each handler method, you see a count of the requests and exceptions logged by Application Insights in the past 24 h.</span></span>

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> <span data-ttu-id="eee92-138">Code Lens zobrazí data Application Insights, pouze pokud jste [nakonfigurovali aplikaci k odesílání telemetrie na portál Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="eee92-138">Code Lens shows Application Insights data only if you have [configured your app to send telemetry to the Application Insights portal](app-insights-asp-net.md).</span></span>
>

[<span data-ttu-id="eee92-139">Další informace o Application Insights v Code Lens</span><span class="sxs-lookup"><span data-stu-id="eee92-139">More about Application Insights in Code Lens</span></span>](app-insights-visual-studio-codelens.md)

## <a name="trends"></a><span data-ttu-id="eee92-140">Trendy</span><span class="sxs-lookup"><span data-stu-id="eee92-140">Trends</span></span>
<span data-ttu-id="eee92-141">Trendy představují nástroj pro vizualizaci chování aplikace v čase.</span><span class="sxs-lookup"><span data-stu-id="eee92-141">Trends is a tool for visualizing how your app behaves over time.</span></span> 

<span data-ttu-id="eee92-142">Vybírejte z **Trendů zkoumání telemetrie** z tlačítka panelu nástrojů Application Insights nebo okna hledání Application Insights.</span><span class="sxs-lookup"><span data-stu-id="eee92-142">Choose **Explore Telemetry Trends** from the Application Insights toolbar button or Application Insights Search window.</span></span> <span data-ttu-id="eee92-143">Zvolte jeden z pěti běžných dotazů, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="eee92-143">Choose one of five common queries to get started.</span></span> <span data-ttu-id="eee92-144">Na základě typů telemetrie, časových rozsahů a dalších vlastností můžete analyzovat různé datové sady.</span><span class="sxs-lookup"><span data-stu-id="eee92-144">You can analyze different datasets based on telemetry types, time ranges, and other properties.</span></span> 

<span data-ttu-id="eee92-145">Pokud chcete vyhledat anomálie v datech, vyberte jednu z možností anomálií v rozevíracím seznamu „Typ zobrazení“.</span><span class="sxs-lookup"><span data-stu-id="eee92-145">To find anomalies in your data, choose one of the anomaly options under the "View Type" dropdown.</span></span> <span data-ttu-id="eee92-146">Možnosti filtrování v dolní části okna usnadňují zdokonalování v konkrétních podmnožinách vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eee92-146">The filtering options at the bottom of the window make it easy to hone in on specific subsets of your telemetry.</span></span>

![Trendy](./media/app-insights-visual-studio/51.png)

<span data-ttu-id="eee92-148">[Další informace o trendech](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="eee92-148">[More about Trends](app-insights-visual-studio-trends.md).</span></span>

## <a name="local-monitoring"></a><span data-ttu-id="eee92-149">Místní monitorování</span><span class="sxs-lookup"><span data-stu-id="eee92-149">Local monitoring</span></span>
<span data-ttu-id="eee92-150">(Z Visual Studio 2015 Update 2) Pokud jste nenakonfigurovali SDK k odesílání telemetrie na portál Application Insights (takže existuje v souboru ApplicationInsights.config nenachází žádný klíč instrumentace) zobrazí okno diagnostiky telemetrii z nejnovější relace ladění.</span><span class="sxs-lookup"><span data-stu-id="eee92-150">(From Visual Studio 2015 Update 2) If you haven't configured the SDK to send telemetry to the Application Insights portal (so that there is no instrumentation key in ApplicationInsights.config) then the diagnostics window displays telemetry from your latest debugging session.</span></span> 

<span data-ttu-id="eee92-151">Toto je žádoucí, pokud jste již publikovali předchozí verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="eee92-151">This is desirable if you have already published a previous version of your app.</span></span> <span data-ttu-id="eee92-152">Nechcete, aby se telemetrie z vaší relace ladění promíchala s telemetrií na portálu služby Application Insights z publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="eee92-152">You don't want the telemetry from your debugging sessions to be mixed up with the telemetry on the Application Insights portal from the published app.</span></span>

<span data-ttu-id="eee92-153">Je také užitečné, pokud máte některou [vlastní telemetrii](app-insights-api-custom-events-metrics.md), kterou chcete ladit před odesláním telemetrie na portál.</span><span class="sxs-lookup"><span data-stu-id="eee92-153">It's also useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want to debug before sending telemetry to the portal.</span></span>

* <span data-ttu-id="eee92-154">*Napřed jsem službu Application Insights plně nakonfiguroval/a tak, aby posílala telemetrické údaje na portál. Ale teď chci telemetrické údaje zobrazovat jen v sadě Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="eee92-154">*At first, I fully configured Application Insights to send telemetry to the portal. But now I'd like to see the telemetry only in Visual Studio.*</span></span>
  
  * <span data-ttu-id="eee92-155">V okně hledání nastavení je možnost vyhledávání místní diagnostiky i v případě, že vaše aplikace odesílá telemetrii na portál.</span><span class="sxs-lookup"><span data-stu-id="eee92-155">In the Search window's Settings, there's an option to search local diagnostics even if your app sends telemetry to the portal.</span></span>
  * <span data-ttu-id="eee92-156">Odesílání telemetrie na portál zastavíte okomentováním řádku `<instrumentationkey>...` ze souboru ApplicationInsights.config. Jakmile budete připraveni k opětovnému odeslání telemetrie na portál, komentář zrušte.</span><span class="sxs-lookup"><span data-stu-id="eee92-156">To stop telemetry being sent to the portal, comment out the line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready to send telemetry to the portal again, uncomment it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="eee92-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eee92-157">Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="eee92-158">**[Přidání dalších dat](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="eee92-158">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="eee92-159">Sledování využití, dostupnosti, závislostí, výjimek.</span><span class="sxs-lookup"><span data-stu-id="eee92-159">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="eee92-160">Integrujte trasování z rozhraní protokolování.</span><span class="sxs-lookup"><span data-stu-id="eee92-160">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="eee92-161">Zapisuje vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="eee92-161">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| <span data-ttu-id="eee92-163">**[Práce s portálem Application Insights](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="eee92-163">**[Working with the Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="eee92-164">Zobrazit řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a exportovaný telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="eee92-164">View dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and exported telemetry data.</span></span> |![Visual Studio](./media/app-insights-visual-studio/62.png) |

