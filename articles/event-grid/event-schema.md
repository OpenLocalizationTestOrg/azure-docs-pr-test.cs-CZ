---
title: "Azure schématu události událostí mřížky"
description: "Popisuje vlastnosti, které jsou k dispozici pro události se mřížky událostí Azure."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/15/2017
ms.author: babanisa
ms.openlocfilehash: 9e3c7b31ef23b29827d7184dc033227685ed92f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="event-grid-event-schema"></a><span data-ttu-id="f5842-103">Událost schématu události mřížky</span><span class="sxs-lookup"><span data-stu-id="f5842-103">Event Grid event schema</span></span>

<span data-ttu-id="f5842-104">Tento článek poskytuje vlastnosti a schématu pro události.</span><span class="sxs-lookup"><span data-stu-id="f5842-104">This article provides the properties and schema for events.</span></span> <span data-ttu-id="f5842-105">Události obsahují sadu pěti vlastností požadovaný řetězec a požadovanou **data** objektu.</span><span class="sxs-lookup"><span data-stu-id="f5842-105">Events consist of a set of five required string properties and a required **data** object.</span></span> <span data-ttu-id="f5842-106">Vlastnosti jsou společné pro všechny události z libovolného vydavatele.</span><span class="sxs-lookup"><span data-stu-id="f5842-106">The properties are common to all events from any publisher.</span></span> <span data-ttu-id="f5842-107">**Data** objekt obsahuje vlastnosti, které jsou specifické pro každý vydavatele.</span><span class="sxs-lookup"><span data-stu-id="f5842-107">The **data** object contains properties that are specific to each publisher.</span></span> <span data-ttu-id="f5842-108">Témata systému tyto vlastnosti jsou specifické pro poskytovatele prostředků, jako je například úložiště nebo Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="f5842-108">For system topics, these properties are specific to the resource provider, such as Storage or Event Hubs.</span></span>

<span data-ttu-id="f5842-109">Události se posílají do Azure událostí mřížky ve pole, která může obsahovat více objektů událostí.</span><span class="sxs-lookup"><span data-stu-id="f5842-109">Events are sent to Azure Event Grid in an array, which can contain multiple event objects.</span></span> <span data-ttu-id="f5842-110">Pokud existuje jenom jedna událost, má pole o délce 1.</span><span class="sxs-lookup"><span data-stu-id="f5842-110">If there is only a single event, the array has a length of 1.</span></span> 
 
## <a name="event-properties"></a><span data-ttu-id="f5842-111">Vlastnosti události</span><span class="sxs-lookup"><span data-stu-id="f5842-111">Event properties</span></span>

<span data-ttu-id="f5842-112">Všechny události bude obsahovat stejné dat následující nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="f5842-112">All events will contain the same following top level data.</span></span>

| <span data-ttu-id="f5842-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f5842-113">Property</span></span> | <span data-ttu-id="f5842-114">Typ</span><span class="sxs-lookup"><span data-stu-id="f5842-114">Type</span></span> | <span data-ttu-id="f5842-115">Popis</span><span class="sxs-lookup"><span data-stu-id="f5842-115">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="f5842-116">Téma</span><span class="sxs-lookup"><span data-stu-id="f5842-116">topic</span></span> | <span data-ttu-id="f5842-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f5842-117">string</span></span> | <span data-ttu-id="f5842-118">Úplné prostředků cesta ke zdroji událostí.</span><span class="sxs-lookup"><span data-stu-id="f5842-118">Full resource path to the event source.</span></span> <span data-ttu-id="f5842-119">Toto pole není možné zapisovat.</span><span class="sxs-lookup"><span data-stu-id="f5842-119">This field is not writeable.</span></span> |
| <span data-ttu-id="f5842-120">Předmět</span><span class="sxs-lookup"><span data-stu-id="f5842-120">subject</span></span> | <span data-ttu-id="f5842-121">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f5842-121">string</span></span> | <span data-ttu-id="f5842-122">Vydavatel definována cesta předmět události.</span><span class="sxs-lookup"><span data-stu-id="f5842-122">Publisher defined path to the event subject.</span></span> |
| <span data-ttu-id="f5842-123">Typ události</span><span class="sxs-lookup"><span data-stu-id="f5842-123">eventType</span></span> | <span data-ttu-id="f5842-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f5842-124">string</span></span> | <span data-ttu-id="f5842-125">Jeden z typů událostí registrovaných pro tento zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="f5842-125">One of the registered event types for this event source.</span></span> |
| <span data-ttu-id="f5842-126">eventTime</span><span class="sxs-lookup"><span data-stu-id="f5842-126">eventTime</span></span> | <span data-ttu-id="f5842-127">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f5842-127">string</span></span> | <span data-ttu-id="f5842-128">Čas, který se vygeneruje událost založené na čas UTC poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="f5842-128">The time the event is generated based on the provider's UTC time.</span></span> |
| <span data-ttu-id="f5842-129">id</span><span class="sxs-lookup"><span data-stu-id="f5842-129">id</span></span> | <span data-ttu-id="f5842-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f5842-130">string</span></span> | <span data-ttu-id="f5842-131">Jedinečný identifikátor pro událost.</span><span class="sxs-lookup"><span data-stu-id="f5842-131">Unique identifier for the event.</span></span> |
| <span data-ttu-id="f5842-132">data</span><span class="sxs-lookup"><span data-stu-id="f5842-132">data</span></span> | <span data-ttu-id="f5842-133">Objekt</span><span class="sxs-lookup"><span data-stu-id="f5842-133">object</span></span> | <span data-ttu-id="f5842-134">Data události specifické pro poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5842-134">Event data specific to the resource provider.</span></span> |

## <a name="available-event-sources"></a><span data-ttu-id="f5842-135">Zdroje událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="f5842-135">Available event sources</span></span>

<span data-ttu-id="f5842-136">Následující zdroje událostí publikování událostí pro používání prostřednictvím mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="f5842-136">The following event sources publish events for consumption via Event Grid:</span></span>

* <span data-ttu-id="f5842-137">Skupiny prostředků (operace správy)</span><span class="sxs-lookup"><span data-stu-id="f5842-137">Resource Groups (management operations)</span></span>
* <span data-ttu-id="f5842-138">Předplatná Azure (operace správy)</span><span class="sxs-lookup"><span data-stu-id="f5842-138">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="f5842-139">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f5842-139">Event Hubs</span></span>
* <span data-ttu-id="f5842-140">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="f5842-140">Custom Topics</span></span>

## <a name="azure-subscriptions"></a><span data-ttu-id="f5842-141">Předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="f5842-141">Azure Subscriptions</span></span>

<span data-ttu-id="f5842-142">Předplatná Azure můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5842-142">Azure subscriptions can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="f5842-143">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="f5842-143">Available event types</span></span>

- <span data-ttu-id="f5842-144">**Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="f5842-144">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="f5842-145">**Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f5842-145">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="f5842-146">**Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="f5842-146">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="f5842-147">**Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.</span><span class="sxs-lookup"><span data-stu-id="f5842-147">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="f5842-148">**Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5842-148">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="f5842-149">**Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5842-149">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="f5842-150">To se stane, když byla zrušena nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="f5842-150">This happens when template deployment is cancelled.</span></span>

### <a name="example-event-schema"></a><span data-ttu-id="f5842-151">Příklad událostí schématu</span><span class="sxs-lookup"><span data-stu-id="f5842-151">Example event schema</span></span>

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="resource-groups"></a><span data-ttu-id="f5842-152">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f5842-152">Resource Groups</span></span>

<span data-ttu-id="f5842-153">Skupiny prostředků můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5842-153">Resource Groups can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="f5842-154">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="f5842-154">Available event types</span></span>

- <span data-ttu-id="f5842-155">**Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="f5842-155">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="f5842-156">**Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f5842-156">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="f5842-157">**Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="f5842-157">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="f5842-158">**Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.</span><span class="sxs-lookup"><span data-stu-id="f5842-158">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="f5842-159">**Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5842-159">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="f5842-160">**Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5842-160">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="f5842-161">To se stane, když byla zrušena nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="f5842-161">This happens when template deployment is cancelled.</span></span>

### <a name="example-event"></a><span data-ttu-id="f5842-162">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="f5842-162">Example event</span></span>

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="event-hubs"></a><span data-ttu-id="f5842-163">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f5842-163">Event Hubs</span></span>

<span data-ttu-id="f5842-164">Události centra událostí jsou aktuálně pouze vygenerované při soubor je automaticky odeslán do úložiště používá funkci zachycení.</span><span class="sxs-lookup"><span data-stu-id="f5842-164">Event Hubs events are currently only emitted when a file is automatically sent to storage using the Capture feature.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="f5842-165">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="f5842-165">Available event types</span></span>

- <span data-ttu-id="f5842-166">**Microsoft.EventHub.CaptureFileCreated**: vyvolá, když se vytvoří soubor zachycení.</span><span class="sxs-lookup"><span data-stu-id="f5842-166">**Microsoft.EventHub.CaptureFileCreated**: Raised when a capture file is created.</span></span>

### <a name="example-event"></a><span data-ttu-id="f5842-167">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="f5842-167">Example event</span></span>

<span data-ttu-id="f5842-168">Tato ukázka událost znázorňuje schéma událost Event Hubs vyvolá, když zachycení ukládá do souboru.</span><span class="sxs-lookup"><span data-stu-id="f5842-168">This sample event shows the schema of an Event Hubs event raised when Capture stores a file.</span></span> 

```json
[
    {
        "topic": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{event-hubs-ns}",
        "subject": "eventhubs/eh1",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-07-11T00:55:55.0120485Z",
        "id": "bd440490-a65e-4c97-8298-ef1eb325673c",
        "data": {
            "fileUrl": "https://gridtest1.blob.core.windows.net/acontainer/eventgridtest1/eh1/1/2017/07/11/00/54/54.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 0,
            "eventCount": 0,
            "firstSequenceNumber": -1,
            "lastSequenceNumber": -1,
            "firstEnqueueTime": "0001-01-01T00:00:00",
            "lastEnqueueTime": "0001-01-01T00:00:00"
        },
    }
]

```



## <a name="azure-blob-storage"></a><span data-ttu-id="f5842-169">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="f5842-169">Azure Blob Storage</span></span>

<span data-ttu-id="f5842-170">Azure Blob Storage v privátní Preview verzi s registrace pro integraci s událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="f5842-170">Azure Blob Storage in private preview with sign-up for integration with Event Grid.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="f5842-171">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="f5842-171">Available event types</span></span>

- <span data-ttu-id="f5842-172">**Microsoft.Storage.BlobCreated**: vyvolá, když je vytvořen objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f5842-172">**Microsoft.Storage.BlobCreated**: Raised when a blob is created.</span></span>
- <span data-ttu-id="f5842-173">**Microsoft.Storage.BlobDeleted**: vyvolá, když je odstraněn objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f5842-173">**Microsoft.Storage.BlobDeleted**: Raised when a blob is deleted.</span></span>

### <a name="example-event"></a><span data-ttu-id="f5842-174">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="f5842-174">Example event</span></span>

<span data-ttu-id="f5842-175">Tato ukázka událost znázorňuje schéma úložiště události vyvolá, když je vytvořen objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f5842-175">This sample event shows the schema of a storage event raised when a blob is created.</span></span> 

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```




## <a name="custom-topics"></a><span data-ttu-id="f5842-176">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="f5842-176">Custom Topics</span></span>

<span data-ttu-id="f5842-177">Datová vaše vlastní událostí, které je definované uživatelem a může být jakékoli dobře uvedena ve správném formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f5842-177">The data payload of your custom events is defined by you and can be any well formated JSON.</span></span> <span data-ttu-id="f5842-178">Data nejvyšší úrovně by měl obsahovat stejná pole jako standardní prostředek definice události.</span><span class="sxs-lookup"><span data-stu-id="f5842-178">The top level data should contain the same fields as standard resource defined events.</span></span> <span data-ttu-id="f5842-179">Při publikování událostí do vlastní témata byste měli zvážit, modelování předmět události pro usnadnění směrování a filtrování.</span><span class="sxs-lookup"><span data-stu-id="f5842-179">When publishing events to custom topics you should consider modeling the subject of your events to aid in routing and filtering.</span></span>

### <a name="example-event"></a><span data-ttu-id="f5842-180">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="f5842-180">Example event</span></span>

<span data-ttu-id="f5842-181">Následující příklad ukazuje událost pro vlastní tématu:</span><span class="sxs-lookup"><span data-stu-id="f5842-181">The following example shows an event for a custom topic:</span></span>
````json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.EventGrid/topics/myeventgridtopic",
    "subject": "/myapp/vehicles/motorcycles",    
    "id": "b68529f3-68cd-4744-baa4-3c0498ec19e2",
    "eventType": "recordInserted",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "data":{
      "make": "Ducati",
      "model": "Monster"
    }
  }
]

````

## <a name="next-steps"></a><span data-ttu-id="f5842-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5842-182">Next steps</span></span>

* <span data-ttu-id="f5842-183">Úvod k mřížce událostí, naleznete v části [co je mřížky událostí?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="f5842-183">For an introduction to Event Grid, see [What is Event Grid?](overview.md)</span></span>
* <span data-ttu-id="f5842-184">Další informace o vytváření předplatné mřížky událostí najdete v tématu [schématu odběru událostí mřížky](subscription-creation-schema.md).</span><span class="sxs-lookup"><span data-stu-id="f5842-184">To learn about creating an Event Grid subscription, see [Event Grid subscription schema](subscription-creation-schema.md).</span></span>
