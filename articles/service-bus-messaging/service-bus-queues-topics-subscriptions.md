---
title: "aaaOverview fronty, témata a odběry pro zasílání zpráv Azure Service Bus | Microsoft Docs"
description: "Přehled služby Service Bus entity pro zasílání zpráv."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 73135d2658e341c14dbb114ab938faed91578ff1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Fronty, témata a odběry služby Service Bus

Microsoft Azure Service Bus podporuje sadu založené na cloudu, orientovaný na zprávy middleware technologie včetně služby Řízení front zpráv spolehlivé a trvanlivé publikování a přihlášení k odběru zasílání zpráv. Tyto funkce "zprostředkovaného" přenosu zpráv může považovat za odpojeného zasílání zpráv funkce, které podpora publikování a odběru, dočasné oddělení a vyvažování zátěže scénáře s využitím infrastruktury zasílání zpráv Service Bus hello. Oddělená komunikace má mnoho výhod – klienti a servery se například můžou spojit podle potřeby a provádět své operace asynchronním způsobem.

Hello entity zasílání zpráv, které tvoří jádro hello Dobrý den, zasílání zpráv možnosti v Service Bus jsou fronty, témata a odběry a pravidla nebo akce.

## <a name="queues"></a>Fronty

Fronty nabízejí *First In, Out první* tooone doručování zpráv (metodou FIFO) nebo další konkurenčních spotřebitelů. To znamená že zprávy jsou obvykle očekávané toobe přijme a zpracuje hello příjemci v hello pořadí, ve kterém byly přidány toohello fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy. Klíčovou výhodou použití front je tooachieve "časové oddělení" součásti aplikace. Jinými slovy, hello producenti (odesílatelé) a spotřebitelé (příjemci) nemusí toobe odesílání a příjem zpráv v hello stejné času, protože zprávy jsou bezpečně uložené ve frontě hello. Kromě toho hello producent neobsahovala toowait na odpověď od příjemce hello v pořadí toocontinue tooprocess a odesílání zpráv.

Výhodou je "zatížení vyrovnávání,", který umožňuje producenti a spotřebitelé toosend a přijímat zprávy různými rychlostmi. V mnoha aplikacích zatížení systému hello se liší v čase; ale hello zpracování čas potřebný pro jednotlivé jednotky práce je obvykle stálá. Propojovací producenti a spotřebitelé zpráv s frontou znamená, že tento hello použití jenom aplikace nemá toobe zřízené toobe možné toohandle průměrnou zátěž místo zátěž ve špičce. Hello hloubka fronty hello s měnící hello příchozí zátěží se mění. To znamená přímou úsporu nákladů s ohledem toohello množství infrastruktury požadované tooservice hello aplikace zatížení. Jako hello zátěž zvyšuje, další pracovních procesů může být přidané tooread z fronty hello. Každou zprávu zpracovává jenom jeden hello pracovních procesů. Kromě toho tato Vyrovnávání zatížení založené na operaci pull umožňuje optimální využívání pracovních počítačů hello i v případě hello pracovní počítače liší s ohledem tooprocessing výkonu, jak se bude načítat zprávy na svou vlastním maximální rychlostí. Toto chování se často říká vzor "neslučitelných příjemce" hello.

Pomocí fronty toointermediate mezi producenti a spotřebitelé zpráv poskytuje vyplývajících volné párování mezi součástmi hello. Protože producenti a spotřebitelé nemají informace o sobě navzájem, bez nutnosti nijak neprojeví na hello producent lze upgradovat příjemce.

Vytvoření fronty je několika krocích. Provádět operace správy pro Service Bus (fronty a témata) entity prostřednictvím hello zasílání zpráv [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třídy, která je vytvořená zadáním hello bázové adresy hello Service Bus obor názvů a hello přihlašovací údaje uživatele. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) poskytuje metody toocreate, výčet a odstranění entit přenosu zpráv. Po vytvoření [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) objekt objektu hello SAS název a klíč a správu oboru názvů služby, můžete použít hello [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoda toocreate hello fronty. Například:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Potom můžete vytvořit objekt fronty a objekt zasílání zpráv s hello identifikátor URI služby Service Bus jako argument. Například:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Potom může odeslat toohello fronty zpráv. Například, pokud máte seznam zprostředkovaných zpráv názvem `MessageList`, podobně jako následující toohello se zobrazí kód hello:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Potom zobrazí zprávy z fronty hello následujícím způsobem:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

V hello [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) přijímat hello režimu, je jednorázová operace; to znamená, když Service Bus přijme požadavek hello, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace. **ReceiveAndDelete** režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, které hello aplikace může tolerovat hello události selhání se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Vzhledem k tomu, že Service Bus označí uvítací zprávu jako spotřebovávanou, když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.

V [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) přijímat hello režimu, operace se stane dvoufázová, proto je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek hello, vyhledá hello další zprávy toobe využívat, uzamkne ji tooprevent dalšími uživateli, příjem a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) na uvítací přijal zprávu. Když Service Bus uvidí hello **Complete** volání, která se označuje uvítací zprávu jako spotřebovávanou.

Pokud aplikace hello nedokáže tooprocess hello zprávy z nějakého důvodu, může zavolat hello [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) na hello přijal zprávu (místo [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). To umožňuje Service Bus toounlock uvítací zprávu a nastavit jej jako dostupné toobe přijetí, buď hello příjemce stejné nebo jiné konkurence mezi spotřebiteli. Za druhé existuje je přidružen vypršení časového limitu zámku hello a pokud aplikace hello selže tooprocess hello zprávy vyprší časový limit uzamčení hello (například pokud hello aplikace spadne), pak Service Bus odemkne uvítací zprávu a je k dispozici toobe přijetí (v podstatě provádění [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) operace ve výchozím nastavení).

Všimněte si, že v hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello **Complete** požadavku, uvítací zprávu je víckrát toohello aplikace odešle znovu. To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou. Ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, pak v hello duplikáty toodetect aplikace, které lze dosáhnout na základě hello je vyžadován další logiku **MessageId** vlastnost uvítací zprávu, která zůstává Konstanta mezi pokusy o doručení. To se označuje jako *právě jednou* zpracování.

## <a name="topics-and-subscriptions"></a>Témata a předplatná
V kontrastu tooqueues, ve kterém je každá zpráva zpracuje jeden spotřebitel, *témata* a *odběry* poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor. Užitečné pro škálování toovery velký počet příjemců, každá publikovaná zpráva se provádí k dispozici tooeach odběru registrovanému pro téma hello. Zprávy jsou odesílány tooa tématu a doručené tooone nebo více přidružených odběrů, v závislosti na pravidla filtru, které lze nastavit na základě za předplatné. odběry Hello můžete použít další filtry toorestrict hello zprávy, že chcete tooreceive. Zprávy jsou odesílány tooa téma v hello stejný způsobem se odešlou tooa fronty, ale nejsou přijaty zprávy z tématu hello přímo. Místo toho jsou přijímány z předplatného. Předplatné tématu se podobá virtuální frontě, která obdrží kopii zprávy hello, které jsou odesílány toohello tématu. Jsou přijaty zprávy z odběru stejně jako toohello způsob příjmu z fronty.

Prostřednictvím porovnání, hello funkce odesílání zpráv z fronty mapuje přímo tooa téma a jeho funkce příjem zpráv mapuje tooa předplatné. Kromě jiných věcí, to znamená, že odběry podporují stejné vzory dříve popisované v této části s ohledem tooqueues hello: konkurence mezi spotřebiteli, časové oddělení, Vyrovnávání zatížení a vyrovnávání zatížení.

Vytvoření tématu je podobný toocreating fronty, jak je znázorněno v příkladu hello v předchozí části hello. Vytvoření služby hello URI a pak použijte hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třída toocreate hello obor názvů klienta. Potom můžete vytvořit téma pomocí hello [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metoda. Například:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

V dalším kroku přidejte odběry podle potřeby:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Potom můžete vytvořit téma klienta. Například:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Pomocí odesílatele zprávy hello, můžete odesílat a přijímat zprávy tooand hello tématu, jak je uvedeno v předchozí části hello. Například:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Podobně jako tooqueues zprávy byly přijaty z předplatného pomocí [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) objektu místo [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objektu. Vytvoření klienta hello předplatné, předávání hello název tématu hello hello název hello odběru, a (volitelně) hello přijímat režimu jako parametry. Například s hello **inventáře** předplatného:

```csharp
// Create hello subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Pravidla a akce
V mnoha případech je nutné zpracovat zprávy, které mají určité charakteristické vlastnosti různými způsoby. tooenable, můžete nakonfigurovat odběry toofind zprávy, které mají požadovaných vlastností a pak provést určité úpravy toothose vlastnosti. Při všech zpráv odeslaných toohello tématu odběry služby Service Bus najdete kopírovat můžete pouze podmnožinu těchto fronty zpráv toohello virtuální odběru. To se provádí pomocí filtrů odběrů. Tyto změny se nazývají *akce filtru*. Při vytváření odběru zadáte výraz filtru, který funguje na hello vlastnosti uvítací zprávu, jak hello vlastnosti systému (například **popisek**) a vlastností vlastní aplikaci (například **StoreName**.) hello SQL výraz filtru se v takovém případě; volitelné bez výraz filtru SQL filtru akce definované na předplatné bude třeba provést na všechny zprávy hello pro toto předplatné.

Pomocí hello předchozí příklad, toofilter zprávy přicházející pouze z **Store1**, by vytvořit řídicí panel předplatné hello takto:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

K tomuto filtru předplatné v místě, jen zprávy, které mají hello `StoreName` vlastností nastavenou příliš`Store1` jsou zkopírované toohello virtuální fronty pro hello `Dashboard` předplatné.

Další informace o možných filtru hodnoty, najdete v dokumentaci hello hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) a [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) třídy. Viz také hello [zprostředkované zasílání zpráv: rozšířené filtry](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) a [tématu filtry](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) ukázky.

## <a name="next-steps"></a>Další kroky
Viz následující hello advanced témata pro další informace a příklady použití zasílání zpráv Service Bus.

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus](service-bus-brokered-tutorial-dotnet.md)
* [Zprostředkované zasílání zpráv kurz REST](service-bus-brokered-tutorial-rest.md)
* [Ukázka filtrů témat](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters)
* [Zprostředkované zasílání zpráv: Ukázka filtrů Advanced](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

