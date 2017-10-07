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
# <a name="logical-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="a49cf-103">Logické funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a49cf-103">Logical functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="a49cf-104">Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.</span><span class="sxs-lookup"><span data-stu-id="a49cf-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="a49cf-105">a</span><span class="sxs-lookup"><span data-stu-id="a49cf-105">and</span></span>](#and)
* [<span data-ttu-id="a49cf-106">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-106">bool</span></span>](#bool)
* [<span data-ttu-id="a49cf-107">Pokud</span><span class="sxs-lookup"><span data-stu-id="a49cf-107">if</span></span>](#if)
* [<span data-ttu-id="a49cf-108">není</span><span class="sxs-lookup"><span data-stu-id="a49cf-108">not</span></span>](#not)
* [<span data-ttu-id="a49cf-109">nebo</span><span class="sxs-lookup"><span data-stu-id="a49cf-109">or</span></span>](#or)

## <a name="and"></a><span data-ttu-id="a49cf-110">a</span><span class="sxs-lookup"><span data-stu-id="a49cf-110">and</span></span>
`and(arg1, arg2)`

<span data-ttu-id="a49cf-111">Kontroluje, zda jsou true obě hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="a49cf-111">Checks whether both parameter values are true.</span></span>

### <a name="parameters"></a><span data-ttu-id="a49cf-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="a49cf-112">Parameters</span></span>

| <span data-ttu-id="a49cf-113">Parametr</span><span class="sxs-lookup"><span data-stu-id="a49cf-113">Parameter</span></span> | <span data-ttu-id="a49cf-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a49cf-114">Required</span></span> | <span data-ttu-id="a49cf-115">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-115">Type</span></span> | <span data-ttu-id="a49cf-116">Popis</span><span class="sxs-lookup"><span data-stu-id="a49cf-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a49cf-117">arg1</span><span class="sxs-lookup"><span data-stu-id="a49cf-117">arg1</span></span> |<span data-ttu-id="a49cf-118">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-118">Yes</span></span> |<span data-ttu-id="a49cf-119">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-119">boolean</span></span> |<span data-ttu-id="a49cf-120">tom, zda text Hello toocheck první hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-120">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="a49cf-121">arg2</span><span class="sxs-lookup"><span data-stu-id="a49cf-121">arg2</span></span> |<span data-ttu-id="a49cf-122">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-122">Yes</span></span> |<span data-ttu-id="a49cf-123">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-123">boolean</span></span> |<span data-ttu-id="a49cf-124">tom, zda text Hello druhý toocheck hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-124">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a49cf-125">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-125">Return value</span></span>

<span data-ttu-id="a49cf-126">Vrátí **True** Pokud jsou obě hodnoty true; v opačném **False**.</span><span class="sxs-lookup"><span data-stu-id="a49cf-126">Returns **True** if both values are true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a49cf-127">Příklady</span><span class="sxs-lookup"><span data-stu-id="a49cf-127">Examples</span></span>

<span data-ttu-id="a49cf-128">Následující příklad ukazuje, jak Hello toouse logické funkce.</span><span class="sxs-lookup"><span data-stu-id="a49cf-128">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="a49cf-129">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-129">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="a49cf-130">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-130">Name</span></span> | <span data-ttu-id="a49cf-131">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-131">Type</span></span> | <span data-ttu-id="a49cf-132">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-132">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-133">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-133">andExampleOutput</span></span> | <span data-ttu-id="a49cf-134">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-134">Bool</span></span> | <span data-ttu-id="a49cf-135">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-135">False</span></span> |
| <span data-ttu-id="a49cf-136">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-136">orExampleOutput</span></span> | <span data-ttu-id="a49cf-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-137">Bool</span></span> | <span data-ttu-id="a49cf-138">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-138">True</span></span> |
| <span data-ttu-id="a49cf-139">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-139">notExampleOutput</span></span> | <span data-ttu-id="a49cf-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-140">Bool</span></span> | <span data-ttu-id="a49cf-141">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-141">False</span></span> |


## <a name="bool"></a><span data-ttu-id="a49cf-142">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-142">bool</span></span>
`bool(arg1)`

<span data-ttu-id="a49cf-143">Převede hello parametr tooa logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="a49cf-143">Converts hello parameter tooa boolean.</span></span>

### <a name="parameters"></a><span data-ttu-id="a49cf-144">Parametry</span><span class="sxs-lookup"><span data-stu-id="a49cf-144">Parameters</span></span>

| <span data-ttu-id="a49cf-145">Parametr</span><span class="sxs-lookup"><span data-stu-id="a49cf-145">Parameter</span></span> | <span data-ttu-id="a49cf-146">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a49cf-146">Required</span></span> | <span data-ttu-id="a49cf-147">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-147">Type</span></span> | <span data-ttu-id="a49cf-148">Popis</span><span class="sxs-lookup"><span data-stu-id="a49cf-148">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a49cf-149">arg1</span><span class="sxs-lookup"><span data-stu-id="a49cf-149">arg1</span></span> |<span data-ttu-id="a49cf-150">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-150">Yes</span></span> |<span data-ttu-id="a49cf-151">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="a49cf-151">string or int</span></span> |<span data-ttu-id="a49cf-152">Hello tooa tooconvert hodnota logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="a49cf-152">hello value tooconvert tooa boolean.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a49cf-153">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-153">Return value</span></span>
<span data-ttu-id="a49cf-154">Logická hodnota Dobrý den převést hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a49cf-154">A boolean of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="a49cf-155">Příklady</span><span class="sxs-lookup"><span data-stu-id="a49cf-155">Examples</span></span>

<span data-ttu-id="a49cf-156">Následující příklad ukazuje, jak Hello bool toouse řetězec nebo celé číslo.</span><span class="sxs-lookup"><span data-stu-id="a49cf-156">hello following example shows how toouse bool with a string or integer.</span></span>

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

<span data-ttu-id="a49cf-157">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-157">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a49cf-158">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-158">Name</span></span> | <span data-ttu-id="a49cf-159">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-159">Type</span></span> | <span data-ttu-id="a49cf-160">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-160">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-161">trueString</span><span class="sxs-lookup"><span data-stu-id="a49cf-161">trueString</span></span> | <span data-ttu-id="a49cf-162">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-162">Bool</span></span> | <span data-ttu-id="a49cf-163">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-163">True</span></span> |
| <span data-ttu-id="a49cf-164">falseString</span><span class="sxs-lookup"><span data-stu-id="a49cf-164">falseString</span></span> | <span data-ttu-id="a49cf-165">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-165">Bool</span></span> | <span data-ttu-id="a49cf-166">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-166">False</span></span> |
| <span data-ttu-id="a49cf-167">trueInt</span><span class="sxs-lookup"><span data-stu-id="a49cf-167">trueInt</span></span> | <span data-ttu-id="a49cf-168">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-168">Bool</span></span> | <span data-ttu-id="a49cf-169">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-169">True</span></span> |
| <span data-ttu-id="a49cf-170">falseInt</span><span class="sxs-lookup"><span data-stu-id="a49cf-170">falseInt</span></span> | <span data-ttu-id="a49cf-171">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-171">Bool</span></span> | <span data-ttu-id="a49cf-172">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-172">False</span></span> |

## <a name="if"></a><span data-ttu-id="a49cf-173">Pokud</span><span class="sxs-lookup"><span data-stu-id="a49cf-173">if</span></span>
`if(condition, trueValue, falseValue)`

<span data-ttu-id="a49cf-174">Vrátí hodnotu, podle toho, zda je podmínka true nebo false.</span><span class="sxs-lookup"><span data-stu-id="a49cf-174">Returns a value based on whether a condition is true or false.</span></span>

### <a name="parameters"></a><span data-ttu-id="a49cf-175">Parametry</span><span class="sxs-lookup"><span data-stu-id="a49cf-175">Parameters</span></span>

| <span data-ttu-id="a49cf-176">Parametr</span><span class="sxs-lookup"><span data-stu-id="a49cf-176">Parameter</span></span> | <span data-ttu-id="a49cf-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a49cf-177">Required</span></span> | <span data-ttu-id="a49cf-178">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-178">Type</span></span> | <span data-ttu-id="a49cf-179">Popis</span><span class="sxs-lookup"><span data-stu-id="a49cf-179">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a49cf-180">Podmínka</span><span class="sxs-lookup"><span data-stu-id="a49cf-180">condition</span></span> |<span data-ttu-id="a49cf-181">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-181">Yes</span></span> |<span data-ttu-id="a49cf-182">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-182">boolean</span></span> |<span data-ttu-id="a49cf-183">Hodnota toocheck Hello, jestli je true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-183">hello value toocheck whether it is true.</span></span> |
| <span data-ttu-id="a49cf-184">trueValue</span><span class="sxs-lookup"><span data-stu-id="a49cf-184">trueValue</span></span> |<span data-ttu-id="a49cf-185">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-185">Yes</span></span> | <span data-ttu-id="a49cf-186">řetězec, int, objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="a49cf-186">string, int, object, or array</span></span> |<span data-ttu-id="a49cf-187">Hodnota tooreturn Hello, pokud je splněna podmínka hello.</span><span class="sxs-lookup"><span data-stu-id="a49cf-187">hello value tooreturn when hello condition is true.</span></span> |
| <span data-ttu-id="a49cf-188">falseValue</span><span class="sxs-lookup"><span data-stu-id="a49cf-188">falseValue</span></span> |<span data-ttu-id="a49cf-189">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-189">Yes</span></span> | <span data-ttu-id="a49cf-190">řetězec, int, objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="a49cf-190">string, int, object, or array</span></span> |<span data-ttu-id="a49cf-191">Hello hodnota tooreturn při hello podmínka vyhodnocena jako false.</span><span class="sxs-lookup"><span data-stu-id="a49cf-191">hello value tooreturn when hello condition is false.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a49cf-192">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-192">Return value</span></span>

<span data-ttu-id="a49cf-193">Vrátí druhý parametr, pokud je první parametr **True**, jinak vrátí třetí parametr.</span><span class="sxs-lookup"><span data-stu-id="a49cf-193">Returns second parameter when first parameter is **True**; otherwise, returns third parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="a49cf-194">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a49cf-194">Remarks</span></span>

<span data-ttu-id="a49cf-195">Tato sada tooconditionally funkce můžete použít vlastnost prostředku.</span><span class="sxs-lookup"><span data-stu-id="a49cf-195">You can use this function tooconditionally set a resource property.</span></span> <span data-ttu-id="a49cf-196">Hello následující příklad není kompletní šablonu, ale zobrazuje hello příslušné části pro skupinu dostupnosti hello podmíněně nastavení.</span><span class="sxs-lookup"><span data-stu-id="a49cf-196">hello following example is not a full template, but it shows hello relevant portions for conditionally setting hello availability set.</span></span>

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

### <a name="examples"></a><span data-ttu-id="a49cf-197">Příklady</span><span class="sxs-lookup"><span data-stu-id="a49cf-197">Examples</span></span>

<span data-ttu-id="a49cf-198">Následující příklad ukazuje, jak Hello toouse hello `if` funkce.</span><span class="sxs-lookup"><span data-stu-id="a49cf-198">hello following example shows how toouse hello `if` function.</span></span>

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

<span data-ttu-id="a49cf-199">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-199">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="a49cf-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-200">Name</span></span> | <span data-ttu-id="a49cf-201">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-201">Type</span></span> | <span data-ttu-id="a49cf-202">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-202">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-203">yesOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-203">yesOutput</span></span> | <span data-ttu-id="a49cf-204">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a49cf-204">String</span></span> | <span data-ttu-id="a49cf-205">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-205">yes</span></span> |
| <span data-ttu-id="a49cf-206">noOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-206">noOutput</span></span> | <span data-ttu-id="a49cf-207">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a49cf-207">String</span></span> | <span data-ttu-id="a49cf-208">Ne</span><span class="sxs-lookup"><span data-stu-id="a49cf-208">no</span></span> |


## <a name="not"></a><span data-ttu-id="a49cf-209">není</span><span class="sxs-lookup"><span data-stu-id="a49cf-209">not</span></span>
`not(arg1)`

<span data-ttu-id="a49cf-210">Převede logickou hodnotu tooits opačným hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a49cf-210">Converts boolean value tooits opposite value.</span></span>

### <a name="parameters"></a><span data-ttu-id="a49cf-211">Parametry</span><span class="sxs-lookup"><span data-stu-id="a49cf-211">Parameters</span></span>

| <span data-ttu-id="a49cf-212">Parametr</span><span class="sxs-lookup"><span data-stu-id="a49cf-212">Parameter</span></span> | <span data-ttu-id="a49cf-213">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a49cf-213">Required</span></span> | <span data-ttu-id="a49cf-214">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-214">Type</span></span> | <span data-ttu-id="a49cf-215">Popis</span><span class="sxs-lookup"><span data-stu-id="a49cf-215">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a49cf-216">arg1</span><span class="sxs-lookup"><span data-stu-id="a49cf-216">arg1</span></span> |<span data-ttu-id="a49cf-217">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-217">Yes</span></span> |<span data-ttu-id="a49cf-218">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-218">boolean</span></span> |<span data-ttu-id="a49cf-219">Hodnota tooconvert Hello.</span><span class="sxs-lookup"><span data-stu-id="a49cf-219">hello value tooconvert.</span></span> |


### <a name="return-value"></a><span data-ttu-id="a49cf-220">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-220">Return value</span></span>

<span data-ttu-id="a49cf-221">Vrátí **True** po parametr **False**.</span><span class="sxs-lookup"><span data-stu-id="a49cf-221">Returns **True** when parameter is **False**.</span></span> <span data-ttu-id="a49cf-222">Vrátí **False** po parametr **True**.</span><span class="sxs-lookup"><span data-stu-id="a49cf-222">Returns **False** when parameter is **True**.</span></span>

### <a name="examples"></a><span data-ttu-id="a49cf-223">Příklady</span><span class="sxs-lookup"><span data-stu-id="a49cf-223">Examples</span></span>

<span data-ttu-id="a49cf-224">Následující příklad ukazuje, jak Hello toouse logické funkce.</span><span class="sxs-lookup"><span data-stu-id="a49cf-224">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="a49cf-225">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-225">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="a49cf-226">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-226">Name</span></span> | <span data-ttu-id="a49cf-227">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-227">Type</span></span> | <span data-ttu-id="a49cf-228">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-228">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-229">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-229">andExampleOutput</span></span> | <span data-ttu-id="a49cf-230">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-230">Bool</span></span> | <span data-ttu-id="a49cf-231">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-231">False</span></span> |
| <span data-ttu-id="a49cf-232">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-232">orExampleOutput</span></span> | <span data-ttu-id="a49cf-233">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-233">Bool</span></span> | <span data-ttu-id="a49cf-234">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-234">True</span></span> |
| <span data-ttu-id="a49cf-235">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-235">notExampleOutput</span></span> | <span data-ttu-id="a49cf-236">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-236">Bool</span></span> | <span data-ttu-id="a49cf-237">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-237">False</span></span> |

<span data-ttu-id="a49cf-238">Hello následující příklad používá **není** s [rovná](resource-group-template-functions-comparison.md#equals).</span><span class="sxs-lookup"><span data-stu-id="a49cf-238">hello following example uses **not** with [equals](resource-group-template-functions-comparison.md#equals).</span></span>

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

<span data-ttu-id="a49cf-239">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-239">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="a49cf-240">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-240">Name</span></span> | <span data-ttu-id="a49cf-241">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-241">Type</span></span> | <span data-ttu-id="a49cf-242">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-242">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-243">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="a49cf-243">checkNotEquals</span></span> | <span data-ttu-id="a49cf-244">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-244">Bool</span></span> | <span data-ttu-id="a49cf-245">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-245">True</span></span> |


## <a name="or"></a><span data-ttu-id="a49cf-246">nebo</span><span class="sxs-lookup"><span data-stu-id="a49cf-246">or</span></span>
`or(arg1, arg2)`

<span data-ttu-id="a49cf-247">Zkontroluje, zda je buď parametr hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-247">Checks whether either parameter value is true.</span></span>

### <a name="parameters"></a><span data-ttu-id="a49cf-248">Parametry</span><span class="sxs-lookup"><span data-stu-id="a49cf-248">Parameters</span></span>

| <span data-ttu-id="a49cf-249">Parametr</span><span class="sxs-lookup"><span data-stu-id="a49cf-249">Parameter</span></span> | <span data-ttu-id="a49cf-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a49cf-250">Required</span></span> | <span data-ttu-id="a49cf-251">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-251">Type</span></span> | <span data-ttu-id="a49cf-252">Popis</span><span class="sxs-lookup"><span data-stu-id="a49cf-252">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a49cf-253">arg1</span><span class="sxs-lookup"><span data-stu-id="a49cf-253">arg1</span></span> |<span data-ttu-id="a49cf-254">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-254">Yes</span></span> |<span data-ttu-id="a49cf-255">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-255">boolean</span></span> |<span data-ttu-id="a49cf-256">tom, zda text Hello toocheck první hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-256">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="a49cf-257">arg2</span><span class="sxs-lookup"><span data-stu-id="a49cf-257">arg2</span></span> |<span data-ttu-id="a49cf-258">Ano</span><span class="sxs-lookup"><span data-stu-id="a49cf-258">Yes</span></span> |<span data-ttu-id="a49cf-259">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-259">boolean</span></span> |<span data-ttu-id="a49cf-260">tom, zda text Hello druhý toocheck hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="a49cf-260">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a49cf-261">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-261">Return value</span></span>

<span data-ttu-id="a49cf-262">Vrátí **True** Pokud buď hodnota je true; v opačném **False**.</span><span class="sxs-lookup"><span data-stu-id="a49cf-262">Returns **True** if either value is true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a49cf-263">Příklady</span><span class="sxs-lookup"><span data-stu-id="a49cf-263">Examples</span></span>

<span data-ttu-id="a49cf-264">Následující příklad ukazuje, jak Hello toouse logické funkce.</span><span class="sxs-lookup"><span data-stu-id="a49cf-264">hello following example shows how toouse logical functions.</span></span>

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

<span data-ttu-id="a49cf-265">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="a49cf-265">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="a49cf-266">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a49cf-266">Name</span></span> | <span data-ttu-id="a49cf-267">Typ</span><span class="sxs-lookup"><span data-stu-id="a49cf-267">Type</span></span> | <span data-ttu-id="a49cf-268">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a49cf-268">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a49cf-269">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-269">andExampleOutput</span></span> | <span data-ttu-id="a49cf-270">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-270">Bool</span></span> | <span data-ttu-id="a49cf-271">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-271">False</span></span> |
| <span data-ttu-id="a49cf-272">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-272">orExampleOutput</span></span> | <span data-ttu-id="a49cf-273">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-273">Bool</span></span> | <span data-ttu-id="a49cf-274">True</span><span class="sxs-lookup"><span data-stu-id="a49cf-274">True</span></span> |
| <span data-ttu-id="a49cf-275">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="a49cf-275">notExampleOutput</span></span> | <span data-ttu-id="a49cf-276">BOOL</span><span class="sxs-lookup"><span data-stu-id="a49cf-276">Bool</span></span> | <span data-ttu-id="a49cf-277">False</span><span class="sxs-lookup"><span data-stu-id="a49cf-277">False</span></span> |


## <a name="next-steps"></a><span data-ttu-id="a49cf-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a49cf-278">Next steps</span></span>
* <span data-ttu-id="a49cf-279">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a49cf-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a49cf-280">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a49cf-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="a49cf-281">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a49cf-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="a49cf-282">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="a49cf-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

