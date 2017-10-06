---
title: "aaaCreate aplikace, které používají témata a předplatné Azure Service Bus | Microsoft Docs"
description: "Úvod toohello publikování a odběru na možnosti, které nabízí témat a odběrů Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a48fc9b0-b7b0-464e-8187-a517bf8b4eb4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: sethm
ms.openlocfilehash: f6d7de46ace7bd5b49de612db213ced789308d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Vytvoření aplikace, které používají témata a odběry Service Bus
Azure Service Bus podporuje sadu založené na cloudu, orientovaný na zprávy middleware technologie včetně služby Řízení front zpráv spolehlivé a trvanlivé publikování a přihlášení k odběru zasílání zpráv. Tento článek vychází hello informací uvedených v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) a nabízí úvod toohello publikování a přihlášení k odběru funkce nabízené sítěmi témat sběrnice Service Bus.

## <a name="evolving-retail-scenario"></a>Měnící prodejní scénář
Tento článek pokračuje scénář prodejní hello používá v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md). Odvolat, že data prodeje z jednotlivých terminály bodu prodej (POS) musí být systému pro správu inventáře směrované tooan se používá tento toodetermine dat při stock má toobe doplnit. Každý terminál POS sestavy jeho data prodeje odesláním zprávy toohello **DataCollectionQueue** fronty, kde zůstávají, dokud jsou přijímány hello systém pro správu inventáře, jak je vidět tady:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

tooevolve tento scénář, nový požadavek byl přidán toohello systému: vlastníka obchodu hello chce mít toomonitor toobe jak hello úložiště provádí v reálném čase.

tooaddress tento požadavek hello systému musí "klepněte na" off hello prodeje datového proudu. Každá zpráva odeslaná pomocí toobe terminály POS hello odeslat systém pro správu inventáře toohello jako před stále chceme, ale chceme jinou kopii každou zprávu, že budeme moci použít toopresent hello zobrazení toohello úložiště u vlastníka řídicího panelu.

V každé situaci, jako je tato, ve které budete potřebovat každou zprávu toobe spotřebovávají více stranami, můžete použít Service Bus *témata*. Témata poskytují vzor publikování/přihlášení ve kterém je každá publikovaná zpráva provedené dostupné tooone nebo více odběrů zaregistrován v tématu hello. Naopak v případě front každou zprávu přijme jeden spotřebitel.

Jsou zprávy odesílány tooa tématu hello stejným způsobem, jak se odešlou tooa fronty. Však nejsou přijaty zprávy z tématu hello přímo. příjmu z předplatného. Téma tooa předplatné si lze představit jako virtuální frontě, která obdrží kopii zprávy hello, které jsou odesílány toothat tématu. Přijímání zpráv z odběru hello stejný způsobem, jak příjmu z fronty.

Návratem toohello prodejní scénář, hello fronty je nahrazena téma a přidat předplatné, které součásti hello inventáře správy systému můžete použít. Hello systému nyní vypadat takto:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Konfigurace Hello zde provádí stejně jako toohello předchozí návrhu na základě fronty. To znamená, zprávy odeslané toohello tématu jsou směrované toohello **inventáře** předplatného, ze které hello **systém pro správu inventáře** je využívá.

V pořadí toosupport hello řídicí panel správy vytvoříme druhého předplatného na hello tématu, jak je vidět tady:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Pomocí této konfigurace se provádí každou zprávu z terminály hello POS hello k dispozici tooboth **řídicí panel** a **inventáře** odběry.

## <a name="show-me-hello-code"></a>Zobrazit mi hello kódu
článek Hello [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) popisuje, jak toosign k účtu Azure a vytvořit obor názvů služby. Hello nejjednodušší způsob, jak tooreference Service Bus závislostí je tooinstall hello Service Bus [balíček Nuget](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Můžete také získat hello knihovny Service Bus jako součást hello Azure SDK. Hello stažení je k dispozici na hello [stránky pro stažení sady Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-hello-topic-and-subscriptions"></a>Vytvoření tématu hello a odběry
Operace správy pro Service Bus entit pro zasílání zpráv (fronty a publikování a přihlášení k odběru témata) se provádí prostřednictvím hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třídy. Příslušná pověření jsou potřeba v pořadí toocreate **NamespaceManager** instance pro konkrétní obor názvů. Service Bus používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) model zabezpečení založený na. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracení některé známé poskytovatele tokenů. Použijeme [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) přihlašovací údaje SAS metoda toohold hello. Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) instance je pak vytvořený pomocí hello bázové adresy oboru názvů Service Bus hello a zprostředkovatele tokenu hello.

Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třída poskytuje metody toocreate, výčet a odstranění entit pro zasílání zpráv. Hello kód, který zobrazí se zde zobrazí jak hello **NamespaceManager** instance je vytvořený a slouží toocreate hello **DataCollectionTopic** tématu.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Všimněte si, že jsou přetížení hello [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metoda, která umožňují tooset vlastnosti tématu hello. Například můžete nastavit hello výchozí time to live (TTL) hodnotu pro zprávy odeslané toohello tématu. Dál přidejte hello **inventáře** a **řídicí panel** odběry.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-toohello-topic"></a>Odeslání zprávy toohello tématu
Pro spuštění operace na Service Bus entity; například odesílání a přijímání zpráv, aplikace musíte nejdřív vytvořit [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory) objektu. Podobně jako toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třídy, hello **MessagingFactory** instance je vytvořený z hello bázové adresy oboru názvů služby hello a zprostředkovatele tokenu hello.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Zprávy odeslané tooand přijal od témat sběrnice Service Bus jsou instance třídy hello [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) třídy. Tato třída obsahuje sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník, který je použité toohold vlastnosti aplikace a tělo s libovolnými aplikačními daty. Aplikace může tělo hello nastavit tak předávání jakýkoli serializovatelný objekt (hello následující příklad předá **SalesData** objekt, který představuje data prodeje hello z terminálu hello POS), který budou používat hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello objektu. Alternativně [datového proudu](https://msdn.microsoft.com/library/system.io.stream.aspx) může být zadán objekt.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Hello nejjednodušší způsob, jak toosend zprávy toohello téma je toouse [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objekt přímo z hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Podobně jako fronty toousing, tooreceive zprávy z odběru můžete použít [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt, který vytvoříte přímo z hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) pomocí [ CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Můžete použít některou z hello dva různé režimy přijímat (**ReceiveAndDelete** a **PeekLock**), jak je popsáno v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md).

Všimněte si, že při vytváření **MessageReceiver** odběrů, hello *entityPath* parametr má podobu hello `topicPath/subscriptions/subscriptionName`. Proto toocreate **MessageReceiver** pro hello **inventáře** předplatné hello **DataCollectionTopic** tématu *entityPath*musí být nastaven příliš`DataCollectionTopic/subscriptions/Inventory`. Hello kódu vypadá takto:

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Filtrů odběrů
Pokud v tomto scénáři jsou vytvářeny všech zpráv odeslaných toohello tématu k dispozici tooall zaregistrované předplatné. Hello zde klíče frázi je "k dispozici." Při odběry služby Service Bus najdete v části všech zpráv odeslaných toohello téma, můžete zkopírovat jenom podmnožinu těchto fronty zpráv toohello virtuální odběru. To se provádí pomocí předplatného *filtry*. Při vytváření odběru zadáte výraz filtru v podobě hello predikátu SQL92 styl, který pracuje přes hello vlastnosti uvítací zprávu, jak hello vlastnosti systému (například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) a aplikace hello vlastnosti, jako například **StoreName** v předchozím příkladu hello.

Vyvíjejí tooillustrate hello scénář tím, druhý úložiště je toobe přidané tooour prodejní scénář. Prodejní data ze všech terminály hello POS z obou úložiště ještě systém pro správu inventáře toohello centralizované toobe směrovat, ale manažera úložiště pomocí nástroje hello řídicí panel je jen zájem o hello výkon toto úložiště. Pomocí filtrování tooachieve toto předplatné. Všimněte si, že když terminály hello POS publikování zpráv, nastavují hello **StoreName** vlastnosti aplikace na uvítací zprávu. Zadána dvě úložiště, například **Redmond** a **Praha**, terminály hello POS v hello Redmond ukládání razítko jejich data prodeje zprávy s **StoreName** rovnat příliš **Redmond**, zatímco použití terminály POS ukládání hello Praha **StoreName** rovnat příliš**Praha**. Správce úložiště Hello hello Redmond uložit jenom chce toosee data z jeho terminály POS. Hello systému vypadat takto:

![Služby Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

tooset až tento směrování, vytvoříte hello **řídicí panel** předplatné následujícím způsobem:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Pomocí této [filtr předplatných](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), jen zprávy, které mají hello **StoreName** vlastností nastavenou příliš**Redmond** bude zkopírovaný toohello virtuální fronty pro hello **Řídicí panel** předplatné. Je mnohem víc toosubscription filtrování, ale. Aplikace mohou mít více pravidel filtru podle předplatného v přidání toohello možnost toomodify hello vlastnosti zprávy, jak předává virtuální fronty odběru tooa.

## <a name="summary"></a>Souhrn
Všechny hello důvodů toouse služby Řízení front popsané v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) vztahovat tootopics, konkrétně:

* Časové oddělení – zpráva producenti a spotřebitelé nemají toobe online na hello stejnou dobu.
* Vyrovnávání zátěže – vrcholů v zatížení jsou vyhlazené v tématu hello povolení spotřebitelskou aplikací toobe zřízené pro průměrnou zátěž místo zátěž ve špičce.
* Vyrovnávání zatížení – podobně jako fronty tooa, může mít několik konkurenčních spotřebitelů naslouchání v rámci jednoho předplatného s každou zprávu předáno tooonly jeden hello příjemců, a tím Vyrovnávání zatížení.
* Volné párování – můžete rozvíjet hello zasílání zpráv na úrovni sítě bez ovlivnění stávající koncové body; například přidáním odběrů nebo změna tooallow tématu tooa filtry pro nové spotřebitele.

## <a name="next-steps"></a>Další kroky

V tématu [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) informace o způsobu toouse fronty ve scénáři prodejní hello POS.

