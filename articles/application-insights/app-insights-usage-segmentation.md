---
title: "Analýza aaaUser, relace a událostí ve službě Azure Application Insights | Microsoft docs"
description: "Demografické údaje analýzy uživatelů vaší webové aplikace."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analýza uživatelů, relací a událostí ve službě Application Insights

Zjistěte, kdy lidí používá vaši webovou aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají. Analyzovat obchodní a využití telemetrie pomocí [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Začínáme

Pokud nevidíte ještě data v hello uživatelů, relací nebo události okna portálu Application Insights hello [zjistěte, jak tooget pracovat s nástroji využití hello](app-insights-usage-overview.md).

## <a name="hello-users-sessions-and-events-segmentation-tool"></a>Nástroj segmentaci uživatelů, relací a události Hello

Tři hello využití pomocí okna hello stejné nástroj tooslice a rozčlenění telemetrie z vaší webové aplikace z tři perspektivy. Filtrování a rozdělení hello dat, můžete odkrýt přehledy o využití relativní hello různé stránky a funkcí.

* **Nástroj Uživatelé**: kolik lidí používá vaši aplikaci a jejich funkce.  Uživatelé, se počítají pomocí anonymní ID uložené v prohlížeči soubory cookie. Jednoho uživatele, kteří používají různé prohlížeče nebo počítače se budou počítat jako více než jeden uživatel.
* **Nástroj relací**: kolik relací aktivity uživatelů, zahrnuli určité stránky a funkce vaší aplikace. Relace se počítá po půl hodiny nečinnosti uživatele nebo po průběžné 24h použití.
* **Nástroj pro události**: jak často se používají určité stránky a funkce vaší aplikace. Zobrazení stránky se počítá při prohlížeč načte stránky z vaší aplikace, pokud máte [instrumentovány ho](app-insights-javascript.md). 

    Vlastní události představuje jeden výskyt něco stane ve vaší aplikaci, často interakci s uživatelem jako klikněte na tlačítko nebo hello dokončení některé úlohy. Příliš vložení kódu v aplikaci[generovat vlastní události](app-insights-api-custom-events-metrics.md#trackevent).

![Použití nástroje](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Dotaz na určité uživatele 

Prozkoumejte různé skupiny uživatelů úpravou možností dotazu hello v horní části hello hello uživatelé nástroje: 

* Kdo používá: Zvolte vlastní události a stránky zobrazení. 
* Během: Vyberte časový rozsah. 
* Podle: Vyberte, jak toobucket hello dat, buď v časovém intervalu, nebo jinou vlastnost třeba prohlížeče nebo města. 
* Rozdělit: Zvolte vlastnosti a datových hello toosplit nebo segmentu. 
* Přidání filtrů: Omezit hello dotazu toocertain uživatelů, relací nebo události na základě jejich vlastností, jako je například prohlížeč nebo města. 
 
## <a name="saving-and-sharing-reports"></a>Ukládání a sdílení sestavy 
Uživatelé sestavy, buď soukromé jenom tooyou v části Moje sestavy hello můžete uložit nebo sdíleny se všemi ostatními s toothis přístup k prostředku Application Insights v hello část sdílené sestavy.  
 
Při ukládání sestavy nebo úpravou jeho vlastnosti, vyberte toosave "Aktuální relativní časové rozmezí", které sestavy bude nepřetržitě aktualizovat data, návratem některé pevné množství času.  
 
Vyberte sestavu s pevnou sadu dat toosave "Aktuální absolutní časový rozsah". Mějte na paměti, že data ve službě Application Insights je uložena pouze po dobu 90 dnů, takže pokud uplynulo víc než 90 dní od sestavy s rozsahem absolutním čase byla uložena, hello sestavy se zobrazí prázdné. 
 
## <a name="example-instances"></a>Příklad instancí

Hello oddílu instancí příklad zobrazuje informace o několik jednotlivých uživatelů, relací a události, které jsou porovnávány pomocí hello aktuální dotaz. Vzhledem k tomu a prohlížení hello chování jednotlivce v přidání tooaggregates může poskytovat přehled o tom, jak lidé ve skutečnosti používají vaši aplikaci. 
 
## <a name="insights"></a>Insights 

Statistika Hello bočním panelu ukazuje velkých clusterech uživatelů, které sdílejí společné vlastnosti. Tyto clustery odkrýt překvapivé trendy v tom, jak uživatelé používají vaši aplikaci. Pokud například 40 % všech hello využití aplikace pochází z uživatelé, kteří používají jeden funkce.  


## <a name="next-steps"></a>Další kroky
- využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.
    - [Trychtýře](usage-funnels.md)
    - [Uchování](app-insights-usage-retention.md)
    - [Toky uživatele](app-insights-usage-flows.md)
    - [Workbooks](app-insights-usage-workbooks.md)
    - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)

