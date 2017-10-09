---
title: "tooCreate aaaUse prostředí PowerShell a nakonfigurovat pracovní prostor Log Analytics | Microsoft Docs"
description: "Protokolovat Analytics používá data ze serverů v místní nebo cloudové infrastruktury. Můžete shromáždit data počítače z úložiště Azure generování Azure diagnostics."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: a6d66194204cc58de6aafb687a19fe9611e0c58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="e4e26-104">Správa služby Log Analytics pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="e4e26-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="e4e26-105">Můžete použít hello [rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform různé funkce v analýzy protokolů z příkazového řádku nebo v rámci skriptu.</span><span class="sxs-lookup"><span data-stu-id="e4e26-105">You can use hello [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="e4e26-106">Příklady hello úlohy, které můžete provádět pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e4e26-106">Examples of hello tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="e4e26-107">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="e4e26-107">Create a workspace</span></span>
* <span data-ttu-id="e4e26-108">Přidat nebo odebrat řešení</span><span class="sxs-lookup"><span data-stu-id="e4e26-108">Add or remove a solution</span></span>
* <span data-ttu-id="e4e26-109">Import a export uložených hledání</span><span class="sxs-lookup"><span data-stu-id="e4e26-109">Import and export saved searches</span></span>
* <span data-ttu-id="e4e26-110">Vytvořit skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="e4e26-110">Create a computer group</span></span>
* <span data-ttu-id="e4e26-111">Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem Windows hello</span><span class="sxs-lookup"><span data-stu-id="e4e26-111">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
* <span data-ttu-id="e4e26-112">Shromáždit čítače výkonu z počítačů se systémy Linux a Windows</span><span class="sxs-lookup"><span data-stu-id="e4e26-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="e4e26-113">Shromažďování událostí z syslog počítačů se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="e4e26-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="e4e26-114">Shromažďování událostí z protokolů událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="e4e26-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="e4e26-115">Shromažďovat vlastní protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="e4e26-115">Collect custom event logs</span></span>
* <span data-ttu-id="e4e26-116">Přidat hello log analytics agenta tooan virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="e4e26-116">Add hello log analytics agent tooan Azure virtual machine</span></span>
* <span data-ttu-id="e4e26-117">Konfigurace protokolu analýzy tooindex data shromážděná pomocí Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="e4e26-117">Configure log analytics tooindex data collected using Azure diagnostics</span></span>

<span data-ttu-id="e4e26-118">Tento článek obsahuje dvě ukázky kódu, které ilustrují některé hello funkce, které můžete provádět z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4e26-118">This article provides two code samples that illustrate some of hello functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="e4e26-119">Může odkazovat toohello [odkazu na rutiny Powershellu Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) pro další funkce.</span><span class="sxs-lookup"><span data-stu-id="e4e26-119">You can refer toohello [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="e4e26-120">Analýzy protokolů volala dřív Operational Insights, proto je hello název používaný v hello rutiny.</span><span class="sxs-lookup"><span data-stu-id="e4e26-120">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e4e26-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4e26-121">Prerequisites</span></span>
<span data-ttu-id="e4e26-122">Tyto příklady fungovat s verzí 2.3.0 nebo později hello AzureRm.OperationalInsights modulu.</span><span class="sxs-lookup"><span data-stu-id="e4e26-122">These examples work with version 2.3.0 or later of hello AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="e4e26-123">Vytvořit a nakonfigurovat pracovní prostor Log Analytics</span><span class="sxs-lookup"><span data-stu-id="e4e26-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="e4e26-124">Hello následující ukázka skriptu je znázorněný postup:</span><span class="sxs-lookup"><span data-stu-id="e4e26-124">hello following script sample illustrates how to:</span></span>

1. <span data-ttu-id="e4e26-125">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="e4e26-125">Create a workspace</span></span>
2. <span data-ttu-id="e4e26-126">K dispozici řešení seznamu hello</span><span class="sxs-lookup"><span data-stu-id="e4e26-126">List hello available solutions</span></span>
3. <span data-ttu-id="e4e26-127">Přidání prostoru toohello řešení</span><span class="sxs-lookup"><span data-stu-id="e4e26-127">Add solutions toohello workspace</span></span>
4. <span data-ttu-id="e4e26-128">Importovat uložené hledání</span><span class="sxs-lookup"><span data-stu-id="e4e26-128">Import saved searches</span></span>
5. <span data-ttu-id="e4e26-129">Export uložené hledání</span><span class="sxs-lookup"><span data-stu-id="e4e26-129">Export saved searches</span></span>
6. <span data-ttu-id="e4e26-130">Vytvořit skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="e4e26-130">Create a computer group</span></span>
7. <span data-ttu-id="e4e26-131">Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem Windows hello</span><span class="sxs-lookup"><span data-stu-id="e4e26-131">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
8. <span data-ttu-id="e4e26-132">Z počítače se systémem Linux shromáždit čítače výkonu logický Disk (% použitých uzlů; Volné megabajty; % Využitého místa; Přenosy disku/s; Čtení disku/s; Zápis disku/s)</span><span class="sxs-lookup"><span data-stu-id="e4e26-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="e4e26-133">Shromažďovat události procesu syslog z počítače se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="e4e26-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="e4e26-134">Shromažďování událostí chyb a upozornění z hello protokolu událostí aplikace z počítače se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="e4e26-134">Collect Error and Warning events from hello Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="e4e26-135">Shromažďovat čítač výkonu paměť v MB k dispozici z počítače se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="e4e26-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="e4e26-136">Shromažďovat vlastní protokol</span><span class="sxs-lookup"><span data-stu-id="e4e26-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need toobe unique - Get-Random helps with this for hello example code
$Location = "westeurope"

# List of solutions tooenable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches tooimport
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log toocollect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create hello resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create hello workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up too5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-tooindex-azure-diagnostics"></a><span data-ttu-id="e4e26-137">Konfigurace tooindex analýzy protokolů Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="e4e26-137">Configuring Log Analytics tooindex Azure diagnostics</span></span>
<span data-ttu-id="e4e26-138">Hello prostředky pro monitorování bez agentů prostředků Azure, je nutné toohave Azure diagnostics povolené a nakonfigurované toowrite tooa pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="e4e26-138">For agentless monitoring of Azure resources, hello resources need toohave Azure diagnostics enabled and configured toowrite tooa Log Analytics workspace.</span></span> <span data-ttu-id="e4e26-139">Tento přístup odešle data přímo tooLog analýzy a nevyžaduje toobe data zapsána tooa účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e4e26-139">This approach sends data directly tooLog Analytics and does not require data toobe written tooa storage account.</span></span> <span data-ttu-id="e4e26-140">Podporované prostředky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="e4e26-140">Supported resources include:</span></span>

| <span data-ttu-id="e4e26-141">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="e4e26-141">Resource Type</span></span> | <span data-ttu-id="e4e26-142">Logs</span><span class="sxs-lookup"><span data-stu-id="e4e26-142">Logs</span></span> | <span data-ttu-id="e4e26-143">Metriky</span><span class="sxs-lookup"><span data-stu-id="e4e26-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e4e26-144">Brány Application Gateway</span><span class="sxs-lookup"><span data-stu-id="e4e26-144">Application Gateways</span></span>    | <span data-ttu-id="e4e26-145">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-145">Yes</span></span> | <span data-ttu-id="e4e26-146">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-146">Yes</span></span> |
| <span data-ttu-id="e4e26-147">Účty Automation</span><span class="sxs-lookup"><span data-stu-id="e4e26-147">Automation accounts</span></span>     | <span data-ttu-id="e4e26-148">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-148">Yes</span></span> | |
| <span data-ttu-id="e4e26-149">Účty batch</span><span class="sxs-lookup"><span data-stu-id="e4e26-149">Batch accounts</span></span>          | <span data-ttu-id="e4e26-150">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-150">Yes</span></span> | <span data-ttu-id="e4e26-151">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-151">Yes</span></span> |
| <span data-ttu-id="e4e26-152">Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="e4e26-152">Data Lake analytics</span></span>     | <span data-ttu-id="e4e26-153">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-153">Yes</span></span> | | 
| <span data-ttu-id="e4e26-154">Úložiště data Lake store</span><span class="sxs-lookup"><span data-stu-id="e4e26-154">Data Lake store</span></span>         | <span data-ttu-id="e4e26-155">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-155">Yes</span></span> | |
| <span data-ttu-id="e4e26-156">Fond elastické SQL</span><span class="sxs-lookup"><span data-stu-id="e4e26-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="e4e26-157">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-157">Yes</span></span> |
| <span data-ttu-id="e4e26-158">Názvový prostor události rozbočovače</span><span class="sxs-lookup"><span data-stu-id="e4e26-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="e4e26-159">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-159">Yes</span></span> |
| <span data-ttu-id="e4e26-160">Centra IoT</span><span class="sxs-lookup"><span data-stu-id="e4e26-160">IoT Hubs</span></span>                |     | <span data-ttu-id="e4e26-161">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-161">Yes</span></span> |
| <span data-ttu-id="e4e26-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="e4e26-162">Key Vault</span></span>               | <span data-ttu-id="e4e26-163">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-163">Yes</span></span> | |
| <span data-ttu-id="e4e26-164">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="e4e26-164">Load Balancers</span></span>          | <span data-ttu-id="e4e26-165">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-165">Yes</span></span> | |
| <span data-ttu-id="e4e26-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="e4e26-166">Logic Apps</span></span>              | <span data-ttu-id="e4e26-167">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-167">Yes</span></span> | <span data-ttu-id="e4e26-168">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-168">Yes</span></span> |
| <span data-ttu-id="e4e26-169">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="e4e26-169">Network Security Groups</span></span> | <span data-ttu-id="e4e26-170">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-170">Yes</span></span> | |
| <span data-ttu-id="e4e26-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e4e26-171">Redis Cache</span></span>             |     | <span data-ttu-id="e4e26-172">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-172">Yes</span></span> |
| <span data-ttu-id="e4e26-173">Služby hledání</span><span class="sxs-lookup"><span data-stu-id="e4e26-173">Search services</span></span>         | <span data-ttu-id="e4e26-174">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-174">Yes</span></span> | <span data-ttu-id="e4e26-175">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-175">Yes</span></span> |
| <span data-ttu-id="e4e26-176">Obor názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="e4e26-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="e4e26-177">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-177">Yes</span></span> |
| <span data-ttu-id="e4e26-178">SQL (v12)</span><span class="sxs-lookup"><span data-stu-id="e4e26-178">SQL (v12)</span></span>               |     | <span data-ttu-id="e4e26-179">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-179">Yes</span></span> |
| <span data-ttu-id="e4e26-180">Weby</span><span class="sxs-lookup"><span data-stu-id="e4e26-180">Web Sites</span></span>               |     | <span data-ttu-id="e4e26-181">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-181">Yes</span></span> |
| <span data-ttu-id="e4e26-182">Webové serverové farmy</span><span class="sxs-lookup"><span data-stu-id="e4e26-182">Web Server farms</span></span>        |     | <span data-ttu-id="e4e26-183">Ano</span><span class="sxs-lookup"><span data-stu-id="e4e26-183">Yes</span></span> |

<span data-ttu-id="e4e26-184">Podrobnosti hello k dispozici metrik hello najdete příliš[podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e4e26-184">For hello details of hello available metrics, refer too[supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="e4e26-185">Podrobnosti hello hello k dispozici protokoly najdete příliš[podporované služby a schématu pro diagnostické protokoly](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="e4e26-185">For hello details of hello available logs, refer too[supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="e4e26-186">Můžete také použít hello předcházející rutiny toocollect protokoly z prostředků, které se nacházejí v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="e4e26-186">You can also use hello preceding cmdlet toocollect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="e4e26-187">rutina Hello je možné toowork ve předplatných vzhledem k tomu, že zadání hello id prostředku obou hello vytváření protokoly a protokoly hello prostoru hello se odesílají do.</span><span class="sxs-lookup"><span data-stu-id="e4e26-187">hello cmdlet is able toowork across subscriptions since you are providing hello id of both hello resource creating logs and hello workspace hello logs are sent to.</span></span>


## <a name="configuring-log-analytics-tooindex-azure-diagnostics-from-storage"></a><span data-ttu-id="e4e26-188">Konfigurace tooindex analýzy protokolů Azure diagnostics z úložiště</span><span class="sxs-lookup"><span data-stu-id="e4e26-188">Configuring Log Analytics tooindex Azure diagnostics from storage</span></span>
<span data-ttu-id="e4e26-189">toocollect dat protokolu z v rámci spuštěnou instanci classic cloudové služby nebo service fabric cluster, musíte toofirst zápisu hello data tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="e4e26-189">toocollect log data from within a running instance of a classic cloud service or a service fabric cluster, you need toofirst write hello data tooAzure storage.</span></span> <span data-ttu-id="e4e26-190">Analýzy protokolů je pak nakonfigurovat toocollect hello protokoly z účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="e4e26-190">Log Analytics is then configured toocollect hello logs from hello storage account.</span></span> <span data-ttu-id="e4e26-191">Podporované prostředky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="e4e26-191">Supported resources include:</span></span>

* <span data-ttu-id="e4e26-192">Classic cloudových služeb (webové a pracovní role)</span><span class="sxs-lookup"><span data-stu-id="e4e26-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="e4e26-193">Clustery služby infrastruktury</span><span class="sxs-lookup"><span data-stu-id="e4e26-193">Service fabric clusters</span></span>

<span data-ttu-id="e4e26-194">Následující příklad ukazuje, jak Hello na:</span><span class="sxs-lookup"><span data-stu-id="e4e26-194">hello following example shows how to:</span></span>

1. <span data-ttu-id="e4e26-195">Seznam hello existující účty úložiště a umístění, které bude analýzy protokolů indexu dat z</span><span class="sxs-lookup"><span data-stu-id="e4e26-195">List hello existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="e4e26-196">Vytvoření konfigurace tooread z účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e4e26-196">Create a configuration tooread from a storage account</span></span>
3. <span data-ttu-id="e4e26-197">Nově vytvořený konfigurační tooindex data z dalších místech hello aktualizace</span><span class="sxs-lookup"><span data-stu-id="e4e26-197">Update hello newly created configuration tooindex data from additional locations</span></span>
4. <span data-ttu-id="e4e26-198">Odstranit konfiguraci nově vytvořeného hello</span><span class="sxs-lookup"><span data-stu-id="e4e26-198">Delete hello newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with hello storage account resource ID and hello storage account key for hello storage account you want tooLog Analytics too 
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove hello insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="e4e26-199">Můžete také použít hello předcházející skriptu toocollect protokoly z účty úložiště v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="e4e26-199">You can also use hello preceding script toocollect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="e4e26-200">vzhledem k tomu, že zadáváte id prostředků účtu úložiště hello a odpovídající přístupový klíč je Hello skript schopný toowork ve předplatných.</span><span class="sxs-lookup"><span data-stu-id="e4e26-200">hello script is able toowork across subscriptions since you are providing hello storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="e4e26-201">Pokud změníte hello přístupový klíč, je potřeba tooupdate hello úložiště přehledy toohave hello nový klíč.</span><span class="sxs-lookup"><span data-stu-id="e4e26-201">When you change hello access key, you need tooupdate hello storage insight toohave hello new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e4e26-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4e26-202">Next steps</span></span>
* <span data-ttu-id="e4e26-203">[Zkontrolujte rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) Další informace o použití prostředí PowerShell pro konfiguraci analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="e4e26-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

