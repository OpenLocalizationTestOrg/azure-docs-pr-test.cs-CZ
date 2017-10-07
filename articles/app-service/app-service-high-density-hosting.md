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
# <a name="high-density-hosting-on-azure-app-service"></a>S vysokou hustotou hostování na Azure App Service
Při používání služby App Service, odpojené od hello kapacity přidělené tooit dvěma konceptů aplikace:

* **Hello aplikace:** představuje hello aplikace a jeho konfigurace modulu runtime. Například obsahuje hello by se měly načíst verzi rozhraní .NET, která hello runtime, nastavení aplikace hello.
* **Plán služby App Service Hello:** definuje vlastnosti hello hello kapacitu, sada k dispozici funkcí a polohu aplikace hello. Charakteristiky může být například velký počítač (4 jádra), čtyři instancí, prémiové funkce v oblasti Východ USA.

Aplikace je vždy propojené tooan plán služby App Service, ale plán služby App Service můžete poskytovat tooone kapacity nebo další aplikace.

V důsledku toho hello platforma poskytuje flexibilitu tooisolate hello jenom jedna aplikace nebo mít víc aplikací sdílení prostředků ve sdílení plán služby App Service.

Ale při více aplikací sdílet plán služby App Service, instance této aplikace běží na všechny instance tohoto plánu služby App Service.

## <a name="per-app-scaling"></a>Pokud na škálování aplikace
*Pokud na škálování aplikace* je funkce, která může být povolena na úrovni plán služby App Service a pak se použije na aplikaci.

Jednotlivé aplikace škáluje škálování aplikace nezávisle plán služby App Service, který je hostitelem ho. Tímto způsobem aplikační službu plán lze škálovat instance too10, ale aplikace lze nastavit pouze pět toouse.

   >[!NOTE]
   >Jednotlivé aplikace škálování je dostupná jenom pro **Premium** plánů služby App Service SKU
   >

### <a name="per-app-scaling-using-powershell"></a>Jednotlivé aplikace příjmu pomocí Powershellu

Můžete vytvořit plán nakonfigurovaný jako *za škálování aplikace* plán předáním v hello ```-perSiteScaling $true``` atribut toohello ```New-AzureRmAppServicePlan``` PowerShell.

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Pokud chcete, aby tooupdate stávající službu App Service plánování toouse této funkce: 

- získat hello cílový plán```Get-AzureRmAppServicePlan```
- Úprava vlastností hello místně```$newASP.PerSiteScaling = $true```
- publikování vaši tooazure zpět změny```Set-AzureRmAppServicePlan``` 

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

Na úrovni aplikace hello potřebujeme tooconfigure hello počet instancí, které aplikace hello můžete používat v hello plán služby app service.

V níže uvedeném příkladu hello aplikace hello je omezená tootwo instance bez ohledu na to, kolik instancí hello základní aplikace služby plán měřítka se k.

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> $newapp. SiteConfig.NumberOfWorkers je různá $newapp. MaxNumberOfWorkers. Jednotlivé aplikace používá škálování $newapp. SiteConfig.NumberOfWorkers toodetermine hello škálování charakteristiky aplikace hello.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Pokud na škálování aplikace pomocí Azure Resource Manager

Následující Hello *šablony Azure Resource Manageru* vytvoří:

- Plán služby App Service, která je škálovat na více systémů too10 instancí
- aplikace, který byl nakonfigurován tooscale tooa maximálně pět instancí.

Hello plán služby App Service je nastavení hello **PerSiteScaling** tootrue vlastnost ```"perSiteScaling": true```. aplikace Hello je nastavení hello **počet pracovních procesů** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.

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

## <a name="recommended-configuration-for-high-density-hosting"></a>Doporučenou konfiguraci pro hostování s vysokou hustotou
Za škálování aplikace je funkce, která je povolena v globální oblastí Azure a prostředí App Service. Ale hello doporučoval strategie je použití prostředí App Service tootake výhod jejich pokročilých funkcí a větší fondy hello kapacity.  

Postupujte podle těchto kroků tooconfigure s vysokou hustotou hostování pro vaše aplikace:

1. Nakonfigurujte hello App Service Environment a vyberte fond pracovních procesů, který je vyhrazený toohello hustotou hostování scénář.
1. Vytvoření jednoho plánu služby App Service a škálování ho toouse všechny hello dostupné kapacity ve fondu pracovních procesů hello.
1. Nastavte hello PerSiteScaling příznak tootrue hello plán služby App Service.
1. Nové aplikace jsou vytvořeny a přiřazeny toothat plán služby App Service pomocí **numberOfWorkers** vlastností nastavenou příliš**1**. Pomocí této konfigurace vypočítá hello nejvyšší hustotou možné u tohoto fondu pracovního procesu.
1. Hello počet pracovních procesů lze nakonfigurovat nezávisle na aplikaci toogrant další prostředky podle potřeby. Například:
    - Můžete nastavit aplikaci intenzivně využívaných **numberOfWorkers** příliš**3** toohave další zpracování kapacity pro tuto aplikaci. 
    - Nízká použití aplikace se nastavuje **numberOfWorkers** příliš**1**.

## <a name="next-steps"></a>Další kroky

- [Podrobný přehled plánů služby Azure App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Úvod tooApp Service Environment](../app-service-web/app-service-app-service-environment-intro.md)
