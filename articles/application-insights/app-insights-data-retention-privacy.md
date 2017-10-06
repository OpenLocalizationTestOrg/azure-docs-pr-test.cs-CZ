---
title: "aaaData uchování a úložiště v Azure Application Insights | Microsoft Docs"
description: "Prohlášení o zásadách uchovávání dat a ochrana osobních údajů"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: bwren
ms.openlocfilehash: 7823431d03a57db5268d2724a0604e40666009f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Shromažďování, uchování a ukládání dat v nástroji Application Insights


Při instalaci [Azure Application Insights] [ start] SDK v aplikaci, odešle telemetrická data o vaší aplikaci toohello cloudu. Samozřejmě zodpovědná vývojáři mají tooknow přesně jaká data se odesílají, co se stane toohello data a jak můžete zachovat kontrolu nad jeho. Konkrétně může být odeslána citlivá data, kde je uložený a jak zabezpečení je? 

První, hello krátké odpovědí:

* Hello standardní telemetrie modulů, které se spouštějí "hello předinstalované" se pravděpodobně toosend citlivá data toohello služby. telemetrie Hello se týká zatížení, metriky výkonu a využití, sestavy výjimek a další diagnostická data. Hello hlavní uživatelská data zobrazená v hello diagnostické zprávy jsou adresy URL; ale aplikace nesmí v žádném případě put citlivá data v prostém textu v adrese URL.
* Můžete napsat kód, který odešle další vlastní telemetrii toohelp k monitorování využití a Diagnostika. (Toto rozšíření je skvělé funkce Application Insights.) Je možné, že, toowrite tento kód tak, že obsahují osobní a další citlivá data. Pokud vaše aplikace funguje s taková data, byste měli použít důkladné zkontrolujte procesy tooall hello kód napsaný.
* Při vývoji a testování vaší aplikace, je snadné tooinspect co odesílány hello SDK. Hello data se zobrazí v hello ladění výstupu windows hello IDE a prohlížeče. 
* Hello data uchovávána v [Microsoft Azure](http://azure.com) servery v hello USA nebo Evropa. (Ale aplikace můžou běžet kdekoli.) Azure má [silné zabezpečení zpracovává a splňuje širokou škálu standardů dodržování předpisů](https://azure.microsoft.com/support/trust-center/). Pouze vám a vašemu určené týmu mít přístup k datům tooyour. Zaměstnanci Microsoftu může mít omezený přístup tooit pouze za určité omezené okolností s vašeho vědomí. Je šifrovaný během přenosu, i když není v hello servery.

Hello zbývající části tohoto článku popisuje podrobněji na tyto odpovědi. Je určený toobe samostatná, takže můžete je zobrazit toocolleagues, kteří nejsou součástí vašeho týmu.

## <a name="what-is-application-insights"></a>Co je Application Insights?
[Azure Application Insights] [ start] je služba poskytovaná společnosti Microsoft, který vám pomůže zlepšit hello výkonnost a použitelnost vaše živé aplikace. Sleduje aplikace všechny hello, když je spuštěná, během testování a po publikována nebo nasazená. Application Insights vytvoří grafů a tabulek, které můžete zobrazit, například jaké časy den zobrazí většina uživatelů, jak je aplikace reaguje hello, a také, který je poskytovaný žádné externí služby, které závisí na. Pokud dojde k chybě, chyby nebo problémy s výkonem, můžete hledat hello telemetrická data v příčina hello toodiagnose podrobností. A hello služby je odešle e-mailů, pokud nedošlo k žádným změnám v hello dostupnosti a výkonu vaší aplikace.

V pořadí tooget tato funkce instalaci Application Insights SDK v aplikaci, která se stane součástí jeho kód. Pokud vaše aplikace běží, hello SDK monitoruje své činnosti a odesílá telemetrii toohello Application Insights service. Toto je Cloudová služba hostovaná společností [Microsoft Azure](http://azure.com). (Ale Application Insights funguje u všech aplikací, nikoli pouze ty, které jsou hostované v Azure.)

![Hello SDK v aplikaci odesílá telemetrii toohello služby Application Insights.](./media/app-insights-data-retention-privacy/01-scheme.png)

Hello služby Application Insights ukládá a analyzuje příchozí telemetrická data hello. Analýza hello toosee nebo vyhledávání prostřednictvím hello uložené telemetrických dat, přihlásíte tooyour účet Azure a hello otevřete prostředek Application Insights pro vaši aplikaci. Můžete také sdílené složce přístup toohello data s jinými členy týmu nebo s Azure stanoveným příjemcům.

Můžete mít data exportovaná z hello služby Application Insights, například tooa databáze nebo tooexternal nástroje. Každý nástroj poskytnete speciální klíč, který můžete získat ze služby hello. v případě potřeby se dají odvolávat Hello klíč. 

Sadách Application Insights SDK jsou k dispozici pro celou řadu typů aplikací: webové služby hostované v vaše servery J2EE nebo v technologii ASP.NET, nebo v Azure; webovými klienty – to znamená, hello kód spuštěný na webové stránce; desktopové aplikace a služby; aplikace zařízení, například Windows Phone, iOS a Android. Všechny odesílání telemetrie toohello stejnou službu.

## <a name="what-data-does-it-collect"></a>Jaká data ho shromáždit?
### <a name="how-is-hello-data-is-collected"></a>Jak se hello data se shromažďují?
Existují tři zdroje dat:

* Hello SDK, kterou můžete integrovat s vaší aplikací buď [vývojem](app-insights-asp-net.md) nebo [v době běhu](app-insights-monitor-performance-live-website-now.md). Existují různé sady SDK pro typy jinou aplikaci. K dispozici je také [SDK pro webové stránky](app-insights-javascript.md), což způsobí načtení do prohlížeče hello koncového uživatele společně s stránku hello.
  
  * Každý sada SDK obsahuje řadu [moduly](app-insights-configuration-with-applicationinsights-config.md), které používají různé techniky toocollect různé typy telemetrie.
  * Pokud instalujete hello SDK v vývoj, můžete jeho rozhraní API toosend vlastní telemetrii, v přidání toohello standardní moduly. Tato vlastní telemetrii může obsahovat žádná data, která chcete toosend.
* V některých webových serverů existují také agentů, které běží souběžně s hello aplikace a odesílat telemetrická data o využití procesoru, paměti a sítě obsazení. Například virtuální počítače Azure, hostitelů Docker a [servery J2EE](app-insights-java-agent.md) může mít tyto agenty.
* [Testy dostupnosti](app-insights-monitor-web-app-availability.md) společnosti Microsoft, který posílat v pravidelných intervalech požadavky tooyour webové aplikace spouštějí procesy. výsledky Hello odešlou toohello služby Application Insights.

### <a name="what-kinds-of-data-are-collected"></a>Jaké druhy dat se shromažďují?
jsou Hello hlavních kategorií:

* [Webový server telemetrie](app-insights-asp-net.md) -požadavky HTTP.  Identifikátor URI, doba tooprocess hello žádost kód odpovědi, IP adresa klienta. Id relace.
* [Webové stránky](app-insights-javascript.md) -stránky, uživatele a počty relací. Časů načtení stránky. Výjimky. Volání AJAX.
* Výkon čítače - paměti, procesoru, vstupně-výstupní operace, obsazení sítě.
* Klient a server kontext - OS, národní prostředí, typ zařízení, prohlížeč, rozlišení obrazovky.
* [Výjimky](app-insights-asp-net-exceptions.md) a havárií - **výpisy zásobníku**, sestavení id, typ procesoru. 
* [Závislosti](app-insights-asp-net-dependencies.md) -volá tooexternal služby, jako je například REST, SQL, AJAX. Identifikátor URI nebo připojovací řetězec, doba trvání, úspěch, příkaz.
* [Testy dostupnosti](app-insights-monitor-web-app-availability.md) – doba trvání testu a kroky, odpovědi.
* [Protokoly trasování](app-insights-asp-net-trace-logs.md) a [vlastní telemetrii](app-insights-api-custom-events-metrics.md) - **nic kódu do protokolů nebo telemetrie**.

[Další podrobnosti](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Jak lze ověřit, co jsou shromažďována?
Pokud vyvíjíte aplikace hello pomocí sady Visual Studio, spusťte aplikaci hello v režimu ladění (F5). telemetrie Hello se zobrazí v okně výstupu hello. Odtud můžete ho zkopírovat a naformátujte ho jako JSON pro snadné kontroly. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

V okně diagnostické hello existuje také srozumitelnější zobrazení.

Pro webové stránky otevřete okno prohlížeče na ladění.

![Stisknutím klávesy F12 a otevřete kartu síť hello.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-toofilter-hello-telemetry-before-it-is-sent"></a>Můžete psát kód toofilter hello telemetrii, před odesláním?
By to bylo možné zápisem [modulu plug-in procesoru telemetrie](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-hello-data-kept"></a>Jak dlouho je hello data uchovávají?
Nezpracovaná data body (to znamená, položky, které se můžete dotazovat v analýzy a zkontrolovat ve vyhledávání) jsou v až too90 dnů. Pokud potřebujete delší, než je tookeep data, můžete použít [průběžné export](app-insights-export-telemetry.md) toocopy ho tooa účet úložiště.

Agregovaná data (to znamená, počty, průměry a další statistické data, která se zobrazí v Průzkumníku metrika) jsou uchovány v interval 1 minutu po dobu 90 dnů.

## <a name="who-can-access-hello-data"></a>Kdo může přistupovat k datům hello?
Hello data jsou viditelná tooyou a, pokud máte účet organizace, členové týmu. 

Je možné exportovat vámi a členové týmu a může být umístění zkopírovaného tooother a předán na tooother osoby.

#### <a name="what-does-microsoft-do-with-hello-information-my-app-sends-tooapplication-insights"></a>Jaké nemá Microsoft dělat s hello informace mé aplikace odešle tooApplication Insights?
Společnost Microsoft používá hello data pouze v pořadí tooprovide hello služby tooyou.

## <a name="where-is-hello-data-held"></a>Kde se nachází hello dat?
* V hello USA nebo Evropa. Když vytvoříte nový prostředek Application Insights, můžete vybrat umístění hello. 


#### <a name="does-that-mean-my-app-has-toobe-hosted-in-hello-usa-or-europe"></a>Znamená to, že aplikace má toobe hostované v USA nebo Evropa hello?
* Ne. Aplikace, můžete spustit v místního hostitele nebo v cloudu hello.

## <a name="how-secure-is-my-data"></a>Do jaké míry je svá data?
Application Insights je služby Azure. Zásady zabezpečení jsou popsané v hello [Azure zabezpečení, ochrany osobních údajů a dodržování předpisů dokumentu white paper](http://go.microsoft.com/fwlink/?linkid=392408).

Hello data se ukládají v serverech Microsoft Azure. Pro účty v hello portálu Azure, účet omezení jsou popsána v hello [Azure zabezpečení, ochrany osobních údajů a dodržování předpisů dokumentu](http://go.microsoft.com/fwlink/?linkid=392408).

Přístup k datům tooyour zaměstnanců společnosti Microsoft je s omezeným přístupem. Jsme přistupovat k datům jenom s vaším svolením a je nutné toosupport používání Application Insights. 

Data v agregace napříč aplikacemi všech našich zákazníků (například datové sazby a průměrná velikost trasování) je použité tooimprove Application Insights.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>By mohla někoho jiného telemetrie narušovat svá data Application Insights?
Další telemetrické tooyour účtu se může odeslat pomocí hello klíč instrumentace, který naleznete v kódu hello webových stránek. S dostatek další data nebude vaše metriky představují správně, výkonu a využití vaší aplikace.

Pokud sdílíte kód s jinými projekty, mějte na paměti tooremove klíč instrumentace.

## <a name="is-hello-data-encrypted"></a>Je hello data zašifrovaná?
Mimo hello servery v současné době.

Všechna data se šifrují, když se přesunuje mezi datovými centry.

#### <a name="is-hello-data-encrypted-in-transit-from-my-application-tooapplication-insights-servers"></a>Je hello data šifrovat při přenosu ze serverů Statistika tooApplication Moje aplikace?
Ano, použijeme https toosend data toohello portál z téměř všechny sady SDK, včetně webových serverů, zařízení a HTTPS webové stránky. Jedinou výjimkou Hello je dat odesílaných ze prostý webové stránky HTTP. 

## <a name="personally-identifiable-information"></a>Identifikovatelné osobní údaje
#### <a name="could-personally-identifiable-information-pii-be-sent-tooapplication-insights"></a>Může odeslat identifikovatelné osobní informace (PII) tooApplication Insights?
Ano, je možné. 

Jako obecné pokyny:

* Většina standardní telemetrie (tedy telemetrické zprávy odesílané bez psaní jakéhokoli kódu) nezahrnuje explicitní PII. Však může být možné tooidentify jednotlivce podle odvozená z kolekce událostí.
* Výjimky a trasování zprávy může obsahovat identifikovatelné osobní údaje
* Vlastní telemetrii – to znamená, volání například TrackEvent, které můžete psát v kódu pomocí rozhraní API nebo protokolu trasování hello - může obsahovat žádná data, které zvolíte.

Tabulka Hello na konci hello tento dokument obsahuje podrobnější popisy shromažďovaných dat hello.

#### <a name="am-i-responsible-for-complying-with-laws-and-regulations-in-regard-toopii"></a>Jsem zodpovědná za soulad s právními předpisy v ohledem tooPII?
Ano. Je vaše odpovědnosti tooensure, který hello shromažďování a používání dat hello v souladu s právními předpisy a podmínky hello společnosti Microsoft Online Services.

Vaši zákazníci měli správně informovat o hello dat, který shromažďuje vaše aplikace a jak se používají hello data.

#### <a name="can-my-users-turn-off-application-insights"></a>Moji uživatelé vypnout Application Insights?
Ne přímo. Jsme neposkytují k přepínači, aby vaši uživatelé mohou pracovat tooturn vypnout Application Insights.

Však můžete implementovat této funkce ve vaší aplikaci. Všechny hello sady SDK zahrnout nastavení rozhraní API, který vypne telemetrii kolekce. 

#### <a name="my-application-is-unintentionally-collecting-sensitive-information-can-application-insights-scrub-this-data-so-it-isnt-retained"></a>Moje aplikace neúmyslně shromažďuje citlivé informace. Můžete Application Insights přesouváním tato data, není zachována?
Application Insights filtrovat nebo odstraňovat data. By měla spravovat hello data správně a vyhnout se odesílání taková data tooApplication statistiky.

## <a name="data-sent-by-application-insights"></a>Data odeslaná Application Insights
Hello sady SDK se liší mezi platformami a je několik součástí, které můžete nainstalovat. (Odkazovat příliš[Application Insights - přehled][start].) Jednotlivé komponenty odešle různých data.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Třídy dat odesílaných v různých scénářích
| Vaše akce | Datové třídy, které jsou shromážděny (viz další tabulce) |
| --- | --- |
| [Přidejte Application Insights SDK tooa .NET webového projektu][greenbrown] |Kontext<br/>Odvodit<br/>Čítače výkonu<br/>Požadavky<br/>**Výjimky**<br/>Relace<br/>uživatelé |
| [Nainstalujte monitorování stavu ve službě IIS][redfield] |Závislosti<br/>Kontext<br/>Odvodit<br/>Čítače výkonu |
| [Přidání Application Insights SDK tooa Java webové aplikace][java] |Kontext<br/>Odvodit<br/>Žádost<br/>Relace<br/>uživatelé |
| [Přidat stránku tooweb JavaScript SDK][client] |ClientContext <br/>Odvodit<br/>Stránka<br/>ClientPerf<br/>AJAX |
| [Definovat výchozí vlastnosti][apiproperties] |**Vlastnosti** na všechny standardní a vlastní události |
| [Volání TrackMetric][api] |Číselné hodnoty.<br/>**Vlastnosti** |
| [Volání sledovat *][api] |Název události<br/>**Vlastnosti** |
| [Volání TrackException][api] |**Výjimky**<br/>Výpis zásobníku<br/>**Vlastnosti** |
| Sady SDK nelze shromažďovat data. Například: <br/> -nemůže získat přístup k čítače výkonu<br/> -výjimky v inicializátoru telemetrie |Diagnostika SDK |

Pro [sady SDK pro jiné platformy][platforms], najdete v části své dokumenty.

#### <a name="hello-classes-of-collected-data"></a>třídy Hello shromážděných dat
| Shromážděná data – třída | Zahrnuje (není vyčerpávající seznam) |
| --- | --- |
| **Vlastnosti** |**Žádná data - určenému kódu** |
| DeviceContext |ID, IP, národní prostředí, model zařízení, sítě, typ sítě, název výrobce OEM, rozlišení obrazovky, instanci Role, název Role, typ zařízení |
| ClientContext |Operační systém, národní prostředí, jazyka, sítě, okno řešení |
| Relace |Id relace |
| Kontext |Název počítače, národní prostředí, operačního systému, zařízení, uživatelské relace, kontextu uživatele, operace |
| Odvodit |geografické umístění z IP adresy, časové razítko, operačního systému, prohlížeč |
| Metriky |Název metriky a hodnotu |
| Události |Název události a hodnotu |
| PageViews |Název adresy URL a stránky nebo obrazovky |
| Výkonu klienta |Název adresy URL/stránky, čas načítání prohlížeče |
| AJAX |Volání protokolu HTTP z tooserver webové stránky |
| Požadavky |Adresa URL, doba trvání, kód odpovědi |
| Závislosti |Typ (SQL, protokolu HTTP,...), připojovací řetězec nebo identifikátor URI, synchronizace nebo asynchronní, doba trvání, úspěch, příkaz jazyka SQL (s monitorování stavu) |
| **Výjimky** |Typ, **zpráva**, zpětná volání, zdrojového souboru a řádku číslo id vlákna |
| Dojde k chybě |Id procesu, id nadřazeného procesu havárií id vlákna; Oprava aplikace, id, sestavení;  Typ výjimky, adresu, důvod; zakódovaná symboly a registrů, binární počáteční a koncové adresy, binární název a cesta, typ procesoru |
| Trasování |**Zpráva** a úroveň závažnosti |
| Čítače výkonu |Čas procesoru, dostupná paměť, rychlost požadavků, výjimka sazba, proces nesdílených bajtů, vstupně-výstupní operace sazba, doba trvání požadavku, délka fronty požadavků |
| Dostupnost |Kód odpovědi webového testu, doba trvání jednotlivých test krok, název testu, časové razítko, úspěch, doby odezvy, umístění testu |
| Diagnostika SDK |Trasovací zpráva nebo výjimky |

Můžete [vypnout některé hello dat úpravou souboru ApplicationInsights.config][config]

## <a name="credits"></a>Kredity
Tento produkt obsahuje GeoLite2 data vytvořená systémem MaxMind, k dispozici z [http://www.maxmind.com](http://www.maxmind.com).



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

