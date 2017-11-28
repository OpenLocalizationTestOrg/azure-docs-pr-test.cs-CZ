---
title: "aaaMonitor a Správa kanálů data - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello monitorování a správu toomonitor aplikace a správa Azure data Factory a kanály."
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
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a><span data-ttu-id="97908-103">Monitorování a Správa kanálů služby Azure Data Factory pomocí monitorování a správu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="97908-103">Monitor and manage Azure Data Factory pipelines by using hello Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="97908-104">Použití Azure portal nebo Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="97908-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="97908-105">Pomocí monitorování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="97908-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="97908-106">Tento článek popisuje, jak toouse hello monitorování a správu toomonitor aplikace, spravovat a ladit kanály Data Factory.</span><span class="sxs-lookup"><span data-stu-id="97908-106">This article describes how toouse hello Monitoring and Management app toomonitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="97908-107">Nabízí taky informace o tom, jak toocreate výstrahy tooget upozornění na selhání.</span><span class="sxs-lookup"><span data-stu-id="97908-107">It also provides information on how toocreate alerts tooget notified on failures.</span></span> <span data-ttu-id="97908-108">Můžete začít s pomocí aplikace hello podle sledováním hello následující video:</span><span class="sxs-lookup"><span data-stu-id="97908-108">You can get started with using hello application by watching hello following video:</span></span>

> [!NOTE]
> <span data-ttu-id="97908-109">uživatelské rozhraní Hello ukazuje hello video nemusí přesně odpovídat najdete v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-109">hello user interface shown in hello video may not exactly match what you see in hello portal.</span></span> <span data-ttu-id="97908-110">Je mírně starší, ale koncepty zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="97908-110">It's slightly older, but concepts remain hello same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a><span data-ttu-id="97908-111">Spustit sledování a správu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="97908-111">Launch hello Monitoring and Management app</span></span>
<span data-ttu-id="97908-112">toolaunch hello monitorování a správu aplikací, klikněte na tlačítko hello **monitorování a správa** na hello dlaždici **Data Factory** okno objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="97908-112">toolaunch hello Monitor and Management app, click hello **Monitor & Manage** tile on hello **Data Factory** blade for your data factory.</span></span>

![Monitorování dlaždice na domovské stránce objektu pro vytváření dat hello](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="97908-114">Měli byste vidět monitorování a správu aplikace hello otevře v samostatném okně.</span><span class="sxs-lookup"><span data-stu-id="97908-114">You should see hello Monitoring and Management app open in a separate window.</span></span>  

![Monitorování a správa aplikací](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="97908-116">Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zrušte hello **blokovat soubory cookie třetích stran a data lokality** políčko--nebo udržování je vybrána, vytvořte výjimku pro **login.microsoftonline.com** a akci opakujte tooopen hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="97908-116">If you see that hello web browser is stuck at "Authorizing...", clear hello **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try tooopen hello app again.</span></span>


<span data-ttu-id="97908-117">V seznamu aktivity Windows hello v prostředním podokně hello zobrazí okno s aktivity pro každé spuštění aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-117">In hello Activity Windows list in hello middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="97908-118">Například pokud máte pět hodin hello naplánované aktivity toorun každou hodinu, uvidíte pět okna aktivity spojené s pěti datové řezy.</span><span class="sxs-lookup"><span data-stu-id="97908-118">For example, if you have hello activity scheduled toorun hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="97908-119">Pokud nevidíte aktivity windows hello seznamu dole v hello, hello následující:</span><span class="sxs-lookup"><span data-stu-id="97908-119">If you don't see activity windows in hello list at hello bottom, do hello following:</span></span>
 
- <span data-ttu-id="97908-120">Aktualizace hello **počáteční čas** a **čas ukončení** filtrů v horní toomatch hello hello spuštění a ukončení vašeho kanálu a klikněte hello **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97908-120">Update hello **start time** and **end time** filters at hello top toomatch hello start and end times of your pipeline, and then click hello **Apply** button.</span></span>  
- <span data-ttu-id="97908-121">seznam aktivity Windows Hello automaticky neobnoví.</span><span class="sxs-lookup"><span data-stu-id="97908-121">hello Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="97908-122">Klikněte na tlačítko hello **aktualizovat** tlačítka na panelu nástrojů hello v hello **aktivity Windows** seznamu.</span><span class="sxs-lookup"><span data-stu-id="97908-122">Click hello **Refresh** button on hello toolbar in hello **Activity Windows** list.</span></span>  

<span data-ttu-id="97908-123">Pokud nemáte tootest aplikace Data Factory tyto kroky hello kurz: [kopírování dat z úložiště objektů Blob tooSQL databázi pomocí služby Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="97908-123">If you don't have a Data Factory application tootest these steps with, do hello tutorial: [copy data from Blob Storage tooSQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-hello-monitoring-and-management-app"></a><span data-ttu-id="97908-124">Pochopení hello monitorování a Správa aplikací</span><span class="sxs-lookup"><span data-stu-id="97908-124">Understand hello Monitoring and Management app</span></span>
<span data-ttu-id="97908-125">Na levé straně hello jsou tři karty: **Průzkumníka prostředků**, **monitorování zobrazení**, a **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="97908-125">There are three tabs on hello left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="97908-126">první kartě Hello (**Průzkumníka prostředků**) je standardně vybraná.</span><span class="sxs-lookup"><span data-stu-id="97908-126">hello first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="97908-127">Průzkumník prostředků</span><span class="sxs-lookup"><span data-stu-id="97908-127">Resource Explorer</span></span>
<span data-ttu-id="97908-128">Zobrazí hello následující:</span><span class="sxs-lookup"><span data-stu-id="97908-128">You see hello following:</span></span>

* <span data-ttu-id="97908-129">Hello Průzkumníka prostředků **stromovém zobrazení** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-129">hello Resource Explorer **tree view** in hello left pane.</span></span>
* <span data-ttu-id="97908-130">Hello **zobrazení diagramu** v horní části hello v prostředním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-130">hello **Diagram View** at hello top in hello middle pane.</span></span>
* <span data-ttu-id="97908-131">Hello **aktivity Windows** seznam v dolní části hello v prostředním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-131">hello **Activity Windows** list at hello bottom in hello middle pane.</span></span>
* <span data-ttu-id="97908-132">Hello **vlastnosti**, **aktivity okno Průzkumníka**, a **skriptu** karty v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-132">hello **Properties**, **Activity Window Explorer**, and **Script** tabs in hello right pane.</span></span>

<span data-ttu-id="97908-133">V Průzkumníku prostředků zobrazí všechny prostředky (kanály, datové sady, propojených služeb) v objektu pro vytváření dat hello ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="97908-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in hello data factory in a tree view.</span></span> <span data-ttu-id="97908-134">Když vyberete objekt v Průzkumníku prostředků:</span><span class="sxs-lookup"><span data-stu-id="97908-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="97908-135">Hello související objekt pro vytváření dat entity je označený na hello zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="97908-135">hello associated Data Factory entity is highlighted in hello Diagram View.</span></span>
* <span data-ttu-id="97908-136">[Související aktivity windows](data-factory-scheduling-and-execution.md) jsou vyznačené na seznamu aktivity Windows hello dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in hello Activity Windows list at hello bottom.</span></span>  
* <span data-ttu-id="97908-137">Hello vlastnosti hello vybraný objekt jsou zobrazeny v okně Vlastnosti hello v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-137">hello properties of hello selected object are shown in hello Properties window in hello right pane.</span></span>
* <span data-ttu-id="97908-138">Hello definici JSON hello vybraného objektu se zobrazí, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="97908-138">hello JSON definition of hello selected object is shown, if applicable.</span></span> <span data-ttu-id="97908-139">Příklad: propojené služby, datové sady nebo kanálu.</span><span class="sxs-lookup"><span data-stu-id="97908-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Průzkumník prostředků](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="97908-141">V tématu hello [plánování a provádění](data-factory-scheduling-and-execution.md) článku podrobné koncepční informace o windows aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-141">See hello [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="97908-142">Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="97908-142">Diagram View</span></span>
<span data-ttu-id="97908-143">Hello zobrazení diagramu objektu pro vytváření dat poskytuje jedno podokno pohotovostní toomonitor a spravovat objekt pro vytváření dat a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="97908-143">hello Diagram View of a data factory provides a single pane of glass toomonitor and manage a data factory and its assets.</span></span> <span data-ttu-id="97908-144">Když vyberete entity služby Data Factory (datové sady nebo kanál) v hello zobrazení diagramu:</span><span class="sxs-lookup"><span data-stu-id="97908-144">When you select a Data Factory entity (dataset/pipeline) in hello Diagram View:</span></span>

* <span data-ttu-id="97908-145">v zobrazení stromu hello je vybrána entity objektu pro vytváření dat Hello.</span><span class="sxs-lookup"><span data-stu-id="97908-145">hello data factory entity is selected in hello tree view.</span></span>
* <span data-ttu-id="97908-146">Hello související aktivity, které windows jsou vyznačené na seznamu aktivity Windows hello.</span><span class="sxs-lookup"><span data-stu-id="97908-146">hello associated activity windows are highlighted in hello Activity Windows list.</span></span>
* <span data-ttu-id="97908-147">Hello vlastnosti vybraného objektu hello se zobrazí v okně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="97908-147">hello properties of hello selected object are shown in hello Properties window.</span></span>

<span data-ttu-id="97908-148">Když hello kanál je povolené (ne v pozastaveném stavu), zobrazí se zelený řádku:</span><span class="sxs-lookup"><span data-stu-id="97908-148">When hello pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![Spuštění kanálu](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="97908-150">Můžete pozastavit, obnovit nebo ukončit kanál tak, že ho vyberete v zobrazení diagramu hello a použití hello tlačítek na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="97908-150">You can pause, resume, or terminate a pipeline by selecting it in hello diagram view and using hello buttons on hello command bar.</span></span>

![Pozastavení nebo obnovení na panelu příkazů hello](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="97908-152">Existují tři panelu příkazů pro kanál hello v hello zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="97908-152">There are three command bar buttons for hello pipeline in hello Diagram View.</span></span> <span data-ttu-id="97908-153">Můžete použít hello druhý tlačítko toopause hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="97908-153">You can use hello second button toopause hello pipeline.</span></span> <span data-ttu-id="97908-154">Pozastavení není ukončení aktuálně spuštěných aktivity hello a umožňuje jim pokračovat toocompletion.</span><span class="sxs-lookup"><span data-stu-id="97908-154">Pausing doesn't terminate hello currently running activities and lets them proceed toocompletion.</span></span> <span data-ttu-id="97908-155">třetí tlačítko Hello pozastaví hello kanálu a ukončí její existující provádění aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-155">hello third button pauses hello pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="97908-156">první tlačítko Hello obnoví hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="97908-156">hello first button resumes hello pipeline.</span></span> <span data-ttu-id="97908-157">Pokud svůj kanál je pozastavena, změní barvu hello hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="97908-157">When your pipeline is paused, hello color of hello pipeline changes.</span></span> <span data-ttu-id="97908-158">Například pozastavený kanálu vypadá v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="97908-158">For example, a paused pipeline looks like in hello following image:</span></span> 

![Pozastavená kanálu](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="97908-160">Pomocí klávesy Ctrl hello můžete vybrat víc dva nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="97908-160">You can multi-select two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="97908-161">Příkaz hello panelu tlačítka toopause/obnovení můžete použít více kanálů najednou.</span><span class="sxs-lookup"><span data-stu-id="97908-161">You can use hello command bar buttons toopause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="97908-162">Můžete také kliknout pravým tlačítkem kanálu a zvolit možnosti toosuspend obnovit nebo ukončit kanál.</span><span class="sxs-lookup"><span data-stu-id="97908-162">You can also right-click a pipeline and select options toosuspend, resume, or terminate a pipeline.</span></span> 

![Kontextové nabídky pro kanál](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="97908-164">Klikněte na tlačítko hello **otevřít kanál** možnost toosee všechny hello aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-164">Click hello **Open pipeline** option toosee all hello activities in hello pipeline.</span></span> 

![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="97908-166">V zobrazení hello otevřít kanál zobrazí všechny aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-166">In hello opened pipeline view, you see all activities in hello pipeline.</span></span> <span data-ttu-id="97908-167">V tomto příkladu je jenom jedna aktivita: aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="97908-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Otevřenou kanálu](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="97908-169">toogo zpět toohello předchozího zobrazení, klikněte na název objektu pro vytváření dat hello v nabídce hello s popisem cesty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="97908-169">toogo back toohello previous view, click hello data factory name in hello breadcrumb menu at hello top.</span></span>

<span data-ttu-id="97908-170">V zobrazení kanálu hello Pokud vyberete datovou sadu výstupů nebo když přesunutím ukazatele myši nad hello výstupní datovou sadu, zobrazí místní okno aktivity Windows hello pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="97908-170">In hello pipeline view, when you select an output dataset or when you move your mouse over hello output dataset, you see hello Activity Windows pop-up window for that dataset.</span></span>

![Místní okno Windows aktivity](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="97908-172">Klepnutím podrobnosti o aktivitě okno toosee pro něj v hello **vlastnosti** okno v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-172">You can click an activity window toosee details for it in hello **Properties** window in hello right pane.</span></span>

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="97908-174">V pravém podokně hello přepínač toohello **aktivity okno Průzkumníka** kartě toosee další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="97908-174">In hello right pane, switch toohello **Activity Window Explorer** tab toosee more details.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="97908-176">Zobrazí také **přeložit proměnné** pro každý pokus o spuštění pro aktivitu v hello **pokusy o** části.</span><span class="sxs-lookup"><span data-stu-id="97908-176">You also see **resolved variables** for each run attempt for an activity in hello **Attempts** section.</span></span>

![Vyřešený proměnné](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="97908-178">Přepínač toohello **skriptu** kartě toosee hello JSON skriptu definice pro vybraný objekt hello.</span><span class="sxs-lookup"><span data-stu-id="97908-178">Switch toohello **Script** tab toosee hello JSON script definition for hello selected object.</span></span>   

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="97908-180">Zobrazí okna aktivity na třech místech:</span><span class="sxs-lookup"><span data-stu-id="97908-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="97908-181">Hello aktivity Windows automaticky otevírané okno v hello zobrazení diagramu (střední podokno).</span><span class="sxs-lookup"><span data-stu-id="97908-181">hello Activity Windows pop-up in hello Diagram View (middle pane).</span></span>
* <span data-ttu-id="97908-182">v pravém podokně hello Hello aktivity okno Průzkumníka.</span><span class="sxs-lookup"><span data-stu-id="97908-182">hello Activity Window Explorer in hello right pane.</span></span>
* <span data-ttu-id="97908-183">seznam aktivity Windows Hello v dolním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-183">hello Activity Windows list in hello bottom pane.</span></span>

<span data-ttu-id="97908-184">V místní nabídce Aktivita Windows hello a aktivity okno Průzkumníka se toohello posunete předchozího týdne a hello příští týden pomocí hello levou a pravou šipku.</span><span class="sxs-lookup"><span data-stu-id="97908-184">In hello Activity Windows pop-up and Activity Window Explorer, you can scroll toohello previous week and hello next week by using hello left and right arrows.</span></span>

![Aktivita okno Průzkumníka levé nebo pravé šipky](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="97908-186">Na hello dolní části hello zobrazení diagramu, zobrazí tato tlačítka: Přiblížit, oddálit, tooFit přiblížení či oddálení, zvětšení 100 %, rozložení zámku.</span><span class="sxs-lookup"><span data-stu-id="97908-186">At hello bottom of hello Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom tooFit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="97908-187">Hello **zámku rozložení** tlačítko zabraňuje nechtěnému přesunutí tabulek a kanálů v hello zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="97908-187">hello **Lock layout** button prevents you from accidentally moving tables and pipelines in hello Diagram View.</span></span> <span data-ttu-id="97908-188">Ve výchozím nastavení je.</span><span class="sxs-lookup"><span data-stu-id="97908-188">It's on by default.</span></span> <span data-ttu-id="97908-189">Můžete ho vypnout a pohyb entity v diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-189">You can turn it off and move entities around in hello diagram.</span></span> <span data-ttu-id="97908-190">Pokud funkci vypnete, můžete použít hello poslední tlačítko tooautomatically pozice tabulky a kanály.</span><span class="sxs-lookup"><span data-stu-id="97908-190">When you turn it off, you can use hello last button tooautomatically position tables and pipelines.</span></span> <span data-ttu-id="97908-191">Můžete také přiblížení nebo oddálení pomocí kolečka myši hello.</span><span class="sxs-lookup"><span data-stu-id="97908-191">You can also zoom in or out by using hello mouse wheel.</span></span>

![Diagram příkazy přiblížení zobrazení](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="97908-193">Seznam aktivit Windows</span><span class="sxs-lookup"><span data-stu-id="97908-193">Activity Windows list</span></span>
<span data-ttu-id="97908-194">Zobrazí se seznam aktivity Windows Hello dole hello v prostředním podokně hello všechny aktivity windows hello datové sady, který jste vybrali v hello Průzkumníka prostředků nebo hello zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="97908-194">hello Activity Windows list at hello bottom of hello middle pane displays all activity windows for hello dataset that you selected in hello Resource Explorer or hello Diagram View.</span></span> <span data-ttu-id="97908-195">Ve výchozím nastavení seznam hello je v sestupném pořadí, což znamená, najdete v části hello nejnovější okně aktivita v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="97908-195">By default, hello list is in descending order, which means that you see hello latest activity window at hello top.</span></span>

![Seznam aktivit Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="97908-197">Tento seznam neobnovuje automaticky, takže jej aktualizovat, použijte tlačítko Aktualizovat hello na panelu nástrojů toomanually hello.</span><span class="sxs-lookup"><span data-stu-id="97908-197">This list doesn't refresh automatically, so use hello refresh button on hello toolbar toomanually refresh it.</span></span>  

<span data-ttu-id="97908-198">Okna aktivity může být v jednom z hello následující stavy:</span><span class="sxs-lookup"><span data-stu-id="97908-198">Activity windows can be in one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="97908-199">Status</span><span class="sxs-lookup"><span data-stu-id="97908-199">Status</span></span></th><th align="left"><span data-ttu-id="97908-200">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="97908-200">Substatus</span></span></th><th align="left"><span data-ttu-id="97908-201">Popis</span><span class="sxs-lookup"><span data-stu-id="97908-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="97908-202">Čekání</span><span class="sxs-lookup"><span data-stu-id="97908-202">Waiting</span></span></td><td><span data-ttu-id="97908-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="97908-203">ScheduleTime</span></span></td><td><span data-ttu-id="97908-204">pro hello aktivity okno toorun ještě nenastal čas Hello.</span><span class="sxs-lookup"><span data-stu-id="97908-204">hello time hasn't come for hello activity window toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="97908-205">DatasetDependencies</span></span></td><td><span data-ttu-id="97908-206">Hello upstreamové závislosti nejsou připravené.</span><span class="sxs-lookup"><span data-stu-id="97908-206">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="97908-207">ComputeResources</span></span></td><td><span data-ttu-id="97908-208">Hello výpočetní prostředky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="97908-208">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="97908-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="97908-210">Všechny instance aktivit hello je zaneprázdněn spouštěním další aktivity windows.</span><span class="sxs-lookup"><span data-stu-id="97908-210">All hello activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="97908-211">ActivityResume</span></span></td><td><span data-ttu-id="97908-212">Hello aktivita je pozastavená a aktivity windows hello nelze spustit, dokud nebude obnovená.</span><span class="sxs-lookup"><span data-stu-id="97908-212">hello activity is paused and can't run hello activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-213">Opakování</span><span class="sxs-lookup"><span data-stu-id="97908-213">Retry</span></span></td><td><span data-ttu-id="97908-214">Probíhá pokus o spuštění aktivity Hello je zopakován.</span><span class="sxs-lookup"><span data-stu-id="97908-214">hello activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-215">Ověření</span><span class="sxs-lookup"><span data-stu-id="97908-215">Validation</span></span></td><td><span data-ttu-id="97908-216">Ověření se ještě nespustilo.</span><span class="sxs-lookup"><span data-stu-id="97908-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="97908-217">ValidationRetry</span></span></td><td><span data-ttu-id="97908-218">Ověření je čekání toobe opakovat.</span><span class="sxs-lookup"><span data-stu-id="97908-218">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="97908-219">InProgress</span><span class="sxs-lookup"><span data-stu-id="97908-219">InProgress</span></span></td><td><span data-ttu-id="97908-220">Probíhá ověřování</span><span class="sxs-lookup"><span data-stu-id="97908-220">Validating</span></span></td><td><span data-ttu-id="97908-221">Probíhá ověřování.</span><span class="sxs-lookup"><span data-stu-id="97908-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="97908-222">okno aktivity Hello je zpracovávána.</span><span class="sxs-lookup"><span data-stu-id="97908-222">hello activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="97908-223">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="97908-223">Failed</span></span></td><td><span data-ttu-id="97908-224">TimedOut</span><span class="sxs-lookup"><span data-stu-id="97908-224">TimedOut</span></span></td><td><span data-ttu-id="97908-225">provedení aktivity Hello trvalo déle, než je povolené aktivitou hello.</span><span class="sxs-lookup"><span data-stu-id="97908-225">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-226">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="97908-226">Canceled</span></span></td><td><span data-ttu-id="97908-227">okno aktivity Hello zrušil akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="97908-227">hello activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-228">Ověření</span><span class="sxs-lookup"><span data-stu-id="97908-228">Validation</span></span></td><td><span data-ttu-id="97908-229">Ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="97908-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="97908-230">Hello aktivity okna se nezdařila toobe generování nebo ověřování.</span><span class="sxs-lookup"><span data-stu-id="97908-230">hello activity window failed toobe generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="97908-231">Připraveno</span><span class="sxs-lookup"><span data-stu-id="97908-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="97908-232">okno aktivity Hello je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="97908-232">hello activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-233">Přeskočena</span><span class="sxs-lookup"><span data-stu-id="97908-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="97908-234">okno aktivity Hello nebyla zpracována.</span><span class="sxs-lookup"><span data-stu-id="97908-234">hello activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="97908-235">Žádný</span><span class="sxs-lookup"><span data-stu-id="97908-235">None</span></span></td><td>-</td><td><span data-ttu-id="97908-236">Okno s aktivita používá tooexist jiný stav, ale byl obnoven.</span><span class="sxs-lookup"><span data-stu-id="97908-236">An activity window used tooexist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="97908-237">Po kliknutí na tlačítko okno s aktivity v seznamu hello, můžete zobrazit podrobnosti o ho v hello **aktivity Windows Explorer** nebo hello **vlastnosti** okno na hello správné.</span><span class="sxs-lookup"><span data-stu-id="97908-237">When you click an activity window in hello list, you see details about it in hello **Activity Windows Explorer** or hello **Properties** window on hello right.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="97908-239">Aktualizujte windows aktivity</span><span class="sxs-lookup"><span data-stu-id="97908-239">Refresh activity windows</span></span>
<span data-ttu-id="97908-240">Podrobnosti Hello se neobnovily automaticky, proto použijte tlačítko Aktualizovat hello (hello druhé tlačítko) na hello příkazovém řádku seznam windows toomanually aktualizace hello aktivit.</span><span class="sxs-lookup"><span data-stu-id="97908-240">hello details aren't automatically refreshed, so use hello refresh button (hello second button) on hello command bar toomanually refresh hello activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="97908-241">Vlastnosti – okno</span><span class="sxs-lookup"><span data-stu-id="97908-241">Properties window</span></span>
<span data-ttu-id="97908-242">v podokně nejvíce vpravo hello hello monitorování a správu aplikace je Hello vlastnosti – okno.</span><span class="sxs-lookup"><span data-stu-id="97908-242">hello Properties window is in hello right-most pane of hello Monitoring and Management app.</span></span>

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="97908-244">Zobrazí vlastnosti pro hello položku, kterou jste vybrali v hello Průzkumníka prostředků (stromové zobrazení), zobrazení diagramu nebo seznamu aktivity Windows.</span><span class="sxs-lookup"><span data-stu-id="97908-244">It displays properties for hello item that you selected in hello Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="97908-245">Okno Průzkumníka aktivity</span><span class="sxs-lookup"><span data-stu-id="97908-245">Activity Window Explorer</span></span>
<span data-ttu-id="97908-246">Hello **aktivity okno Průzkumníka** je okno hello pravé podokno hello monitorování a správu aplikace.</span><span class="sxs-lookup"><span data-stu-id="97908-246">hello **Activity Window Explorer** window is in hello right-most pane of hello Monitoring and Management app.</span></span> <span data-ttu-id="97908-247">Zobrazuje podrobnosti o okně hello aktivity, který jste vybrali v místním okně aktivity Windows hello nebo seznamu aktivity Windows hello.</span><span class="sxs-lookup"><span data-stu-id="97908-247">It displays details about hello activity window that you selected in hello Activity Windows pop-up window or hello Activity Windows list.</span></span>

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="97908-249">Kliknutím na zobrazení kalendáře hello v horní části hello můžete přepnout tooanother aktivity okno.</span><span class="sxs-lookup"><span data-stu-id="97908-249">You can switch tooanother activity window by clicking it in hello calendar view at hello top.</span></span> <span data-ttu-id="97908-250">Můžete také pomocí tlačítek hello šipku vlevo nebo vpravo v hello nejvyšší toosee aktivity windows z hello předchozího týdne nebo hello příští týden.</span><span class="sxs-lookup"><span data-stu-id="97908-250">You can also use hello left arrow/right arrow buttons at hello top toosee activity windows from hello previous week or hello next week.</span></span>

<span data-ttu-id="97908-251">Můžete použít hello tlačítka panelu nástrojů v okně aktivita hello dolní podokno toorerun hello nebo aktualizovat hello podrobnosti v podokně hello.</span><span class="sxs-lookup"><span data-stu-id="97908-251">You can use hello toolbar buttons in hello bottom pane toorerun hello activity window or refresh hello details in hello pane.</span></span>

### <a name="script"></a><span data-ttu-id="97908-252">Skript</span><span class="sxs-lookup"><span data-stu-id="97908-252">Script</span></span>
<span data-ttu-id="97908-253">Můžete použít hello **skriptu** kartě tooview hello JSON definice hello vybrané entity služby Data Factory (propojené služby, datové sady nebo kanál).</span><span class="sxs-lookup"><span data-stu-id="97908-253">You can use hello **Script** tab tooview hello JSON definition of hello selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="97908-255">Pomocí zobrazení systému</span><span class="sxs-lookup"><span data-stu-id="97908-255">Use system views</span></span>
<span data-ttu-id="97908-256">Hello monitorování a správu aplikací obsahuje zobrazení předdefinovaných systému (**poslední aktivity windows**, **se nezdařilo aktivity windows**, **probíhající aktivity windows**), umožní tooview poslední nebo se nezdařilo nebo probíhající aktivity windows pro datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="97908-256">hello Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you tooview recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="97908-257">Přepínač toohello **monitorování zobrazení** karty na levé straně hello kliknutím.</span><span class="sxs-lookup"><span data-stu-id="97908-257">Switch toohello **Monitoring Views** tab on hello left by clicking it.</span></span>

![Zobrazení Karta sledování](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="97908-259">V současné době jsou tři zobrazení systému, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="97908-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="97908-260">Vyberte toosee možnost poslední aktivity windows, windows neúspěšné aktivity nebo okna probíhající aktivity v seznamu aktivity Windows hello (dole hello prostředním podokně hello).</span><span class="sxs-lookup"><span data-stu-id="97908-260">Select an option toosee recent activity windows, failed activity windows, or in-progress activity windows in hello Activity Windows list (at hello bottom of hello middle pane).</span></span>

<span data-ttu-id="97908-261">Když vyberete hello **poslední aktivity windows** možnost zobrazí všechny poslední aktivity windows v sestupném pořadí podle hello **čas posledního pokusu**.</span><span class="sxs-lookup"><span data-stu-id="97908-261">When you select hello **Recent activity windows** option, you see all recent activity windows in descending order of hello **last attempt time**.</span></span>

<span data-ttu-id="97908-262">Můžete použít hello **se nezdařilo aktivity windows** zobrazit v seznamu hello okna aktivity toosee se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="97908-262">You can use hello **Failed activity windows** view toosee all failed activity windows in hello list.</span></span> <span data-ttu-id="97908-263">Vyberte časové období neúspěšné aktivity v hello seznamu toosee jeho podrobnosti v hello **vlastnosti** okno nebo hello **aktivity okno Průzkumníka**.</span><span class="sxs-lookup"><span data-stu-id="97908-263">Select a failed activity window in hello list toosee details about it in hello **Properties** window or hello **Activity Window Explorer**.</span></span> <span data-ttu-id="97908-264">Můžete také stáhnout všechny protokoly pro okno neúspěšné aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="97908-265">Třídění a filtrování aktivity windows</span><span class="sxs-lookup"><span data-stu-id="97908-265">Sort and filter activity windows</span></span>
<span data-ttu-id="97908-266">Změna hello **počáteční čas** a **čas ukončení** nastavení v hello příkazovém řádku windows toofilter aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-266">Change hello **start time** and **end time** settings in hello command bar toofilter activity windows.</span></span> <span data-ttu-id="97908-267">Po změně hello počáteční a koncový čas, klikněte na tlačítko hello tlačítko Další toohello koncový čas toorefresh hello aktivity seznamu Windows.</span><span class="sxs-lookup"><span data-stu-id="97908-267">After you change hello start time and end time, click hello button next toohello end time toorefresh hello Activity Windows list.</span></span>

![Počáteční a koncový čas](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="97908-269">V současné době všechny časy jsou ve formátu UTC hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="97908-269">Currently, all times are in UTC format in hello Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="97908-270">V hello **seznamu aktivity Windows**, klikněte na název sloupce hello (například: stav).</span><span class="sxs-lookup"><span data-stu-id="97908-270">In hello **Activity Windows list**, click hello name of a column (for example: Status).</span></span>

![Aktivity Windows seznamu sloupec nabídky](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="97908-272">Můžete provést následující hello:</span><span class="sxs-lookup"><span data-stu-id="97908-272">You can do hello following:</span></span>

* <span data-ttu-id="97908-273">Seřadit ve vzestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="97908-273">Sort in ascending order.</span></span>
* <span data-ttu-id="97908-274">Řazení v sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="97908-274">Sort in descending order.</span></span>
* <span data-ttu-id="97908-275">Filtrovat podle jednu nebo více hodnot (připravené, čekání a tak dále).</span><span class="sxs-lookup"><span data-stu-id="97908-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="97908-276">Když zadáte filtr na sloupci, zobrazí tlačítko Filtrovat hello povolené pro daný sloupec, který označuje, že hello hodnoty ve sloupci hello jsou filtrované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="97908-276">When you specify a filter on a column, you see hello filter button enabled for that column, which indicates that hello values in hello column are filtered values.</span></span>

![Filtrování podle sloupce seznamu aktivity Windows hello](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="97908-278">Můžete použít hello stejné filtry tooclear automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="97908-278">You can use hello same pop-up window tooclear filters.</span></span> <span data-ttu-id="97908-279">tooclear všechny filtry pro seznamu aktivity Windows hello, klikněte na tlačítko Vymazat filtr hello na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="97908-279">tooclear all filters for hello Activity Windows list, click hello clear filter button on hello command bar.</span></span>

![Zruší všechny filtry pro seznam aktivity Windows hello](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="97908-281">Udělejte batch</span><span class="sxs-lookup"><span data-stu-id="97908-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="97908-282">Znovu spustit vybranou aktivitou windows</span><span class="sxs-lookup"><span data-stu-id="97908-282">Rerun selected activity windows</span></span>
<span data-ttu-id="97908-283">Vyberte okno s aktivity, klikněte na tlačítko hello hello první příkazového řádku tlačítka šipka dolů a vyberte **spusťte znovu** / **znovu spustit s proti proudu v kanálu**.</span><span class="sxs-lookup"><span data-stu-id="97908-283">Select an activity window, click hello down arrow for hello first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="97908-284">Když vyberete hello **znovu spustit s proti proudu v kanálu** možnost, se znovu spustí všechna okna nadřízené činnosti také.</span><span class="sxs-lookup"><span data-stu-id="97908-284">When you select hello **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="97908-285">![Spusťte okno s aktivity](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="97908-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="97908-286">Můžete také vybrat více aktivity windows hello seznamu a znovu je v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="97908-286">You can also select multiple activity windows in hello list and rerun them at hello same time.</span></span> <span data-ttu-id="97908-287">Můžete chtít windows toofilter aktivity na základě stavu hello (například: **se nezdařilo**) – a poté znovu spusťte windows hello se nezdařilo aktivity po vyřešení potíží hello, který způsobuje, že toofail windows hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="97908-287">You might want toofilter activity windows based on hello status (for example: **Failed**)--and then rerun hello failed activity windows after correcting hello issue that causes hello activity windows toofail.</span></span> <span data-ttu-id="97908-288">Najdete v následující části Podrobnosti o filtrování aktivity windows hello seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-288">See hello following section for details about filtering activity windows in hello list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="97908-289">Pozastavení nebo obnovení více kanálů</span><span class="sxs-lookup"><span data-stu-id="97908-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="97908-290">Pomocí klávesy Ctrl hello můžete vícenásobného výběru dva nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="97908-290">You can multiselect two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="97908-291">Můžete použít tlačítka na panelu příkazů hello (které jsou vyznačené na hello red obdélníku v hello následující obrázek) toopause nebo obnovení je.</span><span class="sxs-lookup"><span data-stu-id="97908-291">You can use hello command bar buttons (which are highlighted in hello red rectangle in hello following image) toopause/resume them.</span></span>

![Pozastavení nebo obnovení na panelu příkazů hello](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="97908-293">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="97908-293">Create alerts</span></span>
<span data-ttu-id="97908-294">Hello **výstrahy** stránky umožňuje vytvářet výstrahy a zobrazení, úpravy nebo odstranění existující výstrahy.</span><span class="sxs-lookup"><span data-stu-id="97908-294">hello **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="97908-295">Vám může také zapnout/vypnout výstrahu.</span><span class="sxs-lookup"><span data-stu-id="97908-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="97908-296">toosee hello stránky výstrah, klikněte na tlačítko hello **výstrahy** kartě.</span><span class="sxs-lookup"><span data-stu-id="97908-296">toosee hello Alerts page, click hello **Alerts** tab.</span></span>

![Karta výstrahy](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a><span data-ttu-id="97908-298">toocreate výstrahu</span><span class="sxs-lookup"><span data-stu-id="97908-298">toocreate an alert</span></span>
1. <span data-ttu-id="97908-299">Klikněte na tlačítko **přidat výstraha** tooadd výstrahu.</span><span class="sxs-lookup"><span data-stu-id="97908-299">Click **Add Alert** tooadd an alert.</span></span> <span data-ttu-id="97908-300">Zobrazí hello **podrobnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="97908-300">You see hello **Details** page.</span></span>

    ![Vytvářet výstrahy - stránce s podrobnostmi o](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="97908-302">Zadejte hello **název** a **popis** hello výstrahu a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="97908-302">Specify hello **Name** and **Description** for hello alert, and click **Next**.</span></span> <span data-ttu-id="97908-303">Měli byste vidět hello **filtry** stránky.</span><span class="sxs-lookup"><span data-stu-id="97908-303">You should see hello **Filters** page.</span></span>

    ![Vytvořte upozornění – filtry stránky](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="97908-305">Vyberte hello **událostí**, **stav**, a **substatus** (volitelné), které chcete toocreate služba Data Factory pro výstrahy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="97908-305">Select hello **event**, **status**, and **substatus** (optional) that you want toocreate a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="97908-306">Měli byste vidět hello **příjemce** stránky.</span><span class="sxs-lookup"><span data-stu-id="97908-306">You should see hello **Recipients** page.</span></span>

    ![Vytvořte upozornění – stránka příjemce](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="97908-308">Vyberte hello **e-mailem správci předplatného** možnost a zadejte **další správce e-mailu**a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="97908-308">Select hello **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="97908-309">Měli byste vidět hello výstrahu v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-309">You should see hello alert in hello list.</span></span>

    ![Seznam výstrah](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="97908-311">V seznamu výstrah hello použijte hello tlačítka, které jsou přidruženy hello výstrahy tooedit/odstranění/zapnout/vypnout výstrahu.</span><span class="sxs-lookup"><span data-stu-id="97908-311">In hello Alerts list, use hello buttons that are associated with hello alert tooedit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="97908-312">Události, stav/substatus</span><span class="sxs-lookup"><span data-stu-id="97908-312">Event/status/substatus</span></span>
<span data-ttu-id="97908-313">Hello následující tabulka obsahuje seznam hello k dispozici události a stavy (a dílčí stavy).</span><span class="sxs-lookup"><span data-stu-id="97908-313">hello following table provides hello list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="97908-314">Název události</span><span class="sxs-lookup"><span data-stu-id="97908-314">Event name</span></span> | <span data-ttu-id="97908-315">Status</span><span class="sxs-lookup"><span data-stu-id="97908-315">Status</span></span> | <span data-ttu-id="97908-316">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="97908-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="97908-317">Aktivity při spuštění Začínáme</span><span class="sxs-lookup"><span data-stu-id="97908-317">Activity Run Started</span></span> |<span data-ttu-id="97908-318">spuštění</span><span class="sxs-lookup"><span data-stu-id="97908-318">Started</span></span> |<span data-ttu-id="97908-319">Spouštění</span><span class="sxs-lookup"><span data-stu-id="97908-319">Starting</span></span> |
| <span data-ttu-id="97908-320">Aktivity při spuštění bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="97908-320">Activity Run Finished</span></span> |<span data-ttu-id="97908-321">Úspěch</span><span class="sxs-lookup"><span data-stu-id="97908-321">Succeeded</span></span> |<span data-ttu-id="97908-322">Úspěch</span><span class="sxs-lookup"><span data-stu-id="97908-322">Succeeded</span></span> |
| <span data-ttu-id="97908-323">Aktivity při spuštění bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="97908-323">Activity Run Finished</span></span> |<span data-ttu-id="97908-324">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="97908-324">Failed</span></span> |<span data-ttu-id="97908-325">Přidělení prostředků se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="97908-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="97908-326">Spuštění se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="97908-326">Failed Execution</span></span><br/><br/><span data-ttu-id="97908-327">Vypršel časový limit</span><span class="sxs-lookup"><span data-stu-id="97908-327">Timed Out</span></span><br/><br/><span data-ttu-id="97908-328">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="97908-328">Failed Validation</span></span><br/><br/><span data-ttu-id="97908-329">opuštění</span><span class="sxs-lookup"><span data-stu-id="97908-329">Abandoned</span></span> |
| <span data-ttu-id="97908-330">Vytvoření clusteru HDI na vyžádání Začínáme</span><span class="sxs-lookup"><span data-stu-id="97908-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="97908-331">spuštění</span><span class="sxs-lookup"><span data-stu-id="97908-331">Started</span></span> |-|
| <span data-ttu-id="97908-332">Clusteru HDI na vyžádání úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="97908-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="97908-333">Úspěch</span><span class="sxs-lookup"><span data-stu-id="97908-333">Succeeded</span></span> |-|
| <span data-ttu-id="97908-334">Odstranit clusteru HDI na vyžádání</span><span class="sxs-lookup"><span data-stu-id="97908-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="97908-335">Úspěch</span><span class="sxs-lookup"><span data-stu-id="97908-335">Succeeded</span></span> |-|

### <a name="tooedit-delete-or-disable-an-alert"></a><span data-ttu-id="97908-336">tooedit, odstranit nebo zakázat výstrahy</span><span class="sxs-lookup"><span data-stu-id="97908-336">tooedit, delete, or disable an alert</span></span>

<span data-ttu-id="97908-337">Použijte následující tooedit tlačítka (zvýrazněné červeně), odstranit nebo zakázat výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="97908-337">Use hello following buttons (highlighted in red) tooedit, delete, or disable an alert.</span></span>

![Výstrahy tlačítka](./media/data-factory-monitor-manage-app/AlertButtons.png)
