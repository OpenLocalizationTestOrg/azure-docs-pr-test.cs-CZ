---
title: "Nasazení webové aplikace, která je propojený s úložišti GitHub | Microsoft Docs"
description: "Pomocí šablony Azure Resource Manager nasadit webovou aplikaci, která obsahuje projekt z úložiště Githubu."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: 77064802814296d0c21f004534e4264d2f97252e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Nasazení webové aplikace propojené s úložišti GitHub
V tomto tématu se dozvíte, jak vytvořit šablonu Azure Resource Manager, nasadí webové aplikace, který je propojený na projekt v úložišti GitHub. Se dozvíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení. Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Pro úplnou šablonu, najdete v části [webové aplikace propojené do šablony Githubu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Co budete nasazovat
Pomocí této šablony nasadíte webovou aplikaci, která obsahuje kód z projektu na Githubu.

Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:

[![Nasazení do Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
Adresa URL pro úložiště GitHub obsahující projekt nasadit. Tento parametr obsahuje výchozí hodnotu, ale tato hodnota je určena pouze k ukazují, jak můžete zadat adresu URL pro úložiště. Tuto hodnotu lze použít při testování šablony, ale můžete zadat adresu URL vlastního úložiště při práci se šablonou.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Firemní pobočky
Větev úložiště k použití při nasazení aplikace. Výchozí hodnota je hlavní, ale můžete zadat název žádné větve v úložišti, který chcete nasadit.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-to-deploy"></a>Prostředky k nasazení
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Webová aplikace
Vytvoří webovou aplikaci, která je propojená do projektu na Githubu. 

Zadejte název webové aplikace prostřednictvím **siteName** parametr a umístění webové aplikace prostřednictvím **siteLocation** parametr. V **dependsOn** elementu, že šablona definuje webové aplikace jako závislé na službě hostování plánu. Protože je závislá na hostování plán, webové aplikace se nevytvoří, dokud nedokončí hostingový plán vytváří. **DependsOn** element slouží pouze k určení pořadí nasazení. Pokud webová aplikace jako závislé na hostování plán nezaškrtnete, Azure Resource Manager se pokusí vytvořit i prostředky ve stejnou dobu a může se zobrazit chyba, pokud webová aplikace je vytvořen před hostingový plán.

Webové aplikace má také podřízený prostředek, který je definován v **prostředky** části níže. Tento podřízený prostředek definuje zdrojového kódu pro projekt nasazen s webovou aplikací. V této šabloně se správa zdrojového kódu propojí konkrétní úložiště GitHub. Úložiště GitHub je definován s kódem **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** pravděpodobně pevně adresu URL úložiště Pokud chcete vytvořit šablonu, která opakovaně nasadí jedné Projekt současně vyžaduje minimální počet parametrů.
Místo pevně kódováno adresu URL úložiště, můžete přidat parametr pro adresu URL úložiště a používat pro tuto hodnotu **RepoUrl** vlastnost.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> Obsah souboru JSON parametry, najdete v části [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>

