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
# <a name="azure-cdn-rules-engine-conditional-expressions"></a><span data-ttu-id="258cb-103">Pravidla ve službě Azure CDN modul podmíněné výrazy</span><span class="sxs-lookup"><span data-stu-id="258cb-103">Azure CDN rules engine conditional expressions</span></span>
<span data-ttu-id="258cb-104">Toto téma obsahuje podrobné popisy hello podmíněné výrazy pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="258cb-104">This topic lists detailed descriptions of hello Conditional Expressions for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="258cb-105">první část Hello pravidla je hello podmíněným výrazem.</span><span class="sxs-lookup"><span data-stu-id="258cb-105">hello first part of a rule is hello Conditional Expression.</span></span>

<span data-ttu-id="258cb-106">Podmíněným výrazem</span><span class="sxs-lookup"><span data-stu-id="258cb-106">Conditional Expression</span></span> | <span data-ttu-id="258cb-107">Popis</span><span class="sxs-lookup"><span data-stu-id="258cb-107">Description</span></span>
-----------------------|-------------
<span data-ttu-id="258cb-108">POKUD</span><span class="sxs-lookup"><span data-stu-id="258cb-108">IF</span></span> | <span data-ttu-id="258cb-109">Výraz IF je vždy součástí hello první příkaz v pravidle.</span><span class="sxs-lookup"><span data-stu-id="258cb-109">An IF expression is always a part of hello first statement in a rule.</span></span> <span data-ttu-id="258cb-110">Stejně jako všechny ostatní podmíněné výrazy musí být tento příkaz IF přidružený shody.</span><span class="sxs-lookup"><span data-stu-id="258cb-110">Like all other conditional expressions, this IF statement must be associated with a match.</span></span> <span data-ttu-id="258cb-111">Pokud jsou definovány žádné další podmíněné výrazy, určuje toto porovnání hello kritérium, které je nutné splnit před sadu funkcí, může být použité tooa požadavku.</span><span class="sxs-lookup"><span data-stu-id="258cb-111">If no additional conditional expressions are defined, then this match determines hello criterion that must be met before a set of features may be applied tooa request.</span></span>
<span data-ttu-id="258cb-112">A POKUD</span><span class="sxs-lookup"><span data-stu-id="258cb-112">AND IF</span></span> | <span data-ttu-id="258cb-113">Výraz a v případě lze přidat pouze po hello následující typy podmíněné výrazy: IF, a v případě.</span><span class="sxs-lookup"><span data-stu-id="258cb-113">An AND IF expression may only be added after hello following types of conditional expressions:IF,AND IF.</span></span> <span data-ttu-id="258cb-114">Ho znamená, že existuje jiný stav, který musí být splněné, pokud příkaz počáteční hello.</span><span class="sxs-lookup"><span data-stu-id="258cb-114">It indicates that there is another condition that must be met for hello initial IF statement.</span></span>
<span data-ttu-id="258cb-115">POKUD JINÝ</span><span class="sxs-lookup"><span data-stu-id="258cb-115">ELSE IF</span></span>| <span data-ttu-id="258cb-116">Výraz ELSE IF Určuje alternativní podmínky, které je nutné splnit před sadu funkce konkrétní toothis výraz ELSE když probíhá.</span><span class="sxs-lookup"><span data-stu-id="258cb-116">An ELSE IF expression specifies an alternative condition that must be met before a set of features specific toothis ELSE IF statement takes place.</span></span> <span data-ttu-id="258cb-117">Hello přítomnost příkazu ELSE IF naznačuje hello konec hello předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="258cb-117">hello presence of an ELSE IF statement indicates hello end of hello previous statement.</span></span> <span data-ttu-id="258cb-118">Hello pouze podmíněným výrazem, který se může použít jiný příkaz ELSE IF po příkazu ELSE IF.</span><span class="sxs-lookup"><span data-stu-id="258cb-118">hello only conditional expression that may be placed after an ELSE IF statement is another ELSE IF statement.</span></span> <span data-ttu-id="258cb-119">To znamená, příkaz ELSE IF může být pouze používané toospecify jeden další podmínku, která má toobe splněny.</span><span class="sxs-lookup"><span data-stu-id="258cb-119">This means that an ELSE IF statement may only be used toospecify a single additional condition that has toobe met.</span></span>

<span data-ttu-id="258cb-120">**Příklad**: ![CDN vyhovují podmínce](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span><span class="sxs-lookup"><span data-stu-id="258cb-120">**Example**: ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span></span>

 > [!TIP]
   > <span data-ttu-id="258cb-121">Následující pravidlo může přepsat hello akce zadané předchozí pravidlem.</span><span class="sxs-lookup"><span data-stu-id="258cb-121">A subsequent rule may override hello actions specified by a previous rule.</span></span> <span data-ttu-id="258cb-122">Příklad: Catch všechna pravidla zabezpečuje všechny požadavky na základě tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="258cb-122">Example: A catch-all rule secures all requests via Token-Based Authentication.</span></span> <span data-ttu-id="258cb-123">Jiné pravidlo může být vytvářeny pod ji přímo toomake výjimku pro určité typy požadavků.</span><span class="sxs-lookup"><span data-stu-id="258cb-123">Another rule may be created directly below it toomake an exception for certain types of requests.</span></span>

### <a name="next-steps"></a><span data-ttu-id="258cb-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="258cb-124">Next steps</span></span>
* [<span data-ttu-id="258cb-125">Přehled Azure CDN</span><span class="sxs-lookup"><span data-stu-id="258cb-125">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="258cb-126">Referenční dokumentace pravidel modulu</span><span class="sxs-lookup"><span data-stu-id="258cb-126">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="258cb-127">Stav shody motoru pravidla</span><span class="sxs-lookup"><span data-stu-id="258cb-127">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="258cb-128">Funkce modulu pravidla</span><span class="sxs-lookup"><span data-stu-id="258cb-128">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="258cb-129">Přepsání výchozího nastavení HTTP používá stroj pravidel hello</span><span class="sxs-lookup"><span data-stu-id="258cb-129">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
