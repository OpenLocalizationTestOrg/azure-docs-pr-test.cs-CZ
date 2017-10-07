---
title: aaaService Bus s .NET a protokolu AMQP 1.0 | Microsoft Docs
description: "Pomocí protokolu AMQP Azure Service Bus pomocí technologie .NET"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: d8b40f92ba29058951556fa3db1adcf9383ee69f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-service-bus-from-net-with-amqp-10"></a>Service Bus pomocí technologie .NET pomocí protokolu AMQP 1.0

## <a name="downloading-hello-service-bus-sdk"></a>Stahování hello Service Bus SDK

Podpora protokolu AMQP 1.0 je k dispozici v hello Service Bus SDK verze 2.1 nebo vyšší. Můžete zajistit, máte nejnovější verzi hello stažením hello Service Bus bits z [NuGet][NuGet].

## <a name="configuring-net-applications-toouse-amqp-10"></a>Konfigurace toouse aplikace .NET protokolu AMQP 1.0

Ve výchozím nastavení Klientská knihovna pro Service Bus .NET hello komunikuje s hello služby Service Bus pomocí vyhrazené protokol založený na protokolu SOAP. toouse protokolu AMQP 1.0 místo hello výchozím protokolem vyžaduje explicitní konfiguraci na hello připojovací řetězec sběrnice služeb, jak je popsáno v další části hello. Než tuto změnu zůstává beze změny kódu aplikace, při použití protokolu AMQP 1.0.

V aktuální verzi hello existuje několik funkcí rozhraní API, které nejsou podporována při použití protokolu AMQP. Tyto nepodporovaných funkcích jsou uvedené dále v části hello [nepodporované funkce, omezení a chování rozdíly](#unsupported-features-restrictions-and-behavioral-differences). Některé pokročilé nastavení konfigurace hello také mít jiný význam při použití protokolu AMQP.

### <a name="configuration-using-appconfig"></a>Konfigurace pomocí souboru App.config

Je dobrým zvykem toostore nastavení souboru App.config Konfigurace aplikace toouse hello. U aplikací, Service Bus můžete použít App.config toostore hello Service Bus připojovací řetězec. Příklad souboru App.config vypadá takto:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Hello hodnotu hello `Microsoft.ServiceBus.ConnectionString` nastavení je hello připojovací řetězec sběrnice služeb, který je použité tooconfigure hello připojení tooService sběrnice. Formát Hello je následující:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Kde `[namespace]` a `SharedAccessKey` jsou získávány z hello [portál Azure] [ Azure portal] při vytváření oboru názvů Service Bus. Další informace najdete v tématu [vytvoření oboru názvů Service Bus pomocí portálu Azure hello][Create a Service Bus namespace using hello Azure portal].

Při použití protokolu AMQP, připojit hello připojovací řetězec s `;TransportType=Amqp`. Tento zápis dá pokyn hello klienta knihovny toomake jeho tooService připojení sběrnice pomocí protokolu AMQP 1.0.

## <a name="message-serialization"></a>Zpráva serializace

Při použití protokolu výchozí hello, hello výchozí serializace chování klientské knihovny .NET hello je toouse hello [DataContractSerializer] [ DataContractSerializer] zadejte tooserialize [BrokeredMessage ] [ BrokeredMessage] instance pro přenos mezi hello klientské knihovny a službou Service Bus hello. Při použití režim přenosu protokolu AMQP hello, hello Klientská knihovna používá systém typů AMQP hello za účelem serializace hello [zprostředkované zprávy] [ BrokeredMessage] do zprávy protokolu AMQP. Serializace umožňuje hello zpráva toobe přijaty a interpretovat přijímací aplikace, která potenciálně běží na jiné platformě, například aplikaci Java, která používá rozhraní API JMS tooaccess hello Service Bus.

Když vytvoříte [BrokeredMessage] [ BrokeredMessage] instance, můžete zadat objekt .NET jako parametr toohello konstruktor tooserve jako text hello hello zprávy. Pro objekty, které se dají mapovat tooAMQP primitivní typy je text hello serializován do AMQP datových typů. Pokud objekt hello nelze mapovat přímo do AMQP primitivní typ; To znamená, vlastního typu definované hello aplikace a pak hello objekt serializován pomocí hello [DataContractSerializer][DataContractSerializer], a jsou odesílány bajty hello serializovat data zprávy protokolu AMQP.

toofacilitate vzájemnou spolupráci s klienty rozhraní .NET, použít pouze typy .NET, které lze serializovat přímo do AMQP typy hello tělo zprávy hello. Následující tabulka Hello podrobnosti tyto typy a hello odpovídající mapování toohello AMQP typ systému.

| Typ objektu textu rozhraní .NET | Typ namapované AMQP | Typ oddílu AMQP textu |
| --- | --- | --- |
| BOOL |Logická hodnota |Hodnota AMQP |
| Bajtů |ubyte |Hodnota AMQP |
| ushort – |ushort – |Hodnota AMQP |
| uint |uint |Hodnota AMQP |
| ulong – |ulong – |Hodnota AMQP |
| SByte – |Bajtů |Hodnota AMQP |
| krátký |krátký |Hodnota AMQP |
| celá čísla |celá čísla |Hodnota AMQP |
| dlouhá |dlouhá |Hodnota AMQP |
| Plovoucí desetinná čárka |Plovoucí desetinná čárka |Hodnota AMQP |
| Double |Double |Hodnota AMQP |
| Decimal |decimal128 |Hodnota AMQP |
| Char |Char |Hodnota AMQP |
| Data a času |časové razítko |Hodnota AMQP |
| Identifikátor GUID |UUID |Hodnota AMQP |
| Byte] |Binární |Hodnota AMQP |
| Řetězec |Řetězec |Hodnota AMQP |
| System.Collections.IList |seznam |Hodnota AMQP: položek obsažených v kolekci hello lze pouze ty, které jsou definovány v této tabulce. |
| System.Array |Pole |Hodnota AMQP: položek obsažených v kolekci hello lze pouze ty, které jsou definovány v této tabulce. |
| System.Collections.IDictionary |mapy |Hodnota AMQP: položek obsažených v kolekci hello lze pouze ty, které jsou definovány v této tabulce. Poznámka: jsou podporovány pouze řetězcových klíčů. |
| identifikátor URI |Popisuje řetězec (viz následující tabulka hello) |Hodnota AMQP |
| Datový typ DateTimeOffset |Popisuje dlouho (viz následující tabulka hello) |Hodnota AMQP |
| Časový interval |Popisuje dlouho (viz následující hello) |Hodnota AMQP |
| Datový proud |Binární |Data protokolu AMQP (může být více). Hello datové části obsahují hello nezpracovaná bajtů přečtených z datového proudu hello. |
| Druhý objekt |Binární |Data protokolu AMQP (může být více). Obsahuje binární hello serializovat hello objektu, který používá hello DataContractSerializer nebo serializátor poskytl aplikace hello. |

| Typ formátu .NET | Mapovat AMQP popisuje typ | Poznámky |
| --- | --- | --- |
| identifikátor URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| Datový typ DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| Časový interval |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nepodporované funkce, omezení a rozdíly v chování

Následující funkce hello Service Bus .NET API Hello nejsou aktuálně podporovány při použití protokolu AMQP:

* Transakce
* Odesílání prostřednictvím cíl přenosu

Existují zde také některé malé rozdíly v chování hello hello rozhraní API služby Service Bus .NET při použití protokolu AMQP, porovnání toohello výchozí protokol:

* Hello [OperationTimeout] [ OperationTimeout] vlastnost je ignorována.
* `MessageReceiver.Receive(TimeSpan.Zero)`je implementovaný jako `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Dokončení zprávy pomocí tokenů zámku lze provést pouze hello příjemci zprávy, které původně přijaté zprávy hello.

## <a name="controlling-amqp-protocol-settings"></a>Nastavení protokolu AMQP řízení

Hello [rozhraní API technologie .NET](/dotnet/api/) vystavit toocontrol nastavení několik hello chování hello protokolu AMQP:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: ovládací prvky hello počáteční Dal použít tooa odkaz. Hello výchozí hodnota je 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: ovládací prvky hello maximální AMQP rámce velikost nabízená při vyjednávání hello v době otevřené připojení. Hello výchozí hodnota je 65 536 bajtů.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Pokud batchable přenosů se tato hodnota určuje maximální zpoždění hello k odeslání potížemi. Ve výchozím nastavení dědí odesílatelé nebo příjemci. Jednotlivé odesílatel/příjemce můžete přepsat výchozí hello, což je 20 milisekundách.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: Určuje, zda jsou připojení AMQP navázat připojení přes protokol SSL. Výchozí hodnota Hello je **true**.

## <a name="next-steps"></a>Další kroky

Připraveno toolearn další? Hello najdete na následující odkazy:

* [Přehled protokolu AMQP Service Bus]
* [Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]
* [AMQP v Service Bus pro systém Windows Server]

[Create a Service Bus namespace using hello Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Přehled protokolu AMQP Service Bus]: service-bus-amqp-overview.md
[Podpora protokolu AMQP 1.0 témata a fronty Service Bus rozdělena na oddíly]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
