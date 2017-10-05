---
title: "Spravované aplikace Azure vytvořit definici funkcí uživatelského rozhraní | Microsoft Docs"
description: "Popisuje funkce pro použití při vytváření definice uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 62ee10eb8e6f33cc4d828cf01b405c846bef8aa4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="793d3-103">Funkce CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="793d3-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="793d3-104">Tato část obsahuje podpisy pro všechny podporované funkce CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="793d3-104">This section contains the signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="793d3-105">Pokud chcete používat funkci, uzavřete deklaraci do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="793d3-105">To use a function, surround the declaration with square brackets.</span></span> <span data-ttu-id="793d3-106">Například:</span><span class="sxs-lookup"><span data-stu-id="793d3-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="793d3-107">Řetězce a dalších funkcí, může být odkazován jako parametry pro funkci, ale řetězce musí být uzavřena do jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="793d3-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="793d3-108">Například:</span><span class="sxs-lookup"><span data-stu-id="793d3-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="793d3-109">Případně vlastnosti výstupu funkce můžete odkazovat pomocí operátoru tečka.</span><span class="sxs-lookup"><span data-stu-id="793d3-109">Where applicable, you can reference properties of the output of a function by using the dot operator.</span></span> <span data-ttu-id="793d3-110">Například:</span><span class="sxs-lookup"><span data-stu-id="793d3-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="793d3-111">Odkazování na funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-111">Referencing functions</span></span>
<span data-ttu-id="793d3-112">Tyto funkce slouží k odkazování výstupy z vlastnosti nebo kontextu CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="793d3-112">These functions can be used to reference outputs from the properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="793d3-113">Základy</span><span class="sxs-lookup"><span data-stu-id="793d3-113">basics</span></span>
<span data-ttu-id="793d3-114">Vrátí hodnoty výstup elementu, který je definovaný v kroku základy.</span><span class="sxs-lookup"><span data-stu-id="793d3-114">Returns the output values of an element that is defined in the Basics step.</span></span>

<span data-ttu-id="793d3-115">Následující příklad vrátí její výstup elementu s názvem `foo` v základní informace o kroku:</span><span class="sxs-lookup"><span data-stu-id="793d3-115">The following example returns the output of the element named `foo` in the Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="793d3-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="793d3-116">steps</span></span>
<span data-ttu-id="793d3-117">Vrátí hodnoty výstup elementu, který je definován v zadané kroku.</span><span class="sxs-lookup"><span data-stu-id="793d3-117">Returns the output values of an element that is defined in the specified step.</span></span> <span data-ttu-id="793d3-118">Výstupní hodnoty elementů v kroku základy, použijte `basics()` místo.</span><span class="sxs-lookup"><span data-stu-id="793d3-118">To get the output values of elements in the Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="793d3-119">Následující příklad vrátí její výstup elementu s názvem `bar` v kroku s názvem `foo`:</span><span class="sxs-lookup"><span data-stu-id="793d3-119">The following example returns the output of the element named `bar` in the step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="793d3-120">location</span><span class="sxs-lookup"><span data-stu-id="793d3-120">location</span></span>
<span data-ttu-id="793d3-121">Vrátí umístění vybrané v kroku základy nebo aktuální kontext.</span><span class="sxs-lookup"><span data-stu-id="793d3-121">Returns the location selected in the Basics step or the current context.</span></span>

<span data-ttu-id="793d3-122">V následujícím příkladu může vrátit `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-122">The following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="793d3-123">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-123">String functions</span></span>
<span data-ttu-id="793d3-124">Tyto funkce slouží pouze s řetězce formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="793d3-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="793d3-125">concat</span><span class="sxs-lookup"><span data-stu-id="793d3-125">concat</span></span>
<span data-ttu-id="793d3-126">Zřetězí nejméně jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="793d3-127">Například pokud hodnota výstupu `element1` Pokud `"bar"`, pak tento příklad vrátí řetězec `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-127">For example, if the output value of `element1` if `"bar"`, then this example returns the string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="793d3-128">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-128">substring</span></span>
<span data-ttu-id="793d3-129">Vrátí dílčí řetězec zadaného řetězce.</span><span class="sxs-lookup"><span data-stu-id="793d3-129">Returns the substring of the specified string.</span></span> <span data-ttu-id="793d3-130">Dílčí řetězec spustí v zadaném indexu a má na určenou délku.</span><span class="sxs-lookup"><span data-stu-id="793d3-130">The substring starts at the specified index and has the specified length.</span></span>

<span data-ttu-id="793d3-131">Následující příklad vrací `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-131">The following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="793d3-132">Nahradit</span><span class="sxs-lookup"><span data-stu-id="793d3-132">replace</span></span>
<span data-ttu-id="793d3-133">Vrátí řetězec, ve kterém jsou všechny výskyty zadaného řetězce v aktuálním řetězec nahrazen jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="793d3-133">Returns a string in which all occurrences of the specified string in the current string are replaced with another string.</span></span>

<span data-ttu-id="793d3-134">Následující příklad vrací `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-134">The following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="793d3-135">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="793d3-135">guid</span></span>
<span data-ttu-id="793d3-136">Generuje řetězec globálně jedinečný (identifikátor GUID).</span><span class="sxs-lookup"><span data-stu-id="793d3-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="793d3-137">V následujícím příkladu může vrátit `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-137">The following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="793d3-138">toLower</span><span class="sxs-lookup"><span data-stu-id="793d3-138">toLower</span></span>
<span data-ttu-id="793d3-139">Vrací řetězec převedený na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="793d3-139">Returns a string converted to lowercase.</span></span>

<span data-ttu-id="793d3-140">Následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-140">The following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="793d3-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="793d3-141">toUpper</span></span>
<span data-ttu-id="793d3-142">Vrací řetězec převedený na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="793d3-142">Returns a string converted to uppercase.</span></span>

<span data-ttu-id="793d3-143">Následující příklad vrací `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-143">The following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="793d3-144">Kolekce funkcí</span><span class="sxs-lookup"><span data-stu-id="793d3-144">Collection functions</span></span>
<span data-ttu-id="793d3-145">Tyto funkce slouží ke kolekcím, jako je řetězce formátu JSON, pole a objekty.</span><span class="sxs-lookup"><span data-stu-id="793d3-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="793d3-146">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="793d3-146">contains</span></span>
<span data-ttu-id="793d3-147">Vrátí `true` řetězec obsahuje určený dílčí řetězec, pole obsahuje zadanou hodnotu, nebo objekt obsahuje zadaný klíč.</span><span class="sxs-lookup"><span data-stu-id="793d3-147">Returns `true` if a string contains the specified substring, an array contains the specified value, or an object contains the specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-148">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-148">Example 1: string</span></span>
<span data-ttu-id="793d3-149">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-149">The following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-150">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-150">Example 2: array</span></span>
<span data-ttu-id="793d3-151">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-152">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-152">The following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-153">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-153">Example 3: object</span></span>
<span data-ttu-id="793d3-154">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="793d3-155">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-155">The following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="793d3-156">Délka</span><span class="sxs-lookup"><span data-stu-id="793d3-156">length</span></span>
<span data-ttu-id="793d3-157">Vrátí počet znaků v řetězci, počet hodnot v poli nebo počet klíčů v objektu.</span><span class="sxs-lookup"><span data-stu-id="793d3-157">Returns the number of characters in a string, the number of values in an array, or the number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-158">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-158">Example 1: string</span></span>
<span data-ttu-id="793d3-159">Následující příklad vrací `6`:</span><span class="sxs-lookup"><span data-stu-id="793d3-159">The following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-160">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-160">Example 2: array</span></span>
<span data-ttu-id="793d3-161">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-162">Následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="793d3-162">The following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-163">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-163">Example 3: object</span></span>
<span data-ttu-id="793d3-164">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="793d3-165">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-165">The following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="793d3-166">prázdný</span><span class="sxs-lookup"><span data-stu-id="793d3-166">empty</span></span>
<span data-ttu-id="793d3-167">Vrátí `true` Pokud řetězec, pole nebo objekt je null nebo prázdná.</span><span class="sxs-lookup"><span data-stu-id="793d3-167">Returns `true` if the string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-168">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-168">Example 1: string</span></span>
<span data-ttu-id="793d3-169">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-169">The following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-170">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-170">Example 2: array</span></span>
<span data-ttu-id="793d3-171">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-172">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-172">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-173">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-173">Example 3: object</span></span>
<span data-ttu-id="793d3-174">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="793d3-175">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-175">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="793d3-176">Příklad 4: null a Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="793d3-176">Example 4: null and undefined</span></span>
<span data-ttu-id="793d3-177">Předpokládejme `element1` je `null` nebo nedefinované.</span><span class="sxs-lookup"><span data-stu-id="793d3-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="793d3-178">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-178">The following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="793d3-179">první</span><span class="sxs-lookup"><span data-stu-id="793d3-179">first</span></span>
<span data-ttu-id="793d3-180">Vrátí první znak zadaný řetězec; první hodnotu zadaného pole; nebo první klíč a hodnotu zadaného objektu.</span><span class="sxs-lookup"><span data-stu-id="793d3-180">Returns the first character of the specified string; first value of the specified array; or the first key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-181">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-181">Example 1: string</span></span>
<span data-ttu-id="793d3-182">Následující příklad vrací `"f"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-182">The following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-183">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-183">Example 2: array</span></span>
<span data-ttu-id="793d3-184">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-185">Následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="793d3-185">The following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-186">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-186">Example 3: object</span></span>
<span data-ttu-id="793d3-187">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="793d3-188">Následující příklad vrací `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="793d3-188">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="793d3-189">poslední</span><span class="sxs-lookup"><span data-stu-id="793d3-189">last</span></span>
<span data-ttu-id="793d3-190">Vrátí poslední znak zadaný řetězec, poslední hodnotu zadaného pole nebo poslední klíč a hodnotu zadaného objektu.</span><span class="sxs-lookup"><span data-stu-id="793d3-190">Returns the last character of the specified string, the last value of the specified array, or the last key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-191">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-191">Example 1: string</span></span>
<span data-ttu-id="793d3-192">Následující příklad vrací `"r"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-192">The following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-193">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-193">Example 2: array</span></span>
<span data-ttu-id="793d3-194">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-195">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-195">The following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-196">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-196">Example 3: object</span></span>
<span data-ttu-id="793d3-197">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="793d3-198">Následující příklad vrací `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="793d3-198">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="793d3-199">proveďte</span><span class="sxs-lookup"><span data-stu-id="793d3-199">take</span></span>
<span data-ttu-id="793d3-200">Vrátí zadaný počet po sobě jdoucích znaků od začátku řetězce, zadaný počet souvislý hodnoty od začátku pole nebo zadaný počet sousedních klíčů a hodnoty ze začátku objektu.</span><span class="sxs-lookup"><span data-stu-id="793d3-200">Returns a specified number of contiguous characters from the start of the string, a specified number of contiguous values from the start of the array, or a specified number of contiguous keys and values from the start of the object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-201">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-201">Example 1: string</span></span>
<span data-ttu-id="793d3-202">Následující příklad vrací `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-202">The following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-203">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-203">Example 2: array</span></span>
<span data-ttu-id="793d3-204">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-205">Následující příklad vrací `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="793d3-205">The following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-206">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-206">Example 3: object</span></span>
<span data-ttu-id="793d3-207">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="793d3-208">Následující příklad vrací `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="793d3-208">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="793d3-209">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="793d3-209">skip</span></span>
<span data-ttu-id="793d3-210">Obchází zadaný počet elementů v kolekci a vrátí zbývající elementy.</span><span class="sxs-lookup"><span data-stu-id="793d3-210">Bypasses a specified number of elements in a collection, and then returns the remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="793d3-211">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-211">Example 1: string</span></span>
<span data-ttu-id="793d3-212">Následující příklad vrací `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-212">The following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="793d3-213">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="793d3-213">Example 2: array</span></span>
<span data-ttu-id="793d3-214">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="793d3-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="793d3-215">Následující příklad vrací `[3]`:</span><span class="sxs-lookup"><span data-stu-id="793d3-215">The following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="793d3-216">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="793d3-216">Example 3: object</span></span>
<span data-ttu-id="793d3-217">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="793d3-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="793d3-218">Následující příklad vrací `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="793d3-218">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="793d3-219">Logické funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-219">Logical functions</span></span>
<span data-ttu-id="793d3-220">Tyto funkce lze používat ve podmíněné příkazy.</span><span class="sxs-lookup"><span data-stu-id="793d3-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="793d3-221">Některé funkce nemusí podporovat všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="793d3-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="793d3-222">Rovná se</span><span class="sxs-lookup"><span data-stu-id="793d3-222">equals</span></span>
<span data-ttu-id="793d3-223">Vrátí `true` Pokud oba parametry mají stejný typ a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="793d3-223">Returns `true` if both parameters have the same type and value.</span></span> <span data-ttu-id="793d3-224">Tato funkce podporuje všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="793d3-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="793d3-225">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-225">The following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="793d3-226">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-226">The following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="793d3-227">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-227">The following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="793d3-228">menší</span><span class="sxs-lookup"><span data-stu-id="793d3-228">less</span></span>
<span data-ttu-id="793d3-229">Vrátí `true` Pokud první parametr je striktně menší než druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="793d3-229">Returns `true` if the first parameter is strictly less than the second parameter.</span></span> <span data-ttu-id="793d3-230">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="793d3-231">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-231">The following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="793d3-232">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-232">The following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="793d3-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="793d3-233">lessOrEquals</span></span>
<span data-ttu-id="793d3-234">Vrátí `true` Pokud první parametr je menší než nebo rovna druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="793d3-234">Returns `true` if the first parameter is less than or equal to the second parameter.</span></span> <span data-ttu-id="793d3-235">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="793d3-236">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-236">The following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="793d3-237">větší</span><span class="sxs-lookup"><span data-stu-id="793d3-237">greater</span></span>
<span data-ttu-id="793d3-238">Vrátí `true` Pokud první parametr je výhradně větší než druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="793d3-238">Returns `true` if the first parameter is strictly greater than the second parameter.</span></span> <span data-ttu-id="793d3-239">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="793d3-240">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-240">The following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="793d3-241">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-241">The following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="793d3-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="793d3-242">greaterOrEquals</span></span>
<span data-ttu-id="793d3-243">Vrátí `true` Pokud první parametr je větší než nebo rovna hodnotě druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="793d3-243">Returns `true` if the first parameter is greater than or equal to the second parameter.</span></span> <span data-ttu-id="793d3-244">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="793d3-245">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-245">The following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="793d3-246">a</span><span class="sxs-lookup"><span data-stu-id="793d3-246">and</span></span>
<span data-ttu-id="793d3-247">Vrátí `true` Pokud jsou všechny parametry vyhodnoceny `true`.</span><span class="sxs-lookup"><span data-stu-id="793d3-247">Returns `true` if all the parameters evaluate to `true`.</span></span> <span data-ttu-id="793d3-248">Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="793d3-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="793d3-249">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-249">The following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="793d3-250">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-250">The following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="793d3-251">nebo</span><span class="sxs-lookup"><span data-stu-id="793d3-251">or</span></span>
<span data-ttu-id="793d3-252">Vrátí `true` Pokud je alespoň jeden z parametrů výsledkem `true`.</span><span class="sxs-lookup"><span data-stu-id="793d3-252">Returns `true` if at least one of the parameters evaluates to `true`.</span></span> <span data-ttu-id="793d3-253">Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="793d3-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="793d3-254">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-254">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="793d3-255">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-255">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="793d3-256">není</span><span class="sxs-lookup"><span data-stu-id="793d3-256">not</span></span>
<span data-ttu-id="793d3-257">Vrátí `true` Pokud je parametr výsledkem `false`.</span><span class="sxs-lookup"><span data-stu-id="793d3-257">Returns `true` if the parameter evaluates to `false`.</span></span> <span data-ttu-id="793d3-258">Tato funkce podporuje jenom parametry typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="793d3-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="793d3-259">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-259">The following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="793d3-260">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-260">The following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="793d3-261">sloučení</span><span class="sxs-lookup"><span data-stu-id="793d3-261">coalesce</span></span>
<span data-ttu-id="793d3-262">Vrátí hodnotu prvního parametru nesmí být nulová.</span><span class="sxs-lookup"><span data-stu-id="793d3-262">Returns the value of the first non-null parameter.</span></span> <span data-ttu-id="793d3-263">Tato funkce podporuje všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="793d3-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="793d3-264">Předpokládejme `element1` a `element2` nejsou definovány.</span><span class="sxs-lookup"><span data-stu-id="793d3-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="793d3-265">Následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-265">The following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="793d3-266">Převodní funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-266">Conversion functions</span></span>
<span data-ttu-id="793d3-267">Tyto funkce lze převést hodnoty mezi typy dat JSON a kódování.</span><span class="sxs-lookup"><span data-stu-id="793d3-267">These functions can be used to convert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="793d3-268">celá čísla</span><span class="sxs-lookup"><span data-stu-id="793d3-268">int</span></span>
<span data-ttu-id="793d3-269">Parametr převede na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="793d3-269">Converts the parameter to an integer.</span></span> <span data-ttu-id="793d3-270">Tato funkce podporuje parametry počet typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="793d3-271">Následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="793d3-271">The following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="793d3-272">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-272">The following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="793d3-273">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="793d3-273">float</span></span>
<span data-ttu-id="793d3-274">Převede parametr s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="793d3-274">Converts the parameter to a floating-point.</span></span> <span data-ttu-id="793d3-275">Tato funkce podporuje parametry počet typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="793d3-276">Následující příklad vrací `1.0`:</span><span class="sxs-lookup"><span data-stu-id="793d3-276">The following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="793d3-277">Následující příklad vrací `2.9`:</span><span class="sxs-lookup"><span data-stu-id="793d3-277">The following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="793d3-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="793d3-278">string</span></span>
<span data-ttu-id="793d3-279">Převede parametr na řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-279">Converts the parameter to a string.</span></span> <span data-ttu-id="793d3-280">Tato funkce podporuje parametry všech typů dat JSON.</span><span class="sxs-lookup"><span data-stu-id="793d3-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="793d3-281">Následující příklad vrací `"1"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-281">The following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="793d3-282">Následující příklad vrací `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-282">The following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="793d3-283">Následující příklad vrací `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-283">The following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="793d3-284">Následující příklad vrací `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-284">The following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="793d3-285">BOOL</span><span class="sxs-lookup"><span data-stu-id="793d3-285">bool</span></span>
<span data-ttu-id="793d3-286">Parametr převede na booleovskou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="793d3-286">Converts the parameter to a Boolean.</span></span> <span data-ttu-id="793d3-287">Tato funkce podporuje parametry typ čísla, řetězce a logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="793d3-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="793d3-288">Podobně jako u logických výrazů v jazyce JavaScript, některá hodnota s výjimkou `0` nebo `'false'` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="793d3-288">Similar to Booleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="793d3-289">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-289">The following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="793d3-290">Následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="793d3-290">The following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="793d3-291">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-291">The following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="793d3-292">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-292">The following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="793d3-293">analyzovat</span><span class="sxs-lookup"><span data-stu-id="793d3-293">parse</span></span>
<span data-ttu-id="793d3-294">Převede parametr nativního typu.</span><span class="sxs-lookup"><span data-stu-id="793d3-294">Converts the parameter to a native type.</span></span> <span data-ttu-id="793d3-295">Jinými slovy, tato funkce je opakem `string()`.</span><span class="sxs-lookup"><span data-stu-id="793d3-295">In other words, this function is the inverse of `string()`.</span></span> <span data-ttu-id="793d3-296">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="793d3-297">Následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="793d3-297">The following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="793d3-298">Následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="793d3-298">The following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="793d3-299">Následující příklad vrací `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="793d3-299">The following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="793d3-300">Následující příklad vrací `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="793d3-300">The following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="793d3-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="793d3-301">encodeBase64</span></span>
<span data-ttu-id="793d3-302">Kóduje parametr na řetězec s kódováním base-64.</span><span class="sxs-lookup"><span data-stu-id="793d3-302">Encodes the parameter to a base-64 encoded string.</span></span> <span data-ttu-id="793d3-303">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="793d3-304">Následující příklad vrací `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-304">The following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="793d3-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="793d3-305">decodeBase64</span></span>
<span data-ttu-id="793d3-306">Dekóduje parametru z řetězec s kódováním base-64.</span><span class="sxs-lookup"><span data-stu-id="793d3-306">Decodes the parameter from a base-64 encoded string.</span></span> <span data-ttu-id="793d3-307">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="793d3-308">Následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-308">The following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="793d3-309">encodeuricomponent –</span><span class="sxs-lookup"><span data-stu-id="793d3-309">encodeUriComponent</span></span>
<span data-ttu-id="793d3-310">Kóduje parametr na řetězec kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="793d3-310">Encodes the parameter to a URL encoded string.</span></span> <span data-ttu-id="793d3-311">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="793d3-312">Následující příklad vrací `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-312">The following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="793d3-313">decodeuricomponent –</span><span class="sxs-lookup"><span data-stu-id="793d3-313">decodeUriComponent</span></span>
<span data-ttu-id="793d3-314">Dekóduje parametr v řetězci kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="793d3-314">Decodes the parameter from a URL encoded string.</span></span> <span data-ttu-id="793d3-315">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="793d3-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="793d3-316">Následující příklad vrací `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-316">The following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="793d3-317">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="793d3-318">Přidat</span><span class="sxs-lookup"><span data-stu-id="793d3-318">add</span></span>
<span data-ttu-id="793d3-319">Sečte dvě čísla a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="793d3-319">Adds two numbers, and returns the result.</span></span>

<span data-ttu-id="793d3-320">Následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="793d3-320">The following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="793d3-321">Sub –</span><span class="sxs-lookup"><span data-stu-id="793d3-321">sub</span></span>
<span data-ttu-id="793d3-322">Odečítá od druhé číslo z první číslo a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="793d3-322">Subtracts the second number from the first number, and returns the result.</span></span>

<span data-ttu-id="793d3-323">Následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="793d3-323">The following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="793d3-324">mul</span><span class="sxs-lookup"><span data-stu-id="793d3-324">mul</span></span>
<span data-ttu-id="793d3-325">Vynásobí dvou čísel a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="793d3-325">Multiplies two numbers, and returns the result.</span></span>

<span data-ttu-id="793d3-326">Následující příklad vrací `6`:</span><span class="sxs-lookup"><span data-stu-id="793d3-326">The following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="793d3-327">div</span><span class="sxs-lookup"><span data-stu-id="793d3-327">div</span></span>
<span data-ttu-id="793d3-328">Rozdělí prvního čísla druhé číslo a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="793d3-328">Divides the first number by the second number, and returns the result.</span></span> <span data-ttu-id="793d3-329">Výsledkem je vždy celé číslo.</span><span class="sxs-lookup"><span data-stu-id="793d3-329">The result is always an integer.</span></span>

<span data-ttu-id="793d3-330">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-330">The following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="793d3-331">MOD</span><span class="sxs-lookup"><span data-stu-id="793d3-331">mod</span></span>
<span data-ttu-id="793d3-332">Provede podíl prvního čísla druhé číslo a vrátí zbytek.</span><span class="sxs-lookup"><span data-stu-id="793d3-332">Divides the first number by the second number, and returns the remainder.</span></span>

<span data-ttu-id="793d3-333">Následující příklad vrací `0`:</span><span class="sxs-lookup"><span data-stu-id="793d3-333">The following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="793d3-334">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-334">The following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="793d3-335">min</span><span class="sxs-lookup"><span data-stu-id="793d3-335">min</span></span>
<span data-ttu-id="793d3-336">Vrátí malým dvou čísel.</span><span class="sxs-lookup"><span data-stu-id="793d3-336">Returns the small of the two numbers.</span></span>

<span data-ttu-id="793d3-337">Následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="793d3-337">The following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="793d3-338">maximální počet</span><span class="sxs-lookup"><span data-stu-id="793d3-338">max</span></span>
<span data-ttu-id="793d3-339">Vrátí větší dvou čísel.</span><span class="sxs-lookup"><span data-stu-id="793d3-339">Returns the larger of the two numbers.</span></span>

<span data-ttu-id="793d3-340">Následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="793d3-340">The following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="793d3-341">rozsah</span><span class="sxs-lookup"><span data-stu-id="793d3-341">range</span></span>
<span data-ttu-id="793d3-342">Generuje pořadí integrální čísel v daném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="793d3-342">Generates a sequence of integral numbers within the specified range.</span></span>

<span data-ttu-id="793d3-343">Následující příklad vrací `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="793d3-343">The following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="793d3-344">rand –</span><span class="sxs-lookup"><span data-stu-id="793d3-344">rand</span></span>
<span data-ttu-id="793d3-345">Vrátí náhodné číslo integrální v zadaném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="793d3-345">Returns a random integral number within the specified range.</span></span> <span data-ttu-id="793d3-346">Tato funkce negeneruje kryptograficky zabezpečené náhodných čísel.</span><span class="sxs-lookup"><span data-stu-id="793d3-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="793d3-347">V následujícím příkladu může vrátit `42`:</span><span class="sxs-lookup"><span data-stu-id="793d3-347">The following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="793d3-348">Floor</span><span class="sxs-lookup"><span data-stu-id="793d3-348">floor</span></span>
<span data-ttu-id="793d3-349">Vrátí největší celé číslo menší než nebo rovna zadané číslo.</span><span class="sxs-lookup"><span data-stu-id="793d3-349">Returns the largest integer less than or equal to the specified number.</span></span>

<span data-ttu-id="793d3-350">Následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="793d3-350">The following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="793d3-351">ceil</span><span class="sxs-lookup"><span data-stu-id="793d3-351">ceil</span></span>
<span data-ttu-id="793d3-352">Vrátí největší celé číslo větší než nebo rovna hodnotě zadané číslo.</span><span class="sxs-lookup"><span data-stu-id="793d3-352">Returns the largest integer greater than or equal to the specified number.</span></span>

<span data-ttu-id="793d3-353">Následující příklad vrací `4`:</span><span class="sxs-lookup"><span data-stu-id="793d3-353">The following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="793d3-354">Datové funkce</span><span class="sxs-lookup"><span data-stu-id="793d3-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="793d3-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="793d3-355">utcNow</span></span>
<span data-ttu-id="793d3-356">Vrací řetězec ve formátu ISO 8601 aktuální datum a čas v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="793d3-356">Returns a string in ISO 8601 format of the current date and time on the local computer.</span></span>

<span data-ttu-id="793d3-357">V následujícím příkladu může vrátit `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-357">The following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="793d3-358">Přidat_sekundy</span><span class="sxs-lookup"><span data-stu-id="793d3-358">addSeconds</span></span>
<span data-ttu-id="793d3-359">Přidá celočíselný počet sekund pro zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="793d3-359">Adds an integral number of seconds to the specified timestamp.</span></span>

<span data-ttu-id="793d3-360">Následující příklad vrací `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-360">The following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="793d3-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="793d3-361">addMinutes</span></span>
<span data-ttu-id="793d3-362">Přidá celočíselný počet minut pro zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="793d3-362">Adds an integral number of minutes to the specified timestamp.</span></span>

<span data-ttu-id="793d3-363">Následující příklad vrací `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-363">The following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="793d3-364">addHours</span><span class="sxs-lookup"><span data-stu-id="793d3-364">addHours</span></span>
<span data-ttu-id="793d3-365">Přidá celočíselný počet hodin na zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="793d3-365">Adds an integral number of hours to the specified timestamp.</span></span>

<span data-ttu-id="793d3-366">Následující příklad vrací `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="793d3-366">The following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="793d3-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="793d3-367">Next steps</span></span>
* <span data-ttu-id="793d3-368">Úvod do Azure Resource Manageru, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="793d3-368">For an introduction to Azure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

