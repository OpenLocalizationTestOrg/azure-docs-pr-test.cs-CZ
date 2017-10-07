---
title: "Analýza aaaUser uchovávání informací pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
description: "Počet uživatelů, kteří vrátit tooyour aplikace?"
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
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Analýza uživatelů uchovávání informací pro webové aplikace pomocí Application Insights

Funkce uchování Hello v [Azure Application Insights](app-insights-overview.md) pomáhá analyzovat počet uživatelů, kteří vrátit tooyour aplikace a jak často provádět konkrétní úlohy nebo dosáhnout cíle. Například pokud spustíte herní lokality, může porovnat hello počet uživatelů, kteří vrátit toohello lokality po ztrátě hry s číslem hello, kteří se vrátí po hodnocený. Tyto znalosti vám pomůžou líp uživatelské prostředí a obchodní strategie.

## <a name="get-started"></a>Začínáme

Pokud nevidíte ještě dat v nástroji uchování hello hello portálu Application Insights, [zjistěte, jak tooget pracovat s nástroji využití hello](app-insights-usage-overview.md).

## <a name="hello-retention-tool"></a>Nástroj pro uchování Hello

![Nástroj Udržení](./media/app-insights-usage-retention/retention.png)

1. Hello nástrojů umožňuje uživatelům toocreate nové sestavy uchování, otevřete stávajících sestav uchování, uložit aktuální sestavu uchování nebo uložit jako, vrátit zpět změny provedené toosaved sestavy a aktualizovat data v sestavě hello, sdílené složky sestavy prostřednictvím e-mailu nebo přímý odkaz a hello přístup stránka dokumentace. 
2. Ve výchozím nastavení zobrazuje uchování všichni uživatelé, kteří neprovedli nic, pak se vrátila zpět a neprovedli nic jiného v období. Můžete vybrat různé kombinace události toonarrow hello zaměřením na aktivit konkrétního uživatele.
3. Přidejte jeden nebo více filtry vlastností. Například se můžete soustředit na uživatele v konkrétní země nebo oblasti. Klikněte na tlačítko **aktualizace** po nastavení filtrů hello. 
4. Hello celkové uchování graf zobrazuje souhrnné údaje o uchování uživatele napříč hello vybrané časové období. 
5. Hello mřížky ukazuje zachování hello počet uživatelů podle toohello Tvůrce dotazů v #. 2. Každý řádek představuje kohorty uživatele, který provedl všechny události v hello časovém období. Každá buňka hello řádek ukazuje, kolik této kohorty vrátil nejméně jednou za na pozdější dobu. Někteří uživatelé se můžou vrátit ve více než jedno období. 
6. Hello Statistika karty Zobrazit nejvyšší pět inicializace události a nejvyšší pět vrátil události toogive uživatelé lépe porozumět jejich uchování sestav. 

![Při přechodu myší uchování](./media/app-insights-usage-retention/hover.png)

Uživatelé mohou pozastavte ukazatel myši nad buněk na hello uchování nástroj tooaccess hello analytics tlačítko a popisy vysvětlující, co hello buňky prostředky. Tlačítko Analytics Hello trvá nástroj pro analýzu toohello uživatelé s uživateli předem vyplněná dotazu toogenerate z hello buňky. 

## <a name="use-business-events-tootrack-retention"></a>Použít obchodní události tootrack uchování

tooget hello nejužitečnější uchování analýzy, míru události, které představují důležité obchodní aktivity. 

Například může být řada uživatelů otevřete stránku ve vaší aplikaci bez hraní her hello, který se zobrazí. Sledování právě zobrazení stránky hello by proto zadejte nesprávné odhad kolik lidí vrátit tooplay hello herní po jeho využívat dříve. tooget přehledné informace o vrácení přehrávače, vaše aplikace by měli poslat vlastní události, když uživatel ve skutečnosti hraje.  

Je toocode vhodné vlastní se události, které představují klíče obchodní akce a použít pro uchování analýzy. toocapture hello herní výsledek, musíte toowrite řádek kódu toosend vlastní události tooApplication statistiky. Pokud píšete ho v kódu webové stránky hello nebo v Node.JS, vypadá takto:

```JavaScript
    appinsights.trackEvent("won game");
```

Nebo v serverovém kódu ASP.NET:

```C#
   telemetry.TrackEvent("won game");
```

[Další informace o vytváření vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Další kroky
- využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.
    - [Uživatelé, relace, události](app-insights-usage-segmentation.md)
    - [Trychtýře](usage-funnels.md)
    - [Toky uživatele](app-insights-usage-flows.md)
    - [Workbooks](app-insights-usage-workbooks.md)
    - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)


