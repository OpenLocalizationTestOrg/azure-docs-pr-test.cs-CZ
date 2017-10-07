---
title: "aaaExploring metriky ve službě Azure Application Insights | Microsoft Docs"
description: "Jak toointerpret grafy na metriky Průzkumníka a jak toocustomize metriky explorer okna."
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
ms.openlocfilehash: b77ae227ae61e800ad6f3af8a05cd123ea1d69e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-metrics-in-application-insights"></a><span data-ttu-id="9b341-103">Zkoumání metriky ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="9b341-103">Exploring Metrics in Application Insights</span></span>
<span data-ttu-id="9b341-104">Metriky v [Application Insights] [ start] jsou měřené hodnoty a počet událostí, které se odesílají v telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b341-104">Metrics in [Application Insights][start] are measured values and counts of events that are sent in telemetry from your application.</span></span> <span data-ttu-id="9b341-105">Pomáhají zjistit problémy s výkonem a sledujte trendy ve využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b341-105">They help you detect performance issues and watch trends in how your application is being used.</span></span> <span data-ttu-id="9b341-106">Je širokou škálu standardní metriky a můžete také vytvořit vlastní vlastní metriky a události.</span><span class="sxs-lookup"><span data-stu-id="9b341-106">There's a wide range of standard metrics, and you can also create your own custom metrics and events.</span></span>

<span data-ttu-id="9b341-107">Počet metriky a události se zobrazí v grafech agregované hodnoty, jako je například součtů a průměry, počty.</span><span class="sxs-lookup"><span data-stu-id="9b341-107">Metrics and event counts are displayed in charts of aggregated values such as sums, averages, or counts.</span></span>

<span data-ttu-id="9b341-108">Zde je ukázka sadu grafy:</span><span class="sxs-lookup"><span data-stu-id="9b341-108">Here's a sample set of charts:</span></span>

![](./media/app-insights-metrics-explorer/01-overview.png)

<span data-ttu-id="9b341-109">Metriky grafy všude, kde zjistíte hello portálu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9b341-109">You find metrics charts everywhere in hello Application Insights portal.</span></span> <span data-ttu-id="9b341-110">Ve většině případů lze přizpůsobit a můžete přidat další okno toohello grafy.</span><span class="sxs-lookup"><span data-stu-id="9b341-110">In most cases, they can be customized, and you can add more charts toohello blade.</span></span> <span data-ttu-id="9b341-111">V okně Přehled hello, klikněte na tlačítko prostřednictvím toomore podrobné grafy (která mají názvy, například "Servery"), nebo klikněte na tlačítko **Průzkumníku metrik** tooopen nové okno, kde můžete vytvořit vlastní grafy.</span><span class="sxs-lookup"><span data-stu-id="9b341-111">From hello Overview blade, click through toomore detailed charts (which have titles such as "Servers"), or click **Metrics Explorer** tooopen a new blade where you can create custom charts.</span></span>

## <a name="time-range"></a><span data-ttu-id="9b341-112">Časové rozmezí</span><span class="sxs-lookup"><span data-stu-id="9b341-112">Time range</span></span>
<span data-ttu-id="9b341-113">Můžete změnit hello časové rozmezí předmětem hello grafy nebo mřížky na libovolné okno.</span><span class="sxs-lookup"><span data-stu-id="9b341-113">You can change hello Time range covered by hello charts or grids on any blade.</span></span>

![Okno Přehled otevřete hello vaší aplikace v hello portálu Azure](./media/app-insights-metrics-explorer/03-range.png)

<span data-ttu-id="9b341-115">Pokud očekáváte data, která ještě nebyla zobrazovaly, klikněte na tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9b341-115">If you're expecting some data that hasn't appeared yet, click Refresh.</span></span> <span data-ttu-id="9b341-116">Grafy sami aktualizace v intervalech, ale jsou hello intervaly delší dobu, větší časových rozsahů.</span><span class="sxs-lookup"><span data-stu-id="9b341-116">Charts refresh themselves at intervals, but hello intervals are longer for larger time ranges.</span></span> <span data-ttu-id="9b341-117">Může trvat nějakou dobu toocome data prostřednictvím kanálu analýzy hello do grafu.</span><span class="sxs-lookup"><span data-stu-id="9b341-117">It can take a while for data toocome through hello analysis pipeline onto a chart.</span></span>

<span data-ttu-id="9b341-118">toozoom do části grafu, přetáhněte nad ním:</span><span class="sxs-lookup"><span data-stu-id="9b341-118">toozoom into part of a chart, drag over it:</span></span>

![Přetažením přes část grafu.](./media/app-insights-metrics-explorer/12-drag.png)

<span data-ttu-id="9b341-120">Klikněte na tlačítko hello vrátit zpět zvětšení tlačítko toorestore ho.</span><span class="sxs-lookup"><span data-stu-id="9b341-120">Click hello Undo Zoom button toorestore it.</span></span>

## <a name="granularity-and-point-values"></a><span data-ttu-id="9b341-121">Hodnoty členitosti a bodu</span><span class="sxs-lookup"><span data-stu-id="9b341-121">Granularity and point values</span></span>
<span data-ttu-id="9b341-122">V tomto bodě umístěte ukazatel myši nad hello grafu toodisplay hello hodnoty metrik hello.</span><span class="sxs-lookup"><span data-stu-id="9b341-122">Hover your mouse over hello chart toodisplay hello values of hello metrics at that point.</span></span>

![Najeďte hello myš graf](./media/app-insights-metrics-explorer/02-focus.png)

<span data-ttu-id="9b341-124">Hodnota Hello hello metrika na určitém místě je agregován v hello předcházející intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="9b341-124">hello value of hello metric at a particular point is aggregated over hello preceding sampling interval.</span></span>

<span data-ttu-id="9b341-125">interval vzorkování Hello nebo "členitosti" se zobrazí v horní části hello hello okna.</span><span class="sxs-lookup"><span data-stu-id="9b341-125">hello sampling interval or "granularity" is shown at hello top of hello blade.</span></span>

![záhlaví Hello okno.](./media/app-insights-metrics-explorer/11-grain.png)

<span data-ttu-id="9b341-127">Můžete upravit hello členitosti v okně rozsah hello čas:</span><span class="sxs-lookup"><span data-stu-id="9b341-127">You can adjust hello granularity in hello Time range blade:</span></span>

![záhlaví Hello okno.](./media/app-insights-metrics-explorer/grain.png)

<span data-ttu-id="9b341-129">k dispozici Hello členitostí závisí na hello časové rozmezí, kterou vyberete.</span><span class="sxs-lookup"><span data-stu-id="9b341-129">hello granularities available depend on hello time range you select.</span></span> <span data-ttu-id="9b341-130">explicitní členitostí Hello jsou alternativy toohello "automatické" členitosti hello časovém rozmezí.</span><span class="sxs-lookup"><span data-stu-id="9b341-130">hello explicit granularities are alternatives toohello "automatic" granularity for hello time range.</span></span>


## <a name="editing-charts-and-grids"></a><span data-ttu-id="9b341-131">Úpravy grafy a mřížky</span><span class="sxs-lookup"><span data-stu-id="9b341-131">Editing charts and grids</span></span>
<span data-ttu-id="9b341-132">tooadd nové okno toohello grafu:</span><span class="sxs-lookup"><span data-stu-id="9b341-132">tooadd a new chart toohello blade:</span></span>

![V Průzkumníku metrik zvolte přidat graf](./media/app-insights-metrics-explorer/04-add.png)

<span data-ttu-id="9b341-134">Vyberte **upravit** na graf tooedit existující nebo nové co zobrazí:</span><span class="sxs-lookup"><span data-stu-id="9b341-134">Select **Edit** on an existing or new chart tooedit what it shows:</span></span>

![Vyberte jeden nebo více metriky](./media/app-insights-metrics-explorer/08-select.png)

<span data-ttu-id="9b341-136">Více než jeden metrika můžete zobrazit v grafu, i když existují omezení o hello kombinace, které lze zobrazit najednou.</span><span class="sxs-lookup"><span data-stu-id="9b341-136">You can display more than one metric on a chart, though there are restrictions about hello combinations that can be displayed together.</span></span> <span data-ttu-id="9b341-137">Jakmile vyberete jeden metriku, některé hello jiné jsou zakázány.</span><span class="sxs-lookup"><span data-stu-id="9b341-137">As soon as you choose one metric, some of hello others are disabled.</span></span>

<span data-ttu-id="9b341-138">Pokud jste programového [vlastní metriky] [ track] do vaší aplikace (volání tooTrackMetric a TrackEvent) budou uvedené v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="9b341-138">If you coded [custom metrics][track] into your app (calls tooTrackMetric and TrackEvent) they will be listed here.</span></span>

## <a name="segment-your-data"></a><span data-ttu-id="9b341-139">Segmentovat vaše data</span><span class="sxs-lookup"><span data-stu-id="9b341-139">Segment your data</span></span>
<span data-ttu-id="9b341-140">Metriky lze rozdělit vlastnost – například toocompare stránky zobrazení na klientských počítačích s jinými operačními systémy.</span><span class="sxs-lookup"><span data-stu-id="9b341-140">You can split a metric by property - for example, toocompare page views on clients with different operating systems.</span></span>

<span data-ttu-id="9b341-141">Vyberte graf nebo tabulku, přepínač na seskupení a vyberte vlastnost toogroup podle:</span><span class="sxs-lookup"><span data-stu-id="9b341-141">Select a chart or grid, switch on grouping and pick a property toogroup by:</span></span>

![Vyberte seskupení na a pak sadu vyberte vlastnost v Seskupit podle](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> <span data-ttu-id="9b341-143">Pokud používáte seskupení, oblasti a pruhový graf typy hello poskytují skládaný zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9b341-143">When you use grouping, hello Area and Bar chart types provide a stacked display.</span></span> <span data-ttu-id="9b341-144">Toto je vhodný, kde je metoda hello agregace Sum.</span><span class="sxs-lookup"><span data-stu-id="9b341-144">This is suitable where hello Aggregation method is Sum.</span></span> <span data-ttu-id="9b341-145">Ale hello typ agregace jsou průměr, zvolte Zobrazení typů hello řádek nebo mřížky.</span><span class="sxs-lookup"><span data-stu-id="9b341-145">But where hello aggregation type is Average, choose hello Line or Grid display types.</span></span>
>
>

<span data-ttu-id="9b341-146">Pokud jste programového [vlastní metriky] [ track] do vaší aplikace a obsahují hodnoty vlastností, budete moct tooselect hello vlastnost v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="9b341-146">If you coded [custom metrics][track] into your app and they include property values, you'll be able tooselect hello property in hello list.</span></span>

<span data-ttu-id="9b341-147">Graf hello je příliš malá pro segmentovaným dat?</span><span class="sxs-lookup"><span data-stu-id="9b341-147">Is hello chart too small for segmented data?</span></span> <span data-ttu-id="9b341-148">Nastavte jeho výšku:</span><span class="sxs-lookup"><span data-stu-id="9b341-148">Adjust its height:</span></span>

![Upravit hello posuvníku](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a><span data-ttu-id="9b341-150">Typy agregace</span><span class="sxs-lookup"><span data-stu-id="9b341-150">Aggregation types</span></span>
<span data-ttu-id="9b341-151">Hello legendy na stranu hello ve výchozím nastavení je obvykle zobrazena hodnota hello agregovat období hello hello grafu.</span><span class="sxs-lookup"><span data-stu-id="9b341-151">hello legend at hello side by default usually shows hello aggregated value over hello period of hello chart.</span></span> <span data-ttu-id="9b341-152">Pokud je ukazatel myši nad hello grafu, zobrazí hello hodnotu v tomto bodě.</span><span class="sxs-lookup"><span data-stu-id="9b341-152">If you hover over hello chart, it shows hello value at that point.</span></span>

<span data-ttu-id="9b341-153">Každý datový bod v grafu hello je agregace hodnot hello dat přijatých v předchozím vzorkování interval nebo "členitosti" hello.</span><span class="sxs-lookup"><span data-stu-id="9b341-153">Each data point on hello chart is an aggregate of hello data values received in hello preceding sampling interval or "granularity".</span></span> <span data-ttu-id="9b341-154">členitost Hello se zobrazí v horní části hello hello okna a se liší podle hello celkový časový rámec hello grafu.</span><span class="sxs-lookup"><span data-stu-id="9b341-154">hello granularity is shown at hello top of hello blade, and varies with hello overall timescale of hello chart.</span></span>

<span data-ttu-id="9b341-155">Metriky lze agregovat různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="9b341-155">Metrics can be aggregated in different ways:</span></span>

* <span data-ttu-id="9b341-156">**Počet** je počet událostí hello přijatých hello intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="9b341-156">**Count** is a count of hello events received in hello sampling interval.</span></span> <span data-ttu-id="9b341-157">Používá se pro události, jako jsou žádosti o.</span><span class="sxs-lookup"><span data-stu-id="9b341-157">It is used for events such as requests.</span></span> <span data-ttu-id="9b341-158">Rozdíly v hello výška grafu hello označuje rozdíly v hello rychlost, jakou hello k událostem.</span><span class="sxs-lookup"><span data-stu-id="9b341-158">Variations in hello height of hello chart indicates variations in hello rate at which hello events occur.</span></span> <span data-ttu-id="9b341-159">Ale Všimněte si, že hello číselná hodnota se změní při změně hello intervalu vzorkování.</span><span class="sxs-lookup"><span data-stu-id="9b341-159">But note that hello numeric value changes when you change hello sampling interval.</span></span>
* <span data-ttu-id="9b341-160">**Součet** přidá hello hodnot všech datových bodů hello přijatých prostřednictvím interval vzorkování hello nebo období hello hello grafu.</span><span class="sxs-lookup"><span data-stu-id="9b341-160">**Sum** adds up hello values of all hello data points received over hello sampling interval, or hello period of hello chart.</span></span>
* <span data-ttu-id="9b341-161">**Průměrná** vydělí hello součet hello počtem přijatých prostřednictvím hello interval datových bodů.</span><span class="sxs-lookup"><span data-stu-id="9b341-161">**Average** divides hello Sum by hello number of data points received over hello interval.</span></span>
* <span data-ttu-id="9b341-162">**Jedinečné** počty se používají pro počty uživatelů a účty.</span><span class="sxs-lookup"><span data-stu-id="9b341-162">**Unique** counts are used for counts of users and accounts.</span></span> <span data-ttu-id="9b341-163">Během intervalu vzorkování hello nebo období hello hello grafu hello obrázku hello počet různých uživatelé vidět v té době.</span><span class="sxs-lookup"><span data-stu-id="9b341-163">Over hello sampling interval, or over hello period of hello chart, hello figure shows hello count of different users seen in that time.</span></span>
* <span data-ttu-id="9b341-164">**%**-Procento verze každé agregace se používají pouze s segmentovaným grafy.</span><span class="sxs-lookup"><span data-stu-id="9b341-164">**%** - percentage versions of each aggregation are used only with segmented charts.</span></span> <span data-ttu-id="9b341-165">Celkový počet Hello vždy přidá too100 % a hello graf zobrazuje hello relativní příspěvek různé komponenty celkem.</span><span class="sxs-lookup"><span data-stu-id="9b341-165">hello total always adds up too100%, and hello chart shows hello relative contribution of different components of a total.</span></span>

    ![Procento agregace](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a><span data-ttu-id="9b341-167">Změnit typ agregace hello</span><span class="sxs-lookup"><span data-stu-id="9b341-167">Change hello aggregation type</span></span>

![Upravit hello graf a pak vyberte agregace](./media/app-insights-metrics-explorer/05-aggregation.png)

<span data-ttu-id="9b341-169">Hello výchozí metodou pro jednotlivé metriky se zobrazí, když vytvoříte nový graf nebo když vypnete všechny metriky:</span><span class="sxs-lookup"><span data-stu-id="9b341-169">hello default method for each metric is shown when you create a new chart or when all metrics are deselected:</span></span>

![Zrušte výběr všechny metriky toosee hello výchozí hodnoty](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a><span data-ttu-id="9b341-171">Osy y PIN kódu</span><span class="sxs-lookup"><span data-stu-id="9b341-171">Pin Y-axis</span></span> 
<span data-ttu-id="9b341-172">Ve výchozím nastavení graf znázorňuje hodnoty osy Y od nuly do maximální hodnoty v oblasti dat hello se toogive vizuální reprezentace quantum hello hodnot.</span><span class="sxs-lookup"><span data-stu-id="9b341-172">By default a chart shows Y axis values starting from zero till maximum values in hello data range, toogive a visual representation of quantum of hello values.</span></span> <span data-ttu-id="9b341-173">Ale v některých případech víc než hello quantum, může to být zajímavé toovisually zkontrolovat malých změn v hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9b341-173">But in some cases more than hello quantum it might be interesting toovisually inspect minor changes in values.</span></span> <span data-ttu-id="9b341-174">Pro přizpůsobení tuto použijte hello osy y úpravy funkce toopin hello osy y minimální nebo maximální hodnotu rozsahu na požadovanou místě.</span><span class="sxs-lookup"><span data-stu-id="9b341-174">For customizations like this use hello Y-axis range editing feature toopin hello Y-axis minimum or maximum value at desired place.</span></span>
<span data-ttu-id="9b341-175">Klikněte na políčko toobring "Upřesnit nastavení" nahoru hello osy y rozsah nastavení</span><span class="sxs-lookup"><span data-stu-id="9b341-175">Click on "Advanced Settings" check box toobring up hello Y-axis range Settings</span></span>

![Klikněte na tlačítko Upřesnit nastavení, vyberte vlastní rozsah a zadejte maximální hodnoty min.](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a><span data-ttu-id="9b341-177">Filtrování dat</span><span class="sxs-lookup"><span data-stu-id="9b341-177">Filter your data</span></span>
<span data-ttu-id="9b341-178">toosee jenom hello metriky pro vybranou sadu hodnot vlastností:</span><span class="sxs-lookup"><span data-stu-id="9b341-178">toosee just hello metrics for a selected set of property values:</span></span>

![Klikněte na tlačítko filtru, rozbalte vlastnost a zkontrolujte některé hodnoty](./media/app-insights-metrics-explorer/19-filter.png)

<span data-ttu-id="9b341-180">Pokud nevyberete žádné hodnoty pro určité vlastnosti, ho má stejné hello jako jejich všechny výběrem: pro tuto vlastnost neexistuje žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="9b341-180">If you don't select any values for a particular property, it's hello same as selecting them all: there is no filter on that property.</span></span>

<span data-ttu-id="9b341-181">Všimněte si hello počty událostí společně se všechny hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="9b341-181">Notice hello counts of events alongside each property value.</span></span> <span data-ttu-id="9b341-182">Když vyberete hodnoty jednu vlastnost, hello počty spolu s další vlastnosti, které hodnoty se upraví.</span><span class="sxs-lookup"><span data-stu-id="9b341-182">When you select values of one property, hello counts alongside other property values are adjusted.</span></span>

<span data-ttu-id="9b341-183">Filtry se používají tooall hello grafy v okně.</span><span class="sxs-lookup"><span data-stu-id="9b341-183">Filters apply tooall hello charts on a blade.</span></span> <span data-ttu-id="9b341-184">Pokud chcete použít jiné filtry toodifferent grafy, vytvořte a uložte okna jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="9b341-184">If you want different filters applied toodifferent charts, create and save different metrics blades.</span></span> <span data-ttu-id="9b341-185">Pokud chcete, můžete připnout grafy z různých oknech toohello řídicího panelu, tak, aby si mohli prohlédnout vedle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="9b341-185">If you want, you can pin charts from different blades toohello dashboard, so that you can see them alongside each other.</span></span>

### <a name="remove-bot-and-web-test-traffic"></a><span data-ttu-id="9b341-186">Odebrat robota a webové přenosy testu</span><span class="sxs-lookup"><span data-stu-id="9b341-186">Remove bot and web test traffic</span></span>
<span data-ttu-id="9b341-187">Použití filtru hello **skutečné nebo syntetické přenosů** a zkontrolujte **skutečné**.</span><span class="sxs-lookup"><span data-stu-id="9b341-187">Use hello filter **Real or synthetic traffic** and check **Real**.</span></span>

<span data-ttu-id="9b341-188">Můžete také filtrovat podle **zdroj syntetické provoz**.</span><span class="sxs-lookup"><span data-stu-id="9b341-188">You can also filter by **Source of synthetic traffic**.</span></span>

### <a name="tooadd-properties-toohello-filter-list"></a><span data-ttu-id="9b341-189">seznam filtrů toohello tooadd vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9b341-189">tooadd properties toohello filter list</span></span>
<span data-ttu-id="9b341-190">Chcete, aby toofilter telemetrii na vlastního výběru kategorie?</span><span class="sxs-lookup"><span data-stu-id="9b341-190">Would you like toofilter telemetry on a category of your own choosing?</span></span> <span data-ttu-id="9b341-191">Například možná zdola nahoru vaši uživatelé do různých kategorií a chcete segmentovat vaše data podle těchto kategorií.</span><span class="sxs-lookup"><span data-stu-id="9b341-191">For example, maybe you divide up your users into different categories, and you would like segment your data by these categories.</span></span>

<span data-ttu-id="9b341-192">[Vytvořit vlastní vlastnost](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="9b341-192">[Create your own property](app-insights-api-custom-events-metrics.md#properties).</span></span> <span data-ttu-id="9b341-193">Nastavit [Telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) toohave se objevit ve všech telemetrie – včetně standardní telemetrie hello poslal jiné moduly SDK.</span><span class="sxs-lookup"><span data-stu-id="9b341-193">Set it in a [Telemetry Initializer](app-insights-api-custom-events-metrics.md#defaults) toohave it appear in all telemetry - including hello standard telemetry sent by different SDK modules.</span></span>

## <a name="edit-hello-chart-type"></a><span data-ttu-id="9b341-194">Upravit typ grafu hello</span><span class="sxs-lookup"><span data-stu-id="9b341-194">Edit hello chart type</span></span>
<span data-ttu-id="9b341-195">Všimněte si, že můžete přepínat mezi mřížky a grafy:</span><span class="sxs-lookup"><span data-stu-id="9b341-195">Notice that you can switch between grids and graphs:</span></span>

![Vyberte graf nebo mřížky, a potom vyberte typ grafu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a><span data-ttu-id="9b341-197">Uložit okně vaší metriky</span><span class="sxs-lookup"><span data-stu-id="9b341-197">Save your metrics blade</span></span>
<span data-ttu-id="9b341-198">Pokud jste vytvořili některé grafy, je uložte jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="9b341-198">When you've created some charts, save them as a favorite.</span></span> <span data-ttu-id="9b341-199">Můžete vybrat, zda tooshare ho s jinými členové týmu, pokud používáte účet organizace.</span><span class="sxs-lookup"><span data-stu-id="9b341-199">You can choose whether tooshare it with other team members, if you use an organizational account.</span></span>

![Vyberte Oblíbené položky.](./media/app-insights-metrics-explorer/21-favorite-save.png)

<span data-ttu-id="9b341-201">toosee hello znovu okno **okno Přehled přejděte toohello** a otevřete oblíbených položek:</span><span class="sxs-lookup"><span data-stu-id="9b341-201">toosee hello blade again, **go toohello overview blade** and open Favorites:</span></span>

![V okně přehledu hello vyberte Oblíbené položky](./media/app-insights-metrics-explorer/22-favorite-get.png)

<span data-ttu-id="9b341-203">Pokud jste zvolili relativní časové rozmezí, když jste uložili, hello okno bude aktualizována hello nejnovější metriky.</span><span class="sxs-lookup"><span data-stu-id="9b341-203">If you chose Relative time range when you saved, hello blade will be updated with hello latest metrics.</span></span> <span data-ttu-id="9b341-204">Pokud jste zvolili absolutní časové rozmezí, se zobrazí pokaždé, když text hello stejná data.</span><span class="sxs-lookup"><span data-stu-id="9b341-204">If you chose Absolute time range, it will show hello same data every time.</span></span>

## <a name="reset-hello-blade"></a><span data-ttu-id="9b341-205">Resetování hello okno</span><span class="sxs-lookup"><span data-stu-id="9b341-205">Reset hello blade</span></span>
<span data-ttu-id="9b341-206">Pokud upravíte okno, ale pak byste chtěli tooget původní back toohello uložit sadu, stačí kliknout na resetovat.</span><span class="sxs-lookup"><span data-stu-id="9b341-206">If you edit a blade but then you'd like tooget back toohello original saved set, just click Reset.</span></span>

![V hello tlačítka v horní části hello metrika Exploreru](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a><span data-ttu-id="9b341-208">Datový proud za provozu metriky</span><span class="sxs-lookup"><span data-stu-id="9b341-208">Live metrics stream</span></span>

<span data-ttu-id="9b341-209">Zobrazení telemetrie a víc okamžitou otevřete [živý datový proud](app-insights-live-stream.md).</span><span class="sxs-lookup"><span data-stu-id="9b341-209">For a much more immediate view of your telemetry, open [Live Stream](app-insights-live-stream.md).</span></span> <span data-ttu-id="9b341-210">Většina metriky trvat několik minut tooappear kvůli hello proces agregace.</span><span class="sxs-lookup"><span data-stu-id="9b341-210">Most metrics take a few minutes tooappear, because of hello process of aggregation.</span></span> <span data-ttu-id="9b341-211">Naopak za provozu metriky jsou optimalizované pro s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="9b341-211">By contrast, live metrics are optimized for low latency.</span></span> 

## <a name="set-alerts"></a><span data-ttu-id="9b341-212">Nastavení upozornění</span><span class="sxs-lookup"><span data-stu-id="9b341-212">Set alerts</span></span>
<span data-ttu-id="9b341-213">toobe oznámení e-mailem neobvyklou hodnot všechny metriky přidat oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b341-213">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="9b341-214">Můžete buď toosend hello e-mailu toohello účet správce, nebo toospecific e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="9b341-214">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![V Průzkumníku metrik zvolte pravidla výstrah, přidejte výstrah](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

<span data-ttu-id="9b341-216">[Další informace o výstrahách][alerts].</span><span class="sxs-lookup"><span data-stu-id="9b341-216">[Learn more about alerts][alerts].</span></span>


## <a name="continuous-export"></a><span data-ttu-id="9b341-217">Souvislý export</span><span class="sxs-lookup"><span data-stu-id="9b341-217">Continuous Export</span></span>
<span data-ttu-id="9b341-218">Pokud chcete data nepřetržitě exportovali, aby ho mohl zpracovávat externě, zvažte použití [průběžné export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="9b341-218">If you want data continuously exported so that you can process it externally, consider using [Continuous export](app-insights-export-telemetry.md).</span></span>

### <a name="power-bi"></a><span data-ttu-id="9b341-219">Power BI</span><span class="sxs-lookup"><span data-stu-id="9b341-219">Power BI</span></span>
<span data-ttu-id="9b341-220">Pokud chcete, aby i bohatší zobrazení dat, můžete [exportovat tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b341-220">If you want even richer views of your data, you can [export tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span></span>

## <a name="analytics"></a><span data-ttu-id="9b341-221">Analýza</span><span class="sxs-lookup"><span data-stu-id="9b341-221">Analytics</span></span>
<span data-ttu-id="9b341-222">[Analýza](app-insights-analytics.md) je rozmanitější tooanalyze způsob telemetrie pomocí účinný dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="9b341-222">[Analytics](app-insights-analytics.md) is a more versatile way tooanalyze your telemetry using a powerful query language.</span></span> <span data-ttu-id="9b341-223">Použijte jej, pokud chcete toocombine nebo výpočetní výsledky z metriky nebo provést podrobné zkoumání poslední výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b341-223">Use it if you want toocombine or compute results from metrics, or perform an in-depth exploration of your app's recent performance.</span></span> 

<span data-ttu-id="9b341-224">Z metriky grafu, můžete kliknout na hello Analytics ikonu tooget přímo toohello ekvivalentní Analytics dotazu.</span><span class="sxs-lookup"><span data-stu-id="9b341-224">From a metric chart, you can click hello Analytics icon tooget directly toohello equivalent Analytics query.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9b341-225">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9b341-225">Troubleshooting</span></span>
<span data-ttu-id="9b341-226">*V grafu se nezobrazí žádná data.*</span><span class="sxs-lookup"><span data-stu-id="9b341-226">*I don't see any data on my chart.*</span></span>

* <span data-ttu-id="9b341-227">Filtry se používají tooall hello grafy v okně hello.</span><span class="sxs-lookup"><span data-stu-id="9b341-227">Filters apply tooall hello charts on hello blade.</span></span> <span data-ttu-id="9b341-228">Ujistěte se, že když se zaměřením na jeden graf, nebyla nastavena filtr, který vylučuje všechna data hello na jiném.</span><span class="sxs-lookup"><span data-stu-id="9b341-228">Make sure that, while you're focusing on one chart, you didn't set a filter that excludes all hello data on another.</span></span>

    <span data-ttu-id="9b341-229">Pokud chcete tooset různých filtrů na různých grafů, je vytvořte v různých oknech, je uložíte jako samostatné Oblíbené položky.</span><span class="sxs-lookup"><span data-stu-id="9b341-229">If you want tooset different filters on different charts, create them in different blades, save them as separate favorites.</span></span> <span data-ttu-id="9b341-230">Pokud chcete, můžete připnout je toohello řídicí panel, abyste je viděli vedle sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="9b341-230">If you want, you can pin them toohello dashboard so that you can see them alongside each other.</span></span>
* <span data-ttu-id="9b341-231">Graf při seskupení podle vlastnosti, která není definován na hello metrika, pak bude nic na hello grafu.</span><span class="sxs-lookup"><span data-stu-id="9b341-231">If you group a chart by a property that is not defined on hello metric, then there will be nothing on hello chart.</span></span> <span data-ttu-id="9b341-232">Vymažte "Seskupit podle", nebo zvolte jinou seskupení vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9b341-232">Try clearing 'group by', or choose a different grouping property.</span></span>
* <span data-ttu-id="9b341-233">Údaje o výkonu (procesoru, vstupně-výstupní operace rychlost a tak dále) je k dispozici pro webové služby, Java, desktopové aplikace systému Windows, [IIS webové aplikace a služby, pokud nainstalujete monitorování stavu](app-insights-monitor-performance-live-website-now.md), a [Azure Cloud Services](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9b341-233">Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install status monitor](app-insights-monitor-performance-live-website-now.md), and [Azure Cloud Services](app-insights-azure.md).</span></span> <span data-ttu-id="9b341-234">Není k dispozici pro weby Azure.</span><span class="sxs-lookup"><span data-stu-id="9b341-234">It isn't available for Azure websites.</span></span>

## <a name="video"></a><span data-ttu-id="9b341-235">Video</span><span class="sxs-lookup"><span data-stu-id="9b341-235">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="9b341-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b341-236">Next steps</span></span>
* [<span data-ttu-id="9b341-237">Sledování použití s nástrojem Application Insights</span><span class="sxs-lookup"><span data-stu-id="9b341-237">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="9b341-238">Pomocí vyhledávání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="9b341-238">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
