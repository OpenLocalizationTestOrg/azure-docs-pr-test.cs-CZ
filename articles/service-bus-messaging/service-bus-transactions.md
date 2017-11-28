---
title: "aaaOverview zpracování transakcí v Azure Service Bus | Microsoft Docs"
description: "Přehled Azure Service Bus jednotlivé transakce a odesílání prostřednictvím"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: 5ed4d1fd3a089b8ebcd69a568f4ac863e753aecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a><span data-ttu-id="4e90f-103">Přehled služby Service Bus zpracování transakcí</span><span class="sxs-lookup"><span data-stu-id="4e90f-103">Overview of Service Bus transaction processing</span></span>
<span data-ttu-id="4e90f-104">Tento článek popisuje možnosti transakce hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="4e90f-104">This article discusses hello transaction capabilities of Azure Service Bus.</span></span> <span data-ttu-id="4e90f-105">Velká část hello diskusní je zobrazená ve hello [jednotlivé transakce s ukázkou Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span><span class="sxs-lookup"><span data-stu-id="4e90f-105">Much of hello discussion is illustrated by hello [Atomic Transactions with Service Bus sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span></span> <span data-ttu-id="4e90f-106">Tento článek je omezená tooan přehled zpracování transakcí a hello *odesílání prostřednictvím* funkce v Service Bus, zatímco ukázka jednotlivé transakce hello je širší a složitější v oboru.</span><span class="sxs-lookup"><span data-stu-id="4e90f-106">This article is limited tooan overview of transaction processing and hello *send via* feature in Service Bus, while hello Atomic Transactions sample is broader and more complex in scope.</span></span>

## <a name="transactions-in-service-bus"></a><span data-ttu-id="4e90f-107">Transakce v Service Bus</span><span class="sxs-lookup"><span data-stu-id="4e90f-107">Transactions in Service Bus</span></span>
<span data-ttu-id="4e90f-108">A [transakce](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) seskupí dva nebo více operací společně do *spuštění rozsah*.</span><span class="sxs-lookup"><span data-stu-id="4e90f-108">A [transaction](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) groups two or more operations together into an *execution scope*.</span></span> <span data-ttu-id="4e90f-109">Vzhledem k povaze tuto transakci Ujistěte se, že všechny operace, které patří tooa danou skupinu operací úspěšné, nebo selže společně.</span><span class="sxs-lookup"><span data-stu-id="4e90f-109">By nature, such a transaction must ensure that all operations belonging tooa given group of operations either succeed or fail jointly.</span></span> <span data-ttu-id="4e90f-110">V tomto ohledu transakce fungovat jako jednu jednotku, což je často označují tooas *nedělitelnost*.</span><span class="sxs-lookup"><span data-stu-id="4e90f-110">In this respect transactions act as one unit, which is often referred tooas *atomicity*.</span></span> 

<span data-ttu-id="4e90f-111">Service Bus je zprostředkovatele transakčních zpráv a zajistíte integritu transakcí pro všechny interní operace před jeho úložiště zpráv.</span><span class="sxs-lookup"><span data-stu-id="4e90f-111">Service Bus is a transactional message broker and ensures transactional integrity for all internal operations against its message stores.</span></span> <span data-ttu-id="4e90f-112">Všechny přenosy zpráv v rámci služby Service Bus, jako je například přesunutí zprávy tooa [frontu nedoručených zpráv](service-bus-dead-letter-queues.md) nebo [automatické přesměrování](service-bus-auto-forwarding.md) zpráv mezi entitami, jsou transakcí.</span><span class="sxs-lookup"><span data-stu-id="4e90f-112">All transfers of messages inside of Service Bus, such as moving messages tooa [dead-letter queue](service-bus-dead-letter-queues.md) or [automatic forwarding](service-bus-auto-forwarding.md) of messages between entities, are transactional.</span></span> <span data-ttu-id="4e90f-113">Jako takový Pokud Service Bus přijme zprávu, již bylo uložené a s názvem bez přípony s pořadovým číslem.</span><span class="sxs-lookup"><span data-stu-id="4e90f-113">As such, if Service Bus accepts a message, it has already been stored and labeled with a sequence number.</span></span> <span data-ttu-id="4e90f-114">Od toho všechny přenosy zprávy v rámci služby Service Bus jsou koordinované operace napříč entity a povede ani tooloss (zdroj úspěšné a cíle selže) nebo tooduplication (selže zdroj a cíl úspěšné) hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="4e90f-114">From then on, any message transfers within Service Bus are coordinated operations across entities, and will neither lead tooloss (source succeeds and target fails) or tooduplication (source fails and target succeeds) of hello message.</span></span>

<span data-ttu-id="4e90f-115">Service Bus podporuje seskupení operace u jedné entity přenosu zpráv (fronty, tématu, odběru) v rámci oboru hello transakce.</span><span class="sxs-lookup"><span data-stu-id="4e90f-115">Service Bus supports grouping operations against a single messaging entity (queue, topic, subscription) within hello scope of a transaction.</span></span> <span data-ttu-id="4e90f-116">Například můžete odeslat několik tooone fronty zpráv z v oboru transakce a hello pouze budou zprávy fronty potvrdit toohello protokolu při úspěšném dokončení transakce hello.</span><span class="sxs-lookup"><span data-stu-id="4e90f-116">For example, you can send several messages tooone queue from within a transaction scope, and hello messages will only be committed toohello queue's log when hello transaction successfully completes.</span></span>

## <a name="operations-within-a-transaction-scope"></a><span data-ttu-id="4e90f-117">Operace v rámci oboru transakce</span><span class="sxs-lookup"><span data-stu-id="4e90f-117">Operations within a transaction scope</span></span>
<span data-ttu-id="4e90f-118">Hello operace, které lze provést v oboru transakce jsou následující:</span><span class="sxs-lookup"><span data-stu-id="4e90f-118">hello operations that can be performed within a transaction scope are as follows:</span></span>

* <span data-ttu-id="4e90f-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: odeslat, SendAsync, SendBatch, SendBatchAsync</span><span class="sxs-lookup"><span data-stu-id="4e90f-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync</span></span> 
* <span data-ttu-id="4e90f-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: dokončení CompleteAsync, Abandon, AbandonAsync, nedoručených zpráv, DeadletterAsync, odložení, DeferAsync, RenewLock, RenewLockAsync</span><span class="sxs-lookup"><span data-stu-id="4e90f-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync</span></span> 

<span data-ttu-id="4e90f-121">Zobrazí operace nejsou zahrnuté, protože se předpokládá, že aplikace hello získá zprávy pomocí hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu uvnitř některé přijímat smyčky nebo s [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) zpětného volání, a obor pouze poté otevře a transakce pro zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="4e90f-121">Receive operations are not included, because it is assumed that hello application acquires messages using hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, inside some receive loop or with an [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback, and only then opens a transaction scope for processing hello message.</span></span>

<span data-ttu-id="4e90f-122">Hello dispozice uvítací zprávu (dokončení odložení abandon nedoručených zpráv) potom se provede v rámci oboru hello a závisí na, hello celkový výsledek hello transakce.</span><span class="sxs-lookup"><span data-stu-id="4e90f-122">hello disposition of hello message (complete, abandon, dead-letter, defer) then occurs within hello scope of, and dependent on, hello overall outcome of hello transaction.</span></span>

## <a name="transfers-and-send-via"></a><span data-ttu-id="4e90f-123">Přenosy a "odesílání prostřednictvím"</span><span class="sxs-lookup"><span data-stu-id="4e90f-123">Transfers and "send via"</span></span>
<span data-ttu-id="4e90f-124">tooenable transakční předání dat z fronty tooa procesoru a potom tooanother fronty Service Bus podporuje *přenosy*.</span><span class="sxs-lookup"><span data-stu-id="4e90f-124">tooenable transactional handover of data from a queue tooa processor, and then tooanother queue, Service Bus supports *transfers*.</span></span> <span data-ttu-id="4e90f-125">V rámci operace přenosu odesílatel nejprve odešle zprávu tooa "fronty přenosu" a fronty přenosu hello ihned přesouvá toohello hello zprávy určené cílové fronty pomocí stejné robustní přenosu implementace, která využívá funkce automatického předání hello hello na.</span><span class="sxs-lookup"><span data-stu-id="4e90f-125">In a transfer operation, a sender first sends a message tooa "transfer queue" and hello transfer queue immediately moves hello message toohello intended destination queue using hello same robust transfer implementation that hello auto-forward capability relies on.</span></span> <span data-ttu-id="4e90f-126">uvítací zprávu se nikdy fronty přenosu potvrdit toohello protokolu tak, že se zobrazí u spotřebitelé fronty přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="4e90f-126">hello message is never committed toohello transfer queue's log in a way that it becomes visible for hello transfer queue's consumers.</span></span>

<span data-ttu-id="4e90f-127">napájení Hello tuto funkci transakční vyvstává zřejmá při fronty přenosu hello, samotné hello zdroj vstupní zpráv hello odesílatele.</span><span class="sxs-lookup"><span data-stu-id="4e90f-127">hello power of this transactional capability becomes apparent when hello transfer queue itself is hello source of hello sender's input messages.</span></span> <span data-ttu-id="4e90f-128">Jinými slovy, Service Bus se můžou přenášet hello zpráva toohello cílové fronty "přes" hello fronty přenosu, při provádění úplná (nebo odložení, nebo nedoručených zpráv) operace na uvítací vstupní zprávu, všechny v rámci jedné atomické operace.</span><span class="sxs-lookup"><span data-stu-id="4e90f-128">In other words, Service Bus can transfer hello message toohello destination queue "via" hello transfer queue, while performing a complete (or defer, or dead-letter) operation on hello input message, all in one atomic operation.</span></span> 

### <a name="see-it-in-code"></a><span data-ttu-id="4e90f-129">Zobrazení v kódu</span><span class="sxs-lookup"><span data-stu-id="4e90f-129">See it in code</span></span>
<span data-ttu-id="4e90f-130">tooset se tyto přenosy, můžete vytvořit odesílatele zprávy, která je cílena hello cílové fronty prostřednictvím fronty přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="4e90f-130">tooset up such transfers, you create a message sender that targets hello destination queue via hello transfer queue.</span></span> <span data-ttu-id="4e90f-131">Také budete mít příjemce, který žádá o zprávy z této fronty stejné.</span><span class="sxs-lookup"><span data-stu-id="4e90f-131">You will also have a receiver that pulls messages from that same queue.</span></span> <span data-ttu-id="4e90f-132">Například:</span><span class="sxs-lookup"><span data-stu-id="4e90f-132">For example:</span></span>

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

<span data-ttu-id="4e90f-133">Jednoduché transakce pak používá tyto prvky, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4e90f-133">A simple transaction then uses these elements, as in hello following example:</span></span>

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package hello result 

    msg.Complete(); // mark hello message as done
    sender.Send(newmsg); // forward hello result

    scope.Complete(); // declare hello transaction done
} 
```

## <a name="next-steps"></a><span data-ttu-id="4e90f-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e90f-134">Next steps</span></span>

<span data-ttu-id="4e90f-135">Viz následující články pro další informace o fronty Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="4e90f-135">See hello following articles for more information about Service Bus queues:</span></span>

* [<span data-ttu-id="4e90f-136">Řetězení entit služby Service Bus s automatické přesměrování</span><span class="sxs-lookup"><span data-stu-id="4e90f-136">Chaining Service Bus entities with auto-forwarding</span></span>](service-bus-auto-forwarding.md)
* [<span data-ttu-id="4e90f-137">Ukázka automatické vpřed</span><span class="sxs-lookup"><span data-stu-id="4e90f-137">Auto-forward sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [<span data-ttu-id="4e90f-138">Jednotlivé transakce s ukázkou Service Bus</span><span class="sxs-lookup"><span data-stu-id="4e90f-138">Atomic Transactions with Service Bus sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [<span data-ttu-id="4e90f-139">Azure Service Bus a fronty fronty porovnání</span><span class="sxs-lookup"><span data-stu-id="4e90f-139">Azure Queues and Service Bus queues compared</span></span>](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [<span data-ttu-id="4e90f-140">Jak toouse fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="4e90f-140">How toouse Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

