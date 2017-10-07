---
title: "aaaPowerShell skriptu toocreate prostředek Application Insights | Microsoft Docs"
description: "Automatizovat vytváření prostředků Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: bwren
ms.openlocfilehash: 2ac00376d38026d64c2c5deabfaca60588924510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-script-toocreate-an-application-insights-resource"></a>Toocreate skript prostředí PowerShell prostředek Application Insights


Když chcete toomonitor novou aplikaci - nebo nová verze aplikace - s [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), můžete nastavit nový prostředek v Microsoft Azure. Tento prostředek je, kde je hello telemetrická data z vaší aplikace analyzovat a zobrazí. 

Hello vytvoření nového prostředku můžete automatizovat pomocí prostředí PowerShell.

Například pokud vyvíjíte aplikace mobilních zařízení, je pravděpodobné, že, kdykoli bude několik publikované verze aplikace používá vašich zákazníků. Nechcete tooget hello telemetrie výsledky z různých verzí ve smíšeném. Proto zobrazí vaše toocreate procesu sestavení nový prostředek pro každé sestavení.

> [!NOTE]
> Pokud chcete toocreate sadu prostředků vše na hello stejný čas, zvažte [vytváření hello prostředků pomocí šablony Azure](app-insights-powershell.md).
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a>Skript toocreate prostředek Application Insights
Zobrazit hello příslušné rutiny specifikace:

* [Nové AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*Skript prostředí PowerShell*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before hello first 
# execution toologin toohello Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set hello name of hello Application Insights Resource

$appInsightsName = "TestApp"

# Set hello application name used for hello value of hello Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set hello name of hello Resource Group toouse.  
# Default is hello application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create hello Resource and Output hello name and iKey
###################################################

# Select hello azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create hello App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access toohello team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-toodo-with-hello-ikey"></a>Jaké toodo s hello iKey
Každý prostředek, je identifikován svůj klíč instrumentace (iKey). Hello iKey je výstup skriptu pro vytváření prostředků hello. Skript buildu by měl poskytovat hello iKey toohello, které Application Insights SDK vložených do aplikace.

Existují dva způsoby toomake hello iKey dostupné toohello SDK:

* V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Nebo v [inicializace kód](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Viz také
* [Vytvoření služby Application Insights a web test prostředky ze šablon](app-insights-powershell.md)
* [Nastavení monitorování diagnostiky Azure pomocí prostředí PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Nastavit upozornění pomocí prostředí PowerShell](app-insights-powershell-alerts.md)

