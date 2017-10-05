---
title: "Automaticky povolit nastavení pro diagnostiku pomocí šablony Resource Manageru | Microsoft Docs"
description: "Další informace o použití šablony Resource Manageru k vytvoření nastavení diagnostiky, které vám umožní datového proudu k diagnostickým protokolům do centra událostí nebo uchováváte v účtu úložiště."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/14/2017
ms.author: johnkem
ms.openlocfilehash: dde2435e976bbd14ca35cccc714ea21dcc5817b7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="e1172-103">Automaticky povolte nastavení pro diagnostiku při vytváření prostředků pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e1172-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="e1172-104">V tomto článku jsme ukazují, jak můžete použít [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) ke konfiguraci nastavení pro diagnostiku na prostředku při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="e1172-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) to configure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="e1172-105">To umožňuje automatické spuštění streamování diagnostické protokoly a metriky do centra událostí, archivaci je v účtu úložiště nebo jejich odeslání k analýze protokolů při vytváření prostředku.</span><span class="sxs-lookup"><span data-stu-id="e1172-105">This enables you to automatically start streaming your Diagnostic Logs and metrics to Event Hubs, archiving them in a Storage Account, or sending them to Log Analytics when a resource is created.</span></span>

<span data-ttu-id="e1172-106">Metoda pro povolení diagnostických protokolů pomocí šablony Resource Manageru závisí na typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="e1172-106">The method for enabling Diagnostic Logs using a Resource Manager template depends on the resource type.</span></span>

* <span data-ttu-id="e1172-107">**Bez výpočetní** prostředků (například skupiny zabezpečení sítě, Logic Apps automatizace), použijte [diagnostické nastavení popsané v tomto článku](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="e1172-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="e1172-108">**Výpočetní** prostředků (WAD/LAD na základě), použijte [WAD/LAD konfigurační soubor popsané v tomto článku](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="e1172-108">**Compute** (WAD/LAD-based) resources use the [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="e1172-109">V tomto článku jsme popisují, jak nakonfigurovat diagnostiku pomocí těchto metod.</span><span class="sxs-lookup"><span data-stu-id="e1172-109">In this article we describe how to configure diagnostics using either method.</span></span>

<span data-ttu-id="e1172-110">Základní kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="e1172-110">The basic steps are as follows:</span></span>

1. <span data-ttu-id="e1172-111">Vytvořte šablonu jako soubor JSON, který popisuje, jak vytvořit prostředek a zapněte diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="e1172-111">Create a template as a JSON file that describes how to create the resource and enable diagnostics.</span></span>
2. <span data-ttu-id="e1172-112">[Šablonu nasadit pomocí libovolné metody nasazení](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e1172-112">[Deploy the template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="e1172-113">Níže nás dostanete příklad soubor JSON šablony, které potřebujete k vygenerování pro jiný výpočetní prostředí a výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="e1172-113">Below we give an example of the template JSON file you need to generate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="e1172-114">Bez výpočetních prostředků šablony</span><span class="sxs-lookup"><span data-stu-id="e1172-114">Non-Compute resource template</span></span>
<span data-ttu-id="e1172-115">Pro jiné výpočetní prostředky budete muset udělat dvě věci:</span><span class="sxs-lookup"><span data-stu-id="e1172-115">For non-Compute resources, you will need to do two things:</span></span>

1. <span data-ttu-id="e1172-116">Přidání parametrů do objektu blob parametry pro název účtu úložiště, ID pravidla service bus nebo ID pracovního prostoru analýzy protokolů OMS (povolení archivace diagnostických protokolů v účtu úložiště, datové proudy protokoluje události do centra událostí a odesílání protokolů k analýze protokolů).</span><span class="sxs-lookup"><span data-stu-id="e1172-116">Add parameters to the parameters blob for the storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs to Event Hubs, and/or sending logs to Log Analytics).</span></span>
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. <span data-ttu-id="e1172-117">V poli prostředky prostředků, pro který chcete povolit diagnostických protokolů přidat prostředek typu `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="e1172-117">In the resources array of the resource for which you want to enable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "timeGrain": "PT1M",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

<span data-ttu-id="e1172-118">Odpovídá vlastnosti objektu blob pro nastavení diagnostiky [formát popsaný v tomto článku](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1172-118">The properties blob for the Diagnostic Setting follows [the format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="e1172-119">Přidávání `metrics` vlastnost vám umožní také odeslat metrika prostředků tyto stejné výstupů za předpokladu, že [prostředek podporuje Azure monitorování metriky](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e1172-119">Adding the `metrics` property will enable you to also send resource metrics to these same outputs, provided that [the resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="e1172-120">Zde je úplný příklad, který vytvoří aplikace logiky a zapne streamování centrům událostí a úložiště v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1172-120">Here is a full example that creates a Logic App and turns on streaming to Event Hubs and storage in a storage account.</span></span>

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a><span data-ttu-id="e1172-121">Výpočetní šablony prostředků</span><span class="sxs-lookup"><span data-stu-id="e1172-121">Compute resource template</span></span>
<span data-ttu-id="e1172-122">Povolí se Diagnostika na výpočetních prostředků, například cluster virtuálního počítače nebo Service Fabric, budete muset:</span><span class="sxs-lookup"><span data-stu-id="e1172-122">To enable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="e1172-123">Přidejte rozšíření Azure Diagnostics do definice prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e1172-123">Add the Azure Diagnostics extension to the VM resource definition.</span></span>
2. <span data-ttu-id="e1172-124">Zadejte úložiště účet nebo události rozbočovače jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e1172-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="e1172-125">Přidejte obsah souboru WADCfg XML do vlastnost XMLCfg správně uvozovací znaky všechny znaky XML.</span><span class="sxs-lookup"><span data-stu-id="e1172-125">Add the contents of your WADCfg XML file into the XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="e1172-126">Tento poslední krok může být složité získat správné.</span><span class="sxs-lookup"><span data-stu-id="e1172-126">This last step can be tricky to get right.</span></span> <span data-ttu-id="e1172-127">[Najdete v článku](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) příklad, která rozdělí schématu konfigurace diagnostiky do proměnné, které jsou uvozené a správně naformátovaná.</span><span class="sxs-lookup"><span data-stu-id="e1172-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits the Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="e1172-128">Celý proces, včetně ukázky, je popsaný [v tomto dokumentu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e1172-128">The entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1172-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1172-129">Next Steps</span></span>
* [<span data-ttu-id="e1172-130">Další informace o diagnostických protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="e1172-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="e1172-131">Stream Azure diagnostických protokolů do centra událostí</span><span class="sxs-lookup"><span data-stu-id="e1172-131">Stream Azure Diagnostic Logs to Event Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

