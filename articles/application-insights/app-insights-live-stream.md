---
title: "aaaLive metriky datový proud s vlastní metriky a diagnostiky ve službě Azure Application Insights | Microsoft Docs"
description: "Monitorování webové aplikace v reálném čase s vlastní metriky a diagnostikovat problémy s za provozu informační kanál selhání, trasování a události."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: bwren
ms.openlocfilehash: 68ddfbf387379ea778c20280c4ec96baa06d3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Živý datový proud metriky: Diagnostikujte s latencí 1 sekundu & monitorovat 

Probe prezenční signál hello za provozu, v produkční webové aplikace pomocí živý datový proud metriky z [Application Insights](app-insights-overview.md). Vyberte a filtrovat metriky a výkonu toowatch čítačů v reálném čase, bez jakékoli služby tooyour narušení. Zkontrolujte trasování zásobníku z ukázkové se nezdařilo požadavky a výjimkami. Společně s [profileru](app-insights-profiler.md), [ladicí program snímku](app-insights-snapshot-debugger.md), a [testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests), živý datový proud metriky poskytuje výkonný a neinvazivní nástroj pro diagnostiku pro váš web za provozu lokalita.

Živý datový proud metriky můžete:

* Ověření opravu, při jeho vydání, pomocí sledování výkonu a selhání počty.
* Podívejte se na hello účinku testování zatížení a diagnostikovat problémy za provozu. 
* Zaměřit na konkrétní test relací nebo odfiltrovat známých problémů, výběru a filtrování hello metriky, které chcete toowatch.
* Získáte výjimka trasování, při jejich provádění.
* Experiment s filtry toofind hello nejdůležitější klíčových ukazatelů výkonu.
* Monitorování oknech výkonu čítač za provozu.
* Snadno identifikovat server, který je problémy s a filtrovat všechny hello, za provozu nebo klíčového ukazatele výkonu informačního kanálu toojust tohoto serveru.

[![Za provozu metriky streamování videa](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Živý datový proud metriky je aktuálně k dispozici pro aplikace ASP.NET z běžících místně nebo v hello cloudu. 

## <a name="get-started"></a>Začínáme

1. Pokud jste ještě [nenainstalovali Application Insights](app-insights-asp-net.md) ve vaší webové aplikaci ASP.NET nebo [aplikace pro Windows server](app-insights-windows-services.md), udělejte to teď. 
2. **Nejnovější verzi aktualizace toohello** hello Application Insights balíčku. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte **spravovat balíčky Nuget balíčky**. Otevřete hello **aktualizace** zkontrolujte **zahrnout předběžné verze**a vybrat všechny balíčky Microsoft.ApplicationInsights.* hello.

    Znovu nasaďte aplikaci.

3. V hello [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights hello pro vaši aplikaci a pak otevřete živý datový proud.

4. [Řídicí kanál zabezpečeného hello](#secure-the-control-channel) Pokud můžete použít citlivá data, jako jsou jména zákazníků v filtry.


![V okně Přehled hello klikněte na tlačítko živý datový proud](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Žádná data? Zkontrolujte server brány firewall

Zkontrolujte hello [Odchozí porty pro živý datový proud metriky](app-insights-ip-addresses.md#outgoing-ports) jsou otevřeny v bráně firewall hello vašich serverů. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Jak živý datový proud metriky liší od Průzkumníku metrik a analýzy?

| |Live Stream | Průzkumníku metrik a analýzy |
|---|---|---|
|Latence|Data zobrazená v rámci jedné sekundy|Agregován v minutách|
|Žádné uchování|Data udržuje je v grafu hello a potom je zahozen|[Data uchovávat 90 dnů](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|Na vyžádání|Data je streamování při otevření metriky za provozu|Data se odesílají vždycky, když je nainstalovaný a povolený hello SDK|
|Free|Je bezplatná živý datový proud dat|Subjektu příliš[ceny](app-insights-pricing.md)
|Vzorkování|Přenášejí se všechny vybrané metriky a čítače. Chyby a trasování zásobníku jsou odebírána data. TelemetryProcessors se nepoužijí.|Události může být [vzorků](app-insights-api-filtering-sampling.md)|
|Řídicí kanál|Filtr řízení signálů toohello SDK. Doporučujeme [zabezpečit tento kanál](#secure-channel).|Komunikace je jednosměrný, toohello portálu|


## <a name="select-and-filter-your-metrics"></a>Vyberte a filtrovat vaše metriky

(K dispozici v klasickém aplikace ASP.NET s hello nejnovější sadu SDK.)

Vlastní klíčového ukazatele výkonu za provozu můžete monitorovat použitím libovolný filtry na všechny telemetrie Application Insights z portálu hello. Klikněte na ovládací prvek hello filtr, který ukazuje, když jste myší nad žádné hello grafů. Následující graf Hello je vykreslení vlastní počtu žádostí o klíčového ukazatele výkonu s filtry na adresu URL a doba trvání atributy. Ověřte filtry s hello oddíl Preview datový proud, který ukazuje za provozu informační kanál telemetrická data, která odpovídá hello kritéria, která jste zadali v libovolném bodě v čase. 

![Vlastní požadavek klíčového ukazatele výkonu](./media/app-insights-live-stream/live-stream-filteredMetric.png)

Hodnota, která se liší od počtu můžete monitorovat. Hello možnosti závisí na typu hello datového proudu, který může být jakékoli telemetrie Application Insights: požadavky, závislostí, výjimek, trasování, události nebo metriky. Může být vlastní [vlastní měření](app-insights-api-custom-events-metrics.md#properties):

![Hodnota možnosti](./media/app-insights-live-stream/live-stream-valueoptions.png)

Kromě toho tooApplication telemetrii Insights, můžete taky sledovat všechny čítačů výkonu systému Windows, výběrem z možností hello datového proudu a poskytování hello název čítače výkonu hello.

Za provozu metriky agregován na dva body: místně na každém serveru a na všechny servery. Můžete změnit výchozí hello v buď pomocí dalších možností v hello příslušných rozevírací seznamy.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Ukázka Telemetrie: Vlastní za provozu diagnostických událostí
Ve výchozím nastavení zobrazuje hello za provozu informační kanál události ukázky neúspěšných požadavků a závislostí volání, výjimky, událostí a trasování. Klikněte na tlačítko hello hello použít kritéria toosee ikonu filtru v libovolném bodě v čase. 

![Výchozí živého kanálu](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Jako s metriky, můžete zadat všechny libovolný kritéria tooany typů telemetrie Application Insights hello. V tomto příkladu jsme se výběr konkrétního požadavku selhání, trasování a události. Také jsme jsou vyberete všechny výjimky a selhání závislostí.

![Vlastní za provozu informačního kanálu](./media/app-insights-live-stream/live-stream-events.png)

Poznámka: v současné době pro výjimky na základě zpráv kritéria používáte zpráva o výjimce nejkrajnější hello. V předchozím příkladu hello toofilter out hello neškodné výjimka zpráva o vnitřní výjimce (způsobem hello "<--" oddělovač) "hello klient odpojen." použití zprávu není – obsahuje kritéria pro "Chyba při čtení obsahu žádosti".

Zobrazit podrobnosti hello položky v hello kliknutím za provozu informačního kanálu. Je možné pozastavit hello kanálu buď kliknutím **pozastavit** nebo jednoduše posouvání dolů, nebo kliknutím na položku. Za provozu informačního kanálu bude pokračovat po přejděte zpět toohello horní, nebo kliknutím hello Čítač položek shromážděných během byla pozastavena.

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Filtrovat podle instance serveru

Pokud chcete toomonitor instanci role konkrétní server, můžete filtrovat podle serveru.

![Vzorkovat selhání za provozu](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>Požadavky na sady SDK
Vlastní živý datový proud metriky je k dispozici verze 2.4.0-beta2 nebo novější z [Application Insights SDK pro webové](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Mějte na paměti, možnost "Zahrnout předprodejní verze" tooselect ze Správce balíčků NuGet.

## <a name="secure-hello-control-channel"></a>Řídicí kanál zabezpečeného hello
Hello vlastní filtry kritéria, které zadáte jsou odesílány zpět toohello Live metriky součást hello Application Insights SDK. filtry Hello mohou obsahovat citlivé informace, jako je například customerIDs. Můžete nastavit hello kanál, zabezpečte pomocí tajný klíč rozhraní API kromě toohello klíč instrumentace.
### <a name="create-an-api-key"></a>Vytvořte klíč rozhraní API

![Vytvořte klíč rozhraní api](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-tooconfiguration"></a>Přidat tooConfiguration klíče rozhraní API
V souboru hello souboru applicationinsights.config přidejte hello AuthenticationApiKey toohello QuickPulseTelemetryModule:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
Nebo v kódu, nastavte ji na hello QuickPulseTelemetryModule:

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

Pokud znáte a důvěřovat, že všechny propojené servery hello, můžete zkusit hello vlastní filtry bez hello ověřený kanálu. Tato možnost je k dispozici po dobu šesti měsíců. Toto přepsání je požadovaná jednou každou novou relaci, nebo při přechodu do režimu online na nový server.

![Za provozu možnosti ověřování metriky](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Důrazně doporučujeme, abyste nastavili kanál hello ověření před vstupem potenciálně citlivé informace, jako je CustomerID v hello kritéria filtru.
>

## <a name="generating-a-performance-test-load"></a>Generování zátěžového testu výkonu

Pokud chcete toowatch hello účinek zatížení zvýšit, použijte hello testu výkonnosti okno. Simuluje požadavky od počet současně připojených uživatelů. Ho můžete spustit buď "ruční testy" (ping testy) jedné adresy URL, nebo můžete spustit [testu výkonnosti webu vícekrokový](app-insights-monitor-web-app-availability.md#multi-step-web-tests) odeslat (v hello stejný způsobem jako dostupnosti test).

> [!TIP]
> Po vytvoření testu výkonnosti hello otevřete testovací hello a hello okno živý datový proud v samostatném systému windows. Se zobrazí, když ve frontě hello spuštění testu výkonu a sledovat živý datový proud na hello stejnou dobu.
>


## <a name="troubleshooting"></a>Řešení potíží

Žádná data? Pokud je aplikace v chráněná síťová: Live Stream metriky používá jinou IP adresy, než ostatní telemetrie Application Insights. Zajistěte, aby [tyto IP adresy](app-insights-ip-addresses.md) jsou otevřeny v bráně firewall.



## <a name="next-steps"></a>Další kroky
* [Sledování použití s nástrojem Application Insights](app-insights-web-track-usage.md)
* [Pomocí vyhledávání diagnostiky](app-insights-diagnostic-search.md)
* [Profiler](app-insights-profiler.md)
* [Ladicí program snímku](app-insights-snapshot-debugger.md)
