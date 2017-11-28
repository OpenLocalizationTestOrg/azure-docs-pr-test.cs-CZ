---
title: "aaaHandling – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a><span data-ttu-id="b335e-104">Zpracování pořadí událostí služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b335e-104">Azure Stream Analytics event order handling</span></span>

<span data-ttu-id="b335e-105">V dočasné datovém proudu událostí, se zaznamenává všechny události s časem hello tuto událost hello přijetí.</span><span class="sxs-lookup"><span data-stu-id="b335e-105">In a temporal data stream of events, each event is recorded with hello time that hello event is received.</span></span> <span data-ttu-id="b335e-106">Některé podmínky může způsobit streamů událostí toooccasionally přijímat některé události v jiném pořadí, než které byly odeslány.</span><span class="sxs-lookup"><span data-stu-id="b335e-106">Some conditions might cause event streams toooccasionally receive some events in a different order than which they were sent.</span></span> <span data-ttu-id="b335e-107">Přenos jednoduché TCP nebo i zkosení hodin mezi hello odesílání zařízení a hello přijetí centra událostí může způsobit, že tento toooccur.</span><span class="sxs-lookup"><span data-stu-id="b335e-107">A simple TCP retransmit, or even a clock skew between hello sending device and hello receiving event hub might cause this toooccur.</span></span> <span data-ttu-id="b335e-108">"Interpunkce" události také přidají datových proudů událostí tooreceived, tooadvance hello čas v hello absence událostí doručení.</span><span class="sxs-lookup"><span data-stu-id="b335e-108">“Punctuation” events also are added tooreceived event streams, tooadvance hello time in hello absence of event arrivals.</span></span> <span data-ttu-id="b335e-109">Je potřeba ve scénářích, jako je "Upozornit mě, pokud žádná přihlášení dojít k 3 minuty."</span><span class="sxs-lookup"><span data-stu-id="b335e-109">These are needed in scenarios like “Notify me when no logins occur for 3 minutes."</span></span>

<span data-ttu-id="b335e-110">Vstupní datové proudy, které nejsou v pořadí jsou buď:</span><span class="sxs-lookup"><span data-stu-id="b335e-110">Input streams that are not in order are either:</span></span>
* <span data-ttu-id="b335e-111">Seřadit (a proto **odložené**).</span><span class="sxs-lookup"><span data-stu-id="b335e-111">Sorted (and therefore **delayed**).</span></span>
* <span data-ttu-id="b335e-112">Upraví systémem hello podle zásad tooa zadaného uživatelem.</span><span class="sxs-lookup"><span data-stu-id="b335e-112">Adjusted by hello system, according tooa user-specified policy.</span></span>


## <a name="lateness-tolerance"></a><span data-ttu-id="b335e-113">Tolerance zpoždění</span><span class="sxs-lookup"><span data-stu-id="b335e-113">Lateness tolerance</span></span>
<span data-ttu-id="b335e-114">Stream Analytics toleruje tyto typy scénáře.</span><span class="sxs-lookup"><span data-stu-id="b335e-114">Stream Analytics tolerates these types of scenarios.</span></span> <span data-ttu-id="b335e-115">Stream Analytics má zpracování pro "na více systémů pořadí" a "pozdní" události.</span><span class="sxs-lookup"><span data-stu-id="b335e-115">Stream Analytics has handling for "out-of-order" and "late" events.</span></span> <span data-ttu-id="b335e-116">Zpracovává tyto události v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="b335e-116">It handles these events in hello following ways:</span></span>

* <span data-ttu-id="b335e-117">Události, které dorazí mimo pořadí, ale v rámci hello nastavit tolerance **přeuspořádány pomocí časového razítka**.</span><span class="sxs-lookup"><span data-stu-id="b335e-117">Events that arrive out of order but within hello set tolerance are **reordered by timestamp**.</span></span>
* <span data-ttu-id="b335e-118">Události, které přicházejí později než proti chybám jsou **vyřadit nebo upravena**.</span><span class="sxs-lookup"><span data-stu-id="b335e-118">Events that arrive later than tolerance are **dropped or adjusted**.</span></span>
    * <span data-ttu-id="b335e-119">**Upraví**: upravenou tooappear toohave byly přijaty nejnovější přijatelný časový hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-119">**Adjusted**: Adjusted tooappear toohave arrived at hello latest acceptable time.</span></span>
    * <span data-ttu-id="b335e-120">**Vyřadit**: zahozeny.</span><span class="sxs-lookup"><span data-stu-id="b335e-120">**Dropped**: Discarded.</span></span>

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a><span data-ttu-id="b335e-122">Snižte počet hello na více systémů pořadí událostí</span><span class="sxs-lookup"><span data-stu-id="b335e-122">Reduce hello number of out-of-order events</span></span>

<span data-ttu-id="b335e-123">Protože Stream Analytics vztahuje dočasné transformace při zpracování příchozí události (například pro agregací v časových oknech nebo dočasných spojení), Stream Analytics seřadí příchozí události podle pořadí časové razítko.</span><span class="sxs-lookup"><span data-stu-id="b335e-123">Because Stream Analytics applies a temporal transformation when it processes incoming events (for example, for windowed aggregates or temporal joins), Stream Analytics sorts incoming events by timestamp order.</span></span>

<span data-ttu-id="b335e-124">Pokud je – klíčové slovo hello "časové razítko podle" **není** používá, čas zařazování události hello Azure Event Hubs se používá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b335e-124">When hello “timestamp by” keyword is **not** used, hello Azure Event Hubs event enqueue time is used by default.</span></span> <span data-ttu-id="b335e-125">Služba Event Hubs zaručuje monotonicity hello časového razítka v každém oddílu centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-125">Event Hubs guarantees monotonicity of hello timestamp on each partition of hello event hub.</span></span> <span data-ttu-id="b335e-126">Také zaručuje, že události ze všech oddílů se sloučí v pořadí časové razítko.</span><span class="sxs-lookup"><span data-stu-id="b335e-126">It also guarantees that events from all partitions will be merged in timestamp order.</span></span> <span data-ttu-id="b335e-127">Tyto dvě služby Event Hubs záruky zajistěte žádné události mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="b335e-127">These two Event Hubs guarantees ensure no out-of-order events.</span></span>

<span data-ttu-id="b335e-128">V některých případech je důležité pro časové razítko je toouse hello odesílatele.</span><span class="sxs-lookup"><span data-stu-id="b335e-128">Sometimes, it’s important for you toouse hello sender’s timestamp.</span></span> <span data-ttu-id="b335e-129">V takovém případě je zvolen časové razítko z datová část události hello pomocí "časového razítka podle."</span><span class="sxs-lookup"><span data-stu-id="b335e-129">In that case, a timestamp from hello event payload is chosen by using “timestamp by.”</span></span> <span data-ttu-id="b335e-130">V těchto scénářích může být zavedl jednoho nebo více zdrojů misorder událostí:</span><span class="sxs-lookup"><span data-stu-id="b335e-130">In these scenarios, one or more sources of event misorder might be introduced:</span></span>

* <span data-ttu-id="b335e-131">Producenti událostí mít zkosí hodiny.</span><span class="sxs-lookup"><span data-stu-id="b335e-131">Event producers have clock skews.</span></span> <span data-ttu-id="b335e-132">To je běžné, když producenti jsou z různých počítačů, takže mají různé hodiny.</span><span class="sxs-lookup"><span data-stu-id="b335e-132">This is common when producers are from different computers, so they have different clocks.</span></span>
* <span data-ttu-id="b335e-133">Dochází ke zpoždění sítě ze zdroje hello centra událostí cílové toohello události hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-133">There's a network delay from hello source of hello events toohello destination event hub.</span></span>
* <span data-ttu-id="b335e-134">Hodiny zkosí existovat mezi oddílů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b335e-134">Clock skews exist between event hub partitions.</span></span> <span data-ttu-id="b335e-135">Stream Analytics nejprve řadí události ze všech oddílů centra událostí podle zařazování čas události.</span><span class="sxs-lookup"><span data-stu-id="b335e-135">Stream Analytics first sorts events from all event hub partitions by event enqueue time.</span></span> <span data-ttu-id="b335e-136">Pak zkontroluje hello datový proud pro misordered události.</span><span class="sxs-lookup"><span data-stu-id="b335e-136">Then, it examines hello data stream for misordered events.</span></span>

<span data-ttu-id="b335e-137">Na kartě Konfigurace hello najdete v části hello následující výchozí nastavení:</span><span class="sxs-lookup"><span data-stu-id="b335e-137">On hello configuration tab, you see hello following defaults:</span></span>

![Stream Analytics se na pořadí zpracování](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

<span data-ttu-id="b335e-139">Pokud používáte 0 s jako interval tolerance na více systémů pořadí hello, uplatňujete, že jsou všechny události v pořadí celou dobu hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-139">If you use 0 seconds as hello out-of-order tolerance window, you are asserting that all events are in order all hello time.</span></span> <span data-ttu-id="b335e-140">Zadané hello tři zdroje misordered událostí, nepravděpodobné, že se jedná o hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="b335e-140">Given hello three sources of misordered events, it’s unlikely that this is true.</span></span> 

<span data-ttu-id="b335e-141">Stream Analytics toocorrect tooallow misorder události, můžete zadat interval tolerance nenulové mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="b335e-141">tooallow Stream Analytics toocorrect an event misorder, you can specify a non-zero out-of-order tolerance window.</span></span> <span data-ttu-id="b335e-142">Stream Analytics vyrovnávací paměti události toothat okna a potom přiobjednávky je pomocí hello časové razítko, které jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="b335e-142">Stream Analytics buffers events up toothat window, and then reorders them by using hello timestamp you chose.</span></span> <span data-ttu-id="b335e-143">Aplikace používá dočasné transformace hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-143">It then applies hello temporal transformation.</span></span> <span data-ttu-id="b335e-144">Můžete začít s okno 3 sekundu a ladit hello hodnota tooreduce hello počet událostí, které jsou upraveny čas.</span><span class="sxs-lookup"><span data-stu-id="b335e-144">You can start with a 3-second window, and tune hello value tooreduce hello number of events that are time-adjusted.</span></span> 

<span data-ttu-id="b335e-145">Vedlejším účinkem ukládání do vyrovnávací paměti hello je, že je výstup hello **zpožděn hello stejné množství času**.</span><span class="sxs-lookup"><span data-stu-id="b335e-145">A side effect of hello buffering is that hello output is **delayed by hello same amount of time**.</span></span> <span data-ttu-id="b335e-146">Můžete ladit hello hodnota tooreduce hello počet událostí na více systémů pořadí a zachovat nízkou latencí úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="b335e-146">You can tune hello value tooreduce hello number of out-of-order events, and keep hello job latency low.</span></span>

## <a name="get-help"></a><span data-ttu-id="b335e-147">Podpora</span><span class="sxs-lookup"><span data-stu-id="b335e-147">Get help</span></span>
<span data-ttu-id="b335e-148">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="b335e-148">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b335e-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b335e-149">Next steps</span></span>
* [<span data-ttu-id="b335e-150">Úvod tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="b335e-150">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="b335e-151">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b335e-151">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="b335e-152">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b335e-152">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="b335e-153">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b335e-153">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="b335e-154">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="b335e-154">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)