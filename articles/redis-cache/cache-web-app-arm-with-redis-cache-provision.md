---
title: "Zřízení webové aplikace s Redis Cache"
description: "Šablona Azure Resource Manager k nasazení webové aplikace s Redis Cache."
services: app-service
documentationcenter: 
author: steved0x
manager: erickson-doug
editor: 
ms.assetid: 6e99c71f-ef8e-4570-a307-e4c059e60c35
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: 810c1cedd4fe0bd6ecdf9bd32dfb241f5f345300
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="5faa5-103">Vytvoření webové aplikace a Redis Cache pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="5faa5-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="5faa5-104">V tomto tématu se dozvíte, jak vytvořit šablonu Azure Resource Manager, nasadí webové aplikace Azure s Redis cache.</span><span class="sxs-lookup"><span data-stu-id="5faa5-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="5faa5-105">Se dozvíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="5faa5-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="5faa5-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="5faa5-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="5faa5-107">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5faa5-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="5faa5-108">Pro úplnou šablonu, najdete v části [webové aplikace s Redis Cache šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="5faa5-108">For the complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="5faa5-109">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="5faa5-109">What you will deploy</span></span>
<span data-ttu-id="5faa5-110">V této šabloně je nasadit:</span><span class="sxs-lookup"><span data-stu-id="5faa5-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="5faa5-111">Webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="5faa5-111">Azure Web App</span></span>
* <span data-ttu-id="5faa5-112">Mezipaměť Redis systému Azure.</span><span class="sxs-lookup"><span data-stu-id="5faa5-112">Azure Redis Cache.</span></span>

<span data-ttu-id="5faa5-113">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="5faa5-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="5faa5-114">[![Nasazení do Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="5faa5-114">[![Deploy to Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-to-specify"></a><span data-ttu-id="5faa5-115">Chcete-li zadat parametry</span><span class="sxs-lookup"><span data-stu-id="5faa5-115">Parameters to specify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="5faa5-116">Proměnné pro názvy</span><span class="sxs-lookup"><span data-stu-id="5faa5-116">Variables for names</span></span>
<span data-ttu-id="5faa5-117">Tato šablona používá proměnné vytvořit názvy pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="5faa5-117">This template uses variables to construct names for the resources.</span></span> <span data-ttu-id="5faa5-118">Použije [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) funkce vytvořit hodnotu podle id skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5faa5-118">It uses the [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function to construct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a><span data-ttu-id="5faa5-119">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="5faa5-119">Resources to deploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="5faa5-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="5faa5-120">Redis Cache</span></span>
<span data-ttu-id="5faa5-121">Vytvoří Azure Redis Cache, která se používá s webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5faa5-121">Creates the Azure Redis Cache that is used with the web app.</span></span> <span data-ttu-id="5faa5-122">Název mezipaměti je zadán v **cacheName** proměnné.</span><span class="sxs-lookup"><span data-stu-id="5faa5-122">The name of the cache is specified in the **cacheName** variable.</span></span>

<span data-ttu-id="5faa5-123">Šablona vytvoří mezipaměti ve stejném umístění jako pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5faa5-123">The template creates the cache in the same location as the resource group.</span></span>

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a><span data-ttu-id="5faa5-124">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="5faa5-124">Web app</span></span>
<span data-ttu-id="5faa5-125">Vytvoří webovou aplikaci s názvem zadaným v **zadaným hodnotám webSiteName** proměnné.</span><span class="sxs-lookup"><span data-stu-id="5faa5-125">Creates the web app with name specified in the **webSiteName** variable.</span></span>

<span data-ttu-id="5faa5-126">Všimněte si, že webová aplikace nakonfigurovaná s vlastností nastavení aplikace, které umožňují ho na spolupráci s Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="5faa5-126">Notice that the web app is configured with app setting properties that enable it to work with the Redis Cache.</span></span> <span data-ttu-id="5faa5-127">Tuto aplikaci, které nastavení se vytvářejí dynamicky podle hodnoty poskytnuté během nasazení.</span><span class="sxs-lookup"><span data-stu-id="5faa5-127">This app settings are dynamically created based on values provided during deployment.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a><span data-ttu-id="5faa5-128">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="5faa5-128">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="5faa5-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5faa5-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="5faa5-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5faa5-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
