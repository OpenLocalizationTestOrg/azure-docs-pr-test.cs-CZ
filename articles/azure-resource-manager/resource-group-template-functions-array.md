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
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="52319-103">Pole a objektu funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="52319-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="52319-104">Resource Manager poskytuje několik funkce pro práci s pole a objekty.</span><span class="sxs-lookup"><span data-stu-id="52319-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="52319-105">pole</span><span class="sxs-lookup"><span data-stu-id="52319-105">array</span></span>](#array)
* [<span data-ttu-id="52319-106">sloučení</span><span class="sxs-lookup"><span data-stu-id="52319-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="52319-107">concat</span><span class="sxs-lookup"><span data-stu-id="52319-107">concat</span></span>](#concat)
* [<span data-ttu-id="52319-108">obsahuje</span><span class="sxs-lookup"><span data-stu-id="52319-108">contains</span></span>](#contains)
* [<span data-ttu-id="52319-109">createArray</span><span class="sxs-lookup"><span data-stu-id="52319-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="52319-110">prázdný</span><span class="sxs-lookup"><span data-stu-id="52319-110">empty</span></span>](#empty)
* [<span data-ttu-id="52319-111">první</span><span class="sxs-lookup"><span data-stu-id="52319-111">first</span></span>](#first)
* [<span data-ttu-id="52319-112">průnik</span><span class="sxs-lookup"><span data-stu-id="52319-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="52319-113">JSON</span><span class="sxs-lookup"><span data-stu-id="52319-113">json</span></span>](#json)
* [<span data-ttu-id="52319-114">poslední</span><span class="sxs-lookup"><span data-stu-id="52319-114">last</span></span>](#last)
* [<span data-ttu-id="52319-115">Délka</span><span class="sxs-lookup"><span data-stu-id="52319-115">length</span></span>](#length)
* [<span data-ttu-id="52319-116">min.</span><span class="sxs-lookup"><span data-stu-id="52319-116">min</span></span>](#min)
* [<span data-ttu-id="52319-117">maximální počet</span><span class="sxs-lookup"><span data-stu-id="52319-117">max</span></span>](#max)
* [<span data-ttu-id="52319-118">rozsah</span><span class="sxs-lookup"><span data-stu-id="52319-118">range</span></span>](#range)
* [<span data-ttu-id="52319-119">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="52319-119">skip</span></span>](#skip)
* [<span data-ttu-id="52319-120">proveďte</span><span class="sxs-lookup"><span data-stu-id="52319-120">take</span></span>](#take)
* [<span data-ttu-id="52319-121">sjednocení</span><span class="sxs-lookup"><span data-stu-id="52319-121">union</span></span>](#union)

<span data-ttu-id="52319-122">tooget pole hodnot řetězec oddělená hodnotu, najdete v části [rozdělení](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="52319-122">tooget an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="52319-123">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="52319-124">Převede pole tooan hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="52319-124">Converts hello value tooan array.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-125">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-125">Parameters</span></span>

| <span data-ttu-id="52319-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-126">Parameter</span></span> | <span data-ttu-id="52319-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-127">Required</span></span> | <span data-ttu-id="52319-128">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-128">Type</span></span> | <span data-ttu-id="52319-129">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="52319-130">convertToArray</span></span> |<span data-ttu-id="52319-131">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-131">Yes</span></span> |<span data-ttu-id="52319-132">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="52319-132">int, string, array, or object</span></span> |<span data-ttu-id="52319-133">Hello hodnotu tooconvert tooan pole.</span><span class="sxs-lookup"><span data-stu-id="52319-133">hello value tooconvert tooan array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-134">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-134">Return value</span></span>

<span data-ttu-id="52319-135">Pole.</span><span class="sxs-lookup"><span data-stu-id="52319-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="52319-136">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-136">Example</span></span>

<span data-ttu-id="52319-137">Hello následující příklad ukazuje, jak toouse hello funkce pole s různými typy.</span><span class="sxs-lookup"><span data-stu-id="52319-137">hello following example shows how toouse hello array function with different types.</span></span>

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

<span data-ttu-id="52319-138">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-138">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-139">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-139">Name</span></span> | <span data-ttu-id="52319-140">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-140">Type</span></span> | <span data-ttu-id="52319-141">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="52319-142">intOutput</span></span> | <span data-ttu-id="52319-143">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-143">Array</span></span> | <span data-ttu-id="52319-144">[1]</span><span class="sxs-lookup"><span data-stu-id="52319-144">[1]</span></span> |
| <span data-ttu-id="52319-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-145">stringOutput</span></span> | <span data-ttu-id="52319-146">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-146">Array</span></span> | <span data-ttu-id="52319-147">["a"]</span><span class="sxs-lookup"><span data-stu-id="52319-147">["a"]</span></span> |
| <span data-ttu-id="52319-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="52319-148">objectOutput</span></span> | <span data-ttu-id="52319-149">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-149">Array</span></span> | <span data-ttu-id="52319-150">[{"a": "b", "c": "d"}]</span><span class="sxs-lookup"><span data-stu-id="52319-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="52319-151">sloučení</span><span class="sxs-lookup"><span data-stu-id="52319-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="52319-152">Vrátí první hodnotu než null z hello parametry.</span><span class="sxs-lookup"><span data-stu-id="52319-152">Returns first non-null value from hello parameters.</span></span> <span data-ttu-id="52319-153">Prázdné řetězce, prázdné pole a prázdné objekty nejsou null.</span><span class="sxs-lookup"><span data-stu-id="52319-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-154">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-154">Parameters</span></span>

| <span data-ttu-id="52319-155">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-155">Parameter</span></span> | <span data-ttu-id="52319-156">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-156">Required</span></span> | <span data-ttu-id="52319-157">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-157">Type</span></span> | <span data-ttu-id="52319-158">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-159">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-159">arg1</span></span> |<span data-ttu-id="52319-160">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-160">Yes</span></span> |<span data-ttu-id="52319-161">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="52319-161">int, string, array, or object</span></span> |<span data-ttu-id="52319-162">Hello první hodnota tootest pro hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="52319-162">hello first value tootest for null.</span></span> |
| <span data-ttu-id="52319-163">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="52319-163">additional args</span></span> |<span data-ttu-id="52319-164">Ne</span><span class="sxs-lookup"><span data-stu-id="52319-164">No</span></span> |<span data-ttu-id="52319-165">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="52319-165">int, string, array, or object</span></span> |<span data-ttu-id="52319-166">Další hodnoty tootest pro hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="52319-166">Additional values tootest for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-167">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-167">Return value</span></span>

<span data-ttu-id="52319-168">Hodnota Hello parametrů hello první jinou hodnotu než null, což může být řetězec, int, pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="52319-168">hello value of hello first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="52319-169">Hodnota Null, pokud jsou všechny parametry hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="52319-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="52319-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-170">Example</span></span>

<span data-ttu-id="52319-171">Hello následující příklad ukazuje výstup hello z různých používá funkci coalesce.</span><span class="sxs-lookup"><span data-stu-id="52319-171">hello following example shows hello output from different uses of coalesce.</span></span>

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

<span data-ttu-id="52319-172">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-172">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-173">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-173">Name</span></span> | <span data-ttu-id="52319-174">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-174">Type</span></span> | <span data-ttu-id="52319-175">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-176">stringOutput</span></span> | <span data-ttu-id="52319-177">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-177">String</span></span> | <span data-ttu-id="52319-178">Výchozí</span><span class="sxs-lookup"><span data-stu-id="52319-178">default</span></span> |
| <span data-ttu-id="52319-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="52319-179">intOutput</span></span> | <span data-ttu-id="52319-180">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-180">Int</span></span> | <span data-ttu-id="52319-181">1</span><span class="sxs-lookup"><span data-stu-id="52319-181">1</span></span> |
| <span data-ttu-id="52319-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="52319-182">objectOutput</span></span> | <span data-ttu-id="52319-183">Objekt</span><span class="sxs-lookup"><span data-stu-id="52319-183">Object</span></span> | <span data-ttu-id="52319-184">{"první": "Výchozí"}</span><span class="sxs-lookup"><span data-stu-id="52319-184">{"first": "default"}</span></span> |
| <span data-ttu-id="52319-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-185">arrayOutput</span></span> | <span data-ttu-id="52319-186">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-186">Array</span></span> | <span data-ttu-id="52319-187">[1]</span><span class="sxs-lookup"><span data-stu-id="52319-187">[1]</span></span> |
| <span data-ttu-id="52319-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="52319-188">emptyOutput</span></span> | <span data-ttu-id="52319-189">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-189">Bool</span></span> | <span data-ttu-id="52319-190">True</span><span class="sxs-lookup"><span data-stu-id="52319-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="52319-191">concat</span><span class="sxs-lookup"><span data-stu-id="52319-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="52319-192">Kombinuje více polí a vrátí hello zřetězených pole, nebo kombinuje více řetězcové hodnoty a vrátí hello zřetězených řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-192">Combines multiple arrays and returns hello concatenated array, or combines multiple string values and returns hello concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="52319-193">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-193">Parameters</span></span>

| <span data-ttu-id="52319-194">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-194">Parameter</span></span> | <span data-ttu-id="52319-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-195">Required</span></span> | <span data-ttu-id="52319-196">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-196">Type</span></span> | <span data-ttu-id="52319-197">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-198">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-198">arg1</span></span> |<span data-ttu-id="52319-199">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-199">Yes</span></span> |<span data-ttu-id="52319-200">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-200">array or string</span></span> |<span data-ttu-id="52319-201">Hello první pole nebo řetězec pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="52319-201">hello first array or string for concatenation.</span></span> |
| <span data-ttu-id="52319-202">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="52319-202">additional arguments</span></span> |<span data-ttu-id="52319-203">Ne</span><span class="sxs-lookup"><span data-stu-id="52319-203">No</span></span> |<span data-ttu-id="52319-204">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-204">array or string</span></span> |<span data-ttu-id="52319-205">Další pole nebo řetězců v sekvenčním pořadí pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="52319-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="52319-206">Tato funkce může trvat libovolný počet argumentů a může přijímat řetězce nebo pole parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="52319-206">This function can take any number of arguments, and can accept either strings or arrays for hello parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="52319-207">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-207">Return value</span></span>
<span data-ttu-id="52319-208">Řetězec nebo pole zřetězených hodnot.</span><span class="sxs-lookup"><span data-stu-id="52319-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="52319-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-209">Example</span></span>

<span data-ttu-id="52319-210">Hello následující příklad ukazuje, jak maticových toocombine dva.</span><span class="sxs-lookup"><span data-stu-id="52319-210">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="52319-211">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-211">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-212">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-212">Name</span></span> | <span data-ttu-id="52319-213">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-213">Type</span></span> | <span data-ttu-id="52319-214">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-215">Vrátí</span><span class="sxs-lookup"><span data-stu-id="52319-215">return</span></span> | <span data-ttu-id="52319-216">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-216">Array</span></span> | <span data-ttu-id="52319-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="52319-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="52319-218">Hello následující příklad ukazuje, jak toocombine dva řetězce hodnoty a vrátí spojený řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-218">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="52319-219">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-219">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-220">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-220">Name</span></span> | <span data-ttu-id="52319-221">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-221">Type</span></span> | <span data-ttu-id="52319-222">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="52319-223">concatOutput</span></span> | <span data-ttu-id="52319-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-224">String</span></span> | <span data-ttu-id="52319-225">Předpona 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="52319-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="52319-226">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="52319-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="52319-227">Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-228">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-228">Parameters</span></span>

| <span data-ttu-id="52319-229">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-229">Parameter</span></span> | <span data-ttu-id="52319-230">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-230">Required</span></span> | <span data-ttu-id="52319-231">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-231">Type</span></span> | <span data-ttu-id="52319-232">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-233">kontejner</span><span class="sxs-lookup"><span data-stu-id="52319-233">container</span></span> |<span data-ttu-id="52319-234">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-234">Yes</span></span> |<span data-ttu-id="52319-235">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-235">array, object, or string</span></span> |<span data-ttu-id="52319-236">Hello hodnotu, která obsahuje hodnotu toofind hello.</span><span class="sxs-lookup"><span data-stu-id="52319-236">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="52319-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="52319-237">itemToFind</span></span> |<span data-ttu-id="52319-238">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-238">Yes</span></span> |<span data-ttu-id="52319-239">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-239">string or int</span></span> |<span data-ttu-id="52319-240">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="52319-240">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-241">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-241">Return value</span></span>

<span data-ttu-id="52319-242">**Hodnota TRUE,** Pokud je položka hello, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="52319-242">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="52319-243">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-243">Example</span></span>

<span data-ttu-id="52319-244">Hello následující příklad ukazuje, jak toouse obsahuje s různými typy:</span><span class="sxs-lookup"><span data-stu-id="52319-244">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="52319-245">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-246">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-246">Name</span></span> | <span data-ttu-id="52319-247">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-247">Type</span></span> | <span data-ttu-id="52319-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="52319-249">stringTrue</span></span> | <span data-ttu-id="52319-250">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-250">Bool</span></span> | <span data-ttu-id="52319-251">True</span><span class="sxs-lookup"><span data-stu-id="52319-251">True</span></span> |
| <span data-ttu-id="52319-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="52319-252">stringFalse</span></span> | <span data-ttu-id="52319-253">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-253">Bool</span></span> | <span data-ttu-id="52319-254">False</span><span class="sxs-lookup"><span data-stu-id="52319-254">False</span></span> |
| <span data-ttu-id="52319-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="52319-255">objectTrue</span></span> | <span data-ttu-id="52319-256">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-256">Bool</span></span> | <span data-ttu-id="52319-257">True</span><span class="sxs-lookup"><span data-stu-id="52319-257">True</span></span> |
| <span data-ttu-id="52319-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="52319-258">objectFalse</span></span> | <span data-ttu-id="52319-259">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-259">Bool</span></span> | <span data-ttu-id="52319-260">False</span><span class="sxs-lookup"><span data-stu-id="52319-260">False</span></span> |
| <span data-ttu-id="52319-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="52319-261">arrayTrue</span></span> | <span data-ttu-id="52319-262">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-262">Bool</span></span> | <span data-ttu-id="52319-263">True</span><span class="sxs-lookup"><span data-stu-id="52319-263">True</span></span> |
| <span data-ttu-id="52319-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="52319-264">arrayFalse</span></span> | <span data-ttu-id="52319-265">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-265">Bool</span></span> | <span data-ttu-id="52319-266">False</span><span class="sxs-lookup"><span data-stu-id="52319-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="52319-267">createarray</span><span class="sxs-lookup"><span data-stu-id="52319-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="52319-268">Vytvoří pole z hello parametry.</span><span class="sxs-lookup"><span data-stu-id="52319-268">Creates an array from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-269">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-269">Parameters</span></span>

| <span data-ttu-id="52319-270">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-270">Parameter</span></span> | <span data-ttu-id="52319-271">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-271">Required</span></span> | <span data-ttu-id="52319-272">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-272">Type</span></span> | <span data-ttu-id="52319-273">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-274">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-274">arg1</span></span> |<span data-ttu-id="52319-275">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-275">Yes</span></span> |<span data-ttu-id="52319-276">Řetězec, celé číslo, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="52319-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="52319-277">Hello první hodnotu v poli hello.</span><span class="sxs-lookup"><span data-stu-id="52319-277">hello first value in hello array.</span></span> |
| <span data-ttu-id="52319-278">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="52319-278">additional arguments</span></span> |<span data-ttu-id="52319-279">Ne</span><span class="sxs-lookup"><span data-stu-id="52319-279">No</span></span> |<span data-ttu-id="52319-280">Řetězec, celé číslo, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="52319-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="52319-281">Další hodnoty v poli hello.</span><span class="sxs-lookup"><span data-stu-id="52319-281">Additional values in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-282">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-282">Return value</span></span>

<span data-ttu-id="52319-283">Pole.</span><span class="sxs-lookup"><span data-stu-id="52319-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="52319-284">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-284">Example</span></span>

<span data-ttu-id="52319-285">Následující příklad ukazuje, jak Hello toouse createArray s různými typy:</span><span class="sxs-lookup"><span data-stu-id="52319-285">hello following example shows how toouse createArray with different types:</span></span>

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

<span data-ttu-id="52319-286">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-286">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-287">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-287">Name</span></span> | <span data-ttu-id="52319-288">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-288">Type</span></span> | <span data-ttu-id="52319-289">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="52319-290">stringArray</span></span> | <span data-ttu-id="52319-291">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-291">Array</span></span> | <span data-ttu-id="52319-292">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="52319-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="52319-293">intArray</span><span class="sxs-lookup"><span data-stu-id="52319-293">intArray</span></span> | <span data-ttu-id="52319-294">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-294">Array</span></span> | <span data-ttu-id="52319-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="52319-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="52319-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="52319-296">objectArray</span></span> | <span data-ttu-id="52319-297">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-297">Array</span></span> | <span data-ttu-id="52319-298">[{"1": "a", "dva": "b", "tři": "c"}]</span><span class="sxs-lookup"><span data-stu-id="52319-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="52319-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="52319-299">arrayArray</span></span> | <span data-ttu-id="52319-300">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-300">Array</span></span> | <span data-ttu-id="52319-301">[["1", "dva", "tři"]]</span><span class="sxs-lookup"><span data-stu-id="52319-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="52319-302">prázdný</span><span class="sxs-lookup"><span data-stu-id="52319-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="52319-303">Určuje, zda je prázdný řetězec, objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="52319-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-304">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-304">Parameters</span></span>

| <span data-ttu-id="52319-305">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-305">Parameter</span></span> | <span data-ttu-id="52319-306">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-306">Required</span></span> | <span data-ttu-id="52319-307">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-307">Type</span></span> | <span data-ttu-id="52319-308">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="52319-309">itemToTest</span></span> |<span data-ttu-id="52319-310">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-310">Yes</span></span> |<span data-ttu-id="52319-311">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-311">array, object, or string</span></span> |<span data-ttu-id="52319-312">Hodnota toocheck Hello, pokud je prázdná.</span><span class="sxs-lookup"><span data-stu-id="52319-312">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-313">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-313">Return value</span></span>

<span data-ttu-id="52319-314">Vrátí **True** Pokud hello hodnota je prázdný, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="52319-314">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="52319-315">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-315">Example</span></span>

<span data-ttu-id="52319-316">Následující ukázka Hello ověří, zda jsou řetězec, objekt a pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="52319-316">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="52319-317">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-317">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-318">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-318">Name</span></span> | <span data-ttu-id="52319-319">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-319">Type</span></span> | <span data-ttu-id="52319-320">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="52319-321">arrayEmpty</span></span> | <span data-ttu-id="52319-322">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-322">Bool</span></span> | <span data-ttu-id="52319-323">True</span><span class="sxs-lookup"><span data-stu-id="52319-323">True</span></span> |
| <span data-ttu-id="52319-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="52319-324">objectEmpty</span></span> | <span data-ttu-id="52319-325">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-325">Bool</span></span> | <span data-ttu-id="52319-326">True</span><span class="sxs-lookup"><span data-stu-id="52319-326">True</span></span> |
| <span data-ttu-id="52319-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="52319-327">stringEmpty</span></span> | <span data-ttu-id="52319-328">BOOL</span><span class="sxs-lookup"><span data-stu-id="52319-328">Bool</span></span> | <span data-ttu-id="52319-329">True</span><span class="sxs-lookup"><span data-stu-id="52319-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="52319-330">první</span><span class="sxs-lookup"><span data-stu-id="52319-330">first</span></span>
`first(arg1)`

<span data-ttu-id="52319-331">Vrátí hello první prvek pole hello nebo první znak hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-331">Returns hello first element of hello array, or first character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-332">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-332">Parameters</span></span>

| <span data-ttu-id="52319-333">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-333">Parameter</span></span> | <span data-ttu-id="52319-334">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-334">Required</span></span> | <span data-ttu-id="52319-335">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-335">Type</span></span> | <span data-ttu-id="52319-336">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-337">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-337">arg1</span></span> |<span data-ttu-id="52319-338">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-338">Yes</span></span> |<span data-ttu-id="52319-339">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-339">array or string</span></span> |<span data-ttu-id="52319-340">Hello hodnota tooretrieve hello první prvek nebo znak.</span><span class="sxs-lookup"><span data-stu-id="52319-340">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-341">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-341">Return value</span></span>

<span data-ttu-id="52319-342">Hello typ (řetězec, int, pole nebo objekt) hello první prvek v poli, nebo první znak hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-342">hello type (string, int, array, or object) of hello first element in an array, or hello first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="52319-343">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-343">Example</span></span>

<span data-ttu-id="52319-344">Hello následující příklad ukazuje, jak toouse hello první funkce s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="52319-344">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="52319-345">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-345">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-346">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-346">Name</span></span> | <span data-ttu-id="52319-347">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-347">Type</span></span> | <span data-ttu-id="52319-348">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-349">arrayOutput</span></span> | <span data-ttu-id="52319-350">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-350">String</span></span> | <span data-ttu-id="52319-351">jeden</span><span class="sxs-lookup"><span data-stu-id="52319-351">one</span></span> |
| <span data-ttu-id="52319-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-352">stringOutput</span></span> | <span data-ttu-id="52319-353">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-353">String</span></span> | <span data-ttu-id="52319-354">O</span><span class="sxs-lookup"><span data-stu-id="52319-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="52319-355">průnik</span><span class="sxs-lookup"><span data-stu-id="52319-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="52319-356">Vrátí jeden pole nebo objekt s společné prvky hello z hello parametry.</span><span class="sxs-lookup"><span data-stu-id="52319-356">Returns a single array or object with hello common elements from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-357">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-357">Parameters</span></span>

| <span data-ttu-id="52319-358">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-358">Parameter</span></span> | <span data-ttu-id="52319-359">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-359">Required</span></span> | <span data-ttu-id="52319-360">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-360">Type</span></span> | <span data-ttu-id="52319-361">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-362">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-362">arg1</span></span> |<span data-ttu-id="52319-363">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-363">Yes</span></span> |<span data-ttu-id="52319-364">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-364">array or object</span></span> |<span data-ttu-id="52319-365">Hello první hodnota toouse pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="52319-365">hello first value toouse for finding common elements.</span></span> |
| <span data-ttu-id="52319-366">arg2</span><span class="sxs-lookup"><span data-stu-id="52319-366">arg2</span></span> |<span data-ttu-id="52319-367">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-367">Yes</span></span> |<span data-ttu-id="52319-368">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-368">array or object</span></span> |<span data-ttu-id="52319-369">Hello druhá hodnota toouse pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="52319-369">hello second value toouse for finding common elements.</span></span> |
| <span data-ttu-id="52319-370">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="52319-370">additional arguments</span></span> |<span data-ttu-id="52319-371">Ne</span><span class="sxs-lookup"><span data-stu-id="52319-371">No</span></span> |<span data-ttu-id="52319-372">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-372">array or object</span></span> |<span data-ttu-id="52319-373">Další hodnoty toouse pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="52319-373">Additional values toouse for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-374">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-374">Return value</span></span>

<span data-ttu-id="52319-375">Pole nebo objekt s společné prvky hello.</span><span class="sxs-lookup"><span data-stu-id="52319-375">An array or object with hello common elements.</span></span>

### <a name="example"></a><span data-ttu-id="52319-376">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-376">Example</span></span>

<span data-ttu-id="52319-377">Následující příklad ukazuje, jak toouse průnik s maticových Hello a objekty:</span><span class="sxs-lookup"><span data-stu-id="52319-377">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="52319-378">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-378">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-379">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-379">Name</span></span> | <span data-ttu-id="52319-380">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-380">Type</span></span> | <span data-ttu-id="52319-381">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="52319-382">objectOutput</span></span> | <span data-ttu-id="52319-383">Objekt</span><span class="sxs-lookup"><span data-stu-id="52319-383">Object</span></span> | <span data-ttu-id="52319-384">{"1": "a", "tři": "c"}</span><span class="sxs-lookup"><span data-stu-id="52319-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="52319-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-385">arrayOutput</span></span> | <span data-ttu-id="52319-386">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-386">Array</span></span> | <span data-ttu-id="52319-387">["dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="52319-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="52319-388">JSON</span><span class="sxs-lookup"><span data-stu-id="52319-388">json</span></span>
`json(arg1)`

<span data-ttu-id="52319-389">Vrátí objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="52319-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-390">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-390">Parameters</span></span>

| <span data-ttu-id="52319-391">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-391">Parameter</span></span> | <span data-ttu-id="52319-392">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-392">Required</span></span> | <span data-ttu-id="52319-393">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-393">Type</span></span> | <span data-ttu-id="52319-394">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-395">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-395">arg1</span></span> |<span data-ttu-id="52319-396">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-396">Yes</span></span> |<span data-ttu-id="52319-397">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-397">string</span></span> |<span data-ttu-id="52319-398">tooJSON tooconvert hodnotu Hello.</span><span class="sxs-lookup"><span data-stu-id="52319-398">hello value tooconvert tooJSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="52319-399">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-399">Return value</span></span>

<span data-ttu-id="52319-400">Zadaný objekt JSON Hello z hello řetězec nebo objekt prázdný při **null** je zadán.</span><span class="sxs-lookup"><span data-stu-id="52319-400">hello JSON object from hello specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="52319-401">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-401">Example</span></span>

<span data-ttu-id="52319-402">Následující příklad ukazuje, jak toouse průnik s maticových Hello a objekty:</span><span class="sxs-lookup"><span data-stu-id="52319-402">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="52319-403">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-403">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-404">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-404">Name</span></span> | <span data-ttu-id="52319-405">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-405">Type</span></span> | <span data-ttu-id="52319-406">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="52319-407">jsonOutput</span></span> | <span data-ttu-id="52319-408">Objekt</span><span class="sxs-lookup"><span data-stu-id="52319-408">Object</span></span> | <span data-ttu-id="52319-409">{"a": "b"}</span><span class="sxs-lookup"><span data-stu-id="52319-409">{"a": "b"}</span></span> |
| <span data-ttu-id="52319-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="52319-410">nullOutput</span></span> | <span data-ttu-id="52319-411">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-411">Boolean</span></span> | <span data-ttu-id="52319-412">True</span><span class="sxs-lookup"><span data-stu-id="52319-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="52319-413">poslední</span><span class="sxs-lookup"><span data-stu-id="52319-413">last</span></span>
`last (arg1)`

<span data-ttu-id="52319-414">Vrátí hello posledním elementem pole hello nebo poslední znak hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-414">Returns hello last element of hello array, or last character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-415">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-415">Parameters</span></span>

| <span data-ttu-id="52319-416">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-416">Parameter</span></span> | <span data-ttu-id="52319-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-417">Required</span></span> | <span data-ttu-id="52319-418">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-418">Type</span></span> | <span data-ttu-id="52319-419">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-420">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-420">arg1</span></span> |<span data-ttu-id="52319-421">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-421">Yes</span></span> |<span data-ttu-id="52319-422">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-422">array or string</span></span> |<span data-ttu-id="52319-423">Hello hodnota tooretrieve hello poslední element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="52319-423">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-424">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-424">Return value</span></span>

<span data-ttu-id="52319-425">Hello typ (řetězec, int, pole nebo objekt) hello posledním prvkem v matici, nebo poslední znak hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-425">hello type (string, int, array, or object) of hello last element in an array, or hello last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="52319-426">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-426">Example</span></span>

<span data-ttu-id="52319-427">Hello následující příklad ukazuje, jak toouse hello poslední funkci s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="52319-427">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="52319-428">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-429">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-429">Name</span></span> | <span data-ttu-id="52319-430">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-430">Type</span></span> | <span data-ttu-id="52319-431">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-432">arrayOutput</span></span> | <span data-ttu-id="52319-433">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-433">String</span></span> | <span data-ttu-id="52319-434">tři</span><span class="sxs-lookup"><span data-stu-id="52319-434">three</span></span> |
| <span data-ttu-id="52319-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-435">stringOutput</span></span> | <span data-ttu-id="52319-436">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-436">String</span></span> | <span data-ttu-id="52319-437">E</span><span class="sxs-lookup"><span data-stu-id="52319-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="52319-438">Délka</span><span class="sxs-lookup"><span data-stu-id="52319-438">length</span></span>
`length(arg1)`

<span data-ttu-id="52319-439">Vrátí hello počet elementů pole nebo znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="52319-439">Returns hello number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-440">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-440">Parameters</span></span>

| <span data-ttu-id="52319-441">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-441">Parameter</span></span> | <span data-ttu-id="52319-442">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-442">Required</span></span> | <span data-ttu-id="52319-443">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-443">Type</span></span> | <span data-ttu-id="52319-444">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-445">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-445">arg1</span></span> |<span data-ttu-id="52319-446">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-446">Yes</span></span> |<span data-ttu-id="52319-447">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-447">array or string</span></span> |<span data-ttu-id="52319-448">Hello toouse pole pro získání hello počet elementů nebo hello toouse řetězec pro získání hello počet znaků.</span><span class="sxs-lookup"><span data-stu-id="52319-448">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-449">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-449">Return value</span></span>

<span data-ttu-id="52319-450">Typ int.</span><span class="sxs-lookup"><span data-stu-id="52319-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="52319-451">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-451">Example</span></span>

<span data-ttu-id="52319-452">Následující příklad ukazuje, jak Hello toouse délka s řetězec a pole:</span><span class="sxs-lookup"><span data-stu-id="52319-452">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="52319-453">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-453">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-454">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-454">Name</span></span> | <span data-ttu-id="52319-455">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-455">Type</span></span> | <span data-ttu-id="52319-456">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="52319-457">arrayLength</span></span> | <span data-ttu-id="52319-458">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-458">Int</span></span> | <span data-ttu-id="52319-459">3</span><span class="sxs-lookup"><span data-stu-id="52319-459">3</span></span> |
| <span data-ttu-id="52319-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="52319-460">stringLength</span></span> | <span data-ttu-id="52319-461">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-461">Int</span></span> | <span data-ttu-id="52319-462">13</span><span class="sxs-lookup"><span data-stu-id="52319-462">13</span></span> |

<span data-ttu-id="52319-463">Při vytváření prostředků, můžete použít tuto funkci s číslem pole toospecify hello iterací.</span><span class="sxs-lookup"><span data-stu-id="52319-463">You can use this function with an array toospecify hello number of iterations when creating resources.</span></span> <span data-ttu-id="52319-464">V následujícím příkladu hello, hello parametr **siteNames** odkazovaly tooan pole názvy toouse při tvorbě webů hello.</span><span class="sxs-lookup"><span data-stu-id="52319-464">In hello following example, hello parameter **siteNames** would refer tooan array of names toouse when creating hello web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="52319-465">Další informace o použití této funkce se pole najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="52319-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="52319-466">min</span><span class="sxs-lookup"><span data-stu-id="52319-466">min</span></span>
`min(arg1)`

<span data-ttu-id="52319-467">Vrátí hello minimální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="52319-467">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-468">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-468">Parameters</span></span>

| <span data-ttu-id="52319-469">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-469">Parameter</span></span> | <span data-ttu-id="52319-470">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-470">Required</span></span> | <span data-ttu-id="52319-471">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-471">Type</span></span> | <span data-ttu-id="52319-472">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-473">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-473">arg1</span></span> |<span data-ttu-id="52319-474">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-474">Yes</span></span> |<span data-ttu-id="52319-475">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="52319-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="52319-476">Hello kolekce tooget hello minimální hodnota.</span><span class="sxs-lookup"><span data-stu-id="52319-476">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-477">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-477">Return value</span></span>

<span data-ttu-id="52319-478">Typ int představující hello minimální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52319-478">An int representing hello minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="52319-479">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-479">Example</span></span>

<span data-ttu-id="52319-480">Následující příklad ukazuje, jak Hello toouse min s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="52319-480">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="52319-481">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-481">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-482">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-482">Name</span></span> | <span data-ttu-id="52319-483">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-483">Type</span></span> | <span data-ttu-id="52319-484">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-485">arrayOutput</span></span> | <span data-ttu-id="52319-486">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-486">Int</span></span> | <span data-ttu-id="52319-487">0</span><span class="sxs-lookup"><span data-stu-id="52319-487">0</span></span> |
| <span data-ttu-id="52319-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="52319-488">intOutput</span></span> | <span data-ttu-id="52319-489">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-489">Int</span></span> | <span data-ttu-id="52319-490">0</span><span class="sxs-lookup"><span data-stu-id="52319-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="52319-491">maximální počet</span><span class="sxs-lookup"><span data-stu-id="52319-491">max</span></span>
`max(arg1)`

<span data-ttu-id="52319-492">Vrátí hello maximální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="52319-492">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-493">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-493">Parameters</span></span>

| <span data-ttu-id="52319-494">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-494">Parameter</span></span> | <span data-ttu-id="52319-495">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-495">Required</span></span> | <span data-ttu-id="52319-496">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-496">Type</span></span> | <span data-ttu-id="52319-497">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-498">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-498">arg1</span></span> |<span data-ttu-id="52319-499">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-499">Yes</span></span> |<span data-ttu-id="52319-500">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="52319-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="52319-501">Hello kolekce tooget hello maximální hodnota.</span><span class="sxs-lookup"><span data-stu-id="52319-501">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-502">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-502">Return value</span></span>

<span data-ttu-id="52319-503">Typ int představující hello maximální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52319-503">An int representing hello maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="52319-504">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-504">Example</span></span>

<span data-ttu-id="52319-505">Následující příklad ukazuje, jak Hello toouse maximální s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="52319-505">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="52319-506">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-506">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-507">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-507">Name</span></span> | <span data-ttu-id="52319-508">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-508">Type</span></span> | <span data-ttu-id="52319-509">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-510">arrayOutput</span></span> | <span data-ttu-id="52319-511">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-511">Int</span></span> | <span data-ttu-id="52319-512">5</span><span class="sxs-lookup"><span data-stu-id="52319-512">5</span></span> |
| <span data-ttu-id="52319-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="52319-513">intOutput</span></span> | <span data-ttu-id="52319-514">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-514">Int</span></span> | <span data-ttu-id="52319-515">5</span><span class="sxs-lookup"><span data-stu-id="52319-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="52319-516">rozsah</span><span class="sxs-lookup"><span data-stu-id="52319-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="52319-517">Vytvoří pole celých čísel od spuštění celé číslo a který obsahuje počet položek.</span><span class="sxs-lookup"><span data-stu-id="52319-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-518">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-518">Parameters</span></span>

| <span data-ttu-id="52319-519">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-519">Parameter</span></span> | <span data-ttu-id="52319-520">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-520">Required</span></span> | <span data-ttu-id="52319-521">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-521">Type</span></span> | <span data-ttu-id="52319-522">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="52319-523">startingInteger</span></span> |<span data-ttu-id="52319-524">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-524">Yes</span></span> |<span data-ttu-id="52319-525">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-525">int</span></span> |<span data-ttu-id="52319-526">Hello první celé číslo v poli hello.</span><span class="sxs-lookup"><span data-stu-id="52319-526">hello first integer in hello array.</span></span> |
| <span data-ttu-id="52319-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="52319-527">numberofElements</span></span> |<span data-ttu-id="52319-528">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-528">Yes</span></span> |<span data-ttu-id="52319-529">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-529">int</span></span> |<span data-ttu-id="52319-530">Hello počet celých čísel v poli hello.</span><span class="sxs-lookup"><span data-stu-id="52319-530">hello number of integers in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-531">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-531">Return value</span></span>

<span data-ttu-id="52319-532">Pole celých čísel.</span><span class="sxs-lookup"><span data-stu-id="52319-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="52319-533">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-533">Example</span></span>

<span data-ttu-id="52319-534">Hello následující příklad ukazuje, jak toouse hello rozsah funkce:</span><span class="sxs-lookup"><span data-stu-id="52319-534">hello following example shows how toouse hello range function:</span></span>

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

<span data-ttu-id="52319-535">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-535">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-536">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-536">Name</span></span> | <span data-ttu-id="52319-537">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-537">Type</span></span> | <span data-ttu-id="52319-538">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="52319-539">rangeOutput</span></span> | <span data-ttu-id="52319-540">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-540">Array</span></span> | <span data-ttu-id="52319-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="52319-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="52319-542">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="52319-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="52319-543">Vrátí pole s všechny elementy hello po zadaný počet v poli hello hello nebo vrátí řetězec s všechny znaky hello po hello zadaný počet v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="52319-543">Returns an array with all hello elements after hello specified number in hello array, or returns a string with all hello characters after hello specified number in hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-544">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-544">Parameters</span></span>

| <span data-ttu-id="52319-545">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-545">Parameter</span></span> | <span data-ttu-id="52319-546">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-546">Required</span></span> | <span data-ttu-id="52319-547">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-547">Type</span></span> | <span data-ttu-id="52319-548">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-549">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-549">originalValue</span></span> |<span data-ttu-id="52319-550">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-550">Yes</span></span> |<span data-ttu-id="52319-551">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-551">array or string</span></span> |<span data-ttu-id="52319-552">Hello toouse pro přeskočení pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-552">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="52319-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="52319-553">numberToSkip</span></span> |<span data-ttu-id="52319-554">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-554">Yes</span></span> |<span data-ttu-id="52319-555">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-555">int</span></span> |<span data-ttu-id="52319-556">Hello počet tooskip elementy nebo znaky.</span><span class="sxs-lookup"><span data-stu-id="52319-556">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="52319-557">Pokud tato hodnota je 0 nebo menší, všechny hello elementy nebo znaků hello hodnoty jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="52319-557">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="52319-558">Pokud je větší než délka hello hello pole nebo řetězec, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-558">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-559">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-559">Return value</span></span>

<span data-ttu-id="52319-560">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="52319-561">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-561">Example</span></span>

<span data-ttu-id="52319-562">Následující příklad přeskočí hello Hello zadaný počet prvků v poli hello a hello zadaný počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="52319-562">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="52319-563">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-563">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-564">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-564">Name</span></span> | <span data-ttu-id="52319-565">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-565">Type</span></span> | <span data-ttu-id="52319-566">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-567">arrayOutput</span></span> | <span data-ttu-id="52319-568">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-568">Array</span></span> | <span data-ttu-id="52319-569">["tři"]</span><span class="sxs-lookup"><span data-stu-id="52319-569">["three"]</span></span> |
| <span data-ttu-id="52319-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-570">stringOutput</span></span> | <span data-ttu-id="52319-571">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-571">String</span></span> | <span data-ttu-id="52319-572">dvě tři</span><span class="sxs-lookup"><span data-stu-id="52319-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="52319-573">proveďte</span><span class="sxs-lookup"><span data-stu-id="52319-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="52319-574">Vrátí pole s hello zadaný počet elementů od hello začátek hello pole nebo řetězec s hello zadaný počet znaků od začátku hello hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-574">Returns an array with hello specified number of elements from hello start of hello array, or a string with hello specified number of characters from hello start of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-575">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-575">Parameters</span></span>

| <span data-ttu-id="52319-576">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-576">Parameter</span></span> | <span data-ttu-id="52319-577">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-577">Required</span></span> | <span data-ttu-id="52319-578">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-578">Type</span></span> | <span data-ttu-id="52319-579">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-580">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-580">originalValue</span></span> |<span data-ttu-id="52319-581">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-581">Yes</span></span> |<span data-ttu-id="52319-582">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-582">array or string</span></span> |<span data-ttu-id="52319-583">Hello pole nebo řetězec elementy hello tootake z.</span><span class="sxs-lookup"><span data-stu-id="52319-583">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="52319-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="52319-584">numberToTake</span></span> |<span data-ttu-id="52319-585">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-585">Yes</span></span> |<span data-ttu-id="52319-586">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52319-586">int</span></span> |<span data-ttu-id="52319-587">Hello počet tootake elementy nebo znaky.</span><span class="sxs-lookup"><span data-stu-id="52319-587">hello number of elements or characters tootake.</span></span> <span data-ttu-id="52319-588">Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="52319-589">Pokud je větší než délka hello hello zadané pole nebo řetězec, vrátí se všechny elementy hello ve hello pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-589">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-590">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-590">Return value</span></span>

<span data-ttu-id="52319-591">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="52319-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="52319-592">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-592">Example</span></span>

<span data-ttu-id="52319-593">Následující příklad trvá hello Hello zadaný počet elementů od hello pole a znaků z řetězce.</span><span class="sxs-lookup"><span data-stu-id="52319-593">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="52319-594">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-594">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-595">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-595">Name</span></span> | <span data-ttu-id="52319-596">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-596">Type</span></span> | <span data-ttu-id="52319-597">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-598">arrayOutput</span></span> | <span data-ttu-id="52319-599">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-599">Array</span></span> | <span data-ttu-id="52319-600">["1", "dva"]</span><span class="sxs-lookup"><span data-stu-id="52319-600">["one", "two"]</span></span> |
| <span data-ttu-id="52319-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="52319-601">stringOutput</span></span> | <span data-ttu-id="52319-602">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52319-602">String</span></span> | <span data-ttu-id="52319-603">na</span><span class="sxs-lookup"><span data-stu-id="52319-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="52319-604">sjednocení</span><span class="sxs-lookup"><span data-stu-id="52319-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="52319-605">Vrátí jeden pole nebo objekt s všechny elementy z hello parametry.</span><span class="sxs-lookup"><span data-stu-id="52319-605">Returns a single array or object with all elements from hello parameters.</span></span> <span data-ttu-id="52319-606">Duplicitní hodnoty nebo klíče jsou jenom jednou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="52319-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="52319-607">Parametry</span><span class="sxs-lookup"><span data-stu-id="52319-607">Parameters</span></span>

| <span data-ttu-id="52319-608">Parametr</span><span class="sxs-lookup"><span data-stu-id="52319-608">Parameter</span></span> | <span data-ttu-id="52319-609">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52319-609">Required</span></span> | <span data-ttu-id="52319-610">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-610">Type</span></span> | <span data-ttu-id="52319-611">Popis</span><span class="sxs-lookup"><span data-stu-id="52319-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="52319-612">arg1</span><span class="sxs-lookup"><span data-stu-id="52319-612">arg1</span></span> |<span data-ttu-id="52319-613">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-613">Yes</span></span> |<span data-ttu-id="52319-614">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-614">array or object</span></span> |<span data-ttu-id="52319-615">Hello první hodnota toouse pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="52319-615">hello first value toouse for joining elements.</span></span> |
| <span data-ttu-id="52319-616">arg2</span><span class="sxs-lookup"><span data-stu-id="52319-616">arg2</span></span> |<span data-ttu-id="52319-617">Ano</span><span class="sxs-lookup"><span data-stu-id="52319-617">Yes</span></span> |<span data-ttu-id="52319-618">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-618">array or object</span></span> |<span data-ttu-id="52319-619">Hello druhá hodnota toouse pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="52319-619">hello second value toouse for joining elements.</span></span> |
| <span data-ttu-id="52319-620">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="52319-620">additional arguments</span></span> |<span data-ttu-id="52319-621">Ne</span><span class="sxs-lookup"><span data-stu-id="52319-621">No</span></span> |<span data-ttu-id="52319-622">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="52319-622">array or object</span></span> |<span data-ttu-id="52319-623">Další hodnoty toouse pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="52319-623">Additional values toouse for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="52319-624">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-624">Return value</span></span>

<span data-ttu-id="52319-625">Pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="52319-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="52319-626">Příklad</span><span class="sxs-lookup"><span data-stu-id="52319-626">Example</span></span>

<span data-ttu-id="52319-627">Následující příklad ukazuje, jak toouse sjednocení s maticových Hello a objekty:</span><span class="sxs-lookup"><span data-stu-id="52319-627">hello following example shows how toouse union with arrays and objects:</span></span>

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

<span data-ttu-id="52319-628">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="52319-628">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="52319-629">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52319-629">Name</span></span> | <span data-ttu-id="52319-630">Typ</span><span class="sxs-lookup"><span data-stu-id="52319-630">Type</span></span> | <span data-ttu-id="52319-631">Hodnota</span><span class="sxs-lookup"><span data-stu-id="52319-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="52319-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="52319-632">objectOutput</span></span> | <span data-ttu-id="52319-633">Objekt</span><span class="sxs-lookup"><span data-stu-id="52319-633">Object</span></span> | <span data-ttu-id="52319-634">{"1": "a", "dva": "b", "tři": "c", "4": "d", "5": "e"}</span><span class="sxs-lookup"><span data-stu-id="52319-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="52319-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="52319-635">arrayOutput</span></span> | <span data-ttu-id="52319-636">Pole</span><span class="sxs-lookup"><span data-stu-id="52319-636">Array</span></span> | <span data-ttu-id="52319-637">["1", "dva", "tři", "čtyři"]</span><span class="sxs-lookup"><span data-stu-id="52319-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="52319-638">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52319-638">Next steps</span></span>
* <span data-ttu-id="52319-639">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="52319-639">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="52319-640">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="52319-640">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="52319-641">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="52319-641">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="52319-642">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="52319-642">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

