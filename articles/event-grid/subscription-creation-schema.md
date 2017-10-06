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
# <a name="event-grid-subscription-schema"></a>Schématu předplatné mřížky události

toocreate předplatné událostí mřížky, odešlete toohello žádost o operaci vytvoření události odběru. Hello použijte následující formát:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Například toocreate odběr událostí pro účet úložiště s názvem `examplestorage` ve skupině prostředků s názvem `examplegroup`, použijte hello následující formát:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Hello článek popisuje vlastnosti hello a schéma pro hello textu hello požadavku.
 
## <a name="event-subscription-properties"></a>Vlastnosti odběru událostí

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Cílový | Objekt | Hello objekt, který definuje hello koncový bod. |
| Filtr | Objekt | Volitelné pole pro filtrování hello typy událostí. |

### <a name="destination-object"></a>cílový objekt

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| endpointType | Řetězec | Hello typ koncového bodu pro předplatné hello (webhooku nebo HTTP, centra událostí nebo fronty). | 
| Adresa URL koncového bodu | Řetězec |  | 

### <a name="filter-object"></a>objekt filtru

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| includedEventTypes | Pole | Shoda, pokud je typ události hello ve zprávě události hello přesnou shodu tooone tyto názvy typů událostí. Vyvolá chybu, pokud název události se neshoduje s názvy typů událostí hello zaregistrovaný pro zdroj události hello. Výchozí odpovídá všechny typy událostí. |
| subjectBeginsWith | Řetězec | Předpona match filtru toohello pole subject ve zprávě události hello. Výchozí Hello nebo prázdný řetězec odpovídá všechny. | 
| subjectEndsWith | Řetězec | Přípona match filtru toohello pole subject ve zprávě události hello. Výchozí Hello nebo prázdný řetězec odpovídá všechny. |
| subjectIsCaseSensitive | Řetězec | Ovládací prvky malá a velká písmena odpovídající pro filtry. |


## <a name="example-subscription-schema"></a>Příklad předplatné schématu

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

## <a name="next-steps"></a>Další kroky

* TooEvent Úvod mřížky, najdete v části [co je mřížky událostí?](overview.md)
