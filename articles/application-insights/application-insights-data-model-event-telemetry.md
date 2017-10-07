---
title: "aaaAzure aplikace Insights Telemetrie datový Model - událostí Telemetrie | Microsoft Docs"
description: "Aplikace Insights datový model pro událost telemetrie"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: cd7dc3c5f4f3df22b7a52ee79fcad566a27a9f4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-telemetry-application-insights-data-model"></a>Událost telemetrie: Application Insights datový model

Události můžete vytvořit položky telemetrie (v [Application Insights](app-insights-overview.md)) toorepresent událost, která v aplikaci. Obvykle je zásahu uživatele, jako je tlačítko klikněte na tlačítko nebo pořadí checkout. Může být také událost životní cyklus aplikace jako aktualizace inicializace nebo konfigurace. 

Události sémanticky, může nebo nemusí být korelační toorequests. Ale pokud používá správně, telemetrie událostí je důležitější než požadavky nebo trasování. Události představují obchodní telemetrie a měl by být tooseparate předmětu, méně agresivní [vzorkování](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Name (Název)

Název události. tooallow správné seskupení a užitečné metriky, omezit aplikace tak, aby vygeneruje malý počet názvy samostatná událost. Nepoužívejte například samostatné název pro každou instanci vygenerované události.

Maximální délka: 512 znaků

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Vlastní měření

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Další kroky

- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- [Psát vlastní události telemetrii](app-insights-api-custom-events-metrics.md#trackevent)
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
