---
title: "Pravidla ve službě Azure CDN modul podmíněné výrazy | Microsoft Docs"
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
ms.openlocfilehash: 57e56c38e003cb83dcf44f455c4451d159db8a59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a><span data-ttu-id="e87a2-103">Pravidla ve službě Azure CDN modul podmíněné výrazy</span><span class="sxs-lookup"><span data-stu-id="e87a2-103">Azure CDN rules engine conditional expressions</span></span>
<span data-ttu-id="e87a2-104">Toto téma obsahuje podrobné popisy podmíněné výrazy pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="e87a2-104">This topic lists detailed descriptions of the Conditional Expressions for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="e87a2-105">První část pravidla je podmíněným výrazem.</span><span class="sxs-lookup"><span data-stu-id="e87a2-105">The first part of a rule is the Conditional Expression.</span></span>

<span data-ttu-id="e87a2-106">Podmíněným výrazem</span><span class="sxs-lookup"><span data-stu-id="e87a2-106">Conditional Expression</span></span> | <span data-ttu-id="e87a2-107">Popis</span><span class="sxs-lookup"><span data-stu-id="e87a2-107">Description</span></span>
-----------------------|-------------
<span data-ttu-id="e87a2-108">POKUD</span><span class="sxs-lookup"><span data-stu-id="e87a2-108">IF</span></span> | <span data-ttu-id="e87a2-109">Výraz IF je vždy součástí první příkaz v pravidle.</span><span class="sxs-lookup"><span data-stu-id="e87a2-109">An IF expression is always a part of the first statement in a rule.</span></span> <span data-ttu-id="e87a2-110">Stejně jako všechny ostatní podmíněné výrazy musí být tento příkaz IF přidružený shody.</span><span class="sxs-lookup"><span data-stu-id="e87a2-110">Like all other conditional expressions, this IF statement must be associated with a match.</span></span> <span data-ttu-id="e87a2-111">Pokud jsou definovány žádné další podmíněné výrazy, určuje toto porovnání kritérium, které je nutné splnit před sadu funkcí, je možné používat na žádost.</span><span class="sxs-lookup"><span data-stu-id="e87a2-111">If no additional conditional expressions are defined, then this match determines the criterion that must be met before a set of features may be applied to a request.</span></span>
<span data-ttu-id="e87a2-112">A POKUD</span><span class="sxs-lookup"><span data-stu-id="e87a2-112">AND IF</span></span> | <span data-ttu-id="e87a2-113">Výraz a v případě lze přidat pouze po následující typy podmíněné výrazy: IF, a v případě.</span><span class="sxs-lookup"><span data-stu-id="e87a2-113">An AND IF expression may only be added after the following types of conditional expressions:IF,AND IF.</span></span> <span data-ttu-id="e87a2-114">Ho znamená, že existuje jiný stav, který musí být splněné počáteční Pokud příkaz.</span><span class="sxs-lookup"><span data-stu-id="e87a2-114">It indicates that there is another condition that must be met for the initial IF statement.</span></span>
<span data-ttu-id="e87a2-115">POKUD JINÝ</span><span class="sxs-lookup"><span data-stu-id="e87a2-115">ELSE IF</span></span>| <span data-ttu-id="e87a2-116">Výraz ELSE IF Určuje alternativní podmínku, která je nutné splnit před sadu funkcí, které jsou specifické pro tento výraz ELSE když probíhá.</span><span class="sxs-lookup"><span data-stu-id="e87a2-116">An ELSE IF expression specifies an alternative condition that must be met before a set of features specific to this ELSE IF statement takes place.</span></span> <span data-ttu-id="e87a2-117">Přítomnost příkazu ELSE IF označuje konec předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="e87a2-117">The presence of an ELSE IF statement indicates the end of the previous statement.</span></span> <span data-ttu-id="e87a2-118">Pouze podmíněným výrazem, který se může použít jiný příkaz ELSE IF po příkazu ELSE IF.</span><span class="sxs-lookup"><span data-stu-id="e87a2-118">The only conditional expression that may be placed after an ELSE IF statement is another ELSE IF statement.</span></span> <span data-ttu-id="e87a2-119">To znamená, že příkaz ELSE IF lze použít pouze k určení jeden další podmínku, která musí být splněny.</span><span class="sxs-lookup"><span data-stu-id="e87a2-119">This means that an ELSE IF statement may only be used to specify a single additional condition that has to be met.</span></span>

<span data-ttu-id="e87a2-120">**Příklad**: ![CDN vyhovují podmínce](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span><span class="sxs-lookup"><span data-stu-id="e87a2-120">**Example**: ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span></span>

 > [!TIP]
   > <span data-ttu-id="e87a2-121">Následující pravidlo může přepsat akce zadané předchozí pravidlem.</span><span class="sxs-lookup"><span data-stu-id="e87a2-121">A subsequent rule may override the actions specified by a previous rule.</span></span> <span data-ttu-id="e87a2-122">Příklad: Catch všechna pravidla zabezpečuje všechny požadavky na základě tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="e87a2-122">Example: A catch-all rule secures all requests via Token-Based Authentication.</span></span> <span data-ttu-id="e87a2-123">Přímo pod ním vytvářet výjimky pro určité typy požadavků může vytvořit jiné pravidlo.</span><span class="sxs-lookup"><span data-stu-id="e87a2-123">Another rule may be created directly below it to make an exception for certain types of requests.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e87a2-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e87a2-124">Next steps</span></span>
* [<span data-ttu-id="e87a2-125">Přehled Azure CDN</span><span class="sxs-lookup"><span data-stu-id="e87a2-125">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="e87a2-126">Referenční dokumentace pravidel modulu</span><span class="sxs-lookup"><span data-stu-id="e87a2-126">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="e87a2-127">Stav shody motoru pravidla</span><span class="sxs-lookup"><span data-stu-id="e87a2-127">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="e87a2-128">Funkce modulu pravidla</span><span class="sxs-lookup"><span data-stu-id="e87a2-128">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="e87a2-129">Přepsání výchozího nastavení HTTP používá stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="e87a2-129">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
