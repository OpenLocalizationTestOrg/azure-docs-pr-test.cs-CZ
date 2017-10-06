---
title: "aaaOverview hello Azure Event Hubs standardní rozhraní API technologie .NET | Microsoft Docs"
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
ms.openlocfilehash: c97acecb35b69039e06ded7203c75fca41ce98f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-net-standard-api-overview"></a><span data-ttu-id="e3258-103">Přehled služby Event Hubs .NET standardní API</span><span class="sxs-lookup"><span data-stu-id="e3258-103">Event Hubs .NET Standard API overview</span></span>
<span data-ttu-id="e3258-104">Tento článek shrnuje některé hello klíče rozhraní API Standard .NET události rozbočovače klienta.</span><span class="sxs-lookup"><span data-stu-id="e3258-104">This article summarizes some of hello key Event Hubs .NET Standard client APIs.</span></span> <span data-ttu-id="e3258-105">Aktuálně existují dvě rozhraní .NET standardní knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="e3258-105">There are currently two .NET Standard client libraries:</span></span>
* [<span data-ttu-id="e3258-106">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="e3258-106">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
  *  <span data-ttu-id="e3258-107">Tato knihovna nabízí všechny operace základní runtime.</span><span class="sxs-lookup"><span data-stu-id="e3258-107">This library provides all basic runtime operations.</span></span>
* [<span data-ttu-id="e3258-108">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="e3258-108">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)
  * <span data-ttu-id="e3258-109">Tato knihovna přidává další funkce, která umožňuje pro udržování přehledu o zpracování události a hello nejjednodušší způsob, jak tooread z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="e3258-109">This library adds additional functionality that allows for keeping track of processed events, and is hello easiest way tooread from an event hub.</span></span>

## <a name="event-hubs-client"></a><span data-ttu-id="e3258-110">Události rozbočovače klienta</span><span class="sxs-lookup"><span data-stu-id="e3258-110">Event Hubs client</span></span>
<span data-ttu-id="e3258-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) je hello primární objekt použijete toosend události, vytvořte příjemce a informace o běhu tooget.</span><span class="sxs-lookup"><span data-stu-id="e3258-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is hello primary object you use toosend events, create receivers, and tooget run-time information.</span></span> <span data-ttu-id="e3258-112">Tento klient je propojené tooa určitá událost rozbočovače a vytvoří nový koncový bod připojení služby Event Hubs toohello.</span><span class="sxs-lookup"><span data-stu-id="e3258-112">This client is linked tooa particular event hub, and creates a new connection toohello Event Hubs endpoint.</span></span>

### <a name="create-an-event-hubs-client"></a><span data-ttu-id="e3258-113">Vytvoření klienta pro centra událostí (Event Hubs)</span><span class="sxs-lookup"><span data-stu-id="e3258-113">Create an Event Hubs client</span></span>
<span data-ttu-id="e3258-114">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) z připojovacího řetězce je vytvořen objekt.</span><span class="sxs-lookup"><span data-stu-id="e3258-114">An [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is created from a connection string.</span></span> <span data-ttu-id="e3258-115">Nejjednodušší způsob, jak tooinstantiate Hello nového klienta se zobrazí v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e3258-115">hello simplest way tooinstantiate a new client is shown in hello following example:</span></span>

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

<span data-ttu-id="e3258-116">tooprogrammatically upravit hello připojovací řetězec, můžete použít hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) třídy a předejte hello připojovací řetězec jako parametr příliš[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span><span class="sxs-lookup"><span data-stu-id="e3258-116">tooprogrammatically edit hello connection string, you can use hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) class, and pass hello connection string as a parameter too[EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span></span>

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a><span data-ttu-id="e3258-117">Odesílání událostí</span><span class="sxs-lookup"><span data-stu-id="e3258-117">Send events</span></span>
<span data-ttu-id="e3258-118">centra událostí toosend události tooan, použijte hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) třídy.</span><span class="sxs-lookup"><span data-stu-id="e3258-118">toosend events tooan event hub, use hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) class.</span></span> <span data-ttu-id="e3258-119">Hello obsah musí být `byte` pole, nebo `byte` segmentu pole.</span><span class="sxs-lookup"><span data-stu-id="e3258-119">hello body must be a `byte` array, or a `byte` array segment.</span></span>

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a><span data-ttu-id="e3258-120">Příjem událostí</span><span class="sxs-lookup"><span data-stu-id="e3258-120">Receive events</span></span>
<span data-ttu-id="e3258-121">Doporučená způsob tooreceive události ze služby Event Hubs Hello používá hello [Event Processor Host](#event-processor-host-apis), který poskytuje funkce tooautomatically sledovat posun a informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="e3258-121">hello recommended way tooreceive events from Event Hubs is using hello [Event Processor Host](#event-processor-host-apis), which provides functionality tooautomatically keep track of offset, and partition information.</span></span> <span data-ttu-id="e3258-122">Existují však určité situace, ve kterých může být vhodné toouse hello flexibilitu hello základní služby Event Hubs knihovny tooreceive události.</span><span class="sxs-lookup"><span data-stu-id="e3258-122">However, there are certain situations in which you may want toouse hello flexibility of hello core Event Hubs library tooreceive events.</span></span>

#### <a name="create-a-receiver"></a><span data-ttu-id="e3258-123">Vytvoření příjemce</span><span class="sxs-lookup"><span data-stu-id="e3258-123">Create a receiver</span></span>
<span data-ttu-id="e3258-124">Příjemci jsou vázané toospecific oddíly, tak v pořadí tooreceive všechny události v Centru událostí, budete potřebovat toocreate více instancí.</span><span class="sxs-lookup"><span data-stu-id="e3258-124">Receivers are tied toospecific partitions, so in order tooreceive all events in an event hub, you will need toocreate multiple instances.</span></span> <span data-ttu-id="e3258-125">Obecně je vhodné tooget hello oddílu informace o programově, místo pevně kódováno ID oddílů hello.</span><span class="sxs-lookup"><span data-stu-id="e3258-125">Generally speaking, it is a good practice tooget hello partition information programatically, rather than hard-coding hello partition ids.</span></span> <span data-ttu-id="e3258-126">V pořadí toodo tak, můžete použít hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metoda.</span><span class="sxs-lookup"><span data-stu-id="e3258-126">In order toodo so, you can use hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) method.</span></span>

```csharp
// Create a list tookeep track of hello receivers
var receivers = new List<PartitionReceiver>();
// Use hello eventHubClient created above tooget hello runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over hello resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create hello receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add hello receiver toohello list
    receivers.Add(receiver);
}
```

<span data-ttu-id="e3258-127">Vzhledem k tomu, že události jsou nikdy z centra událostí (a pouze vyprší platnost), je nutné toospecify hello správné počáteční bod.</span><span class="sxs-lookup"><span data-stu-id="e3258-127">Since events are never removed from an event hub (and only expire), you need toospecify hello proper starting point.</span></span> <span data-ttu-id="e3258-128">Hello následující příklad ukazuje možné kombinace.</span><span class="sxs-lookup"><span data-stu-id="e3258-128">hello following example shows possible combinations.</span></span>

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a><span data-ttu-id="e3258-129">Využívat událost</span><span class="sxs-lookup"><span data-stu-id="e3258-129">Consume an event</span></span>
```csharp
// Receive a maximum of 100 messages in this call tooReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop tooprocess
    foreach (var ehEvent in ehEvents)
    {
        // Decode hello byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load hello custom property that we set in hello send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a><span data-ttu-id="e3258-130">Rozhraní API hostitele procesor událostí</span><span class="sxs-lookup"><span data-stu-id="e3258-130">Event Processor Host APIs</span></span>
<span data-ttu-id="e3258-131">Tato rozhraní API nabízejí odolnosti tooworker procesy, které mohly být nedostupné, a to distribucí oddíly mezi dostupné pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="e3258-131">These APIs provide resiliency tooworker processes that may become unavailable, by distributing partitions across available workers.</span></span>

```csharp
// Checkpointing is done within hello SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

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

// Disposes of hello Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

<span data-ttu-id="e3258-132">Hello tady je příklad implementace hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span><span class="sxs-lookup"><span data-stu-id="e3258-132">hello following is a sample implementation of hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e3258-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3258-133">Next steps</span></span>
<span data-ttu-id="e3258-134">toolearn Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="e3258-134">toolearn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="e3258-135">Co je Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="e3258-135">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e3258-136">Rozhraní API k dispozici události rozbočovače</span><span class="sxs-lookup"><span data-stu-id="e3258-136">Available Event Hubs apis</span></span>](event-hubs-api-overview.md)

<span data-ttu-id="e3258-137">Hello .NET API odkazy jsou tady:</span><span class="sxs-lookup"><span data-stu-id="e3258-137">hello .NET API references are here:</span></span>

* [<span data-ttu-id="e3258-138">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="e3258-138">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
* [<span data-ttu-id="e3258-139">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="e3258-139">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)