---
title: "Psaní aplikací, které používají fronty Azure Service Bus | Microsoft Docs"
description: "Jak napsat jednoduchou aplikaci na základě fronty, která používá Azure Service Bus."
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
ms.openlocfilehash: 419caff7e8ceeb419c89a2ef9a6614c1accf3e52
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Vytváření aplikací používajících fronty Service Bus
Toto téma popisuje fronty Service Bus a ukazuje, jak napsat jednoduchou aplikaci na základě fronty, která používá Service Bus.

Představte si třeba situaci ze světa prodejní, ve kterém prodeje dat z jednotlivých terminály Point-of-Sale (POS) musejí směrovat na systém pro správu inventáře, která data používá k určení, kdy musí být doplněny stock. Toto řešení používá pro komunikaci mezi terminály a systém pro správu inventáře, zasílání zpráv Service Bus, jak je znázorněno na následujícím obrázku:

![Fronty služby Service Bus obrázku 1](./media/service-bus-create-queues/IC657161.gif)

Každý terminál POS sestavy jeho data prodeje odesláním zprávy a pokuste se **DataCollectionQueue**. Tyto zprávy zůstanou v této frontě, dokud se načítají pomocí systému pro správu inventáře. Toto chování se často říká *asynchronní zasílání zpráv*, protože není nutné čekat na odpověď od systém pro správu inventáře pokračovat zpracování POS terminálu.

## <a name="why-queuing"></a>Proč služba Řízení front?
Předtím, než se podíváme na kód, který je nutný k nastavení této aplikace, zvažte výhody používání fronty v tomto scénáři místo nutnosti terminály POS komunikovat přímo (synchronně) do systému pro správu inventáře.

### <a name="temporal-decoupling"></a>Časové oddělení
S asynchronním vzorcem zasílání zpráv producenti a spotřebitelé nemusí být online ve stejnou dobu. Infrastruktura přenosu zpráv spolehlivě uloží zprávy, dokud spotřebitel nebude připravený je přijmout. To znamená, že se že součásti distribuované aplikace můžou být odpojit; například pro údržbu, nebo kvůli součástí, bez ovlivnění celý systém. Kromě toho spotřebitelskou aplikací může mít pouze jako online v určitou dobu dne. Například v tomto scénáři prodejní systém pro správu inventáře může mít pouze do režimu online na konci pracovního dne.

### <a name="load-leveling"></a>Vyrovnávání zátěže
V mnoha aplikacích zatížení systému se liší v čase, zatímco zpracování čas potřebný pro jednotlivé jednotky práce je obvykle stálá. Propojovací producenti a spotřebitelé zpráv s frontou znamená, že spotřebitelskou aplikaci (pracovní proces) má jenom zřídit na služby průměrnou zátěž spíše než zátěž ve špičce. Hloubka fronty budou růst a smluv jako příchozí zátěží se mění. To znamená přímou úsporu nákladů s ohledem na množství infrastruktury nutné pro zvládání zatížení aplikace.

![Fronty služby Service Bus obrázku 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Vyrovnávání zatížení
Při rostoucí zátěži, lze přidat další pracovní procesy ke čtení z fronty pracovních procesů. Každou zprávu zpracovává jen jeden pracovní proces. Kromě toho tato Vyrovnávání zatížení založené na operaci pull umožňuje optimální využívání pracovních počítačů i v případě, že pracovní počítače liší s ohledem na výpočetní výkon, jak se bude načítat zprávy na svou vlastním maximální rychlostí. Toto chování se často říká konkurenční vzoru příjemce.

![Obrázek 3 fronty Service Bus](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Volné párování
Použití pro zprostředkující mezi producenti a spotřebitelé zpráv služby Řízení front zpráv poskytuje vnitřní volné párování mezi součástmi. Protože producenti a spotřebitelé nemají informace o sobě navzájem, bez nutnosti nijak neprojeví na Autor lze upgradovat příjemce. Kromě toho můžete zasílání zpráv topologie rozvíjet bez ovlivnění stávající koncové body. Probereme to více při mluvíme o publikování a přihlášení k odběru.

## <a name="show-me-the-code"></a>Zobrazit mi kód
V následující části ukazuje, jak sběrnice můžete použít k vytvoření této aplikace.

### <a name="sign-up-for-an-azure-account"></a>Zaregistrujte si účet Azure
Chcete-li začít pracovat se Service Bus, budete potřebovat účet Azure. Pokud není již nemáte, můžete zaregistrovat bezplatný účet [zde](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Vytvoření oboru názvů
Jakmile máte předplatné, můžete [vytvořit obor názvů služby](service-bus-create-namespace-portal.md). Každý obor názvů funguje jako kontejner oboru pro sadu entit služby Service Bus. Zadejte jedinečný název váš nový obor názvů mezi všechny účty služby Service Bus. 

### <a name="install-the-nuget-package"></a>Nainstalujte balíček NuGet
Pro používání oboru názvů Service Bus, aplikace musí odkazovat na sestavení Service Bus, konkrétně Microsoft.ServiceBus.dll. Můžete najít toto sestavení v rámci Microsoft Azure SDK a je k dispozici ke stažení [stránky pro stažení sady Azure SDK](https://azure.microsoft.com/downloads/). Ale [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) Nejsnadnějším způsobem, chcete-li získat rozhraní API Service Bus a nakonfigurovat svoji aplikaci se všemi závislostmi služby Service Bus.

### <a name="create-the-queue"></a>Vytvoření fronty
Operace správy pro Service Bus entit pro zasílání zpráv (fronty a publikování a přihlášení k odběru témata) se provádí prostřednictvím [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třídy. Service Bus používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) model zabezpečení založený na. [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracení některé známé poskytovatele tokenů. Použijeme [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoda pro uložení pověření SAS. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instance je pak vytvořený pomocí bázové adresy oboru názvů Service Bus a zprostředkovateli tokenu.

[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třída poskytuje metody pro vytvoření, výčet a odstranění entit přenosu zpráv. Kód, který zobrazí se zde zobrazí jak [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) je vytvořit a použít k vytvoření instance **DataCollectionQueue** fronty.

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

Všimněte si, že existují přetížení [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoda, která povolení vlastností fronty v úzkém ladit. Můžete například nastavit výchozí time to live (TTL) hodnotě pro zprávy odeslané do fronty.

### <a name="send-messages-to-the-queue"></a>Zasílání zpráv do fronty
Pro spuštění operace na Service Bus entity; například odesílání a přijímání zpráv, aplikace musíte nejdřív vytvořit [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objektu. Podobně jako [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) třídy, [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance je vytvořený z bázové adresy oboru názvů služby a zprostředkovateli tokenu.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Zprávy odeslané do a fronty přijal od služby Service Bus jsou instance [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) třídy. Tato třída obsahuje sadu standardních vlastností (jako například [popisek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) a [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), slovník používaný pro udržení vlastností aplikace a tělo s libovolnými aplikačními daty. Aplikace můžete nastavit text tak předávání jakýkoli serializovatelný objekt (v následujícím příkladu předá v **SalesData** objekt, který představuje data z prodeje z terminálu POS), který bude používat [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) k serializaci objektu. Alternativně [datového proudu](https://msdn.microsoft.com/library/system.io.stream.aspx) může být zadán objekt.

Nejjednodušší způsob, jak zasílání zpráv do dané fronty, v našem případě **DataCollectionQueue**, je použití [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) k vytvoření [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) přímo objektu z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Přijímání zpráv z fronty
Pro příjem zpráv z fronty, můžete použít [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt, který vytvoříte přímo z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) pomocí [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Zpráva příjemci můžou pracovat ve dvou různých režimech: **ReceiveAndDelete** a **PeekLock**. [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) při vytvoření příjemce zprávu jako parametr pro nastavený [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) volání.

Při použití **ReceiveAndDelete** režimu, je je přijetí jednorázová operace; to znamená, když Service Bus přijme požadavek, označí zprávu jako spotřebovávanou a vrátí ji do aplikace. **ReceiveAndDelete** režimu je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat Pokud dojde k selhání se zpráva nezpracuje. Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se. Vzhledem k tomu, že Service Bus označit zprávu jako spotřebovávanou, když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.

V **PeekLock** režimu, je přijetí stane dvoufázová operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde zprávu, který se má používat, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace. Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), zavolá na přijatou zprávu [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), a tím potvrdí dokončení druhé fáze přijetí. Když Service Bus uvidí [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) volání, označí zprávu jako spotřebovávanou.

K dispozici jsou dva další výsledky. První, pokud aplikace nemůže zpracovat zprávu z nějakého důvodu, může zavolat [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) na přijatou zprávu (místo [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). To způsobí, že Service Bus zprávu odemkne a zpřístupní ji pro další přijetí, buď stejným uživatelem, nebo jiné dokončuje příjemce. Druhý je časový limit zámku a pokud aplikace nelze zpracovat zprávu, vyprší časový limit uzamčení (například pokud aplikace spadne), pak bude Service Bus zprávu odemkne a nastavit jej jako lze přijaté znovu) v podstatě provádění [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) operace ve výchozím nastavení).

Všimněte si, že pokud aplikace spadne po zpracování zprávy ale předtím, než [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) byl vydán požadavek, zpráva bude vysláním do aplikace odešle znovu. To se často říká * alespoň jednou * zpracování. To znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích může být stejná zpráva víckrát. Pokud scénář nemůže tolerovat zpracování duplicitní, bude potřeba další logiku v aplikaci duplicity. Toho lze dosáhnout na základě [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) vlastnost zprávy. Hodnota této vlastnosti konstantní mezi pokusy o doručení. To se říká *právě jednou* zpracování.

Kód, který se zobrazí tady obdrží a zpracuje zprávu pomocí **PeekLock** režimu, který je výchozím nastavením, pokud žádné [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) explicitně zadat hodnotu.

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

### <a name="use-the-queue-client"></a>Pomocí klienta fronty
Příklady dříve v této části vytvořit [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) a [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekty přímo z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) odesílat a přijímat zprávy z ve frontě, v uvedeném pořadí. Alternativní způsob je použít [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objekt, který podporuje jak operace odesílání a přijímání kromě dalších pokročilých funkcí, jako je například relací.

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
Teď, když jste se naučili základy front, najdete v části [vytvářet aplikace, které používají témata a odběry Service Bus](service-bus-create-topics-subscriptions.md) pokračujte toto pojednání pomocí možnosti publikování a přihlášení k odběru témat a odběrů Service Bus.

