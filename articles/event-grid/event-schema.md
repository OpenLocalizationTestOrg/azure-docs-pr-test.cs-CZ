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
# <a name="event-grid-event-schema"></a>Událost schématu události mřížky

Tento článek poskytuje vlastnosti a schématu pro události. Události obsahují sadu pěti vlastností požadovaný řetězec a požadovanou **data** objektu. Vlastnosti jsou společné pro všechny události z libovolného vydavatele. **Data** objekt obsahuje vlastnosti, které jsou specifické pro každý vydavatele. Témata systému tyto vlastnosti jsou specifické pro poskytovatele prostředků, jako je například úložiště nebo Event Hubs.

Události se posílají do Azure událostí mřížky ve pole, která může obsahovat více objektů událostí. Pokud existuje jenom jedna událost, má pole o délce 1. 
 
## <a name="event-properties"></a>Vlastnosti události

Všechny události bude obsahovat stejné dat následující nejvyšší úrovně.

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Téma | Řetězec | Úplné prostředků cesta ke zdroji událostí. Toto pole není možné zapisovat. |
| Předmět | Řetězec | Vydavatel definována cesta předmět události. |
| Typ události | Řetězec | Jeden z typů událostí registrovaných pro tento zdroj událostí. |
| eventTime | Řetězec | Čas, který se vygeneruje událost založené na čas UTC poskytovatele. |
| id | Řetězec | Jedinečný identifikátor pro událost. |
| data | Objekt | Data události specifické pro poskytovatele prostředků. |

## <a name="available-event-sources"></a>Zdroje událostí k dispozici

Následující zdroje událostí publikování událostí pro používání prostřednictvím mřížky událostí:

* Skupiny prostředků (operace správy)
* Předplatná Azure (operace správy)
* Event Hubs
* Vlastní témata

## <a name="azure-subscriptions"></a>Předplatná Azure

Předplatná Azure můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.

### <a name="available-event-types"></a>Typy událostí k dispozici

- **Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.  
- **Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.  
- **Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.  
- **Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.  
- **Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.  
- **Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků. To se stane, když byla zrušena nasazení šablony.

### <a name="example-event-schema"></a>Příklad událostí schématu

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



## <a name="resource-groups"></a>Skupiny prostředků

Skupiny prostředků můžete nyní emitování správu události ze Správce prostředků Azure třeba vytvoření virtuálního počítače nebo je odstraněný účet úložiště.

### <a name="available-event-types"></a>Typy událostí k dispozici

- **Microsoft.Resources.ResourceWriteSuccess**: vyvolá, když prostředek vytvořit nebo aktualizovat operace úspěšná.  
- **Microsoft.Resources.ResourceWriteFailure**: vyvolá, když vytvoření prostředku nebo operace aktualizace se nezdaří.  
- **Microsoft.Resources.ResourceWriteCancel**: vyvolá, když prostředek vytvořit nebo aktualizovat operace byla zrušena.  
- **Microsoft.Resources.ResourceDeleteSuccess**: vyvolá, když se podaří operace odstranění prostředku.  
- **Microsoft.Resources.ResourceDeleteFailure**: vyvolá, když se nezdaří operace odstranění prostředků.  
- **Microsoft.Resources.ResourceDeleteCancel**: "vyvolá, když byla zrušena odstranění prostředků. To se stane, když byla zrušena nasazení šablony.

### <a name="example-event"></a>Příklad událostí

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



## <a name="event-hubs"></a>Event Hubs

Události centra událostí jsou aktuálně pouze vygenerované při soubor je automaticky odeslán do úložiště používá funkci zachycení.

### <a name="available-event-types"></a>Typy událostí k dispozici

- **Microsoft.EventHub.CaptureFileCreated**: vyvolá, když se vytvoří soubor zachycení.

### <a name="example-event"></a>Příklad událostí

Tato ukázka událost znázorňuje schéma událost Event Hubs vyvolá, když zachycení ukládá do souboru. 

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



## <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Blob Storage v privátní Preview verzi s registrace pro integraci s událostí mřížky.

### <a name="available-event-types"></a>Typy událostí k dispozici

- **Microsoft.Storage.BlobCreated**: vyvolá, když je vytvořen objekt blob.
- **Microsoft.Storage.BlobDeleted**: vyvolá, když je odstraněn objekt blob.

### <a name="example-event"></a>Příklad událostí

Tato ukázka událost znázorňuje schéma úložiště události vyvolá, když je vytvořen objekt blob. 

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




## <a name="custom-topics"></a>Vlastní témata

Datová vaše vlastní událostí, které je definované uživatelem a může být jakékoli dobře uvedena ve správném formátu JSON. Data nejvyšší úrovně by měl obsahovat stejná pole jako standardní prostředek definice události. Při publikování událostí do vlastní témata byste měli zvážit, modelování předmět události pro usnadnění směrování a filtrování.

### <a name="example-event"></a>Příklad událostí

Následující příklad ukazuje událost pro vlastní tématu:
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

## <a name="next-steps"></a>Další kroky

* Úvod k mřížce událostí, naleznete v části [co je mřížky událostí?](overview.md)
* Další informace o vytváření předplatné mřížky událostí najdete v tématu [schématu odběru událostí mřížky](subscription-creation-schema.md).
