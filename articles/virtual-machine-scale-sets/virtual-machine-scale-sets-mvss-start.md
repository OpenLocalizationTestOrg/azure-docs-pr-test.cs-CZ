---
title: "aaaLearn o škálování virtuálních počítačů nastavit šablony | Microsoft Docs"
description: "Další informace, že toocreate minimální přijatelná škálování nastavení šablony pro sady škálování virtuálního počítače"
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
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Další informace o šablonách sady škálování virtuálního počítače
[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) jsou skvělý způsob toodeploy skupiny související prostředky. Tato řada kurz ukazuje, jak toocreate minimální přijatelná škálování nastavit šablonu a toomodify této šablony toosuit různé scénáře. Všechny příklady pocházejí z tohoto [úložiště GitHub](https://github.com/gatneil/mvss). 

Tato šablona je určený toobe jednoduché. Podrobnější příklady škálování nastavení šablony najdete hello [úložiště GitHub šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) a vyhledejte složky, které obsahují řetězec hello `vmss`.

Pokud jste již obeznámeni s vytváření šablon, můžete přeskočit toohello toosee části "Další kroky" jak toomodify této šablony.

## <a name="review-hello-template"></a>Šablona kontrolní hello

Použít Githubu tooreview naše minimální přijatelná škálování nastavení šablony, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

V tomto kurzu jsme zkontrolujte hello diff (`git diff master minimum-viable-scale-set`) sady toocreate hello minimální přijatelná škálování šablony část podle část.

## <a name="define-schema-and-contentversion"></a>Zadejte $schema a contentVersion
Nejprve definujeme `$schema` a `contentVersion` v šabloně hello. Hello `$schema` element definuje hello verze jazyka šablony hello a používá se pro Visual Studio zvýraznění syntaxe a podobné funkce ověřování. Hello `contentVersion` element nepoužívá Azure. Místo toho pomáhá udržovat přehled o verzi šablony hello.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>Definujte parametry
V dalším kroku jsme definovali dva parametry `adminUsername` a `adminPassword`. Parametry jsou hodnoty, které zadáte během hello nasazení. Hello `adminUsername` parametr je jednoduše `string` typu, ale protože `adminPassword` je tajný klíč, jsme poskytněte typu `securestring`. Později tyto parametry se předávají do konfigurace sady škálování hello.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Definování proměnných
Šablony Resource Manageru vám také umožní definovat proměnné toobe použít později v šabloně hello. Naše ukázka nepoužívá všechny proměnné, takže jsme jste prázdné objekt JSON hello.

```json
  "variables": {},
```

## <a name="define-resources"></a>Definování zdrojů
Dále je oddílu prostředků hello hello šablony. Zde, určit, co je ve skutečnosti toodeploy. Na rozdíl od `parameters` a `variables` (které jsou objekty JSON), `resources` je JSON seznam objektů JSON.

```json
   "resources": [
```

Všechny zdroje vyžadují `type`, `name`, `apiVersion`, a `location` vlastnosti. První prostředek v tomto příkladu má typ `Microsft.Network/virtualNetwork`, název `myVnet`a apiVersion `2016-03-30`. (toofind hello nejnovější verzi rozhraní API pro typ prostředku, najdete v části hello [dokumentace k Azure REST API](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Zadejte umístění
toospecify hello umístění pro virtuální síť hello používáme [funkce šablony Resource Manageru](../azure-resource-manager/resource-group-template-functions.md). Tato funkce musí být uzavřena do uvozovek a hranaté závorky takto: `"[<template-function>]"`. V tomto případě používáme hello `resourceGroup` funkce. Přebírá v bez argumentů a vrátí objekt JSON s metadata o hello skupinu prostředků, které toto nasazení je nasazován na. Skupina prostředků Hello nastavena hello uživatelem v době hello nasazení. Jsme pak index do tohoto objektu JSON s `.location` tooget hello umístění z objektu JSON, hello.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Zadejte vlastnosti virtuální sítě
Každý prostředek, správce prostředků má svou vlastní `properties` části pro konkrétní toohello prostředek konfigurace. V takovém případě určíme tuto hello virtuální síť musí mít jednu podsíť pomocí rozsah hello privátních IP adres `10.0.0.0/16`. Sada škálování je vždy obsažené v jedné podsíti. Ho nemůže span podsítě.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Přidat seznam dependsOn
Kromě toho toohello požaduje `type`, `name`, `apiVersion`, a `location` vlastnosti každého prostředku může mít volitelný `dependsOn` seznamu řetězců. Tento seznam určuje, které další prostředky z tohoto nasazení musí dokončit před nasazením tohoto prostředku.

V seznamu hello hello virtuální sítě z předchozího příkladu hello je v tomto případě pouze jeden element. Tuto závislost určíme, protože hello sady škálování vyžaduje hello sítě tooexist před vytvořením všechny virtuální počítače. Tímto způsobem hello škálovací sadu mohou poskytnout tyto virtuální počítače privátních IP adres z rozsahu IP adres hello dříve zadanou ve vlastnosti sítě hello. Formát Hello každý řetězec v seznamu dependsOn hello je `<type>/<name>`. Použití hello stejné `type` a `name` použít dříve v definici prostředků hello virtuální sítě.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Zadejte vlastnosti sady škálování
Sady škálování mít mnoho vlastností pro přizpůsobení hello virtuální počítače ve škálovací sadě hello. Úplný seznam těchto vlastností najdete v tématu hello [škálovací sady dokumentace k REST API](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set). V tomto kurzu jsme nastaví jen několik běžně používané vlastností.
### <a name="supply-vm-size-and-capacity"></a>Zadejte velikost virtuálního počítače a kapacity
škálování Hello nastavit tooknow potřebám, jakou velikost virtuálního počítače toocreate ("název sku") a kolik takové toocreate virtuální počítače ("kapacita sku"). toosee které velikosti virtuálních počítačů jsou k dispozici, najdete v části hello [velikosti virtuálních počítačů dokumentaci](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Vyberte typ aktualizace
také je nutné Hello škálovací sadu tooknow jak toohandle aktualizuje na sadě škálování hello. V současné době existují dvě možnosti, `Manual` a `Automatic`. Další informace o hello rozdíly mezi hello dva, naleznete v dokumentaci k hello na [jak tooupgrade škálování nastavit](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Vyberte operační systém virtuálního počítače
Hello sady škálování potřebám tooknow tooput jaký operační systém na virtuálních počítačích hello. Zde vytvoříme hello virtuální počítače s bitovou kopii plně opravou 16.04 LTS Ubuntu.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Zadejte computerNamePrefix
Sada škálování Hello nasadí víc virtuálních počítačů. Místo zadání každý název virtuálního počítače, určíme `computerNamePrefix`. Hello škálovací sadu připojí předponu toohello index pro každý virtuální počítač, aby názvy virtuálních počítačů hello formuláře `<computerNamePrefix>_<auto-generated-index>`.

V hello následující fragment kódu použijeme hello parametry z před tooset hello jméno a heslo správce pro všechny virtuální počítače ve škálovací sadě hello. Provedeme to pomocí hello `parameters` funkce šablony. Tato funkce přebírá řetězec, který určuje, které tooand toorefer parametr výstupy hello hodnotu tohoto parametru.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>Zadejte konfigurace sítě virtuálních počítačů
Nakonec potřebujeme toospecify hello síťovou konfiguraci pro virtuální počítače hello v sadě škálování hello. V takovém případě potřebujeme pouze toospecify hello ID podsítě hello vytvořili dříve. Tato hodnota informuje, že hello sady škálování tooput hello síťových rozhraní v této podsíti.

Můžete získat ID hello hello virtuální sítě obsahující hello podsíť pomocí hello `resourceId` funkce šablony. Tato funkce přebírá hello typ a název prostředku a vrátí hello plně kvalifikovaný identifikátor prostředku. Toto ID má hello formuláře:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Ale hello identifikátor hello virtuální sítě není dostatečná. Je nutné zadat hello konkrétní podsítě, která hello škálovací sadu virtuálních počítačů by měla být ve. toodo, zřetězení `/subnets/mySubnet` toohello ID hello virtuální sítě. výsledek Hello je hello plně kvalifikovaný ID podsítě hello. Proveďte toto zřetězení s hello `concat` funkce, která přebírá v řadě řetězce a vrátí jejich zřetězení.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
