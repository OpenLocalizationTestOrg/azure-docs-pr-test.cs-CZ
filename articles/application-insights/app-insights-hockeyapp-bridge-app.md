---
title: "aaaExploring HockeyApp dat ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Zkoumat HockeyApp data ve službě Application Insights
[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) doporučuje hello platforma pro monitorování provozu desktop a mobilní aplikace. Z HockeyApp můžete odeslat vlastní a sledování využití telemetrie toomonitor a pomoct s diagnostikou (v přidání data o chybách toogetting). Tento datový proud telemetrie lze dotazovat pomocí hello výkonné [Analytics](app-insights-analytics.md) funkce [Azure Application Insights](app-insights-overview.md). Kromě toho můžete [exportovat vlastní hello a trasování telemetrie](app-insights-export-telemetry.md). tooenable tyto funkce, musíte nastavit mostu, který předává HockeyApp vlastních dat tooApplication statistiky.

## <a name="hello-hockeyapp-bridge-app"></a>aplikace Hello most HockeyApp
Hello HockeyApp most aplikace je hello základní funkce, která umožňuje tooaccess HockeyApp vlastní a trasování telemetrii ve službě Application Insights prostřednictvím hello analýzy a průběžné Export funkcí. Vlastní a trasování událostí shromážděných za HockeyApp po vytvoření hello hello HockeyApp most aplikace budou přístupné z těchto funkcí. Podívejme se, jak tooset si některé z těchto aplikací most.

V HockeyApp, otevřete nastavení účtu [rozhraní API tokenů](https://rink.hockeyapp.net/manage/auth_tokens). Vytvořit nový token nebo znovu použít existující šablonu. minimální oprávnění Hello vyžaduje, jsou "jen pro čtení". Trvat kopii hello API tokenu.

![Získání tokenu rozhraní API HockeyApp](./media/app-insights-hockeyapp-bridge-app/01.png)

Otevřete hello portálu Microsoft Azure a [vytvořte prostředek Application Insights](app-insights-create-new-resource.md). Nastavte typ aplikace příliš "HockeyApp most aplikace":

![Nový prostředek Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

Nepotřebujete tooset název – automaticky nastaví ho z názvu HockeyApp hello.

Hello HockeyApp most pole se zobrazí. 

![Zadejte pole most](./media/app-insights-hockeyapp-bridge-app/03.png)

Zadejte hello HockeyApp token, který jste si předtím poznamenali. Tato akce vyplní hello "HockeyApp aplikace" rozevírací nabídku s vašimi aplikacemi HockeyApp. Vyberte hello jeden chcete toouse a dokončení hello zbytek hello pole. 

Otevřete nový prostředek hello. 

Všimněte si, že jeho zpracování chvíli trvá hello dat předávaných toostart.

![Prostředek Application Insights čekání dat](./media/app-insights-hockeyapp-bridge-app/04.png)

A to je vše! Vlastní a trasování data shromážděná ve vaší aplikaci instrumentovány HockeyApp od tohoto okamžiku je nyní také k dispozici tooyou v hello analýzy a průběžné exportovat funkce Application Insights.

Stručně pojďme si shrnout každý z těchto funkcí tooyou nyní k dispozici.

## <a name="analytics"></a>Analýza
Analýza je výkonný nástroj pro ad-hoc dotazování dat, což vám toodiagnose a analyzovat telemetrie a rychle zjistit hlavní příčiny a vzory.

![Analýza](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Další informace o analýzy](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Průběžný export
Průběžné Export vám umožní tooexport dat do kontejner úložiště objektů Blob Azure. To je užitečné, pokud potřebujete mít tookeep data po dobu delší než doba uchování hello aktuálně nabízená Application Insights. Můžete ponechat hello data v úložišti objektů blob, zpracování do databáze SQL nebo vaše upřednostňované řešení datového skladu.

[Další informace o průběžné Export](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Další kroky
* [Použít Analytics tooyour data](app-insights-analytics-tour.md)

