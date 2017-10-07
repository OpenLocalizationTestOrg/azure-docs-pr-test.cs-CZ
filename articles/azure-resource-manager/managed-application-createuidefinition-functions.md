---
title: "aaaAzure spravované aplikace vytvořit definici funkcí uživatelského rozhraní | Microsoft Docs"
description: "Popisuje funkce toouse hello při vytváření definice uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 7a311a25404ccaec8c19c3ed8cd7038f6887c013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="5cb90-103">Funkce CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="5cb90-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="5cb90-104">Tato část obsahuje hello podpisy pro všechny podporované funkce CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="5cb90-104">This section contains hello signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="5cb90-105">toouse funkci, příkazu obklopit hello deklaraci do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="5cb90-105">toouse a function, surround hello declaration with square brackets.</span></span> <span data-ttu-id="5cb90-106">Například:</span><span class="sxs-lookup"><span data-stu-id="5cb90-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="5cb90-107">Řetězce a dalších funkcí, může být odkazován jako parametry pro funkci, ale řetězce musí být uzavřena do jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="5cb90-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="5cb90-108">Například:</span><span class="sxs-lookup"><span data-stu-id="5cb90-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="5cb90-109">Případně můžete odkazovat vlastnosti výstup hello funkce pomocí hello tečka – operátor.</span><span class="sxs-lookup"><span data-stu-id="5cb90-109">Where applicable, you can reference properties of hello output of a function by using hello dot operator.</span></span> <span data-ttu-id="5cb90-110">Například:</span><span class="sxs-lookup"><span data-stu-id="5cb90-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="5cb90-111">Odkazování na funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-111">Referencing functions</span></span>
<span data-ttu-id="5cb90-112">Tyto funkce můžou být použité tooreference výstupy z vlastnosti hello nebo kontextu CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="5cb90-112">These functions can be used tooreference outputs from hello properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="5cb90-113">Základy</span><span class="sxs-lookup"><span data-stu-id="5cb90-113">basics</span></span>
<span data-ttu-id="5cb90-114">Vrátí hodnoty výstup hello elementu, který je definovaný v kroku základy hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-114">Returns hello output values of an element that is defined in hello Basics step.</span></span>

<span data-ttu-id="5cb90-115">Hello následující příklad vrátí hello výstup hello elementu s názvem `foo` v kroku základy hello:</span><span class="sxs-lookup"><span data-stu-id="5cb90-115">hello following example returns hello output of hello element named `foo` in hello Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="5cb90-116">kroky</span><span class="sxs-lookup"><span data-stu-id="5cb90-116">steps</span></span>
<span data-ttu-id="5cb90-117">Vrátí hodnoty výstup hello elementu, který je definovaný v kroku zadaný hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-117">Returns hello output values of an element that is defined in hello specified step.</span></span> <span data-ttu-id="5cb90-118">použít tooget hello výstupní hodnoty elementů v kroku základy hello `basics()` místo.</span><span class="sxs-lookup"><span data-stu-id="5cb90-118">tooget hello output values of elements in hello Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="5cb90-119">Hello následující příklad vrátí hello výstup hello elementu s názvem `bar` v kroku hello s názvem `foo`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-119">hello following example returns hello output of hello element named `bar` in hello step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="5cb90-120">location</span><span class="sxs-lookup"><span data-stu-id="5cb90-120">location</span></span>
<span data-ttu-id="5cb90-121">Vrátí vybrané v kroku základy hello nebo aktuální kontext hello hello umístění.</span><span class="sxs-lookup"><span data-stu-id="5cb90-121">Returns hello location selected in hello Basics step or hello current context.</span></span>

<span data-ttu-id="5cb90-122">Hello následující příklad může vrátit `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-122">hello following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="5cb90-123">Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-123">String functions</span></span>
<span data-ttu-id="5cb90-124">Tyto funkce slouží pouze s řetězce formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="5cb90-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="5cb90-125">concat</span><span class="sxs-lookup"><span data-stu-id="5cb90-125">concat</span></span>
<span data-ttu-id="5cb90-126">Zřetězí nejméně jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="5cb90-127">Například, pokud hodnota výstup hello `element1` Pokud `"bar"`, pak tento příklad vrátí řetězec hello `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-127">For example, if hello output value of `element1` if `"bar"`, then this example returns hello string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="5cb90-128">dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-128">substring</span></span>
<span data-ttu-id="5cb90-129">Vrátí hello podřetězcem hello zadaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-129">Returns hello substring of hello specified string.</span></span> <span data-ttu-id="5cb90-130">Hello substring začne u zadaného indexu hello a má zadaný hello délka.</span><span class="sxs-lookup"><span data-stu-id="5cb90-130">hello substring starts at hello specified index and has hello specified length.</span></span>

<span data-ttu-id="5cb90-131">Hello následující příklad vrací `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-131">hello following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="5cb90-132">Nahradit</span><span class="sxs-lookup"><span data-stu-id="5cb90-132">replace</span></span>
<span data-ttu-id="5cb90-133">Vrátí řetězec, ve které všechny výskyty hello zadání řetězce v aktuálním řetězci hello se nahradí jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="5cb90-133">Returns a string in which all occurrences of hello specified string in hello current string are replaced with another string.</span></span>

<span data-ttu-id="5cb90-134">Hello následující příklad vrací `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-134">hello following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="5cb90-135">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="5cb90-135">guid</span></span>
<span data-ttu-id="5cb90-136">Generuje řetězec globálně jedinečný (identifikátor GUID).</span><span class="sxs-lookup"><span data-stu-id="5cb90-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="5cb90-137">Hello následující příklad může vrátit `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-137">hello following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="5cb90-138">toLower</span><span class="sxs-lookup"><span data-stu-id="5cb90-138">toLower</span></span>
<span data-ttu-id="5cb90-139">Vrátí řetězec převedený toolowercase.</span><span class="sxs-lookup"><span data-stu-id="5cb90-139">Returns a string converted toolowercase.</span></span>

<span data-ttu-id="5cb90-140">Hello následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-140">hello following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="5cb90-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="5cb90-141">toUpper</span></span>
<span data-ttu-id="5cb90-142">Vrátí řetězec převedený toouppercase.</span><span class="sxs-lookup"><span data-stu-id="5cb90-142">Returns a string converted toouppercase.</span></span>

<span data-ttu-id="5cb90-143">Hello následující příklad vrací `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-143">hello following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="5cb90-144">Kolekce funkcí</span><span class="sxs-lookup"><span data-stu-id="5cb90-144">Collection functions</span></span>
<span data-ttu-id="5cb90-145">Tyto funkce slouží ke kolekcím, jako je řetězce formátu JSON, pole a objekty.</span><span class="sxs-lookup"><span data-stu-id="5cb90-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="5cb90-146">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="5cb90-146">contains</span></span>
<span data-ttu-id="5cb90-147">Vrátí `true` řetězec obsahuje hello určený dílčí řetězec, obsahuje pole hello zadané hodnotě, nebo objekt obsahuje zadaný klíč hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-147">Returns `true` if a string contains hello specified substring, an array contains hello specified value, or an object contains hello specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-148">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-148">Example 1: string</span></span>
<span data-ttu-id="5cb90-149">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-149">hello following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-150">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-150">Example 2: array</span></span>
<span data-ttu-id="5cb90-151">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-152">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-152">hello following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-153">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-153">Example 3: object</span></span>
<span data-ttu-id="5cb90-154">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="5cb90-155">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-155">hello following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="5cb90-156">Délka</span><span class="sxs-lookup"><span data-stu-id="5cb90-156">length</span></span>
<span data-ttu-id="5cb90-157">Vrátí hello počet znaků v řetězci, hello počet hodnot v matici nebo hello počet klíčů v objektu.</span><span class="sxs-lookup"><span data-stu-id="5cb90-157">Returns hello number of characters in a string, hello number of values in an array, or hello number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-158">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-158">Example 1: string</span></span>
<span data-ttu-id="5cb90-159">Hello následující příklad vrací `6`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-159">hello following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-160">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-160">Example 2: array</span></span>
<span data-ttu-id="5cb90-161">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-162">Hello následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-162">hello following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-163">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-163">Example 3: object</span></span>
<span data-ttu-id="5cb90-164">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="5cb90-165">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-165">hello following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="5cb90-166">prázdný</span><span class="sxs-lookup"><span data-stu-id="5cb90-166">empty</span></span>
<span data-ttu-id="5cb90-167">Vrátí `true` Pokud hello řetězec, pole nebo objekt je null nebo prázdná.</span><span class="sxs-lookup"><span data-stu-id="5cb90-167">Returns `true` if hello string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-168">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-168">Example 1: string</span></span>
<span data-ttu-id="5cb90-169">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-169">hello following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-170">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-170">Example 2: array</span></span>
<span data-ttu-id="5cb90-171">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-172">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-172">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-173">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-173">Example 3: object</span></span>
<span data-ttu-id="5cb90-174">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="5cb90-175">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-175">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="5cb90-176">Příklad 4: null a Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="5cb90-176">Example 4: null and undefined</span></span>
<span data-ttu-id="5cb90-177">Předpokládejme `element1` je `null` nebo nedefinované.</span><span class="sxs-lookup"><span data-stu-id="5cb90-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="5cb90-178">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-178">hello following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="5cb90-179">první</span><span class="sxs-lookup"><span data-stu-id="5cb90-179">first</span></span>
<span data-ttu-id="5cb90-180">Vrátí hello první znak hello zadaný řetězec; první hodnotu zadaného pole hello; nebo hello první klíč a hodnotu zadaného objektu hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-180">Returns hello first character of hello specified string; first value of hello specified array; or hello first key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-181">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-181">Example 1: string</span></span>
<span data-ttu-id="5cb90-182">Hello následující příklad vrací `"f"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-182">hello following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-183">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-183">Example 2: array</span></span>
<span data-ttu-id="5cb90-184">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-185">Hello následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-185">hello following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-186">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-186">Example 3: object</span></span>
<span data-ttu-id="5cb90-187">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="5cb90-188">Hello následující příklad vrací `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-188">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="5cb90-189">poslední</span><span class="sxs-lookup"><span data-stu-id="5cb90-189">last</span></span>
<span data-ttu-id="5cb90-190">Vrátí hello poslední znak hello zadaný řetězec hello poslední hodnotu zadaného pole hello, nebo hello poslední klíč a hodnotu zadaného objektu hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-190">Returns hello last character of hello specified string, hello last value of hello specified array, or hello last key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-191">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-191">Example 1: string</span></span>
<span data-ttu-id="5cb90-192">Hello následující příklad vrací `"r"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-192">hello following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-193">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-193">Example 2: array</span></span>
<span data-ttu-id="5cb90-194">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-195">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-195">hello following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-196">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-196">Example 3: object</span></span>
<span data-ttu-id="5cb90-197">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="5cb90-198">Hello následující příklad vrací `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-198">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="5cb90-199">proveďte</span><span class="sxs-lookup"><span data-stu-id="5cb90-199">take</span></span>
<span data-ttu-id="5cb90-200">Vrátí zadaný počet po sobě jdoucích znaků z počátku hello hello řetězce, zadaný počet souvislý hodnoty od začátku hello hello pole nebo zadaný počet sousedních klíčů a hodnot od začátku hello hello objektu.</span><span class="sxs-lookup"><span data-stu-id="5cb90-200">Returns a specified number of contiguous characters from hello start of hello string, a specified number of contiguous values from hello start of hello array, or a specified number of contiguous keys and values from hello start of hello object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-201">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-201">Example 1: string</span></span>
<span data-ttu-id="5cb90-202">Hello následující příklad vrací `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-202">hello following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-203">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-203">Example 2: array</span></span>
<span data-ttu-id="5cb90-204">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-205">Hello následující příklad vrací `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-205">hello following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-206">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-206">Example 3: object</span></span>
<span data-ttu-id="5cb90-207">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="5cb90-208">Hello následující příklad vrací `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-208">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="5cb90-209">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="5cb90-209">skip</span></span>
<span data-ttu-id="5cb90-210">Obchází zadaný počet elementů v kolekci a vrátí hello zbývající elementy.</span><span class="sxs-lookup"><span data-stu-id="5cb90-210">Bypasses a specified number of elements in a collection, and then returns hello remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="5cb90-211">Příklad 1: řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-211">Example 1: string</span></span>
<span data-ttu-id="5cb90-212">Hello následující příklad vrací `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-212">hello following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="5cb90-213">Příklad 2: pole</span><span class="sxs-lookup"><span data-stu-id="5cb90-213">Example 2: array</span></span>
<span data-ttu-id="5cb90-214">Předpokládejme `element1` vrátí `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="5cb90-215">Hello následující příklad vrací `[3]`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-215">hello following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="5cb90-216">Příklad 3: objekt</span><span class="sxs-lookup"><span data-stu-id="5cb90-216">Example 3: object</span></span>
<span data-ttu-id="5cb90-217">Předpokládejme `element1` vrátí:</span><span class="sxs-lookup"><span data-stu-id="5cb90-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="5cb90-218">Hello následující příklad vrací `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-218">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="5cb90-219">Logické funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-219">Logical functions</span></span>
<span data-ttu-id="5cb90-220">Tyto funkce lze používat ve podmíněné příkazy.</span><span class="sxs-lookup"><span data-stu-id="5cb90-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="5cb90-221">Některé funkce nemusí podporovat všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="5cb90-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="5cb90-222">Rovná se</span><span class="sxs-lookup"><span data-stu-id="5cb90-222">equals</span></span>
<span data-ttu-id="5cb90-223">Vrátí `true` Pokud oba parametry mají hello stejný typ a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5cb90-223">Returns `true` if both parameters have hello same type and value.</span></span> <span data-ttu-id="5cb90-224">Tato funkce podporuje všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="5cb90-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="5cb90-225">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-225">hello following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="5cb90-226">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-226">hello following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="5cb90-227">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-227">hello following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="5cb90-228">menší</span><span class="sxs-lookup"><span data-stu-id="5cb90-228">less</span></span>
<span data-ttu-id="5cb90-229">Vrátí `true` Pokud hello první parametr je striktně menší než druhý parametr hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-229">Returns `true` if hello first parameter is strictly less than hello second parameter.</span></span> <span data-ttu-id="5cb90-230">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="5cb90-231">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-231">hello following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="5cb90-232">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-232">hello following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="5cb90-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="5cb90-233">lessOrEquals</span></span>
<span data-ttu-id="5cb90-234">Vrátí `true` Pokud hello první parametr je menší než nebo rovna toohello druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="5cb90-234">Returns `true` if hello first parameter is less than or equal toohello second parameter.</span></span> <span data-ttu-id="5cb90-235">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="5cb90-236">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-236">hello following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="5cb90-237">větší</span><span class="sxs-lookup"><span data-stu-id="5cb90-237">greater</span></span>
<span data-ttu-id="5cb90-238">Vrátí `true` Pokud je první parametr hello striktně větší než druhý parametr hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-238">Returns `true` if hello first parameter is strictly greater than hello second parameter.</span></span> <span data-ttu-id="5cb90-239">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="5cb90-240">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-240">hello following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="5cb90-241">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-241">hello following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="5cb90-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="5cb90-242">greaterOrEquals</span></span>
<span data-ttu-id="5cb90-243">Vrátí `true` Pokud hello první parametr je větší než nebo rovna toohello druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="5cb90-243">Returns `true` if hello first parameter is greater than or equal toohello second parameter.</span></span> <span data-ttu-id="5cb90-244">Tato funkce podporuje parametrů jenom číslo typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="5cb90-245">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-245">hello following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="5cb90-246">a</span><span class="sxs-lookup"><span data-stu-id="5cb90-246">and</span></span>
<span data-ttu-id="5cb90-247">Vrátí `true` Pokud všechny parametry hello vyhodnocení příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-247">Returns `true` if all hello parameters evaluate too`true`.</span></span> <span data-ttu-id="5cb90-248">Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="5cb90-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="5cb90-249">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-249">hello following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="5cb90-250">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-250">hello following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="5cb90-251">nebo</span><span class="sxs-lookup"><span data-stu-id="5cb90-251">or</span></span>
<span data-ttu-id="5cb90-252">Vrátí `true` pokud alespoň jeden z parametrů hello vyhodnotí příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-252">Returns `true` if at least one of hello parameters evaluates too`true`.</span></span> <span data-ttu-id="5cb90-253">Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="5cb90-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="5cb90-254">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-254">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="5cb90-255">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-255">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="5cb90-256">není</span><span class="sxs-lookup"><span data-stu-id="5cb90-256">not</span></span>
<span data-ttu-id="5cb90-257">Vrátí `true` Pokud parametr hello vyhodnotí příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-257">Returns `true` if hello parameter evaluates too`false`.</span></span> <span data-ttu-id="5cb90-258">Tato funkce podporuje jenom parametry typu logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="5cb90-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="5cb90-259">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-259">hello following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="5cb90-260">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-260">hello following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="5cb90-261">sloučení</span><span class="sxs-lookup"><span data-stu-id="5cb90-261">coalesce</span></span>
<span data-ttu-id="5cb90-262">Vrátí hello hodnotu hello první parametr nesmí být nulová.</span><span class="sxs-lookup"><span data-stu-id="5cb90-262">Returns hello value of hello first non-null parameter.</span></span> <span data-ttu-id="5cb90-263">Tato funkce podporuje všechny typy dat JSON.</span><span class="sxs-lookup"><span data-stu-id="5cb90-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="5cb90-264">Předpokládejme `element1` a `element2` nejsou definovány.</span><span class="sxs-lookup"><span data-stu-id="5cb90-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="5cb90-265">Hello následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-265">hello following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="5cb90-266">Převodní funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-266">Conversion functions</span></span>
<span data-ttu-id="5cb90-267">Tyto funkce lze použít tooconvert hodnoty mezi typy dat JSON a kódování.</span><span class="sxs-lookup"><span data-stu-id="5cb90-267">These functions can be used tooconvert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="5cb90-268">celá čísla</span><span class="sxs-lookup"><span data-stu-id="5cb90-268">int</span></span>
<span data-ttu-id="5cb90-269">Převede hello parametr tooan celé číslo.</span><span class="sxs-lookup"><span data-stu-id="5cb90-269">Converts hello parameter tooan integer.</span></span> <span data-ttu-id="5cb90-270">Tato funkce podporuje parametry počet typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="5cb90-271">Hello následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-271">hello following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="5cb90-272">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-272">hello following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="5cb90-273">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="5cb90-273">float</span></span>
<span data-ttu-id="5cb90-274">Převede hello parametr tooa s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="5cb90-274">Converts hello parameter tooa floating-point.</span></span> <span data-ttu-id="5cb90-275">Tato funkce podporuje parametry počet typ a řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="5cb90-276">Hello následující příklad vrací `1.0`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-276">hello following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="5cb90-277">Hello následující příklad vrací `2.9`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-277">hello following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="5cb90-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5cb90-278">string</span></span>
<span data-ttu-id="5cb90-279">Převede řetězec tooa hello parametrů.</span><span class="sxs-lookup"><span data-stu-id="5cb90-279">Converts hello parameter tooa string.</span></span> <span data-ttu-id="5cb90-280">Tato funkce podporuje parametry všech typů dat JSON.</span><span class="sxs-lookup"><span data-stu-id="5cb90-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="5cb90-281">Hello následující příklad vrací `"1"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-281">hello following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="5cb90-282">Hello následující příklad vrací `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-282">hello following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="5cb90-283">Hello následující příklad vrací `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-283">hello following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="5cb90-284">Hello následující příklad vrací `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-284">hello following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="5cb90-285">BOOL</span><span class="sxs-lookup"><span data-stu-id="5cb90-285">bool</span></span>
<span data-ttu-id="5cb90-286">Převede hello parametr tooa logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="5cb90-286">Converts hello parameter tooa Boolean.</span></span> <span data-ttu-id="5cb90-287">Tato funkce podporuje parametry typ čísla, řetězce a logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5cb90-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="5cb90-288">Podobně jako tooBooleans v jazyce JavaScript, libovolná hodnota s výjimkou `0` nebo `'false'` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-288">Similar tooBooleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="5cb90-289">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-289">hello following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="5cb90-290">Hello následující příklad vrací `false`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-290">hello following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="5cb90-291">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-291">hello following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="5cb90-292">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-292">hello following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="5cb90-293">analyzovat</span><span class="sxs-lookup"><span data-stu-id="5cb90-293">parse</span></span>
<span data-ttu-id="5cb90-294">Převede nativní typ tooa hello parametru.</span><span class="sxs-lookup"><span data-stu-id="5cb90-294">Converts hello parameter tooa native type.</span></span> <span data-ttu-id="5cb90-295">Jinými slovy, tato funkce je inverzní hello `string()`.</span><span class="sxs-lookup"><span data-stu-id="5cb90-295">In other words, this function is hello inverse of `string()`.</span></span> <span data-ttu-id="5cb90-296">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="5cb90-297">Hello následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-297">hello following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="5cb90-298">Hello následující příklad vrací `true`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-298">hello following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="5cb90-299">Hello následující příklad vrací `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-299">hello following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="5cb90-300">Hello následující příklad vrací `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-300">hello following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="5cb90-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="5cb90-301">encodeBase64</span></span>
<span data-ttu-id="5cb90-302">Kóduje kódovaného řetězec hello parametrů tooa kódování base-64.</span><span class="sxs-lookup"><span data-stu-id="5cb90-302">Encodes hello parameter tooa base-64 encoded string.</span></span> <span data-ttu-id="5cb90-303">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="5cb90-304">Hello následující příklad vrací `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-304">hello following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="5cb90-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="5cb90-305">decodeBase64</span></span>
<span data-ttu-id="5cb90-306">Dekóduje parametr hello z řetězec s kódováním base-64.</span><span class="sxs-lookup"><span data-stu-id="5cb90-306">Decodes hello parameter from a base-64 encoded string.</span></span> <span data-ttu-id="5cb90-307">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="5cb90-308">Hello následující příklad vrací `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-308">hello following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="5cb90-309">encodeuricomponent –</span><span class="sxs-lookup"><span data-stu-id="5cb90-309">encodeUriComponent</span></span>
<span data-ttu-id="5cb90-310">Kóduje řetězec hello parametrů tooa kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="5cb90-310">Encodes hello parameter tooa URL encoded string.</span></span> <span data-ttu-id="5cb90-311">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="5cb90-312">Hello následující příklad vrací `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-312">hello following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="5cb90-313">decodeuricomponent –</span><span class="sxs-lookup"><span data-stu-id="5cb90-313">decodeUriComponent</span></span>
<span data-ttu-id="5cb90-314">Dekóduje hello parametr v řetězci kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="5cb90-314">Decodes hello parameter from a URL encoded string.</span></span> <span data-ttu-id="5cb90-315">Tato funkce podporuje jenom parametry typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5cb90-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="5cb90-316">Hello následující příklad vrací `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-316">hello following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="5cb90-317">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="5cb90-318">Přidat</span><span class="sxs-lookup"><span data-stu-id="5cb90-318">add</span></span>
<span data-ttu-id="5cb90-319">Sečte dvě čísla a vrátí výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-319">Adds two numbers, and returns hello result.</span></span>

<span data-ttu-id="5cb90-320">Hello následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-320">hello following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="5cb90-321">Sub –</span><span class="sxs-lookup"><span data-stu-id="5cb90-321">sub</span></span>
<span data-ttu-id="5cb90-322">Odečítá od hello druhé číslo z první číslo hello a vrátí výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-322">Subtracts hello second number from hello first number, and returns hello result.</span></span>

<span data-ttu-id="5cb90-323">Hello následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-323">hello following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="5cb90-324">mul</span><span class="sxs-lookup"><span data-stu-id="5cb90-324">mul</span></span>
<span data-ttu-id="5cb90-325">Vynásobí dvou čísel a vrátí výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-325">Multiplies two numbers, and returns hello result.</span></span>

<span data-ttu-id="5cb90-326">Hello následující příklad vrací `6`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-326">hello following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="5cb90-327">div</span><span class="sxs-lookup"><span data-stu-id="5cb90-327">div</span></span>
<span data-ttu-id="5cb90-328">Vydělí hello první číslo hello druhé číslo a vrátí výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-328">Divides hello first number by hello second number, and returns hello result.</span></span> <span data-ttu-id="5cb90-329">Hello výsledek je vždy celé číslo.</span><span class="sxs-lookup"><span data-stu-id="5cb90-329">hello result is always an integer.</span></span>

<span data-ttu-id="5cb90-330">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-330">hello following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="5cb90-331">MOD</span><span class="sxs-lookup"><span data-stu-id="5cb90-331">mod</span></span>
<span data-ttu-id="5cb90-332">Vydělí hello první číslo hello druhé číslo a vrátí zbytek hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-332">Divides hello first number by hello second number, and returns hello remainder.</span></span>

<span data-ttu-id="5cb90-333">Hello následující příklad vrací `0`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-333">hello following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="5cb90-334">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-334">hello following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="5cb90-335">min</span><span class="sxs-lookup"><span data-stu-id="5cb90-335">min</span></span>
<span data-ttu-id="5cb90-336">Vrátí hello malá hello dvou čísel.</span><span class="sxs-lookup"><span data-stu-id="5cb90-336">Returns hello small of hello two numbers.</span></span>

<span data-ttu-id="5cb90-337">Hello následující příklad vrací `1`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-337">hello following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="5cb90-338">maximální počet</span><span class="sxs-lookup"><span data-stu-id="5cb90-338">max</span></span>
<span data-ttu-id="5cb90-339">Vrátí hello větší hello dvou čísel.</span><span class="sxs-lookup"><span data-stu-id="5cb90-339">Returns hello larger of hello two numbers.</span></span>

<span data-ttu-id="5cb90-340">Hello následující příklad vrací `2`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-340">hello following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="5cb90-341">rozsah</span><span class="sxs-lookup"><span data-stu-id="5cb90-341">range</span></span>
<span data-ttu-id="5cb90-342">Generuje posloupnost integrální čísla v rámci hello zadaný rozsah.</span><span class="sxs-lookup"><span data-stu-id="5cb90-342">Generates a sequence of integral numbers within hello specified range.</span></span>

<span data-ttu-id="5cb90-343">Hello následující příklad vrací `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-343">hello following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="5cb90-344">rand –</span><span class="sxs-lookup"><span data-stu-id="5cb90-344">rand</span></span>
<span data-ttu-id="5cb90-345">Vrátí náhodné celé číslo v rámci hello zadaný rozsah.</span><span class="sxs-lookup"><span data-stu-id="5cb90-345">Returns a random integral number within hello specified range.</span></span> <span data-ttu-id="5cb90-346">Tato funkce negeneruje kryptograficky zabezpečené náhodných čísel.</span><span class="sxs-lookup"><span data-stu-id="5cb90-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="5cb90-347">Hello následující příklad může vrátit `42`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-347">hello following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="5cb90-348">Floor</span><span class="sxs-lookup"><span data-stu-id="5cb90-348">floor</span></span>
<span data-ttu-id="5cb90-349">Vrátí hello největší celé číslo menší než nebo rovna toohello zadané číslo.</span><span class="sxs-lookup"><span data-stu-id="5cb90-349">Returns hello largest integer less than or equal toohello specified number.</span></span>

<span data-ttu-id="5cb90-350">Hello následující příklad vrací `3`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-350">hello following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="5cb90-351">ceil</span><span class="sxs-lookup"><span data-stu-id="5cb90-351">ceil</span></span>
<span data-ttu-id="5cb90-352">Vrátí hello největší celé číslo větší než nebo rovna toohello zadané číslo.</span><span class="sxs-lookup"><span data-stu-id="5cb90-352">Returns hello largest integer greater than or equal toohello specified number.</span></span>

<span data-ttu-id="5cb90-353">Hello následující příklad vrací `4`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-353">hello following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="5cb90-354">Datové funkce</span><span class="sxs-lookup"><span data-stu-id="5cb90-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="5cb90-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="5cb90-355">utcNow</span></span>
<span data-ttu-id="5cb90-356">Vrací řetězec ve formátu ISO 8601 hello aktuální datum a čas v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="5cb90-356">Returns a string in ISO 8601 format of hello current date and time on hello local computer.</span></span>

<span data-ttu-id="5cb90-357">Hello následující příklad může vrátit `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-357">hello following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="5cb90-358">Přidat_sekundy</span><span class="sxs-lookup"><span data-stu-id="5cb90-358">addSeconds</span></span>
<span data-ttu-id="5cb90-359">Přidá celočíselný počet sekund toohello zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="5cb90-359">Adds an integral number of seconds toohello specified timestamp.</span></span>

<span data-ttu-id="5cb90-360">Hello následující příklad vrací `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-360">hello following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="5cb90-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="5cb90-361">addMinutes</span></span>
<span data-ttu-id="5cb90-362">Přidá celočíselný počet minut toohello zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="5cb90-362">Adds an integral number of minutes toohello specified timestamp.</span></span>

<span data-ttu-id="5cb90-363">Hello následující příklad vrací `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-363">hello following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="5cb90-364">addHours</span><span class="sxs-lookup"><span data-stu-id="5cb90-364">addHours</span></span>
<span data-ttu-id="5cb90-365">Přidá celočíselný počet hodin toohello zadané časové razítko.</span><span class="sxs-lookup"><span data-stu-id="5cb90-365">Adds an integral number of hours toohello specified timestamp.</span></span>

<span data-ttu-id="5cb90-366">Hello následující příklad vrací `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="5cb90-366">hello following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="5cb90-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5cb90-367">Next steps</span></span>
* <span data-ttu-id="5cb90-368">TooAzure Úvod Resource Manager, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5cb90-368">For an introduction tooAzure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

