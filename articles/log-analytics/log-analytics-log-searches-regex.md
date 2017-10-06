---
title: "hledání protokolu aaaRegular výrazy v OMS Log Analytics | Microsoft Docs"
description: "Můžete vytvořit hello RegEx – klíčové slovo v analýzy protokolů protokolu hledání toohello filtru hello výsledky podle tooa regulární výraz.  Tento článek obsahuje hello syntaxe pro tyto výrazy s několik příkladů."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 3033593dac2c50e911fc69054947d40d4a74369b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-regular-expressions-toofilter-log-searches-in-log-analytics"></a>Pomocí regulárních výrazů toofilter protokolu vyhledá v analýzy protokolů

[Přihlaste se hledání](log-analytics-log-searches.md) umožňují tooextract informace z úložiště analýzy protokolů hello.  [Výrazech filtrů](log-analytics-search-reference.md#filter-expressions) umožňují toofilter hello výsledky hledání hello podle toospecific kritéria.  Hello **RegEx** – klíčové slovo vám umožní toospecify regulární výraz pro filtr.  

Tento článek poskytuje podrobné informace o syntaxi regulárního výrazu hello používá analýzy protokolů.

> [!NOTE]
> Regulární výraz můžete použít pouze s prohledávatelné pole.  Další informace o prohledatelná pole najdete v tématu **typy polí** v [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx – klíčové slovo

Hello použijte následující syntaxi toouse hello **RegEx** – klíčové slovo v hledání protokolů.  Můžete použít hello ostatní části tohoto článku toodetermine hello syntaxe samotného regulárního výrazu hello.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Například zaznamenává toouse tooreturn upozornění na regulární výraz s typem *upozornění* nebo *chyba*, využije hello následující hledání protokolů.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Částečné shody
Všimněte si, že hello regulární výraz musí odpovídat hello celý text hello vlastnosti.  Termínem shodují jen částečně nevrátí žádné záznamy.  Například pokud jste se pokoušeli tooreturn záznamů z počítače s názvem srv01.contoso.com, hello následující hledání protokolů by **není** vrátit žádné záznamy.

    Computer=RegEx("srv..")

Je to proto, že pouze hello první část názvu hello odpovídá regulárnímu výrazu hello.  Hello následující dvě protokolu hledání by vrátit záznamů z tohoto počítače vzhledem k tomu, že budou odpovídat hello celý název.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Znaky
Zadejte jiné znaky.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| A | Jeden výskyt hello znaku. | Computer=Regex("Srv01.contoso.com") | Srv01.contoso.com |
| . | Jednoho libovolného znaku. | Computer=Regex("SRV...contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| na? | Nula nebo jeden výskyt hello znak. | Počítač = RegEx ("srv01?. contoso.com") | srv0.contoso.com<br>Srv01.contoso.com |
| * | Nula nebo více výskytů hello znaku. | Computer=Regex("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| + | Jeden nebo více výskytů hello znaku. | Computer=Regex("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Nahradit libovolný znak v závorkách hello | Computer=Regex("srv0[123].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [**-*z*] | Odpovídat jednoho znaku v rozsahu hello.  Může zahrnovat více oblastí. | Computer=Regex("srv0[1-3].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Žádná z hello znaků v závorkách hello | Computer=Regex("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^**-*z*] | Žádná z hello znaků v rozsahu hello. | Computer=Regex("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Odpovídat rozsahu číselné znaky. | Computer=Regex("SRV[01-03].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Libovolný řetězec znaků. | Počítač = RegEx ("srv@.contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Více výskytů znaku
Zadejte vícenásobné výskyty konkrétní znaků.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| {n} |  *n*výskyty hello znak. | Computer=Regex("BW-Win-sc01{3}.bwren.Lab") | BW-win-sc0111.bwren.lab |
| {n} |  *n*nebo více výskytů hello znak. | Computer=Regex("BW-Win-sc01{3,}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab<br>BW-win-sc0111111.bwren.lab |
| {n, m} |  *n*příliš*m* výskyty hello znak. | Computer=Regex("BW-Win-sc01{3,5}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logické výrazy
Vyberte z více hodnot.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| &#124; | Logické OR.  Vrátí výsledek, pokud odpovídají na buď výraz. | Typ = výstrahy AlertSeverity = RegEx ("upozornění &#124; Chyba") | Upozornění<br>Chyba |
| & | Logickým operátorem a.  Vrátí výsledek, pokud odpovídají na oba výrazy | EventData = regex ("(zabezpečení.\* &. \*úspěch. \*)") | Auditování úspěšných zabezpečení |


## <a name="literals"></a>Literály
Převod znaků tooliteral speciální znaky.  To zahrnuje znaky, které poskytují funkce tooregular výrazy například?-\*^\[\]{}\(\)+\|. &.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| \\ | Převede literálu tooa speciální znak. | Status_CF =\\[Chyba\\] @<br>Status_CF = chyby\\-@ | [Chyba] Soubor nebyl nalezen.<br>Chyba – soubor nebyl nalezen. |


## <a name="next-steps"></a>Další kroky

* Seznamte se s [protokolu hledání](log-analytics-log-searches.md) tooview a analyzovat data v úložišti analýzy protokolů hello.
