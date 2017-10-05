---
title: "Jak používat fronty Azure Service Bus s Javou | Microsoft Docs"
description: "Naučte se používat fronty Service Bus v Azure. Ukázky kódu napsanou v jazyce Java."
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
ms.openlocfilehash: 170f431525ffdc93a01fc085e48e69c3a774968e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-java"></a><span data-ttu-id="1832c-104">Jak používat fronty Service Bus s Javou</span><span class="sxs-lookup"><span data-stu-id="1832c-104">How to use Service Bus queues with Java</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="1832c-105">Tento článek popisuje, jak používat fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1832c-105">This article describes how to use Service Bus queues.</span></span> <span data-ttu-id="1832c-106">Ukázky jsou napsané v jazyce Java a použít [Azure SDK pro jazyk Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="1832c-106">The samples are written in Java and use the [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="1832c-107">Pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="1832c-107">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="1832c-108">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="1832c-108">Configure your application to use Service Bus</span></span>
<span data-ttu-id="1832c-109">Ujistěte se, že máte nainstalovanou [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce.</span><span class="sxs-lookup"><span data-stu-id="1832c-109">Make sure you have installed the [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="1832c-110">Pokud používáte Eclipse, můžete nainstalovat [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] , který obsahuje sadu Azure SDK pro jazyk Java.</span><span class="sxs-lookup"><span data-stu-id="1832c-110">If you are using Eclipse, you can install the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes the Azure SDK for Java.</span></span> <span data-ttu-id="1832c-111">Poté můžete přidat **knihovny Microsoft Azure Libraries for Java** do projektu:</span><span class="sxs-lookup"><span data-stu-id="1832c-111">You can then add the **Microsoft Azure Libraries for Java** to your project:</span></span>

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

<span data-ttu-id="1832c-112">Přidejte následující `import` příkazů do horní části souboru Java:</span><span class="sxs-lookup"><span data-stu-id="1832c-112">Add the following `import` statements to the top of the Java file:</span></span>

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a><span data-ttu-id="1832c-113">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="1832c-113">Create a queue</span></span>
<span data-ttu-id="1832c-114">Operace správy front služby Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy.</span><span class="sxs-lookup"><span data-stu-id="1832c-114">Management operations for Service Bus queues can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="1832c-115">A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje tokenu SAS s oprávněními ke správě, a **ServiceBusContract** třída je jediný bod komunikace s Azure.</span><span class="sxs-lookup"><span data-stu-id="1832c-115">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions to manage it, and the **ServiceBusContract** class is the sole point of communication with Azure.</span></span>

<span data-ttu-id="1832c-116">**ServiceBusService** třída poskytuje metody pro vytvoření, výčet a odstranění front.</span><span class="sxs-lookup"><span data-stu-id="1832c-116">The **ServiceBusService** class provides methods to create, enumerate, and delete queues.</span></span> <span data-ttu-id="1832c-117">V příkladu níže znázorňuje způsob **ServiceBusService** objekt lze vytvořit frontu s názvem `TestQueue`, obor názvů s názvem `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="1832c-117">The example below shows how a **ServiceBusService** object can be used to create a queue named `TestQueue`, with a namespace named `HowToSample`:</span></span>

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

<span data-ttu-id="1832c-118">Způsoby na `QueueInfo` které umožňují vlastností fronty v úzkém ladit (například: Chcete-li hodnotu výchozí time to live (TTL) má být použita pro zprávy odeslané do fronty).</span><span class="sxs-lookup"><span data-stu-id="1832c-118">There are methods on `QueueInfo` that allow properties of the queue to be tuned (for example: to set the default time-to-live (TTL) value to be applied to messages sent to the queue).</span></span> <span data-ttu-id="1832c-119">Následující příklad ukazuje, jak vytvořit frontu s názvem `TestQueue` s maximální velikostí 5 GB:</span><span class="sxs-lookup"><span data-stu-id="1832c-119">The following example shows how to create a queue named `TestQueue` with a maximum size of 5GB:</span></span>

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

<span data-ttu-id="1832c-120">Všimněte si, že můžete použít `listQueues` metodu **ServiceBusContract** objekty, které chcete zkontrolovat, zda fronta se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="1832c-120">Note that you can use the `listQueues` method on **ServiceBusContract** objects to check if a queue with a specified name already exists within a service namespace.</span></span>

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="1832c-121">Zasílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="1832c-121">Send messages to a queue</span></span>
<span data-ttu-id="1832c-122">K odeslání zprávy do fronty Service Bus, vaše aplikace získává **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="1832c-122">To send a message to a Service Bus queue, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="1832c-123">Následující kód ukazuje, jak odeslat zprávu `TestQueue` vytvořili ve frontě `HowToSample` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="1832c-123">The following code shows how to send a message for the `TestQueue` queue previously created in the `HowToSample` namespace:</span></span>

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

<span data-ttu-id="1832c-124">Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="1832c-124">Messages sent to, and received from Service Bus queues are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="1832c-125">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník používaný pro udržení vlastních vlastností konkrétní aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="1832c-125">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties (such as [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) and [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="1832c-126">Aplikace může tělo zprávy nastavit podle předá jakýkoli serializovatelný objekt do konstruktoru objektu [BrokeredMessage][BrokeredMessage], a odpovídající serializátoru, který pak bude sloužit k serializaci objektu.</span><span class="sxs-lookup"><span data-stu-id="1832c-126">An application can set the body of the message by passing any serializable object into the constructor of the [BrokeredMessage][BrokeredMessage], and the appropriate serializer will then be used to serialize the object.</span></span> <span data-ttu-id="1832c-127">Alternativně můžete zadat **java. VSTUPNĚ-VÝSTUPNÍ OPERACE. Třídy InputStream** objektu.</span><span class="sxs-lookup"><span data-stu-id="1832c-127">Alternatively, you can provide a **java.IO.InputStream** object.</span></span>

<span data-ttu-id="1832c-128">Následující příklad ukazuje, jak odeslat pět zkušebních zpráv, které mají `TestQueue` **MessageSender** jsme získali v předchozím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="1832c-128">The following example demonstrates how to send five test messages to the `TestQueue` **MessageSender** we obtained in the previous code snippet:</span></span>

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

<span data-ttu-id="1832c-129">Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="1832c-129">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="1832c-130">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="1832c-130">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="1832c-131">Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je.</span><span class="sxs-lookup"><span data-stu-id="1832c-131">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="1832c-132">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="1832c-132">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="1832c-133">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="1832c-133">Receive messages from a queue</span></span>
<span data-ttu-id="1832c-134">Primární způsob pro příjem zpráv z fronty je použití **ServiceBusContract** objektu.</span><span class="sxs-lookup"><span data-stu-id="1832c-134">The primary way to receive messages from a queue is to use a **ServiceBusContract** object.</span></span> <span data-ttu-id="1832c-135">Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="1832c-135">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="1832c-136">Při použití **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí zprávu jako spotřebovávanou a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1832c-136">When using the **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="1832c-137">**ReceiveAndDelete** režimu (což je výchozí režim) je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="1832c-137">**ReceiveAndDelete** mode (which is the default mode) is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="1832c-138">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="1832c-138">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span>
<span data-ttu-id="1832c-139">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="1832c-139">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="1832c-140">V **PeekLock** režimu, zobrazí se změní na dvě fáze operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="1832c-140">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="1832c-141">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1832c-141">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="1832c-142">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání **odstranit** na přijatou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1832c-142">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **Delete** on the received message.</span></span> <span data-ttu-id="1832c-143">Když Service Bus uvidí **odstranit** volání, která se bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="1832c-143">When Service Bus sees the **Delete** call, it will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="1832c-144">Následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (není výchozí režim).</span><span class="sxs-lookup"><span data-stu-id="1832c-144">The following example demonstrates how messages can be received and processed using **PeekLock** mode (not the default mode).</span></span> <span data-ttu-id="1832c-145">Následující příklad nemá nekonečné smyčce a zpracuje zprávy, když dorazí do našich `TestQueue`:</span><span class="sxs-lookup"><span data-stu-id="1832c-145">The example below does an infinite loop and processes messages as they arrive into our `TestQueue`:</span></span>

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
            // Display the queue message.
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
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="1832c-146">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="1832c-146">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="1832c-147">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="1832c-147">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="1832c-148">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat **unlockMessage** metoda na přijatou zprávu (místo **deleteMessage** metoda).</span><span class="sxs-lookup"><span data-stu-id="1832c-148">If a receiver application is unable to process the message for some reason, then it can call the **unlockMessage** method on the received message (instead of the **deleteMessage** method).</span></span> <span data-ttu-id="1832c-149">To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, buďto stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="1832c-149">This causes Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="1832c-150">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak Service Bus zprávu automaticky odemkne a ji zpřístupní k přijetí.</span><span class="sxs-lookup"><span data-stu-id="1832c-150">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="1832c-151">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než **deleteMessage** požadavku a potom zprávy je víckrát do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="1832c-151">In the event that the application crashes after processing the message but before the **deleteMessage** request is issued, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="1832c-152">To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1832c-152">This is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="1832c-153">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="1832c-153">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="1832c-154">To se často opírá **getMessageId** metoda zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="1832c-154">This is often achieved using the **getMessageId** method of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1832c-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1832c-155">Next Steps</span></span>
<span data-ttu-id="1832c-156">Teď, když jste se naučili základy front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="1832c-156">Now that you've learned the basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="1832c-157">Další informace naleznete ve [Středisku pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="1832c-157">For more information, see the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
