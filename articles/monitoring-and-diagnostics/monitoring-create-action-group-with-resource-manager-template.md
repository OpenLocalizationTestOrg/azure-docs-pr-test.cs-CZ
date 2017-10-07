---
title: "skupiny aaaCreate akce pomocí šablony Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak seskupit toocreate akce pomocí šablony Azure Resource Manager."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 9902b33cad99bd99b3deda0cf6f4ff12278c89c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Vytvořit skupinu akce pomocí šablony Resource Manageru
Tento článek ukazuje, jak toouse [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure akce skupiny. Pomocí šablony můžete automaticky nastavit skupiny akce, které lze znovu použít v určitých typů výstrahy. Tyto skupiny akce Ujistěte se, že všechny hello správné strany se upozorní, když se výstraha.

Toto jsou základní kroky Hello:

1. Vytvořte šablonu jako soubor JSON, který popisuje, jak toocreate hello akce skupiny.

2. Nasazení šablony hello pomocí [libovolnou metodu nasazení](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

Nejdřív jsme popisují, jak toocreate šablony Resource Manageru pro akce skupiny, kde definice hello akce jsou pevně zakódovaná v šabloně hello. Druhý jsme popisují, jak toocreate šablonu, která přijímá informace o konfiguraci webhooku hello jako vstupní parametry při nasazení šablony hello.

## <a name="resource-manager-templates-for-an-action-group"></a>Šablony Resource Manageru pro skupinu akce

toocreate skupinu akce pomocí šablony Resource Manageru, vytvořit prostředek typu hello `Microsoft.Insights/actionGroups`. Potom můžete vyplnit všech souvisejících vlastností. Tady jsou dvě ukázkové šablony, které vytvořit skupinu akce.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for hello Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for hello Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Další kroky
* Další informace o [skupiny akcí](monitoring-action-groups.md).
* Další informace o [výstrahy](monitoring-overview-alerts.md).
* Zjistěte, jak tooadd [výstrah pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
