---
title: "Pomocí prostředí PowerShell vytvořit a nakonfigurovat pracovní prostor Log Analytics | Microsoft Docs"
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
ms.openlocfilehash: 6807ab67e3593da82c147669b29bfdae3b6c967c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="cd5e1-104">Správa služby Log Analytics pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="cd5e1-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="cd5e1-105">Můžete použít [rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) k provádění různých funkcí v analýzy protokolů z příkazového řádku nebo v rámci skriptu.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-105">You can use the [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) to perform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="cd5e1-106">Příklady úlohy, které můžete provést pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cd5e1-106">Examples of the tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="cd5e1-107">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="cd5e1-107">Create a workspace</span></span>
* <span data-ttu-id="cd5e1-108">Přidat nebo odebrat řešení</span><span class="sxs-lookup"><span data-stu-id="cd5e1-108">Add or remove a solution</span></span>
* <span data-ttu-id="cd5e1-109">Import a export uložených hledání</span><span class="sxs-lookup"><span data-stu-id="cd5e1-109">Import and export saved searches</span></span>
* <span data-ttu-id="cd5e1-110">Vytvořit skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="cd5e1-110">Create a computer group</span></span>
* <span data-ttu-id="cd5e1-111">Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem systému Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-111">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
* <span data-ttu-id="cd5e1-112">Shromáždit čítače výkonu z počítačů se systémy Linux a Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="cd5e1-113">Shromažďování událostí z syslog počítačů se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="cd5e1-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="cd5e1-114">Shromažďování událostí z protokolů událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="cd5e1-115">Shromažďovat vlastní protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="cd5e1-115">Collect custom event logs</span></span>
* <span data-ttu-id="cd5e1-116">Přidat agenta analýzy protokolů pro virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="cd5e1-116">Add the log analytics agent to an Azure virtual machine</span></span>
* <span data-ttu-id="cd5e1-117">Konfigurace analýzy protokolů pro data indexu shromažďována pomocí Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="cd5e1-117">Configure log analytics to index data collected using Azure diagnostics</span></span>

<span data-ttu-id="cd5e1-118">Tento článek obsahuje dvě ukázky kódu, které ilustrovat některé z funkcí, které lze provádět z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-118">This article provides two code samples that illustrate some of the functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="cd5e1-119">Můžete se podívat do [odkazu na rutiny Powershellu Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) pro další funkce.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-119">You can refer to the [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="cd5e1-120">Analýzy protokolů volala dřív Operational Insights, proto je název používaný v rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-120">Log Analytics was previously called Operational Insights, which is why it is the name used in the cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="cd5e1-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd5e1-121">Prerequisites</span></span>
<span data-ttu-id="cd5e1-122">Tyto příklady fungovat s verzí 2.3.0 nebo později AzureRm.OperationalInsights modulu.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-122">These examples work with version 2.3.0 or later of the AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="cd5e1-123">Vytvořit a nakonfigurovat pracovní prostor Log Analytics</span><span class="sxs-lookup"><span data-stu-id="cd5e1-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="cd5e1-124">Znázorňuje následující ukázka skriptu postup:</span><span class="sxs-lookup"><span data-stu-id="cd5e1-124">The following script sample illustrates how to:</span></span>

1. <span data-ttu-id="cd5e1-125">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="cd5e1-125">Create a workspace</span></span>
2. <span data-ttu-id="cd5e1-126">Seznam dostupných řešení</span><span class="sxs-lookup"><span data-stu-id="cd5e1-126">List the available solutions</span></span>
3. <span data-ttu-id="cd5e1-127">Přidat řešení do pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="cd5e1-127">Add solutions to the workspace</span></span>
4. <span data-ttu-id="cd5e1-128">Importovat uložené hledání</span><span class="sxs-lookup"><span data-stu-id="cd5e1-128">Import saved searches</span></span>
5. <span data-ttu-id="cd5e1-129">Export uložené hledání</span><span class="sxs-lookup"><span data-stu-id="cd5e1-129">Export saved searches</span></span>
6. <span data-ttu-id="cd5e1-130">Vytvořit skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="cd5e1-130">Create a computer group</span></span>
7. <span data-ttu-id="cd5e1-131">Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem systému Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-131">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
8. <span data-ttu-id="cd5e1-132">Z počítače se systémem Linux shromáždit čítače výkonu logický Disk (% použitých uzlů; Volné megabajty; % Využitého místa; Přenosy disku/s; Čtení disku/s; Zápis disku/s)</span><span class="sxs-lookup"><span data-stu-id="cd5e1-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="cd5e1-133">Shromažďovat události procesu syslog z počítače se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="cd5e1-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="cd5e1-134">Shromažďování událostí chyb a upozornění z protokolu událostí aplikace z počítače se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-134">Collect Error and Warning events from the Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="cd5e1-135">Shromažďovat čítač výkonu paměť v MB k dispozici z počítače se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="cd5e1-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="cd5e1-136">Shromažďovat vlastní protokol</span><span class="sxs-lookup"><span data-stu-id="cd5e1-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
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

# Custom Log to collect
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

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
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

# Create a computer group based on names (up to 5000)
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

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a><span data-ttu-id="cd5e1-137">Konfigurace analýzy protokolů do indexu Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="cd5e1-137">Configuring Log Analytics to index Azure diagnostics</span></span>
<span data-ttu-id="cd5e1-138">Pro monitorování bez agentů prostředků Azure, prostředky musí být Azure diagnostics povolené a nakonfigurované k zápisu do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-138">For agentless monitoring of Azure resources, the resources need to have Azure diagnostics enabled and configured to write to a Log Analytics workspace.</span></span> <span data-ttu-id="cd5e1-139">Tento přístup přímo k Log Analytics odesílá data a nevyžaduje data k zápisu do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-139">This approach sends data directly to Log Analytics and does not require data to be written to a storage account.</span></span> <span data-ttu-id="cd5e1-140">Podporované prostředky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="cd5e1-140">Supported resources include:</span></span>

| <span data-ttu-id="cd5e1-141">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="cd5e1-141">Resource Type</span></span> | <span data-ttu-id="cd5e1-142">Logs</span><span class="sxs-lookup"><span data-stu-id="cd5e1-142">Logs</span></span> | <span data-ttu-id="cd5e1-143">Metriky</span><span class="sxs-lookup"><span data-stu-id="cd5e1-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd5e1-144">Brány Application Gateway</span><span class="sxs-lookup"><span data-stu-id="cd5e1-144">Application Gateways</span></span>    | <span data-ttu-id="cd5e1-145">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-145">Yes</span></span> | <span data-ttu-id="cd5e1-146">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-146">Yes</span></span> |
| <span data-ttu-id="cd5e1-147">Účty Automation</span><span class="sxs-lookup"><span data-stu-id="cd5e1-147">Automation accounts</span></span>     | <span data-ttu-id="cd5e1-148">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-148">Yes</span></span> | |
| <span data-ttu-id="cd5e1-149">Účty batch</span><span class="sxs-lookup"><span data-stu-id="cd5e1-149">Batch accounts</span></span>          | <span data-ttu-id="cd5e1-150">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-150">Yes</span></span> | <span data-ttu-id="cd5e1-151">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-151">Yes</span></span> |
| <span data-ttu-id="cd5e1-152">Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="cd5e1-152">Data Lake analytics</span></span>     | <span data-ttu-id="cd5e1-153">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-153">Yes</span></span> | | 
| <span data-ttu-id="cd5e1-154">Úložiště data Lake store</span><span class="sxs-lookup"><span data-stu-id="cd5e1-154">Data Lake store</span></span>         | <span data-ttu-id="cd5e1-155">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-155">Yes</span></span> | |
| <span data-ttu-id="cd5e1-156">Fond elastické SQL</span><span class="sxs-lookup"><span data-stu-id="cd5e1-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="cd5e1-157">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-157">Yes</span></span> |
| <span data-ttu-id="cd5e1-158">Názvový prostor události rozbočovače</span><span class="sxs-lookup"><span data-stu-id="cd5e1-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="cd5e1-159">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-159">Yes</span></span> |
| <span data-ttu-id="cd5e1-160">Centra IoT</span><span class="sxs-lookup"><span data-stu-id="cd5e1-160">IoT Hubs</span></span>                |     | <span data-ttu-id="cd5e1-161">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-161">Yes</span></span> |
| <span data-ttu-id="cd5e1-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="cd5e1-162">Key Vault</span></span>               | <span data-ttu-id="cd5e1-163">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-163">Yes</span></span> | |
| <span data-ttu-id="cd5e1-164">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="cd5e1-164">Load Balancers</span></span>          | <span data-ttu-id="cd5e1-165">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-165">Yes</span></span> | |
| <span data-ttu-id="cd5e1-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="cd5e1-166">Logic Apps</span></span>              | <span data-ttu-id="cd5e1-167">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-167">Yes</span></span> | <span data-ttu-id="cd5e1-168">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-168">Yes</span></span> |
| <span data-ttu-id="cd5e1-169">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="cd5e1-169">Network Security Groups</span></span> | <span data-ttu-id="cd5e1-170">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-170">Yes</span></span> | |
| <span data-ttu-id="cd5e1-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="cd5e1-171">Redis Cache</span></span>             |     | <span data-ttu-id="cd5e1-172">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-172">Yes</span></span> |
| <span data-ttu-id="cd5e1-173">Služby hledání</span><span class="sxs-lookup"><span data-stu-id="cd5e1-173">Search services</span></span>         | <span data-ttu-id="cd5e1-174">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-174">Yes</span></span> | <span data-ttu-id="cd5e1-175">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-175">Yes</span></span> |
| <span data-ttu-id="cd5e1-176">Obor názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="cd5e1-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="cd5e1-177">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-177">Yes</span></span> |
| <span data-ttu-id="cd5e1-178">SQL (v12)</span><span class="sxs-lookup"><span data-stu-id="cd5e1-178">SQL (v12)</span></span>               |     | <span data-ttu-id="cd5e1-179">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-179">Yes</span></span> |
| <span data-ttu-id="cd5e1-180">Weby</span><span class="sxs-lookup"><span data-stu-id="cd5e1-180">Web Sites</span></span>               |     | <span data-ttu-id="cd5e1-181">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-181">Yes</span></span> |
| <span data-ttu-id="cd5e1-182">Webové serverové farmy</span><span class="sxs-lookup"><span data-stu-id="cd5e1-182">Web Server farms</span></span>        |     | <span data-ttu-id="cd5e1-183">Ano</span><span class="sxs-lookup"><span data-stu-id="cd5e1-183">Yes</span></span> |

<span data-ttu-id="cd5e1-184">Podrobnosti k dispozici metrik [podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="cd5e1-184">For the details of the available metrics, refer to [supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="cd5e1-185">Podrobnosti o dostupných protokolů, najdete v části [podporované služby a schématu pro diagnostické protokoly](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cd5e1-185">For the details of the available logs, refer to [supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="cd5e1-186">Můžete také použít rutinu předchozí ke shromažďování protokolů z prostředků, které se nacházejí v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-186">You can also use the preceding cmdlet to collect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="cd5e1-187">Rutina je možné pracovat ve předplatných vzhledem k tomu, že zadáte id prostředku vytváření protokoly a protokoly jsou odeslána do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-187">The cmdlet is able to work across subscriptions since you are providing the id of both the resource creating logs and the workspace the logs are sent to.</span></span>


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a><span data-ttu-id="cd5e1-188">Konfigurace analýzy protokolů do indexu Azure diagnostics z úložiště</span><span class="sxs-lookup"><span data-stu-id="cd5e1-188">Configuring Log Analytics to index Azure diagnostics from storage</span></span>
<span data-ttu-id="cd5e1-189">Shromažďování dat protokolu z v rámci spuštěnou instanci classic cloudové služby nebo service fabric cluster, musíte nejprve zapíše data do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-189">To collect log data from within a running instance of a classic cloud service or a service fabric cluster, you need to first write the data to Azure storage.</span></span> <span data-ttu-id="cd5e1-190">Analýzy protokolů je nakonfigurovaný pro shromažďování protokolů z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-190">Log Analytics is then configured to collect the logs from the storage account.</span></span> <span data-ttu-id="cd5e1-191">Podporované prostředky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="cd5e1-191">Supported resources include:</span></span>

* <span data-ttu-id="cd5e1-192">Classic cloudových služeb (webové a pracovní role)</span><span class="sxs-lookup"><span data-stu-id="cd5e1-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="cd5e1-193">Clustery služby infrastruktury</span><span class="sxs-lookup"><span data-stu-id="cd5e1-193">Service fabric clusters</span></span>

<span data-ttu-id="cd5e1-194">Následující příklad ukazuje postup:</span><span class="sxs-lookup"><span data-stu-id="cd5e1-194">The following example shows how to:</span></span>

1. <span data-ttu-id="cd5e1-195">Seznam existující účty úložiště a umístění, které bude analýzy protokolů indexu dat z</span><span class="sxs-lookup"><span data-stu-id="cd5e1-195">List the existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="cd5e1-196">Vytvořit konfiguraci, kterou chcete číst z účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="cd5e1-196">Create a configuration to read from a storage account</span></span>
3. <span data-ttu-id="cd5e1-197">Aktualizace se nově vytvořená konfigurace data indexu z další umístění</span><span class="sxs-lookup"><span data-stu-id="cd5e1-197">Update the newly created configuration to index data from additional locations</span></span>
4. <span data-ttu-id="cd5e1-198">Odstranit nově vytvořenou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="cd5e1-198">Delete the newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="cd5e1-199">Můžete taky uvedený skript ke shromažďování protokolů z účty úložiště v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-199">You can also use the preceding script to collect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="cd5e1-200">Skript je možné pracovat ve předplatných vzhledem k tomu, že zadáváte id prostředků účtu úložiště a odpovídající přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-200">The script is able to work across subscriptions since you are providing the storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="cd5e1-201">Když změníte přístupový klíč, je potřeba aktualizovat náhled úložiště tak, aby měl nový klíč.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-201">When you change the access key, you need to update the storage insight to have the new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cd5e1-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd5e1-202">Next steps</span></span>
* <span data-ttu-id="cd5e1-203">[Zkontrolujte rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) Další informace o použití prostředí PowerShell pro konfiguraci analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="cd5e1-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

