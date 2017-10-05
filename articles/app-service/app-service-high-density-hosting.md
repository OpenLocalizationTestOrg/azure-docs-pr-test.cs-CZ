---
title: "S vysokou hustotou hostování na Azure App Service | Microsoft Docs"
description: "S vysokou hustotou hostování na Azure App Service"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: byvinyal
ms.openlocfilehash: 459a310a719695f6366470976d857ec2f9d6f4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="8eba7-103">S vysokou hustotou hostování na Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8eba7-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="8eba7-104">Při používání služby App Service, odpojené od kapacitu přidělených dvěma konceptů aplikace:</span><span class="sxs-lookup"><span data-stu-id="8eba7-104">When using App Service, your application is decoupled from the capacity allocated to it by two concepts:</span></span>

* <span data-ttu-id="8eba7-105">**Aplikace:** představuje aplikace a jeho konfigurace modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="8eba7-105">**The Application:** Represents the app and its runtime configuration.</span></span> <span data-ttu-id="8eba7-106">Například obsahuje verze rozhraní .NET, která by se měly načíst modul runtime, nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eba7-106">For example, it includes the version of .NET that the runtime should load, the app settings.</span></span>
* <span data-ttu-id="8eba7-107">**Plán služby App Service:** definuje vlastnosti kapacitu, sada k dispozici funkcí a polohu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eba7-107">**The App Service Plan:** Defines the characteristics of the capacity, available feature set, and locality of the application.</span></span> <span data-ttu-id="8eba7-108">Charakteristiky může být například velký počítač (4 jádra), čtyři instancí, prémiové funkce v oblasti Východ USA.</span><span class="sxs-lookup"><span data-stu-id="8eba7-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="8eba7-109">Aplikace je vždy spojen plán služby App Service, ale plán služby App Service může poskytnout dostatečnou kapacitu pro jednu nebo více aplikací.</span><span class="sxs-lookup"><span data-stu-id="8eba7-109">An app is always linked to an App Service plan, but an App Service plan can provide capacity to one or more apps.</span></span>

<span data-ttu-id="8eba7-110">V důsledku toho platformou poskytuje možnost izolovat aplikace na jeden nebo více aplikacemi sdílet prostředky sdílením plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8eba7-110">As a result, the platform provides the flexibility to isolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="8eba7-111">Ale při více aplikací sdílet plán služby App Service, instance této aplikace běží na všechny instance tohoto plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8eba7-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="8eba7-112">Pokud na škálování aplikace</span><span class="sxs-lookup"><span data-stu-id="8eba7-112">Per app scaling</span></span>
<span data-ttu-id="8eba7-113">*Pokud na škálování aplikace* je funkce, která může být povolena na úrovni plán služby App Service a pak se použije na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eba7-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="8eba7-114">Jednotlivé aplikace škáluje škálování aplikace nezávisle plán služby App Service, který je hostitelem ho.</span><span class="sxs-lookup"><span data-stu-id="8eba7-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="8eba7-115">Tímto způsobem plán služby App Service můžete škálovat na 10 instancí, ale aplikace může být nastaven na použití pouze pěti.</span><span class="sxs-lookup"><span data-stu-id="8eba7-115">This way, an App Service plan can be scaled to 10 instances, but an app can be set to use only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="8eba7-116">Jednotlivé aplikace škálování je dostupná jenom pro **Premium** plánů služby App Service SKU</span><span class="sxs-lookup"><span data-stu-id="8eba7-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="8eba7-117">Jednotlivé aplikace příjmu pomocí Powershellu</span><span class="sxs-lookup"><span data-stu-id="8eba7-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="8eba7-118">Můžete vytvořit plán nakonfigurovaný jako *za škálování aplikace* plán předáním v ```-perSiteScaling $true``` atribut ```New-AzureRmAppServicePlan``` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8eba7-118">You can create a plan configured as a *Per app scaling* plan by passing in the ```-perSiteScaling $true``` attribute to the ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="8eba7-119">Pokud chcete aktualizovat existující plán služby App Service k použití této funkce:</span><span class="sxs-lookup"><span data-stu-id="8eba7-119">If you want to update an existing App Service plan to use this feature:</span></span> 

- <span data-ttu-id="8eba7-120">získat plán cíl```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="8eba7-120">get the target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="8eba7-121">Úprava vlastností místně```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="8eba7-121">modifying the property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="8eba7-122">uložení změn zpět do azure```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="8eba7-122">posting your changes back to azure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="8eba7-123">Na úrovni aplikace je potřeba nakonfigurovat počet instancí, které aplikace můžete používat v plánu služby app service.</span><span class="sxs-lookup"><span data-stu-id="8eba7-123">At the app level, we need to configure the number of instances the app can use in the app service plan.</span></span>

<span data-ttu-id="8eba7-124">V následujícím příkladu je omezený na dvě instance bez ohledu na to, kolik instancí základní plán služby app service horizontálně navýší kapacitu pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eba7-124">In the example below, the app is limited to two instances regardless of how many instances the underlying app service plan scales out to.</span></span>

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="8eba7-125">$newapp. SiteConfig.NumberOfWorkers je různá $newapp. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="8eba7-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="8eba7-126">Jednotlivé aplikace používá škálování $newapp. SiteConfig.NumberOfWorkers k určení charakteristik škálování aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eba7-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers to determine the scale characteristics of the app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="8eba7-127">Pokud na škálování aplikace pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8eba7-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="8eba7-128">Následující *šablony Azure Resource Manageru* vytvoří:</span><span class="sxs-lookup"><span data-stu-id="8eba7-128">The following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="8eba7-129">Plán služby App Service, která je škálovat na 10 instancí</span><span class="sxs-lookup"><span data-stu-id="8eba7-129">An App Service plan that's scaled out to 10 instances</span></span>
- <span data-ttu-id="8eba7-130">aplikace, který je nakonfigurovaný škálování na maximální pět instancí.</span><span class="sxs-lookup"><span data-stu-id="8eba7-130">an app that's configured to scale to a max of five instances.</span></span>

<span data-ttu-id="8eba7-131">Plán služby App Service se nastaví **PerSiteScaling** vlastnost na hodnotu true ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="8eba7-131">The App Service plan is setting the **PerSiteScaling** property to true ```"perSiteScaling": true```.</span></span> <span data-ttu-id="8eba7-132">Aplikace se nastaví **počet pracovních procesů** sloužící k 5 ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="8eba7-132">The app is setting the **number of workers** to use to 5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="8eba7-133">Doporučenou konfiguraci pro hostování s vysokou hustotou</span><span class="sxs-lookup"><span data-stu-id="8eba7-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="8eba7-134">Za škálování aplikace je funkce, která je povolena v globální oblastí Azure a prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8eba7-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="8eba7-135">Doporučená strategie je však používat prostředí App Service k využívat jejich pokročilých funkcí a větší fondy kapacity.</span><span class="sxs-lookup"><span data-stu-id="8eba7-135">However, the recommended strategy is to use App Service Environments to take advantage of their advanced features and the larger pools of capacity.</span></span>  

<span data-ttu-id="8eba7-136">Postupujte podle těchto kroků nakonfigurujete vysokou hustotou hostování pro vaše aplikace:</span><span class="sxs-lookup"><span data-stu-id="8eba7-136">Follow these steps to configure high density hosting for your apps:</span></span>

1. <span data-ttu-id="8eba7-137">Konfigurace služby App Service Environment a vyberte fond pracovních procesů, který je vyhrazen pro hostování scénáři s vysokou hustotou.</span><span class="sxs-lookup"><span data-stu-id="8eba7-137">Configure the App Service Environment and choose a worker pool that is dedicated to the high density hosting scenario.</span></span>
1. <span data-ttu-id="8eba7-138">Vytvoření jednoho plánu služby App Service a škálujte ji používat všechny dostupné kapacity ve fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="8eba7-138">Create a single App Service plan, and scale it to use all the available capacity on the worker pool.</span></span>
1. <span data-ttu-id="8eba7-139">Nastavte příznak PerSiteScaling plán aplikační služby na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="8eba7-139">Set the PerSiteScaling flag to true on the App Service plan.</span></span>
1. <span data-ttu-id="8eba7-140">Nové aplikace jsou vytvořeny a přiřazené plán služby App Service pomocí **numberOfWorkers** vlastnost nastavena na hodnotu **1**.</span><span class="sxs-lookup"><span data-stu-id="8eba7-140">New apps are created and assigned to that App Service plan with the **numberOfWorkers** property set to **1**.</span></span> <span data-ttu-id="8eba7-141">Pomocí této konfigurace poskytuje nejvyšší hustotou možné u tohoto fondu pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="8eba7-141">Using this configuration yields the highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="8eba7-142">Počet pracovních procesů může být nakonfigurováno nezávisle na aplikaci podle potřeby udělovat další prostředky.</span><span class="sxs-lookup"><span data-stu-id="8eba7-142">The number of workers can be configured independently per app to grant additional resources as needed.</span></span> <span data-ttu-id="8eba7-143">Například:</span><span class="sxs-lookup"><span data-stu-id="8eba7-143">For example:</span></span>
    - <span data-ttu-id="8eba7-144">Můžete nastavit aplikaci intenzivně využívaných **numberOfWorkers** k **3** tak, aby měl větší kapacitu zpracování pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eba7-144">A high-use app can set **numberOfWorkers** to **3** to have more processing capacity for that app.</span></span> 
    - <span data-ttu-id="8eba7-145">Nízká použití aplikace se nastavuje **numberOfWorkers** k **1**.</span><span class="sxs-lookup"><span data-stu-id="8eba7-145">Low-use apps would set **numberOfWorkers** to **1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8eba7-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8eba7-146">Next Steps</span></span>

- [<span data-ttu-id="8eba7-147">Podrobný přehled plánů služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8eba7-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="8eba7-148">Úvod do prostředí App Service</span><span class="sxs-lookup"><span data-stu-id="8eba7-148">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)