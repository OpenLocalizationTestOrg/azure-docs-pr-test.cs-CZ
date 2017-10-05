---
title: "Zpracování – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Informace o fungování služby Stream Analytics s mimo pořadí nebo pozdní události v datových proudů."
keywords: "mimo provoz, pozdní, události"
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
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: a48e48c5fc65d0ffbb7c23f426a4b06dcd568821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a><span data-ttu-id="78cd2-104">Zpracování pořadí událostí služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="78cd2-104">Azure Stream Analytics event order handling</span></span>

<span data-ttu-id="78cd2-105">V dočasné datovém proudu událostí každá událost se zaznamená s časem, které se získaly události.</span><span class="sxs-lookup"><span data-stu-id="78cd2-105">In a temporal data stream of events, each event is recorded with the time that the event is received.</span></span> <span data-ttu-id="78cd2-106">Některé podmínky může způsobit streamů událostí příležitostně přijímat některé události v jiném pořadí, než které byly odeslány.</span><span class="sxs-lookup"><span data-stu-id="78cd2-106">Some conditions might cause event streams to occasionally receive some events in a different order than which they were sent.</span></span> <span data-ttu-id="78cd2-107">Jednoduché opakování TCP nebo i hodiny zkosení mezi zařízením odesílání a příjmu centra událostí může dojít k tomu došlo.</span><span class="sxs-lookup"><span data-stu-id="78cd2-107">A simple TCP retransmit, or even a clock skew between the sending device and the receiving event hub might cause this to occur.</span></span> <span data-ttu-id="78cd2-108">"Interpunkce" události také se přidají do datových proudů přijatou událost posunut čas při absenci události doručení.</span><span class="sxs-lookup"><span data-stu-id="78cd2-108">“Punctuation” events also are added to received event streams, to advance the time in the absence of event arrivals.</span></span> <span data-ttu-id="78cd2-109">Je potřeba ve scénářích, jako je "Upozornit mě, pokud žádná přihlášení dojít k 3 minuty."</span><span class="sxs-lookup"><span data-stu-id="78cd2-109">These are needed in scenarios like “Notify me when no logins occur for 3 minutes."</span></span>

<span data-ttu-id="78cd2-110">Vstupní datové proudy, které nejsou v pořadí jsou buď:</span><span class="sxs-lookup"><span data-stu-id="78cd2-110">Input streams that are not in order are either:</span></span>
* <span data-ttu-id="78cd2-111">Seřadit (a proto **odložené**).</span><span class="sxs-lookup"><span data-stu-id="78cd2-111">Sorted (and therefore **delayed**).</span></span>
* <span data-ttu-id="78cd2-112">Upravit v systému podle zásady definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="78cd2-112">Adjusted by the system, according to a user-specified policy.</span></span>


## <a name="lateness-tolerance"></a><span data-ttu-id="78cd2-113">Tolerance zpoždění</span><span class="sxs-lookup"><span data-stu-id="78cd2-113">Lateness tolerance</span></span>
<span data-ttu-id="78cd2-114">Stream Analytics toleruje tyto typy scénáře.</span><span class="sxs-lookup"><span data-stu-id="78cd2-114">Stream Analytics tolerates these types of scenarios.</span></span> <span data-ttu-id="78cd2-115">Stream Analytics má zpracování pro "na více systémů pořadí" a "pozdní" události.</span><span class="sxs-lookup"><span data-stu-id="78cd2-115">Stream Analytics has handling for "out-of-order" and "late" events.</span></span> <span data-ttu-id="78cd2-116">Tyto události zpracovává následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="78cd2-116">It handles these events in the following ways:</span></span>

* <span data-ttu-id="78cd2-117">Události, které přicházejí mimo pořadí, ale v rámci sady tolerance **přeuspořádány pomocí časového razítka**.</span><span class="sxs-lookup"><span data-stu-id="78cd2-117">Events that arrive out of order but within the set tolerance are **reordered by timestamp**.</span></span>
* <span data-ttu-id="78cd2-118">Události, které přicházejí později než proti chybám jsou **vyřadit nebo upravena**.</span><span class="sxs-lookup"><span data-stu-id="78cd2-118">Events that arrive later than tolerance are **dropped or adjusted**.</span></span>
    * <span data-ttu-id="78cd2-119">**Upraví**: upravený tak, aby se zobrazí určeny na nejnovější přijatelný čas.</span><span class="sxs-lookup"><span data-stu-id="78cd2-119">**Adjusted**: Adjusted to appear to have arrived at the latest acceptable time.</span></span>
    * <span data-ttu-id="78cd2-120">**Vyřadit**: zahozeny.</span><span class="sxs-lookup"><span data-stu-id="78cd2-120">**Dropped**: Discarded.</span></span>

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-the-number-of-out-of-order-events"></a><span data-ttu-id="78cd2-122">Snižte počet mimo pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="78cd2-122">Reduce the number of out-of-order events</span></span>

<span data-ttu-id="78cd2-123">Protože Stream Analytics vztahuje dočasné transformace při zpracování příchozí události (například pro agregací v časových oknech nebo dočasných spojení), Stream Analytics seřadí příchozí události podle pořadí časové razítko.</span><span class="sxs-lookup"><span data-stu-id="78cd2-123">Because Stream Analytics applies a temporal transformation when it processes incoming events (for example, for windowed aggregates or temporal joins), Stream Analytics sorts incoming events by timestamp order.</span></span>

<span data-ttu-id="78cd2-124">Pokud klíčové slovo "časové razítko podle" je **není** používá, čas zařazování Azure Event Hubs události se používá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="78cd2-124">When the “timestamp by” keyword is **not** used, the Azure Event Hubs event enqueue time is used by default.</span></span> <span data-ttu-id="78cd2-125">Služba Event Hubs zaručuje monotonicity časového razítka v každém oddílu centra událostí.</span><span class="sxs-lookup"><span data-stu-id="78cd2-125">Event Hubs guarantees monotonicity of the timestamp on each partition of the event hub.</span></span> <span data-ttu-id="78cd2-126">Také zaručuje, že události ze všech oddílů se sloučí v pořadí časové razítko.</span><span class="sxs-lookup"><span data-stu-id="78cd2-126">It also guarantees that events from all partitions will be merged in timestamp order.</span></span> <span data-ttu-id="78cd2-127">Tyto dvě služby Event Hubs záruky zajistěte žádné události mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="78cd2-127">These two Event Hubs guarantees ensure no out-of-order events.</span></span>

<span data-ttu-id="78cd2-128">V některých případech je důležité budete moci použít časové razítko odesílatele.</span><span class="sxs-lookup"><span data-stu-id="78cd2-128">Sometimes, it’s important for you to use the sender’s timestamp.</span></span> <span data-ttu-id="78cd2-129">V takovém případě je zvolen časové razítko z datová část události pomocí "časového razítka podle."</span><span class="sxs-lookup"><span data-stu-id="78cd2-129">In that case, a timestamp from the event payload is chosen by using “timestamp by.”</span></span> <span data-ttu-id="78cd2-130">V těchto scénářích může být zavedl jednoho nebo více zdrojů misorder událostí:</span><span class="sxs-lookup"><span data-stu-id="78cd2-130">In these scenarios, one or more sources of event misorder might be introduced:</span></span>

* <span data-ttu-id="78cd2-131">Producenti událostí mít zkosí hodiny.</span><span class="sxs-lookup"><span data-stu-id="78cd2-131">Event producers have clock skews.</span></span> <span data-ttu-id="78cd2-132">To je běžné, když producenti jsou z různých počítačů, takže mají různé hodiny.</span><span class="sxs-lookup"><span data-stu-id="78cd2-132">This is common when producers are from different computers, so they have different clocks.</span></span>
* <span data-ttu-id="78cd2-133">Dochází ke zpoždění sítě ze zdroje událostí do centra událostí cílový.</span><span class="sxs-lookup"><span data-stu-id="78cd2-133">There's a network delay from the source of the events to the destination event hub.</span></span>
* <span data-ttu-id="78cd2-134">Hodiny zkosí existovat mezi oddílů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="78cd2-134">Clock skews exist between event hub partitions.</span></span> <span data-ttu-id="78cd2-135">Stream Analytics nejprve řadí události ze všech oddílů centra událostí podle zařazování čas události.</span><span class="sxs-lookup"><span data-stu-id="78cd2-135">Stream Analytics first sorts events from all event hub partitions by event enqueue time.</span></span> <span data-ttu-id="78cd2-136">Pak zkontroluje datový proud pro misordered události.</span><span class="sxs-lookup"><span data-stu-id="78cd2-136">Then, it examines the data stream for misordered events.</span></span>

<span data-ttu-id="78cd2-137">Na kartě konfigurace zobrazí následující výchozí nastavení:</span><span class="sxs-lookup"><span data-stu-id="78cd2-137">On the configuration tab, you see the following defaults:</span></span>

![Stream Analytics se na pořadí zpracování](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

<span data-ttu-id="78cd2-139">Pokud používáte 0 s jako interval tolerance pořadí se na více systémů, uplatňujete, že jsou všechny události v pořadí vždy.</span><span class="sxs-lookup"><span data-stu-id="78cd2-139">If you use 0 seconds as the out-of-order tolerance window, you are asserting that all events are in order all the time.</span></span> <span data-ttu-id="78cd2-140">Zadána tři zdroje misordered událostí nepravděpodobné, že se jedná o hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="78cd2-140">Given the three sources of misordered events, it’s unlikely that this is true.</span></span> 

<span data-ttu-id="78cd2-141">Povolit Stream Analytics opravit misorder události, můžete zadat interval tolerance nenulové mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="78cd2-141">To allow Stream Analytics to correct an event misorder, you can specify a non-zero out-of-order tolerance window.</span></span> <span data-ttu-id="78cd2-142">Stream Analytics ukládá vyrovnávací paměti události až toto okno a potom je změní pomocí časového razítka, které jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="78cd2-142">Stream Analytics buffers events up to that window, and then reorders them by using the timestamp you chose.</span></span> <span data-ttu-id="78cd2-143">Aplikace používá dočasné transformace.</span><span class="sxs-lookup"><span data-stu-id="78cd2-143">It then applies the temporal transformation.</span></span> <span data-ttu-id="78cd2-144">Můžete začínat okno 3 sekund a ladit hodnota a snížit počet událostí, které jsou upraveny čas.</span><span class="sxs-lookup"><span data-stu-id="78cd2-144">You can start with a 3-second window, and tune the value to reduce the number of events that are time-adjusted.</span></span> 

<span data-ttu-id="78cd2-145">Vedlejším účinkem ukládání do vyrovnávací paměti je, že je výstup **zpožděn stejnou dobu**.</span><span class="sxs-lookup"><span data-stu-id="78cd2-145">A side effect of the buffering is that the output is **delayed by the same amount of time**.</span></span> <span data-ttu-id="78cd2-146">Můžete ladit hodnota ke snížení počtu událostí na více systémů pořadí a zachovat nízkou latencí úlohy.</span><span class="sxs-lookup"><span data-stu-id="78cd2-146">You can tune the value to reduce the number of out-of-order events, and keep the job latency low.</span></span>

## <a name="get-help"></a><span data-ttu-id="78cd2-147">Podpora</span><span class="sxs-lookup"><span data-stu-id="78cd2-147">Get help</span></span>
<span data-ttu-id="78cd2-148">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="78cd2-148">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="78cd2-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78cd2-149">Next steps</span></span>
* [<span data-ttu-id="78cd2-150">Úvod do služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="78cd2-150">Introduction to Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="78cd2-151">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="78cd2-151">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="78cd2-152">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="78cd2-152">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="78cd2-153">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="78cd2-153">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="78cd2-154">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="78cd2-154">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)