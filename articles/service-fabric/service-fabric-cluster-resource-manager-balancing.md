---
title: "Vyvážit cluster Azure Service Fabric | Microsoft Docs"
description: "Úvod do vyrovnávání cluster pomocí Service Fabric clusteru správce prostředků."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 34eacb29f324025c1d2803c9690600227d3ec457
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="balancing-your-service-fabric-cluster"></a><span data-ttu-id="89efc-103">Vyrovnávání váš cluster service fabric</span><span class="sxs-lookup"><span data-stu-id="89efc-103">Balancing your service fabric cluster</span></span>
<span data-ttu-id="89efc-104">Správce prostředků clusteru Service Fabric podporuje dynamické zatížení změny reaktivní dodatky nebo odstraňování uzly nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="89efc-104">The Service Fabric Cluster Resource Manager supports dynamic load changes, reacting to additions or removals of nodes or services.</span></span> <span data-ttu-id="89efc-105">Také automaticky opraví narušení omezení a proaktivně znovu vytvoří rovnováhu clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-105">It also automatically corrects constraint violations, and proactively rebalances the cluster.</span></span> <span data-ttu-id="89efc-106">Ale jsou jak často tyto akce provést a co je aktivuje?</span><span class="sxs-lookup"><span data-stu-id="89efc-106">But how often are these actions taken, and what triggers them?</span></span>

<span data-ttu-id="89efc-107">Existují tři různé kategorie práci, kterou provádí správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-107">There are three different categories of work that the Cluster Resource Manager performs.</span></span> <span data-ttu-id="89efc-108">Jsou:</span><span class="sxs-lookup"><span data-stu-id="89efc-108">They are:</span></span>

1. <span data-ttu-id="89efc-109">Umístění – tato fáze se zabývá umístění žádné stavové repliky nebo bezstavové instancí, které chybí.</span><span class="sxs-lookup"><span data-stu-id="89efc-109">Placement – this stage deals with placing any stateful replicas or stateless instances that are missing.</span></span> <span data-ttu-id="89efc-110">Umístění zahrnuje nové služby a zpracování stavových repliky nebo bezstavové instancí, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="89efc-110">Placement includes both new services and handling stateful replicas or stateless instances that have failed.</span></span> <span data-ttu-id="89efc-111">Tady jsou zpracovávány odstranění a vyřazení replik nebo instancí.</span><span class="sxs-lookup"><span data-stu-id="89efc-111">Deleting and dropping replicas or instances are handled here.</span></span>
2. <span data-ttu-id="89efc-112">Omezení kontroluje – tato fáze kontroluje a opravuje porušení omezení jiné umístění (pravidla) v systému.</span><span class="sxs-lookup"><span data-stu-id="89efc-112">Constraint Checks – this stage checks for and corrects violations of the different placement constraints (rules) within the system.</span></span> <span data-ttu-id="89efc-113">Příklady pravidel jsou třeba zajistit, že uzly nejsou přes kapacity a že jsou splněny omezení umístění služby.</span><span class="sxs-lookup"><span data-stu-id="89efc-113">Examples of rules are things like ensuring that nodes are not over capacity and that a service’s placement constraints are met.</span></span>
3. <span data-ttu-id="89efc-114">Vyrovnávání – tato fáze ověří, jestli nové vyrovnání je nezbytné podle nakonfigurované požadované úrovně vyrovnání pro jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="89efc-114">Balancing – this stage checks to see if rebalancing is necessary based on the configured desired level of balance for different metrics.</span></span> <span data-ttu-id="89efc-115">Pokud ano pokusí se najít uspořádání v clusteru, který je více vyrovnáváním.</span><span class="sxs-lookup"><span data-stu-id="89efc-115">If so it attempts to find an arrangement in the cluster that is more balanced.</span></span>

## <a name="configuring-cluster-resource-manager-timers"></a><span data-ttu-id="89efc-116">Konfigurace správce prostředků clusteru časovače</span><span class="sxs-lookup"><span data-stu-id="89efc-116">Configuring Cluster Resource Manager Timers</span></span>
<span data-ttu-id="89efc-117">První sadu ovládacích prvků kolem vyrovnávání jsou sady časovače.</span><span class="sxs-lookup"><span data-stu-id="89efc-117">The first set of controls around balancing are a set of timers.</span></span> <span data-ttu-id="89efc-118">Tyto časovače řídí, jak často správce prostředků clusteru prozkoumá clusteru a provede nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="89efc-118">These timers govern how often the Cluster Resource Manager examines the cluster and takes corrective actions.</span></span>

<span data-ttu-id="89efc-119">Každý z těchto různých typů opravy, které správce prostředků clusteru můžete provést řídí jiné časovače, která řídí její interval.</span><span class="sxs-lookup"><span data-stu-id="89efc-119">Each of these different types of corrections the Cluster Resource Manager can make is controlled by a different timer that governs its frequency.</span></span> <span data-ttu-id="89efc-120">Při každém časovače aktivuje, je naplánován.</span><span class="sxs-lookup"><span data-stu-id="89efc-120">When each timer fires, the task is scheduled.</span></span> <span data-ttu-id="89efc-121">Ve výchozím nastavení správce prostředků:</span><span class="sxs-lookup"><span data-stu-id="89efc-121">By default the Resource Manager:</span></span>

* <span data-ttu-id="89efc-122">kontroluje stav a použije aktualizace (jako záznam, který je uzlem dolů) každých 1/10 sekund</span><span class="sxs-lookup"><span data-stu-id="89efc-122">scans its state and applies updates (like recording that a node is down) every 1/10th of a second</span></span>
* <span data-ttu-id="89efc-123">Nastaví příznak zkontrolujte umístění</span><span class="sxs-lookup"><span data-stu-id="89efc-123">sets the placement check flag</span></span> 
* <span data-ttu-id="89efc-124">Nastaví příznak kontrola omezení za sekundu</span><span class="sxs-lookup"><span data-stu-id="89efc-124">sets the constraint check flag every second</span></span>
* <span data-ttu-id="89efc-125">Nastaví příznak vyrovnávání každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="89efc-125">sets the balancing flag every five seconds.</span></span>

<span data-ttu-id="89efc-126">Níže jsou příklady konfigurace řídících tyto časovače:</span><span class="sxs-lookup"><span data-stu-id="89efc-126">Examples of the configuration governing these timers are below:</span></span>

<span data-ttu-id="89efc-127">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="89efc-127">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

<span data-ttu-id="89efc-128">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="89efc-128">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

<span data-ttu-id="89efc-129">Dnes clusteru provede správce prostředků pouze jednu z těchto akcí najednou, postupně.</span><span class="sxs-lookup"><span data-stu-id="89efc-129">Today the Cluster Resource Manager only performs one of these actions at a time, sequentially.</span></span> <span data-ttu-id="89efc-130">Z tohoto důvodu označujeme těchto časovačů jako "minimální intervaly" a akcí, které získat provedeny, když časovače přejděte jako "nastavení příznaky".</span><span class="sxs-lookup"><span data-stu-id="89efc-130">This is why we refer to these timers as “minimum intervals” and the actions that get taken when the timers go off as "setting flags".</span></span> <span data-ttu-id="89efc-131">Například správce prostředků clusteru má na starosti čekající požadavky na vytvoření služeb před vyrovnávání clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-131">For example, the Cluster Resource Manager takes care of pending requests to create services before balancing the cluster.</span></span> <span data-ttu-id="89efc-132">Jak je vidět ve výchozí časové intervaly Zadaný správce prostředků clusteru hledá nic, co je potřeba udělat často.</span><span class="sxs-lookup"><span data-stu-id="89efc-132">As you can see by the default time intervals specified, the Cluster Resource Manager scans for anything it needs to do frequently.</span></span> <span data-ttu-id="89efc-133">Obvykle znamená, že je sada změny provedené v průběhu každého kroku malé.</span><span class="sxs-lookup"><span data-stu-id="89efc-133">Normally this means that the set of changes made during each step is small.</span></span> <span data-ttu-id="89efc-134">Provedení malých změn často umožňuje správci prostředků clusteru jako odpovídající při problémech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-134">Making small changes frequently allows the Cluster Resource Manager to be responsive when things happen in the cluster.</span></span> <span data-ttu-id="89efc-135">Výchozí časovače poskytují některé dávkování vzhledem k tomu, že mnoho stejné typy událostí, které jsou obvykle současných.</span><span class="sxs-lookup"><span data-stu-id="89efc-135">The default timers provide some batching since many of the same types of events tend to occur simultaneously.</span></span> 

<span data-ttu-id="89efc-136">Například, když nepovede uzly mohou provádět tak celý domén selhání najednou.</span><span class="sxs-lookup"><span data-stu-id="89efc-136">For example, when nodes fail they can do so entire fault domains at a time.</span></span> <span data-ttu-id="89efc-137">Všechny tyto chyby jsou zaznamenána během další stav aktualizace po *PLBRefreshGap*.</span><span class="sxs-lookup"><span data-stu-id="89efc-137">All these failures are captured during the next state update after the *PLBRefreshGap*.</span></span> <span data-ttu-id="89efc-138">Opravy jsou určeny v průběhu následujících umístění, kontrola omezení a vyrovnávání spustí.</span><span class="sxs-lookup"><span data-stu-id="89efc-138">The corrections are determined during the following placement, constraint check, and balancing runs.</span></span> <span data-ttu-id="89efc-139">Ve výchozím nastavení není správce prostředků clusteru kontrolu prostřednictvím čas změny v clusteru a zkusit vyřešit všechny změny najednou.</span><span class="sxs-lookup"><span data-stu-id="89efc-139">By default the Cluster Resource Manager is not scanning through hours of changes in the cluster and trying to address all changes at once.</span></span> <span data-ttu-id="89efc-140">Díky tomu by vedlo k shluky změn.</span><span class="sxs-lookup"><span data-stu-id="89efc-140">Doing so would lead to bursts of churn.</span></span>

<span data-ttu-id="89efc-141">Správce prostředků clusteru musí také doplňující informace, které určí, zda imbalanced clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-141">The Cluster Resource Manager also needs some additional information to determine if the cluster imbalanced.</span></span> <span data-ttu-id="89efc-142">Pro tento máme dva další požadované konfigurace: *BalancingThresholds* a *ActivityThresholds*.</span><span class="sxs-lookup"><span data-stu-id="89efc-142">For that we have two other pieces of configuration: *BalancingThresholds* and *ActivityThresholds*.</span></span>

## <a name="balancing-thresholds"></a><span data-ttu-id="89efc-143">Vyrovnávání prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="89efc-143">Balancing thresholds</span></span>
<span data-ttu-id="89efc-144">Prahovou hodnotu vyrovnávání je hlavní ovládací prvek pro spuštění vyrovnává.</span><span class="sxs-lookup"><span data-stu-id="89efc-144">A Balancing Threshold is the main control for triggering rebalancing.</span></span> <span data-ttu-id="89efc-145">Prahová hodnota vyrovnávání pro metriku _poměr_.</span><span class="sxs-lookup"><span data-stu-id="89efc-145">The Balancing Threshold for a metric is a _ratio_.</span></span> <span data-ttu-id="89efc-146">Pokud zatížení pro metriku na uzlu nejvíce načíst rozdělené podle množství zatížení na uzlu alespoň načíst překročí tuto metriku *BalancingThreshold*, pak je imbalanced clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-146">If the load for a metric on the most loaded node divided by the amount of load on the least loaded node exceeds that metric's *BalancingThreshold*, then the cluster is imbalanced.</span></span> <span data-ttu-id="89efc-147">V důsledku vyrovnávání se aktivuje při příštím zkontroluje Správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-147">As a result balancing is triggered the next time the Cluster Resource Manager checks.</span></span> <span data-ttu-id="89efc-148">*MinLoadBalancingInterval* časovače definuje, jak často by měl správce prostředků clusteru zkontrolujte, zda vyrovnává je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="89efc-148">The *MinLoadBalancingInterval* timer defines how often the Cluster Resource Manager should check if rebalancing is necessary.</span></span> <span data-ttu-id="89efc-149">Kontrola neznamená, že se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="89efc-149">Checking doesn't mean that anything happens.</span></span> 

<span data-ttu-id="89efc-150">Vyrovnávání prahové hodnoty jsou definovány na základě-metrika jako součást definice clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-150">Balancing Thresholds are defined on a per-metric basis as a part of the cluster definition.</span></span> <span data-ttu-id="89efc-151">Další informace o metrikách, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="89efc-151">For more information on metrics, check out [this article](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="89efc-152">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="89efc-152">ClusterManifest.xml</span></span>

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

<span data-ttu-id="89efc-153">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="89efc-153">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<span data-ttu-id="89efc-154"><center>
![Příklad vyrovnávání prahová hodnota][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="89efc-154"><center>
![Balancing Threshold Example][Image1]
</center></span></span>

<span data-ttu-id="89efc-155">V tomto příkladu je každá služba využívání jednu jednotku některé metriky.</span><span class="sxs-lookup"><span data-stu-id="89efc-155">In this example, each service is consuming one unit of some metric.</span></span> <span data-ttu-id="89efc-156">V horní příkladu maximální zatížení na uzlu je pět a minimum je dva.</span><span class="sxs-lookup"><span data-stu-id="89efc-156">In the top example, the maximum load on a node is five and the minimum is two.</span></span> <span data-ttu-id="89efc-157">Řekněme, že vyrovnávání prahová hodnota pro tato metrika je tři.</span><span class="sxs-lookup"><span data-stu-id="89efc-157">Let’s say that the balancing threshold for this metric is three.</span></span> <span data-ttu-id="89efc-158">Vzhledem k tomu, že poměr v clusteru je 5/2 = 2.5 a který je menší než zadaný vyrovnávání tři prahovou hodnotu, jsou rovnoměrně clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-158">Since the ratio in the cluster is 5/2 = 2.5 and that is less than the specified balancing threshold of three, the cluster is balanced.</span></span> <span data-ttu-id="89efc-159">Žádné vyrovnávání se aktivuje, když kontroluje správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-159">No balancing is triggered when the Cluster Resource Manager checks.</span></span>

<span data-ttu-id="89efc-160">V příkladu dolní maximální zatížení na uzlu je 10, při minimálně dva týdny, výsledkem je poměr pět.</span><span class="sxs-lookup"><span data-stu-id="89efc-160">In the bottom example, the maximum load on a node is 10, while the minimum is two, resulting in a ratio of five.</span></span> <span data-ttu-id="89efc-161">Pět je větší než určené vyrovnávání prahová hodnota tří pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="89efc-161">Five is greater than the designated balancing threshold of three for that metric.</span></span> <span data-ttu-id="89efc-162">V důsledku toho vyrovnává spustit bude naplánované příštím aktivuje vyrovnávání časovač.</span><span class="sxs-lookup"><span data-stu-id="89efc-162">As a result, a rebalancing run will be scheduled next time the balancing timer fires.</span></span> <span data-ttu-id="89efc-163">V situaci, jako je to je obvykle distribuován některé zatížení do Uzel3.</span><span class="sxs-lookup"><span data-stu-id="89efc-163">In a situation like this some load is usually distributed to Node3.</span></span> <span data-ttu-id="89efc-164">Vzhledem k tomu, že správce prostředků clusteru Service Fabric nepoužívá typu greedy přístup, může na Uzel2 distribuován některé zatížení.</span><span class="sxs-lookup"><span data-stu-id="89efc-164">Because the Service Fabric Cluster Resource Manager doesn't use a greedy approach, some load could also be distributed to Node2.</span></span> 

<span data-ttu-id="89efc-165"><center>
![Akce příklad vyrovnávání prahová hodnota][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="89efc-165"><center>
![Balancing Threshold Example Actions][Image2]
</center></span></span>

> [!NOTE]
> <span data-ttu-id="89efc-166">"Vyrovnávání" zpracovává dva různé strategie pro správu zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-166">"Balancing" handles two different strategies for managing load in your cluster.</span></span> <span data-ttu-id="89efc-167">Výchozí strategii, která používá správce prostředků clusteru je rozdělovat mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-167">The default strategy that the Cluster Resource Manager uses is to distribute load across the nodes in the cluster.</span></span> <span data-ttu-id="89efc-168">Další strategie je [defragmentace](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="89efc-168">The other strategy is [defragmentation](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span></span> <span data-ttu-id="89efc-169">Defragmentace se provede během stejné vyrovnávání spustit.</span><span class="sxs-lookup"><span data-stu-id="89efc-169">Defragmentation is performed during the same balancing run.</span></span> <span data-ttu-id="89efc-170">Strategie vyrovnávání a defragmentace lze použít pro jiné metriky ve stejném clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-170">The balancing and defragmentation strategies can be used for different metrics within the same cluster.</span></span> <span data-ttu-id="89efc-171">Služba může mít vyrovnávání a defragmentace metriky.</span><span class="sxs-lookup"><span data-stu-id="89efc-171">A service can have both balancing and defragmentation metrics.</span></span> <span data-ttu-id="89efc-172">Defragmentace metrik poměr zatížení v clusteru spustí nové vyvážení v případě, že je _pod_ vyrovnávání prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="89efc-172">For defragmentation metrics, the ratio of the loads in the cluster triggers rebalancing when it is _below_ the balancing threshold.</span></span> 
>

<span data-ttu-id="89efc-173">Získání pod prahovou hodnotou vyrovnávání není explicitní cíle.</span><span class="sxs-lookup"><span data-stu-id="89efc-173">Getting below the balancing threshold is not an explicit goal.</span></span> <span data-ttu-id="89efc-174">Vyrovnávání prahové hodnoty jsou právě *aktivační událost*.</span><span class="sxs-lookup"><span data-stu-id="89efc-174">Balancing Thresholds are just a *trigger*.</span></span> <span data-ttu-id="89efc-175">Při vyrovnávání spustí, určuje správce prostředků clusteru jaká vylepšení mohl zajistit, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="89efc-175">When balancing runs, the Cluster Resource Manager determines what improvements it can make, if any.</span></span> <span data-ttu-id="89efc-176">Právě, protože vyrovnávání vyhledávání je spuštěna neznamená, že nic přesune.</span><span class="sxs-lookup"><span data-stu-id="89efc-176">Just because a balancing search is kicked off doesn't mean anything moves.</span></span> <span data-ttu-id="89efc-177">Někdy je clusteru imbalanced ale příliš omezené opravit.</span><span class="sxs-lookup"><span data-stu-id="89efc-177">Sometimes the cluster is imbalanced but too constrained to correct.</span></span> <span data-ttu-id="89efc-178">Alternativně vylepšení vyžadují přesuny, které jsou příliš [nákladná](service-fabric-cluster-resource-manager-movement-cost.md)).</span><span class="sxs-lookup"><span data-stu-id="89efc-178">Alternatively, the improvements require movements that are too [costly](service-fabric-cluster-resource-manager-movement-cost.md)).</span></span>

## <a name="activity-thresholds"></a><span data-ttu-id="89efc-179">Aktivita prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="89efc-179">Activity thresholds</span></span>
<span data-ttu-id="89efc-180">V některých případech, i když jsou relativně imbalanced uzly *celkový* množství zatížení v clusteru je nízká.</span><span class="sxs-lookup"><span data-stu-id="89efc-180">Sometimes, although nodes are relatively imbalanced, the *total* amount of load in the cluster is low.</span></span> <span data-ttu-id="89efc-181">Nedostatečná zatížení může být přechodný vyhrazené IP adresy, nebo protože clusteru je nový a právě získávání připravili.</span><span class="sxs-lookup"><span data-stu-id="89efc-181">The lack of load could be a transient dip, or because the cluster is new and just getting bootstrapped.</span></span> <span data-ttu-id="89efc-182">V obou případech nemusíte chtít věnovat času vyrovnávání clusteru, protože je málo užitečného.</span><span class="sxs-lookup"><span data-stu-id="89efc-182">In either case, you may not want to spend time balancing the cluster because there’s little to be gained.</span></span> <span data-ttu-id="89efc-183">Pokud cluster podstoupila vyrovnávání by tráví síť a výpočetní prostředky pohyb věcí bez provedení všechny velké *absolutní* rozdíl.</span><span class="sxs-lookup"><span data-stu-id="89efc-183">If the cluster underwent balancing, you’d spend network and compute resources to move things around without making any large *absolute* difference.</span></span> <span data-ttu-id="89efc-184">Aby se zabránilo zbytečným přesune, další ovládací prvek označuje jako aktivita prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="89efc-184">To avoid unnecessary moves, there’s another control known as Activity Thresholds.</span></span> <span data-ttu-id="89efc-185">Prahové hodnoty aktivity umožňuje zadat některé absolutní dolní mez aktivity.</span><span class="sxs-lookup"><span data-stu-id="89efc-185">Activity Thresholds allows you to specify some absolute lower bound for activity.</span></span> <span data-ttu-id="89efc-186">Pokud žádný uzel nad touto prahovou hodnotou, vyrovnávání není aktivované i v případě dosáhla prahová hodnota vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="89efc-186">If no node is over this threshold, balancing isn't triggered even if the Balancing Threshold is met.</span></span>

<span data-ttu-id="89efc-187">Řekněme, že jsme zachovat naše vyrovnávání prahovou hodnotu tři pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="89efc-187">Let’s say that we retain our Balancing Threshold of three for this metric.</span></span> <span data-ttu-id="89efc-188">Také se stát, že máme 1536 prahovou hodnotu aktivity.</span><span class="sxs-lookup"><span data-stu-id="89efc-188">Let's also say we have an Activity Threshold of 1536.</span></span> <span data-ttu-id="89efc-189">V prvním případě clusteru je imbalanced za vyrovnávání prahová hodnota je žádný uzel splňuje této prahové hodnoty aktivity, nedojde k žádné akci.</span><span class="sxs-lookup"><span data-stu-id="89efc-189">In the first case, while the cluster is imbalanced per the Balancing Threshold there's no node meets that Activity Threshold, so nothing happens.</span></span> <span data-ttu-id="89efc-190">V příkladu dolní Uzel1 je nad prahovou hodnotou aktivity.</span><span class="sxs-lookup"><span data-stu-id="89efc-190">In the bottom example, Node1 is over the Activity Threshold.</span></span> <span data-ttu-id="89efc-191">Vzhledem k tomu, že se překročí prahovou hodnotu vyrovnávání a aktivity prahovou hodnotu pro metriku, vyrovnávání naplánován.</span><span class="sxs-lookup"><span data-stu-id="89efc-191">Since both the Balancing Threshold and the Activity Threshold for the metric are exceeded, balancing is scheduled.</span></span> <span data-ttu-id="89efc-192">Jako příklad Podíváme se na následující diagram:</span><span class="sxs-lookup"><span data-stu-id="89efc-192">As an example, let's look at the following diagram:</span></span> 

<span data-ttu-id="89efc-193"><center>
![Příklad aktivity prahová hodnota][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="89efc-193"><center>
![Activity Threshold Example][Image3]
</center></span></span>

<span data-ttu-id="89efc-194">Stejně jako vyrovnávání prahové hodnoty aktivity prahové hodnoty jsou definované za metriku prostřednictvím definice clusteru:</span><span class="sxs-lookup"><span data-stu-id="89efc-194">Just like Balancing Thresholds, Activity Thresholds are defined per-metric via the cluster definition:</span></span>

<span data-ttu-id="89efc-195">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="89efc-195">ClusterManifest.xml</span></span>

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

<span data-ttu-id="89efc-196">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="89efc-196">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

<span data-ttu-id="89efc-197">Prahové hodnoty vyrovnávání a aktivity jsou obě vázaný na konkrétní metrika - vyrovnávání se spustí pouze v případě překročení vyrovnávání prahová hodnota i aktivity prahové hodnoty pro stejnou metriku.</span><span class="sxs-lookup"><span data-stu-id="89efc-197">Balancing and activity thresholds are both tied to a specific metric - balancing is triggered only if both the Balancing Threshold and Activity Threshold is exceeded for the same metric.</span></span>

## <a name="balancing-services-together"></a><span data-ttu-id="89efc-198">Společně vyrovnávání služby</span><span class="sxs-lookup"><span data-stu-id="89efc-198">Balancing services together</span></span>
<span data-ttu-id="89efc-199">Zda je cluster imbalanced nebo ne je platné pro celý cluster rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="89efc-199">Whether the cluster is imbalanced or not is a cluster-wide decision.</span></span> <span data-ttu-id="89efc-200">Způsob, jakým jsme přejděte o její opravu je však přesunutí repliky jednotlivé služby a instance kolem.</span><span class="sxs-lookup"><span data-stu-id="89efc-200">However, the way we go about fixing it is moving individual service replicas and instances around.</span></span> <span data-ttu-id="89efc-201">To dává smysl, ne?</span><span class="sxs-lookup"><span data-stu-id="89efc-201">This makes sense, right?</span></span> <span data-ttu-id="89efc-202">Pokud paměti je skládaný na jednom uzlu, může být více replik nebo instancí přispívání do ho.</span><span class="sxs-lookup"><span data-stu-id="89efc-202">If memory is stacked up on one node, multiple replicas or instances could be contributing to it.</span></span> <span data-ttu-id="89efc-203">Opravě nevyváženosti může vyžadovat přesunutí stavová repliky nebo bezstavové instancí, které použít imbalanced metriku.</span><span class="sxs-lookup"><span data-stu-id="89efc-203">Fixing the imbalance could require moving any of the stateful replicas or stateless instances that use the imbalanced metric.</span></span>

<span data-ttu-id="89efc-204">Občas, když služba, která nebyla samotné imbalanced získá přesunout (Nezapomeňte diskuzi o místní a globální provede dříve).</span><span class="sxs-lookup"><span data-stu-id="89efc-204">Occasionally though, a service that wasn’t itself imbalanced gets moved (remember the discussion of local and global weights earlier).</span></span> <span data-ttu-id="89efc-205">Proč by služby získat přesunuta, když všechno, co byly vyrovnáváním metriky služby?</span><span class="sxs-lookup"><span data-stu-id="89efc-205">Why would a service get moved when all that service’s metrics were balanced?</span></span> <span data-ttu-id="89efc-206">Podíváme se na příklad:</span><span class="sxs-lookup"><span data-stu-id="89efc-206">Let’s see an example:</span></span>

- <span data-ttu-id="89efc-207">Řekněme, že existují čtyři služby, Service1, Service2, Service3 a Service4.</span><span class="sxs-lookup"><span data-stu-id="89efc-207">Let's say there are four services, Service1, Service2, Service3, and Service4.</span></span> 
- <span data-ttu-id="89efc-208">Service1 sestavy metrik Metric1 a Metric2.</span><span class="sxs-lookup"><span data-stu-id="89efc-208">Service1 reports metrics Metric1 and Metric2.</span></span> 
- <span data-ttu-id="89efc-209">Service2 sestavy metrik Metric2 a Metric3.</span><span class="sxs-lookup"><span data-stu-id="89efc-209">Service2 reports metrics Metric2 and Metric3.</span></span> 
- <span data-ttu-id="89efc-210">Service3 sestavy metrik Metric3 a Metric4.</span><span class="sxs-lookup"><span data-stu-id="89efc-210">Service3 reports metrics Metric3 and Metric4.</span></span>
- <span data-ttu-id="89efc-211">Service4 hlásí metriky Metric99.</span><span class="sxs-lookup"><span data-stu-id="89efc-211">Service4 reports metric Metric99.</span></span> 

<span data-ttu-id="89efc-212">Surely se zobrazí, kde vytvoříme zde: existuje řetězec!</span><span class="sxs-lookup"><span data-stu-id="89efc-212">Surely you can see where we’re going here: There's a chain!</span></span> <span data-ttu-id="89efc-213">Nemáme skutečně čtyři nezávislé služby, máme tři služby, které se vztahují a ten, který je vypnutý o sobě.</span><span class="sxs-lookup"><span data-stu-id="89efc-213">We don’t really have four independent services, we have three services that are related and one that is off on its own.</span></span>

<span data-ttu-id="89efc-214"><center>
![Společně vyrovnávání služby][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="89efc-214"><center>
![Balancing Services Together][Image4]
</center></span></span>

<span data-ttu-id="89efc-215">Z důvodu tento řetězec je možné, že byl zjištěn rozdíl v metriky 1 – 4 může způsobit repliky nebo instance, které patří ke službám 1 – 3 pohyb.</span><span class="sxs-lookup"><span data-stu-id="89efc-215">Because of this chain, it's possible that an imbalance in metrics 1-4 can cause replicas or instances belonging to services 1-3 to move around.</span></span> <span data-ttu-id="89efc-216">Taky víme, že byl zjištěn rozdíl v metriky 1, 2 nebo 3 nelze způsobit pohybů v Service4.</span><span class="sxs-lookup"><span data-stu-id="89efc-216">We also know that an imbalance in Metrics 1, 2, or 3 can't cause movements in Service4.</span></span> <span data-ttu-id="89efc-217">Od přesunutí repliky by být žádný bod, nebo instance, které patří k Service4 kolem provést absolutně nic mít dopad na rovnováhu mezi počtem metriky 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="89efc-217">There would be no point since moving the replicas or instances belonging to Service4 around can do absolutely nothing to impact the balance of Metrics 1-3.</span></span>

<span data-ttu-id="89efc-218">Správce prostředků clusteru automaticky hodnoty na jaké služby jsou související.</span><span class="sxs-lookup"><span data-stu-id="89efc-218">The Cluster Resource Manager automatically figures out what services are related.</span></span> <span data-ttu-id="89efc-219">Přidání, odebrání nebo změna metriky pro služby může ovlivnit jejich vztahů.</span><span class="sxs-lookup"><span data-stu-id="89efc-219">Adding, removing, or changing the metrics for services can impact their relationships.</span></span> <span data-ttu-id="89efc-220">Například mezi dvěma spustí vyrovnávání Service2 může mít aktualizovaná tak, aby odebrat Metric2.</span><span class="sxs-lookup"><span data-stu-id="89efc-220">For example, between two runs of balancing Service2 may have been updated to remove Metric2.</span></span> <span data-ttu-id="89efc-221">Tím se přeruší řetězu mezi Service1 a Service2.</span><span class="sxs-lookup"><span data-stu-id="89efc-221">This breaks the chain between Service1 and Service2.</span></span> <span data-ttu-id="89efc-222">Teď místo dvě skupiny související služby, existují tři:</span><span class="sxs-lookup"><span data-stu-id="89efc-222">Now instead of two groups of related services, there are three:</span></span>

<span data-ttu-id="89efc-223"><center>
![Společně vyrovnávání služby][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="89efc-223"><center>
![Balancing Services Together][Image5]
</center></span></span>

## <a name="next-steps"></a><span data-ttu-id="89efc-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89efc-224">Next steps</span></span>
* <span data-ttu-id="89efc-225">Metriky se, jak správce prostředků služby Fabric clusteru spravuje využívání a kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-225">Metrics are how the Service Fabric Cluster Resource Manger manages consumption and capacity in the cluster.</span></span> <span data-ttu-id="89efc-226">Další informace o metriky a způsob jejich konfigurace, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="89efc-226">To learn more about metrics and how to configure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
* <span data-ttu-id="89efc-227">Náklady na přesunutí je jeden ze způsobů signalizace správce prostředků clusteru, že jsou dražší než jiné přesunout určité služby.</span><span class="sxs-lookup"><span data-stu-id="89efc-227">Movement Cost is one way of signaling to the Cluster Resource Manager that certain services are more expensive to move than others.</span></span> <span data-ttu-id="89efc-228">Další informace o náklady na přesunutí, najdete v části [v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md)</span><span class="sxs-lookup"><span data-stu-id="89efc-228">For more about movement cost, refer to [this article](service-fabric-cluster-resource-manager-movement-cost.md)</span></span>
* <span data-ttu-id="89efc-229">Správce prostředků clusteru má několik omezení, které můžete nakonfigurovat zpomalit změn v clusteru.</span><span class="sxs-lookup"><span data-stu-id="89efc-229">The Cluster Resource Manager has several throttles that you can configure to slow down churn in the cluster.</span></span> <span data-ttu-id="89efc-230">Nejsou běžně potřebné, ale chcete-li se dozvíte je [sem](service-fabric-cluster-resource-manager-advanced-throttling.md)</span><span class="sxs-lookup"><span data-stu-id="89efc-230">They're not normally necessary, but if you need them you can learn about them [here](service-fabric-cluster-resource-manager-advanced-throttling.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
