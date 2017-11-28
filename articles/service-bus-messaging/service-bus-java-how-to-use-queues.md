---
title: aaaHow toouse Azure Service Bus fronty s Javou | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure. Ukázky kódu napsanou v jazyce Java."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: f68e941438134090c5eee53459e7667bda13ff3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-java"></a><span data-ttu-id="bdbca-104">Jak toouse Service Bus fronty s Javou</span><span class="sxs-lookup"><span data-stu-id="bdbca-104">How toouse Service Bus queues with Java</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="bdbca-105">Tento článek popisuje, jak toouse fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="bdbca-105">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="bdbca-106">Hello ukázky jsou napsané v jazyce Java a používají hello [Azure SDK pro jazyk Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="bdbca-106">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="bdbca-107">Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="bdbca-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="bdbca-108">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="bdbca-108">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="bdbca-109">Ujistěte se, že máte nainstalovanou hello [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce.</span><span class="sxs-lookup"><span data-stu-id="bdbca-109">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="bdbca-110">Pokud používáte Eclipse, můžete nainstalovat hello [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] zahrnující hello Azure SDK pro jazyk Java.</span><span class="sxs-lookup"><span data-stu-id="bdbca-110">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="bdbca-111">Poté můžete přidat hello **knihovny Microsoft Azure Libraries for Java** tooyour projektu:</span><span class="sxs-lookup"><span data-stu-id="bdbca-111">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

<span data-ttu-id="bdbca-112">Přidejte následující hello `import` toohello příkazy na začátek souboru Java hello:</span><span class="sxs-lookup"><span data-stu-id="bdbca-112">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a><span data-ttu-id="bdbca-113">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="bdbca-113">Create a queue</span></span>
<span data-ttu-id="bdbca-114">Operace správy front služby Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy.</span><span class="sxs-lookup"><span data-stu-id="bdbca-114">Management operations for Service Bus queues can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="bdbca-115">A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje token SAS s oprávněními toomanage a hello **ServiceBusContract** třída je jediný bod hello komunikace s Azure.</span><span class="sxs-lookup"><span data-stu-id="bdbca-115">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="bdbca-116">Hello **ServiceBusService** třída poskytuje metody toocreate, výčet a odstranění front.</span><span class="sxs-lookup"><span data-stu-id="bdbca-116">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete queues.</span></span> <span data-ttu-id="bdbca-117">Hello Příklad dole ukazuje, jak **ServiceBusService** objekt může být použité toocreate frontu s názvem `TestQueue`, obor názvů s názvem `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="bdbca-117">hello example below shows how a **ServiceBusService** object can be used toocreate a queue named `TestQueue`, with a namespace named `HowToSample`:</span></span>

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="bdbca-118">Způsoby na `QueueInfo` které umožňují vlastnosti toobe fronty hello přizpůsobená (například: tooset hello výchozí time to live (TTL) hodnotu toobe použít toomessages odeslané toohello fronty).</span><span class="sxs-lookup"><span data-stu-id="bdbca-118">There are methods on `QueueInfo` that allow properties of hello queue toobe tuned (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello queue).</span></span> <span data-ttu-id="bdbca-119">Hello následující příklad ukazuje, jak toocreate frontu s názvem `TestQueue` s maximální velikostí 5 GB:</span><span class="sxs-lookup"><span data-stu-id="bdbca-119">hello following example shows how toocreate a queue named `TestQueue` with a maximum size of 5GB:</span></span>

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

<span data-ttu-id="bdbca-120">Všimněte si, které můžete použít hello `listQueues` metodu **ServiceBusContract** objekty toocheck Pokud fronta se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="bdbca-120">Note that you can use hello `listQueues` method on **ServiceBusContract** objects toocheck if a queue with a specified name already exists within a service namespace.</span></span>

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="bdbca-121">Odesílat zprávy fronty tooa</span><span class="sxs-lookup"><span data-stu-id="bdbca-121">Send messages tooa queue</span></span>
<span data-ttu-id="bdbca-122">Získá toosend fronty Service Bus zprávu tooa, aplikace **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="bdbca-122">toosend a message tooa Service Bus queue, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="bdbca-123">Následující kód ukazuje, jak Hello toosend zprávu o hello `TestQueue` fronty předtím vytvořili v hello `HowToSample` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="bdbca-123">hello following code shows how toosend a message for hello `TestQueue` queue previously created in hello `HowToSample` namespace:</span></span>

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="bdbca-124">Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="bdbca-124">Messages sent to, and received from Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="bdbca-125">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="bdbca-125">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties (such as [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) and [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="bdbca-126">Aplikace můžete nastavit hello těla zprávy hello tak předá jakýkoli serializovatelný objekt do konstruktoru hello hello [BrokeredMessage][BrokeredMessage], a odpovídající serializátor hello se pak použije objekt tooserialize hello.</span><span class="sxs-lookup"><span data-stu-id="bdbca-126">An application can set hello body of hello message by passing any serializable object into hello constructor of hello [BrokeredMessage][BrokeredMessage], and hello appropriate serializer will then be used tooserialize hello object.</span></span> <span data-ttu-id="bdbca-127">Alternativně můžete zadat **java. VSTUPNĚ-VÝSTUPNÍ OPERACE. Třídy InputStream** objektu.</span><span class="sxs-lookup"><span data-stu-id="bdbca-127">Alternatively, you can provide a **java.IO.InputStream** object.</span></span>

<span data-ttu-id="bdbca-128">Hello následující příklad ukazuje, jak testovací toosend pět zprávy toothe `TestQueue` **MessageSender** jsme získali v předchozím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="bdbca-128">hello following example demonstrates how toosend five test messages toothe `TestQueue` **MessageSender** we obtained in hello previous code snippet:</span></span>

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for hello body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message toohello queue
     service.sendQueueMessage("TestQueue", message);
}
```

<span data-ttu-id="bdbca-129">Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="bdbca-129">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="bdbca-130">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="bdbca-130">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="bdbca-131">Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě.</span><span class="sxs-lookup"><span data-stu-id="bdbca-131">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="bdbca-132">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="bdbca-132">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="bdbca-133">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="bdbca-133">Receive messages from a queue</span></span>
<span data-ttu-id="bdbca-134">Hello primární způsob tooreceive zprávy z fronty je toouse **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="bdbca-134">hello primary way tooreceive messages from a queue is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="bdbca-135">Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="bdbca-135">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="bdbca-136">Při použití hello **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdbca-136">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="bdbca-137">**ReceiveAndDelete** režimu (což je výchozí režim hello) je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="bdbca-137">**ReceiveAndDelete** mode (which is hello default mode) is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="bdbca-138">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="bdbca-138">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span>
<span data-ttu-id="bdbca-139">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="bdbca-139">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="bdbca-140">V **PeekLock** režimu, zobrazí se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="bdbca-140">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="bdbca-141">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdbca-141">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="bdbca-142">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním **odstranit** na uvítací přijal zprávu.</span><span class="sxs-lookup"><span data-stu-id="bdbca-142">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="bdbca-143">Když Service Bus uvidí hello **odstranit** volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="bdbca-143">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="bdbca-144">Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (ne hello výchozí režim).</span><span class="sxs-lookup"><span data-stu-id="bdbca-144">hello following example demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="bdbca-145">Hello příklad nepodporuje nekonečné smyčce a zpracuje zprávy, když dorazí do našich `TestQueue`:</span><span class="sxs-lookup"><span data-stu-id="bdbca-145">hello example below does an infinite loop and processes messages as they arrive into our `TestQueue`:</span></span>

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display hello queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added toohandle no more messages.
            // Could instead wait for more messages toobe added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="bdbca-146">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="bdbca-146">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="bdbca-147">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="bdbca-147">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="bdbca-148">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **unlockMessage** na hello přijal zprávu (místo hello **deleteMessage** metoda).</span><span class="sxs-lookup"><span data-stu-id="bdbca-148">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="bdbca-149">Tato příčiny hello toounlock sběrnice zpráv ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="bdbca-149">This causes Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="bdbca-150">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud aplikace hello selže tooprocess hello zprávy vyprší časový limit uzamčení (například pokud hello aplikace spadne), pak Service Bus automaticky odemkne uvítací zprávu a je k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="bdbca-150">There is also a timeout associated with a message locked within the queue, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="bdbca-151">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **deleteMessage** požadavku a potom uvítací zprávu je víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="bdbca-151">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="bdbca-152">To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="bdbca-152">This is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="bdbca-153">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="bdbca-153">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="bdbca-154">To se často opírá hello **getMessageId** metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="bdbca-154">This is often achieved using hello **getMessageId** method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdbca-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bdbca-155">Next Steps</span></span>
<span data-ttu-id="bdbca-156">Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="bdbca-156">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="bdbca-157">Další informace najdete v tématu hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="bdbca-157">For more information, see hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
