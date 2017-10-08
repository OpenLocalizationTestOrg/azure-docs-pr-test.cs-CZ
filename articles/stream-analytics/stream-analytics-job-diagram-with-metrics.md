---
title: "AAA Azure Stream Analytics řízené daty ladění pomocí hello úlohy diagram | Microsoft Docs"
description: "Řešení potíží úlohu služby Stream Analytics pomocí diagram hello úlohy a metriky."
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
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a><span data-ttu-id="b91ea-103">Řízené daty ladění pomocí hello úlohy diagram</span><span class="sxs-lookup"><span data-stu-id="b91ea-103">Data-driven debugging by using hello job diagram</span></span>

<span data-ttu-id="b91ea-104">diagram Hello úlohy na hello **monitorování** okno v hello portálu Azure můžete vizualizovat vaše úlohy kanálu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-104">hello job diagram on hello **Monitoring** blade in hello Azure portal can help you visualize your job pipeline.</span></span> <span data-ttu-id="b91ea-105">Zobrazuje vstupy, výstupy a kroky dotazu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-105">It shows inputs, outputs, and query steps.</span></span> <span data-ttu-id="b91ea-106">Hello úlohy diagram tooexamine hello metriky můžete použít pro každý krok, toomore rychle izolovat hello zdroj problému při odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="b91ea-106">You can use hello job diagram tooexamine hello metrics for each step, toomore quickly isolate hello source of a problem when you troubleshoot issues.</span></span>

## <a name="using-hello-job-diagram"></a><span data-ttu-id="b91ea-107">Pomocí úlohy diagram hello</span><span class="sxs-lookup"><span data-stu-id="b91ea-107">Using hello job diagram</span></span>

<span data-ttu-id="b91ea-108">V hello portálu Azure při v úloze Stream Analytics, v části **podporu + Poradce při potížích s**, vyberte **úlohy diagram**:</span><span class="sxs-lookup"><span data-stu-id="b91ea-108">In hello Azure portal, while in a Stream Analytics job, under **SUPPORT + TROUBLESHOOTING**, select **Job diagram**:</span></span>

![Diagram úlohy s metriky - umístění](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

<span data-ttu-id="b91ea-110">V dotazu úpravy podokně vyberte každý krok toosee hello odpovídající oddílů dotazu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-110">Select each query step toosee hello corresponding section in a query editing pane.</span></span> <span data-ttu-id="b91ea-111">Metriky graf pro krok hello se zobrazí v dolním podokně na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="b91ea-111">A metric chart for hello step is displayed in a lower pane on hello page.</span></span>

![Diagram úlohy s metriky – základní úlohy](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

<span data-ttu-id="b91ea-113">Vyberte toosee hello oddíly hello vstupu Azure Event Hubs, **...**</span><span class="sxs-lookup"><span data-stu-id="b91ea-113">toosee hello partitions of hello Azure Event Hubs input, select **. . .**</span></span> <span data-ttu-id="b91ea-114">Zobrazí se kontextová nabídka.</span><span class="sxs-lookup"><span data-stu-id="b91ea-114">A context menu appears.</span></span> <span data-ttu-id="b91ea-115">Také můžete zobrazit vstupních fúzí hello.</span><span class="sxs-lookup"><span data-stu-id="b91ea-115">You also can see hello input merger.</span></span>

![Diagram úlohy s metriky - rozbalte oddíl](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

<span data-ttu-id="b91ea-117">Graf metriky toosee hello pro pouze jeden oddíl, vyberte hello oddílu uzlu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-117">toosee hello metric chart for only a single partition, select hello partition node.</span></span> <span data-ttu-id="b91ea-118">metriky Hello se zobrazí v dolní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="b91ea-118">hello metrics are shown at hello bottom of hello page.</span></span>

![Diagram úlohy s metriky - další metriky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

<span data-ttu-id="b91ea-120">Graf toosee hello metriky pro fúze, vyberte hello fúze uzlu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-120">toosee hello metrics chart for a merger, select hello merger node.</span></span> <span data-ttu-id="b91ea-121">Hello následující graf ukazuje, že žádné události byly zrušené nebo upravena.</span><span class="sxs-lookup"><span data-stu-id="b91ea-121">hello following chart shows that no events were dropped or adjusted.</span></span>

![Diagram úlohy s metriky - mřížky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

<span data-ttu-id="b91ea-123">Podrobnosti o hello toosee hello hodnota metriky a času, bod toohello grafu.</span><span class="sxs-lookup"><span data-stu-id="b91ea-123">toosee hello details of hello metric value and time, point toohello chart.</span></span>

![Úloha diagram s metriky – ukazatele](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a><span data-ttu-id="b91ea-125">Řešení potíží pomocí metrik</span><span class="sxs-lookup"><span data-stu-id="b91ea-125">Troubleshoot by using metrics</span></span>

<span data-ttu-id="b91ea-126">Hello **QueryLastProcessedTime** Metrika udává, kdy konkrétní krok přijal data.</span><span class="sxs-lookup"><span data-stu-id="b91ea-126">hello **QueryLastProcessedTime** metric indicates when a specific step received data.</span></span> <span data-ttu-id="b91ea-127">Prohlížením hello topologie, je možné pracovat zpátky z toosee procesoru výstup hello, který krok nepřijímá data.</span><span class="sxs-lookup"><span data-stu-id="b91ea-127">By looking at hello topology, you can work backward from hello output processor toosee which step is not receiving data.</span></span> <span data-ttu-id="b91ea-128">Pokud krok není získávání dat, přejděte toohello dotazu krok těsně před.</span><span class="sxs-lookup"><span data-stu-id="b91ea-128">If a step is not getting data, go toohello query step just before it.</span></span> <span data-ttu-id="b91ea-129">Zkontrolujte, jestli hello předchozí krok dotazu má časový interval, a pokud uplynutí dostatek času pro něj toooutput data.</span><span class="sxs-lookup"><span data-stu-id="b91ea-129">Check whether hello preceding query step has a time window, and if enough time has passed for it toooutput data.</span></span> <span data-ttu-id="b91ea-130">(Poznámka: Tento čas systému windows jsou zaseknutá toohello hodinu.)</span><span class="sxs-lookup"><span data-stu-id="b91ea-130">(Note that time windows are snapped toohello hour.)</span></span>
 
<span data-ttu-id="b91ea-131">Pokud hello předchozí krok dotazu vstupní procesoru, cílem použití hello vstupní metriky toohelp odpovědí hello následující otázky.</span><span class="sxs-lookup"><span data-stu-id="b91ea-131">If hello preceding query step is an input processor, use hello input metrics toohelp answer hello following targeted questions.</span></span> <span data-ttu-id="b91ea-132">Můžou pomoct určit, zda je úloha získání dat z jeho zadávání.</span><span class="sxs-lookup"><span data-stu-id="b91ea-132">They can help you determine whether a job is getting data from its input sources.</span></span> <span data-ttu-id="b91ea-133">Pokud se dotaz hello je rozdělena na oddíly, zkontrolujte každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="b91ea-133">If hello query is partitioned, examine each partition.</span></span>
 
### <a name="how-much-data-is-being-read"></a><span data-ttu-id="b91ea-134">Kolik dat se čte?</span><span class="sxs-lookup"><span data-stu-id="b91ea-134">How much data is being read?</span></span>

*   <span data-ttu-id="b91ea-135">**InputEventsSourcesTotal** je hello počet jednotek dat pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b91ea-135">**InputEventsSourcesTotal** is hello number of data units read.</span></span> <span data-ttu-id="b91ea-136">Například hello počet objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="b91ea-136">For example, hello number of blobs.</span></span>
*   <span data-ttu-id="b91ea-137">**InputEventsTotal** je hello počet událostí pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b91ea-137">**InputEventsTotal** is hello number of events read.</span></span> <span data-ttu-id="b91ea-138">Tato metrika je dostupná pro jednotlivé oddíly.</span><span class="sxs-lookup"><span data-stu-id="b91ea-138">This metric is available per partition.</span></span>
*   <span data-ttu-id="b91ea-139">**InputEventsInBytesTotal** je hello počet přečtených bajtů.</span><span class="sxs-lookup"><span data-stu-id="b91ea-139">**InputEventsInBytesTotal** is hello number of bytes read.</span></span>
*   <span data-ttu-id="b91ea-140">**InputEventsLastArrivalTime** je aktualizováno každých přijatou událost čas zařazených do fronty.</span><span class="sxs-lookup"><span data-stu-id="b91ea-140">**InputEventsLastArrivalTime** is updated with every received event's enqueued time.</span></span>
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a><span data-ttu-id="b91ea-141">Je čas postoupíte?</span><span class="sxs-lookup"><span data-stu-id="b91ea-141">Is time moving forward?</span></span> <span data-ttu-id="b91ea-142">Pokud se čtou skutečné události, nemusí být vydaný interpunkce.</span><span class="sxs-lookup"><span data-stu-id="b91ea-142">If actual events are read, punctuation might not be issued.</span></span>

*   <span data-ttu-id="b91ea-143">**InputEventsLastPunctuationTime** označuje interpunkce byl vydán při přesouvání čas tookeep dál.</span><span class="sxs-lookup"><span data-stu-id="b91ea-143">**InputEventsLastPunctuationTime** indicates when a punctuation was issued tookeep time moving forward.</span></span> <span data-ttu-id="b91ea-144">Pokud není objeví interpunkční znaménka, může zablokování datový tok.</span><span class="sxs-lookup"><span data-stu-id="b91ea-144">If punctuation is not issued, data flow can get blocked.</span></span>
 
### <a name="are-there-any-errors-in-hello-input"></a><span data-ttu-id="b91ea-145">Jsou všechny chyby hello vstup?</span><span class="sxs-lookup"><span data-stu-id="b91ea-145">Are there any errors in hello input?</span></span>

*   <span data-ttu-id="b91ea-146">**InputEventsEventDataNullTotal** je počet událostí, které mají hodnotu null data.</span><span class="sxs-lookup"><span data-stu-id="b91ea-146">**InputEventsEventDataNullTotal** is a count of events that have null data.</span></span>
*   <span data-ttu-id="b91ea-147">**InputEventsSerializerErrorsTotal** je počet událostí, které nelze deserializovat správně.</span><span class="sxs-lookup"><span data-stu-id="b91ea-147">**InputEventsSerializerErrorsTotal** is a count of events that could not be deserialized correctly.</span></span>
*   <span data-ttu-id="b91ea-148">**InputEventsDegradedTotal** je počet událostí, které se za potíže než s deserializace.</span><span class="sxs-lookup"><span data-stu-id="b91ea-148">**InputEventsDegradedTotal** is a count of events that had an issue other than with deserialization.</span></span>
 
### <a name="are-events-being-dropped-or-adjusted"></a><span data-ttu-id="b91ea-149">Jsou vyřazeny nebo upravena události?</span><span class="sxs-lookup"><span data-stu-id="b91ea-149">Are events being dropped or adjusted?</span></span>

*   <span data-ttu-id="b91ea-150">**InputEventsEarlyTotal** je hello počet událostí, které mají časové razítko aplikace před hello horní meze.</span><span class="sxs-lookup"><span data-stu-id="b91ea-150">**InputEventsEarlyTotal** is hello number of events that have an application timestamp before hello high watermark.</span></span>
*   <span data-ttu-id="b91ea-151">**InputEventsLateTotal** je hello počet událostí, které mají časové razítko aplikace po hello horní meze.</span><span class="sxs-lookup"><span data-stu-id="b91ea-151">**InputEventsLateTotal** is hello number of events that have an application timestamp after hello high watermark.</span></span>
*   <span data-ttu-id="b91ea-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** je číslo události hello vyřazen dříve, než čas spuštění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="b91ea-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** is hello number events dropped before hello job start time.</span></span>
 
### <a name="are-we-falling-behind-in-reading-data"></a><span data-ttu-id="b91ea-153">Jsme padají při čtení dat?</span><span class="sxs-lookup"><span data-stu-id="b91ea-153">Are we falling behind in reading data?</span></span>

*   <span data-ttu-id="b91ea-154">**InputEventsSourcesBackloggedTotal** zjistíte, kolik další zprávy nutné toobe čtení pro službu Event Hubs a Azure IoT Hub vstupy.</span><span class="sxs-lookup"><span data-stu-id="b91ea-154">**InputEventsSourcesBackloggedTotal** tells you how many more messages need toobe read for Event Hubs and Azure IoT Hub inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="b91ea-155">Podpora</span><span class="sxs-lookup"><span data-stu-id="b91ea-155">Get help</span></span>
<span data-ttu-id="b91ea-156">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="b91ea-156">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b91ea-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b91ea-157">Next steps</span></span>
* [<span data-ttu-id="b91ea-158">Úvod tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="b91ea-158">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="b91ea-159">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b91ea-159">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="b91ea-160">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b91ea-160">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="b91ea-161">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b91ea-161">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="b91ea-162">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="b91ea-162">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
