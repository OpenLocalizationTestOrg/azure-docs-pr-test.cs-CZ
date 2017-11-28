---
title: "webové aplikace aaaAzure Application Insights pro JavaScript | Microsoft Docs"
description: "Načtení zobrazení stránek a počty relací, data webového klienta a sledování vzorů využití. Zjištění výjimek a problémů s výkonem na webových stránkách v jazyce JavaScript."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a><span data-ttu-id="dba3a-104">Application Insights pro webové stránky</span><span class="sxs-lookup"><span data-stu-id="dba3a-104">Application Insights for web pages</span></span>
<span data-ttu-id="dba3a-105">Zjistěte informace o hello výkonu a využití webové stránky nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="dba3a-105">Find out about hello performance and usage of your web page or app.</span></span> <span data-ttu-id="dba3a-106">Pokud přidáte [Application Insights](app-insights-overview.md) tooyour skript stránky, získáte časování načtení stránky a volání AJAX, počty a podrobnosti výjimek prohlížeče a selhání AJAX, a také uživatelům a počty relací.</span><span class="sxs-lookup"><span data-stu-id="dba3a-106">If you add [Application Insights](app-insights-overview.md) tooyour page script, you get timings of page loads and AJAX calls, counts and details of browser exceptions and AJAX failures, as well as users and session counts.</span></span> <span data-ttu-id="dba3a-107">Všechny tyto hodnoty mohou být segmentovány podle stránky, klientského operačního systému a verze prohlížeče, zeměpisné polohy a ostatních dimenzí.</span><span class="sxs-lookup"><span data-stu-id="dba3a-107">All these can be segmented by page, client OS and browser version, geo location, and other dimensions.</span></span> <span data-ttu-id="dba3a-108">Můžete nastavit výstrahy na počet selhání nebo pomalé načítání stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-108">You can set alerts on failure counts or slow page loading.</span></span> <span data-ttu-id="dba3a-109">A vkládání trasovacího volání do kódu jazyka JavaScript, můžete sledovat, použití různých funkcí aplikace webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="dba3a-109">And by inserting trace calls in your JavaScript code, you can track how hello different features of your web page application are used.</span></span>

<span data-ttu-id="dba3a-110">Application Insights můžete použít s jakýmikoli webovými stránkami – stačí přidat krátký kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dba3a-110">Application Insights can be used with any web pages - you just add a short piece of JavaScript.</span></span> <span data-ttu-id="dba3a-111">Pokud používáte webovou službu [Java](app-insights-java-get-started.md) nebo [ASP.NET](app-insights-asp-net.md), můžete integrovat telemetrii ze serveru a klientů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-111">If your web service is [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md), you can integrate telemetry from your server and clients.</span></span>

![Na stránce portal.azure.com otevřete prostředek vaší aplikace a klikněte na Prohlížeč.](./media/app-insights-javascript/03.png)

<span data-ttu-id="dba3a-113">Musíte mít předplatné příliš[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="dba3a-113">You need a subscription too[Microsoft Azure](https://azure.com).</span></span> <span data-ttu-id="dba3a-114">Pokud má váš tým předplatné pro společnosti, požádejte vlastníka tooadd hello vaše tooit Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dba3a-114">If your team has an organizational subscription, ask hello owner tooadd your Microsoft Account tooit.</span></span> <span data-ttu-id="dba3a-115">Vývoj a méně rozsáhlé používání vás nebudou nic stát.</span><span class="sxs-lookup"><span data-stu-id="dba3a-115">Development and small-scale use won't cost anything.</span></span>

## <a name="set-up-application-insights-for-your-web-page"></a><span data-ttu-id="dba3a-116">Nastavte Application Insights pro svou webovou stránku</span><span class="sxs-lookup"><span data-stu-id="dba3a-116">Set up Application Insights for your web page</span></span>
<span data-ttu-id="dba3a-117">Přidejte hello zavaděč kód fragment kódu tooyour webové stránky, následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dba3a-117">Add hello loader code snippet tooyour web pages, as follows.</span></span>

### <a name="open-or-create-application-insights-resource"></a><span data-ttu-id="dba3a-118">Otevření nebo vytvoření prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="dba3a-118">Open or create Application Insights resource</span></span>
<span data-ttu-id="dba3a-119">Hello prostředek Application Insights je, kde se zobrazí data o výkonu a využití vaší stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-119">hello Application Insights resource is where data about your page's performance and usage is displayed.</span></span> 

<span data-ttu-id="dba3a-120">Přihlaste se na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dba3a-120">Sign into [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="dba3a-121">Pokud jste již nastavili monitorování na straně serveru hello vaší aplikace, již mít prostředek:</span><span class="sxs-lookup"><span data-stu-id="dba3a-121">If you already set up monitoring for hello server side of your app, you already have a resource:</span></span>

![Zvolte Procházet, služby pro vývojáře, Application Insights.](./media/app-insights-javascript/01-find.png)

<span data-ttu-id="dba3a-123">Pokud ji nemáte, vytvořte ji:</span><span class="sxs-lookup"><span data-stu-id="dba3a-123">If you don't have one, create it:</span></span>

![Zvolte Nový, služby pro vývojáře, Application Insights.](./media/app-insights-javascript/01-create.png)

<span data-ttu-id="dba3a-125">*Již máte dotazy?*</span><span class="sxs-lookup"><span data-stu-id="dba3a-125">*Questions already?*</span></span> <span data-ttu-id="dba3a-126">[Další informace o vytvoření prostředku](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="dba3a-126">[More about creating a resource](app-insights-create-new-resource.md).</span></span>

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a><span data-ttu-id="dba3a-127">Přidat hello SDK skriptu tooyour aplikace nebo webové stránky</span><span class="sxs-lookup"><span data-stu-id="dba3a-127">Add hello SDK script tooyour app or web pages</span></span>
<span data-ttu-id="dba3a-128">V části rychlý Start získáte hello skript pro webové stránky:</span><span class="sxs-lookup"><span data-stu-id="dba3a-128">In Quick Start, get hello script for web pages:</span></span>

![V okně přehledu aplikace zvolte rychlý Start, získat kód toomonitor webové stránky.](./media/app-insights-javascript/02-monitor-web-page.png)

<span data-ttu-id="dba3a-131">Vložte skript hello těsně před hello `</head>` značky každé stránce, kterou chcete tootrack.</span><span class="sxs-lookup"><span data-stu-id="dba3a-131">Insert hello script just before hello `</head>` tag of every page you want tootrack.</span></span> <span data-ttu-id="dba3a-132">Pokud má daný web stránku předlohy, můžete umístit hello skriptu existuje.</span><span class="sxs-lookup"><span data-stu-id="dba3a-132">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="dba3a-133">Například:</span><span class="sxs-lookup"><span data-stu-id="dba3a-133">For example:</span></span>

* <span data-ttu-id="dba3a-134">Vložíte ho do projektu aplikace ASP.NET MVC do složky `View\Shared\_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="dba3a-134">In an ASP.NET MVC project, you'd put it in `View\Shared\_Layout.cshtml`</span></span>
* <span data-ttu-id="dba3a-135">Na webu služby SharePoint, v Ovládacích panelech hello otevřete [nastavení webu / stránky předlohy](app-insights-sharepoint.md).</span><span class="sxs-lookup"><span data-stu-id="dba3a-135">In a SharePoint site, on hello control panel, open [Site Settings / Master Page](app-insights-sharepoint.md).</span></span>

<span data-ttu-id="dba3a-136">Hello skript obsahuje klíč instrumentace hello, který přesměruje prostředek Application Insights tooyour hello data.</span><span class="sxs-lookup"><span data-stu-id="dba3a-136">hello script contains hello instrumentation key that directs hello data tooyour Application Insights resource.</span></span> 

<span data-ttu-id="dba3a-137">([Hlubší vysvětlení skriptu hello. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span><span class="sxs-lookup"><span data-stu-id="dba3a-137">([Deeper explanation of hello script.](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span></span>

<span data-ttu-id="dba3a-138">*(Pokud používáte framework dobře známé webové stránky, vyhledejte adaptéry Application Insights. Například je k dispozici [modul AngularJS](http://ngmodules.org/modules/angular-appinsights).)*</span><span class="sxs-lookup"><span data-stu-id="dba3a-138">*(If you're using a well-known web page framework, look around for Application Insights adaptors. For example, there's [an AngularJS module](http://ngmodules.org/modules/angular-appinsights).)*</span></span>

## <a name="detailed-configuration"></a><span data-ttu-id="dba3a-139">Podrobná konfigurace</span><span class="sxs-lookup"><span data-stu-id="dba3a-139">Detailed configuration</span></span>
<span data-ttu-id="dba3a-140">Nastavit můžete několik [Parametrů](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config), i když ve většině případů to není třeba.</span><span class="sxs-lookup"><span data-stu-id="dba3a-140">There are several [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) you can set, though in most cases, you shouldn't need to.</span></span> <span data-ttu-id="dba3a-141">Můžete například zakázat nebo omezit hello počet volání Ajax hlášených na zobrazení stránky (tooreduce provoz).</span><span class="sxs-lookup"><span data-stu-id="dba3a-141">For example, you can disable or limit hello number of Ajax calls reported per page view (tooreduce traffic).</span></span> <span data-ttu-id="dba3a-142">Nebo můžete nastavit ladění režimu toohave telemetrie přesunutí rychle prostřednictvím hello kanálu bez provedení dávkou.</span><span class="sxs-lookup"><span data-stu-id="dba3a-142">Or you can set debug mode toohave telemetry move rapidly through hello pipeline without being batched.</span></span>

<span data-ttu-id="dba3a-143">tooset tyto parametry, vyhledejte tento řádek ve fragmentu kódu hello a po ní přidat další položky oddělené čárkami:</span><span class="sxs-lookup"><span data-stu-id="dba3a-143">tooset these parameters, look for this line in hello code snippet, and add more comma-separated items after it:</span></span>

    })({
      instrumentationKey: "..."
      // Insert here
    });

<span data-ttu-id="dba3a-144">Hello [dostupné parametry](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) zahrnují:</span><span class="sxs-lookup"><span data-stu-id="dba3a-144">hello [available parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) include:</span></span>

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <span data-ttu-id="dba3a-145"><a name="run"></a>Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="dba3a-145"><a name="run"></a>Run your app</span></span>
<span data-ttu-id="dba3a-146">Spouštění vaší webové aplikace, použijte ji při toogenerate telemetrie a počkejte a několik sekund.</span><span class="sxs-lookup"><span data-stu-id="dba3a-146">Run your web app, use it a while toogenerate telemetry, and wait a few seconds.</span></span> <span data-ttu-id="dba3a-147">Můžete buď spustit pomocí hello **F5** klíče na vývojovém počítači, nebo ji publikovat a umožnit uživatelům si ji vyzkoušeli.</span><span class="sxs-lookup"><span data-stu-id="dba3a-147">You can either run it using hello **F5** key on your development machine, or publish it and let users play with it.</span></span>

<span data-ttu-id="dba3a-148">Pokud chcete toocheck hello telemetrie, že webová aplikace odesílá tooApplication Insights, použijte ladicí nástroje prohlížeče (**F12** u mnoha prohlížečů).</span><span class="sxs-lookup"><span data-stu-id="dba3a-148">If you want toocheck hello telemetry that a web app is sending tooApplication Insights, use your browser's debugging tools (**F12** on many browsers).</span></span> <span data-ttu-id="dba3a-149">Data se odešlou toodc.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="dba3a-149">Data is sent toodc.services.visualstudio.com.</span></span>

## <a name="explore-your-browser-performance-data"></a><span data-ttu-id="dba3a-150">Prozkoumejte data výkonu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="dba3a-150">Explore your browser performance data</span></span>
<span data-ttu-id="dba3a-151">Otevřete hello tooshow okno prohlížeče agregovat údaje o výkonu z prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-151">Open hello Browser blade tooshow aggregated performance data from your users' browsers.</span></span>

![Na stránce portal.azure.com otevřete prostředek vaší aplikace a klikněte na tlačítko Nastavení, Prohlížeč](./media/app-insights-javascript/03.png)

<span data-ttu-id="dba3a-153">*Žádná data? Klikněte na tlačítko **aktualizovat** hello horní části stránky hello. Stále nic? Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).*</span><span class="sxs-lookup"><span data-stu-id="dba3a-153">*No data yet? Click **Refresh** at hello top of hello page. Still nothing? See [Troubleshooting](app-insights-troubleshoot-faq.md).*</span></span>

<span data-ttu-id="dba3a-154">je Hello okno prohlížeče [okno Průzkumníku metrik](app-insights-metrics-explorer.md) s přednastavenými filtry a výběry grafu.</span><span class="sxs-lookup"><span data-stu-id="dba3a-154">hello Browser blade is a [Metrics Explorer blade](app-insights-metrics-explorer.md) with preset filters and chart selections.</span></span> <span data-ttu-id="dba3a-155">Pokud chcete a uložit výsledek hello do oblíbených položek můžete upravit hello časové rozmezí, filtry a konfiguraci grafu.</span><span class="sxs-lookup"><span data-stu-id="dba3a-155">You can edit hello time range, filters, and chart configuration if you want, and save hello result as a favorite.</span></span> <span data-ttu-id="dba3a-156">Klikněte na tlačítko **obnovit výchozí nastavení** tooget back toohello původní konfigurace okna.</span><span class="sxs-lookup"><span data-stu-id="dba3a-156">Click **Restore defaults** tooget back toohello original blade configuration.</span></span>

## <a name="page-load-performance"></a><span data-ttu-id="dba3a-157">Stav zatížení stránky</span><span class="sxs-lookup"><span data-stu-id="dba3a-157">Page load performance</span></span>
<span data-ttu-id="dba3a-158">V hello je horní části naleznete Segmentovaný grafu časů načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-158">At hello top is a segmented chart of page load times.</span></span> <span data-ttu-id="dba3a-159">Celková výška grafu hello Hello představuje hello Průměrná doba tooload a zobrazení stránky z vaší aplikace v prohlížečích vašich uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-159">hello total height of hello chart represents hello average time tooload and display pages from your app in your users' browsers.</span></span> <span data-ttu-id="dba3a-160">Hello čas se měří od, když hello prohlížeč odesílá počáteční požadavek HTTP hello dokud veškerých synchronních zatížení, které byly zpracovány události, včetně rozložení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-160">hello time is measured from when hello browser sends hello initial HTTP request until all synchronous load events have been processed, including layout and running scripts.</span></span> <span data-ttu-id="dba3a-161">Neobsahuje asynchronní úlohy, například načítání webových součástí z volání AJAX.</span><span class="sxs-lookup"><span data-stu-id="dba3a-161">It doesn't include asynchronous tasks such as loading web parts from AJAX calls.</span></span>

<span data-ttu-id="dba3a-162">Hello tabulka segmentuje hello celkovou dobu načítání stránky do hello [standardních časování definovaných pomocí W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span><span class="sxs-lookup"><span data-stu-id="dba3a-162">hello chart segments hello total page load time into hello [standard timings defined by W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span></span> 

![](./media/app-insights-javascript/08-client-split.png)

<span data-ttu-id="dba3a-163">Všimněte si, že hello *připojení k síti* čas je často nižší, než by se dalo očekávat, protože je průměrem přes všechny požadavky ze serveru toohello hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dba3a-163">Note that hello *network connect* time is often lower than you might expect, because it's an average over all requests from hello browser toohello server.</span></span> <span data-ttu-id="dba3a-164">Mnoho jednotlivých požadavků obsahuje dobu připojení 0, protože je již serveru toohello aktivní připojení.</span><span class="sxs-lookup"><span data-stu-id="dba3a-164">Many individual requests have a connect time of 0 because there is already an active connection toohello server.</span></span>

### <a name="slow-loading"></a><span data-ttu-id="dba3a-165">Pomalé načítání?</span><span class="sxs-lookup"><span data-stu-id="dba3a-165">Slow loading?</span></span>
<span data-ttu-id="dba3a-166">Pomalé načítání stránek představuje hlavní zdroj nespokojenosti uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-166">Slow page loads are a major source of dissatisfaction for your users.</span></span> <span data-ttu-id="dba3a-167">Pokud hello tabulka naznačuje pomalé načítání stránky, je snadné toodo trochu diagnostického výzkumu.</span><span class="sxs-lookup"><span data-stu-id="dba3a-167">If hello chart indicates slow page loads, it's easy toodo some diagnostic research.</span></span>

<span data-ttu-id="dba3a-168">Hello graf znázorňuje hello průměr všech načtení stránky ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dba3a-168">hello chart shows hello average of all page loads in your app.</span></span> <span data-ttu-id="dba3a-169">toosee, pokud je problém hello omezen tooparticular stránky, naleznete další dolů hello okno, kde jsou mřížky segmentované podle adresy URL stránky:</span><span class="sxs-lookup"><span data-stu-id="dba3a-169">toosee if hello problem is confined tooparticular pages, look further down hello blade, where there's a grid segmented by page URL:</span></span>

![](./media/app-insights-javascript/09-page-perf.png)

<span data-ttu-id="dba3a-170">Všimněte si počtu zobrazení hello stránky a směrodatné odchylky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-170">Notice hello page view count and standard deviation.</span></span> <span data-ttu-id="dba3a-171">Pokud je hello počet stránek velmi nízký, pak problém hello není dopad na uživatele mnohem.</span><span class="sxs-lookup"><span data-stu-id="dba3a-171">If hello page count is very low, then hello issue isn't affecting users much.</span></span> <span data-ttu-id="dba3a-172">Vysoká směrodatná odchylka (srovnatelná toohello samotným průměrem) označuje velké rozdíly mezi jednotlivými měřeními.</span><span class="sxs-lookup"><span data-stu-id="dba3a-172">A high standard deviation (comparable toohello average itself) indicates a lot of variation between individual measurements.</span></span>

<span data-ttu-id="dba3a-173">**Přibližte si jednu adresu URL a jednu stránku zobrazení.**</span><span class="sxs-lookup"><span data-stu-id="dba3a-173">**Zoom in on one URL and one page view.**</span></span> <span data-ttu-id="dba3a-174">Klikněte na všechny stránky název toosee okno prohlížeče grafy filtrované právě toothat URL; a pak na instanci zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-174">Click any page name toosee a blade of browser charts filtered just toothat URL; and then on an instance of a page view.</span></span>

![](./media/app-insights-javascript/35.png)

<span data-ttu-id="dba3a-175">Klikněte na tlačítko `...` pro úplný seznam vlastností pro danou událost nebo zkontrolujte volání Ajax hello a související události.</span><span class="sxs-lookup"><span data-stu-id="dba3a-175">Click `...` for a full list of properties for that event, or inspect hello Ajax calls and related events.</span></span> <span data-ttu-id="dba3a-176">Pomalá volání Ajax ovlivňují hello celkový čas načítání stránky, pokud jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="dba3a-176">Slow Ajax calls affect hello overall page load time if they are synchronous.</span></span> <span data-ttu-id="dba3a-177">Související události zahrnují požadavky serveru pro hello stejnou adresu URL (Pokud jste nastavili Application Insights na webovém serveru).</span><span class="sxs-lookup"><span data-stu-id="dba3a-177">Related events include server requests for hello same URL (if you've set up Application Insights on your web server).</span></span>

<span data-ttu-id="dba3a-178">**Výkon stránky v čase.**</span><span class="sxs-lookup"><span data-stu-id="dba3a-178">**Page performance over time.**</span></span> <span data-ttu-id="dba3a-179">Zpět v okně prohlížeče hello změňte hello zobrazení času načítání stránky mřížky do toosee grafu na řádku, pokud existuje v určitou dobu ke špičkám:</span><span class="sxs-lookup"><span data-stu-id="dba3a-179">Back at hello Browsers blade, change hello Page View Load Time grid into a line chart toosee if there were peaks at particular times:</span></span>

![Klikněte na tlačítko hello head hello mřížky a vyberte nový typ grafu](./media/app-insights-javascript/10-page-perf-area.png)

<span data-ttu-id="dba3a-181">**Rozdělení pomocí dalších dimenzí.**</span><span class="sxs-lookup"><span data-stu-id="dba3a-181">**Segment by other dimensions.**</span></span> <span data-ttu-id="dba3a-182">Vaše stránky se pomalejší tooload v určité lokalitě prohlížeče, klientského operačního systému nebo uživatele?</span><span class="sxs-lookup"><span data-stu-id="dba3a-182">Maybe your pages are slower tooload on a particular browser, client OS, or user locality?</span></span> <span data-ttu-id="dba3a-183">Přidejte nový graf a Experimentujte s hello **Seskupit podle** dimenze.</span><span class="sxs-lookup"><span data-stu-id="dba3a-183">Add a new chart and experiment with hello **Group-by** dimension.</span></span>

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a><span data-ttu-id="dba3a-184">Výkon AJAX</span><span class="sxs-lookup"><span data-stu-id="dba3a-184">AJAX Performance</span></span>
<span data-ttu-id="dba3a-185">Ujistěte se, že všechna volání AJAX na webových stránkách dobře fungují.</span><span class="sxs-lookup"><span data-stu-id="dba3a-185">Make sure any AJAX calls in your web pages are performing well.</span></span> <span data-ttu-id="dba3a-186">Jsou často používané toofill částí stránek asynchronně.</span><span class="sxs-lookup"><span data-stu-id="dba3a-186">They are often used toofill parts of your page asynchronously.</span></span> <span data-ttu-id="dba3a-187">I když hello celou stránku může načíst okamžitě, uživatelé mohou být frustrováni sledováním prázdných webových části čekání tooappear data v nich.</span><span class="sxs-lookup"><span data-stu-id="dba3a-187">Although hello overall page might load promptly, your users could be frustrated by staring at blank web parts, waiting for data tooappear in them.</span></span>

<span data-ttu-id="dba3a-188">Volání AJAX provedená z webové stránky se zobrazí v okně prohlížeče hello jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="dba3a-188">AJAX calls made from your web page are shown on hello Browsers blade as dependencies.</span></span>

<span data-ttu-id="dba3a-189">Nachází souhrnné grafy v horní části okna hello hello:</span><span class="sxs-lookup"><span data-stu-id="dba3a-189">There are summary charts in hello upper part of hello blade:</span></span>

![](./media/app-insights-javascript/31.png)

<span data-ttu-id="dba3a-190">a níže pak podrobné mřížky:</span><span class="sxs-lookup"><span data-stu-id="dba3a-190">and detailed grids lower down:</span></span>

![](./media/app-insights-javascript/33.png)

<span data-ttu-id="dba3a-191">Klikněte na libovolný řádek pro konkrétní podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="dba3a-191">Click any row for specific details.</span></span>

> [!NOTE]
> <span data-ttu-id="dba3a-192">Pokud odstraníte hello filtru prohlížečů v okně hello, server a závislosti AJAX zahrnuty do těchto grafů.</span><span class="sxs-lookup"><span data-stu-id="dba3a-192">If you delete hello Browsers filter on hello blade, both server and AJAX dependencies are included in these charts.</span></span> <span data-ttu-id="dba3a-193">Klikněte na tlačítko Obnovit výchozí nastavení filtru tooreconfigure hello.</span><span class="sxs-lookup"><span data-stu-id="dba3a-193">Click Restore Defaults tooreconfigure hello filter.</span></span>
> 
> 

<span data-ttu-id="dba3a-194">**toodrill do selhání volání Ajax** posuňte se dolů toohello mřížce selhání závislostí a pak klikněte na řádek určité instance toosee.</span><span class="sxs-lookup"><span data-stu-id="dba3a-194">**toodrill into failed Ajax calls** scroll down toohello Dependency failures grid, and then click a row toosee specific instances.</span></span>

![](./media/app-insights-javascript/37.png)


<span data-ttu-id="dba3a-195">Klikněte na tlačítko `...` pro úplnou telemetrii volání Ajax hello.</span><span class="sxs-lookup"><span data-stu-id="dba3a-195">Click `...` for hello full telemetry for an Ajax call.</span></span>

### <a name="no-ajax-calls-reported"></a><span data-ttu-id="dba3a-196">Žádná nahlášená volání Ajax?</span><span class="sxs-lookup"><span data-stu-id="dba3a-196">No Ajax calls reported?</span></span>
<span data-ttu-id="dba3a-197">Volání AJAX zahrnují HTTP a HTTPS volání ze skriptu hello webové stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-197">Ajax calls include any HTTP/HTTPS  calls made from hello script of your web page.</span></span> <span data-ttu-id="dba3a-198">Pokud je nevidíte nahlášená, zkontrolujte, že hello fragment kódu nenastavil hello `disableAjaxTracking` nebo `maxAjaxCallsPerView` [parametry](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="dba3a-198">If you don't see them reported, check that hello code snippet doesn't set hello `disableAjaxTracking` or `maxAjaxCallsPerView` [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="dba3a-199">Výjimky prohlížečů</span><span class="sxs-lookup"><span data-stu-id="dba3a-199">Browser exceptions</span></span>
<span data-ttu-id="dba3a-200">V okně prohlížeče hello je graf souhrnu výjimek a mřížka typů výjimek dolů hello okno.</span><span class="sxs-lookup"><span data-stu-id="dba3a-200">On hello Browsers blade, there's an exceptions summary chart, and a grid of exception types further down hello blade.</span></span>

![](./media/app-insights-javascript/39.png)

<span data-ttu-id="dba3a-201">Pokud nevidíte nahlášené výjimky prohlížeče, zkontrolujte, že hello fragment kódu nenastavil hello `disableExceptionTracking` [parametr](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="dba3a-201">If you don't see browser exceptions reported, check that hello code snippet doesn't set hello `disableExceptionTracking` [parameter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="inspect-individual-page-view-events"></a><span data-ttu-id="dba3a-202">Zkontrolujte jednotlivé stránky zobrazení událostí</span><span class="sxs-lookup"><span data-stu-id="dba3a-202">Inspect individual page view events</span></span>

<span data-ttu-id="dba3a-203">Obvykle jsou telemetrická zobrazení stránky analyzována pomocí Application Insights a zobrazí se pouze kumulativní sestavy s průměrem za všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="dba3a-203">Usually page view telemetry is analyzed by Application Insights and you see only cumulative reports, averaged over all your users.</span></span> <span data-ttu-id="dba3a-204">Ale pro účely ladění si můžete také prohlédnout jednotlivé stránky zobrazení událostí.</span><span class="sxs-lookup"><span data-stu-id="dba3a-204">But for debugging purposes, you can also look at individual page view events.</span></span>

<span data-ttu-id="dba3a-205">V okně diagnostické vyhledávání hello nastavte filtry tooPage zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dba3a-205">In hello Diagnostic Search blade, set Filters tooPage View.</span></span>

![](./media/app-insights-javascript/12-search-pages.png)

<span data-ttu-id="dba3a-206">Vyberte všechny události toosee podrobněji.</span><span class="sxs-lookup"><span data-stu-id="dba3a-206">Select any event toosee more detail.</span></span> <span data-ttu-id="dba3a-207">Na stránce Podrobnosti hello klikněte na tlačítko "..." toosee více podrobností.</span><span class="sxs-lookup"><span data-stu-id="dba3a-207">In hello details page, click "..." toosee even more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="dba3a-208">Pokud používáte [vyhledávání](app-insights-diagnostic-search.md), Všimněte si, že máte toomatch celá slova: "Abou" a "bout" neodpovídá "O".</span><span class="sxs-lookup"><span data-stu-id="dba3a-208">If you use [Search](app-insights-diagnostic-search.md), notice that you have toomatch whole words: "Abou" and "bout" do not match "About".</span></span>
> 
> 

<span data-ttu-id="dba3a-209">Můžete také použít hello výkonné [analýzy protokolů dotazu jazyka](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dba3a-209">You can also use hello powerful [Log Analytics query language](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch page views.</span></span>

### <a name="page-view-properties"></a><span data-ttu-id="dba3a-210">Zobrazení vlastností stránky</span><span class="sxs-lookup"><span data-stu-id="dba3a-210">Page view properties</span></span>
* <span data-ttu-id="dba3a-211">**Doba trvání zobrazení stránky**</span><span class="sxs-lookup"><span data-stu-id="dba3a-211">**Page view duration**</span></span> 
  
  * <span data-ttu-id="dba3a-212">Ve výchozím nastavení hello čas ho stránku hello tooload trvá, z klienta toofull žádost o načtení (včetně pomocných souborů, ale s výjimkou asynchronních úloh, jako je například volání Ajax).</span><span class="sxs-lookup"><span data-stu-id="dba3a-212">By default, hello time it takes tooload hello page, from client request toofull load (including auxiliary files but excluding asynchronous tasks such as Ajax calls).</span></span> 
  * <span data-ttu-id="dba3a-213">Pokud nastavíte `overridePageViewDuration` v hello [konfiguraci stránky](#detailed-configuration), nejprve hello interval mezi tooexecution požadavek klienta z hello `trackPageView`.</span><span class="sxs-lookup"><span data-stu-id="dba3a-213">If you set `overridePageViewDuration` in hello [page configuration](#detailed-configuration), hello interval between client request tooexecution of hello first `trackPageView`.</span></span> <span data-ttu-id="dba3a-214">Pokud jste přesunuli trackPageView z obvyklé pozice po inicializaci hello hello skriptu, bude odrážet odlišnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dba3a-214">If you moved trackPageView from its usual position after hello initialization of hello script, it will reflect a different value.</span></span>
  * <span data-ttu-id="dba3a-215">Pokud `overridePageViewDuration` je sada a dobu trvání argumentu k dispozici v hello `trackPageView()` volat, pak bude místo něj použita hodnota argumentu hello.</span><span class="sxs-lookup"><span data-stu-id="dba3a-215">If `overridePageViewDuration` is set and a duration argument is provided in hello `trackPageView()` call, then hello argument value is used instead.</span></span> 

## <a name="custom-page-counts"></a><span data-ttu-id="dba3a-216">Počty vlastních stránek</span><span class="sxs-lookup"><span data-stu-id="dba3a-216">Custom page counts</span></span>
<span data-ttu-id="dba3a-217">Ve výchozím nastavení počet stránek objeví vždy, když do hello prohlížeče klienta načte nová stránka.</span><span class="sxs-lookup"><span data-stu-id="dba3a-217">By default, a page count occurs each time a new page loads into hello client browser.</span></span>  <span data-ttu-id="dba3a-218">Ale můžete chtít toocount další zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="dba3a-218">But you might want toocount additional page views.</span></span> <span data-ttu-id="dba3a-219">Například stránka může zobrazit jeho obsah na kartách a chcete toocount na stránce, když hello uživatel přepíná karty.</span><span class="sxs-lookup"><span data-stu-id="dba3a-219">For example, a page might display its content in tabs and you want toocount a page when hello user switches tabs.</span></span> <span data-ttu-id="dba3a-220">Nebo kód jazyka JavaScript v hello stránka může načíst nový obsah beze změny adresy URL hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dba3a-220">Or JavaScript code in hello page might load new content without changing hello browser's URL.</span></span>

<span data-ttu-id="dba3a-221">Vložte podobné volání jazyka JavaScript v odpovídajícím bodě hello v klientském kódu:</span><span class="sxs-lookup"><span data-stu-id="dba3a-221">Insert a JavaScript call like this at hello appropriate point in your client code:</span></span>

    appInsights.trackPageView(myPageName);

<span data-ttu-id="dba3a-222">Název stránky Hello může obsahovat stejné znaky jako adresa URL, ale cokoli za "#" hello nebo "?" je ignorována.</span><span class="sxs-lookup"><span data-stu-id="dba3a-222">hello page name can contain hello same characters as a URL, but anything after "#" or "?" is ignored.</span></span>

## <a name="usage-tracking"></a><span data-ttu-id="dba3a-223">Sledování využití</span><span class="sxs-lookup"><span data-stu-id="dba3a-223">Usage tracking</span></span>
<span data-ttu-id="dba3a-224">Chcete toofind na co vaši uživatelé dělají s vaší aplikací?</span><span class="sxs-lookup"><span data-stu-id="dba3a-224">Want toofind out what your users do with your app?</span></span>

* [<span data-ttu-id="dba3a-225">Další informace o sledování využití</span><span class="sxs-lookup"><span data-stu-id="dba3a-225">Learn about usage tracking</span></span>](app-insights-web-track-usage.md)
* <span data-ttu-id="dba3a-226">[Další informace o vlastních událostech a metrikách rozhraní API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="dba3a-226">[Learn about custom events and metrics API](app-insights-api-custom-events-metrics.md).</span></span>

## <span data-ttu-id="dba3a-227"><a name="video"></a> Video</span><span class="sxs-lookup"><span data-stu-id="dba3a-227"><a name="video"></a> Video</span></span>


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <span data-ttu-id="dba3a-228"><a name="next"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="dba3a-228"><a name="next"></a> Next steps</span></span>
* [<span data-ttu-id="dba3a-229">Sledování využití</span><span class="sxs-lookup"><span data-stu-id="dba3a-229">Track usage</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="dba3a-230">Vlastní události a metriky</span><span class="sxs-lookup"><span data-stu-id="dba3a-230">Custom events and metrics</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="dba3a-231">Sestavení vyhodnocení poučení</span><span class="sxs-lookup"><span data-stu-id="dba3a-231">Build-measure-learn</span></span>](app-insights-web-track-usage.md)

