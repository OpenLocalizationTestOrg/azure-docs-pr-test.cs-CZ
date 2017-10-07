---
title: aaaOverview metriky v Microsoft Azure | Microsoft Docs
description: "Zjistěte, jak monitorování toocustomize grafy v Azure."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c36031eb-4df5-4cd5-9479-311d493a40d2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 4196b2e9bda713e9a0abc349eed78685abcaf194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Přehled metriky v Microsoft Azure
Všech služeb Azure sledovat klíčové metriky, které umožňují toomonitor hello stav, výkon, dostupnost a využití vašich služeb. Tyto metriky lze zobrazit v hello portálu Azure, a můžete také hello [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess hello úplnou sadu metriky prostřednictvím kódu programu.

U některých služeb může být nutné tooturn na Diagnostika v pořadí toosee všechny metriky. Pro jiné, jako jsou virtuální počítače zobrazí se základní sadu metriky, ale potřebovat tooenable hello kompletní vysoká frekvence metriky. V tématu [zapínání monitorování a Diagnostika](insights-how-to-use-diagnostics.md) toolearn Další.

## <a name="using-monitoring-charts"></a>Používání monitorování grafů
Můžete všechny metriky hello grafu je přes vybraném časovém intervalu.

1. V hello [portálu Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet**, a pak prostředek vás zajímá monitorování.
2. Hello **monitorování** část obsahuje hello nejdůležitější metriky pro každý prostředků Azure. Například webové aplikace se **požadavky a chyby**, kde jako virtuální počítač by mít **procento využití procesoru** a **pro čtení a zápisu disku**: ![přehledu monitorování](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Kliknutím na jakékoli grafu se zobrazí můžete hello **metrika** okno. V okně hello kromě toohello grafu je tabulka zobrazující agregace metrik hello (například průměr, minimální a maximální přes hello časové rozmezí, které jste zvolili). V následující tabulce, jsou hello pravidla výstrah pro prostředek hello.
    ![Okno metriky](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. toocustomize hello řádky, které se zobrazí, klikněte na tlačítko hello **upravit** tlačítko v grafu hello nebo hello **upravit graf** příkazu v okně metriky hello.
5. V okně Upravit dotaz hello můžete udělat tři věci:
   
   * Změnit časové rozmezí hello
   * Přepínač hello vzhled mezi panelu a řádku
   * Zvolte jiný metics ![upravit dotaz](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Změnit časový rozsah hello je stejně snadná jako výběr jiný rozsah (například **poslední hodinu**) a kliknutím na **Uložit** v hello dolní části okna hello. Můžete také **vlastní**, což vám umožní toochoose období přes hello poslední dva týdny. Například se zobrazí hello celou dva týdny, nebo jenom 1 hodinu od včerejška. Zadejte hello textové pole tooenter jiné hodinu.
    ![Vlastní časový rozsah](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Kanál je pod hello časový rozsah, vyberte libovolný počet tooshow metriky v grafu hello.
8. Po kliknutí na tlačítko Uložit provedené změny se uloží pro tuto konkrétní prostředek. Například pokud máte dva virtuální počítače a změníte grafu na jednom, nemá vliv hello jiné.

## <a name="creating-side-by-side-charts"></a>Vytváření grafů vedle sebe
S hello výkonné přizpůsobení portálu hello můžete přidat libovolný počet grafy tak, jak chcete.

1. V hello **...**  nabídky hello horní části klikněte na okno hello **přidat dlaždice**:  
    ![Nabídka Přidat](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Potom můžete vyberte graf můžete vybrat z hello **Galerie** na pravé straně hello obrazovky: ![Galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Pokud nevidíte hello metrika chcete, můžete vždy přidat jeden z hello přednastavení metriky, a **upravit** hello grafu tooshow hello metriku, které potřebujete.

## <a name="monitoring-usage-quotas"></a>Sledování využití kvóty
Většina metriky ukazují trendů v čase, ale některá data, jako je využití kvóty, jsou informace o bodu v čase s prahovou hodnotou.

V okně hello pro prostředky, které mají kvóty, můžete také zjistit kvóty využití:

![Využití](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Jako metriky, můžete využít hello [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess hello úplnou sadu kvóty využití prostřednictvím kódu programu.

## <a name="next-steps"></a>Další kroky
* [Příjem oznámení o výstrahách](insights-receive-alert-notifications.md) vždy, když metriky protne prahovou hodnotu.
* [Povolit monitorování a Diagnostika](insights-how-to-use-diagnostics.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.
* [Automatické škálování počtu instancí](insights-how-to-scale.md) toomake, že je služba dostupná a dobře reagovaly.
* [Monitorování výkonu aplikací](../application-insights/app-insights-azure-web-apps.md) Pokud chcete toounderstand přesně jak kód provádí v cloudu hello.
* Použití [Application Insights pro aplikace JavaScript a webové stránky](../application-insights/app-insights-web-track-usage.md) tooget analytics klienta o hello prohlížečů, které navštívit webovou stránku.
* [Sledování dostupnosti a odezvy žádné webové stránce](../application-insights/app-insights-monitor-web-app-availability.md) s Application Insights, můžete zjistit, pokud stránka je vypnutý.

