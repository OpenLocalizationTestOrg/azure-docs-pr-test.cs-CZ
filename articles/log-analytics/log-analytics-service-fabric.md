---
title: "aaaAssess aplikace Service Fabric se analýza protokolů Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Hello Service Fabric řešení můžete použít v analýzy protokolů pomocí prostředí PowerShell tooassess hello rizika a stavu aplikací Service Fabric, micro-services, uzly a clustery."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 2047b3fa-96b1-4230-af5d-a4c331d973ce
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: nini
ms.openlocfilehash: 3f6d6c0df02d6d453b77e50b75b64bf7eb73bbbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assess-azure-service-fabric-applications-and-micro-services-with-powershell"></a><span data-ttu-id="64340-103">Vyhodnocení aplikace Azure Service Fabric a micro-services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="64340-103">Assess Azure Service Fabric applications and micro-services with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="64340-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="64340-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="64340-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="64340-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>


![Symbol Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="64340-107">Tento článek popisuje, jak toouse hello řešení Service Fabric v toohelp Log Analytics identifikovat a řešit problémy mezi cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64340-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span> <span data-ttu-id="64340-108">Pomáhá můžete zjistit, jak Service Fabric uzly fungují a jak vašim aplikacím a službám micro běží.</span><span class="sxs-lookup"><span data-stu-id="64340-108">It helps you see how your Service Fabric nodes are performing and how your applications and micro-services are running.</span></span>

<span data-ttu-id="64340-109">Hello Service Fabric řešení používá Azure Diagnostics data z virtuálních počítačů služby infrastruktury, shromažďováním těchto dat z Azure WAD tabulek.</span><span class="sxs-lookup"><span data-stu-id="64340-109">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="64340-110">Analýzy protokolů potom načte hello následující události Service Fabric framework:</span><span class="sxs-lookup"><span data-stu-id="64340-110">Log Analytics then reads hello following Service Fabric framework events:</span></span>

- <span data-ttu-id="64340-111">**Události spolehlivé služby**</span><span class="sxs-lookup"><span data-stu-id="64340-111">**Reliable Service Events**</span></span>
- <span data-ttu-id="64340-112">**Události objektu actor**</span><span class="sxs-lookup"><span data-stu-id="64340-112">**Actor Events**</span></span>
- <span data-ttu-id="64340-113">**Provozní události**</span><span class="sxs-lookup"><span data-stu-id="64340-113">**Operational Events**</span></span>
- <span data-ttu-id="64340-114">**Vlastní události trasování událostí pro Windows**</span><span class="sxs-lookup"><span data-stu-id="64340-114">**Custom ETW events**</span></span>

<span data-ttu-id="64340-115">řídicí panel řešení Service Fabric Hello zobrazuje můžete významné problémy a relevantní události ve vašem prostředí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64340-115">hello Service Fabric solution dashboard shows you notable issues and relevant events in your Service Fabric environment.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="64340-116">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="64340-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="64340-117">Postupujte podle těchto tří jednoduché kroky tooinstall a nakonfigurujte hello řešení:</span><span class="sxs-lookup"><span data-stu-id="64340-117">Follow these three easy steps tooinstall and configure hello solution:</span></span>

1. <span data-ttu-id="64340-118">Přidružte hello předplatné Azure použité toocreate všechny prostředky clusteru, včetně účtů úložiště s pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="64340-118">Associate hello Azure subscription that you used toocreate all cluster resources, including storage accounts, with your workspace.</span></span> <span data-ttu-id="64340-119">V tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md) informace o vytváření pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="64340-119">See [Get started with Log Analytics](log-analytics-get-started.md) for information about creating a Log Analytics workspace.</span></span>
2. <span data-ttu-id="64340-120">Nakonfigurujte toocollect analýzy protokolů a zobrazit protokoly Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64340-120">Configure Log Analytics toocollect and view Service Fabric logs.</span></span>
3. <span data-ttu-id="64340-121">Povolte hello řešení Service Fabric v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="64340-121">Enable hello Service Fabric solution in your workspace.</span></span>

## <a name="configure-log-analytics-toocollect-and-view-service-fabric-logs"></a><span data-ttu-id="64340-122">Nakonfigurujte toocollect analýzy protokolů a zobrazit protokoly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64340-122">Configure Log Analytics toocollect and view Service Fabric logs</span></span>
<span data-ttu-id="64340-123">V této části se dozvíte, jak tooconfigure analýzy protokolů tooretrieve Service Fabric protokoly.</span><span class="sxs-lookup"><span data-stu-id="64340-123">In this section, you learn how tooconfigure Log Analytics tooretrieve Service Fabric logs.</span></span> <span data-ttu-id="64340-124">protokoly Hello umožňují tooview, analyzovat a řešení problémů v clusteru nebo v hello aplikací a služeb spuštěných v daném clusteru pomocí portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="64340-124">hello logs allow you tooview, analyze, and troubleshoot issues in your cluster or in hello applications and services running in that cluster, using hello OMS portal.</span></span>

> [!NOTE]
> <span data-ttu-id="64340-125">Nakonfigurujte hello Azure Diagnostics rozšíření tooupload hello protokoly pro úložiště tabulek.</span><span class="sxs-lookup"><span data-stu-id="64340-125">Configure hello Azure Diagnostics extension tooupload hello logs for storage tables.</span></span> <span data-ttu-id="64340-126">Hello tabulky se musí shodovat co hledá analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="64340-126">hello tables must match what Log Analytics looks for.</span></span> <span data-ttu-id="64340-127">Další informace najdete v tématu [jak toocollect protokoly s Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="64340-127">For more information, see [How toocollect logs with Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span></span> <span data-ttu-id="64340-128">Hello konfigurace nastavení příklady v tomto článku zobrazit, jaké názvy hello hello úložiště by měla být tabulky.</span><span class="sxs-lookup"><span data-stu-id="64340-128">hello configuration settings examples in this article show you what hello names of hello storage tables should be.</span></span> <span data-ttu-id="64340-129">Jakmile diagnostiky je nastavený na hello clusteru a odesílá účet úložiště tooa protokoly, hello dalším krokem je tooconfigure analýzy protokolů toocollect tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="64340-129">Once Diagnostics is set up on hello cluster and is uploading logs tooa storage account, hello next step is tooconfigure Log Analytics toocollect these logs.</span></span>
>
>

<span data-ttu-id="64340-130">Ujistěte se, že aktualizujete hello **EtwEventSourceProviderConfiguration** část v hello **template.json** souboru tooadd záznamy pro nové EventSources před použitím konfigurace hello aktualizovat hello spuštění **deploy.ps1**.</span><span class="sxs-lookup"><span data-stu-id="64340-130">Ensure that you update hello **EtwEventSourceProviderConfiguration** section in hello **template.json** file tooadd entries for hello new EventSources before you apply hello configuration update by running **deploy.ps1**.</span></span> <span data-ttu-id="64340-131">Hello tabulky pro nahrávání hello stejné jako (ETWEventTable).</span><span class="sxs-lookup"><span data-stu-id="64340-131">hello table for upload is hello same as (ETWEventTable).</span></span> <span data-ttu-id="64340-132">Momentálně hello, analýzy protokolů můžete číst data jenom události trasování událostí aplikace na hello *WADETWEventTable* tabulky.</span><span class="sxs-lookup"><span data-stu-id="64340-132">At hello moment, Log Analytics can only read application ETW events from hello *WADETWEventTable* table.</span></span>

<span data-ttu-id="64340-133">Hello tyto nástroje jsou použité tooperform některé operace hello v této části:</span><span class="sxs-lookup"><span data-stu-id="64340-133">hello following tools are used tooperform some of hello operations in this section:</span></span>

* <span data-ttu-id="64340-134">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="64340-134">Azure PowerShell</span></span>
* [<span data-ttu-id="64340-135">Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="64340-135">Operations Management Suite</span></span>](http://www.microsoft.com/oms)

### <a name="configure-a-log-analytics-workspace-tooshow-hello-cluster-logs"></a><span data-ttu-id="64340-136">Konfigurace analýzy protokolů prostoru tooshow hello clusteru protokolů</span><span class="sxs-lookup"><span data-stu-id="64340-136">Configure a Log Analytics workspace tooshow hello cluster logs</span></span>

<span data-ttu-id="64340-137">Po vytvoření pracovního prostoru analýzy protokolů, nakonfigurujte hello prostoru toopull protokoly z tabulky úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="64340-137">After you create a Log Analytics workspace, configure hello workspace toopull logs from hello Azure storage tables.</span></span> <span data-ttu-id="64340-138">Potom spusťte následující skript prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="64340-138">Then, run hello following PowerShell script:</span></span>

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) tooread Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for hello subscription tooconfigure.
    If you have more than one Log Analytics workspace you will be prompted for hello workspace tooconfigure.
    It will then look through your Service Fabric clusters, and configure your Log Analytics workspace tooread Diagnostics from storage accounts that are connected toothat cluster and have diagnostics enabled.
#>

try
{
    Get-AzureRMContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Add-AzureRmAccount
}

$validTables = "WADServiceFabric*EventTable", "WADETWEventTable"
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"

            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.Name + " (" + $subscription.Id + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.Id
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  

    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found. `n"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
             Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}

function Check-ETWProviderLogging {
     param(
     [string]$id,
     [string]$provider,
     [string]$expectedTable,
     [string]$table
    )       
         Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
         if ( ($table -eq $null) -or ($table -eq ""))  
         {
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
         }
 }

function Check-ServiceFabricScaleSetDiagnostics {
     param(
          [psobject]$scaleSetDiagnostics
   )
     $storageAccountsFound = @()
     Write-Verbose ("Checking " + $scaleSetDiagnostics)
     $sfReliableActorTable = $null
     $sfReliableServiceTable = $null
     $sfOperationalTable = $null

     Write-Debug $scaleSetDiagnostics
     $serviceFabricProviderList = ""
     $etwManifestProviderList = ""

     if ( $scaleSetDiagnostics.xmlCfg )  
      {
             Write-Debug ("Found XMLcfg")
             $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
             Write-Debug $xmlCfg
             $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                 
             $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
             $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
      } elseif ($scaleSetDiagnostics.WadCfg )  
     {
         Write-Debug ("Found WADcfg")
         Write-Debug $scaleSetDiagnostics.WadCfg
         $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
         $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
     } else
     {
         Write-Error "Unable tooparse Azure Diagnostics setting for $id"
             Write-Warning ("$id does not have diagnostics enabled")
     }
     foreach ($provider in $serviceFabricProviderList)  
     {
         Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
         {
             $sfReliableActorTable = $provider.DefaultEvents.eventDestination  
         } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")  
         {  
             $sfReliableServiceTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }
     foreach ($provider in $etwManifestProviderList)
     {
         Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
         {
             $sfOperationalTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }

     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
     Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

     Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)
     $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
     return ($storageAccountsFound)
 }

function Select-StorageAccount {
    $allResources = Get-AzureRmResource #pulls in all resources
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in hello resource
    $storageAccountList = @()
    foreach($cluster in $serviceFabricClusters) {
        Write-Host("Checking cluster: " + $cluster.Name)
         $scaleSet = $allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)})

         foreach($set in $scaleSet) {
             $resource = Get-AzureRmResource -ResourceId $set.ResourceId
             $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions

             foreach($ext in $extensions) {
                 if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                     $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
                 }
             }
          }

         $storageAccountsToCheck = $allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)})

         if ($storageAccountsToCheck.Count -eq "0") {
                Write-Error "No storage accounts found"
           }
           else {
                    foreach ($storageAccount in $storageAccountsToCheck) {
                        Write-Host("Checking Storage Account: " + $storageAccount.Name)
                        $insightsName = $storageAccount.Name + $workspace.Name
                        $existingConfig = ""
                        try
                            {
                                $existingConfig = Get-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -ErrorAction Stop
                            }
                        catch
                            {
                                # HTTP Not Found is returned if hello storage insight doesn't exist
                            }
                        if ($existingConfig) {                         
                                  [array]$Tables = $existingConfig.Tables
                                   foreach($table in $validTables) {
                                         if($Tables -notcontains $table) {
                                               $Tables += $table
                                               $dirty = $true;
                                               Write-Host "Adding Table: $table";
                                         }
                                         else {
                                               Write-Host "$table is already configured.`n";
                                             }
                                      }
                                      # If any of hello tables from hello table list are not already monitored, then we add them
                                   if($dirty -eq $true) {
                                           Set-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -Tables $Tables
                                           Write-Host "Updating Storage Insight. `n"
                                    }
                                    else {
                                           Write-Host "Storage Insight already updated."
                                  }
                          }
                     else {
                            $key = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.Name)[0].Value
                           New-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -StorageAccountResourceId $storageAccount.ResourceId -StorageAccountKey $key -Tables $validTables
                            Write-Host "New Azure Storage Insight Configured. `n"
                           }
                    }
             }
      }
      return
     }

$subscription = Select-Subscription
$subscriptionId = $subscription.SubscriptionId
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
$storageAccount = Select-StorageAccount
```

<span data-ttu-id="64340-139">Po dokončení konfigurace tooread pracovní prostor analýzy protokolů hello z hello Azure tabulek v účtu úložiště, přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64340-139">After you've configured hello Log Analytics workspace tooread from hello Azure tables in your storage account, log in toohello Azure portal.</span></span> <span data-ttu-id="64340-140">Vyberte pracovní prostor analýzy protokolů hello z **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="64340-140">Select hello Log Analytics workspace from **All Resources**.</span></span> <span data-ttu-id="64340-141">Zobrazí se počet Hello prostoru toohello protokoly připojené účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="64340-141">hello number of storage account logs connected toohello workspace is displayed.</span></span> <span data-ttu-id="64340-142">Vyberte hello **protokol účtu úložiště** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="64340-142">Select hello **Storage account logs** tile.</span></span> <span data-ttu-id="64340-143">Zkontrolujte seznam hello tooverify protokoly účet úložiště, že váš účet úložiště je připojený toohello správné prostoru.</span><span class="sxs-lookup"><span data-stu-id="64340-143">Review hello list of storage account logs tooverify that your storage account is connected toohello correct workspace.</span></span>

![Protokol účtu úložiště](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-hello-service-fabric-solution"></a><span data-ttu-id="64340-145">Povolit řešení hello Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64340-145">Enable hello Service Fabric solution</span></span>
<span data-ttu-id="64340-146">Použijte následující skript tooadd hello řešení tooyour pracovní prostor analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="64340-146">Use hello following script tooadd hello solution tooyour Log Analytics workspace.</span></span> <span data-ttu-id="64340-147">Spusťte skript hello v prostředí PowerShell, pomocí hello předplatné Azure, který je přidružen hello pracovní prostor analýzy protokolů, který požadujete tooenable hello Service Fabric řešení.</span><span class="sxs-lookup"><span data-stu-id="64340-147">Run hello script in PowerShell, using hello Azure subscription that is associated with hello Log Analytics workspace that you want tooenable hello Service Fabric solution in.</span></span>

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"
            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.SubscriptionName + " (" + $subscription.SubscriptionId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.SubscriptionId
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  
    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
                           Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}
$subscription = Select-Subscription
$subscriptionId = $subscription.Id
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -IntelligencePackName "ServiceFabric" -Enabled $true
```

<span data-ttu-id="64340-148">Jakmile povolíte hello řešení, dlaždice hello Service Fabric se přidá tooyour analýzy protokolů *přehled* stránky.</span><span class="sxs-lookup"><span data-stu-id="64340-148">After you enable hello solution, hello Service Fabric tile is added tooyour Log Analytics *Overview* page.</span></span> <span data-ttu-id="64340-149">Hello stránka zobrazuje zobrazení významné problémy, jako je například runAsync selhání a zrušení, ke kterým došlo v hello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="64340-149">hello page shows a view of notable issues such as runAsync failures and cancellations that occurred in hello last 24 hours.</span></span>

![Dlaždice Service Fabric](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a><span data-ttu-id="64340-151">Zobrazit události Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64340-151">View Service Fabric events</span></span>
<span data-ttu-id="64340-152">Klikněte na tlačítko hello **Service Fabric** řídicí panel dlaždice tooopen hello Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64340-152">Click hello **Service Fabric** tile tooopen hello Service Fabric dashboard.</span></span> <span data-ttu-id="64340-153">řídicí panel Hello zahrnuje hello sloupce v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="64340-153">hello dashboard includes hello columns in hello table that follows.</span></span> <span data-ttu-id="64340-154">Každý sloupec uvádí hello top 10 události pomocí počet odpovídajících, že kritéria sloupce pro hello zadaný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="64340-154">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="64340-155">Můžete spustit hledání protokolu, které poskytuje celý seznam hello kliknutím **zobrazit všechny** v pravém dolním hello jednotlivých sloupců, nebo klikněte na záhlaví sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="64340-155">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="64340-156">**Události Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="64340-156">**Service Fabric event**</span></span> | <span data-ttu-id="64340-157">**Popis**</span><span class="sxs-lookup"><span data-stu-id="64340-157">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="64340-158">Významné problémy</span><span class="sxs-lookup"><span data-stu-id="64340-158">Notable Issues</span></span> | <span data-ttu-id="64340-159">Zobrazí problémy, včetně RunAsyncFailures, RunAsynCancellations a seznamy uzlu.</span><span class="sxs-lookup"><span data-stu-id="64340-159">Displays issues including RunAsyncFailures, RunAsynCancellations, and Node Downs.</span></span> |
| <span data-ttu-id="64340-160">Provozní události</span><span class="sxs-lookup"><span data-stu-id="64340-160">Operational Events</span></span> | <span data-ttu-id="64340-161">Zobrazuje významné provozní události, včetně upgradu aplikace a nasazení.</span><span class="sxs-lookup"><span data-stu-id="64340-161">Displays notable operational events including application upgrade and deployments.</span></span> |
| <span data-ttu-id="64340-162">Události spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="64340-162">Reliable Service Events</span></span> | <span data-ttu-id="64340-163">Zobrazí události významné spolehlivé služby včetně Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="64340-163">Displays notable reliable service events including  Runasyncinvocations.</span></span> |
| <span data-ttu-id="64340-164">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="64340-164">Actor Events</span></span> | <span data-ttu-id="64340-165">Zobrazuje významné objektu actor události vygenerované modulem micro-services.</span><span class="sxs-lookup"><span data-stu-id="64340-165">Displays notable actor events generated by your micro-services.</span></span> <span data-ttu-id="64340-166">Události zahrnují výjimky vyvolané metodu objektu actor, objektu actor aktivací a deaktivací a tak dále.</span><span class="sxs-lookup"><span data-stu-id="64340-166">Events include exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="64340-167">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="64340-167">Application Events</span></span> | <span data-ttu-id="64340-168">Zobrazí všechny vlastní události trasování událostí generovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="64340-168">Displays all custom ETW events generated by your applications.</span></span> |

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="64340-171">Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="64340-171">hello following table shows data collection methods and other details about how data is collected for Service Fabric:</span></span>

| <span data-ttu-id="64340-172">Platforma</span><span class="sxs-lookup"><span data-stu-id="64340-172">platform</span></span> | <span data-ttu-id="64340-173">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="64340-173">Direct Agent</span></span> | <span data-ttu-id="64340-174">Agent nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="64340-174">Operations Manager agent</span></span> | <span data-ttu-id="64340-175">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="64340-175">Azure Storage</span></span> | <span data-ttu-id="64340-176">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="64340-176">Operations Manager required?</span></span> | <span data-ttu-id="64340-177">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="64340-177">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="64340-178">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="64340-178">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="64340-179">Windows</span><span class="sxs-lookup"><span data-stu-id="64340-179">Windows</span></span> |  |  | <span data-ttu-id="64340-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="64340-180">&#8226;</span></span> |  |  |<span data-ttu-id="64340-181">10 minut</span><span class="sxs-lookup"><span data-stu-id="64340-181">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="64340-182">Změnit rozsah hello událostí s **dat podle posledních sedmi dnů** hello horní části řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="64340-182">Change hello scope of events with **Data based on last seven days** at hello top of hello dashboard.</span></span> <span data-ttu-id="64340-183">Můžete také zobrazit události generované v rámci hello posledních sedmi dnů, jeden den nebo šest hodin.</span><span class="sxs-lookup"><span data-stu-id="64340-183">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="64340-184">Nebo můžete vybrat **vlastní** toospecify vlastní období.</span><span class="sxs-lookup"><span data-stu-id="64340-184">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="troubleshoot-your-service-fabric-and-log-analytics-configuration"></a><span data-ttu-id="64340-185">Řešení potíží s konfiguraci Service Fabric a analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="64340-185">Troubleshoot your Service Fabric and Log Analytics configuration</span></span>
<span data-ttu-id="64340-186">Pokud potřebujete tooverify konfiguraci analýzy protokolů vzhledem k tomu, že jsou data události nelze tooview v analýzy protokolů, použijte následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="64340-186">If you need tooverify your Log Analytics configuration because you are unable tooview event data in Log Analytics, use hello following script.</span></span> <span data-ttu-id="64340-187">Provede hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="64340-187">It performs hello following actions:</span></span>

1. <span data-ttu-id="64340-188">Načte konfiguraci diagnostiky Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64340-188">Reads your Service Fabric diagnostics configuration</span></span>
2. <span data-ttu-id="64340-189">Kontroluje data zapsaná do tabulek hello</span><span class="sxs-lookup"><span data-stu-id="64340-189">Checks for data written into hello tables</span></span>
3. <span data-ttu-id="64340-190">Ověřuje, že analýzy protokolů je nakonfigurované tooread z tabulek hello</span><span class="sxs-lookup"><span data-stu-id="64340-190">Verifies that Log Analytics is configured tooread from hello tables</span></span>

```
<#
    Verify Service Fabric and Log Analytics configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into hello tables
    3. Verify Log Analytics is configured tooread from hello tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    toosee items that are correctly configured set $VerbosePreference="Continue"
#>
Param
(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true,
    Position=1)]
    [string]$workspaceName
)

$WADtables = @("WADServiceFabricReliableActorEventTable",
               "WADServiceFabricReliableServiceEventTable",
               "WADServiceFabricSystemEventTable",
               "WADETWEventTable"
               )

<#
    Check if OMS Log Analytics is configured tooindex service fabric events from hello specified table
#>

function Check-OMSLogAnalyticsConfiguration {
    param(
    [psobject]$workspace,
    [psobject]$storageAccount,
    [string]$id
    )

    $existingInsights = Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

    if ($existingInsights)
    {
        $currentStorageAccountInsight = $existingInsights.Where({$_.StorageAccountResourceId -eq $storageAccount.ResourceId})

        if ("WADServiceFabric*EventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured tooread service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage tooconfirm there is recent data written by Service Fabric
#>

function Check-TablesForData {
    param(
    [psobject]$storageAccount
    )

    $ctx = (Get-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.ResourceName).Context

    $createdTables = Get-AzureStorageTable -Context $ctx

    $recently = Get-Date -Format s ((Get-Date).AddMinutes(-20).ToUniversalTime())
    $recently = $recently + "Z"

    foreach ($table in $WADtables)
    {
        if ($table -in $createdTables.Name)
        {
            $tbl = Get-AzureStorageTable -Name $table -Context $ctx
            $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery
            $list = New-Object System.Collections.Generic.List[string]
            $list.Add("RowKey")
            $list.Add("ProviderName")
            $list.Add("Timestamp")
            $query.FilterString = "Timestamp gt datetime'$recently'"
            $query.SelectColumns = $list
            $query.TakeCount = 20
            $entities = $tbl.CloudTable.ExecuteQuery($query)
            Write-Debug $entities
            if ($entities.Count -gt 0)
            {
                Write-Verbose ("Data was written too$table in " + $storageAccount.ResourceName + "after $recently")
            } else
            {
                Write-Warning ("No data after $recently is in  $table in " + $storageAccount.ResourceName)
            }
        } else
        {
            Write-Warning ("$table does not exist in storage account " + $storageAccount.ResourceName)
        }
    }
}

<#
    Check if ETW provider is configured toolog events toohello expected table storage
#>
function Check-ETWProviderLogging {
    param(
    [string]$id,
    [string]$provider,
    [string]$expectedTable,
    [string]$table
    )      
        Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
        if ( ($table -eq $null) -or ($table -eq ""))
        {
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
        }
        else
        {
            Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
        }
}

<#
    Check Azure Diagnostics Configuration for a Service Fabric cluster
#>
function Check-ServiceFabricScaleSetDiagnostics {
    param(
    [psobject]$scaleSetDiagnostics
    )

    $storageAccountsFound = @()
    Write-Verbose ("Checking " + $scaleSetDiagnostics)
    $sfReliableActorTable = $null
    $sfReliableServiceTable = $null
    $sfOperationalTable = $null
    Write-Debug $scaleSetDiagnostics
    $serviceFabricProviderList = ""
    $etwManifestProviderList = ""

    if ( $scaleSetDiagnostics.xmlCfg )
    {
        Write-Debug ("Found XMLcfg")
        $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
        Write-Debug $xmlCfg
        $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                
        $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
    } elseif ($scaleSetDiagnostics.WadCfg )
    {
        Write-Debug ("Found WADcfg")
        Write-Debug $scaleSetDiagnostics.WadCfg
        $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
    } else
    {
        Write-Error "Unable tooparse Azure Diagnostics setting for $id"
        Write-Warning ("$id does not have diagnostics enabled")
    }

    foreach ($provider in $serviceFabricProviderList)
    {
        Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
        {
            $sfReliableActorTable = $provider.DefaultEvents.eventDestination
        } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")
        {
            $sfReliableServiceTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }
    foreach ($provider in $etwManifestProviderList)
    {
        Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
        {
            $sfOperationalTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }

    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
    Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

    Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)

    $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
    return ($storageAccountsFound)
}

# This script uses Get-AzureRmVMDiagnosticsExtension and needs a version where -Name is not a required parameter
Import-Module AzureRM.Compute -MinimumVersion 1.2.2

try
{
    Get-AzureRmContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Login-AzureRmAccount
}

$allResources = Get-AzureRmResource

$OMSworkspace = $allResources.Where({($_.ResourceType -eq "Microsoft.OperationalInsights/workspaces") -and ($_.ResourceName -eq $workspaceName)})

if ($OMSworkspace.Name -ne $workspaceName)
{
    Write-Error ("Unable toofind Log Analytics Workspace " + $workspaceName)
}

$serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"})
$storageAccountList = @()
foreach($cluster in $serviceFabricClusters) {
    Write-Verbose ("Checking cluster: " + $cluster.Name)
    $scaleSet = ($allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)}))

    foreach($set in $scaleSet) {
        $resource = Get-AzureRmResource -ResourceId $set.ResourceId
        $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions
        foreach($ext in $extensions) {
            if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
            }
        }
    }
}

$storageAccountList = $storageAccountList | Sort-Object | Get-Unique
$storageAccountsToCheck = ($allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)}))

foreach($storageAccount in $storageAccountsToCheck)
{
    Check-TablesForData $storageAccount
    Check-OMSLogAnalyticsConfiguration $OMSworkspace $storageAccount
}
 ```


## <a name="next-steps"></a><span data-ttu-id="64340-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64340-191">Next steps</span></span>
* <span data-ttu-id="64340-192">Použití [protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) tooview podrobná data události Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64340-192">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
