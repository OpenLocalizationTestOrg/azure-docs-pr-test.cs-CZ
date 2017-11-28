---
title: aaaBalance cluster Azure Service Fabric | Microsoft Docs
description: "Úvod toobalancing clusteru pomocí hello správce prostředků clusteru Service Fabric."
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
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a><span data-ttu-id="0cd03-103">Vyrovnávání váš cluster service fabric</span><span class="sxs-lookup"><span data-stu-id="0cd03-103">Balancing your service fabric cluster</span></span>
<span data-ttu-id="0cd03-104">Hello správce prostředků clusteru Service Fabric podporuje dynamické zatížení změny, reagující tooadditions nebo odstraňování uzly nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="0cd03-104">hello Service Fabric Cluster Resource Manager supports dynamic load changes, reacting tooadditions or removals of nodes or services.</span></span> <span data-ttu-id="0cd03-105">Také automaticky opraví narušení omezení a proaktivně znovu vytvoří rovnováhu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-105">It also automatically corrects constraint violations, and proactively rebalances hello cluster.</span></span> <span data-ttu-id="0cd03-106">Ale jsou jak často tyto akce provést a co je aktivuje?</span><span class="sxs-lookup"><span data-stu-id="0cd03-106">But how often are these actions taken, and what triggers them?</span></span>

<span data-ttu-id="0cd03-107">Existují tři různé kategorie pracovní této hello, které provádí správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-107">There are three different categories of work that hello Cluster Resource Manager performs.</span></span> <span data-ttu-id="0cd03-108">Jsou:</span><span class="sxs-lookup"><span data-stu-id="0cd03-108">They are:</span></span>

1. <span data-ttu-id="0cd03-109">Umístění – tato fáze se zabývá umístění žádné stavové repliky nebo bezstavové instancí, které chybí.</span><span class="sxs-lookup"><span data-stu-id="0cd03-109">Placement – this stage deals with placing any stateful replicas or stateless instances that are missing.</span></span> <span data-ttu-id="0cd03-110">Umístění zahrnuje nové služby a zpracování stavových repliky nebo bezstavové instancí, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="0cd03-110">Placement includes both new services and handling stateful replicas or stateless instances that have failed.</span></span> <span data-ttu-id="0cd03-111">Tady jsou zpracovávány odstranění a vyřazení replik nebo instancí.</span><span class="sxs-lookup"><span data-stu-id="0cd03-111">Deleting and dropping replicas or instances are handled here.</span></span>
2. <span data-ttu-id="0cd03-112">Omezení kontroluje – tato fáze kontroluje a opravuje porušení omezení různých umístění hello (pravidla) v rámci systému hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-112">Constraint Checks – this stage checks for and corrects violations of hello different placement constraints (rules) within hello system.</span></span> <span data-ttu-id="0cd03-113">Příklady pravidel jsou třeba zajistit, že uzly nejsou přes kapacity a že jsou splněny omezení umístění služby.</span><span class="sxs-lookup"><span data-stu-id="0cd03-113">Examples of rules are things like ensuring that nodes are not over capacity and that a service’s placement constraints are met.</span></span>
3. <span data-ttu-id="0cd03-114">Vyrovnávání – tato fáze ověří toosee Pokud vyrovnává je nutné hello nakonfigurovaný podle potřeby úroveň vyrovnání pro jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="0cd03-114">Balancing – this stage checks toosee if rebalancing is necessary based on hello configured desired level of balance for different metrics.</span></span> <span data-ttu-id="0cd03-115">Pokud ano pokusí toofind uspořádání v hello clusteru tedy více vyrovnáváním.</span><span class="sxs-lookup"><span data-stu-id="0cd03-115">If so it attempts toofind an arrangement in hello cluster that is more balanced.</span></span>

## <a name="configuring-cluster-resource-manager-timers"></a><span data-ttu-id="0cd03-116">Konfigurace správce prostředků clusteru časovače</span><span class="sxs-lookup"><span data-stu-id="0cd03-116">Configuring Cluster Resource Manager Timers</span></span>
<span data-ttu-id="0cd03-117">Hello první sadu ovládacích prvků kolem vyrovnávání jsou sady časovače.</span><span class="sxs-lookup"><span data-stu-id="0cd03-117">hello first set of controls around balancing are a set of timers.</span></span> <span data-ttu-id="0cd03-118">Tyto časovače řídí, jak často hello správce prostředků clusteru prozkoumá hello clusteru a provede nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="0cd03-118">These timers govern how often hello Cluster Resource Manager examines hello cluster and takes corrective actions.</span></span>

<span data-ttu-id="0cd03-119">Každý z těchto různých typů hello opravy, které má správce prostředků clusteru řídí jiné časovače, která řídí její interval.</span><span class="sxs-lookup"><span data-stu-id="0cd03-119">Each of these different types of corrections hello Cluster Resource Manager can make is controlled by a different timer that governs its frequency.</span></span> <span data-ttu-id="0cd03-120">Při každém časovače hello je naplánován.</span><span class="sxs-lookup"><span data-stu-id="0cd03-120">When each timer fires, hello task is scheduled.</span></span> <span data-ttu-id="0cd03-121">Ve výchozím nastavení hello Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="0cd03-121">By default hello Resource Manager:</span></span>

* <span data-ttu-id="0cd03-122">kontroluje stav a použije aktualizace (jako záznam, který je uzlem dolů) každých 1/10 sekund</span><span class="sxs-lookup"><span data-stu-id="0cd03-122">scans its state and applies updates (like recording that a node is down) every 1/10th of a second</span></span>
* <span data-ttu-id="0cd03-123">Nastaví příznak zkontrolujte umístění hello</span><span class="sxs-lookup"><span data-stu-id="0cd03-123">sets hello placement check flag</span></span> 
* <span data-ttu-id="0cd03-124">Nastaví příznak kontrola omezení hello za sekundu</span><span class="sxs-lookup"><span data-stu-id="0cd03-124">sets hello constraint check flag every second</span></span>
* <span data-ttu-id="0cd03-125">Nastaví hello vyrovnávání příznak každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="0cd03-125">sets hello balancing flag every five seconds.</span></span>

<span data-ttu-id="0cd03-126">Níže jsou příklady konfigurace hello řídících tyto časovače:</span><span class="sxs-lookup"><span data-stu-id="0cd03-126">Examples of hello configuration governing these timers are below:</span></span>

<span data-ttu-id="0cd03-127">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="0cd03-127">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

<span data-ttu-id="0cd03-128">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="0cd03-128">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0cd03-129">Dnes hello clusteru Resource Manager provádí pouze jednu z těchto akcí najednou, postupně.</span><span class="sxs-lookup"><span data-stu-id="0cd03-129">Today hello Cluster Resource Manager only performs one of these actions at a time, sequentially.</span></span> <span data-ttu-id="0cd03-130">Z tohoto důvodu jsme naleznete toothese časovače jako "minimální intervaly" a hello akce, které získat provedeny, když přejít časovačů hello jako "nastavení příznaky".</span><span class="sxs-lookup"><span data-stu-id="0cd03-130">This is why we refer toothese timers as “minimum intervals” and hello actions that get taken when hello timers go off as "setting flags".</span></span> <span data-ttu-id="0cd03-131">Například hello, který má na starosti clusteru Resource Manager čeká na vyřízení požadavků služby toocreate před hello cluster vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="0cd03-131">For example, hello Cluster Resource Manager takes care of pending requests toocreate services before balancing hello cluster.</span></span> <span data-ttu-id="0cd03-132">Jak je vidět ve hello výchozí časové intervaly zadaný hello správce prostředků clusteru kontroluje pro všechno, co ji toodo potřebám často.</span><span class="sxs-lookup"><span data-stu-id="0cd03-132">As you can see by hello default time intervals specified, hello Cluster Resource Manager scans for anything it needs toodo frequently.</span></span> <span data-ttu-id="0cd03-133">Obvykle znamená, že je malý hello sadu změny provedené v průběhu každého kroku.</span><span class="sxs-lookup"><span data-stu-id="0cd03-133">Normally this means that hello set of changes made during each step is small.</span></span> <span data-ttu-id="0cd03-134">Provedení malých změn často umožňuje hello správce prostředků clusteru toobe přizpůsobivý při problémech v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-134">Making small changes frequently allows hello Cluster Resource Manager toobe responsive when things happen in hello cluster.</span></span> <span data-ttu-id="0cd03-135">Hello výchozí časovače poskytují některé dávkování od řadu hello stejné typy událostí, které jsou obvykle toooccur současně.</span><span class="sxs-lookup"><span data-stu-id="0cd03-135">hello default timers provide some batching since many of hello same types of events tend toooccur simultaneously.</span></span> 

<span data-ttu-id="0cd03-136">Například, když nepovede uzly mohou provádět tak celý domén selhání najednou.</span><span class="sxs-lookup"><span data-stu-id="0cd03-136">For example, when nodes fail they can do so entire fault domains at a time.</span></span> <span data-ttu-id="0cd03-137">Všechny tyto chyby jsou zaznamenána během hello další stav aktualizace po hello *PLBRefreshGap*.</span><span class="sxs-lookup"><span data-stu-id="0cd03-137">All these failures are captured during hello next state update after hello *PLBRefreshGap*.</span></span> <span data-ttu-id="0cd03-138">Hello opravy jsou určeny v průběhu hello následující umístění, kontrola omezení a vyrovnávání spustí.</span><span class="sxs-lookup"><span data-stu-id="0cd03-138">hello corrections are determined during hello following placement, constraint check, and balancing runs.</span></span> <span data-ttu-id="0cd03-139">Správce prostředků clusteru není ve výchozím nastavení hello kontrolu prostřednictvím čas změny v clusteru hello a zkusit tooaddress všechny změny najednou.</span><span class="sxs-lookup"><span data-stu-id="0cd03-139">By default hello Cluster Resource Manager is not scanning through hours of changes in hello cluster and trying tooaddress all changes at once.</span></span> <span data-ttu-id="0cd03-140">Díky tomu by vést toobursts změn.</span><span class="sxs-lookup"><span data-stu-id="0cd03-140">Doing so would lead toobursts of churn.</span></span>

<span data-ttu-id="0cd03-141">Hello správce prostředků clusteru také musí některé další informace o toodetermine Pokud hello clusteru imbalanced.</span><span class="sxs-lookup"><span data-stu-id="0cd03-141">hello Cluster Resource Manager also needs some additional information toodetermine if hello cluster imbalanced.</span></span> <span data-ttu-id="0cd03-142">Pro tento máme dva další požadované konfigurace: *BalancingThresholds* a *ActivityThresholds*.</span><span class="sxs-lookup"><span data-stu-id="0cd03-142">For that we have two other pieces of configuration: *BalancingThresholds* and *ActivityThresholds*.</span></span>

## <a name="balancing-thresholds"></a><span data-ttu-id="0cd03-143">Vyrovnávání prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="0cd03-143">Balancing thresholds</span></span>
<span data-ttu-id="0cd03-144">Prahovou hodnotu vyrovnávání je hello hlavní řízení pro spuštění vyrovnává.</span><span class="sxs-lookup"><span data-stu-id="0cd03-144">A Balancing Threshold is hello main control for triggering rebalancing.</span></span> <span data-ttu-id="0cd03-145">Hello vyrovnávání prahová hodnota pro metriku je _poměr_.</span><span class="sxs-lookup"><span data-stu-id="0cd03-145">hello Balancing Threshold for a metric is a _ratio_.</span></span> <span data-ttu-id="0cd03-146">Pokud hello zatížení pro metriku na hello nejvíce načíst uzlu dělený hello množství zatížení na hello alespoň načíst uzlu překročí tuto metriku *BalancingThreshold*, pak je imbalanced hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-146">If hello load for a metric on hello most loaded node divided by hello amount of load on hello least loaded node exceeds that metric's *BalancingThreshold*, then hello cluster is imbalanced.</span></span> <span data-ttu-id="0cd03-147">V důsledku vyrovnávání je spouštěná hello další čas hello kontroly správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-147">As a result balancing is triggered hello next time hello Cluster Resource Manager checks.</span></span> <span data-ttu-id="0cd03-148">Hello *MinLoadBalancingInterval* časovače definuje, jak často hello clusteru Resource Manager by měla zkontrolujte, zda vyrovnává je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="0cd03-148">hello *MinLoadBalancingInterval* timer defines how often hello Cluster Resource Manager should check if rebalancing is necessary.</span></span> <span data-ttu-id="0cd03-149">Kontrola neznamená, že se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="0cd03-149">Checking doesn't mean that anything happens.</span></span> 

<span data-ttu-id="0cd03-150">Vyrovnávání prahové hodnoty jsou definovány na základě-metrika jako součást definice hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-150">Balancing Thresholds are defined on a per-metric basis as a part of hello cluster definition.</span></span> <span data-ttu-id="0cd03-151">Další informace o metrikách, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0cd03-151">For more information on metrics, check out [this article](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="0cd03-152">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="0cd03-152">ClusterManifest.xml</span></span>

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

<span data-ttu-id="0cd03-153">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="0cd03-153">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0cd03-154"><center>
![Příklad vyrovnávání prahová hodnota][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="0cd03-154"><center>
![Balancing Threshold Example][Image1]
</center></span></span>

<span data-ttu-id="0cd03-155">V tomto příkladu je každá služba využívání jednu jednotku některé metriky.</span><span class="sxs-lookup"><span data-stu-id="0cd03-155">In this example, each service is consuming one unit of some metric.</span></span> <span data-ttu-id="0cd03-156">V příkladu nejvyšší hello hello maximální zatížení na uzlu je pět a minimální hello je dva.</span><span class="sxs-lookup"><span data-stu-id="0cd03-156">In hello top example, hello maximum load on a node is five and hello minimum is two.</span></span> <span data-ttu-id="0cd03-157">Řekněme, že tento hello vyrovnávání prahová hodnota pro tato metrika je tři.</span><span class="sxs-lookup"><span data-stu-id="0cd03-157">Let’s say that hello balancing threshold for this metric is three.</span></span> <span data-ttu-id="0cd03-158">Vzhledem k tomu, že poměr hello v clusteru hello je 5/2 = 2.5 a který je menší než hello zadaných vyrovnávání prahovou hodnotu tři, hello clusteru jsou rovnoměrně.</span><span class="sxs-lookup"><span data-stu-id="0cd03-158">Since hello ratio in hello cluster is 5/2 = 2.5 and that is less than hello specified balancing threshold of three, hello cluster is balanced.</span></span> <span data-ttu-id="0cd03-159">Žádné vyrovnávání se aktivuje, když kontroluje hello správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-159">No balancing is triggered when hello Cluster Resource Manager checks.</span></span>

<span data-ttu-id="0cd03-160">V příkladu dolní hello hello maximální zatížení na uzlu je 10, zatímco hello je minimálně dva týdny, výsledkem je poměr pět.</span><span class="sxs-lookup"><span data-stu-id="0cd03-160">In hello bottom example, hello maximum load on a node is 10, while hello minimum is two, resulting in a ratio of five.</span></span> <span data-ttu-id="0cd03-161">Pět je větší než hello určené vyrovnávání prahovou hodnotu tři pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="0cd03-161">Five is greater than hello designated balancing threshold of three for that metric.</span></span> <span data-ttu-id="0cd03-162">V důsledku toho budou rebalancing spustit naplánované další čas hello vyrovnávání aktivuje časovače.</span><span class="sxs-lookup"><span data-stu-id="0cd03-162">As a result, a rebalancing run will be scheduled next time hello balancing timer fires.</span></span> <span data-ttu-id="0cd03-163">Některé zatížení v situaci, jako je to je obvykle distribuované tooNode3.</span><span class="sxs-lookup"><span data-stu-id="0cd03-163">In a situation like this some load is usually distributed tooNode3.</span></span> <span data-ttu-id="0cd03-164">Protože hello správce prostředků clusteru Service Fabric nepoužívá typu greedy přístup, některé zatížení mohou být také distribuované tooNode2.</span><span class="sxs-lookup"><span data-stu-id="0cd03-164">Because hello Service Fabric Cluster Resource Manager doesn't use a greedy approach, some load could also be distributed tooNode2.</span></span> 

<span data-ttu-id="0cd03-165"><center>
![Akce příklad vyrovnávání prahová hodnota][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="0cd03-165"><center>
![Balancing Threshold Example Actions][Image2]
</center></span></span>

> [!NOTE]
> <span data-ttu-id="0cd03-166">"Vyrovnávání" zpracovává dva různé strategie pro správu zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-166">"Balancing" handles two different strategies for managing load in your cluster.</span></span> <span data-ttu-id="0cd03-167">Hello výchozí strategie, která hello používá správce prostředků clusteru je toodistribute zatížení mezi hello uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-167">hello default strategy that hello Cluster Resource Manager uses is toodistribute load across hello nodes in hello cluster.</span></span> <span data-ttu-id="0cd03-168">Hello další strategie je [defragmentace](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0cd03-168">hello other strategy is [defragmentation](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span></span> <span data-ttu-id="0cd03-169">Defragmentace provádí během hello stejné vyrovnávání spustit.</span><span class="sxs-lookup"><span data-stu-id="0cd03-169">Defragmentation is performed during hello same balancing run.</span></span> <span data-ttu-id="0cd03-170">Hello vyrovnávání a defragmentace strategie lze použít pro jiné metriky v rámci hello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="0cd03-170">hello balancing and defragmentation strategies can be used for different metrics within hello same cluster.</span></span> <span data-ttu-id="0cd03-171">Služba může mít vyrovnávání a defragmentace metriky.</span><span class="sxs-lookup"><span data-stu-id="0cd03-171">A service can have both balancing and defragmentation metrics.</span></span> <span data-ttu-id="0cd03-172">Defragmentace metrik hello poměr hello načte v aktivační události clusteru hello nové vyvážení v případě, že je _pod_ hello vyrovnávání prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0cd03-172">For defragmentation metrics, hello ratio of hello loads in hello cluster triggers rebalancing when it is _below_ hello balancing threshold.</span></span> 
>

<span data-ttu-id="0cd03-173">Získání níže hello vyrovnávání prahová hodnota není explicitní cíle.</span><span class="sxs-lookup"><span data-stu-id="0cd03-173">Getting below hello balancing threshold is not an explicit goal.</span></span> <span data-ttu-id="0cd03-174">Vyrovnávání prahové hodnoty jsou právě *aktivační událost*.</span><span class="sxs-lookup"><span data-stu-id="0cd03-174">Balancing Thresholds are just a *trigger*.</span></span> <span data-ttu-id="0cd03-175">Při vyrovnávání spustí, určuje hello správce prostředků clusteru jaká vylepšení mohl zajistit, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="0cd03-175">When balancing runs, hello Cluster Resource Manager determines what improvements it can make, if any.</span></span> <span data-ttu-id="0cd03-176">Právě, protože vyrovnávání vyhledávání je spuštěna neznamená, že nic přesune.</span><span class="sxs-lookup"><span data-stu-id="0cd03-176">Just because a balancing search is kicked off doesn't mean anything moves.</span></span> <span data-ttu-id="0cd03-177">Někdy hello clusteru je toocorrect imbalanced ale příliš omezené.</span><span class="sxs-lookup"><span data-stu-id="0cd03-177">Sometimes hello cluster is imbalanced but too constrained toocorrect.</span></span> <span data-ttu-id="0cd03-178">Můžete taky vyžadovat vylepšení hello pohybů, které jsou příliš [nákladná](service-fabric-cluster-resource-manager-movement-cost.md)).</span><span class="sxs-lookup"><span data-stu-id="0cd03-178">Alternatively, hello improvements require movements that are too [costly](service-fabric-cluster-resource-manager-movement-cost.md)).</span></span>

## <a name="activity-thresholds"></a><span data-ttu-id="0cd03-179">Aktivita prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="0cd03-179">Activity thresholds</span></span>
<span data-ttu-id="0cd03-180">V některých případech i když jsou relativně imbalanced uzly, hello *celkový* množství zatížení v clusteru hello je nízká.</span><span class="sxs-lookup"><span data-stu-id="0cd03-180">Sometimes, although nodes are relatively imbalanced, hello *total* amount of load in hello cluster is low.</span></span> <span data-ttu-id="0cd03-181">Nedostatečná Hello zatížení může být přechodný vyhrazené IP adresy, nebo protože hello clusteru je nový a právě získávání připravili.</span><span class="sxs-lookup"><span data-stu-id="0cd03-181">hello lack of load could be a transient dip, or because hello cluster is new and just getting bootstrapped.</span></span> <span data-ttu-id="0cd03-182">V obou případech nemusí chcete toospend čas vyrovnávání hello clusteru, protože je moc toobe získávají.</span><span class="sxs-lookup"><span data-stu-id="0cd03-182">In either case, you may not want toospend time balancing hello cluster because there’s little toobe gained.</span></span> <span data-ttu-id="0cd03-183">Pokud hello cluster podstoupila vyrovnávání by tráví síť a výpočetní prostředky toomove věcí kolem bez provedení všechny velké *absolutní* rozdíl.</span><span class="sxs-lookup"><span data-stu-id="0cd03-183">If hello cluster underwent balancing, you’d spend network and compute resources toomove things around without making any large *absolute* difference.</span></span> <span data-ttu-id="0cd03-184">Přesune tooavoid nepotřebné, je další ovládací prvek označuje jako aktivita prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0cd03-184">tooavoid unnecessary moves, there’s another control known as Activity Thresholds.</span></span> <span data-ttu-id="0cd03-185">Prahové hodnoty aktivity vám umožní toospecify některé absolutní dolní mez aktivity.</span><span class="sxs-lookup"><span data-stu-id="0cd03-185">Activity Thresholds allows you toospecify some absolute lower bound for activity.</span></span> <span data-ttu-id="0cd03-186">Pokud žádný uzel nad touto prahovou hodnotou, vyrovnávání není aktivované i v případě hello vyrovnávání dosáhla prahová hodnota.</span><span class="sxs-lookup"><span data-stu-id="0cd03-186">If no node is over this threshold, balancing isn't triggered even if hello Balancing Threshold is met.</span></span>

<span data-ttu-id="0cd03-187">Řekněme, že jsme zachovat naše vyrovnávání prahovou hodnotu tři pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="0cd03-187">Let’s say that we retain our Balancing Threshold of three for this metric.</span></span> <span data-ttu-id="0cd03-188">Také se stát, že máme 1536 prahovou hodnotu aktivity.</span><span class="sxs-lookup"><span data-stu-id="0cd03-188">Let's also say we have an Activity Threshold of 1536.</span></span> <span data-ttu-id="0cd03-189">V prvním případě hello při imbalanced za hello hello clusteru vyrovnávání prahová hodnota došlo je žádný uzel splňuje prahové hodnoty aktivity, nic se stane.</span><span class="sxs-lookup"><span data-stu-id="0cd03-189">In hello first case, while hello cluster is imbalanced per hello Balancing Threshold there's no node meets that Activity Threshold, so nothing happens.</span></span> <span data-ttu-id="0cd03-190">V příkladu dolní hello Uzel1 je nad hello aktivity prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0cd03-190">In hello bottom example, Node1 is over hello Activity Threshold.</span></span> <span data-ttu-id="0cd03-191">Vzhledem k tomu, že oba hello vyrovnávání prahovou hodnotu a hello aktivity prahovou hodnotu pro metriku hello překročen, vyrovnávání naplánován.</span><span class="sxs-lookup"><span data-stu-id="0cd03-191">Since both hello Balancing Threshold and hello Activity Threshold for hello metric are exceeded, balancing is scheduled.</span></span> <span data-ttu-id="0cd03-192">Jako příklad Podíváme se na následující diagram hello:</span><span class="sxs-lookup"><span data-stu-id="0cd03-192">As an example, let's look at hello following diagram:</span></span> 

<span data-ttu-id="0cd03-193"><center>
![Příklad aktivity prahová hodnota][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="0cd03-193"><center>
![Activity Threshold Example][Image3]
</center></span></span>

<span data-ttu-id="0cd03-194">Stejně jako vyrovnávání prahové hodnoty aktivity prahové hodnoty jsou definované za metriku prostřednictvím definice hello clusteru:</span><span class="sxs-lookup"><span data-stu-id="0cd03-194">Just like Balancing Thresholds, Activity Thresholds are defined per-metric via hello cluster definition:</span></span>

<span data-ttu-id="0cd03-195">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="0cd03-195">ClusterManifest.xml</span></span>

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

<span data-ttu-id="0cd03-196">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="0cd03-196">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0cd03-197">Prahové hodnoty vyrovnávání a aktivity jsou oba konkrétní metrika vázanou tooa - vyrovnávání se spustí pouze v případě, že oba hello vyrovnávání prahovou hodnotu a aktivity je prahová hodnota pro hello stejnou metriku.</span><span class="sxs-lookup"><span data-stu-id="0cd03-197">Balancing and activity thresholds are both tied tooa specific metric - balancing is triggered only if both hello Balancing Threshold and Activity Threshold is exceeded for hello same metric.</span></span>

## <a name="balancing-services-together"></a><span data-ttu-id="0cd03-198">Společně vyrovnávání služby</span><span class="sxs-lookup"><span data-stu-id="0cd03-198">Balancing services together</span></span>
<span data-ttu-id="0cd03-199">Zda je hello cluster imbalanced nebo ne je platné pro celý cluster rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="0cd03-199">Whether hello cluster is imbalanced or not is a cluster-wide decision.</span></span> <span data-ttu-id="0cd03-200">Způsob hello jsme přejděte o její opravu je však přesunutí repliky jednotlivé služby a instance kolem.</span><span class="sxs-lookup"><span data-stu-id="0cd03-200">However, hello way we go about fixing it is moving individual service replicas and instances around.</span></span> <span data-ttu-id="0cd03-201">To dává smysl, ne?</span><span class="sxs-lookup"><span data-stu-id="0cd03-201">This makes sense, right?</span></span> <span data-ttu-id="0cd03-202">Pokud paměti je skládaný na jednom uzlu, více replik nebo instancí může být jednou z příčin tooit.</span><span class="sxs-lookup"><span data-stu-id="0cd03-202">If memory is stacked up on one node, multiple replicas or instances could be contributing tooit.</span></span> <span data-ttu-id="0cd03-203">Uchycení nevyváženosti hello může vyžadovat přesunutí hello stavová repliky nebo bezstavové instancí, které použít imbalanced metrika hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-203">Fixing hello imbalance could require moving any of hello stateful replicas or stateless instances that use hello imbalanced metric.</span></span>

<span data-ttu-id="0cd03-204">Občas, když služba, která nebyla samotné imbalanced získá přesunout (Nezapomeňte hello diskuzi o místní a globální provede dříve).</span><span class="sxs-lookup"><span data-stu-id="0cd03-204">Occasionally though, a service that wasn’t itself imbalanced gets moved (remember hello discussion of local and global weights earlier).</span></span> <span data-ttu-id="0cd03-205">Proč by služby získat přesunuta, když všechno, co byly vyrovnáváním metriky služby?</span><span class="sxs-lookup"><span data-stu-id="0cd03-205">Why would a service get moved when all that service’s metrics were balanced?</span></span> <span data-ttu-id="0cd03-206">Podíváme se na příklad:</span><span class="sxs-lookup"><span data-stu-id="0cd03-206">Let’s see an example:</span></span>

- <span data-ttu-id="0cd03-207">Řekněme, že existují čtyři služby, Service1, Service2, Service3 a Service4.</span><span class="sxs-lookup"><span data-stu-id="0cd03-207">Let's say there are four services, Service1, Service2, Service3, and Service4.</span></span> 
- <span data-ttu-id="0cd03-208">Service1 sestavy metrik Metric1 a Metric2.</span><span class="sxs-lookup"><span data-stu-id="0cd03-208">Service1 reports metrics Metric1 and Metric2.</span></span> 
- <span data-ttu-id="0cd03-209">Service2 sestavy metrik Metric2 a Metric3.</span><span class="sxs-lookup"><span data-stu-id="0cd03-209">Service2 reports metrics Metric2 and Metric3.</span></span> 
- <span data-ttu-id="0cd03-210">Service3 sestavy metrik Metric3 a Metric4.</span><span class="sxs-lookup"><span data-stu-id="0cd03-210">Service3 reports metrics Metric3 and Metric4.</span></span>
- <span data-ttu-id="0cd03-211">Service4 hlásí metriky Metric99.</span><span class="sxs-lookup"><span data-stu-id="0cd03-211">Service4 reports metric Metric99.</span></span> 

<span data-ttu-id="0cd03-212">Surely se zobrazí, kde vytvoříme zde: existuje řetězec!</span><span class="sxs-lookup"><span data-stu-id="0cd03-212">Surely you can see where we’re going here: There's a chain!</span></span> <span data-ttu-id="0cd03-213">Nemáme skutečně čtyři nezávislé služby, máme tři služby, které se vztahují a ten, který je vypnutý o sobě.</span><span class="sxs-lookup"><span data-stu-id="0cd03-213">We don’t really have four independent services, we have three services that are related and one that is off on its own.</span></span>

<span data-ttu-id="0cd03-214"><center>
![Společně vyrovnávání služby][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="0cd03-214"><center>
![Balancing Services Together][Image4]
</center></span></span>

<span data-ttu-id="0cd03-215">Z důvodu tento řetězec je možné, že byl zjištěn rozdíl v metriky 1 – 4 může způsobit repliky nebo instance, které patří tooservices 1 – 3 toomove kolem.</span><span class="sxs-lookup"><span data-stu-id="0cd03-215">Because of this chain, it's possible that an imbalance in metrics 1-4 can cause replicas or instances belonging tooservices 1-3 toomove around.</span></span> <span data-ttu-id="0cd03-216">Taky víme, že byl zjištěn rozdíl v metriky 1, 2 nebo 3 nelze způsobit pohybů v Service4.</span><span class="sxs-lookup"><span data-stu-id="0cd03-216">We also know that an imbalance in Metrics 1, 2, or 3 can't cause movements in Service4.</span></span> <span data-ttu-id="0cd03-217">Od přesunutí hello repliky by být žádný bod nebo absolutně nic nestane. instance, které patří tooService4 kolem můžete vyrovnávat hello tooimpact metrik 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="0cd03-217">There would be no point since moving hello replicas or instances belonging tooService4 around can do absolutely nothing tooimpact hello balance of Metrics 1-3.</span></span>

<span data-ttu-id="0cd03-218">Hello správce prostředků clusteru automaticky hodnoty na jaké služby jsou související.</span><span class="sxs-lookup"><span data-stu-id="0cd03-218">hello Cluster Resource Manager automatically figures out what services are related.</span></span> <span data-ttu-id="0cd03-219">Přidání, odebrání nebo změna hello metriky pro služby může ovlivnit jejich vztahů.</span><span class="sxs-lookup"><span data-stu-id="0cd03-219">Adding, removing, or changing hello metrics for services can impact their relationships.</span></span> <span data-ttu-id="0cd03-220">Například mezi dvěma spustí vyrovnávání Service2 pravděpodobně aktualizované tooremove Metric2.</span><span class="sxs-lookup"><span data-stu-id="0cd03-220">For example, between two runs of balancing Service2 may have been updated tooremove Metric2.</span></span> <span data-ttu-id="0cd03-221">Tím se přeruší hello řetězu mezi Service1 a Service2.</span><span class="sxs-lookup"><span data-stu-id="0cd03-221">This breaks hello chain between Service1 and Service2.</span></span> <span data-ttu-id="0cd03-222">Teď místo dvě skupiny související služby, existují tři:</span><span class="sxs-lookup"><span data-stu-id="0cd03-222">Now instead of two groups of related services, there are three:</span></span>

<span data-ttu-id="0cd03-223"><center>
![Společně vyrovnávání služby][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="0cd03-223"><center>
![Balancing Services Together][Image5]
</center></span></span>

## <a name="next-steps"></a><span data-ttu-id="0cd03-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0cd03-224">Next steps</span></span>
* <span data-ttu-id="0cd03-225">Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-225">Metrics are how hello Service Fabric Cluster Resource Manger manages consumption and capacity in hello cluster.</span></span> <span data-ttu-id="0cd03-226">Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="0cd03-226">toolearn more about metrics and how tooconfigure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
* <span data-ttu-id="0cd03-227">Náklady na přesunutí je jeden ze způsobů signalizace toohello správce prostředků clusteru, že jsou určité služby toomove dražší než jiné.</span><span class="sxs-lookup"><span data-stu-id="0cd03-227">Movement Cost is one way of signaling toohello Cluster Resource Manager that certain services are more expensive toomove than others.</span></span> <span data-ttu-id="0cd03-228">Další informace o náklady na přesunutí najdete příliš[v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md)</span><span class="sxs-lookup"><span data-stu-id="0cd03-228">For more about movement cost, refer too[this article](service-fabric-cluster-resource-manager-movement-cost.md)</span></span>
* <span data-ttu-id="0cd03-229">Hello správce prostředků clusteru má několik omezení, můžete nakonfigurovat tooslow dolů změn v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="0cd03-229">hello Cluster Resource Manager has several throttles that you can configure tooslow down churn in hello cluster.</span></span> <span data-ttu-id="0cd03-230">Nejsou běžně potřebné, ale chcete-li se dozvíte je [sem](service-fabric-cluster-resource-manager-advanced-throttling.md)</span><span class="sxs-lookup"><span data-stu-id="0cd03-230">They're not normally necessary, but if you need them you can learn about them [here](service-fabric-cluster-resource-manager-advanced-throttling.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
