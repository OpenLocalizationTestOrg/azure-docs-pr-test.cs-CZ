---
title: "Exportovat data analýzy protokolů do Power BI | Microsoft Docs"
description: "Power BI je služba cloudové obchodní analýzy od společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete nepřetržitě exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Tento článek popisuje, jak nakonfigurovat dotazy v analýzy protokolů, které automaticky exportovat do Power BI v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 98befb16d27387e8f65a27771a2a32c264119d74
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a><span data-ttu-id="474cb-105">Exportovat data analýzy protokolů do Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-105">Export Log Analytics data to Power BI</span></span>

>[!NOTE]
> <span data-ttu-id="474cb-106">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak tento proces exportu analýzy protokolů dat do Power BI přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="474cb-106">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then this process for exporting Log Analytics data to Power BI will no longer work.</span></span>  <span data-ttu-id="474cb-107">Všechny existující plány, které jste vytvořili před upgradem bude zakázán.</span><span class="sxs-lookup"><span data-stu-id="474cb-107">Any existing schedules that you created before upgrading will become disabled.</span></span> 
>
> <span data-ttu-id="474cb-108">Po upgradu, analýzy protokolů Azure používá stejnou platformu jako Application Insights a použít stejný postup exportování dotazů analýzy protokolů pro Power BI jako [proces exportu dotazy Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span><span class="sxs-lookup"><span data-stu-id="474cb-108">After upgrade, Azure Log Analytics uses the same platform as Application Insights, and you use the same process to export Log Analytics queries to Power BI as [the process to export Application Insights queries to Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span></span>  <span data-ttu-id="474cb-109">Můžete buď export dotazu pomocí konzole analýzy, jak je popsáno v tomto článku, nebo můžete vybrat **Power BI** tlačítka v horní části obrazovky na portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="474cb-109">You can either export the query using the Analytics console as described in that article, or you can select the **Power BI** button at the top of the screen in the Log Search portal.</span></span>



<span data-ttu-id="474cb-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudové obchodní analýzy služba společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.</span><span class="sxs-lookup"><span data-stu-id="474cb-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is a cloud based business analytics service from Microsoft that provides rich visualizations and reports for analysis of different sets of data.</span></span>  <span data-ttu-id="474cb-111">Analýzy protokolů můžete automaticky exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="474cb-111">Log Analytics can automatically export data from the OMS repository into Power BI so you can leverage its visualizations and analysis tools.</span></span>

<span data-ttu-id="474cb-112">Když nakonfigurujete Power BI s analýzy protokolů, můžete vytvořit dotazy protokolu, které jejich výsledky exportovat do odpovídající datové sady ve službě Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-112">When you configure Power BI with Log Analytics, you create log queries that export their results to corresponding datasets in Power BI.</span></span>  <span data-ttu-id="474cb-113">Dotaz a export i nadále spouštět automaticky podle plánu, který definujete, aby byl aktuální datovou sadu s nejnovější data shromažďovaná společností analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="474cb-113">The query and export continues to automatically run on a schedule that you define to keep the dataset up to date with the latest data collected by Log Analytics.</span></span>

![Analýzy protokolů k Power BI.](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a><span data-ttu-id="474cb-115">Plány Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-115">Power BI Schedules</span></span>
<span data-ttu-id="474cb-116">A *Power BI plán* zahrnuje hledání protokolů, který exportuje sady dat z úložiště OMS na odpovídající datovou sadu v Power BI a plán, který definuje, jak často se spustí tohoto hledání zachovat aktuální datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="474cb-116">A *Power BI Schedule* includes a log search that exports a set of data from the OMS repository to a corresponding dataset in Power BI and a schedule that defines how often this search is run to keep the dataset current.</span></span>

<span data-ttu-id="474cb-117">Pole v sadě dat bude odpovídat vlastnosti záznamů protokolu nalezené.</span><span class="sxs-lookup"><span data-stu-id="474cb-117">The fields in the dataset will match the properties of the records returned by the log search.</span></span>  <span data-ttu-id="474cb-118">Pokud hledání vrátí záznamy různých typů, bude obsahovat datovou sadu všechny vlastnosti z jednotlivých typů součástí záznamu.</span><span class="sxs-lookup"><span data-stu-id="474cb-118">If the search returns records of different types then the dataset will include all of the properties from each of the included record types.</span></span>  

> [!NOTE]
> <span data-ttu-id="474cb-119">Je vhodné použít dotaz vyhledávání protokolu, který vrátí data o hrubém a provádění všech konsolidace pomocí příkazů jako třeba [měr](log-analytics-search-reference.md#measure).</span><span class="sxs-lookup"><span data-stu-id="474cb-119">It is a best practice to use a log search query that returns raw data as opposed to performing any consolidation using commands such as [Measure](log-analytics-search-reference.md#measure).</span></span>  <span data-ttu-id="474cb-120">Všechny agregací a výpočtů v Power BI můžete provést z nezpracovaná data.</span><span class="sxs-lookup"><span data-stu-id="474cb-120">You can perform any aggregation and calculations in Power BI from the raw data.</span></span>
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a><span data-ttu-id="474cb-121">Pracovní prostor OMS připojuje k Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-121">Connecting OMS workspace to Power BI</span></span>
<span data-ttu-id="474cb-122">Před exportem z analýzy protokolů pro Power BI, je nutné připojit ke svému účtu Power BI pomocí následujícího postupu vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="474cb-122">Before you can export from Log Analytics to Power BI, you must connect your OMS workspace to your Power BI account using the following procedure.</span></span>  

1. <span data-ttu-id="474cb-123">V konzole OMS klikněte na **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="474cb-123">In the OMS console click the **Settings** tile.</span></span>
2. <span data-ttu-id="474cb-124">Vyberte **účty**.</span><span class="sxs-lookup"><span data-stu-id="474cb-124">Select **Accounts**.</span></span>
3. <span data-ttu-id="474cb-125">V **informace o pracovním prostoru** klikněte na část **připojit k účtu Power BI**.</span><span class="sxs-lookup"><span data-stu-id="474cb-125">In the **Workspace Information** section click **Connect to Power BI Account**.</span></span>
4. <span data-ttu-id="474cb-126">Zadejte přihlašovací údaje pro svůj účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-126">Enter the credentials for your Power BI account.</span></span>

## <a name="create-a-power-bi-schedule"></a><span data-ttu-id="474cb-127">Vytvoření plánu Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-127">Create a Power BI Schedule</span></span>
<span data-ttu-id="474cb-128">Vytvoření plánu Power BI pro každé datové sady, pomocí následujícího postupu.</span><span class="sxs-lookup"><span data-stu-id="474cb-128">Create a Power BI Schedule for each dataset using the following procedure.</span></span>

1. <span data-ttu-id="474cb-129">V konzole OMS klikněte na **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="474cb-129">In the OMS console click the **Log Search** tile.</span></span>
2. <span data-ttu-id="474cb-130">Zadejte nový dotaz nebo vyberte uloženého hledání, který vrací data, která chcete exportovat do **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="474cb-130">Type in a new query or select a saved search that returns the data that you want to export to **Power BI**.</span></span>  
3. <span data-ttu-id="474cb-131">Klikněte **Power BI** tlačítka v horní části stránky otevřete **Power BI** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="474cb-131">Click the **Power BI** button at the top of the page to open the **Power BI** dialog.</span></span>
4. <span data-ttu-id="474cb-132">Poskytování informací v následující tabulce a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="474cb-132">Provide the information in the following table and click **Save**.</span></span>

| <span data-ttu-id="474cb-133">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="474cb-133">Property</span></span> | <span data-ttu-id="474cb-134">Popis</span><span class="sxs-lookup"><span data-stu-id="474cb-134">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="474cb-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="474cb-135">Name</span></span> |<span data-ttu-id="474cb-136">Název pro identifikaci podle plánu, při zobrazení seznamu plány Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-136">Name to identify the schedule when you view the list of Power BI schedules.</span></span> |
| <span data-ttu-id="474cb-137">Uložené hledání</span><span class="sxs-lookup"><span data-stu-id="474cb-137">Saved Search</span></span> |<span data-ttu-id="474cb-138">Vyhledávání protokolu ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="474cb-138">The log search to run.</span></span>  <span data-ttu-id="474cb-139">Můžete vybrat aktuální dotaz nebo rozevíracího seznamu vyberte položku existujícího uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="474cb-139">You can either select the current query or select an existing saved search from the dropdown box.</span></span> |
| <span data-ttu-id="474cb-140">Plán</span><span class="sxs-lookup"><span data-stu-id="474cb-140">Schedule</span></span> |<span data-ttu-id="474cb-141">Jak často ke spuštění uložené hledání a exportovat do datové sady Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-141">How often to run the saved search and export to the Power BI dataset.</span></span>  <span data-ttu-id="474cb-142">Hodnota musí být mezi 15 minutami a 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="474cb-142">The value must be between 15 minutes and 24 hours.</span></span> |
| <span data-ttu-id="474cb-143">Název datové sady</span><span class="sxs-lookup"><span data-stu-id="474cb-143">Dataset Name</span></span> |<span data-ttu-id="474cb-144">Název datové sady ve službě Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-144">The name of the dataset in Power BI.</span></span>  <span data-ttu-id="474cb-145">Bude vytvořen, pokud neexistuje a aktualizovat, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="474cb-145">It will be created if it doesn’t exist and updated if it does exist.</span></span> |

## <a name="viewing-and-removing-power-bi-schedules"></a><span data-ttu-id="474cb-146">Zobrazení a odebrání plány Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-146">Viewing and Removing Power BI Schedules</span></span>
<span data-ttu-id="474cb-147">Zobrazení seznamu existující plán Power BI pomocí následujícího postupu.</span><span class="sxs-lookup"><span data-stu-id="474cb-147">View the list of existing Power BI Schedules with the following procedure.</span></span>

1. <span data-ttu-id="474cb-148">V konzole OMS klikněte na **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="474cb-148">In the OMS console click the **Settings** tile.</span></span>
2. <span data-ttu-id="474cb-149">Vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="474cb-149">Select **Power BI**.</span></span>

<span data-ttu-id="474cb-150">Kromě podrobnosti plánu se zobrazí počet používající plán minulého týdne a stav poslední synchronizace.</span><span class="sxs-lookup"><span data-stu-id="474cb-150">In addition to the details of the schedule, the number of times that the schedule has run in the past week and the status of the last sync are displayed.</span></span>  <span data-ttu-id="474cb-151">Pokud synchronizace došlo k chybám, můžete kliknutím na odkaz ke spuštění vyhledávání protokolu pro záznamy s informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="474cb-151">If the sync encountered errors, you can click the link to run a log search for records with details of the error.</span></span>

<span data-ttu-id="474cb-152">Plán můžete odebrat tak, že kliknete na **X** v **odebrat sloupec**.</span><span class="sxs-lookup"><span data-stu-id="474cb-152">You can remove a schedule by clicking on the **X** in the **Remove column**.</span></span>  <span data-ttu-id="474cb-153">Plán můžete zakázat tak, že vyberete **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="474cb-153">You can disable a schedule by selecting **Off**.</span></span>  <span data-ttu-id="474cb-154">Změna plánu je třeba odebrat ji a znovu ji vytvořte s novým nastavením.</span><span class="sxs-lookup"><span data-stu-id="474cb-154">To modify a schedule you must remove it and recreate it with the new settings.</span></span>

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a><span data-ttu-id="474cb-156">Ukázka návod</span><span class="sxs-lookup"><span data-stu-id="474cb-156">Sample walkthrough</span></span>
<span data-ttu-id="474cb-157">V následující části vás provede příklad vytvoření plánu Power BI a vytvoříte jednoduchou sestavu pomocí její datové sady.</span><span class="sxs-lookup"><span data-stu-id="474cb-157">The following section walks through an example of creating a Power BI Schedule and using its dataset to create a simple report.</span></span>  <span data-ttu-id="474cb-158">V tomto příkladu se exportují všechny údaje o výkonu pro skupinu počítačů do Power BI a pak se vytvoří spojnicový graf pro zobrazení využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="474cb-158">In this example, all performance data for a set of computers is exported to Power BI and then a line graph is created to display processor utilization.</span></span>

### <a name="create-log-search"></a><span data-ttu-id="474cb-159">Vytvoření hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="474cb-159">Create log search</span></span>
<span data-ttu-id="474cb-160">Začneme vytvořením hledání protokolů pro data, která nám chcete poslat do datové sady.</span><span class="sxs-lookup"><span data-stu-id="474cb-160">We start by creating a log search for the data that we want to send to the dataset.</span></span>  <span data-ttu-id="474cb-161">V tomto příkladu použijeme dotaz, který vrátí všechny údaje o výkonu pro počítače s názvem, které začíná *srv*.</span><span class="sxs-lookup"><span data-stu-id="474cb-161">In this example, we’ll use a query that returns all performance data for computers with a name that starts with *srv*.</span></span>  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a><span data-ttu-id="474cb-163">Vytvoření hledání Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-163">Create Power BI Search</span></span>
<span data-ttu-id="474cb-164">Kliknete na **Power BI** tlačítko otevřete dialogové okno Power BI a zadejte požadované informace.</span><span class="sxs-lookup"><span data-stu-id="474cb-164">We click the **Power BI** button to open the Power BI dialog and provide the required information.</span></span>  <span data-ttu-id="474cb-165">Chceme tohoto hledání spustit jednou za hodinu a vytvořit datovou sadu s názvem *Contoso výkonu*.</span><span class="sxs-lookup"><span data-stu-id="474cb-165">We want this search to run once per hour and create a dataset called *Contoso Perf*.</span></span>  <span data-ttu-id="474cb-166">Vzhledem k tomu, že již máme otevřete vyhledávání, který vytváří dat chceme, jsme ponechte výchozí *použít aktuální vyhledávací dotaz* pro **uložené výsledky hledání**.</span><span class="sxs-lookup"><span data-stu-id="474cb-166">Since we already have the search open that creates the data we want, we keep the default of *Use current search query* for **Saved Search**.</span></span>

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a><span data-ttu-id="474cb-168">Ověřte Power BI vyhledávání</span><span class="sxs-lookup"><span data-stu-id="474cb-168">Verify Power BI Search</span></span>
<span data-ttu-id="474cb-169">Chcete zkontrolovat, že jsme vytvořili plán správně, nemůžeme zobrazit seznam hledání Power BI pod **nastavení** dlaždici na řídicím panelu OMS.</span><span class="sxs-lookup"><span data-stu-id="474cb-169">To verify that we created the schedule correctly, we view the list of Power BI Searches under the **Settings** tile in the OMS dashboard.</span></span>  <span data-ttu-id="474cb-170">Jsme Počkejte několik minut a aktualizujte toto zobrazení, dokud ho hlásí, že synchronizace byl spuštěn.</span><span class="sxs-lookup"><span data-stu-id="474cb-170">We wait several minutes and refresh this view until it reports that the sync has been run.</span></span>

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a><span data-ttu-id="474cb-172">Ověření datové sady ve službě Power BI</span><span class="sxs-lookup"><span data-stu-id="474cb-172">Verify the dataset in Power BI</span></span>
<span data-ttu-id="474cb-173">Jsme Přihlaste se k naší účtu v [powerbi.microsoft.com](http://powerbi.microsoft.com/) a přejděte k položce **datové sady** v dolní části levého podokna.</span><span class="sxs-lookup"><span data-stu-id="474cb-173">We log into our account at [powerbi.microsoft.com](http://powerbi.microsoft.com/) and scroll to **Datasets** at the bottom of the left pane.</span></span>  <span data-ttu-id="474cb-174">Jsme uvidíte, že *Contoso výkonu* datové sady je uveden indikující, že naše exportu byla úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="474cb-174">We can see that the *Contoso Perf* dataset is listed indicating that our export has run successfully.</span></span>

![Datová sada Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a><span data-ttu-id="474cb-176">Vytvoření sestavy založené na datové sady</span><span class="sxs-lookup"><span data-stu-id="474cb-176">Create report based on dataset</span></span>
<span data-ttu-id="474cb-177">Jsme vyberte **Contoso výkonu** datovou sadu a potom klikněte na **výsledky** v **pole** podokně na pravé straně, chcete-li zobrazit pole, které jsou součástí této datové sady.</span><span class="sxs-lookup"><span data-stu-id="474cb-177">We select the **Contoso Perf** dataset and then click on **Results** in the **Fields** pane on the right to view the fields that are part of this dataset.</span></span>  <span data-ttu-id="474cb-178">Pokud chcete vytvořit spojnicový graf zobrazující využití procesoru pro každý počítač, můžeme provést následující akce.</span><span class="sxs-lookup"><span data-stu-id="474cb-178">To create a line graph showing processor utilization for each computer, we perform the following actions.</span></span>

1. <span data-ttu-id="474cb-179">Vyberte graf vizualizaci řádku.</span><span class="sxs-lookup"><span data-stu-id="474cb-179">Select the Line chart visualization.</span></span>
2. <span data-ttu-id="474cb-180">Přetáhněte **ObjectName** k **filtr úrovně sestav** a zkontrolujte **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="474cb-180">Drag **ObjectName** to **Report level filter** and check **Processor**.</span></span>
3. <span data-ttu-id="474cb-181">Přetáhněte **název_čítače** k **filtr úrovně sestav** a zkontrolujte **% času procesoru**.</span><span class="sxs-lookup"><span data-stu-id="474cb-181">Drag **CounterName** to **Report level filter** and check **% Processor Time**.</span></span>
4. <span data-ttu-id="474cb-182">Přetáhněte **přepočtené** k **hodnoty**.</span><span class="sxs-lookup"><span data-stu-id="474cb-182">Drag **CounterValue** to **Values**.</span></span>
5. <span data-ttu-id="474cb-183">Přetáhněte **počítače** k **legendy**.</span><span class="sxs-lookup"><span data-stu-id="474cb-183">Drag **Computer** to **Legend**.</span></span>
6. <span data-ttu-id="474cb-184">Přetáhněte **TimeGenerated** k **osy**.</span><span class="sxs-lookup"><span data-stu-id="474cb-184">Drag **TimeGenerated** to **Axis**.</span></span>

<span data-ttu-id="474cb-185">Vidíme, že se zobrazí výsledná spojnicový graf s daty z naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="474cb-185">We can see that the resulting line graph is displayed with the data from our dataset.</span></span>

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a><span data-ttu-id="474cb-187">Uložení sestavy</span><span class="sxs-lookup"><span data-stu-id="474cb-187">Save the report</span></span>
<span data-ttu-id="474cb-188">Jsme sestavu uložte kliknutím na tlačítko Uložit v horní části obrazovky a ověřit, že je teď uvedený v části sestavy v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="474cb-188">We save the report by clicking on the Save button at the top of the screen and validate that it is now listed in the Reports section in the left pane.</span></span>

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a><span data-ttu-id="474cb-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="474cb-190">Next steps</span></span>
* <span data-ttu-id="474cb-191">Další informace o [protokolu hledání](log-analytics-log-searches.md) vytvářet dotazy, které je možné exportovat do Power BI.</span><span class="sxs-lookup"><span data-stu-id="474cb-191">Learn about [log searches](log-analytics-log-searches.md) to build queries that can be exported to Power BI.</span></span>
* <span data-ttu-id="474cb-192">Další informace o [Power BI](http://powerbi.microsoft.com) vytvářet vizualizace podle exportuje analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="474cb-192">Learn more about [Power BI](http://powerbi.microsoft.com) to build visualizations based on Log Analytics exports.</span></span>
