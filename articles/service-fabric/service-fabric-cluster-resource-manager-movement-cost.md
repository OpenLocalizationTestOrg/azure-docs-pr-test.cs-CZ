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
ms.openlocfilehash: 5de07c259d1d327d0211338c2911804445dd6b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="adc00-103">Náklady na přesunutí služby</span><span class="sxs-lookup"><span data-stu-id="adc00-103">Service movement cost</span></span>
<span data-ttu-id="adc00-104">Faktor, který správce prostředků clusteru Service Fabric zvažuje při pokusu určit, jaké změny, aby do clusteru s podporou jsou náklady na tyto změny.</span><span class="sxs-lookup"><span data-stu-id="adc00-104">A factor that the Service Fabric Cluster Resource Manager considers when trying to determine what changes to make to a cluster is the cost of those changes.</span></span> <span data-ttu-id="adc00-105">Pojem "náklady" se prodávají vypnout proti kolik clusteru je možné zlepšit.</span><span class="sxs-lookup"><span data-stu-id="adc00-105">The notion of "cost" is traded off against how much the cluster can be improved.</span></span> <span data-ttu-id="adc00-106">Náklady se započítá při přesunu služby pro vyrovnání, Defragmentace a další požadavky.</span><span class="sxs-lookup"><span data-stu-id="adc00-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="adc00-107">Cílem je pro splnění požadavků způsobem alespoň rušivý nebo nákladné.</span><span class="sxs-lookup"><span data-stu-id="adc00-107">The goal is to meet the requirements in the least disruptive or expensive way.</span></span> 

<span data-ttu-id="adc00-108">Přesunutí čas procesoru náklady na služby a šířka pásma sítě na minimální.</span><span class="sxs-lookup"><span data-stu-id="adc00-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="adc00-109">Pro stavové služby vyžaduje kopírování stav těchto služeb, využívání další paměti a disku.</span><span class="sxs-lookup"><span data-stu-id="adc00-109">For stateful services, it requires copying the state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="adc00-110">Minimalizovat náklady na řešení, které správce prostředků Azure Service Fabric clusteru se dodává s pomáhá zajistit, že nejsou zbytečně stráví prostředky clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc00-110">Minimizing the cost of solutions that the Azure Service Fabric Cluster Resource Manager comes up with helps ensure that the cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="adc00-111">Však také nechcete ignorovat řešení, které by výrazně zlepšit přidělení prostředků v clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc00-111">However, you also don’t want to ignore solutions that would significantly improve the allocation of resources in the cluster.</span></span>

<span data-ttu-id="adc00-112">Správce prostředků clusteru má dva způsoby, kterými computing náklady a omezení je při pokusu o správě clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc00-112">The Cluster Resource Manager has two ways of computing costs and limiting them while it tries to manage the cluster.</span></span> <span data-ttu-id="adc00-113">První mechanismus je jednoduše počítání každých přesunu, která by byla.</span><span class="sxs-lookup"><span data-stu-id="adc00-113">The first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="adc00-114">Pokud dvě řešení se generují se o stejné vyvážit (skóre), pak upřednostní správce prostředků clusteru, jeden s nejnižší náklady na (celkový počet přesune).</span><span class="sxs-lookup"><span data-stu-id="adc00-114">If two solutions are generated with about the same balance (score), then the Cluster Resource Manager prefers the one with the lowest cost (total number of moves).</span></span>

<span data-ttu-id="adc00-115">Tato strategie funguje správně.</span><span class="sxs-lookup"><span data-stu-id="adc00-115">This strategy works well.</span></span> <span data-ttu-id="adc00-116">Ale stejně jako u výchozí nebo statické zatížení, není pravděpodobné v jakékoli komplexní systému, zda jsou si rovny přesune všechny.</span><span class="sxs-lookup"><span data-stu-id="adc00-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="adc00-117">Některé by mohly být mnohem nákladnější.</span><span class="sxs-lookup"><span data-stu-id="adc00-117">Some are likely to be much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="adc00-118">Náklady na přesunutí nastavení</span><span class="sxs-lookup"><span data-stu-id="adc00-118">Setting Move Costs</span></span> 
<span data-ttu-id="adc00-119">Můžete určit výchozí přesunout náklady pro službu, když je vytvořeno:</span><span class="sxs-lookup"><span data-stu-id="adc00-119">You can specify the default move cost for a service when it is created:</span></span>

<span data-ttu-id="adc00-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="adc00-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="adc00-121">C#:</span><span class="sxs-lookup"><span data-stu-id="adc00-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="adc00-122">Můžete také zadat nebo aktualizovat MoveCost dynamicky pro službu po vytvoření služby:</span><span class="sxs-lookup"><span data-stu-id="adc00-122">You can also specify or update MoveCost dynamically for a service after the service has been created:</span></span> 

<span data-ttu-id="adc00-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="adc00-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="adc00-124">C#:</span><span class="sxs-lookup"><span data-stu-id="adc00-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="adc00-125">Dynamicky zadání náklady na přesunutí na základě za repliky</span><span class="sxs-lookup"><span data-stu-id="adc00-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="adc00-126">Předchozí fragmenty kódu jsou všechny pro zadání MoveCost celou službu najednou mimo službu samotnou.</span><span class="sxs-lookup"><span data-stu-id="adc00-126">The preceding snippets are all for specifying MoveCost for a whole service at once from outside the service itself.</span></span> <span data-ttu-id="adc00-127">Však přesunout náklady je velmi užitečné je při náklady na přesunutí objektu specifické služby změní přes jeho životnost.</span><span class="sxs-lookup"><span data-stu-id="adc00-127">However, move cost is most useful is when the move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="adc00-128">Vzhledem k tomu, že sami služeb pravděpodobně mají nejlepší představu o tom, jak nákladná jsou přesunout okamžiku, je rozhraní API služeb sestavu vlastní individuální přesunout náklady za běhu.</span><span class="sxs-lookup"><span data-stu-id="adc00-128">Since the services themselves probably have the best idea of how costly they are to move a given time, there's an API for services to report their own individual move cost during runtime.</span></span> 

<span data-ttu-id="adc00-129">C#:</span><span class="sxs-lookup"><span data-stu-id="adc00-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="adc00-130">Dopad náklady na přesunutí</span><span class="sxs-lookup"><span data-stu-id="adc00-130">Impact of move cost</span></span>
<span data-ttu-id="adc00-131">MoveCost má čtyři úrovně: nula, nízké, střední a vysokou.</span><span class="sxs-lookup"><span data-stu-id="adc00-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="adc00-132">MoveCosts jsou relativní vzhledem k sobě navzájem, s výjimkou nula.</span><span class="sxs-lookup"><span data-stu-id="adc00-132">MoveCosts are relative to each other, except for Zero.</span></span> <span data-ttu-id="adc00-133">Nulové náklady na přesunutí znamená, že pohyb je zdarma a neměli započítává skóre řešení.</span><span class="sxs-lookup"><span data-stu-id="adc00-133">Zero move cost means that movement is free and should not count against the score of the solution.</span></span> <span data-ttu-id="adc00-134">Nastavení vaší přesunutí náklady na horní nemá *není* záruka, že replika zůstane na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="adc00-134">Setting your move cost to High does *not* guarantee that the replica stays in one place.</span></span>

<span data-ttu-id="adc00-135"><center>
![Náklady na přesunutí jako faktor při výběru replik pro přesun][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="adc00-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="adc00-136">MoveCost vám pomůže najít řešení, která způsobí celkové minimálně přerušení a jsou nejjednodušší dosáhnout při stále přicházejících na ekvivalentní vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="adc00-136">MoveCost helps you find the solutions that cause the least disruption overall and are easiest to achieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="adc00-137">Služby představu o náklady může být relativní vzhledem k celou řadu věcí.</span><span class="sxs-lookup"><span data-stu-id="adc00-137">A service’s notion of cost can be relative to many things.</span></span> <span data-ttu-id="adc00-138">Nejběžnější faktory při výpočtu vaše náklady na přesunutí jsou:</span><span class="sxs-lookup"><span data-stu-id="adc00-138">The most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="adc00-139">Množství dat, který má služba pro přesun nebo stav.</span><span class="sxs-lookup"><span data-stu-id="adc00-139">The amount of state or data that the service has to move.</span></span>
- <span data-ttu-id="adc00-140">Náklady na odpojení klientů.</span><span class="sxs-lookup"><span data-stu-id="adc00-140">The cost of disconnection of clients.</span></span> <span data-ttu-id="adc00-141">Přesunutí primární replice je obvykle dražší než náklady na přesunutí sekundární repliku.</span><span class="sxs-lookup"><span data-stu-id="adc00-141">Moving a primary replica is usually more costly than the cost of moving a secondary replica.</span></span>
- <span data-ttu-id="adc00-142">Náklady na přerušení operace během letu.</span><span class="sxs-lookup"><span data-stu-id="adc00-142">The cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="adc00-143">Některé operace na data ukládat úroveň nebo jsou nákladná operace provedené v reakci na hovor klienta.</span><span class="sxs-lookup"><span data-stu-id="adc00-143">Some operations at the data store level or operations performed in response to a client call are costly.</span></span> <span data-ttu-id="adc00-144">Po určité míry nechcete nezastavíte, pokud není nutné.</span><span class="sxs-lookup"><span data-stu-id="adc00-144">After a certain point, you don’t want to stop them if you don’t have to.</span></span> <span data-ttu-id="adc00-145">Proto při operaci se děje, můžete zvýšit náklady na přesunutí tohoto objektu služby snížit pravděpodobnost, že ji přesune.</span><span class="sxs-lookup"><span data-stu-id="adc00-145">So while the operation is going on, you increase the move cost of this service object to reduce the likelihood that it moves.</span></span> <span data-ttu-id="adc00-146">Pokud se provádí operaci, nastavíte náklady na zpět na normální.</span><span class="sxs-lookup"><span data-stu-id="adc00-146">When the operation is done, you set the cost back to normal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="adc00-147">Povolení náklady na přesunutí v clusteru</span><span class="sxs-lookup"><span data-stu-id="adc00-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="adc00-148">Aby podrobnější MoveCosts vzít v úvahu musí být povolena MoveCost v clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc00-148">In order for the more granular MoveCosts to be taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="adc00-149">Bez tohoto nastavení výchozí režim počítání přesune se používá pro výpočet MoveCost a MoveCost sestavy jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="adc00-149">Without this setting, the default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="adc00-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="adc00-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="adc00-151">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="adc00-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="adc00-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="adc00-152">Next steps</span></span>
- <span data-ttu-id="adc00-153">Správce prostředků clusteru Service Fabric používá metriky ke správě využívání a kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc00-153">Service Fabric Cluster Resource Manger uses metrics to manage consumption and capacity in the cluster.</span></span> <span data-ttu-id="adc00-154">Další informace o metriky a způsob jejich konfigurace, podívejte se na [Správa spotřeby prostředků a zatížení v Service Fabric s metriky](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="adc00-154">To learn more about metrics and how to configure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="adc00-155">Další informace o tom, jak správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru, podívejte se na [vyrovnávání cluster Service Fabric](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="adc00-155">To learn about how the Cluster Resource Manager manages and balances load in the cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
