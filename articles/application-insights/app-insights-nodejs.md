---
title: "Monitorování služeb Node.js pomocí Azure Application Insights | Dokumentace Microsoftu"
description: "Monitorujte výkon a diagnostikujte problémy ve službách Node.js pomocí Application Insights."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: bwren
ms.openlocfilehash: ee65207e546c7050cc7bf35c36624fc49ad9eec4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a><span data-ttu-id="bd595-103">Monitorování služeb a aplikací Node.js pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="bd595-103">Monitor your Node.js services and apps with Application Insights</span></span>

<span data-ttu-id="bd595-104">[Azure Application Insights](app-insights-overview.md) monitoruje po nasazení vaše back-endové služby a komponenty a tím vám pomáhá [zjišťovat a rychle diagnostikovat problémy nejen s výkonem](app-insights-detect-triage-diagnose.md).</span><span class="sxs-lookup"><span data-stu-id="bd595-104">[Azure Application Insights](app-insights-overview.md) monitors your backend services and components after you deploy them to help you [discover and rapidly diagnose performance and other issues](app-insights-detect-triage-diagnose.md).</span></span> <span data-ttu-id="bd595-105">Tuto službu můžete použít pro služby Node.js hostované kdekoli: ve vašem datacentru, na virtuálních počítačích nebo ve webových aplikacích Azure a dokonce i v dalších veřejných cloudech.</span><span class="sxs-lookup"><span data-stu-id="bd595-105">Use it for Node.js services hosted anywhere: your datacenter, Azure VMs and Web Apps, and even other public clouds.</span></span>

<span data-ttu-id="bd595-106">Pokud chcete přijímat, ukládat a prozkoumávat data monitorování, postupujte podle následujících pokynů a vložte do svého kódu agenta a nastavte v Azure odpovídající prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bd595-106">To receive, store, and explore your monitoring data, follow the following instructions to include an agent in your code and set up a corresponding Application Insights resource in Azure.</span></span> <span data-ttu-id="bd595-107">Agent do tohoto prostředku odesílá data pro další analýzy a prozkoumávání.</span><span class="sxs-lookup"><span data-stu-id="bd595-107">The agent sends data to that resource for further analysis and exploration.</span></span>

<span data-ttu-id="bd595-108">Agent Node.js dokáže automaticky monitorovat příchozí a odchozí požadavky HTTP, řadu systémových metrik a výjimky.</span><span class="sxs-lookup"><span data-stu-id="bd595-108">The Node.js agent can automatically monitor incoming and outgoing HTTP requests, several system metrics, and exceptions.</span></span> <span data-ttu-id="bd595-109">Počínaje verzí 0.20 dokáže monitorovat také některé běžné balíčky třetích stran, jako například `mongodb`, `mysql` a `redis`.</span><span class="sxs-lookup"><span data-stu-id="bd595-109">Beginning in v0.20, it can also monitor some common third-party packages such as `mongodb`, `mysql`, and `redis`.</span></span> <span data-ttu-id="bd595-110">Všechny události související s příchozím požadavkem HTTP se korelují za účelem rychlejšího řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="bd595-110">All events related to an incoming HTTP request are correlated for faster troubleshooting.</span></span>

<span data-ttu-id="bd595-111">Můžete monitorovat více aspektů aplikace a systému, pokud je ručně nakonfigurujete pomocí rozhraní API agenta popsaného níže.</span><span class="sxs-lookup"><span data-stu-id="bd595-111">You can monitor more aspects of your app and system by manually instrumenting them using the agent API described later.</span></span>

![Příklady tabulek sledování výkonu](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a><span data-ttu-id="bd595-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="bd595-113">Getting Started</span></span>

<span data-ttu-id="bd595-114">Projděme si nastavení monitorování pro aplikaci nebo službu.</span><span class="sxs-lookup"><span data-stu-id="bd595-114">Let's step through setting up monitoring for an app or service.</span></span>

### <span data-ttu-id="bd595-115"><a name="resource"></a> Nastavení prostředku App Insights</span><span class="sxs-lookup"><span data-stu-id="bd595-115"><a name="resource"></a> Set up an App Insights resource</span></span>

<span data-ttu-id="bd595-116">**Než začnete**, ujistěte se, že máte předplatné Azure nebo [zdarma získejte nové předplatné][azure-free-offer].</span><span class="sxs-lookup"><span data-stu-id="bd595-116">**Before you start**, make sure you have an Azure subscription or [get a new one for free][azure-free-offer].</span></span> <span data-ttu-id="bd595-117">Pokud vaše organizace již má předplatné Azure, správce vás do něj může přidat pomocí [těchto pokynů][add-aad-user].</span><span class="sxs-lookup"><span data-stu-id="bd595-117">If your organization already has an Azure subscription, an administrator can follow [these instructions][add-aad-user] to add you to it.</span></span>

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

<span data-ttu-id="bd595-118">Nyní se přihlaste k webu [Azure Portal][portal] a vytvořte prostředek Application Insights, jak je znázorněno na následujícím obrázku – Klikněte na Nový > Vývojářské nástroje > Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bd595-118">Now log in to the [Azure portal][portal] and create an Application Insights resource as illustrated in the following - click "New" > "Developer tools" > "Application Insights".</span></span> <span data-ttu-id="bd595-119">Prostředek zahrnuje koncový bod pro příjem telemetrických dat, úložiště pro tato data, uložené sestavy a řídicí panely, konfigurace pravidel a upozornění a ještě více.</span><span class="sxs-lookup"><span data-stu-id="bd595-119">The resource includes an endpoint for receiving telemetry data, storage for this data, saved reports and dashboards, rule and alert configuration, and more.</span></span>

![Vytvoření prostředku App Insights](./media/app-insights-nodejs/03-new_appinsights_resource.png)

<span data-ttu-id="bd595-121">Na stránce vytváření prostředku vyberte z rozevírací nabídky typu aplikace možnost Aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="bd595-121">On the resource creation page, choose "Node.js Application" from the application type drop-down.</span></span> <span data-ttu-id="bd595-122">Typ aplikace určuje výchozí sadu řídicích panelů a sestav, které se pro vás vytvoří.</span><span class="sxs-lookup"><span data-stu-id="bd595-122">The app type determines the default set of dashboards and reports created for you.</span></span> <span data-ttu-id="bd595-123">Ale nebojte se, každý prostředek App Insights může ve skutečnosti shromažďovat data z jakéhokoli jazyka a libovolné platformy.</span><span class="sxs-lookup"><span data-stu-id="bd595-123">Don't worry though, any App Insights resource can in fact collect data from any language and platform.</span></span>

![Formulář Nový prostředek App Insights](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <span data-ttu-id="bd595-125"><a name="agent"></a> Nastavení agenta Node.js</span><span class="sxs-lookup"><span data-stu-id="bd595-125"><a name="agent"></a> Set up the Node.js agent</span></span>

<span data-ttu-id="bd595-126">Nyní je čas do aplikace vložit agenta, aby mohl shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="bd595-126">Now it's time to include the agent in your app so it can gather data.</span></span>
<span data-ttu-id="bd595-127">Začněte zkopírováním instrumentačního klíče vašeho prostředku (dále jen `ikey`) z portálu, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="bd595-127">Start by copying your resource's Instrumentation Key (hereinafter referred to as your `ikey`) from the portal as shown below.</span></span> <span data-ttu-id="bd595-128">Systém App Insights pomocí tohoto klíče mapuje data na váš prostředek Azure, proto ho musíte zadat ve svém kódu jako proměnnou prostředí, aby ho agent mohl použít.</span><span class="sxs-lookup"><span data-stu-id="bd595-128">The App Insights system uses this key to map data to your Azure resource so you need to specify it in an environment variable or your code for the agent to use.</span></span>  

![Zkopírování instrumentačního klíče](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

<span data-ttu-id="bd595-130">Dále přidejte do závislostí aplikace knihovnu agenta Node.js prostřednictvím souboru package.json.</span><span class="sxs-lookup"><span data-stu-id="bd595-130">Next, add the Node.js agent library to your app's dependencies via package.json.</span></span> <span data-ttu-id="bd595-131">Z kořenové složky aplikace spusťte:</span><span class="sxs-lookup"><span data-stu-id="bd595-131">From the root folder of your app, run:</span></span>

```bash
npm install applicationinsights --save
```

<span data-ttu-id="bd595-132">Nyní je potřeba knihovnu explicitně načíst v kódu.</span><span class="sxs-lookup"><span data-stu-id="bd595-132">Now you need to explicitly load the library in your code.</span></span> <span data-ttu-id="bd595-133">Vzhledem k tomu, že agent vkládá instrumentaci do mnoha dalších knihoven, musíte ho načíst co nejdříve, dokonce ještě před dalšími příkazy `require`.</span><span class="sxs-lookup"><span data-stu-id="bd595-133">Because the agent injects instrumentation into many other libraries, you should load it as early as possible, even before other `require` statements.</span></span> <span data-ttu-id="bd595-134">Začněte tak, že na začátek vašeho prvního souboru .js přidáte:</span><span class="sxs-lookup"><span data-stu-id="bd595-134">To get started, at the top of your first .js file add:</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

<span data-ttu-id="bd595-135">Metoda `setup` konfiguruje instrumentační klíč (a tedy prostředek Azure), který se použije jako výchozí pro všechny sledované položky.</span><span class="sxs-lookup"><span data-stu-id="bd595-135">The `setup` method configures the instrumentation key (and thus Azure resource) to be used by default for all tracked items.</span></span> <span data-ttu-id="bd595-136">Po dokončení konfigurace zavolejte metodu `start`, která spustí shromažďování a odesílání telemetrických dat.</span><span class="sxs-lookup"><span data-stu-id="bd595-136">Call `start` after configuration is finished to begin gathering and sending telemetry data.</span></span>

<span data-ttu-id="bd595-137">Istrumentační klíč můžete místo ručního předávání do metod `setup()` nebo`getClient()` zadat také přes proměnnou prostředí APPINSIGHTS\_INSTRUMENTATIONKEY.</span><span class="sxs-lookup"><span data-stu-id="bd595-137">You can also provide an ikey via the environment variable APPINSIGHTS\_INSTRUMENTATIONKEY instead of passing it manually to  `setup()` or `getClient()`.</span></span> <span data-ttu-id="bd595-138">Tento postup umožňuje oddělit instrumentační klíče od potvrzeného zdrojového kódu a určit různé instrumentační klíče pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd595-138">This practice lets you keep ikeys out of committed source code and to specify different ikeys for different environments.</span></span>

<span data-ttu-id="bd595-139">Další možnosti konfigurace jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="bd595-139">Additional configuration options are documented below.</span></span>

<span data-ttu-id="bd595-140">Agenta můžete vyzkoušet bez odesílání telemetrie tak, že instrumentační klíč nastavíte na neprázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="bd595-140">You can try the agent without sending telemetry by setting the instrumentation key to any non-empty string.</span></span>

### <span data-ttu-id="bd595-141"><a name="monitor"></a> Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="bd595-141"><a name="monitor"></a> Monitor your app</span></span>

<span data-ttu-id="bd595-142">Agent automaticky shromažďuje telemetrii o modulu runtime Node.js a některých dalších modulech třetích stran.</span><span class="sxs-lookup"><span data-stu-id="bd595-142">The agent automatically gathers telemetry about the Node.js runtime and some common third-party modules.</span></span> <span data-ttu-id="bd595-143">Nyní použijte svou aplikaci k vygenerování nějakých dat.</span><span class="sxs-lookup"><span data-stu-id="bd595-143">Use your application now to generate some of this data.</span></span>

<span data-ttu-id="bd595-144">Potom na webu [Azure Portal][portal] přejděte do prostředku Application Insights, který jste vytvořili dříve, a na časové ose Přehled vyhledejte vaše první datové body, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="bd595-144">Then, in the [Azure portal][portal] browse to the Application Insights resource you created earlier and look for your first few data points in the Overview timeline, as in the following image.</span></span> <span data-ttu-id="bd595-145">Přes grafy se můžete proklikat k dalším podrobnostem.</span><span class="sxs-lookup"><span data-stu-id="bd595-145">Click through the charts for more details.</span></span>

![První datové body](./media/app-insights-nodejs/12-first-perf.png)

<span data-ttu-id="bd595-147">Kliknutím na tlačítko Mapa aplikace zobrazte zjištěnou topologii vaší aplikace, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="bd595-147">Click the Application map button to view the topology discovered for your app, as in the following image.</span></span> <span data-ttu-id="bd595-148">Přes komponenty na mapě se můžete proklikat k dalším podrobnostem.</span><span class="sxs-lookup"><span data-stu-id="bd595-148">Click through components in the map for more details.</span></span>

![Mapa jedné aplikace](./media/app-insights-nodejs/06-appinsights_appmap.png)

<span data-ttu-id="bd595-150">Pomocí dalších zobrazení dostupných v části Prozkoumávání můžete zjistit další informace o vaší aplikaci a řešit problémy.</span><span class="sxs-lookup"><span data-stu-id="bd595-150">Learn more about your app and troubleshoot problems using the other views available under the "Investigate" section.</span></span>

![Část Prozkoumávání](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a><span data-ttu-id="bd595-152">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="bd595-152">No data?</span></span>

<span data-ttu-id="bd595-153">Vzhledem k tomu, že agent seskupuje data do dávek pro odesílání, může docházet k prodlevám v zobrazení položek na portálu.</span><span class="sxs-lookup"><span data-stu-id="bd595-153">Because the agent batches data for submission there may be a delay before items are displayed in the portal.</span></span> <span data-ttu-id="bd595-154">Pokud ve svém prostředku nevidíte data, vyzkoušejte některou z následujících oprav:</span><span class="sxs-lookup"><span data-stu-id="bd595-154">If you don't see data in your resource try some of the following fixes:</span></span>

* <span data-ttu-id="bd595-155">Používejte aplikaci trochu více a proveďte více akcí pro vygenerování další telemetrie.</span><span class="sxs-lookup"><span data-stu-id="bd595-155">Use the application some more; take more actions to generate more telemetry.</span></span>
* <span data-ttu-id="bd595-156">V zobrazení prostředku na portálu klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="bd595-156">Click **Refresh** in the portal resource view.</span></span> <span data-ttu-id="bd595-157">Grafy se samy pravidelně automaticky aktualizují, ale aktualizace vynutí, že k tomu dojde okamžitě.</span><span class="sxs-lookup"><span data-stu-id="bd595-157">Charts automatically refresh themselves periodically but refreshing forces this to happen immediately.</span></span>
* <span data-ttu-id="bd595-158">Ověřte, že jsou otevřené [požadované výchozí porty](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="bd595-158">Verify that [needed outgoing ports](app-insights-ip-addresses.md) are open.</span></span>
* <span data-ttu-id="bd595-159">Otevřete dlaždici [Vyhledávání](app-insights-diagnostic-search.md) a vyhledejte jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="bd595-159">Open the [Search](app-insights-diagnostic-search.md) tile and look for individual events.</span></span>
* <span data-ttu-id="bd595-160">Přečtěte si část [Nejčastější dotazy][].</span><span class="sxs-lookup"><span data-stu-id="bd595-160">Check the [FAQ][].</span></span>


## <a name="agent-configuration"></a><span data-ttu-id="bd595-161">Konfigurace agenta</span><span class="sxs-lookup"><span data-stu-id="bd595-161">Agent Configuration</span></span>

<span data-ttu-id="bd595-162">Dále jsou uvedeny metody konfigurace agenta a jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bd595-162">Following are the agent's configuration methods and their default values.</span></span>

<span data-ttu-id="bd595-163">Pro úplnou korelaci událostí ve službě nezapomeňte nastavit `.setAutoDependencyCorrelation(true)`.</span><span class="sxs-lookup"><span data-stu-id="bd595-163">To fully correlate events in a service, be sure to set `.setAutoDependencyCorrelation(true)`.</span></span> <span data-ttu-id="bd595-164">To agentovi umožní sledovat kontext napříč asynchronními zpětnými voláními v Node.js.</span><span class="sxs-lookup"><span data-stu-id="bd595-164">This allows the agent to track context across asynchronous callbacks in Node.js.</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a><span data-ttu-id="bd595-165">Rozhraní API agenta</span><span class="sxs-lookup"><span data-stu-id="bd595-165">Agent API</span></span>

<!-- TODO: Fully document agent API. -->

<span data-ttu-id="bd595-166">Rozhraní API agenta pro .NET je podrobně popsané [tady](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="bd595-166">The .NET agent API is fully described [here](app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="bd595-167">Pomocí klienta Application Insights pro Node.js můžete sledovat jakékoli žádosti, události, metriky nebo výjimky.</span><span class="sxs-lookup"><span data-stu-id="bd595-167">You can track any request, event, metric, or exception using the Application Insights Node.js client.</span></span> <span data-ttu-id="bd595-168">Následující příklad ukazuje některá dostupná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bd595-168">The following example demonstrates some of the available APIs.</span></span>

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a><span data-ttu-id="bd595-169">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="bd595-169">Track your dependencies</span></span>

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a><span data-ttu-id="bd595-170">Přidání vlastní vlastnosti do všech událostí</span><span class="sxs-lookup"><span data-stu-id="bd595-170">Add a custom property to all events</span></span>

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a><span data-ttu-id="bd595-171">Sledování požadavků HTTP GET</span><span class="sxs-lookup"><span data-stu-id="bd595-171">Track HTTP GET requests</span></span>

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a><span data-ttu-id="bd595-172">Sledování času spuštění serveru</span><span class="sxs-lookup"><span data-stu-id="bd595-172">Track server startup time</span></span>

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a><span data-ttu-id="bd595-173">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="bd595-173">More resources</span></span>

* [<span data-ttu-id="bd595-174">Monitorování telemetrických dat na portálu</span><span class="sxs-lookup"><span data-stu-id="bd595-174">Monitor your telemetry in the portal</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="bd595-175">Psaní analytických dotazů do telemetrických dat</span><span class="sxs-lookup"><span data-stu-id="bd595-175">Write Analytics queries over your telemetry</span></span>](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
<span data-ttu-id="bd595-176">[Nejčastější dotazy]: app-insights-troubleshoot-faq.md</span><span class="sxs-lookup"><span data-stu-id="bd595-176">[FAQ]: app-insights-troubleshoot-faq.md</span></span>
