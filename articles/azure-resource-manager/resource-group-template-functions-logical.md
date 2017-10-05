---
title: "Azure Resource Manager funkce šablon - logické | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manager k určení logické hodnoty."
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
ms.openlocfilehash: 313601ad99cdc12c4b50f5469959d37a9fa70d35
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="c6998-103">Logické funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c6998-103">Logical functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="c6998-104">Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.</span><span class="sxs-lookup"><span data-stu-id="c6998-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="c6998-105">a</span><span class="sxs-lookup"><span data-stu-id="c6998-105">and</span></span>](#and)
* [<span data-ttu-id="c6998-106">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-106">bool</span></span>](#bool)
* [<span data-ttu-id="c6998-107">Pokud</span><span class="sxs-lookup"><span data-stu-id="c6998-107">if</span></span>](#if)
* [<span data-ttu-id="c6998-108">není</span><span class="sxs-lookup"><span data-stu-id="c6998-108">not</span></span>](#not)
* [<span data-ttu-id="c6998-109">nebo</span><span class="sxs-lookup"><span data-stu-id="c6998-109">or</span></span>](#or)

## <a name="and"></a><span data-ttu-id="c6998-110">a</span><span class="sxs-lookup"><span data-stu-id="c6998-110">and</span></span>
`and(arg1, arg2)`

<span data-ttu-id="c6998-111">Kontroluje, zda jsou true obě hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="c6998-111">Checks whether both parameter values are true.</span></span>

### <a name="parameters"></a><span data-ttu-id="c6998-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="c6998-112">Parameters</span></span>

| <span data-ttu-id="c6998-113">Parametr</span><span class="sxs-lookup"><span data-stu-id="c6998-113">Parameter</span></span> | <span data-ttu-id="c6998-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c6998-114">Required</span></span> | <span data-ttu-id="c6998-115">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-115">Type</span></span> | <span data-ttu-id="c6998-116">Popis</span><span class="sxs-lookup"><span data-stu-id="c6998-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c6998-117">arg1</span><span class="sxs-lookup"><span data-stu-id="c6998-117">arg1</span></span> |<span data-ttu-id="c6998-118">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-118">Yes</span></span> |<span data-ttu-id="c6998-119">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-119">boolean</span></span> |<span data-ttu-id="c6998-120">První hodnota ke kontrole zda hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c6998-120">The first value to check whether is true.</span></span> |
| <span data-ttu-id="c6998-121">arg2</span><span class="sxs-lookup"><span data-stu-id="c6998-121">arg2</span></span> |<span data-ttu-id="c6998-122">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-122">Yes</span></span> |<span data-ttu-id="c6998-123">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-123">boolean</span></span> |<span data-ttu-id="c6998-124">Druhá hodnota, která má zkontrolujte, zda je true.</span><span class="sxs-lookup"><span data-stu-id="c6998-124">The second value to check whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="c6998-125">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-125">Return value</span></span>

<span data-ttu-id="c6998-126">Vrátí **True** Pokud jsou obě hodnoty true; v opačném **False**.</span><span class="sxs-lookup"><span data-stu-id="c6998-126">Returns **True** if both values are true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="c6998-127">Příklady</span><span class="sxs-lookup"><span data-stu-id="c6998-127">Examples</span></span>

<span data-ttu-id="c6998-128">Následující příklad ukazuje, jak použít logické funkce.</span><span class="sxs-lookup"><span data-stu-id="c6998-128">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="c6998-129">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="c6998-129">The output from the preceding example is:</span></span>

| <span data-ttu-id="c6998-130">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-130">Name</span></span> | <span data-ttu-id="c6998-131">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-131">Type</span></span> | <span data-ttu-id="c6998-132">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-132">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-133">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-133">andExampleOutput</span></span> | <span data-ttu-id="c6998-134">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-134">Bool</span></span> | <span data-ttu-id="c6998-135">False</span><span class="sxs-lookup"><span data-stu-id="c6998-135">False</span></span> |
| <span data-ttu-id="c6998-136">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-136">orExampleOutput</span></span> | <span data-ttu-id="c6998-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-137">Bool</span></span> | <span data-ttu-id="c6998-138">True</span><span class="sxs-lookup"><span data-stu-id="c6998-138">True</span></span> |
| <span data-ttu-id="c6998-139">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-139">notExampleOutput</span></span> | <span data-ttu-id="c6998-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-140">Bool</span></span> | <span data-ttu-id="c6998-141">False</span><span class="sxs-lookup"><span data-stu-id="c6998-141">False</span></span> |


## <a name="bool"></a><span data-ttu-id="c6998-142">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-142">bool</span></span>
`bool(arg1)`

<span data-ttu-id="c6998-143">Parametr převede na booleovskou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c6998-143">Converts the parameter to a boolean.</span></span>

### <a name="parameters"></a><span data-ttu-id="c6998-144">Parametry</span><span class="sxs-lookup"><span data-stu-id="c6998-144">Parameters</span></span>

| <span data-ttu-id="c6998-145">Parametr</span><span class="sxs-lookup"><span data-stu-id="c6998-145">Parameter</span></span> | <span data-ttu-id="c6998-146">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c6998-146">Required</span></span> | <span data-ttu-id="c6998-147">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-147">Type</span></span> | <span data-ttu-id="c6998-148">Popis</span><span class="sxs-lookup"><span data-stu-id="c6998-148">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c6998-149">arg1</span><span class="sxs-lookup"><span data-stu-id="c6998-149">arg1</span></span> |<span data-ttu-id="c6998-150">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-150">Yes</span></span> |<span data-ttu-id="c6998-151">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="c6998-151">string or int</span></span> |<span data-ttu-id="c6998-152">Hodnota převést na logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c6998-152">The value to convert to a boolean.</span></span> |

### <a name="return-value"></a><span data-ttu-id="c6998-153">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-153">Return value</span></span>
<span data-ttu-id="c6998-154">Logická hodnota převedená hodnota.</span><span class="sxs-lookup"><span data-stu-id="c6998-154">A boolean of the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="c6998-155">Příklady</span><span class="sxs-lookup"><span data-stu-id="c6998-155">Examples</span></span>

<span data-ttu-id="c6998-156">Následující příklad ukazuje, jak používat bool s řetězec nebo celé číslo.</span><span class="sxs-lookup"><span data-stu-id="c6998-156">The following example shows how to use bool with a string or integer.</span></span>

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

<span data-ttu-id="c6998-157">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="c6998-157">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="c6998-158">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-158">Name</span></span> | <span data-ttu-id="c6998-159">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-159">Type</span></span> | <span data-ttu-id="c6998-160">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-160">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-161">trueString</span><span class="sxs-lookup"><span data-stu-id="c6998-161">trueString</span></span> | <span data-ttu-id="c6998-162">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-162">Bool</span></span> | <span data-ttu-id="c6998-163">True</span><span class="sxs-lookup"><span data-stu-id="c6998-163">True</span></span> |
| <span data-ttu-id="c6998-164">falseString</span><span class="sxs-lookup"><span data-stu-id="c6998-164">falseString</span></span> | <span data-ttu-id="c6998-165">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-165">Bool</span></span> | <span data-ttu-id="c6998-166">False</span><span class="sxs-lookup"><span data-stu-id="c6998-166">False</span></span> |
| <span data-ttu-id="c6998-167">trueInt</span><span class="sxs-lookup"><span data-stu-id="c6998-167">trueInt</span></span> | <span data-ttu-id="c6998-168">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-168">Bool</span></span> | <span data-ttu-id="c6998-169">True</span><span class="sxs-lookup"><span data-stu-id="c6998-169">True</span></span> |
| <span data-ttu-id="c6998-170">falseInt</span><span class="sxs-lookup"><span data-stu-id="c6998-170">falseInt</span></span> | <span data-ttu-id="c6998-171">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-171">Bool</span></span> | <span data-ttu-id="c6998-172">False</span><span class="sxs-lookup"><span data-stu-id="c6998-172">False</span></span> |

## <a name="if"></a><span data-ttu-id="c6998-173">Pokud</span><span class="sxs-lookup"><span data-stu-id="c6998-173">if</span></span>
`if(condition, trueValue, falseValue)`

<span data-ttu-id="c6998-174">Vrátí hodnotu, podle toho, zda je podmínka true nebo false.</span><span class="sxs-lookup"><span data-stu-id="c6998-174">Returns a value based on whether a condition is true or false.</span></span>

### <a name="parameters"></a><span data-ttu-id="c6998-175">Parametry</span><span class="sxs-lookup"><span data-stu-id="c6998-175">Parameters</span></span>

| <span data-ttu-id="c6998-176">Parametr</span><span class="sxs-lookup"><span data-stu-id="c6998-176">Parameter</span></span> | <span data-ttu-id="c6998-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c6998-177">Required</span></span> | <span data-ttu-id="c6998-178">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-178">Type</span></span> | <span data-ttu-id="c6998-179">Popis</span><span class="sxs-lookup"><span data-stu-id="c6998-179">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c6998-180">Podmínka</span><span class="sxs-lookup"><span data-stu-id="c6998-180">condition</span></span> |<span data-ttu-id="c6998-181">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-181">Yes</span></span> |<span data-ttu-id="c6998-182">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-182">boolean</span></span> |<span data-ttu-id="c6998-183">Hodnota, zkontrolujte, zda se jedná o hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c6998-183">The value to check whether it is true.</span></span> |
| <span data-ttu-id="c6998-184">trueValue</span><span class="sxs-lookup"><span data-stu-id="c6998-184">trueValue</span></span> |<span data-ttu-id="c6998-185">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-185">Yes</span></span> | <span data-ttu-id="c6998-186">řetězec, int, objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="c6998-186">string, int, object, or array</span></span> |<span data-ttu-id="c6998-187">Hodnota vrácené v případě, že je podmínka vyhodnocena jako true.</span><span class="sxs-lookup"><span data-stu-id="c6998-187">The value to return when the condition is true.</span></span> |
| <span data-ttu-id="c6998-188">falseValue</span><span class="sxs-lookup"><span data-stu-id="c6998-188">falseValue</span></span> |<span data-ttu-id="c6998-189">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-189">Yes</span></span> | <span data-ttu-id="c6998-190">řetězec, int, objekt nebo pole</span><span class="sxs-lookup"><span data-stu-id="c6998-190">string, int, object, or array</span></span> |<span data-ttu-id="c6998-191">Hodnota vrácené v případě, že je podmínka vyhodnocena jako false.</span><span class="sxs-lookup"><span data-stu-id="c6998-191">The value to return when the condition is false.</span></span> |

### <a name="return-value"></a><span data-ttu-id="c6998-192">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-192">Return value</span></span>

<span data-ttu-id="c6998-193">Vrátí druhý parametr, pokud je první parametr **True**, jinak vrátí třetí parametr.</span><span class="sxs-lookup"><span data-stu-id="c6998-193">Returns second parameter when first parameter is **True**; otherwise, returns third parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="c6998-194">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c6998-194">Remarks</span></span>

<span data-ttu-id="c6998-195">Tato funkce slouží k podmíněně nastavení vlastnosti prostředku.</span><span class="sxs-lookup"><span data-stu-id="c6998-195">You can use this function to conditionally set a resource property.</span></span> <span data-ttu-id="c6998-196">V následujícím příkladu není kompletní šablonu, ale zobrazuje části relevantní pro podmíněně nastavení skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c6998-196">The following example is not a full template, but it shows the relevant portions for conditionally setting the availability set.</span></span>

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

### <a name="examples"></a><span data-ttu-id="c6998-197">Příklady</span><span class="sxs-lookup"><span data-stu-id="c6998-197">Examples</span></span>

<span data-ttu-id="c6998-198">Následující příklad ukazuje, jak používat `if` funkce.</span><span class="sxs-lookup"><span data-stu-id="c6998-198">The following example shows how to use the `if` function.</span></span>

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

<span data-ttu-id="c6998-199">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="c6998-199">The output from the preceding example is:</span></span>

| <span data-ttu-id="c6998-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-200">Name</span></span> | <span data-ttu-id="c6998-201">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-201">Type</span></span> | <span data-ttu-id="c6998-202">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-202">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-203">yesOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-203">yesOutput</span></span> | <span data-ttu-id="c6998-204">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c6998-204">String</span></span> | <span data-ttu-id="c6998-205">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-205">yes</span></span> |
| <span data-ttu-id="c6998-206">noOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-206">noOutput</span></span> | <span data-ttu-id="c6998-207">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c6998-207">String</span></span> | <span data-ttu-id="c6998-208">Ne</span><span class="sxs-lookup"><span data-stu-id="c6998-208">no</span></span> |


## <a name="not"></a><span data-ttu-id="c6998-209">není</span><span class="sxs-lookup"><span data-stu-id="c6998-209">not</span></span>
`not(arg1)`

<span data-ttu-id="c6998-210">Převede logickou hodnotu na opačnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c6998-210">Converts boolean value to its opposite value.</span></span>

### <a name="parameters"></a><span data-ttu-id="c6998-211">Parametry</span><span class="sxs-lookup"><span data-stu-id="c6998-211">Parameters</span></span>

| <span data-ttu-id="c6998-212">Parametr</span><span class="sxs-lookup"><span data-stu-id="c6998-212">Parameter</span></span> | <span data-ttu-id="c6998-213">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c6998-213">Required</span></span> | <span data-ttu-id="c6998-214">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-214">Type</span></span> | <span data-ttu-id="c6998-215">Popis</span><span class="sxs-lookup"><span data-stu-id="c6998-215">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c6998-216">arg1</span><span class="sxs-lookup"><span data-stu-id="c6998-216">arg1</span></span> |<span data-ttu-id="c6998-217">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-217">Yes</span></span> |<span data-ttu-id="c6998-218">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-218">boolean</span></span> |<span data-ttu-id="c6998-219">Hodnota k převedení.</span><span class="sxs-lookup"><span data-stu-id="c6998-219">The value to convert.</span></span> |


### <a name="return-value"></a><span data-ttu-id="c6998-220">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-220">Return value</span></span>

<span data-ttu-id="c6998-221">Vrátí **True** po parametr **False**.</span><span class="sxs-lookup"><span data-stu-id="c6998-221">Returns **True** when parameter is **False**.</span></span> <span data-ttu-id="c6998-222">Vrátí **False** po parametr **True**.</span><span class="sxs-lookup"><span data-stu-id="c6998-222">Returns **False** when parameter is **True**.</span></span>

### <a name="examples"></a><span data-ttu-id="c6998-223">Příklady</span><span class="sxs-lookup"><span data-stu-id="c6998-223">Examples</span></span>

<span data-ttu-id="c6998-224">Následující příklad ukazuje, jak použít logické funkce.</span><span class="sxs-lookup"><span data-stu-id="c6998-224">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="c6998-225">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="c6998-225">The output from the preceding example is:</span></span>

| <span data-ttu-id="c6998-226">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-226">Name</span></span> | <span data-ttu-id="c6998-227">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-227">Type</span></span> | <span data-ttu-id="c6998-228">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-228">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-229">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-229">andExampleOutput</span></span> | <span data-ttu-id="c6998-230">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-230">Bool</span></span> | <span data-ttu-id="c6998-231">False</span><span class="sxs-lookup"><span data-stu-id="c6998-231">False</span></span> |
| <span data-ttu-id="c6998-232">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-232">orExampleOutput</span></span> | <span data-ttu-id="c6998-233">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-233">Bool</span></span> | <span data-ttu-id="c6998-234">True</span><span class="sxs-lookup"><span data-stu-id="c6998-234">True</span></span> |
| <span data-ttu-id="c6998-235">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-235">notExampleOutput</span></span> | <span data-ttu-id="c6998-236">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-236">Bool</span></span> | <span data-ttu-id="c6998-237">False</span><span class="sxs-lookup"><span data-stu-id="c6998-237">False</span></span> |

<span data-ttu-id="c6998-238">Následující příklad používá **není** s [rovná](resource-group-template-functions-comparison.md#equals).</span><span class="sxs-lookup"><span data-stu-id="c6998-238">The following example uses **not** with [equals](resource-group-template-functions-comparison.md#equals).</span></span>

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

<span data-ttu-id="c6998-239">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="c6998-239">The output from the preceding example is:</span></span>

| <span data-ttu-id="c6998-240">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-240">Name</span></span> | <span data-ttu-id="c6998-241">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-241">Type</span></span> | <span data-ttu-id="c6998-242">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-242">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-243">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="c6998-243">checkNotEquals</span></span> | <span data-ttu-id="c6998-244">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-244">Bool</span></span> | <span data-ttu-id="c6998-245">True</span><span class="sxs-lookup"><span data-stu-id="c6998-245">True</span></span> |


## <a name="or"></a><span data-ttu-id="c6998-246">nebo</span><span class="sxs-lookup"><span data-stu-id="c6998-246">or</span></span>
`or(arg1, arg2)`

<span data-ttu-id="c6998-247">Zkontroluje, zda je buď parametr hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c6998-247">Checks whether either parameter value is true.</span></span>

### <a name="parameters"></a><span data-ttu-id="c6998-248">Parametry</span><span class="sxs-lookup"><span data-stu-id="c6998-248">Parameters</span></span>

| <span data-ttu-id="c6998-249">Parametr</span><span class="sxs-lookup"><span data-stu-id="c6998-249">Parameter</span></span> | <span data-ttu-id="c6998-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c6998-250">Required</span></span> | <span data-ttu-id="c6998-251">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-251">Type</span></span> | <span data-ttu-id="c6998-252">Popis</span><span class="sxs-lookup"><span data-stu-id="c6998-252">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c6998-253">arg1</span><span class="sxs-lookup"><span data-stu-id="c6998-253">arg1</span></span> |<span data-ttu-id="c6998-254">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-254">Yes</span></span> |<span data-ttu-id="c6998-255">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-255">boolean</span></span> |<span data-ttu-id="c6998-256">První hodnota ke kontrole zda hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c6998-256">The first value to check whether is true.</span></span> |
| <span data-ttu-id="c6998-257">arg2</span><span class="sxs-lookup"><span data-stu-id="c6998-257">arg2</span></span> |<span data-ttu-id="c6998-258">Ano</span><span class="sxs-lookup"><span data-stu-id="c6998-258">Yes</span></span> |<span data-ttu-id="c6998-259">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-259">boolean</span></span> |<span data-ttu-id="c6998-260">Druhá hodnota, která má zkontrolujte, zda je true.</span><span class="sxs-lookup"><span data-stu-id="c6998-260">The second value to check whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="c6998-261">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-261">Return value</span></span>

<span data-ttu-id="c6998-262">Vrátí **True** Pokud buď hodnota je true; v opačném **False**.</span><span class="sxs-lookup"><span data-stu-id="c6998-262">Returns **True** if either value is true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="c6998-263">Příklady</span><span class="sxs-lookup"><span data-stu-id="c6998-263">Examples</span></span>

<span data-ttu-id="c6998-264">Následující příklad ukazuje, jak použít logické funkce.</span><span class="sxs-lookup"><span data-stu-id="c6998-264">The following example shows how to use logical functions.</span></span>

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

<span data-ttu-id="c6998-265">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="c6998-265">The output from the preceding example is:</span></span>

| <span data-ttu-id="c6998-266">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c6998-266">Name</span></span> | <span data-ttu-id="c6998-267">Typ</span><span class="sxs-lookup"><span data-stu-id="c6998-267">Type</span></span> | <span data-ttu-id="c6998-268">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c6998-268">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="c6998-269">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-269">andExampleOutput</span></span> | <span data-ttu-id="c6998-270">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-270">Bool</span></span> | <span data-ttu-id="c6998-271">False</span><span class="sxs-lookup"><span data-stu-id="c6998-271">False</span></span> |
| <span data-ttu-id="c6998-272">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-272">orExampleOutput</span></span> | <span data-ttu-id="c6998-273">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-273">Bool</span></span> | <span data-ttu-id="c6998-274">True</span><span class="sxs-lookup"><span data-stu-id="c6998-274">True</span></span> |
| <span data-ttu-id="c6998-275">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="c6998-275">notExampleOutput</span></span> | <span data-ttu-id="c6998-276">BOOL</span><span class="sxs-lookup"><span data-stu-id="c6998-276">Bool</span></span> | <span data-ttu-id="c6998-277">False</span><span class="sxs-lookup"><span data-stu-id="c6998-277">False</span></span> |


## <a name="next-steps"></a><span data-ttu-id="c6998-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6998-278">Next steps</span></span>
* <span data-ttu-id="c6998-279">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c6998-279">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="c6998-280">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c6998-280">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="c6998-281">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="c6998-281">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="c6998-282">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="c6998-282">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

