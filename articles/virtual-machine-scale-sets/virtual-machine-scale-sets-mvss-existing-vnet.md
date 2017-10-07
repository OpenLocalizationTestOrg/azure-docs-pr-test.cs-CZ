---
title: "Odkazovat na existující virtuální síť v šablonu sady Azure škálování | Microsoft Docs"
description: "Zjistěte, jak tooadd a virtuální sítě šablony tooan existující sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: c3034b577e17abc4643dc26d7c38ad643fa26322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a>Přidat odkaz na tooan existující virtuální síť v šablonu sady Azure škálování

Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toodeploy do existující virtuální síť místo vytvoření nové.

## <a name="change-hello-template-definition"></a>Změna definice šablony hello

Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování sad do existující virtuální síť hello škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set existing-vnet`) část podle část:

Nejprve přidáme `subnetId` parametr. Tento řetězec se předají do konfigurace sady škálování hello, což sady škálování hello tooidentify hello předem vytvořené podsíť toodeploy virtuálních počítačů do. Tento řetězec musí být ve formátu hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Pro instanci sady škálování hello toodeploy do existující virtuální síť s názvem `myvnet`, podsíť `mysubnet`, skupinu prostředků `myrg`a předplatné `00000000-0000-0000-0000-000000000000`, bude hello subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

V dalším kroku prostředků hello virtuální sítě můžete odstranit z hello `resources` pole, protože jsme se použití existující virtuální sítě a nepotřebujete toodeploy novou.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Hello virtuální sítě už před nasazením šablony hello, takže není bez nutnosti toospecify klauzuli dependsOn od stupnice hello nastavit toohello virtuální sítě. Proto jsme odstranit tyto řádky:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Nakonec jsme předat hello `subnetId` parametr nastavit uživatelem hello (místo použití `resourceId` tooget hello id virtuální sítě v hello nemá stejné nasazení, která je šablona sady škálování minimální přijatelná jaké hello).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
