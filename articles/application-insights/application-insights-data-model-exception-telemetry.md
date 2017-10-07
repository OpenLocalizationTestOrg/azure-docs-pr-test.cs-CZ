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
# <a name="exception-telemetry-application-insights-data-model"></a><span data-ttu-id="8780c-103">Telemetrie výjimek: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="8780c-103">Exception telemetry: Application Insights data model</span></span>

<span data-ttu-id="8780c-104">V [Application Insights](app-insights-overview.md), instance výjimky představuje zpracovávaný nebo neošetřené výjimky, které došlo během provádění hello monitorované aplikace.</span><span class="sxs-lookup"><span data-stu-id="8780c-104">In [Application Insights](app-insights-overview.md), an instance of Exception represents a handled or unhandled exception that occurred during execution of hello monitored application.</span></span>

## <a name="problem-id"></a><span data-ttu-id="8780c-105">Id problému</span><span class="sxs-lookup"><span data-stu-id="8780c-105">Problem Id</span></span>

<span data-ttu-id="8780c-106">Identifikátor kde hello došlo k výjimce v kódu.</span><span class="sxs-lookup"><span data-stu-id="8780c-106">Identifier of where hello exception was thrown in code.</span></span> <span data-ttu-id="8780c-107">Použít pro seskupení výjimky.</span><span class="sxs-lookup"><span data-stu-id="8780c-107">Used for exceptions grouping.</span></span> <span data-ttu-id="8780c-108">Zásobník volání obvykle kombinací typ výjimky a funkci ze hello.</span><span class="sxs-lookup"><span data-stu-id="8780c-108">Typically a combination of exception type and a function from hello call stack.</span></span>

<span data-ttu-id="8780c-109">Maximální délka: 1024 znaků</span><span class="sxs-lookup"><span data-stu-id="8780c-109">Max length: 1024 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="8780c-110">Úroveň závažnosti</span><span class="sxs-lookup"><span data-stu-id="8780c-110">Severity level</span></span>

<span data-ttu-id="8780c-111">Úroveň závažnosti trasování.</span><span class="sxs-lookup"><span data-stu-id="8780c-111">Trace severity level.</span></span> <span data-ttu-id="8780c-112">Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="8780c-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="exception-details"></a><span data-ttu-id="8780c-113">Podrobnosti o výjimce</span><span class="sxs-lookup"><span data-stu-id="8780c-113">Exception details</span></span>

<span data-ttu-id="8780c-114">(toobe rozšířené)</span><span class="sxs-lookup"><span data-stu-id="8780c-114">(toobe extended)</span></span>

## <a name="custom-properties"></a><span data-ttu-id="8780c-115">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="8780c-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="8780c-116">Vlastní měření</span><span class="sxs-lookup"><span data-stu-id="8780c-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="8780c-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8780c-117">Next steps</span></span>

- <span data-ttu-id="8780c-118">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="8780c-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="8780c-119">Zjistěte, jak příliš[diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="8780c-119">Learn how too[diagnose exceptions in your web apps with Application Insights](app-insights-asp-net-exceptions.md).</span></span>
- <span data-ttu-id="8780c-120">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8780c-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
