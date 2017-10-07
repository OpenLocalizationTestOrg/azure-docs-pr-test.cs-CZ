---
title: "aaaMonitor web služby SharePoint s Application Insights"
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
ms.openlocfilehash: acfe99c24a4d77daec1017de0442ec952a1faba2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Monitorování webu SharePointu pomocí Application Insights
Azure Application Insights monitoruje hello dostupnosti, výkonu a využití vaší aplikace. Zde dozvíte, jak tooset ho pro web služby SharePoint.

## <a name="create-an-application-insights-resource"></a>Vytvořte prostředek Application Insights
V hello [portál Azure](https://portal.azure.com), vytvořte nový prostředek Application Insights. Vyberte jako typ aplikace hello ASP.NET.

![Klikněte na tlačítko Vlastnosti, vyberte klíč hello a stiskněte ctrl + C](./media/app-insights-sharepoint/01-new.png)

Hello okno, které se otevře je hello místě, kde se zobrazí data o využití a výkonu o vaší aplikaci. tooget back tooit příštím přihlášení tooAzure, byste měli najít dlaždici pro ni na obrazovce start hello. Případně kliknutím na tlačítko Procházet toofind ho.

## <a name="add-our-script-tooyour-web-pages"></a>Přidat naše skriptu tooyour webové stránky
V části rychlý Start získáte hello skript pro webové stránky:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Vložte skript hello těsně před hello &lt;/head&gt; značky každé stránce, kterou chcete tootrack. Pokud má daný web stránku předlohy, můžete umístit hello skriptu existuje. Například v projektu ASP.NET MVC ho vložíte do souboru View\Shared\_Layout.cshtml.

Hello skript obsahuje klíč instrumentace hello, který přesměruje prostředek Application Insights tooyour telemetrie hello.

### <a name="add-hello-code-tooyour-site-pages"></a>Přidat weby tooyour hello kódu
#### <a name="on-hello-master-page"></a>Na hlavní stránce hello
Pokud upravíte stránku předlohy hello webu, který poskytne monitorování na každé stránce webu hello.

Podívejte se na hlavní stránku hello a upravit ho pomocí návrháře služby SharePoint nebo v jiném editoru.

![](./media/app-insights-sharepoint/03-master.png)

Přidejte kód hello těsně před hello </head> značky. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Nebo na jednotlivé stránky
toomonitor omezenou sadu stránek, přidejte skript hello samostatně tooeach stránky. 

Vložit webovou část a Vložit fragment kódu hello v ní.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Zobrazení dat o aplikaci
Znovu nasaďte aplikaci.

Návratový tooyour okna aplikací v hello [portál Azure](https://portal.azure.com).

Hello první události se zobrazí v hledání. 

![](./media/app-insights-sharepoint/09-search.png)

Pokud očekáváte více dat, klikněte za několik sekund na Aktualizovat.

V okně Přehled hello, klikněte na tlačítko **analýzy využití** toosee toocharts uživatelů, relací a zobrazení stránky:

![](./media/app-insights-sharepoint/06-usage.png)

Klikněte na libovolný graf toosee podrobnosti – například zobrazení stránky:

![](./media/app-insights-sharepoint/07-pages.png)

Nebo Uživatelé:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Zaznamenání ID uživatele
fragment kódu standardní webovou stránku Hello neukládá hello id uživatele ze služby SharePoint, ale můžete to udělat pomocí malé změny.

1. Zkopírujte klíč instrumentace vaší aplikace z hello Essentials rozevíracího seznamu ve službě Application Insights. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. V následující fragment hello nahraďte klíč instrumentace hello 'XXXX'. 
2. Vložte skript hello ve vaší aplikaci SharePoint místo hello fragment kódu, které můžete získat z portálu hello.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that hello SP.UserProfiles.js file is loaded before hello custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get hello current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for hello target user. 
    // tooget hello PersonProperties object for hello current user, use hello 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load hello PersonProperties object and send hello request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if hello executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if hello executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Další kroky
* [Webové testy](app-insights-monitor-web-app-availability.md) toomonitor hello dostupnosti vaší lokality.
* [Application Insights](app-insights-overview.md) pro jiné typy aplikace.

<!--Link references-->


