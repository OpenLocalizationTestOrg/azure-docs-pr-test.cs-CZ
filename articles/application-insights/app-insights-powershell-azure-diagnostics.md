---
title: "Použití prostředí PowerShell k nastavení Application Insights v Azure | Dokumentace Microsoftu"
description: "Automatizujte konfiguraci kanálu Azure Diagnostics pro službu Application Insights."
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
ms.openlocfilehash: 3b6da89cc33cda713b483a2af3cbb493a03d6bec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a><span data-ttu-id="4cd48-103">Použití prostředí PowerShell k nastavení Application Insights pro webovou aplikaci v Azure</span><span class="sxs-lookup"><span data-stu-id="4cd48-103">Using PowerShell to set up Application Insights for an Azure web app</span></span>
<span data-ttu-id="4cd48-104">[Microsoft Azure](https://azure.com) může být [konfigurované k odesílání Azure Diagnostics](app-insights-azure-diagnostics.md) do [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4cd48-104">[Microsoft Azure](https://azure.com) can be [configured to send Azure Diagnostics](app-insights-azure-diagnostics.md) to [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="4cd48-105">Diagnostika se týká cloudových služeb Azure a virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="4cd48-105">The diagnostics relate to Azure Cloud Services and Azure VMs.</span></span> <span data-ttu-id="4cd48-106">Doplňují telemetrii, kterou odesíláte z aplikace pomocí Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="4cd48-106">They complement the telemetry that you send from within the app using the Application Insights SDK.</span></span> <span data-ttu-id="4cd48-107">Jako součást automatizace procesu vytváření nových prostředků v Azure můžete nakonfigurovat diagnostiku pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4cd48-107">As part of automating the process of creating new resources in Azure, you can configure diagnostics using PowerShell.</span></span>

## <a name="azure-template"></a><span data-ttu-id="4cd48-108">Šablony Azure</span><span class="sxs-lookup"><span data-stu-id="4cd48-108">Azure template</span></span>
<span data-ttu-id="4cd48-109">Pokud je webová aplikace v Azure a vy vytvoříte své prostředky pomocí šablony správce prostředků Azure, můžete nakonfigurovat Application Insights přidáním tohoto uzlu prostředků:</span><span class="sxs-lookup"><span data-stu-id="4cd48-109">If the web app is in Azure and you create your resources using an Azure Resource Manager template, you can configure Application Insights by adding this to the resources node:</span></span>

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

* <span data-ttu-id="4cd48-110">`nameOfAIAppResource` – název prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="4cd48-110">`nameOfAIAppResource` - a name for the Application Insights resource</span></span>
* <span data-ttu-id="4cd48-111">`myWebAppName` – ID webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4cd48-111">`myWebAppName` - the id of the web app</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="4cd48-112">Povolit rozšíření diagnostiky jako součást nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="4cd48-112">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="4cd48-113">Rutina `New-AzureDeployment` obsahuje parametr `ExtensionConfiguration`, který přijímá pole konfigurace diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="4cd48-113">The `New-AzureDeployment` cmdlet has a parameter `ExtensionConfiguration`, which takes an array of diagnostics configurations.</span></span> <span data-ttu-id="4cd48-114">Ty lze vytvořit pomocí rutiny `New-AzureServiceDiagnosticsExtensionConfig`.</span><span class="sxs-lookup"><span data-stu-id="4cd48-114">These can be created using the `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span></span> <span data-ttu-id="4cd48-115">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4cd48-115">For example:</span></span>

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

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="4cd48-116">Povolit rozšíření diagnostiky na existující službu Cloud</span><span class="sxs-lookup"><span data-stu-id="4cd48-116">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="4cd48-117">Na existující službu použijte `Set-AzureServiceDiagnosticsExtension`.</span><span class="sxs-lookup"><span data-stu-id="4cd48-117">On an existing service, use `Set-AzureServiceDiagnosticsExtension`.</span></span>

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

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="4cd48-118">Získejte aktuální konfiguraci rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="4cd48-118">Get current diagnostics extension configuration</span></span>
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a><span data-ttu-id="4cd48-119">Odeberte rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="4cd48-119">Remove diagnostics extension</span></span>
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="4cd48-120">Pokud jste povolili rozšíření diagnostiky pomocí `Set-AzureServiceDiagnosticsExtension` nebo `New-AzureServiceDiagnosticsExtensionConfig` bez parametru Role můžete rozšíření odebrat pomocí `Remove-AzureServiceDiagnosticsExtension` bez parametru Role.</span><span class="sxs-lookup"><span data-stu-id="4cd48-120">If you enabled the diagnostics extension using either `Set-AzureServiceDiagnosticsExtension` or `New-AzureServiceDiagnosticsExtensionConfig` without the Role parameter, then you can remove the extension using `Remove-AzureServiceDiagnosticsExtension` without the Role parameter.</span></span> <span data-ttu-id="4cd48-121">Pokud byl použit parametr role při povolování rozšíření, pak musí být rovněž použit při odebírání rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4cd48-121">If the Role parameter was used when enabling the extension then it must also be used when removing the extension.</span></span>

<span data-ttu-id="4cd48-122">Chcete-li odebrat rozšíření diagnostiky pro každou jednotlivou roli:</span><span class="sxs-lookup"><span data-stu-id="4cd48-122">To remove the diagnostics extension from each individual role:</span></span>

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a><span data-ttu-id="4cd48-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="4cd48-123">See also</span></span>
* [<span data-ttu-id="4cd48-124">Monitorování aplikací v Azure Cloud Services službou Application Insights</span><span class="sxs-lookup"><span data-stu-id="4cd48-124">Monitor Azure Cloud Services apps with Application Insights</span></span>](app-insights-cloudservices.md)
* [<span data-ttu-id="4cd48-125">Odesílání Diagnostiky Azure do Application Insights</span><span class="sxs-lookup"><span data-stu-id="4cd48-125">Send Azure Diagnostics to Application Insights</span></span>](app-insights-azure-diagnostics.md)
* [<span data-ttu-id="4cd48-126">Automatizace konfigurace výstrah</span><span class="sxs-lookup"><span data-stu-id="4cd48-126">Automate configuring alerts</span></span>](app-insights-powershell-alerts.md)

