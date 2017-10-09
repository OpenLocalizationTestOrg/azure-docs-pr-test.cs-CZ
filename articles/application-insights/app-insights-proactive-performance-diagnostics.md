---
title: "aaaSmart detekce - anomálie výkonu | Microsoft Docs"
description: "Application Insights provede inteligentní analýzu aplikace telemetrie a zobrazí upozornění na potenciální problémy. Tato funkce vyžaduje žádné nastavení."
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: bwren
ms.openlocfilehash: 60f10612188920330030129f7464e2f398ef1f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---performance-anomalies"></a>Inteligentní detekce - anomálie výkonu

[Application Insights](app-insights-overview.md) automaticky analyzuje hello výkonu webové aplikace a může vás upozorní na potenciální problémy. Vám může být čtení to vzhledem k tomu, že jste dostali jeden z našich Inteligentní detekce oznámení.

Tato funkce vyžaduje žádné speciální nastavení, než konfigurace aplikace pro službu Application Insights (na [ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), nebo [Node.js](app-insights-nodejs.md)a v [kódu webové stránky](app-insights-javascript.md)). Bude aktivní, pokud vaše aplikace generuje dost telemetrických dat.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Při obdržení oznámení Inteligentní detekce

Application Insights zjistil, že má ke snížení výkonu hello vaší aplikace v jednom z těchto způsobů:

* **Snížení času odezvy** -aplikace bylo zahájeno neodpovídá toorequests pomalejší, než kterou používal na. Změna Hello byla pravděpodobně rychlé, například kvůli Regrese v nejnovější nasazení. Nebo mohlo být postupné, může být způsobeno nevrácenou pamětí. 
* **Snížení doby trvání závislosti** -aplikace provede volání tooa REST API, databáze nebo jinou závislost. závislost Hello je pomalejší, než kterou používal na reagovat.
* **Vzor nízký výkon** – aplikace se zobrazí toohave vystavuje výkonu, které ovlivňuje jenom některé požadavky. Například stránek načítání pomaleji na jeden typ prohlížeče než jiné; nebo požadavků, jsou obsluhovány pomaleji z jednoho konkrétního serveru. V současné době naše algoritmy podívejte se na časů načtení stránky, doby odezvy požadavku a časy odezvy závislostí.  

Inteligentní detekce vyžaduje minimálně 8 dní telemetrii na svazek použitelnou v pořadí tooestablish základní úroveň výkonu normální. Ano po spuštění vaší aplikace pro toto období, žádné významné problém výsledkem bude oznámení.


## <a name="does-my-app-definitely-have-a-problem"></a>Aplikace my máme věci má problém?

Ne, oznámení neznamená, že vaše aplikace výborný došlo k problému. Je jednoduše návrhu o něco, co můžete chtít toolook v přesněji.

## <a name="how-do-i-fix-it"></a>Jakým způsobem ji lze upravit?

Hello oznámení zahrnují diagnostické informace. Tady je příklad:


![Tady je příklad zjišťování snížení času odezvy serveru](./media/app-insights-proactive-diagnostics/server_response_time_degradation.png)

1. **Třídění**. Hello oznámení se zobrazuje počet uživatelů, kteří nebo jsou ovlivněny kolik operace. Můžete přiřadit problém toohello s prioritou.
2. **Obor**. Hello problém ovlivňuje veškerý provoz, nebo jen některé stránky? Je omezený it tooparticular prohlížeče nebo umístění? Tyto informace můžete získat z hello oznámení.
3. **Diagnostika**. Často hello diagnostické informace v oznámení hello navrhne hello povaze problému hello. Například pokud doba odezvy zpomalí, když je vysoká rychlost požadavků, který naznačuje, že jsou přetížené serveru nebo v závislosti. 

    Jinak otevřete okno výkon hello ve službě Application Insights. Zde najdete [profileru](app-insights-profiler.md) data. Pokud jsou výjimky vyvolány, můžete také zkusit hello [ladicí program snímku](app-insights-snapshot-debugger.md).



## <a name="configure-email-notifications"></a>Konfigurace e-mailových oznámení

Inteligentní detekce oznámení jsou ve výchozím nastavení povolené a odeslat toothose, kteří mají [vlastníci, přispěvatelé a čtenáři přístup k prostředku Application Insights toohello](app-insights-resources-roles-access-control.md). toochange to, klikněte na tlačítko **konfigurace** hello e-mailových oznámení nebo otevřete nastavení inteligentního vyhledávání ve službě Application Insights. 
  
  ![Nastavení inteligentního detekce](./media/app-insights-proactive-diagnostics/smart_detection_configuration.png)
  
  * Můžete použít hello **odhlášení** odkaz v e-mailu toostop Inteligentní detekce pro hello hello e-mailových oznámení.

E-mailů o inteligentní detekce anomálií výkonu jsou omezené tooone e-mailu za den za prostředek Application Insights. Hello email bude odeslán, pouze v případě, že existuje alespoň jeden nový problém, která byla zjištěna v daný den. Nezískáte opakuje žádné zprávy. 

## <a name="faq"></a>Nejčastější dotazy

* *Ano se možnost nepřetržitého podíváte na svá data?*
  * Ne. Služba Hello je zcela automatické. Můžete získat pouze hello oznámení. Vaše data jsou [privátní](app-insights-data-retention-privacy.md).
* *Analýza všechny hello data shromážděná pomocí Application Insights?*
  * Není v současné době. V současné době budeme analyzovat požadavku, že doba načítání doba odezvy, závislosti doba odezvy a stránky. Analýza další metriky je na našem nevyřízených položek Těšíme se.

* Jaké typy aplikací to funguje pro?
  * Tyto degradations zjištění v jakékoli aplikace, která generuje hello odpovídající telemetrie. Pokud jste nenainstalovali Application Insights ve vaší webové aplikaci, pak požadavky a závislosti automaticky sledovány. Ale v back-end služby nebo jiné aplikace, pokud jste vložili volání příliš[TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) nebo [TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency), pak Inteligentní detekce bude fungovat v hello stejný způsobem.

* *Můžete vytvořit vlastní anomálií pravidla detekce nebo přizpůsobit existující pravidla?*

  * Ještě nebyla ale můžete:
    * [Nastavení výstrah](app-insights-alerts.md) který říct, kdy metriky protne prahovou hodnotu.
    * [Export telemetrie](app-insights-export-telemetry.md) tooa [databáze](app-insights-code-sample-export-sql-stream-analytics.md) nebo [tooPowerBI](app-insights-export-power-bi.md), kde můžete analyzovat sami.
* *Jak často se provádí analýzu hello?*

  * Jsme hello analysis každodenní spouštění na hello telemetrie z hello předchozí den (plný den v časovém pásmu UTC).
* *Proto nemá tato nahradit [metriky výstrahy](app-insights-alerts.md)?*
  * Ne.  Toodetecting jsme nemáte potvrdit každých chování, které můžete zvážit neobvyklé.


* *Pokud dělat nemáte nic v oznámení tooa odezvy, bude získat připomenutí?*
  * Ne, dostanete zprávu o každém problému pouze jednou. Pokud hello potíže potrvají, bude aktualizována hello Inteligentní detekce kanálu okno.
* *Ztrátou hello e-mailu. Kde najdu hello oznámení portálu hello?*
  * V přehledu Application Insights hello vaší aplikace, klikněte na tlačítko hello **Inteligentní detekce** dlaždici. Existuje budete moct toofind všechna oznámení až too90 dnů zpět.

## <a name="how-can-i-improve-performance"></a>Jak může zlepšit výkon?
Pomalé a k selhání odpovědi jsou jeden z největších frustrations hello pro uživatele, webu, protože víte z vlastního prostředí. Ano je důležité, tooaddress hello problémy.

### <a name="triage"></a>Třídění
Nejprve záleží? Pokud na stránce je vždy pomalé tooload, ale pouze 1 % uživatelé vaší lokality třeba toolook na to, možná máte více důležité věci toothink o. Na hello druhé straně, pokud pouze 1 % uživatelé otevřít, ale vyvolá výjimek pokaždé, když, který může být vhodné příčin.

Příkaz dopad hello (ovlivnění uživatelé nebo % z provozu), použijte jako vodítko Obecné, ale uvědomte si, že to není celého textu hello. Shromažďování dalších tooconfirm důkaz.

Vezměte v úvahu parametry hello hello problému. Pokud je závislé na geography, nastavit [testy dostupnosti](app-insights-monitor-web-app-availability.md) včetně danou oblast: existuje může být jednoduše problémů se sítí v této oblasti.

### <a name="diagnose-slow-page-loads"></a>Diagnostika pomalé načítání stránky
Kde je hello problém? Je pomalé toorespond hello serveru, je velmi náročná hello stránka nebo hello prohlížeče neobsahuje toodo velké množství pracovních toodisplay ho?

Otevřete okno metriky hello prohlížeče. Hello segmentované zobrazení prohlížeče stránku zatížení čas ukazuje kam hello čas. 

* Pokud **odeslat doba požadavku** je vysoká, buď hello server bude reagovat pomalu nebo je hello požadavek post s velkým množstvím dat. Podívejte se na hello [metriky výkonu](app-insights-web-monitor-performance.md#metrics) tooinvestigate odezvy.
* Nastavit [závislost sledování](app-insights-asp-net-dependencies.md) toosee jestli pomalost hello je z důvodu tooexternal služby nebo databáze.
* Pokud **přijetí odpovědi** převládá, jsou dlouhé stránku a jeho závislé součásti.-JavaScript, CSS, obrázků a tak dále (ale asynchronně načtená data). Nastavení [test dostupnosti](app-insights-monitor-web-app-availability.md), a že tooset hello možnost tooload závislé součásti. Pokud některé výsledky získáte, otevřete hello podrobností výsledků a rozbalte ho toosee hello načíst časy různých souborů.
* Vysoká **doba zpracování klienta** navrhuje skripty běží pomalu. Pokud není důvod hello zřejmé, zvažte přidání nějaký kód časování a odeslat hello časy v trackMetric volání.

### <a name="improve-slow-pages"></a>Zlepšení pomalé stránky
Je úplná poradenství zlepšení odpovědí serveru a časů načtení stránky, takže jsme nebude zkoušet toorepeat webové jej všechny sem. Zde jsou několik tipy, které pravděpodobně již víte o jenom tooget jste myslím:

* Pomalé načítání kvůli velkých souborů: načtení hello skripty a dalšími částmi asynchronně. Použijte sdružování skriptů. Hlavní stránka hello rozdělte pomůcky, které samostatně načíst svá data. Neodesílat prostý staré HTML pro dlouho tabulky: použít skript toorequest hello data jako JSON nebo jiných sbaleném formátu a potom zadejte hello tabulku v místě. Existují toohelp skvělé rozhraní s to vše. (Také má za následek big skripty samozřejmě.)
* Pomalé serveru závislosti: Zvažte hello zeměpisné umístění komponent. Například pokud používáte Azure, ujistěte se, hello webový server a databáze hello jsou v hello stejné oblasti. Dotazy načítají více informací, než potřebují? By ukládání do mezipaměti nebo dávkování pomoc?
* Problémy s kapacity: Podívejte se na metriky server hello doby odezvy a počet požadavku. Pokud odezvy ve špičkách nepřiměřeně s vrcholů v žádosti, je pravděpodobné, že jsou vaše servery roztažená.


## <a name="server-response-time-degradation"></a>Snížení času odezvy serveru

oznámení snížení času odezvy Hello poskytuje:

* Doba odezvy Hello porovná toonormal doba odezvy pro tuto operaci.
* Počet uživatelů, kteří jsou vliv.
* Průměrná doba odezvy a 90 percentilu doba odezvy pro tuto operaci na den hello hello detekce a 7 dní před. 
* Počet tuto operaci požadavků na den hello hello detekce a 7 dní před.
* Korelace mezi snížení v této operaci a degradations v související závislosti. 
* Odkazy toohelp diagnostikovat problém hello.
  * Profileru trasování toohelp můžete zobrazit, kde je operace čas strávený (hello odkaz není k dispozici, pokud profileru trasování příklady, které byly shromážděny pro tuto operaci během období detekce hello). 
  * Sestavy pro zvýšení výkonu v Průzkumníku metrika, kde můžete vyfiltrování a rozčlenění čas nebo filtry rozsahu pro tuto operaci.
  * Vyhledávání pro toto volání tooview konkrétní volání vlastnosti.
  * Selhání sestavy - li počet > 1 to znamenat, že došlo k chybám při této operaci, která může přispět tooperformance snížení.

## <a name="dependency-duration-degradation"></a>Snížení doby trvání závislosti

Moderní aplikace více přijmout malých služby návrhu přístup, což v mnoha případech vede spolehlivost tooheavy na externích služeb. Například pokud vaše aplikace spoléhá na platformu některá data, nebo i v případě, že vytvoříte vlastní robota služba bude pravděpodobně předávání na některé tooenable kognitivní služby poskytovatele služby pro robota toopull hello uložit vaše robotů toointeract více lidského způsoby a některá data odpovědi z.  

Příklad závislostí snížení oznámení:

![Tady je příklad zjišťování snížení doby trvání závislosti](./media/app-insights-proactive-diagnostics/dependency_duration_degradation.png)

Všimněte si, že se dozvíte:

* Doba trvání Hello porovnání toonormal doba odezvy pro tuto operaci
* Počet uživatelů, kteří se vztahuje.
* Průměrná doba trvání a 90 percentilu trvání pro tuto závislost na den hello hello detekce a 7 dní před
* Počet závislostí volání na den hello hello detekce a 7 dní před
* Odkazy toohelp diagnostikovat problém hello
  * Sestavy pro zvýšení výkonu v Průzkumníku Metrika pro tuto závislost
  * Hledání pro tuto závislost volá tooview vlastnosti volání
  * Selhání sestavy – Pokud počet > 1, znamená to, že byly neúspěšné závislostí volá během hello detekce dobu, po kterou může přispět tooduration snížení. 
  * Otevřete Analytics s dotazy, které vypočítat této doby trvání závislosti a počet  

## <a name="smart-detection-of-slow-performing-patterns"></a>Inteligentní detekce pomalé provádění schémat 

Application Insights vyhledá problémy s výkonem, které mohou ovlivňují jenom některé část vaši uživatelé, nebo které ovlivňují jenom uživatelé v některých případech. Například oznámení o zatížení stránky je slowler na jeden typ prohlížeče než na jiné typy prohlížečů, nebo pokud z určitého serveru pomaleji zpracovat požadavky. Problémy spojené s kombinací vlastností, může také vyhledat, jako je pomalé stránka načte v jedné zeměpisné oblasti pro klienty, kteří používají konkrétní operační systém.  

Anomálie takovéto jsou velmi obtížné toodetect právě zkontrolováním hello data, ale jsou častější, než si myslíte. Často se pouze upozornit, když stížnost vašich zákazníků. Do té doby, je příliš pozdní: hello vliv uživatelé jsou již přepínání konkurenci tooyour!

V současné době naše algoritmy podívejte se na časů načtení stránky, dobu odezvy požadavku na hello server a časy odezvy závislostí.  

Nemáte tooset všechny prahové hodnoty nebo konfigurovat pravidla. Machine learning a algoritmy dolování dat jsou použité toodetect neobvyklé vzory.

![Z hello e-mailové výstrahy klikněte na tlačítko hello odkaz tooopen hello diagnostickou zprávu v Azure](./media/app-insights-proactive-performance-diagnostics/03.png)

* **Když** ukazuje byl zjištěn problém hello čas hello.
* **Co** popisuje:

  * Hello problém, který byl zjištěn;
  * Hello charakteristika hello sada události, které jsme našli zobrazí hello problém chování.
* Hello tabulka porovnává hello nedostatečně výkonná sada hello průměrná chování všechny další události.

Klikněte na příslušné sestavy, filtrováno hello čas a vlastnosti hello pomalé, provádění sady hello odkazy tooopen metrika Průzkumníka a vyhledávání.

Upravte hello časové rozmezí a filtry tooexplore hello telemetrie.

## <a name="next-steps"></a>Další kroky
Tyto diagnostické nástroje můžete zkontrolovat hello telemetrie z vaší aplikace:

* [Profiler](app-insights-profiler.md) 
* [Ladicí program snímku](app-insights-snapshot-debugger.md)
* [Analýzy](app-insights-analytics-tour.md)
* [Analýza inteligentní diagnostiky](app-insights-analytics-diagnostics.md)

Inteligentní detekce jsou zcela automatické. Ale možná byste chtěli tooset si některé další výstrahy?

* [Ručně konfigurované metriky výstrahy](app-insights-alerts.md)
* [Testy dostupnosti webu](app-insights-monitor-web-app-availability.md)
