---
title: "Předávání dat úlohu Azure Automation k analýze protokolů OMS | Microsoft Docs"
description: "Tento článek ukazuje, jak odesílat stav úlohy a runbook proudy úlohy Microsoft Operations Management Suite Log Analytics k poskytování další aspekty a správu."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/2017
ms.author: magoedte
ms.openlocfilehash: 2c0ca7fc332963e5a5db3c20c400ed877ae0cc54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a><span data-ttu-id="766d6-103">Předávání zpráv o stavu úlohy a datové proudy úlohy z Automatizace analýzy protokolů (OMS)</span><span class="sxs-lookup"><span data-stu-id="766d6-103">Forward job status and job streams from Automation to Log Analytics (OMS)</span></span>
<span data-ttu-id="766d6-104">Automatizace můžete odeslat runbook datové proudy úlohy stavu a úlohu do pracovního prostoru analýzy protokolů Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="766d6-104">Automation can send runbook job status and job streams to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>  <span data-ttu-id="766d6-105">Protokoly úlohy a datové proudy úlohy jsou viditelné na portálu Azure nebo v prostředí PowerShell pro jednotlivé úlohy a to umožňuje provádět jednoduché šetření.</span><span class="sxs-lookup"><span data-stu-id="766d6-105">Job logs and job streams are visible in the Azure portal, or with PowerShell, for individual jobs and this allows you to perform simple investigations.</span></span> <span data-ttu-id="766d6-106">Pomocí analýzy protokolů můžete nyní:</span><span class="sxs-lookup"><span data-stu-id="766d6-106">Now with Log Analytics you can:</span></span>

* <span data-ttu-id="766d6-107">Pohled na vaše úlohy automatizace</span><span class="sxs-lookup"><span data-stu-id="766d6-107">Get insight on your Automation jobs</span></span>
* <span data-ttu-id="766d6-108">Aktivační událost e-mailem nebo výstrahy podle runbook stav úlohy (například chybných nebo pozastavených)</span><span class="sxs-lookup"><span data-stu-id="766d6-108">Trigger an email or alert based on your runbook job status (for example, failed or suspended)</span></span>
* <span data-ttu-id="766d6-109">Zápis pokročilými dotazy napříč vaše datové proudy úlohy</span><span class="sxs-lookup"><span data-stu-id="766d6-109">Write advanced queries across your job streams</span></span>
* <span data-ttu-id="766d6-110">Vazbu mezi úlohy v účtech Automation</span><span class="sxs-lookup"><span data-stu-id="766d6-110">Correlate jobs across Automation accounts</span></span>
* <span data-ttu-id="766d6-111">Vizualizace historii úlohy v čase</span><span class="sxs-lookup"><span data-stu-id="766d6-111">Visualize your job history over time</span></span>     

## <a name="prerequisites-and-deployment-considerations"></a><span data-ttu-id="766d6-112">Požadavky a důležité informace o nasazení</span><span class="sxs-lookup"><span data-stu-id="766d6-112">Prerequisites and deployment considerations</span></span>
<span data-ttu-id="766d6-113">Chcete-li zahájit odesílání protokolů služby Automation k analýze protokolů, je třeba:</span><span class="sxs-lookup"><span data-stu-id="766d6-113">To start sending your Automation logs to Log Analytics, you need:</span></span>

1. <span data-ttu-id="766d6-114">Listopadu 2016 nebo novější vydání [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span><span class="sxs-lookup"><span data-stu-id="766d6-114">The November 2016 or later release of [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span></span>
2. <span data-ttu-id="766d6-115">Pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="766d6-115">A Log Analytics workspace.</span></span> <span data-ttu-id="766d6-116">Další informace najdete v tématu [začít pracovat s analýzy protokolů](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="766d6-116">For more information, see [Get started with Log Analytics](../log-analytics/log-analytics-get-started.md).</span></span> 
3. <span data-ttu-id="766d6-117">ID prostředku pro váš účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-117">The ResourceId for your Azure Automation account</span></span>

<span data-ttu-id="766d6-118">Najít ResourceId pro váš účet Azure Automation a pracovní prostor analýzy protokolů, spusťte následující prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="766d6-118">To find the ResourceId for your Azure Automation account and Log Analytics workspace, run the following PowerShell:</span></span>

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

<span data-ttu-id="766d6-119">Pokud máte více účtů Automation, nebo pracovní prostory v výstup z předchozích příkazů *název* budete muset nakonfigurovat a zkopírujte hodnotu pro *ResourceId*.</span><span class="sxs-lookup"><span data-stu-id="766d6-119">If you have multiple Automation accounts, or workspaces, in the output of the preceding commands, find the *Name* you need to configure and copy the value for *ResourceId*.</span></span>

<span data-ttu-id="766d6-120">Pokud potřebujete najít *název* účtu Automation na portálu Azure vyberte svůj účet Automation z **účet Automation** a vyberte **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="766d6-120">If you need to find the *Name* of your Automation account, in the Azure portal select your Automation account from the **Automation account** blade and select **All settings**.</span></span>  <span data-ttu-id="766d6-121">V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="766d6-121">From the **All settings** blade, under **Account Settings** select **Properties**.</span></span>  <span data-ttu-id="766d6-122">V okně **Vlastnosti** si můžete tyto hodnoty opsat.</span><span class="sxs-lookup"><span data-stu-id="766d6-122">In the **Properties** blade, you can note these values.</span></span><br> <span data-ttu-id="766d6-123">![Vlastnosti účtu Automation](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span><span class="sxs-lookup"><span data-stu-id="766d6-123">![Automation Account properties](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span></span>

## <a name="set-up-integration-with-log-analytics"></a><span data-ttu-id="766d6-124">Nastavení integrace s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="766d6-124">Set up integration with Log Analytics</span></span>
1. <span data-ttu-id="766d6-125">V počítači, spusťte **prostředí Windows PowerShell** z **spustit** obrazovky.</span><span class="sxs-lookup"><span data-stu-id="766d6-125">On your computer, start **Windows PowerShell** from the **Start** screen.</span></span>  
2. <span data-ttu-id="766d6-126">Zkopírujte a vložte následující prostředí PowerShell a upravit její hodnotu `$workspaceId` a `$automationAccountId`.</span><span class="sxs-lookup"><span data-stu-id="766d6-126">Copy and paste the following PowerShell, and edit the value for the `$workspaceId` and `$automationAccountId`.</span></span>  <span data-ttu-id="766d6-127">Pro `-Environment` parametr platné hodnoty jsou *AzureCloud* nebo *AzureUSGovernment* v závislosti na práci v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="766d6-127">For the `-Environment` parameter, valid values are *AzureCloud* or *AzureUSGovernment* depending on the cloud environment you are working in.</span></span>     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

<span data-ttu-id="766d6-128">Po spuštění tohoto skriptu, zobrazí se záznamy v analýzy protokolů během deseti minut nové JobLogs nebo JobStreams zapisovaný.</span><span class="sxs-lookup"><span data-stu-id="766d6-128">After running this script, you will see records in Log Analytics within 10 minutes of new JobLogs or JobStreams being written.</span></span>

<span data-ttu-id="766d6-129">Pokud chcete zobrazit protokoly, spusťte následující dotaz ve vyhledávání protokolu analýzy protokolů:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="766d6-129">To see the logs, run the following query in Log Analytics log search: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="verify-configuration"></a><span data-ttu-id="766d6-130">Ověření konfigurace</span><span class="sxs-lookup"><span data-stu-id="766d6-130">Verify configuration</span></span>
<span data-ttu-id="766d6-131">Potvrďte, že váš účet Automation odesílá protokoly do pracovního prostoru analýzy protokolů, zkontrolujte, jestli jsou správně nastavené diagnostiky na účtu Automation pomocí prostředí PowerShell následující:</span><span class="sxs-lookup"><span data-stu-id="766d6-131">To confirm that your Automation account is sending logs to your Log Analytics workspace, check that diagnostics are set correctly on the Automation account using the following PowerShell:</span></span>

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

<span data-ttu-id="766d6-132">Ve výstupu zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="766d6-132">In the output ensure that:</span></span>
+ <span data-ttu-id="766d6-133">V části *protokoly*, hodnota *povoleno* je *True*</span><span class="sxs-lookup"><span data-stu-id="766d6-133">Under *Logs*, the value for *Enabled* is *True*</span></span>
+ <span data-ttu-id="766d6-134">Hodnota *WorkspaceId* je nastaven na ResourceId pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="766d6-134">The value of *WorkspaceId* is set to the ResourceId of your Log Analytics workspace</span></span>


## <a name="log-analytics-records"></a><span data-ttu-id="766d6-135">Záznamy služby Log Analytics</span><span class="sxs-lookup"><span data-stu-id="766d6-135">Log Analytics records</span></span>
<span data-ttu-id="766d6-136">Diagnostika z Azure Automation vytvoří dva typy záznamů v analýzy protokolů a jsou označené jako **typ = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="766d6-136">Diagnostics from Azure Automation creates two types of records in Log Analytics and are tagged as **Type=AzureDiagnostics**.</span></span>

### <a name="job-logs"></a><span data-ttu-id="766d6-137">V protokolech úloh</span><span class="sxs-lookup"><span data-stu-id="766d6-137">Job Logs</span></span>
| <span data-ttu-id="766d6-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="766d6-138">Property</span></span> | <span data-ttu-id="766d6-139">Popis</span><span class="sxs-lookup"><span data-stu-id="766d6-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="766d6-140">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="766d6-140">TimeGenerated</span></span> |<span data-ttu-id="766d6-141">Datum a čas provedení úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-141">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="766d6-142">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="766d6-142">RunbookName_s</span></span> |<span data-ttu-id="766d6-143">Název runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-143">The name of the runbook.</span></span> |
| <span data-ttu-id="766d6-144">Caller_s</span><span class="sxs-lookup"><span data-stu-id="766d6-144">Caller_s</span></span> |<span data-ttu-id="766d6-145">Kdo operaci zahájil.</span><span class="sxs-lookup"><span data-stu-id="766d6-145">Who initiated the operation.</span></span>  <span data-ttu-id="766d6-146">Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.</span><span class="sxs-lookup"><span data-stu-id="766d6-146">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="766d6-147">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="766d6-147">Tenant_g</span></span> | <span data-ttu-id="766d6-148">Identifikátor GUID, který identifikuje klienta pro volajícího.</span><span class="sxs-lookup"><span data-stu-id="766d6-148">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="766d6-149">JobId_g</span><span class="sxs-lookup"><span data-stu-id="766d6-149">JobId_g</span></span> |<span data-ttu-id="766d6-150">Identifikátor GUID, který představuje ID úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-150">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="766d6-151">ResultType</span><span class="sxs-lookup"><span data-stu-id="766d6-151">ResultType</span></span> |<span data-ttu-id="766d6-152">Stav úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-152">The status of the runbook job.</span></span>  <span data-ttu-id="766d6-153">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="766d6-153">Possible values are:</span></span><br><span data-ttu-id="766d6-154">- Spuštěno</span><span class="sxs-lookup"><span data-stu-id="766d6-154">- Started</span></span><br><span data-ttu-id="766d6-155">- Zastaveno</span><span class="sxs-lookup"><span data-stu-id="766d6-155">- Stopped</span></span><br><span data-ttu-id="766d6-156">- Pozastaveno</span><span class="sxs-lookup"><span data-stu-id="766d6-156">- Suspended</span></span><br><span data-ttu-id="766d6-157">- Neúspěch</span><span class="sxs-lookup"><span data-stu-id="766d6-157">- Failed</span></span><br><span data-ttu-id="766d6-158">-Byla dokončena</span><span class="sxs-lookup"><span data-stu-id="766d6-158">- Completed</span></span> |
| <span data-ttu-id="766d6-159">Kategorie</span><span class="sxs-lookup"><span data-stu-id="766d6-159">Category</span></span> | <span data-ttu-id="766d6-160">Klasifikace typu dat.</span><span class="sxs-lookup"><span data-stu-id="766d6-160">Classification of the type of data.</span></span>  <span data-ttu-id="766d6-161">Službě Automation odpovídá hodnota JobLogs.</span><span class="sxs-lookup"><span data-stu-id="766d6-161">For Automation, the value is JobLogs.</span></span> |
| <span data-ttu-id="766d6-162">OperationName</span><span class="sxs-lookup"><span data-stu-id="766d6-162">OperationName</span></span> | <span data-ttu-id="766d6-163">Určuje typ operace prováděné v Azure.</span><span class="sxs-lookup"><span data-stu-id="766d6-163">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="766d6-164">Hodnota pro automatizaci je úloha.</span><span class="sxs-lookup"><span data-stu-id="766d6-164">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="766d6-165">Prostředek</span><span class="sxs-lookup"><span data-stu-id="766d6-165">Resource</span></span> | <span data-ttu-id="766d6-166">Název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-166">Name of the Automation account</span></span> |
| <span data-ttu-id="766d6-167">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="766d6-167">SourceSystem</span></span> | <span data-ttu-id="766d6-168">Jak analýzy protokolů shromáždit data.</span><span class="sxs-lookup"><span data-stu-id="766d6-168">How Log Analytics collected the data.</span></span> <span data-ttu-id="766d6-169">Vždy *Azure* Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="766d6-169">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="766d6-170">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="766d6-170">ResultDescription</span></span> |<span data-ttu-id="766d6-171">Popisuje výsledný stav úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-171">Describes the runbook job result state.</span></span>  <span data-ttu-id="766d6-172">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="766d6-172">Possible values are:</span></span><br><span data-ttu-id="766d6-173">- Úloha se spustila</span><span class="sxs-lookup"><span data-stu-id="766d6-173">- Job is started</span></span><br><span data-ttu-id="766d6-174">- Zpracování úlohy se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="766d6-174">- Job Failed</span></span><br><span data-ttu-id="766d6-175">- Úloha je dokončená</span><span class="sxs-lookup"><span data-stu-id="766d6-175">- Job Completed</span></span> |
| <span data-ttu-id="766d6-176">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="766d6-176">CorrelationId</span></span> |<span data-ttu-id="766d6-177">Identifikátor GUID, který představuje ID korelace úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-177">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="766d6-178">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="766d6-178">ResourceId</span></span> |<span data-ttu-id="766d6-179">Určuje id prostředku účet Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="766d6-179">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="766d6-180">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="766d6-180">SubscriptionId</span></span> | <span data-ttu-id="766d6-181">Předplatné Azure Id (GUID) pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-181">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="766d6-182">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="766d6-182">ResourceGroup</span></span> | <span data-ttu-id="766d6-183">Název skupiny prostředků pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-183">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="766d6-184">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="766d6-184">ResourceProvider</span></span> | <span data-ttu-id="766d6-185">SPOLEČNOSTI MICROSOFT. AUTOMATIZACE</span><span class="sxs-lookup"><span data-stu-id="766d6-185">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="766d6-186">ResourceType</span><span class="sxs-lookup"><span data-stu-id="766d6-186">ResourceType</span></span> | <span data-ttu-id="766d6-187">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="766d6-187">AUTOMATIONACCOUNTS</span></span> |


### <a name="job-streams"></a><span data-ttu-id="766d6-188">Datové proudy úlohy</span><span class="sxs-lookup"><span data-stu-id="766d6-188">Job Streams</span></span>
| <span data-ttu-id="766d6-189">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="766d6-189">Property</span></span> | <span data-ttu-id="766d6-190">Popis</span><span class="sxs-lookup"><span data-stu-id="766d6-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="766d6-191">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="766d6-191">TimeGenerated</span></span> |<span data-ttu-id="766d6-192">Datum a čas provedení úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-192">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="766d6-193">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="766d6-193">RunbookName_s</span></span> |<span data-ttu-id="766d6-194">Název runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-194">The name of the runbook.</span></span> |
| <span data-ttu-id="766d6-195">Caller_s</span><span class="sxs-lookup"><span data-stu-id="766d6-195">Caller_s</span></span> |<span data-ttu-id="766d6-196">Kdo operaci zahájil.</span><span class="sxs-lookup"><span data-stu-id="766d6-196">Who initiated the operation.</span></span>  <span data-ttu-id="766d6-197">Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.</span><span class="sxs-lookup"><span data-stu-id="766d6-197">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="766d6-198">StreamType_s</span><span class="sxs-lookup"><span data-stu-id="766d6-198">StreamType_s</span></span> |<span data-ttu-id="766d6-199">Typ datového proudu úlohy.</span><span class="sxs-lookup"><span data-stu-id="766d6-199">The type of job stream.</span></span> <span data-ttu-id="766d6-200">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="766d6-200">Possible values are:</span></span><br><span data-ttu-id="766d6-201">- Průběh</span><span class="sxs-lookup"><span data-stu-id="766d6-201">-Progress</span></span><br><span data-ttu-id="766d6-202">- Výstup</span><span class="sxs-lookup"><span data-stu-id="766d6-202">- Output</span></span><br><span data-ttu-id="766d6-203">- Varování</span><span class="sxs-lookup"><span data-stu-id="766d6-203">- Warning</span></span><br><span data-ttu-id="766d6-204">- Chyba</span><span class="sxs-lookup"><span data-stu-id="766d6-204">- Error</span></span><br><span data-ttu-id="766d6-205">- Ladění</span><span class="sxs-lookup"><span data-stu-id="766d6-205">- Debug</span></span><br><span data-ttu-id="766d6-206">- Podrobné</span><span class="sxs-lookup"><span data-stu-id="766d6-206">- Verbose</span></span> |
| <span data-ttu-id="766d6-207">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="766d6-207">Tenant_g</span></span> | <span data-ttu-id="766d6-208">Identifikátor GUID, který identifikuje klienta pro volajícího.</span><span class="sxs-lookup"><span data-stu-id="766d6-208">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="766d6-209">JobId_g</span><span class="sxs-lookup"><span data-stu-id="766d6-209">JobId_g</span></span> |<span data-ttu-id="766d6-210">Identifikátor GUID, který představuje ID úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-210">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="766d6-211">ResultType</span><span class="sxs-lookup"><span data-stu-id="766d6-211">ResultType</span></span> |<span data-ttu-id="766d6-212">Stav úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-212">The status of the runbook job.</span></span>  <span data-ttu-id="766d6-213">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="766d6-213">Possible values are:</span></span><br><span data-ttu-id="766d6-214">– V průběhu</span><span class="sxs-lookup"><span data-stu-id="766d6-214">- In Progress</span></span> |
| <span data-ttu-id="766d6-215">Kategorie</span><span class="sxs-lookup"><span data-stu-id="766d6-215">Category</span></span> | <span data-ttu-id="766d6-216">Klasifikace typu dat.</span><span class="sxs-lookup"><span data-stu-id="766d6-216">Classification of the type of data.</span></span>  <span data-ttu-id="766d6-217">Službě Automation odpovídá hodnota JobStreams.</span><span class="sxs-lookup"><span data-stu-id="766d6-217">For Automation, the value is JobStreams.</span></span> |
| <span data-ttu-id="766d6-218">OperationName</span><span class="sxs-lookup"><span data-stu-id="766d6-218">OperationName</span></span> | <span data-ttu-id="766d6-219">Určuje typ operace prováděné v Azure.</span><span class="sxs-lookup"><span data-stu-id="766d6-219">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="766d6-220">Hodnota pro automatizaci je úloha.</span><span class="sxs-lookup"><span data-stu-id="766d6-220">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="766d6-221">Prostředek</span><span class="sxs-lookup"><span data-stu-id="766d6-221">Resource</span></span> | <span data-ttu-id="766d6-222">Název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-222">Name of the Automation account</span></span> |
| <span data-ttu-id="766d6-223">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="766d6-223">SourceSystem</span></span> | <span data-ttu-id="766d6-224">Jak analýzy protokolů shromáždit data.</span><span class="sxs-lookup"><span data-stu-id="766d6-224">How Log Analytics collected the data.</span></span> <span data-ttu-id="766d6-225">Vždy *Azure* Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="766d6-225">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="766d6-226">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="766d6-226">ResultDescription</span></span> |<span data-ttu-id="766d6-227">Zahrnuje výstupní datový proud z runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-227">Includes the output stream from the runbook.</span></span> |
| <span data-ttu-id="766d6-228">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="766d6-228">CorrelationId</span></span> |<span data-ttu-id="766d6-229">Identifikátor GUID, který představuje ID korelace úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-229">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="766d6-230">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="766d6-230">ResourceId</span></span> |<span data-ttu-id="766d6-231">Určuje id prostředku účet Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="766d6-231">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="766d6-232">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="766d6-232">SubscriptionId</span></span> | <span data-ttu-id="766d6-233">Předplatné Azure Id (GUID) pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-233">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="766d6-234">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="766d6-234">ResourceGroup</span></span> | <span data-ttu-id="766d6-235">Název skupiny prostředků pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-235">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="766d6-236">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="766d6-236">ResourceProvider</span></span> | <span data-ttu-id="766d6-237">SPOLEČNOSTI MICROSOFT. AUTOMATIZACE</span><span class="sxs-lookup"><span data-stu-id="766d6-237">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="766d6-238">ResourceType</span><span class="sxs-lookup"><span data-stu-id="766d6-238">ResourceType</span></span> | <span data-ttu-id="766d6-239">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="766d6-239">AUTOMATIONACCOUNTS</span></span> |

## <a name="viewing-automation-logs-in-log-analytics"></a><span data-ttu-id="766d6-240">Zobrazení automatizace přihlásí analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="766d6-240">Viewing Automation Logs in Log Analytics</span></span>
<span data-ttu-id="766d6-241">Teď, když jste spustili odesílání protokolů úlohy služby Automation k analýze protokolů, podíváme se, co můžete dělat s tyto protokoly uvnitř analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="766d6-241">Now that you have started sending your Automation job logs to Log Analytics, let’s see what you can do with these logs inside Log Analytics.</span></span>

<span data-ttu-id="766d6-242">Pokud chcete zobrazit protokoly, spusťte následující dotaz:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="766d6-242">To see the logs, run the following query: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a><span data-ttu-id="766d6-243">Odeslat e-mail, dojde k selhání úlohy runbooku nebo pozastaví</span><span class="sxs-lookup"><span data-stu-id="766d6-243">Send an email when a runbook job fails or suspends</span></span>
<span data-ttu-id="766d6-244">Jeden z našich zákazníků nejvyšší požádá, je pro možnost odesílat e-mailem nebo jako text v případě problémů s úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="766d6-244">One of our top customer asks is for the ability to send an email or a text when something goes wrong with a runbook job.</span></span>   

<span data-ttu-id="766d6-245">Pokud chcete vytvořit pravidlo výstrahy, začněte vytvořením hledání protokolů pro záznamy úlohy sady runbook, které by měla vyvolat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="766d6-245">To create an alert rule, you start by creating a log search for the runbook job records that should invoke the alert.</span></span>  <span data-ttu-id="766d6-246">Klikněte **výstraha** tlačítko Vytvořit a nakonfigurovat pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="766d6-246">Click the **Alert** button to create and configure the alert rule.</span></span>

1. <span data-ttu-id="766d6-247">Na stránce Přehled protokolu Analytics klikněte na tlačítko **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="766d6-247">From the Log Analytics Overview page, click **Log Search**.</span></span>
2. <span data-ttu-id="766d6-248">Vytvoření vyhledávací dotaz protokolu pro upozornění zadáním následujících hledání do pole dotazu: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` můžete taky Seskupit podle RunbookName pomocí:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span><span class="sxs-lookup"><span data-stu-id="766d6-248">Create a log search query for your alert by typing the following search into the query field:  `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)`  You can also group by the RunbookName by using: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span></span>   

   <span data-ttu-id="766d6-249">Pokud jste nastavili protokolů z více než jeden účet Automation nebo odběr do pracovního prostoru, můžete je seskupovat vaše předplatné a účet Automation.</span><span class="sxs-lookup"><span data-stu-id="766d6-249">If you have set up logs from more than one Automation account or subscription to your workspace, you can group your alerts by subscription and Automation account.</span></span>  <span data-ttu-id="766d6-250">Název účtu Automation může být odvozen z pole prostředků do vyhledávání JobLogs.</span><span class="sxs-lookup"><span data-stu-id="766d6-250">Automation account name can be derived from the Resource field in the search of JobLogs.</span></span>  
3. <span data-ttu-id="766d6-251">Chcete-li otevřít **přidat pravidlo výstrahy** obrazovky, klikněte na tlačítko **výstrah** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="766d6-251">To open the **Add Alert Rule** screen, click **Alert** at the top of the page.</span></span> <span data-ttu-id="766d6-252">Další informace o možnostech konfigurace upozornění najdete v tématu [výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md#alert-rules).</span><span class="sxs-lookup"><span data-stu-id="766d6-252">For further details on the options to configure the alert, see [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules).</span></span>

### <a name="find-all-jobs-that-have-completed-with-errors"></a><span data-ttu-id="766d6-253">Najít všechny úlohy, které byly dokončeny s chybami</span><span class="sxs-lookup"><span data-stu-id="766d6-253">Find all jobs that have completed with errors</span></span>
<span data-ttu-id="766d6-254">Kromě zobrazení výstrah o selhání, můžete najít při úlohy runbooku se neukončující chybu.</span><span class="sxs-lookup"><span data-stu-id="766d6-254">In addition to alerting on failures, you can find when a runbook job has a non-terminating error.</span></span> <span data-ttu-id="766d6-255">V těchto případech prostředí PowerShell vytvoří chybový proud, ale chyby neukončující nezpůsobí úlohu pozastavit nebo selže.</span><span class="sxs-lookup"><span data-stu-id="766d6-255">In these cases PowerShell produces an error stream, but the non-terminating errors do not cause your job to suspend or fail.</span></span>    

1. <span data-ttu-id="766d6-256">V pracovním prostoru analýzy protokolů, klikněte na tlačítko **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="766d6-256">In your Log Analytics workspace, click **Log Search**.</span></span>
2. <span data-ttu-id="766d6-257">V poli dotazu zadejte `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` a pak klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="766d6-257">In the query field, type `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` and then click **Search**.</span></span>

### <a name="view-job-streams-for-a-job"></a><span data-ttu-id="766d6-258">Zobrazení datové proudy úlohy pro úlohu</span><span class="sxs-lookup"><span data-stu-id="766d6-258">View job streams for a job</span></span>
<span data-ttu-id="766d6-259">Když ladíte úlohu, můžete také viděl datové proudy úlohy.</span><span class="sxs-lookup"><span data-stu-id="766d6-259">When you are debugging a job, you may also want to look into the job streams.</span></span>  <span data-ttu-id="766d6-260">Následující dotaz zobrazí všechny datové proudy pro jednu úlohu s identifikátorem GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:</span><span class="sxs-lookup"><span data-stu-id="766d6-260">The following query shows all the streams for a single job with GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0:</span></span>   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a><span data-ttu-id="766d6-261">Zobrazit stav historie úlohy</span><span class="sxs-lookup"><span data-stu-id="766d6-261">View historical job status</span></span>
<span data-ttu-id="766d6-262">Nakonec můžete vizualizovat historii úlohy v čase.</span><span class="sxs-lookup"><span data-stu-id="766d6-262">Finally, you may want to visualize your job history over time.</span></span>  <span data-ttu-id="766d6-263">Tento dotaz můžete použít k vyhledání stav úloh v čase.</span><span class="sxs-lookup"><span data-stu-id="766d6-263">You can use this query to search for the status of your jobs over time.</span></span>

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> <span data-ttu-id="766d6-264">![OMS historie úlohy stavu grafu](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span><span class="sxs-lookup"><span data-stu-id="766d6-264">![OMS Historical Job Status Chart](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span></span><br>

## <a name="summary"></a><span data-ttu-id="766d6-265">Souhrn</span><span class="sxs-lookup"><span data-stu-id="766d6-265">Summary</span></span>
<span data-ttu-id="766d6-266">Odeslání dat datového proudu a stav úlohy automatizace k analýze protokolů, lze získat lepší přehled o stavu vaší automatizace úloh podle:</span><span class="sxs-lookup"><span data-stu-id="766d6-266">By sending your Automation job status and stream data to Log Analytics, you can get better insight into the status of your Automation jobs by:</span></span>
+ <span data-ttu-id="766d6-267">Nastavení výstrah upozornění v případě, že se vyskytl problém</span><span class="sxs-lookup"><span data-stu-id="766d6-267">Setting up alerts to notify you when there is an issue</span></span>
+ <span data-ttu-id="766d6-268">Pomocí vlastních zobrazení a vyhledávací dotazy k vizualizaci výsledky sady runbook, stav úlohy sady runbook a další související klíčové ukazatele nebo metriky.</span><span class="sxs-lookup"><span data-stu-id="766d6-268">Using custom views and search queries to visualize your runbook results, runbook job status, and other related key indicators or metrics.</span></span>  

<span data-ttu-id="766d6-269">Log Analytics poskytuje větší provozní viditelnost do automatizace úloh a může pomoct adresu incidenty rychlejší.</span><span class="sxs-lookup"><span data-stu-id="766d6-269">Log Analytics provides greater operational visibility to your Automation jobs and can help address incidents quicker.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="766d6-270">Další kroky</span><span class="sxs-lookup"><span data-stu-id="766d6-270">Next steps</span></span>
* <span data-ttu-id="766d6-271">Další informace o tom, jak vytvářet různé vyhledávací dotazy a kontrolovat protokoly úloh služby Automation s použitím služby Log Analytics, najdete v článku [Vyhledávání protokolů v Log Analytics](../log-analytics/log-analytics-log-searches.md)</span><span class="sxs-lookup"><span data-stu-id="766d6-271">To learn more about how to construct different search queries and review the Automation job logs with Log Analytics, see [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md)</span></span>
* <span data-ttu-id="766d6-272">Chcete-li pochopit, jak vytvořit a ze sady runbook načíst výstupní a chybové zprávy, přečtěte si téma [Runbook výstup a zprávy](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="766d6-272">To understand how to create and retrieve output and error messages from runbooks, see [Runbook output and messages](automation-runbook-output-and-messages.md)</span></span>
* <span data-ttu-id="766d6-273">Další informace o spouštění runbooků, postupy při monitorování úloh runbooků a další technické podrobnosti najdete v článku [Sledování úlohy runbooku](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="766d6-273">To learn more about runbook execution, how to monitor runbook jobs, and other technical details, see [Track a runbook job](automation-runbook-execution.md)</span></span>
* <span data-ttu-id="766d6-274">Další informace o službě Log Analytics v OMS a o zdrojích pro shromažďování dat najdete v článku [Přehled shromažďování dat úložiště Azure ve službě Log Analytics](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="766d6-274">To learn more about OMS Log Analytics and data collection sources, see [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md)</span></span>
