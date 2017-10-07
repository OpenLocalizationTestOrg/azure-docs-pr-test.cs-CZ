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
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="9701b-103">Přidat odkaz na tooan existující virtuální síť v šablonu sady Azure škálování</span><span class="sxs-lookup"><span data-stu-id="9701b-103">Add reference tooan existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="9701b-104">Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toodeploy do existující virtuální síť místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="9701b-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="9701b-105">Změna definice šablony hello</span><span class="sxs-lookup"><span data-stu-id="9701b-105">Change hello template definition</span></span>

<span data-ttu-id="9701b-106">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování sad do existující virtuální síť hello škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="9701b-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="9701b-107">Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set existing-vnet`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="9701b-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="9701b-108">Nejprve přidáme `subnetId` parametr.</span><span class="sxs-lookup"><span data-stu-id="9701b-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="9701b-109">Tento řetězec se předají do konfigurace sady škálování hello, což sady škálování hello tooidentify hello předem vytvořené podsíť toodeploy virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="9701b-109">This string will be passed into hello scale set configuration, allowing hello scale set tooidentify hello pre-created subnet toodeploy virtual machines into.</span></span> <span data-ttu-id="9701b-110">Tento řetězec musí být ve formátu hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="9701b-110">This string must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="9701b-111">Pro instanci sady škálování hello toodeploy do existující virtuální síť s názvem `myvnet`, podsíť `mysubnet`, skupinu prostředků `myrg`a předplatné `00000000-0000-0000-0000-000000000000`, bude hello subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="9701b-111">For instance, toodeploy hello scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, hello subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

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

<span data-ttu-id="9701b-112">V dalším kroku prostředků hello virtuální sítě můžete odstranit z hello `resources` pole, protože jsme se použití existující virtuální sítě a nepotřebujete toodeploy novou.</span><span class="sxs-lookup"><span data-stu-id="9701b-112">Next, we can delete hello virtual network resource from hello `resources` array, since we are using an existing virtual network and don't need toodeploy a new one.</span></span>

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

<span data-ttu-id="9701b-113">Hello virtuální sítě už před nasazením šablony hello, takže není bez nutnosti toospecify klauzuli dependsOn od stupnice hello nastavit toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9701b-113">hello virtual network already exists before hello template is deployed, so there is no need toospecify a dependsOn clause from hello scale set toohello virtual network.</span></span> <span data-ttu-id="9701b-114">Proto jsme odstranit tyto řádky:</span><span class="sxs-lookup"><span data-stu-id="9701b-114">Thus, we delete these lines:</span></span>

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

<span data-ttu-id="9701b-115">Nakonec jsme předat hello `subnetId` parametr nastavit uživatelem hello (místo použití `resourceId` tooget hello id virtuální sítě v hello nemá stejné nasazení, která je šablona sady škálování minimální přijatelná jaké hello).</span><span class="sxs-lookup"><span data-stu-id="9701b-115">Finally, we pass in hello `subnetId` parameter set by hello user (instead of using `resourceId` tooget hello id of a vnet in hello same deployment, which is what hello minimum viable scale set template does).</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="9701b-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9701b-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
