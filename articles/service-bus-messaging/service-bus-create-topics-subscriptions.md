---
title: "Vytvoření aplikace, které používají témata a předplatné Azure Service Bus | Microsoft Docs"
description: "Úvod k publikování-přihlášení k odběru možnosti, které nabízí témat a odběrů Service Bus."
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
ms.openlocfilehash: eb01120ce9578f716e5381c107faa93f0b36e358
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Vytvoření aplikace, které používají témata a odběry Service Bus
Azure Service Bus podporuje sadu založené na cloudu, orientovaný na zprávy middleware technologie včetně služby Řízení front zpráv spolehlivé a trvanlivé publikování a přihlášení k odběru zasílání zpráv. Tento článek je založený na informacích uvedených v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) a nabízí úvod k publikování a přihlášení k odběru funkcím, které nabízí témat sběrnice Service Bus.

## <a name="evolving-retail-scenario"></a>Měnící prodejní scénář
Tento článek pokračuje prodejní scénář použitý v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md). Odvolat prodeje dat z jednotlivých bodu prodej (POS) terminály musejí směrovat na systém pro správu inventáře, který používá tato data k určení, kdy musí být doplněny stock. Každý terminál POS sestavy jeho data prodeje odesláním zprávy a pokuste se **DataCollectionQueue** fronty, kde zůstávají, dokud jsou přijímány systém pro správu inventáře, jak je vidět tady:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Vyvíjí tento scénář, byla do systému přidaný nový požadavek: vlastník úložiště chce moci sledovat, jak úložiště provádí v reálném čase.

Chcete-li vyřešit tento požadavek, systém musí "klepněte na" off prodeje datový proud. Každá zpráva odeslaná pomocí terminály POS k odeslání do systému správy inventáře jako před stále chceme, ale chceme jinou kopii každé zprávy, můžeme použít na zobrazení řídicího panelu pro vlastníka úložiště.

V každé situaci, jako je tato, ve které budete potřebovat každou zprávu zpracovat více stranami, můžete použít Service Bus *témata*. Témata poskytují se vzorem publikovat/odebírat, ve kterém je každá publikovaná zpráva k dispozici jeden nebo více odběrů registrovaným pro příslušné téma. Naopak v případě front každou zprávu přijme jeden spotřebitel.

Zprávy se posílají do tématu stejným způsobem, jako jsou odeslána do fronty. Však nejsou přijaty zprávy v tématu přímo. příjmu z předplatného. Si můžete představit předplatné téma jako virtuální frontě, která obdrží kopii zprávy, které se odesílají do tohoto tématu. Jsou přijaty zprávy z odběru stejným způsobem jako přijaté z fronty.

Po návratu do prodejní scénář, fronty je nahrazena téma a předplatné se přidá, které můžete použít systémové součásti pro správu inventáře. Systém se nyní vypadat takto:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Konfigurace zde funguje stejně jako předchozí návrhu na základě fronty. To znamená, jsou zprávy odeslané do tématu směrovány do **inventáře** předplatného, ze kterého **systém pro správu inventáře** je využívá.

Za účelem podpory řídicí panel správy, vytvoříme druhého předplatného na tématu, jak je vidět tady:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Pomocí této konfigurace je k dispozici na obě každou zprávu z terminály POS **řídicí panel** a **inventáře** odběry.

## <a name="show-me-the-code"></a>Zobrazit mi kód
Článek [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) popisuje, jak vytvořit obor názvů služby a zaregistrujte si účet Azure. Nejjednodušší způsob, jak odkazovat na závislosti služby Service Bus je instalace služby Service Bus [balíček Nuget](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Můžete také získat knihovny Service Bus v rámci sady Azure SDK. Je k dispozici ke stažení [stránky pro stažení sady Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Vytvoření tématu a odběry
Operace správy pro Service Bus entit pro zasílání zpráv (fronty a publikování a přihlášení k odběru témata) se provádí prostřednictvím [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třídy. Příslušná pověření jsou nutné k vytvoření **NamespaceManager** instance pro konkrétní obor názvů. Service Bus používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) model zabezpečení založený na. [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracení některé známé poskytovatele tokenů. Použijeme [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoda pro uložení pověření SAS. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) instance je pak vytvořený pomocí bázové adresy oboru názvů Service Bus a zprostředkovateli tokenu.

[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třída poskytuje metody pro vytvoření, výčet a odstranění entit přenosu zpráv. Kód, který zobrazí se zde zobrazí jak **NamespaceManager** je vytvořit a použít k vytvoření instance **DataCollectionTopic** tématu.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Všimněte si, že existují přetížení [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metoda, která vám umožní nastavit vlastnosti tématu. Můžete například nastavit výchozí time to live (TTL) hodnotě pro zprávy odeslané do tématu. Dál přidejte **inventáře** a **řídicí panel** odběry.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Odesílání zpráv do tématu
Pro spuštění operace na Service Bus entity; například odesílání a přijímání zpráv, aplikace musíte nejdřív vytvořit [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory) objektu. Podobně jako [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) třídy, **MessagingFactory** instance je vytvořený z bázové adresy oboru názvů služby a zprostředkovateli tokenu.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Zprávy odesílané do a přijetí od témat sběrnice Service Bus jsou instance [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) třídy. Tato třída obsahuje sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník používaný pro udržení vlastností aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete nastavit text tak předávání jakýkoli serializovatelný objekt (v následujícím příkladu předá v **SalesData** objekt, který představuje data z prodeje z terminálu POS), který bude používat [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) k serializaci objektu. Alternativně [datového proudu](https://msdn.microsoft.com/library/system.io.stream.aspx) může být zadán objekt.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Nejjednodušší způsob, jak zasílat zprávy do tématu je použití [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) k vytvoření [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objektu přímo z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Příjem zpráv z odběru
Podobně jako pomocí front, pro příjem zpráv z odběru můžete použít [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt, který vytvoříte přímo z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) pomocí [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Můžete použít jednu ze dvou různých režimech přijímat (**ReceiveAndDelete** a **PeekLock**), jak je popsáno v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md).

Všimněte si, že při vytváření **MessageReceiver** pro předplatné, *entityPath* parametr má formát `topicPath/subscriptions/subscriptionName`. Proto k vytvoření **MessageReceiver** pro **inventáře** předplatné **DataCollectionTopic** tématu *entityPath* musí být nastavena na `DataCollectionTopic/subscriptions/Inventory`. Kód zobrazí se následující:

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
Pokud v tomto scénáři všechny zprávy odeslané do tématu jsou k dispozici všechny odběry registrované. Klíče fráze je "k dispozici." Při odběry služby Service Bus zobrazit všechny zprávy odeslané do tématu, můžete zkopírovat jenom podmnožinu těchto zpráv do fronty virtuální odběru. To se provádí pomocí předplatného *filtry*. Při vytváření předplatného, můžete zadat výraz filtru ve formě predikát SQL92 styl, který funguje prostřednictvím vlastnosti zprávy, vlastnosti systému (například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) a vlastnosti aplikace, jako například **StoreName** v předchozím příkladu.

Scénář pro znázornění vyvíjejí, druhý úložiště je pro přidání do náš scénář prodejní. Prodejní data ze všech terminály POS z obou úložiště ještě směrovat na systém pro správu centralizované inventáře, ale manažera úložiště pomocí nástroje řídicí panel je jen zájem o výkon toto úložiště. Můžete uživatelům pomocí odběru filtrování dosáhnout. Všimněte si, že když terminály POS publikování zpráv, nastaveny **StoreName** vlastnosti aplikace na zprávu. Zadána dvě úložiště, například **Redmond** a **Praha**, terminály POS v úložišti Redmond razítka jejich data prodeje zprávy s **StoreName** rovna **Redmond**, zatímco použití terminály POS ukládat Praze **StoreName** rovna **Praha**. Správce obchodu úložiště Redmond pouze chce vidět data z jeho terminály POS. Systém vypadat takto:

![Služby Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Chcete-li nastavit tento směrování, musíte vytvořit **řídicí panel** předplatné následujícím způsobem:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Pomocí této [filtr předplatných](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), jen zprávy, které mají **StoreName** vlastnost nastavena na hodnotu **Redmond** se zkopírují do virtuální fronty pro **řídicí panel** předplatné. Je mnohem víc filtrování předplatné, ale. Aplikace mohou mít více pravidel filtru na jedno předplatné kromě schopnost jak předává do virtuální fronty odběru upravit vlastnosti zprávy.

## <a name="summary"></a>Souhrn
Všechny důvodů pro použití služby Řízení front popsané v [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) se rovněž vztahují na témata, konkrétně:

* Časové oddělení – producenti a spotřebitelé zpráv nemusí být online ve stejnou dobu.
* Vyrovnávání zátěže – vrcholů v zatížení jsou vyhlazené v tématu Povolení přijímajícím aplikacím zřídily pro průměrnou zátěž místo zátěž ve špičce.
* Vyrovnávání zatížení – podobně jako fronty, můžete mít několik konkurenčních spotřebitelů naslouchání v rámci jednoho předplatného s každou zprávu předávána pouze jeden příjemce, a tím Vyrovnávání zatížení.
* Volné párování – zasílání zpráv sítě můžete rozvíjet bez ovlivnění stávající koncové body; například přidáním odběrů nebo změna filtry do tématu povolit pro nové spotřebitele.

## <a name="next-steps"></a>Další kroky

V tématu [vytvářet aplikace, které používají fronty Service Bus](service-bus-create-queues.md) informace o tom, jak používat fronty ve scénáři prodejní POS.

