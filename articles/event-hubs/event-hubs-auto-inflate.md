---
title: "Automaticky škálovat jednotky propustnosti Azure Event Hubs | Microsoft Docs"
description: "Povolit automatické zvýšilo u oboru názvů automaticky škálování jednotky propustnosti"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: shvija;sethm
ms.openlocfilehash: b085091ea7bfd601efb0eee84144ddd091422d6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automaticky škálovat jednotky propustnosti Azure Event Hubs

## <a name="overview"></a>Přehled

Azure Event Hubs je vysoce škálovatelná data streamování platformy. Zákazníci služby Event Hubs jako takový často zvýšit jejich využití po připojování ke službě. Toto zvýšení vyžadují zvýšení jednotky předem určený propustnosti (TUs) škálování služby Event Hubs a zpracování větší množství přenesených dat. *Zvýšilo automaticky* funkce služby Event Hubs automatické škálování počtu TUs podle potřeb využití. Zvýšení TUs brání scénáře, ve kterém omezování:

* Příchozí přenos dat, které překračují sazby nastavit TUs.
* Žádosti o sazbách za odchozí data, kterou sazby překročit nastavit TUs.

## <a name="how-auto-inflate-works"></a>Jak funguje automatické zvýšilo

Je provoz Event Hubs řízená prostřednictvím jednotek propustnosti. Jeden TU umožňuje 1 MB za sekundu příjem příchozích dat a dvakrát toto množství odchozí. Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotek propustnosti. Automatické zvýšilo umožňuje začněte v malém rozsahu u jednotek minimální požadované propustnost. Funkci pak škáluje automaticky maximálnímu limitu jednotek propustnosti, které potřebujete, v závislosti na zvýšení provozu. Zvýšilo automaticky poskytuje následující výhody:

- Efektivní škálování mechanismus začněte v malém rozsahu a škálovat postupně.
- Automaticky škálovat zadanou horní limit bez omezení problémy.
- Větší kontrolu nad škálování, jak můžete řídit, kdy a jak mnohem škálování.

## <a name="enable-auto-inflate-on-a-namespace"></a>Povolit automatické rozšířené na obor názvů

Můžete povolit nebo zakázat automatické zvýšilo v oboru názvů pomocí některé z následujících metod:

1. [Portál Azure](https://portal.azure.com).
2. Šablonu Azure Resource Manager.

### <a name="enable-auto-inflate-through-the-portal"></a>Povolit automatické zvýšilo prostřednictvím portálu

Při vytváření na obor názvů služby Event Hubs, můžete povolit funkci Automatické rozšířené na obor názvů:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Když povolíte tuto možnost můžete začněte v malém rozsahu na vaší jednotky propustnosti a škálovat vaše využití stačit zvýšení. Horní limit pro inflační nemá vliv na ceny, které závisí na počtu TUs použít za hodinu.

Můžete také povolit automatické zvýšilo pomocí **škálování** možnost v okně nastavení na portálu:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Povolit automatické zvýšilo pomocí šablony Azure Resource Manager

Můžete povolit zvýšilo automaticky během nasazení šablony Azure Resource Manager. Například nastavit `isAutoInflateEnabled` vlastnost **true** a nastavte `maximumThroughputUnits` do 10.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Úplnou šablonu, najdete v článku [centra událostí vytvořit obor názvů a povolit zvýšilo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) šablony na Githubu.

## <a name="next-steps"></a>Další kroky

Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
