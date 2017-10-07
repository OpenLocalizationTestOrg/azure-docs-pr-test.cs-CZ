---
title: "aaaAzure Resource Manager šablony funkce – logické | Microsoft Docs"
description: "Popisuje funkce toouse hello v toodetermine šablony Azure Resource Manageru pro logické hodnoty."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: aec6341fbde00b4eba3b4539ff9a9aec774333fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logické funkce pro šablony Azure Resource Manager

Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.

* [a](#and)
* [BOOL](#bool)
* [Pokud](#if)
* [není](#not)
* [nebo](#or)

## <a name="and"></a>a
`and(arg1, arg2)`

Kontroluje, zda jsou true obě hodnoty parametru.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |Logická hodnota |tom, zda text Hello toocheck první hodnota je true. |
| arg2 |Ano |Logická hodnota |tom, zda text Hello druhý toocheck hodnota je true. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud jsou obě hodnoty true; v opačném **False**.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse logické funkce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

výstup Hello z hello předchozím příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |


## <a name="bool"></a>BOOL
`bool(arg1)`

Převede hello parametr tooa logická hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |řetězec nebo celá čísla |Hello tooa tooconvert hodnota logická hodnota. |

### <a name="return-value"></a>Návratová hodnota
Logická hodnota Dobrý den převést hodnotu.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello bool toouse řetězec nebo celé číslo.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| trueString | BOOL | True |
| falseString | BOOL | False |
| trueInt | BOOL | True |
| falseInt | BOOL | False |

## <a name="if"></a>Pokud
`if(condition, trueValue, falseValue)`

Vrátí hodnotu, podle toho, zda je podmínka true nebo false.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| Podmínka |Ano |Logická hodnota |Hodnota toocheck Hello, jestli je true. |
| trueValue |Ano | řetězec, int, objekt nebo pole |Hodnota tooreturn Hello, pokud je splněna podmínka hello. |
| falseValue |Ano | řetězec, int, objekt nebo pole |Hello hodnota tooreturn při hello podmínka vyhodnocena jako false. |

### <a name="return-value"></a>Návratová hodnota

Vrátí druhý parametr, pokud je první parametr **True**, jinak vrátí třetí parametr.

### <a name="remarks"></a>Poznámky

Tato sada tooconditionally funkce můžete použít vlastnost prostředku. Hello následující příklad není kompletní šablonu, ale zobrazuje hello příslušné části pro skupinu dostupnosti hello podmíněně nastavení.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse hello `if` funkce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

výstup Hello z hello předchozím příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| yesOutput | Řetězec | Ano |
| noOutput | Řetězec | Ne |


## <a name="not"></a>není
`not(arg1)`

Převede logickou hodnotu tooits opačným hodnotu.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |Logická hodnota |Hodnota tooconvert Hello. |


### <a name="return-value"></a>Návratová hodnota

Vrátí **True** po parametr **False**. Vrátí **False** po parametr **True**.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse logické funkce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

výstup Hello z hello předchozím příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |

Hello následující příklad používá **není** s [rovná](resource-group-template-functions-comparison.md#equals).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

výstup Hello z hello předchozím příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkNotEquals | BOOL | True |


## <a name="or"></a>nebo
`or(arg1, arg2)`

Zkontroluje, zda je buď parametr hodnotu true.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |Logická hodnota |tom, zda text Hello toocheck první hodnota je true. |
| arg2 |Ano |Logická hodnota |tom, zda text Hello druhý toocheck hodnota je true. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud buď hodnota je true; v opačném **False**.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse logické funkce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

výstup Hello z hello předchozím příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| andExampleOutput | BOOL | False |
| orExampleOutput | BOOL | True |
| notExampleOutput | BOOL | False |


## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

