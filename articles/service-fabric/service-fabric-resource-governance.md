---
title: "aaaAzure řízení prostředků infrastruktury služby pro kontejnery a služby | Microsoft Docs"
description: "Azure Service Fabric umožňuje toospecify limitů prostředků pro služby spuštěné uvnitř nebo mimo kontejnery."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 34e368211d98ff6b5b294c9c8b3af5ca30eeb20c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-governance"></a><span data-ttu-id="5434b-103">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="5434b-103">Resource governance</span></span> 

<span data-ttu-id="5434b-104">Při spuštění více hello stejného uzlu nebo clusteru, je možné, že jedna služba může spotřebují více prostředků starving dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="5434b-104">When running multiple services on hello same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="5434b-105">Tento problém se označují tooas hello aktivní sousedním problém.</span><span class="sxs-lookup"><span data-stu-id="5434b-105">This problem is referred tooas hello noisy-neighbor problem.</span></span> <span data-ttu-id="5434b-106">Service Fabric umožňuje hello vývojáře toospecify rezervace a omezení na prostředky tooguarantee služby a také omezit jeho využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="5434b-106">Service Fabric allows hello developer toospecify reservations and limits per service tooguarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="5434b-107">Metrika prostředků zásad správného řízení</span><span class="sxs-lookup"><span data-stu-id="5434b-107">Resource governance metrics</span></span> 

<span data-ttu-id="5434b-108">Zásady správného řízení prostředků je podporována v Service Fabric na [balíček služby](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="5434b-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="5434b-109">Hello prostředky, které jsou přiřazeny tooService balíček lze dále rozdělit mezi balíčky kódu.</span><span class="sxs-lookup"><span data-stu-id="5434b-109">hello resources that are assigned tooService Package can be further divided between code packages.</span></span> <span data-ttu-id="5434b-110">zadané limity Hello prostředků přístup také znamenat hello rezervace hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="5434b-110">hello resource limits specified also mean hello reservation of hello resources.</span></span> <span data-ttu-id="5434b-111">Service Fabric podporuje určení procesoru a paměti na balíček služby, pomocí dvě předdefinované [metriky](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="5434b-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="5434b-112">Procesor (název metriky `ServiceFabric:/_CpuCores`): jádro je logické jádra, která je dostupná na hello hostitelský počítač a všechny jader pro všechny uzly jsou vyváženy hello stejné.</span><span class="sxs-lookup"><span data-stu-id="5434b-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on hello host machine, and all cores across all nodes are weighted hello same.</span></span>
* <span data-ttu-id="5434b-113">Paměť (název metriky `ServiceFabric:/_MemoryInMB`): paměti je vyjádřeno v megabajtech a mapuje toophysical paměti, který je dostupný na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="5434b-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps toophysical memory that is available on hello machine.</span></span>

<span data-ttu-id="5434b-114">Záruky vyhrazené logicky pouze jsou k dispozici - hello runtime odmítne otevření nové služby, které jsou překročil dostupné prostředky balíčky.</span><span class="sxs-lookup"><span data-stu-id="5434b-114">Only soft reservation guarantees are provided - hello runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="5434b-115">Ale pokud jiný spustitelný soubor nebo kontejneru je umístěn na uzlu hello, který může porušení záruky vyhrazené původní hello.</span><span class="sxs-lookup"><span data-stu-id="5434b-115">However, if another executable or container is placed on hello node, that may violate hello original reservation guarantees.</span></span>

<span data-ttu-id="5434b-116">Tyto dvě metriky hello [správce prostředků clusteru](service-fabric-cluster-resource-manager-cluster-description.md) sleduje celkové clusteru kapacitu, hello zatížení na každém uzlu v clusteru hello a zbývající prostředků v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5434b-116">For these two metrics, hello [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, hello load on each node in hello cluster, and, remaining resources in hello cluster.</span></span> <span data-ttu-id="5434b-117">Tyto dvě metriky jsou ekvivalentní tooany jiný uživatel nebo vlastní metriky a všechny existující funkce lze použít s nimi:</span><span class="sxs-lookup"><span data-stu-id="5434b-117">These two metrics are equivalent tooany other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="5434b-118">Cluster může být [vyrovnáváním](service-fabric-cluster-resource-manager-balancing.md) podle metriky toothese dva (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="5434b-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according toothese two metrics (default behavior).</span></span>
* <span data-ttu-id="5434b-119">Cluster může být [defragmentovat](service-fabric-cluster-resource-manager-defragmentation-metrics.md) podle toothese dva metriky.</span><span class="sxs-lookup"><span data-stu-id="5434b-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according toothese two metrics.</span></span>
* <span data-ttu-id="5434b-120">Když [popisující cluster](service-fabric-cluster-resource-manager-cluster-description.md), ve vyrovnávací paměti kapacity lze nastavit pro tyto dvě metriky.</span><span class="sxs-lookup"><span data-stu-id="5434b-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="5434b-121">[Dynamické načtení reporting](service-fabric-cluster-resource-manager-metrics.md) není podporována pro tyto metriky a načte pro tyto metriky, které jsou definovány v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="5434b-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="5434b-122">Cluster sady pro povolení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="5434b-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="5434b-123">Kapacita musí být definovány ručně v každém typu uzlu v clusteru hello takto:</span><span class="sxs-lookup"><span data-stu-id="5434b-123">Capacity should be defined manually in each node type in hello cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="5434b-124">Zásady správného řízení prostředků je povolen pouze na uživatele služby, nikoli na všechny systémové služby.</span><span class="sxs-lookup"><span data-stu-id="5434b-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="5434b-125">Při zadávání kapacitu, některé jader a paměti musí být vlevo volné pro systémových služeb.</span><span class="sxs-lookup"><span data-stu-id="5434b-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="5434b-126">Pro optimální výkon hello následující nastavení by také být zapnut v manifestu clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="5434b-126">For optimal performance, hello following setting should also be turned on in hello cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="5434b-127">Zadání zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="5434b-127">Specifying resource governance</span></span> 

<span data-ttu-id="5434b-128">Omezení zásad správného řízení prostředků jsou určené v manifestu aplikace hello (ServiceManifestImport část), jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="5434b-128">Resource governance limits are specified in hello application manifest (ServiceManifestImport section) as shown in hello following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has hello number of CPU cores defined, but doesn't have hello MemoryInMB defined.
  In this case, Service Fabric will sum hello limits on code packages and uses hello sum as 
  hello overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
<span data-ttu-id="5434b-129">V tomto příkladu získá balíček služby ServicePackageA hello uzly, kde je umístěn jednoho jádra.</span><span class="sxs-lookup"><span data-stu-id="5434b-129">In this example, service package ServicePackageA gets one core on hello nodes where it is placed.</span></span> <span data-ttu-id="5434b-130">Tento balíček služby obsahuje dva balíčky kódu (CodeA1 a CodeA2) a obě zadejte hello `CpuShares` parametr.</span><span class="sxs-lookup"><span data-stu-id="5434b-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify hello `CpuShares` parameter.</span></span> <span data-ttu-id="5434b-131">Hello podíl CpuShares 512:256 vydělí hello základní napříč hello dva balíčky pro kód.</span><span class="sxs-lookup"><span data-stu-id="5434b-131">hello proportion of CpuShares 512:256  divides hello core across hello two code packages.</span></span> <span data-ttu-id="5434b-132">Proto v tomto příkladu CodeA1 získá 2 / 3 jádro, CodeA2 získá jednu třetinu jádro (a konfigurace soft záruku rezervace hello stejné).</span><span class="sxs-lookup"><span data-stu-id="5434b-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="5434b-133">V případě, že když CpuShares nejsou zadané pro balíčky kódu, Service Fabric vydělí hello jader rovnoměrně mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="5434b-133">In case when CpuShares are not specified for code packages, Service Fabric divides hello cores equally among them.</span></span>

<span data-ttu-id="5434b-134">Omezení paměti jsou absolutní, takže oba balíčky kódu jsou omezené too1024 MB paměti (a konfigurace soft záruku rezervace z stejné hello).</span><span class="sxs-lookup"><span data-stu-id="5434b-134">Memory limits are absolute, so both code packages are limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="5434b-135">Kód balíčky (kontejnerů a procesy) nejsou možné tooallocate, které větší spotřebu paměti než tento limit a pokus toodo tak vede k výjimce z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="5434b-135">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="5434b-136">Pro toowork vynucení omezení prostředků musí mít všechny balíčky kódu v rámci balíčku služby zadaná paměťová omezení.</span><span class="sxs-lookup"><span data-stu-id="5434b-136">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5434b-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5434b-137">Next steps</span></span>
* <span data-ttu-id="5434b-138">toolearn více o clusteru Resource Manager, přečtěte si to [článku](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5434b-138">toolearn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="5434b-139">toolearn Další informace o modelu aplikace, balíčky služeb, kód balíčky a jak repliky mapování toothem načíst tato [článku](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="5434b-139">toolearn more about application model, service packages, code packages and how replicas map toothem read this [article](service-fabric-application-model.md).</span></span>
