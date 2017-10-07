---
title: "aaaSmart detekce - selhání anomálie ve službě Application Insights | Microsoft Docs"
description: "Upozorňuje toounusual změny v hello počet neúspěšných požadavků tooyour webové aplikace a obsahuje diagnostiky analýzu. Není nutné konfigurovat."
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: bwren
ms.openlocfilehash: dfd178ca9546294be91f27b0c1b846d519539b77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---failure-anomalies"></a>Inteligentní detekce - anomálií selhání
[Application Insights](app-insights-overview.md) automaticky vás upozorní skoro v reálném čase, pokud dojde neobvyklé zvýšení hello počet neúspěšných žádostí vaší webové aplikace. Zjistí neobvyklého nárůstu hello počet požadavků HTTP nebo závislostí volání, které jsou hlášeny jako se nezdařilo. Pro žádosti neúspěšné požadavky jsou obvykle s kódy odpovědí 400 nebo vyšší. toohelp rychlou kontrolu a diagnostikovat problém hello analýzu hello vlastnosti hello selhání a související telemetrii je součástí hello oznámení. Existují také odkazy toohello portál Application Insights pro další diagnostiku. Hello musí funkce žádné nastavení ani konfigurace, protože využívá strojové učení míra normální selhání hello toopredict algoritmy.

Tato funkce funguje pro Java a ASP.NET webové aplikace hostované v cloudu hello nebo na serverech. Taky funguje pro každou aplikaci, která generuje telemetrická žádost nebo závislost – například pokud máte role pracovního procesu, který volá [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) nebo [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency).

Po nastavení [Application Insights pro svůj projekt](app-insights-overview.md), a pokud vaše aplikace generuje určité minimální množství telemetrie, inteligentní detekce anomálií selhání trvá 24 hodin toolearn hello normální chování vaší aplikace, než ho je zapnutá a může odesílat výstrahy.

Zde je ukázka výstraha.

![Ukázka inteligentní detekce výstraha zobrazující clusteru analysis kolem selhání](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Ve výchozím nastavení zobrazí kratší pošty formátu než v tomto příkladu. Ale můžete [přepínač toothis podrobném formátu](#configure-alerts).
>
>

Všimněte si, že se dozvíte:

* míra selhání Hello porovná toonormal chování aplikace.
* Počet uživatelů, kteří jsou vliv – abyste věděli, kolik tooworry.
* Charakteristik vzor spojené s chybami hello. V tomto příkladu je kód konkrétní odpovědi, název požadavku (operace) a verzí v aplikaci. Které okamžitě sděluje, kde toostart vyhledávání v kódu. Další možnosti může být konkrétní operační systém prohlížeče nebo klienta.
* Hello výjimce, trasování protokolu a selhání závislosti (databáze nebo dalších externích součástí), které zobrazí toobe přidružené hello rozdělení selhání.
* Propojí přímo toorelevant hledání na hello telemetrii ve službě Application Insights.

## <a name="benefits-of-smart-detection"></a>Výhody Inteligentní detekce
Obyčejnou [metriky výstrahy](app-insights-alerts.md) zjistíte, může se jednat o problém. Ale inteligentní zjišťování spustí hello diagnostiky pracovní pro vás, provádění spoustu hello analýzy, že byste jinak museli toodo sami. Získáte výsledky hello přehledně zabalené, že vám pomůžeme tooget rychle toohello kořenové hello problému.

## <a name="how-it-works"></a>Jak to funguje
Inteligentní detekce monitoruje hello telemetrie získané z vaší aplikace a v konkrétní hello selhání sazby. Toto pravidlo počty hello počet požadavků, pro které hello `Successful request` vlastnost má hodnotu false a hello počet závislostí volání pro které hello `Successful call` vlastnost je false. Pro žádosti, ve výchozím nastavení `Successful request == (resultCode < 400)` (Pokud jste napsali vlastní kód příliš[filtru](app-insights-api-filtering-sampling.md#filtering) nebo vygenerování vlastního [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest) volání). 

Výkon vaší aplikace má typický vzor chování. Některé požadavků nebo závislostí volání budou náchylnější toofailure než jiné; a hello celkové míra selhání může se stát, zatížením. Inteligentní detekce využívá strojové učení toofind tyto anomálií.

Jako telemetrie dodává do Application Insights z vaší webové aplikace, inteligentní detekce porovná aktuální chování hello hello vzory vidět přes hello posledních několik dnů. Pokud neobvyklý růst v míra selhání pozorovanou porovnáním s předchozí výkonu, analýzu se aktivuje.

Když se aktivuje analýzu hello služby clusteru analysis provádí hello chybných požadavků, tootry tooidentify vzorek hodnot, které charakterizovat hello selhání. V předchozím příkladu hello analýzy hello zjistila, že většina selhání jsou o konkrétní výsledný kód, název požadavku, hostitele adresy URL serveru a role instance. Naopak hello analysis zjistí hello vlastnosti operačního systému klienta v průběhu více hodnot, a proto není uveden.

Když služby je instrumentována pomocí těchto volání telemetrie, hello analyzátoru hledá výjimku a selhání závislostí, které jsou přidružené požadavky hello cluster, ve kterém má identifikuje, společně s příklady všech protokolů trasování, které jsou spojené s těmi požadavky.

Výsledný analysis Hello se odešle tooyou jako výstrahu, pokud jste ji nakonfigurovali nikoli k.

Jako hello [výstrahy, můžete nastavit ručně](app-insights-alerts.md), můžou kontrolovat stav hello hello výstrahy a jeho konfiguraci v hello výstrahy okno prostředku Application Insights. Ale na rozdíl od ostatních výstrah, není nutné tooset nahoru nebo nakonfigurovat Inteligentní detekce. Pokud chcete, můžete zakázat nebo změnit jeho cíl e-mailové adresy.

## <a name="configure-alerts"></a>Konfigurace výstrah
Můžete zakázat Inteligentní detekce, změnit hello příjemců e-mailu, vytvořit webhook, jehož nebo vyjádřit výslovný souhlas toomore podrobné zprávy upozornění.

Otevřete stránku hello výstrahy. Selhání anomálie se dodává spolu s všechny výstrahy, které jste si nastavili ručně, a zobrazí se, zda je momentálně ve stavu výstrahy hello.

![Na stránce Přehled hello klikněte na dlaždici výstrahy. Nebo na libovolné stránce metriky, klikněte na tlačítko výstrahy.](./media/app-insights-proactive-failure-diagnostics/021.png)

Klikněte na výstrahu tooconfigure hello ho.

![Konfigurace](./media/app-insights-proactive-failure-diagnostics/032.png)

Všimněte si, že můžete zakázat Inteligentní detekce, ale nelze ho proto odstranit (nebo vytvořte jiný).

#### <a name="detailed-alerts"></a>Podrobné výstrahy
Pokud zvolíte možnost "Získat podrobnější Diagnostika" hello e-mailu, bude obsahovat další diagnostické informace. V některých případech budete moct toodiagnose hello problém právě z hello dat v e-mailu hello.

Je mírné riziko, že hello podrobnější výstrah můžou obsahovat citlivé informace, protože obsahuje výjimku a trasování zpráv. Však tomu by mohlo dojít pouze pokud váš kód by se mohl citlivých informací do těchto zpráv.

## <a name="triaging-and-diagnosing-an-alert"></a>Triaging a diagnostice výstrahu
Výstraha naznačuje, že byl zjištěn neobvyklý růst v hello frekvence neúspěšných požadavků. Je pravděpodobné, že je nějaký problém s vaší aplikace nebo jeho prostředí.

Z hello procento požadavky a počet ovlivněných uživatelů, můžete rozhodnout, jak naléhavě vyřešit určitý problém hello je. V předchozím příkladu hello míra selhání hello 22,5 % porovná s normální rychlosti % 1, znamená, že něco chybný se děje. Na dobrý den druhé straně, situace měla vliv na uživatele pouze 11. Pokud by měla aplikace, bude jak závažná možné tooassess, který je.

V mnoha případech bude možné toodiagnose hello problém rychle z hello žádosti o název, výjimky, závislost selhání a trasování data poskytnutá.

Existují některé další různá vodítka. Míra selhání hello závislostí v tomto příkladu je hello například stejné jako hello výjimka rychlost (89.3 %). To naznačuje, že nastane hello výjimka přímo z chyby závislosti hello - budete jasno, kde toostart vyhledávání v kódu.

tooinvestigate navíc hello odkazy v každé části přejdete přímých tooa [stránky hledání](app-insights-diagnostic-search.md) filtrované toohello příslušné požadavky, výjimky, závislostí nebo trasování. Nebo můžete otevřít hello [portál Azure](https://portal.azure.com), přejděte toohello prostředek Application Insights pro vaši aplikaci a otevřete okno selhání hello.

V tomto příkladu kliknutím na odkaz hello 'zobrazit závislosti selhání podrobnosti' otevře okno hledání Application Insights hello. Zobrazuje hello SQL příkaz, který obsahuje příklad hello hlavní příčina: hodnoty Null byly poskytnuty v povinná pole a neprošel ověřením platnosti během hello operace uložení.

![Diagnostické vyhledávání](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Nedávné výstrahy můžete zkontrolovat

Klikněte na tlačítko **Inteligentní detekce** tooget toohello poslední výstrahy:

![Souhrn výstrah](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-hello-difference-"></a>Jaký je rozdíl hello...
Inteligentní detekce anomálií selhání doplňuje jiné podobné avšak odlišné funkce Application Insights.

* [Metriky výstrahy](app-insights-alerts.md) jsou nastavené sami a můžete monitorovat řadu metrik, jako je například obsazení procesoru, požadavků, časů načtení stránky a tak dále. Můžete je použít toowarn vás, například, pokud potřebujete tooadd další prostředky. Naopak Inteligentní detekce anomálií selhání obsahuje malé rozsah kritické metriky (aktuálně pouze chybných požadavků rychlost), určené toonotify, je téměř v reálném čase způsobem po významně zvyšuje rychlost neúspěšných požadavků vaší webové aplikace ve srovnání tooweb aplikace normální chování.

    Inteligentní detekce automaticky upraví jeho prahovou hodnotu v odpovědi tooprevailing podmínky.

    Inteligentní zjišťování spustí hello diagnostické práce pro uživatele.
* [Inteligentní detekce anomálií výkonu](app-insights-proactive-performance-diagnostics.md) také používá počítač intelligence toodiscover neobvyklou vzorců vaše metriky a není nutná žádná konfigurace vy. Ale na rozdíl od Inteligentní detekce anomálií selhání hello účelem Inteligentní detekce anomálií výkonu je toofind segmenty vaší potrubí využití, který může být chybně zpracoval – například podle konkrétní stránky na určitý typ prohlížeče. Analýza Hello se provádí denně a pokud se nenajde žádný výsledek, je pravděpodobně toobe naléhavé mnohem menší než výstrahu. Naopak hello analýza selhání anomálií provádí nepřetržitě na příchozích telemetrických dat, a budete informováni, minut Pokud sazby selhání serveru jsou větší, než se očekávalo.

## <a name="if-you-receive-a-smart-detection-alert"></a>Pokud se zobrazí upozornění na inteligentní detekce
*Proč obdrželi tuto výstrahu?*

* Zjistili jsme neobvyklý růst v neúspěšných požadavků míra porovnání toohello normální účaří hello předcházející období. Po dokončení analýzy selhání hello a související telemetrii myslíme si, že dojde k problému, který by měl vypadat do.

*Znamená hello oznámení, že jsou výborný problém?*

* Pokusíme tooalert na přerušení aplikace nebo snížení výkonu, ale pouze můžete plně pochopit hello sémantiku a hello dopad na hello aplikace nebo uživatele.

*Ano se možnost nepřetržitého podíváte na svá data?*

* Ne. Služba Hello je zcela automatické. Můžete získat pouze hello oznámení. Vaše data jsou [privátní](app-insights-data-retention-privacy.md).

*Jsou toosubscribe toothis výstraha?*

* Ne. Každá aplikace, že odešle požadavek telemetrie má pravidlo výstrahy Inteligentní detekce hello.

*Můžete zrušit nebo dostávat oznámení hello namísto toho odesílána toomy kolegy?*

* Ano, pravidla v výstrah, klikněte na tlačítko hello Inteligentní detekce pravidlo tooconfigure ho. Můžete zakázat hello výstrahy, nebo změnit příjemce pro hello výstrahy.

*Ztrátou hello e-mailu. Kde najdu hello oznámení portálu hello?*

* V protokolech hello aktivity. V Azure otevřete prostředek Application Insights hello pro vaši aplikaci a potom vyberte protokoly aktivity.

*Některé výstrahy hello jsou známé problémy a chcete tooreceive je.*

* Máme potlačení výstrahy na našem nevyřízených položek.

## <a name="next-steps"></a>Další kroky
Tyto diagnostické nástroje můžete zkontrolovat hello telemetrie z vaší aplikace:

* [Metriky explorer](app-insights-metrics-explorer.md)
* [Průzkumník služby Search](app-insights-diagnostic-search.md)
* [Analýza - účinný dotazovací jazyk](app-insights-analytics-tour.md)

Inteligentní detekce jsou zcela automatické. Ale možná byste chtěli tooset si některé další výstrahy?

* [Ručně konfigurované metriky výstrahy](app-insights-alerts.md)
* [Testy dostupnosti webu](app-insights-monitor-web-app-availability.md)
