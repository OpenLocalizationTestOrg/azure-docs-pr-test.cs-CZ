---
title: "Správce prostředků clusteru Service Fabric: pohyb náklady | Microsoft Docs"
description: "Přehled náklady na přesunutí pro služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 65d4ac73efffcf7b25b1e95da6f9012a9238cb75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="5f1b1-103">Náklady na přesunutí služby</span><span class="sxs-lookup"><span data-stu-id="5f1b1-103">Service movement cost</span></span>
<span data-ttu-id="5f1b1-104">Faktor, který hello správce prostředků clusteru Service Fabric zvažuje při pokusu o toodetermine, jaké změny toomake tooa clusteru je hello náklady na tyto změny.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-104">A factor that hello Service Fabric Cluster Resource Manager considers when trying toodetermine what changes toomake tooa cluster is hello cost of those changes.</span></span> <span data-ttu-id="5f1b1-105">Hello představu o "náklady" se prodávají vypnout proti kolik hello clusteru je možné zlepšit.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-105">hello notion of "cost" is traded off against how much hello cluster can be improved.</span></span> <span data-ttu-id="5f1b1-106">Náklady se započítá při přesunu služby pro vyrovnání, Defragmentace a další požadavky.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="5f1b1-107">cílem Hello je toomeet hello požadavky v hello minimálně způsob rušivý nebo nákladné.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-107">hello goal is toomeet hello requirements in hello least disruptive or expensive way.</span></span> 

<span data-ttu-id="5f1b1-108">Přesunutí čas procesoru náklady na služby a šířka pásma sítě na minimální.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="5f1b1-109">Pro stavové služby vyžaduje kopírování hello stav těchto služeb, využívání další paměti a disku.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-109">For stateful services, it requires copying hello state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="5f1b1-110">Minimalizovat náklady hello řešení této hello správce prostředků Azure Service Fabric clusteru se dodává s pomáhá zajistit, že nejsou prostředky clusteru hello stráví zbytečně.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-110">Minimizing hello cost of solutions that hello Azure Service Fabric Cluster Resource Manager comes up with helps ensure that hello cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="5f1b1-111">Však také nechcete, aby tooignore řešení, které by výrazně zlepšit hello přidělení prostředků v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-111">However, you also don’t want tooignore solutions that would significantly improve hello allocation of resources in hello cluster.</span></span>

<span data-ttu-id="5f1b1-112">Hello správce prostředků clusteru má dva způsoby, kterými computing náklady a omezení je, že při pokusu o toomanage hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-112">hello Cluster Resource Manager has two ways of computing costs and limiting them while it tries toomanage hello cluster.</span></span> <span data-ttu-id="5f1b1-113">První mechanismus Hello je jednoduše počítání každých přesunu, která by byla.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-113">hello first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="5f1b1-114">Pokud dvě řešení se generují se o hello stejné vyrovnávat (skóre), pak hello správce prostředků clusteru upřednostní hello, jeden s hello nejnižší náklady (celkový počet přesune).</span><span class="sxs-lookup"><span data-stu-id="5f1b1-114">If two solutions are generated with about hello same balance (score), then hello Cluster Resource Manager prefers hello one with hello lowest cost (total number of moves).</span></span>

<span data-ttu-id="5f1b1-115">Tato strategie funguje správně.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-115">This strategy works well.</span></span> <span data-ttu-id="5f1b1-116">Ale stejně jako u výchozí nebo statické zatížení, není pravděpodobné v jakékoli komplexní systému, zda jsou si rovny přesune všechny.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="5f1b1-117">Některé jsou pravděpodobně toobe mnohem nákladnější.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-117">Some are likely toobe much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="5f1b1-118">Náklady na přesunutí nastavení</span><span class="sxs-lookup"><span data-stu-id="5f1b1-118">Setting Move Costs</span></span> 
<span data-ttu-id="5f1b1-119">Náklady na přesunutí výchozí hello pro službu můžete určit, když je vytvořeno:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-119">You can specify hello default move cost for a service when it is created:</span></span>

<span data-ttu-id="5f1b1-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="5f1b1-121">C#:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="5f1b1-122">Můžete také zadat nebo aktualizovat MoveCost dynamicky pro službu po vytvoření služby hello:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-122">You can also specify or update MoveCost dynamically for a service after hello service has been created:</span></span> 

<span data-ttu-id="5f1b1-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="5f1b1-124">C#:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="5f1b1-125">Dynamicky zadání náklady na přesunutí na základě za repliky</span><span class="sxs-lookup"><span data-stu-id="5f1b1-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="5f1b1-126">Hello předchozí fragmenty kódu jsou všechny pro zadání MoveCost najednou celou službu samotnou službu mimo hello.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-126">hello preceding snippets are all for specifying MoveCost for a whole service at once from outside hello service itself.</span></span> <span data-ttu-id="5f1b1-127">Však přesunout náklady je velmi užitečné je při hello náklady na přesunutí objektu specifické služby změní přes jeho životnost.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-127">However, move cost is most useful is when hello move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="5f1b1-128">Vzhledem k tomu, že pravděpodobně hello sami služeb mít hello nejlepší představu o tom, jak nákladná jsou toomove daném okamžiku, je rozhraní API pro služby tooreport vlastní jednotlivých přesunutí náklady za běhu.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-128">Since hello services themselves probably have hello best idea of how costly they are toomove a given time, there's an API for services tooreport their own individual move cost during runtime.</span></span> 

<span data-ttu-id="5f1b1-129">C#:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="5f1b1-130">Dopad náklady na přesunutí</span><span class="sxs-lookup"><span data-stu-id="5f1b1-130">Impact of move cost</span></span>
<span data-ttu-id="5f1b1-131">MoveCost má čtyři úrovně: nula, nízké, střední a vysokou.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="5f1b1-132">MoveCosts jsou relativní tooeach jiných, s výjimkou nula.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-132">MoveCosts are relative tooeach other, except for Zero.</span></span> <span data-ttu-id="5f1b1-133">Nulové náklady na přesunutí znamená, že pohyb je zdarma a neměli započítává hello skóre hello řešení.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-133">Zero move cost means that movement is free and should not count against hello score of hello solution.</span></span> <span data-ttu-id="5f1b1-134">Nastavení vaší přesunutí náklady tooHigh nemá *není* zaručit repliky hello zůstává na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-134">Setting your move cost tooHigh does *not* guarantee that hello replica stays in one place.</span></span>

<span data-ttu-id="5f1b1-135"><center>
![Náklady na přesunutí jako faktor při výběru replik pro přesun][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="5f1b1-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="5f1b1-136">MoveCost vám pomůže najít řešení hello že příčina celkové hello minimálně přerušení a jsou nejjednodušší tooachieve při stále přicházejících na ekvivalentní vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-136">MoveCost helps you find hello solutions that cause hello least disruption overall and are easiest tooachieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="5f1b1-137">Služby představu o náklady může být relativní toomany věcí.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-137">A service’s notion of cost can be relative toomany things.</span></span> <span data-ttu-id="5f1b1-138">Hello nejběžnější faktory při výpočtu vaše náklady na přesunutí jsou:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-138">hello most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="5f1b1-139">Hello množství dat, zda má služba hello toomove nebo stav.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-139">hello amount of state or data that hello service has toomove.</span></span>
- <span data-ttu-id="5f1b1-140">Hello náklady na odpojení klientů.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-140">hello cost of disconnection of clients.</span></span> <span data-ttu-id="5f1b1-141">Přesunutí primární replice je obvykle dražší než hello náklady na přesunutí sekundární repliku.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-141">Moving a primary replica is usually more costly than hello cost of moving a secondary replica.</span></span>
- <span data-ttu-id="5f1b1-142">Hello náklady na přerušení operace během letu.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-142">hello cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="5f1b1-143">Některé operace v hello data ukládat úroveň nebo jsou nákladná operace provedené v odpovědi tooa klienta volání.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-143">Some operations at hello data store level or operations performed in response tooa client call are costly.</span></span> <span data-ttu-id="5f1b1-144">Po určité míry, které nechcete toostop je pokud nemusíte.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-144">After a certain point, you don’t want toostop them if you don’t have to.</span></span> <span data-ttu-id="5f1b1-145">Proto při operaci hello se děje, můžete zvýšit náklady na přesunutí hello této služby objektu tooreduce hello pravděpodobnost, že ji přesune.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-145">So while hello operation is going on, you increase hello move cost of this service object tooreduce hello likelihood that it moves.</span></span> <span data-ttu-id="5f1b1-146">Pokud se provádí operaci hello, nastavíte back toonormal hello náklady.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-146">When hello operation is done, you set hello cost back toonormal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="5f1b1-147">Povolení náklady na přesunutí v clusteru</span><span class="sxs-lookup"><span data-stu-id="5f1b1-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="5f1b1-148">Aby hello podrobnější MoveCosts toobe vzít v úvahu, MoveCost musí být povolen v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-148">In order for hello more granular MoveCosts toobe taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="5f1b1-149">Bez tohoto nastavení hello výchozí režim počítání přesune se používá pro výpočet MoveCost a MoveCost sestavy jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-149">Without this setting, hello default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="5f1b1-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="5f1b1-151">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="5f1b1-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a><span data-ttu-id="5f1b1-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f1b1-152">Next steps</span></span>
- <span data-ttu-id="5f1b1-153">Správce prostředků clusteru Service Fabric používá v clusteru hello využívání toomanage metriky a kapacity.</span><span class="sxs-lookup"><span data-stu-id="5f1b1-153">Service Fabric Cluster Resource Manger uses metrics toomanage consumption and capacity in hello cluster.</span></span> <span data-ttu-id="5f1b1-154">Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [Správa spotřeby prostředků a zatížení v Service Fabric s metriky](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5f1b1-154">toolearn more about metrics and how tooconfigure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="5f1b1-155">toolearn o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na [vyrovnávání cluster Service Fabric](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="5f1b1-155">toolearn about how hello Cluster Resource Manager manages and balances load in hello cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
