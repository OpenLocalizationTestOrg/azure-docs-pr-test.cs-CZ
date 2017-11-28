---
title: "Řetězec funkce šablon Azure Resource Manager - | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manageru pro práci s řetězce."
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
ms.openlocfilehash: 3e5c9ca546629f782a3d722b49f5fbaf5147e823
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="b7851-103">Řetězcové funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b7851-103">String functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="b7851-104">Resource Manager poskytuje následující funkce pro práci s řetězce:</span><span class="sxs-lookup"><span data-stu-id="b7851-104">Resource Manager provides the following functions for working with strings:</span></span>

* [<span data-ttu-id="b7851-105">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-105">base64</span></span>](#base64)
* [<span data-ttu-id="b7851-106">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="b7851-106">base64ToJson</span></span>](#base64tojson)
* [<span data-ttu-id="b7851-107">base64ToString</span><span class="sxs-lookup"><span data-stu-id="b7851-107">base64ToString</span></span>](#base64tostring)
* [<span data-ttu-id="b7851-108">concat</span><span class="sxs-lookup"><span data-stu-id="b7851-108">concat</span></span>](#concat)
* [<span data-ttu-id="b7851-109">obsahuje</span><span class="sxs-lookup"><span data-stu-id="b7851-109">contains</span></span>](#contains)
* [<span data-ttu-id="b7851-110">dataUri</span><span class="sxs-lookup"><span data-stu-id="b7851-110">dataUri</span></span>](#datauri)
* [<span data-ttu-id="b7851-111">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="b7851-111">dataUriToString</span></span>](#datauritostring)
* [<span data-ttu-id="b7851-112">prázdný</span><span class="sxs-lookup"><span data-stu-id="b7851-112">empty</span></span>](#empty)
* [<span data-ttu-id="b7851-113">endsWith</span><span class="sxs-lookup"><span data-stu-id="b7851-113">endsWith</span></span>](#endswith)
* [<span data-ttu-id="b7851-114">první</span><span class="sxs-lookup"><span data-stu-id="b7851-114">first</span></span>](#first)
* [<span data-ttu-id="b7851-115">indexOf</span><span class="sxs-lookup"><span data-stu-id="b7851-115">indexOf</span></span>](#indexof)
* [<span data-ttu-id="b7851-116">poslední</span><span class="sxs-lookup"><span data-stu-id="b7851-116">last</span></span>](#last)
* [<span data-ttu-id="b7851-117">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="b7851-117">lastIndexOf</span></span>](#lastindexof)
* [<span data-ttu-id="b7851-118">Délka</span><span class="sxs-lookup"><span data-stu-id="b7851-118">length</span></span>](#length)
* [<span data-ttu-id="b7851-119">padLeft</span><span class="sxs-lookup"><span data-stu-id="b7851-119">padLeft</span></span>](#padleft)
* [<span data-ttu-id="b7851-120">nahradit</span><span class="sxs-lookup"><span data-stu-id="b7851-120">replace</span></span>](#replace)
* [<span data-ttu-id="b7851-121">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="b7851-121">skip</span></span>](#skip)
* [<span data-ttu-id="b7851-122">split</span><span class="sxs-lookup"><span data-stu-id="b7851-122">split</span></span>](#split)
* [<span data-ttu-id="b7851-123">startsWith</span><span class="sxs-lookup"><span data-stu-id="b7851-123">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="b7851-124">řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-124">string</span></span>](#string)
* [<span data-ttu-id="b7851-125">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-125">substring</span></span>](#substring)
* [<span data-ttu-id="b7851-126">proveďte</span><span class="sxs-lookup"><span data-stu-id="b7851-126">take</span></span>](#take)
* [<span data-ttu-id="b7851-127">toLower</span><span class="sxs-lookup"><span data-stu-id="b7851-127">toLower</span></span>](#tolower)
* [<span data-ttu-id="b7851-128">toUpper</span><span class="sxs-lookup"><span data-stu-id="b7851-128">toUpper</span></span>](#toupper)
* [<span data-ttu-id="b7851-129">uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="b7851-129">trim</span></span>](#trim)
* [<span data-ttu-id="b7851-130">uniqueString</span><span class="sxs-lookup"><span data-stu-id="b7851-130">uniqueString</span></span>](#uniquestring)
* [<span data-ttu-id="b7851-131">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="b7851-131">uri</span></span>](#uri)
* [<span data-ttu-id="b7851-132">uriComponent</span><span class="sxs-lookup"><span data-stu-id="b7851-132">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="b7851-133">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="b7851-133">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a><span data-ttu-id="b7851-134">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-134">base64</span></span>
`base64(inputString)`

<span data-ttu-id="b7851-135">Vrátí reprezentaci base64 vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-135">Returns the base64 representation of the input string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-136">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-136">Parameters</span></span>

| <span data-ttu-id="b7851-137">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-137">Parameter</span></span> | <span data-ttu-id="b7851-138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-138">Required</span></span> | <span data-ttu-id="b7851-139">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-139">Type</span></span> | <span data-ttu-id="b7851-140">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-140">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-141">inputString</span><span class="sxs-lookup"><span data-stu-id="b7851-141">inputString</span></span> |<span data-ttu-id="b7851-142">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-142">Yes</span></span> |<span data-ttu-id="b7851-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-143">string</span></span> |<span data-ttu-id="b7851-144">Hodnota k vrátit jako znázornění base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-144">The value to return as a base64 representation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-145">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-145">Return value</span></span>

<span data-ttu-id="b7851-146">Řetězec obsahující reprezentace base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-146">A string containing the base64 representation.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-147">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-147">Examples</span></span>

<span data-ttu-id="b7851-148">Následující příklad ukazuje, jak chcete používat funkci base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-148">The following example shows how to use the base64 function.</span></span>

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

<span data-ttu-id="b7851-149">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-149">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-150">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-150">Name</span></span> | <span data-ttu-id="b7851-151">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-151">Type</span></span> | <span data-ttu-id="b7851-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-153">base64Output</span><span class="sxs-lookup"><span data-stu-id="b7851-153">base64Output</span></span> | <span data-ttu-id="b7851-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-154">String</span></span> | <span data-ttu-id="b7851-155">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b7851-155">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b7851-156">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-156">toStringOutput</span></span> | <span data-ttu-id="b7851-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-157">String</span></span> | <span data-ttu-id="b7851-158">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-158">one, two, three</span></span> |
| <span data-ttu-id="b7851-159">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-159">toJsonOutput</span></span> | <span data-ttu-id="b7851-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="b7851-160">Object</span></span> | <span data-ttu-id="b7851-161">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b7851-161">{"one": "a", "two": "b"}</span></span> |

<a id="base64tojson" />

## <a name="base64tojson"></a><span data-ttu-id="b7851-162">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="b7851-162">base64ToJson</span></span>
`base64tojson`

<span data-ttu-id="b7851-163">Převede znázornění base64 objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="b7851-163">Converts a base64 representation to a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-164">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-164">Parameters</span></span>

| <span data-ttu-id="b7851-165">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-165">Parameter</span></span> | <span data-ttu-id="b7851-166">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-166">Required</span></span> | <span data-ttu-id="b7851-167">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-167">Type</span></span> | <span data-ttu-id="b7851-168">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-168">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-169">base64Value</span><span class="sxs-lookup"><span data-stu-id="b7851-169">base64Value</span></span> |<span data-ttu-id="b7851-170">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-170">Yes</span></span> |<span data-ttu-id="b7851-171">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-171">string</span></span> |<span data-ttu-id="b7851-172">Reprezentace base64 převést na objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="b7851-172">The base64 representation to convert to a JSON object.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-173">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-173">Return value</span></span>

<span data-ttu-id="b7851-174">Objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="b7851-174">A JSON object.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-175">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-175">Examples</span></span>

<span data-ttu-id="b7851-176">Následující příklad používá funkci base64ToJson převést hodnotu base64:</span><span class="sxs-lookup"><span data-stu-id="b7851-176">The following example uses the base64ToJson function to convert a base64 value:</span></span>

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

<span data-ttu-id="b7851-177">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-177">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-178">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-178">Name</span></span> | <span data-ttu-id="b7851-179">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-179">Type</span></span> | <span data-ttu-id="b7851-180">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-180">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-181">base64Output</span><span class="sxs-lookup"><span data-stu-id="b7851-181">base64Output</span></span> | <span data-ttu-id="b7851-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-182">String</span></span> | <span data-ttu-id="b7851-183">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b7851-183">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b7851-184">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-184">toStringOutput</span></span> | <span data-ttu-id="b7851-185">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-185">String</span></span> | <span data-ttu-id="b7851-186">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-186">one, two, three</span></span> |
| <span data-ttu-id="b7851-187">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-187">toJsonOutput</span></span> | <span data-ttu-id="b7851-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="b7851-188">Object</span></span> | <span data-ttu-id="b7851-189">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b7851-189">{"one": "a", "two": "b"}</span></span> |

<a id="base64tostring" />

## <a name="base64tostring"></a><span data-ttu-id="b7851-190">base64ToString</span><span class="sxs-lookup"><span data-stu-id="b7851-190">base64ToString</span></span>
`base64ToString(base64Value)`

<span data-ttu-id="b7851-191">Převede řetězec znázornění base64.</span><span class="sxs-lookup"><span data-stu-id="b7851-191">Converts a base64 representation to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-192">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-192">Parameters</span></span>

| <span data-ttu-id="b7851-193">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-193">Parameter</span></span> | <span data-ttu-id="b7851-194">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-194">Required</span></span> | <span data-ttu-id="b7851-195">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-195">Type</span></span> | <span data-ttu-id="b7851-196">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-196">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-197">base64Value</span><span class="sxs-lookup"><span data-stu-id="b7851-197">base64Value</span></span> |<span data-ttu-id="b7851-198">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-198">Yes</span></span> |<span data-ttu-id="b7851-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-199">string</span></span> |<span data-ttu-id="b7851-200">Reprezentace base64 převést na řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-200">The base64 representation to convert to a string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-201">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-201">Return value</span></span>

<span data-ttu-id="b7851-202">Řetězec převedený base64 hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b7851-202">A string of the converted base64 value.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-203">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-203">Examples</span></span>

<span data-ttu-id="b7851-204">Následující příklad používá funkci base64ToString převést hodnotu base64:</span><span class="sxs-lookup"><span data-stu-id="b7851-204">The following example uses the base64ToString function to convert a base64 value:</span></span>

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

<span data-ttu-id="b7851-205">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-205">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-206">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-206">Name</span></span> | <span data-ttu-id="b7851-207">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-207">Type</span></span> | <span data-ttu-id="b7851-208">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-208">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-209">base64Output</span><span class="sxs-lookup"><span data-stu-id="b7851-209">base64Output</span></span> | <span data-ttu-id="b7851-210">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-210">String</span></span> | <span data-ttu-id="b7851-211">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b7851-211">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b7851-212">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-212">toStringOutput</span></span> | <span data-ttu-id="b7851-213">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-213">String</span></span> | <span data-ttu-id="b7851-214">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-214">one, two, three</span></span> |
| <span data-ttu-id="b7851-215">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-215">toJsonOutput</span></span> | <span data-ttu-id="b7851-216">Objekt</span><span class="sxs-lookup"><span data-stu-id="b7851-216">Object</span></span> | <span data-ttu-id="b7851-217">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b7851-217">{"one": "a", "two": "b"}</span></span> |



<a id="concat" />

## <a name="concat"></a><span data-ttu-id="b7851-218">concat</span><span class="sxs-lookup"><span data-stu-id="b7851-218">concat</span></span>
`concat (arg1, arg2, arg3, ...)`

<span data-ttu-id="b7851-219">Kombinuje více řetězcové hodnoty a vrací spojený řetězec nebo kombinuje několik polí a vrací zřetězených pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-219">Combines multiple string values and returns the concatenated string, or combines multiple arrays and returns the concatenated array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-220">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-220">Parameters</span></span>

| <span data-ttu-id="b7851-221">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-221">Parameter</span></span> | <span data-ttu-id="b7851-222">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-222">Required</span></span> | <span data-ttu-id="b7851-223">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-223">Type</span></span> | <span data-ttu-id="b7851-224">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-224">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-225">arg1</span><span class="sxs-lookup"><span data-stu-id="b7851-225">arg1</span></span> |<span data-ttu-id="b7851-226">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-226">Yes</span></span> |<span data-ttu-id="b7851-227">řetězec nebo pole</span><span class="sxs-lookup"><span data-stu-id="b7851-227">string or array</span></span> |<span data-ttu-id="b7851-228">První hodnota zřetězení.</span><span class="sxs-lookup"><span data-stu-id="b7851-228">The first value for concatenation.</span></span> |
| <span data-ttu-id="b7851-229">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="b7851-229">additional arguments</span></span> |<span data-ttu-id="b7851-230">Ne</span><span class="sxs-lookup"><span data-stu-id="b7851-230">No</span></span> |<span data-ttu-id="b7851-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-231">string</span></span> |<span data-ttu-id="b7851-232">Další hodnoty v sekvenčním pořadí pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="b7851-232">Additional values in sequential order for concatenation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-233">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-233">Return value</span></span>
<span data-ttu-id="b7851-234">Řetězec nebo pole zřetězených hodnot.</span><span class="sxs-lookup"><span data-stu-id="b7851-234">A string or array of concatenated values.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-235">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-235">Examples</span></span>

<span data-ttu-id="b7851-236">Následující příklad ukazuje, jak kombinovat dvou řetězcových hodnot a vrátí spojený řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-236">The following example shows how to combine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="b7851-237">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-237">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-238">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-238">Name</span></span> | <span data-ttu-id="b7851-239">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-239">Type</span></span> | <span data-ttu-id="b7851-240">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-240">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-241">concatOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-241">concatOutput</span></span> | <span data-ttu-id="b7851-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-242">String</span></span> | <span data-ttu-id="b7851-243">Předpona 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="b7851-243">prefix-5yj4yjf5mbg72</span></span> |

<span data-ttu-id="b7851-244">Následující příklad ukazuje, jak kombinovat dvěma poli.</span><span class="sxs-lookup"><span data-stu-id="b7851-244">The following example shows how to combine two arrays.</span></span>

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

<span data-ttu-id="b7851-245">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-245">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-246">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-246">Name</span></span> | <span data-ttu-id="b7851-247">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-247">Type</span></span> | <span data-ttu-id="b7851-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-249">Vrátí</span><span class="sxs-lookup"><span data-stu-id="b7851-249">return</span></span> | <span data-ttu-id="b7851-250">Pole</span><span class="sxs-lookup"><span data-stu-id="b7851-250">Array</span></span> | <span data-ttu-id="b7851-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="b7851-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="b7851-252">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="b7851-252">contains</span></span>
`contains (container, itemToFind)`

<span data-ttu-id="b7851-253">Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-253">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-254">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-254">Parameters</span></span>

| <span data-ttu-id="b7851-255">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-255">Parameter</span></span> | <span data-ttu-id="b7851-256">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-256">Required</span></span> | <span data-ttu-id="b7851-257">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-257">Type</span></span> | <span data-ttu-id="b7851-258">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-258">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-259">kontejner</span><span class="sxs-lookup"><span data-stu-id="b7851-259">container</span></span> |<span data-ttu-id="b7851-260">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-260">Yes</span></span> |<span data-ttu-id="b7851-261">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-261">array, object, or string</span></span> |<span data-ttu-id="b7851-262">Hodnota, která obsahuje hodnotu k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-262">The value that contains the value to find.</span></span> |
| <span data-ttu-id="b7851-263">itemToFind</span><span class="sxs-lookup"><span data-stu-id="b7851-263">itemToFind</span></span> |<span data-ttu-id="b7851-264">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-264">Yes</span></span> |<span data-ttu-id="b7851-265">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-265">string or int</span></span> |<span data-ttu-id="b7851-266">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-266">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-267">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-267">Return value</span></span>

<span data-ttu-id="b7851-268">**Hodnota TRUE,** Pokud je položka, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="b7851-268">**True** if the item is found; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-269">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-269">Examples</span></span>

<span data-ttu-id="b7851-270">Následující příklad ukazuje, jak používat s různými typy obsahuje:</span><span class="sxs-lookup"><span data-stu-id="b7851-270">The following example shows how to use contains with different types:</span></span>

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

<span data-ttu-id="b7851-271">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-271">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-272">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-272">Name</span></span> | <span data-ttu-id="b7851-273">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-273">Type</span></span> | <span data-ttu-id="b7851-274">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-274">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-275">stringTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-275">stringTrue</span></span> | <span data-ttu-id="b7851-276">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-276">Bool</span></span> | <span data-ttu-id="b7851-277">True</span><span class="sxs-lookup"><span data-stu-id="b7851-277">True</span></span> |
| <span data-ttu-id="b7851-278">stringFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-278">stringFalse</span></span> | <span data-ttu-id="b7851-279">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-279">Bool</span></span> | <span data-ttu-id="b7851-280">False</span><span class="sxs-lookup"><span data-stu-id="b7851-280">False</span></span> |
| <span data-ttu-id="b7851-281">objectTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-281">objectTrue</span></span> | <span data-ttu-id="b7851-282">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-282">Bool</span></span> | <span data-ttu-id="b7851-283">True</span><span class="sxs-lookup"><span data-stu-id="b7851-283">True</span></span> |
| <span data-ttu-id="b7851-284">objectFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-284">objectFalse</span></span> | <span data-ttu-id="b7851-285">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-285">Bool</span></span> | <span data-ttu-id="b7851-286">False</span><span class="sxs-lookup"><span data-stu-id="b7851-286">False</span></span> |
| <span data-ttu-id="b7851-287">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-287">arrayTrue</span></span> | <span data-ttu-id="b7851-288">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-288">Bool</span></span> | <span data-ttu-id="b7851-289">True</span><span class="sxs-lookup"><span data-stu-id="b7851-289">True</span></span> |
| <span data-ttu-id="b7851-290">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-290">arrayFalse</span></span> | <span data-ttu-id="b7851-291">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-291">Bool</span></span> | <span data-ttu-id="b7851-292">False</span><span class="sxs-lookup"><span data-stu-id="b7851-292">False</span></span> |

<a id="datauri" />

## <a name="datauri"></a><span data-ttu-id="b7851-293">dataUri</span><span class="sxs-lookup"><span data-stu-id="b7851-293">dataUri</span></span>
`dataUri(stringToConvert)`

<span data-ttu-id="b7851-294">Převede hodnotu na datový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b7851-294">Converts a value to a data URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-295">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-295">Parameters</span></span>

| <span data-ttu-id="b7851-296">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-296">Parameter</span></span> | <span data-ttu-id="b7851-297">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-297">Required</span></span> | <span data-ttu-id="b7851-298">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-298">Type</span></span> | <span data-ttu-id="b7851-299">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-299">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-300">stringToConvert</span><span class="sxs-lookup"><span data-stu-id="b7851-300">stringToConvert</span></span> |<span data-ttu-id="b7851-301">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-301">Yes</span></span> |<span data-ttu-id="b7851-302">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-302">string</span></span> |<span data-ttu-id="b7851-303">Hodnota převést na identifikátor URI dat.</span><span class="sxs-lookup"><span data-stu-id="b7851-303">The value to convert to a data URI.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-304">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-304">Return value</span></span>

<span data-ttu-id="b7851-305">Řetězec formátu dat identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b7851-305">A string formatted as a data URI.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-306">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-306">Examples</span></span>

<span data-ttu-id="b7851-307">Následující příklad převede hodnotu na identifikátor URI dat. a převede data URI na řetězec:</span><span class="sxs-lookup"><span data-stu-id="b7851-307">The following example converts a value to a data URI, and converts a data URI to a string:</span></span>

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

<span data-ttu-id="b7851-308">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-308">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-309">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-309">Name</span></span> | <span data-ttu-id="b7851-310">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-310">Type</span></span> | <span data-ttu-id="b7851-311">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-311">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-312">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-312">dataUriOutput</span></span> | <span data-ttu-id="b7851-313">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-313">String</span></span> | <span data-ttu-id="b7851-314">data: text / prostý; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="b7851-314">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="b7851-315">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-315">toStringOutput</span></span> | <span data-ttu-id="b7851-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-316">String</span></span> | <span data-ttu-id="b7851-317">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="b7851-317">Hello, World!</span></span> |

<a id="datauritostring" />

## <a name="datauritostring"></a><span data-ttu-id="b7851-318">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="b7851-318">dataUriToString</span></span>
`dataUriToString(dataUriToConvert)`

<span data-ttu-id="b7851-319">Převádí data URI ve formátu hodnotu na řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-319">Converts a data URI formatted value to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-320">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-320">Parameters</span></span>

| <span data-ttu-id="b7851-321">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-321">Parameter</span></span> | <span data-ttu-id="b7851-322">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-322">Required</span></span> | <span data-ttu-id="b7851-323">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-323">Type</span></span> | <span data-ttu-id="b7851-324">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-324">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-325">dataUriToConvert</span><span class="sxs-lookup"><span data-stu-id="b7851-325">dataUriToConvert</span></span> |<span data-ttu-id="b7851-326">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-326">Yes</span></span> |<span data-ttu-id="b7851-327">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-327">string</span></span> |<span data-ttu-id="b7851-328">Data, která hodnota identifikátoru URI k převedení.</span><span class="sxs-lookup"><span data-stu-id="b7851-328">The data URI value to convert.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-329">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-329">Return value</span></span>

<span data-ttu-id="b7851-330">Řetězec obsahující převedenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b7851-330">A string containing the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-331">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-331">Examples</span></span>

<span data-ttu-id="b7851-332">Následující příklad převede hodnotu na identifikátor URI dat. a převede data URI na řetězec:</span><span class="sxs-lookup"><span data-stu-id="b7851-332">The following example converts a value to a data URI, and converts a data URI to a string:</span></span>

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

<span data-ttu-id="b7851-333">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-333">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-334">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-334">Name</span></span> | <span data-ttu-id="b7851-335">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-335">Type</span></span> | <span data-ttu-id="b7851-336">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-336">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-337">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-337">dataUriOutput</span></span> | <span data-ttu-id="b7851-338">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-338">String</span></span> | <span data-ttu-id="b7851-339">data: text / prostý; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="b7851-339">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="b7851-340">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-340">toStringOutput</span></span> | <span data-ttu-id="b7851-341">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-341">String</span></span> | <span data-ttu-id="b7851-342">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="b7851-342">Hello, World!</span></span> |

<a id="empty" /> 

## <a name="empty"></a><span data-ttu-id="b7851-343">prázdný</span><span class="sxs-lookup"><span data-stu-id="b7851-343">empty</span></span>
`empty(itemToTest)`

<span data-ttu-id="b7851-344">Určuje, zda je prázdný řetězec, objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-344">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-345">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-345">Parameters</span></span>

| <span data-ttu-id="b7851-346">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-346">Parameter</span></span> | <span data-ttu-id="b7851-347">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-347">Required</span></span> | <span data-ttu-id="b7851-348">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-348">Type</span></span> | <span data-ttu-id="b7851-349">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-349">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-350">itemToTest</span><span class="sxs-lookup"><span data-stu-id="b7851-350">itemToTest</span></span> |<span data-ttu-id="b7851-351">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-351">Yes</span></span> |<span data-ttu-id="b7851-352">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-352">array, object, or string</span></span> |<span data-ttu-id="b7851-353">Hodnota ke kontrole, jestli je prázdný.</span><span class="sxs-lookup"><span data-stu-id="b7851-353">The value to check if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-354">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-354">Return value</span></span>

<span data-ttu-id="b7851-355">Vrátí **True** Pokud hodnota je prázdný, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="b7851-355">Returns **True** if the value is empty; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-356">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-356">Examples</span></span>

<span data-ttu-id="b7851-357">Následující příklad ověří, zda jsou řetězec, objekt a pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="b7851-357">The following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="b7851-358">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-358">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-359">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-359">Name</span></span> | <span data-ttu-id="b7851-360">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-360">Type</span></span> | <span data-ttu-id="b7851-361">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-361">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-362">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="b7851-362">arrayEmpty</span></span> | <span data-ttu-id="b7851-363">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-363">Bool</span></span> | <span data-ttu-id="b7851-364">True</span><span class="sxs-lookup"><span data-stu-id="b7851-364">True</span></span> |
| <span data-ttu-id="b7851-365">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="b7851-365">objectEmpty</span></span> | <span data-ttu-id="b7851-366">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-366">Bool</span></span> | <span data-ttu-id="b7851-367">True</span><span class="sxs-lookup"><span data-stu-id="b7851-367">True</span></span> |
| <span data-ttu-id="b7851-368">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="b7851-368">stringEmpty</span></span> | <span data-ttu-id="b7851-369">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-369">Bool</span></span> | <span data-ttu-id="b7851-370">True</span><span class="sxs-lookup"><span data-stu-id="b7851-370">True</span></span> |

<a id="endswith" />

## <a name="endswith"></a><span data-ttu-id="b7851-371">endsWith</span><span class="sxs-lookup"><span data-stu-id="b7851-371">endsWith</span></span>
`endsWith(stringToSearch, stringToFind)`

<span data-ttu-id="b7851-372">Určuje, zda řetězec končí s hodnotou.</span><span class="sxs-lookup"><span data-stu-id="b7851-372">Determines whether a string ends with a value.</span></span> <span data-ttu-id="b7851-373">Porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-373">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-374">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-374">Parameters</span></span>

| <span data-ttu-id="b7851-375">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-375">Parameter</span></span> | <span data-ttu-id="b7851-376">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-376">Required</span></span> | <span data-ttu-id="b7851-377">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-377">Type</span></span> | <span data-ttu-id="b7851-378">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-378">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-379">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b7851-379">stringToSearch</span></span> |<span data-ttu-id="b7851-380">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-380">Yes</span></span> |<span data-ttu-id="b7851-381">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-381">string</span></span> |<span data-ttu-id="b7851-382">Hodnota, která obsahuje položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-382">The value that contains the item to find.</span></span> |
| <span data-ttu-id="b7851-383">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b7851-383">stringToFind</span></span> |<span data-ttu-id="b7851-384">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-384">Yes</span></span> |<span data-ttu-id="b7851-385">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-385">string</span></span> |<span data-ttu-id="b7851-386">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-386">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-387">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-387">Return value</span></span>

<span data-ttu-id="b7851-388">**Hodnota TRUE,** Pokud poslední znak nebo znaky řetězce odpovídají hodnotě; jinak **False**.</span><span class="sxs-lookup"><span data-stu-id="b7851-388">**True** if the last character or characters of the string match the value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-389">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-389">Examples</span></span>

<span data-ttu-id="b7851-390">Následující příklad ukazuje, jak používat funkce startsWith a endsWith:</span><span class="sxs-lookup"><span data-stu-id="b7851-390">The following example shows how to use the startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="b7851-391">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-391">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-392">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-392">Name</span></span> | <span data-ttu-id="b7851-393">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-393">Type</span></span> | <span data-ttu-id="b7851-394">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-394">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-395">startsTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-395">startsTrue</span></span> | <span data-ttu-id="b7851-396">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-396">Bool</span></span> | <span data-ttu-id="b7851-397">True</span><span class="sxs-lookup"><span data-stu-id="b7851-397">True</span></span> |
| <span data-ttu-id="b7851-398">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-398">startsCapTrue</span></span> | <span data-ttu-id="b7851-399">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-399">Bool</span></span> | <span data-ttu-id="b7851-400">True</span><span class="sxs-lookup"><span data-stu-id="b7851-400">True</span></span> |
| <span data-ttu-id="b7851-401">startsFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-401">startsFalse</span></span> | <span data-ttu-id="b7851-402">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-402">Bool</span></span> | <span data-ttu-id="b7851-403">False</span><span class="sxs-lookup"><span data-stu-id="b7851-403">False</span></span> |
| <span data-ttu-id="b7851-404">endsTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-404">endsTrue</span></span> | <span data-ttu-id="b7851-405">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-405">Bool</span></span> | <span data-ttu-id="b7851-406">True</span><span class="sxs-lookup"><span data-stu-id="b7851-406">True</span></span> |
| <span data-ttu-id="b7851-407">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-407">endsCapTrue</span></span> | <span data-ttu-id="b7851-408">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-408">Bool</span></span> | <span data-ttu-id="b7851-409">True</span><span class="sxs-lookup"><span data-stu-id="b7851-409">True</span></span> |
| <span data-ttu-id="b7851-410">endsFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-410">endsFalse</span></span> | <span data-ttu-id="b7851-411">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-411">Bool</span></span> | <span data-ttu-id="b7851-412">False</span><span class="sxs-lookup"><span data-stu-id="b7851-412">False</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="b7851-413">první</span><span class="sxs-lookup"><span data-stu-id="b7851-413">first</span></span>
`first(arg1)`

<span data-ttu-id="b7851-414">Vrátí první znak řetězec, nebo první prvek pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-414">Returns the first character of the string, or first element of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-415">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-415">Parameters</span></span>

| <span data-ttu-id="b7851-416">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-416">Parameter</span></span> | <span data-ttu-id="b7851-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-417">Required</span></span> | <span data-ttu-id="b7851-418">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-418">Type</span></span> | <span data-ttu-id="b7851-419">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-420">arg1</span><span class="sxs-lookup"><span data-stu-id="b7851-420">arg1</span></span> |<span data-ttu-id="b7851-421">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-421">Yes</span></span> |<span data-ttu-id="b7851-422">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-422">array or string</span></span> |<span data-ttu-id="b7851-423">Hodnota k načtení první element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="b7851-423">The value to retrieve the first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-424">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-424">Return value</span></span>

<span data-ttu-id="b7851-425">Řetězec prvního znaku nebo typ první prvek v poli (řetězec, int, pole nebo objekt).</span><span class="sxs-lookup"><span data-stu-id="b7851-425">A string of the first character, or the type (string, int, array, or object) of the first element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-426">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-426">Examples</span></span>

<span data-ttu-id="b7851-427">Následující příklad ukazuje, jak používat funkci první s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-427">The following example shows how to use the first function with an array and string.</span></span>

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

<span data-ttu-id="b7851-428">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-428">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-429">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-429">Name</span></span> | <span data-ttu-id="b7851-430">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-430">Type</span></span> | <span data-ttu-id="b7851-431">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-432">arrayOutput</span></span> | <span data-ttu-id="b7851-433">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-433">String</span></span> | <span data-ttu-id="b7851-434">jeden</span><span class="sxs-lookup"><span data-stu-id="b7851-434">one</span></span> |
| <span data-ttu-id="b7851-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-435">stringOutput</span></span> | <span data-ttu-id="b7851-436">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-436">String</span></span> | <span data-ttu-id="b7851-437">O</span><span class="sxs-lookup"><span data-stu-id="b7851-437">O</span></span> |

<a id="indexof" />

## <a name="indexof"></a><span data-ttu-id="b7851-438">indexOf</span><span class="sxs-lookup"><span data-stu-id="b7851-438">indexOf</span></span>
`indexOf(stringToSearch, stringToFind)`

<span data-ttu-id="b7851-439">Vrátí první pozici hodnoty v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b7851-439">Returns the first position of a value within a string.</span></span> <span data-ttu-id="b7851-440">Porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-440">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-441">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-441">Parameters</span></span>

| <span data-ttu-id="b7851-442">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-442">Parameter</span></span> | <span data-ttu-id="b7851-443">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-443">Required</span></span> | <span data-ttu-id="b7851-444">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-444">Type</span></span> | <span data-ttu-id="b7851-445">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-445">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-446">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b7851-446">stringToSearch</span></span> |<span data-ttu-id="b7851-447">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-447">Yes</span></span> |<span data-ttu-id="b7851-448">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-448">string</span></span> |<span data-ttu-id="b7851-449">Hodnota, která obsahuje položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-449">The value that contains the item to find.</span></span> |
| <span data-ttu-id="b7851-450">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b7851-450">stringToFind</span></span> |<span data-ttu-id="b7851-451">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-451">Yes</span></span> |<span data-ttu-id="b7851-452">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-452">string</span></span> |<span data-ttu-id="b7851-453">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-453">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-454">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-454">Return value</span></span>

<span data-ttu-id="b7851-455">Celé číslo představující pozici položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-455">An integer that represents the position of the item to find.</span></span> <span data-ttu-id="b7851-456">Hodnota je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="b7851-456">The value is zero-based.</span></span> <span data-ttu-id="b7851-457">Pokud položka není nalezena, vrátí se -1.</span><span class="sxs-lookup"><span data-stu-id="b7851-457">If the item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-458">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-458">Examples</span></span>

<span data-ttu-id="b7851-459">Následující příklad ukazuje, jak používat funkce indexOf a lastIndexOf:</span><span class="sxs-lookup"><span data-stu-id="b7851-459">The following example shows how to use the indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="b7851-460">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-460">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-461">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-461">Name</span></span> | <span data-ttu-id="b7851-462">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-462">Type</span></span> | <span data-ttu-id="b7851-463">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-463">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-464">firstT</span><span class="sxs-lookup"><span data-stu-id="b7851-464">firstT</span></span> | <span data-ttu-id="b7851-465">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-465">Int</span></span> | <span data-ttu-id="b7851-466">0</span><span class="sxs-lookup"><span data-stu-id="b7851-466">0</span></span> |
| <span data-ttu-id="b7851-467">lastT</span><span class="sxs-lookup"><span data-stu-id="b7851-467">lastT</span></span> | <span data-ttu-id="b7851-468">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-468">Int</span></span> | <span data-ttu-id="b7851-469">3</span><span class="sxs-lookup"><span data-stu-id="b7851-469">3</span></span> |
| <span data-ttu-id="b7851-470">firstString</span><span class="sxs-lookup"><span data-stu-id="b7851-470">firstString</span></span> | <span data-ttu-id="b7851-471">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-471">Int</span></span> | <span data-ttu-id="b7851-472">2</span><span class="sxs-lookup"><span data-stu-id="b7851-472">2</span></span> |
| <span data-ttu-id="b7851-473">lastString</span><span class="sxs-lookup"><span data-stu-id="b7851-473">lastString</span></span> | <span data-ttu-id="b7851-474">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-474">Int</span></span> | <span data-ttu-id="b7851-475">0</span><span class="sxs-lookup"><span data-stu-id="b7851-475">0</span></span> |
| <span data-ttu-id="b7851-476">notFound</span><span class="sxs-lookup"><span data-stu-id="b7851-476">notFound</span></span> | <span data-ttu-id="b7851-477">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-477">Int</span></span> | <span data-ttu-id="b7851-478">-1</span><span class="sxs-lookup"><span data-stu-id="b7851-478">-1</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="b7851-479">poslední</span><span class="sxs-lookup"><span data-stu-id="b7851-479">last</span></span>
`last (arg1)`

<span data-ttu-id="b7851-480">Vrátí poslední znak řetězce nebo posledním elementem pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-480">Returns last character of the string, or the last element of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-481">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-481">Parameters</span></span>

| <span data-ttu-id="b7851-482">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-482">Parameter</span></span> | <span data-ttu-id="b7851-483">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-483">Required</span></span> | <span data-ttu-id="b7851-484">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-484">Type</span></span> | <span data-ttu-id="b7851-485">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-485">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-486">arg1</span><span class="sxs-lookup"><span data-stu-id="b7851-486">arg1</span></span> |<span data-ttu-id="b7851-487">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-487">Yes</span></span> |<span data-ttu-id="b7851-488">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-488">array or string</span></span> |<span data-ttu-id="b7851-489">Hodnota k načtení poslední element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="b7851-489">The value to retrieve the last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-490">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-490">Return value</span></span>

<span data-ttu-id="b7851-491">Řetězec poslední znak nebo typ posledním prvkem v pole (řetězec, int, pole nebo objekt).</span><span class="sxs-lookup"><span data-stu-id="b7851-491">A string of the last character, or the type (string, int, array, or object) of the last element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-492">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-492">Examples</span></span>

<span data-ttu-id="b7851-493">Následující příklad ukazuje, jak používat funkci naposledy s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-493">The following example shows how to use the last function with an array and string.</span></span>

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

<span data-ttu-id="b7851-494">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-494">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-495">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-495">Name</span></span> | <span data-ttu-id="b7851-496">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-496">Type</span></span> | <span data-ttu-id="b7851-497">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-497">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-498">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-498">arrayOutput</span></span> | <span data-ttu-id="b7851-499">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-499">String</span></span> | <span data-ttu-id="b7851-500">tři</span><span class="sxs-lookup"><span data-stu-id="b7851-500">three</span></span> |
| <span data-ttu-id="b7851-501">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-501">stringOutput</span></span> | <span data-ttu-id="b7851-502">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-502">String</span></span> | <span data-ttu-id="b7851-503">E</span><span class="sxs-lookup"><span data-stu-id="b7851-503">e</span></span> |

<a id="lastindexof" />

## <a name="lastindexof"></a><span data-ttu-id="b7851-504">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="b7851-504">lastIndexOf</span></span>
`lastIndexOf(stringToSearch, stringToFind)`

<span data-ttu-id="b7851-505">Vrátí poslední umístění hodnoty v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b7851-505">Returns the last position of a value within a string.</span></span> <span data-ttu-id="b7851-506">Porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-506">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-507">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-507">Parameters</span></span>

| <span data-ttu-id="b7851-508">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-508">Parameter</span></span> | <span data-ttu-id="b7851-509">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-509">Required</span></span> | <span data-ttu-id="b7851-510">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-510">Type</span></span> | <span data-ttu-id="b7851-511">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-511">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-512">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b7851-512">stringToSearch</span></span> |<span data-ttu-id="b7851-513">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-513">Yes</span></span> |<span data-ttu-id="b7851-514">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-514">string</span></span> |<span data-ttu-id="b7851-515">Hodnota, která obsahuje položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-515">The value that contains the item to find.</span></span> |
| <span data-ttu-id="b7851-516">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b7851-516">stringToFind</span></span> |<span data-ttu-id="b7851-517">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-517">Yes</span></span> |<span data-ttu-id="b7851-518">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-518">string</span></span> |<span data-ttu-id="b7851-519">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-519">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-520">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-520">Return value</span></span>

<span data-ttu-id="b7851-521">Celé číslo představující pozici poslední položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-521">An integer that represents the last position of the item to find.</span></span> <span data-ttu-id="b7851-522">Hodnota je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="b7851-522">The value is zero-based.</span></span> <span data-ttu-id="b7851-523">Pokud položka není nalezena, vrátí se -1.</span><span class="sxs-lookup"><span data-stu-id="b7851-523">If the item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-524">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-524">Examples</span></span>

<span data-ttu-id="b7851-525">Následující příklad ukazuje, jak používat funkce indexOf a lastIndexOf:</span><span class="sxs-lookup"><span data-stu-id="b7851-525">The following example shows how to use the indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="b7851-526">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-526">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-527">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-527">Name</span></span> | <span data-ttu-id="b7851-528">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-528">Type</span></span> | <span data-ttu-id="b7851-529">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-529">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-530">firstT</span><span class="sxs-lookup"><span data-stu-id="b7851-530">firstT</span></span> | <span data-ttu-id="b7851-531">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-531">Int</span></span> | <span data-ttu-id="b7851-532">0</span><span class="sxs-lookup"><span data-stu-id="b7851-532">0</span></span> |
| <span data-ttu-id="b7851-533">lastT</span><span class="sxs-lookup"><span data-stu-id="b7851-533">lastT</span></span> | <span data-ttu-id="b7851-534">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-534">Int</span></span> | <span data-ttu-id="b7851-535">3</span><span class="sxs-lookup"><span data-stu-id="b7851-535">3</span></span> |
| <span data-ttu-id="b7851-536">firstString</span><span class="sxs-lookup"><span data-stu-id="b7851-536">firstString</span></span> | <span data-ttu-id="b7851-537">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-537">Int</span></span> | <span data-ttu-id="b7851-538">2</span><span class="sxs-lookup"><span data-stu-id="b7851-538">2</span></span> |
| <span data-ttu-id="b7851-539">lastString</span><span class="sxs-lookup"><span data-stu-id="b7851-539">lastString</span></span> | <span data-ttu-id="b7851-540">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-540">Int</span></span> | <span data-ttu-id="b7851-541">0</span><span class="sxs-lookup"><span data-stu-id="b7851-541">0</span></span> |
| <span data-ttu-id="b7851-542">notFound</span><span class="sxs-lookup"><span data-stu-id="b7851-542">notFound</span></span> | <span data-ttu-id="b7851-543">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-543">Int</span></span> | <span data-ttu-id="b7851-544">-1</span><span class="sxs-lookup"><span data-stu-id="b7851-544">-1</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="b7851-545">Délka</span><span class="sxs-lookup"><span data-stu-id="b7851-545">length</span></span>
`length(string)`

<span data-ttu-id="b7851-546">Vrátí počet znaků v řetězci nebo prvků v poli.</span><span class="sxs-lookup"><span data-stu-id="b7851-546">Returns the number of characters in a string, or elements in an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-547">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-547">Parameters</span></span>

| <span data-ttu-id="b7851-548">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-548">Parameter</span></span> | <span data-ttu-id="b7851-549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-549">Required</span></span> | <span data-ttu-id="b7851-550">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-550">Type</span></span> | <span data-ttu-id="b7851-551">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-551">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-552">arg1</span><span class="sxs-lookup"><span data-stu-id="b7851-552">arg1</span></span> |<span data-ttu-id="b7851-553">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-553">Yes</span></span> |<span data-ttu-id="b7851-554">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-554">array or string</span></span> |<span data-ttu-id="b7851-555">Pole na použití pro získání počet elementů nebo řetězec Pokud chcete použít pro maximální počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-555">The array to use for getting the number of elements, or the string to use for getting the number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-556">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-556">Return value</span></span>

<span data-ttu-id="b7851-557">Typ int.</span><span class="sxs-lookup"><span data-stu-id="b7851-557">An int.</span></span> 

### <a name="examples"></a><span data-ttu-id="b7851-558">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-558">Examples</span></span>

<span data-ttu-id="b7851-559">Následující příklad ukazuje, jak používat délka s řetězec a pole:</span><span class="sxs-lookup"><span data-stu-id="b7851-559">The following example shows how to use length with an array and string:</span></span>

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

<span data-ttu-id="b7851-560">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-560">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-561">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-561">Name</span></span> | <span data-ttu-id="b7851-562">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-562">Type</span></span> | <span data-ttu-id="b7851-563">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-563">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-564">arrayLength</span><span class="sxs-lookup"><span data-stu-id="b7851-564">arrayLength</span></span> | <span data-ttu-id="b7851-565">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-565">Int</span></span> | <span data-ttu-id="b7851-566">3</span><span class="sxs-lookup"><span data-stu-id="b7851-566">3</span></span> |
| <span data-ttu-id="b7851-567">stringLength</span><span class="sxs-lookup"><span data-stu-id="b7851-567">stringLength</span></span> | <span data-ttu-id="b7851-568">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-568">Int</span></span> | <span data-ttu-id="b7851-569">13</span><span class="sxs-lookup"><span data-stu-id="b7851-569">13</span></span> |

<a id="padleft" />

## <a name="padleft"></a><span data-ttu-id="b7851-570">padLeft</span><span class="sxs-lookup"><span data-stu-id="b7851-570">padLeft</span></span>
`padLeft(valueToPad, totalLength, paddingCharacter)`

<span data-ttu-id="b7851-571">Vrací vpravo zarovnaný řetězec přidáním znaků na levé straně až do dosažení celkové určenou délku.</span><span class="sxs-lookup"><span data-stu-id="b7851-571">Returns a right-aligned string by adding characters to the left until reaching the total specified length.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-572">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-572">Parameters</span></span>

| <span data-ttu-id="b7851-573">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-573">Parameter</span></span> | <span data-ttu-id="b7851-574">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-574">Required</span></span> | <span data-ttu-id="b7851-575">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-575">Type</span></span> | <span data-ttu-id="b7851-576">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-576">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-577">valueToPad</span><span class="sxs-lookup"><span data-stu-id="b7851-577">valueToPad</span></span> |<span data-ttu-id="b7851-578">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-578">Yes</span></span> |<span data-ttu-id="b7851-579">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-579">string or int</span></span> |<span data-ttu-id="b7851-580">Hodnota Zarovnat vpravo.</span><span class="sxs-lookup"><span data-stu-id="b7851-580">The value to right-align.</span></span> |
| <span data-ttu-id="b7851-581">Hodnota totalLength</span><span class="sxs-lookup"><span data-stu-id="b7851-581">totalLength</span></span> |<span data-ttu-id="b7851-582">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-582">Yes</span></span> |<span data-ttu-id="b7851-583">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-583">int</span></span> |<span data-ttu-id="b7851-584">Celkový počet znaků v vrácený řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-584">The total number of characters in the returned string.</span></span> |
| <span data-ttu-id="b7851-585">paddingCharacter</span><span class="sxs-lookup"><span data-stu-id="b7851-585">paddingCharacter</span></span> |<span data-ttu-id="b7851-586">Ne</span><span class="sxs-lookup"><span data-stu-id="b7851-586">No</span></span> |<span data-ttu-id="b7851-587">jeden znak</span><span class="sxs-lookup"><span data-stu-id="b7851-587">single character</span></span> |<span data-ttu-id="b7851-588">Znak, který má používat pro odsazení nalevo až do dosažení celkové délky.</span><span class="sxs-lookup"><span data-stu-id="b7851-588">The character to use for left-padding until the total length is reached.</span></span> <span data-ttu-id="b7851-589">Výchozí hodnota je mezera.</span><span class="sxs-lookup"><span data-stu-id="b7851-589">The default value is a space.</span></span> |

<span data-ttu-id="b7851-590">Pokud původní text je delší než počet znaků k vyplnění, přidají se žádné znaky.</span><span class="sxs-lookup"><span data-stu-id="b7851-590">If the original string is longer than the number of characters to pad, no characters are added.</span></span>

### <a name="return-value"></a><span data-ttu-id="b7851-591">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-591">Return value</span></span>

<span data-ttu-id="b7851-592">A řetězec s minimálně počet zadaný znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-592">A string with at least the number of specified characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-593">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-593">Examples</span></span>

<span data-ttu-id="b7851-594">Následující příklad ukazuje, jak k vyplnění hodnota parametru zadaný uživatelem přidáním nulu, dokud nebude dosaženo celkový počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-594">The following example shows how to pad the user-provided parameter value by adding the zero character until it reaches the total number of characters.</span></span> 

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

<span data-ttu-id="b7851-595">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-595">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-596">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-596">Name</span></span> | <span data-ttu-id="b7851-597">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-597">Type</span></span> | <span data-ttu-id="b7851-598">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-598">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-599">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-599">stringOutput</span></span> | <span data-ttu-id="b7851-600">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-600">String</span></span> | <span data-ttu-id="b7851-601">0000000123</span><span class="sxs-lookup"><span data-stu-id="b7851-601">0000000123</span></span> |

<a id="replace" />

## <a name="replace"></a><span data-ttu-id="b7851-602">Nahradit</span><span class="sxs-lookup"><span data-stu-id="b7851-602">replace</span></span>
`replace(originalString, oldString, newString)`

<span data-ttu-id="b7851-603">Vrátí nový řetězec se všechny instance jeden řetězec nahrazen jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b7851-603">Returns a new string with all instances of one string replaced by another string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-604">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-604">Parameters</span></span>

| <span data-ttu-id="b7851-605">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-605">Parameter</span></span> | <span data-ttu-id="b7851-606">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-606">Required</span></span> | <span data-ttu-id="b7851-607">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-607">Type</span></span> | <span data-ttu-id="b7851-608">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-608">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-609">originalString</span><span class="sxs-lookup"><span data-stu-id="b7851-609">originalString</span></span> |<span data-ttu-id="b7851-610">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-610">Yes</span></span> |<span data-ttu-id="b7851-611">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-611">string</span></span> |<span data-ttu-id="b7851-612">Hodnota, která obsahuje všechny instance jeden řetězec nahrazen jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b7851-612">The value that has all instances of one string replaced by another string.</span></span> |
| <span data-ttu-id="b7851-613">oldString</span><span class="sxs-lookup"><span data-stu-id="b7851-613">oldString</span></span> |<span data-ttu-id="b7851-614">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-614">Yes</span></span> |<span data-ttu-id="b7851-615">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-615">string</span></span> |<span data-ttu-id="b7851-616">Řetězec, který má být odebrána z původního řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-616">The string to be removed from the original string.</span></span> |
| <span data-ttu-id="b7851-617">newstring –</span><span class="sxs-lookup"><span data-stu-id="b7851-617">newString</span></span> |<span data-ttu-id="b7851-618">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-618">Yes</span></span> |<span data-ttu-id="b7851-619">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-619">string</span></span> |<span data-ttu-id="b7851-620">Řetězec, který se má přidat místo text odebrané.</span><span class="sxs-lookup"><span data-stu-id="b7851-620">The string to add in place of the removed string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-621">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-621">Return value</span></span>

<span data-ttu-id="b7851-622">Řetězec s nahrazené znaky.</span><span class="sxs-lookup"><span data-stu-id="b7851-622">A string with the replaced characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-623">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-623">Examples</span></span>

<span data-ttu-id="b7851-624">Následující příklad ukazuje, jak odebrat všechny pomlčky z řetězce zadaný uživatelem a jak součást řetězce identifikátoru nahradit jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b7851-624">The following example shows how to remove all dashes from the user-provided string, and how to replace part of the string with another string.</span></span>

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

<span data-ttu-id="b7851-625">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-625">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-626">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-626">Name</span></span> | <span data-ttu-id="b7851-627">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-627">Type</span></span> | <span data-ttu-id="b7851-628">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-628">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-629">firstOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-629">firstOutput</span></span> | <span data-ttu-id="b7851-630">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-630">String</span></span> | <span data-ttu-id="b7851-631">1231231234</span><span class="sxs-lookup"><span data-stu-id="b7851-631">1231231234</span></span> |
| <span data-ttu-id="b7851-632">secodeOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-632">secodeOutput</span></span> | <span data-ttu-id="b7851-633">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-633">String</span></span> | <span data-ttu-id="b7851-634">123-123-xxxx</span><span class="sxs-lookup"><span data-stu-id="b7851-634">123-123-xxxx</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="b7851-635">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="b7851-635">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="b7851-636">Vrátí řetězec s odebranými znaky po zadaný počet znaků, nebo pole s všechny elementy po zadaný počet elementů.</span><span class="sxs-lookup"><span data-stu-id="b7851-636">Returns a string with all the characters after the specified number of characters, or an array with all the elements after the specified number of elements.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-637">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-637">Parameters</span></span>

| <span data-ttu-id="b7851-638">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-638">Parameter</span></span> | <span data-ttu-id="b7851-639">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-639">Required</span></span> | <span data-ttu-id="b7851-640">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-640">Type</span></span> | <span data-ttu-id="b7851-641">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-641">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-642">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-642">originalValue</span></span> |<span data-ttu-id="b7851-643">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-643">Yes</span></span> |<span data-ttu-id="b7851-644">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-644">array or string</span></span> |<span data-ttu-id="b7851-645">Pole nebo řetězec, který má používat pro přeskočení.</span><span class="sxs-lookup"><span data-stu-id="b7851-645">The array or string to use for skipping.</span></span> |
| <span data-ttu-id="b7851-646">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="b7851-646">numberToSkip</span></span> |<span data-ttu-id="b7851-647">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-647">Yes</span></span> |<span data-ttu-id="b7851-648">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-648">int</span></span> |<span data-ttu-id="b7851-649">Počet elementů nebo znaků, které chcete vynechat.</span><span class="sxs-lookup"><span data-stu-id="b7851-649">The number of elements or characters to skip.</span></span> <span data-ttu-id="b7851-650">Pokud tato hodnota je 0 nebo menší, vrátí se všechny elementy nebo znaků v hodnotě.</span><span class="sxs-lookup"><span data-stu-id="b7851-650">If this value is 0 or less, all the elements or characters in the value are returned.</span></span> <span data-ttu-id="b7851-651">Pokud je větší než délka pole nebo řetězec, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-651">If it is larger than the length of the array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-652">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-652">Return value</span></span>

<span data-ttu-id="b7851-653">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-653">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-654">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-654">Examples</span></span>

<span data-ttu-id="b7851-655">Následující příklad přeskočí zadaný počet elementů v poli a zadaný počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b7851-655">The following example skips the specified number of elements in the array, and the specified number of characters in a string.</span></span>

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

<span data-ttu-id="b7851-656">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-656">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-657">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-657">Name</span></span> | <span data-ttu-id="b7851-658">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-658">Type</span></span> | <span data-ttu-id="b7851-659">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-659">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-660">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-660">arrayOutput</span></span> | <span data-ttu-id="b7851-661">Pole</span><span class="sxs-lookup"><span data-stu-id="b7851-661">Array</span></span> | <span data-ttu-id="b7851-662">["tři"]</span><span class="sxs-lookup"><span data-stu-id="b7851-662">["three"]</span></span> |
| <span data-ttu-id="b7851-663">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-663">stringOutput</span></span> | <span data-ttu-id="b7851-664">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-664">String</span></span> | <span data-ttu-id="b7851-665">dvě tři</span><span class="sxs-lookup"><span data-stu-id="b7851-665">two three</span></span> |

<a id="split" />

## <a name="split"></a><span data-ttu-id="b7851-666">split</span><span class="sxs-lookup"><span data-stu-id="b7851-666">split</span></span>
`split(inputString, delimiter)`

<span data-ttu-id="b7851-667">Vrátí pole řetězců obsahující dílčích řetězců vstupního řetězce, které jsou odděleny zadaných oddělovačů.</span><span class="sxs-lookup"><span data-stu-id="b7851-667">Returns an array of strings that contains the substrings of the input string that are delimited by the specified delimiters.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-668">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-668">Parameters</span></span>

| <span data-ttu-id="b7851-669">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-669">Parameter</span></span> | <span data-ttu-id="b7851-670">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-670">Required</span></span> | <span data-ttu-id="b7851-671">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-671">Type</span></span> | <span data-ttu-id="b7851-672">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-672">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-673">inputString</span><span class="sxs-lookup"><span data-stu-id="b7851-673">inputString</span></span> |<span data-ttu-id="b7851-674">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-674">Yes</span></span> |<span data-ttu-id="b7851-675">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-675">string</span></span> |<span data-ttu-id="b7851-676">Řetězec k rozdělení.</span><span class="sxs-lookup"><span data-stu-id="b7851-676">The string to split.</span></span> |
| <span data-ttu-id="b7851-677">Oddělovač</span><span class="sxs-lookup"><span data-stu-id="b7851-677">delimiter</span></span> |<span data-ttu-id="b7851-678">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-678">Yes</span></span> |<span data-ttu-id="b7851-679">řetězec nebo pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="b7851-679">string or array of strings</span></span> |<span data-ttu-id="b7851-680">Oddělovač, který se má použít k rozdělení řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-680">The delimiter to use for splitting the string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-681">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-681">Return value</span></span>

<span data-ttu-id="b7851-682">Pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="b7851-682">An array of strings.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-683">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-683">Examples</span></span>

<span data-ttu-id="b7851-684">Následující příklad rozdělí vstupní řetězec s čárkou a s čárkou nebo středníkem.</span><span class="sxs-lookup"><span data-stu-id="b7851-684">The following example splits the input string with a comma, and with either a comma or a semi-colon.</span></span>

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

<span data-ttu-id="b7851-685">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-685">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-686">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-686">Name</span></span> | <span data-ttu-id="b7851-687">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-687">Type</span></span> | <span data-ttu-id="b7851-688">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-688">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-689">firstOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-689">firstOutput</span></span> | <span data-ttu-id="b7851-690">Pole</span><span class="sxs-lookup"><span data-stu-id="b7851-690">Array</span></span> | <span data-ttu-id="b7851-691">["1", "dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="b7851-691">["one", "two", "three"]</span></span> |
| <span data-ttu-id="b7851-692">secondOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-692">secondOutput</span></span> | <span data-ttu-id="b7851-693">Pole</span><span class="sxs-lookup"><span data-stu-id="b7851-693">Array</span></span> | <span data-ttu-id="b7851-694">["1", "dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="b7851-694">["one", "two", "three"]</span></span> |

<a id="startswith" />

## <a name="startswith"></a><span data-ttu-id="b7851-695">startsWith</span><span class="sxs-lookup"><span data-stu-id="b7851-695">startsWith</span></span>
`startsWith(stringToSearch, stringToFind)`

<span data-ttu-id="b7851-696">Určuje, zda řetězec začíná hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b7851-696">Determines whether a string starts with a value.</span></span> <span data-ttu-id="b7851-697">Porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-697">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-698">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-698">Parameters</span></span>

| <span data-ttu-id="b7851-699">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-699">Parameter</span></span> | <span data-ttu-id="b7851-700">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-700">Required</span></span> | <span data-ttu-id="b7851-701">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-701">Type</span></span> | <span data-ttu-id="b7851-702">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-702">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-703">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b7851-703">stringToSearch</span></span> |<span data-ttu-id="b7851-704">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-704">Yes</span></span> |<span data-ttu-id="b7851-705">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-705">string</span></span> |<span data-ttu-id="b7851-706">Hodnota, která obsahuje položku, kterou chcete najít.</span><span class="sxs-lookup"><span data-stu-id="b7851-706">The value that contains the item to find.</span></span> |
| <span data-ttu-id="b7851-707">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b7851-707">stringToFind</span></span> |<span data-ttu-id="b7851-708">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-708">Yes</span></span> |<span data-ttu-id="b7851-709">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-709">string</span></span> |<span data-ttu-id="b7851-710">Hodnota k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="b7851-710">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-711">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-711">Return value</span></span>

<span data-ttu-id="b7851-712">**Hodnota TRUE,** Pokud první znak nebo znaky řetězce odpovídají hodnotě; jinak **False**.</span><span class="sxs-lookup"><span data-stu-id="b7851-712">**True** if the first character or characters of the string match the value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-713">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-713">Examples</span></span>

<span data-ttu-id="b7851-714">Následující příklad ukazuje, jak používat funkce startsWith a endsWith:</span><span class="sxs-lookup"><span data-stu-id="b7851-714">The following example shows how to use the startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="b7851-715">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-715">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-716">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-716">Name</span></span> | <span data-ttu-id="b7851-717">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-717">Type</span></span> | <span data-ttu-id="b7851-718">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-718">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-719">startsTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-719">startsTrue</span></span> | <span data-ttu-id="b7851-720">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-720">Bool</span></span> | <span data-ttu-id="b7851-721">True</span><span class="sxs-lookup"><span data-stu-id="b7851-721">True</span></span> |
| <span data-ttu-id="b7851-722">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-722">startsCapTrue</span></span> | <span data-ttu-id="b7851-723">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-723">Bool</span></span> | <span data-ttu-id="b7851-724">True</span><span class="sxs-lookup"><span data-stu-id="b7851-724">True</span></span> |
| <span data-ttu-id="b7851-725">startsFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-725">startsFalse</span></span> | <span data-ttu-id="b7851-726">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-726">Bool</span></span> | <span data-ttu-id="b7851-727">False</span><span class="sxs-lookup"><span data-stu-id="b7851-727">False</span></span> |
| <span data-ttu-id="b7851-728">endsTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-728">endsTrue</span></span> | <span data-ttu-id="b7851-729">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-729">Bool</span></span> | <span data-ttu-id="b7851-730">True</span><span class="sxs-lookup"><span data-stu-id="b7851-730">True</span></span> |
| <span data-ttu-id="b7851-731">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b7851-731">endsCapTrue</span></span> | <span data-ttu-id="b7851-732">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-732">Bool</span></span> | <span data-ttu-id="b7851-733">True</span><span class="sxs-lookup"><span data-stu-id="b7851-733">True</span></span> |
| <span data-ttu-id="b7851-734">endsFalse</span><span class="sxs-lookup"><span data-stu-id="b7851-734">endsFalse</span></span> | <span data-ttu-id="b7851-735">BOOL</span><span class="sxs-lookup"><span data-stu-id="b7851-735">Bool</span></span> | <span data-ttu-id="b7851-736">False</span><span class="sxs-lookup"><span data-stu-id="b7851-736">False</span></span> |

<a id="string" />

## <a name="string"></a><span data-ttu-id="b7851-737">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-737">string</span></span>
`string(valueToConvert)`

<span data-ttu-id="b7851-738">Převede zadanou hodnotu na řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-738">Converts the specified value to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-739">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-739">Parameters</span></span>

| <span data-ttu-id="b7851-740">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-740">Parameter</span></span> | <span data-ttu-id="b7851-741">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-741">Required</span></span> | <span data-ttu-id="b7851-742">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-742">Type</span></span> | <span data-ttu-id="b7851-743">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-743">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-744">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="b7851-744">valueToConvert</span></span> |<span data-ttu-id="b7851-745">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-745">Yes</span></span> | <span data-ttu-id="b7851-746">Všechny</span><span class="sxs-lookup"><span data-stu-id="b7851-746">Any</span></span> |<span data-ttu-id="b7851-747">Hodnota převést na řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-747">The value to convert to string.</span></span> <span data-ttu-id="b7851-748">Žádný druh hodnotu lze převést, včetně objekty a pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-748">Any type of value can be converted, including objects and arrays.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-749">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-749">Return value</span></span>

<span data-ttu-id="b7851-750">Řetězec převedenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b7851-750">A string of the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-751">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-751">Examples</span></span>

<span data-ttu-id="b7851-752">Následující příklad ukazuje, jak převést různé typy hodnot řetězců:</span><span class="sxs-lookup"><span data-stu-id="b7851-752">The following example shows how to convert different types of values to strings:</span></span>

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

<span data-ttu-id="b7851-753">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-753">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-754">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-754">Name</span></span> | <span data-ttu-id="b7851-755">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-755">Type</span></span> | <span data-ttu-id="b7851-756">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-756">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-757">objectOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-757">objectOutput</span></span> | <span data-ttu-id="b7851-758">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-758">String</span></span> | <span data-ttu-id="b7851-759">{"dat": 10, hodnotu "b": "Text, například"}</span><span class="sxs-lookup"><span data-stu-id="b7851-759">{"valueA":10,"valueB":"Example Text"}</span></span> |
| <span data-ttu-id="b7851-760">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-760">arrayOutput</span></span> | <span data-ttu-id="b7851-761">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-761">String</span></span> | <span data-ttu-id="b7851-762">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="b7851-762">["a","b","c"]</span></span> |
| <span data-ttu-id="b7851-763">intOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-763">intOutput</span></span> | <span data-ttu-id="b7851-764">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-764">String</span></span> | <span data-ttu-id="b7851-765">5</span><span class="sxs-lookup"><span data-stu-id="b7851-765">5</span></span> |

<a id="substring" />

## <a name="substring"></a><span data-ttu-id="b7851-766">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-766">substring</span></span>
`substring(stringToParse, startIndex, length)`

<span data-ttu-id="b7851-767">Vrátí dílčí řetězec, který začíná na pozici zadaný znak a obsahuje zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-767">Returns a substring that starts at the specified character position and contains the specified number of characters.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-768">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-768">Parameters</span></span>

| <span data-ttu-id="b7851-769">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-769">Parameter</span></span> | <span data-ttu-id="b7851-770">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-770">Required</span></span> | <span data-ttu-id="b7851-771">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-771">Type</span></span> | <span data-ttu-id="b7851-772">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-772">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-773">stringToParse</span><span class="sxs-lookup"><span data-stu-id="b7851-773">stringToParse</span></span> |<span data-ttu-id="b7851-774">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-774">Yes</span></span> |<span data-ttu-id="b7851-775">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-775">string</span></span> |<span data-ttu-id="b7851-776">Původní řetězec, ze které je extrahován dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-776">The original string from which the substring is extracted.</span></span> |
| <span data-ttu-id="b7851-777">Počáteční index</span><span class="sxs-lookup"><span data-stu-id="b7851-777">startIndex</span></span> |<span data-ttu-id="b7851-778">Ne</span><span class="sxs-lookup"><span data-stu-id="b7851-778">No</span></span> |<span data-ttu-id="b7851-779">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-779">int</span></span> |<span data-ttu-id="b7851-780">Počáteční znak pozice s nulovým základem pro dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-780">The zero-based starting character position for the substring.</span></span> |
| <span data-ttu-id="b7851-781">Délka</span><span class="sxs-lookup"><span data-stu-id="b7851-781">length</span></span> |<span data-ttu-id="b7851-782">Ne</span><span class="sxs-lookup"><span data-stu-id="b7851-782">No</span></span> |<span data-ttu-id="b7851-783">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-783">int</span></span> |<span data-ttu-id="b7851-784">Počet znaků pro dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-784">The number of characters for the substring.</span></span> <span data-ttu-id="b7851-785">Musí odkazovat na umístění v rámci řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-785">Must refer to a location within the string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-786">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-786">Return value</span></span>

<span data-ttu-id="b7851-787">Dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-787">The substring.</span></span>

### <a name="remarks"></a><span data-ttu-id="b7851-788">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b7851-788">Remarks</span></span>

<span data-ttu-id="b7851-789">Funkce selže, když dílčí řetězec přesahuje konci řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-789">The function fails when the substring extends beyond the end of the string.</span></span> <span data-ttu-id="b7851-790">V následujícím příkladu se nezdaří s chybou "parametry indexu a délky musí odkazovat na umístění v rámci řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-790">The following example fails with the error "The index and length parameters must refer to a location within the string.</span></span> <span data-ttu-id="b7851-791">Parametr indexu: "0", parametr délky: 11, Délka parametru řetězce: "10". ".</span><span class="sxs-lookup"><span data-stu-id="b7851-791">The index parameter: '0', the length parameter: '11', the length of the string parameter: '10'.".</span></span>

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a><span data-ttu-id="b7851-792">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-792">Examples</span></span>

<span data-ttu-id="b7851-793">Následující příklad extrahuje dílčí řetězec z parametr.</span><span class="sxs-lookup"><span data-stu-id="b7851-793">The following example extracts a substring from a parameter.</span></span>

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

<span data-ttu-id="b7851-794">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-794">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-795">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-795">Name</span></span> | <span data-ttu-id="b7851-796">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-796">Type</span></span> | <span data-ttu-id="b7851-797">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-797">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-798">substringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-798">substringOutput</span></span> | <span data-ttu-id="b7851-799">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-799">String</span></span> | <span data-ttu-id="b7851-800">dvě</span><span class="sxs-lookup"><span data-stu-id="b7851-800">two</span></span> |


<a id="take" />

## <a name="take"></a><span data-ttu-id="b7851-801">proveďte</span><span class="sxs-lookup"><span data-stu-id="b7851-801">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="b7851-802">Vrátí řetězec s zadaný počet znaků od začátku řetězec nebo pole s zadaný počet elementů od začátku pole.</span><span class="sxs-lookup"><span data-stu-id="b7851-802">Returns a string with the specified number of characters from the start of the string, or an array with the specified number of elements from the start of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-803">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-803">Parameters</span></span>

| <span data-ttu-id="b7851-804">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-804">Parameter</span></span> | <span data-ttu-id="b7851-805">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-805">Required</span></span> | <span data-ttu-id="b7851-806">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-806">Type</span></span> | <span data-ttu-id="b7851-807">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-807">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-808">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-808">originalValue</span></span> |<span data-ttu-id="b7851-809">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-809">Yes</span></span> |<span data-ttu-id="b7851-810">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-810">array or string</span></span> |<span data-ttu-id="b7851-811">Pole nebo řetězec, který má trvat elementy ze.</span><span class="sxs-lookup"><span data-stu-id="b7851-811">The array or string to take the elements from.</span></span> |
| <span data-ttu-id="b7851-812">numberToTake</span><span class="sxs-lookup"><span data-stu-id="b7851-812">numberToTake</span></span> |<span data-ttu-id="b7851-813">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-813">Yes</span></span> |<span data-ttu-id="b7851-814">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b7851-814">int</span></span> |<span data-ttu-id="b7851-815">Počet elementů nebo znaků, který má trvat.</span><span class="sxs-lookup"><span data-stu-id="b7851-815">The number of elements or characters to take.</span></span> <span data-ttu-id="b7851-816">Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-816">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="b7851-817">Pokud je větší než délka dané pole nebo řetězec, vrátí se všechny elementy ve pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-817">If it is larger than the length of the given array or string, all the elements in the array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-818">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-818">Return value</span></span>

<span data-ttu-id="b7851-819">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-819">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-820">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-820">Examples</span></span>

<span data-ttu-id="b7851-821">V následujícím příkladu má zadaný počet prvků z pole a znaků z řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-821">The following example takes the specified number of elements from the array, and characters from a string.</span></span>

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

<span data-ttu-id="b7851-822">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-822">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-823">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-823">Name</span></span> | <span data-ttu-id="b7851-824">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-824">Type</span></span> | <span data-ttu-id="b7851-825">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-825">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-826">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-826">arrayOutput</span></span> | <span data-ttu-id="b7851-827">Pole</span><span class="sxs-lookup"><span data-stu-id="b7851-827">Array</span></span> | <span data-ttu-id="b7851-828">["1", "dva"]</span><span class="sxs-lookup"><span data-stu-id="b7851-828">["one", "two"]</span></span> |
| <span data-ttu-id="b7851-829">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-829">stringOutput</span></span> | <span data-ttu-id="b7851-830">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-830">String</span></span> | <span data-ttu-id="b7851-831">na</span><span class="sxs-lookup"><span data-stu-id="b7851-831">on</span></span> |

<a id="tolower" />

## <a name="tolower"></a><span data-ttu-id="b7851-832">toLower</span><span class="sxs-lookup"><span data-stu-id="b7851-832">toLower</span></span>
`toLower(stringToChange)`

<span data-ttu-id="b7851-833">Převede zadaný řetězec na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-833">Converts the specified string to lower case.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-834">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-834">Parameters</span></span>

| <span data-ttu-id="b7851-835">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-835">Parameter</span></span> | <span data-ttu-id="b7851-836">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-836">Required</span></span> | <span data-ttu-id="b7851-837">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-837">Type</span></span> | <span data-ttu-id="b7851-838">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-838">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-839">stringToChange</span><span class="sxs-lookup"><span data-stu-id="b7851-839">stringToChange</span></span> |<span data-ttu-id="b7851-840">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-840">Yes</span></span> |<span data-ttu-id="b7851-841">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-841">string</span></span> |<span data-ttu-id="b7851-842">Hodnota k převedení na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-842">The value to convert to lower case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-843">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-843">Return value</span></span>

<span data-ttu-id="b7851-844">Daný řetězec převést na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-844">The string converted to lower case.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-845">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-845">Examples</span></span>

<span data-ttu-id="b7851-846">Následující příklad převede hodnotu parametru na malá písmena a na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-846">The following example converts a parameter value to lower case and to upper case.</span></span>

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

<span data-ttu-id="b7851-847">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-847">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-848">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-848">Name</span></span> | <span data-ttu-id="b7851-849">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-849">Type</span></span> | <span data-ttu-id="b7851-850">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-850">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-851">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-851">toLowerOutput</span></span> | <span data-ttu-id="b7851-852">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-852">String</span></span> | <span data-ttu-id="b7851-853">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-853">one two three</span></span> |
| <span data-ttu-id="b7851-854">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-854">toUpperOutput</span></span> | <span data-ttu-id="b7851-855">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-855">String</span></span> | <span data-ttu-id="b7851-856">JEDNA DVA TŘI</span><span class="sxs-lookup"><span data-stu-id="b7851-856">ONE TWO THREE</span></span> |

<a id="toupper" />

## <a name="toupper"></a><span data-ttu-id="b7851-857">toUpper</span><span class="sxs-lookup"><span data-stu-id="b7851-857">toUpper</span></span>
`toUpper(stringToChange)`

<span data-ttu-id="b7851-858">Převede zadaný řetězec na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-858">Converts the specified string to upper case.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-859">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-859">Parameters</span></span>

| <span data-ttu-id="b7851-860">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-860">Parameter</span></span> | <span data-ttu-id="b7851-861">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-861">Required</span></span> | <span data-ttu-id="b7851-862">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-862">Type</span></span> | <span data-ttu-id="b7851-863">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-863">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-864">stringToChange</span><span class="sxs-lookup"><span data-stu-id="b7851-864">stringToChange</span></span> |<span data-ttu-id="b7851-865">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-865">Yes</span></span> |<span data-ttu-id="b7851-866">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-866">string</span></span> |<span data-ttu-id="b7851-867">Hodnota k převedení na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-867">The value to convert to upper case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-868">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-868">Return value</span></span>

<span data-ttu-id="b7851-869">Daný řetězec převést na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-869">The string converted to upper case.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-870">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-870">Examples</span></span>

<span data-ttu-id="b7851-871">Následující příklad převede hodnotu parametru na malá písmena a na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b7851-871">The following example converts a parameter value to lower case and to upper case.</span></span>

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

<span data-ttu-id="b7851-872">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-872">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-873">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-873">Name</span></span> | <span data-ttu-id="b7851-874">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-874">Type</span></span> | <span data-ttu-id="b7851-875">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-875">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-876">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-876">toLowerOutput</span></span> | <span data-ttu-id="b7851-877">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-877">String</span></span> | <span data-ttu-id="b7851-878">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-878">one two three</span></span> |
| <span data-ttu-id="b7851-879">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-879">toUpperOutput</span></span> | <span data-ttu-id="b7851-880">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-880">String</span></span> | <span data-ttu-id="b7851-881">JEDNA DVA TŘI</span><span class="sxs-lookup"><span data-stu-id="b7851-881">ONE TWO THREE</span></span> |

<a id="trim" />

## <a name="trim"></a><span data-ttu-id="b7851-882">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="b7851-882">trim</span></span>
`trim (stringToTrim)`

<span data-ttu-id="b7851-883">Odebere všechny úvodní a koncové prázdné znaky ze zadaného řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-883">Removes all leading and trailing white-space characters from the specified string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-884">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-884">Parameters</span></span>

| <span data-ttu-id="b7851-885">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-885">Parameter</span></span> | <span data-ttu-id="b7851-886">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-886">Required</span></span> | <span data-ttu-id="b7851-887">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-887">Type</span></span> | <span data-ttu-id="b7851-888">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-888">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-889">stringToTrim</span><span class="sxs-lookup"><span data-stu-id="b7851-889">stringToTrim</span></span> |<span data-ttu-id="b7851-890">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-890">Yes</span></span> |<span data-ttu-id="b7851-891">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-891">string</span></span> |<span data-ttu-id="b7851-892">Hodnota k uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="b7851-892">The value to trim.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-893">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-893">Return value</span></span>

<span data-ttu-id="b7851-894">Řetězec, který bez úvodní a koncové prázdné znaky.</span><span class="sxs-lookup"><span data-stu-id="b7851-894">The string without leading and trailing white-space characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-895">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-895">Examples</span></span>

<span data-ttu-id="b7851-896">Následující příklad ořízne prázdné znaky z parametru.</span><span class="sxs-lookup"><span data-stu-id="b7851-896">The following example trims the white-space characters from the parameter.</span></span>

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

<span data-ttu-id="b7851-897">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-897">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-898">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-898">Name</span></span> | <span data-ttu-id="b7851-899">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-899">Type</span></span> | <span data-ttu-id="b7851-900">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-900">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-901">Vrátí</span><span class="sxs-lookup"><span data-stu-id="b7851-901">return</span></span> | <span data-ttu-id="b7851-902">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-902">String</span></span> | <span data-ttu-id="b7851-903">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b7851-903">one two three</span></span> |

<a id="uniquestring" />

## <a name="uniquestring"></a><span data-ttu-id="b7851-904">uniqueString</span><span class="sxs-lookup"><span data-stu-id="b7851-904">uniqueString</span></span>
`uniqueString (baseString, ...)`

<span data-ttu-id="b7851-905">Vytvoří řetězec deterministickou hash na základě hodnot zadaných jako parametry.</span><span class="sxs-lookup"><span data-stu-id="b7851-905">Creates a deterministic hash string based on the values provided as parameters.</span></span> 

### <a name="parameters"></a><span data-ttu-id="b7851-906">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-906">Parameters</span></span>

| <span data-ttu-id="b7851-907">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-907">Parameter</span></span> | <span data-ttu-id="b7851-908">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-908">Required</span></span> | <span data-ttu-id="b7851-909">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-909">Type</span></span> | <span data-ttu-id="b7851-910">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-910">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-911">baseString</span><span class="sxs-lookup"><span data-stu-id="b7851-911">baseString</span></span> |<span data-ttu-id="b7851-912">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-912">Yes</span></span> |<span data-ttu-id="b7851-913">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-913">string</span></span> |<span data-ttu-id="b7851-914">Hodnota použitá v funkce hash pro vytvoření jedinečné řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-914">The value used in the hash function to create a unique string.</span></span> |
| <span data-ttu-id="b7851-915">Další parametry podle potřeby</span><span class="sxs-lookup"><span data-stu-id="b7851-915">additional parameters as needed</span></span> |<span data-ttu-id="b7851-916">Ne</span><span class="sxs-lookup"><span data-stu-id="b7851-916">No</span></span> |<span data-ttu-id="b7851-917">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-917">string</span></span> |<span data-ttu-id="b7851-918">Můžete přidat libovolný počet řetězce podle potřeby vytvořit hodnotu, která určuje úroveň jedinečnosti.</span><span class="sxs-lookup"><span data-stu-id="b7851-918">You can add as many strings as needed to create the value that specifies the level of uniqueness.</span></span> |

### <a name="remarks"></a><span data-ttu-id="b7851-919">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b7851-919">Remarks</span></span>

<span data-ttu-id="b7851-920">Tato funkce je užitečné, pokud je potřeba vytvořit jedinečný název pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="b7851-920">This function is helpful when you need to create a unique name for a resource.</span></span> <span data-ttu-id="b7851-921">Je-li zadat hodnoty parametrů, které omezí rozsah jedinečnosti pro výsledek.</span><span class="sxs-lookup"><span data-stu-id="b7851-921">You provide parameter values that limit the scope of uniqueness for the result.</span></span> <span data-ttu-id="b7851-922">Můžete zadat, zda je název jedinečný dolů předplatné, skupinu prostředků nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="b7851-922">You can specify whether the name is unique down to subscription, resource group, or deployment.</span></span> 

<span data-ttu-id="b7851-923">Vrácená hodnota není náhodný řetězec, ale spíš výsledek funkce hash.</span><span class="sxs-lookup"><span data-stu-id="b7851-923">The returned value is not a random string, but rather the result of a hash function.</span></span> <span data-ttu-id="b7851-924">Vrácená hodnota je 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-924">The returned value is 13 characters long.</span></span> <span data-ttu-id="b7851-925">Není globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="b7851-925">It is not globally unique.</span></span> <span data-ttu-id="b7851-926">Chcete zkombinovat hodnotu s předponou z vaší zásady vytváření názvů vytvořit smysluplný název.</span><span class="sxs-lookup"><span data-stu-id="b7851-926">You may want to combine the value with a prefix from your naming convention to create a name that is meaningful.</span></span> <span data-ttu-id="b7851-927">Následující příklad ukazuje formát vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b7851-927">The following example shows the format of the returned value.</span></span> <span data-ttu-id="b7851-928">Skutečná hodnota se liší podle parametrů.</span><span class="sxs-lookup"><span data-stu-id="b7851-928">The actual value varies by the provided parameters.</span></span>

    tcvhiyu5h2o5o

<span data-ttu-id="b7851-929">Následující příklady ukazují, jak pomocí uniqueString můžete vytvořit jedinečnou hodnotu pro běžně používané úrovně.</span><span class="sxs-lookup"><span data-stu-id="b7851-929">The following examples show how to use uniqueString to create a unique value for commonly used levels.</span></span>

<span data-ttu-id="b7851-930">Jedinečný obor do předplatného</span><span class="sxs-lookup"><span data-stu-id="b7851-930">Unique scoped to subscription</span></span>

```json
"[uniqueString(subscription().subscriptionId)]"
```

<span data-ttu-id="b7851-931">Jedinečný obor do skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b7851-931">Unique scoped to resource group</span></span>

```json
"[uniqueString(resourceGroup().id)]"
```

<span data-ttu-id="b7851-932">Jedinečný rozsah nasazení pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b7851-932">Unique scoped to deployment for a resource group</span></span>

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

<span data-ttu-id="b7851-933">Následující příklad ukazuje, jak vytvořit jedinečný název pro účet úložiště na základě vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b7851-933">The following example shows how to create a unique name for a storage account based on your resource group.</span></span> <span data-ttu-id="b7851-934">Uvnitř skupinu prostředků název není jedinečný, pokud je vytvořen stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b7851-934">Inside the resource group, the name is not unique if constructed the same way.</span></span>

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a><span data-ttu-id="b7851-935">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-935">Return value</span></span>

<span data-ttu-id="b7851-936">Řetězec obsahující 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="b7851-936">A string containing 13 characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-937">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-937">Examples</span></span>

<span data-ttu-id="b7851-938">Následující příklad vrátí výsledky z uniquestring:</span><span class="sxs-lookup"><span data-stu-id="b7851-938">The following example returns results from uniquestring:</span></span>

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

## <a name="uri"></a><span data-ttu-id="b7851-939">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="b7851-939">uri</span></span>
`uri (baseUri, relativeUri)`

<span data-ttu-id="b7851-940">Vytvoří absolutní identifikátor URI kombinací baseUri a relativeUri řetězce.</span><span class="sxs-lookup"><span data-stu-id="b7851-940">Creates an absolute URI by combining the baseUri and the relativeUri string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-941">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-941">Parameters</span></span>

| <span data-ttu-id="b7851-942">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-942">Parameter</span></span> | <span data-ttu-id="b7851-943">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-943">Required</span></span> | <span data-ttu-id="b7851-944">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-944">Type</span></span> | <span data-ttu-id="b7851-945">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-945">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-946">baseUri</span><span class="sxs-lookup"><span data-stu-id="b7851-946">baseUri</span></span> |<span data-ttu-id="b7851-947">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-947">Yes</span></span> |<span data-ttu-id="b7851-948">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-948">string</span></span> |<span data-ttu-id="b7851-949">Řetězec základní identifikátor uri.</span><span class="sxs-lookup"><span data-stu-id="b7851-949">The base uri string.</span></span> |
| <span data-ttu-id="b7851-950">relativeUri</span><span class="sxs-lookup"><span data-stu-id="b7851-950">relativeUri</span></span> |<span data-ttu-id="b7851-951">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-951">Yes</span></span> |<span data-ttu-id="b7851-952">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-952">string</span></span> |<span data-ttu-id="b7851-953">Řetězec relativní identifikátor uri pro přidání do řetězce základní identifikátor uri.</span><span class="sxs-lookup"><span data-stu-id="b7851-953">The relative uri string to add to the base uri string.</span></span> |

<span data-ttu-id="b7851-954">Hodnota **baseUri** parametr může obsahovat konkrétní soubor, ale jenom základní cesta se používá při vytváření identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b7851-954">The value for the **baseUri** parameter can include a specific file, but only the base path is used when constructing the URI.</span></span> <span data-ttu-id="b7851-955">Například předávání `http://contoso.com/resources/azuredeploy.json` jako parametr výsledky baseUri v základní identifikátor URI služby `http://contoso.com/resources/`.</span><span class="sxs-lookup"><span data-stu-id="b7851-955">For example, passing `http://contoso.com/resources/azuredeploy.json` as the baseUri parameter results in a base URI of `http://contoso.com/resources/`.</span></span>

### <a name="return-value"></a><span data-ttu-id="b7851-956">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-956">Return value</span></span>

<span data-ttu-id="b7851-957">Řetězec představující absolutní identifikátor URI pro základní a relativní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b7851-957">A string representing the absolute URI for the base and relative values.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-958">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-958">Examples</span></span>

<span data-ttu-id="b7851-959">Následující příklad ukazuje, jak vytvořit odkaz na vnořené šablonu na základě hodnoty nadřazené šablony.</span><span class="sxs-lookup"><span data-stu-id="b7851-959">The following example shows how to construct a link to a nested template based on the value of the parent template.</span></span>

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

<span data-ttu-id="b7851-960">Následující příklad ukazuje, jak použít identifikátor uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b7851-960">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b7851-961">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-961">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-962">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-962">Name</span></span> | <span data-ttu-id="b7851-963">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-963">Type</span></span> | <span data-ttu-id="b7851-964">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-964">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-965">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-965">uriOutput</span></span> | <span data-ttu-id="b7851-966">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-966">String</span></span> | <span data-ttu-id="b7851-967">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-967">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b7851-968">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-968">componentOutput</span></span> | <span data-ttu-id="b7851-969">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-969">String</span></span> | <span data-ttu-id="b7851-970">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-970">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b7851-971">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-971">toStringOutput</span></span> | <span data-ttu-id="b7851-972">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-972">String</span></span> | <span data-ttu-id="b7851-973">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-973">http://contoso.com/resources/nested/azuredeploy.json</span></span> |

<a id="uricomponent" />

## <a name="uricomponent"></a><span data-ttu-id="b7851-974">uriComponent</span><span class="sxs-lookup"><span data-stu-id="b7851-974">uriComponent</span></span>
`uricomponent(stringToEncode)`

<span data-ttu-id="b7851-975">Kóduje identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b7851-975">Encodes a URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-976">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-976">Parameters</span></span>

| <span data-ttu-id="b7851-977">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-977">Parameter</span></span> | <span data-ttu-id="b7851-978">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-978">Required</span></span> | <span data-ttu-id="b7851-979">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-979">Type</span></span> | <span data-ttu-id="b7851-980">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-980">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-981">stringToEncode</span><span class="sxs-lookup"><span data-stu-id="b7851-981">stringToEncode</span></span> |<span data-ttu-id="b7851-982">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-982">Yes</span></span> |<span data-ttu-id="b7851-983">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-983">string</span></span> |<span data-ttu-id="b7851-984">Hodnota ke kódování.</span><span class="sxs-lookup"><span data-stu-id="b7851-984">The value to encode.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-985">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-985">Return value</span></span>

<span data-ttu-id="b7851-986">Řetězec identifikátoru URI kódovaný hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b7851-986">A string of the URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-987">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-987">Examples</span></span>

<span data-ttu-id="b7851-988">Následující příklad ukazuje, jak použít identifikátor uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b7851-988">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b7851-989">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-989">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-990">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-990">Name</span></span> | <span data-ttu-id="b7851-991">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-991">Type</span></span> | <span data-ttu-id="b7851-992">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-992">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-993">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-993">uriOutput</span></span> | <span data-ttu-id="b7851-994">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-994">String</span></span> | <span data-ttu-id="b7851-995">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-995">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b7851-996">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-996">componentOutput</span></span> | <span data-ttu-id="b7851-997">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-997">String</span></span> | <span data-ttu-id="b7851-998">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-998">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b7851-999">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-999">toStringOutput</span></span> | <span data-ttu-id="b7851-1000">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-1000">String</span></span> | <span data-ttu-id="b7851-1001">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-1001">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a><span data-ttu-id="b7851-1002">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="b7851-1002">uriComponentToString</span></span>
`uriComponentToString(uriEncodedString)`

<span data-ttu-id="b7851-1003">Vrátí že hodnotu kódovaný řetězec identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b7851-1003">Returns a string of a URI encoded value.</span></span>

### <a name="parameters"></a><span data-ttu-id="b7851-1004">Parametry</span><span class="sxs-lookup"><span data-stu-id="b7851-1004">Parameters</span></span>

| <span data-ttu-id="b7851-1005">Parametr</span><span class="sxs-lookup"><span data-stu-id="b7851-1005">Parameter</span></span> | <span data-ttu-id="b7851-1006">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b7851-1006">Required</span></span> | <span data-ttu-id="b7851-1007">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-1007">Type</span></span> | <span data-ttu-id="b7851-1008">Popis</span><span class="sxs-lookup"><span data-stu-id="b7851-1008">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b7851-1009">uriEncodedString</span><span class="sxs-lookup"><span data-stu-id="b7851-1009">uriEncodedString</span></span> |<span data-ttu-id="b7851-1010">Ano</span><span class="sxs-lookup"><span data-stu-id="b7851-1010">Yes</span></span> |<span data-ttu-id="b7851-1011">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-1011">string</span></span> |<span data-ttu-id="b7851-1012">Identifikátor URI kódovaný hodnotu převést na řetězec.</span><span class="sxs-lookup"><span data-stu-id="b7851-1012">The URI encoded value to convert to a string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b7851-1013">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-1013">Return value</span></span>

<span data-ttu-id="b7851-1014">Řetězec dekódované identifikátoru URI kódovaný hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b7851-1014">A decoded string of URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="b7851-1015">Příklady</span><span class="sxs-lookup"><span data-stu-id="b7851-1015">Examples</span></span>

<span data-ttu-id="b7851-1016">Následující příklad ukazuje, jak použít identifikátor uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b7851-1016">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b7851-1017">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="b7851-1017">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="b7851-1018">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b7851-1018">Name</span></span> | <span data-ttu-id="b7851-1019">Typ</span><span class="sxs-lookup"><span data-stu-id="b7851-1019">Type</span></span> | <span data-ttu-id="b7851-1020">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b7851-1020">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b7851-1021">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-1021">uriOutput</span></span> | <span data-ttu-id="b7851-1022">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-1022">String</span></span> | <span data-ttu-id="b7851-1023">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-1023">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b7851-1024">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-1024">componentOutput</span></span> | <span data-ttu-id="b7851-1025">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-1025">String</span></span> | <span data-ttu-id="b7851-1026">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-1026">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b7851-1027">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b7851-1027">toStringOutput</span></span> | <span data-ttu-id="b7851-1028">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b7851-1028">String</span></span> | <span data-ttu-id="b7851-1029">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b7851-1029">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


## <a name="next-steps"></a><span data-ttu-id="b7851-1030">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7851-1030">Next steps</span></span>
* <span data-ttu-id="b7851-1031">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b7851-1031">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b7851-1032">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b7851-1032">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="b7851-1033">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="b7851-1033">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="b7851-1034">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b7851-1034">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

