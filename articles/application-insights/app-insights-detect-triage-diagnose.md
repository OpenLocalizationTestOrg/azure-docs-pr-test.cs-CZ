---
title: aaaOverview Azure Application Insights pro DevOps | Microsoft Docs
description: "Zjistěte, jak toouse Application Insights v prostředí Ops vývojářů."
author: CFreemanwa
services: application-insights
documentationcenter: 
manager: carmonm
ms.assetid: 6ccab5d4-34c4-4303-9d3b-a0f1b11e6651
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: bwren
ms.openlocfilehash: 42139f4645e815f26378726f4716a9bfbdc78551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-application-insights-for-devops"></a>Přehled Application Insights pro DevOps

S [Application Insights](app-insights-overview.md), můžete rychle zjistit, jaký je výkon aplikace a právě používá, když je za provozu. Pokud dojde k potížím, umožňuje vědět o jeho, pomůže posoudit dopad hello a pomáhají zjistit příčinu hello.

Zde je účet od týmu, který sama vyvinula webových aplikací:

* *"Během několika dní před jsme nasadili, vedlejší, opravy hotfix. Jsme nespustily průchodu testu široký, ale bohužel získali některé nečekaným změnám sloučeny do datové části hello způsobuje nekompatibilitu mezi hello front-end a back-EndY. Hned aktivováno naše výstrahy výjimek serveru surged a jsme byly provedeny vědět hello situaci. Pár kliknutí rychle na portálu služby Application Insights hello My dostatek informací z výjimka callstacks toonarrow dolů hello problém. Jsme vrácena okamžitě a omezené hello škody. Application Insights udělal tuto část hello devops cyklus velmi snadno a níž lze provést akci."*

V tomto článku jsme postupujte podle týmu v bance společnosti Fabrikam, která sama vyvinula hello online bankovnictví toosee systému (OBS), jak použít Application Insights tooquickly odpovídají toocustomers a provádět aktualizace.  

Hello týmu funguje v cyklu DevOps znázorňuje následující obrázek hello:

![Cyklus DevOps](./media/app-insights-detect-triage-diagnose/00-devcycle.png)

Požadavky na informačního kanálu do jejich vývoj nevyřízených položek (seznam úkolů). Tyto funkce fungují v krátkém sprintů, které často poskytovat software pracovní – obvykle ve formuláři hello vylepšení a rozšíření toohello existující aplikace. pomocí nové funkce je často aktualizována aplikace Hello za provozu. I když je za provozu, hello team se monitoruje výkonu a využití hello pomoci Application Insights. Tato data APM kanály zpátky na jejich vývoj nevyřízených položek.

Hello používá Application Insights toomonitor hello živou webovou aplikaci úzce pro:

* Výkon. Chtějí toounderstand jak odezvy lišit počtu žádostí o; kolik procesoru, sítě, disk a další prostředky jsou používány; a kde jsou hello kritická místa.
* Selhání. Pokud jsou výjimky nebo neúspěšné požadavky, nebo pokud se čítač výkonu ocitne mimo rozsah jeho možnost, hello team potřebám tooknow rychle, aby přijaly akce.
* Využití. Při každém vydání nové funkce, hello team má rozsah toowhat tooknow, používá se a toho, jestli uživatelé mají všechny problémy s ním.

Umožňuje zaměřit se na část zpětnou vazbu hello hello cyklus:

![Rozpoznat, třídění, diagnostikovat](./media/app-insights-detect-triage-diagnose/01-pipe1.png)

## <a name="detect-poor-availability"></a>Zjištění nízký dostupnosti
Marcela Markova je vývojář senior hello OBS týmu a trvá hello realizace na sledování výkonu online. Jana nastaví několik [testy dostupnosti](app-insights-monitor-web-app-availability.md):

* Testu jedné adresy URL pro hello hlavní cílová stránka aplikace hello http://fabrikambank.com/onlinebanking/. Jana nastaví kritéria kód HTTP 200 a text "Vítejte!". Pokud tento test ale selže, je něco vážně špatného hello síť hello servery nebo může být problém s nasazení. (Nebo uživatel změnil hello Vítejte! zpráva na stránce hello bez možnosti čtení jeho Přehled.)
* Hlubší vícekrokového testu, který se přihlásí a získá aktuálního účtu výpis, kontrola pár klíčů podrobnosti na každé stránce. Tento test ověřuje, že účty databázi hello odkaz toohello pracuje. Použije Jana id fiktivní zákazníka: několik z nich jsou zachována pro účely testování.

Tyto testy nastavit je Marcela jisti, že tento tým hello rychle věděli o všech výpadku.  

Selhání zobrazují jako červené tečky v hello webového testu grafu:

![Zobrazení webové testy, které mají běžet po hello předcházející období](./media/app-insights-detect-triage-diagnose/04-webtests.png)

Ale ještě důležitější, výstrahu o jakákoli chyba e-mailu toohello vývojový tým. Tento způsob se znát o něm téměř všechny hello zákazníků.

## <a name="monitor-performance"></a>Monitorování výkonu
Na stránce Přehled hello ve službě Application Insights, je graf, který obsahuje celou řadu [klíčové metriky](app-insights-web-monitor-performance.md).

![Různé metriky](./media/app-insights-detect-triage-diagnose/05-perfMetrics.png)

Čas načítání stránky prohlížeče je odvozená od telemetrické zprávy odesílané přímo z webové stránky. Doba odezvy serveru, počtu žádostí o serveru a počet neúspěšných požadavků jsou všechny měřená v hello webový server a odeslat tooApplication statistiky z ní.

Marcela je mírně nevadí hello graf odpovědi serveru. Tento graf znázorňuje průměrný čas hello mezi když hello server obdrží požadavek HTTP z prohlížeče uživatele a když se vrátí odpověď hello. Není neobvyklou toosee varianta v tomto grafu, protože se zatížení systému hello liší. Ale v takovém případě vypadá to, že toobe korelace mezi malé přírůstky. hello počet požadavků a big roste hello doby odezvy. Který by to znamenat, že hello systému pracuje pouze při jeho omezení.

Jana otevře hello servery grafy:

![Různé metriky](./media/app-insights-detect-triage-diagnose/06.png)

Vypadá to, toobe žádné přihlašovací omezení prostředků existuje, takže možná hello hrboly v grafech odpovědi serveru hello právě shoda.

## <a name="set-alerts-toomeet-goals"></a>Nastavit výstrahy toomeet cíle
Uživatel nicméně přeje, aby tookeep přehled na dobu odezvy hello. Pokud přejde příliš vysoké, tooknow o něm Jana chce okamžitě.

Aby Jana nastaví [výstraha](app-insights-metrics-explorer.md), pro větší než typické prahová hodnota doby odezvy. Díky tomu svůj spolehlivosti, které Jana budete vědět o něm, pokud jsou pomalé odezvy.

![Výstrahy okně Přidat](./media/app-insights-detect-triage-diagnose/07-alerts.png)

Výstrahy můžete nastavit na širokou škálu jiné metriky. Například může přijímat e-mailů, pokud počet výjimka hello se změní na vysokou nebo přejde nízkou hello dostupné paměti, nebo pokud je v klientských požadavků ve špičce.

## <a name="stay-informed-with-smart-detection-alerts"></a>Udržení informovanosti s výstrahami Inteligentní detekce
Další den výstrahy e-mailu přicházejí z Application Insights. Ale když uživatel otevře, Jana zjistí, že není hello odpovědi čas výstraha, která uživatel nastavit. Místo toho se sdělením, že došlo nečekané zvýšení neúspěšných požadavků – to znamená, požadavků, které vráceno selhání kódy 500 nebo víc.

Neúspěšné požadavky jsou, odkud mají uživatelé vidět chybu – obvykle následující výjimka vyvolána v kódu hello. Možná zobrazí zpráva s oznámením "Bohužel jsme nelze nyní aktualizovat podrobností o." Nebo na absolutní to nepříjemné nejhorší, výpisu zásobníku se zobrazí na obrazovce hello uživatele s laskavým svolením hello webový server.

Tato výstraha je neočekávaném, protože hello čas posledního Jana zvážení, hello chybných požadavků, že byl encouragingly nízkou počet. Malý počet selhání je toobe v zaneprázdněný server očekává.

Bylo rovněž kousek neočekávaném pro ní protože Jana neměly tooconfigure této výstrahy. Application Insights zahrnují inteligentní zjišťování. Automaticky upraví obvyklé selhání vzor tooyour aplikace a selhání "získá se používá pro" na konkrétní stránky, nebo pod vysokého zatížení nebo propojené tooother metriky. Vyvolá výstrahy hello pouze v případě, že je zvýšení výše co IT oddělení dodává tooexpect.

![e-mailu proaktivní diagnostiky](./media/app-insights-detect-triage-diagnose/21.png)

To je velmi užitečná e-mailu. Právě se nepodporuje vyvolat alarm. Příliš mnoho hello třídění a diagnostiky pracovní dělá.

Zobrazuje jak mnoho zákazníků pocítí důsledky a které webové stránky nebo operace. Marcela můžete rozhodnout, jestli Jana potřebuje tooget hello celý tým práce na tomto jako protipožární cvičení, nebo jestli se můžete ignorovat až do příštího týdne.

e-mailu Hello také ukazuje konkrétní výjimce došlo, a - i více zajímavé - tohoto selhání hello je přidružen konkrétní databázi tooa volání se nezdařilo. Tato část popisuje, proč hello selhání najednou zobrazovaly i v případě, že všechny aktualizace nebyla nasazena nedávno Marcela na týmu.

Marcella příkazy ping hello vedoucí týmu hello databáze založené na tento e-mail. Jana zjišťuje, zda se vydaná oprava v hello posledních půl hodiny; a bohužel se možná pravděpodobně změnu menší schématu...

Proto je hello problém na toobeing způsob hello odstraněna, i před příčin protokoly a do 15 minut od jeho použití. Ale Marcela klikne na odkaz tooopen hello Application Insights. Otevře se přímo na chybné žádosti a tak může vidět databázi se nezdařilo volání v hello přidružené seznam závislostí volání.

![neúspěšných žádostí.](./media/app-insights-detect-triage-diagnose/23.png)

## <a name="detect-exceptions"></a>Zjištění výjimek
S chvilku instalačního programu [výjimky](app-insights-asp-net-exceptions.md) jsou hlášené tooApplication Insights automaticky. Se také dají zachytit explicitně vložením volání příliš[TrackException()](app-insights-api-custom-events-metrics.md#trackexception) do hello kódu:  

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }


tým Fabrikam Bank Hello vyvinul hello postup vždy odesílat telemetrii na výjimku, pokud je zřejmé obnovení.  

Ve skutečnosti je i širší než jejich strategie: odesílání telemetrických dat ve všech případech, kde je frustrovaní v co zákazník hello se chtěli toodo, jestli odpovídá tooan výjimka v hello kódu, nebo ne. Například pokud hello externí přenos mezi bank systém vrátí zprávu "nelze dokončit tuto transakci" nějakého důvodu provozní (nezávisle na hello zákazníka) pak budou sledovat tuto událost.

    var successCode = AttemptTransfer(transferAmount, ...);
    if (successCode < 0)
    {
       var properties = new Dictionary <string, string>
            {{ "Code", returnCode, ... }};
       var measurements = new Dictionary <string, double>
         {{"Value", transferAmount}};
       telemetry.TrackEvent("transfer failed", properties, measurements);
    }

TrackException je použité tooreport výjimky, protože odešle kopii hello zásobníku. TrackEvent je použité tooreport další události. Všechny vlastnosti, které mohou být užitečné při diagnostiku můžete připojit.

Výjimky a události zobrazí v hello [diagnostické vyhledávání](app-insights-diagnostic-search.md) okno. Můžete přejít k podrobnostem je toosee hello další vlastnosti a trasováním zásobníku.

![Ve vyhledávání diagnostiky použijte filtry tooshow konkrétní typy dat](./media/app-insights-detect-triage-diagnose/appinsights-333facets.png)


## <a name="monitor-proactively"></a>Proaktivní monitorování
Marcela není právě nacházejí kolem čekání výstrahy. Krátce po každé nové nasazení, která se podíváme na [odezvy](app-insights-web-monitor-performance.md) – obě hello celkový obrázek a počty hello tabulky nejpomalejší žádosti, stejně jako výjimka.  

![Graf čas odpovědi a mřížky doby odezvy serveru.](./media/app-insights-detect-triage-diagnose/09-dependencies.png)

Jana můžete vyhodnotit vliv na výkon hello každé nasazení obvykle poslední porovnávání každý týden s hello. Pokud je nečekané zhoršování, se vyvolá, s vývojáři relevantní hello.

## <a name="triage-issues"></a>Problémy třídění
Třídění - hodnocení závažnosti hello a rozsah problém - je prvním krokem hello po zjišťování. Měli jsme vyvolávající hello team půlnoci? Nebo může být až další pohodlný mezera hello hello nevyřízené ponecháno? Existují některé klíčové otázky v třídění.

Jak často se se děje? grafy Hello v okně Přehled hello zadejte nějaký problém tooa perspektivy. Například hello Fabrikam aplikace generuje čtyři webového testu výstrahy jednu noc. Prohlížení hello grafu v hello ráno, hello team by mohli zobrazit, aby byly skutečně některé červené tečky, i když stále většinu hello testů byly zelená. Podrobnostem hello dostupnosti grafu, bylo naprosto jasné, že všechny tyto přerušované problémy byly z umístění jeden test. To se samozřejmě potíže se sítí, které mají vliv jenom jeden postup a sám s největší pravděpodobností zrušte.  

Naopak stabilní a výrazné zvýšení v grafu hello počty výjimky nebo odezvy je samozřejmě něco toopanic o.

Užitečné třídění cílem je, zkuste ho sami. Pokud spustíte do hello stejný problém, víte, jsou skutečná.

Jaké podíl uživatelů ovlivněných? tooobtain hrubý odpovědí, vydělte míra selhání hello počet relací hello.

![Grafy neúspěšných požadavků a relací](./media/app-insights-detect-triage-diagnose/10-failureRate.png)

Po pomalé odezvy se porovnejte hello tabulky nejpomalejší neodpovídá požadavků s četností hello využití každé stránky.

Jak důležité je scénář hello blokované? Pokud je to funkčního problému blokování konkrétní uživatelský scénář, záleží mnohem? Pokud zákazníci nelze platit svými účty, je to závažné; Pokud nemohou změnit svoje předvolby barvy obrazovky, možná ho můžete počkat. Dobrý den podrobnosti události hello nebo výjimky nebo hello identity hello pomalé stránky, zjistíte, kde mají zákazníci potíže s.

## <a name="diagnose-issues"></a>Diagnostikovat problémy
Diagnostika není hello poměrně stejné jako ladění. Než začnete trasování prostřednictvím hello kódu, byste měli mít hrubý představu o tom, proč, kde a kdy dochází k hello problém.

**Pokud k tomu dojít?**  hello Historický přehled poskytované hello události a metriky grafy umožňuje snadno toocorrelate důsledky s možné příčiny. Pokud jsou v odpovědi čas nebo výjimky sazby přerušované vrcholů, podívejte se na počtu žádostí o hello: Pokud je nejlepší při hello stejný čas, pak to vypadá problém prostředků. Potřebujete tooassign další procesoru nebo paměti? Nebo je závislost, která nemůže spravovat hello zatížení?

**Je nám?**  Pokud máte nečekané pokles výkonu konkrétní typ požadavku – například když hello zákazník požaduje stavu účtu - pak je možné, může to být externí subsystému spíše než webové aplikace. V Průzkumníku metrik vyberte hello míra selhání závislostí a doby trvání závislosti sazby a porovnání jejich historií přes hello po několik hodin nebo dnů s hello problému, který jste rozpoznali. Pokud jsou existuje korelace změny, může být externí subsystému tooblame.  

![Grafy závislostí selhání a dobu trvání volání toodependencies](./media/app-insights-detect-triage-diagnose/11-dependencies.png)

Některé pomalé závislosti jsou problémy, které informace o zeměpisné poloze. Společnost Fabrikam Bank používá virtuální počítače Azure a zjistit, že jejich měl nechtěně umístění jejich webový server a server poskytující účty v různých zemí. Představují výrazné zlepšení byl způsobené migrace jeden z nich.

**Co jsme?** Pokud se problém hello nezobrazí toobe v závislost a nebyl vždy existuje, je pravděpodobně způsobena změnou poslední. Hello Historický přehled poskytované hello metriky a události grafy umožňuje snadno toocorrelate změny nečekané s nasazením. Který zužuje hello hledat hello problém.

**Co se děje?** Některé problémy dojít pouze zřídka a může být obtížné tootrack dolů testování do offline režimu. Jediné, co můžeme udělat je tootry toocapture hello chyb případě za provozu. Si můžete prohlédnout hello výpisy zásobníku v sestavách výjimka. Kromě toho můžete zapsat trasování volání, buď vašeho oblíbeného rozhraní protokolování, nebo TrackTrace() nebo TrackEvent().  

Společnost Fabrikam měl občasný problém s přenosy mezi společnostmi, ale pouze s určitými typy účtu. toounderstand lepší co se děje, budou vloženy TrackTrace() volání na klíčové body v kódu hello připojení typ účtu hello jako vlastnost tooeach volání. Které provádí ho snadno toofilter se pouze tyto trasování v diagnostické vyhledávání. Jako vlastnosti a míry toohello trasování volání se také připojit hodnoty parametrů.

## <a name="respond-toodiscovered-issues"></a>Odpověď toodiscovered problémy
Jakmile jste diagnostice hello problém, můžete použít plán toofix ho. Možná budete potřebovat tooroll zpět ke změnám, nebo může být můžete právě pokračujte a opravte ho. Po dokončení opravy hello Application Insights poznáte, jestli byla úspěšná.  

Společnost Fabrikam Bank vývojový tým trvat více strukturovanými měření tooperformance přístup, než používají toobefore používají Application Insights.

* Na stránce Přehled služby Application Insights hello nastavují výkonnostní cíle z hlediska konkrétní míry.
* Jejich návrh měření výkonu do aplikace hello od začátku hello, jako je například hello metriky, které měření postupu uživatele prostřednictvím nálevky.  


## <a name="monitor-user-activity"></a>Monitorování aktivity uživatelů
Když je trvale dobré doba odezvy a existuje několik výjimek, tým vývojářů hello můžete přesunout na toousability. Můžete myslíte o tom, jak tooimprove hello zkušeností uživatelů a jak tooencourage další uživatelé tooachieve hello požadovaných cílů.

Application Insights může být také použít toolearn uživatelů při práci s aplikací. Jakmile je plynulý chod, hello team přeje tooknow funkce, které jsou hello nejoblíbenější, co uživatelé jako nebo mít potíže s a jak často se vrátí. Které budou pomoci určit priority nadcházející práci. A jejich můžete naplánovat úspěšné hello toomeasure každé funkce jako součást cyklu vývoje hello. 

Například běžný uživatel cesty přes hello webu má zrušte "trychtýřového grafu." Podívejte se na hello míry různé typy úvěr mnoho zákazníků. Zmenšete počet, přejděte na toofill v podobě hello uvozovek. Několik těch, kteří získat nabídky, pokračujte a vyjměte hello úvěr.

![Spočítá počet zobrazení stránky](./media/app-insights-detect-triage-diagnose/12-funnel.png)

Vzhledem k tomu kde hello největší množství zákazníků vyřadit, hello firmy pracovat na tom, jak tooget více uživatelů prostřednictvím toohello dolní části hello trychtýřový. V některých případech může být selhání uživatelské rozhraní (UX) – například tlačítka 'Další' hello je pevný toofind, nebo nejsou zřejmé hello pokyny. Existuje více pravděpodobně větších obchodní důvody pro rozevírací li: možná jsou příliš vysoké míry úvěr hello.

Libovolnou z důvodů hello hello data pomáhají hello team vycházejí co uživatelé dělají. Další sledování, který může být volání vložit toowork se podrobněji. TrackEvent() lze použít toocount žádné akce uživatele, z hello podrobností dobře jednotlivé tlačítko klikne, herních bonusů toosignificant například platícího vypnout úvěr.

tým Hello je získávání použité toohaving informace o činnosti uživatelů. V současné době vždy, když se navrhnout novou funkci, pracují na tom, jak bude získání zpětné vazby o jeho používání. Jejich návrh sledování volání do funkce hello od začátku hello. V každém cyklu vývoje používají funkce hello tooimprove hello zpětné vazby.

[Další informace o sledování využití](app-insights-usage-overview.md).

## <a name="apply-hello-devops-cycle"></a>Použít hello DevOps cyklu
Proto je jak jedno použití team Application Insights nejenom toofix jednotlivé problémy, ale tooimprove jejich životního cyklu. Snad že ho udělil některé nápady o tom, jak Application Insights vám může pomoct se správou výkonu aplikací v aplikaci.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky
Můžete začít používat několika způsoby v závislosti na vlastnosti hello vaší aplikace. Vyberte, co vám nejvíc vyhovuje:

* [Webové aplikace ASP.NET](app-insights-asp-net.md)
* [Webové aplikace Java](app-insights-java-get-started.md)
* [Webové aplikace Node.js](app-insights-nodejs.md)
* Nasazené aplikace, které jsou hostované na [IIS](app-insights-monitor-web-app-availability.md), [J2EE](app-insights-java-live.md), nebo [Azure](app-insights-azure.md).
* [Webové stránky](app-insights-javascript.md) -jedné stránky aplikace nebo obyčejnou webová stránka – použít samostatně nebo v tooany přidání možností serveru hello.
* [Testy dostupnosti](app-insights-monitor-web-app-availability.md) tootest hello aplikace z veřejného Internetu.
