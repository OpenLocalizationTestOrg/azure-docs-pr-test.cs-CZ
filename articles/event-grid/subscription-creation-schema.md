---
title: "schéma předplatné aaaAzure událostí mřížky"
description: "Popisuje vlastnosti odběru tooan událost s událostí mřížky Azure hello."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: 6a96d67975a5a733c5ea3c56ea54501f94ea4cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-subscription-schema"></a><span data-ttu-id="25106-103">Schématu předplatné mřížky události</span><span class="sxs-lookup"><span data-stu-id="25106-103">Event Grid subscription schema</span></span>

<span data-ttu-id="25106-104">toocreate předplatné událostí mřížky, odešlete toohello žádost o operaci vytvoření události odběru.</span><span class="sxs-lookup"><span data-stu-id="25106-104">toocreate an Event Grid subscription, you send a request toohello Create Event subscription operation.</span></span> <span data-ttu-id="25106-105">Hello použijte následující formát:</span><span class="sxs-lookup"><span data-stu-id="25106-105">Use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="25106-106">Například toocreate odběr událostí pro účet úložiště s názvem `examplestorage` ve skupině prostředků s názvem `examplegroup`, použijte hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="25106-106">For example, toocreate an event subscription for a storage account named `examplestorage` in a resource group named `examplegroup`, use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="25106-107">Hello článek popisuje vlastnosti hello a schéma pro hello textu hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="25106-107">hello article describes hello properties and schema for hello body of hello request.</span></span>
 
## <a name="event-subscription-properties"></a><span data-ttu-id="25106-108">Vlastnosti odběru událostí</span><span class="sxs-lookup"><span data-stu-id="25106-108">Event subscription properties</span></span>

| <span data-ttu-id="25106-109">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="25106-109">Property</span></span> | <span data-ttu-id="25106-110">Typ</span><span class="sxs-lookup"><span data-stu-id="25106-110">Type</span></span> | <span data-ttu-id="25106-111">Popis</span><span class="sxs-lookup"><span data-stu-id="25106-111">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="25106-112">Cílový</span><span class="sxs-lookup"><span data-stu-id="25106-112">destination</span></span> | <span data-ttu-id="25106-113">Objekt</span><span class="sxs-lookup"><span data-stu-id="25106-113">object</span></span> | <span data-ttu-id="25106-114">Hello objekt, který definuje hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="25106-114">hello object that defines hello endpoint.</span></span> |
| <span data-ttu-id="25106-115">Filtr</span><span class="sxs-lookup"><span data-stu-id="25106-115">filter</span></span> | <span data-ttu-id="25106-116">Objekt</span><span class="sxs-lookup"><span data-stu-id="25106-116">object</span></span> | <span data-ttu-id="25106-117">Volitelné pole pro filtrování hello typy událostí.</span><span class="sxs-lookup"><span data-stu-id="25106-117">An optional field for filtering hello types of events.</span></span> |

### <a name="destination-object"></a><span data-ttu-id="25106-118">cílový objekt</span><span class="sxs-lookup"><span data-stu-id="25106-118">destination object</span></span>

| <span data-ttu-id="25106-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="25106-119">Property</span></span> | <span data-ttu-id="25106-120">Typ</span><span class="sxs-lookup"><span data-stu-id="25106-120">Type</span></span> | <span data-ttu-id="25106-121">Popis</span><span class="sxs-lookup"><span data-stu-id="25106-121">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="25106-122">endpointType</span><span class="sxs-lookup"><span data-stu-id="25106-122">endpointType</span></span> | <span data-ttu-id="25106-123">Řetězec</span><span class="sxs-lookup"><span data-stu-id="25106-123">string</span></span> | <span data-ttu-id="25106-124">Hello typ koncového bodu pro předplatné hello (webhooku nebo HTTP, centra událostí nebo fronty).</span><span class="sxs-lookup"><span data-stu-id="25106-124">hello type of endpoint for hello subscription (webhook/HTTP, Event Hub, or queue).</span></span> | 
| <span data-ttu-id="25106-125">Adresa URL koncového bodu</span><span class="sxs-lookup"><span data-stu-id="25106-125">endpointUrl</span></span> | <span data-ttu-id="25106-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="25106-126">string</span></span> |  | 

### <a name="filter-object"></a><span data-ttu-id="25106-127">objekt filtru</span><span class="sxs-lookup"><span data-stu-id="25106-127">filter object</span></span>

| <span data-ttu-id="25106-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="25106-128">Property</span></span> | <span data-ttu-id="25106-129">Typ</span><span class="sxs-lookup"><span data-stu-id="25106-129">Type</span></span> | <span data-ttu-id="25106-130">Popis</span><span class="sxs-lookup"><span data-stu-id="25106-130">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="25106-131">includedEventTypes</span><span class="sxs-lookup"><span data-stu-id="25106-131">includedEventTypes</span></span> | <span data-ttu-id="25106-132">Pole</span><span class="sxs-lookup"><span data-stu-id="25106-132">array</span></span> | <span data-ttu-id="25106-133">Shoda, pokud je typ události hello ve zprávě události hello přesnou shodu tooone tyto názvy typů událostí.</span><span class="sxs-lookup"><span data-stu-id="25106-133">Match when hello event type in hello event message is an exact match tooone of these event type names.</span></span> <span data-ttu-id="25106-134">Vyvolá chybu, pokud název události se neshoduje s názvy typů událostí hello zaregistrovaný pro zdroj události hello.</span><span class="sxs-lookup"><span data-stu-id="25106-134">Raises an error when event name does not match hello registered event type names for hello event source.</span></span> <span data-ttu-id="25106-135">Výchozí odpovídá všechny typy událostí.</span><span class="sxs-lookup"><span data-stu-id="25106-135">Default matches all event types.</span></span> |
| <span data-ttu-id="25106-136">subjectBeginsWith</span><span class="sxs-lookup"><span data-stu-id="25106-136">subjectBeginsWith</span></span> | <span data-ttu-id="25106-137">Řetězec</span><span class="sxs-lookup"><span data-stu-id="25106-137">string</span></span> | <span data-ttu-id="25106-138">Předpona match filtru toohello pole subject ve zprávě události hello.</span><span class="sxs-lookup"><span data-stu-id="25106-138">A prefix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="25106-139">Výchozí Hello nebo prázdný řetězec odpovídá všechny.</span><span class="sxs-lookup"><span data-stu-id="25106-139">hello default or empty string matches all.</span></span> | 
| <span data-ttu-id="25106-140">subjectEndsWith</span><span class="sxs-lookup"><span data-stu-id="25106-140">subjectEndsWith</span></span> | <span data-ttu-id="25106-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="25106-141">string</span></span> | <span data-ttu-id="25106-142">Přípona match filtru toohello pole subject ve zprávě události hello.</span><span class="sxs-lookup"><span data-stu-id="25106-142">A suffix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="25106-143">Výchozí Hello nebo prázdný řetězec odpovídá všechny.</span><span class="sxs-lookup"><span data-stu-id="25106-143">hello default or empty string matches all.</span></span> |
| <span data-ttu-id="25106-144">subjectIsCaseSensitive</span><span class="sxs-lookup"><span data-stu-id="25106-144">subjectIsCaseSensitive</span></span> | <span data-ttu-id="25106-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="25106-145">string</span></span> | <span data-ttu-id="25106-146">Ovládací prvky malá a velká písmena odpovídající pro filtry.</span><span class="sxs-lookup"><span data-stu-id="25106-146">Controls case-sensitive matching for filters.</span></span> |


## <a name="example-subscription-schema"></a><span data-ttu-id="25106-147">Příklad předplatné schématu</span><span class="sxs-lookup"><span data-stu-id="25106-147">Example subscription schema</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="25106-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25106-148">Next steps</span></span>

* <span data-ttu-id="25106-149">TooEvent Úvod mřížky, najdete v části [co je mřížky událostí?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="25106-149">For an introduction tooEvent Grid, see [What is Event Grid?](overview.md)</span></span>
