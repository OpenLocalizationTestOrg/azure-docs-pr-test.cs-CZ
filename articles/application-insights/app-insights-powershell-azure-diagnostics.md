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
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a>Pomocí prostředí PowerShell tooset až Application Insights pro webové aplikace Azure
[Microsoft Azure](https://azure.com) může být [nakonfigurované toosend Azure Diagnostics](app-insights-azure-diagnostics.md) příliš[Azure Application Insights](app-insights-overview.md). Hello Diagnostika se týká tooAzure cloudových služeb a virtuálních počítačích Azure. Doplňují telemetrii hello, kterou odesíláte z aplikace hello pomocí hello Application Insights SDK. Jako součást automatizace procesu hello vytváření nových prostředků v Azure, můžete nakonfigurovat diagnostiku pomocí prostředí PowerShell.

## <a name="azure-template"></a>Šablony Azure
Pokud webová aplikace hello je v Azure a vy vytvoříte své prostředky pomocí šablony Azure Resource Manager, můžete nakonfigurovat Application Insights přidáním tohoto uzlu prostředků toohello:

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

* `nameOfAIAppResource`-Název hello prostředek Application Insights
* `myWebAppName`-id hello hello webové aplikace

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Povolit rozšíření diagnostiky jako součást nasazení cloudové služby
Hello `New-AzureDeployment` rutina má parametr `ExtensionConfiguration`, který přijímá pole Konfigurace diagnostiky. Ty lze vytvořit pomocí hello `New-AzureServiceDiagnosticsExtensionConfig` rutiny. Například:

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

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Povolit rozšíření diagnostiky na existující službu Cloud
Na existující službu použijte `Set-AzureServiceDiagnosticsExtension`.

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

## <a name="get-current-diagnostics-extension-configuration"></a>Získejte aktuální konfiguraci rozšíření diagnostiky
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Odeberte rozšíření diagnostiky
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Pokud jste povolili rozšíření diagnostiky hello pomocí `Set-AzureServiceDiagnosticsExtension` nebo `New-AzureServiceDiagnosticsExtensionConfig` bez parametru Role hello, můžete odebrat pomocí rozšíření hello `Remove-AzureServiceDiagnosticsExtension` bez parametru Role hello. Pokud byl použit parametr Role hello při povolování rozšíření hello pak musíte také ho použít při odebírání rozšíření hello.

rozšíření diagnostiky tooremove hello každou jednotlivou roli:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Viz také
* [Monitorování aplikací v Azure Cloud Services službou Application Insights](app-insights-cloudservices.md)
* [Odesílání Azure Diagnostics tooApplication statistiky](app-insights-azure-diagnostics.md)
* [Automatizace konfigurace výstrah](app-insights-powershell-alerts.md)

