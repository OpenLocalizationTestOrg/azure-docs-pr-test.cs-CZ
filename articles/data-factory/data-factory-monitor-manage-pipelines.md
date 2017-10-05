---
title: "Monitorování a Správa kanálů pomocí portálu Azure a prostředí PowerShell | Microsoft Docs"
description: "Další informace o použití portálu Azure a prostředí Azure PowerShell monitorovat a spravovat Azure data Factory a kanály, které jste vytvořili."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: 61bb5379cd94dd00814e14420947e7783999ff0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a><span data-ttu-id="b5add-103">Monitorování a Správa kanálů služby Azure Data Factory pomocí portálu Azure a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5add-103">Monitor and manage Azure Data Factory pipelines by using the Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b5add-104">Použití Azure portal nebo Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5add-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="b5add-105">Pomocí monitorování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="b5add-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="b5add-106">Aplikace monitorování a správu poskytuje lepší podporu pro monitorování a správy datových kanálů a řešení potíží s problémy.</span><span class="sxs-lookup"><span data-stu-id="b5add-106">The monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="b5add-107">Podrobnosti o použití této aplikace najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="b5add-107">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="b5add-108">Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí portálu Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5add-108">This article describes how to monitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="b5add-109">Tento článek také poskytuje informace o tom, jak vytvářet výstrahy a upozorňování o selhání.</span><span class="sxs-lookup"><span data-stu-id="b5add-109">The article also provides information on how to create alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="b5add-110">Pochopit kanály a aktivity stavy</span><span class="sxs-lookup"><span data-stu-id="b5add-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="b5add-111">Pomocí portálu Azure, můžete:</span><span class="sxs-lookup"><span data-stu-id="b5add-111">By using the Azure portal, you can:</span></span>

* <span data-ttu-id="b5add-112">Objekt pro vytváření dat zobrazte jako diagram.</span><span class="sxs-lookup"><span data-stu-id="b5add-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="b5add-113">Zobrazit aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="b5add-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="b5add-114">Zobrazte vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="b5add-114">View input and output datasets.</span></span>

<span data-ttu-id="b5add-115">Tato část také popisuje, jak se řez datovou sadu přechází z jednoho stavu do jiného stavu.</span><span class="sxs-lookup"><span data-stu-id="b5add-115">This section also describes how a dataset slice transitions from one state to another state.</span></span>   

### <a name="navigate-to-your-data-factory"></a><span data-ttu-id="b5add-116">Přejděte do data factory</span><span class="sxs-lookup"><span data-stu-id="b5add-116">Navigate to your data factory</span></span>
1. <span data-ttu-id="b5add-117">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b5add-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b5add-118">Klikněte na tlačítko **datové továrny** v nabídce na levé straně.</span><span class="sxs-lookup"><span data-stu-id="b5add-118">Click **Data factories** on the menu on the left.</span></span> <span data-ttu-id="b5add-119">Pokud ho nevidíte, klikněte na tlačítko **další služby >**a potom klikněte na **datové továrny** pod **INTELLIGENCE + analýzy** kategorie.</span><span class="sxs-lookup"><span data-stu-id="b5add-119">If you don't see it, click **More services >**, and then click **Data factories** under the **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Procházet všechny > datové továrny](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="b5add-121">Na **datové továrny** okně vyberte služby data factory, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="b5add-121">On the **Data factories** blade, select the data factory that you're interested in.</span></span>

    ![Vyberte objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="b5add-123">Měli byste vidět domovské stránce služby data Factory.</span><span class="sxs-lookup"><span data-stu-id="b5add-123">You should see the home page for the data factory.</span></span>

   ![Okno objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="b5add-125">Zobrazení diagramu svojí datové továrny</span><span class="sxs-lookup"><span data-stu-id="b5add-125">Diagram view of your data factory</span></span>
<span data-ttu-id="b5add-126">**Diagram** zobrazení objektu pro vytváření dat poskytuje skla ke sledování a správě objektu pro vytváření dat a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="b5add-126">The **Diagram** view of a data factory provides a single pane of glass to monitor and manage the data factory and its assets.</span></span> <span data-ttu-id="b5add-127">Chcete-li zobrazit **Diagram** zobrazení objektu pro vytváření dat, klikněte na tlačítko **Diagram** na domovské stránce objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="b5add-127">To see the **Diagram** view of your data factory, click **Diagram** on the home page for the data factory.</span></span>

![Zobrazení diagramu](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="b5add-129">Můžete přiblížení, oddálení, zvětšení přizpůsobit, zvětšení na 100 %, uzamčení rozložení diagramu a automatické umísťování kanálů a datové sady.</span><span class="sxs-lookup"><span data-stu-id="b5add-129">You can zoom in, zoom out, zoom to fit, zoom to 100%, lock the layout of the diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="b5add-130">Můžete také zjistit informace o rodokmenu dat (tedy zobrazit nadřazené a podřízené položky vybraných položek).</span><span class="sxs-lookup"><span data-stu-id="b5add-130">You can also see the data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="b5add-131">Aktivity v kanálu</span><span class="sxs-lookup"><span data-stu-id="b5add-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="b5add-132">Klikněte pravým tlačítkem na kanál a pak klikněte na **otevřít kanál** zobrazíte všechny aktivity v kanálu spolu s vstupní a výstupní datové sady pro aktivity.</span><span class="sxs-lookup"><span data-stu-id="b5add-132">Right-click the pipeline, and then click **Open pipeline** to see all activities in the pipeline, along with input and output datasets for the activities.</span></span> <span data-ttu-id="b5add-133">Tato funkce je užitečná, když vaše kanál obsahuje více než jednu aktivitu a chcete se dozvědět provozní rodokmenu jednoho kanálu.</span><span class="sxs-lookup"><span data-stu-id="b5add-133">This feature is useful when your pipeline includes more than one activity and you want to understand the operational lineage of a single pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="b5add-135">V následujícím příkladu najdete v části aktivity kopírování v kanálu s vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="b5add-135">In the following example, you see a copy activity in the pipeline with an input and an output.</span></span> 

    ![Aktivity v kanálu](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="b5add-137">Můžete přejít zpět na domovské stránce objektu pro vytváření dat kliknutím **objekt pro vytváření dat** odkaz v zobrazení cesty v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="b5add-137">You can navigate back to the home page of the data factory by clicking the **Data factory** link in the breadcrumb at the top-left corner.</span></span>

    ![Přejděte zpět na objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="b5add-139">Zobrazit stav každé aktivity v kanálu</span><span class="sxs-lookup"><span data-stu-id="b5add-139">View the state of each activity inside a pipeline</span></span>
<span data-ttu-id="b5add-140">Zobrazení stavu libovolných datové sady, které jsou produkované aktivitou, můžete zobrazit aktuální stav aktivity.</span><span class="sxs-lookup"><span data-stu-id="b5add-140">You can view the current state of an activity by viewing the status of any of the datasets that are produced by the activity.</span></span>

<span data-ttu-id="b5add-141">Dvojitým kliknutím **OutputBlobTable** v **Diagram**, zobrazí se všechny datové řezy, které vznikají pomocí funkcí spustí jinou aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="b5add-141">By double-clicking the **OutputBlobTable** in the **Diagram**, you can see all the slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="b5add-142">Uvidíte, aktivitě kopírování byla spuštěna úspěšně za posledních 8 hodin a vytváří výseče **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="b5add-142">You can see that the copy activity ran successfully for the last eight hours and produced the slices in the **Ready** state.</span></span>  

![Stav kanálu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="b5add-144">Řezy datovou sadu v objektu pro vytváření dat může mít jednu z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="b5add-144">The dataset slices in the data factory can have one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="b5add-145">Stav</span><span class="sxs-lookup"><span data-stu-id="b5add-145">State</span></span></th><th align="left"><span data-ttu-id="b5add-146">Dílčím stavem</span><span class="sxs-lookup"><span data-stu-id="b5add-146">Substate</span></span></th><th align="left"><span data-ttu-id="b5add-147">Popis</span><span class="sxs-lookup"><span data-stu-id="b5add-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="b5add-148">Čekání</span><span class="sxs-lookup"><span data-stu-id="b5add-148">Waiting</span></span></td><td><span data-ttu-id="b5add-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="b5add-149">ScheduleTime</span></span></td><td><span data-ttu-id="b5add-150">Pro spuštění řezu ještě nenastal čas.</span><span class="sxs-lookup"><span data-stu-id="b5add-150">The time hasn't come for the slice to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="b5add-151">DatasetDependencies</span></span></td><td><span data-ttu-id="b5add-152">Upstreamové závislosti nejsou připravené.</span><span class="sxs-lookup"><span data-stu-id="b5add-152">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="b5add-153">ComputeResources</span></span></td><td><span data-ttu-id="b5add-154">Výpočetní prostředky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b5add-154">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="b5add-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="b5add-156">Všechny instance aktivit právě zpracovávají jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="b5add-156">All the activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="b5add-157">ActivityResume</span></span></td><td><span data-ttu-id="b5add-158">Aktivita je pozastavená a zpracování řezů nejde spustit, dokud je obnoveno aktivity.</span><span class="sxs-lookup"><span data-stu-id="b5add-158">The activity is paused and can't run the slices until the activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-159">Opakování</span><span class="sxs-lookup"><span data-stu-id="b5add-159">Retry</span></span></td><td><span data-ttu-id="b5add-160">Probíhá pokus o spuštění aktivity je zopakován.</span><span class="sxs-lookup"><span data-stu-id="b5add-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-161">Ověření</span><span class="sxs-lookup"><span data-stu-id="b5add-161">Validation</span></span></td><td><span data-ttu-id="b5add-162">Ověření se ještě nespustilo.</span><span class="sxs-lookup"><span data-stu-id="b5add-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="b5add-163">ValidationRetry</span></span></td><td><span data-ttu-id="b5add-164">Ověření čeká na opakovat.</span><span class="sxs-lookup"><span data-stu-id="b5add-164">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="b5add-165">InProgress</span><span class="sxs-lookup"><span data-stu-id="b5add-165">InProgress</span></span></td><td><span data-ttu-id="b5add-166">Probíhá ověřování</span><span class="sxs-lookup"><span data-stu-id="b5add-166">Validating</span></span></td><td><span data-ttu-id="b5add-167">Probíhá ověřování.</span><span class="sxs-lookup"><span data-stu-id="b5add-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="b5add-168">Řez se zpracovává.</span><span class="sxs-lookup"><span data-stu-id="b5add-168">The slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="b5add-169">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b5add-169">Failed</span></span></td><td><span data-ttu-id="b5add-170">TimedOut</span><span class="sxs-lookup"><span data-stu-id="b5add-170">TimedOut</span></span></td><td><span data-ttu-id="b5add-171">Provedení aktivity trvalo déle, než je povolené aktivitou.</span><span class="sxs-lookup"><span data-stu-id="b5add-171">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-172">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="b5add-172">Canceled</span></span></td><td><span data-ttu-id="b5add-173">Řez zrušil akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="b5add-173">The slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-174">Ověření</span><span class="sxs-lookup"><span data-stu-id="b5add-174">Validation</span></span></td><td><span data-ttu-id="b5add-175">Ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="b5add-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="b5add-176">Řez se nepodařilo vygenerovat nebo ověřit.</span><span class="sxs-lookup"><span data-stu-id="b5add-176">The slice failed to be generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="b5add-177">Připraveno</span><span class="sxs-lookup"><span data-stu-id="b5add-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="b5add-178">Řez je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="b5add-178">The slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-179">Přeskočena</span><span class="sxs-lookup"><span data-stu-id="b5add-179">Skipped</span></span></td><td><span data-ttu-id="b5add-180">Žádný</span><span class="sxs-lookup"><span data-stu-id="b5add-180">None</span></span></td><td><span data-ttu-id="b5add-181">Řez se zpracovává.</span><span class="sxs-lookup"><span data-stu-id="b5add-181">The slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b5add-182">Žádný</span><span class="sxs-lookup"><span data-stu-id="b5add-182">None</span></span></td><td>-</td><td><span data-ttu-id="b5add-183">Řez měl dříve jiný stav, ale byla obnovena.</span><span class="sxs-lookup"><span data-stu-id="b5add-183">A slice used to exist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="b5add-184">Podrobnosti o řez můžete zobrazit kliknutím na položku řez **nedávno aktualizován řezy** okno.</span><span class="sxs-lookup"><span data-stu-id="b5add-184">You can view the details about a slice by clicking a slice entry on the **Recently Updated Slices** blade.</span></span>

![Řez podrobnosti](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="b5add-186">Pokud je řez provedl vícekrát, zobrazí více řádků v **aktivita spuštěna** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5add-186">If the slice has been executed multiple times, you see multiple rows in the **Activity runs** list.</span></span> <span data-ttu-id="b5add-187">Můžete zobrazit podrobnosti o aktivitě spustíte kliknutím na položku Spustit v **aktivita spuštěna** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5add-187">You can view details about an activity run by clicking the run entry in the **Activity runs** list.</span></span> <span data-ttu-id="b5add-188">V seznamu jsou uvedeny všechny soubory protokolu, spolu s chybovou zprávu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="b5add-188">The list shows all the log files, along with an error message if there is one.</span></span> <span data-ttu-id="b5add-189">Tato funkce je užitečná k zobrazení a ladění protokoly, aniž by museli opustit váš služby data factory.</span><span class="sxs-lookup"><span data-stu-id="b5add-189">This feature is useful to view and debug logs without having to leave your data factory.</span></span>

![Podrobnosti o spuštění aktivit](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="b5add-191">Není-li řez v **připraven** stavu, můžete zobrazit upstreamové datové řezy, které nejsou připravené a které blokují spuštění aktuálního řezu ve spuštění **Upstreamové datové řezy, které nejsou připraveny** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5add-191">If the slice isn't in the **Ready** state, you can see the upstream slices that aren't ready and are blocking the current slice from executing in the **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="b5add-192">Tato funkce je užitečná, když vaše řez v **čekání** stavu a chcete pochopili nadřazeného závislosti, které řez čeká na.</span><span class="sxs-lookup"><span data-stu-id="b5add-192">This feature is useful when your slice is in **Waiting** state and you want to understand the upstream dependencies that the slice is waiting on.</span></span>

![Upstreamové datové řezy, které nejsou připraveny](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="b5add-194">Diagram stavu datové sady</span><span class="sxs-lookup"><span data-stu-id="b5add-194">Dataset state diagram</span></span>
<span data-ttu-id="b5add-195">Po nasazení služby data factory a kanálů mají platný aktivní období, datová sada řezy přechod z jednoho stavu do jiného.</span><span class="sxs-lookup"><span data-stu-id="b5add-195">After you deploy a data factory and the pipelines have a valid active period, the dataset slices transition from one state to another.</span></span> <span data-ttu-id="b5add-196">V současné době stav řezu zahrnuje následující diagram stavu:</span><span class="sxs-lookup"><span data-stu-id="b5add-196">Currently, the slice status follows the following state diagram:</span></span>

![Diagram stavu](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="b5add-198">Tok přechod stavu datovou sadu v objektu pro vytváření dat je následující: Čekání na -> průběh/v – probíhající (ověřování) -> Připraveno nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="b5add-198">The dataset state transition flow in data factory is the following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="b5add-199">Řez se spustí v **čekání** stavu čekání předběžných podmínek, které musí být splněné, než se provede.</span><span class="sxs-lookup"><span data-stu-id="b5add-199">The slice starts in a **Waiting** state, waiting for preconditions to be met before it executes.</span></span> <span data-ttu-id="b5add-200">Pak spustí aktivita provádění a řez přejde do **probíhající** stavu.</span><span class="sxs-lookup"><span data-stu-id="b5add-200">Then, the activity starts executing, and the slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="b5add-201">Provedení aktivity může úspěch nebo neúspěch.</span><span class="sxs-lookup"><span data-stu-id="b5add-201">The activity execution might succeed or fail.</span></span> <span data-ttu-id="b5add-202">Řez je označena jako **připraven** nebo **se nezdařilo**, podle výsledek provedení.</span><span class="sxs-lookup"><span data-stu-id="b5add-202">The slice is marked as **Ready** or **Failed**, based on the result of the execution.</span></span>

<span data-ttu-id="b5add-203">Můžete resetovat řez se vrátíte z **připraven** nebo **se nezdařilo** stavu na **čekání** stavu.</span><span class="sxs-lookup"><span data-stu-id="b5add-203">You can reset the slice to go back from the **Ready** or **Failed** state to the **Waiting** state.</span></span> <span data-ttu-id="b5add-204">Můžete také označit stav řez, který má **přeskočit**, která brání aktivity z provádění a není zpracování řezu.</span><span class="sxs-lookup"><span data-stu-id="b5add-204">You can also mark the slice state to **Skip**, which prevents the activity from executing and not processing the slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="b5add-205">Pozastavení a obnovení kanálů</span><span class="sxs-lookup"><span data-stu-id="b5add-205">Pause and resume pipelines</span></span>
<span data-ttu-id="b5add-206">Kanály můžete spravovat pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5add-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="b5add-207">Například můžete pozastavit a obnovit kanály spuštěním rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5add-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="b5add-208">Zobrazení diagramu nepodporuje pozastavení a obnovení kanály.</span><span class="sxs-lookup"><span data-stu-id="b5add-208">The diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="b5add-209">Pokud chcete použít uživatelské rozhraní, použijte monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5add-209">If you want to use an user interface, use the monitoring and managing application.</span></span> <span data-ttu-id="b5add-210">Podrobnosti o použití této aplikace najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b5add-210">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="b5add-211">Je možné pozastavit nebo pozastavit kanály pomocí **Suspend-AzureRmDataFactoryPipeline** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5add-211">You can pause/suspend pipelines by using the **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="b5add-212">Tato rutina je užitečná, když nechcete, aby běžela kanály, dokud nebude problém vyřešen.</span><span class="sxs-lookup"><span data-stu-id="b5add-212">This cmdlet is useful when you don’t want to run your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="b5add-213">Například:</span><span class="sxs-lookup"><span data-stu-id="b5add-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="b5add-214">Po napravení problému s kanálu, můžete obnovit pozastavenou kanálu spuštěním následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="b5add-214">After the issue has been fixed with the pipeline, you can resume the suspended pipeline by running the following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="b5add-215">Například:</span><span class="sxs-lookup"><span data-stu-id="b5add-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="b5add-216">Ladit kanály</span><span class="sxs-lookup"><span data-stu-id="b5add-216">Debug pipelines</span></span>
<span data-ttu-id="b5add-217">Azure Data Factory poskytuje bohaté možnosti pro ladění a řešení potíží kanály pomocí portálu Azure a prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5add-217">Azure Data Factory provides rich capabilities for you to debug and troubleshoot pipelines by using the Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="b5add-218">[! Poznámka:}, že je mnohem snazší potíže chyby pomocí aplikace pro správu a monitorování.</span><span class="sxs-lookup"><span data-stu-id="b5add-218">[!NOTE} It is much easier to troubleshot errors using the Monitoring & Management App.</span></span> <span data-ttu-id="b5add-219">Podrobnosti o použití této aplikace najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b5add-219">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="b5add-220">Vyhledejte chyby v kanálu</span><span class="sxs-lookup"><span data-stu-id="b5add-220">Find errors in a pipeline</span></span>
<span data-ttu-id="b5add-221">Pokud se nezdaří spustit aktivitu v kanálu, datovou sadu, která je vytvořena v kanálu je v chybovém stavu z důvodu selhání.</span><span class="sxs-lookup"><span data-stu-id="b5add-221">If the activity run fails in a pipeline, the dataset that is produced by the pipeline is in an error state because of the failure.</span></span> <span data-ttu-id="b5add-222">Můžete ladění a odstraňování chyb v Azure Data Factory pomocí následujících metod.</span><span class="sxs-lookup"><span data-stu-id="b5add-222">You can debug and troubleshoot errors in Azure Data Factory by using the following methods.</span></span>

#### <a name="use-the-azure-portal-to-debug-an-error"></a><span data-ttu-id="b5add-223">Použití portálu Azure k ladění k chybě</span><span class="sxs-lookup"><span data-stu-id="b5add-223">Use the Azure portal to debug an error</span></span>
1. <span data-ttu-id="b5add-224">Na **tabulky** okně klikněte na tlačítko řez problém, který má **stav** nastavena na **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="b5add-224">On the **Table** blade, click the problem slice that has the **Status** set to **Failed**.</span></span>

   ![Okno tabulky s řez problém](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="b5add-226">Na **datový řez** okno, klikněte na tlačítko spuštění aktivity, která se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="b5add-226">On the **Data slice** blade, click the activity run that failed.</span></span>

   ![Datový řez s chybou](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="b5add-228">Na **podrobnosti o spuštění aktivit** okně si můžete stáhnout soubory, které jsou spojeny s HDInsight zpracování.</span><span class="sxs-lookup"><span data-stu-id="b5add-228">On the **Activity run details** blade, you can download the files that are associated with the HDInsight processing.</span></span> <span data-ttu-id="b5add-229">Klikněte na tlačítko **Stáhnout** pro stav nebo stderr ke stažení souboru protokolu chyb, který obsahuje podrobnosti o této chybě.</span><span class="sxs-lookup"><span data-stu-id="b5add-229">Click **Download** for Status/stderr to download the error log file that contains details about the error.</span></span>

   ![Aktivity při spuštění podrobnosti okno s chybou](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a><span data-ttu-id="b5add-231">Použití prostředí PowerShell k ladění k chybě</span><span class="sxs-lookup"><span data-stu-id="b5add-231">Use PowerShell to debug an error</span></span>
1. <span data-ttu-id="b5add-232">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="b5add-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="b5add-233">Spustit **Get-AzureRmDataFactorySlice** příkazu zobrazte řezy a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="b5add-233">Run the **Get-AzureRmDataFactorySlice** command to see the slices and their statuses.</span></span> <span data-ttu-id="b5add-234">Měli byste vidět řez se stavem **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="b5add-234">You should see a slice with the status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="b5add-235">Například:</span><span class="sxs-lookup"><span data-stu-id="b5add-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="b5add-236">Nahraďte **StartDateTime** se časem spuštění vaší kanálu.</span><span class="sxs-lookup"><span data-stu-id="b5add-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="b5add-237">Teď, spusťte **Get-AzureRmDataFactoryRun** rutinu získat tak podrobné údaje o aktivitě spustit řez.</span><span class="sxs-lookup"><span data-stu-id="b5add-237">Now, run the **Get-AzureRmDataFactoryRun** cmdlet to get details about the activity run for the slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="b5add-238">Například:</span><span class="sxs-lookup"><span data-stu-id="b5add-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="b5add-239">Hodnota StartDateTime je čas zahájení pro řez chyby nebo problému, který jste si poznamenali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b5add-239">The value of StartDateTime is the start time for the error/problem slice that you noted from the previous step.</span></span> <span data-ttu-id="b5add-240">Datum a čas by měl být uzavřena do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="b5add-240">The date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="b5add-241">Měli byste vidět výstup s podrobnostmi o chybě, který je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="b5add-241">You should see output with details about the error that is similar to the following:</span></span>

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. <span data-ttu-id="b5add-242">Můžete spustit **uložit AzureRmDataFactoryLog** rutiny se hodnota Id najdete v části z výstupu a stažení souborů protokolu pomocí **- DownloadLogsoption** pro rutinu.</span><span class="sxs-lookup"><span data-stu-id="b5add-242">You can run the **Save-AzureRmDataFactoryLog** cmdlet with the Id value that you see from the output, and download the log files by using the **-DownloadLogsoption** for the cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="b5add-243">Znovu spustit chyby v kanálu</span><span class="sxs-lookup"><span data-stu-id="b5add-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5add-244">Je snazší řešení chyb a znovu spusťte selhání řezy pomocí monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5add-244">It's easier to troubleshoot errors and rerun failed slices by using the Monitoring & Management App.</span></span> <span data-ttu-id="b5add-245">Podrobnosti o použití této aplikace najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="b5add-245">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-the-azure-portal"></a><span data-ttu-id="b5add-246">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5add-246">Use the Azure portal</span></span>
<span data-ttu-id="b5add-247">Po řešení potíží a ladění chyby v kanálu se může znovu selhání přejdete na řez chyba a kliknutím na **spustit** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b5add-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating to the error slice and clicking the **Run** button on the command bar.</span></span>

![Opětovné spuštění neúspěšné řez](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="b5add-249">V případě řezu selhalo ověření z důvodu selhání zásad (například pokud data nejsou k dispozici), můžete opravit chyby a znovu ověřit kliknutím **ověřením** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b5add-249">In case the slice has failed validation because of a policy failure (for example, if data isn't available), you can fix the failure and validate again by clicking the **Validate** button on the command bar.</span></span>

![Opravte chyby a ověření](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="b5add-251">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="b5add-251">Use Azure PowerShell</span></span>
<span data-ttu-id="b5add-252">Selhání může znovu s použitím **Set-AzureRmDataFactorySliceStatus** rutiny.</span><span class="sxs-lookup"><span data-stu-id="b5add-252">You can rerun failures by using the **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="b5add-253">Najdete v článku [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) téma pro syntaxi a další podrobnosti o rutině.</span><span class="sxs-lookup"><span data-stu-id="b5add-253">See the [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about the cmdlet.</span></span>

<span data-ttu-id="b5add-254">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b5add-254">**Example:**</span></span>

<span data-ttu-id="b5add-255">Následující příklad nastaví stav všech řezech tabulky 'DAWikiAggregatedData' čekání v Azure data factory 'WikiADF'.</span><span class="sxs-lookup"><span data-stu-id="b5add-255">The following example sets the status of all slices for the table 'DAWikiAggregatedData' to 'Waiting' in the Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="b5add-256">Typ 'aktualizace, je nastaven na 'UpstreamInPipeline", což znamená, že stavy každý řez tabulky a všechny závislé (nadřazený) tabulky jsou nastavená na 'Čekání na'.</span><span class="sxs-lookup"><span data-stu-id="b5add-256">The 'UpdateType' is set to 'UpstreamInPipeline', which means that statuses of each slice for the table and all the dependent (upstream) tables are set to 'Waiting'.</span></span> <span data-ttu-id="b5add-257">S možnou hodnotou tohoto parametru je "Individuální".</span><span class="sxs-lookup"><span data-stu-id="b5add-257">The other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="b5add-258">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="b5add-258">Create alerts</span></span>
<span data-ttu-id="b5add-259">Azure protokoly událostí uživatele při prostředků Azure (například objekt pro vytváření dat) je vytvořen, aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="b5add-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="b5add-260">Výstrahy můžete vytvořit na těchto událostech.</span><span class="sxs-lookup"><span data-stu-id="b5add-260">You can create alerts on these events.</span></span> <span data-ttu-id="b5add-261">Objekt pro vytváření dat můžete použít k zachycení různé metriky a vytvořit oznámení o metrikách.</span><span class="sxs-lookup"><span data-stu-id="b5add-261">You can use Data Factory to capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="b5add-262">Doporučujeme použít události pro monitorování v reálném čase a použít metriky pro účely záznamu historie událostí.</span><span class="sxs-lookup"><span data-stu-id="b5add-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="b5add-263">Výstrahy na události</span><span class="sxs-lookup"><span data-stu-id="b5add-263">Alerts on events</span></span>
<span data-ttu-id="b5add-264">Azure události poskytují užitečné přehledy co se děje v vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b5add-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="b5add-265">Pokud používáte Azure Data Factory, události se generují při:</span><span class="sxs-lookup"><span data-stu-id="b5add-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="b5add-266">Objekt pro vytváření dat je vytvořen, aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="b5add-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="b5add-267">Zpracování dat (jako "spuštěno") je spuštěna, nebo byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="b5add-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="b5add-268">Vytvoření clusteru HDInsight na vyžádání nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="b5add-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="b5add-269">Můžete vytvářet výstrahy na těchto událostech uživatele a nakonfigurovat je pro odeslání e-mailová oznámení pro správce a coadministrators předplatného.</span><span class="sxs-lookup"><span data-stu-id="b5add-269">You can create alerts on these user events and configure them to send email notifications to the administrator and coadministrators of the subscription.</span></span> <span data-ttu-id="b5add-270">Kromě toho můžete zadat další e-mailové adresy uživatelů, kteří potřebují pro příjem e-mailová oznámení, pokud jsou splněny podmínky.</span><span class="sxs-lookup"><span data-stu-id="b5add-270">In addition, you can specify additional email addresses of users who need to receive email notifications when the conditions are met.</span></span> <span data-ttu-id="b5add-271">Tato funkce je užitečná, když chcete dostat upozornění na selhání a nechcete neustále monitorovat váš služby data factory.</span><span class="sxs-lookup"><span data-stu-id="b5add-271">This feature is useful when you want to get notified on failures and don’t want to continuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="b5add-272">V současné době nepodporuje na portálu zobrazit výstrahy na události.</span><span class="sxs-lookup"><span data-stu-id="b5add-272">Currently, the portal doesn't show alerts on events.</span></span> <span data-ttu-id="b5add-273">Použití [monitorování a správu aplikace](data-factory-monitor-manage-app.md) zobrazíte všechny výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b5add-273">Use the [Monitoring and Management app](data-factory-monitor-manage-app.md) to see all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="b5add-274">Zadejte definici výstrah</span><span class="sxs-lookup"><span data-stu-id="b5add-274">Specify an alert definition</span></span>
<span data-ttu-id="b5add-275">Pokud chcete zadat výstrahy definice, vytvořte soubor JSON, který popisuje operace, které chcete být upozorněni na.</span><span class="sxs-lookup"><span data-stu-id="b5add-275">To specify an alert definition, you create a JSON file that describes the operations that you want to be alerted on.</span></span> <span data-ttu-id="b5add-276">V následujícím příkladu odešle výstrahy e-mailové oznámení pro operaci RunFinished.</span><span class="sxs-lookup"><span data-stu-id="b5add-276">In the following example, the alert sends an email notification for the RunFinished operation.</span></span> <span data-ttu-id="b5add-277">Být konkrétní, je odeslána e-mailové oznámení, pokud bylo dokončeno spustit v datové továrně a spustit se nezdařila (stav = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="b5add-277">To be specific, an email notification is sent when a run in the data factory has completed and the run has failed (Status = FailedExecution).</span></span>

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

<span data-ttu-id="b5add-278">Můžete odebrat **subStatus** z definice JSON, pokud chcete být upozorněni na konkrétní chyby.</span><span class="sxs-lookup"><span data-stu-id="b5add-278">You can remove **subStatus** from the JSON definition if you don’t want to be alerted on a specific failure.</span></span>

<span data-ttu-id="b5add-279">Tento příklad nastaví upozornění pro všechny datové továrny v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="b5add-279">This example sets up the alert for all data factories in your subscription.</span></span> <span data-ttu-id="b5add-280">Pokud chcete výstrahu, kterou chcete být nastavenou službu objekt pro vytváření konkrétní data, můžete zadat objekt pro vytváření dat **resourceUri** v **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="b5add-280">If you want the alert to be set up for a particular data factory, you can specify data factory **resourceUri** in the **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="b5add-281">Následující tabulka obsahuje seznam dostupné operace a stavy (a dílčí stavy).</span><span class="sxs-lookup"><span data-stu-id="b5add-281">The following table provides the list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="b5add-282">Název operace</span><span class="sxs-lookup"><span data-stu-id="b5add-282">Operation name</span></span> | <span data-ttu-id="b5add-283">Status</span><span class="sxs-lookup"><span data-stu-id="b5add-283">Status</span></span> | <span data-ttu-id="b5add-284">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="b5add-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b5add-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="b5add-285">RunStarted</span></span> |<span data-ttu-id="b5add-286">spuštění</span><span class="sxs-lookup"><span data-stu-id="b5add-286">Started</span></span> |<span data-ttu-id="b5add-287">Spouštění</span><span class="sxs-lookup"><span data-stu-id="b5add-287">Starting</span></span> |
| <span data-ttu-id="b5add-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="b5add-288">RunFinished</span></span> |<span data-ttu-id="b5add-289">Nemohl / bylo úspěšné</span><span class="sxs-lookup"><span data-stu-id="b5add-289">Failed / Succeeded</span></span> |<span data-ttu-id="b5add-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="b5add-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="b5add-291">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b5add-291">Succeeded</span></span><br/><br/><span data-ttu-id="b5add-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="b5add-292">FailedExecution</span></span><br/><br/><span data-ttu-id="b5add-293">TimedOut</span><span class="sxs-lookup"><span data-stu-id="b5add-293">TimedOut</span></span><br/><br/><span data-ttu-id="b5add-294">< zrušena</span><span class="sxs-lookup"><span data-stu-id="b5add-294"><Canceled</span></span><br/><br/><span data-ttu-id="b5add-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="b5add-295">FailedValidation</span></span><br/><br/><span data-ttu-id="b5add-296">opuštění</span><span class="sxs-lookup"><span data-stu-id="b5add-296">Abandoned</span></span> |
| <span data-ttu-id="b5add-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="b5add-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="b5add-298">spuštění</span><span class="sxs-lookup"><span data-stu-id="b5add-298">Started</span></span> | |
| <span data-ttu-id="b5add-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="b5add-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="b5add-300">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b5add-300">Succeeded</span></span> | |
| <span data-ttu-id="b5add-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="b5add-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="b5add-302">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b5add-302">Succeeded</span></span> | |

<span data-ttu-id="b5add-303">V tématu [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) podrobnosti o elementy JSON, které se používají v příkladu.</span><span class="sxs-lookup"><span data-stu-id="b5add-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about the JSON elements that are used in the example.</span></span>

#### <a name="deploy-the-alert"></a><span data-ttu-id="b5add-304">Nasazení výstrahy</span><span class="sxs-lookup"><span data-stu-id="b5add-304">Deploy the alert</span></span>
<span data-ttu-id="b5add-305">Nasadit výstrahy, použijte rutinu prostředí Azure PowerShell **New-AzureRmResourceGroupDeployment**, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b5add-305">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="b5add-306">Po nasazení skupiny prostředků se úspěšně dokončil, můžete zobrazit následující zprávy:</span><span class="sxs-lookup"><span data-stu-id="b5add-306">After the resource group deployment has finished successfully, you see the following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts to remove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> <span data-ttu-id="b5add-307">Můžete použít [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API pro vytvoření pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b5add-307">You can use the [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API to create an alert rule.</span></span> <span data-ttu-id="b5add-308">Datová část JSON je podobné jako v příkladu JSON.</span><span class="sxs-lookup"><span data-stu-id="b5add-308">The JSON payload is similar to the JSON example.</span></span>  


#### <a name="retrieve-the-list-of-azure-resource-group-deployments"></a><span data-ttu-id="b5add-309">Načtení seznamu nasazení skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b5add-309">Retrieve the list of Azure resource group deployments</span></span>
<span data-ttu-id="b5add-310">Pro načtení seznamu nasazení skupiny prostředků nasazené Azure, použijte rutinu **Get-AzureRmResourceGroupDeployment**, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b5add-310">To retrieve the list of deployed Azure resource group deployments, use the cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="b5add-311">Řešení potíží s událostmi uživatele</span><span class="sxs-lookup"><span data-stu-id="b5add-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="b5add-312">Zobrazí všechny události, které jsou generovány po kliknutí na tlačítko **metriky a operace** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="b5add-312">You can see all the events that are generated after clicking the **Metrics and operations** tile.</span></span>

    ![Dlaždice metriky a operace](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="b5add-314">Klikněte **události** dlaždice sledovat události.</span><span class="sxs-lookup"><span data-stu-id="b5add-314">Click the **Events** tile to see the events.</span></span>

    ![Dlaždice události](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="b5add-316">Na **události** okno, můžete zobrazit podrobnosti o události, filtrované události a tak dále.</span><span class="sxs-lookup"><span data-stu-id="b5add-316">On the **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Okno události](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="b5add-318">Klikněte na tlačítko **operace** v seznamu operací, která způsobuje chybu.</span><span class="sxs-lookup"><span data-stu-id="b5add-318">Click an **Operation** in the operations list that causes an error.</span></span>

    ![Vyberte operaci](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="b5add-320">Klikněte na tlačítko **chyba** událost zobrazíte podrobnosti o této chybě.</span><span class="sxs-lookup"><span data-stu-id="b5add-320">Click an **Error** event to see details about the error.</span></span>

    ![Chyba události](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="b5add-322">V tématu [rutiny Azure přehledy](https://msdn.microsoft.com/library/mt282452.aspx) pro rutiny prostředí PowerShell, které můžete přidat, získat, nebo odeberte výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b5add-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use to add, get, or remove alerts.</span></span> <span data-ttu-id="b5add-323">Tady je několik příkladů použití **Get-AlertRule** rutiny:</span><span class="sxs-lookup"><span data-stu-id="b5add-323">Here are a few examples of using the **Get-AlertRule** cmdlet:</span></span>

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of the data slices for the Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

<span data-ttu-id="b5add-324">Spusťte následující příkazy get-help zobrazíte podrobnosti a příklady pro rutinu Get-AlertRule.</span><span class="sxs-lookup"><span data-stu-id="b5add-324">Run the following get-help commands to see details and examples for the Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="b5add-325">Pokud uvidíte události generování výstrah v okně portálu, ale nebudete dostávat e-mailová oznámení, zkontrolujte, zda je pro příjem e-mailů z externí odesílatelé nastaven e-mailovou adresu, která je zadána.</span><span class="sxs-lookup"><span data-stu-id="b5add-325">If you see the alert generation events on the portal blade but you don't receive email notifications, check whether the email address that is specified is set to receive emails from external senders.</span></span> <span data-ttu-id="b5add-326">Výstrahy e-mailů může mít zablokovaný nastavení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b5add-326">The alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="b5add-327">Výstrahy na metriky</span><span class="sxs-lookup"><span data-stu-id="b5add-327">Alerts on metrics</span></span>
<span data-ttu-id="b5add-328">V objektu pro vytváření dat můžete zachytit různé metriky a vytvořit oznámení o metrikách.</span><span class="sxs-lookup"><span data-stu-id="b5add-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="b5add-329">Můžete sledovat a vytvářet upozornění na následující metriky pro řezy v datové továrně:</span><span class="sxs-lookup"><span data-stu-id="b5add-329">You can monitor and create alerts on the following metrics for the slices in your data factory:</span></span>

* <span data-ttu-id="b5add-330">**Spustí se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="b5add-330">**Failed Runs**</span></span>
* <span data-ttu-id="b5add-331">**Úspěšné spuštění**</span><span class="sxs-lookup"><span data-stu-id="b5add-331">**Successful Runs**</span></span>

<span data-ttu-id="b5add-332">Tyto metriky jsou užitečné a umožňují získat přehled o celkové úspěšné a neúspěšné spuštění v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="b5add-332">These metrics are useful and help you to get an overview of overall failed and successful runs in the data factory.</span></span> <span data-ttu-id="b5add-333">Metriky jsou vydávány pokaždé, když je spustit řez.</span><span class="sxs-lookup"><span data-stu-id="b5add-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="b5add-334">Na začátku hodinu, jsou tyto metriky agregovat a instaluje do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b5add-334">At the beginning of the hour, these metrics are aggregated and pushed to your storage account.</span></span> <span data-ttu-id="b5add-335">Chcete-li povolit metriky, nastavte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b5add-335">To enable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="b5add-336">Povolit metriky</span><span class="sxs-lookup"><span data-stu-id="b5add-336">Enable metrics</span></span>
<span data-ttu-id="b5add-337">Pokud chcete povolit metriky, klikněte na následující z **Data Factory** okno:</span><span class="sxs-lookup"><span data-stu-id="b5add-337">To enable metrics, click the following from the **Data Factory** blade:</span></span>

<span data-ttu-id="b5add-338">**Monitorování** > **metrika** > **nastavení pro diagnostiku** > **diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="b5add-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Odkaz diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="b5add-340">Na **diagnostiky** okně klikněte na tlačítko **na**, vyberte účet úložiště a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b5add-340">On the **Diagnostics** blade, click **On**, select the storage account, and click **Save**.</span></span>

![Okno diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="b5add-342">Může trvat až jednu hodinu metrik, které mají být zobrazeny na **monitorování** okno protože metriky agregace se stane každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b5add-342">It might take up to one hour for the metrics to be visible on the **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="b5add-343">Nastavit výstrahy na metriky</span><span class="sxs-lookup"><span data-stu-id="b5add-343">Set up an alert on metrics</span></span>
<span data-ttu-id="b5add-344">Klikněte **metriky služby Data Factory** dlaždice:</span><span class="sxs-lookup"><span data-stu-id="b5add-344">Click the **Data Factory metrics** tile:</span></span>

![Dlaždice metriky objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="b5add-346">Na **metrika** okně klikněte na tlačítko **+ přidat upozornění** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="b5add-346">On the **Metric** blade, click **+ Add alert** on the toolbar.</span></span>
<span data-ttu-id="b5add-347">![Okno metriky objekt pro vytváření dat > Přidat upozornění](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="b5add-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="b5add-348">Na **přidání pravidla výstrahy** proveďte následující kroky a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5add-348">On the **Add an alert rule** page, do the following steps, and click **OK**.</span></span>

* <span data-ttu-id="b5add-349">Zadejte název pro výstrahu (Příklad: "se nezdařilo výstraha").</span><span class="sxs-lookup"><span data-stu-id="b5add-349">Enter a name for the alert (example: "failed alert").</span></span>
* <span data-ttu-id="b5add-350">Zadejte popis pro výstrahu (Příklad: "Odeslat e-mail, pokud dojde k selhání").</span><span class="sxs-lookup"><span data-stu-id="b5add-350">Enter a description for the alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="b5add-351">Vyberte metriku (vs "Spuštění se nezdařilo". "Úspěšně spuštěno").</span><span class="sxs-lookup"><span data-stu-id="b5add-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="b5add-352">Zadejte podmínku a prahová hodnota.</span><span class="sxs-lookup"><span data-stu-id="b5add-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="b5add-353">Zadejte časové období.</span><span class="sxs-lookup"><span data-stu-id="b5add-353">Specify the period of time.</span></span>
* <span data-ttu-id="b5add-354">Určete, zda se mají odesílat e-mailu vlastníci, přispěvatelé a čtenáři.</span><span class="sxs-lookup"><span data-stu-id="b5add-354">Specify whether an email should be sent to owners, contributors, and readers.</span></span>

![Okno metriky objekt pro vytváření dat > Přidat pravidlo výstrahy](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="b5add-356">Po pravidlo výstrahy bylo úspěšně přidáno, zavře okno a uvidíte nové výstrahy na **metrika** okno.</span><span class="sxs-lookup"><span data-stu-id="b5add-356">After the alert rule is added successfully, the blade closes and you see the new alert on the **Metric** blade.</span></span>

![Okno metriky objekt pro vytváření dat > Přidat nové výstrahy](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="b5add-358">Měli byste taky vidět počet výstrah v **výstrah pravidla** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="b5add-358">You should also see the number of alerts in the **Alert rules** tile.</span></span> <span data-ttu-id="b5add-359">Klikněte **výstrah pravidla** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="b5add-359">Click the **Alert rules** tile.</span></span>

![Data factory metriky okno - pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="b5add-361">Na **výstrahy pravidla** okno, zobrazit všechny existující výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b5add-361">On the **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="b5add-362">Chcete-li přidat výstrahu, klikněte na tlačítko **přidat upozornění** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="b5add-362">To add an alert, click **Add alert** on the toolbar.</span></span>

![Okno pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="b5add-364">Oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="b5add-364">Alert notifications</span></span>
<span data-ttu-id="b5add-365">Po pravidlo výstrahy odpovídá podmínku, měli byste obdržet e-mail s upozorněním, že se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b5add-365">After the alert rule matches the condition, you should get an email that says the alert is activated.</span></span> <span data-ttu-id="b5add-366">Jakmile je problém vyřešen a podmínka upozornění se neshoduje se už, získáte e-mail s upozorněním, že výstraha je vyřešený.</span><span class="sxs-lookup"><span data-stu-id="b5add-366">After the issue is resolved and the alert condition doesn’t match anymore, you get an email that says the alert is resolved.</span></span>

<span data-ttu-id="b5add-367">Toto chování se liší od událostí, kde jsou oznámení odesílána v každé selhání, který identifikuje pravidlo výstrahy pro.</span><span class="sxs-lookup"><span data-stu-id="b5add-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="b5add-368">Výstrahy nasazení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5add-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="b5add-369">Výstrahy metrik, které můžete nasadit stejně, jako je tomu u události.</span><span class="sxs-lookup"><span data-stu-id="b5add-369">You can deploy alerts for metrics the same way that you do for events.</span></span>

<span data-ttu-id="b5add-370">**Definice upozornění**</span><span class="sxs-lookup"><span data-stu-id="b5add-370">**Alert definition**</span></span>

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

<span data-ttu-id="b5add-371">Nahraďte *subscriptionId*, *resourceGroupName*, a *dataFactoryName* v ukázce s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b5add-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in the sample with appropriate values.</span></span>

<span data-ttu-id="b5add-372">*metricName* aktuálně podporuje dvě hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b5add-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="b5add-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="b5add-373">FailedRuns</span></span>
* <span data-ttu-id="b5add-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="b5add-374">SuccessfulRuns</span></span>

<span data-ttu-id="b5add-375">**Nasazení výstrahy**</span><span class="sxs-lookup"><span data-stu-id="b5add-375">**Deploy the alert**</span></span>

<span data-ttu-id="b5add-376">Nasadit výstrahy, použijte rutinu prostředí Azure PowerShell **New-AzureRmResourceGroupDeployment**, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b5add-376">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="b5add-377">Měli byste vidět následující zprávou po úspěšné nasazení:</span><span class="sxs-lookup"><span data-stu-id="b5add-377">You should see following message after a successful deployment:</span></span>

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

<span data-ttu-id="b5add-378">Můžete také **přidat AlertRule** nasadíte pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b5add-378">You can also use the **Add-AlertRule** cmdlet to deploy an alert rule.</span></span> <span data-ttu-id="b5add-379">Najdete v článku [přidat AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) téma podrobné informace a příklady.</span><span class="sxs-lookup"><span data-stu-id="b5add-379">See the [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a><span data-ttu-id="b5add-380">Přesunout objekt pro vytváření dat do jiné skupině prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="b5add-380">Move a data factory to a different resource group or subscription</span></span>
<span data-ttu-id="b5add-381">Objekt pro vytváření dat můžete přesunout na jinou skupinu prostředků nebo jiného předplatného pomocí **přesunout** příkazový řádek na domovské stránce objektu pro vytváření dat tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5add-381">You can move a data factory to a different resource group or a different subscription by using the **Move** command bar button on the home page of your data factory.</span></span>

![Přesunout objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="b5add-383">Také můžete přesunout všechny související prostředky (například výstrahy, které jsou přidruženy objektu pro vytváření dat), spolu s služby data factory.</span><span class="sxs-lookup"><span data-stu-id="b5add-383">You can also move any related resources (such as alerts that are associated with the data factory), along with the data factory.</span></span>

![Dialogové okno prostředků](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
