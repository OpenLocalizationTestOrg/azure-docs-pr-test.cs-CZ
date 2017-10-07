---
title: "aaaMonitor stavu a využití pomocí služby Application Insights vaší aplikace"
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
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a>Sledování výkonu webových aplikací


Ujistěte se, že aplikace je výkon na dobré a rychle zjistěte informace o případných selhání. [Application Insights] [ start] bude vás informovat o žádné problémy s výkonem a výjimkami a pomáhají najít a diagnostikovat hello hlavní příčiny.

Application Insights můžete monitorovat webové aplikace Java a ASP.NET a služby, služby WCF. Mohou být hostovaná místně, virtuálními počítači, nebo jako weby Microsoft Azure. 

Na straně klienta hello Application Insights může trvat telemetrie z webových stránek a širokou škálu zařízení se systémy iOS, Android a aplikací pro Windows Store.

>[!Note]
> Provedli jsme nové prostředí pro vyhledání pomalé, provádění stránky ve vaší webové aplikaci k dispozici. Pokud nemáte přístup tooit, povolte ji tak, že konfigurace možností preview s hello [Preview okno](app-insights-previews.md). Přečtěte si informace o toto nové prostředí v [najít a opravit kritická místa výkonu s hello interaktivní výkon šetření](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Nastavení monitorování výkonu
Pokud jste ještě nepřidali Application Insights tooyour projektu (to znamená, pokud nemá soubor ApplicationInsights.config), vyberte jednu z těchto způsobů tooget spuštění:

* [Webové aplikace ASP.NET](app-insights-asp-net.md)
  * [Přidat monitorování výjimek](app-insights-asp-net-exceptions.md)
  * [Přidat monitorování závislostí](app-insights-monitor-performance-live-website-now.md)
* [Webových aplikací J2EE](app-insights-java-get-started.md)
  * [Přidat monitorování závislostí](app-insights-java-agent.md)

## <a name="view"></a>Zkoumání metriky výkonu
V [hello portál Azure](https://portal.azure.com), procházet toohello prostředek Application Insights, které jste nastavili pro vaši aplikaci. okno Přehled Hello ukazuje údaje o výkonu základní:

Klikněte na libovolný graf toosee podrobněji a toosee výsledky delší dobu. Například klikněte na dlaždici hello požadavků a pak vyberte časový interval:

![Klikněte na tlačítko prostřednictvím toomore dat a vyberte časový rozsah](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Klikněte na graf toochoose metriky, které se zobrazí, nebo přidejte nový graf a vyberte jeho metriky:

![Klikněte na tlačítko metriky toochoose grafu](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Zrušte zaškrtnutí políčka všechny metriky hello** hello toosee úplné se výběru, která je k dispozici. metriky Hello spadají do skupiny; Pokud je vybraná kteréhokoli člena skupiny, objeví jenom tehdy hello ostatní členové této skupiny.

## <a name="metrics"></a>Jaké jsou všechny střední? Dlaždice výkonu a sestavy
Existují různé metriky výkonu, které můžete získat. Začněme s těmi, které se ve výchozím nastavení v okně aplikace hello.

### <a name="requests"></a>Požadavky
Hello počet požadavků HTTP přijaté v zadaném období. Toto porovnání s výsledky hello na jiné sestavy toosee chování vaší aplikace jako zatížení hello liší.

Požadavky HTTP zahrnují všechny požadavky GET nebo POST pro stránky, data a bitové kopie.

Klikněte na hello dlaždice tooget počty pro konkrétní adresy URL.

### <a name="average-response-time"></a>Průměrná doba odezvy
Míry hello čas mezi zadáním vaše aplikace a hello odpověď nevrátila webového požadavku.

body Hello zobrazit klouzavý průměr. Pokud je celá řada požadavků, mohou existovat některé, který odchylují od hello průměr bez zřejmé ve špičce nebo ponořit v grafu hello.

Podívejte se na neobvyklé vrcholů. Obecně platí očekávejte toorise čas odpovědi s nárůstem požadavky. Pokud zvýšení hello nesoustředil příliš velký, může být aplikace nedosáhli limitu prostředků, jako je kapacita procesoru nebo hello služby, kterou používá.

Klikněte na tlačítko hello dlaždice tooget časy pro konkrétní adresy URL.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Nejpomalejší požadavků
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Zobrazuje, které požadavky může být nutné optimalizace výkonu.

### <a name="failed-requests"></a>Neúspěšné požadavky
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Počet požadavků, které vyvolala nezachycená výjimek.

Klikněte hello dlaždice toosee hello podrobnosti o konkrétní selhání a vyberte individuální žádosti toosee její podrobnosti. 

Pro jednotlivé kontroly se uchovávají pouze reprezentativní vzorek chyb.

### <a name="other-metrics"></a>Další metriky
toosee jaké další metriky, můžete zobrazit, klikněte na graf a potom zrušte výběr všech hello metriky toosee hello veškerou dostupnou sadu. Klikněte na tlačítko (i) toosee definice jednotlivé metriky.

![Zrušte výběr všech metriky toosee hello celé sady](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Výběr všechny metriky zakáže hello ostatní, které se nesmí objevit na stejném grafu hello.

## <a name="set-alerts"></a>Nastavení upozornění
toobe oznámení e-mailem neobvyklou hodnot všechny metriky přidat oznámení. Můžete buď toosend hello e-mailu toohello účet správce, nebo toospecific e-mailové adresy.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Nastavte další vlastnosti prostředku hello před hello. Hello testu webu prostředky nemáte zvolte, pokud chcete generovat výstrahy tooset na výkon nebo metriky využití.

Být opatrní toonote hello jednotky, ve kterých se dotaz tooenter hello prahovou hodnotu.

*Výstrahy tlačítko Přidat hello se nezobrazí.* -Jedná toowhich účet skupiny mají oprávnění jen pro čtení? Zeptejte se správce účtu hello.

## <a name="diagnosis"></a>Diagnostika problémů
Zde je několik tipů pro hledání a diagnostice problémů s výkonem.

* Nastavit [webové testy] [ availability] toobe zobrazí výstraha, pokud váš web z umístění ocitne mimo provoz nebo reaguje pomalu nebo nesprávně. 
* Porovnání počtu žádostí o hello s další metriky toosee, pokud jsou související tooload selhání nebo pomalé odezvy.
* [Vložení a vyhledávací příkazy trasování] [ diagnostic] v váš kód toohelp přesné problémy.
* Monitorování webové aplikace v operaci s [živý datový proud metriky][livestream].
* Zaznamenat stav hello aplikace .net s [ladicí program snímku][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a>Najít a opravit kritická místa výkonu s šetření interaktivní výkon

Oblasti lze použít hello nové Application Insights interaktivní výkon šetření toolocate vaší webové aplikace, které jsou zpomalení celkový výkon. Můžete rychle najít konkrétní stránky, které jsou zpomaluje a použijte hello [profilování nástroj](app-insights-profiler.md) toosee, pokud existuje korelace mezi tyto stránek.

### <a name="create-a-list-of-slow-performing-pages"></a>Vytvoří seznam pomalé provádění stránky 

prvním krokem Hello naleznou problémy s výkonem je tooget seznam hello pomalé zpracování stránky. úvodní obrazovka snímek níže ukazuje, jak pomocí hello výkonu okno tooget seznam další potenciální tooinvestigate stránky. Rychle uvidíte z této stránky, že došlo zpomalování doby odezvy hello aplikace hello v přibližně 6:00 PM a znovu přibližně 22: 00. Můžete také zjistit, že operace zákazníka nebo podrobnosti o získání hello měl některé dlouho běžící operace s střední odezvu 507.05 milisekund. 

![Interaktivní výkon statistiky aplikace](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Přejděte na konkrétní stránky

Jakmile máte snímek výkon vaší aplikace, můžete získat podrobnosti o konkrétních zpomalit provádění operací. Klikněte na všechny operace v hello seznamu toosee hello podrobnosti jak je uvedeno níže. Z hello grafu se zobrazí, pokud byl hello výkonu podle závislost. Uvidíte také kolik uživatelů zkušeného hello různé doby odezvy. 

![Okna operations Statistika aplikací](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Přejděte na určitém časovém období

Poté, co jste našli bod v časové tooinvestigate, k podrobnostem i další toolook v hello konkrétních operací, které mohly způsobit zpomalování výkonu hello. Po kliknutí na tlačítko k určitému bodu v čase získáte podrobnosti o hello hello stránky, jak je uvedeno níže. V hello najdete příklad níže uvedené pro dané časové období spolu s kódy odpovědí serveru hello a doba trvání operace hello operations hello. Adresa url hello máte také můžete otevřít pracovní položky sady TFS, pokud potřebujete toosend tato informace tooyour vývojový tým.

![Application Insights časovém intervalu](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Přejděte na konkrétní operaci

Poté, co jste našli bod v časové tooinvestigate, k podrobnostem i další toolook v hello konkrétních operací, které mohly způsobit zpomalování výkonu hello. Klikněte na operace z hello seznamu toosee hello podrobnosti hello operace, jak je uvedeno níže. V tomto příkladu, které se zobrazí hello operace se nezdařila, a Application Insights poskytl hello podrobnosti o hello aplikace hello výjimka vyvolala. V tomto okně znovu, můžete snadno vytvořit pracovní položky sady TFS.

![Application Insights operaci okno](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Další kroky
[Webové testy] [ availability] -webových požadavků odeslali tooyour aplikace v pravidelných intervalech z kolem hello, world.

[Zaznamenání a hledání diagnostické trasování] [ diagnostic] – vložení trasovacího volání a analyzovat problémy toopinpoint výsledky hello.

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



