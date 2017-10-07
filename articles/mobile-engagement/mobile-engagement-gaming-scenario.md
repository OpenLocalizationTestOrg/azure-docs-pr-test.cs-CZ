---
title: "implementace aaaAzure Mobile Engagementu pro herní aplikace"
description: "Herní aplikace scénář tooimplement Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="198f7-103">Implementace s herní aplikace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="198f7-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="198f7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="198f7-104">Overview</span></span>
<span data-ttu-id="198f7-105">Spuštění herní má spuštění nové aplikace herní play role, strategie rybolovu na základě.</span><span class="sxs-lookup"><span data-stu-id="198f7-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="198f7-106">herní Hello byl spuštěný a funkční po dobu 6 měsíců.</span><span class="sxs-lookup"><span data-stu-id="198f7-106">hello game has been up and running for 6 months.</span></span> <span data-ttu-id="198f7-107">Tato hra je velký úspěšné a má miliony souborů ke stažení a uchování hello je velmi vysoká porovnání tooother spuštění herní aplikace.</span><span class="sxs-lookup"><span data-stu-id="198f7-107">This game is a huge success, and it has millions of downloads and hello retention is very high compared tooother start-up game apps.</span></span> <span data-ttu-id="198f7-108">Na zasedání čtvrtletně zkontrolujte hello zúčastněným stranám souhlas, že které potřebují tooincrease průměrný výnos na uživatele.</span><span class="sxs-lookup"><span data-stu-id="198f7-108">At hello quarterly review meeting, stakeholders agree they need tooincrease average revenue per user (ARPU).</span></span> <span data-ttu-id="198f7-109">Premium ve hře balíčky jsou k dispozici jako speciální nabídky.</span><span class="sxs-lookup"><span data-stu-id="198f7-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="198f7-110">Tyto sady pro herní povolit uživatelům tooupgrade hello vzhled a výkon svých rybářských čar a lures nebo tackles v herním hello.</span><span class="sxs-lookup"><span data-stu-id="198f7-110">These game packs allow users tooupgrade hello appearance and performance of their fishing lines and lures or tackles in hello game.</span></span> <span data-ttu-id="198f7-111">Balíček prodeje jsou však velmi nízké.</span><span class="sxs-lookup"><span data-stu-id="198f7-111">However, package sales are very low.</span></span> <span data-ttu-id="198f7-112">Proto se rozhodnou první tooanalyze hello na základě zkušeností uživatelů se nástroj pro analýzu a pak toodevelop k engagement program tooincrease prodeje pomocí pokročilé segmentace.</span><span class="sxs-lookup"><span data-stu-id="198f7-112">So they decide first tooanalyze hello customer experience with an analytics tool, and then toodevelop an engagement program tooincrease sales using advanced segmentation.</span></span>

<span data-ttu-id="198f7-113">Podle hello [Azure Mobile Engagement – Příručka Začínáme s osvědčenými postupy](mobile-engagement-getting-started-best-practices.md) vytváří strategii zapojení.</span><span class="sxs-lookup"><span data-stu-id="198f7-113">Based on hello [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="198f7-114">Cíle a klíčové ukazatele výkonu.</span><span class="sxs-lookup"><span data-stu-id="198f7-114">Objectives and KPIs</span></span>
<span data-ttu-id="198f7-115">Klíčových zúčastněných stran pro herní splňují hello.</span><span class="sxs-lookup"><span data-stu-id="198f7-115">Key stakeholders for hello game meet.</span></span> <span data-ttu-id="198f7-116">Všechny ke shodě o jeden hlavní cíl - tooincrease premium balíček prodeje podle 15 %.</span><span class="sxs-lookup"><span data-stu-id="198f7-116">All agree on one main objective - tooincrease premium package sales by 15%.</span></span> <span data-ttu-id="198f7-117">Uživatel vytvořit toomeasure obchodní klíčové ukazatele výkonu (KPI) a disku tohoto cíle</span><span class="sxs-lookup"><span data-stu-id="198f7-117">They create Business Key Performance Indicators (KPIs) toomeasure and drive this objective</span></span>

* <span data-ttu-id="198f7-118">Na kterou úroveň ve hře hello jsou tyto balíčky zakoupili?</span><span class="sxs-lookup"><span data-stu-id="198f7-118">On which level of hello game are these packages purchased?</span></span>
* <span data-ttu-id="198f7-119">Co je hello tržby na uživatele, na relaci, za týden a za měsíc?</span><span class="sxs-lookup"><span data-stu-id="198f7-119">What is hello revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="198f7-120">Jaké jsou typy Oblíbené nákupu hello?</span><span class="sxs-lookup"><span data-stu-id="198f7-120">What are hello favorite purchase types?</span></span>

<span data-ttu-id="198f7-121">Část 1 / hello [– Příručka Začínáme](mobile-engagement-getting-started-best-practices.md) vysvětluje, jak toodefine hello cílů a klíčových ukazatelů výkonu.</span><span class="sxs-lookup"><span data-stu-id="198f7-121">Part 1 of hello [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how toodefine hello objectives and KPIs.</span></span> 

<span data-ttu-id="198f7-122">U hello, které teď definované klíčové ukazatele výkonu vytvoří hello Mobile produktu Manager klíčové ukazatele zapojení toodetermine nové uživatele trendy a jejich uchovávání.</span><span class="sxs-lookup"><span data-stu-id="198f7-122">With hello Business KPIs now defined, hello Mobile Product Manager creates Engagement KPIs toodetermine new user trends and retention.</span></span>

* <span data-ttu-id="198f7-123">Monitorování uchovávání a použít v rámci hello následující intervaly: každý den, každý 2 dní, každý týden, měsíc a každé 3 měsíce</span><span class="sxs-lookup"><span data-stu-id="198f7-123">Monitor retention and use across hello following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="198f7-124">Počty aktivního uživatele</span><span class="sxs-lookup"><span data-stu-id="198f7-124">Active user counts</span></span>
* <span data-ttu-id="198f7-125">hodnocení aplikace Hello v hello uložit</span><span class="sxs-lookup"><span data-stu-id="198f7-125">hello app rating in hello store</span></span>

<span data-ttu-id="198f7-126">Na základě doporučení z hello IT tým, hello následující technické klíčové ukazatele výkonu byly přidány hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="198f7-126">Based on recommendations from hello IT team, hello following technical KPIs were added tooanswer hello following questions:</span></span>

* <span data-ttu-id="198f7-127">Co je cesta k uživatele (je navštívené stránky, která, kolik času stráví na něm)</span><span class="sxs-lookup"><span data-stu-id="198f7-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="198f7-128">Počet havárií nebo oznámení chyb došlo na relaci</span><span class="sxs-lookup"><span data-stu-id="198f7-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="198f7-129">Jaké verze operačního systému jsou mé uživatelé používají?</span><span class="sxs-lookup"><span data-stu-id="198f7-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="198f7-130">Jaká je průměrná velikost hello obrazovky pro svoje uživatele?</span><span class="sxs-lookup"><span data-stu-id="198f7-130">What is hello average size of screen for my users?</span></span>
* <span data-ttu-id="198f7-131">Jaký druh připojení k Internetu Moji uživatelé mají?</span><span class="sxs-lookup"><span data-stu-id="198f7-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="198f7-132">Pro každý ukazatel KPI hello Mobile produktu Manager určuje hello data Jana potřebuje, a kde se nachází v jeho scénářem.</span><span class="sxs-lookup"><span data-stu-id="198f7-132">For each KPI hello Mobile Product Manager specifies hello data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="198f7-133">Integrace produktů a program zapojení</span><span class="sxs-lookup"><span data-stu-id="198f7-133">Engagement program and integration</span></span>
<span data-ttu-id="198f7-134">Před vytvořením program k pokročilé zapojení, měli hello Mobile projektu ředitel starosti hello projektu hluboké znalosti jak a kdy jsou uživatelé hello uplatníte produkty.</span><span class="sxs-lookup"><span data-stu-id="198f7-134">Before building an advanced engagement program, hello Mobile Project Director in charge of hello project should have a deep understanding of how and when products are consumed by hello users.</span></span>

<span data-ttu-id="198f7-135">Po 3 měsíce shromáždil hello Mobile projektu ředitel dostatek dat tooenhance jeho prodej v aplikaci nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="198f7-135">After 3 months, hello Mobile Project Director has collected enough data tooenhance his in-app push notification sales.</span></span> <span data-ttu-id="198f7-136">Dozví, který:</span><span class="sxs-lookup"><span data-stu-id="198f7-136">He learns that:</span></span>

* <span data-ttu-id="198f7-137">první nákup Hello obecně se odehrává na úrovni hello 14.</span><span class="sxs-lookup"><span data-stu-id="198f7-137">hello first purchase generally happens at hello level 14.</span></span> <span data-ttu-id="198f7-138">Hello nákupu pro 90 % případy, je nové Legendární zbraní $3.</span><span class="sxs-lookup"><span data-stu-id="198f7-138">For 90% of those cases, hello purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="198f7-139">V těchto případech 80 % uživatelé, kteří provedli nákup, pokračovat s produktem hello a nastavit další uzavře.</span><span class="sxs-lookup"><span data-stu-id="198f7-139">In 80 % of those cases, users who have made a purchase, continue with hello product and make more purchases.</span></span>
* <span data-ttu-id="198f7-140">Uživatelé, kteří uplynulo hello úrovně 20, spusťte toospend více než 10 za týden.</span><span class="sxs-lookup"><span data-stu-id="198f7-140">Users who have passed hello level 20, start toospend more than $10/week.</span></span>
* <span data-ttu-id="198f7-141">Uživatelé zpravidla toobuy premium balíčky na úrovni 16, 24 a 32.</span><span class="sxs-lookup"><span data-stu-id="198f7-141">Users tend toobuy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="198f7-142">Děkujeme vám toothis analytických hello Mobile projektu ředitel rozhodne toocreate konkrétní nabízená oznámení pořadí tooincrease v aplikaci prodej.</span><span class="sxs-lookup"><span data-stu-id="198f7-142">Thanks toothis analysis hello Mobile Project Director decides toocreate specific push notification sequences tooincrease in app sales.</span></span> <span data-ttu-id="198f7-143">Vytvoří tři sekvencí nabízených oznámení, které mu volá: Vítejte program, Program prodeje a neaktivní programu.</span><span class="sxs-lookup"><span data-stu-id="198f7-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="198f7-144">Další informace najdete v části toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="198f7-144">For more information refer toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
