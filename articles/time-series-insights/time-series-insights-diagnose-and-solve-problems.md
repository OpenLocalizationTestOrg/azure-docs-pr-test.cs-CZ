---
title: "Diagnostika a řešení problémů | Microsoft Docs"
description: "Tento kurz se zaměřuje jak diagnostikovat a řešit problémy ve vašem prostředí Statistika časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4b8d5fdab1744b2db658917f91d6dac05db30d2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a><span data-ttu-id="f858b-103">Diagnostika a řešení problémů ve vašem prostředí Statistika časové řady</span><span class="sxs-lookup"><span data-stu-id="f858b-103">Diagnose and solve problems in your Time Series Insights environment</span></span>

## <a name="i-dont-see-my-data"></a><span data-ttu-id="f858b-104">Data se nezobrazí</span><span class="sxs-lookup"><span data-stu-id="f858b-104">I don't see my data</span></span>
<span data-ttu-id="f858b-105">Tady jsou některé důvody, proč nemusí zobrazit vaše data ve vašem prostředí v [portálu Statistika řady čas Azure](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f858b-105">Here are some reasons why you might not see your data in your environment in the [Azure Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-event-source-doesnt-have-data-in-json-format"></a><span data-ttu-id="f858b-106">Váš zdroj událostí nemá data ve formátu JSON</span><span class="sxs-lookup"><span data-stu-id="f858b-106">Your event source doesn't have data in JSON format</span></span>
<span data-ttu-id="f858b-107">Statistiky Azure řady čas podporuje pouze data JSON ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="f858b-107">Azure Time Series Insights supports only JSON data today.</span></span> <span data-ttu-id="f858b-108">Ukázky JSON naleznete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="f858b-108">For JSON samples, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a><span data-ttu-id="f858b-109">Při registraci zdroje událostí neposkytli klíč, který má požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="f858b-109">When you registered your event source, you didn't provide the key that has the required permission</span></span>
* <span data-ttu-id="f858b-110">Pro Centrum IoT, budete muset zadat klíč, který má **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f858b-110">For an IoT hub, you need to provide the key that has **service connect** permission.</span></span>

   ![Povolení pro připojení služby IoT hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   <span data-ttu-id="f858b-112">Jak je vidět na předchozím obrázku, buď zásady **iothubowner** a **služby** by pracovat, protože mají **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f858b-112">As shown in the preceding image, either of the policies **iothubowner** and **service** would work, because both have **service connect** permission.</span></span>
* <span data-ttu-id="f858b-113">Pro centra událostí, budete muset zadat klíč, který má **naslouchání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f858b-113">For an event hub, you need to provide the key that has **Listen** permission.</span></span>

   ![Oprávnění naslouchání centra událostí](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   <span data-ttu-id="f858b-115">Jak je vidět na předchozím obrázku, buď zásady **číst** a **spravovat** by pracovat, protože mají **naslouchání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f858b-115">As shown in the preceding image, either of the policies **read** and **manage** would work, because both have **Listen** permission.</span></span>

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a><span data-ttu-id="f858b-116">Skupina zadaná příjemce není výhradně pro přehledy časové řady</span><span class="sxs-lookup"><span data-stu-id="f858b-116">The provided consumer group is not exclusive to Time Series Insights</span></span>
<span data-ttu-id="f858b-117">Centrum IoT nebo centra událostí během registrace jsme vyžadují zadání skupiny uživatelů, který se má použít pro čtení vaše data.</span><span class="sxs-lookup"><span data-stu-id="f858b-117">For an IoT hub or an event hub, during registration we require you to specify the consumer group that should be used for reading your data.</span></span> <span data-ttu-id="f858b-118">Tato skupina uživatelů nesmí sdílet.</span><span class="sxs-lookup"><span data-stu-id="f858b-118">This consumer group must not be shared.</span></span> <span data-ttu-id="f858b-119">Když ho sdílíte, základní centra událostí automaticky odpojí jeden čtečky náhodně.</span><span class="sxs-lookup"><span data-stu-id="f858b-119">If it's shared, the underlying event hub automatically disconnects one of the readers randomly.</span></span>

## <a name="i-see-my-data-but-theres-a-lag"></a><span data-ttu-id="f858b-120">Vidět data, ale existuje prodleva</span><span class="sxs-lookup"><span data-stu-id="f858b-120">I see my data, but there's a lag</span></span>
<span data-ttu-id="f858b-121">Tady jsou z důvodů, proč může se zobrazit částečná data ve vašem prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f858b-121">Here are reasons why you might see partial data in your environment in the [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-environment-is-getting-throttled"></a><span data-ttu-id="f858b-122">Prostředí je získávání omezen.</span><span class="sxs-lookup"><span data-stu-id="f858b-122">Your environment is getting throttled</span></span>
<span data-ttu-id="f858b-123">Omezení limit vynucený na základě typu SKU v prostředí a kapacity.</span><span class="sxs-lookup"><span data-stu-id="f858b-123">The throttling limit is enforced based on the environment's SKU type and capacity.</span></span> <span data-ttu-id="f858b-124">Všechny zdroje událostí v prostředí sdílet tento kapacitu.</span><span class="sxs-lookup"><span data-stu-id="f858b-124">All event sources in the environment share this capacity.</span></span> <span data-ttu-id="f858b-125">Pokud zdroj události pro vaše Centrum IoT nebo Centrum událostí je předání dat nad rámec Vynucené limity, zobrazí omezení a prodleva.</span><span class="sxs-lookup"><span data-stu-id="f858b-125">If the event source for your IoT hub or event hub is pushing data beyond the enforced limits, you see throttling and a lag.</span></span>

<span data-ttu-id="f858b-126">Následující diagram znázorňuje časové řady Statistika prostředí, které má SKU S1 a kapacitou 3.</span><span class="sxs-lookup"><span data-stu-id="f858b-126">The following diagram shows a Time Series Insights environment that has a SKU of S1 and a capacity of 3.</span></span> <span data-ttu-id="f858b-127">Můžete ho příjem příchozích dat 3 miliony událostí za den.</span><span class="sxs-lookup"><span data-stu-id="f858b-127">It can ingress 3 million events per day.</span></span>

![Aktuální kapacita SKU prostředí](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

<span data-ttu-id="f858b-129">Předpokládejme, že toto prostředí je příjem zpráv z centra událostí míru příjem příchozích dat vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="f858b-129">Assume that this environment is ingesting messages from an event hub with the ingress rate shown in the following diagram:</span></span>

![Příklad příchozího rychlost pro centra událostí](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

<span data-ttu-id="f858b-131">Jak je vidět v diagramu, denní míra příjem příchozích dat je ~ 67,000 zprávy.</span><span class="sxs-lookup"><span data-stu-id="f858b-131">As shown in the diagram, the daily ingress rate is ~67,000 messages.</span></span> <span data-ttu-id="f858b-132">Tato míra překládá zhruba na 46 zprávy každou minutu.</span><span class="sxs-lookup"><span data-stu-id="f858b-132">This rate translates roughly to 46 messages every minute.</span></span> <span data-ttu-id="f858b-133">Pokud každou zprávu centra událostí je průmětu do jedné události časové řady statistiky, se zobrazí toto prostředí žádné omezení.</span><span class="sxs-lookup"><span data-stu-id="f858b-133">If each event hub message is flattened to a single Time Series Insights event, this environment sees no throttling.</span></span> <span data-ttu-id="f858b-134">Pokud každá zpráva centra událostí se sloučí na 100 Statistika řady čas události, pak 4,600 události by měl konzumaci každou minutu.</span><span class="sxs-lookup"><span data-stu-id="f858b-134">If each event hub message is flattened to 100 Time Series Insights events, then 4,600 events should be ingested every minute.</span></span> <span data-ttu-id="f858b-135">S1 SKU prostředí, které má kapacitou 3 můžete pouze 2100 události příchozího každou minutu (1 milion událostí za den = 700 události za minutu ve 3 jednotky = 2100 události za minutu).</span><span class="sxs-lookup"><span data-stu-id="f858b-135">An S1 SKU environment that has a capacity of 3 can ingress only 2,100 events every minute (1 million events per day = 700 events per minute at 3 units = 2,100 events per minute).</span></span> <span data-ttu-id="f858b-136">Proto najdete v části funkce lag kvůli omezování.</span><span class="sxs-lookup"><span data-stu-id="f858b-136">Therefore you see a lag due to throttling.</span></span> 

<span data-ttu-id="f858b-137">Do hloubky pochopili, jak vyrovnání logiku funguje, najdete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="f858b-137">For a high-level understanding of how flattening logic works, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="f858b-138">Doporučené kroky</span><span class="sxs-lookup"><span data-stu-id="f858b-138">Recommended steps</span></span>
<span data-ttu-id="f858b-139">Pokud chcete vyřešit je zpoždění, zvýšit kapacitu SKU vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="f858b-139">To fix the lag, increase the SKU capacity of your environment.</span></span> <span data-ttu-id="f858b-140">Další informace najdete v tématu [postup škálování prostředí časové řady Insights](time-series-insights-how-to-scale-your-environment.md).</span><span class="sxs-lookup"><span data-stu-id="f858b-140">For more information, see [How to scale your Time Series Insights environment](time-series-insights-how-to-scale-your-environment.md).</span></span>

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a><span data-ttu-id="f858b-141">Jste předání historických dat a způsobuje pomalé příjem příchozích dat</span><span class="sxs-lookup"><span data-stu-id="f858b-141">You're pushing historical data and causing slow ingress</span></span>
<span data-ttu-id="f858b-142">Pokud se připojujete existujícího zdroje událostí, je pravděpodobné, že Centrum IoT nebo Centrum událostí už obsahuje data v ní.</span><span class="sxs-lookup"><span data-stu-id="f858b-142">If you are connecting an existing event source, it's likely that your IoT hub or event hub already has data in it.</span></span> <span data-ttu-id="f858b-143">Proto prostředí spustí, stahování dat od začátku dobu uchování zpráv zdroj události.</span><span class="sxs-lookup"><span data-stu-id="f858b-143">So the environment starts pulling data from the beginning of the event source's message retention period.</span></span> 

<span data-ttu-id="f858b-144">Toto chování je výchozí chování a nelze přepsat.</span><span class="sxs-lookup"><span data-stu-id="f858b-144">This behavior is the default behavior and cannot be overridden.</span></span> <span data-ttu-id="f858b-145">Můžete použít omezení, a že může trvat dobu aktualizovány na příjem historická data.</span><span class="sxs-lookup"><span data-stu-id="f858b-145">You can engage throttling, and it may take a while to catch up on ingesting historical data.</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="f858b-146">Doporučené kroky</span><span class="sxs-lookup"><span data-stu-id="f858b-146">Recommended steps</span></span>
<span data-ttu-id="f858b-147">Pokud chcete vyřešit je zpoždění, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f858b-147">To fix the lag, take the following steps:</span></span>
1. <span data-ttu-id="f858b-148">Zvětšete kapacitu SKU pro maximální povolenou hodnotu (v tomto případě 10).</span><span class="sxs-lookup"><span data-stu-id="f858b-148">Increase the SKU capacity to the maximum allowed value (10 in this case).</span></span> <span data-ttu-id="f858b-149">Po kapacitu vzroste, spustí proces příjem příchozích dat, zachytávání až mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="f858b-149">After the capacity is increased, the ingress process starts catching up much faster.</span></span> <span data-ttu-id="f858b-150">Můžete vizualizovat, jak rychle jste jste zachytávání prostřednictvím grafu dostupnosti v [portálu Statistika časové řady](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f858b-150">You can visualize how quickly you're catching up through the availability chart in the [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span> <span data-ttu-id="f858b-151">Budou účtovat zvýšené kapacity.</span><span class="sxs-lookup"><span data-stu-id="f858b-151">You are charged for the increased capacity.</span></span>
2. <span data-ttu-id="f858b-152">Jakmile je zpoždění je zachycena, snížit kapacita SKU zpět do vaší normální příchozí míra.</span><span class="sxs-lookup"><span data-stu-id="f858b-152">After the lag is caught up, decrease the SKU capacity back to your normal ingress rate.</span></span>

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a><span data-ttu-id="f858b-153">Zdroj události *název vlastnosti časové razítko* nastavení nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="f858b-153">My event source's *timestamp property name* setting doesn't work</span></span>
<span data-ttu-id="f858b-154">Ujistěte se, že název a hodnotu odpovídají následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="f858b-154">Ensure that the name and value conform to the following rules:</span></span>
* <span data-ttu-id="f858b-155">Název vlastnosti časové razítko je _malá a velká písmena_.</span><span class="sxs-lookup"><span data-stu-id="f858b-155">The timestamp property name is _case-sensitive_.</span></span>
* <span data-ttu-id="f858b-156">Hodnota vlastnosti časové razítko, které pochází ze zdroje událostí, jako řetězec formátu JSON by měl mít formát _rrrr-MM-ddTHH. FFFFFFFK_.</span><span class="sxs-lookup"><span data-stu-id="f858b-156">The timestamp property value that's coming from your event source, as a JSON string, should have the format _yyyy-MM-ddTHH:mm:ss.FFFFFFFK_.</span></span> <span data-ttu-id="f858b-157">Příkladem takových řetězec je "2008-04-12T12:53Z".</span><span class="sxs-lookup"><span data-stu-id="f858b-157">An example of such a string is “2008-04-12T12:53Z”.</span></span>
