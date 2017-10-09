---
title: aaaDashboards a navigace ve hello Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a><span data-ttu-id="d0f88-103">Navigační a řídicí panely portálu Application Insights hello</span><span class="sxs-lookup"><span data-stu-id="d0f88-103">Navigation and Dashboards in hello Application Insights portal</span></span>
<span data-ttu-id="d0f88-104">Až budete mít [nastavte Application Insights na projektu](app-insights-overview.md), telemetrická data o výkonu a využití vaší aplikace se zobrazí v projektu na prostředek Application Insights hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d0f88-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in hello [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="d0f88-105">Najít telemetrii</span><span class="sxs-lookup"><span data-stu-id="d0f88-105">Find your telemetry</span></span>
<span data-ttu-id="d0f88-106">Přihlaste se toohello [portál Azure](https://portal.azure.com) a přejděte toohello prostředek Application Insights, kterou jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0f88-106">Sign in toohello [Azure portal](https://portal.azure.com) and navigate toohello Application Insights resource that you created for your app.</span></span>

![Klikněte na tlačítko Procházet, vyberte Application Insights a pak vaší aplikace.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="d0f88-108">okno Přehled Hello (stránky) pro vaši aplikaci se zobrazí souhrn klíčových diagnostiky hello metrik vaší aplikace a je toohello brány dalších funkcí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="d0f88-108">hello overview blade (page) for your app shows a summary of hello key diagnostic metrics of your app, and is a gateway toohello other features of hello portal.</span></span>

![Hlavní trasy tooview telemetrie](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="d0f88-110">Můžete přizpůsobit kterékoli z hello grafy a mřížky a připnete ji tooa řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-110">You can customize any of hello charts and grids and pin them tooa dashboard.</span></span> <span data-ttu-id="d0f88-111">Tímto způsobem můžete zahrnout společně hello klíče telemetrie z různých aplikací Centrální řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d0f88-111">That way, you can bring together hello key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="d0f88-112">Řídicí panely</span><span class="sxs-lookup"><span data-stu-id="d0f88-112">Dashboards</span></span>
<span data-ttu-id="d0f88-113">Hello nejprve thing se zobrazí po přihlášení toohello [portálu Microsoft Azure](https://portal.azure.com) je řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d0f88-113">hello first thing you see after you sign in toohello [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="d0f88-114">Zde můžete najednou přenést hello grafů, které jsou pro všechny vaše prostředky Azure, včetně telemetrie z nejdůležitějších tooyou [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-114">Here you can bring together hello charts that are most important tooyou across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![Přizpůsobený řídicí panel.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="d0f88-116">**Přejděte toospecific prostředky** například vaše aplikace ve službě Application Insights: použití hello levém panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-116">**Navigate toospecific resources** such as your app in Application Insights: Use hello left bar.</span></span>
2. <span data-ttu-id="d0f88-117">**Návratový toohello aktuálního řídicího panelu**, nebo Přepnout poslední zobrazení tooother: použití hello rozevírací nabídce nahoře vlevo.</span><span class="sxs-lookup"><span data-stu-id="d0f88-117">**Return toohello current dashboard**, or switch tooother recent views: Use hello drop-down menu at top left.</span></span>
3. <span data-ttu-id="d0f88-118">**Přepínač řídicí panely**: použití hello rozevírací nabídky na název řídicího panelu hello</span><span class="sxs-lookup"><span data-stu-id="d0f88-118">**Switch dashboards**: Use hello drop-down menu on hello dashboard title</span></span>
4. <span data-ttu-id="d0f88-119">**Vytvářet, upravovat a sdílet řídicí panely** na hello řídicího panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="d0f88-119">**Create, edit, and share dashboards** in hello dashboard toolbar.</span></span>
5. <span data-ttu-id="d0f88-120">**Upravit řídicí panel hello**: najeďte na dlaždici a potom pomocí jeho horním panelu toomove, přizpůsobení, nebo ji odeberte.</span><span class="sxs-lookup"><span data-stu-id="d0f88-120">**Edit hello dashboard**: Hover over a tile and then use its top bar toomove, customize, or remove it.</span></span>

## <a name="add-tooa-dashboard"></a><span data-ttu-id="d0f88-121">Přidat řídicí panel tooa</span><span class="sxs-lookup"><span data-stu-id="d0f88-121">Add tooa dashboard</span></span>
<span data-ttu-id="d0f88-122">Když se díváte na okno nebo sadu grafů, který je zajímavé, budete moct připnout jeho kopii toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it toohello dashboard.</span></span> <span data-ttu-id="d0f88-123">Se zobrazí při příštím vrátíte existuje.</span><span class="sxs-lookup"><span data-stu-id="d0f88-123">You'll see it next time you return there.</span></span>

![toopin graf, pozastavte ukazatel myši nad ním a pak klikněte na tlačítko "..." v záhlaví hello.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="d0f88-125">Graf toodashboard PIN kód.</span><span class="sxs-lookup"><span data-stu-id="d0f88-125">Pin chart toodashboard.</span></span> <span data-ttu-id="d0f88-126">Kopii hello grafu se zobrazí na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="d0f88-126">A copy of hello chart appears on hello dashboard.</span></span>
2. <span data-ttu-id="d0f88-127">PIN kód hello celé okno toohello řídicí panel – zobrazí se na řídicím panelu hello jako dlaždici, prostřednictvím které po kliknutí.</span><span class="sxs-lookup"><span data-stu-id="d0f88-127">Pin hello whole blade toohello dashboard - it appears on hello dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="d0f88-128">Klikněte na tlačítko hello levého horního rohu tooreturn toohello aktuálního řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-128">Click hello top left corner tooreturn toohello current dashboard.</span></span> <span data-ttu-id="d0f88-129">Pak můžete použít hello rozevírací nabídky tooreturn toohello aktuální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d0f88-129">Then you can use hello drop-down menu tooreturn toohello current view.</span></span>

<span data-ttu-id="d0f88-130">Všimněte si, že grafy jsou seskupené do dlaždice: dlaždici může obsahovat více než jeden graf.</span><span class="sxs-lookup"><span data-stu-id="d0f88-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="d0f88-131">Připnete hello celou dlaždice toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-131">You pin hello whole tile toohello dashboard.</span></span>

<span data-ttu-id="d0f88-132">Graf Hello se automaticky aktualizují s frekvencí, která závisí na graf hello časové rozmezí:</span><span class="sxs-lookup"><span data-stu-id="d0f88-132">hello chart is automatically refreshed with a frequency that depends on hello chart's time range:</span></span>

* <span data-ttu-id="d0f88-133">Čas rozsah až hodinu too1: aktualizace každých 5 minut</span><span class="sxs-lookup"><span data-stu-id="d0f88-133">Time range up too1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="d0f88-134">Čas rozsah 1 – 24 hodin: aktualizace každých 15 minut</span><span class="sxs-lookup"><span data-stu-id="d0f88-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="d0f88-135">Čas rozsah vyšší než 24 hodin: (čas rozsah) / 60.</span><span class="sxs-lookup"><span data-stu-id="d0f88-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="d0f88-136">Připnout jakýkoli dotaz ve Analytics</span><span class="sxs-lookup"><span data-stu-id="d0f88-136">Pin any query in Analytics</span></span>
<span data-ttu-id="d0f88-137">Můžete také [připnout Analytics](app-insights-analytics-using.md#pin-to-dashboard) grafy tooa [sdílené](#share-dashboards-with-your-team) řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts tooa [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="d0f88-138">To vám umožní tooadd grafy žádné libovolný dotazu spolu s hello standardní metriky.</span><span class="sxs-lookup"><span data-stu-id="d0f88-138">This allows you tooadd charts of any arbitrary query alongside hello standard metrics.</span></span> 

<span data-ttu-id="d0f88-139">Výsledky jsou přepočítána automaticky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="d0f88-140">Klikněte na ikonu aktualizace hello na hello grafu toorecalculate okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d0f88-140">Click hello Refresh icon on hello chart toorecalculate immediately.</span></span> <span data-ttu-id="d0f88-141">(Obnovit v prohlížeči nezměněný.)</span><span class="sxs-lookup"><span data-stu-id="d0f88-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-hello-dashboard"></a><span data-ttu-id="d0f88-142">Upravit dlaždice na řídicím panelu hello</span><span class="sxs-lookup"><span data-stu-id="d0f88-142">Adjust a tile on hello dashboard</span></span>
<span data-ttu-id="d0f88-143">Jakmile na dlaždici na řídicím panelu hello, můžete ji upravit.</span><span class="sxs-lookup"><span data-stu-id="d0f88-143">Once a tile is on hello dashboard, you can adjust it.</span></span>

![Pozastavte ukazatel myši nad grafu v pořadí tooedit ho.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="d0f88-145">Přidáte dlaždice toohello grafu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-145">Add a chart toohello tile.</span></span>
2. <span data-ttu-id="d0f88-146">Nastavte metriku hello, dimenze Seskupit podle a stylu grafu (tabulky, grafu).</span><span class="sxs-lookup"><span data-stu-id="d0f88-146">Set hello metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="d0f88-147">Přetáhněte napříč hello diagram toozoom v; Klikněte na tlačítko hello vrácení zpět tlačítko tooreset hello timespan; nastavit vlastnosti filtru pro grafy hello na dlaždici hello.</span><span class="sxs-lookup"><span data-stu-id="d0f88-147">Drag across hello diagram toozoom in; click hello undo button tooreset hello timespan; set filter properties for hello charts on hello tile.</span></span>
4. <span data-ttu-id="d0f88-148">Nastavte název dlaždice.</span><span class="sxs-lookup"><span data-stu-id="d0f88-148">Set tile title.</span></span>

<span data-ttu-id="d0f88-149">Dlaždice připnuté z okna Průzkumníka metriky mají další možnosti úprav než dlaždice připnuté z okno Přehled.</span><span class="sxs-lookup"><span data-stu-id="d0f88-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="d0f88-150">Hello původní dlaždice, který jste připnuli nemá vliv provedené úpravy.</span><span class="sxs-lookup"><span data-stu-id="d0f88-150">hello original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="d0f88-151">Přepínání mezi řídicí panely</span><span class="sxs-lookup"><span data-stu-id="d0f88-151">Switch between dashboards</span></span>
<span data-ttu-id="d0f88-152">Můžete uložit více než jeden řídicí panel a mezi nimi přepínat.</span><span class="sxs-lookup"><span data-stu-id="d0f88-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="d0f88-153">Pokud připnete graf nebo okna, přidají se toohello aktuálního řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-153">When you pin a chart or blade, they're added toohello current dashboard.</span></span>

![tooswitch mezi řídicí panely, klikněte na řídicí panel a vyberte uložené řídicí panel.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="d0f88-157">Například můžete mít jeden řídicí panel pro zobrazení celé obrazovky v týmové místnosti hello a druhý pro obecné vývoj.</span><span class="sxs-lookup"><span data-stu-id="d0f88-157">For example, you might have one dashboard for displaying full screen in hello team room, and another for general development.</span></span>

<span data-ttu-id="d0f88-158">Na řídicím panelu hello, okno se zobrazí jako dlaždici: klikněte na něj toogo toohello okno.</span><span class="sxs-lookup"><span data-stu-id="d0f88-158">On hello dashboard, a blade appears as a tile: click it toogo toohello blade.</span></span> <span data-ttu-id="d0f88-159">Graf replikuje hello grafu v původním umístění.</span><span class="sxs-lookup"><span data-stu-id="d0f88-159">A chart replicates hello chart in its original location.</span></span>

![Klikněte na okno hello tooopen dlaždice, který představuje](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="d0f88-161">Sdílet řídicí panely</span><span class="sxs-lookup"><span data-stu-id="d0f88-161">Share dashboards</span></span>
<span data-ttu-id="d0f88-162">Pokud jste vytvořili řídicí panel, můžete ji sdílet s ostatními uživateli.</span><span class="sxs-lookup"><span data-stu-id="d0f88-162">When you've created a dashboard, you can share it with other users.</span></span>

![V záhlaví hello řídicí panel klikněte na sdílenou složku](./media/app-insights-dashboards/41.png)

<span data-ttu-id="d0f88-164">Další informace o [role a řízení přístupu](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="d0f88-165">Navigace aplikace</span><span class="sxs-lookup"><span data-stu-id="d0f88-165">App navigation</span></span>
<span data-ttu-id="d0f88-166">okno Přehled Hello je hello brány toomore informace o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0f88-166">hello overview blade is hello gateway toomore information about your app.</span></span>

* <span data-ttu-id="d0f88-167">**Všechny graf nebo dlaždice** – kliknutím na libovolnou dlaždici nebo grafu toosee více podrobností o co se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="d0f88-167">**Any chart or tile** - Click any tile or chart toosee more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="d0f88-168">Přehled okno tlačítka</span><span class="sxs-lookup"><span data-stu-id="d0f88-168">Overview blade buttons</span></span>
![Přehled okno horního navigačního panelu](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="d0f88-170">[**Průzkumníku metrik** ](app-insights-metrics-explorer.md) -vytvářet vlastní grafy výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="d0f88-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="d0f88-171">[**Hledání** ](app-insights-diagnostic-search.md) – prozkoumat konkrétní instanci události, jako jsou žádosti o, výjimky, nebo protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="d0f88-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="d0f88-172">[**Analýza** ](app-insights-analytics.md) -výkonných dotazů přes telemetrie.</span><span class="sxs-lookup"><span data-stu-id="d0f88-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="d0f88-173">**Čas rozsah** -upravit rozsah hello zobrazuje všechny hello grafy v okně hello.</span><span class="sxs-lookup"><span data-stu-id="d0f88-173">**Time range** - Adjust hello range displayed by all hello charts on hello blade.</span></span>
* <span data-ttu-id="d0f88-174">**Odstranit** -odstranit hello prostředek Application Insights pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0f88-174">**Delete** - Delete hello Application Insights resource for this app.</span></span> <span data-ttu-id="d0f88-175">Si také odebrat hello Application Insights balíčky z kódu aplikace, nebo upravit hello [klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) ve vaší aplikaci toodirect telemetrie tooa jiný prostředek služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d0f88-175">You should also either remove hello Application Insights packages from your app code, or edit hello [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app toodirect telemetry tooa different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="d0f88-176">Karta Essentials</span><span class="sxs-lookup"><span data-stu-id="d0f88-176">Essentials tab</span></span>
* <span data-ttu-id="d0f88-177">[Klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifikuje tento prostředek aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0f88-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="d0f88-178">Ceny - byly funkce svazek k dispozici a nastavené CAP k vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="d0f88-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="d0f88-179">Navigační panel aplikace</span><span class="sxs-lookup"><span data-stu-id="d0f88-179">App navigation bar</span></span>
![Levý navigační panel](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="d0f88-181">**Přehled** -návratový toohello okně přehledu aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0f88-181">**Overview** - Return toohello app overview blade.</span></span>
* <span data-ttu-id="d0f88-182">**Protokol aktivit** -výstrahy a událostí Azure pro správu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="d0f88-183">[**Řízení přístupu** ](app-insights-resources-roles-access-control.md) -poskytnout přístup tooteam členy a další.</span><span class="sxs-lookup"><span data-stu-id="d0f88-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access tooteam members and others.</span></span>
* <span data-ttu-id="d0f88-184">[**Značky** ](../azure-resource-manager/resource-group-using-tags.md) -použití značky toogroup vaší aplikace s ostatními.</span><span class="sxs-lookup"><span data-stu-id="d0f88-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags toogroup your app with others.</span></span>

<span data-ttu-id="d0f88-185">PROZKOUMAT</span><span class="sxs-lookup"><span data-stu-id="d0f88-185">INVESTIGATE</span></span>

* <span data-ttu-id="d0f88-186">[**Mapa aplikace** ](app-insights-app-map.md) -Active mapování s hello součástí aplikace, odvozené z informací o závislostech hello.</span><span class="sxs-lookup"><span data-stu-id="d0f88-186">[**Application map**](app-insights-app-map.md) - Active map showing hello components of your application, derived from hello dependency information.</span></span>
* <span data-ttu-id="d0f88-187">[**Inteligentní detekce** ](app-insights-proactive-diagnostics.md) – přečtěte si poslední výstrahy výkonu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="d0f88-188">[**Živý datový proud** ](app-insights-live-stream.md) – A fixed sadu téměř rychlých metriky, užitečné při nasazování nového sestavení nebo ladění.</span><span class="sxs-lookup"><span data-stu-id="d0f88-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="d0f88-189">[**Dostupnost / webové testy** ](app-insights-monitor-web-app-availability.md) -odesílání pravidelných požadavky tooyour webové aplikace z kolem hello world.*</span><span class="sxs-lookup"><span data-stu-id="d0f88-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests tooyour web app from around hello world.*</span></span>
* <span data-ttu-id="d0f88-190">[**Selhání, výkon** ](app-insights-web-monitor-performance.md) -výjimky, selhání sazby a odpovědi časový limit pro aplikaci tooyour požadavky a požadavky z vaší aplikace příliš[závislosti](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests tooyour app and for requests from your app too[dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="d0f88-191">[**Výkon** ](app-insights-web-monitor-performance.md) -doba odezvy, závislosti odezvy.</span><span class="sxs-lookup"><span data-stu-id="d0f88-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="d0f88-192">[Servery](app-insights-web-monitor-performance.md) -čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="d0f88-193">K dispozici, pokud jste [nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="d0f88-194">**Prohlížeč** -stránka zobrazení a výkon AJAX.</span><span class="sxs-lookup"><span data-stu-id="d0f88-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="d0f88-195">K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="d0f88-196">**Využití** -stránka zobrazení, uživatele a počty relací.</span><span class="sxs-lookup"><span data-stu-id="d0f88-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="d0f88-197">K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="d0f88-198">KONFIGURACE</span><span class="sxs-lookup"><span data-stu-id="d0f88-198">CONFIGURE</span></span>

* <span data-ttu-id="d0f88-199">**Začínáme** -vložené kurzu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="d0f88-200">**Vlastnosti** -klíč instrumentace, předplatné a id prostředku.</span><span class="sxs-lookup"><span data-stu-id="d0f88-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="d0f88-201">[Výstrahy](app-insights-alerts.md) -metriky konfigurace výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d0f88-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="d0f88-202">[Průběžné export](app-insights-export-telemetry.md) -konfigurace export telemetrie tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0f88-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry tooAzure storage.</span></span>
* <span data-ttu-id="d0f88-203">[Testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests) -nastavit syntetické zatížení na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="d0f88-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="d0f88-204">[Kvóty a ceny](app-insights-pricing.md) a [přijímání vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="d0f88-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="d0f88-205">**Přístup pomocí rozhraní API** -vytvořit [verze poznámek](app-insights-annotations.md) a hello API služby Data Access.</span><span class="sxs-lookup"><span data-stu-id="d0f88-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for hello Data Access API.</span></span>
* <span data-ttu-id="d0f88-206">[**Pracovní položky** ](app-insights-diagnostic-search.md#create-work-item) -připojení pracovní tooa sledování systému, takže můžete vytvořit chyby při kontrole telemetrie.</span><span class="sxs-lookup"><span data-stu-id="d0f88-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect tooa work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="d0f88-207">NASTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="d0f88-207">SETTINGS</span></span>

* <span data-ttu-id="d0f88-208">[**Zamkne** ](../azure-resource-manager/resource-group-lock-resources.md) -zamknutí prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="d0f88-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="d0f88-209">[**Skriptu pro automatizaci** ](app-insights-powershell.md) -export definice hello prostředků Azure, aby ho můžete používat jako šablona nových prostředků toocreate.</span><span class="sxs-lookup"><span data-stu-id="d0f88-209">[**Automation script**](app-insights-powershell.md) - export a definition of hello Azure resource so that you can use it as a template toocreate new resources.</span></span>


## <a name="video"></a><span data-ttu-id="d0f88-210">Video</span><span class="sxs-lookup"><span data-stu-id="d0f88-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="d0f88-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0f88-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="d0f88-212">Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="d0f88-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="d0f88-213">Filtr a segment metriky</span><span class="sxs-lookup"><span data-stu-id="d0f88-213">Filter and segment metrics</span></span> |![Příklad hledání](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="d0f88-215">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="d0f88-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="d0f88-216">Najít a kontrola události, související události a vytvořit chyby</span><span class="sxs-lookup"><span data-stu-id="d0f88-216">Find and inspect events, related events, and create bugs</span></span> |![Příklad hledání](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="d0f88-218">Analýzy</span><span class="sxs-lookup"><span data-stu-id="d0f88-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="d0f88-219">Účinný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="d0f88-219">Powerful query language</span></span> |![Příklad hledání](./media/app-insights-dashboards/63.png) |
