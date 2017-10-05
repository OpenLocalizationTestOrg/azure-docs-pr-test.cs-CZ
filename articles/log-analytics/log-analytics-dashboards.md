---
title: "Vytvořit vlastní řídicí panel v Azure Log Analytics | Microsoft Docs"
description: "Tento průvodce vám pomůže pochopit jak řídicí panely analýzy protokolů můžete vizualizovat všechny uložený protokol hledání, budete jeden přehledu zobrazení prostředí."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a90d9c620221bffbb225fb060b997af2f5e90390
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="a2c8e-103">Vytvořit vlastní řídicí panel pro použití v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a2c8e-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="a2c8e-104">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak nelze vytvořit nové řídicí panely nebo upravit existující řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-104">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="a2c8e-105">Tento průvodce vám pomůže pochopit jak řídicí panely analýzy protokolů můžete vizualizovat všechny uložený protokol hledání, budete jeden přehledu zobrazení prostředí.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens to view your environment.</span></span>

![Příklad řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="a2c8e-107">Všechny vlastní řídicí panely, které vytvoříte na portálu OMS jsou také k dispozici v OMS mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-107">All the custom dashboards that you create in the OMS portal are also available in the OMS Mobile App.</span></span> <span data-ttu-id="a2c8e-108">Najdete na následujících stránkách Další informace o aplikacích.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-108">See the following pages for more information about the apps.</span></span>

* [<span data-ttu-id="a2c8e-109">OMS mobilní aplikace z Windows Store Microsoft</span><span class="sxs-lookup"><span data-stu-id="a2c8e-109">OMS mobile app from the Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="a2c8e-110">OMS mobilních aplikací z Apple iTunes</span><span class="sxs-lookup"><span data-stu-id="a2c8e-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobilní řídicí panel](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="a2c8e-112">Vytvoření Můj řídicí panel</span><span class="sxs-lookup"><span data-stu-id="a2c8e-112">How do I create my dashboard?</span></span>
<span data-ttu-id="a2c8e-113">Pokud chcete začít, přejděte na stránku přehled OMS.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-113">To begin, go to the OMS Overview page.</span></span> <span data-ttu-id="a2c8e-114">Zobrazí se **vlastní řídicí panel** dlaždici na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-114">You'll see the **My Dashboard** tile on the left.</span></span> <span data-ttu-id="a2c8e-115">Klikněte na něj můžete rozbalit řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-115">Click it to drill down into your dashboard.</span></span>

![Přehled](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="a2c8e-117">Přidat dlaždice</span><span class="sxs-lookup"><span data-stu-id="a2c8e-117">Adding a tile</span></span>
<span data-ttu-id="a2c8e-118">V řídicí panely dlaždice se používá technologii uložený protokol hledání.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="a2c8e-119">OMS se dodává s mnoha předem provedené uložený protokol hledání, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="a2c8e-120">Použijte následující kroky, které popisují, jak začít.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-120">Use the following steps that outline how to begin.</span></span>

<span data-ttu-id="a2c8e-121">V zobrazení vlastní řídicí panel, klikněte jednoduše **přizpůsobit** zadat vlastní nastavení režimu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-121">In the My Dashboard view, simply click **Customize** to enter customize mode.</span></span>

![Obrazové](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="a2c8e-123">Na panelu, které se otevře na pravé straně stránky zobrazí všechny pracovního prostoru uložený protokol hledání.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-123">The panel that opens on the right side of the page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="a2c8e-124">K vizualizaci uložený protokol hledání jako dlaždici, najeďte myší na uloženého hledání a klikněte **plus** symbol.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-124">To visualize a saved log search as a tile,  hover over a saved search and then click the **plus** symbol.</span></span>

![Přidat dlaždice 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="a2c8e-126">Když kliknete **plus** symbolů, nová dlaždice se zobrazí v zobrazení Moje řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-126">When you click the **plus** symbol, a new tile appears in the My Dashboard view.</span></span>

![Přidat dlaždice 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="a2c8e-128">Upravit dlaždici</span><span class="sxs-lookup"><span data-stu-id="a2c8e-128">Edit a tile</span></span>
<span data-ttu-id="a2c8e-129">V zobrazení vlastní řídicí panel, klikněte jednoduše **přizpůsobit** zadat vlastní nastavení režimu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-129">In the My Dashboard view, simply click  **Customize** to enter customize mode.</span></span> <span data-ttu-id="a2c8e-130">Klikněte na dlaždici, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-130">Click the tile you want to edit.</span></span> <span data-ttu-id="a2c8e-131">Změny pravém panelu, který chcete upravit a umožňuje výběr možností:</span><span class="sxs-lookup"><span data-stu-id="a2c8e-131">The right panel changes to edit, and gives a selection of options:</span></span>

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="a2c8e-134">Vizualizace dlaždice</span><span class="sxs-lookup"><span data-stu-id="a2c8e-134">Tile visualizations</span></span>
<span data-ttu-id="a2c8e-135">Existují tři druhy vizualizace dlaždice můžete vybrat ze:</span><span class="sxs-lookup"><span data-stu-id="a2c8e-135">There are three kinds of tile visualizations to choose from:</span></span>

| <span data-ttu-id="a2c8e-136">Typ grafu</span><span class="sxs-lookup"><span data-stu-id="a2c8e-136">chart type</span></span> | <span data-ttu-id="a2c8e-137">Jak funguje</span><span class="sxs-lookup"><span data-stu-id="a2c8e-137">what it does</span></span> |
| --- | --- |
| ![Pruhový graf](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="a2c8e-139">Časová osa výsledků hledání uložený protokol zobrazí jako pruhový graf nebo seznam výsledků podle pole v závislosti na tom, pokud vyhledávání protokolu agreguje výsledky podle pole, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![Metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="a2c8e-141">Zobrazí vaše přístupů výsledek hledání celkový protokolu jako číslo v dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="a2c8e-142">Metriky dlaždice umožňují nastavit prahovou hodnotu, která se zaměřuje na dlaždici na dosažení prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-142">Metric tiles allow you to set a threshold that will highlight the tile when the threshold is reached.</span></span> |
| ![řádek](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="a2c8e-144">Jako spojnicový graf zobrazuje časová osa přístupů vaší uložený protokol hledání výsledek s hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="a2c8e-145">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="a2c8e-145">Threshold</span></span>
<span data-ttu-id="a2c8e-146">Na dlaždici pomocí metriky vizualizace můžete vytvořit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-146">You can create a threshold on a tile using the Metric visualization.</span></span> <span data-ttu-id="a2c8e-147">Vyberte na vytvoření prahovou hodnotu na dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-147">Select on to create a threshold value on the tile.</span></span> <span data-ttu-id="a2c8e-148">Vyberte, zda chcete zvýraznit dlaždici, když hodnota je nad nebo pod zvolenou prahovou hodnotu, a poté nastavit mezní hodnotu níže.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-148">Choose whether to highlight the tile when the value is over or under the chosen threshold, then set the threshold value below.</span></span>

## <a name="organizing-the-dashboard"></a><span data-ttu-id="a2c8e-149">Uspořádání řídicí panel</span><span class="sxs-lookup"><span data-stu-id="a2c8e-149">Organizing the dashboard</span></span>
<span data-ttu-id="a2c8e-150">Chcete-li uspořádat řídicího panelu, přejděte do zobrazení Moje řídicího panelu a klikněte na **přizpůsobit** k zadání vlastní nastavení režimu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-150">To organize your dashboard, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="a2c8e-151">Klikněte na tlačítko a přetáhněte dlaždice, které chcete přesunout a přesunout na místo, kde dlaždice. Chcete-li být.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-151">Click and drag the tile you want to move, and move it to where you want your tile to be.</span></span>

![Uspořádání řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="a2c8e-153">Odebrat dlaždici</span><span class="sxs-lookup"><span data-stu-id="a2c8e-153">Remove a tile</span></span>
<span data-ttu-id="a2c8e-154">Odebrat dlaždici, přejděte do zobrazení Moje řídicího panelu a klikněte na tlačítko **přizpůsobit** zadat vlastní nastavení režimu.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-154">To remove a tile, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="a2c8e-155">Vyberte dlaždici, kterou chcete odebrat a pak na pravém panelu vyberte **odebrat dlaždici**.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-155">Select the tile you want to remove, then on the right panel select **Remove Tile**.</span></span>

![Odebrat dlaždici](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="a2c8e-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2c8e-157">Next steps</span></span>
* <span data-ttu-id="a2c8e-158">Vytvoření [výstrahy](log-analytics-alerts.md) v analýzy protokolů generování oznámení a opravovat problémy.</span><span class="sxs-lookup"><span data-stu-id="a2c8e-158">Create [alerts](log-analytics-alerts.md) in Log Analytics to generate notifications and to remediate problems.</span></span>
