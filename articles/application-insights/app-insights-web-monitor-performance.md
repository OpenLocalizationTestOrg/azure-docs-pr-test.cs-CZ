---
title: "aaaMonitor stavu a využití pomocí služby Application Insights vaší aplikace"
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
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="0013e-104">Sledování výkonu webových aplikací</span><span class="sxs-lookup"><span data-stu-id="0013e-104">Monitor performance in web applications</span></span>


<span data-ttu-id="0013e-105">Ujistěte se, že aplikace je výkon na dobré a rychle zjistěte informace o případných selhání.</span><span class="sxs-lookup"><span data-stu-id="0013e-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="0013e-106">[Application Insights] [ start] bude vás informovat o žádné problémy s výkonem a výjimkami a pomáhají najít a diagnostikovat hello hlavní příčiny.</span><span class="sxs-lookup"><span data-stu-id="0013e-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose hello root causes.</span></span>

<span data-ttu-id="0013e-107">Application Insights můžete monitorovat webové aplikace Java a ASP.NET a služby, služby WCF.</span><span class="sxs-lookup"><span data-stu-id="0013e-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="0013e-108">Mohou být hostovaná místně, virtuálními počítači, nebo jako weby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0013e-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="0013e-109">Na straně klienta hello Application Insights může trvat telemetrie z webových stránek a širokou škálu zařízení se systémy iOS, Android a aplikací pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="0013e-109">On hello client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="0013e-110">Provedli jsme nové prostředí pro vyhledání pomalé, provádění stránky ve vaší webové aplikaci k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0013e-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="0013e-111">Pokud nemáte přístup tooit, povolte ji tak, že konfigurace možností preview s hello [Preview okno](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="0013e-111">If you don't have access tooit, enable it by configuring your preview options with hello [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="0013e-112">Přečtěte si informace o toto nové prostředí v [najít a opravit kritická místa výkonu s hello interaktivní výkon šetření](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="0013e-112">Read about this new experience in [Find and fix performance bottlenecks with hello interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="0013e-113"><a name="setup"></a>Nastavení monitorování výkonu</span><span class="sxs-lookup"><span data-stu-id="0013e-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="0013e-114">Pokud jste ještě nepřidali Application Insights tooyour projektu (to znamená, pokud nemá soubor ApplicationInsights.config), vyberte jednu z těchto způsobů tooget spuštění:</span><span class="sxs-lookup"><span data-stu-id="0013e-114">If you haven't yet added Application Insights tooyour project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways tooget started:</span></span>

* [<span data-ttu-id="0013e-115">Webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0013e-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="0013e-116">Přidat monitorování výjimek</span><span class="sxs-lookup"><span data-stu-id="0013e-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="0013e-117">Přidat monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="0013e-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="0013e-118">Webových aplikací J2EE</span><span class="sxs-lookup"><span data-stu-id="0013e-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="0013e-119">Přidat monitorování závislostí</span><span class="sxs-lookup"><span data-stu-id="0013e-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="0013e-120"><a name="view"></a>Zkoumání metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="0013e-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="0013e-121">V [hello portál Azure](https://portal.azure.com), procházet toohello prostředek Application Insights, které jste nastavili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0013e-121">In [hello Azure portal](https://portal.azure.com), browse toohello Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="0013e-122">okno Přehled Hello ukazuje údaje o výkonu základní:</span><span class="sxs-lookup"><span data-stu-id="0013e-122">hello overview blade shows basic performance data:</span></span>

<span data-ttu-id="0013e-123">Klikněte na libovolný graf toosee podrobněji a toosee výsledky delší dobu.</span><span class="sxs-lookup"><span data-stu-id="0013e-123">Click any chart toosee more detail, and toosee results for a longer period.</span></span> <span data-ttu-id="0013e-124">Například klikněte na dlaždici hello požadavků a pak vyberte časový interval:</span><span class="sxs-lookup"><span data-stu-id="0013e-124">For example, click hello Requests tile and then select a time range:</span></span>

![Klikněte na tlačítko prostřednictvím toomore dat a vyberte časový rozsah](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="0013e-126">Klikněte na graf toochoose metriky, které se zobrazí, nebo přidejte nový graf a vyberte jeho metriky:</span><span class="sxs-lookup"><span data-stu-id="0013e-126">Click a chart toochoose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Klikněte na tlačítko metriky toochoose grafu](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="0013e-128">**Zrušte zaškrtnutí políčka všechny metriky hello** hello toosee úplné se výběru, která je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0013e-128">**Uncheck all hello metrics** toosee hello full selection that is available.</span></span> <span data-ttu-id="0013e-129">metriky Hello spadají do skupiny; Pokud je vybraná kteréhokoli člena skupiny, objeví jenom tehdy hello ostatní členové této skupiny.</span><span class="sxs-lookup"><span data-stu-id="0013e-129">hello metrics fall into groups; when any member of a group is selected, only hello other members of that group appear.</span></span>

## <span data-ttu-id="0013e-130"><a name="metrics"></a>Jaké jsou všechny střední?</span><span class="sxs-lookup"><span data-stu-id="0013e-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="0013e-131">Dlaždice výkonu a sestavy</span><span class="sxs-lookup"><span data-stu-id="0013e-131">Performance tiles and reports</span></span>
<span data-ttu-id="0013e-132">Existují různé metriky výkonu, které můžete získat.</span><span class="sxs-lookup"><span data-stu-id="0013e-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="0013e-133">Začněme s těmi, které se ve výchozím nastavení v okně aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-133">Let's start with those that appear by default on hello application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="0013e-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0013e-134">Requests</span></span>
<span data-ttu-id="0013e-135">Hello počet požadavků HTTP přijaté v zadaném období.</span><span class="sxs-lookup"><span data-stu-id="0013e-135">hello number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="0013e-136">Toto porovnání s výsledky hello na jiné sestavy toosee chování vaší aplikace jako zatížení hello liší.</span><span class="sxs-lookup"><span data-stu-id="0013e-136">Compare this with hello results on other reports toosee how your app behaves as hello load varies.</span></span>

<span data-ttu-id="0013e-137">Požadavky HTTP zahrnují všechny požadavky GET nebo POST pro stránky, data a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0013e-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="0013e-138">Klikněte na hello dlaždice tooget počty pro konkrétní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0013e-138">Click on hello tile tooget counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="0013e-139">Průměrná doba odezvy</span><span class="sxs-lookup"><span data-stu-id="0013e-139">Average response time</span></span>
<span data-ttu-id="0013e-140">Míry hello čas mezi zadáním vaše aplikace a hello odpověď nevrátila webového požadavku.</span><span class="sxs-lookup"><span data-stu-id="0013e-140">Measures hello time between a web request entering your application and hello response being returned.</span></span>

<span data-ttu-id="0013e-141">body Hello zobrazit klouzavý průměr.</span><span class="sxs-lookup"><span data-stu-id="0013e-141">hello points show a moving average.</span></span> <span data-ttu-id="0013e-142">Pokud je celá řada požadavků, mohou existovat některé, který odchylují od hello průměr bez zřejmé ve špičce nebo ponořit v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-142">If there are a lot of requests, there might be some that deviate from hello average without an obvious peak or dip in hello graph.</span></span>

<span data-ttu-id="0013e-143">Podívejte se na neobvyklé vrcholů.</span><span class="sxs-lookup"><span data-stu-id="0013e-143">Look for unusual peaks.</span></span> <span data-ttu-id="0013e-144">Obecně platí očekávejte toorise čas odpovědi s nárůstem požadavky.</span><span class="sxs-lookup"><span data-stu-id="0013e-144">In general, expect response time toorise with a rise in requests.</span></span> <span data-ttu-id="0013e-145">Pokud zvýšení hello nesoustředil příliš velký, může být aplikace nedosáhli limitu prostředků, jako je kapacita procesoru nebo hello služby, kterou používá.</span><span class="sxs-lookup"><span data-stu-id="0013e-145">If hello rise is disproportionate, your app might be hitting a resource limit such as CPU or hello capacity of a service it uses.</span></span>

<span data-ttu-id="0013e-146">Klikněte na tlačítko hello dlaždice tooget časy pro konkrétní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0013e-146">Click hello tile tooget times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="0013e-147">Nejpomalejší požadavků</span><span class="sxs-lookup"><span data-stu-id="0013e-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="0013e-148">Zobrazuje, které požadavky může být nutné optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="0013e-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="0013e-149">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="0013e-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="0013e-150">Počet požadavků, které vyvolala nezachycená výjimek.</span><span class="sxs-lookup"><span data-stu-id="0013e-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="0013e-151">Klikněte hello dlaždice toosee hello podrobnosti o konkrétní selhání a vyberte individuální žádosti toosee její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0013e-151">Click hello tile toosee hello details of specific failures, and select an individual request toosee its detail.</span></span> 

<span data-ttu-id="0013e-152">Pro jednotlivé kontroly se uchovávají pouze reprezentativní vzorek chyb.</span><span class="sxs-lookup"><span data-stu-id="0013e-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="0013e-153">Další metriky</span><span class="sxs-lookup"><span data-stu-id="0013e-153">Other metrics</span></span>
<span data-ttu-id="0013e-154">toosee jaké další metriky, můžete zobrazit, klikněte na graf a potom zrušte výběr všech hello metriky toosee hello veškerou dostupnou sadu.</span><span class="sxs-lookup"><span data-stu-id="0013e-154">toosee what other metrics you can display, click a graph, and then deselect all hello metrics toosee hello full available set.</span></span> <span data-ttu-id="0013e-155">Klikněte na tlačítko (i) toosee definice jednotlivé metriky.</span><span class="sxs-lookup"><span data-stu-id="0013e-155">Click (i) toosee each metric's definition.</span></span>

![Zrušte výběr všech metriky toosee hello celé sady](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="0013e-157">Výběr všechny metriky zakáže hello ostatní, které se nesmí objevit na stejném grafu hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-157">Selecting any metric disables hello others that can't appear on hello same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="0013e-158">Nastavení upozornění</span><span class="sxs-lookup"><span data-stu-id="0013e-158">Set alerts</span></span>
<span data-ttu-id="0013e-159">toobe oznámení e-mailem neobvyklou hodnot všechny metriky přidat oznámení.</span><span class="sxs-lookup"><span data-stu-id="0013e-159">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="0013e-160">Můžete buď toosend hello e-mailu toohello účet správce, nebo toospecific e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="0013e-160">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="0013e-161">Nastavte další vlastnosti prostředku hello před hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-161">Set hello resource before hello other properties.</span></span> <span data-ttu-id="0013e-162">Hello testu webu prostředky nemáte zvolte, pokud chcete generovat výstrahy tooset na výkon nebo metriky využití.</span><span class="sxs-lookup"><span data-stu-id="0013e-162">Don't choose hello webtest resources if you want tooset alerts on performance or usage metrics.</span></span>

<span data-ttu-id="0013e-163">Být opatrní toonote hello jednotky, ve kterých se dotaz tooenter hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0013e-163">Be careful toonote hello units in which you're asked tooenter hello threshold value.</span></span>

<span data-ttu-id="0013e-164">*Výstrahy tlačítko Přidat hello se nezobrazí.*</span><span class="sxs-lookup"><span data-stu-id="0013e-164">*I don't see hello Add Alert button.*</span></span> <span data-ttu-id="0013e-165">-Jedná toowhich účet skupiny mají oprávnění jen pro čtení?</span><span class="sxs-lookup"><span data-stu-id="0013e-165">- Is this a group account toowhich you have read-only access?</span></span> <span data-ttu-id="0013e-166">Zeptejte se správce účtu hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-166">Check with hello account administrator.</span></span>

## <span data-ttu-id="0013e-167"><a name="diagnosis"></a>Diagnostika problémů</span><span class="sxs-lookup"><span data-stu-id="0013e-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="0013e-168">Zde je několik tipů pro hledání a diagnostice problémů s výkonem.</span><span class="sxs-lookup"><span data-stu-id="0013e-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="0013e-169">Nastavit [webové testy] [ availability] toobe zobrazí výstraha, pokud váš web z umístění ocitne mimo provoz nebo reaguje pomalu nebo nesprávně.</span><span class="sxs-lookup"><span data-stu-id="0013e-169">Set up [web tests][availability] toobe alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="0013e-170">Porovnání počtu žádostí o hello s další metriky toosee, pokud jsou související tooload selhání nebo pomalé odezvy.</span><span class="sxs-lookup"><span data-stu-id="0013e-170">Compare hello Request count with other metrics toosee if failures or slow response are related tooload.</span></span>
* <span data-ttu-id="0013e-171">[Vložení a vyhledávací příkazy trasování] [ diagnostic] v váš kód toohelp přesné problémy.</span><span class="sxs-lookup"><span data-stu-id="0013e-171">[Insert and search trace statements][diagnostic] in your code toohelp pinpoint problems.</span></span>
* <span data-ttu-id="0013e-172">Monitorování webové aplikace v operaci s [živý datový proud metriky][livestream].</span><span class="sxs-lookup"><span data-stu-id="0013e-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="0013e-173">Zaznamenat stav hello aplikace .net s [ladicí program snímku][snapshot].</span><span class="sxs-lookup"><span data-stu-id="0013e-173">Capture hello state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="0013e-174">Najít a opravit kritická místa výkonu s šetření interaktivní výkon</span><span class="sxs-lookup"><span data-stu-id="0013e-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="0013e-175">Oblasti lze použít hello nové Application Insights interaktivní výkon šetření toolocate vaší webové aplikace, které jsou zpomalení celkový výkon.</span><span class="sxs-lookup"><span data-stu-id="0013e-175">You can use hello new Application Insights interactive performance investigation toolocate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="0013e-176">Můžete rychle najít konkrétní stránky, které jsou zpomaluje a použijte hello [profilování nástroj](app-insights-profiler.md) toosee, pokud existuje korelace mezi tyto stránek.</span><span class="sxs-lookup"><span data-stu-id="0013e-176">You can quickly find specific pages that are slowing down, and use hello [Profiling tool](app-insights-profiler.md) toosee if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="0013e-177">Vytvoří seznam pomalé provádění stránky</span><span class="sxs-lookup"><span data-stu-id="0013e-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="0013e-178">prvním krokem Hello naleznou problémy s výkonem je tooget seznam hello pomalé zpracování stránky.</span><span class="sxs-lookup"><span data-stu-id="0013e-178">hello first step for finding performance issues is tooget a list of hello slow responding pages.</span></span> <span data-ttu-id="0013e-179">úvodní obrazovka snímek níže ukazuje, jak pomocí hello výkonu okno tooget seznam další potenciální tooinvestigate stránky.</span><span class="sxs-lookup"><span data-stu-id="0013e-179">hello screen shot below demonstrates using hello Performance blade tooget a list of potential pages tooinvestigate further.</span></span> <span data-ttu-id="0013e-180">Rychle uvidíte z této stránky, že došlo zpomalování doby odezvy hello aplikace hello v přibližně 6:00 PM a znovu přibližně 22: 00.</span><span class="sxs-lookup"><span data-stu-id="0013e-180">You can quickly see from this page that there was a slow-down in hello response time of hello app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="0013e-181">Můžete také zjistit, že operace zákazníka nebo podrobnosti o získání hello měl některé dlouho běžící operace s střední odezvu 507.05 milisekund.</span><span class="sxs-lookup"><span data-stu-id="0013e-181">You can also see that hello GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Interaktivní výkon statistiky aplikace](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="0013e-183">Přejděte na konkrétní stránky</span><span class="sxs-lookup"><span data-stu-id="0013e-183">Drill down on specific pages</span></span>

<span data-ttu-id="0013e-184">Jakmile máte snímek výkon vaší aplikace, můžete získat podrobnosti o konkrétních zpomalit provádění operací.</span><span class="sxs-lookup"><span data-stu-id="0013e-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="0013e-185">Klikněte na všechny operace v hello seznamu toosee hello podrobnosti jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0013e-185">Click on any operation in hello list toosee hello details as shown below.</span></span> <span data-ttu-id="0013e-186">Z hello grafu se zobrazí, pokud byl hello výkonu podle závislost.</span><span class="sxs-lookup"><span data-stu-id="0013e-186">From hello chart you can see if hello performance was based on a dependency.</span></span> <span data-ttu-id="0013e-187">Uvidíte také kolik uživatelů zkušeného hello různé doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="0013e-187">You can also see how many users experienced hello various response times.</span></span> 

![Okna operations Statistika aplikací](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="0013e-189">Přejděte na určitém časovém období</span><span class="sxs-lookup"><span data-stu-id="0013e-189">Drill down on a specific time period</span></span>

<span data-ttu-id="0013e-190">Poté, co jste našli bod v časové tooinvestigate, k podrobnostem i další toolook v hello konkrétních operací, které mohly způsobit zpomalování výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-190">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="0013e-191">Po kliknutí na tlačítko k určitému bodu v čase získáte podrobnosti o hello hello stránky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0013e-191">When you click on a specific point in time you get hello details of hello page as shown below.</span></span> <span data-ttu-id="0013e-192">V hello najdete příklad níže uvedené pro dané časové období spolu s kódy odpovědí serveru hello a doba trvání operace hello operations hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-192">In hello example below you can see hello operations listed for a given time period along with hello server response codes and hello operation duration.</span></span> <span data-ttu-id="0013e-193">Adresa url hello máte také můžete otevřít pracovní položky sady TFS, pokud potřebujete toosend tato informace tooyour vývojový tým.</span><span class="sxs-lookup"><span data-stu-id="0013e-193">You also have hello url for opening a TFS work item if you need toosend this information tooyour development team.</span></span>

![Application Insights časovém intervalu](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="0013e-195">Přejděte na konkrétní operaci</span><span class="sxs-lookup"><span data-stu-id="0013e-195">Drill down on a specific operation</span></span>

<span data-ttu-id="0013e-196">Poté, co jste našli bod v časové tooinvestigate, k podrobnostem i další toolook v hello konkrétních operací, které mohly způsobit zpomalování výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-196">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="0013e-197">Klikněte na operace z hello seznamu toosee hello podrobnosti hello operace, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0013e-197">Click on an operation from hello list toosee hello details of hello operation as shown below.</span></span> <span data-ttu-id="0013e-198">V tomto příkladu, které se zobrazí hello operace se nezdařila, a Application Insights poskytl hello podrobnosti o hello aplikace hello výjimka vyvolala.</span><span class="sxs-lookup"><span data-stu-id="0013e-198">In this example you can see that hello operation failed, and Application Insights has provided hello details of hello exception hello application threw.</span></span> <span data-ttu-id="0013e-199">V tomto okně znovu, můžete snadno vytvořit pracovní položky sady TFS.</span><span class="sxs-lookup"><span data-stu-id="0013e-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights operaci okno](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="0013e-201"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0013e-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="0013e-202">[Webové testy] [ availability] -webových požadavků odeslali tooyour aplikace v pravidelných intervalech z kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="0013e-202">[Web tests][availability] - Have web requests sent tooyour application at regular intervals from around hello world.</span></span>

<span data-ttu-id="0013e-203">[Zaznamenání a hledání diagnostické trasování] [ diagnostic] – vložení trasovacího volání a analyzovat problémy toopinpoint výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="0013e-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through hello results toopinpoint issues.</span></span>

<span data-ttu-id="0013e-204">[Sledování využití] [ usage] – zjistěte, jak lidé používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0013e-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="0013e-205">[Řešení potíží s] [ qna] - a otázky A odpovědi</span><span class="sxs-lookup"><span data-stu-id="0013e-205">[Troubleshooting][qna] - and Q & A</span></span>



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



