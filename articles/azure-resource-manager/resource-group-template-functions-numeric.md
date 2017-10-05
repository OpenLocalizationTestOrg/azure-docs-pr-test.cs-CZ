---
title: "Azure Resource Manager funkce šablon - číselné | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manageru pro práci s čísla."
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
ms.openlocfilehash: ae0261134b8d4a934048f58d6c679a48a904950b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="79293-103">Numerické funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="79293-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="79293-104">Resource Manager poskytuje následující funkce pro práci s celými čísly:</span><span class="sxs-lookup"><span data-stu-id="79293-104">Resource Manager provides the following functions for working with integers:</span></span>

* [<span data-ttu-id="79293-105">Přidat</span><span class="sxs-lookup"><span data-stu-id="79293-105">add</span></span>](#add)
* [<span data-ttu-id="79293-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="79293-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="79293-107">div</span><span class="sxs-lookup"><span data-stu-id="79293-107">div</span></span>](#div)
* [<span data-ttu-id="79293-108">plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="79293-108">float</span></span>](#float)
* [<span data-ttu-id="79293-109">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-109">int</span></span>](#int)
* [<span data-ttu-id="79293-110">min.</span><span class="sxs-lookup"><span data-stu-id="79293-110">min</span></span>](#min)
* [<span data-ttu-id="79293-111">maximální počet</span><span class="sxs-lookup"><span data-stu-id="79293-111">max</span></span>](#max)
* [<span data-ttu-id="79293-112">MOD</span><span class="sxs-lookup"><span data-stu-id="79293-112">mod</span></span>](#mod)
* [<span data-ttu-id="79293-113">mul</span><span class="sxs-lookup"><span data-stu-id="79293-113">mul</span></span>](#mul)
* [<span data-ttu-id="79293-114">Sub –</span><span class="sxs-lookup"><span data-stu-id="79293-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="79293-115">Přidat</span><span class="sxs-lookup"><span data-stu-id="79293-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="79293-116">Vrátí součet dvou zadaný celých čísel.</span><span class="sxs-lookup"><span data-stu-id="79293-116">Returns the sum of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-117">Parameters</span></span>

| <span data-ttu-id="79293-118">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-118">Parameter</span></span> | <span data-ttu-id="79293-119">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-119">Required</span></span> | <span data-ttu-id="79293-120">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-120">Type</span></span> | <span data-ttu-id="79293-121">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="79293-122">operand1</span><span class="sxs-lookup"><span data-stu-id="79293-122">operand1</span></span> |<span data-ttu-id="79293-123">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-123">Yes</span></span> |<span data-ttu-id="79293-124">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-124">int</span></span> |<span data-ttu-id="79293-125">První číslo přidat.</span><span class="sxs-lookup"><span data-stu-id="79293-125">First number to add.</span></span> |
|<span data-ttu-id="79293-126">operand2</span><span class="sxs-lookup"><span data-stu-id="79293-126">operand2</span></span> |<span data-ttu-id="79293-127">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-127">Yes</span></span> |<span data-ttu-id="79293-128">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-128">int</span></span> |<span data-ttu-id="79293-129">Druhé číslo, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="79293-129">Second number to add.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-130">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-130">Return value</span></span>

<span data-ttu-id="79293-131">Celé číslo, které obsahuje součet hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="79293-131">An integer that contains the sum of the parameters.</span></span>

### <a name="example"></a><span data-ttu-id="79293-132">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-132">Example</span></span>

<span data-ttu-id="79293-133">Následující příklad přidá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="79293-133">The following example adds two parameters.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
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

<span data-ttu-id="79293-134">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-134">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-135">Name</span></span> | <span data-ttu-id="79293-136">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-136">Type</span></span> | <span data-ttu-id="79293-137">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-138">addResult</span><span class="sxs-lookup"><span data-stu-id="79293-138">addResult</span></span> | <span data-ttu-id="79293-139">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-139">Int</span></span> | <span data-ttu-id="79293-140">8</span><span class="sxs-lookup"><span data-stu-id="79293-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="79293-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="79293-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="79293-142">Vrátí index smyčky iterací.</span><span class="sxs-lookup"><span data-stu-id="79293-142">Returns the index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="79293-143">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-143">Parameters</span></span>

| <span data-ttu-id="79293-144">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-144">Parameter</span></span> | <span data-ttu-id="79293-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-145">Required</span></span> | <span data-ttu-id="79293-146">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-146">Type</span></span> | <span data-ttu-id="79293-147">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-148">loopName</span><span class="sxs-lookup"><span data-stu-id="79293-148">loopName</span></span> | <span data-ttu-id="79293-149">Ne</span><span class="sxs-lookup"><span data-stu-id="79293-149">No</span></span> | <span data-ttu-id="79293-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="79293-150">string</span></span> | <span data-ttu-id="79293-151">Název smyčky pro získávání iterace.</span><span class="sxs-lookup"><span data-stu-id="79293-151">The name of the loop for getting the iteration.</span></span> |
| <span data-ttu-id="79293-152">Posun</span><span class="sxs-lookup"><span data-stu-id="79293-152">offset</span></span> |<span data-ttu-id="79293-153">Ne</span><span class="sxs-lookup"><span data-stu-id="79293-153">No</span></span> |<span data-ttu-id="79293-154">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-154">int</span></span> |<span data-ttu-id="79293-155">Číslo, který se má přidat na nule iterace hodnotu.</span><span class="sxs-lookup"><span data-stu-id="79293-155">The number to add to the zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="79293-156">Poznámky</span><span class="sxs-lookup"><span data-stu-id="79293-156">Remarks</span></span>

<span data-ttu-id="79293-157">Tato funkce se vždy používá s **kopie** objektu.</span><span class="sxs-lookup"><span data-stu-id="79293-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="79293-158">Pokud není zadána žádná hodnota pro **posun**, je vrácena hodnota aktuální iteraci.</span><span class="sxs-lookup"><span data-stu-id="79293-158">If no value is provided for **offset**, the current iteration value is returned.</span></span> <span data-ttu-id="79293-159">Hodnota iterace začíná od nuly.</span><span class="sxs-lookup"><span data-stu-id="79293-159">The iteration value starts at zero.</span></span>

<span data-ttu-id="79293-160">**LoopName** vlastnost umožňuje určit, zda copyIndex odkazuje na prostředek iterace nebo vlastnost iterace.</span><span class="sxs-lookup"><span data-stu-id="79293-160">The **loopName** property enables you to specify whether copyIndex is referring to a resource iteration or property iteration.</span></span> <span data-ttu-id="79293-161">Pokud není zadána žádná hodnota pro **loopName**, se používá na aktuální iteraci typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="79293-161">If no value is provided for **loopName**, the current resource type iteration is used.</span></span> <span data-ttu-id="79293-162">Zadejte hodnotu pro **loopName** během iterace u vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="79293-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="79293-163">Úplný popis jak používat **copyIndex**, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="79293-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="79293-164">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-164">Example</span></span>

<span data-ttu-id="79293-165">Následující příklad ukazuje kopírovací smyčkou a hodnotu indexu, který je součástí názvu.</span><span class="sxs-lookup"><span data-stu-id="79293-165">The following example shows a copy loop and the index value included in the name.</span></span> 

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

### <a name="return-value"></a><span data-ttu-id="79293-166">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-166">Return value</span></span>

<span data-ttu-id="79293-167">Celé číslo představující aktuální index iterace.</span><span class="sxs-lookup"><span data-stu-id="79293-167">An integer representing the current index of the iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="79293-168">div</span><span class="sxs-lookup"><span data-stu-id="79293-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="79293-169">Vrátí celočíselné dělení dvou zadaný celých čísel.</span><span class="sxs-lookup"><span data-stu-id="79293-169">Returns the integer division of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-170">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-170">Parameters</span></span>

| <span data-ttu-id="79293-171">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-171">Parameter</span></span> | <span data-ttu-id="79293-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-172">Required</span></span> | <span data-ttu-id="79293-173">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-173">Type</span></span> | <span data-ttu-id="79293-174">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-175">operand1</span><span class="sxs-lookup"><span data-stu-id="79293-175">operand1</span></span> |<span data-ttu-id="79293-176">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-176">Yes</span></span> |<span data-ttu-id="79293-177">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-177">int</span></span> |<span data-ttu-id="79293-178">Číslo je rozdělen.</span><span class="sxs-lookup"><span data-stu-id="79293-178">The number being divided.</span></span> |
| <span data-ttu-id="79293-179">operand2</span><span class="sxs-lookup"><span data-stu-id="79293-179">operand2</span></span> |<span data-ttu-id="79293-180">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-180">Yes</span></span> |<span data-ttu-id="79293-181">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-181">int</span></span> |<span data-ttu-id="79293-182">Číslo, které slouží k rozdělení.</span><span class="sxs-lookup"><span data-stu-id="79293-182">The number that is used to divide.</span></span> <span data-ttu-id="79293-183">Nemůže být 0.</span><span class="sxs-lookup"><span data-stu-id="79293-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-184">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-184">Return value</span></span>

<span data-ttu-id="79293-185">Celé číslo představující rozdělení.</span><span class="sxs-lookup"><span data-stu-id="79293-185">An integer representing the division.</span></span>

### <a name="example"></a><span data-ttu-id="79293-186">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-186">Example</span></span>

<span data-ttu-id="79293-187">Následující příklad vydělí jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="79293-187">The following example divides one parameter by another parameter.</span></span>

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
                "description": "Integer used to divide"
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

<span data-ttu-id="79293-188">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-188">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-189">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-189">Name</span></span> | <span data-ttu-id="79293-190">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-190">Type</span></span> | <span data-ttu-id="79293-191">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-192">divResult</span><span class="sxs-lookup"><span data-stu-id="79293-192">divResult</span></span> | <span data-ttu-id="79293-193">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-193">Int</span></span> | <span data-ttu-id="79293-194">2</span><span class="sxs-lookup"><span data-stu-id="79293-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="79293-195">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="79293-195">float</span></span>
`float(arg1)`

<span data-ttu-id="79293-196">Převede hodnotu na plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-196">Converts the value to a floating point number.</span></span> <span data-ttu-id="79293-197">Pouze použijete tuto funkci při předávání vlastních parametrů aplikace, jako je například aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="79293-197">You only use this function when passing custom parameters to an application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-198">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-198">Parameters</span></span>

| <span data-ttu-id="79293-199">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-199">Parameter</span></span> | <span data-ttu-id="79293-200">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-200">Required</span></span> | <span data-ttu-id="79293-201">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-201">Type</span></span> | <span data-ttu-id="79293-202">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-203">arg1</span><span class="sxs-lookup"><span data-stu-id="79293-203">arg1</span></span> |<span data-ttu-id="79293-204">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-204">Yes</span></span> |<span data-ttu-id="79293-205">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-205">string or int</span></span> |<span data-ttu-id="79293-206">Hodnota, která má převést na plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-206">The value to convert to a floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-207">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-207">Return value</span></span>
<span data-ttu-id="79293-208">Plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="79293-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-209">Example</span></span>

<span data-ttu-id="79293-210">Následující příklad ukazuje, jak použít float a předat parametry do aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="79293-210">The following example shows how to use float to pass parameters to a Logic App:</span></span>

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

## <a name="int"></a><span data-ttu-id="79293-211">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="79293-212">Převede zadanou hodnotu na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-212">Converts the specified value to an integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-213">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-213">Parameters</span></span>

| <span data-ttu-id="79293-214">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-214">Parameter</span></span> | <span data-ttu-id="79293-215">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-215">Required</span></span> | <span data-ttu-id="79293-216">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-216">Type</span></span> | <span data-ttu-id="79293-217">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="79293-218">valueToConvert</span></span> |<span data-ttu-id="79293-219">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-219">Yes</span></span> |<span data-ttu-id="79293-220">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-220">string or int</span></span> |<span data-ttu-id="79293-221">Hodnota převést na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-221">The value to convert to an integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-222">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-222">Return value</span></span>

<span data-ttu-id="79293-223">Celé číslo převedenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="79293-223">An integer of the converted value.</span></span>

### <a name="example"></a><span data-ttu-id="79293-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-224">Example</span></span>

<span data-ttu-id="79293-225">Následující příklad převede hodnotu parametru zadaný uživatelem na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="79293-225">The following example converts the user-provided parameter value to integer.</span></span>

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

<span data-ttu-id="79293-226">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-226">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-227">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-227">Name</span></span> | <span data-ttu-id="79293-228">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-228">Type</span></span> | <span data-ttu-id="79293-229">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-230">Zavřete</span><span class="sxs-lookup"><span data-stu-id="79293-230">intResult</span></span> | <span data-ttu-id="79293-231">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-231">Int</span></span> | <span data-ttu-id="79293-232">4</span><span class="sxs-lookup"><span data-stu-id="79293-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="79293-233">min</span><span class="sxs-lookup"><span data-stu-id="79293-233">min</span></span>
`min (arg1)`

<span data-ttu-id="79293-234">Vrátí minimální hodnotu z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="79293-234">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-235">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-235">Parameters</span></span>

| <span data-ttu-id="79293-236">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-236">Parameter</span></span> | <span data-ttu-id="79293-237">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-237">Required</span></span> | <span data-ttu-id="79293-238">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-238">Type</span></span> | <span data-ttu-id="79293-239">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-240">arg1</span><span class="sxs-lookup"><span data-stu-id="79293-240">arg1</span></span> |<span data-ttu-id="79293-241">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-241">Yes</span></span> |<span data-ttu-id="79293-242">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="79293-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="79293-243">Kolekce získat minimální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="79293-243">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-244">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-244">Return value</span></span>

<span data-ttu-id="79293-245">Celé číslo představující minimální hodnota z kolekce.</span><span class="sxs-lookup"><span data-stu-id="79293-245">An integer representing minimum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="79293-246">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-246">Example</span></span>

<span data-ttu-id="79293-247">Následující příklad ukazuje, jak použít min s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="79293-247">The following example shows how to use min with an array and a list of integers:</span></span>

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

<span data-ttu-id="79293-248">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-248">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-249">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-249">Name</span></span> | <span data-ttu-id="79293-250">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-250">Type</span></span> | <span data-ttu-id="79293-251">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="79293-252">arrayOutput</span></span> | <span data-ttu-id="79293-253">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-253">Int</span></span> | <span data-ttu-id="79293-254">0</span><span class="sxs-lookup"><span data-stu-id="79293-254">0</span></span> |
| <span data-ttu-id="79293-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="79293-255">intOutput</span></span> | <span data-ttu-id="79293-256">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-256">Int</span></span> | <span data-ttu-id="79293-257">0</span><span class="sxs-lookup"><span data-stu-id="79293-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="79293-258">maximální počet</span><span class="sxs-lookup"><span data-stu-id="79293-258">max</span></span>
`max (arg1)`

<span data-ttu-id="79293-259">Vrací maximální hodnotu z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="79293-259">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-260">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-260">Parameters</span></span>

| <span data-ttu-id="79293-261">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-261">Parameter</span></span> | <span data-ttu-id="79293-262">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-262">Required</span></span> | <span data-ttu-id="79293-263">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-263">Type</span></span> | <span data-ttu-id="79293-264">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-265">arg1</span><span class="sxs-lookup"><span data-stu-id="79293-265">arg1</span></span> |<span data-ttu-id="79293-266">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-266">Yes</span></span> |<span data-ttu-id="79293-267">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="79293-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="79293-268">Kolekce získat maximální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="79293-268">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-269">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-269">Return value</span></span>

<span data-ttu-id="79293-270">Celé číslo představující maximální hodnotu z kolekce.</span><span class="sxs-lookup"><span data-stu-id="79293-270">An integer representing the maximum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="79293-271">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-271">Example</span></span>

<span data-ttu-id="79293-272">Následující příklad ukazuje, jak použít maximum s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="79293-272">The following example shows how to use max with an array and a list of integers:</span></span>

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

<span data-ttu-id="79293-273">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-273">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-274">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-274">Name</span></span> | <span data-ttu-id="79293-275">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-275">Type</span></span> | <span data-ttu-id="79293-276">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="79293-277">arrayOutput</span></span> | <span data-ttu-id="79293-278">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-278">Int</span></span> | <span data-ttu-id="79293-279">5</span><span class="sxs-lookup"><span data-stu-id="79293-279">5</span></span> |
| <span data-ttu-id="79293-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="79293-280">intOutput</span></span> | <span data-ttu-id="79293-281">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-281">Int</span></span> | <span data-ttu-id="79293-282">5</span><span class="sxs-lookup"><span data-stu-id="79293-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="79293-283">MOD</span><span class="sxs-lookup"><span data-stu-id="79293-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="79293-284">Vrátí zbytek celočíselného dělení pomocí dvě zadané celá čísla.</span><span class="sxs-lookup"><span data-stu-id="79293-284">Returns the remainder of the integer division using the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-285">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-285">Parameters</span></span>

| <span data-ttu-id="79293-286">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-286">Parameter</span></span> | <span data-ttu-id="79293-287">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-287">Required</span></span> | <span data-ttu-id="79293-288">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-288">Type</span></span> | <span data-ttu-id="79293-289">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-290">operand1</span><span class="sxs-lookup"><span data-stu-id="79293-290">operand1</span></span> |<span data-ttu-id="79293-291">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-291">Yes</span></span> |<span data-ttu-id="79293-292">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-292">int</span></span> |<span data-ttu-id="79293-293">Číslo je rozdělen.</span><span class="sxs-lookup"><span data-stu-id="79293-293">The number being divided.</span></span> |
| <span data-ttu-id="79293-294">operand2</span><span class="sxs-lookup"><span data-stu-id="79293-294">operand2</span></span> |<span data-ttu-id="79293-295">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-295">Yes</span></span> |<span data-ttu-id="79293-296">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-296">int</span></span> |<span data-ttu-id="79293-297">Číslo, které slouží k rozdělení, nemůže být 0.</span><span class="sxs-lookup"><span data-stu-id="79293-297">The number that is used to divide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-298">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-298">Return value</span></span>
<span data-ttu-id="79293-299">Celé číslo představující zbytek.</span><span class="sxs-lookup"><span data-stu-id="79293-299">An integer representing the remainder.</span></span>

### <a name="example"></a><span data-ttu-id="79293-300">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-300">Example</span></span>

<span data-ttu-id="79293-301">Následující příklad vrátí zbytek po dělení jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="79293-301">The following example returns the remainder of dividing one parameter by another parameter.</span></span>

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
                "description": "Integer used to divide"
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

<span data-ttu-id="79293-302">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-302">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-303">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-303">Name</span></span> | <span data-ttu-id="79293-304">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-304">Type</span></span> | <span data-ttu-id="79293-305">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-306">modResult</span><span class="sxs-lookup"><span data-stu-id="79293-306">modResult</span></span> | <span data-ttu-id="79293-307">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-307">Int</span></span> | <span data-ttu-id="79293-308">1</span><span class="sxs-lookup"><span data-stu-id="79293-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="79293-309">mul</span><span class="sxs-lookup"><span data-stu-id="79293-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="79293-310">Vrátí násobení dvě zadané celých čísel.</span><span class="sxs-lookup"><span data-stu-id="79293-310">Returns the multiplication of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-311">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-311">Parameters</span></span>

| <span data-ttu-id="79293-312">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-312">Parameter</span></span> | <span data-ttu-id="79293-313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-313">Required</span></span> | <span data-ttu-id="79293-314">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-314">Type</span></span> | <span data-ttu-id="79293-315">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-316">operand1</span><span class="sxs-lookup"><span data-stu-id="79293-316">operand1</span></span> |<span data-ttu-id="79293-317">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-317">Yes</span></span> |<span data-ttu-id="79293-318">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-318">int</span></span> |<span data-ttu-id="79293-319">První číslo mají vynásobit.</span><span class="sxs-lookup"><span data-stu-id="79293-319">First number to multiply.</span></span> |
| <span data-ttu-id="79293-320">operand2</span><span class="sxs-lookup"><span data-stu-id="79293-320">operand2</span></span> |<span data-ttu-id="79293-321">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-321">Yes</span></span> |<span data-ttu-id="79293-322">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-322">int</span></span> |<span data-ttu-id="79293-323">Druhé číslo, které mají vynásobit.</span><span class="sxs-lookup"><span data-stu-id="79293-323">Second number to multiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-324">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-324">Return value</span></span>

<span data-ttu-id="79293-325">Celé číslo představující násobení.</span><span class="sxs-lookup"><span data-stu-id="79293-325">An integer representing the multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="79293-326">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-326">Example</span></span>

<span data-ttu-id="79293-327">Následující příklad vynásobí jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="79293-327">The following example multiplies one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
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

<span data-ttu-id="79293-328">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-328">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-329">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-329">Name</span></span> | <span data-ttu-id="79293-330">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-330">Type</span></span> | <span data-ttu-id="79293-331">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="79293-332">mulResult</span></span> | <span data-ttu-id="79293-333">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-333">Int</span></span> | <span data-ttu-id="79293-334">15</span><span class="sxs-lookup"><span data-stu-id="79293-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="79293-335">Sub –</span><span class="sxs-lookup"><span data-stu-id="79293-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="79293-336">Vrátí odčítání dvě zadané celých čísel.</span><span class="sxs-lookup"><span data-stu-id="79293-336">Returns the subtraction of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="79293-337">Parametry</span><span class="sxs-lookup"><span data-stu-id="79293-337">Parameters</span></span>

| <span data-ttu-id="79293-338">Parametr</span><span class="sxs-lookup"><span data-stu-id="79293-338">Parameter</span></span> | <span data-ttu-id="79293-339">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="79293-339">Required</span></span> | <span data-ttu-id="79293-340">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-340">Type</span></span> | <span data-ttu-id="79293-341">Popis</span><span class="sxs-lookup"><span data-stu-id="79293-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="79293-342">operand1</span><span class="sxs-lookup"><span data-stu-id="79293-342">operand1</span></span> |<span data-ttu-id="79293-343">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-343">Yes</span></span> |<span data-ttu-id="79293-344">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-344">int</span></span> |<span data-ttu-id="79293-345">Číslo, které je odečten od.</span><span class="sxs-lookup"><span data-stu-id="79293-345">The number that is subtracted from.</span></span> |
| <span data-ttu-id="79293-346">operand2</span><span class="sxs-lookup"><span data-stu-id="79293-346">operand2</span></span> |<span data-ttu-id="79293-347">Ano</span><span class="sxs-lookup"><span data-stu-id="79293-347">Yes</span></span> |<span data-ttu-id="79293-348">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-348">int</span></span> |<span data-ttu-id="79293-349">Číslo, které je odečten.</span><span class="sxs-lookup"><span data-stu-id="79293-349">The number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="79293-350">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-350">Return value</span></span>
<span data-ttu-id="79293-351">Celé číslo představující odčítání.</span><span class="sxs-lookup"><span data-stu-id="79293-351">An integer representing the subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="79293-352">Příklad</span><span class="sxs-lookup"><span data-stu-id="79293-352">Example</span></span>

<span data-ttu-id="79293-353">Následující příklad odečítá od jiného parametru jeden parametr.</span><span class="sxs-lookup"><span data-stu-id="79293-353">The following example subtracts one parameter from another parameter.</span></span>

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
                "description": "Integer to subtract"
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

<span data-ttu-id="79293-354">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="79293-354">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="79293-355">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="79293-355">Name</span></span> | <span data-ttu-id="79293-356">Typ</span><span class="sxs-lookup"><span data-stu-id="79293-356">Type</span></span> | <span data-ttu-id="79293-357">Hodnota</span><span class="sxs-lookup"><span data-stu-id="79293-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="79293-358">subResult</span><span class="sxs-lookup"><span data-stu-id="79293-358">subResult</span></span> | <span data-ttu-id="79293-359">celá čísla</span><span class="sxs-lookup"><span data-stu-id="79293-359">Int</span></span> | <span data-ttu-id="79293-360">4</span><span class="sxs-lookup"><span data-stu-id="79293-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="79293-361">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79293-361">Next steps</span></span>
* <span data-ttu-id="79293-362">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="79293-362">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="79293-363">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="79293-363">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="79293-364">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="79293-364">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="79293-365">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="79293-365">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

