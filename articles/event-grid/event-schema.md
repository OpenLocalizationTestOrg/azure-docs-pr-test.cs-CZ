---
title: "schématu události aaaAzure událostí mřížky"
description: "Popisuje hello vlastnosti, které jsou k dispozici pro události se mřížky událostí Azure."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/15/2017
ms.author: babanisa
ms.openlocfilehash: 37178a5650b93fd9072d9cff3333aae14b2a2ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-event-schema"></a><span data-ttu-id="70316-103">Událost schématu události mřížky</span><span class="sxs-lookup"><span data-stu-id="70316-103">Event Grid event schema</span></span>

<span data-ttu-id="70316-104">Tento článek obsahuje hello vlastnosti a schématu pro události.</span><span class="sxs-lookup"><span data-stu-id="70316-104">This article provides hello properties and schema for events.</span></span> <span data-ttu-id="70316-105">Události obsahují sadu pěti vlastností požadovaný řetězec a požadovanou **data** objektu.</span><span class="sxs-lookup"><span data-stu-id="70316-105">Events consist of a set of five required string properties and a required **data** object.</span></span> <span data-ttu-id="70316-106">Hello vlastnosti jsou společné tooall události z libovolného vydavatele.</span><span class="sxs-lookup"><span data-stu-id="70316-106">hello properties are common tooall events from any publisher.</span></span> <span data-ttu-id="70316-107">Hello **data** objekt obsahuje vlastnosti, které jsou specifické tooeach vydavatele.</span><span class="sxs-lookup"><span data-stu-id="70316-107">hello **data** object contains properties that are specific tooeach publisher.</span></span> <span data-ttu-id="70316-108">Témata systému tyto vlastnosti jsou konkrétní toohello poskytovatele prostředků, jako je například úložiště nebo Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="70316-108">For system topics, these properties are specific toohello resource provider, such as Storage or Event Hubs.</span></span>

<span data-ttu-id="70316-109">Události se posílají tooAzure událostí mřížky ve pole, která může obsahovat více objektů událostí.</span><span class="sxs-lookup"><span data-stu-id="70316-109">Events are sent tooAzure Event Grid in an array, which can contain multiple event objects.</span></span> <span data-ttu-id="70316-110">Pokud existuje jenom jedna událost, má pole hello o délce 1.</span><span class="sxs-lookup"><span data-stu-id="70316-110">If there is only a single event, hello array has a length of 1.</span></span> 
 
## <a name="event-properties"></a><span data-ttu-id="70316-111">Vlastnosti události</span><span class="sxs-lookup"><span data-stu-id="70316-111">Event properties</span></span>

<span data-ttu-id="70316-112">Všechny události bude obsahovat hello stejné následující dat nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="70316-112">All events will contain hello same following top level data.</span></span>

| <span data-ttu-id="70316-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="70316-113">Property</span></span> | <span data-ttu-id="70316-114">Typ</span><span class="sxs-lookup"><span data-stu-id="70316-114">Type</span></span> | <span data-ttu-id="70316-115">Popis</span><span class="sxs-lookup"><span data-stu-id="70316-115">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="70316-116">Téma</span><span class="sxs-lookup"><span data-stu-id="70316-116">topic</span></span> | <span data-ttu-id="70316-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="70316-117">string</span></span> | <span data-ttu-id="70316-118">Zdroj události toohello prostředků úplná cesta.</span><span class="sxs-lookup"><span data-stu-id="70316-118">Full resource path toohello event source.</span></span> <span data-ttu-id="70316-119">Toto pole není možné zapisovat.</span><span class="sxs-lookup"><span data-stu-id="70316-119">This field is not writeable.</span></span> |
| <span data-ttu-id="70316-120">Předmět</span><span class="sxs-lookup"><span data-stu-id="70316-120">subject</span></span> | <span data-ttu-id="70316-121">Řetězec</span><span class="sxs-lookup"><span data-stu-id="70316-121">string</span></span> | <span data-ttu-id="70316-122">Vydavatel definované cesta toohello událostí předmět.</span><span class="sxs-lookup"><span data-stu-id="70316-122">Publisher defined path toohello event subject.</span></span> |
| <span data-ttu-id="70316-123">Typ události</span><span class="sxs-lookup"><span data-stu-id="70316-123">eventType</span></span> | <span data-ttu-id="70316-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="70316-124">string</span></span> | <span data-ttu-id="70316-125">Jeden z hello registrován typů událostí pro tento zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="70316-125">One of hello registered event types for this event source.</span></span> |
| <span data-ttu-id="70316-126">eventTime</span><span class="sxs-lookup"><span data-stu-id="70316-126">eventTime</span></span> | <span data-ttu-id="70316-127">Řetězec</span><span class="sxs-lookup"><span data-stu-id="70316-127">string</span></span> | <span data-ttu-id="70316-128">vygenerování Hello čas hello události na základě času UTC hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="70316-128">hello time hello event is generated based on hello provider's UTC time.</span></span> |
| <span data-ttu-id="70316-129">id</span><span class="sxs-lookup"><span data-stu-id="70316-129">id</span></span> | <span data-ttu-id="70316-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="70316-130">string</span></span> | <span data-ttu-id="70316-131">Jedinečný identifikátor pro událost hello.</span><span class="sxs-lookup"><span data-stu-id="70316-131">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="70316-132">data</span><span class="sxs-lookup"><span data-stu-id="70316-132">data</span></span> | <span data-ttu-id="70316-133">Objekt</span><span class="sxs-lookup"><span data-stu-id="70316-133">object</span></span> | <span data-ttu-id="70316-134">Zprostředkovatel prostředků konkrétní toohello dat události.</span><span class="sxs-lookup"><span data-stu-id="70316-134">Event data specific toohello resource provider.</span></span> |

## <a name="available-event-sources"></a><span data-ttu-id="70316-135">Zdroje událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="70316-135">Available event sources</span></span>

<span data-ttu-id="70316-136">Následující zdroje událostí Hello publikování událostí pro používání prostřednictvím mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="70316-136">hello following event sources publish events for consumption via Event Grid:</span></span>

* <span data-ttu-id="70316-137">Skupiny prostředků (operace správy)</span><span class="sxs-lookup"><span data-stu-id="70316-137">Resource Groups (management operations)</span></span>
* <span data-ttu-id="70316-138">Předplatná Azure (operace správy)</span><span class="sxs-lookup"><span data-stu-id="70316-138">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="70316-139">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="70316-139">Event Hubs</span></span>
* <span data-ttu-id="70316-140">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="70316-140">Custom Topics</span></span>

## <a name="azure-subscriptions"></a><span data-ttu-id="70316-141">Předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="70316-141">Azure Subscriptions</span></span>

<span data-ttu-id="70316-142">Předplatná Azure můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="70316-142">Azure subscriptions can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="70316-143">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="70316-143">Available event types</span></span>

- <span data-ttu-id="70316-144">**Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="70316-144">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="70316-145">**Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="70316-145">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="70316-146">**Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="70316-146">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="70316-147">**Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.</span><span class="sxs-lookup"><span data-stu-id="70316-147">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="70316-148">**Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="70316-148">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="70316-149">**Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="70316-149">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="70316-150">To se stane, když byla zrušena nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="70316-150">This happens when template deployment is cancelled.</span></span>

### <a name="example-event-schema"></a><span data-ttu-id="70316-151">Příklad událostí schématu</span><span class="sxs-lookup"><span data-stu-id="70316-151">Example event schema</span></span>

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



## <a name="resource-groups"></a><span data-ttu-id="70316-152">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="70316-152">Resource Groups</span></span>

<span data-ttu-id="70316-153">Skupiny prostředků můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="70316-153">Resource Groups can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="70316-154">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="70316-154">Available event types</span></span>

- <span data-ttu-id="70316-155">**Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="70316-155">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="70316-156">**Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="70316-156">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="70316-157">**Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="70316-157">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="70316-158">**Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.</span><span class="sxs-lookup"><span data-stu-id="70316-158">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="70316-159">**Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="70316-159">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="70316-160">**Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="70316-160">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="70316-161">To se stane, když byla zrušena nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="70316-161">This happens when template deployment is cancelled.</span></span>

### <a name="example-event"></a><span data-ttu-id="70316-162">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="70316-162">Example event</span></span>

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



## <a name="event-hubs"></a><span data-ttu-id="70316-163">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="70316-163">Event Hubs</span></span>

<span data-ttu-id="70316-164">Události centra událostí jsou aktuálně pouze vygenerované při toostorage pomocí funkce zachycení hello je automaticky odeslán soubor.</span><span class="sxs-lookup"><span data-stu-id="70316-164">Event Hubs events are currently only emitted when a file is automatically sent toostorage using hello Capture feature.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="70316-165">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="70316-165">Available event types</span></span>

- <span data-ttu-id="70316-166">**Microsoft.EventHub.CaptureFileCreated**: vyvolá, když se vytvoří soubor zachycení.</span><span class="sxs-lookup"><span data-stu-id="70316-166">**Microsoft.EventHub.CaptureFileCreated**: Raised when a capture file is created.</span></span>

### <a name="example-event"></a><span data-ttu-id="70316-167">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="70316-167">Example event</span></span>

<span data-ttu-id="70316-168">Tato ukázka událost ukazuje hello schéma událost Event Hubs vyvolá, když zachycení ukládá do souboru.</span><span class="sxs-lookup"><span data-stu-id="70316-168">This sample event shows hello schema of an Event Hubs event raised when Capture stores a file.</span></span> 

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



## <a name="azure-blob-storage"></a><span data-ttu-id="70316-169">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="70316-169">Azure Blob Storage</span></span>

<span data-ttu-id="70316-170">Azure Blob Storage v privátní Preview verzi s registrace pro integraci s událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="70316-170">Azure Blob Storage in private preview with sign-up for integration with Event Grid.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="70316-171">Typy událostí k dispozici</span><span class="sxs-lookup"><span data-stu-id="70316-171">Available event types</span></span>

- <span data-ttu-id="70316-172">**Microsoft.Storage.BlobCreated**: vyvolá, když je vytvořen objekt blob.</span><span class="sxs-lookup"><span data-stu-id="70316-172">**Microsoft.Storage.BlobCreated**: Raised when a blob is created.</span></span>
- <span data-ttu-id="70316-173">**Microsoft.Storage.BlobDeleted**: vyvolá, když je odstraněn objekt blob.</span><span class="sxs-lookup"><span data-stu-id="70316-173">**Microsoft.Storage.BlobDeleted**: Raised when a blob is deleted.</span></span>

### <a name="example-event"></a><span data-ttu-id="70316-174">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="70316-174">Example event</span></span>

<span data-ttu-id="70316-175">Tato ukázka událost ukazuje hello schéma úložiště události vyvolá, když je vytvořen objekt blob.</span><span class="sxs-lookup"><span data-stu-id="70316-175">This sample event shows hello schema of a storage event raised when a blob is created.</span></span> 

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




## <a name="custom-topics"></a><span data-ttu-id="70316-176">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="70316-176">Custom Topics</span></span>

<span data-ttu-id="70316-177">Hello datovou vaše vlastní událostí, které je definované uživatelem a může být jakékoli dobře uvedena ve správném formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="70316-177">hello data payload of your custom events is defined by you and can be any well formated JSON.</span></span> <span data-ttu-id="70316-178">Hello dat nejvyšší úrovně by měl obsahovat hello stejné pole jako standardní prostředek definice události.</span><span class="sxs-lookup"><span data-stu-id="70316-178">hello top level data should contain hello same fields as standard resource defined events.</span></span> <span data-ttu-id="70316-179">Při publikování události toocustom témata byste měli zvážit, modelování hello předmět vaše události tooaid směrování a filtrování.</span><span class="sxs-lookup"><span data-stu-id="70316-179">When publishing events toocustom topics you should consider modeling hello subject of your events tooaid in routing and filtering.</span></span>

### <a name="example-event"></a><span data-ttu-id="70316-180">Příklad událostí</span><span class="sxs-lookup"><span data-stu-id="70316-180">Example event</span></span>

<span data-ttu-id="70316-181">Hello následující příklad zobrazuje událost pro vlastní tématu:</span><span class="sxs-lookup"><span data-stu-id="70316-181">hello following example shows an event for a custom topic:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="70316-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70316-182">Next steps</span></span>

* <span data-ttu-id="70316-183">TooEvent Úvod mřížky, najdete v části [co je mřížky událostí?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="70316-183">For an introduction tooEvent Grid, see [What is Event Grid?](overview.md)</span></span>
* <span data-ttu-id="70316-184">toolearn o vytváření předplatné mřížky událostí najdete v části [schématu odběru událostí mřížky](subscription-creation-schema.md).</span><span class="sxs-lookup"><span data-stu-id="70316-184">toolearn about creating an Event Grid subscription, see [Event Grid subscription schema](subscription-creation-schema.md).</span></span>
