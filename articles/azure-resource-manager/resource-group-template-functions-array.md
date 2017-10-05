---
title: "Šablona Azure Resource Manageru funkce – polí a objekty | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manageru pro práci s pole a objekty."
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
ms.openlocfilehash: 0bd9ec41761c9ce575f3bcf4d1f8e8578b83e01c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="e3eb4-103">Pole a objektu funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e3eb4-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="e3eb4-104">Resource Manager poskytuje několik funkce pro práci s pole a objekty.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="e3eb4-105">pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-105">array</span></span>](#array)
* [<span data-ttu-id="e3eb4-106">sloučení</span><span class="sxs-lookup"><span data-stu-id="e3eb4-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="e3eb4-107">concat</span><span class="sxs-lookup"><span data-stu-id="e3eb4-107">concat</span></span>](#concat)
* [<span data-ttu-id="e3eb4-108">obsahuje</span><span class="sxs-lookup"><span data-stu-id="e3eb4-108">contains</span></span>](#contains)
* [<span data-ttu-id="e3eb4-109">createArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="e3eb4-110">prázdný</span><span class="sxs-lookup"><span data-stu-id="e3eb4-110">empty</span></span>](#empty)
* [<span data-ttu-id="e3eb4-111">první</span><span class="sxs-lookup"><span data-stu-id="e3eb4-111">first</span></span>](#first)
* [<span data-ttu-id="e3eb4-112">průnik</span><span class="sxs-lookup"><span data-stu-id="e3eb4-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="e3eb4-113">JSON</span><span class="sxs-lookup"><span data-stu-id="e3eb4-113">json</span></span>](#json)
* [<span data-ttu-id="e3eb4-114">poslední</span><span class="sxs-lookup"><span data-stu-id="e3eb4-114">last</span></span>](#last)
* [<span data-ttu-id="e3eb4-115">Délka</span><span class="sxs-lookup"><span data-stu-id="e3eb4-115">length</span></span>](#length)
* [<span data-ttu-id="e3eb4-116">min.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-116">min</span></span>](#min)
* [<span data-ttu-id="e3eb4-117">maximální počet</span><span class="sxs-lookup"><span data-stu-id="e3eb4-117">max</span></span>](#max)
* [<span data-ttu-id="e3eb4-118">rozsah</span><span class="sxs-lookup"><span data-stu-id="e3eb4-118">range</span></span>](#range)
* [<span data-ttu-id="e3eb4-119">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="e3eb4-119">skip</span></span>](#skip)
* [<span data-ttu-id="e3eb4-120">proveďte</span><span class="sxs-lookup"><span data-stu-id="e3eb4-120">take</span></span>](#take)
* [<span data-ttu-id="e3eb4-121">sjednocení</span><span class="sxs-lookup"><span data-stu-id="e3eb4-121">union</span></span>](#union)

<span data-ttu-id="e3eb4-122">Pole hodnot řetězec oddělená hodnotu získáte v tématu [rozdělení](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-122">To get an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="e3eb4-123">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="e3eb4-124">Převede hodnotu na pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-124">Converts the value to an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-125">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-125">Parameters</span></span>

| <span data-ttu-id="e3eb4-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-126">Parameter</span></span> | <span data-ttu-id="e3eb4-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-127">Required</span></span> | <span data-ttu-id="e3eb4-128">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-128">Type</span></span> | <span data-ttu-id="e3eb4-129">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-130">convertToArray</span></span> |<span data-ttu-id="e3eb4-131">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-131">Yes</span></span> |<span data-ttu-id="e3eb4-132">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-132">int, string, array, or object</span></span> |<span data-ttu-id="e3eb4-133">Hodnota pro převod na pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-133">The value to convert to an array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-134">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-134">Return value</span></span>

<span data-ttu-id="e3eb4-135">Pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-136">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-136">Example</span></span>

<span data-ttu-id="e3eb4-137">Následující příklad ukazuje, jak chcete používat funkci pole s různými typy.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-137">The following example shows how to use the array function with different types.</span></span>

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

<span data-ttu-id="e3eb4-138">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-138">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-139">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-139">Name</span></span> | <span data-ttu-id="e3eb4-140">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-140">Type</span></span> | <span data-ttu-id="e3eb4-141">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-142">intOutput</span></span> | <span data-ttu-id="e3eb4-143">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-143">Array</span></span> | <span data-ttu-id="e3eb4-144">[1]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-144">[1]</span></span> |
| <span data-ttu-id="e3eb4-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-145">stringOutput</span></span> | <span data-ttu-id="e3eb4-146">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-146">Array</span></span> | <span data-ttu-id="e3eb4-147">["a"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-147">["a"]</span></span> |
| <span data-ttu-id="e3eb4-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-148">objectOutput</span></span> | <span data-ttu-id="e3eb4-149">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-149">Array</span></span> | <span data-ttu-id="e3eb4-150">[{"a": "b", "c": "d"}]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="e3eb4-151">sloučení</span><span class="sxs-lookup"><span data-stu-id="e3eb4-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="e3eb4-152">Vrátí první hodnotu než null z parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-152">Returns first non-null value from the parameters.</span></span> <span data-ttu-id="e3eb4-153">Prázdné řetězce, prázdné pole a prázdné objekty nejsou null.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-154">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-154">Parameters</span></span>

| <span data-ttu-id="e3eb4-155">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-155">Parameter</span></span> | <span data-ttu-id="e3eb4-156">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-156">Required</span></span> | <span data-ttu-id="e3eb4-157">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-157">Type</span></span> | <span data-ttu-id="e3eb4-158">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-159">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-159">arg1</span></span> |<span data-ttu-id="e3eb4-160">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-160">Yes</span></span> |<span data-ttu-id="e3eb4-161">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-161">int, string, array, or object</span></span> |<span data-ttu-id="e3eb4-162">První hodnota k testování pro hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-162">The first value to test for null.</span></span> |
| <span data-ttu-id="e3eb4-163">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-163">additional args</span></span> |<span data-ttu-id="e3eb4-164">Ne</span><span class="sxs-lookup"><span data-stu-id="e3eb4-164">No</span></span> |<span data-ttu-id="e3eb4-165">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-165">int, string, array, or object</span></span> |<span data-ttu-id="e3eb4-166">Další hodnoty, které chcete otestovat hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-166">Additional values to test for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-167">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-167">Return value</span></span>

<span data-ttu-id="e3eb4-168">Hodnota první parametry jinou hodnotu než null, což může být řetězec, int, pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-168">The value of the first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="e3eb4-169">Hodnota Null, pokud jsou všechny parametry hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="e3eb4-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-170">Example</span></span>

<span data-ttu-id="e3eb4-171">Následující příklad ukazuje výstup z různých používá funkci coalesce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-171">The following example shows the output from different uses of coalesce.</span></span>

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

<span data-ttu-id="e3eb4-172">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-172">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-173">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-173">Name</span></span> | <span data-ttu-id="e3eb4-174">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-174">Type</span></span> | <span data-ttu-id="e3eb4-175">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-176">stringOutput</span></span> | <span data-ttu-id="e3eb4-177">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-177">String</span></span> | <span data-ttu-id="e3eb4-178">Výchozí</span><span class="sxs-lookup"><span data-stu-id="e3eb4-178">default</span></span> |
| <span data-ttu-id="e3eb4-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-179">intOutput</span></span> | <span data-ttu-id="e3eb4-180">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-180">Int</span></span> | <span data-ttu-id="e3eb4-181">1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-181">1</span></span> |
| <span data-ttu-id="e3eb4-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-182">objectOutput</span></span> | <span data-ttu-id="e3eb4-183">Objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-183">Object</span></span> | <span data-ttu-id="e3eb4-184">{"první": "Výchozí"}</span><span class="sxs-lookup"><span data-stu-id="e3eb4-184">{"first": "default"}</span></span> |
| <span data-ttu-id="e3eb4-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-185">arrayOutput</span></span> | <span data-ttu-id="e3eb4-186">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-186">Array</span></span> | <span data-ttu-id="e3eb4-187">[1]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-187">[1]</span></span> |
| <span data-ttu-id="e3eb4-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-188">emptyOutput</span></span> | <span data-ttu-id="e3eb4-189">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-189">Bool</span></span> | <span data-ttu-id="e3eb4-190">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="e3eb4-191">concat</span><span class="sxs-lookup"><span data-stu-id="e3eb4-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="e3eb4-192">Kombinuje několik polí a vrátí pole zřetězených nebo kombinuje více řetězcové hodnoty a vrací spojený řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-192">Combines multiple arrays and returns the concatenated array, or combines multiple string values and returns the concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="e3eb4-193">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-193">Parameters</span></span>

| <span data-ttu-id="e3eb4-194">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-194">Parameter</span></span> | <span data-ttu-id="e3eb4-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-195">Required</span></span> | <span data-ttu-id="e3eb4-196">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-196">Type</span></span> | <span data-ttu-id="e3eb4-197">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-198">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-198">arg1</span></span> |<span data-ttu-id="e3eb4-199">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-199">Yes</span></span> |<span data-ttu-id="e3eb4-200">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-200">array or string</span></span> |<span data-ttu-id="e3eb4-201">První pole nebo řetězec pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-201">The first array or string for concatenation.</span></span> |
| <span data-ttu-id="e3eb4-202">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-202">additional arguments</span></span> |<span data-ttu-id="e3eb4-203">Ne</span><span class="sxs-lookup"><span data-stu-id="e3eb4-203">No</span></span> |<span data-ttu-id="e3eb4-204">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-204">array or string</span></span> |<span data-ttu-id="e3eb4-205">Další pole nebo řetězců v sekvenčním pořadí pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="e3eb4-206">Tato funkce může trvat libovolný počet argumentů a může přijímat řetězce nebo pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-206">This function can take any number of arguments, and can accept either strings or arrays for the parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="e3eb4-207">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-207">Return value</span></span>
<span data-ttu-id="e3eb4-208">Řetězec nebo pole zřetězených hodnot.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-209">Example</span></span>

<span data-ttu-id="e3eb4-210">Následující příklad ukazuje, jak kombinovat dvěma poli.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-210">The following example shows how to combine two arrays.</span></span>

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

<span data-ttu-id="e3eb4-211">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-211">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-212">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-212">Name</span></span> | <span data-ttu-id="e3eb4-213">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-213">Type</span></span> | <span data-ttu-id="e3eb4-214">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-215">Vrátí</span><span class="sxs-lookup"><span data-stu-id="e3eb4-215">return</span></span> | <span data-ttu-id="e3eb4-216">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-216">Array</span></span> | <span data-ttu-id="e3eb4-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="e3eb4-218">Následující příklad ukazuje, jak kombinovat dvou řetězcových hodnot a vrátí spojený řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-218">The following example shows how to combine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="e3eb4-219">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-219">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-220">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-220">Name</span></span> | <span data-ttu-id="e3eb4-221">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-221">Type</span></span> | <span data-ttu-id="e3eb4-222">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-223">concatOutput</span></span> | <span data-ttu-id="e3eb4-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-224">String</span></span> | <span data-ttu-id="e3eb4-225">Předpona 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="e3eb4-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="e3eb4-226">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="e3eb4-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="e3eb4-227">Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-228">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-228">Parameters</span></span>

| <span data-ttu-id="e3eb4-229">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-229">Parameter</span></span> | <span data-ttu-id="e3eb4-230">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-230">Required</span></span> | <span data-ttu-id="e3eb4-231">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-231">Type</span></span> | <span data-ttu-id="e3eb4-232">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-233">kontejner</span><span class="sxs-lookup"><span data-stu-id="e3eb4-233">container</span></span> |<span data-ttu-id="e3eb4-234">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-234">Yes</span></span> |<span data-ttu-id="e3eb4-235">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-235">array, object, or string</span></span> |<span data-ttu-id="e3eb4-236">Hodnota, která obsahuje hodnotu k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-236">The value that contains the value to find.</span></span> |
| <span data-ttu-id="e3eb4-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="e3eb4-237">itemToFind</span></span> |<span data-ttu-id="e3eb4-238">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-238">Yes</span></span> |<span data-ttu-id="e3eb4-239">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-239">string or int</span></span> |<span data-ttu-id="e3eb4-240">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-240">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-241">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-241">Return value</span></span>

<span data-ttu-id="e3eb4-242">**Hodnota TRUE,** Pokud je položka, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-242">**True** if the item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-243">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-243">Example</span></span>

<span data-ttu-id="e3eb4-244">Následující příklad ukazuje, jak používat s různými typy obsahuje:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-244">The following example shows how to use contains with different types:</span></span>

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

<span data-ttu-id="e3eb4-245">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-245">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-246">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-246">Name</span></span> | <span data-ttu-id="e3eb4-247">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-247">Type</span></span> | <span data-ttu-id="e3eb4-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="e3eb4-249">stringTrue</span></span> | <span data-ttu-id="e3eb4-250">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-250">Bool</span></span> | <span data-ttu-id="e3eb4-251">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-251">True</span></span> |
| <span data-ttu-id="e3eb4-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="e3eb4-252">stringFalse</span></span> | <span data-ttu-id="e3eb4-253">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-253">Bool</span></span> | <span data-ttu-id="e3eb4-254">False</span><span class="sxs-lookup"><span data-stu-id="e3eb4-254">False</span></span> |
| <span data-ttu-id="e3eb4-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="e3eb4-255">objectTrue</span></span> | <span data-ttu-id="e3eb4-256">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-256">Bool</span></span> | <span data-ttu-id="e3eb4-257">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-257">True</span></span> |
| <span data-ttu-id="e3eb4-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="e3eb4-258">objectFalse</span></span> | <span data-ttu-id="e3eb4-259">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-259">Bool</span></span> | <span data-ttu-id="e3eb4-260">False</span><span class="sxs-lookup"><span data-stu-id="e3eb4-260">False</span></span> |
| <span data-ttu-id="e3eb4-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="e3eb4-261">arrayTrue</span></span> | <span data-ttu-id="e3eb4-262">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-262">Bool</span></span> | <span data-ttu-id="e3eb4-263">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-263">True</span></span> |
| <span data-ttu-id="e3eb4-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="e3eb4-264">arrayFalse</span></span> | <span data-ttu-id="e3eb4-265">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-265">Bool</span></span> | <span data-ttu-id="e3eb4-266">False</span><span class="sxs-lookup"><span data-stu-id="e3eb4-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="e3eb4-267">createarray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="e3eb4-268">Vytvoří pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-268">Creates an array from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-269">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-269">Parameters</span></span>

| <span data-ttu-id="e3eb4-270">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-270">Parameter</span></span> | <span data-ttu-id="e3eb4-271">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-271">Required</span></span> | <span data-ttu-id="e3eb4-272">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-272">Type</span></span> | <span data-ttu-id="e3eb4-273">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-274">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-274">arg1</span></span> |<span data-ttu-id="e3eb4-275">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-275">Yes</span></span> |<span data-ttu-id="e3eb4-276">Řetězec, celé číslo, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="e3eb4-277">První hodnota v poli.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-277">The first value in the array.</span></span> |
| <span data-ttu-id="e3eb4-278">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-278">additional arguments</span></span> |<span data-ttu-id="e3eb4-279">Ne</span><span class="sxs-lookup"><span data-stu-id="e3eb4-279">No</span></span> |<span data-ttu-id="e3eb4-280">Řetězec, celé číslo, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="e3eb4-281">Další hodnoty v poli.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-281">Additional values in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-282">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-282">Return value</span></span>

<span data-ttu-id="e3eb4-283">Pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-284">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-284">Example</span></span>

<span data-ttu-id="e3eb4-285">Následující příklad ukazuje, jak používat createArray s různými typy:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-285">The following example shows how to use createArray with different types:</span></span>

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

<span data-ttu-id="e3eb4-286">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-286">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-287">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-287">Name</span></span> | <span data-ttu-id="e3eb4-288">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-288">Type</span></span> | <span data-ttu-id="e3eb4-289">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-290">stringArray</span></span> | <span data-ttu-id="e3eb4-291">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-291">Array</span></span> | <span data-ttu-id="e3eb4-292">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="e3eb4-293">intArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-293">intArray</span></span> | <span data-ttu-id="e3eb4-294">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-294">Array</span></span> | <span data-ttu-id="e3eb4-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="e3eb4-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-296">objectArray</span></span> | <span data-ttu-id="e3eb4-297">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-297">Array</span></span> | <span data-ttu-id="e3eb4-298">[{"1": "a", "dva": "b", "tři": "c"}]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="e3eb4-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="e3eb4-299">arrayArray</span></span> | <span data-ttu-id="e3eb4-300">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-300">Array</span></span> | <span data-ttu-id="e3eb4-301">[["1", "dva", "tři"]]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="e3eb4-302">prázdný</span><span class="sxs-lookup"><span data-stu-id="e3eb4-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="e3eb4-303">Určuje, zda je prázdný řetězec, objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-304">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-304">Parameters</span></span>

| <span data-ttu-id="e3eb4-305">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-305">Parameter</span></span> | <span data-ttu-id="e3eb4-306">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-306">Required</span></span> | <span data-ttu-id="e3eb4-307">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-307">Type</span></span> | <span data-ttu-id="e3eb4-308">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="e3eb4-309">itemToTest</span></span> |<span data-ttu-id="e3eb4-310">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-310">Yes</span></span> |<span data-ttu-id="e3eb4-311">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-311">array, object, or string</span></span> |<span data-ttu-id="e3eb4-312">Hodnota ke kontrole, jestli je prázdný.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-312">The value to check if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-313">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-313">Return value</span></span>

<span data-ttu-id="e3eb4-314">Vrátí **True** Pokud hodnota je prázdný, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-314">Returns **True** if the value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-315">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-315">Example</span></span>

<span data-ttu-id="e3eb4-316">Následující příklad ověří, zda jsou řetězec, objekt a pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-316">The following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="e3eb4-317">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-317">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-318">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-318">Name</span></span> | <span data-ttu-id="e3eb4-319">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-319">Type</span></span> | <span data-ttu-id="e3eb4-320">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-321">arrayEmpty</span></span> | <span data-ttu-id="e3eb4-322">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-322">Bool</span></span> | <span data-ttu-id="e3eb4-323">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-323">True</span></span> |
| <span data-ttu-id="e3eb4-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-324">objectEmpty</span></span> | <span data-ttu-id="e3eb4-325">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-325">Bool</span></span> | <span data-ttu-id="e3eb4-326">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-326">True</span></span> |
| <span data-ttu-id="e3eb4-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-327">stringEmpty</span></span> | <span data-ttu-id="e3eb4-328">BOOL</span><span class="sxs-lookup"><span data-stu-id="e3eb4-328">Bool</span></span> | <span data-ttu-id="e3eb4-329">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="e3eb4-330">první</span><span class="sxs-lookup"><span data-stu-id="e3eb4-330">first</span></span>
`first(arg1)`

<span data-ttu-id="e3eb4-331">Vrátí první prvek pole nebo první znak řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-331">Returns the first element of the array, or first character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-332">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-332">Parameters</span></span>

| <span data-ttu-id="e3eb4-333">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-333">Parameter</span></span> | <span data-ttu-id="e3eb4-334">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-334">Required</span></span> | <span data-ttu-id="e3eb4-335">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-335">Type</span></span> | <span data-ttu-id="e3eb4-336">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-337">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-337">arg1</span></span> |<span data-ttu-id="e3eb4-338">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-338">Yes</span></span> |<span data-ttu-id="e3eb4-339">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-339">array or string</span></span> |<span data-ttu-id="e3eb4-340">Hodnota k načtení první element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-340">The value to retrieve the first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-341">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-341">Return value</span></span>

<span data-ttu-id="e3eb4-342">Typ (řetězec, int, pole nebo objekt) první prvek pole nebo první znak řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-342">The type (string, int, array, or object) of the first element in an array, or the first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-343">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-343">Example</span></span>

<span data-ttu-id="e3eb4-344">Následující příklad ukazuje, jak používat funkci první s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-344">The following example shows how to use the first function with an array and string.</span></span>

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

<span data-ttu-id="e3eb4-345">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-345">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-346">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-346">Name</span></span> | <span data-ttu-id="e3eb4-347">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-347">Type</span></span> | <span data-ttu-id="e3eb4-348">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-349">arrayOutput</span></span> | <span data-ttu-id="e3eb4-350">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-350">String</span></span> | <span data-ttu-id="e3eb4-351">jeden</span><span class="sxs-lookup"><span data-stu-id="e3eb4-351">one</span></span> |
| <span data-ttu-id="e3eb4-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-352">stringOutput</span></span> | <span data-ttu-id="e3eb4-353">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-353">String</span></span> | <span data-ttu-id="e3eb4-354">O</span><span class="sxs-lookup"><span data-stu-id="e3eb4-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="e3eb4-355">průnik</span><span class="sxs-lookup"><span data-stu-id="e3eb4-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="e3eb4-356">Vrátí objekt s běžných elementech nebo jednoho pole z parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-356">Returns a single array or object with the common elements from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-357">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-357">Parameters</span></span>

| <span data-ttu-id="e3eb4-358">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-358">Parameter</span></span> | <span data-ttu-id="e3eb4-359">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-359">Required</span></span> | <span data-ttu-id="e3eb4-360">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-360">Type</span></span> | <span data-ttu-id="e3eb4-361">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-362">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-362">arg1</span></span> |<span data-ttu-id="e3eb4-363">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-363">Yes</span></span> |<span data-ttu-id="e3eb4-364">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-364">array or object</span></span> |<span data-ttu-id="e3eb4-365">První hodnota, kterou chcete použít pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-365">The first value to use for finding common elements.</span></span> |
| <span data-ttu-id="e3eb4-366">arg2</span><span class="sxs-lookup"><span data-stu-id="e3eb4-366">arg2</span></span> |<span data-ttu-id="e3eb4-367">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-367">Yes</span></span> |<span data-ttu-id="e3eb4-368">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-368">array or object</span></span> |<span data-ttu-id="e3eb4-369">Druhá hodnota, kterou chcete použít pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-369">The second value to use for finding common elements.</span></span> |
| <span data-ttu-id="e3eb4-370">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-370">additional arguments</span></span> |<span data-ttu-id="e3eb4-371">Ne</span><span class="sxs-lookup"><span data-stu-id="e3eb4-371">No</span></span> |<span data-ttu-id="e3eb4-372">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-372">array or object</span></span> |<span data-ttu-id="e3eb4-373">Další hodnoty, které chcete použít pro hledání běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-373">Additional values to use for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-374">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-374">Return value</span></span>

<span data-ttu-id="e3eb4-375">Pole nebo objekt s běžných elementech.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-375">An array or object with the common elements.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-376">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-376">Example</span></span>

<span data-ttu-id="e3eb4-377">Následující příklad ukazuje, jak použít průnik s pole a objekty:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-377">The following example shows how to use intersection with arrays and objects:</span></span>

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

<span data-ttu-id="e3eb4-378">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-378">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-379">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-379">Name</span></span> | <span data-ttu-id="e3eb4-380">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-380">Type</span></span> | <span data-ttu-id="e3eb4-381">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-382">objectOutput</span></span> | <span data-ttu-id="e3eb4-383">Objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-383">Object</span></span> | <span data-ttu-id="e3eb4-384">{"1": "a", "tři": "c"}</span><span class="sxs-lookup"><span data-stu-id="e3eb4-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="e3eb4-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-385">arrayOutput</span></span> | <span data-ttu-id="e3eb4-386">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-386">Array</span></span> | <span data-ttu-id="e3eb4-387">["dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="e3eb4-388">JSON</span><span class="sxs-lookup"><span data-stu-id="e3eb4-388">json</span></span>
`json(arg1)`

<span data-ttu-id="e3eb4-389">Vrátí objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-390">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-390">Parameters</span></span>

| <span data-ttu-id="e3eb4-391">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-391">Parameter</span></span> | <span data-ttu-id="e3eb4-392">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-392">Required</span></span> | <span data-ttu-id="e3eb4-393">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-393">Type</span></span> | <span data-ttu-id="e3eb4-394">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-395">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-395">arg1</span></span> |<span data-ttu-id="e3eb4-396">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-396">Yes</span></span> |<span data-ttu-id="e3eb4-397">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-397">string</span></span> |<span data-ttu-id="e3eb4-398">Hodnota k převést na JSON.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-398">The value to convert to JSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="e3eb4-399">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-399">Return value</span></span>

<span data-ttu-id="e3eb4-400">Objekt JSON v zadaný řetězec nebo objekt prázdný při **null** je zadán.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-400">The JSON object from the specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-401">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-401">Example</span></span>

<span data-ttu-id="e3eb4-402">Následující příklad ukazuje, jak použít průnik s pole a objekty:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-402">The following example shows how to use intersection with arrays and objects:</span></span>

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

<span data-ttu-id="e3eb4-403">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-403">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-404">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-404">Name</span></span> | <span data-ttu-id="e3eb4-405">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-405">Type</span></span> | <span data-ttu-id="e3eb4-406">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-407">jsonOutput</span></span> | <span data-ttu-id="e3eb4-408">Objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-408">Object</span></span> | <span data-ttu-id="e3eb4-409">{"a": "b"}</span><span class="sxs-lookup"><span data-stu-id="e3eb4-409">{"a": "b"}</span></span> |
| <span data-ttu-id="e3eb4-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-410">nullOutput</span></span> | <span data-ttu-id="e3eb4-411">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-411">Boolean</span></span> | <span data-ttu-id="e3eb4-412">True</span><span class="sxs-lookup"><span data-stu-id="e3eb4-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="e3eb4-413">poslední</span><span class="sxs-lookup"><span data-stu-id="e3eb4-413">last</span></span>
`last (arg1)`

<span data-ttu-id="e3eb4-414">Vrátí poslední element pole nebo poslední znak řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-414">Returns the last element of the array, or last character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-415">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-415">Parameters</span></span>

| <span data-ttu-id="e3eb4-416">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-416">Parameter</span></span> | <span data-ttu-id="e3eb4-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-417">Required</span></span> | <span data-ttu-id="e3eb4-418">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-418">Type</span></span> | <span data-ttu-id="e3eb4-419">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-420">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-420">arg1</span></span> |<span data-ttu-id="e3eb4-421">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-421">Yes</span></span> |<span data-ttu-id="e3eb4-422">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-422">array or string</span></span> |<span data-ttu-id="e3eb4-423">Hodnota k načtení poslední element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-423">The value to retrieve the last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-424">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-424">Return value</span></span>

<span data-ttu-id="e3eb4-425">Typ (řetězec, int, pole nebo objekt) posledním prvkem v pole nebo poslední znak řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-425">The type (string, int, array, or object) of the last element in an array, or the last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-426">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-426">Example</span></span>

<span data-ttu-id="e3eb4-427">Následující příklad ukazuje, jak používat funkci naposledy s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-427">The following example shows how to use the last function with an array and string.</span></span>

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

<span data-ttu-id="e3eb4-428">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-428">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-429">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-429">Name</span></span> | <span data-ttu-id="e3eb4-430">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-430">Type</span></span> | <span data-ttu-id="e3eb4-431">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-432">arrayOutput</span></span> | <span data-ttu-id="e3eb4-433">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-433">String</span></span> | <span data-ttu-id="e3eb4-434">tři</span><span class="sxs-lookup"><span data-stu-id="e3eb4-434">three</span></span> |
| <span data-ttu-id="e3eb4-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-435">stringOutput</span></span> | <span data-ttu-id="e3eb4-436">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-436">String</span></span> | <span data-ttu-id="e3eb4-437">E</span><span class="sxs-lookup"><span data-stu-id="e3eb4-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="e3eb4-438">Délka</span><span class="sxs-lookup"><span data-stu-id="e3eb4-438">length</span></span>
`length(arg1)`

<span data-ttu-id="e3eb4-439">Vrátí počet prvků pole nebo znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-439">Returns the number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-440">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-440">Parameters</span></span>

| <span data-ttu-id="e3eb4-441">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-441">Parameter</span></span> | <span data-ttu-id="e3eb4-442">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-442">Required</span></span> | <span data-ttu-id="e3eb4-443">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-443">Type</span></span> | <span data-ttu-id="e3eb4-444">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-445">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-445">arg1</span></span> |<span data-ttu-id="e3eb4-446">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-446">Yes</span></span> |<span data-ttu-id="e3eb4-447">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-447">array or string</span></span> |<span data-ttu-id="e3eb4-448">Pole na použití pro získání počet elementů nebo řetězec Pokud chcete použít pro maximální počet znaků.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-448">The array to use for getting the number of elements, or the string to use for getting the number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-449">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-449">Return value</span></span>

<span data-ttu-id="e3eb4-450">Typ int.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="e3eb4-451">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-451">Example</span></span>

<span data-ttu-id="e3eb4-452">Následující příklad ukazuje, jak používat délka s řetězec a pole:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-452">The following example shows how to use length with an array and string:</span></span>

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

<span data-ttu-id="e3eb4-453">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-453">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-454">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-454">Name</span></span> | <span data-ttu-id="e3eb4-455">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-455">Type</span></span> | <span data-ttu-id="e3eb4-456">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="e3eb4-457">arrayLength</span></span> | <span data-ttu-id="e3eb4-458">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-458">Int</span></span> | <span data-ttu-id="e3eb4-459">3</span><span class="sxs-lookup"><span data-stu-id="e3eb4-459">3</span></span> |
| <span data-ttu-id="e3eb4-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="e3eb4-460">stringLength</span></span> | <span data-ttu-id="e3eb4-461">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-461">Int</span></span> | <span data-ttu-id="e3eb4-462">13</span><span class="sxs-lookup"><span data-stu-id="e3eb4-462">13</span></span> |

<span data-ttu-id="e3eb4-463">Tato funkce se pole vám pomůže při vytváření prostředků zadat počet opakování.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-463">You can use this function with an array to specify the number of iterations when creating resources.</span></span> <span data-ttu-id="e3eb4-464">V následujícím příkladu, parametr **siteNames** by odkazovat na pole názvů pro použití při vytváření webových serverů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-464">In the following example, the parameter **siteNames** would refer to an array of names to use when creating the web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="e3eb4-465">Další informace o použití této funkce se pole najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="e3eb4-466">min</span><span class="sxs-lookup"><span data-stu-id="e3eb4-466">min</span></span>
`min(arg1)`

<span data-ttu-id="e3eb4-467">Vrátí minimální hodnotu z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-467">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-468">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-468">Parameters</span></span>

| <span data-ttu-id="e3eb4-469">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-469">Parameter</span></span> | <span data-ttu-id="e3eb4-470">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-470">Required</span></span> | <span data-ttu-id="e3eb4-471">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-471">Type</span></span> | <span data-ttu-id="e3eb4-472">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-473">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-473">arg1</span></span> |<span data-ttu-id="e3eb4-474">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-474">Yes</span></span> |<span data-ttu-id="e3eb4-475">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="e3eb4-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="e3eb4-476">Kolekce získat minimální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-476">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-477">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-477">Return value</span></span>

<span data-ttu-id="e3eb4-478">Typ int představující minimální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-478">An int representing the minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-479">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-479">Example</span></span>

<span data-ttu-id="e3eb4-480">Následující příklad ukazuje, jak použít min s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-480">The following example shows how to use min with an array and a list of integers:</span></span>

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

<span data-ttu-id="e3eb4-481">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-481">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-482">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-482">Name</span></span> | <span data-ttu-id="e3eb4-483">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-483">Type</span></span> | <span data-ttu-id="e3eb4-484">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-485">arrayOutput</span></span> | <span data-ttu-id="e3eb4-486">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-486">Int</span></span> | <span data-ttu-id="e3eb4-487">0</span><span class="sxs-lookup"><span data-stu-id="e3eb4-487">0</span></span> |
| <span data-ttu-id="e3eb4-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-488">intOutput</span></span> | <span data-ttu-id="e3eb4-489">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-489">Int</span></span> | <span data-ttu-id="e3eb4-490">0</span><span class="sxs-lookup"><span data-stu-id="e3eb4-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="e3eb4-491">maximální počet</span><span class="sxs-lookup"><span data-stu-id="e3eb4-491">max</span></span>
`max(arg1)`

<span data-ttu-id="e3eb4-492">Vrací maximální hodnotu z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-492">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-493">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-493">Parameters</span></span>

| <span data-ttu-id="e3eb4-494">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-494">Parameter</span></span> | <span data-ttu-id="e3eb4-495">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-495">Required</span></span> | <span data-ttu-id="e3eb4-496">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-496">Type</span></span> | <span data-ttu-id="e3eb4-497">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-498">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-498">arg1</span></span> |<span data-ttu-id="e3eb4-499">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-499">Yes</span></span> |<span data-ttu-id="e3eb4-500">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="e3eb4-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="e3eb4-501">Kolekce získat maximální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-501">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-502">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-502">Return value</span></span>

<span data-ttu-id="e3eb4-503">Typ int představující maximální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-503">An int representing the maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-504">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-504">Example</span></span>

<span data-ttu-id="e3eb4-505">Následující příklad ukazuje, jak použít maximum s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-505">The following example shows how to use max with an array and a list of integers:</span></span>

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

<span data-ttu-id="e3eb4-506">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-506">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-507">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-507">Name</span></span> | <span data-ttu-id="e3eb4-508">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-508">Type</span></span> | <span data-ttu-id="e3eb4-509">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-510">arrayOutput</span></span> | <span data-ttu-id="e3eb4-511">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-511">Int</span></span> | <span data-ttu-id="e3eb4-512">5</span><span class="sxs-lookup"><span data-stu-id="e3eb4-512">5</span></span> |
| <span data-ttu-id="e3eb4-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-513">intOutput</span></span> | <span data-ttu-id="e3eb4-514">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-514">Int</span></span> | <span data-ttu-id="e3eb4-515">5</span><span class="sxs-lookup"><span data-stu-id="e3eb4-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="e3eb4-516">rozsah</span><span class="sxs-lookup"><span data-stu-id="e3eb4-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="e3eb4-517">Vytvoří pole celých čísel od spuštění celé číslo a který obsahuje počet položek.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-518">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-518">Parameters</span></span>

| <span data-ttu-id="e3eb4-519">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-519">Parameter</span></span> | <span data-ttu-id="e3eb4-520">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-520">Required</span></span> | <span data-ttu-id="e3eb4-521">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-521">Type</span></span> | <span data-ttu-id="e3eb4-522">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="e3eb4-523">startingInteger</span></span> |<span data-ttu-id="e3eb4-524">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-524">Yes</span></span> |<span data-ttu-id="e3eb4-525">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-525">int</span></span> |<span data-ttu-id="e3eb4-526">První celé číslo v poli.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-526">The first integer in the array.</span></span> |
| <span data-ttu-id="e3eb4-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="e3eb4-527">numberofElements</span></span> |<span data-ttu-id="e3eb4-528">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-528">Yes</span></span> |<span data-ttu-id="e3eb4-529">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-529">int</span></span> |<span data-ttu-id="e3eb4-530">Počet celých čísel v poli.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-530">The number of integers in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-531">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-531">Return value</span></span>

<span data-ttu-id="e3eb4-532">Pole celých čísel.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-533">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-533">Example</span></span>

<span data-ttu-id="e3eb4-534">Následující příklad ukazuje, jak používat funkci rozsahu:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-534">The following example shows how to use the range function:</span></span>

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

<span data-ttu-id="e3eb4-535">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-535">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-536">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-536">Name</span></span> | <span data-ttu-id="e3eb4-537">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-537">Type</span></span> | <span data-ttu-id="e3eb4-538">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-539">rangeOutput</span></span> | <span data-ttu-id="e3eb4-540">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-540">Array</span></span> | <span data-ttu-id="e3eb4-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="e3eb4-542">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="e3eb4-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="e3eb4-543">Vrátí pole s všechny elementy po zadaný počet v poli nebo vrátí řetězec s odebranými znaky po zadaný počet v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-543">Returns an array with all the elements after the specified number in the array, or returns a string with all the characters after the specified number in the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-544">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-544">Parameters</span></span>

| <span data-ttu-id="e3eb4-545">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-545">Parameter</span></span> | <span data-ttu-id="e3eb4-546">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-546">Required</span></span> | <span data-ttu-id="e3eb4-547">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-547">Type</span></span> | <span data-ttu-id="e3eb4-548">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-549">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-549">originalValue</span></span> |<span data-ttu-id="e3eb4-550">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-550">Yes</span></span> |<span data-ttu-id="e3eb4-551">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-551">array or string</span></span> |<span data-ttu-id="e3eb4-552">Pole nebo řetězec, který má používat pro přeskočení.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-552">The array or string to use for skipping.</span></span> |
| <span data-ttu-id="e3eb4-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="e3eb4-553">numberToSkip</span></span> |<span data-ttu-id="e3eb4-554">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-554">Yes</span></span> |<span data-ttu-id="e3eb4-555">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-555">int</span></span> |<span data-ttu-id="e3eb4-556">Počet elementů nebo znaků, které chcete vynechat.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-556">The number of elements or characters to skip.</span></span> <span data-ttu-id="e3eb4-557">Pokud tato hodnota je 0 nebo menší, vrátí se všechny elementy nebo znaků v hodnotě.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-557">If this value is 0 or less, all the elements or characters in the value are returned.</span></span> <span data-ttu-id="e3eb4-558">Pokud je větší než délka pole nebo řetězec, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-558">If it is larger than the length of the array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-559">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-559">Return value</span></span>

<span data-ttu-id="e3eb4-560">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-561">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-561">Example</span></span>

<span data-ttu-id="e3eb4-562">Následující příklad přeskočí zadaný počet elementů v poli a zadaný počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-562">The following example skips the specified number of elements in the array, and the specified number of characters in a string.</span></span>

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

<span data-ttu-id="e3eb4-563">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-563">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-564">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-564">Name</span></span> | <span data-ttu-id="e3eb4-565">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-565">Type</span></span> | <span data-ttu-id="e3eb4-566">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-567">arrayOutput</span></span> | <span data-ttu-id="e3eb4-568">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-568">Array</span></span> | <span data-ttu-id="e3eb4-569">["tři"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-569">["three"]</span></span> |
| <span data-ttu-id="e3eb4-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-570">stringOutput</span></span> | <span data-ttu-id="e3eb4-571">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-571">String</span></span> | <span data-ttu-id="e3eb4-572">dvě tři</span><span class="sxs-lookup"><span data-stu-id="e3eb4-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="e3eb4-573">proveďte</span><span class="sxs-lookup"><span data-stu-id="e3eb4-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="e3eb4-574">Vrátí pole s zadaný počet elementů od začátku pole nebo řetězec s zadaný počet znaků od začátku řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-574">Returns an array with the specified number of elements from the start of the array, or a string with the specified number of characters from the start of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-575">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-575">Parameters</span></span>

| <span data-ttu-id="e3eb4-576">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-576">Parameter</span></span> | <span data-ttu-id="e3eb4-577">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-577">Required</span></span> | <span data-ttu-id="e3eb4-578">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-578">Type</span></span> | <span data-ttu-id="e3eb4-579">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-580">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-580">originalValue</span></span> |<span data-ttu-id="e3eb4-581">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-581">Yes</span></span> |<span data-ttu-id="e3eb4-582">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-582">array or string</span></span> |<span data-ttu-id="e3eb4-583">Pole nebo řetězec, který má trvat elementy ze.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-583">The array or string to take the elements from.</span></span> |
| <span data-ttu-id="e3eb4-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="e3eb4-584">numberToTake</span></span> |<span data-ttu-id="e3eb4-585">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-585">Yes</span></span> |<span data-ttu-id="e3eb4-586">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3eb4-586">int</span></span> |<span data-ttu-id="e3eb4-587">Počet elementů nebo znaků, který má trvat.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-587">The number of elements or characters to take.</span></span> <span data-ttu-id="e3eb4-588">Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="e3eb4-589">Pokud je větší než délka dané pole nebo řetězec, vrátí se všechny elementy ve pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-589">If it is larger than the length of the given array or string, all the elements in the array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-590">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-590">Return value</span></span>

<span data-ttu-id="e3eb4-591">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-592">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-592">Example</span></span>

<span data-ttu-id="e3eb4-593">V následujícím příkladu má zadaný počet prvků z pole a znaků z řetězce.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-593">The following example takes the specified number of elements from the array, and characters from a string.</span></span>

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

<span data-ttu-id="e3eb4-594">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-594">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-595">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-595">Name</span></span> | <span data-ttu-id="e3eb4-596">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-596">Type</span></span> | <span data-ttu-id="e3eb4-597">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-598">arrayOutput</span></span> | <span data-ttu-id="e3eb4-599">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-599">Array</span></span> | <span data-ttu-id="e3eb4-600">["1", "dva"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-600">["one", "two"]</span></span> |
| <span data-ttu-id="e3eb4-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-601">stringOutput</span></span> | <span data-ttu-id="e3eb4-602">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e3eb4-602">String</span></span> | <span data-ttu-id="e3eb4-603">na</span><span class="sxs-lookup"><span data-stu-id="e3eb4-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="e3eb4-604">sjednocení</span><span class="sxs-lookup"><span data-stu-id="e3eb4-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="e3eb4-605">Vrací jednu pole nebo objekt s všechny elementy z parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-605">Returns a single array or object with all elements from the parameters.</span></span> <span data-ttu-id="e3eb4-606">Duplicitní hodnoty nebo klíče jsou jenom jednou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="e3eb4-607">Parametry</span><span class="sxs-lookup"><span data-stu-id="e3eb4-607">Parameters</span></span>

| <span data-ttu-id="e3eb4-608">Parametr</span><span class="sxs-lookup"><span data-stu-id="e3eb4-608">Parameter</span></span> | <span data-ttu-id="e3eb4-609">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3eb4-609">Required</span></span> | <span data-ttu-id="e3eb4-610">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-610">Type</span></span> | <span data-ttu-id="e3eb4-611">Popis</span><span class="sxs-lookup"><span data-stu-id="e3eb4-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e3eb4-612">arg1</span><span class="sxs-lookup"><span data-stu-id="e3eb4-612">arg1</span></span> |<span data-ttu-id="e3eb4-613">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-613">Yes</span></span> |<span data-ttu-id="e3eb4-614">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-614">array or object</span></span> |<span data-ttu-id="e3eb4-615">První hodnota má použít pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-615">The first value to use for joining elements.</span></span> |
| <span data-ttu-id="e3eb4-616">arg2</span><span class="sxs-lookup"><span data-stu-id="e3eb4-616">arg2</span></span> |<span data-ttu-id="e3eb4-617">Ano</span><span class="sxs-lookup"><span data-stu-id="e3eb4-617">Yes</span></span> |<span data-ttu-id="e3eb4-618">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-618">array or object</span></span> |<span data-ttu-id="e3eb4-619">Druhá hodnota má použít pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-619">The second value to use for joining elements.</span></span> |
| <span data-ttu-id="e3eb4-620">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="e3eb4-620">additional arguments</span></span> |<span data-ttu-id="e3eb4-621">Ne</span><span class="sxs-lookup"><span data-stu-id="e3eb4-621">No</span></span> |<span data-ttu-id="e3eb4-622">objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-622">array or object</span></span> |<span data-ttu-id="e3eb4-623">Další hodnoty má použít pro připojení elementy.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-623">Additional values to use for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e3eb4-624">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-624">Return value</span></span>

<span data-ttu-id="e3eb4-625">Pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="e3eb4-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="e3eb4-626">Příklad</span><span class="sxs-lookup"><span data-stu-id="e3eb4-626">Example</span></span>

<span data-ttu-id="e3eb4-627">Následující příklad ukazuje, jak použít sjednocení s pole a objekty:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-627">The following example shows how to use union with arrays and objects:</span></span>

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

<span data-ttu-id="e3eb4-628">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="e3eb4-628">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="e3eb4-629">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e3eb4-629">Name</span></span> | <span data-ttu-id="e3eb4-630">Typ</span><span class="sxs-lookup"><span data-stu-id="e3eb4-630">Type</span></span> | <span data-ttu-id="e3eb4-631">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e3eb4-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e3eb4-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-632">objectOutput</span></span> | <span data-ttu-id="e3eb4-633">Objekt</span><span class="sxs-lookup"><span data-stu-id="e3eb4-633">Object</span></span> | <span data-ttu-id="e3eb4-634">{"1": "a", "dva": "b", "tři": "c", "4": "d", "5": "e"}</span><span class="sxs-lookup"><span data-stu-id="e3eb4-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="e3eb4-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="e3eb4-635">arrayOutput</span></span> | <span data-ttu-id="e3eb4-636">Pole</span><span class="sxs-lookup"><span data-stu-id="e3eb4-636">Array</span></span> | <span data-ttu-id="e3eb4-637">["1", "dva", "tři", "čtyři"]</span><span class="sxs-lookup"><span data-stu-id="e3eb4-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e3eb4-638">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3eb4-638">Next steps</span></span>
* <span data-ttu-id="e3eb4-639">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-639">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="e3eb4-640">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-640">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="e3eb4-641">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-641">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="e3eb4-642">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e3eb4-642">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

