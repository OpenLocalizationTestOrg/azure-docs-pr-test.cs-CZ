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
# <a name="create-a-logic-app-using-a-template"></a><span data-ttu-id="0949e-103">Vytvoření aplikace logiky pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="0949e-103">Create a Logic App using a template</span></span>
<span data-ttu-id="0949e-104">Šablony poskytují rychlý způsob toouse různé konektory v rámci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="0949e-104">Templates provide a quick way toouse different connectors within a logic app.</span></span> <span data-ttu-id="0949e-105">Aplikace logiky obsahuje šablony Azure Resource Manageru pro toocreate aplikace logiky, kterou lze použít toodefine obchodní pracovních.</span><span class="sxs-lookup"><span data-stu-id="0949e-105">Logic apps includes Azure Resource Manager templates for you toocreate a logic app that can be used toodefine business workflows.</span></span> <span data-ttu-id="0949e-106">Můžete definovat, jaké prostředky jsou nasazeny a jak toodefine parametry při nasazení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="0949e-106">You can define which resources are deployed, and how toodefine parameters when you deploy your logic app.</span></span> <span data-ttu-id="0949e-107">Můžete tuto šablonu použít pro vaše vlastní obchodní scénáře, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="0949e-107">You can use this template for your own business scenarios, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="0949e-108">Další informace o vlastnosti aplikace logiky hello v [rozhraní API pro správu aplikace logiky aplikace pracovního postupu](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span><span class="sxs-lookup"><span data-stu-id="0949e-108">For more details on hello Logic app properties, see [Logic App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span></span> 

<span data-ttu-id="0949e-109">Příklady definice hello, samotné najdete v tématu [definice aplikace logiky Autor](logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="0949e-109">For examples of hello definition itself, see [Author Logic App definitions](logic-apps-author-definitions.md).</span></span> 

<span data-ttu-id="0949e-110">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0949e-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="0949e-111">Hello úplnou šablonu, najdete v části [šablona aplikace logiky](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="0949e-111">For hello complete template, see [Logic App template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span></span>

## <a name="what-you-deploy"></a><span data-ttu-id="0949e-112">Můžete nasadit</span><span class="sxs-lookup"><span data-stu-id="0949e-112">What you deploy</span></span>
<span data-ttu-id="0949e-113">Pomocí této šablony můžete nasadit aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="0949e-113">With this template, you deploy a logic app.</span></span>

<span data-ttu-id="0949e-114">nasazení hello toorun automaticky, vyberte hello následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="0949e-114">toorun hello deployment automatically, select hello following button:</span></span>  

<span data-ttu-id="0949e-115">[![Nasazení tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0949e-115">[![Deploy tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="0949e-116">Parametry</span><span class="sxs-lookup"><span data-stu-id="0949e-116">Parameters</span></span>
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a><span data-ttu-id="0949e-117">testUri</span><span class="sxs-lookup"><span data-stu-id="0949e-117">testUri</span></span>
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-toodeploy"></a><span data-ttu-id="0949e-118">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="0949e-118">Resources toodeploy</span></span>
### <a name="logic-app"></a><span data-ttu-id="0949e-119">Aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="0949e-119">Logic app</span></span>
<span data-ttu-id="0949e-120">Vytvoří aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="0949e-120">Creates hello logic app.</span></span>

<span data-ttu-id="0949e-121">šablony Hello používá hodnotu parametru pro název aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="0949e-121">hello templates uses a parameter value for hello logic app name.</span></span> <span data-ttu-id="0949e-122">Nastaví hello umístění hello logiku aplikace toohello stejné umístění jako skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="0949e-122">It sets hello location of hello logic app toohello same location as hello resource group.</span></span> 

<span data-ttu-id="0949e-123">Definice této konkrétní spouští jednou za hodinu a příkazy ping hello umístění zadaná v hello **testUri** parametr.</span><span class="sxs-lookup"><span data-stu-id="0949e-123">This particular definition runs once an hour, and pings hello location specified in hello **testUri** parameter.</span></span> 

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


## <a name="commands-toorun-deployment"></a><span data-ttu-id="0949e-124">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="0949e-124">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="0949e-125">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0949e-125">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="0949e-126">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0949e-126">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



