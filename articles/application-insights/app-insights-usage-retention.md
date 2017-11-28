---
title: "Analýza aaaUser uchovávání informací pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
description: "Počet uživatelů, kteří vrátit tooyour aplikace?"
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
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="309c6-103">Analýza uživatelů uchovávání informací pro webové aplikace pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="309c6-103">User retention analysis for web applications with Application Insights</span></span>

<span data-ttu-id="309c6-104">Funkce uchování Hello v [Azure Application Insights](app-insights-overview.md) pomáhá analyzovat počet uživatelů, kteří vrátit tooyour aplikace a jak často provádět konkrétní úlohy nebo dosáhnout cíle.</span><span class="sxs-lookup"><span data-stu-id="309c6-104">hello retention feature in [Azure Application Insights](app-insights-overview.md) helps you analyze how many users return tooyour app, and how often they perform particular tasks or achieve goals.</span></span> <span data-ttu-id="309c6-105">Například pokud spustíte herní lokality, může porovnat hello počet uživatelů, kteří vrátit toohello lokality po ztrátě hry s číslem hello, kteří se vrátí po hodnocený.</span><span class="sxs-lookup"><span data-stu-id="309c6-105">For example, if you run a game site, you could compare hello numbers of users who return toohello site after losing a game with hello number who return after winning.</span></span> <span data-ttu-id="309c6-106">Tyto znalosti vám pomůžou líp uživatelské prostředí a obchodní strategie.</span><span class="sxs-lookup"><span data-stu-id="309c6-106">This knowledge can help you improve both your user experience and your business strategy.</span></span>

## <a name="get-started"></a><span data-ttu-id="309c6-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="309c6-107">Get started</span></span>

<span data-ttu-id="309c6-108">Pokud nevidíte ještě dat v nástroji uchování hello hello portálu Application Insights, [zjistěte, jak tooget pracovat s nástroji využití hello](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="309c6-108">If you don't yet see data in hello retention tool in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-retention-tool"></a><span data-ttu-id="309c6-109">Nástroj pro uchování Hello</span><span class="sxs-lookup"><span data-stu-id="309c6-109">hello Retention tool</span></span>

![Nástroj Udržení](./media/app-insights-usage-retention/retention.png)

1. <span data-ttu-id="309c6-111">Hello nástrojů umožňuje uživatelům toocreate nové sestavy uchování, otevřete stávajících sestav uchování, uložit aktuální sestavu uchování nebo uložit jako, vrátit zpět změny provedené toosaved sestavy a aktualizovat data v sestavě hello, sdílené složky sestavy prostřednictvím e-mailu nebo přímý odkaz a hello přístup stránka dokumentace.</span><span class="sxs-lookup"><span data-stu-id="309c6-111">hello toolbar allows users toocreate new retention reports, open existing retention reports, save current retention report or save as, revert changes made toosaved reports, refresh data on hello report, share report via email or direct link, and access hello documentation page.</span></span> 
2. <span data-ttu-id="309c6-112">Ve výchozím nastavení zobrazuje uchování všichni uživatelé, kteří neprovedli nic, pak se vrátila zpět a neprovedli nic jiného v období.</span><span class="sxs-lookup"><span data-stu-id="309c6-112">By default, retention shows all users who did anything then came back and did anything else over a period.</span></span> <span data-ttu-id="309c6-113">Můžete vybrat různé kombinace události toonarrow hello zaměřením na aktivit konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="309c6-113">You can select different combination of events toonarrow hello focus on specific user activities.</span></span>
3. <span data-ttu-id="309c6-114">Přidejte jeden nebo více filtry vlastností.</span><span class="sxs-lookup"><span data-stu-id="309c6-114">Add one or more filters on properties.</span></span> <span data-ttu-id="309c6-115">Například se můžete soustředit na uživatele v konkrétní země nebo oblasti.</span><span class="sxs-lookup"><span data-stu-id="309c6-115">For example, you can focus on users in a particular country or region.</span></span> <span data-ttu-id="309c6-116">Klikněte na tlačítko **aktualizace** po nastavení filtrů hello.</span><span class="sxs-lookup"><span data-stu-id="309c6-116">Click **Update** after setting hello filters.</span></span> 
4. <span data-ttu-id="309c6-117">Hello celkové uchování graf zobrazuje souhrnné údaje o uchování uživatele napříč hello vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="309c6-117">hello overall retention chart shows a summary of user retention across hello selected time period.</span></span> 
5. <span data-ttu-id="309c6-118">Hello mřížky ukazuje zachování hello počet uživatelů podle toohello Tvůrce dotazů v #. 2.</span><span class="sxs-lookup"><span data-stu-id="309c6-118">hello grid shows hello number of users retained according toohello query builder in #2.</span></span> <span data-ttu-id="309c6-119">Každý řádek představuje kohorty uživatele, který provedl všechny události v hello časovém období.</span><span class="sxs-lookup"><span data-stu-id="309c6-119">Each row represents a cohort of users who performed any event in hello time period shown.</span></span> <span data-ttu-id="309c6-120">Každá buňka hello řádek ukazuje, kolik této kohorty vrátil nejméně jednou za na pozdější dobu.</span><span class="sxs-lookup"><span data-stu-id="309c6-120">Each cell in hello row shows how many of that cohort returned at least once in a later period.</span></span> <span data-ttu-id="309c6-121">Někteří uživatelé se můžou vrátit ve více než jedno období.</span><span class="sxs-lookup"><span data-stu-id="309c6-121">Some users may return in more than one period.</span></span> 
6. <span data-ttu-id="309c6-122">Hello Statistika karty Zobrazit nejvyšší pět inicializace události a nejvyšší pět vrátil události toogive uživatelé lépe porozumět jejich uchování sestav.</span><span class="sxs-lookup"><span data-stu-id="309c6-122">hello insights cards show top five initiating events, and top five returned events toogive users a better understanding of their retention report.</span></span> 

![Při přechodu myší uchování](./media/app-insights-usage-retention/hover.png)

<span data-ttu-id="309c6-124">Uživatelé mohou pozastavte ukazatel myši nad buněk na hello uchování nástroj tooaccess hello analytics tlačítko a popisy vysvětlující, co hello buňky prostředky.</span><span class="sxs-lookup"><span data-stu-id="309c6-124">Users can hover over cells on hello retention tool tooaccess hello analytics button and tool tips explaining what hello cell means.</span></span> <span data-ttu-id="309c6-125">Tlačítko Analytics Hello trvá nástroj pro analýzu toohello uživatelé s uživateli předem vyplněná dotazu toogenerate z hello buňky.</span><span class="sxs-lookup"><span data-stu-id="309c6-125">hello Analytics button takes users toohello Analytics tool with a pre-populated query toogenerate users from hello cell.</span></span> 

## <a name="use-business-events-tootrack-retention"></a><span data-ttu-id="309c6-126">Použít obchodní události tootrack uchování</span><span class="sxs-lookup"><span data-stu-id="309c6-126">Use business events tootrack retention</span></span>

<span data-ttu-id="309c6-127">tooget hello nejužitečnější uchování analýzy, míru události, které představují důležité obchodní aktivity.</span><span class="sxs-lookup"><span data-stu-id="309c6-127">tooget hello most useful retention analysis, measure events that represent significant business activities.</span></span> 

<span data-ttu-id="309c6-128">Například může být řada uživatelů otevřete stránku ve vaší aplikaci bez hraní her hello, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="309c6-128">For example, many users might open a page in your app without playing hello game that it displays.</span></span> <span data-ttu-id="309c6-129">Sledování právě zobrazení stránky hello by proto zadejte nesprávné odhad kolik lidí vrátit tooplay hello herní po jeho využívat dříve.</span><span class="sxs-lookup"><span data-stu-id="309c6-129">Tracking just hello page views would therefore provide an inaccurate estimate of how many people return tooplay hello game after enjoying it previously.</span></span> <span data-ttu-id="309c6-130">tooget přehledné informace o vrácení přehrávače, vaše aplikace by měli poslat vlastní události, když uživatel ve skutečnosti hraje.</span><span class="sxs-lookup"><span data-stu-id="309c6-130">tooget a clear picture of returning players, your app should send a custom event when a user actually plays.</span></span>  

<span data-ttu-id="309c6-131">Je toocode vhodné vlastní se události, které představují klíče obchodní akce a použít pro uchování analýzy.</span><span class="sxs-lookup"><span data-stu-id="309c6-131">It's good practice toocode custom events that represent key business actions, and use these for your retention analysis.</span></span> <span data-ttu-id="309c6-132">toocapture hello herní výsledek, musíte toowrite řádek kódu toosend vlastní události tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="309c6-132">toocapture hello game outcome, you need toowrite a line of code toosend a custom event tooApplication Insights.</span></span> <span data-ttu-id="309c6-133">Pokud píšete ho v kódu webové stránky hello nebo v Node.JS, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="309c6-133">If you write it in hello web page code or in Node.JS, it looks like this:</span></span>

```JavaScript
    appinsights.trackEvent("won game");
```

<span data-ttu-id="309c6-134">Nebo v serverovém kódu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="309c6-134">Or in ASP.NET server code:</span></span>

```C#
   telemetry.TrackEvent("won game");
```

<span data-ttu-id="309c6-135">[Další informace o vytváření vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="309c6-135">[Learn more about writing custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>


## <a name="next-steps"></a><span data-ttu-id="309c6-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="309c6-136">Next steps</span></span>
- <span data-ttu-id="309c6-137">využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="309c6-137">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="309c6-138">Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="309c6-138">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="309c6-139">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="309c6-139">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="309c6-140">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="309c6-140">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="309c6-141">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="309c6-141">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="309c6-142">Workbooks</span><span class="sxs-lookup"><span data-stu-id="309c6-142">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="309c6-143">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="309c6-143">Add user context</span></span>](app-insights-usage-send-user-context.md)


