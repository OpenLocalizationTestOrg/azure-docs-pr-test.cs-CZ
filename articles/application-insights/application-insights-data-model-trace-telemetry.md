---
title: "Aplikace Azure přehled Telemetrie datový Model - trasování Telemetrie | Microsoft Docs"
description: "Application Insights datový model pro trasování telemetrie"
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
ms.openlocfilehash: e1da0d6a6fbd9ca5486936c326ade667d7b01006
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="trace-telemetry-application-insights-data-model"></a>Trasování telemetrie: Application Insights datový model

Trasování telemetrie (v [Application Insights](app-insights-overview.md)) představuje `printf` styl příkazy trasování, které jsou prohledávat text. `Log4Net`, `NLog`, a další položky založený na textu protokolu jsou převedeny do instance tohoto typu. Trasování nemá měření jako rozšíření.

## <a name="message"></a>Zpráva

Zpráva trasování.

Maximální délka: 32768 znaků

## <a name="severity-level"></a>Úroveň závažnosti

Úroveň závažnosti trasování. Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Další kroky

- [Prozkoumejte protokoly trasování .NET ve službě Application Insights](app-insights-asp-net-trace-logs.md).
- [Prozkoumejte Java protokolů trasování ve službě Application Insights](app-insights-java-trace-logs.md).
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- [Psát vlastní trasování telemetrii](app-insights-api-custom-events-metrics.md#tracktrace)
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
