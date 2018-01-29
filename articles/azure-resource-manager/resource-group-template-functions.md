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
ms.date: 09/18/2017
ms.author: tomfitz
ms.openlocfilehash: 725f12a6b5dcf4b66109512336e8a617013c5974
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-manager-template-functions"></a>Funkce šablony Azure Resource Manager
Toto téma popisuje všechny funkce, které můžete použít v šablonu Azure Resource Manager.

Přidání funkce v šablonách uzavřené v závorkách: `[` a `]`, v uvedeném pořadí. Výraz se vyhodnotí během nasazování. Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu jiného typu formátu JSON, jako je například pole, objektu nebo celé číslo. Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`. Vlastnosti odkazovat pomocí operátorů dot a [index].

Výraz šablony nesmí překročit 24,576 znaků.

Šablony funkcí a jejich parametrů se velká a malá písmena. Například správce prostředků přeloží **variables('var1')** a **VARIABLES('VAR1')** jako stejné. Při hodnocení, pokud funkci výslovně upraví případ (například toUpper nebo toLower), funkce zachovává malá a velká. Některé typy prostředků může mít případu požadavky bez ohledu na to, jak se vyhodnocují funkce.

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

## <a name="array-and-object-functions"></a>Funkce pole a objektu
Resource Manager poskytuje několik funkce pro práci s pole a objekty.

* [pole](resource-group-template-functions-array.md#array)
* [sloučení](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [obsahuje](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [prázdný](resource-group-template-functions-array.md#empty)
* [první](resource-group-template-functions-array.md#first)
* [průnik](resource-group-template-functions-array.md#intersection)
* [JSON](resource-group-template-functions-array.md#json)
* [poslední](resource-group-template-functions-array.md#last)
* [Délka](resource-group-template-functions-array.md#length)
* [min.](resource-group-template-functions-array.md#min)
* [maximální počet](resource-group-template-functions-array.md#max)
* [rozsah](resource-group-template-functions-array.md#range)
* [Přeskočit](resource-group-template-functions-array.md#skip)
* [proveďte](resource-group-template-functions-array.md#take)
* [sjednocení](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Porovnání funkcí
Resource Manager poskytuje několik funkcí pro porovnání ve vašich šablon.

* [rovná se](resource-group-template-functions-comparison.md#equals)
* [menší](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [větší](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Hodnota funkce nasazení
Resource Manager poskytuje následující funkce pro získání hodnoty z části šablony a hodnoty týkající se nasazení:

* [nasazení](resource-group-template-functions-deployment.md#deployment)
* [Parametry](resource-group-template-functions-deployment.md#parameters)
* [proměnné](resource-group-template-functions-deployment.md#variables)

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

## <a name="logical-functions"></a>Logické funkce
Resource Manager poskytuje následující funkce pro práci s logických podmínek:

* [a](resource-group-template-functions-logical.md#and)
* [BOOL](resource-group-template-functions-logical.md#bool)
* [Pokud](resource-group-template-functions-logical.md#if)
* [není](resource-group-template-functions-logical.md#not)
* [nebo](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a>Numerické funkce
Resource Manager poskytuje následující funkce pro práci s celými čísly:

* [Přidat](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [plovoucí desetinná čárka](resource-group-template-functions-numeric.md#float)
* [celá čísla](resource-group-template-functions-numeric.md#int)
* [min.](resource-group-template-functions-numeric.md#min)
* [maximální počet](resource-group-template-functions-numeric.md#max)
* [MOD](resource-group-template-functions-numeric.md#mod)
* [mul](resource-group-template-functions-numeric.md#mul)
* [Sub –](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Funkce prostředků
Resource Manager poskytuje následující funkce pro získání hodnoty prostředku:

* [listKeys a seznamu {Value}](resource-group-template-functions-resource.md#listkeys)
* [Zprostředkovatelé](resource-group-template-functions-resource.md#providers)
* [referenční dokumentace](resource-group-template-functions-resource.md#reference)
* [Skupina prostředků](resource-group-template-functions-resource.md#resourcegroup)
* [ID prostředku](resource-group-template-functions-resource.md#resourceid)
* [předplatné](resource-group-template-functions-resource.md#subscription)

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

## <a name="string-functions"></a>Řetězcové funkce
Resource Manager poskytuje následující funkce pro práci s řetězce:

* [formátu Base64.](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [obsahuje](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [prázdný](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [první](resource-group-template-functions-string.md#first)
* [identifikátor GUID](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [poslední](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [Délka](resource-group-template-functions-string.md#length)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [nahradit](resource-group-template-functions-string.md#replace)
* [Přeskočit](resource-group-template-functions-string.md#skip)
* [split](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [řetězec](resource-group-template-functions-string.md#string)
* [dílčí řetězec](resource-group-template-functions-string.md#substring)
* [proveďte](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [uvolnění dočasné paměti](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [identifikátor URI](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

## <a name="next-steps"></a>Další kroky
* Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md)
* Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md)
* K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)
* Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md)
