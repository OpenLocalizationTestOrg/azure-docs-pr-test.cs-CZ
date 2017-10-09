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
# <a name="string-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="b3da9-103">Řetězcové funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b3da9-103">String functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="b3da9-104">Resource Manager poskytuje následující funkce pro práci s řetězci hello:</span><span class="sxs-lookup"><span data-stu-id="b3da9-104">Resource Manager provides hello following functions for working with strings:</span></span>

* [<span data-ttu-id="b3da9-105">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-105">base64</span></span>](#base64)
* [<span data-ttu-id="b3da9-106">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="b3da9-106">base64ToJson</span></span>](#base64tojson)
* [<span data-ttu-id="b3da9-107">base64ToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-107">base64ToString</span></span>](#base64tostring)
* [<span data-ttu-id="b3da9-108">concat</span><span class="sxs-lookup"><span data-stu-id="b3da9-108">concat</span></span>](#concat)
* [<span data-ttu-id="b3da9-109">obsahuje</span><span class="sxs-lookup"><span data-stu-id="b3da9-109">contains</span></span>](#contains)
* [<span data-ttu-id="b3da9-110">dataUri</span><span class="sxs-lookup"><span data-stu-id="b3da9-110">dataUri</span></span>](#datauri)
* [<span data-ttu-id="b3da9-111">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-111">dataUriToString</span></span>](#datauritostring)
* [<span data-ttu-id="b3da9-112">prázdný</span><span class="sxs-lookup"><span data-stu-id="b3da9-112">empty</span></span>](#empty)
* [<span data-ttu-id="b3da9-113">endsWith</span><span class="sxs-lookup"><span data-stu-id="b3da9-113">endsWith</span></span>](#endswith)
* [<span data-ttu-id="b3da9-114">první</span><span class="sxs-lookup"><span data-stu-id="b3da9-114">first</span></span>](#first)
* [<span data-ttu-id="b3da9-115">indexOf</span><span class="sxs-lookup"><span data-stu-id="b3da9-115">indexOf</span></span>](#indexof)
* [<span data-ttu-id="b3da9-116">poslední</span><span class="sxs-lookup"><span data-stu-id="b3da9-116">last</span></span>](#last)
* [<span data-ttu-id="b3da9-117">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="b3da9-117">lastIndexOf</span></span>](#lastindexof)
* [<span data-ttu-id="b3da9-118">Délka</span><span class="sxs-lookup"><span data-stu-id="b3da9-118">length</span></span>](#length)
* [<span data-ttu-id="b3da9-119">padLeft</span><span class="sxs-lookup"><span data-stu-id="b3da9-119">padLeft</span></span>](#padleft)
* [<span data-ttu-id="b3da9-120">nahradit</span><span class="sxs-lookup"><span data-stu-id="b3da9-120">replace</span></span>](#replace)
* [<span data-ttu-id="b3da9-121">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="b3da9-121">skip</span></span>](#skip)
* [<span data-ttu-id="b3da9-122">split</span><span class="sxs-lookup"><span data-stu-id="b3da9-122">split</span></span>](#split)
* [<span data-ttu-id="b3da9-123">startsWith</span><span class="sxs-lookup"><span data-stu-id="b3da9-123">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="b3da9-124">řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-124">string</span></span>](#string)
* [<span data-ttu-id="b3da9-125">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-125">substring</span></span>](#substring)
* [<span data-ttu-id="b3da9-126">proveďte</span><span class="sxs-lookup"><span data-stu-id="b3da9-126">take</span></span>](#take)
* [<span data-ttu-id="b3da9-127">toLower</span><span class="sxs-lookup"><span data-stu-id="b3da9-127">toLower</span></span>](#tolower)
* [<span data-ttu-id="b3da9-128">toUpper</span><span class="sxs-lookup"><span data-stu-id="b3da9-128">toUpper</span></span>](#toupper)
* [<span data-ttu-id="b3da9-129">uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="b3da9-129">trim</span></span>](#trim)
* [<span data-ttu-id="b3da9-130">uniqueString</span><span class="sxs-lookup"><span data-stu-id="b3da9-130">uniqueString</span></span>](#uniquestring)
* [<span data-ttu-id="b3da9-131">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="b3da9-131">uri</span></span>](#uri)
* [<span data-ttu-id="b3da9-132">uriComponent</span><span class="sxs-lookup"><span data-stu-id="b3da9-132">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="b3da9-133">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-133">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a><span data-ttu-id="b3da9-134">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-134">base64</span></span>
`base64(inputString)`

<span data-ttu-id="b3da9-135">Vrátí hello reprezentace hello vstupní řetězec base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-135">Returns hello base64 representation of hello input string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-136">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-136">Parameters</span></span>

| <span data-ttu-id="b3da9-137">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-137">Parameter</span></span> | <span data-ttu-id="b3da9-138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-138">Required</span></span> | <span data-ttu-id="b3da9-139">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-139">Type</span></span> | <span data-ttu-id="b3da9-140">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-140">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-141">inputString</span><span class="sxs-lookup"><span data-stu-id="b3da9-141">inputString</span></span> |<span data-ttu-id="b3da9-142">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-142">Yes</span></span> |<span data-ttu-id="b3da9-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-143">string</span></span> |<span data-ttu-id="b3da9-144">Hodnota tooreturn Hello jako znázornění base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-144">hello value tooreturn as a base64 representation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-145">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-145">Return value</span></span>

<span data-ttu-id="b3da9-146">Řetězec obsahující reprezentace hello base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-146">A string containing hello base64 representation.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-147">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-147">Examples</span></span>

<span data-ttu-id="b3da9-148">Hello následující příklad ukazuje, jak toouse hello funkce base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-148">hello following example shows how toouse hello base64 function.</span></span>

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

<span data-ttu-id="b3da9-149">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-149">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-150">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-150">Name</span></span> | <span data-ttu-id="b3da9-151">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-151">Type</span></span> | <span data-ttu-id="b3da9-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-153">base64Output</span><span class="sxs-lookup"><span data-stu-id="b3da9-153">base64Output</span></span> | <span data-ttu-id="b3da9-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-154">String</span></span> | <span data-ttu-id="b3da9-155">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b3da9-155">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b3da9-156">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-156">toStringOutput</span></span> | <span data-ttu-id="b3da9-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-157">String</span></span> | <span data-ttu-id="b3da9-158">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-158">one, two, three</span></span> |
| <span data-ttu-id="b3da9-159">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-159">toJsonOutput</span></span> | <span data-ttu-id="b3da9-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="b3da9-160">Object</span></span> | <span data-ttu-id="b3da9-161">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b3da9-161">{"one": "a", "two": "b"}</span></span> |

<a id="base64tojson" />

## <a name="base64tojson"></a><span data-ttu-id="b3da9-162">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="b3da9-162">base64ToJson</span></span>
`base64tojson`

<span data-ttu-id="b3da9-163">Převede objekt base64 tooa reprezentaci JSON.</span><span class="sxs-lookup"><span data-stu-id="b3da9-163">Converts a base64 representation tooa JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-164">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-164">Parameters</span></span>

| <span data-ttu-id="b3da9-165">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-165">Parameter</span></span> | <span data-ttu-id="b3da9-166">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-166">Required</span></span> | <span data-ttu-id="b3da9-167">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-167">Type</span></span> | <span data-ttu-id="b3da9-168">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-168">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-169">base64Value</span><span class="sxs-lookup"><span data-stu-id="b3da9-169">base64Value</span></span> |<span data-ttu-id="b3da9-170">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-170">Yes</span></span> |<span data-ttu-id="b3da9-171">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-171">string</span></span> |<span data-ttu-id="b3da9-172">Hello base64 reprezentace tooconvert tooa objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="b3da9-172">hello base64 representation tooconvert tooa JSON object.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-173">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-173">Return value</span></span>

<span data-ttu-id="b3da9-174">Objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="b3da9-174">A JSON object.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-175">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-175">Examples</span></span>

<span data-ttu-id="b3da9-176">Hello následující příklad používá hello base64ToJson funkce tooconvert hodnotu base64:</span><span class="sxs-lookup"><span data-stu-id="b3da9-176">hello following example uses hello base64ToJson function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="b3da9-177">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-177">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-178">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-178">Name</span></span> | <span data-ttu-id="b3da9-179">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-179">Type</span></span> | <span data-ttu-id="b3da9-180">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-180">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-181">base64Output</span><span class="sxs-lookup"><span data-stu-id="b3da9-181">base64Output</span></span> | <span data-ttu-id="b3da9-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-182">String</span></span> | <span data-ttu-id="b3da9-183">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b3da9-183">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b3da9-184">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-184">toStringOutput</span></span> | <span data-ttu-id="b3da9-185">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-185">String</span></span> | <span data-ttu-id="b3da9-186">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-186">one, two, three</span></span> |
| <span data-ttu-id="b3da9-187">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-187">toJsonOutput</span></span> | <span data-ttu-id="b3da9-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="b3da9-188">Object</span></span> | <span data-ttu-id="b3da9-189">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b3da9-189">{"one": "a", "two": "b"}</span></span> |

<a id="base64tostring" />

## <a name="base64tostring"></a><span data-ttu-id="b3da9-190">base64ToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-190">base64ToString</span></span>
`base64ToString(base64Value)`

<span data-ttu-id="b3da9-191">Převede řetězec base64 reprezentace tooa.</span><span class="sxs-lookup"><span data-stu-id="b3da9-191">Converts a base64 representation tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-192">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-192">Parameters</span></span>

| <span data-ttu-id="b3da9-193">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-193">Parameter</span></span> | <span data-ttu-id="b3da9-194">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-194">Required</span></span> | <span data-ttu-id="b3da9-195">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-195">Type</span></span> | <span data-ttu-id="b3da9-196">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-196">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-197">base64Value</span><span class="sxs-lookup"><span data-stu-id="b3da9-197">base64Value</span></span> |<span data-ttu-id="b3da9-198">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-198">Yes</span></span> |<span data-ttu-id="b3da9-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-199">string</span></span> |<span data-ttu-id="b3da9-200">Hello base64 reprezentace tooconvert tooa řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-200">hello base64 representation tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-201">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-201">Return value</span></span>

<span data-ttu-id="b3da9-202">Řetězec hello převést hodnotu base64.</span><span class="sxs-lookup"><span data-stu-id="b3da9-202">A string of hello converted base64 value.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-203">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-203">Examples</span></span>

<span data-ttu-id="b3da9-204">Hello následující příklad používá hello base64ToString funkce tooconvert hodnotu base64:</span><span class="sxs-lookup"><span data-stu-id="b3da9-204">hello following example uses hello base64ToString function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="b3da9-205">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-205">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-206">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-206">Name</span></span> | <span data-ttu-id="b3da9-207">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-207">Type</span></span> | <span data-ttu-id="b3da9-208">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-208">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-209">base64Output</span><span class="sxs-lookup"><span data-stu-id="b3da9-209">base64Output</span></span> | <span data-ttu-id="b3da9-210">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-210">String</span></span> | <span data-ttu-id="b3da9-211">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="b3da9-211">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="b3da9-212">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-212">toStringOutput</span></span> | <span data-ttu-id="b3da9-213">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-213">String</span></span> | <span data-ttu-id="b3da9-214">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-214">one, two, three</span></span> |
| <span data-ttu-id="b3da9-215">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-215">toJsonOutput</span></span> | <span data-ttu-id="b3da9-216">Objekt</span><span class="sxs-lookup"><span data-stu-id="b3da9-216">Object</span></span> | <span data-ttu-id="b3da9-217">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="b3da9-217">{"one": "a", "two": "b"}</span></span> |



<a id="concat" />

## <a name="concat"></a><span data-ttu-id="b3da9-218">concat</span><span class="sxs-lookup"><span data-stu-id="b3da9-218">concat</span></span>
`concat (arg1, arg2, arg3, ...)`

<span data-ttu-id="b3da9-219">Kombinuje více řetězcové hodnoty a vrátí řetězec hello zřetězených nebo kombinuje několik polí a vrací hello zřetězených pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-219">Combines multiple string values and returns hello concatenated string, or combines multiple arrays and returns hello concatenated array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-220">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-220">Parameters</span></span>

| <span data-ttu-id="b3da9-221">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-221">Parameter</span></span> | <span data-ttu-id="b3da9-222">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-222">Required</span></span> | <span data-ttu-id="b3da9-223">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-223">Type</span></span> | <span data-ttu-id="b3da9-224">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-224">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-225">arg1</span><span class="sxs-lookup"><span data-stu-id="b3da9-225">arg1</span></span> |<span data-ttu-id="b3da9-226">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-226">Yes</span></span> |<span data-ttu-id="b3da9-227">řetězec nebo pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-227">string or array</span></span> |<span data-ttu-id="b3da9-228">Hello první hodnota zřetězení.</span><span class="sxs-lookup"><span data-stu-id="b3da9-228">hello first value for concatenation.</span></span> |
| <span data-ttu-id="b3da9-229">Další argumenty</span><span class="sxs-lookup"><span data-stu-id="b3da9-229">additional arguments</span></span> |<span data-ttu-id="b3da9-230">Ne</span><span class="sxs-lookup"><span data-stu-id="b3da9-230">No</span></span> |<span data-ttu-id="b3da9-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-231">string</span></span> |<span data-ttu-id="b3da9-232">Další hodnoty v sekvenčním pořadí pro zřetězení.</span><span class="sxs-lookup"><span data-stu-id="b3da9-232">Additional values in sequential order for concatenation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-233">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-233">Return value</span></span>
<span data-ttu-id="b3da9-234">Řetězec nebo pole zřetězených hodnot.</span><span class="sxs-lookup"><span data-stu-id="b3da9-234">A string or array of concatenated values.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-235">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-235">Examples</span></span>

<span data-ttu-id="b3da9-236">Hello následující příklad ukazuje, jak toocombine dva řetězce hodnoty a vrátí spojený řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-236">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="b3da9-237">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-237">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-238">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-238">Name</span></span> | <span data-ttu-id="b3da9-239">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-239">Type</span></span> | <span data-ttu-id="b3da9-240">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-240">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-241">concatOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-241">concatOutput</span></span> | <span data-ttu-id="b3da9-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-242">String</span></span> | <span data-ttu-id="b3da9-243">Předpona 5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="b3da9-243">prefix-5yj4yjf5mbg72</span></span> |

<span data-ttu-id="b3da9-244">Hello následující příklad ukazuje, jak maticových toocombine dva.</span><span class="sxs-lookup"><span data-stu-id="b3da9-244">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="b3da9-245">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-246">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-246">Name</span></span> | <span data-ttu-id="b3da9-247">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-247">Type</span></span> | <span data-ttu-id="b3da9-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-249">Vrátí</span><span class="sxs-lookup"><span data-stu-id="b3da9-249">return</span></span> | <span data-ttu-id="b3da9-250">Pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-250">Array</span></span> | <span data-ttu-id="b3da9-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="b3da9-252">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="b3da9-252">contains</span></span>
`contains (container, itemToFind)`

<span data-ttu-id="b3da9-253">Kontroluje, zda pole obsahuje hodnotu, objekt obsahuje klíč nebo řetězec obsahuje dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-253">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-254">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-254">Parameters</span></span>

| <span data-ttu-id="b3da9-255">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-255">Parameter</span></span> | <span data-ttu-id="b3da9-256">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-256">Required</span></span> | <span data-ttu-id="b3da9-257">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-257">Type</span></span> | <span data-ttu-id="b3da9-258">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-258">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-259">kontejner</span><span class="sxs-lookup"><span data-stu-id="b3da9-259">container</span></span> |<span data-ttu-id="b3da9-260">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-260">Yes</span></span> |<span data-ttu-id="b3da9-261">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-261">array, object, or string</span></span> |<span data-ttu-id="b3da9-262">Hello hodnotu, která obsahuje hodnotu toofind hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-262">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="b3da9-263">itemToFind</span><span class="sxs-lookup"><span data-stu-id="b3da9-263">itemToFind</span></span> |<span data-ttu-id="b3da9-264">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-264">Yes</span></span> |<span data-ttu-id="b3da9-265">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-265">string or int</span></span> |<span data-ttu-id="b3da9-266">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-266">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-267">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-267">Return value</span></span>

<span data-ttu-id="b3da9-268">**Hodnota TRUE,** Pokud je položka hello, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="b3da9-268">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-269">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-269">Examples</span></span>

<span data-ttu-id="b3da9-270">Hello následující příklad ukazuje, jak toouse obsahuje s různými typy:</span><span class="sxs-lookup"><span data-stu-id="b3da9-270">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="b3da9-271">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-271">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-272">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-272">Name</span></span> | <span data-ttu-id="b3da9-273">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-273">Type</span></span> | <span data-ttu-id="b3da9-274">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-274">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-275">stringTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-275">stringTrue</span></span> | <span data-ttu-id="b3da9-276">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-276">Bool</span></span> | <span data-ttu-id="b3da9-277">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-277">True</span></span> |
| <span data-ttu-id="b3da9-278">stringFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-278">stringFalse</span></span> | <span data-ttu-id="b3da9-279">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-279">Bool</span></span> | <span data-ttu-id="b3da9-280">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-280">False</span></span> |
| <span data-ttu-id="b3da9-281">objectTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-281">objectTrue</span></span> | <span data-ttu-id="b3da9-282">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-282">Bool</span></span> | <span data-ttu-id="b3da9-283">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-283">True</span></span> |
| <span data-ttu-id="b3da9-284">objectFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-284">objectFalse</span></span> | <span data-ttu-id="b3da9-285">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-285">Bool</span></span> | <span data-ttu-id="b3da9-286">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-286">False</span></span> |
| <span data-ttu-id="b3da9-287">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-287">arrayTrue</span></span> | <span data-ttu-id="b3da9-288">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-288">Bool</span></span> | <span data-ttu-id="b3da9-289">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-289">True</span></span> |
| <span data-ttu-id="b3da9-290">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-290">arrayFalse</span></span> | <span data-ttu-id="b3da9-291">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-291">Bool</span></span> | <span data-ttu-id="b3da9-292">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-292">False</span></span> |

<a id="datauri" />

## <a name="datauri"></a><span data-ttu-id="b3da9-293">dataUri</span><span class="sxs-lookup"><span data-stu-id="b3da9-293">dataUri</span></span>
`dataUri(stringToConvert)`

<span data-ttu-id="b3da9-294">Převede data tooa hodnota identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-294">Converts a value tooa data URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-295">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-295">Parameters</span></span>

| <span data-ttu-id="b3da9-296">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-296">Parameter</span></span> | <span data-ttu-id="b3da9-297">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-297">Required</span></span> | <span data-ttu-id="b3da9-298">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-298">Type</span></span> | <span data-ttu-id="b3da9-299">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-299">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-300">stringToConvert</span><span class="sxs-lookup"><span data-stu-id="b3da9-300">stringToConvert</span></span> |<span data-ttu-id="b3da9-301">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-301">Yes</span></span> |<span data-ttu-id="b3da9-302">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-302">string</span></span> |<span data-ttu-id="b3da9-303">Hello tooconvert tooa údaj hodnoty identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-303">hello value tooconvert tooa data URI.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-304">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-304">Return value</span></span>

<span data-ttu-id="b3da9-305">Řetězec formátu dat identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-305">A string formatted as a data URI.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-306">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-306">Examples</span></span>

<span data-ttu-id="b3da9-307">Následující ukázka Hello převede data tooa hodnota identifikátoru URI a převede data řetězce tooa URI:</span><span class="sxs-lookup"><span data-stu-id="b3da9-307">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="b3da9-308">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-308">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-309">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-309">Name</span></span> | <span data-ttu-id="b3da9-310">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-310">Type</span></span> | <span data-ttu-id="b3da9-311">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-311">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-312">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-312">dataUriOutput</span></span> | <span data-ttu-id="b3da9-313">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-313">String</span></span> | <span data-ttu-id="b3da9-314">data: text / prostý; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="b3da9-314">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="b3da9-315">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-315">toStringOutput</span></span> | <span data-ttu-id="b3da9-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-316">String</span></span> | <span data-ttu-id="b3da9-317">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="b3da9-317">Hello, World!</span></span> |

<a id="datauritostring" />

## <a name="datauritostring"></a><span data-ttu-id="b3da9-318">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-318">dataUriToString</span></span>
`dataUriToString(dataUriToConvert)`

<span data-ttu-id="b3da9-319">Hodnota tooa řetězec ve formátu převádí data identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-319">Converts a data URI formatted value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-320">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-320">Parameters</span></span>

| <span data-ttu-id="b3da9-321">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-321">Parameter</span></span> | <span data-ttu-id="b3da9-322">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-322">Required</span></span> | <span data-ttu-id="b3da9-323">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-323">Type</span></span> | <span data-ttu-id="b3da9-324">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-324">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-325">dataUriToConvert</span><span class="sxs-lookup"><span data-stu-id="b3da9-325">dataUriToConvert</span></span> |<span data-ttu-id="b3da9-326">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-326">Yes</span></span> |<span data-ttu-id="b3da9-327">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-327">string</span></span> |<span data-ttu-id="b3da9-328">data Hello tooconvert hodnota identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-328">hello data URI value tooconvert.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-329">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-329">Return value</span></span>

<span data-ttu-id="b3da9-330">Řetězec obsahující hello převést hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-330">A string containing hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-331">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-331">Examples</span></span>

<span data-ttu-id="b3da9-332">Následující ukázka Hello převede data tooa hodnota identifikátoru URI a převede data řetězce tooa URI:</span><span class="sxs-lookup"><span data-stu-id="b3da9-332">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="b3da9-333">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-333">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-334">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-334">Name</span></span> | <span data-ttu-id="b3da9-335">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-335">Type</span></span> | <span data-ttu-id="b3da9-336">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-336">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-337">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-337">dataUriOutput</span></span> | <span data-ttu-id="b3da9-338">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-338">String</span></span> | <span data-ttu-id="b3da9-339">data: text / prostý; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="b3da9-339">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="b3da9-340">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-340">toStringOutput</span></span> | <span data-ttu-id="b3da9-341">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-341">String</span></span> | <span data-ttu-id="b3da9-342">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="b3da9-342">Hello, World!</span></span> |

<a id="empty" /> 

## <a name="empty"></a><span data-ttu-id="b3da9-343">prázdný</span><span class="sxs-lookup"><span data-stu-id="b3da9-343">empty</span></span>
`empty(itemToTest)`

<span data-ttu-id="b3da9-344">Určuje, zda je prázdný řetězec, objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-344">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-345">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-345">Parameters</span></span>

| <span data-ttu-id="b3da9-346">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-346">Parameter</span></span> | <span data-ttu-id="b3da9-347">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-347">Required</span></span> | <span data-ttu-id="b3da9-348">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-348">Type</span></span> | <span data-ttu-id="b3da9-349">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-349">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-350">itemToTest</span><span class="sxs-lookup"><span data-stu-id="b3da9-350">itemToTest</span></span> |<span data-ttu-id="b3da9-351">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-351">Yes</span></span> |<span data-ttu-id="b3da9-352">pole, objekt nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-352">array, object, or string</span></span> |<span data-ttu-id="b3da9-353">Hodnota toocheck Hello, pokud je prázdná.</span><span class="sxs-lookup"><span data-stu-id="b3da9-353">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-354">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-354">Return value</span></span>

<span data-ttu-id="b3da9-355">Vrátí **True** Pokud hello hodnota je prázdný, jinak hodnota **False**.</span><span class="sxs-lookup"><span data-stu-id="b3da9-355">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-356">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-356">Examples</span></span>

<span data-ttu-id="b3da9-357">Následující ukázka Hello ověří, zda jsou řetězec, objekt a pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="b3da9-357">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="b3da9-358">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-358">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-359">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-359">Name</span></span> | <span data-ttu-id="b3da9-360">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-360">Type</span></span> | <span data-ttu-id="b3da9-361">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-361">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-362">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="b3da9-362">arrayEmpty</span></span> | <span data-ttu-id="b3da9-363">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-363">Bool</span></span> | <span data-ttu-id="b3da9-364">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-364">True</span></span> |
| <span data-ttu-id="b3da9-365">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="b3da9-365">objectEmpty</span></span> | <span data-ttu-id="b3da9-366">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-366">Bool</span></span> | <span data-ttu-id="b3da9-367">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-367">True</span></span> |
| <span data-ttu-id="b3da9-368">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="b3da9-368">stringEmpty</span></span> | <span data-ttu-id="b3da9-369">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-369">Bool</span></span> | <span data-ttu-id="b3da9-370">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-370">True</span></span> |

<a id="endswith" />

## <a name="endswith"></a><span data-ttu-id="b3da9-371">endsWith</span><span class="sxs-lookup"><span data-stu-id="b3da9-371">endsWith</span></span>
`endsWith(stringToSearch, stringToFind)`

<span data-ttu-id="b3da9-372">Určuje, zda řetězec končí s hodnotou.</span><span class="sxs-lookup"><span data-stu-id="b3da9-372">Determines whether a string ends with a value.</span></span> <span data-ttu-id="b3da9-373">Hello porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b3da9-373">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-374">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-374">Parameters</span></span>

| <span data-ttu-id="b3da9-375">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-375">Parameter</span></span> | <span data-ttu-id="b3da9-376">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-376">Required</span></span> | <span data-ttu-id="b3da9-377">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-377">Type</span></span> | <span data-ttu-id="b3da9-378">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-378">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-379">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b3da9-379">stringToSearch</span></span> |<span data-ttu-id="b3da9-380">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-380">Yes</span></span> |<span data-ttu-id="b3da9-381">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-381">string</span></span> |<span data-ttu-id="b3da9-382">Hello hodnotu, která obsahuje položky toofind hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-382">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="b3da9-383">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b3da9-383">stringToFind</span></span> |<span data-ttu-id="b3da9-384">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-384">Yes</span></span> |<span data-ttu-id="b3da9-385">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-385">string</span></span> |<span data-ttu-id="b3da9-386">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-386">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-387">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-387">Return value</span></span>

<span data-ttu-id="b3da9-388">**Hodnota TRUE,** Pokud hello poslední znak nebo znaky řetězce hello odpovídají hello hodnotu; jinak, **False**.</span><span class="sxs-lookup"><span data-stu-id="b3da9-388">**True** if hello last character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-389">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-389">Examples</span></span>

<span data-ttu-id="b3da9-390">Hello následující příklad ukazuje, jak toouse hello funkce startsWith a endsWith:</span><span class="sxs-lookup"><span data-stu-id="b3da9-390">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="b3da9-391">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-391">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-392">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-392">Name</span></span> | <span data-ttu-id="b3da9-393">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-393">Type</span></span> | <span data-ttu-id="b3da9-394">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-394">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-395">startsTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-395">startsTrue</span></span> | <span data-ttu-id="b3da9-396">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-396">Bool</span></span> | <span data-ttu-id="b3da9-397">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-397">True</span></span> |
| <span data-ttu-id="b3da9-398">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-398">startsCapTrue</span></span> | <span data-ttu-id="b3da9-399">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-399">Bool</span></span> | <span data-ttu-id="b3da9-400">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-400">True</span></span> |
| <span data-ttu-id="b3da9-401">startsFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-401">startsFalse</span></span> | <span data-ttu-id="b3da9-402">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-402">Bool</span></span> | <span data-ttu-id="b3da9-403">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-403">False</span></span> |
| <span data-ttu-id="b3da9-404">endsTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-404">endsTrue</span></span> | <span data-ttu-id="b3da9-405">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-405">Bool</span></span> | <span data-ttu-id="b3da9-406">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-406">True</span></span> |
| <span data-ttu-id="b3da9-407">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-407">endsCapTrue</span></span> | <span data-ttu-id="b3da9-408">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-408">Bool</span></span> | <span data-ttu-id="b3da9-409">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-409">True</span></span> |
| <span data-ttu-id="b3da9-410">endsFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-410">endsFalse</span></span> | <span data-ttu-id="b3da9-411">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-411">Bool</span></span> | <span data-ttu-id="b3da9-412">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-412">False</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="b3da9-413">první</span><span class="sxs-lookup"><span data-stu-id="b3da9-413">first</span></span>
`first(arg1)`

<span data-ttu-id="b3da9-414">Vrátí hello první znak řetězce hello nebo první prvek pole hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-414">Returns hello first character of hello string, or first element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-415">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-415">Parameters</span></span>

| <span data-ttu-id="b3da9-416">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-416">Parameter</span></span> | <span data-ttu-id="b3da9-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-417">Required</span></span> | <span data-ttu-id="b3da9-418">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-418">Type</span></span> | <span data-ttu-id="b3da9-419">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-420">arg1</span><span class="sxs-lookup"><span data-stu-id="b3da9-420">arg1</span></span> |<span data-ttu-id="b3da9-421">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-421">Yes</span></span> |<span data-ttu-id="b3da9-422">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-422">array or string</span></span> |<span data-ttu-id="b3da9-423">Hello hodnota tooretrieve hello první prvek nebo znak.</span><span class="sxs-lookup"><span data-stu-id="b3da9-423">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-424">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-424">Return value</span></span>

<span data-ttu-id="b3da9-425">Řetězec hello první znak nebo typ hello (řetězec, int, pole nebo objekt) hello první prvek v poli.</span><span class="sxs-lookup"><span data-stu-id="b3da9-425">A string of hello first character, or hello type (string, int, array, or object) of hello first element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-426">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-426">Examples</span></span>

<span data-ttu-id="b3da9-427">Hello následující příklad ukazuje, jak toouse hello první funkce s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-427">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="b3da9-428">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-429">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-429">Name</span></span> | <span data-ttu-id="b3da9-430">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-430">Type</span></span> | <span data-ttu-id="b3da9-431">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-432">arrayOutput</span></span> | <span data-ttu-id="b3da9-433">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-433">String</span></span> | <span data-ttu-id="b3da9-434">jeden</span><span class="sxs-lookup"><span data-stu-id="b3da9-434">one</span></span> |
| <span data-ttu-id="b3da9-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-435">stringOutput</span></span> | <span data-ttu-id="b3da9-436">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-436">String</span></span> | <span data-ttu-id="b3da9-437">O</span><span class="sxs-lookup"><span data-stu-id="b3da9-437">O</span></span> |

<a id="indexof" />

## <a name="indexof"></a><span data-ttu-id="b3da9-438">indexOf</span><span class="sxs-lookup"><span data-stu-id="b3da9-438">indexOf</span></span>
`indexOf(stringToSearch, stringToFind)`

<span data-ttu-id="b3da9-439">Vrátí hello první pozici hodnoty v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b3da9-439">Returns hello first position of a value within a string.</span></span> <span data-ttu-id="b3da9-440">Hello porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b3da9-440">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-441">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-441">Parameters</span></span>

| <span data-ttu-id="b3da9-442">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-442">Parameter</span></span> | <span data-ttu-id="b3da9-443">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-443">Required</span></span> | <span data-ttu-id="b3da9-444">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-444">Type</span></span> | <span data-ttu-id="b3da9-445">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-445">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-446">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b3da9-446">stringToSearch</span></span> |<span data-ttu-id="b3da9-447">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-447">Yes</span></span> |<span data-ttu-id="b3da9-448">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-448">string</span></span> |<span data-ttu-id="b3da9-449">Hello hodnotu, která obsahuje položky toofind hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-449">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="b3da9-450">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b3da9-450">stringToFind</span></span> |<span data-ttu-id="b3da9-451">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-451">Yes</span></span> |<span data-ttu-id="b3da9-452">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-452">string</span></span> |<span data-ttu-id="b3da9-453">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-453">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-454">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-454">Return value</span></span>

<span data-ttu-id="b3da9-455">Celé číslo představující pozici hello toofind položky hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-455">An integer that represents hello position of hello item toofind.</span></span> <span data-ttu-id="b3da9-456">Hodnota Hello je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="b3da9-456">hello value is zero-based.</span></span> <span data-ttu-id="b3da9-457">Pokud hello položka není nalezena, se vrátí hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="b3da9-457">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-458">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-458">Examples</span></span>

<span data-ttu-id="b3da9-459">Hello následující příklad ukazuje, jak toouse hello funkce indexOf a lastIndexOf:</span><span class="sxs-lookup"><span data-stu-id="b3da9-459">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="b3da9-460">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-460">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-461">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-461">Name</span></span> | <span data-ttu-id="b3da9-462">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-462">Type</span></span> | <span data-ttu-id="b3da9-463">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-463">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-464">firstT</span><span class="sxs-lookup"><span data-stu-id="b3da9-464">firstT</span></span> | <span data-ttu-id="b3da9-465">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-465">Int</span></span> | <span data-ttu-id="b3da9-466">0</span><span class="sxs-lookup"><span data-stu-id="b3da9-466">0</span></span> |
| <span data-ttu-id="b3da9-467">lastT</span><span class="sxs-lookup"><span data-stu-id="b3da9-467">lastT</span></span> | <span data-ttu-id="b3da9-468">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-468">Int</span></span> | <span data-ttu-id="b3da9-469">3</span><span class="sxs-lookup"><span data-stu-id="b3da9-469">3</span></span> |
| <span data-ttu-id="b3da9-470">firstString</span><span class="sxs-lookup"><span data-stu-id="b3da9-470">firstString</span></span> | <span data-ttu-id="b3da9-471">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-471">Int</span></span> | <span data-ttu-id="b3da9-472">2</span><span class="sxs-lookup"><span data-stu-id="b3da9-472">2</span></span> |
| <span data-ttu-id="b3da9-473">lastString</span><span class="sxs-lookup"><span data-stu-id="b3da9-473">lastString</span></span> | <span data-ttu-id="b3da9-474">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-474">Int</span></span> | <span data-ttu-id="b3da9-475">0</span><span class="sxs-lookup"><span data-stu-id="b3da9-475">0</span></span> |
| <span data-ttu-id="b3da9-476">notFound</span><span class="sxs-lookup"><span data-stu-id="b3da9-476">notFound</span></span> | <span data-ttu-id="b3da9-477">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-477">Int</span></span> | <span data-ttu-id="b3da9-478">-1</span><span class="sxs-lookup"><span data-stu-id="b3da9-478">-1</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="b3da9-479">poslední</span><span class="sxs-lookup"><span data-stu-id="b3da9-479">last</span></span>
`last (arg1)`

<span data-ttu-id="b3da9-480">Vrátí poslední znak řetězce hello nebo hello posledním elementem pole hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-480">Returns last character of hello string, or hello last element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-481">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-481">Parameters</span></span>

| <span data-ttu-id="b3da9-482">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-482">Parameter</span></span> | <span data-ttu-id="b3da9-483">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-483">Required</span></span> | <span data-ttu-id="b3da9-484">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-484">Type</span></span> | <span data-ttu-id="b3da9-485">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-485">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-486">arg1</span><span class="sxs-lookup"><span data-stu-id="b3da9-486">arg1</span></span> |<span data-ttu-id="b3da9-487">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-487">Yes</span></span> |<span data-ttu-id="b3da9-488">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-488">array or string</span></span> |<span data-ttu-id="b3da9-489">Hello hodnota tooretrieve hello poslední element nebo znak.</span><span class="sxs-lookup"><span data-stu-id="b3da9-489">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-490">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-490">Return value</span></span>

<span data-ttu-id="b3da9-491">Řetězec hello poslední znak nebo typ hello (řetězec, int, pole nebo objekt) hello poslední prvek v poli.</span><span class="sxs-lookup"><span data-stu-id="b3da9-491">A string of hello last character, or hello type (string, int, array, or object) of hello last element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-492">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-492">Examples</span></span>

<span data-ttu-id="b3da9-493">Hello následující příklad ukazuje, jak toouse hello poslední funkci s řetězec a pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-493">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="b3da9-494">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-494">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-495">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-495">Name</span></span> | <span data-ttu-id="b3da9-496">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-496">Type</span></span> | <span data-ttu-id="b3da9-497">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-497">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-498">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-498">arrayOutput</span></span> | <span data-ttu-id="b3da9-499">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-499">String</span></span> | <span data-ttu-id="b3da9-500">tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-500">three</span></span> |
| <span data-ttu-id="b3da9-501">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-501">stringOutput</span></span> | <span data-ttu-id="b3da9-502">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-502">String</span></span> | <span data-ttu-id="b3da9-503">E</span><span class="sxs-lookup"><span data-stu-id="b3da9-503">e</span></span> |

<a id="lastindexof" />

## <a name="lastindexof"></a><span data-ttu-id="b3da9-504">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="b3da9-504">lastIndexOf</span></span>
`lastIndexOf(stringToSearch, stringToFind)`

<span data-ttu-id="b3da9-505">Vrátí hello poslední umístění hodnoty v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b3da9-505">Returns hello last position of a value within a string.</span></span> <span data-ttu-id="b3da9-506">Hello porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b3da9-506">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-507">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-507">Parameters</span></span>

| <span data-ttu-id="b3da9-508">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-508">Parameter</span></span> | <span data-ttu-id="b3da9-509">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-509">Required</span></span> | <span data-ttu-id="b3da9-510">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-510">Type</span></span> | <span data-ttu-id="b3da9-511">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-511">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-512">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b3da9-512">stringToSearch</span></span> |<span data-ttu-id="b3da9-513">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-513">Yes</span></span> |<span data-ttu-id="b3da9-514">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-514">string</span></span> |<span data-ttu-id="b3da9-515">Hello hodnotu, která obsahuje položky toofind hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-515">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="b3da9-516">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b3da9-516">stringToFind</span></span> |<span data-ttu-id="b3da9-517">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-517">Yes</span></span> |<span data-ttu-id="b3da9-518">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-518">string</span></span> |<span data-ttu-id="b3da9-519">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-519">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-520">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-520">Return value</span></span>

<span data-ttu-id="b3da9-521">Celé číslo, které představuje poslední pozice hello toofind položky hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-521">An integer that represents hello last position of hello item toofind.</span></span> <span data-ttu-id="b3da9-522">Hodnota Hello je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="b3da9-522">hello value is zero-based.</span></span> <span data-ttu-id="b3da9-523">Pokud hello položka není nalezena, se vrátí hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="b3da9-523">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-524">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-524">Examples</span></span>

<span data-ttu-id="b3da9-525">Hello následující příklad ukazuje, jak toouse hello funkce indexOf a lastIndexOf:</span><span class="sxs-lookup"><span data-stu-id="b3da9-525">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="b3da9-526">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-526">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-527">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-527">Name</span></span> | <span data-ttu-id="b3da9-528">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-528">Type</span></span> | <span data-ttu-id="b3da9-529">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-529">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-530">firstT</span><span class="sxs-lookup"><span data-stu-id="b3da9-530">firstT</span></span> | <span data-ttu-id="b3da9-531">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-531">Int</span></span> | <span data-ttu-id="b3da9-532">0</span><span class="sxs-lookup"><span data-stu-id="b3da9-532">0</span></span> |
| <span data-ttu-id="b3da9-533">lastT</span><span class="sxs-lookup"><span data-stu-id="b3da9-533">lastT</span></span> | <span data-ttu-id="b3da9-534">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-534">Int</span></span> | <span data-ttu-id="b3da9-535">3</span><span class="sxs-lookup"><span data-stu-id="b3da9-535">3</span></span> |
| <span data-ttu-id="b3da9-536">firstString</span><span class="sxs-lookup"><span data-stu-id="b3da9-536">firstString</span></span> | <span data-ttu-id="b3da9-537">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-537">Int</span></span> | <span data-ttu-id="b3da9-538">2</span><span class="sxs-lookup"><span data-stu-id="b3da9-538">2</span></span> |
| <span data-ttu-id="b3da9-539">lastString</span><span class="sxs-lookup"><span data-stu-id="b3da9-539">lastString</span></span> | <span data-ttu-id="b3da9-540">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-540">Int</span></span> | <span data-ttu-id="b3da9-541">0</span><span class="sxs-lookup"><span data-stu-id="b3da9-541">0</span></span> |
| <span data-ttu-id="b3da9-542">notFound</span><span class="sxs-lookup"><span data-stu-id="b3da9-542">notFound</span></span> | <span data-ttu-id="b3da9-543">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-543">Int</span></span> | <span data-ttu-id="b3da9-544">-1</span><span class="sxs-lookup"><span data-stu-id="b3da9-544">-1</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="b3da9-545">Délka</span><span class="sxs-lookup"><span data-stu-id="b3da9-545">length</span></span>
`length(string)`

<span data-ttu-id="b3da9-546">Vrátí hello počet znaků v řetězci nebo prvků v poli.</span><span class="sxs-lookup"><span data-stu-id="b3da9-546">Returns hello number of characters in a string, or elements in an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-547">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-547">Parameters</span></span>

| <span data-ttu-id="b3da9-548">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-548">Parameter</span></span> | <span data-ttu-id="b3da9-549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-549">Required</span></span> | <span data-ttu-id="b3da9-550">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-550">Type</span></span> | <span data-ttu-id="b3da9-551">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-551">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-552">arg1</span><span class="sxs-lookup"><span data-stu-id="b3da9-552">arg1</span></span> |<span data-ttu-id="b3da9-553">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-553">Yes</span></span> |<span data-ttu-id="b3da9-554">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-554">array or string</span></span> |<span data-ttu-id="b3da9-555">Hello toouse pole pro získání hello počet elementů nebo hello toouse řetězec pro získání hello počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-555">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-556">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-556">Return value</span></span>

<span data-ttu-id="b3da9-557">Typ int.</span><span class="sxs-lookup"><span data-stu-id="b3da9-557">An int.</span></span> 

### <a name="examples"></a><span data-ttu-id="b3da9-558">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-558">Examples</span></span>

<span data-ttu-id="b3da9-559">Následující příklad ukazuje, jak Hello toouse délka s řetězec a pole:</span><span class="sxs-lookup"><span data-stu-id="b3da9-559">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="b3da9-560">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-560">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-561">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-561">Name</span></span> | <span data-ttu-id="b3da9-562">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-562">Type</span></span> | <span data-ttu-id="b3da9-563">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-563">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-564">arrayLength</span><span class="sxs-lookup"><span data-stu-id="b3da9-564">arrayLength</span></span> | <span data-ttu-id="b3da9-565">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-565">Int</span></span> | <span data-ttu-id="b3da9-566">3</span><span class="sxs-lookup"><span data-stu-id="b3da9-566">3</span></span> |
| <span data-ttu-id="b3da9-567">stringLength</span><span class="sxs-lookup"><span data-stu-id="b3da9-567">stringLength</span></span> | <span data-ttu-id="b3da9-568">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-568">Int</span></span> | <span data-ttu-id="b3da9-569">13</span><span class="sxs-lookup"><span data-stu-id="b3da9-569">13</span></span> |

<a id="padleft" />

## <a name="padleft"></a><span data-ttu-id="b3da9-570">padLeft</span><span class="sxs-lookup"><span data-stu-id="b3da9-570">padLeft</span></span>
`padLeft(valueToPad, totalLength, paddingCharacter)`

<span data-ttu-id="b3da9-571">Vrací vpravo zarovnaný řetězec přidáním znaků toohello doleva až do dosažení celkové zadané délky hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-571">Returns a right-aligned string by adding characters toohello left until reaching hello total specified length.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-572">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-572">Parameters</span></span>

| <span data-ttu-id="b3da9-573">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-573">Parameter</span></span> | <span data-ttu-id="b3da9-574">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-574">Required</span></span> | <span data-ttu-id="b3da9-575">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-575">Type</span></span> | <span data-ttu-id="b3da9-576">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-576">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-577">valueToPad</span><span class="sxs-lookup"><span data-stu-id="b3da9-577">valueToPad</span></span> |<span data-ttu-id="b3da9-578">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-578">Yes</span></span> |<span data-ttu-id="b3da9-579">řetězec nebo celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-579">string or int</span></span> |<span data-ttu-id="b3da9-580">Hello hodnota tooright-zarovnat.</span><span class="sxs-lookup"><span data-stu-id="b3da9-580">hello value tooright-align.</span></span> |
| <span data-ttu-id="b3da9-581">Hodnota totalLength</span><span class="sxs-lookup"><span data-stu-id="b3da9-581">totalLength</span></span> |<span data-ttu-id="b3da9-582">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-582">Yes</span></span> |<span data-ttu-id="b3da9-583">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-583">int</span></span> |<span data-ttu-id="b3da9-584">Celkový počet znaků v hello Hello vrátil řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-584">hello total number of characters in hello returned string.</span></span> |
| <span data-ttu-id="b3da9-585">paddingCharacter</span><span class="sxs-lookup"><span data-stu-id="b3da9-585">paddingCharacter</span></span> |<span data-ttu-id="b3da9-586">Ne</span><span class="sxs-lookup"><span data-stu-id="b3da9-586">No</span></span> |<span data-ttu-id="b3da9-587">jeden znak</span><span class="sxs-lookup"><span data-stu-id="b3da9-587">single character</span></span> |<span data-ttu-id="b3da9-588">Hello toouse znak pro odsazení nalevo dokud nebude dosaženo celková délka hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-588">hello character toouse for left-padding until hello total length is reached.</span></span> <span data-ttu-id="b3da9-589">Hello výchozí hodnota je mezera.</span><span class="sxs-lookup"><span data-stu-id="b3da9-589">hello default value is a space.</span></span> |

<span data-ttu-id="b3da9-590">Pokud původní řetězec hello je delší než hello počet znaků toopad, přidají se žádné znaky.</span><span class="sxs-lookup"><span data-stu-id="b3da9-590">If hello original string is longer than hello number of characters toopad, no characters are added.</span></span>

### <a name="return-value"></a><span data-ttu-id="b3da9-591">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-591">Return value</span></span>

<span data-ttu-id="b3da9-592">Řetězec s minimálně hello počet zadaný znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-592">A string with at least hello number of specified characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-593">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-593">Examples</span></span>

<span data-ttu-id="b3da9-594">Hello následující příklad ukazuje, jak toopad hello hodnota parametru zadaný uživatelem přidáním hello nulové znak dokud nedosáhne hello celkový počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-594">hello following example shows how toopad hello user-provided parameter value by adding hello zero character until it reaches hello total number of characters.</span></span> 

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

<span data-ttu-id="b3da9-595">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-595">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-596">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-596">Name</span></span> | <span data-ttu-id="b3da9-597">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-597">Type</span></span> | <span data-ttu-id="b3da9-598">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-598">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-599">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-599">stringOutput</span></span> | <span data-ttu-id="b3da9-600">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-600">String</span></span> | <span data-ttu-id="b3da9-601">0000000123</span><span class="sxs-lookup"><span data-stu-id="b3da9-601">0000000123</span></span> |

<a id="replace" />

## <a name="replace"></a><span data-ttu-id="b3da9-602">Nahradit</span><span class="sxs-lookup"><span data-stu-id="b3da9-602">replace</span></span>
`replace(originalString, oldString, newString)`

<span data-ttu-id="b3da9-603">Vrátí nový řetězec se všechny instance jeden řetězec nahrazen jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b3da9-603">Returns a new string with all instances of one string replaced by another string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-604">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-604">Parameters</span></span>

| <span data-ttu-id="b3da9-605">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-605">Parameter</span></span> | <span data-ttu-id="b3da9-606">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-606">Required</span></span> | <span data-ttu-id="b3da9-607">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-607">Type</span></span> | <span data-ttu-id="b3da9-608">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-608">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-609">originalString</span><span class="sxs-lookup"><span data-stu-id="b3da9-609">originalString</span></span> |<span data-ttu-id="b3da9-610">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-610">Yes</span></span> |<span data-ttu-id="b3da9-611">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-611">string</span></span> |<span data-ttu-id="b3da9-612">Hello hodnotu, která obsahuje všechny instance jeden řetězec nahrazen jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b3da9-612">hello value that has all instances of one string replaced by another string.</span></span> |
| <span data-ttu-id="b3da9-613">oldString</span><span class="sxs-lookup"><span data-stu-id="b3da9-613">oldString</span></span> |<span data-ttu-id="b3da9-614">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-614">Yes</span></span> |<span data-ttu-id="b3da9-615">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-615">string</span></span> |<span data-ttu-id="b3da9-616">odebrat z původního řetězce hello toobe řetězec Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-616">hello string toobe removed from hello original string.</span></span> |
| <span data-ttu-id="b3da9-617">newstring –</span><span class="sxs-lookup"><span data-stu-id="b3da9-617">newString</span></span> |<span data-ttu-id="b3da9-618">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-618">Yes</span></span> |<span data-ttu-id="b3da9-619">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-619">string</span></span> |<span data-ttu-id="b3da9-620">řetězec tooadd Hello místo hello odebrat řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-620">hello string tooadd in place of hello removed string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-621">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-621">Return value</span></span>

<span data-ttu-id="b3da9-622">Řetězec s hello nahradit znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-622">A string with hello replaced characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-623">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-623">Examples</span></span>

<span data-ttu-id="b3da9-624">Hello následující příklad ukazuje, jak tooremove všechny pomlčky z řetězce hello zadaný uživatelem a jak tooreplace součástí hello jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b3da9-624">hello following example shows how tooremove all dashes from hello user-provided string, and how tooreplace part of hello string with another string.</span></span>

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

<span data-ttu-id="b3da9-625">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-625">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-626">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-626">Name</span></span> | <span data-ttu-id="b3da9-627">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-627">Type</span></span> | <span data-ttu-id="b3da9-628">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-628">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-629">firstOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-629">firstOutput</span></span> | <span data-ttu-id="b3da9-630">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-630">String</span></span> | <span data-ttu-id="b3da9-631">1231231234</span><span class="sxs-lookup"><span data-stu-id="b3da9-631">1231231234</span></span> |
| <span data-ttu-id="b3da9-632">secodeOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-632">secodeOutput</span></span> | <span data-ttu-id="b3da9-633">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-633">String</span></span> | <span data-ttu-id="b3da9-634">123-123-xxxx</span><span class="sxs-lookup"><span data-stu-id="b3da9-634">123-123-xxxx</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="b3da9-635">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="b3da9-635">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="b3da9-636">Vrátí řetězec s všechny znaky hello po hello zadaný počet znaků, nebo pole s všechny elementy hello po hello zadaný počet elementů.</span><span class="sxs-lookup"><span data-stu-id="b3da9-636">Returns a string with all hello characters after hello specified number of characters, or an array with all hello elements after hello specified number of elements.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-637">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-637">Parameters</span></span>

| <span data-ttu-id="b3da9-638">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-638">Parameter</span></span> | <span data-ttu-id="b3da9-639">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-639">Required</span></span> | <span data-ttu-id="b3da9-640">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-640">Type</span></span> | <span data-ttu-id="b3da9-641">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-641">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-642">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-642">originalValue</span></span> |<span data-ttu-id="b3da9-643">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-643">Yes</span></span> |<span data-ttu-id="b3da9-644">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-644">array or string</span></span> |<span data-ttu-id="b3da9-645">Hello toouse pro přeskočení pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-645">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="b3da9-646">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="b3da9-646">numberToSkip</span></span> |<span data-ttu-id="b3da9-647">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-647">Yes</span></span> |<span data-ttu-id="b3da9-648">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-648">int</span></span> |<span data-ttu-id="b3da9-649">Hello počet tooskip elementy nebo znaky.</span><span class="sxs-lookup"><span data-stu-id="b3da9-649">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="b3da9-650">Pokud tato hodnota je 0 nebo menší, všechny hello elementy nebo znaků hello hodnoty jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="b3da9-650">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="b3da9-651">Pokud je větší než délka hello hello pole nebo řetězec, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-651">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-652">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-652">Return value</span></span>

<span data-ttu-id="b3da9-653">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-653">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-654">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-654">Examples</span></span>

<span data-ttu-id="b3da9-655">Následující příklad přeskočí hello Hello zadaný počet prvků v poli hello a hello zadaný počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="b3da9-655">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="b3da9-656">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-656">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-657">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-657">Name</span></span> | <span data-ttu-id="b3da9-658">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-658">Type</span></span> | <span data-ttu-id="b3da9-659">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-659">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-660">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-660">arrayOutput</span></span> | <span data-ttu-id="b3da9-661">Pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-661">Array</span></span> | <span data-ttu-id="b3da9-662">["tři"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-662">["three"]</span></span> |
| <span data-ttu-id="b3da9-663">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-663">stringOutput</span></span> | <span data-ttu-id="b3da9-664">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-664">String</span></span> | <span data-ttu-id="b3da9-665">dvě tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-665">two three</span></span> |

<a id="split" />

## <a name="split"></a><span data-ttu-id="b3da9-666">split</span><span class="sxs-lookup"><span data-stu-id="b3da9-666">split</span></span>
`split(inputString, delimiter)`

<span data-ttu-id="b3da9-667">Vrací pole řetězců obsahující hello podřetězce hello vstupní řetězec, které jsou odděleny hello zadaného oddělovače.</span><span class="sxs-lookup"><span data-stu-id="b3da9-667">Returns an array of strings that contains hello substrings of hello input string that are delimited by hello specified delimiters.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-668">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-668">Parameters</span></span>

| <span data-ttu-id="b3da9-669">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-669">Parameter</span></span> | <span data-ttu-id="b3da9-670">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-670">Required</span></span> | <span data-ttu-id="b3da9-671">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-671">Type</span></span> | <span data-ttu-id="b3da9-672">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-672">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-673">inputString</span><span class="sxs-lookup"><span data-stu-id="b3da9-673">inputString</span></span> |<span data-ttu-id="b3da9-674">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-674">Yes</span></span> |<span data-ttu-id="b3da9-675">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-675">string</span></span> |<span data-ttu-id="b3da9-676">řetězec toosplit Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-676">hello string toosplit.</span></span> |
| <span data-ttu-id="b3da9-677">Oddělovač</span><span class="sxs-lookup"><span data-stu-id="b3da9-677">delimiter</span></span> |<span data-ttu-id="b3da9-678">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-678">Yes</span></span> |<span data-ttu-id="b3da9-679">řetězec nebo pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="b3da9-679">string or array of strings</span></span> |<span data-ttu-id="b3da9-680">Oddělovač toouse Hello k rozdělení hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-680">hello delimiter toouse for splitting hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-681">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-681">Return value</span></span>

<span data-ttu-id="b3da9-682">Pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="b3da9-682">An array of strings.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-683">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-683">Examples</span></span>

<span data-ttu-id="b3da9-684">Hello následující příklad rozdělí vstupní řetězec hello se čárkou a s čárkou nebo středníkem.</span><span class="sxs-lookup"><span data-stu-id="b3da9-684">hello following example splits hello input string with a comma, and with either a comma or a semi-colon.</span></span>

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

<span data-ttu-id="b3da9-685">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-685">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-686">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-686">Name</span></span> | <span data-ttu-id="b3da9-687">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-687">Type</span></span> | <span data-ttu-id="b3da9-688">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-688">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-689">firstOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-689">firstOutput</span></span> | <span data-ttu-id="b3da9-690">Pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-690">Array</span></span> | <span data-ttu-id="b3da9-691">["1", "dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-691">["one", "two", "three"]</span></span> |
| <span data-ttu-id="b3da9-692">secondOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-692">secondOutput</span></span> | <span data-ttu-id="b3da9-693">Pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-693">Array</span></span> | <span data-ttu-id="b3da9-694">["1", "dva", "tři"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-694">["one", "two", "three"]</span></span> |

<a id="startswith" />

## <a name="startswith"></a><span data-ttu-id="b3da9-695">startsWith</span><span class="sxs-lookup"><span data-stu-id="b3da9-695">startsWith</span></span>
`startsWith(stringToSearch, stringToFind)`

<span data-ttu-id="b3da9-696">Určuje, zda řetězec začíná hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-696">Determines whether a string starts with a value.</span></span> <span data-ttu-id="b3da9-697">Hello porovnání nerozlišuje malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b3da9-697">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-698">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-698">Parameters</span></span>

| <span data-ttu-id="b3da9-699">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-699">Parameter</span></span> | <span data-ttu-id="b3da9-700">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-700">Required</span></span> | <span data-ttu-id="b3da9-701">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-701">Type</span></span> | <span data-ttu-id="b3da9-702">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-702">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-703">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="b3da9-703">stringToSearch</span></span> |<span data-ttu-id="b3da9-704">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-704">Yes</span></span> |<span data-ttu-id="b3da9-705">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-705">string</span></span> |<span data-ttu-id="b3da9-706">Hello hodnotu, která obsahuje položky toofind hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-706">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="b3da9-707">stringToFind</span><span class="sxs-lookup"><span data-stu-id="b3da9-707">stringToFind</span></span> |<span data-ttu-id="b3da9-708">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-708">Yes</span></span> |<span data-ttu-id="b3da9-709">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-709">string</span></span> |<span data-ttu-id="b3da9-710">Hodnota toofind Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-710">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-711">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-711">Return value</span></span>

<span data-ttu-id="b3da9-712">**Hodnota TRUE,** Pokud hello první znak nebo znaky řetězce hello odpovídají hello hodnotu; jinak, **False**.</span><span class="sxs-lookup"><span data-stu-id="b3da9-712">**True** if hello first character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-713">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-713">Examples</span></span>

<span data-ttu-id="b3da9-714">Hello následující příklad ukazuje, jak toouse hello funkce startsWith a endsWith:</span><span class="sxs-lookup"><span data-stu-id="b3da9-714">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="b3da9-715">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-715">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-716">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-716">Name</span></span> | <span data-ttu-id="b3da9-717">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-717">Type</span></span> | <span data-ttu-id="b3da9-718">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-718">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-719">startsTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-719">startsTrue</span></span> | <span data-ttu-id="b3da9-720">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-720">Bool</span></span> | <span data-ttu-id="b3da9-721">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-721">True</span></span> |
| <span data-ttu-id="b3da9-722">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-722">startsCapTrue</span></span> | <span data-ttu-id="b3da9-723">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-723">Bool</span></span> | <span data-ttu-id="b3da9-724">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-724">True</span></span> |
| <span data-ttu-id="b3da9-725">startsFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-725">startsFalse</span></span> | <span data-ttu-id="b3da9-726">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-726">Bool</span></span> | <span data-ttu-id="b3da9-727">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-727">False</span></span> |
| <span data-ttu-id="b3da9-728">endsTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-728">endsTrue</span></span> | <span data-ttu-id="b3da9-729">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-729">Bool</span></span> | <span data-ttu-id="b3da9-730">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-730">True</span></span> |
| <span data-ttu-id="b3da9-731">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="b3da9-731">endsCapTrue</span></span> | <span data-ttu-id="b3da9-732">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-732">Bool</span></span> | <span data-ttu-id="b3da9-733">True</span><span class="sxs-lookup"><span data-stu-id="b3da9-733">True</span></span> |
| <span data-ttu-id="b3da9-734">endsFalse</span><span class="sxs-lookup"><span data-stu-id="b3da9-734">endsFalse</span></span> | <span data-ttu-id="b3da9-735">BOOL</span><span class="sxs-lookup"><span data-stu-id="b3da9-735">Bool</span></span> | <span data-ttu-id="b3da9-736">False</span><span class="sxs-lookup"><span data-stu-id="b3da9-736">False</span></span> |

<a id="string" />

## <a name="string"></a><span data-ttu-id="b3da9-737">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-737">string</span></span>
`string(valueToConvert)`

<span data-ttu-id="b3da9-738">Hello převede zadaný řetězec tooa hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b3da9-738">Converts hello specified value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-739">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-739">Parameters</span></span>

| <span data-ttu-id="b3da9-740">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-740">Parameter</span></span> | <span data-ttu-id="b3da9-741">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-741">Required</span></span> | <span data-ttu-id="b3da9-742">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-742">Type</span></span> | <span data-ttu-id="b3da9-743">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-743">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-744">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="b3da9-744">valueToConvert</span></span> |<span data-ttu-id="b3da9-745">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-745">Yes</span></span> | <span data-ttu-id="b3da9-746">Všechny</span><span class="sxs-lookup"><span data-stu-id="b3da9-746">Any</span></span> |<span data-ttu-id="b3da9-747">toostring tooconvert hodnotu Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-747">hello value tooconvert toostring.</span></span> <span data-ttu-id="b3da9-748">Žádný druh hodnotu lze převést, včetně objekty a pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-748">Any type of value can be converted, including objects and arrays.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-749">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-749">Return value</span></span>

<span data-ttu-id="b3da9-750">Řetězec hello převést hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-750">A string of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-751">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-751">Examples</span></span>

<span data-ttu-id="b3da9-752">Hello následující příklad ukazuje, jak tooconvert různé typy hodnot toostrings:</span><span class="sxs-lookup"><span data-stu-id="b3da9-752">hello following example shows how tooconvert different types of values toostrings:</span></span>

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

<span data-ttu-id="b3da9-753">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-753">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-754">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-754">Name</span></span> | <span data-ttu-id="b3da9-755">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-755">Type</span></span> | <span data-ttu-id="b3da9-756">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-756">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-757">objectOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-757">objectOutput</span></span> | <span data-ttu-id="b3da9-758">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-758">String</span></span> | <span data-ttu-id="b3da9-759">{"dat": 10, hodnotu "b": "Text, například"}</span><span class="sxs-lookup"><span data-stu-id="b3da9-759">{"valueA":10,"valueB":"Example Text"}</span></span> |
| <span data-ttu-id="b3da9-760">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-760">arrayOutput</span></span> | <span data-ttu-id="b3da9-761">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-761">String</span></span> | <span data-ttu-id="b3da9-762">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-762">["a","b","c"]</span></span> |
| <span data-ttu-id="b3da9-763">intOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-763">intOutput</span></span> | <span data-ttu-id="b3da9-764">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-764">String</span></span> | <span data-ttu-id="b3da9-765">5</span><span class="sxs-lookup"><span data-stu-id="b3da9-765">5</span></span> |

<a id="substring" />

## <a name="substring"></a><span data-ttu-id="b3da9-766">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-766">substring</span></span>
`substring(stringToParse, startIndex, length)`

<span data-ttu-id="b3da9-767">Vrátí dílčí řetězec, spustí hello zadaný znak pozice a že obsahuje hello zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-767">Returns a substring that starts at hello specified character position and contains hello specified number of characters.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-768">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-768">Parameters</span></span>

| <span data-ttu-id="b3da9-769">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-769">Parameter</span></span> | <span data-ttu-id="b3da9-770">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-770">Required</span></span> | <span data-ttu-id="b3da9-771">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-771">Type</span></span> | <span data-ttu-id="b3da9-772">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-772">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-773">stringToParse</span><span class="sxs-lookup"><span data-stu-id="b3da9-773">stringToParse</span></span> |<span data-ttu-id="b3da9-774">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-774">Yes</span></span> |<span data-ttu-id="b3da9-775">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-775">string</span></span> |<span data-ttu-id="b3da9-776">řetězec původní Hello, ze které hello dílčí řetězec extrahován.</span><span class="sxs-lookup"><span data-stu-id="b3da9-776">hello original string from which hello substring is extracted.</span></span> |
| <span data-ttu-id="b3da9-777">Počáteční index</span><span class="sxs-lookup"><span data-stu-id="b3da9-777">startIndex</span></span> |<span data-ttu-id="b3da9-778">Ne</span><span class="sxs-lookup"><span data-stu-id="b3da9-778">No</span></span> |<span data-ttu-id="b3da9-779">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-779">int</span></span> |<span data-ttu-id="b3da9-780">Hello počáteční znak pozice s nulovým základem pro hello dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-780">hello zero-based starting character position for hello substring.</span></span> |
| <span data-ttu-id="b3da9-781">Délka</span><span class="sxs-lookup"><span data-stu-id="b3da9-781">length</span></span> |<span data-ttu-id="b3da9-782">Ne</span><span class="sxs-lookup"><span data-stu-id="b3da9-782">No</span></span> |<span data-ttu-id="b3da9-783">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-783">int</span></span> |<span data-ttu-id="b3da9-784">Hello počet znaků pro hello dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-784">hello number of characters for hello substring.</span></span> <span data-ttu-id="b3da9-785">Musí odkazovat tooa umístění v rámci hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-785">Must refer tooa location within hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-786">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-786">Return value</span></span>

<span data-ttu-id="b3da9-787">Hello dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-787">hello substring.</span></span>

### <a name="remarks"></a><span data-ttu-id="b3da9-788">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b3da9-788">Remarks</span></span>

<span data-ttu-id="b3da9-789">Funkce Hello selže, když hello substring přesahuje hello konce řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-789">hello function fails when hello substring extends beyond hello end of hello string.</span></span> <span data-ttu-id="b3da9-790">Následující ukázka Hello selže s hello chyba "hello parametry indexu a délky musí odkazovat tooa umístění v rámci hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-790">hello following example fails with hello error "hello index and length parameters must refer tooa location within hello string.</span></span> <span data-ttu-id="b3da9-791">Parametr index Hello: '0' hello parametr délky: 11, hello Délka parametru řetězce hello: "10". ".</span><span class="sxs-lookup"><span data-stu-id="b3da9-791">hello index parameter: '0', hello length parameter: '11', hello length of hello string parameter: '10'.".</span></span>

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a><span data-ttu-id="b3da9-792">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-792">Examples</span></span>

<span data-ttu-id="b3da9-793">Následující ukázka Hello extrahuje dílčí řetězec z parametr.</span><span class="sxs-lookup"><span data-stu-id="b3da9-793">hello following example extracts a substring from a parameter.</span></span>

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

<span data-ttu-id="b3da9-794">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-794">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-795">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-795">Name</span></span> | <span data-ttu-id="b3da9-796">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-796">Type</span></span> | <span data-ttu-id="b3da9-797">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-797">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-798">substringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-798">substringOutput</span></span> | <span data-ttu-id="b3da9-799">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-799">String</span></span> | <span data-ttu-id="b3da9-800">dvě</span><span class="sxs-lookup"><span data-stu-id="b3da9-800">two</span></span> |


<a id="take" />

## <a name="take"></a><span data-ttu-id="b3da9-801">proveďte</span><span class="sxs-lookup"><span data-stu-id="b3da9-801">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="b3da9-802">Vrátí řetězec s hello zadaný počet znaků od začátku hello hello řetězec nebo pole s hello zadaný počet elementů od začátku hello hello pole.</span><span class="sxs-lookup"><span data-stu-id="b3da9-802">Returns a string with hello specified number of characters from hello start of hello string, or an array with hello specified number of elements from hello start of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-803">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-803">Parameters</span></span>

| <span data-ttu-id="b3da9-804">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-804">Parameter</span></span> | <span data-ttu-id="b3da9-805">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-805">Required</span></span> | <span data-ttu-id="b3da9-806">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-806">Type</span></span> | <span data-ttu-id="b3da9-807">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-807">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-808">původní hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-808">originalValue</span></span> |<span data-ttu-id="b3da9-809">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-809">Yes</span></span> |<span data-ttu-id="b3da9-810">pole nebo řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-810">array or string</span></span> |<span data-ttu-id="b3da9-811">Hello pole nebo řetězec elementy hello tootake z.</span><span class="sxs-lookup"><span data-stu-id="b3da9-811">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="b3da9-812">numberToTake</span><span class="sxs-lookup"><span data-stu-id="b3da9-812">numberToTake</span></span> |<span data-ttu-id="b3da9-813">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-813">Yes</span></span> |<span data-ttu-id="b3da9-814">celá čísla</span><span class="sxs-lookup"><span data-stu-id="b3da9-814">int</span></span> |<span data-ttu-id="b3da9-815">Hello počet tootake elementy nebo znaky.</span><span class="sxs-lookup"><span data-stu-id="b3da9-815">hello number of elements or characters tootake.</span></span> <span data-ttu-id="b3da9-816">Pokud tato hodnota je 0 nebo menší, se vrátí prázdné pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-816">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="b3da9-817">Pokud je větší než délka hello hello zadané pole nebo řetězec, vrátí se všechny elementy hello ve hello pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-817">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-818">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-818">Return value</span></span>

<span data-ttu-id="b3da9-819">Pole nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-819">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-820">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-820">Examples</span></span>

<span data-ttu-id="b3da9-821">Následující příklad trvá hello Hello zadaný počet elementů od hello pole a znaků z řetězce.</span><span class="sxs-lookup"><span data-stu-id="b3da9-821">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="b3da9-822">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-822">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-823">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-823">Name</span></span> | <span data-ttu-id="b3da9-824">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-824">Type</span></span> | <span data-ttu-id="b3da9-825">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-825">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-826">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-826">arrayOutput</span></span> | <span data-ttu-id="b3da9-827">Pole</span><span class="sxs-lookup"><span data-stu-id="b3da9-827">Array</span></span> | <span data-ttu-id="b3da9-828">["1", "dva"]</span><span class="sxs-lookup"><span data-stu-id="b3da9-828">["one", "two"]</span></span> |
| <span data-ttu-id="b3da9-829">stringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-829">stringOutput</span></span> | <span data-ttu-id="b3da9-830">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-830">String</span></span> | <span data-ttu-id="b3da9-831">na</span><span class="sxs-lookup"><span data-stu-id="b3da9-831">on</span></span> |

<a id="tolower" />

## <a name="tolower"></a><span data-ttu-id="b3da9-832">toLower</span><span class="sxs-lookup"><span data-stu-id="b3da9-832">toLower</span></span>
`toLower(stringToChange)`

<span data-ttu-id="b3da9-833">Hello převede zadaný řetězec toolower případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-833">Converts hello specified string toolower case.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-834">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-834">Parameters</span></span>

| <span data-ttu-id="b3da9-835">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-835">Parameter</span></span> | <span data-ttu-id="b3da9-836">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-836">Required</span></span> | <span data-ttu-id="b3da9-837">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-837">Type</span></span> | <span data-ttu-id="b3da9-838">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-838">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-839">stringToChange</span><span class="sxs-lookup"><span data-stu-id="b3da9-839">stringToChange</span></span> |<span data-ttu-id="b3da9-840">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-840">Yes</span></span> |<span data-ttu-id="b3da9-841">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-841">string</span></span> |<span data-ttu-id="b3da9-842">Hello hodnotu tooconvert toolower případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-842">hello value tooconvert toolower case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-843">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-843">Return value</span></span>

<span data-ttu-id="b3da9-844">Hello řetězec převést toolower případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-844">hello string converted toolower case.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-845">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-845">Examples</span></span>

<span data-ttu-id="b3da9-846">Následující ukázka Hello převede případ toolower hodnotu parametru a tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-846">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="b3da9-847">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-847">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-848">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-848">Name</span></span> | <span data-ttu-id="b3da9-849">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-849">Type</span></span> | <span data-ttu-id="b3da9-850">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-850">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-851">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-851">toLowerOutput</span></span> | <span data-ttu-id="b3da9-852">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-852">String</span></span> | <span data-ttu-id="b3da9-853">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-853">one two three</span></span> |
| <span data-ttu-id="b3da9-854">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-854">toUpperOutput</span></span> | <span data-ttu-id="b3da9-855">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-855">String</span></span> | <span data-ttu-id="b3da9-856">JEDNA DVA TŘI</span><span class="sxs-lookup"><span data-stu-id="b3da9-856">ONE TWO THREE</span></span> |

<a id="toupper" />

## <a name="toupper"></a><span data-ttu-id="b3da9-857">toUpper</span><span class="sxs-lookup"><span data-stu-id="b3da9-857">toUpper</span></span>
`toUpper(stringToChange)`

<span data-ttu-id="b3da9-858">Hello převede zadaný řetězec tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-858">Converts hello specified string tooupper case.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-859">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-859">Parameters</span></span>

| <span data-ttu-id="b3da9-860">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-860">Parameter</span></span> | <span data-ttu-id="b3da9-861">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-861">Required</span></span> | <span data-ttu-id="b3da9-862">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-862">Type</span></span> | <span data-ttu-id="b3da9-863">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-863">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-864">stringToChange</span><span class="sxs-lookup"><span data-stu-id="b3da9-864">stringToChange</span></span> |<span data-ttu-id="b3da9-865">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-865">Yes</span></span> |<span data-ttu-id="b3da9-866">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-866">string</span></span> |<span data-ttu-id="b3da9-867">Hello hodnotu tooconvert tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-867">hello value tooconvert tooupper case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-868">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-868">Return value</span></span>

<span data-ttu-id="b3da9-869">Hello řetězec převést tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-869">hello string converted tooupper case.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-870">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-870">Examples</span></span>

<span data-ttu-id="b3da9-871">Následující ukázka Hello převede případ toolower hodnotu parametru a tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-871">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="b3da9-872">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-872">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-873">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-873">Name</span></span> | <span data-ttu-id="b3da9-874">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-874">Type</span></span> | <span data-ttu-id="b3da9-875">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-875">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-876">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-876">toLowerOutput</span></span> | <span data-ttu-id="b3da9-877">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-877">String</span></span> | <span data-ttu-id="b3da9-878">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-878">one two three</span></span> |
| <span data-ttu-id="b3da9-879">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-879">toUpperOutput</span></span> | <span data-ttu-id="b3da9-880">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-880">String</span></span> | <span data-ttu-id="b3da9-881">JEDNA DVA TŘI</span><span class="sxs-lookup"><span data-stu-id="b3da9-881">ONE TWO THREE</span></span> |

<a id="trim" />

## <a name="trim"></a><span data-ttu-id="b3da9-882">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="b3da9-882">trim</span></span>
`trim (stringToTrim)`

<span data-ttu-id="b3da9-883">Odebere všechny úvodní a koncové prázdné znaky z hello zadaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-883">Removes all leading and trailing white-space characters from hello specified string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-884">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-884">Parameters</span></span>

| <span data-ttu-id="b3da9-885">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-885">Parameter</span></span> | <span data-ttu-id="b3da9-886">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-886">Required</span></span> | <span data-ttu-id="b3da9-887">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-887">Type</span></span> | <span data-ttu-id="b3da9-888">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-888">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-889">stringToTrim</span><span class="sxs-lookup"><span data-stu-id="b3da9-889">stringToTrim</span></span> |<span data-ttu-id="b3da9-890">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-890">Yes</span></span> |<span data-ttu-id="b3da9-891">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-891">string</span></span> |<span data-ttu-id="b3da9-892">Hodnota tootrim Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-892">hello value tootrim.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-893">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-893">Return value</span></span>

<span data-ttu-id="b3da9-894">řetězec Hello bez úvodní a koncové prázdné znaky.</span><span class="sxs-lookup"><span data-stu-id="b3da9-894">hello string without leading and trailing white-space characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-895">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-895">Examples</span></span>

<span data-ttu-id="b3da9-896">Hello následující příklad ořízne hello prázdné znaky z parametru hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-896">hello following example trims hello white-space characters from hello parameter.</span></span>

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

<span data-ttu-id="b3da9-897">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-897">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-898">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-898">Name</span></span> | <span data-ttu-id="b3da9-899">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-899">Type</span></span> | <span data-ttu-id="b3da9-900">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-900">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-901">Vrátí</span><span class="sxs-lookup"><span data-stu-id="b3da9-901">return</span></span> | <span data-ttu-id="b3da9-902">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-902">String</span></span> | <span data-ttu-id="b3da9-903">Jedna dva tři</span><span class="sxs-lookup"><span data-stu-id="b3da9-903">one two three</span></span> |

<a id="uniquestring" />

## <a name="uniquestring"></a><span data-ttu-id="b3da9-904">uniqueString</span><span class="sxs-lookup"><span data-stu-id="b3da9-904">uniqueString</span></span>
`uniqueString (baseString, ...)`

<span data-ttu-id="b3da9-905">Vytvoří řetězec deterministickou hash na základě hodnot hello zadané jako parametry.</span><span class="sxs-lookup"><span data-stu-id="b3da9-905">Creates a deterministic hash string based on hello values provided as parameters.</span></span> 

### <a name="parameters"></a><span data-ttu-id="b3da9-906">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-906">Parameters</span></span>

| <span data-ttu-id="b3da9-907">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-907">Parameter</span></span> | <span data-ttu-id="b3da9-908">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-908">Required</span></span> | <span data-ttu-id="b3da9-909">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-909">Type</span></span> | <span data-ttu-id="b3da9-910">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-910">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-911">baseString</span><span class="sxs-lookup"><span data-stu-id="b3da9-911">baseString</span></span> |<span data-ttu-id="b3da9-912">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-912">Yes</span></span> |<span data-ttu-id="b3da9-913">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-913">string</span></span> |<span data-ttu-id="b3da9-914">Hodnota Hello používá toocreate funkce hash hello do jedinečného řetězce.</span><span class="sxs-lookup"><span data-stu-id="b3da9-914">hello value used in hello hash function toocreate a unique string.</span></span> |
| <span data-ttu-id="b3da9-915">Další parametry podle potřeby</span><span class="sxs-lookup"><span data-stu-id="b3da9-915">additional parameters as needed</span></span> |<span data-ttu-id="b3da9-916">Ne</span><span class="sxs-lookup"><span data-stu-id="b3da9-916">No</span></span> |<span data-ttu-id="b3da9-917">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-917">string</span></span> |<span data-ttu-id="b3da9-918">Můžete přidat tolik řetězce jako potřebné toocreate hello hodnotu, která určuje úroveň hello jedinečnosti.</span><span class="sxs-lookup"><span data-stu-id="b3da9-918">You can add as many strings as needed toocreate hello value that specifies hello level of uniqueness.</span></span> |

### <a name="remarks"></a><span data-ttu-id="b3da9-919">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b3da9-919">Remarks</span></span>

<span data-ttu-id="b3da9-920">Tato funkce je užitečné, když potřebujete toocreate jedinečný název pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="b3da9-920">This function is helpful when you need toocreate a unique name for a resource.</span></span> <span data-ttu-id="b3da9-921">Je-li zadat hodnoty parametrů, které omezit obor hello jedinečnosti pro výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-921">You provide parameter values that limit hello scope of uniqueness for hello result.</span></span> <span data-ttu-id="b3da9-922">Můžete zadat, zda je název hello jedinečný dolů toosubscription, skupinu prostředků nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="b3da9-922">You can specify whether hello name is unique down toosubscription, resource group, or deployment.</span></span> 

<span data-ttu-id="b3da9-923">Hello vrátil hodnoty není náhodný řetězec, ale spíš hello výsledek funkce hash.</span><span class="sxs-lookup"><span data-stu-id="b3da9-923">hello returned value is not a random string, but rather hello result of a hash function.</span></span> <span data-ttu-id="b3da9-924">Hello vrátit hodnota je 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-924">hello returned value is 13 characters long.</span></span> <span data-ttu-id="b3da9-925">Není globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="b3da9-925">It is not globally unique.</span></span> <span data-ttu-id="b3da9-926">Můžete chtít toocombine hello hodnotu s předponou ze zásady vytváření názvů toocreate smysluplný název.</span><span class="sxs-lookup"><span data-stu-id="b3da9-926">You may want toocombine hello value with a prefix from your naming convention toocreate a name that is meaningful.</span></span> <span data-ttu-id="b3da9-927">Hello následující příklad ukazuje hello formát hello vrátil hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-927">hello following example shows hello format of hello returned value.</span></span> <span data-ttu-id="b3da9-928">Skutečná hodnota Hello se liší podle hello poskytnutými parametry.</span><span class="sxs-lookup"><span data-stu-id="b3da9-928">hello actual value varies by hello provided parameters.</span></span>

    tcvhiyu5h2o5o

<span data-ttu-id="b3da9-929">Hello následující příklady ukazují, jak toouse uniqueString toocreate a jedinečné hodnoty pro běžně používané úrovně.</span><span class="sxs-lookup"><span data-stu-id="b3da9-929">hello following examples show how toouse uniqueString toocreate a unique value for commonly used levels.</span></span>

<span data-ttu-id="b3da9-930">Jedinečný oboru toosubscription</span><span class="sxs-lookup"><span data-stu-id="b3da9-930">Unique scoped toosubscription</span></span>

```json
"[uniqueString(subscription().subscriptionId)]"
```

<span data-ttu-id="b3da9-931">Jedinečný vymezená tooresource skupiny</span><span class="sxs-lookup"><span data-stu-id="b3da9-931">Unique scoped tooresource group</span></span>

```json
"[uniqueString(resourceGroup().id)]"
```

<span data-ttu-id="b3da9-932">Jedinečný obor toodeployment pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-932">Unique scoped toodeployment for a resource group</span></span>

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

<span data-ttu-id="b3da9-933">Hello následující příklad ukazuje, jak toocreate jedinečný název pro účet úložiště na základě vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-933">hello following example shows how toocreate a unique name for a storage account based on your resource group.</span></span> <span data-ttu-id="b3da9-934">Uvnitř hello skupinu prostředků, název hello není jedinečný, pokud sestavený hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b3da9-934">Inside hello resource group, hello name is not unique if constructed hello same way.</span></span>

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a><span data-ttu-id="b3da9-935">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-935">Return value</span></span>

<span data-ttu-id="b3da9-936">Řetězec obsahující 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="b3da9-936">A string containing 13 characters.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-937">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-937">Examples</span></span>

<span data-ttu-id="b3da9-938">Hello následující příklad vrátí výsledky z uniquestring:</span><span class="sxs-lookup"><span data-stu-id="b3da9-938">hello following example returns results from uniquestring:</span></span>

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

## <a name="uri"></a><span data-ttu-id="b3da9-939">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="b3da9-939">uri</span></span>
`uri (baseUri, relativeUri)`

<span data-ttu-id="b3da9-940">Vytvoří absolutní identifikátor URI kombinací hello baseUri a hello relativeUri řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-940">Creates an absolute URI by combining hello baseUri and hello relativeUri string.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-941">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-941">Parameters</span></span>

| <span data-ttu-id="b3da9-942">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-942">Parameter</span></span> | <span data-ttu-id="b3da9-943">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-943">Required</span></span> | <span data-ttu-id="b3da9-944">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-944">Type</span></span> | <span data-ttu-id="b3da9-945">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-945">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-946">baseUri</span><span class="sxs-lookup"><span data-stu-id="b3da9-946">baseUri</span></span> |<span data-ttu-id="b3da9-947">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-947">Yes</span></span> |<span data-ttu-id="b3da9-948">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-948">string</span></span> |<span data-ttu-id="b3da9-949">řetězec Hello základní identifikátor uri.</span><span class="sxs-lookup"><span data-stu-id="b3da9-949">hello base uri string.</span></span> |
| <span data-ttu-id="b3da9-950">relativeUri</span><span class="sxs-lookup"><span data-stu-id="b3da9-950">relativeUri</span></span> |<span data-ttu-id="b3da9-951">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-951">Yes</span></span> |<span data-ttu-id="b3da9-952">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-952">string</span></span> |<span data-ttu-id="b3da9-953">Hello relativní identifikátor uri řetězec tooadd toohello základní identifikátor uri řetězec.</span><span class="sxs-lookup"><span data-stu-id="b3da9-953">hello relative uri string tooadd toohello base uri string.</span></span> |

<span data-ttu-id="b3da9-954">hodnota pro hello Hello **baseUri** parametr může obsahovat konkrétní soubor, ale jenom základní cesta hello se používá při vytváření hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-954">hello value for hello **baseUri** parameter can include a specific file, but only hello base path is used when constructing hello URI.</span></span> <span data-ttu-id="b3da9-955">Například předávání `http://contoso.com/resources/azuredeploy.json` jako hello baseUri parametr výsledky v základní identifikátor URI služby `http://contoso.com/resources/`.</span><span class="sxs-lookup"><span data-stu-id="b3da9-955">For example, passing `http://contoso.com/resources/azuredeploy.json` as hello baseUri parameter results in a base URI of `http://contoso.com/resources/`.</span></span>

### <a name="return-value"></a><span data-ttu-id="b3da9-956">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-956">Return value</span></span>

<span data-ttu-id="b3da9-957">Řetězec představující hello absolutní identifikátor URI pro základní a relativní hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-957">A string representing hello absolute URI for hello base and relative values.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-958">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-958">Examples</span></span>

<span data-ttu-id="b3da9-959">Hello následující příklad ukazuje, jak tooconstruct šablonu vnořené tooa odkaz založená na hodnotě hello hello nadřazené šablony.</span><span class="sxs-lookup"><span data-stu-id="b3da9-959">hello following example shows how tooconstruct a link tooa nested template based on hello value of hello parent template.</span></span>

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

<span data-ttu-id="b3da9-960">Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b3da9-960">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b3da9-961">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-961">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-962">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-962">Name</span></span> | <span data-ttu-id="b3da9-963">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-963">Type</span></span> | <span data-ttu-id="b3da9-964">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-964">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-965">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-965">uriOutput</span></span> | <span data-ttu-id="b3da9-966">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-966">String</span></span> | <span data-ttu-id="b3da9-967">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-967">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-968">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-968">componentOutput</span></span> | <span data-ttu-id="b3da9-969">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-969">String</span></span> | <span data-ttu-id="b3da9-970">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-970">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-971">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-971">toStringOutput</span></span> | <span data-ttu-id="b3da9-972">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-972">String</span></span> | <span data-ttu-id="b3da9-973">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-973">http://contoso.com/resources/nested/azuredeploy.json</span></span> |

<a id="uricomponent" />

## <a name="uricomponent"></a><span data-ttu-id="b3da9-974">uriComponent</span><span class="sxs-lookup"><span data-stu-id="b3da9-974">uriComponent</span></span>
`uricomponent(stringToEncode)`

<span data-ttu-id="b3da9-975">Kóduje identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-975">Encodes a URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-976">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-976">Parameters</span></span>

| <span data-ttu-id="b3da9-977">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-977">Parameter</span></span> | <span data-ttu-id="b3da9-978">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-978">Required</span></span> | <span data-ttu-id="b3da9-979">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-979">Type</span></span> | <span data-ttu-id="b3da9-980">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-980">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-981">stringToEncode</span><span class="sxs-lookup"><span data-stu-id="b3da9-981">stringToEncode</span></span> |<span data-ttu-id="b3da9-982">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-982">Yes</span></span> |<span data-ttu-id="b3da9-983">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-983">string</span></span> |<span data-ttu-id="b3da9-984">Hodnota tooencode Hello.</span><span class="sxs-lookup"><span data-stu-id="b3da9-984">hello value tooencode.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-985">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-985">Return value</span></span>

<span data-ttu-id="b3da9-986">Řetězec hello URI kódovaný hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-986">A string of hello URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-987">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-987">Examples</span></span>

<span data-ttu-id="b3da9-988">Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b3da9-988">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b3da9-989">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-989">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-990">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-990">Name</span></span> | <span data-ttu-id="b3da9-991">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-991">Type</span></span> | <span data-ttu-id="b3da9-992">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-992">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-993">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-993">uriOutput</span></span> | <span data-ttu-id="b3da9-994">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-994">String</span></span> | <span data-ttu-id="b3da9-995">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-995">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-996">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-996">componentOutput</span></span> | <span data-ttu-id="b3da9-997">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-997">String</span></span> | <span data-ttu-id="b3da9-998">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-998">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-999">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-999">toStringOutput</span></span> | <span data-ttu-id="b3da9-1000">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-1000">String</span></span> | <span data-ttu-id="b3da9-1001">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-1001">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a><span data-ttu-id="b3da9-1002">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="b3da9-1002">uriComponentToString</span></span>
`uriComponentToString(uriEncodedString)`

<span data-ttu-id="b3da9-1003">Vrátí že hodnotu kódovaný řetězec identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-1003">Returns a string of a URI encoded value.</span></span>

### <a name="parameters"></a><span data-ttu-id="b3da9-1004">Parametry</span><span class="sxs-lookup"><span data-stu-id="b3da9-1004">Parameters</span></span>

| <span data-ttu-id="b3da9-1005">Parametr</span><span class="sxs-lookup"><span data-stu-id="b3da9-1005">Parameter</span></span> | <span data-ttu-id="b3da9-1006">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b3da9-1006">Required</span></span> | <span data-ttu-id="b3da9-1007">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-1007">Type</span></span> | <span data-ttu-id="b3da9-1008">Popis</span><span class="sxs-lookup"><span data-stu-id="b3da9-1008">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="b3da9-1009">uriEncodedString</span><span class="sxs-lookup"><span data-stu-id="b3da9-1009">uriEncodedString</span></span> |<span data-ttu-id="b3da9-1010">Ano</span><span class="sxs-lookup"><span data-stu-id="b3da9-1010">Yes</span></span> |<span data-ttu-id="b3da9-1011">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-1011">string</span></span> |<span data-ttu-id="b3da9-1012">Hodnota tooconvert tooa řetězec kódovaný Hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="b3da9-1012">hello URI encoded value tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="b3da9-1013">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-1013">Return value</span></span>

<span data-ttu-id="b3da9-1014">Řetězec dekódované identifikátoru URI kódovaný hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b3da9-1014">A decoded string of URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="b3da9-1015">Příklady</span><span class="sxs-lookup"><span data-stu-id="b3da9-1015">Examples</span></span>

<span data-ttu-id="b3da9-1016">Následující příklad ukazuje, jak Hello toouse uri, uriComponent a uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="b3da9-1016">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="b3da9-1017">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="b3da9-1017">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="b3da9-1018">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b3da9-1018">Name</span></span> | <span data-ttu-id="b3da9-1019">Typ</span><span class="sxs-lookup"><span data-stu-id="b3da9-1019">Type</span></span> | <span data-ttu-id="b3da9-1020">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b3da9-1020">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="b3da9-1021">uriOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-1021">uriOutput</span></span> | <span data-ttu-id="b3da9-1022">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-1022">String</span></span> | <span data-ttu-id="b3da9-1023">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-1023">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-1024">componentOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-1024">componentOutput</span></span> | <span data-ttu-id="b3da9-1025">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-1025">String</span></span> | <span data-ttu-id="b3da9-1026">http%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-1026">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="b3da9-1027">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="b3da9-1027">toStringOutput</span></span> | <span data-ttu-id="b3da9-1028">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b3da9-1028">String</span></span> | <span data-ttu-id="b3da9-1029">http://contoso.com/resources/Nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b3da9-1029">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


## <a name="next-steps"></a><span data-ttu-id="b3da9-1030">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3da9-1030">Next steps</span></span>
* <span data-ttu-id="b3da9-1031">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3da9-1031">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b3da9-1032">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3da9-1032">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="b3da9-1033">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="b3da9-1033">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="b3da9-1034">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b3da9-1034">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

