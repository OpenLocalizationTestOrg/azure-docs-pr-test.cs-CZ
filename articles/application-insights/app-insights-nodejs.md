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
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Monitorování služeb a aplikací Node.js pomocí Application Insights

[Azure Application Insights](app-insights-overview.md) monitoruje back-end služby a součásti po nasazení je toohelp jste [zjistit a rychle diagnostikovat výkonu a další problémy](app-insights-detect-triage-diagnose.md). Tuto službu můžete použít pro služby Node.js hostované kdekoli: ve vašem datacentru, na virtuálních počítačích nebo ve webových aplikacích Azure a dokonce i v dalších veřejných cloudech.

tooreceive, uložit a prozkoumejte monitorování data, postupujte podle následujících pokynů tooinclude agenta ve vašem kódu hello a nastavit odpovídající prostředek Application Insights v Azure. Hello agent odesílá data toothat prostředku pro další analýzy a zkoumání.

agenta Node.js Hello může automaticky sledovat příchozí a odchozí HTTP požadavků, několik systému metriky a výjimky. Počínaje verzí 0.20 dokáže monitorovat také některé běžné balíčky třetích stran, jako například `mongodb`, `mysql` a `redis`. Všechny události související se příchozí požadavek HTTP tooan jsou korelační rychlejší při řešení potíží.

Další aspekty aplikace můžete monitorovat a systému instrumentace je ručně pomocí rozhraní API agenta hello popsané dál.

![Příklady tabulek sledování výkonu](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Začínáme

Projděme si nastavení monitorování pro aplikaci nebo službu.

### <a name="resource"></a> Nastavení prostředku App Insights

**Než začnete**, ujistěte se, že máte předplatné Azure nebo [zdarma získejte nové předplatné][azure-free-offer]. Pokud už vaše organizace má předplatné Azure, můžete postupovat podle správce [tyto pokyny] [ add-aad-user] tooadd tooit je.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Teď se přihlásit toohello [portál Azure] [ portal] a vytvořte prostředek Application Insights, jak je znázorněno v následující hello – klikněte na tlačítko "New" > "Developer tools" > "Application Insights". Hello prostředků obsahuje koncový bod pro příjem telemetrická data, úložiště pro tato data uložena sestavy a řídicí panely, pravidla a konfigurace výstrah a další.

![Vytvoření prostředku App Insights](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Na stránce vytváření prostředků hello vyberte "Aplikace Node.js" hello aplikace typu rozevíracího seznamu. Typ aplikace Hello Určuje výchozí sadu hello řídicí panely a sestavy vytvořené pro vás. Ale nebojte se, každý prostředek App Insights může ve skutečnosti shromažďovat data z jakéhokoli jazyka a libovolné platformy.

![Formulář Nový prostředek App Insights](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a>Nastavení agenta Node.js hello

Nyní je čas tooinclude hello agenta ve vaší aplikaci, aby mohla shromažďovat data.
Začněte tím, že kopírování klíč instrumentace vaší prostředků (dále tooas vaše `ikey`) z portálu hello, jak je uvedeno níže. Hello App Insights systému používá tento klíč toomap data tooyour prostředků Azure, takže je nutné toospecify v proměnné prostředí nebo v kódu, hello agent toouse.  

![Zkopírování instrumentačního klíče](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Dál přidejte závislosti hello Node.js agenta knihovny tooyour aplikace prostřednictvím package.json. Z hello kořenové složky vaší aplikace spusťte:

```bash
npm install applicationinsights --save
```

Nyní je třeba tooexplicitly zatížení hello knihovně ve vašem kódu. Vzhledem k tomu, že hello agent vloží instrumentace do mnoha další knihovny, by se měly načíst co nejdříve, i před ostatními `require` příkazy. spuštění tooget, hello horní části souboru první .js přidejte:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Hello `setup` metoda nakonfiguruje klíč instrumentace hello (a tedy prostředků Azure) toobe používá ve výchozím nastavení pro všechny sledované položky. Volání `start` po dokončení toobegin shromažďování a odesílání telemetrických dat konfigurace.

Můžete zadat také ikey prostřednictvím proměnné prostředí hello APPINSIGHTS\_INSTRUMENTATIONKEY neprochází ručně příliš `setup()` nebo `getClient()`. Tento postup umožňuje zachovat ikeys mimo potvrdit zdrojového kódu a jiné ikeys toospecify pro různá prostředí.

Další možnosti konfigurace jsou popsány níže.

Hello agenta můžete vyzkoušet bez odesílání telemetrie nastavením hello instrumentace klíče tooany neprázdný řetězec.

### <a name="monitor"></a> Monitorování aplikace

Hello agent automaticky shromažďuje telemetrická data o běhu Node.js hello a některé běžné modulů třetích stran. Použít toogenerate teď vaše aplikace některé tato data.

Potom v hello [portál Azure] [ portal] procházet prostředek Application Insights toohello jste vytvořili dříve a vyhledejte první několik datových bodů v hello přehled časová osa jako hello následující obrázek. Proklikejte se prostřednictvím hello grafy pro další podrobnosti.

![První datové body](./media/app-insights-nodejs/12-first-perf.png)

Klikněte na tlačítko hello aplikace mapy tlačítko tooview hello topologie zjištěných pro aplikace, jako hello následující obrázek. Proklikejte se prostřednictvím součásti v mapě hello další podrobnosti.

![Mapa jedné aplikace](./media/app-insights-nodejs/06-appinsights_appmap.png)

Další informace o vaší aplikace a řešení problémů pomocí hello s dalšími zobrazeními k dispozici v části hello část "Prošetření".

![Část Prozkoumávání](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Žádná data?

Vzhledem k tomu, že hello agent dávek dat k odeslání může nastat zpoždění před položky jsou zobrazeny v portálu hello. Pokud nevidíte data v prostředku vyzkoušíme některá hello následující opravy:

* Pomocí aplikace hello některé více; Proveďte další akce toogenerate další telemetrie.
* Klikněte na tlačítko **aktualizovat** v zobrazení portálu zdrojů hello. Grafy automaticky aktualizovat sami pravidelně ale aktualizace vynutí tato toohappen okamžitě.
* Ověřte, že jsou otevřené [požadované výchozí porty](app-insights-ip-addresses.md).
* Otevřete hello [vyhledávání](app-insights-diagnostic-search.md) dlaždici a vyhledejte jednotlivé události.
* Zkontrolujte hello [– nejčastější dotazy][].


## <a name="agent-configuration"></a>Konfigurace agenta

Tady jsou způsoby konfigurace hello agentů a jejich výchozí hodnoty.

být toofully correlate událostí ve službě, že tooset `.setAutoDependencyCorrelation(true)`. To umožňuje hello agenta tootrack kontextu napříč asynchronní zpětná volání v Node.js.

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

## <a name="agent-api"></a>Rozhraní API agenta

<!-- TODO: Fully document agent API. -->

Hello .NET API agenta je podrobně popsán [zde](app-insights-api-custom-events-metrics.md).

Můžete sledovat všechny žádosti, události, Metrika nebo výjimky pomocí klienta Application Insights Node.js hello. Hello následující příklad ukazuje některé hello dostupných rozhraní API.

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

### <a name="track-your-dependencies"></a>Sledování závislostí

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

### <a name="add-a-custom-property-tooall-events"></a>Přidání události tooall vlastní vlastnosti

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>Sledování požadavků HTTP GET

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Sledování času spuštění serveru

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Další zdroje informací

* [Monitorování telemetrie hello portálu](app-insights-dashboards.md)
* [Psaní analytických dotazů do telemetrických dat](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[– nejčastější dotazy]: app-insights-troubleshoot-faq.md
