---
title: "aaaAzure Application Insights – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy o Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: e27ee9b7d040a04828a9892865a6681b83f94326
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Nejčastější dotazy

## <a name="configuration-problems"></a>Problémy s konfigurací
*Mám potíže při nastavení Moje:*

* [Aplikace .NET](app-insights-asp-net-troubleshoot-no-data.md)
* [Monitorování aplikace již spuštěná](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure diagnostics](app-insights-azure-diagnostics.md)
* [Webová aplikace Java](app-insights-java-troubleshoot.md)

*Zobrazí žádná data ze svého serveru*

* [Sada výjimky brány firewall](app-insights-ip-addresses.md)
* [Nastavení serveru technologie ASP.NET](app-insights-monitor-performance-live-website-now.md)
* [Nastavení serveru Java](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Můžete použít Application Insights s...?

* [Webové aplikace na serveru služby IIS – místně nebo ve virtuálním počítači](app-insights-asp-net.md)
* [Webové aplikace v jazyce Java](app-insights-java-get-started.md)
* [Aplikace v Node.js](app-insights-nodejs.md)
* [Webové aplikace v Azure](app-insights-azure-web-apps.md)
* [Cloudové služby v Azure](app-insights-cloudservices.md)
* [Servery aplikace spuštěné v Docker](app-insights-docker.md)
* [Jednostránkové webové aplikace](app-insights-javascript.md)
* [Služby SharePoint](app-insights-sharepoint.md)
* [Aplikace na ploše systému Windows](app-insights-windows-desktop.md)
* [Jiné platformy](app-insights-platforms.md)

## <a name="is-it-free"></a>Je bezplatný?

Ano, pro experimentální použití. V hello základní ceny plánu můžete odeslat aplikace určité příspěvek na dat každý měsíc zdarma. volné povoleného užívání Hello je dostatečně velké na to toocover vývoj a publikování aplikace pro malý počet uživatelů. Můžete nastavit cap tooprevent více než po zadanou dobu data z zpracování.

Větší objemy telemetrie budou účtovat podle hello Gb. Poskytujeme některé tipy, jak příliš[omezit vaše poplatky](app-insights-pricing.md).

Plán podnikových Hello způsobuje poplatků za každý den, které každý uzel webového serveru odesílá telemetrická data. Je vhodný, pokud chcete, aby toouse průběžné exportovat ve velkém rozsahu.

[Čtení hello ceny plán](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Kolik je je výpočet nákladů?

* Otevřete hello **funkce + ceny** stránky v prostředek Application Insights. Je graf posledního použití. Svazek cap data, můžete nastavit, pokud chcete.
* Otevřete hello [Azure fakturace okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) toosee vaše faktur mezi všechny prostředky.

## <a name="q14"></a>Co Application Insights v projektu změnit?
Podrobnosti o Hello závisí na typu hello projektu. Pro webovou aplikaci:

* Přidá tyto soubory tooyour projektu:

  * Soubor ApplicationInsights.config.
  * AI.js
* Nainstaluje tyto balíčky NuGet:

  * *Application Insights API* – hello základní rozhraní API
  * *Application Insights API pro webové aplikace* -použít toosend telemetrie z hello server
  * *Application Insights API pro aplikace JavaScript* -použít toosend telemetrie z klienta hello

    balíčky Hello obsahovat tyto sestavení:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Vloží položky do:

  * Soubor web.config
  * souboru Packages.config je.
* (Nové projekty pouze – pokud jste [přidat existující projekt Application Insights tooan][start], máte toodo to ručně.) Fragmenty kódu vloží do kódu tooinitialize hello klient a server je s ID hello Application Insights zdroje. V aplikaci MVC, například kód je vložen do stránky předlohy hello Views/Shared/_Layout.cshtml

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Jak upgradovat ze starší verze sady SDK?
V tématu hello [poznámky k verzi](app-insights-release-notes.md) pro hello SDK vhodné tooyour typu aplikace.

## <a name="update"></a>Jak můžete změnit který Azure prostředek projektu odešle data?
V Průzkumníku řešení klikněte pravým tlačítkem na `ApplicationInsights.config` a zvolte **aktualizace Application Insights**. Můžete odeslat hello data tooan stávajícího nebo nového prostředku v Azure. změny Průvodce aktualizace Hello hello klíč instrumentace v souboru ApplicationInsights.config, která určuje, kde hello server SDK odešle svá data. Pokud zrušíte výběr "Aktualizovat vše", změní se také hello klíč, pokud se objeví v webových stránek.

## <a name="what-is-status-monitor"></a>Co je Monitorování stavu?

Aplikace na ploše, který můžete použít v vaší toohelp webového serveru IIS nakonfigurovat Application Insights ve webové aplikace. Ho nebude shromažďovat telemetrická data: můžete zastavit, pokud nejsou konfigurace aplikace. 

[Další informace](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Jaké telemetrických dat shromažďovaných pomocí Application Insights?

Ze serveru webové aplikace:

* Požadavky HTTP
* [Závislosti](app-insights-asp-net-dependencies.md). Volání: databáze SQL; HTTP volá tooexternal služeb; Azure Cosmos DB, table, úložiště objektů blob a fronty. 
* [Výjimky](app-insights-asp-net-exceptions.md) a trasování zásobníku.
* [Čítače výkonu](app-insights-performance-counters.md) – Pokud používáte [monitorování stavu](app-insights-monitor-performance-live-website-now.md), Azure monitoring(app-insights-azure-web-apps.md) nebo hello [Application Insights collectd zapisovače](app-insights-java-collectd.md).
* [Vlastní události a metriky](app-insights-api-custom-events-metrics.md) vám kód.
* [Protokoly trasování](app-insights-asp-net-trace-logs.md) Pokud nakonfigurujete hello příslušné kolekce.

Z [klienta webové stránky](app-insights-javascript.md):

* [Spočítá počet zobrazení stránky](app-insights-web-track-usage.md)
* [Volání AJAX](app-insights-asp-net-dependencies.md) požadavky vytvořené z spouštění skriptu.
* Stránka zobrazení načtení dat
* Počet uživatelů a relace
* [Ověřený uživatel ID](app-insights-api-custom-events-metrics.md#authenticated-users)

Z jiných zdrojů, pokud je nakonfigurovat:

* [Azure diagnostics](app-insights-azure-diagnostics.md)
* [Kontejnery docker](app-insights-docker.md)
* [Import tabulky tooAnalytics](app-insights-analytics-import.md)
* [OMS (analýzy protokolů)](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Můžete filtrovat nebo upravovat nějaké telemetrie?

Ano, v hello serveru, můžete napsat:

* Procesor toofilter telemetrie nebo přidání vlastnosti tooselected telemetrie položek před jejich odesláním z vaší aplikace.
* Položky tooall telemetrie inicializátoru tooadd vlastnosti telemetrie.

Další informace pro [ASP.NET](app-insights-api-filtering-sampling.md) nebo [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Jak jsou vypočítávány města, země a další data geografické umístění?

Podíváme se hello adresa IP (IPv4 nebo IPv6) hello webového klienta pomocí [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Prohlížeč telemetrie: shromažďujeme hello odesílatele IP adresu.
* Server telemetrie: modul Application Insights hello shromažďuje IP adresa klienta hello. Nejsou shromažďovány, pokud `X-Forwarded-For` nastavena.

Můžete nakonfigurovat hello `ClientIpHeaderTelemetryInitializer` tootake hello IP adresu z jiné záhlaví. V některých systémů, například se přesune pomocí serveru proxy a načtěte vyrovnávání nebo CDN příliš`X-Originating-IP`. [Další informace](http://apmtips.com/blog/2016/07/05/client-ip-address/).

Můžete [používat Power BI](app-insights-export-power-bi.md) toodisplay telemetrie požadavek na mapě.


## <a name="data"></a>Jak dlouho se data uchovávají portálu hello? Je bezpečné?
Podívejte se na [uchovávání dat a ochrana osobních údajů][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-hello-telemetry"></a>Mohou identifikovatelné osobní údaje (PII) být odeslány hello telemetrie?

To je možné, pokud kód odešle taková data. Může také dojít, pokud obsahovat PII variables v trasování zásobníku. Váš vývojový tým musí proveďte tooensure posuzování rizika správné zpracování PII. [Další informace o uchovávání dat a ochrany osobních údajů](app-insights-data-retention-privacy.md).

poslední oktet Hello hello klienta webové adresy je vždycky nastavený too0 po přijímání portálem hello.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>Moje iKey je viditelná ve zdroji webové stránky. 

* Toto je běžnou praxí v sledování řešení.
* Ji nelze použít toosteal vaše data.
* Je možné použít tooskew data nebo aktivační událost upozornění.
* Nebyly Slyšeli jsme, že všechny odběratel měl takovým problémům.

Vám může:

* Použití dvou samostatných iKeys (jednotlivé prostředky Application Insights) pro klienta a serveru data. Nebo
* Zápis proxy server, který běží na vašem serveru a mít hello webového klienta odesílat data prostřednictvím tohoto proxy serveru.

## <a name="post"></a>Jak zobrazit data POST ve vyhledávání diagnostiky?
Jsme nemáte protokolovat POST data automaticky, ale můžete použít volání TrackTrace: put hello dat v parametru zpráva hello. Tato akce nemá omezení velikosti delší než hello limity pro vlastnosti string, i když ho nelze filtrovat.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Použít jeden nebo více prostředků Application Insights?

Použijte jediný zdroj pro všechny součásti hello nebo role v rámci jedné obchodní systému. Použijte samostatný prostředky pro vývoj, testování a verze a nezávislé aplikace.

* [V tématu hello diskuzi](app-insights-separate-resources.md)
* [Příklad: Cloudová služba se worker a webové role](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-hello-instrumentation-key"></a>Dynamicky změna klíč instrumentace hello?

* [Tato diskuse](app-insights-separate-resources.md)
* [Příklad: Cloudová služba se worker a webové role](app-insights-cloudservices.md)

## <a name="what-are-hello-user-and-session-counts"></a>Co jsou hello uživatele a relace počty?

* Hello JavaScript SDK nastaví soubor cookie uživatele na hello webovému klientovi, tooidentify stávajícím uživatelům a aktivity toogroup souboru cookie relace.
* Pokud není žádný skript na straně klienta, můžete [nastavit soubory cookie na serveru hello](http://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Pokud jeden reálný uživatel používá váš web v různých prohlížečích nebo pomocí procházení v privátní nebo incognito nebo různých počítačů a potom se budou počítat více než jednou.
* tooidentify přihlášeného uživatele počítačům a prohlížeče, přidejte volání příliš[setAuthenticatedUserContect()](app-insights-api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a>I povolili všechno ve službě Application Insights?
| Co byste měli vidět | Jak tooget ho | Proč byste je |
| --- | --- | --- |
| Grafy dostupnosti |[Testy webu](app-insights-monitor-web-app-availability.md) |Vědět, že vaše webová aplikace je nahoru |
| Server aplikace výkonu: odezvy,... |[Přidejte Application Insights tooyour projekt](app-insights-asp-net.md) nebo [nainstalujte monitorování stavu AI na serveru](app-insights-monitor-performance-live-website-now.md) (nebo napsat vlastní kód příliš[sledování závislostí](app-insights-api-custom-events-metrics.md#trackdependency)) |Rozpoznat problémy s výkonu |
| Telemetrických závislostí |[Nainstalujte monitorování stavu AI na serveru](app-insights-monitor-performance-live-website-now.md) |Diagnostikovat problémy s databází nebo jiné externí součásti |
| Sám trasování zásobníku výjimky |[Vložit volání TrackException ve vašem kódu](app-insights-asp-net-exceptions.md) (ale některé jsou hlášeny automaticky) |Najít a diagnostikovat výjimky |
| Vyhledávání protokolu trasování |[Přidat adaptér protokolování](app-insights-asp-net-trace-logs.md) |Diagnostikovat výjimky výkonu problémy |
| Základy použití klienta: zobrazení stránky, relace,... |[Inicializátor JavaScript ve webových stránkách](app-insights-javascript.md) |Analýza využití |
| Vlastní metriky klienta |[Sledování volání do webové stránky](app-insights-api-custom-events-metrics.md) |Vylepšení činnost koncového uživatele |
| Vlastní metriky serveru |[Sledování volání na serveru](app-insights-api-custom-events-metrics.md) |Business intelligence |

## <a name="why-are-hello-counts-in-search-and-metrics-charts-unequal"></a>Proč se hello spočítá v grafech vyhledávání a metriky nerovné?

[Vzorkování](app-insights-sampling.md) snižuje hello počet položek telemetrie (požadavků, vlastní události a tak dále), ve skutečnosti odesílaných z portálu toohello aplikace. Do pole hledání zobrazí hello počet položek ve skutečnosti přijata. Metriky grafy, které zobrazí počet událostí se zobrazuje počet hello původní událostí, které došlo k chybě. 

Všechny položky, které se má u sebe transmmitted `itemCount` představuje vlastnost, která ukazuje, kolik Původní události této položky. tooobserve vzorkování v operaci, můžete spustit tento dotaz v Analytics:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Automation

### <a name="configuring-application-insights"></a>Konfigurace služby Application Insights

Můžete [psaní skriptů prostředí PowerShell](app-insights-powershell.md) pomocí nástroje Sledování prostředků Azure pro:

* Vytvořte a aktualizují prostředky s Application Insights.
* Nastavit hello ceny plánu.
* Získejte klíč instrumentace hello.
* Přidání metriky oznámení.
* Přidáte test dostupnosti.

Nelze nastavit metrika Explorer sestavy nebo nastavit průběžné export.

### <a name="querying-hello-telemetry"></a>Dotazování hello telemetrie

Použití hello [REST API](https://dev.applicationinsights.io/) toorun [Analytics](app-insights-analytics.md) dotazy.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Jak můžete nastavit upozornění na událost?

Azure výstrahy jsou pouze o metrikách. Vytvoření vlastní metriky protne hodnota prahové hodnoty při každém výskytu události. Nastavte na hello metrika výstrahu. Všimněte si, že: se vám zobrazí oznámení pokaždé, když metrika hello protne hello prahovou hodnotu v obou směrech; nezobrazí se oznámení do hello první překročení, bez ohledu na to, jestli hodnota počáteční hello je vysoká nebo Nízká; je vždy latence několik minut.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Existují poplatky za přenos dat mezi webové aplikace Azure a Application Insights?

* Pokud vaše webové aplikace Azure je hostována v datovém centru je koncový bod shromažďování Application Insights, není nijak zpoplatněn. 
* Pokud ve vašem datovém centru hostitele není žádný koncový bod kolekce, pak je zpoplatněná telemetrii aplikace [Azure odchozí poplatky](https://azure.microsoft.com/pricing/details/bandwidth/).

To nezávisí na je hostitelem prostředku Application Insights. Právě závisí na hello distribuční naše koncových bodů.

## <a name="can-i-send-telemetry-toohello-application-insights-portal"></a>Můžete odeslat portál Application Insights toohello telemetrie?

Doporučujeme používat naše sady SDK a používat hello (app-insights-api-custom-events-metrics.md) rozhraní API sady SDK. Variant hello SDK pro různé [platformy](app-insights-platforms.md). Tyto sady SDK zpracovávat ukládání do vyrovnávací paměti, komprese, omezení šířky pásma, opakování a tak dále. Ale hello [přijímání schématu](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) a [koncový bod protokolu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) jsou veřejné.

## <a name="can-i-monitor-an-intranet-web-server"></a>Můžete sledovat webový server intranetu?

Zde jsou dvě metody:

### <a name="firewall-door"></a>Dvířka brány firewall

Povolte webový server toosend telemetrie tooour koncové body https://dc.services.visualstudio.com:443 a https://rt.services.visualstudio.com:443. 

### <a name="proxy"></a>Proxy server

Směrovat provoz z vaší brány tooa serveru v síti intranet, pomocí tohoto nastavení v souboru ApplicationInsights.config:

```XML
<TelemetryChannel>
    <EndpointAddress>your gateway endpoint</EndpointAddress>
</TelemetryChannel>
```

Vaše brána by měl směrovat provoz hello toohttps://dc.services.visualstudio.com:443/v2/sledování

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Můžete spouštět testy dostupnosti webu na intranetový server?

Naše [webové testy](app-insights-monitor-web-app-availability.md) spustit na bodů přítomnosti distribuované kolem hello zeměkouli. Existují dvě řešení:

* Brány firewall dveře – povolit požadavky na server tooyour z [hello dlouhé a změnit seznam webové testovací agenti](app-insights-ip-addresses.md).
* Psát vlastní kód toosend pravidelné požadavky tooyour server z uvnitř intranetu. Můžete spustit webových testů Visual Studia pro tento účel. Hello tester může odeslat výsledky hello tooApplication statistika pomocí hello TrackAvailability() rozhraní API.

## <a name="more-answers"></a>Další odpovědi
* [Fórum Application Insights](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
