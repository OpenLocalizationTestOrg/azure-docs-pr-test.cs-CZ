---
title: "Schéma předplatné Azure událostí mřížky"
description: "Popisuje vlastnosti odběru pro událost s událostí mřížky Azure."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: eff2352066a76010d6d882a7b7e1961870cd2d46
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-grid-subscription-schema"></a><span data-ttu-id="133fb-103">Schématu předplatné mřížky události</span><span class="sxs-lookup"><span data-stu-id="133fb-103">Event Grid subscription schema</span></span>

<span data-ttu-id="133fb-104">Pokud chcete vytvořit předplatné událostí mřížky, odeslat požadavek na operaci vytvoření události odběru.</span><span class="sxs-lookup"><span data-stu-id="133fb-104">To create an Event Grid subscription, you send a request to the Create Event subscription operation.</span></span> <span data-ttu-id="133fb-105">Použijte následující formát:</span><span class="sxs-lookup"><span data-stu-id="133fb-105">Use the following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="133fb-106">Můžete například vytvořit odběr událostí pro účet úložiště s názvem `examplestorage` ve skupině prostředků s názvem `examplegroup`, použijte následující formát:</span><span class="sxs-lookup"><span data-stu-id="133fb-106">For example, to create an event subscription for a storage account named `examplestorage` in a resource group named `examplegroup`, use the following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="133fb-107">Článek popisuje vlastnosti a schéma pro text žádosti.</span><span class="sxs-lookup"><span data-stu-id="133fb-107">The article describes the properties and schema for the body of the request.</span></span>
 
## <a name="event-subscription-properties"></a><span data-ttu-id="133fb-108">Vlastnosti odběru událostí</span><span class="sxs-lookup"><span data-stu-id="133fb-108">Event subscription properties</span></span>

| <span data-ttu-id="133fb-109">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="133fb-109">Property</span></span> | <span data-ttu-id="133fb-110">Typ</span><span class="sxs-lookup"><span data-stu-id="133fb-110">Type</span></span> | <span data-ttu-id="133fb-111">Popis</span><span class="sxs-lookup"><span data-stu-id="133fb-111">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="133fb-112">Cílový</span><span class="sxs-lookup"><span data-stu-id="133fb-112">destination</span></span> | <span data-ttu-id="133fb-113">Objekt</span><span class="sxs-lookup"><span data-stu-id="133fb-113">object</span></span> | <span data-ttu-id="133fb-114">Objekt, který definuje koncový bod.</span><span class="sxs-lookup"><span data-stu-id="133fb-114">The object that defines the endpoint.</span></span> |
| <span data-ttu-id="133fb-115">Filtr</span><span class="sxs-lookup"><span data-stu-id="133fb-115">filter</span></span> | <span data-ttu-id="133fb-116">Objekt</span><span class="sxs-lookup"><span data-stu-id="133fb-116">object</span></span> | <span data-ttu-id="133fb-117">Volitelné pole pro filtrování typy událostí.</span><span class="sxs-lookup"><span data-stu-id="133fb-117">An optional field for filtering the types of events.</span></span> |

### <a name="destination-object"></a><span data-ttu-id="133fb-118">cílový objekt</span><span class="sxs-lookup"><span data-stu-id="133fb-118">destination object</span></span>

| <span data-ttu-id="133fb-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="133fb-119">Property</span></span> | <span data-ttu-id="133fb-120">Typ</span><span class="sxs-lookup"><span data-stu-id="133fb-120">Type</span></span> | <span data-ttu-id="133fb-121">Popis</span><span class="sxs-lookup"><span data-stu-id="133fb-121">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="133fb-122">endpointType</span><span class="sxs-lookup"><span data-stu-id="133fb-122">endpointType</span></span> | <span data-ttu-id="133fb-123">Řetězec</span><span class="sxs-lookup"><span data-stu-id="133fb-123">string</span></span> | <span data-ttu-id="133fb-124">Typ koncového bodu pro předplatné (webhooku nebo HTTP, centra událostí nebo fronty).</span><span class="sxs-lookup"><span data-stu-id="133fb-124">The type of endpoint for the subscription (webhook/HTTP, Event Hub, or queue).</span></span> | 
| <span data-ttu-id="133fb-125">Adresa URL koncového bodu</span><span class="sxs-lookup"><span data-stu-id="133fb-125">endpointUrl</span></span> | <span data-ttu-id="133fb-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="133fb-126">string</span></span> |  | 

### <a name="filter-object"></a><span data-ttu-id="133fb-127">objekt filtru</span><span class="sxs-lookup"><span data-stu-id="133fb-127">filter object</span></span>

| <span data-ttu-id="133fb-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="133fb-128">Property</span></span> | <span data-ttu-id="133fb-129">Typ</span><span class="sxs-lookup"><span data-stu-id="133fb-129">Type</span></span> | <span data-ttu-id="133fb-130">Popis</span><span class="sxs-lookup"><span data-stu-id="133fb-130">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="133fb-131">includedEventTypes</span><span class="sxs-lookup"><span data-stu-id="133fb-131">includedEventTypes</span></span> | <span data-ttu-id="133fb-132">Pole</span><span class="sxs-lookup"><span data-stu-id="133fb-132">array</span></span> | <span data-ttu-id="133fb-133">Shoda, když typ události ve zprávě události je přesnou shodou jeden z těchto názvů typu události.</span><span class="sxs-lookup"><span data-stu-id="133fb-133">Match when the event type in the event message is an exact match to one of these event type names.</span></span> <span data-ttu-id="133fb-134">Vyvolá chybu, pokud název události neodpovídá typu názvy registrovaných událostí pro zdroj události.</span><span class="sxs-lookup"><span data-stu-id="133fb-134">Raises an error when event name does not match the registered event type names for the event source.</span></span> <span data-ttu-id="133fb-135">Výchozí odpovídá všechny typy událostí.</span><span class="sxs-lookup"><span data-stu-id="133fb-135">Default matches all event types.</span></span> |
| <span data-ttu-id="133fb-136">subjectBeginsWith</span><span class="sxs-lookup"><span data-stu-id="133fb-136">subjectBeginsWith</span></span> | <span data-ttu-id="133fb-137">Řetězec</span><span class="sxs-lookup"><span data-stu-id="133fb-137">string</span></span> | <span data-ttu-id="133fb-138">Shoda předpony filtrovat pole předmět události zprávy.</span><span class="sxs-lookup"><span data-stu-id="133fb-138">A prefix-match filter to the subject field in the event message.</span></span> <span data-ttu-id="133fb-139">Výchozí nebo prázdný řetězec odpovídá všem.</span><span class="sxs-lookup"><span data-stu-id="133fb-139">The default or empty string matches all.</span></span> | 
| <span data-ttu-id="133fb-140">subjectEndsWith</span><span class="sxs-lookup"><span data-stu-id="133fb-140">subjectEndsWith</span></span> | <span data-ttu-id="133fb-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="133fb-141">string</span></span> | <span data-ttu-id="133fb-142">Přípona match filtrovat pole předmět události zprávy.</span><span class="sxs-lookup"><span data-stu-id="133fb-142">A suffix-match filter to the subject field in the event message.</span></span> <span data-ttu-id="133fb-143">Výchozí nebo prázdný řetězec odpovídá všem.</span><span class="sxs-lookup"><span data-stu-id="133fb-143">The default or empty string matches all.</span></span> |
| <span data-ttu-id="133fb-144">subjectIsCaseSensitive</span><span class="sxs-lookup"><span data-stu-id="133fb-144">subjectIsCaseSensitive</span></span> | <span data-ttu-id="133fb-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="133fb-145">string</span></span> | <span data-ttu-id="133fb-146">Ovládací prvky malá a velká písmena odpovídající pro filtry.</span><span class="sxs-lookup"><span data-stu-id="133fb-146">Controls case-sensitive matching for filters.</span></span> |


## <a name="example-subscription-schema"></a><span data-ttu-id="133fb-147">Příklad předplatné schématu</span><span class="sxs-lookup"><span data-stu-id="133fb-147">Example subscription schema</span></span>

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="133fb-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="133fb-148">Next steps</span></span>

* <span data-ttu-id="133fb-149">Úvod k mřížce událostí, naleznete v části [co je mřížky událostí?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="133fb-149">For an introduction to Event Grid, see [What is Event Grid?](overview.md)</span></span>