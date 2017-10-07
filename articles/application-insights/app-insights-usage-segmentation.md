---
title: "Analýza aaaUser, relace a událostí ve službě Azure Application Insights | Microsoft docs"
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
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a><span data-ttu-id="fe4f9-103">Analýza uživatelů, relací a událostí ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="fe4f9-103">Users, sessions, and events analysis in Application Insights</span></span>

<span data-ttu-id="fe4f9-104">Zjistěte, kdy lidí používá vaši webovou aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-104">Find out when people use your web app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> <span data-ttu-id="fe4f9-105">Analyzovat obchodní a využití telemetrie pomocí [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe4f9-105">Analyze business and usage telemetry by using [Azure Application Insights](app-insights-overview.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="fe4f9-106">Začínáme</span><span class="sxs-lookup"><span data-stu-id="fe4f9-106">Get started</span></span>

<span data-ttu-id="fe4f9-107">Pokud nevidíte ještě data v hello uživatelů, relací nebo události okna portálu Application Insights hello [zjistěte, jak tooget pracovat s nástroji využití hello](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe4f9-107">If you don't yet see data in hello users, sessions, or events blades in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-users-sessions-and-events-segmentation-tool"></a><span data-ttu-id="fe4f9-108">Nástroj segmentaci uživatelů, relací a události Hello</span><span class="sxs-lookup"><span data-stu-id="fe4f9-108">hello Users, Sessions, and Events segmentation tool</span></span>

<span data-ttu-id="fe4f9-109">Tři hello využití pomocí okna hello stejné nástroj tooslice a rozčlenění telemetrie z vaší webové aplikace z tři perspektivy.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-109">Three of hello usage blades use hello same tool tooslice and dice telemetry from your web app from three perspectives.</span></span> <span data-ttu-id="fe4f9-110">Filtrování a rozdělení hello dat, můžete odkrýt přehledy o využití relativní hello různé stránky a funkcí.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-110">By filtering and splitting hello data, you can uncover insights about hello relative usage of different pages and features.</span></span>

* <span data-ttu-id="fe4f9-111">**Nástroj Uživatelé**: kolik lidí používá vaši aplikaci a jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-111">**Users tool**: How many people used your app and its features.</span></span>  <span data-ttu-id="fe4f9-112">Uživatelé, se počítají pomocí anonymní ID uložené v prohlížeči soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-112">Users are counted by using anonymous IDs stored in browser cookies.</span></span> <span data-ttu-id="fe4f9-113">Jednoho uživatele, kteří používají různé prohlížeče nebo počítače se budou počítat jako více než jeden uživatel.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-113">A single person using different browsers or machines will be counted as more than one user.</span></span>
* <span data-ttu-id="fe4f9-114">**Nástroj relací**: kolik relací aktivity uživatelů, zahrnuli určité stránky a funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-114">**Sessions tool**: How many sessions of user activity have included certain pages and features of your app.</span></span> <span data-ttu-id="fe4f9-115">Relace se počítá po půl hodiny nečinnosti uživatele nebo po průběžné 24h použití.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-115">A session is counted after half an hour of user inactivity, or after continuous 24h of use.</span></span>
* <span data-ttu-id="fe4f9-116">**Nástroj pro události**: jak často se používají určité stránky a funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-116">**Events tool**: How often certain pages and features of your app are used.</span></span> <span data-ttu-id="fe4f9-117">Zobrazení stránky se počítá při prohlížeč načte stránky z vaší aplikace, pokud máte [instrumentovány ho](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="fe4f9-117">A page view is counted when a browser loads a page from your app, provided you have [instrumented it](app-insights-javascript.md).</span></span> 

    <span data-ttu-id="fe4f9-118">Vlastní události představuje jeden výskyt něco stane ve vaší aplikaci, často interakci s uživatelem jako klikněte na tlačítko nebo hello dokončení některé úlohy.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-118">A custom event represents one occurrence of something happening in your app, often a user interaction like a button click or hello completion of some task.</span></span> <span data-ttu-id="fe4f9-119">Příliš vložení kódu v aplikaci[generovat vlastní události](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="fe4f9-119">You insert code in your app too[generate custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

![Použití nástroje](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a><span data-ttu-id="fe4f9-121">Dotaz na určité uživatele</span><span class="sxs-lookup"><span data-stu-id="fe4f9-121">Querying for Certain Users</span></span> 

<span data-ttu-id="fe4f9-122">Prozkoumejte různé skupiny uživatelů úpravou možností dotazu hello v horní části hello hello uživatelé nástroje:</span><span class="sxs-lookup"><span data-stu-id="fe4f9-122">Explore different groups of users by adjusting hello query options at hello top of hello Users tool:</span></span> 

* <span data-ttu-id="fe4f9-123">Kdo používá: Zvolte vlastní události a stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-123">Who used: Choose custom events and page views.</span></span> 
* <span data-ttu-id="fe4f9-124">Během: Vyberte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-124">During: Choose a time range.</span></span> 
* <span data-ttu-id="fe4f9-125">Podle: Vyberte, jak toobucket hello dat, buď v časovém intervalu, nebo jinou vlastnost třeba prohlížeče nebo města.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-125">By: Choose how toobucket hello data, either by a period of time or by another property such as browser or city.</span></span> 
* <span data-ttu-id="fe4f9-126">Rozdělit: Zvolte vlastnosti a datových hello toosplit nebo segmentu.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-126">Split By: Choose a property by which toosplit or segment hello data.</span></span> 
* <span data-ttu-id="fe4f9-127">Přidání filtrů: Omezit hello dotazu toocertain uživatelů, relací nebo události na základě jejich vlastností, jako je například prohlížeč nebo města.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-127">Add Filters: Limit hello query toocertain users, sessions, or events based on their properties, such as browser or city.</span></span> 
 
## <a name="saving-and-sharing-reports"></a><span data-ttu-id="fe4f9-128">Ukládání a sdílení sestavy</span><span class="sxs-lookup"><span data-stu-id="fe4f9-128">Saving and sharing reports</span></span> 
<span data-ttu-id="fe4f9-129">Uživatelé sestavy, buď soukromé jenom tooyou v části Moje sestavy hello můžete uložit nebo sdíleny se všemi ostatními s toothis přístup k prostředku Application Insights v hello část sdílené sestavy.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-129">You can save Users reports, either private just tooyou in hello My Reports section, or shared with everyone else with access toothis Application Insights resource in hello Shared Reports section.</span></span>  
 
<span data-ttu-id="fe4f9-130">Při ukládání sestavy nebo úpravou jeho vlastnosti, vyberte toosave "Aktuální relativní časové rozmezí", které sestavy bude nepřetržitě aktualizovat data, návratem některé pevné množství času.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-130">While saving a report or editing its properties, choose "Current Relative Time Range" toosave a report will continuously refreshed data, going back some fixed amount of time.</span></span>  
 
<span data-ttu-id="fe4f9-131">Vyberte sestavu s pevnou sadu dat toosave "Aktuální absolutní časový rozsah".</span><span class="sxs-lookup"><span data-stu-id="fe4f9-131">Choose "Current Absolute Time Range" toosave a report with a fixed set of data.</span></span> <span data-ttu-id="fe4f9-132">Mějte na paměti, že data ve službě Application Insights je uložena pouze po dobu 90 dnů, takže pokud uplynulo víc než 90 dní od sestavy s rozsahem absolutním čase byla uložena, hello sestavy se zobrazí prázdné.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-132">Keep in mind that data in Application Insights is only stored for 90 days, so if more than 90 days have passed since a report with an absolute time range was saved, hello report will appear empty.</span></span> 
 
## <a name="example-instances"></a><span data-ttu-id="fe4f9-133">Příklad instancí</span><span class="sxs-lookup"><span data-stu-id="fe4f9-133">Example instances</span></span>

<span data-ttu-id="fe4f9-134">Hello oddílu instancí příklad zobrazuje informace o několik jednotlivých uživatelů, relací a události, které jsou porovnávány pomocí hello aktuální dotaz.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-134">hello Example instances section shows information about a handful of individual users, sessions, or events that are matched by hello current query.</span></span> <span data-ttu-id="fe4f9-135">Vzhledem k tomu a prohlížení hello chování jednotlivce v přidání tooaggregates může poskytovat přehled o tom, jak lidé ve skutečnosti používají vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-135">Considering and exploring hello behaviors of individuals, in addition tooaggregates, can provide insights about how people actually use your app.</span></span> 
 
## <a name="insights"></a><span data-ttu-id="fe4f9-136">Insights</span><span class="sxs-lookup"><span data-stu-id="fe4f9-136">Insights</span></span> 

<span data-ttu-id="fe4f9-137">Statistika Hello bočním panelu ukazuje velkých clusterech uživatelů, které sdílejí společné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-137">hello Insights sidebar shows large clusters of users that share common properties.</span></span> <span data-ttu-id="fe4f9-138">Tyto clustery odkrýt překvapivé trendy v tom, jak uživatelé používají vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-138">These clusters can uncover surprising trends in how people use your app.</span></span> <span data-ttu-id="fe4f9-139">Pokud například 40 % všech hello využití aplikace pochází z uživatelé, kteří používají jeden funkce.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-139">For example, if 40% of all of hello usage of your app comes from people using a single feature.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="fe4f9-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe4f9-140">Next steps</span></span>
- <span data-ttu-id="fe4f9-141">využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="fe4f9-141">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="fe4f9-142">Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="fe4f9-142">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="fe4f9-143">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="fe4f9-143">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="fe4f9-144">Uchování</span><span class="sxs-lookup"><span data-stu-id="fe4f9-144">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="fe4f9-145">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="fe4f9-145">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="fe4f9-146">Workbooks</span><span class="sxs-lookup"><span data-stu-id="fe4f9-146">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="fe4f9-147">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="fe4f9-147">Add user context</span></span>](app-insights-usage-send-user-context.md)

