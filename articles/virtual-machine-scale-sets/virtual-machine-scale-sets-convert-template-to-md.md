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
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a>Převést šablonu tooa spravovaných disků na škálování sada škálování sada šablony

Zákazníci pomocí šablony Resource Manageru pro vytváření škálování nastavit nepoužíváte spravovaných disků může přát toomodify ho toouse spravované disku. Tento článek ukazuje, jak toodo se používá jako příklad žádost o přijetí změn z hello [šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates), úložišti komunitou vytvářený pro ukázkové šablony Resource Manageru. Zde můžete zjistit žádost o úplné přijetí změn Hello: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), a odpovídající části hello hello rozdílové jsou níže, společně s vysvětlení:

## <a name="making-hello-os-disks-managed"></a>Vytváření disků operačního systému hello spravované

V rozdílové hello níže jsme se zobrazí, jsme odebrali několik proměnné související toostorage vlastností účtu a disku. Typ účtu úložiště už není nutné (Standard_LRS je výchozí hello), ale může stále určíme ho Pokud jsme nepřeje. Jsou podporovány pouze Standard_LRS a Premium_LRS s spravovaného disku. Nová přípona účet úložiště, pole jedinečné řetězce a počet sa byly používány v hello původní šablony toogenerate názvy účtů úložiště. Tyto proměnné již nejsou potřebné v nové šabloně hello protože spravovaných disků na účty úložiště automaticky vytvoří jménem zákazníka hello. Podobně název kontejneru virtuálního pevného disku a název disku operačního systému již nejsou potřebné protože spravovaných disků na názvy automaticky hello základní kontejnery úložiště objektů blob a disků.

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


V hello rozdílové níže, jsme můžete najdete, že byl aktualizován hello výpočetní rozhraní api verze too2016-04-30-preview, což je hello nejdříve požadovaná verze spravovaných disků na podporu pro sady škálování. Upozorňujeme, že jsme může stále použít nespravované disky v nové verzi rozhraní api hello pomocí staré syntaxe hello v případě potřeby. Jinými slovy Pokud jsme aktualizovat pouze hello výpočetní verze rozhraní api a neměnit cokoliv jiného, hello šablony by měly pokračovat ve toowork jako před.

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

V rozdílové hello níže jsme se zobrazí, že jsme odebírání prostředků účtu úložiště hello z pole prostředky hello úplně. Už potřebujeme je vzhledem k tomu, že spravovaných disků je automaticky vytvoří naším jménem.

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

V hello rozdílové níže, jsme můžete najdete odebírání hello závisí na klauzule odkazující hello škálování sadu toohello smyčky, který byl vytváření účtů úložiště. V původní šabloně hello to byla zajistíte, že účty úložiště hello byly vytvořeny před hello škálovací sadu začal vytvoření, ale tuto klauzuli už není nutné u spravovaných disků na. Jsme také odeberte vlastnost kontejnery vhd hello a hello vlastnost název disku operačního systému, jak tyto vlastnosti jsou automaticky zpracovávány pod pokličkou hello spravovaného disku. Pokud jsme si přáli, může přidáme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v konfiguraci "osDisk" hello, pokud jsme chtěli prémiové disky operačního systému. Pouze virtuální počítače s velkým nebo na malá ' v hello virtuálních počítačů můžete použít sku prémiové disky.

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

Konfigurace sady škálování hello jestli toouse spravované nebo nespravované disku není žádný explicitní vlastnost. Sada škálování Hello ví, které toouse na základě hello vlastností, které se nacházejí v profilu úložiště hello. Proto je důležité při úpravě tooensure hello šablony, které vlastnosti hello oprávnění jsou v profilu úložiště hello škálovací sady hello.


## <a name="data-disks"></a>Datové disky

Změny hello výše hello škálování sady používá spravovaných disků pro hello operačního systému na disku, ale co o datových disků? tooadd datových disků, přidejte vlastnost hello "dataDisks" v části "storageProfile" na stejné úrovni jako "osDisk" hello. Hello hello vlastnost hodnotu JSON seznam objektů, z nichž každá má vlastnosti "lun" (které musí být jedinečný pro každé datový disk na virtuálním počítači), "createOption" ("prázdná" je aktuálně hello jen podporované možnosti) a "diskSizeGB" (hello velikost hello disku v gigabajtech; musí být větší než 0 a menší než 1024) jako v hello následující ukázka: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Pokud zadáte `n` disky v toto pole, jednotlivé virtuální počítače ve škálovací hello nastavit získá `n` datových disků. Upozorňujeme však, že jsou tyto datové disky nezpracované zařízení. Jejich nejsou formátovány. Je tooattach toohello zákazníka, paritition a formát hello disků před jejich používání. Volitelně může také určíme `"managedDisk": { "storageAccountType": "Premium_LRS" }` v každé data toospecify objektu disku, je nutné premium datový disk. Pouze virtuální počítače s velkým nebo na malá ' v hello virtuálních počítačů můžete použít sku prémiové disky.

toolearn Další informace o datových disků pomocí sad škálování, najdete v části [v tomto článku](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Další kroky
Například šablony Resource Manageru pomocí sad škálování, vyhledejte "vmss" v hello [úložiště github šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).

Obecné informace, podívejte se na hello [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

