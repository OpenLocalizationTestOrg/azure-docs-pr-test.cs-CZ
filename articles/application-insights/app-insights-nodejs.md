---
title: "aaaMonitor Node.js služby pomocí služby Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 0a7e66990cd4d3a2fcaf3fa779adb336c861f8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a><span data-ttu-id="03a45-103">Monitorování služeb a aplikací Node.js pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="03a45-103">Monitor your Node.js services and apps with Application Insights</span></span>

<span data-ttu-id="03a45-104">[Azure Application Insights](app-insights-overview.md) monitoruje back-end služby a součásti po nasazení je toohelp jste [zjistit a rychle diagnostikovat výkonu a další problémy](app-insights-detect-triage-diagnose.md).</span><span class="sxs-lookup"><span data-stu-id="03a45-104">[Azure Application Insights](app-insights-overview.md) monitors your backend services and components after you deploy them toohelp you [discover and rapidly diagnose performance and other issues](app-insights-detect-triage-diagnose.md).</span></span> <span data-ttu-id="03a45-105">Tuto službu můžete použít pro služby Node.js hostované kdekoli: ve vašem datacentru, na virtuálních počítačích nebo ve webových aplikacích Azure a dokonce i v dalších veřejných cloudech.</span><span class="sxs-lookup"><span data-stu-id="03a45-105">Use it for Node.js services hosted anywhere: your datacenter, Azure VMs and Web Apps, and even other public clouds.</span></span>

<span data-ttu-id="03a45-106">tooreceive, uložit a prozkoumejte monitorování data, postupujte podle následujících pokynů tooinclude agenta ve vašem kódu hello a nastavit odpovídající prostředek Application Insights v Azure.</span><span class="sxs-lookup"><span data-stu-id="03a45-106">tooreceive, store, and explore your monitoring data, follow hello following instructions tooinclude an agent in your code and set up a corresponding Application Insights resource in Azure.</span></span> <span data-ttu-id="03a45-107">Hello agent odesílá data toothat prostředku pro další analýzy a zkoumání.</span><span class="sxs-lookup"><span data-stu-id="03a45-107">hello agent sends data toothat resource for further analysis and exploration.</span></span>

<span data-ttu-id="03a45-108">agenta Node.js Hello může automaticky sledovat příchozí a odchozí HTTP požadavků, několik systému metriky a výjimky.</span><span class="sxs-lookup"><span data-stu-id="03a45-108">hello Node.js agent can automatically monitor incoming and outgoing HTTP requests, several system metrics, and exceptions.</span></span> <span data-ttu-id="03a45-109">Počínaje verzí 0.20 dokáže monitorovat také některé běžné balíčky třetích stran, jako například `mongodb`, `mysql` a `redis`.</span><span class="sxs-lookup"><span data-stu-id="03a45-109">Beginning in v0.20, it can also monitor some common third-party packages such as `mongodb`, `mysql`, and `redis`.</span></span> <span data-ttu-id="03a45-110">Všechny události související se příchozí požadavek HTTP tooan jsou korelační rychlejší při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="03a45-110">All events related tooan incoming HTTP request are correlated for faster troubleshooting.</span></span>

<span data-ttu-id="03a45-111">Další aspekty aplikace můžete monitorovat a systému instrumentace je ručně pomocí rozhraní API agenta hello popsané dál.</span><span class="sxs-lookup"><span data-stu-id="03a45-111">You can monitor more aspects of your app and system by manually instrumenting them using hello agent API described later.</span></span>

![Příklady tabulek sledování výkonu](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a><span data-ttu-id="03a45-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="03a45-113">Getting Started</span></span>

<span data-ttu-id="03a45-114">Projděme si nastavení monitorování pro aplikaci nebo službu.</span><span class="sxs-lookup"><span data-stu-id="03a45-114">Let's step through setting up monitoring for an app or service.</span></span>

### <span data-ttu-id="03a45-115"><a name="resource"></a> Nastavení prostředku App Insights</span><span class="sxs-lookup"><span data-stu-id="03a45-115"><a name="resource"></a> Set up an App Insights resource</span></span>

<span data-ttu-id="03a45-116">**Než začnete**, ujistěte se, že máte předplatné Azure nebo [zdarma získejte nové předplatné][azure-free-offer].</span><span class="sxs-lookup"><span data-stu-id="03a45-116">**Before you start**, make sure you have an Azure subscription or [get a new one for free][azure-free-offer].</span></span> <span data-ttu-id="03a45-117">Pokud už vaše organizace má předplatné Azure, můžete postupovat podle správce [tyto pokyny] [ add-aad-user] tooadd tooit je.</span><span class="sxs-lookup"><span data-stu-id="03a45-117">If your organization already has an Azure subscription, an administrator can follow [these instructions][add-aad-user] tooadd you tooit.</span></span>

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

<span data-ttu-id="03a45-118">Teď se přihlásit toohello [portál Azure] [ portal] a vytvořte prostředek Application Insights, jak je znázorněno v následující hello – klikněte na tlačítko "New" > "Developer tools" > "Application Insights".</span><span class="sxs-lookup"><span data-stu-id="03a45-118">Now log in toohello [Azure portal][portal] and create an Application Insights resource as illustrated in hello following - click "New" > "Developer tools" > "Application Insights".</span></span> <span data-ttu-id="03a45-119">Hello prostředků obsahuje koncový bod pro příjem telemetrická data, úložiště pro tato data uložena sestavy a řídicí panely, pravidla a konfigurace výstrah a další.</span><span class="sxs-lookup"><span data-stu-id="03a45-119">hello resource includes an endpoint for receiving telemetry data, storage for this data, saved reports and dashboards, rule and alert configuration, and more.</span></span>

![Vytvoření prostředku App Insights](./media/app-insights-nodejs/03-new_appinsights_resource.png)

<span data-ttu-id="03a45-121">Na stránce vytváření prostředků hello vyberte "Aplikace Node.js" hello aplikace typu rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="03a45-121">On hello resource creation page, choose "Node.js Application" from hello application type drop-down.</span></span> <span data-ttu-id="03a45-122">Typ aplikace Hello Určuje výchozí sadu hello řídicí panely a sestavy vytvořené pro vás.</span><span class="sxs-lookup"><span data-stu-id="03a45-122">hello app type determines hello default set of dashboards and reports created for you.</span></span> <span data-ttu-id="03a45-123">Ale nebojte se, každý prostředek App Insights může ve skutečnosti shromažďovat data z jakéhokoli jazyka a libovolné platformy.</span><span class="sxs-lookup"><span data-stu-id="03a45-123">Don't worry though, any App Insights resource can in fact collect data from any language and platform.</span></span>

![Formulář Nový prostředek App Insights](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <span data-ttu-id="03a45-125"><a name="agent"></a>Nastavení agenta Node.js hello</span><span class="sxs-lookup"><span data-stu-id="03a45-125"><a name="agent"></a> Set up hello Node.js agent</span></span>

<span data-ttu-id="03a45-126">Nyní je čas tooinclude hello agenta ve vaší aplikaci, aby mohla shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="03a45-126">Now it's time tooinclude hello agent in your app so it can gather data.</span></span>
<span data-ttu-id="03a45-127">Začněte tím, že kopírování klíč instrumentace vaší prostředků (dále tooas vaše `ikey`) z portálu hello, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="03a45-127">Start by copying your resource's Instrumentation Key (hereinafter referred tooas your `ikey`) from hello portal as shown below.</span></span> <span data-ttu-id="03a45-128">Hello App Insights systému používá tento klíč toomap data tooyour prostředků Azure, takže je nutné toospecify v proměnné prostředí nebo v kódu, hello agent toouse.</span><span class="sxs-lookup"><span data-stu-id="03a45-128">hello App Insights system uses this key toomap data tooyour Azure resource so you need toospecify it in an environment variable or your code for hello agent toouse.</span></span>  

![Zkopírování instrumentačního klíče](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

<span data-ttu-id="03a45-130">Dál přidejte závislosti hello Node.js agenta knihovny tooyour aplikace prostřednictvím package.json.</span><span class="sxs-lookup"><span data-stu-id="03a45-130">Next, add hello Node.js agent library tooyour app's dependencies via package.json.</span></span> <span data-ttu-id="03a45-131">Z hello kořenové složky vaší aplikace spusťte:</span><span class="sxs-lookup"><span data-stu-id="03a45-131">From hello root folder of your app, run:</span></span>

```bash
npm install applicationinsights --save
```

<span data-ttu-id="03a45-132">Nyní je třeba tooexplicitly zatížení hello knihovně ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="03a45-132">Now you need tooexplicitly load hello library in your code.</span></span> <span data-ttu-id="03a45-133">Vzhledem k tomu, že hello agent vloží instrumentace do mnoha další knihovny, by se měly načíst co nejdříve, i před ostatními `require` příkazy.</span><span class="sxs-lookup"><span data-stu-id="03a45-133">Because hello agent injects instrumentation into many other libraries, you should load it as early as possible, even before other `require` statements.</span></span> <span data-ttu-id="03a45-134">spuštění tooget, hello horní části souboru první .js přidejte:</span><span class="sxs-lookup"><span data-stu-id="03a45-134">tooget started, at hello top of your first .js file add:</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

<span data-ttu-id="03a45-135">Hello `setup` metoda nakonfiguruje klíč instrumentace hello (a tedy prostředků Azure) toobe používá ve výchozím nastavení pro všechny sledované položky.</span><span class="sxs-lookup"><span data-stu-id="03a45-135">hello `setup` method configures hello instrumentation key (and thus Azure resource) toobe used by default for all tracked items.</span></span> <span data-ttu-id="03a45-136">Volání `start` po dokončení toobegin shromažďování a odesílání telemetrických dat konfigurace.</span><span class="sxs-lookup"><span data-stu-id="03a45-136">Call `start` after configuration is finished toobegin gathering and sending telemetry data.</span></span>

<span data-ttu-id="03a45-137">Můžete zadat také ikey prostřednictvím proměnné prostředí hello APPINSIGHTS\_INSTRUMENTATIONKEY neprochází ručně příliš `setup()` nebo `getClient()`.</span><span class="sxs-lookup"><span data-stu-id="03a45-137">You can also provide an ikey via hello environment variable APPINSIGHTS\_INSTRUMENTATIONKEY instead of passing it manually too `setup()` or `getClient()`.</span></span> <span data-ttu-id="03a45-138">Tento postup umožňuje zachovat ikeys mimo potvrdit zdrojového kódu a jiné ikeys toospecify pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="03a45-138">This practice lets you keep ikeys out of committed source code and toospecify different ikeys for different environments.</span></span>

<span data-ttu-id="03a45-139">Další možnosti konfigurace jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="03a45-139">Additional configuration options are documented below.</span></span>

<span data-ttu-id="03a45-140">Hello agenta můžete vyzkoušet bez odesílání telemetrie nastavením hello instrumentace klíče tooany neprázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="03a45-140">You can try hello agent without sending telemetry by setting hello instrumentation key tooany non-empty string.</span></span>

### <span data-ttu-id="03a45-141"><a name="monitor"></a> Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="03a45-141"><a name="monitor"></a> Monitor your app</span></span>

<span data-ttu-id="03a45-142">Hello agent automaticky shromažďuje telemetrická data o běhu Node.js hello a některé běžné modulů třetích stran.</span><span class="sxs-lookup"><span data-stu-id="03a45-142">hello agent automatically gathers telemetry about hello Node.js runtime and some common third-party modules.</span></span> <span data-ttu-id="03a45-143">Použít toogenerate teď vaše aplikace některé tato data.</span><span class="sxs-lookup"><span data-stu-id="03a45-143">Use your application now toogenerate some of this data.</span></span>

<span data-ttu-id="03a45-144">Potom v hello [portál Azure] [ portal] procházet prostředek Application Insights toohello jste vytvořili dříve a vyhledejte první několik datových bodů v hello přehled časová osa jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="03a45-144">Then, in hello [Azure portal][portal] browse toohello Application Insights resource you created earlier and look for your first few data points in hello Overview timeline, as in hello following image.</span></span> <span data-ttu-id="03a45-145">Proklikejte se prostřednictvím hello grafy pro další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="03a45-145">Click through hello charts for more details.</span></span>

![První datové body](./media/app-insights-nodejs/12-first-perf.png)

<span data-ttu-id="03a45-147">Klikněte na tlačítko hello aplikace mapy tlačítko tooview hello topologie zjištěných pro aplikace, jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="03a45-147">Click hello Application map button tooview hello topology discovered for your app, as in hello following image.</span></span> <span data-ttu-id="03a45-148">Proklikejte se prostřednictvím součásti v mapě hello další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="03a45-148">Click through components in hello map for more details.</span></span>

![Mapa jedné aplikace](./media/app-insights-nodejs/06-appinsights_appmap.png)

<span data-ttu-id="03a45-150">Další informace o vaší aplikace a řešení problémů pomocí hello s dalšími zobrazeními k dispozici v části hello část "Prošetření".</span><span class="sxs-lookup"><span data-stu-id="03a45-150">Learn more about your app and troubleshoot problems using hello other views available under hello "Investigate" section.</span></span>

![Část Prozkoumávání](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a><span data-ttu-id="03a45-152">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="03a45-152">No data?</span></span>

<span data-ttu-id="03a45-153">Vzhledem k tomu, že hello agent dávek dat k odeslání může nastat zpoždění před položky jsou zobrazeny v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="03a45-153">Because hello agent batches data for submission there may be a delay before items are displayed in hello portal.</span></span> <span data-ttu-id="03a45-154">Pokud nevidíte data v prostředku vyzkoušíme některá hello následující opravy:</span><span class="sxs-lookup"><span data-stu-id="03a45-154">If you don't see data in your resource try some of hello following fixes:</span></span>

* <span data-ttu-id="03a45-155">Pomocí aplikace hello některé více; Proveďte další akce toogenerate další telemetrie.</span><span class="sxs-lookup"><span data-stu-id="03a45-155">Use hello application some more; take more actions toogenerate more telemetry.</span></span>
* <span data-ttu-id="03a45-156">Klikněte na tlačítko **aktualizovat** v zobrazení portálu zdrojů hello.</span><span class="sxs-lookup"><span data-stu-id="03a45-156">Click **Refresh** in hello portal resource view.</span></span> <span data-ttu-id="03a45-157">Grafy automaticky aktualizovat sami pravidelně ale aktualizace vynutí tato toohappen okamžitě.</span><span class="sxs-lookup"><span data-stu-id="03a45-157">Charts automatically refresh themselves periodically but refreshing forces this toohappen immediately.</span></span>
* <span data-ttu-id="03a45-158">Ověřte, že jsou otevřené [požadované výchozí porty](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="03a45-158">Verify that [needed outgoing ports](app-insights-ip-addresses.md) are open.</span></span>
* <span data-ttu-id="03a45-159">Otevřete hello [vyhledávání](app-insights-diagnostic-search.md) dlaždici a vyhledejte jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="03a45-159">Open hello [Search](app-insights-diagnostic-search.md) tile and look for individual events.</span></span>
* <span data-ttu-id="03a45-160">Zkontrolujte hello [– nejčastější dotazy][].</span><span class="sxs-lookup"><span data-stu-id="03a45-160">Check hello [FAQ][].</span></span>


## <a name="agent-configuration"></a><span data-ttu-id="03a45-161">Konfigurace agenta</span><span class="sxs-lookup"><span data-stu-id="03a45-161">Agent Configuration</span></span>

<span data-ttu-id="03a45-162">Tady jsou způsoby konfigurace hello agentů a jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="03a45-162">Following are hello agent's configuration methods and their default values.</span></span>

<span data-ttu-id="03a45-163">být toofully correlate událostí ve službě, že tooset `.setAutoDependencyCorrelation(true)`.</span><span class="sxs-lookup"><span data-stu-id="03a45-163">toofully correlate events in a service, be sure tooset `.setAutoDependencyCorrelation(true)`.</span></span> <span data-ttu-id="03a45-164">To umožňuje hello agenta tootrack kontextu napříč asynchronní zpětná volání v Node.js.</span><span class="sxs-lookup"><span data-stu-id="03a45-164">This allows hello agent tootrack context across asynchronous callbacks in Node.js.</span></span>

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

## <a name="agent-api"></a><span data-ttu-id="03a45-165">Rozhraní API agenta</span><span class="sxs-lookup"><span data-stu-id="03a45-165">Agent API</span></span>

<!-- TODO: Fully document agent API. -->

<span data-ttu-id="03a45-166">Hello .NET API agenta je podrobně popsán [zde](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="03a45-166">hello .NET agent API is fully described [here](app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="03a45-167">Můžete sledovat všechny žádosti, události, Metrika nebo výjimky pomocí klienta Application Insights Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="03a45-167">You can track any request, event, metric, or exception using hello Application Insights Node.js client.</span></span> <span data-ttu-id="03a45-168">Hello následující příklad ukazuje některé hello dostupných rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="03a45-168">hello following example demonstrates some of hello available APIs.</span></span>

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
  client.trackRequest(req, res); // Place at hello beginning of your request handler
});
```

### <a name="track-your-dependencies"></a><span data-ttu-id="03a45-169">Sledování závislostí</span><span class="sxs-lookup"><span data-stu-id="03a45-169">Track your dependencies</span></span>

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

### <a name="add-a-custom-property-tooall-events"></a><span data-ttu-id="03a45-170">Přidání události tooall vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="03a45-170">Add a custom property tooall events</span></span>

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a><span data-ttu-id="03a45-171">Sledování požadavků HTTP GET</span><span class="sxs-lookup"><span data-stu-id="03a45-171">Track HTTP GET requests</span></span>

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a><span data-ttu-id="03a45-172">Sledování času spuštění serveru</span><span class="sxs-lookup"><span data-stu-id="03a45-172">Track server startup time</span></span>

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a><span data-ttu-id="03a45-173">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="03a45-173">More resources</span></span>

* [<span data-ttu-id="03a45-174">Monitorování telemetrie hello portálu</span><span class="sxs-lookup"><span data-stu-id="03a45-174">Monitor your telemetry in hello portal</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="03a45-175">Psaní analytických dotazů do telemetrických dat</span><span class="sxs-lookup"><span data-stu-id="03a45-175">Write Analytics queries over your telemetry</span></span>](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[– nejčastější dotazy]: app-insights-troubleshoot-faq.md
