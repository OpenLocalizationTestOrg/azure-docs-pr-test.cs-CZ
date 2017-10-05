---
title: "Použití Azure škálování s hosta metriky v šabloně sady škálování Linux | Microsoft Docs"
description: "Zjistěte, jak chcete používat automatické škálování pomocí hosta metriky v šabloně sadu škálování virtuálního počítače systému Linux"
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
ms.openlocfilehash: ac0bbb4dbfccca3f3fc31526aeff11afe55d44be
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a><span data-ttu-id="61400-103">Škálování pomocí hosta metriky v šabloně sady škálování Linux</span><span class="sxs-lookup"><span data-stu-id="61400-103">Autoscale using guest metrics in a Linux scale set template</span></span>

<span data-ttu-id="61400-104">Existují dva typy metriky v Azure, které jsou shromážděná z virtuálních počítačů a škálovat nastaví: některé pocházet z hostitele virtuálního počítače a jiné pocházet z virtuálním počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="61400-104">There are two types of metrics in Azure that are gathered from VMs and scale sets: some come from the host VM and others come from the guest VM.</span></span> <span data-ttu-id="61400-105">Metriky hostitele nevyžadují další nastavení vzhledem k tomu, že se shromažďují pro hostitele virtuálního počítače, zatímco hosta metriky vyžadují nám k instalaci [rozšíření Windows Azure Diagnostics](../virtual-machines/windows/extensions-diagnostics-template.md) nebo [rozšíření diagnostiky Azure Linux ](../virtual-machines/linux/diagnostic-extension.md) ve virtuálním počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="61400-105">Host metrics do not require additional setup because they are collected by the host VM, whereas guest metrics require us to install the [Windows Azure Diagnostics extension](../virtual-machines/windows/extensions-diagnostics-template.md) or the [Linux Azure Diagnostics extension](../virtual-machines/linux/diagnostic-extension.md) in the guest VM.</span></span> <span data-ttu-id="61400-106">Jeden běžným důvodem k použití hosta metriky místo metriky hostitele je, že hosta metriky poskytovat větší výběr metriky než metriky hostitele.</span><span class="sxs-lookup"><span data-stu-id="61400-106">One common reason to use guest metrics instead of host metrics is that guest metrics provide a larger selection of metrics than host metrics.</span></span> <span data-ttu-id="61400-107">Jedním z těchto příkladů je metriky využití paměti, které jsou k dispozici prostřednictvím metriky hosta.</span><span class="sxs-lookup"><span data-stu-id="61400-107">One such example is memory-consumption metrics, which are only available via guest metrics.</span></span> <span data-ttu-id="61400-108">Jsou uvedeny podporované hostitele metriky [sem](../monitoring-and-diagnostics/monitoring-supported-metrics.md), a jsou uvedeny hosta běžně používané metriky [zde](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="61400-108">The supported host metrics are listed [here](../monitoring-and-diagnostics/monitoring-supported-metrics.md), and commonly used guest metrics are listed [here](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span> <span data-ttu-id="61400-109">Tento článek ukazuje, jak upravit [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) používat automatické škálování pravidla na základě hosta metriky pro Linux sady škálování.</span><span class="sxs-lookup"><span data-stu-id="61400-109">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to use autoscale rules based on guest metrics for Linux scale sets.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="61400-110">Změna definice šablony</span><span class="sxs-lookup"><span data-stu-id="61400-110">Change the template definition</span></span>

<span data-ttu-id="61400-111">Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablony pro nasazení měřítka Linux nastavit ochranu hosta škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="61400-111">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the Linux scale set with guest-based autoscale can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span></span> <span data-ttu-id="61400-112">Podívejme se na rozdílové použít k vytvoření této šablony (`git diff minimum-viable-scale-set existing-vnet`) část podle část:</span><span class="sxs-lookup"><span data-stu-id="61400-112">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="61400-113">Nejprve přidáme parametry pro `storageAccountName` a `storageAccountSasToken`.</span><span class="sxs-lookup"><span data-stu-id="61400-113">First, we add parameters for `storageAccountName` and `storageAccountSasToken`.</span></span> <span data-ttu-id="61400-114">Diagnostika agenta bude ukládat data metriky v [tabulky](../cosmos-db/table-storage-how-to-use-dotnet.md) v rámci tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61400-114">The diagnostics agent will store metric data in a [table](../cosmos-db/table-storage-how-to-use-dotnet.md) in this storage account.</span></span> <span data-ttu-id="61400-115">Od verze agenta diagnostiky Linux verze 3.0 pomocí přístupový klíč k úložišti není podporován.</span><span class="sxs-lookup"><span data-stu-id="61400-115">As of the Linux Diagnostics Agent version 3.0, using a storage access key is no longer supported.</span></span> <span data-ttu-id="61400-116">Musí používáme [tokenu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="61400-116">We must use a [SAS Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

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

<span data-ttu-id="61400-117">V dalším kroku jsme upravit sadu škálování `extensionProfile` zahrnout rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="61400-117">Next, we modify the scale set `extensionProfile` to include the diagnostics extension.</span></span> <span data-ttu-id="61400-118">V této konfiguraci určíme sad shromažďovat metriky, jakož i účet úložiště a tokenu SAS škálování ID prostředku používat k uložení metriky.</span><span class="sxs-lookup"><span data-stu-id="61400-118">In this configuration, we specify the resource ID of the scale set to collect metrics from, as well as the storage account and SAS token to use to store the metrics.</span></span> <span data-ttu-id="61400-119">Můžeme také určit, jak často jsou metriky agregovat (v tomto případě každou minutu) a které metriky pro sledování (v této případu procento použité paměti).</span><span class="sxs-lookup"><span data-stu-id="61400-119">We also specify how frequently the metrics are aggregated (in this case every minute) and which metrics to track (in this case percent used memory).</span></span> <span data-ttu-id="61400-120">Podrobnější informace o této konfiguraci a metriky než procento využité paměti, najdete v části [této dokumentace](../virtual-machines/linux/diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="61400-120">For more detailed information on this configuration and metrics other than percent used memory, see [this documentation](../virtual-machines/linux/diagnostic-extension.md).</span></span>

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

<span data-ttu-id="61400-121">Nakonec přidáme `autoscaleSettings` prostředku nakonfigurovat automatické škálování podle tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="61400-121">Finally, we add an `autoscaleSettings` resource to configure autoscale based on these metrics.</span></span> <span data-ttu-id="61400-122">Tento prostředek má `dependsOn` klauzuli, která odkazuje na měřítka nastavena na zkontrolujte, zda byly sadou škálování před pokusem o škálování ho.</span><span class="sxs-lookup"><span data-stu-id="61400-122">This resource has a `dependsOn` clause that references the scale set to ensure that the scale set exists before attempting to autoscale it.</span></span> <span data-ttu-id="61400-123">Pokud jsme na jiné metriky chcete používat automatické škálování, byste použili jsme `counterSpecifier` z konfiguraci rozšíření diagnostiky jako `metricName` v konfiguraci automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="61400-123">If we choose a different metric to autoscale on, we would use the `counterSpecifier` from the diagnostics extension configuration as the `metricName` in the autoscale configuration.</span></span> <span data-ttu-id="61400-124">Další informace o konfiguraci automatického škálování, najdete v článku [osvědčené postupy škálování](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) a [referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="61400-124">For more information on autoscale configuration, see the [autoscale best practices](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) and the [Azure Monitor REST API reference documentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span></span>

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





## <a name="next-steps"></a><span data-ttu-id="61400-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61400-125">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
