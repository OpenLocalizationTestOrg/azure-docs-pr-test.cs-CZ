---
title: "aaaCreate v Azure pomocí šablony aplikace logiky | Microsoft Docs"
description: "Použijte toodeploy šablony Azure Resource Manager aplikace logiky pro definování pracovních postupů."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: efbacb534fc7f11e9b593aae4383480ce3a1752f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-logic-app-using-a-template"></a>Vytvoření aplikace logiky pomocí šablony
Šablony poskytují rychlý způsob toouse různé konektory v rámci aplikace logiky. Aplikace logiky obsahuje šablony Azure Resource Manageru pro toocreate aplikace logiky, kterou lze použít toodefine obchodní pracovních. Můžete definovat, jaké prostředky jsou nasazeny a jak toodefine parametry při nasazení aplikace logiky. Můžete tuto šablonu použít pro vaše vlastní obchodní scénáře, nebo si ji přizpůsobit toomeet vašim požadavkům.

Další informace o vlastnosti aplikace logiky hello v [rozhraní API pro správu aplikace logiky aplikace pracovního postupu](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Příklady definice hello, samotné najdete v tématu [definice aplikace logiky Autor](logic-apps-author-definitions.md). 

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Hello úplnou šablonu, najdete v části [šablona aplikace logiky](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Můžete nasadit
Pomocí této šablony můžete nasadit aplikace logiky.

nasazení hello toorun automaticky, vyberte hello následující tlačítko:  

[![Nasazení tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-toodeploy"></a>Toodeploy prostředky
### <a name="logic-app"></a>Aplikace logiky
Vytvoří aplikace logiky hello.

šablony Hello používá hodnotu parametru pro název aplikace logiky hello. Nastaví hello umístění hello logiku aplikace toohello stejné umístění jako skupina prostředků hello. 

Definice této konkrétní spouští jednou za hodinu a příkazy ping hello umístění zadaná v hello **testUri** parametr. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



