---
title: "aaaAnalyzing trendy v sadě Visual Studio | Microsoft Docs"
description: "Analyzujte, vizualizujte a zkoumejte trendy v telemetrii služby Application Insights v sadě Visual Studio."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 3150c6fc-2691-44f6-a290-fc5cd68e692a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: 5c623ec040363f05e80ca927dc8855eb016adc99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-trends-in-visual-studio"></a>Analýza trendů v sadě Visual Studio
Nástroj trendy Application Insights Hello vizualizuje, jak důležité telemetrické události webové aplikace v průběhu času mění, dokážete rychle identifikovat problémy a anomálií. Pomocí propojení můžete toomore podrobné diagnostické informace, trendy vám může pomoct zlepšit výkon vaší aplikace, sledovat hello příčinami výjimek a odkrýt přehledy z vlastních událostí.

![Příklad okna nástroje Trendy](./media/app-insights-visual-studio-trends/app-insights-trends-hero-750.png)

## <a name="configure-your-web-app-for-application-insights"></a>Konfigurace webové aplikace pro Application Insights

Pokud jste to ještě neudělali, [nakonfigurujte svou webovou aplikaci pro Application Insights](app-insights-overview.md). To umožňuje, je portál Application Insights toohello toosend telemetrie. Nástroj trendy Hello přečte hello telemetrie z ní.

Nástroj Trendy Application Insights je dostupný v sadě Visual Studio 2015 s aktualizací Update 3 a vyšší.

## <a name="open-application-insights-trends"></a>Otevření nástroje Trendy Application Insights
okno trendy Application Insights tooopen hello:

* Pomocí tlačítka panelu nástrojů Application Insights hello, zvolte **prozkoumat trendy Telemetrie**, nebo
* Hello projektu místní nabídce vyberte **Application Insights > prozkoumat trendy Telemetrie**, nebo
* V řádku nabídek hello Visual Studio, vyberte příkaz **zobrazení > ostatní okna > trendy Application Insights**.

Může se zobrazit výzva tooselect prostředku. Klikněte na tlačítko **vyberte prostředek**, přihlaste se pomocí předplatného Azure, a potom vyberte prostředek Application Insights hello seznamu, pro kterou chcete tooanalyze telemetrie trendy.

## <a name="choose-a-trend-analysis"></a>Výběr analýzy trendů
![Nabídka běžných typů analýz trendů](./media/app-insights-visual-studio-trends/app-insights-trends-1-750.png)

Začínáme se tak, že zvolíte jednu z pěti běžné analýzy trendů, každý analýza dat z hello posledních 24 hodin:

* **Prozkoumat problémy s výkonem se vaše požadavky serveru** -vytvářeny požadavky služby tooyour, seskupené podle doby odezvy
* **Analýza chyb ve své žádosti serveru** -vytvářeny tooyour služby, seskupené podle kódu odpovědi HTTP požadavky
* **Zjištění výjimek hello ve vaší aplikaci** -výjimky z vaší služby, seskupené podle typ výjimky
* **Kontrola výkonu hello závislostí vaší aplikace** -služby s názvem vaší službou seskupené podle doby odezvy
* **Zkontrolovat vlastní události** – vlastní události, které jste pro svoji službu nastavili, seskupené podle typu události.

Předem předdefinované analýzy jsou k dispozici později z hello **zobrazit běžné typy telemetrie analýzy** tlačítka na hello levého horního rohu okna trendy hello.

## <a name="visualize-trends-in-your-application"></a>Vizualizace trendů v aplikaci
Nástroj Trendy Application Insights pracuje s telemetrií vaší aplikace a vizualizuje z ní časové řady. Každá vizualizace časové řady zobrazuje jeden typ telemetrie (seskupený podle jedné vlastnosti takové telemetrie) za určité časové období. Například můžete požadavků na server tooview seskupené podle hello země, ze kterého budou pochází, přes hello posledních 24 hodin. V tomto příkladu by každý bublin na hello vizualizace představují počet hello požadavky serveru pro některé země nebo oblast během jedné hodiny.

Pomocí ovládacích prvků hello hello horní části okna tooadjust hello jaké typy telemetrických dat zobrazení. Nejdřív vyberte hello telemetrie typy, které byste chtěli:

* **Typ telemetrie** – požadavky serveru, výjimky, závislosti nebo vlastní události.
* **Časové rozmezí** – kdekoli z hello posledních 30 minut toohello poslední 3 dny.
* **Seskupit podle** – typ výjimky, ID problému, země/oblast a další.

Potom klikněte na **analyzovat Telemetrická** toorun hello dotazu.

toonavigate mezi bublinách v hello vizualizaci:

* Klikněte na tlačítko tooselect bubliny, která aktualizuje hello filtry dolnímu hello okna hello shrnutí jenom hello událostí, které nastaly v konkrétním časovém období
* Dvakrát klikněte na nástroj bublin toonavigate toohello hledání a zobrazit všechny hello jednotlivých telemetrické události, k nimž došlo během tohoto časového období
* CTRL + kliknutí bublin toode vyberte ho v hello vizualizace.

> [!TIP]
> Hello trendy a hledání nástroje spolupracují toohelp přesné hello požadovaného problémy ve službě mezi tisíce telemetrické události. Pokud si vaši zákazníci například jedno odpoledne všimnou, že je odezva aplikace pomalejší, spusťte nástroj Trendy. Analýza požadavky služby tooyour přes hello několika posledních hodinách, seskupené podle doby odezvy. Zjistěte, jestli se neobjevil neobvykle velký cluster pomalých požadavků. Dvakrát klikněte tohoto bublin toogo toohello vyhledávání nástroje, události filtrovaný toothose žádostí. Z hledání, vám umožní zkoumat hello obsah těchto požadavků a přejděte toohello kód potřebný tooresolve hello problém.
> 
> 

## <a name="filter"></a>Filtr
Zjistit konkrétnější trendy s ovládacími prvky filtru hello v hello dolní části okna hello. tooapply filtr, klikněte na jeho název. Můžete rychle přepínat mezi trendy toodiscover různých filtrů, které může být skrytí v konkrétní dimenzi vaší telemetrie. Pokud použijete filtr ve funkci jednou dimenzí, jako je typ výjimky, zůstanou filtry v ostatních dimenzí můžete kliknout, i když se zobrazí šedě. tooun-použít filtr, klikněte na znovu. CTRL + kliknutí tooselect více filtrů v hello stejné dimenze.

![Filtry trendů](./media/app-insights-visual-studio-trends/TrendsFiltering-750.png)

Co dělat, když chcete tooapply více filtrů? 

1. Použijte první filtr hello. 
2. Klikněte na tlačítko hello **použít vybrané filtry a dotazy znovu** tlačítko podle názvu hello hello dimenze první filtru. To bude dotaz znovu telemetrii pouze události, které odpovídají filtru první hello. 
3. Použijte druhý filtr. 
4. Opakujte hello proces toofind trendy v konkrétní podmnožiny telemetrie. Například požadavky serveru s názvem „GET Home/Index“ *a* ty, které přišly z Německa *a* získaly stavový kód 500. 

tooun-použít jednu z těchto filtry, klikněte na hello **odebrat vybrané filtry a dotazy znovu** tlačítko pro dimenzi hello.

![Několik filtrů](./media/app-insights-visual-studio-trends/TrendsFiltering2-750.png)

## <a name="find-anomalies"></a>Nalezení anomálií
Nástroj trendy Hello můžete zvýraznit bublinách události, které jsou neobvyklé porovnání tooother bublinách v hello stejné časové řady. V rozevírací nabídce hello typ zobrazení, zvolte **spočítá v sady času (zvýraznění anomálií)** nebo **procenta v sady času (zvýraznění anomálií)**. Červené bubliny označují anomálie. Anomálie jsou definovány jako bubliny s počty/procenta přesahující 2.1 časy hello směrodatnou odchylku hello počty nebo procentuální hodnoty, které došlo k chybě v hello po dvou časových období (48 hodin, pokud zobrazíte hello posledních 24 hodin, atd.).

![Barevné tečky označují anomálie.](./media/app-insights-visual-studio-trends/TrendsAnomalies-750.png)

> [!TIP]
> Zvýraznění anomálií může být zvláště užitečné při hledání mimořádných hodnot v časových řadách malých bublin, u kterých by se jinak mohlo zdát, že mají podobnou velikost.  
> 
> 

## <a name="next"></a>Další kroky
|  |  |
| --- | --- |
| **[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Hledejte telemetrii, zobrazujte data v CodeLens a konfigurujte Application Insights. Vše v sadě Visual Studio. |![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, vyhledávání](./media/app-insights-visual-studio-trends/34.png) |
| **[Přidání dalších dat](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |![Visual Studio](./media/app-insights-visual-studio-trends/64.png) |
| **[Práce s Application Insights portál hello](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/app-insights-visual-studio-trends/62.png) |

