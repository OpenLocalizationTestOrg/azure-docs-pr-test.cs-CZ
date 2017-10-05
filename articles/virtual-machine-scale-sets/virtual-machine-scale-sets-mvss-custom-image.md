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
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a><span data-ttu-id="0f04c-103">Přidat vlastní image na šablonu sady Azure škálování</span><span class="sxs-lookup"><span data-stu-id="0f04c-103">Add a custom image to an Azure scale set template</span></span>

<span data-ttu-id="0f04c-104">Tento článek ukazuje, jak upravit [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) k nasazení z vlastní image.</span><span class="sxs-lookup"><span data-stu-id="0f04c-104">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to deploy from custom image.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="0f04c-105">Změna definice šablony</span><span class="sxs-lookup"><span data-stu-id="0f04c-105">Change the template definition</span></span>

<span data-ttu-id="0f04c-106">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablony pro nasazení měřítka, nastavte z vlastní image můžete vidět [zde](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="0f04c-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the scale set from a custom image can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span></span> <span data-ttu-id="0f04c-107">Podívejme se na rozdílové použít k vytvoření této šablony (`git diff minimum-viable-scale-set custom-image`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="0f04c-107">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set custom-image`) piece by piece:</span></span>

### <a name="creating-a-managed-disk-image"></a><span data-ttu-id="0f04c-108">Vytvoření image spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="0f04c-108">Creating a managed disk image</span></span>

<span data-ttu-id="0f04c-109">Pokud již máte vlastní spravovaných disků na obrázku (prostředek typu `Microsoft.Compute/images`), potom můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="0f04c-109">If you already have a custom managed disk image (a resource of type `Microsoft.Compute/images`), then you can skip this section.</span></span>

<span data-ttu-id="0f04c-110">Nejprve přidáme `sourceImageVhdUri` parametr, který je identifikátor URI pro zobecněný objektu blob ve službě Azure Storage, který obsahuje vlastní image pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="0f04c-110">First, we add a `sourceImageVhdUri` parameter, which is the URI to the generalized blob in Azure Storage that contains the custom image to deploy from.</span></span>


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

<span data-ttu-id="0f04c-111">Potom přidáme prostředek typu `Microsoft.Compute/images`, které je založené na zobecněný umístěné v identifikátoru URI objektu blob bitové kopie spravovaného disku `sourceImageVhdUri`.</span><span class="sxs-lookup"><span data-stu-id="0f04c-111">Next, we add a resource of type `Microsoft.Compute/images`, which is the managed disk image based on the generalized blob located at URI `sourceImageVhdUri`.</span></span> <span data-ttu-id="0f04c-112">Tato bitová kopie musí být ve stejné oblasti jako sada škálování, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="0f04c-112">This image must be in the same region as the scale set that uses it.</span></span> <span data-ttu-id="0f04c-113">Ve vlastnostech bitovou kopii, určíme typ operačního systému, umístění objektu blob (z `sourceImageVhdUri` parametr) a typ účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="0f04c-113">In the properties of the image, we specify the OS type, the location of the blob (from the `sourceImageVhdUri` parameter), and the storage account type:</span></span>

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

<span data-ttu-id="0f04c-114">V měřítka nastavení prostředku, přidáme `dependsOn` klauzule odkazující na vlastní obrázek, který má zkontrolujte, zda se vytvoří před měřítka pokusí nasazení z této bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="0f04c-114">In the scale set resource, we add a `dependsOn` clause referring to the custom image to make sure the image gets created before the scale set tries to deploy from that image:</span></span>

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

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a><span data-ttu-id="0f04c-115">Změna měřítka nastavit vlastnosti používat bitovou kopii, spravovaný disku</span><span class="sxs-lookup"><span data-stu-id="0f04c-115">Changing scale set properties to use the managed disk image</span></span>

<span data-ttu-id="0f04c-116">V `imageReference` měřítka nastavit `storageProfile`, místo zadání vydavatele, nabídky, sku a verzi image platformy, určíme `id` z `Microsoft.Compute/images` prostředků:</span><span class="sxs-lookup"><span data-stu-id="0f04c-116">In the `imageReference` of the scale set `storageProfile`, instead of specifying the publisher, offer, sku, and version of a platform image, we specify the `id` of the `Microsoft.Compute/images` resource:</span></span>

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

<span data-ttu-id="0f04c-117">V tomto příkladu používáme `resourceId` funkce získat ID prostředku bitové kopie vytvořené v stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="0f04c-117">In this example, we use the `resourceId` function to get the resource ID of the image created in the same template.</span></span> <span data-ttu-id="0f04c-118">Pokud jste vytvořili bitové kopie disku spravované předem, měli byste poskytnout id této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0f04c-118">If you have created the managed disk image beforehand, you should provide the id of that image instead.</span></span> <span data-ttu-id="0f04c-119">Toto id musí být ve tvaru: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span><span class="sxs-lookup"><span data-stu-id="0f04c-119">This id must be of the form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0f04c-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f04c-120">Next Steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]