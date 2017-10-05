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
# <a name="trace-telemetry-application-insights-data-model"></a><span data-ttu-id="05e31-103">Trasování telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="05e31-103">Trace telemetry: Application Insights data model</span></span>

<span data-ttu-id="05e31-104">Trasování telemetrie (v [Application Insights](app-insights-overview.md)) představuje `printf` styl příkazy trasování, které jsou prohledávat text.</span><span class="sxs-lookup"><span data-stu-id="05e31-104">Trace telemetry (in [Application Insights](app-insights-overview.md)) represents `printf` style trace statements that are text-searched.</span></span> <span data-ttu-id="05e31-105">`Log4Net`, `NLog`, a další položky založený na textu protokolu jsou převedeny do instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="05e31-105">`Log4Net`, `NLog`, and other text-based log file entries are translated into instances of this type.</span></span> <span data-ttu-id="05e31-106">Trasování nemá měření jako rozšíření.</span><span class="sxs-lookup"><span data-stu-id="05e31-106">The trace does not have measurements as an extensibility.</span></span>

## <a name="message"></a><span data-ttu-id="05e31-107">Zpráva</span><span class="sxs-lookup"><span data-stu-id="05e31-107">Message</span></span>

<span data-ttu-id="05e31-108">Zpráva trasování.</span><span class="sxs-lookup"><span data-stu-id="05e31-108">Trace message.</span></span>

<span data-ttu-id="05e31-109">Maximální délka: 32768 znaků</span><span class="sxs-lookup"><span data-stu-id="05e31-109">Max length: 32768 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="05e31-110">Úroveň závažnosti</span><span class="sxs-lookup"><span data-stu-id="05e31-110">Severity level</span></span>

<span data-ttu-id="05e31-111">Úroveň závažnosti trasování.</span><span class="sxs-lookup"><span data-stu-id="05e31-111">Trace severity level.</span></span> <span data-ttu-id="05e31-112">Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="05e31-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="05e31-113">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="05e31-113">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="05e31-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05e31-114">Next steps</span></span>

- <span data-ttu-id="05e31-115">[Prozkoumejte protokoly trasování .NET ve službě Application Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="05e31-115">[Explore .NET trace logs in Application Insights](app-insights-asp-net-trace-logs.md).</span></span>
- <span data-ttu-id="05e31-116">[Prozkoumejte Java protokolů trasování ve službě Application Insights](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="05e31-116">[Explore Java trace logs in Application Insights](app-insights-java-trace-logs.md).</span></span>
- <span data-ttu-id="05e31-117">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="05e31-117">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="05e31-118">Psát vlastní trasování telemetrii</span><span class="sxs-lookup"><span data-stu-id="05e31-118">Write custom trace telemetry</span></span>](app-insights-api-custom-events-metrics.md#tracktrace)
- <span data-ttu-id="05e31-119">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="05e31-119">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
