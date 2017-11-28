---
title: "aaaMonitor a Správa kanálů pomocí hello portál Azure a prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toouse hello portál Azure a prostředí Azure PowerShell toomonitor a spravovat hello Azure data Factory a kanály, které jste vytvořili."
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
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a><span data-ttu-id="a5228-103">Monitorování a Správa kanálů služby Azure Data Factory pomocí hello portál Azure a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5228-103">Monitor and manage Azure Data Factory pipelines by using hello Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5228-104">Použití Azure portal nebo Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5228-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="a5228-105">Pomocí monitorování a správu aplikací</span><span class="sxs-lookup"><span data-stu-id="a5228-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="a5228-106">monitorování a správu aplikace Hello poskytuje lepší podporu pro monitorování a správy datových kanálů a řešení potíží s problémy.</span><span class="sxs-lookup"><span data-stu-id="a5228-106">hello monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="a5228-107">Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5228-107">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="a5228-108">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí portálu Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5228-108">This article describes how toomonitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="a5228-109">Hello článku také obsahuje informace o tom, jak toocreate výstrahy a získat oznámení o selhání.</span><span class="sxs-lookup"><span data-stu-id="a5228-109">hello article also provides information on how toocreate alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="a5228-110">Pochopit kanály a aktivity stavy</span><span class="sxs-lookup"><span data-stu-id="a5228-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="a5228-111">Pomocí hello portálu Azure, můžete:</span><span class="sxs-lookup"><span data-stu-id="a5228-111">By using hello Azure portal, you can:</span></span>

* <span data-ttu-id="a5228-112">Objekt pro vytváření dat zobrazte jako diagram.</span><span class="sxs-lookup"><span data-stu-id="a5228-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="a5228-113">Zobrazit aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5228-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="a5228-114">Zobrazte vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="a5228-114">View input and output datasets.</span></span>

<span data-ttu-id="a5228-115">Tato část také popisuje, jak řez datovou sadu přechází z jednoho stavu tooanother.</span><span class="sxs-lookup"><span data-stu-id="a5228-115">This section also describes how a dataset slice transitions from one state tooanother state.</span></span>   

### <a name="navigate-tooyour-data-factory"></a><span data-ttu-id="a5228-116">Přejděte tooyour pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="a5228-116">Navigate tooyour data factory</span></span>
1. <span data-ttu-id="a5228-117">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a5228-117">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a5228-118">Klikněte na tlačítko **datové továrny** hello nabídky na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-118">Click **Data factories** on hello menu on hello left.</span></span> <span data-ttu-id="a5228-119">Pokud ho nevidíte, klikněte na tlačítko **další služby >**a potom klikněte na **datové továrny** pod hello **INTELLIGENCE + analýzy** kategorie.</span><span class="sxs-lookup"><span data-stu-id="a5228-119">If you don't see it, click **More services >**, and then click **Data factories** under hello **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Procházet všechny > datové továrny](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="a5228-121">Na hello **datové továrny** okně, vyberte hello datovou továrnu, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="a5228-121">On hello **Data factories** blade, select hello data factory that you're interested in.</span></span>

    ![Vyberte objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="a5228-123">Měli byste vidět hello Domovská stránka objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-123">You should see hello home page for hello data factory.</span></span>

   ![Okno objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="a5228-125">Zobrazení diagramu svojí datové továrny</span><span class="sxs-lookup"><span data-stu-id="a5228-125">Diagram view of your data factory</span></span>
<span data-ttu-id="a5228-126">Hello **Diagram** zobrazení objektu pro vytváření dat poskytuje jedno podokno pohotovostní toomonitor a spravovat hello data factory a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="a5228-126">hello **Diagram** view of a data factory provides a single pane of glass toomonitor and manage hello data factory and its assets.</span></span> <span data-ttu-id="a5228-127">toosee hello **Diagram** zobrazení objektu pro vytváření dat, klikněte na tlačítko **Diagram** na hello domovské stránce objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-127">toosee hello **Diagram** view of your data factory, click **Diagram** on hello home page for hello data factory.</span></span>

![Zobrazení diagramu](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="a5228-129">Přiblížení, oddálení, zvětšení toofit, % too100 přiblížení, uzamčení hello rozložení diagramu hello a automatické umísťování kanálů a datové sady.</span><span class="sxs-lookup"><span data-stu-id="a5228-129">You can zoom in, zoom out, zoom toofit, zoom too100%, lock hello layout of hello diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="a5228-130">Můžete také zjistit informace o rodokmenu dat hello (tedy zobrazit nadřazené a podřízené položky vybraných položek).</span><span class="sxs-lookup"><span data-stu-id="a5228-130">You can also see hello data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="a5228-131">Aktivity v kanálu</span><span class="sxs-lookup"><span data-stu-id="a5228-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="a5228-132">Klikněte pravým tlačítkem na hello kanál a pak klikněte na **otevřít kanál** toosee všechny aktivity v hello kanálu spolu s vstupní a výstupní datové sady pro hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="a5228-132">Right-click hello pipeline, and then click **Open pipeline** toosee all activities in hello pipeline, along with input and output datasets for hello activities.</span></span> <span data-ttu-id="a5228-133">Tato funkce je užitečná, pokud vaše kanál obsahuje více než jednu aktivitu a chcete toounderstand hello provozní rodokmenu jednoho kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5228-133">This feature is useful when your pipeline includes more than one activity and you want toounderstand hello operational lineage of a single pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="a5228-135">V následujícím příkladu hello najdete v části aktivity kopírování v kanálu hello s vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="a5228-135">In hello following example, you see a copy activity in hello pipeline with an input and an output.</span></span> 

    ![Aktivity v kanálu](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="a5228-137">Můžete přejít zpět toohello Domovská stránka objektu pro vytváření dat hello kliknutím hello **objekt pro vytváření dat** odkaz v hello s popisem cesty v levém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-137">You can navigate back toohello home page of hello data factory by clicking hello **Data factory** link in hello breadcrumb at hello top-left corner.</span></span>

    ![Přejděte zpět toodata factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="a5228-139">Zobrazení stavu hello každé aktivity v kanálu</span><span class="sxs-lookup"><span data-stu-id="a5228-139">View hello state of each activity inside a pipeline</span></span>
<span data-ttu-id="a5228-140">Můžete zobrazit aktuální stav hello aktivity zobrazením hello stav každého hello datové sady, které vytváří aktivitou hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-140">You can view hello current state of an activity by viewing hello status of any of hello datasets that are produced by hello activity.</span></span>

<span data-ttu-id="a5228-141">Dvojitým kliknutím na soubor hello **OutputBlobTable** v hello **Diagram**, zobrazí se všechny hello datové řezy, které vznikají pomocí funkcí spustí jinou aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5228-141">By double-clicking hello **OutputBlobTable** in hello **Diagram**, you can see all hello slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="a5228-142">Uvidíte, že aktivity kopírování hello proběhla úspěšně pro hello posledních 8 hodin a vytvořeného hello řezy v hello **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="a5228-142">You can see that hello copy activity ran successfully for hello last eight hours and produced hello slices in hello **Ready** state.</span></span>  

![Stav hello kanálu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="a5228-144">Hello řezy datovou sadu v datové továrně hello může mít jednu z hello následující stavy:</span><span class="sxs-lookup"><span data-stu-id="a5228-144">hello dataset slices in hello data factory can have one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="a5228-145">Stav</span><span class="sxs-lookup"><span data-stu-id="a5228-145">State</span></span></th><th align="left"><span data-ttu-id="a5228-146">Dílčím stavem</span><span class="sxs-lookup"><span data-stu-id="a5228-146">Substate</span></span></th><th align="left"><span data-ttu-id="a5228-147">Popis</span><span class="sxs-lookup"><span data-stu-id="a5228-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="a5228-148">Čekání</span><span class="sxs-lookup"><span data-stu-id="a5228-148">Waiting</span></span></td><td><span data-ttu-id="a5228-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="a5228-149">ScheduleTime</span></span></td><td><span data-ttu-id="a5228-150">pro toorun hello řezu ještě nenastal čas Hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-150">hello time hasn't come for hello slice toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="a5228-151">DatasetDependencies</span></span></td><td><span data-ttu-id="a5228-152">Hello upstreamové závislosti nejsou připravené.</span><span class="sxs-lookup"><span data-stu-id="a5228-152">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="a5228-153">ComputeResources</span></span></td><td><span data-ttu-id="a5228-154">Hello výpočetní prostředky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a5228-154">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="a5228-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="a5228-156">Všechny instance aktivit hello jsou právě zpracovávají jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="a5228-156">All hello activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="a5228-157">ActivityResume</span></span></td><td><span data-ttu-id="a5228-158">Hello aktivita je pozastavená a hello řezy nelze spustit, dokud je obnoveno hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="a5228-158">hello activity is paused and can't run hello slices until hello activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-159">Opakování</span><span class="sxs-lookup"><span data-stu-id="a5228-159">Retry</span></span></td><td><span data-ttu-id="a5228-160">Probíhá pokus o spuštění aktivity je zopakován.</span><span class="sxs-lookup"><span data-stu-id="a5228-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-161">Ověření</span><span class="sxs-lookup"><span data-stu-id="a5228-161">Validation</span></span></td><td><span data-ttu-id="a5228-162">Ověření se ještě nespustilo.</span><span class="sxs-lookup"><span data-stu-id="a5228-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="a5228-163">ValidationRetry</span></span></td><td><span data-ttu-id="a5228-164">Ověření je čekání toobe opakovat.</span><span class="sxs-lookup"><span data-stu-id="a5228-164">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="a5228-165">InProgress</span><span class="sxs-lookup"><span data-stu-id="a5228-165">InProgress</span></span></td><td><span data-ttu-id="a5228-166">Probíhá ověřování</span><span class="sxs-lookup"><span data-stu-id="a5228-166">Validating</span></span></td><td><span data-ttu-id="a5228-167">Probíhá ověřování.</span><span class="sxs-lookup"><span data-stu-id="a5228-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="a5228-168">Hello řez se zpracovává.</span><span class="sxs-lookup"><span data-stu-id="a5228-168">hello slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="a5228-169">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="a5228-169">Failed</span></span></td><td><span data-ttu-id="a5228-170">TimedOut</span><span class="sxs-lookup"><span data-stu-id="a5228-170">TimedOut</span></span></td><td><span data-ttu-id="a5228-171">provedení aktivity Hello trvalo déle, než je povolené aktivitou hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-171">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-172">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="a5228-172">Canceled</span></span></td><td><span data-ttu-id="a5228-173">řez Hello zrušil akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5228-173">hello slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-174">Ověření</span><span class="sxs-lookup"><span data-stu-id="a5228-174">Validation</span></span></td><td><span data-ttu-id="a5228-175">Ověření se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="a5228-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="a5228-176">řez Hello se nezdařilo toobe vygenerovat nebo ověřit.</span><span class="sxs-lookup"><span data-stu-id="a5228-176">hello slice failed toobe generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="a5228-177">Připraveno</span><span class="sxs-lookup"><span data-stu-id="a5228-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="a5228-178">Hello řez je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="a5228-178">hello slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-179">Přeskočena</span><span class="sxs-lookup"><span data-stu-id="a5228-179">Skipped</span></span></td><td><span data-ttu-id="a5228-180">Žádný</span><span class="sxs-lookup"><span data-stu-id="a5228-180">None</span></span></td><td><span data-ttu-id="a5228-181">Hello řez se zpracovává.</span><span class="sxs-lookup"><span data-stu-id="a5228-181">hello slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a5228-182">Žádný</span><span class="sxs-lookup"><span data-stu-id="a5228-182">None</span></span></td><td>-</td><td><span data-ttu-id="a5228-183">Řez používá tooexist jiný stav, ale byla obnovena.</span><span class="sxs-lookup"><span data-stu-id="a5228-183">A slice used tooexist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="a5228-184">Hello podrobnosti o řez lze zobrazit kliknutím na položku řez na hello **nedávno aktualizován řezy** okno.</span><span class="sxs-lookup"><span data-stu-id="a5228-184">You can view hello details about a slice by clicking a slice entry on hello **Recently Updated Slices** blade.</span></span>

![Řez podrobnosti](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="a5228-186">Pokud hello řez provedl vícekrát, zobrazí více řádků v hello **aktivita spuštěna** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a5228-186">If hello slice has been executed multiple times, you see multiple rows in hello **Activity runs** list.</span></span> <span data-ttu-id="a5228-187">Můžete zobrazit podrobnosti o aktivitě spustíte kliknutím na položku hello spustit v hello **aktivita spuštěna** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a5228-187">You can view details about an activity run by clicking hello run entry in hello **Activity runs** list.</span></span> <span data-ttu-id="a5228-188">Hello seznamu jsou uvedeny všechny soubory protokolu hello, spolu s chybovou zprávu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="a5228-188">hello list shows all hello log files, along with an error message if there is one.</span></span> <span data-ttu-id="a5228-189">Tato funkce je užitečná protokoly tooview a ladění bez nutnosti tooleave datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="a5228-189">This feature is useful tooview and debug logs without having tooleave your data factory.</span></span>

![Podrobnosti o spuštění aktivit](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="a5228-191">Není-li řez hello v hello **připraven** stavu, můžete zobrazit upstreamové datové řezy hello, které nejsou připravené a které blokují hello aktuálního řezu v hello **Upstreamové datové řezy, které nejsou připraveny** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a5228-191">If hello slice isn't in hello **Ready** state, you can see hello upstream slices that aren't ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="a5228-192">Tato funkce je užitečná, když vaše řez v **čekání** stavu a chcete, aby hello toounderstand nadřazeného se závislosti, které hello řez čeká na.</span><span class="sxs-lookup"><span data-stu-id="a5228-192">This feature is useful when your slice is in **Waiting** state and you want toounderstand hello upstream dependencies that hello slice is waiting on.</span></span>

![Upstreamové datové řezy, které nejsou připraveny](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="a5228-194">Diagram stavu datové sady</span><span class="sxs-lookup"><span data-stu-id="a5228-194">Dataset state diagram</span></span>
<span data-ttu-id="a5228-195">Po nasazení služby data factory a kanály hello mají platný aktivní období, datová sada hello řezy přechod z jednoho stavu tooanother.</span><span class="sxs-lookup"><span data-stu-id="a5228-195">After you deploy a data factory and hello pipelines have a valid active period, hello dataset slices transition from one state tooanother.</span></span> <span data-ttu-id="a5228-196">V současné době stav řezu hello dodržuje hello následující diagram stavu:</span><span class="sxs-lookup"><span data-stu-id="a5228-196">Currently, hello slice status follows hello following state diagram:</span></span>

![Diagram stavu](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="a5228-198">Hello tok přechod stavu datovou sadu v datové továrně je hello následující: Čekání na -> průběh/v – probíhající (ověřování) -> Připraveno nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="a5228-198">hello dataset state transition flow in data factory is hello following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="a5228-199">řez Hello se spustí v **čekání** stavu čekání toobe předběžné podmínky splněny, než se provede.</span><span class="sxs-lookup"><span data-stu-id="a5228-199">hello slice starts in a **Waiting** state, waiting for preconditions toobe met before it executes.</span></span> <span data-ttu-id="a5228-200">Pak hello aktivity spustí provádění a hello řez přejde do **probíhající** stavu.</span><span class="sxs-lookup"><span data-stu-id="a5228-200">Then, hello activity starts executing, and hello slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="a5228-201">provedení aktivity Hello může úspěch nebo neúspěch.</span><span class="sxs-lookup"><span data-stu-id="a5228-201">hello activity execution might succeed or fail.</span></span> <span data-ttu-id="a5228-202">Hello řez je označena jako **připraven** nebo **se nezdařilo**, podle hello výsledek spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-202">hello slice is marked as **Ready** or **Failed**, based on hello result of hello execution.</span></span>

<span data-ttu-id="a5228-203">Můžete resetovat hello řez toogo zpět z hello **připraven** nebo **se nezdařilo** stavu toohello **čekání** stavu.</span><span class="sxs-lookup"><span data-stu-id="a5228-203">You can reset hello slice toogo back from hello **Ready** or **Failed** state toohello **Waiting** state.</span></span> <span data-ttu-id="a5228-204">Můžete také označit stav řezu hello příliš**přeskočit**, která brání hello aktivity z provádění a není zpracování řezu hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-204">You can also mark hello slice state too**Skip**, which prevents hello activity from executing and not processing hello slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="a5228-205">Pozastavení a obnovení kanálů</span><span class="sxs-lookup"><span data-stu-id="a5228-205">Pause and resume pipelines</span></span>
<span data-ttu-id="a5228-206">Kanály můžete spravovat pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5228-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="a5228-207">Například můžete pozastavit a obnovit kanály spuštěním rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5228-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="a5228-208">zobrazení diagramu Hello nepodporuje pozastavení a obnovení kanály.</span><span class="sxs-lookup"><span data-stu-id="a5228-208">hello diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="a5228-209">Pokud chcete toouse uživatelské rozhraní, použijte hello sledování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="a5228-209">If you want toouse an user interface, use hello monitoring and managing application.</span></span> <span data-ttu-id="a5228-210">Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a5228-210">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="a5228-211">Je možné pozastavit nebo pozastavit kanály pomocí hello **Suspend-AzureRmDataFactoryPipeline** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5228-211">You can pause/suspend pipelines by using hello **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="a5228-212">Tato rutina je užitečná, když nechcete, aby toorun kanály dokud nebude problém vyřešen.</span><span class="sxs-lookup"><span data-stu-id="a5228-212">This cmdlet is useful when you don’t want toorun your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="a5228-213">Například:</span><span class="sxs-lookup"><span data-stu-id="a5228-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="a5228-214">Po napravení problému hello s hello kanálu, můžete obnovit hello pozastaveno kanálu tak, že spustíte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="a5228-214">After hello issue has been fixed with hello pipeline, you can resume hello suspended pipeline by running hello following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="a5228-215">Například:</span><span class="sxs-lookup"><span data-stu-id="a5228-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="a5228-216">Ladit kanály</span><span class="sxs-lookup"><span data-stu-id="a5228-216">Debug pipelines</span></span>
<span data-ttu-id="a5228-217">Azure Data Factory poskytuje bohaté možnosti pro vás toodebug a řešení potíží kanály pomocí hello portál Azure a prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5228-217">Azure Data Factory provides rich capabilities for you toodebug and troubleshoot pipelines by using hello Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="a5228-218">[! Poznámka:} je mnohem snazší tootroubleshot, které chyb s použitím hello monitorování aplikace a správu.</span><span class="sxs-lookup"><span data-stu-id="a5228-218">[!NOTE} It is much easier tootroubleshot errors using hello Monitoring & Management App.</span></span> <span data-ttu-id="a5228-219">Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a5228-219">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="a5228-220">Vyhledejte chyby v kanálu</span><span class="sxs-lookup"><span data-stu-id="a5228-220">Find errors in a pipeline</span></span>
<span data-ttu-id="a5228-221">Pokud se nezdaří hello aktivity při spuštění v kanálu, hello datovou sadu, která je vytvořena hello kanálu je v chybovém stavu z důvodu selhání hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-221">If hello activity run fails in a pipeline, hello dataset that is produced by hello pipeline is in an error state because of hello failure.</span></span> <span data-ttu-id="a5228-222">Můžete ladění a odstraňování chyb v Azure Data Factory pomocí hello následující metody.</span><span class="sxs-lookup"><span data-stu-id="a5228-222">You can debug and troubleshoot errors in Azure Data Factory by using hello following methods.</span></span>

#### <a name="use-hello-azure-portal-toodebug-an-error"></a><span data-ttu-id="a5228-223">Použít hello Azure portálu toodebug chybu</span><span class="sxs-lookup"><span data-stu-id="a5228-223">Use hello Azure portal toodebug an error</span></span>
1. <span data-ttu-id="a5228-224">Na hello **tabulky** okně klikněte na tlačítko hello problém řez, který má hello **stav** nastavit příliš**se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="a5228-224">On hello **Table** blade, click hello problem slice that has hello **Status** set too**Failed**.</span></span>

   ![Okno tabulky s řez problém](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="a5228-226">Na hello **datový řez** okně klikněte na tlačítko hello aktivity při spuštění, který selhal.</span><span class="sxs-lookup"><span data-stu-id="a5228-226">On hello **Data slice** blade, click hello activity run that failed.</span></span>

   ![Datový řez s chybou](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="a5228-228">Na hello **podrobnosti o spuštění aktivit** okně si můžete stáhnout hello soubory, které jsou spojeny s HDInsight zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-228">On hello **Activity run details** blade, you can download hello files that are associated with hello HDInsight processing.</span></span> <span data-ttu-id="a5228-229">Klikněte na tlačítko **Stáhnout** pro stav nebo stderr toodownload hello Chyba souboru protokolu, který obsahuje podrobnosti o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-229">Click **Download** for Status/stderr toodownload hello error log file that contains details about hello error.</span></span>

   ![Aktivity při spuštění podrobnosti okno s chybou](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a><span data-ttu-id="a5228-231">Pomocí prostředí PowerShell toodebug chybu</span><span class="sxs-lookup"><span data-stu-id="a5228-231">Use PowerShell toodebug an error</span></span>
1. <span data-ttu-id="a5228-232">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a5228-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="a5228-233">Spustit hello **Get-AzureRmDataFactorySlice** příkaz toosee hello řezy a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="a5228-233">Run hello **Get-AzureRmDataFactorySlice** command toosee hello slices and their statuses.</span></span> <span data-ttu-id="a5228-234">Měli byste vidět řez hello stav **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="a5228-234">You should see a slice with hello status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="a5228-235">Například:</span><span class="sxs-lookup"><span data-stu-id="a5228-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="a5228-236">Nahraďte **StartDateTime** se časem spuštění vaší kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5228-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="a5228-237">Nyní, spustit hello **Get-AzureRmDataFactoryRun** rutiny tooget podrobnosti o aktivitě hello spustit pro řez hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-237">Now, run hello **Get-AzureRmDataFactoryRun** cmdlet tooget details about hello activity run for hello slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="a5228-238">Například:</span><span class="sxs-lookup"><span data-stu-id="a5228-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="a5228-239">Hello hodnota StartDateTime je čas spuštění hello hello chyby nebo problému řez, který jste si poznamenali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-239">hello value of StartDateTime is hello start time for hello error/problem slice that you noted from hello previous step.</span></span> <span data-ttu-id="a5228-240">Hello datum a čas by měl být uzavřena do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="a5228-240">hello date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="a5228-241">Měli byste vidět výstup s podrobnostmi o chybě hello, který je podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="a5228-241">You should see output with details about hello error that is similar toohello following:</span></span>

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
5. <span data-ttu-id="a5228-242">Můžete spustit hello **uložit AzureRmDataFactoryLog** rutiny s hello hodnota Id najdete v části z výstupu hello a stažení souborů protokolu hello pomocí hello **- DownloadLogsoption** pro rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-242">You can run hello **Save-AzureRmDataFactoryLog** cmdlet with hello Id value that you see from hello output, and download hello log files by using hello **-DownloadLogsoption** for hello cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="a5228-243">Znovu spustit chyby v kanálu</span><span class="sxs-lookup"><span data-stu-id="a5228-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5228-244">Je snazší tootroubleshoot chyby a znovu spusťte selhání řezy pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="a5228-244">It's easier tootroubleshoot errors and rerun failed slices by using hello Monitoring & Management App.</span></span> <span data-ttu-id="a5228-245">Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5228-245">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-hello-azure-portal"></a><span data-ttu-id="a5228-246">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a5228-246">Use hello Azure portal</span></span>
<span data-ttu-id="a5228-247">Po řešení potíží a ladění chyby v kanálu se může znovu selhání navigace toohello chyba řez a kliknutím na hello **spustit** tlačítka na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating toohello error slice and clicking hello **Run** button on hello command bar.</span></span>

![Opětovné spuštění neúspěšné řez](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="a5228-249">V případě hello řezu selhalo ověření z důvodu selhání zásad (například pokud data nejsou k dispozici), můžete opravit chyby hello a znovu ověřit kliknutím hello **ověřením** tlačítka na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-249">In case hello slice has failed validation because of a policy failure (for example, if data isn't available), you can fix hello failure and validate again by clicking hello **Validate** button on hello command bar.</span></span>

![Opravte chyby a ověření](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="a5228-251">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="a5228-251">Use Azure PowerShell</span></span>
<span data-ttu-id="a5228-252">Selhání může znovu pomocí hello **Set-AzureRmDataFactorySliceStatus** rutiny.</span><span class="sxs-lookup"><span data-stu-id="a5228-252">You can rerun failures by using hello **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="a5228-253">V tématu hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) téma pro syntaxi a další podrobnosti o rutině hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-253">See hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about hello cmdlet.</span></span>

<span data-ttu-id="a5228-254">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="a5228-254">**Example:**</span></span>

<span data-ttu-id="a5228-255">Hello následující příklad ilustruje hello stav všech řezech pro hello tabulka 'DAWikiAggregatedData' too'Waiting' v Azure data factory hello 'WikiADF'.</span><span class="sxs-lookup"><span data-stu-id="a5228-255">hello following example sets hello status of all slices for hello table 'DAWikiAggregatedData' too'Waiting' in hello Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="a5228-256">Hello 'typ aktualizace, je nastaven too'UpstreamInPipeline ', což znamená, že stavy každý řez hello tabulky a všechny hello závislé (nadřazený) tabulky nastavené too'Waiting'.</span><span class="sxs-lookup"><span data-stu-id="a5228-256">hello 'UpdateType' is set too'UpstreamInPipeline', which means that statuses of each slice for hello table and all hello dependent (upstream) tables are set too'Waiting'.</span></span> <span data-ttu-id="a5228-257">Hello jiných možná hodnota pro tento parametr je "Individuální".</span><span class="sxs-lookup"><span data-stu-id="a5228-257">hello other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="a5228-258">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="a5228-258">Create alerts</span></span>
<span data-ttu-id="a5228-259">Azure protokoly událostí uživatele při prostředků Azure (například objekt pro vytváření dat) je vytvořen, aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="a5228-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="a5228-260">Výstrahy můžete vytvořit na těchto událostech.</span><span class="sxs-lookup"><span data-stu-id="a5228-260">You can create alerts on these events.</span></span> <span data-ttu-id="a5228-261">Můžete použít různé metriky toocapture objekt pro vytváření dat a vytvářet výstrahy o metrikách.</span><span class="sxs-lookup"><span data-stu-id="a5228-261">You can use Data Factory toocapture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="a5228-262">Doporučujeme použít události pro monitorování v reálném čase a použít metriky pro účely záznamu historie událostí.</span><span class="sxs-lookup"><span data-stu-id="a5228-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="a5228-263">Výstrahy na události</span><span class="sxs-lookup"><span data-stu-id="a5228-263">Alerts on events</span></span>
<span data-ttu-id="a5228-264">Azure události poskytují užitečné přehledy co se děje v vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a5228-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="a5228-265">Pokud používáte Azure Data Factory, události se generují při:</span><span class="sxs-lookup"><span data-stu-id="a5228-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="a5228-266">Objekt pro vytváření dat je vytvořen, aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="a5228-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="a5228-267">Zpracování dat (jako "spuštěno") je spuštěna, nebo byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="a5228-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="a5228-268">Vytvoření clusteru HDInsight na vyžádání nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="a5228-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="a5228-269">Můžete vytvářet výstrahy na těchto událostech uživatele a jejich konfigurace toosend e-mailové oznámení toohello správce a coadministrators hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="a5228-269">You can create alerts on these user events and configure them toosend email notifications toohello administrator and coadministrators of hello subscription.</span></span> <span data-ttu-id="a5228-270">Kromě toho můžete zadat další e-mailové adresy uživatelů, kteří potřebují tooreceive e-mailová oznámení, pokud jsou splněny podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-270">In addition, you can specify additional email addresses of users who need tooreceive email notifications when hello conditions are met.</span></span> <span data-ttu-id="a5228-271">Tato funkce je užitečná, když chcete tooget upozornění na selhání a nechcete, aby toocontinuously monitorování vaší služby data factory.</span><span class="sxs-lookup"><span data-stu-id="a5228-271">This feature is useful when you want tooget notified on failures and don’t want toocontinuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="a5228-272">V současné době nepodporuje hello portálu zobrazit výstrahy na události.</span><span class="sxs-lookup"><span data-stu-id="a5228-272">Currently, hello portal doesn't show alerts on events.</span></span> <span data-ttu-id="a5228-273">Použití hello [monitorování a správu aplikace](data-factory-monitor-manage-app.md) toosee všechny výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5228-273">Use hello [Monitoring and Management app](data-factory-monitor-manage-app.md) toosee all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="a5228-274">Zadejte definici výstrah</span><span class="sxs-lookup"><span data-stu-id="a5228-274">Specify an alert definition</span></span>
<span data-ttu-id="a5228-275">toospecify výstrahy definice, vytvořte soubor JSON, který popisuje hello operace, které chcete toobe upozorněni na.</span><span class="sxs-lookup"><span data-stu-id="a5228-275">toospecify an alert definition, you create a JSON file that describes hello operations that you want toobe alerted on.</span></span> <span data-ttu-id="a5228-276">V následujícím příkladu hello odešle výstrahu hello e-mailové oznámení pro hello RunFinished operaci.</span><span class="sxs-lookup"><span data-stu-id="a5228-276">In hello following example, hello alert sends an email notification for hello RunFinished operation.</span></span> <span data-ttu-id="a5228-277">konkrétní toobe, e-mailových oznámení je odeslána při spuštění v datové továrně hello bylo dokončeno a hello spuštění se nezdařilo (stav = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="a5228-277">toobe specific, an email notification is sent when a run in hello data factory has completed and hello run has failed (Status = FailedExecution).</span></span>

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
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
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

<span data-ttu-id="a5228-278">Můžete odebrat **subStatus** z hello definici JSON, pokud nechcete, aby toobe upozorněni na konkrétní chyby.</span><span class="sxs-lookup"><span data-stu-id="a5228-278">You can remove **subStatus** from hello JSON definition if you don’t want toobe alerted on a specific failure.</span></span>

<span data-ttu-id="a5228-279">Tento příklad nastaví hello upozornění pro všechny datové továrny v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="a5228-279">This example sets up hello alert for all data factories in your subscription.</span></span> <span data-ttu-id="a5228-280">Pokud chcete nastavit pro objekt pro vytváření dat konkrétní výstrahu toobe hello, můžete zadat objekt pro vytváření dat **resourceUri** v hello **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="a5228-280">If you want hello alert toobe set up for a particular data factory, you can specify data factory **resourceUri** in hello **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="a5228-281">Hello následující tabulka obsahuje seznam hello dostupné operace a stavy (a dílčí stavy).</span><span class="sxs-lookup"><span data-stu-id="a5228-281">hello following table provides hello list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="a5228-282">Název operace</span><span class="sxs-lookup"><span data-stu-id="a5228-282">Operation name</span></span> | <span data-ttu-id="a5228-283">Status</span><span class="sxs-lookup"><span data-stu-id="a5228-283">Status</span></span> | <span data-ttu-id="a5228-284">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="a5228-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5228-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="a5228-285">RunStarted</span></span> |<span data-ttu-id="a5228-286">spuštění</span><span class="sxs-lookup"><span data-stu-id="a5228-286">Started</span></span> |<span data-ttu-id="a5228-287">Spouštění</span><span class="sxs-lookup"><span data-stu-id="a5228-287">Starting</span></span> |
| <span data-ttu-id="a5228-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="a5228-288">RunFinished</span></span> |<span data-ttu-id="a5228-289">Nemohl / bylo úspěšné</span><span class="sxs-lookup"><span data-stu-id="a5228-289">Failed / Succeeded</span></span> |<span data-ttu-id="a5228-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="a5228-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="a5228-291">Úspěch</span><span class="sxs-lookup"><span data-stu-id="a5228-291">Succeeded</span></span><br/><br/><span data-ttu-id="a5228-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="a5228-292">FailedExecution</span></span><br/><br/><span data-ttu-id="a5228-293">TimedOut</span><span class="sxs-lookup"><span data-stu-id="a5228-293">TimedOut</span></span><br/><br/><span data-ttu-id="a5228-294">< zrušena</span><span class="sxs-lookup"><span data-stu-id="a5228-294"><Canceled</span></span><br/><br/><span data-ttu-id="a5228-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="a5228-295">FailedValidation</span></span><br/><br/><span data-ttu-id="a5228-296">opuštění</span><span class="sxs-lookup"><span data-stu-id="a5228-296">Abandoned</span></span> |
| <span data-ttu-id="a5228-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="a5228-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="a5228-298">spuštění</span><span class="sxs-lookup"><span data-stu-id="a5228-298">Started</span></span> | |
| <span data-ttu-id="a5228-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="a5228-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="a5228-300">Úspěch</span><span class="sxs-lookup"><span data-stu-id="a5228-300">Succeeded</span></span> | |
| <span data-ttu-id="a5228-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="a5228-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="a5228-302">Úspěch</span><span class="sxs-lookup"><span data-stu-id="a5228-302">Succeeded</span></span> | |

<span data-ttu-id="a5228-303">V tématu [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) podrobnosti o hello elementy JSON, které se používají v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about hello JSON elements that are used in hello example.</span></span>

#### <a name="deploy-hello-alert"></a><span data-ttu-id="a5228-304">Nasazení hello výstrahy</span><span class="sxs-lookup"><span data-stu-id="a5228-304">Deploy hello alert</span></span>
<span data-ttu-id="a5228-305">toodeploy hello výstrahy, použijte rutinu prostředí Azure PowerShell hello **New-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="a5228-305">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="a5228-306">Po nasazení skupiny prostředků hello byl úspěšně dokončen, zobrazí hello následující zprávy:</span><span class="sxs-lookup"><span data-stu-id="a5228-306">After hello resource group deployment has finished successfully, you see hello following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
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
> <span data-ttu-id="a5228-307">Můžete použít hello [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5228-307">You can use hello [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate an alert rule.</span></span> <span data-ttu-id="a5228-308">datová část JSON Hello je podobný příklad toohello JSON.</span><span class="sxs-lookup"><span data-stu-id="a5228-308">hello JSON payload is similar toohello JSON example.</span></span>  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a><span data-ttu-id="a5228-309">Načtení seznamu hello nasazení skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a5228-309">Retrieve hello list of Azure resource group deployments</span></span>
<span data-ttu-id="a5228-310">seznam hello tooretrieve nasazené nasazení skupiny prostředků Azure, použijte rutinu hello **Get-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="a5228-310">tooretrieve hello list of deployed Azure resource group deployments, use hello cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

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

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="a5228-311">Řešení potíží s událostmi uživatele</span><span class="sxs-lookup"><span data-stu-id="a5228-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="a5228-312">Zobrazí všechny události hello, které jsou generovány po kliknutí na hello **metriky a operace** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a5228-312">You can see all hello events that are generated after clicking hello **Metrics and operations** tile.</span></span>

    ![Dlaždice metriky a operace](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="a5228-314">Klikněte na tlačítko hello **události** dlaždici toosee hello události.</span><span class="sxs-lookup"><span data-stu-id="a5228-314">Click hello **Events** tile toosee hello events.</span></span>

    ![Dlaždice události](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="a5228-316">Na hello **události** okno, můžete zobrazit podrobnosti o události, filtrované události a tak dále.</span><span class="sxs-lookup"><span data-stu-id="a5228-316">On hello **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Okno události](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="a5228-318">Klikněte na tlačítko **operace** v seznamu hello operace, která způsobuje chybu.</span><span class="sxs-lookup"><span data-stu-id="a5228-318">Click an **Operation** in hello operations list that causes an error.</span></span>

    ![Vyberte operaci](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="a5228-320">Klikněte na tlačítko **chyba** událostí toosee podrobnosti o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-320">Click an **Error** event toosee details about hello error.</span></span>

    ![Chyba události](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="a5228-322">V tématu [rutiny Azure přehledy](https://msdn.microsoft.com/library/mt282452.aspx) pro rutiny prostředí PowerShell, které můžete použít tooadd, získat, nebo odeberte výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5228-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use tooadd, get, or remove alerts.</span></span> <span data-ttu-id="a5228-323">Tady je několik příkladů použití hello **Get-AlertRule** rutiny:</span><span class="sxs-lookup"><span data-stu-id="a5228-323">Here are a few examples of using hello **Get-AlertRule** cmdlet:</span></span>

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
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
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

<span data-ttu-id="a5228-324">Spusťte následující podrobnosti toosee příkazy get-help a příklady pro rutinu Get-AlertRule hello hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-324">Run hello following get-help commands toosee details and examples for hello Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="a5228-325">Pokud se zobrazí události hello generování výstrah v hello okno portálu, ale nebudete dostávat e-mailová oznámení, zkontrolujte, zda text hello e-mailovou adresu, která je zadána je nastaven tooreceive e-mailů externích odesílatelů.</span><span class="sxs-lookup"><span data-stu-id="a5228-325">If you see hello alert generation events on hello portal blade but you don't receive email notifications, check whether hello email address that is specified is set tooreceive emails from external senders.</span></span> <span data-ttu-id="a5228-326">výstrahy e-mailů Hello může mít zablokovaný nastavení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5228-326">hello alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="a5228-327">Výstrahy na metriky</span><span class="sxs-lookup"><span data-stu-id="a5228-327">Alerts on metrics</span></span>
<span data-ttu-id="a5228-328">V objektu pro vytváření dat můžete zachytit různé metriky a vytvořit oznámení o metrikách.</span><span class="sxs-lookup"><span data-stu-id="a5228-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="a5228-329">Můžete sledovat a vytvářet upozornění na následující metriky hello řezy v datové továrně hello:</span><span class="sxs-lookup"><span data-stu-id="a5228-329">You can monitor and create alerts on hello following metrics for hello slices in your data factory:</span></span>

* <span data-ttu-id="a5228-330">**Spustí se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="a5228-330">**Failed Runs**</span></span>
* <span data-ttu-id="a5228-331">**Úspěšné spuštění**</span><span class="sxs-lookup"><span data-stu-id="a5228-331">**Successful Runs**</span></span>

<span data-ttu-id="a5228-332">Tyto metriky jsou užitečné a vám pomohou tooget přehled celkového úspěšné a neúspěšné spustí v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-332">These metrics are useful and help you tooget an overview of overall failed and successful runs in hello data factory.</span></span> <span data-ttu-id="a5228-333">Metriky jsou vydávány pokaždé, když je spustit řez.</span><span class="sxs-lookup"><span data-stu-id="a5228-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="a5228-334">Tyto metriky od začátku hello hello hodina, jsou agregovat a nabídnutých tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5228-334">At hello beginning of hello hour, these metrics are aggregated and pushed tooyour storage account.</span></span> <span data-ttu-id="a5228-335">metriky tooenable nastavit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5228-335">tooenable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="a5228-336">Povolit metriky</span><span class="sxs-lookup"><span data-stu-id="a5228-336">Enable metrics</span></span>
<span data-ttu-id="a5228-337">tooenable metriky, klikněte na následující hello z hello **Data Factory** okno:</span><span class="sxs-lookup"><span data-stu-id="a5228-337">tooenable metrics, click hello following from hello **Data Factory** blade:</span></span>

<span data-ttu-id="a5228-338">**Monitorování** > **metrika** > **nastavení pro diagnostiku** > **diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="a5228-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Odkaz diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="a5228-340">Na hello **diagnostiky** okně klikněte na tlačítko **na**, vyberte účet úložiště hello a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a5228-340">On hello **Diagnostics** blade, click **On**, select hello storage account, and click **Save**.</span></span>

![Okno diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="a5228-342">To může trvat až hodinu tooone toobe metriky hello viditelný na hello **monitorování** okno protože metriky agregace se stane každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a5228-342">It might take up tooone hour for hello metrics toobe visible on hello **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="a5228-343">Nastavit výstrahy na metriky</span><span class="sxs-lookup"><span data-stu-id="a5228-343">Set up an alert on metrics</span></span>
<span data-ttu-id="a5228-344">Klikněte na tlačítko hello **metriky služby Data Factory** dlaždice:</span><span class="sxs-lookup"><span data-stu-id="a5228-344">Click hello **Data Factory metrics** tile:</span></span>

![Dlaždice metriky objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="a5228-346">Na hello **metrika** okně klikněte na tlačítko **+ přidat upozornění** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-346">On hello **Metric** blade, click **+ Add alert** on hello toolbar.</span></span>
<span data-ttu-id="a5228-347">![Okno metriky objekt pro vytváření dat > Přidat upozornění](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="a5228-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="a5228-348">Na hello **přidání pravidla výstrahy** proveďte hello následující kroky a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a5228-348">On hello **Add an alert rule** page, do hello following steps, and click **OK**.</span></span>

* <span data-ttu-id="a5228-349">Zadejte název pro výstrahu hello (Příklad: "se nezdařilo výstraha").</span><span class="sxs-lookup"><span data-stu-id="a5228-349">Enter a name for hello alert (example: "failed alert").</span></span>
* <span data-ttu-id="a5228-350">Zadejte popis výstrahy hello (Příklad: "Odeslat e-mail, pokud dojde k selhání").</span><span class="sxs-lookup"><span data-stu-id="a5228-350">Enter a description for hello alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="a5228-351">Vyberte metriku (vs "Spuštění se nezdařilo". "Úspěšně spuštěno").</span><span class="sxs-lookup"><span data-stu-id="a5228-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="a5228-352">Zadejte podmínku a prahová hodnota.</span><span class="sxs-lookup"><span data-stu-id="a5228-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="a5228-353">Zadejte dobu hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-353">Specify hello period of time.</span></span>
* <span data-ttu-id="a5228-354">Určete, zda tooowners, přispěvatelé a čtenáři se mají odesílat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5228-354">Specify whether an email should be sent tooowners, contributors, and readers.</span></span>

![Okno metriky objekt pro vytváření dat > Přidat pravidlo výstrahy](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="a5228-356">Po hello pravidlo výstrahy bylo úspěšně přidáno, zavře se okno hello a zobrazí nová výstraha hello na hello **metrika** okno.</span><span class="sxs-lookup"><span data-stu-id="a5228-356">After hello alert rule is added successfully, hello blade closes and you see hello new alert on hello **Metric** blade.</span></span>

![Okno metriky objekt pro vytváření dat > Přidat nové výstrahy](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="a5228-358">Měli byste taky vidět hello počet výstrah v hello **výstrah pravidla** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a5228-358">You should also see hello number of alerts in hello **Alert rules** tile.</span></span> <span data-ttu-id="a5228-359">Klikněte na tlačítko hello **výstrah pravidla** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a5228-359">Click hello **Alert rules** tile.</span></span>

![Data factory metriky okno - pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="a5228-361">Na hello **výstrahy pravidla** okně se zobrazí všechny existující výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5228-361">On hello **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="a5228-362">tooadd výstraha, klikněte na tlačítko **přidat upozornění** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-362">tooadd an alert, click **Add alert** on hello toolbar.</span></span>

![Okno pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="a5228-364">Oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="a5228-364">Alert notifications</span></span>
<span data-ttu-id="a5228-365">Po pravidlo výstrahy hello odpovídá hello podmínku, měli byste obdržet e-mail s upozorněním, že se aktivuje výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="a5228-365">After hello alert rule matches hello condition, you should get an email that says hello alert is activated.</span></span> <span data-ttu-id="a5228-366">Po hello problém se vyřeší a výstražný stav hello neodpovídá už, můžete získat e-mailu s upozorněním, že hello výstraha vyřeší.</span><span class="sxs-lookup"><span data-stu-id="a5228-366">After hello issue is resolved and hello alert condition doesn’t match anymore, you get an email that says hello alert is resolved.</span></span>

<span data-ttu-id="a5228-367">Toto chování se liší od událostí, kde jsou oznámení odesílána v každé selhání, který identifikuje pravidlo výstrahy pro.</span><span class="sxs-lookup"><span data-stu-id="a5228-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="a5228-368">Výstrahy nasazení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5228-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="a5228-369">Výstrahy můžete nasadit pro metriky hello stejné tak, aby se pro události.</span><span class="sxs-lookup"><span data-stu-id="a5228-369">You can deploy alerts for metrics hello same way that you do for events.</span></span>

<span data-ttu-id="a5228-370">**Definice upozornění**</span><span class="sxs-lookup"><span data-stu-id="a5228-370">**Alert definition**</span></span>

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

<span data-ttu-id="a5228-371">Nahraďte *subscriptionId*, *resourceGroupName*, a *dataFactoryName* v ukázce hello s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a5228-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in hello sample with appropriate values.</span></span>

<span data-ttu-id="a5228-372">*metricName* aktuálně podporuje dvě hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a5228-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="a5228-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="a5228-373">FailedRuns</span></span>
* <span data-ttu-id="a5228-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="a5228-374">SuccessfulRuns</span></span>

<span data-ttu-id="a5228-375">**Nasazení hello výstrahy**</span><span class="sxs-lookup"><span data-stu-id="a5228-375">**Deploy hello alert**</span></span>

<span data-ttu-id="a5228-376">toodeploy hello výstrahy, použijte rutinu prostředí Azure PowerShell hello **New-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="a5228-376">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="a5228-377">Měli byste vidět následující zprávou po úspěšné nasazení:</span><span class="sxs-lookup"><span data-stu-id="a5228-377">You should see following message after a successful deployment:</span></span>

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

<span data-ttu-id="a5228-378">Můžete taky hello **přidat AlertRule** rutiny toodeploy pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5228-378">You can also use hello **Add-AlertRule** cmdlet toodeploy an alert rule.</span></span> <span data-ttu-id="a5228-379">V tématu hello [přidat AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) téma podrobné informace a příklady.</span><span class="sxs-lookup"><span data-stu-id="a5228-379">See hello [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a><span data-ttu-id="a5228-380">Přesunout data factory tooa jiné skupině prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="a5228-380">Move a data factory tooa different resource group or subscription</span></span>
<span data-ttu-id="a5228-381">Můžete přesunout data factory tooa jiné skupině prostředků nebo jiného předplatného pomocí hello **přesunout** příkazu panelu tlačítko na domovskou stránku hello svojí datové továrny.</span><span class="sxs-lookup"><span data-stu-id="a5228-381">You can move a data factory tooa different resource group or a different subscription by using hello **Move** command bar button on hello home page of your data factory.</span></span>

![Přesunout objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="a5228-383">Také můžete přesunout všechny související prostředky (například výstrahy, které jsou spojeny s hello data factory), spolu s hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a5228-383">You can also move any related resources (such as alerts that are associated with hello data factory), along with hello data factory.</span></span>

![Dialogové okno prostředků](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
