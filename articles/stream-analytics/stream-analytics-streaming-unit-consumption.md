---
title: "Azure Stream Analytics: Optimalizace na efektivně používat jednotek streamování vaše úlohy | Microsoft Docs"
description: "Dotaz osvědčené postupy pro škálování a výkon v Azure Stream Analytics."
keywords: "streamování jednotky, výkon dotazů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 1441a5df4fd92abf85763ca9a1512503b1a0da56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-your-job-to-use-streaming-units-efficiently"></a><span data-ttu-id="ece0a-104">Optimalizace úlohu efektivně používat jednotky streamování</span><span class="sxs-lookup"><span data-stu-id="ece0a-104">Optimize your job to use Streaming Units efficiently</span></span>

<span data-ttu-id="ece0a-105">Azure Stream Analytics agreguje výkonu "váhu" běžící úlohy do jednotek streamování (SUs).</span><span class="sxs-lookup"><span data-stu-id="ece0a-105">Azure Stream Analytics aggregates the performance "weight" of running a job into Streaming Units (SUs).</span></span> <span data-ttu-id="ece0a-106">Služba SUs představují výpočetní prostředky, které jsou využívat k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="ece0a-106">SUs represent the computing resources that are consumed to execute a job.</span></span> <span data-ttu-id="ece0a-107">Jednotky SU umožňují popsat relativní kapacitu zpracování událostí na základě výkonu procesoru, paměti a rychlosti čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-107">SUs provide a way to describe the relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="ece0a-108">Tato kapacita vám umožní soustředit na logiku dotazu a odebere, museli jste z znáte úložiště vrstvy důležité informace o výkonu, přidělit paměť pro úlohu ručně a přibližná core-počet procesorů spouštění úloh v časovém limitu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-108">This capacity lets you focus on the query logic and removes you from needing to know storage tier performance considerations, allocate memory for your job manually, and approximate the CPU core-count needed to run your job in a timely manner.</span></span>

## <a name="how-many-sus-are-required-for-a-job"></a><span data-ttu-id="ece0a-109">Kolik služby SUs jsou potřeba pro úlohu?</span><span class="sxs-lookup"><span data-stu-id="ece0a-109">How many SUs are required for a job?</span></span>

<span data-ttu-id="ece0a-110">Volba číslo požadované služby SUs pro konkrétní úlohy závisí na konfiguraci oddílů pro vstupy a dotaz, který je definován v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="ece0a-110">Choosing the number of required SUs for a particular job depends on the partition configuration for the inputs and the query that's defined within the job.</span></span> <span data-ttu-id="ece0a-111">**Škálování** okně můžete nastavit správný počet služby SUs.</span><span class="sxs-lookup"><span data-stu-id="ece0a-111">The **Scale** blade allows you to set the right number of SUs.</span></span> <span data-ttu-id="ece0a-112">Je osvědčeným postupem přidělit další služby SUs, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="ece0a-112">It is a best practice to allocate more SUs than needed.</span></span> <span data-ttu-id="ece0a-113">Modul služby Stream Analytics zpracování optimalizuje latence a propustnosti cenu přidělování další paměť.</span><span class="sxs-lookup"><span data-stu-id="ece0a-113">The Stream Analytics processing engine optimizes for latency and throughput at the cost of allocating additional memory.</span></span>

<span data-ttu-id="ece0a-114">Obecně platí, osvědčeným postupem je začínat 6 služby SUs pro dotazy, které nepoužívají *PARTITION BY*.</span><span class="sxs-lookup"><span data-stu-id="ece0a-114">In general, the best practice is to start with 6 SUs for queries that don't use *PARTITION BY*.</span></span> <span data-ttu-id="ece0a-115">Pak určete místo paprika pomocí metody omyl a ve kterém můžete změnit počet SUs po předání reprezentativní objemy dat a prozkoumat metriky využití SU %.</span><span class="sxs-lookup"><span data-stu-id="ece0a-115">Then determine the sweet spot by using a trial and error method in which you modify the number of SUs after you pass representative amounts of data and examine the SU %Utilization metric.</span></span>

<span data-ttu-id="ece0a-116">Azure Stream Analytics udržuje v okně "vyrovnávací paměť změnit pořadí" volá se před spuštěním jakékoli zpracovávání událostí.</span><span class="sxs-lookup"><span data-stu-id="ece0a-116">Azure Stream Analytics keeps events in a window called the “reorder buffer” before it starts any processing.</span></span> <span data-ttu-id="ece0a-117">Události jsou v rámci okna změnit pořadí seřazené podle času a další operace jsou prováděny dočasně seřazených událostí.</span><span class="sxs-lookup"><span data-stu-id="ece0a-117">Events are sorted within the reorder window by time, and subsequent operations are performed on the temporally sorted events.</span></span> <span data-ttu-id="ece0a-118">Změna pořadí událostí podle času zajistí, že operátor mít přehled o všech událostí v stanovené časový rámec.</span><span class="sxs-lookup"><span data-stu-id="ece0a-118">Reordering events by time ensures that the operator has visibility into all the events in the stipulated timeframe.</span></span> <span data-ttu-id="ece0a-119">Umožňuje také operátor provést zpracování požadavků a vytváření výstupu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-119">It also lets the operator perform the requisite processing and produce an output.</span></span> <span data-ttu-id="ece0a-120">Vedlejším efektem tohoto mechanismu je, že zpracování je zpožděné o dobu trvání tohoto okna.</span><span class="sxs-lookup"><span data-stu-id="ece0a-120">A side effect of this mechanism is that processing is delayed by the duration of the reorder window.</span></span> <span data-ttu-id="ece0a-121">Nároky na paměť úlohy (která ovlivňuje SU spotřeby) je funkce, velikosti tohoto okna změnit pořadí a počet událostí, které jsou v něm obsažena.</span><span class="sxs-lookup"><span data-stu-id="ece0a-121">The memory footprint of the job (which affects SU consumption) is a function of the size of this reorder window and the number of events contained within it.</span></span>

> [!NOTE]
> <span data-ttu-id="ece0a-122">Při změně počet čtenářů při upgradech úlohy přechodný upozornění se zapisují do protokolů auditů.</span><span class="sxs-lookup"><span data-stu-id="ece0a-122">When the number of readers changes during job upgrades, transient warnings are written to audit logs.</span></span> <span data-ttu-id="ece0a-123">Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="ece0a-123">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a><span data-ttu-id="ece0a-124">Běžné příčiny vysoké paměti na vysoké využití SU pro spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="ece0a-124">Common high-memory causes for high SU usage for running jobs</span></span>

### <a name="high-cardinality-for-group-by"></a><span data-ttu-id="ece0a-125">Vysokou kardinalitou Seskupit podle</span><span class="sxs-lookup"><span data-stu-id="ece0a-125">High cardinality for GROUP BY</span></span>

<span data-ttu-id="ece0a-126">Kardinalita příchozích událostí určuje využití paměti pro příslušnou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-126">The cardinality of incoming events dictates memory usage for the job.</span></span>

<span data-ttu-id="ece0a-127">Například v `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, číslo přiřazené **clusterové** je Kardinalita dotazu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-127">For example, in `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, the number associated with **clustered** is the cardinality of the query.</span></span>

<span data-ttu-id="ece0a-128">Zmírnit problémy, které jsou způsobeny vysokou kardinalitou, škálovat dotaz zvýšením oddíly používající **PARTITION BY**.</span><span class="sxs-lookup"><span data-stu-id="ece0a-128">To mitigate issues that are caused by high cardinality, scale out the query by increasing partitions using **PARTITION BY**.</span></span>

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

<span data-ttu-id="ece0a-129">Počet *clusterové* je mohutnost GROUP BY v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="ece0a-129">The number of *clustered* is the cardinality of GROUP BY here.</span></span>

<span data-ttu-id="ece0a-130">Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="ece0a-130">After the query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="ece0a-131">V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="ece0a-131">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span> <span data-ttu-id="ece0a-132">Oddíly centra událostí by také oddílu podle partitionid.</span><span class="sxs-lookup"><span data-stu-id="ece0a-132">You should also partition event hub partitions by partitionid.</span></span>

### <a name="high-unmatched-event-count-for-join"></a><span data-ttu-id="ece0a-133">Počet událostí vysokou neodpovídající ke spojení</span><span class="sxs-lookup"><span data-stu-id="ece0a-133">High unmatched event count for JOIN</span></span>

<span data-ttu-id="ece0a-134">Počet neodpovídající událostí ve spojení ovlivňuje využití paměti v dotazu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-134">The number of unmatched events in a JOIN affects the memory utilization of the query.</span></span> <span data-ttu-id="ece0a-135">Například proveďte dotaz, který je vyhledávání k nalezení čísla otisky ad, který generuje klikne na:</span><span class="sxs-lookup"><span data-stu-id="ece0a-135">For example, take a query that is looking to find the number of ad impressions that generates clicks:</span></span>

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

<span data-ttu-id="ece0a-136">V tomto scénáři je možné, že se zobrazují mnoho reklamy a jsou generovány několika kliknutími.</span><span class="sxs-lookup"><span data-stu-id="ece0a-136">In this scenario, it is possible that many ads are shown and few clicks are generated.</span></span> <span data-ttu-id="ece0a-137">V důsledku toho by vyžadovaly úlohu chcete zachovat všechny události v rámci časový interval.</span><span class="sxs-lookup"><span data-stu-id="ece0a-137">Such a result would require the job to keep all the events within the time window.</span></span> <span data-ttu-id="ece0a-138">Množství paměti spotřebované je úměrná okno velikost a událostí rychlost.</span><span class="sxs-lookup"><span data-stu-id="ece0a-138">The amount of memory consumed is proportional to the window size and event rate.</span></span> 

<span data-ttu-id="ece0a-139">Pro zmírnění této situaci, škálovat zvýšením oddíly pomocí oddílu pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-139">To mitigate this situation, scale out the query by increasing partitions by using PARTITION BY.</span></span> 

<span data-ttu-id="ece0a-140">Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů zpracování.</span><span class="sxs-lookup"><span data-stu-id="ece0a-140">After the query is partitioned, it is spread out over multiple processing nodes.</span></span> <span data-ttu-id="ece0a-141">V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="ece0a-141">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span>

### <a name="large-number-of-out-of-order-events"></a><span data-ttu-id="ece0a-142">Velké množství mimo pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="ece0a-142">Large number of out of order events</span></span> 

<span data-ttu-id="ece0a-143">Velký počet události mimo pořadí v rámci velké časové okno způsobí, že velikost "vyrovnávací paměti změnit pořadí" musí být větší.</span><span class="sxs-lookup"><span data-stu-id="ece0a-143">A large number of out of order events within a large time window causes the size of the "reorder buffer" to be larger.</span></span> <span data-ttu-id="ece0a-144">Pro zmírnění této situaci, škálovat zvýšením oddíly pomocí oddílu pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="ece0a-144">To mitigate this situation, scale the query by increasing partitions by using PARTITION BY.</span></span> <span data-ttu-id="ece0a-145">Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="ece0a-145">After the query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="ece0a-146">V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="ece0a-146">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span> 


## <a name="get-help"></a><span data-ttu-id="ece0a-147">Podpora</span><span class="sxs-lookup"><span data-stu-id="ece0a-147">Get help</span></span>
<span data-ttu-id="ece0a-148">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ece0a-148">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ece0a-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ece0a-149">Next steps</span></span>
* [<span data-ttu-id="ece0a-150">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ece0a-150">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ece0a-151">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ece0a-151">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ece0a-152">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ece0a-152">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ece0a-153">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ece0a-153">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ece0a-154">Referenční dokumentace Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="ece0a-154">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
