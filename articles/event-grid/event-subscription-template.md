---
title: "Mřížky předplatného Azure události pomocí šablony"
description: "Vytvořte odběr mřížky událostí pomocí šablony Resource Manageru."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Pomocí Správce prostředků šablony pro předplatné událostí mřížky

Tento článek ukazuje, jak vytvářet odběry mřížky událostí pomocí šablony Azure Resource Manager. Formát, který můžete použít se liší v závislosti na tom, jestli se odběru události skupinu prostředků nebo události pro konkrétní typ prostředku. V tomto článku jsou uvedeny oba formáty.

## <a name="subscribe-to-resource-group-events"></a>Přihlásit k odběru událostí skupiny prostředků

Když se přihlásíte k odběru události skupiny prostředků, použijte `Microsoft.EventGrid/eventSubscriptions` pro typ prostředku. Pro událost typu bodu, použijte buď `WebHook` nebo `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Při nasazení této šablony do skupiny prostředků, se přihlásíte k odběru události pro danou skupinu prostředků.

## <a name="subscribe-to-resource-events"></a>Přihlásit k odběru událostí prostředků

Když se přihlásíte k odběru události prostředků, přidružíte odběr správný zdroj zahrnutím typ prostředku a název v definici odběru. Pro typ prostředku použít `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Pro název, použijte `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Pro událost typu bodu, použijte buď `WebHook` nebo `EventHub`.

Následující příklad ukazuje, jak přihlásit k odběru událostí úložiště objektů Blob.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Další postup

* Úvod k mřížce událostí, naleznete v části [o mřížky událostí](overview.md).
* Úvod do Resource Manager, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
* Začínáme s událostí mřížky, najdete v tématu [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).