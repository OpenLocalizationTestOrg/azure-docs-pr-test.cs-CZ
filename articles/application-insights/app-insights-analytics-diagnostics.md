---
title: "Inteligentní diagnostiky změny výkonu webové aplikace ve službě Azure Application Insights | Microsoft Docs"
description: "Automatickou diagnostiku špičky nebo kroky v výkonu telemetrie z vaší webové aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 5e53bc714d89bf6204681349e7890e0b8fbc7046
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a><span data-ttu-id="6c2b5-103">Diagnostika nečekané změny v telemetrii aplikace</span><span class="sxs-lookup"><span data-stu-id="6c2b5-103">Diagnose sudden changes in your app telemetry</span></span>

<span data-ttu-id="6c2b5-104">*Tato funkce je ve verzi preview.*</span><span class="sxs-lookup"><span data-stu-id="6c2b5-104">*This feature is in preview.*</span></span>

<span data-ttu-id="6c2b5-105">Diagnostika nečekané změny ve vaší webové aplikace výkonu a využití s jedním kliknutím!</span><span class="sxs-lookup"><span data-stu-id="6c2b5-105">Diagnose sudden changes in your web app’s performance or usage with a single click!</span></span> <span data-ttu-id="6c2b5-106">Inteligentní diagnostiky funkce je k dispozici vždy, když vytváříte graf doby zpracování v [Analytics](app-insights-analytics.md) v [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c2b5-106">The Smart Diagnostics feature is available whenever you create a time chart in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="6c2b5-107">Vždy, když dojde neobvyklou změnu z trend výsledků, například Špička nebo dip, identifikuje inteligentní diagnostiky vzor dimenzí a související hodnoty, které mohou vysvětlit změny.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-107">Wherever there is an unusual change from the trend of your results, such as a spike or a dip, Smart Diagnostics identifies a pattern of dimensions and related values that might explain the change.</span></span> <span data-ttu-id="6c2b5-108">Díky tomu můžete rychle diagnostikovat problém.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-108">This helps you diagnose the problem quickly.</span></span> 

<span data-ttu-id="6c2b5-109">V tomto příkladu inteligentní diagnostiky nalezen se vzor přidružené změny hodnot vlastností a zvýrazňuje rozdíl mezi výsledky a bez tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="6c2b5-109">In this example, Smart Diagnostics has identified a pattern of property values associated with the change, and highlights the difference between results with and without that pattern:</span></span>

![Příklad analytics diagnostiky výsledků](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a><span data-ttu-id="6c2b5-111">Diagnostika změny dat</span><span class="sxs-lookup"><span data-stu-id="6c2b5-111">Diagnose data changes</span></span>

1.  <span data-ttu-id="6c2b5-112">Spuštění dotazu v analýzy a vykreslit ho jako graf doby zpracování.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-112">Run a query in Analytics, and render it as a time chart.</span></span> 
2.  <span data-ttu-id="6c2b5-113">Klikněte na libovolného bodu zvýrazněné ve špičce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-113">Click any highlighted peak point, if there is one.</span></span>
 
    ![bod ve špičce](./media/app-insights-analytics-diagnostics/peak.png)

    <span data-ttu-id="6c2b5-115">Diagnostika trvá několik sekund zjistit vzor.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-115">Diagnostics takes a few seconds to discover a pattern.</span></span>

3. <span data-ttu-id="6c2b5-116">Na kartě diagnostiky výsledků zobrazuje vzor, který může vysvětlují nespojitost vaše data.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-116">The Diagnostics Results tab shows a pattern which may explain your data discontinuity.</span></span>

    ![výsledek](./media/app-insights-analytics-diagnostics/result.png)
 
    <span data-ttu-id="6c2b5-118">Text zobrazuje hodnoty dimenze, které se zobrazují ke korelaci s k posunu.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-118">The text shows the dimension values that appear to correlate with the shift.</span></span> <span data-ttu-id="6c2b5-119">V tomto příkladu je spojen s určité žádosti a verzí určitého prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-119">In this example, it’s associated with a particular request and a particular browser version.</span></span>

    <span data-ttu-id="6c2b5-120">Všimněte si také dvě součásti grafu se filtr true a false.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-120">Notice also the two components of the chart, with the filter true and false.</span></span> <span data-ttu-id="6c2b5-121">Komponentu false ukazuje beze změny trendu.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-121">The false component shows an unchanged trend.</span></span> <span data-ttu-id="6c2b5-122">Jinými slovy se nezměnila ve výsledcích telemetrie, pokud jsou vyloučeny problematické kombinace dimenzí, které má identifikovat diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-122">In other words, there is no change in the telemetry results, if we exclude the problematic combination of dimensions that Diagnostics has identified.</span></span> <span data-ttu-id="6c2b5-123">Naopak zobrazí výsledky v rámci této kombinace výrazné změny z oblasti zvýrazněná šetření.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-123">By contrast, the results within that combination do show a dramatic change within the highlighted area of investigation.</span></span> <span data-ttu-id="6c2b5-124">Ukazuje to, že diagnostiky nalezl kombinace vlastnosti, která vysvětluje změny.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-124">This shows that Diagnostics has found a combination of properties that explains the change.</span></span>

4.  <span data-ttu-id="6c2b5-125">Pokud vzor je komplexní, budete muset pozastavte ukazatel myši nad **Zobrazit vše** zobrazíte dimenze.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-125">If the pattern is complex, you need to hover over **Show all** to see the dimensions.</span></span>

    ![zobrazit vše](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  <span data-ttu-id="6c2b5-127">V případě, že diagnostiky najde žádné významné vzor k oznamování, budou uvedeny stránce žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-127">In case Diagnostics finds no significant pattern to notify about, the ‘no results’ page will be presented.</span></span> <span data-ttu-id="6c2b5-128">V tomto okamžiku můžete změnit svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-128">At this point, you may change your query.</span></span> <span data-ttu-id="6c2b5-129">Například můžete zúžit časové rozmezí a přihrádkování v dotazu Analytics pro další analýzu a potenciálně lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-129">For example, you could narrow the time range and binning in Analytics query, for a further analysis and potentially better results.</span></span>

<span data-ttu-id="6c2b5-130">Díky znalosti, že konkrétní stránky vašeho webu došlo k problému v určitého prohlížeče, teď můžete přejít přímo na stránku problém a prozkoumat nedávné změny.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-130">Armed with the knowledge that a particular page of your website has a problem on a particular browser, you can now go straight to the problem page, and investigate recent changes.</span></span>

## <a name="try-the-demo"></a><span data-ttu-id="6c2b5-131">Vyzkoušet ukázkovou verzi</span><span class="sxs-lookup"><span data-stu-id="6c2b5-131">Try the demo</span></span>

<span data-ttu-id="6c2b5-132">[Kliknutím sem zobrazíte ukázka](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) na ukázková data.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-132">[Click here to see a demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) on sample data.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6c2b5-133">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="6c2b5-133">How it works</span></span>

<span data-ttu-id="6c2b5-134">Inteligentní diagnostiky používá nepodporovaný algoritmus učení pokročilé bez dohledu počítače na základě [DiffPatterns](app-insights-analytics-reference.md) operaci.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-134">Smart Diagnostics uses an advanced unsupervised machine learning algorithm based on the [DiffPatterns](app-insights-analytics-reference.md) operation.</span></span> <span data-ttu-id="6c2b5-135">Vypadá to candidate vzorů, které mohou vysvětlit, změny dat.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-135">It looks for candidate patterns that might explain the data change.</span></span> <span data-ttu-id="6c2b5-136">Analyzuje dopad kandidáti na metriku a zobrazuje vzor, nejlépe koreluje s změnu.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-136">It analyses the impact of each candidate on the metric, and shows the pattern that best correlates with the change.</span></span>

## <a name="no-diagnostic-points"></a><span data-ttu-id="6c2b5-137">Žádné diagnostické body?</span><span class="sxs-lookup"><span data-stu-id="6c2b5-137">No diagnostic points?</span></span>

<span data-ttu-id="6c2b5-138">Inteligentní diagnostiky funguje pouze pokud jsou splněny následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="6c2b5-138">Smart Diagnostics only works when the following criteria are satisfied:</span></span>

 * <span data-ttu-id="6c2b5-139">Inteligentní nastavení diagnostiky je zapnutá.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-139">Smart Diagnostics setting is switched on.</span></span> <span data-ttu-id="6c2b5-140">Podívejte se do části na ikonu nastavení v Analytics.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-140">Look under the Settings icon in Analytics.</span></span>
 * <span data-ttu-id="6c2b5-141">Je vybrána možnost Inteligentní diagnostiky v nastavení analýzy.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-141">The Smart Diagnostics option in Analytics settings is selected.</span></span> 
 * <span data-ttu-id="6c2b5-142">Časová osa: osy x grafu musí být typu `datetime`.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-142">Time axis: The X-axis of the chart must be of type `datetime`.</span></span>
 * <span data-ttu-id="6c2b5-143">Řádek nebo oblast grafu: Diagnostics lze použít pouze tyto typy grafu.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-143">Line or area chart: Diagnostics only works these types of chart.</span></span> <span data-ttu-id="6c2b5-144">Použití `| render timechart` nebo `| render areachart` na konci tohoto dotazu; nebo vyberte řádek nebo plošný graf z rozevíracího seznamu výběru.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-144">Use `| render timechart` or `| render areachart` at the end of your query; or select Line or Area Chart from the drop-down selector.</span></span>
 * <span data-ttu-id="6c2b5-145">Nespojitost: Musí být významné nespojitost v datech.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-145">Discontinuity: There must be a significant discontinuity in the data.</span></span>
 * <span data-ttu-id="6c2b5-146">Dostatečná body k analýze.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-146">Sufficient points to analyze.</span></span>
 * <span data-ttu-id="6c2b5-147">Víc než jeden shrnují klauzule dotazu.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-147">No more than one summarize clause in the query.</span></span>
 * <span data-ttu-id="6c2b5-148">Žádné projektu klauzuli, která obsahuje název definice před klauzuli summarize.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-148">No project clause that contains a name definition before the summarize clause.</span></span>

 
 ## <a name="related-articles"></a><span data-ttu-id="6c2b5-149">Související články</span><span class="sxs-lookup"><span data-stu-id="6c2b5-149">Related articles</span></span>

 * [<span data-ttu-id="6c2b5-150">Analýza kurzu</span><span class="sxs-lookup"><span data-stu-id="6c2b5-150">Analytics tutorial</span></span>](app-insights-analytics-tour.md)
 * <span data-ttu-id="6c2b5-151">[Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky upozorňuje na problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="6c2b5-151">[Smart detection](app-insights-proactive-diagnostics.md) automatically alerts you to performance issues.</span></span>