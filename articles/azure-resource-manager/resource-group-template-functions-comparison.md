---
title: "funkce pro šablony správce prostředků ze aaaAzure - porovnání | Microsoft Docs"
description: "Popisuje funkce toouse hello hodnoty toocompare šablony Azure Resource Manager."
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
ms.openlocfilehash: ebcfc9ed6c93f8b540ec4c066e9457c621800b7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
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
| arg1 |Ano |int, string, pole nebo objekt |Hello první hodnota toocheck rovnosti. |
| arg2 |Ano |int, string, pole nebo objekt |Hello druhá hodnota toocheck rovnosti. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello hodnoty jsou stejné, jinak hodnota **False**.

### <a name="remarks"></a>Poznámky

Hello funkce rovná se často používá u hello `condition` element tootest tom, jestli je nasazený prostředku.

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

Příklad šablony Hello kontroluje různé typy hodnot rovnosti. Všechny hello výchozí hodnoty, vrátí hodnotu True.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | True |
| checkArrays | BOOL | True |
| checkObjects | BOOL | True |


Hello následující příklad používá [není](resource-group-template-functions-logical.md#not) s **rovná**.

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


## <a name="greater"></a>větší
`greater(arg1, arg2)`

Kontroluje, zda text hello, první hodnota je větší než druhá hodnota, která hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |Hello první hodnotu pro porovnání větší hello. |
| arg2 |Ano |int nebo string |Druhá hodnota Hello větší porovnání hello. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello první hodnota je větší než druhá hodnota, která hello; v opačném **False**.

### <a name="example"></a>Příklad

Příklad šablony Hello zkontroluje, zda je větší než hello jiných hello jednu hodnotu.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | False |
| checkStrings | BOOL | True |


## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Kontroluje, zda text hello, první hodnota je větší než nebo rovna toohello druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |Hello první hodnotu pro porovnání větší nebo rovna hello. |
| arg2 |Ano |int nebo string |Druhá hodnota Hello hello větší nebo rovna porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello první hodnota je druhá hodnota větší než nebo rovna toohello; jinak **False**.

### <a name="example"></a>Příklad

Hello příklad šablony kontroluje, zda text hello jedna hodnota je větší než nebo rovna toohello jiné.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | False |
| checkStrings | BOOL | True |



## <a name="less"></a>menší
`less(arg1, arg2)`

Kontroluje, zda text hello první hodnota je menší než hello druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |Hello první hodnota hello méně porovnání. |
| arg2 |Ano |int nebo string |Hello druhá hodnota pro hello méně porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello první hodnota je menší než hello druhý hodnotu; jinak **False**.

### <a name="example"></a>Příklad

Hello příklad šablony kontroluje, zda hello jedna hodnota je menší než hello jiné.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | False |


## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Zkontroluje, zda text hello, první hodnota je menší než nebo rovna toohello druhá hodnota.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int nebo string |první hodnota Hello hello menší nebo rovná porovnání. |
| arg2 |Ano |int nebo string |Druhá hodnota, která pro hello Hello menší nebo rovno porovnání. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello první hodnota je menší než nebo rovna toohello druhá hodnota, jinak hodnota **False**.

### <a name="example"></a>Příklad

Hello příklad šablony zkontroluje, zda hello jedna hodnota je menší než nebo rovna toohello jiné.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| checkInts | BOOL | True |
| checkStrings | BOOL | False |



## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

