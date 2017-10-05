---
title: "Azure Service Fabric prostředků řízení pro kontejnery a služby | Microsoft Docs"
description: "Azure Service Fabric můžete zadat omezení prostředků pro služby spuštěné uvnitř nebo mimo kontejnery."
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
ms.openlocfilehash: 88d44953ad83f9e7401fd087a39842e4a3790124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="resource-governance"></a><span data-ttu-id="1757d-103">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="1757d-103">Resource governance</span></span> 

<span data-ttu-id="1757d-104">Při spuštění více služeb na stejném uzlu nebo clusteru, je možné, že jedna služba může spotřebují více prostředků starving dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="1757d-104">When running multiple services on the same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="1757d-105">Tento problém se označuje jako aktivní sousedním problém.</span><span class="sxs-lookup"><span data-stu-id="1757d-105">This problem is referred to as the noisy-neighbor problem.</span></span> <span data-ttu-id="1757d-106">Service Fabric umožňuje vývojáři zadejte rezervace a omezení pro službu zajistit prostředků a také omezit jeho využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="1757d-106">Service Fabric allows the developer to specify reservations and limits per service to guarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="1757d-107">Metrika prostředků zásad správného řízení</span><span class="sxs-lookup"><span data-stu-id="1757d-107">Resource governance metrics</span></span> 

<span data-ttu-id="1757d-108">Zásady správného řízení prostředků je podporována v Service Fabric na [balíček služby](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="1757d-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="1757d-109">Prostředky, které jsou přiřazeny k balíčku služby lze dále rozdělit mezi balíčky kódu.</span><span class="sxs-lookup"><span data-stu-id="1757d-109">The resources that are assigned to Service Package can be further divided between code packages.</span></span> <span data-ttu-id="1757d-110">Zadané limity prostředku přístup také znamenat rezervace prostředků.</span><span class="sxs-lookup"><span data-stu-id="1757d-110">The resource limits specified also mean the reservation of the resources.</span></span> <span data-ttu-id="1757d-111">Service Fabric podporuje určení procesoru a paměti na balíček služby, pomocí dvě předdefinované [metriky](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="1757d-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="1757d-112">Procesor (název metriky `ServiceFabric:/_CpuCores`): jádro je logická jádra, která je k dispozici na hostitelském počítači, a všechny jader pro všechny uzly jsou vyváženy stejné.</span><span class="sxs-lookup"><span data-stu-id="1757d-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on the host machine, and all cores across all nodes are weighted the same.</span></span>
* <span data-ttu-id="1757d-113">Paměť (název metriky `ServiceFabric:/_MemoryInMB`): paměti je vyjádřeno v megabajtech a se mapuje na fyzické paměti, která je k dispozici na počítači.</span><span class="sxs-lookup"><span data-stu-id="1757d-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps to physical memory that is available on the machine.</span></span>

<span data-ttu-id="1757d-114">Záruky vyhrazené logicky pouze jsou k dispozici – modul runtime odmítne otevření nové balíčky služeb, které jsou překročil dostupné prostředky.</span><span class="sxs-lookup"><span data-stu-id="1757d-114">Only soft reservation guarantees are provided - the runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="1757d-115">Ale pokud jiný spustitelný soubor nebo kontejneru je umístěn na uzlu, který může být v rozporu původní záruky vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="1757d-115">However, if another executable or container is placed on the node, that may violate the original reservation guarantees.</span></span>

<span data-ttu-id="1757d-116">Pro tyto dvě metriky [správce prostředků clusteru](service-fabric-cluster-resource-manager-cluster-description.md) sleduje celkové clusteru kapacitu, zatížení na každém uzlu v clusteru, a zbývající prostředků v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1757d-116">For these two metrics, the [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, the load on each node in the cluster, and, remaining resources in the cluster.</span></span> <span data-ttu-id="1757d-117">Tyto dvě metriky odpovídají na jiné uživatele nebo vlastní metriky a všechny existující funkce lze použít s nimi:</span><span class="sxs-lookup"><span data-stu-id="1757d-117">These two metrics are equivalent to any other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="1757d-118">Cluster může být [vyrovnáváním](service-fabric-cluster-resource-manager-balancing.md) podle tyto dvě metriky (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="1757d-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according to these two metrics (default behavior).</span></span>
* <span data-ttu-id="1757d-119">Cluster může být [defragmentovat](service-fabric-cluster-resource-manager-defragmentation-metrics.md) podle tyto dvě metriky.</span><span class="sxs-lookup"><span data-stu-id="1757d-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according to these two metrics.</span></span>
* <span data-ttu-id="1757d-120">Když [popisující cluster](service-fabric-cluster-resource-manager-cluster-description.md), ve vyrovnávací paměti kapacity lze nastavit pro tyto dvě metriky.</span><span class="sxs-lookup"><span data-stu-id="1757d-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="1757d-121">[Dynamické načtení reporting](service-fabric-cluster-resource-manager-metrics.md) není podporována pro tyto metriky a načte pro tyto metriky, které jsou definovány v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1757d-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="1757d-122">Cluster sady pro povolení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="1757d-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="1757d-123">Kapacita musí být definovány ručně v každém typu uzlu v clusteru takto:</span><span class="sxs-lookup"><span data-stu-id="1757d-123">Capacity should be defined manually in each node type in the cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="1757d-124">Zásady správného řízení prostředků je povolen pouze na uživatele služby, nikoli na všechny systémové služby.</span><span class="sxs-lookup"><span data-stu-id="1757d-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="1757d-125">Při zadávání kapacitu, některé jader a paměti musí být vlevo volné pro systémových služeb.</span><span class="sxs-lookup"><span data-stu-id="1757d-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="1757d-126">Pro optimální výkon toto nastavení by také být zapnut v manifestu clusteru:</span><span class="sxs-lookup"><span data-stu-id="1757d-126">For optimal performance, the following setting should also be turned on in the cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="1757d-127">Zadání zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="1757d-127">Specifying resource governance</span></span> 

<span data-ttu-id="1757d-128">Omezení zásad správného řízení prostředků jsou určené v manifest aplikace (ServiceManifestImport oddílu), jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1757d-128">Resource governance limits are specified in the application manifest (ServiceManifestImport section) as shown in the following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
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
  
<span data-ttu-id="1757d-129">V tomto příkladu balíček služby ServicePackageA získá jednoho jádra na uzlech, kde je umístěn.</span><span class="sxs-lookup"><span data-stu-id="1757d-129">In this example, service package ServicePackageA gets one core on the nodes where it is placed.</span></span> <span data-ttu-id="1757d-130">Tento balíček služby obsahuje dva balíčky kódu (CodeA1 a CodeA2) a obě zadejte `CpuShares` parametr.</span><span class="sxs-lookup"><span data-stu-id="1757d-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify the `CpuShares` parameter.</span></span> <span data-ttu-id="1757d-131">Poměr CpuShares 512:256 vydělí základní mezi dvěma kód balíčky.</span><span class="sxs-lookup"><span data-stu-id="1757d-131">The proportion of CpuShares 512:256  divides the core across the two code packages.</span></span> <span data-ttu-id="1757d-132">Proto v tomto příkladu CodeA1 získá 2 / 3 jádro a CodeA2 získá jednu třetinu jádro (a rezervace konfigurace soft záruku stejného).</span><span class="sxs-lookup"><span data-stu-id="1757d-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="1757d-133">V případě, že pokud nejsou zadané CpuShares pro balíčky kódu, Service Fabric vydělí jader rovnoměrně mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="1757d-133">In case when CpuShares are not specified for code packages, Service Fabric divides the cores equally among them.</span></span>

<span data-ttu-id="1757d-134">Omezení paměti jsou absolutní, takže oba balíčky kódu jsou omezeny na 1024 MB paměti (a rezervace konfigurace soft záruku stejného).</span><span class="sxs-lookup"><span data-stu-id="1757d-134">Memory limits are absolute, so both code packages are limited to 1024 MB of memory (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="1757d-135">Balíčky kódu (kontejnery a procesy) nejsou schopné přidělit víc paměti, než je toto omezení, a případný pokus o takové přidělení má za následek výjimku z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="1757d-135">Code packages (containers or processes) are not able to allocate more memory than this limit, and attempting to do so results in an out-of-memory exception.</span></span> <span data-ttu-id="1757d-136">Aby vynucení omezení prostředků fungovala, musí být omezení paměti zadaná pro všechny balíčky kódu v rámci balíčku služby.</span><span class="sxs-lookup"><span data-stu-id="1757d-136">For resource limit enforcement to work, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1757d-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1757d-137">Next steps</span></span>
* <span data-ttu-id="1757d-138">Další informace o službě Správce prostředků clusteru, přečtěte si to [článku](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1757d-138">To learn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="1757d-139">Další informace o modelu aplikace, balíčky služeb, balíčky kódu a jak mapování repliky na ně načíst tato [článku](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="1757d-139">To learn more about application model, service packages, code packages and how replicas map to them read this [article](service-fabric-application-model.md).</span></span>
