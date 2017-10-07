---
title: "aaaAzure CDN pravidla podmíněné výrazy na modul | Microsoft Docs"
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
ms.openlocfilehash: 39d0754c34a577f77ca87b6fd92e2b6a9e4ff8fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Pravidla ve službě Azure CDN modul podmíněné výrazy
Toto téma obsahuje podrobné popisy hello podmíněné výrazy pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

první část Hello pravidla je hello podmíněným výrazem.

Podmíněným výrazem | Popis
-----------------------|-------------
POKUD | Výraz IF je vždy součástí hello první příkaz v pravidle. Stejně jako všechny ostatní podmíněné výrazy musí být tento příkaz IF přidružený shody. Pokud jsou definovány žádné další podmíněné výrazy, určuje toto porovnání hello kritérium, které je nutné splnit před sadu funkcí, může být použité tooa požadavku.
A POKUD | Výraz a v případě lze přidat pouze po hello následující typy podmíněné výrazy: IF, a v případě. Ho znamená, že existuje jiný stav, který musí být splněné, pokud příkaz počáteční hello.
POKUD JINÝ| Výraz ELSE IF Určuje alternativní podmínky, které je nutné splnit před sadu funkce konkrétní toothis výraz ELSE když probíhá. Hello přítomnost příkazu ELSE IF naznačuje hello konec hello předchozí příkaz. Hello pouze podmíněným výrazem, který se může použít jiný příkaz ELSE IF po příkazu ELSE IF. To znamená, příkaz ELSE IF může být pouze používané toospecify jeden další podmínku, která má toobe splněny.

**Příklad**: ![CDN vyhovují podmínce](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Následující pravidlo může přepsat hello akce zadané předchozí pravidlem. Příklad: Catch všechna pravidla zabezpečuje všechny požadavky na základě tokenu ověřování. Jiné pravidlo může být vytvářeny pod ji přímo toomake výjimku pro určité typy požadavků.

### <a name="next-steps"></a>Další kroky
* [Přehled Azure CDN](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
