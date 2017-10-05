---
title: "Analýza uživatelů uchovávání informací pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
description: "Počet uživatelů, kteří se vrátit do vaší aplikace?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 7f7ca19ab171278bbd82f68e3822bc650b25373d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="0f556-103">Analýza uživatelů uchovávání informací pro webové aplikace pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="0f556-103">User retention analysis for web applications with Application Insights</span></span>

<span data-ttu-id="0f556-104">Funkci uchování v [Azure Application Insights](app-insights-overview.md) pomáhá analyzovat počet uživatelů, kteří se vraťte do vaší aplikace a jak často provádět konkrétní úlohy nebo dosáhnout cíle.</span><span class="sxs-lookup"><span data-stu-id="0f556-104">The retention feature in [Azure Application Insights](app-insights-overview.md) helps you analyze how many users return to your app, and how often they perform particular tasks or achieve goals.</span></span> <span data-ttu-id="0f556-105">Například pokud spustíte herní lokality, může porovnejte počet uživatelů, kteří vrátit do lokality po ztrátě hry s číslem, kteří se vrátí po hodnocený.</span><span class="sxs-lookup"><span data-stu-id="0f556-105">For example, if you run a game site, you could compare the numbers of users who return to the site after losing a game with the number who return after winning.</span></span> <span data-ttu-id="0f556-106">Tyto znalosti vám pomůžou líp uživatelské prostředí a obchodní strategie.</span><span class="sxs-lookup"><span data-stu-id="0f556-106">This knowledge can help you improve both your user experience and your business strategy.</span></span>

## <a name="get-started"></a><span data-ttu-id="0f556-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0f556-107">Get started</span></span>

<span data-ttu-id="0f556-108">Pokud nevidíte ještě dat v nástroji pro uchovávání informací na portálu služby Application Insights [zjistěte, jak začít pracovat s nástroji využití](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0f556-108">If you don't yet see data in the retention tool in the Application Insights portal, [learn how to get started with the usage tools](app-insights-usage-overview.md).</span></span>

## <a name="the-retention-tool"></a><span data-ttu-id="0f556-109">Nástroj pro uchování</span><span class="sxs-lookup"><span data-stu-id="0f556-109">The Retention tool</span></span>

![Nástroj Udržení](./media/app-insights-usage-retention/retention.png)

1. <span data-ttu-id="0f556-111">Panel nástrojů umožňuje uživatelům vytváření nových sestav uchování, otevřete existující sestavy uchování, uloží aktuální sestavu uchování nebo uložit, protože vrátit zpět změny provedené v uložených sestav, aktualizovat data v sestavě, sdílené složky sestav e-mailem nebo přímý odkaz a přístup k na stránce dokumentace.</span><span class="sxs-lookup"><span data-stu-id="0f556-111">The toolbar allows users to create new retention reports, open existing retention reports, save current retention report or save as, revert changes made to saved reports, refresh data on the report, share report via email or direct link, and access the documentation page.</span></span> 
2. <span data-ttu-id="0f556-112">Ve výchozím nastavení zobrazuje uchování všichni uživatelé, kteří neprovedli nic, pak se vrátila zpět a neprovedli nic jiného v období.</span><span class="sxs-lookup"><span data-stu-id="0f556-112">By default, retention shows all users who did anything then came back and did anything else over a period.</span></span> <span data-ttu-id="0f556-113">Můžete vybrat různé kombinace události zúžit zaměření na konkrétního uživatele aktivity.</span><span class="sxs-lookup"><span data-stu-id="0f556-113">You can select different combination of events to narrow the focus on specific user activities.</span></span>
3. <span data-ttu-id="0f556-114">Přidejte jeden nebo více filtry vlastností.</span><span class="sxs-lookup"><span data-stu-id="0f556-114">Add one or more filters on properties.</span></span> <span data-ttu-id="0f556-115">Například se můžete soustředit na uživatele v konkrétní země nebo oblasti.</span><span class="sxs-lookup"><span data-stu-id="0f556-115">For example, you can focus on users in a particular country or region.</span></span> <span data-ttu-id="0f556-116">Klikněte na tlačítko **aktualizace** po nastavení filtrů.</span><span class="sxs-lookup"><span data-stu-id="0f556-116">Click **Update** after setting the filters.</span></span> 
4. <span data-ttu-id="0f556-117">Celkové uchování graf zobrazuje souhrn uživatele uchování mezi zvolené časové období.</span><span class="sxs-lookup"><span data-stu-id="0f556-117">The overall retention chart shows a summary of user retention across the selected time period.</span></span> 
5. <span data-ttu-id="0f556-118">Mřížky zobrazuje počet uživatelů, které uchovávají podle Tvůrce dotazů v #. 2.</span><span class="sxs-lookup"><span data-stu-id="0f556-118">The grid shows the number of users retained according to the query builder in #2.</span></span> <span data-ttu-id="0f556-119">Každý řádek představuje kohorty uživatele, který provedl všechny události v zobrazeném časovém období.</span><span class="sxs-lookup"><span data-stu-id="0f556-119">Each row represents a cohort of users who performed any event in the time period shown.</span></span> <span data-ttu-id="0f556-120">Každý buňky v řádku zobrazuje, kolik této kohorty vrátil nejméně jednou za na pozdější dobu.</span><span class="sxs-lookup"><span data-stu-id="0f556-120">Each cell in the row shows how many of that cohort returned at least once in a later period.</span></span> <span data-ttu-id="0f556-121">Někteří uživatelé se můžou vrátit ve více než jedno období.</span><span class="sxs-lookup"><span data-stu-id="0f556-121">Some users may return in more than one period.</span></span> 
6. <span data-ttu-id="0f556-122">Statistika karty Zobrazit nejvyšší pět inicializace události a horní pět vrácený události uživatelům lépe porozumět jejich uchovávání dat sestavy.</span><span class="sxs-lookup"><span data-stu-id="0f556-122">The insights cards show top five initiating events, and top five returned events to give users a better understanding of their retention report.</span></span> 

![Při přechodu myší uchování](./media/app-insights-usage-retention/hover.png)

<span data-ttu-id="0f556-124">Uživatelé mohou pozastavte ukazatel myši nad buněk na nástroj uchování pro přístup k analytics nástroj a tlačítko tipy vysvětlující, co znamená buňky.</span><span class="sxs-lookup"><span data-stu-id="0f556-124">Users can hover over cells on the retention tool to access the analytics button and tool tips explaining what the cell means.</span></span> <span data-ttu-id="0f556-125">Tlačítko Analytics trvá uživatelům nástroj pro analýzu s předem vyplněná dotazu generovat uživatelé z buňky.</span><span class="sxs-lookup"><span data-stu-id="0f556-125">The Analytics button takes users to the Analytics tool with a pre-populated query to generate users from the cell.</span></span> 

## <a name="use-business-events-to-track-retention"></a><span data-ttu-id="0f556-126">Použít obchodní události ke sledování uchování</span><span class="sxs-lookup"><span data-stu-id="0f556-126">Use business events to track retention</span></span>

<span data-ttu-id="0f556-127">Nejužitečnější analysis uchování získáte měření události, které představují důležité obchodní aktivity.</span><span class="sxs-lookup"><span data-stu-id="0f556-127">To get the most useful retention analysis, measure events that represent significant business activities.</span></span> 

<span data-ttu-id="0f556-128">Například může být řada uživatelů otevřete stránku ve vaší aplikaci bez hru, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="0f556-128">For example, many users might open a page in your app without playing the game that it displays.</span></span> <span data-ttu-id="0f556-129">Sledování právě zobrazení stránky by proto zadejte nesprávné odhad kolik lidí vrátit ve hře po jeho využívat dříve.</span><span class="sxs-lookup"><span data-stu-id="0f556-129">Tracking just the page views would therefore provide an inaccurate estimate of how many people return to play the game after enjoying it previously.</span></span> <span data-ttu-id="0f556-130">Získat přehledné informace o vrácení přehrávače, vaše aplikace by měli poslat vlastní události, když uživatel ve skutečnosti hraje.</span><span class="sxs-lookup"><span data-stu-id="0f556-130">To get a clear picture of returning players, your app should send a custom event when a user actually plays.</span></span>  

<span data-ttu-id="0f556-131">Je dobrým zvykem kód vlastní události, které představují klíče obchodní akce a použít pro uchování analýzy.</span><span class="sxs-lookup"><span data-stu-id="0f556-131">It's good practice to code custom events that represent key business actions, and use these for your retention analysis.</span></span> <span data-ttu-id="0f556-132">Když Pokud chcete zachytit herní výsledek, budete muset napsat řádek kódu pro odeslání vlastní události Application insights.</span><span class="sxs-lookup"><span data-stu-id="0f556-132">To capture the game outcome, you need to write a line of code to send a custom event to Application Insights.</span></span> <span data-ttu-id="0f556-133">Pokud píšete ho v kódu webové stránky nebo v Node.JS, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0f556-133">If you write it in the web page code or in Node.JS, it looks like this:</span></span>

```JavaScript
    appinsights.trackEvent("won game");
```

<span data-ttu-id="0f556-134">Nebo v serverovém kódu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="0f556-134">Or in ASP.NET server code:</span></span>

```C#
   telemetry.TrackEvent("won game");
```

<span data-ttu-id="0f556-135">[Další informace o vytváření vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="0f556-135">[Learn more about writing custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>


## <a name="next-steps"></a><span data-ttu-id="0f556-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f556-136">Next steps</span></span>
- <span data-ttu-id="0f556-137">Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="0f556-137">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="0f556-138">Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="0f556-138">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="0f556-139">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="0f556-139">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="0f556-140">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="0f556-140">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="0f556-141">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="0f556-141">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="0f556-142">Workbooks</span><span class="sxs-lookup"><span data-stu-id="0f556-142">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="0f556-143">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="0f556-143">Add user context</span></span>](app-insights-usage-send-user-context.md)


