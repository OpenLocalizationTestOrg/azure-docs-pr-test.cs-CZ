---
title: "prostředí PowerShell toosetup aaaUsing Application Insights v Azure | Microsoft Docs"
description: Automatizovat konfiguraci Azure Diagnostics toopipe tooApplication statistiky.
services: application-insights
documentationcenter: .net
author: sbtron
manager: carmonm
ms.assetid: 4ac803a8-f424-4c0c-b18f-4b9c189a64a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/17/2015
ms.author: bwren
ms.openlocfilehash: c48a5d8eb23df162522860935af876063aaa6976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a><span data-ttu-id="0e587-103">Pomocí prostředí PowerShell tooset až Application Insights pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="0e587-103">Using PowerShell tooset up Application Insights for an Azure web app</span></span>
<span data-ttu-id="0e587-104">[Microsoft Azure](https://azure.com) může být [nakonfigurované toosend Azure Diagnostics](app-insights-azure-diagnostics.md) příliš[Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0e587-104">[Microsoft Azure](https://azure.com) can be [configured toosend Azure Diagnostics](app-insights-azure-diagnostics.md) too[Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="0e587-105">Hello Diagnostika se týká tooAzure cloudových služeb a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="0e587-105">hello diagnostics relate tooAzure Cloud Services and Azure VMs.</span></span> <span data-ttu-id="0e587-106">Doplňují telemetrii hello, kterou odesíláte z aplikace hello pomocí hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="0e587-106">They complement hello telemetry that you send from within hello app using hello Application Insights SDK.</span></span> <span data-ttu-id="0e587-107">Jako součást automatizace procesu hello vytváření nových prostředků v Azure, můžete nakonfigurovat diagnostiku pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0e587-107">As part of automating hello process of creating new resources in Azure, you can configure diagnostics using PowerShell.</span></span>

## <a name="azure-template"></a><span data-ttu-id="0e587-108">Šablony Azure</span><span class="sxs-lookup"><span data-stu-id="0e587-108">Azure template</span></span>
<span data-ttu-id="0e587-109">Pokud webová aplikace hello je v Azure a vy vytvoříte své prostředky pomocí šablony Azure Resource Manager, můžete nakonfigurovat Application Insights přidáním tohoto uzlu prostředků toohello:</span><span class="sxs-lookup"><span data-stu-id="0e587-109">If hello web app is in Azure and you create your resources using an Azure Resource Manager template, you can configure Application Insights by adding this toohello resources node:</span></span>

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* <span data-ttu-id="0e587-110">`nameOfAIAppResource`-Název hello prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="0e587-110">`nameOfAIAppResource` - a name for hello Application Insights resource</span></span>
* <span data-ttu-id="0e587-111">`myWebAppName`-id hello hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0e587-111">`myWebAppName` - hello id of hello web app</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="0e587-112">Povolit rozšíření diagnostiky jako součást nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="0e587-112">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="0e587-113">Hello `New-AzureDeployment` rutina má parametr `ExtensionConfiguration`, který přijímá pole Konfigurace diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="0e587-113">hello `New-AzureDeployment` cmdlet has a parameter `ExtensionConfiguration`, which takes an array of diagnostics configurations.</span></span> <span data-ttu-id="0e587-114">Ty lze vytvořit pomocí hello `New-AzureServiceDiagnosticsExtensionConfig` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0e587-114">These can be created using hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span></span> <span data-ttu-id="0e587-115">Například:</span><span class="sxs-lookup"><span data-stu-id="0e587-115">For example:</span></span>

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="0e587-116">Povolit rozšíření diagnostiky na existující službu Cloud</span><span class="sxs-lookup"><span data-stu-id="0e587-116">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="0e587-117">Na existující službu použijte `Set-AzureServiceDiagnosticsExtension`.</span><span class="sxs-lookup"><span data-stu-id="0e587-117">On an existing service, use `Set-AzureServiceDiagnosticsExtension`.</span></span>

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="0e587-118">Získejte aktuální konfiguraci rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0e587-118">Get current diagnostics extension configuration</span></span>
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a><span data-ttu-id="0e587-119">Odeberte rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0e587-119">Remove diagnostics extension</span></span>
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="0e587-120">Pokud jste povolili rozšíření diagnostiky hello pomocí `Set-AzureServiceDiagnosticsExtension` nebo `New-AzureServiceDiagnosticsExtensionConfig` bez parametru Role hello, můžete odebrat pomocí rozšíření hello `Remove-AzureServiceDiagnosticsExtension` bez parametru Role hello.</span><span class="sxs-lookup"><span data-stu-id="0e587-120">If you enabled hello diagnostics extension using either `Set-AzureServiceDiagnosticsExtension` or `New-AzureServiceDiagnosticsExtensionConfig` without hello Role parameter, then you can remove hello extension using `Remove-AzureServiceDiagnosticsExtension` without hello Role parameter.</span></span> <span data-ttu-id="0e587-121">Pokud byl použit parametr Role hello při povolování rozšíření hello pak musíte také ho použít při odebírání rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="0e587-121">If hello Role parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="0e587-122">rozšíření diagnostiky tooremove hello každou jednotlivou roli:</span><span class="sxs-lookup"><span data-stu-id="0e587-122">tooremove hello diagnostics extension from each individual role:</span></span>

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a><span data-ttu-id="0e587-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="0e587-123">See also</span></span>
* [<span data-ttu-id="0e587-124">Monitorování aplikací v Azure Cloud Services službou Application Insights</span><span class="sxs-lookup"><span data-stu-id="0e587-124">Monitor Azure Cloud Services apps with Application Insights</span></span>](app-insights-cloudservices.md)
* [<span data-ttu-id="0e587-125">Odesílání Azure Diagnostics tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="0e587-125">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-azure-diagnostics.md)
* [<span data-ttu-id="0e587-126">Automatizace konfigurace výstrah</span><span class="sxs-lookup"><span data-stu-id="0e587-126">Automate configuring alerts</span></span>](app-insights-powershell-alerts.md)

