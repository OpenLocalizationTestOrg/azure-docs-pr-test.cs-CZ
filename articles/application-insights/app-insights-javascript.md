---
title: "webové aplikace aaaAzure Application Insights pro JavaScript | Microsoft Docs"
description: "Načtení zobrazení stránek a počty relací, data webového klienta a sledování vzorů využití. Zjištění výjimek a problémů s výkonem na webových stránkách v jazyce JavaScript."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a>Application Insights pro webové stránky
Zjistěte informace o hello výkonu a využití webové stránky nebo aplikace. Pokud přidáte [Application Insights](app-insights-overview.md) tooyour skript stránky, získáte časování načtení stránky a volání AJAX, počty a podrobnosti výjimek prohlížeče a selhání AJAX, a také uživatelům a počty relací. Všechny tyto hodnoty mohou být segmentovány podle stránky, klientského operačního systému a verze prohlížeče, zeměpisné polohy a ostatních dimenzí. Můžete nastavit výstrahy na počet selhání nebo pomalé načítání stránky. A vkládání trasovacího volání do kódu jazyka JavaScript, můžete sledovat, použití různých funkcí aplikace webovou stránku hello.

Application Insights můžete použít s jakýmikoli webovými stránkami – stačí přidat krátký kód jazyka JavaScript. Pokud používáte webovou službu [Java](app-insights-java-get-started.md) nebo [ASP.NET](app-insights-asp-net.md), můžete integrovat telemetrii ze serveru a klientů.

![Na stránce portal.azure.com otevřete prostředek vaší aplikace a klikněte na Prohlížeč.](./media/app-insights-javascript/03.png)

Musíte mít předplatné příliš[Microsoft Azure](https://azure.com). Pokud má váš tým předplatné pro společnosti, požádejte vlastníka tooadd hello vaše tooit Account Microsoft. Vývoj a méně rozsáhlé používání vás nebudou nic stát.

## <a name="set-up-application-insights-for-your-web-page"></a>Nastavte Application Insights pro svou webovou stránku
Přidejte hello zavaděč kód fragment kódu tooyour webové stránky, následujícím způsobem.

### <a name="open-or-create-application-insights-resource"></a>Otevření nebo vytvoření prostředku Application Insights
Hello prostředek Application Insights je, kde se zobrazí data o výkonu a využití vaší stránky. 

Přihlaste se na [portál Azure](https://portal.azure.com).

Pokud jste již nastavili monitorování na straně serveru hello vaší aplikace, již mít prostředek:

![Zvolte Procházet, služby pro vývojáře, Application Insights.](./media/app-insights-javascript/01-find.png)

Pokud ji nemáte, vytvořte ji:

![Zvolte Nový, služby pro vývojáře, Application Insights.](./media/app-insights-javascript/01-create.png)

*Již máte dotazy?* [Další informace o vytvoření prostředku](app-insights-create-new-resource.md).

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a>Přidat hello SDK skriptu tooyour aplikace nebo webové stránky
V části rychlý Start získáte hello skript pro webové stránky:

![V okně přehledu aplikace zvolte rychlý Start, získat kód toomonitor webové stránky. Zkopírujte skript hello.](./media/app-insights-javascript/02-monitor-web-page.png)

Vložte skript hello těsně před hello `</head>` značky každé stránce, kterou chcete tootrack. Pokud má daný web stránku předlohy, můžete umístit hello skriptu existuje. Například:

* Vložíte ho do projektu aplikace ASP.NET MVC do složky `View\Shared\_Layout.cshtml`.
* Na webu služby SharePoint, v Ovládacích panelech hello otevřete [nastavení webu / stránky předlohy](app-insights-sharepoint.md).

Hello skript obsahuje klíč instrumentace hello, který přesměruje prostředek Application Insights tooyour hello data. 

([Hlubší vysvětlení skriptu hello. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

*(Pokud používáte framework dobře známé webové stránky, vyhledejte adaptéry Application Insights. Například je k dispozici [modul AngularJS](http://ngmodules.org/modules/angular-appinsights).)*

## <a name="detailed-configuration"></a>Podrobná konfigurace
Nastavit můžete několik [Parametrů](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config), i když ve většině případů to není třeba. Můžete například zakázat nebo omezit hello počet volání Ajax hlášených na zobrazení stránky (tooreduce provoz). Nebo můžete nastavit ladění režimu toohave telemetrie přesunutí rychle prostřednictvím hello kanálu bez provedení dávkou.

tooset tyto parametry, vyhledejte tento řádek ve fragmentu kódu hello a po ní přidat další položky oddělené čárkami:

    })({
      instrumentationKey: "..."
      // Insert here
    });

Hello [dostupné parametry](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) zahrnují:

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <a name="run"></a>Spuštění aplikace
Spouštění vaší webové aplikace, použijte ji při toogenerate telemetrie a počkejte a několik sekund. Můžete buď spustit pomocí hello **F5** klíče na vývojovém počítači, nebo ji publikovat a umožnit uživatelům si ji vyzkoušeli.

Pokud chcete toocheck hello telemetrie, že webová aplikace odesílá tooApplication Insights, použijte ladicí nástroje prohlížeče (**F12** u mnoha prohlížečů). Data se odešlou toodc.services.visualstudio.com.

## <a name="explore-your-browser-performance-data"></a>Prozkoumejte data výkonu prohlížeče
Otevřete hello tooshow okno prohlížeče agregovat údaje o výkonu z prohlížečů uživatelů.

![Na stránce portal.azure.com otevřete prostředek vaší aplikace a klikněte na tlačítko Nastavení, Prohlížeč](./media/app-insights-javascript/03.png)

*Žádná data? Klikněte na tlačítko **aktualizovat** hello horní části stránky hello. Stále nic? Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).*

je Hello okno prohlížeče [okno Průzkumníku metrik](app-insights-metrics-explorer.md) s přednastavenými filtry a výběry grafu. Pokud chcete a uložit výsledek hello do oblíbených položek můžete upravit hello časové rozmezí, filtry a konfiguraci grafu. Klikněte na tlačítko **obnovit výchozí nastavení** tooget back toohello původní konfigurace okna.

## <a name="page-load-performance"></a>Stav zatížení stránky
V hello je horní části naleznete Segmentovaný grafu časů načtení stránky. Celková výška grafu hello Hello představuje hello Průměrná doba tooload a zobrazení stránky z vaší aplikace v prohlížečích vašich uživatelů. Hello čas se měří od, když hello prohlížeč odesílá počáteční požadavek HTTP hello dokud veškerých synchronních zatížení, které byly zpracovány události, včetně rozložení a spouštění skriptů. Neobsahuje asynchronní úlohy, například načítání webových součástí z volání AJAX.

Hello tabulka segmentuje hello celkovou dobu načítání stránky do hello [standardních časování definovaných pomocí W3C](http://www.w3.org/TR/navigation-timing/#processing-model). 

![](./media/app-insights-javascript/08-client-split.png)

Všimněte si, že hello *připojení k síti* čas je často nižší, než by se dalo očekávat, protože je průměrem přes všechny požadavky ze serveru toohello hello prohlížeče. Mnoho jednotlivých požadavků obsahuje dobu připojení 0, protože je již serveru toohello aktivní připojení.

### <a name="slow-loading"></a>Pomalé načítání?
Pomalé načítání stránek představuje hlavní zdroj nespokojenosti uživatelů. Pokud hello tabulka naznačuje pomalé načítání stránky, je snadné toodo trochu diagnostického výzkumu.

Hello graf znázorňuje hello průměr všech načtení stránky ve vaší aplikaci. toosee, pokud je problém hello omezen tooparticular stránky, naleznete další dolů hello okno, kde jsou mřížky segmentované podle adresy URL stránky:

![](./media/app-insights-javascript/09-page-perf.png)

Všimněte si počtu zobrazení hello stránky a směrodatné odchylky. Pokud je hello počet stránek velmi nízký, pak problém hello není dopad na uživatele mnohem. Vysoká směrodatná odchylka (srovnatelná toohello samotným průměrem) označuje velké rozdíly mezi jednotlivými měřeními.

**Přibližte si jednu adresu URL a jednu stránku zobrazení.** Klikněte na všechny stránky název toosee okno prohlížeče grafy filtrované právě toothat URL; a pak na instanci zobrazení stránky.

![](./media/app-insights-javascript/35.png)

Klikněte na tlačítko `...` pro úplný seznam vlastností pro danou událost nebo zkontrolujte volání Ajax hello a související události. Pomalá volání Ajax ovlivňují hello celkový čas načítání stránky, pokud jsou synchronní. Související události zahrnují požadavky serveru pro hello stejnou adresu URL (Pokud jste nastavili Application Insights na webovém serveru).

**Výkon stránky v čase.** Zpět v okně prohlížeče hello změňte hello zobrazení času načítání stránky mřížky do toosee grafu na řádku, pokud existuje v určitou dobu ke špičkám:

![Klikněte na tlačítko hello head hello mřížky a vyberte nový typ grafu](./media/app-insights-javascript/10-page-perf-area.png)

**Rozdělení pomocí dalších dimenzí.** Vaše stránky se pomalejší tooload v určité lokalitě prohlížeče, klientského operačního systému nebo uživatele? Přidejte nový graf a Experimentujte s hello **Seskupit podle** dimenze.

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a>Výkon AJAX
Ujistěte se, že všechna volání AJAX na webových stránkách dobře fungují. Jsou často používané toofill částí stránek asynchronně. I když hello celou stránku může načíst okamžitě, uživatelé mohou být frustrováni sledováním prázdných webových části čekání tooappear data v nich.

Volání AJAX provedená z webové stránky se zobrazí v okně prohlížeče hello jako závislosti.

Nachází souhrnné grafy v horní části okna hello hello:

![](./media/app-insights-javascript/31.png)

a níže pak podrobné mřížky:

![](./media/app-insights-javascript/33.png)

Klikněte na libovolný řádek pro konkrétní podrobnosti.

> [!NOTE]
> Pokud odstraníte hello filtru prohlížečů v okně hello, server a závislosti AJAX zahrnuty do těchto grafů. Klikněte na tlačítko Obnovit výchozí nastavení filtru tooreconfigure hello.
> 
> 

**toodrill do selhání volání Ajax** posuňte se dolů toohello mřížce selhání závislostí a pak klikněte na řádek určité instance toosee.

![](./media/app-insights-javascript/37.png)


Klikněte na tlačítko `...` pro úplnou telemetrii volání Ajax hello.

### <a name="no-ajax-calls-reported"></a>Žádná nahlášená volání Ajax?
Volání AJAX zahrnují HTTP a HTTPS volání ze skriptu hello webové stránky. Pokud je nevidíte nahlášená, zkontrolujte, že hello fragment kódu nenastavil hello `disableAjaxTracking` nebo `maxAjaxCallsPerView` [parametry](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="browser-exceptions"></a>Výjimky prohlížečů
V okně prohlížeče hello je graf souhrnu výjimek a mřížka typů výjimek dolů hello okno.

![](./media/app-insights-javascript/39.png)

Pokud nevidíte nahlášené výjimky prohlížeče, zkontrolujte, že hello fragment kódu nenastavil hello `disableExceptionTracking` [parametr](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="inspect-individual-page-view-events"></a>Zkontrolujte jednotlivé stránky zobrazení událostí

Obvykle jsou telemetrická zobrazení stránky analyzována pomocí Application Insights a zobrazí se pouze kumulativní sestavy s průměrem za všechny uživatele. Ale pro účely ladění si můžete také prohlédnout jednotlivé stránky zobrazení událostí.

V okně diagnostické vyhledávání hello nastavte filtry tooPage zobrazení.

![](./media/app-insights-javascript/12-search-pages.png)

Vyberte všechny události toosee podrobněji. Na stránce Podrobnosti hello klikněte na tlačítko "..." toosee více podrobností.

> [!NOTE]
> Pokud používáte [vyhledávání](app-insights-diagnostic-search.md), Všimněte si, že máte toomatch celá slova: "Abou" a "bout" neodpovídá "O".
> 
> 

Můžete také použít hello výkonné [analýzy protokolů dotazu jazyka](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch stránky zobrazení.

### <a name="page-view-properties"></a>Zobrazení vlastností stránky
* **Doba trvání zobrazení stránky** 
  
  * Ve výchozím nastavení hello čas ho stránku hello tooload trvá, z klienta toofull žádost o načtení (včetně pomocných souborů, ale s výjimkou asynchronních úloh, jako je například volání Ajax). 
  * Pokud nastavíte `overridePageViewDuration` v hello [konfiguraci stránky](#detailed-configuration), nejprve hello interval mezi tooexecution požadavek klienta z hello `trackPageView`. Pokud jste přesunuli trackPageView z obvyklé pozice po inicializaci hello hello skriptu, bude odrážet odlišnou hodnotu.
  * Pokud `overridePageViewDuration` je sada a dobu trvání argumentu k dispozici v hello `trackPageView()` volat, pak bude místo něj použita hodnota argumentu hello. 

## <a name="custom-page-counts"></a>Počty vlastních stránek
Ve výchozím nastavení počet stránek objeví vždy, když do hello prohlížeče klienta načte nová stránka.  Ale můžete chtít toocount další zobrazení stránky. Například stránka může zobrazit jeho obsah na kartách a chcete toocount na stránce, když hello uživatel přepíná karty. Nebo kód jazyka JavaScript v hello stránka může načíst nový obsah beze změny adresy URL hello prohlížeče.

Vložte podobné volání jazyka JavaScript v odpovídajícím bodě hello v klientském kódu:

    appInsights.trackPageView(myPageName);

Název stránky Hello může obsahovat stejné znaky jako adresa URL, ale cokoli za "#" hello nebo "?" je ignorována.

## <a name="usage-tracking"></a>Sledování využití
Chcete toofind na co vaši uživatelé dělají s vaší aplikací?

* [Další informace o sledování využití](app-insights-web-track-usage.md)
* [Další informace o vlastních událostech a metrikách rozhraní API](app-insights-api-custom-events-metrics.md).

## <a name="video"></a> Video


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <a name="next"></a> Další kroky
* [Sledování využití](app-insights-web-track-usage.md)
* [Vlastní události a metriky](app-insights-api-custom-events-metrics.md)
* [Sestavení vyhodnocení poučení](app-insights-web-track-usage.md)

