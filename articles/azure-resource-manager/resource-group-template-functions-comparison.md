---
title: "funkce pro šablony správce prostředků ze aaaAzure - porovnání | Microsoft Docs"
description: "Popisuje funkce toouse hello hodnoty toocompare šablony Azure Resource Manager."
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
ms.openlocfilehash: ebcfc9ed6c93f8b540ec4c066e9457c621800b7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="5fa48-103">Porovnání funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5fa48-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="5fa48-104">Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.</span><span class="sxs-lookup"><span data-stu-id="5fa48-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="5fa48-105">rovná se</span><span class="sxs-lookup"><span data-stu-id="5fa48-105">equals</span></span>](#equals)
* [<span data-ttu-id="5fa48-106">větší</span><span class="sxs-lookup"><span data-stu-id="5fa48-106">greater</span></span>](#greater)
* [<span data-ttu-id="5fa48-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="5fa48-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="5fa48-108">menší</span><span class="sxs-lookup"><span data-stu-id="5fa48-108">less</span></span>](#less)
* [<span data-ttu-id="5fa48-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="5fa48-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="5fa48-110">Rovná se</span><span class="sxs-lookup"><span data-stu-id="5fa48-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="5fa48-111">Kontroluje, zda dvě hodnoty rovny navzájem.</span><span class="sxs-lookup"><span data-stu-id="5fa48-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="5fa48-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="5fa48-112">Parameters</span></span>

| <span data-ttu-id="5fa48-113">Parametr</span><span class="sxs-lookup"><span data-stu-id="5fa48-113">Parameter</span></span> | <span data-ttu-id="5fa48-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5fa48-114">Required</span></span> | <span data-ttu-id="5fa48-115">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-115">Type</span></span> | <span data-ttu-id="5fa48-116">Popis</span><span class="sxs-lookup"><span data-stu-id="5fa48-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5fa48-117">arg1</span><span class="sxs-lookup"><span data-stu-id="5fa48-117">arg1</span></span> |<span data-ttu-id="5fa48-118">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-118">Yes</span></span> |<span data-ttu-id="5fa48-119">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="5fa48-119">int, string, array, or object</span></span> |<span data-ttu-id="5fa48-120">Hello první hodnota toocheck rovnosti.</span><span class="sxs-lookup"><span data-stu-id="5fa48-120">hello first value toocheck for equality.</span></span> |
| <span data-ttu-id="5fa48-121">arg2</span><span class="sxs-lookup"><span data-stu-id="5fa48-121">arg2</span></span> |<span data-ttu-id="5fa48-122">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-122">Yes</span></span> |<span data-ttu-id="5fa48-123">int, string, pole nebo objekt</span><span class="sxs-lookup"><span data-stu-id="5fa48-123">int, string, array, or object</span></span> |<span data-ttu-id="5fa48-124">Hello druhá hodnota toocheck rovnosti.</span><span class="sxs-lookup"><span data-stu-id="5fa48-124">hello second value toocheck for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5fa48-125">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-125">Return value</span></span>

<span data-ttu-id="5fa48-126">Vrátí **True** Pokud hello hodnoty jsou stejné, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-126">Returns **True** if hello values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="5fa48-127">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5fa48-127">Remarks</span></span>

<span data-ttu-id="5fa48-128">Hello funkce rovná se často používá u hello `condition` element tootest tom, jestli je nasazený prostředku.</span><span class="sxs-lookup"><span data-stu-id="5fa48-128">hello equals function is often used with hello `condition` element tootest whether a resource is deployed.</span></span>

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

### <a name="example"></a><span data-ttu-id="5fa48-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="5fa48-129">Example</span></span>

<span data-ttu-id="5fa48-130">Příklad šablony Hello kontroluje různé typy hodnot rovnosti.</span><span class="sxs-lookup"><span data-stu-id="5fa48-130">hello example template checks different types of values for equality.</span></span> <span data-ttu-id="5fa48-131">Všechny hello výchozí hodnoty, vrátí hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="5fa48-131">All hello default values return True.</span></span>

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

<span data-ttu-id="5fa48-132">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-132">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5fa48-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-133">Name</span></span> | <span data-ttu-id="5fa48-134">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-134">Type</span></span> | <span data-ttu-id="5fa48-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="5fa48-136">checkInts</span></span> | <span data-ttu-id="5fa48-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-137">Bool</span></span> | <span data-ttu-id="5fa48-138">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-138">True</span></span> |
| <span data-ttu-id="5fa48-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5fa48-139">checkStrings</span></span> | <span data-ttu-id="5fa48-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-140">Bool</span></span> | <span data-ttu-id="5fa48-141">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-141">True</span></span> |
| <span data-ttu-id="5fa48-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="5fa48-142">checkArrays</span></span> | <span data-ttu-id="5fa48-143">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-143">Bool</span></span> | <span data-ttu-id="5fa48-144">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-144">True</span></span> |
| <span data-ttu-id="5fa48-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="5fa48-145">checkObjects</span></span> | <span data-ttu-id="5fa48-146">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-146">Bool</span></span> | <span data-ttu-id="5fa48-147">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-147">True</span></span> |


<span data-ttu-id="5fa48-148">Hello následující příklad používá [není](resource-group-template-functions-logical.md#not) s **rovná**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-148">hello following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="5fa48-149">výstup Hello z hello předchozím příkladu je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-149">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="5fa48-150">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-150">Name</span></span> | <span data-ttu-id="5fa48-151">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-151">Type</span></span> | <span data-ttu-id="5fa48-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="5fa48-153">checkNotEquals</span></span> | <span data-ttu-id="5fa48-154">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-154">Bool</span></span> | <span data-ttu-id="5fa48-155">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="5fa48-156">větší</span><span class="sxs-lookup"><span data-stu-id="5fa48-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="5fa48-157">Kontroluje, zda text hello, první hodnota je větší než druhá hodnota, která hello.</span><span class="sxs-lookup"><span data-stu-id="5fa48-157">Checks whether hello first value is greater than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5fa48-158">Parametry</span><span class="sxs-lookup"><span data-stu-id="5fa48-158">Parameters</span></span>

| <span data-ttu-id="5fa48-159">Parametr</span><span class="sxs-lookup"><span data-stu-id="5fa48-159">Parameter</span></span> | <span data-ttu-id="5fa48-160">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5fa48-160">Required</span></span> | <span data-ttu-id="5fa48-161">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-161">Type</span></span> | <span data-ttu-id="5fa48-162">Popis</span><span class="sxs-lookup"><span data-stu-id="5fa48-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5fa48-163">arg1</span><span class="sxs-lookup"><span data-stu-id="5fa48-163">arg1</span></span> |<span data-ttu-id="5fa48-164">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-164">Yes</span></span> |<span data-ttu-id="5fa48-165">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-165">int or string</span></span> |<span data-ttu-id="5fa48-166">Hello první hodnotu pro porovnání větší hello.</span><span class="sxs-lookup"><span data-stu-id="5fa48-166">hello first value for hello greater comparison.</span></span> |
| <span data-ttu-id="5fa48-167">arg2</span><span class="sxs-lookup"><span data-stu-id="5fa48-167">arg2</span></span> |<span data-ttu-id="5fa48-168">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-168">Yes</span></span> |<span data-ttu-id="5fa48-169">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-169">int or string</span></span> |<span data-ttu-id="5fa48-170">Druhá hodnota Hello větší porovnání hello.</span><span class="sxs-lookup"><span data-stu-id="5fa48-170">hello second value for hello greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5fa48-171">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-171">Return value</span></span>

<span data-ttu-id="5fa48-172">Vrátí **True** Pokud hello první hodnota je větší než druhá hodnota, která hello; v opačném **False**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-172">Returns **True** if hello first value is greater than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5fa48-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="5fa48-173">Example</span></span>

<span data-ttu-id="5fa48-174">Příklad šablony Hello zkontroluje, zda je větší než hello jiných hello jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5fa48-174">hello example template checks whether hello one value is greater than hello other.</span></span>

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

<span data-ttu-id="5fa48-175">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-175">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5fa48-176">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-176">Name</span></span> | <span data-ttu-id="5fa48-177">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-177">Type</span></span> | <span data-ttu-id="5fa48-178">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="5fa48-179">checkInts</span></span> | <span data-ttu-id="5fa48-180">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-180">Bool</span></span> | <span data-ttu-id="5fa48-181">False</span><span class="sxs-lookup"><span data-stu-id="5fa48-181">False</span></span> |
| <span data-ttu-id="5fa48-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5fa48-182">checkStrings</span></span> | <span data-ttu-id="5fa48-183">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-183">Bool</span></span> | <span data-ttu-id="5fa48-184">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="5fa48-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="5fa48-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="5fa48-186">Kontroluje, zda text hello, první hodnota je větší než nebo rovna toohello druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="5fa48-186">Checks whether hello first value is greater than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5fa48-187">Parametry</span><span class="sxs-lookup"><span data-stu-id="5fa48-187">Parameters</span></span>

| <span data-ttu-id="5fa48-188">Parametr</span><span class="sxs-lookup"><span data-stu-id="5fa48-188">Parameter</span></span> | <span data-ttu-id="5fa48-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5fa48-189">Required</span></span> | <span data-ttu-id="5fa48-190">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-190">Type</span></span> | <span data-ttu-id="5fa48-191">Popis</span><span class="sxs-lookup"><span data-stu-id="5fa48-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5fa48-192">arg1</span><span class="sxs-lookup"><span data-stu-id="5fa48-192">arg1</span></span> |<span data-ttu-id="5fa48-193">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-193">Yes</span></span> |<span data-ttu-id="5fa48-194">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-194">int or string</span></span> |<span data-ttu-id="5fa48-195">Hello první hodnotu pro porovnání větší nebo rovna hello.</span><span class="sxs-lookup"><span data-stu-id="5fa48-195">hello first value for hello greater or equal comparison.</span></span> |
| <span data-ttu-id="5fa48-196">arg2</span><span class="sxs-lookup"><span data-stu-id="5fa48-196">arg2</span></span> |<span data-ttu-id="5fa48-197">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-197">Yes</span></span> |<span data-ttu-id="5fa48-198">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-198">int or string</span></span> |<span data-ttu-id="5fa48-199">Druhá hodnota Hello hello větší nebo rovna porovnání.</span><span class="sxs-lookup"><span data-stu-id="5fa48-199">hello second value for hello greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5fa48-200">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-200">Return value</span></span>

<span data-ttu-id="5fa48-201">Vrátí **True** Pokud hello první hodnota je druhá hodnota větší než nebo rovna toohello; jinak **False**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-201">Returns **True** if hello first value is greater than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5fa48-202">Příklad</span><span class="sxs-lookup"><span data-stu-id="5fa48-202">Example</span></span>

<span data-ttu-id="5fa48-203">Hello příklad šablony kontroluje, zda text hello jedna hodnota je větší než nebo rovna toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="5fa48-203">hello example template checks whether hello one value is greater than or equal toohello other.</span></span>

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

<span data-ttu-id="5fa48-204">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-204">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5fa48-205">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-205">Name</span></span> | <span data-ttu-id="5fa48-206">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-206">Type</span></span> | <span data-ttu-id="5fa48-207">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="5fa48-208">checkInts</span></span> | <span data-ttu-id="5fa48-209">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-209">Bool</span></span> | <span data-ttu-id="5fa48-210">False</span><span class="sxs-lookup"><span data-stu-id="5fa48-210">False</span></span> |
| <span data-ttu-id="5fa48-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5fa48-211">checkStrings</span></span> | <span data-ttu-id="5fa48-212">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-212">Bool</span></span> | <span data-ttu-id="5fa48-213">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="5fa48-214">menší</span><span class="sxs-lookup"><span data-stu-id="5fa48-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="5fa48-215">Kontroluje, zda text hello první hodnota je menší než hello druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="5fa48-215">Checks whether hello first value is less than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5fa48-216">Parametry</span><span class="sxs-lookup"><span data-stu-id="5fa48-216">Parameters</span></span>

| <span data-ttu-id="5fa48-217">Parametr</span><span class="sxs-lookup"><span data-stu-id="5fa48-217">Parameter</span></span> | <span data-ttu-id="5fa48-218">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5fa48-218">Required</span></span> | <span data-ttu-id="5fa48-219">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-219">Type</span></span> | <span data-ttu-id="5fa48-220">Popis</span><span class="sxs-lookup"><span data-stu-id="5fa48-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5fa48-221">arg1</span><span class="sxs-lookup"><span data-stu-id="5fa48-221">arg1</span></span> |<span data-ttu-id="5fa48-222">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-222">Yes</span></span> |<span data-ttu-id="5fa48-223">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-223">int or string</span></span> |<span data-ttu-id="5fa48-224">Hello první hodnota hello méně porovnání.</span><span class="sxs-lookup"><span data-stu-id="5fa48-224">hello first value for hello less comparison.</span></span> |
| <span data-ttu-id="5fa48-225">arg2</span><span class="sxs-lookup"><span data-stu-id="5fa48-225">arg2</span></span> |<span data-ttu-id="5fa48-226">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-226">Yes</span></span> |<span data-ttu-id="5fa48-227">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-227">int or string</span></span> |<span data-ttu-id="5fa48-228">Hello druhá hodnota pro hello méně porovnání.</span><span class="sxs-lookup"><span data-stu-id="5fa48-228">hello second value for hello less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5fa48-229">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-229">Return value</span></span>

<span data-ttu-id="5fa48-230">Vrátí **True** Pokud hello první hodnota je menší než hello druhý hodnotu; jinak **False**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-230">Returns **True** if hello first value is less than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5fa48-231">Příklad</span><span class="sxs-lookup"><span data-stu-id="5fa48-231">Example</span></span>

<span data-ttu-id="5fa48-232">Hello příklad šablony kontroluje, zda hello jedna hodnota je menší než hello jiné.</span><span class="sxs-lookup"><span data-stu-id="5fa48-232">hello example template checks whether hello one value is less than hello other.</span></span>

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

<span data-ttu-id="5fa48-233">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-233">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5fa48-234">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-234">Name</span></span> | <span data-ttu-id="5fa48-235">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-235">Type</span></span> | <span data-ttu-id="5fa48-236">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="5fa48-237">checkInts</span></span> | <span data-ttu-id="5fa48-238">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-238">Bool</span></span> | <span data-ttu-id="5fa48-239">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-239">True</span></span> |
| <span data-ttu-id="5fa48-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5fa48-240">checkStrings</span></span> | <span data-ttu-id="5fa48-241">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-241">Bool</span></span> | <span data-ttu-id="5fa48-242">False</span><span class="sxs-lookup"><span data-stu-id="5fa48-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="5fa48-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="5fa48-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="5fa48-244">Zkontroluje, zda text hello, první hodnota je menší než nebo rovna toohello druhá hodnota.</span><span class="sxs-lookup"><span data-stu-id="5fa48-244">Checks whether hello first value is less than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="5fa48-245">Parametry</span><span class="sxs-lookup"><span data-stu-id="5fa48-245">Parameters</span></span>

| <span data-ttu-id="5fa48-246">Parametr</span><span class="sxs-lookup"><span data-stu-id="5fa48-246">Parameter</span></span> | <span data-ttu-id="5fa48-247">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5fa48-247">Required</span></span> | <span data-ttu-id="5fa48-248">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-248">Type</span></span> | <span data-ttu-id="5fa48-249">Popis</span><span class="sxs-lookup"><span data-stu-id="5fa48-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5fa48-250">arg1</span><span class="sxs-lookup"><span data-stu-id="5fa48-250">arg1</span></span> |<span data-ttu-id="5fa48-251">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-251">Yes</span></span> |<span data-ttu-id="5fa48-252">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-252">int or string</span></span> |<span data-ttu-id="5fa48-253">první hodnota Hello hello menší nebo rovná porovnání.</span><span class="sxs-lookup"><span data-stu-id="5fa48-253">hello first value for hello less or equals comparison.</span></span> |
| <span data-ttu-id="5fa48-254">arg2</span><span class="sxs-lookup"><span data-stu-id="5fa48-254">arg2</span></span> |<span data-ttu-id="5fa48-255">Ano</span><span class="sxs-lookup"><span data-stu-id="5fa48-255">Yes</span></span> |<span data-ttu-id="5fa48-256">int nebo string</span><span class="sxs-lookup"><span data-stu-id="5fa48-256">int or string</span></span> |<span data-ttu-id="5fa48-257">Druhá hodnota, která pro hello Hello menší nebo rovno porovnání.</span><span class="sxs-lookup"><span data-stu-id="5fa48-257">hello second value for hello less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="5fa48-258">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-258">Return value</span></span>

<span data-ttu-id="5fa48-259">Vrátí **True** Pokud hello první hodnota je menší než nebo rovna toohello druhá hodnota, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="5fa48-259">Returns **True** if hello first value is less than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="5fa48-260">Příklad</span><span class="sxs-lookup"><span data-stu-id="5fa48-260">Example</span></span>

<span data-ttu-id="5fa48-261">Hello příklad šablony zkontroluje, zda hello jedna hodnota je menší než nebo rovna toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="5fa48-261">hello example template checks whether hello one value is less than or equal toohello other.</span></span>

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

<span data-ttu-id="5fa48-262">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="5fa48-262">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="5fa48-263">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5fa48-263">Name</span></span> | <span data-ttu-id="5fa48-264">Typ</span><span class="sxs-lookup"><span data-stu-id="5fa48-264">Type</span></span> | <span data-ttu-id="5fa48-265">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5fa48-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="5fa48-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="5fa48-266">checkInts</span></span> | <span data-ttu-id="5fa48-267">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-267">Bool</span></span> | <span data-ttu-id="5fa48-268">True</span><span class="sxs-lookup"><span data-stu-id="5fa48-268">True</span></span> |
| <span data-ttu-id="5fa48-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="5fa48-269">checkStrings</span></span> | <span data-ttu-id="5fa48-270">BOOL</span><span class="sxs-lookup"><span data-stu-id="5fa48-270">Bool</span></span> | <span data-ttu-id="5fa48-271">False</span><span class="sxs-lookup"><span data-stu-id="5fa48-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="5fa48-272">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5fa48-272">Next steps</span></span>
* <span data-ttu-id="5fa48-273">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5fa48-273">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="5fa48-274">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5fa48-274">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="5fa48-275">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="5fa48-275">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="5fa48-276">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5fa48-276">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

