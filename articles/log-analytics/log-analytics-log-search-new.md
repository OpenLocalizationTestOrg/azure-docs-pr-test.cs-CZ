---
title: "hledání aaaLog v OMS Log Analytics | Microsoft Docs"
description: "Vyžadujete tooretrieve vyhledávání protokolu žádná data z analýzy protokolů.  Tento článek popisuje, jak nový protokol hledání se používají v analýzy protokolů a poskytuje koncepty, je nutné před vytvořením jeden toounderstand."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 08fda1d9eb9e6ab824ffb9e12af09832c3e3fad2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-log-searches-in-log-analytics"></a>Principy protokolu hledání v analýzy protokolů

> [!NOTE]
> Tento článek popisuje protokol hledání v Azure Log Analytics pomocí dotazovacího jazyka pro nové hello.  Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).  
>
> Pokud pracovní prostor nebyla upgradovaná toohello nové dotazovací jazyk, se seznamte s příliš[najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md).

Vyžadujete tooretrieve vyhledávání protokolu žádná data z analýzy protokolů.  Zda při analýze dat hello portálu, konfigurace toobe pravidlo výstrahy oznámení určitá podmínka, nebo načítání dat pomocí hello Log Analytics API, budete používat hledání toospecify hello data protokolu, které chcete.  Tento článek popisuje, jak se používají protokol hledání v analýzy protokolů a poskytuje koncepty, které je třeba porozumět před vytvořením jeden. V tématu hello [další kroky](#next-steps) části Podrobné informace o vytváření a úpravy protokolu hledání a odkazy na hello dotazovací jazyk.

## <a name="where-log-searches-are-used"></a>Použití protokolu hledání

Hello různé způsoby, že použijete protokolu hledání v analýzy protokolů zahrnout hello následující:

- **Portálů.** Interaktivní analýzu dat můžete provádět v hello úložiště s hello [hledání protokolů portál](log-analytics-log-search-log-search-portal.md) nebo hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587).  To vám umožní tooedit vaše dotazování a analýze hello výsledky v různých formátech a vizualizací.  Většina dotazů, které vytvoříte se spustí v jednom hello portálů a pak zkopíruje Jakmile ověříte, že funguje podle očekávání.
- **Pravidla výstrah.** [Pravidla výstrah](log-analytics-alerts.md) aktivně identifikovat problémy z dat v pracovním prostoru.  Každé pravidlo výstrahy je založen na protokolu vyhledávání, který se automaticky spouští v pravidelných intervalech.  výsledky Hello jsou zkontrolovanou toodetermine, pokud by se měl vytvořit výstrahu.
- **Zobrazení.**  Můžete vytvořit vizualizace dat toobe součástí řídicí panely uživatele s [Návrhář zobrazení](log-analytics-view-designer.md).  Protokol hledání poskytují data hello používá [dlaždice](log-analytics-view-designer-tiles.md) a [vizualizace částí](log-analytics-view-designer-parts.md) v každém zobrazení.  Podrobnostem z části vizualizace do hello hledání protokolů portálu tooperform další analýzu dat hello.
- **Export.**  Při exportu dat z tooExcel pracovní prostor analýzy protokolů hello nebo [Power BI](log-analytics-powerbi.md), vytvoříte protokolu vyhledávání toodefine hello data tooexport.
- **Prostředí PowerShell.** Skript prostředí PowerShell můžete spustit z příkazového řádku nebo runbooku automatizace Azure, který používá [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) tooretrieve data z analýzy protokolů.  Tento příkaz cmdlet vyžaduje tooretrieve data hello toodetermine dotazu.
- **Log Analytics API.**  Hello [analýzy protokolů protokolu rozhraní API služby search](log-analytics-log-search-api.md) umožňuje tooretrieve data z pracovního prostoru hello libovolného klienta REST API.  požadavek Hello rozhraní API obsahuje dotaz, který spustí pro analýzy protokolů toodetermine hello data tooretrieve.

![Protokol hledání](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>Uspořádání dat analýzy protokolů
Když vytvoříte dotaz, je třeba spustit tak, že určíte, které tabulky mají hello data, která hledáte. Každý [zdroj dat](log-analytics-data-sources.md) a [řešení](../operations-management-suite/operations-management-suite-solutions.md) ukládá data ve vyhrazené tabulkách pracovní prostor analýzy protokolů hello.  Dokumentace pro každý zdroj dat a řešení obsahuje název hello hello datového typu, který vytvoří a popis každého z jeho vlastnosti.     Mnoho dotazů bude potřebovat pouze data z jedné tabulky, ale ostatní mohou používat různé možnosti tooinclude dat z více tabulek.

![Tabulky](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>Zápis dotazu
Jádrem hello protokolu hledání v analýzy protokolů je [rozsáhlé dotazovací jazyk](https://docs.loganalytics.io/) které vám umožní načíst a analyzovat data z úložiště hello mnoha různými způsoby.  Tento stejný jazyk dotazu se používá pro [Application Insights](../application-insights/app-insights-analytics.md).  Zjištění, jak toowrite dotazu je důležité toocreating protokolu hledání v analýzy protokolů.  Budete obvykle začínat základní dotazy a pak průběh toouse pokročilejší funkce jsou složitější vašim požadavkům.

Základní struktura Hello dotazu je zdrojová tabulka, za nímž následuje řadu operátory, které jsou odděleny svislou čarou `|`.  Můžete zřetězit více operátory toorefine hello data za sebou a provádět pokročilé funkce.

Předpokládejme například, že jste chtěli toofind hello top deset počítače s hello většina chybové události přes hello poslední den.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Nebo možná budete chtít toofind počítačů, které nebyly obsahovaly prezenční signál v hello poslední den.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Co spojnicový graf s hello využití procesoru pro každý počítač, z poslední týden?

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

Je vidět na tyto rychlé vzorků, které se bez ohledu na druh hello dat, se kterými pracujete, hello struktura hello dotazu je podobné.  Lze ho rozdělit na odlišné kroky, které se budou odesílat hello Výsledná data z jednoho příkazu prostřednictvím hello kanálu toohello další příkaz.

Úplnou dokumentaci k hello Azure Log Analytics dotazovací jazyk včetně kurzy a referenční dokumentace jazyka najdete v části hello [Azure Log Analytics dotazu jazyka dokumentaci](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Další kroky

- Další informace o hello [portálů použít toocreate a upravovat vyhledávání protokolu](log-analytics-log-search-portals.md).
- Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí dotazovacího jazyka pro nové hello.
