---
title: "aaaUsing Analytics - hello vyhledávání výkonné nástroje Azure Application Insights | Microsoft Docs"
description: "Pomocí hello analýzy, hello výkonné diagnostické vyhledávání nástroje Application Insights. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a><span data-ttu-id="a0394-103">Pomocí analýzy ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="a0394-103">Using Analytics in Application Insights</span></span>
<span data-ttu-id="a0394-104">[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a0394-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="a0394-105">Tyto stránek popisují dotazovací jazyk analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="a0394-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="a0394-106">**[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="a0394-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="a0394-107">**[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="a0394-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>

## <a name="open-analytics"></a><span data-ttu-id="a0394-108">Otevřete Analytics</span><span class="sxs-lookup"><span data-stu-id="a0394-108">Open Analytics</span></span>
<span data-ttu-id="a0394-109">Z domovské prostředek vaší aplikace ve službě Application Insights klikněte na tlačítko Analytics.</span><span class="sxs-lookup"><span data-stu-id="a0394-109">From your app's home resource in Application Insights, click Analytics.</span></span>

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics-using/001.png)

<span data-ttu-id="a0394-111">Hello vložené kurzu vám dává některé nápady, o co můžete dělat.</span><span class="sxs-lookup"><span data-stu-id="a0394-111">hello inline tutorial gives you some ideas about what you can do.</span></span>

<span data-ttu-id="a0394-112">Je [rozsáhlejší prohlídka zde](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="a0394-112">There's a [more extensive tour here](app-insights-analytics-tour.md).</span></span>

## <a name="query-your-telemetry"></a><span data-ttu-id="a0394-113">Dotaz telemetrie</span><span class="sxs-lookup"><span data-stu-id="a0394-113">Query your telemetry</span></span>
### <a name="write-a-query"></a><span data-ttu-id="a0394-114">Napsat dotaz</span><span class="sxs-lookup"><span data-stu-id="a0394-114">Write a query</span></span>
![Zobrazení schématu](./media/app-insights-analytics-using/150.png)

<span data-ttu-id="a0394-116">Začněte s hello názvy všech hello tabulky na levé straně hello (nebo hello [rozsah](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) nebo [sjednocení](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operátory).</span><span class="sxs-lookup"><span data-stu-id="a0394-116">Begin with hello names of any of hello tables listed on hello left (or hello [range](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) or [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operators).</span></span> <span data-ttu-id="a0394-117">Použití `|` toocreate a kanál z [operátory](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span><span class="sxs-lookup"><span data-stu-id="a0394-117">Use `|` toocreate a pipeline of [operators](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span></span> 

<span data-ttu-id="a0394-118">IntelliSense zobrazí výzvu s hello operátory a hello výraz prvky, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="a0394-118">IntelliSense prompts you with hello operators and hello expression elements that you can use.</span></span> <span data-ttu-id="a0394-119">Klikněte na ikonu informací o hello (nebo stiskněte klávesu CTRL + MEZERNÍK) tooget delší popis a příklady, jak toouse každý prvek.</span><span class="sxs-lookup"><span data-stu-id="a0394-119">Click hello information icon (or press CTRL+Space) tooget a longer description and examples of how toouse each element.</span></span>

<span data-ttu-id="a0394-120">V tématu hello [Analytics jazyk prohlídka](app-insights-analytics-tour.md) a [referenční příručka jazyka](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="a0394-120">See hello [Analytics language tour](app-insights-analytics-tour.md) and [language reference](app-insights-analytics-reference.md).</span></span>

### <a name="run-a-query"></a><span data-ttu-id="a0394-121">Spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="a0394-121">Run a query</span></span>
![Spuštění dotazu](./media/app-insights-analytics-using/130.png)

1. <span data-ttu-id="a0394-123">Jeden řádek zalomení můžete použít v dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-123">You can use single line breaks in a query.</span></span>
2. <span data-ttu-id="a0394-124">Umístěte kurzor hello uvnitř nebo na konci hello hello dotazu, že který má toorun.</span><span class="sxs-lookup"><span data-stu-id="a0394-124">Put hello cursor inside or at hello end of hello query you want toorun.</span></span>
3. <span data-ttu-id="a0394-125">Zkontrolujte časové rozmezí hello tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-125">Check hello time range of your query.</span></span> <span data-ttu-id="a0394-126">(Můžete ho změnit nebo potlačit včetně vlastní [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) klauzule v dotazu.)</span><span class="sxs-lookup"><span data-stu-id="a0394-126">(You can change it, or override it by including your own [`where...timestamp...`](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) clause in your query.)</span></span>
3. <span data-ttu-id="a0394-127">Klikněte na dotaz hello toorun přejděte.</span><span class="sxs-lookup"><span data-stu-id="a0394-127">Click Go toorun hello query.</span></span>
4. <span data-ttu-id="a0394-128">Nevkládejte prázdné řádky v dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-128">Don't put blank lines in your query.</span></span> <span data-ttu-id="a0394-129">Několik oddělených dotazů můžete ponechat v jedné karty dotazu jejich oddělením prázdné řádky.</span><span class="sxs-lookup"><span data-stu-id="a0394-129">You can keep several separated queries in one query tab by separating them with blank lines.</span></span> <span data-ttu-id="a0394-130">Spuštění pouze hello dotazu, který má hello kurzoru.</span><span class="sxs-lookup"><span data-stu-id="a0394-130">Only hello query that has hello cursor runs.</span></span>

### <a name="save-a-query"></a><span data-ttu-id="a0394-131">Uložení dotazu</span><span class="sxs-lookup"><span data-stu-id="a0394-131">Save a query</span></span>
![Uložení dotazu](./media/app-insights-analytics-using/140.png)

1. <span data-ttu-id="a0394-133">Uložte aktuální soubor dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-133">Save hello current query file.</span></span>
2. <span data-ttu-id="a0394-134">Otevřete soubor uložený dotaz.</span><span class="sxs-lookup"><span data-stu-id="a0394-134">Open a saved query file.</span></span>
3. <span data-ttu-id="a0394-135">Vytvořte nový soubor dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-135">Create a new query file.</span></span>

## <a name="see-hello-details"></a><span data-ttu-id="a0394-136">Zobrazit podrobnosti hello</span><span class="sxs-lookup"><span data-stu-id="a0394-136">See hello details</span></span>
<span data-ttu-id="a0394-137">Rozbalte všechny řádek hello výsledky toosee jeho úplný seznam vlastností.</span><span class="sxs-lookup"><span data-stu-id="a0394-137">Expand any row in hello results toosee its complete list of properties.</span></span> <span data-ttu-id="a0394-138">Můžete dále rozšířit všechny vlastnosti, která je strukturovaných hodnota – například, vlastní dimenze nebo hello zásobníku výpis výjimku.</span><span class="sxs-lookup"><span data-stu-id="a0394-138">You can further expand any property that is a structured value - for example, custom dimensions, or hello stack listing in an exception.</span></span>

![Rozbalte řádek](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a><span data-ttu-id="a0394-140">Uspořádat výsledky hello</span><span class="sxs-lookup"><span data-stu-id="a0394-140">Arrange hello results</span></span>
<span data-ttu-id="a0394-141">Můžete řadit, filtrovat, stránkování a seskupení hello výsledků vrácená z dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-141">You can sort, filter, paginate, and group hello results returned from your query.</span></span>

> [!NOTE]
> <span data-ttu-id="a0394-142">Řazení, seskupování a filtrování v prohlížeči hello nemáte spusťte znovu dotaz.</span><span class="sxs-lookup"><span data-stu-id="a0394-142">Sorting, grouping, and filtering in hello browser don't re-run your query.</span></span> <span data-ttu-id="a0394-143">Jejich pouze změna uspořádání hello výsledky, které byly vráceny poslední dotaz.</span><span class="sxs-lookup"><span data-stu-id="a0394-143">They only rearrange hello results that were returned by your last query.</span></span> 
> 
> <span data-ttu-id="a0394-144">tooperform tyto úlohy v hello serveru předtím, než budou vráceny výsledky hello, napsat dotaz s hello [řazení](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [shrnout](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) a [kde](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operátory.</span><span class="sxs-lookup"><span data-stu-id="a0394-144">tooperform these tasks in hello server before hello results are returned, write your query with hello [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) and [where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operators.</span></span>
> 
> 

<span data-ttu-id="a0394-145">Vyberte sloupce hello byste jako toosee, přetáhněte toorearrange záhlaví sloupce a změny velikosti sloupce přetažením jejich ohraničení.</span><span class="sxs-lookup"><span data-stu-id="a0394-145">Pick hello columns you'd like toosee, drag column headers toorearrange them, and resize columns by dragging their borders.</span></span>

![Uspořádání sloupců](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a><span data-ttu-id="a0394-147">Třídění a filtrování položek</span><span class="sxs-lookup"><span data-stu-id="a0394-147">Sort and filter items</span></span>
<span data-ttu-id="a0394-148">Seřaďte výsledky klepnutím hello head sloupce.</span><span class="sxs-lookup"><span data-stu-id="a0394-148">Sort your results by clicking hello head of a column.</span></span> <span data-ttu-id="a0394-149">Klikněte na tlačítko znovu toosort hello jiným způsobem, a klikněte na třetí čas toorevert toohello původní pořadí vrácených v dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-149">Click again toosort hello other way, and click a third time toorevert toohello original ordering returned by your query.</span></span>

<span data-ttu-id="a0394-150">Použití hello filtrovat toonarrow ikonu hledání.</span><span class="sxs-lookup"><span data-stu-id="a0394-150">Use hello filter icon toonarrow your search.</span></span>

![Sloupce řadit a filtrovat.](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a><span data-ttu-id="a0394-152">Seskupení položek</span><span class="sxs-lookup"><span data-stu-id="a0394-152">Group items</span></span>
<span data-ttu-id="a0394-153">toosort pomocí seskupení více než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="a0394-153">toosort by more than one column, use grouping.</span></span> <span data-ttu-id="a0394-154">Nejprve povolte a poté přetáhněte záhlaví sloupců do prostoru hello nad hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="a0394-154">First enable it, and then drag column headers into hello space above hello table.</span></span>

![Skupina](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a><span data-ttu-id="a0394-156">Chybí některé výsledky?</span><span class="sxs-lookup"><span data-stu-id="a0394-156">Missing some results?</span></span>

<span data-ttu-id="a0394-157">Pokud se domníváte, že nevidíte všechny výsledky hello vašim očekáváním, existuje několik možných příčin.</span><span class="sxs-lookup"><span data-stu-id="a0394-157">If you think you're not seeing all hello results you expected, there are a couple of possible reasons.</span></span>

* <span data-ttu-id="a0394-158">**Filtr času rozsah**.</span><span class="sxs-lookup"><span data-stu-id="a0394-158">**Time range filter**.</span></span> <span data-ttu-id="a0394-159">Ve výchozím nastavení zobrazí, jenom výsledky z hello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="a0394-159">By default, you will only see results from hello last 24 hours.</span></span> <span data-ttu-id="a0394-160">Není automatické filtr, který omezuje hello rozsah výsledky, které jsou načteny z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="a0394-160">There is an automatic filter that limits hello range of results that are retrieved from hello source tables.</span></span> 

    <span data-ttu-id="a0394-161">Můžete však změnit časový rozsah hello filtr pomocí hello rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="a0394-161">However, you can change hello time range filter by using hello drop-down menu.</span></span>

    <span data-ttu-id="a0394-162">Nebo můžete přepsat hello automatického rozsahu včetně vlastní [ `where  ... timestamp ...` klauzule](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) do dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0394-162">Or you can override hello automatic range by including your own [`where  ... timestamp ...` clause](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) into your query.</span></span> <span data-ttu-id="a0394-163">Například:</span><span class="sxs-lookup"><span data-stu-id="a0394-163">For example:</span></span>

    `requests | where timestamp > ago('2d')`

* <span data-ttu-id="a0394-164">**Limit výsledků**.</span><span class="sxs-lookup"><span data-stu-id="a0394-164">**Results limit**.</span></span> <span data-ttu-id="a0394-165">Existuje limit asi 10 kB řádků na hello výsledky vrácené z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-165">There's a limit of about 10k rows on hello results returned from hello portal.</span></span> <span data-ttu-id="a0394-166">Upozornění zobrazí, pokud přejdete přesahuje hello limit.</span><span class="sxs-lookup"><span data-stu-id="a0394-166">A warning shows if you go over hello limit.</span></span> <span data-ttu-id="a0394-167">Pokud k tomu dojde, řazení výsledky v tabulce hello vždy nezobrazí se vám všechny hello skutečné první nebo poslední výsledky.</span><span class="sxs-lookup"><span data-stu-id="a0394-167">If that happens, sorting your results in hello table won't always show you all hello actual first or last results.</span></span> 

    <span data-ttu-id="a0394-168">Je dobrým zvykem tooavoid stiskne hello limit.</span><span class="sxs-lookup"><span data-stu-id="a0394-168">It's good practice tooavoid hitting hello limit.</span></span> <span data-ttu-id="a0394-169">Použijte hello časový rozsah filtr, nebo použijte například operátory:</span><span class="sxs-lookup"><span data-stu-id="a0394-169">Use hello time range filter, or use operators such as:</span></span>

  * [<span data-ttu-id="a0394-170">prvních 100 pomocí časového razítka</span><span class="sxs-lookup"><span data-stu-id="a0394-170">top 100 by timestamp</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [<span data-ttu-id="a0394-171">trvat 100</span><span class="sxs-lookup"><span data-stu-id="a0394-171">take 100</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [<span data-ttu-id="a0394-172">shrnutí</span><span class="sxs-lookup"><span data-stu-id="a0394-172">summarize </span></span>](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [<span data-ttu-id="a0394-173">kde časové razítko > ago(3d)</span><span class="sxs-lookup"><span data-stu-id="a0394-173">where timestamp > ago(3d)</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

<span data-ttu-id="a0394-174">(Má více než 10 TIS řádků?</span><span class="sxs-lookup"><span data-stu-id="a0394-174">(Want more than 10k rows?</span></span> <span data-ttu-id="a0394-175">Zvažte použití [průběžné exportovat](app-insights-export-telemetry.md) místo.</span><span class="sxs-lookup"><span data-stu-id="a0394-175">Consider using [Continuous Export](app-insights-export-telemetry.md) instead.</span></span> <span data-ttu-id="a0394-176">Analýza je určená pro analýzy, nikoli načítání nezpracovaná data.)</span><span class="sxs-lookup"><span data-stu-id="a0394-176">Analytics is designed for analysis, rather than retrieving raw data.)</span></span>

## <a name="diagrams"></a><span data-ttu-id="a0394-177">Diagramy</span><span class="sxs-lookup"><span data-stu-id="a0394-177">Diagrams</span></span>
<span data-ttu-id="a0394-178">Vyberte typ hello diagramu, který chcete:</span><span class="sxs-lookup"><span data-stu-id="a0394-178">Select hello type of diagram you'd like:</span></span>

![Vyberte typ diagramu](./media/app-insights-analytics-using/230.png)

<span data-ttu-id="a0394-180">Pokud máte několik sloupců hello správné typy, můžete hello x a y osy a sloupec dimenze toosplit hello výsledků podle.</span><span class="sxs-lookup"><span data-stu-id="a0394-180">If you have several columns of hello right types, you can choose hello x and y axes, and a column of dimensions toosplit hello results by.</span></span>

<span data-ttu-id="a0394-181">Ve výchozím nastavení výsledky se zpočátku zobrazují jako tabulku a vyberete hello diagram ručně.</span><span class="sxs-lookup"><span data-stu-id="a0394-181">By default, results are initially displayed as a table, and you select hello diagram manually.</span></span> <span data-ttu-id="a0394-182">Ale můžete použít hello [vykreslení – direktiva](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) na konci hello tohoto dotazu tooselect diagram.</span><span class="sxs-lookup"><span data-stu-id="a0394-182">But you can use hello [render directive](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) at hello end of a query tooselect a diagram.</span></span>

### <a name="analytics-diagnostics"></a><span data-ttu-id="a0394-183">Analýza diagnostiky</span><span class="sxs-lookup"><span data-stu-id="a0394-183">Analytics diagnostics</span></span>


<span data-ttu-id="a0394-184">Na timechart Pokud nečekané Špička nebo krok v datech, může se zobrazit bod zvýrazněných v řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-184">On a timechart, if there is a sudden spike or step in your data, you may see a highlighted point on hello line.</span></span> <span data-ttu-id="a0394-185">To znamená, že Analytics diagnostiky zjistil kombinaci vlastnosti, které vyfiltrovat hello náhlé změny.</span><span class="sxs-lookup"><span data-stu-id="a0394-185">This indicates that Analytics Diagnostics has identified a combination of properties that filter out hello sudden change.</span></span> <span data-ttu-id="a0394-186">Klikněte na tlačítko hello bodu tooget další podrobnosti o hello filtr a toosee hello filtrované verze.</span><span class="sxs-lookup"><span data-stu-id="a0394-186">Click hello point tooget more detail on hello filter, and toosee hello filtered version.</span></span> <span data-ttu-id="a0394-187">To může pomoct zjistit, jaké změny způsobeny hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-187">This may help you identify what caused hello change.</span></span> 

[<span data-ttu-id="a0394-188">Další informace o analýzy diagnostiky</span><span class="sxs-lookup"><span data-stu-id="a0394-188">Learn more about Analytics Diagnostics</span></span>](app-insights-analytics-diagnostics.md)


![Analýza diagnostiky](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a><span data-ttu-id="a0394-190">Toodashboard PIN kód</span><span class="sxs-lookup"><span data-stu-id="a0394-190">Pin toodashboard</span></span>
<span data-ttu-id="a0394-191">Budete moct připnout diagram nebo tabulka, tooone z vaší [sdílet řídicí panely](app-insights-dashboards.md) – stačí kliknout na hello pin.</span><span class="sxs-lookup"><span data-stu-id="a0394-191">You can pin a diagram or table tooone of your [shared dashboards](app-insights-dashboards.md) - just click hello pin.</span></span> <span data-ttu-id="a0394-192">(Může být nutné příliš[upgradu vaší aplikace cenové balíček](app-insights-pricing.md) tooturn na tuto funkci.)</span><span class="sxs-lookup"><span data-stu-id="a0394-192">(You might need too[upgrade your app's pricing package](app-insights-pricing.md) tooturn on this feature.)</span></span> 

![Klikněte na připnout hello](./media/app-insights-analytics-using/pin-01.png)

<span data-ttu-id="a0394-194">To znamená, že při umístění společně toohelp řídicího panelu monitorování hello výkonu a využití webových služeb, můžete zahrnout poměrně složité analysis spolu s hello jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="a0394-194">This means that, when you put together a dashboard toohelp you monitor hello performance or usage of your web services, you can include quite complex analysis alongside hello other metrics.</span></span> 

<span data-ttu-id="a0394-195">Řídicí panel toohello tabulky, můžete připnout, pokud má čtyři nebo méně sloupců.</span><span class="sxs-lookup"><span data-stu-id="a0394-195">You can pin a table toohello dashboard, if it has four or fewer columns.</span></span> <span data-ttu-id="a0394-196">Zobrazí se pouze hello horních sedm řádků.</span><span class="sxs-lookup"><span data-stu-id="a0394-196">Only hello top seven rows are displayed.</span></span>

### <a name="dashboard-refresh"></a><span data-ttu-id="a0394-197">Obnovení řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="a0394-197">Dashboard refresh</span></span>
<span data-ttu-id="a0394-198">Hello grafu připnutý toohello řídicí panel se automaticky aktualizují podle opětovné spuštění dotazu hello přibližně každých hodin.</span><span class="sxs-lookup"><span data-stu-id="a0394-198">hello chart pinned toohello dashboard is refreshed automatically by re-running hello query approximately every hours.</span></span> <span data-ttu-id="a0394-199">Můžete také kliknutím na tlačítko Aktualizovat hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-199">You can also click hello Refresh button.</span></span>

### <a name="automatic-simplifications"></a><span data-ttu-id="a0394-200">Automatické zjednodušení</span><span class="sxs-lookup"><span data-stu-id="a0394-200">Automatic simplifications</span></span>

<span data-ttu-id="a0394-201">Některé zjednodušení jsou použité tooa grafu při připnout tooa řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="a0394-201">Certain simplifications are applied tooa chart when you pin it tooa dashboard.</span></span>

<span data-ttu-id="a0394-202">**Čas omezení:** dotazy jsou automaticky omezený toohello posledních 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="a0394-202">**Time restriction:** Queries are automatically limited toohello past 14 days.</span></span> <span data-ttu-id="a0394-203">Hello vliv je hello stejné jako v případě, že váš dotaz obsahuje `where timestamp > ago(14d)`.</span><span class="sxs-lookup"><span data-stu-id="a0394-203">hello effect is hello same as if your query includes `where timestamp > ago(14d)`.</span></span>

<span data-ttu-id="a0394-204">**Omezení počtu Bin:** -li zobrazit graf, který obsahuje mnoho diskrétní přihrádek (obvykle pruhový graf), hello menší vyplněná přihrádek budou automaticky seskupeny do jedné "ostatní" bin.</span><span class="sxs-lookup"><span data-stu-id="a0394-204">**Bin count restriction:** If you display a chart that has a lot of discrete bins (typically a bar chart), hello less populated bins are automatically grouped into a single "others" bin.</span></span> <span data-ttu-id="a0394-205">Tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="a0394-205">For example, this query:</span></span>

    requests | summarize count_search = count() by client_CountryOrRegion

<span data-ttu-id="a0394-206">Vypadá to v Analytics:</span><span class="sxs-lookup"><span data-stu-id="a0394-206">looks like this in Analytics:</span></span>

![Graf s dlouhou tail](./media/app-insights-analytics-using/pin-07.png)

<span data-ttu-id="a0394-208">ale pokud připnete ji tooa řídicí panel, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a0394-208">but when you pin it tooa dashboard, it looks like this:</span></span>

![Graf s omezenou přihrádek](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a><span data-ttu-id="a0394-210">Export tooExcel</span><span class="sxs-lookup"><span data-stu-id="a0394-210">Export tooExcel</span></span>
<span data-ttu-id="a0394-211">Po spuštění dotazu, můžete stáhnout soubor .csv.</span><span class="sxs-lookup"><span data-stu-id="a0394-211">After you've run a query, you can download a .csv file.</span></span> <span data-ttu-id="a0394-212">Klikněte na tlačítko **exportovat, aplikace Excel**.</span><span class="sxs-lookup"><span data-stu-id="a0394-212">Click **Export,  Excel**.</span></span>

## <a name="export-toopower-bi"></a><span data-ttu-id="a0394-213">Export tooPower BI</span><span class="sxs-lookup"><span data-stu-id="a0394-213">Export tooPower BI</span></span>
<span data-ttu-id="a0394-214">Umístěte kurzor hello v dotazu a vyberte **exportovat, Power BI**.</span><span class="sxs-lookup"><span data-stu-id="a0394-214">Put hello cursor in a query and choose **Export, Power BI**.</span></span>

![Export z Analytics tooPower BI](./media/app-insights-analytics-using/240.png)

<span data-ttu-id="a0394-216">Při spuštění dotazu hello v Power BI.</span><span class="sxs-lookup"><span data-stu-id="a0394-216">You run hello query in Power BI.</span></span> <span data-ttu-id="a0394-217">Můžete ho nastavit toorefresh podle plánu.</span><span class="sxs-lookup"><span data-stu-id="a0394-217">You can set it toorefresh on a schedule.</span></span>

<span data-ttu-id="a0394-218">S Power BI můžete vytvořit řídicí panely, které shromažďování dat z různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="a0394-218">With Power BI, you can create dashboards that bring together data from a wide variety of sources.</span></span>

[<span data-ttu-id="a0394-219">Další informace o exportu tooPower BI</span><span class="sxs-lookup"><span data-stu-id="a0394-219">Learn more about export tooPower BI</span></span>](app-insights-export-power-bi.md)

## <a name="deep-link"></a><span data-ttu-id="a0394-220">Přímý odkaz</span><span class="sxs-lookup"><span data-stu-id="a0394-220">Deep link</span></span>

<span data-ttu-id="a0394-221">Získejte odkaz v části **exportu, sdílené složky odkaz** , můžete odeslat tooanother uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0394-221">Get a link under **Export, Share link** that you can send tooanother user.</span></span> <span data-ttu-id="a0394-222">Zadaný uživatel hello [skupině prostředků přístup tooyour](app-insights-resources-roles-access-control.md), hello dotazu se otevřou v hello Analytics uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a0394-222">Provided hello user has [access tooyour resource group](app-insights-resources-roles-access-control.md), hello query will open in hello Analytics UI.</span></span>

<span data-ttu-id="a0394-223">(V odkazu hello hello text dotazu se zobrazí po "? q =", gzip komprimované a s kódováním base-64.</span><span class="sxs-lookup"><span data-stu-id="a0394-223">(In hello link, hello query text appears after "?q=", gzip compressed and base-64 encoded.</span></span> <span data-ttu-id="a0394-224">Můžete napsat kód toogenerate přímých odkazů zadat toousers.</span><span class="sxs-lookup"><span data-stu-id="a0394-224">You could write code toogenerate deep links that you provide toousers.</span></span> <span data-ttu-id="a0394-225">Způsob toorun Analytics z kódu je pomocí hello však doporučeno hello [REST API](https://dev.applicationinsights.io/).)</span><span class="sxs-lookup"><span data-stu-id="a0394-225">However, hello recommended way toorun Analytics from code is by using hello [REST API](https://dev.applicationinsights.io/).)</span></span>


## <a name="automation"></a><span data-ttu-id="a0394-226">Automation</span><span class="sxs-lookup"><span data-stu-id="a0394-226">Automation</span></span>

<span data-ttu-id="a0394-227">Použití hello [REST API služby Data Access](https://dev.applicationinsights.io/) toorun analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="a0394-227">Use hello  [Data Access REST API](https://dev.applicationinsights.io/) toorun Analytics queries.</span></span> <span data-ttu-id="a0394-228">[Například](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (pomocí prostředí PowerShell):</span><span class="sxs-lookup"><span data-stu-id="a0394-228">[For example](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (using PowerShell):</span></span>

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

<span data-ttu-id="a0394-229">Na rozdíl od hello Analytics uživatelského rozhraní hello REST API automaticky nepřidá všechny dotazy tooyour omezení časové razítko.</span><span class="sxs-lookup"><span data-stu-id="a0394-229">Unlike hello Analytics UI, hello REST API does not automatically add any timestamp limitation tooyour queries.</span></span> <span data-ttu-id="a0394-230">Mějte na paměti tooadd vlastní-klauzule where, tooavoid získávání obrovské odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a0394-230">Remember tooadd your own where-clause, tooavoid getting huge responses.</span></span>



## <a name="import-data"></a><span data-ttu-id="a0394-231">Import dat</span><span class="sxs-lookup"><span data-stu-id="a0394-231">Import data</span></span>

<span data-ttu-id="a0394-232">Data můžete importovat ze souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="a0394-232">You can import data from a CSV file.</span></span> <span data-ttu-id="a0394-233">Typická využití je tooimport statických dat, které se můžete připojit s tabulkami z telemetrie.</span><span class="sxs-lookup"><span data-stu-id="a0394-233">A typical usage is tooimport static data that you can join with tables from your telemetry.</span></span> 

<span data-ttu-id="a0394-234">Například pokud ověřený uživatelé se budou identifikovat telemetrie alias nebo zkomolené id, může importovat tabulku, která mapuje názvy tooreal aliasy.</span><span class="sxs-lookup"><span data-stu-id="a0394-234">For example, if authenticated users are identified in your telemetry by an alias or obfuscated id, you could import a table that maps aliases tooreal names.</span></span> <span data-ttu-id="a0394-235">Provedením spojení na telemetrická žádost hello můžete identifikovat uživatele, jejich skutečné názvy v sestavách analýzy hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-235">By performing a join on hello request telemetry, you can identify users by their real names in hello Analytics reports.</span></span>

### <a name="define-your-data-schema"></a><span data-ttu-id="a0394-236">Definování schématu dat</span><span class="sxs-lookup"><span data-stu-id="a0394-236">Define your data schema</span></span>

1. <span data-ttu-id="a0394-237">Klikněte na tlačítko **nastavení** (v horní části vlevo) a potom **zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="a0394-237">Click **Settings** (at top left) and then **Data Sources**.</span></span> 
2. <span data-ttu-id="a0394-238">Přidání zdroje dat, hello pokynů.</span><span class="sxs-lookup"><span data-stu-id="a0394-238">Add a data source, following hello instructions.</span></span> <span data-ttu-id="a0394-239">Jste vyzváni toosupply ukázku hello dat, která by měla obsahovat aspoň deset řádků.</span><span class="sxs-lookup"><span data-stu-id="a0394-239">You are asked toosupply a sample of hello data, which should include at least ten rows.</span></span> <span data-ttu-id="a0394-240">Je pak opravit hello schématu.</span><span class="sxs-lookup"><span data-stu-id="a0394-240">You then correct hello schema.</span></span>

<span data-ttu-id="a0394-241">Definuje zdroj dat, který pak můžete použít tooimport jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="a0394-241">This defines a data source, which you can then use tooimport individual tables.</span></span>

### <a name="import-a-table"></a><span data-ttu-id="a0394-242">Import tabulky</span><span class="sxs-lookup"><span data-stu-id="a0394-242">Import a table</span></span>

1. <span data-ttu-id="a0394-243">Otevřete vaše definice zdroje dat ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="a0394-243">Open your data source definition from hello list.</span></span>
2. <span data-ttu-id="a0394-244">Klikněte na tlačítko "Odeslat" a postupujte podle hello pokyny tooupload hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="a0394-244">Click "Upload" and follow hello instructions tooupload hello table.</span></span> <span data-ttu-id="a0394-245">To zahrnuje tooa volání rozhraní REST API, a proto je snadno tooautomate.</span><span class="sxs-lookup"><span data-stu-id="a0394-245">This involves a call tooa REST API, and so it is easy tooautomate.</span></span> 

<span data-ttu-id="a0394-246">Vaše tabulka je nyní k dispozici pro použití v analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="a0394-246">Your table is now available for use in Analytics queries.</span></span> <span data-ttu-id="a0394-247">Zobrazí se vám v Analytics</span><span class="sxs-lookup"><span data-stu-id="a0394-247">It will appear in Analytics</span></span> 

### <a name="use-hello-table"></a><span data-ttu-id="a0394-248">Tabulka hello</span><span class="sxs-lookup"><span data-stu-id="a0394-248">Use hello table</span></span>

<span data-ttu-id="a0394-249">Předpokládejme, že vaše definice zdroje dat se nazývá `usermap`, a že má dvě pole `realName` a `user_AuthenticatedId`.</span><span class="sxs-lookup"><span data-stu-id="a0394-249">Let's suppose your data source definition is called `usermap`, and that it has two fields, `realName` and `user_AuthenticatedId`.</span></span> <span data-ttu-id="a0394-250">Hello `requests` tabulka má také pole s názvem `user_AuthenticatedId`, tak, aby byl snadno toojoin je:</span><span class="sxs-lookup"><span data-stu-id="a0394-250">hello `requests` table also has a field named `user_AuthenticatedId`, so it's easy toojoin them:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
<span data-ttu-id="a0394-251">Hello výsledná tabulka požadavků má sloupec, `realName`.</span><span class="sxs-lookup"><span data-stu-id="a0394-251">hello resulting table of requests has an additional column, `realName`.</span></span>

### <a name="import-from-logstash"></a><span data-ttu-id="a0394-252">Importovat z LogStash</span><span class="sxs-lookup"><span data-stu-id="a0394-252">Import from LogStash</span></span>

<span data-ttu-id="a0394-253">Pokud používáte [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), můžete použít tooquery analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="a0394-253">If you use [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), you can use Analytics tooquery your logs.</span></span> <span data-ttu-id="a0394-254">Použití hello [modul plug-in, který prostřednictvím kanálu předá data do Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span><span class="sxs-lookup"><span data-stu-id="a0394-254">Use hello [plugin that pipes data into Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span></span> 

## <a name="video"></a><span data-ttu-id="a0394-255">Video</span><span class="sxs-lookup"><span data-stu-id="a0394-255">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

