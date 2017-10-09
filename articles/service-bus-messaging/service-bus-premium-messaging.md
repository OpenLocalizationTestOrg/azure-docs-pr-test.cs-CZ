---
title: "aaaAzure Service Bus Premium a Standard zasílání zpráv cenové úrovně přehled | Microsoft Docs"
description: "Úrovně zasílání zpráv Service Bus Premium a Standard"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: darosa;sethm
ms.openlocfilehash: 4eea5d86d342e858f50450308fb3d96a7a80b49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Úrovně zasílání zpráv Service Bus Premium a Standard

Zasílání zpráv Service Bus, které zahrnuje entity jako jsou fronty a témata, v sobě kombinuje funkce pro zasílání zpráv v rámci podniku s bohatou sémantikou publikování a odběru na úrovni cloudu. Zasílání zpráv Service Bus se používá jako páteřní hello mnoha sofistikovaných cloudových řešení.

Hello *Premium* úroveň zasílání zpráv Service Bus řeší běžné požadavky zákazníků z pohledu škálování, výkon a dostupnost důležitých aplikací. I když jsou skoro stejné sady funkcí hello, jsou tyto dvě úrovně zasílání zpráv Service Bus navrženou tooserve použití v odlišných situacích.

Několik nejvýraznějších rozdílů jsou vyznačené na hello následující tabulka.

| Premium | Standard |
| --- | --- |
| Vysoká propustnost |Variabilní propustnost |
| Předvídatelný výkon |Variabilní latence |
| Pevné ceny |Variabilní průběžná cena  |
| Možnost tooscale zatížení nahoru či dolů |Není k dispozici |
| Velikost zprávy až too1 MB |Velikost zprávy až too256 KB |

**Zasílání zpráv úrovně Premium Service Bus** nabízí izolaci prostředků procesoru a paměti úrovni hello tak, aby každá úloha zákazníka běží izolovaně. Kontejner prostředků se nazývá *jednotka zasílání zpráv*. Každému prémiovému obor názvů se přiřadí aspoň jedna jednotka zasílání zpráv. Pro každý obor názvů Service Bus Premium můžete koupit 1, 2 nebo 4 jednotky zasílání zpráv. Jedna úloha nebo entita může zabírat několik jednotek zasílání zpráv a hello počet jednotek zasílání zpráv můžete změnit podle libosti, ale fakturuje se podle 24hodinoví / denní sazby. Hello výsledkem je předvídatelný a Opakovatelný výkon vašeho řešení postaveného na Service Bus.

Vedle toho, že je tento výkon předvídatelnější, je také rychlejší. Zasílání zpráv úrovně Premium Service Bus je založený na stroji úložiště hello byla zavedená v [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). S zasílání zpráv úrovně Premium je výkon ve špičce mnohem rychlejší než s hello úrovně Standard.

## <a name="premium-messaging-technical-differences"></a>Technické rozdíly zasílání zpráv na úrovni Premium

Hello následující části popisují několik rozdílů mezi Premium a Standard úrovněmi zasílání zpráv.

### <a name="partitioned-queues-and-topics"></a>Dělené fronty a témata

Dělené fronty a témata se podporují v zasílání zpráv úrovně Premium; ve skutečnosti jsou tyto entity vždy dělené (a nelze je vypnout). Ale Premium oddíly fronty a témata nefungují hello stejným způsobem jako v hello úrovních Standard a Basic zasílání zpráv Service Bus. Premium zasílání zpráv nemá SQL jako úložiště dat a již má hello možné prostředků soutěže přidružené na sdílené platformě. V důsledku toho vytváření oddílů není nutné tooimprove výkonu. Kromě toho počet oddílů hello byl změněn z 16 oddíly too2 standardní zasílání zpráv na úrovni Premium. Má dva oddíly zajišťují dostupnost, je to vhodnější počet pro prostředí Permium hello. 

S zasílání zpráv úrovně Premium, když zadáte hello velikost entity s [MaxSizeInMegabytes](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxsizeinmegabytes#Microsoft_ServiceBus_Messaging_QueueDescription_MaxSizeInMegabytes), že velikost je rovnoměrně rozdělit do oddílů hello 2, na rozdíl od [Standard segmentované entity](service-bus-partitioning.md#standard) v které hello Celková velikost je 16 časy hello zadaná velikost. 

Další informace o dělení najdete v oddílu [Dělené fronty a témata](service-bus-partitioning.md).

### <a name="express-entities"></a>Expresní entity

Protože zasílání zpráv úrovně Premium běží v kompletně izolovaném prostředí, nejsou expresní entity v oborech názvů úrovně Premium podporované. Další informace o hello expresní funkci najdete v tématu hello [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) vlastnost.

Pokud máte kód spuštěný pod standardní tooport zasílání zpráv a chcete ho úroveň Premium toohello, ujistěte se, zda text hello [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) vlastnost je nastavena příliš**false** (hello výchozí hodnota).

## <a name="get-started-with-premium-messaging"></a>Začínáme se zasíláním zpráv na úrovni Premium

Začínáme se službou zasílání zpráv úrovně Premium je jednoduchý a hello proces je podobný toothat standardní zasílání zpráv. Nejdřív [vytvořte obor názvů](service-bus-create-namespace-portal.md). Zkontrolujte, že jste v části **Cenová úroveň** vybrali **Premium**.

![create-premium-namespace][create-premium-namespace]

Můžete také vytvářet [obory názvů Premium pomocí šablon Azure Resource Manageru](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Další kroky

toolearn Další informace o zasílání zpráv Service Bus, najdete v následujících tématech hello.

* [Představení zasílání zpráv Azure Service Bus úrovně Premium (příspěvek na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Představení zasílání zpráv Azure Service Bus úrovně Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Přehled zasílání zpráv Service Bus](service-bus-messaging-overview.md)
* [Jak toouse fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
