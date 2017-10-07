---
title: "aaaAzure kanál Operational prostředků infrastruktury služby | Microsoft Docs"
description: "Úplný seznam protokolů generovaných v clusterech pro provozní kanál služby Azure Service Fabric hello."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: 358782420ed62b202d6a89fe0f200b5ef0384c9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operational-channel"></a>Provozní kanál 

provozní kanál Hello se skládá z protokolů z vysoké úrovně akci, kterou provádí na uzly a cluster Service Fabric. Pokud je povolená "Diagnostika" pro cluster, hello Azure Diagnostics agenta je nasazen v clusteru a je ve výchozím nastavení nakonfigurovaná tooread protokoly z hello provozní kanál. Další informace o konfiguraci hello [agenta Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) toomodify hello diagnostiky konfiguraci vašeho clusteru toopick další protokoly nebo čítače výkonu. 

## <a name="operational-channel-logs"></a>Protokoly pro provozní kanál 

Tady je kompletní seznam protokolů poskytované Service Fabric v hello provozní kanál. 

| ID události | Name (Název) | Zdroj (úloha) | Úroveň |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informační |
| 25621 | NodeOpenedSuccess | FabricNode | Informační |
| 25622 | NodeOpenedFailed | FabricNode | Informační |
| 25623 | NodeClosing | FabricNode | Informační |
| 25624 | NodeClosed | FabricNode | Informační |
| 25625 | NodeAborting | FabricNode | Informační |
| 25626 | NodeAborted | FabricNode | Informační |
| 29627 | ClusterUpgradeStart | CM | Informační |
| 29628 | ClusterUpgradeComplete | CM | Informační |
| 29629 | ClusterUpgradeRollback | CM | Informační |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informační |
| 29631 | ClusterUpgradeDomainComplete | CM | Informační |
| 23074 | ContainerActivated | Hostování | Informační |
| 23075 | ContainerDeactivated | Hostování | Informační |
| 29620 | ApplicationCreated | CM | Informační |
| 29621 | ApplicationUpgradeStart | CM | Informační |
| 29622 | ApplicationUpgradeComplete | CM | Informační |
| 29623 | ApplicationUpgradeRollback | CM | Informační |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informační |
| 29625 | ApplicationDeleted | CM | Informační |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informační |
| 18566 | ServiceCreated | FM | Informační |
| 18567 | ServiceDeleted | FM | Informační |

## <a name="next-steps"></a>Další kroky

* Další informace o celkové [generování událostí na úrovni platformy hello](service-fabric-diagnostics-event-generation-infra.md) v Service Fabric
* Úprava vaše [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) toocollect konfigurace více protokolů.
* [Nastavení služby Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) toosee provozní kanál protokolů
