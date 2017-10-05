---
title: "Monitorování a Správa kanálů data - Azure | Microsoft Docs"
description: "Naučte se používat monitorování a správu aplikace ke sledování a správě Azure data Factory a kanály."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: d5a2d1f3d85b8a2212326cfcfd0ba5d80356b769
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a><span data-ttu-id="b47fb-103">Monitorování a Správa kanálů služby Azure Data Factory pomocí monitorování a správy aplikace</span><span class="sxs-lookup"><span data-stu-id="b47fb-103">Monitor and manage Azure Data Factory pipelines by using the Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b47fb-104">Použití Azure portal nebo Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b47fb-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="b47fb-105">Pomocí monitorování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="b47fb-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="b47fb-106">Tento článek popisuje, jak používat monitorování a správu aplikace monitorovat, spravovat a ladit kanály Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b47fb-106">This article describes how to use the Monitoring and Management app to monitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="b47fb-107">Nabízí taky informace o tom, jak vytvářet výstrahy, které dostanete upozornění na selhání.</span><span class="sxs-lookup"><span data-stu-id="b47fb-107">It also provides information on how to create alerts to get notified on failures.</span></span> <span data-ttu-id="b47fb-108">Můžete začít s pomocí následujícím videem aplikace:</span><span class="sxs-lookup"><span data-stu-id="b47fb-108">You can get started with using the application by watching the following video:</span></span>

> [!NOTE]
> <span data-ttu-id="b47fb-109">Uživatelské rozhraní zobrazené na videu nemusí přesně odpovídat vidět na portálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-109">The user interface shown in the video may not exactly match what you see in the portal.</span></span> <span data-ttu-id="b47fb-110">Je mírně starší, ale koncepty zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="b47fb-110">It's slightly older, but concepts remain the same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a><span data-ttu-id="b47fb-111">Spustit monitorování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="b47fb-111">Launch the Monitoring and Management app</span></span>
<span data-ttu-id="b47fb-112">Chcete-li spustit aplikaci monitorování a správa, klikněte na tlačítko **monitorování a správa** na dlaždici **Data Factory** okno objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="b47fb-112">To launch the Monitor and Management app, click the **Monitor & Manage** tile on the **Data Factory** blade for your data factory.</span></span>

![Monitorování dlaždice na domovské stránce objektu pro vytváření dat](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="b47fb-114">Měli byste vidět monitorování a správu aplikací, otevřete v samostatném okně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-114">You should see the Monitoring and Management app open in a separate window.</span></span>  

![Monitorování a správa aplikací](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="b47fb-116">Pokud se zobrazí, že webový prohlížeč zasekl ve fázi "autorizace …", zrušte **blokovat soubory cookie třetích stran a data lokality** políčko--nebo udržování je vybrána, vytvořte výjimku pro **login.microsoftonline.com**, a Zkuste se znovu otevřete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b47fb-116">If you see that the web browser is stuck at "Authorizing...", clear the **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try to open the app again.</span></span>


<span data-ttu-id="b47fb-117">V seznamu okna aktivity v prostředním podokně zobrazí okno s aktivity pro každé spuštění aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-117">In the Activity Windows list in the middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="b47fb-118">Například pokud máte aktivity naplánované spuštění každou hodinu pět hodin, uvidíte pět okna aktivity spojené s pěti datové řezy.</span><span class="sxs-lookup"><span data-stu-id="b47fb-118">For example, if you have the activity scheduled to run hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="b47fb-119">Pokud nevidíte okna aktivity v seznamu dole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b47fb-119">If you don't see activity windows in the list at the bottom, do the following:</span></span>
 
- <span data-ttu-id="b47fb-120">Aktualizace **počáteční čas** a **čas ukončení** filtrů v horní části odpovídající počáteční a koncový čas svůj kanál, a klikněte **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b47fb-120">Update the **start time** and **end time** filters at the top to match the start and end times of your pipeline, and then click the **Apply** button.</span></span>  
- <span data-ttu-id="b47fb-121">V seznamu aktivity Windows automaticky neobnoví.</span><span class="sxs-lookup"><span data-stu-id="b47fb-121">The Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="b47fb-122">Klikněte na tlačítko **aktualizovat** tlačítka na panelu nástrojů v **aktivity Windows** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-122">Click the **Refresh** button on the toolbar in the **Activity Windows** list.</span></span>  

<span data-ttu-id="b47fb-123">Pokud nemáte aplikaci služby Data Factory tyto kroky otestovat, proveďte tento kurz: [kopírování dat z úložiště objektů Blob do SQL Database pomocí služby Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b47fb-123">If you don't have a Data Factory application to test these steps with, do the tutorial: [copy data from Blob Storage to SQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-the-monitoring-and-management-app"></a><span data-ttu-id="b47fb-124">Porozumění sledování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="b47fb-124">Understand the Monitoring and Management app</span></span>
<span data-ttu-id="b47fb-125">Na levé straně jsou tři karty: **Průzkumníka prostředků**, **monitorování zobrazení**, a **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-125">There are three tabs on the left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="b47fb-126">Na první kartě (**Průzkumníka prostředků**) je standardně vybraná.</span><span class="sxs-lookup"><span data-stu-id="b47fb-126">The first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="b47fb-127">Průzkumník prostředků</span><span class="sxs-lookup"><span data-stu-id="b47fb-127">Resource Explorer</span></span>
<span data-ttu-id="b47fb-128">Zobrazí následující:</span><span class="sxs-lookup"><span data-stu-id="b47fb-128">You see the following:</span></span>

* <span data-ttu-id="b47fb-129">Průzkumník prostředků **stromovém zobrazení** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-129">The Resource Explorer **tree view** in the left pane.</span></span>
* <span data-ttu-id="b47fb-130">**Zobrazení diagramu** nahoře v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-130">The **Diagram View** at the top in the middle pane.</span></span>
* <span data-ttu-id="b47fb-131">**Aktivity Windows** seznamu dole v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-131">The **Activity Windows** list at the bottom in the middle pane.</span></span>
* <span data-ttu-id="b47fb-132">**Vlastnosti**, **aktivity okno Průzkumníka**, a **skriptu** karty v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-132">The **Properties**, **Activity Window Explorer**, and **Script** tabs in the right pane.</span></span>

<span data-ttu-id="b47fb-133">V Průzkumníku prostředků zobrazí všechny prostředky (kanály, datové sady, propojených služeb) v objektu pro vytváření dat ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b47fb-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in the data factory in a tree view.</span></span> <span data-ttu-id="b47fb-134">Když vyberete objekt v Průzkumníku prostředků:</span><span class="sxs-lookup"><span data-stu-id="b47fb-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="b47fb-135">Související entity služby Data Factory je zvýrazněn v zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-135">The associated Data Factory entity is highlighted in the Diagram View.</span></span>
* <span data-ttu-id="b47fb-136">[Související aktivity windows](data-factory-scheduling-and-execution.md) jsou vyznačené na seznamu okna aktivity v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b47fb-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in the Activity Windows list at the bottom.</span></span>  
* <span data-ttu-id="b47fb-137">V okně vlastností v pravém podokně se zobrazí vlastnosti vybraného objektu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-137">The properties of the selected object are shown in the Properties window in the right pane.</span></span>
* <span data-ttu-id="b47fb-138">Definici JSON vybraného objektu se zobrazí, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b47fb-138">The JSON definition of the selected object is shown, if applicable.</span></span> <span data-ttu-id="b47fb-139">Příklad: propojené služby, datové sady nebo kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Průzkumník prostředků](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="b47fb-141">Najdete v článku [plánování a provádění](data-factory-scheduling-and-execution.md) článku podrobné koncepční informace o windows aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-141">See the [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="b47fb-142">Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="b47fb-142">Diagram View</span></span>
<span data-ttu-id="b47fb-143">Zobrazení diagramu objektu pro vytváření dat poskytuje skla ke sledování a správě objekt pro vytváření dat a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="b47fb-143">The Diagram View of a data factory provides a single pane of glass to monitor and manage a data factory and its assets.</span></span> <span data-ttu-id="b47fb-144">Když vyberete entity služby Data Factory (datové sady nebo kanál) v zobrazení diagramu:</span><span class="sxs-lookup"><span data-stu-id="b47fb-144">When you select a Data Factory entity (dataset/pipeline) in the Diagram View:</span></span>

* <span data-ttu-id="b47fb-145">Entity objektu pro vytváření dat je vybraný ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b47fb-145">The data factory entity is selected in the tree view.</span></span>
* <span data-ttu-id="b47fb-146">Okna přidružené aktivity jsou vyznačené na seznamu okna aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-146">The associated activity windows are highlighted in the Activity Windows list.</span></span>
* <span data-ttu-id="b47fb-147">V okně Vlastnosti se zobrazí vlastnosti vybraného objektu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-147">The properties of the selected object are shown in the Properties window.</span></span>

<span data-ttu-id="b47fb-148">Když kanál je povolené (ne v pozastaveném stavu), zobrazí se zelený řádku:</span><span class="sxs-lookup"><span data-stu-id="b47fb-148">When the pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![Spuštění kanálu](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="b47fb-150">Můžete pozastavit, obnovit nebo ukončit kanál tak, že ho vyberete v zobrazení diagramu a pomocí tlačítek na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b47fb-150">You can pause, resume, or terminate a pipeline by selecting it in the diagram view and using the buttons on the command bar.</span></span>

![Pozastavení nebo obnovení na panelu příkazů](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="b47fb-152">Existují tři panelu příkazů pro kanál v zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-152">There are three command bar buttons for the pipeline in the Diagram View.</span></span> <span data-ttu-id="b47fb-153">Na druhé tlačítko můžete pozastavit kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-153">You can use the second button to pause the pipeline.</span></span> <span data-ttu-id="b47fb-154">Pozastavení není ukončit probíhající aktivity a umožňuje jim přejít k dokončení.</span><span class="sxs-lookup"><span data-stu-id="b47fb-154">Pausing doesn't terminate the currently running activities and lets them proceed to completion.</span></span> <span data-ttu-id="b47fb-155">Tlačítko třetí pozastavuje kanálu a ukončí její existující provádění aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-155">The third button pauses the pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="b47fb-156">Na první tlačítko obnoví kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-156">The first button resumes the pipeline.</span></span> <span data-ttu-id="b47fb-157">Pokud vaše kanálu je pozastavena, změní barvu kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-157">When your pipeline is paused, the color of the pipeline changes.</span></span> <span data-ttu-id="b47fb-158">Například pozastavený kanálu vypadá jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="b47fb-158">For example, a paused pipeline looks like in the following image:</span></span> 

![Pozastavená kanálu](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="b47fb-160">Pomocí klávesy Ctrl můžete vybrat víc dva nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="b47fb-160">You can multi-select two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="b47fb-161">Panelu příkazů můžete pozastavit nebo obnovit více kanálů najednou.</span><span class="sxs-lookup"><span data-stu-id="b47fb-161">You can use the command bar buttons to pause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="b47fb-162">Můžete také kanál klikněte pravým tlačítkem a vyberte možnosti pro pozastavení, obnovení nebo ukončit kanál.</span><span class="sxs-lookup"><span data-stu-id="b47fb-162">You can also right-click a pipeline and select options to suspend, resume, or terminate a pipeline.</span></span> 

![Kontextové nabídky pro kanál](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="b47fb-164">Klikněte **otevřít kanál** možnost zobrazíte všechny aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-164">Click the **Open pipeline** option to see all the activities in the pipeline.</span></span> 

![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="b47fb-166">V zobrazení otevřenou kanálu zobrazí všechny aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-166">In the opened pipeline view, you see all activities in the pipeline.</span></span> <span data-ttu-id="b47fb-167">V tomto příkladu je jenom jedna aktivita: aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="b47fb-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Otevřenou kanálu](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="b47fb-169">Chcete-li vrátit do předchozího zobrazení, klikněte na název objektu pro vytváření dat v nabídce s popisem cesty v horní části.</span><span class="sxs-lookup"><span data-stu-id="b47fb-169">To go back to the previous view, click the data factory name in the breadcrumb menu at the top.</span></span>

<span data-ttu-id="b47fb-170">V zobrazení kanálu po výběru datovou sadu výstupů nebo když přesunutím ukazatele myši nad výstupní datovou sadu, se zobrazí automaticky otevírané okno okno aktivity Windows pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-170">In the pipeline view, when you select an output dataset or when you move your mouse over the output dataset, you see the Activity Windows pop-up window for that dataset.</span></span>

![Místní okno Windows aktivity](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="b47fb-172">Můžete kliknout na okno s aktivity zobrazíte podrobnosti ho **vlastnosti** okno v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-172">You can click an activity window to see details for it in the **Properties** window in the right pane.</span></span>

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="b47fb-174">V pravém podokně přepnout **aktivity okno Průzkumníka** karty zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b47fb-174">In the right pane, switch to the **Activity Window Explorer** tab to see more details.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="b47fb-176">Zobrazí také **přeložit proměnné** pro každý pokus o spuštění pro aktivitu v **pokusy o** části.</span><span class="sxs-lookup"><span data-stu-id="b47fb-176">You also see **resolved variables** for each run attempt for an activity in the **Attempts** section.</span></span>

![Vyřešený proměnné](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="b47fb-178">Přepnout **skriptu** karty zobrazíte definici JSON skript pro vybraný objekt.</span><span class="sxs-lookup"><span data-stu-id="b47fb-178">Switch to the **Script** tab to see the JSON script definition for the selected object.</span></span>   

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="b47fb-180">Zobrazí okna aktivity na třech místech:</span><span class="sxs-lookup"><span data-stu-id="b47fb-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="b47fb-181">Místní nabídce okna aktivity v zobrazení diagramu (střední podokno).</span><span class="sxs-lookup"><span data-stu-id="b47fb-181">The Activity Windows pop-up in the Diagram View (middle pane).</span></span>
* <span data-ttu-id="b47fb-182">Průzkumník okno aktivity v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-182">The Activity Window Explorer in the right pane.</span></span>
* <span data-ttu-id="b47fb-183">Seznam okna aktivity v dolním podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-183">The Activity Windows list in the bottom pane.</span></span>

<span data-ttu-id="b47fb-184">V automaticky otevírané okno aktivity Windows a aktivity okno Průzkumníka můžete přejít do předchozího týdne a další týden pomocí šipky vlevo a vpravo.</span><span class="sxs-lookup"><span data-stu-id="b47fb-184">In the Activity Windows pop-up and Activity Window Explorer, you can scroll to the previous week and the next week by using the left and right arrows.</span></span>

![Aktivita okno Průzkumníka levé nebo pravé šipky](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="b47fb-186">V dolní části zobrazení diagramu, zobrazí tato tlačítka: přiblížení přiblížit, oddálit, přizpůsobit, zvětšení 100 %, rozložení zámku.</span><span class="sxs-lookup"><span data-stu-id="b47fb-186">At the bottom of the Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom to Fit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="b47fb-187">**Zámku rozložení** tlačítko zabraňuje nechtěnému přesunutí tabulek a kanálů v zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-187">The **Lock layout** button prevents you from accidentally moving tables and pipelines in the Diagram View.</span></span> <span data-ttu-id="b47fb-188">Ve výchozím nastavení je.</span><span class="sxs-lookup"><span data-stu-id="b47fb-188">It's on by default.</span></span> <span data-ttu-id="b47fb-189">Můžete ho vypnout a pohyb entity v diagramu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-189">You can turn it off and move entities around in the diagram.</span></span> <span data-ttu-id="b47fb-190">Když vypnete ho, můžete na poslední tlačítko umístit automaticky tabulky a kanály.</span><span class="sxs-lookup"><span data-stu-id="b47fb-190">When you turn it off, you can use the last button to automatically position tables and pipelines.</span></span> <span data-ttu-id="b47fb-191">Můžete také přiblížení nebo oddálení pomocí kolečka myši.</span><span class="sxs-lookup"><span data-stu-id="b47fb-191">You can also zoom in or out by using the mouse wheel.</span></span>

![Diagram příkazy přiblížení zobrazení](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="b47fb-193">Seznam aktivit Windows</span><span class="sxs-lookup"><span data-stu-id="b47fb-193">Activity Windows list</span></span>
<span data-ttu-id="b47fb-194">V seznamu okna aktivity v dolní části v prostředním podokně se zobrazí všechny aktivity windows pro datovou sadu, který jste vybrali v Průzkumníku prostředků nebo zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-194">The Activity Windows list at the bottom of the middle pane displays all activity windows for the dataset that you selected in the Resource Explorer or the Diagram View.</span></span> <span data-ttu-id="b47fb-195">Ve výchozím nastavení je v seznamu v sestupném pořadí, což znamená, že vidíte nejnovější okna aktivita v horní části.</span><span class="sxs-lookup"><span data-stu-id="b47fb-195">By default, the list is in descending order, which means that you see the latest activity window at the top.</span></span>

![Seznam aktivit Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="b47fb-197">Tento seznam není aktualizuje automaticky, takže použijte tlačítko Aktualizovat na panelu nástrojů jej ručně aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b47fb-197">This list doesn't refresh automatically, so use the refresh button on the toolbar to manually refresh it.</span></span>  

<span data-ttu-id="b47fb-198">Okna aktivity může být v jednom z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="b47fb-198">Activity windows can be in one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="b47fb-199">Status</span><span class="sxs-lookup"><span data-stu-id="b47fb-199">Status</span></span></th><th align="left"><span data-ttu-id="b47fb-200">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="b47fb-200">Substatus</span></span></th><th align="left"><span data-ttu-id="b47fb-201">Popis</span><span class="sxs-lookup"><span data-stu-id="b47fb-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="b47fb-202">Čekání</span><span class="sxs-lookup"><span data-stu-id="b47fb-202">Waiting</span></span></td><td><span data-ttu-id="b47fb-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="b47fb-203">ScheduleTime</span></span></td><td><span data-ttu-id="b47fb-204">Čas ještě nenastal pro okna aktivity ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="b47fb-204">The time hasn't come for the activity window to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="b47fb-205">DatasetDependencies</span></span></td><td><span data-ttu-id="b47fb-206">Upstreamové závislosti nejsou připravené.</span><span class="sxs-lookup"><span data-stu-id="b47fb-206">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="b47fb-207">ComputeResources</span></span></td><td><span data-ttu-id="b47fb-208">Výpočetní prostředky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b47fb-208">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="b47fb-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="b47fb-210">Jsou všechny instance aktivit právě zpracovávají jiné aktivity windows.</span><span class="sxs-lookup"><span data-stu-id="b47fb-210">All the activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="b47fb-211">ActivityResume</span></span></td><td><span data-ttu-id="b47fb-212">Aktivita je pozastavená a aktivity windows nelze spustit, dokud nebude obnovená.</span><span class="sxs-lookup"><span data-stu-id="b47fb-212">The activity is paused and can't run the activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-213">Opakování</span><span class="sxs-lookup"><span data-stu-id="b47fb-213">Retry</span></span></td><td><span data-ttu-id="b47fb-214">Probíhá pokus o spuštění aktivity je zopakován.</span><span class="sxs-lookup"><span data-stu-id="b47fb-214">The activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-215">Ověření</span><span class="sxs-lookup"><span data-stu-id="b47fb-215">Validation</span></span></td><td><span data-ttu-id="b47fb-216">Ověření se ještě nespustilo.</span><span class="sxs-lookup"><span data-stu-id="b47fb-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="b47fb-217">ValidationRetry</span></span></td><td><span data-ttu-id="b47fb-218">Ověření čeká na opakovat.</span><span class="sxs-lookup"><span data-stu-id="b47fb-218">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="b47fb-219">InProgress</span><span class="sxs-lookup"><span data-stu-id="b47fb-219">InProgress</span></span></td><td><span data-ttu-id="b47fb-220">Probíhá ověřování</span><span class="sxs-lookup"><span data-stu-id="b47fb-220">Validating</span></span></td><td><span data-ttu-id="b47fb-221">Probíhá ověřování.</span><span class="sxs-lookup"><span data-stu-id="b47fb-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="b47fb-222">Okna aktivity je zpracovávána.</span><span class="sxs-lookup"><span data-stu-id="b47fb-222">The activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="b47fb-223">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b47fb-223">Failed</span></span></td><td><span data-ttu-id="b47fb-224">TimedOut</span><span class="sxs-lookup"><span data-stu-id="b47fb-224">TimedOut</span></span></td><td><span data-ttu-id="b47fb-225">Provedení aktivity trvalo déle, než je povolené aktivitou.</span><span class="sxs-lookup"><span data-stu-id="b47fb-225">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-226">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="b47fb-226">Canceled</span></span></td><td><span data-ttu-id="b47fb-227">Okno aktivity zrušil akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="b47fb-227">The activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-228">Ověření</span><span class="sxs-lookup"><span data-stu-id="b47fb-228">Validation</span></span></td><td><span data-ttu-id="b47fb-229">Ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="b47fb-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="b47fb-230">Okno aktivity se nepodařilo vygenerovat nebo ověřit.</span><span class="sxs-lookup"><span data-stu-id="b47fb-230">The activity window failed to be generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="b47fb-231">Připraveno</span><span class="sxs-lookup"><span data-stu-id="b47fb-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="b47fb-232">Okna aktivity je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="b47fb-232">The activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-233">Přeskočena</span><span class="sxs-lookup"><span data-stu-id="b47fb-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="b47fb-234">Okno aktivity nebyla zpracována.</span><span class="sxs-lookup"><span data-stu-id="b47fb-234">The activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b47fb-235">Žádný</span><span class="sxs-lookup"><span data-stu-id="b47fb-235">None</span></span></td><td>-</td><td><span data-ttu-id="b47fb-236">Okno s aktivity měl dříve jiný stav, ale byl obnoven.</span><span class="sxs-lookup"><span data-stu-id="b47fb-236">An activity window used to exist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="b47fb-237">Po kliknutí na tlačítko okno s aktivity v seznamu, můžete zobrazit podrobnosti o ho **aktivity Windows Explorer** nebo **vlastnosti** okno na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-237">When you click an activity window in the list, you see details about it in the **Activity Windows Explorer** or the **Properties** window on the right.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="b47fb-239">Aktualizujte windows aktivity</span><span class="sxs-lookup"><span data-stu-id="b47fb-239">Refresh activity windows</span></span>
<span data-ttu-id="b47fb-240">Podrobnosti nejsou automaticky aktualizují, proto použijte tlačítko Aktualizovat (druhé tlačítko) na panelu příkazů ručně aktualizovat seznam windows aktivit.</span><span class="sxs-lookup"><span data-stu-id="b47fb-240">The details aren't automatically refreshed, so use the refresh button (the second button) on the command bar to manually refresh the activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="b47fb-241">Vlastnosti – okno</span><span class="sxs-lookup"><span data-stu-id="b47fb-241">Properties window</span></span>
<span data-ttu-id="b47fb-242">Okno Vlastnosti je v podokně nejvíce vpravo monitorování a správu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b47fb-242">The Properties window is in the right-most pane of the Monitoring and Management app.</span></span>

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="b47fb-244">Zobrazí vlastnosti pro položku, kterou jste vybrali v Průzkumníku prostředků (stromové zobrazení), zobrazení diagramu nebo seznamu aktivity Windows.</span><span class="sxs-lookup"><span data-stu-id="b47fb-244">It displays properties for the item that you selected in the Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="b47fb-245">Okno Průzkumníka aktivity</span><span class="sxs-lookup"><span data-stu-id="b47fb-245">Activity Window Explorer</span></span>
<span data-ttu-id="b47fb-246">**Aktivity okno Průzkumníka** okno se v podokně nejvíce vpravo monitorování a správu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b47fb-246">The **Activity Window Explorer** window is in the right-most pane of the Monitoring and Management app.</span></span> <span data-ttu-id="b47fb-247">Zobrazuje podrobnosti o okně aktivity, které jste vybrali v místním okně aktivity Windows nebo seznamu okna aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-247">It displays details about the activity window that you selected in the Activity Windows pop-up window or the Activity Windows list.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="b47fb-249">Kliknutím na zobrazení kalendáře v horní části můžete přepnout do jiného okna aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-249">You can switch to another activity window by clicking it in the calendar view at the top.</span></span> <span data-ttu-id="b47fb-250">Také můžete šipku vlevo nebo vpravo tlačítek v horní části zobrazíte okna aktivity z předchozího týdne nebo do následujícího týdne.</span><span class="sxs-lookup"><span data-stu-id="b47fb-250">You can also use the left arrow/right arrow buttons at the top to see activity windows from the previous week or the next week.</span></span>

<span data-ttu-id="b47fb-251">Tlačítka panelu nástrojů v dolním podokně slouží k spustit znovu okna aktivity nebo aktualizovat podrobnosti v podokně.</span><span class="sxs-lookup"><span data-stu-id="b47fb-251">You can use the toolbar buttons in the bottom pane to rerun the activity window or refresh the details in the pane.</span></span>

### <a name="script"></a><span data-ttu-id="b47fb-252">Skript</span><span class="sxs-lookup"><span data-stu-id="b47fb-252">Script</span></span>
<span data-ttu-id="b47fb-253">Můžete použít **skriptu** zobrazíte definici JSON vybrané entity služby Data Factory (propojené služby, datové sady nebo kanál).</span><span class="sxs-lookup"><span data-stu-id="b47fb-253">You can use the **Script** tab to view the JSON definition of the selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="b47fb-255">Pomocí zobrazení systému</span><span class="sxs-lookup"><span data-stu-id="b47fb-255">Use system views</span></span>
<span data-ttu-id="b47fb-256">Monitorování a správu aplikace obsahuje předem připravené systémová zobrazení (**poslední aktivity windows**, **se nezdařilo aktivity windows**, **probíhající aktivity windows**), povolit můžete zobrazit posledních nebo se nezdařilo nebo probíhající aktivity windows pro datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-256">The Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you to view recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="b47fb-257">Přepnout **monitorování zobrazení** karty na levé straně kliknutím.</span><span class="sxs-lookup"><span data-stu-id="b47fb-257">Switch to the **Monitoring Views** tab on the left by clicking it.</span></span>

![Zobrazení Karta sledování](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="b47fb-259">V současné době jsou tři zobrazení systému, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b47fb-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="b47fb-260">Vyberte možnost Zobrazit nedávné aktivity windows, windows neúspěšné aktivity nebo okna probíhající aktivity v seznamu okna aktivity (v dolní části v prostředním podokně).</span><span class="sxs-lookup"><span data-stu-id="b47fb-260">Select an option to see recent activity windows, failed activity windows, or in-progress activity windows in the Activity Windows list (at the bottom of the middle pane).</span></span>

<span data-ttu-id="b47fb-261">Když vyberete **poslední aktivity windows** možnost zobrazí všechny poslední aktivity windows v sestupném pořadí podle **čas posledního pokusu**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-261">When you select the **Recent activity windows** option, you see all recent activity windows in descending order of the **last attempt time**.</span></span>

<span data-ttu-id="b47fb-262">Můžete použít **se nezdařilo aktivity windows** zobrazíte všechny neúspěšné aktivity windows v seznamu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-262">You can use the **Failed activity windows** view to see all failed activity windows in the list.</span></span> <span data-ttu-id="b47fb-263">Vyberte v seznamu zobrazíte podrobnosti o něm v období neúspěšné aktivity **vlastnosti** okno nebo **aktivity okno Průzkumníka**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-263">Select a failed activity window in the list to see details about it in the **Properties** window or the **Activity Window Explorer**.</span></span> <span data-ttu-id="b47fb-264">Můžete také stáhnout všechny protokoly pro okno neúspěšné aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="b47fb-265">Třídění a filtrování aktivity windows</span><span class="sxs-lookup"><span data-stu-id="b47fb-265">Sort and filter activity windows</span></span>
<span data-ttu-id="b47fb-266">Změna **počáteční čas** a **čas ukončení** nastavení na panelu příkazů do filtru aktivity windows.</span><span class="sxs-lookup"><span data-stu-id="b47fb-266">Change the **start time** and **end time** settings in the command bar to filter activity windows.</span></span> <span data-ttu-id="b47fb-267">Po změně počáteční čas a koncový čas, klikněte na tlačítko vedle koncový čas k aktualizaci seznamu okna aktivity.</span><span class="sxs-lookup"><span data-stu-id="b47fb-267">After you change the start time and end time, click the button next to the end time to refresh the Activity Windows list.</span></span>

![Počáteční a koncový čas](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="b47fb-269">V současné době všechny časy jsou ve formátu UTC v aplikaci sledování a správu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-269">Currently, all times are in UTC format in the Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="b47fb-270">V **seznamu aktivity Windows**, klikněte na název sloupce (například: stav).</span><span class="sxs-lookup"><span data-stu-id="b47fb-270">In the **Activity Windows list**, click the name of a column (for example: Status).</span></span>

![Aktivity Windows seznamu sloupec nabídky](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="b47fb-272">Můžete provést následující:</span><span class="sxs-lookup"><span data-stu-id="b47fb-272">You can do the following:</span></span>

* <span data-ttu-id="b47fb-273">Seřadit ve vzestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b47fb-273">Sort in ascending order.</span></span>
* <span data-ttu-id="b47fb-274">Řazení v sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b47fb-274">Sort in descending order.</span></span>
* <span data-ttu-id="b47fb-275">Filtrovat podle jednu nebo více hodnot (připravené, čekání a tak dále).</span><span class="sxs-lookup"><span data-stu-id="b47fb-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="b47fb-276">Když zadáte filtr na sloupci, zobrazí tlačítko filtru pro sloupce, který označuje, jestli jsou hodnoty ve sloupci filtrované hodnoty povoleno.</span><span class="sxs-lookup"><span data-stu-id="b47fb-276">When you specify a filter on a column, you see the filter button enabled for that column, which indicates that the values in the column are filtered values.</span></span>

![Filtrování podle sloupce seznamu okna aktivity](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="b47fb-278">Zrušení filtrů můžete stejné překryvné okno.</span><span class="sxs-lookup"><span data-stu-id="b47fb-278">You can use the same pop-up window to clear filters.</span></span> <span data-ttu-id="b47fb-279">Zrušení všech filtrů pro okna aktivity seznamu, klikněte na tlačítko Vymazat filtr na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b47fb-279">To clear all filters for the Activity Windows list, click the clear filter button on the command bar.</span></span>

![Zruší všechny filtry pro okna aktivity seznamu](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="b47fb-281">Udělejte batch</span><span class="sxs-lookup"><span data-stu-id="b47fb-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="b47fb-282">Znovu spustit vybranou aktivitou windows</span><span class="sxs-lookup"><span data-stu-id="b47fb-282">Rerun selected activity windows</span></span>
<span data-ttu-id="b47fb-283">Vyberte okno s aktivity, klikněte na šipku dolů pro první příkazového tlačítka panelu a vyberte **spusťte znovu** / **znovu spustit s proti proudu v kanálu**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-283">Select an activity window, click the down arrow for the first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="b47fb-284">Když vyberete **znovu spustit s proti proudu v kanálu** možnost, se znovu spustí všechna okna nadřízené činnosti také.</span><span class="sxs-lookup"><span data-stu-id="b47fb-284">When you select the **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="b47fb-285">![Spusťte okno s aktivity](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="b47fb-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="b47fb-286">Můžete také v seznamu vyberte více aktivity windows a znovu spustit, je ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-286">You can also select multiple activity windows in the list and rerun them at the same time.</span></span> <span data-ttu-id="b47fb-287">Můžete filtrovat na základě stavu okna aktivity (například: **se nezdařilo**) – a poté znovu spusťte windows neúspěšné aktivity po vyřešení potíží, které způsobí, že aktivita windows selhání.</span><span class="sxs-lookup"><span data-stu-id="b47fb-287">You might want to filter activity windows based on the status (for example: **Failed**)--and then rerun the failed activity windows after correcting the issue that causes the activity windows to fail.</span></span> <span data-ttu-id="b47fb-288">Následující části Podrobnosti o filtrování okna aktivity v seznamu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-288">See the following section for details about filtering activity windows in the list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="b47fb-289">Pozastavení nebo obnovení více kanálů</span><span class="sxs-lookup"><span data-stu-id="b47fb-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="b47fb-290">Vícenásobný výběr dva nebo víc kanálů můžete pomocí klávesu Ctrl.</span><span class="sxs-lookup"><span data-stu-id="b47fb-290">You can multiselect two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="b47fb-291">Pozastavení nebo obnovení je můžete panelu příkazů (které jsou vyznačené na červeným rámečkem na následujícím obrázku).</span><span class="sxs-lookup"><span data-stu-id="b47fb-291">You can use the command bar buttons (which are highlighted in the red rectangle in the following image) to pause/resume them.</span></span>

![Pozastavení nebo obnovení na panelu příkazů](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="b47fb-293">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="b47fb-293">Create alerts</span></span>
<span data-ttu-id="b47fb-294">**Výstrahy** stránky umožňuje vytvářet výstrahy a zobrazení, úpravy nebo odstranění existující výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b47fb-294">The **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="b47fb-295">Vám může také zapnout/vypnout výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="b47fb-296">Chcete-li zobrazit stránku výstrahy, klikněte na tlačítko **výstrahy** kartě.</span><span class="sxs-lookup"><span data-stu-id="b47fb-296">To see the Alerts page, click the **Alerts** tab.</span></span>

![Karta výstrahy](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a><span data-ttu-id="b47fb-298">Vytvoření výstrahy</span><span class="sxs-lookup"><span data-stu-id="b47fb-298">To create an alert</span></span>
1. <span data-ttu-id="b47fb-299">Klikněte na tlačítko **přidat výstraha** přidat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-299">Click **Add Alert** to add an alert.</span></span> <span data-ttu-id="b47fb-300">Zobrazí **podrobnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="b47fb-300">You see the **Details** page.</span></span>

    ![Vytvářet výstrahy - stránce s podrobnostmi o](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="b47fb-302">Zadejte **název** a **popis** pro výstrahy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-302">Specify the **Name** and **Description** for the alert, and click **Next**.</span></span> <span data-ttu-id="b47fb-303">Měli byste vidět **filtry** stránky.</span><span class="sxs-lookup"><span data-stu-id="b47fb-303">You should see the **Filters** page.</span></span>

    ![Vytvořte upozornění – filtry stránky](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="b47fb-305">Vyberte **událostí**, **stav**, a **substatus** (volitelné) Chcete-li vytvořit výstrahu pro službu Data Factory, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-305">Select the **event**, **status**, and **substatus** (optional) that you want to create a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="b47fb-306">Měli byste vidět **příjemce** stránky.</span><span class="sxs-lookup"><span data-stu-id="b47fb-306">You should see the **Recipients** page.</span></span>

    ![Vytvořte upozornění – stránka příjemce](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="b47fb-308">Vyberte **e-mailem správci předplatného** možnost a zadejte **další správce e-mailu**a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b47fb-308">Select the **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="b47fb-309">Měli byste vidět výstrahu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-309">You should see the alert in the list.</span></span>

    ![Seznam výstrah](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="b47fb-311">V seznamu výstrah pomocí tlačítek, které jsou přidruženy výstrahu, kterou chcete upravit nebo odstranit nebo zapnout/vypnout výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-311">In the Alerts list, use the buttons that are associated with the alert to edit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="b47fb-312">Události, stav/substatus</span><span class="sxs-lookup"><span data-stu-id="b47fb-312">Event/status/substatus</span></span>
<span data-ttu-id="b47fb-313">Následující tabulka obsahuje seznam dostupných událostí a stavy (a dílčí stavy).</span><span class="sxs-lookup"><span data-stu-id="b47fb-313">The following table provides the list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="b47fb-314">Název události</span><span class="sxs-lookup"><span data-stu-id="b47fb-314">Event name</span></span> | <span data-ttu-id="b47fb-315">Status</span><span class="sxs-lookup"><span data-stu-id="b47fb-315">Status</span></span> | <span data-ttu-id="b47fb-316">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="b47fb-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b47fb-317">Aktivity při spuštění Začínáme</span><span class="sxs-lookup"><span data-stu-id="b47fb-317">Activity Run Started</span></span> |<span data-ttu-id="b47fb-318">spuštění</span><span class="sxs-lookup"><span data-stu-id="b47fb-318">Started</span></span> |<span data-ttu-id="b47fb-319">Spouštění</span><span class="sxs-lookup"><span data-stu-id="b47fb-319">Starting</span></span> |
| <span data-ttu-id="b47fb-320">Aktivity při spuštění bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="b47fb-320">Activity Run Finished</span></span> |<span data-ttu-id="b47fb-321">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b47fb-321">Succeeded</span></span> |<span data-ttu-id="b47fb-322">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b47fb-322">Succeeded</span></span> |
| <span data-ttu-id="b47fb-323">Aktivity při spuštění bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="b47fb-323">Activity Run Finished</span></span> |<span data-ttu-id="b47fb-324">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b47fb-324">Failed</span></span> |<span data-ttu-id="b47fb-325">Přidělení prostředků se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b47fb-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="b47fb-326">Spuštění se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b47fb-326">Failed Execution</span></span><br/><br/><span data-ttu-id="b47fb-327">Vypršel časový limit</span><span class="sxs-lookup"><span data-stu-id="b47fb-327">Timed Out</span></span><br/><br/><span data-ttu-id="b47fb-328">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b47fb-328">Failed Validation</span></span><br/><br/><span data-ttu-id="b47fb-329">opuštění</span><span class="sxs-lookup"><span data-stu-id="b47fb-329">Abandoned</span></span> |
| <span data-ttu-id="b47fb-330">Vytvoření clusteru HDI na vyžádání Začínáme</span><span class="sxs-lookup"><span data-stu-id="b47fb-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="b47fb-331">spuštění</span><span class="sxs-lookup"><span data-stu-id="b47fb-331">Started</span></span> |-|
| <span data-ttu-id="b47fb-332">Clusteru HDI na vyžádání úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="b47fb-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="b47fb-333">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b47fb-333">Succeeded</span></span> |-|
| <span data-ttu-id="b47fb-334">Odstranit clusteru HDI na vyžádání</span><span class="sxs-lookup"><span data-stu-id="b47fb-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="b47fb-335">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b47fb-335">Succeeded</span></span> |-|

### <a name="to-edit-delete-or-disable-an-alert"></a><span data-ttu-id="b47fb-336">Chcete-li upravit, odstranit nebo zakázat výstrahy</span><span class="sxs-lookup"><span data-stu-id="b47fb-336">To edit, delete, or disable an alert</span></span>

<span data-ttu-id="b47fb-337">Pomocí následujících tlačítek (zvýrazněné červeně) upravit, odstranit nebo zakázat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b47fb-337">Use the following buttons (highlighted in red) to edit, delete, or disable an alert.</span></span>

![Výstrahy tlačítka](./media/data-factory-monitor-manage-app/AlertButtons.png)
