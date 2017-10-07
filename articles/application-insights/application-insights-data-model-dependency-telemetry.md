---
title: "aaaAzure aplikace Insights Telemetrie datový Model - Telemetrických závislostí | Microsoft Docs"
description: "Application Insights datový model pro telemetrických závislostí"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: bwren
ms.openlocfilehash: cd5ab7c61d3498e4aa2a0aa0c8b0d106a92912e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a><span data-ttu-id="340e3-103">Telemetrických závislostí: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="340e3-103">Dependency telemetry: Application Insights data model</span></span>

<span data-ttu-id="340e3-104">Telemetrických závislostí (v [Application Insights](app-insights-overview.md)) představuje interakci hello monitorované součásti se vzdálené součásti, např. SQL nebo koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="340e3-104">Dependency Telemetry (in [Application Insights](app-insights-overview.md)) represents an interaction of hello monitored component with a remote component such as SQL or an HTTP endpoint.</span></span>

## <a name="name"></a><span data-ttu-id="340e3-105">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="340e3-105">Name</span></span>

<span data-ttu-id="340e3-106">Název příkazu hello iniciovala s toto volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="340e3-106">Name of hello command initiated with this dependency call.</span></span> <span data-ttu-id="340e3-107">Mohutnost nízkou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="340e3-107">Low cardinality value.</span></span> <span data-ttu-id="340e3-108">Příklady jsou název uložené procedury a šablonu cesty URL.</span><span class="sxs-lookup"><span data-stu-id="340e3-108">Examples are stored procedure name and URL path template.</span></span>

## <a name="id"></a><span data-ttu-id="340e3-109">ID</span><span class="sxs-lookup"><span data-stu-id="340e3-109">ID</span></span>

<span data-ttu-id="340e3-110">Identifikátor závislosti volání instance.</span><span class="sxs-lookup"><span data-stu-id="340e3-110">Identifier of a dependency call instance.</span></span> <span data-ttu-id="340e3-111">Slouží pro korelaci s položkou telemetrie požadavek hello odpovídající toothis závislostí volání.</span><span class="sxs-lookup"><span data-stu-id="340e3-111">Used for correlation with hello request telemetry item corresponding toothis dependency call.</span></span> <span data-ttu-id="340e3-112">Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="340e3-112">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="data"></a><span data-ttu-id="340e3-113">Data</span><span class="sxs-lookup"><span data-stu-id="340e3-113">Data</span></span>

<span data-ttu-id="340e3-114">Příkaz iniciovaná toto volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="340e3-114">Command initiated by this dependency call.</span></span> <span data-ttu-id="340e3-115">Příklady jsou příkaz jazyka SQL a adresy URL protokolu HTTP se všemi parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="340e3-115">Examples are SQL statement and HTTP URL with all query parameters.</span></span>

## <a name="type"></a><span data-ttu-id="340e3-116">Typ</span><span class="sxs-lookup"><span data-stu-id="340e3-116">Type</span></span>

<span data-ttu-id="340e3-117">Název typu závislosti.</span><span class="sxs-lookup"><span data-stu-id="340e3-117">Dependency type name.</span></span> <span data-ttu-id="340e3-118">Hodnota nízkou mohutnost pro logické seskupení závislosti a výklad další pole jako commandName a resultCode.</span><span class="sxs-lookup"><span data-stu-id="340e3-118">Low cardinality value for logical grouping of dependencies and interpretation of other fields like commandName and resultCode.</span></span> <span data-ttu-id="340e3-119">Příklady jsou SQL, Azure table a HTTP.</span><span class="sxs-lookup"><span data-stu-id="340e3-119">Examples are SQL, Azure table, and HTTP.</span></span>

## <a name="target"></a><span data-ttu-id="340e3-120">cíl</span><span class="sxs-lookup"><span data-stu-id="340e3-120">Target</span></span>

<span data-ttu-id="340e3-121">Cílovou lokalitu volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="340e3-121">Target site of a dependency call.</span></span> <span data-ttu-id="340e3-122">Příklady jsou název serveru, adresa hostitele.</span><span class="sxs-lookup"><span data-stu-id="340e3-122">Examples are server name, host address.</span></span> <span data-ttu-id="340e3-123">Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="340e3-123">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="duration"></a><span data-ttu-id="340e3-124">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="340e3-124">Duration</span></span>

<span data-ttu-id="340e3-125">Žádosti o trvání ve formátu: `DD.HH:MM:SS.MMMMMM`.</span><span class="sxs-lookup"><span data-stu-id="340e3-125">Request duration in format: `DD.HH:MM:SS.MMMMMM`.</span></span> <span data-ttu-id="340e3-126">Musí být menší než `1000` dnů.</span><span class="sxs-lookup"><span data-stu-id="340e3-126">Must be less than `1000` days.</span></span>

## <a name="result-code"></a><span data-ttu-id="340e3-127">Kód výsledku</span><span class="sxs-lookup"><span data-stu-id="340e3-127">Result code</span></span>

<span data-ttu-id="340e3-128">Kód výsledku volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="340e3-128">Result code of a dependency call.</span></span> <span data-ttu-id="340e3-129">Příklady kódu chyby SQL a stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="340e3-129">Examples are SQL error code and HTTP status code.</span></span>

## <a name="success"></a><span data-ttu-id="340e3-130">Úspěch</span><span class="sxs-lookup"><span data-stu-id="340e3-130">Success</span></span>

<span data-ttu-id="340e3-131">Údaj o volání úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="340e3-131">Indication of successful or unsuccessful call.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="340e3-132">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="340e3-132">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="340e3-133">Vlastní měření</span><span class="sxs-lookup"><span data-stu-id="340e3-133">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a><span data-ttu-id="340e3-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="340e3-134">Next steps</span></span>

- <span data-ttu-id="340e3-135">Nastavení sledování závislostí [.NET](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="340e3-135">Set up dependency tracking for [.NET](app-insights-asp-net-dependencies.md).</span></span>
- <span data-ttu-id="340e3-136">Nastavení sledování závislostí [Java](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="340e3-136">Set up dependency tracking for [Java](app-insights-java-agent.md).</span></span>
- [<span data-ttu-id="340e3-137">Psát vlastní závislosti telemetrii</span><span class="sxs-lookup"><span data-stu-id="340e3-137">Write custom dependency telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackdependency)
- <span data-ttu-id="340e3-138">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="340e3-138">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="340e3-139">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="340e3-139">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
