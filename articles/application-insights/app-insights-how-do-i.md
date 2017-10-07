---
title: "aaaHow na to... ve službě Azure Application Insights | Microsoft Docs"
description: "Nejčastější dotazy týkající se ve službě Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a>Jak mám udělat ... pomocí Application Insights?
## <a name="get-an-email-when-"></a>Získat e-mailem při...
### <a name="email-if-my-site-goes-down"></a>Pokud web přestane fungovat e-mailu
Nastavit [dostupnosti webového testu](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>E-mailu, pokud je přetížena mé lokality
Nastavit [výstraha](app-insights-alerts.md) na **doba odezvy serveru**. Prahová hodnota mezi 1 a 2 sekundami by měly fungovat.

![](./media/app-insights-how-do-i/030-server.png)

Aplikace může také zobrazit známky kmen vrácením selhání kódy. Nastavit výstrahy na **neúspěšné požadavky**.

Pokud chcete tooset výstrahu na **výjimky serveru**, můžete mít toodo [některé další nastavení](app-insights-asp-net-exceptions.md) v datech toosee pořadí.

### <a name="email-on-exceptions"></a>E-mailu na výjimky
1. [Nastavili monitorování výjimek](app-insights-asp-net-exceptions.md)
2. [Nastavit výstrahy](app-insights-alerts.md) na hello výjimka počet metrika

### <a name="email-on-an-event-in-my-app"></a>E-mailu na události v mé aplikace
Předpokládejme, že byste chtěli tooget e-mailu, když dojde k určité události. Application Insights neposkytuje tato zařízení přímo, ale může [odešle výstrahu, když metriky překračuje prahovou hodnotu](app-insights-alerts.md).

Výstrahy lze nastavit u [vlastní metriky](app-insights-api-custom-events-metrics.md#trackmetric), i když není vlastních událostí. Některé tooincrease kód zápisu metriky při výskytu události hello:

    telemetry.TrackMetric("Alarm", 10);

nebo:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Vzhledem k tomu, že výstrahy dvou stavů, máte toosend nízkou hodnotu při zvažování hello výstraha toohave byl ukončen:

    telemetry.TrackMetric("Alarm", 0.5);

Vytvoření grafu v [metriky explorer](app-insights-metrics-explorer.md) toosee vaše výstrahy:

![](./media/app-insights-how-do-i/010-alarm.png)

Nyní nastavte výstrahy toofire při hello metrika překročí hodnotu mid na krátkou dobu:

![](./media/app-insights-how-do-i/020-threshold.png)

Nastavit hello průměrování období toohello minimální.

E-mailů získáte, když překročí hello metrika i pod prahovou hodnotou hello.

Některé tooconsider body:

* Výstraha má dva stavy ("upozornění" a "v pořádku"). Stav Hello vyhodnotí jenom v případě, že je obdržena metriky.
* E-mail je odeslán, pouze v případě změny stavu hello. To je důvod, proč máte toosend vysoké a nízké hodnoty metriky.
* tooevaluate hello výstrahy hello průměr je převzat hodnot hello přijatých přes hello předcházející období. Aby byla odeslána e-mailů častěji, než nastavený interval hello proběhne pokaždé, když byl přijat metriky.
* Vzhledem k tomu, že se odesílají e-mailů, "upozornění" a "v pořádku", můžete chtít tooconsider znovu myslím jednorázové událost jako podmínku dvou stavů. Například místo "Úloha byla dokončena" události, máte podmínku "úloha v průběhu", kde získat e-mailů v hello počáteční a koncové úlohy.

### <a name="set-up-alerts-automatically"></a>Nastavit výstrahy automaticky
[Pomocí prostředí PowerShell toocreate nové výstrahy](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a>Pomocí prostředí PowerShell tooManage Application Insights
* [Vytvoření nové prostředky](app-insights-powershell-script-create-resource.md)
* [Vytvoření nové výstrahy](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Samostatné telemetrická data z různých verzí

* Více rolí v aplikaci: pomocí jednoho prostředku Application Insights a filtrovat cloud_Rolename. [Další informace](app-insights-monitor-multi-role-apps.md)
* Oddělení vývoj, testování a verze: použití různých prostředků Application Insights. Vyberte si klíčů instrumentace hello ze souboru web.config. [Další informace](app-insights-separate-resources.md)
* Vytváření sestav sestavení verze: Přidání vlastnosti pomocí inicializátoru telemetrie. [Další informace](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Monitorování back-end serverů a aplikace klasické pracovní plochy
[Modul Windows Server SDK hello použití](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Vizualizaci dat
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Řídicí panel se metriky z více aplikací
* V [Explorer metrika](app-insights-metrics-explorer.md), graf přizpůsobit a uložit jako oblíbenou položku. Připnete toohello řídicí panel Azure.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Řídicí panel se data z jiných zdrojů a Application Insights
* [Export telemetrie tooPower BI](app-insights-export-power-bi.md).

Nebo

* Používání služby SharePoint jako řídicí panel, zobrazení dat ve webové části služby SharePoint. [Použít průběžné export a Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).  Použít databázi hello tooexamine PowerView a vytvoření webové části služby SharePoint pro PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Filtrovat anonymní nebo ověřeného uživatele
Pokud vaši uživatelé přihlásí, můžete nastavit hello [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users). (Ho nedojde automaticky.)

Pak můžete:

* Hledání ID konkrétního uživatele

![](./media/app-insights-how-do-i/110-search.png)

* Filtrovat metriky tooeither anonymní nebo ověřeného uživatele

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Upravit názvy vlastností nebo hodnot
Vytvoření [filtru](app-insights-api-filtering-sampling.md#filtering). Díky tomu můžete upravit nebo filtrování telemetrie před odesláním z vaší aplikace tooApplication statistiky.

## <a name="list-specific-users-and-their-usage"></a>Seznam konkrétních uživatelů a jejich využití
Pokud chcete příliš[vyhledávání pro konkrétní uživatele](#search-specific-users), můžete nastavit hello [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users).

Pokud chcete seznam uživatelů s daty, jako je například jaké stránky se podívejte se na nebo jak často se přihlásit, máte dvě možnosti:

* [Id sady ověřený uživatel](app-insights-api-custom-events-metrics.md#authenticated-users), [export databáze. tooa](app-insights-code-sample-export-sql-stream-analytics.md) a pomocí vhodného nástroje tooanalyze existuje uživatelská data.
* Pokud máte pouze malý počet uživatelů, odesílat vlastní události nebo metriky, pomocí hello data týkající se jako hello hodnota metriky nebo název události a id uživatele hello nastavení jako vlastnost. zobrazení stránky tooanalyze, nahraďte hello volání trackPageView standardní jazyka JavaScript. telemetrických dat na straně serveru tooanalyze, používat telemetrie inicializátoru tooadd hello uživatelské id tooall telemetrii serveru. Pak můžete filtrovat a segment metriky a hledání na id uživatele hello.

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a>Omezit přenos z mé aplikace tooApplication statistiky
* V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), zakažte všechny moduly, nepotřebujete, kolekce pro čítač výkonu takové hello.
* Použití [vzorkování a filtrování](app-insights-api-filtering-sampling.md) v hello SDK.
* Na webových stránkách omezte hello počet volání Ajax hlášených pro každé stránky zobrazení. Ve fragmentu hello skriptu po `instrumentationKey:...` , vložte: `,maxAjaxCallsPerView:3` (nebo vhodný číslo).
* Pokud používáte [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), výpočetní hello agregace dávek hodnot metriky před odesláním hello výsledek. Přetížení TrackMetric(), která poskytuje pro tento není k dispozici.

Další informace o [ceny a kvóty](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Zakázat telemetrii
příliš**dynamicky zastavit a spustit** hello shromažďování a předávání telemetrie ze serveru hello:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



příliš**zakázat vybrané standardní Kolektory** – například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář hello relevantní řádků v [souboru ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Můžete tak učinit, například pokud chcete toosend TrackRequest data.

## <a name="view-system-performance-counters"></a>Čítače výkonu systému zobrazení
Mezi hello metriky, které můžete zobrazit v Průzkumníku metrik jsou sady čítačů výkonu systému. Je předdefinovaný okno s názvem **servery** který zobrazí několik z nich.

![Otevřete prostředek Application Insights a klikněte na servery](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Pokud se zobrazí žádná data čítače výkonu
* **Server služby IIS** vlastní počítače nebo na virtuálním počítači. [Nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).
* **Webu Azure** -ještě nepodporujeme čítače výkonu. Existuje několik metriky, které můžete získat jako standardní součást hello webu Azure ovládací panely.
* **UNIX server** - [nainstalujte collectd](app-insights-java-collectd.md)

### <a name="toodisplay-more-performance-counters"></a>toodisplay další čítače výkonu
* První, [přidejte nový graf](app-insights-metrics-explorer.md) a zjistit, zda hello čítač je v hello základní nastavení, nabízíme.
* Pokud ne, [přidat sada toohello čítačů hello shromažďují modul čítače výkonu hello](app-insights-performance-counters.md).
