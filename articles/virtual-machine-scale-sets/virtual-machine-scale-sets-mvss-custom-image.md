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
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a><span data-ttu-id="5a909-103">Přidat že vlastní image tooan Azure škálování nastavení šablony</span><span class="sxs-lookup"><span data-stu-id="5a909-103">Add a custom image tooan Azure scale set template</span></span>

<span data-ttu-id="5a909-104">Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toodeploy z vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5a909-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy from custom image.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="5a909-105">Změna definice šablony hello</span><span class="sxs-lookup"><span data-stu-id="5a909-105">Change hello template definition</span></span>

<span data-ttu-id="5a909-106">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování sad z vlastní image hello škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="5a909-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set from a custom image can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span></span> <span data-ttu-id="5a909-107">Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set custom-image`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="5a909-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set custom-image`) piece by piece:</span></span>

### <a name="creating-a-managed-disk-image"></a><span data-ttu-id="5a909-108">Vytvoření image spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="5a909-108">Creating a managed disk image</span></span>

<span data-ttu-id="5a909-109">Pokud již máte vlastní spravovaných disků na obrázku (prostředek typu `Microsoft.Compute/images`), potom můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="5a909-109">If you already have a custom managed disk image (a resource of type `Microsoft.Compute/images`), then you can skip this section.</span></span>

<span data-ttu-id="5a909-110">Nejprve přidáme `sourceImageVhdUri` parametr, který je hello URI toohello zobecněn objektů blob v Azure Storage, který obsahuje vlastní image toodeploy hello z.</span><span class="sxs-lookup"><span data-stu-id="5a909-110">First, we add a `sourceImageVhdUri` parameter, which is hello URI toohello generalized blob in Azure Storage that contains hello custom image toodeploy from.</span></span>


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

<span data-ttu-id="5a909-111">Potom přidáme prostředek typu `Microsoft.Compute/images`, což je hello spravovaných disků na image založena na blob hello zobecněn nacházející se v identifikátoru URI `sourceImageVhdUri`.</span><span class="sxs-lookup"><span data-stu-id="5a909-111">Next, we add a resource of type `Microsoft.Compute/images`, which is hello managed disk image based on hello generalized blob located at URI `sourceImageVhdUri`.</span></span> <span data-ttu-id="5a909-112">Tento obrázek musí být ve hello stejné oblasti jako hello škálovací sadu, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="5a909-112">This image must be in hello same region as hello scale set that uses it.</span></span> <span data-ttu-id="5a909-113">Ve vlastnostech hello hello bitové kopie, můžeme zadat typ hello operačního systému, hello umístění objektu hello blob (z hello `sourceImageVhdUri` parametr) a typ účtu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="5a909-113">In hello properties of hello image, we specify hello OS type, hello location of hello blob (from hello `sourceImageVhdUri` parameter), and hello storage account type:</span></span>

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

<span data-ttu-id="5a909-114">V hello sady škálování prostředku, přidáme `dependsOn` klauzule odkazující toohello vlastní image toomake zda hello image získá vytvořili předtím, než hello škálovací sadu pokusí toodeploy z této bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="5a909-114">In hello scale set resource, we add a `dependsOn` clause referring toohello custom image toomake sure hello image gets created before hello scale set tries toodeploy from that image:</span></span>

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

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a><span data-ttu-id="5a909-115">Změna měřítka nastavit vlastnosti toouse hello spravovaných disků na obrázek</span><span class="sxs-lookup"><span data-stu-id="5a909-115">Changing scale set properties toouse hello managed disk image</span></span>

<span data-ttu-id="5a909-116">V hello `imageReference` hello měřítka nastavit `storageProfile`, místo zadání hello vydavatele, nabídky, sku a verzi image platformy, určíme hello `id` z hello `Microsoft.Compute/images` prostředků:</span><span class="sxs-lookup"><span data-stu-id="5a909-116">In hello `imageReference` of hello scale set `storageProfile`, instead of specifying hello publisher, offer, sku, and version of a platform image, we specify hello `id` of hello `Microsoft.Compute/images` resource:</span></span>

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

<span data-ttu-id="5a909-117">V tomto příkladu používáme hello `resourceId` funkce tooget hello ID prostředku bitové kopie hello vytvořené v hello stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="5a909-117">In this example, we use hello `resourceId` function tooget hello resource ID of hello image created in hello same template.</span></span> <span data-ttu-id="5a909-118">Pokud jste vytvořili image spravovaného disku hello předem, měli byste poskytnout hello id této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a909-118">If you have created hello managed disk image beforehand, you should provide hello id of that image instead.</span></span> <span data-ttu-id="5a909-119">Toto id musí být ve formátu hello: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span><span class="sxs-lookup"><span data-stu-id="5a909-119">This id must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5a909-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a909-120">Next Steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
