---
title: "aaaDeploy webovou aplikaci, která je propojená úložiště GitHub tooa | Microsoft Docs"
description: "Použijte toodeploy šablony Azure Resource Manager webovou aplikaci, která obsahuje projekt z úložiště Githubu."
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
ms.openlocfilehash: 8b23416c4c06a60991517e6ee4cd82bebc5a9d73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a>Nasazení webové aplikace propojené tooa Githubu úložiště
V tomto tématu se dozvíte, jak toocreate šablonu Azure Resource Manager, která nasazuje webovou aplikaci, která je propojená tooa projekt v úložišti GitHub. Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello. Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Hello úplnou šablonu, najdete v části [šablony webové aplikace propojené tooGitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Co budete nasazovat
Pomocí této šablony nasadíte webovou aplikaci, která obsahuje kód hello z projektu na Githubu.

toorun hello nasazení automaticky, klikněte na následující tlačítko hello:

[![Nasazení tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
Adresa URL Hello pro úložiště GitHub obsahující toodeploy projektu hello. Tento parametr obsahuje výchozí hodnotu, ale tato hodnota je jenom zamýšlený tooshow můžete jak tooprovide hello adresu URL pro úložiště. Tuto hodnotu můžete použít při testování hello šablony, ale bude vhodné tooprovide hello URL vlastního úložiště při práci s hello šablony.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Firemní pobočky
větev Hello hello úložiště toouse při nasazování aplikace hello. Hello výchozí hodnota je hlavní, ale můžete zadat název hello žádné větve v úložišti hello chcete toodeploy.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a>Toodeploy prostředky
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Webová aplikace
Vytvoří hello webovou aplikaci, která je propojená toohello projektu na Githubu. 

Zadejte název hello hello webové aplikace prostřednictvím hello **siteName** parametr a umístění hello hello webové aplikace prostřednictvím hello **siteLocation** parametr. V hello **dependsOn** elementu hello Šablona definuje hello webové aplikace jako závisí na službě hello hostování plánu. Vzhledem k tomu, že je závislá na hello hostování plán, hello webové aplikace se nevytvoří, dokud nedokončí hello hostování plán vytváří. Hello **dependsOn** element je pouze použité toospecify pořadí nasazení. Pokud webová aplikace hello jako závislé na hostování plán hello nezaškrtnete, Azure Resource Manager pokusí toocreate i prostředky v hello stejnou dobu a může se zobrazit chyba Pokud hello webové aplikace se vytvoří před hello hostování plánu.

Hello webové aplikace se také podřízený prostředek, který je definován v **prostředky** části níže. Tento podřízený prostředek definuje zdrojového kódu pro nasazení s hello webové aplikace projektu hello. V této šabloně je hello zdrojového kódu propojené tooa konkrétní úložiště GitHub. úložiště GitHub Hello je definován s kódem hello **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** může adresu URL úložiště pevně hello, pokud chcete toocreate šablonu, která opakovaně nasadí jeden projektu při vyžadování hello minimální počet parametrů.
Místo pevně kódováno hello adresu URL úložiště, můžete přidat parametr pro adresu URL úložiště hello a používat tuto hodnotu pro hello **RepoUrl** vlastnost.

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

## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> Obsah souboru JSON hello parametry, najdete v části [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>

