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
# <a name="event-telemetry-application-insights-data-model"></a><span data-ttu-id="c92d2-103">Událost telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="c92d2-103">Event telemetry: Application Insights data model</span></span>

<span data-ttu-id="c92d2-104">Události můžete vytvořit položky telemetrie (v [Application Insights](app-insights-overview.md)) toorepresent událost, která v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c92d2-104">You can create event telemetry items (in [Application Insights](app-insights-overview.md)) toorepresent an event that occurred in your application.</span></span> <span data-ttu-id="c92d2-105">Obvykle je zásahu uživatele, jako je tlačítko klikněte na tlačítko nebo pořadí checkout.</span><span class="sxs-lookup"><span data-stu-id="c92d2-105">Typically it is a user interaction such as button click or order checkout.</span></span> <span data-ttu-id="c92d2-106">Může být také událost životní cyklus aplikace jako aktualizace inicializace nebo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c92d2-106">It can also be an application life cycle event like initialization or configuration update.</span></span> 

<span data-ttu-id="c92d2-107">Události sémanticky, může nebo nemusí být korelační toorequests.</span><span class="sxs-lookup"><span data-stu-id="c92d2-107">Semantically, events may or may not be correlated toorequests.</span></span> <span data-ttu-id="c92d2-108">Ale pokud používá správně, telemetrie událostí je důležitější než požadavky nebo trasování.</span><span class="sxs-lookup"><span data-stu-id="c92d2-108">However, if used properly, event telemetry is more important than requests or traces.</span></span> <span data-ttu-id="c92d2-109">Události představují obchodní telemetrie a měl by být tooseparate předmětu, méně agresivní [vzorkování](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="c92d2-109">Events represent business telemetry and should be a subject tooseparate, less aggressive [sampling](app-insights-api-filtering-sampling.md).</span></span>

## <a name="name"></a><span data-ttu-id="c92d2-110">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c92d2-110">Name</span></span>

<span data-ttu-id="c92d2-111">Název události.</span><span class="sxs-lookup"><span data-stu-id="c92d2-111">Event name.</span></span> <span data-ttu-id="c92d2-112">tooallow správné seskupení a užitečné metriky, omezit aplikace tak, aby vygeneruje malý počet názvy samostatná událost.</span><span class="sxs-lookup"><span data-stu-id="c92d2-112">tooallow proper grouping and useful metrics, restrict your application so that it generates a small number of separate event names.</span></span> <span data-ttu-id="c92d2-113">Nepoužívejte například samostatné název pro každou instanci vygenerované události.</span><span class="sxs-lookup"><span data-stu-id="c92d2-113">For example, don't use a separate name for each generated instance of an event.</span></span>

<span data-ttu-id="c92d2-114">Maximální délka: 512 znaků</span><span class="sxs-lookup"><span data-stu-id="c92d2-114">Max length: 512 characters</span></span>

## <a name="custom-properties"></a><span data-ttu-id="c92d2-115">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c92d2-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="c92d2-116">Vlastní měření</span><span class="sxs-lookup"><span data-stu-id="c92d2-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="c92d2-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c92d2-117">Next steps</span></span>

- <span data-ttu-id="c92d2-118">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="c92d2-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="c92d2-119">Psát vlastní události telemetrii</span><span class="sxs-lookup"><span data-stu-id="c92d2-119">Write custom event telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackevent)
- <span data-ttu-id="c92d2-120">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c92d2-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
