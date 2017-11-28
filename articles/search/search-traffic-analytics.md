---
title: "Analýza provozu vyhledávání pro službu Azure Search | Microsoft Docs"
description: "Povolte Analýza provozu vyhledávání pro službu Azure Search, hostované cloudové vyhledávací službu v Microsoft Azure, k odemčení přehledy o vašich dat a jak pracují uživatelé."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
ms.openlocfilehash: 303ca5c820f573dc0b58f1910f258403c3baad2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-search-traffic-analytics"></a><span data-ttu-id="eaa11-103">Co je analýza provozu vyhledávání</span><span class="sxs-lookup"><span data-stu-id="eaa11-103">What is search traffic analytics</span></span>
<span data-ttu-id="eaa11-104">Analýza provozu vyhledávání je vzor pro implementaci zpětné vazby pro vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-104">Search traffic analytics is a pattern for implementing a feedback loop for your search service.</span></span> <span data-ttu-id="eaa11-105">Tento vzor popisuje potřebná data a jak shromažďovat pomocí Application Insights, vedoucí odvětví pro monitorování služeb ve více platformách.</span><span class="sxs-lookup"><span data-stu-id="eaa11-105">This pattern describes the necessary data and how to collect it using Application Insights, an industry leader for monitoring services in multiple platforms.</span></span>

<span data-ttu-id="eaa11-106">Analýza provozu vyhledávání vám umožní získat přehled o vaši službu vyhledávání a odemknutí přehledy o uživatele a jejich chování.</span><span class="sxs-lookup"><span data-stu-id="eaa11-106">Search traffic analytics lets you gain visibility into your search service and unlock insights about your users and their behavior.</span></span> <span data-ttu-id="eaa11-107">Tak, že data o co vaši uživatelé vybrat, je možné při rozhodování, které vylepšují další možnosti vyhledávání a regrese není tom, co očekávat při jsou výsledky.</span><span class="sxs-lookup"><span data-stu-id="eaa11-107">By having data about what your users choose, it's possible to make decisions that further improve your search experience, and to back off when the results are not what expected.</span></span>

<span data-ttu-id="eaa11-108">Služba Azure Search nabízí telemetrii řešení, která integruje služby Azure Application Insights a Power BI vám umožňuje hlubší monitorování a sledování.</span><span class="sxs-lookup"><span data-stu-id="eaa11-108">Azure Search offers a telemetry solution that integrates Azure Application Insights and Power BI to provide in-depth monitoring and tracking.</span></span> <span data-ttu-id="eaa11-109">Vzhledem k interakci s Azure Search je pouze prostřednictvím rozhraní API, musí být implementované telemetrii vývojáři hledání, pomocí pokynů v této stránce.</span><span class="sxs-lookup"><span data-stu-id="eaa11-109">Because interaction with Azure Search is only through APIs, the telemetry must be implemented by the developers using search, following the instructions in this page.</span></span>

## <a name="identify-the-relevant-search-data"></a><span data-ttu-id="eaa11-110">Identifikovat data relevantní hledání</span><span class="sxs-lookup"><span data-stu-id="eaa11-110">Identify the relevant search data</span></span>

<span data-ttu-id="eaa11-111">Pokud chcete, aby užitečná hledání metriky, je potřeba protokolu některé signály od uživatelů aplikace vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-111">To have useful search metrics, it's necessary to log some signals from the users of the search application.</span></span> <span data-ttu-id="eaa11-112">Tyto signály označují obsah, který se uživatelé chtějí a zvažte relevantní pro jejich potřeb.</span><span class="sxs-lookup"><span data-stu-id="eaa11-112">These signals signify content that users are interested in and that they consider relevant to their needs.</span></span>

<span data-ttu-id="eaa11-113">Existují dva signály, které potřebuje Analýza provozu vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="eaa11-113">There are two signals Search Traffic Analytics needs:</span></span>

1. <span data-ttu-id="eaa11-114">Uživatelem generovaný vyhledávání události: jenom zajímavé vyhledávací dotazy iniciovaná uživatelem.</span><span class="sxs-lookup"><span data-stu-id="eaa11-114">User generated search events: only search queries initiated by a user are interesting.</span></span> <span data-ttu-id="eaa11-115">Požadavky Search používá k naplnění omezující, další obsah nebo nějaké interní informace o, nejsou důležité a zkreslit a usměrní výsledky.</span><span class="sxs-lookup"><span data-stu-id="eaa11-115">Search requests used to populate facets, additional content or any internal information, are not important and they skew and bias your results.</span></span>

2. <span data-ttu-id="eaa11-116">Události kliknutí uživatelem generovaný: pomocí kliknutí v tomto dokumentu označujeme uživateli výběr konkrétní vyhledávání výsledku vrácený z vyhledávacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="eaa11-116">User generated click events: By clicks in this document, we refer to a user selecting a particular search result returned from a search query.</span></span> <span data-ttu-id="eaa11-117">Klikněte na obvykle znamená, že dokument je výsledku relevantní pro konkrétní vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="eaa11-117">A click generally means that a document is a relevant result for a specific search query.</span></span>

<span data-ttu-id="eaa11-118">Pomocí propojení vyhledávání a klikněte na události s id korelace, je možné k analýze chování uživatelů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eaa11-118">By linking search and click events with a correlation id, it's possible to analyze the behaviors of users on your application.</span></span> <span data-ttu-id="eaa11-119">Tyto přehledy hledání je možné dosáhnout s pouze provoz protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-119">These search insights are impossible to obtain with only search traffic logs.</span></span>

## <a name="how-to-implement-search-traffic-analytics"></a><span data-ttu-id="eaa11-120">Jak implementovat Analýza provozu vyhledávání</span><span class="sxs-lookup"><span data-stu-id="eaa11-120">How to implement search traffic analytics</span></span>

<span data-ttu-id="eaa11-121">Signály uvedeno v předchozí části musí shromáždit z vyhledávací aplikaci jako uživatel pracuje s ním.</span><span class="sxs-lookup"><span data-stu-id="eaa11-121">The signals mentioned in the preceding section must be gathered from the search application as the user interacts with it.</span></span> <span data-ttu-id="eaa11-122">Application Insights je rozšiřitelný monitorování řešení, k dispozici pro různé platformy, s možností flexibilní instrumentace.</span><span class="sxs-lookup"><span data-stu-id="eaa11-122">Application Insights is an extensible monitoring solution, available for multiple platforms, with flexible instrumentation options.</span></span> <span data-ttu-id="eaa11-123">Využití Application Insights umožňuje využít výhod vytvořené Azure Search v zájmu snazší analýzy dat sestavy Power BI vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-123">Usage of Application Insights lets you take advantage of the Power BI search reports created by Azure Search to make the analysis of data easier.</span></span>

<span data-ttu-id="eaa11-124">V [portál](https://portal.azure.com) stránka pro službu Azure Search, v okně Analýza provozu vyhledávání obsahuje tahák podle tohoto vzoru telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eaa11-124">In the [portal](https://portal.azure.com) page for your Azure Search service, the Search Traffic Analytics blade contains a cheat sheet for following this telemetry pattern.</span></span> <span data-ttu-id="eaa11-125">Můžete také vyberte nebo vytvořte prostředek Application Insights a najdete v části potřebná data z jednoho místa.</span><span class="sxs-lookup"><span data-stu-id="eaa11-125">You can also select or create an Application Insights resource, and see the necessary data, all in one place.</span></span>

![Pokyny Analýza provozu vyhledávání][1]

### <a name="1-select-an-application-insights-resource"></a><span data-ttu-id="eaa11-127">1. Vyberte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="eaa11-127">1. Select an Application Insights resource</span></span>

<span data-ttu-id="eaa11-128">Je nutné vybrat prostředek Application Insights nebo, pokud již nemáte vytvořit.</span><span class="sxs-lookup"><span data-stu-id="eaa11-128">You need to select an Application Insights resource to use or create one if you don't have one already.</span></span> <span data-ttu-id="eaa11-129">Můžete použít na prostředek, který se už používá k přihlášení vyžaduje vlastní události.</span><span class="sxs-lookup"><span data-stu-id="eaa11-129">You can use a resource that's already in use to log the required custom events.</span></span>

<span data-ttu-id="eaa11-130">Když vytváříte nový prostředek Application Insights, všechny typy aplikací jsou platné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="eaa11-130">When creating a new Application Insights resource, all application types are valid for this scenario.</span></span> <span data-ttu-id="eaa11-131">Vyberte ten, který nejlépe vyhovuje platformy, které používáte.</span><span class="sxs-lookup"><span data-stu-id="eaa11-131">Select the one that best fits the platform you are using.</span></span>

<span data-ttu-id="eaa11-132">Klíč instrumentace můžete potřebovat pro vytvoření klienta telemetrie pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eaa11-132">You need the instrumentation key for creating the telemetry client for your application.</span></span> <span data-ttu-id="eaa11-133">Můžete ho získat na řídicím panelu portálu Application Insights, nebo můžete ho získat na stránce Analýza provozu vyhledávání výběr instance, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="eaa11-133">You can get it from the Application Insights portal dashboard, or you can get it from the Search Traffic Analytics page, selecting the instance you want to use.</span></span>

### <a name="2-instrument-your-application"></a><span data-ttu-id="eaa11-134">2. Instrumentace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="eaa11-134">2. Instrument your application</span></span>

<span data-ttu-id="eaa11-135">Tato fáze je, kde instrumentace vlastní vyhledávací aplikaci, pomocí vaší vytvořený prostředek Application Insights v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="eaa11-135">This phase is where you instrument your own search application, using the Application Insights resource your created in the step above.</span></span> <span data-ttu-id="eaa11-136">Existují čtyři kroky tohoto procesu:</span><span class="sxs-lookup"><span data-stu-id="eaa11-136">There are four steps to this process:</span></span>

<span data-ttu-id="eaa11-137">**I. Vytvoření klienta telemetrie** to je objekt, který odesílá události pro prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="eaa11-137">**I. Create a telemetry client** This is the object that sends events to the Application Insights Resource.</span></span>

<span data-ttu-id="eaa11-138">*C#*</span><span class="sxs-lookup"><span data-stu-id="eaa11-138">*C#*</span></span>

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

<span data-ttu-id="eaa11-139">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="eaa11-139">*JavaScript*</span></span>

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

<span data-ttu-id="eaa11-140">Pro jiné jazyky a platformy, najdete v úplných [seznamu](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span><span class="sxs-lookup"><span data-stu-id="eaa11-140">For other languages and platforms, see the complete [list](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span></span>

<span data-ttu-id="eaa11-141">**II. Vyhledávání ID pro korelace požadavku** ke korelaci požadavky search kliknutími, je potřeba mít id korelace, které se týkají těchto dvou různých událostí.</span><span class="sxs-lookup"><span data-stu-id="eaa11-141">**II. Request a Search ID for correlation** To correlate search requests with clicks, it's necessary to have a correlation id that relates these two distinct events.</span></span> <span data-ttu-id="eaa11-142">Vyhledávání systému Azure poskytuje Id vyhledávání, pokud budete požadovat s záhlaví:</span><span class="sxs-lookup"><span data-stu-id="eaa11-142">Azure Search provides you with a Search Id when you request it with a header:</span></span>

<span data-ttu-id="eaa11-143">*C#*</span><span class="sxs-lookup"><span data-stu-id="eaa11-143">*C#*</span></span>

    // This sample uses the Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

<span data-ttu-id="eaa11-144">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="eaa11-144">*JavaScript*</span></span>

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

<span data-ttu-id="eaa11-145">**III. Protokolování událostí, které vyhledávání**</span><span class="sxs-lookup"><span data-stu-id="eaa11-145">**III. Log Search events**</span></span>

<span data-ttu-id="eaa11-146">Pokaždé, když je žádost o vyhledávání vydané určitým uživatelem, je zapotřebí přihlásit, jako událost vyhledávání s následující schéma na vlastní událost Application Insights:</span><span class="sxs-lookup"><span data-stu-id="eaa11-146">Every time that a search request is issued by a user, you should log that as a search event with the following schema on an Application Insights custom event:</span></span>

<span data-ttu-id="eaa11-147">**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) vyhledávací dotaz (dodává v odpovědi vyhledávání) **IndexName**: index služby search (string) jako dotaz **QueryTerms**: (string) hledaných termínů zadá uživatel **element resultcount element**: číslo (int) dokumentů, které byly vráceny (dodává v odpovědi vyhledávání)  **ScoringProfile**: (string) název profilu vyhodnocování používá, pokud existuje</span><span class="sxs-lookup"><span data-stu-id="eaa11-147">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of the search query (comes in the search response) **IndexName**: (string) search service index to be queried **QueryTerms**: (string) search terms entered by the user **ResultCount**: (int) number of documents that were returned (comes in the search response) **ScoringProfile**: (string) name of the scoring profile used, if any</span></span>

> [!NOTE]
> <span data-ttu-id="eaa11-148">Počet požadavků na dotazy na uživatelem generovaný přidáním $count = true vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="eaa11-148">Request count on user generated queries by adding $count=true to your search query.</span></span> <span data-ttu-id="eaa11-149">Další informace najdete v části [sem](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span><span class="sxs-lookup"><span data-stu-id="eaa11-149">See more information [here](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span></span>
>

> [!NOTE]
> <span data-ttu-id="eaa11-150">Mějte na paměti se protokolovat jenom vyhledávací dotazy, které jsou generovány uživatelé.</span><span class="sxs-lookup"><span data-stu-id="eaa11-150">Remember to only log search queries that are generated by users.</span></span>
>

<span data-ttu-id="eaa11-151">*C#*</span><span class="sxs-lookup"><span data-stu-id="eaa11-151">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

<span data-ttu-id="eaa11-152">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="eaa11-152">*JavaScript*</span></span>

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

<span data-ttu-id="eaa11-153">**IV. Přihlaste se kliknutím na události**</span><span class="sxs-lookup"><span data-stu-id="eaa11-153">**IV. Log Click events**</span></span>

<span data-ttu-id="eaa11-154">Pokaždé, když uživatel klikne na dokument, který je signál, musíte být přihlášeni pro účely analýzy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-154">Every time that a user clicks on a document, that's a signal that must be logged for search analysis purposes.</span></span> <span data-ttu-id="eaa11-155">Použijte vlastní události Application Insights do protokolu tyto události se schématem následující:</span><span class="sxs-lookup"><span data-stu-id="eaa11-155">Use Application Insights custom events to log these events with the following schema:</span></span>

<span data-ttu-id="eaa11-156">**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) související vyhledávací dotaz **identifikátorů DocId**: identifikátor dokumentu (string) **pozice** : stránka s výsledky pořadí (int) dokumentu do vyhledávání</span><span class="sxs-lookup"><span data-stu-id="eaa11-156">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of the related search query **DocId**: (string) document identifier **Position**: (int) rank of the document in the search results page</span></span>

> [!NOTE]
> <span data-ttu-id="eaa11-157">Pozice odkazuje mohutnosti pořadí, v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eaa11-157">Position refers to the cardinal order in your application.</span></span> <span data-ttu-id="eaa11-158">Jste volné nastavit toto číslo, dokud je vždy stejný, aby pro porovnání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-158">You are free to set this number, as long as it's always the same, to allow for comparison.</span></span>
>

<span data-ttu-id="eaa11-159">*C#*</span><span class="sxs-lookup"><span data-stu-id="eaa11-159">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

<span data-ttu-id="eaa11-160">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="eaa11-160">*JavaScript*</span></span>

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a><span data-ttu-id="eaa11-161">3. Analýza s Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="eaa11-161">3. Analyze with Power BI Desktop</span></span>

<span data-ttu-id="eaa11-162">Po instrumentovány vaší aplikace a ověřit, zda že je aplikace správně připojený k Application Insights, můžete použít předdefinované šablony vytvořené Azure Search pro Power BI desktop.</span><span class="sxs-lookup"><span data-stu-id="eaa11-162">After you have instrumented your app and verified your application is correctly connected to Application Insights, you can use a predefined template created by Azure Search for Power BI desktop.</span></span>
<span data-ttu-id="eaa11-163">Tato šablona obsahuje grafů a tabulek, které vám pomůžou přijímání rozhodnutí za účelem zvýšení výkonu vyhledávání a závažnosti.</span><span class="sxs-lookup"><span data-stu-id="eaa11-163">This template contains charts and tables that help you make more informed decisions to improve your search performance and relevance.</span></span>

<span data-ttu-id="eaa11-164">K vytvoření instance šablony klienta Power BI, potřebujete tři údaje o Application Insights.</span><span class="sxs-lookup"><span data-stu-id="eaa11-164">To instantiate the Power BI desktop template, you need three pieces of information about Application Insights.</span></span> <span data-ttu-id="eaa11-165">Tato data naleznete na stránce Analýza provozu vyhledávání vyberete-li použít tento prostředek</span><span class="sxs-lookup"><span data-stu-id="eaa11-165">This data can be found in the Search Traffic Analytics page, when you select the resource to use</span></span>

![Přehled Data aplikací v okně Analýza provozu vyhledávání][2]

<span data-ttu-id="eaa11-167">Metrika součástí šablony klienta Power BI:</span><span class="sxs-lookup"><span data-stu-id="eaa11-167">Metrics included in the Power BI desktop template:</span></span>

*   <span data-ttu-id="eaa11-168">Klikněte na tlačítko prostřednictvím rychlost (PEV.cenu): poměr uživatelů, kteří klikněte na konkrétní dokument počet celkový vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-168">Click through Rate (CTR): ratio of users who click on a specific document to the number of total searches.</span></span>
*   <span data-ttu-id="eaa11-169">Hledání bez kliknutí: podmínky pro Nejoblíbenější dotazy, které registrují bez kliknutí</span><span class="sxs-lookup"><span data-stu-id="eaa11-169">Searches without clicks: terms for top queries that register no clicks</span></span>
*   <span data-ttu-id="eaa11-170">Většina kliknutí na dokumenty: nejvíce kliknutí na dokumenty podle ID za posledních 24 hodin, 7 dní až 30 dní.</span><span class="sxs-lookup"><span data-stu-id="eaa11-170">Most clicked documents: most clicked documents by ID in the last 24 hours, 7 days, and 30 days.</span></span>
*   <span data-ttu-id="eaa11-171">Dvojice oblíbených termín document: podmínky, jejichž výsledkem stejné dokumentu klikli, seřazené podle kliknutí.</span><span class="sxs-lookup"><span data-stu-id="eaa11-171">Popular term-document pairs: terms that result in the same document clicked, ordered by clicks.</span></span>
*   <span data-ttu-id="eaa11-172">Čas klikněte na tlačítko: kliknutí kategorizovaná podle doby od vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="eaa11-172">Time to click: clicks bucketed by time since the search query</span></span>

![Power BI šablonu pro čtení ze služby Application Insights][3]


## <a name="next-steps"></a><span data-ttu-id="eaa11-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eaa11-174">Next Steps</span></span>
<span data-ttu-id="eaa11-175">Instrumentace aplikace vyhledávání výkonný a pronikavého údaje o vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="eaa11-175">Instrument your search application to get powerful and insightful data about your search service.</span></span>

<span data-ttu-id="eaa11-176">Můžete najít další informace o Application Insights [zde](https://go.microsoft.com/fwlink/?linkid=842905).</span><span class="sxs-lookup"><span data-stu-id="eaa11-176">You can find more information on Application Insights [here](https://go.microsoft.com/fwlink/?linkid=842905).</span></span> <span data-ttu-id="eaa11-177">Navštivte Application Insights [stránce s cenami](https://azure.microsoft.com/pricing/details/application-insights/) Další informace o jejich různých úrovních služby.</span><span class="sxs-lookup"><span data-stu-id="eaa11-177">Visit Application Insights [pricing page](https://azure.microsoft.com/pricing/details/application-insights/) to learn more about their different service tiers.</span></span>

<span data-ttu-id="eaa11-178">Další informace o vytváření úžasné sestav.</span><span class="sxs-lookup"><span data-stu-id="eaa11-178">Learn more about creating amazing reports.</span></span> <span data-ttu-id="eaa11-179">V tématu [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) podrobnosti</span><span class="sxs-lookup"><span data-stu-id="eaa11-179">See [Getting started with Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) for details</span></span>

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
