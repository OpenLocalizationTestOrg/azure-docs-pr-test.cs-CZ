---
title: "Azure Stream Analytics: Optimalizace vaše úlohy toouse jednotek Streaming efektivně | Microsoft Docs"
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
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a><span data-ttu-id="f9820-104">Efektivní optimalizovat vaše úlohy toouse jednotek Streaming</span><span class="sxs-lookup"><span data-stu-id="f9820-104">Optimize your job toouse Streaming Units efficiently</span></span>

<span data-ttu-id="f9820-105">Azure Stream Analytics agreguje hello výkonu "váhu" běžící úlohy do jednotek streamování (SUs).</span><span class="sxs-lookup"><span data-stu-id="f9820-105">Azure Stream Analytics aggregates hello performance "weight" of running a job into Streaming Units (SUs).</span></span> <span data-ttu-id="f9820-106">Služba SUs představují hello výpočetní prostředky, které jsou spotřebované tooexecute úlohu.</span><span class="sxs-lookup"><span data-stu-id="f9820-106">SUs represent hello computing resources that are consumed tooexecute a job.</span></span> <span data-ttu-id="f9820-107">Služba SUs poskytují způsob toodescribe hello relativní zpracování událostí založené na kombinaci měření procesoru, paměti, kapacity a čtení a zápisu sazby.</span><span class="sxs-lookup"><span data-stu-id="f9820-107">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="f9820-108">Tato kapacita vám umožní soustředit na logiku hello dotazu a odebere z tooknow aspekty výkonu vrstvy úložiště, které se musí přidělit paměť pro úlohu ručně a přibližná hello procesoru core-počet potřeby toorun úlohu v časovém limitu.</span><span class="sxs-lookup"><span data-stu-id="f9820-108">This capacity lets you focus on hello query logic and removes you from needing tooknow storage tier performance considerations, allocate memory for your job manually, and approximate hello CPU core-count needed toorun your job in a timely manner.</span></span>

## <a name="how-many-sus-are-required-for-a-job"></a><span data-ttu-id="f9820-109">Kolik služby SUs jsou potřeba pro úlohu?</span><span class="sxs-lookup"><span data-stu-id="f9820-109">How many SUs are required for a job?</span></span>

<span data-ttu-id="f9820-110">Volba hello počet požadované služby SUs pro konkrétní úlohy závisí na konfiguraci hello oddílů pro vstupy hello a hello dotazu, který je definován v rámci úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="f9820-110">Choosing hello number of required SUs for a particular job depends on hello partition configuration for hello inputs and hello query that's defined within hello job.</span></span> <span data-ttu-id="f9820-111">Hello **škálování** okno vám umožní tooset hello právo počet služby SUs.</span><span class="sxs-lookup"><span data-stu-id="f9820-111">hello **Scale** blade allows you tooset hello right number of SUs.</span></span> <span data-ttu-id="f9820-112">Je představuje dobrou praxi tooallocate další služby SUs, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="f9820-112">It is a best practice tooallocate more SUs than needed.</span></span> <span data-ttu-id="f9820-113">modul zpracování Stream Analytics Hello optimalizuje latence a propustnosti na hello náklady přidělování další paměť.</span><span class="sxs-lookup"><span data-stu-id="f9820-113">hello Stream Analytics processing engine optimizes for latency and throughput at hello cost of allocating additional memory.</span></span>

<span data-ttu-id="f9820-114">Obecně platí, hello osvědčeným postupem je toostart s 6 služby SUs pro dotazy, které nepoužívají *PARTITION BY*.</span><span class="sxs-lookup"><span data-stu-id="f9820-114">In general, hello best practice is toostart with 6 SUs for queries that don't use *PARTITION BY*.</span></span> <span data-ttu-id="f9820-115">Pak určete hello paprika místo pomocí metody omyl a ve kterém můžete upravit hello počet služby SUs po předání reprezentativní objemy dat a prozkoumat hello SU % využití metrika.</span><span class="sxs-lookup"><span data-stu-id="f9820-115">Then determine hello sweet spot by using a trial and error method in which you modify hello number of SUs after you pass representative amounts of data and examine hello SU %Utilization metric.</span></span>

<span data-ttu-id="f9820-116">Azure Stream Analytics udržuje v okně hello "změnit pořadí vyrovnávací paměti" volá se před spuštěním jakékoli zpracovávání událostí.</span><span class="sxs-lookup"><span data-stu-id="f9820-116">Azure Stream Analytics keeps events in a window called hello “reorder buffer” before it starts any processing.</span></span> <span data-ttu-id="f9820-117">Události jsou v rámci okna změnit pořadí hello seřazené podle času a další operace jsou prováděny v hello dočasně seřadit události.</span><span class="sxs-lookup"><span data-stu-id="f9820-117">Events are sorted within hello reorder window by time, and subsequent operations are performed on hello temporally sorted events.</span></span> <span data-ttu-id="f9820-118">Změna pořadí událostí podle času zajistí, že hello operátor má získat přehled o všech hello události v hello stanovený časový rámec.</span><span class="sxs-lookup"><span data-stu-id="f9820-118">Reordering events by time ensures that hello operator has visibility into all hello events in hello stipulated timeframe.</span></span> <span data-ttu-id="f9820-119">Umožňuje také operátor hello provádějí hello zpracování požadavků a vytvářejí výstup.</span><span class="sxs-lookup"><span data-stu-id="f9820-119">It also lets hello operator perform hello requisite processing and produce an output.</span></span> <span data-ttu-id="f9820-120">Vedlejším účinkem tento mechanismus je, že je zpracování zpožděn hello doba trvání okna hello změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="f9820-120">A side effect of this mechanism is that processing is delayed by hello duration of hello reorder window.</span></span> <span data-ttu-id="f9820-121">spotřeba paměti Hello hello úlohy (která ovlivňuje SU spotřeby) je funkce hello velikosti toto změnit pořadí událostí, které jsou v něm obsažena číslo okno a hello.</span><span class="sxs-lookup"><span data-stu-id="f9820-121">hello memory footprint of hello job (which affects SU consumption) is a function of hello size of this reorder window and hello number of events contained within it.</span></span>

> [!NOTE]
> <span data-ttu-id="f9820-122">Při změně hello počet čtenářů při upgradech úlohy se zapisují protokoly tooaudit přechodný upozornění.</span><span class="sxs-lookup"><span data-stu-id="f9820-122">When hello number of readers changes during job upgrades, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="f9820-123">Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="f9820-123">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a><span data-ttu-id="f9820-124">Běžné příčiny vysoké paměti na vysoké využití SU pro spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="f9820-124">Common high-memory causes for high SU usage for running jobs</span></span>

### <a name="high-cardinality-for-group-by"></a><span data-ttu-id="f9820-125">Vysokou kardinalitou Seskupit podle</span><span class="sxs-lookup"><span data-stu-id="f9820-125">High cardinality for GROUP BY</span></span>

<span data-ttu-id="f9820-126">Mohutnost Hello příchozích událostí, které určuje využití paměti pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="f9820-126">hello cardinality of incoming events dictates memory usage for hello job.</span></span>

<span data-ttu-id="f9820-127">Například v `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello číslo přidružené k **clusterové** je Kardinalita hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="f9820-127">For example, in `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello number associated with **clustered** is hello cardinality of hello query.</span></span>

<span data-ttu-id="f9820-128">toomitigate problémy, které jsou způsobeny vysokou kardinalitou škálovat hello dotazu zvýšením oddíly používající **PARTITION BY**.</span><span class="sxs-lookup"><span data-stu-id="f9820-128">toomitigate issues that are caused by high cardinality, scale out hello query by increasing partitions using **PARTITION BY**.</span></span>

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

<span data-ttu-id="f9820-129">Hello počet *clusterové* je hello Kardinalita GROUP BY v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="f9820-129">hello number of *clustered* is hello cardinality of GROUP BY here.</span></span>

<span data-ttu-id="f9820-130">Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="f9820-130">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="f9820-131">V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="f9820-131">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> <span data-ttu-id="f9820-132">Oddíly centra událostí by také oddílu podle partitionid.</span><span class="sxs-lookup"><span data-stu-id="f9820-132">You should also partition event hub partitions by partitionid.</span></span>

### <a name="high-unmatched-event-count-for-join"></a><span data-ttu-id="f9820-133">Počet událostí vysokou neodpovídající ke spojení</span><span class="sxs-lookup"><span data-stu-id="f9820-133">High unmatched event count for JOIN</span></span>

<span data-ttu-id="f9820-134">Hello počet neodpovídající událostí ve spojení ovlivňuje využití paměti hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="f9820-134">hello number of unmatched events in a JOIN affects hello memory utilization of hello query.</span></span> <span data-ttu-id="f9820-135">Například proveďte dotaz, který je vyhledávání toofind hello počet otisky ad vygeneruje klikne na:</span><span class="sxs-lookup"><span data-stu-id="f9820-135">For example, take a query that is looking toofind hello number of ad impressions that generates clicks:</span></span>

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

<span data-ttu-id="f9820-136">V tomto scénáři je možné, že se zobrazují mnoho reklamy a jsou generovány několika kliknutími.</span><span class="sxs-lookup"><span data-stu-id="f9820-136">In this scenario, it is possible that many ads are shown and few clicks are generated.</span></span> <span data-ttu-id="f9820-137">V důsledku toho by vyžadovaly hello úlohy tookeep všechny hello události v rámci hello časový interval.</span><span class="sxs-lookup"><span data-stu-id="f9820-137">Such a result would require hello job tookeep all hello events within hello time window.</span></span> <span data-ttu-id="f9820-138">Hello množství paměti spotřebované je přímo úměrná toohello okno velikost a událostí rychlost.</span><span class="sxs-lookup"><span data-stu-id="f9820-138">hello amount of memory consumed is proportional toohello window size and event rate.</span></span> 

<span data-ttu-id="f9820-139">toomitigate této situaci škálování hello dotazu zvýšením oddíly pomocí PARTITION BY.</span><span class="sxs-lookup"><span data-stu-id="f9820-139">toomitigate this situation, scale out hello query by increasing partitions by using PARTITION BY.</span></span> 

<span data-ttu-id="f9820-140">Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů zpracování.</span><span class="sxs-lookup"><span data-stu-id="f9820-140">After hello query is partitioned, it is spread out over multiple processing nodes.</span></span> <span data-ttu-id="f9820-141">V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="f9820-141">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span>

### <a name="large-number-of-out-of-order-events"></a><span data-ttu-id="f9820-142">Velké množství mimo pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="f9820-142">Large number of out of order events</span></span> 

<span data-ttu-id="f9820-143">Velký počet události mimo pořadí v rámci velké časové okno způsobí, že velikost hello hello "Změna pořadí vyrovnávací paměti" toobe větší.</span><span class="sxs-lookup"><span data-stu-id="f9820-143">A large number of out of order events within a large time window causes hello size of hello "reorder buffer" toobe larger.</span></span> <span data-ttu-id="f9820-144">toomitigate této situaci škálování hello dotazu zvýšením oddíly pomocí PARTITION BY.</span><span class="sxs-lookup"><span data-stu-id="f9820-144">toomitigate this situation, scale hello query by increasing partitions by using PARTITION BY.</span></span> <span data-ttu-id="f9820-145">Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="f9820-145">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="f9820-146">V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí.</span><span class="sxs-lookup"><span data-stu-id="f9820-146">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> 


## <a name="get-help"></a><span data-ttu-id="f9820-147">Podpora</span><span class="sxs-lookup"><span data-stu-id="f9820-147">Get help</span></span>
<span data-ttu-id="f9820-148">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="f9820-148">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9820-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9820-149">Next steps</span></span>
* [<span data-ttu-id="f9820-150">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f9820-150">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="f9820-151">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f9820-151">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="f9820-152">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f9820-152">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="f9820-153">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="f9820-153">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="f9820-154">Referenční dokumentace Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="f9820-154">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
