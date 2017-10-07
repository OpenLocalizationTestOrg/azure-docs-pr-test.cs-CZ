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
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="dc6bb-103">Numerické funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="dc6bb-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="dc6bb-104">Resource Manager poskytuje následující funkce pro práci s celými čísly hello:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-104">Resource Manager provides hello following functions for working with integers:</span></span>

* [<span data-ttu-id="dc6bb-105">Přidat</span><span class="sxs-lookup"><span data-stu-id="dc6bb-105">add</span></span>](#add)
* [<span data-ttu-id="dc6bb-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="dc6bb-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="dc6bb-107">div</span><span class="sxs-lookup"><span data-stu-id="dc6bb-107">div</span></span>](#div)
* [<span data-ttu-id="dc6bb-108">plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="dc6bb-108">float</span></span>](#float)
* [<span data-ttu-id="dc6bb-109">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-109">int</span></span>](#int)
* [<span data-ttu-id="dc6bb-110">min.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-110">min</span></span>](#min)
* [<span data-ttu-id="dc6bb-111">maximální počet</span><span class="sxs-lookup"><span data-stu-id="dc6bb-111">max</span></span>](#max)
* [<span data-ttu-id="dc6bb-112">MOD</span><span class="sxs-lookup"><span data-stu-id="dc6bb-112">mod</span></span>](#mod)
* [<span data-ttu-id="dc6bb-113">mul</span><span class="sxs-lookup"><span data-stu-id="dc6bb-113">mul</span></span>](#mul)
* [<span data-ttu-id="dc6bb-114">Sub –</span><span class="sxs-lookup"><span data-stu-id="dc6bb-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="dc6bb-115">Přidat</span><span class="sxs-lookup"><span data-stu-id="dc6bb-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="dc6bb-116">Vrátí hello součet hello dvě zadané celá čísla.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-116">Returns hello sum of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-117">Parameters</span></span>

| <span data-ttu-id="dc6bb-118">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-118">Parameter</span></span> | <span data-ttu-id="dc6bb-119">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-119">Required</span></span> | <span data-ttu-id="dc6bb-120">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-120">Type</span></span> | <span data-ttu-id="dc6bb-121">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="dc6bb-122">operand1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-122">operand1</span></span> |<span data-ttu-id="dc6bb-123">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-123">Yes</span></span> |<span data-ttu-id="dc6bb-124">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-124">int</span></span> |<span data-ttu-id="dc6bb-125">První číslo tooadd.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-125">First number tooadd.</span></span> |
|<span data-ttu-id="dc6bb-126">operand2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-126">operand2</span></span> |<span data-ttu-id="dc6bb-127">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-127">Yes</span></span> |<span data-ttu-id="dc6bb-128">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-128">int</span></span> |<span data-ttu-id="dc6bb-129">Druhé číslo tooadd.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-129">Second number tooadd.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-130">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-130">Return value</span></span>

<span data-ttu-id="dc6bb-131">Celé číslo, které obsahuje součet hello hello parametry.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-131">An integer that contains hello sum of hello parameters.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-132">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-132">Example</span></span>

<span data-ttu-id="dc6bb-133">Hello následující příklad přidá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-133">hello following example adds two parameters.</span></span>

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

<span data-ttu-id="dc6bb-134">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-134">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-135">Name</span></span> | <span data-ttu-id="dc6bb-136">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-136">Type</span></span> | <span data-ttu-id="dc6bb-137">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-138">addResult</span><span class="sxs-lookup"><span data-stu-id="dc6bb-138">addResult</span></span> | <span data-ttu-id="dc6bb-139">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-139">Int</span></span> | <span data-ttu-id="dc6bb-140">8</span><span class="sxs-lookup"><span data-stu-id="dc6bb-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="dc6bb-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="dc6bb-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="dc6bb-142">Vrátí hello index smyčky iterací.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-142">Returns hello index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="dc6bb-143">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-143">Parameters</span></span>

| <span data-ttu-id="dc6bb-144">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-144">Parameter</span></span> | <span data-ttu-id="dc6bb-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-145">Required</span></span> | <span data-ttu-id="dc6bb-146">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-146">Type</span></span> | <span data-ttu-id="dc6bb-147">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-148">loopName</span><span class="sxs-lookup"><span data-stu-id="dc6bb-148">loopName</span></span> | <span data-ttu-id="dc6bb-149">Ne</span><span class="sxs-lookup"><span data-stu-id="dc6bb-149">No</span></span> | <span data-ttu-id="dc6bb-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="dc6bb-150">string</span></span> | <span data-ttu-id="dc6bb-151">Hello název pro získávání hello iteraci smyčky hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-151">hello name of hello loop for getting hello iteration.</span></span> |
| <span data-ttu-id="dc6bb-152">Posun</span><span class="sxs-lookup"><span data-stu-id="dc6bb-152">offset</span></span> |<span data-ttu-id="dc6bb-153">Ne</span><span class="sxs-lookup"><span data-stu-id="dc6bb-153">No</span></span> |<span data-ttu-id="dc6bb-154">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-154">int</span></span> |<span data-ttu-id="dc6bb-155">Hello číslo tooadd toohello nule iterace hodnota.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-155">hello number tooadd toohello zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="dc6bb-156">Poznámky</span><span class="sxs-lookup"><span data-stu-id="dc6bb-156">Remarks</span></span>

<span data-ttu-id="dc6bb-157">Tato funkce se vždy používá s **kopie** objektu.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="dc6bb-158">Pokud není zadána žádná hodnota pro **posun**, je vrácena hodnota aktuální iterace hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-158">If no value is provided for **offset**, hello current iteration value is returned.</span></span> <span data-ttu-id="dc6bb-159">Hodnota iterace Hello začíná od nuly.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-159">hello iteration value starts at zero.</span></span>

<span data-ttu-id="dc6bb-160">Hello **loopName** vlastnost vám umožní toospecify zda copyIndex odkazuje tooa prostředků iterace nebo vlastnost iterací.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-160">hello **loopName** property enables you toospecify whether copyIndex is referring tooa resource iteration or property iteration.</span></span> <span data-ttu-id="dc6bb-161">Pokud není zadána žádná hodnota pro **loopName**, se používá hello aktuální iterace typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-161">If no value is provided for **loopName**, hello current resource type iteration is used.</span></span> <span data-ttu-id="dc6bb-162">Zadejte hodnotu pro **loopName** během iterace u vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="dc6bb-163">Úplný popis jak používat **copyIndex**, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="dc6bb-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-164">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-164">Example</span></span>

<span data-ttu-id="dc6bb-165">Hello následující příklad ukazuje kopírování smyčky a hello index hodnotu součástí názvu hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-165">hello following example shows a copy loop and hello index value included in hello name.</span></span> 

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

### <a name="return-value"></a><span data-ttu-id="dc6bb-166">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-166">Return value</span></span>

<span data-ttu-id="dc6bb-167">Celé číslo představující aktuální index hello iterace hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-167">An integer representing hello current index of hello iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="dc6bb-168">div</span><span class="sxs-lookup"><span data-stu-id="dc6bb-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="dc6bb-169">Vrátí hello celočíselného dělení čísla hello dvě zadané celá čísla.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-169">Returns hello integer division of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-170">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-170">Parameters</span></span>

| <span data-ttu-id="dc6bb-171">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-171">Parameter</span></span> | <span data-ttu-id="dc6bb-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-172">Required</span></span> | <span data-ttu-id="dc6bb-173">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-173">Type</span></span> | <span data-ttu-id="dc6bb-174">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-175">operand1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-175">operand1</span></span> |<span data-ttu-id="dc6bb-176">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-176">Yes</span></span> |<span data-ttu-id="dc6bb-177">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-177">int</span></span> |<span data-ttu-id="dc6bb-178">číslo Hello rozdělené.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-178">hello number being divided.</span></span> |
| <span data-ttu-id="dc6bb-179">operand2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-179">operand2</span></span> |<span data-ttu-id="dc6bb-180">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-180">Yes</span></span> |<span data-ttu-id="dc6bb-181">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-181">int</span></span> |<span data-ttu-id="dc6bb-182">Hello číslo, které je použité toodivide.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-182">hello number that is used toodivide.</span></span> <span data-ttu-id="dc6bb-183">Nemůže být 0.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-184">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-184">Return value</span></span>

<span data-ttu-id="dc6bb-185">Dělení celého čísla představující hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-185">An integer representing hello division.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-186">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-186">Example</span></span>

<span data-ttu-id="dc6bb-187">Následující ukázka Hello vydělí jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-187">hello following example divides one parameter by another parameter.</span></span>

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

<span data-ttu-id="dc6bb-188">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-188">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-189">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-189">Name</span></span> | <span data-ttu-id="dc6bb-190">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-190">Type</span></span> | <span data-ttu-id="dc6bb-191">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-192">divResult</span><span class="sxs-lookup"><span data-stu-id="dc6bb-192">divResult</span></span> | <span data-ttu-id="dc6bb-193">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-193">Int</span></span> | <span data-ttu-id="dc6bb-194">2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="dc6bb-195">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="dc6bb-195">float</span></span>
`float(arg1)`

<span data-ttu-id="dc6bb-196">Převede tooa hello hodnotu bodu číslo s plovoucí čárkou.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-196">Converts hello value tooa floating point number.</span></span> <span data-ttu-id="dc6bb-197">Při předávání vlastních parametrů tooan aplikaci, například aplikace logiky pouze použití této funkce.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-197">You only use this function when passing custom parameters tooan application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-198">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-198">Parameters</span></span>

| <span data-ttu-id="dc6bb-199">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-199">Parameter</span></span> | <span data-ttu-id="dc6bb-200">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-200">Required</span></span> | <span data-ttu-id="dc6bb-201">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-201">Type</span></span> | <span data-ttu-id="dc6bb-202">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-203">arg1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-203">arg1</span></span> |<span data-ttu-id="dc6bb-204">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-204">Yes</span></span> |<span data-ttu-id="dc6bb-205">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-205">string or int</span></span> |<span data-ttu-id="dc6bb-206">tooconvert tooa Hello hodnotu bodu číslo s plovoucí čárkou.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-206">hello value tooconvert tooa floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-207">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-207">Return value</span></span>
<span data-ttu-id="dc6bb-208">Plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-209">Example</span></span>

<span data-ttu-id="dc6bb-210">Hello následující příklad ukazuje, jak toouse float toopass parametry tooa aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-210">hello following example shows how toouse float toopass parameters tooa Logic App:</span></span>

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

## <a name="int"></a><span data-ttu-id="dc6bb-211">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="dc6bb-212">Převede hello zadaná hodnota tooan celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-212">Converts hello specified value tooan integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-213">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-213">Parameters</span></span>

| <span data-ttu-id="dc6bb-214">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-214">Parameter</span></span> | <span data-ttu-id="dc6bb-215">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-215">Required</span></span> | <span data-ttu-id="dc6bb-216">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-216">Type</span></span> | <span data-ttu-id="dc6bb-217">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="dc6bb-218">valueToConvert</span></span> |<span data-ttu-id="dc6bb-219">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-219">Yes</span></span> |<span data-ttu-id="dc6bb-220">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-220">string or int</span></span> |<span data-ttu-id="dc6bb-221">Hello hodnotu tooconvert tooan celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-221">hello value tooconvert tooan integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-222">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-222">Return value</span></span>

<span data-ttu-id="dc6bb-223">Celé číslo hodnoty hello převést.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-223">An integer of hello converted value.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-224">Example</span></span>

<span data-ttu-id="dc6bb-225">Hello následující příklad převede toointeger hodnota hello parametr zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-225">hello following example converts hello user-provided parameter value toointeger.</span></span>

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

<span data-ttu-id="dc6bb-226">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-226">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-227">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-227">Name</span></span> | <span data-ttu-id="dc6bb-228">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-228">Type</span></span> | <span data-ttu-id="dc6bb-229">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-230">Zavřete</span><span class="sxs-lookup"><span data-stu-id="dc6bb-230">intResult</span></span> | <span data-ttu-id="dc6bb-231">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-231">Int</span></span> | <span data-ttu-id="dc6bb-232">4</span><span class="sxs-lookup"><span data-stu-id="dc6bb-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="dc6bb-233">min</span><span class="sxs-lookup"><span data-stu-id="dc6bb-233">min</span></span>
`min (arg1)`

<span data-ttu-id="dc6bb-234">Vrátí hello minimální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-234">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-235">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-235">Parameters</span></span>

| <span data-ttu-id="dc6bb-236">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-236">Parameter</span></span> | <span data-ttu-id="dc6bb-237">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-237">Required</span></span> | <span data-ttu-id="dc6bb-238">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-238">Type</span></span> | <span data-ttu-id="dc6bb-239">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-240">arg1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-240">arg1</span></span> |<span data-ttu-id="dc6bb-241">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-241">Yes</span></span> |<span data-ttu-id="dc6bb-242">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="dc6bb-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="dc6bb-243">Hello kolekce tooget hello minimální hodnota.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-243">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-244">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-244">Return value</span></span>

<span data-ttu-id="dc6bb-245">Celé číslo představující minimální hodnota z kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-245">An integer representing minimum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-246">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-246">Example</span></span>

<span data-ttu-id="dc6bb-247">Následující příklad ukazuje, jak Hello toouse min s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-247">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="dc6bb-248">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-248">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-249">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-249">Name</span></span> | <span data-ttu-id="dc6bb-250">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-250">Type</span></span> | <span data-ttu-id="dc6bb-251">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="dc6bb-252">arrayOutput</span></span> | <span data-ttu-id="dc6bb-253">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-253">Int</span></span> | <span data-ttu-id="dc6bb-254">0</span><span class="sxs-lookup"><span data-stu-id="dc6bb-254">0</span></span> |
| <span data-ttu-id="dc6bb-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="dc6bb-255">intOutput</span></span> | <span data-ttu-id="dc6bb-256">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-256">Int</span></span> | <span data-ttu-id="dc6bb-257">0</span><span class="sxs-lookup"><span data-stu-id="dc6bb-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="dc6bb-258">maximální počet</span><span class="sxs-lookup"><span data-stu-id="dc6bb-258">max</span></span>
`max (arg1)`

<span data-ttu-id="dc6bb-259">Vrátí hello maximální hodnota z pole celá čísla nebo seznam celých čísel oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-259">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-260">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-260">Parameters</span></span>

| <span data-ttu-id="dc6bb-261">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-261">Parameter</span></span> | <span data-ttu-id="dc6bb-262">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-262">Required</span></span> | <span data-ttu-id="dc6bb-263">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-263">Type</span></span> | <span data-ttu-id="dc6bb-264">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-265">arg1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-265">arg1</span></span> |<span data-ttu-id="dc6bb-266">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-266">Yes</span></span> |<span data-ttu-id="dc6bb-267">pole celá čísla nebo seznam celých čísel oddělených čárkou</span><span class="sxs-lookup"><span data-stu-id="dc6bb-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="dc6bb-268">Hello kolekce tooget hello maximální hodnota.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-268">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-269">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-269">Return value</span></span>

<span data-ttu-id="dc6bb-270">Celé číslo představující hello maximální hodnotu z kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-270">An integer representing hello maximum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-271">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-271">Example</span></span>

<span data-ttu-id="dc6bb-272">Následující příklad ukazuje, jak Hello toouse maximální s pole a seznam celých čísel:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-272">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="dc6bb-273">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-273">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-274">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-274">Name</span></span> | <span data-ttu-id="dc6bb-275">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-275">Type</span></span> | <span data-ttu-id="dc6bb-276">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="dc6bb-277">arrayOutput</span></span> | <span data-ttu-id="dc6bb-278">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-278">Int</span></span> | <span data-ttu-id="dc6bb-279">5</span><span class="sxs-lookup"><span data-stu-id="dc6bb-279">5</span></span> |
| <span data-ttu-id="dc6bb-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="dc6bb-280">intOutput</span></span> | <span data-ttu-id="dc6bb-281">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-281">Int</span></span> | <span data-ttu-id="dc6bb-282">5</span><span class="sxs-lookup"><span data-stu-id="dc6bb-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="dc6bb-283">MOD</span><span class="sxs-lookup"><span data-stu-id="dc6bb-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="dc6bb-284">Vrátí zbytek hello hello celočíselné dělení pomocí hello dvě zadané celá čísla.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-284">Returns hello remainder of hello integer division using hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-285">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-285">Parameters</span></span>

| <span data-ttu-id="dc6bb-286">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-286">Parameter</span></span> | <span data-ttu-id="dc6bb-287">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-287">Required</span></span> | <span data-ttu-id="dc6bb-288">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-288">Type</span></span> | <span data-ttu-id="dc6bb-289">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-290">operand1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-290">operand1</span></span> |<span data-ttu-id="dc6bb-291">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-291">Yes</span></span> |<span data-ttu-id="dc6bb-292">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-292">int</span></span> |<span data-ttu-id="dc6bb-293">číslo Hello rozdělené.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-293">hello number being divided.</span></span> |
| <span data-ttu-id="dc6bb-294">operand2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-294">operand2</span></span> |<span data-ttu-id="dc6bb-295">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-295">Yes</span></span> |<span data-ttu-id="dc6bb-296">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-296">int</span></span> |<span data-ttu-id="dc6bb-297">Hello číslo, které je použité toodivide, nemůže být 0.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-297">hello number that is used toodivide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-298">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-298">Return value</span></span>
<span data-ttu-id="dc6bb-299">Celé číslo představující hello zbytek.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-299">An integer representing hello remainder.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-300">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-300">Example</span></span>

<span data-ttu-id="dc6bb-301">Hello následující příklad vrátí hello zbytek po dělení jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-301">hello following example returns hello remainder of dividing one parameter by another parameter.</span></span>

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

<span data-ttu-id="dc6bb-302">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-302">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-303">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-303">Name</span></span> | <span data-ttu-id="dc6bb-304">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-304">Type</span></span> | <span data-ttu-id="dc6bb-305">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-306">modResult</span><span class="sxs-lookup"><span data-stu-id="dc6bb-306">modResult</span></span> | <span data-ttu-id="dc6bb-307">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-307">Int</span></span> | <span data-ttu-id="dc6bb-308">1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="dc6bb-309">mul</span><span class="sxs-lookup"><span data-stu-id="dc6bb-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="dc6bb-310">Vrátí hello násobení hello dvě zadané celých čísel.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-310">Returns hello multiplication of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-311">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-311">Parameters</span></span>

| <span data-ttu-id="dc6bb-312">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-312">Parameter</span></span> | <span data-ttu-id="dc6bb-313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-313">Required</span></span> | <span data-ttu-id="dc6bb-314">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-314">Type</span></span> | <span data-ttu-id="dc6bb-315">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-316">operand1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-316">operand1</span></span> |<span data-ttu-id="dc6bb-317">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-317">Yes</span></span> |<span data-ttu-id="dc6bb-318">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-318">int</span></span> |<span data-ttu-id="dc6bb-319">První číslo toomultiply.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-319">First number toomultiply.</span></span> |
| <span data-ttu-id="dc6bb-320">operand2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-320">operand2</span></span> |<span data-ttu-id="dc6bb-321">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-321">Yes</span></span> |<span data-ttu-id="dc6bb-322">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-322">int</span></span> |<span data-ttu-id="dc6bb-323">Druhé číslo toomultiply.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-323">Second number toomultiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-324">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-324">Return value</span></span>

<span data-ttu-id="dc6bb-325">Celé číslo představující hello násobení.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-325">An integer representing hello multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-326">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-326">Example</span></span>

<span data-ttu-id="dc6bb-327">Následující ukázka Hello vynásobí jeden parametr jiné parametrem.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-327">hello following example multiplies one parameter by another parameter.</span></span>

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

<span data-ttu-id="dc6bb-328">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-328">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-329">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-329">Name</span></span> | <span data-ttu-id="dc6bb-330">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-330">Type</span></span> | <span data-ttu-id="dc6bb-331">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="dc6bb-332">mulResult</span></span> | <span data-ttu-id="dc6bb-333">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-333">Int</span></span> | <span data-ttu-id="dc6bb-334">15</span><span class="sxs-lookup"><span data-stu-id="dc6bb-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="dc6bb-335">Sub –</span><span class="sxs-lookup"><span data-stu-id="dc6bb-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="dc6bb-336">Vrátí hello odčítání hello dvě zadané celých čísel.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-336">Returns hello subtraction of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="dc6bb-337">Parametry</span><span class="sxs-lookup"><span data-stu-id="dc6bb-337">Parameters</span></span>

| <span data-ttu-id="dc6bb-338">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc6bb-338">Parameter</span></span> | <span data-ttu-id="dc6bb-339">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dc6bb-339">Required</span></span> | <span data-ttu-id="dc6bb-340">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-340">Type</span></span> | <span data-ttu-id="dc6bb-341">Popis</span><span class="sxs-lookup"><span data-stu-id="dc6bb-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc6bb-342">operand1</span><span class="sxs-lookup"><span data-stu-id="dc6bb-342">operand1</span></span> |<span data-ttu-id="dc6bb-343">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-343">Yes</span></span> |<span data-ttu-id="dc6bb-344">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-344">int</span></span> |<span data-ttu-id="dc6bb-345">Hello číslo, které je odečten od.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-345">hello number that is subtracted from.</span></span> |
| <span data-ttu-id="dc6bb-346">operand2</span><span class="sxs-lookup"><span data-stu-id="dc6bb-346">operand2</span></span> |<span data-ttu-id="dc6bb-347">Ano</span><span class="sxs-lookup"><span data-stu-id="dc6bb-347">Yes</span></span> |<span data-ttu-id="dc6bb-348">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-348">int</span></span> |<span data-ttu-id="dc6bb-349">Hello číslo, které je odečten.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-349">hello number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="dc6bb-350">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-350">Return value</span></span>
<span data-ttu-id="dc6bb-351">Celé číslo představující hello odčítání.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-351">An integer representing hello subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="dc6bb-352">Příklad</span><span class="sxs-lookup"><span data-stu-id="dc6bb-352">Example</span></span>

<span data-ttu-id="dc6bb-353">Následující ukázka Hello odečítá jeden parametr z jiného parametru.</span><span class="sxs-lookup"><span data-stu-id="dc6bb-353">hello following example subtracts one parameter from another parameter.</span></span>

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

<span data-ttu-id="dc6bb-354">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="dc6bb-354">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="dc6bb-355">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc6bb-355">Name</span></span> | <span data-ttu-id="dc6bb-356">Typ</span><span class="sxs-lookup"><span data-stu-id="dc6bb-356">Type</span></span> | <span data-ttu-id="dc6bb-357">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dc6bb-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="dc6bb-358">subResult</span><span class="sxs-lookup"><span data-stu-id="dc6bb-358">subResult</span></span> | <span data-ttu-id="dc6bb-359">celá čísla</span><span class="sxs-lookup"><span data-stu-id="dc6bb-359">Int</span></span> | <span data-ttu-id="dc6bb-360">4</span><span class="sxs-lookup"><span data-stu-id="dc6bb-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dc6bb-361">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc6bb-361">Next steps</span></span>
* <span data-ttu-id="dc6bb-362">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dc6bb-362">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="dc6bb-363">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dc6bb-363">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="dc6bb-364">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="dc6bb-364">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="dc6bb-365">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="dc6bb-365">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

