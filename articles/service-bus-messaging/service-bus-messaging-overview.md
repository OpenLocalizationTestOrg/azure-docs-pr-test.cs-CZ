---
title: "Přehled přenosu zpráv Service Bus aaaAzure | Microsoft Docs"
description: "Popis přenosu zpráv ve službě Service Bus a Azure Relay"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: ede2904e11544d8f9428a2d657dcc77dacd95ac4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-hello-cloud"></a>Zasílání zpráv Service Bus: flexibilní přenos dat v cloudu hello
Microsoft Azure Service Bus je spolehlivá služba pro přenos informací. účelem Hello této služby je snazší toomake komunikace. Pokud dvě nebo více stran, aby byly informace tooexchange, musí zprostředkovatel komunikace. Služba Service Bus je mechanizmus pro komunikaci zprostředkovanou přes třetí stranu. Toto je podobné tooa zásilkových služeb fyzické hello, world. Díky poštovním službám velmi snadno toosend nejrůznější dopisy a balíky s určitými zárukami, kdekoli v hello, world.

Podobně jako toohello poštovní služba doručuje dopisy, Service Bus je flexibilní doručení informací od hello odesílatele i příjemce hello. Hello zasílání zpráv service zajistí, že hello informace doručily i v případě hello dvě strany nejsou online na hello stejný čas, nebo pokud nejsou k dispozici na hello přesnou stejný čas. Tímto způsobem zasílání zpráv je podobné toosending písmenem, zatímco jiné nezprostředkovaná komunikace je podobné tooplacing telefonní hovor (nebo použití toobe - před čekání a volající volání ID, které jsou mnohem víc jako zprostředkované zasílání zpráv telefonní hovor).

odesílatele zprávy Hello můžete také potřebovat nejrůznější charakteristiky dodání, včetně transakce, detekci duplikátů, časově omezené a dávkování. I tyto funkce mají předobraz v poště: opakované dodání, dodejka, změna adresy nebo odvolání.

Service Bus podporuje dva rozdílné způsoby přenosu zpráv: *Azure Relay* a *zasílání zpráv Service Bus*.

## <a name="azure-relay"></a>Azure Relay
Hello [WCF předávání](../service-bus-relay/relay-what-is-it.md) součástí předávání přes Azure je služba centralizované (ale vysoce vyváženou zátěží), která podporuje různé přenosové protokoly a standardy webových služeb. Mezi ty patří SOAP, WS-*, a dokonce i REST. Hello [služba předávání](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) poskytuje celou řadu různých možností předávání a může pomoct vyjednat přímé připojení peer-to-peer, když je možné. Service Bus je optimalizovaná pro vývojáře .NET, kteří používají hello Windows Communication Foundation (WCF), i s ohledem tooperformance a použitelnost, a poskytuje plný přístup tooits předávací službě přes rozhraní SOAP a REST. To umožňuje u SOAP nebo REST programovací prostředí toointegrate službou Service Bus.

Hello předávací služba podporuje tradiční jednosměrný zasílání zpráv, požadavků a odpovědí zasílání zpráv a peer-to-peer zasílání zpráv. Taky podporuje distribuci událostí na úrovni Internetu tooenable publikování a odběru scénáře a obousměrnou soketovou komunikací pro zvýšenou point-to-point účinnost. Ve vzoru hello přes předávací službu zasílání zpráv k místní službě toohello předávací služba se připojuje přes odchozí port a vytvoří obousměrný soket pro komunikaci vázanou tooa konkrétní potkávací adresu. Hello Klient pak může komunikovat s místní službou hello odesláním zprávy toohello předávací služba cílení hello potkávací adresa. Hello předávací služba se pak "" zprávy toohello místní služba předávání prostřednictvím hello obousměrnou soketovou již na místě. Hello klient nepotřebuje přímé spojení toohello místní služby, ani je tooknow it vyžaduje, kde hello služba nachází, a hello lokální služba nepotřebuje mít žádné příchozí porty otevřít v bráně firewall hello.

Zahájení hello připojení mezi místní službou a předávací službou hello pomocí skupiny "předávání" vazeb WCF. Pozadí se děje hello hello předávací vazby mapují tootransport vazby prvky určené toocreate komponentů kanálu WCF které se integrují se službou Service Bus v cloudu hello.

Předávání WCF poskytuje řadu výhod, ale vyžaduje hello server a klienta tooboth být online na hello stejný čas v pořadí toosend a přijímat zprávy. Tento způsob není ideální pro komunikaci ve stylu HTTP, ve které hello nemusí být požadavky dlouhou životnost, ani pro klienty, kteří se připojují jen občas, jako jsou prohlížeče, mobilní aplikace a tak dále. Zprostředkované zasílání zpráv podporuje oddělenou komunikaci, která má sama o sobě svoje výhody– klienti a servery se například můžou spojit podle potřeby a provádět své operace asynchronním způsobem.

## <a name="brokered-messaging"></a>Zprostředkované zasílání zpráv
Na rozdíl od toohello předávání schéma Service Bus zasílání zpráv, nebo [zprostředkované zasílání zpráv](service-bus-queues-topics-subscriptions.md) můžete představit jako o asynchronním nebo "časově odděleném". Producenti (odesílatelé) a spotřebitelé (příjemci) nemusí toobe online na hello stejnou dobu. Hello infrastruktury přenosu zpráv spolehlivě uloží zprávy do "zprostředkovatele" (například fronty), dokud spotřebitel hello je připraven tooreceive je. To umožňuje hello součástí hello distribuované aplikace toobe odpojit; například za účelem údržby nebo z důvodu tooa součást havárií, aniž by to ovlivnilo hello celý systém. Kromě toho hello přijímající aplikace může mít pouze toocome online v určitou dobu hello den, jako je například systém pro správu inventáře, který je pouze požadované toorun na konci hello hello pracovního dne.

základní součásti Hello hello Service Bus zprostředkované zasílání zpráv infrastruktury jsou fronty, témata a odběry.  Hello základní rozdíl je, že témata podporují funkce pbulikovat/odebírat, které lze použít pro sofistikované na základě obsahu směrování a logiku, včetně odesílání toomultiple příjemce. Komponenty umožňují nové scénáře pro zasílání zpráv, jako je časové oddělení, publikování/odběr a vyvažování zátěže. Další informace o těchto entitách zasílání zpráv najdete v tématu [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md).

Jako s infrastrukturou hello předávání WCF, hello zprostředkované zasílání zpráv se schopností poskytuje pro programátory v WCF a rozhraní .NET Framework a také přes REST.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o zasílání zpráv Service Bus, najdete v následujících tématech hello.

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak toouse fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak toouse Service Bus témat a odběrů](service-bus-dotnet-how-to-use-topics-subscriptions.md)

