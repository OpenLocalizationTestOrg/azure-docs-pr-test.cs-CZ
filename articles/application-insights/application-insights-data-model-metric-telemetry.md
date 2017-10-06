---
title: "aaaAzure aplikace Insights Telemetrie datový Model - metrika Telemetrie | Microsoft Docs"
description: "Application Insights datový model pro metriky telemetrie"
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
ms.openlocfilehash: 005e218a8451007458185f1e457a20cee93fa630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a><span data-ttu-id="20df3-103">Metriky telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="20df3-103">Metric telemetry: Application Insights data model</span></span>

<span data-ttu-id="20df3-104">Existují dva typy metriky telemetrie nepodporuje [Application Insights](app-insights-overview.md): jeden měření a předem agregovaná metrika.</span><span class="sxs-lookup"><span data-stu-id="20df3-104">There are two types of metric telemetry supported by [Application Insights](app-insights-overview.md): single measurement and pre-aggregated metric.</span></span> <span data-ttu-id="20df3-105">Jedno měření je právě název a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="20df3-105">Single measurement is just a name and value.</span></span> <span data-ttu-id="20df3-106">Předem agregovaná metrika určuje minimální a maximální hodnotou hello metriky v hello jako interval agregace a směrodatné odchylky ho.</span><span class="sxs-lookup"><span data-stu-id="20df3-106">Pre-aggregated metric specifies minimum and maximum value of hello metric in hello aggregation interval and standard deviation of it.</span></span>

<span data-ttu-id="20df3-107">Předem agregované metriky telemetrie předpokládá toto období agregace byl jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="20df3-107">Pre-aggregated metric telemetry assumes that aggregation period was one minute.</span></span>

<span data-ttu-id="20df3-108">Existuje několik dobře známé metriky jmen nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="20df3-108">There are several well-known metric names supported by Application Insights.</span></span> 

<span data-ttu-id="20df3-109">Metrika představující systém a proces čítače:</span><span class="sxs-lookup"><span data-stu-id="20df3-109">Metric representing system and process counters:</span></span>

| <span data-ttu-id="20df3-110">**Název rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="20df3-110">**.NET name**</span></span>             | <span data-ttu-id="20df3-111">**Název lhostejné platformy**</span><span class="sxs-lookup"><span data-stu-id="20df3-111">**Platform agnostic name**</span></span> | <span data-ttu-id="20df3-112">**Název REST API**</span><span class="sxs-lookup"><span data-stu-id="20df3-112">**REST API name**</span></span> | <span data-ttu-id="20df3-113">**Popis**</span><span class="sxs-lookup"><span data-stu-id="20df3-113">**Description**</span></span>
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | <span data-ttu-id="20df3-114">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-114">Work in progress...</span></span> | [<span data-ttu-id="20df3-115">processorCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="20df3-115">processorCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | <span data-ttu-id="20df3-116">Celkový počet počítačů procesoru</span><span class="sxs-lookup"><span data-stu-id="20df3-116">total machine CPU</span></span>
| `\Memory\Available Bytes`                 | <span data-ttu-id="20df3-117">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-117">Work in progress...</span></span> | [<span data-ttu-id="20df3-118">memoryAvailableBytes</span><span class="sxs-lookup"><span data-stu-id="20df3-118">memoryAvailableBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | <span data-ttu-id="20df3-119">paměti k dispozici na disku</span><span class="sxs-lookup"><span data-stu-id="20df3-119">memory available on disk</span></span>
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | <span data-ttu-id="20df3-120">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-120">Work in progress...</span></span> | [<span data-ttu-id="20df3-121">processCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="20df3-121">processCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | <span data-ttu-id="20df3-122">Procesor hello procesu, který je hostitelem aplikace hello</span><span class="sxs-lookup"><span data-stu-id="20df3-122">CPU of hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | <span data-ttu-id="20df3-123">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-123">Work in progress...</span></span> | [<span data-ttu-id="20df3-124">processPrivateBytes</span><span class="sxs-lookup"><span data-stu-id="20df3-124">processPrivateBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | <span data-ttu-id="20df3-125">využité procesem hello hostování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="20df3-125">memory used by hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | <span data-ttu-id="20df3-126">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-126">Work in progress...</span></span> | [<span data-ttu-id="20df3-127">processIOBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="20df3-127">processIOBytesPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | <span data-ttu-id="20df3-128">Počet vstupně-výstupních operací se spustí proces, který je hostitelem aplikace hello</span><span class="sxs-lookup"><span data-stu-id="20df3-128">rate of I/O operations runs by process hosting hello application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | <span data-ttu-id="20df3-129">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-129">Work in progress...</span></span> | [<span data-ttu-id="20df3-130">requestsPerSecond</span><span class="sxs-lookup"><span data-stu-id="20df3-130">requestsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | <span data-ttu-id="20df3-131">Míra požadavků zpracovaných aplikací</span><span class="sxs-lookup"><span data-stu-id="20df3-131">rate of requests processed by application</span></span> 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | <span data-ttu-id="20df3-132">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-132">Work in progress...</span></span> | [<span data-ttu-id="20df3-133">exceptionsPerSecond</span><span class="sxs-lookup"><span data-stu-id="20df3-133">exceptionsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | <span data-ttu-id="20df3-134">počet výjimek vyvolaných aplikace</span><span class="sxs-lookup"><span data-stu-id="20df3-134">rate of exceptions thrown by application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | <span data-ttu-id="20df3-135">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-135">Work in progress...</span></span> | [<span data-ttu-id="20df3-136">requestExecutionTime</span><span class="sxs-lookup"><span data-stu-id="20df3-136">requestExecutionTime</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | <span data-ttu-id="20df3-137">čas spuštění průměrný počet požadavků</span><span class="sxs-lookup"><span data-stu-id="20df3-137">average requests execution time</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | <span data-ttu-id="20df3-138">Probíhající práce...</span><span class="sxs-lookup"><span data-stu-id="20df3-138">Work in progress...</span></span> | [<span data-ttu-id="20df3-139">requestsInQueue</span><span class="sxs-lookup"><span data-stu-id="20df3-139">requestsInQueue</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | <span data-ttu-id="20df3-140">Počet požadavků čekajících na zpracování ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="20df3-140">number of requests waiting for hello processing in a queue</span></span>

## <a name="name"></a><span data-ttu-id="20df3-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="20df3-141">Name</span></span>

<span data-ttu-id="20df3-142">Název metriky hello byste chtěli toosee v portálu Application Insights a uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="20df3-142">Name of hello metric you'd like toosee in Application Insights portal and UI.</span></span> 

## <a name="value"></a><span data-ttu-id="20df3-143">Hodnota</span><span class="sxs-lookup"><span data-stu-id="20df3-143">Value</span></span>

<span data-ttu-id="20df3-144">Jednu hodnotu pro měření.</span><span class="sxs-lookup"><span data-stu-id="20df3-144">Single value for measurement.</span></span> <span data-ttu-id="20df3-145">Součet jednotlivými měřeními za účelem agregace hello.</span><span class="sxs-lookup"><span data-stu-id="20df3-145">Sum of individual measurements for hello aggregation.</span></span>

## <a name="count"></a><span data-ttu-id="20df3-146">Počet</span><span class="sxs-lookup"><span data-stu-id="20df3-146">Count</span></span>

<span data-ttu-id="20df3-147">Váha metriky hello agregovat metriky.</span><span class="sxs-lookup"><span data-stu-id="20df3-147">Metric weight of hello aggregated metric.</span></span> <span data-ttu-id="20df3-148">Neměla by být nastavená pro měření.</span><span class="sxs-lookup"><span data-stu-id="20df3-148">Should not be set for a measurement.</span></span>

## <a name="min"></a><span data-ttu-id="20df3-149">Min.</span><span class="sxs-lookup"><span data-stu-id="20df3-149">Min</span></span>

<span data-ttu-id="20df3-150">Minimální hodnota metrika hello agregovat.</span><span class="sxs-lookup"><span data-stu-id="20df3-150">Minimum value of hello aggregated metric.</span></span> <span data-ttu-id="20df3-151">Neměla by být nastavená pro měření.</span><span class="sxs-lookup"><span data-stu-id="20df3-151">Should not be set for a measurement.</span></span>

## <a name="max"></a><span data-ttu-id="20df3-152">Max.</span><span class="sxs-lookup"><span data-stu-id="20df3-152">Max</span></span>

<span data-ttu-id="20df3-153">Maximální hodnota, která hello agregovaná metrika.</span><span class="sxs-lookup"><span data-stu-id="20df3-153">Maximum value of hello aggregated metric.</span></span> <span data-ttu-id="20df3-154">Neměla by být nastavená pro měření.</span><span class="sxs-lookup"><span data-stu-id="20df3-154">Should not be set for a measurement.</span></span>

## <a name="standard-deviation"></a><span data-ttu-id="20df3-155">Směrodatná odchylka</span><span class="sxs-lookup"><span data-stu-id="20df3-155">Standard deviation</span></span>

<span data-ttu-id="20df3-156">Standardní odchylka metrika hello agregovat.</span><span class="sxs-lookup"><span data-stu-id="20df3-156">Standard deviation of hello aggregated metric.</span></span> <span data-ttu-id="20df3-157">Neměla by být nastavená pro měření.</span><span class="sxs-lookup"><span data-stu-id="20df3-157">Should not be set for a measurement.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="20df3-158">Vlastní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="20df3-158">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="20df3-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20df3-159">Next steps</span></span>

- <span data-ttu-id="20df3-160">Zjistěte, jak toouse [Application Insights API pro vlastní události a metriky](app-insights-api-custom-events-metrics.md#trackmetric).</span><span class="sxs-lookup"><span data-stu-id="20df3-160">Learn how toouse [Application Insights API for custom events and metrics](app-insights-api-custom-events-metrics.md#trackmetric).</span></span>
- <span data-ttu-id="20df3-161">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="20df3-161">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="20df3-162">Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="20df3-162">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
