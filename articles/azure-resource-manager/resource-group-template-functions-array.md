---
title: "šablony Resource Manageru aaaAzure funkce – polí a objekty | Microsoft Docs"
description: "Popisuje funkce toouse hello v šablonu Azure Resource Manageru pro práci s pole a objekty."
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
ms.date: 06/12/2017
ms.author: tomfitz
ms.openlocfilehash: e5f1a9b2a71039562eae7e48c2474a1fa59a7bea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Pole a objektu funkce pro šablony Azure Resource Manager 

Resource Manager poskytuje několik funkce pro práci s pole a objekty.

* [pole](#array)
* [sloučení](#coalesce)
* [concat](#concat)
* [obsahuje](#contains)
* [createArray](#createarray)
* [prázdný](#empty)
* [první](#first)
* [průnik](#intersection)
* [JSON](#json)
* [poslední](#last)
* [Délka](#length)
* [min.](#min)
* [maximální počet](#max)
* [rozsah](#range)
* [Přeskočit](#skip)
* [proveďte](#take)
* [sjednocení](#union)

tooget pole hodnot řetězec oddělená hodnotu, najdete v části [rozdělení](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>Pole
`array(convertToArray)`

Převede pole tooan hodnota hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| convertToArray |Ano |int, string, pole nebo objekt |Hello hodnotu tooconvert tooan pole. |

### <a name="return-value"></a>Návratová hodnota

Pole.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse hello funkce pole s různými typy.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "a"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| intOutput | Pole | [1] |
| stringOutput | Pole | ["a"] |
| objectOutput | Pole | [{"a": "b", "c": "d"}] |

<a id="coalesce" />

## <a name="coalesce"></a>sloučení
`coalesce(arg1, arg2, arg3, ...)`

Vrátí první hodnotu než null z hello parametry. Prázdné řetězce, prázdné pole a prázdné objekty nejsou null.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |int, string, pole nebo objekt |Hello první hodnota tootest pro hodnotu null. |
| Další argumenty |Ne |int, string, pole nebo objekt |Další hodnoty tootest pro hodnotu null. |

### <a name="return-value"></a>Návratová hodnota

Hodnota Hello parametrů hello první jinou hodnotu než null, což může být řetězec, int, pole nebo objekt. Hodnota Null, pokud jsou všechny parametry hodnotu null. 

### <a name="example"></a>Příklad

Hello následující příklad ukazuje výstup hello z různých používá funkci coalesce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| stringOutput | Řetězec | Výchozí |
| intOutput | celá čísla | 1 |
| objectOutput | Objekt | {"první": "Výchozí"} |
| arrayOutput | Pole | [1] |
| emptyOutput | BOOL | True |

<a id="concat" />

## <a name="concat"></a>concat
`concat(arg1, arg2, arg3, ...)`

Kombinuje více polí a vrátí hello zřetězených pole, nebo kombinuje více řetězcové hodnoty a vrátí hello zřetězených řetězec. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello první pole nebo řetězec pro zřetězení. |
| Další argumenty |Ne |pole nebo řetězec |Další pole nebo řetězců v sekvenčním pořadí pro zřetězení. |

Tato funkce může trvat libovolný počet argumentů a může přijímat řetězce nebo pole parametrů hello.

### <a name="return-value"></a>Návratová hodnota
Řetězec nebo pole zřetězených hodnot.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak maticových toocombine dva.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| Vrátí | Pole | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Hello následující příklad ukazuje, jak toocombine dva řetězce hodnoty a vrátí spojený řetězec.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| concatOutput | Řetězec | Předpona 5yj4yjf5mbg72 |

<a id="contains" />

## <a name="contains"></a>Obsahuje
`contains(container, itemToFind)`

Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| kontejner |Ano |pole, objekt nebo řetězec |Hello hodnotu, která obsahuje hodnotu toofind hello. |
| itemToFind |Ano |řetězec nebo celá čísla |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

**Hodnota TRUE,** Pokud je položka hello, jinak hodnota **False**.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse obsahuje s různými typy:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| stringTrue | BOOL | True |
| stringFalse | BOOL | False |
| objectTrue | BOOL | True |
| objectFalse | BOOL | False |
| arrayTrue | BOOL | True |
| arrayFalse | BOOL | False |

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

Vytvoří pole z hello parametry.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |Řetězec, celé číslo, pole nebo objekt |Hello první hodnotu v poli hello. |
| Další argumenty |Ne |Řetězec, celé číslo, pole nebo objekt |Další hodnoty v poli hello. |

### <a name="return-value"></a>Návratová hodnota

Pole.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak Hello toouse createArray s různými typy:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| stringArray | Pole | ["a", "b", "c"] |
| intArray | Pole | [1, 2, 3] |
| objectArray | Pole | [{"1": "a", "dva": "b", "tři": "c"}] |
| arrayArray | Pole | [["1", "dva", "tři"]] |

<a id="empty" />

## <a name="empty"></a>prázdný

`empty(itemToTest)`

Určuje, zda je prázdný řetězec, objekt nebo pole.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| itemToTest |Ano |pole, objekt nebo řetězec |Hodnota toocheck Hello, pokud je prázdná. |

### <a name="return-value"></a>Návratová hodnota

Vrátí **True** Pokud hello hodnota je prázdný, jinak hodnota **False**.

### <a name="example"></a>Příklad

Následující ukázka Hello ověří, zda jsou řetězec, objekt a pole prázdné.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayEmpty | BOOL | True |
| objectEmpty | BOOL | True |
| stringEmpty | BOOL | True |

<a id="first" />

## <a name="first"></a>první
`first(arg1)`

Vrátí hello první prvek pole hello nebo první znak hello řetězce.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello hodnota tooretrieve hello první prvek nebo znak. |

### <a name="return-value"></a>Návratová hodnota

Hello typ (řetězec, int, pole nebo objekt) hello první prvek v poli, nebo první znak hello řetězce.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse hello první funkce s řetězec a pole.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Řetězec | jeden |
| stringOutput | Řetězec | O |

<a id="intersection" />

## <a name="intersection"></a>průnik
`intersection(arg1, arg2, arg3, ...)`

Vrátí jeden pole nebo objekt s společné prvky hello z hello parametry.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |objekt nebo pole |Hello první hodnota toouse pro hledání běžných elementech. |
| arg2 |Ano |objekt nebo pole |Hello druhá hodnota toouse pro hledání běžných elementech. |
| Další argumenty |Ne |objekt nebo pole |Další hodnoty toouse pro hledání běžných elementech. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo objekt s společné prvky hello.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak toouse průnik s maticových Hello a objekty:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| objectOutput | Objekt | {"1": "a", "tři": "c"} |
| arrayOutput | Pole | ["dva", "tři"] |


## <a name="json"></a>JSON
`json(arg1)`

Vrátí objekt JSON.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |Řetězec |tooJSON tooconvert hodnotu Hello. |


### <a name="return-value"></a>Návratová hodnota

Zadaný objekt JSON Hello z hello řetězec nebo objekt prázdný při **null** je zadán.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak toouse průnik s maticových Hello a objekty:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| jsonOutput | Objekt | {"a": "b"} |
| nullOutput | Logická hodnota | True |

<a id="last" />

## <a name="last"></a>poslední
`last (arg1)`

Vrátí hello posledním elementem pole hello nebo poslední znak hello řetězce.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello hodnota tooretrieve hello poslední element nebo znak. |

### <a name="return-value"></a>Návratová hodnota

Hello typ (řetězec, int, pole nebo objekt) hello posledním prvkem v matici, nebo poslední znak hello řetězce.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse hello poslední funkci s řetězec a pole.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Řetězec | tři |
| stringOutput | Řetězec | E |

<a id="length" />

## <a name="length"></a>Délka
`length(arg1)`

Vrátí hello počet elementů pole nebo znaků v řetězci.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello toouse pole pro získání hello počet elementů nebo hello toouse řetězec pro získání hello počet znaků. |

### <a name="return-value"></a>Návratová hodnota

Typ int. 

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak Hello toouse délka s řetězec a pole:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayLength | celá čísla | 3 |
| stringLength | celá čísla | 13 |

Při vytváření prostředků, můžete použít tuto funkci s číslem pole toospecify hello iterací. V následujícím příkladu hello, hello parametr **siteNames** odkazovaly tooan pole názvy toouse při tvorbě webů hello.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Další informace o použití této funkce se pole najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).

<a id="min" />

## <a name="min"></a>min
`min(arg1)`

Vrátí hello minimální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole celá čísla nebo seznam celých čísel oddělených čárkou |Hello kolekce tooget hello minimální hodnota. |

### <a name="return-value"></a>Návratová hodnota

Typ int představující hello minimální hodnotu.

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
`max(arg1)`

Vrátí hello maximální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole celá čísla nebo seznam celých čísel oddělených čárkou |Hello kolekce tooget hello maximální hodnota. |

### <a name="return-value"></a>Návratová hodnota

Typ int představující hello maximální hodnotu.

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

<a id="range" />

## <a name="range"></a>rozsah
`range(startingInteger, numberOfElements)`

Vytvoří pole celých čísel od spuštění celé číslo a který obsahuje počet položek.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| startingInteger |Ano |celá čísla |Hello první celé číslo v poli hello. |
| numberofElements |Ano |celá čísla |Hello počet celých čísel v poli hello. |

### <a name="return-value"></a>Návratová hodnota

Pole celých čísel.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse hello rozsah funkce:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| rangeOutput | Pole | [5, 6, 7] |

<a id="skip" />

## <a name="skip"></a>Přeskočit
`skip(originalValue, numberToSkip)`

Vrátí pole s všechny elementy hello po zadaný počet v poli hello hello nebo vrátí řetězec s všechny znaky hello po hello zadaný počet v řetězci hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| původní hodnota |Ano |pole nebo řetězec |Hello toouse pro přeskočení pole nebo řetězec. |
| numberToSkip |Ano |celá čísla |Hello počet tooskip elementy nebo znaky. Pokud tato hodnota je 0 nebo menší, všechny hello elementy nebo znaků hello hodnoty jsou vráceny. Pokud je větší než délka hello hello pole nebo řetězec, se vrátí prázdné pole nebo řetězec. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo řetězec.

### <a name="example"></a>Příklad

Následující příklad přeskočí hello Hello zadaný počet prvků v poli hello a hello zadaný počet znaků v řetězci.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Pole | ["tři"] |
| stringOutput | Řetězec | dvě tři |

<a id="take" />

## <a name="take"></a>proveďte
`take(originalValue, numberToTake)`

Vrátí pole s hello zadaný počet elementů od hello začátek hello pole nebo řetězec s hello zadaný počet znaků od začátku hello hello řetězce.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| původní hodnota |Ano |pole nebo řetězec |Hello pole nebo řetězec elementy hello tootake z. |
| numberToTake |Ano |celá čísla |Hello počet tootake elementy nebo znaky. Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec. Pokud je větší než délka hello hello zadané pole nebo řetězec, vrátí se všechny elementy hello ve hello pole nebo řetězec. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo řetězec.

### <a name="example"></a>Příklad

Následující příklad trvá hello Hello zadaný počet elementů od hello pole a znaků z řetězce.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Pole | ["1", "dva"] |
| stringOutput | Řetězec | na |

<a id="union" />

## <a name="union"></a>sjednocení
`union(arg1, arg2, arg3, ...)`

Vrátí jeden pole nebo objekt s všechny elementy z hello parametry. Duplicitní hodnoty nebo klíče jsou jenom jednou zahrnuty.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |objekt nebo pole |Hello první hodnota toouse pro připojení elementy. |
| arg2 |Ano |objekt nebo pole |Hello druhá hodnota toouse pro připojení elementy. |
| Další argumenty |Ne |objekt nebo pole |Další hodnoty toouse pro připojení elementy. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo objekt.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak toouse sjednocení s maticových Hello a objekty:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| objectOutput | Objekt | {"1": "a", "dva": "b", "tři": "c", "4": "d", "5": "e"} |
| arrayOutput | Pole | ["1", "dva", "tři", "čtyři"] |

## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

