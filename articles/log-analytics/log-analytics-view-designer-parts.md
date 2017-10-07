---
title: "aaaPart odkaz pro Návrhář zobrazení v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů vám umožní toocreate vlastní zobrazení v konzole hello OMS, které obsahují různé vizualizace dat v úložišti OMS hello. Tento článek obsahuje odkaz hello nastavení pro jednotlivé dostupné toouse hello vizualizace částí do vlastních zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 6a19a451cf4cefd2fa5c94e6f61d812c4f820f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Odkazy na protokol Návrhář zobrazení Analytics vizualizace část
Hello Návrhář zobrazení v analýzy protokolů umožňuje toocreate vlastních zobrazení v konzole OMS hello obsahující různé vizualizace dat z úložiště OMS hello. Tento článek obsahuje odkaz hello nastavení pro jednotlivé dostupné toouse hello vizualizace částí do vlastních zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Zobrazit návrháře](log-analytics-view-designer.md) -přehled hello Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.
* [Odkaz na dlaždici](log-analytics-view-designer-tiles.md) -odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná v hello [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován hello prostoru bude automtically převést.

Hello následující tabulka popisuje různé typy hello dlaždice, které jsou k dispozici v hello Návrhář zobrazení.  Hello části níže popisují každý typ dlaždice v podrobností a jejich vlastnosti.

| Typ zobrazení | Popis |
|:--- |:--- |
| [Seznam dotazů](#list-of-queries-part) |Zobrazí seznam protokolů vyhledávací dotazy.  Hello uživatel kliknutím na každý dotaz toodisplay své výsledky. |
| [Počet & seznamu](#number-amp-list-part) |Hlavička obsahuje jeden číslo zobrazuje počet záznamů z protokolu vyhledávací dotaz.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Dvě čísla & seznamu](#two-numbers-amp-list-part) |Hlavička obsahuje dvě čísla zobrazující počet záznamy ze samostatné protokolu vyhledávací dotazy.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Prstenec & seznamu](#donut-amp-list-part) |Hlavička zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Hello prstenec graficky zobrazí výsledky hello nejvyšší tři záznamů. |
| [Dva časové osy & seznamu](#two-timelines-amp-list-part) |Hlavička zobrazí hello výsledky dotazů dvě protokolu časem jako sloupcové grafy s popisek zobrazení jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Informace o](#information-part) |Záhlaví obsahuje statický text a nepovinný odkaz.  Zobrazí se seznam jedné nebo více položek s statický text a název. |
| [Spojnicový graf, popisku & seznamu](#line-chart-callout-amp-list-part) |Hlavička zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Seznam & Spojnicový graf](#line-chart-amp-list-part) |Hlavička zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Zásobník části grafy řádku](#stack-of-line-charts-part) |Zobrazuje tři samostatné spojnicových grafů s více řad z protokolu dotazu v čase. |

## <a name="list-of-queries-part"></a>Seznam dotazů část
Zobrazí seznam protokolů vyhledávací dotazy.  Hello uživatel kliknutím na každý dotaz toodisplay své výsledky.  Hello zobrazení bude obsahovat jeden dotaz ve výchozím nastavení, a můžete kliknout na **+ dotazu** tooadd další dotazy.

![Seznam zobrazení dotazů](media/log-analytics-view-designer/view-list-queries.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název |Toodisplay textu v horní části hello hello zobrazení. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Předvybrané filtry |Čárkami oddělený seznam tooinclude vlastnosti v podokně levém filtru hello při hello uživatel vybere dotazu. |
| V režimu |Počáteční zobrazení zobrazí, když je vybraný dotaz hello.  Hello uživatel může vybrat všechny dostupné zobrazení po otevření hello dotazu. |
| **Dotazy** | |
| Vyhledávací dotaz. |Toorun dotazu. |
| Popisný název |Popisný název hello dotazu toodisplay toohello uživatele. |

## <a name="number--list-part"></a>Počet & seznam součástí
Hlavička obsahuje jeden číslo zobrazuje počet záznamů z protokolu vyhledávací dotaz.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Seznam zobrazení dotazů](media/log-analytics-view-designer/view-number-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu v horní části hello hello zobrazení. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Název** | |
| Legendy |Toodisplay textu v horní části hello hello hlavičky. |
| Dotaz |Dotaz toorun hello hlavičky.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Hello první dvě vlastnosti pro hello prvních deset ve výsledcích hello budou zobrazeny záznamy.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Řádky se vytvářejí automaticky podle hello relativní hodnota hello číselné sloupce.<br><br>Pomocí příkazu řazení hello v hello dotazu toosort hello záznamy v seznamu hello.  můžete kliknout na Hello uživatele najdete v článku všechny toorun hello dotazování a vrátí všechny záznamy. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Název & hodnota oddělovače |Pokud chcete vlastnost textu hello tooparse do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="two-numbers--list-part"></a>Dvou čísel & část Seznam
Hlavička obsahuje dvě čísla zobrazující počet záznamy ze samostatné protokolu vyhledávací dotazy.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Dvou čísel a zobrazení seznamu](media/log-analytics-view-designer/view-two-numbers-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu v horní části hello hello zobrazení. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Název** | |
| Legendy |Toodisplay textu v horní části hello hello hlavičky. |
| Dotaz |Dotaz toorun hello hlavičky.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Hello první dvě vlastnosti pro hello prvních deset ve výsledcích hello budou zobrazeny záznamy.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Řádky se vytvářejí automaticky podle hello relativní hodnota hello číselné sloupce.<br><br>Pomocí příkazu řazení hello v hello dotazu toosort hello záznamy v seznamu hello.  můžete kliknout na Hello uživatele najdete v článku všechny toorun hello dotazování a vrátí všechny záznamy. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Operace |Operace tooperform pro minigraf hello.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete vlastnost textu hello tooparse do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="donut--list-part"></a>Část prstenec & seznamu
Hlavička zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Hello prstenec graficky zobrazí výsledky hello nejvyšší tři záznamů.

![Zobrazení prstenec & seznamu](media/log-analytics-view-designer/view-donut-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Záhlaví** | |
| Název |Toodisplay textu v horní části hello hello hlavičky. |
| Subtitle |Text toodisplay pod hello nadpis v horní části hello hello hlavičky. |
| **Prstenec** | |
| Dotaz |Dotaz toorun pro prstenec hello.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota. |
| **Prstenec** |**> Center** |
| Text |Text toodisplay pod hodnotu hello uvnitř prstenec hello. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu.<br><br>-Součet: Přidejte hello hodnoty všech záznamů.<br>-Procento: Procento záznamů hello vrátil hodnoty hello v **způsobit hodnoty použít v operaci center** toohello celkový počet záznamů v dotazu hello. |
| Výsledek hodnoty použít v operaci center |Volitelně klikněte na tlačítko hello tooadd znaménko plus jedna nebo více hodnot.  Hello výsledky dotazu hello bude omezený toorecords s hello hodnoty vlastností, které určíte.  Pokud budou přidávána žádné hodnoty, jsou zahrnuty všechny záznamy v dotazu hello. |
| **Další možnosti** |**> Barvy** |
| Barva 1<br>Barva 2<br>Barva 3 |Vyberte barvu hello hello hodnot hello zobrazí v prstenec hello. |
| **Další možnosti** |**> Mapování Upřesnit barev** |
| Hodnota pole |Název typu hello toodisplay pole jej jako různých barev, pokud je součástí prstenec hello. |
| Barva |Vyberte barvu hello hello jedinečné pole. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Operace |Operace tooperform pro minigraf hello.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete vlastnost textu hello tooparse do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="two-timelines--list-part"></a>Dva časové osy & seznam součástí
Hlavička zobrazí hello výsledky dotazů dvě protokolu časem jako sloupcové grafy s popisek zobrazení jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Dva časové osy & seznamu zobrazení](media/log-analytics-view-designer/view-two-timelines-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Nejprve grafu<br>druhé grafu** | |
| Legendy |Text toodisplay pod hello popisku řady, první hello. |
| Barva |Barva toouse pro hello sloupce v řadě hello. |
| Dotaz |Dotaz toorun pro první řady hello.  hello grafu sloupců budou odpovídat Hello počet hello záznamy průběhu v každém časovém intervalu. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu pro popisek hello.<br><br>-Součet: Součet hodnoty hello ze všech záznamů.<br>-Průměr: Průměr hello hodnoty ze všech záznamů.<br>-Posledního vzorku: Hodnota od posledního intervalu hello součástí hello grafu.<br>-První ukázka: Hodnota z první interval hello součástí hello grafu.<br>-Count: Počet všech záznamů vrácených dotazem hello. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Operace |Operace tooperform pro minigraf hello.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="information-part"></a>Část informace
Záhlaví obsahuje statický text a nepovinný odkaz.  Zobrazí se seznam jedné nebo více položek s statický text a název.

![Informace o zobrazení](media/log-analytics-view-designer/view-information.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Barva |Barva pozadí hlavičky hello. |
| **Záhlaví** | |
| Image |Toodisplay soubor bitové kopie v záhlaví hello. |
| Štítek |Text toodisplay v záhlaví hello. |
| **Záhlaví** |**> Odkaz** |
| Štítek |Text odkazu. |
| URL |Adresa URL pro odkaz. |
| **Informace položky** | |
| Název |Toodisplay text pro nadpis každé položky. |
| Obsah |Toodisplay text pro každou položku. |

## <a name="line-chart-callout--list-part"></a>Spojnicový graf, popisku & část Seznam
Hlavička zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Spojnicový graf, popisků a zobrazení seznamu](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Záhlaví** | |
| Název |Toodisplay textu v horní části hello hello hlavičky. |
| Subtitle |Text toodisplay pod hello nadpis v horní části hello hello hlavičky. |
| **Spojnicový graf** | |
| Dotaz |Dotaz toorun pro hello spojnicový graf.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky.  Pokud dotaz hello používá hello **interval** – klíčové slovo pak hello osy x grafu hello použije tento časový interval.  Pokud dotaz hello neobsahuje hello **interval** – klíčové slovo a každou hodinu intervaly se používají pro hello osy x. |
| **Spojnicový graf** |**> Popisku** |
| Název popisku |Text toodisplay větší než hodnota popisku hello. |
| Název řady |Hodnota vlastnosti pro hello řady toouse pro hodnotu popisku hello.  Pokud je k dispozici žádné řady, použijí se všechny záznamy z dotazu hello. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu pro popisek hello.<br><br>-Průměr: Průměr hello hodnoty ze všech záznamů.<br>-Count počet všech záznamů vrácených dotazem hello.<br>-Posledního vzorku: Hodnota od posledního intervalu hello součástí hello grafu.<br>-Max: Maximální hodnota z hello intervaly součástí hello grafu.<br>-Min: Minimální hodnota z hello intervaly součástí hello grafu.<br>-Součet: Součet hodnoty hello ze všech záznamů. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte toouse hodnota na logaritmické stupnici pro hello osy y. |
| Jednotky |Zadejte hello jednotky pro hello hodnot vrácených dotazem hello.  Tyto informace jsou použité toodisplay popisky v grafu hello označující hello typy hodnot a volitelně pro převod hodnoty hello.  Hello typ jednotky určuje kategorii hello hello jednotky a definuje hello aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v převést toothen hello číselné hodnoty jsou převedeny z hello aktuální jednotku typu toohello převést tootype. |
| Vlastní popisek |Text toodisplay hello osy Y další toohello popisu pro typ jednotky hello.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky hello. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Operace |Operace tooperform pro minigraf hello.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete vlastnost textu hello tooparse do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="line-chart--list-part"></a>Část řádku seznamu & grafu
Hlavička zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  Seznam zobrazí hello top deset výsledků dotazu s grafické vyjádření hello relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Řádky – zobrazení seznamu & grafu](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| Použití ikony |Vyberte toohave hello ikona zobrazení. |
| **Záhlaví** | |
| Název |Toodisplay textu v horní části hello hello hlavičky. |
| Subtitle |Text toodisplay pod hello nadpis v horní části hello hello hlavičky. |
| **Spojnicový graf** | |
| Dotaz |Dotaz toorun pro hello spojnicový graf.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky.  Pokud dotaz hello používá hello **interval** – klíčové slovo pak hello osy x grafu hello použije tento časový interval.  Pokud dotaz hello neobsahuje hello **interval** – klíčové slovo a každou hodinu intervaly se používají pro hello osy x. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte toouse hodnota na logaritmické stupnici pro hello osy y. |
| Jednotky |Zadejte hello jednotky pro hello hodnot vrácených dotazem hello.  Tyto informace jsou použité toodisplay popisky v grafu hello označující hello typy hodnot a volitelně pro převod hodnoty hello.  Hello typ jednotky určuje kategorii hello hello jednotky a definuje hello aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v převést toothen hello číselné hodnoty jsou převedeny z hello aktuální jednotku typu toohello převést tootype. |
| Vlastní popisek |Text toodisplay hello osy Y další toohello popisu pro typ jednotky hello.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky hello. |
| **Seznam** | |
| Dotaz |Dotaz toorun seznam hello.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| Skrýt grafu |Vyberte toodisable hello grafu toohello napravo od hello číselný sloupec. |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva hello řádky nebo minigrafů. |
| Operace |Operace tooperform pro minigraf hello.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete vlastnost textu hello tooparse do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text toodisplay hello horní části hello první sloupec seznamu hello. |
| Hodnota |Text toodisplay hello horní části hello druhý sloupec seznamu hello. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte tooenable prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="stack-of-line-charts-part"></a>Zásobník části grafy řádku
Zobrazuje tři samostatné spojnicových grafů s více řad z protokolu dotazu v čase.

![Zásobník spojnicových grafů](media/log-analytics-view-designer/view-stack-line-charts.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Toodisplay textu hello horní části hello dlaždici. |
| Nové skupiny |V zobrazení hello počínaje hello aktuální zobrazení vyberte toocreate novou skupinu. |
| Ikona |Bitové kopie souboru toodisplay další toohello výsledek v záhlaví hello. |
| **Grafu 1<br>grafu 2<br>grafu 3** |**> Záhlaví** |
| Název |Toodisplay textu v horní části hello hello grafu. |
| Subtitle |Text toodisplay pod hello nadpis v horní části hello hello grafu. |
| **Grafu 1<br>grafu 2<br>grafu 3** |**Spojnicový graf** |
| Dotaz |Dotaz toorun pro hello spojnicový graf.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky.  Pokud dotaz hello používá hello **interval** – klíčové slovo pak hello osy x grafu hello použije tento časový interval.  Pokud dotaz hello neobsahuje hello **interval** – klíčové slovo a každou hodinu intervaly se používají pro hello osy x. |
| **Graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte toouse hodnota na logaritmické stupnici pro hello osy y. |
| Jednotky |Zadejte hello jednotky pro hello hodnot vrácených dotazem hello.  Tyto informace jsou použité toodisplay popisky v grafu hello označující hello typy hodnot a volitelně pro převod hodnoty hello.  Hello typ jednotky určuje kategorii hello hello jednotky a definuje hello aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v převést toothen hello číselné hodnoty jsou převedeny z hello aktuální jednotku typu toohello převést tootype. |
| Vlastní popisek |Text toodisplay hello osy Y další toohello popisu pro typ jednotky hello.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky hello. |

## <a name="common-settings"></a>Obecná nastavení
Hello následující oddíly popisují části nastavení běžné tooseveral vizualizace.

### <a name="name-value-separator">Název & hodnota oddělovače</a>
Pokud chcete vlastnost textu hello tooparse ze seznamu dotazu na více hodnot jeden znak oddělovač.  Pokud zadáte oddělovač, můžete zadat názvy pro každé pole oddělených hello stejné oddělovač hello název pole.

Představte si třeba vlastnost s názvem *umístění* , jako například zahrnuty hodnoty *Redmond vytváření 41* a *Bellevue Building12*.  Můžete zadat – pro hello název a hodnotu oddělovače a *města vytváření* pro hello název.  To by rozloží hodnoty jednotlivých do dvou vlastností názvem *města* a *vytváření*.

### <a name="navigation-query">Navigace dotazu</a>
Dotaz toorun, když uživatel hello vybere položku v seznamu hello.  Použití *{vybranou položku}* tooinclude hello syntaxe pro položku, která hello vybraného uživatele.

Například pokud hello dotaz má sloupec s názvem *počítače* a dotaz navigační hello je *{vybranou položku}*, dotazu jako *počítač = "Tento počítač"* by se spustí, když uživatel Hello vybrané počítače.  Pokud se dotaz navigační hello *typ = událostí {vybranou položku}* pak hello dotazu *typ = událostí počítač = "Tento počítač"* by spustit.

### <a name="sparklines">Minigrafů</a>
Minigraf je malý spojnicový graf, který znázorňuje hello hodnotu položky seznamu v čase.  Pro vizualizaci částí se seznamem můžete zvolit, zda toodisplay vodorovných ukazující hello relativní hodnota je číselný sloupec nebo minigraf indikující její hodnota v čase.

Hello následující tabulka popisuje hello nastavení pro minigrafů.

| Nastavení | Popis |
|:--- |:--- |
| Povolit minigrafů |Vyberte toodisplay minigraf místo vodorovném řádku. |
| Operace |Pokud jsou povolené minigrafů, jde hello operace tooperform na každou vlastnost v hello seznamu toocalculate hello hodnoty pro minigraf hello.<br><br>-Posledního vzorku: Poslední hodnotu pro sérii hello přes hello časový interval.<br>-Max: Maximální hodnota pro řadu hello přes hello časový interval.<br>-Min: Minimální hodnota pro řadu hello přes hello časový interval.<br>-Součet: Součet hodnot pro řadu hello přes hello časový interval.<br>-Shrnutí: Hello používá stejný příkaz měr jako hello dotaz v záhlaví hello. |

### <a name="thresholds">Prahové hodnoty</a>
Prahové hodnoty povolit toodisplay barevnou ikonu další tooeach položky v seznamu budete rychlý vizuální indikátor položek, které překročí určitou hodnotu nebo spadá do určitého rozsahu.  Pokud překročí hodnotu chyby, například může zobrazit zelená ikona pro položky s přijatelnou hodnotu, žlutý, pokud je hodnota hello v rozsahu, která určuje, upozornění a red.

Když povolíte prahové hodnoty pro část, je nutné zadat jeden nebo více prahové hodnoty.  Pokud hodnota hello položky je větší než prahová hodnota a nižší než prahová hodnota další hello, se používá tuto barvu.  Pokud hello položka je větší než pak nejvyšší prahovou hodnotu, tato barva nastavena.   

Každá sada prahová hodnota má jeden prahové hodnoty s hodnotou **výchozí**.  Toto je barva hello nastavit, pokud se překročí žádné jiné hodnoty.  Můžete přidat nebo odebrat prahové hodnoty kliknutím hello  **+**  nebo **x** tlačítko.

Hello následující tabulka popisuje hello nastavení pro prahů.

| Nastavení | Popis |
|:--- |:--- |
| Povolit prahové hodnoty |Vyberte toodisplay barvu ikonu toohello levé části každou hodnotu, která určuje jeho prahové hodnoty stavu relativní toospecified. |
| Name (Název) |Název tooidentify hello prahovou hodnotu. |
| Prahová hodnota |Hodnota pro mezní hodnotu hello.  Barva stavu Hello pro každou položku seznamu nastavena toohello barva hello nejvyšší prahová hodnota překročena hodnotou hello položky.  Neexistuje jeden výchozí prahová hodnota, která je barva hello, pokud se překročí žádné prahové hodnoty. |
| Barva |Barva hello prahovou hodnotu. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) toosupport hello dotazy v části vizualizace.
