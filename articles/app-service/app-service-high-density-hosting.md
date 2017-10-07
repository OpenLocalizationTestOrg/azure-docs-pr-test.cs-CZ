---
title: "aaaHigh hustotu hostování na Azure App Service | Microsoft Docs"
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
ms.openlocfilehash: a10cb81ace13ba6992b572a44361061ecf72b266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="81a49-103">S vysokou hustotou hostování na Azure App Service</span><span class="sxs-lookup"><span data-stu-id="81a49-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="81a49-104">Při používání služby App Service, odpojené od hello kapacity přidělené tooit dvěma konceptů aplikace:</span><span class="sxs-lookup"><span data-stu-id="81a49-104">When using App Service, your application is decoupled from hello capacity allocated tooit by two concepts:</span></span>

* <span data-ttu-id="81a49-105">**Hello aplikace:** představuje hello aplikace a jeho konfigurace modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="81a49-105">**hello Application:** Represents hello app and its runtime configuration.</span></span> <span data-ttu-id="81a49-106">Například obsahuje hello by se měly načíst verzi rozhraní .NET, která hello runtime, nastavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="81a49-106">For example, it includes hello version of .NET that hello runtime should load, hello app settings.</span></span>
* <span data-ttu-id="81a49-107">**Plán služby App Service Hello:** definuje vlastnosti hello hello kapacitu, sada k dispozici funkcí a polohu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="81a49-107">**hello App Service Plan:** Defines hello characteristics of hello capacity, available feature set, and locality of hello application.</span></span> <span data-ttu-id="81a49-108">Charakteristiky může být například velký počítač (4 jádra), čtyři instancí, prémiové funkce v oblasti Východ USA.</span><span class="sxs-lookup"><span data-stu-id="81a49-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="81a49-109">Aplikace je vždy propojené tooan plán služby App Service, ale plán služby App Service můžete poskytovat tooone kapacity nebo další aplikace.</span><span class="sxs-lookup"><span data-stu-id="81a49-109">An app is always linked tooan App Service plan, but an App Service plan can provide capacity tooone or more apps.</span></span>

<span data-ttu-id="81a49-110">V důsledku toho hello platforma poskytuje flexibilitu tooisolate hello jenom jedna aplikace nebo mít víc aplikací sdílení prostředků ve sdílení plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="81a49-110">As a result, hello platform provides hello flexibility tooisolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="81a49-111">Ale při více aplikací sdílet plán služby App Service, instance této aplikace běží na všechny instance tohoto plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="81a49-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="81a49-112">Pokud na škálování aplikace</span><span class="sxs-lookup"><span data-stu-id="81a49-112">Per app scaling</span></span>
<span data-ttu-id="81a49-113">*Pokud na škálování aplikace* je funkce, která může být povolena na úrovni plán služby App Service a pak se použije na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81a49-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="81a49-114">Jednotlivé aplikace škáluje škálování aplikace nezávisle plán služby App Service, který je hostitelem ho.</span><span class="sxs-lookup"><span data-stu-id="81a49-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="81a49-115">Tímto způsobem aplikační službu plán lze škálovat instance too10, ale aplikace lze nastavit pouze pět toouse.</span><span class="sxs-lookup"><span data-stu-id="81a49-115">This way, an App Service plan can be scaled too10 instances, but an app can be set toouse only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="81a49-116">Jednotlivé aplikace škálování je dostupná jenom pro **Premium** plánů služby App Service SKU</span><span class="sxs-lookup"><span data-stu-id="81a49-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="81a49-117">Jednotlivé aplikace příjmu pomocí Powershellu</span><span class="sxs-lookup"><span data-stu-id="81a49-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="81a49-118">Můžete vytvořit plán nakonfigurovaný jako *za škálování aplikace* plán předáním v hello ```-perSiteScaling $true``` atribut toohello ```New-AzureRmAppServicePlan``` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="81a49-118">You can create a plan configured as a *Per app scaling* plan by passing in hello ```-perSiteScaling $true``` attribute toohello ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="81a49-119">Pokud chcete, aby tooupdate stávající službu App Service plánování toouse této funkce:</span><span class="sxs-lookup"><span data-stu-id="81a49-119">If you want tooupdate an existing App Service plan toouse this feature:</span></span> 

- <span data-ttu-id="81a49-120">získat hello cílový plán```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="81a49-120">get hello target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="81a49-121">Úprava vlastností hello místně```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="81a49-121">modifying hello property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="81a49-122">publikování vaši tooazure zpět změny```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="81a49-122">posting your changes back tooazure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get hello new App Service Plan and modify hello "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify hello local copy toouse "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back tooazure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="81a49-123">Na úrovni aplikace hello potřebujeme tooconfigure hello počet instancí, které aplikace hello můžete používat v hello plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="81a49-123">At hello app level, we need tooconfigure hello number of instances hello app can use in hello app service plan.</span></span>

<span data-ttu-id="81a49-124">V níže uvedeném příkladu hello aplikace hello je omezená tootwo instance bez ohledu na to, kolik instancí hello základní aplikace služby plán měřítka se k.</span><span class="sxs-lookup"><span data-stu-id="81a49-124">In hello example below, hello app is limited tootwo instances regardless of how many instances hello underlying app service plan scales out to.</span></span>

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="81a49-125">$newapp. SiteConfig.NumberOfWorkers je různá $newapp. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="81a49-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="81a49-126">Jednotlivé aplikace používá škálování $newapp. SiteConfig.NumberOfWorkers toodetermine hello škálování charakteristiky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="81a49-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers toodetermine hello scale characteristics of hello app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="81a49-127">Pokud na škálování aplikace pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="81a49-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="81a49-128">Následující Hello *šablony Azure Resource Manageru* vytvoří:</span><span class="sxs-lookup"><span data-stu-id="81a49-128">hello following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="81a49-129">Plán služby App Service, která je škálovat na více systémů too10 instancí</span><span class="sxs-lookup"><span data-stu-id="81a49-129">An App Service plan that's scaled out too10 instances</span></span>
- <span data-ttu-id="81a49-130">aplikace, který byl nakonfigurován tooscale tooa maximálně pět instancí.</span><span class="sxs-lookup"><span data-stu-id="81a49-130">an app that's configured tooscale tooa max of five instances.</span></span>

<span data-ttu-id="81a49-131">Hello plán služby App Service je nastavení hello **PerSiteScaling** tootrue vlastnost ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="81a49-131">hello App Service plan is setting hello **PerSiteScaling** property tootrue ```"perSiteScaling": true```.</span></span> <span data-ttu-id="81a49-132">aplikace Hello je nastavení hello **počet pracovních procesů** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="81a49-132">hello app is setting hello **number of workers** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

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

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="81a49-133">Doporučenou konfiguraci pro hostování s vysokou hustotou</span><span class="sxs-lookup"><span data-stu-id="81a49-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="81a49-134">Za škálování aplikace je funkce, která je povolena v globální oblastí Azure a prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="81a49-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="81a49-135">Ale hello doporučoval strategie je použití prostředí App Service tootake výhod jejich pokročilých funkcí a větší fondy hello kapacity.</span><span class="sxs-lookup"><span data-stu-id="81a49-135">However, hello recommended strategy is to use App Service Environments tootake advantage of their advanced features and hello larger pools of capacity.</span></span>  

<span data-ttu-id="81a49-136">Postupujte podle těchto kroků tooconfigure s vysokou hustotou hostování pro vaše aplikace:</span><span class="sxs-lookup"><span data-stu-id="81a49-136">Follow these steps tooconfigure high density hosting for your apps:</span></span>

1. <span data-ttu-id="81a49-137">Nakonfigurujte hello App Service Environment a vyberte fond pracovních procesů, který je vyhrazený toohello hustotou hostování scénář.</span><span class="sxs-lookup"><span data-stu-id="81a49-137">Configure hello App Service Environment and choose a worker pool that is dedicated toohello high density hosting scenario.</span></span>
1. <span data-ttu-id="81a49-138">Vytvoření jednoho plánu služby App Service a škálování ho toouse všechny hello dostupné kapacity ve fondu pracovních procesů hello.</span><span class="sxs-lookup"><span data-stu-id="81a49-138">Create a single App Service plan, and scale it toouse all hello available capacity on hello worker pool.</span></span>
1. <span data-ttu-id="81a49-139">Nastavte hello PerSiteScaling příznak tootrue hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="81a49-139">Set hello PerSiteScaling flag tootrue on hello App Service plan.</span></span>
1. <span data-ttu-id="81a49-140">Nové aplikace jsou vytvořeny a přiřazeny toothat plán služby App Service pomocí **numberOfWorkers** vlastností nastavenou příliš**1**.</span><span class="sxs-lookup"><span data-stu-id="81a49-140">New apps are created and assigned toothat App Service plan with the **numberOfWorkers** property set too**1**.</span></span> <span data-ttu-id="81a49-141">Pomocí této konfigurace vypočítá hello nejvyšší hustotou možné u tohoto fondu pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="81a49-141">Using this configuration yields hello highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="81a49-142">Hello počet pracovních procesů lze nakonfigurovat nezávisle na aplikaci toogrant další prostředky podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="81a49-142">hello number of workers can be configured independently per app toogrant additional resources as needed.</span></span> <span data-ttu-id="81a49-143">Například:</span><span class="sxs-lookup"><span data-stu-id="81a49-143">For example:</span></span>
    - <span data-ttu-id="81a49-144">Můžete nastavit aplikaci intenzivně využívaných **numberOfWorkers** příliš**3** toohave další zpracování kapacity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81a49-144">A high-use app can set **numberOfWorkers** too**3** toohave more processing capacity for that app.</span></span> 
    - <span data-ttu-id="81a49-145">Nízká použití aplikace se nastavuje **numberOfWorkers** příliš**1**.</span><span class="sxs-lookup"><span data-stu-id="81a49-145">Low-use apps would set **numberOfWorkers** too**1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81a49-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81a49-146">Next Steps</span></span>

- [<span data-ttu-id="81a49-147">Podrobný přehled plánů služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="81a49-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="81a49-148">Úvod tooApp Service Environment</span><span class="sxs-lookup"><span data-stu-id="81a49-148">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
