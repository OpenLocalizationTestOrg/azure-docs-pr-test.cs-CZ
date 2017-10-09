---
title: aaaDashboards a navigace ve hello Azure Application Insights | Microsoft Docs
description: "Vytvořte zobrazení klíče APM grafy a dotazů."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a>Navigační a řídicí panely portálu Application Insights hello
Až budete mít [nastavte Application Insights na projektu](app-insights-overview.md), telemetrická data o výkonu a využití vaší aplikace se zobrazí v projektu na prostředek Application Insights hello [portál Azure](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Najít telemetrii
Přihlaste se toohello [portál Azure](https://portal.azure.com) a přejděte toohello prostředek Application Insights, kterou jste vytvořili pro vaši aplikaci.

![Klikněte na tlačítko Procházet, vyberte Application Insights a pak vaší aplikace.](./media/app-insights-dashboards/00-start.png)

okno Přehled Hello (stránky) pro vaši aplikaci se zobrazí souhrn klíčových diagnostiky hello metrik vaší aplikace a je toohello brány dalších funkcí portálu hello.

![Hlavní trasy tooview telemetrie](./media/app-insights-dashboards/010-oview.png)

Můžete přizpůsobit kterékoli z hello grafy a mřížky a připnete ji tooa řídicího panelu. Tímto způsobem můžete zahrnout společně hello klíče telemetrie z různých aplikací Centrální řídicí panel.

## <a name="dashboards"></a>Řídicí panely
Hello nejprve thing se zobrazí po přihlášení toohello [portálu Microsoft Azure](https://portal.azure.com) je řídicí panel. Zde můžete najednou přenést hello grafů, které jsou pro všechny vaše prostředky Azure, včetně telemetrie z nejdůležitějších tooyou [Azure Application Insights](app-insights-overview.md).

![Přizpůsobený řídicí panel.](./media/app-insights-dashboards/31.png)

1. **Přejděte toospecific prostředky** například vaše aplikace ve službě Application Insights: použití hello levém panelu.
2. **Návratový toohello aktuálního řídicího panelu**, nebo Přepnout poslední zobrazení tooother: použití hello rozevírací nabídce nahoře vlevo.
3. **Přepínač řídicí panely**: použití hello rozevírací nabídky na název řídicího panelu hello
4. **Vytvářet, upravovat a sdílet řídicí panely** na hello řídicího panelu nástrojů.
5. **Upravit řídicí panel hello**: najeďte na dlaždici a potom pomocí jeho horním panelu toomove, přizpůsobení, nebo ji odeberte.

## <a name="add-tooa-dashboard"></a>Přidat řídicí panel tooa
Když se díváte na okno nebo sadu grafů, který je zajímavé, budete moct připnout jeho kopii toohello řídicího panelu. Se zobrazí při příštím vrátíte existuje.

![toopin graf, pozastavte ukazatel myši nad ním a pak klikněte na tlačítko "..." v záhlaví hello.](./media/app-insights-dashboards/33.png)

1. Graf toodashboard PIN kód. Kopii hello grafu se zobrazí na řídicím panelu hello.
2. PIN kód hello celé okno toohello řídicí panel – zobrazí se na řídicím panelu hello jako dlaždici, prostřednictvím které po kliknutí.
3. Klikněte na tlačítko hello levého horního rohu tooreturn toohello aktuálního řídicího panelu. Pak můžete použít hello rozevírací nabídky tooreturn toohello aktuální zobrazení.

Všimněte si, že grafy jsou seskupené do dlaždice: dlaždici může obsahovat více než jeden graf. Připnete hello celou dlaždice toohello řídicího panelu.

Graf Hello se automaticky aktualizují s frekvencí, která závisí na graf hello časové rozmezí:

* Čas rozsah až hodinu too1: aktualizace každých 5 minut
* Čas rozsah 1 – 24 hodin: aktualizace každých 15 minut
* Čas rozsah vyšší než 24 hodin: (čas rozsah) / 60.

### <a name="pin-any-query-in-analytics"></a>Připnout jakýkoli dotaz ve Analytics
Můžete také [připnout Analytics](app-insights-analytics-using.md#pin-to-dashboard) grafy tooa [sdílené](#share-dashboards-with-your-team) řídicího panelu. To vám umožní tooadd grafy žádné libovolný dotazu spolu s hello standardní metriky. 

Výsledky jsou přepočítána automaticky každou hodinu. Klikněte na ikonu aktualizace hello na hello grafu toorecalculate okamžitě. (Obnovit v prohlížeči nezměněný.)

## <a name="adjust-a-tile-on-hello-dashboard"></a>Upravit dlaždice na řídicím panelu hello
Jakmile na dlaždici na řídicím panelu hello, můžete ji upravit.

![Pozastavte ukazatel myši nad grafu v pořadí tooedit ho.](./media/app-insights-dashboards/36.png)

1. Přidáte dlaždice toohello grafu.
2. Nastavte metriku hello, dimenze Seskupit podle a stylu grafu (tabulky, grafu).
3. Přetáhněte napříč hello diagram toozoom v; Klikněte na tlačítko hello vrácení zpět tlačítko tooreset hello timespan; nastavit vlastnosti filtru pro grafy hello na dlaždici hello.
4. Nastavte název dlaždice.

Dlaždice připnuté z okna Průzkumníka metriky mají další možnosti úprav než dlaždice připnuté z okno Přehled.

Hello původní dlaždice, který jste připnuli nemá vliv provedené úpravy.

## <a name="switch-between-dashboards"></a>Přepínání mezi řídicí panely
Můžete uložit více než jeden řídicí panel a mezi nimi přepínat. Pokud připnete graf nebo okna, přidají se toohello aktuálního řídicího panelu.

![tooswitch mezi řídicí panely, klikněte na řídicí panel a vyberte uložené řídicí panel. toocreate a uložte nový řídicí panel, klikněte na tlačítko Nový. toorearrange, klikněte na položku upravit.](./media/app-insights-dashboards/32.png)

Například můžete mít jeden řídicí panel pro zobrazení celé obrazovky v týmové místnosti hello a druhý pro obecné vývoj.

Na řídicím panelu hello, okno se zobrazí jako dlaždici: klikněte na něj toogo toohello okno. Graf replikuje hello grafu v původním umístění.

![Klikněte na okno hello tooopen dlaždice, který představuje](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Sdílet řídicí panely
Pokud jste vytvořili řídicí panel, můžete ji sdílet s ostatními uživateli.

![V záhlaví hello řídicí panel klikněte na sdílenou složku](./media/app-insights-dashboards/41.png)

Další informace o [role a řízení přístupu](app-insights-resources-roles-access-control.md).

## <a name="app-navigation"></a>Navigace aplikace
okno Přehled Hello je hello brány toomore informace o vaší aplikaci.

* **Všechny graf nebo dlaždice** – kliknutím na libovolnou dlaždici nebo grafu toosee více podrobností o co se zobrazí.

### <a name="overview-blade-buttons"></a>Přehled okno tlačítka
![Přehled okno horního navigačního panelu](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Průzkumníku metrik** ](app-insights-metrics-explorer.md) -vytvářet vlastní grafy výkonu a využití.
* [**Hledání** ](app-insights-diagnostic-search.md) – prozkoumat konkrétní instanci události, jako jsou žádosti o, výjimky, nebo protokolu trasování.
* [**Analýza** ](app-insights-analytics.md) -výkonných dotazů přes telemetrie.
* **Čas rozsah** -upravit rozsah hello zobrazuje všechny hello grafy v okně hello.
* **Odstranit** -odstranit hello prostředek Application Insights pro tuto aplikaci. Si také odebrat hello Application Insights balíčky z kódu aplikace, nebo upravit hello [klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) ve vaší aplikaci toodirect telemetrie tooa jiný prostředek služby Application Insights.

### <a name="essentials-tab"></a>Karta Essentials
* [Klíč instrumentace](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifikuje tento prostředek aplikace.
* Ceny - byly funkce svazek k dispozici a nastavené CAP k vzdálené ploše.

### <a name="app-navigation-bar"></a>Navigační panel aplikace
![Levý navigační panel](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Přehled** -návratový toohello okně přehledu aplikace.
* **Protokol aktivit** -výstrahy a událostí Azure pro správu.
* [**Řízení přístupu** ](app-insights-resources-roles-access-control.md) -poskytnout přístup tooteam členy a další.
* [**Značky** ](../azure-resource-manager/resource-group-using-tags.md) -použití značky toogroup vaší aplikace s ostatními.

PROZKOUMAT

* [**Mapa aplikace** ](app-insights-app-map.md) -Active mapování s hello součástí aplikace, odvozené z informací o závislostech hello.
* [**Inteligentní detekce** ](app-insights-proactive-diagnostics.md) – přečtěte si poslední výstrahy výkonu.
* [**Živý datový proud** ](app-insights-live-stream.md) – A fixed sadu téměř rychlých metriky, užitečné při nasazování nového sestavení nebo ladění.
* [**Dostupnost / webové testy** ](app-insights-monitor-web-app-availability.md) -odesílání pravidelných požadavky tooyour webové aplikace z kolem hello world.*
* [**Selhání, výkon** ](app-insights-web-monitor-performance.md) -výjimky, selhání sazby a odpovědi časový limit pro aplikaci tooyour požadavky a požadavky z vaší aplikace příliš[závislosti](app-insights-asp-net-dependencies.md).
* [**Výkon** ](app-insights-web-monitor-performance.md) -doba odezvy, závislosti odezvy.
* [Servery](app-insights-web-monitor-performance.md) -čítače výkonu. K dispozici, pokud jste [nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).
* **Prohlížeč** -stránka zobrazení a výkon AJAX. K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).
* **Využití** -stránka zobrazení, uživatele a počty relací. K dispozici, pokud jste [instrumentace webových stránek](app-insights-javascript.md).

KONFIGURACE

* **Začínáme** -vložené kurzu.
* **Vlastnosti** -klíč instrumentace, předplatné a id prostředku.
* [Výstrahy](app-insights-alerts.md) -metriky konfigurace výstrahy.
* [Průběžné export](app-insights-export-telemetry.md) -konfigurace export telemetrie tooAzure úložiště.
* [Testování výkonu](app-insights-monitor-web-app-availability.md#performance-tests) -nastavit syntetické zatížení na vašem webu.
* [Kvóty a ceny](app-insights-pricing.md) a [přijímání vzorkování](app-insights-sampling.md).
* **Přístup pomocí rozhraní API** -vytvořit [verze poznámek](app-insights-annotations.md) a hello API služby Data Access.
* [**Pracovní položky** ](app-insights-diagnostic-search.md#create-work-item) -připojení pracovní tooa sledování systému, takže můžete vytvořit chyby při kontrole telemetrie.

NASTAVENÍ

* [**Zamkne** ](../azure-resource-manager/resource-group-lock-resources.md) -zamknutí prostředků Azure
* [**Skriptu pro automatizaci** ](app-insights-powershell.md) -export definice hello prostředků Azure, aby ho můžete používat jako šablona nových prostředků toocreate.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky

|  |  |
| --- | --- |
| [Průzkumníku metrik](app-insights-metrics-explorer.md)<br/>Filtr a segment metriky |![Příklad hledání](./media/app-insights-dashboards/64.png) |
| [Diagnostické vyhledávání](app-insights-diagnostic-search.md)<br/>Najít a kontrola události, související události a vytvořit chyby |![Příklad hledání](./media/app-insights-dashboards/61.png) |
| [Analýzy](app-insights-analytics.md)<br/>Účinný dotazovací jazyk |![Příklad hledání](./media/app-insights-dashboards/63.png) |
