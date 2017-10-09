---
title: "funkce šablony Resource Manageru aaaAzure - řetězec | Microsoft Docs"
description: "Popisuje funkce toouse hello v toowork šablony Azure Resource Manager s řetězci."
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
ms.openlocfilehash: 27f7f6a52cbe4e9915718184433e92ca92999346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Řetězcové funkce pro šablony Azure Resource Manager

Resource Manager poskytuje následující funkce pro práci s řetězci hello:

* [formátu Base64.](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [obsahuje](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [prázdný](#empty)
* [endsWith](#endswith)
* [první](#first)
* [indexOf](#indexof)
* [poslední](#last)
* [lastIndexOf](#lastindexof)
* [Délka](#length)
* [padLeft](#padleft)
* [nahradit](#replace)
* [Přeskočit](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [řetězec](#string)
* [dílčí řetězec](#substring)
* [proveďte](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [uvolnění dočasné paměti](#trim)
* [uniqueString](#uniquestring)
* [identifikátor URI](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>formátu Base64.
`base64(inputString)`

Vrátí hello reprezentace hello vstupní řetězec base64.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| inputString |Ano |Řetězec |Hodnota tooreturn Hello jako znázornění base64. |

### <a name="return-value"></a>Návratová hodnota

Řetězec obsahující reprezentace hello base64.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toouse hello funkce base64.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| base64Output | Řetězec | b25lLCB0d28sIHRocmVl |
| toStringOutput | Řetězec | Jedna dva tři |
| toJsonOutput | Objekt | {"1": "a", "dva": "b"} |

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

Převede objekt base64 tooa reprezentaci JSON.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| base64Value |Ano |Řetězec |Hello base64 reprezentace tooconvert tooa objekt JSON. |

### <a name="return-value"></a>Návratová hodnota

Objekt JSON.

### <a name="examples"></a>Příklady

Hello následující příklad používá hello base64ToJson funkce tooconvert hodnotu base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| base64Output | Řetězec | b25lLCB0d28sIHRocmVl |
| toStringOutput | Řetězec | Jedna dva tři |
| toJsonOutput | Objekt | {"1": "a", "dva": "b"} |

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Převede řetězec base64 reprezentace tooa.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| base64Value |Ano |Řetězec |Hello base64 reprezentace tooconvert tooa řetězec. |

### <a name="return-value"></a>Návratová hodnota

Řetězec hello převést hodnotu base64.

### <a name="examples"></a>Příklady

Hello následující příklad používá hello base64ToString funkce tooconvert hodnotu base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| base64Output | Řetězec | b25lLCB0d28sIHRocmVl |
| toStringOutput | Řetězec | Jedna dva tři |
| toJsonOutput | Objekt | {"1": "a", "dva": "b"} |



<a id="concat" />

## <a name="concat"></a>concat
`concat (arg1, arg2, arg3, ...)`

Kombinuje více řetězcové hodnoty a vrátí řetězec hello zřetězených nebo kombinuje několik polí a vrací hello zřetězených pole.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |řetězec nebo pole |Hello první hodnota zřetězení. |
| Další argumenty |Ne |Řetězec |Další hodnoty v sekvenčním pořadí pro zřetězení. |

### <a name="return-value"></a>Návratová hodnota
Řetězec nebo pole zřetězených hodnot.

### <a name="examples"></a>Příklady

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

<a id="contains" />

## <a name="contains"></a>Obsahuje
`contains (container, itemToFind)`

Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| kontejner |Ano |pole, objekt nebo řetězec |Hello hodnotu, která obsahuje hodnotu toofind hello. |
| itemToFind |Ano |řetězec nebo celá čísla |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

**Hodnota TRUE,** Pokud je položka hello, jinak hodnota **False**.

### <a name="examples"></a>Příklady

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

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Převede data tooa hodnota identifikátoru URI.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToConvert |Ano |Řetězec |Hello tooconvert tooa údaj hodnoty identifikátoru URI. |

### <a name="return-value"></a>Návratová hodnota

Řetězec formátu dat identifikátor URI.

### <a name="examples"></a>Příklady

Následující ukázka Hello převede data tooa hodnota identifikátoru URI a převede data řetězce tooa URI:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| dataUriOutput | Řetězec | data: text / prostý; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Řetězec | Ahoj světe! |

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Hodnota tooa řetězec ve formátu převádí data identifikátor URI.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Ano |Řetězec |data Hello tooconvert hodnota identifikátoru URI. |

### <a name="return-value"></a>Návratová hodnota

Řetězec obsahující hello převést hodnotu.

### <a name="examples"></a>Příklady

Následující ukázka Hello převede data tooa hodnota identifikátoru URI a převede data řetězce tooa URI:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| dataUriOutput | Řetězec | data: text / prostý; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Řetězec | Ahoj světe! |

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

### <a name="examples"></a>Příklady

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

<a id="endswith" />

## <a name="endswith"></a>endsWith
`endsWith(stringToSearch, stringToFind)`

Určuje, zda řetězec končí s hodnotou. Hello porovnání nerozlišuje malá a velká písmena.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ano |Řetězec |Hello hodnotu, která obsahuje položky toofind hello. |
| stringToFind |Ano |Řetězec |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

**Hodnota TRUE,** Pokud hello poslední znak nebo znaky řetězce hello odpovídají hello hodnotu; jinak, **False**.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toouse hello funkce startsWith a endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| startsTrue | BOOL | True |
| startsCapTrue | BOOL | True |
| startsFalse | BOOL | False |
| endsTrue | BOOL | True |
| endsCapTrue | BOOL | True |
| endsFalse | BOOL | False |

<a id="first" />

## <a name="first"></a>první
`first(arg1)`

Vrátí hello první znak řetězce hello nebo první prvek pole hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello hodnota tooretrieve hello první prvek nebo znak. |

### <a name="return-value"></a>Návratová hodnota

Řetězec hello první znak nebo typ hello (řetězec, int, pole nebo objekt) hello první prvek v poli.

### <a name="examples"></a>Příklady

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

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Vrátí hello první pozici hodnoty v řetězci. Hello porovnání nerozlišuje malá a velká písmena.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ano |Řetězec |Hello hodnotu, která obsahuje položky toofind hello. |
| stringToFind |Ano |Řetězec |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo představující pozici hello toofind položky hello. Hodnota Hello je počítáno od nuly. Pokud hello položka není nalezena, se vrátí hodnotu -1.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toouse hello funkce indexOf a lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| firstT | celá čísla | 0 |
| lastT | celá čísla | 3 |
| firstString | celá čísla | 2 |
| lastString | celá čísla | 0 |
| notFound | celá čísla | -1 |

<a id="last" />

## <a name="last"></a>poslední
`last (arg1)`

Vrátí poslední znak řetězce hello nebo hello posledním elementem pole hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello hodnota tooretrieve hello poslední element nebo znak. |

### <a name="return-value"></a>Návratová hodnota

Řetězec hello poslední znak nebo typ hello (řetězec, int, pole nebo objekt) hello poslední prvek v poli.

### <a name="examples"></a>Příklady

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

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Vrátí hello poslední umístění hodnoty v řetězci. Hello porovnání nerozlišuje malá a velká písmena.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ano |Řetězec |Hello hodnotu, která obsahuje položky toofind hello. |
| stringToFind |Ano |Řetězec |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

Celé číslo, které představuje poslední pozice hello toofind položky hello. Hodnota Hello je počítáno od nuly. Pokud hello položka není nalezena, se vrátí hodnotu -1.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toouse hello funkce indexOf a lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| firstT | celá čísla | 0 |
| lastT | celá čísla | 3 |
| firstString | celá čísla | 2 |
| lastString | celá čísla | 0 |
| notFound | celá čísla | -1 |

<a id="length" />

## <a name="length"></a>Délka
`length(string)`

Vrátí hello počet znaků v řetězci nebo prvků v poli.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| arg1 |Ano |pole nebo řetězec |Hello toouse pole pro získání hello počet elementů nebo hello toouse řetězec pro získání hello počet znaků. |

### <a name="return-value"></a>Návratová hodnota

Typ int. 

### <a name="examples"></a>Příklady

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

<a id="padleft" />

## <a name="padleft"></a>padLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Vrací vpravo zarovnaný řetězec přidáním znaků toohello doleva až do dosažení celkové zadané délky hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| valueToPad |Ano |řetězec nebo celá čísla |Hello hodnota tooright-zarovnat. |
| Hodnota totalLength |Ano |celá čísla |Celkový počet znaků v hello Hello vrátil řetězec. |
| paddingCharacter |Ne |jeden znak |Hello toouse znak pro odsazení nalevo dokud nebude dosaženo celková délka hello. Hello výchozí hodnota je mezera. |

Pokud původní řetězec hello je delší než hello počet znaků toopad, přidají se žádné znaky.

### <a name="return-value"></a>Návratová hodnota

Řetězec s minimálně hello počet zadaný znaků.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toopad hello hodnota parametru zadaný uživatelem přidáním hello nulové znak dokud nedosáhne hello celkový počet znaků. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| stringOutput | Řetězec | 0000000123 |

<a id="replace" />

## <a name="replace"></a>Nahradit
`replace(originalString, oldString, newString)`

Vrátí nový řetězec se všechny instance jeden řetězec nahrazen jiným řetězcem.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| originalString |Ano |Řetězec |Hello hodnotu, která obsahuje všechny instance jeden řetězec nahrazen jiným řetězcem. |
| oldString |Ano |Řetězec |odebrat z původního řetězce hello toobe řetězec Hello. |
| newstring – |Ano |Řetězec |řetězec tooadd Hello místo hello odebrat řetězec. |

### <a name="return-value"></a>Návratová hodnota

Řetězec s hello nahradit znaků.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak tooremove všechny pomlčky z řetězce hello zadaný uživatelem a jak tooreplace součástí hello jiným řetězcem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| firstOutput | Řetězec | 1231231234 |
| secodeOutput | Řetězec | 123-123-xxxx |

<a id="skip" />

## <a name="skip"></a>Přeskočit
`skip(originalValue, numberToSkip)`

Vrátí řetězec s všechny znaky hello po hello zadaný počet znaků, nebo pole s všechny elementy hello po hello zadaný počet elementů.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| původní hodnota |Ano |pole nebo řetězec |Hello toouse pro přeskočení pole nebo řetězec. |
| numberToSkip |Ano |celá čísla |Hello počet tooskip elementy nebo znaky. Pokud tato hodnota je 0 nebo menší, všechny hello elementy nebo znaků hello hodnoty jsou vráceny. Pokud je větší než délka hello hello pole nebo řetězec, se vrátí prázdné pole nebo řetězec. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo řetězec.

### <a name="examples"></a>Příklady

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

<a id="split" />

## <a name="split"></a>split
`split(inputString, delimiter)`

Vrací pole řetězců obsahující hello podřetězce hello vstupní řetězec, které jsou odděleny hello zadaného oddělovače.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| inputString |Ano |Řetězec |řetězec toosplit Hello. |
| Oddělovač |Ano |řetězec nebo pole řetězců. |Oddělovač toouse Hello k rozdělení hello řetězec. |

### <a name="return-value"></a>Návratová hodnota

Pole řetězců.

### <a name="examples"></a>Příklady

Hello následující příklad rozdělí vstupní řetězec hello se čárkou a s čárkou nebo středníkem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| firstOutput | Pole | ["1", "dva", "tři"] |
| secondOutput | Pole | ["1", "dva", "tři"] |

<a id="startswith" />

## <a name="startswith"></a>startsWith
`startsWith(stringToSearch, stringToFind)`

Určuje, zda řetězec začíná hodnotu. Hello porovnání nerozlišuje malá a velká písmena.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ano |Řetězec |Hello hodnotu, která obsahuje položky toofind hello. |
| stringToFind |Ano |Řetězec |Hodnota toofind Hello. |

### <a name="return-value"></a>Návratová hodnota

**Hodnota TRUE,** Pokud hello první znak nebo znaky řetězce hello odpovídají hello hodnotu; jinak, **False**.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak toouse hello funkce startsWith a endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| startsTrue | BOOL | True |
| startsCapTrue | BOOL | True |
| startsFalse | BOOL | False |
| endsTrue | BOOL | True |
| endsCapTrue | BOOL | True |
| endsFalse | BOOL | False |

<a id="string" />

## <a name="string"></a>Řetězec
`string(valueToConvert)`

Hello převede zadaný řetězec tooa hodnoty.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ano | Všechny |toostring tooconvert hodnotu Hello. Žádný druh hodnotu lze převést, včetně objekty a pole. |

### <a name="return-value"></a>Návratová hodnota

Řetězec hello převést hodnotu.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak tooconvert různé typy hodnot toostrings:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| objectOutput | Řetězec | {"dat": 10, hodnotu "b": "Text, například"} |
| arrayOutput | Řetězec | ["a", "b", "c"] |
| intOutput | Řetězec | 5 |

<a id="substring" />

## <a name="substring"></a>dílčí řetězec
`substring(stringToParse, startIndex, length)`

Vrátí dílčí řetězec, spustí hello zadaný znak pozice a že obsahuje hello zadaný počet znaků.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToParse |Ano |Řetězec |řetězec původní Hello, ze které hello dílčí řetězec extrahován. |
| Počáteční index |Ne |celá čísla |Hello počáteční znak pozice s nulovým základem pro hello dílčí řetězec. |
| Délka |Ne |celá čísla |Hello počet znaků pro hello dílčí řetězec. Musí odkazovat tooa umístění v rámci hello řetězec. |

### <a name="return-value"></a>Návratová hodnota

Hello dílčí řetězec.

### <a name="remarks"></a>Poznámky

Funkce Hello selže, když hello substring přesahuje hello konce řetězce hello. Následující ukázka Hello selže s hello chyba "hello parametry indexu a délky musí odkazovat tooa umístění v rámci hello řetězec. Parametr index Hello: '0' hello parametr délky: 11, hello Délka parametru řetězce hello: "10". ".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Příklady

Následující ukázka Hello extrahuje dílčí řetězec z parametr.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| substringOutput | Řetězec | dvě |


<a id="take" />

## <a name="take"></a>proveďte
`take(originalValue, numberToTake)`

Vrátí řetězec s hello zadaný počet znaků od začátku hello hello řetězec nebo pole s hello zadaný počet elementů od začátku hello hello pole.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| původní hodnota |Ano |pole nebo řetězec |Hello pole nebo řetězec elementy hello tootake z. |
| numberToTake |Ano |celá čísla |Hello počet tootake elementy nebo znaky. Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec. Pokud je větší než délka hello hello zadané pole nebo řetězec, vrátí se všechny elementy hello ve hello pole nebo řetězec. |

### <a name="return-value"></a>Návratová hodnota

Pole nebo řetězec.

### <a name="examples"></a>Příklady

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

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

Hello převede zadaný řetězec toolower případu.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToChange |Ano |Řetězec |Hello hodnotu tooconvert toolower případu. |

### <a name="return-value"></a>Návratová hodnota

Hello řetězec převést toolower případu.

### <a name="examples"></a>Příklady

Následující ukázka Hello převede případ toolower hodnotu parametru a tooupper případu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| toLowerOutput | Řetězec | Jedna dva tři |
| toUpperOutput | Řetězec | JEDNA DVA TŘI |

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

Hello převede zadaný řetězec tooupper případu.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToChange |Ano |Řetězec |Hello hodnotu tooconvert tooupper případu. |

### <a name="return-value"></a>Návratová hodnota

Hello řetězec převést tooupper případu.

### <a name="examples"></a>Příklady

Následující ukázka Hello převede případ toolower hodnotu parametru a tooupper případu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| toLowerOutput | Řetězec | Jedna dva tři |
| toUpperOutput | Řetězec | JEDNA DVA TŘI |

<a id="trim" />

## <a name="trim"></a>Uvolnění dočasné paměti
`trim (stringToTrim)`

Odebere všechny úvodní a koncové prázdné znaky z hello zadaný řetězec.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ano |Řetězec |Hodnota tootrim Hello. |

### <a name="return-value"></a>Návratová hodnota

řetězec Hello bez úvodní a koncové prázdné znaky.

### <a name="examples"></a>Příklady

Hello následující příklad ořízne hello prázdné znaky z parametru hello.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| Vrátí | Řetězec | Jedna dva tři |

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Vytvoří řetězec deterministickou hash na základě hodnot hello zadané jako parametry. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| baseString |Ano |Řetězec |Hodnota Hello používá toocreate funkce hash hello do jedinečného řetězce. |
| Další parametry podle potřeby |Ne |Řetězec |Můžete přidat tolik řetězce jako potřebné toocreate hello hodnotu, která určuje úroveň hello jedinečnosti. |

### <a name="remarks"></a>Poznámky

Tato funkce je užitečné, když potřebujete toocreate jedinečný název pro prostředek. Je-li zadat hodnoty parametrů, které omezit obor hello jedinečnosti pro výsledek hello. Můžete zadat, zda je název hello jedinečný dolů toosubscription, skupinu prostředků nebo nasazení. 

Hello vrátil hodnoty není náhodný řetězec, ale spíš hello výsledek funkce hash. Hello vrátit hodnota je 13 znaků. Není globálně jedinečný. Můžete chtít toocombine hello hodnotu s předponou ze zásady vytváření názvů toocreate smysluplný název. Hello následující příklad ukazuje hello formát hello vrátil hodnotu. Skutečná hodnota Hello se liší podle hello poskytnutými parametry.

    tcvhiyu5h2o5o

Hello následující příklady ukazují, jak toouse uniqueString toocreate a jedinečné hodnoty pro běžně používané úrovně.

Jedinečný oboru toosubscription

```json
"[uniqueString(subscription().subscriptionId)]"
```

Jedinečný vymezená tooresource skupiny

```json
"[uniqueString(resourceGroup().id)]"
```

Jedinečný obor toodeployment pro skupinu prostředků.

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Hello následující příklad ukazuje, jak toocreate jedinečný název pro účet úložiště na základě vaší skupiny prostředků. Uvnitř hello skupinu prostředků, název hello není jedinečný, pokud sestavený hello stejným způsobem.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Návratová hodnota

Řetězec obsahující 13 znaků.

### <a name="examples"></a>Příklady

Hello následující příklad vrátí výsledky z uniquestring:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

<a id="uri" />

## <a name="uri"></a>identifikátor URI
`uri (baseUri, relativeUri)`

Vytvoří absolutní identifikátor URI kombinací hello baseUri a hello relativeUri řetězec.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| baseUri |Ano |Řetězec |řetězec Hello základní identifikátor uri. |
| relativeUri |Ano |Řetězec |Hello relativní identifikátor uri řetězec tooadd toohello základní identifikátor uri řetězec. |

hodnota pro hello Hello **baseUri** parametr může obsahovat konkrétní soubor, ale jenom základní cesta hello se používá při vytváření hello identifikátor URI. Například předávání `http://contoso.com/resources/azuredeploy.json` jako hello baseUri parametr výsledky v základní identifikátor URI služby `http://contoso.com/resources/`.

### <a name="return-value"></a>Návratová hodnota

Řetězec představující hello absolutní identifikátor URI pro základní a relativní hodnoty hello.

### <a name="examples"></a>Příklady

Hello následující příklad ukazuje, jak tooconstruct šablonu vnořené tooa odkaz založená na hodnotě hello hello nadřazené šablony.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| uriOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Řetězec | http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Kóduje identifikátoru URI.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| stringToEncode |Ano |Řetězec |Hodnota tooencode Hello. |

### <a name="return-value"></a>Návratová hodnota

Řetězec hello URI kódovaný hodnotu.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| uriOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Řetězec | http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Vrátí že hodnotu kódovaný řetězec identifikátoru URI.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Ano |Řetězec |Hodnota tooconvert tooa řetězec kódovaný Hello identifikátor URI. |

### <a name="return-value"></a>Návratová hodnota

Řetězec dekódované identifikátoru URI kódovaný hodnotu.

### <a name="examples"></a>Příklady

Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| uriOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |
| componentOutput | Řetězec | http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Řetězec | http://contoso.com/resources/Nested/azuredeploy.JSON |


## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

