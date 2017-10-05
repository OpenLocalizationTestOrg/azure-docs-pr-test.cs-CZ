---
title: "Jak používat fronty Azure Service Bus s Pythonem | Microsoft Docs"
description: "Naučte se používat fronty Azure Service Bus z Pythonu."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm;lmazuel
ms.openlocfilehash: e1e81ad1d7b4fe0e044917f090cac59dfd5b6332
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-python"></a><span data-ttu-id="b8517-103">Jak používat fronty Service Bus s Pythonem</span><span class="sxs-lookup"><span data-stu-id="b8517-103">How to use Service Bus queues with Python</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="b8517-104">Tento článek popisuje, jak používat fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b8517-104">This article describes how to use Service Bus queues.</span></span> <span data-ttu-id="b8517-105">Ukázky jsou napsané v Pythonu a použití [balíček Python Azure Service Bus][Python Azure Service Bus package].</span><span class="sxs-lookup"><span data-stu-id="b8517-105">The samples are written in Python and use the [Python Azure Service Bus package][Python Azure Service Bus package].</span></span> <span data-ttu-id="b8517-106">Pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="b8517-106">The scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> <span data-ttu-id="b8517-107">K instalaci Pythonu nebo [balíček Python Azure Service Bus][Python Azure Service Bus package], najdete v článku [Průvodce instalací Python](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="b8517-107">To install Python or the [Python Azure Service Bus package][Python Azure Service Bus package], see the [Python Installation Guide](../python-how-to-install.md).</span></span>
> 
> 

## <a name="create-a-queue"></a><span data-ttu-id="b8517-108">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="b8517-108">Create a queue</span></span>
<span data-ttu-id="b8517-109">**ServiceBusService** objektu umožňuje pracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="b8517-109">The **ServiceBusService** object enables you to work with queues.</span></span> <span data-ttu-id="b8517-110">Přidejte následující kód v horní jakéhokoliv Python souboru, ve kterém chcete programový přístupu k Service Bus:</span><span class="sxs-lookup"><span data-stu-id="b8517-110">Add the following code near the top of any Python file in which you wish to programmatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

<span data-ttu-id="b8517-111">Následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="b8517-111">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="b8517-112">Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` se váš obor názvů, název klíče sdíleného přístupového podpisu (SAS) a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="b8517-112">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your namespace, shared access signature (SAS) key name, and value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="b8517-113">Hodnoty pro název klíče SAS a hodnotu lze nalézt v [portál Azure] [ Azure portal] informace o připojení, nebo v sadě Visual Studio **vlastnosti** podokně při výběru službu Obor názvů sběrnice v Průzkumníku serveru (jak je uvedeno v předchozí části).</span><span class="sxs-lookup"><span data-stu-id="b8517-113">The values for the SAS key name and value can be found in the [Azure portal][Azure portal] connection information, or in the Visual Studio **Properties** pane when selecting the Service Bus namespace in Server Explorer (as shown in the previous section).</span></span>

```python
bus_service.create_queue('taskqueue')
```

<span data-ttu-id="b8517-114">`create_queue` Metoda také podporuje další možnosti, které vám umožní přepsat výchozí nastavení fronty například čas zprávy live (TTL) nebo maximální velikost fronty.</span><span class="sxs-lookup"><span data-stu-id="b8517-114">The `create_queue` method also supports additional options, which enable you to override default queue settings such as message time to live (TTL) or maximum queue size.</span></span> <span data-ttu-id="b8517-115">Následující příklad nastaví na 5 GB a hodnota TTL na 1 minutu maximální velikost fronty:</span><span class="sxs-lookup"><span data-stu-id="b8517-115">The following example sets the maximum queue size to 5 GB, and the TTL value to 1 minute:</span></span>

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="b8517-116">Zasílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="b8517-116">Send messages to a queue</span></span>
<span data-ttu-id="b8517-117">K odeslání zprávy do fronty Service Bus, vaše aplikace volání `send_queue_message` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="b8517-117">To send a message to a Service Bus queue, your application calls the `send_queue_message` method on the **ServiceBusService** object.</span></span>

<span data-ttu-id="b8517-118">Následující příklad ukazuje, jak odeslat testovací zprávu do fronty s názvem `taskqueue` pomocí `send_queue_message`:</span><span class="sxs-lookup"><span data-stu-id="b8517-118">The following example demonstrates how to send a test message to the queue named `taskqueue` using `send_queue_message`:</span></span>

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="b8517-119">Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="b8517-119">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="b8517-120">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="b8517-120">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="b8517-121">Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je.</span><span class="sxs-lookup"><span data-stu-id="b8517-121">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="b8517-122">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="b8517-122">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="b8517-123">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="b8517-123">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="b8517-124">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="b8517-124">Receive messages from a queue</span></span>
<span data-ttu-id="b8517-125">Přijímání zpráv z fronty pomocí `receive_queue_message` metodu **ServiceBusService** objektu:</span><span class="sxs-lookup"><span data-stu-id="b8517-125">Messages are received from a queue using the `receive_queue_message` method on the **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="b8517-126">Zprávy jsou odstraněny z fronty, jako při jejich přečtení parametr `peek_lock` je nastaven na **False**.</span><span class="sxs-lookup"><span data-stu-id="b8517-126">Messages are deleted from the queue as they are read when the parameter `peek_lock` is set to **False**.</span></span> <span data-ttu-id="b8517-127">Můžete číst (funkce Náhled) a uzamčení zpráva bez odstranění z fronty nastavením parametru `peek_lock` k **True**.</span><span class="sxs-lookup"><span data-stu-id="b8517-127">You can read (peek) and lock the message without deleting it from the queue by setting the parameter `peek_lock` to **True**.</span></span>

<span data-ttu-id="b8517-128">Chování čtení a odstranění zprávy v rámci operace příjmu je nejjednodušší model a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="b8517-128">The behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="b8517-129">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="b8517-129">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="b8517-130">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="b8517-130">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="b8517-131">Pokud `peek_lock` parametr je nastaven na **True**, receive stane dvoufázového operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="b8517-131">If the `peek_lock` parameter is set to **True**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="b8517-132">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8517-132">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="b8517-133">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání **odstranit** metodu **zpráva** objekt.</span><span class="sxs-lookup"><span data-stu-id="b8517-133">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling the **delete** method on the **Message** object.</span></span> <span data-ttu-id="b8517-134">**Odstranit** metoda bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="b8517-134">The **delete** method will mark the message as being consumed and remove it from the queue.</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="b8517-135">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="b8517-135">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="b8517-136">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="b8517-136">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="b8517-137">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak můžete volat **odemknutí** metodu **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="b8517-137">If a receiver application is unable to process the message for some reason, then it can call the **unlock** method on the **Message** object.</span></span> <span data-ttu-id="b8517-138">To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="b8517-138">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="b8517-139">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak se Service Bus zprávu automaticky odemkne a zpřístupní ji pro další přijetí.</span><span class="sxs-lookup"><span data-stu-id="b8517-139">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="b8517-140">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než **odstranit** metoda je volána, pak zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="b8517-140">In the event that the application crashes after processing the message but before the **delete** method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="b8517-141">To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="b8517-141">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="b8517-142">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="b8517-142">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="b8517-143">To se často opírá **MessageId** vlastnosti zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="b8517-143">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8517-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8517-144">Next steps</span></span>
<span data-ttu-id="b8517-145">Teď, když jste se naučili základy front Service Bus, najdete v těchto článcích Další informace.</span><span class="sxs-lookup"><span data-stu-id="b8517-145">Now that you have learned the basics of Service Bus queues, see these articles to learn more.</span></span>

* <span data-ttu-id="b8517-146">[Fronty, témata a odběry][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="b8517-146">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

