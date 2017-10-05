---
title: "Plánování runbooku ve službě Azure Automation | Microsoft Docs"
description: "Popisuje, jak vytvořit plán ve službě Azure Automation, takže může automaticky spustit sadu runbook v určitém čase nebo podle plánu opakování."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 52f1d55f141bb1b3948e3b7039cfc131a5e407b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="566a3-103">Naplánování runbooku v Azure Automation</span><span class="sxs-lookup"><span data-stu-id="566a3-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="566a3-104">Pokud chcete naplánovat spuštění runbooku ve službě Azure Automation spustit v zadanou dobu, můžete ho propojit s jedním nebo víc plány.</span><span class="sxs-lookup"><span data-stu-id="566a3-104">To schedule a runbook in Azure Automation to start at a specified time, you link it to one or more schedules.</span></span> <span data-ttu-id="566a3-105">Plán můžete nakonfigurovat pro spuštění jednou nebo na nadále hodinové nebo denní plán pro sady runbook na portálu Azure classic a pro sady runbook na portálu Azure, můžete taky naplánovat je na týdně, měsíčně, konkrétní dny v týdnu nebo dny v měsíci nebo určitý den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="566a3-105">A schedule can be configured to either run once or on a reoccurring hourly or daily schedule for runbooks in the Azure classic portal and for runbooks in the Azure portal,  you can additionally schedule them for weekly, monthly, specific days of the week or days of the month, or a particular day of the month.</span></span>  <span data-ttu-id="566a3-106">Sady runbook mohou být spojeny s víc plány a plán může mít víc runbooků.</span><span class="sxs-lookup"><span data-stu-id="566a3-106">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="566a3-107">Vytvoření plánu</span><span class="sxs-lookup"><span data-stu-id="566a3-107">Creating a schedule</span></span>
<span data-ttu-id="566a3-108">Můžete vytvořit nový plán pro sady runbook na portálu Azure, na klasickém portálu nebo pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="566a3-108">You can create a new schedule for runbooks in the Azure portal, in the classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="566a3-109">Máte také možnost vytvořit nový plán, když připojujete runbook k plánu pomocí portálu Azure classic nebo Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-109">You also have the option of creating a new schedule when you link a runbook to a schedule using the Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="566a3-110">Když přidružení plánu k sadě runbook, automatizace ukládá aktuální verze modulů ve vašem účtu a je odkazuje na tento plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-110">When you associate a schedule with a runbook, Automation stores the current versions of the modules in your account and links them to that schedule.</span></span>  <span data-ttu-id="566a3-111">To znamená, že pokud jste při vytvoření plánu a pak aktualizujte na verzi 2.0 modul měli modul s verze 1.0 ve vašem účtu, plán bude nadále používat 1.0.</span><span class="sxs-lookup"><span data-stu-id="566a3-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update the module to version 2.0, the schedule will continue to use 1.0.</span></span>  <span data-ttu-id="566a3-112">Chcete-li použít modul aktualizovanou verzi, musíte vytvořit nový plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-112">In order to use the updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a><span data-ttu-id="566a3-113">Chcete-li vytvořit nový plán na portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="566a3-113">To create a new schedule in the Azure classic portal</span></span>
1. <span data-ttu-id="566a3-114">Na portálu Azure classic vyberte automatizace a pak vyberte název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="566a3-114">In the Azure classic portal, select Automation and then then select the name of an automation account.</span></span>
2. <span data-ttu-id="566a3-115">Vyberte **prostředky** kartě.</span><span class="sxs-lookup"><span data-stu-id="566a3-115">Select the **Assets** tab.</span></span>
3. <span data-ttu-id="566a3-116">V dolní části okna klikněte na tlačítko **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="566a3-116">At the bottom of the window, click **Add Setting**.</span></span>
4. <span data-ttu-id="566a3-117">Klikněte na tlačítko **přidat plán**.</span><span class="sxs-lookup"><span data-stu-id="566a3-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="566a3-118">Zadejte **název** a volitelně **popis** pro nové schedule.your bude spuštěn plán **jednou**, **hodinové**, **denní**, **týdenní**, nebo **měsíční**.</span><span class="sxs-lookup"><span data-stu-id="566a3-118">Type a **Name** and optionally a **Description** for the new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="566a3-119">Zadejte **čas spuštění** a další možnosti v závislosti na typu plánu, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="566a3-119">Specify a **Start Time** and other options depending on the type of schedule that you selected.</span></span>

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a><span data-ttu-id="566a3-120">Chcete-li vytvořit nový plán na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="566a3-120">To create a new schedule in the Azure portal</span></span>
1. <span data-ttu-id="566a3-121">Na portálu Azure z vašeho účtu automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="566a3-121">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="566a3-122">Klikněte **plány** dlaždici otevřete **plány** okno.</span><span class="sxs-lookup"><span data-stu-id="566a3-122">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="566a3-123">Klikněte na tlačítko **přidat plán** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="566a3-123">Click **Add a schedule** at the top of the blade.</span></span>
4. <span data-ttu-id="566a3-124">Na **nový plán** okno, zadejte **název** a volitelně **popis** pro nový plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-124">On the **New schedule** blade, type a **Name** and optionally a **Description** for the new schedule.</span></span>
5. <span data-ttu-id="566a3-125">Vyberte jestli plán poběží jednorázově nebo podle plánu opakovaném výběrem **jednou** nebo **opakování**.</span><span class="sxs-lookup"><span data-stu-id="566a3-125">Select whether the schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="566a3-126">Pokud vyberete **jednou** zadejte **počáteční čas** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="566a3-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="566a3-127">Pokud vyberete **opakování**, zadejte **počáteční čas** a jak často chcete runbooku opakovat - nástrojem frekvenci **hodinu**, **den**, **týden**, nebo pomocí **měsíc**.</span><span class="sxs-lookup"><span data-stu-id="566a3-127">If you select **Recurrence**, specify a **Start time** and the frequency for how often you want the runbook to repeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="566a3-128">Pokud vyberete **týden** nebo **měsíc** z rozevíracího seznamu **opakování možnost** se zobrazí v okně a při výběru, **opakování možnost** zobrazí okno a den v týdnu můžete vybrat, pokud jste vybrali **týden**.</span><span class="sxs-lookup"><span data-stu-id="566a3-128">If you select **week** or **month** from the drop-down list, the **Recurrence option** will appear in the blade and upon selection, the **Recurrence option** blade will be presented and you can select the day of week if you selected **week**.</span></span>  <span data-ttu-id="566a3-129">Pokud jste vybrali **měsíc**, můžete **dny v týdnu** nebo konkrétní dny v měsíci v kalendáři a nakonec chcete spustit poslední den v měsíci, nebo Ne, a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="566a3-129">If you selected **month**, you can choose by **week days** or specific days of the month on the calendar and finally, do you want to run it on the last day of the month or not and then click **OK**.</span></span>   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="566a3-130">K vytvoření nového plánu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="566a3-130">To create a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="566a3-131">Můžete použít [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) vytvořte nový plán ve službě Azure Automation pro classic sady runbook, nebo [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) rutiny pro sady runbook na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-131">You can use the [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet to create a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="566a3-132">Musíte zadat čas zahájení pro plán a četnosti, který se má spustit.</span><span class="sxs-lookup"><span data-stu-id="566a3-132">You must specify the start time for the schedule and the frequency it should run.</span></span>

<span data-ttu-id="566a3-133">Následující vzorové příkazy ukazují, jak vytvořit nový plán, který spouští každý den ve 3:30 20 leden 2015 počínaje rutiny Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="566a3-133">The following sample commands show how to create a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="566a3-134">Následující vzorové příkazy ukazuje, jak vytvořit plán pro 15. dne a 30 v každém měsíci rutinou služby Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-134">The following sample commands shows how to create a schedule for the 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a><span data-ttu-id="566a3-135">Propojování plánu k sadě runbook</span><span class="sxs-lookup"><span data-stu-id="566a3-135">Linking a schedule to a runbook</span></span>
<span data-ttu-id="566a3-136">Sady runbook mohou být spojeny s víc plány a plán může mít víc runbooků.</span><span class="sxs-lookup"><span data-stu-id="566a3-136">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span> <span data-ttu-id="566a3-137">Pokud runbook obsahuje parametry, můžete zadat hodnoty pro ně.</span><span class="sxs-lookup"><span data-stu-id="566a3-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="566a3-138">Zadejte hodnoty všech povinných parametrů a může poskytnout hodnoty pro všechny volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="566a3-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="566a3-139">Tyto hodnoty se použije při každém spuštění runbooku podle tohoto plánu.</span><span class="sxs-lookup"><span data-stu-id="566a3-139">These values will be used each time the runbook is started by this schedule.</span></span>  <span data-ttu-id="566a3-140">Můžete přiřadit stejné sady runbook jiný plán a zadejte jiné hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="566a3-140">You can attach the same runbook to another schedule and specify different parameter values.</span></span>

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a><span data-ttu-id="566a3-141">Pro připojení plánu k sadě runbook pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="566a3-141">To link a schedule to a runbook with the Azure classic portal</span></span>
1. <span data-ttu-id="566a3-142">Na portálu Azure classic, vyberte **automatizace** a pak klikněte na název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="566a3-142">In the Azure classic portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="566a3-143">Vyberte **Runbooky** kartě.</span><span class="sxs-lookup"><span data-stu-id="566a3-143">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="566a3-144">Klikněte na název sady runbook k plánu.</span><span class="sxs-lookup"><span data-stu-id="566a3-144">Click on the name of the runbook to schedule.</span></span>
4. <span data-ttu-id="566a3-145">Klikněte **plán** kartě.</span><span class="sxs-lookup"><span data-stu-id="566a3-145">Click the **Schedule** tab.</span></span>
5. <span data-ttu-id="566a3-146">Pokud sada runbook není aktuálně propojený s plán, pak budete mít možnost **odkaz na nový plán** nebo **odkaz na existující plán**.</span><span class="sxs-lookup"><span data-stu-id="566a3-146">If the runbook is not currently linked to a schedule, then you will be given the option to **Link to a New Schedule** or **Link to an Existing Schedule**.</span></span>  <span data-ttu-id="566a3-147">Pokud runbook už připojený k plánu, klikněte na tlačítko **odkaz** v dolní části okna pro přístup k tyto možnosti.</span><span class="sxs-lookup"><span data-stu-id="566a3-147">If the runbook is currently linked to a schedule, click **Link** at the bottom of the window to access these options.</span></span>
6. <span data-ttu-id="566a3-148">Pokud sada runbook obsahuje parametry, zobrazí se výzva pro jejich hodnot.</span><span class="sxs-lookup"><span data-stu-id="566a3-148">If the runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a><span data-ttu-id="566a3-149">Pro připojení plánu k sadě runbook pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="566a3-149">To link a schedule to a runbook with the Azure portal</span></span>
1. <span data-ttu-id="566a3-150">Na portálu Azure z vašeho účtu automation, klikněte na tlačítko **Runbooky** dlaždici otevřete **sady Runbook** okno.</span><span class="sxs-lookup"><span data-stu-id="566a3-150">In the Azure portal, from your automation account, click the **Runbooks** tile to open the **Runbooks** blade.</span></span>
2. <span data-ttu-id="566a3-151">Klikněte na název sady runbook k plánu.</span><span class="sxs-lookup"><span data-stu-id="566a3-151">Click on the name of the runbook to schedule.</span></span>
3. <span data-ttu-id="566a3-152">Pokud sada runbook není aktuálně propojena k plánu, pak budete mít možnost vytvořit nový plán nebo odkaz na existující plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-152">If the runbook is not currently linked to a schedule, then you will be given the option to create a new schedule or link to an existing schedule.</span></span>  
4. <span data-ttu-id="566a3-153">Pokud má runbook parametry, můžete vybrat možnost **upravit nastavení spouštění (výchozí: Azure)** a **parametry** okno se zobrazí, kde můžete zadat informace odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="566a3-153">If the runbook has parameters, you can select the option **Modify run settings (Default:Azure)** and the **Parameters** blade is presented where you can enter the information accordingly.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a><span data-ttu-id="566a3-154">Pro připojení plánu k sadě runbook pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="566a3-154">To link a schedule to a runbook with Windows PowerShell</span></span>
<span data-ttu-id="566a3-155">Můžete použít [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) pro připojení plánu k sadě runbook classic nebo [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) rutiny pro sady runbook na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-155">You can use the [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) to link a schedule to a classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in the Azure portal.</span></span>  <span data-ttu-id="566a3-156">S parametrem parametrů můžete zadat hodnoty pro parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="566a3-156">You can specify values for the runbook’s parameters with the Parameters parameter.</span></span> <span data-ttu-id="566a3-157">V tématu [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) Další informace o zadání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="566a3-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="566a3-158">Následující vzorové příkazy ukazují, jak propojit plán s rutinou služby Azure Service Management s parametry.</span><span class="sxs-lookup"><span data-stu-id="566a3-158">The following sample commands show how to link a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="566a3-159">Následující vzorové příkazy ukazují, jak pro připojení plánu k sadě runbook s parametry rutinou služby Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-159">The following sample commands show how to link a schedule to a runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="566a3-160">Zakázání plánu</span><span class="sxs-lookup"><span data-stu-id="566a3-160">Disabling a schedule</span></span>
<span data-ttu-id="566a3-161">Při zakázání plánu všechny runbooky propojené s ho nebude možné spustit na tento plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-161">When you disable a schedule, any runbooks linked to it will no longer run on that schedule.</span></span> <span data-ttu-id="566a3-162">Můžete ručně zakázání plánu nebo můžete nastavit dobu vypršení platnosti plány s frekvencí při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="566a3-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="566a3-163">Když je dosaženo času vypršení platnosti, bude plán zakázán.</span><span class="sxs-lookup"><span data-stu-id="566a3-163">When the expiration time is reached, the schedule will be disabled.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a><span data-ttu-id="566a3-164">Zakázání plánu z portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="566a3-164">To disable a schedule from the Azure classic portal</span></span>
<span data-ttu-id="566a3-165">Můžete zakázat plán na portálu Azure classic na stránce Podrobnosti plánu pro plán.</span><span class="sxs-lookup"><span data-stu-id="566a3-165">You can disable a schedule in the Azure classic portal from the Schedule Details page for the schedule.</span></span>

1. <span data-ttu-id="566a3-166">Na portálu Azure classic vyberte automatizace a pak klikněte na název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="566a3-166">In the Azure classic portal, select Automation and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="566a3-167">Vyberte kartu prostředky.</span><span class="sxs-lookup"><span data-stu-id="566a3-167">Select the Assets tab.</span></span>
3. <span data-ttu-id="566a3-168">Klikněte na název plán, který chcete otevřít stránku s jeho podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="566a3-168">Click the name of a schedule to open its detail page.</span></span>
4. <span data-ttu-id="566a3-169">Změna **povoleno** k **ne**.</span><span class="sxs-lookup"><span data-stu-id="566a3-169">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-portal"></a><span data-ttu-id="566a3-170">Zakázání plánu z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="566a3-170">To disable a schedule from the Azure portal</span></span>
1. <span data-ttu-id="566a3-171">Na portálu Azure z vašeho účtu automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="566a3-171">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="566a3-172">Klikněte **plány** dlaždici otevřete **plány** okno.</span><span class="sxs-lookup"><span data-stu-id="566a3-172">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="566a3-173">Klikněte na název plánu a otevřete okno Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="566a3-173">Click the name of a schedule to open the details blade.</span></span>
4. <span data-ttu-id="566a3-174">Změna **povoleno** k **ne**.</span><span class="sxs-lookup"><span data-stu-id="566a3-174">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-with-windows-powershell"></a><span data-ttu-id="566a3-175">Zakázání plánu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="566a3-175">To disable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="566a3-176">Můžete použít [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) rutiny změnit vlastnosti existující plán pro classic sadu runbook nebo [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) rutiny pro sady runbook na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="566a3-176">You can use the [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet to change the properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="566a3-177">Pokud chcete zakázat plán, zadejte **false** pro **hodnotu IsEnabled** parametr.</span><span class="sxs-lookup"><span data-stu-id="566a3-177">To disable the schedule, specify **false** for the **IsEnabled** parameter.</span></span>

<span data-ttu-id="566a3-178">Následující vzorové příkazy znázorňují postup zakázání plánu pomocí rutiny Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="566a3-178">The following sample commands show how to disable a schedule using the Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="566a3-179">Následující vzorové příkazy ukazují, jak zakázat plán pro sady runbook pomocí rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="566a3-179">The following sample commands show how to disable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="566a3-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="566a3-180">Next steps</span></span>
* <span data-ttu-id="566a3-181">Další informace o práci s plány najdete v tématu [plán prostředky ve službě Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="566a3-181">To learn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="566a3-182">Chcete-li začít pracovat se sadami runbook ve službě Azure Automation, přečtěte si téma [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="566a3-182">To get started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

