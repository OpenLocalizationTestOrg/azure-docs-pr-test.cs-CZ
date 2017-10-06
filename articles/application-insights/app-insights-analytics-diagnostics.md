---
title: "aaaSmart diagnostiku změny výkonu webové aplikace ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 8891762c4a4bfdb08b647fe3b702349eb30ec9c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a><span data-ttu-id="c4991-103">Diagnostika nečekané změny v telemetrii aplikace</span><span class="sxs-lookup"><span data-stu-id="c4991-103">Diagnose sudden changes in your app telemetry</span></span>

<span data-ttu-id="c4991-104">*Tato funkce je ve verzi preview.*</span><span class="sxs-lookup"><span data-stu-id="c4991-104">*This feature is in preview.*</span></span>

<span data-ttu-id="c4991-105">Diagnostika nečekané změny ve vaší webové aplikace výkonu a využití s jedním kliknutím!</span><span class="sxs-lookup"><span data-stu-id="c4991-105">Diagnose sudden changes in your web app’s performance or usage with a single click!</span></span> <span data-ttu-id="c4991-106">Hello inteligentní diagnostiky funkce je k dispozici vždy, když vytváříte graf doby zpracování v [Analytics](app-insights-analytics.md) v [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4991-106">hello Smart Diagnostics feature is available whenever you create a time chart in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="c4991-107">Vždy, když dojde neobvyklou změnu z hello trend výsledků, například Špička nebo dip, identifikuje inteligentní diagnostiky vzor dimenzí a související hodnoty, které mohou vysvětlit hello změnu.</span><span class="sxs-lookup"><span data-stu-id="c4991-107">Wherever there is an unusual change from hello trend of your results, such as a spike or a dip, Smart Diagnostics identifies a pattern of dimensions and related values that might explain hello change.</span></span> <span data-ttu-id="c4991-108">Díky tomu můžete rychle diagnostikovat problém hello.</span><span class="sxs-lookup"><span data-stu-id="c4991-108">This helps you diagnose hello problem quickly.</span></span> 

<span data-ttu-id="c4991-109">V tomto příkladu inteligentní diagnostiky nalezen se vzor hodnot vlastností, které jsou přidružené k hello změnu a zvýrazňuje hello rozdíl mezi výsledky a bez tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="c4991-109">In this example, Smart Diagnostics has identified a pattern of property values associated with hello change, and highlights hello difference between results with and without that pattern:</span></span>

![Příklad analytics diagnostiky výsledků](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a><span data-ttu-id="c4991-111">Diagnostika změny dat</span><span class="sxs-lookup"><span data-stu-id="c4991-111">Diagnose data changes</span></span>

1.  <span data-ttu-id="c4991-112">Spuštění dotazu v analýzy a vykreslit ho jako graf doby zpracování.</span><span class="sxs-lookup"><span data-stu-id="c4991-112">Run a query in Analytics, and render it as a time chart.</span></span> 
2.  <span data-ttu-id="c4991-113">Klikněte na libovolného bodu zvýrazněné ve špičce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="c4991-113">Click any highlighted peak point, if there is one.</span></span>
 
    ![bod ve špičce](./media/app-insights-analytics-diagnostics/peak.png)

    <span data-ttu-id="c4991-115">Diagnostika trvá několik sekund toodiscover vzor.</span><span class="sxs-lookup"><span data-stu-id="c4991-115">Diagnostics takes a few seconds toodiscover a pattern.</span></span>

3. <span data-ttu-id="c4991-116">Hello diagnostiky výsledků kartě se zobrazují vzor, který může vysvětlují nespojitost vaše data.</span><span class="sxs-lookup"><span data-stu-id="c4991-116">hello Diagnostics Results tab shows a pattern which may explain your data discontinuity.</span></span>

    ![výsledek](./media/app-insights-analytics-diagnostics/result.png)
 
    <span data-ttu-id="c4991-118">Hello text znázorňuje hodnoty hello dimenze, které se zobrazují toocorrelate s hello shift.</span><span class="sxs-lookup"><span data-stu-id="c4991-118">hello text shows hello dimension values that appear toocorrelate with hello shift.</span></span> <span data-ttu-id="c4991-119">V tomto příkladu je spojen s určité žádosti a verzí určitého prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c4991-119">In this example, it’s associated with a particular request and a particular browser version.</span></span>

    <span data-ttu-id="c4991-120">Všimněte si také hello dvě součásti hello grafu s hello filtru true a false.</span><span class="sxs-lookup"><span data-stu-id="c4991-120">Notice also hello two components of hello chart, with hello filter true and false.</span></span> <span data-ttu-id="c4991-121">false součást Hello ukazuje beze změny trendu.</span><span class="sxs-lookup"><span data-stu-id="c4991-121">hello false component shows an unchanged trend.</span></span> <span data-ttu-id="c4991-122">Jinými slovy se nezměnila ve výsledcích telemetrie hello, pokud jsou vyloučeny hello problematické kombinace dimenzí, které má identifikovat diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c4991-122">In other words, there is no change in hello telemetry results, if we exclude hello problematic combination of dimensions that Diagnostics has identified.</span></span> <span data-ttu-id="c4991-123">Naopak hello výsledky v rámci této kombinace zobrazit výrazné změny v rámci oblasti hello zvýrazněná šetření.</span><span class="sxs-lookup"><span data-stu-id="c4991-123">By contrast, hello results within that combination do show a dramatic change within hello highlighted area of investigation.</span></span> <span data-ttu-id="c4991-124">Ukazuje to, že diagnostiky nalezl kombinace vlastnosti, které popisuje změnu hello.</span><span class="sxs-lookup"><span data-stu-id="c4991-124">This shows that Diagnostics has found a combination of properties that explains hello change.</span></span>

4.  <span data-ttu-id="c4991-125">Pokud vzor hello je složité, je třeba toohover přes **Zobrazit vše** toosee hello dimenzí.</span><span class="sxs-lookup"><span data-stu-id="c4991-125">If hello pattern is complex, you need toohover over **Show all** toosee hello dimensions.</span></span>

    ![zobrazit vše](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  <span data-ttu-id="c4991-127">V případě, že diagnostiky vyhledá žádné významné vzor toonotify o hello, které se zobrazí stránka žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="c4991-127">In case Diagnostics finds no significant pattern toonotify about, hello ‘no results’ page will be presented.</span></span> <span data-ttu-id="c4991-128">V tomto okamžiku můžete změnit svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="c4991-128">At this point, you may change your query.</span></span> <span data-ttu-id="c4991-129">Například můžete zúžit hello časové rozmezí a přihrádkování v dotazu Analytics pro další analýzu a potenciálně lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="c4991-129">For example, you could narrow hello time range and binning in Analytics query, for a further analysis and potentially better results.</span></span>

<span data-ttu-id="c4991-130">Díky znalosti hello že určitou stránku vašeho webu došlo k problému v určitého prohlížeče, můžete nyní přejděte rovnou toohello problém stránku a prozkoumat nedávné změny.</span><span class="sxs-lookup"><span data-stu-id="c4991-130">Armed with hello knowledge that a particular page of your website has a problem on a particular browser, you can now go straight toohello problem page, and investigate recent changes.</span></span>

## <a name="try-hello-demo"></a><span data-ttu-id="c4991-131">Zkuste ukázku hello</span><span class="sxs-lookup"><span data-stu-id="c4991-131">Try hello demo</span></span>

<span data-ttu-id="c4991-132">[Kliknutím sem toosee ukázka](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) na ukázková data.</span><span class="sxs-lookup"><span data-stu-id="c4991-132">[Click here toosee a demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) on sample data.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="c4991-133">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="c4991-133">How it works</span></span>

<span data-ttu-id="c4991-134">Inteligentní diagnostiky používá nepodporovaný algoritmus pokročilé bez dohledu machine learning podle hello [DiffPatterns](app-insights-analytics-reference.md) operaci.</span><span class="sxs-lookup"><span data-stu-id="c4991-134">Smart Diagnostics uses an advanced unsupervised machine learning algorithm based on hello [DiffPatterns](app-insights-analytics-reference.md) operation.</span></span> <span data-ttu-id="c4991-135">Vypadá to candidate vzorů, které mohou vysvětlit, změny dat hello.</span><span class="sxs-lookup"><span data-stu-id="c4991-135">It looks for candidate patterns that might explain hello data change.</span></span> <span data-ttu-id="c4991-136">Analyzuje hello dopad kandidáti na hello metrika a zobrazuje hello vzor, nejlépe koreluje s hello změnu.</span><span class="sxs-lookup"><span data-stu-id="c4991-136">It analyses hello impact of each candidate on hello metric, and shows hello pattern that best correlates with hello change.</span></span>

## <a name="no-diagnostic-points"></a><span data-ttu-id="c4991-137">Žádné diagnostické body?</span><span class="sxs-lookup"><span data-stu-id="c4991-137">No diagnostic points?</span></span>

<span data-ttu-id="c4991-138">Inteligentní diagnostiky funguje pouze pokud jsou splněny hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="c4991-138">Smart Diagnostics only works when hello following criteria are satisfied:</span></span>

 * <span data-ttu-id="c4991-139">Inteligentní nastavení diagnostiky je zapnutá.</span><span class="sxs-lookup"><span data-stu-id="c4991-139">Smart Diagnostics setting is switched on.</span></span> <span data-ttu-id="c4991-140">Podívejte se do části ikonu hello nastavení v Analytics.</span><span class="sxs-lookup"><span data-stu-id="c4991-140">Look under hello Settings icon in Analytics.</span></span>
 * <span data-ttu-id="c4991-141">je vybrán Hello inteligentní diagnostiky možnost v nastavení analýzy.</span><span class="sxs-lookup"><span data-stu-id="c4991-141">hello Smart Diagnostics option in Analytics settings is selected.</span></span> 
 * <span data-ttu-id="c4991-142">Časová osa: hello osy x grafu hello musí být typu `datetime`.</span><span class="sxs-lookup"><span data-stu-id="c4991-142">Time axis: hello X-axis of hello chart must be of type `datetime`.</span></span>
 * <span data-ttu-id="c4991-143">Řádek nebo oblast grafu: Diagnostics lze použít pouze tyto typy grafu.</span><span class="sxs-lookup"><span data-stu-id="c4991-143">Line or area chart: Diagnostics only works these types of chart.</span></span> <span data-ttu-id="c4991-144">Použití `| render timechart` nebo `| render areachart` na konci hello tohoto dotazu; nebo vyberte řádek nebo plošný graf z rozevíracího seznamu pro výběr hello.</span><span class="sxs-lookup"><span data-stu-id="c4991-144">Use `| render timechart` or `| render areachart` at hello end of your query; or select Line or Area Chart from hello drop-down selector.</span></span>
 * <span data-ttu-id="c4991-145">Nespojitost: Musí být významné nespojitost v datech hello.</span><span class="sxs-lookup"><span data-stu-id="c4991-145">Discontinuity: There must be a significant discontinuity in hello data.</span></span>
 * <span data-ttu-id="c4991-146">Tooanalyze dostatek bodů.</span><span class="sxs-lookup"><span data-stu-id="c4991-146">Sufficient points tooanalyze.</span></span>
 * <span data-ttu-id="c4991-147">Více než jeden shrnují klauzuli hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="c4991-147">No more than one summarize clause in hello query.</span></span>
 * <span data-ttu-id="c4991-148">Žádné klauzule projektu, který obsahuje název definice před hello shrnují klauzule.</span><span class="sxs-lookup"><span data-stu-id="c4991-148">No project clause that contains a name definition before hello summarize clause.</span></span>

 
 ## <a name="related-articles"></a><span data-ttu-id="c4991-149">Související články</span><span class="sxs-lookup"><span data-stu-id="c4991-149">Related articles</span></span>

 * [<span data-ttu-id="c4991-150">Analýza kurzu</span><span class="sxs-lookup"><span data-stu-id="c4991-150">Analytics tutorial</span></span>](app-insights-analytics-tour.md)
 * <span data-ttu-id="c4991-151">[Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky upozorňuje tooperformance problémy.</span><span class="sxs-lookup"><span data-stu-id="c4991-151">[Smart detection](app-insights-proactive-diagnostics.md) automatically alerts you tooperformance issues.</span></span>
