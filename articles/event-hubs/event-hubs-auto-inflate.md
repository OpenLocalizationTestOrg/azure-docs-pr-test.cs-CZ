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
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="b649a-103">Automaticky škálovat jednotky propustnosti Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b649a-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="b649a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b649a-104">Overview</span></span>

<span data-ttu-id="b649a-105">Azure Event Hubs je vysoce škálovatelná data streamování platformy.</span><span class="sxs-lookup"><span data-stu-id="b649a-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="b649a-106">Zákazníci služby Event Hubs jako takový často zvýšit jejich využití po registrace toohello služby.</span><span class="sxs-lookup"><span data-stu-id="b649a-106">As such, Event Hubs customers often increase their usage after onboarding toohello service.</span></span> <span data-ttu-id="b649a-107">Toto zvýšení vyžadují roste hello předdefinovány propustnost jednotky (TUs) tooscale Event Hubs a větší množství přenesených dat zpracovat.</span><span class="sxs-lookup"><span data-stu-id="b649a-107">Such increases require increasing hello predetermined throughput units (TUs) tooscale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="b649a-108">Hello *zvýšilo automaticky* funkce služby Event Hubs automaticky škálování hello počet TUs toomeet využití potřebám.</span><span class="sxs-lookup"><span data-stu-id="b649a-108">hello *Auto-inflate* feature of Event Hubs automatically scales up hello number of TUs toomeet usage needs.</span></span> <span data-ttu-id="b649a-109">Zvýšení TUs brání scénáře, ve kterém omezování:</span><span class="sxs-lookup"><span data-stu-id="b649a-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="b649a-110">Příchozí přenos dat, které překračují sazby nastavit TUs.</span><span class="sxs-lookup"><span data-stu-id="b649a-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="b649a-111">Žádosti o sazbách za odchozí data, kterou sazby překročit nastavit TUs.</span><span class="sxs-lookup"><span data-stu-id="b649a-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="b649a-112">Jak funguje automatické zvýšilo</span><span class="sxs-lookup"><span data-stu-id="b649a-112">How Auto-inflate works</span></span>

<span data-ttu-id="b649a-113">Je provoz Event Hubs řízená prostřednictvím jednotek propustnosti.</span><span class="sxs-lookup"><span data-stu-id="b649a-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="b649a-114">Jeden TU umožňuje 1 MB za sekundu příjem příchozích dat a dvakrát toto množství odchozí.</span><span class="sxs-lookup"><span data-stu-id="b649a-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="b649a-115">Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotek propustnosti.</span><span class="sxs-lookup"><span data-stu-id="b649a-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="b649a-116">Automatické zvýšilo umožňuje toostart malé s jednotky minimální požadované propustnosti hello.</span><span class="sxs-lookup"><span data-stu-id="b649a-116">Auto-inflate enables you toostart small with hello minimum required throughput units.</span></span> <span data-ttu-id="b649a-117">Funkce Hello automaticky pak škáluje toohello maximální limit jednotek propustnosti, které potřebujete, v závislosti na hello zvýšení provozu.</span><span class="sxs-lookup"><span data-stu-id="b649a-117">hello feature then scales automatically toohello maximum limit of throughput units you need, depending on hello increase in your traffic.</span></span> <span data-ttu-id="b649a-118">Automatické zvýšilo poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b649a-118">Auto-inflate provides hello following benefits:</span></span>

- <span data-ttu-id="b649a-119">Efektivní škálování mechanismus toostart malé a rozšiřování škálování využívajících podle růst.</span><span class="sxs-lookup"><span data-stu-id="b649a-119">An efficient scaling mechanism toostart small and scale up as you grow.</span></span>
- <span data-ttu-id="b649a-120">Automatické škálování zadanou horní limit toohello bez omezení problémů.</span><span class="sxs-lookup"><span data-stu-id="b649a-120">Automatically scale toohello specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="b649a-121">Větší kontrolu nad škálování, jak můžete řídit, kdy a kolik tooscale.</span><span class="sxs-lookup"><span data-stu-id="b649a-121">More control over scaling, as you control when and how much tooscale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="b649a-122">Povolit automatické rozšířené na obor názvů</span><span class="sxs-lookup"><span data-stu-id="b649a-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="b649a-123">Můžete povolit nebo zakázat automatické zvýšilo na některou z následujících metod hello obor názvů:</span><span class="sxs-lookup"><span data-stu-id="b649a-123">You can enable or disable Auto-inflate on a namespace using either of hello following methods:</span></span>

1. <span data-ttu-id="b649a-124">Hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b649a-124">hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b649a-125">Šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b649a-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-hello-portal"></a><span data-ttu-id="b649a-126">Povolit automatické zvýšilo prostřednictvím portálu hello</span><span class="sxs-lookup"><span data-stu-id="b649a-126">Enable Auto-inflate through hello portal</span></span>

<span data-ttu-id="b649a-127">Při vytváření na obor názvů služby Event Hubs můžete povolit funkci automatického zvýšilo hello u oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="b649a-127">You can enable hello Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="b649a-128">Když povolíte tuto možnost můžete začněte v malém rozsahu na vaší jednotky propustnosti a škálovat vaše využití stačit zvýšení.</span><span class="sxs-lookup"><span data-stu-id="b649a-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="b649a-129">Hello horní limit pro inflační nemá vliv na ceny, které závisí na počtu hello TUs použít za hodinu.</span><span class="sxs-lookup"><span data-stu-id="b649a-129">hello upper limit for inflation does not affect pricing, which depends on hello number of TUs used per hour.</span></span>

<span data-ttu-id="b649a-130">Můžete také povolit automatické zvýšilo pomocí hello **škálování** možnost v okně Nastavení hello hello portálu:</span><span class="sxs-lookup"><span data-stu-id="b649a-130">You can also enable Auto-inflate using hello **Scale** option on hello settings blade in hello portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="b649a-131">Povolit automatické zvýšilo pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b649a-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="b649a-132">Můžete povolit zvýšilo automaticky během nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b649a-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="b649a-133">Například sada hello `isAutoInflateEnabled` vlastnost příliš**true** a nastavte `maximumThroughputUnits` too10.</span><span class="sxs-lookup"><span data-stu-id="b649a-133">For example, set hello `isAutoInflateEnabled` property too**true** and set `maximumThroughputUnits` too10.</span></span>

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

<span data-ttu-id="b649a-134">Hello úplnou šablonu, najdete v části hello [centra událostí vytvořit obor názvů a povolit zvýšilo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) šablony na Githubu.</span><span class="sxs-lookup"><span data-stu-id="b649a-134">For hello complete template, see hello [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b649a-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b649a-135">Next steps</span></span>

<span data-ttu-id="b649a-136">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="b649a-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="b649a-137">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b649a-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="b649a-138">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="b649a-138">Create an Event Hub</span></span>](event-hubs-create.md)
