---
title: "Monitorovat stav a využití pomocí Application Insights vaší aplikace"
description: "Začínáme s Application Insights. Analýza využití, dostupnosti a výkonu místního nebo aplikace Microsoft Azure."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 5b7b1f4a53cd2624ee8e2ab684ba6ba63252674f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="f5e92-104">Sledování výkonu webových aplikací</span><span class="sxs-lookup"><span data-stu-id="f5e92-104">Monitor performance in web applications</span></span>


<span data-ttu-id="f5e92-105">Ujistěte se, že aplikace je výkon na dobré a rychle zjistěte informace o případných selhání.</span><span class="sxs-lookup"><span data-stu-id="f5e92-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="f5e92-106">[Application Insights] [ start] bude vás informovat o žádné problémy s výkonem a výjimkami a můžete najít a diagnostikovat základní příčiny.</span><span class="sxs-lookup"><span data-stu-id="f5e92-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose the root causes.</span></span>

<span data-ttu-id="f5e92-107">Application Insights můžete monitorovat webové aplikace Java a ASP.NET a služby, služby WCF.</span><span class="sxs-lookup"><span data-stu-id="f5e92-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="f5e92-108">Mohou být hostovaná místně, virtuálními počítači, nebo jako weby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f5e92-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="f5e92-109">Na straně klienta může trvat Application Insights telemetrie z webových stránek a širokou škálu zařízení se systémy iOS, Android a aplikací pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="f5e92-109">On the client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="f5e92-110">Provedli jsme nové prostředí pro vyhledání pomalé, provádění stránky ve vaší webové aplikaci k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f5e92-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="f5e92-111">Pokud nemáte přístup k němu, povolení konfigurací možnosti preview s [Preview okno](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="f5e92-111">If you don't have access to it, enable it by configuring your preview options with the [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="f5e92-112">Přečtěte si informace o toto nové prostředí v [najít a opravit kritická místa výkonu s interaktivní zkoumání výkonu](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="f5e92-112">Read about this new experience in [Find and fix performance bottlenecks with the interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="f5e92-113"><a name="setup"></a>Nastavení monitorování výkonu</span><span class="sxs-lookup"><span data-stu-id="f5e92-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="f5e92-114">Pokud jste ještě nepřidali Application Insights do projektu (to znamená, pokud nemá soubor ApplicationInsights.config), vyberte jednu z těchto způsobů, jak začít:</span><span class="sxs-lookup"><span data-stu-id="f5e92-114">If you haven't yet added Application Insights to your project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways to get started:</span></span>

* [<span data-ttu-id="f5e92-115">Webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5e92-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="f5e92-116">Přidat monitorování výjimek</span><span class="sxs-lookup"><span data-stu-id="f5e92-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="f5e92-117">Přidat monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="f5e92-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="f5e92-118">Webových aplikací J2EE</span><span class="sxs-lookup"><span data-stu-id="f5e92-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="f5e92-119">Přidat monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="f5e92-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="f5e92-120"><a name="view"></a>Zkoumání metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="f5e92-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="f5e92-121">V [portálu Azure](https://portal.azure.com), procházejte do zdroje Application Insights, které jste nastavili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f5e92-121">In [the Azure portal](https://portal.azure.com), browse to the Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="f5e92-122">V okně Přehled zobrazuje data výkonu základní:</span><span class="sxs-lookup"><span data-stu-id="f5e92-122">The overview blade shows basic performance data:</span></span>

<span data-ttu-id="f5e92-123">Klikněte na libovolný graf zobrazíte další podrobnosti a zobrazit výsledky delší dobu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-123">Click any chart to see more detail, and to see results for a longer period.</span></span> <span data-ttu-id="f5e92-124">Například klikněte na dlaždici požadavků a pak vyberte časové rozmezí:</span><span class="sxs-lookup"><span data-stu-id="f5e92-124">For example, click the Requests tile and then select a time range:</span></span>

![Proklikejte se k více dat a vyberte časový rozsah](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="f5e92-126">Klikněte na graf vybrat metriky, které se zobrazí, nebo přidejte nový graf a vyberte jeho metriky:</span><span class="sxs-lookup"><span data-stu-id="f5e92-126">Click a chart to choose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Klikněte na graf vybrat metriky](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="f5e92-128">**Zrušte zaškrtnutí políčka všechny metriky** zobrazíte úplné výběru, která je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f5e92-128">**Uncheck all the metrics** to see the full selection that is available.</span></span> <span data-ttu-id="f5e92-129">Metriky lze rozdělit do skupin; Pokud je vybraná kteréhokoli člena skupiny, zobrazí se pouze ostatní členové této skupiny.</span><span class="sxs-lookup"><span data-stu-id="f5e92-129">The metrics fall into groups; when any member of a group is selected, only the other members of that group appear.</span></span>

## <span data-ttu-id="f5e92-130"><a name="metrics"></a>Jaké jsou všechny střední?</span><span class="sxs-lookup"><span data-stu-id="f5e92-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="f5e92-131">Dlaždice výkonu a sestavy</span><span class="sxs-lookup"><span data-stu-id="f5e92-131">Performance tiles and reports</span></span>
<span data-ttu-id="f5e92-132">Existují různé metriky výkonu, které můžete získat.</span><span class="sxs-lookup"><span data-stu-id="f5e92-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="f5e92-133">Začněme s těmi, které se ve výchozím nastavení v okně aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5e92-133">Let's start with those that appear by default on the application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="f5e92-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f5e92-134">Requests</span></span>
<span data-ttu-id="f5e92-135">Počet požadavků HTTP přijaté v zadaném období.</span><span class="sxs-lookup"><span data-stu-id="f5e92-135">The number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="f5e92-136">Výsledky porovnejte s výsledky v jiných sestavách zobrazíte chování vaší aplikace jako zatížení se liší.</span><span class="sxs-lookup"><span data-stu-id="f5e92-136">Compare this with the results on other reports to see how your app behaves as the load varies.</span></span>

<span data-ttu-id="f5e92-137">Požadavky HTTP zahrnují všechny požadavky GET nebo POST pro stránky, data a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f5e92-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="f5e92-138">Klikněte na dlaždici získat počty pro konkrétní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f5e92-138">Click on the tile to get counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="f5e92-139">Průměrná doba odezvy</span><span class="sxs-lookup"><span data-stu-id="f5e92-139">Average response time</span></span>
<span data-ttu-id="f5e92-140">Měří čas mezi zadání vaší aplikace a odpovědi nevrátila webového požadavku.</span><span class="sxs-lookup"><span data-stu-id="f5e92-140">Measures the time between a web request entering your application and the response being returned.</span></span>

<span data-ttu-id="f5e92-141">Body zobrazit klouzavý průměr.</span><span class="sxs-lookup"><span data-stu-id="f5e92-141">The points show a moving average.</span></span> <span data-ttu-id="f5e92-142">Pokud je celá řada požadavků, mohou existovat některé, který odchylují od průměr bez zřejmé ve špičce nebo ponořit v grafu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-142">If there are a lot of requests, there might be some that deviate from the average without an obvious peak or dip in the graph.</span></span>

<span data-ttu-id="f5e92-143">Podívejte se na neobvyklé vrcholů.</span><span class="sxs-lookup"><span data-stu-id="f5e92-143">Look for unusual peaks.</span></span> <span data-ttu-id="f5e92-144">Obecně platí očekávané doby odezvy roste s nárůstem požadavky.</span><span class="sxs-lookup"><span data-stu-id="f5e92-144">In general, expect response time to rise with a rise in requests.</span></span> <span data-ttu-id="f5e92-145">Pokud zvýšení nesoustředil příliš velký, může aplikace nedosáhli limitu prostředků, například CPU, kapacitu služby, kterou používá.</span><span class="sxs-lookup"><span data-stu-id="f5e92-145">If the rise is disproportionate, your app might be hitting a resource limit such as CPU or the capacity of a service it uses.</span></span>

<span data-ttu-id="f5e92-146">Klikněte na dlaždici získat časy pro konkrétní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f5e92-146">Click the tile to get times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="f5e92-147">Nejpomalejší požadavků</span><span class="sxs-lookup"><span data-stu-id="f5e92-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="f5e92-148">Zobrazuje, které požadavky může být nutné optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="f5e92-149">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="f5e92-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="f5e92-150">Počet požadavků, které vyvolala nezachycená výjimek.</span><span class="sxs-lookup"><span data-stu-id="f5e92-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="f5e92-151">Klikněte na dlaždici pro podrobné informace o specifických chybách a vyberte individuální žádosti zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f5e92-151">Click the tile to see the details of specific failures, and select an individual request to see its detail.</span></span> 

<span data-ttu-id="f5e92-152">Pro jednotlivé kontroly se uchovávají pouze reprezentativní vzorek chyb.</span><span class="sxs-lookup"><span data-stu-id="f5e92-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="f5e92-153">Další metriky</span><span class="sxs-lookup"><span data-stu-id="f5e92-153">Other metrics</span></span>
<span data-ttu-id="f5e92-154">Pokud chcete zjistit, co nastavit další metriky můžete zobrazit, klikněte na graf a poté zrušte všechny metriky zobrazíte kompletní k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f5e92-154">To see what other metrics you can display, click a graph, and then deselect all the metrics to see the full available set.</span></span> <span data-ttu-id="f5e92-155">Viz definice jednotlivé metriky kliknutím (i).</span><span class="sxs-lookup"><span data-stu-id="f5e92-155">Click (i) to see each metric's definition.</span></span>

![Zrušte výběr všechny metriky zobrazíte celé sady](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="f5e92-157">Výběr všech metrika zakáže ostatní, které se nesmí objevit na stejném grafu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-157">Selecting any metric disables the others that can't appear on the same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="f5e92-158">Nastavení upozornění</span><span class="sxs-lookup"><span data-stu-id="f5e92-158">Set alerts</span></span>
<span data-ttu-id="f5e92-159">Mají být informování tímto e-mailu neobvyklou hodnot všechny metriky, přidáte oznámení.</span><span class="sxs-lookup"><span data-stu-id="f5e92-159">To be notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="f5e92-160">Můžete buď k odeslání e-mailu pro účet správce, nebo na konkrétní e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="f5e92-160">You can choose either to send the email to the account administrators, or to specific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="f5e92-161">Nastavte prostředků než ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f5e92-161">Set the resource before the other properties.</span></span> <span data-ttu-id="f5e92-162">Nemáte zvolte zdroje, testu webu, pokud chcete nastavit výstrahy na metriky výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="f5e92-162">Don't choose the webtest resources if you want to set alerts on performance or usage metrics.</span></span>

<span data-ttu-id="f5e92-163">Pečlivě si uvědomit a jednotky, ve kterých budete vyzváni k zadání prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-163">Be careful to note the units in which you're asked to enter the threshold value.</span></span>

<span data-ttu-id="f5e92-164">*Tlačítko Přidat oznámení se nezobrazí.*</span><span class="sxs-lookup"><span data-stu-id="f5e92-164">*I don't see the Add Alert button.*</span></span> <span data-ttu-id="f5e92-165">-Je tato skupina účet, ke které máte oprávnění jen pro čtení?</span><span class="sxs-lookup"><span data-stu-id="f5e92-165">- Is this a group account to which you have read-only access?</span></span> <span data-ttu-id="f5e92-166">Obraťte se na správce účtu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-166">Check with the account administrator.</span></span>

## <span data-ttu-id="f5e92-167"><a name="diagnosis"></a>Diagnostika problémů</span><span class="sxs-lookup"><span data-stu-id="f5e92-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="f5e92-168">Zde je několik tipů pro hledání a diagnostice problémů s výkonem.</span><span class="sxs-lookup"><span data-stu-id="f5e92-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="f5e92-169">Nastavit [webové testy] [ availability] výstrahy, pokud váš web z umístění ocitne mimo provoz nebo reaguje pomalu nebo nesprávně.</span><span class="sxs-lookup"><span data-stu-id="f5e92-169">Set up [web tests][availability] to be alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="f5e92-170">Porovnejte počtu žádostí o s další metriky pro případ selhání nebo pomalé odezvy se vztahují k načtení.</span><span class="sxs-lookup"><span data-stu-id="f5e92-170">Compare the Request count with other metrics to see if failures or slow response are related to load.</span></span>
* <span data-ttu-id="f5e92-171">[Vložení a vyhledávací příkazy trasování] [ diagnostic] ve svém kódu pro pomoci přesně určit problémy.</span><span class="sxs-lookup"><span data-stu-id="f5e92-171">[Insert and search trace statements][diagnostic] in your code to help pinpoint problems.</span></span>
* <span data-ttu-id="f5e92-172">Monitorování webové aplikace v operaci s [živý datový proud metriky][livestream].</span><span class="sxs-lookup"><span data-stu-id="f5e92-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="f5e92-173">Zaznamenat stav aplikace .net s [ladicí program snímku][snapshot].</span><span class="sxs-lookup"><span data-stu-id="f5e92-173">Capture the state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="f5e92-174">Najít a opravit kritická místa výkonu s šetření interaktivní výkon</span><span class="sxs-lookup"><span data-stu-id="f5e92-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="f5e92-175">Nové šetření interaktivní výkonu Application Insights můžete použít k vyhledání oblasti vaší webové aplikace, které jsou zpomalení celkový výkon.</span><span class="sxs-lookup"><span data-stu-id="f5e92-175">You can use the new Application Insights interactive performance investigation to locate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="f5e92-176">Můžete rychle najít konkrétní stránky, které jsou zpomalení a použít [profilování nástroj](app-insights-profiler.md) chcete zobrazit, pokud existuje korelace mezi tyto stránek.</span><span class="sxs-lookup"><span data-stu-id="f5e92-176">You can quickly find specific pages that are slowing down, and use the [Profiling tool](app-insights-profiler.md) to see if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="f5e92-177">Vytvoří seznam pomalé provádění stránky</span><span class="sxs-lookup"><span data-stu-id="f5e92-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="f5e92-178">Prvním krokem pro hledání problémy s výkonem je získat seznam pomalé odpovídá stránky.</span><span class="sxs-lookup"><span data-stu-id="f5e92-178">The first step for finding performance issues is to get a list of the slow responding pages.</span></span> <span data-ttu-id="f5e92-179">Následující kopie obrazovky ukazuje, jak získat seznam potenciální na stránkách prošetřily pomocí okno výkon.</span><span class="sxs-lookup"><span data-stu-id="f5e92-179">The screen shot below demonstrates using the Performance blade to get a list of potential pages to investigate further.</span></span> <span data-ttu-id="f5e92-180">Rychle uvidíte z této stránky, že došlo zpomalování doby odezvy aplikace v přibližně 6:00 PM a znovu přibližně 22: 00.</span><span class="sxs-lookup"><span data-stu-id="f5e92-180">You can quickly see from this page that there was a slow-down in the response time of the app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="f5e92-181">Můžete také zjistit, že zákazník nebo podrobnosti o operaci GET měl některé dlouhotrvající operace s dobou odezvy střední 507.05 milisekundách.</span><span class="sxs-lookup"><span data-stu-id="f5e92-181">You can also see that the GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Interaktivní výkon statistiky aplikace](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="f5e92-183">Přejděte na konkrétní stránky</span><span class="sxs-lookup"><span data-stu-id="f5e92-183">Drill down on specific pages</span></span>

<span data-ttu-id="f5e92-184">Jakmile máte snímek výkon vaší aplikace, můžete získat podrobnosti o konkrétních zpomalit provádění operací.</span><span class="sxs-lookup"><span data-stu-id="f5e92-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="f5e92-185">Klikněte na všechny operace v seznamu a zobrazit podrobnosti, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f5e92-185">Click on any operation in the list to see the details as shown below.</span></span> <span data-ttu-id="f5e92-186">Z grafu se zobrazí, pokud byl výkon podle závislost.</span><span class="sxs-lookup"><span data-stu-id="f5e92-186">From the chart you can see if the performance was based on a dependency.</span></span> <span data-ttu-id="f5e92-187">Můžete také zjistit, kolik uživatelů došlo různé doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="f5e92-187">You can also see how many users experienced the various response times.</span></span> 

![Okna operations Statistika aplikací](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="f5e92-189">Přejděte na určitém časovém období</span><span class="sxs-lookup"><span data-stu-id="f5e92-189">Drill down on a specific time period</span></span>

<span data-ttu-id="f5e92-190">Po zjištění bod v čase k prozkoumání, podrobnostem a i podívejte se na konkrétní operace, které mohly způsobit zpomalování výkonu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-190">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="f5e92-191">Po kliknutí na tlačítko k určitému bodu v čase získání podrobností o stránce jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f5e92-191">When you click on a specific point in time you get the details of the page as shown below.</span></span> <span data-ttu-id="f5e92-192">V příkladu níže můžete zobrazit činnosti uvedené pro dané časové období spolu s kódy odpovědí serveru a doba trvání operace.</span><span class="sxs-lookup"><span data-stu-id="f5e92-192">In the example below you can see the operations listed for a given time period along with the server response codes and the operation duration.</span></span> <span data-ttu-id="f5e92-193">Máte také adresu url pro otevření pracovní položky sady TFS, pokud je potřeba odesílat tyto informace váš vývojový tým.</span><span class="sxs-lookup"><span data-stu-id="f5e92-193">You also have the url for opening a TFS work item if you need to send this information to your development team.</span></span>

![Application Insights časovém intervalu](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="f5e92-195">Přejděte na konkrétní operaci</span><span class="sxs-lookup"><span data-stu-id="f5e92-195">Drill down on a specific operation</span></span>

<span data-ttu-id="f5e92-196">Po zjištění bod v čase k prozkoumání, podrobnostem a i podívejte se na konkrétní operace, které mohly způsobit zpomalování výkonu.</span><span class="sxs-lookup"><span data-stu-id="f5e92-196">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="f5e92-197">Klikněte na operace ze seznamu a zobrazit podrobnosti operace, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f5e92-197">Click on an operation from the list to see the details of the operation as shown below.</span></span> <span data-ttu-id="f5e92-198">V tomto příkladu uvidíte, že operace se nezdařila a Application Insights poskytl podrobnosti, které aplikace vyvolala výjimku.</span><span class="sxs-lookup"><span data-stu-id="f5e92-198">In this example you can see that the operation failed, and Application Insights has provided the details of the exception the application threw.</span></span> <span data-ttu-id="f5e92-199">V tomto okně znovu, můžete snadno vytvořit pracovní položky sady TFS.</span><span class="sxs-lookup"><span data-stu-id="f5e92-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights operaci okno](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="f5e92-201"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5e92-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="f5e92-202">[Webové testy] [ availability] -mít webové požadavky odeslané do vaší aplikace v pravidelných intervalech z po celém světě.</span><span class="sxs-lookup"><span data-stu-id="f5e92-202">[Web tests][availability] - Have web requests sent to your application at regular intervals from around the world.</span></span>

<span data-ttu-id="f5e92-203">[Zaznamenání a hledání diagnostické trasování] [ diagnostic] – vložení trasovacího volání a analyzovat výsledky ke kotvícímu bodu problémy.</span><span class="sxs-lookup"><span data-stu-id="f5e92-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through the results to pinpoint issues.</span></span>

<span data-ttu-id="f5e92-204">[Sledování využití] [ usage] – zjistěte, jak lidé používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f5e92-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="f5e92-205">[Řešení potíží s] [ qna] - a otázky A odpovědi</span><span class="sxs-lookup"><span data-stu-id="f5e92-205">[Troubleshooting][qna] - and Q & A</span></span>



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



