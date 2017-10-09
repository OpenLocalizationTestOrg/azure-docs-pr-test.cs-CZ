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
# <a name="manage-log-analytics-using-powershell"></a>Správa služby Log Analytics pomocí PowerShellu
Můžete použít hello [rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform různé funkce v analýzy protokolů z příkazového řádku nebo v rámci skriptu.  Příklady hello úlohy, které můžete provádět pomocí prostředí PowerShell:

* Vytvoření pracovního prostoru
* Přidat nebo odebrat řešení
* Import a export uložených hledání
* Vytvořit skupinu počítačů
* Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem Windows hello
* Shromáždit čítače výkonu z počítačů se systémy Linux a Windows
* Shromažďování událostí z syslog počítačů se systémem Linux 
* Shromažďování událostí z protokolů událostí systému Windows
* Shromažďovat vlastní protokoly událostí
* Přidat hello log analytics agenta tooan virtuální počítač Azure
* Konfigurace protokolu analýzy tooindex data shromážděná pomocí Azure diagnostics

Tento článek obsahuje dvě ukázky kódu, které ilustrují některé hello funkce, které můžete provádět z prostředí PowerShell.  Může odkazovat toohello [odkazu na rutiny Powershellu Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) pro další funkce.

> [!NOTE]
> Analýzy protokolů volala dřív Operational Insights, proto je hello název používaný v hello rutiny.
> 
> 

## <a name="prerequisites"></a>Požadavky
Tyto příklady fungovat s verzí 2.3.0 nebo později hello AzureRm.OperationalInsights modulu.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Vytvořit a nakonfigurovat pracovní prostor Log Analytics
Hello následující ukázka skriptu je znázorněný postup:

1. Vytvoření pracovního prostoru
2. K dispozici řešení seznamu hello
3. Přidání prostoru toohello řešení
4. Importovat uložené hledání
5. Export uložené hledání
6. Vytvořit skupinu počítačů
7. Povolit shromažďování protokolů služby IIS z počítačů s nainstalovaným agentem Windows hello
8. Z počítače se systémem Linux shromáždit čítače výkonu logický Disk (% použitých uzlů; Volné megabajty; % Využitého místa; Přenosy disku/s; Čtení disku/s; Zápis disku/s)
9. Shromažďovat události procesu syslog z počítače se systémem Linux
10. Shromažďování událostí chyb a upozornění z hello protokolu událostí aplikace z počítače se systémem Windows
11. Shromažďovat čítač výkonu paměť v MB k dispozici z počítače se systémem Windows
12. Shromažďovat vlastní protokol 

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

## <a name="configuring-log-analytics-tooindex-azure-diagnostics"></a>Konfigurace tooindex analýzy protokolů Azure diagnostics
Hello prostředky pro monitorování bez agentů prostředků Azure, je nutné toohave Azure diagnostics povolené a nakonfigurované toowrite tooa pracovní prostor analýzy protokolů. Tento přístup odešle data přímo tooLog analýzy a nevyžaduje toobe data zapsána tooa účet úložiště. Podporované prostředky zahrnují:

| Typ prostředku | Logs | Metriky |
| --- | --- | --- |
| Brány Application Gateway    | Ano | Ano |
| Účty Automation     | Ano | |
| Účty batch          | Ano | Ano |
| Data Lake analytics     | Ano | | 
| Úložiště data Lake store         | Ano | |
| Fond elastické SQL        |     | Ano |
| Názvový prostor události rozbočovače     |     | Ano |
| Centra IoT                |     | Ano |
| Key Vault               | Ano | |
| Nástroje pro vyrovnávání zatížení          | Ano | |
| Logic Apps              | Ano | Ano |
| Network Security Groups (Skupiny zabezpečení sítě) | Ano | |
| Redis Cache             |     | Ano |
| Služby hledání         | Ano | Ano |
| Obor názvů Service Bus   |     | Ano |
| SQL (v12)               |     | Ano |
| Weby               |     | Ano |
| Webové serverové farmy        |     | Ano |

Podrobnosti hello k dispozici metrik hello najdete příliš[podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

Podrobnosti hello hello k dispozici protokoly najdete příliš[podporované služby a schématu pro diagnostické protokoly](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Můžete také použít hello předcházející rutiny toocollect protokoly z prostředků, které se nacházejí v různých předplatných. rutina Hello je možné toowork ve předplatných vzhledem k tomu, že zadání hello id prostředku obou hello vytváření protokoly a protokoly hello prostoru hello se odesílají do.


## <a name="configuring-log-analytics-tooindex-azure-diagnostics-from-storage"></a>Konfigurace tooindex analýzy protokolů Azure diagnostics z úložiště
toocollect dat protokolu z v rámci spuštěnou instanci classic cloudové služby nebo service fabric cluster, musíte toofirst zápisu hello data tooAzure úložiště. Analýzy protokolů je pak nakonfigurovat toocollect hello protokoly z účtu úložiště hello. Podporované prostředky zahrnují:

* Classic cloudových služeb (webové a pracovní role)
* Clustery služby infrastruktury

Následující příklad ukazuje, jak Hello na:

1. Seznam hello existující účty úložiště a umístění, které bude analýzy protokolů indexu dat z
2. Vytvoření konfigurace tooread z účtu úložiště
3. Nově vytvořený konfigurační tooindex data z dalších místech hello aktualizace
4. Odstranit konfiguraci nově vytvořeného hello

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

Můžete také použít hello předcházející skriptu toocollect protokoly z účty úložiště v různých předplatných. vzhledem k tomu, že zadáváte id prostředků účtu úložiště hello a odpovídající přístupový klíč je Hello skript schopný toowork ve předplatných. Pokud změníte hello přístupový klíč, je potřeba tooupdate hello úložiště přehledy toohave hello nový klíč.


## <a name="next-steps"></a>Další kroky
* [Zkontrolujte rutiny prostředí PowerShell Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) Další informace o použití prostředí PowerShell pro konfiguraci analýzy protokolů.

