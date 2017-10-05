---
title: "Spuštění sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Shrnuje různé metody, které můžete použít ke spuštění sady runbook ve službě Azure Automation a poskytuje podrobnosti o použití portálu Azure a prostředí Windows PowerShell."
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
ms.openlocfilehash: 844831b63d5263987ed05370125fbe9f01913ab9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="4ac05-103">Spuštění sady runbook ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="4ac05-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="4ac05-104">Následující tabulka vám pomůže určit metodu pro spuštění sady runbook ve službě Azure Automation, který je nejvhodnější k danému scénáři.</span><span class="sxs-lookup"><span data-stu-id="4ac05-104">The following table will help you determine the method to start a runbook in Azure Automation that is most suitable to your particular scenario.</span></span> <span data-ttu-id="4ac05-105">Tento článek obsahuje informace o spuštění sady runbook pomocí portálu Azure a pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac05-105">This article includes details on starting a runbook with the Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="4ac05-106">Informace o jiných metod jsou uvedeny v jiných dokumentace, která je přístupné z níže uvedených odkazů.</span><span class="sxs-lookup"><span data-stu-id="4ac05-106">Details on the other methods are provided in other documentation that you can access from the links below.</span></span>

| <span data-ttu-id="4ac05-107">**– METODA**</span><span class="sxs-lookup"><span data-stu-id="4ac05-107">**METHOD**</span></span> | <span data-ttu-id="4ac05-108">**VLASTNOSTI**</span><span class="sxs-lookup"><span data-stu-id="4ac05-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="4ac05-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ac05-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="4ac05-110">Nejjednodušším způsobem s interaktivní uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="4ac05-111">Formulář zadat jednoduchý parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4ac05-111">Form to provide simple parameter values.</span></span><br> <li><span data-ttu-id="4ac05-112">Snadno sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-112">Easily track job state.</span></span><br> <li><span data-ttu-id="4ac05-113">Přístup k ověření pomocí přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="4ac05-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="4ac05-114">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ac05-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="4ac05-115">Volání z příkazového řádku pomocí rutin prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac05-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="4ac05-116">Můžou být součástí automatizované řešení s více kroků.</span><span class="sxs-lookup"><span data-stu-id="4ac05-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="4ac05-117">Ověření žádosti o certifikát nebo OAuth uživatele hlavní / service hlavní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="4ac05-118">Zadejte hodnoty parametrů jednoduché a komplexní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="4ac05-119">Sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-119">Track job state.</span></span><br> <li><span data-ttu-id="4ac05-120">Klient potřebné k podpoře rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac05-120">Client required to support PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="4ac05-121">Rozhraní API služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="4ac05-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="4ac05-122">Nejflexibilnější, ale také většina komplexní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="4ac05-123">Volat z libovolný vlastní kód, který umí vytvářet požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="4ac05-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="4ac05-124">Žádost o ověření pomocí certifikátu nebo Oauth uživatele hlavní / service hlavní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="4ac05-125">Zadejte hodnoty parametrů jednoduché a komplexní.</span><span class="sxs-lookup"><span data-stu-id="4ac05-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="4ac05-126">Sledovat stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-126">Track job state.</span></span> |
| [<span data-ttu-id="4ac05-127">Webhooky</span><span class="sxs-lookup"><span data-stu-id="4ac05-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="4ac05-128">Spusťte runbook z jednoho požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="4ac05-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="4ac05-129">K ověření pomocí tokenu zabezpečení v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="4ac05-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="4ac05-130">Hodnoty parametrů zadané při vytvoření webhooku nejde přepsat klienta.</span><span class="sxs-lookup"><span data-stu-id="4ac05-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="4ac05-131">Sada Runbook může definovat jeden parametr, který je naplněn podrobnosti požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="4ac05-131">Runbook can define single parameter that is populated with the HTTP request details.</span></span><br> <li><span data-ttu-id="4ac05-132">Žádná možnost sledovat stav úlohy prostřednictvím URL webhooku se nenačetla.</span><span class="sxs-lookup"><span data-stu-id="4ac05-132">No ability to track job state through webhook URL.</span></span> |
| [<span data-ttu-id="4ac05-133">Reakce na výstrahy Azure</span><span class="sxs-lookup"><span data-stu-id="4ac05-133">Respond to Azure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="4ac05-134">Spuštění sady runbook v reakci na výstrahy Azure.</span><span class="sxs-lookup"><span data-stu-id="4ac05-134">Start a runbook in response to Azure alert.</span></span><br> <li><span data-ttu-id="4ac05-135">Nakonfigurujte webhooku pro sadu runbook a odkaz na výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-135">Configure webhook for runbook and link to alert.</span></span><br> <li><span data-ttu-id="4ac05-136">K ověření pomocí tokenu zabezpečení v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="4ac05-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="4ac05-137">Plán</span><span class="sxs-lookup"><span data-stu-id="4ac05-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="4ac05-138">Runbook se automaticky spustí podle plánu hodinové, denní, týdenní nebo měsíční.</span><span class="sxs-lookup"><span data-stu-id="4ac05-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="4ac05-139">Upravit plán prostřednictvím portálu Azure, rutiny prostředí PowerShell nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="4ac05-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="4ac05-140">Zadejte hodnoty parametrů, který se má použít s plánem.</span><span class="sxs-lookup"><span data-stu-id="4ac05-140">Provide parameter values to be used with schedule.</span></span> |
| [<span data-ttu-id="4ac05-141">Z jiného Runbooku</span><span class="sxs-lookup"><span data-stu-id="4ac05-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="4ac05-142">Sada runbook použijte jako aktivita v jiné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="4ac05-143">Tato možnost je užitečná pro funkce, které používá více sad runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="4ac05-144">Zadejte hodnoty parametrů pro podřízené sady runbook a použít výstup v nadřazené sady runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-144">Provide parameter values to child runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="4ac05-145">Následující obrázek ukazuje podrobné celým procesem v životním cyklu sady runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-145">The following image illustrates detailed step-by-step process in the life cycle of a runbook.</span></span> <span data-ttu-id="4ac05-146">Obsahuje různé způsoby spuštění sady runbook ve službě Azure Automation, součásti požadované pro hybridní pracovní proces Runbooku provést sad Azure Automation runbook a interakce mezi různými součástmi.</span><span class="sxs-lookup"><span data-stu-id="4ac05-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker to execute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="4ac05-147">Další informace o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v tématu [procesy hybrid runbook Worker](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="4ac05-147">To learn about executing Automation runbooks in your datacenter, refer to [hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Architektura sady Runbook](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a><span data-ttu-id="4ac05-149">Spuštění sady runbook pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4ac05-149">Starting a runbook with the Azure portal</span></span>
1. <span data-ttu-id="4ac05-150">Na portálu Azure vyberte **automatizace** a pak klikněte na název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="4ac05-150">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="4ac05-151">V nabídce centra vyberte **Runbooky**.</span><span class="sxs-lookup"><span data-stu-id="4ac05-151">On the Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="4ac05-152">Na **Runbooky** okně Vybrat sadu runbook a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4ac05-152">On the **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="4ac05-153">Pokud sada runbook obsahuje parametry, budete vyzváni k zadání hodnoty s textové pole pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="4ac05-153">If the runbook has parameters, you will be prompted to provide values with a text box for each parameter.</span></span> <span data-ttu-id="4ac05-154">V tématu [parametry Runbooku](#Runbook-parameters) níže další podrobnosti o parametry.</span><span class="sxs-lookup"><span data-stu-id="4ac05-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="4ac05-155">Na **úlohy** okno, můžete zobrazit stav úlohy sady runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-155">On the **Job** blade, you can view the status of the runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="4ac05-156">Spuštění sady runbook pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ac05-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="4ac05-157">Můžete použít [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) spuštění runbooku pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac05-157">You can use the [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) to start a runbook with Windows PowerShell.</span></span> <span data-ttu-id="4ac05-158">Následující vzorový kód spustí runbook s názvem Test-Runbook.</span><span class="sxs-lookup"><span data-stu-id="4ac05-158">The following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="4ac05-159">Spuštění AzureRmAutomationRunbook vrátí objekt úlohy, které můžete použít ke sledování jeho stavu po spuštění runbooku.</span><span class="sxs-lookup"><span data-stu-id="4ac05-159">Start-AzureRmAutomationRunbook returns a job object that you can use to track its status once the runbook is started.</span></span> <span data-ttu-id="4ac05-160">Pak můžete použít tento objekt úlohy v [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) k určení stavu úlohy a [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) k získání jejího výstupu.</span><span class="sxs-lookup"><span data-stu-id="4ac05-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) to determine the status of the job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) to get its output.</span></span> <span data-ttu-id="4ac05-161">Následující vzorový kód spustí runbook s názvem Test-Runbook, počká byla dokončena a potom zobrazí jeho výstup.</span><span class="sxs-lookup"><span data-stu-id="4ac05-161">The following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

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

<span data-ttu-id="4ac05-162">Pokud runbook vyžaduje parametry, pak je nutné zadat jako [zatřiďovací tabulky](http://technet.microsoft.com/library/hh847780.aspx) kde klíč zatřiďovací tabulky odpovídá názvu parametru a hodnota je hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="4ac05-162">If the runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key of the hashtable matches the parameter name and the value is the parameter value.</span></span> <span data-ttu-id="4ac05-163">Následující příklad ukazuje spuštění runbooku se dvěma řetězcovými parametry s názvem FirstName a LastName, celočíselným parametrem s názvem RepeatCount a logickým parametrem s názvem Show.</span><span class="sxs-lookup"><span data-stu-id="4ac05-163">The following example shows how to start a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="4ac05-164">Další informace o parametrech najdete v tématu [parametry Runbooku](#Runbook-parameters) níže.</span><span class="sxs-lookup"><span data-stu-id="4ac05-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="4ac05-165">Parametry Runbooku</span><span class="sxs-lookup"><span data-stu-id="4ac05-165">Runbook parameters</span></span>
<span data-ttu-id="4ac05-166">Při spuštění runbooku z portálu Azure nebo prostředí Windows PowerShell, instrukce se posílá prostřednictvím webové služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4ac05-166">When you start a runbook from the Azure Portal or Windows PowerShell, the instruction is sent through the Azure Automation web service.</span></span> <span data-ttu-id="4ac05-167">Tato služba nepodporuje parametry s komplexními datovými typy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="4ac05-168">Pokud je třeba zadat hodnotu komplexního parametru, pak je musíte ji volat z jiného runbooku jak je popsáno v [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="4ac05-168">If you need to provide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="4ac05-169">Webové služby Azure Automation nabízí zvláštní funkce pro parametry pomocí určitých datových typů, jak je popsáno v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="4ac05-169">The Azure Automation web service will provide special functionality for parameters using certain data types as described in the following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="4ac05-170">Pojmenovaných hodnot</span><span class="sxs-lookup"><span data-stu-id="4ac05-170">Named values</span></span>
<span data-ttu-id="4ac05-171">Pokud je parametr datového typu [object], pak do něj poslat seznam pojmenovaných hodnot můžete pomocí následujícího formátu JSON: *{název1: 'Value1', NÁZEV2: 'Hodnota2', Name3: 'Hodnota3'}*.</span><span class="sxs-lookup"><span data-stu-id="4ac05-171">If the parameter is data type [object], then you can use the following JSON format to send it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="4ac05-172">Tyto hodnoty musí být jednoduché typy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-172">These values must be simple types.</span></span> <span data-ttu-id="4ac05-173">Runbook obdrží parametr jako [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) s vlastnostmi, které odpovídají každé pojmenované hodnotě.</span><span class="sxs-lookup"><span data-stu-id="4ac05-173">The runbook will receive the parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond to each named value.</span></span>

<span data-ttu-id="4ac05-174">Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem uživatele.</span><span class="sxs-lookup"><span data-stu-id="4ac05-174">Consider the following test runbook that accepts a parameter called user.</span></span>

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

<span data-ttu-id="4ac05-175">Následující text by bylo možné pro tento parametr.</span><span class="sxs-lookup"><span data-stu-id="4ac05-175">The following text could be used for the user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="4ac05-176">Výsledkem je následující výstup.</span><span class="sxs-lookup"><span data-stu-id="4ac05-176">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="4ac05-177">Pole</span><span class="sxs-lookup"><span data-stu-id="4ac05-177">Arrays</span></span>
<span data-ttu-id="4ac05-178">Pokud je parametr pole, jako třeba [array] nebo [string []], můžete do něj poslat seznam hodnot pomocí následujícího formátu JSON: *[hodnota1, hodnota2, hodnota3]*.</span><span class="sxs-lookup"><span data-stu-id="4ac05-178">If the parameter is an array such as [array] or [string[]], then you can use the following JSON format to send it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="4ac05-179">Tyto hodnoty musí být jednoduché typy.</span><span class="sxs-lookup"><span data-stu-id="4ac05-179">These values must be simple types.</span></span>

<span data-ttu-id="4ac05-180">Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem *uživatele*.</span><span class="sxs-lookup"><span data-stu-id="4ac05-180">Consider the following test runbook that accepts a parameter called *user*.</span></span>

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

<span data-ttu-id="4ac05-181">Následující text by bylo možné pro tento parametr.</span><span class="sxs-lookup"><span data-stu-id="4ac05-181">The following text could be used for the user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="4ac05-182">Výsledkem je následující výstup.</span><span class="sxs-lookup"><span data-stu-id="4ac05-182">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="4ac05-183">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="4ac05-183">Credentials</span></span>
<span data-ttu-id="4ac05-184">Pokud je parametr datový typ **PSCredential**, můžete zadat název Azure Automation [asset přihlašovacích údajů](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="4ac05-184">If the parameter is data type **PSCredential**, then you can provide the name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="4ac05-185">Runbook načte přihlašovací údaje s názvem, který určíte.</span><span class="sxs-lookup"><span data-stu-id="4ac05-185">The runbook will retrieve the credential with the name that you specify.</span></span>

<span data-ttu-id="4ac05-186">Vezměte v úvahu následující testovací runbook, který přijme parametr s názvem přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4ac05-186">Consider the following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="4ac05-187">Následující text by mohly být použity parametr user za předpokladu, že došlo asset přihlašovacích údajů názvem *moje pověření*.</span><span class="sxs-lookup"><span data-stu-id="4ac05-187">The following text could be used for the user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="4ac05-188">Za předpokladu, že uživatelské jméno v přihlašovacích údajích bylo *jsmith*, výsledkem je následující výstup.</span><span class="sxs-lookup"><span data-stu-id="4ac05-188">Assuming the username in the credential was *jsmith*, this results in the following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="4ac05-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ac05-189">Next steps</span></span>
* <span data-ttu-id="4ac05-190">Architektura sady runbook v aktuální článek poskytuje souhrnné informace o správě prostředků sady runbook v Azure a místně s hybridní pracovní proces Runbooku.</span><span class="sxs-lookup"><span data-stu-id="4ac05-190">The runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with the Hybrid Runbook Worker.</span></span>  <span data-ttu-id="4ac05-191">Další informace o spouštění runbooků služeb automatizace ve vašem datovém centru, najdete v tématu [procesy Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="4ac05-191">To learn about executing Automation runbooks in your datacenter, refer to [Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="4ac05-192">Další informace o vytváření modulární runbooky tak, aby se používat ostatní runbooky pro konkrétní nebo běžné funkce, najdete v tématu [podřízené Runbooky](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="4ac05-192">To learn more about the creating modular runbooks to be used by other runbooks for specific or common functions, refer to [Child Runbooks](automation-child-runbooks.md).</span></span>

