---
title: "aaaUsing vyhledávání ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="6a68b-103">Pomocí vyhledávání ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="6a68b-103">Using Search in Application Insights</span></span>
<span data-ttu-id="6a68b-104">Hledání je funkce [Application Insights](app-insights-overview.md) použít toofind a prozkoumejte telemetrie jednotlivých položek, například zobrazení stránky, výjimky nebo webové požadavky.</span><span class="sxs-lookup"><span data-stu-id="6a68b-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use toofind and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="6a68b-105">A můžete zobrazit protokol trasování a události, které mají programového.</span><span class="sxs-lookup"><span data-stu-id="6a68b-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="6a68b-106">(Pro složitější dotazy na data, použijte [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="6a68b-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="6a68b-107">Kde uvidíte hledání?</span><span class="sxs-lookup"><span data-stu-id="6a68b-107">Where do you see Search?</span></span>
### <a name="in-hello-azure-portal"></a><span data-ttu-id="6a68b-108">V hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6a68b-108">In hello Azure portal</span></span>
<span data-ttu-id="6a68b-109">Diagnostické vyhledávání můžete otevřít explicitně v okně Přehled Statistika aplikace hello vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="6a68b-109">You can open diagnostic search explicitly from hello Application Insights Overview blade of your application:</span></span>

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="6a68b-111">Otevře také po kliknutí na tlačítko prostřednictvím některé grafy a položek mřížky.</span><span class="sxs-lookup"><span data-stu-id="6a68b-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="6a68b-112">V takovém případě jeho filtry jsou předem nastavené toofocus na hello typ položky, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="6a68b-112">In this case, its filters are pre-set toofocus on hello type of item you selected.</span></span> 

<span data-ttu-id="6a68b-113">Například v okně Přehled hello, není pruhový graf požadavků, které jsou klasifikovány podle doby odezvy.</span><span class="sxs-lookup"><span data-stu-id="6a68b-113">For example, on hello Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="6a68b-114">Klikněte na tlačítko prostřednictvím toosee rozsah výkon seznam jednotlivých požadavků v tom, že rozsah doby odezvy:</span><span class="sxs-lookup"><span data-stu-id="6a68b-114">Click through a performance range toosee a list of individual requests in that response time range:</span></span>

![Proklikejte se žádost o výkonu](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="6a68b-116">hlavní text Hello diagnostické vyhledávání je seznam položek telemetrie – požadavky na server, vyberte zobrazení, vlastních událostí, které mají programového a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6a68b-116">hello main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="6a68b-117">V hello je horní části seznamu hello souhrnné graf zobrazující počet událostí v čase.</span><span class="sxs-lookup"><span data-stu-id="6a68b-117">At hello top of hello list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="6a68b-118">Klikněte na tlačítko Aktualizovat tooget nové události.</span><span class="sxs-lookup"><span data-stu-id="6a68b-118">Click Refresh tooget new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="6a68b-119">V nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a68b-119">In Visual Studio</span></span>

<span data-ttu-id="6a68b-120">V sadě Visual Studio je také hledání Application Insights okno.</span><span class="sxs-lookup"><span data-stu-id="6a68b-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="6a68b-121">Je velmi užitečné pro zobrazení telemetrie události vygenerované modulem hello aplikace, kterou ladíte.</span><span class="sxs-lookup"><span data-stu-id="6a68b-121">It's most useful for displaying telemetry events generated by hello application that you're debugging.</span></span> <span data-ttu-id="6a68b-122">Ale může také zobrazit události hello získanou z vašich publikovaných aplikací v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6a68b-122">But it can also show hello events collected from your published app at hello Azure portal.</span></span>

<span data-ttu-id="6a68b-123">Otevřete okno vyhledávání hello v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6a68b-123">Open hello Search window in Visual Studio:</span></span>

![Otevřete Visual Studio Application Insights vyhledávání](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="6a68b-125">okno vyhledávání Hello má webový portál podobný toohello funkce:</span><span class="sxs-lookup"><span data-stu-id="6a68b-125">hello Search window has features similar toohello web portal:</span></span>

![Visual Studio Application Insights vyhledávání oken](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="6a68b-127">Karta sledovat operace Hello je k dispozici, při otevření žádost nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="6a68b-127">hello Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="6a68b-128">' Operace, je posloupnost událostí, který je přidružen tooa jednoho požadavku nebo stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6a68b-128">An 'operation' is a sequence of events that is associated with tooa single request or page view.</span></span> <span data-ttu-id="6a68b-129">Například volání závislostí, výjimek, protokoly trasování a vlastních událostí může být součástí jedné operace.</span><span class="sxs-lookup"><span data-stu-id="6a68b-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="6a68b-130">Zobrazí kartu Sledování operaci Hello graficky hello načasování a dobu trvání tyto události v vztah toohello požadavku nebo stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6a68b-130">hello Track Operation tab shows graphically hello timing and duration of these events in relation toohello request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="6a68b-131">Zkontrolujte jednotlivé položky</span><span class="sxs-lookup"><span data-stu-id="6a68b-131">Inspect individual items</span></span>
<span data-ttu-id="6a68b-132">Vyberte všechny klíče polí toosee položek telemetrie a související položky.</span><span class="sxs-lookup"><span data-stu-id="6a68b-132">Select any telemetry item toosee key fields and related items.</span></span> <span data-ttu-id="6a68b-133">Pokud chcete toosee hello úplnou sadu pole, klikněte na tlačítko "...".</span><span class="sxs-lookup"><span data-stu-id="6a68b-133">If you want toosee hello full set of fields, click "...".</span></span> 

![Klikněte na novou pracovní položku, upravit hello pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="6a68b-135">Filtrování typů událostí</span><span class="sxs-lookup"><span data-stu-id="6a68b-135">Filter event types</span></span>
<span data-ttu-id="6a68b-136">Otevřete okno filtru hello a vyberte typy událostí hello se vám mají toosee.</span><span class="sxs-lookup"><span data-stu-id="6a68b-136">Open hello Filter blade and choose hello event types you want toosee.</span></span> <span data-ttu-id="6a68b-137">(Pokud chcete později, toorestore hello filtry, pomocí kterých můžete otevřít okno hello, klikněte na resetovat.)</span><span class="sxs-lookup"><span data-stu-id="6a68b-137">(If, later, you want toorestore hello filters with which you opened hello blade, click Reset.)</span></span>

![Vyberte filtr a vyberte typy telemetrie](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="6a68b-139">typy událostí Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="6a68b-139">hello event types are:</span></span>

* <span data-ttu-id="6a68b-140">**Trasování** - [diagnostické protokoly](app-insights-asp-net-trace-logs.md) včetně TrackTrace, log4Net, NLog a System.Diagnostic.Trace volání.</span><span class="sxs-lookup"><span data-stu-id="6a68b-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="6a68b-141">**Žádosti o** -požadavky HTTP přijatých vaší serverové aplikace, včetně stránky, skripty, bitové kopie, soubory stylu a data.</span><span class="sxs-lookup"><span data-stu-id="6a68b-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="6a68b-142">Tyto události jsou použité toocreate hello žádostí a odpovědí tabulkách Přehled.</span><span class="sxs-lookup"><span data-stu-id="6a68b-142">These events are used toocreate hello request and response overview charts.</span></span>
* <span data-ttu-id="6a68b-143">**Zobrazení stránky** - [Telemetrie odesílaný klientem webové hello](app-insights-javascript.md), používá toocreate stránky zobrazení sestavy.</span><span class="sxs-lookup"><span data-stu-id="6a68b-143">**Page View** - [Telemetry sent by hello web client](app-insights-javascript.md), used toocreate page view reports.</span></span> 
* <span data-ttu-id="6a68b-144">**Vlastní události** – Pokud je příliš vložit volání tooTrackEvent() v pořadí[sledování využití](app-insights-api-custom-events-metrics.md), můžete je zde prohledat.</span><span class="sxs-lookup"><span data-stu-id="6a68b-144">**Custom Event** - If you inserted calls tooTrackEvent() in order too[monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="6a68b-145">**Výjimka** – nezachycená [výjimky v serveru hello](app-insights-asp-net-exceptions.md)a ty, které se můžete přihlásit pomocí TrackException().</span><span class="sxs-lookup"><span data-stu-id="6a68b-145">**Exception** - Uncaught [exceptions in hello server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="6a68b-146">**Závislost** - [volání ze serveru aplikace](app-insights-asp-net-dependencies.md) tooother službami, jako je rozhraní REST API nebo databáze a volání AJAX z vaší [kód klienta](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) tooother services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="6a68b-147">**Dostupnost** -výsledky [testy dostupnosti](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="6a68b-148">Filtrovat podle hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="6a68b-148">Filter on property values</span></span>
<span data-ttu-id="6a68b-149">Můžete filtrovat události na hodnotách hello jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6a68b-149">You can filter events on hello values of their properties.</span></span> <span data-ttu-id="6a68b-150">Hello dostupné vlastnosti závisí na hello typy událostí, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="6a68b-150">hello available properties depend on hello event types you selected.</span></span> 

<span data-ttu-id="6a68b-151">Můžete například vyberte požadavků s kódem konkrétní odezvy.</span><span class="sxs-lookup"><span data-stu-id="6a68b-151">For example, pick out requests with a specific response code.</span></span> 

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="6a68b-153">Výběr žádné hodnoty určité vlastnosti má hello stejná jako volba všech hodnot, které ovlivňuje.</span><span class="sxs-lookup"><span data-stu-id="6a68b-153">Choosing no values of a particular property has hello same effect as choosing all values.</span></span> <span data-ttu-id="6a68b-154">Přepíná vypnout filtrování pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6a68b-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="6a68b-155">Upřesněte hledání</span><span class="sxs-lookup"><span data-stu-id="6a68b-155">Narrow your search</span></span>
<span data-ttu-id="6a68b-156">Všimněte si, že tento hello počty toohello napravo od hodnoty filtru hello zobrazit, kolik výskytů existuje jsou v aktuální sadě filtrované hello.</span><span class="sxs-lookup"><span data-stu-id="6a68b-156">Notice that hello counts toohello right of hello filter values show how many occurrences there are in hello current filtered set.</span></span> 

<span data-ttu-id="6a68b-157">V tomto příkladu je jasné, že výsledky této žádosti hello 'RTP – nebo zaměstnancům, ve většině chyb hello "500":</span><span class="sxs-lookup"><span data-stu-id="6a68b-157">In this example, it's clear that hello 'Rpt/Employees' request results in most of hello '500' errors:</span></span>

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a><span data-ttu-id="6a68b-159">Vyhledejte události s hello stejnou vlastnost</span><span class="sxs-lookup"><span data-stu-id="6a68b-159">Find events with hello same property</span></span>
<span data-ttu-id="6a68b-160">Najít všechny hello položky hello stejná hodnota vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6a68b-160">Find all hello items with hello same property value:</span></span>

![Klikněte pravým tlačítkem na vlastnosti](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a><span data-ttu-id="6a68b-162">Data vyhledávání hello</span><span class="sxs-lookup"><span data-stu-id="6a68b-162">Search hello data</span></span>

> [!NOTE]
> <span data-ttu-id="6a68b-163">toowrite složitější dotazy, otevřete [ **Analytics** ](app-insights-analytics-tour.md) shora hello hello hledání okna.</span><span class="sxs-lookup"><span data-stu-id="6a68b-163">toowrite more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from hello top of hello Search blade.</span></span>
> 

<span data-ttu-id="6a68b-164">Můžete vyhledat podmínky v některém z hodnot vlastností hello.</span><span class="sxs-lookup"><span data-stu-id="6a68b-164">You can search for terms in any of hello property values.</span></span> <span data-ttu-id="6a68b-165">To je zvlášť užitečné, pokud jste napsali [vlastních událostí](app-insights-api-custom-events-metrics.md) s hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="6a68b-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="6a68b-166">Můžete chtít tooset časový rozsah vyhledávání za kratší rozsah jsou stejně jako rychlejší.</span><span class="sxs-lookup"><span data-stu-id="6a68b-166">You might want tooset a time range, as searches over a shorter range are faster.</span></span> 

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="6a68b-168">Hledat celá slova, není dílčích řetězců.</span><span class="sxs-lookup"><span data-stu-id="6a68b-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="6a68b-169">Používejte speciální znaky uvozovek tooenclose.</span><span class="sxs-lookup"><span data-stu-id="6a68b-169">Use quotation marks tooenclose special characters.</span></span>

| <span data-ttu-id="6a68b-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="6a68b-170">string</span></span> | <span data-ttu-id="6a68b-171">je *není* nalezena.</span><span class="sxs-lookup"><span data-stu-id="6a68b-171">is *not* found by</span></span> | <span data-ttu-id="6a68b-172">ale tyto ji najít</span><span class="sxs-lookup"><span data-stu-id="6a68b-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6a68b-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="6a68b-173">HomeController.About</span></span> |<span data-ttu-id="6a68b-174">domácí</span><span class="sxs-lookup"><span data-stu-id="6a68b-174">home</span></span><br/><span data-ttu-id="6a68b-175">Řadiče</span><span class="sxs-lookup"><span data-stu-id="6a68b-175">controller</span></span><br/><span data-ttu-id="6a68b-176">na více systémů</span><span class="sxs-lookup"><span data-stu-id="6a68b-176">out</span></span> | <span data-ttu-id="6a68b-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="6a68b-177">homecontroller</span></span><br/><span data-ttu-id="6a68b-178">o</span><span class="sxs-lookup"><span data-stu-id="6a68b-178">about</span></span><br/><span data-ttu-id="6a68b-179">"homecontroller.about"</span><span class="sxs-lookup"><span data-stu-id="6a68b-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="6a68b-180">Spojené státy</span><span class="sxs-lookup"><span data-stu-id="6a68b-180">United States</span></span>|<span data-ttu-id="6a68b-181">UNI</span><span class="sxs-lookup"><span data-stu-id="6a68b-181">Uni</span></span><br/><span data-ttu-id="6a68b-182">Tomáš</span><span class="sxs-lookup"><span data-stu-id="6a68b-182">ted</span></span>|<span data-ttu-id="6a68b-183">Spojené</span><span class="sxs-lookup"><span data-stu-id="6a68b-183">united</span></span><br/><span data-ttu-id="6a68b-184">stavy</span><span class="sxs-lookup"><span data-stu-id="6a68b-184">states</span></span><br/><span data-ttu-id="6a68b-185">Spojené státy a</span><span class="sxs-lookup"><span data-stu-id="6a68b-185">united AND states</span></span><br/><span data-ttu-id="6a68b-186">"Spojené státy"</span><span class="sxs-lookup"><span data-stu-id="6a68b-186">"united states"</span></span>

<span data-ttu-id="6a68b-187">Zde jsou hello vyhledávacích výrazech, které můžete použít:</span><span class="sxs-lookup"><span data-stu-id="6a68b-187">Here are hello search expressions you can use:</span></span>

| <span data-ttu-id="6a68b-188">Ukázkový dotaz</span><span class="sxs-lookup"><span data-stu-id="6a68b-188">Sample query</span></span> | <span data-ttu-id="6a68b-189">Efekt</span><span class="sxs-lookup"><span data-stu-id="6a68b-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="6a68b-190">Najde všechny události v hello časové rozmezí, jejichž pole obsahovat hello slovo "apple"</span><span class="sxs-lookup"><span data-stu-id="6a68b-190">Find all events in hello time range whose fields include hello word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="6a68b-191">Najde události, které obsahují i slova.</span><span class="sxs-lookup"><span data-stu-id="6a68b-191">Find events that contain both words.</span></span> <span data-ttu-id="6a68b-192">Použít kapitálu "a", není "a".</span><span class="sxs-lookup"><span data-stu-id="6a68b-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="6a68b-193">Najde události, které obsahují buď aplikace word.</span><span class="sxs-lookup"><span data-stu-id="6a68b-193">Find events that contain either word.</span></span> <span data-ttu-id="6a68b-194">Použití "Nebo", ne "nebo".</span><span class="sxs-lookup"><span data-stu-id="6a68b-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="6a68b-195">Krátký formulář.</span><span class="sxs-lookup"><span data-stu-id="6a68b-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="6a68b-196">Najít události, které obsahují, ale není hello jiných jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a68b-196">Find events that contain one word but not hello other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="6a68b-197">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="6a68b-197">Sampling</span></span>
<span data-ttu-id="6a68b-198">Pokud vaše aplikace generuje mnoho telemetrie (a používáte hello sady SDK technologie ASP.NET verze 2.0.0-beta3 nebo novější), hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události.</span><span class="sxs-lookup"><span data-stu-id="6a68b-198">If your app generates a lot of telemetry (and you are using hello ASP.NET SDK version 2.0.0-beta3 or later), hello adaptive sampling module automatically reduces hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="6a68b-199">Ale události, které jsou související toohello stejné žádosti jsou vybrané nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="6a68b-199">However, events that are related toohello same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="6a68b-200">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="6a68b-201">Vytvoření pracovní položky</span><span class="sxs-lookup"><span data-stu-id="6a68b-201">Create work item</span></span>
<span data-ttu-id="6a68b-202">Chyby v Githubu nebo Visual Studio Team Services můžete vytvořit s podrobnostmi hello z libovolné položce telemetrie.</span><span class="sxs-lookup"><span data-stu-id="6a68b-202">You can create a bug in GitHub or Visual Studio Team Services with hello details from any telemetry item.</span></span> 

![Klikněte na novou pracovní položku, upravit hello pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="6a68b-204">Hello poprvé, které použijete, se zobrazí výzva tooconfigure tooyour odkaz Team Services účet a projektu.</span><span class="sxs-lookup"><span data-stu-id="6a68b-204">hello first time you do this, you are asked tooconfigure a link tooyour Team Services account and project.</span></span>

![Zadejte adresu URL hello serverem Team Services a hello název projektu a klikněte na autorizovat](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="6a68b-206">(Můžete také nakonfigurovat hello odkaz na okno hello pracovních položek).</span><span class="sxs-lookup"><span data-stu-id="6a68b-206">(You can also configure hello link on hello Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="6a68b-207">Uložení hledání</span><span class="sxs-lookup"><span data-stu-id="6a68b-207">Save your search</span></span>
<span data-ttu-id="6a68b-208">Po nastavení všech hello filtry, které chcete, můžete hello vyhledávání uložit jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="6a68b-208">When you've set all hello filters you want, you can save hello search as a favorite.</span></span> <span data-ttu-id="6a68b-209">Pokud pracujete v účet organizace, můžete vybrat zda tooshare ji s ostatními členy týmu.</span><span class="sxs-lookup"><span data-stu-id="6a68b-209">If you work in an organizational account, you can choose whether tooshare it with other team members.</span></span>

![Klikněte na tlačítko Oblíbené položky, hello název sady a klikněte na Uložit](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="6a68b-211">toosee hello znovu vyhledávání **okno Přehled přejděte toohello** a otevřete oblíbených položek:</span><span class="sxs-lookup"><span data-stu-id="6a68b-211">toosee hello search again, **go toohello overview blade** and open Favorites:</span></span>

![Dlaždice k oblíbeným položkám.](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="6a68b-213">Pokud jste uložili s relativní časové rozmezí, znovu otevřít okno hello má hello nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="6a68b-213">If you saved with Relative time range, hello re-opened blade has hello latest data.</span></span> <span data-ttu-id="6a68b-214">Pokud jste uložili s absolutní časové rozmezí, zobrazí hello pokaždé, když stejná data.</span><span class="sxs-lookup"><span data-stu-id="6a68b-214">If you saved with Absolute time range, you see hello same data every time.</span></span> <span data-ttu-id="6a68b-215">(Pokud "Relativní" není dostupná, pokud chcete, aby toosave Oblíbené, klepněte časový rozsah v záhlaví hello a nastavte časový rozsah, který není vlastní rozsah.)</span><span class="sxs-lookup"><span data-stu-id="6a68b-215">(If 'Relative' isn't available when you want toosave a favorite, click Time Range in hello header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-tooapplication-insights"></a><span data-ttu-id="6a68b-216">Odeslat další telemetrie tooApplication Insights</span><span class="sxs-lookup"><span data-stu-id="6a68b-216">Send more telemetry tooApplication Insights</span></span>
<span data-ttu-id="6a68b-217">Přidání toohello out box telemetrické zprávy odesílané Application Insights SDK můžete:</span><span class="sxs-lookup"><span data-stu-id="6a68b-217">In addition toohello out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="6a68b-218">Zaznamenat trasování protokolu z vašeho oblíbeného rozhraní protokolování v [.NET](app-insights-asp-net-trace-logs.md) nebo [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="6a68b-219">To znamená, můžete vyhledat pomocí protokolů trasování a korelovat je zobrazení stránky, výjimky a dalších událostí.</span><span class="sxs-lookup"><span data-stu-id="6a68b-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="6a68b-220">[Psaní kódu](app-insights-api-custom-events-metrics.md) toosend vlastních událostí, zobrazení stránek a výjimek.</span><span class="sxs-lookup"><span data-stu-id="6a68b-220">[Write code](app-insights-api-custom-events-metrics.md) toosend custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="6a68b-221">[Zjistěte, jak toosend protokoly a vlastní telemetrii tooApplication Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-221">[Learn how toosend logs and custom telemetry tooApplication Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="6a68b-222"><a name="questions"></a>MODUL OTÁZKY A ODPOVĚDI</span><span class="sxs-lookup"><span data-stu-id="6a68b-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="6a68b-223"><a name="limits"></a>Množství dat, které se uchovávají?</span><span class="sxs-lookup"><span data-stu-id="6a68b-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="6a68b-224">V tématu hello [souhrn omezení](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="6a68b-224">See hello [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="6a68b-225">Jak lze zobrazit následná data v Moje žádosti serveru?</span><span class="sxs-lookup"><span data-stu-id="6a68b-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="6a68b-226">Jsme nemáte protokolovat hello POST data automaticky, ale můžete použít [TrackTrace nebo protokolu volání](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="6a68b-226">We don't log hello POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="6a68b-227">Uveďte hello následná data v parametru zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="6a68b-227">Put hello POST data in hello message parameter.</span></span> <span data-ttu-id="6a68b-228">Nelze filtrovat uvítací zprávu v hello stejný způsobem můžete filtrovat podle vlastností, ale limit velikosti hello je delší.</span><span class="sxs-lookup"><span data-stu-id="6a68b-228">You can't filter on hello message in hello same way you can filter on properties, but hello size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="6a68b-229">Video</span><span class="sxs-lookup"><span data-stu-id="6a68b-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="6a68b-230"><a name="add"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a68b-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="6a68b-231">Zapisovat složitých dotazů v Analytics</span><span class="sxs-lookup"><span data-stu-id="6a68b-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="6a68b-232">Odeslání protokolů a vlastní telemetrii tooApplication Insights</span><span class="sxs-lookup"><span data-stu-id="6a68b-232">Send logs and custom telemetry tooApplication Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="6a68b-233">Nastavení dostupnosti a odezvy testů</span><span class="sxs-lookup"><span data-stu-id="6a68b-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="6a68b-234">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6a68b-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
