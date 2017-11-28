---
title: "aaaScheduling sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Popisuje, jak toocreate plánu ve službě Azure Automation, aby může automaticky spustit sadu runbook v určitém čase nebo podle plánu opakování."
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
ms.openlocfilehash: c215b7ff6aa200466f3be566facba3c0cffcc924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="79070-103">Naplánování runbooku v Azure Automation</span><span class="sxs-lookup"><span data-stu-id="79070-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="79070-104">tooschedule sady runbook v Azure Automation toostart v zadanou dobu, můžete ho propojit tooone nebo více plánů.</span><span class="sxs-lookup"><span data-stu-id="79070-104">tooschedule a runbook in Azure Automation toostart at a specified time, you link it tooone or more schedules.</span></span> <span data-ttu-id="79070-105">Plán může být nakonfigurované tooeither spustit jednou nebo na opakovaném hodinových nebo denních plánu pro sady runbook ve hello portál Azure classic a pro sady runbook ve hello portálu Azure, můžete kromě naplánovat je na týdně, měsíčně, konkrétní dny v týdnu hello nebo dní Dobrý den, měsíc, nebo určitý den v měsíci hello.</span><span class="sxs-lookup"><span data-stu-id="79070-105">A schedule can be configured tooeither run once or on a reoccurring hourly or daily schedule for runbooks in hello Azure classic portal and for runbooks in hello Azure portal,  you can additionally schedule them for weekly, monthly, specific days of hello week or days of hello month, or a particular day of hello month.</span></span>  <span data-ttu-id="79070-106">Sada runbook může být propojené toomultiple plány a plán může mít několik tooit runbooků.</span><span class="sxs-lookup"><span data-stu-id="79070-106">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="79070-107">Vytvoření plánu</span><span class="sxs-lookup"><span data-stu-id="79070-107">Creating a schedule</span></span>
<span data-ttu-id="79070-108">V hello portál Azure, můžete vytvořit nový plán pro sady runbook na portálu classic hello, nebo pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="79070-108">You can create a new schedule for runbooks in hello Azure portal, in hello classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="79070-109">Máte také možnost hello vytvoření nového plánu, když připojujete runbook tooa plánu pomocí hello Azure classic nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="79070-109">You also have hello option of creating a new schedule when you link a runbook tooa schedule using hello Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="79070-110">Když přidružení plánu k sadě runbook, automatizace ukládá hello aktuální verze hello modulů ve vašem účtu a je propojuje toothat plán.</span><span class="sxs-lookup"><span data-stu-id="79070-110">When you associate a schedule with a runbook, Automation stores hello current versions of hello modules in your account and links them toothat schedule.</span></span>  <span data-ttu-id="79070-111">To znamená, že pokud měl modul s verze 1.0 ve vašem účtu při vytvoření plánu a pak aktualizujte hello modulu tooversion 2.0, bude plán hello toouse 1.0.</span><span class="sxs-lookup"><span data-stu-id="79070-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update hello module tooversion 2.0, hello schedule will continue toouse 1.0.</span></span>  <span data-ttu-id="79070-112">V pořadí toouse hello modulu aktualizovanou verzi musíte vytvořit nový plán.</span><span class="sxs-lookup"><span data-stu-id="79070-112">In order toouse hello updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a><span data-ttu-id="79070-113">toocreate nového plánu v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="79070-113">toocreate a new schedule in hello Azure classic portal</span></span>
1. <span data-ttu-id="79070-114">V hello portál Azure classic vyberte automatizace a pak vyberte název hello účet automation.</span><span class="sxs-lookup"><span data-stu-id="79070-114">In hello Azure classic portal, select Automation and then then select hello name of an automation account.</span></span>
2. <span data-ttu-id="79070-115">Vyberte hello **prostředky** kartě.</span><span class="sxs-lookup"><span data-stu-id="79070-115">Select hello **Assets** tab.</span></span>
3. <span data-ttu-id="79070-116">V dolní části hello hello okna, klikněte na tlačítko **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="79070-116">At hello bottom of hello window, click **Add Setting**.</span></span>
4. <span data-ttu-id="79070-117">Klikněte na tlačítko **přidat plán**.</span><span class="sxs-lookup"><span data-stu-id="79070-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="79070-118">Zadejte **název** a volitelně **popis** pro nové schedule.your hello bude spuštěn plán **jednou**, **hodinové**, **Denní**, **týdenní**, nebo **měsíční**.</span><span class="sxs-lookup"><span data-stu-id="79070-118">Type a **Name** and optionally a **Description** for hello new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="79070-119">Zadejte **čas spuštění** a další možnosti v závislosti na typu hello plánu, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="79070-119">Specify a **Start Time** and other options depending on hello type of schedule that you selected.</span></span>

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a><span data-ttu-id="79070-120">toocreate nového plánu v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79070-120">toocreate a new schedule in hello Azure portal</span></span>
1. <span data-ttu-id="79070-121">V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="79070-121">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="79070-122">Klikněte na tlačítko hello **plány** dlaždice tooopen hello **plány** okno.</span><span class="sxs-lookup"><span data-stu-id="79070-122">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="79070-123">Klikněte na tlačítko **přidat plán** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="79070-123">Click **Add a schedule** at hello top of hello blade.</span></span>
4. <span data-ttu-id="79070-124">Na hello **nový plán** okno, zadejte **název** a volitelně **popis** pro hello nový plán.</span><span class="sxs-lookup"><span data-stu-id="79070-124">On hello **New schedule** blade, type a **Name** and optionally a **Description** for hello new schedule.</span></span>
5. <span data-ttu-id="79070-125">Vyberte, zda plán hello se spustí jednou, nebo podle plánu opakovaném výběrem **jednou** nebo **opakování**.</span><span class="sxs-lookup"><span data-stu-id="79070-125">Select whether hello schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="79070-126">Pokud vyberete **jednou** zadejte **počáteční čas** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="79070-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="79070-127">Pokud vyberete **opakování**, zadejte **počáteční čas** a četnost hello pro interval hello runbook toorepeat - nástrojem **hodinu**, **den**, **týden**, nebo pomocí **měsíc**.</span><span class="sxs-lookup"><span data-stu-id="79070-127">If you select **Recurrence**, specify a **Start time** and hello frequency for how often you want hello runbook toorepeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="79070-128">Pokud vyberete **týden** nebo **měsíc** z rozevíracího seznamu hello hello **opakování možnost** se zobrazí v okně hello a při výběru, hello **opakování možnost** zobrazí okno a hello den v týdnu můžete vybrat, pokud jste vybrali **týden**.</span><span class="sxs-lookup"><span data-stu-id="79070-128">If you select **week** or **month** from hello drop-down list, hello **Recurrence option** will appear in hello blade and upon selection, hello **Recurrence option** blade will be presented and you can select hello day of week if you selected **week**.</span></span>  <span data-ttu-id="79070-129">Pokud jste vybrali **měsíc**, můžete **dny v týdnu** nebo konkrétní dny v měsíci hello na hello kalendáře a nakonec chcete toorun ho na hello poslední den v měsíci hello nebo Ne a pak klikněte na tlačítko **OK** .</span><span class="sxs-lookup"><span data-stu-id="79070-129">If you selected **month**, you can choose by **week days** or specific days of hello month on hello calendar and finally, do you want toorun it on hello last day of hello month or not and then click **OK**.</span></span>   

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="79070-130">toocreate nového plánu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="79070-130">toocreate a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="79070-131">Můžete použít hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) rutiny toocreate nový plán ve službě Azure Automation pro classic sady runbook, nebo [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) rutiny pro sady runbook ve hello Azure portál.</span><span class="sxs-lookup"><span data-stu-id="79070-131">You can use hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet toocreate a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="79070-132">Je nutné zadat počáteční čas hello hello plánu a četnosti hello, který se má spustit.</span><span class="sxs-lookup"><span data-stu-id="79070-132">You must specify hello start time for hello schedule and hello frequency it should run.</span></span>

<span data-ttu-id="79070-133">Hello následující vzorové příkazy ukazují, jak toocreate nový plán, spouští každý den ve 3:30 20 leden 2015 počínaje rutiny Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="79070-133">hello following sample commands show how toocreate a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="79070-134">Hello následující ukázkové příkazy ukazuje jak toocreate plán pro hello 15 a 30 v každém měsíci rutinou služby Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="79070-134">hello following sample commands shows how toocreate a schedule for hello 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-tooa-runbook"></a><span data-ttu-id="79070-135">Propojování runbook tooa plán</span><span class="sxs-lookup"><span data-stu-id="79070-135">Linking a schedule tooa runbook</span></span>
<span data-ttu-id="79070-136">Sada runbook může být propojené toomultiple plány a plán může mít několik tooit runbooků.</span><span class="sxs-lookup"><span data-stu-id="79070-136">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span> <span data-ttu-id="79070-137">Pokud runbook obsahuje parametry, můžete zadat hodnoty pro ně.</span><span class="sxs-lookup"><span data-stu-id="79070-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="79070-138">Zadejte hodnoty všech povinných parametrů a může poskytnout hodnoty pro všechny volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="79070-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="79070-139">Tyto hodnoty se použije při každém spuštění runbooku hello podle tohoto plánu.</span><span class="sxs-lookup"><span data-stu-id="79070-139">These values will be used each time hello runbook is started by this schedule.</span></span>  <span data-ttu-id="79070-140">Můžete připojit hello stejné tooanother plán sad runbook a zadejte jiné hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="79070-140">You can attach hello same runbook tooanother schedule and specify different parameter values.</span></span>

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a><span data-ttu-id="79070-141">toolink runbook tooa plán s hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="79070-141">toolink a schedule tooa runbook with hello Azure classic portal</span></span>
1. <span data-ttu-id="79070-142">V hello portál Azure classic, vyberte **automatizace** a pak klikněte hello název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="79070-142">In hello Azure classic portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="79070-143">Vyberte hello **Runbooky** kartě.</span><span class="sxs-lookup"><span data-stu-id="79070-143">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="79070-144">Klikněte na název hello hello runbook tooschedule.</span><span class="sxs-lookup"><span data-stu-id="79070-144">Click on hello name of hello runbook tooschedule.</span></span>
4. <span data-ttu-id="79070-145">Klikněte na tlačítko hello **plán** kartě.</span><span class="sxs-lookup"><span data-stu-id="79070-145">Click hello **Schedule** tab.</span></span>
5. <span data-ttu-id="79070-146">Pokud hello runbook není aktuálně propojené tooa plán, pak budete mít možnost hello příliš**odkaz tooa nový plán** nebo **odkaz tooan existující plán**.</span><span class="sxs-lookup"><span data-stu-id="79070-146">If hello runbook is not currently linked tooa schedule, then you will be given hello option too**Link tooa New Schedule** or **Link tooan Existing Schedule**.</span></span>  <span data-ttu-id="79070-147">Pokud je hello runbook aktuálně propojené tooa plán, klikněte na tlačítko **odkaz** v hello dolní části okna tooaccess hello tyto možnosti.</span><span class="sxs-lookup"><span data-stu-id="79070-147">If hello runbook is currently linked tooa schedule, click **Link** at hello bottom of hello window tooaccess these options.</span></span>
6. <span data-ttu-id="79070-148">Pokud hello runbook obsahuje parametry, budete vyzváni k jejich hodnot.</span><span class="sxs-lookup"><span data-stu-id="79070-148">If hello runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a><span data-ttu-id="79070-149">toolink runbook tooa plán s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79070-149">toolink a schedule tooa runbook with hello Azure portal</span></span>
1. <span data-ttu-id="79070-150">V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello **Runbooky** okno.</span><span class="sxs-lookup"><span data-stu-id="79070-150">In hello Azure portal, from your automation account, click hello **Runbooks** tile tooopen hello **Runbooks** blade.</span></span>
2. <span data-ttu-id="79070-151">Klikněte na název hello hello runbook tooschedule.</span><span class="sxs-lookup"><span data-stu-id="79070-151">Click on hello name of hello runbook tooschedule.</span></span>
3. <span data-ttu-id="79070-152">Pokud hello runbook není aktuálně propojené tooa plán, bude se daný hello možnost toocreate nový plán nebo na odkaz tooan existující plán.</span><span class="sxs-lookup"><span data-stu-id="79070-152">If hello runbook is not currently linked tooa schedule, then you will be given hello option toocreate a new schedule or link tooan existing schedule.</span></span>  
4. <span data-ttu-id="79070-153">Pokud hello runbook obsahuje parametry, můžete vybrat možnost hello **upravit nastavení spouštění (výchozí: Azure)** a hello **parametry** okno se zobrazí, kde můžete zadat informace hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="79070-153">If hello runbook has parameters, you can select hello option **Modify run settings (Default:Azure)** and hello **Parameters** blade is presented where you can enter hello information accordingly.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a><span data-ttu-id="79070-154">toolink runbook tooa plánu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="79070-154">toolink a schedule tooa runbook with Windows PowerShell</span></span>
<span data-ttu-id="79070-155">Můžete použít hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink classic runbook tooa plán nebo [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) rutiny pro sady runbook ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="79070-155">You can use hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink a schedule tooa classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in hello Azure portal.</span></span>  <span data-ttu-id="79070-156">Můžete zadat hodnoty pro parametry runbooku hello s parametrem parametry hello.</span><span class="sxs-lookup"><span data-stu-id="79070-156">You can specify values for hello runbook’s parameters with hello Parameters parameter.</span></span> <span data-ttu-id="79070-157">V tématu [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) Další informace o zadání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="79070-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="79070-158">Hello následující ukázkové příkazy Zobrazit jak toolink plánu pomocí rutiny Azure Service Management s parametry.</span><span class="sxs-lookup"><span data-stu-id="79070-158">hello following sample commands show how toolink a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="79070-159">Hello následující ukázkové příkazy Zobrazit jak toolink plán tooa runbooku pomocí rutiny Azure Resource Manager s parametry.</span><span class="sxs-lookup"><span data-stu-id="79070-159">hello following sample commands show how toolink a schedule tooa runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="79070-160">Zakázání plánu</span><span class="sxs-lookup"><span data-stu-id="79070-160">Disabling a schedule</span></span>
<span data-ttu-id="79070-161">Při zakázání plánu všechny runbooky propojené tooit nebude možné spustit na tento plán.</span><span class="sxs-lookup"><span data-stu-id="79070-161">When you disable a schedule, any runbooks linked tooit will no longer run on that schedule.</span></span> <span data-ttu-id="79070-162">Můžete ručně zakázání plánu nebo můžete nastavit dobu vypršení platnosti plány s frekvencí při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="79070-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="79070-163">Když je dosaženo času vypršení platnosti hello, hello plán zakázán.</span><span class="sxs-lookup"><span data-stu-id="79070-163">When hello expiration time is reached, hello schedule will be disabled.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a><span data-ttu-id="79070-164">toodisable plánu z hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="79070-164">toodisable a schedule from hello Azure classic portal</span></span>
<span data-ttu-id="79070-165">Můžete zakázat plán v hello portál Azure classic na stránce hello podrobnosti plánu pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="79070-165">You can disable a schedule in hello Azure classic portal from hello Schedule Details page for hello schedule.</span></span>

1. <span data-ttu-id="79070-166">V hello portál Azure classic vyberte automatizace a pak klikněte hello název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="79070-166">In hello Azure classic portal, select Automation and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="79070-167">Vyberte kartu prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="79070-167">Select hello Assets tab.</span></span>
3. <span data-ttu-id="79070-168">Klikněte na název hello plán tooopen stránku s jeho podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="79070-168">Click hello name of a schedule tooopen its detail page.</span></span>
4. <span data-ttu-id="79070-169">Změna **povoleno** příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="79070-169">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a><span data-ttu-id="79070-170">toodisable plánu z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79070-170">toodisable a schedule from hello Azure portal</span></span>
1. <span data-ttu-id="79070-171">V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="79070-171">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="79070-172">Klikněte na tlačítko hello **plány** dlaždice tooopen hello **plány** okno.</span><span class="sxs-lookup"><span data-stu-id="79070-172">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="79070-173">Klikněte na název hello okno Podrobnosti plánu tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="79070-173">Click hello name of a schedule tooopen hello details blade.</span></span>
4. <span data-ttu-id="79070-174">Změna **povoleno** příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="79070-174">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-with-windows-powershell"></a><span data-ttu-id="79070-175">toodisable plánu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="79070-175">toodisable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="79070-176">Můžete použít hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) rutiny toochange hello vlastnosti existující plán pro classic sadu runbook nebo [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) rutiny pro sady runbook ve hello Azure portál.</span><span class="sxs-lookup"><span data-stu-id="79070-176">You can use hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet toochange hello properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="79070-177">toodisable hello naplánovat, zadejte **false** pro hello **hodnotu IsEnabled** parametr.</span><span class="sxs-lookup"><span data-stu-id="79070-177">toodisable hello schedule, specify **false** for hello **IsEnabled** parameter.</span></span>

<span data-ttu-id="79070-178">Hello následující vzorové příkazy ukazují, jak hello toodisable plán pomocí rutiny Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="79070-178">hello following sample commands show how toodisable a schedule using hello Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="79070-179">Hello následující ukázkové příkazy Zobrazit jak toodisable plán pro sady runbook pomocí rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79070-179">hello following sample commands show how toodisable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="79070-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79070-180">Next steps</span></span>
* <span data-ttu-id="79070-181">toolearn Další informace o práci s plány, najdete v části [plán prostředky ve službě Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="79070-181">toolearn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="79070-182">tooget kroky s runbooky ve službě Azure Automation najdete v části [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="79070-182">tooget started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

