---
title: "Zřídit Redis Cache pomocí Azure Resource Manager | Microsoft Docs"
description: "Šablona Azure Resource Manager k nasazení Azure Redis Cache."
services: app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: cce5d63e8bad2dd066cb4c28e2a8a9cb16c47953
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-redis-cache-using-a-template"></a><span data-ttu-id="246d3-103">Vytvoření mezipaměti Redis Cache pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="246d3-103">Create a Redis Cache using a template</span></span>
<span data-ttu-id="246d3-104">V tomto tématu se dozvíte, jak vytvořit šablonu Azure Resource Manager, která nasazuje Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="246d3-104">In this topic, you learn how to create an Azure Resource Manager template that deploys an Azure Redis Cache.</span></span> <span data-ttu-id="246d3-105">Mezipaměti umožňuje s existujícím účtem úložiště zachovat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="246d3-105">The cache can be used with an existing storage account to keep diagnostic data.</span></span> <span data-ttu-id="246d3-106">Také zjistíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="246d3-106">You also learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="246d3-107">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="246d3-107">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="246d3-108">Nastavení diagnostiky se v současné době jsou sdíleny pro všechny mezipaměti ve stejné oblasti pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="246d3-108">Currently, diagnostic settings are shared for all caches in the same region for a subscription.</span></span> <span data-ttu-id="246d3-109">Aktualizace mezipaměti jeden v oblasti ovlivňuje všechny mezipaměti v oblasti.</span><span class="sxs-lookup"><span data-stu-id="246d3-109">Updating one cache in the region affects all other caches in the region.</span></span>

<span data-ttu-id="246d3-110">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="246d3-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="246d3-111">Pro úplnou šablonu, najdete v části [Redis Cache šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="246d3-111">For the complete template, see [Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span></span>

> [!NOTE]
> <span data-ttu-id="246d3-112">Šablony Resource Manageru pro nový [úroveň Premium](cache-premium-tier-intro.md) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="246d3-112">Resource Manager templates for the new [Premium tier](cache-premium-tier-intro.md) are available.</span></span> 
> 
> * [<span data-ttu-id="246d3-113">Vytvořit Premium Redis Cache s clusteringem</span><span class="sxs-lookup"><span data-stu-id="246d3-113">Create a Premium Redis Cache with clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [<span data-ttu-id="246d3-114">Vytvoření pomocí trvalosti dat Premium Redis Cache</span><span class="sxs-lookup"><span data-stu-id="246d3-114">Create Premium Redis Cache with data persistence</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [<span data-ttu-id="246d3-115">Vytvořit Premium Redis Cache s virtuální sítí a volitelné clustering</span><span class="sxs-lookup"><span data-stu-id="246d3-115">Create Premium Redis Cache with VNet and optional clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> <span data-ttu-id="246d3-116">Vyhledat nejnovější šablony, najdete v tématu [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) a vyhledejte řetězec `Redis Cache`.</span><span class="sxs-lookup"><span data-stu-id="246d3-116">To check for the latest templates, see [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/) and search for `Redis Cache`.</span></span>
> 
> 

## <a name="what-you-will-deploy"></a><span data-ttu-id="246d3-117">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="246d3-117">What you will deploy</span></span>
<span data-ttu-id="246d3-118">V této šabloně nasadíte Azure Redis Cache, který používá stávající účet úložiště pro diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="246d3-118">In this template, you will deploy an Azure Redis Cache that uses an existing storage account for diagnostic data.</span></span>

<span data-ttu-id="246d3-119">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="246d3-119">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="246d3-120">[![Nasazení do Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="246d3-120">[![Deploy to Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="246d3-121">Parametry</span><span class="sxs-lookup"><span data-stu-id="246d3-121">Parameters</span></span>
<span data-ttu-id="246d3-122">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="246d3-122">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="246d3-123">Šablona obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="246d3-123">The template includes a section called Parameters that contains all of the parameter values.</span></span>
<span data-ttu-id="246d3-124">Parametr byste měli definovat pro hodnoty, které se mění v závislosti na nasazovaném projektu nebo prostředí, do kterého nasazujete.</span><span class="sxs-lookup"><span data-stu-id="246d3-124">You should define a parameter for those values that vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="246d3-125">Nedefinujte parametry pro hodnoty, které jsou vždy stejné.</span><span class="sxs-lookup"><span data-stu-id="246d3-125">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="246d3-126">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="246d3-126">Each parameter value is used in the template to define the resources that are deployed.</span></span> 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a><span data-ttu-id="246d3-127">redisCacheLocation</span><span class="sxs-lookup"><span data-stu-id="246d3-127">redisCacheLocation</span></span>
<span data-ttu-id="246d3-128">Umístění mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="246d3-128">The location of the Redis Cache.</span></span> <span data-ttu-id="246d3-129">Pro zajištění nejlepšího výkonu používalo stejné umístění jako aplikace pro použití s mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="246d3-129">For best performance, use the same location as the app to be used with the cache.</span></span>

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a><span data-ttu-id="246d3-130">existingDiagnosticsStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="246d3-130">existingDiagnosticsStorageAccountName</span></span>
<span data-ttu-id="246d3-131">Název existující účet úložiště pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="246d3-131">The name of the existing storage account to use for diagnostics.</span></span> 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a><span data-ttu-id="246d3-132">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="246d3-132">enableNonSslPort</span></span>
<span data-ttu-id="246d3-133">Logická hodnota, která určuje, jestli se má povolit přístup přes porty bez SSL.</span><span class="sxs-lookup"><span data-stu-id="246d3-133">A boolean value that indicates whether to allow access via non-SSL ports.</span></span>

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a><span data-ttu-id="246d3-134">diagnosticsStatus</span><span class="sxs-lookup"><span data-stu-id="246d3-134">diagnosticsStatus</span></span>
<span data-ttu-id="246d3-135">Hodnota, která určuje, zda je povoleno diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="246d3-135">A value that indicates whether diagnostics is enabled.</span></span> <span data-ttu-id="246d3-136">Použití ON nebo OFF.</span><span class="sxs-lookup"><span data-stu-id="246d3-136">Use ON or OFF.</span></span>

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a><span data-ttu-id="246d3-137">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="246d3-137">Resources to deploy</span></span>
### <a name="redis-cache"></a><span data-ttu-id="246d3-138">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="246d3-138">Redis Cache</span></span>
<span data-ttu-id="246d3-139">Vytvoří mezipaměť Redis systému Azure.</span><span class="sxs-lookup"><span data-stu-id="246d3-139">Creates the Azure Redis Cache.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a><span data-ttu-id="246d3-140">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="246d3-140">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="246d3-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="246d3-141">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a><span data-ttu-id="246d3-142">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="246d3-142">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


