---
title: "Vytvoření aplikace logiky pomocí šablony v Azure | Microsoft Docs"
description: "Použijte šablonu Azure Resource Manager k nasazení aplikace logiky pro definování pracovních postupů."
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
ms.openlocfilehash: 161adeacd6da2b15225c8a4ddae171e19e539967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-logic-app-using-a-template"></a><span data-ttu-id="ac3c1-103">Vytvoření aplikace logiky pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="ac3c1-103">Create a Logic App using a template</span></span>
<span data-ttu-id="ac3c1-104">Šablony poskytují rychlý způsob, jak použít jiný konektory v rámci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-104">Templates provide a quick way to use different connectors within a logic app.</span></span> <span data-ttu-id="ac3c1-105">Aplikace logiky zahrnuje šablon Azure Resource Manageru k vytvoření aplikace logiky, která lze použít k definování pracovních firmy.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-105">Logic apps includes Azure Resource Manager templates for you to create a logic app that can be used to define business workflows.</span></span> <span data-ttu-id="ac3c1-106">Můžete definovat prostředky, ke kterým jsou nasazeny a jak definovat parametry, když nasazujete aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-106">You can define which resources are deployed, and how to define parameters when you deploy your logic app.</span></span> <span data-ttu-id="ac3c1-107">Můžete tuto šablonu použít pro vaše vlastní obchodní scénáře, nebo si přizpůsobit tak, aby vyhovovala vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-107">You can use this template for your own business scenarios, or customize it to meet your requirements.</span></span>

<span data-ttu-id="ac3c1-108">Další informace o vlastnostech aplikace logiky získáte v tématu [rozhraní API pro správu aplikace logiky aplikace pracovního postupu](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac3c1-108">For more details on the Logic app properties, see [Logic App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span></span> 

<span data-ttu-id="ac3c1-109">Příklady definice samotné najdete v tématu [definice aplikace logiky Autor](logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="ac3c1-109">For examples of the definition itself, see [Author Logic App definitions](logic-apps-author-definitions.md).</span></span> 

<span data-ttu-id="ac3c1-110">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ac3c1-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="ac3c1-111">Pro úplnou šablonu, najdete v části [šablona aplikace logiky](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="ac3c1-111">For the complete template, see [Logic App template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span></span>

## <a name="what-you-deploy"></a><span data-ttu-id="ac3c1-112">Můžete nasadit</span><span class="sxs-lookup"><span data-stu-id="ac3c1-112">What you deploy</span></span>
<span data-ttu-id="ac3c1-113">Pomocí této šablony můžete nasadit aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-113">With this template, you deploy a logic app.</span></span>

<span data-ttu-id="ac3c1-114">Chcete-li spustit nasazení automaticky, vyberte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="ac3c1-114">To run the deployment automatically, select the following button:</span></span>  

<span data-ttu-id="ac3c1-115">[![Nasazení do Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="ac3c1-115">[![Deploy to Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="ac3c1-116">Parametry</span><span class="sxs-lookup"><span data-stu-id="ac3c1-116">Parameters</span></span>
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a><span data-ttu-id="ac3c1-117">testUri</span><span class="sxs-lookup"><span data-stu-id="ac3c1-117">testUri</span></span>
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a><span data-ttu-id="ac3c1-118">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="ac3c1-118">Resources to deploy</span></span>
### <a name="logic-app"></a><span data-ttu-id="ac3c1-119">Aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="ac3c1-119">Logic app</span></span>
<span data-ttu-id="ac3c1-120">Vytvoří aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-120">Creates the logic app.</span></span>

<span data-ttu-id="ac3c1-121">Šablony používá hodnotu parametru pro název aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-121">The templates uses a parameter value for the logic app name.</span></span> <span data-ttu-id="ac3c1-122">Nastaví umístění aplikaci logiky do stejného umístění jako pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-122">It sets the location of the logic app to the same location as the resource group.</span></span> 

<span data-ttu-id="ac3c1-123">Definice této konkrétní spouští jednou za hodinu a odešle příkaz ping umístění zadané v **testUri** parametr.</span><span class="sxs-lookup"><span data-stu-id="ac3c1-123">This particular definition runs once an hour, and pings the location specified in the **testUri** parameter.</span></span> 

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


## <a name="commands-to-run-deployment"></a><span data-ttu-id="ac3c1-124">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="ac3c1-124">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="ac3c1-125">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac3c1-125">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="ac3c1-126">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ac3c1-126">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



