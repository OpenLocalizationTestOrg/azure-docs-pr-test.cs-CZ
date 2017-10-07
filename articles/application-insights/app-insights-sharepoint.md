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
# <a name="monitor-a-sharepoint-site-with-application-insights"></a><span data-ttu-id="67d6b-103">Monitorování webu SharePointu pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="67d6b-103">Monitor a SharePoint site with Application Insights</span></span>
<span data-ttu-id="67d6b-104">Azure Application Insights monitoruje hello dostupnosti, výkonu a využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="67d6b-104">Azure Application Insights monitors hello availability, performance and usage of your apps.</span></span> <span data-ttu-id="67d6b-105">Zde dozvíte, jak tooset ho pro web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="67d6b-105">Here you'll learn how tooset it up for a SharePoint site.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="67d6b-106">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="67d6b-106">Create an Application Insights resource</span></span>
<span data-ttu-id="67d6b-107">V hello [portál Azure](https://portal.azure.com), vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="67d6b-107">In hello [Azure portal](https://portal.azure.com), create a new Application Insights resource.</span></span> <span data-ttu-id="67d6b-108">Vyberte jako typ aplikace hello ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="67d6b-108">Choose ASP.NET as hello application type.</span></span>

![Klikněte na tlačítko Vlastnosti, vyberte klíč hello a stiskněte ctrl + C](./media/app-insights-sharepoint/01-new.png)

<span data-ttu-id="67d6b-110">Hello okno, které se otevře je hello místě, kde se zobrazí data o využití a výkonu o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67d6b-110">hello blade that opens is hello place where you'll see performance and usage data about your app.</span></span> <span data-ttu-id="67d6b-111">tooget back tooit příštím přihlášení tooAzure, byste měli najít dlaždici pro ni na obrazovce start hello.</span><span class="sxs-lookup"><span data-stu-id="67d6b-111">tooget back tooit next time you login tooAzure, you should find a tile for it on hello start screen.</span></span> <span data-ttu-id="67d6b-112">Případně kliknutím na tlačítko Procházet toofind ho.</span><span class="sxs-lookup"><span data-stu-id="67d6b-112">Alternatively click Browse toofind it.</span></span>

## <a name="add-our-script-tooyour-web-pages"></a><span data-ttu-id="67d6b-113">Přidat naše skriptu tooyour webové stránky</span><span class="sxs-lookup"><span data-stu-id="67d6b-113">Add our script tooyour web pages</span></span>
<span data-ttu-id="67d6b-114">V části rychlý Start získáte hello skript pro webové stránky:</span><span class="sxs-lookup"><span data-stu-id="67d6b-114">In Quick Start, get hello script for web pages:</span></span>

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

<span data-ttu-id="67d6b-115">Vložte skript hello těsně před hello &lt;/head&gt; značky každé stránce, kterou chcete tootrack.</span><span class="sxs-lookup"><span data-stu-id="67d6b-115">Insert hello script just before hello &lt;/head&gt; tag of every page you want tootrack.</span></span> <span data-ttu-id="67d6b-116">Pokud má daný web stránku předlohy, můžete umístit hello skriptu existuje.</span><span class="sxs-lookup"><span data-stu-id="67d6b-116">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="67d6b-117">Například v projektu ASP.NET MVC ho vložíte do souboru View\Shared\_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="67d6b-117">For example, in an ASP.NET MVC project, you'd put it in View\Shared\_Layout.cshtml</span></span>

<span data-ttu-id="67d6b-118">Hello skript obsahuje klíč instrumentace hello, který přesměruje prostředek Application Insights tooyour telemetrie hello.</span><span class="sxs-lookup"><span data-stu-id="67d6b-118">hello script contains hello instrumentation key that directs hello telemetry tooyour Application Insights resource.</span></span>

### <a name="add-hello-code-tooyour-site-pages"></a><span data-ttu-id="67d6b-119">Přidat weby tooyour hello kódu</span><span class="sxs-lookup"><span data-stu-id="67d6b-119">Add hello code tooyour site pages</span></span>
#### <a name="on-hello-master-page"></a><span data-ttu-id="67d6b-120">Na hlavní stránce hello</span><span class="sxs-lookup"><span data-stu-id="67d6b-120">On hello master page</span></span>
<span data-ttu-id="67d6b-121">Pokud upravíte stránku předlohy hello webu, který poskytne monitorování na každé stránce webu hello.</span><span class="sxs-lookup"><span data-stu-id="67d6b-121">If you can edit hello site's master page, that will provide monitoring for every page in hello site.</span></span>

<span data-ttu-id="67d6b-122">Podívejte se na hlavní stránku hello a upravit ho pomocí návrháře služby SharePoint nebo v jiném editoru.</span><span class="sxs-lookup"><span data-stu-id="67d6b-122">Check out hello master page and edit it using SharePoint Designer or any other editor.</span></span>

![](./media/app-insights-sharepoint/03-master.png)

<span data-ttu-id="67d6b-123">Přidejte kód hello těsně před hello </head> značky.</span><span class="sxs-lookup"><span data-stu-id="67d6b-123">Add hello code just before hello </head> tag.</span></span> 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a><span data-ttu-id="67d6b-124">Nebo na jednotlivé stránky</span><span class="sxs-lookup"><span data-stu-id="67d6b-124">Or on individual pages</span></span>
<span data-ttu-id="67d6b-125">toomonitor omezenou sadu stránek, přidejte skript hello samostatně tooeach stránky.</span><span class="sxs-lookup"><span data-stu-id="67d6b-125">toomonitor a limited set of pages, add hello script separately tooeach page.</span></span> 

<span data-ttu-id="67d6b-126">Vložit webovou část a Vložit fragment kódu hello v ní.</span><span class="sxs-lookup"><span data-stu-id="67d6b-126">Insert a web part and embed hello code snippet in it.</span></span>

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a><span data-ttu-id="67d6b-127">Zobrazení dat o aplikaci</span><span class="sxs-lookup"><span data-stu-id="67d6b-127">View data about your app</span></span>
<span data-ttu-id="67d6b-128">Znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67d6b-128">Redeploy your app.</span></span>

<span data-ttu-id="67d6b-129">Návratový tooyour okna aplikací v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67d6b-129">Return tooyour application blade in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="67d6b-130">Hello první události se zobrazí v hledání.</span><span class="sxs-lookup"><span data-stu-id="67d6b-130">hello first events will appear in Search.</span></span> 

![](./media/app-insights-sharepoint/09-search.png)

<span data-ttu-id="67d6b-131">Pokud očekáváte více dat, klikněte za několik sekund na Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="67d6b-131">Click Refresh after a few seconds if you're expecting more data.</span></span>

<span data-ttu-id="67d6b-132">V okně Přehled hello, klikněte na tlačítko **analýzy využití** toosee toocharts uživatelů, relací a zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="67d6b-132">From hello overview blade, click **Usage analytics** toosee toocharts of users, sessions and page views:</span></span>

![](./media/app-insights-sharepoint/06-usage.png)

<span data-ttu-id="67d6b-133">Klikněte na libovolný graf toosee podrobnosti – například zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="67d6b-133">Click any chart toosee more details - for example Page Views:</span></span>

![](./media/app-insights-sharepoint/07-pages.png)

<span data-ttu-id="67d6b-134">Nebo Uživatelé:</span><span class="sxs-lookup"><span data-stu-id="67d6b-134">Or Users:</span></span>

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a><span data-ttu-id="67d6b-135">Zaznamenání ID uživatele</span><span class="sxs-lookup"><span data-stu-id="67d6b-135">Capturing User Id</span></span>
<span data-ttu-id="67d6b-136">fragment kódu standardní webovou stránku Hello neukládá hello id uživatele ze služby SharePoint, ale můžete to udělat pomocí malé změny.</span><span class="sxs-lookup"><span data-stu-id="67d6b-136">hello standard web page code snippet doesn't capture hello user id from SharePoint, but you can do that with a small modification.</span></span>

1. <span data-ttu-id="67d6b-137">Zkopírujte klíč instrumentace vaší aplikace z hello Essentials rozevíracího seznamu ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="67d6b-137">Copy your app's instrumentation key from hello Essentials drop-down in Application Insights.</span></span> 

    ![](./media/app-insights-sharepoint/02-props.png)

1. <span data-ttu-id="67d6b-138">V následující fragment hello nahraďte klíč instrumentace hello 'XXXX'.</span><span class="sxs-lookup"><span data-stu-id="67d6b-138">Substitute hello instrumentation key for 'XXXX' in hello snippet below.</span></span> 
2. <span data-ttu-id="67d6b-139">Vložte skript hello ve vaší aplikaci SharePoint místo hello fragment kódu, které můžete získat z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="67d6b-139">Embed hello script in your SharePoint app instead of hello snippet you get from hello portal.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="67d6b-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67d6b-140">Next Steps</span></span>
* <span data-ttu-id="67d6b-141">[Webové testy](app-insights-monitor-web-app-availability.md) toomonitor hello dostupnosti vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="67d6b-141">[Web tests](app-insights-monitor-web-app-availability.md) toomonitor hello availability of your site.</span></span>
* <span data-ttu-id="67d6b-142">[Application Insights](app-insights-overview.md) pro jiné typy aplikace.</span><span class="sxs-lookup"><span data-stu-id="67d6b-142">[Application Insights](app-insights-overview.md) for other types of app.</span></span>

<!--Link references-->


