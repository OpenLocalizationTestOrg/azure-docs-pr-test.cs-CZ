---
title: "Analýza uživatelů, relací a událostí ve službě Azure Application Insights | Microsoft docs"
description: "Demografické údaje analýzy uživatelů vaší webové aplikace."
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
ms.openlocfilehash: b154a01d1690bff4950ebc1ff5a5b89894d4d111
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a><span data-ttu-id="78ac0-103">Analýza uživatelů, relací a událostí ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="78ac0-103">Users, sessions, and events analysis in Application Insights</span></span>

<span data-ttu-id="78ac0-104">Zjistěte, kdy lidí používá vaši webovou aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají.</span><span class="sxs-lookup"><span data-stu-id="78ac0-104">Find out when people use your web app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> <span data-ttu-id="78ac0-105">Analyzovat obchodní a využití telemetrie pomocí [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="78ac0-105">Analyze business and usage telemetry by using [Azure Application Insights](app-insights-overview.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="78ac0-106">Začínáme</span><span class="sxs-lookup"><span data-stu-id="78ac0-106">Get started</span></span>

<span data-ttu-id="78ac0-107">Pokud nevidíte ještě data uživatelů, relací nebo okna události na portálu služby Application Insights [zjistěte, jak začít pracovat s nástroji využití](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="78ac0-107">If you don't yet see data in the users, sessions, or events blades in the Application Insights portal, [learn how to get started with the usage tools](app-insights-usage-overview.md).</span></span>

## <a name="the-users-sessions-and-events-segmentation-tool"></a><span data-ttu-id="78ac0-108">Nástroj segmentaci uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="78ac0-108">The Users, Sessions, and Events segmentation tool</span></span>

<span data-ttu-id="78ac0-109">Tři z okna využití použít nástroj stejné k nařezání a rozčlenění telemetrie z vaší webové aplikace z tři perspektivy.</span><span class="sxs-lookup"><span data-stu-id="78ac0-109">Three of the usage blades use the same tool to slice and dice telemetry from your web app from three perspectives.</span></span> <span data-ttu-id="78ac0-110">Filtrování a rozdělení dat, můžete odkrýt přehledy o relativní použití různých stránek a funkcí.</span><span class="sxs-lookup"><span data-stu-id="78ac0-110">By filtering and splitting the data, you can uncover insights about the relative usage of different pages and features.</span></span>

* <span data-ttu-id="78ac0-111">**Nástroj Uživatelé**: kolik lidí používá vaši aplikaci a jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="78ac0-111">**Users tool**: How many people used your app and its features.</span></span>  <span data-ttu-id="78ac0-112">Uživatelé, se počítají pomocí anonymní ID uložené v prohlížeči soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="78ac0-112">Users are counted by using anonymous IDs stored in browser cookies.</span></span> <span data-ttu-id="78ac0-113">Jednoho uživatele, kteří používají různé prohlížeče nebo počítače se budou počítat jako více než jeden uživatel.</span><span class="sxs-lookup"><span data-stu-id="78ac0-113">A single person using different browsers or machines will be counted as more than one user.</span></span>
* <span data-ttu-id="78ac0-114">**Nástroj relací**: kolik relací aktivity uživatelů, zahrnuli určité stránky a funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="78ac0-114">**Sessions tool**: How many sessions of user activity have included certain pages and features of your app.</span></span> <span data-ttu-id="78ac0-115">Relace se počítá po půl hodiny nečinnosti uživatele nebo po průběžné 24h použití.</span><span class="sxs-lookup"><span data-stu-id="78ac0-115">A session is counted after half an hour of user inactivity, or after continuous 24h of use.</span></span>
* <span data-ttu-id="78ac0-116">**Nástroj pro události**: jak často se používají určité stránky a funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="78ac0-116">**Events tool**: How often certain pages and features of your app are used.</span></span> <span data-ttu-id="78ac0-117">Zobrazení stránky se počítá při prohlížeč načte stránky z vaší aplikace, pokud máte [instrumentovány ho](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="78ac0-117">A page view is counted when a browser loads a page from your app, provided you have [instrumented it](app-insights-javascript.md).</span></span> 

    <span data-ttu-id="78ac0-118">Vlastní události představuje jeden výskyt něco stane v aplikaci, často zásah uživatele takto: klikněte na tlačítko nebo na dokončení některých úloh.</span><span class="sxs-lookup"><span data-stu-id="78ac0-118">A custom event represents one occurrence of something happening in your app, often a user interaction like a button click or the completion of some task.</span></span> <span data-ttu-id="78ac0-119">Vložení kódu v aplikaci, kterou chcete [generovat vlastní události](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="78ac0-119">You insert code in your app to [generate custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

![Použití nástroje](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a><span data-ttu-id="78ac0-121">Dotaz na určité uživatele</span><span class="sxs-lookup"><span data-stu-id="78ac0-121">Querying for Certain Users</span></span> 

<span data-ttu-id="78ac0-122">Prozkoumejte různé skupiny uživatelů úpravou možností dotazu v horní části nástroj Uživatelé:</span><span class="sxs-lookup"><span data-stu-id="78ac0-122">Explore different groups of users by adjusting the query options at the top of the Users tool:</span></span> 

* <span data-ttu-id="78ac0-123">Kdo používá: Zvolte vlastní události a stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="78ac0-123">Who used: Choose custom events and page views.</span></span> 
* <span data-ttu-id="78ac0-124">Během: Vyberte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="78ac0-124">During: Choose a time range.</span></span> 
* <span data-ttu-id="78ac0-125">Podle: Volba způsobu bucket dat, buď v časovém intervalu, nebo jinou vlastnost třeba prohlížeče nebo města.</span><span class="sxs-lookup"><span data-stu-id="78ac0-125">By: Choose how to bucket the data, either by a period of time or by another property such as browser or city.</span></span> 
* <span data-ttu-id="78ac0-126">Rozdělit: Zvolte vlastnosti a které segmentu nebo rozdělení data.</span><span class="sxs-lookup"><span data-stu-id="78ac0-126">Split By: Choose a property by which to split or segment the data.</span></span> 
* <span data-ttu-id="78ac0-127">Přidání filtrů: Omezte dotaz na určité uživatele, relace nebo události na základě jejich vlastností, jako je například prohlížeč nebo města.</span><span class="sxs-lookup"><span data-stu-id="78ac0-127">Add Filters: Limit the query to certain users, sessions, or events based on their properties, such as browser or city.</span></span> 
 
## <a name="saving-and-sharing-reports"></a><span data-ttu-id="78ac0-128">Ukládání a sdílení sestavy</span><span class="sxs-lookup"><span data-stu-id="78ac0-128">Saving and sharing reports</span></span> 
<span data-ttu-id="78ac0-129">Uživatelé sestavy, privátní vám v části Moje sestavy nebo sdílené můžete uložit se všemi ostatními s přístupem k tomuto prostředku Application Insights v části sdílené sestavy.</span><span class="sxs-lookup"><span data-stu-id="78ac0-129">You can save Users reports, either private just to you in the My Reports section, or shared with everyone else with access to this Application Insights resource in the Shared Reports section.</span></span>  
 
<span data-ttu-id="78ac0-130">Při ukládání sestavy nebo úpravou jeho vlastnosti, vyberte "Aktuální relativní časové rozmezí" pro uložení sestavy budou průběžně aktualizovat data, návratem některé pevné množství času.</span><span class="sxs-lookup"><span data-stu-id="78ac0-130">While saving a report or editing its properties, choose "Current Relative Time Range" to save a report will continuously refreshed data, going back some fixed amount of time.</span></span>  
 
<span data-ttu-id="78ac0-131">Zvolte "Aktuální absolutní časové rozmezí" Uložit sestavu s pevnou sadu dat.</span><span class="sxs-lookup"><span data-stu-id="78ac0-131">Choose "Current Absolute Time Range" to save a report with a fixed set of data.</span></span> <span data-ttu-id="78ac0-132">Mějte na paměti, že data ve službě Application Insights je uložena pouze po dobu 90 dnů, takže pokud uplynulo víc než 90 dní od sestavy s rozsahem absolutním čase byla uložena, sestavy se zobrazí prázdné.</span><span class="sxs-lookup"><span data-stu-id="78ac0-132">Keep in mind that data in Application Insights is only stored for 90 days, so if more than 90 days have passed since a report with an absolute time range was saved, the report will appear empty.</span></span> 
 
## <a name="example-instances"></a><span data-ttu-id="78ac0-133">Příklad instancí</span><span class="sxs-lookup"><span data-stu-id="78ac0-133">Example instances</span></span>

<span data-ttu-id="78ac0-134">V části instancí příklad zobrazí informace o několik jednotlivých uživatelů, relací nebo událostí, které jsou porovnávány pomocí aktuálního dotazu.</span><span class="sxs-lookup"><span data-stu-id="78ac0-134">The Example instances section shows information about a handful of individual users, sessions, or events that are matched by the current query.</span></span> <span data-ttu-id="78ac0-135">Vzhledem k tomu a zkoumat chování jednotlivce, kromě agregace, může poskytovat přehled o tom, jak lidé ve skutečnosti používají vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78ac0-135">Considering and exploring the behaviors of individuals, in addition to aggregates, can provide insights about how people actually use your app.</span></span> 
 
## <a name="insights"></a><span data-ttu-id="78ac0-136">Insights</span><span class="sxs-lookup"><span data-stu-id="78ac0-136">Insights</span></span> 

<span data-ttu-id="78ac0-137">Na bočním panelu Statistika ukazuje velkých clusterech uživatelů, které sdílejí společné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="78ac0-137">The Insights sidebar shows large clusters of users that share common properties.</span></span> <span data-ttu-id="78ac0-138">Tyto clustery odkrýt překvapivé trendy v tom, jak uživatelé používají vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78ac0-138">These clusters can uncover surprising trends in how people use your app.</span></span> <span data-ttu-id="78ac0-139">Pokud například 40 % všech využití aplikace pochází z uživatelé, kteří používají jeden funkce.</span><span class="sxs-lookup"><span data-stu-id="78ac0-139">For example, if 40% of all of the usage of your app comes from people using a single feature.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="78ac0-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78ac0-140">Next steps</span></span>
- <span data-ttu-id="78ac0-141">Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="78ac0-141">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="78ac0-142">Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="78ac0-142">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="78ac0-143">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="78ac0-143">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="78ac0-144">Uchování</span><span class="sxs-lookup"><span data-stu-id="78ac0-144">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="78ac0-145">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="78ac0-145">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="78ac0-146">Workbooks</span><span class="sxs-lookup"><span data-stu-id="78ac0-146">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="78ac0-147">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="78ac0-147">Add user context</span></span>](app-insights-usage-send-user-context.md)

