---
title: aaaHow toouse Azure Service Bus fronty s Ruby | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure. Ukázky kódu jsou vytvořeny v Ruby."
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
ms.openlocfilehash: 7270154583f98e3372e82efbb967ea7a5acd1686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-ruby"></a><span data-ttu-id="1cd95-104">Jak toouse Service Bus fronty s Ruby</span><span class="sxs-lookup"><span data-stu-id="1cd95-104">How toouse Service Bus queues with Ruby</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="1cd95-105">Tato příručka popisuje, jak toouse fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1cd95-105">This guide describes how toouse Service Bus queues.</span></span> <span data-ttu-id="1cd95-106">Hello ukázky jsou napsané v Ruby a používají hello Azure gem.</span><span class="sxs-lookup"><span data-stu-id="1cd95-106">hello samples are written in Ruby and use hello Azure gem.</span></span> <span data-ttu-id="1cd95-107">Hello pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="1cd95-107">hello scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="1cd95-108">Další informace o fronty Service Bus, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="1cd95-108">For more information about Service Bus queues, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-toocreate-a-queue"></a><span data-ttu-id="1cd95-109">Jak toocreate fronty</span><span class="sxs-lookup"><span data-stu-id="1cd95-109">How toocreate a queue</span></span>
<span data-ttu-id="1cd95-110">Hello **Azure::ServiceBusService** objekt vám umožní toowork s fronty.</span><span class="sxs-lookup"><span data-stu-id="1cd95-110">hello **Azure::ServiceBusService** object enables you toowork with queues.</span></span> <span data-ttu-id="1cd95-111">toocreate fronty, použijte hello `create_queue()` metoda.</span><span class="sxs-lookup"><span data-stu-id="1cd95-111">toocreate a queue, use hello `create_queue()` method.</span></span> <span data-ttu-id="1cd95-112">Hello následující příklad vytvoří frontu nebo vytiskne případné chyby.</span><span class="sxs-lookup"><span data-stu-id="1cd95-112">hello following example creates a queue or prints out any errors.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

<span data-ttu-id="1cd95-113">Můžete také předat **Azure::ServiceBus::Queue** objektu s další možnosti, což vám umožní toooverride hello výchozí nastavení fronty, jako je například velikost fronty toolive nebo maximální doba zprávy.</span><span class="sxs-lookup"><span data-stu-id="1cd95-113">You can also pass a **Azure::ServiceBus::Queue** object with additional options, which enables you toooverride hello default queue settings, such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="1cd95-114">Hello následující příklad ukazuje, jak tooset hello fronty maximální velikost too5 GB a čas toolive too1 minutu:</span><span class="sxs-lookup"><span data-stu-id="1cd95-114">hello following example shows how tooset hello maximum queue size too5 GB and time toolive too1 minute:</span></span>

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-toosend-messages-tooa-queue"></a><span data-ttu-id="1cd95-115">Jak toosend tooa fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="1cd95-115">How toosend messages tooa queue</span></span>
<span data-ttu-id="1cd95-116">toosend fronty Service Bus zprávu tooa aplikace volá hello `send_queue_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1cd95-116">toosend a message tooa Service Bus queue, your application calls hello `send_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1cd95-117">Zprávy odeslané příliš (a přijaté z) jsou fronty služby Service Bus **Azure::ServiceBus::BrokeredMessage** objekty a mají sadu standardních vlastností (jako například `label` a `time_to_live`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="1cd95-117">Messages sent too(and received from) Service Bus queues are **Azure::ServiceBus::BrokeredMessage** objects, and have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="1cd95-118">Aplikace můžete nastavit hello těla zprávy hello předáním řetězcovou hodnotu jako uvítací zprávu a jakékoliv nezbytné vlastnosti, standardní se naplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1cd95-118">An application can set hello body of hello message by passing a string value as hello message and any required standard properties are populated with default values.</span></span>

<span data-ttu-id="1cd95-119">Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `test-queue` pomocí `send_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="1cd95-119">hello following example demonstrates how toosend a test message toohello queue named `test-queue` using `send_queue_message()`:</span></span>

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

<span data-ttu-id="1cd95-120">Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="1cd95-120">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="1cd95-121">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="1cd95-121">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="1cd95-122">Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě.</span><span class="sxs-lookup"><span data-stu-id="1cd95-122">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="1cd95-123">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="1cd95-123">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-tooreceive-messages-from-a-queue"></a><span data-ttu-id="1cd95-124">Jak tooreceive zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="1cd95-124">How tooreceive messages from a queue</span></span>
<span data-ttu-id="1cd95-125">Přijímání zpráv z fronty pomocí hello `receive_queue_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1cd95-125">Messages are received from a queue using hello `receive_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1cd95-126">Zprávy jsou ve výchozím nastavení, přečtěte si a uzamčení bez odstraňuje z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="1cd95-126">By default, messages are read and locked without being deleted from hello queue.</span></span> <span data-ttu-id="1cd95-127">Však můžete odstranit zprávy z fronty hello číst nastavení hello `:peek_lock` možnost příliš**false**.</span><span class="sxs-lookup"><span data-stu-id="1cd95-127">However, you can delete messages from hello queue as they are read by setting hello `:peek_lock` option too**false**.</span></span>

<span data-ttu-id="1cd95-128">výchozí chování Hello díky hello čtení a odstraňování dvoufázová operace, takže také je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="1cd95-128">hello default behavior makes hello reading and deleting a two-stage operation, which also makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="1cd95-129">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1cd95-129">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="1cd95-130">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `delete_queue_message()` metoda a poskytování toobe hello zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="1cd95-130">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete_queue_message()` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="1cd95-131">Hello `delete_queue_message()` metoda bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="1cd95-131">hello `delete_queue_message()` method will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="1cd95-132">Pokud hello `:peek_lock` parametr je nastaven příliš**false**, čtení a odstranění uvítací zprávu stane hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat není zpracování zprávy ve hello události došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="1cd95-132">If hello `:peek_lock` parameter is set too**false**, reading and deleting hello message becomes hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="1cd95-133">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="1cd95-133">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="1cd95-134">Vzhledem k tomu, že Service Bus označila uvítací zprávu jako spotřebovávanou, když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="1cd95-134">Because Service Bus has marked hello message as being consumed, when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="1cd95-135">Hello následující příklad ukazuje, jak se proces a tooreceive zprávy, pomocí `receive_queue_message()`.</span><span class="sxs-lookup"><span data-stu-id="1cd95-135">hello following example demonstrates how tooreceive and process messages using `receive_queue_message()`.</span></span> <span data-ttu-id="1cd95-136">Příklad Hello nejprve přijme a odstraní zprávu pomocí `:peek_lock` nastavit příliš**false**, obdrží další zprávu a pak odstraní hello zprávu pomocí `delete_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="1cd95-136">hello example first receives and deletes a message by using `:peek_lock` set too**false**, then it receives another message and then deletes hello message using `delete_queue_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="1cd95-137">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="1cd95-137">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="1cd95-138">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="1cd95-138">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="1cd95-139">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlock_queue_message()` metodu hello **Azure::ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1cd95-139">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1cd95-140">Toto volání příčiny hello toounlock sběrnice zpráv ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="1cd95-140">This call causes Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="1cd95-141">Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="1cd95-141">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="1cd95-142">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `delete_queue_message()` metoda je volána, pak uvítací zprávu je víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="1cd95-142">In hello event that hello application crashes after processing hello message but before hello `delete_queue_message()` method is called, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="1cd95-143">Tento proces se často nazývá *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="1cd95-143">This process is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="1cd95-144">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="1cd95-144">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="1cd95-145">To se často opírá hello `message_id` vlastnost hello zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="1cd95-145">This is often achieved using hello `message_id` property of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1cd95-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1cd95-146">Next steps</span></span>
<span data-ttu-id="1cd95-147">Teď, když jste se naučili základy hello front Service Bus, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="1cd95-147">Now that you've learned hello basics of Service Bus queues, follow these links toolearn more.</span></span>

* <span data-ttu-id="1cd95-148">Přehled [fronty, témata a odběry](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="1cd95-148">Overview of [queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="1cd95-149">Navštivte hello [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1cd95-149">Visit hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

<span data-ttu-id="1cd95-150">Porovnání mezi fronty Azure Service Bus hello popsané v tomto článku a fronty Azure, které jsou popsané v hello [jak toouse úložiště Queue z Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) článku najdete v tématu [fronty Azure a Azure fronty služby Service Bus - porovnání a kontrastní](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="1cd95-150">For a comparison between hello Azure Service Bus queues discussed in this article and Azure Queues discussed in hello [How toouse Queue storage from Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) article, see [Azure Queues and Azure Service Bus Queues - Compared and Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>

