---
title: "Přehled služby Azure Event Hubs standardní rozhraní API technologie .NET | Microsoft Docs"
description: "Přehled standardní rozhraní API .NET"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: eea682c40cd415b383a8b2f0004a5f3648e2f01f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-net-standard-api-overview"></a><span data-ttu-id="c011f-103">Přehled služby Event Hubs .NET standardní API</span><span class="sxs-lookup"><span data-stu-id="c011f-103">Event Hubs .NET Standard API overview</span></span>
<span data-ttu-id="c011f-104">Tento článek shrnuje některé klíče rozhraní API Standard .NET události rozbočovače klienta.</span><span class="sxs-lookup"><span data-stu-id="c011f-104">This article summarizes some of the key Event Hubs .NET Standard client APIs.</span></span> <span data-ttu-id="c011f-105">Aktuálně existují dvě rozhraní .NET standardní knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="c011f-105">There are currently two .NET Standard client libraries:</span></span>
* [<span data-ttu-id="c011f-106">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="c011f-106">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
  *  <span data-ttu-id="c011f-107">Tato knihovna nabízí všechny operace základní runtime.</span><span class="sxs-lookup"><span data-stu-id="c011f-107">This library provides all basic runtime operations.</span></span>
* [<span data-ttu-id="c011f-108">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="c011f-108">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)
  * <span data-ttu-id="c011f-109">Tato knihovna přidává další funkce, která umožňuje pro udržování přehledu o zpracování událostí a je nejjednodušší způsob, jak číst z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c011f-109">This library adds additional functionality that allows for keeping track of processed events, and is the easiest way to read from an event hub.</span></span>

## <a name="event-hubs-client"></a><span data-ttu-id="c011f-110">Události rozbočovače klienta</span><span class="sxs-lookup"><span data-stu-id="c011f-110">Event Hubs client</span></span>
<span data-ttu-id="c011f-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) je primární objekt, který používáte pro odeslání události vytvořit příjemci a získat informace o běhu.</span><span class="sxs-lookup"><span data-stu-id="c011f-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is the primary object you use to send events, create receivers, and to get run-time information.</span></span> <span data-ttu-id="c011f-112">Tento klient je propojena k rozbočovači určitá událost a vytvoří nové připojení ke koncovému bodu služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="c011f-112">This client is linked to a particular event hub, and creates a new connection to the Event Hubs endpoint.</span></span>

### <a name="create-an-event-hubs-client"></a><span data-ttu-id="c011f-113">Vytvoření klienta pro centra událostí (Event Hubs)</span><span class="sxs-lookup"><span data-stu-id="c011f-113">Create an Event Hubs client</span></span>
<span data-ttu-id="c011f-114">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) z připojovacího řetězce je vytvořen objekt.</span><span class="sxs-lookup"><span data-stu-id="c011f-114">An [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is created from a connection string.</span></span> <span data-ttu-id="c011f-115">Nejjednodušší způsob, jak vytvořit nový klient je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c011f-115">The simplest way to instantiate a new client is shown in the following example:</span></span>

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

<span data-ttu-id="c011f-116">Chcete-li prostřednictvím kódu programu upravit připojovací řetězec, můžete použít [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) třídy a předejte připojovací řetězec jako parametr, který se [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span><span class="sxs-lookup"><span data-stu-id="c011f-116">To programmatically edit the connection string, you can use the [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) class, and pass the connection string as a parameter to [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span></span>

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a><span data-ttu-id="c011f-117">Odesílání událostí</span><span class="sxs-lookup"><span data-stu-id="c011f-117">Send events</span></span>
<span data-ttu-id="c011f-118">Chcete-li odesílání událostí do centra událostí, použijte [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) třídy.</span><span class="sxs-lookup"><span data-stu-id="c011f-118">To send events to an event hub, use the [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) class.</span></span> <span data-ttu-id="c011f-119">Obsah musí být `byte` pole, nebo `byte` segmentu pole.</span><span class="sxs-lookup"><span data-stu-id="c011f-119">The body must be a `byte` array, or a `byte` array segment.</span></span>

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a><span data-ttu-id="c011f-120">Příjem událostí</span><span class="sxs-lookup"><span data-stu-id="c011f-120">Receive events</span></span>
<span data-ttu-id="c011f-121">Doporučený způsob přijímání události ze služby Event Hubs je pomocí [Event Processor Host](#event-processor-host-apis), která poskytuje funkce můžete automaticky sledovat posun a informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="c011f-121">The recommended way to receive events from Event Hubs is using the [Event Processor Host](#event-processor-host-apis), which provides functionality to automatically keep track of offset, and partition information.</span></span> <span data-ttu-id="c011f-122">Existují však určité situace, ve kterých můžete chtít použít možnost základní knihovny služby Event Hubs přijímat události.</span><span class="sxs-lookup"><span data-stu-id="c011f-122">However, there are certain situations in which you may want to use the flexibility of the core Event Hubs library to receive events.</span></span>

#### <a name="create-a-receiver"></a><span data-ttu-id="c011f-123">Vytvoření příjemce</span><span class="sxs-lookup"><span data-stu-id="c011f-123">Create a receiver</span></span>
<span data-ttu-id="c011f-124">Příjemci, jsou svázané s konkrétní oddíly, tak aby bylo možné přijímat všechny události v Centru událostí, budete muset vytvořit více instancí.</span><span class="sxs-lookup"><span data-stu-id="c011f-124">Receivers are tied to specific partitions, so in order to receive all events in an event hub, you will need to create multiple instances.</span></span> <span data-ttu-id="c011f-125">Obecně je vhodné se získat informace o oddílu programově, místo pevně kódováno ID oddílů.</span><span class="sxs-lookup"><span data-stu-id="c011f-125">Generally speaking, it is a good practice to get the partition information programatically, rather than hard-coding the partition ids.</span></span> <span data-ttu-id="c011f-126">Chcete-li to provést, můžete použít [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metoda.</span><span class="sxs-lookup"><span data-stu-id="c011f-126">In order to do so, you can use the [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) method.</span></span>

```csharp
// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

<span data-ttu-id="c011f-127">Vzhledem k tomu, že události jsou nikdy z centra událostí (a pouze vyprší platnost), budete muset zadat správné počáteční bod.</span><span class="sxs-lookup"><span data-stu-id="c011f-127">Since events are never removed from an event hub (and only expire), you need to specify the proper starting point.</span></span> <span data-ttu-id="c011f-128">Následující příklad ukazuje možné kombinace.</span><span class="sxs-lookup"><span data-stu-id="c011f-128">The following example shows possible combinations.</span></span>

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a><span data-ttu-id="c011f-129">Využívat událost</span><span class="sxs-lookup"><span data-stu-id="c011f-129">Consume an event</span></span>
```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a><span data-ttu-id="c011f-130">Rozhraní API hostitele procesor událostí</span><span class="sxs-lookup"><span data-stu-id="c011f-130">Event Processor Host APIs</span></span>
<span data-ttu-id="c011f-131">Tato rozhraní API poskytují odolnost pro pracovní procesy, které mohly být nedostupné, a to distribucí oddíly mezi dostupné pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="c011f-131">These APIs provide resiliency to worker processes that may become unavailable, by distributing partitions across available workers.</span></span>

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes of the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

<span data-ttu-id="c011f-132">Tady je příklad implementace [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span><span class="sxs-lookup"><span data-stu-id="c011f-132">The following is a sample implementation of the [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span></span>

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
        return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
        Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
        return Task.CompletedTask;
    }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
        Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
        return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (var eventData in messages)
        {
            var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
            Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
        }

        return context.CheckpointAsync();
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="c011f-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c011f-133">Next steps</span></span>
<span data-ttu-id="c011f-134">Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="c011f-134">To learn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="c011f-135">Co je Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="c011f-135">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="c011f-136">Rozhraní API k dispozici události rozbočovače</span><span class="sxs-lookup"><span data-stu-id="c011f-136">Available Event Hubs apis</span></span>](event-hubs-api-overview.md)

<span data-ttu-id="c011f-137">Odkazy na rozhraní API .NET jsou tady:</span><span class="sxs-lookup"><span data-stu-id="c011f-137">The .NET API references are here:</span></span>

* [<span data-ttu-id="c011f-138">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="c011f-138">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
* [<span data-ttu-id="c011f-139">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="c011f-139">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)