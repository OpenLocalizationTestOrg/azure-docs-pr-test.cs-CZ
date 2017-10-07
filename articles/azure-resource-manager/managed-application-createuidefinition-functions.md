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
# <a name="createuidefinition-functions"></a>Funkce CreateUiDefinition
Tato část obsahuje hello podpisy pro všechny podporované funkce CreateUiDefinition.

toouse funkci, příkazu obklopit hello deklaraci do složených závorek. Například:

```json
"[function()]"
```

Řetězce a dalších funkcí, může být odkazován jako parametry pro funkci, ale řetězce musí být uzavřena do jednoduchých uvozovek. Například:

```json
"[fn1(fn2(), 'foobar')]"
```

Případně můžete odkazovat vlastnosti výstup hello funkce pomocí hello tečka – operátor. Například:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Odkazování na funkce
Tyto funkce můžou být použité tooreference výstupy z vlastnosti hello nebo kontextu CreateUiDefinition.

### <a name="basics"></a>Základy
Vrátí hodnoty výstup hello elementu, který je definovaný v kroku základy hello.

Hello následující příklad vrátí hello výstup hello elementu s názvem `foo` v kroku základy hello:

```json
"[basics('foo')]"
```

### <a name="steps"></a>kroky
Vrátí hodnoty výstup hello elementu, který je definovaný v kroku zadaný hello. použít tooget hello výstupní hodnoty elementů v kroku základy hello `basics()` místo.

Hello následující příklad vrátí hello výstup hello elementu s názvem `bar` v kroku hello s názvem `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Vrátí vybrané v kroku základy hello nebo aktuální kontext hello hello umístění.

Hello následující příklad může vrátit `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Řetězcové funkce
Tyto funkce slouží pouze s řetězce formátu JSON.

### <a name="concat"></a>concat
Zřetězí nejméně jeden řetězec.

Například, pokud hodnota výstup hello `element1` Pokud `"bar"`, pak tento příklad vrátí řetězec hello `"foobar!"`:

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a>dílčí řetězec
Vrátí hello podřetězcem hello zadaný řetězec. Hello substring začne u zadaného indexu hello a má zadaný hello délka.

Hello následující příklad vrací `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>Nahradit
Vrátí řetězec, ve které všechny výskyty hello zadání řetězce v aktuálním řetězci hello se nahradí jiným řetězcem.

Hello následující příklad vrací `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>Identifikátor GUID
Generuje řetězec globálně jedinečný (identifikátor GUID).

Hello následující příklad může vrátit `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Vrátí řetězec převedený toolowercase.

Hello následující příklad vrací `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Vrátí řetězec převedený toouppercase.

Hello následující příklad vrací `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Kolekce funkcí
Tyto funkce slouží ke kolekcím, jako je řetězce formátu JSON, pole a objekty.

### <a name="contains"></a>Obsahuje
Vrátí `true` řetězec obsahuje hello určený dílčí řetězec, obsahuje pole hello zadané hodnotě, nebo objekt obsahuje zadaný klíč hello.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Hello následující příklad vrací `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>Délka
Vrátí hello počet znaků v řetězci, hello počet hodnot v matici nebo hello počet klíčů v objektu.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Hello následující příklad vrací `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>prázdný
Vrátí `true` Pokud hello řetězec, pole nebo objekt je null nebo prázdná.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Hello následující příklad vrací `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Příklad 4: null a Nedefinovaná
Předpokládejme `element1` je `null` nebo nedefinované. Hello následující příklad vrací `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>první
Vrátí hello první znak hello zadaný řetězec; první hodnotu zadaného pole hello; nebo hello první klíč a hodnotu zadaného objektu hello.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Hello následující příklad vrací `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>poslední
Vrátí hello poslední znak hello zadaný řetězec hello poslední hodnotu zadaného pole hello, nebo hello poslední klíč a hodnotu zadaného objektu hello.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Hello následující příklad vrací `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>proveďte
Vrátí zadaný počet po sobě jdoucích znaků z počátku hello hello řetězce, zadaný počet souvislý hodnoty od začátku hello hello pole nebo zadaný počet sousedních klíčů a hodnot od začátku hello hello objektu.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Hello následující příklad vrací `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Přeskočit
Obchází zadaný počet elementů v kolekci a vrátí hello zbývající elementy.

#### <a name="example-1-string"></a>Příklad 1: řetězec
Hello následující příklad vrací `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Příklad 2: pole
Předpokládejme `element1` vrátí `[1, 2, 3]`. Hello následující příklad vrací `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Příklad 3: objekt
Předpokládejme `element1` vrátí:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Hello následující příklad vrací `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Logické funkce
Tyto funkce lze používat ve podmíněné příkazy. Některé funkce nemusí podporovat všechny typy dat JSON.

### <a name="equals"></a>Rovná se
Vrátí `true` Pokud oba parametry mají hello stejný typ a hodnotu. Tato funkce podporuje všechny typy dat JSON.

Hello následující příklad vrací `true`:

```json
"[equals(0, 0)]"
```

Hello následující příklad vrací `true`:

```json
"[equals('foo', 'foo')]"
```

Hello následující příklad vrací `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>menší
Vrátí `true` Pokud hello první parametr je striktně menší než druhý parametr hello. Tato funkce podporuje parametrů jenom číslo typ a řetězec.

Hello následující příklad vrací `true`:

```json
"[less(1, 2)]"
```

Hello následující příklad vrací `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Vrátí `true` Pokud hello první parametr je menší než nebo rovna toohello druhý parametr. Tato funkce podporuje parametrů jenom číslo typ a řetězec.

Hello následující příklad vrací `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>větší
Vrátí `true` Pokud je první parametr hello striktně větší než druhý parametr hello. Tato funkce podporuje parametrů jenom číslo typ a řetězec.

Hello následující příklad vrací `false`:

```json
"[greater(1, 2)]"
```

Hello následující příklad vrací `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Vrátí `true` Pokud hello první parametr je větší než nebo rovna toohello druhý parametr. Tato funkce podporuje parametrů jenom číslo typ a řetězec.

Hello následující příklad vrací `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>a
Vrátí `true` Pokud všechny parametry hello vyhodnocení příliš`true`. Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.

Hello následující příklad vrací `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Hello následující příklad vrací `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>nebo
Vrátí `true` pokud alespoň jeden z parametrů hello vyhodnotí příliš`true`. Tato funkce podporuje dva nebo více parametrů pouze typu logická hodnota.

Hello následující příklad vrací `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Hello následující příklad vrací `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>není
Vrátí `true` Pokud parametr hello vyhodnotí příliš`false`. Tato funkce podporuje jenom parametry typu logická hodnota.

Hello následující příklad vrací `true`:

```json
"[not(false)]"
```

Hello následující příklad vrací `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>sloučení
Vrátí hello hodnotu hello první parametr nesmí být nulová. Tato funkce podporuje všechny typy dat JSON.

Předpokládejme `element1` a `element2` nejsou definovány. Hello následující příklad vrací `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Převodní funkce
Tyto funkce lze použít tooconvert hodnoty mezi typy dat JSON a kódování.

### <a name="int"></a>celá čísla
Převede hello parametr tooan celé číslo. Tato funkce podporuje parametry počet typ a řetězec.

Hello následující příklad vrací `1`:

```json
"[int('1')]"
```

Hello následující příklad vrací `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>Plovoucí desetinná čárka
Převede hello parametr tooa s plovoucí desetinnou čárkou. Tato funkce podporuje parametry počet typ a řetězec.

Hello následující příklad vrací `1.0`:

```json
"[float('1.0')]"
```

Hello následující příklad vrací `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>Řetězec
Převede řetězec tooa hello parametrů. Tato funkce podporuje parametry všech typů dat JSON.

Hello následující příklad vrací `"1"`:

```json
"[string(1)]"
```

Hello následující příklad vrací `"2.9"`:

```json
"[string(2.9)]"
```

Hello následující příklad vrací `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

Hello následující příklad vrací `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>BOOL
Převede hello parametr tooa logická hodnota. Tato funkce podporuje parametry typ čísla, řetězce a logickou hodnotu. Podobně jako tooBooleans v jazyce JavaScript, libovolná hodnota s výjimkou `0` nebo `'false'` vrátí `true`.

Hello následující příklad vrací `true`:

```json
"[bool(1)]"
```

Hello následující příklad vrací `false`:

```json
"[bool(0)]"
```

Hello následující příklad vrací `true`:

```json
"[bool(true)]"
```

Hello následující příklad vrací `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>analyzovat
Převede nativní typ tooa hello parametru. Jinými slovy, tato funkce je inverzní hello `string()`. Tato funkce podporuje jenom parametry typu řetězec.

Hello následující příklad vrací `1`:

```json
"[parse('1')]"
```

Hello následující příklad vrací `true`:

```json
"[parse('true')]"
```

Hello následující příklad vrací `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

Hello následující příklad vrací `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Kóduje kódovaného řetězec hello parametrů tooa kódování base-64. Tato funkce podporuje jenom parametry typu řetězec.

Hello následující příklad vrací `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Dekóduje parametr hello z řetězec s kódováním base-64. Tato funkce podporuje jenom parametry typu řetězec.

Hello následující příklad vrací `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeuricomponent –
Kóduje řetězec hello parametrů tooa kódovaná adresou URL. Tato funkce podporuje jenom parametry typu řetězec.

Hello následující příklad vrací `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeuricomponent –
Dekóduje hello parametr v řetězci kódovaná adresou URL. Tato funkce podporuje jenom parametry typu řetězec.

Hello následující příklad vrací `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematické funkce
### <a name="add"></a>Přidat
Sečte dvě čísla a vrátí výsledek hello.

Hello následující příklad vrací `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub –
Odečítá od hello druhé číslo z první číslo hello a vrátí výsledek hello.

Hello následující příklad vrací `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
Vynásobí dvou čísel a vrátí výsledek hello.

Hello následující příklad vrací `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
Vydělí hello první číslo hello druhé číslo a vrátí výsledek hello. Hello výsledek je vždy celé číslo.

Hello následující příklad vrací `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>MOD
Vydělí hello první číslo hello druhé číslo a vrátí zbytek hello.

Hello následující příklad vrací `0`:

```json
"[mod(6, 3)]"
```

Hello následující příklad vrací `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>min
Vrátí hello malá hello dvou čísel.

Hello následující příklad vrací `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>maximální počet
Vrátí hello větší hello dvou čísel.

Hello následující příklad vrací `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>rozsah
Generuje posloupnost integrální čísla v rámci hello zadaný rozsah.

Hello následující příklad vrací `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand –
Vrátí náhodné celé číslo v rámci hello zadaný rozsah. Tato funkce negeneruje kryptograficky zabezpečené náhodných čísel.

Hello následující příklad může vrátit `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Floor
Vrátí hello největší celé číslo menší než nebo rovna toohello zadané číslo.

Hello následující příklad vrací `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Vrátí hello největší celé číslo větší než nebo rovna toohello zadané číslo.

Hello následující příklad vrací `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Datové funkce
### <a name="utcnow"></a>utcNow
Vrací řetězec ve formátu ISO 8601 hello aktuální datum a čas v místním počítači hello.

Hello následující příklad může vrátit `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>Přidat_sekundy
Přidá celočíselný počet sekund toohello zadané časové razítko.

Hello následující příklad vrací `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Přidá celočíselný počet minut toohello zadané časové razítko.

Hello následující příklad vrací `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Přidá celočíselný počet hodin toohello zadané časové razítko.

Hello následující příklad vrací `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Další kroky
* TooAzure Úvod Resource Manager, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

