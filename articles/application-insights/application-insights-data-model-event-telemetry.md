---
title: "Aplikace Azure Statistika Telemetrie datový Model - událostí Telemetrie | Microsoft Docs"
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
ms.openlocfilehash: 422e193ae10938954602a6ef8c49fd47f473bc01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-telemetry-application-insights-data-model"></a><span data-ttu-id="6f077-103">Událost telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="6f077-103">Event telemetry: Application Insights data model</span></span>

<span data-ttu-id="6f077-104">Události můžete vytvořit položky telemetrie (v [Application Insights](app-insights-overview.md)) představují událost, která v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f077-104">You can create event telemetry items (in [Application Insights](app-insights-overview.md)) to represent an event that occurred in your application.</span></span> <span data-ttu-id="6f077-105">Obvykle je zásahu uživatele, jako je tlačítko klikněte na tlačítko nebo pořadí checkout.</span><span class="sxs-lookup"><span data-stu-id="6f077-105">Typically it is a user interaction such as button click or order checkout.</span></span> <span data-ttu-id="6f077-106">Může být také událost životní cyklus aplikace jako aktualizace inicializace nebo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6f077-106">It can also be an application life cycle event like initialization or configuration update.</span></span> 

<span data-ttu-id="6f077-107">Události sémanticky, může nebo nemusí být korelační na požadavky.</span><span class="sxs-lookup"><span data-stu-id="6f077-107">Semantically, events may or may not be correlated to requests.</span></span> <span data-ttu-id="6f077-108">Ale pokud používá správně, telemetrie událostí je důležitější než požadavky nebo trasování.</span><span class="sxs-lookup"><span data-stu-id="6f077-108">However, if used properly, event telemetry is more important than requests or traces.</span></span> <span data-ttu-id="6f077-109">Události představují obchodní telemetrie a měl by být subjektu k oddělení, méně agresivní [vzorkování](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="6f077-109">Events represent business telemetry and should be a subject to separate, less aggressive [sampling](app-insights-api-filtering-sampling.md).</span></span>

## <a name="name"></a><span data-ttu-id="6f077-110">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6f077-110">Name</span></span>

<span data-ttu-id="6f077-111">Název události.</span><span class="sxs-lookup"><span data-stu-id="6f077-111">Event name.</span></span> <span data-ttu-id="6f077-112">Chcete-li povolit správné seskupení a užitečné metriky, omezte aplikace tak, aby vygeneruje malý počet názvy samostatná událost.</span><span class="sxs-lookup"><span data-stu-id="6f077-112">To allow proper grouping and useful metrics, restrict your application so that it generates a small number of separate event names.</span></span> <span data-ttu-id="6f077-113">Nepoužívejte například samostatné název pro každou instanci vygenerované události.</span><span class="sxs-lookup"><span data-stu-id="6f077-113">For example, don't use a separate name for each generated instance of an event.</span></span>

<span data-ttu-id="6f077-114">Maximální délka: 512 znaků</span><span class="sxs-lookup"><span data-stu-id="6f077-114">Max length: 512 characters</span></span>

## <a name="custom-properties"></a><span data-ttu-id="6f077-115">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6f077-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="6f077-116">Vlastní měření</span><span class="sxs-lookup"><span data-stu-id="6f077-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="6f077-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f077-117">Next steps</span></span>

- <span data-ttu-id="6f077-118">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="6f077-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="6f077-119">Psát vlastní události telemetrii</span><span class="sxs-lookup"><span data-stu-id="6f077-119">Write custom event telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackevent)
- <span data-ttu-id="6f077-120">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6f077-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
