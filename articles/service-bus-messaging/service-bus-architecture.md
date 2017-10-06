---
title: "zpracování přehled architektury zpráv Service Bus aaaAzure | Microsoft Docs"
description: "Popisuje architekturu zpracování zpráv hello Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: f7606e40cdf6db3797a0db2de9365453ff2a158e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-architecture"></a>Architektura služby Service Bus
Tento článek popisuje architekturu zpracování zpráv hello Azure Service Bus.

## <a name="service-bus-scale-units"></a>Jednotky škálování služby Service Bus
Service Bus se organizuje podle *jednotek škálování*. Jednotka škálování je jednotka nasazení a obsahuje všechny součásti požadované hello spuštění služby. Každá oblast nasadí jednu nebo víc jednotek škálování služby Service Bus.

Obor názvů sběrnice je tooa mapované jednotky škálování. Hello jednotka škálování zpracovává všechny typy entit služby Service Bus (fronty, témata, odběry). Jednotka škálování služby Service Bus se skládá z hello následující součásti:

* **Sada uzlů brány.** Uzly brány ověřují příchozí požadavky. Každý uzel brány má veřejnou IP adresu.
* **Sada zprostředkovatelských uzlů pro přenos zpráv.** Zprostředkovatelské uzly pro přenos zpráv zpracovávají požadavky týkající se entit přenos zpráv.
* **Jedno úložiště brány.** Hello úložiště brány uchovává data hello pro každou entitu, která je definována v této jednotce škálování. Hello úložiště brány se implementuje nad databázi SQL Azure.
* **Několik úložišť pro přenos zpráv.** Přenos zpráv uchovává zprávy hello všechny fronty, témata a odběry, které jsou definovány v této jednotce škálování. Taky obsahuje všechna data odběru. Pokud [dělení entit pro zasílání zpráv](service-bus-partitioning.md) je povoleno, fronta nebo téma je namapované tooone úložišti pro přenos zpráv. Odběry jsou uložené ve hello stejném úložišti pro přenos zpráv jako jejich nadřazené téma. S výjimkou služby Service Bus [zasílání zpráv úrovně Premium](service-bus-premium-messaging.md), hello úložiště pro zasílání zpráv implementují nad databáze SQL Azure.

## <a name="containers"></a>Kontejnery
Každé entitě přenosu zpráv je přiřazený specifický kontejner. Kontejner je logická konstrukce, která používá právě jeden zasílání zpráv toostore úložiště všechny relevantní data pro tento kontejner. Každý kontejner je přiřazený tooa zprostředkovatelský uzel pro zasílání zpráv. Typicky je víc kontejnerů než zprostředkovatelských uzlů přenosu zpráv. Každý zprostředkovatelský uzel služeb načte několik kontejnerů. Hello distribuce kontejnerů tooa zprostředkovatelský uzel přenosu zpráv je organizovaná tak, aby všechny zprostředkovatelské uzly přenosu zpráv byly rovnoměrně zatížené. Pokud hello hello zatížení vzor změny (například jeden z hello kontejnerů začne být velmi vytížený) nebo pokud zprostředkovatelský uzel přenosu zpráv dočasně nedostupný, kontejnery se předistribuují mezi hello zprostředkovatelských uzlů přenosu zpráv.

## <a name="processing-of-incoming-messaging-requests"></a>Zpracování příchozích událostí přenosu zpráv
Když klient odešle požadavek tooService sběrnice, hello Vyrovnávání zatížení Azure ho přesměruje tooany uzlů brány hello. Hello uzel brány ověří požadavek hello. Pokud hello požadavek týká entity přenosu zpráv (fronty, tématu, odběru), uzel brány hello vyhledává hello entity v úložišti brány hello a určí, ve které zasílání zpráv hello úložiště se entita nachází. Potom vyhledá, který zprostředkovatelský uzel se aktuálně stará o tento kontejner a odešle toothat požadavek hello zprostředkovatelský uzel pro zasílání zpráv. Hello zprostředkovatelský uzel pro zasílání zpráv zpracuje požadavek hello a aktualizuje stav entity hello v úložišti kontejneru hello. Hello zprostředkovatelský uzel zasílání zpráv pak odešle hello odpověď zpět toohello brány uzlu, který odešle klientem odpovídající odpověď zpět toohello této vydané hello původní žádosti.

![Zpracování Příchozích událostí přenosu zpráv](./media/service-bus-architecture/ic690644.png)

## <a name="next-steps"></a>Další kroky
Teď, když jste si přečetli Přehled architektury služby Service Bus, najdete na adrese hello následující odkazy na další informace:

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Řešení zasílání zpráv ve frontě pomocí front Service Bus](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)


