---
title: "propustnost tooincrease úlohy Stream Analytics aaaScale | Microsoft Docs"
description: "Zjistěte, jak úlohy Stream Analytics tooscale nakonfigurováním vstupní oddíly, ladění hello Definice dotazu a nastavení úlohy jednotek streamování."
keywords: "data streamování, streamování zpracování dat, optimalizovat analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a><span data-ttu-id="4a843-104">Škálování Azure Stream Analytics úlohy tooincrease datového proudu zpracování dat propustnosti</span><span class="sxs-lookup"><span data-stu-id="4a843-104">Scale Azure Stream Analytics jobs tooincrease stream data processing throughput</span></span>
<span data-ttu-id="4a843-105">Tento článek ukazuje, jak tootune Stream Analytics dotaz tooincrease propustnost pro úlohy streamování Analytics.</span><span class="sxs-lookup"><span data-stu-id="4a843-105">This article shows you how tootune a Stream Analytics query tooincrease throughput for Streaming Analytics jobs.</span></span> <span data-ttu-id="4a843-106">Zjistíte, jak tooscale Stream Analytics úlohy nakonfigurováním vstupní oddíly, definici dotazu vyladění hello analýzy a výpočet a nastavení úlohy *jednotek streaming* (SUs).</span><span class="sxs-lookup"><span data-stu-id="4a843-106">You learn how tooscale Stream Analytics jobs by configuring input partitions, tuning hello analytics query definition, and calculating and setting job *streaming units* (SUs).</span></span> 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a><span data-ttu-id="4a843-107">Jaké jsou části hello úlohy Stream Analytics?</span><span class="sxs-lookup"><span data-stu-id="4a843-107">What are hello parts of a Stream Analytics job?</span></span>
<span data-ttu-id="4a843-108">Definice úlohy Stream Analytics obsahuje vstupy, dotaz a výstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-108">A Stream Analytics job definition includes inputs, a query, and output.</span></span> <span data-ttu-id="4a843-109">Vstupní hodnoty jsou, kde úloha hello čte hello datový proud z.</span><span class="sxs-lookup"><span data-stu-id="4a843-109">Inputs are where hello job reads hello data stream from.</span></span> <span data-ttu-id="4a843-110">Hello dotazu je použité tootransform hello dat vstupní datový proud a výstup hello je, kde hello odesílá výsledky úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-110">hello query is used tootransform hello data input stream, and hello output is where hello job sends hello job results to.</span></span>  

<span data-ttu-id="4a843-111">Úloha vyžaduje alespoň jeden vstupní zdroj pro streamování data.</span><span class="sxs-lookup"><span data-stu-id="4a843-111">A job requires at least one input source for data streaming.</span></span> <span data-ttu-id="4a843-112">Hello vstupní zdroj dat datového proudu může být uložená v Centru událostí Azure nebo v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4a843-112">hello data stream input source can be stored in an Azure event hub or in Azure blob storage.</span></span> <span data-ttu-id="4a843-113">Další informace najdete v tématu [tooAzure Úvod Stream Analytics](stream-analytics-introduction.md) a [začít používat Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span><span class="sxs-lookup"><span data-stu-id="4a843-113">For more information, see [Introduction tooAzure Stream Analytics](stream-analytics-introduction.md) and [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span></span>

## <a name="partitions-in-event-hubs-and-azure-storage"></a><span data-ttu-id="4a843-114">Oddíly v centrům událostí a úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4a843-114">Partitions in event hubs and Azure storage</span></span>
<span data-ttu-id="4a843-115">Škálování úloha Stream Analytics využívá výhod oddíly v hello vstup nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-115">Scaling a Stream Analytics job takes advantage of partitions in hello input or output.</span></span> <span data-ttu-id="4a843-116">Vytváření oddílů umožňuje data rozdělíte do podmnožin podle klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="4a843-116">Partitioning lets you divide data into subsets based on a partition key.</span></span> <span data-ttu-id="4a843-117">Proces, který využívá hello data (například úlohu streamování Analytics) může spotřebovávat a zápis různých oddílů souběžně, což zvyšuje propustnost.</span><span class="sxs-lookup"><span data-stu-id="4a843-117">A process that consumes hello data (such as a Streaming Analytics job) can consume and write different partitions in parallel, which increases throughput.</span></span> <span data-ttu-id="4a843-118">Při práci s streamování analýzy, můžete využít výhod vytváření oddílů v centra událostí a v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4a843-118">When you work with Streaming Analytics, you can take advantage of partitioning in event hubs and in Blob storage.</span></span> 

<span data-ttu-id="4a843-119">Další informace o oddílech najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="4a843-119">For more information about partitions, see hello following articles:</span></span>

* [<span data-ttu-id="4a843-120">Přehled funkcí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="4a843-120">Event Hubs features overview</span></span>](../event-hubs/event-hubs-features.md#partitions)
* [<span data-ttu-id="4a843-121">Segmentace dat</span><span class="sxs-lookup"><span data-stu-id="4a843-121">Data partitioning</span></span>](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a><span data-ttu-id="4a843-122">Jednotky streamování (SUs)</span><span class="sxs-lookup"><span data-stu-id="4a843-122">Streaming units (SUs)</span></span>
<span data-ttu-id="4a843-123">Streamování jednotky (SUs) představují hello prostředků a výpočetních napájení, které jsou nutné v pořadí tooexecute úlohu služby Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="4a843-123">Streaming units (SUs) represent hello resources and computing power that are required in order tooexecute an Azure Stream Analytics job.</span></span> <span data-ttu-id="4a843-124">Služba SUs poskytují způsob toodescribe hello relativní zpracování událostí založené na kombinaci měření procesoru, paměti, kapacity a čtení a zápisu sazby.</span><span class="sxs-lookup"><span data-stu-id="4a843-124">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="4a843-125">Každý SU odpovídá tooroughly 1 MB za sekundu, propustnosti.</span><span class="sxs-lookup"><span data-stu-id="4a843-125">Each SU corresponds tooroughly 1 MB/second of throughput.</span></span> 

<span data-ttu-id="4a843-126">Výběr kolik služby SUs jsou požadovány pro konkrétní úlohy závisí na konfiguraci hello oddílů pro vstupy hello a na dotaz hello definovaný pro úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-126">Choosing how many SUs are required for a particular job depends on hello partition configuration for hello inputs and on hello query defined for hello job.</span></span> <span data-ttu-id="4a843-127">Můžete vybrat až tooyour kvótu v služby SUs pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="4a843-127">You can select up tooyour quota in SUs for a job.</span></span> <span data-ttu-id="4a843-128">Ve výchozím nastavení má každé předplatné Azure kvótu až služby SUs too50 pro všechny úlohy analýzy hello v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="4a843-128">By default, each Azure subscription has a quota of up too50 SUs for all hello analytics jobs in a specific region.</span></span> <span data-ttu-id="4a843-129">tooincrease služby SUs pro vaše předplatné nad rámec této kvóty, obraťte se na [Microsoft Support](http://support.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4a843-129">tooincrease SUs for your subscriptions beyond this quota, contact [Microsoft Support](http://support.microsoft.com).</span></span> <span data-ttu-id="4a843-130">Platné hodnoty pro služby SUs na úlohu jsou 1, 3, 6 a až v přírůstcích po 6.</span><span class="sxs-lookup"><span data-stu-id="4a843-130">Valid values for SUs per job are 1, 3, 6, and up in increments of 6.</span></span>

## <a name="embarrassingly-parallel-jobs"></a><span data-ttu-id="4a843-131">Trapně paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="4a843-131">Embarrassingly parallel jobs</span></span>
<span data-ttu-id="4a843-132">*Paralelně zpracovatelné* úlohy je nejvíce škálovatelným scénář hello máme v Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="4a843-132">An *embarrassingly parallel* job is hello most scalable scenario we have in Azure Stream Analytics.</span></span> <span data-ttu-id="4a843-133">Připojí se jeden oddíl hello vstupní tooone instance hello dotazu tooone oddílu hello výstupu.</span><span class="sxs-lookup"><span data-stu-id="4a843-133">It connects one partition of hello input tooone instance of hello query tooone partition of hello output.</span></span> <span data-ttu-id="4a843-134">Tato paralelismus má hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="4a843-134">This parallelism has hello following requirements:</span></span>

1. <span data-ttu-id="4a843-135">Pokud logika dotazu závisí na stejný klíč zpracovává hello podle hello stejný dotaz instance, ujistěte se, že události hello přejděte toohello stejného oddílu váš vstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-135">If your query logic depends on hello same key being processed by hello same query instance, you must make sure that hello events go toohello same partition of your input.</span></span> <span data-ttu-id="4a843-136">Pro službu event hubs to znamená, že data události hello musí mít hello **PartitionKey** hodnotu sady.</span><span class="sxs-lookup"><span data-stu-id="4a843-136">For event hubs, this means that hello event data must have hello **PartitionKey** value set.</span></span> <span data-ttu-id="4a843-137">Alternativně můžete použít odesílatelé oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-137">Alternatively, you can use partitioned senders.</span></span> <span data-ttu-id="4a843-138">Úložiště objektů blob, to znamená, že hello události se posílají toohello stejné složce oddílu.</span><span class="sxs-lookup"><span data-stu-id="4a843-138">For blob storage, this means that hello events are sent toohello same partition folder.</span></span> <span data-ttu-id="4a843-139">Pokud logika dotazu nevyžaduje hello stejný klíč toobe zpracovat podle hello stejný dotaz instance, můžete ignorovat tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="4a843-139">If your query logic does not require hello same key toobe processed by hello same query instance, you can ignore this requirement.</span></span> <span data-ttu-id="4a843-140">Příkladem této logiky, může být jednoduchý dotaz vyberte filtr projektu.</span><span class="sxs-lookup"><span data-stu-id="4a843-140">An example of this logic would be a simple select-project-filter query.</span></span>  

2. <span data-ttu-id="4a843-141">Jakmile hello data na straně vstupní hello rozložená, musí se ujistěte, že váš dotaz je rozdělený do oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-141">Once hello data is laid out on hello input side, you must make sure that your query is partitioned.</span></span> <span data-ttu-id="4a843-142">To vyžaduje, abyste toouse **Partition By** v všechny kroky hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-142">This requires you toouse **Partition By** in all hello steps.</span></span> <span data-ttu-id="4a843-143">Jsou povoleny několik kroků, ale musí mít všechny oddíly podle hello stejný klíč.</span><span class="sxs-lookup"><span data-stu-id="4a843-143">Multiple steps are allowed, but they all must be partitioned by hello same key.</span></span> <span data-ttu-id="4a843-144">V současné době hello klíč rozdělení do oddílů musí být nastaven příliš**PartitionId** v pořadí pro plně paralelní toobe úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-144">Currently, hello partitioning key must be set too**PartitionId** in order for hello job toobe fully parallel.</span></span>  

3. <span data-ttu-id="4a843-145">Pouze služba event hubs a úložiště objektů blob v současné době podporují dělené výstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-145">Currently only event hubs and blob storage support partitioned output.</span></span> <span data-ttu-id="4a843-146">Pro výstupu centra událostí, musíte nakonfigurovat toobe klíče oddílu hello **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="4a843-146">For event hub output, you must configure hello partition key toobe **PartitionId**.</span></span> <span data-ttu-id="4a843-147">Pro výstup úložiště objektů blob nemáte toodo nic.</span><span class="sxs-lookup"><span data-stu-id="4a843-147">For blob storage output, you don't have toodo anything.</span></span>  

4. <span data-ttu-id="4a843-148">Hello počet vstupních oddílů musí být roven hello počet oddílů výstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-148">hello number of input partitions must equal hello number of output partitions.</span></span> <span data-ttu-id="4a843-149">Výstup úložiště objektů BLOB aktuálně nepodporuje oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-149">Blob storage output doesn't currently support partitions.</span></span> <span data-ttu-id="4a843-150">Ale to nevadí, protože dědí hello dělení schéma hello nadřazeného dotazu.</span><span class="sxs-lookup"><span data-stu-id="4a843-150">But that's okay, because it inherits hello partitioning scheme of hello upstream query.</span></span> <span data-ttu-id="4a843-151">Zde jsou příklady oddílu hodnot, které umožňují plně paralelní úlohy:</span><span class="sxs-lookup"><span data-stu-id="4a843-151">Here are examples of partition values that allow a fully parallel job:</span></span>  

   * <span data-ttu-id="4a843-152">8 oddíly vstupní centra událostí a Centrum událostí 8 výstupní oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-152">8 event hub input partitions and 8 event hub output partitions</span></span>
   * <span data-ttu-id="4a843-153">8 oddíly vstupní centra událostí a výstup úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="4a843-153">8 event hub input partitions and blob storage output</span></span>  
   * <span data-ttu-id="4a843-154">8 oddíly vstupní úložiště objektů blob a výstup úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="4a843-154">8 blob storage input partitions and blob storage output</span></span>  
   * <span data-ttu-id="4a843-155">8 blob oddílů pro úložiště a 8 oddíly výstupu centra událostí</span><span class="sxs-lookup"><span data-stu-id="4a843-155">8 blob storage input partitions and 8 event hub output partitions</span></span>  

<span data-ttu-id="4a843-156">Hello následující části popisují některé ukázkové scénáře, které jsou jednoduše paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="4a843-156">hello following sections discuss some example scenarios that are embarrassingly parallel.</span></span>

### <a name="simple-query"></a><span data-ttu-id="4a843-157">Jednoduchý dotaz</span><span class="sxs-lookup"><span data-stu-id="4a843-157">Simple query</span></span>

* <span data-ttu-id="4a843-158">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-158">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-159">Výstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-159">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="4a843-160">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="4a843-160">Query:</span></span>

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

<span data-ttu-id="4a843-161">Dotaz je jednoduchý filtr.</span><span class="sxs-lookup"><span data-stu-id="4a843-161">This query is a simple filter.</span></span> <span data-ttu-id="4a843-162">Proto jsme nepotřebují tooworry o vytváření oddílů hello vstupu, který je odesílán toohello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="4a843-162">Therefore, we don't need tooworry about partitioning hello input that is being sent toohello event hub.</span></span> <span data-ttu-id="4a843-163">Všimněte si, že dotaz hello obsahuje **oddílu podle PartitionId**, takže se splní požadavek #2 z dříve.</span><span class="sxs-lookup"><span data-stu-id="4a843-163">Notice that hello query includes **Partition By PartitionId**, so it fulfills requirement #2 from earlier.</span></span> <span data-ttu-id="4a843-164">Pro výstup hello potřebujeme výstupu centra událostí tooconfigure hello sady hello úlohy toohave hello vytvoření oddílu klíče příliš**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="4a843-164">For hello output, we need tooconfigure hello event hub output in hello job toohave hello parition key set too**PartitionId**.</span></span> <span data-ttu-id="4a843-165">Jeden poslední kontrola je toomake se, že hello počet vstupních oddílů je rovna toohello počet oddílů výstup.</span><span class="sxs-lookup"><span data-stu-id="4a843-165">One last check is toomake sure that hello number of input partitions is equal toohello number of output partitions.</span></span>

### <a name="query-with-a-grouping-key"></a><span data-ttu-id="4a843-166">Dotaz s klíčem seskupení</span><span class="sxs-lookup"><span data-stu-id="4a843-166">Query with a grouping key</span></span>

* <span data-ttu-id="4a843-167">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-167">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-168">Výstup: Úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="4a843-168">Output: Blob storage</span></span>

<span data-ttu-id="4a843-169">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="4a843-169">Query:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="4a843-170">Tento dotaz obsahuje seskupení klíč.</span><span class="sxs-lookup"><span data-stu-id="4a843-170">This query has a grouping key.</span></span> <span data-ttu-id="4a843-171">Proto hello stejné toobe klíčových požadavků zpracovává hello stejný dotaz instance, což znamená, že události, musí se poslat centra událostí toohello oddílů způsobem.</span><span class="sxs-lookup"><span data-stu-id="4a843-171">Therefore, hello same key needs toobe processed by hello same query instance, which means that events must be sent toohello event hub in a partitioned manner.</span></span> <span data-ttu-id="4a843-172">Ale které klíč by měl použít?</span><span class="sxs-lookup"><span data-stu-id="4a843-172">But which key should be used?</span></span> <span data-ttu-id="4a843-173">**PartitionId** je koncept úlohy logiku.</span><span class="sxs-lookup"><span data-stu-id="4a843-173">**PartitionId** is a job-logic concept.</span></span> <span data-ttu-id="4a843-174">klíč Hello skutečně záleží nám je **TollBoothId**, takže hello **PartitionKey** hodnota dat událostí hello by měla být **TollBoothId**.</span><span class="sxs-lookup"><span data-stu-id="4a843-174">hello key we actually care about is **TollBoothId**, so hello **PartitionKey** value of hello event data should be **TollBoothId**.</span></span> <span data-ttu-id="4a843-175">Můžeme provést v dotazu hello nastavením **Partition By** příliš**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="4a843-175">We do this in hello query by setting **Partition By** too**PartitionId**.</span></span> <span data-ttu-id="4a843-176">Vzhledem k tomu, že výstup hello je úložiště objektů blob, společnost Microsoft nepotřebuje tooworry o konfiguraci hodnotu klíče oddílu, podle požadavků #4.</span><span class="sxs-lookup"><span data-stu-id="4a843-176">Since hello output is blob storage, we don't need tooworry about configuring a partition key value, as per requirement #4.</span></span>

### <a name="multi-step-query-with-a-grouping-key"></a><span data-ttu-id="4a843-177">Vícekrokový dotazu s klíčem seskupení</span><span class="sxs-lookup"><span data-stu-id="4a843-177">Multi-step query with a grouping key</span></span>
* <span data-ttu-id="4a843-178">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-178">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-179">Výstup: Instance rozbočovače událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-179">Output: Event hub instance with 8 partitions</span></span>

<span data-ttu-id="4a843-180">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="4a843-180">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="4a843-181">Tento dotaz obsahuje seskupení klíč, takže hello stejné toobe klíčových požadavků zpracovává hello stejnou instanci dotazu.</span><span class="sxs-lookup"><span data-stu-id="4a843-181">This query has a grouping key, so hello same key needs toobe processed by hello same query instance.</span></span> <span data-ttu-id="4a843-182">Můžeme použít hello stejné strategie jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-182">We can use hello same strategy as in hello previous example.</span></span> <span data-ttu-id="4a843-183">V takovém případě hello dotaz má několik kroků.</span><span class="sxs-lookup"><span data-stu-id="4a843-183">In this case, hello query has multiple steps.</span></span> <span data-ttu-id="4a843-184">Má každý krok **oddílu podle PartitionId**?</span><span class="sxs-lookup"><span data-stu-id="4a843-184">Does each step have **Partition By PartitionId**?</span></span> <span data-ttu-id="4a843-185">Ano, takže hello dotazu splní požadavek #3.</span><span class="sxs-lookup"><span data-stu-id="4a843-185">Yes, so hello query fulfills requirement #3.</span></span> <span data-ttu-id="4a843-186">Pro výstup hello potřebujeme klíč oddílu hello tooset příliš**PartitionId**, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="4a843-186">For hello output, we need tooset hello partition key too**PartitionId**, as discussed earlier.</span></span> <span data-ttu-id="4a843-187">Také uvidíme, že má hello stejný počet oddílů jako vstup hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-187">We can also see that it has hello same number of partitions as hello input.</span></span>

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a><span data-ttu-id="4a843-188">Ukázkové scénáře, které jsou *není* paralelně zpracovatelné</span><span class="sxs-lookup"><span data-stu-id="4a843-188">Example scenarios that are *not* embarrassingly parallel</span></span>

<span data-ttu-id="4a843-189">V předchozí části hello jsme vám ukázal některé paralelně zpracovatelné scénáře.</span><span class="sxs-lookup"><span data-stu-id="4a843-189">In hello previous section, we showed some embarrassingly parallel scenarios.</span></span> <span data-ttu-id="4a843-190">V této části probereme scénáře, které nesplňují všechny požadavky hello toobe paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="4a843-190">In this section, we discuss scenarios that don't meet all hello requirements toobe embarrassingly parallel.</span></span> 

### <a name="mismatched-partition-count"></a><span data-ttu-id="4a843-191">Počet neodpovídající oddílů</span><span class="sxs-lookup"><span data-stu-id="4a843-191">Mismatched partition count</span></span>
* <span data-ttu-id="4a843-192">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-192">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-193">Výstup: Centra událostí s 32 oddílů</span><span class="sxs-lookup"><span data-stu-id="4a843-193">Output: Event hub with 32 partitions</span></span>

<span data-ttu-id="4a843-194">V takovém případě nezávisle na tom, jaký dotaz hello je.</span><span class="sxs-lookup"><span data-stu-id="4a843-194">In this case, it doesn't matter what hello query is.</span></span> <span data-ttu-id="4a843-195">Pokud počet vstupních oddílů hello se neshoduje se počet oddílů výstup hello, není paralelně zpracovatelné hello topologie.</span><span class="sxs-lookup"><span data-stu-id="4a843-195">If hello input partition count doesn't match hello output partition count, hello topology isn't embarrassingly parallel.</span></span>

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a><span data-ttu-id="4a843-196">Jako výstup není pomocí služby event hubs nebo úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="4a843-196">Not using event hubs or blob storage as output</span></span>
* <span data-ttu-id="4a843-197">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-197">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-198">Výstup: PowerBI</span><span class="sxs-lookup"><span data-stu-id="4a843-198">Output: PowerBI</span></span>

<span data-ttu-id="4a843-199">Výstup PowerBI aktuálně nepodporuje vytváření oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-199">PowerBI output doesn't currently support partitioning.</span></span> <span data-ttu-id="4a843-200">Proto tento scénář není paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="4a843-200">Therefore, this scenario is not embarrassingly parallel.</span></span>

### <a name="multi-step-query-with-different-partition-by-values"></a><span data-ttu-id="4a843-201">Vícekrokový dotazu s různými hodnotami Partition By</span><span class="sxs-lookup"><span data-stu-id="4a843-201">Multi-step query with different Partition By values</span></span>
* <span data-ttu-id="4a843-202">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-202">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="4a843-203">Výstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-203">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="4a843-204">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="4a843-204">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="4a843-205">Jak vidíte, druhý krok hello používá **TollBoothId** jako hello klíč rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-205">As you can see, hello second step uses **TollBoothId** as hello partitioning key.</span></span> <span data-ttu-id="4a843-206">Tento krok není hello stejné jako první krok text hello, a proto vyžaduje nám toodo náhodně.</span><span class="sxs-lookup"><span data-stu-id="4a843-206">This step is not hello same as hello first step, and it therefore requires us toodo a shuffle.</span></span> 

<span data-ttu-id="4a843-207">Hello předchozí příklady ukazují, některé úlohy Stream Analytics, které vyhovují příliš (nebo nemusíte) paralelně zpracovatelné topologie.</span><span class="sxs-lookup"><span data-stu-id="4a843-207">hello preceding examples show some Stream Analytics jobs that conform too(or don't) an embarrassingly parallel topology.</span></span> <span data-ttu-id="4a843-208">Pokud jsou v souladu, mají hello potenciální pro maximální měřítko.</span><span class="sxs-lookup"><span data-stu-id="4a843-208">If they do conform, they have hello potential for maximum scale.</span></span> <span data-ttu-id="4a843-209">Pro úlohy, které se nehodí jeden z těchto profilů škálování pokyny bude k dispozici v budoucích aktualizací.</span><span class="sxs-lookup"><span data-stu-id="4a843-209">For jobs that don't fit one of these profiles, scaling guidance will be available in future updates.</span></span> <span data-ttu-id="4a843-210">Prozatím použijte hello obecné pokyny v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-210">For now, use hello general guidance in hello following sections.</span></span>

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a><span data-ttu-id="4a843-211">Vypočítat hello maximální jednotek streamování úlohy</span><span class="sxs-lookup"><span data-stu-id="4a843-211">Calculate hello maximum streaming units of a job</span></span>
<span data-ttu-id="4a843-212">Celkový počet jednotek streamování, které lze použít v rámci úlohy Stream Analytics Hello závisí na hello počet kroků v dotazu hello definovány pro hello úlohy a hello počet oddílů pro každý krok.</span><span class="sxs-lookup"><span data-stu-id="4a843-212">hello total number of streaming units that can be used by a Stream Analytics job depends on hello number of steps in hello query defined for hello job and hello number of partitions for each step.</span></span>

### <a name="steps-in-a-query"></a><span data-ttu-id="4a843-213">Kroky v dotazu</span><span class="sxs-lookup"><span data-stu-id="4a843-213">Steps in a query</span></span>
<span data-ttu-id="4a843-214">Dotaz může mít jeden nebo mnoho kroků.</span><span class="sxs-lookup"><span data-stu-id="4a843-214">A query can have one or many steps.</span></span> <span data-ttu-id="4a843-215">Každý krok je poddotaz definované hello **WITH** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="4a843-215">Each step is a subquery defined by hello **WITH** keyword.</span></span> <span data-ttu-id="4a843-216">Hello dotazu, který je mimo hello **WITH** – klíčové slovo (pouze jeden dotaz) se také počítá jako krok, jako je například hello **vyberte** příkaz v hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="4a843-216">hello query that is outside hello **WITH** keyword (one query only) is also counted as a step, such as hello **SELECT** statement in hello following query:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

<span data-ttu-id="4a843-217">Tento dotaz má dva kroky.</span><span class="sxs-lookup"><span data-stu-id="4a843-217">This query has two steps.</span></span>

> [!NOTE]
> <span data-ttu-id="4a843-218">Tento dotaz je podrobněji popsána dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-218">This query is discussed in more detail later in hello article.</span></span>
>  

### <a name="partition-a-step"></a><span data-ttu-id="4a843-219">Oddílu krok</span><span class="sxs-lookup"><span data-stu-id="4a843-219">Partition a step</span></span>
<span data-ttu-id="4a843-220">Vytváření oddílů krok vyžaduje hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="4a843-220">Partitioning a step requires hello following conditions:</span></span>

* <span data-ttu-id="4a843-221">vstupní zdroj Hello musí mít oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-221">hello input source must be partitioned.</span></span> 
* <span data-ttu-id="4a843-222">Hello **vyberte** výraz dotazu hello musí ke čtení z oddílů vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="4a843-222">hello **SELECT** statement of hello query must read from a partitioned input source.</span></span>
* <span data-ttu-id="4a843-223">dotaz Hello v rámci kroku hello musí mít hello **Partition By** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="4a843-223">hello query within hello step must have hello **Partition By** keyword.</span></span>

<span data-ttu-id="4a843-224">Při dotazu je rozdělena na oddíly, hello vstupních událostech se zpracování a agregované v samostatném oddílu skupiny a výstupy události jsou generovány pro každou ze skupin hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-224">When a query is partitioned, hello input events are processed and aggregated in separate partition groups, and outputs events are generated for each of hello groups.</span></span> <span data-ttu-id="4a843-225">Pokud chcete, aby součet agregace, je nutné vytvořit druhý tooaggregate krok bez oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-225">If you want a combined aggregate, you must create a second non-partitioned step tooaggregate.</span></span>

### <a name="calculate-hello-max-streaming-units-for-a-job"></a><span data-ttu-id="4a843-226">Vypočítat hello maximální počet jednotek pro úlohu streamování</span><span class="sxs-lookup"><span data-stu-id="4a843-226">Calculate hello max streaming units for a job</span></span>
<span data-ttu-id="4a843-227">Všechny kroky bez oddílů společně můžete postupně škálovat toosix jednotky (SUs) u úlohy Stream Analytics streamování.</span><span class="sxs-lookup"><span data-stu-id="4a843-227">All non-partitioned steps together can scale up toosix streaming units (SUs) for a Stream Analytics job.</span></span> <span data-ttu-id="4a843-228">Služba SUs tooadd, krok musí mít oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-228">tooadd SUs, a step must be partitioned.</span></span> <span data-ttu-id="4a843-229">Každý oddíl můžete mít šest služby SUs.</span><span class="sxs-lookup"><span data-stu-id="4a843-229">Each partition can have six SUs.</span></span>

<table border="1">
<tr><th><span data-ttu-id="4a843-230">Dotaz</span><span class="sxs-lookup"><span data-stu-id="4a843-230">Query</span></span></th><th><span data-ttu-id="4a843-231">Služba SUs Max pro úlohu hello</span><span class="sxs-lookup"><span data-stu-id="4a843-231">Max SUs for hello job</span></span></th></td>

<tr><td>
<ul>
<li><span data-ttu-id="4a843-232">Hello dotaz obsahuje jeden krok.</span><span class="sxs-lookup"><span data-stu-id="4a843-232">hello query contains one step.</span></span></li>
<li><span data-ttu-id="4a843-233">Krok Hello není rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-233">hello step is not partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="4a843-234">6</span><span class="sxs-lookup"><span data-stu-id="4a843-234">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="4a843-235">Hello vstupní datový proud je rozdělena na oddíly 3.</span><span class="sxs-lookup"><span data-stu-id="4a843-235">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="4a843-236">Hello dotaz obsahuje jeden krok.</span><span class="sxs-lookup"><span data-stu-id="4a843-236">hello query contains one step.</span></span></li>
<li><span data-ttu-id="4a843-237">Krok Hello je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-237">hello step is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="4a843-238">18</span><span class="sxs-lookup"><span data-stu-id="4a843-238">18</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="4a843-239">Hello dotaz obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="4a843-239">hello query contains two steps.</span></span></li>
<li><span data-ttu-id="4a843-240">Ani jeden z kroků hello je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-240">Neither of hello steps is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="4a843-241">6</span><span class="sxs-lookup"><span data-stu-id="4a843-241">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="4a843-242">Hello vstupní datový proud je rozdělena na oddíly 3.</span><span class="sxs-lookup"><span data-stu-id="4a843-242">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="4a843-243">Hello dotaz obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="4a843-243">hello query contains two steps.</span></span> <span data-ttu-id="4a843-244">vstupní krok Hello je rozdělena na oddíly a druhý krok hello není.</span><span class="sxs-lookup"><span data-stu-id="4a843-244">hello input step is partitioned and hello second step is not.</span></span></li>
<li><span data-ttu-id="4a843-245">Hello <strong>vyberte</strong> příkaz čte ze vstupu hello rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-245">hello <strong>SELECT</strong> statement reads from hello partitioned input.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="4a843-246">24 (oddílů kroky 18) + 6 pokyny bez oddílů</span><span class="sxs-lookup"><span data-stu-id="4a843-246">24 (18 for partitioned steps + 6 for non-partitioned steps)</span></span></td></tr>
</table>

### <a name="examples-of-scaling"></a><span data-ttu-id="4a843-247">Příklady škálování</span><span class="sxs-lookup"><span data-stu-id="4a843-247">Examples of scaling</span></span>

<span data-ttu-id="4a843-248">Hello následující dotaz vypočítá hello počet aut, v rámci průchodu přes projedou stanice, která má tři tollbooths časového období tři minuty.</span><span class="sxs-lookup"><span data-stu-id="4a843-248">hello following query calculates hello number of cars within a three-minute window going through a toll station that has three tollbooths.</span></span> <span data-ttu-id="4a843-249">Tento dotaz je možné škálovat toosix služby SUs.</span><span class="sxs-lookup"><span data-stu-id="4a843-249">This query can be scaled up toosix SUs.</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="4a843-250">toouse další služby SUs pro hello dotazu, obě hello vstupního datového proudu a musí mít oddíly hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="4a843-250">toouse more SUs for hello query, both hello input data stream and hello query must be partitioned.</span></span> <span data-ttu-id="4a843-251">Vzhledem k tomu, že hello datový proud oddíl nastavena too3, hello následující změny dotazu je možné škálovat služby SUs too18:</span><span class="sxs-lookup"><span data-stu-id="4a843-251">Since hello data stream partition is set too3, hello following modified query can be scaled up too18 SUs:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="4a843-252">Při dotazu je rozdělena na oddíly, hello vstupních událostech se zpracovat a agregovat v samostatném oddílu skupiny.</span><span class="sxs-lookup"><span data-stu-id="4a843-252">When a query is partitioned, hello input events are processed and aggregated in separate partition groups.</span></span> <span data-ttu-id="4a843-253">Výstup události jsou také vytvářeny pro každou ze skupin hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-253">Output events are also generated for each of hello groups.</span></span> <span data-ttu-id="4a843-254">Dělení může způsobit, že některé neočekávané výsledky při hello **GROUP BY** pole není klíč oddílu hello v hello vstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="4a843-254">Partitioning can cause some unexpected results when hello **GROUP BY** field is not hello partition key in hello input data stream.</span></span> <span data-ttu-id="4a843-255">Například hello **TollBoothId** pole v hello předchozího dotazu není klíč oddílu hello **Input1**.</span><span class="sxs-lookup"><span data-stu-id="4a843-255">For example, hello **TollBoothId** field in hello previous query is not hello partition key of **Input1**.</span></span> <span data-ttu-id="4a843-256">Hello výsledkem je, že hello data z mýtná celnice č. 1 možné rozdělit do několika oddílů.</span><span class="sxs-lookup"><span data-stu-id="4a843-256">hello result is that hello data from TollBooth #1 can be spread in multiple partitions.</span></span>

<span data-ttu-id="4a843-257">Každý z hello **Input1** oddíly se zpracovávají odděleně podle Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="4a843-257">Each of hello **Input1** partitions will be processed separately by Stream Analytics.</span></span> <span data-ttu-id="4a843-258">V důsledku toho více záznamů počtu hello car pro hello stejné mýtná celnice v hello stejné Přeskakující okno bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4a843-258">As a result, multiple records of hello car count for hello same tollbooth in hello same Tumbling window will be created.</span></span> <span data-ttu-id="4a843-259">Pokud klíč hello vstupní oddílu nelze změnit, můžete tento problém opravit tak, že přidáte krok bez oddílu, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4a843-259">If hello input partition key can't be changed, this problem can be fixed by adding a non-partition step, as in hello following example:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="4a843-260">Tento dotaz lze škálovat too24 SUs.</span><span class="sxs-lookup"><span data-stu-id="4a843-260">This query can be scaled too24 SUs.</span></span>

> [!NOTE]
> <span data-ttu-id="4a843-261">Pokud jsou připojení dvě datové proudy, ujistěte se, že datové proudy hello jsou rozdělena na oddíly pomocí klíče oddílu hello hello sloupce použít toocreate hello spojení.</span><span class="sxs-lookup"><span data-stu-id="4a843-261">If you are joining two streams, make sure that hello streams are partitioned by hello partition key of hello column that you use toocreate hello joins.</span></span> <span data-ttu-id="4a843-262">Také se ujistěte, že máte hello stejný počet oddílů v obou datových proudů.</span><span class="sxs-lookup"><span data-stu-id="4a843-262">Also make sure that you have hello same number of partitions in both streams.</span></span>
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a><span data-ttu-id="4a843-263">Konfigurace služby Stream Analytics jednotky streamování</span><span class="sxs-lookup"><span data-stu-id="4a843-263">Configure Stream Analytics streaming units</span></span>

1. <span data-ttu-id="4a843-264">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a843-264">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4a843-265">V seznamu hello prostředků najdete úlohu Stream Analytics hello má tooscale a potom ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="4a843-265">In hello list of resources, find hello Stream Analytics job that you want tooscale and then open it.</span></span>
3. <span data-ttu-id="4a843-266">V hello úlohy okno, v části **konfigurace**, klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="4a843-266">In hello job blade, under **Configure**, click **Scale**.</span></span>

    ![Azure konfigurace portálu úlohy Stream Analytics][img.stream.analytics.preview.portal.settings.scale]

4. <span data-ttu-id="4a843-268">Použijte hello posuvníku tooset hello služby SUs pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-268">Use hello slider tooset hello SUs for hello job.</span></span> <span data-ttu-id="4a843-269">Všimněte si, že jste omezené toospecific SU nastavení.</span><span class="sxs-lookup"><span data-stu-id="4a843-269">Notice that you are limited toospecific SU settings.</span></span>


## <a name="monitor-job-performance"></a><span data-ttu-id="4a843-270">Monitorování výkonu úlohy</span><span class="sxs-lookup"><span data-stu-id="4a843-270">Monitor job performance</span></span>
<span data-ttu-id="4a843-271">Pomocí hello portálu Azure, můžete sledovat hello propustnost úlohy:</span><span class="sxs-lookup"><span data-stu-id="4a843-271">Using hello Azure portal, you can track hello throughput of a job:</span></span>

![Azure Stream Analytics monitorování úloh][img.stream.analytics.monitor.job]

<span data-ttu-id="4a843-273">Vypočítejte propustnost hello očekávání pro zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="4a843-273">Calculate hello expected throughput of hello workload.</span></span> <span data-ttu-id="4a843-274">Pokud hello propustnost je menší než se očekávalo, ladit hello vstupní oddílu, ladit hello dotazu a přidat úloha tooyour služby SUs.</span><span class="sxs-lookup"><span data-stu-id="4a843-274">If hello throughput is less than expected, tune hello input partition, tune hello query, and add SUs tooyour job.</span></span>


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a><span data-ttu-id="4a843-275">Vizualizace Stream Analytics propustnost škálované: hello malin pí scénář</span><span class="sxs-lookup"><span data-stu-id="4a843-275">Visualize Stream Analytics throughput at scale: hello Raspberry Pi scenario</span></span>
<span data-ttu-id="4a843-276">toohelp víte, jak úlohy Stream Analytics škálování, jsme provedli experimentu založené na vstupu z malin platformy zařízení.</span><span class="sxs-lookup"><span data-stu-id="4a843-276">toohelp you understand how Stream Analytics jobs scale, we performed an experiment based on input from a Raspberry Pi device.</span></span> <span data-ttu-id="4a843-277">Tento experiment dejte nám najdete v části hello vliv na propustnost několika streamování jednotky a oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-277">This experiment let us see hello effect on throughput of multiple streaming units and partitions.</span></span>

<span data-ttu-id="4a843-278">V tomto scénáři hello zařízení odesílá centra událostí tooan senzor dat (klientů).</span><span class="sxs-lookup"><span data-stu-id="4a843-278">In this scenario, hello device sends sensor data (clients) tooan event hub.</span></span> <span data-ttu-id="4a843-279">Streamování Analytics zpracovává hello data a odešle výstrahu nebo statistiky jako výstup tooanother centra událostí.</span><span class="sxs-lookup"><span data-stu-id="4a843-279">Streaming Analytics processes hello data and sends an alert or statistics as an output tooanother event hub.</span></span> 

<span data-ttu-id="4a843-280">Hello klient odešle data snímačů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4a843-280">hello client sends sensor data in JSON format.</span></span> <span data-ttu-id="4a843-281">výstup Hello dat je také ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4a843-281">hello data output is also in JSON format.</span></span> <span data-ttu-id="4a843-282">Hello data vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4a843-282">hello data looks like this:</span></span>

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

<span data-ttu-id="4a843-283">Hello následující dotaz je použité toosend výstrahu, když je vypnuté světlý:</span><span class="sxs-lookup"><span data-stu-id="4a843-283">hello following query is used toosend an alert when a light is switched off:</span></span>

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a><span data-ttu-id="4a843-284">Míra propustnost</span><span class="sxs-lookup"><span data-stu-id="4a843-284">Measure throughput</span></span>

<span data-ttu-id="4a843-285">V tomto kontextu je propustnost hello množství vstupních dat zpracovávaných Stream Analytics pevné množství času.</span><span class="sxs-lookup"><span data-stu-id="4a843-285">In this context, throughput is hello amount of input data processed by Stream Analytics in a fixed amount of time.</span></span> <span data-ttu-id="4a843-286">(Jsme měří 10 minut.) tooachieve hello nejlepší zpracování propustnost pro hello vstupní data, data i hello stream vstup a hello dotazu byly rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="4a843-286">(We measured for 10 minutes.) tooachieve hello best processing throughput for hello input data, both hello data stream input and hello query were  partitioned.</span></span> <span data-ttu-id="4a843-287">Jsme zahrnuté **COUNT()** v toomeasure hello dotazu byly zpracovány kolik vstupních událostech.</span><span class="sxs-lookup"><span data-stu-id="4a843-287">We included **COUNT()** in hello query toomeasure how many input events were processed.</span></span> <span data-ttu-id="4a843-288">zda úlohy hello toomake nebyl jednoduše čekání toocome vstupních událostech, každý oddíl centra událostí vstupní hello se předem načtou přibližně 300 MB vstupní data.</span><span class="sxs-lookup"><span data-stu-id="4a843-288">toomake sure hello job was not simply waiting for input events toocome, each partition of hello input event hub was preloaded with about 300 MB of input data.</span></span>

<span data-ttu-id="4a843-289">Hello následující tabulka uvádí hello výsledků, které jsme viděli při odpovídající oddíl hello počty ve službě event hubs a jsme vyšší hello počet jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="4a843-289">hello following table shows hello results we saw when we increased hello number of streaming units and hello corresponding partition counts in event hubs.</span></span>  

<table border="1">
<tr><th><span data-ttu-id="4a843-290">Vstupní oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-290">Input Partitions</span></span></th><th><span data-ttu-id="4a843-291">Výstup oddíly</span><span class="sxs-lookup"><span data-stu-id="4a843-291">Output Partitions</span></span></th><th><span data-ttu-id="4a843-292">Jednotky streamování</span><span class="sxs-lookup"><span data-stu-id="4a843-292">Streaming Units</span></span></th><th><span data-ttu-id="4a843-293">Propustnost</span><span class="sxs-lookup"><span data-stu-id="4a843-293">Sustained Throughput</span></span>
</th></td>

<tr><td><span data-ttu-id="4a843-294">12</span><span class="sxs-lookup"><span data-stu-id="4a843-294">12</span></span></td>
<td><span data-ttu-id="4a843-295">12</span><span class="sxs-lookup"><span data-stu-id="4a843-295">12</span></span></td>
<td><span data-ttu-id="4a843-296">6</span><span class="sxs-lookup"><span data-stu-id="4a843-296">6</span></span></td>
<td><span data-ttu-id="4a843-297">4.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-297">4.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="4a843-298">12</span><span class="sxs-lookup"><span data-stu-id="4a843-298">12</span></span></td>
<td><span data-ttu-id="4a843-299">12</span><span class="sxs-lookup"><span data-stu-id="4a843-299">12</span></span></td>
<td><span data-ttu-id="4a843-300">12</span><span class="sxs-lookup"><span data-stu-id="4a843-300">12</span></span></td>
<td><span data-ttu-id="4a843-301">8.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-301">8.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="4a843-302">48</span><span class="sxs-lookup"><span data-stu-id="4a843-302">48</span></span></td>
<td><span data-ttu-id="4a843-303">48</span><span class="sxs-lookup"><span data-stu-id="4a843-303">48</span></span></td>
<td><span data-ttu-id="4a843-304">48</span><span class="sxs-lookup"><span data-stu-id="4a843-304">48</span></span></td>
<td><span data-ttu-id="4a843-305">38.32 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-305">38.32 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="4a843-306">192</span><span class="sxs-lookup"><span data-stu-id="4a843-306">192</span></span></td>
<td><span data-ttu-id="4a843-307">192</span><span class="sxs-lookup"><span data-stu-id="4a843-307">192</span></span></td>
<td><span data-ttu-id="4a843-308">192</span><span class="sxs-lookup"><span data-stu-id="4a843-308">192</span></span></td>
<td><span data-ttu-id="4a843-309">172.67 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-309">172.67 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="4a843-310">480</span><span class="sxs-lookup"><span data-stu-id="4a843-310">480</span></span></td>
<td><span data-ttu-id="4a843-311">480</span><span class="sxs-lookup"><span data-stu-id="4a843-311">480</span></span></td>
<td><span data-ttu-id="4a843-312">480</span><span class="sxs-lookup"><span data-stu-id="4a843-312">480</span></span></td>
<td><span data-ttu-id="4a843-313">454.27 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-313">454.27 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="4a843-314">720</span><span class="sxs-lookup"><span data-stu-id="4a843-314">720</span></span></td>
<td><span data-ttu-id="4a843-315">720</span><span class="sxs-lookup"><span data-stu-id="4a843-315">720</span></span></td>
<td><span data-ttu-id="4a843-316">720</span><span class="sxs-lookup"><span data-stu-id="4a843-316">720</span></span></td>
<td><span data-ttu-id="4a843-317">609.69 MB/s</span><span class="sxs-lookup"><span data-stu-id="4a843-317">609.69 MB/s</span></span></td>
</tr>
</table>

<span data-ttu-id="4a843-318">A hello následující graf ukazuje vizualizaci hello vztah mezi službou SUs a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="4a843-318">And hello following graph shows a visualization of hello relationship between SUs and throughput.</span></span>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a><span data-ttu-id="4a843-320">Podpora</span><span class="sxs-lookup"><span data-stu-id="4a843-320">Get help</span></span>
<span data-ttu-id="4a843-321">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="4a843-321">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a843-322">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a843-322">Next steps</span></span>
* [<span data-ttu-id="4a843-323">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4a843-323">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="4a843-324">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4a843-324">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="4a843-325">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="4a843-325">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="4a843-326">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4a843-326">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

