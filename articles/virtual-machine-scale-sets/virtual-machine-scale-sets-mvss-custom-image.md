---
title: "Odkazovat na vlastní image ve šablonu sady Azure škálování | Microsoft Docs"
description: "Zjistěte, jak tooadd vlastní obrázek tooan existující šablonu Azure Škálovací sadu virtuálních počítačů"
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
ms.openlocfilehash: 6a17d989e44d241b460238c0106350c3ef038e56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a>Přidat že vlastní image tooan Azure škálování nastavení šablony

Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toodeploy z vlastní image.

## <a name="change-hello-template-definition"></a>Změna definice šablony hello

Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování sad z vlastní image hello škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set custom-image`) část podle část:

### <a name="creating-a-managed-disk-image"></a>Vytvoření image spravovaných disků

Pokud již máte vlastní spravovaných disků na obrázku (prostředek typu `Microsoft.Compute/images`), potom můžete tuto část přeskočit.

Nejprve přidáme `sourceImageVhdUri` parametr, který je hello URI toohello zobecněn objektů blob v Azure Storage, který obsahuje vlastní image toodeploy hello z.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "hello source of hello generalized blob containing hello custom image"
+      }
     }
   },
   "variables": {},
```

Potom přidáme prostředek typu `Microsoft.Compute/images`, což je hello spravovaných disků na image založena na blob hello zobecněn nacházející se v identifikátoru URI `sourceImageVhdUri`. Tento obrázek musí být ve hello stejné oblasti jako hello škálovací sadu, která jej používá. Ve vlastnostech hello hello bitové kopie, můžeme zadat typ hello operačního systému, hello umístění objektu hello blob (z hello `sourceImageVhdUri` parametr) a typ účtu úložiště hello:

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

V hello sady škálování prostředku, přidáme `dependsOn` klauzule odkazující toohello vlastní image toomake zda hello image získá vytvořili předtím, než hello škálovací sadu pokusí toodeploy z této bitové kopie:

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

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a>Změna měřítka nastavit vlastnosti toouse hello spravovaných disků na obrázek

V hello `imageReference` hello měřítka nastavit `storageProfile`, místo zadání hello vydavatele, nabídky, sku a verzi image platformy, určíme hello `id` z hello `Microsoft.Compute/images` prostředků:

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

V tomto příkladu používáme hello `resourceId` funkce tooget hello ID prostředku bitové kopie hello vytvořené v hello stejné šablony. Pokud jste vytvořili image spravovaného disku hello předem, měli byste poskytnout hello id této bitové kopie. Toto id musí být ve formátu hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
