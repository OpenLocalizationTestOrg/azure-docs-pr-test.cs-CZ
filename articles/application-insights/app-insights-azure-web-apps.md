---
title: "aaaMonitor službě Azure web app výkonu | Microsoft Docs"
description: "Monitorování výkonu webových aplikací Azure. Můžete vytvářet grafy zatížení a doby odezvy nebo informací o závislosti a nastavovat upozornění týkající se výkonu."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a><span data-ttu-id="b876b-104">Monitorování výkonu webových aplikací Azure</span><span class="sxs-lookup"><span data-stu-id="b876b-104">Monitor Azure web app performance</span></span>
<span data-ttu-id="b876b-105">V hello [portálu Azure](https://portal.azure.com) můžete nastavit monitorování výkonu aplikací na vašem [webové aplikace Azure](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b876b-105">In hello [Azure Portal](https://portal.azure.com) you can set up application performance monitoring for your [Azure web apps](../app-service-web/app-service-web-overview.md).</span></span> <span data-ttu-id="b876b-106">[Azure Application Insights](app-insights-overview.md) instruments telemetrie toosend aplikace o jeho aktivity toohello služby Application Insights, kde je uložený a analyzovat.</span><span class="sxs-lookup"><span data-stu-id="b876b-106">[Azure Application Insights](app-insights-overview.md) instruments your app toosend telemetry about its activities toohello Application Insights service, where it is stored and analyzed.</span></span> <span data-ttu-id="b876b-107">Zde je možné metriky grafů a vyhledávací nástroje toohelp diagnostikovat problémy, výkon a vyhodnocení využití.</span><span class="sxs-lookup"><span data-stu-id="b876b-107">There, metric charts and search tools can be used toohelp diagnose issues, improve performance, and assess usage.</span></span>

## <a name="run-time-or-build-time"></a><span data-ttu-id="b876b-108">Za běhu nebo při sestavení</span><span class="sxs-lookup"><span data-stu-id="b876b-108">Run time or build time</span></span>
<span data-ttu-id="b876b-109">Můžete nakonfigurovat monitorování instrumentaci aplikace hello v některém ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="b876b-109">You can configure monitoring by instrumenting hello app in either of two ways:</span></span>

* <span data-ttu-id="b876b-110">**Za běhu** – Můžete vybrat rozšíření pro monitorování výkonu, když je webová aplikace již v živém provozu.</span><span class="sxs-lookup"><span data-stu-id="b876b-110">**Run-time** - You can select a performance monitoring extension when your web app is already live.</span></span> <span data-ttu-id="b876b-111">Není nutné toorebuild, nebo znovu nainstalujte vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b876b-111">It isn't necessary toorebuild or re-install your app.</span></span> <span data-ttu-id="b876b-112">Obdržíte standardní sadu balíčků, které monitorují dobu odezvy, úspěšnost, výjimky, závislosti a další.</span><span class="sxs-lookup"><span data-stu-id="b876b-112">You get a standard set of packages that monitor response times, success rates, exceptions, dependencies, and so on.</span></span> 
* <span data-ttu-id="b876b-113">**Při sestavení** – Do aplikace můžete balíček nainstalovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="b876b-113">**Build time** - You can install a package in your app in development.</span></span> <span data-ttu-id="b876b-114">Tato možnost nabízí větší variabilitu.</span><span class="sxs-lookup"><span data-stu-id="b876b-114">This option is more versatile.</span></span> <span data-ttu-id="b876b-115">V toohello přidání stejné standardní balíčků, můžete napsat kód toocustomize hello telemetrie nebo toosend vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="b876b-115">In addition toohello same standard packages, you can write code toocustomize hello telemetry or toosend your own telemetry.</span></span> <span data-ttu-id="b876b-116">Můžete protokolovat konkrétní aktivity nebo záznam událostí podle toohello sémantiku vaší domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="b876b-116">You can log specific activities or record events according toohello semantics of your app domain.</span></span> 

## <a name="run-time-instrumentation-with-application-insights"></a><span data-ttu-id="b876b-117">Použití za běhu s Application Insights</span><span class="sxs-lookup"><span data-stu-id="b876b-117">Run time instrumentation with Application Insights</span></span>
<span data-ttu-id="b876b-118">Pokud už webovou aplikaci v Azure spouštíte, máte již monitorování do jisté míry k dispozici: můžete monitorovat požadavky a chybovost.</span><span class="sxs-lookup"><span data-stu-id="b876b-118">If you're already running a web app in Azure, you already get some monitoring: request and error rates.</span></span> <span data-ttu-id="b876b-119">Přidejte Application Insights tooget další, například dobu odezvy, monitorování toodependencies volání, inteligentní detekce a hello dotazovacího jazyka pro efektivní analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="b876b-119">Add Application Insights tooget more, such as response times, monitoring calls toodependencies, smart detection, and hello powerful Log Analytics query language.</span></span> 

1. <span data-ttu-id="b876b-120">**Vyberte Application Insights** Azure ovládacího panelu hello pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b876b-120">**Select Application Insights** in hello Azure control panel for your web app.</span></span>
   
    ![V části Monitorování zvolte Application Insights.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * <span data-ttu-id="b876b-122">Zvolte toocreate nový prostředek, pokud jste již nastavili prostředek Application Insights pro tuto aplikaci pomocí jiné trasy.</span><span class="sxs-lookup"><span data-stu-id="b876b-122">Choose toocreate a new resource, unless you already set up an Application Insights resource for this app by another route.</span></span>
2. <span data-ttu-id="b876b-123">Po instalaci Application Insights **webovou aplikaci používejte**.</span><span class="sxs-lookup"><span data-stu-id="b876b-123">**Instrument your web app** after Application Insights has been installed.</span></span> 
   
    ![Používejte webovou aplikaci.](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   <span data-ttu-id="b876b-125">**Povolte monitorování na straně klienta** pro zobrazení stránek a telemetrii uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b876b-125">**Enable client side monitoring** for page view and user telemetry.</span></span>

   * <span data-ttu-id="b876b-126">Klikněte na Nastavení > Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b876b-126">Select Settings > Application Settings</span></span>
   * <span data-ttu-id="b876b-127">V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b876b-127">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="b876b-128">Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="b876b-128">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="b876b-129">Hodnota: `true`</span><span class="sxs-lookup"><span data-stu-id="b876b-129">Value: `true`</span></span>
   * <span data-ttu-id="b876b-130">**Uložit** hello nastavení a **restartujte** vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b876b-130">**Save** hello settings and **Restart** your app.</span></span>
3. <span data-ttu-id="b876b-131">**Monitorujte aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="b876b-131">**Monitor your app**.</span></span>  <span data-ttu-id="b876b-132">[Expore hello data](#explore-the-data).</span><span class="sxs-lookup"><span data-stu-id="b876b-132">[Expore hello data](#explore-the-data).</span></span>

<span data-ttu-id="b876b-133">Pokud chcete později, můžete vytvořit aplikace hello pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b876b-133">Later, you can build hello app with Application Insights if you want.</span></span>

<span data-ttu-id="b876b-134">*Jak odebrat Application Insights, nebo přepnout toosending tooanother prostředků?*</span><span class="sxs-lookup"><span data-stu-id="b876b-134">*How do I remove Application Insights, or switch toosending tooanother resource?*</span></span>

* <span data-ttu-id="b876b-135">V Azure, otevřete hello webové aplikace ovládacího prvku okno a v části nástroje pro vývoj, otevřete **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="b876b-135">In Azure, open hello web app control blade, and under Development Tools, open **Extensions**.</span></span> <span data-ttu-id="b876b-136">Odstraňte rozšíření Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="b876b-136">Delete hello Application Insights extension.</span></span> <span data-ttu-id="b876b-137">Potom v části sledování, vyberte Application Insights a vytvořte nebo vyberte hello prostředek, který chcete.</span><span class="sxs-lookup"><span data-stu-id="b876b-137">Then under Monitoring, choose Application Insights and create or select hello resource you want.</span></span>

## <a name="build-hello-app-with-application-insights"></a><span data-ttu-id="b876b-138">Sestavení aplikace hello s Application Insights</span><span class="sxs-lookup"><span data-stu-id="b876b-138">Build hello app with Application Insights</span></span>
<span data-ttu-id="b876b-139">Application Insights může poskytovat podrobnější telemetrie po nainstalování sady SDK do příslušné aplikace.</span><span class="sxs-lookup"><span data-stu-id="b876b-139">Application Insights can provide more detailed telemetry by installing an SDK into your app.</span></span> <span data-ttu-id="b876b-140">Konkrétně je možné shromažďovat protokoly trasování, [psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)a získávat podrobnější sestavy výjimek.</span><span class="sxs-lookup"><span data-stu-id="b876b-140">In particular, you can collect trace logs, [write custom telemetry](app-insights-api-custom-events-metrics.md), and get more detailed exception reports.</span></span>

1. <span data-ttu-id="b876b-141">**V sadě Visual Studio** (2013 s aktualizací 2 nebo novější) nakonfigurujte Application Insights pro svůj projekt.</span><span class="sxs-lookup"><span data-stu-id="b876b-141">**In Visual Studio** (2013 update 2 or later), configure Application Insights for your project.</span></span>

    <span data-ttu-id="b876b-142">Klikněte pravým tlačítkem na hello webový projekt a vyberte **Přidat > Application Insights** nebo **konfigurovat Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="b876b-142">Right-click hello web project, and select **Add > Application Insights** or **Configure Application Insights**.</span></span>
   
    ![Klikněte pravým tlačítkem na hello webový projekt a zvolte Přidat nebo konfigurovat Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    <span data-ttu-id="b876b-144">Zobrazení dotazu toosign v, použijte hello přihlašovací údaje k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b876b-144">If you're asked toosign in, use hello credentials for your Azure account.</span></span>
   
    <span data-ttu-id="b876b-145">operace Hello má dva důsledky:</span><span class="sxs-lookup"><span data-stu-id="b876b-145">hello operation has two effects:</span></span>
   
   1. <span data-ttu-id="b876b-146">Vytvoří prostředek Application Insights v Azure, kde se ukládají, analyzují a zobrazují telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="b876b-146">Creates an Application Insights resource in Azure, where telemetry is stored, analyzed and displayed.</span></span>
   2. <span data-ttu-id="b876b-147">Přidá hello Application Insights NuGet balíček tooyour kódu (pokud existuje již není) a nakonfiguruje jej toosend telemetrie toohello prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b876b-147">Adds hello Application Insights NuGet package tooyour code (if it isn't there already), and configures it toosend telemetry toohello Azure resource.</span></span>
2. <span data-ttu-id="b876b-148">**Testování hello telemetrie** ve spuštěné aplikaci hello ve vývojovém počítači (F5).</span><span class="sxs-lookup"><span data-stu-id="b876b-148">**Test hello telemetry** by running hello app in your development machine (F5).</span></span>
3. <span data-ttu-id="b876b-149">**Publikování aplikace hello** tooAzure v hello obvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="b876b-149">**Publish hello app** tooAzure in hello usual way.</span></span> 

<span data-ttu-id="b876b-150">*Jak lze přepnout toosending tooa jiný prostředek služby Application Insights?*</span><span class="sxs-lookup"><span data-stu-id="b876b-150">*How do I switch toosending tooa different Application Insights resource?*</span></span>

* <span data-ttu-id="b876b-151">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello, zvolte **konfigurovat Application Insights** a vyberte prostředek hello chcete.</span><span class="sxs-lookup"><span data-stu-id="b876b-151">In Visual Studio, right-click hello project, choose **Configure Application Insights** and choose hello resource you want.</span></span> <span data-ttu-id="b876b-152">Získáte možnost toocreate hello nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="b876b-152">You get hello option toocreate a new resource.</span></span> <span data-ttu-id="b876b-153">Proveďte opětné sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="b876b-153">Rebuild and redeploy.</span></span>

## <a name="explore-hello-data"></a><span data-ttu-id="b876b-154">Prozkoumejte hello data</span><span class="sxs-lookup"><span data-stu-id="b876b-154">Explore hello data</span></span>
1. <span data-ttu-id="b876b-155">V okně hello Application Insights z vaší webové aplikace ovládacích panelů, uvidíte metriky za provozu, který ukazuje žádostí a selhání v rámci druhý nebo dvě z nich ke kterým dochází.</span><span class="sxs-lookup"><span data-stu-id="b876b-155">On hello Application Insights blade of your web app control panel, you see Live Metrics, which shows requests and failures within a second or two of them occurring.</span></span> <span data-ttu-id="b876b-156">Toto zobrazení je velmi užitečné, pokud aplikaci znovu publikujete – okamžitě uvidíte veškeré případné problémy.</span><span class="sxs-lookup"><span data-stu-id="b876b-156">It's very useful display when you're republishing your app - you can see any problems immediately.</span></span>
2. <span data-ttu-id="b876b-157">Proklikejte se prostřednictvím toohello úplné prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b876b-157">Click through toohello full Application Insights resource.</span></span>

    ![Proklikejte se.](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    <span data-ttu-id="b876b-159">Můžete tam přejít i přímo z navigace pro prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="b876b-159">You can also go there either directly from Azure resource navigation.</span></span>

1. <span data-ttu-id="b876b-160">Proklikejte se prostřednictvím jakékoli grafu tooget podrobněji:</span><span class="sxs-lookup"><span data-stu-id="b876b-160">Click through any chart tooget more detail:</span></span>
   
    ![V okně Přehled hello Application Insights klikněte na graf](./media/app-insights-azure-web-apps/07-dependency.png)
   
    <span data-ttu-id="b876b-162">Můžete [přizpůsobit okna metrik](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="b876b-162">You can [customize metrics blades](app-insights-metrics-explorer.md).</span></span>
2. <span data-ttu-id="b876b-163">Klikněte na další toosee jednotlivé události a jejich vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b876b-163">Click through further toosee individual events and their properties:</span></span>
   
    ![Klikněte na tlačítko tooopen typ události vyhledávání filtrováno typu](./media/app-insights-azure-web-apps/08-requests.png)
   
    <span data-ttu-id="b876b-165">Všimněte si, hello "..." odkaz tooopen všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b876b-165">Notice hello "..." link tooopen all properties.</span></span>
   
    <span data-ttu-id="b876b-166">Můžete [přizpůsobovat hledání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b876b-166">You can [customize searches](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="b876b-167">Pro účinnější hledání přes telemetrie, použijte hello [analýzy protokolů dotazu jazyka](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="b876b-167">For more powerful searches over your telemetry, use hello [Log Analytics query language](app-insights-analytics-tour.md).</span></span>

## <a name="more-telemetry"></a><span data-ttu-id="b876b-168">Další telemetrická data</span><span class="sxs-lookup"><span data-stu-id="b876b-168">More telemetry</span></span>

* [<span data-ttu-id="b876b-169">Data načítání webové stránky</span><span class="sxs-lookup"><span data-stu-id="b876b-169">Web page load data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="b876b-170">Vlastní telemetrická data</span><span class="sxs-lookup"><span data-stu-id="b876b-170">Custom telemetry</span></span>](app-insights-api-custom-events-metrics.md)

## <a name="video"></a><span data-ttu-id="b876b-171">Video</span><span class="sxs-lookup"><span data-stu-id="b876b-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="b876b-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b876b-172">Next steps</span></span>
* <span data-ttu-id="b876b-173">[Spustit hello profileru ve vaší živé aplikaci](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="b876b-173">[Run hello profiler on your live app](app-insights-profiler.md).</span></span>
* <span data-ttu-id="b876b-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – monitorujte službu Azure Functions pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b876b-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitor Azure Functions with Application Insights</span></span>
* <span data-ttu-id="b876b-175">[Povolit Azure diagnostics](app-insights-azure-diagnostics.md) toobe odeslané tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="b876b-175">[Enable Azure diagnostics](app-insights-azure-diagnostics.md) toobe sent tooApplication Insights.</span></span>
* <span data-ttu-id="b876b-176">[Monitorování stavu metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="b876b-176">[Monitor service health metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
* <span data-ttu-id="b876b-177">[Přijímejte oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b876b-177">[Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) whenever operational events happen or metrics cross a threshold.</span></span>
* <span data-ttu-id="b876b-178">Použití [Application Insights pro aplikace JavaScript a webové stránky](app-insights-javascript.md) tooget klienta telemetrie z hello prohlížečů, které navštívit webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="b876b-178">Use [Application Insights for JavaScript apps and web pages](app-insights-javascript.md) tooget client telemetry from hello browsers that visit a web page.</span></span>
* <span data-ttu-id="b876b-179">[Nastavit testy dostupnosti webu](app-insights-monitor-web-app-availability.md) toobe zobrazí výstraha, pokud váš webový server je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="b876b-179">[Set up Availability web tests](app-insights-monitor-web-app-availability.md) toobe alerted if your site is down.</span></span>

