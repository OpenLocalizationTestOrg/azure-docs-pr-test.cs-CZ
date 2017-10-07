---
title: "aaaProvision Redis Cache pomocí Azure Resource Manager | Microsoft Docs"
description: "Použijte toodeploy šablony Azure Resource Manager Azure Redis Cache."
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
ms.openlocfilehash: 46e7b3b2493ac51dbe6bab0b086304802afc5d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-redis-cache-using-a-template"></a><span data-ttu-id="d8516-103">Vytvoření mezipaměti Redis Cache pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="d8516-103">Create a Redis Cache using a template</span></span>
<span data-ttu-id="d8516-104">V tomto tématu se dozvíte, jak toocreate šablonu Azure Resource Manager, které nasazuje Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d8516-104">In this topic, you learn how toocreate an Azure Resource Manager template that deploys an Azure Redis Cache.</span></span> <span data-ttu-id="d8516-105">Hello mezipaměti můžete použít s existující úložiště účet tookeep diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="d8516-105">hello cache can be used with an existing storage account tookeep diagnostic data.</span></span> <span data-ttu-id="d8516-106">Také zjistíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="d8516-106">You also learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="d8516-107">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="d8516-107">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="d8516-108">V současné době jsou sdílené nastavení diagnostiky pro všechny mezipaměti v hello stejné oblasti pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="d8516-108">Currently, diagnostic settings are shared for all caches in hello same region for a subscription.</span></span> <span data-ttu-id="d8516-109">Aktualizace mezipaměti jeden v oblasti hello ovlivňuje všechny mezipaměti v oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="d8516-109">Updating one cache in hello region affects all other caches in hello region.</span></span>

<span data-ttu-id="d8516-110">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d8516-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="d8516-111">Hello úplnou šablonu, najdete v části [Redis Cache šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="d8516-111">For hello complete template, see [Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span></span>

> [!NOTE]
> <span data-ttu-id="d8516-112">Šablony Resource Manageru pro nový hello [úroveň Premium](cache-premium-tier-intro.md) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d8516-112">Resource Manager templates for hello new [Premium tier](cache-premium-tier-intro.md) are available.</span></span> 
> 
> * [<span data-ttu-id="d8516-113">Vytvořit Premium Redis Cache s clusteringem</span><span class="sxs-lookup"><span data-stu-id="d8516-113">Create a Premium Redis Cache with clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [<span data-ttu-id="d8516-114">Vytvoření pomocí trvalosti dat Premium Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d8516-114">Create Premium Redis Cache with data persistence</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [<span data-ttu-id="d8516-115">Vytvořit Premium Redis Cache s virtuální sítí a volitelné clustering</span><span class="sxs-lookup"><span data-stu-id="d8516-115">Create Premium Redis Cache with VNet and optional clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> <span data-ttu-id="d8516-116">toocheck pro hello nejnovější šablony, najdete v části [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) a vyhledejte řetězec `Redis Cache`.</span><span class="sxs-lookup"><span data-stu-id="d8516-116">toocheck for hello latest templates, see [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/) and search for `Redis Cache`.</span></span>
> 
> 

## <a name="what-you-will-deploy"></a><span data-ttu-id="d8516-117">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="d8516-117">What you will deploy</span></span>
<span data-ttu-id="d8516-118">V této šabloně nasadíte Azure Redis Cache, který používá stávající účet úložiště pro diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="d8516-118">In this template, you will deploy an Azure Redis Cache that uses an existing storage account for diagnostic data.</span></span>

<span data-ttu-id="d8516-119">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="d8516-119">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="d8516-120">[![Nasazení tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="d8516-120">[![Deploy tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="d8516-121">Parametry</span><span class="sxs-lookup"><span data-stu-id="d8516-121">Parameters</span></span>
<span data-ttu-id="d8516-122">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="d8516-122">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="d8516-123">Šablona Hello obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="d8516-123">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="d8516-124">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="d8516-124">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="d8516-125">Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="d8516-125">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="d8516-126">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="d8516-126">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a><span data-ttu-id="d8516-127">redisCacheLocation</span><span class="sxs-lookup"><span data-stu-id="d8516-127">redisCacheLocation</span></span>
<span data-ttu-id="d8516-128">umístění Hello hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d8516-128">hello location of hello Redis Cache.</span></span> <span data-ttu-id="d8516-129">Pro nejlepší výkon použijte hello stejné umístění jako hello toobe aplikace použít s hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d8516-129">For best performance, use hello same location as hello app toobe used with hello cache.</span></span>

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a><span data-ttu-id="d8516-130">existingDiagnosticsStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="d8516-130">existingDiagnosticsStorageAccountName</span></span>
<span data-ttu-id="d8516-131">Hello název hello toouse účet existující úložiště pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="d8516-131">hello name of hello existing storage account toouse for diagnostics.</span></span> 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a><span data-ttu-id="d8516-132">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="d8516-132">enableNonSslPort</span></span>
<span data-ttu-id="d8516-133">Logická hodnota, která určuje, zda tooallow přístup přes porty bez SSL.</span><span class="sxs-lookup"><span data-stu-id="d8516-133">A boolean value that indicates whether tooallow access via non-SSL ports.</span></span>

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a><span data-ttu-id="d8516-134">diagnosticsStatus</span><span class="sxs-lookup"><span data-stu-id="d8516-134">diagnosticsStatus</span></span>
<span data-ttu-id="d8516-135">Hodnota, která určuje, zda je povoleno diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d8516-135">A value that indicates whether diagnostics is enabled.</span></span> <span data-ttu-id="d8516-136">Použití ON nebo OFF.</span><span class="sxs-lookup"><span data-stu-id="d8516-136">Use ON or OFF.</span></span>

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="d8516-137">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="d8516-137">Resources toodeploy</span></span>
### <a name="redis-cache"></a><span data-ttu-id="d8516-138">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d8516-138">Redis Cache</span></span>
<span data-ttu-id="d8516-139">Vytvoří hello Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d8516-139">Creates hello Azure Redis Cache.</span></span>

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



## <a name="commands-toorun-deployment"></a><span data-ttu-id="d8516-140">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="d8516-140">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="d8516-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d8516-141">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a><span data-ttu-id="d8516-142">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d8516-142">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


