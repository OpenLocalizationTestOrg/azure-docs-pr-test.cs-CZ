---
title: "Řídicí panely a navigace ve službě Azure Application Insights | Microsoft Docs"
description: "Vytvořte zobrazení klíče APM grafy a dotazů."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9987f04e7e71df5fe10c8bc209a390cb940ec4f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a><span data-ttu-id="bfc65-103">Navigační a řídicích panelů v portálu služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="bfc65-103">Navigation and Dashboards in the Application Insights portal</span></span>
<span data-ttu-id="bfc65-104">Až budete mít [nastavte Application Insights na projektu](app-insights-overview.md), telemetrická data o výkonu a využití vaší aplikace se zobrazí v projektu na prostředek Application Insights [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bfc65-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in the [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="bfc65-105">Najít telemetrii</span><span class="sxs-lookup"><span data-stu-id="bfc65-105">Find your telemetry</span></span>
<span data-ttu-id="bfc65-106">Přihlaste se k [portál Azure](https://portal.azure.com) a přejděte do prostředku Application Insights, kterou jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bfc65-106">Sign in to the [Azure portal](https://portal.azure.com) and navigate to the Application Insights resource that you created for your app.</span></span>

![Klikněte na tlačítko Procházet, vyberte Application Insights a pak vaší aplikace.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="bfc65-108">V okně Přehled (stránky) pro vaši aplikaci se zobrazí souhrn klíčových diagnostiky metrik vaší aplikace a je brány do dalších funkcí portálu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-108">The overview blade (page) for your app shows a summary of the key diagnostic metrics of your app, and is a gateway to the other features of the portal.</span></span>

![Hlavní trasy k zobrazení telemetrie](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="bfc65-110">Můžete přizpůsobit kterékoli z grafy a mřížky a jejich Připnutí na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bfc65-110">You can customize any of the charts and grids and pin them to a dashboard.</span></span> <span data-ttu-id="bfc65-111">Tímto způsobem můžete zahrnout společně klíče telemetrii z různých aplikací Centrální řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bfc65-111">That way, you can bring together the key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="bfc65-112">Řídicí panely</span><span class="sxs-lookup"><span data-stu-id="bfc65-112">Dashboards</span></span>
<span data-ttu-id="bfc65-113">První věc, se zobrazí po přihlášení k [portálu Microsoft Azure](https://portal.azure.com) je řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bfc65-113">The first thing you see after you sign in to the [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="bfc65-114">Zde můžete najednou přenést grafů, které jsou pro vás nejdůležitější mezi všechny vaše prostředky Azure, včetně telemetrie z [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-114">Here you can bring together the charts that are most important to you across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![Přizpůsobený řídicí panel.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="bfc65-116">**Přejděte ke konkrétním prostředkům** například vaše aplikace ve službě Application Insights: použijte na levém panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-116">**Navigate to specific resources** such as your app in Application Insights: Use the left bar.</span></span>
2. <span data-ttu-id="bfc65-117">**Vrátit na aktuální řídicí panel**, nebo přepnout do jiných poslední zobrazení: použijte rozevírací nabídce nahoře vlevo.</span><span class="sxs-lookup"><span data-stu-id="bfc65-117">**Return to the current dashboard**, or switch to other recent views: Use the drop-down menu at top left.</span></span>
3. <span data-ttu-id="bfc65-118">**Přepínač řídicí panely**: použijte rozevírací nabídky na název řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="bfc65-118">**Switch dashboards**: Use the drop-down menu on the dashboard title</span></span>
4. <span data-ttu-id="bfc65-119">**Vytvářet, upravovat a sdílet řídicí panely** na řídicím panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="bfc65-119">**Create, edit, and share dashboards** in the dashboard toolbar.</span></span>
5. <span data-ttu-id="bfc65-120">**Upravit řídicí panel**: najeďte na dlaždici a potom pomocí jeho horním panelu přesunout, přizpůsobení nebo ji odeberte.</span><span class="sxs-lookup"><span data-stu-id="bfc65-120">**Edit the dashboard**: Hover over a tile and then use its top bar to move, customize, or remove it.</span></span>

## <a name="add-to-a-dashboard"></a><span data-ttu-id="bfc65-121">Přidat na řídicí panel</span><span class="sxs-lookup"><span data-stu-id="bfc65-121">Add to a dashboard</span></span>
<span data-ttu-id="bfc65-122">Když se díváte na okno nebo sadu grafů, který je zajímavé, budete moct připnout kopie ho na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bfc65-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it to the dashboard.</span></span> <span data-ttu-id="bfc65-123">Se zobrazí při příštím vrátíte existuje.</span><span class="sxs-lookup"><span data-stu-id="bfc65-123">You'll see it next time you return there.</span></span>

![Připnete graf, pozastavte ukazatel myši nad ním a pak klikněte na tlačítko "..." v záhlaví.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="bfc65-125">PIN kód graf na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bfc65-125">Pin chart to dashboard.</span></span> <span data-ttu-id="bfc65-126">Kopie grafu se zobrazí na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-126">A copy of the chart appears on the dashboard.</span></span>
2. <span data-ttu-id="bfc65-127">Připnout celé okno na řídicí panel – zobrazí se na řídicím panelu jako dlaždici, prostřednictvím které po kliknutí.</span><span class="sxs-lookup"><span data-stu-id="bfc65-127">Pin the whole blade to the dashboard - it appears on the dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="bfc65-128">Klikněte na tlačítko levého horního rohu se vraťte do aktuálního řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-128">Click the top left corner to return to the current dashboard.</span></span> <span data-ttu-id="bfc65-129">Pak můžete použít rozevírací nabídky se vrátit do aktuální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bfc65-129">Then you can use the drop-down menu to return to the current view.</span></span>

<span data-ttu-id="bfc65-130">Všimněte si, že grafy jsou seskupené do dlaždice: dlaždici může obsahovat více než jeden graf.</span><span class="sxs-lookup"><span data-stu-id="bfc65-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="bfc65-131">Připnete celou dlaždice na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-131">You pin the whole tile to the dashboard.</span></span>

<span data-ttu-id="bfc65-132">Graf se automaticky aktualizují s frekvencí, která závisí na grafu časové rozmezí:</span><span class="sxs-lookup"><span data-stu-id="bfc65-132">The chart is automatically refreshed with a frequency that depends on the chart's time range:</span></span>

* <span data-ttu-id="bfc65-133">Čas v rozsahu až 1 hodina: aktualizace každých 5 minut</span><span class="sxs-lookup"><span data-stu-id="bfc65-133">Time range up to 1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="bfc65-134">Čas rozsah 1 – 24 hodin: aktualizace každých 15 minut</span><span class="sxs-lookup"><span data-stu-id="bfc65-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="bfc65-135">Čas rozsah vyšší než 24 hodin: (čas rozsah) / 60.</span><span class="sxs-lookup"><span data-stu-id="bfc65-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="bfc65-136">Připnout jakýkoli dotaz ve Analytics</span><span class="sxs-lookup"><span data-stu-id="bfc65-136">Pin any query in Analytics</span></span>
<span data-ttu-id="bfc65-137">Můžete také [připnout Analytics](app-insights-analytics-using.md#pin-to-dashboard) grafy k [sdílené](#share-dashboards-with-your-team) řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts to a [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="bfc65-138">To umožňuje přidat grafy jakýkoli libovolný dotaz spolu s standardní metriky.</span><span class="sxs-lookup"><span data-stu-id="bfc65-138">This allows you to add charts of any arbitrary query alongside the standard metrics.</span></span> 

<span data-ttu-id="bfc65-139">Výsledky jsou přepočítána automaticky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="bfc65-140">Klikněte na ikonu aktualizace na graf přepočítat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="bfc65-140">Click the Refresh icon on the chart to recalculate immediately.</span></span> <span data-ttu-id="bfc65-141">(Obnovit v prohlížeči nezměněný.)</span><span class="sxs-lookup"><span data-stu-id="bfc65-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-the-dashboard"></a><span data-ttu-id="bfc65-142">Upravit na dlaždici na řídicím panelu</span><span class="sxs-lookup"><span data-stu-id="bfc65-142">Adjust a tile on the dashboard</span></span>
<span data-ttu-id="bfc65-143">Jakmile dlaždici na řídicím panelu, můžete ji upravit.</span><span class="sxs-lookup"><span data-stu-id="bfc65-143">Once a tile is on the dashboard, you can adjust it.</span></span>

![Najeďte myší na graf ji Pokud chcete upravit.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="bfc65-145">Přidáte graf dlaždici.</span><span class="sxs-lookup"><span data-stu-id="bfc65-145">Add a chart to the tile.</span></span>
2. <span data-ttu-id="bfc65-146">Nastavte metriku, dimenze Seskupit podle a stylu grafu (tabulky, grafu).</span><span class="sxs-lookup"><span data-stu-id="bfc65-146">Set the metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="bfc65-147">Přetáhněte v diagramu na přiblížit; Klikněte na tlačítko Zpět a obnovit časový interval; nastavit vlastnosti filtru pro grafy na dlaždici.</span><span class="sxs-lookup"><span data-stu-id="bfc65-147">Drag across the diagram to zoom in; click the undo button to reset the timespan; set filter properties for the charts on the tile.</span></span>
4. <span data-ttu-id="bfc65-148">Nastavte název dlaždice.</span><span class="sxs-lookup"><span data-stu-id="bfc65-148">Set tile title.</span></span>

<span data-ttu-id="bfc65-149">Dlaždice připnuté z okna Průzkumníka metriky mají další možnosti úprav než dlaždice připnuté z okno Přehled.</span><span class="sxs-lookup"><span data-stu-id="bfc65-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="bfc65-150">Původní dlaždici, která jste připnuli nemá vliv provedené úpravy.</span><span class="sxs-lookup"><span data-stu-id="bfc65-150">The original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="bfc65-151">Přepínání mezi řídicí panely</span><span class="sxs-lookup"><span data-stu-id="bfc65-151">Switch between dashboards</span></span>
<span data-ttu-id="bfc65-152">Můžete uložit více než jeden řídicí panel a mezi nimi přepínat.</span><span class="sxs-lookup"><span data-stu-id="bfc65-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="bfc65-153">Pokud připnete graf nebo okna, přidají se do aktuálního řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-153">When you pin a chart or blade, they're added to the current dashboard.</span></span>

![Přepnout mezi řídicí panely, klikněte na řídicí panel a vyberte uložené řídicí panel.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="bfc65-157">Například můžete mít jeden řídicí panel pro zobrazení celé obrazovky v týmové místnosti a druhý pro obecné vývoj.</span><span class="sxs-lookup"><span data-stu-id="bfc65-157">For example, you might have one dashboard for displaying full screen in the team room, and another for general development.</span></span>

<span data-ttu-id="bfc65-158">Na řídicím panelu, okno se zobrazí jako dlaždici: klikněte na něj přejdete do okna.</span><span class="sxs-lookup"><span data-stu-id="bfc65-158">On the dashboard, a blade appears as a tile: click it to go to the blade.</span></span> <span data-ttu-id="bfc65-159">Graf replikuje grafu v původním umístění.</span><span class="sxs-lookup"><span data-stu-id="bfc65-159">A chart replicates the chart in its original location.</span></span>

![Klikněte na dlaždici otevřete okno, který představuje](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="bfc65-161">Sdílet řídicí panely</span><span class="sxs-lookup"><span data-stu-id="bfc65-161">Share dashboards</span></span>
<span data-ttu-id="bfc65-162">Pokud jste vytvořili řídicí panel, můžete ji sdílet s ostatními uživateli.</span><span class="sxs-lookup"><span data-stu-id="bfc65-162">When you've created a dashboard, you can share it with other users.</span></span>

![V záhlaví řídicí panel klikněte na sdílenou složku](./media/app-insights-dashboards/41.png)

<span data-ttu-id="bfc65-164">Další informace o [role a řízení přístupu](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="bfc65-165">Navigace aplikace</span><span class="sxs-lookup"><span data-stu-id="bfc65-165">App navigation</span></span>
<span data-ttu-id="bfc65-166">V okně Přehled je bránu a další informace o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bfc65-166">The overview blade is the gateway to more information about your app.</span></span>

* <span data-ttu-id="bfc65-167">**Všechny graf nebo dlaždice** – kliknutím na libovolnou dlaždici nebo grafu pro zobrazení dalších podrobností o co se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bfc65-167">**Any chart or tile** - Click any tile or chart to see more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="bfc65-168">Přehled okno tlačítka</span><span class="sxs-lookup"><span data-stu-id="bfc65-168">Overview blade buttons</span></span>
![Přehled okno horního navigačního panelu](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="bfc65-170">[**Průzkumníku metrik** ](app-insights-metrics-explorer.md) -vytvářet vlastní grafy výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="bfc65-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="bfc65-171">[**Hledání** ](app-insights-diagnostic-search.md) – prozkoumat konkrétní instanci události, jako jsou žádosti o, výjimky, nebo protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="bfc65-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="bfc65-172">[**Analýza** ](app-insights-analytics.md) -výkonných dotazů přes telemetrie.</span><span class="sxs-lookup"><span data-stu-id="bfc65-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="bfc65-173">**Čas rozsah** -upravit rozsah zobrazuje všechny grafy v okně.</span><span class="sxs-lookup"><span data-stu-id="bfc65-173">**Time range** - Adjust the range displayed by all the charts on the blade.</span></span>
* <span data-ttu-id="bfc65-174">**Odstranit** – prostředek Application Insights pro tuto aplikaci odstranit.</span><span class="sxs-lookup"><span data-stu-id="bfc65-174">**Delete** - Delete the Application Insights resource for this app.</span></span> <span data-ttu-id="bfc65-175">Si také odebrat balíčky Application Insights z kódu aplikace, nebo upravit [klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) ve vaší aplikaci směrovat telemetrii na jiný prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bfc65-175">You should also either remove the Application Insights packages from your app code, or edit the [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app to direct telemetry to a different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="bfc65-176">Karta Essentials</span><span class="sxs-lookup"><span data-stu-id="bfc65-176">Essentials tab</span></span>
* <span data-ttu-id="bfc65-177">[Klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifikuje tento prostředek aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfc65-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="bfc65-178">Ceny - byly funkce svazek k dispozici a nastavené CAP k vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="bfc65-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="bfc65-179">Navigační panel aplikace</span><span class="sxs-lookup"><span data-stu-id="bfc65-179">App navigation bar</span></span>
![Levý navigační panel](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="bfc65-181">**Přehled** -vraťte do okna Přehled aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfc65-181">**Overview** - Return to the app overview blade.</span></span>
* <span data-ttu-id="bfc65-182">**Protokol aktivit** -výstrahy a událostí Azure pro správu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="bfc65-183">[**Řízení přístupu** ](app-insights-resources-roles-access-control.md) -poskytnutí přístupu k členům týmu a další.</span><span class="sxs-lookup"><span data-stu-id="bfc65-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access to team members and others.</span></span>
* <span data-ttu-id="bfc65-184">[**Značky** ](../azure-resource-manager/resource-group-using-tags.md) -použití značek ke skupině vaší aplikace s ostatními.</span><span class="sxs-lookup"><span data-stu-id="bfc65-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags to group your app with others.</span></span>

<span data-ttu-id="bfc65-185">PROZKOUMAT</span><span class="sxs-lookup"><span data-stu-id="bfc65-185">INVESTIGATE</span></span>

* <span data-ttu-id="bfc65-186">[**Mapa aplikace** ](app-insights-app-map.md) -Active mapě a ukáže jednotlivých součástí aplikace, odvozené z informací o závislostech.</span><span class="sxs-lookup"><span data-stu-id="bfc65-186">[**Application map**](app-insights-app-map.md) - Active map showing the components of your application, derived from the dependency information.</span></span>
* <span data-ttu-id="bfc65-187">[**Inteligentní detekce** ](app-insights-proactive-diagnostics.md) – přečtěte si poslední výstrahy výkonu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="bfc65-188">[**Živý datový proud** ](app-insights-live-stream.md) – A fixed sadu téměř rychlých metriky, užitečné při nasazování nového sestavení nebo ladění.</span><span class="sxs-lookup"><span data-stu-id="bfc65-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="bfc65-189">[**Dostupnost / webové testy** ](app-insights-monitor-web-app-availability.md) -odesílání pravidelných požadavků na vaši webovou aplikaci z kolem world.*</span><span class="sxs-lookup"><span data-stu-id="bfc65-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests to your web app from around the world.*</span></span>
* <span data-ttu-id="bfc65-190">[**Selhání, výkon** ](app-insights-web-monitor-performance.md) -výjimky, selhání sazby a dobu odezvy pro požadavky na aplikace a požadavky z vaší aplikace [závislosti](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests to your app and for requests from your app to [dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="bfc65-191">[**Výkon** ](app-insights-web-monitor-performance.md) -doba odezvy, závislosti odezvy.</span><span class="sxs-lookup"><span data-stu-id="bfc65-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="bfc65-192">[Servery](app-insights-web-monitor-performance.md) -čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="bfc65-193">K dispozici, pokud jste [nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="bfc65-194">**Prohlížeč** -stránka zobrazení a výkon AJAX.</span><span class="sxs-lookup"><span data-stu-id="bfc65-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="bfc65-195">K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="bfc65-196">**Využití** -stránka zobrazení, uživatele a počty relací.</span><span class="sxs-lookup"><span data-stu-id="bfc65-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="bfc65-197">K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="bfc65-198">KONFIGURACE</span><span class="sxs-lookup"><span data-stu-id="bfc65-198">CONFIGURE</span></span>

* <span data-ttu-id="bfc65-199">**Začínáme** -vložené kurzu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="bfc65-200">**Vlastnosti** -klíč instrumentace, předplatné a id prostředku.</span><span class="sxs-lookup"><span data-stu-id="bfc65-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="bfc65-201">[Výstrahy](app-insights-alerts.md) -metriky konfigurace výstrahy.</span><span class="sxs-lookup"><span data-stu-id="bfc65-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="bfc65-202">[Průběžné export](app-insights-export-telemetry.md) -konfigurace export telemetrie do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bfc65-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry to Azure storage.</span></span>
* <span data-ttu-id="bfc65-203">[Testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests) -nastavit syntetické zatížení na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="bfc65-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="bfc65-204">[Kvóty a ceny](app-insights-pricing.md) a [přijímání vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="bfc65-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="bfc65-205">**Přístup pomocí rozhraní API** -vytvořit [verze poznámek](app-insights-annotations.md) a pro rozhraní API služby Data Access.</span><span class="sxs-lookup"><span data-stu-id="bfc65-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for the Data Access API.</span></span>
* <span data-ttu-id="bfc65-206">[**Pracovní položky** ](app-insights-diagnostic-search.md#create-work-item) – připojení k pracovním sledování systému, takže můžete vytvořit chyby při kontrole telemetrie.</span><span class="sxs-lookup"><span data-stu-id="bfc65-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect to a work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="bfc65-207">NASTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="bfc65-207">SETTINGS</span></span>

* <span data-ttu-id="bfc65-208">[**Zamkne** ](../azure-resource-manager/resource-group-lock-resources.md) -zamknutí prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="bfc65-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="bfc65-209">[**Skriptu pro automatizaci** ](app-insights-powershell.md) -export definice prostředku Azure tak, aby vám ho použít jako šablonu pro vytvoření nových prostředků.</span><span class="sxs-lookup"><span data-stu-id="bfc65-209">[**Automation script**](app-insights-powershell.md) - export a definition of the Azure resource so that you can use it as a template to create new resources.</span></span>


## <a name="video"></a><span data-ttu-id="bfc65-210">Video</span><span class="sxs-lookup"><span data-stu-id="bfc65-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="bfc65-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bfc65-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="bfc65-212">Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="bfc65-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="bfc65-213">Filtr a segment metriky</span><span class="sxs-lookup"><span data-stu-id="bfc65-213">Filter and segment metrics</span></span> |![Příklad hledání](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="bfc65-215">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="bfc65-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="bfc65-216">Najít a kontrola události, související události a vytvořit chyby</span><span class="sxs-lookup"><span data-stu-id="bfc65-216">Find and inspect events, related events, and create bugs</span></span> |![Příklad hledání](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="bfc65-218">Analýzy</span><span class="sxs-lookup"><span data-stu-id="bfc65-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="bfc65-219">Účinný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="bfc65-219">Powerful query language</span></span> |![Příklad hledání](./media/app-insights-dashboards/63.png) |
