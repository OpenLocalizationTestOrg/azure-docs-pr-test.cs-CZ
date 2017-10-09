---
title: "aaaExport analýzy protokolů data tooPower BI | Microsoft Docs"
description: "Power BI je služba cloudové obchodní analýzy od společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete nepřetržitě exportovat data z úložiště OMS hello do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Tento článek popisuje, jak tooconfigure dotazy v analýzy protokolů, které automaticky exportovat tooPower BI v pravidelných intervalech."
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
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a><span data-ttu-id="d13fe-105">Export dat tooPower analýzy protokolů BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-105">Export Log Analytics data tooPower BI</span></span>

>[!NOTE]
> <span data-ttu-id="d13fe-106">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak tento proces pro export dat tooPower analýzy protokolů BI přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="d13fe-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then this process for exporting Log Analytics data tooPower BI will no longer work.</span></span>  <span data-ttu-id="d13fe-107">Všechny existující plány, které jste vytvořili před upgradem bude zakázán.</span><span class="sxs-lookup"><span data-stu-id="d13fe-107">Any existing schedules that you created before upgrading will become disabled.</span></span> 
>
> <span data-ttu-id="d13fe-108">Po upgradu, hello Azure Log Analytics používá stejnou platformu jako Application Insights a použijete stejný proces tooexport analýzy protokolů dotazy tooPower BI jako hello [hello proces tooexport Application Insights dotazuje tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span><span class="sxs-lookup"><span data-stu-id="d13fe-108">After upgrade, Azure Log Analytics uses hello same platform as Application Insights, and you use hello same process tooexport Log Analytics queries tooPower BI as [hello process tooexport Application Insights queries tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span></span>  <span data-ttu-id="d13fe-109">Můžete buď vyexportovat hello dotazu pomocí hello Analytics konzole, jak je popsáno v tomto článku, nebo můžete vybrat hello **Power BI** tlačítko hello horní části obrazovky hello hello hledání protokolů portálu.</span><span class="sxs-lookup"><span data-stu-id="d13fe-109">You can either export hello query using hello Analytics console as described in that article, or you can select hello **Power BI** button at hello top of hello screen in hello Log Search portal.</span></span>



<span data-ttu-id="d13fe-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudové obchodní analýzy služba společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.</span><span class="sxs-lookup"><span data-stu-id="d13fe-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is a cloud based business analytics service from Microsoft that provides rich visualizations and reports for analysis of different sets of data.</span></span>  <span data-ttu-id="d13fe-111">Analýzy protokolů můžete automaticky exportovat data z úložiště OMS hello do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="d13fe-111">Log Analytics can automatically export data from hello OMS repository into Power BI so you can leverage its visualizations and analysis tools.</span></span>

<span data-ttu-id="d13fe-112">Když nakonfigurujete Power BI s analýzy protokolů, můžete vytvořit dotazy protokolu, které se export datových sad jejich výsledky toocorresponding v Power BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-112">When you configure Power BI with Log Analytics, you create log queries that export their results toocorresponding datasets in Power BI.</span></span>  <span data-ttu-id="d13fe-113">Hello dotazu a export pokračuje tooautomatically spustit podle plánu, který definujete datovou sadu hello tookeep až toodate s hello nejnovější data shromažďovaná společností analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d13fe-113">hello query and export continues tooautomatically run on a schedule that you define tookeep hello dataset up toodate with hello latest data collected by Log Analytics.</span></span>

![Log Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a><span data-ttu-id="d13fe-115">Plány Power BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-115">Power BI Schedules</span></span>
<span data-ttu-id="d13fe-116">A *Power BI plán* zahrnuje hledání protokolů, který vyexportuje sadu dat z hello OMS úložiště tooa odpovídající datovou sadu v Power BI a plán, který definuje, jak často tohoto hledání běží tookeep hello dataset aktuální.</span><span class="sxs-lookup"><span data-stu-id="d13fe-116">A *Power BI Schedule* includes a log search that exports a set of data from hello OMS repository tooa corresponding dataset in Power BI and a schedule that defines how often this search is run tookeep hello dataset current.</span></span>

<span data-ttu-id="d13fe-117">Hello pole v datové sadě hello bude odpovídat vlastnosti hello hello záznamů vrácených hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-117">hello fields in hello dataset will match hello properties of hello records returned by hello log search.</span></span>  <span data-ttu-id="d13fe-118">Pokud hledání hello vrátí záznamy různých typů pak datovou sadu hello bude obsahovat všechny vlastnosti hello z každé hello zahrnuty typů záznamů.</span><span class="sxs-lookup"><span data-stu-id="d13fe-118">If hello search returns records of different types then hello dataset will include all of hello properties from each of hello included record types.</span></span>  

> [!NOTE]
> <span data-ttu-id="d13fe-119">Je nejlepší postup toouse dotaz vyhledávání protokolu, který vrátí nezpracovaná data jako názvem na rozdíl od tooperforming žádné konsolidace pomocí příkazů jako třeba [měr](log-analytics-search-reference.md#measure).</span><span class="sxs-lookup"><span data-stu-id="d13fe-119">It is a best practice toouse a log search query that returns raw data as opposed tooperforming any consolidation using commands such as [Measure](log-analytics-search-reference.md#measure).</span></span>  <span data-ttu-id="d13fe-120">Všechny agregace a výpočty můžete provádět v Power BI od nezpracovaných dat hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-120">You can perform any aggregation and calculations in Power BI from hello raw data.</span></span>
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a><span data-ttu-id="d13fe-121">Připojení tooPower pracovní prostor OMS BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-121">Connecting OMS workspace tooPower BI</span></span>
<span data-ttu-id="d13fe-122">Před exportem z analýzy protokolů tooPower BI, je nutné připojit účtu Power BI tooyour pracovní prostor OMS pomocí hello následující postup.</span><span class="sxs-lookup"><span data-stu-id="d13fe-122">Before you can export from Log Analytics tooPower BI, you must connect your OMS workspace tooyour Power BI account using hello following procedure.</span></span>  

1. <span data-ttu-id="d13fe-123">V konzole OMS hello klikněte na tlačítko hello **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d13fe-123">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="d13fe-124">Vyberte **účty**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-124">Select **Accounts**.</span></span>
3. <span data-ttu-id="d13fe-125">V hello **informace o pracovním prostoru** klikněte na část **připojit tooPower účet BI**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-125">In hello **Workspace Information** section click **Connect tooPower BI Account**.</span></span>
4. <span data-ttu-id="d13fe-126">Zadejte hello přihlašovací údaje pro svůj účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-126">Enter hello credentials for your Power BI account.</span></span>

## <a name="create-a-power-bi-schedule"></a><span data-ttu-id="d13fe-127">Vytvoření plánu Power BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-127">Create a Power BI Schedule</span></span>
<span data-ttu-id="d13fe-128">Vytvořte plán Power BI pro každé datové sady pomocí hello následující postup.</span><span class="sxs-lookup"><span data-stu-id="d13fe-128">Create a Power BI Schedule for each dataset using hello following procedure.</span></span>

1. <span data-ttu-id="d13fe-129">V konzole OMS hello klikněte na tlačítko hello **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d13fe-129">In hello OMS console click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="d13fe-130">Zadejte nový dotaz nebo vyberte uloženého hledání, vrátí hello dat, které chcete tooexport příliš**Power BI**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-130">Type in a new query or select a saved search that returns hello data that you want tooexport too**Power BI**.</span></span>  
3. <span data-ttu-id="d13fe-131">Klikněte na tlačítko hello **Power BI** tlačítko hello horní části hello tooopen stránku hello **Power BI** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d13fe-131">Click hello **Power BI** button at hello top of hello page tooopen hello **Power BI** dialog.</span></span>
4. <span data-ttu-id="d13fe-132">Poskytování informací hello v hello následující tabulky a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-132">Provide hello information in hello following table and click **Save**.</span></span>

| <span data-ttu-id="d13fe-133">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d13fe-133">Property</span></span> | <span data-ttu-id="d13fe-134">Popis</span><span class="sxs-lookup"><span data-stu-id="d13fe-134">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d13fe-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d13fe-135">Name</span></span> |<span data-ttu-id="d13fe-136">Název tooidentify hello plán při zobrazení seznamu hello plány Power BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-136">Name tooidentify hello schedule when you view hello list of Power BI schedules.</span></span> |
| <span data-ttu-id="d13fe-137">Uložené hledání</span><span class="sxs-lookup"><span data-stu-id="d13fe-137">Saved Search</span></span> |<span data-ttu-id="d13fe-138">toorun vyhledávání protokolu Hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-138">hello log search toorun.</span></span>  <span data-ttu-id="d13fe-139">Můžete buď vybrat hello aktuální dotaz, nebo vyberte existující uložené hledání rozevíracím hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-139">You can either select hello current query or select an existing saved search from hello dropdown box.</span></span> |
| <span data-ttu-id="d13fe-140">Plán</span><span class="sxs-lookup"><span data-stu-id="d13fe-140">Schedule</span></span> |<span data-ttu-id="d13fe-141">Jak často toorun hello uložené vyhledávání a export datovou sadu toohello Power BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-141">How often toorun hello saved search and export toohello Power BI dataset.</span></span>  <span data-ttu-id="d13fe-142">Hello hodnota musí být mezi 15 minutami a 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="d13fe-142">hello value must be between 15 minutes and 24 hours.</span></span> |
| <span data-ttu-id="d13fe-143">Název datové sady</span><span class="sxs-lookup"><span data-stu-id="d13fe-143">Dataset Name</span></span> |<span data-ttu-id="d13fe-144">Název Hello hello datovou sadu v Power BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-144">hello name of hello dataset in Power BI.</span></span>  <span data-ttu-id="d13fe-145">Bude vytvořen, pokud neexistuje a aktualizovat, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d13fe-145">It will be created if it doesn’t exist and updated if it does exist.</span></span> |

## <a name="viewing-and-removing-power-bi-schedules"></a><span data-ttu-id="d13fe-146">Zobrazení a odebrání plány Power BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-146">Viewing and Removing Power BI Schedules</span></span>
<span data-ttu-id="d13fe-147">Zobrazení seznamu hello existující plán Power BI s hello následující postup.</span><span class="sxs-lookup"><span data-stu-id="d13fe-147">View hello list of existing Power BI Schedules with hello following procedure.</span></span>

1. <span data-ttu-id="d13fe-148">V konzole OMS hello klikněte na tlačítko hello **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d13fe-148">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="d13fe-149">Vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-149">Select **Power BI**.</span></span>

<span data-ttu-id="d13fe-150">Kromě toho toohello podrobnosti o hello naplánovat, hello počet pokusů, které hello plán byla spuštěna v hello minulého týdne a jsou zobrazeny hello stav poslední synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-150">In addition toohello details of hello schedule, hello number of times that hello schedule has run in hello past week and hello status of hello last sync are displayed.</span></span>  <span data-ttu-id="d13fe-151">Pokud hello synchronizace došlo k chybám, můžete kliknout na odkaz toorun hello hledání protokolů pro záznamy podrobné informace o této chybě hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-151">If hello sync encountered errors, you can click hello link toorun a log search for records with details of hello error.</span></span>

<span data-ttu-id="d13fe-152">Plán můžete odebrat tak, že kliknete na hello **X** v hello **odebrat sloupec**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-152">You can remove a schedule by clicking on hello **X** in hello **Remove column**.</span></span>  <span data-ttu-id="d13fe-153">Plán můžete zakázat tak, že vyberete **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-153">You can disable a schedule by selecting **Off**.</span></span>  <span data-ttu-id="d13fe-154">toomodify plánu, musíte ho odebrat a znovu ji vytvořte s novým nastavením hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-154">toomodify a schedule you must remove it and recreate it with hello new settings.</span></span>

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a><span data-ttu-id="d13fe-156">Ukázka návod</span><span class="sxs-lookup"><span data-stu-id="d13fe-156">Sample walkthrough</span></span>
<span data-ttu-id="d13fe-157">Hello následující části vás provede příklad vytvoření plánu Power BI a pomocí jeho toocreate datovou sadu jednoduchou sestavu.</span><span class="sxs-lookup"><span data-stu-id="d13fe-157">hello following section walks through an example of creating a Power BI Schedule and using its dataset toocreate a simple report.</span></span>  <span data-ttu-id="d13fe-158">V tomto příkladu všechny údaje o výkonu pro skupinu počítačů, je exportovaný tooPower BI a pak spojnicový graf je vytvořen toodisplay využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="d13fe-158">In this example, all performance data for a set of computers is exported tooPower BI and then a line graph is created toodisplay processor utilization.</span></span>

### <a name="create-log-search"></a><span data-ttu-id="d13fe-159">Vytvoření hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="d13fe-159">Create log search</span></span>
<span data-ttu-id="d13fe-160">Začneme vytvořením hledání protokolů pro hello data, že má být toosend toohello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="d13fe-160">We start by creating a log search for hello data that we want toosend toohello dataset.</span></span>  <span data-ttu-id="d13fe-161">V tomto příkladu použijeme dotaz, který vrátí všechny údaje o výkonu pro počítače s názvem, které začíná *srv*.</span><span class="sxs-lookup"><span data-stu-id="d13fe-161">In this example, we’ll use a query that returns all performance data for computers with a name that starts with *srv*.</span></span>  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a><span data-ttu-id="d13fe-163">Vytvoření hledání Power BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-163">Create Power BI Search</span></span>
<span data-ttu-id="d13fe-164">Kliknete na hello **Power BI** tlačítko tooopen hello Power BI dialogové okno a zadejte hello požadované informace.</span><span class="sxs-lookup"><span data-stu-id="d13fe-164">We click hello **Power BI** button tooopen hello Power BI dialog and provide hello required information.</span></span>  <span data-ttu-id="d13fe-165">Jsme chcete toto hledání toorun jednou za hodinu a vytvořit datovou sadu s názvem *Contoso výkonu*.</span><span class="sxs-lookup"><span data-stu-id="d13fe-165">We want this search toorun once per hour and create a dataset called *Contoso Perf*.</span></span>  <span data-ttu-id="d13fe-166">Vzhledem k tomu, že již máme otevřete vyhledávání hello, která vytvoří hello dat chceme, jsme zachovat výchozí hello *použít aktuální vyhledávací dotaz* pro **uložené výsledky hledání**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-166">Since we already have hello search open that creates hello data we want, we keep hello default of *Use current search query* for **Saved Search**.</span></span>

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a><span data-ttu-id="d13fe-168">Ověřte Power BI vyhledávání</span><span class="sxs-lookup"><span data-stu-id="d13fe-168">Verify Power BI Search</span></span>
<span data-ttu-id="d13fe-169">tooverify, že jsme vytvořili plán hello správně, nemůžeme zobrazit hello seznam hledání Power BI pod hello **nastavení** dlaždici na řídicím panelu hello OMS.</span><span class="sxs-lookup"><span data-stu-id="d13fe-169">tooverify that we created hello schedule correctly, we view hello list of Power BI Searches under hello **Settings** tile in hello OMS dashboard.</span></span>  <span data-ttu-id="d13fe-170">Jsme Počkejte několik minut a aktualizujte toto zobrazení, dokud hlásí, že byla spuštěna synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-170">We wait several minutes and refresh this view until it reports that hello sync has been run.</span></span>

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a><span data-ttu-id="d13fe-172">Ověření datové sady hello v Power BI</span><span class="sxs-lookup"><span data-stu-id="d13fe-172">Verify hello dataset in Power BI</span></span>
<span data-ttu-id="d13fe-173">Jsme Přihlaste se k naší účtu v [powerbi.microsoft.com](http://powerbi.microsoft.com/) a posuňte se příliš**datové sady** dole hello v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-173">We log into our account at [powerbi.microsoft.com](http://powerbi.microsoft.com/) and scroll too**Datasets** at hello bottom of hello left pane.</span></span>  <span data-ttu-id="d13fe-174">Uvidíte, že hello *Contoso výkonu* datové sady je uveden indikující, že naše exportu byla úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d13fe-174">We can see that hello *Contoso Perf* dataset is listed indicating that our export has run successfully.</span></span>

![Datová sada Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a><span data-ttu-id="d13fe-176">Vytvoření sestavy založené na datové sady</span><span class="sxs-lookup"><span data-stu-id="d13fe-176">Create report based on dataset</span></span>
<span data-ttu-id="d13fe-177">Jsme vyberte hello **Contoso výkonu** datovou sadu a potom klikněte na **výsledky** v hello **pole** podokně na hello správné tooview hello pole, které jsou součástí této datové sady.</span><span class="sxs-lookup"><span data-stu-id="d13fe-177">We select hello **Contoso Perf** dataset and then click on **Results** in hello **Fields** pane on hello right tooview hello fields that are part of this dataset.</span></span>  <span data-ttu-id="d13fe-178">toocreate řádku graf zobrazující využití procesoru pro každý počítač, můžeme provést hello následující akce.</span><span class="sxs-lookup"><span data-stu-id="d13fe-178">toocreate a line graph showing processor utilization for each computer, we perform hello following actions.</span></span>

1. <span data-ttu-id="d13fe-179">Vyberte hello řádku grafu vizualizace.</span><span class="sxs-lookup"><span data-stu-id="d13fe-179">Select hello Line chart visualization.</span></span>
2. <span data-ttu-id="d13fe-180">Přetáhněte **ObjectName** příliš**filtr úrovně sestav** a zkontrolujte **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-180">Drag **ObjectName** too**Report level filter** and check **Processor**.</span></span>
3. <span data-ttu-id="d13fe-181">Přetáhněte **název_čítače** příliš**filtr úrovně sestav** a zkontrolujte **% času procesoru**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-181">Drag **CounterName** too**Report level filter** and check **% Processor Time**.</span></span>
4. <span data-ttu-id="d13fe-182">Přetáhněte **přepočtené** příliš**hodnoty**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-182">Drag **CounterValue** too**Values**.</span></span>
5. <span data-ttu-id="d13fe-183">Přetáhněte **počítače** příliš**legendy**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-183">Drag **Computer** too**Legend**.</span></span>
6. <span data-ttu-id="d13fe-184">Přetáhněte **TimeGenerated** příliš**osy**.</span><span class="sxs-lookup"><span data-stu-id="d13fe-184">Drag **TimeGenerated** too**Axis**.</span></span>

<span data-ttu-id="d13fe-185">Vidíme, že hello výsledné spojnicový graf se zobrazí hello daty z naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="d13fe-185">We can see that hello resulting line graph is displayed with hello data from our dataset.</span></span>

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a><span data-ttu-id="d13fe-187">Uloží sestavu hello</span><span class="sxs-lookup"><span data-stu-id="d13fe-187">Save hello report</span></span>
<span data-ttu-id="d13fe-188">Nemůžeme uložit hello sestavy kliknutím na hello tlačítko hello horní části obrazovky hello uložit a ověřit, že je teď uvedený v části sestavy hello v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="d13fe-188">We save hello report by clicking on hello Save button at hello top of hello screen and validate that it is now listed in hello Reports section in hello left pane.</span></span>

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a><span data-ttu-id="d13fe-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d13fe-190">Next steps</span></span>
* <span data-ttu-id="d13fe-191">Další informace o [protokolu hledání](log-analytics-log-searches.md) toobuild dotazy, které se dají exportovat tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="d13fe-191">Learn about [log searches](log-analytics-log-searches.md) toobuild queries that can be exported tooPower BI.</span></span>
* <span data-ttu-id="d13fe-192">Další informace o [Power BI](http://powerbi.microsoft.com) toobuild vizualizace podle exportuje analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d13fe-192">Learn more about [Power BI](http://powerbi.microsoft.com) toobuild visualizations based on Log Analytics exports.</span></span>
