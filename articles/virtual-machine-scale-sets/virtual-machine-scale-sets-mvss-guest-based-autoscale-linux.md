---
title: "Použití Azure škálování s hosta metriky v šabloně sady škálování Linux | Microsoft Docs"
description: "Zjistěte, jak tooautoscale pomocí hosta metriky v šabloně sadu škálování virtuálního počítače systému Linux"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 7afbef943a5f15c7a72dcf7114f46d521c504424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Škálování pomocí hosta metriky v šabloně sady škálování Linux

Existují dva typy metriky v Azure, které jsou shromážděná z virtuálních počítačů a škálovat nastaví: některé přijde od hello hostitele virtuálního počítače a jiné pocházet z hello hosta virtuálního počítače. Metriky hostitele nevyžadují další nastavení vzhledem k tomu, že byly shromážděny sadou hello hostitele virtuálních počítačů, že nám hosta metriky vyžadují tooinstall hello [rozšíření Windows Azure Diagnostics](../virtual-machines/windows/extensions-diagnostics-template.md) nebo hello [Linux Azure Diagnostics rozšíření](../virtual-machines/linux/diagnostic-extension.md) v hello hosta virtuálního počítače. Jeden běžné důvod toouse hosta metriky místo metriky hostitele je, že hosta metriky poskytovat větší výběr metriky než metriky hostitele. Jedním z těchto příkladů je metriky využití paměti, které jsou k dispozici prostřednictvím metriky hosta. jsou uvedeny podporované Hello hostitele metriky [sem](../monitoring-and-diagnostics/monitoring-supported-metrics.md), a jsou uvedeny hosta běžně používané metriky [zde](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toouse škálování pravidla založená na hosta metriky pro Linux sady škálování.

## <a name="change-hello-template-definition"></a>Změna definice šablony hello

Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování škálování Linux hello nastavit ochranu hosta škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set existing-vnet`) část podle část:

Nejprve přidáme parametry pro `storageAccountName` a `storageAccountSasToken`. Diagnostika agenta Hello uloží data metriky v [tabulky](../cosmos-db/table-storage-how-to-use-dotnet.md) v rámci tohoto účtu úložiště. Od verze hello agenta diagnostiky Linux verze 3.0 pomocí přístupový klíč k úložišti není podporován. Musí používáme [tokenu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

V dalším kroku jsme upravit hello škálovací sadu `extensionProfile` rozšíření diagnostiky tooinclude hello. V této konfiguraci jsme zadejte hello prostředek nastaveno ID hello měřítka toocollect metriky z, a také hello účtu úložiště a SAS token toouse toostore hello metriky. Můžeme také určit, jak často jsou hello metriky agregovat (v tomto případě každou minutu) a které tootrack metriky (v této případu procento použité paměti). Podrobnější informace o této konfiguraci a metriky než procento využité paměti, najdete v části [této dokumentace](../virtual-machines/linux/diagnostic-extension.md).

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Nakonec přidáme `autoscaleSettings` prostředků tooconfigure škálování podle tyto metriky. Tento prostředek má `dependsOn` klauzuli, která odkazuje na škálování hello nastavit tooensure, který existuje sada škálování hello před pokusem o tooautoscale ho. Pokud jsme na jiné metriky tooautoscale, by používáme hello `counterSpecifier` z hello konfiguraci rozšíření diagnostiky jako hello `metricName` v konfiguraci automatického škálování hello. Další informace o konfiguraci automatického škálování, najdete v části hello [osvědčené postupy škálování](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) a hello [referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931928.aspx).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
