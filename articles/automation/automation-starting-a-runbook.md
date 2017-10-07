---
title: "aaaStarting sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Shrnuje hello různé metody, které lze použít toostart sady runbook ve službě Azure Automation a poskytuje podrobnosti o použití obě hello portál Azure a prostředí Windows PowerShell."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e44bce5b56b8e803f9247fbb4f3d4db7ab35c913
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="dcfbb-103">Spuštění sady runbook ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="dcfbb-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="dcfbb-104">Hello následující tabulka vám pomůže určit hello metoda toostart sady runbook ve službě Azure Automation, který je nejvhodnější tooyour určitého scénáře.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-104">hello following table will help you determine hello method toostart a runbook in Azure Automation that is most suitable tooyour particular scenario.</span></span> <span data-ttu-id="dcfbb-105">Tento článek obsahuje informace o spuštění sady runbook s hello portál Azure a prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-105">This article includes details on starting a runbook with hello Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="dcfbb-106">Podrobnosti na hello jiných metod jsou uvedeny v jiných dokumentace, která je přístupné z hello odkazy níže.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-106">Details on hello other methods are provided in other documentation that you can access from hello links below.</span></span>

| <span data-ttu-id="dcfbb-107">**– METODA**</span><span class="sxs-lookup"><span data-stu-id="dcfbb-107">**METHOD**</span></span> | <span data-ttu-id="dcfbb-108">**VLASTNOSTI**</span><span class="sxs-lookup"><span data-stu-id="dcfbb-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="dcfbb-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dcfbb-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="dcfbb-110">Nejjednodušším způsobem s interaktivní uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="dcfbb-111">Formulář tooprovide jednoduché parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-111">Form tooprovide simple parameter values.</span></span><br> <li><span data-ttu-id="dcfbb-112">Snadno sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-112">Easily track job state.</span></span><br> <li><span data-ttu-id="dcfbb-113">Přístup k ověření pomocí přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="dcfbb-114">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="dcfbb-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="dcfbb-115">Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="dcfbb-116">Můžou být součástí automatizované řešení s více kroků.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="dcfbb-117">Ověření žádosti o certifikát nebo OAuth uživatele hlavní / service hlavní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="dcfbb-118">Zadejte hodnoty parametrů jednoduché a komplexní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="dcfbb-119">Sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-119">Track job state.</span></span><br> <li><span data-ttu-id="dcfbb-120">Klient vyžaduje toosupport rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-120">Client required toosupport PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="dcfbb-121">Rozhraní API služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="dcfbb-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="dcfbb-122">Nejflexibilnější, ale také většina komplexní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="dcfbb-123">Volat z libovolný vlastní kód, který umí vytvářet požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="dcfbb-124">Žádost o ověření pomocí certifikátu nebo Oauth uživatele hlavní / service hlavní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="dcfbb-125">Zadejte hodnoty parametrů jednoduché a komplexní.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="dcfbb-126">Sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-126">Track job state.</span></span> |
| [<span data-ttu-id="dcfbb-127">Webhooky</span><span class="sxs-lookup"><span data-stu-id="dcfbb-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="dcfbb-128">Spusťte runbook z jednoho požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="dcfbb-129">K ověření pomocí tokenu zabezpečení v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="dcfbb-130">Hodnoty parametrů zadané při vytvoření webhooku nejde přepsat klienta.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="dcfbb-131">Sada Runbook může definovat jeden parametr, který je naplněn hello podrobnosti požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-131">Runbook can define single parameter that is populated with hello HTTP request details.</span></span><br> <li><span data-ttu-id="dcfbb-132">Žádná možnost tootrack stav úlohy prostřednictvím URL webhooku se nenačetla.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-132">No ability tootrack job state through webhook URL.</span></span> |
| [<span data-ttu-id="dcfbb-133">Odpověď tooAzure výstrahy</span><span class="sxs-lookup"><span data-stu-id="dcfbb-133">Respond tooAzure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="dcfbb-134">Spuštění runbooku v odpovědi tooAzure výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-134">Start a runbook in response tooAzure alert.</span></span><br> <li><span data-ttu-id="dcfbb-135">Nakonfigurujte webhooku pro sadu runbook a propojit tooalert.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-135">Configure webhook for runbook and link tooalert.</span></span><br> <li><span data-ttu-id="dcfbb-136">K ověření pomocí tokenu zabezpečení v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="dcfbb-137">Plán</span><span class="sxs-lookup"><span data-stu-id="dcfbb-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="dcfbb-138">Runbook se automaticky spustí podle plánu hodinové, denní, týdenní nebo měsíční.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="dcfbb-139">Upravit plán prostřednictvím portálu Azure, rutiny prostředí PowerShell nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="dcfbb-140">Zadejte parametr hodnoty toobe použít s plánem.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-140">Provide parameter values toobe used with schedule.</span></span> |
| [<span data-ttu-id="dcfbb-141">Z jiného Runbooku</span><span class="sxs-lookup"><span data-stu-id="dcfbb-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="dcfbb-142">Sada runbook použijte jako aktivita v jiné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="dcfbb-143">Tato možnost je užitečná pro funkce, které používá více sad runbook.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="dcfbb-144">Zadejte parametr hodnoty toochild runbook a výstup použít v nadřazené sady runbook.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-144">Provide parameter values toochild runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="dcfbb-145">Hello následující obrázek ukazuje podrobné celým procesem v životním cyklu hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-145">hello following image illustrates detailed step-by-step process in hello life cycle of a runbook.</span></span> <span data-ttu-id="dcfbb-146">Zahrnuje to různými způsoby, kterými spuštění sady runbook ve službě Azure Automation komponent potřebných pro hybridní pracovní proces Runbooku tooexecute Azure Automation runbook a interakce mezi různými součástmi.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker tooexecute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="dcfbb-147">toolearn o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v příliš[procesy hybrid runbook Worker](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="dcfbb-147">toolearn about executing Automation runbooks in your datacenter, refer too[hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Architektura sady Runbook](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a><span data-ttu-id="dcfbb-149">Spuštění sady runbook s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dcfbb-149">Starting a runbook with hello Azure portal</span></span>
1. <span data-ttu-id="dcfbb-150">V hello portálu Azure, vyberte **automatizace** a pak klikněte hello název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-150">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="dcfbb-151">V nabídce centra hello vyberte **Runbooky**.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-151">On hello Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="dcfbb-152">Na hello **Runbooky** okně Vybrat sadu runbook a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-152">On hello **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="dcfbb-153">Pokud hello runbook obsahuje parametry, budou hodnoty výzvami tooprovide se textové pole pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-153">If hello runbook has parameters, you will be prompted tooprovide values with a text box for each parameter.</span></span> <span data-ttu-id="dcfbb-154">V tématu [parametry Runbooku](#Runbook-parameters) níže další podrobnosti o parametry.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="dcfbb-155">Na hello **úlohy** okno, můžete zobrazit stav úlohy runbooku hello hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-155">On hello **Job** blade, you can view hello status of hello runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="dcfbb-156">Spuštění sady runbook pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="dcfbb-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="dcfbb-157">Můžete použít hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart sady runbook pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-157">You can use hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a runbook with Windows PowerShell.</span></span> <span data-ttu-id="dcfbb-158">Hello následující vzorový kód spustí runbook s názvem Test-Runbook.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-158">hello following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="dcfbb-159">Vrátí AzureRmAutomationRunbook počáteční úlohu a objektů, které můžete použít tootrack její stav po spuštění runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-159">Start-AzureRmAutomationRunbook returns a job object that you can use tootrack its status once hello runbook is started.</span></span> <span data-ttu-id="dcfbb-160">Pak můžete použít tento objekt úlohy v [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello stav úlohy hello a [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget její výstup.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello status of hello job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget its output.</span></span> <span data-ttu-id="dcfbb-161">Hello následující vzorový kód spustí runbook s názvem Test-Runbook, počká byla dokončena a potom zobrazí jeho výstup.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-161">hello following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

<span data-ttu-id="dcfbb-162">Pokud hello runbook vyžaduje parametry, pak je nutné zadat jako [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) kde klíč zatřiďovací tabulky hello hello odpovídá hello parametr název a hodnotu hello je hodnota parametru hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-162">If hello runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key of hello hashtable matches hello parameter name and hello value is hello parameter value.</span></span> <span data-ttu-id="dcfbb-163">Hello následující příklad ukazuje, jak toostart runbooku se dvěma řetězcovými parametry s názvem FirstName a LastName, celočíselným parametrem s názvem RepeatCount a logickým parametrem s názvem Show.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-163">hello following example shows how toostart a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="dcfbb-164">Další informace o parametrech najdete v tématu [parametry Runbooku](#Runbook-parameters) níže.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="dcfbb-165">Parametry Runbooku</span><span class="sxs-lookup"><span data-stu-id="dcfbb-165">Runbook parameters</span></span>
<span data-ttu-id="dcfbb-166">Při spuštění runbooku z hello portálu Azure nebo prostředí Windows PowerShell hello instrukce se posílá prostřednictvím hello webové služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-166">When you start a runbook from hello Azure Portal or Windows PowerShell, hello instruction is sent through hello Azure Automation web service.</span></span> <span data-ttu-id="dcfbb-167">Tato služba nepodporuje parametry s komplexními datovými typy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="dcfbb-168">Pokud potřebujete tooprovide hodnotu komplexního parametru, pak je musíte ji volat z jiného runbooku jak je popsáno v [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="dcfbb-168">If you need tooprovide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="dcfbb-169">Hello webové služby Azure Automation nabízí zvláštní funkce pro parametry pomocí určitých datových typů, jak je popsáno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-169">hello Azure Automation web service will provide special functionality for parameters using certain data types as described in hello following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="dcfbb-170">Pojmenovaných hodnot</span><span class="sxs-lookup"><span data-stu-id="dcfbb-170">Named values</span></span>
<span data-ttu-id="dcfbb-171">Pokud parametr hello je datového typu [object], pak můžete použít následující toosend formátu JSON je seznam s názvem hodnoty hello: *{název1: 'Value1', NÁZEV2: 'Hodnota2', Name3: 'Hodnota3'}*.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-171">If hello parameter is data type [object], then you can use hello following JSON format toosend it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="dcfbb-172">Tyto hodnoty musí být jednoduché typy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-172">These values must be simple types.</span></span> <span data-ttu-id="dcfbb-173">Hello runbook obdrží parametr hello jako [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) s vlastnostmi, které odpovídají tooeach s názvem hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-173">hello runbook will receive hello parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond tooeach named value.</span></span>

<span data-ttu-id="dcfbb-174">Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-174">Consider hello following test runbook that accepts a parameter called user.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

<span data-ttu-id="dcfbb-175">Následující text Hello může použitý pro parametr hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-175">hello following text could be used for hello user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="dcfbb-176">Výsledkem je následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-176">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="dcfbb-177">Pole</span><span class="sxs-lookup"><span data-stu-id="dcfbb-177">Arrays</span></span>
<span data-ttu-id="dcfbb-178">Pokud je parametr hello pole, jako třeba [array] nebo [string []], můžete použít hello následující toosend formátu JSON je seznam hodnot: *[hodnota1, hodnota2, hodnota3]*.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-178">If hello parameter is an array such as [array] or [string[]], then you can use hello following JSON format toosend it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="dcfbb-179">Tyto hodnoty musí být jednoduché typy.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-179">These values must be simple types.</span></span>

<span data-ttu-id="dcfbb-180">Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem hello *uživatele*.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-180">Consider hello following test runbook that accepts a parameter called *user*.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

<span data-ttu-id="dcfbb-181">Následující text Hello může použitý pro parametr hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-181">hello following text could be used for hello user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="dcfbb-182">Výsledkem je následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-182">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="dcfbb-183">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="dcfbb-183">Credentials</span></span>
<span data-ttu-id="dcfbb-184">Pokud parametr hello je datový typ **PSCredential**, můžete zadat název hello Azure Automation [asset přihlašovacích údajů](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="dcfbb-184">If hello parameter is data type **PSCredential**, then you can provide hello name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="dcfbb-185">Hello runbook načte hello přihlašovací údaje s vámi určeným názvem hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-185">hello runbook will retrieve hello credential with hello name that you specify.</span></span>

<span data-ttu-id="dcfbb-186">Vezměte v úvahu hello následující testovací runbook, který přijme parametr s názvem přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-186">Consider hello following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="dcfbb-187">Hello následující text by mohly být použity hello parametr za předpokladu, že byla volána asset přihlašovacích údajů *moje pověření*.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-187">hello following text could be used for hello user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="dcfbb-188">Za předpokladu, že hello uživatelské jméno v přihlašovacích údajů hello *jsmith*, výsledkem je následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-188">Assuming hello username in hello credential was *jsmith*, this results in hello following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="dcfbb-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcfbb-189">Next steps</span></span>
* <span data-ttu-id="dcfbb-190">architektura sady runbook Hello v aktuální článek poskytuje souhrnné informace o správě prostředků sady runbook v Azure a místně s hello hybridní pracovní proces Runbooku.</span><span class="sxs-lookup"><span data-stu-id="dcfbb-190">hello runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with hello Hybrid Runbook Worker.</span></span>  <span data-ttu-id="dcfbb-191">toolearn o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v příliš[procesy Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="dcfbb-191">toolearn about executing Automation runbooks in your datacenter, refer too[Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="dcfbb-192">toolearn Další informace o vytváření toobe modulární runbooky používat ostatní runbooky pro konkrétní nebo běžné funkce hello odkazovat příliš[podřízené Runbooky](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="dcfbb-192">toolearn more about hello creating modular runbooks toobe used by other runbooks for specific or common functions, refer too[Child Runbooks](automation-child-runbooks.md).</span></span>

