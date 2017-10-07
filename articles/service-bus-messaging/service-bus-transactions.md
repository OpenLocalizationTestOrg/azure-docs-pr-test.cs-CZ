---
title: "aaaOverview zpracování transakcí v Azure Service Bus | Microsoft Docs"
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
ms.openlocfilehash: 5ed4d1fd3a089b8ebcd69a568f4ac863e753aecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a>Přehled služby Service Bus zpracování transakcí
Tento článek popisuje možnosti transakce hello Azure Service Bus. Velká část hello diskusní je zobrazená ve hello [jednotlivé transakce s ukázkou Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). Tento článek je omezená tooan přehled zpracování transakcí a hello *odesílání prostřednictvím* funkce v Service Bus, zatímco ukázka jednotlivé transakce hello je širší a složitější v oboru.

## <a name="transactions-in-service-bus"></a>Transakce v Service Bus
A [transakce](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) seskupí dva nebo více operací společně do *spuštění rozsah*. Vzhledem k povaze tuto transakci Ujistěte se, že všechny operace, které patří tooa danou skupinu operací úspěšné, nebo selže společně. V tomto ohledu transakce fungovat jako jednu jednotku, což je často označují tooas *nedělitelnost*. 

Service Bus je zprostředkovatele transakčních zpráv a zajistíte integritu transakcí pro všechny interní operace před jeho úložiště zpráv. Všechny přenosy zpráv v rámci služby Service Bus, jako je například přesunutí zprávy tooa [frontu nedoručených zpráv](service-bus-dead-letter-queues.md) nebo [automatické přesměrování](service-bus-auto-forwarding.md) zpráv mezi entitami, jsou transakcí. Jako takový Pokud Service Bus přijme zprávu, již bylo uložené a s názvem bez přípony s pořadovým číslem. Od toho všechny přenosy zprávy v rámci služby Service Bus jsou koordinované operace napříč entity a povede ani tooloss (zdroj úspěšné a cíle selže) nebo tooduplication (selže zdroj a cíl úspěšné) hello zprávy.

Service Bus podporuje seskupení operace u jedné entity přenosu zpráv (fronty, tématu, odběru) v rámci oboru hello transakce. Například můžete odeslat několik tooone fronty zpráv z v oboru transakce a hello pouze budou zprávy fronty potvrdit toohello protokolu při úspěšném dokončení transakce hello.

## <a name="operations-within-a-transaction-scope"></a>Operace v rámci oboru transakce
Hello operace, které lze provést v oboru transakce jsou následující:

* **[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: odeslat, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: dokončení CompleteAsync, Abandon, AbandonAsync, nedoručených zpráv, DeadletterAsync, odložení, DeferAsync, RenewLock, RenewLockAsync 

Zobrazí operace nejsou zahrnuté, protože se předpokládá, že aplikace hello získá zprávy pomocí hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu uvnitř některé přijímat smyčky nebo s [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) zpětného volání, a obor pouze poté otevře a transakce pro zpracování zprávy hello.

Hello dispozice uvítací zprávu (dokončení odložení abandon nedoručených zpráv) potom se provede v rámci oboru hello a závisí na, hello celkový výsledek hello transakce.

## <a name="transfers-and-send-via"></a>Přenosy a "odesílání prostřednictvím"
tooenable transakční předání dat z fronty tooa procesoru a potom tooanother fronty Service Bus podporuje *přenosy*. V rámci operace přenosu odesílatel nejprve odešle zprávu tooa "fronty přenosu" a fronty přenosu hello ihned přesouvá toohello hello zprávy určené cílové fronty pomocí stejné robustní přenosu implementace, která využívá funkce automatického předání hello hello na. uvítací zprávu se nikdy fronty přenosu potvrdit toohello protokolu tak, že se zobrazí u spotřebitelé fronty přenosu hello.

napájení Hello tuto funkci transakční vyvstává zřejmá při fronty přenosu hello, samotné hello zdroj vstupní zpráv hello odesílatele. Jinými slovy, Service Bus se můžou přenášet hello zpráva toohello cílové fronty "přes" hello fronty přenosu, při provádění úplná (nebo odložení, nebo nedoručených zpráv) operace na uvítací vstupní zprávu, všechny v rámci jedné atomické operace. 

### <a name="see-it-in-code"></a>Zobrazení v kódu
tooset se tyto přenosy, můžete vytvořit odesílatele zprávy, která je cílena hello cílové fronty prostřednictvím fronty přenosu hello. Také budete mít příjemce, který žádá o zprávy z této fronty stejné. Například:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Jednoduché transakce pak používá tyto prvky, jako hello následující ukázka:

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package hello result 

    msg.Complete(); // mark hello message as done
    sender.Send(newmsg); // forward hello result

    scope.Complete(); // declare hello transaction done
} 
```

## <a name="next-steps"></a>Další kroky

Viz následující články pro další informace o fronty Service Bus hello:

* [Řetězení entit služby Service Bus s automatické přesměrování](service-bus-auto-forwarding.md)
* [Ukázka automatické vpřed](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Jednotlivé transakce s ukázkou Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure Service Bus a fronty fronty porovnání](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [Jak toouse fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)

