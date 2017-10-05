---
title: "Odkazovat na vlastní image ve šablonu sady Azure škálování | Microsoft Docs"
description: "Zjistěte, jak přidat vlastní image do stávající šablony sadu škálování virtuálního počítače Azure"
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
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: cf52fc9e95267c4bc5c0106aadf626685ddd5c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Přidat vlastní image na šablonu sady Azure škálování

Tento článek ukazuje, jak upravit [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) k nasazení z vlastní image.

## <a name="change-the-template-definition"></a>Změna definice šablony

Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablony pro nasazení měřítka, nastavte z vlastní image můžete vidět [zde](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Podívejme se na rozdílové použít k vytvoření této šablony (`git diff minimum-viable-scale-set custom-image`) část podle část:

### <a name="creating-a-managed-disk-image"></a>Vytvoření image spravovaných disků

Pokud již máte vlastní spravovaných disků na obrázku (prostředek typu `Microsoft.Compute/images`), potom můžete tuto část přeskočit.

Nejprve přidáme `sourceImageVhdUri` parametr, který je identifikátor URI pro zobecněný objektu blob ve službě Azure Storage, který obsahuje vlastní image pro nasazení.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Potom přidáme prostředek typu `Microsoft.Compute/images`, které je založené na zobecněný umístěné v identifikátoru URI objektu blob bitové kopie spravovaného disku `sourceImageVhdUri`. Tato bitová kopie musí být ve stejné oblasti jako sada škálování, která jej používá. Ve vlastnostech bitovou kopii, určíme typ operačního systému, umístění objektu blob (z `sourceImageVhdUri` parametr) a typ účtu úložiště:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

V měřítka nastavení prostředku, přidáme `dependsOn` klauzule odkazující na vlastní obrázek, který má zkontrolujte, zda se vytvoří před měřítka pokusí nasazení z této bitové kopie:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Změna měřítka nastavit vlastnosti používat bitovou kopii, spravovaný disku

V `imageReference` měřítka nastavit `storageProfile`, místo zadání vydavatele, nabídky, sku a verzi image platformy, určíme `id` z `Microsoft.Compute/images` prostředků:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

V tomto příkladu používáme `resourceId` funkce získat ID prostředku bitové kopie vytvořené v stejné šablony. Pokud jste vytvořili bitové kopie disku spravované předem, měli byste poskytnout id této bitové kopie. Toto id musí být ve tvaru: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
