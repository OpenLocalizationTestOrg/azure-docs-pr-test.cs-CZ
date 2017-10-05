---
title: "Funkce šablony Resource Manageru | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manager k získávání hodnot, pracovat s řetězce a numerické hodnoty a načíst informace o nasazení."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: 1324bed07e991e9d84cb6832afe78bdb2d3348fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-resource-manager-template-functions"></a><span data-ttu-id="af7e1-103">Funkce šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="af7e1-103">Azure Resource Manager template functions</span></span>
<span data-ttu-id="af7e1-104">Toto téma popisuje všechny funkce, které můžete použít v šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="af7e1-104">This topic describes all the functions you can use in an Azure Resource Manager template.</span></span>

<span data-ttu-id="af7e1-105">Přidání funkce v šablonách uzavřené v závorkách: `[` a `]`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="af7e1-105">You add functions in your templates by enclosing them within brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="af7e1-106">Výraz se vyhodnotí během nasazování.</span><span class="sxs-lookup"><span data-stu-id="af7e1-106">The expression is evaluated during deployment.</span></span> <span data-ttu-id="af7e1-107">Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu jiného typu formátu JSON, jako je například pole, objektu nebo celé číslo.</span><span class="sxs-lookup"><span data-stu-id="af7e1-107">While written as a string literal, the result of evaluating the expression can be of a different JSON type, such as an array, object, or integer.</span></span> <span data-ttu-id="af7e1-108">Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="af7e1-108">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="af7e1-109">Vlastnosti odkazovat pomocí operátorů dot a [index].</span><span class="sxs-lookup"><span data-stu-id="af7e1-109">You reference properties by using the dot and [index] operators.</span></span>

<span data-ttu-id="af7e1-110">Výraz šablony nesmí překročit 24,576 znaků.</span><span class="sxs-lookup"><span data-stu-id="af7e1-110">A template expression cannot exceed 24,576 characters.</span></span>

<span data-ttu-id="af7e1-111">Šablony funkcí a jejich parametrů se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="af7e1-111">Template functions and their parameters are case-insensitive.</span></span> <span data-ttu-id="af7e1-112">Například správce prostředků přeloží **variables('var1')** a **VARIABLES('VAR1')** jako stejné.</span><span class="sxs-lookup"><span data-stu-id="af7e1-112">For example, Resource Manager resolves **variables('var1')** and **VARIABLES('VAR1')** as the same.</span></span> <span data-ttu-id="af7e1-113">Při hodnocení, pokud funkci výslovně upraví případ (například toUpper nebo toLower), funkce zachovává malá a velká.</span><span class="sxs-lookup"><span data-stu-id="af7e1-113">When evaluated, unless the function expressly modifies case (such as toUpper or toLower), the function preserves the case.</span></span> <span data-ttu-id="af7e1-114">Některé typy prostředků může mít případu požadavky bez ohledu na to, jak se vyhodnocují funkce.</span><span class="sxs-lookup"><span data-stu-id="af7e1-114">Certain resource types may have case requirements irrespective of how functions are evaluated.</span></span>

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a><span data-ttu-id="af7e1-115">Funkce pole a objektu</span><span class="sxs-lookup"><span data-stu-id="af7e1-115">Array and object functions</span></span>
<span data-ttu-id="af7e1-116">Resource Manager poskytuje několik funkce pro práci s pole a objekty.</span><span class="sxs-lookup"><span data-stu-id="af7e1-116">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="af7e1-117">pole</span><span class="sxs-lookup"><span data-stu-id="af7e1-117">array</span></span>](resource-group-template-functions-array.md#array)
* [<span data-ttu-id="af7e1-118">sloučení</span><span class="sxs-lookup"><span data-stu-id="af7e1-118">coalesce</span></span>](resource-group-template-functions-array.md#coalesce)
* [<span data-ttu-id="af7e1-119">concat</span><span class="sxs-lookup"><span data-stu-id="af7e1-119">concat</span></span>](resource-group-template-functions-array.md#concat)
* [<span data-ttu-id="af7e1-120">obsahuje</span><span class="sxs-lookup"><span data-stu-id="af7e1-120">contains</span></span>](resource-group-template-functions-array.md#contains)
* [<span data-ttu-id="af7e1-121">createArray</span><span class="sxs-lookup"><span data-stu-id="af7e1-121">createArray</span></span>](resource-group-template-functions-array.md#createarray)
* [<span data-ttu-id="af7e1-122">prázdný</span><span class="sxs-lookup"><span data-stu-id="af7e1-122">empty</span></span>](resource-group-template-functions-array.md#empty)
* [<span data-ttu-id="af7e1-123">první</span><span class="sxs-lookup"><span data-stu-id="af7e1-123">first</span></span>](resource-group-template-functions-array.md#first)
* [<span data-ttu-id="af7e1-124">průnik</span><span class="sxs-lookup"><span data-stu-id="af7e1-124">intersection</span></span>](resource-group-template-functions-array.md#intersection)
* [<span data-ttu-id="af7e1-125">JSON</span><span class="sxs-lookup"><span data-stu-id="af7e1-125">json</span></span>](resource-group-template-functions-array.md#json)
* [<span data-ttu-id="af7e1-126">poslední</span><span class="sxs-lookup"><span data-stu-id="af7e1-126">last</span></span>](resource-group-template-functions-array.md#last)
* [<span data-ttu-id="af7e1-127">Délka</span><span class="sxs-lookup"><span data-stu-id="af7e1-127">length</span></span>](resource-group-template-functions-array.md#length)
* [<span data-ttu-id="af7e1-128">min.</span><span class="sxs-lookup"><span data-stu-id="af7e1-128">min</span></span>](resource-group-template-functions-array.md#min)
* [<span data-ttu-id="af7e1-129">maximální počet</span><span class="sxs-lookup"><span data-stu-id="af7e1-129">max</span></span>](resource-group-template-functions-array.md#max)
* [<span data-ttu-id="af7e1-130">rozsah</span><span class="sxs-lookup"><span data-stu-id="af7e1-130">range</span></span>](resource-group-template-functions-array.md#range)
* [<span data-ttu-id="af7e1-131">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="af7e1-131">skip</span></span>](resource-group-template-functions-array.md#skip)
* [<span data-ttu-id="af7e1-132">proveďte</span><span class="sxs-lookup"><span data-stu-id="af7e1-132">take</span></span>](resource-group-template-functions-array.md#take)
* [<span data-ttu-id="af7e1-133">sjednocení</span><span class="sxs-lookup"><span data-stu-id="af7e1-133">union</span></span>](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a><span data-ttu-id="af7e1-134">Porovnání funkcí</span><span class="sxs-lookup"><span data-stu-id="af7e1-134">Comparison functions</span></span>
<span data-ttu-id="af7e1-135">Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.</span><span class="sxs-lookup"><span data-stu-id="af7e1-135">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="af7e1-136">rovná se</span><span class="sxs-lookup"><span data-stu-id="af7e1-136">equals</span></span>](resource-group-template-functions-comparison.md#equals)
* [<span data-ttu-id="af7e1-137">menší</span><span class="sxs-lookup"><span data-stu-id="af7e1-137">less</span></span>](resource-group-template-functions-comparison.md#less)
* [<span data-ttu-id="af7e1-138">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="af7e1-138">lessOrEquals</span></span>](resource-group-template-functions-comparison.md#lessorequals)
* [<span data-ttu-id="af7e1-139">větší</span><span class="sxs-lookup"><span data-stu-id="af7e1-139">greater</span></span>](resource-group-template-functions-comparison.md#greater)
* [<span data-ttu-id="af7e1-140">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="af7e1-140">greaterOrEquals</span></span>](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a><span data-ttu-id="af7e1-141">Hodnota funkce nasazení</span><span class="sxs-lookup"><span data-stu-id="af7e1-141">Deployment value functions</span></span>
<span data-ttu-id="af7e1-142">Resource Manager poskytuje následující funkce pro získání hodnoty z části šablony a hodnoty týkající se nasazení:</span><span class="sxs-lookup"><span data-stu-id="af7e1-142">Resource Manager provides the following functions for getting values from sections of the template and values related to the deployment:</span></span>

* [<span data-ttu-id="af7e1-143">nasazení</span><span class="sxs-lookup"><span data-stu-id="af7e1-143">deployment</span></span>](resource-group-template-functions-deployment.md#deployment)
* [<span data-ttu-id="af7e1-144">Parametry</span><span class="sxs-lookup"><span data-stu-id="af7e1-144">parameters</span></span>](resource-group-template-functions-deployment.md#parameters)
* [<span data-ttu-id="af7e1-145">proměnné</span><span class="sxs-lookup"><span data-stu-id="af7e1-145">variables</span></span>](resource-group-template-functions-deployment.md#variables)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="logical-functions"></a><span data-ttu-id="af7e1-146">Logické funkce</span><span class="sxs-lookup"><span data-stu-id="af7e1-146">Logical functions</span></span>
<span data-ttu-id="af7e1-147">Resource Manager poskytuje následující funkce pro práci s logických podmínek:</span><span class="sxs-lookup"><span data-stu-id="af7e1-147">Resource Manager provides the following functions for working with logical conditions:</span></span>

* [<span data-ttu-id="af7e1-148">a</span><span class="sxs-lookup"><span data-stu-id="af7e1-148">and</span></span>](resource-group-template-functions-logical.md#and)
* [<span data-ttu-id="af7e1-149">BOOL</span><span class="sxs-lookup"><span data-stu-id="af7e1-149">bool</span></span>](resource-group-template-functions-logical.md#bool)
* [<span data-ttu-id="af7e1-150">Pokud</span><span class="sxs-lookup"><span data-stu-id="af7e1-150">if</span></span>](resource-group-template-functions-logical.md#if)
* [<span data-ttu-id="af7e1-151">není</span><span class="sxs-lookup"><span data-stu-id="af7e1-151">not</span></span>](resource-group-template-functions-logical.md#not)
* [<span data-ttu-id="af7e1-152">nebo</span><span class="sxs-lookup"><span data-stu-id="af7e1-152">or</span></span>](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a><span data-ttu-id="af7e1-153">Numerické funkce</span><span class="sxs-lookup"><span data-stu-id="af7e1-153">Numeric functions</span></span>
<span data-ttu-id="af7e1-154">Resource Manager poskytuje následující funkce pro práci s celými čísly:</span><span class="sxs-lookup"><span data-stu-id="af7e1-154">Resource Manager provides the following functions for working with integers:</span></span>

* [<span data-ttu-id="af7e1-155">Přidat</span><span class="sxs-lookup"><span data-stu-id="af7e1-155">add</span></span>](resource-group-template-functions-numeric.md#add)
* [<span data-ttu-id="af7e1-156">copyIndex</span><span class="sxs-lookup"><span data-stu-id="af7e1-156">copyIndex</span></span>](resource-group-template-functions-numeric.md#copyindex)
* [<span data-ttu-id="af7e1-157">div</span><span class="sxs-lookup"><span data-stu-id="af7e1-157">div</span></span>](resource-group-template-functions-numeric.md#div)
* [<span data-ttu-id="af7e1-158">plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="af7e1-158">float</span></span>](resource-group-template-functions-numeric.md#float)
* [<span data-ttu-id="af7e1-159">celá čísla</span><span class="sxs-lookup"><span data-stu-id="af7e1-159">int</span></span>](resource-group-template-functions-numeric.md#int)
* [<span data-ttu-id="af7e1-160">min.</span><span class="sxs-lookup"><span data-stu-id="af7e1-160">min</span></span>](resource-group-template-functions-numeric.md#min)
* [<span data-ttu-id="af7e1-161">maximální počet</span><span class="sxs-lookup"><span data-stu-id="af7e1-161">max</span></span>](resource-group-template-functions-numeric.md#max)
* [<span data-ttu-id="af7e1-162">MOD</span><span class="sxs-lookup"><span data-stu-id="af7e1-162">mod</span></span>](resource-group-template-functions-numeric.md#mod)
* [<span data-ttu-id="af7e1-163">mul</span><span class="sxs-lookup"><span data-stu-id="af7e1-163">mul</span></span>](resource-group-template-functions-numeric.md#mul)
* [<span data-ttu-id="af7e1-164">Sub –</span><span class="sxs-lookup"><span data-stu-id="af7e1-164">sub</span></span>](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a><span data-ttu-id="af7e1-165">Funkce prostředků</span><span class="sxs-lookup"><span data-stu-id="af7e1-165">Resource functions</span></span>
<span data-ttu-id="af7e1-166">Resource Manager poskytuje následující funkce pro získání hodnoty prostředku:</span><span class="sxs-lookup"><span data-stu-id="af7e1-166">Resource Manager provides the following functions for getting resource values:</span></span>

* [<span data-ttu-id="af7e1-167">listKeys a seznamu {Value}</span><span class="sxs-lookup"><span data-stu-id="af7e1-167">listKeys and list{Value}</span></span>](resource-group-template-functions-resource.md#listkeys)
* [<span data-ttu-id="af7e1-168">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="af7e1-168">providers</span></span>](resource-group-template-functions-resource.md#providers)
* [<span data-ttu-id="af7e1-169">referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="af7e1-169">reference</span></span>](resource-group-template-functions-resource.md#reference)
* [<span data-ttu-id="af7e1-170">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="af7e1-170">resourceGroup</span></span>](resource-group-template-functions-resource.md#resourcegroup)
* [<span data-ttu-id="af7e1-171">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="af7e1-171">resourceId</span></span>](resource-group-template-functions-resource.md#resourceid)
* [<span data-ttu-id="af7e1-172">předplatné</span><span class="sxs-lookup"><span data-stu-id="af7e1-172">subscription</span></span>](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a><span data-ttu-id="af7e1-173">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="af7e1-173">String functions</span></span>
<span data-ttu-id="af7e1-174">Resource Manager poskytuje následující funkce pro práci s řetězce:</span><span class="sxs-lookup"><span data-stu-id="af7e1-174">Resource Manager provides the following functions for working with strings:</span></span>

* [<span data-ttu-id="af7e1-175">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="af7e1-175">base64</span></span>](resource-group-template-functions-string.md#base64)
* [<span data-ttu-id="af7e1-176">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="af7e1-176">base64ToJson</span></span>](resource-group-template-functions-string.md#base64tojson)
* [<span data-ttu-id="af7e1-177">base64ToString</span><span class="sxs-lookup"><span data-stu-id="af7e1-177">base64ToString</span></span>](resource-group-template-functions-string.md#base64tostring)
* [<span data-ttu-id="af7e1-178">concat</span><span class="sxs-lookup"><span data-stu-id="af7e1-178">concat</span></span>](resource-group-template-functions-string.md#concat)
* [<span data-ttu-id="af7e1-179">obsahuje</span><span class="sxs-lookup"><span data-stu-id="af7e1-179">contains</span></span>](resource-group-template-functions-string.md#contains)
* [<span data-ttu-id="af7e1-180">dataUri</span><span class="sxs-lookup"><span data-stu-id="af7e1-180">dataUri</span></span>](resource-group-template-functions-string.md#datauri)
* [<span data-ttu-id="af7e1-181">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="af7e1-181">dataUriToString</span></span>](resource-group-template-functions-string.md#datauritostring)
* [<span data-ttu-id="af7e1-182">prázdný</span><span class="sxs-lookup"><span data-stu-id="af7e1-182">empty</span></span>](resource-group-template-functions-string.md#empty)
* [<span data-ttu-id="af7e1-183">endsWith</span><span class="sxs-lookup"><span data-stu-id="af7e1-183">endsWith</span></span>](resource-group-template-functions-string.md#endswith)
* [<span data-ttu-id="af7e1-184">první</span><span class="sxs-lookup"><span data-stu-id="af7e1-184">first</span></span>](resource-group-template-functions-string.md#first)
* [<span data-ttu-id="af7e1-185">indexOf</span><span class="sxs-lookup"><span data-stu-id="af7e1-185">indexOf</span></span>](resource-group-template-functions-string.md#indexof)
* [<span data-ttu-id="af7e1-186">poslední</span><span class="sxs-lookup"><span data-stu-id="af7e1-186">last</span></span>](resource-group-template-functions-string.md#last)
* [<span data-ttu-id="af7e1-187">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="af7e1-187">lastIndexOf</span></span>](resource-group-template-functions-string.md#lastindexof)
* [<span data-ttu-id="af7e1-188">Délka</span><span class="sxs-lookup"><span data-stu-id="af7e1-188">length</span></span>](resource-group-template-functions-string.md#length)
* [<span data-ttu-id="af7e1-189">padLeft</span><span class="sxs-lookup"><span data-stu-id="af7e1-189">padLeft</span></span>](resource-group-template-functions-string.md#padleft)
* [<span data-ttu-id="af7e1-190">nahradit</span><span class="sxs-lookup"><span data-stu-id="af7e1-190">replace</span></span>](resource-group-template-functions-string.md#replace)
* [<span data-ttu-id="af7e1-191">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="af7e1-191">skip</span></span>](resource-group-template-functions-string.md#skip)
* [<span data-ttu-id="af7e1-192">split</span><span class="sxs-lookup"><span data-stu-id="af7e1-192">split</span></span>](resource-group-template-functions-string.md#split)
* [<span data-ttu-id="af7e1-193">startsWith</span><span class="sxs-lookup"><span data-stu-id="af7e1-193">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="af7e1-194">řetězec</span><span class="sxs-lookup"><span data-stu-id="af7e1-194">string</span></span>](resource-group-template-functions-string.md#string)
* [<span data-ttu-id="af7e1-195">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="af7e1-195">substring</span></span>](resource-group-template-functions-string.md#substring)
* [<span data-ttu-id="af7e1-196">proveďte</span><span class="sxs-lookup"><span data-stu-id="af7e1-196">take</span></span>](resource-group-template-functions-string.md#take)
* [<span data-ttu-id="af7e1-197">toLower</span><span class="sxs-lookup"><span data-stu-id="af7e1-197">toLower</span></span>](resource-group-template-functions-string.md#tolower)
* [<span data-ttu-id="af7e1-198">toUpper</span><span class="sxs-lookup"><span data-stu-id="af7e1-198">toUpper</span></span>](resource-group-template-functions-string.md#toupper)
* [<span data-ttu-id="af7e1-199">uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="af7e1-199">trim</span></span>](resource-group-template-functions-string.md#trim)
* [<span data-ttu-id="af7e1-200">uniqueString</span><span class="sxs-lookup"><span data-stu-id="af7e1-200">uniqueString</span></span>](resource-group-template-functions-string.md#uniquestring)
* [<span data-ttu-id="af7e1-201">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="af7e1-201">uri</span></span>](resource-group-template-functions-string.md#uri)
* [<span data-ttu-id="af7e1-202">uriComponent</span><span class="sxs-lookup"><span data-stu-id="af7e1-202">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="af7e1-203">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="af7e1-203">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)


## <a name="next-steps"></a><span data-ttu-id="af7e1-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af7e1-204">Next steps</span></span>
* <span data-ttu-id="af7e1-205">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="af7e1-205">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="af7e1-206">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md)</span><span class="sxs-lookup"><span data-stu-id="af7e1-206">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md)</span></span>
* <span data-ttu-id="af7e1-207">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="af7e1-207">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>
* <span data-ttu-id="af7e1-208">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="af7e1-208">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)</span></span>

