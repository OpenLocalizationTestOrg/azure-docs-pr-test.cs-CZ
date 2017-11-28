---
title: "aaaSend události tooAzure časové řady Přehled prostředí | Microsoft Docs"
description: "Tento kurz se zaměřuje hello kroky toopush události tooyour časové řady Přehled prostředí"
keywords: 
services: tsi
documentationcenter: 
author: venkatgct
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: venkatja
ms.openlocfilehash: dbccc23f61351a0033cd48c1a02fb3841b45d560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooa-time-series-insights-environment-using-event-hub"></a><span data-ttu-id="712ec-103">Odeslání události tooa časové řady Statistika prostředí pomocí centra událostí</span><span class="sxs-lookup"><span data-stu-id="712ec-103">Send events tooa Time Series Insights environment using event hub</span></span>

<span data-ttu-id="712ec-104">Tento kurz vysvětluje, jak toocreate a konfiguraci centra událostí a spuštění ukázkové aplikace toopush události.</span><span class="sxs-lookup"><span data-stu-id="712ec-104">This tutorial explains how toocreate and configure event hub and run a sample application toopush events.</span></span> <span data-ttu-id="712ec-105">Pokud máte existující centrum událostí s událostmi ve formátu JSON, přeskočte tento kurz a zobrazte své prostředí v [Time Series Insights](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="712ec-105">If you have an existing event hub with events in JSON format, skip this tutorial and view your environment in [time series insights](https://insights.timeseries.azure.com).</span></span>

## <a name="configure-an-event-hub"></a><span data-ttu-id="712ec-106">Konfigurace centra událostí</span><span class="sxs-lookup"><span data-stu-id="712ec-106">Configure an event hub</span></span>
1. <span data-ttu-id="712ec-107">toocreate centra událostí, postupujte podle pokynů z hello centra událostí [dokumentaci](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span><span class="sxs-lookup"><span data-stu-id="712ec-107">toocreate an event hub, follow instructions from hello Event Hub [documentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span></span>

2. <span data-ttu-id="712ec-108">Ujistěte se, že vytváříte skupinu příjemců, kterou používá výhradně váš zdroj událostí Time Series Insights.</span><span class="sxs-lookup"><span data-stu-id="712ec-108">Make sure you create a consumer group that is used exclusively by your Time Series Insights event source.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="712ec-109">Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights).</span><span class="sxs-lookup"><span data-stu-id="712ec-109">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="712ec-110">Pokud je skupina uživatelů používají i jiné služby, přečtěte si, že operace je negativně ovlivňovat to pro toto prostředí a hello dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="712ec-110">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="712ec-111">Pokud používáte "$Default" jako hello skupiny příjemců, je by mohlo vést toopotential opakované použití jiných čtečky.</span><span class="sxs-lookup"><span data-stu-id="712ec-111">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

  ![Výběr skupiny příjemců centra událostí](media/send-events/consumer-group.png)

3. <span data-ttu-id="712ec-113">Vytvořte "MySendPolicy" na hello centra událostí, který je použité toosend události v ukázce csharp hello.</span><span class="sxs-lookup"><span data-stu-id="712ec-113">On hello event hub, create “MySendPolicy” that is used toosend events in hello csharp sample.</span></span>

  ![Vyberte Zásady sdíleného přístupu a klikněte na tlačítko Přidat.](media/send-events/shared-access-policy.png)  

  ![Přidání nové zásady sdíleného přístupu](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a><span data-ttu-id="712ec-116">Vytvoření zdroje událostí Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="712ec-116">Create Time Series Insights event source</span></span>
1. <span data-ttu-id="712ec-117">Pokud jste dosud nevytvořili zdroje událostí, postupujte podle [tyto pokyny](time-series-insights-add-event-source.md) toocreate zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="712ec-117">If you haven't created an event source, follow [these instructions](time-series-insights-add-event-source.md) toocreate an event source.</span></span>

2. <span data-ttu-id="712ec-118">Zadejte "deviceTimestamp" jako název vlastnosti časové razítko hello – tato vlastnost se používá jako hello skutečné časové razítko v ukázce csharp hello.</span><span class="sxs-lookup"><span data-stu-id="712ec-118">Specify “deviceTimestamp” as hello timestamp property name – this property is used as hello actual timestamp in hello csharp sample.</span></span> <span data-ttu-id="712ec-119">Název vlastnosti Hello časové razítko je malá a velká písmena a hodnoty musí mít formát hello __rrrr-MM-ddTHH. FFFFFFFK__ při odeslání jako JSON tooevent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="712ec-119">hello timestamp property name is case-sensitive and values must follow hello format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ when sent as JSON tooevent hub.</span></span> <span data-ttu-id="712ec-120">Pokud vlastnost hello hello události neexistuje, pak hello čas události rozbočovače zařazených do fronty se používá.</span><span class="sxs-lookup"><span data-stu-id="712ec-120">If hello property does not exist in hello event, then hello event hub enqueued time is used.</span></span>

  ![Vytvoření zdroje událostí](media/send-events/event-source-1.png)

## <a name="sample-code-toopush-events"></a><span data-ttu-id="712ec-122">Ukázka kódu toopush události</span><span class="sxs-lookup"><span data-stu-id="712ec-122">Sample code toopush events</span></span>
1. <span data-ttu-id="712ec-123">Přejděte zásady centra událostí toohello "MySendPolicy" a zkopírujte připojovací řetězec hello klíčem hello zásad.</span><span class="sxs-lookup"><span data-stu-id="712ec-123">Go toohello event hub policy “MySendPolicy” and copy hello connection string with hello policy key.</span></span>

  ![Zkopírování připojovacího řetězce zásady MySendPolicy](media/send-events/sample-code-connection-string.png)

2. <span data-ttu-id="712ec-125">Spusťte následující kód hello této události toosend 600 pro každou z hello tři zařízení.</span><span class="sxs-lookup"><span data-stu-id="712ec-125">Run hello following code that toosend 600 events per each of hello three devices.</span></span> <span data-ttu-id="712ec-126">Nahraďte `eventHubConnectionString` vaším připojovacím řetězcem.</span><span class="sxs-lookup"><span data-stu-id="712ec-126">Update `eventHubConnectionString` with your connection string.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON tooevent hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a><span data-ttu-id="712ec-127">Podporované tvary JSON</span><span class="sxs-lookup"><span data-stu-id="712ec-127">Supported JSON shapes</span></span>
### <a name="sample-1"></a><span data-ttu-id="712ec-128">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="712ec-128">Sample 1</span></span>

#### <a name="input"></a><span data-ttu-id="712ec-129">Vstup</span><span class="sxs-lookup"><span data-stu-id="712ec-129">Input</span></span>

<span data-ttu-id="712ec-130">Jednoduchý objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="712ec-130">A simple JSON object.</span></span>

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a><span data-ttu-id="712ec-131">Výstup – 1 událost</span><span class="sxs-lookup"><span data-stu-id="712ec-131">Output - 1 event</span></span>

|<span data-ttu-id="712ec-132">id</span><span class="sxs-lookup"><span data-stu-id="712ec-132">id</span></span>|<span data-ttu-id="712ec-133">časové razítko</span><span class="sxs-lookup"><span data-stu-id="712ec-133">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="712ec-134">device1</span><span class="sxs-lookup"><span data-stu-id="712ec-134">device1</span></span>|<span data-ttu-id="712ec-135">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-135">2016-01-08T01:08:00Z</span></span>|

### <a name="sample-2"></a><span data-ttu-id="712ec-136">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="712ec-136">Sample 2</span></span>

#### <a name="input"></a><span data-ttu-id="712ec-137">Vstup</span><span class="sxs-lookup"><span data-stu-id="712ec-137">Input</span></span>
<span data-ttu-id="712ec-138">Pole JSON se dvěma objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="712ec-138">A JSON array with two JSON objects.</span></span> <span data-ttu-id="712ec-139">Každý objekt JSON, bude převedený tooan událostí.</span><span class="sxs-lookup"><span data-stu-id="712ec-139">Each JSON object will be converted tooan event.</span></span>
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a><span data-ttu-id="712ec-140">Výstup – 2 události</span><span class="sxs-lookup"><span data-stu-id="712ec-140">Output - 2 Events</span></span>

|<span data-ttu-id="712ec-141">id</span><span class="sxs-lookup"><span data-stu-id="712ec-141">id</span></span>|<span data-ttu-id="712ec-142">časové razítko</span><span class="sxs-lookup"><span data-stu-id="712ec-142">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="712ec-143">device1</span><span class="sxs-lookup"><span data-stu-id="712ec-143">device1</span></span>|<span data-ttu-id="712ec-144">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-144">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="712ec-145">device2</span><span class="sxs-lookup"><span data-stu-id="712ec-145">device2</span></span>|<span data-ttu-id="712ec-146">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-146">2016-01-08T01:17:00Z</span></span>|
### <a name="sample-3"></a><span data-ttu-id="712ec-147">Ukázka 3</span><span class="sxs-lookup"><span data-stu-id="712ec-147">Sample 3</span></span>

#### <a name="input"></a><span data-ttu-id="712ec-148">Vstup</span><span class="sxs-lookup"><span data-stu-id="712ec-148">Input</span></span>

<span data-ttu-id="712ec-149">Objekt JSON s vnořeným polem JSON, které obsahuje dva objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="712ec-149">A JSON object with a nested JSON array containing two JSON objects.</span></span>
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a><span data-ttu-id="712ec-150">Výstup – 2 události</span><span class="sxs-lookup"><span data-stu-id="712ec-150">Output - 2 Events</span></span>
<span data-ttu-id="712ec-151">Všimněte si, že je vlastnost hello "umístění" zkopíruje přes tooeach hello události.</span><span class="sxs-lookup"><span data-stu-id="712ec-151">Note that hello property "location" is copied over tooeach of hello event.</span></span>

|<span data-ttu-id="712ec-152">location</span><span class="sxs-lookup"><span data-stu-id="712ec-152">location</span></span>|<span data-ttu-id="712ec-153">events.id</span><span class="sxs-lookup"><span data-stu-id="712ec-153">events.id</span></span>|<span data-ttu-id="712ec-154">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="712ec-154">events.timestamp</span></span>|
|--------|---------------|----------------------|
|<span data-ttu-id="712ec-155">WestUs</span><span class="sxs-lookup"><span data-stu-id="712ec-155">WestUs</span></span>|<span data-ttu-id="712ec-156">device1</span><span class="sxs-lookup"><span data-stu-id="712ec-156">device1</span></span>|<span data-ttu-id="712ec-157">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-157">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="712ec-158">WestUs</span><span class="sxs-lookup"><span data-stu-id="712ec-158">WestUs</span></span>|<span data-ttu-id="712ec-159">device2</span><span class="sxs-lookup"><span data-stu-id="712ec-159">device2</span></span>|<span data-ttu-id="712ec-160">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-160">2016-01-08T01:17:00Z</span></span>|

### <a name="sample-4"></a><span data-ttu-id="712ec-161">Ukázka 4</span><span class="sxs-lookup"><span data-stu-id="712ec-161">Sample 4</span></span>

#### <a name="input"></a><span data-ttu-id="712ec-162">Vstup</span><span class="sxs-lookup"><span data-stu-id="712ec-162">Input</span></span>

<span data-ttu-id="712ec-163">Objekt JSON s vnořeným polem JSON, které obsahuje dva objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="712ec-163">A JSON object with a nested JSON array containing two JSON objects.</span></span> <span data-ttu-id="712ec-164">Tento vstup ukazuje, že vlastnosti globálních hello může být zastoupena hello komplexní objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="712ec-164">This input demonstrates that hello global properties may be represented by hello complex JSON object.</span></span>

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a><span data-ttu-id="712ec-165">Výstup – 2 události</span><span class="sxs-lookup"><span data-stu-id="712ec-165">Output - 2 Events</span></span>

|<span data-ttu-id="712ec-166">location</span><span class="sxs-lookup"><span data-stu-id="712ec-166">location</span></span>|<span data-ttu-id="712ec-167">manufacturer.name</span><span class="sxs-lookup"><span data-stu-id="712ec-167">manufacturer.name</span></span>|<span data-ttu-id="712ec-168">manufacturer.location</span><span class="sxs-lookup"><span data-stu-id="712ec-168">manufacturer.location</span></span>|<span data-ttu-id="712ec-169">events.id</span><span class="sxs-lookup"><span data-stu-id="712ec-169">events.id</span></span>|<span data-ttu-id="712ec-170">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="712ec-170">events.timestamp</span></span>|<span data-ttu-id="712ec-171">events.data.type</span><span class="sxs-lookup"><span data-stu-id="712ec-171">events.data.type</span></span>|<span data-ttu-id="712ec-172">events.data.units</span><span class="sxs-lookup"><span data-stu-id="712ec-172">events.data.units</span></span>|<span data-ttu-id="712ec-173">events.data.value</span><span class="sxs-lookup"><span data-stu-id="712ec-173">events.data.value</span></span>|
|---|---|---|---|---|---|---|---|
|<span data-ttu-id="712ec-174">WestUs</span><span class="sxs-lookup"><span data-stu-id="712ec-174">WestUs</span></span>|<span data-ttu-id="712ec-175">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="712ec-175">manufacturer1</span></span>|<span data-ttu-id="712ec-176">EastUs</span><span class="sxs-lookup"><span data-stu-id="712ec-176">EastUs</span></span>|<span data-ttu-id="712ec-177">device1</span><span class="sxs-lookup"><span data-stu-id="712ec-177">device1</span></span>|<span data-ttu-id="712ec-178">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-178">2016-01-08T01:08:00Z</span></span>|<span data-ttu-id="712ec-179">pressure</span><span class="sxs-lookup"><span data-stu-id="712ec-179">pressure</span></span>|<span data-ttu-id="712ec-180">psi</span><span class="sxs-lookup"><span data-stu-id="712ec-180">psi</span></span>|<span data-ttu-id="712ec-181">108.09</span><span class="sxs-lookup"><span data-stu-id="712ec-181">108.09</span></span>|
|<span data-ttu-id="712ec-182">WestUs</span><span class="sxs-lookup"><span data-stu-id="712ec-182">WestUs</span></span>|<span data-ttu-id="712ec-183">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="712ec-183">manufacturer1</span></span>|<span data-ttu-id="712ec-184">EastUs</span><span class="sxs-lookup"><span data-stu-id="712ec-184">EastUs</span></span>|<span data-ttu-id="712ec-185">device2</span><span class="sxs-lookup"><span data-stu-id="712ec-185">device2</span></span>|<span data-ttu-id="712ec-186">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="712ec-186">2016-01-08T01:17:00Z</span></span>|<span data-ttu-id="712ec-187">vibration</span><span class="sxs-lookup"><span data-stu-id="712ec-187">vibration</span></span>|<span data-ttu-id="712ec-188">abs G</span><span class="sxs-lookup"><span data-stu-id="712ec-188">abs G</span></span>|<span data-ttu-id="712ec-189">217.09</span><span class="sxs-lookup"><span data-stu-id="712ec-189">217.09</span></span>|

## <a name="next-steps"></a><span data-ttu-id="712ec-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="712ec-190">Next steps</span></span>

* <span data-ttu-id="712ec-191">Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="712ec-191">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
