---
title: "aaaAzure aplikace Insights Telemetrie datový Model - Telemetrie výjimek | Microsoft Docs"
description: "Application Insights datový model pro telemetrie výjimek"
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
ms.openlocfilehash: 4c2b7d1ac3816d5623db9a35819a48a68a13a9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Telemetrie výjimek: Application Insights datový model

V [Application Insights](app-insights-overview.md), instance výjimky představuje zpracovávaný nebo neošetřené výjimky, které došlo během provádění hello monitorované aplikace.

## <a name="problem-id"></a>Id problému

Identifikátor kde hello došlo k výjimce v kódu. Použít pro seskupení výjimky. Zásobník volání obvykle kombinací typ výjimky a funkci ze hello.

Maximální délka: 1024 znaků

## <a name="severity-level"></a>Úroveň závažnosti

Úroveň závažnosti trasování. Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Podrobnosti o výjimce

(toobe rozšířené)

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Vlastní měření

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Další kroky

- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Zjistěte, jak příliš[diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights](app-insights-asp-net-exceptions.md).
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
