---
title: "Živý datový proud metriky s vlastní metriky a diagnostiky ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 1eb2e0c467d4fb4cb263047caf58d36231578d9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a><span data-ttu-id="8997d-103">Živý datový proud metriky: Diagnostikujte s latencí 1 sekundu & monitorovat</span><span class="sxs-lookup"><span data-stu-id="8997d-103">Live Metrics Stream: Monitor & Diagnose with 1-second latency</span></span> 

<span data-ttu-id="8997d-104">Probe prezenční signál za provozu, v produkční webové aplikace pomocí živý datový proud metriky z [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8997d-104">Probe the beating heart of your live, in-production web application by using Live Metrics Stream from [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="8997d-105">Vyberte a filtrovat metriky a výkonu čítače pro sledování v reálném čase, bez narušení k službě.</span><span class="sxs-lookup"><span data-stu-id="8997d-105">Select and filter metrics and performance counters to watch in real time, without any disturbance to your service.</span></span> <span data-ttu-id="8997d-106">Zkontrolujte trasování zásobníku z ukázkové se nezdařilo požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="8997d-106">Inspect stack traces from sample failed requests and exceptions.</span></span> <span data-ttu-id="8997d-107">Společně s [profileru](app-insights-profiler.md), [ladicí program snímku](app-insights-snapshot-debugger.md), a [testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests), živý datový proud metriky poskytuje výkonný a neinvazivní nástroj pro diagnostiku pro váš web za provozu lokalita.</span><span class="sxs-lookup"><span data-stu-id="8997d-107">Together with [Profiler](app-insights-profiler.md), [Snapshot debugger](app-insights-snapshot-debugger.md), and [performance testing](app-insights-monitor-web-app-availability.md#performance-tests),  Live Metrics Stream provides a powerful and non-invasive diagnostic tool for your live web site.</span></span>

<span data-ttu-id="8997d-108">Živý datový proud metriky můžete:</span><span class="sxs-lookup"><span data-stu-id="8997d-108">With Live Metrics Stream, you can:</span></span>

* <span data-ttu-id="8997d-109">Ověření opravu, při jeho vydání, pomocí sledování výkonu a selhání počty.</span><span class="sxs-lookup"><span data-stu-id="8997d-109">Validate a fix while it is released, by watching performance and failure counts.</span></span>
* <span data-ttu-id="8997d-110">Sledování účinku testování zatížení a diagnostikovat problémy za provozu.</span><span class="sxs-lookup"><span data-stu-id="8997d-110">Watch the effect of test loads, and diagnose issues live.</span></span> 
* <span data-ttu-id="8997d-111">Zaměřit na konkrétní test relací nebo odfiltrovat známých problémů, výběru a filtrování metriku, kterou chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="8997d-111">Focus on particular test sessions or filter out known issues, by selecting and filtering the metrics you want to watch.</span></span>
* <span data-ttu-id="8997d-112">Získáte výjimka trasování, při jejich provádění.</span><span class="sxs-lookup"><span data-stu-id="8997d-112">Get exception traces as they happen.</span></span>
* <span data-ttu-id="8997d-113">Experimentujte s filtry najít nejdůležitější klíčových ukazatelů výkonu.</span><span class="sxs-lookup"><span data-stu-id="8997d-113">Experiment with filters to find the most relevant KPIs.</span></span>
* <span data-ttu-id="8997d-114">Monitorování oknech výkonu čítač za provozu.</span><span class="sxs-lookup"><span data-stu-id="8997d-114">Monitor any Windows performance counter live.</span></span>
* <span data-ttu-id="8997d-115">Snadno Identifikujte server, který má problémy a filtr, který všechny klíčový ukazatel výkonu nebo live kanálu právě daného serveru.</span><span class="sxs-lookup"><span data-stu-id="8997d-115">Easily identify a server that is having issues, and filter all the KPI/live feed to just that server.</span></span>

<span data-ttu-id="8997d-116">[![Za provozu metriky streamování videa](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span><span class="sxs-lookup"><span data-stu-id="8997d-116">[![Live Metrics Stream video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span></span>

<span data-ttu-id="8997d-117">Živý datový proud metriky je aktuálně k dispozici pro aplikace ASP.NET z běžících místně nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="8997d-117">Live Metrics Stream is currently available on ASP.NET apps running on-premises or in the Cloud.</span></span> 

## <a name="get-started"></a><span data-ttu-id="8997d-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="8997d-118">Get started</span></span>

1. <span data-ttu-id="8997d-119">Pokud jste ještě [nenainstalovali Application Insights](app-insights-asp-net.md) ve vaší webové aplikaci ASP.NET nebo [aplikace pro Windows server](app-insights-windows-services.md), udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="8997d-119">If you haven't yet [installed Application Insights](app-insights-asp-net.md) in your ASP.NET web app or [Windows server app](app-insights-windows-services.md), do that now.</span></span> 
2. <span data-ttu-id="8997d-120">**Aktualizace na nejnovější verzi** balíčku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8997d-120">**Update to the latest version** of the Application Insights package.</span></span> <span data-ttu-id="8997d-121">V sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky Nuget balíčky**.</span><span class="sxs-lookup"><span data-stu-id="8997d-121">In Visual Studio, right-click your project and choose **Manage Nuget packages**.</span></span> <span data-ttu-id="8997d-122">Otevřete **aktualizace** zkontrolujte **zahrnout předběžné verze**a vybrat všechny balíčky Microsoft.ApplicationInsights.*.</span><span class="sxs-lookup"><span data-stu-id="8997d-122">Open the **Updates** tab, check **Include prerelease**, and select all the Microsoft.ApplicationInsights.* packages.</span></span>

    <span data-ttu-id="8997d-123">Znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8997d-123">Redeploy your app.</span></span>

3. <span data-ttu-id="8997d-124">V [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights pro vaši aplikaci a pak otevřete živý datový proud.</span><span class="sxs-lookup"><span data-stu-id="8997d-124">In the [Azure portal](https://portal.azure.com), open the Application Insights resource for your app, and then open Live Stream.</span></span>

4. <span data-ttu-id="8997d-125">[Zabezpečený kanál řízení](#secure-the-control-channel) Pokud můžete použít citlivá data, jako jsou jména zákazníků v filtry.</span><span class="sxs-lookup"><span data-stu-id="8997d-125">[Secure the control channel](#secure-the-control-channel) if you might use sensitive data such as customer names in your filters.</span></span>


![V okně Přehled klikněte na živý datový proud](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a><span data-ttu-id="8997d-127">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="8997d-127">No data?</span></span> <span data-ttu-id="8997d-128">Zkontrolujte server brány firewall</span><span class="sxs-lookup"><span data-stu-id="8997d-128">Check your server firewall</span></span>

<span data-ttu-id="8997d-129">Zkontrolujte [Odchozí porty pro živý datový proud metriky](app-insights-ip-addresses.md#outgoing-ports) jsou otevřeny v bráně firewall vašich serverů.</span><span class="sxs-lookup"><span data-stu-id="8997d-129">Check the [outgoing ports for Live Metrics Stream](app-insights-ip-addresses.md#outgoing-ports) are open in the firewall of your servers.</span></span> 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a><span data-ttu-id="8997d-130">Jak živý datový proud metriky liší od Průzkumníku metrik a analýzy?</span><span class="sxs-lookup"><span data-stu-id="8997d-130">How does Live Metrics Stream differ from Metrics Explorer and Analytics?</span></span>

| |<span data-ttu-id="8997d-131">Živý datový proud</span><span class="sxs-lookup"><span data-stu-id="8997d-131">Live Stream</span></span> | <span data-ttu-id="8997d-132">Průzkumníku metrik a analýzy</span><span class="sxs-lookup"><span data-stu-id="8997d-132">Metrics Explorer and Analytics</span></span> |
|---|---|---|
|<span data-ttu-id="8997d-133">Latence</span><span class="sxs-lookup"><span data-stu-id="8997d-133">Latency</span></span>|<span data-ttu-id="8997d-134">Data zobrazená v rámci jedné sekundy</span><span class="sxs-lookup"><span data-stu-id="8997d-134">Data displayed within one second</span></span>|<span data-ttu-id="8997d-135">Agregován v minutách</span><span class="sxs-lookup"><span data-stu-id="8997d-135">Aggregated over minutes</span></span>|
|<span data-ttu-id="8997d-136">Žádné uchování</span><span class="sxs-lookup"><span data-stu-id="8997d-136">No retention</span></span>|<span data-ttu-id="8997d-137">Data udržuje je na graf a potom je zahozen</span><span class="sxs-lookup"><span data-stu-id="8997d-137">Data persists while it's on the chart, and is then discarded</span></span>|[<span data-ttu-id="8997d-138">Data uchovávat 90 dnů</span><span class="sxs-lookup"><span data-stu-id="8997d-138">Data retained for 90 days</span></span>](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|<span data-ttu-id="8997d-139">Na vyžádání</span><span class="sxs-lookup"><span data-stu-id="8997d-139">On demand</span></span>|<span data-ttu-id="8997d-140">Data je streamování při otevření metriky za provozu</span><span class="sxs-lookup"><span data-stu-id="8997d-140">Data is streamed while you open Live Metrics</span></span>|<span data-ttu-id="8997d-141">Data se odesílají vždy, když je nainstalované a povolené sady SDK</span><span class="sxs-lookup"><span data-stu-id="8997d-141">Data is sent whenever the SDK is installed and enabled</span></span>|
|<span data-ttu-id="8997d-142">Free</span><span class="sxs-lookup"><span data-stu-id="8997d-142">Free</span></span>|<span data-ttu-id="8997d-143">Je bezplatná živý datový proud dat</span><span class="sxs-lookup"><span data-stu-id="8997d-143">There is no charge for Live Stream data</span></span>|<span data-ttu-id="8997d-144">Podléhají [ceny](app-insights-pricing.md)</span><span class="sxs-lookup"><span data-stu-id="8997d-144">Subject to [pricing](app-insights-pricing.md)</span></span>
|<span data-ttu-id="8997d-145">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="8997d-145">Sampling</span></span>|<span data-ttu-id="8997d-146">Přenášejí se všechny vybrané metriky a čítače.</span><span class="sxs-lookup"><span data-stu-id="8997d-146">All selected metrics and counters are transmitted.</span></span> <span data-ttu-id="8997d-147">Chyby a trasování zásobníku jsou odebírána data.</span><span class="sxs-lookup"><span data-stu-id="8997d-147">Failures and stack traces are sampled.</span></span> <span data-ttu-id="8997d-148">TelemetryProcessors se nepoužijí.</span><span class="sxs-lookup"><span data-stu-id="8997d-148">TelemetryProcessors are not applied.</span></span>|<span data-ttu-id="8997d-149">Události může být [vzorků](app-insights-api-filtering-sampling.md)</span><span class="sxs-lookup"><span data-stu-id="8997d-149">Events may be [sampled](app-insights-api-filtering-sampling.md)</span></span>|
|<span data-ttu-id="8997d-150">Řídicí kanál</span><span class="sxs-lookup"><span data-stu-id="8997d-150">Control channel</span></span>|<span data-ttu-id="8997d-151">Filtr řízení signálů k sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="8997d-151">Filter control signals are sent to the SDK.</span></span> <span data-ttu-id="8997d-152">Doporučujeme [zabezpečit tento kanál](#secure-channel).</span><span class="sxs-lookup"><span data-stu-id="8997d-152">We recommend you [secure this channel](#secure-channel).</span></span>|<span data-ttu-id="8997d-153">Komunikace je jednosměrný k portálu</span><span class="sxs-lookup"><span data-stu-id="8997d-153">Communication is one-way, to the portal</span></span>|


## <a name="select-and-filter-your-metrics"></a><span data-ttu-id="8997d-154">Vyberte a filtrovat vaše metriky</span><span class="sxs-lookup"><span data-stu-id="8997d-154">Select and filter your metrics</span></span>

<span data-ttu-id="8997d-155">(K dispozici na klasické aplikace ASP.NET s nejnovější sadu SDK).</span><span class="sxs-lookup"><span data-stu-id="8997d-155">(Available on classic ASP.NET apps with the latest SDK.)</span></span>

<span data-ttu-id="8997d-156">Vlastní klíčového ukazatele výkonu za provozu můžete monitorovat použitím libovolný filtry na všechny telemetrie Application Insights z portálu.</span><span class="sxs-lookup"><span data-stu-id="8997d-156">You can monitor custom KPI live by applying arbitrary filters on any Application Insights telemetry from the portal.</span></span> <span data-ttu-id="8997d-157">Klikněte na ovládací prvek filtru, který ukazuje, když jste myší nad žádné grafy.</span><span class="sxs-lookup"><span data-stu-id="8997d-157">Click the filter control that shows when you mouse-over any of the charts.</span></span> <span data-ttu-id="8997d-158">Následující graf je vykreslení vlastní počtu žádostí o klíčového ukazatele výkonu s filtry na adresu URL a doba trvání atributy.</span><span class="sxs-lookup"><span data-stu-id="8997d-158">The following chart is plotting a custom Request count KPI with filters on URL and Duration attributes.</span></span> <span data-ttu-id="8997d-159">Ověřte filtry s oddílu Preview datový proud, který ukazuje za provozu informační kanál telemetrická data, která odpovídá kritéria, která jste zadali v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="8997d-159">Validate your filters with the Stream Preview section that shows a live feed of telemetry that matches the criteria you have specified at any point in time.</span></span> 

![Vlastní požadavek klíčového ukazatele výkonu](./media/app-insights-live-stream/live-stream-filteredMetric.png)

<span data-ttu-id="8997d-161">Hodnota, která se liší od počtu můžete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="8997d-161">You can monitor a value different from Count.</span></span> <span data-ttu-id="8997d-162">Možnosti závisí na typu datový proud, který může být jakékoli telemetrie Application Insights: požadavky, závislostí, výjimek, trasování, události nebo metriky.</span><span class="sxs-lookup"><span data-stu-id="8997d-162">The options depend on the type of stream, which could be any Application Insights telemetry: requests, dependencies, exceptions, traces, events, or metrics.</span></span> <span data-ttu-id="8997d-163">Může být vlastní [vlastní měření](app-insights-api-custom-events-metrics.md#properties):</span><span class="sxs-lookup"><span data-stu-id="8997d-163">It can be your own [custom measurement](app-insights-api-custom-events-metrics.md#properties):</span></span>

![Hodnota možnosti](./media/app-insights-live-stream/live-stream-valueoptions.png)

<span data-ttu-id="8997d-165">Kromě telemetrie Application Insights můžete také sledovat všech čítačů výkonu systému Windows, vyberte jednu z možností datového proudu a poskytnutím název čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="8997d-165">In addition to Application Insights telemetry, you can also monitor any Windows performance counter by selecting that from the stream options, and providing the name of the performance counter.</span></span>

<span data-ttu-id="8997d-166">Za provozu metriky agregován na dva body: místně na každém serveru a na všechny servery.</span><span class="sxs-lookup"><span data-stu-id="8997d-166">Live metrics are aggregated at two points: locally on each server, and then across all servers.</span></span> <span data-ttu-id="8997d-167">Můžete změnit výchozí nastavení v buď pomocí dalších možností v příslušných rozevírací seznamy.</span><span class="sxs-lookup"><span data-stu-id="8997d-167">You can change the default at either by selecting other options in the respective drop-downs.</span></span>

## <a name="sample-telemetry-custom-live-diagnostic-events"></a><span data-ttu-id="8997d-168">Ukázka Telemetrie: Vlastní za provozu diagnostických událostí</span><span class="sxs-lookup"><span data-stu-id="8997d-168">Sample Telemetry: Custom Live Diagnostic Events</span></span>
<span data-ttu-id="8997d-169">Ve výchozím nastavení za provozu informačního kanálu událostí zobrazuje ukázky neúspěšných požadavků a závislostí volání, výjimky, událostí a trasování.</span><span class="sxs-lookup"><span data-stu-id="8997d-169">By default, the live feed of events shows samples of failed requests and dependency calls, exceptions, events, and traces.</span></span> <span data-ttu-id="8997d-170">Klikněte na ikonu filtru zobrazit kritéria použitá v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="8997d-170">Click the filter icon to see the applied criteria at any point in time.</span></span> 

![Výchozí živého kanálu](./media/app-insights-live-stream/live-stream-eventsdefault.png)

<span data-ttu-id="8997d-172">Jako s metriky, můžete zadat všechny libovolný kritéria pro jakýkoli z typů telemetrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8997d-172">As with metrics, you can specify any arbitrary criteria to any of the Application Insights telemetry types.</span></span> <span data-ttu-id="8997d-173">V tomto příkladu jsme se výběr konkrétního požadavku selhání, trasování a události.</span><span class="sxs-lookup"><span data-stu-id="8997d-173">In this example, we are selecting specific request failures, traces, and events.</span></span> <span data-ttu-id="8997d-174">Také jsme jsou vyberete všechny výjimky a selhání závislostí.</span><span class="sxs-lookup"><span data-stu-id="8997d-174">We are also selecting all exceptions and dependency failures.</span></span>

![Vlastní za provozu informačního kanálu](./media/app-insights-live-stream/live-stream-events.png)

<span data-ttu-id="8997d-176">Poznámka: v současné době pro výjimky na základě zpráv kritéria používáte zpráva nejkrajnější výjimky.</span><span class="sxs-lookup"><span data-stu-id="8997d-176">Note: Currently, for Exception message-based criteria, use the outermost exception message.</span></span> <span data-ttu-id="8997d-177">V předchozím příkladu, filtrovat neškodné výjimky s zpráva o vnitřní výjimce (odpovídá "<--" oddělovač) "klient odpojen."</span><span class="sxs-lookup"><span data-stu-id="8997d-177">In the preceding example, to filter out the benign exception with inner exception message (follows the "<--" delimiter) "The client disconnected."</span></span> <span data-ttu-id="8997d-178">použití zprávu není – obsahuje kritéria pro "Chyba při čtení obsahu žádosti".</span><span class="sxs-lookup"><span data-stu-id="8997d-178">use a message not-contains "Error reading request content" criteria.</span></span>

<span data-ttu-id="8997d-179">Kliknutím zobrazit podrobnosti o položce v za provozu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="8997d-179">See the details of an item in the live feed by clicking it.</span></span> <span data-ttu-id="8997d-180">Je možné pozastavit informačního kanálu, buď kliknutím **pozastavit** nebo jednoduše posouvání dolů, nebo kliknutím na položku.</span><span class="sxs-lookup"><span data-stu-id="8997d-180">You can pause the feed either by clicking **Pause** or simply scrolling down, or clicking an item.</span></span> <span data-ttu-id="8997d-181">Za provozu kanálu bude pokračovat po přejděte zpět na horní, nebo klikněte na čítač položek shromážděných během byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="8997d-181">Live feed will resume after you scroll back to the top, or by clicking the counter of items collected while it was paused.</span></span>

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a><span data-ttu-id="8997d-183">Filtrovat podle instance serveru</span><span class="sxs-lookup"><span data-stu-id="8997d-183">Filter by server instance</span></span>

<span data-ttu-id="8997d-184">Pokud chcete monitorovat instanci role konkrétní server, můžete filtrovat podle serveru.</span><span class="sxs-lookup"><span data-stu-id="8997d-184">If you want to monitor a particular server role instance, you can filter by server.</span></span>

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a><span data-ttu-id="8997d-186">Požadavky na sady SDK</span><span class="sxs-lookup"><span data-stu-id="8997d-186">SDK Requirements</span></span>
<span data-ttu-id="8997d-187">Vlastní živý datový proud metriky je k dispozici verze 2.4.0-beta2 nebo novější z [Application Insights SDK pro webové](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="8997d-187">Custom Live Metrics Stream is available with version 2.4.0-beta2 or newer of [Application Insights SDK for web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="8997d-188">Nezapomeňte vybrat možnost "Zahrnout předprodejní verze" ze Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="8997d-188">Remember to select "Include Prerelease" option from NuGet package manager.</span></span>

## <a name="secure-the-control-channel"></a><span data-ttu-id="8997d-189">Zabezpečený kanál ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="8997d-189">Secure the control channel</span></span>
<span data-ttu-id="8997d-190">Vlastní kritéria filtry, které zadáte jsou odesílány zpět za provozu metriky součástí Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="8997d-190">The custom filters criteria you specify are sent back to the Live Metrics component in the Application Insights SDK.</span></span> <span data-ttu-id="8997d-191">Filtry mohou obsahovat citlivé informace, jako je například customerIDs.</span><span class="sxs-lookup"><span data-stu-id="8997d-191">The filters could potentially contain sensitive information such as customerIDs.</span></span> <span data-ttu-id="8997d-192">Kanál můžete zabezpečit s tajným klíčem rozhraní API kromě klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="8997d-192">You can make the channel secure with a secret API key in addition to the instrumentation key.</span></span>
### <a name="create-an-api-key"></a><span data-ttu-id="8997d-193">Vytvořte klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8997d-193">Create an API Key</span></span>

![Vytvořte klíč rozhraní api](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a><span data-ttu-id="8997d-195">Přidat klíč rozhraní API do konfigurace</span><span class="sxs-lookup"><span data-stu-id="8997d-195">Add API key to Configuration</span></span>
<span data-ttu-id="8997d-196">V souboru applicationinsights.config soubor přidejte AuthenticationApiKey QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="8997d-196">In the applicationinsights.config file, add the AuthenticationApiKey to the QuickPulseTelemetryModule:</span></span>
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
<span data-ttu-id="8997d-197">Nebo v kódu, nastavte ji na QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="8997d-197">Or in code, set it on the QuickPulseTelemetryModule:</span></span>

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

<span data-ttu-id="8997d-198">Pokud znáte a důvěřujete všechny propojené servery, můžete zkusit vlastní filtry bez ověřené kanál.</span><span class="sxs-lookup"><span data-stu-id="8997d-198">However, if you recognize and trust all the connected servers, you can try the custom filters without the authenticated channel.</span></span> <span data-ttu-id="8997d-199">Tato možnost je k dispozici po dobu šesti měsíců.</span><span class="sxs-lookup"><span data-stu-id="8997d-199">This option is available for six months.</span></span> <span data-ttu-id="8997d-200">Toto přepsání je požadovaná jednou každou novou relaci, nebo při přechodu do režimu online na nový server.</span><span class="sxs-lookup"><span data-stu-id="8997d-200">This override is required once every new session, or when a new server comes online.</span></span>

![Za provozu možnosti ověřování metriky](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
><span data-ttu-id="8997d-202">Důrazně doporučujeme, abyste nastavili ověřené kanál před vstupem potenciálně citlivé informace, jako je CustomerID v kritéria filtru.</span><span class="sxs-lookup"><span data-stu-id="8997d-202">We strongly recommend that you set up the authenticated channel before entering potentially sensitive information like CustomerID in the filter criteria.</span></span>
>

## <a name="generating-a-performance-test-load"></a><span data-ttu-id="8997d-203">Generování zátěžového testu výkonu</span><span class="sxs-lookup"><span data-stu-id="8997d-203">Generating a performance test load</span></span>

<span data-ttu-id="8997d-204">Pokud chcete sledovat účinek zvýšení zatížení, použijte okno Test výkonu.</span><span class="sxs-lookup"><span data-stu-id="8997d-204">If you want to watch the effect of a load increase, use the Performance Test blade.</span></span> <span data-ttu-id="8997d-205">Simuluje požadavky od počet současně připojených uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8997d-205">It simulates requests from a number of simultaneous users.</span></span> <span data-ttu-id="8997d-206">Můžete ho spustit buď "ruční testy" (ping testy) jedné adresy URL, nebo můžete spustit [vícekrokového webového testu výkonnosti](app-insights-monitor-web-app-availability.md#multi-step-web-tests) nahraného (stejným způsobem jako test dostupnosti).</span><span class="sxs-lookup"><span data-stu-id="8997d-206">It can run either "manual tests" (ping tests) of a single URL, or it can run a [multi-step web performance test](app-insights-monitor-web-app-availability.md#multi-step-web-tests) that you upload (in the same way as an availability test).</span></span>

> [!TIP]
> <span data-ttu-id="8997d-207">Po vytvoření testu výkonnosti otevřete test a v okně živý datový proud v samostatném systému windows.</span><span class="sxs-lookup"><span data-stu-id="8997d-207">After you create the performance test, open the test and the Live Stream blade in separate windows.</span></span> <span data-ttu-id="8997d-208">Se zobrazí při spuštění testu výkonu zařazených do fronty a sledovat živý datový proud ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="8997d-208">You can see when the queued performance test starts, and watch live stream at the same time.</span></span>
>


## <a name="troubleshooting"></a><span data-ttu-id="8997d-209">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="8997d-209">Troubleshooting</span></span>

<span data-ttu-id="8997d-210">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="8997d-210">No data?</span></span> <span data-ttu-id="8997d-211">Pokud je aplikace v chráněná síťová: Live Stream metriky používá jinou IP adresy, než ostatní telemetrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8997d-211">If your application is in a protected network: Live Metrics Stream uses a different IP addresses than other Application Insights telemetry.</span></span> <span data-ttu-id="8997d-212">Zajistěte, aby [tyto IP adresy](app-insights-ip-addresses.md) jsou otevřeny v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="8997d-212">Make sure [those IP addresses](app-insights-ip-addresses.md) are open in your firewall.</span></span>



## <a name="next-steps"></a><span data-ttu-id="8997d-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8997d-213">Next steps</span></span>
* [<span data-ttu-id="8997d-214">Sledování použití s nástrojem Application Insights</span><span class="sxs-lookup"><span data-stu-id="8997d-214">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="8997d-215">Pomocí vyhledávání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="8997d-215">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="8997d-216">Profiler</span><span class="sxs-lookup"><span data-stu-id="8997d-216">Profiler</span></span>](app-insights-profiler.md)
* [<span data-ttu-id="8997d-217">Ladicí program snímku</span><span class="sxs-lookup"><span data-stu-id="8997d-217">Snapshot debugger</span></span>](app-insights-snapshot-debugger.md)
