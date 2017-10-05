---
title: "Monitorování webu SharePointu pomocí Application Insights"
description: "Zahájení monitorování nové aplikace s novým klíčem instrumentace"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2bfe5910-d673-4cf6-a5c1-4c115eae1be0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2016
ms.author: bwren
ms.openlocfilehash: a3b37674469a131016f46af590e1eee3ba4cdc73
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Monitorování webu SharePointu pomocí Application Insights
Služba Azure Application Insights monitoruje dostupnost, výkon a využití vaší aplikace. Zde se dozvíte, jak připravit prostředí pro web SharePointu.

## <a name="create-an-application-insights-resource"></a>Vytvořte prostředek Application Insights
Na webu [Azure Portal](https://portal.azure.com) vytvořte nový prostředek Application Insights. Vyberte jako typ aplikace ASP.NET.

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-sharepoint/01-new.png)

V okně, které se otevře, je místo, kde se zobrazí data o využití a výkonu vaší aplikace. Pokud se k němu chcete vrátit při příštím přihlášení k Azure, měli byste najít příslušnou dlaždici na úvodní obrazovce. Případně ho můžete najít kliknutím na Procházet.

## <a name="add-our-script-to-your-web-pages"></a>Přidání skriptu na webové stránky
V části Rychlý start získáte skript pro webové stránky:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Vložte skript těsně před značku &lt;/head&gt; každé stránky, kterou chcete sledovat. Pokud má daný web stránku předlohy, můžete se skript vložit. Například v projektu ASP.NET MVC ho vložíte do souboru View\Shared\_Layout.cshtml.

Skript obsahuje klíč instrumentace, který nasměruje telemetrii pro daný prostředek Application Insights.

### <a name="add-the-code-to-your-site-pages"></a>Přidání kódu na stránky webu
#### <a name="on-the-master-page"></a>Na stránku šablony
Pokud můžete upravit stránku šablony daného webu, přidáte monitorování na každou stránku webu.

Rezervujte si stránku šablony a upravte ji pomocí nástroje SharePoint Designer nebo jiného editoru.

![](./media/app-insights-sharepoint/03-master.png)

Přidejte kód těsně před značku </head>. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Nebo na jednotlivé stránky
Chcete-li monitorovat omezenou sadu stránek, přidejte skript jednotlivě na každou stránku. 

Vložte webovou část a vložte do ní fragment kódu.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Zobrazení dat o aplikaci
Znovu nasaďte aplikaci.

Vraťte se do okna vaší aplikace na webu [Azure Portal](https://portal.azure.com).

První události se zobrazí v hledání. 

![](./media/app-insights-sharepoint/09-search.png)

Pokud očekáváte více dat, klikněte za několik sekund na Aktualizovat.

V okně přehledu klikněte na **Analýza využití** a zobrazte grafy uživatelů, relací a zobrazení stránky:

![](./media/app-insights-sharepoint/06-usage.png)

Kliknutím na libovolný graf zobrazíte další podrobnosti – například Zobrazení stránky:

![](./media/app-insights-sharepoint/07-pages.png)

Nebo Uživatelé:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Zaznamenání ID uživatele
Standardní fragment kódu webové stránky nezachycuje ze SharePointu ID uživatele, ale můžete toho dosáhnout pomocí malé změny.

1. Zkopírujte klíč instrumentace vaší aplikace z rozevíracího seznamu Základy ve službě Application Insights. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. Nahraďte klíč instrumentace za text „XXXX“ v následujícím fragmentu kódu. 
2. Vložte skript do aplikaci SharePointu místo fragmentu kódu, který jste získali z portálu.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Další kroky
* [Webové testy](app-insights-monitor-web-app-availability.md) k monitorování dostupnosti vašeho webu
* [Application Insights](app-insights-overview.md) pro jiné typy aplikace.

<!--Link references-->


