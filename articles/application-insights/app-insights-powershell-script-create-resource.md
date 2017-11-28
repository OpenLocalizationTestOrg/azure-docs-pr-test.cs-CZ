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
# <a name="powershell-script-toocreate-an-application-insights-resource"></a><span data-ttu-id="e47dd-103">Toocreate skript prostředí PowerShell prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="e47dd-103">PowerShell script toocreate an Application Insights resource</span></span>


<span data-ttu-id="e47dd-104">Když chcete toomonitor novou aplikaci - nebo nová verze aplikace - s [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), můžete nastavit nový prostředek v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e47dd-104">When you want toomonitor a new application - or a new version of an application - with [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), you set up a new resource in Microsoft Azure.</span></span> <span data-ttu-id="e47dd-105">Tento prostředek je, kde je hello telemetrická data z vaší aplikace analyzovat a zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e47dd-105">This resource is where hello telemetry data from your app is analyzed and displayed.</span></span> 

<span data-ttu-id="e47dd-106">Hello vytvoření nového prostředku můžete automatizovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e47dd-106">You can automate hello creation of a new resource by using PowerShell.</span></span>

<span data-ttu-id="e47dd-107">Například pokud vyvíjíte aplikace mobilních zařízení, je pravděpodobné, že, kdykoli bude několik publikované verze aplikace používá vašich zákazníků.</span><span class="sxs-lookup"><span data-stu-id="e47dd-107">For example, if you are developing a mobile device app, it's likely that, at any time, there will be several published versions of your app in use by your customers.</span></span> <span data-ttu-id="e47dd-108">Nechcete tooget hello telemetrie výsledky z různých verzí ve smíšeném.</span><span class="sxs-lookup"><span data-stu-id="e47dd-108">You don't want tooget hello telemetry results from different versions mixed up.</span></span> <span data-ttu-id="e47dd-109">Proto zobrazí vaše toocreate procesu sestavení nový prostředek pro každé sestavení.</span><span class="sxs-lookup"><span data-stu-id="e47dd-109">So you get your build process toocreate a new resource for each build.</span></span>

> [!NOTE]
> <span data-ttu-id="e47dd-110">Pokud chcete toocreate sadu prostředků vše na hello stejný čas, zvažte [vytváření hello prostředků pomocí šablony Azure](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e47dd-110">If you want toocreate a set of resources all at hello same time, consider [creating hello resources using an Azure template](app-insights-powershell.md).</span></span>
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a><span data-ttu-id="e47dd-111">Skript toocreate prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="e47dd-111">Script toocreate an Application Insights resource</span></span>
<span data-ttu-id="e47dd-112">Zobrazit hello příslušné rutiny specifikace:</span><span class="sxs-lookup"><span data-stu-id="e47dd-112">See hello relevant cmdlet specs:</span></span>

* [<span data-ttu-id="e47dd-113">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="e47dd-113">New-AzureRmResource</span></span>](https://msdn.microsoft.com/library/mt652510.aspx)
* [<span data-ttu-id="e47dd-114">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="e47dd-114">New-AzureRmRoleAssignment</span></span>](https://msdn.microsoft.com/library/mt678995.aspx)

<span data-ttu-id="e47dd-115">*Skript prostředí PowerShell*</span><span class="sxs-lookup"><span data-stu-id="e47dd-115">*PowerShell Script*</span></span>  

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

## <a name="what-toodo-with-hello-ikey"></a><span data-ttu-id="e47dd-116">Jaké toodo s hello iKey</span><span class="sxs-lookup"><span data-stu-id="e47dd-116">What toodo with hello iKey</span></span>
<span data-ttu-id="e47dd-117">Každý prostředek, je identifikován svůj klíč instrumentace (iKey).</span><span class="sxs-lookup"><span data-stu-id="e47dd-117">Each resource is identified by its instrumentation key (iKey).</span></span> <span data-ttu-id="e47dd-118">Hello iKey je výstup skriptu pro vytváření prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="e47dd-118">hello iKey is an output of hello resource creation script.</span></span> <span data-ttu-id="e47dd-119">Skript buildu by měl poskytovat hello iKey toohello, které Application Insights SDK vložených do aplikace.</span><span class="sxs-lookup"><span data-stu-id="e47dd-119">Your build script should provide hello iKey toohello Application Insights SDK embedded in your app.</span></span>

<span data-ttu-id="e47dd-120">Existují dva způsoby toomake hello iKey dostupné toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="e47dd-120">There are two ways toomake hello iKey available toohello SDK:</span></span>

* <span data-ttu-id="e47dd-121">V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="e47dd-121">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span> 
  * <span data-ttu-id="e47dd-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span><span class="sxs-lookup"><span data-stu-id="e47dd-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span></span>
* <span data-ttu-id="e47dd-123">Nebo v [inicializace kód](app-insights-api-custom-events-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="e47dd-123">Or in [initialization code](app-insights-api-custom-events-metrics.md):</span></span> 
  * <span data-ttu-id="e47dd-124">`Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span><span class="sxs-lookup"><span data-stu-id="e47dd-124">`Microsoft.ApplicationInsights.Extensibility.
TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span></span>

## <a name="see-also"></a><span data-ttu-id="e47dd-125">Viz také</span><span class="sxs-lookup"><span data-stu-id="e47dd-125">See also</span></span>
* [<span data-ttu-id="e47dd-126">Vytvoření služby Application Insights a web test prostředky ze šablon</span><span class="sxs-lookup"><span data-stu-id="e47dd-126">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="e47dd-127">Nastavení monitorování diagnostiky Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e47dd-127">Set up monitoring of Azure diagnostics with PowerShell</span></span>](app-insights-powershell-azure-diagnostics.md) 
* [<span data-ttu-id="e47dd-128">Nastavit upozornění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e47dd-128">Set alerts by using PowerShell</span></span>](app-insights-powershell-alerts.md)

