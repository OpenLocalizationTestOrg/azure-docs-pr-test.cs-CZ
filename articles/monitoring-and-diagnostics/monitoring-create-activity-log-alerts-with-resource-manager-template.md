---
title: "aaaCreate výstrahu aktivity protokolu pomocí šablony Resource Manageru | Microsoft Docs"
description: "Upozorňování při vytváření prostředků Azure."
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
ms.date: 07/06/2017
ms.author: ancav
ms.openlocfilehash: 0fb8aa037b9dce54ce35498622770955f2341bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a><span data-ttu-id="b16c7-103">Vytvořit výstrahu protokolu aktivit pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b16c7-103">Create an activity log alert with a Resource Manager template</span></span>
<span data-ttu-id="b16c7-104">Tento článek ukazuje, jak toouse [šablony Azure Resource Manageru](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b16c7-104">This article shows you how toouse an [Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure activity log alerts.</span></span> <span data-ttu-id="b16c7-105">Pomocí šablon můžete snadno nastavit více výstrah, které aktivovat na základě konkrétní aktivitu protokolu události podmínek jako součást procesu automatického nasazení.</span><span class="sxs-lookup"><span data-stu-id="b16c7-105">By using templates, you can easily set up many alerts that activate based on specific activity log event conditions as part of your automated deployment process.</span></span>

<span data-ttu-id="b16c7-106">Toto jsou základní kroky Hello:</span><span class="sxs-lookup"><span data-stu-id="b16c7-106">hello basic steps are:</span></span>

1. <span data-ttu-id="b16c7-107">Vytvořte šablonu jako soubor JSON, který popisuje, jak toocreate hello aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b16c7-107">Create a template as a JSON file that describes how toocreate hello activity log alert.</span></span>

2. <span data-ttu-id="b16c7-108">Nasazení šablony hello pomocí [libovolnou metodu nasazení](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="b16c7-108">Deploy hello template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

## <a name="resource-manager-template-for-an-activity-log-alert"></a><span data-ttu-id="b16c7-109">Šablony Resource Manageru pro výstrahu protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="b16c7-109">Resource Manager template for an activity log alert</span></span>
<span data-ttu-id="b16c7-110">toocreate výstrahu aktivity protokolu pomocí šablony Resource Manageru, vytvořit prostředek typu hello `microsoft.insights/activityLogAlerts`.</span><span class="sxs-lookup"><span data-stu-id="b16c7-110">toocreate an activity log alert by using a Resource Manager template, you create a resource of hello type `microsoft.insights/activityLogAlerts`.</span></span> <span data-ttu-id="b16c7-111">Potom můžete vyplnit všech souvisejících vlastností.</span><span class="sxs-lookup"><span data-stu-id="b16c7-111">Then you fill in all related properties.</span></span> <span data-ttu-id="b16c7-112">Zde je šablonu, která vytvoří výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="b16c7-112">Here's a template that creates an activity log alert.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not hello alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for hello Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

<span data-ttu-id="b16c7-113">Navštivte naše [galerii pro rychlý start Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) pro některé příklady výstrahy šablony protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="b16c7-113">Visit our [Azure Quickstart gallery](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) for some examples of activity log alert templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b16c7-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b16c7-114">Next steps</span></span>
- <span data-ttu-id="b16c7-115">Další informace o [výstrahy](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b16c7-115">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="b16c7-116">Zjistěte, jak tooadd [skupiny akce pomocí šablony Resource Manageru](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="b16c7-116">Learn how tooadd [action groups by using a Resource Manager template](monitoring-create-action-group-with-resource-manager-template.md).</span></span>
- <span data-ttu-id="b16c7-117">Zjistěte, jak příliš[vytvoření výstrahy toomonitor aktivity protokolu všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="b16c7-117">Learn how too[create an activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="b16c7-118">Zjistěte, jak příliš[vytvoření výstrahy toomonitor aktivity protokolu všechny operace škálování nebo škálovatelnou neúspěšné automatické škálování na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="b16c7-118">Learn how too[create an activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
