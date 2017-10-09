---
title: aaaAvailability a konzistenci v Azure Event Hubs | Microsoft Docs
description: "Jak tooprovide hello maximální velikost dostupnosti a je konzistentní s používáním Azure Event Hubs oddíly."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a8ededaae1589830da21cb8910ca694d2d628bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a>Dostupnost a konzistence v Event Hubs

## <a name="overview"></a>Přehled
Používá Azure Event Hubs [dělení modelu](event-hubs-features.md#partitions) tooimprove dostupnosti a paralelního zpracování v rozbočovači jedna událost. Například pokud centra událostí má čtyři oddíly, a jeden z těchto oddílů je přesunout z jednoho serveru tooanother v operaci Vyrovnávání zatížení, můžete i nadále odesílat a přijímat z tři oddíly. Kromě toho s více oddíly vám umožní toohave více souběžných čtenářů zpracování dat, vylepšení agregovanou propustnost. Vysvětlení důsledků hello vytváření oddílů a řazení v distribuované systému je zásadní aspekt návrhu řešení.

toohelp vysvětlují hello kompromis mezi řazení a dostupnosti najdete v tématu hello [věta CAP](https://en.wikipedia.org/wiki/CAP_theorem), označované také jako věta Brewer společnosti. Tato věta popisuje hello volba mezi konzistencí, dostupnost a odolnost proti oddílu.

Na Brewer věta definuje konzistence a dostupnost následujícím způsobem:
* Oddílu tolerance: hello schopnost toocontinue systému zpracování dat, zpracování dat i v případě, že dojde k selhání oddílu.
* Dostupnost: bez selhání uzlu vrátí přiměřené odpověď během přiměřeně krátké doby (bez chyb nebo vypršení časových limitů).
* Konzistence: pro čtení je zaručeno tooreturn hello poslední zápis pro daného klienta.

## <a name="partition-tolerance"></a>Tolerance oddílu
Služba Event Hubs je postavená na oddílů datový model. Během instalace můžete nakonfigurovat hello počet oddílů v Centru událostí, ale tuto hodnotu nelze změnit později. Vzhledem k tomu, že oddíly musí používat službou Event Hubs, máte toomake rozhodnutí o dostupnosti a konzistence pro vaši aplikaci.

## <a name="availability"></a>Dostupnost
Hello tooget nejjednodušší způsob, jak začít s Event Hubs je toouse hello výchozí chování. Pokud vytvoříte novou `EventHubClient` objektu a používat hello `Send` metody událostí je automaticky distribuovaná mezi oddílů v Centru událostí. Toto chování umožňuje hello největší množství provoz.

Tento model pro případy použití, které vyžadují hello maximální doba provozu, je upřednostňovaný.

## <a name="consistency"></a>Konzistence
V některých případech může být hello řazení událostí důležité. Například můžete váš back-end systému tooprocess příkazy aktualizace před příkaz delete. V tomto případě můžete buď nastavit klíč oddílu hello na události, nebo použijte `PartitionSender` objektu tooonly odesílat události tooa určité oddílu. Tím zajistíte, že když tyto události se načítají z oddílu hello, jejich přečtení v pořadí.

Pomocí této konfigurace Uvědomte si, že toowhich hello konkrétní oddíl, odesílání není k dispozici, obdržíte chybnou odpověď. Jako bod porovnání Pokud nemáte tooa jeden oddíl spřažení, odešle hello služby Event Hubs vaše události toohello další dostupný oddíl.

Jeden z možných řešení tooensure řazení při také maximalizace provoz, bude tooaggregate události v rámci událost zpracování aplikace. Nejjednodušší způsob, jak tooaccomplish Hello to je toostamp událost s vlastnost počtu vlastní pořadí. Hello následující kód ukazuje příklad:

```csharp
// Get hello latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

Tento příklad odešle vaše události tooone hello dostupných oddílů v Centru událostí a nastaví hello odpovídající pořadové číslo z vaší aplikace. Toto řešení vyžaduje toobe stavu uchovává aplikace zpracování, ale dává vaší odesílatelé koncový bod, který je pravděpodobnější toobe k dispozici.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
