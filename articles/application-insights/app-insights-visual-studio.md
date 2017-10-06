---
title: "aaaDebug aplikace pomocí služby Azure Application Insights v sadě Visual Studio | Microsoft Docs"
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
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a><span data-ttu-id="5691d-103">Ladění aplikací pomocí služby Azure Application Insights v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5691d-103">Debug your applications with Azure Application Insights in Visual Studio</span></span>
<span data-ttu-id="5691d-104">V sadě Visual Studio (2015 a novější) můžete analyzovat výkon a diagnostikovat problémy ve vaší webové aplikaci v ASP.NET během ladění i v produkčním prostředí pomocí telemetrie z [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5691d-104">In Visual Studio (2015 and later), you can analyze performance and diagnose issues in your ASP.NET web app both in debugging and in production, using telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="5691d-105">Pokud jste vytvořili webovou aplikaci ASP.NET pomocí Visual Studio 2017 nebo novější, už je hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="5691d-105">If you created your ASP.NET web app using Visual Studio 2017 or later, it already has hello Application Insights SDK.</span></span> <span data-ttu-id="5691d-106">Jinak, pokud jste to ještě neudělali, [přidat Application Insights tooyour aplikaci](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="5691d-106">Otherwise, if you haven't done so already, [add Application Insights tooyour app](app-insights-asp-net.md).</span></span>

<span data-ttu-id="5691d-107">toomonitor aplikace Pokud je v produkčním prostředí za provozu, můžete zobrazit v hello normálně telemetrie Application Insights hello [portál Azure](https://portal.azure.com), kde můžete nastavit upozornění a použít výkonných monitorovacích nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5691d-107">toomonitor your app when it's in live production, you normally view hello Application Insights telemetry in hello [Azure portal](https://portal.azure.com), where you can set alerts and apply powerful monitoring tools.</span></span> <span data-ttu-id="5691d-108">Ale pro ladění, můžete také vyhledat a analyzovat hello telemetrie v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5691d-108">But for debugging, you can also search and analyze hello telemetry in Visual Studio.</span></span> <span data-ttu-id="5691d-109">Telemetrie tooanalyze Visual Studio můžete použít z provozního webu i z ladění na vývojovém počítači běží.</span><span class="sxs-lookup"><span data-stu-id="5691d-109">You can use Visual Studio tooanalyze telemetry both from your production site and from debugging runs on your development machine.</span></span> <span data-ttu-id="5691d-110">V druhém případě hello můžete analyzovat ladění spustí i v případě, že jste zatím nenakonfigurovali hello SDK toosend telemetrie toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5691d-110">In hello latter case, you can analyze debugging runs even if you haven't yet configured hello SDK toosend telemetry toohello Azure portal.</span></span> 

## <span data-ttu-id="5691d-111"><a name="run"></a> Ladění projektu</span><span class="sxs-lookup"><span data-stu-id="5691d-111"><a name="run"></a> Debug your project</span></span>
<span data-ttu-id="5691d-112">Spusťte webovou aplikaci v režimu místního ladění pomocí klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="5691d-112">Run your web app in local debug mode by using F5.</span></span> <span data-ttu-id="5691d-113">Otevřete různé stránky toogenerate nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="5691d-113">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="5691d-114">V sadě Visual Studio zobrazí počet hello události, které byly zaprotokolovány modulem hello Application Insights ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="5691d-114">In Visual Studio, you see a count of hello events that have been logged by hello Application Insights module in your project.</span></span>

![V sadě Visual Studio zobrazí tlačítko Application Insights hello během ladění.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

<span data-ttu-id="5691d-116">Klikněte na toto tlačítko toosearch telemetrie.</span><span class="sxs-lookup"><span data-stu-id="5691d-116">Click this button toosearch your telemetry.</span></span> 

## <a name="application-insights-search"></a><span data-ttu-id="5691d-117">Hledání Application Insights</span><span class="sxs-lookup"><span data-stu-id="5691d-117">Application Insights search</span></span>
<span data-ttu-id="5691d-118">v okně hledání Application Insights Hello zobrazí události, které byly zaprotokolovány.</span><span class="sxs-lookup"><span data-stu-id="5691d-118">hello Application Insights Search window shows events that have been logged.</span></span> <span data-ttu-id="5691d-119">(Pokud jste přihlášeni tooAzure při nastavení Application Insights, můžete hledat hello stejné události v hello portálu Azure.)</span><span class="sxs-lookup"><span data-stu-id="5691d-119">(If you signed in tooAzure when you set up Application Insights, you can search hello same events in hello Azure portal.)</span></span>

![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, vyhledávání](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> <span data-ttu-id="5691d-121">Po vyberte nebo zrušte výběr filtry, klikněte na tlačítko Hledat hello na konci hello pole hledání text hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-121">After you select or deselect filters, click hello Search button at hello end of hello text search field.</span></span>
>

<span data-ttu-id="5691d-122">Hello textové vyhledávání funguje na všechna pole v událostech hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-122">hello free text search works on any fields in hello events.</span></span> <span data-ttu-id="5691d-123">Například vyhledejte část adresy URL hello stránky; nebo hello hodnotu vlastnosti, například města klienta; nebo určitá slova v protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="5691d-123">For example, search for part of hello URL of a page; or hello value of a property such as client city; or specific words in a trace log.</span></span>

<span data-ttu-id="5691d-124">Klikněte na možnost všechny události toosee podrobné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5691d-124">Click any event toosee its detailed properties.</span></span>

<span data-ttu-id="5691d-125">Pro žádosti o tooyour webovou aplikaci můžete kliknout na prostřednictvím toohello kódu.</span><span class="sxs-lookup"><span data-stu-id="5691d-125">For requests tooyour web app, you can click through toohello code.</span></span>

![V části Podrobnosti o žádosti klikněte na tlačítko prostřednictvím toohello kódu](./media/app-insights-visual-studio/31.png)

<span data-ttu-id="5691d-127">Můžete také otevřít související položky toohelp diagnostikovat neúspěšné požadavky nebo výjimky.</span><span class="sxs-lookup"><span data-stu-id="5691d-127">You can also open related items toohelp diagnose failed requests or exceptions.</span></span>

![V části Podrobnosti o žádosti přejděte dolů toorelated položky](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a><span data-ttu-id="5691d-129">Zobrazit výjimky a nezdařených požadavků.</span><span class="sxs-lookup"><span data-stu-id="5691d-129">View exceptions and failed requests</span></span>
<span data-ttu-id="5691d-130">Zobrazit sestavy výjimka v okně hledání hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-130">Exception reports show in hello Search window.</span></span> <span data-ttu-id="5691d-131">(V některé starší typy aplikace ASP.NET, máte příliš[nastavili monitorování výjimek](app-insights-asp-net-exceptions.md) toosee výjimky, které jsou zpracovávány hello framework.)</span><span class="sxs-lookup"><span data-stu-id="5691d-131">(In some older types of ASP.NET application, you have too[set up exception monitoring](app-insights-asp-net-exceptions.md) toosee exceptions that are handled by hello framework.)</span></span>

<span data-ttu-id="5691d-132">Klikněte na tlačítko k výjimce tooget trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="5691d-132">Click an exception tooget a stack trace.</span></span> <span data-ttu-id="5691d-133">Pokud je kód hello aplikace hello otevřete v sadě Visual Studio, můžete kliknutím z hello zásobník trasování toohello příslušný řádek kódu hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-133">If hello code of hello app is open in Visual Studio, you can click through from hello stack trace toohello relevant line of hello code.</span></span>

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a><span data-ttu-id="5691d-135">Zobrazit souhrny požadavku a výjimek v kódu hello</span><span class="sxs-lookup"><span data-stu-id="5691d-135">View request and exception summaries in hello code</span></span>
<span data-ttu-id="5691d-136">Hello řádku kódu přehledu výše každá metoda obslužná rutina se zobrazuje počet hello požadavky a výjimkami hello za posledních 24 h přihlášení pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5691d-136">In hello Code Lens line above each handler method, you see a count of hello requests and exceptions logged by Application Insights in hello past 24 h.</span></span>

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> <span data-ttu-id="5691d-138">Přehledu kódu zobrazuje pouze data Application Insights, pokud máte [nakonfigurované portálu služby Application Insights toohello telemetrie aplikace toosend](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="5691d-138">Code Lens shows Application Insights data only if you have [configured your app toosend telemetry toohello Application Insights portal](app-insights-asp-net.md).</span></span>
>

[<span data-ttu-id="5691d-139">Další informace o Application Insights v Code Lens</span><span class="sxs-lookup"><span data-stu-id="5691d-139">More about Application Insights in Code Lens</span></span>](app-insights-visual-studio-codelens.md)

## <a name="trends"></a><span data-ttu-id="5691d-140">Trendy</span><span class="sxs-lookup"><span data-stu-id="5691d-140">Trends</span></span>
<span data-ttu-id="5691d-141">Trendy představují nástroj pro vizualizaci chování aplikace v čase.</span><span class="sxs-lookup"><span data-stu-id="5691d-141">Trends is a tool for visualizing how your app behaves over time.</span></span> 

<span data-ttu-id="5691d-142">Zvolte **prozkoumat trendy Telemetrie** z tlačítka panelu nástrojů Application Insights hello nebo v okně hledání Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5691d-142">Choose **Explore Telemetry Trends** from hello Application Insights toolbar button or Application Insights Search window.</span></span> <span data-ttu-id="5691d-143">Vyberte jednu z pěti běžné dotazy tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5691d-143">Choose one of five common queries tooget started.</span></span> <span data-ttu-id="5691d-144">Na základě typů telemetrie, časových rozsahů a dalších vlastností můžete analyzovat různé datové sady.</span><span class="sxs-lookup"><span data-stu-id="5691d-144">You can analyze different datasets based on telemetry types, time ranges, and other properties.</span></span> 

<span data-ttu-id="5691d-145">toofind anomálie v datech, vyberte jednu z možností anomálií hello pod rozevíracího seznamu "Typ zobrazení" hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-145">toofind anomalies in your data, choose one of hello anomaly options under hello "View Type" dropdown.</span></span> <span data-ttu-id="5691d-146">Možnosti filtrování Hello v hello dolní části okna hello umožňují snadno toohone v na konkrétní podmnožiny telemetrie.</span><span class="sxs-lookup"><span data-stu-id="5691d-146">hello filtering options at hello bottom of hello window make it easy toohone in on specific subsets of your telemetry.</span></span>

![Trendy](./media/app-insights-visual-studio/51.png)

<span data-ttu-id="5691d-148">[Další informace o trendech](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="5691d-148">[More about Trends](app-insights-visual-studio-trends.md).</span></span>

## <a name="local-monitoring"></a><span data-ttu-id="5691d-149">Místní monitorování</span><span class="sxs-lookup"><span data-stu-id="5691d-149">Local monitoring</span></span>
<span data-ttu-id="5691d-150">(Z Visual Studio 2015 Update 2) Pokud jste nenakonfigurovali hello SDK toosend telemetrie toohello portál Application Insights (takže existuje v souboru ApplicationInsights.config nenachází žádný klíč instrumentace) zobrazí hello okno diagnostiky telemetrii z nejnovější relace ladění.</span><span class="sxs-lookup"><span data-stu-id="5691d-150">(From Visual Studio 2015 Update 2) If you haven't configured hello SDK toosend telemetry toohello Application Insights portal (so that there is no instrumentation key in ApplicationInsights.config) then hello diagnostics window displays telemetry from your latest debugging session.</span></span> 

<span data-ttu-id="5691d-151">Toto je žádoucí, pokud jste již publikovali předchozí verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="5691d-151">This is desirable if you have already published a previous version of your app.</span></span> <span data-ttu-id="5691d-152">Nechcete, aby hello telemetrie z vaší ladění toobe relací promíchají s telemetrií hello na hello portál Application Insights z publikované aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5691d-152">You don't want hello telemetry from your debugging sessions toobe mixed up with hello telemetry on hello Application Insights portal from hello published app.</span></span>

<span data-ttu-id="5691d-153">Je také užitečné, pokud máte některou [vlastní telemetrii](app-insights-api-custom-events-metrics.md) chcete toodebug před odesláním telemetrie toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="5691d-153">It's also useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want toodebug before sending telemetry toohello portal.</span></span>

* <span data-ttu-id="5691d-154">*Zpočátku jsem plně nakonfiguroval službu Application Insights toosend telemetrie toohello portálu. Ale nyní chcete toosee hello telemetrii pouze v sadě Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="5691d-154">*At first, I fully configured Application Insights toosend telemetry toohello portal. But now I'd like toosee hello telemetry only in Visual Studio.*</span></span>
  
  * <span data-ttu-id="5691d-155">V okně vyhledávání hello nastavení je možnost toosearch místní diagnostiky i v případě, že vaše aplikace odesílá telemetrii toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="5691d-155">In hello Search window's Settings, there's an option toosearch local diagnostics even if your app sends telemetry toohello portal.</span></span>
  * <span data-ttu-id="5691d-156">odesílání toohello portál, komentář hello řádku telemetrie toostop `<instrumentationkey>...` ze souboru ApplicationInsights.config. Až budete znovu připraven toosend telemetrie toohello portál, komentář zrušte.</span><span class="sxs-lookup"><span data-stu-id="5691d-156">toostop telemetry being sent toohello portal, comment out hello line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready toosend telemetry toohello portal again, uncomment it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5691d-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5691d-157">Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="5691d-158">**[Přidání dalších dat](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="5691d-158">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="5691d-159">Sledování využití, dostupnosti, závislostí, výjimek.</span><span class="sxs-lookup"><span data-stu-id="5691d-159">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="5691d-160">Integrujte trasování z rozhraní protokolování.</span><span class="sxs-lookup"><span data-stu-id="5691d-160">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="5691d-161">Zapisuje vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="5691d-161">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| <span data-ttu-id="5691d-163">**[Práce s Application Insights portál hello](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="5691d-163">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="5691d-164">Zobrazit řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a exportovaný telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="5691d-164">View dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and exported telemetry data.</span></span> |![Visual Studio](./media/app-insights-visual-studio/62.png) |

