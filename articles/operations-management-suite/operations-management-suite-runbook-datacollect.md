---
title: "Shromažďování dat analýzy protokolů se sadou runbook ve službě Azure Automation | Microsoft Docs"
description: "Podrobný kurz, který provede procesem vytvoření sady runbook ve službě Azure Automation ke shromažďování dat do úložiště OMS pro analýzu podle analýzy protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 59f674c9c6404da7f5384539189f41a4ba1a939a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="8c1f8-103">Shromáždit data Log Analytics s runbook služby automatizace Azure</span><span class="sxs-lookup"><span data-stu-id="8c1f8-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="8c1f8-104">Můžete shromáždit významné množství dat v analýzy protokolů z různých zdrojů včetně [zdroje dat](../log-analytics/log-analytics-data-sources.md) na agentech a také [data shromážděná z Azure](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="8c1f8-105">Když potřebujete-li shromažďovat data, není přístupná prostřednictvím těchto zdrojů je standardní existují scénáře.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-105">There are a scenarios though where you need to collect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="8c1f8-106">V těchto případech můžete použít [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md) při zápisu dat k analýze protokolů z libovolného klienta REST API.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-106">In these cases, you can use the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) to write data to Log Analytics from any REST API client.</span></span>  <span data-ttu-id="8c1f8-107">Běžnou metodou k provedení této kolekce dat používá sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-107">A common method to perform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="8c1f8-108">Tento kurz vás provede proces pro vytvoření a plánování runbooku ve službě Azure Automation k zápisu dat k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-108">This tutorial walks through the process for creating and scheduling a runbook in Azure Automation to write data to Log Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="8c1f8-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8c1f8-109">Prerequisites</span></span>
<span data-ttu-id="8c1f8-110">Tento scénář vyžaduje následující prostředky, které jsou nakonfigurované ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-110">This scenario requires the following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="8c1f8-111">Jak může být bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-111">Both can be a free account.</span></span>

- <span data-ttu-id="8c1f8-112">[Přihlaste se pracovní prostor analýzy](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="8c1f8-113">[Účet Azure automation](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="8c1f8-114">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="8c1f8-114">Overview of scenario</span></span>
<span data-ttu-id="8c1f8-115">V tomto kurzu napíšete sadu runbook, která shromažďuje informace o automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="8c1f8-116">Runbooky ve službě Azure Automation jsou implementované v prostředí PowerShell, spusťte psaní a testování skript v editoru Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in the Azure Automation editor.</span></span>  <span data-ttu-id="8c1f8-117">Jakmile ověříte, že shromažďujete požadované informace, budete zápisu dat k analýze protokolů a ověřte typ vlastní data.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-117">Once you verify that you're collecting the required information, you'll write that data to Log Analytics and verify the custom data type.</span></span>  <span data-ttu-id="8c1f8-118">Nakonec vytvoříte plán, který chcete spouštět sadu runbook v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-118">Finally, you'll create a schedule to start the runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="8c1f8-119">Můžete nakonfigurovat automatizace Azure, aby odesílat informace o úloze k analýze protokolů bez této sady runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-119">You can configure Azure Automation to send job information to Log Analytics without this runbook.</span></span>  <span data-ttu-id="8c1f8-120">Tento scénář se používá hlavně pro podporu tohoto kurzu a doporučuje posílat data do pracovního prostoru testu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-120">This scenario is primarily used to support the tutorial, and it's recommended that you send the data to a test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="8c1f8-121">1. Instalace modulu rozhraní API sady kolekcí dat</span><span class="sxs-lookup"><span data-stu-id="8c1f8-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="8c1f8-122">Každý [žádosti z rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md#create-a-request) musí být správně naformátován a obsahovat hlavičku autorizace.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-122">Every [request from the HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="8c1f8-123">Můžete to provést ve vašem runbooku, ale můžete snížit množství pomocí modulu, který zjednodušuje tento proces vyžaduje kód.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-123">You can do this in your runbook, but you can reduce the amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="8c1f8-124">Jeden modul, který můžete použít se [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI) v galerii prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in the PowerShell Gallery.</span></span>

<span data-ttu-id="8c1f8-125">Použít [modulu](../automation/automation-integration-modules.md) v sadě runbook, musí být nainstalován ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-125">To use a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="8c1f8-126">Všechny sady runbook ve stejném účtu pak můžete použít funkce v modulu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-126">Any runbook in the same account can then use the functions in the module.</span></span>  <span data-ttu-id="8c1f8-127">Můžete nainstalovat nový modul výběrem **prostředky** > **moduly** > **přidat modul** ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="8c1f8-128">Galerie prostředí PowerShell ale nabízí rychlou možnost k nasazení modul přímo na účtu automation, proto tuto možnost můžete použít pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-128">The PowerShell Gallery though gives you a quick option to deploy a module directly to your automation account so you can use that option for this tutorial.</span></span>  

![Modul OMSIngestionAPI](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="8c1f8-130">Přejděte na [Galerie prostředí PowerShell](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-130">Go to [PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="8c1f8-131">Vyhledejte **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="8c1f8-132">Klikněte na **nasadit do Azure Automation** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-132">Click on the **Deploy to Azure Automation** button.</span></span>
4. <span data-ttu-id="8c1f8-133">Vyberte svůj účet automation a klikněte na **OK** nainstalovat modul.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-133">Select your automation account and click **OK** to install the module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="8c1f8-134">2. Vytvoření proměnné služeb automatizace</span><span class="sxs-lookup"><span data-stu-id="8c1f8-134">2. Create Automation variables</span></span>
<span data-ttu-id="8c1f8-135">[Proměnné služeb automatizace](..\automation\automation-variables.md) obsahovat hodnoty, které mohou být využívána všem runbookům v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="8c1f8-136">Provádění sady runbook flexibilnější tím, že se tyto hodnoty změnit bez úprav skutečné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-136">They make runbooks more flexible by allowing you to change these values without editing the actual runbook.</span></span> <span data-ttu-id="8c1f8-137">Každý požadavek z rozhraní API sady kolekcí dat protokolu HTTP vyžaduje ID a klíč pracovního prostoru OMS a proměnné prostředky jsou ideální pro ukládání těchto informací.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-137">Every request from the HTTP Data Collector API requires the ID and key of the OMS workspace, and variable assets are ideal to store this information.</span></span>  

![Proměnné](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="8c1f8-139">Na portálu Azure přejděte na svůj účet Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-139">In the Azure portal, navigate to your Automation account.</span></span>
2. <span data-ttu-id="8c1f8-140">Vyberte **proměnné** pod **sdílené prostředky**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="8c1f8-141">Klikněte na tlačítko **přidat proměnnou** a vytvořte dvě proměnné, které v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-141">Click **Add a variable** and create the two variables in the following table.</span></span>

| <span data-ttu-id="8c1f8-142">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8c1f8-142">Property</span></span> | <span data-ttu-id="8c1f8-143">Hodnota ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="8c1f8-143">Workspace ID Value</span></span> | <span data-ttu-id="8c1f8-144">Hodnota klíče pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="8c1f8-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="8c1f8-145">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8c1f8-145">Name</span></span> | <span data-ttu-id="8c1f8-146">ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="8c1f8-146">WorkspaceId</span></span> | <span data-ttu-id="8c1f8-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="8c1f8-147">WorkspaceKey</span></span> |
| <span data-ttu-id="8c1f8-148">Typ</span><span class="sxs-lookup"><span data-stu-id="8c1f8-148">Type</span></span> | <span data-ttu-id="8c1f8-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8c1f8-149">String</span></span> | <span data-ttu-id="8c1f8-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8c1f8-150">String</span></span> |
| <span data-ttu-id="8c1f8-151">Hodnota</span><span class="sxs-lookup"><span data-stu-id="8c1f8-151">Value</span></span> | <span data-ttu-id="8c1f8-152">Vložte ID pracovního prostoru pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-152">Paste in the Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="8c1f8-153">Vkládání pomocí primární nebo sekundární klíč pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-153">Paste in with the Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="8c1f8-154">Šifrované</span><span class="sxs-lookup"><span data-stu-id="8c1f8-154">Encrypted</span></span> | <span data-ttu-id="8c1f8-155">Ne</span><span class="sxs-lookup"><span data-stu-id="8c1f8-155">No</span></span> | <span data-ttu-id="8c1f8-156">Ano</span><span class="sxs-lookup"><span data-stu-id="8c1f8-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="8c1f8-157">3. Vytvoření sady runbook</span><span class="sxs-lookup"><span data-stu-id="8c1f8-157">3. Create runbook</span></span>

<span data-ttu-id="8c1f8-158">Automatizace Azure má editoru na portálu, kde můžete upravit a otestujte svůj runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-158">Azure Automation has an editor in the portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="8c1f8-159">Máte možnost použít editor skriptů pro práci s [prostředí PowerShell přímo](../automation/automation-edit-textual-runbook.md) nebo [vytvořit grafický runbook](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-159">You have the option to use the script editor to work with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="8c1f8-160">V tomto kurzu bude fungovat se skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Úprava runbooku](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="8c1f8-162">Přejděte na svůj účet Automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-162">Navigate to your Automation account.</span></span>  
2. <span data-ttu-id="8c1f8-163">Klikněte na tlačítko **Runbooky** > **přidat runbook** > **vytvořit nový runbook**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="8c1f8-164">Název sady runbook, zadejte **shromažďování. automatizace úloh**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-164">For the runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="8c1f8-165">Typ runbooku, vyberte **prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-165">For the runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="8c1f8-166">Klikněte na tlačítko **vytvořit** vytvořit sadu runbook a spusťte editor.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-166">Click **Create** to create the runbook and start the editor.</span></span>
5. <span data-ttu-id="8c1f8-167">Zkopírujte a vložte následující kód do runbooku.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-167">Copy and paste the following code into the runbook.</span></span>  <span data-ttu-id="8c1f8-168">Komentáře ve skriptu pro vysvětlení kódu odkazovat.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-168">Refer to the comments in the script for explanation of the code.</span></span>
    
        # Get information required for the automation account from parameter values when the runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate to the Automation account using the Azure connection created when the Automation account was created.
        # Code copied from the runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set the $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set the name of the record type.
        $logType = "AutomationJob"
        
        # Get the jobs from the past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert the job data to json
            $body = $jobs | ConvertTo-Json
        
            # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
            Write-Verbose $body
        
            # Send the data to Log Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="8c1f8-169">4. Služba test runbook</span><span class="sxs-lookup"><span data-stu-id="8c1f8-169">4. Test runbook</span></span>
<span data-ttu-id="8c1f8-170">Zahrnuje prostředí do služby Azure Automation [Otestujte svůj runbook](../automation/automation-testing-runbook.md) před publikováním.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-170">Azure Automation includes an environment to [test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="8c1f8-171">Můžou kontrolovat data shromažďovaná společností sady runbook a ověřte ji k analýze protokolů zapíše podle očekávání před publikováním do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-171">You can inspect the data collected by the runbook and verify that it writes to Log Analytics as expected before publishing it to production.</span></span> 
 
![Služba test runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="8c1f8-173">Klikněte na tlačítko **Uložit** uložit sady runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-173">Click **Save** to save the runbook.</span></span>
1. <span data-ttu-id="8c1f8-174">Klikněte na tlačítko **testovací podokno** otevřete sadu runbook v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-174">Click **Test pane** to open the runbook in the test environment.</span></span>
3. <span data-ttu-id="8c1f8-175">Vzhledem k tomu, že vaše sada runbook obsahuje parametry, se zobrazí výzva k zadání hodnot pro ně.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-175">Since your runbook has parameters, you're prompted to enter values for them.</span></span>  <span data-ttu-id="8c1f8-176">Zadejte název skupiny prostředků a automatizace účet, který budete shromažďovat informace o úlohách z.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-176">Enter the name of the resource group and the automation account that your going to collect job information from.</span></span>
4. <span data-ttu-id="8c1f8-177">Klikněte na tlačítko **spustit** spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-177">Click **Start** to the start the runbook.</span></span>
3. <span data-ttu-id="8c1f8-178">Runbook se spustí se stavem **zařazeno ve frontě** předtím, než přejdete do **systémem**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-178">The runbook will start with a status of **Queued** before it goes to **Running**.</span></span>  
3. <span data-ttu-id="8c1f8-179">Sada runbook by měl zobrazit podrobný výstup s úlohami shromážděných ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-179">The runbook should display verbose output with the jobs collected in json format.</span></span>  <span data-ttu-id="8c1f8-180">Pokud nejsou uvedeny žádné úlohy, pak může byly žádné úlohy vytvořené v účtu automation za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-180">If no jobs are listed, then there may have been no jobs created in the automation account in the last hour.</span></span>  <span data-ttu-id="8c1f8-181">Pokuste se spustit žádné sady runbook v účtu automation a potom proveďte test znovu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-181">Try starting any runbook in the automation account and then perform the test again.</span></span>
4. <span data-ttu-id="8c1f8-182">Ujistěte se, že výstup nezobrazí žádné chyby v příkazu post k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-182">Ensure that the output doesn't show any errors in the post command to Log Analytics.</span></span>  <span data-ttu-id="8c1f8-183">Měli byste mít zprávu podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-183">You should have a message similar to the following.</span></span>

    ![Výstup POST](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="8c1f8-185">5. Ověřit záznamy v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8c1f8-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="8c1f8-186">Po sada runbook byla dokončena v testu, a ověřit, že byly úspěšně získány výstup, můžete ověřit, že záznamy byly vytvořené pomocí [hledání protokolů v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-186">Once the runbook has completed in test, and you verified that the output was successfully received, you can verify that the records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Protokolování výstupu](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="8c1f8-188">Na portálu Azure vyberte pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-188">In the Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="8c1f8-189">Klikněte na **protokolu vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="8c1f8-190">Zadejte následující příkaz `Type=AutomationJob_CL` a klikněte na tlačítko Hledat.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-190">Type the following command `Type=AutomationJob_CL` and click the search button.</span></span> <span data-ttu-id="8c1f8-191">Všimněte si, že typ záznamu obsahuje _CL, které není zadané ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-191">Note that the record type includes _CL that isn't specified in the script.</span></span>  <span data-ttu-id="8c1f8-192">Tuto příponu automaticky připojena typ protokolu to znamená, že je typ vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-192">That suffix is automatically appended to the log type to indicate that it's a custom log type.</span></span>
4. <span data-ttu-id="8c1f8-193">Měli byste vidět jeden nebo více záznamů vrátil, která určuje, že sada runbook funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-193">You should see one or more records returned indicating that the runbook is working as expected.</span></span>


## <a name="6-publish-the-runbook"></a><span data-ttu-id="8c1f8-194">6. Publikovat sadu runbook</span><span class="sxs-lookup"><span data-stu-id="8c1f8-194">6. Publish the runbook</span></span>
<span data-ttu-id="8c1f8-195">Jakmile se ujistíte, že runbook správně funguje, musíte ho publikujete, aby ji můžete spustit v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-195">Once you've verified that the runbook is working correctly, you need to publish it so you can run it in production.</span></span>  <span data-ttu-id="8c1f8-196">Můžete upravit a otestování sady runbook beze změny publikovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-196">You can continue to edit and test the runbook without modifying the published version.</span></span>  

![Publikování sady runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="8c1f8-198">Vraťte se k účtu automation.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-198">Return to your automation account.</span></span>
2. <span data-ttu-id="8c1f8-199">Klikněte na **Runbooky** a vyberte **shromažďování. automatizace úloh**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="8c1f8-200">Klikněte na tlačítko **upravit** a potom **publikování**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="8c1f8-201">Klikněte na tlačítko **Ano** při výzva, abyste ověřili, že chcete přepsat dříve publikovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-201">Click **Yes** when asked to verify that you want to overwrite the previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="8c1f8-202">7. Nastavení možností protokolování</span><span class="sxs-lookup"><span data-stu-id="8c1f8-202">7. Set logging options</span></span> 
<span data-ttu-id="8c1f8-203">Pro test, bylo možné zobrazit [podrobný výstup](../automation/automation-runbook-output-and-messages.md#message-streams) protože nastavte proměnnou $VerbosePreference ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-203">For test, you were able to view [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set the $VerbosePreference variable in the script.</span></span>  <span data-ttu-id="8c1f8-204">V produkčním prostředí budete muset nastavit vlastnosti protokolování pro sady runbook, pokud chcete zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-204">For production, you need to set the logging properties for the runbook if you want to view verbose output.</span></span>  <span data-ttu-id="8c1f8-205">Pro sadu runbook v tomto kurzu použít bude se zobrazovat data json odesílány k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-205">For the runbook used in this tutorial, this will display the json data being sent to Log Analytics.</span></span>

![Protokolování a trasování](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="8c1f8-207">Ve vlastnostech pro vaše sada runbook vyberte **protokolování a trasování** pod **nastavení sady Runbook**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-207">In the properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="8c1f8-208">Změna nastavení pro **protokolování podrobných záznamů** k **na**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-208">Change the setting for **Log verbose records** to **On**.</span></span>
3. <span data-ttu-id="8c1f8-209">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="8c1f8-210">8. Plánování runbooku</span><span class="sxs-lookup"><span data-stu-id="8c1f8-210">8. Schedule runbook</span></span>
<span data-ttu-id="8c1f8-211">Nejběžnější způsob spuštění sady runbook, která shromažďuje data monitorování je naplánovat její automatické spouštění.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-211">The most common way to start a runbook that collects monitoring data is to schedule it to run automatically.</span></span>  <span data-ttu-id="8c1f8-212">To uděláte tak, že vytvoříte [plán ve službě Azure Automation](../automation/automation-schedules.md) a připojíte ho k sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it to your runbook.</span></span>

![Plánování runbooku](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="8c1f8-214">Ve vlastnostech pro vaše sada runbook, zvolte **plány** pod **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-214">In the properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="8c1f8-215">Klikněte na tlačítko **přidat plán** > **propojit plán s runbookem** > **vytvořte nový plán**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-215">Click **Add a schedule** > **Link a schedule to your runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="8c1f8-216">Zadejte následující hodnoty pro plán a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-216">Type in the following values for the schedule and click **Create**.</span></span>

| <span data-ttu-id="8c1f8-217">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8c1f8-217">Property</span></span> | <span data-ttu-id="8c1f8-218">Hodnota</span><span class="sxs-lookup"><span data-stu-id="8c1f8-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="8c1f8-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8c1f8-219">Name</span></span> | <span data-ttu-id="8c1f8-220">AutomationJobs-každou hodinu</span><span class="sxs-lookup"><span data-stu-id="8c1f8-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="8c1f8-221">Spustí</span><span class="sxs-lookup"><span data-stu-id="8c1f8-221">Starts</span></span> | <span data-ttu-id="8c1f8-222">Vyberte, kdykoli se alespoň 5 minut po aktuálním čase.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-222">Select any time at least 5 minutes past the current time.</span></span> |
| <span data-ttu-id="8c1f8-223">Opakování</span><span class="sxs-lookup"><span data-stu-id="8c1f8-223">Recurrence</span></span> | <span data-ttu-id="8c1f8-224">Opakování</span><span class="sxs-lookup"><span data-stu-id="8c1f8-224">Recurring</span></span> |
| <span data-ttu-id="8c1f8-225">Opakovat každých</span><span class="sxs-lookup"><span data-stu-id="8c1f8-225">Recur every</span></span> | <span data-ttu-id="8c1f8-226">1 hodina</span><span class="sxs-lookup"><span data-stu-id="8c1f8-226">1 hour</span></span> |
| <span data-ttu-id="8c1f8-227">Sada vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="8c1f8-227">Set expiration</span></span> | <span data-ttu-id="8c1f8-228">Ne</span><span class="sxs-lookup"><span data-stu-id="8c1f8-228">No</span></span> |

<span data-ttu-id="8c1f8-229">Po vytvoření plánu, budete muset nastavit hodnoty parametrů, které se použije vždy, když tento plán spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-229">Once the schedule is created, you need to set the parameter values that will be used each time this schedule starts the runbook.</span></span>

6. <span data-ttu-id="8c1f8-230">Klikněte na tlačítko **nakonfigurovat parametry a nastavení spouštění**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="8c1f8-231">Zadejte hodnoty pro vaše **ResourceGroupName** a **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="8c1f8-232">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="8c1f8-233">9. Ověřte, sada runbook spustí podle plánu.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="8c1f8-234">Při spuštění sady runbook [se vytvoří úloha](../automation/automation-runbook-execution.md) a jakéhokoli výstupu protokolována.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="8c1f8-235">Ve skutečnosti jedná se o stejné úlohy, které sada runbook je shromažďování.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-235">In fact, these are the same jobs that the runbook is collecting.</span></span>  <span data-ttu-id="8c1f8-236">Můžete ověřit, že sada runbook spustí podle očekávání kontrolou úlohy pro runbook po uplynutí čas zahájení pro plán.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-236">You can verify that the runbook starts as expected by checking the jobs for the runbook after the start time for the schedule has passed.</span></span>

![Úlohy](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="8c1f8-238">Ve vlastnostech pro vaše sada runbook, zvolte **úlohy** pod **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-238">In the properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="8c1f8-239">Měli byste vidět v seznamu úloh pro pokaždé, když sada runbook byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-239">You should see a listing of jobs for each time the runbook was started.</span></span>
3. <span data-ttu-id="8c1f8-240">Klikněte na jednu z úlohy zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-240">Click on one of the jobs to view its details.</span></span>
4. <span data-ttu-id="8c1f8-241">Klikněte na **všechny protokoly** k zobrazení protokolů a výstup z runbooku.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-241">Click on **All logs** to view the logs and output from the runbook.</span></span>
5. <span data-ttu-id="8c1f8-242">Posuňte se dolů, chcete-li položku Najít podobná následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-242">Scroll to the bottom to find an entry similar to the image below.</span></span><br>![Verbose](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="8c1f8-244">Kliknutím na tuto položku, chcete-li zobrazit podrobné json data, která byla odeslána k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-244">Click on this entry to view the detailed json data  that was sent to Log Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="8c1f8-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c1f8-245">Next steps</span></span>
- <span data-ttu-id="8c1f8-246">Použití [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) vytvoření zobrazení zobrazení data, která jste shromážděných do úložiště analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) to create a view displaying the data that you've collected to the Log Analytics repository.</span></span>
- <span data-ttu-id="8c1f8-247">Balíček svoji sadu runbook v [řešení pro správu](operations-management-suite-solutions-creating.md) distribuovat zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="8c1f8-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) to distribute to customers.</span></span>
- <span data-ttu-id="8c1f8-248">Další informace o [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="8c1f8-249">Další informace o [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="8c1f8-250">Další informace o [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="8c1f8-250">Learn more about the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>