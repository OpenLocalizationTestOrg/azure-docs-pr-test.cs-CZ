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
# <a name="how-toouse-service-bus-queues-with-java"></a>Jak toouse Service Bus fronty s Javou
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak toouse fronty Service Bus. Hello ukázky jsou napsané v jazyce Java a používají hello [Azure SDK pro jazyk Java][Azure SDK for Java]. Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace toouse Service Bus
Ujistěte se, že máte nainstalovanou hello [Azure SDK pro jazyk Java] [ Azure SDK for Java] před vytvořením této ukázce. Pokud používáte Eclipse, můžete nainstalovat hello [nástrojů Azure pro Eclipse] [ Azure Toolkit for Eclipse] zahrnující hello Azure SDK pro jazyk Java. Poté můžete přidat hello **knihovny Microsoft Azure Libraries for Java** tooyour projektu:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Přidejte následující hello `import` toohello příkazy na začátek souboru Java hello:

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Vytvoření fronty
Operace správy front služby Service Bus je možné provádět prostřednictvím **ServiceBusContract** třídy. A **ServiceBusContract** objektu je vytvořený pomocí odpovídající konfiguraci, který zapouzdřuje token SAS s oprávněními toomanage a hello **ServiceBusContract** třída je jediný bod hello komunikace s Azure.

Hello **ServiceBusService** třída poskytuje metody toocreate, výčet a odstranění front. Hello Příklad dole ukazuje, jak **ServiceBusService** objekt může být použité toocreate frontu s názvem `TestQueue`, obor názvů s názvem `HowToSample`:

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

Způsoby na `QueueInfo` které umožňují vlastnosti toobe fronty hello přizpůsobená (například: tooset hello výchozí time to live (TTL) hodnotu toobe použít toomessages odeslané toohello fronty). Hello následující příklad ukazuje, jak toocreate frontu s názvem `TestQueue` s maximální velikostí 5 GB:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Všimněte si, které můžete použít hello `listQueues` metodu **ServiceBusContract** objekty toocheck Pokud fronta se zadaným názvem již existuje v rámci oboru názvů služby.

## <a name="send-messages-tooa-queue"></a>Odesílat zprávy fronty tooa
Získá toosend fronty Service Bus zprávu tooa, aplikace **ServiceBusContract** objektu. Následující kód ukazuje, jak Hello toosend zprávu o hello `TestQueue` fronty předtím vytvořili v hello `HowToSample` obor názvů:

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

Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy. [BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete nastavit hello těla zprávy hello tak předá jakýkoli serializovatelný objekt do konstruktoru hello hello [BrokeredMessage][BrokeredMessage], a odpovídající serializátor hello se pak použije objekt tooserialize hello. Alternativně můžete zadat **java. VSTUPNĚ-VÝSTUPNÍ OPERACE. Třídy InputStream** objektu.

Hello následující příklad ukazuje, jak testovací toosend pět zprávy toothe `TestQueue` **MessageSender** jsme získali v předchozím fragmentu kódu hello:

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

Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md). Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB. Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.

## <a name="receive-messages-from-a-queue"></a>Příjem zpráv z fronty
Hello primární způsob tooreceive zprávy z fronty je toouse **ServiceBusContract** objektu. Přijaté zprávy můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**.

Při použití hello **ReceiveAndDelete** režimu přijímat je jednorázová operace – to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace. **ReceiveAndDelete** režimu (což je výchozí režim hello) je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.
Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

V **PeekLock** režimu, zobrazí se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním **odstranit** na uvítací přijal zprávu. Když Service Bus uvidí hello **odstranit** volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.

Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí **PeekLock** režimu (ne hello výchozí režim). Hello příklad nepodporuje nekonečné smyčce a zpracuje zprávy, když dorazí do našich `TestQueue`:

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Jak toohandle aplikace spadne a nečitelných zpráv
Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello **unlockMessage** na hello přijal zprávu (místo hello **deleteMessage** metoda). Tato příčiny hello toounlock sběrnice zpráv ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.

Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud aplikace hello selže tooprocess hello zprávy vyprší časový limit uzamčení (například pokud hello aplikace spadne), pak Service Bus automaticky odemkne uvítací zprávu a je k dispozici toobe přijetí.

V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **deleteMessage** požadavku a potom uvítací zprávu je víckrát toohello aplikace odešle znovu. To se často označuje jako *zpracování nejméně jednou*; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy. To se často opírá hello **getMessageId** metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.

Další informace najdete v tématu hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
