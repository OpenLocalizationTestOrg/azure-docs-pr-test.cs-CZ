---
title: "Azure Service Bus zprávy, datové části a serializace | Microsoft Docs"
description: "Přehled datových částí zpráv Service Bus"
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: sethm
ms.openlocfilehash: 7c18cc4a0e6a8dbf3a47c146707666c5538ff7c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="messages-payloads-and-serialization"></a>Zprávy, datové části a serializace

Microsoft Azure Service Bus zpracovává zprávy. Zprávy provádění datové části a také metadata, a to ve formě dvojice klíč hodnota vlastnosti, popisující datové části a poskytnete zpracování pokyny k Service Bus a aplikacím. Občas stát že metadata samostatně jsou dostatečná k provedení informace, které odesilatel vyžaduje pro komunikaci se příjemci a datové části zůstane prázdný.

Objektový model pro rozhraní .NET a Javu oficiální klienty služby Service Bus projeví abstraktní struktura zpráv Service Bus, která se mapuje na a z přenosu protokolů, které podporuje Service Bus.
 
Zprávy služby Service Bus se skládá z binární datová část oddílu, který Service Bus nikdy zpracovává všechny formuláře na straně služby a dvě sady vlastností. *Zprostředkovatel vlastnosti* jsou předdefinované v systému. Tyto předdefinované vlastnosti buď řídit úroveň zprávy funkce uvnitř zprostředkovatele nebo jsou mapovány na běžné a standardizovaném metadata položky. *Vlastnosti uživatele* jsou kolekce páry klíč hodnota, které mohou být definovaný a nastavení aplikací.
 
V následující tabulce jsou uvedeny vlastnosti předdefinované zprostředkovatele. Názvy jsou použity s všechny rozhraní API oficiální klienta a taky [BrokerProperties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Properties_) objekt JSON mapování protokolu HTTP.
 
Ekvivalentní názvů používaných na úrovni protokolu AMQP se uvádějí v závorkách. 

| Název vlastnosti                         | Popis                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  [ContentType](/dotnet/api/microsoft.azure.servicebus.message.contenttype) (typu obsahu)           | Volitelně popisuje datovou část zprávy, s popisovačem následující formát RFC2045, část 5, například `application/json`.                                                                                                                                                                                                                                                                                             |
|  [CorrelationId](/dotnet/api/microsoft.azure.servicebus.message.correlationid#Microsoft_Azure_ServiceBus_Message_CorrelationId) (id korelace)       | Umožňuje aplikaci určit kontext pro zprávu pro účely korelace, například odrážející **MessageId** zprávy, který je právě odpověděl.                                                                                                                                                                                                                                                                  |
| [DeadLetterSource](/dotnet/api/microsoft.azure.servicebus.message.deadlettersource)                      | Nastavte pouze v zprávy, které byly lettered zpráv a následně automaticky předána z fronty nedoručených zpráv k jiné entitě. Určuje entity, ve kterém byla zpráva lettered zpráv. Tato vlastnost je jen pro čtení.                                                                                                                                                                                                                                  |
| [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deliverycount)                         | Počet doručení, které se pokusil pro tuto zprávu. Hodnota tohoto čítače se zvýší, když vyprší platnost zámek zpráva nebo zpráva explicitně opuštění příjemce. Tato vlastnost je jen pro čtení.                                                                                                                                                                                                                                                  |
| [EnqueuedSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedsequencenumber)                | Pro zprávy, které byly automaticky předána tato vlastnost odráží pořadové číslo, které měl byla přiřazena první zprávu na jeho původní bod odeslání. Tato vlastnost je jen pro čtení.                                                                                                                                                                                                                                                                |
| [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc)                       | Čas UTC rychlých, kdy byla zpráva přijatá a uložená v entitě. Tato hodnota slouží jako ukazatel čas doručení autoritativní a neutrální při příjemce nechce důvěřovat hodiny odesílatele. Tato vlastnost je jen pro čtení.                                                                                                                                                                                                   |
|  [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) (absolutní čas vypršení platnosti) | Čas UTC rychlých, kdy zpráva je označena pro odstranění a již není k dispozici pro načtení z entity z důvodu vypršení platnosti. Vypršení platnosti řídí **TimeToLive** vlastnost a tato vlastnost se počítá z EnqueuedTimeUtc + TimeToLive. Tato vlastnost je jen pro čtení.                                                                                                                                                                           |
| [ForcePersistence](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence)                      | Pro fronty nebo témata, které mají [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) nastaven, příznak tuto vlastnost lze nastavit k označení, že zpráva musí nastavit jako trvalý, na disku než je potvrzené. Toto je standardní chování pro všechny entity není express.                                                                                                                                                                                                         |
| [Popisek](/dotnet/api/microsoft.azure.servicebus.message.label) (předmět)                       | Tato vlastnost umožňuje aplikaci k označení účel zprávy k příjemce v standardizovaným způsobem, podobně jako řádek předmětu e-mailu.                                                                                                                                                                                                                                                                                  |
| [LockedUntilUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.lockeduntilutc)                        | Pro zprávy přijaté v části o zámku (lock funkce Náhled přijímat režimu, předem nevyrovnaná) Tato vlastnost odráží uzamčena rychlých UTC, do kterého je uchovávat zprávy do fronty předplatného. Když zámek vyprší, [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deliverycount) se zvýší a opět k dispozici pro načtení zprávy. Tato vlastnost je jen pro čtení.                                                                                                                         |
| [LockToken](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.locktoken)                             | Token zámku je odkaz na zámek, který čeká zprostředkovatelem v *funkce Náhled zámku* přijímat režimu. Token umožňuje připnout trvale pomocí zámku [odložení](message-deferral.md) rozhraní API a který, provést zprávu mimo tok stavu regulární doručení. Tato vlastnost je jen pro čtení.                                                                                                                                                               |
| [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) (id zprávy)                | Identifikátor zprávy je hodnota definované aplikací, která jednoznačně identifikuje zprávu a jeho datovou část. Identifikátor je řetězec volného formátu a můžou odrážet identifikátor GUID nebo identifikátor odvozené z kontextu aplikací. Pokud je povoleno, [duplicitní detekce](duplicate-detection.md) funkce identifikuje a odebírá sekundu a další odesílání zpráv se stejným **MessageId**.                                                                |
| [Klíč oddílu](/dotnet/api/microsoft.azure.servicebus.message.partitionkey)                          | Pro [segmentované entity](service-bus-partitioning.md), nastavení této hodnoty umožňuje přiřazení související zprávy do stejného interní oddílu, tak, aby se správně zaznamenává odesílání pořadí pořadí. Oddíl je zvolen podle funkce hash přes tuto hodnotu a nemůže být zvolena přímo. Pro relaci využívající entity **SessionId** vlastnost má přednost před tuto hodnotu.                                                                                                       |
| [ReplyTo](/dotnet/api/microsoft.azure.servicebus.message.replyto) (Komu)                    | Tato hodnota volitelné a definované aplikací je standardní způsob, jak express odpovědi cestu k příjemce zprávy. Když odesílatele očekává odpověď, nastaví hodnotu na absolutní nebo relativní cestu fronta nebo téma se očekává, že odpověď k odeslání.                                                                                                                                           |
| [ReplyToSessionId](/dotnet/api/microsoft.azure.servicebus.message.replytosessionid) (odpovědi skupiny id)  | Tato hodnota rozšiřuje **ReplyTo** informace a určuje, které **SessionId** měli nastavit pro odpověď při odeslání odpovědi entitě.                                                                                                                                                                                                                                                                            |
| [ScheduledEnqueueTimeUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc)               | Pro zprávy, které jsou pouze k dispozici pro načtení po prodlevě definuje tato vlastnost rychlých UTC, kdy se zpráva bude logicky zařazených do fronty, sekvencování a proto k dispozici pro načtení.                                                                                                                                                                                                                 |
| [SequenceNumber –](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber)                        | Pořadové číslo je jedinečné 64bitové celočíselné přiřazen na zprávy, jako je přijatá a uložená zprostředkovatele a funkce jako jeho hodnotu true identifikátoru. Pro dělené entity projeví nejhornější 16 bitů identifikátor oddílu. Pořadová čísla monotónně zvětšují a bez mezer. Jejich přejít na 0 je vyčerpání 48 64bitový rozsahu. Tato vlastnost je jen pro čtení.                                                                |
| [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) (id skupiny)                  | Pro relaci využívající entity tato hodnota definované aplikací Určuje přidružení relace zprávy. Zprávy se stejným identifikátorem relace se vztahují souhrnné zamykání a povolení přesné v pořadí zpracování a demultiplexování. Pro nebere v úvahu relace entity tato hodnota je ignorována.                                                                                                                                     |
| [Velikost](/dotnet/api/microsoft.azure.servicebus.message.size)                                  | Zbývajícímu uložené zprávy v protokolu broker jako počet bajtů, jak se započítává kvótu úložiště. Tato vlastnost je jen pro čtení.                                                                                                                                                                                                                                                                                                       |
| [Stav](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.state)                                 | Označuje stav zprávy v protokolu. Tato vlastnost je relevantní pouze během zpráva o procházení ("funkce Náhled"), chcete-li určit, zda je zpráva "aktivní" a k dispozici, jak dorazí na začátek fronty, ať už je odložení nebo čeká na naplánovat. Tato vlastnost je jen pro čtení.                                                                                                                                           |
| [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive)                            | Tato hodnota je relativní doba, po jejímž uplynutí vyprší platnost zprávy, od pro rychlé zprávy je přijatá a uložená zprostředkovatelem, jak zachytit v **EnqueueTimeUtc**. Pokud není explicitně nastavena, se předpokládá, že hodnota **DefaultTimeToLive** pro příslušné fronta nebo téma. Úroveň zprávy **TimeToLive** hodnota nemůže být delší než entity **DefaultTimeToLive** nastavení a je bezobslužně upravována případě. |
| [K](/dotnet/api/microsoft.azure.servicebus.message.to) (do)                               | Tato vlastnost je vyhrazena pro budoucí použití v scénáře směrování a v současné době ignorovány zprostředkovatelem sám sebe. Aplikace můžete použít tuto hodnotu v řízené pravidlo řetězení scénářích automaticky dopředného udávajících určený logické cíl zprávy.                                                                                                                                                                                   |
| [ViaPartitionKey](/dotnet/api/microsoft.azure.servicebus.message.viapartitionkey)                       | Pokud je odeslána zpráva prostřednictvím fronty přenosu v oboru transakce, tato hodnota vybere oddílu fronty přenosu.                                                                                                                                                                                                                                                                                                                 |

Model abstraktní zpráv umožňuje zprávu odeslat do fronty pomocí protokolu HTTP (ve skutečnosti vždy HTTPS) a mohou být načteny prostřednictvím protokolu AMQP. V obou případech zpráva vypadá normální v rámci příslušného protokolu. Vlastnosti zprostředkovatele jsou převedeny podle potřeby a vlastnosti uživatele jsou namapované na nejvhodnější umístění na modelu zpráva příslušného protokolu. V protokolu HTTP mapovat vlastnosti uživatele přímo do a z hlavičky protokolu HTTP; v protokolu AMQP mapují do a z **vlastnosti aplikace** mapy.

## <a name="message-routing-and-correlation"></a>Směrování a korelace zpráv

Podmnožinu vlastnosti zprostředkovatele popsané dříve, konkrétně [k](/dotnet/api/microsoft.azure.servicebus.message.to), [ReplyTo](/dotnet/api/microsoft.azure.servicebus.message.replyto), [ReplyToSessionId](/dotnet/api/microsoft.azure.servicebus.message.replytosessionid), [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid), [ CorrelationId](/dotnet/api/microsoft.azure.servicebus.message.correlationid), a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), pomáhají aplikace směrování zpráv do konkrétního umístění. Pro znázornění je vezměte v úvahu několik vzorce:

- **Jednoduché požadavek nebo odpověď**: vydavatel odešle zprávu do fronty a očekává odpověď od příjemce zprávy. Pokud chcete získat odpovědi, vlastní vydavatele fronty, do kterého se očekává, že odpoví, který bude doručen. Na adresu této fronty je vyjádřena v odchozí zprávy **ReplyTo** vlastnost. Příjemce odpoví, zkopíruje **MessageId** zpracovávaný zprávy do **CorrelationId** vlastnost zpráva s odpovědí a doručí zprávy do cílové indikován **ReplyTo** vlastnost. Jednu zprávu přispět více odpovědí, v závislosti na kontextu aplikací.
- **Vícesměrového vysílání požadavek nebo odpověď**: vydavatel jako varianta předchozí vzor, odešle zprávu do tématu a více odběrateli nárok využívat zprávy. Každý odběratelů může reagovat způsobem popsané. Tento vzor se používá ve scénářích zjišťování nebo vrácení volání a respondent obvykle identifikuje s vlastností uživatele nebo do datové části. Pokud **ReplyTo** bodů do tématu, sada zjišťování odpovědí lze distribuovat cílové skupině.
- **Multiplexní**: Tato funkce relace umožňuje multiplexní streamů související zprávy prostřednictvím jedné frontě nebo předplatného, tak, aby každý relace (nebo skupiny) související zpráv identifikovaný odpovídající **SessionId**hodnoty, jsou směrovány do konkrétní příjemce příjemce drží relaci pod zámku. Další informace o podrobnosti o relací [zde](message-sessions.md).
- **Multiplexní požadavek nebo odpověď**: Tato funkce relace umožňuje multiplexní odpovědi, což několik vydavatele sdílení fronty odpovědí. Nastavením **ReplyToSessionId**, vydavatele určit, aby consumer(s) zkopírovat tuto hodnotu do **SessionId** vlastnost zprávy odpovědi. Publikování fronta nebo téma neobsahuje relace vědět. Jak je zpráva odeslána, vydavatele můžete konkrétně počkejte relaci s danou **SessionId** k vyhodnotit pro frontu podmíněně přijetím příjemce relace. 

Směrování v rámci oboru názvů Service Bus může být dosaženo pomocí automatického předání řetězení a pravidel odběru tématu. Směrování mezi obory názvů, může být dosaženo [pomocí Azure LogicApps](https://azure.microsoft.com/services/logic-apps/). Jak je uvedeno v předchozím seznamu, **k** vlastnost je rezervovaná pro budoucí použití a může server interpretovat zprostředkovatele s speciálně povolené funkce. Aplikace, které chcete implementovat směrování by se tak na základě vlastností uživatele a není štíhlé na **k** vlastnost; však nebude tak teď způsobit problémy s kompatibilitou.

## <a name="payload-serialization"></a>Datová část serializace

Při přenosu nebo uložené v rámci služby Service Bus, datové části je vždy bloku neprůhledné, binární. [ContentType](/dotnet/api/microsoft.azure.servicebus.message.contenttype) vlastnost umožňuje aplikacím popisují datové části s doporučený formát pro hodnoty vlastností se popis obsah typu MIME podle IETF RFC2045; například `application/json;charset=utf-8`.

Na rozdíl od variant Java nebo .NET Standard, rozhraní .NET Framework verze rozhraní API Service Bus podporuje vytváření **BrokeredMessage** instance předáním libovolné objekty .NET do konstruktoru. 

Pokud používáte starší verzi protokolu SBMP, tyto objekty jsou pak serializovat výchozí binární serializátor nebo s serializátoru, který se dodává externě. Pokud používáte protokol AMQP, je objekt serializován do objektu AMQP. Příjemce může načíst tyto objekty se [GetBody<T>()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) metodu očekávaný typ; s AMQP, se objekty serializují do AMQP graf **ArrayList** a **IDictionary < string, object >** objekty a může dekódovat pomocí libovolného klienta protokolu AMQP. 

Při této magic skrytá serializace je vhodné, aplikace by měla explicitní řídit serializace objektu a zapnout jejich grafy objektu do datové proudy před zahrnutím je do zprávy a opačný na straně příjemce. To dává umožňuje vzájemnou spolupráci výsledky. Musí být také poznamenat, že při AMQP má model efektivní binární kódování, je vázaný na ekosystému AMQP zasílání zpráv a klientů protokolu HTTP bude mít potíže při dekódování takové datové části. 

Obecně doporučujeme JSON a Apache Avro jako datové části formátů pro strukturovaná data.

Variant .NET Standard a Java API přijímají pouze pole bajtů, což znamená, že aplikace musí zpracovat řízení serializace objektu. 

## <a name="next-steps"></a>Další kroky

Další informace o zasílání zpráv Service Bus, najdete v následujících tématech:

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)