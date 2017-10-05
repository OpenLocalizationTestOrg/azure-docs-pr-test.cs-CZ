---
title: "Přehled zpracování transakcí v Azure Service Bus | Microsoft Docs"
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
ms.openlocfilehash: a88f2d81ab43e38c9363a67aaefc178b47bfb259
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a><span data-ttu-id="20c97-103">Přehled služby Service Bus zpracování transakcí</span><span class="sxs-lookup"><span data-stu-id="20c97-103">Overview of Service Bus transaction processing</span></span>
<span data-ttu-id="20c97-104">Tento článek popisuje možnosti transakce Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="20c97-104">This article discusses the transaction capabilities of Azure Service Bus.</span></span> <span data-ttu-id="20c97-105">Hodně diskuse je zobrazená ve [jednotlivé transakce s ukázkou Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span><span class="sxs-lookup"><span data-stu-id="20c97-105">Much of the discussion is illustrated by the [Atomic Transactions with Service Bus sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span></span> <span data-ttu-id="20c97-106">Tento článek je omezený na přehled zpracování transakcí a *odesílání prostřednictvím* funkce v Service Bus, zatímco ukázka jednotlivé transakce je širší a složitější v oboru.</span><span class="sxs-lookup"><span data-stu-id="20c97-106">This article is limited to an overview of transaction processing and the *send via* feature in Service Bus, while the Atomic Transactions sample is broader and more complex in scope.</span></span>

## <a name="transactions-in-service-bus"></a><span data-ttu-id="20c97-107">Transakce v Service Bus</span><span class="sxs-lookup"><span data-stu-id="20c97-107">Transactions in Service Bus</span></span>
<span data-ttu-id="20c97-108">A [transakce](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) seskupí dva nebo více operací společně do *spuštění rozsah*.</span><span class="sxs-lookup"><span data-stu-id="20c97-108">A [transaction](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) groups two or more operations together into an *execution scope*.</span></span> <span data-ttu-id="20c97-109">Vzhledem k povaze tuto transakci Ujistěte se, že všechny operace, které patří do dané skupiny operací úspěšné, nebo selže společně.</span><span class="sxs-lookup"><span data-stu-id="20c97-109">By nature, such a transaction must ensure that all operations belonging to a given group of operations either succeed or fail jointly.</span></span> <span data-ttu-id="20c97-110">V tomto ohledu transakce fungovat jako jednu jednotku, která se často označuje jako *nedělitelnost*.</span><span class="sxs-lookup"><span data-stu-id="20c97-110">In this respect transactions act as one unit, which is often referred to as *atomicity*.</span></span> 

<span data-ttu-id="20c97-111">Service Bus je zprostředkovatele transakčních zpráv a zajistíte integritu transakcí pro všechny interní operace před jeho úložiště zpráv.</span><span class="sxs-lookup"><span data-stu-id="20c97-111">Service Bus is a transactional message broker and ensures transactional integrity for all internal operations against its message stores.</span></span> <span data-ttu-id="20c97-112">Všechny přenosy zpráv v rámci služby Service Bus, například přesun zpráv [frontu nedoručených zpráv](service-bus-dead-letter-queues.md) nebo [automatické přesměrování](service-bus-auto-forwarding.md) zpráv mezi entitami, jsou transakcí.</span><span class="sxs-lookup"><span data-stu-id="20c97-112">All transfers of messages inside of Service Bus, such as moving messages to a [dead-letter queue](service-bus-dead-letter-queues.md) or [automatic forwarding](service-bus-auto-forwarding.md) of messages between entities, are transactional.</span></span> <span data-ttu-id="20c97-113">Jako takový Pokud Service Bus přijme zprávu, již bylo uložené a s názvem bez přípony s pořadovým číslem.</span><span class="sxs-lookup"><span data-stu-id="20c97-113">As such, if Service Bus accepts a message, it has already been stored and labeled with a sequence number.</span></span> <span data-ttu-id="20c97-114">Od toho všechny přenosy zprávy v rámci služby Service Bus jsou koordinované operace napříč entity a ani jeden z nich povede ke ztrátě (úspěšné zdroj a cíl selže) nebo duplikace (selže zdroj a cíl úspěšné) zprávy.</span><span class="sxs-lookup"><span data-stu-id="20c97-114">From then on, any message transfers within Service Bus are coordinated operations across entities, and will neither lead to loss (source succeeds and target fails) or to duplication (source fails and target succeeds) of the message.</span></span>

<span data-ttu-id="20c97-115">Service Bus podporuje seskupení operace u jedné entity přenosu zpráv (fronty, tématu, odběru) v rámci oboru transakce.</span><span class="sxs-lookup"><span data-stu-id="20c97-115">Service Bus supports grouping operations against a single messaging entity (queue, topic, subscription) within the scope of a transaction.</span></span> <span data-ttu-id="20c97-116">Například mohou zasílat několik zprávy do jedné frontu z v oboru transakce a zprávy pouze budou potvrzeny do fronty protokolu při úspěšném dokončení transakce.</span><span class="sxs-lookup"><span data-stu-id="20c97-116">For example, you can send several messages to one queue from within a transaction scope, and the messages will only be committed to the queue's log when the transaction successfully completes.</span></span>

## <a name="operations-within-a-transaction-scope"></a><span data-ttu-id="20c97-117">Operace v rámci oboru transakce</span><span class="sxs-lookup"><span data-stu-id="20c97-117">Operations within a transaction scope</span></span>
<span data-ttu-id="20c97-118">Operace, které lze provést v oboru transakce jsou následující:</span><span class="sxs-lookup"><span data-stu-id="20c97-118">The operations that can be performed within a transaction scope are as follows:</span></span>

* <span data-ttu-id="20c97-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: odeslat, SendAsync, SendBatch, SendBatchAsync</span><span class="sxs-lookup"><span data-stu-id="20c97-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync</span></span> 
* <span data-ttu-id="20c97-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: dokončení CompleteAsync, Abandon, AbandonAsync, nedoručených zpráv, DeadletterAsync, odložení, DeferAsync, RenewLock, RenewLockAsync</span><span class="sxs-lookup"><span data-stu-id="20c97-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync</span></span> 

<span data-ttu-id="20c97-121">Zobrazí operace nejsou zahrnuty, protože se předpokládá, že aplikace získá zprávy pomocí [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu uvnitř některé přijímat smyčky nebo s [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) zpětného volání, a Otevře se pak oboru transakce pro zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="20c97-121">Receive operations are not included, because it is assumed that the application acquires messages using the [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, inside some receive loop or with an [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback, and only then opens a transaction scope for processing the message.</span></span>

<span data-ttu-id="20c97-122">Poté dojde k dispozice zprávy (dokončení odložení abandon nedoručených zpráv) v rámci oboru a v závislosti na celkový výsledek transakce.</span><span class="sxs-lookup"><span data-stu-id="20c97-122">The disposition of the message (complete, abandon, dead-letter, defer) then occurs within the scope of, and dependent on, the overall outcome of the transaction.</span></span>

## <a name="transfers-and-send-via"></a><span data-ttu-id="20c97-123">Přenosy a "odesílání prostřednictvím"</span><span class="sxs-lookup"><span data-stu-id="20c97-123">Transfers and "send via"</span></span>
<span data-ttu-id="20c97-124">Pokud chcete povolit transakční předání dat z fronty pro procesor a pak do jiné fronty, Service Bus podporuje *přenosy*.</span><span class="sxs-lookup"><span data-stu-id="20c97-124">To enable transactional handover of data from a queue to a processor, and then to another queue, Service Bus supports *transfers*.</span></span> <span data-ttu-id="20c97-125">V rámci operace přenosu odesílatele nejprve odešle zprávu do fronty přenosu"" a fronty přenosu okamžitě přesune do určené cílové fronty pomocí stejné robustní přenos implementace, která možnost automatického předání spoléhá na zprávu.</span><span class="sxs-lookup"><span data-stu-id="20c97-125">In a transfer operation, a sender first sends a message to a "transfer queue" and the transfer queue immediately moves the message to the intended destination queue using the same robust transfer implementation that the auto-forward capability relies on.</span></span> <span data-ttu-id="20c97-126">Zpráva se nikdy zavazuje fronty přenosu protokolu tak, že se zobrazí u spotřebitelé fronty přenosu.</span><span class="sxs-lookup"><span data-stu-id="20c97-126">The message is never committed to the transfer queue's log in a way that it becomes visible for the transfer queue's consumers.</span></span>

<span data-ttu-id="20c97-127">Napájení tuto funkci transakční vyvstává zřejmá, když se fronta přenosu samotného zdroji vstupní zpráv odesílatele.</span><span class="sxs-lookup"><span data-stu-id="20c97-127">The power of this transactional capability becomes apparent when the transfer queue itself is the source of the sender's input messages.</span></span> <span data-ttu-id="20c97-128">Jinými slovy, Service Bus můžete přenést do cílové fronty "přes" fronty přenosu při provádění úplná (nebo odložení, nebo nedoručených zpráv) operaci na vstupní zprávu, všechny v rámci jedné atomické operace.</span><span class="sxs-lookup"><span data-stu-id="20c97-128">In other words, Service Bus can transfer the message to the destination queue "via" the transfer queue, while performing a complete (or defer, or dead-letter) operation on the input message, all in one atomic operation.</span></span> 

### <a name="see-it-in-code"></a><span data-ttu-id="20c97-129">Zobrazení v kódu</span><span class="sxs-lookup"><span data-stu-id="20c97-129">See it in code</span></span>
<span data-ttu-id="20c97-130">Chcete-li nastavit tyto přenosy, vytvořte odesílatele zprávy, který cílí cílové fronty prostřednictvím fronty přenosu.</span><span class="sxs-lookup"><span data-stu-id="20c97-130">To set up such transfers, you create a message sender that targets the destination queue via the transfer queue.</span></span> <span data-ttu-id="20c97-131">Také budete mít příjemce, který žádá o zprávy z této fronty stejné.</span><span class="sxs-lookup"><span data-stu-id="20c97-131">You will also have a receiver that pulls messages from that same queue.</span></span> <span data-ttu-id="20c97-132">Například:</span><span class="sxs-lookup"><span data-stu-id="20c97-132">For example:</span></span>

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

<span data-ttu-id="20c97-133">Jednoduché transakce pak používá tyto prvky, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="20c97-133">A simple transaction then uses these elements, as in the following example:</span></span>

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a><span data-ttu-id="20c97-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20c97-134">Next steps</span></span>

<span data-ttu-id="20c97-135">Najdete v následujících článcích pro další informace o fronty Service Bus:</span><span class="sxs-lookup"><span data-stu-id="20c97-135">See the following articles for more information about Service Bus queues:</span></span>

* [<span data-ttu-id="20c97-136">Řetězení entit služby Service Bus s automatické přesměrování</span><span class="sxs-lookup"><span data-stu-id="20c97-136">Chaining Service Bus entities with auto-forwarding</span></span>](service-bus-auto-forwarding.md)
* [<span data-ttu-id="20c97-137">Ukázka automatické vpřed</span><span class="sxs-lookup"><span data-stu-id="20c97-137">Auto-forward sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [<span data-ttu-id="20c97-138">Jednotlivé transakce s ukázkou Service Bus</span><span class="sxs-lookup"><span data-stu-id="20c97-138">Atomic Transactions with Service Bus sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [<span data-ttu-id="20c97-139">Azure Service Bus a fronty fronty porovnání</span><span class="sxs-lookup"><span data-stu-id="20c97-139">Azure Queues and Service Bus queues compared</span></span>](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [<span data-ttu-id="20c97-140">Jak používat fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="20c97-140">How to use Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

