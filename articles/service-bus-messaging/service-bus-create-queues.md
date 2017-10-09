---
title: "aaaWrite aplikace, které používají fronty Azure Service Bus | Microsoft Docs"
description: "Jak toowrite jednoduchou aplikaci na základě fronty, která používá Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 754d91b3-1426-405e-84b4-fd36d65b114a
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 93b75902a06becd6e33e05298e09f5669d0e2aef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Vytváření aplikací používajících fronty Service Bus
Toto téma popisuje fronty Service Bus a ukazuje, jak toowrite jednoduchou aplikaci na základě fronty, která používá Service Bus.

Představte si třeba situaci z hello, world maloobchodních, ve kterém směruje prodeje dat z jednotlivých Point-of-Sale (POS) terminály musí být tooan inventáře správy systému, který používá hello data toodetermine, když má stock toobe doplnit. Toto řešení používá pro komunikaci hello mezi hello terminály a systém pro správu inventáře hello, zasílání zpráv Service Bus, jak ukazuje následující obrázek hello:

![Fronty služby Service Bus obrázku 1](./media/service-bus-create-queues/IC657161.gif)

Každý terminál POS sestavy jeho data prodeje odesláním zprávy toohello **DataCollectionQueue**. Tyto zprávy zůstanou v této frontě, dokud se načítají pomocí systému pro správu inventáře hello. Toto chování se často říká *asynchronní zasílání zpráv*, protože hello POS Terminálové nemá toowait na odpověď od hello inventáře správy systému toocontinue zpracování.

## <a name="why-queuing"></a>Proč služba Řízení front?
Předtím, než se podíváme na hello kód, který je požadovaný tooset si tuto aplikaci, zvažte hello výhody používání fronty v tomto scénáři místo nutnosti terminály hello POS komunikovat přímo (synchronně) toohello inventáře systému pro správu.

### <a name="temporal-decoupling"></a>Časové oddělení
S asynchronním vzorcem zasílání zpráv hello producenti a spotřebitelé nemají toobe online na hello stejnou dobu. Hello infrastruktury přenosu zpráv spolehlivě uloží zprávy, dokud spotřebitel hello je připraven tooreceive je. To znamená, že hello součásti hello distribuované aplikace můžou být odpojit; například za účelem údržby nebo z důvodu tooa součást havárií, aniž by to ovlivnilo hello celý systém. Kromě toho hello využívání aplikace může mít pouze toobe online v určitou dobu hello den. Například v tomto scénáři prodejní systém pro správu inventáře hello může mít pouze toocome online po skončení hello hello pracovního dne.

### <a name="load-leveling"></a>Vyrovnávání zátěže
V mnoha aplikacích zatížení systému se liší v čase, zatímco hello zpracování čas potřebný pro jednotlivé jednotky práce je obvykle stálá. Propojovací zpráva producenti a spotřebitelé s znamená fronty, které hello spotřebitelskou aplikací (pracovník hello) má jenom toobe zřizuje tooservice průměrem načíst místo zátěž ve špičce. Hello hloubka fronty hello se zvýší a kontrakt jako hello příchozí zátěží se liší. To znamená přímou úsporu nákladů s ohledem toohello množství infrastruktury požadované tooservice hello aplikace zatížení.

![Fronty služby Service Bus obrázku 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Vyrovnávání zatížení
Jako hello zátěž zvyšuje, další pracovních procesů může být přidané tooread z fronty pracovních procesů hello. Každou zprávu zpracovává jenom jeden hello pracovních procesů. Kromě toho tato Vyrovnávání zatížení založené na operaci pull umožňuje optimální využití hello pracovních počítačů i v případě hello pracovní počítače liší s ohledem tooprocessing výkonu, jak se bude načítat zprávy na svou vlastním maximální rychlostí. Toto chování se často říká hello konkurence mezi spotřebiteli vzor.

![Obrázek 3 fronty Service Bus](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Volné párování
Pomocí toointermediate služby Řízení front zpráv mezi producenti a spotřebitelé zpráv poskytuje vnitřní volné párování mezi součástmi hello. Protože producenti a spotřebitelé nemají informace o sobě navzájem, bez nutnosti nijak neprojeví na hello producent lze upgradovat příjemce. Kromě toho hello zasílání zpráv topologie můžete rozvíjet bez ovlivnění hello stávající koncové body. Probereme to více při mluvíme o publikování a přihlášení k odběru.

## <a name="show-me-hello-code"></a>Zobrazit mi hello kódu
Následující části ukazuje, jak Hello toouse Service Bus toobuild této aplikace.

### <a name="sign-up-for-an-azure-account"></a>Zaregistrujte si účet Azure
Budete potřebovat účet Azure v pořadí toostart práce s Service Bus. Pokud není již nemáte, můžete zaregistrovat bezplatný účet [zde](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Vytvoření oboru názvů
Jakmile máte předplatné, můžete [vytvořit obor názvů služby](service-bus-create-namespace-portal.md). Každý obor názvů funguje jako kontejner oboru pro sadu entit služby Service Bus. Zadejte jedinečný název váš nový obor názvů mezi všechny účty služby Service Bus. 

### <a name="install-hello-nuget-package"></a>Nainstalujte balíček NuGet hello
obor názvů sběrnice hello toouse, aplikace musí odkazovat hello sestavení Service Bus, konkrétně Microsoft.ServiceBus.dll. Můžete najít toto sestavení součástí hello Microsoft Azure SDK a stažení hello je k dispozici na hello [stránky pro stažení sady Azure SDK](https://azure.microsoft.com/downloads/). Ale hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je hello nejjednodušší způsob, jak tooget hello rozhraní API služby Service Bus a tooconfigure aplikaci se všemi závislostmi služby Service Bus hello služby.

### <a name="create-hello-queue"></a>Vytvořit frontu hello
Operace správy pro Service Bus entit pro zasílání zpráv (fronty a publikování a přihlášení k odběru témata) se provádí prostřednictvím hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třídy. Service Bus používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) model zabezpečení založený na. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracení některé známé poskytovatele tokenů. Použijeme [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) přihlašovací údaje SAS metoda toohold hello. Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instance je pak vytvořený pomocí hello bázové adresy oboru názvů Service Bus hello a zprostředkovatele tokenu hello.

Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třída poskytuje metody toocreate, výčet a odstranění entit pro zasílání zpráv. Hello kód, který zobrazí se zde zobrazí jak hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instance je vytvořený a slouží toocreate hello **DataCollectionQueue** fronty.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Všimněte si, že jsou přetížení hello [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoda, která povolení vlastností toobe fronty hello přizpůsobená. Například můžete nastavit hello výchozí time to live (TTL) hodnotu pro zprávy odeslané toohello fronty.

### <a name="send-messages-toohello-queue"></a>Odesílat zprávy fronty toohello
Pro spuštění operace na Service Bus entity; například odesílání a přijímání zpráv, aplikace musíte nejdřív vytvořit [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objektu. Podobně jako toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třídy, hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance je vytvořený z hello bázové adresy oboru názvů služby hello a zprostředkovatele tokenu hello.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance třídy hello [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) třídy. Tato třída obsahuje sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník, který je použité toohold vlastnosti aplikace a tělo s libovolnými aplikačními daty. Aplikace může tělo hello nastavit tak předávání jakýkoli serializovatelný objekt (hello následující příklad předá **SalesData** objekt, který představuje data prodeje hello z terminálu hello POS), který budou používat hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello objektu. Alternativně [datového proudu](https://msdn.microsoft.com/library/system.io.stream.aspx) může být zadán objekt.

Nejjednodušší způsob, jak toosend zprávy tooa zadané frontě, v našem případu hello Hello **DataCollectionQueue**, je toouse [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objektu přímo z hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-hello-queue"></a>Přijímání zpráv z fronty hello
tooreceive zprávy z fronty hello, můžete použít [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt, který vytvoříte přímo z hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) pomocí [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Zpráva příjemci můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**. Hello [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) je nastaven při vytváření příjemce zprávu hello jako parametr toohello [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) volání.

Při použití hello **ReceiveAndDelete** přijímat hello režimu, je jednorázová operace; to znamená, když Service Bus přijme požadavek hello, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace. **ReceiveAndDelete** režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, které hello aplikace může tolerovat selhání kdyby toooccur se zpráva nezpracuje. toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho. Vzhledem k tomu, že Service Bus označena uvítací zprávu jako spotřebovávanou, když hello aplikace se restartuje a začne znovu přijímat zprávy ho bude neuskutečnily uvítací zprávu, která se spotřebovala před hello havárií.

V **PeekLock** přijímat hello režimu, se stane dvoufázová operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek hello, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace. Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) na uvítací přijal zprávu. Když Service Bus uvidí hello [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) volání, která se označuje uvítací zprávu jako spotřebovávanou.

K dispozici jsou dva další výsledky. První, pokud aplikace hello nedokáže tooprocess hello zprávy z nějakého důvodu, můžete volat [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) na uvítací přijal zprávu (místo [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). To způsobí, že Service Bus toounlock uvítací zprávu a nastavit jej jako dostupné toobe přijetí, buď hello příjemce stejné nebo jiné dokončuje příjemce. Druhý je časový limit zámku hello a pokud aplikace hello nemůže zpracovat zprávu hello před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak bude odemknutí Service Bus hello zprávy a nastavit jej jako dostupné toobe přijetí (v podstatě provádění [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) operace ve výchozím nastavení).

Všimněte si, že pokud hello aplikace spadne po zpracování uvítací zprávu, ale před hello [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) byl vydán požadavek, uvítací zprávu bude víckrát toohello aplikace odešle znovu. To se často říká * alespoň jednou * zpracování. To znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát. Pokud hello scénář nemůže tolerovat zpracování duplicitní, bude potřeba další logiku v hello aplikace toodetect duplikáty. Toho lze dosáhnout podle hello [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) vlastnost uvítací zprávu. Hodnota této vlastnosti Hello konstantní mezi pokusy o doručení. To se říká *právě jednou* zpracování.

Hello kód, který se zobrazí tady obdrží a zpracuje zprávy pomocí hello **PeekLock** režimu, což je výchozí hello, pokud žádné [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) explicitně zadat hodnotu.

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-hello-queue-client"></a>Použijte hello fronty klienta
Hello příklady dříve v této části vytvořit [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) a [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekty přímo z hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) toosend a přijímat zprávy z fronty hello v uvedeném pořadí. Alternativní způsob je toouse [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objekt, který podporuje jak operace odesílání a přijímání v přidání toomore rozšířené funkce, jako je například relací.

```csharp
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);

BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello front, najdete v části [vytvářet aplikace, které používají témata a odběry Service Bus](service-bus-create-topics-subscriptions.md) toocontinue toto pojednání pomocí funkce pbulikovat/odebírat hello témat Service Bus a odběry.

