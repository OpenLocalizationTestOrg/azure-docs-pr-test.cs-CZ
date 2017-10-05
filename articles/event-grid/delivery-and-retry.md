---
title: "Azure mřížky události doručení a zkuste to znovu"
description: "Popisuje, jak Azure událostí mřížky doručí události a jak zpracovává nedoručených zpráv."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: e0f8afdfd84ea3c0c061459c27da285f6ae8957e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-grid-message-delivery-and-retry"></a><span data-ttu-id="18289-103">Doručení zpráv událostí mřížky a zkuste to znovu</span><span class="sxs-lookup"><span data-stu-id="18289-103">Event Grid message delivery and retry</span></span> 

<span data-ttu-id="18289-104">Tento článek popisuje, jak Azure událostí mřížky zpracovává události, když není potvrdí doručení.</span><span class="sxs-lookup"><span data-stu-id="18289-104">This article describes how Azure Event Grid handles events when delivery is not acknowledged.</span></span>

<span data-ttu-id="18289-105">Událost mřížky poskytuje trvanlivý doručení.</span><span class="sxs-lookup"><span data-stu-id="18289-105">Event Grid provides durable delivery.</span></span> <span data-ttu-id="18289-106">Zajišťuje každou zprávu alespoň jednou pro každé předplatné.</span><span class="sxs-lookup"><span data-stu-id="18289-106">It delivers each message at least once for each subscription.</span></span> <span data-ttu-id="18289-107">Události se posílají okamžitě registrované webhooku každého předplatného.</span><span class="sxs-lookup"><span data-stu-id="18289-107">Events are sent to the registered webhook of each subscription immediately.</span></span> <span data-ttu-id="18289-108">Webhook, jehož není potvrdil přijetí události do 60 sekund od první pokus o doručení, události mřížky opakuje doručení události.</span><span class="sxs-lookup"><span data-stu-id="18289-108">If a webhook does not acknowledge receipt of an event within 60 seconds of the first delivery attempt, Event Grid retries delivery of the event.</span></span>

## <a name="message-delivery-status"></a><span data-ttu-id="18289-109">Stav doručení zpráv</span><span class="sxs-lookup"><span data-stu-id="18289-109">Message delivery status</span></span>

<span data-ttu-id="18289-110">Událost mřížky používá kódy odpovědi HTTP potvrdit k přijetí události.</span><span class="sxs-lookup"><span data-stu-id="18289-110">Event Grid uses HTTP response codes to acknowledge receipt of events.</span></span> 

### <a name="success-codes"></a><span data-ttu-id="18289-111">Kódy úspěch</span><span class="sxs-lookup"><span data-stu-id="18289-111">Success codes</span></span>

<span data-ttu-id="18289-112">Následující kódy odpovědi HTTP znamenat, že událost byla doručena úspěšně vaší webhooku.</span><span class="sxs-lookup"><span data-stu-id="18289-112">The following HTTP response codes indicate that an event has been delivered successfully to your webhook.</span></span> <span data-ttu-id="18289-113">Událost mřížky zvažuje doručení dokončení.</span><span class="sxs-lookup"><span data-stu-id="18289-113">Event Grid considers delivery complete.</span></span>

- <span data-ttu-id="18289-114">200 OK</span><span class="sxs-lookup"><span data-stu-id="18289-114">200 OK</span></span>
- <span data-ttu-id="18289-115">202 platných</span><span class="sxs-lookup"><span data-stu-id="18289-115">202 Accepted</span></span>

### <a name="failure-codes"></a><span data-ttu-id="18289-116">Selhání kódy</span><span class="sxs-lookup"><span data-stu-id="18289-116">Failure codes</span></span>

<span data-ttu-id="18289-117">Následující kódy odpovědi HTTP znamenat, že události doručení pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="18289-117">The following HTTP response codes indicate that an event delivery attempt failed.</span></span> <span data-ttu-id="18289-118">Mřížky událostí se pokusí znovu odeslat událost.</span><span class="sxs-lookup"><span data-stu-id="18289-118">Event Grid tries again to send the event.</span></span> 

- <span data-ttu-id="18289-119">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="18289-119">400 Bad Request</span></span>
- <span data-ttu-id="18289-120">401 unauthorized</span><span class="sxs-lookup"><span data-stu-id="18289-120">401 Unauthorized</span></span>
- <span data-ttu-id="18289-121">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="18289-121">404 Not Found</span></span>
- <span data-ttu-id="18289-122">408 časový limit požadavku.</span><span class="sxs-lookup"><span data-stu-id="18289-122">408 Request timeout</span></span>
- <span data-ttu-id="18289-123">414 URI příliš dlouhý</span><span class="sxs-lookup"><span data-stu-id="18289-123">414 URI Too Long</span></span>
- <span data-ttu-id="18289-124">500 vnitřní chybu serveru</span><span class="sxs-lookup"><span data-stu-id="18289-124">500 Internal Server Error</span></span>
- <span data-ttu-id="18289-125">503 Služba nedostupná</span><span class="sxs-lookup"><span data-stu-id="18289-125">503 Service Unavailable</span></span>
- <span data-ttu-id="18289-126">Vypršel časový limit 504 brány</span><span class="sxs-lookup"><span data-stu-id="18289-126">504 Gateway Timeout</span></span>

<span data-ttu-id="18289-127">Další kód odpovědi nebo nedostatek odpověď znamená chybu.</span><span class="sxs-lookup"><span data-stu-id="18289-127">Any other response code or a lack of a response indicates a failure.</span></span> <span data-ttu-id="18289-128">Událost mřížky opakuje doručení.</span><span class="sxs-lookup"><span data-stu-id="18289-128">Event Grid retries delivery.</span></span> 

## <a name="retry-intervals"></a><span data-ttu-id="18289-129">Opakujte intervaly</span><span class="sxs-lookup"><span data-stu-id="18289-129">Retry intervals</span></span>

<span data-ttu-id="18289-130">Událost mřížky používá zásady opakování exponenciálního omezení rychlosti pro odeslání události.</span><span class="sxs-lookup"><span data-stu-id="18289-130">Event Grid uses an exponential backoff retry policy for event delivery.</span></span> <span data-ttu-id="18289-131">Pokud vaše webhooku neodpovídá nebo vrací kód chyby, opakování mřížky události doručení podle plánu, následující:</span><span class="sxs-lookup"><span data-stu-id="18289-131">If your webhook does not respond or returns a failure code, Event Grid retries delivery on the following schedule:</span></span>

1. <span data-ttu-id="18289-132">10 sekund.</span><span class="sxs-lookup"><span data-stu-id="18289-132">10 seconds</span></span>
2. <span data-ttu-id="18289-133">30 sekund</span><span class="sxs-lookup"><span data-stu-id="18289-133">30 seconds</span></span>
3. <span data-ttu-id="18289-134">1 minuta</span><span class="sxs-lookup"><span data-stu-id="18289-134">1 minute</span></span>
4. <span data-ttu-id="18289-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="18289-135">5 minutes</span></span>
5. <span data-ttu-id="18289-136">10 minut</span><span class="sxs-lookup"><span data-stu-id="18289-136">10 minutes</span></span>
6. <span data-ttu-id="18289-137">30 minut</span><span class="sxs-lookup"><span data-stu-id="18289-137">30 minutes</span></span>
7. <span data-ttu-id="18289-138">1 hodina</span><span class="sxs-lookup"><span data-stu-id="18289-138">1 hour</span></span>

<span data-ttu-id="18289-139">Událost mřížky přidá malé náhodné přeskupování všechny intervalech zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="18289-139">Event Grid adds a small randomization to all retry intervals.</span></span>

## <a name="retry-duration"></a><span data-ttu-id="18289-140">Opakujte doba trvání</span><span class="sxs-lookup"><span data-stu-id="18289-140">Retry duration</span></span>

<span data-ttu-id="18289-141">Ve verzi Preview vyprší mřížky událostí Azure všechny události, které nejsou doručeny během dvou hodin.</span><span class="sxs-lookup"><span data-stu-id="18289-141">During the preview, Azure Event Grid expires all events that are not delivered within two hours.</span></span> <span data-ttu-id="18289-142">Před zveřejněním tentokrát se zvýší na 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="18289-142">Before General Availability, this time will be increased to 24 hours.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="18289-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18289-143">Next steps</span></span>

* <span data-ttu-id="18289-144">Úvod k mřížce událostí, naleznete v části [o mřížky událostí](overview.md).</span><span class="sxs-lookup"><span data-stu-id="18289-144">For an introduction to Event Grid, see [About Event Grid](overview.md).</span></span>
* <span data-ttu-id="18289-145">Chcete-li rychle začít používat událostí mřížky, přečtěte si téma [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="18289-145">To quickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>