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
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automaticky povolte nastavení pro diagnostiku při vytváření prostředků pomocí šablony Resource Manageru
V tomto článku jsme ukazují, jak můžete použít [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure nastavení diagnostiky pro prostředek při vytvoření. To vám umožní spustit tooautomatically streamování diagnostické protokoly a metriky tooEvent rozbočovače, je archivaci v účtu úložiště, nebo je při vytváření prostředku odeslání tooLog Analytics.

Metoda Hello k povolení diagnostických protokolů pomocí šablony Resource Manageru závisí na typu prostředku hello.

* **Bez výpočetní** prostředků (například skupiny zabezpečení sítě, Logic Apps automatizace), použijte [diagnostické nastavení popsané v tomto článku](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **Výpočetní** (WAD/LAD na základě) prostředky používají hello [WAD/LAD konfigurační soubor popsané v tomto článku](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

V tomto článku jsme popisují, jak Diagnostika tooconfigure pomocí obou těchto metod.

Hello základní kroky jsou následující:

1. Vytvořte šablonu jako soubor JSON, který popisuje, jak toocreate hello prostředků a zapněte diagnostiku.
2. [Nasazení šablony hello pomocí libovolné metody nasazení](../azure-resource-manager/resource-group-template-deploy.md).

Níže nás dostanete příklad šablony hello soubor JSON, je nutné toogenerate pro jiný výpočetní prostředí a výpočetní prostředky.

## <a name="non-compute-resource-template"></a>Bez výpočetních prostředků šablony
Pro jiné výpočetní prostředky budete potřebovat toodo dvě věci:

1. Přidáte parametry toohello parametry objektu blob pro název účtu úložiště hello, ID pravidla service bus nebo ID pracovního prostoru analýzy protokolů OMS (povolení archivace diagnostických protokolů v účtu úložiště, datové proudy protokoly tooEvent centra nebo odesílání protokolů tooLog Analytics).
   
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
2. V poli prostředky hello hello prostředku, pro které chcete tooenable diagnostické protokoly, přidejte prostředek typu `[resource namespace]/providers/diagnosticSettings`.
   
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

Následuje technologie Hello vlastnosti objektu blob pro hello nastavení diagnostiky [hello formát je popsaný v tomto článku](https://msdn.microsoft.com/library/azure/dn931931.aspx). Přidání hello `metrics` vlastnost vám umožní tooalso odesílání prostředku metriky toothese stejné výstupy, za předpokladu, že [hello prostředek podporuje Azure monitorování metriky](monitoring-supported-metrics.md).

Zde je úplný příklad, který vytvoří aplikace logiky a zapne streamování tooEvent rozbočovače a úložiště v účtu úložiště.

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

## <a name="compute-resource-template"></a>Výpočetní šablony prostředků
Diagnostika tooenable na výpočetních prostředků, například virtuální počítač nebo cluster Service Fabric, budete muset:

1. Přidejte definici prostředků hello Azure Diagnostics rozšíření toohello virtuálních počítačů.
2. Zadejte úložiště účet nebo události rozbočovače jako parametr.
3. Přidejte hello obsah souboru WADCfg XML do vlastnosti XMLCfg hello, správně uvozovací znaky všechny znaky XML.

> [!WARNING]
> Tento poslední krok může být složité tooget vpravo. [Najdete v článku](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) příklad, rozdělení hello schéma konfigurace diagnostiky do proměnné, které jsou uvozené a správně naformátovaná.
> 
> 

Hello celý proces, včetně ukázky, je popsaný [v tomto dokumentu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostických protokolů Azure.](monitoring-overview-of-diagnostic-logs.md)
* [Datový proud diagnostických protokolů Azure tooEvent rozbočovače](monitoring-stream-diagnostic-logs-to-event-hubs.md)

