---
title: "Pomocí vyhledávání ve službě Azure Application Insights | Microsoft Docs"
description: "Hledat a filtr nezpracovaná telemetrická data odesílaná vaší webové aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e2d12f807756b778a64920b12a66fba184a99844
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="3541c-103">Pomocí vyhledávání ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="3541c-103">Using Search in Application Insights</span></span>
<span data-ttu-id="3541c-104">Hledání je funkce [Application Insights](app-insights-overview.md) můžete použít k vyhledání a prozkoumejte telemetrie jednotlivých položek, například zobrazení stránky, výjimky nebo k webové požadavky.</span><span class="sxs-lookup"><span data-stu-id="3541c-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use to find and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="3541c-105">A můžete zobrazit protokol trasování a události, které mají programového.</span><span class="sxs-lookup"><span data-stu-id="3541c-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="3541c-106">(Pro složitější dotazy na data, použijte [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="3541c-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="3541c-107">Kde uvidíte hledání?</span><span class="sxs-lookup"><span data-stu-id="3541c-107">Where do you see Search?</span></span>
### <a name="in-the-azure-portal"></a><span data-ttu-id="3541c-108">Na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3541c-108">In the Azure portal</span></span>
<span data-ttu-id="3541c-109">Diagnostické vyhledávání můžete otevřít explicitně v okně Přehled Application Insights vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="3541c-109">You can open diagnostic search explicitly from the Application Insights Overview blade of your application:</span></span>

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="3541c-111">Otevře také po kliknutí na tlačítko prostřednictvím některé grafy a položek mřížky.</span><span class="sxs-lookup"><span data-stu-id="3541c-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="3541c-112">V takovém případě jeho filtry jsou předem nastavené a zaměřit se na typ položky, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3541c-112">In this case, its filters are pre-set to focus on the type of item you selected.</span></span> 

<span data-ttu-id="3541c-113">Například v okně Přehled není pruhový graf požadavků, které jsou klasifikovány podle doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="3541c-113">For example, on the Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="3541c-114">Proklikejte se prostřednictvím rozsahu výkonu zobrazíte seznam jednotlivých požadavků v tomto rozsahu čas odezvy:</span><span class="sxs-lookup"><span data-stu-id="3541c-114">Click through a performance range to see a list of individual requests in that response time range:</span></span>

![Proklikejte se žádost o výkonu](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="3541c-116">Hlavní část diagnostické vyhledávání je seznam položek telemetrie – požadavky na server, vyberte zobrazení, vlastních událostí, které mají programového a tak dále.</span><span class="sxs-lookup"><span data-stu-id="3541c-116">The main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="3541c-117">V horní části seznamu je souhrn graf zobrazující počet událostí v čase.</span><span class="sxs-lookup"><span data-stu-id="3541c-117">At the top of the list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="3541c-118">Klikněte na tlačítko Aktualizovat se získat nové události.</span><span class="sxs-lookup"><span data-stu-id="3541c-118">Click Refresh to get new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="3541c-119">V nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3541c-119">In Visual Studio</span></span>

<span data-ttu-id="3541c-120">V sadě Visual Studio je také hledání Application Insights okno.</span><span class="sxs-lookup"><span data-stu-id="3541c-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="3541c-121">Je velmi užitečné pro zobrazení telemetrie události generované aplikací, kterou ladíte.</span><span class="sxs-lookup"><span data-stu-id="3541c-121">It's most useful for displaying telemetry events generated by the application that you're debugging.</span></span> <span data-ttu-id="3541c-122">Ale může také zobrazit události shromážděné z publikované aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3541c-122">But it can also show the events collected from your published app at the Azure portal.</span></span>

<span data-ttu-id="3541c-123">Otevřete okno hledání v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3541c-123">Open the Search window in Visual Studio:</span></span>

![Otevřete Visual Studio Application Insights vyhledávání](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="3541c-125">V okně vyhledávání má funkce podobné webového portálu:</span><span class="sxs-lookup"><span data-stu-id="3541c-125">The Search window has features similar to the web portal:</span></span>

![Visual Studio Application Insights vyhledávání oken](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="3541c-127">Na kartě Sledování operaci je k dispozici při otevření žádost nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="3541c-127">The Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="3541c-128">' Operace, je posloupnost událostí, který je přidružen k jediné zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="3541c-128">An 'operation' is a sequence of events that is associated with to a single request or page view.</span></span> <span data-ttu-id="3541c-129">Například volání závislostí, výjimek, protokoly trasování a vlastních událostí může být součástí jedné operace.</span><span class="sxs-lookup"><span data-stu-id="3541c-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="3541c-130">Na kartě Sledování operace zobrazí graficky načasování a dobu trvání tyto události ve vztahu k zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="3541c-130">The Track Operation tab shows graphically the timing and duration of these events in relation to the request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="3541c-131">Zkontrolujte jednotlivé položky</span><span class="sxs-lookup"><span data-stu-id="3541c-131">Inspect individual items</span></span>
<span data-ttu-id="3541c-132">Vyberte libovolnou položku telemetrie zobrazíte klíčová pole a související položky.</span><span class="sxs-lookup"><span data-stu-id="3541c-132">Select any telemetry item to see key fields and related items.</span></span> <span data-ttu-id="3541c-133">Pokud chcete zobrazit úplnou sadu pole, klikněte na tlačítko "...".</span><span class="sxs-lookup"><span data-stu-id="3541c-133">If you want to see the full set of fields, click "...".</span></span> 

![Klikněte na novou pracovní položku, upravte pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="3541c-135">Filtrování typů událostí</span><span class="sxs-lookup"><span data-stu-id="3541c-135">Filter event types</span></span>
<span data-ttu-id="3541c-136">Otevřete okno filtru a vyberte typy událostí, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="3541c-136">Open the Filter blade and choose the event types you want to see.</span></span> <span data-ttu-id="3541c-137">(Pokud chcete později, obnovte filtry, pomocí kterých můžete otevřít okno, klikněte na resetovat.)</span><span class="sxs-lookup"><span data-stu-id="3541c-137">(If, later, you want to restore the filters with which you opened the blade, click Reset.)</span></span>

![Vyberte filtr a vyberte typy telemetrie](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="3541c-139">Typy událostí jsou:</span><span class="sxs-lookup"><span data-stu-id="3541c-139">The event types are:</span></span>

* <span data-ttu-id="3541c-140">**Trasování** - [diagnostické protokoly](app-insights-asp-net-trace-logs.md) včetně TrackTrace, log4Net, NLog a System.Diagnostic.Trace volání.</span><span class="sxs-lookup"><span data-stu-id="3541c-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="3541c-141">**Žádosti o** -požadavky HTTP přijatých vaší serverové aplikace, včetně stránky, skripty, bitové kopie, soubory stylu a data.</span><span class="sxs-lookup"><span data-stu-id="3541c-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="3541c-142">Tyto události se používají k vytvoření žádosti a odpovědi tabulkách Přehled.</span><span class="sxs-lookup"><span data-stu-id="3541c-142">These events are used to create the request and response overview charts.</span></span>
* <span data-ttu-id="3541c-143">**Zobrazení stránky** - [Telemetrie odesílaný klientem webové](app-insights-javascript.md)používané k vytváření sestav zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="3541c-143">**Page View** - [Telemetry sent by the web client](app-insights-javascript.md), used to create page view reports.</span></span> 
* <span data-ttu-id="3541c-144">**Vlastní události** – Pokud jste vložili volání TrackEvent() za účelem [sledování využití](app-insights-api-custom-events-metrics.md), můžete je zde prohledat.</span><span class="sxs-lookup"><span data-stu-id="3541c-144">**Custom Event** - If you inserted calls to TrackEvent() in order to [monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="3541c-145">**Výjimka** – nezachycená [výjimky na serveru](app-insights-asp-net-exceptions.md)a ty, které se můžete přihlásit pomocí TrackException().</span><span class="sxs-lookup"><span data-stu-id="3541c-145">**Exception** - Uncaught [exceptions in the server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="3541c-146">**Závislost** - [volání ze serveru aplikace](app-insights-asp-net-dependencies.md) k jiným službám, jako je například rozhraní REST API nebo databáze a AJAX volá z vaší [kód klienta](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) to other services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="3541c-147">**Dostupnost** -výsledky [testy dostupnosti](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="3541c-148">Filtrovat podle hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="3541c-148">Filter on property values</span></span>
<span data-ttu-id="3541c-149">Můžete filtrovat události na hodnotách jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3541c-149">You can filter events on the values of their properties.</span></span> <span data-ttu-id="3541c-150">Dostupné vlastnosti závisí na typy událostí, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3541c-150">The available properties depend on the event types you selected.</span></span> 

<span data-ttu-id="3541c-151">Můžete například vyberte požadavků s kódem konkrétní odezvy.</span><span class="sxs-lookup"><span data-stu-id="3541c-151">For example, pick out requests with a specific response code.</span></span> 

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="3541c-153">Výběr žádné hodnoty určité vlastnosti má stejný účinek jako volba všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3541c-153">Choosing no values of a particular property has the same effect as choosing all values.</span></span> <span data-ttu-id="3541c-154">Přepíná vypnout filtrování pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3541c-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="3541c-155">Upřesněte hledání</span><span class="sxs-lookup"><span data-stu-id="3541c-155">Narrow your search</span></span>
<span data-ttu-id="3541c-156">Všimněte si, že počty napravo od hodnoty filtru zobrazit, kolik výskytů existuje jsou v aktuální sadě filtrované.</span><span class="sxs-lookup"><span data-stu-id="3541c-156">Notice that the counts to the right of the filter values show how many occurrences there are in the current filtered set.</span></span> 

<span data-ttu-id="3541c-157">V tomto příkladu je jasné, že 'RTP – nebo zaměstnancům, vyžádat výsledky ve většině "500" chyb:</span><span class="sxs-lookup"><span data-stu-id="3541c-157">In this example, it's clear that the 'Rpt/Employees' request results in most of the '500' errors:</span></span>

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a><span data-ttu-id="3541c-159">Najít události se stejnou vlastnost</span><span class="sxs-lookup"><span data-stu-id="3541c-159">Find events with the same property</span></span>
<span data-ttu-id="3541c-160">Najít všechny položky se stejnou hodnotou vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3541c-160">Find all the items with the same property value:</span></span>

![Klikněte pravým tlačítkem na vlastnosti](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a><span data-ttu-id="3541c-162">Vyhledávání dat</span><span class="sxs-lookup"><span data-stu-id="3541c-162">Search the data</span></span>

> [!NOTE]
> <span data-ttu-id="3541c-163">Chcete-li psát složitější dotazy, otevřete [ **Analytics** ](app-insights-analytics-tour.md) z horní části okna vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="3541c-163">To write more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from the top of the Search blade.</span></span>
> 

<span data-ttu-id="3541c-164">Můžete vyhledat podmínky v některém z hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="3541c-164">You can search for terms in any of the property values.</span></span> <span data-ttu-id="3541c-165">To je zvlášť užitečné, pokud jste napsali [vlastních událostí](app-insights-api-custom-events-metrics.md) s hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="3541c-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="3541c-166">Můžete chtít nastavit čas rozsah vyhledávání za kratší rozsah jsou stejně jako rychlejší.</span><span class="sxs-lookup"><span data-stu-id="3541c-166">You might want to set a time range, as searches over a shorter range are faster.</span></span> 

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="3541c-168">Hledat celá slova, není dílčích řetězců.</span><span class="sxs-lookup"><span data-stu-id="3541c-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="3541c-169">Použijte speciální znaky, uzavřete do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="3541c-169">Use quotation marks to enclose special characters.</span></span>

| <span data-ttu-id="3541c-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3541c-170">string</span></span> | <span data-ttu-id="3541c-171">je *není* nalezena.</span><span class="sxs-lookup"><span data-stu-id="3541c-171">is *not* found by</span></span> | <span data-ttu-id="3541c-172">ale tyto ji najít</span><span class="sxs-lookup"><span data-stu-id="3541c-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3541c-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="3541c-173">HomeController.About</span></span> |<span data-ttu-id="3541c-174">domácí</span><span class="sxs-lookup"><span data-stu-id="3541c-174">home</span></span><br/><span data-ttu-id="3541c-175">Řadiče</span><span class="sxs-lookup"><span data-stu-id="3541c-175">controller</span></span><br/><span data-ttu-id="3541c-176">na více systémů</span><span class="sxs-lookup"><span data-stu-id="3541c-176">out</span></span> | <span data-ttu-id="3541c-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="3541c-177">homecontroller</span></span><br/><span data-ttu-id="3541c-178">o</span><span class="sxs-lookup"><span data-stu-id="3541c-178">about</span></span><br/><span data-ttu-id="3541c-179">"homecontroller.about"</span><span class="sxs-lookup"><span data-stu-id="3541c-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="3541c-180">Spojené státy</span><span class="sxs-lookup"><span data-stu-id="3541c-180">United States</span></span>|<span data-ttu-id="3541c-181">UNI</span><span class="sxs-lookup"><span data-stu-id="3541c-181">Uni</span></span><br/><span data-ttu-id="3541c-182">Tomáš</span><span class="sxs-lookup"><span data-stu-id="3541c-182">ted</span></span>|<span data-ttu-id="3541c-183">Spojené</span><span class="sxs-lookup"><span data-stu-id="3541c-183">united</span></span><br/><span data-ttu-id="3541c-184">stavy</span><span class="sxs-lookup"><span data-stu-id="3541c-184">states</span></span><br/><span data-ttu-id="3541c-185">Spojené státy a</span><span class="sxs-lookup"><span data-stu-id="3541c-185">united AND states</span></span><br/><span data-ttu-id="3541c-186">"Spojené státy"</span><span class="sxs-lookup"><span data-stu-id="3541c-186">"united states"</span></span>

<span data-ttu-id="3541c-187">Zde jsou vyhledávacích výrazech, které můžete použít:</span><span class="sxs-lookup"><span data-stu-id="3541c-187">Here are the search expressions you can use:</span></span>

| <span data-ttu-id="3541c-188">Ukázkový dotaz</span><span class="sxs-lookup"><span data-stu-id="3541c-188">Sample query</span></span> | <span data-ttu-id="3541c-189">Efekt</span><span class="sxs-lookup"><span data-stu-id="3541c-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="3541c-190">Najde všechny události v časové rozmezí, jejichž pole použít slovo "apple"</span><span class="sxs-lookup"><span data-stu-id="3541c-190">Find all events in the time range whose fields include the word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="3541c-191">Najde události, které obsahují i slova.</span><span class="sxs-lookup"><span data-stu-id="3541c-191">Find events that contain both words.</span></span> <span data-ttu-id="3541c-192">Použít kapitálu "a", není "a".</span><span class="sxs-lookup"><span data-stu-id="3541c-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="3541c-193">Najde události, které obsahují buď aplikace word.</span><span class="sxs-lookup"><span data-stu-id="3541c-193">Find events that contain either word.</span></span> <span data-ttu-id="3541c-194">Použití "Nebo", ne "nebo".</span><span class="sxs-lookup"><span data-stu-id="3541c-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="3541c-195">Krátký formulář.</span><span class="sxs-lookup"><span data-stu-id="3541c-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="3541c-196">Najde události, které obsahují jeden word, ale ne na druhou.</span><span class="sxs-lookup"><span data-stu-id="3541c-196">Find events that contain one word but not the other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="3541c-197">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="3541c-197">Sampling</span></span>
<span data-ttu-id="3541c-198">Pokud vaše aplikace generuje mnoho telemetrie (a používáte 2.0.0-beta3 verze sady SDK technologie ASP.NET nebo novější), modul adaptivního vzorkování automaticky sníží svazku, který je odesílán na portál odesláním pouze reprezentativní části události.</span><span class="sxs-lookup"><span data-stu-id="3541c-198">If your app generates a lot of telemetry (and you are using the ASP.NET SDK version 2.0.0-beta3 or later), the adaptive sampling module automatically reduces the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="3541c-199">Události, které se vztahují ke stejnému požadavku jsou však vybrali nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="3541c-199">However, events that are related to the same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="3541c-200">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="3541c-201">Vytvoření pracovní položky</span><span class="sxs-lookup"><span data-stu-id="3541c-201">Create work item</span></span>
<span data-ttu-id="3541c-202">Chyby v Githubu nebo Visual Studio Team Services můžete vytvořit s podrobnostmi z libovolné položce telemetrie.</span><span class="sxs-lookup"><span data-stu-id="3541c-202">You can create a bug in GitHub or Visual Studio Team Services with the details from any telemetry item.</span></span> 

![Klikněte na novou pracovní položku, upravte pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="3541c-204">To poprvé, zobrazí se výzva pro konfiguraci připojení k účtu Team Services a projekt.</span><span class="sxs-lookup"><span data-stu-id="3541c-204">The first time you do this, you are asked to configure a link to your Team Services account and project.</span></span>

![Zadejte adresu URL vašeho serveru Team Services a název projektu a klikněte na autorizovat](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="3541c-206">(Můžete také nakonfigurovat na odkaz v okně pracovních položek).</span><span class="sxs-lookup"><span data-stu-id="3541c-206">(You can also configure the link on the Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="3541c-207">Uložení hledání</span><span class="sxs-lookup"><span data-stu-id="3541c-207">Save your search</span></span>
<span data-ttu-id="3541c-208">Když nastavíte všechny filtry, které chcete, můžete vyhledávání uložit jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="3541c-208">When you've set all the filters you want, you can save the search as a favorite.</span></span> <span data-ttu-id="3541c-209">Pokud pracujete v účet organizace, můžete sdílet s ostatními členy týmu.</span><span class="sxs-lookup"><span data-stu-id="3541c-209">If you work in an organizational account, you can choose whether to share it with other team members.</span></span>

![Klikněte na tlačítko Oblíbené položky, nastavte název a klikněte na Uložit](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="3541c-211">Zobrazíte hledání znovu **přejděte do okna Přehled** a otevřete oblíbených položek:</span><span class="sxs-lookup"><span data-stu-id="3541c-211">To see the search again, **go to the overview blade** and open Favorites:</span></span>

![Dlaždice k oblíbeným položkám.](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="3541c-213">Pokud jste uložili s relativní časové rozmezí, znovu otevřít okno obsahuje nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="3541c-213">If you saved with Relative time range, the re-opened blade has the latest data.</span></span> <span data-ttu-id="3541c-214">Pokud jste uložili s absolutní časové rozmezí, zobrazí stejná data pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="3541c-214">If you saved with Absolute time range, you see the same data every time.</span></span> <span data-ttu-id="3541c-215">(Pokud "Relativní" není dostupná, pokud chcete uložit do oblíbených položek, klikněte na tlačítko časový rozsah v záhlaví a nastavte časový rozsah, který není vlastní rozsah.)</span><span class="sxs-lookup"><span data-stu-id="3541c-215">(If 'Relative' isn't available when you want to save a favorite, click Time Range in the header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-to-application-insights"></a><span data-ttu-id="3541c-216">Odesílání další telemetrické Application Insights</span><span class="sxs-lookup"><span data-stu-id="3541c-216">Send more telemetry to Application Insights</span></span>
<span data-ttu-id="3541c-217">Kromě out box telemetrické zprávy odesílané Application Insights SDK můžete:</span><span class="sxs-lookup"><span data-stu-id="3541c-217">In addition to the out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="3541c-218">Zaznamenat trasování protokolu z vašeho oblíbeného rozhraní protokolování v [.NET](app-insights-asp-net-trace-logs.md) nebo [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="3541c-219">To znamená, můžete vyhledat pomocí protokolů trasování a korelovat je zobrazení stránky, výjimky a dalších událostí.</span><span class="sxs-lookup"><span data-stu-id="3541c-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="3541c-220">[Psaní kódu](app-insights-api-custom-events-metrics.md) k odeslání vlastní události, zobrazení stránek a výjimek.</span><span class="sxs-lookup"><span data-stu-id="3541c-220">[Write code](app-insights-api-custom-events-metrics.md) to send custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="3541c-221">[Zjistěte, jak odeslat protokoly a vlastní telemetrii Application Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-221">[Learn how to send logs and custom telemetry to Application Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="3541c-222"><a name="questions"></a>MODUL OTÁZKY A ODPOVĚDI</span><span class="sxs-lookup"><span data-stu-id="3541c-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="3541c-223"><a name="limits"></a>Množství dat, které se uchovávají?</span><span class="sxs-lookup"><span data-stu-id="3541c-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="3541c-224">Najdete v článku [souhrn omezení](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="3541c-224">See the [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="3541c-225">Jak lze zobrazit následná data v Moje žádosti serveru?</span><span class="sxs-lookup"><span data-stu-id="3541c-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="3541c-226">NÁSLEDNÁ data jsme nemáte protokolu automaticky, ale můžete použít [TrackTrace nebo protokolu volání](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="3541c-226">We don't log the POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="3541c-227">Uveďte následná data v parametru zprávy.</span><span class="sxs-lookup"><span data-stu-id="3541c-227">Put the POST data in the message parameter.</span></span> <span data-ttu-id="3541c-228">Nelze filtrovat zprávy stejným způsobem, který můžete filtrovat podle vlastností, ale maximální velikost je delší.</span><span class="sxs-lookup"><span data-stu-id="3541c-228">You can't filter on the message in the same way you can filter on properties, but the size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="3541c-229">Video</span><span class="sxs-lookup"><span data-stu-id="3541c-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="3541c-230"><a name="add"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="3541c-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="3541c-231">Zapisovat složitých dotazů v Analytics</span><span class="sxs-lookup"><span data-stu-id="3541c-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="3541c-232">Odesílání protokolů a vlastní telemetrii Application Insights</span><span class="sxs-lookup"><span data-stu-id="3541c-232">Send logs and custom telemetry to Application Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="3541c-233">Nastavení dostupnosti a odezvy testů</span><span class="sxs-lookup"><span data-stu-id="3541c-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="3541c-234">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="3541c-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
