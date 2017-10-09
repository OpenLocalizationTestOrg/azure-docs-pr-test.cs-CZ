---
title: aaaWeb funkce application performance monitoring - Azure Application Insights | Microsoft Docs
description: "Jak Application Insights zapadá do hello devOps cyklu"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: bba2d6c59de1794adcbf8e298d0ef4f0dbaa700f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Hloubková diagnostika webových aplikací a služeb pomocí Application Insights
## <a name="why-do-i-need-application-insights"></a>Proč musím Application Insights?
Application Insights monitoruje funkční webovou aplikaci. Informace o selhání a problémy s výkonem a pomáhá analyzovat, jak zákazníci používají vaši aplikaci. Ho funguje pro aplikace běžící na spoustě platforem (ASP.NET, J2EE, Node.js,...) a je hostovaná v hello cloudu nebo místně. 

![Aspekty hello složitosti doručování webové aplikace](./media/app-insights-devops/010.png)

Je nezbytné toomonitor moderních aplikací je spuštěna. Co je nejdůležitější – chcete selhání toodetect před většina vašich zákazníků. Také chcete toodiscover a opravte problémy s výkonem, které, když nejsou závažné, možná zpomalují počítač nebo způsobit některé komplikace tooyour uživatelé. A pokud hello systému provádí tooyour spokojenost, chcete tooknow jaké hello uživatelé dělají s ním: budou pomocí nejnovější funkce hello? Jsou s ním úspěšné jejich?

Moderní webové aplikace jsou vyvinuté v cyklus nastavené průběžné doručování: uvolnění nové funkce nebo zlepšování; Sledujte, jak dobře funguje pro uživatele hello; Naplánujte hello další přírůstek vývoj podle dané znalosti. Klíčovou součástí tohoto cyklu je hello pozorování fáze. Application Insights poskytuje toomonitor hello nástroje pro webovou aplikaci pro využití a výkonu.

Hello nejdůležitějších aspektů tento proces je diagnostiky a diagnostiku. V případě selhání aplikace hello je právě ztratil firmy. Primárním úkolem Hello monitorování prostředí je proto toodetect selhání spolehlivě, upozornění můžete okamžitě a toopresent, že budete s informacemi o hello toodiagnose hello problém. Toto je přesně co dělají Application Insights.

### <a name="where-do-bugs-come-from"></a>Odkud pocházejí chyby?
Selhání v systémech webové obvykle způsobit problémy s konfigurací nebo chybných interakce mezi jejich celá řada komponent. první úlohou Hello při řešení incidentu živý web je proto místo hello tooidentify hello problému: které komponenta nebo relace je hello příčina?

Některé z nás, ty s šedé kříž můžete mějte na paměti jednodušší letopočtu, ve kterém byl počítačový program spuštěn v jednom počítači. Vývojáři Hello by otestovat důkladně před přenosů. a nutnosti dodaný, by zřídka najdete v článku nebo myslíte o ho znovu. Uživatelé Hello by mít tooput s zbytkové chyby hello mnoho let. 

Co jsou nyní tak liší. Aplikace má na nadbytku toorun různých zařízení a může být obtížné tooguarantee hello přesný stejné chování na každé z nich. Hostování aplikace v cloudu hello znamená můžete rychle opraveno chyb, ale taky to znamená průběžné soutěže a hello očekávání nových funkcí v pravidelných intervalech. 

V těchto podmínkách hello pouze tookeep způsobem, který je pevně ovládací prvek v počtu chyb hello automatizované testování částí. Je možné toomanually znovu proveďte kompletní testování na každý doručení. Testování částí je běžná součástí hello proces sestavení. Nápověda k nástrojům například hello Xamarin Test Cloud tím, že poskytuje automatizované uživatelského rozhraní testování na více verzí prohlížeče. Tyto režimy testování nám umožňují toohope této hello počet chyb nalezena uvnitř aplikace je možné mít tooa minimální.

Typické webových aplikací mít celou řadu součástí za provozu. Přidání toohello klienta (v aplikaci prohlížeč nebo zařízení) a hello webový server je pravděpodobně toobe významné back-end zpracování. Možná hello back-end je kanál součástí nebo čím větší kolekci spolupráce částí. A kolika z nich nebudou v vlastního ovládacího prvku – jsou externích služeb, na kterých závisí.

V konfiguracích takovéto ho být obtížné a uneconomical tootest, nebo předvídáte, všechny možné selhání režimu, než v hello živé samotného systému. 

### <a name="questions-"></a>Dotazy...
Některé dotazy, můžeme požádat, když jsme se vývojem webového systému:

* Selhává Moje aplikace 
* Co přesně se stalo? – Pokud se nezdařilo žádost, chci tooknow jak získali existuje. Potřebujeme trasování událostí...
* Moje aplikace je dostatečně rychle? Jak dlouho trvá toorespond tootypical požadavky?
* Můžete načíst hello popisovač server hello Jakmile se hello počet žádostí o roste, doba odezvy hello stiskněte konstantní?
* Jak přizpůsobivý jsou moje závislosti - hello rozhraní REST API, databáze a další součásti, které volá mé aplikace. Konkrétní systém hello je pomalé, je můj součásti nebo dochází k pomalé odezvy od jiného uživatele?
* Moje aplikace je nahoru nebo dolů? Můžete ji zobrazit z kolem hello, world? Chci vědět, pokud přestane...
* Co je hlavní příčinou hello? Chyba hello Moje součásti nebo závislost? Je problém komunikace?
* Dopad na tom, kolik uživatelů? Pokud jsou tootackle více než jeden problém, který je hello nejdůležitější?

## <a name="what-is-application-insights"></a>Co je Application Insights?
![Základní pracovní postup Application Insights](./media/app-insights-devops/020.png)

1. Application Insights instruments vaší aplikace a odesílá telemetrii týkající se ho, když běží aplikace hello. Buď můžete vytvořit hello Application Insights SDK do aplikace hello, nebo můžete použít instrumentace za běhu. Metoda Hello je flexibilnější, jak můžete přidat vlastní telemetrii toohello regulární moduly.
2. telemetrie Hello se odesílají toohello portál Application Insights, kde je uložena a zpracována. (I když Application Insights je hostován v Microsoft Azure, může monitorovat všechny webové aplikace – aplikace právě Azure.)
3. telemetrie Hello se zobrazí tooyou ve formě hello grafů a tabulek událostí.

Existují dva hlavní typy telemetrických dat: agregovaná a raw instancí. 

* Instance data obsahuje například sestavu žádosti, která byla přijata ve vaší webové aplikace. Můžete najít pro a zkontrolujte podrobnosti hello požadavku pomocí nástroje vyhledávání hello hello portálu Application Insights. Hello instance by obsahovat data, jako je například jak dlouho trvalo aplikace toorespond toohello požadavku, jakož i hello požadovanou adresu URL, přibližnou polohu hello klienta a další data.
* Agregovaná data zahrnuje počet událostí za jednotku času, takže můžete porovnat hello míra požadavků s hello odezvy. Zahrnuje také průměry metrik, jako je například dobu odezvy požadavku.

Hello hlavních kategorií dat jsou:

* Požadavky tooyour aplikaci (obvykle požadavků protokolu HTTP), s daty na adrese URL, doby odezvy a úspěch nebo selhání.
* Závislosti - volání REST a SQL provedené vaší aplikace, také pomocí URI, doby odezvy a úspěch
* Výjimky, včetně trasování zásobníku.
* Stránka zobrazení data, která pochází z prohlížečů uživatelů hello.
* Metriky například čítače výkonu, jakož i metriky, které můžete psát sami. 
* Vlastní události, které můžete použít tootrack obchodní události
* Slouží k ladění protokolu trasování.

## <a name="case-study-real-madrid-fc"></a>Případová studie: Skutečné Madridu F.C.
Hello webovou službu [skutečné křížovou kartou Madridu fotbalové](http://www.realmadrid.com/) slouží o 450 milionů ventilátory kolem hello, world. Ventilátory přistupovat i prostřednictvím webových prohlížečů a hello křížovou kartou na mobilní aplikace. Ventilátory můžete nejen sešit lístků, ale také přístup k informace a video klipů na výsledky, přehrávače a nadcházející hry. Vyhledávání můžete s filtry, jako je počet cílů skóre pro magnitudu. Existují také odkazy toosocial média. činnost koncového uživatele Hello je vysoce přizpůsobené a slouží jako ventilátory tooengage obousměrné komunikace.

Hello řešení [je systém služeb a aplikací v Microsoft Azure](https://www.microsoft.com/en-us/enterprise/microsoftcloud/realmadrid.aspx). Škálovatelnost je klíčovým požadavkem: provoz je proměnná a mohou dosáhnout velké svazky, během a kolem odpovídá.

Skutečných Madrid, je výkon systému důležitých toomonitor hello. Azure Application Insights poskytuje komplexní pohled v rámci systému hello, zajištění spolehlivé a vysokou úroveň služeb. 

Hello křížovou kartou také získá podrobný přehled o jeho ventilátory: tam, kde jsou (pouze % 3 jsou v Španělsko), které vás zajímají mají v přehrávače, historické výsledky a nadcházející hry a jak se reagují toomatch výstupy.

Většina této telemetrická data jsou shromažďována automaticky žádné přidané kód, který zjednodušené hello řešení a snižuje provozní složitost.  Skutečných Madrid, Application Insights se zabývá body telemetrie 3.8 miliardy každý měsíc.

Skutečné Madridu používá hello Power BI modulu tooview jejich telemetrie.

![Power BI zobrazení telemetrie Application Insights](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Inteligentní detekce
[Proaktivní diagnostiky](app-insights-proactive-diagnostics.md) je nejnovější funkce. Bez jakékoli speciální konfigurace, které jste Application Insights automaticky zjišťuje a upozorní na neobvyklé přírůstky. selhání měr ve vaší aplikaci. Je dostatečně inteligentní tooignore pozadí příležitostné selhání, a také složitost, které jsou jednoduše přiměřené tooa zvýšení požadavky. Tak například pokud dojde k selhání v jednom z hello služby, které závisí na, nebo pokud hello nové sestavení jste právě nasadili nefunguje, tak i pak poznáte o tom, co nejrychleji se podíváte na e-mailu. (A k dispozici jsou webhooky můžete aktivovat jiné aplikace.)

Další aspekt této funkce provede denní podrobné analýzy vaší telemetrie, hledá neobvyklou vzory výkonu, které jsou pevné toodiscover. Například najdete nízký výkon, které jsou spojené s konkrétní zeměpisná oblast nebo s verzí určitého prohlížeče.

V obou případech hello výstraha nejen informuje hello příznaky se zjistí, ale také poskytuje datech potřebujete toohelp diagnostikovat hello problém, jako je například relevantní výjimka sestavy.

![E-mailu z proaktivní diagnostiky](./media/app-insights-devops/030.png)

Zákazník Samtec uvedená: "během posledních funkci cutover jsme našli databázi škálovat, který byl nedosáhli limitů jeho prostředků a způsobuje vypršení časových limitů. Výstrahy proaktivní zjišťování byla přijata oznámena jako inzerované jsme byly triaging hello problém velmi v reálném čase. Tato výstraha doplněná s výstrahami platformy Azure hello pomohl nám prakticky okamžitě hello problém opravte. Celkové prostoje < 10 minut."

## <a name="live-metrics-stream"></a>Živý datový proud metriky
Nasazení sestavení nejnovější hello může být usilujíce prostředí. Pokud se vyskytnou potíže, chcete tooknow o nich hned, takže můžete zálohovat v případě potřeby. Živý datový proud metriky vám dává klíčové metriky s latencí přibližně jednu sekundu.

![Metriky za provozu](./media/app-insights-devops/040.png)

A umožňuje okamžitě zkontrolujte ukázka chyby a výjimky.

![Události chyb za provozu](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Mapa aplikace
Mapa aplikace automaticky vyhledá topologie vaší aplikace, kterým se informace o výkonu hello nad jeho toolet snadno identifikovat kritická místa výkonu a problematické toků v distribuovaném prostředí. Je možné závislosti aplikací toodiscover na služby Azure. Můžete rychlou kontrolu hello problém vysvětlení, pokud je související s kódem nebo související závislosti a z jednom místě přejít k podrobnostem související diagnostiky prostředí. Aplikace může být například selhání kvůli snížení tooperformance ve vrstvě SQL. S aplikací mapy můžete vidět okamžitě a přejít k podrobnostem hello Poradce pro Index SQL nebo Přehled dotazu prostředí.

![Mapa aplikace](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Analýza statistiky aplikace
S [Analytics](app-insights-analytics.md), můžete napsat libovolný dotazy v jazyce, výkonné jako SQL.  Diagnostikování napříč zásobníku celá aplikace hello stane snadno spojit různých perspektiv a můžete požádat hello správné otázky toocorrelate výkon služby s obchodní metriky a zkušeností zákazníků. 

Všechny instance telemetrie a metriky nezpracovaná data uložená v portálu hello se můžete dotazovat. jazyk Hello zahrnuje filtru, spojení, agregace a další operace. Můžete vypočítat pole a provádět statistickou analýzu. Existují tabulkový a grafické vizualizace.

![Graf dotazu a výsledky analýzy](./media/app-insights-devops/025.png)

Například je snadno:

* Segment žádost o vaší aplikaci, že data výkonu zákazníkem úrovně toounderstand jejich prostředí.
* Vyhledat konkrétní chybové kódy a názvy vlastních událostí během šetření živý web.
* Rozbalit soubor využití aplikace hello konkrétní zákazníky toounderstand jak funkce jsou získat a přijmout.
* Sledování relací a dobu odezvy pro podporu tooenable konkrétní uživatele a operace týmy tooprovide rychlých zákaznickou podporu.
* Určení často používané aplikace funkce tooanswer funkce stanovení priorit otázky.

Uvedená zákazníka DNN: "Application Insights poskytl nám s hello chybí součást hello rovnice, který dokáže toocombine, řazení, dotazů a filtrovat data podle potřeby. Povolení naše toouse team vlastní vynalézavosti a prostředí toofind data s účinný dotazovací jazyk má povolené nám toofind přehledy a vyřešit problémy, které nebylo i víme, že jsme měli. Mnoho zajímavé odpovědi pocházet z otázek hello počínaje *' I wonder if...'.*"

## <a name="development-tools-integration"></a>Integrace nástroje pro vývoj
### <a name="configuring-application-insights"></a>Konfigurace služby Application Insights
Visual Studio a Eclipse mít nástroje tooconfigure hello správné SDK balíčků pro projekt hello, které vyvíjíte. Není k dispozici tooadd příkaz nabídky Application Insights.

Pokud se stát toobe pomocí rozhraní protokolování trasování například Log4N, NLog nebo System.Diagnostics.Trace, můžete získat hello možnost toosend hello protokoly tooApplication Statistika společně s hello další telemetrií tak, aby hello trasování mohou snadno korelovat s požadavky , volání závislostí a výjimky.

### <a name="search-telemetry-in-visual-studio"></a>Telemetrie vyhledávání v sadě Visual Studio
Při vývoji a ladění funkce, můžete zobrazit a hledání hello telemetrie přímo v sadě Visual Studio, pomocí hello stejné vyhledávání zařízení jako v hello webový portál.

A když Application Insights protokoluje výjimku, můžete zobrazit hello datového bodu v sadě Visual Studio a přejít přímých toohello odpovídající kód.

![Visual Studio vyhledávání](./media/app-insights-devops/060.png)

Během ladění, máte hello možnost tookeep hello telemetrie ve vývojovém počítači, jeho zobrazení v sadě Visual Studio, ale bez odeslání toohello portálu. Tato možnost místní zabraňuje kombinování ladění pomocí telemetrie produkční.

### <a name="build-annotations"></a>Sestavení poznámky
Pokud používáte Visual Studio Team Services toobuild a nasazení aplikace, nasazení poznámky zobrazí v grafech hello portálu. Nejnovější verze měli nijak neprojeví na hello metriky, je zřejmé.

![Sestavení poznámky](./media/app-insights-devops/070.png)

### <a name="work-items"></a>Pracovní položky
Pokud je vyvolána výstraha, Application Insights do systému pro sledování práce automaticky vytvořit pracovní položku.

## <a name="but-what-about"></a>Ale co o...?
* [Ochrana osobních údajů a úložiště](app-insights-data-retention-privacy.md) -telemetrie se ukládají na Azure zabezpečení serverů.
* Výkon - dopad hello je velmi nízké. Telemetrie je zpracovat v dávce.
* [Ceny](app-insights-pricing.md) – můžete začít používat zdarma a který pokračuje v nízkou svazku.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky
Začínáme s Application Insights je snadné. Hello hlavní možnosti jsou:

* Instrumentace webové aplikace už spuštěná. To vám dává všechny hello předdefinované výkonu telemetrie. Je k dispozici pro [Java](app-insights-java-live.md) a [servery služby IIS](app-insights-monitor-performance-live-website-now.md)a také pro [webové aplikace Azure](app-insights-azure.md).
* Instrumentace projektu během vývoje. Můžete to udělat pro [ASP.NET](app-insights-asp-net.md) nebo [Java](app-insights-java-get-started.md) aplikace, a také [Node.js](app-insights-nodejs.md) a hostitele z [jiné typy](app-insights-platforms.md). 
* Nástroj [žádné webové stránce](app-insights-javascript.md) přidáním fragmentu kódu krátké.

