---
title: "aaaAzure Resource Manager šablony funkce – číselné | Microsoft Docs"
description: "Popisuje funkce toouse hello v toowork šablony Azure Resource Manager se čísla."
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: 855d5b354d094b9815edc160e3d72efbfd36ba77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Numerické funkce pro šablony Azure Resource Manager

Resource Manager poskytuje následující funkce pro práci s celými čísly hello:

* [Přidat](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [plovoucí desetinná čárka](#float)
* [celá čísla](#int)
* [min.](#min)
* [maximální počet](#max)
* [MOD](#mod)
* [mul](#mul)
* [Sub –](#sub)

<a id="add" />

## <a name="add"></a>Přidat
`add(operand1, operand2)`

Vrátí hello součet hello dvě zadané celá čísla.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- | 
|operand1 |Ano |celá čísla |První číslo tooadd. |
|operand2 |Ano |celá čísla |Druhé číslo tooadd. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo, které obsahuje součet hello hello parametry.

### <a name="example"></a>Příklad

Hello následující příklad přidá dva parametry.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer tooadd"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer tooadd"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| addResult | celá čísla | 8 |

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

Vrátí hello index smyčky iterací. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| loopName | Ne | Řetězec | Hello název pro získávání hello iteraci smyčky hello. |
| Posun |Ne |celá čísla |Hello číslo tooadd toohello nule iterace hodnota. |

### <a name="remarks"></a>Poznámky

Tato funkce se vždy používá s **kopie** objektu. Pokud není zadána žádná hodnota pro **posun**, je vrácena hodnota aktuální iterace hello. Hodnota iterace Hello začíná od nuly.

Hello **loopName** vlastnost vám umožní toospecify zda copyIndex odkazuje tooa prostředků iterace nebo vlastnost iterací. Pokud není zadána žádná hodnota pro **loopName**, se používá hello aktuální iterace typ prostředku. Zadejte hodnotu pro **loopName** během iterace u vlastnosti. 
 
Úplný popis jak používat **copyIndex**, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).

### <a name="example"></a>Příklad

Hello následující příklad ukazuje kopírování smyčky a hello index hodnotu součástí názvu hello. 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a>Návratová hodnota

Celé číslo představující aktuální index hello iterace hello.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Vrátí hello celočíselného dělení čísla hello dvě zadané celá čísla.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| operand1 |Ano |celá čísla |číslo Hello rozdělené. |
| operand2 |Ano |celá čísla |Hello číslo, které je použité toodivide. Nemůže být 0. |

### <a name="return-value"></a>Návratová hodnota

Dělení celého čísla představující hello.

### <a name="example"></a>Příklad

Následující ukázka Hello vydělí jeden parametr jiné parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| divResult | celá čísla | 2 |

<a id="float" />

## <a name="float"></a>Plovoucí desetinná čárka
`float(arg1)`

Převede tooa hello hodnotu bodu číslo s plovoucí čárkou. Při předávání vlastních parametrů tooan aplikaci, například aplikace logiky pouze použití této funkce.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |řetězec nebo celá čísla |tooconvert tooa Hello hodnotu bodu číslo s plovoucí čárkou. |

### <a name="return-value"></a>Návratová hodnota
Plovoucí bodu číslo.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse float toopass parametry tooa aplikace logiky:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
        "custom1": {
            "value": "[float('3.0')]"
        },
        "custom2": {
            "value": "[float(3)]"
        },
```

<a id="int" />

## <a name="int"></a>celá čísla
`int(valueToConvert)`

Převede hello zadaná hodnota tooan celé číslo.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ano |řetězec nebo celá čísla |Hello hodnotu tooconvert tooan celé číslo. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo hodnoty hello převést.

### <a name="example"></a>Příklad

Hello následující příklad převede toointeger hodnota hello parametr zadaný uživatelem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| Zavřete | celá čísla | 4 |


<a id="min" />

## <a name="min"></a>min
`min (arg1)`

Vrátí hello minimální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole celá čísla nebo seznam celých čísel oddělených čárkou |Hello kolekce tooget hello minimální hodnota. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo představující minimální hodnota z kolekce hello.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak Hello toouse min s pole a seznam celých čísel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | celá čísla | 0 |
| intOutput | celá čísla | 0 |

<a id="max" />

## <a name="max"></a>maximální počet
`max (arg1)`

Vrátí hello maximální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole celá čísla nebo seznam celých čísel oddělených čárkou |Hello kolekce tooget hello maximální hodnota. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo představující hello maximální hodnotu z kolekce hello.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak Hello toouse maximální s pole a seznam celých čísel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | celá čísla | 5 |
| intOutput | celá čísla | 5 |

<a id="mod" />

## <a name="mod"></a>MOD
`mod(operand1, operand2)`

Vrátí zbytek hello hello celočíselné dělení pomocí hello dvě zadané celá čísla.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| operand1 |Ano |celá čísla |číslo Hello rozdělené. |
| operand2 |Ano |celá čísla |Hello číslo, které je použité toodivide, nemůže být 0. |

### <a name="return-value"></a>Návratová hodnota
Celé číslo představující hello zbytek.

### <a name="example"></a>Příklad

Hello následující příklad vrátí hello zbytek po dělení jeden parametr jiné parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| modResult | celá čísla | 1 |

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

Vrátí hello násobení hello dvě zadané celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| operand1 |Ano |celá čísla |První číslo toomultiply. |
| operand2 |Ano |celá čísla |Druhé číslo toomultiply. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo představující hello násobení.

### <a name="example"></a>Příklad

Následující ukázka Hello vynásobí jeden parametr jiné parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer toomultiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer toomultiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| mulResult | celá čísla | 15 |

<a id="sub" />

## <a name="sub"></a>Sub –
`sub(operand1, operand2)`

Vrátí hello odčítání hello dvě zadané celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| operand1 |Ano |celá čísla |Hello číslo, které je odečten od. |
| operand2 |Ano |celá čísla |Hello číslo, které je odečten. |

### <a name="return-value"></a>Návratová hodnota
Celé číslo představující hello odčítání.

### <a name="example"></a>Příklad

Následující ukázka Hello odečítá jeden parametr z jiného parametru.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer toosubtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| subResult | celá čísla | 4 |

## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

