---
title: "Monitorovat stav a využití pomocí Application Insights vaší aplikace"
description: "Začínáme s Application Insights. Analýza využití, dostupnosti a výkonu místního nebo aplikace Microsoft Azure."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 5b7b1f4a53cd2624ee8e2ab684ba6ba63252674f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-performance-in-web-applications"></a>Sledování výkonu webových aplikací


Ujistěte se, že aplikace je výkon na dobré a rychle zjistěte informace o případných selhání. [Application Insights] [ start] bude vás informovat o žádné problémy s výkonem a výjimkami a můžete najít a diagnostikovat základní příčiny.

Application Insights můžete monitorovat webové aplikace Java a ASP.NET a služby, služby WCF. Mohou být hostovaná místně, virtuálními počítači, nebo jako weby Microsoft Azure. 

Na straně klienta může trvat Application Insights telemetrie z webových stránek a širokou škálu zařízení se systémy iOS, Android a aplikací pro Windows Store.

>[!Note]
> Provedli jsme nové prostředí pro vyhledání pomalé, provádění stránky ve vaší webové aplikaci k dispozici. Pokud nemáte přístup k němu, povolení konfigurací možnosti preview s [Preview okno](app-insights-previews.md). Přečtěte si informace o toto nové prostředí v [najít a opravit kritická místa výkonu s interaktivní zkoumání výkonu](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Nastavení monitorování výkonu
Pokud jste ještě nepřidali Application Insights do projektu (to znamená, pokud nemá soubor ApplicationInsights.config), vyberte jednu z těchto způsobů, jak začít:

* [Webové aplikace ASP.NET](app-insights-asp-net.md)
  * [Přidat monitorování výjimek](app-insights-asp-net-exceptions.md)
  * [Přidat monitorování závislostí](app-insights-monitor-performance-live-website-now.md)
* [Webových aplikací J2EE](app-insights-java-get-started.md)
  * [Přidat monitorování závislostí](app-insights-java-agent.md)

## <a name="view"></a>Zkoumání metriky výkonu
V [portálu Azure](https://portal.azure.com), procházejte do zdroje Application Insights, které jste nastavili pro vaši aplikaci. V okně Přehled zobrazuje data výkonu základní:

Klikněte na libovolný graf zobrazíte další podrobnosti a zobrazit výsledky delší dobu. Například klikněte na dlaždici požadavků a pak vyberte časové rozmezí:

![Proklikejte se k více dat a vyberte časový rozsah](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Klikněte na graf vybrat metriky, které se zobrazí, nebo přidejte nový graf a vyberte jeho metriky:

![Klikněte na graf vybrat metriky](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Zrušte zaškrtnutí políčka všechny metriky** zobrazíte úplné výběru, která je k dispozici. Metriky lze rozdělit do skupin; Pokud je vybraná kteréhokoli člena skupiny, zobrazí se pouze ostatní členové této skupiny.

## <a name="metrics"></a>Jaké jsou všechny střední? Dlaždice výkonu a sestavy
Existují různé metriky výkonu, které můžete získat. Začněme s těmi, které se ve výchozím nastavení v okně aplikace.

### <a name="requests"></a>Požadavky
Počet požadavků HTTP přijaté v zadaném období. Výsledky porovnejte s výsledky v jiných sestavách zobrazíte chování vaší aplikace jako zatížení se liší.

Požadavky HTTP zahrnují všechny požadavky GET nebo POST pro stránky, data a bitové kopie.

Klikněte na dlaždici získat počty pro konkrétní adresy URL.

### <a name="average-response-time"></a>Průměrná doba odezvy
Měří čas mezi zadání vaší aplikace a odpovědi nevrátila webového požadavku.

Body zobrazit klouzavý průměr. Pokud je celá řada požadavků, mohou existovat některé, který odchylují od průměr bez zřejmé ve špičce nebo ponořit v grafu.

Podívejte se na neobvyklé vrcholů. Obecně platí očekávané doby odezvy roste s nárůstem požadavky. Pokud zvýšení nesoustředil příliš velký, může aplikace nedosáhli limitu prostředků, například CPU, kapacitu služby, kterou používá.

Klikněte na dlaždici získat časy pro konkrétní adresy URL.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Nejpomalejší požadavků
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Zobrazuje, které požadavky může být nutné optimalizace výkonu.

### <a name="failed-requests"></a>Neúspěšné požadavky
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Počet požadavků, které vyvolala nezachycená výjimek.

Klikněte na dlaždici pro podrobné informace o specifických chybách a vyberte individuální žádosti zobrazíte její podrobnosti. 

Pro jednotlivé kontroly se uchovávají pouze reprezentativní vzorek chyb.

### <a name="other-metrics"></a>Další metriky
Pokud chcete zjistit, co nastavit další metriky můžete zobrazit, klikněte na graf a poté zrušte všechny metriky zobrazíte kompletní k dispozici. Viz definice jednotlivé metriky kliknutím (i).

![Zrušte výběr všechny metriky zobrazíte celé sady](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Výběr všech metrika zakáže ostatní, které se nesmí objevit na stejném grafu.

## <a name="set-alerts"></a>Nastavení upozornění
Mají být informování tímto e-mailu neobvyklou hodnot všechny metriky, přidáte oznámení. Můžete buď k odeslání e-mailu pro účet správce, nebo na konkrétní e-mailové adresy.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Nastavte prostředků než ostatní vlastnosti. Nemáte zvolte zdroje, testu webu, pokud chcete nastavit výstrahy na metriky výkonu a využití.

Pečlivě si uvědomit a jednotky, ve kterých budete vyzváni k zadání prahovou hodnotu.

*Tlačítko Přidat oznámení se nezobrazí.* -Je tato skupina účet, ke které máte oprávnění jen pro čtení? Obraťte se na správce účtu.

## <a name="diagnosis"></a>Diagnostika problémů
Zde je několik tipů pro hledání a diagnostice problémů s výkonem.

* Nastavit [webové testy] [ availability] výstrahy, pokud váš web z umístění ocitne mimo provoz nebo reaguje pomalu nebo nesprávně. 
* Porovnejte počtu žádostí o s další metriky pro případ selhání nebo pomalé odezvy se vztahují k načtení.
* [Vložení a vyhledávací příkazy trasování] [ diagnostic] ve svém kódu pro pomoci přesně určit problémy.
* Monitorování webové aplikace v operaci s [živý datový proud metriky][livestream].
* Zaznamenat stav aplikace .net s [ladicí program snímku][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a>Najít a opravit kritická místa výkonu s šetření interaktivní výkon

Nové šetření interaktivní výkonu Application Insights můžete použít k vyhledání oblasti vaší webové aplikace, které jsou zpomalení celkový výkon. Můžete rychle najít konkrétní stránky, které jsou zpomalení a použít [profilování nástroj](app-insights-profiler.md) chcete zobrazit, pokud existuje korelace mezi tyto stránek.

### <a name="create-a-list-of-slow-performing-pages"></a>Vytvoří seznam pomalé provádění stránky 

Prvním krokem pro hledání problémy s výkonem je získat seznam pomalé odpovídá stránky. Následující kopie obrazovky ukazuje, jak získat seznam potenciální na stránkách prošetřily pomocí okno výkon. Rychle uvidíte z této stránky, že došlo zpomalování doby odezvy aplikace v přibližně 6:00 PM a znovu přibližně 22: 00. Můžete také zjistit, že zákazník nebo podrobnosti o operaci GET měl některé dlouhotrvající operace s dobou odezvy střední 507.05 milisekundách. 

![Interaktivní výkon statistiky aplikace](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Přejděte na konkrétní stránky

Jakmile máte snímek výkon vaší aplikace, můžete získat podrobnosti o konkrétních zpomalit provádění operací. Klikněte na všechny operace v seznamu a zobrazit podrobnosti, jak je uvedeno níže. Z grafu se zobrazí, pokud byl výkon podle závislost. Můžete také zjistit, kolik uživatelů došlo různé doby odezvy. 

![Okna operations Statistika aplikací](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Přejděte na určitém časovém období

Po zjištění bod v čase k prozkoumání, podrobnostem a i podívejte se na konkrétní operace, které mohly způsobit zpomalování výkonu. Po kliknutí na tlačítko k určitému bodu v čase získání podrobností o stránce jak je uvedeno níže. V příkladu níže můžete zobrazit činnosti uvedené pro dané časové období spolu s kódy odpovědí serveru a doba trvání operace. Máte také adresu url pro otevření pracovní položky sady TFS, pokud je potřeba odesílat tyto informace váš vývojový tým.

![Application Insights časovém intervalu](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Přejděte na konkrétní operaci

Po zjištění bod v čase k prozkoumání, podrobnostem a i podívejte se na konkrétní operace, které mohly způsobit zpomalování výkonu. Klikněte na operace ze seznamu a zobrazit podrobnosti operace, jak je uvedeno níže. V tomto příkladu uvidíte, že operace se nezdařila a Application Insights poskytl podrobnosti, které aplikace vyvolala výjimku. V tomto okně znovu, můžete snadno vytvořit pracovní položky sady TFS.

![Application Insights operaci okno](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Další kroky
[Webové testy] [ availability] -mít webové požadavky odeslané do vaší aplikace v pravidelných intervalech z po celém světě.

[Zaznamenání a hledání diagnostické trasování] [ diagnostic] – vložení trasovacího volání a analyzovat výsledky ke kotvícímu bodu problémy.

[Sledování využití] [ usage] – zjistěte, jak lidé používat vaši aplikaci.

[Řešení potíží s] [ qna] - a otázky A odpovědi



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



