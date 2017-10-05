---
title: "Skript prostředí PowerShell pro vytvoření prostředek Application Insights | Microsoft Docs"
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
ms.openlocfilehash: a828af9c7d207dd84cc626fc70206018fd67e2dd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a><span data-ttu-id="5fa4a-103">Rutina PowerShell pro vytvoření prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="5fa4a-103">PowerShell script to create an Application Insights resource</span></span>


<span data-ttu-id="5fa4a-104">Pokud chcete monitorování nové aplikace - nebo nová verze aplikace - s [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), můžete nastavit nový prostředek v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-104">When you want to monitor a new application - or a new version of an application - with [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), you set up a new resource in Microsoft Azure.</span></span> <span data-ttu-id="5fa4a-105">Tento prostředek je, kde analyzovat a zobrazí data telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-105">This resource is where the telemetry data from your app is analyzed and displayed.</span></span> 

<span data-ttu-id="5fa4a-106">Vytvoření nového prostředku můžete automatizovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-106">You can automate the creation of a new resource by using PowerShell.</span></span>

<span data-ttu-id="5fa4a-107">Například pokud vyvíjíte aplikace mobilních zařízení, je pravděpodobné, že, kdykoli bude několik publikované verze aplikace používá vašich zákazníků.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-107">For example, if you are developing a mobile device app, it's likely that, at any time, there will be several published versions of your app in use by your customers.</span></span> <span data-ttu-id="5fa4a-108">Nechcete získat výsledky telemetrická data z různých verzí ve smíšeném.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-108">You don't want to get the telemetry results from different versions mixed up.</span></span> <span data-ttu-id="5fa4a-109">Získáte tak vaše sestavení postup vytvoření nového prostředku pro každé sestavení.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-109">So you get your build process to create a new resource for each build.</span></span>

> [!NOTE]
> <span data-ttu-id="5fa4a-110">Pokud chcete vytvořit sadu prostředků všechny najednou, zvažte [vytváření prostředků pomocí šablony Azure](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5fa4a-110">If you want to create a set of resources all at the same time, consider [creating the resources using an Azure template](app-insights-powershell.md).</span></span>
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a><span data-ttu-id="5fa4a-111">Skript pro vytvoření prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="5fa4a-111">Script to create an Application Insights resource</span></span>
<span data-ttu-id="5fa4a-112">Zobrazit specifikace příslušné rutiny:</span><span class="sxs-lookup"><span data-stu-id="5fa4a-112">See the relevant cmdlet specs:</span></span>

* [<span data-ttu-id="5fa4a-113">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="5fa4a-113">New-AzureRmResource</span></span>](https://msdn.microsoft.com/library/mt652510.aspx)
* [<span data-ttu-id="5fa4a-114">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="5fa4a-114">New-AzureRmRoleAssignment</span></span>](https://msdn.microsoft.com/library/mt678995.aspx)

<span data-ttu-id="5fa4a-115">*Skript prostředí PowerShell*</span><span class="sxs-lookup"><span data-stu-id="5fa4a-115">*PowerShell Script*</span></span>  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a><span data-ttu-id="5fa4a-116">Co dělat s iKey</span><span class="sxs-lookup"><span data-stu-id="5fa4a-116">What to do with the iKey</span></span>
<span data-ttu-id="5fa4a-117">Každý prostředek, je identifikován svůj klíč instrumentace (iKey).</span><span class="sxs-lookup"><span data-stu-id="5fa4a-117">Each resource is identified by its instrumentation key (iKey).</span></span> <span data-ttu-id="5fa4a-118">IKey je výstup skriptu pro vytváření prostředků.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-118">The iKey is an output of the resource creation script.</span></span> <span data-ttu-id="5fa4a-119">Skript buildu by měl poskytovat iKey do Application Insights SDK vloženému ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5fa4a-119">Your build script should provide the iKey to the Application Insights SDK embedded in your app.</span></span>

<span data-ttu-id="5fa4a-120">Existují dva způsoby, jak zpřístupnit iKey k sadě SDK:</span><span class="sxs-lookup"><span data-stu-id="5fa4a-120">There are two ways to make the iKey available to the SDK:</span></span>

* <span data-ttu-id="5fa4a-121">V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="5fa4a-121">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span> 
  * <span data-ttu-id="5fa4a-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span><span class="sxs-lookup"><span data-stu-id="5fa4a-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span></span>
* <span data-ttu-id="5fa4a-123">Nebo v [inicializace kód](app-insights-api-custom-events-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="5fa4a-123">Or in [initialization code](app-insights-api-custom-events-metrics.md):</span></span> 
  * <span data-ttu-id="5fa4a-124">`Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span><span class="sxs-lookup"><span data-stu-id="5fa4a-124">`Microsoft.ApplicationInsights.Extensibility.
TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span></span>

## <a name="see-also"></a><span data-ttu-id="5fa4a-125">Viz také</span><span class="sxs-lookup"><span data-stu-id="5fa4a-125">See also</span></span>
* [<span data-ttu-id="5fa4a-126">Vytvoření služby Application Insights a web test prostředky ze šablon</span><span class="sxs-lookup"><span data-stu-id="5fa4a-126">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="5fa4a-127">Nastavení monitorování diagnostiky Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5fa4a-127">Set up monitoring of Azure diagnostics with PowerShell</span></span>](app-insights-powershell-azure-diagnostics.md) 
* [<span data-ttu-id="5fa4a-128">Nastavit upozornění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5fa4a-128">Set alerts by using PowerShell</span></span>](app-insights-powershell-alerts.md)

