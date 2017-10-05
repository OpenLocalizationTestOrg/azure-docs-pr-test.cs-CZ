---
title: "Datový Model aplikace Azure Statistika Telemetrie - Telemetrie výjimek | Microsoft Docs"
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
ms.openlocfilehash: 6b220b0cb6719bac606f599d657d08ab847c7590
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a><span data-ttu-id="62fb9-103">Telemetrie výjimek: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="62fb9-103">Exception telemetry: Application Insights data model</span></span>

<span data-ttu-id="62fb9-104">V [Application Insights](app-insights-overview.md), představuje instanci výjimky zpracované nebo neošetřené výjimky, které došlo během provádění v monitorované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="62fb9-104">In [Application Insights](app-insights-overview.md), an instance of Exception represents a handled or unhandled exception that occurred during execution of the monitored application.</span></span>

## <a name="problem-id"></a><span data-ttu-id="62fb9-105">Id problému</span><span class="sxs-lookup"><span data-stu-id="62fb9-105">Problem Id</span></span>

<span data-ttu-id="62fb9-106">Identifikátor, kde byla výjimka vydána v kódu.</span><span class="sxs-lookup"><span data-stu-id="62fb9-106">Identifier of where the exception was thrown in code.</span></span> <span data-ttu-id="62fb9-107">Použít pro seskupení výjimky.</span><span class="sxs-lookup"><span data-stu-id="62fb9-107">Used for exceptions grouping.</span></span> <span data-ttu-id="62fb9-108">Obvykle kombinace typ výjimky a funkce v zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="62fb9-108">Typically a combination of exception type and a function from the call stack.</span></span>

<span data-ttu-id="62fb9-109">Maximální délka: 1024 znaků</span><span class="sxs-lookup"><span data-stu-id="62fb9-109">Max length: 1024 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="62fb9-110">Úroveň závažnosti</span><span class="sxs-lookup"><span data-stu-id="62fb9-110">Severity level</span></span>

<span data-ttu-id="62fb9-111">Úroveň závažnosti trasování.</span><span class="sxs-lookup"><span data-stu-id="62fb9-111">Trace severity level.</span></span> <span data-ttu-id="62fb9-112">Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="62fb9-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="exception-details"></a><span data-ttu-id="62fb9-113">Podrobnosti o výjimce</span><span class="sxs-lookup"><span data-stu-id="62fb9-113">Exception details</span></span>

<span data-ttu-id="62fb9-114">(Chcete-li být rozšířené)</span><span class="sxs-lookup"><span data-stu-id="62fb9-114">(To be extended)</span></span>

## <a name="custom-properties"></a><span data-ttu-id="62fb9-115">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="62fb9-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="62fb9-116">Vlastní měření</span><span class="sxs-lookup"><span data-stu-id="62fb9-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="62fb9-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62fb9-117">Next steps</span></span>

- <span data-ttu-id="62fb9-118">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="62fb9-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="62fb9-119">Zjistěte, jak [diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="62fb9-119">Learn how to [diagnose exceptions in your web apps with Application Insights](app-insights-asp-net-exceptions.md).</span></span>
- <span data-ttu-id="62fb9-120">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="62fb9-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
