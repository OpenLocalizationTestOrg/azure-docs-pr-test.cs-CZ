---
title: "Analýza využití pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
description: "Porozumět, co dělají s vaší webové aplikace a jak pracují uživatelé."
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
ms.openlocfilehash: 63b74399790b718e14a5b6e09bc009a336caf928
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="bd923-103">Analýza využití pro webové aplikace pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="bd923-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="bd923-104">Funkce, které vaší webové aplikace je nejoblíbenější?</span><span class="sxs-lookup"><span data-stu-id="bd923-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="bd923-105">Vaši uživatelé dosáhli svých cílů s vaší aplikací?</span><span class="sxs-lookup"><span data-stu-id="bd923-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="bd923-106">Jejich vyřadit na konkrétní body a vracejí později?</span><span class="sxs-lookup"><span data-stu-id="bd923-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="bd923-107">[Azure Application Insights](app-insights-overview.md) umožňuje efektivní proniknout do jak lidí používá vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd923-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="bd923-108">Při každé aktualizaci aplikace můžete vyhodnotit, jak dobře funguje pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd923-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="bd923-109">Replikace můžete provést rozhodnutí o další cyklech vývoj řízených daty.</span><span class="sxs-lookup"><span data-stu-id="bd923-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="bd923-110">Odeslání telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd923-110">Send telemetry from your app</span></span>

<span data-ttu-id="bd923-111">Dosažení co nejlepších výsledků je získat nainstalováním Application Insights v kódu aplikace serveru i na webových stránkách.</span><span class="sxs-lookup"><span data-stu-id="bd923-111">The best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="bd923-112">Klientské a serverové součásti vaší aplikace odesílat telemetrii zpět na portál Azure pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="bd923-112">The client and server components of your app send telemetry back to the Azure portal for analysis.</span></span>

1. <span data-ttu-id="bd923-113">**Serverový kód:** nainstalovat modul vhodné pro vaše [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), nebo [jiných](app-insights-platforms.md) aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd923-113">**Server code:** Install the appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="bd923-114">*Nechcete instalovat serverový kód? Právě [vytvořte prostředek Azure Application Insights](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="bd923-114">*Don't want to install server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="bd923-115">**Kód webové stránky:** otevřete [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights pro vaši aplikaci a pak otevřete **Začínáme > sledování a diagnostikovat Client-Side**.</span><span class="sxs-lookup"><span data-stu-id="bd923-115">**Web page code:** Open the [Azure portal](https://portal.azure.com), open the Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Zkopírujte skript do head vaší hlavní webové stránky.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="bd923-117">**Získat telemetrie:** spusťte projekt v režimu ladění pro několik minut a potom vyhledejte výsledky v okně Přehled ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bd923-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in the Overview blade in Application Insights.</span></span>

    <span data-ttu-id="bd923-118">Publikování aplikace ke sledování výkonu vaší aplikace a zjistěte, co uživatelé dělají s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="bd923-118">Publish your app to monitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="bd923-119">Zahrnout ID uživatele a relace telemetrie</span><span class="sxs-lookup"><span data-stu-id="bd923-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="bd923-120">Sledování uživatelů v čase, Application Insights vyžaduje způsob, jak je identifikovat.</span><span class="sxs-lookup"><span data-stu-id="bd923-120">To track users over time, Application Insights requires a way to identify them.</span></span> <span data-ttu-id="bd923-121">Nástroj události je pouze využití nástroj, který nevyžaduje ID uživatele nebo ID relace.</span><span class="sxs-lookup"><span data-stu-id="bd923-121">The Events tool is the only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="bd923-122">Zahájit odesílání tyto identifikátory [zde](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="bd923-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="bd923-123">Prozkoumat využití demografické údaje a statistiky</span><span class="sxs-lookup"><span data-stu-id="bd923-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="bd923-124">Zjistí, pokud uživatelé používají vaši aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají.</span><span class="sxs-lookup"><span data-stu-id="bd923-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="bd923-125">Uživatele a relace sestavy filtr dat, stránky nebo vlastní události a je rozdělit pomocí vlastnosti, jako je například umístění prostředí a stránky.</span><span class="sxs-lookup"><span data-stu-id="bd923-125">The Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="bd923-126">Můžete také přidat vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="bd923-126">You can also add your own filters.</span></span>

![Uživatelé](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="bd923-128">Statistika na pravé straně bodu zajímavých vzorců v sadě dat.</span><span class="sxs-lookup"><span data-stu-id="bd923-128">Insights on the right point out interesting patterns in the set of data.</span></span>  

* <span data-ttu-id="bd923-129">**Uživatelé** sestava udává počet jedinečných uživatelů, kteří přístup k vaší stránky v rámci vašich vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="bd923-129">The **Users** report counts the numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="bd923-130">(Uživatelé, se počítají pomocí souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="bd923-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="bd923-131">Pokud někdo přistupuje k webu s jinou prohlížeče nebo klientské počítače nebo vymaže jejich souborů cookie, pak se bude počítat více než jednou.)</span><span class="sxs-lookup"><span data-stu-id="bd923-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="bd923-132">**Relací** sestava vrátí počet uživatelských relací, které přistupují k webu.</span><span class="sxs-lookup"><span data-stu-id="bd923-132">The **Sessions** report counts the number of user sessions that access your site.</span></span> <span data-ttu-id="bd923-133">Relace je období aktivity uživatelem, byl ukončen tečkou nečinnosti více než půl hodiny.</span><span class="sxs-lookup"><span data-stu-id="bd923-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="bd923-134">Další informace o nástroje uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="bd923-134">More about the Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="bd923-135">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="bd923-135">Page views</span></span>

<span data-ttu-id="bd923-136">V okně využití klikněte na tlačítko prostřednictvím zobrazení stránky dlaždicí a získejte rozpis nejoblíbenější stránky:</span><span class="sxs-lookup"><span data-stu-id="bd923-136">From the Usage blade, click through the Page Views tile to get a breakdown of your most popular pages:</span></span>

![V okně Přehled klikněte na graf zobrazení stránky](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="bd923-138">V předchozím příkladu je z webu hry.</span><span class="sxs-lookup"><span data-stu-id="bd923-138">The example above is from a games web site.</span></span> <span data-ttu-id="bd923-139">Z grafy okamžitě uvidíme:</span><span class="sxs-lookup"><span data-stu-id="bd923-139">From the charts, we can instantly see:</span></span>

* <span data-ttu-id="bd923-140">Využití nebyl vylepšené minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="bd923-140">Usage hasn't improved in the past week.</span></span> <span data-ttu-id="bd923-141">Možná jsme měli zamyslet optimalizaci pro vyhledávací weby?</span><span class="sxs-lookup"><span data-stu-id="bd923-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="bd923-142">Tenis je nejoblíbenější herní stránka.</span><span class="sxs-lookup"><span data-stu-id="bd923-142">Tennis is the most popular game page.</span></span> <span data-ttu-id="bd923-143">Umožňuje zaměřit se na další vylepšení na tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="bd923-143">Let's focus on further improvements to this page.</span></span>
* <span data-ttu-id="bd923-144">Uživatelé v průměru, najdete na stránce tenis o třikrát za týden.</span><span class="sxs-lookup"><span data-stu-id="bd923-144">On average, users visit the Tennis page about three times per week.</span></span> <span data-ttu-id="bd923-145">(Existují další o třikrát relace než uživatelů.)</span><span class="sxs-lookup"><span data-stu-id="bd923-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="bd923-146">Většina uživatelů přejděte na web během pracovní týden USA a v pracovní době.</span><span class="sxs-lookup"><span data-stu-id="bd923-146">Most users visit the site during the U.S. working week, and in working hours.</span></span> <span data-ttu-id="bd923-147">Možná jsme by měl poskytovat tlačítko "rychlý skrýt" na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="bd923-147">Perhaps we should provide a "quick hide" button on the web page.</span></span>
* <span data-ttu-id="bd923-148">[Poznámky](app-insights-annotations.md) na graf zobrazit, pokud byly nasazeny nové verze webu.</span><span class="sxs-lookup"><span data-stu-id="bd923-148">The [annotations](app-insights-annotations.md) on the chart show when new versions of the website were deployed.</span></span> <span data-ttu-id="bd923-149">Žádná z poslední nasazení měl znatelný vliv na využití.</span><span class="sxs-lookup"><span data-stu-id="bd923-149">None of the recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="bd923-150">Co dělat, pokud chcete prozkoumat provoz na váš web podrobněji, jako je rozdělení pomocí vlastní vlastnosti, které váš web odesílá v jeho telemetrická zobrazení stránky?</span><span class="sxs-lookup"><span data-stu-id="bd923-150">What if you want to investigate the traffic to your site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="bd923-151">Otevřete **události** nástroj v nabídce prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bd923-151">Open the **Events** tool in the Application Insights resource menu.</span></span> <span data-ttu-id="bd923-152">Tento nástroj umožňuje analyzovat, kolik zobrazení stránky a vlastních událostí, které byly odeslány z vaší aplikace na základě různých možností filtrování, cohorting a segmentace.</span><span class="sxs-lookup"><span data-stu-id="bd923-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="bd923-153">V rozevírací nabídce "Kdo používá" Vyberte "Any zobrazení stránky".</span><span class="sxs-lookup"><span data-stu-id="bd923-153">In the "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="bd923-154">V rozevírací nabídce "Rozdělení podle" Vyberte vlastnosti, podle kterého rozdělit vaší telemetrická zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="bd923-154">In the "Split by" dropdown, select a property by which to split your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="bd923-155">Uchování – počet uživatelů, kteří se vrátit?</span><span class="sxs-lookup"><span data-stu-id="bd923-155">Retention - how many users come back?</span></span>

<span data-ttu-id="bd923-156">Uchování pomáhá pochopit, jak často se vaši uživatelé vrátit používat své aplikace založené na kohorty uživatele, kteří provést některé akce obchodní během určité časové období.</span><span class="sxs-lookup"><span data-stu-id="bd923-156">Retention helps you understand how often your users return to use their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="bd923-157">Pochopit, jaké konkrétní funkce způsobit, že uživatelům se zpět více než jiné</span><span class="sxs-lookup"><span data-stu-id="bd923-157">Understand what specific features cause users to come back more than others</span></span> 
- <span data-ttu-id="bd923-158">Hypotézy formulář na základě reálného uživatelských dat</span><span class="sxs-lookup"><span data-stu-id="bd923-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="bd923-159">Zjistěte, jestli uchování problém ve vašem produkt</span><span class="sxs-lookup"><span data-stu-id="bd923-159">Determine whether retention is a problem in your product</span></span> 

![Uchovávání](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="bd923-161">Ovládací prvky uchovávání informací v horní části umožňují definovat konkrétních událostí a časový rozsah uchování vypočítat.</span><span class="sxs-lookup"><span data-stu-id="bd923-161">The retention controls on top allow you to define specific events and time range to calculate retention.</span></span> <span data-ttu-id="bd923-162">Graf uprostřed nabízí vizuální znázornění uchování procentuální podle zadaný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="bd923-162">The graph in the middle gives a visual representation of the overall retention percentage by the time range specified.</span></span> <span data-ttu-id="bd923-163">Graf na dolní představuje jednotlivé uchování v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="bd923-163">The graph on the bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="bd923-164">Tato úroveň podrobností můžete zjistit, co uživatelé dělají a co může mít vliv na stávajícím uživatelům na podrobnější členitosti.</span><span class="sxs-lookup"><span data-stu-id="bd923-164">This level of detail allows you to understand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="bd923-165">Informace o nástroji pro uchování</span><span class="sxs-lookup"><span data-stu-id="bd923-165">More about the Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="bd923-166">Vlastní obchodní události</span><span class="sxs-lookup"><span data-stu-id="bd923-166">Custom business events</span></span>

<span data-ttu-id="bd923-167">Potřebujete vědět, co uživatelé dělají s vaší webové aplikace, je užitečné k vložení řádků kódu do protokolu vlastních událostí.</span><span class="sxs-lookup"><span data-stu-id="bd923-167">To get a clear understanding of what users do with your web app, it's useful to insert lines of code to log custom events.</span></span> <span data-ttu-id="bd923-168">Tyto události můžete sledovat nic akcím podrobné uživatel kliknutím na konkrétní tlačítka na větších obchodní události, jako je například nakupování nebo vítězství.</span><span class="sxs-lookup"><span data-stu-id="bd923-168">These events can track anything from detailed user actions such as clicking specific buttons, to more significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="bd923-169">I když v některých případech může představovat zobrazení stránky užitečné události, není true obecně.</span><span class="sxs-lookup"><span data-stu-id="bd923-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="bd923-170">Uživatele můžete otevřít stránku produktu bez kdybyste kupovali produktu.</span><span class="sxs-lookup"><span data-stu-id="bd923-170">A user can open a product page without buying the product.</span></span> 

<span data-ttu-id="bd923-171">S konkrétní obchodní události můžete grafu vaši uživatelé průběh svého webu.</span><span class="sxs-lookup"><span data-stu-id="bd923-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="bd923-172">Můžete zjistit jejich předvoleb pro různé možnosti a tam, kde se vyřadit out nebo mít problémy.</span><span class="sxs-lookup"><span data-stu-id="bd923-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="bd923-173">Replikace můžete provést informované rozhodnutí o priority v backlogu vývoj.</span><span class="sxs-lookup"><span data-stu-id="bd923-173">With this knowledge, you can make informed decisions about the priorities in your development backlog.</span></span>

<span data-ttu-id="bd923-174">Na webové stránce mohou být protokolovány události:</span><span class="sxs-lookup"><span data-stu-id="bd923-174">Events can be logged in the web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="bd923-175">Nebo na straně serveru webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="bd923-175">Or in the server side of the web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="bd923-176">Hodnoty vlastností lze připojit k tyto události, takže můžete filtrovat nebo rozdělení událostí v případě, že si prohlédnout na portálu.</span><span class="sxs-lookup"><span data-stu-id="bd923-176">You can attach property values to these events, so that you can filter or split the events when you inspect them in the portal.</span></span> <span data-ttu-id="bd923-177">Kromě toho je standardní sada vlastností připojený k každé události, jako je například ID anonymního uživatele, které umožňuje sledovat posloupnost aktivit jednotlivého uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd923-177">In addition, a standard set of properties is attached to each event, such as anonymous user ID, which allows you to trace the sequence of activities of an individual user.</span></span>

<span data-ttu-id="bd923-178">Další informace o [vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent) a [vlastnosti](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="bd923-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="bd923-179">Nařezání a rozčlenění události</span><span class="sxs-lookup"><span data-stu-id="bd923-179">Slice and dice events</span></span>

<span data-ttu-id="bd923-180">V nástrojích pro uživatele, relace a události můžete řezu a rozčlenění vlastních událostí podle uživatele, název události a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bd923-180">In the Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="bd923-181">![Uživatelé](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="bd923-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-the-telemetry-with-the-app"></a><span data-ttu-id="bd923-182">Návrh telemetrie s aplikací</span><span class="sxs-lookup"><span data-stu-id="bd923-182">Design the telemetry with the app</span></span>

<span data-ttu-id="bd923-183">Při navrhování jednotlivých funkcí aplikace, zvažte, jak chcete měřit její úspěšnost s uživateli.</span><span class="sxs-lookup"><span data-stu-id="bd923-183">When you are designing each feature of your app, consider how you are going to measure its success with your users.</span></span> <span data-ttu-id="bd923-184">Rozhodněte, jaké obchodní události, které je potřeba zaznamenat a kód sledování volání pro tyto události do vaší aplikace od začátku.</span><span class="sxs-lookup"><span data-stu-id="bd923-184">Decide what business events you need to record, and code the tracking calls for those events into your app from the start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="bd923-185">A | Testování B</span><span class="sxs-lookup"><span data-stu-id="bd923-185">A | B Testing</span></span>
<span data-ttu-id="bd923-186">Pokud si nejste jisti, který variant funkce, která bude více úspěšné, uvolněte obou z nich, provádění jednotlivých přístupné pro různé uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd923-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible to different users.</span></span> <span data-ttu-id="bd923-187">Měřit její úspěšnost. pro každou z a poté přesuňte jednotná verzi.</span><span class="sxs-lookup"><span data-stu-id="bd923-187">Measure the success of each, and then move to a unified version.</span></span>

<span data-ttu-id="bd923-188">Pro tento postup můžete hodnot různých vlastností připojení k všechny telemetrická data, které odesílají každou verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd923-188">For this technique, you attach distinct property values to all the telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="bd923-189">Můžete to udělat tak, že definují vlastnosti v active TelemetryContext.</span><span class="sxs-lookup"><span data-stu-id="bd923-189">You can do that by defining properties in the active TelemetryContext.</span></span> <span data-ttu-id="bd923-190">Tyto výchozí vlastnosti jsou přidány do každé telemetrie zprávu, která aplikace odešle – nejen vlastní zprávy, ale také standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="bd923-190">These default properties are added to every telemetry message that the application sends - not just your custom messages, but the standard telemetry as well.</span></span>

<span data-ttu-id="bd923-191">V portálu služby Application Insights filtrovat a rozdělení dat na hodnoty vlastností, která porovnat různé verze.</span><span class="sxs-lookup"><span data-stu-id="bd923-191">In the Application Insights portal, filter and split your data on the property values, so as to compare the different versions.</span></span>

<span data-ttu-id="bd923-192">K tomu, [nastavení inicializátoru telemetrie](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="bd923-192">To do this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

<span data-ttu-id="bd923-193">Ve webové aplikaci inicializátoru například Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="bd923-193">In the web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="bd923-194">Všechny nové TelemetryClients automaticky přidat hodnotu vlastnosti, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="bd923-194">All new TelemetryClients automatically add the property value you specify.</span></span> <span data-ttu-id="bd923-195">Jednotlivé telemetrické události můžete přepsat výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bd923-195">Individual telemetry events can override the default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd923-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd923-196">Next steps</span></span>
   - [<span data-ttu-id="bd923-197">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="bd923-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="bd923-198">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="bd923-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="bd923-199">Uchování</span><span class="sxs-lookup"><span data-stu-id="bd923-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="bd923-200">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="bd923-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="bd923-201">Workbooks</span><span class="sxs-lookup"><span data-stu-id="bd923-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="bd923-202">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="bd923-202">Add user context</span></span>](app-insights-usage-send-user-context.md)
