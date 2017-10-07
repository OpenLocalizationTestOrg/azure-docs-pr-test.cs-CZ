---
title: "aaaCreate vlastní řídicí panel v Azure Log Analytics | Microsoft Docs"
description: "Tato příručka vám pomůže pochopit, jak řídicí panely analýzy protokolů můžete vizualizovat všechny svoje uložený protokol vyhledávání, která poskytuje jeden objektivu tooview prostředí."
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
ms.openlocfilehash: 73fcf131a91c743d473f37d5a40d52eaf78a7ba3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="df1f0-103">Vytvořit vlastní řídicí panel pro použití v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="df1f0-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="df1f0-104">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak nelze vytvořit nové řídicí panely nebo upravit existující řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="df1f0-104">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="df1f0-105">Tato příručka vám pomůže pochopit, jak řídicí panely analýzy protokolů můžete vizualizovat všechny svoje uložený protokol vyhledávání, která poskytuje jeden objektivu tooview prostředí.</span><span class="sxs-lookup"><span data-stu-id="df1f0-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens tooview your environment.</span></span>

![Příklad řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="df1f0-107">Všechny hello vlastní řídicí panely, které vytvoříte na portálu OMS hello jsou také k dispozici v hello OMS mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="df1f0-107">All hello custom dashboards that you create in hello OMS portal are also available in hello OMS Mobile App.</span></span> <span data-ttu-id="df1f0-108">Viz následující stránky pro další informace o aplikacích hello hello.</span><span class="sxs-lookup"><span data-stu-id="df1f0-108">See hello following pages for more information about hello apps.</span></span>

* [<span data-ttu-id="df1f0-109">Mobilní aplikace OMS z hello Microsoft Store</span><span class="sxs-lookup"><span data-stu-id="df1f0-109">OMS mobile app from hello Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="df1f0-110">OMS mobilních aplikací z Apple iTunes</span><span class="sxs-lookup"><span data-stu-id="df1f0-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobilní řídicí panel](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="df1f0-112">Vytvoření Můj řídicí panel</span><span class="sxs-lookup"><span data-stu-id="df1f0-112">How do I create my dashboard?</span></span>
<span data-ttu-id="df1f0-113">toobegin, přejděte toohello OMS Přehled stránky.</span><span class="sxs-lookup"><span data-stu-id="df1f0-113">toobegin, go toohello OMS Overview page.</span></span> <span data-ttu-id="df1f0-114">Uvidíte hello **vlastní řídicí panel** dlaždice na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="df1f0-114">You'll see hello **My Dashboard** tile on hello left.</span></span> <span data-ttu-id="df1f0-115">Klikněte na něj toodrill dolů do vašeho řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-115">Click it toodrill down into your dashboard.</span></span>

![Přehled](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="df1f0-117">Přidat dlaždice</span><span class="sxs-lookup"><span data-stu-id="df1f0-117">Adding a tile</span></span>
<span data-ttu-id="df1f0-118">V řídicí panely dlaždice se používá technologii uložený protokol hledání.</span><span class="sxs-lookup"><span data-stu-id="df1f0-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="df1f0-119">OMS se dodává s mnoha předem provedené uložený protokol hledání, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="df1f0-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="df1f0-120">Použití hello následující kroky tohoto outline jak toobegin.</span><span class="sxs-lookup"><span data-stu-id="df1f0-120">Use hello following steps that outline how toobegin.</span></span>

<span data-ttu-id="df1f0-121">V zobrazení řídicího panelu Moje hello, jednoduše klikněte **přizpůsobit** tooenter přizpůsobit režimu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-121">In hello My Dashboard view, simply click **Customize** tooenter customize mode.</span></span>

![Obrazové](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="df1f0-123">Hello panely, které se otevře na pravé straně hello hello stránky zobrazí všechny pracovního prostoru uložený protokol hledání.</span><span class="sxs-lookup"><span data-stu-id="df1f0-123">hello panel that opens on hello right side of hello page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="df1f0-124">toovisualize uložený protokol hledání jako dlaždici, najeďte myší na uloženého hledání a potom klikněte na hello **plus** symbol.</span><span class="sxs-lookup"><span data-stu-id="df1f0-124">toovisualize a saved log search as a tile,  hover over a saved search and then click hello **plus** symbol.</span></span>

![Přidat dlaždice 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="df1f0-126">Když kliknete na tlačítko hello **plus** symbolů, nová dlaždice se zobrazí v zobrazení řídicího panelu Moje hello.</span><span class="sxs-lookup"><span data-stu-id="df1f0-126">When you click hello **plus** symbol, a new tile appears in hello My Dashboard view.</span></span>

![Přidat dlaždice 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="df1f0-128">Upravit dlaždici</span><span class="sxs-lookup"><span data-stu-id="df1f0-128">Edit a tile</span></span>
<span data-ttu-id="df1f0-129">V zobrazení řídicího panelu Moje hello, jednoduše klikněte **přizpůsobit** tooenter přizpůsobit režimu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-129">In hello My Dashboard view, simply click  **Customize** tooenter customize mode.</span></span> <span data-ttu-id="df1f0-130">Klikněte na dlaždici hello chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="df1f0-130">Click hello tile you want tooedit.</span></span> <span data-ttu-id="df1f0-131">Hello pravém panelu změny tooedit a poskytuje výběr možností:</span><span class="sxs-lookup"><span data-stu-id="df1f0-131">hello right panel changes tooedit, and gives a selection of options:</span></span>

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="df1f0-134">Vizualizace dlaždice</span><span class="sxs-lookup"><span data-stu-id="df1f0-134">Tile visualizations</span></span>
<span data-ttu-id="df1f0-135">Existují tři druhy toochoose vizualizace dlaždice z:</span><span class="sxs-lookup"><span data-stu-id="df1f0-135">There are three kinds of tile visualizations toochoose from:</span></span>

| <span data-ttu-id="df1f0-136">Typ grafu</span><span class="sxs-lookup"><span data-stu-id="df1f0-136">chart type</span></span> | <span data-ttu-id="df1f0-137">Jak funguje</span><span class="sxs-lookup"><span data-stu-id="df1f0-137">what it does</span></span> |
| --- | --- |
| ![Pruhový graf](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="df1f0-139">Časová osa výsledků hledání uložený protokol zobrazí jako pruhový graf nebo seznam výsledků podle pole v závislosti na tom, pokud vyhledávání protokolu agreguje výsledky podle pole, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="df1f0-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![Metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="df1f0-141">Zobrazí vaše přístupů výsledek hledání celkový protokolu jako číslo v dlaždici.</span><span class="sxs-lookup"><span data-stu-id="df1f0-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="df1f0-142">Metriky dlaždice povolit tooset prahovou hodnotu, která bude zvýrazněte hello dlaždice, když je dosaženo prahové hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="df1f0-142">Metric tiles allow you tooset a threshold that will highlight hello tile when hello threshold is reached.</span></span> |
| ![řádek](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="df1f0-144">Jako spojnicový graf zobrazuje časová osa přístupů vaší uložený protokol hledání výsledek s hodnotami.</span><span class="sxs-lookup"><span data-stu-id="df1f0-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="df1f0-145">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="df1f0-145">Threshold</span></span>
<span data-ttu-id="df1f0-146">Na dlaždici pomocí hello metriky vizualizace můžete vytvořit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-146">You can create a threshold on a tile using hello Metric visualization.</span></span> <span data-ttu-id="df1f0-147">Vyberte na toocreate prahovou hodnotu na dlaždici hello.</span><span class="sxs-lookup"><span data-stu-id="df1f0-147">Select on toocreate a threshold value on hello tile.</span></span> <span data-ttu-id="df1f0-148">Zvolte, zda toohighlight hello dlaždice, pokud je hodnota hello nad nebo pod prahovou hodnotou hello vybrali, pak nastavit mezní hodnotu hello níže.</span><span class="sxs-lookup"><span data-stu-id="df1f0-148">Choose whether toohighlight hello tile when hello value is over or under hello chosen threshold, then set hello threshold value below.</span></span>

## <a name="organizing-hello-dashboard"></a><span data-ttu-id="df1f0-149">Uspořádání hello řídicí panel</span><span class="sxs-lookup"><span data-stu-id="df1f0-149">Organizing hello dashboard</span></span>
<span data-ttu-id="df1f0-150">tooorganize přejděte toohello vlastní řídicí panel zobrazení řídicího panelu a klikněte na **přizpůsobit** tooenter přizpůsobit režimu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-150">tooorganize your dashboard, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="df1f0-151">Klikněte na tlačítko a přetáhněte dlaždici hello chcete toomove a přesunout ho toowhere chcete, aby vaše toobe dlaždice.</span><span class="sxs-lookup"><span data-stu-id="df1f0-151">Click and drag hello tile you want toomove, and move it toowhere you want your tile toobe.</span></span>

![Uspořádání řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="df1f0-153">Odebrat dlaždici</span><span class="sxs-lookup"><span data-stu-id="df1f0-153">Remove a tile</span></span>
<span data-ttu-id="df1f0-154">tooremove dlaždici, přejděte toohello vlastní řídicí panel zobrazit a klikněte na tlačítko **přizpůsobit** tooenter přizpůsobit režimu.</span><span class="sxs-lookup"><span data-stu-id="df1f0-154">tooremove a tile, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="df1f0-155">Vyberte hello dlaždice, které tooremove, a potom na pravém panelu hello vyberte **odebrat dlaždici**.</span><span class="sxs-lookup"><span data-stu-id="df1f0-155">Select hello tile you want tooremove, then on hello right panel select **Remove Tile**.</span></span>

![Odebrat dlaždici](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="df1f0-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df1f0-157">Next steps</span></span>
* <span data-ttu-id="df1f0-158">Vytvoření [výstrahy](log-analytics-alerts.md) v oznámení toogenerate analýzy protokolů a tooremediate problémů.</span><span class="sxs-lookup"><span data-stu-id="df1f0-158">Create [alerts](log-analytics-alerts.md) in Log Analytics toogenerate notifications and tooremediate problems.</span></span>
