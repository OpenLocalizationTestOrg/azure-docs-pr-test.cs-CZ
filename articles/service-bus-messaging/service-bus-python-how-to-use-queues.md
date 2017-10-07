---
title: aaaHow toouse Azure Service Bus fronty s Pythonem | Microsoft Docs
description: "Zjistěte, jak fronty Azure Service Bus toouse z Pythonu."
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
ms.openlocfilehash: bceb84d04ff3445c3087a9c246c583d6630f07af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-python"></a><span data-ttu-id="cd59d-103">Jak toouse Service Bus fronty s Pythonem</span><span class="sxs-lookup"><span data-stu-id="cd59d-103">How toouse Service Bus queues with Python</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="cd59d-104">Tento článek popisuje, jak toouse fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="cd59d-104">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="cd59d-105">Hello ukázky jsou napsané v Pythonu a používají hello [balíček Python Azure Service Bus][Python Azure Service Bus package].</span><span class="sxs-lookup"><span data-stu-id="cd59d-105">hello samples are written in Python and use hello [Python Azure Service Bus package][Python Azure Service Bus package].</span></span> <span data-ttu-id="cd59d-106">Hello pokryté scénáře zahrnují **vytváření front, odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="cd59d-106">hello scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> <span data-ttu-id="cd59d-107">tooinstall Python nebo hello [balíček Python Azure Service Bus][Python Azure Service Bus package], najdete v části hello [Průvodce instalací Python](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="cd59d-107">tooinstall Python or hello [Python Azure Service Bus package][Python Azure Service Bus package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>
> 
> 

## <a name="create-a-queue"></a><span data-ttu-id="cd59d-108">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="cd59d-108">Create a queue</span></span>
<span data-ttu-id="cd59d-109">Hello **ServiceBusService** objekt vám umožní toowork s fronty.</span><span class="sxs-lookup"><span data-stu-id="cd59d-109">hello **ServiceBusService** object enables you toowork with queues.</span></span> <span data-ttu-id="cd59d-110">Přidejte následující kód v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístup Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="cd59d-110">Add hello following code near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

<span data-ttu-id="cd59d-111">Hello následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="cd59d-111">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="cd59d-112">Nahraďte `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` se váš obor názvů, název klíče sdíleného přístupového podpisu (SAS) a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="cd59d-112">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your namespace, shared access signature (SAS) key name, and value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="cd59d-113">Hello hodnoty hello název klíče SAS a hodnotu lze nalézt v hello [portál Azure] [ Azure portal] informace o připojení, nebo v sadě Visual Studio hello **vlastnosti** podokně při výběru Hello oboru názvů Service Bus v Průzkumníku serveru (jak je uvedeno v předchozí části hello).</span><span class="sxs-lookup"><span data-stu-id="cd59d-113">hello values for hello SAS key name and value can be found in hello [Azure portal][Azure portal] connection information, or in hello Visual Studio **Properties** pane when selecting hello Service Bus namespace in Server Explorer (as shown in hello previous section).</span></span>

```python
bus_service.create_queue('taskqueue')
```

<span data-ttu-id="cd59d-114">Hello `create_queue` metoda také podporuje další možnosti, které umožňují toooverride výchozí nastavení fronty například čas toolive zprávy (TTL) nebo maximální velikost fronty.</span><span class="sxs-lookup"><span data-stu-id="cd59d-114">hello `create_queue` method also supports additional options, which enable you toooverride default queue settings such as message time toolive (TTL) or maximum queue size.</span></span> <span data-ttu-id="cd59d-115">Hello následující příklad ilustruje hello fronty maximální velikost too5 GB a minutu too1 hodnotu TTL hello:</span><span class="sxs-lookup"><span data-stu-id="cd59d-115">hello following example sets hello maximum queue size too5 GB, and hello TTL value too1 minute:</span></span>

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="cd59d-116">Odesílat zprávy fronty tooa</span><span class="sxs-lookup"><span data-stu-id="cd59d-116">Send messages tooa queue</span></span>
<span data-ttu-id="cd59d-117">toosend fronty Service Bus zprávu tooa aplikace volá hello `send_queue_message` metodu hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="cd59d-117">toosend a message tooa Service Bus queue, your application calls hello `send_queue_message` method on hello **ServiceBusService** object.</span></span>

<span data-ttu-id="cd59d-118">Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `taskqueue` pomocí `send_queue_message`:</span><span class="sxs-lookup"><span data-stu-id="cd59d-118">hello following example demonstrates how toosend a test message toohello queue named `taskqueue` using `send_queue_message`:</span></span>

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="cd59d-119">Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="cd59d-119">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="cd59d-120">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="cd59d-120">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="cd59d-121">Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě.</span><span class="sxs-lookup"><span data-stu-id="cd59d-121">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="cd59d-122">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="cd59d-122">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="cd59d-123">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="cd59d-123">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="cd59d-124">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="cd59d-124">Receive messages from a queue</span></span>
<span data-ttu-id="cd59d-125">Přijímání zpráv z fronty pomocí hello `receive_queue_message` metodu hello **ServiceBusService** objektu:</span><span class="sxs-lookup"><span data-stu-id="cd59d-125">Messages are received from a queue using hello `receive_queue_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="cd59d-126">Zprávy jsou odstraněny z fronty hello jako jejich přečtení při hello parametr `peek_lock` je nastaven příliš**False**.</span><span class="sxs-lookup"><span data-stu-id="cd59d-126">Messages are deleted from hello queue as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="cd59d-127">Můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello parametrem hello nastavení `peek_lock` příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="cd59d-127">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="cd59d-128">Hello chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="cd59d-128">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="cd59d-129">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="cd59d-129">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="cd59d-130">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="cd59d-130">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="cd59d-131">Pokud hello `peek_lock` parametr je nastaven příliš**True**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="cd59d-131">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="cd59d-132">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd59d-132">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="cd59d-133">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello obdrží proces volání hello **odstranit** metodu hello **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="cd59d-133">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling hello **delete** method on hello **Message** object.</span></span> <span data-ttu-id="cd59d-134">Hello **odstranit** metoda bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="cd59d-134">hello **delete** method will mark hello message as being consumed and remove it from hello queue.</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="cd59d-135">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="cd59d-135">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="cd59d-136">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="cd59d-136">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="cd59d-137">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **odemknutí** metodu hello **zpráva** objektu.</span><span class="sxs-lookup"><span data-stu-id="cd59d-137">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlock** method on hello **Message** object.</span></span> <span data-ttu-id="cd59d-138">To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="cd59d-138">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="cd59d-139">Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud selže hello aplikace, které tooprocess hello zprávy před hello zámku vyprší časový limit (např. Pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="cd59d-139">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="cd59d-140">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **odstranit** metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="cd59d-140">In hello event that hello application crashes after processing hello message but before hello **delete** method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="cd59d-141">To se často označuje jako **zpracování nejméně jednou**, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="cd59d-141">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="cd59d-142">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="cd59d-142">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="cd59d-143">To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="cd59d-143">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd59d-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd59d-144">Next steps</span></span>
<span data-ttu-id="cd59d-145">Teď, když jste se naučili základy hello front Service Bus, najdete v těchto článcích toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="cd59d-145">Now that you have learned hello basics of Service Bus queues, see these articles toolearn more.</span></span>

* <span data-ttu-id="cd59d-146">[Fronty, témata a odběry][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="cd59d-146">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

