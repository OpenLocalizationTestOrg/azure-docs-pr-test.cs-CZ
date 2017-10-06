---
title: odkaz na modul pravidla aaaAzure CDN | Microsoft Docs
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
ms.openlocfilehash: 4159715d15fc792afb49dcd2a30752fabcd94a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine"></a>Stroj pravidel Azure CDN
Toto téma obsahuje podrobný popis podmínky k dispozici shodu hello a funkcí pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Hello stroj pravidel HTTP je poslední autority hello navrženou toobe na tom, jak konkrétní typy žádostí jsou zpracovávány hello CDN.

**Běžná použití**:

- Přepsání nebo definovat zásady vlastní mezipaměti.
- Zabezpečení nebo odmítnout požadavky pro citlivého obsahu.
- Přesměrování požadavků.
- Uložit data vlastního protokolu.

## <a name="terminology"></a>Terminologie
Pravidlo je definováno prostřednictvím použití hello [ **podmíněné výrazy**](cdn-rules-engine-reference-conditional-expressions.md), [ **odpovídá**](cdn-rules-engine-reference-match-conditions.md), a [  **funkce**](cdn-rules-engine-reference-features.md). Tyto prvky jsou vyznačené na hello následující obrázek.

 ![Stav shody CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntaxe

Hello způsobem, ve kterém budou zpracovány speciální znaky se liší podle toohow podmínku shody nebo funkce obslužných rutin textové hodnoty. Stav shody nebo funkce mohou interpretovat text v jednom z následujících způsobů hello:

1. [**Literálových hodnot**](#literal-values) 
2. [**Hodnoty zástupných znaků**](#wildcard-values)
3. [**Regulární výrazy**](#regular-expressions)

### <a name="literal-values"></a>Literálových hodnot
Text, který je interpretována jako hodnota literálu bude považovat všechny speciální znaky, s výjimkou hello hello % symbolu, jako součást hello hodnotu, která musí odpovídat. Jinými slovy, literál vyhovují podmínce nastavit příliš`\'*'\` bude pouze uspokojit, při které přesnou hodnotu (tj, `\'*'\`) se nachází.
 
Symbol procenta je adresa URL použité tooindicate kódování (například `%20`).

### <a name="wildcard-values"></a>Hodnoty zástupných znaků
Text, který je interpretován jako zástupný znak hodnotu přiřadí další významy toospecial znaků. Hello následující tabulka popisuje, jak se interpretují hello následující sadu znaků.

Znak | Popis
----------|------------
\ | Zpětné lomítko je použité tooescape hello znaky zadané v této tabulce. Zpětné lomítko je třeba zadat přímo před hello speciální znak, který by měly být ukončeny.<br/>Například následující syntaxe hello řídicí sekvence hvězdičku:`\*`
% | Symbol procenta je adresa URL použité tooindicate kódování (například `%20`).
* | Znak hvězdičky je zástupný znak, který reprezentuje jeden nebo více znaků.
Místo | Znak mezery označuje, že shoda podmínku může vyhovět, buď hello zadané hodnoty nebo vzory.
'Hodnota' | V jednoduchých uvozovkách nemá zvláštní význam. Však použít sadu jednoduchých uvozovek a být tooindicate, která hodnota by měla být považován za literálovou hodnotou. Dá se v hello následující způsoby:<br><br/>– Umožňuje shodu podmínku toobe uspokojit vždy, když hello zadaná hodnota odpovídá jakékoli její části hodnotu porovnání hello.  Například `'ma'` by odpovídalo hello následující řetězce: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**Ma**p.gif<br/>/ obchodní či šablonu. **ma**p<br /><br />– Umožňuje speciální znak toobe, zadaný jako literál znak. Můžete například určit literálu mezerou uzavřením znak mezery v rámci sady jednoduchých uvozovek (tj, `' '` nebo `'sample value'`).<br/>– Umožňuje toobe prázdnou hodnotu zadat. Zadejte prázdnou hodnotu zadáním sadu jednoduchých uvozovek (tj, ").<br /><br/>**Důležité:**<br/>-Pokud hello zadané hodnoty neobsahuje zástupný znak, pak je automaticky považována za literálovou hodnotou. To znamená, že to není nutné toospecify sadu jednoduchých uvozovkách.<br/>– Pokud zpětné lomítko není řídicí jiný znak v této tabulce, pak se budou ignorovat při zadané v rámci sady jednoduchých uvozovek.<br/>-Jiný způsob toospecify speciální znak jako literál znak je tooescape pomocí zpětné lomítko (tj, `\`).

### <a name="regular-expressions"></a>Regulární výrazy

Regulární výrazy definovat vzor, který bude vyhledávat v rámci textovou hodnotu. Zápis regulární výraz definuje určité významy tooa řadu symboly. Hello následující tabulka uvádí jak speciální znaky jsou považovány podmínky shody a funkce, které podporují regulární výrazy.

Speciální znak | Popis
------------------|------------
\ | Následujícího hello zpětné lomítko hello řídicí sekvence znaků. To způsobí, že tento znak toobe považován za literálovou hodnotou místo s ohledem na její význam regulární výraz. Například následující syntaxe hello řídicí sekvence hvězdičku:`\*`
% | Hello význam symbol procento závisí na jeho použití.<br/><br/> `%{HTTPVariable}`: Tato syntaxe identifikuje Proměnná HTTP.<br/>`%{HTTPVariable%Pattern}`: Tato syntaxe používá procento symbol tooidentify protokolu HTTP, proměnné a jako oddělovač.<br />`\%`: Uvozovací znaky symbol procento umožňuje toobe použít jako hodnota literálu nebo tooindicate kódování URL (například `\%20`).
* | Znak hvězdičky umožňuje hello předcházející znak toobe odpovídá počtu nula či více krát. 
Místo | Znak mezery je obvykle považovány za znak literálu. 
'Hodnota' | Jednoduchých uvozovek a jsou považovány za literály. Sadu jednoduchých uvozovek a nemá žádné zvláštní význam.


## <a name="next-steps"></a>Další kroky
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
* [Přehled Azure CDN](cdn-overview.md)
