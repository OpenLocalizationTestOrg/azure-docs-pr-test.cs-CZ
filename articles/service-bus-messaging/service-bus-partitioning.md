---
title: "aaaCreate oddíly témata a fronty Azure Service Bus | Microsoft Docs"
description: "Popisuje, jak zprostředkovává toopartition fronty Service Bus a témat pomocí více zprávy."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;hillaryc
ms.openlocfilehash: 6d42556a0714d6a012dc319f662521c8b0bb958b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partitioned-queues-and-topics"></a>Dělené fronty a témata
Azure Service Bus používá více zpráv zprostředkovatelé tooprocess zprávy a více zasílání zpráv ukládá toostore zprávy. Konvenční fronta nebo téma zpracování zprostředkovatelem jedné zprávy a uloženy v úložišti jeden zasílání zpráv. Service Bus *oddíly* povolit front a témat, nebo *entity pro zasílání zpráv*, toobe rozděleného mezi více zpráv zprostředkovatelé a úložiště pro zasílání zpráv. To znamená, že hello celkovou propustnost dělené entity je již omezena hello výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv. Kromě toho dočasnému výpadku zasílání zpráv úložiště nevykresluje oddílů fronta nebo téma není k dispozici. Oddílů fronty a témata může obsahovat všechny pokročilé funkce služby Service Bus, například podporu transakce a relací.

Informace o interní informace o Service Bus najdete v tématu hello [architektura služby Service Bus] [ Service Bus architecture] článku.

Ve výchozím nastavení při vytváření entity na všechny fronty a témata v Standard a Premium dělení je povoleno zasílání zpráv. Můžete vytvořit standardní vrstvě entity pro zasílání zpráv bez vytváření oddílů, ale front a témat v oboru názvů Premium jsou vždy oddíly; Tuto možnost nelze zakázat. 

Není možné toochange hello oddílů možnost na existující frontu nebo téma na úrovních Standard nebo Premium, můžete nastavit pouze možnost hello při vytvoření hello entity.

## <a name="how-it-works"></a>Jak to funguje

Každý oddílů fronta nebo téma se skládá z více fragmentů. Každý fragment je uložen v jiném úložišti zasílání zpráv a provádí jiná zpráva zprostředkovatele. Odeslání zprávy tooa oddílů fronta nebo téma sběrnice přiřadí hello tooone zprávy z fragmentů hello. Výběr Hello je náhodně provádí sběrnice nebo můžete zadat pomocí klíč oddílu, který hello odesílatele.

Pokud klient chce tooreceive zprávu z fronty oddílů nebo předplatné tooa oddílů tématu Service Bus dotazuje všechny fragmenty pro zprávy, pak vrátí hello první zprávu, která se získá z jakéhokoli z hello příjemce toohello úložiště pro zasílání zpráv. Service Bus mezipamětí hello další zprávy a vrátí je při přijetí další přijímat požadavky. Přijímající klient nemá informace o vytváření oddílů hello; Hello zajišťující komunikaci s klienty chování oddílů fronta nebo téma (například číst, dokončení, odložení, nedoručených zpráv, prefetching) je identické toohello chování regulárních entity.

Není k dispozici bez dalších nákladů při odesílání zprávy, která se nebo přijímání zprávy z oddílů fronta nebo téma.

## <a name="enable-partitioning"></a>Povolit vytváření oddílů

toouse rozdělena na oddíly front a témat s Azure Service Bus, použít hello Azure SDK verze 2.2 nebo vyšší, nebo zadat `api-version=2013-10` požadavků v protokolu HTTP.

### <a name="standard"></a>Standard

Na úrovni Standard zasílání zpráv hello můžete vytvořit fronty Service Bus a témat v 1, 2, 3, 4 nebo 5 GB velikosti (hello výchozí hodnota je 1 GB). Service Bus s oddílů povolen, vytváří 16 kopie (16 oddíly) hello entity pro každého GB zadáte. Jako takový, když vytvoříte frontu, který je 5 GB velikost, s 16 oddíly hello maximální velikost fronty stane (5 \* 16) = 80 GB. Zobrazí maximální velikost hello oddílů fronta nebo téma na základě jeho položku na hello [portál Azure][Azure portal], v hello **přehled** okno této entity.

### <a name="premium"></a>Premium

V oboru názvů úrovně Premium můžete vytvořit fronty Service Bus a témat v 1, 2, 3, 4, 5, 10, 20, 40 nebo 80 GB velikosti (hello výchozí hodnota je 1 GB). Service Bus s oddíly, ve výchozím nastavení povolená, vytvoří dva oddíly na entity. Zobrazí maximální velikost hello oddílů fronta nebo téma na základě jeho položku na hello [portál Azure][Azure portal], v hello **přehled** okno této entity.

Další informace o vytváření oddílů v zasílání zpráv úrovně Premium hello najdete v tématu [Service Bus Premium a Standard zasílání zpráv úrovně](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>Vytvoření oddílů entity

Existuje několik způsobů toocreate oddílů fronta nebo téma. Když vytvoříte hello fronta nebo téma z vaší aplikace, můžete povolit vytváření oddílů pro hello fronta nebo téma podle v uvedeném pořadí nastavení hello [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] nebo [TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] vlastnost příliš**true**. Tyto vlastnosti musí být nastaven do fronty hello čas hello nebo se vytvoří téma. Jak jsme uvedli dříve, není možné toochange tyto vlastnosti existující fronta nebo téma. Například:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternativně můžete vytvořit oddílů fronta nebo téma v hello [portál Azure] [ Azure portal] nebo v sadě Visual Studio. Když vytvoříte se fronta nebo téma hello portálu, hello **povolit vytváření oddílů** možnost v hello fronta nebo téma **vytvořit** okno je ve výchozím nastavení zaškrtnuto. Pouze můžete zakázat tuto možnost v entitě úrovně Standard; v hello dělení úroveň Premium je vždy povolena. V sadě Visual Studio, klikněte na tlačítko hello **povolit vytváření oddílů** zaškrtnout políčko hello **novou frontu** nebo **nové téma** dialogové okno.

## <a name="use-of-partition-keys"></a>Použití klíče oddílů
Zařazených do fronty do oddílů fronta nebo téma po zprávu Service Bus ověří přítomnost hello klíč oddílu. V případě, že některou najde, vybere hello fragment na základě tohoto klíče. Pokud nenajde klíč oddílu, vybere hello fragment založený na interní algoritmu.

### <a name="using-a-partition-key"></a>Použitím klíče oddílu
Některé scénáře, jako je například relací nebo transakce, vyžadují toobe zprávy, které jsou uložené v určité fragment. Všechny tyto scénáře vyžadují použití hello klíč oddílu. Všechny zprávy této použití hello stejným klíčem oddílu přiřazené toohello stejné fragmentovat. Pokud hello fragment je dočasně nedostupný, Service Bus vrátí chybu.

V závislosti na scénáři hello vlastnosti jiná zpráva slouží jako klíč oddílu:

**SessionId**: Pokud zpráva má hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] nastavte vlastnost a potom Service Bus používá tuto vlastnost jako klíč oddílu hello. Tímto způsobem, všechny zprávy, které patří toohello stejné relace jsou zpracovávány hello stejné zprostředkovatele zpráv. To umožňuje zpráv Service Bus tooguarantee řazení a také hello konzistence stavy relací.

**PartitionKey**: Pokud zpráva má hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] vlastnost, která však není hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] nastavte vlastnost a potom Service Bus používá hello [PartitionKey] [ PartitionKey] vlastnost jako klíč oddílu hello. Pokud má obě hello uvítací zprávu [SessionId] [ SessionId] a hello [PartitionKey] [ PartitionKey] nastavení vlastnosti, obě vlastnosti musí být identické. Pokud hello [PartitionKey] [ PartitionKey] je nastavena tooa jinou hodnotu než hello [SessionId] [ SessionId] neplatný vrátí vlastnost, Service Bus operace došlo k výjimce. Hello [PartitionKey] [ PartitionKey] vlastnost je užitečný v případě, že odesílatel odešle vědět transakční zprávy mimo relaci. Hello klíč oddílu zajistí, že všechny zprávy, které se odesílají v rámci transakce jsou zpracovávány hello stejné zasílání zpráv zprostředkovatele.

**MessageId**: Pokud hello fronta nebo téma má hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] vlastností nastavenou příliš**true** a hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] nebo [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] nejsou nastaveny vlastnosti a potom hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] vlastnost slouží jako klíč oddílu hello. (Všimněte si, že knihovny rozhraní Microsoft .NET a AMQP hello automaticky přiřadit ID zprávy, pokud hello odesílající aplikací nemá.) V takovém případě všechny kopie hello stejnou zprávu zpracovává hello stejné zprostředkovatele zpráv. To umožňuje toodetect Service Bus a odstranění duplicitních zpráv. Pokud hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] příliš není nastavena vlastnost**true**, Service Bus nebere v úvahu hello [MessageId] [ MessageId] vlastnost jako klíč oddílu.

### <a name="not-using-a-partition-key"></a>Není použitím klíče oddílu
V hello chybí klíč oddílu distribuuje sběrnice zpráv v každém fragmentů kruhového dotazování tooall hello hello oddílů fronta nebo téma. Pokud hello vybrali fragment není k dispozici, Service Bus přiřadí různé fragmentu tooa hello zprávy. Tímto způsobem operaci odeslání hello úspěšné i přes hello dočasné nedostupnosti úložišti pro přenos zpráv. Nebudou však dosáhnout hello zaručit, řazení, že poskytuje klíč oddílu.

Podrobné informace o hello kompromis mezi dostupnost (žádný klíč oddílu) a konzistence (s použitím klíč oddílu), najdete v části [v tomto článku](../event-hubs/event-hubs-availability-and-consistency.md). Tyto informace se vztahuje stejnou měrou toopartitioned entit služby Service Bus a Event Hubs oddíly.

toogive služby sběrnici dostatek času tooenqueue uvítací zprávu do různých fragment, hello [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] hodnotu zadanou pomocí hello klienta, který odešle zprávu hello musí být větší než 15 sekund. Doporučujeme, abyste nastavili hello [OperationTimeout] [ OperationTimeout] vlastnost toohello výchozí hodnotu 60 sekund.

Všimněte si, že klíč oddílu "PIN kódy" fragment konkrétní tooa zprávy. Pokud není k dispozici hello v úložišti pro přenos zpráv, který obsahuje tento fragment, Service Bus vrátí chybu. V hello chybí klíč oddílu Service Bus můžete vybrat jiný fragment a hello operace úspěšná. Proto se doporučuje, aby nezadáte klíč oddílu Pokud to není nutné.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Rozšířené témata: použití transakcí s dělené entity
Zprávy, které se odesílají v rámci transakce. musíte zadat klíč oddílu. To může být jedna z hello následující vlastnosti: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], nebo [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Všechny zprávy, které se odesílají jako součást hello musíte zadat stejné transakci hello stejným klíčem oddílu. Když zkusíte toosend zprávu bez klíče oddílu v rámci transakce, Service Bus vrací výjimku neplatná operace. Pokud se pokusíte toosend hello více zpráv v rámci stejné transakce, které mají různé klíče oddílů, Service Bus vrací výjimku neplatná operace. Například:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Pokud jsou nastavená jakékoli hello vlastnosti, které slouží jako klíč oddílu, Service Bus PIN hello konkrétní fragmentu tooa zprávy. K tomuto chování dochází, zda se používá transakce. Doporučuje se, že nezadáte klíč oddílu Pokud není nutné.

## <a name="using-sessions-with-partitioned-entities"></a>Použití relací s dělené entity
toosend deklaracemi relace téma tooa transakční zprávy nebo fronty, uvítací zprávu musí mít hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] sadu vlastností. Pokud hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] také zadána vlastnost, musí být identické toohello [SessionId] [ SessionId] vlastnost. Pokud se liší, vrátí se Service Bus výjimku neplatná operace.

Na rozdíl od front regulární (bez oddílů) nebo témata není možné toouse toosend jedné transakce více relací toodifferent zprávy. Pokud pokus, vrátí se Service Bus výjimku neplatná operace. Například:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Předávání automatické zpráv s dělené entity
Service Bus podporuje automatické zpráva předávání od, k nebo mezi dělené entity. Automatické zpráva tooenable předávání, nastavte hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] vlastnost hello zdrojové fronty nebo předplatné. Pokud zpráva hello Určuje klíč oddílu ([SessionId][SessionId], [PartitionKey][PartitionKey], nebo [MessageId] [ MessageId]), tento klíč oddílu se používá pro hello cílové entity.

## <a name="considerations-and-guidelines"></a>Důležité informace a pokyny
* **Funkce vysoké konzistence**: Pokud entity používá funkce, jako jsou relace, detekci duplikátů nebo explicitní kontrolu nad klíč rozdělení do oddílů, pak hello zasílání zpráv operace jsou vždy směrované toospecific fragmenty. Pokud je některý z hello fragmentů intenzivní provoz nebo hello příslušné úložiště není v pořádku, tyto operace nezdaří a sníží dostupnosti. Obecně je stále mnohem vyšší než bez oddílů entity; hello konzistence pouze podmnožinu provozu dochází k problémům, jako názvem na rozdíl od tooall hello provoz. Další informace najdete v tématu to [diskuzi o dostupnosti a konzistence](../event-hubs/event-hubs-availability-and-consistency.md).
* **Správa**: operací, jako je vytvoření, aktualizace a odstranění je potřeba provést na všechny fragmenty hello hello entity. Pokud není v pořádku žádné fragment to může způsobit selhání pro tyto operace. Pro operaci Get hello informace, jako je počty zprávy musí být agregován z všechny fragmenty. Pokud žádné fragment není v pořádku, stav dostupnosti entity hello se hlásí jako omezené.
* **Nízká svazku zpráva scénáře**: pro takové scénáře, zejména v případě, že pomocí protokolu hello HTTP, může mít tooperform více přijímat operace v pořadí tooobtain všechny zprávy hello. Pro přijetí žádosti front-endu hello provádí všechny fragmenty hello příjmu a ukládá do mezipaměti všechny hello odpovědí přijatých. Následné receive požadavek na hello stejné připojení by mít užitek z této ukládání do mezipaměti a přijímat latence bude nižší. Ale pokud máte více připojení nebo prostřednictvím protokolu HTTP, který vytvoří nové připojení pro každý požadavek. Jako takový neexistuje žádná záruka, který se bude zobrazovat hello stejného uzlu. Pokud všechny existující zprávy jsou zamčené uložené v mezipaměti v jiné front-endu, hello zobrazí operaci vrátí **null**. Zprávy nakonec vyprší a je můžete přijímat znovu. Doporučuje se udržování připojení HTTP.
* **Procházet zprávy funkce Náhled**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) vždy nevrací hello počet zpráv zadaný v hello [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MessageCount) vlastnost. Existují dvě běžné důvody. Jednou z příčin je, že hello agregovaná velikost hello kolekci zpráv překračuje maximální velikost 256KB hello. Dalším důvodem je, že pokud hello fronta nebo téma má hello [enablepartitioning je vlastnost](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning) nastavit příliš**true**, oddíl nemusí mít dostatek zprávy toocomplete hello požadovaný počet zpráv. Obecně platí, pokud aplikace chce tooreceive konkrétní počet zpráv, ho by měly volat [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) opakovaně, dokud získá počet zpráv, nebo nejsou žádné další zprávy toopeek. Další informace, včetně ukázky kódu, najdete v části [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) nebo [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_PeekBatch_System_Int32_).

## <a name="latest-added-features"></a>Nejnovější přidané funkce
* Přidat nebo odebrat pravidlo se teď podporuje s dělené entity. Liší od bez oddílů entity, tyto operace nejsou podporovány v rámci transakce. 
* AMQP se teď podporuje pro posílání a přijímání zpráv tooand z oddílů entity.
* AMQP je nyní podporován pro hello následující operace: [odeslání dávky](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_BrokeredMessage__), [přijímat Batch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ReceiveBatch_System_Int32_), [Receive podle pořadových čísel](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Receive_System_Int64_), [prohlížet](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Peek), [ Obnovení zámku](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_RenewMessageLock_System_Guid_), [naplánovat zpráva](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ScheduleMessageAsync_Microsoft_ServiceBus_Messaging_BrokeredMessage_System_DateTimeOffset_), [zrušit naplánované zpráva](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_CancelScheduledMessageAsync_System_Int64_), [přidat pravidlo](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [odebrat pravidlo](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Relace obnovení zámku](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_RenewLock), [stav relace sady](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_), [stav relace Get](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_GetState), a [výčet relací](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessionsAsync).

## <a name="partitioned-entities-limitations"></a>Dělené entity omezení
Service Bus aktuálně ukládá hello následující omezení na oddílů fronty a témata:

* Oddílů fronty a témata nepodporují odesílání zpráv, které patří toodifferent relací v rámci jedné transakce.
* Service Bus aktuálně umožňuje si too100 rozdělena na oddíly fronty a témata na obor názvů. Každý oddílů fronta nebo téma započítává hello kvóty 10 000 entit na obor názvů (nevztahuje tooPremium úroveň).

## <a name="next-steps"></a>Další kroky
V tématu hello diskuzi o [podporu protokolu AMQP 1.0 pro Service Bus oddíly fronty a témata] [ AMQP 1.0 support for Service Bus partitioned queues and topics] toolearn informace o vytváření oddílů entit pro zasílání zpráv. 

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription#Microsoft_ServiceBus_Messaging_TopicDescription_EnablePartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
