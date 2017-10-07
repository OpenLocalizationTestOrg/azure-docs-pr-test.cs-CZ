---
title: "fronty nedoručených sběrnice aaaService | Microsoft Docs"
description: "Přehled fronty nedoručených zpráv Azure Service Bus"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: 1638272085b8a3a59e8814f6f943caee35a2bfdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Přehled fronty nedoručených zpráv služby Service Bus

Fronty služby Service Bus a odběry tématu poskytují sekundární dílčí fronty, názvem *frontu nedoručených zpráv* (DLQ se). fronty nedoručených zpráv Hello není nutné explicitně vytvořit toobe a nemůže být odstraněný nebo jinak spravované nezávisle hello hlavní entity.

Tento článek popisuje fronty nedoručených zpráv v Azure Service Bus. Velká část hello diskusní je zobrazená ve hello [fronty nedoručených zpráv ukázka](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) na Githubu.
 
## <a name="hello-dead-letter-queue"></a>fronty nedoručených zpráv Hello

účelem Hello fronty nedoručených zpráv hello je toohold zprávy, které nelze doručit tooany příjemce nebo zprávy, které nebylo možné zpracovat. Zprávy můžete pak hello DLQ se odeberou a prověřovány. Aplikace může pomocí operátoru opravit problémy a znovu odeslat uvítací zprávu, protokolovat hello fakt, že došlo k chybě a proveďte opravné akce. 

Z rozhraní API a protokol perspektivy, hello DLQ se s tím rozdílem, že zprávy lze odeslat pouze prostřednictvím hello nedoručených zpráv gesto hello nadřazené entity je většinou podobné tooany jiné fronty. Kromě toho není dodržen time to live, a nemůžete nedoručených zpráv ze DLQ se. fronty nedoručených zpráv Hello plně podporuje funkce Náhled zámku doručení a transakční operace.

Všimněte si, že žádné automatické čištění hello DLQ se. Zprávy zůstávají v hello DLQ se, dokud je explicitně načíst z hello DLQ se a volání [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) na hello nedoručených zpráv.

## <a name="moving-messages-toohello-dlq"></a>Přesunutí zprávy toohello DLQ se

Existuje několik aktivity v Service Bus, které způsobí tooget zpráv nabídnutých toohello DLQ se z v rámci hello zasílání zpráv modul sám sebe. Aplikace můžete také explicitně přesunout zprávy toohello DLQ se. 

Získá během pohybu zprostředkovatelem hello uvítací zprávu, dvě vlastnosti se přidají toohello zprávu jako zprostředkovatel hello volá jeho vnitřní verzi hello [nedoručených zpráv](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_) metoda na uvítací zprávu: `DeadLetterReason` a `DeadLetterErrorDescription`.

Aplikace můžete definovat vlastní kódy pro hello `DeadLetterReason` vlastnost, ale hello systému nastaví hello následující hodnoty.

| Podmínka | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Vždy |HeaderSizeExceeded |byla překročena kvóta Hello velikosti pro tento datový proud. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing a SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |došlo k výjimce. GetType(). Jméno |došlo k výjimce. Zpráva |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |uvítací zprávu platnost a byl mrtvých lettered. |
| SubscriptionDescription.RequiresSession |Id relace má hodnotu null. |Relace povoleno entity neumožňuje zprávu, jejíž identifikátor relace má hodnotu null. |
| ! fronty nedoručených zpráv |MaxTransferHopCountExceeded |Hodnotu Null |
| Aplikace explicitní mrtvých písmem. |Zadaná aplikace |Zadaná aplikace |

## <a name="exceeding-maxdeliverycount"></a>Překročení MaxDeliveryCount
Fronty a odběry mají [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) a [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount) vlastnost v uvedeném pořadí; hello výchozí hodnota je 10. Vždy, když byla doručena zpráva pod zámek ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), ale byl buď explicitně opuštění nebo hello zámku vypršela platnost, uvítací zprávu [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) je zvýší. Když [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) překračuje [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount), uvítací zprávu je přesunutý toohello DLQ se, zadání hello `MaxDeliveryCountExceeded` kód důvodu.

Toto chování se nedá zakázat, ale můžete nastavit [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) tooa velmi velký počet.

## <a name="exceeding-timetolive"></a>Překročení TimeToLive
Když hello [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) nebo [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) vlastnost je nastavena příliš**true** (výchozí hodnota hello je **false**), kterým vyprší platnost všechny zprávy jsou přesunutý toohello DLQ se, zadání hello `TTLExpiredException` kód důvodu.

Upozorňujeme, že jsou vyprázdněny, pouze zprávy s vypršenou platností a proto přesunout toohello DLQ se po alespoň jeden příjemce active stahování na hlavní fronty hello nebo předplatného; Toto chování je záměrné.

## <a name="errors-while-processing-subscription-rules"></a>Při zpracování pravidel odběru došlo k chybám
Když hello [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) vlastnost je povolená pro předplatné, všechny chyby, ke kterým dojde během předplatného SQL filtru pravidlo provádí zachyceny v hello DLQ se společně s hello poškozený zprávy.

## <a name="application-level-dead-lettering"></a>Zpráv lettering úrovni aplikace
Přidání toohello poskytované systémem zpráv lettering funkcí můžete použít aplikace hello DLQ se tooexplicitly odmítněte nepřijatelné zprávy. To může zahrnovat zprávy, které nelze správně zpracovat z důvodu tooany řazení systémový problém, zprávy, které mají poškozených datových částí nebo zprávy, které se ověření nezdaří, pokud se používá schéma některé zprávy úroveň zabezpečení.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Zpráv lettering ve scénářích ForwardTo nebo SendVia

Budou zprávy fronty nedoručených zpráv odeslaných toohello přenos pod hello následující podmínky:

- Zpráva projdou více než 3 fronty a témata, která jsou [zřetězen dohromady](service-bus-auto-forwarding.md).
- Hello cílová fronta nebo téma je deaktivované nebo odstraněné.
- Hello cílová fronta nebo téma překračuje hello entity maximální velikost.

tooretrieve tyto zprávy lettered zpráv můžete vytvořit pomocí hello příjemce [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_) metody nástrojů.

## <a name="example"></a>Příklad
Následující fragment kódu Hello vytvoří příjemce zprávy. V hello zobrazí odkaz na hlavní fronty hello, hello kód načte uvítací zprávu s [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), který požádá hello zprostředkovatele tooinstantly návratový jakékoli zprávy snadno dostupné nebo tooreturn s žádný výsledek. Pokud hello kód přijme nějakou zprávu, se okamžitě opustí, což zvýší hello `DeliveryCount`. Jakmile hello systém přesune hello zpráva toohello DLQ se, hlavní fronty hello je prázdná a hello ukončí smyčky, jako [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) vrátí **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Další kroky
Viz následující články pro další informace o fronty Service Bus hello:

* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Azure Service Bus a fronty fronty porovnání](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

