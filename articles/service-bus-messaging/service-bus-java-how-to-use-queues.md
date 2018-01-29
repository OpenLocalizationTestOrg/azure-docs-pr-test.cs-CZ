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
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Jak používat fronty Service Bus s Javou
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak používat fronty Service Bus. Ukázky jsou napsané v jazyce Java a použít [Azure SDK pro jazyk Java][Azure SDK for Java]. Pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pro použití služby Service Bus
Ujistěte se, že máte nainstalovanou [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce. Pokud používáte Eclipse, můžete nainstalovat [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] , který obsahuje sadu Azure SDK pro jazyk Java. Poté můžete přidat **knihovny Microsoft Azure Libraries for Java** do projektu:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Přidejte následující `import` příkazů do horní části souboru Java:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Vytvoření fronty
Operace správy front služby Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy. A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje tokenu SAS s oprávněními ke správě, a **ServiceBusContract** třída je jediný bod komunikace s Azure.

**ServiceBusService** třída poskytuje metody pro vytvoření, výčet a odstranění front. V příkladu níže znázorňuje způsob **ServiceBusService** objekt lze vytvořit frontu s názvem `TestQueue`, obor názvů s názvem `HowToSample`:

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

Způsoby na `QueueInfo` které umožňují vlastností fronty v úzkém ladit (například: Chcete-li hodnotu výchozí time to live (TTL) má být použita pro zprávy odeslané do fronty). Následující příklad ukazuje, jak vytvořit frontu s názvem `TestQueue` s maximální velikostí 5 GB:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Všimněte si, že můžete použít `listQueues` metodu **ServiceBusContract** objekty, které chcete zkontrolovat, zda fronta se zadaným názvem již existuje v rámci oboru názvů služby.

## <a name="send-messages-to-a-queue"></a>Zasílání zpráv do fronty
K odeslání zprávy do fronty Service Bus, vaše aplikace získává **ServiceBusContract** objektu. Následující kód ukazuje, jak odeslat zprávu `TestQueue` vytvořili ve frontě `HowToSample` obor názvů:

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

Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy. [BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník používaný pro udržení vlastních vlastností konkrétní aplikace a tělo s libovolnými aplikačními daty. Aplikace může tělo zprávy nastavit podle předá jakýkoli serializovatelný objekt do konstruktoru objektu [BrokeredMessage][BrokeredMessage], a odpovídající serializátoru, který pak bude sloužit k serializaci objektu. Alternativně můžete zadat **java. VSTUPNĚ-VÝSTUPNÍ OPERACE. Třídy InputStream** objektu.

Následující příklad ukazuje, jak odeslat pět zkušebních zpráv, které mají `TestQueue` **MessageSender** jsme získali v předchozím fragmentu kódu:

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

Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md). Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB. Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.

## <a name="receive-messages-from-a-queue"></a>Příjem zpráv z fronty
Primární způsob pro příjem zpráv z fronty je použití **ServiceBusContract** objektu. Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.

Při použití **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí zprávu jako spotřebovávanou a vrátí ji do aplikace. **ReceiveAndDelete** režimu (což je výchozí režim) je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat selhání se zpráva nezpracuje. Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.
Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.

V **PeekLock** režimu, zobrazí se změní na dvě fáze operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace. Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání **odstranit** na přijatou zprávu. Když Service Bus uvidí **odstranit** volání, která se bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.

Následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (není výchozí režim). Následující příklad nemá nekonečné smyčce a zpracuje zprávy, když dorazí do našich `TestQueue`:

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Zpracování pádů aplikace a nečitelných zpráv
Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat **unlockMessage** metoda na přijatou zprávu (místo **deleteMessage** metoda). To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, buďto stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak Service Bus zprávu automaticky odemkne a ji zpřístupní k přijetí.

V případě, že aplikace spadne po zpracování zprávy, ale předtím, než **deleteMessage** požadavku a potom zprávy je víckrát do aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu. Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy. To se často opírá **getMessageId** metoda zprávy, která zůstává konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.

Další informace naleznete ve [Středisku pro vývojáře Java](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
