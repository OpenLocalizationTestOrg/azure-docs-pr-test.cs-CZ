---
title: "Odkazovat na existující virtuální síť v šablonu sady Azure škálování | Microsoft Docs"
description: "Informace o postupu přidání virtuální sítě do stávající šablony sadu škálování virtuálního počítače Azure"
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
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="9dfad-103">Přidat odkaz na existující virtuální síť v šablonu sady Azure škálování</span><span class="sxs-lookup"><span data-stu-id="9dfad-103">Add reference to an existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="9dfad-104">Tento článek ukazuje, jak upravit [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) k nasazení do existující virtuální síť místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="9dfad-104">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to deploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="9dfad-105">Změna definice šablony</span><span class="sxs-lookup"><span data-stu-id="9dfad-105">Change the template definition</span></span>

<span data-ttu-id="9dfad-106">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablony pro nasazení do existující virtuální síť sad škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="9dfad-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="9dfad-107">Podívejme se na rozdílové použít k vytvoření této šablony (`git diff minimum-viable-scale-set existing-vnet`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="9dfad-107">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="9dfad-108">Nejprve přidáme `subnetId` parametr.</span><span class="sxs-lookup"><span data-stu-id="9dfad-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="9dfad-109">Tento řetězec se předají do konfigurace sady škálování, povolení k identifikaci předem vytvořené podsítě k nasazení virtuálních počítačů do sad škálování.</span><span class="sxs-lookup"><span data-stu-id="9dfad-109">This string will be passed into the scale set configuration, allowing the scale set to identify the pre-created subnet to deploy virtual machines into.</span></span> <span data-ttu-id="9dfad-110">Tento řetězec musí být ve tvaru: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="9dfad-110">This string must be of the form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="9dfad-111">Například nasazení měřítka nastavení do existující virtuální síť s názvem `myvnet`, podsíť `mysubnet`, skupinu prostředků `myrg`a předplatné `00000000-0000-0000-0000-000000000000`, by subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="9dfad-111">For instance, to deploy the scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, the subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

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

<span data-ttu-id="9dfad-112">V dalším kroku jsme odstranit virtuální síť prostředek z `resources` pole, protože jsme se použití existující virtuální sítě a není nutné nasazovat nové.</span><span class="sxs-lookup"><span data-stu-id="9dfad-112">Next, we can delete the virtual network resource from the `resources` array, since we are using an existing virtual network and don't need to deploy a new one.</span></span>

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

<span data-ttu-id="9dfad-113">Virtuální sítě už existuje před nasazením šablony, takže není nutné specifikovat klauzuli dependsOn od stupnice nastavit do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9dfad-113">The virtual network already exists before the template is deployed, so there is no need to specify a dependsOn clause from the scale set to the virtual network.</span></span> <span data-ttu-id="9dfad-114">Proto jsme odstranit tyto řádky:</span><span class="sxs-lookup"><span data-stu-id="9dfad-114">Thus, we delete these lines:</span></span>

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

<span data-ttu-id="9dfad-115">Nakonec jsme předávat `subnetId` parametr nastavený uživatelem (místo použití `resourceId` získat id virtuální sítě v jednom nasazení, který je co minimální přijatelná měřítko nastavit šablonu nemá).</span><span class="sxs-lookup"><span data-stu-id="9dfad-115">Finally, we pass in the `subnetId` parameter set by the user (instead of using `resourceId` to get the id of a vnet in the same deployment, which is what the minimum viable scale set template does).</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="9dfad-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9dfad-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
