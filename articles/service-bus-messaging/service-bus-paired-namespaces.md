---
title: "obory názvů spárovat aaaAzure Service Bus | Microsoft Docs"
description: "Podrobnosti implementace spárované obor názvů a náklady"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: 4c44b2b95d2228e1ad8075b52634d88a1593d3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Spárovat podrobnosti implementace obor názvů a náklady dopad
Hello [PairNamespaceAsync] [ PairNamespaceAsync] metoda, použití [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instance, provádí viditelné úlohy vaším jménem. Vzhledem k tomu, že jsou náklady aspekty při použití funkce hello, je užitečné toounderstand ty úlohy, aby očekáváte hello chování při Odehrává se. Hello API zapojí hello následující automatické chování vaším jménem:

* Vytvoření nevyřízených položek fronty.
* Vytvoření [MessageSender] [ MessageSender] objekt, který komunikuje tooqueues nebo témata.
* Když entity přenosu zpráv stane nedostupným, odešle zprávy ping toohello entity v pokusu o toodetect při dané entity opět k dispozici.
* Volitelně vytvoří sadu "čerpadel zpráva", přesuňte zprávy z hello nevyřízené položky fronty toohello primární fronty.
* Koordinuje ukončovací nebo chybující Dobrý den, primární a sekundární [MessagingFactory] [ MessagingFactory] instance.

Na vysoké úrovni, hello funkce funguje takto: primární entita hello je v pořádku, dojde k žádné změny chování. Když hello [FailoverInterval] [ FailoverInterval] po uplynutí předem doba trvání, a žádné úspěšné zasílá hello primární entity se zobrazí po bez pouze dočasné [MessagingException] [ MessagingException] nebo [TimeoutException][TimeoutException], dojde k hello následující chování:

1. Odešlete operations toohello primární entity jsou zakázány a příkazy ping systému hello hello primární entity, dokud příkazy ping mohou být zajišťovány úspěšně.
2. Je vybrán náhodných nevyřízené položky fronty.
3. [BrokeredMessage] [ BrokeredMessage] objekty jsou směrované toohello vybrali nevyřízené položky fronty.
4. Pokud se nezdaří toohello operaci odeslání vybrali nevyřízené položky fronty, této fronty pocházejí z hello otočení a je vybraná novou frontu. Všech uživatelů na hello [MessagingFactory] [ MessagingFactory] další instance hello selhání.

Následující obrázky Hello zobrazit v ní hello pořadí. Nejprve hello odesílatel odešle zprávy.

![Spárované obory názvů][0]

Při selhání toosend toohello primární fronty začne odesílatel hello odesílání zpráv tooa náhodně vybrali nevyřízené položky fronty. Současně spustí úlohu příkazu ping.

![Spárované obory názvů][1]

V tomto okamžiku hello zprávy jsou stále v hello sekundární fronty a nebyly dodány toohello primární fronty. Jakmile hello primární fronta je v pořádku znovu, by měl běžet hello Trativod alespoň jeden proces. Hello Trativod přináší hello zprávy ze všech hello různé nevyřízených položek fronty toohello správné cílové entity (fronty a témata).

![Spárované obory názvů][2]

Hello zbývající část tohoto tématu popisuje hello konkrétní podrobnosti o fungování tyto údaje.

## <a name="creation-of-backlog-queues"></a>Vytvoření nevyřízených položek fronty
Hello [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] objekt předaný toohello [PairNamespaceAsync] [ PairNamespaceAsync] metoda označuje hello počet nevyřízených položek fronty, které chcete toouse. Každou frontu nevyřízených položek se potom vytvoří s hello následující vlastnosti explicitně nastavit (všechny ostatní hodnoty jsou nastaveny toohello [QueueDescription] [ QueueDescription] výchozí nastavení):

| Cesta | [primární obor názvů] / x-servicebus přenos / [index] kde [index] je hodnota [0, BacklogQueueCount) |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| maxDeliveryCount |int. MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue, což |
| AutoDeleteOnIdle |TimeSpan.MaxValue, což |
| Trvání uzamčení |1 minuta |
| EnableDeadLetteringOnMessageExpiration |Hodnota TRUE |
| EnableBatchedOperations |Hodnota TRUE |

Například vytvořen hello první nevyřízených položek fronty pro obor názvů **contoso** jmenuje `contoso/x-servicebus-transfer/0`.

Při vytváření front hello, hello kód nejprve toosee ověří, zda existuje takový fronty. Pokud hello fronta neexistuje, je vytvářena fronta hello. Kód Hello nemá vyčištění "výběr" nevyřízené položky fronty. Konkrétně, pokud hello aplikace s hello primární obor názvů **contoso** požadavky pět nevyřízené položky fronty, ale nevyřízené položky fronty s cestou hello `contoso/x-servicebus-transfer/7` existuje, že navíc nevyřízené položky fronty stále přítomen, ale nepoužívá. systém Hello explicitně povolí tooexist navíc nevyřízené položky fronty, která nebyla použita. Jako vlastník hello obor názvů je zodpovědná za vyčištění žádné nepoužité nebo nežádoucí nevyřízené položky fronty. Hello důvod pro toto rozhodnutí je, že Service Bus nemůže vědět, jaké účely existovat pro všechny fronty hello v oboru názvů. Kromě toho pokud existuje se zadaným názvem hello fronty, ale nesplňuje hello předpokládá, že [QueueDescription][QueueDescription], pak jsou vaše důvody pro vlastní Změna výchozího chování hello. Žádné záruky jsou vytvářeny pro úpravy toohello nevyřízené položky fronty podle vašeho kódu. Ujistěte se, že tootest změny důkladně.

## <a name="custom-messagesender"></a>Vlastní MessageSender
Při odesílání, všechny zprávy projít interní [MessageSender] [ MessageSender] objekt, který se chovat normálně když všechno funguje a přesměruje toohello nevyřízené položky fronty při věcí "break." Po přijetí selhání bez přechodná, spustí časovač. Po [časový interval] [ TimeSpan] období skládající se z hello [FailoverInterval] [ FailoverInterval] hodnotu vlastnosti, během které se odesílají žádné úspěšné zprávy , provozuje hello převzetí služeb při selhání. V tomto okamžiku hello následující umět poradit pro každou entitu:

* Spustí úlohu ping každých [PingPrimaryInterval] [ PingPrimaryInterval] toocheck Pokud hello entita je k dispozici. Jakmile tato úloha úspěšná, všechny klientský kód, který používá hello entitu okamžitě spustí, odeslání nové zprávy toohello primární oboru názvů.
* Budoucí požadavky toosend toothat stejné entity z jiní odesílatelé bude mít za následek hello [BrokeredMessage] [ BrokeredMessage] odesílány toobe upravit toosit ve frontě hello nevyřízených položek. Změna Hello odebere hello některé vlastnosti [BrokeredMessage] [ BrokeredMessage] objektu a ukládá je jinde. Hello následující vlastnosti jsou vymazána a přidají pod nový alias, povolení služby Service Bus a hello SDK tooprocess zprávy jednotně:

| Původní název vlastnosti | Nový název vlastnosti |
| --- | --- |
| ID relace |x-ms-ID relace |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-path |

původní cílovou cestu Hello je také uloženo ve zprávě hello jako vlastnost s názvem x-ms-path. Tento návrh umožňuje zprávy pro mnoho toocoexist entity v jedné nevyřízené položky fronty. Vlastnosti Hello přeložen zpět Trativod hello.

Hello vlastní [MessageSender] [ MessageSender] objekt může dojít k potížím při zprávy přístupu limit 256 KB hello a využívána převzetí služeb při selhání. Hello vlastní [MessageSender] [ MessageSender] objekt ukládá zprávy pro všechny fronty a témata společně v hello nevyřízené položky fronty. Tento objekt zkombinuje zprávy z mnoha základní barvy společně v rámci hello nevyřízené položky fronty. toohandle Vyrovnávání zatížení mezi mnoho klientů, které neznáte každý jiných, hello SDK náhodně vybere jeden nevyřízených položek fronty pro každou [QueueClient] [ QueueClient] nebo [TopicClient] [ TopicClient] vytvoříte v kódu.

## <a name="pings"></a>Příkazy ping
Zprávu ping je prázdná [BrokeredMessage] [ BrokeredMessage] s jeho [ContentType] [ ContentType] nastavena tooapplication/vnd.ms-servicebus-ping a [TimeToLive] [ TimeToLive] hodnota 1 sekunda. Tento příkaz ping má jeden speciální vlastnosti v Service Bus: hello server nikdy poskytuje příkazu ping, když si vyžádá všechny volající [BrokeredMessage][BrokeredMessage]. Proto není nutné toolearn jak tooreceive a tyto zprávy ignorovat. Každá entita (jedinečný fronta nebo téma) za [MessagingFactory] [ MessagingFactory] instance pro každý klient bude příkazu ping zkontrolujte dosažitelnost při jsou považovány za toobe není k dispozici. Ve výchozím nastavení k tomu dojde jednou za minutu. Zprávy ping jsou považovány za toobe regulární zpráv Service Bus a může mít za následek poplatky za zprávy a šířky pásma. Jakmile hello klientům zjišťovat, že systém hello je k dispozici, zastavte hello zprávy.

## <a name="hello-syphon"></a>Trativod Hello
Alespoň jeden spustitelný program v aplikaci hello by měl aktivně běžet Trativod hello. Hello Trativod provádí s dlouhým přijímat dotazování, které trvá po 15 minut. Když jsou k dispozici všechny entity a máte 10 nevyřízené položky fronty, hello aplikace, který je hostitelem volání Trativod hello hello přijímat operaci 40 časy za hodinu, 960 časy denně a časy 28 800 bitů za 30 dní. Při Trativod hello je aktivně přesunu zprávy z primární fronty toohello hello nevyřízených položek, každá zpráva vyskytne hello následující poplatky (standardní poplatky za velikost zprávy a šířky pásma použít ve všech fázích):

1. Odešlete toohello nevyřízených položek.
2. Přijímat z hello nevyřízených položek.
3. Odešlete toohello primární.
4. Přijímat z primární hello.

## <a name="closefault-behavior"></a>Zavřít nebo selhání chování
V rámci aplikace, který je hostitelem hello Trativod, jednou hello primární nebo sekundární [MessagingFactory] [ MessagingFactory] chyb nebo je uzavřena bez jeho partnera také dochází k chybě nebo k uzavření a hello Trativod zjistí tento stav , hello Trativod funguje. Pokud hello dalších [MessagingFactory] [ MessagingFactory] není uzavřený během 5 sekund, bude hello Trativod poruch stále otevřete hello [MessagingFactory] [ MessagingFactory] .

## <a name="next-steps"></a>Další kroky
V tématu [asynchronní vzory a vysoká dostupnost pro zasílání zpráv] [ Asynchronous messaging patterns and high availability] pro podrobnou diskuzi o asynchronní zasílání zpráv Service Bus. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
