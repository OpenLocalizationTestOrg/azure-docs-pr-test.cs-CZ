---
title: "aaaConvert škálování Azure Resource Manager nastavte spravovaných disků na šablony toouse | Microsoft Docs"
description: "Převeďte šablonu tooa spravovaných disků na škálování sada škálování sada šablony."
keywords: "Sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: negat
ms.openlocfilehash: 66c2217647e57ed2cfa39660c0175710ae2e63be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a><span data-ttu-id="190be-104">Převést šablonu tooa spravovaných disků na škálování sada škálování sada šablony</span><span class="sxs-lookup"><span data-stu-id="190be-104">Convert a scale set template tooa managed disk scale set template</span></span>

<span data-ttu-id="190be-105">Zákazníci pomocí šablony Resource Manageru pro vytváření škálování nastavit nepoužíváte spravovaných disků může přát toomodify ho toouse spravované disku.</span><span class="sxs-lookup"><span data-stu-id="190be-105">Customers with a Resource Manager template for creating a scale set not using managed disk may wish toomodify it toouse managed disk.</span></span> <span data-ttu-id="190be-106">Tento článek ukazuje, jak toodo se používá jako příklad žádost o přijetí změn z hello [šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates), úložišti komunitou vytvářený pro ukázkové šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="190be-106">This article shows how toodo this, using as an example a pull request from hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates), a community-driven repo for sample Resource Manager templates.</span></span> <span data-ttu-id="190be-107">Zde můžete zjistit žádost o úplné přijetí změn Hello: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), a odpovídající části hello hello rozdílové jsou níže, společně s vysvětlení:</span><span class="sxs-lookup"><span data-stu-id="190be-107">hello full pull request can be seen here: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), and hello relevant parts of hello diff are below, along with explanations:</span></span>

## <a name="making-hello-os-disks-managed"></a><span data-ttu-id="190be-108">Vytváření disků operačního systému hello spravované</span><span class="sxs-lookup"><span data-stu-id="190be-108">Making hello OS disks managed</span></span>

<span data-ttu-id="190be-109">V rozdílové hello níže jsme se zobrazí, jsme odebrali několik proměnné související toostorage vlastností účtu a disku.</span><span class="sxs-lookup"><span data-stu-id="190be-109">In hello diff below, we can see that we have removed several variables related toostorage account and disk properties.</span></span> <span data-ttu-id="190be-110">Typ účtu úložiště už není nutné (Standard_LRS je výchozí hello), ale může stále určíme ho Pokud jsme nepřeje.</span><span class="sxs-lookup"><span data-stu-id="190be-110">Storage account type is no longer necessary (Standard_LRS is hello default), but we could still specify it if we wished to.</span></span> <span data-ttu-id="190be-111">Jsou podporovány pouze Standard_LRS a Premium_LRS s spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="190be-111">Only Standard_LRS and Premium_LRS are supported with managed disk.</span></span> <span data-ttu-id="190be-112">Nová přípona účet úložiště, pole jedinečné řetězce a počet sa byly používány v hello původní šablony toogenerate názvy účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="190be-112">New storage account suffix, unique string array, and sa count were used in hello old template toogenerate storage account names.</span></span> <span data-ttu-id="190be-113">Tyto proměnné již nejsou potřebné v nové šabloně hello protože spravovaných disků na účty úložiště automaticky vytvoří jménem zákazníka hello.</span><span class="sxs-lookup"><span data-stu-id="190be-113">These variables are no longer necessary in hello new template because managed disk automatically creates storage accounts on hello customer's behalf.</span></span> <span data-ttu-id="190be-114">Podobně název kontejneru virtuálního pevného disku a název disku operačního systému již nejsou potřebné protože spravovaných disků na názvy automaticky hello základní kontejnery úložiště objektů blob a disků.</span><span class="sxs-lookup"><span data-stu-id="190be-114">Similarly, vhd container name and os disk name are no longer necessary because managed disk automatically names hello underlying storage blob containers and disks.</span></span>

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


<span data-ttu-id="190be-115">V hello rozdílové níže, jsme můžete najdete, že byl aktualizován hello výpočetní rozhraní api verze too2016-04-30-preview, což je hello nejdříve požadovaná verze spravovaných disků na podporu pro sady škálování.</span><span class="sxs-lookup"><span data-stu-id="190be-115">In hello diff below, we can see that we updated hello compute api version too2016-04-30-preview, which is hello earliest required version for managed disk support with scale sets.</span></span> <span data-ttu-id="190be-116">Upozorňujeme, že jsme může stále použít nespravované disky v nové verzi rozhraní api hello pomocí staré syntaxe hello v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="190be-116">Note that we could still use unmanaged disks in hello new api version with hello old syntax if desired.</span></span> <span data-ttu-id="190be-117">Jinými slovy Pokud jsme aktualizovat pouze hello výpočetní verze rozhraní api a neměnit cokoliv jiného, hello šablony by měly pokračovat ve toowork jako před.</span><span class="sxs-lookup"><span data-stu-id="190be-117">In other words, if we only update hello compute api version and don't change anything else, hello template should continue toowork as before.</span></span>

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

<span data-ttu-id="190be-118">V rozdílové hello níže jsme se zobrazí, že jsme odebírání prostředků účtu úložiště hello z pole prostředky hello úplně.</span><span class="sxs-lookup"><span data-stu-id="190be-118">In hello diff below, we can see that we are removing hello storage account resource from hello resources array completely.</span></span> <span data-ttu-id="190be-119">Už potřebujeme je vzhledem k tomu, že spravovaných disků je automaticky vytvoří naším jménem.</span><span class="sxs-lookup"><span data-stu-id="190be-119">We no longer need them since managed disk creates them automatically on our behalf.</span></span>

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

<span data-ttu-id="190be-120">V hello rozdílové níže, jsme můžete najdete odebírání hello závisí na klauzule odkazující hello škálování sadu toohello smyčky, který byl vytváření účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="190be-120">In hello diff below, we can see that we are removing hello depends on clause referring from hello scale set toohello loop that was creating storage accounts.</span></span> <span data-ttu-id="190be-121">V původní šabloně hello to byla zajistíte, že účty úložiště hello byly vytvořeny před hello škálovací sadu začal vytvoření, ale tuto klauzuli už není nutné u spravovaných disků na.</span><span class="sxs-lookup"><span data-stu-id="190be-121">In hello old template, this was ensuring that hello storage accounts were created before hello scale set began creation, but this clause is no longer necessary with managed disk.</span></span> <span data-ttu-id="190be-122">Jsme také odeberte vlastnost kontejnery vhd hello a hello vlastnost název disku operačního systému, jak tyto vlastnosti jsou automaticky zpracovávány pod pokličkou hello spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="190be-122">We also remove hello vhd containers property, and hello os disk name property as these properties are automatically handled under hello hood by managed disk.</span></span> <span data-ttu-id="190be-123">Pokud jsme si přáli, může přidáme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v konfiguraci "osDisk" hello, pokud jsme chtěli prémiové disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="190be-123">If we wished, we could add `"managedDisk": { "storageAccountType": "Premium_LRS" }` in hello "osDisk" configuration if we wanted premium OS disks.</span></span> <span data-ttu-id="190be-124">Pouze virtuální počítače s velkým nebo na malá ' v hello virtuálních počítačů můžete použít sku prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="190be-124">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

<span data-ttu-id="190be-125">Konfigurace sady škálování hello jestli toouse spravované nebo nespravované disku není žádný explicitní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="190be-125">There is no explicit property in hello scale set configuration for whether toouse managed or unmanaged disk.</span></span> <span data-ttu-id="190be-126">Sada škálování Hello ví, které toouse na základě hello vlastností, které se nacházejí v profilu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="190be-126">hello scale set knows which toouse based on hello properties that are present in hello storage profile.</span></span> <span data-ttu-id="190be-127">Proto je důležité při úpravě tooensure hello šablony, které vlastnosti hello oprávnění jsou v profilu úložiště hello škálovací sady hello.</span><span class="sxs-lookup"><span data-stu-id="190be-127">Thus, it is important when modifying hello template tooensure that hello right properties are in hello storage profile of hello scale set.</span></span>


## <a name="data-disks"></a><span data-ttu-id="190be-128">Datové disky</span><span class="sxs-lookup"><span data-stu-id="190be-128">Data disks</span></span>

<span data-ttu-id="190be-129">Změny hello výše hello škálování sady používá spravovaných disků pro hello operačního systému na disku, ale co o datových disků? tooadd datových disků, přidejte vlastnost hello "dataDisks" v části "storageProfile" na stejné úrovni jako "osDisk" hello.</span><span class="sxs-lookup"><span data-stu-id="190be-129">With hello changes above, hello scale set uses managed disks for hello OS disk, but what about data disks? tooadd data disks, add hello "dataDisks" property under "storageProfile" at hello same level as "osDisk".</span></span> <span data-ttu-id="190be-130">Hello hello vlastnost hodnotu JSON seznam objektů, z nichž každá má vlastnosti "lun" (které musí být jedinečný pro každé datový disk na virtuálním počítači), "createOption" ("prázdná" je aktuálně hello jen podporované možnosti) a "diskSizeGB" (hello velikost hello disku v gigabajtech; musí být větší než 0 a menší než 1024) jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="190be-130">hello value of hello property is a JSON list of objects, each of which has properties "lun" (which must be unique per data disk on a VM), "createOption" ("empty" is currently hello only supported option), and "diskSizeGB" (hello size of hello disk in gigabytes; must be greater than 0 and less than 1024) as in hello following example:</span></span> 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

<span data-ttu-id="190be-131">Pokud zadáte `n` disky v toto pole, jednotlivé virtuální počítače ve škálovací hello nastavit získá `n` datových disků.</span><span class="sxs-lookup"><span data-stu-id="190be-131">If you specify `n` disks in this array, each VM in hello scale set gets `n` data disks.</span></span> <span data-ttu-id="190be-132">Upozorňujeme však, že jsou tyto datové disky nezpracované zařízení.</span><span class="sxs-lookup"><span data-stu-id="190be-132">Do note, however, that these data disks are raw devices.</span></span> <span data-ttu-id="190be-133">Jejich nejsou formátovány.</span><span class="sxs-lookup"><span data-stu-id="190be-133">They are not formatted.</span></span> <span data-ttu-id="190be-134">Je tooattach toohello zákazníka, paritition a formát hello disků před jejich používání.</span><span class="sxs-lookup"><span data-stu-id="190be-134">It is up toohello customer tooattach, paritition, and format hello disks before using them.</span></span> <span data-ttu-id="190be-135">Volitelně může také určíme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v každé data toospecify objektu disku, je nutné premium datový disk.</span><span class="sxs-lookup"><span data-stu-id="190be-135">Optionally, we could also specify `"managedDisk": { "storageAccountType": "Premium_LRS" }` in each data disk object toospecify that it should be a premium data disk.</span></span> <span data-ttu-id="190be-136">Pouze virtuální počítače s velkým nebo na malá ' v hello virtuálních počítačů můžete použít sku prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="190be-136">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

<span data-ttu-id="190be-137">toolearn Další informace o datových disků pomocí sad škálování, najdete v části [v tomto článku](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="190be-137">toolearn more about using data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="190be-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="190be-138">Next steps</span></span>
<span data-ttu-id="190be-139">Například šablony Resource Manageru pomocí sad škálování, vyhledejte "vmss" v hello [úložiště github šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="190be-139">For example Resource Manager templates using scale sets, search for "vmss" in hello [Azure Quickstart Templates github repo](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="190be-140">Obecné informace, podívejte se na hello [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="190be-140">For general information, check out hello [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

