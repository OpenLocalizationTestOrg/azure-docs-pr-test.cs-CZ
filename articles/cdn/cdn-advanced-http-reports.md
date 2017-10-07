---
title: "Statistika využití aaaAnalyze s Azure CDN Rozšířené sestavy HTTP | Microsoft Docs"
description: "Zjistěte, jak toocreate Rozšířené sestavy HTTP v Microsoft Azure CDN. Tyto sestavy poskytují podrobné informace o aktivity CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 3184ec00d089613e25c62762f93043cb4ba68394
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Statistika využití analyzovat pomocí Azure CDN Rozšířené sestavy HTTP
## <a name="overview"></a>Přehled
Tento dokument popisuje Rozšířené sestavy HTTP v Microsoft Azure CDN. Tyto sestavy poskytují podrobné informace o aktivity CDN.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Přístup k rozšířené sestavy HTTP
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **Analytics** kartu a potom hover přes hello **Rozšířené sestavy HTTP** plovoucím panelem.  Klikněte na **velké platformy HTTP**.
   
    ![Portál pro správu CDN - nabídky Rozšířené sestavy](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Zobrazí se možnosti sestav.

## <a name="geography-reports-map-based"></a>Sestavy Geography (mapy na základě)
Nejsou pět sestavy, které využívá výhod mapy tooindicate hello oblastí, ze kterých je vyžadován obsah. Tyto sestavy jsou světové mapy, mapy Spojených státech amerických, Kanada mapy, Evropa mapy a mapy Asie a Tichomoří.

Všechny sestavy založené na mapu určuje pořadí geografické entit (tj, zemích, stavy a provincie) podle toohello Procento přístupů, které pochází z této oblasti. Kromě toho mapu se toohelp vizualizovat hello umístění, ze kterých je požadovaný obsah. Je možné toodo tak podle barevné kódování každou oblast podle toohello množství vyžádání došlo v této oblasti. Zapalovače podbarvené oblasti znamenat nižší požadavků na obsah, zatímco tmavšího oblasti znamenat vyšší úrovně požadavků na obsah.

Podrobné informace o provozu a šířky pásma pro každou oblast zajišťuje přímo pod hello mapy. To vám umožní tooview hello celkový počet přístupů, hello Procento přístupů, hello celkové množství dat přenesených (v gigabajtech) a hello procento dat přenesených pro každou oblast. Zobrazte popis pro každou z těchto metriky. Nakonec po přesunutí ukazatele myši v oblasti (tj, země, stavu nebo provincii), název hello a hello Procento přístupů, k nimž došlo v oblasti hello bude zobrazovat jako popisek.

Stručný popis najdete níže pro každý typ sestavy na základě mapy geography.

| Název sestavy | Popis |
| --- | --- |
| Světové mapy |Tato sestava vám umožní tooview hello po celém světě vyžádání pro obsah CDN. Každé země, jsou rozlišené barevně na hello world mapy tooindicate hello Procento přístupů, které pochází z této oblasti. |
| Spojené státy mapy |Tato sestava vám umožní tooview hello vyžádání pro obsah CDN ve Spojených státech amerických hello. Každý stav je barevné kódování na toto procento mapy tooindicate hello přístupů, které pochází z této oblasti. |
| Mapa Kanada |Tato sestava vám umožní tooview hello vyžádání pro obsah v Kanadě CDN. Každý provincii je barevné kódování na toto procento mapy tooindicate hello přístupů, které pochází z této oblasti. |
| Evropa mapy |Tato sestava vám umožní tooview hello vyžádání pro obsah CDN v Evropě. Každé země je barevné kódování na toto procento mapy tooindicate hello přístupů, které pochází z této oblasti. |
| Asie pacifického mapy |Tato sestava vám umožní tooview hello poptávky vašeho obsahu CDN v Asii. Každé země je barevné kódování na toto procento mapy tooindicate hello přístupů, které pochází z této oblasti. |

## <a name="geography-reports-bar-charts"></a>Sestavy Geography (pruhové grafy)
Existují dva další sestavy, které obsahují statistické informace podle toogeography, horní města a horní zemích. Tyto sestavy zařadit a obsahuje města a zemích, v uvedeném pořadí, podle toohello počet přístupů, které pochází z těchto oblastí. Při generování tento typ sestavy, označí pruhový graf hello top 10 města nebo země, které požadovaný obsah přes konkrétní platformu. Tento pruhový graf, které vám umožní tooquickly vyhodnocení hello oblastí, které generují hello nejvyšší počet požadavků na obsah.

levé straně Hello hello grafu (na ose y) určuje, kolik přístupů došlo k chybě v zadané oblasti hello. Přímo pod hello grafu (na ose x) zjistí popisek pro každý top 10 oblastí hello.

### <a name="using-hello-bar-charts"></a>Pomocí hello pruhové grafy
* Pokud se ukazatel myši přesunete panelu, název hello a hello celkový počet přístupů, k nimž došlo v oblasti hello bude zobrazovat jako popisek.
* Hello popis tlačítka pro hello horní města Sestava identifikuje města jeho název, Kraj a zkratka země.
* Pokud hello města nebo oblasti (tj, Kraj), ze které pochází požadavek nebylo možné určit, pak ji bude znamenat, že neznámé. Pokud země hello je neznámý, pak dva otazníky (tj.)??), se zobrazí.
* Sestavy mohou zahrnovat metriky pro "Evropa" nebo hello "A Asie/Tichomoří Region." Tyto položky nejsou určeny tooprovide statistické informace o všechny IP adresy v těchto oblastech. Místo toho vztahují pouze toorequests, které pocházejí z IP adresy, které jsou umístěny na Evropa nebo Asie/Tichomoří místo tooa konkrétní městě nebo zemi.

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Zde najdete hello celkový počet přístupů, hello Procento přístupů hello množství dat přenesených (v gigabajtech), a hello procento data přenesená pro hello nejvyšší 250 oblasti. Zobrazte popis pro každou z těchto metriky.

Oba typy sestav níže je uveden stručný popis.

| Název sestavy | Popis |
| --- | --- |
| Horní města |Tato sestava určuje pořadí města podle toohello počet přístupů, které pochází z této oblasti. |
| Horní zemích |Tato sestava určuje pořadí zemích podle toohello počet přístupů, které pochází z této oblasti. |

## <a name="daily-summary"></a>Denní souhrn
Hello denní Souhrnná sestava vám umožní tooview hello celkový počet přístupů a data přenesená v konkrétní platformu každý den. Tyto informace můžete použít tooquickly rozpoznat vzory aktivity CDN. Například tato sestava vám může pomoci zjistit, které dny zkušeného vyšší nebo nižší než očekávaném provozu.

Při generování tento typ sestavy, pruhový graf se poskytují vizuální označení jako toohello množství specifické pro platformu vyžádání zkušeného denně přes hello časové období, které jsou předmětem hello sestavy. Bude tak učinit zobrazením panelu pro každý den v sestavě hello. Například výběr hello časové období názvem "Poslední týden" vytvoří pruhový graf s sedm řádky. Každý pruh označí hello celkový počet přístupů došlo v daný den.

Hello levé straně hello grafu (na ose y) určuje, kolik přístupů došlo k chybě na hello zadané datum. Přímo pod hello grafu (na ose x), najdou se popisek, který označuje datum hello (formát: rrrr-MM-DD) pro každý den obsažených v sestavě hello.

> [!TIP]
> Pokud se ukazatel myši přesunete panelu, zobrazí se jako popisek hello celkový počet přístupů, které došlo k tomuto datu.
> 
> 

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Existuje najdete hello celkový počet přístupů a hello množství dat přenesených (v gigabajtech) pro každý den hello zpráva vztahuje.

## <a name="by-hour"></a>Za hodinu
Hello podle hodinu sestava vám umožní tooview hello celkový počet přístupů a data přenesená v konkrétní platformu hodinu. Tyto informace můžete použít tooquickly rozpoznat vzory aktivity CDN. Například tato sestava vám může pomoci odhalit hello časových období dni Dobrý den, které zaznamenat vyšší nebo nižší, než očekávaném provozu.

Při generování tento typ sestavy, bude pruhový graf poskytuje vizuální znázornění jako toohello množství specifické pro platformu vyžádání došlo hodinu přes hello časové období, které jsou předmětem hello sestavy. Bude tak učinit zobrazením panelu pro každou hodinu hello zpráva vztahuje. Například výběr 24 hodin časové období se vygeneruje pruhový graf s pruhy dvacet čtyři. Každý pruh označí hello celkový počet přístupů během tohoto hodinu.

levé straně Hello hello grafu (na ose y) určuje, kolik přístupů došlo k chybě na určenou hodinu hello. Přímo pod hello grafu (na ose x), zjistíte, popisek, který označuje hello data a času (formát: rrrr-MM-DD hh: mm) pro každou hodinu obsažených v sestavě hello. Čas údajně formátu 24 hodin a není zadán pomocí časové pásmo UTC nebo GMT hello.

> [!TIP]
> Pokud se ukazatel myši přesunete panelu, zobrazí se jako popisek hello celkový počet přístupů, které došlo k chybě danou dobu.
> 
> 

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Existuje najdete hello celkový počet přístupů a hello množství dat přenesených (v gigabajtech) pro každou hodinu hello zpráva vztahuje.

## <a name="by-file"></a>Pomocí souboru
Hello po souboru sestava umožňuje jste tooview hello objem vyžádání a hello provozu, které vznikly v průběhu konkrétní platformu pro hello nejvíce požadované prostředky. Při generování tento typ sestavy, pruhový graf vygeneruje na hello top 10 nejžádanějších prostředky přes hello zadané časové období.

> [!NOTE]
> Pro účely hello této sestavy se adresy URL CNAME edge převedený tootheir ekvivalentní CDN adresy URL. To umožňuje že přesné tally hello celkový počet přístupů k přidružený prostředek bez ohledu na hello CDN nebo hraniční CNAME adresa URL použitá toorequest ho.
> 
> 

levé straně Hello hello grafu (na ose y) označuje hello počet požadavků pro každý prostředek přes hello zadané časové období. Přímo pod hello grafu (na ose x) zjistí štítek, který označuje hello název souboru pro každou hello top 10 požadované prostředky.

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Zde najdete následující informace pro každý hello nejvyšší 250 požadovaných prostředků hello: relativní cestu, hello celkový počet přístupů, hello Procento přístupů, hello množství dat přenesených (v gigabajtech) a hello procento data přenesená.

## <a name="by-file-detail"></a>Pomocí souboru podrobností
Hello sestava podle podrobností souboru umožňuje tooview hello objem vyžádání a hello provozu, které vznikly v průběhu konkrétní platformu pro konkrétní prostředek. V hello velmi horní části této sestavy je hello souboru podrobnosti pro možnost. Tato možnost poskytuje seznam vaše nejžádanějších prostředky na vybranou platformu hello. V pořadí toogenerate podle podrobností souboru sestavy budete potřebovat tooselect hello požadovaného assetu z hello souboru podrobnosti pro možnost. A pruhový graf označí hello množství denní vyžádání generovaný přes hello zadané časové období.

levé straně Hello hello grafu (na ose y) označuje hello celkový počet požadavků, že prostředek došlo v určitý den. Přímo pod hello grafu (na ose x), najdou se popisek, který označuje datum hello (formát: rrrr-MM-DD) pro které vyžádání CDN pro hello asset ohlásil.

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Existuje najdete hello celkový počet přístupů a hello množství dat přenesených (v gigabajtech) pro každý den hello zpráva vztahuje.

## <a name="by-file-type"></a>Podle typu souboru
Hello sestavy podle typu souboru umožňuje vám tooview hello objem vyžádání a hello provozu způsobené typ souboru. Při generování tento typ sestavy, označí graf prstenec hello Procento přístupů generovaných hello typy top 10 souborů.

> [!TIP]
> Pokud se ukazatel myši přesunete na výseč hello prstenec grafu, hello Internet média typ, typ souboru se zobrazí jako popisek.
> 
> 

Hello data, která byla použité toogenerate hello prstenec grafu lze zobrazit pod ním. Existuje se najít typ média hello souboru název rozšíření nebo Internet, hello celkový počet přístupů, hello Procento přístupů, hello množství dat přenesených (v gigabajtech) a hello procento data přenesená pro každou hello top 250 typy souborů.

## <a name="by-directory"></a>Pomocí adresáře
Sestava podle Directory Hello vám umožní tooview hello objem vyžádání a hello provozu, které vznikly v průběhu konkrétní platformu pro obsah z určitého adresáře. Při generování tento typ sestavy, označí pruhový graf hello celkový počet přístupů generovaných obsah hello top 10 adresáře.

### <a name="using-hello-bar-chart"></a>Pomocí hello pruhový graf
* Najeďte myší panelu tooview hello relativní cestu toohello odpovídající adresář.
* Obsah uložený v podsložce adresáře nepočítá při výpočtu požadavků službou Active directory. Tento výpočet využívá výhradně hello počet požadavků vygenerované obsah uložený v adresáři skutečné hello.
* Pro účely hello této sestavy se adresy URL CNAME edge převedený tootheir ekvivalentní CDN adresy URL. Díky tomu, že že přesné tally pro všechny statistické údaje o přidružený prostředek bez ohledu na hello CDN nebo hraniční CNAME adresa URL použitá toorequest ho.

Hello levé straně hello grafu (na ose y) označuje hello celkový počet požadavků pro hello obsah uložený v top 10 adresářů. Každý pruh v grafu hello představuje adresář. Použití hello barevné kódování toomatch schéma se panelu tooa directory uvedené v části horní 250 úplné adresáře hello.

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Zde najdete následující informace pro každý z adresáře horní 250 hello hello: relativní cestu, hello celkový počet přístupů, hello Procento přístupů, hello množství dat přenesených (v gigabajtech) a hello procento data přenesená.

## <a name="by-browser"></a>Pomocí prohlížeče
Hello pomocí prohlížeče sestav umožňuje tooview prohlížečů, které byly použité toorequest obsahu. Při generování tento typ sestavy, označí výsečového grafu hello procento žádostí zpracovávaných hello top 10 prohlížeče.

### <a name="using-hello-pie-chart"></a>Pomocí hello výsečového grafu
* Najeďte myší na výseč hello výsečového grafu tooview prohlížeče název a verze.
* Pro účely hello této sestavy považuje za každé kombinaci jedinečný verze prohlížeče nebo jiném prohlížeči.
* řez Hello názvem "Ostatní" označuje hello procento žádostí, které zpracovávají všechny ostatní prohlížeče a verze.

Hello data, která byla použité toogenerate hello výsečového grafu lze zobrazit pod ním. Zde najdete hello prohlížeče typu nebo číslo verze, hello celkový počet přístupů a hello Procento přístupů pro každou hello nejvyšší 250 prohlížečů.

## <a name="by-referrer"></a>Podle odkazující server
Hello podle odkazující server sestav umožňuje tooview hello Nejčastější odkazující servery toocontent na vybranou platformu hello. Odkazující server označuje hello název hostitele, ze které byla vygenerována žádost. Při generování tento typ sestavy, bude znamenat pruhový graf hello množství vyžádání (tj, přístupů) generované hello top 10 odkazující servery.

levé straně Hello hello grafu (na ose y) označuje hello celkový počet požadavků, že prostředek došlo odkazující na každý server. Každý pruh v grafu hello představuje odkazující server. Použití hello barevné kódování toomatch schéma se panelu odkazující server tooa uvedené v části odkazující horní 250 server hello.

Hello data, která byla použité toogenerate hello pruhový graf lze zobrazit pod ním. Zde najdete hello adresu URL, hello celkový počet přístupů a hello Procento přístupů generované z každé hello nejčastější 250 odkazující servery.

## <a name="by-download"></a>Můžete si je stáhnout
Hello ve stahovat sestava vám umožní tooanalyze stažení vzory pro nejvíce požadovaný obsah. Hello horní části sestavy hello obsahuje pruhový graf, porovná se pokusil stahování s dokončené stahování pro hello top 10 požadované prostředky. Každý panelu je barevné kódování podle toowhether, je pokusu o stažení (modré) nebo dokončené stahování (zelený).

> [!NOTE]
> Pro účely hello této sestavy se adresy URL CNAME edge převedený tootheir ekvivalentní CDN adresy URL. Díky tomu, že že přesné tally pro všechny statistické údaje o přidružený prostředek bez ohledu na hello CDN nebo hraniční CNAME adresa URL použitá toorequest ho.
> 
> 

levé straně Hello hello grafu (na ose y) označuje hello název souboru pro každou hello top 10 požadované prostředky. Přímo pod hello grafu (na ose x) zjistíte, popisky, které indikují hello celkový počet souborů ke stažení, pokus o dokončení.

Přímo pod hello pruhový graf, hello následující informace budou uvedené pro hello nejvyšší 250 požadovaných prostředků: relativní cesta (včetně název souboru), hello počet pokusů, které bylo stažené toocompletion hello počet pokusů, které byla vyžádána a hello Procento požadavků, jejichž výsledkem dokončení stahování.

> [!TIP]
> Naše CDN není informována klientem HTTP (tj. prohlížeč) Pokud byl prostředek zcela stažen. V důsledku toho máme toocalculate, zda prostředek úplném stažení podle toostatus kódy a žádosti o rozsah bajtů. Hello thing nejprve vyhledáme při provádění tohoto výpočtu je, zda text hello zadání požadavku 200 OK stavový kód. Pokud ano, pak podíváme na tooensure žádosti o rozsah bajtů, aby pokrývaly hello celý asset. Nakonec jsme porovnat hello množství dat přenesených toohello velikost hello požadovaný prostředek. Pokud přenesená hello data je větší než velikost souboru hello rovna tooor a žádosti o rozsah bajtů hello jsou vhodné pro tento prostředek, pak stiskněte klávesu hello se budou počítat jako dokončení stahování.
> 
> Z důvodu toohello interpretive povaha této sestavy byste měli mít na paměti hello následující body, které mohou změnit hello konzistence a přesnost této sestavy.
> 
> * Vzory přenosů dat nelze přesně předpovědět, když Uživatelští agenti chovat jinak. To může vytvořit výsledky dokončené stahování, které jsou větší než 100 %.
> * Prostředky, které budou využívat protokol HTTP progresivní stahování nemusí přesně v této sestavě. Toto je z důvodu toousers požádat o toodifferent pozice v videa.
> 
> 

## <a name="by-404-errors"></a>Podle chyb 404
Hello podle chyb 404 sestava vám umožní tooidentify hello typ obsahu, který generuje hello nejvíce počet 404 stavové kódy nebyl nalezen. Hello horní části sestavy hello obsahuje pruhový graf u hello prvních 10 prostředků, pro které byl vrácen stavový kód 404 nebyl nalezen. Tento pruhový graf porovnává hello celkový počet požadavků s požadavky, jejichž výsledkem 404 nebyl nalezen stavový kód pro tyto prostředky. Každý panelu jsou rozlišené barevně. Žlutý pruh slouží způsobil tooindicate, který hello požadavek stavový kód 404 nebyl nalezen. Červený pruh se používá tooindicate hello celkový počet požadavků pro prostředek hello.

> [!NOTE]
> Pro účely hello této sestavy vezměte na vědomí následující hello:
> 
> * Stiskněte klávesu představuje každá žádost o prostředek bez ohledu na to, stavový kód.
> * Adresy URL CNAME hraniční jsou převedené tootheir ekvivalentní adresy URL CDN. Díky tomu, že že přesné tally pro všechny statistické údaje o přidružený prostředek bez ohledu na hello CDN nebo hraniční CNAME adresa URL použitá toorequest ho.
> 
> 

levé straně Hello hello grafu (na ose y) označuje hello název souboru pro každou hello top 10 požadovaných prostředků, jejichž výsledkem stavový kód 404 nebyl nalezen. Přímo pod hello grafu (na ose x) zjistíte, popisky, které označují hello celkový počet požadavků a hello počet požadavků, jejichž výsledkem stavový kód 404 nebyl nalezen.

Přímo pod hello pruhový graf, hello následující informace budou uvedené pro hello nejvyšší 250 požadovaných prostředků: hello počet požadavků, jejichž výsledkem 404 nebyl nalezen stavový kód, celkový počet pokusů, které hello asset hello byl relativní cesta (včetně název souboru), požadované a hello procento žádostí, jejichž výsledkem stavový kód 404 nebyl nalezen.

## <a name="see-also"></a>Viz také
* [Přehled Azure CDN](cdn-overview.md)
* [Statistiky v reálném čase v Microsoft Azure CDN](cdn-real-time-stats.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
* [Analýza výkonu Edge](cdn-edge-performance.md)

