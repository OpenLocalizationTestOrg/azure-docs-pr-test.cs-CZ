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
# <a name="monitor-a-sharepoint-site-with-application-insights"></a><span data-ttu-id="478f7-103">Monitorování webu SharePointu pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="478f7-103">Monitor a SharePoint site with Application Insights</span></span>
<span data-ttu-id="478f7-104">Služba Azure Application Insights monitoruje dostupnost, výkon a využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="478f7-104">Azure Application Insights monitors the availability, performance and usage of your apps.</span></span> <span data-ttu-id="478f7-105">Zde se dozvíte, jak připravit prostředí pro web SharePointu.</span><span class="sxs-lookup"><span data-stu-id="478f7-105">Here you'll learn how to set it up for a SharePoint site.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="478f7-106">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="478f7-106">Create an Application Insights resource</span></span>
<span data-ttu-id="478f7-107">Na webu [Azure Portal](https://portal.azure.com) vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="478f7-107">In the [Azure portal](https://portal.azure.com), create a new Application Insights resource.</span></span> <span data-ttu-id="478f7-108">Vyberte jako typ aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="478f7-108">Choose ASP.NET as the application type.</span></span>

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-sharepoint/01-new.png)

<span data-ttu-id="478f7-110">V okně, které se otevře, je místo, kde se zobrazí data o využití a výkonu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="478f7-110">The blade that opens is the place where you'll see performance and usage data about your app.</span></span> <span data-ttu-id="478f7-111">Pokud se k němu chcete vrátit při příštím přihlášení k Azure, měli byste najít příslušnou dlaždici na úvodní obrazovce.</span><span class="sxs-lookup"><span data-stu-id="478f7-111">To get back to it next time you login to Azure, you should find a tile for it on the start screen.</span></span> <span data-ttu-id="478f7-112">Případně ho můžete najít kliknutím na Procházet.</span><span class="sxs-lookup"><span data-stu-id="478f7-112">Alternatively click Browse to find it.</span></span>

## <a name="add-our-script-to-your-web-pages"></a><span data-ttu-id="478f7-113">Přidání skriptu na webové stránky</span><span class="sxs-lookup"><span data-stu-id="478f7-113">Add our script to your web pages</span></span>
<span data-ttu-id="478f7-114">V části Rychlý start získáte skript pro webové stránky:</span><span class="sxs-lookup"><span data-stu-id="478f7-114">In Quick Start, get the script for web pages:</span></span>

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

<span data-ttu-id="478f7-115">Vložte skript těsně před značku &lt;/head&gt; každé stránky, kterou chcete sledovat. Pokud má daný web stránku předlohy, můžete se skript vložit.</span><span class="sxs-lookup"><span data-stu-id="478f7-115">Insert the script just before the &lt;/head&gt; tag of every page you want to track. If your website has a master page, you can put the script there.</span></span> <span data-ttu-id="478f7-116">Například v projektu ASP.NET MVC ho vložíte do souboru View\Shared\_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="478f7-116">For example, in an ASP.NET MVC project, you'd put it in View\Shared\_Layout.cshtml</span></span>

<span data-ttu-id="478f7-117">Skript obsahuje klíč instrumentace, který nasměruje telemetrii pro daný prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="478f7-117">The script contains the instrumentation key that directs the telemetry to your Application Insights resource.</span></span>

### <a name="add-the-code-to-your-site-pages"></a><span data-ttu-id="478f7-118">Přidání kódu na stránky webu</span><span class="sxs-lookup"><span data-stu-id="478f7-118">Add the code to your site pages</span></span>
#### <a name="on-the-master-page"></a><span data-ttu-id="478f7-119">Na stránku šablony</span><span class="sxs-lookup"><span data-stu-id="478f7-119">On the master page</span></span>
<span data-ttu-id="478f7-120">Pokud můžete upravit stránku šablony daného webu, přidáte monitorování na každou stránku webu.</span><span class="sxs-lookup"><span data-stu-id="478f7-120">If you can edit the site's master page, that will provide monitoring for every page in the site.</span></span>

<span data-ttu-id="478f7-121">Rezervujte si stránku šablony a upravte ji pomocí nástroje SharePoint Designer nebo jiného editoru.</span><span class="sxs-lookup"><span data-stu-id="478f7-121">Check out the master page and edit it using SharePoint Designer or any other editor.</span></span>

![](./media/app-insights-sharepoint/03-master.png)

<span data-ttu-id="478f7-122">Přidejte kód těsně před značku </head>.</span><span class="sxs-lookup"><span data-stu-id="478f7-122">Add the code just before the </head> tag.</span></span> 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a><span data-ttu-id="478f7-123">Nebo na jednotlivé stránky</span><span class="sxs-lookup"><span data-stu-id="478f7-123">Or on individual pages</span></span>
<span data-ttu-id="478f7-124">Chcete-li monitorovat omezenou sadu stránek, přidejte skript jednotlivě na každou stránku.</span><span class="sxs-lookup"><span data-stu-id="478f7-124">To monitor a limited set of pages, add the script separately to each page.</span></span> 

<span data-ttu-id="478f7-125">Vložte webovou část a vložte do ní fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="478f7-125">Insert a web part and embed the code snippet in it.</span></span>

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a><span data-ttu-id="478f7-126">Zobrazení dat o aplikaci</span><span class="sxs-lookup"><span data-stu-id="478f7-126">View data about your app</span></span>
<span data-ttu-id="478f7-127">Znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="478f7-127">Redeploy your app.</span></span>

<span data-ttu-id="478f7-128">Vraťte se do okna vaší aplikace na webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="478f7-128">Return to your application blade in the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="478f7-129">První události se zobrazí v hledání.</span><span class="sxs-lookup"><span data-stu-id="478f7-129">The first events will appear in Search.</span></span> 

![](./media/app-insights-sharepoint/09-search.png)

<span data-ttu-id="478f7-130">Pokud očekáváte více dat, klikněte za několik sekund na Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="478f7-130">Click Refresh after a few seconds if you're expecting more data.</span></span>

<span data-ttu-id="478f7-131">V okně přehledu klikněte na **Analýza využití** a zobrazte grafy uživatelů, relací a zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="478f7-131">From the overview blade, click **Usage analytics** to see to charts of users, sessions and page views:</span></span>

![](./media/app-insights-sharepoint/06-usage.png)

<span data-ttu-id="478f7-132">Kliknutím na libovolný graf zobrazíte další podrobnosti – například Zobrazení stránky:</span><span class="sxs-lookup"><span data-stu-id="478f7-132">Click any chart to see more details - for example Page Views:</span></span>

![](./media/app-insights-sharepoint/07-pages.png)

<span data-ttu-id="478f7-133">Nebo Uživatelé:</span><span class="sxs-lookup"><span data-stu-id="478f7-133">Or Users:</span></span>

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a><span data-ttu-id="478f7-134">Zaznamenání ID uživatele</span><span class="sxs-lookup"><span data-stu-id="478f7-134">Capturing User Id</span></span>
<span data-ttu-id="478f7-135">Standardní fragment kódu webové stránky nezachycuje ze SharePointu ID uživatele, ale můžete toho dosáhnout pomocí malé změny.</span><span class="sxs-lookup"><span data-stu-id="478f7-135">The standard web page code snippet doesn't capture the user id from SharePoint, but you can do that with a small modification.</span></span>

1. <span data-ttu-id="478f7-136">Zkopírujte klíč instrumentace vaší aplikace z rozevíracího seznamu Základy ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="478f7-136">Copy your app's instrumentation key from the Essentials drop-down in Application Insights.</span></span> 

    ![](./media/app-insights-sharepoint/02-props.png)

1. <span data-ttu-id="478f7-137">Nahraďte klíč instrumentace za text „XXXX“ v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="478f7-137">Substitute the instrumentation key for 'XXXX' in the snippet below.</span></span> 
2. <span data-ttu-id="478f7-138">Vložte skript do aplikaci SharePointu místo fragmentu kódu, který jste získali z portálu.</span><span class="sxs-lookup"><span data-stu-id="478f7-138">Embed the script in your SharePoint app instead of the snippet you get from the portal.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="478f7-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="478f7-139">Next Steps</span></span>
* <span data-ttu-id="478f7-140">[Webové testy](app-insights-monitor-web-app-availability.md) k monitorování dostupnosti vašeho webu</span><span class="sxs-lookup"><span data-stu-id="478f7-140">[Web tests](app-insights-monitor-web-app-availability.md) to monitor the availability of your site.</span></span>
* <span data-ttu-id="478f7-141">[Application Insights](app-insights-overview.md) pro jiné typy aplikace.</span><span class="sxs-lookup"><span data-stu-id="478f7-141">[Application Insights](app-insights-overview.md) for other types of app.</span></span>

<!--Link references-->


