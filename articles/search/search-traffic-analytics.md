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
# <a name="what-is-search-traffic-analytics"></a>Co je analýza provozu vyhledávání
Analýza provozu vyhledávání je vzor pro implementaci zpětné vazby pro vaši službu vyhledávání. Tento vzor popisuje hello potřebná data a jak toocollect pomocí Application Insights, vedoucí odvětví pro monitorování služeb ve více platformách.

Analýza provozu vyhledávání vám umožní získat přehled o vaši službu vyhledávání a odemknutí přehledy o uživatele a jejich chování. Tak, že data o co vaši uživatelé vybrat, je možné toomake rozhodnutí, která je dále zlepšit vyhledávání a tooback vypnout, pokud jsou výsledky hello není co očekává.

Služba Azure Search nabízí telemetrii řešení, které se integruje s Azure Application Insights a Power BI tooprovide hlubší monitorování a sledování. Vzhledem k interakci s Azure Search je pouze prostřednictvím rozhraní API, musí být implementované hello telemetrie hello vývojáři hledání, pomocí pokynů hello na této stránce.

## <a name="identify-hello-relevant-search-data"></a>Identifikovat data relevantní vyhledávání hello

toohave užitečná hledání metriky, je nutné toolog některé signály z hello uživatelům aplikace hello vyhledávání. Tyto signály označily obsahu, že se uživatelé chtějí, a že za to, že relevantní tootheir potřebuje.

Existují dva signály, které potřebuje Analýza provozu vyhledávání:

1. Uživatelem generovaný vyhledávání události: jenom zajímavé vyhledávací dotazy iniciovaná uživatelem. Hledání požadavků používá toopopulate omezující, další obsah nebo nějaké interní informace o, nejsou důležité a zkreslit a usměrní výsledky.

2. Události kliknutí uživatelem generovaný: pomocí kliknutí v tomto dokumentu označujeme tooa uživatele výběrem konkrétní vyhledávání výsledku vrácený z vyhledávacího dotazu. Klikněte na obvykle znamená, že dokument je výsledku relevantní pro konkrétní vyhledávací dotaz.

Pomocí propojení vyhledávání a klikněte na události s id korelace, je možné tooanalyze hello chování uživatelů ve vaší aplikaci. Tyto přehledy vyhledávání jsou možné tooobtain se jenom provoz protokolů hledání.

## <a name="how-tooimplement-search-traffic-analytics"></a>Jak tooimplement hledání Analýza provozu

signály Hello uvedeno v předchozí hello, část musí být shromáždili z hello vyhledávací aplikaci jako hello uživatel pracuje s ním. Application Insights je rozšiřitelný monitorování řešení, k dispozici pro různé platformy, s možností flexibilní instrumentace. Využití Application Insights umožňuje využít výhod vytvořili ve službě Azure Search toomake hello analýzy dat sestavy pro vyhledávání hello Power BI.

V hello [portál](https://portal.azure.com) stránka pro službu Azure Search, hello Analýza provozu vyhledávání okno obsahuje tahák podle tohoto vzoru telemetrie. Můžete také vyberte nebo vytvořte prostředek Application Insights a najdete v části hello potřebná data z jednoho místa.

![Pokyny Analýza provozu vyhledávání][1]

### <a name="1-select-an-application-insights-resource"></a>1. Vyberte prostředek Application Insights

Třeba tooselect toouse prostředek Application Insights nebo, pokud již nemáte vytvořit. Můžete použít na prostředek, který již v použití toolog hello vyžaduje vlastní události.

Když vytváříte nový prostředek Application Insights, všechny typy aplikací jsou platné pro tento scénář. Vyberte hello jednu, která nejlépe odpovídá hello platformu, kterou používáte.

Klíč instrumentace hello potřebujete pro vytváření hello telemetrie klienta pro vaši aplikaci. Můžete ho získat prostřednictvím hello řídicí panel portálu Application Insights, nebo můžete ho získat na stránce hello Analýza provozu vyhledávání, výběr instance hello chcete toouse.

### <a name="2-instrument-your-application"></a>2. Instrumentace vaší aplikace

Tato fáze je, kde instrumentace vlastní vyhledávací aplikaci, pomocí vaší hello vytvořené v kroku výše prostředek Application Insights hello. Proces toothis existují čtyři kroky:

**I. Vytvoření klienta telemetrie** jde hello objekt, který odesílá události toohello prostředek Application Insights.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Pro jiné jazyky a platformy, najdete v části hello dokončení [seznamu](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Vyhledávání ID pro korelace požadavku** toocorrelate vyhledávání požadavky kliknutími, je nutné toohave id korelace, které se týkají těchto dvou různých událostí. Vyhledávání systému Azure poskytuje Id vyhledávání, pokud budete požadovat s záhlaví:

*C#*

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Protokolování událostí, které vyhledávání**

Pokaždé, když je žádost o vyhledávání vydané určitým uživatelem, je zapotřebí přihlásit, jako událost vyhledávání s hello následující schéma na vlastní události Application Insights:

**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) hello vyhledávací dotaz (dodává v odpovědi vyhledávání hello) **IndexName**: index služby search (string) toobe dotaz **QueryTerms**: (string) hledaný text zadaný uživatelem hello **element resultcount element**: číslo (int) dokumentů, které byly vráceny (dodává v odpovědi vyhledávání hello)  **ScoringProfile**: název (string) hello vyhodnocování profil se má použít, pokud existuje

> [!NOTE]
> Počet požadavků na dotazy na uživatelem generovaný přidáním $count = true tooyour vyhledávací dotaz. Další informace najdete v části [sem](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Mějte na paměti, tooonly protokolu vyhledávací dotazy, které jsou generovány uživatelé.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Přihlaste se kliknutím na události**

Pokaždé, když uživatel klikne na dokument, který je signál, musíte být přihlášeni pro účely analýzy vyhledávání. Vlastní události toolog Application Insights tyto události použijte s hello následující schéma:

**ServiceName**: název vyhledávací služby (string) **SearchId**: Jedinečný identifikátor (guid) hello související vyhledávací dotaz **identifikátorů DocId**: identifikátor dokumentu (string) **pozice** : stránka s výsledky (int) pořadí hello dokumentu v hello vyhledávání

> [!NOTE]
> Pozice odkazuje pořadí toohello mohutnosti ve vaší aplikaci. Jste volné tooset toto číslo dlouho se má stejné tooallow pro porovnání hello vždy.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Analýza s Power BI Desktop

Po instrumentovány vaší aplikace a ověřit, že je aplikace správně připojené tooApplication statistiky, můžete použít předdefinované šablony vytvořené Azure Search pro Power BI desktop.
Tato šablona obsahuje grafů a tabulek, které vám pomohou proveďte více informované rozhodnutí o tooimprove výkonu vyhledávání a závažnosti.

tooinstantiate hello Power BI desktop šablony, potřebujete tři údaje o Application Insights. Tato data lze nalézt v stránku hello Analýza provozu vyhledávání, když vyberete toouse prostředků hello

![Přehled Data aplikací v okně Analýza provozu vyhledávání hello][2]

Metrika součástí hello Power BI desktop šablony:

*   Klikněte na tlačítko prostřednictvím rychlost (PEV.cenu): poměr uživatelů, kteří klikněte na konkrétní dokumentu toohello, počet celkový hledání.
*   Hledání bez kliknutí: podmínky pro Nejoblíbenější dotazy, které registrují bez kliknutí
*   Většina kliknutí na dokumenty: nejvíce kliknutí na dokumenty podle ID v hello posledních 24 hodin, 7 dní až 30 dní.
*   Dvojice oblíbených termín document: podmínky, jejichž výsledkem hello kliknutí na stejném dokumentu, seřazené podle kliknutí.
*   Čas tooclick: kliknutí kategorizovaná podle doby od hello vyhledávací dotaz.

![Power BI šablonu pro čtení ze služby Application Insights][3]


## <a name="next-steps"></a>Další kroky
Instrumentace vyhledávání tooget výkonný a pronikavého data aplikací o vaši službu vyhledávání.

Můžete najít další informace o Application Insights [zde](https://go.microsoft.com/fwlink/?linkid=842905). Navštivte Application Insights [stránce s cenami](https://azure.microsoft.com/pricing/details/application-insights/) toolearn Další informace o jejich různých úrovních služby.

Další informace o vytváření úžasné sestav. V tématu [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) podrobnosti

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
