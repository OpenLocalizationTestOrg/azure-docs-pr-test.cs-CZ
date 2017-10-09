---
title: "aaaAnalyze výkonu hraničního uzlu v Azure CDN | Microsoft Docs"
description: "Analýza výkonu hraničního uzlu v Microsoft Azure CDN. Analýza výkonu Edge poskytuje podrobné informace o provozu a využití šířky pásma pro hello CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 35361d1c5e27fc6b8536c29e33c2ed217ee4d17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analýza výkonu hraničního uzlu v Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled
Analýza výkonu Edge poskytuje podrobné informace o provozu a využití šířky pásma pro hello CDN. Tato informace může být pak trendů statistických použité toogenerate, které umožňují toogain náhled na tom, jak vaše prostředky probíhá v mezipaměti a doručené tooyour klientů. Pak díky tomu, že jste tooform by měla být strategie na tom, jak se toooptimize hello doručování obsahu a toodetermine co problémy řešit toobetter využívají hello CDN. V důsledku toho budou mít tooimprove data doručení výkonu pouze, ale budete také být schopný tooreduce náklady na CDN.

> [!NOTE]
> Všechny sestavy použít čas UTC nebo GMT zápis při zadávání data a času.
> 
> 

## <a name="reports-and-log-collection"></a>Sestavy a kolekce protokolu
Data aktivit CDN sbírají modulem analýzy výkonu Edge hello předtím, než to může generovat sestavy na něm. Tento proces shromažďování nastane jednou denně a popisuje hello aktivity, ke kterým došlo během hello předchozí den. To znamená, které sestavy statistik představují ukázku statistiky hello den v době hello byl zpracován a nechcete nutně obsahovat hello kompletní sadu dat pro hello aktuálního dne. primární Funkce Hello tyto sestavy je tooassess výkon. Není vhodné používat pro účely fakturace nebo přesné číselné statistiky.

> [!NOTE]
> nezpracovaná data Hello, ze které se generují Edge výkonu analytické sestavy je k dispozici alespoň 90 dní.
> 
> 

## <a name="dashboard"></a>Řídicí panel
řídicí panel analýzy výkonu Edge Hello sleduje aktuálním a historickém přenos CDN prostřednictvím grafu a statistiky. Použijte trendy tento řídicí panel toodetect poslední a dlouhodobé na výkon hello CDN provozu pro váš účet.

Tento řídicí panel se skládá z:

* Interaktivní graf, který umožňuje hello vizualizace klíčové metriky a trendů.
* Časová osa poskytuje představu o dlouhodobé vzory pro klíčové metriky a trendy.
* Klíčové metriky a statistické informace o tom, zlepšuje naše síť CDN webový provoz měřený podle celkového výkonu, využití a efektivitu.

### <a name="accessing-hello-edge-performance-dashboard"></a>Přístup k hello edge výkonu řídicí panel
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **Analytics** kartu a potom hover přes hello **Edge prováděcí Analytics** plovoucím panelem.  Klikněte na **řídicí panel**.
   
    řídicí panel analytics Hello edge uzlu se zobrazí.

### <a name="chart"></a>Graf
řídicí panel Hello obsahuje graf, který sleduje metriky v hello časové období vybrané v hello ose, který se zobrazí pod ním.  Časová osa, který grafech až toohello poslední dva roky CDN aktivity se zobrazí pod hello grafu.

#### <a name="using-hello-chart"></a>Pomocí graf hello
* Ve výchozím nastavení bude grafu zobrazena míru efektivitu mezipaměti hello hello posledních 30 dnů.
* Tento graf se generují z dat kompletován každý den.
* Ukazatele myši na den na spojnicový graf hello označí hodnoty data a hello hello metriky na datum.
* Kliknutím zvýrazněte víkendů tootoggle překrytí světla šedé svislých pruhů, které představují víkendů do grafu hello. Tento typ překrytí je užitečné pro identifikaci vzory přenosů dat přes víkendech.
* Klikněte na zobrazení jeden rok před tootoggle překrytí hello předchozí aktivity rok přes hello stejné časové období do grafu hello. Tento typ porovnání poskytuje přehled o způsobech dlouhodobé používání CDN. Hello pravém dolním rohu grafu hello obsahuje legendy, která určuje hello barva kód pro každý spojnicový graf.

#### <a name="updating-hello-chart"></a>Aktualizace hello grafu
* Časové rozmezí: Proveďte jeden z následujících hello:
  * Vyberte požadovanou oblast hello v ose hello. Graf Hello bude aktualizován s daty, která odpovídá toohello vybrané časové období.
  * Poklikejte na soubor hello grafu toodisplay všechny dostupné historická data si tooa maximálně dva roky.
* Metrika: Kliknutím na ikonu hello grafu, který se zobrazí další metrika toohello potřeby. Graf Hello a časový horizont hello se aktualizují s daty pro odpovídající metrika hello.

### <a name="key-metrics-and-statistics"></a>Klíčové metriky a statistiky
#### <a name="efficiency-metrics"></a>Efektivita metriky
účelem Hello tyto metriky je toosee, zda je možné zlepšit efektivitu mezipaměti. hlavní výhody Hello odvozené od efektivitu mezipaměti jsou:

* Hello zdrojový server, což může vést ke snížení zatížení:
  * Lepší výkon webového serveru.
  * Snižuje provozní náklady.
* Vylepšené akcelerace doručování dat, vzhledem k tomu, že další žádosti se zpracuje přímo z hello CDN.

| Pole | Popis |
| --- | --- |
| Efektivitu mezipaměti |Určuje procento hello přenášených dat, která zpracování z mezipaměti. Tato metriky míry, když v mezipaměti verzi hello požádal zpracování obsahu přímo z hello CDN (servery edge) toorequesters (např. webový prohlížeč) |
| Rychlost přístupů do |Určuje procento hello požadavků, které se spouští z mezipaměti. Tato metriky míry, když v mezipaměti verzi hello požadovaného zpracování obsahu přímo z hello CDN (servery edge) toorequesters (např. webový prohlížeč). |
| % Vzdálené bajtů – bez konfigurace mezipaměti |Určuje procento hello provoz, který zpracování z toohello servery původu CDN (servery edge), která nebudou zapisována do mezipaměti v důsledku funkce mezipaměti nepoužívat hello (stroj pravidel HTTP). |
| % Vzdálené bajtů – platnost mezipaměti |Určuje procento hello provoz, který zpracování z toohello servery původu CDN (hraniční servery) v důsledku zastaralé opětovné ověření obsahu. |

#### <a name="usage-metrics"></a>Metriky využití
účelem Hello tyto metriky je tooprovide aspekty hello následující náklady vyjímání opatření:

* Minimalizace provozní náklady prostřednictvím hello CDN.
* Snižuje výdaje CDN prostřednictvím efektivitu mezipaměti a komprese.

> [!NOTE]
> Provoz svazku čísla představují provoz, který byl použit ve výpočtech poměr a procent a může zobrazit pouze část celkového provozu hello vysoký počet zákazníků.
> 
> 

| Pole | Popis |
| --- | --- |
| Odeslané bajty průměr |Označuje hello průměrný počet bajtů přenesených pro každý požadavek z hello CDN (servery edge) toohello žadatel (např. webový prohlížeč). |
| Žádné rychlost bajtů konfigurace mezipaměti |Určuje procento hello provoz z hello CDN (servery edge) toohello žadatel (např. webový prohlížeč), nebudou zapisována do mezipaměti z důvodu toohello mezipaměti Nepoužívat funkce. |
| Míra komprimované bajtů |Určuje procento hello přenosy z hello CDN (servery edge) toorequesters (např. webový prohlížeč) v komprimovaném formátu. |
| Odeslané bajty |Označuje hello množství dat, v bajtech, které byly dodány z hello CDN (servery edge) toohello žadatel (např. webový prohlížeč). |
| Bajtů |Označuje hello množství dat, v bajtech odeslaný žadatelů o (např. webový prohlížeč) toohello CDN (servery edge). |
| Vzdálené bajtů |Označuje hello množství dat, v bajtech, odeslané ze sítě CDN a zákazník servery toohello původu CDN (servery edge). |

#### <a name="performance-metrics"></a>Metriky výkonu
účelem Hello tyto metriky je tootrack celkový výkon CDN pro provozu.

| Pole | Popis |
| --- | --- |
| Rychlost přenosu |Označuje hello průměrná rychlost přenosu obsahu ze hello CDN tooa žadatel byl. |
| Doba trvání |Označuje hello průměrný čas (v milisekundách) trvalo toodeliver asset tooa žadatel (např. webový prohlížeč). |
| Rychlost komprimované požadavků |Určuje procento hello přístupů, které byly dodány z hello CDN (servery edge) toohello žadatel (např. webový prohlížeč) v komprimovaném formátu. |
| Míra chyb 4xx |Určuje procento hello přístupů, které generuje stavový kód 4xx. |
| Míra chyb 5xx |Určuje procento hello přístupů, které generuje 5xx stavový kód. |
| Přístupů |Označuje hello počet žádostí o obsahu CDN. |

#### <a name="secure-traffic-metrics"></a>Zabezpečený provoz metriky
účelem Hello tyto metriky je tootrack CDN výkonu pro komunikaci přes protokol HTTPS.

| Pole | Popis |
| --- | --- |
| Zabezpečení efektivitu mezipaměti |Určuje procento hello data přenesená pro požadavky HTTPS, které byly obsluhovat z mezipaměti. Tato metrika měří při uložené v mezipaměti verzi hello požádal zpracování obsahu přímo z hello CDN (servery edge) toorequesters (např. webový prohlížeč) přes protokol HTTPS. |
| Zabezpečení rychlost přenosu |Označuje hello průměrná rychlost přenosu obsahu ze hello CDN (servery edge) toorequesters (například webové servery) se přes protokol HTTPS. |
| Průměrná doba trvání zabezpečení |Označuje hello průměrný čas (v milisekundách) trvalo toodeliver asset tooa žadatel (např. webový prohlížeč) přes protokol HTTPS. |
| Zabezpečené přístupů |Označuje číslo hello požadavky HTTPS pro obsahu CDN. |
| Zabezpečení odeslané bajty |Označuje hello komunikaci přes protokol HTTPS, v bajtech, které byly dodány z hello CDN (servery edge) toohello žadatel (např. webový prohlížeč). |

## <a name="reports"></a>Reports
Každou sestavu v tomhle module obsahuje graf a statistiku využití šířky pásma a přenosy dat pro různé typy metriky (např. stavové kódy HTTP, mezipaměti stavových kódů, požadavek adresy URL, atd.). Tyto informace může být použité toodelve hlouběji do jak obsah je zpracování tooyour klientů a toofine tune CDN chování tooimprove data doručení výkonu.

### <a name="accessing-hello-edge-performance-reports"></a>Přístup k sestavy pro zvýšení výkonu edge hello
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **Analytics** kartu a potom hover přes hello **Edge prováděcí Analytics** plovoucím panelem.  Klikněte na **velkého objektu HTTP**.
   
    Hello hraniční uzel analytics sestavy obrazovka se zobrazí.

| Sestava | Popis |
| --- | --- |
| Denní souhrn |Umožňuje tooview každodenní provoz trendy za zadané časové období. Každý panelu na tomto grafu představuje konkrétní datum. velikost Hello hello pruhu určuje hello celkový počet přístupů, které došlo k tomuto datu. |
| Hodinové souhrn |Umožňuje tooview hodinové provoz trendy za zadané časové období. Každý pruh na tomto grafu představuje jednu hodinu v konkrétní datum. Hello velikost panelu hello Určuje celkový počet přístupů, které došlo k chybě danou dobu hello. |
| Protokoly |Zobrazí výčet hello komunikace mezi hello HTTP a HTTPS protokoly. Graf prstenec určuje procento hello přístupů, které pro každý typ protokolu došlo k chybě. |
| Metody HTTP |Umožňuje tooget rychlý přehled metod protokolu HTTP, které jsou právě používané toorequest vaše data. Metody požadavku HTTP nejběžnější hello jsou obvykle GET, POST a HEAD. Graf prstenec určuje procento hello přístupů, které pro každý typ metoda požadavku HTTP došlo k chybě. |
| Adresy URL |Obsahuje graf zobrazující hello top 10 požadované adresy URL. Pro každou adresu URL se zobrazí panelu. Hello výška panelu hello Určuje, kolik přístupů generovaných konkrétní adresu URL přes hello časové rozpětí hello zpráva vztahuje. Statistika pro hello prvních 100 požadovaného že adresy URL se zobrazí pod tohoto grafu. |
| Záznamů CNAME |Obsahuje graf, který zobrazuje hello top 10 záznamů CNAME použít toorequest prostředky přes hello časové období sestavy. Statistika pro hello prvních 100 požadovaného že záznamů CNAME se zobrazí pod tohoto grafu. |
| Zdroje |Obsahuje graf zobrazující hello prvních 10 CDN nebo zákazníků počátek serverů, ze kterých se požadovaly prostředky v zadaném časovém období. Statistika pro hello prvních 100 požadovaného přímo níže tohoto grafu se zobrazují servery původu CDN nebo zákazníků. Zákazník počátek servery se identifikují podle názvu hello definované v hello možnost název adresáře. |
| Geograficky bodů POP |Zobrazuje, kolik provozu je směrovány přes konkrétní point of presence (POP). Zkratka tří písmen Hello představuje POP v naše síť CDN. |
| Klienti |Obsahuje graf zobrazující hello top 10 klientů, kteří požadované prostředky v zadaném časovém období. Pro účely hello této sestavy, všechny požadavky, které pocházejí z hello stejné IP adresy jsou považovány za toobe z hello stejného klienta. Statistiky pro horní 100 klienty hello se zobrazí pod tohoto grafu. Tato sestava je užitečné pro určení aktivity vzory stahování pro horní klienty. |
| Stavy mezipaměti |Poskytuje podrobné rozpis chování mezipaměti, který může odhalit přístupy pro zlepšení hello celkové činnost koncového uživatele. Vzhledem k tomu, že nejrychlejší výkonu hello pochází z přístupů k mezipaměti, můžete optimalizovat rychlosti doručování dat minimalizovat Neúspěšné přístupy do mezipaměti a přístupů k mezipaměti vypršela platnost. |
| ŽÁDNÉ podrobnosti |Obsahuje graf zobrazující hello top 10 adresy URL pro prostředky, pro které nebyla zkontrolována aktuálnost obsahu mezipaměti po zadanou dobu. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o CONFIG_NOCACHE |Obsahuje graf, který zobrazí prvních 10 hello adresy URL prostředky, které nebyly uloženy v mezipaměti z důvodu konfigurace CDN toohello zákazníka. Tyto typy prostředků byly zpracovat přímo z hello zdrojový server. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| UNCACHEABLE podrobnosti |Obsahuje graf zobrazující hello top 10 adresy URL pro prostředky, které nelze uložit do mezipaměti z důvodu data toorequest záhlaví. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o TCP_HIT |Obsahuje graf zobrazující hello top 10 adresy URL pro prostředky, které se zpracovávají okamžitě z mezipaměti. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o TCP_MISS |Obsahuje graf zobrazující hello top 10 adresy URL pro prostředky, které mají stav mezipaměti TCP_MISS. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o TCP_EXPIRED_HIT |Obsahuje graf zobrazující hello top 10 adresy URL pro zastaralé prostředky, které byly obsluhovat přímo z hello POP. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o TCP_EXPIRED_MISS |Obsahuje graf zobrazující hello top 10 adresy URL pro zastaralé prostředků, pro které je nová verze měl toobe načíst ze serveru původu hello. Statistiky pro hello nejvyšší 100 adresy URL pro tyto typy prostředků se zobrazí pod tohoto grafu. |
| Podrobnosti o TCP_CLIENT_REFRESH_MISS |Obsahuje pruhový graf, který zobrazuje hello top 10 adresy URL pro prostředky, které byly získány ze zdrojový server z důvodu tooa mezipaměti ne žádost od klienta hello. Přímo níže tento graf se zobrazují statistiky pro hello nejvyšší 100 adresy URL pro tyto typy požadavků. |
| Typy žádostí klienta |Označuje typ hello požadavků, které byly provedeny podle klientů protokolu HTTP (například prohlížeče). Tato sestava obsahuje prstenec graf, který poskytuje si smysl jako toohow, ke které dochází ke zpracování požadavků. Níže hello grafu se zobrazují informace šířky pásma a přenosy dat pro každý typ požadavku. |
| Uživatelský Agent |Obsahuje zobrazení hello top 10 uživatele agenty toorequest svůj obsah pomocí našich CDN pruhový graf. Uživatelský agent je obvykle webový prohlížeč, přehrávač médií nebo prohlížeče mobilního telefonu. Statistika pro hello nejvyšší 100 Uživatelští agenti se zobrazí pod tento graf. |
| Odkazující servery |Obsahuje zobrazení hello top 10 odkazující servery toocontent přistupovat prostřednictvím našich CDN pruhový graf. URL hello hello webové stránky nebo prostředku, který odkazuje tooyour obsah je obvykle odkazující server. Podrobné informace najdete níže hello grafu pro hello nejčastější 100 odkazující servery. |
| Typy komprese |Obsahuje prstenec grafu, jež rozdělí požadované prostředky podle jestli byly komprimované servery edge. Procento Hello komprimované prostředků je rozdělená podle typu hello používá komprese. Podrobné informace najdete níže hello grafu pro každý typ komprese a stav. |
| Typy souborů |Obsahuje pruhový graf, který zobrazí hello top 10 typy souborů, které bylo vyzváno prostřednictvím našich CDN pro váš účet. Pro účely hello této sestavy je definován typ souboru hello asset příponu názvu souboru a typu média Internetu (například .html \[text/html\], .htm \[text/html\], .aspx \[text/html\]atd.). Podrobné informace najdete níže hello grafu pro typy hello nejvyšší 100 souborů. |
| Jedinečný soubory |Obsahuje graf, který ukazuje zeměpisný hello celkový počet jedinečných prostředky, které si vyžádali v určitý den v zadaném časovém období. |
| Souhrn tokenů ověřování |Obsahuje výsečový graf poskytující rychlý přehled o tom, zda byly požadovaných prostředků chráněny pomocí ověřování na základě tokenu. Chráněné prostředky se zobrazí v grafu hello podle toohello výsledky pokus o ověření. |
| Token Auth odepřít podrobnosti |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavků, které byly odepřen z důvodu ověřování na základě tooToken. |
| Kódy odpovědí HTTP |Obsahuje rozpis systémů stavové kódy hello HTTP (například 200 OK 403 Zakázáno, 404 nebyl nalezen, apod.), byly tooyour HTTP klientů doručil naše servery edge. Výsečový graf, které vám umožní tooquickly posoudit, jak byly obsluhovat vaše prostředky. Podrobné statistické údaje se poskytuje pro každý kód odpovědi níže hello grafu. |
| Chyb 404 |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavky, jejichž výsledkem 404 kód odpovědi nebyl nalezen. |
| Chyby 403 |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavky, jejichž výsledkem 403 Zakázáno kód odpovědi. Kód odpovědi 403 Zakázáno nastane, když požadavek je odepřen zdrojový server zákazníka nebo hraniční server na našem POP. |
| 4xx chyby |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavků, které kód odpovědi v rozsahu 400 hello. Vyloučeny z této sestavy jsou 403 nebyl nalezen a kódů odpovědi 404 zakázáno. Kód odpovědi 4xx obvykle nastane, když požadavek byl odepřen v důsledku chyby klienta. |
| 504 chyby |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavků, které 504 kód odpovědi limitu brány. Když dojde k vypršení časového limitu, pokud proxy server HTTP se pokouší toocommunicate k jinému serveru dojde k 504 kód odpovědi limitu brány. V případě hello naše CDN 504 kód odpovědi vypršel časový limit brány obvykle dochází, když hraniční server nelze tooestablish komunikaci s původním serveru zákazníka. |
| Chyby 502 |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavků, které 502 kód odpovědi Chybná brána. Kód odpovědi 502 Chybná brána nastane, když dojde k chybě protokolu HTTP mezi serverem a proxy serveru HTTP. V případě hello z našich CDN 502 kód odpovědi Chybná brána obvykle dojde v případě zdrojový server zákazníka vrátí hraniční server tooan neplatnou odpověď. Odpověď je neplatný, pokud nelze analyzovat nebo pokud je neúplný. |
| Chyby 5xx |Obsahuje pruhový graf, který vám umožní tooview hello top 10 požadavků, které kód odpovědi v rozsahu 500 hello.  Vyloučeny z této sestavy jsou 502 Chybná brána a kódů odpovědi 504 limitu brány. |

## <a name="see-also"></a>Viz také
* [Přehled Azure CDN](cdn-overview.md)
* [Statistiky v reálném čase v Microsoft Azure CDN](cdn-real-time-stats.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
* [Sestavy pokročilé HTTP](cdn-advanced-http-reports.md)

