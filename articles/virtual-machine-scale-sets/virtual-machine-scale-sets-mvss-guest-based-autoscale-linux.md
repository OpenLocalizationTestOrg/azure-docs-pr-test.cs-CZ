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
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a><span data-ttu-id="287a3-103">Škálování pomocí hosta metriky v šabloně sady škálování Linux</span><span class="sxs-lookup"><span data-stu-id="287a3-103">Autoscale using guest metrics in a Linux scale set template</span></span>

<span data-ttu-id="287a3-104">Existují dva typy metriky v Azure, které jsou shromážděná z virtuálních počítačů a škálovat nastaví: některé přijde od hello hostitele virtuálního počítače a jiné pocházet z hello hosta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="287a3-104">There are two types of metrics in Azure that are gathered from VMs and scale sets: some come from hello host VM and others come from hello guest VM.</span></span> <span data-ttu-id="287a3-105">Metriky hostitele nevyžadují další nastavení vzhledem k tomu, že byly shromážděny sadou hello hostitele virtuálních počítačů, že nám hosta metriky vyžadují tooinstall hello [rozšíření Windows Azure Diagnostics](../virtual-machines/windows/extensions-diagnostics-template.md) nebo hello [Linux Azure Diagnostics rozšíření](../virtual-machines/linux/diagnostic-extension.md) v hello hosta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="287a3-105">Host metrics do not require additional setup because they are collected by hello host VM, whereas guest metrics require us tooinstall hello [Windows Azure Diagnostics extension](../virtual-machines/windows/extensions-diagnostics-template.md) or hello [Linux Azure Diagnostics extension](../virtual-machines/linux/diagnostic-extension.md) in hello guest VM.</span></span> <span data-ttu-id="287a3-106">Jeden běžné důvod toouse hosta metriky místo metriky hostitele je, že hosta metriky poskytovat větší výběr metriky než metriky hostitele.</span><span class="sxs-lookup"><span data-stu-id="287a3-106">One common reason toouse guest metrics instead of host metrics is that guest metrics provide a larger selection of metrics than host metrics.</span></span> <span data-ttu-id="287a3-107">Jedním z těchto příkladů je metriky využití paměti, které jsou k dispozici prostřednictvím metriky hosta.</span><span class="sxs-lookup"><span data-stu-id="287a3-107">One such example is memory-consumption metrics, which are only available via guest metrics.</span></span> <span data-ttu-id="287a3-108">jsou uvedeny podporované Hello hostitele metriky [sem](../monitoring-and-diagnostics/monitoring-supported-metrics.md), a jsou uvedeny hosta běžně používané metriky [zde](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="287a3-108">hello supported host metrics are listed [here](../monitoring-and-diagnostics/monitoring-supported-metrics.md), and commonly used guest metrics are listed [here](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span> <span data-ttu-id="287a3-109">Tento článek ukazuje, jak toomodify hello [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) toouse škálování pravidla založená na hosta metriky pro Linux sady škálování.</span><span class="sxs-lookup"><span data-stu-id="287a3-109">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toouse autoscale rules based on guest metrics for Linux scale sets.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="287a3-110">Změna definice šablony hello</span><span class="sxs-lookup"><span data-stu-id="287a3-110">Change hello template definition</span></span>

<span data-ttu-id="287a3-111">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablona pro nasazování škálování Linux hello nastavit ochranu hosta škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="287a3-111">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello Linux scale set with guest-based autoscale can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span></span> <span data-ttu-id="287a3-112">Podívejme se na toocreate hello rozdílové použít tuto šablonu (`git diff minimum-viable-scale-set existing-vnet`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="287a3-112">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="287a3-113">Nejprve přidáme parametry pro `storageAccountName` a `storageAccountSasToken`.</span><span class="sxs-lookup"><span data-stu-id="287a3-113">First, we add parameters for `storageAccountName` and `storageAccountSasToken`.</span></span> <span data-ttu-id="287a3-114">Diagnostika agenta Hello uloží data metriky v [tabulky](../cosmos-db/table-storage-how-to-use-dotnet.md) v rámci tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="287a3-114">hello diagnostics agent will store metric data in a [table](../cosmos-db/table-storage-how-to-use-dotnet.md) in this storage account.</span></span> <span data-ttu-id="287a3-115">Od verze hello agenta diagnostiky Linux verze 3.0 pomocí přístupový klíč k úložišti není podporován.</span><span class="sxs-lookup"><span data-stu-id="287a3-115">As of hello Linux Diagnostics Agent version 3.0, using a storage access key is no longer supported.</span></span> <span data-ttu-id="287a3-116">Musí používáme [tokenu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="287a3-116">We must use a [SAS Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

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

<span data-ttu-id="287a3-117">V dalším kroku jsme upravit hello škálovací sadu `extensionProfile` rozšíření diagnostiky tooinclude hello.</span><span class="sxs-lookup"><span data-stu-id="287a3-117">Next, we modify hello scale set `extensionProfile` tooinclude hello diagnostics extension.</span></span> <span data-ttu-id="287a3-118">V této konfiguraci jsme zadejte hello prostředek nastaveno ID hello měřítka toocollect metriky z, a také hello účtu úložiště a SAS token toouse toostore hello metriky.</span><span class="sxs-lookup"><span data-stu-id="287a3-118">In this configuration, we specify hello resource ID of hello scale set toocollect metrics from, as well as hello storage account and SAS token toouse toostore hello metrics.</span></span> <span data-ttu-id="287a3-119">Můžeme také určit, jak často jsou hello metriky agregovat (v tomto případě každou minutu) a které tootrack metriky (v této případu procento použité paměti).</span><span class="sxs-lookup"><span data-stu-id="287a3-119">We also specify how frequently hello metrics are aggregated (in this case every minute) and which metrics tootrack (in this case percent used memory).</span></span> <span data-ttu-id="287a3-120">Podrobnější informace o této konfiguraci a metriky než procento využité paměti, najdete v části [této dokumentace](../virtual-machines/linux/diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="287a3-120">For more detailed information on this configuration and metrics other than percent used memory, see [this documentation](../virtual-machines/linux/diagnostic-extension.md).</span></span>

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

<span data-ttu-id="287a3-121">Nakonec přidáme `autoscaleSettings` prostředků tooconfigure škálování podle tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="287a3-121">Finally, we add an `autoscaleSettings` resource tooconfigure autoscale based on these metrics.</span></span> <span data-ttu-id="287a3-122">Tento prostředek má `dependsOn` klauzuli, která odkazuje na škálování hello nastavit tooensure, který existuje sada škálování hello před pokusem o tooautoscale ho.</span><span class="sxs-lookup"><span data-stu-id="287a3-122">This resource has a `dependsOn` clause that references hello scale set tooensure that hello scale set exists before attempting tooautoscale it.</span></span> <span data-ttu-id="287a3-123">Pokud jsme na jiné metriky tooautoscale, by používáme hello `counterSpecifier` z hello konfiguraci rozšíření diagnostiky jako hello `metricName` v konfiguraci automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="287a3-123">If we choose a different metric tooautoscale on, we would use hello `counterSpecifier` from hello diagnostics extension configuration as hello `metricName` in hello autoscale configuration.</span></span> <span data-ttu-id="287a3-124">Další informace o konfiguraci automatického škálování, najdete v části hello [osvědčené postupy škálování](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) a hello [referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="287a3-124">For more information on autoscale configuration, see hello [autoscale best practices](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) and hello [Azure Monitor REST API reference documentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span></span>

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





## <a name="next-steps"></a><span data-ttu-id="287a3-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="287a3-125">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
