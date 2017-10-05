---
title: "Převést šablonu Azure Resource Manager škálování sadu používat spravovaných disků na | Microsoft Docs"
description: "Převeďte na šablonu sady škálování spravovaných disků na šablonu sady škálování."
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
ms.openlocfilehash: 2f5cb85703888c5056611d466f508547ee72e44b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a><span data-ttu-id="f7d0a-104">Převést na šablonu sady škálování spravovaných disků na šablonu sady škálování</span><span class="sxs-lookup"><span data-stu-id="f7d0a-104">Convert a scale set template to a managed disk scale set template</span></span>

<span data-ttu-id="f7d0a-105">Zákazníci pomocí šablony Resource Manageru pro vytváření škálovací sadu nepoužíváte spravovaných disků na chtít upravit tak, aby pomocí spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-105">Customers with a Resource Manager template for creating a scale set not using managed disk may wish to modify it to use managed disk.</span></span> <span data-ttu-id="f7d0a-106">Tento článek ukazuje, jak na to, jako příklad použijeme žádost o přijetí změn z [šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates), úložišti komunitou vytvářený pro ukázkové šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-106">This article shows how to do this, using as an example a pull request from the [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates), a community-driven repo for sample Resource Manager templates.</span></span> <span data-ttu-id="f7d0a-107">Zde můžete zjistit úplné přijetí žádosti o: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), a jsou v příslušných částech rozdílové níže, společně s vysvětlení:</span><span class="sxs-lookup"><span data-stu-id="f7d0a-107">The full pull request can be seen here: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), and the relevant parts of the diff are below, along with explanations:</span></span>

## <a name="making-the-os-disks-managed"></a><span data-ttu-id="f7d0a-108">Provedení spravované disky operačního systému</span><span class="sxs-lookup"><span data-stu-id="f7d0a-108">Making the OS disks managed</span></span>

<span data-ttu-id="f7d0a-109">V rozdílů, které jsou níže jsme se zobrazí, že jsme odebrali několika proměnných související s vlastností účtu a disku úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-109">In the diff below, we can see that we have removed several variables related to storage account and disk properties.</span></span> <span data-ttu-id="f7d0a-110">Typ účtu úložiště už není nutné (Standard_LRS je výchozí nastavení), ale může stále určíme ho Pokud jsme nepřeje.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-110">Storage account type is no longer necessary (Standard_LRS is the default), but we could still specify it if we wished to.</span></span> <span data-ttu-id="f7d0a-111">Jsou podporovány pouze Standard_LRS a Premium_LRS s spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-111">Only Standard_LRS and Premium_LRS are supported with managed disk.</span></span> <span data-ttu-id="f7d0a-112">Nová přípona účet úložiště, pole jedinečné řetězce a počet sa byly použity v původní šabloně vygenerovat názvy účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-112">New storage account suffix, unique string array, and sa count were used in the old template to generate storage account names.</span></span> <span data-ttu-id="f7d0a-113">Tyto proměnné již nejsou potřebné v nové šabloně protože spravovaných disků na účty úložiště automaticky vytvoří jménem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-113">These variables are no longer necessary in the new template because managed disk automatically creates storage accounts on the customer's behalf.</span></span> <span data-ttu-id="f7d0a-114">Podobně název kontejneru virtuálního pevného disku a název disku operačního systému již nejsou potřebné protože spravovaných disků na názvy automaticky základní kontejnery úložiště objektů blob a disků.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-114">Similarly, vhd container name and os disk name are no longer necessary because managed disk automatically names the underlying storage blob containers and disks.</span></span>

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


<span data-ttu-id="f7d0a-115">V rozdílů, které jsou níže, vidíme, že byl aktualizován výpočetní verze rozhraní api pro 2016-04-30-preview, což je nejdříve požadovaná verze spravovaných disků na podporu pro sady škálování.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-115">In the diff below, we can see that we updated the compute api version to 2016-04-30-preview, which is the earliest required version for managed disk support with scale sets.</span></span> <span data-ttu-id="f7d0a-116">Upozorňujeme, že jsme může stále použít nespravované disky v nové verzi rozhraní api pomocí staré syntaxe v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-116">Note that we could still use unmanaged disks in the new api version with the old syntax if desired.</span></span> <span data-ttu-id="f7d0a-117">Jinými slovy Pokud jsme výpočetní aktualizovat pouze verze rozhraní api a neměnit cokoliv jiného, šablona by měly být nadále fungovat jako předtím.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-117">In other words, if we only update the compute api version and don't change anything else, the template should continue to work as before.</span></span>

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

<span data-ttu-id="f7d0a-118">V rozdílů, které jsou níže jsme se zobrazí, že jsme odebírání prostředků účtu úložiště z pole prostředkům úplně.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-118">In the diff below, we can see that we are removing the storage account resource from the resources array completely.</span></span> <span data-ttu-id="f7d0a-119">Už potřebujeme je vzhledem k tomu, že spravovaných disků je automaticky vytvoří naším jménem.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-119">We no longer need them since managed disk creates them automatically on our behalf.</span></span>

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

<span data-ttu-id="f7d0a-120">V rozdílů, které jsou níže, uvidíme jsme odebrání závisí na klauzule odkazující od stupnice nastavena na smyčky, která byla vytváření účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-120">In the diff below, we can see that we are removing the depends on clause referring from the scale set to the loop that was creating storage accounts.</span></span> <span data-ttu-id="f7d0a-121">V původní šabloně to byla zajistit, aby byly účty úložiště vytvořené před škálovací sadu začal vytvoření, ale tuto klauzuli už není nutné u spravovaných disků na.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-121">In the old template, this was ensuring that the storage accounts were created before the scale set began creation, but this clause is no longer necessary with managed disk.</span></span> <span data-ttu-id="f7d0a-122">Jsme také odstranit vlastnost kontejnery vhd a vlastnost název disku operačního systému jako tyto vlastnosti jsou automaticky zpracovávány pod pokličkou spravovaného disku.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-122">We also remove the vhd containers property, and the os disk name property as these properties are automatically handled under the hood by managed disk.</span></span> <span data-ttu-id="f7d0a-123">Pokud jsme si přáli, může přidáme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v konfiguraci "osDisk" Pokud jsme chtěli prémiové disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-123">If we wished, we could add `"managedDisk": { "storageAccountType": "Premium_LRS" }` in the "osDisk" configuration if we wanted premium OS disks.</span></span> <span data-ttu-id="f7d0a-124">Pouze virtuální počítače s velkým nebo na malá ' ve virtuálním počítači můžete použít sku prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-124">Only VMs with an uppercase or lowercase 's' in the VM sku can use premium disks.</span></span>

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

<span data-ttu-id="f7d0a-125">Neexistuje žádné explicitní vlastnost v konfiguraci sady škálování pro jestli se má používat disku spravované nebo nespravované.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-125">There is no explicit property in the scale set configuration for whether to use managed or unmanaged disk.</span></span> <span data-ttu-id="f7d0a-126">Sada škálování ví, který se použije na základě vlastností, které se nacházejí v profilu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-126">The scale set knows which to use based on the properties that are present in the storage profile.</span></span> <span data-ttu-id="f7d0a-127">Proto je důležité při úpravě šablonu, zajistěte, aby vlastnosti oprávnění v profilu úložiště škálovací sady.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-127">Thus, it is important when modifying the template to ensure that the right properties are in the storage profile of the scale set.</span></span>


## <a name="data-disks"></a><span data-ttu-id="f7d0a-128">Datové disky</span><span class="sxs-lookup"><span data-stu-id="f7d0a-128">Data disks</span></span>

<span data-ttu-id="f7d0a-129">S výše změny škálování sady používá spravované disky pro operační systém na disku, ale co o datových disků?</span><span class="sxs-lookup"><span data-stu-id="f7d0a-129">With the changes above, the scale set uses managed disks for the OS disk, but what about data disks?</span></span> <span data-ttu-id="f7d0a-130">Chcete-li přidat datových disků, přidejte vlastnost "dataDisks" v části "storageProfile" na stejné úrovni jako "osDisk".</span><span class="sxs-lookup"><span data-stu-id="f7d0a-130">To add data disks, add the "dataDisks" property under "storageProfile" at the same level as "osDisk".</span></span> <span data-ttu-id="f7d0a-131">Hodnota vlastnosti je JSON seznam objektů, z nichž každá má vlastnosti "lun" (které musí být jedinečný pro každé datový disk na virtuálním počítači), "createOption" ("prázdný" je aktuálně jedinou možností podporovaných) a "diskSizeGB" (velikost disku v gigabajtech; musí být větší než 0 a menší než 1024) jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f7d0a-131">The value of the property is a JSON list of objects, each of which has properties "lun" (which must be unique per data disk on a VM), "createOption" ("empty" is currently the only supported option), and "diskSizeGB" (the size of the disk in gigabytes; must be greater than 0 and less than 1024) as in the following example:</span></span> 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

<span data-ttu-id="f7d0a-132">Pokud zadáte `n` disky v toto pole, jednotlivé virtuální počítače v měřítka nastavit získá `n` datových disků.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-132">If you specify `n` disks in this array, each VM in the scale set gets `n` data disks.</span></span> <span data-ttu-id="f7d0a-133">Upozorňujeme však, že jsou tyto datové disky nezpracované zařízení.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-133">Do note, however, that these data disks are raw devices.</span></span> <span data-ttu-id="f7d0a-134">Jejich nejsou formátovány.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-134">They are not formatted.</span></span> <span data-ttu-id="f7d0a-135">Je zákazník připojit, paritition a před jejich použitím disky naformátujte.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-135">It is up to the customer to attach, paritition, and format the disks before using them.</span></span> <span data-ttu-id="f7d0a-136">Volitelně může také určíme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v každý datový disk objekt k určení, že by měl být premium datový disk.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-136">Optionally, we could also specify `"managedDisk": { "storageAccountType": "Premium_LRS" }` in each data disk object to specify that it should be a premium data disk.</span></span> <span data-ttu-id="f7d0a-137">Pouze virtuální počítače s velkým nebo na malá ' ve virtuálním počítači můžete použít sku prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="f7d0a-137">Only VMs with an uppercase or lowercase 's' in the VM sku can use premium disks.</span></span>

<span data-ttu-id="f7d0a-138">Další informace o datových disků pomocí sady škálování najdete v tématu [v tomto článku](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="f7d0a-138">To learn more about using data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="f7d0a-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7d0a-139">Next steps</span></span>
<span data-ttu-id="f7d0a-140">Například šablony Resource Manageru pomocí sad škálování, vyhledejte "vmss" v [úložiště github šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="f7d0a-140">For example Resource Manager templates using scale sets, search for "vmss" in the [Azure Quickstart Templates github repo](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="f7d0a-141">Obecné informace, podívejte se [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="f7d0a-141">For general information, check out the [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

