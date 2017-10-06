---
title: "aaaSmart diagnostiku změny výkonu webové aplikace ve službě Azure Application Insights | Microsoft Docs"
description: "Automatickou diagnostiku špičky nebo kroky v výkonu telemetrie z vaší webové aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 8891762c4a4bfdb08b647fe3b702349eb30ec9c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Diagnostika nečekané změny v telemetrii aplikace

*Tato funkce je ve verzi preview.*

Diagnostika nečekané změny ve vaší webové aplikace výkonu a využití s jedním kliknutím! Hello inteligentní diagnostiky funkce je k dispozici vždy, když vytváříte graf doby zpracování v [Analytics](app-insights-analytics.md) v [Application Insights](app-insights-overview.md). Vždy, když dojde neobvyklou změnu z hello trend výsledků, například Špička nebo dip, identifikuje inteligentní diagnostiky vzor dimenzí a související hodnoty, které mohou vysvětlit hello změnu. Díky tomu můžete rychle diagnostikovat problém hello. 

V tomto příkladu inteligentní diagnostiky nalezen se vzor hodnot vlastností, které jsou přidružené k hello změnu a zvýrazňuje hello rozdíl mezi výsledky a bez tohoto vzoru:

![Příklad analytics diagnostiky výsledků](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Diagnostika změny dat

1.  Spuštění dotazu v analýzy a vykreslit ho jako graf doby zpracování. 
2.  Klikněte na libovolného bodu zvýrazněné ve špičce, pokud existuje.
 
    ![bod ve špičce](./media/app-insights-analytics-diagnostics/peak.png)

    Diagnostika trvá několik sekund toodiscover vzor.

3. Hello diagnostiky výsledků kartě se zobrazují vzor, který může vysvětlují nespojitost vaše data.

    ![výsledek](./media/app-insights-analytics-diagnostics/result.png)
 
    Hello text znázorňuje hodnoty hello dimenze, které se zobrazují toocorrelate s hello shift. V tomto příkladu je spojen s určité žádosti a verzí určitého prohlížeče.

    Všimněte si také hello dvě součásti hello grafu s hello filtru true a false. false součást Hello ukazuje beze změny trendu. Jinými slovy se nezměnila ve výsledcích telemetrie hello, pokud jsou vyloučeny hello problematické kombinace dimenzí, které má identifikovat diagnostiky. Naopak hello výsledky v rámci této kombinace zobrazit výrazné změny v rámci oblasti hello zvýrazněná šetření. Ukazuje to, že diagnostiky nalezl kombinace vlastnosti, které popisuje změnu hello.

4.  Pokud vzor hello je složité, je třeba toohover přes **Zobrazit vše** toosee hello dimenzí.

    ![zobrazit vše](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  V případě, že diagnostiky vyhledá žádné významné vzor toonotify o hello, které se zobrazí stránka žádné výsledky. V tomto okamžiku můžete změnit svůj dotaz. Například můžete zúžit hello časové rozmezí a přihrádkování v dotazu Analytics pro další analýzu a potenciálně lepší výsledky.

Díky znalosti hello že určitou stránku vašeho webu došlo k problému v určitého prohlížeče, můžete nyní přejděte rovnou toohello problém stránku a prozkoumat nedávné změny.

## <a name="try-hello-demo"></a>Zkuste ukázku hello

[Kliknutím sem toosee ukázka](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) na ukázková data.

## <a name="how-it-works"></a>Jak to funguje

Inteligentní diagnostiky používá nepodporovaný algoritmus pokročilé bez dohledu machine learning podle hello [DiffPatterns](app-insights-analytics-reference.md) operaci. Vypadá to candidate vzorů, které mohou vysvětlit, změny dat hello. Analyzuje hello dopad kandidáti na hello metrika a zobrazuje hello vzor, nejlépe koreluje s hello změnu.

## <a name="no-diagnostic-points"></a>Žádné diagnostické body?

Inteligentní diagnostiky funguje pouze pokud jsou splněny hello následující kritéria:

 * Inteligentní nastavení diagnostiky je zapnutá. Podívejte se do části ikonu hello nastavení v Analytics.
 * je vybrán Hello inteligentní diagnostiky možnost v nastavení analýzy. 
 * Časová osa: hello osy x grafu hello musí být typu `datetime`.
 * Řádek nebo oblast grafu: Diagnostics lze použít pouze tyto typy grafu. Použití `| render timechart` nebo `| render areachart` na konci hello tohoto dotazu; nebo vyberte řádek nebo plošný graf z rozevíracího seznamu pro výběr hello.
 * Nespojitost: Musí být významné nespojitost v datech hello.
 * Tooanalyze dostatek bodů.
 * Více než jeden shrnují klauzuli hello dotazu.
 * Žádné klauzule projektu, který obsahuje název definice před hello shrnují klauzule.

 
 ## <a name="related-articles"></a>Související články

 * [Analýza kurzu](app-insights-analytics-tour.md)
 * [Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky upozorňuje tooperformance problémy.
