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
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="c974a-103">Automaticky škálovat jednotky propustnosti Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c974a-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="c974a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c974a-104">Overview</span></span>

<span data-ttu-id="c974a-105">Azure Event Hubs je vysoce škálovatelná data streamování platformy.</span><span class="sxs-lookup"><span data-stu-id="c974a-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="c974a-106">Zákazníci služby Event Hubs jako takový často zvýšit jejich využití po připojování ke službě.</span><span class="sxs-lookup"><span data-stu-id="c974a-106">As such, Event Hubs customers often increase their usage after onboarding to the service.</span></span> <span data-ttu-id="c974a-107">Toto zvýšení vyžadují zvýšení jednotky předem určený propustnosti (TUs) škálování služby Event Hubs a zpracování větší množství přenesených dat.</span><span class="sxs-lookup"><span data-stu-id="c974a-107">Such increases require increasing the predetermined throughput units (TUs) to scale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="c974a-108">*Zvýšilo automaticky* funkce služby Event Hubs automatické škálování počtu TUs podle potřeb využití.</span><span class="sxs-lookup"><span data-stu-id="c974a-108">The *Auto-inflate* feature of Event Hubs automatically scales up the number of TUs to meet usage needs.</span></span> <span data-ttu-id="c974a-109">Zvýšení TUs brání scénáře, ve kterém omezování:</span><span class="sxs-lookup"><span data-stu-id="c974a-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="c974a-110">Příchozí přenos dat, které překračují sazby nastavit TUs.</span><span class="sxs-lookup"><span data-stu-id="c974a-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="c974a-111">Žádosti o sazbách za odchozí data, kterou sazby překročit nastavit TUs.</span><span class="sxs-lookup"><span data-stu-id="c974a-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="c974a-112">Jak funguje automatické zvýšilo</span><span class="sxs-lookup"><span data-stu-id="c974a-112">How Auto-inflate works</span></span>

<span data-ttu-id="c974a-113">Je provoz Event Hubs řízená prostřednictvím jednotek propustnosti.</span><span class="sxs-lookup"><span data-stu-id="c974a-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="c974a-114">Jeden TU umožňuje 1 MB za sekundu příjem příchozích dat a dvakrát toto množství odchozí.</span><span class="sxs-lookup"><span data-stu-id="c974a-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="c974a-115">Standardní služby Event Hubs se dá nakonfigurovat s 1-20 jednotek propustnosti.</span><span class="sxs-lookup"><span data-stu-id="c974a-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="c974a-116">Automatické zvýšilo umožňuje začněte v malém rozsahu u jednotek minimální požadované propustnost.</span><span class="sxs-lookup"><span data-stu-id="c974a-116">Auto-inflate enables you to start small with the minimum required throughput units.</span></span> <span data-ttu-id="c974a-117">Funkci pak škáluje automaticky maximálnímu limitu jednotek propustnosti, které potřebujete, v závislosti na zvýšení provozu.</span><span class="sxs-lookup"><span data-stu-id="c974a-117">The feature then scales automatically to the maximum limit of throughput units you need, depending on the increase in your traffic.</span></span> <span data-ttu-id="c974a-118">Zvýšilo automaticky poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c974a-118">Auto-inflate provides the following benefits:</span></span>

- <span data-ttu-id="c974a-119">Efektivní škálování mechanismus začněte v malém rozsahu a škálovat postupně.</span><span class="sxs-lookup"><span data-stu-id="c974a-119">An efficient scaling mechanism to start small and scale up as you grow.</span></span>
- <span data-ttu-id="c974a-120">Automaticky škálovat zadanou horní limit bez omezení problémy.</span><span class="sxs-lookup"><span data-stu-id="c974a-120">Automatically scale to the specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="c974a-121">Větší kontrolu nad škálování, jak můžete řídit, kdy a jak mnohem škálování.</span><span class="sxs-lookup"><span data-stu-id="c974a-121">More control over scaling, as you control when and how much to scale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="c974a-122">Povolit automatické rozšířené na obor názvů</span><span class="sxs-lookup"><span data-stu-id="c974a-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="c974a-123">Můžete povolit nebo zakázat automatické zvýšilo v oboru názvů pomocí některé z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="c974a-123">You can enable or disable Auto-inflate on a namespace using either of the following methods:</span></span>

1. <span data-ttu-id="c974a-124">[Portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c974a-124">The [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c974a-125">Šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c974a-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-the-portal"></a><span data-ttu-id="c974a-126">Povolit automatické zvýšilo prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="c974a-126">Enable Auto-inflate through the portal</span></span>

<span data-ttu-id="c974a-127">Při vytváření na obor názvů služby Event Hubs, můžete povolit funkci Automatické rozšířené na obor názvů:</span><span class="sxs-lookup"><span data-stu-id="c974a-127">You can enable the Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="c974a-128">Když povolíte tuto možnost můžete začněte v malém rozsahu na vaší jednotky propustnosti a škálovat vaše využití stačit zvýšení.</span><span class="sxs-lookup"><span data-stu-id="c974a-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="c974a-129">Horní limit pro inflační nemá vliv na ceny, které závisí na počtu TUs použít za hodinu.</span><span class="sxs-lookup"><span data-stu-id="c974a-129">The upper limit for inflation does not affect pricing, which depends on the number of TUs used per hour.</span></span>

<span data-ttu-id="c974a-130">Můžete také povolit automatické zvýšilo pomocí **škálování** možnost v okně nastavení na portálu:</span><span class="sxs-lookup"><span data-stu-id="c974a-130">You can also enable Auto-inflate using the **Scale** option on the settings blade in the portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="c974a-131">Povolit automatické zvýšilo pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c974a-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="c974a-132">Můžete povolit zvýšilo automaticky během nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c974a-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="c974a-133">Například nastavit `isAutoInflateEnabled` vlastnost **true** a nastavte `maximumThroughputUnits` do 10.</span><span class="sxs-lookup"><span data-stu-id="c974a-133">For example, set the `isAutoInflateEnabled` property to **true** and set `maximumThroughputUnits` to 10.</span></span>

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

<span data-ttu-id="c974a-134">Úplnou šablonu, najdete v článku [centra událostí vytvořit obor názvů a povolit zvýšilo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) šablony na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c974a-134">For the complete template, see the [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c974a-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c974a-135">Next steps</span></span>

<span data-ttu-id="c974a-136">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="c974a-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="c974a-137">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c974a-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="c974a-138">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="c974a-138">Create an Event Hub</span></span>](event-hubs-create.md)
