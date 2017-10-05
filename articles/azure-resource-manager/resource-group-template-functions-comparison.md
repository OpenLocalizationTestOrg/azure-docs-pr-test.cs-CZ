---
title: "Funkce šablon Azure Resource Manager - porovnání | Microsoft Docs"
description: "Popisuje funkce pro použití pro porovnání hodnot v šablonu Azure Resource Manager."
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
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="68a32-103">Porovnání funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="68a32-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="68a32-104">Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.</span><span class="sxs-lookup"><span data-stu-id="68a32-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="68a32-105">rovná se</span><span class="sxs-lookup"><span data-stu-id="68a32-105">equals</span></span>](#equals)
* [<span data-ttu-id="68a32-106">větší</span><span class="sxs-lookup"><span data-stu-id="68a32-106">greater</span></span>](#greater)
* [<span data-ttu-id="68a32-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="68a32-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="68a32-108">menší</span><span class="sxs-lookup"><span data-stu-id="68a32-108">less</span></span>](#less)
* [<span data-ttu-id="68a32-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="68a32-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="68a32-110">Rovná se</span><span class="sxs-lookup"><span data-stu-id="68a32-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="68a32-111">Kontroluje, zda dvě hodnoty rovny navzájem.</span><span class="sxs-lookup"><span data-stu-id="68a32-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="68a32-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="68a32-112">Parameters</span></span>

| <span data-ttu-id="68a32-113">Parametr</span><span class="sxs-lookup"><span data-stu-id="68a32-113">Parameter</span></span> | <span data-ttu-id="68a32-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68a32-114">Required</span></span> | <span data-ttu-id="68a32-115">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-115">Type</span></span> | <span data-ttu-id="68a32-116">Popis</span><span class="sxs-lookup"><span data-stu-id="68a32-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="68a32-117">arg1</span><span class="sxs-lookup"><span data-stu-id="68a32-117">arg1</span></span> |<span data-ttu-id="68a32-118">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-118">Yes</span></span> |<span data-ttu-id="68a32-119">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="68a32-119">int, string, array, or object</span></span> |<span data-ttu-id="68a32-120">První hodnota ke kontrole rovnosti.</span><span class="sxs-lookup"><span data-stu-id="68a32-120">The first value to check for equality.</span></span> |
| <span data-ttu-id="68a32-121">arg2</span><span class="sxs-lookup"><span data-stu-id="68a32-121">arg2</span></span> |<span data-ttu-id="68a32-122">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-122">Yes</span></span> |<span data-ttu-id="68a32-123">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="68a32-123">int, string, array, or object</span></span> |<span data-ttu-id="68a32-124">Druhá hodnota ke kontrole rovnosti.</span><span class="sxs-lookup"><span data-stu-id="68a32-124">The second value to check for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="68a32-125">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-125">Return value</span></span>

<span data-ttu-id="68a32-126">Vrátí **True** Pokud hodnoty jsou stejné, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="68a32-126">Returns **True** if the values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="68a32-127">Poznámky</span><span class="sxs-lookup"><span data-stu-id="68a32-127">Remarks</span></span>

<span data-ttu-id="68a32-128">Funkce rovná se často používá u `condition` elementu, který chcete otestovat, zda je nasazená prostředku.</span><span class="sxs-lookup"><span data-stu-id="68a32-128">The equals function is often used with the `condition` element to test whether a resource is deployed.</span></span>

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

### <a name="example"></a><span data-ttu-id="68a32-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="68a32-129">Example</span></span>

<span data-ttu-id="68a32-130">Příklad šablony kontroluje různé typy hodnot rovnosti.</span><span class="sxs-lookup"><span data-stu-id="68a32-130">The example template checks different types of values for equality.</span></span> <span data-ttu-id="68a32-131">Všechny výchozí hodnoty vrátí hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="68a32-131">All the default values return True.</span></span>

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

<span data-ttu-id="68a32-132">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="68a32-132">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="68a32-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-133">Name</span></span> | <span data-ttu-id="68a32-134">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-134">Type</span></span> | <span data-ttu-id="68a32-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="68a32-136">checkInts</span></span> | <span data-ttu-id="68a32-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-137">Bool</span></span> | <span data-ttu-id="68a32-138">True</span><span class="sxs-lookup"><span data-stu-id="68a32-138">True</span></span> |
| <span data-ttu-id="68a32-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="68a32-139">checkStrings</span></span> | <span data-ttu-id="68a32-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-140">Bool</span></span> | <span data-ttu-id="68a32-141">True</span><span class="sxs-lookup"><span data-stu-id="68a32-141">True</span></span> |
| <span data-ttu-id="68a32-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="68a32-142">checkArrays</span></span> | <span data-ttu-id="68a32-143">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-143">Bool</span></span> | <span data-ttu-id="68a32-144">True</span><span class="sxs-lookup"><span data-stu-id="68a32-144">True</span></span> |
| <span data-ttu-id="68a32-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="68a32-145">checkObjects</span></span> | <span data-ttu-id="68a32-146">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-146">Bool</span></span> | <span data-ttu-id="68a32-147">True</span><span class="sxs-lookup"><span data-stu-id="68a32-147">True</span></span> |


<span data-ttu-id="68a32-148">Následující příklad používá [není](resource-group-template-functions-logical.md#not) s **rovná**.</span><span class="sxs-lookup"><span data-stu-id="68a32-148">The following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="68a32-149">Výstup z předchozího příkladu je:</span><span class="sxs-lookup"><span data-stu-id="68a32-149">The output from the preceding example is:</span></span>

| <span data-ttu-id="68a32-150">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-150">Name</span></span> | <span data-ttu-id="68a32-151">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-151">Type</span></span> | <span data-ttu-id="68a32-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="68a32-153">checkNotEquals</span></span> | <span data-ttu-id="68a32-154">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-154">Bool</span></span> | <span data-ttu-id="68a32-155">True</span><span class="sxs-lookup"><span data-stu-id="68a32-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="68a32-156">větší</span><span class="sxs-lookup"><span data-stu-id="68a32-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="68a32-157">Kontroluje, zda je první hodnota je větší než druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="68a32-157">Checks whether the first value is greater than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="68a32-158">Parametry</span><span class="sxs-lookup"><span data-stu-id="68a32-158">Parameters</span></span>

| <span data-ttu-id="68a32-159">Parametr</span><span class="sxs-lookup"><span data-stu-id="68a32-159">Parameter</span></span> | <span data-ttu-id="68a32-160">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68a32-160">Required</span></span> | <span data-ttu-id="68a32-161">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-161">Type</span></span> | <span data-ttu-id="68a32-162">Popis</span><span class="sxs-lookup"><span data-stu-id="68a32-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="68a32-163">arg1</span><span class="sxs-lookup"><span data-stu-id="68a32-163">arg1</span></span> |<span data-ttu-id="68a32-164">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-164">Yes</span></span> |<span data-ttu-id="68a32-165">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-165">int or string</span></span> |<span data-ttu-id="68a32-166">První hodnota větší porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-166">The first value for the greater comparison.</span></span> |
| <span data-ttu-id="68a32-167">arg2</span><span class="sxs-lookup"><span data-stu-id="68a32-167">arg2</span></span> |<span data-ttu-id="68a32-168">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-168">Yes</span></span> |<span data-ttu-id="68a32-169">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-169">int or string</span></span> |<span data-ttu-id="68a32-170">Druhá hodnota, pro větší porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-170">The second value for the greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="68a32-171">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-171">Return value</span></span>

<span data-ttu-id="68a32-172">Vrátí **True** Pokud první hodnota je větší než druhá hodnota, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="68a32-172">Returns **True** if the first value is greater than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="68a32-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="68a32-173">Example</span></span>

<span data-ttu-id="68a32-174">Příklad šablony zkontroluje, zda je větší než druhá jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="68a32-174">The example template checks whether the one value is greater than the other.</span></span>

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

<span data-ttu-id="68a32-175">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="68a32-175">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="68a32-176">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-176">Name</span></span> | <span data-ttu-id="68a32-177">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-177">Type</span></span> | <span data-ttu-id="68a32-178">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="68a32-179">checkInts</span></span> | <span data-ttu-id="68a32-180">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-180">Bool</span></span> | <span data-ttu-id="68a32-181">False</span><span class="sxs-lookup"><span data-stu-id="68a32-181">False</span></span> |
| <span data-ttu-id="68a32-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="68a32-182">checkStrings</span></span> | <span data-ttu-id="68a32-183">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-183">Bool</span></span> | <span data-ttu-id="68a32-184">True</span><span class="sxs-lookup"><span data-stu-id="68a32-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="68a32-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="68a32-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="68a32-186">Kontroluje, zda je první hodnota je větší než nebo rovna hodnotě druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="68a32-186">Checks whether the first value is greater than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="68a32-187">Parametry</span><span class="sxs-lookup"><span data-stu-id="68a32-187">Parameters</span></span>

| <span data-ttu-id="68a32-188">Parametr</span><span class="sxs-lookup"><span data-stu-id="68a32-188">Parameter</span></span> | <span data-ttu-id="68a32-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68a32-189">Required</span></span> | <span data-ttu-id="68a32-190">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-190">Type</span></span> | <span data-ttu-id="68a32-191">Popis</span><span class="sxs-lookup"><span data-stu-id="68a32-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="68a32-192">arg1</span><span class="sxs-lookup"><span data-stu-id="68a32-192">arg1</span></span> |<span data-ttu-id="68a32-193">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-193">Yes</span></span> |<span data-ttu-id="68a32-194">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-194">int or string</span></span> |<span data-ttu-id="68a32-195">První hodnotu pro porovnání větší nebo rovna.</span><span class="sxs-lookup"><span data-stu-id="68a32-195">The first value for the greater or equal comparison.</span></span> |
| <span data-ttu-id="68a32-196">arg2</span><span class="sxs-lookup"><span data-stu-id="68a32-196">arg2</span></span> |<span data-ttu-id="68a32-197">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-197">Yes</span></span> |<span data-ttu-id="68a32-198">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-198">int or string</span></span> |<span data-ttu-id="68a32-199">Druhá hodnota pro porovnání větší nebo rovna.</span><span class="sxs-lookup"><span data-stu-id="68a32-199">The second value for the greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="68a32-200">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-200">Return value</span></span>

<span data-ttu-id="68a32-201">Vrátí **True** Pokud první hodnota je větší než nebo rovno druhá hodnota, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="68a32-201">Returns **True** if the first value is greater than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="68a32-202">Příklad</span><span class="sxs-lookup"><span data-stu-id="68a32-202">Example</span></span>

<span data-ttu-id="68a32-203">Příklad šablony kontroluje, zda jedna hodnota je větší než nebo rovna hodnotě dalších.</span><span class="sxs-lookup"><span data-stu-id="68a32-203">The example template checks whether the one value is greater than or equal to the other.</span></span>

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

<span data-ttu-id="68a32-204">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="68a32-204">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="68a32-205">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-205">Name</span></span> | <span data-ttu-id="68a32-206">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-206">Type</span></span> | <span data-ttu-id="68a32-207">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="68a32-208">checkInts</span></span> | <span data-ttu-id="68a32-209">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-209">Bool</span></span> | <span data-ttu-id="68a32-210">False</span><span class="sxs-lookup"><span data-stu-id="68a32-210">False</span></span> |
| <span data-ttu-id="68a32-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="68a32-211">checkStrings</span></span> | <span data-ttu-id="68a32-212">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-212">Bool</span></span> | <span data-ttu-id="68a32-213">True</span><span class="sxs-lookup"><span data-stu-id="68a32-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="68a32-214">menší</span><span class="sxs-lookup"><span data-stu-id="68a32-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="68a32-215">Kontroluje, zda je první hodnota menší než druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="68a32-215">Checks whether the first value is less than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="68a32-216">Parametry</span><span class="sxs-lookup"><span data-stu-id="68a32-216">Parameters</span></span>

| <span data-ttu-id="68a32-217">Parametr</span><span class="sxs-lookup"><span data-stu-id="68a32-217">Parameter</span></span> | <span data-ttu-id="68a32-218">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68a32-218">Required</span></span> | <span data-ttu-id="68a32-219">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-219">Type</span></span> | <span data-ttu-id="68a32-220">Popis</span><span class="sxs-lookup"><span data-stu-id="68a32-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="68a32-221">arg1</span><span class="sxs-lookup"><span data-stu-id="68a32-221">arg1</span></span> |<span data-ttu-id="68a32-222">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-222">Yes</span></span> |<span data-ttu-id="68a32-223">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-223">int or string</span></span> |<span data-ttu-id="68a32-224">První hodnota menší porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-224">The first value for the less comparison.</span></span> |
| <span data-ttu-id="68a32-225">arg2</span><span class="sxs-lookup"><span data-stu-id="68a32-225">arg2</span></span> |<span data-ttu-id="68a32-226">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-226">Yes</span></span> |<span data-ttu-id="68a32-227">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-227">int or string</span></span> |<span data-ttu-id="68a32-228">Druhá hodnota pro menší porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-228">The second value for the less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="68a32-229">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-229">Return value</span></span>

<span data-ttu-id="68a32-230">Vrátí **True** Pokud první hodnota je menší než druhá hodnota, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="68a32-230">Returns **True** if the first value is less than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="68a32-231">Příklad</span><span class="sxs-lookup"><span data-stu-id="68a32-231">Example</span></span>

<span data-ttu-id="68a32-232">Příklad šablony kontroluje, zda jedna hodnota je menší než druhý.</span><span class="sxs-lookup"><span data-stu-id="68a32-232">The example template checks whether the one value is less than the other.</span></span>

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

<span data-ttu-id="68a32-233">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="68a32-233">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="68a32-234">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-234">Name</span></span> | <span data-ttu-id="68a32-235">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-235">Type</span></span> | <span data-ttu-id="68a32-236">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="68a32-237">checkInts</span></span> | <span data-ttu-id="68a32-238">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-238">Bool</span></span> | <span data-ttu-id="68a32-239">True</span><span class="sxs-lookup"><span data-stu-id="68a32-239">True</span></span> |
| <span data-ttu-id="68a32-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="68a32-240">checkStrings</span></span> | <span data-ttu-id="68a32-241">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-241">Bool</span></span> | <span data-ttu-id="68a32-242">False</span><span class="sxs-lookup"><span data-stu-id="68a32-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="68a32-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="68a32-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="68a32-244">Kontroluje, zda je první hodnota menší než nebo rovna hodnotě druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="68a32-244">Checks whether the first value is less than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="68a32-245">Parametry</span><span class="sxs-lookup"><span data-stu-id="68a32-245">Parameters</span></span>

| <span data-ttu-id="68a32-246">Parametr</span><span class="sxs-lookup"><span data-stu-id="68a32-246">Parameter</span></span> | <span data-ttu-id="68a32-247">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68a32-247">Required</span></span> | <span data-ttu-id="68a32-248">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-248">Type</span></span> | <span data-ttu-id="68a32-249">Popis</span><span class="sxs-lookup"><span data-stu-id="68a32-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="68a32-250">arg1</span><span class="sxs-lookup"><span data-stu-id="68a32-250">arg1</span></span> |<span data-ttu-id="68a32-251">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-251">Yes</span></span> |<span data-ttu-id="68a32-252">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-252">int or string</span></span> |<span data-ttu-id="68a32-253">První hodnota je menší nebo rovná porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-253">The first value for the less or equals comparison.</span></span> |
| <span data-ttu-id="68a32-254">arg2</span><span class="sxs-lookup"><span data-stu-id="68a32-254">arg2</span></span> |<span data-ttu-id="68a32-255">Ano</span><span class="sxs-lookup"><span data-stu-id="68a32-255">Yes</span></span> |<span data-ttu-id="68a32-256">int nebo string</span><span class="sxs-lookup"><span data-stu-id="68a32-256">int or string</span></span> |<span data-ttu-id="68a32-257">Druhá hodnota je menší nebo rovná porovnání.</span><span class="sxs-lookup"><span data-stu-id="68a32-257">The second value for the less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="68a32-258">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-258">Return value</span></span>

<span data-ttu-id="68a32-259">Vrátí **True** Pokud první hodnota je menší než nebo rovna hodnotě druhý; jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="68a32-259">Returns **True** if the first value is less than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="68a32-260">Příklad</span><span class="sxs-lookup"><span data-stu-id="68a32-260">Example</span></span>

<span data-ttu-id="68a32-261">Příklad šablony kontroluje, zda jedna hodnota je menší než nebo rovna na druhý.</span><span class="sxs-lookup"><span data-stu-id="68a32-261">The example template checks whether the one value is less than or equal to the other.</span></span>

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

<span data-ttu-id="68a32-262">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="68a32-262">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="68a32-263">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="68a32-263">Name</span></span> | <span data-ttu-id="68a32-264">Typ</span><span class="sxs-lookup"><span data-stu-id="68a32-264">Type</span></span> | <span data-ttu-id="68a32-265">Hodnota</span><span class="sxs-lookup"><span data-stu-id="68a32-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="68a32-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="68a32-266">checkInts</span></span> | <span data-ttu-id="68a32-267">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-267">Bool</span></span> | <span data-ttu-id="68a32-268">True</span><span class="sxs-lookup"><span data-stu-id="68a32-268">True</span></span> |
| <span data-ttu-id="68a32-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="68a32-269">checkStrings</span></span> | <span data-ttu-id="68a32-270">BOOL</span><span class="sxs-lookup"><span data-stu-id="68a32-270">Bool</span></span> | <span data-ttu-id="68a32-271">False</span><span class="sxs-lookup"><span data-stu-id="68a32-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="68a32-272">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68a32-272">Next steps</span></span>
* <span data-ttu-id="68a32-273">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="68a32-273">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="68a32-274">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="68a32-274">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="68a32-275">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="68a32-275">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="68a32-276">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="68a32-276">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

