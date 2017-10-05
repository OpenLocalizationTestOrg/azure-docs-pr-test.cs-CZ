---
title: "Zkoumat HockeyApp data ve službě Azure Application Insights | Microsoft Docs"
description: "Analýza využití a výkonu vaší aplikace Azure pomocí Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: 450ca10613d137393090578619f3766734d1d493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Zkoumat HockeyApp data ve službě Application Insights
[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je doporučené platforma pro monitorování provozu desktop a mobilní aplikace. Z HockeyApp můžete odeslat vlastní a trasování telemetrii ke sledování využití a pomoct s diagnostikou (kromě získávání data o chybách). Tento datový proud telemetrie lze dotazovat pomocí výkonného [Analytics](app-insights-analytics.md) funkce [Azure Application Insights](app-insights-overview.md). Kromě toho můžete [exportovat vlastní a trasování telemetrie](app-insights-export-telemetry.md). Chcete-li tyto funkce povolit, nastavte mostu, který předává HockeyApp vlastních dat do služby Application Insights.

## <a name="the-hockeyapp-bridge-app"></a>HockeyApp most aplikace
Tato aplikace most HockeyApp je základní funkce, která umožňuje přístup k vašim HockeyApp vlastní a trasování telemetrii ve službě Application Insights prostřednictvím analýzy a průběžné Export funkcí. Vlastní a trasování událostí shromážděných za HockeyApp po vytvoření aplikace most HockeyApp budou přístupné z těchto funkcí. Podíváme se, jak nastavit některé z těchto aplikací most.

V HockeyApp, otevřete nastavení účtu [rozhraní API tokenů](https://rink.hockeyapp.net/manage/auth_tokens). Vytvořit nový token nebo znovu použít existující šablonu. Minimální práva potřebná jsou "jen pro čtení". Trvat kopii rozhraní API tokenu.

![Získání tokenu rozhraní API HockeyApp](./media/app-insights-hockeyapp-bridge-app/01.png)

Otevřete portál Microsoft Azure a [vytvořte prostředek Application Insights](app-insights-create-new-resource.md). Typ aplikace nastavte na "HockeyApp most aplikace":

![Nový prostředek Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

Nemusíte nastavení názvu – to bude možné z názvu HockeyApp automaticky nastavit.

Zobrazí se pole most HockeyApp. 

![Zadejte pole most](./media/app-insights-hockeyapp-bridge-app/03.png)

Zadejte HockeyApp token, který jste si předtím poznamenali. Tato akce vyplní "HockeyApp aplikace" rozevírací nabídky s vašimi aplikacemi HockeyApp. Vyberte ten, který chcete použít a dokončete polí. 

Otevřete nový prostředek. 

Všimněte si, že data trvá dobu spuštění toku.

![Prostředek Application Insights čekání dat](./media/app-insights-hockeyapp-bridge-app/04.png)

A to je vše! Vlastní a trasování data shromážděná ve vaší aplikaci instrumentovány HockeyApp od tohoto okamžiku je nyní také k dispozici v analýzy a průběžné Export funkce Application Insights.

Stručně pojďme si shrnout každý z těchto funkcí, které jsou nyní k dispozici.

## <a name="analytics"></a>Analýza
Analytics je výkonný nástroj pro zadávání dotazů ad-hoc vašich dat, který vám umožní diagnostikovat a analyzovat telemetrie a rychle zjistit hlavní příčiny a vzory.

![Analýza](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Další informace o analýzy](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Průběžný export
Průběžné Export umožňuje exportovat data do kontejner úložiště objektů Blob Azure. To je užitečné, pokud je potřeba uchovávat data po dobu delší než doba uchování aktuálně nabízená Application Insights. Můžete ponechat data v úložišti objektů blob, zpracování do databáze SQL nebo vaše upřednostňované řešení datového skladu.

[Další informace o průběžné Export](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Další kroky
* [Pro data aplikace Analytics](app-insights-analytics-tour.md)

