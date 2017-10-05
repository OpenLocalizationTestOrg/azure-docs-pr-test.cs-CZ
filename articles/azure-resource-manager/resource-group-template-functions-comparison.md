---
title: "Funkce šablon Azure Resource Manager - porovnání | Microsoft Docs"
description: "Popisuje funkce pro použití pro porovnání hodnot v šablonu Azure Resource Manager."
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
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Porovnání funkce pro šablony Azure Resource Manager

Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.

* [rovná se](#equals)
* [větší](#greater)
* [greaterOrEquals](#greaterorequals)
* [menší](#less)
* [lessOrEquals](#lessorequals)

## <a name="equals"></a>Rovná se
`equals(arg1, arg2)`

Kontroluje, zda dvě hodnoty rovny navzájem.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int, string, pole nebo objekt |První hodnota ke kontrole rovnosti. |
| arg2 |Ano |int, string, pole nebo objekt |Druhá hodnota ke kontrole rovnosti. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hodnoty jsou stejné, jinak hodnota **False**.

### <a name="remarks"></a>Poznámky

Funkce rovná se často používá u `condition` elementu, který chcete otestovat, zda je nasazená prostředku.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Příklad

Příklad šablony kontroluje různé typy hodnot rovnosti. Všechny výchozí hodnoty vrátí hodnotu True.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | True |
| checkArrays | BOOL | True |
| checkObjects | BOOL | True |


Následující příklad používá [není](resource-group-template-functions-logical.md#not) s **rovná**.

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

Výstup z předchozího příkladu je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkNotEquals | BOOL | True |


## <a name="greater"></a>větší
`greater(arg1, arg2)`

Kontroluje, zda je první hodnota je větší než druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |První hodnota větší porovnání. |
| arg2 |Ano |int nebo string |Druhá hodnota, pro větší porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud první hodnota je větší než druhá hodnota, jinak hodnota **False**.

### <a name="example"></a>Příklad

Příklad šablony zkontroluje, zda je větší než druhá jednu hodnotu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | False |
| checkStrings | BOOL | True |


## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Kontroluje, zda je první hodnota je větší než nebo rovna hodnotě druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |První hodnotu pro porovnání větší nebo rovna. |
| arg2 |Ano |int nebo string |Druhá hodnota pro porovnání větší nebo rovna. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud první hodnota je větší než nebo rovno druhá hodnota, jinak hodnota **False**.

### <a name="example"></a>Příklad

Příklad šablony kontroluje, zda jedna hodnota je větší než nebo rovna hodnotě dalších.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | False |
| checkStrings | BOOL | True |



## <a name="less"></a>menší
`less(arg1, arg2)`

Kontroluje, zda je první hodnota menší než druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |První hodnota menší porovnání. |
| arg2 |Ano |int nebo string |Druhá hodnota pro menší porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud první hodnota je menší než druhá hodnota, jinak hodnota **False**.

### <a name="example"></a>Příklad

Příklad šablony kontroluje, zda jedna hodnota je menší než druhý.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | False |


## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Kontroluje, zda je první hodnota menší než nebo rovna hodnotě druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |První hodnota je menší nebo rovná porovnání. |
| arg2 |Ano |int nebo string |Druhá hodnota je menší nebo rovná porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud první hodnota je menší než nebo rovna hodnotě druhý; jinak hodnota **False**.

### <a name="example"></a>Příklad

Příklad šablony kontroluje, zda jedna hodnota je menší než nebo rovna na druhý.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | False |



## <a name="next-steps"></a>Další kroky
* Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

