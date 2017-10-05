---
title: "Azure Stream Analytics řízené daty ladění pomocí úlohy diagram | Microsoft Docs"
description: "Řešení potíží úlohu služby Stream Analytics pomocí úlohy diagram a metriky."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 4e5949232e8377b7697eaebf96eacdc31c4f5422
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a><span data-ttu-id="8e2d4-103">Řízené daty ladění pomocí diagram úlohy</span><span class="sxs-lookup"><span data-stu-id="8e2d4-103">Data-driven debugging by using the job diagram</span></span>

<span data-ttu-id="8e2d4-104">Diagram úlohy na **monitorování** okno na portálu Azure můžete vizualizovat vaše úlohy kanálu.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-104">The job diagram on the **Monitoring** blade in the Azure portal can help you visualize your job pipeline.</span></span> <span data-ttu-id="8e2d4-105">Zobrazuje vstupy, výstupy a kroky dotazu.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-105">It shows inputs, outputs, and query steps.</span></span> <span data-ttu-id="8e2d4-106">Diagram úlohy můžete použít k prozkoumání metriky pro každý krok, abyste rychleji izolovat zdroj problému při odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-106">You can use the job diagram to examine the metrics for each step, to more quickly isolate the source of a problem when you troubleshoot issues.</span></span>

## <a name="using-the-job-diagram"></a><span data-ttu-id="8e2d4-107">Pomocí úlohy diagram</span><span class="sxs-lookup"><span data-stu-id="8e2d4-107">Using the job diagram</span></span>

<span data-ttu-id="8e2d4-108">Na portálu Azure při v úloze Stream Analytics, v části **podporu + Poradce při potížích s**, vyberte **úlohy diagram**:</span><span class="sxs-lookup"><span data-stu-id="8e2d4-108">In the Azure portal, while in a Stream Analytics job, under **SUPPORT + TROUBLESHOOTING**, select **Job diagram**:</span></span>

![Diagram úlohy s metriky - umístění](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

<span data-ttu-id="8e2d4-110">Vyberte každý dotaz kroku najdete v části odpovídající v podokně úpravy dotazu.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-110">Select each query step to see the corresponding section in a query editing pane.</span></span> <span data-ttu-id="8e2d4-111">V dolním podokně na stránce se zobrazí metriky graf kroku.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-111">A metric chart for the step is displayed in a lower pane on the page.</span></span>

![Diagram úlohy s metriky – základní úlohy](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

<span data-ttu-id="8e2d4-113">Pokud chcete zobrazit oddíly vstupu Azure Event Hubs, vyberte **...**</span><span class="sxs-lookup"><span data-stu-id="8e2d4-113">To see the partitions of the Azure Event Hubs input, select **. . .**</span></span> <span data-ttu-id="8e2d4-114">Zobrazí se kontextová nabídka.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-114">A context menu appears.</span></span> <span data-ttu-id="8e2d4-115">Také můžete zobrazit vstupních fúzí.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-115">You also can see the input merger.</span></span>

![Diagram úlohy s metriky - rozbalte oddíl](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

<span data-ttu-id="8e2d4-117">Se zobrazí metriky graf pro pouze jeden oddíl, vyberte uzel oddílu.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-117">To see the metric chart for only a single partition, select the partition node.</span></span> <span data-ttu-id="8e2d4-118">Metriky se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-118">The metrics are shown at the bottom of the page.</span></span>

![Diagram úlohy s metriky - další metriky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

<span data-ttu-id="8e2d4-120">Pokud chcete zobrazit graf metriky pro spojení, vyberte uzel fúze.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-120">To see the metrics chart for a merger, select the merger node.</span></span> <span data-ttu-id="8e2d4-121">Následující tabulka ukazuje, že žádné události byly zrušené nebo upravena.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-121">The following chart shows that no events were dropped or adjusted.</span></span>

![Diagram úlohy s metriky - mřížky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

<span data-ttu-id="8e2d4-123">Pokud chcete zobrazit podrobnosti hodnota metriky a času, přejděte na grafu.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-123">To see the details of the metric value and time, point to the chart.</span></span>

![Úloha diagram s metriky – ukazatele](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a><span data-ttu-id="8e2d4-125">Řešení potíží pomocí metrik</span><span class="sxs-lookup"><span data-stu-id="8e2d4-125">Troubleshoot by using metrics</span></span>

<span data-ttu-id="8e2d4-126">**QueryLastProcessedTime** Metrika udává, kdy konkrétní krok přijal data.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-126">The **QueryLastProcessedTime** metric indicates when a specific step received data.</span></span> <span data-ttu-id="8e2d4-127">Pohledem na topologii, můžete použít pracovat zpátky z výstupu procesoru zobrazíte, který krok nepřijímá data.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-127">By looking at the topology, you can work backward from the output processor to see which step is not receiving data.</span></span> <span data-ttu-id="8e2d4-128">Pokud krok není získávání dat, přejděte ke kroku dotazu před ním.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-128">If a step is not getting data, go to the query step just before it.</span></span> <span data-ttu-id="8e2d4-129">Zkontrolujte, jestli má časový interval v předchozím kroku dotazu, a pokud uplynutí dostatek času pro něj na výstupní data.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-129">Check whether the preceding query step has a time window, and if enough time has passed for it to output data.</span></span> <span data-ttu-id="8e2d4-130">(Poznámka: Tento čas k hodinu jsou přichytávány systému windows.)</span><span class="sxs-lookup"><span data-stu-id="8e2d4-130">(Note that time windows are snapped to the hour.)</span></span>
 
<span data-ttu-id="8e2d4-131">Pokud v předchozím kroku dotazu je vstupní procesoru, využít vstupní metriky odpovědí na tyto cílové otázky.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-131">If the preceding query step is an input processor, use the input metrics to help answer the following targeted questions.</span></span> <span data-ttu-id="8e2d4-132">Můžou pomoct určit, zda je úloha získání dat z jeho zadávání.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-132">They can help you determine whether a job is getting data from its input sources.</span></span> <span data-ttu-id="8e2d4-133">Pokud je dotaz dělený, zkontrolujte všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-133">If the query is partitioned, examine each partition.</span></span>
 
### <a name="how-much-data-is-being-read"></a><span data-ttu-id="8e2d4-134">Kolik dat se čte?</span><span class="sxs-lookup"><span data-stu-id="8e2d4-134">How much data is being read?</span></span>

*   <span data-ttu-id="8e2d4-135">**InputEventsSourcesTotal** je počet jednotek dat pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-135">**InputEventsSourcesTotal** is the number of data units read.</span></span> <span data-ttu-id="8e2d4-136">Například počet objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-136">For example, the number of blobs.</span></span>
*   <span data-ttu-id="8e2d4-137">**InputEventsTotal** je počet událostí pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-137">**InputEventsTotal** is the number of events read.</span></span> <span data-ttu-id="8e2d4-138">Tato metrika je dostupná pro jednotlivé oddíly.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-138">This metric is available per partition.</span></span>
*   <span data-ttu-id="8e2d4-139">**InputEventsInBytesTotal** je počet přečtených bajtů.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-139">**InputEventsInBytesTotal** is the number of bytes read.</span></span>
*   <span data-ttu-id="8e2d4-140">**InputEventsLastArrivalTime** je aktualizováno každých přijatou událost čas zařazených do fronty.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-140">**InputEventsLastArrivalTime** is updated with every received event's enqueued time.</span></span>
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a><span data-ttu-id="8e2d4-141">Je čas postoupíte?</span><span class="sxs-lookup"><span data-stu-id="8e2d4-141">Is time moving forward?</span></span> <span data-ttu-id="8e2d4-142">Pokud se čtou skutečné události, nemusí být vydaný interpunkce.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-142">If actual events are read, punctuation might not be issued.</span></span>

*   <span data-ttu-id="8e2d4-143">**InputEventsLastPunctuationTime** udává, kdy došlo k přerušení pro zajištění dopředného běhu času.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-143">**InputEventsLastPunctuationTime** indicates when a punctuation was issued to keep time moving forward.</span></span> <span data-ttu-id="8e2d4-144">Pokud není objeví interpunkční znaménka, může zablokování datový tok.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-144">If punctuation is not issued, data flow can get blocked.</span></span>
 
### <a name="are-there-any-errors-in-the-input"></a><span data-ttu-id="8e2d4-145">Dochází k chybám ve vstupu?</span><span class="sxs-lookup"><span data-stu-id="8e2d4-145">Are there any errors in the input?</span></span>

*   <span data-ttu-id="8e2d4-146">**InputEventsEventDataNullTotal** je počet událostí, které mají hodnotu null data.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-146">**InputEventsEventDataNullTotal** is a count of events that have null data.</span></span>
*   <span data-ttu-id="8e2d4-147">**InputEventsSerializerErrorsTotal** je počet událostí, které nelze deserializovat správně.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-147">**InputEventsSerializerErrorsTotal** is a count of events that could not be deserialized correctly.</span></span>
*   <span data-ttu-id="8e2d4-148">**InputEventsDegradedTotal** je počet událostí, které se za potíže než s deserializace.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-148">**InputEventsDegradedTotal** is a count of events that had an issue other than with deserialization.</span></span>
 
### <a name="are-events-being-dropped-or-adjusted"></a><span data-ttu-id="8e2d4-149">Jsou vyřazeny nebo upravena události?</span><span class="sxs-lookup"><span data-stu-id="8e2d4-149">Are events being dropped or adjusted?</span></span>

*   <span data-ttu-id="8e2d4-150">**InputEventsEarlyTotal** je počet událostí, které mají časové razítko aplikace před horní meze.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-150">**InputEventsEarlyTotal** is the number of events that have an application timestamp before the high watermark.</span></span>
*   <span data-ttu-id="8e2d4-151">**InputEventsLateTotal** je počet událostí, které mají časové razítko aplikace po horní meze.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-151">**InputEventsLateTotal** is the number of events that have an application timestamp after the high watermark.</span></span>
*   <span data-ttu-id="8e2d4-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** je číslo události vyřazen dříve, než čas spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** is the number events dropped before the job start time.</span></span>
 
### <a name="are-we-falling-behind-in-reading-data"></a><span data-ttu-id="8e2d4-153">Jsme padají při čtení dat?</span><span class="sxs-lookup"><span data-stu-id="8e2d4-153">Are we falling behind in reading data?</span></span>

*   <span data-ttu-id="8e2d4-154">**InputEventsSourcesBackloggedTotal** zjistíte, kolik další zprávy nutné čtení pro službu Event Hubs a Azure IoT Hub vstupy.</span><span class="sxs-lookup"><span data-stu-id="8e2d4-154">**InputEventsSourcesBackloggedTotal** tells you how many more messages need to be read for Event Hubs and Azure IoT Hub inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="8e2d4-155">Podpora</span><span class="sxs-lookup"><span data-stu-id="8e2d4-155">Get help</span></span>
<span data-ttu-id="8e2d4-156">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="8e2d4-156">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e2d4-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e2d4-157">Next steps</span></span>
* [<span data-ttu-id="8e2d4-158">Úvod do služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e2d4-158">Introduction to Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8e2d4-159">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e2d4-159">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8e2d4-160">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e2d4-160">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8e2d4-161">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e2d4-161">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8e2d4-162">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="8e2d4-162">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
