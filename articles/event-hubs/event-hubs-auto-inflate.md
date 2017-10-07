---
title: "škálování aaaAutomatically jednotky propustnosti Azure Event Hubs | Microsoft Docs"
description: "Povolit automatické zvýšilo na škále tooautomatically oboru názvů jednotky propustnosti"
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
ms.openlocfilehash: 0f5216bcd619ccddc1fd4063a2f0131bfa36c7d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automaticky škálovat jednotky propustnosti Azure Event Hubs

## <a name="overview"></a>Přehled

Azure Event Hubs je vysoce škálovatelná data streamování platformy. Zákazníci služby Event Hubs jako takový často zvýšit jejich využití po registrace toohello služby. Toto zvýšení vyžadují roste hello předdefinovány propustnost jednotky (TUs) tooscale Event Hubs a větší množství přenesených dat zpracovat. Hello *zvýšilo automaticky* funkce služby Event Hubs automaticky škálování hello počet TUs toomeet využití potřebám. Zvýšení TUs brání scénáře, ve kterém omezování:

* Příchozí přenos dat, které překračují sazby nastavit TUs.
* Žádosti o sazbách za odchozí data, kterou sazby překročit nastavit TUs.

## <a name="how-auto-inflate-works"></a>Jak funguje automatické zvýšilo

Je provoz Event Hubs řízená prostřednictvím jednotek propustnosti. Jeden TU umožňuje 1 MB za sekundu příjem příchozích dat a dvakrát toto množství odchozí. Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotek propustnosti. Automatické zvýšilo umožňuje toostart malé s jednotky minimální požadované propustnosti hello. Funkce Hello automaticky pak škáluje toohello maximální limit jednotek propustnosti, které potřebujete, v závislosti na hello zvýšení provozu. Automatické zvýšilo poskytuje hello následující výhody:

- Efektivní škálování mechanismus toostart malé a rozšiřování škálování využívajících podle růst.
- Automatické škálování zadanou horní limit toohello bez omezení problémů.
- Větší kontrolu nad škálování, jak můžete řídit, kdy a kolik tooscale.

## <a name="enable-auto-inflate-on-a-namespace"></a>Povolit automatické rozšířené na obor názvů

Můžete povolit nebo zakázat automatické zvýšilo na některou z následujících metod hello obor názvů:

1. Hello [portál Azure](https://portal.azure.com).
2. Šablonu Azure Resource Manager.

### <a name="enable-auto-inflate-through-hello-portal"></a>Povolit automatické zvýšilo prostřednictvím portálu hello

Při vytváření na obor názvů služby Event Hubs můžete povolit funkci automatického zvýšilo hello u oboru názvů:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Když povolíte tuto možnost můžete začněte v malém rozsahu na vaší jednotky propustnosti a škálovat vaše využití stačit zvýšení. Hello horní limit pro inflační nemá vliv na ceny, které závisí na počtu hello TUs použít za hodinu.

Můžete také povolit automatické zvýšilo pomocí hello **škálování** možnost v okně Nastavení hello hello portálu:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Povolit automatické zvýšilo pomocí šablony Azure Resource Manager

Můžete povolit zvýšilo automaticky během nasazení šablony Azure Resource Manager. Například sada hello `isAutoInflateEnabled` vlastnost příliš**true** a nastavte `maximumThroughputUnits` too10.

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

Hello úplnou šablonu, najdete v části hello [centra událostí vytvořit obor názvů a povolit zvýšilo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) šablony na Githubu.

## <a name="next-steps"></a>Další kroky

Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
