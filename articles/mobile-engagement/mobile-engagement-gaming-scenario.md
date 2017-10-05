---
title: "Azure Mobile Engagement implementace pro herní aplikace"
description: "Scénář herní aplikace k implementaci Azure Mobile Engagement"
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
ms.openlocfilehash: 0ca35a3d634db8eb5c63afacba046a35b8a3e7ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="fa98a-103">Implementace s herní aplikace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="fa98a-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="fa98a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="fa98a-104">Overview</span></span>
<span data-ttu-id="fa98a-105">Spuštění herní má spuštění nové aplikace herní play role, strategie rybolovu na základě.</span><span class="sxs-lookup"><span data-stu-id="fa98a-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="fa98a-106">Hry byl spuštěný a funkční po dobu 6 měsíců.</span><span class="sxs-lookup"><span data-stu-id="fa98a-106">The game has been up and running for 6 months.</span></span> <span data-ttu-id="fa98a-107">Tato hra je velký úspěšné a má miliony souborů ke stažení a uchovávání je velmi vysoká ve srovnání s jinými aplikacemi herní spuštění.</span><span class="sxs-lookup"><span data-stu-id="fa98a-107">This game is a huge success, and it has millions of downloads and the retention is very high compared to other start-up game apps.</span></span> <span data-ttu-id="fa98a-108">Na zasedání čtvrtletně zkontrolujte souhlas zúčastněným v prostředí, že které potřebují ke zvýšení průměrný výnos na uživatele.</span><span class="sxs-lookup"><span data-stu-id="fa98a-108">At the quarterly review meeting, stakeholders agree they need to increase average revenue per user (ARPU).</span></span> <span data-ttu-id="fa98a-109">Premium ve hře balíčky jsou k dispozici jako speciální nabídky.</span><span class="sxs-lookup"><span data-stu-id="fa98a-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="fa98a-110">Tyto sady pro herní umožňují uživatelům vzhled a výkonu svých rybářských řádky a lures nebo tackles ve hře upgradu.</span><span class="sxs-lookup"><span data-stu-id="fa98a-110">These game packs allow users to upgrade the appearance and performance of their fishing lines and lures or tackles in the game.</span></span> <span data-ttu-id="fa98a-111">Balíček prodeje jsou však velmi nízké.</span><span class="sxs-lookup"><span data-stu-id="fa98a-111">However, package sales are very low.</span></span> <span data-ttu-id="fa98a-112">Proto se rozhodnou nejprve k analýze zkušeností zákazníků se nástroj pro analýzu a vyvíjet engagement program a zvyšte prodeje pomocí pokročilé segmentace.</span><span class="sxs-lookup"><span data-stu-id="fa98a-112">So they decide first to analyze the customer experience with an analytics tool, and then to develop an engagement program to increase sales using advanced segmentation.</span></span>

<span data-ttu-id="fa98a-113">Na základě [Azure Mobile Engagement – Příručka Začínáme s osvědčenými postupy](mobile-engagement-getting-started-best-practices.md) vytváří strategii zapojení.</span><span class="sxs-lookup"><span data-stu-id="fa98a-113">Based on the [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="fa98a-114">Cíle a klíčové ukazatele výkonu.</span><span class="sxs-lookup"><span data-stu-id="fa98a-114">Objectives and KPIs</span></span>
<span data-ttu-id="fa98a-115">Splňovat klíčových zúčastněných stran pro příslušnou hru.</span><span class="sxs-lookup"><span data-stu-id="fa98a-115">Key stakeholders for the game meet.</span></span> <span data-ttu-id="fa98a-116">Všechny ke shodě o jeden hlavní cíl - zvýšit prodej balíček premium 15 %.</span><span class="sxs-lookup"><span data-stu-id="fa98a-116">All agree on one main objective - to increase premium package sales by 15%.</span></span> <span data-ttu-id="fa98a-117">Vytvoří obchodní klíčové ukazatele výkonu (KPI) k měření a disku tohoto cíle</span><span class="sxs-lookup"><span data-stu-id="fa98a-117">They create Business Key Performance Indicators (KPIs) to measure and drive this objective</span></span>

* <span data-ttu-id="fa98a-118">Na kterou úroveň ve hře jsou tyto balíčky zakoupili?</span><span class="sxs-lookup"><span data-stu-id="fa98a-118">On which level of the game are these packages purchased?</span></span>
* <span data-ttu-id="fa98a-119">Co je tržby na uživatele, na relaci, za týden a za měsíc?</span><span class="sxs-lookup"><span data-stu-id="fa98a-119">What is the revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="fa98a-120">Jaké jsou typy Oblíbené nákupu?</span><span class="sxs-lookup"><span data-stu-id="fa98a-120">What are the favorite purchase types?</span></span>

<span data-ttu-id="fa98a-121">Část 1 / [– Příručka Začínáme](mobile-engagement-getting-started-best-practices.md) vysvětluje, jak k definování cílů a klíčových ukazatelů výkonu.</span><span class="sxs-lookup"><span data-stu-id="fa98a-121">Part 1 of the [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how to define the objectives and KPIs.</span></span> 

<span data-ttu-id="fa98a-122">U obchodních klíčových ukazatelů výkonu nyní definována vytvoří správce produktu Mobile klíčové ukazatele zapojení k určení nového uživatele trendy a jejich uchovávání.</span><span class="sxs-lookup"><span data-stu-id="fa98a-122">With the Business KPIs now defined, the Mobile Product Manager creates Engagement KPIs to determine new user trends and retention.</span></span>

* <span data-ttu-id="fa98a-123">Monitorování uchovávání a použít v rámci následující intervaly: každý den, každý 2 dní, každý týden, měsíc a každé 3 měsíce</span><span class="sxs-lookup"><span data-stu-id="fa98a-123">Monitor retention and use across the following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="fa98a-124">Počty aktivního uživatele</span><span class="sxs-lookup"><span data-stu-id="fa98a-124">Active user counts</span></span>
* <span data-ttu-id="fa98a-125">Hodnocení aplikace v úložišti</span><span class="sxs-lookup"><span data-stu-id="fa98a-125">The app rating in the store</span></span>

<span data-ttu-id="fa98a-126">Na základě doporučení od týmu IT, byly přidány následující technické klíčové ukazatele výkonu zodpovědět následující otázky:</span><span class="sxs-lookup"><span data-stu-id="fa98a-126">Based on recommendations from the IT team, the following technical KPIs were added to answer the following questions:</span></span>

* <span data-ttu-id="fa98a-127">Co je cesta k uživatele (je navštívené stránky, která, kolik času stráví na něm)</span><span class="sxs-lookup"><span data-stu-id="fa98a-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="fa98a-128">Počet havárií nebo oznámení chyb došlo na relaci</span><span class="sxs-lookup"><span data-stu-id="fa98a-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="fa98a-129">Jaké verze operačního systému jsou mé uživatelé používají?</span><span class="sxs-lookup"><span data-stu-id="fa98a-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="fa98a-130">Jaká je průměrná velikost obrazovky pro svoje uživatele?</span><span class="sxs-lookup"><span data-stu-id="fa98a-130">What is the average size of screen for my users?</span></span>
* <span data-ttu-id="fa98a-131">Jaký druh připojení k Internetu Moji uživatelé mají?</span><span class="sxs-lookup"><span data-stu-id="fa98a-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="fa98a-132">Pro každý ukazatel KPI Mobile produktu Manager určuje data Jana potřebuje, a kde se nachází v jeho scénářem.</span><span class="sxs-lookup"><span data-stu-id="fa98a-132">For each KPI the Mobile Product Manager specifies the data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="fa98a-133">Integrace produktů a program zapojení</span><span class="sxs-lookup"><span data-stu-id="fa98a-133">Engagement program and integration</span></span>
<span data-ttu-id="fa98a-134">Před vytvořením program k pokročilé zapojení, ředitel projektu Mobile starosti projekt by měl mít hluboké znalosti, jak a kdy produkty jsou spotřebovávají uživatele.</span><span class="sxs-lookup"><span data-stu-id="fa98a-134">Before building an advanced engagement program, the Mobile Project Director in charge of the project should have a deep understanding of how and when products are consumed by the users.</span></span>

<span data-ttu-id="fa98a-135">Po 3 měsíce ředitel projektu Mobile shromáždil dostatek dat ke zvýšení jeho prodej v aplikaci nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="fa98a-135">After 3 months, the Mobile Project Director has collected enough data to enhance his in-app push notification sales.</span></span> <span data-ttu-id="fa98a-136">Dozví, který:</span><span class="sxs-lookup"><span data-stu-id="fa98a-136">He learns that:</span></span>

* <span data-ttu-id="fa98a-137">První nákup obvykle dochází na úrovni 14.</span><span class="sxs-lookup"><span data-stu-id="fa98a-137">The first purchase generally happens at the level 14.</span></span> <span data-ttu-id="fa98a-138">Pro 90 % těchto případech je nákupu nové Legendární zbraní $3.</span><span class="sxs-lookup"><span data-stu-id="fa98a-138">For 90% of those cases, the purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="fa98a-139">V těchto případech 80 % uživatelé, kteří provedli nákup, pokračovat s tímto produktem a nastavit další uzavře.</span><span class="sxs-lookup"><span data-stu-id="fa98a-139">In 80 % of those cases, users who have made a purchase, continue with the product and make more purchases.</span></span>
* <span data-ttu-id="fa98a-140">Uživatelé, kteří uplynulo úrovně 20, spustit zatěžovat více než 10 za týden.</span><span class="sxs-lookup"><span data-stu-id="fa98a-140">Users who have passed the level 20, start to spend more than $10/week.</span></span>
* <span data-ttu-id="fa98a-141">Uživatelé zpravidla koupit premium balíčky na úrovni 16, 24 a 32.</span><span class="sxs-lookup"><span data-stu-id="fa98a-141">Users tend to buy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="fa98a-142">Díky této analýze ředitel projektu Mobile rozhodne vytvořit konkrétní nabízená oznámení pořadí k nárůstu prodeje aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa98a-142">Thanks to this analysis the Mobile Project Director decides to create specific push notification sequences to increase in app sales.</span></span> <span data-ttu-id="fa98a-143">Vytvoří tři sekvencí nabízených oznámení, které mu volá: Vítejte program, Program prodeje a neaktivní programu.</span><span class="sxs-lookup"><span data-stu-id="fa98a-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="fa98a-144">Další informace najdete v části [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="fa98a-144">For more information refer to the [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
