---
title: "Hodnocení aplikace Service Fabric se analýza protokolů Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Můžete vytvořit řešení Service Fabric v analýzy protokolů pomocí prostředí PowerShell pro vyhodnocení rizik a stavu Service Fabric aplikací, micro-services, uzly a clustery."
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
ms.openlocfilehash: ca86787e344aa5e9e68934dee6e9e83aeb4cc340
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="assess-azure-service-fabric-applications-and-micro-services-with-powershell"></a><span data-ttu-id="8d5bb-103">Vyhodnocení aplikace Azure Service Fabric a micro-services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d5bb-103">Assess Azure Service Fabric applications and micro-services with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8d5bb-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8d5bb-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="8d5bb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d5bb-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>


![Symbol Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="8d5bb-107">Tento článek popisuje, jak používat řešení Service Fabric v analýzy protokolů pro identifikaci a řešení potíží s napříč cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-107">This article describes how to use the Service Fabric solution in Log Analytics to help identify and troubleshoot issues across your Service Fabric cluster.</span></span> <span data-ttu-id="8d5bb-108">Pomáhá můžete zjistit, jak Service Fabric uzly fungují a jak vašim aplikacím a službám micro běží.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-108">It helps you see how your Service Fabric nodes are performing and how your applications and micro-services are running.</span></span>

<span data-ttu-id="8d5bb-109">Service Fabric řešení používá Azure Diagnostics data z virtuálních počítačů služby infrastruktury, shromažďováním těchto dat z Azure WAD tabulek.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-109">The Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="8d5bb-110">Analýzy protokolů potom načte následující události Service Fabric framework:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-110">Log Analytics then reads the following Service Fabric framework events:</span></span>

- <span data-ttu-id="8d5bb-111">**Události spolehlivé služby**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-111">**Reliable Service Events**</span></span>
- <span data-ttu-id="8d5bb-112">**Události objektu actor**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-112">**Actor Events**</span></span>
- <span data-ttu-id="8d5bb-113">**Provozní události**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-113">**Operational Events**</span></span>
- <span data-ttu-id="8d5bb-114">**Vlastní události trasování událostí pro Windows**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-114">**Custom ETW events**</span></span>

<span data-ttu-id="8d5bb-115">Řídicí panel řešení Service Fabric zobrazuje můžete významné problémy a relevantní události ve vašem prostředí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-115">The Service Fabric solution dashboard shows you notable issues and relevant events in your Service Fabric environment.</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="8d5bb-116">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="8d5bb-116">Installing and configuring the solution</span></span>
<span data-ttu-id="8d5bb-117">Postupujte podle těchto tří jednoduché kroky k instalaci a konfiguraci řešení:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-117">Follow these three easy steps to install and configure the solution:</span></span>

1. <span data-ttu-id="8d5bb-118">Přidružte předplatného Azure, které jste použili k vytvoření všechny prostředky clusteru, včetně účtů úložiště s pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-118">Associate the Azure subscription that you used to create all cluster resources, including storage accounts, with your workspace.</span></span> <span data-ttu-id="8d5bb-119">V tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md) informace o vytváření pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-119">See [Get started with Log Analytics](log-analytics-get-started.md) for information about creating a Log Analytics workspace.</span></span>
2. <span data-ttu-id="8d5bb-120">Konfigurace analýzy protokolů můžete shromažďovat a zobrazovat protokoly Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-120">Configure Log Analytics to collect and view Service Fabric logs.</span></span>
3. <span data-ttu-id="8d5bb-121">Povolte řešení Service Fabric v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-121">Enable the Service Fabric solution in your workspace.</span></span>

## <a name="configure-log-analytics-to-collect-and-view-service-fabric-logs"></a><span data-ttu-id="8d5bb-122">Konfigurace analýzy protokolů můžete shromažďovat a zobrazovat protokoly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8d5bb-122">Configure Log Analytics to collect and view Service Fabric logs</span></span>
<span data-ttu-id="8d5bb-123">V této části se dozvíte, postup konfigurace analýzy protokolů pro načtení protokoly Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-123">In this section, you learn how to configure Log Analytics to retrieve Service Fabric logs.</span></span> <span data-ttu-id="8d5bb-124">Protokoly umožňují zobrazit, analyzovat a řešení problémů v clusteru nebo v aplikací a služeb spuštěných v daném clusteru pomocí portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-124">The logs allow you to view, analyze, and troubleshoot issues in your cluster or in the applications and services running in that cluster, using the OMS portal.</span></span>

> [!NOTE]
> <span data-ttu-id="8d5bb-125">Konfigurace rozšíření Azure Diagnostics odeslat protokoly pro úložiště tabulek.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-125">Configure the Azure Diagnostics extension to upload the logs for storage tables.</span></span> <span data-ttu-id="8d5bb-126">Tabulky se musí shodovat co hledá analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-126">The tables must match what Log Analytics looks for.</span></span> <span data-ttu-id="8d5bb-127">Další informace najdete v tématu [postup shromažďování protokolů pomocí Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="8d5bb-127">For more information, see [How to collect logs with Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span></span> <span data-ttu-id="8d5bb-128">Příklady nastavení konfigurace v tomto článku ukazují, že jste názvy tabulek úložiště by měla být.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-128">The configuration settings examples in this article show you what the names of the storage tables should be.</span></span> <span data-ttu-id="8d5bb-129">Jakmile diagnostiky je nastavený na clusteru a odesílá protokoly na účet úložiště, dalším krokem je konfigurace analýzy protokolů pro shromažďování těchto protokolů.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-129">Once Diagnostics is set up on the cluster and is uploading logs to a storage account, the next step is to configure Log Analytics to collect these logs.</span></span>
>
>

<span data-ttu-id="8d5bb-130">Ujistěte se, že aktualizujete **EtwEventSourceProviderConfiguration** tématu **template.json** soubor pro přidání položek pro nové EventSources před použitím konfigurace aktualizovat spuštěním **deploy.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-130">Ensure that you update the **EtwEventSourceProviderConfiguration** section in the **template.json** file to add entries for the new EventSources before you apply the configuration update by running **deploy.ps1**.</span></span> <span data-ttu-id="8d5bb-131">Tabulka pro nahrávání je stejný jako (ETWEventTable).</span><span class="sxs-lookup"><span data-stu-id="8d5bb-131">The table for upload is the same as (ETWEventTable).</span></span> <span data-ttu-id="8d5bb-132">V tuto chvíli můžete číst analýzy protokolů jenom události trasování událostí pro Windows aplikace z *WADETWEventTable* tabulky.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-132">At the moment, Log Analytics can only read application ETW events from the *WADETWEventTable* table.</span></span>

<span data-ttu-id="8d5bb-133">Tyto nástroje jsou používány k provádění některých operací v této části:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-133">The following tools are used to perform some of the operations in this section:</span></span>

* <span data-ttu-id="8d5bb-134">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d5bb-134">Azure PowerShell</span></span>
* [<span data-ttu-id="8d5bb-135">Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="8d5bb-135">Operations Management Suite</span></span>](http://www.microsoft.com/oms)

### <a name="configure-a-log-analytics-workspace-to-show-the-cluster-logs"></a><span data-ttu-id="8d5bb-136">Konfigurovat pracovní prostor analýzy protokolů zobrazit protokoly clusteru</span><span class="sxs-lookup"><span data-stu-id="8d5bb-136">Configure a Log Analytics workspace to show the cluster logs</span></span>

<span data-ttu-id="8d5bb-137">Po vytvoření pracovního prostoru analýzy protokolů, nakonfigurujte v pracovním prostoru protokoly z tabulky úložiště Azure pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-137">After you create a Log Analytics workspace, configure the workspace to pull logs from the Azure storage tables.</span></span> <span data-ttu-id="8d5bb-138">Potom spusťte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-138">Then, run the following PowerShell script:</span></span>

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) to read Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for the subscription to configure.
    If you have more than one Log Analytics workspace you will be prompted for the workspace to configure.
    It will then look through your Service Fabric clusters, and configure your Log Analytics workspace to read Diagnostics from storage accounts that are connected to that cluster and have diagnostics enabled.
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
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"

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
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
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
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
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
         Write-Error "Unable to parse Azure Diagnostics setting for $id"
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
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in the resource
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
                                # HTTP Not Found is returned if the storage insight doesn't exist
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
                                      # If any of the tables from the table list are not already monitored, then we add them
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

<span data-ttu-id="8d5bb-139">Po dokončení konfigurace pracovní prostor analýzy protokolů pro čtení z tabulky Azure ve vašem účtu úložiště, přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-139">After you've configured the Log Analytics workspace to read from the Azure tables in your storage account, log in to the Azure portal.</span></span> <span data-ttu-id="8d5bb-140">Vyberte pracovní prostor analýzy protokolů z **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-140">Select the Log Analytics workspace from **All Resources**.</span></span> <span data-ttu-id="8d5bb-141">Zobrazí se počet připojení do pracovního prostoru protokol účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-141">The number of storage account logs connected to the workspace is displayed.</span></span> <span data-ttu-id="8d5bb-142">Vyberte **protokol účtu úložiště** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-142">Select the **Storage account logs** tile.</span></span> <span data-ttu-id="8d5bb-143">Projděte si seznam protokol účtu úložiště k ověření, že váš účet úložiště je připojený k správné pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-143">Review the list of storage account logs to verify that your storage account is connected to the correct workspace.</span></span>

![Protokol účtu úložiště](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-the-service-fabric-solution"></a><span data-ttu-id="8d5bb-145">Povolit řešení Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8d5bb-145">Enable the Service Fabric solution</span></span>
<span data-ttu-id="8d5bb-146">Pomocí následujícího skriptu přidat do pracovního prostoru analýzy protokolů řešení.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-146">Use the following script to add the solution to your Log Analytics workspace.</span></span> <span data-ttu-id="8d5bb-147">Spusťte skript prostředí PowerShell, pomocí předplatného Azure, který je přidružen pracovní prostor analýzy protokolů, který chcete povolit řešení v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-147">Run the script in PowerShell, using the Azure subscription that is associated with the Log Analytics workspace that you want to enable the Service Fabric solution in.</span></span>

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"
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
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
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

<span data-ttu-id="8d5bb-148">Jakmile povolíte řešení, dlaždice Service Fabric se přidá k Log Analytics *přehled* stránky.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-148">After you enable the solution, the Service Fabric tile is added to your Log Analytics *Overview* page.</span></span> <span data-ttu-id="8d5bb-149">Stránce zobrazuje zobrazení významné problémy, jako je například runAsync selhání a zrušení, k nimž došlo v posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-149">The page shows a view of notable issues such as runAsync failures and cancellations that occurred in the last 24 hours.</span></span>

![Dlaždice Service Fabric](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a><span data-ttu-id="8d5bb-151">Zobrazit události Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8d5bb-151">View Service Fabric events</span></span>
<span data-ttu-id="8d5bb-152">Klikněte **Service Fabric** dlaždici otevřete řídicí panel Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-152">Click the **Service Fabric** tile to open the Service Fabric dashboard.</span></span> <span data-ttu-id="8d5bb-153">Řídicí panel obsahuje sloupce v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-153">The dashboard includes the columns in the table that follows.</span></span> <span data-ttu-id="8d5bb-154">Každý sloupec uvádí top 10 události podle počtu odpovídající kritériím tento sloupec pro zadaný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-154">Each column lists the top 10 events by count matching that column's criteria for the specified time range.</span></span> <span data-ttu-id="8d5bb-155">Můžete spustit hledání protokolu, které poskytuje celý seznam kliknutím **zobrazit všechny** v pravé dolní jednotlivých sloupců, nebo kliknutím na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-155">You can run a log search that provides the entire list by clicking **See all** at the right bottom of each column, or by clicking the column header.</span></span>

| <span data-ttu-id="8d5bb-156">**Události Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-156">**Service Fabric event**</span></span> | <span data-ttu-id="8d5bb-157">**Popis**</span><span class="sxs-lookup"><span data-stu-id="8d5bb-157">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="8d5bb-158">Významné problémy</span><span class="sxs-lookup"><span data-stu-id="8d5bb-158">Notable Issues</span></span> | <span data-ttu-id="8d5bb-159">Zobrazí problémy, včetně RunAsyncFailures, RunAsynCancellations a seznamy uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-159">Displays issues including RunAsyncFailures, RunAsynCancellations, and Node Downs.</span></span> |
| <span data-ttu-id="8d5bb-160">Provozní události</span><span class="sxs-lookup"><span data-stu-id="8d5bb-160">Operational Events</span></span> | <span data-ttu-id="8d5bb-161">Zobrazuje významné provozní události, včetně upgradu aplikace a nasazení.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-161">Displays notable operational events including application upgrade and deployments.</span></span> |
| <span data-ttu-id="8d5bb-162">Události spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="8d5bb-162">Reliable Service Events</span></span> | <span data-ttu-id="8d5bb-163">Zobrazí události významné spolehlivé služby včetně Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-163">Displays notable reliable service events including  Runasyncinvocations.</span></span> |
| <span data-ttu-id="8d5bb-164">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="8d5bb-164">Actor Events</span></span> | <span data-ttu-id="8d5bb-165">Zobrazuje významné objektu actor události vygenerované modulem micro-services.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-165">Displays notable actor events generated by your micro-services.</span></span> <span data-ttu-id="8d5bb-166">Události zahrnují výjimky vyvolané metodu objektu actor, objektu actor aktivací a deaktivací a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-166">Events include exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="8d5bb-167">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="8d5bb-167">Application Events</span></span> | <span data-ttu-id="8d5bb-168">Zobrazí všechny vlastní události trasování událostí generovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-168">Displays all custom ETW events generated by your applications.</span></span> |

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="8d5bb-171">Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-171">The following table shows data collection methods and other details about how data is collected for Service Fabric:</span></span>

| <span data-ttu-id="8d5bb-172">Platforma</span><span class="sxs-lookup"><span data-stu-id="8d5bb-172">platform</span></span> | <span data-ttu-id="8d5bb-173">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="8d5bb-173">Direct Agent</span></span> | <span data-ttu-id="8d5bb-174">Agent nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="8d5bb-174">Operations Manager agent</span></span> | <span data-ttu-id="8d5bb-175">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8d5bb-175">Azure Storage</span></span> | <span data-ttu-id="8d5bb-176">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="8d5bb-176">Operations Manager required?</span></span> | <span data-ttu-id="8d5bb-177">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="8d5bb-177">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="8d5bb-178">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="8d5bb-178">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8d5bb-179">Windows</span><span class="sxs-lookup"><span data-stu-id="8d5bb-179">Windows</span></span> |  |  | <span data-ttu-id="8d5bb-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="8d5bb-180">&#8226;</span></span> |  |  |<span data-ttu-id="8d5bb-181">10 minut</span><span class="sxs-lookup"><span data-stu-id="8d5bb-181">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="8d5bb-182">Změnit rozsah události s **dat podle posledních sedmi dnů** v horní části řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-182">Change the scope of events with **Data based on last seven days** at the top of the dashboard.</span></span> <span data-ttu-id="8d5bb-183">Můžete také zobrazit události generované během posledních sedmi dnů, jeden den nebo šest hodin.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-183">You can also show events generated within the last seven days, one day, or six hours.</span></span> <span data-ttu-id="8d5bb-184">Nebo můžete vybrat **vlastní** zadat vlastní období.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-184">Or, you can select **Custom** to specify a custom date range.</span></span>
>
>

## <a name="troubleshoot-your-service-fabric-and-log-analytics-configuration"></a><span data-ttu-id="8d5bb-185">Řešení potíží s konfiguraci Service Fabric a analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8d5bb-185">Troubleshoot your Service Fabric and Log Analytics configuration</span></span>
<span data-ttu-id="8d5bb-186">Pokud potřebujete ověřit konfiguraci analýzy protokolů, protože nelze zobrazit data události v analýzy protokolů, použijte následující skript.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-186">If you need to verify your Log Analytics configuration because you are unable to view event data in Log Analytics, use the following script.</span></span> <span data-ttu-id="8d5bb-187">Provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="8d5bb-187">It performs the following actions:</span></span>

1. <span data-ttu-id="8d5bb-188">Načte konfiguraci diagnostiky Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8d5bb-188">Reads your Service Fabric diagnostics configuration</span></span>
2. <span data-ttu-id="8d5bb-189">Kontroluje data zapsaná do tabulek</span><span class="sxs-lookup"><span data-stu-id="8d5bb-189">Checks for data written into the tables</span></span>
3. <span data-ttu-id="8d5bb-190">Ověřuje, jestli je ke čtení z tabulky nakonfigurovaná analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8d5bb-190">Verifies that Log Analytics is configured to read from the tables</span></span>

```
<#
    Verify Service Fabric and Log Analytics configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into the tables
    3. Verify Log Analytics is configured to read from the tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    To see items that are correctly configured set $VerbosePreference="Continue"
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
    Check if OMS Log Analytics is configured to index service fabric events from the specified table
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
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured to read service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage to confirm there is recent data written by Service Fabric
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
                Write-Verbose ("Data was written to $table in " + $storageAccount.ResourceName + "after $recently")
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
    Check if ETW provider is configured to log events to the expected table storage
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
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
        }
        else
        {
            Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
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
        Write-Error "Unable to parse Azure Diagnostics setting for $id"
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
    Write-Error ("Unable to find Log Analytics Workspace " + $workspaceName)
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


## <a name="next-steps"></a><span data-ttu-id="8d5bb-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d5bb-191">Next steps</span></span>
* <span data-ttu-id="8d5bb-192">Použití [protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) zobrazíte podrobné data události Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8d5bb-192">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) to view detailed Service Fabric event data.</span></span>
