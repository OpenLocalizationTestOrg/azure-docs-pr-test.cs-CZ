---
title: "aaaAutomatically povolit nastavení pro diagnostiku pomocí šablony Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak toouse Resource Manager toocreate šablony diagnostiky se nastavení, které vám umožní toostream vaše diagnostické protokoly tooEvent centra nebo uchováváte v účtu úložiště."
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
ms.openlocfilehash: 8f38731107029928029c6d940da7bd076fea5d49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="29cbf-103">Automaticky povolte nastavení pro diagnostiku při vytváření prostředků pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="29cbf-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="29cbf-104">V tomto článku jsme ukazují, jak můžete použít [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure nastavení diagnostiky pro prostředek při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="29cbf-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="29cbf-105">To vám umožní spustit tooautomatically streamování diagnostické protokoly a metriky tooEvent rozbočovače, je archivaci v účtu úložiště, nebo je při vytváření prostředku odeslání tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="29cbf-105">This enables you tooautomatically start streaming your Diagnostic Logs and metrics tooEvent Hubs, archiving them in a Storage Account, or sending them tooLog Analytics when a resource is created.</span></span>

<span data-ttu-id="29cbf-106">Metoda Hello k povolení diagnostických protokolů pomocí šablony Resource Manageru závisí na typu prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="29cbf-106">hello method for enabling Diagnostic Logs using a Resource Manager template depends on hello resource type.</span></span>

* <span data-ttu-id="29cbf-107">**Bez výpočetní** prostředků (například skupiny zabezpečení sítě, Logic Apps automatizace), použijte [diagnostické nastavení popsané v tomto článku](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="29cbf-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="29cbf-108">**Výpočetní** (WAD/LAD na základě) prostředky používají hello [WAD/LAD konfigurační soubor popsané v tomto článku](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="29cbf-108">**Compute** (WAD/LAD-based) resources use hello [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="29cbf-109">V tomto článku jsme popisují, jak Diagnostika tooconfigure pomocí obou těchto metod.</span><span class="sxs-lookup"><span data-stu-id="29cbf-109">In this article we describe how tooconfigure diagnostics using either method.</span></span>

<span data-ttu-id="29cbf-110">Hello základní kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="29cbf-110">hello basic steps are as follows:</span></span>

1. <span data-ttu-id="29cbf-111">Vytvořte šablonu jako soubor JSON, který popisuje, jak toocreate hello prostředků a zapněte diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="29cbf-111">Create a template as a JSON file that describes how toocreate hello resource and enable diagnostics.</span></span>
2. <span data-ttu-id="29cbf-112">[Nasazení šablony hello pomocí libovolné metody nasazení](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="29cbf-112">[Deploy hello template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="29cbf-113">Níže nás dostanete příklad šablony hello soubor JSON, je nutné toogenerate pro jiný výpočetní prostředí a výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="29cbf-113">Below we give an example of hello template JSON file you need toogenerate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="29cbf-114">Bez výpočetních prostředků šablony</span><span class="sxs-lookup"><span data-stu-id="29cbf-114">Non-Compute resource template</span></span>
<span data-ttu-id="29cbf-115">Pro jiné výpočetní prostředky budete potřebovat toodo dvě věci:</span><span class="sxs-lookup"><span data-stu-id="29cbf-115">For non-Compute resources, you will need toodo two things:</span></span>

1. <span data-ttu-id="29cbf-116">Přidáte parametry toohello parametry objektu blob pro název účtu úložiště hello, ID pravidla service bus nebo ID pracovního prostoru analýzy protokolů OMS (povolení archivace diagnostických protokolů v účtu úložiště, datové proudy protokoly tooEvent centra nebo odesílání protokolů tooLog Analytics).</span><span class="sxs-lookup"><span data-stu-id="29cbf-116">Add parameters toohello parameters blob for hello storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs tooEvent Hubs, and/or sending logs tooLog Analytics).</span></span>
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
      }
    }
    ```
2. <span data-ttu-id="29cbf-117">V poli prostředky hello hello prostředku, pro které chcete tooenable diagnostické protokoly, přidejte prostředek typu `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="29cbf-117">In hello resources array of hello resource for which you want tooenable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
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

<span data-ttu-id="29cbf-118">Následuje technologie Hello vlastnosti objektu blob pro hello nastavení diagnostiky [hello formát je popsaný v tomto článku](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="29cbf-118">hello properties blob for hello Diagnostic Setting follows [hello format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="29cbf-119">Přidání hello `metrics` vlastnost vám umožní tooalso odesílání prostředku metriky toothese stejné výstupy, za předpokladu, že [hello prostředek podporuje Azure monitorování metriky](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="29cbf-119">Adding hello `metrics` property will enable you tooalso send resource metrics toothese same outputs, provided that [hello resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="29cbf-120">Zde je úplný příklad, který vytvoří aplikace logiky a zapne streamování tooEvent rozbočovače a úložiště v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="29cbf-120">Here is a full example that creates a Logic App and turns on streaming tooEvent Hubs and storage in a storage account.</span></span>

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
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

## <a name="compute-resource-template"></a><span data-ttu-id="29cbf-121">Výpočetní šablony prostředků</span><span class="sxs-lookup"><span data-stu-id="29cbf-121">Compute resource template</span></span>
<span data-ttu-id="29cbf-122">Diagnostika tooenable na výpočetních prostředků, například virtuální počítač nebo cluster Service Fabric, budete muset:</span><span class="sxs-lookup"><span data-stu-id="29cbf-122">tooenable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="29cbf-123">Přidejte definici prostředků hello Azure Diagnostics rozšíření toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="29cbf-123">Add hello Azure Diagnostics extension toohello VM resource definition.</span></span>
2. <span data-ttu-id="29cbf-124">Zadejte úložiště účet nebo události rozbočovače jako parametr.</span><span class="sxs-lookup"><span data-stu-id="29cbf-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="29cbf-125">Přidejte hello obsah souboru WADCfg XML do vlastnosti XMLCfg hello, správně uvozovací znaky všechny znaky XML.</span><span class="sxs-lookup"><span data-stu-id="29cbf-125">Add hello contents of your WADCfg XML file into hello XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="29cbf-126">Tento poslední krok může být složité tooget vpravo.</span><span class="sxs-lookup"><span data-stu-id="29cbf-126">This last step can be tricky tooget right.</span></span> <span data-ttu-id="29cbf-127">[Najdete v článku](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) příklad, rozdělení hello schéma konfigurace diagnostiky do proměnné, které jsou uvozené a správně naformátovaná.</span><span class="sxs-lookup"><span data-stu-id="29cbf-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits hello Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="29cbf-128">Hello celý proces, včetně ukázky, je popsaný [v tomto dokumentu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29cbf-128">hello entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="29cbf-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29cbf-129">Next Steps</span></span>
* [<span data-ttu-id="29cbf-130">Další informace o diagnostických protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="29cbf-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="29cbf-131">Datový proud diagnostických protokolů Azure tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="29cbf-131">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

