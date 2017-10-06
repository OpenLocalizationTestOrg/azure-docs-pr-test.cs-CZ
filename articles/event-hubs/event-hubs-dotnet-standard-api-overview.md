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
# <a name="event-hubs-net-standard-api-overview"></a>Přehled služby Event Hubs .NET standardní API
Tento článek shrnuje některé hello klíče rozhraní API Standard .NET události rozbočovače klienta. Aktuálně existují dvě rozhraní .NET standardní knihovny klienta:
* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
  *  Tato knihovna nabízí všechny operace základní runtime.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
  * Tato knihovna přidává další funkce, která umožňuje pro udržování přehledu o zpracování události a hello nejjednodušší způsob, jak tooread z centra událostí.

## <a name="event-hubs-client"></a>Události rozbočovače klienta
[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) je hello primární objekt použijete toosend události, vytvořte příjemce a informace o běhu tooget. Tento klient je propojené tooa určitá událost rozbočovače a vytvoří nový koncový bod připojení služby Event Hubs toohello.

### <a name="create-an-event-hubs-client"></a>Vytvoření klienta pro centra událostí (Event Hubs)
[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) z připojovacího řetězce je vytvořen objekt. Nejjednodušší způsob, jak tooinstantiate Hello nového klienta se zobrazí v hello následující ukázka:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

tooprogrammatically upravit hello připojovací řetězec, můžete použít hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) třídy a předejte hello připojovací řetězec jako parametr příliš[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Odesílání událostí
centra událostí toosend události tooan, použijte hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) třídy. Hello obsah musí být `byte` pole, nebo `byte` segmentu pole.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Příjem událostí
Doporučená způsob tooreceive události ze služby Event Hubs Hello používá hello [Event Processor Host](#event-processor-host-apis), který poskytuje funkce tooautomatically sledovat posun a informace o oddílu. Existují však určité situace, ve kterých může být vhodné toouse hello flexibilitu hello základní služby Event Hubs knihovny tooreceive události.

#### <a name="create-a-receiver"></a>Vytvoření příjemce
Příjemci jsou vázané toospecific oddíly, tak v pořadí tooreceive všechny události v Centru událostí, budete potřebovat toocreate více instancí. Obecně je vhodné tooget hello oddílu informace o programově, místo pevně kódováno ID oddílů hello. V pořadí toodo tak, můžete použít hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metoda.

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

Vzhledem k tomu, že události jsou nikdy z centra událostí (a pouze vyprší platnost), je nutné toospecify hello správné počáteční bod. Hello následující příklad ukazuje možné kombinace.

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Využívat událost
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

## <a name="event-processor-host-apis"></a>Rozhraní API hostitele procesor událostí
Tato rozhraní API nabízejí odolnosti tooworker procesy, které mohly být nedostupné, a to distribucí oddíly mezi dostupné pracovních procesů.

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

Hello tady je příklad implementace hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

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

## <a name="next-steps"></a>Další kroky
toolearn Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:

* [Co je Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Rozhraní API k dispozici události rozbočovače](event-hubs-api-overview.md)

Hello .NET API odkazy jsou tady:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)