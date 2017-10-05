---
title: "Škálování úlohy Stream Analytics, pokud chcete zvýšit propustnost | Microsoft Docs"
description: "Postup konfigurace vstupní oddíly, ladění definice dotazu a nastavení úlohu streamování jednotky škálování úlohy Stream Analytics."
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
ms.openlocfilehash: ab894976c72ea3785d7f58e51b3dd64511e1e8e3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a><span data-ttu-id="72b59-104">Škálování služby Stream Analytics chcete zvýšit propustnost dat zpracování datového proudu</span><span class="sxs-lookup"><span data-stu-id="72b59-104">Scale Azure Stream Analytics jobs to increase stream data processing throughput</span></span>
<span data-ttu-id="72b59-105">Tento článek ukazuje, jak ladit dotaz služby Stream Analytics chcete zvýšit propustnost pro úlohy streamování Analytics.</span><span class="sxs-lookup"><span data-stu-id="72b59-105">This article shows you how to tune a Stream Analytics query to increase throughput for Streaming Analytics jobs.</span></span> <span data-ttu-id="72b59-106">Další postup škálování úlohy Stream Analytics podle konfigurace vstupní oddíly, ladění definice dotazu analýzy a výpočet a nastavení úlohy *jednotek streaming* (SUs).</span><span class="sxs-lookup"><span data-stu-id="72b59-106">You learn how to scale Stream Analytics jobs by configuring input partitions, tuning the analytics query definition, and calculating and setting job *streaming units* (SUs).</span></span> 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a><span data-ttu-id="72b59-107">Jaké jsou součástí úlohy Stream Analytics?</span><span class="sxs-lookup"><span data-stu-id="72b59-107">What are the parts of a Stream Analytics job?</span></span>
<span data-ttu-id="72b59-108">Definice úlohy Stream Analytics obsahuje vstupy, dotaz a výstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-108">A Stream Analytics job definition includes inputs, a query, and output.</span></span> <span data-ttu-id="72b59-109">Vstupní hodnoty jsou, kde úloha načte datový proud z.</span><span class="sxs-lookup"><span data-stu-id="72b59-109">Inputs are where the job reads the data stream from.</span></span> <span data-ttu-id="72b59-110">Dotaz se používá k transformaci dat vstupního datového proudu a výstup je, kde úloha odešle na výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="72b59-110">The query is used to transform the data input stream, and the output is where the job sends the job results to.</span></span>  

<span data-ttu-id="72b59-111">Úloha vyžaduje alespoň jeden vstupní zdroj pro streamování data.</span><span class="sxs-lookup"><span data-stu-id="72b59-111">A job requires at least one input source for data streaming.</span></span> <span data-ttu-id="72b59-112">Vstupní zdroj dat datového proudu můžete ukládat v Centru událostí Azure nebo v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="72b59-112">The data stream input source can be stored in an Azure event hub or in Azure blob storage.</span></span> <span data-ttu-id="72b59-113">Další informace najdete v tématu [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md) a [začít používat Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span><span class="sxs-lookup"><span data-stu-id="72b59-113">For more information, see [Introduction to Azure Stream Analytics](stream-analytics-introduction.md) and [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span></span>

## <a name="partitions-in-event-hubs-and-azure-storage"></a><span data-ttu-id="72b59-114">Oddíly v centrům událostí a úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="72b59-114">Partitions in event hubs and Azure storage</span></span>
<span data-ttu-id="72b59-115">Škálování úloha Stream Analytics využívá oddílů v vstup nebo výstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-115">Scaling a Stream Analytics job takes advantage of partitions in the input or output.</span></span> <span data-ttu-id="72b59-116">Vytváření oddílů umožňuje data rozdělíte do podmnožin podle klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="72b59-116">Partitioning lets you divide data into subsets based on a partition key.</span></span> <span data-ttu-id="72b59-117">Proces, který používá data (například úlohu streamování Analytics) může spotřebovávat a zápis různých oddílů souběžně, což zvyšuje propustnost.</span><span class="sxs-lookup"><span data-stu-id="72b59-117">A process that consumes the data (such as a Streaming Analytics job) can consume and write different partitions in parallel, which increases throughput.</span></span> <span data-ttu-id="72b59-118">Při práci s streamování analýzy, můžete využít výhod vytváření oddílů v centra událostí a v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="72b59-118">When you work with Streaming Analytics, you can take advantage of partitioning in event hubs and in Blob storage.</span></span> 

<span data-ttu-id="72b59-119">Další informace o oddílech najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="72b59-119">For more information about partitions, see the following articles:</span></span>

* [<span data-ttu-id="72b59-120">Přehled funkcí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="72b59-120">Event Hubs features overview</span></span>](../event-hubs/event-hubs-features.md#partitions)
* [<span data-ttu-id="72b59-121">Segmentace dat</span><span class="sxs-lookup"><span data-stu-id="72b59-121">Data partitioning</span></span>](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a><span data-ttu-id="72b59-122">Jednotky streamování (SUs)</span><span class="sxs-lookup"><span data-stu-id="72b59-122">Streaming units (SUs)</span></span>
<span data-ttu-id="72b59-123">Jednotky streamování (SUs) představují prostředků a výpočetní výkon, které jsou nutné, aby bylo možné spustit úlohu služby Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="72b59-123">Streaming units (SUs) represent the resources and computing power that are required in order to execute an Azure Stream Analytics job.</span></span> <span data-ttu-id="72b59-124">Jednotky SU umožňují popsat relativní kapacitu zpracování událostí na základě výkonu procesoru, paměti a rychlosti čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="72b59-124">SUs provide a way to describe the relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="72b59-125">Každý SU odpovídá přibližně 1 MB za sekundu propustnost.</span><span class="sxs-lookup"><span data-stu-id="72b59-125">Each SU corresponds to roughly 1 MB/second of throughput.</span></span> 

<span data-ttu-id="72b59-126">Výběr kolik služby SUs jsou požadovány pro konkrétní úlohy závisí na konfiguraci oddílů pro vstupy a na dotaz definovaný pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="72b59-126">Choosing how many SUs are required for a particular job depends on the partition configuration for the inputs and on the query defined for the job.</span></span> <span data-ttu-id="72b59-127">Můžete vybrat až vaší kvóty v služby SUs pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="72b59-127">You can select up to your quota in SUs for a job.</span></span> <span data-ttu-id="72b59-128">Ve výchozím nastavení má každé předplatné Azure kvótu až 50 služby SUs pro všechny úlohy analýzy v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="72b59-128">By default, each Azure subscription has a quota of up to 50 SUs for all the analytics jobs in a specific region.</span></span> <span data-ttu-id="72b59-129">Chcete-li zvýšit služby SUs pro vaše předplatné nad rámec této kvóty, obraťte se na [Microsoft Support](http://support.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="72b59-129">To increase SUs for your subscriptions beyond this quota, contact [Microsoft Support](http://support.microsoft.com).</span></span> <span data-ttu-id="72b59-130">Platné hodnoty pro služby SUs na úlohu jsou 1, 3, 6 a až v přírůstcích po 6.</span><span class="sxs-lookup"><span data-stu-id="72b59-130">Valid values for SUs per job are 1, 3, 6, and up in increments of 6.</span></span>

## <a name="embarrassingly-parallel-jobs"></a><span data-ttu-id="72b59-131">Trapně paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="72b59-131">Embarrassingly parallel jobs</span></span>
<span data-ttu-id="72b59-132">*Paralelně zpracovatelné* úlohy je nejvíce škálovatelným scénář máme v Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="72b59-132">An *embarrassingly parallel* job is the most scalable scenario we have in Azure Stream Analytics.</span></span> <span data-ttu-id="72b59-133">Připojí se jeden oddíl vstupu do jedné instance dotazu do jednoho oddílu výstupu.</span><span class="sxs-lookup"><span data-stu-id="72b59-133">It connects one partition of the input to one instance of the query to one partition of the output.</span></span> <span data-ttu-id="72b59-134">Tato paralelismus má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="72b59-134">This parallelism has the following requirements:</span></span>

1. <span data-ttu-id="72b59-135">Pokud logika dotazu závisí na stejný klíč zpracovávaných stejnou instanci dotazu, musí se ujistěte, že události přejděte do stejného oddílu váš vstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-135">If your query logic depends on the same key being processed by the same query instance, you must make sure that the events go to the same partition of your input.</span></span> <span data-ttu-id="72b59-136">Pro službu event hubs to znamená, že data události musí mít **PartitionKey** hodnotu sady.</span><span class="sxs-lookup"><span data-stu-id="72b59-136">For event hubs, this means that the event data must have the **PartitionKey** value set.</span></span> <span data-ttu-id="72b59-137">Alternativně můžete použít odesílatelé oddílů.</span><span class="sxs-lookup"><span data-stu-id="72b59-137">Alternatively, you can use partitioned senders.</span></span> <span data-ttu-id="72b59-138">Pro úložiště objektů blob to znamená, že události budou odeslány do stejné složky oddílu.</span><span class="sxs-lookup"><span data-stu-id="72b59-138">For blob storage, this means that the events are sent to the same partition folder.</span></span> <span data-ttu-id="72b59-139">Pokud dotaz logiky nevyžaduje stejný klíč zpracování na stejnou instanci dotazu, můžete ignorovat tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="72b59-139">If your query logic does not require the same key to be processed by the same query instance, you can ignore this requirement.</span></span> <span data-ttu-id="72b59-140">Příkladem této logiky, může být jednoduchý dotaz vyberte filtr projektu.</span><span class="sxs-lookup"><span data-stu-id="72b59-140">An example of this logic would be a simple select-project-filter query.</span></span>  

2. <span data-ttu-id="72b59-141">Jakmile data rozložená na vstupní straně, musí se ujistěte, že váš dotaz je rozdělený do oddílů.</span><span class="sxs-lookup"><span data-stu-id="72b59-141">Once the data is laid out on the input side, you must make sure that your query is partitioned.</span></span> <span data-ttu-id="72b59-142">To vyžaduje, abyste použili **Partition By** v všechny kroky.</span><span class="sxs-lookup"><span data-stu-id="72b59-142">This requires you to use **Partition By** in all the steps.</span></span> <span data-ttu-id="72b59-143">Jsou povoleny několik kroků, ale musí mít všechny oddíly pomocí stejného klíče.</span><span class="sxs-lookup"><span data-stu-id="72b59-143">Multiple steps are allowed, but they all must be partitioned by the same key.</span></span> <span data-ttu-id="72b59-144">V současné době rozdělení klíč musí být nastavena na **PartitionId** v pořadí pro úlohu, která má být plně paralelní.</span><span class="sxs-lookup"><span data-stu-id="72b59-144">Currently, the partitioning key must be set to **PartitionId** in order for the job to be fully parallel.</span></span>  

3. <span data-ttu-id="72b59-145">Pouze služba event hubs a úložiště objektů blob v současné době podporují dělené výstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-145">Currently only event hubs and blob storage support partitioned output.</span></span> <span data-ttu-id="72b59-146">Pro výstupu centra událostí, je nutné nakonfigurovat jako klíč oddílu **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="72b59-146">For event hub output, you must configure the partition key to be **PartitionId**.</span></span> <span data-ttu-id="72b59-147">Pro výstup úložiště objektů blob nemusíte provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="72b59-147">For blob storage output, you don't have to do anything.</span></span>  

4. <span data-ttu-id="72b59-148">Počet vstupních oddílů musí být roven počtu oddílů výstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-148">The number of input partitions must equal the number of output partitions.</span></span> <span data-ttu-id="72b59-149">Výstup úložiště objektů BLOB aktuálně nepodporuje oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-149">Blob storage output doesn't currently support partitions.</span></span> <span data-ttu-id="72b59-150">Ale to nevadí, protože dědí schéma rozdělení oddílů nadřazeného dotazu.</span><span class="sxs-lookup"><span data-stu-id="72b59-150">But that's okay, because it inherits the partitioning scheme of the upstream query.</span></span> <span data-ttu-id="72b59-151">Zde jsou příklady oddílu hodnot, které umožňují plně paralelní úlohy:</span><span class="sxs-lookup"><span data-stu-id="72b59-151">Here are examples of partition values that allow a fully parallel job:</span></span>  

   * <span data-ttu-id="72b59-152">8 oddíly vstupní centra událostí a Centrum událostí 8 výstupní oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-152">8 event hub input partitions and 8 event hub output partitions</span></span>
   * <span data-ttu-id="72b59-153">8 oddíly vstupní centra událostí a výstup úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="72b59-153">8 event hub input partitions and blob storage output</span></span>  
   * <span data-ttu-id="72b59-154">8 oddíly vstupní úložiště objektů blob a výstup úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="72b59-154">8 blob storage input partitions and blob storage output</span></span>  
   * <span data-ttu-id="72b59-155">8 blob oddílů pro úložiště a 8 oddíly výstupu centra událostí</span><span class="sxs-lookup"><span data-stu-id="72b59-155">8 blob storage input partitions and 8 event hub output partitions</span></span>  

<span data-ttu-id="72b59-156">Následující části popisují některé ukázkové scénáře, které jsou jednoduše paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="72b59-156">The following sections discuss some example scenarios that are embarrassingly parallel.</span></span>

### <a name="simple-query"></a><span data-ttu-id="72b59-157">Jednoduchý dotaz</span><span class="sxs-lookup"><span data-stu-id="72b59-157">Simple query</span></span>

* <span data-ttu-id="72b59-158">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-158">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-159">Výstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-159">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="72b59-160">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="72b59-160">Query:</span></span>

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

<span data-ttu-id="72b59-161">Dotaz je jednoduchý filtr.</span><span class="sxs-lookup"><span data-stu-id="72b59-161">This query is a simple filter.</span></span> <span data-ttu-id="72b59-162">Proto jsme nemusíte starat o vytváření oddílů vstup odeslaný do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="72b59-162">Therefore, we don't need to worry about partitioning the input that is being sent to the event hub.</span></span> <span data-ttu-id="72b59-163">Všimněte si, že dotaz obsahuje **oddílu podle PartitionId**, takže se splní požadavek #2 z dříve.</span><span class="sxs-lookup"><span data-stu-id="72b59-163">Notice that the query includes **Partition By PartitionId**, so it fulfills requirement #2 from earlier.</span></span> <span data-ttu-id="72b59-164">Pro výstup, je potřeba nakonfigurovat výstupu centra událostí v projektu na sadu klíče k vytvoření oddílu **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="72b59-164">For the output, we need to configure the event hub output in the job to have the parition key set to **PartitionId**.</span></span> <span data-ttu-id="72b59-165">Jeden poslední kontrola je počet vstupních oddílů musí být roven počtu oddílů výstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-165">One last check is to make sure that the number of input partitions is equal to the number of output partitions.</span></span>

### <a name="query-with-a-grouping-key"></a><span data-ttu-id="72b59-166">Dotaz s klíčem seskupení</span><span class="sxs-lookup"><span data-stu-id="72b59-166">Query with a grouping key</span></span>

* <span data-ttu-id="72b59-167">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-167">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-168">Výstup: Úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="72b59-168">Output: Blob storage</span></span>

<span data-ttu-id="72b59-169">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="72b59-169">Query:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="72b59-170">Tento dotaz obsahuje seskupení klíč.</span><span class="sxs-lookup"><span data-stu-id="72b59-170">This query has a grouping key.</span></span> <span data-ttu-id="72b59-171">Proto je potřeba zpracovat ve stejné instanci dotaz, což znamená, že musí události posílají do centra událostí oddílů způsobem stejný klíč.</span><span class="sxs-lookup"><span data-stu-id="72b59-171">Therefore, the same key needs to be processed by the same query instance, which means that events must be sent to the event hub in a partitioned manner.</span></span> <span data-ttu-id="72b59-172">Ale které klíč by měl použít?</span><span class="sxs-lookup"><span data-stu-id="72b59-172">But which key should be used?</span></span> <span data-ttu-id="72b59-173">**PartitionId** je koncept úlohy logiku.</span><span class="sxs-lookup"><span data-stu-id="72b59-173">**PartitionId** is a job-logic concept.</span></span> <span data-ttu-id="72b59-174">Klíč skutečně záleží nám je **TollBoothId**, proto **PartitionKey** hodnota data události by měla být **TollBoothId**.</span><span class="sxs-lookup"><span data-stu-id="72b59-174">The key we actually care about is **TollBoothId**, so the **PartitionKey** value of the event data should be **TollBoothId**.</span></span> <span data-ttu-id="72b59-175">Můžeme provést v dotazu nastavením **Partition By** k **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="72b59-175">We do this in the query by setting **Partition By** to **PartitionId**.</span></span> <span data-ttu-id="72b59-176">Vzhledem k tomu, že výstupem je úložiště objektů blob, jsme nemusíte si dělat starosti o konfiguraci hodnotu klíče oddílu, podle požadavků #4.</span><span class="sxs-lookup"><span data-stu-id="72b59-176">Since the output is blob storage, we don't need to worry about configuring a partition key value, as per requirement #4.</span></span>

### <a name="multi-step-query-with-a-grouping-key"></a><span data-ttu-id="72b59-177">Vícekrokový dotazu s klíčem seskupení</span><span class="sxs-lookup"><span data-stu-id="72b59-177">Multi-step query with a grouping key</span></span>
* <span data-ttu-id="72b59-178">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-178">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-179">Výstup: Instance rozbočovače událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-179">Output: Event hub instance with 8 partitions</span></span>

<span data-ttu-id="72b59-180">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="72b59-180">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="72b59-181">Tento dotaz obsahuje seskupení klíč, takže stejný klíč je potřeba zpracovat stejné instance dotazu.</span><span class="sxs-lookup"><span data-stu-id="72b59-181">This query has a grouping key, so the same key needs to be processed by the same query instance.</span></span> <span data-ttu-id="72b59-182">Můžeme použít stejné strategie jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="72b59-182">We can use the same strategy as in the previous example.</span></span> <span data-ttu-id="72b59-183">Dotaz v tomto případě má několik kroků.</span><span class="sxs-lookup"><span data-stu-id="72b59-183">In this case, the query has multiple steps.</span></span> <span data-ttu-id="72b59-184">Má každý krok **oddílu podle PartitionId**?</span><span class="sxs-lookup"><span data-stu-id="72b59-184">Does each step have **Partition By PartitionId**?</span></span> <span data-ttu-id="72b59-185">Ano, takže dotaz splní požadavek #3.</span><span class="sxs-lookup"><span data-stu-id="72b59-185">Yes, so the query fulfills requirement #3.</span></span> <span data-ttu-id="72b59-186">Výstup, je nutné nastavit klíč oddílu na **PartitionId**, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="72b59-186">For the output, we need to set the partition key to **PartitionId**, as discussed earlier.</span></span> <span data-ttu-id="72b59-187">Jsme můžete také zjistit, že nemá stejný počet oddílů jako vstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-187">We can also see that it has the same number of partitions as the input.</span></span>

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a><span data-ttu-id="72b59-188">Ukázkové scénáře, které jsou *není* paralelně zpracovatelné</span><span class="sxs-lookup"><span data-stu-id="72b59-188">Example scenarios that are *not* embarrassingly parallel</span></span>

<span data-ttu-id="72b59-189">V předchozí části jsme vám ukázal některé paralelně zpracovatelné scénáře.</span><span class="sxs-lookup"><span data-stu-id="72b59-189">In the previous section, we showed some embarrassingly parallel scenarios.</span></span> <span data-ttu-id="72b59-190">V této části probereme scénáře, které nejsou splněny všechny požadavky být jednoduše paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="72b59-190">In this section, we discuss scenarios that don't meet all the requirements to be embarrassingly parallel.</span></span> 

### <a name="mismatched-partition-count"></a><span data-ttu-id="72b59-191">Počet neodpovídající oddílů</span><span class="sxs-lookup"><span data-stu-id="72b59-191">Mismatched partition count</span></span>
* <span data-ttu-id="72b59-192">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-192">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-193">Výstup: Centra událostí s 32 oddílů</span><span class="sxs-lookup"><span data-stu-id="72b59-193">Output: Event hub with 32 partitions</span></span>

<span data-ttu-id="72b59-194">V takovém případě nezávisle na tom, co je dotaz.</span><span class="sxs-lookup"><span data-stu-id="72b59-194">In this case, it doesn't matter what the query is.</span></span> <span data-ttu-id="72b59-195">Pokud počet vstupních oddílů se neshoduje se počet oddílů výstup, není paralelně zpracovatelné topologii.</span><span class="sxs-lookup"><span data-stu-id="72b59-195">If the input partition count doesn't match the output partition count, the topology isn't embarrassingly parallel.</span></span>

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a><span data-ttu-id="72b59-196">Jako výstup není pomocí služby event hubs nebo úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="72b59-196">Not using event hubs or blob storage as output</span></span>
* <span data-ttu-id="72b59-197">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-197">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-198">Výstup: PowerBI</span><span class="sxs-lookup"><span data-stu-id="72b59-198">Output: PowerBI</span></span>

<span data-ttu-id="72b59-199">Výstup PowerBI aktuálně nepodporuje vytváření oddílů.</span><span class="sxs-lookup"><span data-stu-id="72b59-199">PowerBI output doesn't currently support partitioning.</span></span> <span data-ttu-id="72b59-200">Proto tento scénář není paralelně zpracovatelné.</span><span class="sxs-lookup"><span data-stu-id="72b59-200">Therefore, this scenario is not embarrassingly parallel.</span></span>

### <a name="multi-step-query-with-different-partition-by-values"></a><span data-ttu-id="72b59-201">Vícekrokový dotazu s různými hodnotami Partition By</span><span class="sxs-lookup"><span data-stu-id="72b59-201">Multi-step query with different Partition By values</span></span>
* <span data-ttu-id="72b59-202">Vstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-202">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="72b59-203">Výstup: Centra událostí s 8 oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-203">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="72b59-204">Dotaz:</span><span class="sxs-lookup"><span data-stu-id="72b59-204">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="72b59-205">Jak vidíte, druhý krok používá **TollBoothId** jako klíč rozdělení.</span><span class="sxs-lookup"><span data-stu-id="72b59-205">As you can see, the second step uses **TollBoothId** as the partitioning key.</span></span> <span data-ttu-id="72b59-206">Tento krok není stejný jako v prvním kroku, a proto vyžaduje nám udělat náhodně.</span><span class="sxs-lookup"><span data-stu-id="72b59-206">This step is not the same as the first step, and it therefore requires us to do a shuffle.</span></span> 

<span data-ttu-id="72b59-207">Některé úlohy Stream Analytics, které odpovídat (nebo nemusíte) paralelně zpracovatelné topologie v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="72b59-207">The preceding examples show some Stream Analytics jobs that conform to (or don't) an embarrassingly parallel topology.</span></span> <span data-ttu-id="72b59-208">Pokud jsou v souladu, mají potenciální pro maximální měřítko.</span><span class="sxs-lookup"><span data-stu-id="72b59-208">If they do conform, they have the potential for maximum scale.</span></span> <span data-ttu-id="72b59-209">Pro úlohy, které se nehodí jeden z těchto profilů škálování pokyny bude k dispozici v budoucích aktualizací.</span><span class="sxs-lookup"><span data-stu-id="72b59-209">For jobs that don't fit one of these profiles, scaling guidance will be available in future updates.</span></span> <span data-ttu-id="72b59-210">Prozatím použijte obecné pokyny v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="72b59-210">For now, use the general guidance in the following sections.</span></span>

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a><span data-ttu-id="72b59-211">Vypočítat maximální počet jednotek úlohy streamování</span><span class="sxs-lookup"><span data-stu-id="72b59-211">Calculate the maximum streaming units of a job</span></span>
<span data-ttu-id="72b59-212">Celkový počet jednotek streamování, které lze použít v rámci úlohy Stream Analytics závisí na počtu kroků v dotazu definovány pro úlohy a počet oddílů pro každý krok.</span><span class="sxs-lookup"><span data-stu-id="72b59-212">The total number of streaming units that can be used by a Stream Analytics job depends on the number of steps in the query defined for the job and the number of partitions for each step.</span></span>

### <a name="steps-in-a-query"></a><span data-ttu-id="72b59-213">Kroky v dotazu</span><span class="sxs-lookup"><span data-stu-id="72b59-213">Steps in a query</span></span>
<span data-ttu-id="72b59-214">Dotaz může mít jeden nebo mnoho kroků.</span><span class="sxs-lookup"><span data-stu-id="72b59-214">A query can have one or many steps.</span></span> <span data-ttu-id="72b59-215">Každý krok je poddotaz definované **WITH** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="72b59-215">Each step is a subquery defined by the **WITH** keyword.</span></span> <span data-ttu-id="72b59-216">Dotaz, který je mimo **WITH** – klíčové slovo (pouze jeden dotaz) také považován za krok, například **vyberte** příkaz v následujícím dotazu:</span><span class="sxs-lookup"><span data-stu-id="72b59-216">The query that is outside the **WITH** keyword (one query only) is also counted as a step, such as the **SELECT** statement in the following query:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

<span data-ttu-id="72b59-217">Tento dotaz má dva kroky.</span><span class="sxs-lookup"><span data-stu-id="72b59-217">This query has two steps.</span></span>

> [!NOTE]
> <span data-ttu-id="72b59-218">Tento dotaz je podrobněji popsána dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="72b59-218">This query is discussed in more detail later in the article.</span></span>
>  

### <a name="partition-a-step"></a><span data-ttu-id="72b59-219">Oddílu krok</span><span class="sxs-lookup"><span data-stu-id="72b59-219">Partition a step</span></span>
<span data-ttu-id="72b59-220">Vytváření oddílů krok vyžaduje následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="72b59-220">Partitioning a step requires the following conditions:</span></span>

* <span data-ttu-id="72b59-221">Vstupní zdroj, musí mít oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-221">The input source must be partitioned.</span></span> 
* <span data-ttu-id="72b59-222">**Vyberte** příkaz dotazu musí ke čtení z oddílů vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="72b59-222">The **SELECT** statement of the query must read from a partitioned input source.</span></span>
* <span data-ttu-id="72b59-223">Dotaz v rámci kroku, musí mít **Partition By** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="72b59-223">The query within the step must have the **Partition By** keyword.</span></span>

<span data-ttu-id="72b59-224">Při dotazu je rozdělena na oddíly, vstupních událostech se zpracování a agregované v samostatném oddílu skupiny a výstupy události jsou generovány pro každou skupinu.</span><span class="sxs-lookup"><span data-stu-id="72b59-224">When a query is partitioned, the input events are processed and aggregated in separate partition groups, and outputs events are generated for each of the groups.</span></span> <span data-ttu-id="72b59-225">Pokud chcete, aby součet agregace, musíte vytvořit druhý krok bez oddílů k agregaci.</span><span class="sxs-lookup"><span data-stu-id="72b59-225">If you want a combined aggregate, you must create a second non-partitioned step to aggregate.</span></span>

### <a name="calculate-the-max-streaming-units-for-a-job"></a><span data-ttu-id="72b59-226">Vypočítat maximální počet jednotek pro úlohu streamování</span><span class="sxs-lookup"><span data-stu-id="72b59-226">Calculate the max streaming units for a job</span></span>
<span data-ttu-id="72b59-227">Všechny kroky bez oddílů společně můžete škálovat až šest jednotky streamování (SUs) u úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="72b59-227">All non-partitioned steps together can scale up to six streaming units (SUs) for a Stream Analytics job.</span></span> <span data-ttu-id="72b59-228">Pokud chcete přidat služby SUs, musí mít oddíly krok.</span><span class="sxs-lookup"><span data-stu-id="72b59-228">To add SUs, a step must be partitioned.</span></span> <span data-ttu-id="72b59-229">Každý oddíl můžete mít šest služby SUs.</span><span class="sxs-lookup"><span data-stu-id="72b59-229">Each partition can have six SUs.</span></span>

<table border="1">
<tr><th><span data-ttu-id="72b59-230">Dotaz</span><span class="sxs-lookup"><span data-stu-id="72b59-230">Query</span></span></th><th><span data-ttu-id="72b59-231">Služba SUs Max pro úlohu</span><span class="sxs-lookup"><span data-stu-id="72b59-231">Max SUs for the job</span></span></th></td>

<tr><td>
<ul>
<li><span data-ttu-id="72b59-232">Dotaz obsahuje jeden krok.</span><span class="sxs-lookup"><span data-stu-id="72b59-232">The query contains one step.</span></span></li>
<li><span data-ttu-id="72b59-233">V kroku není rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-233">The step is not partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="72b59-234">6</span><span class="sxs-lookup"><span data-stu-id="72b59-234">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="72b59-235">Vstupní datový proud je rozdělena na oddíly 3.</span><span class="sxs-lookup"><span data-stu-id="72b59-235">The input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="72b59-236">Dotaz obsahuje jeden krok.</span><span class="sxs-lookup"><span data-stu-id="72b59-236">The query contains one step.</span></span></li>
<li><span data-ttu-id="72b59-237">V kroku je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-237">The step is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="72b59-238">18</span><span class="sxs-lookup"><span data-stu-id="72b59-238">18</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="72b59-239">Dotaz obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="72b59-239">The query contains two steps.</span></span></li>
<li><span data-ttu-id="72b59-240">Ani jeden z kroků je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-240">Neither of the steps is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="72b59-241">6</span><span class="sxs-lookup"><span data-stu-id="72b59-241">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="72b59-242">Vstupní datový proud je rozdělena na oddíly 3.</span><span class="sxs-lookup"><span data-stu-id="72b59-242">The input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="72b59-243">Dotaz obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="72b59-243">The query contains two steps.</span></span> <span data-ttu-id="72b59-244">Vstupní krok je rozdělena na oddíly a druhý krok není.</span><span class="sxs-lookup"><span data-stu-id="72b59-244">The input step is partitioned and the second step is not.</span></span></li>
<li><span data-ttu-id="72b59-245"><strong>Vyberte</strong> příkaz čte z oddílů vstup.</span><span class="sxs-lookup"><span data-stu-id="72b59-245">The <strong>SELECT</strong> statement reads from the partitioned input.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="72b59-246">24 (oddílů kroky 18) + 6 pokyny bez oddílů</span><span class="sxs-lookup"><span data-stu-id="72b59-246">24 (18 for partitioned steps + 6 for non-partitioned steps)</span></span></td></tr>
</table>

### <a name="examples-of-scaling"></a><span data-ttu-id="72b59-247">Příklady škálování</span><span class="sxs-lookup"><span data-stu-id="72b59-247">Examples of scaling</span></span>

<span data-ttu-id="72b59-248">Následující dotaz vypočítá počet aut, v rámci průchodu přes projedou stanice, která má tři tollbooths časového období tři minuty.</span><span class="sxs-lookup"><span data-stu-id="72b59-248">The following query calculates the number of cars within a three-minute window going through a toll station that has three tollbooths.</span></span> <span data-ttu-id="72b59-249">Tento dotaz je možné rozšířit až šest služby SUs.</span><span class="sxs-lookup"><span data-stu-id="72b59-249">This query can be scaled up to six SUs.</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="72b59-250">Pokud chcete používat další služby SUs pro dotaz, musí mít oddíly vstupní datový proud a dotazu.</span><span class="sxs-lookup"><span data-stu-id="72b59-250">To use more SUs for the query, both the input data stream and the query must be partitioned.</span></span> <span data-ttu-id="72b59-251">Vzhledem k tomu, že oddíl datový proud dat je nastaven na 3, tyto změny dotazu je možné rozšířit až 18 služby SUs:</span><span class="sxs-lookup"><span data-stu-id="72b59-251">Since the data stream partition is set to 3, the following modified query can be scaled up to 18 SUs:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="72b59-252">Při dotazu je rozdělena na oddíly, vstupních událostech se zpracovat a agregovat v samostatném oddílu skupiny.</span><span class="sxs-lookup"><span data-stu-id="72b59-252">When a query is partitioned, the input events are processed and aggregated in separate partition groups.</span></span> <span data-ttu-id="72b59-253">Výstup události jsou také vytvářeny pro každou skupinu.</span><span class="sxs-lookup"><span data-stu-id="72b59-253">Output events are also generated for each of the groups.</span></span> <span data-ttu-id="72b59-254">Dělení může způsobit, že některé neočekávané výsledky při **GROUP BY** pole není klíč oddílu v vstupní datový proud.</span><span class="sxs-lookup"><span data-stu-id="72b59-254">Partitioning can cause some unexpected results when the **GROUP BY** field is not the partition key in the input data stream.</span></span> <span data-ttu-id="72b59-255">Například **TollBoothId** pole v předchozího dotazu není klíč oddílu **Input1**.</span><span class="sxs-lookup"><span data-stu-id="72b59-255">For example, the **TollBoothId** field in the previous query is not the partition key of **Input1**.</span></span> <span data-ttu-id="72b59-256">Výsledkem je, že data z mýtná celnice č. 1 možné rozdělit do několika oddílů.</span><span class="sxs-lookup"><span data-stu-id="72b59-256">The result is that the data from TollBooth #1 can be spread in multiple partitions.</span></span>

<span data-ttu-id="72b59-257">Každý z **Input1** oddíly se zpracovávají odděleně podle Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="72b59-257">Each of the **Input1** partitions will be processed separately by Stream Analytics.</span></span> <span data-ttu-id="72b59-258">V důsledku toho bude vytvořen více záznamů počtu car pro stejné mýtná celnice ve stejném okně Přeskakující.</span><span class="sxs-lookup"><span data-stu-id="72b59-258">As a result, multiple records of the car count for the same tollbooth in the same Tumbling window will be created.</span></span> <span data-ttu-id="72b59-259">Pokud klíč vstupní oddílu nelze změnit, můžete tento problém opravit tak, že přidáte krok bez oddílu, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="72b59-259">If the input partition key can't be changed, this problem can be fixed by adding a non-partition step, as in the following example:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="72b59-260">Tento dotaz můžete škálovat na 24 služby SUs.</span><span class="sxs-lookup"><span data-stu-id="72b59-260">This query can be scaled to 24 SUs.</span></span>

> [!NOTE]
> <span data-ttu-id="72b59-261">Pokud jsou připojení dvě datové proudy, ujistěte se, že datové proudy jsou rozdělena na oddíly pomocí klíče oddílu sloupce, který použijete při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="72b59-261">If you are joining two streams, make sure that the streams are partitioned by the partition key of the column that you use to create the joins.</span></span> <span data-ttu-id="72b59-262">Ujistěte se také mít stejný počet oddílů v obou datových proudů.</span><span class="sxs-lookup"><span data-stu-id="72b59-262">Also make sure that you have the same number of partitions in both streams.</span></span>
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a><span data-ttu-id="72b59-263">Konfigurace služby Stream Analytics jednotky streamování</span><span class="sxs-lookup"><span data-stu-id="72b59-263">Configure Stream Analytics streaming units</span></span>

1. <span data-ttu-id="72b59-264">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="72b59-264">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="72b59-265">V seznamu prostředků najděte úlohu služby Stream Analytics, kterou chcete škálovat a potom ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="72b59-265">In the list of resources, find the Stream Analytics job that you want to scale and then open it.</span></span>
3. <span data-ttu-id="72b59-266">V okně úlohy v části **konfigurace**, klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="72b59-266">In the job blade, under **Configure**, click **Scale**.</span></span>

    ![Azure konfigurace portálu úlohy Stream Analytics][img.stream.analytics.preview.portal.settings.scale]

4. <span data-ttu-id="72b59-268">K nastavení služby SUs úlohy použijte posuvníku.</span><span class="sxs-lookup"><span data-stu-id="72b59-268">Use the slider to set the SUs for the job.</span></span> <span data-ttu-id="72b59-269">Všimněte si, že jste omezeni na konkrétní nastavení SU.</span><span class="sxs-lookup"><span data-stu-id="72b59-269">Notice that you are limited to specific SU settings.</span></span>


## <a name="monitor-job-performance"></a><span data-ttu-id="72b59-270">Monitorování výkonu úlohy</span><span class="sxs-lookup"><span data-stu-id="72b59-270">Monitor job performance</span></span>
<span data-ttu-id="72b59-271">Pomocí portálu Azure, můžete sledovat propustnost úlohy:</span><span class="sxs-lookup"><span data-stu-id="72b59-271">Using the Azure portal, you can track the throughput of a job:</span></span>

![Azure Stream Analytics monitorování úloh][img.stream.analytics.monitor.job]

<span data-ttu-id="72b59-273">Vypočítejte očekávané propustnost zatížení.</span><span class="sxs-lookup"><span data-stu-id="72b59-273">Calculate the expected throughput of the workload.</span></span> <span data-ttu-id="72b59-274">Pokud propustnost je menší než se očekávalo, ladit vstupní oddílu, odladění dotazu a přidejte služby SUs na úlohu.</span><span class="sxs-lookup"><span data-stu-id="72b59-274">If the throughput is less than expected, tune the input partition, tune the query, and add SUs to your job.</span></span>


## <a name="visualize-stream-analytics-throughput-at-scale-the-raspberry-pi-scenario"></a><span data-ttu-id="72b59-275">Vizualizace Stream Analytics propustnost škálované: scénář malin platformy</span><span class="sxs-lookup"><span data-stu-id="72b59-275">Visualize Stream Analytics throughput at scale: the Raspberry Pi scenario</span></span>
<span data-ttu-id="72b59-276">Abyste lépe pochopili, jak úlohy Stream Analytics škálování, jsme provedli experimentu založené na vstupu z malin platformy zařízení.</span><span class="sxs-lookup"><span data-stu-id="72b59-276">To help you understand how Stream Analytics jobs scale, we performed an experiment based on input from a Raspberry Pi device.</span></span> <span data-ttu-id="72b59-277">Tento experiment dejte nám najdete v části vliv na propustnost několika streamování jednotky a oddíly.</span><span class="sxs-lookup"><span data-stu-id="72b59-277">This experiment let us see the effect on throughput of multiple streaming units and partitions.</span></span>

<span data-ttu-id="72b59-278">V tomto scénáři zařízení odesílá data snímačů (klientů) do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="72b59-278">In this scenario, the device sends sensor data (clients) to an event hub.</span></span> <span data-ttu-id="72b59-279">Streamování Analytics zpracovává data a odešle výstrahu nebo statistiky jako výstup do jiného centra událostí.</span><span class="sxs-lookup"><span data-stu-id="72b59-279">Streaming Analytics processes the data and sends an alert or statistics as an output to another event hub.</span></span> 

<span data-ttu-id="72b59-280">Klient odešle data snímačů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="72b59-280">The client sends sensor data in JSON format.</span></span> <span data-ttu-id="72b59-281">Výstup dat je také ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="72b59-281">The data output is also in JSON format.</span></span> <span data-ttu-id="72b59-282">Data vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="72b59-282">The data looks like this:</span></span>

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

<span data-ttu-id="72b59-283">Následující dotaz se používá k odesílání výstrahu, pokud je vypnuté světlý:</span><span class="sxs-lookup"><span data-stu-id="72b59-283">The following query is used to send an alert when a light is switched off:</span></span>

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a><span data-ttu-id="72b59-284">Míra propustnost</span><span class="sxs-lookup"><span data-stu-id="72b59-284">Measure throughput</span></span>

<span data-ttu-id="72b59-285">V tomto kontextu propustnost je množství vstupních dat zpracovávaných Stream Analytics pevné množství času.</span><span class="sxs-lookup"><span data-stu-id="72b59-285">In this context, throughput is the amount of input data processed by Stream Analytics in a fixed amount of time.</span></span> <span data-ttu-id="72b59-286">(Jsme měří 10 minut.) K dosažení nejlepší propustnost zpracování vstupních dat, byly oddíly vstup datového streamu a dotazu.</span><span class="sxs-lookup"><span data-stu-id="72b59-286">(We measured for 10 minutes.) To achieve the best processing throughput for the input data, both the data stream input and the query were  partitioned.</span></span> <span data-ttu-id="72b59-287">Jsme zahrnuté **COUNT()** v dotazu k měření počtu vstupních událostí byly zpracovány.</span><span class="sxs-lookup"><span data-stu-id="72b59-287">We included **COUNT()** in the query to measure how many input events were processed.</span></span> <span data-ttu-id="72b59-288">Pokud chcete mít jistotu, že úloha nebyla jednoduše čekání vstupních událostech pochází, každý oddíl centra vstupní událostí se předem načtou přibližně 300 MB vstupní data.</span><span class="sxs-lookup"><span data-stu-id="72b59-288">To make sure the job was not simply waiting for input events to come, each partition of the input event hub was preloaded with about 300 MB of input data.</span></span>

<span data-ttu-id="72b59-289">V následující tabulce jsou uvedeny výsledků, které jsme viděli při odpovídající oddíl počty ve službě event hubs a jsme zvýšit počet jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="72b59-289">The following table shows the results we saw when we increased the number of streaming units and the corresponding partition counts in event hubs.</span></span>  

<table border="1">
<tr><th><span data-ttu-id="72b59-290">Vstupní oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-290">Input Partitions</span></span></th><th><span data-ttu-id="72b59-291">Výstup oddíly</span><span class="sxs-lookup"><span data-stu-id="72b59-291">Output Partitions</span></span></th><th><span data-ttu-id="72b59-292">Jednotky streamování</span><span class="sxs-lookup"><span data-stu-id="72b59-292">Streaming Units</span></span></th><th><span data-ttu-id="72b59-293">Propustnost</span><span class="sxs-lookup"><span data-stu-id="72b59-293">Sustained Throughput</span></span>
</th></td>

<tr><td><span data-ttu-id="72b59-294">12</span><span class="sxs-lookup"><span data-stu-id="72b59-294">12</span></span></td>
<td><span data-ttu-id="72b59-295">12</span><span class="sxs-lookup"><span data-stu-id="72b59-295">12</span></span></td>
<td><span data-ttu-id="72b59-296">6</span><span class="sxs-lookup"><span data-stu-id="72b59-296">6</span></span></td>
<td><span data-ttu-id="72b59-297">4.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-297">4.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="72b59-298">12</span><span class="sxs-lookup"><span data-stu-id="72b59-298">12</span></span></td>
<td><span data-ttu-id="72b59-299">12</span><span class="sxs-lookup"><span data-stu-id="72b59-299">12</span></span></td>
<td><span data-ttu-id="72b59-300">12</span><span class="sxs-lookup"><span data-stu-id="72b59-300">12</span></span></td>
<td><span data-ttu-id="72b59-301">8.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-301">8.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="72b59-302">48</span><span class="sxs-lookup"><span data-stu-id="72b59-302">48</span></span></td>
<td><span data-ttu-id="72b59-303">48</span><span class="sxs-lookup"><span data-stu-id="72b59-303">48</span></span></td>
<td><span data-ttu-id="72b59-304">48</span><span class="sxs-lookup"><span data-stu-id="72b59-304">48</span></span></td>
<td><span data-ttu-id="72b59-305">38.32 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-305">38.32 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="72b59-306">192</span><span class="sxs-lookup"><span data-stu-id="72b59-306">192</span></span></td>
<td><span data-ttu-id="72b59-307">192</span><span class="sxs-lookup"><span data-stu-id="72b59-307">192</span></span></td>
<td><span data-ttu-id="72b59-308">192</span><span class="sxs-lookup"><span data-stu-id="72b59-308">192</span></span></td>
<td><span data-ttu-id="72b59-309">172.67 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-309">172.67 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="72b59-310">480</span><span class="sxs-lookup"><span data-stu-id="72b59-310">480</span></span></td>
<td><span data-ttu-id="72b59-311">480</span><span class="sxs-lookup"><span data-stu-id="72b59-311">480</span></span></td>
<td><span data-ttu-id="72b59-312">480</span><span class="sxs-lookup"><span data-stu-id="72b59-312">480</span></span></td>
<td><span data-ttu-id="72b59-313">454.27 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-313">454.27 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="72b59-314">720</span><span class="sxs-lookup"><span data-stu-id="72b59-314">720</span></span></td>
<td><span data-ttu-id="72b59-315">720</span><span class="sxs-lookup"><span data-stu-id="72b59-315">720</span></span></td>
<td><span data-ttu-id="72b59-316">720</span><span class="sxs-lookup"><span data-stu-id="72b59-316">720</span></span></td>
<td><span data-ttu-id="72b59-317">609.69 MB/s</span><span class="sxs-lookup"><span data-stu-id="72b59-317">609.69 MB/s</span></span></td>
</tr>
</table>

<span data-ttu-id="72b59-318">A následující graf ukazuje vizualizaci vztah mezi službou SUs a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="72b59-318">And the following graph shows a visualization of the relationship between SUs and throughput.</span></span>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a><span data-ttu-id="72b59-320">Podpora</span><span class="sxs-lookup"><span data-stu-id="72b59-320">Get help</span></span>
<span data-ttu-id="72b59-321">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="72b59-321">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="72b59-322">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72b59-322">Next steps</span></span>
* [<span data-ttu-id="72b59-323">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72b59-323">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="72b59-324">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72b59-324">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="72b59-325">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="72b59-325">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="72b59-326">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72b59-326">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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

