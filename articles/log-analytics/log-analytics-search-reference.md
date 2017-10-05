---
title: "Azure Log Analytics hledání odkaz | Microsoft Docs"
description: "Analýzy protokolů hledání odkaz popisuje jazyk vyhledávání a poskytuje syntaxe dotazu Obecné možnosti můžete můžete použít při vyhledávání pro data a filtrování výrazy a zúžit vyhledávání."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc9c9b0a6292dab256997a86a6db16367fc48cd3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-search-reference"></a><span data-ttu-id="344f8-103">Referenční dokumentace vyhledávání analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="344f8-103">Log Analytics search reference</span></span>

>[!NOTE]
> <span data-ttu-id="344f8-104">Tento článek popisuje protokolu vyhledávání v aktuální jazyk dotazu analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="344f8-104">This article describes log searches using the current query language in Log Analytics.</span></span>  <span data-ttu-id="344f8-105">Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak se seznamte s [referenční příručka jazyka pro nový jazyk](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="344f8-105">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer to [the language reference for the new language](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>

<span data-ttu-id="344f8-106">V následujícím referenčním oddílu o vyhledávání jazyk popisuje možnosti syntaxe obecné dotazu, můžete použít při vyhledávání pro data a filtrování výrazy a zúžit vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="344f8-106">The following reference section about search language describes the general query syntax options you can use when searching for data and filtering expressions to help narrow your search.</span></span> <span data-ttu-id="344f8-107">Popisuje také příkazy, které můžete provést akci pro data načtená.</span><span class="sxs-lookup"><span data-stu-id="344f8-107">It also describes commands that you can use to take action on the data retrieved.</span></span>

<span data-ttu-id="344f8-108">Další informace o pole, vrátí se v hledání a omezující vlastnosti, které vám pomohou zjistit informace o podobné kategorie dat, v [pole hledání a omezující vlastnost odkazovat části](#search-field-and-facet-reference).</span><span class="sxs-lookup"><span data-stu-id="344f8-108">You can read about the fields returned in searches, and the facets that help you discover more about similar categories of data, in the [Search field and facet reference section](#search-field-and-facet-reference).</span></span>

## <a name="general-query-syntax"></a><span data-ttu-id="344f8-109">Syntaxe dotazu obecné</span><span class="sxs-lookup"><span data-stu-id="344f8-109">General query syntax</span></span>
<span data-ttu-id="344f8-110">Syntaxe pro obecné dotazování vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="344f8-110">The syntax for general querying is as follows:</span></span>

```
filterExpression | command1 | command2 …
```

<span data-ttu-id="344f8-111">Výraz filtru (`filterExpression`) definuje "kde" podmínky pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="344f8-111">The filter expression (`filterExpression`) defines the "where" condition for the query.</span></span> <span data-ttu-id="344f8-112">Příkazy použít do výsledků vrácených dotazem.</span><span class="sxs-lookup"><span data-stu-id="344f8-112">The commands apply to the results returned by the query.</span></span> <span data-ttu-id="344f8-113">Více příkazů musí být odděleny znakem (|).</span><span class="sxs-lookup"><span data-stu-id="344f8-113">Multiple commands must be separated by the pipe character ( | ).</span></span>

### <a name="general-syntax-examples"></a><span data-ttu-id="344f8-114">Příklady syntaxe obecné</span><span class="sxs-lookup"><span data-stu-id="344f8-114">General syntax examples</span></span>
<span data-ttu-id="344f8-115">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-115">Examples:</span></span>

```
system
```

<span data-ttu-id="344f8-116">Tento dotaz vrací výsledky, které obsahují slovo *systému* v každé pole, které pro fulltextové indexování nebo podmínek vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="344f8-116">This query returns results that contain the word *system* in any field that has been indexed for full text or terms searching.</span></span>

> [!NOTE]
> <span data-ttu-id="344f8-117">Ne všechna pole jsou indexované tímto způsobem, ale nejběžnější textové pole (například názvy a popisy) obvykle jsou.</span><span class="sxs-lookup"><span data-stu-id="344f8-117">Not all fields are indexed this way, but the most common textual fields (such as descriptions and names) usually are.</span></span>
>
>

```
system error
```

<span data-ttu-id="344f8-118">Tento dotaz vrací výsledky, které obsahují slova *systému* a *chyba*.</span><span class="sxs-lookup"><span data-stu-id="344f8-118">This query returns results that contain the words *system* and *error*.</span></span>

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

<span data-ttu-id="344f8-119">Tento dotaz vrací výsledky, které obsahují slova *systému* a *chyba*.</span><span class="sxs-lookup"><span data-stu-id="344f8-119">This query returns results that contain the words *system* and *error*.</span></span> <span data-ttu-id="344f8-120">Potom setřídí výsledky podle *ManagementGroupName* pole (ve vzestupném pořadí) a potom *TimeGenerated* pole (v sestupném pořadí).</span><span class="sxs-lookup"><span data-stu-id="344f8-120">It then sorts the results by the *ManagementGroupName* field (in ascending order), and then by the *TimeGenerated* field (in descending order).</span></span> <span data-ttu-id="344f8-121">Jak dlouho trvá pouze prvních 10 výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-121">It takes only the first 10 results.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="344f8-122">Názvy všech polí a hodnoty pro pole řetězce a text se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="344f8-122">All the field names and the values for the string and text fields are case sensitive.</span></span>
>
>

## <a name="filter-expressions"></a><span data-ttu-id="344f8-123">Výrazy filtru</span><span class="sxs-lookup"><span data-stu-id="344f8-123">Filter expressions</span></span>
<span data-ttu-id="344f8-124">Následující podčásti vysvětlují výrazech filtru.</span><span class="sxs-lookup"><span data-stu-id="344f8-124">The following subsections explain the filter expressions.</span></span>

### <a name="string-literals"></a><span data-ttu-id="344f8-125">Textové literály</span><span class="sxs-lookup"><span data-stu-id="344f8-125">String literals</span></span>
<span data-ttu-id="344f8-126">Řetězcový literál je řetězec, který nelze rozpoznat analyzátorem jako klíčové slovo nebo předdefinované datový typ (například číslo nebo datum).</span><span class="sxs-lookup"><span data-stu-id="344f8-126">A string literal is any string that is not recognized by the parser as a keyword or a predefined data type (for example, a number or date).</span></span>

<span data-ttu-id="344f8-127">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-127">Examples:</span></span>

```
These all are string literals
```

<span data-ttu-id="344f8-128">Tento dotaz vyhledává výsledky, které obsahují výskyty všechna pět slova.</span><span class="sxs-lookup"><span data-stu-id="344f8-128">This query searches for results that contain occurrences of all five words.</span></span> <span data-ttu-id="344f8-129">Pokud chcete provést vyhledávání složitých řetězec, uzavřete řetězcový literál v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="344f8-129">To perform a complex string search, enclose the string literal in quotation marks.</span></span> <span data-ttu-id="344f8-130">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-130">For example:</span></span>

```
"Windows Server"
```

<span data-ttu-id="344f8-131">Tento příkaz vrátí jenom výsledky s přesné shody pro *systému Windows Server*.</span><span class="sxs-lookup"><span data-stu-id="344f8-131">This only returns results with exact matches for *Windows Server*.</span></span>

### <a name="numbers"></a><span data-ttu-id="344f8-132">Čísla</span><span class="sxs-lookup"><span data-stu-id="344f8-132">Numbers</span></span>
<span data-ttu-id="344f8-133">Analyzátor podporuje desítkové celé číslo a číslo s plovoucí desetinnou čárkou syntaxe pro číselné pole.</span><span class="sxs-lookup"><span data-stu-id="344f8-133">The parser supports the decimal integer and floating-point number syntax for numerical fields.</span></span>

<span data-ttu-id="344f8-134">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-134">Examples:</span></span>

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a><span data-ttu-id="344f8-135">Data a časy</span><span class="sxs-lookup"><span data-stu-id="344f8-135">Dates and times</span></span>
<span data-ttu-id="344f8-136">Má každá část data v systému *TimeGenerated* vlastnosti, která představuje původní datum a čas záznamu.</span><span class="sxs-lookup"><span data-stu-id="344f8-136">Every piece of data in the system has a *TimeGenerated* property, which represents the original date and time of the record.</span></span> <span data-ttu-id="344f8-137">Některé typy dat může mít další datum a čas pole (například *změněno*).</span><span class="sxs-lookup"><span data-stu-id="344f8-137">Some types of data can have additional date and time fields (for example, *LastModified*).</span></span>

<span data-ttu-id="344f8-138">Časovou osu **graf a času** selektor v Azure Log Analytics ukazuje distribuční výsledků v čase (podle aktuální dotaz spuštěn).</span><span class="sxs-lookup"><span data-stu-id="344f8-138">The timeline **Chart/Time** selector in Azure Log Analytics shows a distribution of results over time (according to the current query being run).</span></span> <span data-ttu-id="344f8-139">To je založené na *TimeGenerated* pole.</span><span class="sxs-lookup"><span data-stu-id="344f8-139">This is based on the *TimeGenerated* field.</span></span> <span data-ttu-id="344f8-140">Datum a čas mají konkrétní řetězec formátu, který lze použít v dotazech dotaz omezit na určitý časový rámec.</span><span class="sxs-lookup"><span data-stu-id="344f8-140">Date and time fields have a specific string format that can be used in queries to restrict the query to a particular timeframe.</span></span> <span data-ttu-id="344f8-141">Syntaxe můžete taky odkazovat na relativní časové intervaly (například "mezi před 3 dny a 2 hodinami").</span><span class="sxs-lookup"><span data-stu-id="344f8-141">You can also use syntax to refer to relative time intervals (for example, "between 3 days ago and 2 hours ago").</span></span>

<span data-ttu-id="344f8-142">Tady jsou platné formuláře syntaxe kalendářních dat a časů:</span><span class="sxs-lookup"><span data-stu-id="344f8-142">The following are valid forms of syntax for dates and times:</span></span>

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


<span data-ttu-id="344f8-143">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-143">For example:</span></span>

```
TimeGenerated:2013-10-01T12:20
```

<span data-ttu-id="344f8-144">Předchozí příkaz vrátí jenom záznamy se *TimeGenerated* hodnota přesně 12:20 na 1. října 2013.</span><span class="sxs-lookup"><span data-stu-id="344f8-144">The previous command returns only records with a *TimeGenerated* value of exactly 12:20 on October 1, 2013.</span></span>

<span data-ttu-id="344f8-145">Analyzátor také nyní podporuje klávesovými hodnotě Datum a čas.</span><span class="sxs-lookup"><span data-stu-id="344f8-145">The parser also supports the mnemonic Date/Time value, NOW.</span></span> <span data-ttu-id="344f8-146">(Nepravděpodobné, že to předá výsledky, protože data doesn't make prostřednictvím systému to rychlé.)</span><span class="sxs-lookup"><span data-stu-id="344f8-146">(It is unlikely that this will yield any results, because data doesn’t make it through the system that fast.)</span></span>

<span data-ttu-id="344f8-147">Tyto příklady jsou stavební bloky, které chcete použít pro relativní a absolutní datum.</span><span class="sxs-lookup"><span data-stu-id="344f8-147">These examples are building blocks to use for relative and absolute dates.</span></span> <span data-ttu-id="344f8-148">V následujících třech témata zobrazí se jejich použití v rozšířené filtry s příklady, které používají rozsahy relativní datum.</span><span class="sxs-lookup"><span data-stu-id="344f8-148">In the next three subsections, you'll see how to use them in more advanced filters, with examples that use relative date ranges.</span></span>

### <a name="datetime-math"></a><span data-ttu-id="344f8-149">Matematické datum a čas</span><span class="sxs-lookup"><span data-stu-id="344f8-149">Date/Time math</span></span>
<span data-ttu-id="344f8-150">Pomocí data a času matematické operátory posunutí nebo zaokrouhlit hodnotě Datum a čas, a to pomocí jednoduché výpočty datum a čas.</span><span class="sxs-lookup"><span data-stu-id="344f8-150">Use the Date/Time math operators to offset or round the Date/Time value, by using simple Date/Time calculations.</span></span>

<span data-ttu-id="344f8-151">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-151">Syntax:</span></span>

```
datetime/unit
```

```
datetime[+|-]count unit
```

| <span data-ttu-id="344f8-152">Operátor</span><span class="sxs-lookup"><span data-stu-id="344f8-152">Operator</span></span> | <span data-ttu-id="344f8-153">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-153">Description</span></span> |
| --- | --- |
| / |<span data-ttu-id="344f8-154">Zaokrouhlí na jednotku, zadané datum a čas.</span><span class="sxs-lookup"><span data-stu-id="344f8-154">Rounds Date/Time to the specified unit.</span></span> <span data-ttu-id="344f8-155">Například nyní / den zaokrouhlí aktuální datum a čas na půlnoc aktuálního dne.</span><span class="sxs-lookup"><span data-stu-id="344f8-155">For example, NOW/DAY rounds the current Date/Time to midnight of the current day.</span></span> |
| <span data-ttu-id="344f8-156">+ nebo -</span><span class="sxs-lookup"><span data-stu-id="344f8-156">+ or -</span></span> |<span data-ttu-id="344f8-157">Posune datum a čas zadaný počet jednotek.</span><span class="sxs-lookup"><span data-stu-id="344f8-157">Offsets Date/Time by the specified number of units.</span></span> <span data-ttu-id="344f8-158">Například nyní + 1 hodina posune aktuální datum a čas o jednu hodinu dopředu.</span><span class="sxs-lookup"><span data-stu-id="344f8-158">For example, NOW+1HOUR offsets the current Date/Time by one hour ahead.</span></span> <span data-ttu-id="344f8-159">2013-10-01T12:00-10 dní posune hodnoty Date zpět o 10 dní.</span><span class="sxs-lookup"><span data-stu-id="344f8-159">2013-10-01T12:00-10DAYS offsets the Date value back by 10 days.</span></span> |

<span data-ttu-id="344f8-160">Matematické operátory datum a čas můžete zřetězené společně.</span><span class="sxs-lookup"><span data-stu-id="344f8-160">You can chain the Date/Time math operators together.</span></span> <span data-ttu-id="344f8-161">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-161">For example:</span></span>

```
NOW+1HOUR-10MONTHS/MINUTE
```

<span data-ttu-id="344f8-162">Následující tabulka uvádí podporované jednotky datum a čas.</span><span class="sxs-lookup"><span data-stu-id="344f8-162">The following table lists the supported Date/Time units.</span></span>

| <span data-ttu-id="344f8-163">Datum a čas jednotky</span><span class="sxs-lookup"><span data-stu-id="344f8-163">Date/Time unit</span></span> | <span data-ttu-id="344f8-164">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-164">Description</span></span> |
| --- | --- |
| <span data-ttu-id="344f8-165">ROK, LET.</span><span class="sxs-lookup"><span data-stu-id="344f8-165">YEAR, YEARS</span></span> |<span data-ttu-id="344f8-166">Zaokrouhlí do aktuálního roku a posune o zadaný počet roků.</span><span class="sxs-lookup"><span data-stu-id="344f8-166">Rounds to current year, or offsets by the specified number of years.</span></span> |
| <span data-ttu-id="344f8-167">MĚSÍC, MĚSÍCŮ</span><span class="sxs-lookup"><span data-stu-id="344f8-167">MONTH, MONTHS</span></span> |<span data-ttu-id="344f8-168">Zaokrouhlí do aktuálního měsíce nebo posune o zadaném počtu měsíců.</span><span class="sxs-lookup"><span data-stu-id="344f8-168">Rounds to current month, or offsets by the specified number of months.</span></span> |
| <span data-ttu-id="344f8-169">DEN, DNŮ, DATUM</span><span class="sxs-lookup"><span data-stu-id="344f8-169">DAY, DAYS, DATE</span></span> |<span data-ttu-id="344f8-170">Zaokrouhlí na aktuální den v měsíci, nebo posune o zadaný počet dnů.</span><span class="sxs-lookup"><span data-stu-id="344f8-170">Rounds to current day of the month, or offsets by the specified number of days.</span></span> |
| <span data-ttu-id="344f8-171">HODINA, ČAS</span><span class="sxs-lookup"><span data-stu-id="344f8-171">HOUR, HOURS</span></span> |<span data-ttu-id="344f8-172">Zaokrouhlí číslo na aktuální hodinu nebo posuny podle zadaného počtu hodin.</span><span class="sxs-lookup"><span data-stu-id="344f8-172">Rounds to current hour, or offsets by the specified number of hours.</span></span> |
| <span data-ttu-id="344f8-173">MINUTA, MINUT</span><span class="sxs-lookup"><span data-stu-id="344f8-173">MINUTE, MINUTES</span></span> |<span data-ttu-id="344f8-174">Zaokrouhlí do aktuální minuty nebo posune o zadaný počet minut.</span><span class="sxs-lookup"><span data-stu-id="344f8-174">Rounds to current minute, or offsets by the specified number of minutes.</span></span> |
| <span data-ttu-id="344f8-175">SECOND, SECONDS.</span><span class="sxs-lookup"><span data-stu-id="344f8-175">SECOND, SECONDS</span></span> |<span data-ttu-id="344f8-176">Druhý zaokrouhlí na aktuální nebo posune o zadaný počet sekund.</span><span class="sxs-lookup"><span data-stu-id="344f8-176">Rounds to current second, or offsets by the specified number of seconds.</span></span> |
| <span data-ttu-id="344f8-177">MILISEKUND, POČET MILISEKUND, MILLI, MILLIS</span><span class="sxs-lookup"><span data-stu-id="344f8-177">MILLISECOND, MILLISECONDS, MILLI, MILLIS</span></span> |<span data-ttu-id="344f8-178">Zaokrouhlí na aktuální milisekundu nebo posune o zadaný počet milisekund, po.</span><span class="sxs-lookup"><span data-stu-id="344f8-178">Rounds to current millisecond, or offsets by the specified number of milliseconds.</span></span> |

### <a name="field-facets"></a><span data-ttu-id="344f8-179">Omezující vlastnosti pole</span><span class="sxs-lookup"><span data-stu-id="344f8-179">Field facets</span></span>
<span data-ttu-id="344f8-180">Pomocí pole omezující vlastnosti můžete zadejte podmínku vyhledávání konkrétních polí a jejich přesné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="344f8-180">By using field facets, you can specify the search condition for specific fields and their exact values.</span></span> <span data-ttu-id="344f8-181">To se liší od zápis "bez textu" dotazů pro různé podmínky v celém indexu.</span><span class="sxs-lookup"><span data-stu-id="344f8-181">This differs from writing "free text" queries for various terms throughout the index.</span></span> <span data-ttu-id="344f8-182">Jste viděli již tato technika v několika předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="344f8-182">You have already seen this technique in several previous examples.</span></span> <span data-ttu-id="344f8-183">Následují příklady složitější.</span><span class="sxs-lookup"><span data-stu-id="344f8-183">The following are more complex examples.</span></span>

<span data-ttu-id="344f8-184">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="344f8-184">**Syntax**</span></span>

```
field:value
```

```
field=value
```

<span data-ttu-id="344f8-185">**Popis**</span><span class="sxs-lookup"><span data-stu-id="344f8-185">**Description**</span></span>

<span data-ttu-id="344f8-186">Vyhledá pole pro tuto konkrétní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="344f8-186">Searches the field for the specific value.</span></span> <span data-ttu-id="344f8-187">Hodnota může být řetězcový literál, číslo nebo datum a čas.</span><span class="sxs-lookup"><span data-stu-id="344f8-187">The value can be a string literal, number, or date and time.</span></span>

<span data-ttu-id="344f8-188">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-188">For example:</span></span>

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

<span data-ttu-id="344f8-189">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="344f8-189">**Syntax**</span></span>

<span data-ttu-id="344f8-190">*pole > hodnota*</span><span class="sxs-lookup"><span data-stu-id="344f8-190">*field>value*</span></span>

<span data-ttu-id="344f8-191">*pole < hodnota*</span><span class="sxs-lookup"><span data-stu-id="344f8-191">*field<value*</span></span>

<span data-ttu-id="344f8-192">*pole > = hodnota*</span><span class="sxs-lookup"><span data-stu-id="344f8-192">*field>=value*</span></span>

<span data-ttu-id="344f8-193">*pole < = hodnota*</span><span class="sxs-lookup"><span data-stu-id="344f8-193">*field<=value*</span></span>

<span data-ttu-id="344f8-194">*pole! = hodnota*</span><span class="sxs-lookup"><span data-stu-id="344f8-194">*field!=value*</span></span>

<span data-ttu-id="344f8-195">**Popis**</span><span class="sxs-lookup"><span data-stu-id="344f8-195">**Description**</span></span>

<span data-ttu-id="344f8-196">Poskytuje porovnání.</span><span class="sxs-lookup"><span data-stu-id="344f8-196">Provides comparisons.</span></span>

<span data-ttu-id="344f8-197">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-197">For example:</span></span>

```
TimeGenerated>NOW+2HOURS
```

<span data-ttu-id="344f8-198">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="344f8-198">**Syntax**</span></span>

```
field:[from..to]
```

<span data-ttu-id="344f8-199">**Popis**</span><span class="sxs-lookup"><span data-stu-id="344f8-199">**Description**</span></span>

<span data-ttu-id="344f8-200">Poskytuje rozsah používání omezujících vlastností.</span><span class="sxs-lookup"><span data-stu-id="344f8-200">Provides range faceting.</span></span>

<span data-ttu-id="344f8-201">Například:</span><span class="sxs-lookup"><span data-stu-id="344f8-201">For example:</span></span>

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a><span data-ttu-id="344f8-202">V</span><span class="sxs-lookup"><span data-stu-id="344f8-202">IN</span></span>
<span data-ttu-id="344f8-203">**IN** – klíčové slovo umožňuje vybrat ze seznamu hodnot.</span><span class="sxs-lookup"><span data-stu-id="344f8-203">The **IN** keyword allows you to select from a list of values.</span></span> <span data-ttu-id="344f8-204">V závislosti na syntaxi, kterou používáte může se jednat jednoduchý seznam hodnot, které poskytnete, nebo seznam hodnot z agregace.</span><span class="sxs-lookup"><span data-stu-id="344f8-204">Depending on the syntax you use, this can be a simple list of values you provide, or a list of values from an aggregation.</span></span>

<span data-ttu-id="344f8-205">Syntaxe 1:</span><span class="sxs-lookup"><span data-stu-id="344f8-205">Syntax 1:</span></span>

```
field IN {value1,value2,value3,...}
```

<span data-ttu-id="344f8-206">Tuto syntaxi umožňuje zahrnout všechny hodnoty v jednoduchých seznamů.</span><span class="sxs-lookup"><span data-stu-id="344f8-206">This syntax allows you to include all values in a simple list.</span></span>



<span data-ttu-id="344f8-207">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-207">Examples:</span></span>

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

<span data-ttu-id="344f8-208">Syntaxe 2:</span><span class="sxs-lookup"><span data-stu-id="344f8-208">Syntax 2:</span></span>

```
(Outer query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

<span data-ttu-id="344f8-209">Tuto syntaxi vám umožní vytvořit agregace.</span><span class="sxs-lookup"><span data-stu-id="344f8-209">This syntax allows you to create an aggregation.</span></span> <span data-ttu-id="344f8-210">Seznam hodnot můžete informačního kanálu pak z tohoto agregace do jiné vnější search (primární), která vypadá pro události se tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="344f8-210">You can then feed the list of values from that aggregation into another outer (primary) search that looks for events with those value.</span></span> <span data-ttu-id="344f8-211">To uděláte tak, že uzavření vnitřní hledání do složených závorek a napájení své výsledky jako možných hodnot pro pole ve vnější vyhledávání pomocí operátor.</span><span class="sxs-lookup"><span data-stu-id="344f8-211">You do this by enclosing the inner search in braces, and feeding its results as possible values for a field in the outer search by using the IN operator.</span></span>

<span data-ttu-id="344f8-212">Vnitřní dotaz příklad: *počítače aktuálně chybějící aktualizace zabezpečení* s následující dotaz agregace:</span><span class="sxs-lookup"><span data-stu-id="344f8-212">Inner query example: *computers currently missing security updates* with the following aggregation query:</span></span>

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

<span data-ttu-id="344f8-213">Poslední dotaz, který vyhledá *všechny události systému Windows pro počítače, které jsou aktuálně chybějící aktualizace zabezpečení* vypadá zhruba takto:</span><span class="sxs-lookup"><span data-stu-id="344f8-213">The final query that finds *all Windows events for computers currently missing security updates* resembles the following:</span></span>

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a><span data-ttu-id="344f8-214">Contains</span><span class="sxs-lookup"><span data-stu-id="344f8-214">Contains</span></span>
<span data-ttu-id="344f8-215">**Obsahuje** – klíčové slovo vám umožní filtrovat pro záznamy s polem, které obsahuje zadaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="344f8-215">The **Contains** keyword allows you to filter for records with a field that contains a specified string.</span></span> <span data-ttu-id="344f8-216">To je malá a velká písmena, lze použít pouze u polí s řetězcem a nesmí obsahovat žádné řídicí znaky.</span><span class="sxs-lookup"><span data-stu-id="344f8-216">This is case sensitive, works only with string fields, and may not include any escape characters.</span></span>

<span data-ttu-id="344f8-217">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-217">Syntax:</span></span>

```
field:contains("string")
```

<span data-ttu-id="344f8-218">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-218">Example:</span></span>

```
Type:contains("Event")
```

<span data-ttu-id="344f8-219">Vrátí záznamy s typem, který obsahuje řetězec "Událost".</span><span class="sxs-lookup"><span data-stu-id="344f8-219">This returns records with a type that contains the string "Event".</span></span> <span data-ttu-id="344f8-220">Mezi příklady patří **událostí**, **SecurityEvent**, a **ServiceFabricOperationEvent**.</span><span class="sxs-lookup"><span data-stu-id="344f8-220">Examples include **Event**, **SecurityEvent**, and **ServiceFabricOperationEvent**.</span></span>



### <a name="regular-expressions"></a><span data-ttu-id="344f8-221">Regulární výrazy</span><span class="sxs-lookup"><span data-stu-id="344f8-221">Regular expressions</span></span>
<span data-ttu-id="344f8-222">Můžete určit podmínku vyhledávání pro pole s regulárním výrazem, pomocí **Regex** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="344f8-222">You can specify a search condition for a field with a regular expression, by using the **Regex** keyword.</span></span> <span data-ttu-id="344f8-223">Úplný popis syntaxe můžete použít v regulárních výrazech naleznete v tématu [pomocí regulárních výrazů k filtrování protokolu hledání v analýzy protokolů](log-analytics-log-searches-regex.md).</span><span class="sxs-lookup"><span data-stu-id="344f8-223">For a complete description of the syntax you can use in regular expressions, see [Using regular expressions to filter log searches in Log Analytics](log-analytics-log-searches-regex.md).</span></span>

<span data-ttu-id="344f8-224">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-224">Syntax:</span></span>

```
field:Regex("Regular Expression")
```

<span data-ttu-id="344f8-225">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-225">Example:</span></span>

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a><span data-ttu-id="344f8-226">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="344f8-226">Logical operators</span></span>
<span data-ttu-id="344f8-227">Logické operátory podporu jazyků dotazu (*a*, *nebo*, a *není*) a jejich aliasy stylu jazyka C (*&&*,  *||* , a *!*, v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="344f8-227">The query languages support the logical operators (*AND*, *OR*, and *NOT*) and their C-style aliases (*&&*, *||*, and *!*, respectively).</span></span> <span data-ttu-id="344f8-228">Závorky můžete použít k seskupení těchto operátorů.</span><span class="sxs-lookup"><span data-stu-id="344f8-228">You can use parentheses to group these operators.</span></span>

<span data-ttu-id="344f8-229">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-229">Examples:</span></span>

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

<span data-ttu-id="344f8-230">Logický operátor argumenty filtru nejvyšší úrovně, můžete vynechat.</span><span class="sxs-lookup"><span data-stu-id="344f8-230">You can omit the logical operator for the top-level filter arguments.</span></span> <span data-ttu-id="344f8-231">V takovém případě se předpokládá, operátor a.</span><span class="sxs-lookup"><span data-stu-id="344f8-231">In this case, the AND operator is assumed.</span></span>

| <span data-ttu-id="344f8-232">Výraz filtru</span><span class="sxs-lookup"><span data-stu-id="344f8-232">Filter expression</span></span> | <span data-ttu-id="344f8-233">Ekvivalent hodnoty</span><span class="sxs-lookup"><span data-stu-id="344f8-233">Equivalent to</span></span> |
| --- | --- |
| <span data-ttu-id="344f8-234">Systémová chyba</span><span class="sxs-lookup"><span data-stu-id="344f8-234">system error</span></span> |<span data-ttu-id="344f8-235">systém a chyb</span><span class="sxs-lookup"><span data-stu-id="344f8-235">system AND error</span></span> |
| <span data-ttu-id="344f8-236">systému "Windows Server" nebo závažnosti: 1</span><span class="sxs-lookup"><span data-stu-id="344f8-236">system "Windows Server" OR Severity:1</span></span> |<span data-ttu-id="344f8-237">systém a ("Windows Server" nebo závažnosti: 1)</span><span class="sxs-lookup"><span data-stu-id="344f8-237">system AND ("Windows Server" OR Severity:1)</span></span> |

### <a name="wildcarding"></a><span data-ttu-id="344f8-238">Použití zástupných znaků</span><span class="sxs-lookup"><span data-stu-id="344f8-238">Wildcarding</span></span>
<span data-ttu-id="344f8-239">Dotazovací jazyk podporuje používání ( \* ) znaku, který představuje jeden nebo více znaků pro hodnotu v dotazu.</span><span class="sxs-lookup"><span data-stu-id="344f8-239">The query language supports using the ( \* ) character to  represent one or more characters for a value in a query.</span></span>

<span data-ttu-id="344f8-240">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-240">Example:</span></span>

 <span data-ttu-id="344f8-241">Vyhledáte všechny počítače s "SQL" v názvu, jako je například "Redmond-SQL".</span><span class="sxs-lookup"><span data-stu-id="344f8-241">Find all computers with "SQL" in the name, such as "Redmond-SQL".</span></span>

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> <span data-ttu-id="344f8-242">V tuto chvíli nelze použít zástupné znaky v rámci uvozovky.</span><span class="sxs-lookup"><span data-stu-id="344f8-242">At this time, wildcards cannot be used within quotations.</span></span> <span data-ttu-id="344f8-243">Například zpráva `"*This text*"` zvažuje (\*) použít jako literál (\*) znaků.</span><span class="sxs-lookup"><span data-stu-id="344f8-243">For example, the message `"*This text*"` considers the (\*) used as a literal (\*) character.</span></span>


## <a name="commands"></a><span data-ttu-id="344f8-244">Příkazy</span><span class="sxs-lookup"><span data-stu-id="344f8-244">Commands</span></span>


<span data-ttu-id="344f8-245">Příkazy se vztahuje na výsledky, které jsou v dotazu.</span><span class="sxs-lookup"><span data-stu-id="344f8-245">The commands apply to the results that are returned by the query.</span></span> <span data-ttu-id="344f8-246">Pomocí znaku svislá čára (|) použít příkaz načtené výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-246">Use the pipe character ( | ) to apply a command to the retrieved results.</span></span> <span data-ttu-id="344f8-247">Více příkazů musí být odděleny znakem.</span><span class="sxs-lookup"><span data-stu-id="344f8-247">Multiple commands must be separated by the pipe character.</span></span>

> [!NOTE]
> <span data-ttu-id="344f8-248">Názvy příkazů může být napsán v velká nebo malá písmena, na rozdíl od názvy polí a data.</span><span class="sxs-lookup"><span data-stu-id="344f8-248">Command names can be written in upper case or lower case, unlike the field names and the data.</span></span>
>
>

### <a name="sort"></a><span data-ttu-id="344f8-249">Seřadit</span><span class="sxs-lookup"><span data-stu-id="344f8-249">Sort</span></span>
<span data-ttu-id="344f8-250">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-250">Syntax:</span></span>

    sort field1 asc|desc, field2 asc|desc, …

<span data-ttu-id="344f8-251">Řazení výsledků podle určitého pole.</span><span class="sxs-lookup"><span data-stu-id="344f8-251">Sorts the results by particular fields.</span></span> <span data-ttu-id="344f8-252">Přípona asc nebo desc seřadit výsledky ve vzestupném nebo sestupném pořadí je volitelný.</span><span class="sxs-lookup"><span data-stu-id="344f8-252">The asc/desc suffix to sort the results in ascending or descending order is optional.</span></span> <span data-ttu-id="344f8-253">Je-li vynechán, *asc* se předpokládá, že pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="344f8-253">If it is omitted, the *asc* sort order is assumed.</span></span> <span data-ttu-id="344f8-254">Pro **TimeGenerated** pole, *desc* pořadí řazení se předpokládá, tak, aby vracel nejnovější výsledky nejprve ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="344f8-254">For the **TimeGenerated** field, *desc* sort order is assumed, so it returns the most recent results first by default.</span></span>

### <a name="toplimit"></a><span data-ttu-id="344f8-255">Horní/Limit</span><span class="sxs-lookup"><span data-stu-id="344f8-255">Top/Limit</span></span>
<span data-ttu-id="344f8-256">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-256">Syntax:</span></span>

    top number


    limit number
<span data-ttu-id="344f8-257">Omezuje odpověď na hlavních výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-257">Limits the response to the top N results.</span></span>

<span data-ttu-id="344f8-258">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-258">Example:</span></span>

    Type:Alert errors detected | top 10

<span data-ttu-id="344f8-259">Vrátí top 10 odpovídající výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-259">Returns the top 10 matching results.</span></span>

### <a name="skip"></a><span data-ttu-id="344f8-260">Přeskočit</span><span class="sxs-lookup"><span data-stu-id="344f8-260">Skip</span></span>
<span data-ttu-id="344f8-261">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-261">Syntax:</span></span>

    skip number

<span data-ttu-id="344f8-262">Přeskočí počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="344f8-262">Skips the number of results listed.</span></span>

<span data-ttu-id="344f8-263">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-263">Example:</span></span>

    Type:Alert errors detected | top 10 | skip 200

<span data-ttu-id="344f8-264">Vrátí top 10 odpovídající výsledky začínající na výsledek 200.</span><span class="sxs-lookup"><span data-stu-id="344f8-264">Returns top 10 matching results starting at result 200.</span></span>

### <a name="select"></a><span data-ttu-id="344f8-265">Vyberte</span><span class="sxs-lookup"><span data-stu-id="344f8-265">Select</span></span>
<span data-ttu-id="344f8-266">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-266">Syntax:</span></span>

    select field1, field2, ...

<span data-ttu-id="344f8-267">Omezí výsledky na pole, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="344f8-267">Limits results to the fields you choose.</span></span>

<span data-ttu-id="344f8-268">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-268">Example:</span></span>

    Type:Alert errors detected | select Name, Severity

<span data-ttu-id="344f8-269">Omezení vrácených výsledků pole ke *název* a *závažnost*.</span><span class="sxs-lookup"><span data-stu-id="344f8-269">Limits the returned results fields to *Name* and *Severity*.</span></span>

### <a name="measure"></a><span data-ttu-id="344f8-270">Míra</span><span class="sxs-lookup"><span data-stu-id="344f8-270">Measure</span></span>
<span data-ttu-id="344f8-271">*Měr* příkaz se používá k aplikování statistických funkcí k nezpracované výsledků.</span><span class="sxs-lookup"><span data-stu-id="344f8-271">The *measure* command is used to apply statistical functions to the raw search results.</span></span> <span data-ttu-id="344f8-272">To je velmi užitečné k získání *Seskupit podle* zobrazení nad daty.</span><span class="sxs-lookup"><span data-stu-id="344f8-272">This is very useful to get *group-by* views over the data.</span></span> <span data-ttu-id="344f8-273">Pokud použijete příkaz měr, analýzy protokolů hledání zobrazí tabulku s agregované výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-273">When you use the measure command, Log Analytics search displays a table with aggregated results.</span></span>

<span data-ttu-id="344f8-274">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="344f8-274">**Syntax:**</span></span>

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



<span data-ttu-id="344f8-275">Agreguje výsledky podle *skupinové pole*a vypočítá hodnoty agregované měr pomocí *aggregatedField*.</span><span class="sxs-lookup"><span data-stu-id="344f8-275">Aggregates the results by *groupField*, and calculates the aggregated measure values by using *aggregatedField*.</span></span>

| <span data-ttu-id="344f8-276">Míra statistické funkce</span><span class="sxs-lookup"><span data-stu-id="344f8-276">Measure statistical function</span></span> | <span data-ttu-id="344f8-277">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-277">Description</span></span> |
| --- | --- |
| <span data-ttu-id="344f8-278">*Vlastnost aggregateFunction*</span><span class="sxs-lookup"><span data-stu-id="344f8-278">*aggregateFunction*</span></span> |<span data-ttu-id="344f8-279">Název agregační funkci (rozlišování malých a velkých písmen).</span><span class="sxs-lookup"><span data-stu-id="344f8-279">The name of the aggregate function (case insensitive).</span></span> <span data-ttu-id="344f8-280">Jsou podporovány následující agregační funkce: počet, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99).</span><span class="sxs-lookup"><span data-stu-id="344f8-280">The following aggregate functions are supported: COUNT, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, PERCENTILE##, or PCT## (## is any number between 1 and 99).</span></span> |
| <span data-ttu-id="344f8-281">*aggregatedField*</span><span class="sxs-lookup"><span data-stu-id="344f8-281">*aggregatedField*</span></span> |<span data-ttu-id="344f8-282">Pole, které je agregaci.</span><span class="sxs-lookup"><span data-stu-id="344f8-282">The field that is being aggregated.</span></span> <span data-ttu-id="344f8-283">Toto pole je volitelné pro agregační funkce COUNT, ale musí být existující číselné pole pro SUM, MAX, MIN, AVG, STDDEV, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99).</span><span class="sxs-lookup"><span data-stu-id="344f8-283">This field is optional for the COUNT aggregate function, but has to be an existing numeric field for SUM, MAX, MIN, AVG, STDDEV, PERCENTILE##, or PCT## (## is any number between 1 and 99).</span></span> <span data-ttu-id="344f8-284">Některé z může být také aggregatedField **rozšíření** podporované funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-284">The aggregatedField can also be any of the **Extend** supported functions.</span></span> |
| <span data-ttu-id="344f8-285">*fieldAlias*</span><span class="sxs-lookup"><span data-stu-id="344f8-285">*fieldAlias*</span></span> |<span data-ttu-id="344f8-286">(Volitelné) alias pro počítané agregovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="344f8-286">The (optional) alias for the calculated aggregated value.</span></span> <span data-ttu-id="344f8-287">Pokud není zadáno, je název pole **AggregatedValue**.</span><span class="sxs-lookup"><span data-stu-id="344f8-287">If not specified, the field name is **AggregatedValue**.</span></span> |
| <span data-ttu-id="344f8-288">*skupinové pole*</span><span class="sxs-lookup"><span data-stu-id="344f8-288">*groupField*</span></span> |<span data-ttu-id="344f8-289">Název pole, která nastavit výsledek se seskupují po.</span><span class="sxs-lookup"><span data-stu-id="344f8-289">The name of the field that the result set is grouped by.</span></span> |
| <span data-ttu-id="344f8-290">*Interval*</span><span class="sxs-lookup"><span data-stu-id="344f8-290">*Interval*</span></span> |<span data-ttu-id="344f8-291">Časový interval, ve formátu:**nnnNAME**.</span><span class="sxs-lookup"><span data-stu-id="344f8-291">The time interval, in the format:**nnnNAME**.</span></span> <span data-ttu-id="344f8-292">**nnn**je kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-292">**nnn** is the positive integer number.</span></span> <span data-ttu-id="344f8-293">**NÁZEV** je název intervalu.</span><span class="sxs-lookup"><span data-stu-id="344f8-293">**NAME** is the interval name.</span></span> <span data-ttu-id="344f8-294">Názvy podporovaný interval jsou velká a malá písmena a zahrnují: MILISEKUNDU [S], [S] SEKUNDU MINUTU [S], [S] HODINU dne [S], [S] měsíce a roku [S].</span><span class="sxs-lookup"><span data-stu-id="344f8-294">Supported interval names are case sensitive, and include:MILLISECOND[S], SECOND[S], MINUTE[S], HOUR[S], DAY[S], MONTH[S], and YEAR[S].</span></span> |

<span data-ttu-id="344f8-295">Interval možnost lze použít pouze v polích časových skupinu (například *TimeGenerated* a *TimeCreated*).</span><span class="sxs-lookup"><span data-stu-id="344f8-295">The interval option can only be used in Date/Time group fields (such as *TimeGenerated* and *TimeCreated*).</span></span> <span data-ttu-id="344f8-296">V současné době to nevynucuje službou, ale pole bez datum a čas, který je předán do back-end způsobí, že chyba v běhu.</span><span class="sxs-lookup"><span data-stu-id="344f8-296">Currently, this is not enforced by the service, but a field without Date/Time that is passed to the back end causes a runtime error.</span></span> <span data-ttu-id="344f8-297">Pokud se implementuje ověření schématu, rozhraní API služby odmítne dotazy, které používají pole bez datum a čas pro interval agregace.</span><span class="sxs-lookup"><span data-stu-id="344f8-297">When the schema validation is implemented, the service API rejects queries that use fields without Date/Time for interval aggregation.</span></span> <span data-ttu-id="344f8-298">Aktuální *měr* implementace podporuje interval seskupení pro všechny agregační funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-298">The current *Measure* implementation supports interval grouping for any aggregate function.</span></span>

<span data-ttu-id="344f8-299">Pokud je vynechaná klauzule BY, ale není zadaný interval (jako druhý syntaxe), *TimeGenerated* pole se předpokládá, že ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="344f8-299">If the BY clause is omitted, but an interval is specified (as a second syntax), the *TimeGenerated* field is assumed by default.</span></span>

<span data-ttu-id="344f8-300">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-300">Examples:</span></span>

<span data-ttu-id="344f8-301">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="344f8-301">**Example 1**</span></span>

    Type:Alert | measure count() as Count by ObjectId

<span data-ttu-id="344f8-302">Výstrahy podle skupin *ObjectID*a vypočítá počet výstrah pro každou skupinu.</span><span class="sxs-lookup"><span data-stu-id="344f8-302">Groups the alerts by *ObjectID*, and calculates the number of alerts for each group.</span></span> <span data-ttu-id="344f8-303">Agregovaná hodnota vrácena jako *počet* pole (alias).</span><span class="sxs-lookup"><span data-stu-id="344f8-303">The aggregated value is returned as the *Count* field (alias).</span></span>

<span data-ttu-id="344f8-304">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="344f8-304">**Example 2**</span></span>

    Type:Alert | measure count() interval 1HOUR

<span data-ttu-id="344f8-305">Pomocí skupin výstrahy podle intervalu 1 hodin *TimeGenerated* pole a vrátí počet výstrah v každém intervalu.</span><span class="sxs-lookup"><span data-stu-id="344f8-305">Groups the alerts by 1-hour intervals by using the *TimeGenerated* field, and returns the number of alerts in each interval.</span></span>

<span data-ttu-id="344f8-306">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="344f8-306">**Example 3**</span></span>

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

<span data-ttu-id="344f8-307">Stejné jako v předchozím příkladu, ale s aliasem agregované pole (*AlertsPerHour*).</span><span class="sxs-lookup"><span data-stu-id="344f8-307">Same as the previous example, but with an aggregated field alias (*AlertsPerHour*).</span></span>

<span data-ttu-id="344f8-308">**Příklad 4**</span><span class="sxs-lookup"><span data-stu-id="344f8-308">**Example 4**</span></span>

    * <span data-ttu-id="344f8-309">| míra count() podle 5DAYS TimeCreated intervalu</span><span class="sxs-lookup"><span data-stu-id="344f8-309">| measure count() by TimeCreated interval 5DAYS</span></span>

<span data-ttu-id="344f8-310">Výsledky jsou seskupeny podle intervaly 5 dnů pomocí *TimeCreated* pole a vrátí počet výsledků v každém intervalu.</span><span class="sxs-lookup"><span data-stu-id="344f8-310">Groups the results by 5-day intervals by using the *TimeCreated* field, and returns the number of results in each interval.</span></span>

<span data-ttu-id="344f8-311">**Příklad 5**</span><span class="sxs-lookup"><span data-stu-id="344f8-311">**Example 5**</span></span>

    Type:Alert | measure max(Severity) by WorkflowName

<span data-ttu-id="344f8-312">Skupiny výstrahy podle názvu úlohy a vrátí hodnotu maximální závažnost výstrahy pro každý pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="344f8-312">Groups the alerts by workload name, and returns the maximum alert severity value for each workflow.</span></span>

<span data-ttu-id="344f8-313">**Příklad 6**</span><span class="sxs-lookup"><span data-stu-id="344f8-313">**Example 6**</span></span>

    Type:Alert | measure min(Severity) by WorkflowName

<span data-ttu-id="344f8-314">Stejné jako v předchozím příkladu, ale *min* agregovat funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-314">Same as the previous example, but with the *min* aggregated function.</span></span>

<span data-ttu-id="344f8-315">**Příklad 7**</span><span class="sxs-lookup"><span data-stu-id="344f8-315">**Example 7**</span></span>

    Type:Perf | measure avg(CounterValue) by Computer

<span data-ttu-id="344f8-316">Skupiny výkonu počítačem a vypočítá průměrnou (průměr).</span><span class="sxs-lookup"><span data-stu-id="344f8-316">Groups Perf by computer, and calculates the average (avg).</span></span>

<span data-ttu-id="344f8-317">**Příklad 8**</span><span class="sxs-lookup"><span data-stu-id="344f8-317">**Example 8**</span></span>

    Type:Perf | measure sum(CounterValue) by Computer

<span data-ttu-id="344f8-318">Stejný jako předchozí příklad, ale používá *součet*.</span><span class="sxs-lookup"><span data-stu-id="344f8-318">Same as the previous example, but uses *sum*.</span></span>

<span data-ttu-id="344f8-319">**Příklad 9**</span><span class="sxs-lookup"><span data-stu-id="344f8-319">**Example 9**</span></span>

    Type:Perf | measure stddev(CounterValue) by Computer

<span data-ttu-id="344f8-320">Stejný jako předchozí příklad, ale používá *stddev*.</span><span class="sxs-lookup"><span data-stu-id="344f8-320">Same as the previous example, but uses *stddev*.</span></span>

<span data-ttu-id="344f8-321">**Příklad 10**</span><span class="sxs-lookup"><span data-stu-id="344f8-321">**Example 10**</span></span>

    Type:Perf | measure percentile70(CounterValue) by Computer

<span data-ttu-id="344f8-322">Stejný jako předchozí příklad, ale používá *percentile70*.</span><span class="sxs-lookup"><span data-stu-id="344f8-322">Same as the previous example, but uses *percentile70*.</span></span>

<span data-ttu-id="344f8-323">**Příklad 11**</span><span class="sxs-lookup"><span data-stu-id="344f8-323">**Example 11**</span></span>

    Type:Perf | measure pct70(CounterValue) by Computer

<span data-ttu-id="344f8-324">Stejný jako předchozí příklad, ale používá *pct70*.</span><span class="sxs-lookup"><span data-stu-id="344f8-324">Same as the previous example, but uses *pct70*.</span></span> <span data-ttu-id="344f8-325">Všimněte si, že *PCT ##* je pouze alias *PERCENTILU ##* funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-325">Note that *PCT##* is only an alias for *PERCENTILE##* function.</span></span>

<span data-ttu-id="344f8-326">**Příklad 12**</span><span class="sxs-lookup"><span data-stu-id="344f8-326">**Example 12**</span></span>

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

<span data-ttu-id="344f8-327">Skupiny výkonu nejdřív počítač a potom CounterName a vypočítá průměrnou (průměr).</span><span class="sxs-lookup"><span data-stu-id="344f8-327">Groups Perf first by Computer and then CounterName, and calculates the average (avg).</span></span>

<span data-ttu-id="344f8-328">**Příklad 13**</span><span class="sxs-lookup"><span data-stu-id="344f8-328">**Example 13**</span></span>

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

<span data-ttu-id="344f8-329">Získá nejvyšší pět pracovních s maximální počet výstrah.</span><span class="sxs-lookup"><span data-stu-id="344f8-329">Gets the top five workflows with the maximum number of alerts.</span></span>

<span data-ttu-id="344f8-330">**Příklad 14**</span><span class="sxs-lookup"><span data-stu-id="344f8-330">**Example 14**</span></span>

    * <span data-ttu-id="344f8-331">| míra countdistinct(Computer) podle typu</span><span class="sxs-lookup"><span data-stu-id="344f8-331">| measure countdistinct(Computer) by Type</span></span>

<span data-ttu-id="344f8-332">Spočítá počet jedinečných počítačů, vytváření sestav pro každého typu.</span><span class="sxs-lookup"><span data-stu-id="344f8-332">Counts the number of unique computers reporting for each Type.</span></span>

<span data-ttu-id="344f8-333">**Příklad 15**</span><span class="sxs-lookup"><span data-stu-id="344f8-333">**Example 15**</span></span>

    * <span data-ttu-id="344f8-334">| míra countdistinct(Computer) intervalu 1 hodina</span><span class="sxs-lookup"><span data-stu-id="344f8-334">| measure countdistinct(Computer) Interval 1HOUR</span></span>

<span data-ttu-id="344f8-335">Spočítá počet jedinečných počítačů, vytváření sestav pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="344f8-335">Counts the number of unique computers reporting for every hour.</span></span>

<span data-ttu-id="344f8-336">**Příklad 16**</span><span class="sxs-lookup"><span data-stu-id="344f8-336">**Example 16**</span></span>

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

<span data-ttu-id="344f8-337">Skupiny % času procesoru počítačem a vrátí průměrnou hodnotu pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="344f8-337">Groups % Processor Time by Computer, and returns the average for every hour.</span></span>

<span data-ttu-id="344f8-338">**Příklad 17**</span><span class="sxs-lookup"><span data-stu-id="344f8-338">**Example 17**</span></span>

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

<span data-ttu-id="344f8-339">Skupiny W3CIISLog metodou a vrátí maximální pro každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="344f8-339">Groups W3CIISLog by method, and returns the maximum for every 5 minutes.</span></span>

<span data-ttu-id="344f8-340">**Příklad 18**</span><span class="sxs-lookup"><span data-stu-id="344f8-340">**Example 18**</span></span>

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

<span data-ttu-id="344f8-341">% Času procesoru počítačem skupiny a vrátí minimální, průměr, 75 percentilu a maximální pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="344f8-341">Groups % Processor Time by computer, and returns the minimum, average, 75th percentile, and maximum for every hour.</span></span>

<span data-ttu-id="344f8-342">**Příklad 19**</span><span class="sxs-lookup"><span data-stu-id="344f8-342">**Example 19**</span></span>

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

<span data-ttu-id="344f8-343">Skupiny % času procesoru nejprve podle počítače a poté podle názvu Instance a vrátí minimální, průměr, 75 percentilu a maximální pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="344f8-343">Groups % Processor Time first by computer and then by Instance name, and returns the minimum, average, 75th percentile, and maximum for every hour.</span></span>

<span data-ttu-id="344f8-344">**Příklad 20**</span><span class="sxs-lookup"><span data-stu-id="344f8-344">**Example 20**</span></span>

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

<span data-ttu-id="344f8-345">Vypočítá maximální počet zápisů disku za minutu pro každý disk ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="344f8-345">Calculates the maximum of disk writes per minute for every disk on your computer.</span></span>

### <a name="where"></a><span data-ttu-id="344f8-346">kde</span><span class="sxs-lookup"><span data-stu-id="344f8-346">Where</span></span>
<span data-ttu-id="344f8-347">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-347">Syntax:</span></span>

```
**where** AggregatedValue>20
```

<span data-ttu-id="344f8-348">Lze použít pouze po *měr* příkaz pro další filtrování agregované výsledky, které *měr* má vytvořeného agregační funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-348">Can only be used after a *Measure* command to further filter the aggregated results that the *Measure* aggregation function has produced.</span></span>

<span data-ttu-id="344f8-349">Příklady:</span><span class="sxs-lookup"><span data-stu-id="344f8-349">Examples:</span></span>

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a><span data-ttu-id="344f8-350">Odstranění duplicitních dat</span><span class="sxs-lookup"><span data-stu-id="344f8-350">Dedup</span></span>
<span data-ttu-id="344f8-351">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="344f8-351">Syntax:</span></span>

    Dedup FieldName

<span data-ttu-id="344f8-352">Vrátí první dokument pro každou jedinečnou hodnotu pole daného nalezen.</span><span class="sxs-lookup"><span data-stu-id="344f8-352">Returns the first document found for every unique value of the given field.</span></span>

<span data-ttu-id="344f8-353">Příklad:</span><span class="sxs-lookup"><span data-stu-id="344f8-353">Example:</span></span>

    Type=Event | Dedup EventID | sort TimeGenerated DESC

<span data-ttu-id="344f8-354">Tento příklad vrátí jedna událost (nejnovější událost) na ID události.</span><span class="sxs-lookup"><span data-stu-id="344f8-354">This example returns one event (the latest event) per EventID.</span></span>

### <a name="join"></a><span data-ttu-id="344f8-355">Spojit</span><span class="sxs-lookup"><span data-stu-id="344f8-355">Join</span></span>
<span data-ttu-id="344f8-356">Spojí dva dotazy k jedné sadě výsledků výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-356">Joins the results of two queries to form a single result set.</span></span>  <span data-ttu-id="344f8-357">Podporuje několik typů spojení, které jsou popsané v tabulce postupujte podle kroků.</span><span class="sxs-lookup"><span data-stu-id="344f8-357">Supports multiple join types described in the follow table.</span></span>

| <span data-ttu-id="344f8-358">Typ spojení</span><span class="sxs-lookup"><span data-stu-id="344f8-358">Join type</span></span> | <span data-ttu-id="344f8-359">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-359">Description</span></span> |
|:--|:--|
| <span data-ttu-id="344f8-360">vnitřní</span><span class="sxs-lookup"><span data-stu-id="344f8-360">inner</span></span> | <span data-ttu-id="344f8-361">Vrátí pouze záznamy s odpovídající hodnotou v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="344f8-361">Return only records with a matching value in both queries.</span></span> |
| <span data-ttu-id="344f8-362">vnější</span><span class="sxs-lookup"><span data-stu-id="344f8-362">outer</span></span> | <span data-ttu-id="344f8-363">Vrátí všechny záznamy z obou dotazů.</span><span class="sxs-lookup"><span data-stu-id="344f8-363">Return all records from both queries.</span></span>  |
| <span data-ttu-id="344f8-364">Vlevo</span><span class="sxs-lookup"><span data-stu-id="344f8-364">left</span></span>  | <span data-ttu-id="344f8-365">Vrátí všechny záznamy z levé dotazu a odpovídající záznamy z pravé dotazu.</span><span class="sxs-lookup"><span data-stu-id="344f8-365">Return all records from left query and matching records from right query.</span></span> |


- <span data-ttu-id="344f8-366">Spojení aktuálně nepodporují dotazy, které zahrnují **IN** – klíčové slovo, **měr** příkaz nebo **rozšíření** příkaz, pokud je cílem pole pravé dotazu.</span><span class="sxs-lookup"><span data-stu-id="344f8-366">Joins do not currently support queries that include the **IN** keyword, the **Measure** command or the **Extend** command if it targets a field from the right query.</span></span>
- <span data-ttu-id="344f8-367">Můžete zahrnout aktuálně pouze jednoho pole ke spojení.</span><span class="sxs-lookup"><span data-stu-id="344f8-367">You can currently include only a single field in a join.</span></span>
- <span data-ttu-id="344f8-368">Hledání jednoduchého nesmí obsahovat více než jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="344f8-368">A single search may not include more than one join.</span></span>

<span data-ttu-id="344f8-369">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="344f8-369">**Syntax**</span></span>

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

<span data-ttu-id="344f8-370">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="344f8-370">**Examples**</span></span>

<span data-ttu-id="344f8-371">Pro ilustraci typů různých spojení, zvažte připojení typu dat shromážděných z vlastního protokolu volána MyBackup_CL s prezenčního signálu pro jednotlivé počítače.</span><span class="sxs-lookup"><span data-stu-id="344f8-371">To illustrate the different join types, consider joining a data type collected from a custom log called MyBackup_CL with the heartbeat for each computer.</span></span>  <span data-ttu-id="344f8-372">Tyto datové typy mají následující data.</span><span class="sxs-lookup"><span data-stu-id="344f8-372">These datatypes have the following data.</span></span>

`Type = MyBackup_CL`

| <span data-ttu-id="344f8-373">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-373">TimeGenerated</span></span> | <span data-ttu-id="344f8-374">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-374">Computer</span></span> | <span data-ttu-id="344f8-375">LastBackupStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-375">LastBackupStatus</span></span> |
|:---|:---|:---|
| <span data-ttu-id="344f8-376">4/20/2017 01:26:32.137 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-376">4/20/2017 01:26:32.137 AM</span></span> | <span data-ttu-id="344f8-377">Srv01.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-377">srv01.contoso.com</span></span> | <span data-ttu-id="344f8-378">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-378">Success</span></span> |
| <span data-ttu-id="344f8-379">4/20/2017 02:13:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-379">4/20/2017 02:13:12.381 AM</span></span> | <span data-ttu-id="344f8-380">SRV02.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-380">srv02.contoso.com</span></span> | <span data-ttu-id="344f8-381">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-381">Success</span></span> |
| <span data-ttu-id="344f8-382">4/20/2017 02:13:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-382">4/20/2017 02:13:12.381 AM</span></span> | <span data-ttu-id="344f8-383">srv03.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-383">srv03.contoso.com</span></span> | <span data-ttu-id="344f8-384">Selhání</span><span class="sxs-lookup"><span data-stu-id="344f8-384">Failure</span></span> |

<span data-ttu-id="344f8-385">`Type = Hearbeat`(Jenom podmnožinu zobrazená pole)</span><span class="sxs-lookup"><span data-stu-id="344f8-385">`Type = Hearbeat` (Only a subset of fields shown)</span></span>

| <span data-ttu-id="344f8-386">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-386">TimeGenerated</span></span> | <span data-ttu-id="344f8-387">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-387">Computer</span></span> | <span data-ttu-id="344f8-388">ComputerIP</span><span class="sxs-lookup"><span data-stu-id="344f8-388">ComputerIP</span></span> |
|:---|:---|:---|
| <span data-ttu-id="344f8-389">4/21/2017 12:01:34.482 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-389">4/21/2017 12:01:34.482 PM</span></span> | <span data-ttu-id="344f8-390">Srv01.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-390">srv01.contoso.com</span></span> | <span data-ttu-id="344f8-391">10.10.100.1</span><span class="sxs-lookup"><span data-stu-id="344f8-391">10.10.100.1</span></span> |
| <span data-ttu-id="344f8-392">4/21/2017 12:02:21.916 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-392">4/21/2017 12:02:21.916 PM</span></span> | <span data-ttu-id="344f8-393">SRV02.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-393">srv02.contoso.com</span></span> | <span data-ttu-id="344f8-394">10.10.100.2</span><span class="sxs-lookup"><span data-stu-id="344f8-394">10.10.100.2</span></span> |
| <span data-ttu-id="344f8-395">4/21/2017 12:01:47.373 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-395">4/21/2017 12:01:47.373 PM</span></span> | <span data-ttu-id="344f8-396">srv04.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-396">srv04.contoso.com</span></span> | <span data-ttu-id="344f8-397">10.10.100.4</span><span class="sxs-lookup"><span data-stu-id="344f8-397">10.10.100.4</span></span> |

#### <a name="inner-join"></a><span data-ttu-id="344f8-398">vnitřní spojení</span><span class="sxs-lookup"><span data-stu-id="344f8-398">inner join</span></span>

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

<span data-ttu-id="344f8-399">Vrátí následující záznamy, kde pole počítače odpovídá pro oba datové typy.</span><span class="sxs-lookup"><span data-stu-id="344f8-399">Returns the following records where the computer field matches for both datatypes.</span></span>

| <span data-ttu-id="344f8-400">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-400">Computer</span></span>| <span data-ttu-id="344f8-401">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-401">TimeGenerated</span></span> | <span data-ttu-id="344f8-402">LastBackupStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-402">LastBackupStatus</span></span> | <span data-ttu-id="344f8-403">TimeGenerated_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-403">TimeGenerated_joined</span></span> | <span data-ttu-id="344f8-404">ComputerIP_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-404">ComputerIP_joined</span></span> | <span data-ttu-id="344f8-405">Type_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-405">Type_joined</span></span> |
|:---|:---|:---|:---|:---|:---|
| <span data-ttu-id="344f8-406">Srv01.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-406">srv01.contoso.com</span></span> | <span data-ttu-id="344f8-407">4/20/2017 01:26:32.137 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-407">4/20/2017 01:26:32.137 AM</span></span> | <span data-ttu-id="344f8-408">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-408">Success</span></span> | <span data-ttu-id="344f8-409">4/21/2017 12:01:34.482 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-409">4/21/2017 12:01:34.482 PM</span></span> | <span data-ttu-id="344f8-410">10.10.100.1</span><span class="sxs-lookup"><span data-stu-id="344f8-410">10.10.100.1</span></span> | <span data-ttu-id="344f8-411">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-411">Heartbeat</span></span> |
| <span data-ttu-id="344f8-412">SRV02.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-412">srv02.contoso.com</span></span> | <span data-ttu-id="344f8-413">4/20/2017 02:13:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-413">4/20/2017 02:13:12.381 AM</span></span> | <span data-ttu-id="344f8-414">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-414">Success</span></span> | <span data-ttu-id="344f8-415">4/21/2017 12:02:21.916 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-415">4/21/2017 12:02:21.916 PM</span></span> | <span data-ttu-id="344f8-416">10.10.100.2</span><span class="sxs-lookup"><span data-stu-id="344f8-416">10.10.100.2</span></span> | <span data-ttu-id="344f8-417">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-417">Heartbeat</span></span> |


#### <a name="outer-join"></a><span data-ttu-id="344f8-418">vnější spojení</span><span class="sxs-lookup"><span data-stu-id="344f8-418">outer join</span></span>

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

<span data-ttu-id="344f8-419">Vrátí následující záznamy pro oba datové typy.</span><span class="sxs-lookup"><span data-stu-id="344f8-419">Returns the following records for both datatypes.</span></span>

| <span data-ttu-id="344f8-420">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-420">Computer</span></span>| <span data-ttu-id="344f8-421">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-421">TimeGenerated</span></span> | <span data-ttu-id="344f8-422">LastBackupStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-422">LastBackupStatus</span></span> | <span data-ttu-id="344f8-423">TimeGenerated_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-423">TimeGenerated_joined</span></span> | <span data-ttu-id="344f8-424">ComputerIP_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-424">ComputerIP_joined</span></span> | <span data-ttu-id="344f8-425">Type_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-425">Type_joined</span></span> |
|:---|:---|:---|:---|:---|:---|
| <span data-ttu-id="344f8-426">Srv01.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-426">srv01.contoso.com</span></span> | <span data-ttu-id="344f8-427">4/20/2017 01:26:32.137 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-427">4/20/2017 01:26:32.137 AM</span></span> | <span data-ttu-id="344f8-428">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-428">Success</span></span>  | <span data-ttu-id="344f8-429">4/21/2017 12:01:34.482 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-429">4/21/2017 12:01:34.482 PM</span></span> | <span data-ttu-id="344f8-430">10.10.100.1</span><span class="sxs-lookup"><span data-stu-id="344f8-430">10.10.100.1</span></span> | <span data-ttu-id="344f8-431">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-431">Heartbeat</span></span> |
| <span data-ttu-id="344f8-432">SRV02.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-432">srv02.contoso.com</span></span> | <span data-ttu-id="344f8-433">4/20/2017 02:14:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-433">4/20/2017 02:14:12.381 AM</span></span> | <span data-ttu-id="344f8-434">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-434">Success</span></span>  | <span data-ttu-id="344f8-435">4/21/2017 12:02:21.916 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-435">4/21/2017 12:02:21.916 PM</span></span> | <span data-ttu-id="344f8-436">10.10.100.2</span><span class="sxs-lookup"><span data-stu-id="344f8-436">10.10.100.2</span></span> | <span data-ttu-id="344f8-437">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-437">Heartbeat</span></span> |
| <span data-ttu-id="344f8-438">srv03.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-438">srv03.contoso.com</span></span> | <span data-ttu-id="344f8-439">4/20/2017 01:33:35.974 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-439">4/20/2017 01:33:35.974 AM</span></span> | <span data-ttu-id="344f8-440">Selhání</span><span class="sxs-lookup"><span data-stu-id="344f8-440">Failure</span></span>  | <span data-ttu-id="344f8-441">4/21/2017 12:01:47.373 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-441">4/21/2017 12:01:47.373 PM</span></span> | | |
| <span data-ttu-id="344f8-442">srv04.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-442">srv04.contoso.com</span></span> |                           |          | <span data-ttu-id="344f8-443">4/21/2017 12:01:47.373 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-443">4/21/2017 12:01:47.373 PM</span></span> | <span data-ttu-id="344f8-444">10.10.100.2</span><span class="sxs-lookup"><span data-stu-id="344f8-444">10.10.100.2</span></span> | <span data-ttu-id="344f8-445">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-445">Heartbeat</span></span> |



#### <a name="left-join"></a><span data-ttu-id="344f8-446">Levé spojení</span><span class="sxs-lookup"><span data-stu-id="344f8-446">left join</span></span>

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

<span data-ttu-id="344f8-447">Vrátí následující záznamy z MyBackup_CL s všechna odpovídající pole z prezenčního signálu.</span><span class="sxs-lookup"><span data-stu-id="344f8-447">Returns the following records from MyBackup_CL with any matching fields from Heartbeat.</span></span>

| <span data-ttu-id="344f8-448">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-448">Computer</span></span>| <span data-ttu-id="344f8-449">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-449">TimeGenerated</span></span> | <span data-ttu-id="344f8-450">LastBackupStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-450">LastBackupStatus</span></span> | <span data-ttu-id="344f8-451">TimeGenerated_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-451">TimeGenerated_joined</span></span> | <span data-ttu-id="344f8-452">ComputerIP_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-452">ComputerIP_joined</span></span> | <span data-ttu-id="344f8-453">Type_joined</span><span class="sxs-lookup"><span data-stu-id="344f8-453">Type_joined</span></span> |
|:---|:---|:---|:---|:---|:---|
| <span data-ttu-id="344f8-454">Srv01.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-454">srv01.contoso.com</span></span> | <span data-ttu-id="344f8-455">4/20/2017 01:26:32.137 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-455">4/20/2017 01:26:32.137 AM</span></span> | <span data-ttu-id="344f8-456">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-456">Success</span></span> | <span data-ttu-id="344f8-457">4/21/2017 12:01:34.482 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-457">4/21/2017 12:01:34.482 PM</span></span> | <span data-ttu-id="344f8-458">10.10.100.1</span><span class="sxs-lookup"><span data-stu-id="344f8-458">10.10.100.1</span></span> | <span data-ttu-id="344f8-459">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-459">Heartbeat</span></span> |
| <span data-ttu-id="344f8-460">SRV02.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-460">srv02.contoso.com</span></span> | <span data-ttu-id="344f8-461">4/20/2017 02:13:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-461">4/20/2017 02:13:12.381 AM</span></span> | <span data-ttu-id="344f8-462">Úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-462">Success</span></span> | <span data-ttu-id="344f8-463">4/21/2017 12:02:21.916 PM</span><span class="sxs-lookup"><span data-stu-id="344f8-463">4/21/2017 12:02:21.916 PM</span></span> | <span data-ttu-id="344f8-464">10.10.100.2</span><span class="sxs-lookup"><span data-stu-id="344f8-464">10.10.100.2</span></span> | <span data-ttu-id="344f8-465">Prezenční signál</span><span class="sxs-lookup"><span data-stu-id="344f8-465">Heartbeat</span></span> |
| <span data-ttu-id="344f8-466">srv03.contoso.com</span><span class="sxs-lookup"><span data-stu-id="344f8-466">srv03.contoso.com</span></span> | <span data-ttu-id="344f8-467">4/20/2017 02:13:12.381 AM</span><span class="sxs-lookup"><span data-stu-id="344f8-467">4/20/2017 02:13:12.381 AM</span></span> | <span data-ttu-id="344f8-468">Selhání</span><span class="sxs-lookup"><span data-stu-id="344f8-468">Failure</span></span> | | | |


### <a name="extend"></a><span data-ttu-id="344f8-469">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="344f8-469">Extend</span></span>
<span data-ttu-id="344f8-470">Umožňuje vytvořit běhu pole v dotazech.</span><span class="sxs-lookup"><span data-stu-id="344f8-470">Allows you to create run-time fields in queries.</span></span> <span data-ttu-id="344f8-471">Všimněte si, že spuštění pole nelze použít pomocí příkazu měr k provedení agregace.</span><span class="sxs-lookup"><span data-stu-id="344f8-471">Note that run-time fields cannot be used with the measure command to perform aggregation.</span></span>

<span data-ttu-id="344f8-472">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="344f8-472">**Example 1**</span></span>

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
<span data-ttu-id="344f8-473">Ukazuje váha skóre doporučení pro doporučení vyhodnocení SQL.</span><span class="sxs-lookup"><span data-stu-id="344f8-473">Shows weighted recommendation score for SQL Assessment recommendations.</span></span>

<span data-ttu-id="344f8-474">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="344f8-474">**Example 2**</span></span>

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
<span data-ttu-id="344f8-475">Zobrazí hodnotu čítače v články znalostní báze místo bajtů.</span><span class="sxs-lookup"><span data-stu-id="344f8-475">Shows counter value in KBs instead of bytes.</span></span>

<span data-ttu-id="344f8-476">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="344f8-476">**Example 3**</span></span>

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
<span data-ttu-id="344f8-477">Hodnota WireData TotalBytes škáluje tak, aby všechny výsledky jsou v rozmezí od 0 do 100.</span><span class="sxs-lookup"><span data-stu-id="344f8-477">Scales the value of WireData TotalBytes, such that all results are between 0 and 100.</span></span>

<span data-ttu-id="344f8-478">**Příklad 4**</span><span class="sxs-lookup"><span data-stu-id="344f8-478">**Example 4**</span></span>

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
<span data-ttu-id="344f8-479">Značky hodnoty čítače výkonu menší než 50 procent jako nízká, jiné jako Vysoká.</span><span class="sxs-lookup"><span data-stu-id="344f8-479">Tags Perf Counter Values less than 50 percent as LOW, and others as HIGH.</span></span>

<span data-ttu-id="344f8-480">**Příklad 5**</span><span class="sxs-lookup"><span data-stu-id="344f8-480">**Example 5**</span></span>

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
<span data-ttu-id="344f8-481">Vypočítá maximální počet zápisů disku za minutu pro každý disk ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="344f8-481">Calculates the maximum of disk writes per minute for every disk on your computer.</span></span>

<span data-ttu-id="344f8-482">**Podporované funkce**</span><span class="sxs-lookup"><span data-stu-id="344f8-482">**Supported functions**</span></span>

| <span data-ttu-id="344f8-483">Funkce</span><span class="sxs-lookup"><span data-stu-id="344f8-483">Function</span></span> | <span data-ttu-id="344f8-484">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-484">Description</span></span> | <span data-ttu-id="344f8-485">Příklady syntaxe</span><span class="sxs-lookup"><span data-stu-id="344f8-485">Syntax examples</span></span> |
| --- | --- | --- |
| <span data-ttu-id="344f8-486">Abs</span><span class="sxs-lookup"><span data-stu-id="344f8-486">abs</span></span> |<span data-ttu-id="344f8-487">Vrátí absolutní hodnotu zadanou hodnotu nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-487">Returns the absolute value of the specified value or function.</span></span> |`abs(x)` <br> `abs(-5)` |
| <span data-ttu-id="344f8-488">ACOS</span><span class="sxs-lookup"><span data-stu-id="344f8-488">acos</span></span> |<span data-ttu-id="344f8-489">Vrací kosinus oblouk hodnotu, nebo funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-489">Returns arc cosine of a value or a function.</span></span> |`acos(x)` |
| <span data-ttu-id="344f8-490">a</span><span class="sxs-lookup"><span data-stu-id="344f8-490">and</span></span> |<span data-ttu-id="344f8-491">Vrátí hodnotu true, pokud všechny jeho operandy vyhodnotit na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="344f8-491">Returns a value of true if and only if all of its operands evaluate to true.</span></span> |`and(not(exists(popularity)),exists(price))` |
| <span data-ttu-id="344f8-492">ASIN</span><span class="sxs-lookup"><span data-stu-id="344f8-492">asin</span></span> |<span data-ttu-id="344f8-493">Vrací sinus oblouk hodnotu, nebo funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-493">Returns arc sine of a value or a function.</span></span> |`asin(x)` |
| <span data-ttu-id="344f8-494">Atan</span><span class="sxs-lookup"><span data-stu-id="344f8-494">atan</span></span> |<span data-ttu-id="344f8-495">Vrací tangens oblouk hodnotu, nebo funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-495">Returns arc tangent of a value or a function.</span></span> |`atan(x)` |
| <span data-ttu-id="344f8-496">ATAN2</span><span class="sxs-lookup"><span data-stu-id="344f8-496">atan2</span></span> |<span data-ttu-id="344f8-497">Vrací úhel, vyplývající z převodu obdélníkový souřadnice x, y polární souřadnice.</span><span class="sxs-lookup"><span data-stu-id="344f8-497">Returns the angle resulting from the conversion of the rectangular coordinates x,y to polar coordinates.</span></span> |`atan2(x,y)` |
| <span data-ttu-id="344f8-498">cbrt –</span><span class="sxs-lookup"><span data-stu-id="344f8-498">cbrt</span></span> |<span data-ttu-id="344f8-499">Kořenové datové krychle.</span><span class="sxs-lookup"><span data-stu-id="344f8-499">Cube root.</span></span> |`cbrt(x)` |
| <span data-ttu-id="344f8-500">ceil</span><span class="sxs-lookup"><span data-stu-id="344f8-500">ceil</span></span> |<span data-ttu-id="344f8-501">Zaokrouhlí na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-501">Rounds up to an integer.</span></span> |`ceil(x)`  <br> <span data-ttu-id="344f8-502">`ceil(5.6)`Vrátí hodnotu 6</span><span class="sxs-lookup"><span data-stu-id="344f8-502">`ceil(5.6)` Returns 6</span></span> |
| <span data-ttu-id="344f8-503">Cos</span><span class="sxs-lookup"><span data-stu-id="344f8-503">cos</span></span> |<span data-ttu-id="344f8-504">Vrátí kosinus úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-504">Returns cosine of an angle.</span></span> |`cos(x)` |
| <span data-ttu-id="344f8-505">COSH</span><span class="sxs-lookup"><span data-stu-id="344f8-505">cosh</span></span> |<span data-ttu-id="344f8-506">Vrací hyperbolický kosinus úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-506">Returns hyperbolic cosine of an angle.</span></span> |`cosh(x)` |
| <span data-ttu-id="344f8-507">DEF</span><span class="sxs-lookup"><span data-stu-id="344f8-507">def</span></span> |<span data-ttu-id="344f8-508">Zkratka pro výchozí.</span><span class="sxs-lookup"><span data-stu-id="344f8-508">Abbreviation for default.</span></span> <span data-ttu-id="344f8-509">Vrátí hodnotu pole "pole".</span><span class="sxs-lookup"><span data-stu-id="344f8-509">Returns the value of field "field".</span></span> <span data-ttu-id="344f8-510">Pokud pole neexistuje, vrátí výchozí hodnotu zadanou a výsledkem je první hodnota, kde: `exists()==true`.</span><span class="sxs-lookup"><span data-stu-id="344f8-510">If the field does not exist, returns the default value specified and yields the first value where: `exists()==true`.</span></span> |<span data-ttu-id="344f8-511">`def(rating,5)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-511">`def(rating,5)`.</span></span> <span data-ttu-id="344f8-512">Tato funkce def() vrátí hodnocení nebo pokud není zadán žádný hodnocení v dokumentu, vrátí 5.</span><span class="sxs-lookup"><span data-stu-id="344f8-512">This def() function returns the rating, or if no rating is specified in the document, returns 5.</span></span> <br> <span data-ttu-id="344f8-513">`def(myfield, 1.0)`je ekvivalentní `if(exists(myfield),myfield,1.0)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-513">`def(myfield, 1.0)` is equivalent to `if(exists(myfield),myfield,1.0)`.</span></span> |
| <span data-ttu-id="344f8-514">stupňů</span><span class="sxs-lookup"><span data-stu-id="344f8-514">deg</span></span> |<span data-ttu-id="344f8-515">Převede radiánech stupňů.</span><span class="sxs-lookup"><span data-stu-id="344f8-515">Converts radians to degrees.</span></span> |`deg(x)` |
| <span data-ttu-id="344f8-516">div</span><span class="sxs-lookup"><span data-stu-id="344f8-516">div</span></span> |<span data-ttu-id="344f8-517">`div(x,y)`rozdělí x, y.</span><span class="sxs-lookup"><span data-stu-id="344f8-517">`div(x,y)` divides x by y.</span></span> |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| <span data-ttu-id="344f8-518">DIST</span><span class="sxs-lookup"><span data-stu-id="344f8-518">dist</span></span> |<span data-ttu-id="344f8-519">Vrací vzdálenost mezi dvěma vektorů, (body) v n dimenzí místa.</span><span class="sxs-lookup"><span data-stu-id="344f8-519">Returns the distance between two vectors, (points) in an n-dimensional space.</span></span> <span data-ttu-id="344f8-520">Přebírá napájení plus dva nebo více instancí ValueSource a vypočítá vzdálenosti mezi dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="344f8-520">Takes in the power, plus two or more, ValueSource instances, and calculates the distances between the two vectors.</span></span> <span data-ttu-id="344f8-521">Každý ValueSource musí být číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-521">Each ValueSource must be a number.</span></span> <span data-ttu-id="344f8-522">Musí být sudé číslo instancí ValueSource předaná a metoda předpokládá, že první polovinu představují první vektoru a druhou polovinu představují druhý vektoru.</span><span class="sxs-lookup"><span data-stu-id="344f8-522">There must be an even number of ValueSource instances passed in, and the method assumes that the first half represent the first vector and the second half represent the second vector.</span></span> |<span data-ttu-id="344f8-523">`dist(2, x, y, 0, 0)`Vypočítá Euclidean vzdálenost mezi (0,0) a (x, y) pro každý dokument.</span><span class="sxs-lookup"><span data-stu-id="344f8-523">`dist(2, x, y, 0, 0)` Calculates the Euclidean distance between (0,0) and (x,y) for each document.</span></span> <br> <span data-ttu-id="344f8-524">`dist(1, x, y, 0, 0)`Vypočítá Manhattan (taxicab) vzdálenost mezi (0,0) a (x, y) pro každý dokument.</span><span class="sxs-lookup"><span data-stu-id="344f8-524">`dist(1, x, y, 0, 0)` Calculates the Manhattan (taxicab) distance between (0,0) and (x,y) for each document.</span></span> <br> <span data-ttu-id="344f8-525">`dist(2,,x,y,z,0,0,0)`Euclidean vzdálenost mezi (0,0,0) a (x, y, z) pro každý dokument.</span><span class="sxs-lookup"><span data-stu-id="344f8-525">`dist(2,,x,y,z,0,0,0)` Euclidean distance between (0,0,0) and (x,y,z) for each document.</span></span><br><span data-ttu-id="344f8-526">`dist(1,x,y,z,e,f,g)`Manhattan vzdálenost mezi (x, y, z) a (e, f, g), kde každý znak je název pole.</span><span class="sxs-lookup"><span data-stu-id="344f8-526">`dist(1,x,y,z,e,f,g)` Manhattan distance between (x,y,z) and (e,f,g), where each letter is a field name.</span></span> |
| <span data-ttu-id="344f8-527">existuje</span><span class="sxs-lookup"><span data-stu-id="344f8-527">exists</span></span> |<span data-ttu-id="344f8-528">Vrátí hodnotu TRUE, pokud žádné člen pole existuje.</span><span class="sxs-lookup"><span data-stu-id="344f8-528">Returns TRUE if any member of the field exists.</span></span> |<span data-ttu-id="344f8-529">`exists(author)`Vrátí hodnotu TRUE pro každý dokument, který má hodnotu v poli "Autor".</span><span class="sxs-lookup"><span data-stu-id="344f8-529">`exists(author)` Returns TRUE for any document that has a value in the "author" field.</span></span><br><span data-ttu-id="344f8-530">`exists(query(price:5.00))`Vrátí hodnotu TRUE, pokud "cena" odpovídá, "5.00".</span><span class="sxs-lookup"><span data-stu-id="344f8-530">`exists(query(price:5.00))` Returns TRUE if "price" matches,"5.00".</span></span> |
| <span data-ttu-id="344f8-531">Exp</span><span class="sxs-lookup"><span data-stu-id="344f8-531">exp</span></span> |<span data-ttu-id="344f8-532">Vrátí Eulerova na číslo na mocninu x.</span><span class="sxs-lookup"><span data-stu-id="344f8-532">Returns Euler's number raised to power x.</span></span> |`exp(x)` |
| <span data-ttu-id="344f8-533">Floor</span><span class="sxs-lookup"><span data-stu-id="344f8-533">floor</span></span> |<span data-ttu-id="344f8-534">Zaokrouhlí číslo dolů na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-534">Rounds down to an integer.</span></span> |`floor(x)`  <br> <span data-ttu-id="344f8-535">`floor(5.6)`Vrátí hodnotu 5</span><span class="sxs-lookup"><span data-stu-id="344f8-535">`floor(5.6)` Returns 5</span></span> |
| <span data-ttu-id="344f8-536">hypo</span><span class="sxs-lookup"><span data-stu-id="344f8-536">hypo</span></span> |<span data-ttu-id="344f8-537">Vrátí sqrt(sum(pow(x,2),pow(y,2))) bez zprostředkující přetečení nebo podtečení.</span><span class="sxs-lookup"><span data-stu-id="344f8-537">Returns sqrt(sum(pow(x,2),pow(y,2))) without intermediate overflow or underflow.</span></span> |`hypo(x,y)`  <br> ` |
| <span data-ttu-id="344f8-538">Pokud</span><span class="sxs-lookup"><span data-stu-id="344f8-538">if</span></span> |<span data-ttu-id="344f8-539">Umožňuje podmíněného funkce dotazy.</span><span class="sxs-lookup"><span data-stu-id="344f8-539">Enables conditional function queries.</span></span> <span data-ttu-id="344f8-540">V `if(test,value1,value2)`, test se nebo odkazuje na logickou hodnotu nebo výraz, který vrací logickou hodnotu (TRUE nebo FALSE).</span><span class="sxs-lookup"><span data-stu-id="344f8-540">In `if(test,value1,value2)`, test is or refers to a logical value or expression that returns a logical value (TRUE or FALSE).</span></span> <span data-ttu-id="344f8-541">`value1`je vrácena hodnota funkce Pokud test vypočítá hodnotu TRUE.</span><span class="sxs-lookup"><span data-stu-id="344f8-541">`value1` is the value returned by the function if test yields TRUE.</span></span> <span data-ttu-id="344f8-542">`value2`je vrácena hodnota funkce Pokud test vypočítá hodnotu FALSE.</span><span class="sxs-lookup"><span data-stu-id="344f8-542">`value2` is the value returned by the function if test yields FALSE.</span></span> <span data-ttu-id="344f8-543">Výraz může být žádné funkce, které výstupy logické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="344f8-543">An expression can be any function which outputs boolean values.</span></span> <span data-ttu-id="344f8-544">Může být také funkce vracení číselných hodnot, ve kterých se interpretuje jako false hodnota 0, nebo vrácení řetězce, ve které případu prázdný řetězec interpretována jako false.</span><span class="sxs-lookup"><span data-stu-id="344f8-544">It can also be a function returning numeric values, in which case value 0 is interpreted as false, or returning strings, in which case empty string is interpreted as false.</span></span> |<span data-ttu-id="344f8-545">`if(termfreq(cat,'electronics'),popularity,42)`Tato funkce zkontroluje každý dokument zobrazit, pokud obsahuje termín "electronics" v poli cat.</span><span class="sxs-lookup"><span data-stu-id="344f8-545">`if(termfreq(cat,'electronics'),popularity,42)` This function checks each document to see if it contains the term "electronics" in the cat field.</span></span> <span data-ttu-id="344f8-546">Pokud ano, je vrácena hodnota pole oblíbenosti.</span><span class="sxs-lookup"><span data-stu-id="344f8-546">If it does, then the value of the popularity field is returned.</span></span> <span data-ttu-id="344f8-547">Jinak je vrácena hodnota 42.</span><span class="sxs-lookup"><span data-stu-id="344f8-547">Otherwise, the value of 42 is returned.</span></span> |
| <span data-ttu-id="344f8-548">lineární</span><span class="sxs-lookup"><span data-stu-id="344f8-548">linear</span></span> |<span data-ttu-id="344f8-549">Implementuje `m*x+c`, kde m a c jsou konstanty a x je libovolný funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-549">Implements `m*x+c`, where m and c are constants, and x is an arbitrary function.</span></span> <span data-ttu-id="344f8-550">Jde o ekvivalent `sum(product(m,x),c)`, ale poněkud efektivnější, jak jsou implementované jako jedinou funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-550">This is equivalent to `sum(product(m,x),c)`, but slightly more efficient as it is implemented as a single function.</span></span> |<span data-ttu-id="344f8-551">`linear(x,m,c) linear(x,2,4)`Vrátí`2*x+4`</span><span class="sxs-lookup"><span data-stu-id="344f8-551">`linear(x,m,c) linear(x,2,4)` returns `2*x+4`</span></span> |
| <span data-ttu-id="344f8-552">ln</span><span class="sxs-lookup"><span data-stu-id="344f8-552">ln</span></span> |<span data-ttu-id="344f8-553">Vrátí přirozené protokol zadanou funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-553">Returns the natural log of the specified function.</span></span> |`ln(x)` |
| <span data-ttu-id="344f8-554">Protokolu</span><span class="sxs-lookup"><span data-stu-id="344f8-554">log</span></span> |<span data-ttu-id="344f8-555">Vrátí protokol základní 10 zadanou funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-555">Returns the log base 10 of the specified function.</span></span> |`log(x)   log(sum(x,100))` |
| <span data-ttu-id="344f8-556">mapy</span><span class="sxs-lookup"><span data-stu-id="344f8-556">map</span></span> |<span data-ttu-id="344f8-557">Mapuje hodnoty vstupní funkce x, které spadají do min a max, včetně k zadané cílové.</span><span class="sxs-lookup"><span data-stu-id="344f8-557">Maps any values of an input function x that fall within min and max, inclusive to the specified target.</span></span> <span data-ttu-id="344f8-558">Argumenty min a max musí být konstanty.</span><span class="sxs-lookup"><span data-stu-id="344f8-558">The arguments min and max must be constants.</span></span> <span data-ttu-id="344f8-559">Argumenty cíle a výchozí může být konstanty nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-559">The arguments target and default can be constants or functions.</span></span> <span data-ttu-id="344f8-560">Pokud hodnota x nespadá mezi min a max, poté je vrácena hodnota x nebo výchozí hodnota je vrácena v případě, že zadaný jako 5. argument.</span><span class="sxs-lookup"><span data-stu-id="344f8-560">If the value of x does not fall between min and max, then either the value of x is returned, or a default value is returned if specified as a 5th argument.</span></span> |<span data-ttu-id="344f8-561">`map(x,min,max,target) map(x,0,0,1)`Změní všechny hodnoty 0 až 1.</span><span class="sxs-lookup"><span data-stu-id="344f8-561">`map(x,min,max,target) map(x,0,0,1)` Changes any values of 0 to 1.</span></span> <span data-ttu-id="344f8-562">To může být užitečné při zpracování 0 výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="344f8-562">This can be useful in handling default 0 values.</span></span><br> <span data-ttu-id="344f8-563">`map(x,min,max,target,default)    map(x,0,100,1,-1)`Změní hodnoty mezi 0 a 100 až 1 a všechny ostatní hodnoty na hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="344f8-563">`map(x,min,max,target,default)    map(x,0,100,1,-1)` Changes any values between 0 and 100 to 1, and all other values to -1.</span></span><br>  <span data-ttu-id="344f8-564">`map(x,0,100,sum(x,599),docfreq(text,solr))`Změny žádné hodnoty mezi 0 a 100 x + 599 a všechny ostatní hodnoty frekvence podmínek, solr, v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="344f8-564">`map(x,0,100,sum(x,599),docfreq(text,solr))` Changes any values between 0 and 100 to x+599, and all other values to frequency of the term 'solr' in the field text.</span></span> |
| <span data-ttu-id="344f8-565">maximální počet</span><span class="sxs-lookup"><span data-stu-id="344f8-565">max</span></span> |<span data-ttu-id="344f8-566">Vrací maximální hodnotu číselného více vnořené funkce nebo konstanty, které jsou zadané jako argumenty: `max(x,y,...)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-566">Returns the maximum numeric value of multiple nested functions or constants, which are specified as arguments: `max(x,y,...)`.</span></span> <span data-ttu-id="344f8-567">Maximální funkce může být také užitečná pro "bottoming out" jinou funkci nebo pole v některé zadaný konstanta.</span><span class="sxs-lookup"><span data-stu-id="344f8-567">The max function can also be useful for "bottoming out" another function or field at some specified constant.</span></span>  <span data-ttu-id="344f8-568">Použití `field(myfield,max)` syntaxe pro výběr maximální hodnota, která jediné pole s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="344f8-568">Use the `field(myfield,max)` syntax for selecting the maximum value of a single multivalued field.</span></span> |`max(myfield,myotherfield,0)` |
| <span data-ttu-id="344f8-569">min</span><span class="sxs-lookup"><span data-stu-id="344f8-569">min</span></span> |<span data-ttu-id="344f8-570">Vrací minimální hodnotu číselného více vnořené funkce konstanty, které jsou zadané jako argumenty: `min(x,y,...)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-570">Returns the minimum numeric value of multiple nested functions of constants, which are specified as arguments: `min(x,y,...)`.</span></span> <span data-ttu-id="344f8-571">Funkce min, také může být užitečné pro zajištění "horní mez" na funkce pomocí konstanta.</span><span class="sxs-lookup"><span data-stu-id="344f8-571">The min function can also be useful for providing an "upper bound" on a function using a constant.</span></span> <span data-ttu-id="344f8-572">Použití `field(myfield,min)` syntaxe pro výběr minimální hodnota jednoho pole s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="344f8-572">Use the `field(myfield,min)` syntax for selecting the minimum value of a single multivalued field.</span></span> |`min(myfield,myotherfield,0)` |
| <span data-ttu-id="344f8-573">MOD</span><span class="sxs-lookup"><span data-stu-id="344f8-573">mod</span></span> |<span data-ttu-id="344f8-574">Vypočítá zbytek z funkce x, y funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-574">Computes the modulus of the function x by the function y.</span></span> |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| <span data-ttu-id="344f8-575">MS</span><span class="sxs-lookup"><span data-stu-id="344f8-575">ms</span></span> |<span data-ttu-id="344f8-576">Vrátí rozdíl mezi její argumenty v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="344f8-576">Returns milliseconds of difference between its arguments.</span></span> <span data-ttu-id="344f8-577">Data jsou relativní vzhledem k se systémy Unix a POSIX epoch čas, půlnoc, 1. ledna 1970 UTC.</span><span class="sxs-lookup"><span data-stu-id="344f8-577">Dates are relative to the Unix or POSIX time epoch, midnight, January 1, 1970 UTC.</span></span> <span data-ttu-id="344f8-578">Argumenty může být název indexované TrieDateField, nebo na základě konstantní data matematické datum nebo teď.</span><span class="sxs-lookup"><span data-stu-id="344f8-578">Arguments may be the name of an indexed TrieDateField, or date math based on a constant date or NOW .</span></span> <span data-ttu-id="344f8-579">`ms()`je ekvivalentní `ms(NOW)`, počet milisekund od epoch.</span><span class="sxs-lookup"><span data-stu-id="344f8-579">`ms()` is equivalent to `ms(NOW)`, number of milliseconds since the epoch.</span></span> <span data-ttu-id="344f8-580">`ms(a)`Vrátí počet milisekund od epoch, který představuje argument.</span><span class="sxs-lookup"><span data-stu-id="344f8-580">`ms(a)` returns the number of milliseconds since the epoch that the argument represents.</span></span> <span data-ttu-id="344f8-581">`ms(a,b)`Vrátí počet milisekund, po tomto b dojde před a, což je `a - b`.</span><span class="sxs-lookup"><span data-stu-id="344f8-581">`ms(a,b)` returns the number of milliseconds that b occurs before a, which is `a - b`.</span></span> |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| <span data-ttu-id="344f8-582">není</span><span class="sxs-lookup"><span data-stu-id="344f8-582">not</span></span> |<span data-ttu-id="344f8-583">Logicky posunut hodnota zabalená funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-583">The logically negated value of the wrapped function.</span></span> |<span data-ttu-id="344f8-584">`not(exists(author))`Hodnota TRUE, pouze pokud `exists(author)` je false.</span><span class="sxs-lookup"><span data-stu-id="344f8-584">`not(exists(author))` TRUE only when `exists(author)` is false.</span></span> |
| <span data-ttu-id="344f8-585">nebo</span><span class="sxs-lookup"><span data-stu-id="344f8-585">or</span></span> |<span data-ttu-id="344f8-586">Logická disjunkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-586">A logical disjunction.</span></span> |<span data-ttu-id="344f8-587">`or(value1,value2)`Hodnota TRUE, pokud buď value1 nebo value2 platí.</span><span class="sxs-lookup"><span data-stu-id="344f8-587">`or(value1,value2)` TRUE if either value1 or value2 is true.</span></span> |
| <span data-ttu-id="344f8-588">Pow</span><span class="sxs-lookup"><span data-stu-id="344f8-588">pow</span></span> |<span data-ttu-id="344f8-589">Vyvolá určenou mocninu o zadaném základu.</span><span class="sxs-lookup"><span data-stu-id="344f8-589">Raises the specified base to the specified power.</span></span> <span data-ttu-id="344f8-590">`pow(x,y)`Vyvolá x exponentem y.</span><span class="sxs-lookup"><span data-stu-id="344f8-590">`pow(x,y)` raises x to the power of y.</span></span> |`pow(x,y)`<br>`pow(x,log(y))`<br><span data-ttu-id="344f8-591">`pow(x,0.5)`Stejné jako sqrt.</span><span class="sxs-lookup"><span data-stu-id="344f8-591">`pow(x,0.5)` The same as sqrt.</span></span> |
| <span data-ttu-id="344f8-592">Produktu</span><span class="sxs-lookup"><span data-stu-id="344f8-592">product</span></span> |<span data-ttu-id="344f8-593">Vrátí součin více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami.</span><span class="sxs-lookup"><span data-stu-id="344f8-593">Returns the product of multiple values or functions, which are specified in a comma-separated list.</span></span> <span data-ttu-id="344f8-594">`mul(...)`může také použít jako alias pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-594">`mul(...)` may also be used as an alias for this function.</span></span> |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| <span data-ttu-id="344f8-595">recip</span><span class="sxs-lookup"><span data-stu-id="344f8-595">recip</span></span> |<span data-ttu-id="344f8-596">Provede vzájemných funkce s `recip(x,m,a,b)` implementace `a/(m*x+b)`, kde m, a, b jsou konstanty a x je žádné libovolně komplexní funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-596">Performs a reciprocal function with `recip(x,m,a,b)` implementing `a/(m*x+b)`, where m, a,and b are constants, and x is any arbitrarily complex function.</span></span> <span data-ttu-id="344f8-597">Když a b stejné a je x > = 0, tato funkce má maximální hodnota je 1, který zahodí jako x zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="344f8-597">When a and b are equal, and x>=0, this function has a maximum value of 1 that drops as x increases.</span></span> <span data-ttu-id="344f8-598">Zvýšení hodnoty z a b společně výsledky v Přesun celého funkce plošší část křivky.</span><span class="sxs-lookup"><span data-stu-id="344f8-598">Increasing the value of a and b together results in a movement of the entire function to a flatter part of the curve.</span></span> <span data-ttu-id="344f8-599">Tyto vlastnosti můžete vytvořit ideální funkce pro zvyšovat skóre novější dokumenty, když je x `rord(datefield)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-599">These properties can make this an ideal function for boosting more recent documents when x is `rord(datefield)`.</span></span> |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| <span data-ttu-id="344f8-600">rad</span><span class="sxs-lookup"><span data-stu-id="344f8-600">rad</span></span> |<span data-ttu-id="344f8-601">Převede radiánech stupňů.</span><span class="sxs-lookup"><span data-stu-id="344f8-601">Converts degrees to radians.</span></span> |`rad(x)` |
| <span data-ttu-id="344f8-602">Tisknout</span><span class="sxs-lookup"><span data-stu-id="344f8-602">rint</span></span> |<span data-ttu-id="344f8-603">Zaokrouhlí číslo na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-603">Rounds to the nearest integer.</span></span> |`rint(x)`  <br> <span data-ttu-id="344f8-604">`rint(5.6)`Vrátí hodnotu 6</span><span class="sxs-lookup"><span data-stu-id="344f8-604">`rint(5.6)` Returns 6</span></span> |
| <span data-ttu-id="344f8-605">Sin</span><span class="sxs-lookup"><span data-stu-id="344f8-605">sin</span></span> |<span data-ttu-id="344f8-606">Vrátí sinus úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-606">Returns sine of an angle.</span></span> |`sin(x)` |
| <span data-ttu-id="344f8-607">SINH</span><span class="sxs-lookup"><span data-stu-id="344f8-607">sinh</span></span> |<span data-ttu-id="344f8-608">Vrací hyperbolický sinus úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-608">Returns hyperbolic sine of an angle.</span></span> |`sinh(x)` |
| <span data-ttu-id="344f8-609">Škálování</span><span class="sxs-lookup"><span data-stu-id="344f8-609">scale</span></span> |<span data-ttu-id="344f8-610">Udává hodnoty funkce x tak, aby se patří mezi zadaný minTarget a maxTarget (včetně).</span><span class="sxs-lookup"><span data-stu-id="344f8-610">Scales values of the function x, such that they fall between the specified minTarget and maxTarget inclusive.</span></span> <span data-ttu-id="344f8-611">Aktuální implementace prochází všechny hodnoty funkce získat min a max, takže ho můžete vybrat správné škálování.</span><span class="sxs-lookup"><span data-stu-id="344f8-611">The current implementation traverses all of the function values to obtain the min and max, so it can pick the correct scale.</span></span> <span data-ttu-id="344f8-612">Aktuální implementace nerozlišuje po odstranění dokumentů, nebo dokumenty, které nemají žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="344f8-612">The current implementation cannot distinguish when documents have been deleted, or documents that have no value.</span></span> <span data-ttu-id="344f8-613">Použije 0.0 hodnoty pro tyto případy:</span><span class="sxs-lookup"><span data-stu-id="344f8-613">It uses 0.0 values for these cases.</span></span> <span data-ttu-id="344f8-614">To znamená, že pokud jsou obvykle všechny větší hodnotu než 0,0 hodnoty, jeden můžete nadále binárními 0,0 jako minimální hodnota k namapování z.</span><span class="sxs-lookup"><span data-stu-id="344f8-614">This means that if values are normally all greater than 0.0, one can still end up with 0.0 as the min value to map from.</span></span> <span data-ttu-id="344f8-615">V těchto případech odpovídající `map()` funkce může jako alternativní řešení Chcete-li změnit 0,0 na hodnotu v rozsahu skutečné, jak je vidět tady:`scale(map(x,0,0,5),1,2)`</span><span class="sxs-lookup"><span data-stu-id="344f8-615">In these cases, an appropriate `map()` function could be used as a workaround to change 0.0 to a value in the real range, as shown here: `scale(map(x,0,0,5),1,2)`</span></span> |`scale(x,minTarget,maxTarget)`<br><span data-ttu-id="344f8-616">`scale(x,1,2)`Hodnoty x, škáluje tak, aby všechny hodnoty jsou mezi 1 a 2 (včetně).</span><span class="sxs-lookup"><span data-stu-id="344f8-616">`scale(x,1,2)` Scales the values of x, such that all values are between 1 and 2 inclusive.</span></span> |
| <span data-ttu-id="344f8-617">Sqrt</span><span class="sxs-lookup"><span data-stu-id="344f8-617">sqrt</span></span> |<span data-ttu-id="344f8-618">Vrátí druhou odmocninu čísla se zadanou hodnotou nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="344f8-618">Returns the square root of the specified value or function.</span></span> |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| <span data-ttu-id="344f8-619">strdist</span><span class="sxs-lookup"><span data-stu-id="344f8-619">strdist</span></span> |<span data-ttu-id="344f8-620">Vypočítá vzdálenost mezi dva řetězce.</span><span class="sxs-lookup"><span data-stu-id="344f8-620">Calculates the distance between two strings.</span></span> <span data-ttu-id="344f8-621">Používá rozhraní Lucene kontrolu pravopisu StringDistance a podporuje všechny implementace, které jsou k dispozici v tomto balíčku.</span><span class="sxs-lookup"><span data-stu-id="344f8-621">Uses the Lucene spell checker StringDistance interface, and supports all of the implementations available in that package.</span></span> <span data-ttu-id="344f8-622">Také umožňuje aplikacím zařadit vlastní, prostřednictvím prostředků na Solr možnosti načítání.</span><span class="sxs-lookup"><span data-stu-id="344f8-622">Also allows applications to plug in their own, via Solr's resource loading capabilities.</span></span> <span data-ttu-id="344f8-623">provede strdist `(string1, string2, distance measure)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-623">strdist takes `(string1, string2, distance measure)`.</span></span> <span data-ttu-id="344f8-624">Možné hodnoty pro míru vzdálenost jsou:</span><span class="sxs-lookup"><span data-stu-id="344f8-624">Possible values for distance measure are:</span></span><ul><li><span data-ttu-id="344f8-625">jw: Jaro Winkler</span><span class="sxs-lookup"><span data-stu-id="344f8-625">jw: Jaro-Winkler</span></span></li><li><span data-ttu-id="344f8-626">Upravit: Levenstein nebo upravit vzdálenost</span><span class="sxs-lookup"><span data-stu-id="344f8-626">edit: Levenstein or Edit distance</span></span></li><li><span data-ttu-id="344f8-627">ngram: The NGramDistance-li zadána, můžete volitelně předávat velikost ngram příliš.</span><span class="sxs-lookup"><span data-stu-id="344f8-627">ngram: The NGramDistance, if specified, can optionally pass in the ngram size too.</span></span> <span data-ttu-id="344f8-628">Výchozí hodnota je 2.</span><span class="sxs-lookup"><span data-stu-id="344f8-628">Default is 2.</span></span></li><li><span data-ttu-id="344f8-629">FQN: Plně kvalifikovaný název pro implementaci rozhraní StringDistance třídy.</span><span class="sxs-lookup"><span data-stu-id="344f8-629">FQN: Fully Qualified class Name for an implementation of the StringDistance interface.</span></span> <span data-ttu-id="344f8-630">Musí mít konstruktor bez arg.</span><span class="sxs-lookup"><span data-stu-id="344f8-630">Must have a no-arg constructor.</span></span></li></ul> |`strdist("SOLR",id,edit)` |
| <span data-ttu-id="344f8-631">Sub –</span><span class="sxs-lookup"><span data-stu-id="344f8-631">sub</span></span> |<span data-ttu-id="344f8-632">Vrátí x-y z `sub(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="344f8-632">Returns x-y from `sub(x,y)`.</span></span> |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| <span data-ttu-id="344f8-633">Součet</span><span class="sxs-lookup"><span data-stu-id="344f8-633">sum</span></span> |<span data-ttu-id="344f8-634">Vrátí součet více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami.</span><span class="sxs-lookup"><span data-stu-id="344f8-634">Returns the sum of multiple values or functions, which are specified in a comma-separated list.</span></span> <span data-ttu-id="344f8-635">`add(...)`může se použít jako alias pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="344f8-635">`add(...)` may be used as an alias for this function.</span></span> |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| <span data-ttu-id="344f8-636">termfreq</span><span class="sxs-lookup"><span data-stu-id="344f8-636">termfreq</span></span> |<span data-ttu-id="344f8-637">Vrátí počet termín se zobrazí v poli pro tento dokument.</span><span class="sxs-lookup"><span data-stu-id="344f8-637">Returns the number of times the term appears in the field for that document.</span></span> |<span data-ttu-id="344f8-638">termfreq(text,'memory')</span><span class="sxs-lookup"><span data-stu-id="344f8-638">termfreq(text,'memory')</span></span> |
| <span data-ttu-id="344f8-639">Tan</span><span class="sxs-lookup"><span data-stu-id="344f8-639">tan</span></span> |<span data-ttu-id="344f8-640">Vrátí tangens úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-640">Returns tangent of an angle.</span></span> |`tan(x)` |
| <span data-ttu-id="344f8-641">TANH</span><span class="sxs-lookup"><span data-stu-id="344f8-641">tanh</span></span> |<span data-ttu-id="344f8-642">Vrací hyperbolický tangens úhlu.</span><span class="sxs-lookup"><span data-stu-id="344f8-642">Returns hyperbolic tangent of an angle.</span></span> |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a><span data-ttu-id="344f8-643">Odkaz na pole a omezující vlastnost vyhledávání</span><span class="sxs-lookup"><span data-stu-id="344f8-643">Search field and facet reference</span></span>
<span data-ttu-id="344f8-644">Při použití hledání protokolů hledání dat, zobrazí výsledky různé pole a omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="344f8-644">When you use Log Search to find data, results display various field and facets.</span></span> <span data-ttu-id="344f8-645">Některé z informací se nemusí zobrazit velmi popisný.</span><span class="sxs-lookup"><span data-stu-id="344f8-645">Some of the information might not appear very descriptive.</span></span> <span data-ttu-id="344f8-646">Použijte následující informace vám pomohou pochopit výsledky.</span><span class="sxs-lookup"><span data-stu-id="344f8-646">Use the following information to help you understand the results.</span></span>

| <span data-ttu-id="344f8-647">Pole</span><span class="sxs-lookup"><span data-stu-id="344f8-647">Field</span></span> | <span data-ttu-id="344f8-648">Typ vyhledávání</span><span class="sxs-lookup"><span data-stu-id="344f8-648">Search type</span></span> | <span data-ttu-id="344f8-649">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-649">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="344f8-650">TenantId</span><span class="sxs-lookup"><span data-stu-id="344f8-650">TenantId</span></span> |<span data-ttu-id="344f8-651">Všechny</span><span class="sxs-lookup"><span data-stu-id="344f8-651">All</span></span> |<span data-ttu-id="344f8-652">Používá se k oddílu dat.</span><span class="sxs-lookup"><span data-stu-id="344f8-652">Used to partition data.</span></span> |
| <span data-ttu-id="344f8-653">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="344f8-653">TimeGenerated</span></span> |<span data-ttu-id="344f8-654">Všechny</span><span class="sxs-lookup"><span data-stu-id="344f8-654">All</span></span> |<span data-ttu-id="344f8-655">Používá k řízení časové ose timeselectors (ve vyhledávání a na dalších obrazovkách).</span><span class="sxs-lookup"><span data-stu-id="344f8-655">Used to drive the timeline, timeselectors (in search and in other screens).</span></span> <span data-ttu-id="344f8-656">Reprezentuje při generování část dat (obvykle na agenta).</span><span class="sxs-lookup"><span data-stu-id="344f8-656">It represents when the piece of data was generated (typically on the agent).</span></span> <span data-ttu-id="344f8-657">Čas je vyjádřen ve formátu ISO a je vždycky UTC.</span><span class="sxs-lookup"><span data-stu-id="344f8-657">The time is expressed in ISO format, and is always UTC.</span></span> <span data-ttu-id="344f8-658">U typů, které jsou založeny na existující instrumentace (to znamená, události v protokolu) je to obvykle reálném čase, který položka/řádku/záznam protokolu byla zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="344f8-658">In the case of types that are based on existing instrumentation (that is, events in a log), this is typically the real time that the log entry/line/record was logged.</span></span> <span data-ttu-id="344f8-659">Pro některé typy, které vytváří prostřednictvím sad management Pack nebo v cloudu (například doporučení nebo výstrahy) představuje dobu něco jiného.</span><span class="sxs-lookup"><span data-stu-id="344f8-659">For some of the other types that are produced either via management packs or in the cloud (for example, recommendations or alerts), the time represents something different.</span></span> <span data-ttu-id="344f8-660">Toto je čas tato nová data se snímkem konfigurací nějaká nebyla shromážděna, nebo bylo vytvořeno ho podle doporučení nebo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="344f8-660">This is the time when this new piece of data with a snapshot of a configuration of some sort was collected, or a recommendation/alert was produced based on it.</span></span> |
| <span data-ttu-id="344f8-661">ID události</span><span class="sxs-lookup"><span data-stu-id="344f8-661">EventID</span></span> |<span data-ttu-id="344f8-662">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-662">Event</span></span> |<span data-ttu-id="344f8-663">ID události v protokolu událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-663">EventID in the Windows event log.</span></span> |
| <span data-ttu-id="344f8-664">Protokol událostí</span><span class="sxs-lookup"><span data-stu-id="344f8-664">EventLog</span></span> |<span data-ttu-id="344f8-665">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-665">Event</span></span> |<span data-ttu-id="344f8-666">Protokol událostí, kde byla události zaznamenané v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-666">Event log where the event was logged by Windows.</span></span> |
| <span data-ttu-id="344f8-667">EventLevelName</span><span class="sxs-lookup"><span data-stu-id="344f8-667">EventLevelName</span></span> |<span data-ttu-id="344f8-668">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-668">Event</span></span> |<span data-ttu-id="344f8-669">Kritický nebo upozornění nebo informace nebo úspěch</span><span class="sxs-lookup"><span data-stu-id="344f8-669">Critical/warning/information/success</span></span> |
| <span data-ttu-id="344f8-670">eventLevel</span><span class="sxs-lookup"><span data-stu-id="344f8-670">EventLevel</span></span> |<span data-ttu-id="344f8-671">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-671">Event</span></span> |<span data-ttu-id="344f8-672">Číselnou hodnotu pro kritický nebo upozornění nebo informace nebo úspěch (použijte EventLevelName místo pro snazší/srozumitelnější dotazy).</span><span class="sxs-lookup"><span data-stu-id="344f8-672">Numerical value for critical/warning/information/success (use EventLevelName instead for easier/more readable queries).</span></span> |
| <span data-ttu-id="344f8-673">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="344f8-673">SourceSystem</span></span> |<span data-ttu-id="344f8-674">Všechny</span><span class="sxs-lookup"><span data-stu-id="344f8-674">All</span></span> |<span data-ttu-id="344f8-675">Kde data pocházejí z (z hlediska připojení režimu do služby).</span><span class="sxs-lookup"><span data-stu-id="344f8-675">Where the data comes from (in terms of attach mode to the service).</span></span> <span data-ttu-id="344f8-676">Mezi příklady patří Microsoft System Center Operations Manager a Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="344f8-676">Examples include Microsoft System Center Operations Manager and Azure Storage.</span></span> |
| <span data-ttu-id="344f8-677">Název objektu</span><span class="sxs-lookup"><span data-stu-id="344f8-677">ObjectName</span></span> |<span data-ttu-id="344f8-678">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-678">PerfHourly</span></span> |<span data-ttu-id="344f8-679">Název objektu výkonu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-679">Windows performance object name.</span></span> |
| <span data-ttu-id="344f8-680">InstanceName</span><span class="sxs-lookup"><span data-stu-id="344f8-680">InstanceName</span></span> |<span data-ttu-id="344f8-681">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-681">PerfHourly</span></span> |<span data-ttu-id="344f8-682">Název instance čítače výkonu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-682">Windows performance counter instance name.</span></span> |
| <span data-ttu-id="344f8-683">CounteName</span><span class="sxs-lookup"><span data-stu-id="344f8-683">CounteName</span></span> |<span data-ttu-id="344f8-684">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-684">PerfHourly</span></span> |<span data-ttu-id="344f8-685">Název čítače výkonu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-685">Windows performance counter name.</span></span> |
| <span data-ttu-id="344f8-686">ObjectDisplayName</span><span class="sxs-lookup"><span data-stu-id="344f8-686">ObjectDisplayName</span></span> |<span data-ttu-id="344f8-687">PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject,</span><span class="sxs-lookup"><span data-stu-id="344f8-687">PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty</span></span> |<span data-ttu-id="344f8-688">Zobrazovaný název objektu cílem pravidlo kolekce výkonu v nástroji Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="344f8-688">Display name of the object targeted by a performance collection rule in Operations Manager.</span></span> <span data-ttu-id="344f8-689">Mohou být také zobrazovaný název objektu zjištěné Operational Insights, nebo pro který byla výstraha vygenerována.</span><span class="sxs-lookup"><span data-stu-id="344f8-689">Could also be the display name of the object discovered by Operational Insights, or against which the alert was generated.</span></span> |
| <span data-ttu-id="344f8-690">RootObjectName</span><span class="sxs-lookup"><span data-stu-id="344f8-690">RootObjectName</span></span> |<span data-ttu-id="344f8-691">PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject,</span><span class="sxs-lookup"><span data-stu-id="344f8-691">PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty</span></span> |<span data-ttu-id="344f8-692">Zobrazovaný název nadřazeného člena nadřazeného (v dvojité hostitelský vztah) objekt cílem pravidlo kolekce výkonu v nástroji Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="344f8-692">Display name of the parent of the parent (in a double hosting relationship) of the object targeted by a performance collection rule in Operations Manager.</span></span> <span data-ttu-id="344f8-693">Mohou být také zobrazovaný název objektu zjištěné Operational Insights, nebo pro který byla výstraha vygenerována.</span><span class="sxs-lookup"><span data-stu-id="344f8-693">Could also be the display name of the object discovered by Operational Insights, or against which the alert was generated.</span></span> |
| <span data-ttu-id="344f8-694">Počítač</span><span class="sxs-lookup"><span data-stu-id="344f8-694">Computer</span></span> |<span data-ttu-id="344f8-695">Většina typů</span><span class="sxs-lookup"><span data-stu-id="344f8-695">Most types</span></span> |<span data-ttu-id="344f8-696">Název počítače, které patří data.</span><span class="sxs-lookup"><span data-stu-id="344f8-696">Computer name that the data belongs to.</span></span> |
| <span data-ttu-id="344f8-697">Název zařízení</span><span class="sxs-lookup"><span data-stu-id="344f8-697">DeviceName</span></span> |<span data-ttu-id="344f8-698">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-698">ProtectionStatus</span></span> |<span data-ttu-id="344f8-699">Název počítače dat patří do (stejný jako "Počítač").</span><span class="sxs-lookup"><span data-stu-id="344f8-699">Computer name the data belongs to (same as "Computer").</span></span> |
| <span data-ttu-id="344f8-700">DetectionId</span><span class="sxs-lookup"><span data-stu-id="344f8-700">DetectionId</span></span> |<span data-ttu-id="344f8-701">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-701">ProtectionStatus</span></span> | |
| <span data-ttu-id="344f8-702">ThreatStatusRank</span><span class="sxs-lookup"><span data-stu-id="344f8-702">ThreatStatusRank</span></span> |<span data-ttu-id="344f8-703">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-703">ProtectionStatus</span></span> |<span data-ttu-id="344f8-704">Pořadí stav Threat je číselné vyjádření stavu hrozeb.</span><span class="sxs-lookup"><span data-stu-id="344f8-704">Threat status rank is a numerical representation of the threat status.</span></span> <span data-ttu-id="344f8-705">Podobně jako u kódy odpovědí protokolu HTTP, má pořadí mezery mezi zadanými čísly (proto žádné hrozeb je 150 a není 100 nebo 0), a místo pro přidání nové stavy.</span><span class="sxs-lookup"><span data-stu-id="344f8-705">Similar to HTTP response codes, the ranking has gaps between the numbers (which is why no threats is 150 and not 100 or 0), leaving room to add new states.</span></span> <span data-ttu-id="344f8-706">Pro souhrn stavu hrozeb a stav ochrany je záměrem Zobrazit nejhorší stav, který byl počítač v průběhu časového období.</span><span class="sxs-lookup"><span data-stu-id="344f8-706">For a rollup of threat status and protection status, the intention is to show the worst state that the computer has been in during the selected time period.</span></span> <span data-ttu-id="344f8-707">Čísla rank různé stavy, takže můžete vyhledat záznam s nejvyšší číslo.</span><span class="sxs-lookup"><span data-stu-id="344f8-707">The numbers rank the different states, so you can look for the record with the highest number.</span></span> |
| <span data-ttu-id="344f8-708">ThreatStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-708">ThreatStatus</span></span> |<span data-ttu-id="344f8-709">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-709">ProtectionStatus</span></span> |<span data-ttu-id="344f8-710">Popis ThreatStatus, mapování 1:1 s ThreatStatusRank.</span><span class="sxs-lookup"><span data-stu-id="344f8-710">Description of ThreatStatus, maps 1:1 with ThreatStatusRank.</span></span> |
| <span data-ttu-id="344f8-711">TypeofProtection</span><span class="sxs-lookup"><span data-stu-id="344f8-711">TypeofProtection</span></span> |<span data-ttu-id="344f8-712">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-712">ProtectionStatus</span></span> |<span data-ttu-id="344f8-713">Antimalwarových produktu, který je zjištěn v počítači: none, nástroje pro odebrání Microsoft Malware, Forefront a tak dále.</span><span class="sxs-lookup"><span data-stu-id="344f8-713">Antimalware product that is detected in the computer: none, Microsoft Malware Removal tool, Forefront, and so on.</span></span> |
| <span data-ttu-id="344f8-714">ScanDate</span><span class="sxs-lookup"><span data-stu-id="344f8-714">ScanDate</span></span> |<span data-ttu-id="344f8-715">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-715">ProtectionStatus</span></span> | |
| <span data-ttu-id="344f8-716">SourceHealthServiceId</span><span class="sxs-lookup"><span data-stu-id="344f8-716">SourceHealthServiceId</span></span> |<span data-ttu-id="344f8-717">ProtectionStatus RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-717">ProtectionStatus, RequiredUpdate</span></span> |<span data-ttu-id="344f8-718">ID služby stavu pro tento počítač agenta.</span><span class="sxs-lookup"><span data-stu-id="344f8-718">HealthService ID for this computer's agent.</span></span> |
| <span data-ttu-id="344f8-719">HealthServiceId</span><span class="sxs-lookup"><span data-stu-id="344f8-719">HealthServiceId</span></span> |<span data-ttu-id="344f8-720">Většina typů</span><span class="sxs-lookup"><span data-stu-id="344f8-720">Most types</span></span> |<span data-ttu-id="344f8-721">ID služby stavu pro tento počítač agenta.</span><span class="sxs-lookup"><span data-stu-id="344f8-721">HealthService ID for this computer's agent.</span></span> |
| <span data-ttu-id="344f8-722">ManagementGroupName</span><span class="sxs-lookup"><span data-stu-id="344f8-722">ManagementGroupName</span></span> |<span data-ttu-id="344f8-723">Většina typů</span><span class="sxs-lookup"><span data-stu-id="344f8-723">Most types</span></span> |<span data-ttu-id="344f8-724">Název skupiny pro správu pro agenty nástroje Operations Manager připojit.</span><span class="sxs-lookup"><span data-stu-id="344f8-724">Management Group Name for Operations Manager-attached agents.</span></span> <span data-ttu-id="344f8-725">Jinak má hodnotu null nebo prázdný.</span><span class="sxs-lookup"><span data-stu-id="344f8-725">Otherwise, it is null/blank.</span></span> |
| <span data-ttu-id="344f8-726">objectType</span><span class="sxs-lookup"><span data-stu-id="344f8-726">ObjectType</span></span> |<span data-ttu-id="344f8-727">ConfigurationObject</span><span class="sxs-lookup"><span data-stu-id="344f8-727">ConfigurationObject</span></span> |<span data-ttu-id="344f8-728">Zadejte pro tento objekt zjištěný assessment konfigurace analýzy protokolů (jako sada management pack Operations Manager typu nebo třídy).</span><span class="sxs-lookup"><span data-stu-id="344f8-728">Type (as in Operations Manager management pack's type/class) for this object, discovered by Log Analytics configuration assessment.</span></span> |
| <span data-ttu-id="344f8-729">UpdateTitle</span><span class="sxs-lookup"><span data-stu-id="344f8-729">UpdateTitle</span></span> |<span data-ttu-id="344f8-730">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-730">RequiredUpdate</span></span> |<span data-ttu-id="344f8-731">Název aktualizace, která byla nalezena není nainstalován.</span><span class="sxs-lookup"><span data-stu-id="344f8-731">Name of the update that was found not installed.</span></span> |
| <span data-ttu-id="344f8-732">PublishDate</span><span class="sxs-lookup"><span data-stu-id="344f8-732">PublishDate</span></span> |<span data-ttu-id="344f8-733">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-733">RequiredUpdate</span></span> |<span data-ttu-id="344f8-734">Pokud byla aktualizace publikována ve službě Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="344f8-734">When the update was published on Microsoft Update.</span></span> |
| <span data-ttu-id="344f8-735">Server</span><span class="sxs-lookup"><span data-stu-id="344f8-735">Server</span></span> |<span data-ttu-id="344f8-736">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-736">RequiredUpdate</span></span> |<span data-ttu-id="344f8-737">Název počítače dat patří do (stejný jako "Počítač").</span><span class="sxs-lookup"><span data-stu-id="344f8-737">Computer name the data belongs to (same as "Computer").</span></span> |
| <span data-ttu-id="344f8-738">Produkt</span><span class="sxs-lookup"><span data-stu-id="344f8-738">Product</span></span> |<span data-ttu-id="344f8-739">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-739">RequiredUpdate</span></span> |<span data-ttu-id="344f8-740">Produkt, který se aktualizace vztahuje.</span><span class="sxs-lookup"><span data-stu-id="344f8-740">Product that the update applies to.</span></span> |
| <span data-ttu-id="344f8-741">UpdateClassification</span><span class="sxs-lookup"><span data-stu-id="344f8-741">UpdateClassification</span></span> |<span data-ttu-id="344f8-742">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-742">RequiredUpdate</span></span> |<span data-ttu-id="344f8-743">Typ aktualizace (například kumulativní aktualizace nebo service pack).</span><span class="sxs-lookup"><span data-stu-id="344f8-743">Type of update (for example, update rollup or service pack).</span></span> |
| <span data-ttu-id="344f8-744">KBID</span><span class="sxs-lookup"><span data-stu-id="344f8-744">KBID</span></span> |<span data-ttu-id="344f8-745">RequiredUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-745">RequiredUpdate</span></span> |<span data-ttu-id="344f8-746">ID článku KB, který popisuje tento osvědčený postup nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="344f8-746">KB article ID that describes this best practice or update.</span></span> |
| <span data-ttu-id="344f8-747">WorkflowName</span><span class="sxs-lookup"><span data-stu-id="344f8-747">WorkflowName</span></span> |<span data-ttu-id="344f8-748">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-748">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-749">Název pravidla nebo monitorování, které vytváří výstrahy.</span><span class="sxs-lookup"><span data-stu-id="344f8-749">Name of the rule or monitor that produced the alert.</span></span> |
| <span data-ttu-id="344f8-750">Závažnost</span><span class="sxs-lookup"><span data-stu-id="344f8-750">Severity</span></span> |<span data-ttu-id="344f8-751">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-751">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-752">Závažnost výstrahy.</span><span class="sxs-lookup"><span data-stu-id="344f8-752">Severity of the alert.</span></span> |
| <span data-ttu-id="344f8-753">Priorita</span><span class="sxs-lookup"><span data-stu-id="344f8-753">Priority</span></span> |<span data-ttu-id="344f8-754">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-754">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-755">Priorita výstrahy.</span><span class="sxs-lookup"><span data-stu-id="344f8-755">Priority of the alert.</span></span> |
| <span data-ttu-id="344f8-756">IsMonitorAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-756">IsMonitorAlert</span></span> |<span data-ttu-id="344f8-757">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-757">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-758">Je tato výstraha vygeneruje, monitorování (true) nebo pravidlem (false)?</span><span class="sxs-lookup"><span data-stu-id="344f8-758">Is this alert generated by a monitor (true) or a rule (false)?</span></span> |
| <span data-ttu-id="344f8-759">AlertParameters</span><span class="sxs-lookup"><span data-stu-id="344f8-759">AlertParameters</span></span> |<span data-ttu-id="344f8-760">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-760">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-761">Soubor XML s parametry výstrahy analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="344f8-761">XML with the parameters of the Log Analytics alert.</span></span> |
| <span data-ttu-id="344f8-762">Kontext</span><span class="sxs-lookup"><span data-stu-id="344f8-762">Context</span></span> |<span data-ttu-id="344f8-763">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-763">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-764">Soubor XML s kontextem výstrahy analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="344f8-764">XML with the context of the Log Analytics alert.</span></span> |
| <span data-ttu-id="344f8-765">Úloha</span><span class="sxs-lookup"><span data-stu-id="344f8-765">Workload</span></span> |<span data-ttu-id="344f8-766">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-766">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-767">Technologie nebo úlohy, které výstrahu odkazuje.</span><span class="sxs-lookup"><span data-stu-id="344f8-767">Technology or workload that the alert refers to.</span></span> |
| <span data-ttu-id="344f8-768">AdvisorWorkload</span><span class="sxs-lookup"><span data-stu-id="344f8-768">AdvisorWorkload</span></span> |<span data-ttu-id="344f8-769">Doporučení</span><span class="sxs-lookup"><span data-stu-id="344f8-769">Recommendation</span></span> |<span data-ttu-id="344f8-770">Technologie nebo úlohy, které odkazuje doporučení.</span><span class="sxs-lookup"><span data-stu-id="344f8-770">Technology or workload that the recommendation refers to.</span></span> |
| <span data-ttu-id="344f8-771">Popis</span><span class="sxs-lookup"><span data-stu-id="344f8-771">Description</span></span> |<span data-ttu-id="344f8-772">ConfigurationAlert</span><span class="sxs-lookup"><span data-stu-id="344f8-772">ConfigurationAlert</span></span> |<span data-ttu-id="344f8-773">Popis výstrahy (krátký).</span><span class="sxs-lookup"><span data-stu-id="344f8-773">Alert description (short).</span></span> |
| <span data-ttu-id="344f8-774">DaysSinceLastUpdate</span><span class="sxs-lookup"><span data-stu-id="344f8-774">DaysSinceLastUpdate</span></span> |<span data-ttu-id="344f8-775">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-775">UpdateAgent</span></span> |<span data-ttu-id="344f8-776">Počet dní před (relativní vůči TimeGenerated tento záznam) tohoto agenta nainstalovat v žádné aktualizace ze služby Windows Server Update Service (WSUS) nebo Microsoft Update?</span><span class="sxs-lookup"><span data-stu-id="344f8-776">How many days ago (relative to TimeGenerated of this record) did this agent install any update from Windows Server Update Service (WSUS) or Microsoft Update?</span></span> |
| <span data-ttu-id="344f8-777">DaysSinceLastUpdateBucket</span><span class="sxs-lookup"><span data-stu-id="344f8-777">DaysSinceLastUpdateBucket</span></span> |<span data-ttu-id="344f8-778">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-778">UpdateAgent</span></span> |<span data-ttu-id="344f8-779">Podle DaysSinceLastUpdate kategorizaci v čas kbelíků z dobu počítač poslední nainstalované všechny aktualizace z webu služby WSUS nebo Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="344f8-779">Based on DaysSinceLastUpdate, a categorization in time buckets of how long ago a computer last installed any update from WSUS/Microsoft Update.</span></span> |
| <span data-ttu-id="344f8-780">AutomaticUpdateEnabled</span><span class="sxs-lookup"><span data-stu-id="344f8-780">AutomaticUpdateEnabled</span></span> |<span data-ttu-id="344f8-781">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-781">UpdateAgent</span></span> |<span data-ttu-id="344f8-782">Kontrola automatických aktualizací povolená nebo zakázaná na tento agent?</span><span class="sxs-lookup"><span data-stu-id="344f8-782">Is automatic update checking enabled or disabled on this agent?</span></span> |
| <span data-ttu-id="344f8-783">AutomaticUpdateValue</span><span class="sxs-lookup"><span data-stu-id="344f8-783">AutomaticUpdateValue</span></span> |<span data-ttu-id="344f8-784">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-784">UpdateAgent</span></span> |<span data-ttu-id="344f8-785">Automatické aktualizace kontroluje nastaven na automaticky stahovat a instalovat, stahují jenom nebo jenom zkontrolujte?</span><span class="sxs-lookup"><span data-stu-id="344f8-785">Is automatic update checking set to automatically download and install, only download, or only check?</span></span> |
| <span data-ttu-id="344f8-786">WindowsUpdateAgentVersion</span><span class="sxs-lookup"><span data-stu-id="344f8-786">WindowsUpdateAgentVersion</span></span> |<span data-ttu-id="344f8-787">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-787">UpdateAgent</span></span> |<span data-ttu-id="344f8-788">Číslo verze agenta nástroje Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="344f8-788">Version number of the Microsoft Update agent.</span></span> |
| <span data-ttu-id="344f8-789">WSUSServer</span><span class="sxs-lookup"><span data-stu-id="344f8-789">WSUSServer</span></span> |<span data-ttu-id="344f8-790">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-790">UpdateAgent</span></span> |<span data-ttu-id="344f8-791">Který WSUS server tohoto agenta aktualizace cílí?</span><span class="sxs-lookup"><span data-stu-id="344f8-791">Which WSUS server is this update agent targeting?</span></span> |
| <span data-ttu-id="344f8-792">OSVersion</span><span class="sxs-lookup"><span data-stu-id="344f8-792">OSVersion</span></span> |<span data-ttu-id="344f8-793">UpdateAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-793">UpdateAgent</span></span> |<span data-ttu-id="344f8-794">Verze operačního systému tohoto agenta aktualizace běží na.</span><span class="sxs-lookup"><span data-stu-id="344f8-794">Version of the operating system this update agent is running on.</span></span> |
| <span data-ttu-id="344f8-795">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="344f8-795">Name</span></span> |<span data-ttu-id="344f8-796">Doporučení, ConfigurationObjectProperty</span><span class="sxs-lookup"><span data-stu-id="344f8-796">Recommendation, ConfigurationObjectProperty</span></span> |<span data-ttu-id="344f8-797">Název nebo název doporučení, nebo název vlastnosti z hodnocení konfigurace analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="344f8-797">Name/title of the recommendation, or name of the property from Log Analytics configuration assessment.</span></span> |
| <span data-ttu-id="344f8-798">Hodnota</span><span class="sxs-lookup"><span data-stu-id="344f8-798">Value</span></span> |<span data-ttu-id="344f8-799">ConfigurationObjectProperty</span><span class="sxs-lookup"><span data-stu-id="344f8-799">ConfigurationObjectProperty</span></span> |<span data-ttu-id="344f8-800">Hodnota vlastnosti z hodnocení konfigurace analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="344f8-800">Value of a property from Log Analytics configuration assessment.</span></span> |
| <span data-ttu-id="344f8-801">KBLink</span><span class="sxs-lookup"><span data-stu-id="344f8-801">KBLink</span></span> |<span data-ttu-id="344f8-802">Doporučení</span><span class="sxs-lookup"><span data-stu-id="344f8-802">Recommendation</span></span> |<span data-ttu-id="344f8-803">Adresa URL v článku KB, který popisuje tento osvědčený postup nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="344f8-803">URL to the KB article that describes this best practice or update.</span></span> |
| <span data-ttu-id="344f8-804">RecommendationStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-804">RecommendationStatus</span></span> |<span data-ttu-id="344f8-805">Doporučení</span><span class="sxs-lookup"><span data-stu-id="344f8-805">Recommendation</span></span> |<span data-ttu-id="344f8-806">Doporučení jsou mezi několik typů, jejichž záznamy aktualizovat, ne jenom přidat do indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="344f8-806">Recommendations are among the few types whose records get updated, not just added to the search index.</span></span> <span data-ttu-id="344f8-807">Tento stav se změní, zda doporučení je aktivní/otevřít, nebo pokud analýzy protokolů zjistí, že ho byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="344f8-807">This status changes whether the recommendation is active/open, or if Log Analytics detects that it has been resolved.</span></span> |
| <span data-ttu-id="344f8-808">RenderedDescription</span><span class="sxs-lookup"><span data-stu-id="344f8-808">RenderedDescription</span></span> |<span data-ttu-id="344f8-809">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-809">Event</span></span> |<span data-ttu-id="344f8-810">Vykreslené popis (znovu používaný text se vyplněná parametry) událost systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-810">Rendered description (reused text with populated parameters) of a Windows event.</span></span> |
| <span data-ttu-id="344f8-811">ParameterXml</span><span class="sxs-lookup"><span data-stu-id="344f8-811">ParameterXml</span></span> |<span data-ttu-id="344f8-812">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-812">Event</span></span> |<span data-ttu-id="344f8-813">Soubor XML s parametry v datové části události systému Windows (jak je vidět v prohlížeči událostí).</span><span class="sxs-lookup"><span data-stu-id="344f8-813">XML with the parameters in the data section of a Windows Event (as seen in event viewer).</span></span> |
| <span data-ttu-id="344f8-814">EventData</span><span class="sxs-lookup"><span data-stu-id="344f8-814">EventData</span></span> |<span data-ttu-id="344f8-815">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-815">Event</span></span> |<span data-ttu-id="344f8-816">Soubor XML s celou datová část události systému Windows (jak je vidět v prohlížeči událostí).</span><span class="sxs-lookup"><span data-stu-id="344f8-816">XML with the whole data section of a Windows Event (as seen in event viewer).</span></span> |
| <span data-ttu-id="344f8-817">Zdroj</span><span class="sxs-lookup"><span data-stu-id="344f8-817">Source</span></span> |<span data-ttu-id="344f8-818">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-818">Event</span></span> |<span data-ttu-id="344f8-819">Zdroj protokolu událostí, které vygenerovalo událost.</span><span class="sxs-lookup"><span data-stu-id="344f8-819">Event log source that generated the event.</span></span> |
| <span data-ttu-id="344f8-820">EventCategory</span><span class="sxs-lookup"><span data-stu-id="344f8-820">EventCategory</span></span> |<span data-ttu-id="344f8-821">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-821">Event</span></span> |<span data-ttu-id="344f8-822">Kategorie události přímo z protokolu událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-822">Category of the event, directly from the Windows event log.</span></span> |
| <span data-ttu-id="344f8-823">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="344f8-823">UserName</span></span> |<span data-ttu-id="344f8-824">Událost</span><span class="sxs-lookup"><span data-stu-id="344f8-824">Event</span></span> |<span data-ttu-id="344f8-825">Uživatelské jméno událostí systému Windows (obvykle AUTHORITY\LOCALSYSTEM NT).</span><span class="sxs-lookup"><span data-stu-id="344f8-825">User name of the Windows event (typically, NT AUTHORITY\LOCALSYSTEM).</span></span> |
| <span data-ttu-id="344f8-826">SampleValue</span><span class="sxs-lookup"><span data-stu-id="344f8-826">SampleValue</span></span> |<span data-ttu-id="344f8-827">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-827">PerfHourly</span></span> |<span data-ttu-id="344f8-828">Průměrnou hodnotu pro hodinové agregace čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="344f8-828">Average value for the hourly aggregation of a performance counter.</span></span> |
| <span data-ttu-id="344f8-829">Min.</span><span class="sxs-lookup"><span data-stu-id="344f8-829">Min</span></span> |<span data-ttu-id="344f8-830">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-830">PerfHourly</span></span> |<span data-ttu-id="344f8-831">Minimální hodnota v hodinový interval agregace hodinové čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="344f8-831">Minimum value in the hourly interval of a performance counter hourly aggregate.</span></span> |
| <span data-ttu-id="344f8-832">Max.</span><span class="sxs-lookup"><span data-stu-id="344f8-832">Max</span></span> |<span data-ttu-id="344f8-833">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-833">PerfHourly</span></span> |<span data-ttu-id="344f8-834">Maximální hodnota v hodinový interval agregace hodinové čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="344f8-834">Maximum value in the hourly interval of a performance counter hourly aggregate.</span></span> |
| <span data-ttu-id="344f8-835">Percentile95</span><span class="sxs-lookup"><span data-stu-id="344f8-835">Percentile95</span></span> |<span data-ttu-id="344f8-836">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-836">PerfHourly</span></span> |<span data-ttu-id="344f8-837">Hodnota 95. percentil hodinový interval agregace hodinové čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="344f8-837">The 95th percentile value for the hourly interval of a performance counter hourly aggregate.</span></span> |
| <span data-ttu-id="344f8-838">SampleCount</span><span class="sxs-lookup"><span data-stu-id="344f8-838">SampleCount</span></span> |<span data-ttu-id="344f8-839">PerfHourly</span><span class="sxs-lookup"><span data-stu-id="344f8-839">PerfHourly</span></span> |<span data-ttu-id="344f8-840">Kolik nezpracovaná vzorků jste použili k vytvoření tento záznam hodinové agregace.</span><span class="sxs-lookup"><span data-stu-id="344f8-840">How many raw performance counter samples were used to produce this hourly aggregate record.</span></span> |
| <span data-ttu-id="344f8-841">Hrozby</span><span class="sxs-lookup"><span data-stu-id="344f8-841">Threat</span></span> |<span data-ttu-id="344f8-842">ProtectionStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-842">ProtectionStatus</span></span> |<span data-ttu-id="344f8-843">Název malwaru byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="344f8-843">Name of malware found.</span></span> |
| <span data-ttu-id="344f8-844">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="344f8-844">StorageAccount</span></span> |<span data-ttu-id="344f8-845">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-845">W3CIISLog</span></span> |<span data-ttu-id="344f8-846">Účet služby Azure Storage protokol byl načten z.</span><span class="sxs-lookup"><span data-stu-id="344f8-846">Azure Storage account the log was read from.</span></span> |
| <span data-ttu-id="344f8-847">AzureDeploymentID</span><span class="sxs-lookup"><span data-stu-id="344f8-847">AzureDeploymentID</span></span> |<span data-ttu-id="344f8-848">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-848">W3CIISLog</span></span> |<span data-ttu-id="344f8-849">ID Azure nasazení cloudové služby v protokolu patří.</span><span class="sxs-lookup"><span data-stu-id="344f8-849">Azure deployment ID of the cloud service the log belongs to.</span></span> |
| <span data-ttu-id="344f8-850">Role</span><span class="sxs-lookup"><span data-stu-id="344f8-850">Role</span></span> |<span data-ttu-id="344f8-851">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-851">W3CIISLog</span></span> |<span data-ttu-id="344f8-852">Role služby Azure cloud protokol patří.</span><span class="sxs-lookup"><span data-stu-id="344f8-852">Role of the Azure cloud service the log belongs to.</span></span> |
| <span data-ttu-id="344f8-853">RoleInstance</span><span class="sxs-lookup"><span data-stu-id="344f8-853">RoleInstance</span></span> |<span data-ttu-id="344f8-854">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-854">W3CIISLog</span></span> |<span data-ttu-id="344f8-855">RoleInstance Azure role, které patří do protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-855">RoleInstance of the Azure role that the log belongs to.</span></span> |
| <span data-ttu-id="344f8-856">sSiteName</span><span class="sxs-lookup"><span data-stu-id="344f8-856">sSiteName</span></span> |<span data-ttu-id="344f8-857">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-857">W3CIISLog</span></span> |<span data-ttu-id="344f8-858">Web služby IIS, které patří protokol (metabáze zápis); pole s-sitename v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-858">IIS website that the log belongs to (metabase notation); the s-sitename field in the original log.</span></span> |
| <span data-ttu-id="344f8-859">sComputerName</span><span class="sxs-lookup"><span data-stu-id="344f8-859">sComputerName</span></span> |<span data-ttu-id="344f8-860">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-860">W3CIISLog</span></span> |<span data-ttu-id="344f8-861">Pole s-computername v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-861">The s-computername field in the original log.</span></span> |
| <span data-ttu-id="344f8-862">sIP</span><span class="sxs-lookup"><span data-stu-id="344f8-862">sIP</span></span> |<span data-ttu-id="344f8-863">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-863">W3CIISLog</span></span> |<span data-ttu-id="344f8-864">Požadavek HTTP adresa IP serveru byla určena.</span><span class="sxs-lookup"><span data-stu-id="344f8-864">Server IP address the HTTP request was addressed to.</span></span> <span data-ttu-id="344f8-865">Pole s-ip v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-865">The s-ip field in the original log.</span></span> |
| <span data-ttu-id="344f8-866">csMethod</span><span class="sxs-lookup"><span data-stu-id="344f8-866">csMethod</span></span> |<span data-ttu-id="344f8-867">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-867">W3CIISLog</span></span> |<span data-ttu-id="344f8-868">Metoda HTTP (např. GET nebo POST) používaná klientem v požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="344f8-868">HTTP method (for example, GET/POST) used by the client in the HTTP request.</span></span> <span data-ttu-id="344f8-869">Cs metoda v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-869">The cs-method in the original log.</span></span> |
| <span data-ttu-id="344f8-870">cIP</span><span class="sxs-lookup"><span data-stu-id="344f8-870">cIP</span></span> |<span data-ttu-id="344f8-871">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-871">W3CIISLog</span></span> |<span data-ttu-id="344f8-872">Požadavek HTTP adresa IP klienta pochází.</span><span class="sxs-lookup"><span data-stu-id="344f8-872">Client IP address the HTTP request came from.</span></span> <span data-ttu-id="344f8-873">Pole c-ip v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-873">The c-ip field in the original log.</span></span> |
| <span data-ttu-id="344f8-874">csUserAgent</span><span class="sxs-lookup"><span data-stu-id="344f8-874">csUserAgent</span></span> |<span data-ttu-id="344f8-875">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-875">W3CIISLog</span></span> |<span data-ttu-id="344f8-876">HTTP User-Agent deklarovaná klienta (prohlížeče nebo jinak).</span><span class="sxs-lookup"><span data-stu-id="344f8-876">HTTP User-Agent declared by the client (browser or otherwise).</span></span> <span data-ttu-id="344f8-877">Cs uživatelského agenta v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-877">The cs-user-agent in the original log.</span></span> |
| <span data-ttu-id="344f8-878">scStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-878">scStatus</span></span> |<span data-ttu-id="344f8-879">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-879">W3CIISLog</span></span> |<span data-ttu-id="344f8-880">Kód stavu HTTP (například 200/403/500) vrácená serverem do klienta.</span><span class="sxs-lookup"><span data-stu-id="344f8-880">HTTP Status code (for example, 200/403/500) returned by the server to the client.</span></span> <span data-ttu-id="344f8-881">Cs stav v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-881">The cs-status in the original log.</span></span> |
| <span data-ttu-id="344f8-882">timeTaken</span><span class="sxs-lookup"><span data-stu-id="344f8-882">TimeTaken</span></span> |<span data-ttu-id="344f8-883">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-883">W3CIISLog</span></span> |<span data-ttu-id="344f8-884">Jak dlouho (v milisekundách), kterou trvalo dokončení požadavku.</span><span class="sxs-lookup"><span data-stu-id="344f8-884">How long (in milliseconds) that the request took to complete.</span></span> <span data-ttu-id="344f8-885">Pole timetaken v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-885">The timetaken field in the original log.</span></span> |
| <span data-ttu-id="344f8-886">csUriStem</span><span class="sxs-lookup"><span data-stu-id="344f8-886">csUriStem</span></span> |<span data-ttu-id="344f8-887">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-887">W3CIISLog</span></span> |<span data-ttu-id="344f8-888">Relativní identifikátor URI (bez adresa hostitele, který je, a vyhledávání), byl požadován.</span><span class="sxs-lookup"><span data-stu-id="344f8-888">Relative URI (without host address, that is, /search ) that was requested.</span></span> <span data-ttu-id="344f8-889">Cs-modul uristem pole v původní protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-889">The cs-uristem field in the original log.</span></span> |
| <span data-ttu-id="344f8-890">csUriQuery</span><span class="sxs-lookup"><span data-stu-id="344f8-890">csUriQuery</span></span> |<span data-ttu-id="344f8-891">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-891">W3CIISLog</span></span> |<span data-ttu-id="344f8-892">Dotaz v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="344f8-892">URI query.</span></span> <span data-ttu-id="344f8-893">Identifikátor URI dotazy jsou potřebný jenom u dynamických stránek, například stránky ASP, takže toto pole obvykle obsahuje pomlčka pro statické stránky.</span><span class="sxs-lookup"><span data-stu-id="344f8-893">URI queries are necessary only for dynamic pages, such as ASP pages, so this field usually contains a hyphen for static pages.</span></span> |
| <span data-ttu-id="344f8-894">sPort</span><span class="sxs-lookup"><span data-stu-id="344f8-894">sPort</span></span> |<span data-ttu-id="344f8-895">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-895">W3CIISLog</span></span> |<span data-ttu-id="344f8-896">Port serveru, který požadavek HTTP byl odeslán do (a zda služba IIS naslouchá, protože je převzata).</span><span class="sxs-lookup"><span data-stu-id="344f8-896">Server port that the HTTP request was sent to (and that IIS listens to, since it picked it up).</span></span> |
| <span data-ttu-id="344f8-897">csUserName</span><span class="sxs-lookup"><span data-stu-id="344f8-897">csUserName</span></span> |<span data-ttu-id="344f8-898">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-898">W3CIISLog</span></span> |<span data-ttu-id="344f8-899">Ověřit uživatelské jméno, je-li žádost ověřený a není anonymní.</span><span class="sxs-lookup"><span data-stu-id="344f8-899">Authenticated user name, if the request is authenticated and not anonymous.</span></span> |
| <span data-ttu-id="344f8-900">csVersion</span><span class="sxs-lookup"><span data-stu-id="344f8-900">csVersion</span></span> |<span data-ttu-id="344f8-901">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-901">W3CIISLog</span></span> |<span data-ttu-id="344f8-902">Verze protokolu HTTP použitý v požadavku (například HTTP/1.1).</span><span class="sxs-lookup"><span data-stu-id="344f8-902">HTTP Protocol version used in the request (for example, HTTP/1.1).</span></span> |
| <span data-ttu-id="344f8-903">csCookie</span><span class="sxs-lookup"><span data-stu-id="344f8-903">csCookie</span></span> |<span data-ttu-id="344f8-904">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-904">W3CIISLog</span></span> |<span data-ttu-id="344f8-905">Informace o souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="344f8-905">Cookie information.</span></span> |
| <span data-ttu-id="344f8-906">csReferer</span><span class="sxs-lookup"><span data-stu-id="344f8-906">csReferer</span></span> |<span data-ttu-id="344f8-907">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-907">W3CIISLog</span></span> |<span data-ttu-id="344f8-908">Web, který uživatel navštívil jako poslední.</span><span class="sxs-lookup"><span data-stu-id="344f8-908">Site that the user last visited.</span></span> <span data-ttu-id="344f8-909">Tento web poskytl odkaz na aktuální web.</span><span class="sxs-lookup"><span data-stu-id="344f8-909">This site provided a link to the current site.</span></span> |
| <span data-ttu-id="344f8-910">csHost</span><span class="sxs-lookup"><span data-stu-id="344f8-910">csHost</span></span> |<span data-ttu-id="344f8-911">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-911">W3CIISLog</span></span> |<span data-ttu-id="344f8-912">Hlavička hostitele (například www.mysite.com) požadovanou.</span><span class="sxs-lookup"><span data-stu-id="344f8-912">Host header (for example, www.mysite.com) that was requested.</span></span> |
| <span data-ttu-id="344f8-913">scSubStatus</span><span class="sxs-lookup"><span data-stu-id="344f8-913">scSubStatus</span></span> |<span data-ttu-id="344f8-914">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-914">W3CIISLog</span></span> |<span data-ttu-id="344f8-915">Druhotný stavový kód chyby.</span><span class="sxs-lookup"><span data-stu-id="344f8-915">Substatus error code.</span></span> |
| <span data-ttu-id="344f8-916">scWin32Status</span><span class="sxs-lookup"><span data-stu-id="344f8-916">scWin32Status</span></span> |<span data-ttu-id="344f8-917">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-917">W3CIISLog</span></span> |<span data-ttu-id="344f8-918">Stavový kód Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-918">Windows status code.</span></span> |
| <span data-ttu-id="344f8-919">csBytes</span><span class="sxs-lookup"><span data-stu-id="344f8-919">csBytes</span></span> |<span data-ttu-id="344f8-920">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-920">W3CIISLog</span></span> |<span data-ttu-id="344f8-921">Počet bajtů odeslaných v požadavku z klienta na server.</span><span class="sxs-lookup"><span data-stu-id="344f8-921">Bytes sent in the request from the client to the server.</span></span> |
| <span data-ttu-id="344f8-922">scBytes</span><span class="sxs-lookup"><span data-stu-id="344f8-922">scBytes</span></span> |<span data-ttu-id="344f8-923">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="344f8-923">W3CIISLog</span></span> |<span data-ttu-id="344f8-924">Počet bajtů vrácený zpět v odpovědi ze serveru do klienta.</span><span class="sxs-lookup"><span data-stu-id="344f8-924">Bytes returned back in the response from the server to the client.</span></span> |
| <span data-ttu-id="344f8-925">ConfigChangeType</span><span class="sxs-lookup"><span data-stu-id="344f8-925">ConfigChangeType</span></span> |<span data-ttu-id="344f8-926">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-926">ConfigurationChange</span></span> |<span data-ttu-id="344f8-927">Typ změny (například WindowsServices nebo softwaru).</span><span class="sxs-lookup"><span data-stu-id="344f8-927">Type of change (for example, WindowsServices/Software).</span></span> |
| <span data-ttu-id="344f8-928">ChangeCategory</span><span class="sxs-lookup"><span data-stu-id="344f8-928">ChangeCategory</span></span> |<span data-ttu-id="344f8-929">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-929">ConfigurationChange</span></span> |<span data-ttu-id="344f8-930">Kategorie změn (Added/upravené nebo odebrané).</span><span class="sxs-lookup"><span data-stu-id="344f8-930">Category of the change (Modified/Added/Removed).</span></span> |
| <span data-ttu-id="344f8-931">SoftwareType</span><span class="sxs-lookup"><span data-stu-id="344f8-931">SoftwareType</span></span> |<span data-ttu-id="344f8-932">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-932">ConfigurationChange</span></span> |<span data-ttu-id="344f8-933">Typ softwaru (aplikace nebo aktualizace).</span><span class="sxs-lookup"><span data-stu-id="344f8-933">Type of software (Update/Application).</span></span> |
| <span data-ttu-id="344f8-934">SoftwareName</span><span class="sxs-lookup"><span data-stu-id="344f8-934">SoftwareName</span></span> |<span data-ttu-id="344f8-935">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-935">ConfigurationChange</span></span> |<span data-ttu-id="344f8-936">Název softwaru (platí jenom pro změny softwaru).</span><span class="sxs-lookup"><span data-stu-id="344f8-936">Name of the software (only applicable to software changes).</span></span> |
| <span data-ttu-id="344f8-937">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="344f8-937">Publisher</span></span> |<span data-ttu-id="344f8-938">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-938">ConfigurationChange</span></span> |<span data-ttu-id="344f8-939">Dodavatele, který publikuje softwaru (platí jenom pro změny softwaru).</span><span class="sxs-lookup"><span data-stu-id="344f8-939">Vendor who publishes the software (only applicable to software changes).</span></span> |
| <span data-ttu-id="344f8-940">SvcChangeType</span><span class="sxs-lookup"><span data-stu-id="344f8-940">SvcChangeType</span></span> |<span data-ttu-id="344f8-941">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-941">ConfigurationChange</span></span> |<span data-ttu-id="344f8-942">Typ změny, které bylo použito na služby systému Windows (stavu/StartupType/cesta/Účet_služby).</span><span class="sxs-lookup"><span data-stu-id="344f8-942">Type of change that was applied on a Windows service (State/StartupType/Path/ServiceAccount).</span></span> <span data-ttu-id="344f8-943">Tento krok platí jenom pro změny služby Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-943">This is only applicable to Windows service changes.</span></span> |
| <span data-ttu-id="344f8-944">SvcDisplayName</span><span class="sxs-lookup"><span data-stu-id="344f8-944">SvcDisplayName</span></span> |<span data-ttu-id="344f8-945">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-945">ConfigurationChange</span></span> |<span data-ttu-id="344f8-946">Zobrazovaný název služby, která byla změněna.</span><span class="sxs-lookup"><span data-stu-id="344f8-946">Display name of the service that was changed.</span></span> |
| <span data-ttu-id="344f8-947">SvcName</span><span class="sxs-lookup"><span data-stu-id="344f8-947">SvcName</span></span> |<span data-ttu-id="344f8-948">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-948">ConfigurationChange</span></span> |<span data-ttu-id="344f8-949">Název služby, která byla změněna.</span><span class="sxs-lookup"><span data-stu-id="344f8-949">Name of the service that was changed.</span></span> |
| <span data-ttu-id="344f8-950">SvcState</span><span class="sxs-lookup"><span data-stu-id="344f8-950">SvcState</span></span> |<span data-ttu-id="344f8-951">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-951">ConfigurationChange</span></span> |<span data-ttu-id="344f8-952">Nový (aktuální) stav služby.</span><span class="sxs-lookup"><span data-stu-id="344f8-952">New (current) state of the service.</span></span> |
| <span data-ttu-id="344f8-953">SvcPreviousState</span><span class="sxs-lookup"><span data-stu-id="344f8-953">SvcPreviousState</span></span> |<span data-ttu-id="344f8-954">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-954">ConfigurationChange</span></span> |<span data-ttu-id="344f8-955">Předchozí známé stavu služby (platí jenom Pokud se změnila stav služby).</span><span class="sxs-lookup"><span data-stu-id="344f8-955">Previous known state of the service (only applicable if service state changed).</span></span> |
| <span data-ttu-id="344f8-956">SvcStartupType</span><span class="sxs-lookup"><span data-stu-id="344f8-956">SvcStartupType</span></span> |<span data-ttu-id="344f8-957">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-957">ConfigurationChange</span></span> |<span data-ttu-id="344f8-958">Typ spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="344f8-958">Service startup type.</span></span> |
| <span data-ttu-id="344f8-959">SvcPreviousStartupType</span><span class="sxs-lookup"><span data-stu-id="344f8-959">SvcPreviousStartupType</span></span> |<span data-ttu-id="344f8-960">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-960">ConfigurationChange</span></span> |<span data-ttu-id="344f8-961">Předchozí typ spuštění v služby (platí pouze je-li změnit typ spuštění služby).</span><span class="sxs-lookup"><span data-stu-id="344f8-961">Previous service startup type (only applicable if service startup type changed).</span></span> |
| <span data-ttu-id="344f8-962">SvcAccount</span><span class="sxs-lookup"><span data-stu-id="344f8-962">SvcAccount</span></span> |<span data-ttu-id="344f8-963">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-963">ConfigurationChange</span></span> |<span data-ttu-id="344f8-964">Účet služby.</span><span class="sxs-lookup"><span data-stu-id="344f8-964">Service account.</span></span> |
| <span data-ttu-id="344f8-965">SvcPreviousAccount</span><span class="sxs-lookup"><span data-stu-id="344f8-965">SvcPreviousAccount</span></span> |<span data-ttu-id="344f8-966">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-966">ConfigurationChange</span></span> |<span data-ttu-id="344f8-967">Účet služby předchozí (platí jenom Pokud změněn účet služby).</span><span class="sxs-lookup"><span data-stu-id="344f8-967">Previous service account (only applicable if service account changed).</span></span> |
| <span data-ttu-id="344f8-968">SvcPath</span><span class="sxs-lookup"><span data-stu-id="344f8-968">SvcPath</span></span> |<span data-ttu-id="344f8-969">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-969">ConfigurationChange</span></span> |<span data-ttu-id="344f8-970">Cesta ke spustitelnému souboru služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="344f8-970">Path to the executable of the Windows service.</span></span> |
| <span data-ttu-id="344f8-971">SvcPreviousPath</span><span class="sxs-lookup"><span data-stu-id="344f8-971">SvcPreviousPath</span></span> |<span data-ttu-id="344f8-972">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-972">ConfigurationChange</span></span> |<span data-ttu-id="344f8-973">Předchozí cesta ke spustitelnému souboru pro službu systému Windows (platí jenom pokud ho změnit).</span><span class="sxs-lookup"><span data-stu-id="344f8-973">Previous path of the executable for the Windows service (only applicable if it changed).</span></span> |
| <span data-ttu-id="344f8-974">SvcDescription</span><span class="sxs-lookup"><span data-stu-id="344f8-974">SvcDescription</span></span> |<span data-ttu-id="344f8-975">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-975">ConfigurationChange</span></span> |<span data-ttu-id="344f8-976">Popis služby.</span><span class="sxs-lookup"><span data-stu-id="344f8-976">Description of the service.</span></span> |
| <span data-ttu-id="344f8-977">Předchozí</span><span class="sxs-lookup"><span data-stu-id="344f8-977">Previous</span></span> |<span data-ttu-id="344f8-978">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-978">ConfigurationChange</span></span> |<span data-ttu-id="344f8-979">Předchozí stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo předchozí verze).</span><span class="sxs-lookup"><span data-stu-id="344f8-979">Previous state of this software (Installed/Not Installed/previous version).</span></span> |
| <span data-ttu-id="344f8-980">Aktuální</span><span class="sxs-lookup"><span data-stu-id="344f8-980">Current</span></span> |<span data-ttu-id="344f8-981">Změnakonfigurace</span><span class="sxs-lookup"><span data-stu-id="344f8-981">ConfigurationChange</span></span> |<span data-ttu-id="344f8-982">Nejnovější stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo aktuální verze).</span><span class="sxs-lookup"><span data-stu-id="344f8-982">Latest state of this software (Installed/Not Installed/current version).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="344f8-983">Další kroky</span><span class="sxs-lookup"><span data-stu-id="344f8-983">Next steps</span></span>
<span data-ttu-id="344f8-984">Další informace o protokolu hledání:</span><span class="sxs-lookup"><span data-stu-id="344f8-984">For additional information about log searches:</span></span>

* <span data-ttu-id="344f8-985">Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="344f8-985">Get familiar with [log searches](log-analytics-log-searches.md) to view detailed information gathered by solutions.</span></span>
* <span data-ttu-id="344f8-986">Použití [vlastních polí ve analýzy protokolů](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="344f8-986">Use [custom fields in Log Analytics](log-analytics-custom-fields.md) to extend log searches.</span></span>
