---
title: "aaaTile odkaz pro Návrhář zobrazení v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů vám umožní toocreate vlastní zobrazení v konzole hello OMS, které obsahují různé vizualizace dat v úložišti OMS hello. Tento článek obsahuje odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 4706abb16b8a3719f5dbe8c89cd61739391ab8f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-tile-reference"></a>Odkazy na dlaždici protokol Návrhář zobrazení analýzy
Hello Návrhář zobrazení v analýzy protokolů vám umožní toocreate vlastní zobrazení v konzole hello OMS, které obsahují různé vizualizace dat v úložišti OMS hello. Tento článek obsahuje odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Zobrazit návrháře](log-analytics-view-designer.md) -přehled hello Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.
* [Odkaz na část vizualizace](log-analytics-view-designer-parts.md) -odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná v hello [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován hello prostoru bude automtically převést.

Hello následující tabulka uvádí různé typy hello dlaždice, které jsou k dispozici v hello Návrhář zobrazení.  Hello části níže popisují každý typ dlaždice v podrobností a jejich vlastnosti.

| Dlaždice | Popis |
|:--- |:--- |
| [Číslo](#number-tile) |Jedno číslo zobrazuje počet záznamů z dotazu. |
| [Dvou čísel.](#two-numbers-tile) |Zobrazuje počet záznamů z dva různé dotazy dvou čísel jeden. |
| [Prstenec](#donut-tile) |Prstenec grafu na základě dotazu s souhrnnou hodnotu v centru hello. |
| [Spojnicový graf & popisku](#line-chart-amp-callout-tile) |Spojnicový graf na základě dotazu a popisku s souhrnnou hodnotu. |
| [Spojnicový graf](#line-chart-tile) |Spojnicový graf založené na dotazu. |
| [Dva časové osy](#two-timelines-tile) |Sloupcový graf s dvou řad, každou v závislosti na samostatné dotazu. |

## <a name="number-tile"></a>Číslo dlaždice
Hello **číslo** dlaždice zobrazí jedno číslo zobrazuje počet hello záznamů z protokolu dotazu a štítek.

![Číslo dlaždice](media/log-analytics-view-designer/tile-number.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| **Dlaždice** | |
| Legendy |Text toodisplay pod hello hodnotu. |
| Dotaz |Toorun dotazu.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="two-numbers-tile"></a>Dlaždice dvou čísel.
Hello **dva číslo** dlaždice zobrazí zobrazující hello počet záznamy ze dvou různých protokolových dotazy a popisek pro každý dvou čísel.

![Dlaždice dvou čísel.](media/log-analytics-view-designer/tile-two-numbers.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| **První dlaždice** | |
| Legendy |Text toodisplay pod hello hodnotu. |
| Dotaz |Toorun dotazu.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| **Druhý dlaždice** | |
| Legendy |Text toodisplay pod hello hodnotu. |
| Dotaz |Toorun dotazu.  Zobrazí se počet Hello hello počet záznamů vrácených dotazem hello. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="donut-tile"></a>Dlaždice prstenec
Hello **prstenec** dlaždice zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Hello prstenec graficky zobrazí výsledky hello nejvyšší tři záznamů.

![Dlaždice prstenec](media/log-analytics-view-designer/tile-donut.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| **Prstenec** | |
| Dotaz |Dotaz toorun pro prstenec hello.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky. |
| **Prstenec** |**> Center** |
| Text |Text toodisplay pod hodnotu hello uvnitř prstenec hello. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu.<br><br>-Součet: Přidejte hello hodnoty všech záznamů se hodnota vlastnosti hello.<br>-Procento: Procento hello sčítají hodnoty ze záznamů se hello vlastnost hodnota porovnání toohello sčítají hodnoty všech záznamů. |
| Výsledek hodnoty použít v operaci center |Volitelně klikněte na tlačítko hello tooadd znaménko plus jedna nebo více hodnot.  Hello výsledky dotazu hello bude omezený toorecords s hello hodnoty vlastností, které určíte.  Pokud budou přidávána žádné hodnoty, než všechny záznamy jsou zahrnuty v dotazu hello. |
| **Prstenec** |**> Další možnosti** |
| Barvy |Hello toodisplay barvu pro každé z vlastností pro tři hlavní hello.  Pokud chcete toospecify alternativní barvy pro konkrétní hodnoty vlastnosti, použijte rozšířené mapování barev. |
| Mapování pokročilé barev |Zobrazí barvu pro konkrétní hodnoty vlastnosti.  Pokud hello hodnotu, kterou jste určili v hello nejvyšší tři, tak Alternativní barva hello se zobrazí místo standardní barva hello.  Pokud vlastnost hello není v hello nejvyšší tři a potom hello barva se nezobrazí. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="line-chart-tile"></a>Dlaždice grafu řádku
Hello **spojnicový graf** dlaždice zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  

![Spojnicový graf & popisku dlaždice](media/log-analytics-view-designer/tile-line-chart.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| **Spojnicový graf** | |
| Dotaz |Dotaz toorun pro hello spojnicový graf.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky.  Pokud dotaz hello používá hello **interval** – klíčové slovo pak hello osy x grafu hello použije tento časový interval.  Pokud dotaz hello neobsahuje hello **interval** – klíčové slovo a každou hodinu intervaly se používají pro hello osy x. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte toouse hodnota na logaritmické stupnici pro hello osy y. |
| Jednotky |Zadejte hello jednotky pro hello hodnot vrácených dotazem hello.  Tyto informace jsou použité toodisplay popisky v grafu hello označující hello typy hodnot a volitelně pro převod hodnoty hello.  Hello **typ jednotky** Určuje kategorii hello hello jednotky a definuje hello **aktuální typ jednotky** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** pak hello číselné hodnoty jsou převést z hello **aktuální jednotku** zadejte toohello **převést na** typu. |
| Vlastní popisek |Text toodisplay hello osy Y další toohello popisu pro typ jednotky hello.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky hello. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="line-chart--callout-tile"></a>Dlaždice grafu & popisků čáry
Hello **čáry popisku & grafu** dlaždice zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  

![Spojnicový graf & popisku dlaždice](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| **Spojnicový graf** | |
| Dotaz |Dotaz toorun pro hello spojnicový graf.  první vlastnost Hello by měl být text hodnota a hello druhý vlastnost číselná hodnota.  Toto je obvykle dotaz, který používá hello **měr** – klíčové slovo toosummarize výsledky.  Pokud dotaz hello používá hello **interval** – klíčové slovo pak hello osy x grafu hello použije tento časový interval.  Pokud dotaz hello neobsahuje hello **interval** – klíčové slovo a každou hodinu intervaly se používají pro hello osy x. |
| **Spojnicový graf** |**> Popisku** |
| Popisku |Title Text toodisplay větší než hodnota popisku hello. |
| Název řady |Hodnota vlastnosti pro hello řady toouse pro hodnotu popisku hello.  Pokud je k dispozici žádné řady, použijí se všechny záznamy z dotazu hello. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu pro popisek hello.<br>-Průměr: Průměr hello hodnoty ze všech záznamů.<br><br>-Count: Počet všech záznamů vrácených dotazem hello.<br>-Posledního vzorku: Hodnota od posledního intervalu hello součástí hello grafu.<br>-Max: Maximální hodnota z hello intervaly součástí hello grafu.<br>-Min: Minimální hodnota z hello intervaly součástí hello grafu.<br>-Součet: Součet hodnoty hello ze všech záznamů. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte toouse hodnota na logaritmické stupnici pro hello osy y. |
| Jednotky |Zadejte hello jednotky pro hello hodnot vrácených dotazem hello.  Tyto informace jsou použité toodisplay popisky v grafu hello označující hello typy hodnot a volitelně pro převod hodnoty hello.  Hello **typ jednotky** Určuje kategorii hello hello jednotky a definuje hello **aktuální typ jednotky** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** pak hello číselné hodnoty jsou převést z hello **aktuální jednotku** zadejte toohello **převést na** typu. |
| Vlastní popisek |Text toodisplay hello osy Y další toohello popisu pro typ jednotky hello.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky hello. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="two-timelines-tile"></a>Dlaždice obě časové osy
Hello **obě časové osy** dlaždice zobrazí výsledky hello dva dotazy protokolu časem jako sloupcové grafy.  U každé série se zobrazí popisek.  

![Dlaždice obě časové osy](media/log-analytics-view-designer/tile-two-timelines.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Toodisplay textu hello horní části hello dlaždici. |
| Popis |Text toodisplay pod názvem dlaždice hello. |
| První graf | |
| Legendy |Text toodisplay pod hello popisku řady, první hello. |
| Barva |Barva toouse pro hello sloupce v první řadě hello. |
| Graf dotazu |Dotaz toorun pro první řady hello.  hello grafu sloupců budou odpovídat Hello počet hello záznamy průběhu v každém časovém intervalu. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu pro popisek hello.<br><br>-Průměr: Průměr hello hodnoty ze všech záznamů.<br>-Count: Počet všech záznamů vrácených dotazem hello.<br>-Posledního vzorku: Hodnota od posledního intervalu hello součástí hello grafu.<br>-Max: Maximální hodnota z hello intervaly součástí hello grafu. |
| **Druhý graf** | |
| Legendy |Text toodisplay pod hello popisku řady, druhý hello. |
| Barva |Barva toouse pro hello sloupce v druhé řady hello. |
| Graf dotazu |Dotaz toorun pro druhý řady hello.  hello grafu sloupců budou odpovídat Hello počet hello záznamy průběhu v každém časovém intervalu. |
| Operace |operace tooperform Hello na hello hodnotu vlastnosti toosummarize tooa jednu hodnotu pro popisek hello.<br><br>-Průměr: Průměr hello hodnoty ze všech záznamů.<br>-Count: Počet všech záznamů vrácených dotazem hello.<br>-Posledního vzorku: Hodnota od posledního intervalu hello součástí hello grafu.<br>-Max: Maximální hodnota z hello intervaly součástí hello grafu. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici hello.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici hello.  To je obvykle používanými tooprovide zprávu během hello dočasné období při zobrazení hello je nainstalovaný a dodává data k dispozici. |
| Dotaz |Dotaz toorun toocheck, pokud jsou k dispozici pro zobrazení hello data.  Pokud hello dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hello hodnoty z dotazu hlavní hello. |
| Zpráva |Zpráva toodisplay Pokud hello datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) toosupport hello dotazy v dlaždice.
* Přidat [vizualizace částí](log-analytics-view-designer-parts.md) tooyour vlastní zobrazení.
