---
title: "Zkoumání metriky ve službě Azure Application Insights | Microsoft Docs"
description: "Interpretace grafy na metriky Průzkumníka a postup přizpůsobení okna Průzkumníka metriky."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: a13500284caab79bbe221060ccf3d925ffb1fab5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-metrics-in-application-insights"></a><span data-ttu-id="4c564-103">Zkoumání metriky ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="4c564-103">Exploring Metrics in Application Insights</span></span>
<span data-ttu-id="4c564-104">Metriky v [Application Insights] [ start] jsou měřené hodnoty a počet událostí, které se odesílají v telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c564-104">Metrics in [Application Insights][start] are measured values and counts of events that are sent in telemetry from your application.</span></span> <span data-ttu-id="4c564-105">Pomáhají zjistit problémy s výkonem a sledujte trendy ve využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c564-105">They help you detect performance issues and watch trends in how your application is being used.</span></span> <span data-ttu-id="4c564-106">Je širokou škálu standardní metriky a můžete také vytvořit vlastní vlastní metriky a události.</span><span class="sxs-lookup"><span data-stu-id="4c564-106">There's a wide range of standard metrics, and you can also create your own custom metrics and events.</span></span>

<span data-ttu-id="4c564-107">Počet metriky a události se zobrazí v grafech agregované hodnoty, jako je například součtů a průměry, počty.</span><span class="sxs-lookup"><span data-stu-id="4c564-107">Metrics and event counts are displayed in charts of aggregated values such as sums, averages, or counts.</span></span>

<span data-ttu-id="4c564-108">Zde je ukázka sadu grafy:</span><span class="sxs-lookup"><span data-stu-id="4c564-108">Here's a sample set of charts:</span></span>

![](./media/app-insights-metrics-explorer/01-overview.png)

<span data-ttu-id="4c564-109">Metriky grafy všude, kde zjistíte v portálu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4c564-109">You find metrics charts everywhere in the Application Insights portal.</span></span> <span data-ttu-id="4c564-110">Ve většině případů lze přizpůsobit a další grafy můžete přidat do okna.</span><span class="sxs-lookup"><span data-stu-id="4c564-110">In most cases, they can be customized, and you can add more charts to the blade.</span></span> <span data-ttu-id="4c564-111">V okně Přehled kliknutím prostřednictvím podrobnější grafy (která mají názvy, například "Servery"), nebo klikněte na tlačítko **Průzkumníku metrik** otevřete nové okno, kde můžete vytvořit vlastní grafy.</span><span class="sxs-lookup"><span data-stu-id="4c564-111">From the Overview blade, click through to more detailed charts (which have titles such as "Servers"), or click **Metrics Explorer** to open a new blade where you can create custom charts.</span></span>

## <a name="time-range"></a><span data-ttu-id="4c564-112">Časové rozmezí</span><span class="sxs-lookup"><span data-stu-id="4c564-112">Time range</span></span>
<span data-ttu-id="4c564-113">Můžete změnit časový rozsah, který je předmětem grafy nebo mřížky na libovolné okno.</span><span class="sxs-lookup"><span data-stu-id="4c564-113">You can change the Time range covered by the charts or grids on any blade.</span></span>

![Otevře se okno Přehled vaší aplikace na portálu Azure](./media/app-insights-metrics-explorer/03-range.png)

<span data-ttu-id="4c564-115">Pokud očekáváte data, která ještě nebyla zobrazovaly, klikněte na tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4c564-115">If you're expecting some data that hasn't appeared yet, click Refresh.</span></span> <span data-ttu-id="4c564-116">Grafy sami aktualizace v intervalech, ale jsou delší dobu, větší časových rozsahů intervalů.</span><span class="sxs-lookup"><span data-stu-id="4c564-116">Charts refresh themselves at intervals, but the intervals are longer for larger time ranges.</span></span> <span data-ttu-id="4c564-117">Může trvat nějakou dobu dat do režimu prostřednictvím kanálu analýzy do grafu.</span><span class="sxs-lookup"><span data-stu-id="4c564-117">It can take a while for data to come through the analysis pipeline onto a chart.</span></span>

<span data-ttu-id="4c564-118">Pro zvětšení část grafu, přetáhněte nad ním:</span><span class="sxs-lookup"><span data-stu-id="4c564-118">To zoom into part of a chart, drag over it:</span></span>

![Přetažením přes část grafu.](./media/app-insights-metrics-explorer/12-drag.png)

<span data-ttu-id="4c564-120">Klikněte na tlačítko zpět zvětšení pro jeho obnovení.</span><span class="sxs-lookup"><span data-stu-id="4c564-120">Click the Undo Zoom button to restore it.</span></span>

## <a name="granularity-and-point-values"></a><span data-ttu-id="4c564-121">Hodnoty členitosti a bodu</span><span class="sxs-lookup"><span data-stu-id="4c564-121">Granularity and point values</span></span>
<span data-ttu-id="4c564-122">Najeďte myší na graf k zobrazení hodnot metrik v tomto bodě.</span><span class="sxs-lookup"><span data-stu-id="4c564-122">Hover your mouse over the chart to display the values of the metrics at that point.</span></span>

![Najeďte myší na graf](./media/app-insights-metrics-explorer/02-focus.png)

<span data-ttu-id="4c564-124">Hodnota metriky na určitém místě je agregován v předchozím intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="4c564-124">The value of the metric at a particular point is aggregated over the preceding sampling interval.</span></span>

<span data-ttu-id="4c564-125">Interval vzorkování nebo "členitosti" se zobrazí v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="4c564-125">The sampling interval or "granularity" is shown at the top of the blade.</span></span>

![Hlavička okno.](./media/app-insights-metrics-explorer/11-grain.png)

<span data-ttu-id="4c564-127">Můžete upravit členitost v okně rozsah čas:</span><span class="sxs-lookup"><span data-stu-id="4c564-127">You can adjust the granularity in the Time range blade:</span></span>

![Hlavička okno.](./media/app-insights-metrics-explorer/grain.png)

<span data-ttu-id="4c564-129">K dispozici členitostí závisí na časový rozsah, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4c564-129">The granularities available depend on the time range you select.</span></span> <span data-ttu-id="4c564-130">Explicitní členitostí jsou alternativy "automatické" členitost časovém rozmezí.</span><span class="sxs-lookup"><span data-stu-id="4c564-130">The explicit granularities are alternatives to the "automatic" granularity for the time range.</span></span>


## <a name="editing-charts-and-grids"></a><span data-ttu-id="4c564-131">Úpravy grafy a mřížky</span><span class="sxs-lookup"><span data-stu-id="4c564-131">Editing charts and grids</span></span>
<span data-ttu-id="4c564-132">Chcete-li přidat nový graf do okna:</span><span class="sxs-lookup"><span data-stu-id="4c564-132">To add a new chart to the blade:</span></span>

![V Průzkumníku metrik zvolte přidat graf](./media/app-insights-metrics-explorer/04-add.png)

<span data-ttu-id="4c564-134">Vyberte **upravit** u stávajícího nebo nového grafu upravit, se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="4c564-134">Select **Edit** on an existing or new chart to edit what it shows:</span></span>

![Vyberte jeden nebo více metriky](./media/app-insights-metrics-explorer/08-select.png)

<span data-ttu-id="4c564-136">Více než jeden metrika můžete zobrazit v grafu, i když existují omezení o kombinacích, které lze zobrazit najednou.</span><span class="sxs-lookup"><span data-stu-id="4c564-136">You can display more than one metric on a chart, though there are restrictions about the combinations that can be displayed together.</span></span> <span data-ttu-id="4c564-137">Jakmile vyberete jeden metrika, některé jiné jsou zakázány.</span><span class="sxs-lookup"><span data-stu-id="4c564-137">As soon as you choose one metric, some of the others are disabled.</span></span>

<span data-ttu-id="4c564-138">Pokud jste programového [vlastní metriky] [ track] do vaší aplikace (volání TrackMetric a TrackEvent) budou uvedené v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="4c564-138">If you coded [custom metrics][track] into your app (calls to TrackMetric and TrackEvent) they will be listed here.</span></span>

## <a name="segment-your-data"></a><span data-ttu-id="4c564-139">Segmentovat vaše data</span><span class="sxs-lookup"><span data-stu-id="4c564-139">Segment your data</span></span>
<span data-ttu-id="4c564-140">Rozdělte metriky vlastnost – například k porovnání zobrazení stránky na klientských počítačích s jinými operačními systémy.</span><span class="sxs-lookup"><span data-stu-id="4c564-140">You can split a metric by property - for example, to compare page views on clients with different operating systems.</span></span>

<span data-ttu-id="4c564-141">Vyberte graf nebo tabulku, přepínač na seskupení a vyberte vlastnosti, které chcete seskupit podle:</span><span class="sxs-lookup"><span data-stu-id="4c564-141">Select a chart or grid, switch on grouping and pick a property to group by:</span></span>

![Vyberte seskupení na a pak sadu vyberte vlastnost v Seskupit podle](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> <span data-ttu-id="4c564-143">Pokud používáte seskupení, zadejte typy oblasti a pruhový graf skládaný zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4c564-143">When you use grouping, the Area and Bar chart types provide a stacked display.</span></span> <span data-ttu-id="4c564-144">Toto je vhodný, kde je metoda agregace Sum.</span><span class="sxs-lookup"><span data-stu-id="4c564-144">This is suitable where the Aggregation method is Sum.</span></span> <span data-ttu-id="4c564-145">Ale kde typ agregace je průměr, vyberte řádek nebo Mřížka typů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4c564-145">But where the aggregation type is Average, choose the Line or Grid display types.</span></span>
>
>

<span data-ttu-id="4c564-146">Pokud jste programového [vlastní metriky] [ track] do vaší aplikace a obsahují hodnoty vlastností, budete moct v seznamu vyberte vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4c564-146">If you coded [custom metrics][track] into your app and they include property values, you'll be able to select the property in the list.</span></span>

<span data-ttu-id="4c564-147">Je příliš malá pro segmentovaným data grafu?</span><span class="sxs-lookup"><span data-stu-id="4c564-147">Is the chart too small for segmented data?</span></span> <span data-ttu-id="4c564-148">Nastavte jeho výšku:</span><span class="sxs-lookup"><span data-stu-id="4c564-148">Adjust its height:</span></span>

![Nastavte posuvník](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a><span data-ttu-id="4c564-150">Typy agregace</span><span class="sxs-lookup"><span data-stu-id="4c564-150">Aggregation types</span></span>
<span data-ttu-id="4c564-151">Legendu na stranu ve výchozím nastavení obvykle zobrazuje agregovaná hodnota období grafu.</span><span class="sxs-lookup"><span data-stu-id="4c564-151">The legend at the side by default usually shows the aggregated value over the period of the chart.</span></span> <span data-ttu-id="4c564-152">Pokud je ukazatel myši nad grafu, zobrazuje hodnotu v tomto bodě.</span><span class="sxs-lookup"><span data-stu-id="4c564-152">If you hover over the chart, it shows the value at that point.</span></span>

<span data-ttu-id="4c564-153">Každý datový bod na graf je agregace hodnot dat přijatých v předchozí interval vzorkování nebo "členitosti".</span><span class="sxs-lookup"><span data-stu-id="4c564-153">Each data point on the chart is an aggregate of the data values received in the preceding sampling interval or "granularity".</span></span> <span data-ttu-id="4c564-154">Členitost se zobrazí v horní části okna a se liší podle celkové časová osa grafu.</span><span class="sxs-lookup"><span data-stu-id="4c564-154">The granularity is shown at the top of the blade, and varies with the overall timescale of the chart.</span></span>

<span data-ttu-id="4c564-155">Metriky lze agregovat různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="4c564-155">Metrics can be aggregated in different ways:</span></span>

* <span data-ttu-id="4c564-156">**Počet** je počet událostí přijatých v intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="4c564-156">**Count** is a count of the events received in the sampling interval.</span></span> <span data-ttu-id="4c564-157">Používá se pro události, jako jsou žádosti o.</span><span class="sxs-lookup"><span data-stu-id="4c564-157">It is used for events such as requests.</span></span> <span data-ttu-id="4c564-158">Rozdíly v výška grafu označuje rozdíly v rychlost, kdy dochází k události.</span><span class="sxs-lookup"><span data-stu-id="4c564-158">Variations in the height of the chart indicates variations in the rate at which the events occur.</span></span> <span data-ttu-id="4c564-159">Ale Všimněte si, že číselná hodnota se změní při změně intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="4c564-159">But note that the numeric value changes when you change the sampling interval.</span></span>
* <span data-ttu-id="4c564-160">**Součet** přidá hodnot všech datových bodů přijatých prostřednictvím intervalu vzorkování nebo po dobu grafu.</span><span class="sxs-lookup"><span data-stu-id="4c564-160">**Sum** adds up the values of all the data points received over the sampling interval, or the period of the chart.</span></span>
* <span data-ttu-id="4c564-161">**Průměrná** rozdělí součet podle počtu přijatých prostřednictvím interval datových bodů.</span><span class="sxs-lookup"><span data-stu-id="4c564-161">**Average** divides the Sum by the number of data points received over the interval.</span></span>
* <span data-ttu-id="4c564-162">**Jedinečné** počty se používají pro počty uživatelů a účty.</span><span class="sxs-lookup"><span data-stu-id="4c564-162">**Unique** counts are used for counts of users and accounts.</span></span> <span data-ttu-id="4c564-163">Během intervalu vzorkování, nebo za období grafu obrázku počet různých uživatelé vidět v té době.</span><span class="sxs-lookup"><span data-stu-id="4c564-163">Over the sampling interval, or over the period of the chart, the figure shows the count of different users seen in that time.</span></span>
* <span data-ttu-id="4c564-164">**%**-Procento verze každé agregace se používají pouze s segmentovaným grafy.</span><span class="sxs-lookup"><span data-stu-id="4c564-164">**%** - percentage versions of each aggregation are used only with segmented charts.</span></span> <span data-ttu-id="4c564-165">Celkový počet vždy přidá až o 100 % a graf zobrazuje relativní příspěvek různé komponenty celkem.</span><span class="sxs-lookup"><span data-stu-id="4c564-165">The total always adds up to 100%, and the chart shows the relative contribution of different components of a total.</span></span>

    ![Procento agregace](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a><span data-ttu-id="4c564-167">Změnit typ agregace</span><span class="sxs-lookup"><span data-stu-id="4c564-167">Change the aggregation type</span></span>

![Upravit graf a pak vyberte agregace](./media/app-insights-metrics-explorer/05-aggregation.png)

<span data-ttu-id="4c564-169">Když vytvoříte nový graf nebo když jsou všechny metriky není vybraná, je zobrazena výchozí metodou pro jednotlivé metriky:</span><span class="sxs-lookup"><span data-stu-id="4c564-169">The default method for each metric is shown when you create a new chart or when all metrics are deselected:</span></span>

![Zrušte výběr všechny metriky, které chcete zobrazit výchozí hodnoty](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a><span data-ttu-id="4c564-171">Osy y PIN kódu</span><span class="sxs-lookup"><span data-stu-id="4c564-171">Pin Y-axis</span></span> 
<span data-ttu-id="4c564-172">Ve výchozím nastavení graf znázorňuje hodnoty osy Y od nuly do maximální hodnoty v oblasti dat, umožnit vizuální reprezentace quantum hodnot.</span><span class="sxs-lookup"><span data-stu-id="4c564-172">By default a chart shows Y axis values starting from zero till maximum values in the data range, to give a visual representation of quantum of the values.</span></span> <span data-ttu-id="4c564-173">Ale v některých případech víc než quantum mohou být zajímavé vizuální kontrola malých změn v hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4c564-173">But in some cases more than the quantum it might be interesting to visually inspect minor changes in values.</span></span> <span data-ttu-id="4c564-174">Přizpůsobení jako tuto použijte rozsahu osy y úpravy funkce připnete osy y minimální nebo maximální hodnotu na požadovaný místě.</span><span class="sxs-lookup"><span data-stu-id="4c564-174">For customizations like this use the Y-axis range editing feature to pin the Y-axis minimum or maximum value at desired place.</span></span>
<span data-ttu-id="4c564-175">Klikněte na zaškrtávací políčko "Upřesnit nastavení" se zprovoznit osy y rozsah nastavení</span><span class="sxs-lookup"><span data-stu-id="4c564-175">Click on "Advanced Settings" check box to bring up the Y-axis range Settings</span></span>

![Klikněte na tlačítko Upřesnit nastavení, vyberte vlastní rozsah a zadejte maximální hodnoty min.](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a><span data-ttu-id="4c564-177">Filtrování dat</span><span class="sxs-lookup"><span data-stu-id="4c564-177">Filter your data</span></span>
<span data-ttu-id="4c564-178">Pokud chcete zobrazit jenom metriky pro vybranou sadu hodnot vlastností:</span><span class="sxs-lookup"><span data-stu-id="4c564-178">To see just the metrics for a selected set of property values:</span></span>

![Klikněte na tlačítko filtru, rozbalte vlastnost a zkontrolujte některé hodnoty](./media/app-insights-metrics-explorer/19-filter.png)

<span data-ttu-id="4c564-180">Pokud nevyberete žádné hodnoty pro požadovanou vlastnost, je stejná jako jejich všechny výběrem: pro tuto vlastnost neexistuje žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="4c564-180">If you don't select any values for a particular property, it's the same as selecting them all: there is no filter on that property.</span></span>

<span data-ttu-id="4c564-181">Všimněte si, počet událostí společně se všechny hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="4c564-181">Notice the counts of events alongside each property value.</span></span> <span data-ttu-id="4c564-182">Když vyberete hodnoty jednu vlastnost, upraví se počty vedle dalších hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="4c564-182">When you select values of one property, the counts alongside other property values are adjusted.</span></span>

<span data-ttu-id="4c564-183">Filtry platí pro všechny grafy v okně.</span><span class="sxs-lookup"><span data-stu-id="4c564-183">Filters apply to all the charts on a blade.</span></span> <span data-ttu-id="4c564-184">Pokud chcete použít na různých grafů různých filtrů, vytvořte a uložte okna jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="4c564-184">If you want different filters applied to different charts, create and save different metrics blades.</span></span> <span data-ttu-id="4c564-185">Pokud chcete, můžete připnout grafy z různých okna na řídicí panel, abyste je viděli vedle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="4c564-185">If you want, you can pin charts from different blades to the dashboard, so that you can see them alongside each other.</span></span>

### <a name="remove-bot-and-web-test-traffic"></a><span data-ttu-id="4c564-186">Odebrat robota a webové přenosy testu</span><span class="sxs-lookup"><span data-stu-id="4c564-186">Remove bot and web test traffic</span></span>
<span data-ttu-id="4c564-187">Pomocí filtru **skutečné nebo syntetické přenosů** a zkontrolujte **skutečné**.</span><span class="sxs-lookup"><span data-stu-id="4c564-187">Use the filter **Real or synthetic traffic** and check **Real**.</span></span>

<span data-ttu-id="4c564-188">Můžete také filtrovat podle **zdroj syntetické provoz**.</span><span class="sxs-lookup"><span data-stu-id="4c564-188">You can also filter by **Source of synthetic traffic**.</span></span>

### <a name="to-add-properties-to-the-filter-list"></a><span data-ttu-id="4c564-189">Přidání vlastnosti do seznamu filtru</span><span class="sxs-lookup"><span data-stu-id="4c564-189">To add properties to the filter list</span></span>
<span data-ttu-id="4c564-190">Chcete filtrovat telemetrii na kategorii podle vlastní volby?</span><span class="sxs-lookup"><span data-stu-id="4c564-190">Would you like to filter telemetry on a category of your own choosing?</span></span> <span data-ttu-id="4c564-191">Například možná zdola nahoru vaši uživatelé do různých kategorií a chcete segmentovat vaše data podle těchto kategorií.</span><span class="sxs-lookup"><span data-stu-id="4c564-191">For example, maybe you divide up your users into different categories, and you would like segment your data by these categories.</span></span>

<span data-ttu-id="4c564-192">[Vytvořit vlastní vlastnost](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="4c564-192">[Create your own property](app-insights-api-custom-events-metrics.md#properties).</span></span> <span data-ttu-id="4c564-193">Nastavit [Telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) jej zobrazit v všechny telemetrie – včetně standardní telemetrie poslal jiné moduly SDK.</span><span class="sxs-lookup"><span data-stu-id="4c564-193">Set it in a [Telemetry Initializer](app-insights-api-custom-events-metrics.md#defaults) to have it appear in all telemetry - including the standard telemetry sent by different SDK modules.</span></span>

## <a name="edit-the-chart-type"></a><span data-ttu-id="4c564-194">Upravit typ grafu.</span><span class="sxs-lookup"><span data-stu-id="4c564-194">Edit the chart type</span></span>
<span data-ttu-id="4c564-195">Všimněte si, že můžete přepínat mezi mřížky a grafy:</span><span class="sxs-lookup"><span data-stu-id="4c564-195">Notice that you can switch between grids and graphs:</span></span>

![Vyberte graf nebo mřížky, a potom vyberte typ grafu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a><span data-ttu-id="4c564-197">Uložit okně vaší metriky</span><span class="sxs-lookup"><span data-stu-id="4c564-197">Save your metrics blade</span></span>
<span data-ttu-id="4c564-198">Pokud jste vytvořili některé grafy, je uložte jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="4c564-198">When you've created some charts, save them as a favorite.</span></span> <span data-ttu-id="4c564-199">Můžete zvolit, zda sdílet s ostatními členy týmu, pokud používáte účet organizace.</span><span class="sxs-lookup"><span data-stu-id="4c564-199">You can choose whether to share it with other team members, if you use an organizational account.</span></span>

![Vyberte Oblíbené položky.](./media/app-insights-metrics-explorer/21-favorite-save.png)

<span data-ttu-id="4c564-201">Zobrazit okno znovu **přejděte do okna Přehled** a otevřete oblíbených položek:</span><span class="sxs-lookup"><span data-stu-id="4c564-201">To see the blade again, **go to the overview blade** and open Favorites:</span></span>

![V okně Přehled vyberte Oblíbené položky](./media/app-insights-metrics-explorer/22-favorite-get.png)

<span data-ttu-id="4c564-203">Pokud jste zvolili relativní časové rozmezí, když jste uložili, v okně bude aktualizována nejnovější metriky.</span><span class="sxs-lookup"><span data-stu-id="4c564-203">If you chose Relative time range when you saved, the blade will be updated with the latest metrics.</span></span> <span data-ttu-id="4c564-204">Pokud jste zvolili absolutní časové rozmezí, se zobrazí stejná data pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="4c564-204">If you chose Absolute time range, it will show the same data every time.</span></span>

## <a name="reset-the-blade"></a><span data-ttu-id="4c564-205">Resetování okna</span><span class="sxs-lookup"><span data-stu-id="4c564-205">Reset the blade</span></span>
<span data-ttu-id="4c564-206">Pokud upravíte okno, ale pak chcete získat zpět do původního uložit sadu, stačí kliknout na resetovat.</span><span class="sxs-lookup"><span data-stu-id="4c564-206">If you edit a blade but then you'd like to get back to the original saved set, just click Reset.</span></span>

![V tlačítka v horní části metrika Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a><span data-ttu-id="4c564-208">Datový proud za provozu metriky</span><span class="sxs-lookup"><span data-stu-id="4c564-208">Live metrics stream</span></span>

<span data-ttu-id="4c564-209">Zobrazení telemetrie a víc okamžitou otevřete [živý datový proud](app-insights-live-stream.md).</span><span class="sxs-lookup"><span data-stu-id="4c564-209">For a much more immediate view of your telemetry, open [Live Stream](app-insights-live-stream.md).</span></span> <span data-ttu-id="4c564-210">Většina metriky trvat několik minut, než se objeví z důvodu proces agregace.</span><span class="sxs-lookup"><span data-stu-id="4c564-210">Most metrics take a few minutes to appear, because of the process of aggregation.</span></span> <span data-ttu-id="4c564-211">Naopak za provozu metriky jsou optimalizované pro s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="4c564-211">By contrast, live metrics are optimized for low latency.</span></span> 

## <a name="set-alerts"></a><span data-ttu-id="4c564-212">Nastavení upozornění</span><span class="sxs-lookup"><span data-stu-id="4c564-212">Set alerts</span></span>
<span data-ttu-id="4c564-213">Mají být informování tímto e-mailu neobvyklou hodnot všechny metriky, přidáte oznámení.</span><span class="sxs-lookup"><span data-stu-id="4c564-213">To be notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="4c564-214">Můžete buď k odeslání e-mailu pro účet správce, nebo na konkrétní e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="4c564-214">You can choose either to send the email to the account administrators, or to specific email addresses.</span></span>

![V Průzkumníku metrik zvolte pravidla výstrah, přidejte výstrah](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

<span data-ttu-id="4c564-216">[Další informace o výstrahách][alerts].</span><span class="sxs-lookup"><span data-stu-id="4c564-216">[Learn more about alerts][alerts].</span></span>


## <a name="continuous-export"></a><span data-ttu-id="4c564-217">Souvislý export</span><span class="sxs-lookup"><span data-stu-id="4c564-217">Continuous Export</span></span>
<span data-ttu-id="4c564-218">Pokud chcete data nepřetržitě exportovali, aby ho mohl zpracovávat externě, zvažte použití [průběžné export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="4c564-218">If you want data continuously exported so that you can process it externally, consider using [Continuous export](app-insights-export-telemetry.md).</span></span>

### <a name="power-bi"></a><span data-ttu-id="4c564-219">Power BI</span><span class="sxs-lookup"><span data-stu-id="4c564-219">Power BI</span></span>
<span data-ttu-id="4c564-220">Pokud chcete, aby i bohatší zobrazení dat, můžete [exportovat do Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span><span class="sxs-lookup"><span data-stu-id="4c564-220">If you want even richer views of your data, you can [export to Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span></span>

## <a name="analytics"></a><span data-ttu-id="4c564-221">Analýza</span><span class="sxs-lookup"><span data-stu-id="4c564-221">Analytics</span></span>
<span data-ttu-id="4c564-222">[Analýza](app-insights-analytics.md) je rozmanitější způsob, jak analyzovat telemetrie pomocí účinný dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="4c564-222">[Analytics](app-insights-analytics.md) is a more versatile way to analyze your telemetry using a powerful query language.</span></span> <span data-ttu-id="4c564-223">Použijte jej, pokud chcete kombinovat výpočetní výsledky z metriky nebo provést podrobné zkoumání poslední výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c564-223">Use it if you want to combine or compute results from metrics, or perform an in-depth exploration of your app's recent performance.</span></span> 

<span data-ttu-id="4c564-224">Z metriky grafu můžete kliknutím na ikonu Analytics získat přímo na ekvivalentní Analytics dotazu.</span><span class="sxs-lookup"><span data-stu-id="4c564-224">From a metric chart, you can click the Analytics icon to get directly to the equivalent Analytics query.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4c564-225">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4c564-225">Troubleshooting</span></span>
<span data-ttu-id="4c564-226">*V grafu se nezobrazí žádná data.*</span><span class="sxs-lookup"><span data-stu-id="4c564-226">*I don't see any data on my chart.*</span></span>

* <span data-ttu-id="4c564-227">Filtry platí pro všechny grafy v okně.</span><span class="sxs-lookup"><span data-stu-id="4c564-227">Filters apply to all the charts on the blade.</span></span> <span data-ttu-id="4c564-228">Ujistěte se, že když se zaměřením na jeden graf, nebyla nastavena filtr, který vylučuje všechna data na jiném.</span><span class="sxs-lookup"><span data-stu-id="4c564-228">Make sure that, while you're focusing on one chart, you didn't set a filter that excludes all the data on another.</span></span>

    <span data-ttu-id="4c564-229">Pokud chcete nastavit různých filtrů na různých grafů, vytvořte je v různých oknech, je uložte jako samostatné Oblíbené položky.</span><span class="sxs-lookup"><span data-stu-id="4c564-229">If you want to set different filters on different charts, create them in different blades, save them as separate favorites.</span></span> <span data-ttu-id="4c564-230">Pokud chcete, můžete je připnout na řídicí panel, abyste je viděli vedle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="4c564-230">If you want, you can pin them to the dashboard so that you can see them alongside each other.</span></span>
* <span data-ttu-id="4c564-231">Graf při seskupení podle vlastnosti, která není definován v metriku, pak nebude nic na graf.</span><span class="sxs-lookup"><span data-stu-id="4c564-231">If you group a chart by a property that is not defined on the metric, then there will be nothing on the chart.</span></span> <span data-ttu-id="4c564-232">Vymažte "Seskupit podle", nebo zvolte jinou seskupení vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4c564-232">Try clearing 'group by', or choose a different grouping property.</span></span>
* <span data-ttu-id="4c564-233">Údaje o výkonu (procesoru, vstupně-výstupní operace rychlost a tak dále) je k dispozici pro webové služby, Java, desktopové aplikace systému Windows, [IIS webové aplikace a služby, pokud nainstalujete monitorování stavu](app-insights-monitor-performance-live-website-now.md), a [Azure Cloud Services](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4c564-233">Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install status monitor](app-insights-monitor-performance-live-website-now.md), and [Azure Cloud Services](app-insights-azure.md).</span></span> <span data-ttu-id="4c564-234">Není k dispozici pro weby Azure.</span><span class="sxs-lookup"><span data-stu-id="4c564-234">It isn't available for Azure websites.</span></span>

## <a name="video"></a><span data-ttu-id="4c564-235">Video</span><span class="sxs-lookup"><span data-stu-id="4c564-235">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="4c564-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c564-236">Next steps</span></span>
* [<span data-ttu-id="4c564-237">Sledování použití s nástrojem Application Insights</span><span class="sxs-lookup"><span data-stu-id="4c564-237">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="4c564-238">Pomocí vyhledávání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="4c564-238">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
