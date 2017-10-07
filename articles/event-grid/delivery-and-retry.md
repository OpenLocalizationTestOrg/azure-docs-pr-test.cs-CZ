---
title: "aaaAzure mřížky události doručení a zkuste to znovu"
description: "Popisuje, jak Azure událostí mřížky doručí události a jak zpracovává nedoručených zpráv."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a><span data-ttu-id="675a6-103">Doručení zpráv událostí mřížky a zkuste to znovu</span><span class="sxs-lookup"><span data-stu-id="675a6-103">Event Grid message delivery and retry</span></span> 

<span data-ttu-id="675a6-104">Tento článek popisuje, jak Azure událostí mřížky zpracovává události, když není potvrdí doručení.</span><span class="sxs-lookup"><span data-stu-id="675a6-104">This article describes how Azure Event Grid handles events when delivery is not acknowledged.</span></span>

<span data-ttu-id="675a6-105">Událost mřížky poskytuje trvanlivý doručení.</span><span class="sxs-lookup"><span data-stu-id="675a6-105">Event Grid provides durable delivery.</span></span> <span data-ttu-id="675a6-106">Zajišťuje každou zprávu alespoň jednou pro každé předplatné.</span><span class="sxs-lookup"><span data-stu-id="675a6-106">It delivers each message at least once for each subscription.</span></span> <span data-ttu-id="675a6-107">Události se posílají toohello zaregistrován webhooku každého předplatného okamžitě.</span><span class="sxs-lookup"><span data-stu-id="675a6-107">Events are sent toohello registered webhook of each subscription immediately.</span></span> <span data-ttu-id="675a6-108">Webhook, jehož není potvrdil přijetí události do 60 sekund od první doručení hello pokus, události mřížky opakování doručení hello události.</span><span class="sxs-lookup"><span data-stu-id="675a6-108">If a webhook does not acknowledge receipt of an event within 60 seconds of hello first delivery attempt, Event Grid retries delivery of hello event.</span></span>

## <a name="message-delivery-status"></a><span data-ttu-id="675a6-109">Stav doručení zpráv</span><span class="sxs-lookup"><span data-stu-id="675a6-109">Message delivery status</span></span>

<span data-ttu-id="675a6-110">Událost mřížky používá HTTP odpovědi kódy tooacknowledge příjmu událostí.</span><span class="sxs-lookup"><span data-stu-id="675a6-110">Event Grid uses HTTP response codes tooacknowledge receipt of events.</span></span> 

### <a name="success-codes"></a><span data-ttu-id="675a6-111">Kódy úspěch</span><span class="sxs-lookup"><span data-stu-id="675a6-111">Success codes</span></span>

<span data-ttu-id="675a6-112">Hello následující kódy odpovědi HTTP znamenat, že událost byla doručena úspěšně tooyour webhooku.</span><span class="sxs-lookup"><span data-stu-id="675a6-112">hello following HTTP response codes indicate that an event has been delivered successfully tooyour webhook.</span></span> <span data-ttu-id="675a6-113">Událost mřížky zvažuje doručení dokončení.</span><span class="sxs-lookup"><span data-stu-id="675a6-113">Event Grid considers delivery complete.</span></span>

- <span data-ttu-id="675a6-114">200 OK</span><span class="sxs-lookup"><span data-stu-id="675a6-114">200 OK</span></span>
- <span data-ttu-id="675a6-115">202 platných</span><span class="sxs-lookup"><span data-stu-id="675a6-115">202 Accepted</span></span>

### <a name="failure-codes"></a><span data-ttu-id="675a6-116">Selhání kódy</span><span class="sxs-lookup"><span data-stu-id="675a6-116">Failure codes</span></span>

<span data-ttu-id="675a6-117">Hello následující kódy odpovědi HTTP znamenat, že události doručení pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="675a6-117">hello following HTTP response codes indicate that an event delivery attempt failed.</span></span> <span data-ttu-id="675a6-118">Mřížky událostí se pokusí znovu toosend hello událostí.</span><span class="sxs-lookup"><span data-stu-id="675a6-118">Event Grid tries again toosend hello event.</span></span> 

- <span data-ttu-id="675a6-119">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="675a6-119">400 Bad Request</span></span>
- <span data-ttu-id="675a6-120">401 unauthorized</span><span class="sxs-lookup"><span data-stu-id="675a6-120">401 Unauthorized</span></span>
- <span data-ttu-id="675a6-121">404 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="675a6-121">404 Not Found</span></span>
- <span data-ttu-id="675a6-122">408 časový limit požadavku.</span><span class="sxs-lookup"><span data-stu-id="675a6-122">408 Request timeout</span></span>
- <span data-ttu-id="675a6-123">414 URI příliš dlouhý</span><span class="sxs-lookup"><span data-stu-id="675a6-123">414 URI Too Long</span></span>
- <span data-ttu-id="675a6-124">500 vnitřní chybu serveru</span><span class="sxs-lookup"><span data-stu-id="675a6-124">500 Internal Server Error</span></span>
- <span data-ttu-id="675a6-125">503 Služba nedostupná</span><span class="sxs-lookup"><span data-stu-id="675a6-125">503 Service Unavailable</span></span>
- <span data-ttu-id="675a6-126">Vypršel časový limit 504 brány</span><span class="sxs-lookup"><span data-stu-id="675a6-126">504 Gateway Timeout</span></span>

<span data-ttu-id="675a6-127">Další kód odpovědi nebo nedostatek odpověď znamená chybu.</span><span class="sxs-lookup"><span data-stu-id="675a6-127">Any other response code or a lack of a response indicates a failure.</span></span> <span data-ttu-id="675a6-128">Událost mřížky opakuje doručení.</span><span class="sxs-lookup"><span data-stu-id="675a6-128">Event Grid retries delivery.</span></span> 

## <a name="retry-intervals"></a><span data-ttu-id="675a6-129">Opakujte intervaly</span><span class="sxs-lookup"><span data-stu-id="675a6-129">Retry intervals</span></span>

<span data-ttu-id="675a6-130">Událost mřížky používá zásady opakování exponenciálního omezení rychlosti pro odeslání události.</span><span class="sxs-lookup"><span data-stu-id="675a6-130">Event Grid uses an exponential backoff retry policy for event delivery.</span></span> <span data-ttu-id="675a6-131">Pokud vaše webhooku neodpovídá nebo vrací kód chyby, opakování mřížky události doručení na hello následující plánu:</span><span class="sxs-lookup"><span data-stu-id="675a6-131">If your webhook does not respond or returns a failure code, Event Grid retries delivery on hello following schedule:</span></span>

1. <span data-ttu-id="675a6-132">10 sekund.</span><span class="sxs-lookup"><span data-stu-id="675a6-132">10 seconds</span></span>
2. <span data-ttu-id="675a6-133">30 sekund</span><span class="sxs-lookup"><span data-stu-id="675a6-133">30 seconds</span></span>
3. <span data-ttu-id="675a6-134">1 minuta</span><span class="sxs-lookup"><span data-stu-id="675a6-134">1 minute</span></span>
4. <span data-ttu-id="675a6-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="675a6-135">5 minutes</span></span>
5. <span data-ttu-id="675a6-136">10 minut</span><span class="sxs-lookup"><span data-stu-id="675a6-136">10 minutes</span></span>
6. <span data-ttu-id="675a6-137">30 minut</span><span class="sxs-lookup"><span data-stu-id="675a6-137">30 minutes</span></span>
7. <span data-ttu-id="675a6-138">1 hodina</span><span class="sxs-lookup"><span data-stu-id="675a6-138">1 hour</span></span>

<span data-ttu-id="675a6-139">Událost mřížky přidá malé náhodné přeskupování tooall opakování intervalech.</span><span class="sxs-lookup"><span data-stu-id="675a6-139">Event Grid adds a small randomization tooall retry intervals.</span></span>

## <a name="retry-duration"></a><span data-ttu-id="675a6-140">Opakujte doba trvání</span><span class="sxs-lookup"><span data-stu-id="675a6-140">Retry duration</span></span>

<span data-ttu-id="675a6-141">Během hello preview vyprší mřížky událostí Azure všechny události, které nejsou doručeny během dvou hodin.</span><span class="sxs-lookup"><span data-stu-id="675a6-141">During hello preview, Azure Event Grid expires all events that are not delivered within two hours.</span></span> <span data-ttu-id="675a6-142">Před zveřejněním tentokrát bude vyšší too24 hodin.</span><span class="sxs-lookup"><span data-stu-id="675a6-142">Before General Availability, this time will be increased too24 hours.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="675a6-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="675a6-143">Next steps</span></span>

* <span data-ttu-id="675a6-144">TooEvent Úvod mřížky, najdete v části [o mřížky událostí](overview.md).</span><span class="sxs-lookup"><span data-stu-id="675a6-144">For an introduction tooEvent Grid, see [About Event Grid](overview.md).</span></span>
* <span data-ttu-id="675a6-145">tooquickly začít používat mřížky událostí najdete v tématu [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="675a6-145">tooquickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>
