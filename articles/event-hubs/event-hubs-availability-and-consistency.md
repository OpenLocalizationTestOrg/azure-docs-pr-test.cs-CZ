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
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="d8aa8-103">Dostupnost a konzistence v Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d8aa8-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="d8aa8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d8aa8-104">Overview</span></span>
<span data-ttu-id="d8aa8-105">Používá Azure Event Hubs [dělení modelu](event-hubs-features.md#partitions) tooimprove dostupnosti a paralelního zpracování v rozbočovači jedna událost.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) tooimprove availability and parallelization within a single event hub.</span></span> <span data-ttu-id="d8aa8-106">Například pokud centra událostí má čtyři oddíly, a jeden z těchto oddílů je přesunout z jednoho serveru tooanother v operaci Vyrovnávání zatížení, můžete i nadále odesílat a přijímat z tři oddíly.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server tooanother in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="d8aa8-107">Kromě toho s více oddíly vám umožní toohave více souběžných čtenářů zpracování dat, vylepšení agregovanou propustnost.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-107">Additionally, having more partitions enables you toohave more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="d8aa8-108">Vysvětlení důsledků hello vytváření oddílů a řazení v distribuované systému je zásadní aspekt návrhu řešení.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-108">Understanding hello implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="d8aa8-109">toohelp vysvětlují hello kompromis mezi řazení a dostupnosti najdete v tématu hello [věta CAP](https://en.wikipedia.org/wiki/CAP_theorem), označované také jako věta Brewer společnosti.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-109">toohelp explain hello trade-off between ordering and availability, see hello [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="d8aa8-110">Tato věta popisuje hello volba mezi konzistencí, dostupnost a odolnost proti oddílu.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-110">This theorem discusses hello choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="d8aa8-111">Na Brewer věta definuje konzistence a dostupnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d8aa8-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="d8aa8-112">Oddílu tolerance: hello schopnost toocontinue systému zpracování dat, zpracování dat i v případě, že dojde k selhání oddílu.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-112">Partition tolerance: hello ability of a data processing system toocontinue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="d8aa8-113">Dostupnost: bez selhání uzlu vrátí přiměřené odpověď během přiměřeně krátké doby (bez chyb nebo vypršení časových limitů).</span><span class="sxs-lookup"><span data-stu-id="d8aa8-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="d8aa8-114">Konzistence: pro čtení je zaručeno tooreturn hello poslední zápis pro daného klienta.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-114">Consistency: a read is guaranteed tooreturn hello most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="d8aa8-115">Tolerance oddílu</span><span class="sxs-lookup"><span data-stu-id="d8aa8-115">Partition tolerance</span></span>
<span data-ttu-id="d8aa8-116">Služba Event Hubs je postavená na oddílů datový model.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="d8aa8-117">Během instalace můžete nakonfigurovat hello počet oddílů v Centru událostí, ale tuto hodnotu nelze změnit později.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-117">You can configure hello number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="d8aa8-118">Vzhledem k tomu, že oddíly musí používat službou Event Hubs, máte toomake rozhodnutí o dostupnosti a konzistence pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-118">Since you must use partitions with Event Hubs, you have toomake a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="d8aa8-119">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d8aa8-119">Availability</span></span>
<span data-ttu-id="d8aa8-120">Hello tooget nejjednodušší způsob, jak začít s Event Hubs je toouse hello výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-120">hello simplest way tooget started with Event Hubs is toouse hello default behavior.</span></span> <span data-ttu-id="d8aa8-121">Pokud vytvoříte novou `EventHubClient` objektu a používat hello `Send` metody událostí je automaticky distribuovaná mezi oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-121">If you create a new `EventHubClient` object and use hello `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="d8aa8-122">Toto chování umožňuje hello největší množství provoz.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-122">This behavior allows for hello greatest amount of up time.</span></span>

<span data-ttu-id="d8aa8-123">Tento model pro případy použití, které vyžadují hello maximální doba provozu, je upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-123">For use cases that require hello maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="d8aa8-124">Konzistence</span><span class="sxs-lookup"><span data-stu-id="d8aa8-124">Consistency</span></span>
<span data-ttu-id="d8aa8-125">V některých případech může být hello řazení událostí důležité.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-125">In some scenarios, hello ordering of events can be important.</span></span> <span data-ttu-id="d8aa8-126">Například můžete váš back-end systému tooprocess příkazy aktualizace před příkaz delete.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-126">For example, you may want your back-end system tooprocess an update command before a delete command.</span></span> <span data-ttu-id="d8aa8-127">V tomto případě můžete buď nastavit klíč oddílu hello na události, nebo použijte `PartitionSender` objektu tooonly odesílat události tooa určité oddílu.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-127">In this instance, you can either set hello partition key on an event, or use a `PartitionSender` object tooonly send events tooa certain partition.</span></span> <span data-ttu-id="d8aa8-128">Tím zajistíte, že když tyto události se načítají z oddílu hello, jejich přečtení v pořadí.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-128">Doing so ensures that when these events are read from hello partition, they are read in order.</span></span>

<span data-ttu-id="d8aa8-129">Pomocí této konfigurace Uvědomte si, že toowhich hello konkrétní oddíl, odesílání není k dispozici, obdržíte chybnou odpověď.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-129">With this configuration, keep in mind that if hello particular partition toowhich you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="d8aa8-130">Jako bod porovnání Pokud nemáte tooa jeden oddíl spřažení, odešle hello služby Event Hubs vaše události toohello další dostupný oddíl.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-130">As a point of comparison, if you do not have an affinity tooa single partition, hello Event Hubs service sends your event toohello next available partition.</span></span>

<span data-ttu-id="d8aa8-131">Jeden z možných řešení tooensure řazení při také maximalizace provoz, bude tooaggregate události v rámci událost zpracování aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-131">One possible solution tooensure ordering, while also maximizing up time, would be tooaggregate events as part of your event processing application.</span></span> <span data-ttu-id="d8aa8-132">Nejjednodušší způsob, jak tooaccomplish Hello to je toostamp událost s vlastnost počtu vlastní pořadí.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-132">hello easiest way tooaccomplish this is toostamp your event with a custom sequence number property.</span></span> <span data-ttu-id="d8aa8-133">Hello následující kód ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="d8aa8-133">hello following code shows an example:</span></span>

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

<span data-ttu-id="d8aa8-134">Tento příklad odešle vaše události tooone hello dostupných oddílů v Centru událostí a nastaví hello odpovídající pořadové číslo z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-134">This example sends your event tooone of hello available partitions in your event hub, and sets hello corresponding sequence number from your application.</span></span> <span data-ttu-id="d8aa8-135">Toto řešení vyžaduje toobe stavu uchovává aplikace zpracování, ale dává vaší odesílatelé koncový bod, který je pravděpodobnější toobe k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d8aa8-135">This solution requires state toobe kept by your processing application, but gives your senders an endpoint that is more likely toobe available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8aa8-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8aa8-136">Next steps</span></span>
<span data-ttu-id="d8aa8-137">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="d8aa8-137">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="d8aa8-138">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d8aa8-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="d8aa8-139">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="d8aa8-139">Create an event hub</span></span>](event-hubs-create.md)
