---
title: Odkaz na modul Azure CDN pravidla | Microsoft Docs
description: "Referenční dokumentace pro Azure CDN pravidla shody stav motoru a funkce."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: c10145661a8c575381493c9aaa901c3ef92c2e81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine"></a><span data-ttu-id="77825-103">Stroj pravidel Azure CDN</span><span class="sxs-lookup"><span data-stu-id="77825-103">Azure CDN rules engine</span></span>
<span data-ttu-id="77825-104">Toto téma obsahuje podrobný popis funkcí a podmínky k dispozici shodu pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="77825-104">This topic lists detailed descriptions of the available match conditions and features for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="77825-105">Modul HTTP pravidla slouží jako konečné právo na tom, jak konkrétní typy žádostí jsou zpracovávány CDN.</span><span class="sxs-lookup"><span data-stu-id="77825-105">The HTTP Rules Engine is designed to be the final authority on how specific types of requests are processed by the CDN.</span></span>

<span data-ttu-id="77825-106">**Běžná použití**:</span><span class="sxs-lookup"><span data-stu-id="77825-106">**Common uses**:</span></span>

- <span data-ttu-id="77825-107">Přepsání nebo definovat zásady vlastní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="77825-107">Override or define a custom cache policy.</span></span>
- <span data-ttu-id="77825-108">Zabezpečení nebo odmítnout požadavky pro citlivého obsahu.</span><span class="sxs-lookup"><span data-stu-id="77825-108">Secure or deny requests for sensitive content.</span></span>
- <span data-ttu-id="77825-109">Přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="77825-109">Redirect requests.</span></span>
- <span data-ttu-id="77825-110">Uložit data vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="77825-110">Store custom log data.</span></span>

## <a name="terminology"></a><span data-ttu-id="77825-111">Terminologie</span><span class="sxs-lookup"><span data-stu-id="77825-111">Terminology</span></span>
<span data-ttu-id="77825-112">Pravidlo je definováno prostřednictvím [ **podmíněné výrazy**](cdn-rules-engine-reference-conditional-expressions.md), [ **odpovídá**](cdn-rules-engine-reference-match-conditions.md), a [ **funkce**](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="77825-112">A rule is defined through the use of [**conditional expressions**](cdn-rules-engine-reference-conditional-expressions.md), [**matches**](cdn-rules-engine-reference-match-conditions.md), and [**features**](cdn-rules-engine-reference-features.md).</span></span> <span data-ttu-id="77825-113">Tyto prvky se zvýrazní na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="77825-113">These elements are highlighted in the following illustration.</span></span>

 ![Stav shody CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a><span data-ttu-id="77825-115">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="77825-115">Syntax</span></span>

<span data-ttu-id="77825-116">Způsobem, ve kterém budou zpracovány speciální znaky se liší podle jak podmínky shody nebo funkce zpracovává textové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="77825-116">The manner in which special characters will be treated varies according to how a match condition or feature handles text values.</span></span> <span data-ttu-id="77825-117">Stav shody nebo funkce mohou interpretovat text v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="77825-117">A match condition or feature may interpret text in one of the following ways:</span></span>

1. [<span data-ttu-id="77825-118">**Literálových hodnot**</span><span class="sxs-lookup"><span data-stu-id="77825-118">**Literal Values**</span></span>](#literal-values) 
2. [<span data-ttu-id="77825-119">**Hodnoty zástupných znaků**</span><span class="sxs-lookup"><span data-stu-id="77825-119">**Wildcard Values**</span></span>](#wildcard-values)
3. [<span data-ttu-id="77825-120">**Regulární výrazy**</span><span class="sxs-lookup"><span data-stu-id="77825-120">**Regular Expressions**</span></span>](#regular-expressions)

### <a name="literal-values"></a><span data-ttu-id="77825-121">Literálových hodnot</span><span class="sxs-lookup"><span data-stu-id="77825-121">Literal Values</span></span>
<span data-ttu-id="77825-122">Text, který je interpretována jako hodnota literálu bude považovat všechny speciální znaky, s výjimkou % symbol, jako součást hodnotu, která musí odpovídat.</span><span class="sxs-lookup"><span data-stu-id="77825-122">Text that is interpreted as a literal value will treat all special characters, with the exception of the % symbol, as a part of the value that must be matched.</span></span> <span data-ttu-id="77825-123">Jinými slovy, literál vyhovují podmínce nastavena na `\'*'\` bude pouze uspokojit, při které přesnou hodnotu (tj, `\'*'\`) se nachází.</span><span class="sxs-lookup"><span data-stu-id="77825-123">In other words, a literal match condition set to `\'*'\` will only be satisfied when that exact value (i.e., `\'*'\`) is found.</span></span>
 
<span data-ttu-id="77825-124">Symbol procenta slouží k označení kódování URL (například `%20`).</span><span class="sxs-lookup"><span data-stu-id="77825-124">A percentage symbol is used to indicate URL encoding (e.g., `%20`).</span></span>

### <a name="wildcard-values"></a><span data-ttu-id="77825-125">Hodnoty zástupných znaků</span><span class="sxs-lookup"><span data-stu-id="77825-125">Wildcard Values</span></span>
<span data-ttu-id="77825-126">Text, který je interpretován jako zástupný znak hodnotu, bude další významy přiřadit speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="77825-126">Text that is interpreted as a wildcard value will assign additional meaning to special characters.</span></span> <span data-ttu-id="77825-127">Následující tabulka popisuje, jak se interpretují následující sadu znaků.</span><span class="sxs-lookup"><span data-stu-id="77825-127">The following table describes how the following set of characters will be interpreted.</span></span>

<span data-ttu-id="77825-128">Znak</span><span class="sxs-lookup"><span data-stu-id="77825-128">Character</span></span> | <span data-ttu-id="77825-129">Popis</span><span class="sxs-lookup"><span data-stu-id="77825-129">Description</span></span>
----------|------------
\ | <span data-ttu-id="77825-130">Abyste se vyhnuli znaky zadané v této tabulce se používá zpětné lomítko.</span><span class="sxs-lookup"><span data-stu-id="77825-130">A backslash is used to escape any of the characters specified in this table.</span></span> <span data-ttu-id="77825-131">Zpětné lomítko je třeba zadat přímo před speciální znak, který by měly být ukončeny.</span><span class="sxs-lookup"><span data-stu-id="77825-131">A backslash must be specified directly before the special character that should be escaped.</span></span><br/><span data-ttu-id="77825-132">Například následující syntaxi řídicí sekvence hvězdičku:`\*`</span><span class="sxs-lookup"><span data-stu-id="77825-132">For example, the following syntax escapes an asterisk: `\*`</span></span>
% | <span data-ttu-id="77825-133">Symbol procenta slouží k označení kódování URL (například `%20`).</span><span class="sxs-lookup"><span data-stu-id="77825-133">A percentage symbol is used to indicate URL encoding (e.g., `%20`).</span></span>
* | <span data-ttu-id="77825-134">Znak hvězdičky je zástupný znak, který reprezentuje jeden nebo více znaků.</span><span class="sxs-lookup"><span data-stu-id="77825-134">An asterisk is a wildcard that represents one or more characters.</span></span>
<span data-ttu-id="77825-135">Místo</span><span class="sxs-lookup"><span data-stu-id="77825-135">Space</span></span> | <span data-ttu-id="77825-136">Znak mezery označuje, že shoda podmínku může vyhovět buď zadaným hodnotám nebo vzory.</span><span class="sxs-lookup"><span data-stu-id="77825-136">A space character indicates that a match condition may be satisfied by either of the specified values or patterns.</span></span>
<span data-ttu-id="77825-137">'Hodnota'</span><span class="sxs-lookup"><span data-stu-id="77825-137">'value'</span></span> | <span data-ttu-id="77825-138">V jednoduchých uvozovkách nemá zvláštní význam.</span><span class="sxs-lookup"><span data-stu-id="77825-138">A single quote does not have special meaning.</span></span> <span data-ttu-id="77825-139">Však sadu jednoduchých uvozovek a slouží k označení, že by měl být s hodnotou zacházet jako hodnota literálu.</span><span class="sxs-lookup"><span data-stu-id="77825-139">However, a set of single quotes is used to indicate that a value should be treated as a literal value.</span></span> <span data-ttu-id="77825-140">Dá se následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="77825-140">It can be used in the following ways:</span></span><br><br/><span data-ttu-id="77825-141">– Umožňuje shodu podmínku, kterou chcete být splněna vždy, když je zadaná hodnota odpovídá jakékoli její části hodnotu porovnání.</span><span class="sxs-lookup"><span data-stu-id="77825-141">- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.</span></span>  <span data-ttu-id="77825-142">Například `'ma'` odpovídá některé z následujících řetězců:</span><span class="sxs-lookup"><span data-stu-id="77825-142">For example, `'ma'` would match any of the following strings:</span></span> <br/><br/><span data-ttu-id="77825-143">/Business/**ma**rathon/asset.htm</span><span class="sxs-lookup"><span data-stu-id="77825-143">/business/**ma**rathon/asset.htm</span></span><br/><span data-ttu-id="77825-144">**Ma**p.gif</span><span class="sxs-lookup"><span data-stu-id="77825-144">**ma**p.gif</span></span><br/><span data-ttu-id="77825-145">/ obchodní či šablonu. **ma**p</span><span class="sxs-lookup"><span data-stu-id="77825-145">/business/template.**ma**p</span></span><br /><br /><span data-ttu-id="77825-146">– Umožňuje zadat jako literál znak zvláštní znak.</span><span class="sxs-lookup"><span data-stu-id="77825-146">- It allows a special character to be specified as a literal character.</span></span> <span data-ttu-id="77825-147">Můžete například určit literálu mezerou uzavřením znak mezery v rámci sady jednoduchých uvozovek (tj, `' '` nebo `'sample value'`).</span><span class="sxs-lookup"><span data-stu-id="77825-147">For example, you may specify a literal space character by enclosing a space character within a set of single quotes (i.e., `' '` or `'sample value'`).</span></span><br/><span data-ttu-id="77825-148">– Umožňuje na prázdnou hodnotu zadat.</span><span class="sxs-lookup"><span data-stu-id="77825-148">- It allows a blank value to be specified.</span></span> <span data-ttu-id="77825-149">Zadejte prázdnou hodnotu zadáním sadu jednoduchých uvozovek (tj, ").</span><span class="sxs-lookup"><span data-stu-id="77825-149">Specify a blank value by specifying a set of single quotes (i.e., '').</span></span><br /><br/><span data-ttu-id="77825-150">**Důležité:**</span><span class="sxs-lookup"><span data-stu-id="77825-150">**Important:**</span></span><br/><span data-ttu-id="77825-151">– Pokud je zadaná hodnota neobsahuje zástupný znak, pak je automaticky považována za literálovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="77825-151">- If the specified value does not contain a wildcard, then it will automatically be considered a literal value.</span></span> <span data-ttu-id="77825-152">To znamená, že není nutné zadávat sadu jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="77825-152">This means that it is not necessary to specify a set of single quotes.</span></span><br/><span data-ttu-id="77825-153">– Pokud zpětné lomítko není řídicí jiný znak v této tabulce, pak se budou ignorovat při zadané v rámci sady jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="77825-153">- If a backslash does not escape another character in this table, then it will be ignored when specified within a set of single quotes.</span></span><br/><span data-ttu-id="77825-154">-Jiný způsob, jak zadat speciální znaky, jako je literál znak pro přepnutí pomocí zpětné lomítko (tj, `\`).</span><span class="sxs-lookup"><span data-stu-id="77825-154">- Another way to specify a special character as a literal character is to escape it using a backslash (i.e., `\`).</span></span>

### <a name="regular-expressions"></a><span data-ttu-id="77825-155">Regulární výrazy</span><span class="sxs-lookup"><span data-stu-id="77825-155">Regular Expressions</span></span>

<span data-ttu-id="77825-156">Regulární výrazy definovat vzor, který bude vyhledávat v rámci textovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="77825-156">Regular expressions define a pattern that will be searched for within a text value.</span></span> <span data-ttu-id="77825-157">Zápis regulární výraz definuje určité významy pro celou řadu symboly.</span><span class="sxs-lookup"><span data-stu-id="77825-157">Regular expression notation defines specific meanings to a variety of symbols.</span></span> <span data-ttu-id="77825-158">Následující tabulka uvádí, jak speciální znaky jsou považovány podmínky shody a funkce, které podporují regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="77825-158">The following table indicates how special characters are treated by match conditions and features that support regular expressions.</span></span>

<span data-ttu-id="77825-159">Speciální znak</span><span class="sxs-lookup"><span data-stu-id="77825-159">Special Character</span></span> | <span data-ttu-id="77825-160">Popis</span><span class="sxs-lookup"><span data-stu-id="77825-160">Description</span></span>
------------------|------------
\ | <span data-ttu-id="77825-161">Zpětné lomítko řídicí sekvence znak pomocí následujícího ho.</span><span class="sxs-lookup"><span data-stu-id="77825-161">A backslash escapes the character the follows it.</span></span> <span data-ttu-id="77825-162">To způsobí, že tento znak, který má být považované za literálovou hodnotou místo s ohledem na její význam regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="77825-162">This causes that character to be treated as a literal value instead of taking on its regular expression meaning.</span></span> <span data-ttu-id="77825-163">Například následující syntaxi řídicí sekvence hvězdičku:`\*`</span><span class="sxs-lookup"><span data-stu-id="77825-163">For example, the following syntax escapes an asterisk: `\*`</span></span>
% | <span data-ttu-id="77825-164">Význam symbol procento závisí na jeho použití.</span><span class="sxs-lookup"><span data-stu-id="77825-164">The meaning of a percentage symbol depends on its usage.</span></span><br/><br/> <span data-ttu-id="77825-165">`%{HTTPVariable}`: Tato syntaxe identifikuje Proměnná HTTP.</span><span class="sxs-lookup"><span data-stu-id="77825-165">`%{HTTPVariable}`: This syntax identifies an HTTP variable.</span></span><br/><span data-ttu-id="77825-166">`%{HTTPVariable%Pattern}`: Tento syntaxi symbol procenta používá k identifikaci protokolu HTTP, proměnné a jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="77825-166">`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.</span></span><br /><span data-ttu-id="77825-167">`\%`: To má být použit jako hodnota literálu nebo určete kódování URL uvozovací znaky symbol procento umožňuje (například `\%20`).</span><span class="sxs-lookup"><span data-stu-id="77825-167">`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (e.g., `\%20`).</span></span>
* | <span data-ttu-id="77825-168">Znak hvězdičky umožňuje předchozí znak, který má odpovídat počtu nula či více krát.</span><span class="sxs-lookup"><span data-stu-id="77825-168">An asterisk allows the preceding character to be matched zero or more times.</span></span> 
<span data-ttu-id="77825-169">Místo</span><span class="sxs-lookup"><span data-stu-id="77825-169">Space</span></span> | <span data-ttu-id="77825-170">Znak mezery je obvykle považovány za znak literálu.</span><span class="sxs-lookup"><span data-stu-id="77825-170">A space character is typically treated as a literal character.</span></span> 
<span data-ttu-id="77825-171">'Hodnota'</span><span class="sxs-lookup"><span data-stu-id="77825-171">'value'</span></span> | <span data-ttu-id="77825-172">Jednoduchých uvozovek a jsou považovány za literály.</span><span class="sxs-lookup"><span data-stu-id="77825-172">Single quotes are treated as literal characters.</span></span> <span data-ttu-id="77825-173">Sadu jednoduchých uvozovek a nemá žádné zvláštní význam.</span><span class="sxs-lookup"><span data-stu-id="77825-173">A set of single quotes does not have special meaning.</span></span>


## <a name="next-steps"></a><span data-ttu-id="77825-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77825-174">Next steps</span></span>
* [<span data-ttu-id="77825-175">Stav shody motoru pravidla</span><span class="sxs-lookup"><span data-stu-id="77825-175">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="77825-176">Podmíněné výrazy stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="77825-176">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="77825-177">Funkce modulu pravidla</span><span class="sxs-lookup"><span data-stu-id="77825-177">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="77825-178">Přepsání výchozího nastavení HTTP používá stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="77825-178">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
* [<span data-ttu-id="77825-179">Přehled Azure CDN</span><span class="sxs-lookup"><span data-stu-id="77825-179">Azure CDN Overview</span></span>](cdn-overview.md)