---
title: "aaaMonitor službě Azure web app výkonu | Microsoft Docs"
description: "Monitorování výkonu webových aplikací Azure. Můžete vytvářet grafy zatížení a doby odezvy nebo informací o závislosti a nastavovat upozornění týkající se výkonu."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a>Monitorování výkonu webových aplikací Azure
V hello [portálu Azure](https://portal.azure.com) můžete nastavit monitorování výkonu aplikací na vašem [webové aplikace Azure](../app-service-web/app-service-web-overview.md). [Azure Application Insights](app-insights-overview.md) instruments telemetrie toosend aplikace o jeho aktivity toohello služby Application Insights, kde je uložený a analyzovat. Zde je možné metriky grafů a vyhledávací nástroje toohelp diagnostikovat problémy, výkon a vyhodnocení využití.

## <a name="run-time-or-build-time"></a>Za běhu nebo při sestavení
Můžete nakonfigurovat monitorování instrumentaci aplikace hello v některém ze dvou způsobů:

* **Za běhu** – Můžete vybrat rozšíření pro monitorování výkonu, když je webová aplikace již v živém provozu. Není nutné toorebuild, nebo znovu nainstalujte vaší aplikace. Obdržíte standardní sadu balíčků, které monitorují dobu odezvy, úspěšnost, výjimky, závislosti a další. 
* **Při sestavení** – Do aplikace můžete balíček nainstalovat během vývoje. Tato možnost nabízí větší variabilitu. V toohello přidání stejné standardní balíčků, můžete napsat kód toocustomize hello telemetrie nebo toosend vlastní telemetrii. Můžete protokolovat konkrétní aktivity nebo záznam událostí podle toohello sémantiku vaší domény aplikace. 

## <a name="run-time-instrumentation-with-application-insights"></a>Použití za běhu s Application Insights
Pokud už webovou aplikaci v Azure spouštíte, máte již monitorování do jisté míry k dispozici: můžete monitorovat požadavky a chybovost. Přidejte Application Insights tooget další, například dobu odezvy, monitorování toodependencies volání, inteligentní detekce a hello dotazovacího jazyka pro efektivní analýzy protokolů. 

1. **Vyberte Application Insights** Azure ovládacího panelu hello pro vaši webovou aplikaci.
   
    ![V části Monitorování zvolte Application Insights.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Zvolte toocreate nový prostředek, pokud jste již nastavili prostředek Application Insights pro tuto aplikaci pomocí jiné trasy.
2. Po instalaci Application Insights **webovou aplikaci používejte**. 
   
    ![Používejte webovou aplikaci.](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   **Povolte monitorování na straně klienta** pro zobrazení stránek a telemetrii uživatelů.

   * Klikněte na Nastavení > Nastavení aplikace.
   * V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty: 
   
    Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Hodnota: `true`
   * **Uložit** hello nastavení a **restartujte** vaší aplikace.
3. **Monitorujte aplikaci**.  [Expore hello data](#explore-the-data).

Pokud chcete později, můžete vytvořit aplikace hello pomocí Application Insights.

*Jak odebrat Application Insights, nebo přepnout toosending tooanother prostředků?*

* V Azure, otevřete hello webové aplikace ovládacího prvku okno a v části nástroje pro vývoj, otevřete **rozšíření**. Odstraňte rozšíření Application Insights hello. Potom v části sledování, vyberte Application Insights a vytvořte nebo vyberte hello prostředek, který chcete.

## <a name="build-hello-app-with-application-insights"></a>Sestavení aplikace hello s Application Insights
Application Insights může poskytovat podrobnější telemetrie po nainstalování sady SDK do příslušné aplikace. Konkrétně je možné shromažďovat protokoly trasování, [psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)a získávat podrobnější sestavy výjimek.

1. **V sadě Visual Studio** (2013 s aktualizací 2 nebo novější) nakonfigurujte Application Insights pro svůj projekt.

    Klikněte pravým tlačítkem na hello webový projekt a vyberte **Přidat > Application Insights** nebo **konfigurovat Application Insights**.
   
    ![Klikněte pravým tlačítkem na hello webový projekt a zvolte Přidat nebo konfigurovat Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    Zobrazení dotazu toosign v, použijte hello přihlašovací údaje k účtu Azure.
   
    operace Hello má dva důsledky:
   
   1. Vytvoří prostředek Application Insights v Azure, kde se ukládají, analyzují a zobrazují telemetrická data.
   2. Přidá hello Application Insights NuGet balíček tooyour kódu (pokud existuje již není) a nakonfiguruje jej toosend telemetrie toohello prostředků Azure.
2. **Testování hello telemetrie** ve spuštěné aplikaci hello ve vývojovém počítači (F5).
3. **Publikování aplikace hello** tooAzure v hello obvyklým způsobem. 

*Jak lze přepnout toosending tooa jiný prostředek služby Application Insights?*

* V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello, zvolte **konfigurovat Application Insights** a vyberte prostředek hello chcete. Získáte možnost toocreate hello nový prostředek. Proveďte opětné sestavení a nasazení.

## <a name="explore-hello-data"></a>Prozkoumejte hello data
1. V okně hello Application Insights z vaší webové aplikace ovládacích panelů, uvidíte metriky za provozu, který ukazuje žádostí a selhání v rámci druhý nebo dvě z nich ke kterým dochází. Toto zobrazení je velmi užitečné, pokud aplikaci znovu publikujete – okamžitě uvidíte veškeré případné problémy.
2. Proklikejte se prostřednictvím toohello úplné prostředek Application Insights.

    ![Proklikejte se.](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    Můžete tam přejít i přímo z navigace pro prostředky Azure.

1. Proklikejte se prostřednictvím jakékoli grafu tooget podrobněji:
   
    ![V okně Přehled hello Application Insights klikněte na graf](./media/app-insights-azure-web-apps/07-dependency.png)
   
    Můžete [přizpůsobit okna metrik](app-insights-metrics-explorer.md).
2. Klikněte na další toosee jednotlivé události a jejich vlastnosti:
   
    ![Klikněte na tlačítko tooopen typ události vyhledávání filtrováno typu](./media/app-insights-azure-web-apps/08-requests.png)
   
    Všimněte si, hello "..." odkaz tooopen všechny vlastnosti.
   
    Můžete [přizpůsobovat hledání](app-insights-diagnostic-search.md).

Pro účinnější hledání přes telemetrie, použijte hello [analýzy protokolů dotazu jazyka](app-insights-analytics-tour.md).

## <a name="more-telemetry"></a>Další telemetrická data

* [Data načítání webové stránky](app-insights-javascript.md)
* [Vlastní telemetrická data](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky
* [Spustit hello profileru ve vaší živé aplikaci](app-insights-profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – monitorujte službu Azure Functions pomocí Application Insights.
* [Povolit Azure diagnostics](app-insights-azure-diagnostics.md) toobe odeslané tooApplication statistiky.
* [Monitorování stavu metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
* [Přijímejte oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.
* Použití [Application Insights pro aplikace JavaScript a webové stránky](app-insights-javascript.md) tooget klienta telemetrie z hello prohlížečů, které navštívit webovou stránku.
* [Nastavit testy dostupnosti webu](app-insights-monitor-web-app-availability.md) toobe zobrazí výstraha, pokud váš webový server je mimo provoz.

