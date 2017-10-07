---
title: "aaaProvision webové aplikace s Redis Cache"
description: "Pomocí Azure Resource Manager šablony toodeploy webové aplikace s Redis Cache."
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
ms.openlocfilehash: b95b5e230dc40c1157940c2017cba836975b6930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="82606-103">Vytvoření webové aplikace a Redis Cache pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="82606-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="82606-104">V tomto tématu se dozvíte, jak toocreate šablonu Azure Resource Manager, která nasazuje webové aplikace Azure s Redis cache.</span><span class="sxs-lookup"><span data-stu-id="82606-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="82606-105">Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="82606-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="82606-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="82606-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="82606-107">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="82606-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="82606-108">Hello úplnou šablonu, najdete v části [webové aplikace s Redis Cache šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="82606-108">For hello complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="82606-109">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="82606-109">What you will deploy</span></span>
<span data-ttu-id="82606-110">V této šabloně je nasadit:</span><span class="sxs-lookup"><span data-stu-id="82606-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="82606-111">Webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="82606-111">Azure Web App</span></span>
* <span data-ttu-id="82606-112">Mezipaměť Redis systému Azure.</span><span class="sxs-lookup"><span data-stu-id="82606-112">Azure Redis Cache.</span></span>

<span data-ttu-id="82606-113">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="82606-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="82606-114">[![Nasazení tooAzure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="82606-114">[![Deploy tooAzure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-toospecify"></a><span data-ttu-id="82606-115">Parametry toospecify</span><span class="sxs-lookup"><span data-stu-id="82606-115">Parameters toospecify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="82606-116">Proměnné pro názvy</span><span class="sxs-lookup"><span data-stu-id="82606-116">Variables for names</span></span>
<span data-ttu-id="82606-117">Tato šablona používá názvy proměnných tooconstruct hello zdrojů.</span><span class="sxs-lookup"><span data-stu-id="82606-117">This template uses variables tooconstruct names for hello resources.</span></span> <span data-ttu-id="82606-118">Používá hello [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) funkce tooconstruct hodnotu podle id skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="82606-118">It uses hello [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function tooconstruct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-toodeploy"></a><span data-ttu-id="82606-119">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="82606-119">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="82606-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="82606-120">Redis Cache</span></span>
<span data-ttu-id="82606-121">Vytvoří hello Redis Cache Azure, která se používá s hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82606-121">Creates hello Azure Redis Cache that is used with hello web app.</span></span> <span data-ttu-id="82606-122">Název Hello hello mezipaměti je zadán v hello **cacheName** proměnné.</span><span class="sxs-lookup"><span data-stu-id="82606-122">hello name of hello cache is specified in hello **cacheName** variable.</span></span>

<span data-ttu-id="82606-123">Hello šablona vytváří hello mezipaměti v hello stejné umístění jako skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="82606-123">hello template creates hello cache in hello same location as hello resource group.</span></span>

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


### <a name="web-app"></a><span data-ttu-id="82606-124">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="82606-124">Web app</span></span>
<span data-ttu-id="82606-125">Vytvoří hello webovou aplikaci s názvem zadaným v hello **zadaným hodnotám webSiteName** proměnné.</span><span class="sxs-lookup"><span data-stu-id="82606-125">Creates hello web app with name specified in hello **webSiteName** variable.</span></span>

<span data-ttu-id="82606-126">Všimněte si, že webová aplikace hello nakonfigurovaná v aplikaci nastavení vlastností, které je povolují toowork s hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="82606-126">Notice that hello web app is configured with app setting properties that enable it toowork with hello Redis Cache.</span></span> <span data-ttu-id="82606-127">Tuto aplikaci, které nastavení se vytvářejí dynamicky podle hodnoty poskytnuté během nasazení.</span><span class="sxs-lookup"><span data-stu-id="82606-127">This app settings are dynamically created based on values provided during deployment.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="82606-128">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="82606-128">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="82606-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="82606-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="82606-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="82606-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
