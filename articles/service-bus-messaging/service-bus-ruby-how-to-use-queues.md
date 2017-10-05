---
title: "Jak používat fronty Azure Service Bus s Ruby | Microsoft Docs"
description: "Naučte se používat fronty Service Bus v Azure. Ukázky kódu jsou vytvořeny v Ruby."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 357a7277dd42b6973cf35a9f642b8eec36a745e3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-queues-with-ruby"></a><span data-ttu-id="8e2a0-104">Jak používat fronty Service Bus s Ruby</span><span class="sxs-lookup"><span data-stu-id="8e2a0-104">How to use Service Bus queues with Ruby</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="8e2a0-105">Tato příručka popisuje, jak používat fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-105">This guide describes how to use Service Bus queues.</span></span> <span data-ttu-id="8e2a0-106">Ukázky jsou napsané v Ruby a používají Azure gem.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-106">The samples are written in Ruby and use the Azure gem.</span></span> <span data-ttu-id="8e2a0-107">Pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-107">The scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="8e2a0-108">Další informace o fronty Service Bus, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-108">For more information about Service Bus queues, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="8e2a0-109">Postup vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="8e2a0-109">How to create a queue</span></span>
<span data-ttu-id="8e2a0-110">**Azure::ServiceBusService** objektu umožňuje pracovat s fronty.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-110">The **Azure::ServiceBusService** object enables you to work with queues.</span></span> <span data-ttu-id="8e2a0-111">Chcete-li vytvořit frontu, použijte `create_queue()` metoda.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-111">To create a queue, use the `create_queue()` method.</span></span> <span data-ttu-id="8e2a0-112">Následující příklad vytvoří frontu nebo vytiskne případné chyby.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-112">The following example creates a queue or prints out any errors.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

<span data-ttu-id="8e2a0-113">Můžete také předat **Azure::ServiceBus::Queue** objektu s další možnosti, které můžete přepsat výchozí nastavení fronty, jako je například čas zprávy do fronty za provozu nebo maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-113">You can also pass a **Azure::ServiceBus::Queue** object with additional options, which enables you to override the default queue settings, such as message time to live or maximum queue size.</span></span> <span data-ttu-id="8e2a0-114">Následující příklad ukazuje, jak nastavit maximální velikost fronty 5 GB a čas TTL na 1 minutu:</span><span class="sxs-lookup"><span data-stu-id="8e2a0-114">The following example shows how to set the maximum queue size to 5 GB and time to live to 1 minute:</span></span>

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a><span data-ttu-id="8e2a0-115">Odesílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="8e2a0-115">How to send messages to a queue</span></span>
<span data-ttu-id="8e2a0-116">K odeslání zprávy do fronty Service Bus, vaše aplikace volání `send_queue_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-116">To send a message to a Service Bus queue, your application calls the `send_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="8e2a0-117">Zprávy odeslané do (a přijaté z) jsou fronty Service Bus **Azure::ServiceBus::BrokeredMessage** objekty a mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník, který se používá k ukládání vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-117">Messages sent to (and received from) Service Bus queues are **Azure::ServiceBus::BrokeredMessage** objects, and have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="8e2a0-118">Aplikace může tělo zprávy nastavit předáním řetězcovou hodnotu jako zprávu, a jakékoliv nezbytné vlastnosti, standardní se naplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-118">An application can set the body of the message by passing a string value as the message and any required standard properties are populated with default values.</span></span>

<span data-ttu-id="8e2a0-119">Následující příklad ukazuje, jak odeslat testovací zprávu do fronty s názvem `test-queue` pomocí `send_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="8e2a0-119">The following example demonstrates how to send a test message to the queue named `test-queue` using `send_queue_message()`:</span></span>

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

<span data-ttu-id="8e2a0-120">Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="8e2a0-120">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="8e2a0-121">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-121">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="8e2a0-122">Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-122">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="8e2a0-123">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-123">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-to-receive-messages-from-a-queue"></a><span data-ttu-id="8e2a0-124">Jak přijmout zprávy z fronty</span><span class="sxs-lookup"><span data-stu-id="8e2a0-124">How to receive messages from a queue</span></span>
<span data-ttu-id="8e2a0-125">Přijímání zpráv z fronty pomocí `receive_queue_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-125">Messages are received from a queue using the `receive_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="8e2a0-126">Zprávy jsou ve výchozím nastavení, přečtěte si a uzamčení bez odstraňuje z fronty.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-126">By default, messages are read and locked without being deleted from the queue.</span></span> <span data-ttu-id="8e2a0-127">Však můžete odstranit zprávy z fronty se čtou nastavením `:peek_lock` možnost k **false**.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-127">However, you can delete messages from the queue as they are read by setting the `:peek_lock` option to **false**.</span></span>

<span data-ttu-id="8e2a0-128">Výchozí chování umožňuje čtení a odstraňování dvoufázová operaci, která také umožňuje podpora aplikací, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-128">The default behavior makes the reading and deleting a two-stage operation, which also makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="8e2a0-129">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-129">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="8e2a0-130">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání `delete_queue_message()` metoda a poskytující zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-130">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete_queue_message()` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="8e2a0-131">`delete_queue_message()` Metoda bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-131">The `delete_queue_message()` method will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="8e2a0-132">Pokud `:peek_lock` parametr je nastaven na **false**, čtení a odstranění zprávy stane nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-132">If the `:peek_lock` parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="8e2a0-133">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-133">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="8e2a0-134">Vzhledem k tomu, že Service Bus označila zprávu jako spotřebovávanou, když se aplikace restartuje a začne znovu přijímat zprávy, se neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-134">Because Service Bus has marked the message as being consumed, when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="8e2a0-135">Následující příklad ukazuje, jak přijímat a zpracovávat zprávy pomocí `receive_queue_message()`.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-135">The following example demonstrates how to receive and process messages using `receive_queue_message()`.</span></span> <span data-ttu-id="8e2a0-136">V příkladu nejprve přijme a odstraní zprávu pomocí `:peek_lock` nastavena na **false**, pak přijetí další zprávu a poté se odstraní zprávu pomocí `delete_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="8e2a0-136">The example first receives and deletes a message by using `:peek_lock` set to **false**, then it receives another message and then deletes the message using `delete_queue_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="8e2a0-137">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="8e2a0-137">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="8e2a0-138">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-138">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="8e2a0-139">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlock_queue_message()` metodu **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-139">If a receiver application is unable to process the message for some reason, then it can call the `unlock_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="8e2a0-140">Toto volání způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-140">This call causes Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="8e2a0-141">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak Service Bus zprávu automaticky odemkne a ji zpřístupní k přijetí.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-141">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="8e2a0-142">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `delete_queue_message()` metoda je volána, pak zprávy je víckrát do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-142">In the event that the application crashes after processing the message but before the `delete_queue_message()` method is called, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="8e2a0-143">Tento proces se často nazývá *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-143">This process is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="8e2a0-144">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-144">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="8e2a0-145">To se často opírá `message_id` vlastnosti zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-145">This is often achieved using the `message_id` property of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e2a0-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e2a0-146">Next steps</span></span>
<span data-ttu-id="8e2a0-147">Naučili jste se základy front Service Bus, další informace se dozvíte na následujících odkazech.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-147">Now that you've learned the basics of Service Bus queues, follow these links to learn more.</span></span>

* <span data-ttu-id="8e2a0-148">Přehled [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="8e2a0-148">Overview of [queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="8e2a0-149">Přejděte [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="8e2a0-149">Visit the [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

<span data-ttu-id="8e2a0-150">Porovnání mezi fronty Azure Service Bus, které jsou popsané v tomto článku a fronty Azure, které jsou popsané v [postup používání úložiště Queue z Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) článku najdete v tématu [fronty Azure a Azure fronty služby Service Bus - porovnání a Rozdíl od aktualizovaného](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="8e2a0-150">For a comparison between the Azure Service Bus queues discussed in this article and Azure Queues discussed in the [How to use Queue storage from Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) article, see [Azure Queues and Azure Service Bus Queues - Compared and Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>

