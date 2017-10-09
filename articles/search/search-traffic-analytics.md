---
title: "aaaSearch Analýza provozu pro službu Azure Search | Microsoft Docs"
description: "Povolte Analýza provozu vyhledávání pro službu Azure Search, hostované cloudové vyhledávací službu v Microsoft Azure, toounlock přehledy o vašich dat a jak pracují uživatelé."
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
ms.openlocfilehash: 1d16aa63d05c1c3df1bbfbb4f09ac77705ed9d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-search-traffic-analytics"></a><span data-ttu-id="cb048-103">Co je analýza provozu vyhledávání</span><span class="sxs-lookup"><span data-stu-id="cb048-103">What is search traffic analytics</span></span>
<span data-ttu-id="cb048-104">Analýza provozu vyhledávání je vzor pro implementaci zpětné vazby pro vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="cb048-104">Search traffic analytics is a pattern for implementing a feedback loop for your search service.</span></span> <span data-ttu-id="cb048-105">Tento vzor popisuje hello potřebná data a jak toocollect pomocí Application Insights, vedoucí odvětví pro monitorování služeb ve více platformách.</span><span class="sxs-lookup"><span data-stu-id="cb048-105">This pattern describes hello necessary data and how toocollect it using Application Insights, an industry leader for monitoring services in multiple platforms.</span></span>

<span data-ttu-id="cb048-106">Analýza provozu vyhledávání vám umožní získat přehled o vaši službu vyhledávání a odemknutí přehledy o uživatele a jejich chování.</span><span class="sxs-lookup"><span data-stu-id="cb048-106">Search traffic analytics lets you gain visibility into your search service and unlock insights about your users and their behavior.</span></span> <span data-ttu-id="cb048-107">Tak, že data o co vaši uživatelé vybrat, je možné toomake rozhodnutí, která je dále zlepšit vyhledávání a tooback vypnout, pokud jsou výsledky hello není co očekává.</span><span class="sxs-lookup"><span data-stu-id="cb048-107">By having data about what your users choose, it's possible toomake decisions that further improve your search experience, and tooback off when hello results are not what expected.</span></span>

<span data-ttu-id="cb048-108">Služba Azure Search nabízí telemetrii řešení, které se integruje s Azure Application Insights a Power BI tooprovide hlubší monitorování a sledování.</span><span class="sxs-lookup"><span data-stu-id="cb048-108">Azure Search offers a telemetry solution that integrates Azure Application Insights and Power BI tooprovide in-depth monitoring and tracking.</span></span> <span data-ttu-id="cb048-109">Vzhledem k interakci s Azure Search je pouze prostřednictvím rozhraní API, musí být implementované hello telemetrie hello vývojáři hledání, pomocí pokynů hello na této stránce.</span><span class="sxs-lookup"><span data-stu-id="cb048-109">Because interaction with Azure Search is only through APIs, hello telemetry must be implemented by hello developers using search, following hello instructions in this page.</span></span>

## <a name="identify-hello-relevant-search-data"></a><span data-ttu-id="cb048-110">Identifikovat data relevantní vyhledávání hello</span><span class="sxs-lookup"><span data-stu-id="cb048-110">Identify hello relevant search data</span></span>

<span data-ttu-id="cb048-111">toohave užitečná hledání metriky, je nutné toolog některé signály z hello uživatelům aplikace hello vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="cb048-111">toohave useful search metrics, it's necessary toolog some signals from hello users of hello search application.</span></span> <span data-ttu-id="cb048-112">Tyto signály označily obsahu, že se uživatelé chtějí, a že za to, že relevantní tootheir potřebuje.</span><span class="sxs-lookup"><span data-stu-id="cb048-112">These signals signify content that users are interested in and that they consider relevant tootheir needs.</span></span>

<span data-ttu-id="cb048-113">Existují dva signály, které potřebuje Analýza provozu vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="cb048-113">There are two signals Search Traffic Analytics needs:</span></span>

1. <span data-ttu-id="cb048-114">Uživatelem generovaný vyhledávání události: jenom zajímavé vyhledávací dotazy iniciovaná uživatelem.</span><span class="sxs-lookup"><span data-stu-id="cb048-114">User generated search events: only search queries initiated by a user are interesting.</span></span> <span data-ttu-id="cb048-115">Hledání požadavků používá toopopulate omezující, další obsah nebo nějaké interní informace o, nejsou důležité a zkreslit a usměrní výsledky.</span><span class="sxs-lookup"><span data-stu-id="cb048-115">Search requests used toopopulate facets, additional content or any internal information, are not important and they skew and bias your results.</span></span>

2. <span data-ttu-id="cb048-116">Události kliknutí uživatelem generovaný: pomocí kliknutí v tomto dokumentu označujeme tooa uživatele výběrem konkrétní vyhledávání výsledku vrácený z vyhledávacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb048-116">User generated click events: By clicks in this document, we refer tooa user selecting a particular search result returned from a search query.</span></span> <span data-ttu-id="cb048-117">Klikněte na obvykle znamená, že dokument je výsledku relevantní pro konkrétní vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="cb048-117">A click generally means that a document is a relevant result for a specific search query.</span></span>

<span data-ttu-id="cb048-118">Pomocí propojení vyhledávání a klikněte na události s id korelace, je možné tooanalyze hello chování uživatelů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb048-118">By linking search and click events with a correlation id, it's possible tooanalyze hello behaviors of users on your application.</span></span> <span data-ttu-id="cb048-119">Tyto přehledy vyhledávání jsou možné tooobtain se jenom provoz protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="cb048-119">These search insights are impossible tooobtain with only search traffic logs.</span></span>

## <a name="how-tooimplement-search-traffic-analytics"></a><span data-ttu-id="cb048-120">Jak tooimplement hledání Analýza provozu</span><span class="sxs-lookup"><span data-stu-id="cb048-120">How tooimplement search traffic analytics</span></span>

<span data-ttu-id="cb048-121">signály Hello uvedeno v předchozí hello, část musí být shromáždili z hello vyhledávací aplikaci jako hello uživatel pracuje s ním.</span><span class="sxs-lookup"><span data-stu-id="cb048-121">hello signals mentioned in hello preceding section must be gathered from hello search application as hello user interacts with it.</span></span> <span data-ttu-id="cb048-122">Application Insights je rozšiřitelný monitorování řešení, k dispozici pro různé platformy, s možností flexibilní instrumentace.</span><span class="sxs-lookup"><span data-stu-id="cb048-122">Application Insights is an extensible monitoring solution, available for multiple platforms, with flexible instrumentation options.</span></span> <span data-ttu-id="cb048-123">Využití Application Insights umožňuje využít výhod vytvořili ve službě Azure Search toomake hello analýzy dat sestavy pro vyhledávání hello Power BI.</span><span class="sxs-lookup"><span data-stu-id="cb048-123">Usage of Application Insights lets you take advantage of hello Power BI search reports created by Azure Search toomake hello analysis of data easier.</span></span>

<span data-ttu-id="cb048-124">V hello [portál](https://portal.azure.com) stránka pro službu Azure Search, hello Analýza provozu vyhledávání okno obsahuje tahák podle tohoto vzoru telemetrie.</span><span class="sxs-lookup"><span data-stu-id="cb048-124">In hello [portal](https://portal.azure.com) page for your Azure Search service, hello Search Traffic Analytics blade contains a cheat sheet for following this telemetry pattern.</span></span> <span data-ttu-id="cb048-125">Můžete také vyberte nebo vytvořte prostředek Application Insights a najdete v části hello potřebná data z jednoho místa.</span><span class="sxs-lookup"><span data-stu-id="cb048-125">You can also select or create an Application Insights resource, and see hello necessary data, all in one place.</span></span>

![Pokyny Analýza provozu vyhledávání][1]

### <a name="1-select-an-application-insights-resource"></a><span data-ttu-id="cb048-127">1. Vyberte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="cb048-127">1. Select an Application Insights resource</span></span>

<span data-ttu-id="cb048-128">Třeba tooselect toouse prostředek Application Insights nebo, pokud již nemáte vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cb048-128">You need tooselect an Application Insights resource toouse or create one if you don't have one already.</span></span> <span data-ttu-id="cb048-129">Můžete použít na prostředek, který již v použití toolog hello vyžaduje vlastní události.</span><span class="sxs-lookup"><span data-stu-id="cb048-129">You can use a resource that's already in use toolog hello required custom events.</span></span>

<span data-ttu-id="cb048-130">Když vytváříte nový prostředek Application Insights, všechny typy aplikací jsou platné pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="cb048-130">When creating a new Application Insights resource, all application types are valid for this scenario.</span></span> <span data-ttu-id="cb048-131">Vyberte hello jednu, která nejlépe odpovídá hello platformu, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="cb048-131">Select hello one that best fits hello platform you are using.</span></span>

<span data-ttu-id="cb048-132">Klíč instrumentace hello potřebujete pro vytváření hello telemetrie klienta pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb048-132">You need hello instrumentation key for creating hello telemetry client for your application.</span></span> <span data-ttu-id="cb048-133">Můžete ho získat prostřednictvím hello řídicí panel portálu Application Insights, nebo můžete ho získat na stránce hello Analýza provozu vyhledávání, výběr instance hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="cb048-133">You can get it from hello Application Insights portal dashboard, or you can get it from hello Search Traffic Analytics page, selecting hello instance you want toouse.</span></span>

### <a name="2-instrument-your-application"></a><span data-ttu-id="cb048-134">2. Instrumentace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="cb048-134">2. Instrument your application</span></span>

<span data-ttu-id="cb048-135">Tato fáze je, kde instrumentace vlastní vyhledávací aplikaci, pomocí vaší hello vytvořené v kroku výše prostředek Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="cb048-135">This phase is where you instrument your own search application, using hello Application Insights resource your created in hello step above.</span></span> <span data-ttu-id="cb048-136">Proces toothis existují čtyři kroky:</span><span class="sxs-lookup"><span data-stu-id="cb048-136">There are four steps toothis process:</span></span>

<span data-ttu-id="cb048-137">**I. Vytvoření klienta telemetrie** jde hello objekt, který odesílá události toohello prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="cb048-137">**I. Create a telemetry client** This is hello object that sends events toohello Application Insights Resource.</span></span>

<span data-ttu-id="cb048-138">*C#*</span><span class="sxs-lookup"><span data-stu-id="cb048-138">*C#*</span></span>

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

<span data-ttu-id="cb048-139">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="cb048-139">*JavaScript*</span></span>

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

<span data-ttu-id="cb048-140">Pro jiné jazyky a platformy, najdete v části hello dokončení [seznamu](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span><span class="sxs-lookup"><span data-stu-id="cb048-140">For other languages and platforms, see hello complete [list](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span></span>

<span data-ttu-id="cb048-141">**II. Vyhledávání ID pro korelace požadavku** toocorrelate vyhledávání požadavky kliknutími, je nutné toohave id korelace, které se týkají těchto dvou různých událostí.</span><span class="sxs-lookup"><span data-stu-id="cb048-141">**II. Request a Search ID for correlation** toocorrelate search requests with clicks, it's necessary toohave a correlation id that relates these two distinct events.</span></span> <span data-ttu-id="cb048-142">Vyhledávání systému Azure poskytuje Id vyhledávání, pokud budete požadovat s záhlaví:</span><span class="sxs-lookup"><span data-stu-id="cb048-142">Azure Search provides you with a Search Id when you request it with a header:</span></span>

<span data-ttu-id="cb048-143">*C#*</span><span class="sxs-lookup"><span data-stu-id="cb048-143">*C#*</span></span>

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

<span data-ttu-id="cb048-144">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="cb048-144">*JavaScript*</span></span>

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

<span data-ttu-id="cb048-145">**III. Protokolování událostí, které vyhledávání**</span><span class="sxs-lookup"><span data-stu-id="cb048-145">**III. Log Search events**</span></span>

<span data-ttu-id="cb048-146">Pokaždé, když je žádost o vyhledávání vydané určitým uživatelem, je zapotřebí přihlásit, jako událost vyhledávání s hello následující schéma na vlastní události Application Insights:</span><span class="sxs-lookup"><span data-stu-id="cb048-146">Every time that a search request is issued by a user, you should log that as a search event with hello following schema on an Application Insights custom event:</span></span>

<span data-ttu-id="cb048-147">**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) hello vyhledávací dotaz (dodává v odpovědi vyhledávání hello) **IndexName**: index služby search (string) toobe dotaz **QueryTerms**: (string) hledaný text zadaný uživatelem hello **element resultcount element**: číslo (int) dokumentů, které byly vráceny (dodává v odpovědi vyhledávání hello)  **ScoringProfile**: název (string) hello vyhodnocování profil se má použít, pokud existuje</span><span class="sxs-lookup"><span data-stu-id="cb048-147">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello search query (comes in hello search response) **IndexName**: (string) search service index toobe queried **QueryTerms**: (string) search terms entered by hello user **ResultCount**: (int) number of documents that were returned (comes in hello search response) **ScoringProfile**: (string) name of hello scoring profile used, if any</span></span>

> [!NOTE]
> <span data-ttu-id="cb048-148">Počet požadavků na dotazy na uživatelem generovaný přidáním $count = true tooyour vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="cb048-148">Request count on user generated queries by adding $count=true tooyour search query.</span></span> <span data-ttu-id="cb048-149">Další informace najdete v části [sem](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span><span class="sxs-lookup"><span data-stu-id="cb048-149">See more information [here](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span></span>
>

> [!NOTE]
> <span data-ttu-id="cb048-150">Mějte na paměti, tooonly protokolu vyhledávací dotazy, které jsou generovány uživatelé.</span><span class="sxs-lookup"><span data-stu-id="cb048-150">Remember tooonly log search queries that are generated by users.</span></span>
>

<span data-ttu-id="cb048-151">*C#*</span><span class="sxs-lookup"><span data-stu-id="cb048-151">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

<span data-ttu-id="cb048-152">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="cb048-152">*JavaScript*</span></span>

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

<span data-ttu-id="cb048-153">**IV. Přihlaste se kliknutím na události**</span><span class="sxs-lookup"><span data-stu-id="cb048-153">**IV. Log Click events**</span></span>

<span data-ttu-id="cb048-154">Pokaždé, když uživatel klikne na dokument, který je signál, musíte být přihlášeni pro účely analýzy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="cb048-154">Every time that a user clicks on a document, that's a signal that must be logged for search analysis purposes.</span></span> <span data-ttu-id="cb048-155">Vlastní události toolog Application Insights tyto události použijte s hello následující schéma:</span><span class="sxs-lookup"><span data-stu-id="cb048-155">Use Application Insights custom events toolog these events with hello following schema:</span></span>

<span data-ttu-id="cb048-156">**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) hello související vyhledávací dotaz **identifikátorů DocId**: identifikátor dokumentu (string) **pozice** : stránka s výsledky (int) pořadí hello dokumentu v hello vyhledávání</span><span class="sxs-lookup"><span data-stu-id="cb048-156">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello related search query **DocId**: (string) document identifier **Position**: (int) rank of hello document in hello search results page</span></span>

> [!NOTE]
> <span data-ttu-id="cb048-157">Pozice odkazuje pořadí toohello mohutnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb048-157">Position refers toohello cardinal order in your application.</span></span> <span data-ttu-id="cb048-158">Jste volné tooset toto číslo dlouho se má stejné tooallow pro porovnání hello vždy.</span><span class="sxs-lookup"><span data-stu-id="cb048-158">You are free tooset this number, as long as it's always hello same, tooallow for comparison.</span></span>
>

<span data-ttu-id="cb048-159">*C#*</span><span class="sxs-lookup"><span data-stu-id="cb048-159">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

<span data-ttu-id="cb048-160">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="cb048-160">*JavaScript*</span></span>

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a><span data-ttu-id="cb048-161">3. Analýza s Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="cb048-161">3. Analyze with Power BI Desktop</span></span>

<span data-ttu-id="cb048-162">Po instrumentovány vaší aplikace a ověřit, že je aplikace správně připojené tooApplication statistiky, můžete použít předdefinované šablony vytvořené Azure Search pro Power BI desktop.</span><span class="sxs-lookup"><span data-stu-id="cb048-162">After you have instrumented your app and verified your application is correctly connected tooApplication Insights, you can use a predefined template created by Azure Search for Power BI desktop.</span></span>
<span data-ttu-id="cb048-163">Tato šablona obsahuje grafů a tabulek, které vám pomohou proveďte více informované rozhodnutí o tooimprove výkonu vyhledávání a závažnosti.</span><span class="sxs-lookup"><span data-stu-id="cb048-163">This template contains charts and tables that help you make more informed decisions tooimprove your search performance and relevance.</span></span>

<span data-ttu-id="cb048-164">tooinstantiate hello Power BI desktop šablony, potřebujete tři údaje o Application Insights.</span><span class="sxs-lookup"><span data-stu-id="cb048-164">tooinstantiate hello Power BI desktop template, you need three pieces of information about Application Insights.</span></span> <span data-ttu-id="cb048-165">Tato data lze nalézt v stránku hello Analýza provozu vyhledávání, když vyberete toouse prostředků hello</span><span class="sxs-lookup"><span data-stu-id="cb048-165">This data can be found in hello Search Traffic Analytics page, when you select hello resource toouse</span></span>

![Přehled Data aplikací v okně Analýza provozu vyhledávání hello][2]

<span data-ttu-id="cb048-167">Metrika součástí hello Power BI desktop šablony:</span><span class="sxs-lookup"><span data-stu-id="cb048-167">Metrics included in hello Power BI desktop template:</span></span>

*   <span data-ttu-id="cb048-168">Klikněte na tlačítko prostřednictvím rychlost (PEV.cenu): poměr uživatelů, kteří klikněte na konkrétní dokumentu toohello, počet celkový hledání.</span><span class="sxs-lookup"><span data-stu-id="cb048-168">Click through Rate (CTR): ratio of users who click on a specific document toohello number of total searches.</span></span>
*   <span data-ttu-id="cb048-169">Hledání bez kliknutí: podmínky pro Nejoblíbenější dotazy, které registrují bez kliknutí</span><span class="sxs-lookup"><span data-stu-id="cb048-169">Searches without clicks: terms for top queries that register no clicks</span></span>
*   <span data-ttu-id="cb048-170">Většina kliknutí na dokumenty: nejvíce kliknutí na dokumenty podle ID v hello posledních 24 hodin, 7 dní až 30 dní.</span><span class="sxs-lookup"><span data-stu-id="cb048-170">Most clicked documents: most clicked documents by ID in hello last 24 hours, 7 days, and 30 days.</span></span>
*   <span data-ttu-id="cb048-171">Dvojice oblíbených termín document: podmínky, jejichž výsledkem hello kliknutí na stejném dokumentu, seřazené podle kliknutí.</span><span class="sxs-lookup"><span data-stu-id="cb048-171">Popular term-document pairs: terms that result in hello same document clicked, ordered by clicks.</span></span>
*   <span data-ttu-id="cb048-172">Čas tooclick: kliknutí kategorizovaná podle doby od hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="cb048-172">Time tooclick: clicks bucketed by time since hello search query</span></span>

![Power BI šablonu pro čtení ze služby Application Insights][3]


## <a name="next-steps"></a><span data-ttu-id="cb048-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb048-174">Next Steps</span></span>
<span data-ttu-id="cb048-175">Instrumentace vyhledávání tooget výkonný a pronikavého data aplikací o vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="cb048-175">Instrument your search application tooget powerful and insightful data about your search service.</span></span>

<span data-ttu-id="cb048-176">Můžete najít další informace o Application Insights [zde](https://go.microsoft.com/fwlink/?linkid=842905).</span><span class="sxs-lookup"><span data-stu-id="cb048-176">You can find more information on Application Insights [here](https://go.microsoft.com/fwlink/?linkid=842905).</span></span> <span data-ttu-id="cb048-177">Navštivte Application Insights [stránce s cenami](https://azure.microsoft.com/pricing/details/application-insights/) toolearn Další informace o jejich různých úrovních služby.</span><span class="sxs-lookup"><span data-stu-id="cb048-177">Visit Application Insights [pricing page](https://azure.microsoft.com/pricing/details/application-insights/) toolearn more about their different service tiers.</span></span>

<span data-ttu-id="cb048-178">Další informace o vytváření úžasné sestav.</span><span class="sxs-lookup"><span data-stu-id="cb048-178">Learn more about creating amazing reports.</span></span> <span data-ttu-id="cb048-179">V tématu [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) podrobnosti</span><span class="sxs-lookup"><span data-stu-id="cb048-179">See [Getting started with Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) for details</span></span>

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
