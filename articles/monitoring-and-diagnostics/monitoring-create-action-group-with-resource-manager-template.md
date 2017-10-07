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
# <a name="create-an-action-group-with-a-resource-manager-template"></a><span data-ttu-id="c54f9-103">Vytvořit skupinu akce pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c54f9-103">Create an action group with a Resource Manager template</span></span>
<span data-ttu-id="c54f9-104">Tento článek ukazuje, jak toouse [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="c54f9-104">This article shows you how toouse an [Azure Resource Manager template](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure action groups.</span></span> <span data-ttu-id="c54f9-105">Pomocí šablony můžete automaticky nastavit skupiny akce, které lze znovu použít v určitých typů výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c54f9-105">By using templates, you can automatically set up action groups that can be reused in certain types of alerts.</span></span> <span data-ttu-id="c54f9-106">Tyto skupiny akce Ujistěte se, že všechny hello správné strany se upozorní, když se výstraha.</span><span class="sxs-lookup"><span data-stu-id="c54f9-106">These action groups ensure that all hello correct parties are notified when an alert is triggered.</span></span>

<span data-ttu-id="c54f9-107">Toto jsou základní kroky Hello:</span><span class="sxs-lookup"><span data-stu-id="c54f9-107">hello basic steps are:</span></span>

1. <span data-ttu-id="c54f9-108">Vytvořte šablonu jako soubor JSON, který popisuje, jak toocreate hello akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="c54f9-108">Create a template as a JSON file that describes how toocreate hello action group.</span></span>

2. <span data-ttu-id="c54f9-109">Nasazení šablony hello pomocí [libovolnou metodu nasazení](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="c54f9-109">Deploy hello template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

<span data-ttu-id="c54f9-110">Nejdřív jsme popisují, jak toocreate šablony Resource Manageru pro akce skupiny, kde definice hello akce jsou pevně zakódovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="c54f9-110">First, we describe how toocreate a Resource Manager template for an action group where hello action definitions are hard-coded in hello template.</span></span> <span data-ttu-id="c54f9-111">Druhý jsme popisují, jak toocreate šablonu, která přijímá informace o konfiguraci webhooku hello jako vstupní parametry při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="c54f9-111">Second, we describe how toocreate a template that takes hello webhook configuration information as input parameters when hello template is deployed.</span></span>

## <a name="resource-manager-templates-for-an-action-group"></a><span data-ttu-id="c54f9-112">Šablony Resource Manageru pro skupinu akce</span><span class="sxs-lookup"><span data-stu-id="c54f9-112">Resource Manager templates for an action group</span></span>

<span data-ttu-id="c54f9-113">toocreate skupinu akce pomocí šablony Resource Manageru, vytvořit prostředek typu hello `Microsoft.Insights/actionGroups`.</span><span class="sxs-lookup"><span data-stu-id="c54f9-113">toocreate an action group by using a Resource Manager template, you create a resource of hello type `Microsoft.Insights/actionGroups`.</span></span> <span data-ttu-id="c54f9-114">Potom můžete vyplnit všech souvisejících vlastností.</span><span class="sxs-lookup"><span data-stu-id="c54f9-114">Then you fill in all related properties.</span></span> <span data-ttu-id="c54f9-115">Tady jsou dvě ukázkové šablony, které vytvořit skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="c54f9-115">Here are two sample templates that create an action group.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="c54f9-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c54f9-116">Next steps</span></span>
* <span data-ttu-id="c54f9-117">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="c54f9-117">Learn more about [action groups](monitoring-action-groups.md).</span></span>
* <span data-ttu-id="c54f9-118">Další informace o [výstrahy](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="c54f9-118">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
* <span data-ttu-id="c54f9-119">Zjistěte, jak tooadd [výstrah pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="c54f9-119">Learn how tooadd [alerts by using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
