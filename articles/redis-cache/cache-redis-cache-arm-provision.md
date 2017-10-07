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
# <a name="create-a-redis-cache-using-a-template"></a>Vytvoření mezipaměti Redis Cache pomocí šablony
V tomto tématu se dozvíte, jak toocreate šablonu Azure Resource Manager, které nasazuje Azure Redis Cache. Hello mezipaměti můžete použít s existující úložiště účet tookeep diagnostická data. Také zjistíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello. Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.

V současné době jsou sdílené nastavení diagnostiky pro všechny mezipaměti v hello stejné oblasti pro předplatné. Aktualizace mezipaměti jeden v oblasti hello ovlivňuje všechny mezipaměti v oblasti hello.

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Hello úplnou šablonu, najdete v části [Redis Cache šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Šablony Resource Manageru pro nový hello [úroveň Premium](cache-premium-tier-intro.md) jsou k dispozici. 
> 
> * [Vytvořit Premium Redis Cache s clusteringem](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Vytvoření pomocí trvalosti dat Premium Redis Cache](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Vytvořit Premium Redis Cache s virtuální sítí a volitelné clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> toocheck pro hello nejnovější šablony, najdete v části [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) a vyhledejte řetězec `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Co budete nasazovat
V této šabloně nasadíte Azure Redis Cache, který používá stávající účet úložiště pro diagnostická data.

toorun hello nasazení automaticky, klikněte na následující tlačítko hello:

[![Nasazení tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru hello.
Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné. Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
umístění Hello hello Redis Cache. Pro nejlepší výkon použijte hello stejné umístění jako hello toobe aplikace použít s hello mezipaměti.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Hello název hello toouse účet existující úložiště pro diagnostiku. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>EnableNonSslPort
Logická hodnota, která určuje, zda tooallow přístup přes porty bez SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Hodnota, která určuje, zda je povoleno diagnostiky. Použití ON nebo OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a>Toodeploy prostředky
### <a name="redis-cache"></a>Redis Cache
Vytvoří hello Azure Redis Cache.

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



## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


