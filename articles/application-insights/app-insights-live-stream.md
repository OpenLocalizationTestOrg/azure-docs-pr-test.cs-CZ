---
title: "aaaLive metriky datový proud s vlastní metriky a diagnostiky ve službě Azure Application Insights | Microsoft Docs"
description: "Monitorování webové aplikace v reálném čase s vlastní metriky a diagnostikovat problémy s za provozu informační kanál selhání, trasování a události."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: bwren
ms.openlocfilehash: 68ddfbf387379ea778c20280c4ec96baa06d3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a><span data-ttu-id="6bae9-103">Živý datový proud metriky: Diagnostikujte s latencí 1 sekundu & monitorovat</span><span class="sxs-lookup"><span data-stu-id="6bae9-103">Live Metrics Stream: Monitor & Diagnose with 1-second latency</span></span> 

<span data-ttu-id="6bae9-104">Probe prezenční signál hello za provozu, v produkční webové aplikace pomocí živý datový proud metriky z [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6bae9-104">Probe hello beating heart of your live, in-production web application by using Live Metrics Stream from [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="6bae9-105">Vyberte a filtrovat metriky a výkonu toowatch čítačů v reálném čase, bez jakékoli služby tooyour narušení.</span><span class="sxs-lookup"><span data-stu-id="6bae9-105">Select and filter metrics and performance counters toowatch in real time, without any disturbance tooyour service.</span></span> <span data-ttu-id="6bae9-106">Zkontrolujte trasování zásobníku z ukázkové se nezdařilo požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="6bae9-106">Inspect stack traces from sample failed requests and exceptions.</span></span> <span data-ttu-id="6bae9-107">Společně s [profileru](app-insights-profiler.md), [ladicí program snímku](app-insights-snapshot-debugger.md), a [testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests), živý datový proud metriky poskytuje výkonný a neinvazivní nástroj pro diagnostiku pro váš web za provozu lokalita.</span><span class="sxs-lookup"><span data-stu-id="6bae9-107">Together with [Profiler](app-insights-profiler.md), [Snapshot debugger](app-insights-snapshot-debugger.md), and [performance testing](app-insights-monitor-web-app-availability.md#performance-tests),  Live Metrics Stream provides a powerful and non-invasive diagnostic tool for your live web site.</span></span>

<span data-ttu-id="6bae9-108">Živý datový proud metriky můžete:</span><span class="sxs-lookup"><span data-stu-id="6bae9-108">With Live Metrics Stream, you can:</span></span>

* <span data-ttu-id="6bae9-109">Ověření opravu, při jeho vydání, pomocí sledování výkonu a selhání počty.</span><span class="sxs-lookup"><span data-stu-id="6bae9-109">Validate a fix while it is released, by watching performance and failure counts.</span></span>
* <span data-ttu-id="6bae9-110">Podívejte se na hello účinku testování zatížení a diagnostikovat problémy za provozu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-110">Watch hello effect of test loads, and diagnose issues live.</span></span> 
* <span data-ttu-id="6bae9-111">Zaměřit na konkrétní test relací nebo odfiltrovat známých problémů, výběru a filtrování hello metriky, které chcete toowatch.</span><span class="sxs-lookup"><span data-stu-id="6bae9-111">Focus on particular test sessions or filter out known issues, by selecting and filtering hello metrics you want toowatch.</span></span>
* <span data-ttu-id="6bae9-112">Získáte výjimka trasování, při jejich provádění.</span><span class="sxs-lookup"><span data-stu-id="6bae9-112">Get exception traces as they happen.</span></span>
* <span data-ttu-id="6bae9-113">Experiment s filtry toofind hello nejdůležitější klíčových ukazatelů výkonu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-113">Experiment with filters toofind hello most relevant KPIs.</span></span>
* <span data-ttu-id="6bae9-114">Monitorování oknech výkonu čítač za provozu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-114">Monitor any Windows performance counter live.</span></span>
* <span data-ttu-id="6bae9-115">Snadno identifikovat server, který je problémy s a filtrovat všechny hello, za provozu nebo klíčového ukazatele výkonu informačního kanálu toojust tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="6bae9-115">Easily identify a server that is having issues, and filter all hello KPI/live feed toojust that server.</span></span>

<span data-ttu-id="6bae9-116">[![Za provozu metriky streamování videa](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span><span class="sxs-lookup"><span data-stu-id="6bae9-116">[![Live Metrics Stream video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span></span>

<span data-ttu-id="6bae9-117">Živý datový proud metriky je aktuálně k dispozici pro aplikace ASP.NET z běžících místně nebo v hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-117">Live Metrics Stream is currently available on ASP.NET apps running on-premises or in hello Cloud.</span></span> 

## <a name="get-started"></a><span data-ttu-id="6bae9-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6bae9-118">Get started</span></span>

1. <span data-ttu-id="6bae9-119">Pokud jste ještě [nenainstalovali Application Insights](app-insights-asp-net.md) ve vaší webové aplikaci ASP.NET nebo [aplikace pro Windows server](app-insights-windows-services.md), udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="6bae9-119">If you haven't yet [installed Application Insights](app-insights-asp-net.md) in your ASP.NET web app or [Windows server app](app-insights-windows-services.md), do that now.</span></span> 
2. <span data-ttu-id="6bae9-120">**Nejnovější verzi aktualizace toohello** hello Application Insights balíčku.</span><span class="sxs-lookup"><span data-stu-id="6bae9-120">**Update toohello latest version** of hello Application Insights package.</span></span> <span data-ttu-id="6bae9-121">V sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky Nuget balíčky**.</span><span class="sxs-lookup"><span data-stu-id="6bae9-121">In Visual Studio, right-click your project and choose **Manage Nuget packages**.</span></span> <span data-ttu-id="6bae9-122">Otevřete hello **aktualizace** zkontrolujte **zahrnout předběžné verze**a vybrat všechny balíčky Microsoft.ApplicationInsights.* hello.</span><span class="sxs-lookup"><span data-stu-id="6bae9-122">Open hello **Updates** tab, check **Include prerelease**, and select all hello Microsoft.ApplicationInsights.* packages.</span></span>

    <span data-ttu-id="6bae9-123">Znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6bae9-123">Redeploy your app.</span></span>

3. <span data-ttu-id="6bae9-124">V hello [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights hello pro vaši aplikaci a pak otevřete živý datový proud.</span><span class="sxs-lookup"><span data-stu-id="6bae9-124">In hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open Live Stream.</span></span>

4. <span data-ttu-id="6bae9-125">[Řídicí kanál zabezpečeného hello](#secure-the-control-channel) Pokud můžete použít citlivá data, jako jsou jména zákazníků v filtry.</span><span class="sxs-lookup"><span data-stu-id="6bae9-125">[Secure hello control channel](#secure-the-control-channel) if you might use sensitive data such as customer names in your filters.</span></span>


![V okně Přehled hello klikněte na tlačítko živý datový proud](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a><span data-ttu-id="6bae9-127">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="6bae9-127">No data?</span></span> <span data-ttu-id="6bae9-128">Zkontrolujte server brány firewall</span><span class="sxs-lookup"><span data-stu-id="6bae9-128">Check your server firewall</span></span>

<span data-ttu-id="6bae9-129">Zkontrolujte hello [Odchozí porty pro živý datový proud metriky](app-insights-ip-addresses.md#outgoing-ports) jsou otevřeny v bráně firewall hello vašich serverů.</span><span class="sxs-lookup"><span data-stu-id="6bae9-129">Check hello [outgoing ports for Live Metrics Stream](app-insights-ip-addresses.md#outgoing-ports) are open in hello firewall of your servers.</span></span> 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a><span data-ttu-id="6bae9-130">Jak živý datový proud metriky liší od Průzkumníku metrik a analýzy?</span><span class="sxs-lookup"><span data-stu-id="6bae9-130">How does Live Metrics Stream differ from Metrics Explorer and Analytics?</span></span>

| |<span data-ttu-id="6bae9-131">Live Stream</span><span class="sxs-lookup"><span data-stu-id="6bae9-131">Live Stream</span></span> | <span data-ttu-id="6bae9-132">Průzkumníku metrik a analýzy</span><span class="sxs-lookup"><span data-stu-id="6bae9-132">Metrics Explorer and Analytics</span></span> |
|---|---|---|
|<span data-ttu-id="6bae9-133">Latence</span><span class="sxs-lookup"><span data-stu-id="6bae9-133">Latency</span></span>|<span data-ttu-id="6bae9-134">Data zobrazená v rámci jedné sekundy</span><span class="sxs-lookup"><span data-stu-id="6bae9-134">Data displayed within one second</span></span>|<span data-ttu-id="6bae9-135">Agregován v minutách</span><span class="sxs-lookup"><span data-stu-id="6bae9-135">Aggregated over minutes</span></span>|
|<span data-ttu-id="6bae9-136">Žádné uchování</span><span class="sxs-lookup"><span data-stu-id="6bae9-136">No retention</span></span>|<span data-ttu-id="6bae9-137">Data udržuje je v grafu hello a potom je zahozen</span><span class="sxs-lookup"><span data-stu-id="6bae9-137">Data persists while it's on hello chart, and is then discarded</span></span>|[<span data-ttu-id="6bae9-138">Data uchovávat 90 dnů</span><span class="sxs-lookup"><span data-stu-id="6bae9-138">Data retained for 90 days</span></span>](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|<span data-ttu-id="6bae9-139">Na vyžádání</span><span class="sxs-lookup"><span data-stu-id="6bae9-139">On demand</span></span>|<span data-ttu-id="6bae9-140">Data je streamování při otevření metriky za provozu</span><span class="sxs-lookup"><span data-stu-id="6bae9-140">Data is streamed while you open Live Metrics</span></span>|<span data-ttu-id="6bae9-141">Data se odesílají vždycky, když je nainstalovaný a povolený hello SDK</span><span class="sxs-lookup"><span data-stu-id="6bae9-141">Data is sent whenever hello SDK is installed and enabled</span></span>|
|<span data-ttu-id="6bae9-142">Free</span><span class="sxs-lookup"><span data-stu-id="6bae9-142">Free</span></span>|<span data-ttu-id="6bae9-143">Je bezplatná živý datový proud dat</span><span class="sxs-lookup"><span data-stu-id="6bae9-143">There is no charge for Live Stream data</span></span>|<span data-ttu-id="6bae9-144">Subjektu příliš[ceny](app-insights-pricing.md)</span><span class="sxs-lookup"><span data-stu-id="6bae9-144">Subject too[pricing](app-insights-pricing.md)</span></span>
|<span data-ttu-id="6bae9-145">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="6bae9-145">Sampling</span></span>|<span data-ttu-id="6bae9-146">Přenášejí se všechny vybrané metriky a čítače.</span><span class="sxs-lookup"><span data-stu-id="6bae9-146">All selected metrics and counters are transmitted.</span></span> <span data-ttu-id="6bae9-147">Chyby a trasování zásobníku jsou odebírána data.</span><span class="sxs-lookup"><span data-stu-id="6bae9-147">Failures and stack traces are sampled.</span></span> <span data-ttu-id="6bae9-148">TelemetryProcessors se nepoužijí.</span><span class="sxs-lookup"><span data-stu-id="6bae9-148">TelemetryProcessors are not applied.</span></span>|<span data-ttu-id="6bae9-149">Události může být [vzorků](app-insights-api-filtering-sampling.md)</span><span class="sxs-lookup"><span data-stu-id="6bae9-149">Events may be [sampled](app-insights-api-filtering-sampling.md)</span></span>|
|<span data-ttu-id="6bae9-150">Řídicí kanál</span><span class="sxs-lookup"><span data-stu-id="6bae9-150">Control channel</span></span>|<span data-ttu-id="6bae9-151">Filtr řízení signálů toohello SDK.</span><span class="sxs-lookup"><span data-stu-id="6bae9-151">Filter control signals are sent toohello SDK.</span></span> <span data-ttu-id="6bae9-152">Doporučujeme [zabezpečit tento kanál](#secure-channel).</span><span class="sxs-lookup"><span data-stu-id="6bae9-152">We recommend you [secure this channel](#secure-channel).</span></span>|<span data-ttu-id="6bae9-153">Komunikace je jednosměrný, toohello portálu</span><span class="sxs-lookup"><span data-stu-id="6bae9-153">Communication is one-way, toohello portal</span></span>|


## <a name="select-and-filter-your-metrics"></a><span data-ttu-id="6bae9-154">Vyberte a filtrovat vaše metriky</span><span class="sxs-lookup"><span data-stu-id="6bae9-154">Select and filter your metrics</span></span>

<span data-ttu-id="6bae9-155">(K dispozici v klasickém aplikace ASP.NET s hello nejnovější sadu SDK.)</span><span class="sxs-lookup"><span data-stu-id="6bae9-155">(Available on classic ASP.NET apps with hello latest SDK.)</span></span>

<span data-ttu-id="6bae9-156">Vlastní klíčového ukazatele výkonu za provozu můžete monitorovat použitím libovolný filtry na všechny telemetrie Application Insights z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6bae9-156">You can monitor custom KPI live by applying arbitrary filters on any Application Insights telemetry from hello portal.</span></span> <span data-ttu-id="6bae9-157">Klikněte na ovládací prvek hello filtr, který ukazuje, když jste myší nad žádné hello grafů.</span><span class="sxs-lookup"><span data-stu-id="6bae9-157">Click hello filter control that shows when you mouse-over any of hello charts.</span></span> <span data-ttu-id="6bae9-158">Následující graf Hello je vykreslení vlastní počtu žádostí o klíčového ukazatele výkonu s filtry na adresu URL a doba trvání atributy.</span><span class="sxs-lookup"><span data-stu-id="6bae9-158">hello following chart is plotting a custom Request count KPI with filters on URL and Duration attributes.</span></span> <span data-ttu-id="6bae9-159">Ověřte filtry s hello oddíl Preview datový proud, který ukazuje za provozu informační kanál telemetrická data, která odpovídá hello kritéria, která jste zadali v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="6bae9-159">Validate your filters with hello Stream Preview section that shows a live feed of telemetry that matches hello criteria you have specified at any point in time.</span></span> 

![Vlastní požadavek klíčového ukazatele výkonu](./media/app-insights-live-stream/live-stream-filteredMetric.png)

<span data-ttu-id="6bae9-161">Hodnota, která se liší od počtu můžete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="6bae9-161">You can monitor a value different from Count.</span></span> <span data-ttu-id="6bae9-162">Hello možnosti závisí na typu hello datového proudu, který může být jakékoli telemetrie Application Insights: požadavky, závislostí, výjimek, trasování, události nebo metriky.</span><span class="sxs-lookup"><span data-stu-id="6bae9-162">hello options depend on hello type of stream, which could be any Application Insights telemetry: requests, dependencies, exceptions, traces, events, or metrics.</span></span> <span data-ttu-id="6bae9-163">Může být vlastní [vlastní měření](app-insights-api-custom-events-metrics.md#properties):</span><span class="sxs-lookup"><span data-stu-id="6bae9-163">It can be your own [custom measurement](app-insights-api-custom-events-metrics.md#properties):</span></span>

![Hodnota možnosti](./media/app-insights-live-stream/live-stream-valueoptions.png)

<span data-ttu-id="6bae9-165">Kromě toho tooApplication telemetrii Insights, můžete taky sledovat všechny čítačů výkonu systému Windows, výběrem z možností hello datového proudu a poskytování hello název čítače výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="6bae9-165">In addition tooApplication Insights telemetry, you can also monitor any Windows performance counter by selecting that from hello stream options, and providing hello name of hello performance counter.</span></span>

<span data-ttu-id="6bae9-166">Za provozu metriky agregován na dva body: místně na každém serveru a na všechny servery.</span><span class="sxs-lookup"><span data-stu-id="6bae9-166">Live metrics are aggregated at two points: locally on each server, and then across all servers.</span></span> <span data-ttu-id="6bae9-167">Můžete změnit výchozí hello v buď pomocí dalších možností v hello příslušných rozevírací seznamy.</span><span class="sxs-lookup"><span data-stu-id="6bae9-167">You can change hello default at either by selecting other options in hello respective drop-downs.</span></span>

## <a name="sample-telemetry-custom-live-diagnostic-events"></a><span data-ttu-id="6bae9-168">Ukázka Telemetrie: Vlastní za provozu diagnostických událostí</span><span class="sxs-lookup"><span data-stu-id="6bae9-168">Sample Telemetry: Custom Live Diagnostic Events</span></span>
<span data-ttu-id="6bae9-169">Ve výchozím nastavení zobrazuje hello za provozu informační kanál události ukázky neúspěšných požadavků a závislostí volání, výjimky, událostí a trasování.</span><span class="sxs-lookup"><span data-stu-id="6bae9-169">By default, hello live feed of events shows samples of failed requests and dependency calls, exceptions, events, and traces.</span></span> <span data-ttu-id="6bae9-170">Klikněte na tlačítko hello hello použít kritéria toosee ikonu filtru v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="6bae9-170">Click hello filter icon toosee hello applied criteria at any point in time.</span></span> 

![Výchozí živého kanálu](./media/app-insights-live-stream/live-stream-eventsdefault.png)

<span data-ttu-id="6bae9-172">Jako s metriky, můžete zadat všechny libovolný kritéria tooany typů telemetrie Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="6bae9-172">As with metrics, you can specify any arbitrary criteria tooany of hello Application Insights telemetry types.</span></span> <span data-ttu-id="6bae9-173">V tomto příkladu jsme se výběr konkrétního požadavku selhání, trasování a události.</span><span class="sxs-lookup"><span data-stu-id="6bae9-173">In this example, we are selecting specific request failures, traces, and events.</span></span> <span data-ttu-id="6bae9-174">Také jsme jsou vyberete všechny výjimky a selhání závislostí.</span><span class="sxs-lookup"><span data-stu-id="6bae9-174">We are also selecting all exceptions and dependency failures.</span></span>

![Vlastní za provozu informačního kanálu](./media/app-insights-live-stream/live-stream-events.png)

<span data-ttu-id="6bae9-176">Poznámka: v současné době pro výjimky na základě zpráv kritéria používáte zpráva o výjimce nejkrajnější hello.</span><span class="sxs-lookup"><span data-stu-id="6bae9-176">Note: Currently, for Exception message-based criteria, use hello outermost exception message.</span></span> <span data-ttu-id="6bae9-177">V předchozím příkladu hello toofilter out hello neškodné výjimka zpráva o vnitřní výjimce (způsobem hello "<--" oddělovač) "hello klient odpojen."</span><span class="sxs-lookup"><span data-stu-id="6bae9-177">In hello preceding example, toofilter out hello benign exception with inner exception message (follows hello "<--" delimiter) "hello client disconnected."</span></span> <span data-ttu-id="6bae9-178">použití zprávu není – obsahuje kritéria pro "Chyba při čtení obsahu žádosti".</span><span class="sxs-lookup"><span data-stu-id="6bae9-178">use a message not-contains "Error reading request content" criteria.</span></span>

<span data-ttu-id="6bae9-179">Zobrazit podrobnosti hello položky v hello kliknutím za provozu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-179">See hello details of an item in hello live feed by clicking it.</span></span> <span data-ttu-id="6bae9-180">Je možné pozastavit hello kanálu buď kliknutím **pozastavit** nebo jednoduše posouvání dolů, nebo kliknutím na položku.</span><span class="sxs-lookup"><span data-stu-id="6bae9-180">You can pause hello feed either by clicking **Pause** or simply scrolling down, or clicking an item.</span></span> <span data-ttu-id="6bae9-181">Za provozu informačního kanálu bude pokračovat po přejděte zpět toohello horní, nebo kliknutím hello Čítač položek shromážděných během byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="6bae9-181">Live feed will resume after you scroll back toohello top, or by clicking hello counter of items collected while it was paused.</span></span>

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a><span data-ttu-id="6bae9-183">Filtrovat podle instance serveru</span><span class="sxs-lookup"><span data-stu-id="6bae9-183">Filter by server instance</span></span>

<span data-ttu-id="6bae9-184">Pokud chcete toomonitor instanci role konkrétní server, můžete filtrovat podle serveru.</span><span class="sxs-lookup"><span data-stu-id="6bae9-184">If you want toomonitor a particular server role instance, you can filter by server.</span></span>

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a><span data-ttu-id="6bae9-186">Požadavky na sady SDK</span><span class="sxs-lookup"><span data-stu-id="6bae9-186">SDK Requirements</span></span>
<span data-ttu-id="6bae9-187">Vlastní živý datový proud metriky je k dispozici verze 2.4.0-beta2 nebo novější z [Application Insights SDK pro webové](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="6bae9-187">Custom Live Metrics Stream is available with version 2.4.0-beta2 or newer of [Application Insights SDK for web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="6bae9-188">Mějte na paměti, možnost "Zahrnout předprodejní verze" tooselect ze Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="6bae9-188">Remember tooselect "Include Prerelease" option from NuGet package manager.</span></span>

## <a name="secure-hello-control-channel"></a><span data-ttu-id="6bae9-189">Řídicí kanál zabezpečeného hello</span><span class="sxs-lookup"><span data-stu-id="6bae9-189">Secure hello control channel</span></span>
<span data-ttu-id="6bae9-190">Hello vlastní filtry kritéria, které zadáte jsou odesílány zpět toohello Live metriky součást hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="6bae9-190">hello custom filters criteria you specify are sent back toohello Live Metrics component in hello Application Insights SDK.</span></span> <span data-ttu-id="6bae9-191">filtry Hello mohou obsahovat citlivé informace, jako je například customerIDs.</span><span class="sxs-lookup"><span data-stu-id="6bae9-191">hello filters could potentially contain sensitive information such as customerIDs.</span></span> <span data-ttu-id="6bae9-192">Můžete nastavit hello kanál, zabezpečte pomocí tajný klíč rozhraní API kromě toohello klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="6bae9-192">You can make hello channel secure with a secret API key in addition toohello instrumentation key.</span></span>
### <a name="create-an-api-key"></a><span data-ttu-id="6bae9-193">Vytvořte klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6bae9-193">Create an API Key</span></span>

![Vytvořte klíč rozhraní api](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-tooconfiguration"></a><span data-ttu-id="6bae9-195">Přidat tooConfiguration klíče rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6bae9-195">Add API key tooConfiguration</span></span>
<span data-ttu-id="6bae9-196">V souboru hello souboru applicationinsights.config přidejte hello AuthenticationApiKey toohello QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="6bae9-196">In hello applicationinsights.config file, add hello AuthenticationApiKey toohello QuickPulseTelemetryModule:</span></span>
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
<span data-ttu-id="6bae9-197">Nebo v kódu, nastavte ji na hello QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="6bae9-197">Or in code, set it on hello QuickPulseTelemetryModule:</span></span>

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

<span data-ttu-id="6bae9-198">Pokud znáte a důvěřovat, že všechny propojené servery hello, můžete zkusit hello vlastní filtry bez hello ověřený kanálu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-198">However, if you recognize and trust all hello connected servers, you can try hello custom filters without hello authenticated channel.</span></span> <span data-ttu-id="6bae9-199">Tato možnost je k dispozici po dobu šesti měsíců.</span><span class="sxs-lookup"><span data-stu-id="6bae9-199">This option is available for six months.</span></span> <span data-ttu-id="6bae9-200">Toto přepsání je požadovaná jednou každou novou relaci, nebo při přechodu do režimu online na nový server.</span><span class="sxs-lookup"><span data-stu-id="6bae9-200">This override is required once every new session, or when a new server comes online.</span></span>

![Za provozu možnosti ověřování metriky](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
><span data-ttu-id="6bae9-202">Důrazně doporučujeme, abyste nastavili kanál hello ověření před vstupem potenciálně citlivé informace, jako je CustomerID v hello kritéria filtru.</span><span class="sxs-lookup"><span data-stu-id="6bae9-202">We strongly recommend that you set up hello authenticated channel before entering potentially sensitive information like CustomerID in hello filter criteria.</span></span>
>

## <a name="generating-a-performance-test-load"></a><span data-ttu-id="6bae9-203">Generování zátěžového testu výkonu</span><span class="sxs-lookup"><span data-stu-id="6bae9-203">Generating a performance test load</span></span>

<span data-ttu-id="6bae9-204">Pokud chcete toowatch hello účinek zatížení zvýšit, použijte hello testu výkonnosti okno.</span><span class="sxs-lookup"><span data-stu-id="6bae9-204">If you want toowatch hello effect of a load increase, use hello Performance Test blade.</span></span> <span data-ttu-id="6bae9-205">Simuluje požadavky od počet současně připojených uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6bae9-205">It simulates requests from a number of simultaneous users.</span></span> <span data-ttu-id="6bae9-206">Ho můžete spustit buď "ruční testy" (ping testy) jedné adresy URL, nebo můžete spustit [testu výkonnosti webu vícekrokový](app-insights-monitor-web-app-availability.md#multi-step-web-tests) odeslat (v hello stejný způsobem jako dostupnosti test).</span><span class="sxs-lookup"><span data-stu-id="6bae9-206">It can run either "manual tests" (ping tests) of a single URL, or it can run a [multi-step web performance test](app-insights-monitor-web-app-availability.md#multi-step-web-tests) that you upload (in hello same way as an availability test).</span></span>

> [!TIP]
> <span data-ttu-id="6bae9-207">Po vytvoření testu výkonnosti hello otevřete testovací hello a hello okno živý datový proud v samostatném systému windows.</span><span class="sxs-lookup"><span data-stu-id="6bae9-207">After you create hello performance test, open hello test and hello Live Stream blade in separate windows.</span></span> <span data-ttu-id="6bae9-208">Se zobrazí, když ve frontě hello spuštění testu výkonu a sledovat živý datový proud na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6bae9-208">You can see when hello queued performance test starts, and watch live stream at hello same time.</span></span>
>


## <a name="troubleshooting"></a><span data-ttu-id="6bae9-209">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6bae9-209">Troubleshooting</span></span>

<span data-ttu-id="6bae9-210">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="6bae9-210">No data?</span></span> <span data-ttu-id="6bae9-211">Pokud je aplikace v chráněná síťová: Live Stream metriky používá jinou IP adresy, než ostatní telemetrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6bae9-211">If your application is in a protected network: Live Metrics Stream uses a different IP addresses than other Application Insights telemetry.</span></span> <span data-ttu-id="6bae9-212">Zajistěte, aby [tyto IP adresy](app-insights-ip-addresses.md) jsou otevřeny v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="6bae9-212">Make sure [those IP addresses](app-insights-ip-addresses.md) are open in your firewall.</span></span>



## <a name="next-steps"></a><span data-ttu-id="6bae9-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6bae9-213">Next steps</span></span>
* [<span data-ttu-id="6bae9-214">Sledování použití s nástrojem Application Insights</span><span class="sxs-lookup"><span data-stu-id="6bae9-214">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="6bae9-215">Pomocí vyhledávání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="6bae9-215">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="6bae9-216">Profiler</span><span class="sxs-lookup"><span data-stu-id="6bae9-216">Profiler</span></span>](app-insights-profiler.md)
* [<span data-ttu-id="6bae9-217">Ladicí program snímku</span><span class="sxs-lookup"><span data-stu-id="6bae9-217">Snapshot debugger</span></span>](app-insights-snapshot-debugger.md)
