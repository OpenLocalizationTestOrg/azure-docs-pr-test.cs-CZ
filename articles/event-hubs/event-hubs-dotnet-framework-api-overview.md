---
title: "aaaOverview hello rozhraní API služby Azure Event Hubs rozhraní .NET Framework | Microsoft Docs"
description: "Souhrn některých hello klíče Event Hubs rozhraní .NET Framework klientských rozhraní API."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f3b6cc0-9600-417f-9e80-2345411bd036
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b0e12e43f91b025d7aa4ca03e664b9ff31b04097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-net-framework-api-overview"></a><span data-ttu-id="3d166-103">Přehled služby Event Hubs rozhraní .NET Framework API</span><span class="sxs-lookup"><span data-stu-id="3d166-103">Event Hubs .NET Framework API overview</span></span>
<span data-ttu-id="3d166-104">Tento článek shrnuje některé hello klíče rozhraní API klienta rozhraní .NET Framework centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3d166-104">This article summarizes some of hello key Event Hubs .NET Framework client APIs.</span></span> <span data-ttu-id="3d166-105">Existují dvě kategorie: Správa a rozhraní API pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="3d166-105">There are two categories: management and run-time APIs.</span></span> <span data-ttu-id="3d166-106">Spuštění rozhraní API sestávají z všech potřebných operací toosend a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="3d166-106">Run-time APIs consist of all operations needed toosend and receive a message.</span></span> <span data-ttu-id="3d166-107">Operace správy povolit toomanage stavu entity Event Hubs vytváření, aktualizaci a odstraňování entit.</span><span class="sxs-lookup"><span data-stu-id="3d166-107">Management operations enable you toomanage an Event Hubs entity state by creating, updating, and deleting entities.</span></span>

<span data-ttu-id="3d166-108">Scénáře monitorování span, správu a spuštění.</span><span class="sxs-lookup"><span data-stu-id="3d166-108">Monitoring scenarios span both management and run-time.</span></span> <span data-ttu-id="3d166-109">Podrobné referenční dokumentaci na hello rozhraní API technologie .NET najdete v tématu hello [Service Bus .NET](/dotnet/api/microsoft.servicebus.messaging) a [rozhraní API třídy EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor) odkazy.</span><span class="sxs-lookup"><span data-stu-id="3d166-109">For detailed reference documentation on hello .NET APIs, see hello [Service Bus .NET](/dotnet/api/microsoft.servicebus.messaging) and [EventProcessorHost API](/dotnet/api/microsoft.azure.eventhubs.processor) references.</span></span>

## <a name="management-apis"></a><span data-ttu-id="3d166-110">Rozhraní API pro správu</span><span class="sxs-lookup"><span data-stu-id="3d166-110">Management APIs</span></span>
<span data-ttu-id="3d166-111">tooperform hello následující operace správy, musíte mít **spravovat** oprávnění v oboru názvů služby Event Hubs hello:</span><span class="sxs-lookup"><span data-stu-id="3d166-111">tooperform hello following management operations, you must have **Manage** permissions on hello Event Hubs namespace:</span></span>

### <a name="create"></a><span data-ttu-id="3d166-112">Vytvořit</span><span class="sxs-lookup"><span data-stu-id="3d166-112">Create</span></span>
```csharp
// Create hello event hub
var ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
await namespaceManager.CreateEventHubAsync(ehd);
```

### <a name="update"></a><span data-ttu-id="3d166-113">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="3d166-113">Update</span></span>
```csharp
var ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
var ruleName = "myeventhubmanagerule";
var ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
await namespaceManager.UpdateEventHubAsync(ehd);
```

### <a name="delete"></a><span data-ttu-id="3d166-114">Odstranění</span><span class="sxs-lookup"><span data-stu-id="3d166-114">Delete</span></span>
```csharp
await namespaceManager.DeleteEventHubAsync("Event Hub name");
```

## <a name="run-time-apis"></a><span data-ttu-id="3d166-115">Run-time rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3d166-115">Run-time APIs</span></span>
### <a name="create-publisher"></a><span data-ttu-id="3d166-116">Vytvořit vydavatele</span><span class="sxs-lookup"><span data-stu-id="3d166-116">Create publisher</span></span>
```csharp
// EventHubClient model (uses implicit factory instance, so all links on same connection)
var eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a><span data-ttu-id="3d166-117">Publikování zpráv</span><span class="sxs-lookup"><span data-stu-id="3d166-117">Publish message</span></span>
```csharp
// Create hello device/temperature metric
var info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
var data = new EventData(new byte[10]); // Byte array
var data = new EventData(Stream); // Stream 
var data = new EventData(info, serializer) //Object and serializer 
{
    PartitionKey = info.DeviceId.ToString()
};

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a><span data-ttu-id="3d166-118">Vytvoření příjemce</span><span class="sxs-lookup"><span data-stu-id="3d166-118">Create consumer</span></span>
```csharp
// Create hello Event Hubs client
var eventHubClient = EventHubClient.Create(EventHubName);

// Get hello default consumer group
var defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index);

// From one day ago
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));

// From specific offset, -1 means oldest
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index,startingOffset:-1); 
```

### <a name="consume-message"></a><span data-ttu-id="3d166-119">Zpracovat zprávu</span><span class="sxs-lookup"><span data-stu-id="3d166-119">Consume message</span></span>
```csharp
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)

// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a><span data-ttu-id="3d166-120">Rozhraní API hostitele procesor událostí</span><span class="sxs-lookup"><span data-stu-id="3d166-120">Event Processor Host APIs</span></span>
<span data-ttu-id="3d166-121">Tato rozhraní API nabízejí odolnosti tooworker procesy, které mohly být nedostupné, a to distribucí oddíly mezi dostupné pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="3d166-121">These APIs provide resiliency tooworker processes that may become unavailable, by distributing partitions across available workers.</span></span>

```csharp
// Checkpointing is done within hello SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use hello EventData.Offset value for checkpointing yourself, this value is unique per partition.

var eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
var blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

var eventHubDescription = new EventHubDescription(EventHubName);
var host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
await host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// tooclose
await host.UnregisterEventProcessorAsync();
```

<span data-ttu-id="3d166-122">Hello [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) rozhraní je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3d166-122">hello [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) interface is defined as follows:</span></span>

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            // Process messages here
        }

        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }

    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="3d166-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d166-123">Next steps</span></span>
<span data-ttu-id="3d166-124">toolearn Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="3d166-124">toolearn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="3d166-125">Co je Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="3d166-125">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="3d166-126">Průvodce programováním pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3d166-126">Event Hubs programming guide</span></span>](event-hubs-programming-guide.md)

<span data-ttu-id="3d166-127">Hello .NET API odkazy jsou tady:</span><span class="sxs-lookup"><span data-stu-id="3d166-127">hello .NET API references are here:</span></span>

* [<span data-ttu-id="3d166-128">Microsoft.ServiceBus.Messaging</span><span class="sxs-lookup"><span data-stu-id="3d166-128">Microsoft.ServiceBus.Messaging</span></span>](/dotnet/api/microsoft.servicebus.messaging)
* [<span data-ttu-id="3d166-129">Microsoft.Azure.EventHubs.EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="3d166-129">Microsoft.Azure.EventHubs.EventProcessorHost</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)
