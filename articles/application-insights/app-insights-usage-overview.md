---
title: "Analýza aaaUsage pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
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
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="8a039-103">Analýza využití pro webové aplikace pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="8a039-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="8a039-104">Funkce, které vaší webové aplikace je nejoblíbenější?</span><span class="sxs-lookup"><span data-stu-id="8a039-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="8a039-105">Vaši uživatelé dosáhli svých cílů s vaší aplikací?</span><span class="sxs-lookup"><span data-stu-id="8a039-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="8a039-106">Jejich vyřadit na konkrétní body a vracejí později?</span><span class="sxs-lookup"><span data-stu-id="8a039-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="8a039-107">[Azure Application Insights](app-insights-overview.md) umožňuje efektivní proniknout do jak lidí používá vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a039-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="8a039-108">Při každé aktualizaci aplikace můžete vyhodnotit, jak dobře funguje pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a039-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="8a039-109">Replikace můžete provést rozhodnutí o další cyklech vývoj řízených daty.</span><span class="sxs-lookup"><span data-stu-id="8a039-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="8a039-110">Odeslání telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a039-110">Send telemetry from your app</span></span>

<span data-ttu-id="8a039-111">Hello co nejlepších výsledků je získat nainstalováním Application Insights v kódu aplikace serveru i na webových stránkách.</span><span class="sxs-lookup"><span data-stu-id="8a039-111">hello best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="8a039-112">Hello součásti klienta a serveru vaší aplikace odesílat telemetrii back toohello portál Azure pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="8a039-112">hello client and server components of your app send telemetry back toohello Azure portal for analysis.</span></span>

1. <span data-ttu-id="8a039-113">**Serverový kód:** instalace hello příslušný modul pro vaše [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), nebo [jiných](app-insights-platforms.md) aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a039-113">**Server code:** Install hello appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="8a039-114">*Nechcete tooinstall serverový kód? Právě [vytvořte prostředek Azure Application Insights](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="8a039-114">*Don't want tooinstall server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="8a039-115">**Kód webové stránky:** otevřete hello [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights hello pro vaši aplikaci a pak otevřete **Začínáme > sledování a diagnostikovat Client-Side**.</span><span class="sxs-lookup"><span data-stu-id="8a039-115">**Web page code:** Open hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Zkopírujte skript hello do hello head vaší hlavní webové stránky.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="8a039-117">**Získat telemetrie:** spusťte projekt v režimu ladění pro několik minut a potom vyhledejte výsledky v okně Přehled hello ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8a039-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in hello Overview blade in Application Insights.</span></span>

    <span data-ttu-id="8a039-118">Publikování vaši aplikaci toomonitor výkon vaší aplikace a zjistit, co uživatelé dělají s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="8a039-118">Publish your app toomonitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="8a039-119">Zahrnout ID uživatele a relace telemetrie</span><span class="sxs-lookup"><span data-stu-id="8a039-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="8a039-120">Uživatelé tootrack v čase, Application Insights vyžaduje tooidentify způsob, jak je.</span><span class="sxs-lookup"><span data-stu-id="8a039-120">tootrack users over time, Application Insights requires a way tooidentify them.</span></span> <span data-ttu-id="8a039-121">Hello události, které je nástroj hello pouze využití nástroj, který nevyžaduje ID uživatele nebo ID relace.</span><span class="sxs-lookup"><span data-stu-id="8a039-121">hello Events tool is hello only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="8a039-122">Zahájit odesílání tyto identifikátory [zde](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="8a039-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="8a039-123">Prozkoumat využití demografické údaje a statistiky</span><span class="sxs-lookup"><span data-stu-id="8a039-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="8a039-124">Zjistí, pokud uživatelé používají vaši aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají.</span><span class="sxs-lookup"><span data-stu-id="8a039-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="8a039-125">Hello uživatelů a relací sestavy filtr dat, stránky nebo vlastní události a je rozdělit pomocí vlastnosti, jako je například umístění prostředí a stránky.</span><span class="sxs-lookup"><span data-stu-id="8a039-125">hello Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="8a039-126">Můžete také přidat vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="8a039-126">You can also add your own filters.</span></span>

![Uživatelé](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="8a039-128">Statistika na pravém hello bodu zajímavých vzorců v sadě dat hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-128">Insights on hello right point out interesting patterns in hello set of data.</span></span>  

* <span data-ttu-id="8a039-129">Hello **uživatelé** sestavy počty hello počet jedinečných uživatelů, kteří přístup k vaší stránky v rámci vašich vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="8a039-129">hello **Users** report counts hello numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="8a039-130">(Uživatelé, se počítají pomocí souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="8a039-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="8a039-131">Pokud někdo přistupuje k webu s jinou prohlížeče nebo klientské počítače nebo vymaže jejich souborů cookie, pak se bude počítat více než jednou.)</span><span class="sxs-lookup"><span data-stu-id="8a039-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="8a039-132">Hello **relací** sestavy počty hello počet uživatelských relací, které přistupují k webu.</span><span class="sxs-lookup"><span data-stu-id="8a039-132">hello **Sessions** report counts hello number of user sessions that access your site.</span></span> <span data-ttu-id="8a039-133">Relace je období aktivity uživatelem, byl ukončen tečkou nečinnosti více než půl hodiny.</span><span class="sxs-lookup"><span data-stu-id="8a039-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="8a039-134">Další informace o hello uživatelů, relací a události nástroje</span><span class="sxs-lookup"><span data-stu-id="8a039-134">More about hello Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="8a039-135">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="8a039-135">Page views</span></span>

<span data-ttu-id="8a039-136">V okně hello využití klikněte na tlačítko prostřednictvím hello zobrazení stránky dlaždici tooget rozpis nejoblíbenější stránky:</span><span class="sxs-lookup"><span data-stu-id="8a039-136">From hello Usage blade, click through hello Page Views tile tooget a breakdown of your most popular pages:</span></span>

![V okně Přehled hello klikněte na tlačítko hello stránky zobrazení grafu](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="8a039-138">výše uvedený příklad Hello je z webu hry.</span><span class="sxs-lookup"><span data-stu-id="8a039-138">hello example above is from a games web site.</span></span> <span data-ttu-id="8a039-139">Z hello grafy okamžitě uvidíme:</span><span class="sxs-lookup"><span data-stu-id="8a039-139">From hello charts, we can instantly see:</span></span>

* <span data-ttu-id="8a039-140">Využití nebyl v hello vylepšené minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="8a039-140">Usage hasn't improved in hello past week.</span></span> <span data-ttu-id="8a039-141">Možná jsme měli zamyslet optimalizaci pro vyhledávací weby?</span><span class="sxs-lookup"><span data-stu-id="8a039-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="8a039-142">Tenis je nejoblíbenější herní stránku hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-142">Tennis is hello most popular game page.</span></span> <span data-ttu-id="8a039-143">Umožňuje zaměřit se na další stránce toothis vylepšení.</span><span class="sxs-lookup"><span data-stu-id="8a039-143">Let's focus on further improvements toothis page.</span></span>
* <span data-ttu-id="8a039-144">Uživatelé v průměru, navštivte stránku hello tenis o třikrát za týden.</span><span class="sxs-lookup"><span data-stu-id="8a039-144">On average, users visit hello Tennis page about three times per week.</span></span> <span data-ttu-id="8a039-145">(Existují další o třikrát relace než uživatelů.)</span><span class="sxs-lookup"><span data-stu-id="8a039-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="8a039-146">Většina uživatelů získáte hello serveru během hello USA pracovní týden a v pracovní době.</span><span class="sxs-lookup"><span data-stu-id="8a039-146">Most users visit hello site during hello U.S. working week, and in working hours.</span></span> <span data-ttu-id="8a039-147">Možná jsme by měl poskytovat tlačítko "rychlý skrýt" na webové stránce hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-147">Perhaps we should provide a "quick hide" button on hello web page.</span></span>
* <span data-ttu-id="8a039-148">Hello [poznámky](app-insights-annotations.md) v grafu hello zobrazit, pokud byly nasazeny nové verze hello webu.</span><span class="sxs-lookup"><span data-stu-id="8a039-148">hello [annotations](app-insights-annotations.md) on hello chart show when new versions of hello website were deployed.</span></span> <span data-ttu-id="8a039-149">Žádná z poslední nasazení hello měl znatelný vliv na využití.</span><span class="sxs-lookup"><span data-stu-id="8a039-149">None of hello recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="8a039-150">Co dělat, když chcete tooinvestigate hello tooyour provozem podrobněji, jako je rozdělení pomocí vlastní vlastnosti, které váš web odesílá v jeho telemetrická zobrazení stránky?</span><span class="sxs-lookup"><span data-stu-id="8a039-150">What if you want tooinvestigate hello traffic tooyour site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="8a039-151">Otevřete hello **události** nástroj v nabídce prostředku Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-151">Open hello **Events** tool in hello Application Insights resource menu.</span></span> <span data-ttu-id="8a039-152">Tento nástroj umožňuje analyzovat, kolik zobrazení stránky a vlastních událostí, které byly odeslány z vaší aplikace na základě různých možností filtrování, cohorting a segmentace.</span><span class="sxs-lookup"><span data-stu-id="8a039-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="8a039-153">V rozevírací nabídce "Kdo používá" hello vyberte "Any zobrazení stránky".</span><span class="sxs-lookup"><span data-stu-id="8a039-153">In hello "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="8a039-154">V rozevírací nabídce "Rozdělení podle" hello vyberte vlastnost, pomocí které toosplit stránku zobrazit telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="8a039-154">In hello "Split by" dropdown, select a property by which toosplit your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="8a039-155">Uchování – počet uživatelů, kteří se vrátit?</span><span class="sxs-lookup"><span data-stu-id="8a039-155">Retention - how many users come back?</span></span>

<span data-ttu-id="8a039-156">Uchování pomáhá pochopit, jak často uživatelům vrátit toouse své aplikace založené na kohorty uživatele, kteří provést některé akce obchodní během určité časové období.</span><span class="sxs-lookup"><span data-stu-id="8a039-156">Retention helps you understand how often your users return toouse their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="8a039-157">Pochopit, jaké konkrétní funkce způsobit uživatelé toocome zpět více než jiné</span><span class="sxs-lookup"><span data-stu-id="8a039-157">Understand what specific features cause users toocome back more than others</span></span> 
- <span data-ttu-id="8a039-158">Hypotézy formulář na základě reálného uživatelských dat</span><span class="sxs-lookup"><span data-stu-id="8a039-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="8a039-159">Zjistěte, jestli uchování problém ve vašem produkt</span><span class="sxs-lookup"><span data-stu-id="8a039-159">Determine whether retention is a problem in your product</span></span> 

![Uchovávání](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="8a039-161">ovládací prvky uchování Hello nahoře povolit jste toodefine konkrétních událostí a čas rozsah toocalculate uchování.</span><span class="sxs-lookup"><span data-stu-id="8a039-161">hello retention controls on top allow you toodefine specific events and time range toocalculate retention.</span></span> <span data-ttu-id="8a039-162">Hello grafu uprostřed hello nabízí vizuální znázornění hello celkové procento uchování podle zadaný časový rozsah hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-162">hello graph in hello middle gives a visual representation of hello overall retention percentage by hello time range specified.</span></span> <span data-ttu-id="8a039-163">Graf Hello na hello dolní představuje jednotlivé uchování v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="8a039-163">hello graph on hello bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="8a039-164">Tato úroveň podrobností můžete toounderstand co uživatelé dělají a co může mít vliv na stávajícím uživatelům na podrobnější členitosti.</span><span class="sxs-lookup"><span data-stu-id="8a039-164">This level of detail allows you toounderstand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="8a039-165">Informace o nástroji pro uchování hello</span><span class="sxs-lookup"><span data-stu-id="8a039-165">More about hello Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="8a039-166">Vlastní obchodní události</span><span class="sxs-lookup"><span data-stu-id="8a039-166">Custom business events</span></span>

<span data-ttu-id="8a039-167">tooget vědět, co uživatelé dělají s vaší aplikací webové, je užitečné tooinsert řádků kódu toolog vlastních událostí.</span><span class="sxs-lookup"><span data-stu-id="8a039-167">tooget a clear understanding of what users do with your web app, it's useful tooinsert lines of code toolog custom events.</span></span> <span data-ttu-id="8a039-168">Tyto události můžete sledovat nic akcím podrobné uživatel kliknutím na konkrétní tlačítka, toomore významné obchodní události například nakupování nebo vítězství.</span><span class="sxs-lookup"><span data-stu-id="8a039-168">These events can track anything from detailed user actions such as clicking specific buttons, toomore significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="8a039-169">I když v některých případech může představovat zobrazení stránky užitečné události, není true obecně.</span><span class="sxs-lookup"><span data-stu-id="8a039-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="8a039-170">Uživatele můžete otevřít stránku produktu bez kdybyste kupovali hello produktu.</span><span class="sxs-lookup"><span data-stu-id="8a039-170">A user can open a product page without buying hello product.</span></span> 

<span data-ttu-id="8a039-171">S konkrétní obchodní události můžete grafu vaši uživatelé průběh svého webu.</span><span class="sxs-lookup"><span data-stu-id="8a039-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="8a039-172">Můžete zjistit jejich předvoleb pro různé možnosti a tam, kde se vyřadit out nebo mít problémy.</span><span class="sxs-lookup"><span data-stu-id="8a039-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="8a039-173">Replikace můžete provést informované rozhodnutí o hello priority v backlogu vývoj.</span><span class="sxs-lookup"><span data-stu-id="8a039-173">With this knowledge, you can make informed decisions about hello priorities in your development backlog.</span></span>

<span data-ttu-id="8a039-174">Události může být přihlášen hello webové stránky:</span><span class="sxs-lookup"><span data-stu-id="8a039-174">Events can be logged in hello web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="8a039-175">Nebo v hello hello webové aplikace na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="8a039-175">Or in hello server side of hello web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="8a039-176">Vlastnost hodnoty toothese události, můžete připojit, takže můžete filtrovat nebo rozdělení hello událostí v případě, že si prohlédnout hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8a039-176">You can attach property values toothese events, so that you can filter or split hello events when you inspect them in hello portal.</span></span> <span data-ttu-id="8a039-177">Kromě toho je standardní sada vlastností připojené tooeach události, jako je například ID anonymního uživatele, které vám umožní tootrace hello posloupnost aktivit jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a039-177">In addition, a standard set of properties is attached tooeach event, such as anonymous user ID, which allows you tootrace hello sequence of activities of an individual user.</span></span>

<span data-ttu-id="8a039-178">Další informace o [vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent) a [vlastnosti](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="8a039-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="8a039-179">Nařezání a rozčlenění události</span><span class="sxs-lookup"><span data-stu-id="8a039-179">Slice and dice events</span></span>

<span data-ttu-id="8a039-180">V nástrojích hello uživatelů, relací a události můžete řezu a rozčlenění vlastních událostí podle uživatele, název události a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8a039-180">In hello Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="8a039-181">![Uživatelé](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="8a039-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-hello-telemetry-with-hello-app"></a><span data-ttu-id="8a039-182">Návrh hello telemetrie aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8a039-182">Design hello telemetry with hello app</span></span>

<span data-ttu-id="8a039-183">Při navrhování jednotlivých funkcí aplikace, zvažte, jak budete toomeasure jeho úspěch s uživateli.</span><span class="sxs-lookup"><span data-stu-id="8a039-183">When you are designing each feature of your app, consider how you are going toomeasure its success with your users.</span></span> <span data-ttu-id="8a039-184">Rozhodněte, jaké obchodní události je třeba toorecord a kód hello sledování volání pro tyto události do vaší aplikace z hello spustit.</span><span class="sxs-lookup"><span data-stu-id="8a039-184">Decide what business events you need toorecord, and code hello tracking calls for those events into your app from hello start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="8a039-185">A | Testování B</span><span class="sxs-lookup"><span data-stu-id="8a039-185">A | B Testing</span></span>
<span data-ttu-id="8a039-186">Pokud si nejste jisti, který variant funkce, která bude více úspěšné, uvolněte obou z nich, provádění každé přístupné toodifferent uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a039-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible toodifferent users.</span></span> <span data-ttu-id="8a039-187">Měření úspěchu hello jednotlivých a poté přesuňte tooa jednotná verze.</span><span class="sxs-lookup"><span data-stu-id="8a039-187">Measure hello success of each, and then move tooa unified version.</span></span>

<span data-ttu-id="8a039-188">Pro tento postup připojte jedinečné vlastnosti hodnoty tooall hello telemetrie, které odesílají každou verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a039-188">For this technique, you attach distinct property values tooall hello telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="8a039-189">Můžete to udělat tak, že definují vlastnosti v hello active TelemetryContext.</span><span class="sxs-lookup"><span data-stu-id="8a039-189">You can do that by defining properties in hello active TelemetryContext.</span></span> <span data-ttu-id="8a039-190">Tyto výchozí vlastnosti jsou přidány, odešle zprávu tooevery telemetrická data, která hello aplikace – nejen vaše vlastní zprávy, ale hello standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="8a039-190">These default properties are added tooevery telemetry message that hello application sends - not just your custom messages, but hello standard telemetry as well.</span></span>

<span data-ttu-id="8a039-191">Hello portálu Application Insights filtrovat a rozdělení dat na hodnoty vlastností hello, takže jako toocompare hello různé verze.</span><span class="sxs-lookup"><span data-stu-id="8a039-191">In hello Application Insights portal, filter and split your data on hello property values, so as toocompare hello different versions.</span></span>

<span data-ttu-id="8a039-192">toodo, [nastavení inicializátoru telemetrie](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="8a039-192">toodo this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

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

<span data-ttu-id="8a039-193">V hello webové aplikace inicializátoru například Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="8a039-193">In hello web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="8a039-194">Všechny nové TelemetryClients automaticky přidají hello hodnotu vlastnosti, které určíte.</span><span class="sxs-lookup"><span data-stu-id="8a039-194">All new TelemetryClients automatically add hello property value you specify.</span></span> <span data-ttu-id="8a039-195">Jednotlivé telemetrické události můžete přepsat výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="8a039-195">Individual telemetry events can override hello default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a039-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a039-196">Next steps</span></span>
   - [<span data-ttu-id="8a039-197">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="8a039-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="8a039-198">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="8a039-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="8a039-199">Uchování</span><span class="sxs-lookup"><span data-stu-id="8a039-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="8a039-200">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="8a039-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="8a039-201">Workbooks</span><span class="sxs-lookup"><span data-stu-id="8a039-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="8a039-202">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="8a039-202">Add user context</span></span>](app-insights-usage-send-user-context.md)
