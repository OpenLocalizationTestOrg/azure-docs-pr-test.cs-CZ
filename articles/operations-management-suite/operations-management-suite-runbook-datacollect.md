---
title: "aaaCollecting data Log Analytics pomocí sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Podrobný kurz, který provede procesem vytvoření sady runbook v Azure Automation toocollect data do úložiště hello OMS pro analýzu, analýzy protokolů."
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
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="55716-103">Shromáždit data Log Analytics s runbook služby automatizace Azure</span><span class="sxs-lookup"><span data-stu-id="55716-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="55716-104">Můžete shromáždit významné množství dat v analýzy protokolů z různých zdrojů včetně [zdroje dat](../log-analytics/log-analytics-data-sources.md) na agentech a také [data shromážděná z Azure](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="55716-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="55716-105">V případě, kdy potřebujete toocollect data, není přístupná prostřednictvím těchto zdrojů je standardní existují scénáře.</span><span class="sxs-lookup"><span data-stu-id="55716-105">There are a scenarios though where you need toocollect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="55716-106">V těchto případech můžete použít hello [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog analýzy libovolného klienta REST API.</span><span class="sxs-lookup"><span data-stu-id="55716-106">In these cases, you can use hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog Analytics from any REST API client.</span></span>  <span data-ttu-id="55716-107">Běžné tooperform metoda tento shromažďování dat je pomocí sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-107">A common method tooperform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="55716-108">Tento kurz vás provede hello procesu pro vytváření a plánování v sadě runbook v Azure Automation toowrite data tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="55716-108">This tutorial walks through hello process for creating and scheduling a runbook in Azure Automation toowrite data tooLog Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="55716-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="55716-109">Prerequisites</span></span>
<span data-ttu-id="55716-110">Tento scénář vyžaduje hello následující prostředky, které jsou nakonfigurované ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="55716-110">This scenario requires hello following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="55716-111">Jak může být bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="55716-111">Both can be a free account.</span></span>

- <span data-ttu-id="55716-112">[Přihlaste se pracovní prostor analýzy](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="55716-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="55716-113">[Účet Azure automation](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="55716-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="55716-114">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="55716-114">Overview of scenario</span></span>
<span data-ttu-id="55716-115">V tomto kurzu napíšete sadu runbook, která shromažďuje informace o automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="55716-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="55716-116">Runbooky ve službě Azure Automation jsou implementované v prostředí PowerShell, spusťte psaní a testování skript v editoru hello Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in hello Azure Automation editor.</span></span>  <span data-ttu-id="55716-117">Jakmile ověříte, že shromažďujete hello požadované informace, budete zápis tohoto data tooLog analýzy a ověřte vlastní datový typ hello.</span><span class="sxs-lookup"><span data-stu-id="55716-117">Once you verify that you're collecting hello required information, you'll write that data tooLog Analytics and verify hello custom data type.</span></span>  <span data-ttu-id="55716-118">Nakonec runbook hello toostart plán vytvoříte v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="55716-118">Finally, you'll create a schedule toostart hello runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="55716-119">Můžete nakonfigurovat Azure Automation toosend úlohy informace tooLog Analytics bez této sady runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-119">You can configure Azure Automation toosend job information tooLog Analytics without this runbook.</span></span>  <span data-ttu-id="55716-120">Tento scénář je primárně kurzu hello použité toosupport a se doporučuje poslat hello data tooa testovací prostoru.</span><span class="sxs-lookup"><span data-stu-id="55716-120">This scenario is primarily used toosupport hello tutorial, and it's recommended that you send hello data tooa test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="55716-121">1. Instalace modulu rozhraní API sady kolekcí dat</span><span class="sxs-lookup"><span data-stu-id="55716-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="55716-122">Každý [žádost od hello rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md#create-a-request) musí být správně naformátován a obsahovat hlavičku autorizace.</span><span class="sxs-lookup"><span data-stu-id="55716-122">Every [request from hello HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="55716-123">Můžete to provést ve vašem runbooku, ale můžete snížit množství hello pomocí modulu, který zjednodušuje tento proces vyžaduje kód.</span><span class="sxs-lookup"><span data-stu-id="55716-123">You can do this in your runbook, but you can reduce hello amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="55716-124">Jeden modul, který můžete použít se [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI) v hello Galerie prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="55716-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in hello PowerShell Gallery.</span></span>

<span data-ttu-id="55716-125">toouse [modulu](../automation/automation-integration-modules.md) v sadě runbook, musí být nainstalován ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-125">toouse a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="55716-126">Všechny sady runbook v hello pak můžete použít stejný účet hello funkce v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="55716-126">Any runbook in hello same account can then use hello functions in hello module.</span></span>  <span data-ttu-id="55716-127">Můžete nainstalovat nový modul výběrem **prostředky** > **moduly** > **přidat modul** ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="55716-128">Hello Galerie prostředí PowerShell, když vám dává možnost rychlé toodeploy modul přímo tooyour automatizace účtem, takže můžete použít tuto možnost pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="55716-128">hello PowerShell Gallery though gives you a quick option toodeploy a module directly tooyour automation account so you can use that option for this tutorial.</span></span>  

![Modul OMSIngestionAPI](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="55716-130">Přejděte příliš[Galerie prostředí PowerShell](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="55716-130">Go too[PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="55716-131">Vyhledejte **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="55716-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="55716-132">Klikněte na hello **nasazení tooAzure automatizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="55716-132">Click on hello **Deploy tooAzure Automation** button.</span></span>
4. <span data-ttu-id="55716-133">Vyberte svůj účet automation a klikněte na **OK** tooinstall hello modulu.</span><span class="sxs-lookup"><span data-stu-id="55716-133">Select your automation account and click **OK** tooinstall hello module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="55716-134">2. Vytvoření proměnné služeb automatizace</span><span class="sxs-lookup"><span data-stu-id="55716-134">2. Create Automation variables</span></span>
<span data-ttu-id="55716-135">[Proměnné služeb automatizace](..\automation\automation-variables.md) obsahovat hodnoty, které mohou být využívána všem runbookům v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="55716-136">Provádění sady runbook více flexibilní tak, že umožní toochange tyto hodnoty bez úprav hello skutečné runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-136">They make runbooks more flexible by allowing you toochange these values without editing hello actual runbook.</span></span> <span data-ttu-id="55716-137">Každý požadavek hello rozhraní API sady kolekcí dat protokolu HTTP vyžaduje hello ID a klíč pracovního prostoru hello OMS a proměnné prostředky jsou ideální toostore tyto informace.</span><span class="sxs-lookup"><span data-stu-id="55716-137">Every request from hello HTTP Data Collector API requires hello ID and key of hello OMS workspace, and variable assets are ideal toostore this information.</span></span>  

![Proměnné](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="55716-139">V hello portálu Azure přejděte tooyour účet Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-139">In hello Azure portal, navigate tooyour Automation account.</span></span>
2. <span data-ttu-id="55716-140">Vyberte **proměnné** pod **sdílené prostředky**.</span><span class="sxs-lookup"><span data-stu-id="55716-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="55716-141">Klikněte na tlačítko **přidat proměnnou** a vytvořte dvě proměnné hello v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="55716-141">Click **Add a variable** and create hello two variables in hello following table.</span></span>

| <span data-ttu-id="55716-142">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="55716-142">Property</span></span> | <span data-ttu-id="55716-143">Hodnota ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="55716-143">Workspace ID Value</span></span> | <span data-ttu-id="55716-144">Hodnota klíče pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="55716-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="55716-145">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="55716-145">Name</span></span> | <span data-ttu-id="55716-146">ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="55716-146">WorkspaceId</span></span> | <span data-ttu-id="55716-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="55716-147">WorkspaceKey</span></span> |
| <span data-ttu-id="55716-148">Typ</span><span class="sxs-lookup"><span data-stu-id="55716-148">Type</span></span> | <span data-ttu-id="55716-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="55716-149">String</span></span> | <span data-ttu-id="55716-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="55716-150">String</span></span> |
| <span data-ttu-id="55716-151">Hodnota</span><span class="sxs-lookup"><span data-stu-id="55716-151">Value</span></span> | <span data-ttu-id="55716-152">Vložte hello ID pracovního prostoru pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="55716-152">Paste in hello Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="55716-153">Vkládání pomocí hello primární nebo sekundární klíč pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="55716-153">Paste in with hello Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="55716-154">Šifrované</span><span class="sxs-lookup"><span data-stu-id="55716-154">Encrypted</span></span> | <span data-ttu-id="55716-155">Ne</span><span class="sxs-lookup"><span data-stu-id="55716-155">No</span></span> | <span data-ttu-id="55716-156">Ano</span><span class="sxs-lookup"><span data-stu-id="55716-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="55716-157">3. Vytvoření sady runbook</span><span class="sxs-lookup"><span data-stu-id="55716-157">3. Create runbook</span></span>

<span data-ttu-id="55716-158">Automatizace Azure má editoru portálu hello, kde můžete upravit a otestujte svůj runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-158">Azure Automation has an editor in hello portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="55716-159">Máte hello možnost toouse hello skriptu editor toowork s [prostředí PowerShell přímo](../automation/automation-edit-textual-runbook.md) nebo [vytvořit grafický runbook](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="55716-159">You have hello option toouse hello script editor toowork with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="55716-160">V tomto kurzu bude fungovat se skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="55716-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Úprava runbooku](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="55716-162">Přejděte tooyour účet Automation.</span><span class="sxs-lookup"><span data-stu-id="55716-162">Navigate tooyour Automation account.</span></span>  
2. <span data-ttu-id="55716-163">Klikněte na tlačítko **Runbooky** > **přidat runbook** > **vytvořit nový runbook**.</span><span class="sxs-lookup"><span data-stu-id="55716-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="55716-164">Název sady runbook hello, zadejte **shromažďování. automatizace úloh**.</span><span class="sxs-lookup"><span data-stu-id="55716-164">For hello runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="55716-165">Typ runbooku hello, vyberte **prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="55716-165">For hello runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="55716-166">Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a počáteční hello editor.</span><span class="sxs-lookup"><span data-stu-id="55716-166">Click **Create** toocreate hello runbook and start hello editor.</span></span>
5. <span data-ttu-id="55716-167">Zkopírujte a vložte následující kód do runbooku hello hello.</span><span class="sxs-lookup"><span data-stu-id="55716-167">Copy and paste hello following code into hello runbook.</span></span>  <span data-ttu-id="55716-168">Toohello komentáře ve skriptu hello najdete vysvětlení hello kódu.</span><span class="sxs-lookup"><span data-stu-id="55716-168">Refer toohello comments in hello script for explanation of hello code.</span></span>
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="55716-169">4. Služba test runbook</span><span class="sxs-lookup"><span data-stu-id="55716-169">4. Test runbook</span></span>
<span data-ttu-id="55716-170">Služby Azure Automation zahrnuje prostředí příliš[Otestujte svůj runbook](../automation/automation-testing-runbook.md) před publikováním.</span><span class="sxs-lookup"><span data-stu-id="55716-170">Azure Automation includes an environment too[test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="55716-171">Můžete zkontrolovat hello data shromažďovaná společností hello runbook a ověřte, že se zapíše tooLog Analytics podle očekávání před publikováním tooproduction.</span><span class="sxs-lookup"><span data-stu-id="55716-171">You can inspect hello data collected by hello runbook and verify that it writes tooLog Analytics as expected before publishing it tooproduction.</span></span> 
 
![Služba test runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="55716-173">Klikněte na tlačítko **Uložit** toosave hello runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-173">Click **Save** toosave hello runbook.</span></span>
1. <span data-ttu-id="55716-174">Klikněte na tlačítko **testovací podokno** tooopen hello runbook v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="55716-174">Click **Test pane** tooopen hello runbook in hello test environment.</span></span>
3. <span data-ttu-id="55716-175">Vzhledem k tomu, že vaše sada runbook obsahuje parametry, jste výzvami tooenter hodnoty pro ně.</span><span class="sxs-lookup"><span data-stu-id="55716-175">Since your runbook has parameters, you're prompted tooenter values for them.</span></span>  <span data-ttu-id="55716-176">Zadejte název skupiny prostředků hello hello a automatizace hello účet, který vaše informace o úloze probíhající toocollect z.</span><span class="sxs-lookup"><span data-stu-id="55716-176">Enter hello name of hello resource group and hello automation account that your going toocollect job information from.</span></span>
4. <span data-ttu-id="55716-177">Klikněte na tlačítko **spustit** toohello spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="55716-177">Click **Start** toohello start hello runbook.</span></span>
3. <span data-ttu-id="55716-178">Hello runbook se spustí se stavem **zařazeno ve frontě** před probíhá příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="55716-178">hello runbook will start with a status of **Queued** before it goes too**Running**.</span></span>  
3. <span data-ttu-id="55716-179">Hello runbook by měl zobrazit podrobný výstup s úlohami hello shromážděných ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="55716-179">hello runbook should display verbose output with hello jobs collected in json format.</span></span>  <span data-ttu-id="55716-180">Pokud nejsou uvedeny žádné úlohy, pak může byly žádné úlohy vytvořené v účtu automation hello v hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="55716-180">If no jobs are listed, then there may have been no jobs created in hello automation account in hello last hour.</span></span>  <span data-ttu-id="55716-181">Pokuste se spustit žádné sady runbook v účtu automation hello a pak znovu proveďte hello test.</span><span class="sxs-lookup"><span data-stu-id="55716-181">Try starting any runbook in hello automation account and then perform hello test again.</span></span>
4. <span data-ttu-id="55716-182">Ujistěte se, že výstup hello nezobrazí, že všechny chyby hello post příkaz tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="55716-182">Ensure that hello output doesn't show any errors in hello post command tooLog Analytics.</span></span>  <span data-ttu-id="55716-183">Měli byste mít podobné toohello následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="55716-183">You should have a message similar toohello following.</span></span>

    ![Výstup POST](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="55716-185">5. Ověřit záznamy v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="55716-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="55716-186">Po hello runbook byla dokončena v testu, a jste ověřili, že byl úspěšně přijat výstup hello, můžete ověřit, že hello záznamy byly vytvořené pomocí [hledání protokolů v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="55716-186">Once hello runbook has completed in test, and you verified that hello output was successfully received, you can verify that hello records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Protokolování výstupu](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="55716-188">V hello portálu Azure vyberte pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="55716-188">In hello Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="55716-189">Klikněte na **protokolu vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="55716-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="55716-190">Typ hello následující příkaz `Type=AutomationJob_CL` a klikněte na tlačítko Hledat hello.</span><span class="sxs-lookup"><span data-stu-id="55716-190">Type hello following command `Type=AutomationJob_CL` and click hello search button.</span></span> <span data-ttu-id="55716-191">Všimněte si, že typ záznamu hello obsahuje _CL, které není zadané ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="55716-191">Note that hello record type includes _CL that isn't specified in hello script.</span></span>  <span data-ttu-id="55716-192">Že přípona je automaticky připojením toohello protokolu typ tooindicate, že se jedná o typ vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="55716-192">That suffix is automatically appended toohello log type tooindicate that it's a custom log type.</span></span>
4. <span data-ttu-id="55716-193">Měli byste vidět jeden nebo více záznamů vrátil oznamující, že dané sady runbook hello funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="55716-193">You should see one or more records returned indicating that hello runbook is working as expected.</span></span>


## <a name="6-publish-hello-runbook"></a><span data-ttu-id="55716-194">6. Publikovat hello runbook</span><span class="sxs-lookup"><span data-stu-id="55716-194">6. Publish hello runbook</span></span>
<span data-ttu-id="55716-195">Jakmile se ujistíte, že hello runbook správně funguje, je nutné toopublish ho, můžete ho spustit v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="55716-195">Once you've verified that hello runbook is working correctly, you need toopublish it so you can run it in production.</span></span>  <span data-ttu-id="55716-196">Můžete pokračovat tooedit a otestování sady runbook hello beze změny hello publikovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="55716-196">You can continue tooedit and test hello runbook without modifying hello published version.</span></span>  

![Publikování sady runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="55716-198">Vrátí tooyour účet automation.</span><span class="sxs-lookup"><span data-stu-id="55716-198">Return tooyour automation account.</span></span>
2. <span data-ttu-id="55716-199">Klikněte na **Runbooky** a vyberte **shromažďování. automatizace úloh**.</span><span class="sxs-lookup"><span data-stu-id="55716-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="55716-200">Klikněte na tlačítko **upravit** a potom **publikování**.</span><span class="sxs-lookup"><span data-stu-id="55716-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="55716-201">Klikněte na tlačítko **Ano** při kladené tooverify, které chcete toooverwrite hello dřív publikovaná verze.</span><span class="sxs-lookup"><span data-stu-id="55716-201">Click **Yes** when asked tooverify that you want toooverwrite hello previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="55716-202">7. Nastavení možností protokolování</span><span class="sxs-lookup"><span data-stu-id="55716-202">7. Set logging options</span></span> 
<span data-ttu-id="55716-203">Pro test, měla mít tooview [podrobný výstup](../automation/automation-runbook-output-and-messages.md#message-streams) protože nastavit proměnnou hello $VerbosePreference ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="55716-203">For test, you were able tooview [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set hello $VerbosePreference variable in hello script.</span></span>  <span data-ttu-id="55716-204">Pro produkční prostředí je nutné vlastnosti tooset hello protokolování pro sady runbook hello, pokud chcete, aby tooview podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="55716-204">For production, you need tooset hello logging properties for hello runbook if you want tooview verbose output.</span></span>  <span data-ttu-id="55716-205">Pro sadu runbook hello použili v tomto kurzu bude se zobrazovat data json hello odesílány tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="55716-205">For hello runbook used in this tutorial, this will display hello json data being sent tooLog Analytics.</span></span>

![Protokolování a trasování](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="55716-207">Ve vlastnostech hello vaší sady runbook vyberte **protokolování a trasování** pod **nastavení sady Runbook**.</span><span class="sxs-lookup"><span data-stu-id="55716-207">In hello properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="55716-208">Změna nastavení hello pro **protokolování podrobných záznamů** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="55716-208">Change hello setting for **Log verbose records** too**On**.</span></span>
3. <span data-ttu-id="55716-209">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55716-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="55716-210">8. Plánování runbooku</span><span class="sxs-lookup"><span data-stu-id="55716-210">8. Schedule runbook</span></span>
<span data-ttu-id="55716-211">Hello nejběžnější způsob toostart sady runbook, která shromažďuje data monitorování je tooschedule ho toorun automaticky.</span><span class="sxs-lookup"><span data-stu-id="55716-211">hello most common way toostart a runbook that collects monitoring data is tooschedule it toorun automatically.</span></span>  <span data-ttu-id="55716-212">To uděláte tak, že vytvoříte [plán ve službě Azure Automation](../automation/automation-schedules.md) a připojíte ho tooyour runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it tooyour runbook.</span></span>

![Plánování runbooku](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="55716-214">Ve vlastnostech hello vaší sady runbook, vyberte **plány** pod **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="55716-214">In hello properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="55716-215">Klikněte na tlačítko **přidat plán** > **propojit plán tooyour runbook** > **vytvořte nový plán**.</span><span class="sxs-lookup"><span data-stu-id="55716-215">Click **Add a schedule** > **Link a schedule tooyour runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="55716-216">Zadejte následující hodnoty pro hello plán a klikněte na tlačítko hello **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55716-216">Type in hello following values for hello schedule and click **Create**.</span></span>

| <span data-ttu-id="55716-217">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="55716-217">Property</span></span> | <span data-ttu-id="55716-218">Hodnota</span><span class="sxs-lookup"><span data-stu-id="55716-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="55716-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="55716-219">Name</span></span> | <span data-ttu-id="55716-220">AutomationJobs-každou hodinu</span><span class="sxs-lookup"><span data-stu-id="55716-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="55716-221">Spustí</span><span class="sxs-lookup"><span data-stu-id="55716-221">Starts</span></span> | <span data-ttu-id="55716-222">Vyberte libovolný čas posledních 5 minut hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="55716-222">Select any time at least 5 minutes past hello current time.</span></span> |
| <span data-ttu-id="55716-223">Opakování</span><span class="sxs-lookup"><span data-stu-id="55716-223">Recurrence</span></span> | <span data-ttu-id="55716-224">Opakování</span><span class="sxs-lookup"><span data-stu-id="55716-224">Recurring</span></span> |
| <span data-ttu-id="55716-225">Opakovat každých</span><span class="sxs-lookup"><span data-stu-id="55716-225">Recur every</span></span> | <span data-ttu-id="55716-226">1 hodina</span><span class="sxs-lookup"><span data-stu-id="55716-226">1 hour</span></span> |
| <span data-ttu-id="55716-227">Sada vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="55716-227">Set expiration</span></span> | <span data-ttu-id="55716-228">Ne</span><span class="sxs-lookup"><span data-stu-id="55716-228">No</span></span> |

<span data-ttu-id="55716-229">Po vytvoření plánu hello musíte tooset hello parametr hodnoty, které se použije vždy, když tento plán spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="55716-229">Once hello schedule is created, you need tooset hello parameter values that will be used each time this schedule starts hello runbook.</span></span>

6. <span data-ttu-id="55716-230">Klikněte na tlačítko **nakonfigurovat parametry a nastavení spouštění**.</span><span class="sxs-lookup"><span data-stu-id="55716-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="55716-231">Zadejte hodnoty pro vaše **ResourceGroupName** a **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="55716-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="55716-232">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="55716-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="55716-233">9. Ověřte, sada runbook spustí podle plánu.</span><span class="sxs-lookup"><span data-stu-id="55716-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="55716-234">Při spuštění sady runbook [se vytvoří úloha](../automation/automation-runbook-execution.md) a jakéhokoli výstupu protokolována.</span><span class="sxs-lookup"><span data-stu-id="55716-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="55716-235">Ve skutečnosti jedná se o hello je shromažďování stejných úloh, které hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="55716-235">In fact, these are hello same jobs that hello runbook is collecting.</span></span>  <span data-ttu-id="55716-236">Můžete ověřit, že dané sady runbook hello spustí podle očekávání kontrolou hello úlohy pro hello runbook po uplynutí hello čas zahájení pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="55716-236">You can verify that hello runbook starts as expected by checking hello jobs for hello runbook after hello start time for hello schedule has passed.</span></span>

![Úlohy](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="55716-238">Ve vlastnostech hello vaší sady runbook, vyberte **úlohy** pod **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="55716-238">In hello properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="55716-239">Měli byste vidět, že byla spuštěna v seznamu úloh pro každou sadu runbook hello čas.</span><span class="sxs-lookup"><span data-stu-id="55716-239">You should see a listing of jobs for each time hello runbook was started.</span></span>
3. <span data-ttu-id="55716-240">Klikněte na jednu z úloh tooview hello její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="55716-240">Click on one of hello jobs tooview its details.</span></span>
4. <span data-ttu-id="55716-241">Klikněte na **všechny protokoly** tooview hello protokoly a výstup z runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="55716-241">Click on **All logs** tooview hello logs and output from hello runbook.</span></span>
5. <span data-ttu-id="55716-242">Posuňte se toohello dolní toofind položky podobné toohello bitovou kopii níže.</span><span class="sxs-lookup"><span data-stu-id="55716-242">Scroll toohello bottom toofind an entry similar toohello image below.</span></span><br>![Verbose](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="55716-244">Kliknutím na tuto položku tooview hello podrobná data json, který vám byl zaslán tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="55716-244">Click on this entry tooview hello detailed json data  that was sent tooLog Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="55716-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55716-245">Next steps</span></span>
- <span data-ttu-id="55716-246">Použití [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) toocreate zobrazení zobrazení hello dat, že jste shromážděna toohello úložiště analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="55716-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) toocreate a view displaying hello data that you've collected toohello Log Analytics repository.</span></span>
- <span data-ttu-id="55716-247">Balíček svoji sadu runbook v [řešení pro správu](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span><span class="sxs-lookup"><span data-stu-id="55716-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span></span>
- <span data-ttu-id="55716-248">Další informace o [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="55716-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="55716-249">Další informace o [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="55716-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="55716-250">Další informace o hello [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="55716-250">Learn more about hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>
