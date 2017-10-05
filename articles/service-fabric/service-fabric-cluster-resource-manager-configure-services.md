---
title: "Zadejte nastavení metriky a umístění v Azure mikroslužeb | Microsoft Docs"
description: "Zadáním metriky, omezení umístění a další zásady umístění, která popisují služby Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 807ae469e0555de542212532b829f464c9813598
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a><span data-ttu-id="9a5d5-103">Konfigurace nastavení správce prostředků clusteru služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="9a5d5-103">Configuring cluster resource manager settings for Service Fabric services</span></span>
<span data-ttu-id="9a5d5-104">Správce prostředků clusteru Service Fabric umožňuje jemně odstupňovanou kontrolu nad pravidla, která řídí každé jednotlivé s názvem služby.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-104">The Service Fabric Cluster Resource Manager allows fine-grained control over the rules that govern every individual named service.</span></span> <span data-ttu-id="9a5d5-105">Každé pojmenované služby lze určit pravidla, jak by měla být přidělená v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-105">Each named service can specify rules for how it should be allocated in the cluster.</span></span> <span data-ttu-id="9a5d5-106">Každé pojmenované služby můžete také definovat sadu metriky, které se chce k sestavě, včetně toho, jak důležité jsou k této službě.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-106">Each named service can also define the set of metrics that it wants to report, including how important they are to that service.</span></span> <span data-ttu-id="9a5d5-107">Konfigurace služeb, rozděleny do tří různých úloh:</span><span class="sxs-lookup"><span data-stu-id="9a5d5-107">Configuring services breaks down into three different tasks:</span></span>

1. <span data-ttu-id="9a5d5-108">Konfigurace omezení umístění</span><span class="sxs-lookup"><span data-stu-id="9a5d5-108">Configuring placement constraints</span></span>
2. <span data-ttu-id="9a5d5-109">Konfigurace metriky</span><span class="sxs-lookup"><span data-stu-id="9a5d5-109">Configuring metrics</span></span>
3. <span data-ttu-id="9a5d5-110">Konfigurace zásady rozšířené umístění a ostatní pravidla (méně běžné)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-110">Configuring advanced placement policies and other rules (less common)</span></span>

## <a name="placement-constraints"></a><span data-ttu-id="9a5d5-111">Omezení umístění</span><span class="sxs-lookup"><span data-stu-id="9a5d5-111">Placement constraints</span></span>
<span data-ttu-id="9a5d5-112">Omezení umístění slouží k řízení, které uzly v clusteru služby ve skutečnosti můžete spustit na.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-112">Placement constraints are used to control which nodes in the cluster a service can actually run on.</span></span> <span data-ttu-id="9a5d5-113">Obvykle konkrétní s názvem instance služby nebo všechny služby daného typu omezené ke spuštění na konkrétní typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-113">Typically a particular named service instance or all services of a given type constrained to run on a particular type of node.</span></span> <span data-ttu-id="9a5d5-114">Omezení umístění je rozšiřitelný.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-114">Placement constraints are extensible.</span></span> <span data-ttu-id="9a5d5-115">Můžete definovat libovolnou sadu vlastnosti na typ uzlu a potom vyberte pro ně s omezeními při vytváření služby.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-115">You can define any set of properties per  node type, and then select for them with constraints when creating services.</span></span> <span data-ttu-id="9a5d5-116">Můžete také změnit omezení umístění služby, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-116">You can also change a service's placement constraints while it is running.</span></span> <span data-ttu-id="9a5d5-117">To umožňuje reagovat na změny v clusteru nebo požadavky na služby.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-117">THis allows you to respond to changes in the cluster or the requirements of the service.</span></span> <span data-ttu-id="9a5d5-118">Vlastnosti daného uzlu můžete taky aktualizovat dynamicky v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-118">The properties of a given node can also be updated dynamically in the cluster.</span></span> <span data-ttu-id="9a5d5-119">Další informace o umístění omezení a způsob jejich konfigurace naleznete v [v tomto článku](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-119">More information on placement constraints and how to configure them can be found in [this article](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)</span></span>

## <a name="metrics"></a><span data-ttu-id="9a5d5-120">Metriky</span><span class="sxs-lookup"><span data-stu-id="9a5d5-120">Metrics</span></span>
<span data-ttu-id="9a5d5-121">Metriky jsou sadu prostředků, které potřebuje uvedená služba s názvem.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-121">Metrics are the set of resources that a given named service needs.</span></span> <span data-ttu-id="9a5d5-122">Konfigurace metriky služby zahrnuje kolik prostředku každé stavová repliky nebo bezstavové instance této služby spotřebuje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-122">A service's metric configuration includes how much of that resource each stateful replica or stateless instance of that service consumes by default.</span></span> <span data-ttu-id="9a5d5-123">Metriky taky zahrnovat váhy, která určuje, jak důležité vyrovnávání této metrika je do této služby, v případě kompromisy jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-123">Metrics also include a weight that indicates how important balancing that metric is to that service, in case tradeoffs are necessary.</span></span>

## <a name="advanced-placement-rules"></a><span data-ttu-id="9a5d5-124">Pravidla pro pokročilé umístění</span><span class="sxs-lookup"><span data-stu-id="9a5d5-124">Advanced placement rules</span></span>
<span data-ttu-id="9a5d5-125">Existují jiné typy pravidla pro umístění, které jsou užitečné v méně běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-125">There are other types of placement rules that are useful in less common scenarios.</span></span> <span data-ttu-id="9a5d5-126">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="9a5d5-126">Some examples are:</span></span>
- <span data-ttu-id="9a5d5-127">Omezení, které pomáhají s mnoha místech pracujícími clustery</span><span class="sxs-lookup"><span data-stu-id="9a5d5-127">Constraints that help with geographically distributed clusters</span></span>
- <span data-ttu-id="9a5d5-128">Některé architektury aplikace</span><span class="sxs-lookup"><span data-stu-id="9a5d5-128">Certain application architectures</span></span>

<span data-ttu-id="9a5d5-129">Prostřednictvím korelací nebo zásady jsou nakonfigurované další pravidla pro umístění.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-129">Other placement rules are configured via either Correlations or Policies.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a5d5-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a5d5-130">Next steps</span></span>
- <span data-ttu-id="9a5d5-131">Metriky se, jak správce prostředků služby Fabric clusteru spravuje využívání a kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-131">Metrics are how the Service Fabric Cluster Resource Manger manages consumption and capacity in the cluster.</span></span> <span data-ttu-id="9a5d5-132">Další informace o metriky a způsob jejich konfigurace, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-132">To learn more about metrics and how to configure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="9a5d5-133">Spřažení je jeden režim, které můžete konfigurovat pro vaše služby.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-133">Affinity is one mode you can configure for your services.</span></span> <span data-ttu-id="9a5d5-134">Není běžné, ale pokud to potřebujete informace o najdete ho [sem](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-134">It is not common, but if you need it you can learn about it [here](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)</span></span>
- <span data-ttu-id="9a5d5-135">Existuje mnoho různých umístění pravidla, která se dá konfigurovat na služby pro zpracování další scénáře.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-135">There are many different placement rules that can be configured on your service to handle additional scenarios.</span></span> <span data-ttu-id="9a5d5-136">Můžete zjistit o těchto zásadách různých umístění [sem](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-136">You can find out about those different placement policies [here](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)</span></span>
- <span data-ttu-id="9a5d5-137">Začít od začátku a [získejte Úvod do Service Fabric clusteru správce prostředků](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-137">Start from the beginning and [get an Introduction to the Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="9a5d5-138">Chcete-li zjistit, o tom, jak správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru, podívejte se na článek na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-138">To find out about how the Cluster Resource Manager manages and balances load in the cluster, check out the article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="9a5d5-139">Správce prostředků clusteru má mnoho možností pro popis clusteru.</span><span class="sxs-lookup"><span data-stu-id="9a5d5-139">The Cluster Resource Manager has many options for describing the cluster.</span></span> <span data-ttu-id="9a5d5-140">Další informace o nich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="9a5d5-140">To find out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>