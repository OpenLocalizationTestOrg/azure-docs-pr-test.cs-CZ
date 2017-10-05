---
title: "Monitorování výkonu webových aplikací Azure | Dokumentace Microsoftu"
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
ms.openlocfilehash: f2bbadfbcb93873ed910aeff050bd6896e2e8fec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-azure-web-app-performance"></a><span data-ttu-id="210e1-104">Monitorování výkonu webových aplikací Azure</span><span class="sxs-lookup"><span data-stu-id="210e1-104">Monitor Azure web app performance</span></span>
<span data-ttu-id="210e1-105">Na webu [Azure Portal](https://portal.azure.com) můžete pro své [webové aplikace Azure](../app-service-web/app-service-web-overview.md) nastavit monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="210e1-105">In the [Azure Portal](https://portal.azure.com) you can set up application performance monitoring for your [Azure web apps](../app-service-web/app-service-web-overview.md).</span></span> <span data-ttu-id="210e1-106">[Azure Application Insights](app-insights-overview.md) využívá vaši aplikaci k odesílání telemetrických dat o jejích aktivitách do služby Application Insights, kde se ukládají a analyzují.</span><span class="sxs-lookup"><span data-stu-id="210e1-106">[Azure Application Insights](app-insights-overview.md) instruments your app to send telemetry about its activities to the Application Insights service, where it is stored and analyzed.</span></span> <span data-ttu-id="210e1-107">Tam lze grafy metrik a vyhledávací nástroje použít při řešení problémů s diagnostikou, při zvyšování výkonu a při vyhodnocování využití.</span><span class="sxs-lookup"><span data-stu-id="210e1-107">There, metric charts and search tools can be used to help diagnose issues, improve performance, and assess usage.</span></span>

## <a name="run-time-or-build-time"></a><span data-ttu-id="210e1-108">Za běhu nebo při sestavení</span><span class="sxs-lookup"><span data-stu-id="210e1-108">Run time or build time</span></span>
<span data-ttu-id="210e1-109">Monitorování s použitím aplikace můžete konfigurovat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="210e1-109">You can configure monitoring by instrumenting the app in either of two ways:</span></span>

* <span data-ttu-id="210e1-110">**Za běhu** – Můžete vybrat rozšíření pro monitorování výkonu, když je webová aplikace již v živém provozu.</span><span class="sxs-lookup"><span data-stu-id="210e1-110">**Run-time** - You can select a performance monitoring extension when your web app is already live.</span></span> <span data-ttu-id="210e1-111">Aplikaci není třeba znovu sestavit ani instalovat.</span><span class="sxs-lookup"><span data-stu-id="210e1-111">It isn't necessary to rebuild or re-install your app.</span></span> <span data-ttu-id="210e1-112">Obdržíte standardní sadu balíčků, které monitorují dobu odezvy, úspěšnost, výjimky, závislosti a další.</span><span class="sxs-lookup"><span data-stu-id="210e1-112">You get a standard set of packages that monitor response times, success rates, exceptions, dependencies, and so on.</span></span> 
* <span data-ttu-id="210e1-113">**Při sestavení** – Do aplikace můžete balíček nainstalovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="210e1-113">**Build time** - You can install a package in your app in development.</span></span> <span data-ttu-id="210e1-114">Tato možnost nabízí větší variabilitu.</span><span class="sxs-lookup"><span data-stu-id="210e1-114">This option is more versatile.</span></span> <span data-ttu-id="210e1-115">Kromě stejných standardních balíčků můžete napsat kód pro přizpůsobení telemetrických dat nebo pro odesílání vlastních telemetrických dat.</span><span class="sxs-lookup"><span data-stu-id="210e1-115">In addition to the same standard packages, you can write code to customize the telemetry or to send your own telemetry.</span></span> <span data-ttu-id="210e1-116">Můžete protokolovat konkrétní aktivity nebo zaznamenávat události podle sémantiky domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="210e1-116">You can log specific activities or record events according to the semantics of your app domain.</span></span> 

## <a name="run-time-instrumentation-with-application-insights"></a><span data-ttu-id="210e1-117">Použití za běhu s Application Insights</span><span class="sxs-lookup"><span data-stu-id="210e1-117">Run time instrumentation with Application Insights</span></span>
<span data-ttu-id="210e1-118">Pokud už webovou aplikaci v Azure spouštíte, máte již monitorování do jisté míry k dispozici: můžete monitorovat požadavky a chybovost.</span><span class="sxs-lookup"><span data-stu-id="210e1-118">If you're already running a web app in Azure, you already get some monitoring: request and error rates.</span></span> <span data-ttu-id="210e1-119">Přidejte Application Insights a získejte další možnosti, například zaznamenávání doby odezvy, monitorování volání závislostí, inteligentní detekci a výkonný dotazovací jazyk Log Analytics.</span><span class="sxs-lookup"><span data-stu-id="210e1-119">Add Application Insights to get more, such as response times, monitoring calls to dependencies, smart detection, and the powerful Log Analytics query language.</span></span> 

1. <span data-ttu-id="210e1-120">**Vyberte Application Insights** pro svou webovou aplikaci na ovládacím panelu Azure.</span><span class="sxs-lookup"><span data-stu-id="210e1-120">**Select Application Insights** in the Azure control panel for your web app.</span></span>
   
    ![V části Monitorování zvolte Application Insights.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * <span data-ttu-id="210e1-122">Zvolte možnost vytvoření nového prostředku, pokud jste ještě prostředek Application Insights pro tuto aplikaci nenastavili jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="210e1-122">Choose to create a new resource, unless you already set up an Application Insights resource for this app by another route.</span></span>
2. <span data-ttu-id="210e1-123">Po instalaci Application Insights **webovou aplikaci používejte**.</span><span class="sxs-lookup"><span data-stu-id="210e1-123">**Instrument your web app** after Application Insights has been installed.</span></span> 
   
    ![Používejte webovou aplikaci.](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   <span data-ttu-id="210e1-125">**Povolte monitorování na straně klienta** pro zobrazení stránek a telemetrii uživatelů.</span><span class="sxs-lookup"><span data-stu-id="210e1-125">**Enable client side monitoring** for page view and user telemetry.</span></span>

   * <span data-ttu-id="210e1-126">Klikněte na Nastavení > Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="210e1-126">Select Settings > Application Settings</span></span>
   * <span data-ttu-id="210e1-127">V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty:</span><span class="sxs-lookup"><span data-stu-id="210e1-127">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="210e1-128">Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="210e1-128">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="210e1-129">Hodnota: `true`</span><span class="sxs-lookup"><span data-stu-id="210e1-129">Value: `true`</span></span>
   * <span data-ttu-id="210e1-130">Kliknutím na **Uložit** uložte nastavení a kliknutím na **Restartovat** restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="210e1-130">**Save** the settings and **Restart** your app.</span></span>
3. <span data-ttu-id="210e1-131">**Monitorujte aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="210e1-131">**Monitor your app**.</span></span>  <span data-ttu-id="210e1-132">[Prozkoumejte data](#explore-the-data).</span><span class="sxs-lookup"><span data-stu-id="210e1-132">[Expore the data](#explore-the-data).</span></span>

<span data-ttu-id="210e1-133">Později můžete pomocí Application Insights aplikaci sestavit, pokud budete chtít.</span><span class="sxs-lookup"><span data-stu-id="210e1-133">Later, you can build the app with Application Insights if you want.</span></span>

<span data-ttu-id="210e1-134">*Jak lze odebrat Application Insights nebo přepnout na odesílání do jiného prostředku?*</span><span class="sxs-lookup"><span data-stu-id="210e1-134">*How do I remove Application Insights, or switch to sending to another resource?*</span></span>

* <span data-ttu-id="210e1-135">V Azure otevřete okno s ovládacími prvky webové aplikace a v části Vývojové nástroje otevřete **Rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="210e1-135">In Azure, open the web app control blade, and under Development Tools, open **Extensions**.</span></span> <span data-ttu-id="210e1-136">Odstraňte rozšíření Application Insights.</span><span class="sxs-lookup"><span data-stu-id="210e1-136">Delete the Application Insights extension.</span></span> <span data-ttu-id="210e1-137">Potom v části Monitorování zvolte Application Insights a vytvořte nebo vyberte požadovaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="210e1-137">Then under Monitoring, choose Application Insights and create or select the resource you want.</span></span>

## <a name="build-the-app-with-application-insights"></a><span data-ttu-id="210e1-138">Sestavení aplikace s použitím Application Insights</span><span class="sxs-lookup"><span data-stu-id="210e1-138">Build the app with Application Insights</span></span>
<span data-ttu-id="210e1-139">Application Insights může poskytovat podrobnější telemetrie po nainstalování sady SDK do příslušné aplikace.</span><span class="sxs-lookup"><span data-stu-id="210e1-139">Application Insights can provide more detailed telemetry by installing an SDK into your app.</span></span> <span data-ttu-id="210e1-140">Konkrétně je možné shromažďovat protokoly trasování, [psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)a získávat podrobnější sestavy výjimek.</span><span class="sxs-lookup"><span data-stu-id="210e1-140">In particular, you can collect trace logs, [write custom telemetry](app-insights-api-custom-events-metrics.md), and get more detailed exception reports.</span></span>

1. <span data-ttu-id="210e1-141">**V sadě Visual Studio** (2013 s aktualizací 2 nebo novější) nakonfigurujte Application Insights pro svůj projekt.</span><span class="sxs-lookup"><span data-stu-id="210e1-141">**In Visual Studio** (2013 update 2 or later), configure Application Insights for your project.</span></span>

    <span data-ttu-id="210e1-142">Klikněte pravým tlačítkem myši na webový projekt a vyberte **Přidat > Application Insights** nebo **Nakonfigurovat Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="210e1-142">Right-click the web project, and select **Add > Application Insights** or **Configure Application Insights**.</span></span>
   
    ![Klikněte pravým tlačítkem myši na webový projekt a zvolte Přidat nebo Nakonfigurovat Application Insights.](./media/app-insights-azure-web-apps/03-add.png)
   
    <span data-ttu-id="210e1-144">Pokud budete vyzváni k přihlášení, použijte přihlašovací údaje ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="210e1-144">If you're asked to sign in, use the credentials for your Azure account.</span></span>
   
    <span data-ttu-id="210e1-145">Operace má dva důsledky:</span><span class="sxs-lookup"><span data-stu-id="210e1-145">The operation has two effects:</span></span>
   
   1. <span data-ttu-id="210e1-146">Vytvoří prostředek Application Insights v Azure, kde se ukládají, analyzují a zobrazují telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="210e1-146">Creates an Application Insights resource in Azure, where telemetry is stored, analyzed and displayed.</span></span>
   2. <span data-ttu-id="210e1-147">Přidá do kódu balíček NuGet Application Insights (pokud tam ještě není) a nakonfiguruje ho tak, aby odesílal telemetrická data do příslušného prostředku Azure.</span><span class="sxs-lookup"><span data-stu-id="210e1-147">Adds the Application Insights NuGet package to your code (if it isn't there already), and configures it to send telemetry to the Azure resource.</span></span>
2. <span data-ttu-id="210e1-148">**Otestujte telemetrická data** spuštěním aplikace v počítači pro vývoj (F5).</span><span class="sxs-lookup"><span data-stu-id="210e1-148">**Test the telemetry** by running the app in your development machine (F5).</span></span>
3. <span data-ttu-id="210e1-149">**Publikujte aplikaci** v Azure obvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="210e1-149">**Publish the app** to Azure in the usual way.</span></span> 

<span data-ttu-id="210e1-150">*Jak lze přepnout na odesílání do jiného prostředku Application Insights?*</span><span class="sxs-lookup"><span data-stu-id="210e1-150">*How do I switch to sending to a different Application Insights resource?*</span></span>

* <span data-ttu-id="210e1-151">V sadě Visual Studio klikněte pravým tlačítkem na projekt, zvolte **Nakonfigurovat Application Insights** a zvolte požadovaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="210e1-151">In Visual Studio, right-click the project, choose **Configure Application Insights** and choose the resource you want.</span></span> <span data-ttu-id="210e1-152">Budete mít možnost vytvořit nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="210e1-152">You get the option to create a new resource.</span></span> <span data-ttu-id="210e1-153">Proveďte opětné sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="210e1-153">Rebuild and redeploy.</span></span>

## <a name="explore-the-data"></a><span data-ttu-id="210e1-154">Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="210e1-154">Explore the data</span></span>
1. <span data-ttu-id="210e1-155">V okně Application Insights na ovládacím panelu webové aplikace se zobrazuje část Live Metrics, která obsahuje požadavky a selhání během sekundy či dvou od jejich výskytu.</span><span class="sxs-lookup"><span data-stu-id="210e1-155">On the Application Insights blade of your web app control panel, you see Live Metrics, which shows requests and failures within a second or two of them occurring.</span></span> <span data-ttu-id="210e1-156">Toto zobrazení je velmi užitečné, pokud aplikaci znovu publikujete – okamžitě uvidíte veškeré případné problémy.</span><span class="sxs-lookup"><span data-stu-id="210e1-156">It's very useful display when you're republishing your app - you can see any problems immediately.</span></span>
2. <span data-ttu-id="210e1-157">Proklikejte se k úplnému prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="210e1-157">Click through to the full Application Insights resource.</span></span>

    ![Proklikejte se.](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    <span data-ttu-id="210e1-159">Můžete tam přejít i přímo z navigace pro prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="210e1-159">You can also go there either directly from Azure resource navigation.</span></span>

1. <span data-ttu-id="210e1-160">Proklikáním se kterýmkoli grafem přejdete k zobrazení podrobnějších údajů:</span><span class="sxs-lookup"><span data-stu-id="210e1-160">Click through any chart to get more detail:</span></span>
   
    ![V okně s přehledem Application Insights klikněte na graf.](./media/app-insights-azure-web-apps/07-dependency.png)
   
    <span data-ttu-id="210e1-162">Můžete [přizpůsobit okna metrik](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="210e1-162">You can [customize metrics blades](app-insights-metrics-explorer.md).</span></span>
2. <span data-ttu-id="210e1-163">Další proklikáním se můžete zobrazit jednotlivé události a jejich vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="210e1-163">Click through further to see individual events and their properties:</span></span>
   
    ![Kliknutím na typ události otevřete vyhledávání filtrované podle příslušného typu.](./media/app-insights-azure-web-apps/08-requests.png)
   
    <span data-ttu-id="210e1-165">Všimněte si, že odkaz „...“ otevře všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="210e1-165">Notice the "..." link to open all properties.</span></span>
   
    <span data-ttu-id="210e1-166">Můžete [přizpůsobovat hledání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="210e1-166">You can [customize searches](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="210e1-167">Pokud chcete v telemetrických datech provádět výkonnější hledání, použijte [dotazovací jazyk Log Analytics](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="210e1-167">For more powerful searches over your telemetry, use the [Log Analytics query language](app-insights-analytics-tour.md).</span></span>

## <a name="more-telemetry"></a><span data-ttu-id="210e1-168">Další telemetrická data</span><span class="sxs-lookup"><span data-stu-id="210e1-168">More telemetry</span></span>

* [<span data-ttu-id="210e1-169">Data načítání webové stránky</span><span class="sxs-lookup"><span data-stu-id="210e1-169">Web page load data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="210e1-170">Vlastní telemetrická data</span><span class="sxs-lookup"><span data-stu-id="210e1-170">Custom telemetry</span></span>](app-insights-api-custom-events-metrics.md)

## <a name="video"></a><span data-ttu-id="210e1-171">Video</span><span class="sxs-lookup"><span data-stu-id="210e1-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="210e1-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="210e1-172">Next steps</span></span>
* <span data-ttu-id="210e1-173">[Spusťte profiler v živé aplikaci](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="210e1-173">[Run the profiler on your live app](app-insights-profiler.md).</span></span>
* <span data-ttu-id="210e1-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – monitorujte službu Azure Functions pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="210e1-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitor Azure Functions with Application Insights</span></span>
* <span data-ttu-id="210e1-175">[Povolte odesílání diagnostiky Azure](app-insights-azure-diagnostics.md) do Application Insights.</span><span class="sxs-lookup"><span data-stu-id="210e1-175">[Enable Azure diagnostics](app-insights-azure-diagnostics.md) to be sent to Application Insights.</span></span>
* <span data-ttu-id="210e1-176">[Monitorujte metriky stavu služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md), abyste zajistili dostupnost služby a její schopnost dobře reagovat.</span><span class="sxs-lookup"><span data-stu-id="210e1-176">[Monitor service health metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
* <span data-ttu-id="210e1-177">[Přijímejte oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="210e1-177">[Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) whenever operational events happen or metrics cross a threshold.</span></span>
* <span data-ttu-id="210e1-178">Použitím [Application Insights pro aplikace JavaScript a webové stránky](app-insights-javascript.md) získávejte telemetrické údaje klienta z prohlížečů, které webovou stránky navštíví.</span><span class="sxs-lookup"><span data-stu-id="210e1-178">Use [Application Insights for JavaScript apps and web pages](app-insights-javascript.md) to get client telemetry from the browsers that visit a web page.</span></span>
* <span data-ttu-id="210e1-179">[Nastavte testy dostupnosti webu](app-insights-monitor-web-app-availability.md) tak, aby se aktivovaly výstrahy, pokud je webový server mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="210e1-179">[Set up Availability web tests](app-insights-monitor-web-app-availability.md) to be alerted if your site is down.</span></span>

