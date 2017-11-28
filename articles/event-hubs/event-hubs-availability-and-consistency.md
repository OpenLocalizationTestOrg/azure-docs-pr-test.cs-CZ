---
title: Dostupnost a konzistence v Azure Event Hubs | Microsoft Docs
description: "Jak poskytnout maximální množství dostupnosti a konzistence s Azure Event Hubs pomocí oddíly."
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
ms.openlocfilehash: 681a9d1636d547492f6f827461c6b2494b918778
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="a211d-103">Dostupnost a konzistence v Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a211d-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="a211d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a211d-104">Overview</span></span>
<span data-ttu-id="a211d-105">Používá Azure Event Hubs [dělení modelu](event-hubs-features.md#partitions) ke zlepšení dostupnosti a paralelního zpracování v rozbočovači jedna událost.</span><span class="sxs-lookup"><span data-stu-id="a211d-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) to improve availability and parallelization within a single event hub.</span></span> <span data-ttu-id="a211d-106">Například pokud centra událostí má čtyři oddíly, a jeden z těchto oddílů je přesouvat z jednoho serveru na jiný v operaci Vyrovnávání zatížení, můžete i nadále odesílat a přijímat z tři oddíly.</span><span class="sxs-lookup"><span data-stu-id="a211d-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server to another in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="a211d-107">Kromě toho s více oddíly umožňuje mít víc souběžných čtenářů zpracování dat, vylepšení agregovanou propustnost.</span><span class="sxs-lookup"><span data-stu-id="a211d-107">Additionally, having more partitions enables you to have more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="a211d-108">Vysvětlení důsledků vytváření oddílů a řazení v distribuované systému je zásadní aspekt návrhu řešení.</span><span class="sxs-lookup"><span data-stu-id="a211d-108">Understanding the implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="a211d-109">Vysvětlení kompromis mezi řazení a dostupnosti, najdete v článku [věta CAP](https://en.wikipedia.org/wiki/CAP_theorem), označované také jako věta Brewer společnosti.</span><span class="sxs-lookup"><span data-stu-id="a211d-109">To help explain the trade-off between ordering and availability, see the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="a211d-110">Tato věta popisuje volba mezi konzistence, dostupnost a odolnost proti oddílu.</span><span class="sxs-lookup"><span data-stu-id="a211d-110">This theorem discusses the choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="a211d-111">Na Brewer věta definuje konzistence a dostupnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a211d-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="a211d-112">Oddílu tolerance: možnost zpracování dat systému pokračovat zpracování dat i v případě, že dojde k selhání oddílu.</span><span class="sxs-lookup"><span data-stu-id="a211d-112">Partition tolerance: the ability of a data processing system to continue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="a211d-113">Dostupnost: bez selhání uzlu vrátí přiměřené odpověď během přiměřeně krátké doby (bez chyb nebo vypršení časových limitů).</span><span class="sxs-lookup"><span data-stu-id="a211d-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="a211d-114">Konzistence: čtení záruku, že vrátit poslední zápis pro daného klienta.</span><span class="sxs-lookup"><span data-stu-id="a211d-114">Consistency: a read is guaranteed to return the most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="a211d-115">Tolerance oddílu</span><span class="sxs-lookup"><span data-stu-id="a211d-115">Partition tolerance</span></span>
<span data-ttu-id="a211d-116">Služba Event Hubs je postavená na oddílů datový model.</span><span class="sxs-lookup"><span data-stu-id="a211d-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="a211d-117">Počet oddílů v Centru událostí můžete nakonfigurovat během instalace, ale tuto hodnotu nelze změnit později.</span><span class="sxs-lookup"><span data-stu-id="a211d-117">You can configure the number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="a211d-118">Vzhledem k tomu, že oddíly musí používat službou Event Hubs, máte k provedení rozhodnutí o dostupnosti a konzistence pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a211d-118">Since you must use partitions with Event Hubs, you have to make a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="a211d-119">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="a211d-119">Availability</span></span>
<span data-ttu-id="a211d-120">Nejjednodušší způsob, jak začít pracovat s Event Hubs je použije výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="a211d-120">The simplest way to get started with Event Hubs is to use the default behavior.</span></span> <span data-ttu-id="a211d-121">Pokud vytvoříte novou `EventHubClient` objektu a použít `Send` metody událostí je automaticky distribuovaná mezi oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="a211d-121">If you create a new `EventHubClient` object and use the `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="a211d-122">Toto chování umožňuje největší množství provoz.</span><span class="sxs-lookup"><span data-stu-id="a211d-122">This behavior allows for the greatest amount of up time.</span></span>

<span data-ttu-id="a211d-123">Tento model pro případy použití, které vyžadují maximální doba provozu, je upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="a211d-123">For use cases that require the maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="a211d-124">Konzistence</span><span class="sxs-lookup"><span data-stu-id="a211d-124">Consistency</span></span>
<span data-ttu-id="a211d-125">V některých případech může být řazení událostí důležité.</span><span class="sxs-lookup"><span data-stu-id="a211d-125">In some scenarios, the ordering of events can be important.</span></span> <span data-ttu-id="a211d-126">Například můžete systému back-end a zpracovat příkaz aktualizace před příkaz delete.</span><span class="sxs-lookup"><span data-stu-id="a211d-126">For example, you may want your back-end system to process an update command before a delete command.</span></span> <span data-ttu-id="a211d-127">V tomto případě můžete buď nastavit klíč oddílu na události, nebo použijte `PartitionSender` objekt jenom k odesílání událostí k určité oddílu.</span><span class="sxs-lookup"><span data-stu-id="a211d-127">In this instance, you can either set the partition key on an event, or use a `PartitionSender` object to only send events to a certain partition.</span></span> <span data-ttu-id="a211d-128">Tím zajistíte, že když tyto události se načítají z oddílu, jejich přečtení v pořadí.</span><span class="sxs-lookup"><span data-stu-id="a211d-128">Doing so ensures that when these events are read from the partition, they are read in order.</span></span>

<span data-ttu-id="a211d-129">Pomocí této konfigurace Uvědomte si, že konkrétní oddíl, do které odesíláte, není k dispozici, obdržíte chybnou odpověď.</span><span class="sxs-lookup"><span data-stu-id="a211d-129">With this configuration, keep in mind that if the particular partition to which you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="a211d-130">Jako bod porovnání Pokud nemáte spřažení do jediného oddílu služby Event Hubs odešle událost další dostupný oddíl.</span><span class="sxs-lookup"><span data-stu-id="a211d-130">As a point of comparison, if you do not have an affinity to a single partition, the Event Hubs service sends your event to the next available partition.</span></span>

<span data-ttu-id="a211d-131">Jedním z možných řešení zajistit řazení při také maximalizace provoz, bude pro agregaci událostí v rámci událost zpracování aplikace.</span><span class="sxs-lookup"><span data-stu-id="a211d-131">One possible solution to ensure ordering, while also maximizing up time, would be to aggregate events as part of your event processing application.</span></span> <span data-ttu-id="a211d-132">Nejjednodušší způsob, jak tomu je razítka událost s vlastnost počtu vlastní pořadí.</span><span class="sxs-lookup"><span data-stu-id="a211d-132">The easiest way to accomplish this is to stamp your event with a custom sequence number property.</span></span> <span data-ttu-id="a211d-133">Následující kód ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="a211d-133">The following code shows an example:</span></span>

```csharp
// Get the latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

<span data-ttu-id="a211d-134">Tento příklad odešle událost na jednu z dostupných oddílů v Centru událostí a nastaví odpovídající pořadové číslo z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a211d-134">This example sends your event to one of the available partitions in your event hub, and sets the corresponding sequence number from your application.</span></span> <span data-ttu-id="a211d-135">Toto řešení vyžaduje stavu budou muset zůstat aplikací zpracování, ale dává vaší odesílatelé koncový bod, který může být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a211d-135">This solution requires state to be kept by your processing application, but gives your senders an endpoint that is more likely to be available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a211d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a211d-136">Next steps</span></span>
<span data-ttu-id="a211d-137">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="a211d-137">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="a211d-138">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a211d-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="a211d-139">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="a211d-139">Create an event hub</span></span>](event-hubs-create.md)
