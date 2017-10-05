---
title: "Vytvoření skupin akce pomocí šablony Resource Manageru | Microsoft Docs"
description: "Naučte se vytvořit skupinu akce pomocí šablony Azure Resource Manager."
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
ms.openlocfilehash: 76bf353cac13f1c2169380f8dd3c1e163d4f3f41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a><span data-ttu-id="b4df6-103">Vytvořit skupinu akce pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b4df6-103">Create an action group with a Resource Manager template</span></span>
<span data-ttu-id="b4df6-104">V tomto článku se dozvíte, jak používat [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) ke konfiguraci skupin akce.</span><span class="sxs-lookup"><span data-stu-id="b4df6-104">This article shows you how to use an [Azure Resource Manager template](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) to configure action groups.</span></span> <span data-ttu-id="b4df6-105">Pomocí šablony můžete automaticky nastavit skupiny akce, které lze znovu použít v určitých typů výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b4df6-105">By using templates, you can automatically set up action groups that can be reused in certain types of alerts.</span></span> <span data-ttu-id="b4df6-106">Tyto skupiny akce Ujistěte se, že všechny správné strany se upozorní, když se výstraha.</span><span class="sxs-lookup"><span data-stu-id="b4df6-106">These action groups ensure that all the correct parties are notified when an alert is triggered.</span></span>

<span data-ttu-id="b4df6-107">Toto jsou základní kroky:</span><span class="sxs-lookup"><span data-stu-id="b4df6-107">The basic steps are:</span></span>

1. <span data-ttu-id="b4df6-108">Vytvořte šablonu jako soubor JSON, který popisuje, jak vytvořit skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="b4df6-108">Create a template as a JSON file that describes how to create the action group.</span></span>

2. <span data-ttu-id="b4df6-109">Nasazení šablony pomocí [libovolnou metodu nasazení](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="b4df6-109">Deploy the template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

<span data-ttu-id="b4df6-110">Nejdřív jsme popisují, jak vytvořit šablonu Resource Manageru pro skupinu akcí, kde definice akce jsou pevně zakódovaná v šabloně.</span><span class="sxs-lookup"><span data-stu-id="b4df6-110">First, we describe how to create a Resource Manager template for an action group where the action definitions are hard-coded in the template.</span></span> <span data-ttu-id="b4df6-111">Druhý jsme popisují, jak vytvořit šablonu, která přebírá informace o konfiguraci webhooku jako vstupní parametry při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="b4df6-111">Second, we describe how to create a template that takes the webhook configuration information as input parameters when the template is deployed.</span></span>

## <a name="resource-manager-templates-for-an-action-group"></a><span data-ttu-id="b4df6-112">Šablony Resource Manageru pro skupinu akce</span><span class="sxs-lookup"><span data-stu-id="b4df6-112">Resource Manager templates for an action group</span></span>

<span data-ttu-id="b4df6-113">Pokud chcete vytvořit skupinu akce pomocí šablony Resource Manageru, vytvořit prostředek typu `Microsoft.Insights/actionGroups`.</span><span class="sxs-lookup"><span data-stu-id="b4df6-113">To create an action group by using a Resource Manager template, you create a resource of the type `Microsoft.Insights/actionGroups`.</span></span> <span data-ttu-id="b4df6-114">Potom můžete vyplnit všech souvisejících vlastností.</span><span class="sxs-lookup"><span data-stu-id="b4df6-114">Then you fill in all related properties.</span></span> <span data-ttu-id="b4df6-115">Tady jsou dvě ukázkové šablony, které vytvořit skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="b4df6-115">Here are two sample templates that create an action group.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
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
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
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


## <a name="next-steps"></a><span data-ttu-id="b4df6-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4df6-116">Next steps</span></span>
* <span data-ttu-id="b4df6-117">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="b4df6-117">Learn more about [action groups](monitoring-action-groups.md).</span></span>
* <span data-ttu-id="b4df6-118">Další informace o [výstrahy](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b4df6-118">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
* <span data-ttu-id="b4df6-119">Informace o postupu přidání [výstrah pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="b4df6-119">Learn how to add [alerts by using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
